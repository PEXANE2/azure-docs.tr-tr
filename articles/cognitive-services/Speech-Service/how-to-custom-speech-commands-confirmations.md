---
title: 'Nasıl yapılır: özel komuta onay ekleme (Önizleme)'
titleSuffix: Azure Cognitive Services
description: Bu makalede, özel komutlarda bir komut için onayları nasıl uygularsa.
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: encorona
ms.openlocfilehash: afa197c83b4f66f12863de4185ef7763447f3ed9
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75456503"
---
# <a name="how-to-add-a-confirmation-to-a-custom-command-preview"></a>Nasıl yapılır: özel komuta onay ekleme (Önizleme)

Bu makalede, komuta nasıl onay ekleneceğini öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki makalelerde bulunan adımları tamamlamış olmanız gerekir:

- [Hızlı başlangıç: özel komut oluşturma (Önizleme)](./quickstart-custom-speech-commands-create-new.md)
- [Hızlı başlangıç: parametrelerle özel komut oluşturma (Önizleme)](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-setalarm-command"></a>SetAlarm komutu oluşturma

Doğrulamaları göstermek için, kullanıcının bir alarm ayarlamaya izin veren yeni bir komut oluşturalım.

1. Daha önce oluşturduğunuz özel komutlar uygulamanızı [konuşma Studio](https://speech.microsoft.com/) 'da açın
1. Yeni bir komut **SetAlarm** oluşturma
1. DateTime adlı bir parametre ekleyin

   | Ayar           | Önerilen değer                                          | Açıklama                                                                                      |
   | ----------------- | ---------------------------------------------------------| ------------------------------------------------------------------------------------------------ |
   | Ad              | Tarih Saat                                                 | Komut parametrunuz için açıklayıcı bir ad                                                    |
   | Gereklidir          | doğru                                                     | Komutu tamamlamadan önce Bu parametre için bir değer gerekip gerekmediğini belirten onay kutusu |
   | Yanıt şablonu | "-Ne zaman?"                                           | Bilinmiyorsa bu parametrenin değerini sormak için bir istem                              |
   | Tür              | Tarih Saat                                                 | Parametre türü, örneğin sayı, dize veya tarih saat                                      |
   | Tarih Varsayılanları     | Tarih eksik ise bugün kullan                             |                                                                                                  |
   | Zaman Varsayılanları     | Eğer zaman içinde kullanım günü kullanım günü                      |                                                                                                  | 

1. Örnek tümceler ekleyin
   
   ```
    set an alarm for {DateTime}
    set alarm {DateTime}
    alarm for {DateTime}
   ```

1. Sonucu doğrulamak için bir tamamlama kuralı ekleyin

   | Ayar    | Önerilen değer                                         | Açıklama                                        |
   | ---------- | ------------------------------------------------------- | -------------------------------------------------- |
   | Kural Adı  | Alarmı ayarla                                               | Kuralın amacını açıklayan bir ad          |
   | Eylemler    | SpeechResponse-"-ok, {DateTime} için alarm kümesi"       | Kural koşulu true olduğunda gerçekleştirilecek eylem |

## <a name="try-it-out"></a>Deneyin

Test panelini seçin ve birkaç etkileşimi deneyin.

- Giriş: yarın, öğleden sonra alarmını ayarla
- Çıkış: "Tamam, alarm kümesi 12/06/2019 12:00:00"

- Giriş: bir alarm ayarlama
- Çıkış: "ne zaman?"
- Giriş: 17:00
- Çıkış: "Tamam, alarm kümesi 12/05/2019 17:00:00"

## <a name="add-the-advanced-rules-for-confirmation"></a>Onay için gelişmiş kuralları ekleme

1. Onay için gelişmiş bir kural ekleyin. 

    Bu kural, kullanıcıdan alarm tarihini ve saatini onaylamasını ve bir sonraki sırayla bir onay (Evet/Hayır) bekliyor olduğunu sorar.

   | Ayar               | Önerilen değer                                                                  | Açıklama                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Kural Adı             | Tarih saatini Onayla                                                                | Kuralın amacını açıklayan bir ad          |
   | Koşullar            | Gerekli parametre-DateTime                                                    | Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |   
   | Eylemler               | SpeechResponse-"-{DateTime} için bir alarm ayarlamak istediğinizden emin misiniz?"       | Kural koşulu true olduğunda gerçekleştirilecek eylem |
   | Yürütmeden sonra durum | Girişi bekle                                                                   | Açıldıktan sonra kullanıcının durumu                  |
   | Beklentileri          | Onay                                                                     | Sonraki sırayla beklenme                      |

1. Başarılı bir onaylamayı işlemek için gelişmiş bir kural ekleme (Kullanıcı Evet olarak diyor)

   | Ayar               | Önerilen değer                                                                  | Açıklama                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Kural Adı             | Kabul edilen onay                                                            | Kuralın amacını açıklayan bir ad          |
   | Koşullar            | Gerekli & parametresi için başarılı bir onay-DateTime                           | Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |   
   | Yürütmeden sonra durum | Tamamlamaya hazırlanıyor                                                             | Açıldıktan sonra kullanıcının durumu                   |

1. Onay reddedildi olarak kabul edilecek gelişmiş bir kural ekleme (Kullanıcı Hayır)

   | Ayar               | Önerilen değer                                                                  | Açıklama                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Kural Adı             | Onayla reddedildi                                                                   | Kuralın amacını açıklayan bir ad          |
   | Koşullar            | DeniedConfirmation & gerekli parametre-DateTime                               | Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |   
   | Eylemler               | ClearParameter-DateTime & SpeechResponse-"-sorun yok, ne zaman sonra?"     | Kural koşulu true olduğunda gerçekleştirilecek eylem |
   | Yürütmeden sonra durum | Girişi bekle                                                                   | Açıldıktan sonra kullanıcının durumu                   |
   | Beklentileri          | ElicitParameters-DateTime                                                      | Sonraki sırayla beklenme                      |

## <a name="try-it-out"></a>Deneyin

Test panelini seçin ve birkaç etkileşimi deneyin.

- Giriş: yarın, öğleden sonra alarmını ayarla
- Çıkış: "12/07/2019 12:00:00 için bir alarm ayarlamak istediğinizden emin misiniz?"
- Giriş: Hayır
- Çıkış: "sorun yok, ne zaman sonra?"
- Giriş: 17:00
- Çıkış: "12/06/2019 17:00:00 için bir alarm ayarlamak istediğinizden emin misiniz?"
- Giriş: Evet
- Çıkış: "Tamam, alarm kümesi 12/06/2019 17:00:00"

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Nasıl yapılır: özel komuta tek adımlı bir düzeltme ekleme (Önizleme)](./how-to-custom-speech-commands-one-step-correction.md)