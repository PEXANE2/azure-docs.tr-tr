---
title: Özel komutlar önizleme uygulaması-konuşma hizmetine onaylar ekleme
titleSuffix: Azure Cognitive Services
description: Özel komutlar önizleme uygulamasındaki komutlara nasıl teyid ekleneceğini öğrenin.
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: encorona
ms.openlocfilehash: 1cb0624012b22b6cae2c98bfa6ddc9495e09615d
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84310477"
---
# <a name="add-confirmations-to-a-command-in-a-custom-commands-preview-application"></a>Özel komutlar önizleme uygulamasındaki bir komuta onaylar ekleme

Bu makalede, özel komutlar önizleme uygulamasında komutlar için onayları oluşturmayı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki makalelerdeki adımları uygulayın:
> [!div class="checklist"]
> * [Hızlı başlangıç: özel komutlar önizleme uygulaması oluşturma](./quickstart-custom-speech-commands-create-new.md)
> * [Hızlı başlangıç: parametrelerle özel bir komut önizleme uygulaması oluşturma](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-setalarm-command"></a>SetAlarm komutu oluşturma

Onayları göstermek için, bir alarm ayarlayan yeni bir komut oluşturun.

1. [Konuşma Studio](https://speech.microsoft.com/)'da, oluşturduğunuz özel komutlar Önizleme uygulamasını açın.
1. Yeni bir **SetAlarm** komutu oluşturun.
1. Aşağıdaki yapılandırmaya sahip bir **DateTime** parametresi ekleyin:

   | Ayar                           | Önerilen değer                     |  Description                 |
   | --------------------------------- | -----------------------------------------------------| ------------|
   | **Adı**                              | **Tarih Saat**                                | Parametre için tanımlayıcı ad                                |
   | **Gerekli**                          | İşaretli                                 | Komutu tamamlamadan önce Bu parametre için bir değer gerekip gerekmediğini belirten onay kutusu |
   | **Gerekli bir parametre için yanıt**   | **Basit düzenleyici-ne zaman >?**                              | Bilinmiyorsa bu parametrenin değerini sormak için bir istem |
   | **Tür**                              | **Tarih Saat**                                | Sayı, dize, DateTime veya Coğrafya gibi parametre türü   |
   | **Tarih Varsayılanları**                     | Tarih eksikse, bugünün tarihini kullanın            | Kullanıcı tarafından sağlanmazsa kullanılacak değişkenin varsayılan değeri  |  
   | **Zaman Varsayılanları**                     | Saat eksikse, günün başlangıcını kullanın     |  Kullanıcı tarafından sağlanmazsa kullanılacak değişkenin varsayılan değeri|

1. Örnek cümleler ekleyin.
   
    ```
    set an alarm for {DateTime}
    set alarm {DateTime}
    alarm for {DateTime}
   ```

1. Sonucu doğrulamak için bir tamamlama kuralı ekleyin. Aşağıdaki yapılandırmayı kullanın:

   | Ayar    | Önerilen değer                               |Description                                     |
   | ---------- | ------------------------------------------------------- |-----|
   | **Kural Adı**  | **Alarmı ayarla**                                               |    Kuralın amacını açıklayan bir ad |
   | **Eylemler**    | **Konuşma yanıtı gönder-> Tamam, uyarı kümesi {DateTime}**    |Kural koşulu true olduğunda gerçekleştirilecek eylem

## <a name="try-it-out"></a>Deneyin

1. Sağ bölmenin en üstünde **eğitme** ' yi seçin.

1. Eğitim yapıldıktan sonra **Test**' i seçin ve ardından aşağıdaki etkileşimleri deneyin:
    - Giriş: yarın, öğleden sonra alarmını ayarla
    - Çıkış: Tamam, 2020-05-02 12:00:00 için alarm kümesi
    - Giriş: bir alarm ayarlama
    - Çıkış: ne zaman?
    - Giriş: 17:00
    - Çıkış: Tamam, 2020-05-01 17:00:00 için alarm kümesi

## <a name="add-interaction-rules-for-the-confirmation"></a>Onay için etkileşim kuralları Ekle

Etkileşim kuralları ekleyerek teyitler oluşturun.

1. **SetAlarm** komutdaysanız orta bölmeden **Ekle** ' yi seçin ve ardından **etkileşim kuralları**  >  **Onayla komutunu**seçin.

    Bu kural, kullanıcıdan alarm tarihini ve saatini onaylamasını ister. Şu ayarları kullanın:

   | Ayar               | Önerilen değer                                                                  | Description                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | **Kural Adı**             | **Tarih saatini Onayla**                                                                | Kuralın amacını açıklayan bir ad          |
   | **Koşullar**            | **Gerekli parametre-> DateTime**                                                    | Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |   
   | **Eylemler**               | **Konuşma yanıtı gönderme-> {DateTime} için bir alarm ayarlamak istediğinizden emin misiniz?**     | Kural koşulu true olduğunda gerçekleştirilecek eylem |
   | **Beklentileri**          | **Kullanıcıdan onay bekleniyor**                                                 | Sonraki sırayla beklenme                      |
   | **Yürütme sonrası durumu**  | **Kullanıcının girişini bekle**                                                            | Açıldıktan sonra kullanıcının durumu                  |
  
      > [!div class="mx-imgBorder"]
      > ![Gerekli parametre yanıtı oluştur](media/custom-speech-commands/add-validation-set-temperature.png)

1. Kabul edilen onay için bir etkileşim kuralı ekleyin (Kullanıcı "Evet" diyor ki). Aşağıdaki yapılandırmayı kullanın:

   | Ayar               | Önerilen değer                                                                  | Description                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | **Kural Adı**             | **Kabul edilen onay**                                                            | Kuralın amacını açıklayan bir ad          |
   | **Koşullar**            | **Onay başarılı & gerekli parametre-> DateTime**                      | Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |   
   | **Yürütme sonrası durumu** | **Tamamlanma kuralları yürütme**                                                          | Açıldıktan sonra kullanıcının durumu                   |

1. Reddedilen onay için bir etkileşim kuralı ekleyin (Kullanıcı "Hayır" olarak kabul edilir). Aşağıdaki yapılandırmayı kullanın:

   | Ayar               | Önerilen değer                                                                  | Description                                        |
   | --------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------- |
   | **Kural Adı**             | **Onay reddedildi**                                                                   | Kuralın amacını açıklayan bir ad          |
   | **Koşullar**            | **Gerekli parametre-> DateTime & onay reddedildi**                               | Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |   
   | **Eylemler**               | **Parametre değerini temizle-> DateTime & konuşma yanıtı gönder-> sorun yok, ne zaman?**  | Kural koşulu true olduğunda gerçekleştirilecek eylem |
   | **Yürütmeden sonra durum** | **Girişi bekle**                                                                   | Açıldıktan sonra kullanıcının durumu                   |
   | **Beklentileri**          | **Kullanıcı-> DateTime değerinden parametre girişi bekleniyor**                           | Sonraki sırayla beklenme                      |

## <a name="try-out-the-changes"></a>Değişiklikleri deneyin

1. **Eğitim**' i seçin.

1. Eğitim yapıldıktan sonra **Test**' i seçin ve ardından bu etkileşimleri deneyin:

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
> [Özel komutlar önizleme uygulamasında bir komuta tek adımlı bir düzeltme ekleme](./how-to-custom-speech-commands-one-step-correction.md)
