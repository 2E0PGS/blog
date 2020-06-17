Using IC-7100 for data modes on a PC

"SET"/"Connectors"/"ACC/USB Output Select": "AF" for data modes direct and "IF" for anything via GQRX (IQ)
"SET"/"Connectors"/"DATA OFF MOD": "MIC,ACC" not sure why
"SET"/"Connectors"/"DATA MOD": "USB" not sure why
"SET"/"Connectors"/"USB Audio SQL": "OFF (OPEN)" This is to allow USB audio to always be present regardless of SQL level etc

ACC is accesory socket... DIN

DATA mode seems to allow audio input via USB instead of using the microphone it also seems to change the RX bandwidth to a very small 1.2k or 500 and remove DSP?

See screenshots

So generally switch "ACC/USB Output Select" to IF and AF output. 
Also switch "DATA OFF MOD" 
Probably dont bother with DATA mode as its too narrow for SSTV etc.

Scan function "lambda F" can do + or - 1 MHZ max freq sweep. The SET button allows for resume times etc.

passing auto into VirtualBox VM for windows decoding.

qssstv `Floating point exception (core dumped)`