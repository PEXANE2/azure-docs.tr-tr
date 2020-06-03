---
title: Özel komutlarda doğrulama ekleme önizleme-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Özel komutlar önizleme uygulamasında bir komut parametresine doğrulama eklemeyi öğrenin.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: donkim
ms.openlocfilehash: 6686016f109fad4ee8b7f4e494b1374a6003658c
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84310419"
---
# <a name="add-validations-to-a-command-parameter-in-a-custom-commands-preview-application"></a>Özel komutlar önizleme uygulamasında bir komut parametresine doğrulama ekleme

Bu makalede, parametrelere nasıl doğrulama ekleneceğini ve düzeltme istemlerini öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki makalelerdeki adımları uygulayın:

> [!div class="checklist"]
 
> * [Hızlı başlangıç: özel komutlar önizleme uygulaması oluşturma](./quickstart-custom-speech-commands-create-new.md)
> * [Hızlı başlangıç: parametrelerle özel bir komut önizleme uygulaması oluşturma](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-settemperature-command"></a>Setsıcaklık komutu oluşturma

Doğrulamaları göstermek için, kullanıcıların sıcaklık ayarlamasına izin veren yeni bir komut oluşturun.

1. [Konuşma Studio](https://speech.microsoft.com/)'da, oluşturduğunuz özel komutlar Önizleme uygulamasını açın.
1. Yeni bir **Setsıcaklık** komutu oluşturun.
1. Aşağıdaki yapılandırmaya sahip bir sıcaklık parametresi ekleyin:

   | Parametre yapılandırması           | Önerilen değer    |Description                 |                                    
   | ----------------- | ----------------------------------| -------------|
   | **Adı**              | **Sıcaklık**                       | Parametre için açıklayıcı bir ad                                |
   | **Gerekli**          | İşaretli                           | Komutu tamamlamadan önce Bu parametre için bir değer gerekip gerekmediğini belirten onay kutusu |
   | **Gerekli parametre için yanıt**     | **Basit Düzenleyici-hangi sıcaklığın istediğiniz >?**  | Bilinmiyorsa bu parametrenin değerini sormak için bir istem |
   | **Tür**              | **Sayı**                            | Sayı, dize, DateTime veya Coğrafya gibi parametre türü   |

1. Sıcaklık parametresi için bir doğrulama ekleyin.

    1. Sıcaklık parametresi için **Parametreler** yapılandırma sayfasında, **doğrulamalar** bölümünde **Doğrulama Ekle** ' yi seçin.

    1. **Yeni doğrulama** açılır penceresinde doğrulamayı aşağıdaki şekilde yapılandırın:
  
       | Parametre yapılandırması         | Önerilen değer                                          | Description                                                                        |
       | ----------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
       | **Minimum Değer**        | **60**               | Sayı parametreleri için bu parametrenin varsaydığı en küçük değer |
       | **Maksimum Değer**        | **80**               | Sayı parametreleri için bu parametrenin varsaydığı en büyük değer |
       | **Hata yanıtı-basit düzenleyici**| **İlk değişim-Üzgünüz, yalnızca 60 ve 80 derece arasında ayarlayabiliyorum**      | Doğrulama başarısız olursa yeni bir değer sormak isteyip istemediğinizi sor                                       |

       > [!div class="mx-imgBorder"]
       > ![Aralık doğrulaması ekle](media/custom-speech-commands/validations-add-temperature.png)

1. **Oluştur**’u seçin.

1. Örnek cümleler ekleyin.

   ```
   set the temperature to {Temperature} degrees
   change the temperature to {Temperature}
   set the temperature
   change the temperature
   ```

1. Aşağıdaki yapılandırmaya sahip bir tamamlama kuralı ekleyin. Bu kural, sonucu onaylar.

   | Ayar    | Önerilen değer                                           |Description                                     |
   | ---------- | --------------------------------------------------------- |-----|
   | Name       | Onay Iletisi                                      |Kuralın amacını açıklayan bir ad |
   | **Koşullar** | **Gerekli parametreler-sıcaklık**                       |Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |   
   | **Eylemler**    | **Konuşma yanıtı gönder-tamam, sıcaklığın {sıcaklık} derecenin olarak ayarlanması** | Kural koşulu true olduğunda gerçekleştirilecek eylem |

> [!TIP]
> Bu örnek, sonucu doğrulamak için bir konuşma yanıtı kullanır. Komutu bir istemci eylemiyle tamamlamaya yönelik örnekler için, bkz. [nasıl yapılır: istemci üzerinde konuşma SDK 'sı ile komutları yerine](./how-to-custom-speech-commands-fulfill-sdk.md)alma.

## <a name="try-it-out"></a>Deneyin

1. **Eğitim**' i seçin.

1. Eğitim yapıldıktan sonra **Test**' i seçin ve ardından bu etkileşimleri deneyin:

    - Giriş: sıcaklığı 72 derece olarak ayarlayın
    - Çıkış: Tamam, sıcaklık 72 derece olarak ayarlanıyor
    - Giriş: sıcaklığı 45 derece olarak ayarlayın
    - Çıkış: Üzgünüz, yalnızca 60 ve 80 derece arasında ayarlayabiliyorum
    - Giriş: bunun yerine 72 derece yapın
    - Çıkış: Tamam, sıcaklık 72 derece olarak ayarlanıyor

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel komutlar önizleme uygulamasında bir komuta onaylar ekleme](./how-to-custom-speech-commands-confirmations.md)
