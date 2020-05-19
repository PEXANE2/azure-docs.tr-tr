---
title: Dil desteği-Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Bu makale, Görüntü İşleme özellikleri tarafından desteklenen doğal dillerin bir listesini sağlar; OCR, Metin Tanıma ve okuma.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.openlocfilehash: a207118af0b07be79c934a9665f47e73c5eecde0
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83589202"
---
# <a name="language-support-for-computer-vision"></a>Görüntü İşleme için dil desteği

Görüntü İşleme bazı özellikleri birden çok dili destekler; Burada bahsedilen özellikler yalnızca Ingilizce 'yi destekler.

## <a name="text-recognition"></a>Metin tanıma

Görüntü İşleme, metni birçok dilde tanıyabilir. OCR ve okuma API 'Leri için bir dil kodu belirtmeniz gerekmez. Bu işlevsellik ve her bir API 'nin avantajları hakkında daha fazla bilgi için bkz. [basılı ve el yazısı metnini tanıma](concept-recognizing-text.md) .

|Dil| Dil kodu | OCR APı 'SI | API 'YI oku |
|:-----|:----:|:-----:|:---:|
|Arapça | `ar`|✔ | |
|Basitleştirilmiş Çince | `zh-Hans`|✔ | |
|Geleneksel Çince | `zh-Hant`|✔ | |
|Çekçe | `cs` |✔ | |
|Danca | `da` |✔ | |
|Felemenkçe | `nl` |✔ |✔ |
|İngilizce | `en` |✔ |✔ |
|Fince | `fi` |✔ | |
|Fransızca | `fr` |✔ |✔ |
|Almanca | `de` |✔ |✔ |
|Yunanca | `el` |✔ | |
|Macarca | `hu` |✔ | |
|İtalyanca | `it` |✔ |✔ |
|Japonca | `ja` |✔ | |
|Korece | `ko` |✔ | |
|Norveççe | `nb` |✔ | |
|Lehçe | `pl` |✔ | |
|Portekizce | `pt` |✔ |✔ |
|Rumence | `ro` |✔ | |
|Rusça | `ru` |✔ | |
|Sırpça (Kiril) | `sr-Cyrl` |✔ | |
|Sırpça (Latin) | `sr-Latn` |✔ | |
|Slovakça | `sk` |✔ | |
|İspanyolca | `es` |✔ |✔ |
|İsveççe | `sw` |✔ | |
|Türkçe | `tr` |✔ | |

## <a name="image-analysis"></a>Görüntü Analizi

[Analyze-Image](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API 'sindeki bazı eylemler, sorgu parametresiyle belirtilen diğer dillerde sonuç döndürebilir `language` . Diğer Eylemler, belirtilen dilin ne olduğuna bakmaksızın sonuçları Ingilizce olarak döndürür ve bazıları desteklenmeyen diller için bir özel durum oluşturur. Eylemler `visualFeatures` ve sorgu parametreleriyle birlikte belirtilir `details` ; görüntü analizine sahip yapabileceğiniz tüm eylemlerin bir listesi için bkz. [genel bakış](home.md) .

|Dil | Dil kodu | Kategoriler | Etiketler | Açıklama | Yetişkin | Markalar | Renk | Yüzler | ImageType | Nesneler | Ünlüler | Yer işaretleri |
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
