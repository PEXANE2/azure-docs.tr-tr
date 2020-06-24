---
title: 'Nasıl yapılır: basit komutlarla uygulama oluşturma-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede, basit komutları kullanarak barındırılan özel komutlar uygulaması oluşturmayı ve test yapmayı öğreneceksiniz.
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: sausin
ms.openlocfilehash: 7f98b2c94659c083f5dbd2c43fd9015f8b60a9db
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85307885"
---
# <a name="create-application-with-simple-commands"></a>Basit komutlarla uygulama oluşturma

Bu makalede şunları öğreneceksiniz:
 - Boş bir uygulama oluşturun
 - LUSıS kaynaklarını güncelleştirme
 - Özel komutlar uygulamanıza bazı temel komutlar ekleyin

## <a name="create-empty-application"></a>Boş uygulama oluştur
Boş bir özel komutlar uygulaması oluşturun. Ayrıntılar için [hızlı başlangıç](quickstart-custom-commands-application.md)bölümüne bakın. Bu kez, bir projeyi içeri aktarmak yerine boş bir proje oluşturursunuz.

   > [!div class="mx-imgBorder"]
   > ![Proje oluşturma](media/custom-commands/create-new-project.png)

## <a name="update-luis-resources-optional"></a>LUSıS kaynaklarını güncelleştirme (isteğe bağlı)

**Yeni proje** penceresinde seçtiğiniz yazma kaynağını güncelleştirebilir ve bir tahmin kaynağı ayarlayabilirsiniz. Tahmin kaynağı, özel komutlar uygulamanız yayımlandığında tanınabilmesi için kullanılır. Geliştirme ve test aşamaları sırasında bir tahmin kaynağına ihtiyacınız yoktur.

## <a name="add-turnon-command"></a>TurnOn komutu Ekle

**Akıllı Oda-Lite** özel komutlar uygulamasında, bir utterance işleme, ve iletiyle yanıt veren basit bir komut ekleyin `turn on the tv` `Ok, turning the tv on` .

1. Sol bölmenin en üstünde bulunan **Yeni komut** ' i seçerek yeni bir komut oluşturun. **Yeni komut** penceresi açılır.
1. **Ad** alanı için **turnOn**olarak değer sağlayın.
1. **Oluştur**'u seçin.

Orta bölmede komutunun farklı özellikleri listelenir. Komutun aşağıdaki özelliklerini yapılandırırsınız. Bir komutun tüm yapılandırma özelliklerinin açıklaması için, [Başvurular](./custom-commands-references.md)' a gidin.

| Yapılandırma            | Description                                                                                                                 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **Örnek cümleler** | Kullanıcının bu komutu tetikleyebilmesi için söyledikleri örnek                                                                 |
| **Parametreler**       | Komutu tamamlaması için gereken bilgiler                                                                                |
| **Tamamlama kuralları** | Komutu yerine getirmek için gerçekleştirilecek eylemler. Örneğin, kullanıcıya yanıt vermek veya başka bir Web hizmetiyle iletişim kurmak için. |
| **Etkileşim kuralları**   | Daha belirgin veya karmaşık durumları işlemek için ek kurallar                                                              |


> [!div class="mx-imgBorder"]
> ![Komut oluşturma](media/custom-commands/add-new-command.png)

### <a name="add-example-sentences"></a>Örnek cümleler ekleyin

**Örnek cümleler** bölümüne başlayalım ve kullanıcının söyleme bir örneğini sağlayın.

1. Orta bölmedeki **örnek cümleler** bölümünü seçin.
1. Sağ bölmede örnek ekleyin:

    ```
    turn on the tv
    ```

1.  Bölmenin en üstünde bulunan **Kaydet** ' i seçin.

Şimdilik, parametre yok, bu nedenle **tamamlama kuralları** bölümüne taşıyabiliriz.

### <a name="add-a-completion-rule"></a>Tamamlama kuralı ekleme

Sonra, komutun bir tamamlama kuralına sahip olması gerekir. Bu kural kullanıcıya bir karşılama eyleminin alındığını söyler. Kurallar ve tamamlama kuralları hakkında daha fazla bilgi edinmek için [Başvurular](./custom-commands-references.md)'a gidin.

1. Varsayılan **tamamlama kuralını seçin** ve bunu aşağıdaki gibi düzenleyin: 

    
    | Ayar    | Önerilen değer                          | Description                                        |
    | ---------- | ---------------------------------------- | -------------------------------------------------- |
    | **Adı**       | ConfirmationResponse                  | Kuralın amacını açıklayan bir ad          |
    | **Koşullar** | Yok                                     | Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |
    | **Eylemler**    | Ilk varyasyon > basit düzenleyici > konuşma yanıtı gönderin >`Ok, turning the tv on` | Kural koşulu true olduğunda gerçekleştirilecek eylem |
    

