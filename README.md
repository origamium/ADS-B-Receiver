# ADS-B-Receiver

envファイルになんか書いて各種サイトに巡ったりしてfeeder_idを取得したりすると使えます  
envファイルの例 

```
FEEDER_TZ=Asia/Tokyo
READSB_DEVICE_TYPE=rtlsdr
READSB_GAIN=auto
ADSB_SDR_SERIAL=00000001
FEEDER_LAT=(float)
FEEDER_LONG=(float)
FEEDER_ALT_M=(float and metric)
FEEDER_NAME=(freedom name)

FR24_KEY=
PIAWARE_KEY=
AIRNAV_KEY=
PLANEFINDER_KEY=
```

弊社はraspberry pi 4 model bとかで余裕で動いています 
ADS-B受信機は適当なものをお買い求めください