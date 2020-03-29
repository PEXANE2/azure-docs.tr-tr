---
title: Return N-Best Çeviriler - Çevirmen Metin
titleSuffix: Azure Cognitive Services
description: Çevirmen Metin API'sini kullanarak N-En İyi çevirileri döndürün.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: swmachan
ROBOTS: NOINDEX
ms.openlocfilehash: eff25877165ac365e0af77651147fcdd1eebe294
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73837252"
---
# <a name="how-to-return-n-best-translations"></a>N-Best çevirileri nasıl döndürüler?

> [!NOTE]
> Bu yöntem amortismana hazırdır. Çevirmen Metin API'sinin V3.0'ında kullanılamaz.

Microsoft Translator API'nin GetTranslations() ve GetTranslationsArray() yöntemleri isteğe bağlı Boolean bayrağı "IncludeMultipleMTAlternatives" içerir.
Yöntem, çevirmen motorunun N-Best listesinden deltanın sağlandığı maxTranslations alternatiflerine geri dönecektir.

İmza:

**Sözdizimi**

| C# |
|:---|
| GetTranslationsResponse Microsoft.Translator.GetTranslations(appId, metin, itibaren, maxTranslations, seçenekler); |

**Parametreler**

| Parametre | Açıklama |
|:---|:---|
| appId | **Gerekli** Yetkilendirme üstbilgisi kullanılırsa, appid alanını boş bırakın, "Taşıyıcı" + " + erişim belirteci içeren bir dize belirtin.|
| metin | **Gerekli** Çevrilecek metni temsil eden bir dize. Metnin boyutu 10000 karakteri geçmemelidir.|
| Kaynak | **Gerekli** Çevrilecek metnin dil kodunu temsil eden bir dize. |
| - | **Gerekli** Metni çevirmek için dil kodunu temsil eden bir dize. |
| maxÇeviriler | **Gerekli** Döndürülecek en fazla çeviri sayısını temsil eden bir int. |
| seçenekler | **İsteğe bağlı** Aşağıda listelenen değerleri içeren bir TranslateOptions nesnesi. Bunların tümü isteğe bağlı ve varsayılan en yaygın ayarları.

* Kategori: Desteklenen tek seçenek ve varsayılan seçenek "genel"dir.
* ContentType: Desteklenen tek seçenek ve varsayılan seçenek "metin/düz" dir.
* Durum: İstek ve yanıtı ilişkilendirmeye yardımcı olacak kullanıcı durumu. Aynı içerik yanıtta döndürülür.
* MultipleMTAlternatives'i dahil edin: MT motorundan birden fazla alternatif inip döndürülmeyeceğini belirlemek için bayrak. Varsayılan yanlıştır ve yalnızca 1 alternatif içerir.

## <a name="ratings"></a>Derecelendirmeler
Derecelendirmeler aşağıdaki gibi uygulanır: En iyi otomatik çevirinin 5 derecelendirmesi vardır.
Otomatik olarak oluşturulan (N-Best) çeviri alternatifleri 0 derecelendirmeye ve 100 eşleşme derecesine sahiptir.

## <a name="number-of-alternatives"></a>Alternatif sayısı
Döndürülen alternatiflerin sayısı maksimum Çevirilere kadardır, ancak daha az olabilir.

## <a name="language-pairs"></a>Dil çiftleri
Bu işlevsellik, basitleştirilmiş ve Geleneksel Çince arasındaki çeviriler için kullanılamaz, her iki yönde. Diğer tüm Microsoft Translator destekli dil çiftleri için kullanılabilir.
