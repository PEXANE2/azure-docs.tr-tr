---
title: Dil desteği-Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Bu makale, Görüntü İşleme özellikleri tarafından desteklenen doğal dillerin bir listesini sağlar; OCR, görüntü analizi.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.openlocfilehash: 1e4dfb399432278ee09d1c016faae8fd3b584647
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89318055"
---
# <a name="language-support-for-computer-vision"></a>Görüntü İşleme için dil desteği

Görüntü İşleme bazı özellikleri birden çok dili destekler; Burada bahsedilen özellikler yalnızca Ingilizce 'yi destekler.

## <a name="optical-character-recognition-ocr"></a>Optik Karakter Tanıma (OCR)

Görüntü İşleme OCR API 'Leri çeşitli dilleri destekler. Bir dil kodu belirtmenizi gerektirmez. Daha fazla bilgi için bkz. [optik karakter tanıma (OCR)](concept-recognizing-text.md) .

|Dil| Dil kodu | OCR APı 'SI | V 3.0 'ı oku | V 3.1 Genel önizlemeyi oku |
|:-----|:----:|:-----:|:---:|:---:|
|Arapça | `ar`|✔ | | |
|Basitleştirilmiş Çince | `zh-Hans`|✔ | |✔ |
|Geleneksel Çince | `zh-Hant`|✔ | | |
|Çekçe | `cs` |✔ | | |
|Danca | `da` |✔ | | |
|Felemenkçe | `nl` |✔ |✔ |✔ |
|İngilizce | `en` |✔ |✔ |✔ |
|Fince | `fi` |✔ | | |
|Fransızca | `fr` |✔ |✔ |✔ |
|Almanca | `de` |✔ |✔ |✔ |
|Yunanca | `el` |✔ | | |
|Macarca | `hu` |✔ | | |
|İtalyanca | `it` |✔ |✔ |✔ |
|Japonca | `ja` |✔ | | |
|Korece | `ko` |✔ | | |
|Norveççe | `nb` |✔ | | |
|Lehçe | `pl` |✔ | | |
|Portekizce | `pt` |✔ |✔ |✔ |
|Rumence | `ro` |✔ | | |
|Rusça | `ru` |✔ | | |
|Sırpça (Kiril) | `sr-Cyrl` |✔ | | |
|Sırpça (Latin) | `sr-Latn` |✔ | | |
|Slovakça | `sk` |✔ | | |
|İspanyolca | `es` |✔ |✔ |✔ |
|İsveççe | `sw` |✔ | | |
|Türkçe | `tr` |✔ | | |

## <a name="image-analysis"></a>Görüntü analizi

[Analyze-Image](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API 'sindeki bazı eylemler, sorgu parametresiyle belirtilen diğer dillerde sonuç döndürebilir `language` . Diğer Eylemler, belirtilen dilin ne olduğuna bakmaksızın sonuçları Ingilizce olarak döndürür ve bazıları desteklenmeyen diller için bir özel durum oluşturur. Eylemler `visualFeatures` ve sorgu parametreleriyle birlikte belirtilir `details` ; görüntü analizine sahip yapabileceğiniz tüm eylemlerin bir listesi için bkz. [genel bakış](overview.md) .

|Dil | Dil kodu | Kategoriler | Etiketler | Description | Yetişkin | Markalar | Color | Yüzler | ImageType | Nesneler | Ünlü Kişiler | Bölümler |
|:---|:---:|:----:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Çince | `zh`    | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|İngilizce | `en`   | ✔ | ✔| ✔|✔|✔|✔|✔|✔|✔|✔|✔|
|Japonca | `ja`   | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Portekizce | `pt` | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|İspanyolca | `es`    | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda bahsedilen Görüntü İşleme özelliklerini kullanmaya başlayın.

* [Yerel görüntüyü çözümleme (REST)](./quickstarts/csharp-analyze.md)
* [Yazdırılan metni Ayıkla (REST)](./quickstarts/csharp-print-text.md)
