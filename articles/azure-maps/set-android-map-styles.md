---
title: Azure haritalar 'ı kullanarak bir harita stili ayarlayın Android SDK
description: Bir haritanın stilini ayarlamanın iki yolunu öğrenin. Stili ayarlamak için Düzen dosyasında ya da etkinlik sınıfında Microsoft Azure haritalarını Android SDK nasıl kullanacağınızı öğrenin.
author: anastasia-ms
ms.author: v-stharr
ms.date: 11/18/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 8c7689fb87575ac6e150f793b43f35e8bf6adc83
ms.sourcegitcommit: 5b93010b69895f146b5afd637a42f17d780c165b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96532492"
---
# <a name="set-map-style-using-azure-maps-android-sdk"></a>Azure haritalar 'ı kullanarak harita stili ayarlama Android SDK

Bu makalede, Azure Maps Android SDK kullanarak harita stillerinin nasıl ayarlanacağı gösterilmektedir. Azure haritalar 'ın aralarından seçim yapabileceğiniz altı farklı harita stili vardır. Desteklenen harita stilleri hakkında daha fazla bilgi için bkz. [Azure Maps 'de desteklenen harita stilleri](./supported-map-styles.md).

## <a name="prerequisites"></a>Önkoşullar

1. [Azure haritalar hesabı oluşturma](quick-demo-map-app.md#create-an-azure-maps-account)
2. Birincil anahtar veya abonelik anahtarı olarak da bilinen [birincil bir abonelik anahtarı alın](quick-demo-map-app.md#get-the-primary-key-for-your-account).
3. [Azure haritalar Android SDK](./how-to-use-android-map-control-library.md)indirin ve yükleyin.


## <a name="set-map-style-in-the-layout"></a>Düzende harita stili ayarla

Etkinlik sınıfınızın düzen dosyasında bir harita stili ayarlayabilirsiniz. Düzenleyin `res > layout > activity_main.xml` , bu nedenle aşağıdaki gibi görünür:

```XML
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >

    <com.microsoft.azure.maps.mapcontrol.MapControl
        android:id="@+id/mapcontrol"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:mapcontrol_centerLat="47.602806"
        app:mapcontrol_centerLng="-122.329330"
        app:mapcontrol_zoom="12"
        app:mapcontrol_style="grayscale_dark"
        />

</FrameLayout>
```

`mapcontrol_style`Yukarıdaki öznitelik, eşleme stilini **grayscale_dark** olarak ayarlar.

:::image type="content" source="./media/set-android-map-styles/grayscale-dark.png" border="true" alt-text="Azure haritalar, stili grayscale_dark olarak gösteren harita resmi":::

## <a name="set-map-style-in-the-mainactivity-class"></a>MainActivity sınıfında harita stilini ayarla

Aynı zamanda, MainActivity sınıfında de harita stili ayarlanabilir. Dosyasını açın `java > com.example.myapplication > MainActivity.java` ve aşağıdaki kod parçacığını **OnCreate ()** yöntemine kopyalayın. Bu kod, eşleme stilini **satellite_road_labels** olarak ayarlar.

>[!WARNING]
>Android Studio gerekli sınıfları içeri aktarmayabilir.  Sonuç olarak, kodun çözümlenemeyecek bazı başvuruları olacaktır. Gerekli sınıfları içeri aktarmak için, çözülmemiş her başvurunun üzerine gelin ve `Alt + Enter` (seçenek + bir Mac üzerinde dön) tuşuna basın.

```Java
mapControl.onReady(map -> {

    //Set the camera of the map.
    map.setCamera(center(47.64, -122.33), zoom(14));

    //Set the style of the map.
    map.setStyle((style(SATELLITE_ROAD_LABELS)));
       
});
```

:::image type="content" source="./media/set-android-map-styles/satellite-road-labels.png" border="true" alt-text="Azure haritalar, stili satellite_road_labels olarak gösteren harita resmi":::