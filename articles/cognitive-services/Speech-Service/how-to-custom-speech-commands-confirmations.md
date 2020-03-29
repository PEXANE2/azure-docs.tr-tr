---
title: 'Nasıl yapılır: Özel bir komuta onay ekleme (Önizleme)'
titleSuffix: Azure Cognitive Services
description: Bu makalede, Özel Komutlar bir komut için onayları nasıl uygular.
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: encorona
ms.openlocfilehash: afa197c83b4f66f12863de4185ef7763447f3ed9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75456503"
---
# <a name="how-to-add-a-confirmation-to-a-custom-command-preview"></a>Nasıl Yapılır: Özel Komuta onay ekleme (Önizleme)

Bu makalede, bir komuta onay eklemeyi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki makalelerdeki adımları tamamlamış olmalısınız:

- [Quickstart: Özel Komut Oluşturma (Önizleme)](./quickstart-custom-speech-commands-create-new.md)
- [Quickstart: Parametreler (Önizleme) ile Özel Komut oluşturma](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-setalarm-command"></a>SetAlarm komutu oluşturma

Doğrulamaları göstermek için, kullanıcının bir alarm ayarlamasına izin veren yeni bir Komut oluşturalım.

1. [Konuşma Stüdyosu'nda](https://speech.microsoft.com/) daha önce oluşturduğunuz Özel Komutlar uygulamasını açın
1. Yeni bir Komut **SetAlarm** oluşturma
1. DateTime adlı bir parametre ekleme

   | Ayar           | Önerilen değer                                          | Açıklama                                                                                      |
   | ----------------- | ---------------------------------------------------------| ------------------------------------------------------------------------------------------------ |
   | Adı              | DateTime                                                 | Komut parametreniz için açıklayıcı bir ad                                                    |
   | Gerekli          | true                                                     | Komutu tamamlamadan önce bu parametre için bir değer gerekip gerekmediğini belirten onay kutusu |
   | Yanıt şablonu | "- Ne zaman?"                                           | Bilinmediğinde bu parametrenin değerini sormak için bir istem                              |
   | Tür              | DateTime                                                 | Sayı, Dize veya Tarih Saati gibi parametre türü                                      |
   | Tarih Varsayılanları     | Tarih eksikse bugün kullanım                             |                                                                                                  |
   | Zaman Varsayılanları     | Zaman eksikse günün başında kullanım                      |                                                                                                  | 

1. Bazı örnek cümleler ekleme
   
   ```
    set an alarm for {DateTime}
    set alarm {DateTime}
    alarm for {DateTime}
   ```

1. Sonucu onaylamak için tamamlama kuralı ekleme

   | Ayar    | Önerilen değer                                         | Açıklama                                        |
   | ---------- | ------------------------------------------------------- | -------------------------------------------------- |
   | Kural Adı  | Alarmı ayarlama                                               | Kuralın amacını açıklayan bir ad          |
   | Eylemler    | SpeechResponse - "- Tamam, alarm {DateTime} için ayarlanmıştır"       | Kural koşulu doğru olduğunda yapılacak eylem |

## <a name="try-it-out"></a>Deneyin

Test panelini seçin ve birkaç etkileşim deneyin.

- Giriş: Yarın öğlen alarmı ayarlayın
- Çıktı: "Tamam, alarm 12.06.2019 12:00:00 için ayarlanmıştır"

- Giriş: Alarm ayarlayın
- Çıktı: "Ne zaman?"
- Giriş: 17:00
- Çıkış: "Tamam, alarm 12/05/2019 17:00:00 için ayarlanmış"

## <a name="add-the-advanced-rules-for-confirmation"></a>Onay için gelişmiş kuralları ekleyin

1. Onay için gelişmiş bir kural ekleyin. 

    Bu kural, kullanıcıdan alarmın tarih ve saatini onaylamasını ister ve bir sonraki dönüş için bir onay (evet/hayır) bekleyecektir.

   | Ayar               | Önerilen değer                                                                  | Açıklama                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Kural Adı             | Tarih saatini onaylama                                                                | Kuralın amacını açıklayan bir ad          |
   | Koşullar            | Gerekli Parametre - DateTime                                                    | Kuralın ne zaman çalıştırılacakolabileceğini belirleyen koşullar    |   
   | Eylemler               | SpeechResponse - "- {DateTime} için bir alarm ayarlamak istediğinizden emin misiniz?"       | Kural koşulu doğru olduğunda yapılacak eylem |
   | Yürütme den sonra durum | Giriş için bekleyin                                                                   | Dönüşten sonra kullanıcı için durum                  |
   | Beklenti          | Onay                                                                     | Bir sonraki dönüş beklentisi                      |

1. Başarılı bir onay işlemek için gelişmiş bir kural ekleyin (kullanıcı evet dedi)

   | Ayar               | Önerilen değer                                                                  | Açıklama                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Kural Adı             | Kabul edilen onay                                                            | Kuralın amacını açıklayan bir ad          |
   | Koşullar            | Başarılı Onay & Gerekli Parametre - DateTime                           | Kuralın ne zaman çalıştırılacakolabileceğini belirleyen koşullar    |   
   | Yürütme den sonra durum | Tamamlanmaya Hazır                                                             | Dönüşten sonra kullanıcının durumu                   |

1. Reddedilen bir onayı işlemek için gelişmiş bir kural ekleme (kullanıcı hayır dedi)

   | Ayar               | Önerilen değer                                                                  | Açıklama                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Kural Adı             | Reddedildi onaylamak                                                                   | Kuralın amacını açıklayan bir ad          |
   | Koşullar            | DeniedConfirmation & Gerekli Parametre - DateTime                               | Kuralın ne zaman çalıştırılacakolabileceğini belirleyen koşullar    |   
   | Eylemler               | ClearParameter - DateTime & SpeechResponse - "- Sorun değil, ne zaman?"     | Kural koşulu doğru olduğunda yapılacak eylem |
   | Yürütme den sonra durum | Giriş için bekleyin                                                                   | Dönüşten sonra kullanıcının durumu                   |
   | Beklenti          | ElicitParametreler - DateTime                                                      | Bir sonraki dönüş beklentisi                      |

## <a name="try-it-out"></a>Deneyin

Test panelini seçin ve birkaç etkileşim deneyin.

- Giriş: Yarın öğlen alarmı ayarlayın
- Çıktı: "12/07/2019 12:00:00 için alarm kurmak istediğinizden emin misiniz?"
- Giriş: Hayır
- Çıktı: "Sorun yok, ne zaman?"
- Giriş: 17:00
- Çıktı: "12/06/2019 17:00:00 için alarm kurmak istediğinizden emin misiniz?"
- Giriş: Evet
- Çıkış: "Tamam, alarm 12/06/2019 17:00:00 için ayarlanmış"

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Nasıl Yapılır: Özel Komut'a tek adımlık düzeltme ekleme (Önizleme)](./how-to-custom-speech-commands-one-step-correction.md)