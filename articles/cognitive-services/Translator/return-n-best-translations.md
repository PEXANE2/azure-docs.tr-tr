---
title: N-En Iyi çevirileri Döndür-Translator Metin Çevirisi API'si
titleSuffix: Azure Cognitive Services
description: Microsoft Translator Metin Çevirisi API'si kullanarak N-En Iyi çevirileri döndürün.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: swmachan
ms.openlocfilehash: 7aec6bb81f3ca99ead744767668c2e4f1d1d4d53
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68594891"
---
# <a name="how-to-return-n-best-translations"></a>N-En Iyi çevirileri döndürme

> [!NOTE]
> Bu yöntem kullanım dışıdır. Translator Metin Çevirisi API'si V 3.0 sürümünde kullanılamaz.

Microsoft Translator API 'sinin Getçeviriler () ve GetTranslationsArray () yöntemleri, "ıncludemultipkıtalternatifler" isteğe bağlı Boole bayrağını içerir.
Yöntemi, çevirmen altyapısının N En Iyi listesinden Delta 'un sağlandığı en fazla Maxçevirilerin alternatifleri döndürür.

İmza:

**Söz dizimi**

| C# |
|:---|
| GetTranslationsResponse Microsoft. Translator. Getçevirileri (AppID, metin, from, to, Maxçeviriler, Seçenekler); |

**Parametreler**

| Parametre | Açıklama |
|:---|:---|
| appId | **Gerekli** Yetkilendirme üst bilgisi kullanılırsa, AppID alanını boş bırakın başka bir "taşıyıcı" + "" + erişim belirteci içeren bir dize belirtin.|
| text | **Gerekli** Çevrilecek metni temsil eden bir dize. Metnin boyutu 10000 karakteri aşmamalıdır.|
| from | **Gerekli** Çevrilecek metnin dil kodunu temsil eden bir dize. |
| to | **Gerekli** Metnin çevrilmesi için dil kodunu temsil eden bir dize. |
| Maxçeviriler | **Gerekli** Döndürülecek en fazla çeviri sayısını temsil eden bir int. |
| options | **Isteğe bağlı** Aşağıda listelenen değerleri içeren bir TranslateOptions nesnesi. Bunların tümü isteğe bağlıdır ve varsayılan olarak en yaygın ayarlara ayarlanır.

* Alan Yalnızca "genel" seçeneği desteklenir ve varsayılan seçenektir.
* ContentType Desteklenen tek ve varsayılan, "metin/düz" seçeneğidir.
* Durum: İstek ve yanıtı ilişkilendirmek için Kullanıcı durumu. Yanıtta aynı içerik döndürülecek.
* Includemultiplimon Talternatifleri: MT altyapısından birden fazla alternatifin döndürülüp döndürülmeyeceğini belirleme bayrağı. Varsayılan değer false 'dur ve yalnızca 1 alternatif içerir.

## <a name="ratings"></a>Lendir
Derecelendirmeler aşağıdaki gibi uygulanır: En iyi otomatik çeviri, 5 derecesine sahiptir.
Otomatik olarak oluşturulan (N-En Iyi) çeviri alternatifleri 0 derecelendirmesine sahiptir ve 100 eşleşme derecesine sahiptir.

## <a name="number-of-alternatives"></a>Alternatifin sayısı
Döndürülen alternatiflere göre Maxçevirilerin sayısı, ancak daha az olabilir.

## <a name="language-pairs"></a>Dil çiftleri
Bu işlev, Basitleştirilmiş ve geleneksel Çince arasında her iki yönde de çeviri için kullanılamaz. Diğer tüm Microsoft Translator desteklenen dil çiftleri için kullanılabilir.
