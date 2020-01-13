---
title: Yönlendirme için araç tüketim modelleri | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure haritaları 'nda yönlendirme için araç tüketim modelleri hakkında bilgi edineceksiniz.
author: subbarayudukamma
ms.author: skamma
ms.date: 05/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 5a8a0778ce279846b0d7a66b1729b6898e80a4b5
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2020
ms.locfileid: "75911704"
---
# <a name="consumption-model"></a>Tüketim modeli

Çevrimiçi yönlendirme, araç açısından özel tüketim modelinin ayrıntılı açıklaması için bir dizi parametre sağlar.
**vehicleEngineType** değerine bağlı olarak iki asıl Tüketim Modeli desteklenmektedir: _Combustion_ ve _Electric_. Aynı istekte farklı modellere ait parametrelerin belirtilmesi hataya neden olur.
Tüketim Modeli, _bicycle_ ve _pedestrian_ **travelMode** değerleriyle birlikte kullanılamaz.

## <a name="parameter-constraints-for-consumption-model"></a>Tüketim modeli için parametre kısıtlamaları

Her iki tüketim modelinde de, bazı parametrelerin açıkça belirtilmesi, diğerlerinin belirtilmesini gerektirir. Bu bağımlılıklar şunlardır:

* Tüm parametrelerin **constantSpeedConsumption** için Kullanıcı tarafından belirtilmesi gerekir. **ConstantSpeedConsumption** belirtilmemişse, başka bir tüketim modeli parametresini **vehicleWeight**özel durumu ile belirtmek bir hatadır.
* **ivationverimlilik** ve **yavaşlatma ationverimlilik** her zaman çift olarak belirtilmelidir (yani veya None).
* **İvationverimlilik** ve **yavaşlatma ationverimlilik** belirtilmişse, değerlerinin çarpımı 1 ' den büyük olmamalı (kalıcı hareketi engellemek için).
* **uphillEfficiency** ve **downhillEfficiency** her zaman çift olarak belirtilmelidir (yani veya None).
* **UphillEfficiency** ve **downhillEfficiency** belirtilirse, değerlerinin çarpımı 1 ' den büyük olmamalı (kalıcı hareketi engellemek için).
* \*__verimlilik__ parametreleri Kullanıcı tarafından belirtilmişse, **vehicleWeight** de belirtilmelidir. **VehicleEngineType** , _combustion_olduğunda, **fuelenercaydensityınmjoulesperlitre** de belirtilmelidir.
* **Maxchargeınkwh** ve **Currentchargeınkwh** her zaman çift olarak belirtilmelidir (yani veya None).

> [!NOTE]
> Yalnızca **constantSpeedConsumption** belirtilirse, Slopes ve araç hızlandırma gibi başka bir tüketim yönü, tüketim hesaplamaları için hesaba alınmaz.

## <a name="combustion-consumption-model"></a>Combustion tüketim modeli

**vehicleEngineType** değeri _combustion_ olarak ayarlandığında Combustion Tüketim Modeli kullanılır.
Bu modele ait parametrelerin listesi aşağıda verilmiştir. Ayrıntılı açıklama için parametreler bölümüne bakın.

* constantSpeedConsumptionInLitersPerHundredkm
* vehicleWeight
* Currentfuelinlinler
* auxiliaryPowerInLitersPerHour
* Fuelenercaydensityınmjoulesperlitre
* Ivationverimlilik
* Yavaşlatma verimliliği
* uphillEfficiency
* downhillEfficiency

## <a name="electric-consumption-model"></a>Elektrik tüketim modeli

**vehicleEngineType** değeri _electric_ olarak ayarlandığında Electric Tüketim Modeli kullanılır.
Bu modele ait parametrelerin listesi aşağıda verilmiştir. Ayrıntılı açıklama için parametreler bölümüne bakın.

* constantSpeedConsumptionInkWhPerHundredkm
* vehicleWeight
* Currentchargeınkwh
* Maxchargeınkwh
* auxiliaryPowerInkW
* Ivationverimlilik
* Yavaşlatma verimliliği
* uphillEfficiency
* downhillEfficiency

## <a name="sensible-values-of-consumption-parameters"></a>Tüketim parametrelerinin erişilebilir değerleri

Belirli bir tüketim parametreleri kümesi, yukarıda belirtilen tüm açık gereksinimleri yerine getirebilse de reddedilebilir. Belirli bir parametre değeri veya çeşitli parametrelerin değerlerinin bir birleşimi, tüketim değerlerinin makul olmayan magnitudes ine yol açacağından oluşur. Bu durumda, büyük olasılıkla bir giriş hatası belirtilir ve bu, tüketim parametrelerinin tüm uygun değerlerini karşılamak için gerekli bir işlem yapılır. Belirli bir tüketim parametreleri kümesinin reddedilmesi durumunda, eşlik eden hata mesajı neden (s) için bir metinsel açıklama içerecektir.
Parametrelerin ayrıntılı açıklamaları her iki model için de onaylanan değer örneklerine sahiptir.
