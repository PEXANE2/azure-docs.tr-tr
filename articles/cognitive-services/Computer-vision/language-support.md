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
ms.openlocfilehash: 16bb5fa41389dcf1ddd8ee2e3434e15755bb0d8d
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87290935"
---
# <a name="language-support-for-computer-vision"></a>Görüntü İşleme için dil desteği

Görüntü İşleme bazı özellikleri birden çok dili destekler; Burada bahsedilen özellikler yalnızca Ingilizce 'yi destekler.

## <a name="optical-character-recognition-ocr"></a>Optik Karakter Tanıma (OCR)

Görüntü İşleme OCR API 'Leri çeşitli dilleri destekler. Bir dil kodu belirtmenizi gerektirmez. Daha fazla bilgi için bkz. [optik karakter tanıma (OCR)](concept-recognizing-text.md) .

|Dil| Dil kodu | OCR APı 'SI | V 3.0 'ı oku | Okuma v 3.1-Önizleme. 1 |
|:-----|:----:|:-----:|:---:|:---:|
|Arapça | `ar`|✔ | | |
|Basitleştirilmiş Çince | `zh-Hans`|✔ | |✔ |
|Geleneksel Çince | `zh-Hant`|✔ | | |
|Çekçe | `cs` |✔ | | |
|Danca | `da` |✔ | | |
|Felemenkçe | `nl` |✔ |✔ |✔ |
|Türkçe | `en` |✔ |✔ |✔ |
|Fince | `fi` |✔ | | |
|Fransızca | `fr` |✔ |✔ |✔ |
|Almanca | `de` |✔ |✔ |✔ |
|Yunanca | `el` |✔ | | |
|Macarca | `hu` |✔ | | |
|İtalyanca | `it` |✔ |✔ |✔ |
|Japonca | `ja` |✔ | | |
|Korece | `ko` |✔ | | |
|Norveçce | `nb` |✔ | | |
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

## <a name="image-analysis"></a>Görüntü Analizi

[Analyze-Image](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API 'sindeki bazı eylemler, sorgu parametresiyle belirtilen diğer dillerde sonuç döndürebilir `language` . Diğer Eylemler, belirtilen dilin ne olduğuna bakmaksızın sonuçları Ingilizce olarak döndürür ve bazıları desteklenmeyen diller için bir özel durum oluşturur. Eylemler `visualFeatures` ve sorgu parametreleriyle birlikte belirtilir `details` ; görüntü analizine sahip yapabileceğiniz tüm eylemlerin bir listesi için bkz. [genel bakış](home.md) .

|Dil | Dil kodu | Kategoriler | Etiketler | Açıklama | Yetişkin | Markalar | Color (Renk) | Yüzler | ImageType | Nesneler | Ünlü Kişiler | Bölümler |
|:---|:---:|:----:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Çince | `zh`    | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Türkçe | `en`   | ✔ | ✔| ✔|✔|✔|✔|✔|✔|✔|✔|✔|
|Japonca | `ja`   | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Portekizce | `pt` | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|İspanyolca | `es`    | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda bahsedilen Görüntü İşleme özelliklerini kullanmaya başlayın.

* [Yerel görüntüyü çözümleme (REST)](./quickstarts/csharp-analyze.md)
* [Yazdırılan metni Ayıkla (REST)](./quickstarts/csharp-print-text.md)
