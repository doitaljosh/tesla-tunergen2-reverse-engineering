Tesla Tuner SOME/IP payload reverse engineering

## Summary:
SOME/IP is the IPC protocol used by the Tesla ICE to control the radio tuner.
The protocol itself is open source, but the payloads are undocumented.
Extracting raw SOME/IP payloads is pretty easy, since Wireshark already has a
decoder built-in for the high-level API. Below are some that I've already decoded.

## Payload dump samples:

### Tune to 93300Mhz:
```
Service ID:	0x0065
Method ID:	0x0005
Length:		28
Client ID:	0x1343
Session ID:	0x0001
SOME/IP ver:	1
Interface ver:	2
Message type:	0x00 (request)
Return code:	0x00 (ok)
Payload:
0000   00 00 00 ff 00 01 6c 74 00 00 00 00 00 00 00 01
0010   00 00 00 00
```
#### Payload fields:

|Bytes|Type|Value|
|---|---|---|
|[0:3]|uint32|255|
|[4:7]|uint32|frequency: 93300|
|[8:11]|uint32|0x00|
|[12:15]|uint32|0x01|
|[16:19]|uint32|0x00|


#### Response:
```
Service ID:	0x0065
Method ID:	0x0005
Length:		8
Client ID:	0x1343
Session ID:	0x0001
SOME/IP ver:	1
Interface ver:	2
Message type:	0x80 (response)
Return code:	0x00 (ok)
```
### Get station info:
```
Service ID:	0xffff
Method ID:	0x8100
Length:		48
Client ID:	0x0000
Session ID:	0x000d
SOME/IP ver:	1
Interface ver:	1
Message type:	0x02 (notification)
Return code:	0x00 (ok)
Some/IP SDP:
Flags:		0xc0
Reserved:	0x00 0x00 0x00
EntArray len:	0x00000010
EntArray:
	Type:		0x06 (Subscribe eventgroup)
	Index 1:	0x00
	Index 2:	0x00
	NumOpts:	0x10
	Service ID:	0x0065
	Session ID:	0x0001
	Major version:	2
	TTL:		0xffffff
Reserved: 0x000000:	0x00
	Mask:		0x00
	EventGroup ID:	0x00c9
OptsArray len:	0x0000000c
OptsArray:
	Option:		0x00 (IPv4 endpoint)
	Length:		0x09
	Type:		0x04
Reserved: 0x000000: 	0x00
	IPv4 address:	0xc0a80079 192.168.0.121
Reserved: 0x000000:	0x00
	Protocol:	0x06 (TCP)
	Port:		0xecd2 60626
```	
#### Response:
```
Service ID:	0xffff
Method ID:	0x8100
Length:		36
Client ID:	0x0000
Session ID:	0x000c
SOME/IP ver:	1
Interface ver:	1
Message type:	0x02 (notification)
Return code:	0x00 (ok)
Some/IP SDP:
Flags:		0xc0
Reserved:	0x00 0x00 0x00
EntArray len:	0x00000010
EntArray:
	Type:		0x07 (Subscribe eventgroup ack)
	Index 1:	0x00
	Index 2:	0x00
	NumOpts:	0x00
	Service ID:	0x0065
	Session ID:	0x0001
	Major version:	2
	TTL:		0xffffff
Reserved: 0x000000:	0x00
	Mask:		0x00
	EventGroup ID:	0x00c9
OptsArray len:	0x00000000
```
#### Subscribed response:
```
Service ID:	0x0065
Method ID:	0x8005
Length:		411
Client ID:	0x0000
Session ID:	0x0000
SOME/IP ver:	1
Interface ver:	2
Message type:	0x02 (notification)
Return code:	0x00 (ok)
Payload:
0000   00 00 00 41 00 01 6c 74 00 00 00 10 20 3a 13 13
0010   00 00 13 13 00 00 00 01 00 00 00 00 00 00 80 00
0020   00 00 00 8a 00 00 00 0c ef bb bf 39 33 2d 33 20
0030   46 4c 5a 00 00 00 00 0c ef bb bf 39 33 2d 33 20
0040   46 4c 5a 00 00 00 00 0c ef bb bf 39 33 2d 33 20
0050   46 4c 5a 00 00 00 00 08 ef bb bf 46 4c 5a 20 00
0060   00 00 00 2a ef bb bf 39 33 2d 33 20 46 4c 5a 20
0070   45 2e 54 2e 20 4b 61 74 79 20 50 65 72 72 79 20
0080   2f 20 4b 61 6e 79 65 20 57 65 73 74 20 00 00 00
0090   00 0c ef bb bf 39 33 2d 33 20 46 4c 5a 00 00 00
00a0   00 04 ef bb bf 00 00 00 00 04 ef bb bf 00 00 00
00b0   00 c1 00 00 00 40 ef bb bf 39 33 2d 33 20 46 4c
00c0   5a 20 41 6e 69 64 6a 61 72 20 26 20 4c 65 76 69
00d0   6e 65 20 31 2d 38 30 30 2d 37 34 37 2d 46 52 45
00e0   45 20 20 41 63 63 69 64 65 6e 74 20 41 74 74 6f
00f0   72 6e 65 79 73 00 20 00 00 00 0c ef bb bf 39 33
0100   2d 33 20 46 4c 5a 00 1f 00 00 00 08 ef bb bf 45
0110   2e 54 2e 00 01 00 00 00 1b ef bb bf 4b 61 74 79
0120   20 50 65 72 72 79 20 2f 20 4b 61 6e 79 65 20 57
0130   65 73 74 00 04 00 00 00 1b ef bb bf 4b 61 74 79
0140   20 50 65 72 72 79 20 2f 20 4b 61 6e 79 65 20 57
0150   65 73 74 00 80 00 00 00 08 ef bb bf 45 2e 54 2e
0160   00 81 00 00 00 0c ef bb bf 39 33 2d 33 20 46 4c
0170   5a 00 8d 00 00 00 04 00 00 00 09 00 00 07 00 00
0180   00 00 0c 00 00 00 19 00 00 00 43 00 00 00 3c 00
0190   00 00 03
```
#### tunert output:
```
STATION EVENT
  Frequency: 93300
  Multicast: 1
  Num Multicast: 2
  HD Mode: 1
  HD Acquired:   1
  Quality: 71
  Name: FLZ 
  Text: 93-3 FLZ
    Names=Kanye   ,Kanye   ,93-3 FLZ,FLZ ,93-3 FLZ E.T. Katy Perry / Kanye West ,93-3 FLZ,,,
    Genres=res=
  Metadata:
    Station Name Long = 93-3 FLZ Anidjar & Levine 1-800-747-FREE  Accident Attorneys(32)
    Station Name Short = 93-3 FLZ(31)
    Title = E.T.(1)
    Artist = Katy Perry / Kanye West(4)
    Artist = Katy Perry / Kanye West(128)
    Title = E.T.(129)
    Unknown type 141 = 93-3 FLZ
  Return: 3
```

