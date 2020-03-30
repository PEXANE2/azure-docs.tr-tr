---
title: Azure Haritalar Android SDK| Microsoft Azure Haritaları
description: Bu makalede, Android SDK için Microsoft Azure Haritalar stiliyle ilgili işlevler hakkında bilgi edineceksiniz.
author: philmea
ms.author: philmea
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: a4d761abf54682ed0263922d0a118debc9eccf0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334364"
---
# <a name="set-map-style-using-azure-maps-android-sdk"></a>Azure Haritalar Android SDK'yı kullanarak harita stilini ayarlama

Bu makalede, Azure Haritalar Android SDK'yı kullanarak harita stillerini ayarlamanın iki yolu gösterilmektedir. Azure Haritalar'da seçim yapabileceğiniz altı farklı harita stili vardır. Desteklenen harita stilleri hakkında daha fazla bilgi için [Azure Haritalar'da desteklenen harita stillerine](./supported-map-styles.md)bakın.


## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki işlemi tamamlamak için, bir harita yüklemek için [Azure Haritalar Android SDK'yı](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) yüklemeniz gerekir.


## <a name="set-map-style-in-the-layout"></a>Düzende harita stilini ayarlama

Etkinlik sınıfınızın düzen dosyasında bir harita stili ayarlayabilirsiniz. Düzenle **activity_main.xml > > düzen res,** böylece aşağıdaki gibi görünüyor:

```XML
<?xml version="1.0" encoding="utf-8"?>
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

Yukarıdaki `mapcontrol_style` öznitelik, harita stilini **grayscale_dark**ayarlar. 

<center>

![stil grayscale_dark](./media/set-android-map-styles/grayscale-dark.png)</center>

## <a name="set-map-style-in-the-activity-class"></a>Etkinlik sınıfında harita stilini ayarlama

Eşlemi stili etkinlik sınıfında ayarlanabilir. Aşağıdaki kod parçacıklarını sınıfınızın `MainActivity.java` **onCreate()** yöntemine kopyalayın. Bu kod, harita stilini **satellite_road_labels**ayarlayacaktır.

```Java
mapControl.onReady(map -> {
    //Set the camera of the map.
    map.setCamera(center(47.64, -122.33), zoom(14));

    //Set the style of the map.
    map.setStyle(style(MapStyle.SATELLITE));
});
```

<center>

![stil-uydu-yol-etiketleri](./media/set-android-map-styles/satellite-road-labels.png)</center>