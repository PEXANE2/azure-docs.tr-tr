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
ms.openlocfilehash: bf1b79c1b5d7b9dfd93b354c6b6ff5a512bb74a5
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858217"
---
# <a name="how-to-add-a-confirmation-to-a-custom-command-preview"></a>Nasıl yapılır: özel komuta onay ekleme (Önizleme)

Bu makalede, komuta nasıl onay ekleneceğini öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki makalelerde bulunan adımları tamamlamış olmanız gerekir:
> [!div class="checklist"]
> *  [Hızlı başlangıç: özel komut oluşturma (Önizleme)](./quickstart-custom-speech-commands-create-new.md)
> * [Hızlı başlangıç: parametrelerle özel komut oluşturma (Önizleme)](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-setalarm-command"></a>SetAlarm komutu oluşturma

Onayları göstermek için, kullanıcının bir alarm ayarlamaya izin veren yeni bir komut oluşturalım.

1. Daha önce oluşturduğunuz özel komutlar uygulamanızı [konuşma Studio](https://speech.microsoft.com/)'da açın.
1. Yeni bir komut `SetAlarm`oluşturun.
1. Aşağıdaki yapılandırmayla adlı `DateTime` bir parametre ekleyin.

   | Ayar                           | Önerilen değer                     |  Açıklama                 |
   | --------------------------------- | -----------------------------------------------------| ------------|
   | Name                              | DateTime                                | Parametre için açıklayıcı bir ad                                |
   | Gerekli                          | checked                                 | Komutu tamamlamadan önce Bu parametre için bir değer gerekip gerekmediğini belirten onay kutusu |
   | Gerekli parametre için yanıt   | Basit düzenleyici-ne zaman >?                              | Bilinmiyorsa bu parametrenin değerini sormak için bir istem |
   | Tür                              | DateTime                                | Parametre türü, örneğin sayı, dize, tarih saat veya Coğrafya   |
   | Tarih Varsayılanları                     | Tarih eksik ise bugün kullan            | Kullanıcı tarafından sağlanmazsa kullanılacak değişkenin varsayılan değeri.  |  
   | Zaman Varsayılanları                     | Eğer zaman içinde kullanım günü kullanım günü     |  Kullanıcı tarafından sağlanmazsa kullanılacak değişkenin varsayılan değeri.|

1. Örnek cümleler ekleyin.
   
    ```
    set an alarm for {DateTime}
    set alarm {DateTime}
    alarm for {DateTime}
   ```

1. Sonucu doğrulamak için bir tamamlama kuralı ekleyin.

   | Ayar    | Önerilen değer                               |Açıklama                                     |
   | ---------- | ------------------------------------------------------- |-----|
   | Kural Adı  | Alarmı ayarla                                               |    Kuralın amacını açıklayan bir ad |
   | Eylemler    | Konuşma yanıtı gönder-tamam, alarm kümesi {DateTime} "    |Kural koşulu true olduğunda gerçekleştirilecek eylem

## <a name="try-it-out"></a>Deneyin

1. Sağ `Train` bölmenin en üstünde bulunan simgesi seçin.

1. Eğitim tamamlandığında öğesini seçin `Test`.
    - Giriş: yarın, öğleden sonra alarmını ayarla
    - Çıkış: Tamam, 2020-05-02 12:00:00 için alarm kümesi
    - Giriş: bir alarm ayarlama
    - Çıkış: ne zaman?
    - Giriş: 17:00
    - Çıkış: Tamam, 2020-05-01 17:00:00 için alarm kümesi

## <a name="add-the-advanced-rules-for-confirmation"></a>Onay için gelişmiş kuralları ekleme

Teyitler, etkileşimler kuralları ekleme yoluyla elde edilir.

1. Mevcut `SetAlarm` komutta, orta bölmedeki seçim `+Add` simgesine göre bir **etkileşim kuralı** ekleyin ve ardından **etkileşim kuralları** -> **Onayla komutunu**seçin.

    Bu kural, kullanıcıdan alarm tarihini ve saatini onaylamasını ve bir sonraki sırayla bir onay (Evet/Hayır) bekliyor olduğunu sorar.

   | Ayar               | Önerilen değer                                                                  | Açıklama                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Kural Adı             | Tarih saatini Onayla                                                                | Kuralın amacını açıklayan bir ad          |
   | Koşullar            | Gerekli parametre-> DateTime                                                    | Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |   
   | Eylemler               | Konuşma yanıtı gönderme-> {DateTime} için bir alarm ayarlamak istediğinizden emin misiniz?     | Kural koşulu true olduğunda gerçekleştirilecek eylem |
   | Beklentileri          | Kullanıcıdan onay bekleniyor                                                 | Sonraki sırayla beklenme                      |
   | Yürütme sonrası durumu  | Kullanıcının girişini bekle                                                            | Açıldıktan sonra kullanıcının durumu                  |
  
      > [!div class="mx-imgBorder"]
      > ![Gerekli parametre yanıtı oluştur](media/custom-speech-commands/add-validation-set-temperature.png)

1. Başarılı bir onaylamayı işlemek için başka bir etkileşim kuralı ekleme (Kullanıcı Evet olarak diyor)

   | Ayar               | Önerilen değer                                                                  | Açıklama                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Kural Adı             | Kabul edilen onay                                                            | Kuralın amacını açıklayan bir ad          |
   | Koşullar            | Onay başarılı & gerekli parametre-> DateTime                      | Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |   
   | Yürütme sonrası durumu | Tamamlanma kuralları yürütme                                                          | Açıldıktan sonra kullanıcının durumu                   |

1. Onay reddedildi olarak kabul edilecek gelişmiş bir kural ekleme (Kullanıcı Hayır)

   | Ayar               | Önerilen değer                                                                  | Açıklama                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | Kural Adı             | Onay reddedildi                                                                   | Kuralın amacını açıklayan bir ad          |
   | Koşullar            | Gerekli parametre-> DateTime & onay reddedildi                               | Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |   
   | Eylemler               | Parametre değerini temizle-> DateTime & konuşma yanıtı gönder-> sorun yok, ne zaman?  | Kural koşulu true olduğunda gerçekleştirilecek eylem |
   | Yürütmeden sonra durum | Girişi bekle                                                                   | Açıldıktan sonra kullanıcının durumu                   |
   | Beklentileri          | Kullanıcı-> DateTime değerinden parametre girişi bekleniyor                           | Sonraki sırayla beklenme                      |

## <a name="try-out-the-changes"></a>Değişiklikleri deneyin

Seçin `Train`, eğitimin tamamlanmasını bekleyin ve seçin `Test`.

- Giriş: yarın, öğleden sonra alarmını ayarla
- Çıkış: 2020-05-02 12:00:00 için bir alarm ayarlamak istediğinizden emin misiniz?
- Giriş: Hayır
- Çıkış: sorun değil, ne zaman?
- Giriş: 17:00
- Çıkış: "2020-05-01 17:00:00 için bir alarm ayarlamak istediğinizden emin misiniz?"
- Giriş: Evet
- Çıkış: Tamam, 2020-05-01 17:00:00 için alarm kümesi

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Nasıl yapılır: özel komuta tek adımlı bir düzeltme ekleme (Önizleme)](./how-to-custom-speech-commands-one-step-correction.md)