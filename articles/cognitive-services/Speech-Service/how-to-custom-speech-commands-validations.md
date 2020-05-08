---
title: 'Nasıl yapılır: özel komut parametrelerine doğrulama ekleme'
titleSuffix: Azure Cognitive Services
description: Bu makalede, özel komutlarda bir parametreye nasıl doğrulama ekleneceğini açıkladık.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: donkim
ms.openlocfilehash: 2b7fd608156ab269cfc0c85c6c508fa9d5eebc83
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82857200"
---
# <a name="how-to-add-validations-to-custom-command-parameters-preview"></a>Nasıl yapılır: özel komut parametrelerine doğrulama ekleme (Önizleme)

Bu makalede, parametrelere doğrulama ekleyecek ve düzeltme isteminde bulunur.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki makalelerde bulunan adımları tamamlamış olmanız gerekir:

> [!div class="checklist"]
> * [Hızlı başlangıç: özel bir komut oluşturma](./quickstart-custom-speech-commands-create-new.md)
> * [Hızlı başlangıç: parametrelerle özel komut oluşturma](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-settemperature-command"></a>Setsıcaklık komutu oluşturma

Doğrulamaları göstermek için kullanıcıların sıcaklığa izin vermesini sağlayan yeni bir komut oluşturalım.

1. Daha önce oluşturduğunuz özel komutlar uygulamanızı [konuşma Studio](https://speech.microsoft.com/) 'da açın
1. Yeni bir komut oluştur`SetTemperature`
1. Hedef sıcaklık için bir parametre ekleyin.

   | Parametre yapılandırması           | Önerilen değer    |Açıklama                 |                                    
   | ----------------- | ----------------------------------| -------------|
   | Name              | Sıcaklık                       | Parametre için açıklayıcı bir ad                                |
   | Gerekli          | checked                           | Komutu tamamlamadan önce Bu parametre için bir değer gerekip gerekmediğini belirten onay kutusu |
   | Gerekli parametre için yanıt     | Basit Düzenleyici-hangi sıcaklığın istediğiniz >?  | Bilinmiyorsa bu parametrenin değerini sormak için bir istem |
   | Tür              | Sayı                            | Sayı, dize, tarih saat veya Coğrafya gibi parametre türü   |

1. Sıcaklık parametresi için bir doğrulama ekleyin.

    - Parametresi `Temperature` için **Parametreler** yapılandırma sayfasında, doğrulamalar bölümünden öğesini `Add a validation` seçin.
    - **Yeni doğrulama** açılır penceresinde değerleri aşağıda gösterildiği gibi girin ve **Oluştur**' u seçin.

  
       | Parametre yapılandırması         | Önerilen değer                                          | Açıklama                                                                        |
       | ----------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
       | Minimum Değer        | 60               | Sayı parametreleri için bu parametrenin varsaydığı en küçük değer |
       | Maksimum Değer        | 80               | Sayı parametreleri için bu parametrenin varsaydığı en büyük değer |
       | Hata yanıtı-basit düzenleyici| İlk değişim-Üzgünüz, yalnızca 60 ve 80 derece arasında ayarlayabiliyorum      | Doğrulama başarısız olursa yeni bir değer sormak isteyip istemediğinizi sor                                       |

       > [!div class="mx-imgBorder"]
       > ![Aralık doğrulaması ekle](media/custom-speech-commands/validations-add-temperature.png)

1. Örnek tümceler ekleyin

   ```
   set the temperature to {Temperature} degrees
   change the temperature to {Temperature}
   set the temperature
   change the temperature
   ```

1. Sonucu doğrulamak için bir tamamlama kuralı ekleyin

   | Ayar    | Önerilen değer                                           |Açıklama                                     |
   | ---------- | --------------------------------------------------------- |-----|
   | Name       | Onay Iletisi                                      |Kuralın amacını açıklayan bir ad |
   | Koşullar | Gerekli parametreler-`Temperature`                       |Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |   
   | Eylemler    | Konuşma yanıtı gönder-`Ok, setting temperature to {Temperature} degrees` | Kural koşulu true olduğunda gerçekleştirilecek eylem |

> [!TIP]
> Bu örnek, sonucu doğrulamak için bir konuşma yanıtı kullanır. Komutu bir istemci eylemiyle tamamlamaya yönelik örnekler için bkz. [nasıl yapılır: istemci üzerinde konuşma SDK 'sı Ile komutları yerine](./how-to-custom-speech-commands-fulfill-sdk.md) alma


## <a name="try-it-out"></a>Deneyin
1. Sağ `Train` bölmenin en üstünde bulunan simgesi seçin.

1. Eğitim tamamlandığında, birkaç etkileşimi seçip `Test` yeniden deneyin.

    - Giriş: sıcaklığı 72 derece olarak ayarlayın
    - Çıkış: Tamam, sıcaklık 72 derece olarak ayarlanıyor
    - Giriş: sıcaklığı 45 derece olarak ayarlayın
    - Çıkış: Üzgünüz, yalnızca 60 ve 80 derece arasında ayarlayabiliyorum
    - Giriş: bunun yerine 72 derece yapın
    - Çıkış: Tamam, sıcaklık 72 derece olarak ayarlanıyor

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Nasıl yapılır: özel komuta onay ekleme (Önizleme)](./how-to-custom-speech-commands-confirmations.md)
