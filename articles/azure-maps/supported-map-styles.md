---
title: Desteklenen harita stilleri | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure haritaları tarafından desteklenen farklı eşleme işleme stilleri hakkında bilgi edineceksiniz.
author: farah-alyasari
ms.author: v-faalya
ms.date: 05/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: a65f20e04603403a1d3a180e364626d5b4440661
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77664548"
---
# <a name="azure-maps-supported-map-styles"></a>Azure haritalar desteklenen harita stilleri
Azure Maps, aşağıda açıklandığı gibi birkaç farklı yerleşik harita stilini destekler.

## <a name="road"></a>Road
**Yol** Haritası, bu özelliklerin etiketleriyle birlikte yollar, doğal ve yapay Özellikler görüntüleyen standart bir eşlemedir.

![yol haritası stili](./media/supported-map-styles/road.png)

**Geçerli API 'Ler:**
* [Harita resmi](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Harita kutucuğu](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Web SDK eşlemesi denetimi
* Android harita denetimi

## <a name="blank-and-blank_accessible"></a>boş ve blank_accessible

**Boş** ve **blank_accessible** harita stilleri üzerinde verilerin görselleştirilmesi için boş bir tuval sağlar. **Blank_accessible** stili, temel harita görüntülenmese de haritanın konum ayrıntıları ile ekran okuyucusu güncelleştirmeleri sağlamaya devam edecektir.

> [!Note]
> Web SDK 'sında harita DIV öğesinin CSS `background-color` stilini ayarlayarak haritanın arka plan rengini değiştirebilirsiniz.

**Geçerli API 'Ler:**
* Web SDK eşlemesi denetimi

## <a name="satellite"></a>uydu 
**Uydu** stili, uydu ve hava imasıy 'nin bir birleşimidir.

![uydu kutucuk harita stili](./media/supported-map-styles/satellite.png)

**Geçerli API 'Ler:**
* [Uydu kutucuğu](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* Web SDK eşlemesi denetimi
* Android harita denetimi

## <a name="satellite_road_labels"></a>satellite_road_labels
Bu harita stili, uydu ve hava imakına göre popüler yollar ve Etiketler için bir karma değer.

![satellite_road_labels harita stili](./media/supported-map-styles/satellite-road-labels.png)

**Geçerli API 'Ler:**
* Web SDK eşlemesi denetimi
* Android harita denetimi

## <a name="grayscale_dark"></a>grayscale_dark
**gri tonlamalı koyu** , yol haritası stilinin karanlık bir sürümüdür.

![gray_scale harita stili](./media/supported-map-styles/grayscale-dark.png)

**Geçerli API 'Ler:**
* [Harita resmi](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Harita kutucuğu](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Web SDK eşlemesi denetimi 
* Android harita denetimi


## <a name="grayscale_light"></a>grayscale_light
**gri tonlama ışığı** , yol haritası stilinin hafif bir sürümüdür.

![gri tonlamalı hafif harita stili](./media/supported-map-styles/grayscale-light.png)

**Geçerli API 'Ler:**
* Web SDK eşlemesi denetimi
* Android harita denetimi


## <a name="night"></a>Gerçekleştirilmiştir
**gece** , yol haritası stilinin renkli yollar ve semboller ile karanlık bir sürümüdür.

![gece harita stili](./media/supported-map-styles/night.png)

**Geçerli API 'Ler:**
* Web SDK eşlemesi denetimi
* Android harita denetimi

## <a name="road_shaded_relief"></a>road_shaded_relief
**trafik gölgeli tahliye** , dünya dağılımlarıyla birlikte tamamlanan bir Azure Maps ana stilidir.

![gölgeli emniyet Haritası stili](./media/supported-map-styles/shaded-relief.png)

**Geçerli API 'Ler:**
* [Harita kutucuğu](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Web SDK eşlemesi denetimi
* Android harita denetimi

## <a name="high_contrast_dark"></a>high_contrast_dark

**high_contrast_dark** , diğer stillerden daha yüksek karşıtlığı olan koyu bir harita stilidir.

![yüksek karşıtlık koyu harita stili](./media/supported-map-styles/high-contrast-dark.png)

**Geçerli API 'Ler:**
* Web SDK eşlemesi denetimi

## <a name="next-steps"></a>Sonraki adımlar

Azure haritalar 'da harita stili ayarlama hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Harita stili seçin](https://docs.microsoft.com/azure/azure-maps/choose-map-style)
