---
title: 'Hızlı başlangıç: özel bir komut (Önizleme) oluşturma-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede barındırılan özel komutlar uygulaması oluşturup test edersiniz.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: donkim
ms.openlocfilehash: f31d7279b73bab7aefda4c4b6570500d05cb89d7
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82872515"
---
# <a name="quickstart-create-a-custom-commands-app-preview"></a>Hızlı başlangıç: özel komutlar uygulaması oluşturma (Önizleme)

Bu hızlı başlangıçta, özel komutlar uygulaması oluşturmayı ve test yapmayı öğreneceksiniz.
Oluşturulan uygulama "TV 'yi aç" ve "Tamam, TV 'yi açma" gibi basit bir iletiyle yanıt verecek şekilde işlem görür.

## <a name="prerequisites"></a>Ön koşullar

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Azure konuşma kaynağı oluşturma<span class="docon docon-navigate-external x-hidden-focus"></span></a>

  > [!NOTE]
  > Şu anda, özel komutlar yalnızca westus, westus2 ve neur bölgelerinde konuşma aboneliklerini destekler.

## <a name="go-to-the-speech-studio-for-custom-commands"></a>Özel komutlar için konuşma Studio 'ya gidin

1. Web tarayıcınızı açın ve [konuşma Studio](https://speech.microsoft.com/) 'ya gidin
1. Portalda oturum açmak için kimlik bilgilerinizi girin.

   - Varsayılan görünüm, konuşma abonelikleri listesidir.
     > [!NOTE]
     > Abonelik Seç sayfasını görmüyorsanız, üstteki çubukta ayarlar menüsünden "konuşma kaynakları" ' nı seçerek buraya gidebilirsiniz.

1. Konuşma aboneliğinizi seçin ve ardından **Studio 'Ya git**' i seçin.
1. **Özel komutlar**' ı seçin.

     - Varsayılan görünüm, seçtiğiniz aboneliğin altında sahip olduğunuz özel komutlar uygulamalarının bir listesidir.

## <a name="create-a-custom-commands-project"></a>Özel komutlar projesi oluşturma

1. Yeni proje oluşturmak için **Yeni proje** ' yi seçin.

   > [!div class="mx-imgBorder"]
   > ![Proje oluşturma](media/custom-speech-commands/create-new-project.png)

1. Proje adını girin.
1. Açılan listeden dil ' i seçin.
1. Açılan listeden bir yazma kaynağı seçin. Geçerli bir yazma kaynağı yoksa, **yenı lusıs Authoring Resource oluştur**' a tıklayarak bir tane oluşturun.

   > [!div class="mx-imgBorder"]
   > ![Kaynak oluşturma](media/custom-speech-commands/create-new-resource.png)

   - Kaynak adı, kaynak grubu girin.
   - Konum için değer ' i ve açılan listeden fiyatlandırma katmanını seçin.

      > [!NOTE]
      > "Kaynak grubu" alanına istenen kaynak grubu adını girerek kaynak grupları oluşturabilirsiniz. Kaynak grubu, **Oluştur** seçildiğinde oluşturulur.

1. Ardından, projenizi oluşturmak için **Oluştur** ' u seçin.
1. Oluşturulduktan sonra projenizi seçin.

    - Artık görünümlerinizin yeni oluşturulan özel komutlar uygulamanıza genel bir bakış olması gerekir.

## <a name="update-luis-resources-optional"></a>LUSıS kaynaklarını güncelleştirme (Isteğe bağlı)

Yeni proje penceresinde ayarlanan yazma kaynağını güncelleştirebilir ve bir tahmin kaynağı ayarlayabilirsiniz. Tahmin kaynağı, özel komutlar uygulamanız yayımlandıktan sonra tanıma için kullanılır. Geliştirme ve test aşamaları için bir tahmin kaynağı gerekmez.

1. Sol bölmeden **Ayarlar** ' ı seçin ve ortadaki bölmedeki **lusıs kaynakları** bölümüne gidin.
1. Bir tahmin kaynağı seçin veya **Yeni kaynak oluştur**' u seçerek bir tane oluşturun.
1. **Kaydet**’i seçin.
    
    > [!div class="mx-imgBorder"]
    > ![LUSıS kaynaklarını ayarlama](media/custom-speech-commands/set-luis-resources.png)


> [!NOTE]
> Yazma kaynağı ayda yalnızca 1.000 tahmin uç nokta isteklerini desteklediğinden, özel komutlar uygulamanızı yayımlamadan önce mandatorily bir LUSıS tahmini kaynağı ayarlamanız gerekir.


## <a name="create-a-new-command"></a>Yeni bir komut oluştur

Tek bir utterlik `turn on the tv`alacak ve iletiyle `Ok, turning on the tv`yanıt verecek basit bir komut oluşturalım.

1. En soldaki bölmenin en üstünde bulunan `+ New command` simgeyi seçerek yeni bir komut oluşturun. **Yeni komut**başlıklı yeni bir açılır pencere görünür.
1. **Ad** alanı için değer girin `TurnOn`.
1. **Oluştur**’u seçin.

Orta bölme, bir komutun farklı özelliklerini listeler:


| Yapılandırma            | Açıklama                                                                                                                 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| Örnek cümleler | Kullanıcının bu komutu tetikleyebilmesi için söyledikleri örnek                                                                 |
| Parametreler       | Komutu tamamlaması için gereken bilgiler                                                                                |
| Tamamlama kuralları | Komutu yerine getirmek için gerçekleştirilecek eylemler. Örneğin, kullanıcıya yanıt vermek veya başka bir Web hizmetiyle iletişim kurmak için. |
| Etkileşim kuralları   | Daha belirgin veya karmaşık durumları işlemek için ek kurallar                                                              |


> [!div class="mx-imgBorder"]
> ![Komut oluşturma](media/custom-speech-commands/create-add-command.png)


### <a name="add-example-sentences"></a>Örnek cümleler ekleyin

Örnek cümleler bölümüne başlayalım ve kullanıcının söyleme bir örneğini sağlayın.

1. Orta bölmeden ve en sağdaki bölmedeki **örnek cümleler** ' ı seçin, örnekler ekleyin:

    ```
    turn on the tv
    ```

1. Bu `Save` bölmenin en üstünde bulunan simgesi seçin.

Şimdilik, **tamamlanma kuralları** bölümüne taşıyabilmemiz için parametresiz bir parametre yoktu.

### <a name="add-a-completion-rule"></a>Tamamlama kuralı ekleme

Şimdi aşağıdaki yapılandırmaya sahip bir tamamlama kuralı ekleyin. Bu kural, bir karşılama eyleminin alındığı kullanıcıyı gösterir.


| Ayar    | Önerilen değer                          | Açıklama                                        |
| ---------- | ---------------------------------------- | -------------------------------------------------- |
| Kural Adı  | ConfirmationResponse                  | Kuralın amacını açıklayan bir ad          |
| Koşullar | Yok                                     | Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |
| Eylemler    | SpeechResponse "-Tamam, TV 'yi açma" | Kural koşulu true olduğunda gerçekleştirilecek eylem |

1. Orta bölmenin üst kısmındaki `+Add` simgeyi seçerek yeni bir tamamlanma kuralı oluşturun.
1. **Ad** bölümünde değer sağlayın.
1. Eylem ekleme
   1. **Eylemler** bölümünde **+ Eylem Ekle** ' ye tıklayarak yeni bir eylem oluşturun.
   1. **Yeni eylem** açılır penceresinde, **tür**için açılan Seçenekler `Send speech response` ' i seçin.
   1. Yanıt `Simple editor` alanı için **Response** seçin.
       - **İlk varyasyon** alanında, yanıt için değer belirtin`Ok, turning on the tv`

   > [!div class="mx-imgBorder"]
   > ![Konuşma yanıtı oluşturma](media/custom-speech-commands/create-speech-response-action.png)

1. Kuralı kaydetmek için **Kaydet** ' e tıklayın.
1. **Tamamlama kuralları** bölümüne geri döndüğünüzde tüm değişiklikleri kaydetmek için **Kaydet** ' i seçin. 

> [!div class="mx-imgBorder"]
> ![Tamamlama kuralı oluşturma](media/custom-speech-commands/create-basic-completion-response-rule.png)



## <a name="try-it-out"></a>Deneyin

Test sohbeti panelini kullanarak davranışı test etme
1. Sağ `Train` bölmenin en üstünde bulunan simgesi seçin.
1. Eğitim tamamlandığında, öğesini seçin `Test`. Yeni bir **test, uygulama** pencereniz görüntülenir.
    - Şunu yazın: TV 'yi açın
    - Beklenen yanıt: Tamam, TV 'yi açma


> [!div class="mx-imgBorder"]
> ![Web sohbeti ile test etme](media/custom-speech-commands/create-basic-test-chat.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hızlı başlangıç: parametrelerle özel komut oluşturma (Önizleme)](./quickstart-custom-speech-commands-create-parameters.md)
