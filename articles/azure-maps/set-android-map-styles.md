---
title: Azure haritalar 'ı kullanarak bir harita stili ayarlayın Android SDK | Microsoft Azure haritaları
description: Bu makalede, Android SDK için stille ilgili işlevleri Microsoft Azure eşler hakkında bilgi edineceksiniz.
author: farah-alyasari
ms.author: v-faalya
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 96564a89a2b64203eef913b0d8300f0dafa332c5
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77209588"
---
# <a name="set-map-style-using-azure-maps-android-sdk"></a>Azure haritalar 'ı kullanarak harita stili ayarlama Android SDK

Bu makalede, Azure Haritalar Android SDK kullanarak harita stillerini ayarlamak için kullanabileceğiniz iki yol gösterilmektedir. Azure haritalar 'ın aralarından seçim yapabileceğiniz altı farklı harita stili vardır. Desteklenen harita stilleri hakkında daha fazla bilgi için bkz. [Azure Maps 'de desteklenen harita stilleri](./supported-map-styles.md).


## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki işlemi gerçekleştirmek için, bir harita yüklemek üzere [Azure Maps Android SDK](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) yüklemeniz gerekir.


## <a name="set-map-style-in-the-layout"></a>Düzende harita stili ayarla

Etkinlik sınıfınızın düzen dosyasında bir harita stili ayarlayabilirsiniz. **Kaynak > düzeni activity_main. xml >** düzenleyin, bu nedenle aşağıdaki gibi görünür:

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

Yukarıdaki `mapcontrol_style` özniteliği harita stilini **grayscale_dark**olarak ayarlar. 

<center>

![Style-grayscale_dark](./media/set-android-map-styles/grayscale-dark.png)</center>

## <a name="set-map-style-in-the-activity-class"></a>Etkinlik sınıfında harita stilini ayarla

Eşleme stili etkinlik sınıfında ayarlanabilir. Aşağıdaki kod parçacığını `MainActivity.java` sınıfınızın **OnCreate ()** yöntemine kopyalayın. Bu kod, eşleme stilini **satellite_road_labels**olarak ayarlar.

```Java
mapControl.onReady(map -> {
    //Set the camera of the map.
    map.setCamera(center(47.64, -122.33), zoom(14));

    //Set the style of the map.
    map.setStyle(style(MapStyle.SATELLITE));
});
```

<center>

![Style-uydu-yol-etiketleri](./media/set-android-map-styles/satellite-road-labels.png)</center>