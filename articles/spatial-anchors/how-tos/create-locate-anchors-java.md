---
title: Java'da bağlantı noktası oluşturma &
description: Java'daki Azure Uzamsal Bağlantılarını kullanarak çapaların nasıl oluşturulup buluncayada bulunabildiğini ayrıntılı olarak açıklama.
author: ramonarguelles
manager: vriveras
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: e03fb8bfc221edf209fd2480eedad822983d59a9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74270184"
---
# <a name="how-to-create-and-locate-anchors-using-azure-spatial-anchors-in-java"></a>Java'da Azure Uzamsal Bağlantılarını kullanarak bağlantı oluşturma ve bulma

> [!div  class="op_single_selector"]
> * [Unity](create-locate-anchors-unity.md)
> * [Objective-C](create-locate-anchors-objc.md)
> * [Swift](create-locate-anchors-swift.md)
> * [Android Java](create-locate-anchors-java.md)
> * [C++/NDK](create-locate-anchors-cpp-ndk.md)
> * [C++/WinRT](create-locate-anchors-cpp-winrt.md)

Azure Uzamsal Çapalar, dünyadaki çapaları farklı aygıtlar arasında paylaşmanıza olanak tanır. Birkaç farklı geliştirme ortamını destekler. Bu makalede, Java'daki Azure Uzamsal Çapalar SDK'nın nasıl kullanılacağına aşağıdakileri yapacağız:

- Azure Uzamsal Çapalar oturumunu doğru şekilde ayarlayın ve yönetin.
- Yerel çapalarda özellikler oluşturun ve ayarlayın.
- Buluta yükleyin.
- Bulut uzamsal bağlantı larını bulun ve silin.

## <a name="prerequisites"></a>Ön koşullar

Bu kılavuzu tamamlamak için şunları yaptığınızdan emin olun:

- [Azure Uzamsal Çapalar genel görünümünü](../overview.md)okuyun.
- 5 dakikalık [Quickstarts](../index.yml)biri tamamlandı.
- Java hakkında temel bilgi.
- <a href="https://developers.google.com/ar/discover/" target="_blank">ARCore</a>temel bilgi .

[!INCLUDE [Start](../../../includes/spatial-anchors-create-locate-anchors-start.md)]

[CloudSpatialAnchorSession](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession) sınıfı hakkında daha fazla bilgi edinin.

```java
    private CloudSpatialAnchorSession mCloudSession;
    // In your view handler
    mCloudSession = new CloudSpatialAnchorSession();
```

[!INCLUDE [Account Keys](../../../includes/spatial-anchors-create-locate-anchors-account-keys.md)]

[SessionConfiguration](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.sessionconfiguration) sınıfı hakkında daha fazla bilgi edinin.

```java
    mCloudSession.getConfiguration().setAccountKey("MyAccountKey");
```

[!INCLUDE [Access Tokens](../../../includes/spatial-anchors-create-locate-anchors-access-tokens.md)]

```java
    mCloudSession.getConfiguration().setAccessToken("MyAccessToken");
```

[!INCLUDE [Access Tokens Event](../../../includes/spatial-anchors-create-locate-anchors-access-tokens-event.md)]

[TokenRequiredListener](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.tokenrequiredlistener) arayüzü hakkında daha fazla bilgi edinin.

```java
    mCloudSession.addTokenRequiredListener(args -> {
        args.setAccessToken("MyAccessToken");
    });
```

[!INCLUDE [Asynchronous Tokens](../../../includes/spatial-anchors-create-locate-anchors-asynchronous-tokens.md)]

```java
    mCloudSession.addTokenRequiredListener(args -> {
        CloudSpatialAnchorSessionDeferral deferral = args.getDeferral();
        MyGetTokenAsync(myToken -> {
            if (myToken != null) args.setAccessToken(myToken);
            deferral.complete();
        });
    });
```