1. Alternatif olarak, var olan varsayılan tamamlama kuralını silebilir ve orta bölmenin üst kısmındaki **Ekle** ' yi seçerek yeni bir tane oluşturabilirsiniz.
1. **Ad** bölümünde değer sağlayın.
1. Eylem ekleyin.
   1. **Eylemler** bölümünde **Eylem Ekle** seçeneğini belirleyerek bir eylem oluşturun.
   1. **Yeni eylem** penceresinde, **tür** listesinde, **konuşma yanıtı gönder**' i seçin.
   1. **Yanıt**' ın altında **basit düzenleyici**' yi seçin.
   1. **İlk varyasyon** alanında, yanıt için değer sağlayın `Ok, turning the tv on` .

   > [!div class="mx-imgBorder"]
   > ![Konuşma yanıtı oluşturma](media/custom-commands/create-speech-response-action.png)

1. Eylemi kaydetmek için **Kaydet** ' i seçin.
1. **Tamamlama kuralları** bölümüne geri döndüğünüzde tüm değişiklikleri kaydetmek için **Kaydet** ' i seçin. 


### <a name="try-it-out"></a>Deneyin

Test sohbeti panelini kullanarak davranışı test etme
1. Sağ bölmenin en üstünde bulunan **eğitme** simgesini seçin.
1. Eğitim tamamlandığında **Test**' i seçin. Ses/metin aracılığıyla aşağıdaki şekilde deneyin:
    - Giriş: TV 'yi açın
    - Çıkış: Tamam, TV 'yi açma


> [!div class="mx-imgBorder"]
> ![Web sohbeti ile test etme](media/custom-commands/create-basic-test-chat.png)

> [!TIP]
> Test panelinde, bu ses/metin girişinin nasıl işlendiği konusunda bilgi için **ayrıntıları etkinleştir** ' i seçebilirsiniz.  

## <a name="add-settemperature-command"></a>Setsıcaklık Ekle komutu

Şimdi, tek bir utterlik alacak ve iletiyle yanıt verecek bir daha fazla komut **Setsıcaklığı** ekleyin `set the temperature to 40 degrees` `Ok, setting temperature to 40 degrees` .

Örnek cümlesini kullanarak yeni bir komut oluşturmak için **turnOn** komutu için gösterilen adımları izleyin, "**sıcaklığın 40 derece olarak ayarlanması**".

Ardından **, var olan** tamamlama kurallarını aşağıdaki gibi düzenleyin:

| Ayar    | Önerilen değer                          |
| ---------- | ---------------------------------------- |
| Name  | ConfirmationResponse                  |
| Koşullar | Yok                                     |
| Eylemler    | Ilk varyasyon > basit düzenleyici > konuşma yanıtı gönderin >`Ok, setting temperature to 40 degrees` |

Komutta yapılan tüm değişiklikleri kaydetmek için **Kaydet** ' i seçin.

## <a name="add-setalarm-command"></a>SetAlarm komutu Ekle
"**9 har için alarm ayarla**" örnek cümlesini kullanarak yeni bir komut **SetAlarm** oluşturun. Ardından **, var olan** tamamlama kurallarını aşağıdaki gibi düzenleyin:

| Ayar    | Önerilen değer                          |
| ---------- | ---------------------------------------- |
| Kural Adı  | ConfirmationResponse                  |
| Koşullar | Yok                                     |
| Eylemler    | Ilk varyasyon > basit düzenleyici > konuşma yanıtı gönderin >`Ok, setting an alarm for 9 am tomorrow` |

Komutta yapılan tüm değişiklikleri kaydetmek için **Kaydet** ' i seçin.

## <a name="try-it-out"></a>Deneyin

Test sohbeti panelini kullanarak davranışı test etme
1. **Eğitim**' i seçin. Eğitim başarılı olduktan sonra **Test** ' i seçin ve deneyin:
    - Şunu yazın: sıcaklığını 40 derece olarak ayarlayın
    - Beklenen yanıt: Tamam, sıcaklık 40 derece olarak ayarlanıyor
    - Şunu yazın: TV 'yi açın
    - Beklenen yanıt: Tamam, TV 'yi açma
    - Şunu yazın: 9 yarın için bir alarm ayarlayın
    - Beklenen yanıt: Tamam, 9 yarın için bir alarm ayarlanıyor

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Nasıl yapılır: komutlara parametreler ekleme](./how-to-custom-commands-add-parameters-to-commands.md)
