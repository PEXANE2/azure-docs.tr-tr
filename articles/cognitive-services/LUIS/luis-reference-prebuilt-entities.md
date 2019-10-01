---
title: Önceden oluşturulmuş tüm varlıklar-LUSıS
titleSuffix: Azure Cognitive Services
description: Bu makale, Language Understanding (LUO) ' de bulunan önceden oluşturulmuş varlıkların listesini içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 66cdedb402392340d38ab9a72f6f19012c359664
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71677633"
---
# <a name="entities-per-culture-in-your-luis-model"></a>LUSıS modelinizde kültür başına varlık

Language Understanding (LUSıS) önceden oluşturulmuş varlıklar sağlar. Uygulamanıza önceden oluşturulmuş bir varlık dahil edildiğinde, Lud, uç nokta yanıtında karşılık gelen varlık tahminini içerir. Tüm örnek söyleymalar da varlıkla etiketlidir. Önceden **oluşturulmuş varlıkların davranışı** değiştirilemez. Aksi belirtilmediği takdirde, önceden oluşturulmuş varlıklar tüm LUıN uygulama yerel ayarları 'nda (kültürler) kullanılabilir. Aşağıdaki tabloda her kültür için desteklenen önceden oluşturulmuş varlıklar gösterilmektedir.