[!INCLUDE [Azure AD Tokens](../../../includes/spatial-anchors-create-locate-anchors-aad-tokens.md)]

```java
    mCloudSession.getConfiguration().setAuthenticationToken("MyAuthenticationToken");
```

[!INCLUDE [Azure AD Tokens Event](../../../includes/spatial-anchors-create-locate-anchors-aad-tokens-event.md)]

```java
    mCloudSession.addTokenRequiredListener(args -> {
        args.setAuthenticationToken("MyAuthenticationToken");
    });
```

[!INCLUDE [Asynchronous Tokens](../../../includes/spatial-anchors-create-locate-anchors-asynchronous-tokens.md)]

```java
    mCloudSession.addTokenRequiredListener(args -> {
        CloudSpatialAnchorSessionDeferral deferral = args.getDeferral();
        MyGetTokenAsync(myToken -> {
            if (myToken != null) args.setAuthenticationToken(myToken);
            deferral.complete();
        });
    });
```

[!INCLUDE [Setup](../../../includes/spatial-anchors-create-locate-anchors-setup-non-ios.md)]

[Başlangıç](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.start) yöntemi hakkında daha fazla bilgi edinin.

```java
    mCloudSession.setSession(mSession);
    mCloudSession.start();
```

[!INCLUDE [Frames](../../../includes/spatial-anchors-create-locate-anchors-frames.md)]

[processFrame](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.processframe) yöntemi hakkında daha fazla bilgi edinin.

```java
    mCloudSession.processFrame(mSession.update());
```

[!INCLUDE [Feedback](../../../includes/spatial-anchors-create-locate-anchors-feedback.md)]

[SessionUpdatedListener](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.sessionupdatedlistener) arayüzü hakkında daha fazla bilgi edinin.

```java
    mCloudSession.addSessionUpdatedListener(args -> {
        auto status = args->Status();
        if (status->UserFeedback() == SessionUserFeedback::None) return;
        NumberFormat percentFormat = NumberFormat.getPercentInstance();
        percentFormat.setMaximumFractionDigits(1);
        mFeedback = String.format("Feedback: %s - Recommend Create=%s",
            FeedbackToString(status.getUserFeedback()),
            percentFormat.format(status.getRecommendedForCreateProgress()));
    });
```

[!INCLUDE [Creating](../../../includes/spatial-anchors-create-locate-anchors-creating.md)]

[CloudSpatialAnchor](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchor) sınıfı hakkında daha fazla bilgi edinin.

```java
    // Create a local anchor, perhaps by hit-testing and creating an ARAnchor
    Anchor localAnchor = null;
    List<HitResult> hitResults = mSession.update().hitTest(0.5f, 0.5f);
    for (HitResult hit : hitResults) {
        Trackable trackable = hit.getTrackable();
        if (trackable instanceof Plane) {
            if (((Plane) trackable).isPoseInPolygon(hit.getHitPose())) {
                localAnchor = hit.createAnchor();
                break;
            }
        }
    }

    // If the user is placing some application content in their environment,
    // you might show content at this anchor for a while, then save when
    // the user confirms placement.
    CloudSpatialAnchor cloudAnchor = new CloudSpatialAnchor();
    cloudAnchor.setLocalAnchor(localAnchor);
    Future createAnchorFuture = mCloudSession.createAnchorAsync(cloudAnchor);
    CheckForCompletion(createAnchorFuture, cloudAnchor);

    // ...

    private void CheckForCompletion(Future createAnchorFuture, CloudSpatialAnchor cloudAnchor) {
        new android.os.Handler().postDelayed(() -> {
            if (createAnchorFuture.isDone()) {
                try {
                    createAnchorFuture.get();
                    mFeedback = String.format("Created a cloud anchor with ID=%s", cloudAnchor.getIdentifier());
                }
                catch(InterruptedException e) {
                    mFeedback = String.format("Save Failed:%s", e.getMessage());
                }
                catch(ExecutionException e) {
                    mFeedback = String.format("Save Failed:%s", e.getMessage());
                }
            }
            else {
                CheckForCompletion(createAnchorFuture, cloudAnchor);
            }
        }, 500);
    }
```

