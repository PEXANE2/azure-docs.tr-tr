---
title: Yönlendirme için araç tüketim modelleri | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar'da yönlendirme için araç tüketim modelleri hakkında bilgi edineceksiniz.
author: subbarayudukamma
ms.author: skamma
ms.date: 05/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: c1572eddf78ca2d5f8f4e3ee9f1fe47b0d43f5aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77190243"
---
# <a name="consumption-model"></a>Tüketim modeli

Yönlendirme hizmeti, araca özgü Tüketim Modelinin ayrıntılı bir açıklaması için bir dizi parametre sağlar.
**VehicleEngineType**değerine bağlı olarak, iki ana Tüketim Modelleri desteklenir: _Yanma_ ve _Elektrik_. Aynı istekte farklı modellere ait parametreleri belirtmek yanlış. Ayrıca, Tüketim Modeli parametreleri aşağıdaki **travelMode** değerleri ile kullanılamaz: _bisiklet_ ve _yaya._

## <a name="parameter-constraints-for-consumption-model"></a>Tüketim modeli için parametre kısıtlamaları

Her iki Tüketim Modelinde de parametreler belirtilirken bazı bağımlılıklar vardır. Yani, açıkça bazı parametrelerin belirtilmesi, diğer bazı parametrelerin belirtilmesi gerektirebilir. Dikkat edilmesi gereken bu bağımlılıklar şunlardır:

* Tüm parametreler, kullanıcı tarafından **sürekli SpeedConsumption'in** belirtilmesi gerekir. **Sürekli SpeedConsumption** belirtilmemişse, başka bir tüketim modeli parametresi belirtmek bir hatadır. **AraçAğırlık** parametresi bu gereksinim için bir istisnadır.
* **ivmeVerimlilik** ve **yavaşlamaVerimlilik** her zaman bir çift olarak belirtilmelidir (yani, her ikisi de veya hiçbiri).
* **IvmeVerimlilik** ve **yavaşlamaVerimlilik** belirtilirse, değerlerinin çarpımı 1'den büyük olmamalıdır (sürekli hareketi önlemek için).
* **uphillEfficiency** ve **downhillEfficiency** her zaman bir çift olarak belirtilmelidir (yani, her ikisi de ya da hiçbiri).
* **Yokuş yukarı Verimlilik** ve yokuş aşağı **Verimlilik** belirtilirse, değerlerinin çarpımı 1'den büyük olmamalıdır (sürekli hareketi önlemek için).
* \* __Verimlilik__ parametreleri kullanıcı tarafından belirtilirse, **araç Ağırlığı** da belirtilmelidir. **VehicleEngineType** _yanma_olduğunda, **fuelEnergyDensityInMJoulesPerLiter** de belirtilmelidir.
* **maxChargeInkWh** ve **currentChargeInkWh** her zaman bir çift olarak belirtilmelidir (yani, her ikisi de veya hiçbiri).

> [!NOTE]
> Yalnızca **sabitSpeedConsumption** belirtilmişse, tüketim hesaplamaları için eğimler ve araç ivmesi gibi başka tüketim yönleri dikkate alınmaz.

## <a name="combustion-consumption-model"></a>Yanma tüketim modeli

Yanma Tüketim Modeli **vehicleEngineType** _yanma_ayarlanır kullanılır.
Bu modele ait parametrelerin listesi aşağıda verilmiştir. Ayrıntılı açıklama için Parametreler bölümüne bakın.

* constantSpeedConsumptionInLitersPerHundredkm
* araçAğırlık
* geçerliFuelInLitres
* yardımcıPowerInLitersPerHour
* yakıtEnergyDensityInMJoulesPerLiter
* ivmeVerimlilik
* yavaşlamaVerimlilik
* uphillEfficiency
* yokuş aşağıVerimlilik

## <a name="electric-consumption-model"></a>Elektrik tüketim modeli

Electric Consumption Model **vehicleEngineType** _elektrikli_olarak ayarlandığında kullanılır.
Bu modele ait parametrelerin listesi aşağıda verilmiştir. Ayrıntılı açıklama için Parametreler bölümüne bakın.

* constantSpeedConsumptionInkWhPerHundredkm
* araçAğırlık
* akımChargeInkWh
* maxChargeInkWh
* yardımcıPowerInkW
* ivmeVerimlilik
* yavaşlamaVerimlilik
* uphillEfficiency
* yokuş aşağıVerimlilik

## <a name="sensible-values-of-consumption-parameters"></a>Tüketim parametrelerinin mantıklı değerleri

Belirli bir tüketim parametreleri kümesi, küme tüm açık gereksinimleri karşılayabilir olsa bile reddedilebilir. Belirli bir parametrenin değeri veya çeşitli parametrelerin değerlerinin bir birleşimi, tüketim değerlerinin makul olmayan büyüklüklerine yol açabileceği düşünüldüğünde gerçekleşir. Bu durumda, tüketim parametrelerinin tüm mantıklı değerlerini karşılamak için uygun özen alındığından, büyük olasılıkla bir giriş hatası gösterir. Belirli bir tüketim parametre kümesinin reddedilmesi durumunda, eşlik eden hata iletisi nedeni(ler) hakkında metinsel bir açıklama içerir.
Parametrelerin ayrıntılı açıklamaları her iki model için de mantıklı değerlere örnekler içerir.
