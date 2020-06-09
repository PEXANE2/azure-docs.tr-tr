---
title: 'Hızlı başlangıç: özel komutlar önizleme uygulaması oluşturma-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede barındırılan özel komutlar önizleme uygulaması oluşturup test edersiniz. Uygulama, utbotları işleyecek.
services: cognitive-services
author: nitinme
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: nitinme
ms.openlocfilehash: 5b54923bb667053ffc886b335ab9d1b0cf4cb754
ms.sourcegitcommit: 5504d5a88896c692303b9c676a7d2860f36394c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84509364"
---
# <a name="quickstart-create-a-custom-commands-preview-app"></a>Hızlı başlangıç: özel komutlar önizleme uygulaması oluşturma

Bu hızlı başlangıçta, özel komutlar uygulaması oluşturmayı ve test yapmayı öğreneceksiniz.
Uygulama, "TV 'yi açma" gibi, "Tamam, TV 'yi açma" gibi basit bir iletiyle yanıt vererek "TV 'yi aç" gibi işlemleri işleyecek şekilde işler.

## <a name="prerequisites"></a>Ön koşullar

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Azure konuşma kaynağı oluşturma<span class="docon docon-navigate-external x-hidden-focus"></span></a>

  > [!NOTE]
  > Şu anda, özel komutlar yalnızca westus, westus2 ve neur bölgelerinde konuşma aboneliklerini destekler.

## <a name="go-to-the-speech-studio-for-custom-commands"></a>Özel komutlar için konuşma Studio 'ya gidin

