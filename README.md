# conviva-android-media3-imasdk
Module supporting video analytics for applications using the IMA extension of Media3.
Using this module facilitates auto collection of all possible metrics in addition to player state changes.

## Core SDK Compatibility
This module is compatible with Conviva Core SDK version 4.0.49 and above.

## Media3 Compatibility
This module is compatible with Media3 version 1.8.0.

## Gradle dependency
Add the following dependency to your `build.gradle` file:
```groovy
implementation 'com.conviva.sdk:conviva-media3-ima-sdk:<version>'
```

## Conviva Integration
The module can be setup passing the corresponding `androidx.media3.exoplayer.ima.ImaAdsLoader`
(in case of CSAI) or the `androidx.media3.exoplayer.ima.ImaServerSideAdInsertionMediaSource.AdsLoader`
instance (in case of SSAI / DAI) to Conviva to enable auto collection of metrics and events.

An optional `AdModuleListener` instance can be passed to Conviva, in case the application needs to listen ad events.

**WARNING**: Integrating this Conviva module will replace any existing `AdEventListener` and `AdErrorListener` set on
the `ImaAdsLoader` instance. If the application needs to listen to ad events, an `AdModuleListener` instance should also
be passed.

```java
ConvivaVideoAnalytics videoAnalytics = ConvivaAnalytics.buildVideoAnalytics(context);
ConvivaAdAnalytics adAnalytics = ConvivaAnalytics.buildAdAnalytics(context, videoAnalytics);
Map<String, Object> adMetadata = new HashMap<>();
adMetadata.put(ConvivaSdkConstants.AD_TAG_URL, adUrl); // required for CSAI
adAnalytics.setAdPlayerInfo(adMetadata);

adAnalytics.setAdListener(
        adsLoader, // ImaAdsLoader in case of CSAI / ImaServerSideAdInsertionMediaSource.AdsLoader in case of SSAI
        moduleListener, // Optional listener to receive ad events, omit if not needed
        adMetadata
);
```
## Important Notes
- Ensure that the Ad Tag URL is provided in the ad metadata using the key `ConvivaSdkConstants.AD_TAG_URL` in case of CSAI.
  This should NOT be set in case of SSAI / DAI.

## Limitations
- For SSAI / DAI, a 0-2 seconds delay might be observed in reporting the metrics, which is a limitation of the paradigm itself.