#### Payload fields:
|Bytes|Data type|Value|
|---|---|---|
|[0:3]|type|65|
|[4:7]|frequency|93300|
|[8:11]|Unknown|16|
|[12:15]|Station ID1|0x203a1313|
|[16:19]|Station ID2|0x00001313|
|[20:23]|HD Station ID1|0x00000001|
|[24:27]|HD Station ID2|0x00000000|
|[28:31]|Unknown|32768|
|[32:35]|Unknown|138 10001010|
|[36:39]|Unknown|144 10010000|
|[40:55]|FM1 text|Size: 12,Magic: 0xefbbbf,FM1 radiotext: "93-3 FLZ"|
|[56:71]|FM2 text|Size: 12,Magic: 0xefbbbf,FM2 radiotext: "93-3 FLZ"|
|[72:87]|FM3 text|Size: 12,Magic: 0xefbbbf,FM3 radiotext: "93-3 FLZ"|
|[88:99]|Station code|Size: 8,Magic: 0xefbbbf,Station code: "FLZ "|
|[100:155]|Station name full|Size: 42,Magic: 0xefbbbf,Station name full: "93-3 FLZ E.T. Katy Perry / Kanye West     "|	
|[156:171]|Station name short 1|Size: 12,Magic: 0xefbbbf,Station name short: "93-3 FLZ"|
|[172:179]|Genre|Size: 4,Magic: 0xefbbbf,Genre: 0|
|[180:187]|Unknown|193|
|[188:255]|Station name long|Size: 64,Magic: 0xefbbbf,Station name long: "93-3 FLZ Anidjar & Levine 1-800-747-FREE  Accident Attorneys    "|	
|[256]|Unknown|32|
|[257:272]|Station name short 2|Size: 12,Magic: 0xefbbbf,Station name short: "93-3 FLZ"|	
|[273]|Unknown|31|
|[274:285]|Title 1|Size: 8,Magic: 0xefbbbf,Title: "E.T. "|	
|[286]|Unknown|1|
|[287:317]|Artist 1|Size: 27,Magic: 0xefbbbf,Artist: "Katy Perry / Kanye West "|	
|[318]|Unknown|4|
|[319:349]|Artist 2|Size: 27,Magic: 0xefbbbf,Artist: "Katy Perry / Kanye West "|
|[350]|Unknown|128|
|[351:362]|Title 2|Size: 8,Magic: 0xefbbbf,Title: "E.T. "|
|[363]|Unknown|129|
|[364:379]|Station name short 3|Size: 12,Magic: 0xefbbbf,Station name short: "93-3 FLZ"|
|[380]|Station type|141|
|[381:384]|Unknown|4|
|[385:388]|Genres|9|
|[389:392]|Flags|0x0700 01110000 HDReception,HDAudio,ScrollingPS|
|[393:396]|Unknown|12|
|[397:400]|Quality1|25|
|[401:403]|Quality2|71|
|[404:407]|Quality3|60|
|[408:411]|Return|3|

