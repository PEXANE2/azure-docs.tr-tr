---
title: Verileri Özetleme
titleSuffix: Azure Machine Learning
description: Veri kümesindeki sütunlar için temel açıklayıcı istatistik raporu oluşturmak için Azure Machine Learning'de Verileri Özetle modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 01/27/2020
ms.openlocfilehash: b0def12582dd3795e1b17334406e28d77c3c5656
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477451"
---
# <a name="summarize-data"></a>Verileri Özetleme

Bu makalede, Azure Machine Learning tasarımcısının (önizleme) bir modülü açıklanmaktadır.

Giriş tablosundaki her sütunu açıklayan bir dizi standart istatistiksel ölçü oluşturmak için Verileri Özetle modüllerini kullanın.

Özet istatistikler, tam veri kümesinin özelliklerini anlamak istediğinizde yararlıdır. Örneğin, bilmeniz gerekebilir:

- Her sütunda kaç eksik değer vardır?
- Bir özellik sütununda kaç benzersiz değer vardır?
- Her sütun için ortalama ve standart sapma nedir?

Modül, her sütun için önemli puanları hesaplar ve girdi olarak sağlanan her değişken (veri sütunu) için bir özet istatistik satırı döndürür.

## <a name="how-to-configure-summarize-data"></a>Özet Verileri yapılandırma  

1. Veri hattınıza **Veri Özetle** modüllerini ekleyin. Bu modülü **Tasarımcıda İstatistiksel Fonksiyonlar** kategorisinde bulabilirsiniz.

1. Rapor oluşturmak istediğiniz veri kümesini bağlayın.

    Yalnızca bazı sütunlar üzerinde rapor vermek istiyorsanız, çalışmak üzere sütun alt kümesini yansıtmak için [Veri Kümesi modülündeki Sütunları Seç'i](select-columns-in-dataset.md) kullanın.

1. Ek parametre gerekmez. Varsayılan olarak, modül giriş olarak sağlanan tüm sütunları analiz eder ve sütunlardaki değerlerin türüne bağlı olarak [Sonuçlar](#results) bölümünde açıklandığı gibi ilgili bir istatistik kümesi ni verir.

1. Boru hattını gönderin.

## <a name="results"></a>Sonuçlar

Modülden gelen rapor aşağıdaki istatistikleri içerebilir. 

|Sütun adı|Açıklama|
|------|------|  
|**Özellik**|Sütunun adı|
|**Sayısı**|Tüm satırların sayısı|
|**Benzersiz Değer Sayısı**|Sütundaki benzersiz değer sayısı|
|**Eksik Değer Sayısı**|Sütundaki benzersiz değer sayısı|
|**Dk**|Sütundaki en düşük değer|  
|**Max**|Sütundaki en yüksek değer|
|**Demek**|Tüm sütun değerlerinin ortalaması|
|**Ortalama Sapma**|Sütun değerlerinin ortalama sapması|
|**1. Dörttebirlik**|İlk dörtte birlik değer|
|**Medyan**|Ortanca sütun değeri|
|**3. Dörttebirlik**|Üçüncü dörtte birlik değer|
|**Mod**|Sütun değerlerinin modu|
|**Aralığı**|Maksimum ve minimum değerler arasındaki değer sayısını temsil eden eden eden aralar|
|**Örnek Varyans**|Sütun için varyans; bkz. Not|
|**Örnek Standart Sapma**|Sütun için standart sapma; bkz. Not|
|**Örnek Çarpıklık**|Sütun için çarpıklık; bkz. Not|
|**Örnek Kurtoz**|Sütun için Kurtosis; bkz. Not|
|**P0.5**|%0,5 yüzdelik|
|**P1**|%1 yüzdelik|
|**P5**|%5 yüzdelik|
|**P95**|%95 yüzdelik|
|**P99.5**|%99.5 yüzdelik |

## <a name="technical-notes"></a>Teknik notlar

- Sayısal olmayan sütunlar için yalnızca Count, Benzersiz değer sayısı ve Eksik değer sayısı değerleri hesaplanır. Diğer istatistikler için null değeri döndürülür.

- Boolean değerlerini içeren sütunlar şu kurallar kullanılarak işlenir:

    - Min hesaplanırken, mantıksal VE uygulanır.
    
    - Max hesaplanırken, mantıksal bir OR uygulanır
    
    - Aralık'ı hesaplarken, modül önce sütundaki benzersiz değer sayısının 2'ye eşit olup olmadığını denetler.
    
    - Kayan nokta hesaplamaları gerektiren herhangi bir istatistik hesaplanırken, True değerleri 1.0 olarak kabul edilir ve False değerleri 0.0 olarak kabul edilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın.  