|Kültür|Alt kültürler|Notlar|
|--|--|--|
|Çince|[zh-CN](#chinese-entity-support)||
|Hollanda dili|[NL-NL](#dutch-entity-support)||
|Türkçe|[en-US (Amerikan)](#english-american-entity-support)||
|Fransızca|[fr-CA (Kanada)](#french-canadian-entity-support), [fr-fr (Fransa)](#french-france-entity-support), ||
|Almanca|[de-DE](#german-entity-support)||
|İtalyanca|[BT BT](#italian-entity-support)||
|Japonca|[ja-JP](#japanese-entity-support)||
|Korece|[ko-KR](#korean-entity-support)||
|Portekizce|[PT-BR (Brezilya)](#portuguese-brazil-entity-support)||
|İspanyolca|[es-es (İspanya)](#spanish-spain-entity-support), [es-MX (Meksika)](#spanish-mexico-entity-support)||
|Türkçe|[Türkçe](#turkish-entity-support)|Türkçe 'da önceden oluşturulmuş varlık desteklenmez|

## <a name="chinese-entity-support"></a>Çince varlık desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```zh-CN``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>Başından<br>Hafta<br>Günündeki   |    ✔   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesir birimi (örn: kuruş)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>dateRange<br>time<br>timerange   |    ✔   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birimindeki<br>Alandır<br>Lık<br>bilgi (örn: bit/bayt)<br>Uzunluk (örn: ölçüm)<br>Hız (örn. mil/saat)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    -   | 
[Sayısından](luis-reference-prebuilt-number.md)   |    ✔   |  
[Numarasını](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Den](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[Kişi adı](luis-reference-prebuilt-person.md)   |    ✔   | 
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>Rankine dili<br>Delisle<br>çevirip   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="dutch-entity-support"></a>Felemenkçe varlık desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```nl-NL``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>Başından<br>Hafta<br>Günündeki   |    ✔   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesir birimi (örn: kuruş)  |    ✔   |
[Hem](luis-reference-prebuilt-deprecated.md)   |    -   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birimindeki<br>Alandır<br>Lık<br>bilgi (örn: bit/bayt)<br>Uzunluk (örn: ölçüm)<br>Hız (örn. mil/saat)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayısından](luis-reference-prebuilt-number.md)   |    ✔   |  
[Numarasını](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Den](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[Kişi adı](luis-reference-prebuilt-person.md)   |    -   | 
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>Rankine dili<br>Delisle<br>çevirip   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="english-american-entity-support"></a>İngilizce (Amerikan) varlık desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```en-US``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>Başından<br>Hafta<br>Günündeki   |    ✔   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesir birimi (örn: kuruş)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>dateRange<br>time<br>timerange   |    ✔   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birimindeki<br>Alandır<br>Lık<br>bilgi (örn: bit/bayt)<br>Uzunluk (örn: ölçüm)<br>Hız (örn. mil/saat)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    ✔   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayısından](luis-reference-prebuilt-number.md)   |    ✔   |  
[Numarasını](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    ✔   |
[Den](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[Kişi adı](luis-reference-prebuilt-person.md)   |    ✔   | 
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>Rankine dili<br>Delisle<br>çevirip   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="french-france-entity-support"></a>Fransızca (Fransa) varlık desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```fr-FR``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>Başından<br>Hafta<br>Günündeki   |    ✔   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesir birimi (örn: kuruş)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>dateRange<br>time<br>timerange   |    ✔   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birimindeki<br>Alandır<br>Lık<br>bilgi (örn: bit/bayt)<br>Uzunluk (örn: ölçüm)<br>Hız (örn. mil/saat)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayısından](luis-reference-prebuilt-number.md)   |    ✔   |  
[Numarasını](luis-reference-prebuilt-ordinal.md)   |    ✔   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |  
[Den](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[Kişi adı](luis-reference-prebuilt-person.md)   |   -   | 
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>Rankine dili<br>Delisle<br>çevirip   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="french-canadian-entity-support"></a>Fransızca (Kanada) varlık desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```fr-CA``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>Başından<br>Hafta<br>Günündeki   |    ✔   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesir birimi (örn: kuruş)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>dateRange<br>time<br>timerange   |    ✔   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birimindeki<br>Alandır<br>Lık<br>bilgi (örn: bit/bayt)<br>Uzunluk (örn: ölçüm)<br>Hız (örn. mil/saat)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayısından](luis-reference-prebuilt-number.md)   |    ✔   |  
[Numarasını](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Den](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[Kişi adı](luis-reference-prebuilt-person.md)   |    -   | 
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>Rankine dili<br>Delisle<br>çevirip   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="german-entity-support"></a>Almanya varlık desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```de-DE``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>Başından<br>Hafta<br>Günündeki   |    ✔   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesir birimi (örn: kuruş)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>dateRange<br>time<br>timerange   |    ✔   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birimindeki<br>Alandır<br>Lık<br>bilgi (örn: bit/bayt)<br>Uzunluk (örn: ölçüm)<br>Hız (örn. mil/saat)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayısından](luis-reference-prebuilt-number.md)   |    ✔   |  
[Numarasını](luis-reference-prebuilt-ordinal.md)   |    ✔   |
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |  
[Den](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[Kişi adı](luis-reference-prebuilt-person.md)   |    -   | 
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>Rankine dili<br>Delisle<br>çevirip   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="italian-entity-support"></a>İtalyanca varlık desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```it-IT``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>Başından<br>Hafta<br>Günündeki   |    ✔   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesir birimi (örn: kuruş)  |    ✔   |
[Hem](luis-reference-prebuilt-deprecated.md)   |    -   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birimindeki<br>Alandır<br>Lık<br>bilgi (örn: bit/bayt)<br>Uzunluk (örn: ölçüm)<br>Hız (örn. mil/saat)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayısından](luis-reference-prebuilt-number.md)   |    ✔   |  
[Numarasını](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Den](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[Kişi adı](luis-reference-prebuilt-person.md)   |    -   | 
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>Rankine dili<br>Delisle<br>çevirip   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="japanese-entity-support"></a>Japonca varlık desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```ja-JP``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>Başından<br>Hafta<br>Günündeki   |    ✔   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesir birimi (örn: kuruş)  |    ✔   |
[Hem](luis-reference-prebuilt-deprecated.md)   |    -   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birimindeki<br>Alandır<br>Lık<br>bilgi (örn: bit/bayt)<br>Uzunluk (örn: ölçüm)<br>Hız (örn. mil/saat)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayısından](luis-reference-prebuilt-number.md)   |    ✔   |  
[Numarasını](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Den](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[Kişi adı](luis-reference-prebuilt-person.md)   |    -   | 
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>Rankine dili<br>Delisle<br>çevirip   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="korean-entity-support"></a>Korece varlık desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```ko-KR``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>Başından<br>Hafta<br>Günündeki   |    -   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesir birimi (örn: kuruş)  |    -   |
[Hem](luis-reference-prebuilt-deprecated.md)   |    -   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birimindeki<br>Alandır<br>Lık<br>bilgi (örn: bit/bayt)<br>Uzunluk (örn: ölçüm)<br>Hız (örn. mil/saat)  |    -   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayısından](luis-reference-prebuilt-number.md)   |    -   |  
[Numarasını](luis-reference-prebuilt-ordinal.md)   |    -   |  
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Den](luis-reference-prebuilt-percentage.md)   |    -   | 
[Kişi adı](luis-reference-prebuilt-person.md)   |    -   | 
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>Rankine dili<br>Delisle<br>çevirip   |    -   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="portuguese-brazil-entity-support"></a>Portekizce (Brezilya) varlık desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```pt-BR``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>Başından<br>Hafta<br>Günündeki   |    ✔   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesir birimi (örn: kuruş)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>dateRange<br>time<br>timerange   |    ✔   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birimindeki<br>Alandır<br>Lık<br>bilgi (örn: bit/bayt)<br>Uzunluk (örn: ölçüm)<br>Hız (örn. mil/saat)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayısından](luis-reference-prebuilt-number.md)   |    ✔   |  
[Numarasını](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Den](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[Kişi adı](luis-reference-prebuilt-person.md)   |    -   | 
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>Rankine dili<br>Delisle<br>çevirip   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="spanish-spain-entity-support"></a>İspanyolca (Ispanya) varlık desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```es-ES``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>Başından<br>Hafta<br>Günündeki   |    ✔   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesir birimi (örn: kuruş)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>dateRange<br>time<br>timerange   |    ✔   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birimindeki<br>Alandır<br>Lık<br>bilgi (örn: bit/bayt)<br>Uzunluk (örn: ölçüm)<br>Hız (örn. mil/saat)  |    ✔   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayısından](luis-reference-prebuilt-number.md)   |    ✔   |  
[Numarasını](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Den](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[Kişi adı](luis-reference-prebuilt-person.md)   |    -   | 
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>Rankine dili<br>Delisle<br>çevirip   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="spanish-mexico-entity-support"></a>İspanyolca (Meksika) varlık desteği

Aşağıdaki varlıklar desteklenir:

|Önceden oluşturulmuş varlık|```es-MX``` |
------|:------:|
[Yaş](luis-reference-prebuilt-age.md):<br>yıl<br>Başından<br>Hafta<br>Günündeki   |    -   |
[Para birimi (para)](luis-reference-prebuilt-currency.md):<br>Doları<br>kesir birimi (örn: kuruş)  |    -   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>dateRange<br>time<br>timerange   |    -   | 
[Boyut](luis-reference-prebuilt-dimension.md):<br>birimindeki<br>Alandır<br>Lık<br>bilgi (örn: bit/bayt)<br>Uzunluk (örn: ölçüm)<br>Hız (örn. mil/saat)  |    -   | 
[E-posta](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Sayısından](luis-reference-prebuilt-number.md)   |    ✔   |  
[Numarasını](luis-reference-prebuilt-ordinal.md)   |    -   |  
[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md)   |    -   |
[Den](luis-reference-prebuilt-percentage.md)   |    -   | 
[Kişi adı](luis-reference-prebuilt-person.md)   |    -   | 
[PhoneNumber](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Sıcaklık](luis-reference-prebuilt-temperature.md):<br>Fahrenhayt<br>Kelvin<br>Rankine dili<br>Delisle<br>çevirip   |    -   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

[Kullanım dışı bırakılan varlıklarda](luis-reference-prebuilt-deprecated.md) notlara bakın

KeyPhrase, Portekizce (Brezilya)-```pt-BR``` olan tüm alt kültürlerde kullanılamaz.

## <a name="turkish-entity-support"></a>Türkçe varlık desteği

**Türkçe sürümünde desteklenen önceden oluşturulmuş varlık yok.** 

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

## <a name="contribute-to-prebuilt-entity-cultures"></a>Önceden oluşturulmuş varlık külbilgilerine katkıda bulunun
Önceden oluşturulmuş varlıklar, Tanıyıcılar-metin açık kaynaklı projede geliştirilir. Projeye [katkıda bulunun](https://github.com/Microsoft/Recognizers-Text) . Bu proje, kültür başına para birimi örnekleri içerir. 

GeographyV2 ve PersonName, Tanıyıcılar-metin projesine dahil değildir. Önceden oluşturulmuş bu varlıklarla ilgili sorunlar için lütfen bir [destek isteği](../../azure-supportability/how-to-create-azure-support-request.md)açın. 

## <a name="next-steps"></a>Sonraki adımlar

[Sayı](luis-reference-prebuilt-number.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md)ve [para birimi](luis-reference-prebuilt-currency.md) varlıkları hakkında bilgi edinin. 
