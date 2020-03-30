---
title: Desteklenen harita stilleri | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar tarafından desteklenen farklı harita oluşturma stilleri hakkında bilgi edineceksiniz.
author: philmea
ms.author: philmea
ms.date: 05/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 2eafe3c16a89723d55ec52fde785e9ec69e45e0c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334034"
---
# <a name="azure-maps-supported-map-styles"></a>Azure Haritalar desteklenen harita stilleri
Azure Haritalar, aşağıda açıklandığı gibi birkaç farklı yerleşik harita stilini destekler.

## <a name="road"></a>Road
**Yol** haritası, bu özellikler için etiketlerle birlikte yolları, doğal ve yapay özellikleri görüntüleyen standart bir haritadır.

![yol haritası stili](./media/supported-map-styles/road.png)

**Geçerli API'ler:**
* [Harita görüntüsü](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Harita döşemesi](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Web SDK harita kontrolü
* Android harita kontrolü

## <a name="blank-and-blank_accessible"></a>boş ve blank_accessible

**Boş** ve **blank_accessible** eşleme stilleri, verileri görselleştirmek için boş bir tuval sağlar. **Blank_accessible** stili, temel harita görüntülenmese bile, ekran okuyucugüncellemeleri ile haritanın konum ayrıntılarını sunmaya devam eder.

> [!Note]
> Web SDK'da harita DIV öğesinin CSS `background-color` stilini ayarlayarak haritanın arka plan rengini değiştirebilirsiniz.

**Geçerli API'ler:**
* Web SDK harita kontrolü

## <a name="satellite"></a>satellite 
**Uydu** stili uydu ve havadan görüntülerin bir kombinasyonudur.

![uydu kiremit harita stili](./media/supported-map-styles/satellite.png)

**Geçerli API'ler:**
* [Uydu döşemesi](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* Web SDK harita kontrolü
* Android harita kontrolü

## <a name="satellite_road_labels"></a>satellite_road_labels
Bu harita stili, uydu ve havadan görüntülerin üzerine döşenmiş yolların ve etiketlerin bir melezidir.

![satellite_road_labels harita stili](./media/supported-map-styles/satellite-road-labels.png)

**Geçerli API'ler:**
* Web SDK harita kontrolü
* Android harita kontrolü

## <a name="grayscale_dark"></a>grayscale_dark
**grayscale koyu** yol haritası tarzı koyu bir sürümüdür.

![gray_scale harita stili](./media/supported-map-styles/grayscale-dark.png)

**Geçerli API'ler:**
* [Harita görüntüsü](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Harita döşemesi](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Web SDK harita kontrolü 
* Android harita kontrolü


## <a name="grayscale_light"></a>grayscale_light
**gri tonlama ışık** yol haritası tarzının hafif bir sürümüdür.

![gri tonlu ışık haritası stili](./media/supported-map-styles/grayscale-light.png)

**Geçerli API'ler:**
* Web SDK harita kontrolü
* Android harita kontrolü


## <a name="night"></a>Gece
**gece** renkli yollar ve semboller ile yol haritası tarzı karanlık bir sürümüdür.

![gece haritası tarzı](./media/supported-map-styles/night.png)

**Geçerli API'ler:**
* Web SDK harita kontrolü
* Android harita kontrolü

## <a name="road_shaded_relief"></a>road_shaded_relief
**yol gölgeli kabartma** Dünya'nın hatları ile tamamlanmış bir Azure Maps ana tarzıdır.

![gölgeli kabartma harita stili](./media/supported-map-styles/shaded-relief.png)

**Geçerli API'ler:**
* [Harita döşemesi](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Web SDK harita kontrolü
* Android harita kontrolü

## <a name="high_contrast_dark"></a>high_contrast_dark

**high_contrast_dark** diğer stilleri daha yüksek kontrast ile karanlık bir harita tarzıdır.

![yüksek kontrastlı koyu harita stili](./media/supported-map-styles/high-contrast-dark.png)

**Geçerli API'ler:**
* Web SDK harita kontrolü

## <a name="next-steps"></a>Sonraki adımlar

Azure Haritalar'da harita stilini nasıl ayarlayamaize ne kadar iyi olun:

> [!div class="nextstepaction"]
> [Harita stili seçme](https://docs.microsoft.com/azure/azure-maps/choose-map-style)
