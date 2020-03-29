---
title: Tüm Önceden Oluşturulmuş Varlıklar - LUIS
titleSuffix: Azure Cognitive Services
description: Bu makalede, Dil Anlama (LUIS) dahil olan önceden oluşturulmuş varlıkların listelerini içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/03/2019
ms.author: diberry
ms.openlocfilehash: a49452653f8ac4bcc62758d801a235be0929d314
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219725"
---
# <a name="entities-per-culture-in-your-luis-model"></a>LUIS modelinizde kültür başına varlıklar

Dil Anlama (LUIS) önceden oluşturulmuş varlıklar sağlar. Önceden oluşturulmuş bir varlık uygulamanıza dahil edildiğinde, LUIS bitiş noktası yanıtında karşılık gelen varlık tahminini içerir. Tüm örnek söyleyişler de varlıkla etiketlenir. Önceden oluşturulmuş varlıkların davranışı **değiştirilemez.** Aksi belirtilmedikçe, önceden oluşturulmuş varlıklar tüm LUIS uygulama yerel bölgelerinde (kültürlerde) kullanılabilir. Aşağıdaki tablo, her kültür için desteklenen önceden oluşturulmuş varlıkları gösterir.

|Kültür|Alt kültürler|Notlar|
|--|--|--|
|Çince|[zh-CN](#chinese-entity-support)||
|Felemenkçe|[nl-NL](#dutch-entity-support)||
|Türkçe|[tr-US (Amerikan)](#english-american-entity-support)||
|Fransızca|[fr-CA (Kanada)](#french-canadian-entity-support), [fr-FR (Fransa)](#french-france-entity-support), ||
|Almanca|[de-DE](#german-entity-support)||
|İtalyanca|[it-BT](#italian-entity-support)||
|Japonca|[ja-JP](#japanese-entity-support)||
|Korece|[ko-KR](#korean-entity-support)||
|Portekizce|[pt-BR (Brezilya)](#portuguese-brazil-entity-support)||
|İspanyolca|[es-ES (İspanya)](#spanish-spain-entity-support), [es-MX (Meksika)](#spanish-mexico-entity-support)||
|Türkçe|[Türkçe](#turkish-entity-support)|Türkçe'de önceden inşa edilmiş varlıklar desteklenmez|

## <a name="prediction-endpoint-runtime"></a>Tahmin bitiş noktası çalışma süresi

Önceden oluşturulmuş bir varlığın belirli bir dilde kullanılabilirliği, tahmin bitiş noktası çalışma zamanı sürümüyle belirlenir.

## <a name="chinese-entity-support"></a>Çin taraf desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```zh-CN``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    V2, V3   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesirli birim (ör. kuruş)  |    V2, V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>tarih aralığı<br>time<br>zaman aralığı   |    V2, V3   |
[Boyut](luis-reference-prebuilt-dimension.md):<br>Ses düzeyi<br>alan<br>Ağırlık<br>bilgi (ör. bit/bayt)<br>uzunluk (ör. metre)<br>hız (ör. saat başına mil)  |    V2, V3   |
[E-posta](luis-reference-prebuilt-email.md)   |    V2, V3   |
[CoğrafyaV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[Keyphrase](luis-reference-prebuilt-keyphrase.md)   |    -   |
[Numarası](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Sıralı](luis-reference-prebuilt-ordinal.md)   |    V2, V3   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Yüzde](luis-reference-prebuilt-percentage.md)   |    V2, V3   |
[Kişi Adı](luis-reference-prebuilt-person.md)   |    V2, V3   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>kelvin<br>rankine<br>şarküteri<br>Santigrat   |    V2, V3   |
[URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="dutch-entity-support"></a>Hollanda taraf desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```nl-NL``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    V2, V3   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesirli birim (ör. kuruş)  |    V2, V3   |
[Datetime](luis-reference-prebuilt-deprecated.md)   |    -   |
[Boyut](luis-reference-prebuilt-dimension.md):<br>Ses düzeyi<br>alan<br>Ağırlık<br>bilgi (ör. bit/bayt)<br>uzunluk (ör. metre)<br>hız (ör. saat başına mil)  |    V2, V3   |
[E-posta](luis-reference-prebuilt-email.md)   |    V2, V3   |
[CoğrafyaV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[Keyphrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Numarası](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Sıralı](luis-reference-prebuilt-ordinal.md)   |    V2, V3   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Yüzde](luis-reference-prebuilt-percentage.md)   |    V2, V3   |
[Kişi Adı](luis-reference-prebuilt-person.md)   |    -   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>kelvin<br>rankine<br>şarküteri<br>Santigrat   |    V2, V3   |
[URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="english-american-entity-support"></a>İngilizce (American) varlık desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```en-US``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    V2, V3   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesirli birim (ör. kuruş)  |    V2, V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>tarih aralığı<br>time<br>zaman aralığı   |    V2, V3   |
[Boyut](luis-reference-prebuilt-dimension.md):<br>Ses düzeyi<br>alan<br>Ağırlık<br>bilgi (ör. bit/bayt)<br>uzunluk (ör. metre)<br>hız (ör. saat başına mil)  |    V2, V3   |
[E-posta](luis-reference-prebuilt-email.md)   |    V2, V3   |
[CoğrafyaV2](luis-reference-prebuilt-geographyV2.md)   |    V2, V3   |
[Keyphrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Numarası](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Sıralı](luis-reference-prebuilt-ordinal.md)   |    V2, V3   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    V2, V3   |
[Yüzde](luis-reference-prebuilt-percentage.md)   |    V2, V3   |
[Kişi Adı](luis-reference-prebuilt-person.md)   |    V2, V3   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>kelvin<br>rankine<br>şarküteri<br>Santigrat   |    V2, V3   |
[URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="french-france-entity-support"></a>Fransız (Fransa) taraf desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```fr-FR``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    V2, V3   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesirli birim (ör. kuruş)  |    V2, V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>tarih aralığı<br>time<br>zaman aralığı   |    V2, V3   |
[Boyut](luis-reference-prebuilt-dimension.md):<br>Ses düzeyi<br>alan<br>Ağırlık<br>bilgi (ör. bit/bayt)<br>uzunluk (ör. metre)<br>hız (ör. saat başına mil)  |    V2, V3   |
[E-posta](luis-reference-prebuilt-email.md)   |    V2, V3   |
[CoğrafyaV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[Keyphrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Numarası](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Sıralı](luis-reference-prebuilt-ordinal.md)   |    V2, V3   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Yüzde](luis-reference-prebuilt-percentage.md)   |    V2, V3   |
[Kişi Adı](luis-reference-prebuilt-person.md)   |   -   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>kelvin<br>rankine<br>şarküteri<br>Santigrat   |    V2, V3   |
[URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="french-canadian-entity-support"></a>Fransızca (Kanada) varlık desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```fr-CA``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    V2, V3   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesirli birim (ör. kuruş)  |    V2, V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>tarih aralığı<br>time<br>zaman aralığı   |    V2, V3   |
[Boyut](luis-reference-prebuilt-dimension.md):<br>Ses düzeyi<br>alan<br>Ağırlık<br>bilgi (ör. bit/bayt)<br>uzunluk (ör. metre)<br>hız (ör. saat başına mil)  |    V2, V3   |
[E-posta](luis-reference-prebuilt-email.md)   |    V2, V3   |
[CoğrafyaV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[Keyphrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Numarası](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Sıralı](luis-reference-prebuilt-ordinal.md)   |    V2, V3   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Yüzde](luis-reference-prebuilt-percentage.md)   |    V2, V3   |
[Kişi Adı](luis-reference-prebuilt-person.md)   |    -   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>kelvin<br>rankine<br>şarküteri<br>Santigrat   |    V2, V3   |
[URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="german-entity-support"></a>Alman taraf desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```de-DE``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    V2, V3   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesirli birim (ör. kuruş)  |    V2, V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>tarih aralığı<br>time<br>zaman aralığı   |    V2, V3   |
[Boyut](luis-reference-prebuilt-dimension.md):<br>Ses düzeyi<br>alan<br>Ağırlık<br>bilgi (ör. bit/bayt)<br>uzunluk (ör. metre)<br>hız (ör. saat başına mil)  |    V2, V3   |
[E-posta](luis-reference-prebuilt-email.md)   |    V2, V3   |
[CoğrafyaV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[Keyphrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Numarası](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Sıralı](luis-reference-prebuilt-ordinal.md)   |    V2, V3   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Yüzde](luis-reference-prebuilt-percentage.md)   |    V2, V3   |
[Kişi Adı](luis-reference-prebuilt-person.md)   |    -   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>kelvin<br>rankine<br>şarküteri<br>Santigrat   |    V2, V3   |
[URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="italian-entity-support"></a>İtalyan taraf desteği

İtalyan önceden oluşturulmuş yaş, para birimi, boyut, sayı, yüzde _çözünürlüğü_ V2 ve V3 önizleme değiştirildi.

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```it-IT``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    V2, V3   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesirli birim (ör. kuruş)  |    V2, V3   |
[Datetime](luis-reference-prebuilt-deprecated.md)   |    -   |
[Boyut](luis-reference-prebuilt-dimension.md):<br>Ses düzeyi<br>alan<br>Ağırlık<br>bilgi (ör. bit/bayt)<br>uzunluk (ör. metre)<br>hız (ör. saat başına mil)  |    V2, V3   |
[E-posta](luis-reference-prebuilt-email.md)   |    V2, V3   |
[CoğrafyaV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[Keyphrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Numarası](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Sıralı](luis-reference-prebuilt-ordinal.md)   |    V2, V3   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Yüzde](luis-reference-prebuilt-percentage.md)   |    V2, V3   |
[Kişi Adı](luis-reference-prebuilt-person.md)   |    -   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>kelvin<br>rankine<br>şarküteri<br>Santigrat   |    V2, V3   |
[URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="japanese-entity-support"></a>Japon varlık desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```ja-JP``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    V2, -   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesirli birim (ör. kuruş)  |    V2, -   |
[Datetime](luis-reference-prebuilt-deprecated.md)   |    -   |
[Boyut](luis-reference-prebuilt-dimension.md):<br>Ses düzeyi<br>alan<br>Ağırlık<br>bilgi (ör. bit/bayt)<br>uzunluk (ör. metre)<br>hız (ör. saat başına mil)  |    V2, -   |
[E-posta](luis-reference-prebuilt-email.md)   |    V2, V3   |
[CoğrafyaV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[Keyphrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Numarası](luis-reference-prebuilt-number.md)   |    V2, -   |
[Sıralı](luis-reference-prebuilt-ordinal.md)   |    V2, -   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Yüzde](luis-reference-prebuilt-percentage.md)   |    V2, -   |
[Kişi Adı](luis-reference-prebuilt-person.md)   |    -   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>kelvin<br>rankine<br>şarküteri<br>Santigrat   |    V2, -   |
[URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="korean-entity-support"></a>Kore varlık desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```ko-KR``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    -   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesirli birim (ör. kuruş)  |    -   |
[Datetime](luis-reference-prebuilt-deprecated.md)   |    -   |
[Boyut](luis-reference-prebuilt-dimension.md):<br>Ses düzeyi<br>alan<br>Ağırlık<br>bilgi (ör. bit/bayt)<br>uzunluk (ör. metre)<br>hız (ör. saat başına mil)  |    -   |
[E-posta](luis-reference-prebuilt-email.md)   |    V2, V3   |
[CoğrafyaV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[Keyphrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Numarası](luis-reference-prebuilt-number.md)   |    -   |
[Sıralı](luis-reference-prebuilt-ordinal.md)   |    -   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Yüzde](luis-reference-prebuilt-percentage.md)   |    -   |
[Kişi Adı](luis-reference-prebuilt-person.md)   |    -   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>kelvin<br>rankine<br>şarküteri<br>Santigrat   |    -   |
[URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="portuguese-brazil-entity-support"></a>Portekizce (Brezilya) taraf desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```pt-BR``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    V2, V3   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesirli birim (ör. kuruş)  |    V2, V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>tarih aralığı<br>time<br>zaman aralığı   |    V2, V3   |
[Boyut](luis-reference-prebuilt-dimension.md):<br>Ses düzeyi<br>alan<br>Ağırlık<br>bilgi (ör. bit/bayt)<br>uzunluk (ör. metre)<br>hız (ör. saat başına mil)  |    V2, V3   |
[E-posta](luis-reference-prebuilt-email.md)   |    V2, V3   |
[CoğrafyaV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[Keyphrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Numarası](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Sıralı](luis-reference-prebuilt-ordinal.md)   |    V2, V3   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Yüzde](luis-reference-prebuilt-percentage.md)   |    V2, V3   |
[Kişi Adı](luis-reference-prebuilt-person.md)   |    -   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>kelvin<br>rankine<br>şarküteri<br>Santigrat   |    V2, V3   |
[URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="spanish-spain-entity-support"></a>İspanyolca (İspanya) taraf desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```es-ES``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    V2, V3   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesirli birim (ör. kuruş)  |    V2, V3   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>tarih aralığı<br>time<br>zaman aralığı   |    V2, V3   |
[Boyut](luis-reference-prebuilt-dimension.md):<br>Ses düzeyi<br>alan<br>Ağırlık<br>bilgi (ör. bit/bayt)<br>uzunluk (ör. metre)<br>hız (ör. saat başına mil)  |    V2, V3   |
[E-posta](luis-reference-prebuilt-email.md)   |    V2, V3   |
[CoğrafyaV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[Keyphrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Numarası](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Sıralı](luis-reference-prebuilt-ordinal.md)   |    V2, V3   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Yüzde](luis-reference-prebuilt-percentage.md)   |    V2, V3   |
[Kişi Adı](luis-reference-prebuilt-person.md)   |    -   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>kelvin<br>rankine<br>şarküteri<br>Santigrat   |    V2, V3   |
[URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

## <a name="spanish-mexico-entity-support"></a>İspanyolca (Meksika) varlık desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```es-MX``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>ay<br>hafta<br>gün   |    -   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesirli birim (ör. kuruş)  |    -   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>tarih aralığı<br>time<br>zaman aralığı   |    -   |
[Boyut](luis-reference-prebuilt-dimension.md):<br>Ses düzeyi<br>alan<br>Ağırlık<br>bilgi (ör. bit/bayt)<br>uzunluk (ör. metre)<br>hız (ör. saat başına mil)  |    -   |
[E-posta](luis-reference-prebuilt-email.md)   |    V2, V3   |
[CoğrafyaV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[Keyphrase](luis-reference-prebuilt-keyphrase.md)   |    V2, V3   |
[Numarası](luis-reference-prebuilt-number.md)   |    V2, V3   |
[Sıralı](luis-reference-prebuilt-ordinal.md)   |    -   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Yüzde](luis-reference-prebuilt-percentage.md)   |    -   |
[Kişi Adı](luis-reference-prebuilt-person.md)   |    -   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    V2, V3   |
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>kelvin<br>rankine<br>şarküteri<br>Santigrat   |    -   |
[URL](luis-reference-prebuilt-url.md)   |    V2, V3   |

[Amortismana dayalı önceden oluşturulmuş varlıklar](luis-reference-prebuilt-deprecated.md) hakkındaki notları görme

KeyPhrase Portekizce (Brezilya) tüm alt kültürleri kullanılamaz - ```pt-BR```.

## <a name="turkish-entity-support"></a>Türk taraf desteği

**Türkçe'de önceden desteklenen bir varlık yoktur.**

<!--

|Prebuilt entity|```tr-tr``` |
------|:------:|
[Age](luis-reference-prebuilt-age.md):<br>year<br>month<br>week<br>day   |    -   |
[Currency (money)](luis-reference-prebuilt-currency.md):<br>dollar<br>fractional unit (ex: penny)  |    -   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>daterange<br>time<br>timerange   |    -   |
[Dimension](luis-reference-prebuilt-dimension.md):<br>volume<br>area<br>weight<br>information (ex: bit/byte)<br>length (ex: meter)<br>speed (ex: mile per hour)  |    -   |
[Email](luis-reference-prebuilt-email.md)   |    -   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    -   |
[Number](luis-reference-prebuilt-number.md)   |    -   |
[Ordinal](luis-reference-prebuilt-ordinal.md)   |    -   |
[Percentage](luis-reference-prebuilt-percentage.md)   |    -   |
[PersonName](luis-reference-prebuilt-person.md)   |    -   |
[Phonenumber](luis-reference-prebuilt-phonenumber.md)   |    -   |
[Temperature](luis-reference-prebuilt-temperature.md):<br>fahrenheit<br>kelvin<br>rankine<br>delisle<br>celsius   |    -   |
[URL](luis-reference-prebuilt-url.md)   |    -   |

See notes on [Deprecated prebuilt entities](luis-reference-prebuilt-deprecated.md)


KeyPhrase is not available.
-->

## <a name="contribute-to-prebuilt-entity-cultures"></a>Önceden oluşturulmuş varlık kültürlerine katkıda bulunmak
Önceden oluşturulmuş varlıklar Tanıyanlar-Metin açık kaynak projesinde geliştirilmiştir. Projeye [katkıda bulunun.](https://github.com/Microsoft/Recognizers-Text) Bu proje, kültür başına para birimi örneklerini içerir.

GeographyV2 ve PersonName Tanıyanlar-Metin projesine dahil değildir. Bu önceden oluşturulmuş varlıklarla ilgili sorunlar için lütfen bir [destek isteği](../../azure-portal/supportability/how-to-create-azure-support-request.md)açın.

## <a name="next-steps"></a>Sonraki adımlar

[Sayı,](luis-reference-prebuilt-number.md) [datetimeV2](luis-reference-prebuilt-datetimev2.md)ve [para birimi](luis-reference-prebuilt-currency.md) varlıkları hakkında bilgi edinin.