[!INCLUDE [Session Status](../../../includes/spatial-anchors-create-locate-anchors-session-status.md)]

[getSessionStatusAsync](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.getsessionstatusasync) yöntemi hakkında daha fazla bilgi edinin.

```java
    Future<SessionStatus> sessionStatusFuture = mCloudSession.getSessionStatusAsync();
    CheckForCompletion(sessionStatusFuture);

    // ...

    private void CheckForCompletion(Future<SessionStatus> sessionStatusFuture) {
        new android.os.Handler().postDelayed(() -> {
            if (sessionStatusFuture.isDone()) {
                try {
                    SessionStatus value = sessionStatusFuture.get();
                    if (value.getRecommendedForCreateProgress() < 1.0f) return;
                    // Issue the creation request...
                }
                catch(InterruptedException e) {
                    mFeedback = String.format("Session status error:%s", e.getMessage());
                }
                catch(ExecutionException e) {
                    mFeedback = String.format("Session status error:%s", e.getMessage());
                }
            }
            else {
                CheckForCompletion(sessionStatusFuture);
            }
        }, 500);
    }
```

[!INCLUDE [Setting Properties](../../../includes/spatial-anchors-create-locate-anchors-setting-properties.md)]

[getAppProperties](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchor.getappproperties) yöntemi hakkında daha fazla bilgi edinin.

```java
    CloudSpatialAnchor cloudAnchor = new CloudSpatialAnchor();
    cloudAnchor.setLocalAnchor(localAnchor);
    Map<String,String> properties = cloudAnchor.getAppProperties();
    properties.put("model-type", "frame");
    properties.put("label", "my latest picture");
    Future createAnchorFuture = mCloudSession.createAnchorAsync(cloudAnchor);
    // ...
```

[!INCLUDE [Update Anchor Properties](../../../includes/spatial-anchors-create-locate-anchors-updating-properties.md)]

[UpdateAnchorPropertiesAsync](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.updateanchorpropertiesasync) yöntemi hakkında daha fazla bilgi edinin.

```java
    CloudSpatialAnchor anchor = /* locate your anchor */;
    anchor.getAppProperties().put("last-user-access", "just now");
    Future updateAnchorPropertiesFuture = mCloudSession.updateAnchorPropertiesAsync(anchor);
    CheckForCompletion(updateAnchorPropertiesFuture);

    // ...

    private void CheckForCompletion(Future updateAnchorPropertiesFuture) {
        new android.os.Handler().postDelayed(() -> {
            if (updateAnchorPropertiesFuture.isDone()) {
                try {
                    updateAnchorPropertiesFuture.get();
                }
                catch(InterruptedException e) {
                    mFeedback = String.format("Updating Properties Failed:%s", e.getMessage());
                }
                catch(ExecutionException e) {
                    mFeedback = String.format("Updating Properties Failed:%s", e.getMessage());
                }
            }
            else {
                CheckForCompletion1(updateAnchorPropertiesFuture);
            }
        }, 500);
    }
```

[!INCLUDE [Getting Properties](../../../includes/spatial-anchors-create-locate-anchors-getting-properties.md)]

[getAnchorPropertiesAsync](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.getanchorpropertiesasync) yöntemi hakkında daha fazla bilgi edinin.

