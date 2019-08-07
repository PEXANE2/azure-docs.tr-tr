---
title: Azure haritalar 'da desteklenen harita stilleri | Microsoft Docs
description: Azure haritalar tarafından desteklenen harita stilleri
author: walsehgal
ms.author: v-musehg
ms.date: 05/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 1aad2284c0f64c92efaefe3f9145d95c4aabec67
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68839437"
---
# <a name="azure-maps-supported-map-styles"></a>Azure haritalar desteklenen harita stilleri
Azure Maps, aşağıda açıklandığı gibi birkaç farklı yerleşik harita stilini destekler.

## <a name="road"></a>Road
**Yol** Haritası, bu özelliklerin etiketleriyle birlikte yollar, doğal ve yapay Özellikler görüntüleyen standart bir eşlemedir.

![Road](./media/supported-map-styles/road.png)

**Geçerli API 'Ler:**
* [Harita resmi](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Harita kutucuğu](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Web SDK eşlemesi denetimi
* Android harita denetimi

## <a name="blank-and-blank_accessible"></a>boş ve blank_accessible

**Blank** ve **blank_accessible** harita stilleri üzerinde verilerin görselleştirilecek boş bir tuval sağlar. **Blank_accessible** stili, temel harita görüntülenmese de haritanın bulunduğu konum ayrıntıları ile ekran okuyucu güncelleştirmeleri sağlamaya devam edecektir.

> [!Note]
> Web SDK 'sında harita DIV öğesinin CSS `background-color` stilini ayarlayarak haritanın arka plan rengini değiştirebilirsiniz.

**Geçerli API 'Ler:**
* Web SDK eşlemesi denetimi

## <a name="satellite"></a>uydu 
**Uydu** stili, uydu ve hava imasıy 'nin bir birleşimidir.

![uydu](./media/supported-map-styles/satellite.png)

**Geçerli API 'Ler:**
* [Uydu kutucuğu](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* Web SDK eşlemesi denetimi
* Android harita denetimi

## <a name="satellite_road_labels"></a>satellite_road_labels
Bu harita stili, uydu ve hava imakına göre popüler yollar ve Etiketler için bir karma değer.

![satellite_road_labels](./media/supported-map-styles/satellite_road_labels.png)

**Geçerli API 'Ler:**
* Web SDK eşlemesi denetimi
* Android harita denetimi

## <a name="grayscale_dark"></a>grayscale_dark
**gri tonlamalı koyu** , yol haritası stilinin karanlık bir sürümüdür.

![gray_scale](./media/supported-map-styles/grayscale_dark.png)

**Geçerli API 'Ler:**
* Web SDK eşlemesi denetimi 
* Android harita denetimi


## <a name="grayscale_light"></a>grayscale_light
**gri tonlama ışığı** , yol haritası stilinin hafif bir sürümüdür.

![gri tonlamalı hafif](./media/supported-map-styles/grayscale_light.png)

**Geçerli API 'Ler:**
* Web SDK eşlemesi denetimi
* Android harita denetimi


## <a name="night"></a>gerçekleştirilmiştir
**gece** , yol haritası stilinin renkli yollar ve semboller ile karanlık bir sürümüdür.

![gerçekleştirilmiştir](./media/supported-map-styles/night.png)

**Geçerli API 'Ler:**
* Web SDK eşlemesi denetimi
* Android harita denetimi

## <a name="road_shaded_relief"></a>road_shaded_relief
**trafik gölgeli tahliye** , dünya dağılımlarıyla birlikte tamamlanan bir Azure Maps ana stilidir.

![gölgeli tahliye](./media/supported-map-styles/shaded-relief.png)

**Geçerli API 'Ler:**
* [Harita kutucuğu](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Web SDK eşlemesi denetimi
* Android harita denetimi