```
struct TunerApiPayload_EntityType {
	uint32_t length;
	const uint8_t magic[3] = {0xef, 0xbb, 0xbf};
	char data[length];
};

const struct TunerApiPayload_StationNotify {
	uint8_t type;
	uint32_t tuneFreq;
	uint32_t unk1 = 16;
	uint32_t stationId1;
	uint32_t stationId2;
	uint32_t hdStationId1;
	uint32_t hdStationId2;
	uint32_t unk2;
	uint32_t unk3;
	struct TunerApiPayload_EntityType fm1Text;
	struct TunerApiPayload_EntityType fm2Text;
	struct TunerApiPayload_EntityType fm3Text;
	struct TunerApiPayload_EntityType stationCode;
	struct TunerApiPayload_EntityType stationNameFull;
	struct TunerApiPayload_EntityType stationNameShort1;
	struct TunerApiPayload_EntityType genre;
	struct TunerApiPayload_EntityType unk4;
	struct TunerApiPayload_EntityType stationNameLong;
	struct TunerApiPayload_EntityType stationNameShort2;
	struct TunerApiPayload_EntityType title1;
	struct TunerApiPayload_EntityType artist1;
	struct TunerApiPayload_EntityType artist2;
	struct TunerApiPayload_EntityType title2;
	struct TunerApiPayload_EntityType stationNameShort3;
	uint8_t stationType;
	uint32_t unk5;
	uint32_t genres;
	uint32_t flags;
	uint32_t unk6;
	uint32_t quality1;
	uint32_t quality2;
	uint32_t quality3;
	uint32_t return;
};
```
## Service and method IDs

```
Service ID (SID):
	Method ID (MID):
			Function:
				Payloads:
0x0065:	
	0x0002		seek station
				Send payload:	[0x000000ff][u32 func_id][0x00000000][0x00000000][0x00000000]
					Function IDs:
						Manual up:	1
						Manual down:	2
						Auto up:	3
						Auto down:	4
				Return payload:	none
	0x0003		force update current station
				Send payload:	[0x00000000][0x000000ff][u32 freq][0x00000000]
				Return payload:	none
	0x0005		tune to station
				Send payload:	[0x000000ff][u32 freq][0x00000000][0x00000010][u32 id1][u32 id2][u32 hdradio_id1][u32 hdradio_id2][0x00000001][0x00000000]
				Return payload: none

0x0066:
	0x0001		get antenna state
				Send payload:	Request to sid 0x0066 mid 0x0001	
				Return payload:	[u32 ant_id][u32 fault_mask][u32 current][u32 voltage]
					Antenna IDs:
						FMDAB:	1
						MQS:	2
	0x0002		set antenna power state
				Send payload:	[u32 ant_id][u8 state][0x00000000]
					Antenna IDs:
						FMDAB:	1
						MQS:	2
					States:
						Off:	0
						On:	1
				Return payload:	none	
	0x0011		get tuner diagnostic info
				Send payload:	Request to sid 0x0066 mid 0x0011
				Return payload:	0000000b0000000001000100010000

0x006e:
	0x0005		set fm region
				Send payload:	[u32 fm_region_id]
					Region IDs:
						US:		0
						LATIN_AMERICA:	1
						CANADA:		2
						BRAZIL:		3
						EUROPE:		4
						SOUTH_AFRICA:	5
						REST_OF_WORLD:	6
						MIDDLE_EAST:	7
						JAPAN:		8
						OCEANIA:	9
						AUSTRALIA:	10
						ASIA:		11
						SOUTH_KOREA:	12
						CHINA:		13
						INDIA:		14
						RUSSIA:		15
				Return payload:	none
	0x0006		get FM region
				Send payload:	Request to sid 0x006e mid 0x0006
				Return payload:	[u32 fm_region_id][0x00000000]
	0x07d1		get Tesla part number
				Send payload:	Request to sid 0x006e mid 0x07d1
				Return payload:	[u32 size=16][0xefbbbf][char[size-3] part_number]
	0x07d2		get HW revision
				Send payload:	Request to sid 0x006e mid 0x07d2
				Return payload:	[u32 size=7][0xefbbbf][char[size-3] hw_revision]
	0x07d3		get Dirana SW version
				Send payload:	Request to sid 0x006e mid 0x07d3
				Return payload:	[u32 size=29][0xefbbbf][char[size-3] dirana_swver]
	0x07d4		get Saturn SW version
				Send payload:	Request to sid 0x006e mid 0x07d4
				Return payload:	[u32 size=22][0xefbbbf][char[size-3] saturn_swver]
	0x07d5		get application SW version
				Send payload:	Request to sid 0x006e mid 0x07d5
				Return payload:	[u32 size=30][0xefbbbf][char[size-3] app_swver]
	0x07d6		get production info
				Send payload:	Request to sid 0x006e mid 0x07d6
				Return payload:	[u32 size=4][0xefbbbf][char[size-3] prod_info]
	0x07d7		get temperature
				Send payload:	Request to sid 0x006e mid 0x07d7
				Return payload:	[u8 temperature]
0x0078:
	0x0001		startup tuner
				Send payload:	Request to sid 0x0078 mid 0x0001
				Return payload:	[0x00000000]
0xffff:
	0x8001		get current station info
				Send payload:	
				Return payload:					
```