1. Bir Web tarayıcısında, [konuşma Studio](https://speech.microsoft.com/)'ya gidin.
1. Portalda oturum açmak için kimlik bilgilerinizi girin.

   Varsayılan görünüm, konuşma abonelikleri listesidir.
    > [!NOTE]
    > Abonelik Seç sayfasını görmüyorsanız, ekranın üst kısmındaki Ayarlar menüsünde **konuşma kaynakları** ' nı seçerek buradan ulaşabilirsiniz.

1. Konuşma aboneliğinizi seçin ve ardından **Studio 'Ya git**' i seçin.
1. **Özel komutlar**' ı seçin.

     Varsayılan görünüm, seçili abonelikte sahip olduğunuz özel komutlar uygulamalarının bir listesidir.

## <a name="create-a-custom-commands-project"></a>Özel komutlar projesi oluşturma

1. Proje oluşturmak için **Yeni proje** ' yi seçin.

   > [!div class="mx-imgBorder"]
   > ![Proje oluşturma](media/custom-speech-commands/create-new-project.png)

1. **Ad** kutusuna bir proje adı girin.
1. **Dil** listesinde bir dil seçin.
1. **Lusıs yazma kaynak** listesinde bir yazma kaynağı seçin. Geçerli bir yazma kaynağı yoksa, **yenı lusıs Authoring Resource oluştur**' u seçerek bir tane oluşturun.

   > [!div class="mx-imgBorder"]
   > ![Kaynak oluşturma](media/custom-speech-commands/create-new-resource.png)

   1. **Kaynak adı** kutusuna kaynağın adını girin.
   1. **Kaynak grubu** listesinde, bir kaynak grubu seçin.
   1. **Konum** listesinde bir konum seçin.
   1. **Fiyatlandırma katmanı** listesinde bir katman seçin.

      > [!NOTE]
      > **Kaynak grubu kutusuna kaynak** grubu adı girerek bir kaynak grubu oluşturabilirsiniz. **Oluştur**' u seçtiğinizde kaynak grubu oluşturulur.

1. **Oluştur**'u seçin.
1. Proje oluşturulduktan sonra seçin.

    Artık yeni özel komutlar uygulamanıza ilişkin bir genel bakış görmeniz gerekir.

## <a name="update-luis-resources-optional"></a>LUSıS kaynaklarını güncelleştirme (isteğe bağlı)

**Yeni proje** penceresinde seçtiğiniz yazma kaynağını güncelleştirebilir ve bir tahmin kaynağı ayarlayabilirsiniz. Tahmin kaynağı, özel komutlar uygulamanız yayımlandığında tanınabilmesi için kullanılır. Geliştirme ve test aşamaları sırasında bir tahmin kaynağına ihtiyacınız yoktur.

1. Sol bölmedeki **Ayarlar** ' ı seçin ve ardından orta bölmedeki **lusıs kaynakları** ' nı seçin.
1. Bir tahmin kaynağı seçin veya **Yeni kaynak oluştur**' u seçerek bir tane oluşturun.
1. **Kaydet**'i seçin.
    
    > [!div class="mx-imgBorder"]
    > ![LUSıS kaynaklarını ayarlama](media/custom-speech-commands/set-luis-resources.png)


> [!NOTE]
> Yazma kaynağı ayda yalnızca 1.000 tahmin uç nokta isteklerini desteklediğinden, özel komutlar uygulamanızı yayımlamadan önce bir LUSıS tahmini kaynağı ayarlamanız gerekir.


## <a name="create-a-command"></a>Komut oluşturma

Tek bir utterlik alacak ve iletiyle yanıt verecek basit bir komut oluşturalım `turn on the tv` `Ok, turning on the tv` .

1. Sol bölmenin en üstünde bulunan **Yeni komut** ' i seçerek bir komut oluşturun. **Yeni komut** penceresi açılır.
1. **Ad** kutusuna **turnOn**girin.
1. **Oluştur**'u seçin.

Orta bölmede komutun özellikleri listelenir:


| Yapılandırma            | Description                                                                                                                 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **Örnek cümleler** | Kullanıcının komutu tetikleyebilmesi için söyleme örnekleri.                                                                 |
| **Parametreler**       | Komutu tamamlaması için gereken bilgiler.                                                                                |
| **Tamamlama kuralları** | Komutu yerine getirmek için gerçekleştirilecek eylemler. Örneğin, kullanıcıya yanıt verme veya başka bir Web hizmetiyle iletişim kurma. |
| **Etkileşim kuralları**   | Daha belirgin veya karmaşık durumları işlemek için ek kurallar.                                                              |


> [!div class="mx-imgBorder"]
> ![Komut oluşturma](media/custom-speech-commands/create-add-command.png)


### <a name="add-example-sentences"></a>Örnek cümleler ekleyin

**Örnek cümleler** bölümüyle başlayalım. Kullanıcının söylediklerinize bir örnek sağlıyoruz.

1. Orta bölmede **örnek tümceler** ' ı seçin. 
1. Sağ bölmede örnekler ekleyin:

    ```
    turn on the tv
    ```

1. Bölmenin en üstünde bulunan **Kaydet** ' i seçin.

Şimdilik, parametre yok, bu nedenle **tamamlama kuralları** bölümüne taşıyabiliriz.

### <a name="add-a-completion-rule"></a>Tamamlama kuralı ekleme

Şimdi aşağıdaki yapılandırmaya sahip bir tamamlama kuralı ekleyin. Bu kural kullanıcıya bir karşılama eyleminin alındığını söyler.


| Ayar    | Önerilen değer                          | Description                                        |
| ---------- | ---------------------------------------- | -------------------------------------------------- |
| **Adı**  | **ConfirmationResponse**                  | Kuralın amacını açıklayan bir ad.          |
| **Koşullar** | Yok                                     | Kuralın ne zaman çalıştırılabilirler olduğunu belirleme koşulları.    |
| **Eylemler**    | **Konuşma yanıtı gönderme-> Tamam, TV 'yi açma** | Kural koşulu true olduğunda gerçekleştirilecek eylem. |

1. Orta bölmenin üst kısmındaki **Ekle** ' ye tıklayarak yeni bir tamamlanma kuralı oluşturun.
1. **Ad** kutusuna bir ad girin.
1. Eylem ekleyin.
   1. **Eylemler** bölümünde **Eylem Ekle** seçeneğini belirleyerek bir eylem oluşturun.
   1. **Eylemi Düzenle** penceresinde, **tür** listesinde, **konuşma yanıtı gönder**' i seçin.
   1. **Yanıt**' ın altında **basit düzenleyici**' yi seçin. **İlk varyasyon** kutusunda, **TV 'yi açarak Tamam '** ı girin.

   > [!div class="mx-imgBorder"]
   > ![Yanıt oluşturma](media/custom-speech-commands/create-speech-response-action.png)

1. Kuralı kaydetmek için **Kaydet** ' i seçin.
1. **Tamamlama kuralları** bölümüne geri döndüğünüzde tüm değişiklikleri kaydetmek için **Kaydet** ' i seçin. 

> [!div class="mx-imgBorder"]
> ![Tamamlama kuralı oluşturma](media/custom-speech-commands/create-basic-completion-response-rule.png)



## <a name="try-it-out"></a>Deneyin

Test sohbeti panelini kullanarak davranışı test edin.
1. Sağ bölmenin en üstünde **eğitme** ' yi seçin.
1. Eğitim yapıldıktan sonra **Test**' i seçin. Uygulama pencerenizin yeni bir **testi** görüntülenir.
    - **TV 'yi aç '** a gir
    - Beklenen yanıt: **Tamam, TV 'yi açma**


> [!div class="mx-imgBorder"]
> ![Davranışı test etme](media/custom-speech-commands/create-basic-test-chat.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hızlı başlangıç: parametrelerle özel komutlar önizleme uygulaması oluşturma](./quickstart-custom-speech-commands-create-parameters.md)
