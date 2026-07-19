---
          
title: Freeing and Selfhosting Enterprise Cisco Video Phones + Usage with Axis Doorbell
date: 2026-07-18 22:00:00 UTC
updated: 2026-07-19 22:00:00 UTC
comments: false
categories: homelab
tags: homelab selfhosting de-clouding
---
It was to my dismay that I learned, after buying two Cisco CP-8845's on eBay, Cisco enterprise firmware is locked down to Cisco CUCM/Call manager. I did not realize until after purchase that the much more expensive (on the used market) 3CPP models have local management enabled while my standard ones are locked to Cisco's services. Rather than returning the phones and shelling out more money for these unlocked versions I dug deeper and found out [the phones can have custom XML configuration.](https://usecallmanager.nz/documentation-overview.html) Using a TFTP server and some manually crafted XML files for the phone, we can configure virtually every setting locally (including SIP server).

# The Situation
I wanted to tie two Cisco CP-8845 Video Phones to VOIP.ms (my phone provider) to make standard calls and use a local system to run video calls A8105-E. This would be replacing a PolyCom VVX phone to allow for video calls to the doorbell.

### Largest Issues
* The Cisco phone is not super easily configurable to custom servers
* VOIP.ms does not support video calls
* When loading XML files the phone does not report errors, and must be rebooted each time
* The Cisco phone really only supports one SIP connection

# The setup
With collaboration with my good friend Gemini, I built out a docker stack to support my local phone connection and a bridge to my VOIP provider.

Before going to the phones, the SIP server and TFTP server must be set up and ready for the phones to connect. You can use thee Docker stack and configruation I provided below for a full setup. 

Your TFTP can be the Docker version I provided below or use a testing service such as Tftpd64 on Windows.
Both of the Cicso phones were [reset](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cuipph/8800-series/english/adminguide/P881_BK_C136782F_00_cisco-ip-phone-8800_series/P881_BK_C136782F_00_cisco-ip-phone-8811-8841_chapter_01111.html#P881_TK_P1458B67_00) and had TFTP server configured (ignore the QR scanning portion) under the settings key -> Admin Settings -> Network setup -> Ethernet setup -> IPv4 setup. Here you can set the primary TFTP server (and net settings).  Once they connect they will try to read their SEP\<MAC\>.cnf.xml file and provision themselves.

```
version: '3.8'

services:
  asterisk:
    image: andrius/asterisk:22
    container_name: asterisk
    restart: always
    network_mode: host
    volumes:
      - /APPCONFIGLOCATION/telephony/asterisk/pjsip.conf:/etc/asterisk/pjsip.conf:ro
      - /APPCONFIGLOCATION/telephony/asterisk/extensions.conf:/etc/asterisk/extensions.conf:ro
	  
  cisco-tftp: #TFTP server for loading config files to phones
    image: pghalliday/tftp:latest
    container_name: cisco-tftp

    restart: unless-stopped
    stdin_open: true
    tty: true
    ports:
      - "69:69/udp"
    command: -Lvvv -s /var/tftpboot
    volumes:
      - /dev/log:/dev/log
      - /APPCONFIGLOCATION/telephony/tftp:/var/tftpboot
```

### The biggest part of Asterisk config in this case, the PJSIP.conf.

```
[global]
user_agent=Asterisk


; Transport Configuration
[transport-udp]
type=transport
protocol=udp
bind=0.0.0.0:5060
; IMPORTANT FOR DOCKER: If you experience one-way audio, 
; uncomment and configure your local subnet and public IP:
; local_net=192.168.1.0/24 
; external_media_address=YOUR.PUBLIC.IP
; external_signaling_address=YOUR.PUBLIC.IP

; VoIP.ms Trunk Configuration
[voipms]
type=registration
transport=transport-udp
outbound_auth=voipms_auth
client_uri=sip:myvoipuser@mylocation.voip.ms:5060
server_uri=sip:mylocation.voip.ms:5060
contact_user=myvoipuser
retry_interval=60
expiration=120

[voipms_auth]
type=auth
auth_type=userpass
password=myvoippassword
username=myvoipuser

[voipms]
type=aor
contact=sip:mylocation.voip.ms:5060
qualify_frequency=30

[voipms]
type=endpoint
transport=transport-udp
context=from-voipms
disallow=all
allow=ulaw
outbound_auth=voipms_auth
aors=voipms
from_user=myvoipuser
from_domain=mylocation.voip.ms
direct_media=no
rtp_symmetric = yes
rewrite_contact = yes
send_rpid = yes

[voipms_identify]
type=identify
endpoint=voipms
match=mylocation.voip.ms

; Cisco Video Phone Extension Configuration
[401]
type=endpoint
context=from-internal
disallow=all
allow=ulaw
allow=h264
;callerid=Desk Phone <401>
direct_media=no          ; Anchors the video stream to Asterisk
rtp_symmetric=yes        ; Stabilizes local packet routing
rewrite_contact=yes      ; Flushes ephemeral client port mismatches
force_rport=yes          ; Pierces dynamic network states
auth=401_auth
aors=401

[401_auth]
type=auth
auth_type=userpass
password=CiscoPassword
username=401

[401]
type=aor
;remove_existing=yes
;qualify_frequency=30
max_contacts=1

; Cisco Secondary Phone Extension Configuration
[402]
type=endpoint
context=from-internal
disallow=all
allow=ulaw
allow=h264
;callerid=Cisco Phone <402>
direct_media=no
rtp_symmetric=yes
rewrite_contact=yes
force_rport=yes
auth=402_auth
aors=402

[402_auth]
type=auth
auth_type=userpass
password=CiscoPassword
username=402

[402]
type=aor
;remove_existing=yes
;qualify_frequency=30
max_contacts=1

; AXIS DOORBELL INTERCOM
[axisdoorbell]
type=auth
auth_type=userpass
username=axisdoorbell
password=AxisPassword

[axisdoorbell]
type=aor
max_contacts=1
remove_existing=yes

[axisdoorbell]
type=endpoint
transport=transport-udp
context=from-internal
disallow=all
allow=ulaw
allow=h264
auth=axisdoorbell
aors=axisdoorbell
direct_media=no
rtp_symmetric=yes
```


### Routing and Dial Plans: extensions.conf

```
[general]
static=yes
writeprotect=no

; Inbound Routing (From VoIP.ms)
[from-voipms]
exten => _X.,1,NoOp(Incoming call from VoIP.ms)
 same => n,Dial(PJSIP/401,30)
 same => n,Hangup()

; Outbound Routing (From Internal Devices)
[from-internal]

; 1a. AXIS Doorbell button dials 401 -> Rings Cisco Phone
exten => 401,1,NoOp(AXIS Doorbell button pressed!)
 same => n,Set(CALLERID(num)=401)         
 same => n,Set(CALLERID(name)=Doorbell)   
 same => n,Dial(PJSIP/401,30)
 same => n,Hangup()

; 1b. Dialing 400 -> Rings Doorbell Intercom
exten => 400,1,NoOp(Calling out to AXIS Intercom stream)
 same => n,Dial(PJSIP/axisdoorbell,30)
 same => n,Hangup()


; --- 2. DYNAMIC INTERNAL 400-RANGE ROUTING ---
; Skips some numbers ending in 0-1 just a note when things don't work like 411 or 412 or 421 
exten => _4[0-9][2-9],1,NoOp(Internal local call to extension: ${EXTEN})
same => n,Dial(PJSIP/${EXTEN},30,HhBb(webrtc_profile^add_video^1))
same => n,Hangup()

; --- 3. EXPLICIT PUBLIC NUMBER PATTERNS ---
; Handle 11-digit outbound calls (e.g., 1-800-555-1234)
exten => _1NXXNXXXXXX,1,NoOp(Outbound call to ${EXTEN})
 same => n,Dial(PJSIP/${EXTEN}@voipms,60,r)
 same => n,Hangup()

; Handle 10-digit outbound calls and automatically prepend the 1 for VoIP.ms
exten => _NXXNXXXXXX,1,NoOp(Outbound call to ${EXTEN})
 same => n,Dial(PJSIP/1${EXTEN}@voipms,60,r)
 same => n,Hangup()


; --- 4. STAR CODE ROUTING ---
exten => _*X.,1,NoOp(Routing Star Code to VoIP.ms: ${EXTEN})
 same => n,Dial(PJSIP/${EXTEN}@voipms,60,r)
 same => n,Hangup()


; --- 5. GLOBAL CATCH-ALL ROUTING (EXCLUDING LOCAL 4XX) ---
; By updating this pattern from _X. to _[0-35-9]X., we prevent Asterisk from
; accidentally sending local 4XX routing errors out to your trunk.
exten => _[0-35-9]X.,1,NoOp(Catch-all routing out to VoIP.ms for: ${EXTEN})
 same => n,Dial(PJSIP/${EXTEN}@voipms,60,r)
 same => n,Hangup()
```

### Cisco TFTP Configuration: SEP\<macAddress\>.cnf.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<device>
  <fullConfig>true</fullConfig>
  <deviceProtocol>SIP</deviceProtocol>
  
  <vendorConfig>
       <sshAccess>0</sshAccess>
        <backgroundImageURL>TFTP:Desktops/800x480x24/mainbackground.png</backgroundImageURL>
        <serviceProvisioning>2</serviceProvisioning>
        <lineMode>1</lineMode>
  </vendorConfig>
  <directoryURL>http://serverURL/cisco/directory.php</directoryURL>
  <servicesURL></servicesURL> <!-- Leaves default services intact -->

  <!-- <idleTimeout>30</idleTimeout> -->
  <!-- <idleURL>http://serverURL/cisco/idlepage.php</idleURL>  -->

 
  <devicePool>
       <dateTimeSetting>
           <dateTemplate>D/M/YA</dateTemplate>
           <timeZone>-360</timeZone>
        </dateTimeSetting>
    <callManagerGroup>
      <members>
        <member priority="0">
          <callManager>
            <ports>
              <ethernetPhonePort>5060</ethernetPhonePort>
            </ports>
            <processNodeName>asteriskIP</processNodeName>
          </callManager>
        </member>
      </members>
    </callManagerGroup>
  </devicePool>
<phoneServices useHTTPS="false">
    <provisioning>2</provisioning>
    <phoneService type="1" category="0">
      <name>Missed Calls</name>
      <url>Application:Cisco/MissedCalls</url>
      <vendor></vendor>
      <version></version>
    </phoneService>
    <phoneService type="1" category="0">
      <name>Received Calls</name>
      <url>Application:Cisco/ReceivedCalls</url>
      <vendor></vendor>
      <version></version>
    </phoneService>
    <phoneService type="1" category="0">
      <name>Placed Calls</name>
      <url>Application:Cisco/PlacedCalls</url>
      <vendor></vendor>
      <version></version>
    </phoneService>
    <phoneService type="2" category="0">
      <name>Voicemail</name>
      <url>Application:Cisco/Voicemail</url>
      <vendor></vendor>
      <version></version>
    </phoneService>

</phoneServices>
  <sipProfile>
    <sipLines>
      <line button="1" lineIndex="1">
        <featureID>9</featureID>
        <featureLabel>Home phone</featureLabel>
        <proxy>USECALLMANAGER</proxy>
        <port>5060</port>
        <authName>401</authName>
        <authPassword>CiscoPassword</authPassword>
        <name>401</name>
        <voicemail>*97</voicemail>
        <displayName>Desk Phone</displayName>
        <contact>401</contact>
        <messageWaitingLampPolicy>3</messageWaitingLampPolicy>
        <stutterDialTone>1</stutterDialTone>
        <messagesNumber>*97</messagesNumber>
      </line>
    <line button="2">
        <featureID>2</featureID>
        <featureLabel>Speed Dial 1</featureLabel>
        <speedDialNumber>1234567890</speedDialNumber>
    </line>
    <line button="3">
        <featureID>2</featureID>
        <featureLabel>Speed Dial 2</featureLabel>
        <speedDialNumber>1234567890</speedDialNumber>
    </line>
    
    <line button="10">
        <featureID>2</featureID>
        <featureLabel>DOORBELL</featureLabel>
        <speedDialNumber>400</speedDialNumber>
    </line>
    <line button="9">
    <featureID>2</featureID> <!-- 2 = Standard Line Speed Dial Mapping -->
    <featureLabel>Voicemail</featureLabel>
    <speedDialNumber>*97</speedDialNumber>
    </line>


    <line button="8">
      <featureID>20</featureID>
      <featureLabel>Test</featureLabel>
      <serviceURI>http://serverURL/cisco/idlepage.xml</serviceURI>
    </line>
    </sipLines>

    <registerExpires>120</registerExpires>
    <natEnabled>false</natEnabled> 
    <natReceivedProcessing>false</natReceivedProcessing>
    <sipTimerT1>500</sipTimerT1>
    <sipTimerT2>4000</sipTimerT2>
    
    <capLists>
      <capList>
        <payloadType>0</payloadType>
        <codecName>PCMU</codecName>
      </capList>
      <capList>
        <payloadType>99</payloadType>
        <codecName>H264</codecName>
      </capList>
    </capLists>
  </sipProfile>
</device>
```


### Other things
**Make desktops:**
File listing
TFTPROOT/Desktops/800x480x24
* List.xml
* mainbackground.png
* clearbackground.png

List.xml content:
``` 
<CiscoIPPhoneImageList>
    <ImageItem Image="TFTP:Desktops/800x480x24/mainbackground.png" URL="TFTP:Desktops/800x480x24/mainbackground.png"/>
    <ImageItem Image="TFTP:Desktops/800x480x24/clearbackground.png" URL="TFTP:Desktops/800x480x24/clearbackground.png"/>
</CiscoIPPhoneImageList>
``` 


# Lessons Learned
### 1.  Asymmetric Video Profile Issue
Outbound calls from the phone (Cisco -> Doorbell) had perfect two way video. Inbound calls from the camera (Doorbell -> Cisco) resulted in dropping calls with  an internal state of "Everyone is busy/congested."

#### Cause
When the doorbell camera makes a call, it uses a high-bitrate video schema (e.g., 1080p High-Profile H.264) through the SIP INVITE. The hardware decoder inside the Cisco phone cannot process that resolution. Instead of re-negotiating the call, the enterprise firmware responds with "488 Not Acceptable Here." 

#### Solution
AXIS Device Administration Interface -> VoIP -> change the SIP default encoding stream down to 1280x720 (720p) max resolution with a Baseline Profile H.264 standard. 

### 2. SDP Translation Panic Issue
Outbound calls through VOIP.ms result in a drop failure without showing any sign of leaving the local network.

#### Cause
When dialing out, the Cisco phone makes an offer with both ulaw audio and h264 video. Asterisk tries to pass this video on to VOIP.ms, which can only process audio. Since this is already specified as mismatched in Asterisk, it drops the call internally.

#### Solution
I added allow=h264 to pjsip.conf, which forces Asterisk to allow the h264 offer. VOIP.ms will remove the video track afterward as it won't use it anyway. 

### 3. Multi-Subnet and Routing
When using different networks or VLANS, the routing with these phones will often be broken so we must add the following:

* Network Keepalives: In pjsip.conf include rewrite_contact=yes, force_rport=yes, and qualify_frequency=30. This is to ensure that the UDP streams do not timeout inside firwalls or routers.
* Transport: You must add explicit \[transport-udp\] elements to instruct the PJSIP to adapt network headers across different subnets without trying to use NAT.


#### Good Resources
[usecallmanager.nz](https://usecallmanager.nz/documentation-overview.html) - Gives in depth configuration options for the phones
[usecallmanager/tftpboot](https://github.com/usecallmanagernz/tftpboot) - Configuration Examples on Github
[BrightVoip Tutorial](https://www.brightvoip.co.uk/2018/04/04/sip-xml-configuration-files-for-cisco-handsets/)
[Axis Video Doorbell Tutorial](https://www.youtube.com/watch?v=6r9ugELqIVw&t=1561s)

More configuration examples:
https://gist.github.com/Wriar/cd12599282eceacae21f2547c2c8e827
https://www.encrypted.at/cisco-7940-sepmac-cnf-xml/
https://github.com/provisioner/Provisioner/blob/master/endpoint/cisco/sip78xx/SEP%24mac.cnf.xml