```java
    Future<CloudSpatialAnchor> getAnchorPropertiesFuture = mCloudSession.getAnchorPropertiesAsync("anchorId");
    CheckForCompletion(getAnchorPropertiesFuture);

    // ...

    private void CheckForCompletion(Future<CloudSpatialAnchor> getAnchorPropertiesFuture) {
        new android.os.Handler().postDelayed(() -> {
            if (getAnchorPropertiesFuture.isDone()) {
                try {
                    CloudSpatialAnchor anchor = getAnchorPropertiesFuture.get();
                    if (anchor != null) {
                        anchor.getAppProperties().put("last-user-access", "just now");
                        Future updateAnchorPropertiesFuture = mCloudSession.updateAnchorPropertiesAsync(anchor);
                        // ...
                    }
                } catch (InterruptedException e) {
                    mFeedback = String.format("Getting Properties Failed:%s", e.getMessage());
                } catch (ExecutionException e) {
                    mFeedback = String.format("Getting Properties Failed:%s", e.getMessage());
                }
            } else {
                CheckForCompletion(getAnchorPropertiesFuture);
            }
        }, 500);
    }
```

[!INCLUDE [Expiration](../../../includes/spatial-anchors-create-locate-anchors-expiration.md)]

[setExpiration](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchor.setexpiration) yöntemi hakkında daha fazla bilgi edinin.

```java
    Date now = new Date();
    Calendar cal = Calendar.getInstance();
    cal.setTime(now);
    cal.add(Calendar.DATE, 7);
    Date oneWeekFromNow = cal.getTime();
    cloudAnchor.setExpiration(oneWeekFromNow);
```

[!INCLUDE [Locate](../../../includes/spatial-anchors-create-locate-anchors-locating.md)]

[createWatcher](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.createwatcher) yöntemi hakkında daha fazla bilgi edinin.

```java
    AnchorLocateCriteria criteria = new AnchorLocateCriteria();
    criteria.setIdentifiers(new String[] { "id1", "id2", "id3" });
    mCloudSession.createWatcher(criteria);
```

[!INCLUDE [Locate Events](../../../includes/spatial-anchors-create-locate-anchors-locating-events.md)]

[AnchorLocatedListener](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.anchorlocatedlistener) arayüzü hakkında daha fazla bilgi edinin.

```java
    mCloudSession.addAnchorLocatedListener(args -> {
        switch (args.getStatus()) {
            case Located:
                CloudSpatialAnchor foundAnchor = args.getAnchor();
                // Go add your anchor to the scene...
                break;
            case AlreadyTracked:
                // This anchor has already been reported and is being tracked
                break;
            case NotLocatedAnchorDoesNotExist:
                // The anchor was deleted or never existed in the first place
                // Drop it, or show UI to ask user to anchor the content anew
                break;
            case NotLocated:
                // The anchor hasn't been found given the location data
                // The user might in the wrong location, or maybe more data will help
                // Show UI to tell user to keep looking around
                break;
        }
    });
```

[!INCLUDE [Deleting](../../../includes/spatial-anchors-create-locate-anchors-deleting.md)]

[DeleteAnchorAsync](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.deleteanchorasync) yöntemi hakkında daha fazla bilgi edinin.

```java
    Future deleteAnchorFuture = mCloudSession.deleteAnchorAsync(cloudAnchor);
    // Perform any processing you may want when delete finishes (deleteAnchorFuture is done)
```

[!INCLUDE [Stopping](../../../includes/spatial-anchors-create-locate-anchors-stopping.md)]

[Durdurma](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.stop) yöntemi hakkında daha fazla bilgi edinin.

```java
    mCloudSession.stop();
```

[!INCLUDE [Resetting](../../../includes/spatial-anchors-create-locate-anchors-resetting.md)]

[Sıfırlama](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.reset) yöntemi hakkında daha fazla bilgi edinin.

```java
    mCloudSession.reset();
```

[!INCLUDE [Cleanup](../../../includes/spatial-anchors-create-locate-anchors-cleanup-java.md)]

[Yakın](https://docs.microsoft.com/java/api/com.microsoft.azure.spatialanchors.cloudspatialanchorsession.close) yöntem hakkında daha fazla bilgi edinin.

```java
    mCloudSession.close();
```

[!INCLUDE [Next Steps](../../../includes/spatial-anchors-create-locate-anchors-next-steps.md)]
