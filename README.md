# Sony SmartBand SWR10 and Android Oreo, Pie (Android 8, 9)

## Issue

Unable to connect Sony SmartBand to Android Oreo or Pie (Android 8 or 9).

The issue has the following symptoms:
 
* the Sony SmartBand SWR10 is not recognised by the Sony Smart Connect App (SmartBand does not appear in the list of devices),
* the SmartBand SWR10 app (if able to be opened) shows a yellow warning: `Disconnected. Reconnect with NFC.`,
* the SmartBand does not connect to the phone,
* no SmartBand notifications appear,
* a disconnected SmartBand notification appears.

## Cause

If the SmartBand SWR10 is not already associated with the phone then the only way of connecting it to the SmartBand SWR10 app is via NFC. 

Tapping the SmartBand's NFC chip against the phone's triggers a broadcast to the Smart Connect app with the SmartBand's bluetooth MAC address. The Sony Connect application then instructs the SmartBand SWR10 application to connect to the SmartBand.

Unfortunately Android 8 made a change disallowing [implicit broadcasts](https://commonsware.com/blog/2017/04/11/android-o-implicit-broadcast-ban.html). The Smart Connect app has not been updated to use explicit broadcasts, which means the Smart Connect app never receives this broadcast, therefore no attempt is made to connect to the band.

The cause can be confirmed by using CatLog, attempting to connect the SmartBand and finding the following warning:

```
W/BroadcastQueue: Background execution not allowed: receiving Intent { act=android.nfc.action.NDEF_DISCOVERED dat=semc://liveware/C1/0/SWR10/XX:XX:XX:XX:XX:XX/BAMd flg=0x10900010 pkg=vcom.sonyericsson.extras.liveware cmp=com.sonyericsson.extrase.liveware/.asf.EventReceiver (has extras) } to com.sonyericsson.extras.liveware/.asf.EventReceiver
```

## Fix

From the page [detailing the broadcast ban](https://commonsware.com/blog/2017/04/11/android-o-implicit-broadcast-ban.html):

> Even on O, two sets of receivers will still receive the broadcast:
>
> * Those whose apps have targetSdkVersion of 25 or lower

This also applies to Android Pie.

By installing an older version of the Smart Connect app (targeted at version 25 or lower of the SDK), the SmartBand SWR10 can be connected by NFC as normal. The Sony Connect app can then be updated if required.

Sony Smart Connect app old versions can be found via an web search. This fix was tested with version 5.7.18.401-50718401 of the app.

### Summary

* Uninstall current version of Smart Connect app,
* install Smart Connect app version (you may need to enable install from unknown sources),
* connect SmartBand SWR10 via NFC.
