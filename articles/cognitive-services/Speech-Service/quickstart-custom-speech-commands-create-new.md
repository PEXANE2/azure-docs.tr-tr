---
title: 'Quickstart: Özel Komut oluşturma (Önizleme) - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede, barındırılan özel komutlar uygulaması oluşturmak ve sına.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/09/2019
ms.author: donkim
ms.openlocfilehash: 4ae8f13b4887bbc41b17defa3f9a20c07ed0cb45
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76155596"
---
# <a name="quickstart-create-a-custom-command-preview"></a>Quickstart: Özel Komut Oluşturma (Önizleme)

Bu makalede, barındırılan özel komutlar uygulamasını nasıl oluşturup test edeceğinizi öğreneceksiniz.
Uygulama "tv açmak" gibi bir söyleyiş tanıyacak ve basit bir mesaj ile yanıt verecek "Tamam, tv açma".

## <a name="prerequisites"></a>Ön koşullar

- Konuşma aboneliği.

Konuşma aboneliğiniz yoksa, [Konuşma Stüdyosu'na](https://speech.microsoft.com/) gidip **Konuşma Kaynağı Oluştur'u**seçerek bir konuşma oluşturabilirsiniz.

  > [!div class="mx-imgBorder"]
  > [![Proje](media/custom-speech-commands/create-new-subscription.png) oluşturma](media/custom-speech-commands/create-new-subscription.png#lightbox)

  > [!NOTE]
  > Önizleme sırasında sadece westus2 bölgesi desteklenir.

## <a name="go-to-the-speech-studio-for-custom-commands"></a>Özel Komutlar için Konuşma Stüdyosuna gidin

1. Web tarayıcınızı açın ve [Konuşma Stüdyosu'na](https://speech.microsoft.com/) gidin
1. Portalda oturum açmanız için kimlik bilgilerinizi girin

   - Varsayılan görünüm, Konuşma abonelikleri listenizdir
     > [!NOTE]
     > Seçili abonelik sayfasını görmüyorsanız, üst çubuktaki ayarlar menüsünden "Konuşma kaynakları" seçeneğini seçerek oraya gidebilirsiniz.

1. Konuşma aboneliğinizi seçin ve ardından **Stüdyoya Git'i** seçin
1. Özel Komutları Seçin **(Önizleme)**

Varsayılan görünüm, oluşturduğunuz Özel Komutuygulamalarının listesidir.

## <a name="create-a-custom-commands-project"></a>Özel Komutlar projesi oluşturma

1. Yeni bir proje oluşturmak için **Yeni projeyi** seçin

   > [!div class="mx-imgBorder"]
   > ![Proje oluşturma](media/custom-speech-commands/create-new-project.png)

1. Proje adını ve dilini girin.
1. Bir yazma kaynağı seçin. Geçerli bir yazarlık kaynağı yoksa, yeni **kaynak oluştur'u**seçerek bir tane oluşturun.

   > [!div class="mx-imgBorder"]
   > ![Kaynak oluşturma](media/custom-speech-commands/create-new-resource.png)

   1. Kaynak adını, grubu, konumu ve fiyatlandırma katmanını girin.

         > [!NOTE]
         > İstediğiniz kaynak grubu adını "Kaynak Grubu" alanına girerek kaynak grupları oluşturabilirsiniz. **Create** seçildiğinde kaynak grubu oluşturulur.

1. Projenizi oluşturmak için **Oluştur'u** tıklatın.
1. Oluşturulduktan sonra projenizi seçin.

Görünümünüz artık Özel Komutlar uygulamanızın genel görünümü olmalıdır.

## <a name="update-luis-resources-optional"></a>LUIS Kaynaklarını Güncelleştir (İsteğe Bağlı)

Yeni proje penceresinde yazarlık kaynağı kümesini güncelleştirebilir ve çalışma zamanında girişleri tanımak için kullanılan bir tahmin kaynağı ayarlayabilirsiniz.

> [!NOTE]
> Uygulamanız, yazar kaynak tarafından sağlanan 1.000 isteğin ötesinde öngörüler talep etmeden önce bir tahmin kaynağı ayarlamanız gerekir.

> [!div class="mx-imgBorder"]
> ![LUIS Kaynaklarını Ayarla](media/custom-speech-commands/set-luis-resources.png)

1. Sol bölmeden **Ayarlar'ı** seçerek LUIS Kaynakları bölmesine gidin ve ardından orta bölmeden **LUIS Kaynakları'nı** seçin.
1. Bir tahmin kaynağı seçin veya **yeni kaynak oluştur'u** seçerek bir kaynak oluşturun
1. **Kaydet**’i seçin

## <a name="create-a-new-command"></a>Yeni bir Komut oluşturma

Artık bir Komut oluşturabilirsiniz. Tek bir sözcük `turn on the tv`alacak bir örnek kullanalım ve mesajla `Ok, turning on the TV`yanıt verelim.

1. Komutların yanındaki simgeyi `+` seçerek yeni bir Komut oluşturun ve ona ad verin`TurnOn`
1. **Kaydet**’i seçin

> [!div class="mx-imgBorder"]
> ![Komut oluşturma](media/custom-speech-commands/create-add-command.png)

Komut bir kümedir:

| Grup            | Açıklama                                                                                                                 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| Örnek Cümleler | Kullanıcının bu Komutu tetiklemek için söyleyebileceği örnek söylenmeler                                                                 |
| Parametreler       | Komutanlığı tamamlamak için gerekli bilgiler                                                                                |
| Tamamlama Kuralları | Komutanlığı yerine getirmek için yapılacak işlemler. Örneğin, kullanıcıya yanıt vermek veya başka bir web hizmetiyle iletişim kurmak için |
| Gelişmiş Kurallar   | Daha spesifik veya karmaşık durumları işlemek için ek kurallar                                                              |

### <a name="add-a-sample-sentence"></a>Örnek Cümle Ekle

Örnek Cümlelerile başlayalım ve kullanıcının söyleyebileceği nelere bir örnek verelim:

```
turn on the tv
```

Şimdilik, Tamamlama Kuralları'na geçebilmemiz için parametrelerimiz yok.

### <a name="add-a-completion-rule"></a>Tamamlama Kuralı Ekleme

Şimdi, bir eylemin yapıldığını belirten kullanıcıya yanıt vermek için bir Tamamlama Kuralı ekleyin.

1. Tamamlama Kuralları'nın yanındaki simgeyi `+` seçerek yeni bir Tamamlama Kuralı oluşturma
1. Kural adını girin
1. Eylem ekleme
   1. Eylemler'in yanındaki simgeyi `+` seçerek yeni bir Konuşma Yanıtı Eylemi oluşturun ve`SpeechResponse`
   1. Yanıtı girin

   > [!NOTE]
   > Normal metin bir tire ile başlamalıdır. Daha fazla bilgi için [buraya](https://aka.ms/sc-lg-format) gidin

   > [!div class="mx-imgBorder"]
   > ![Konuşma yanıtı oluşturma](media/custom-speech-commands/create-speech-response-action.png)

1. Kuralı kaydetmek için **Kaydet'i** tıklatın

> [!div class="mx-imgBorder"]
> ![Tamamlama kuralı oluşturma](media/custom-speech-commands/create-basic-completion-response-rule.png)

| Ayar    | Önerilen değer                          | Açıklama                                        |
| ---------- | ---------------------------------------- | -------------------------------------------------- |
| Kural Adı  | "Onay Yanıtı"                   | Kuralın amacını açıklayan bir ad          |
| Koşullar | None                                     | Kuralın ne zaman çalıştırılacakolabileceğini belirleyen koşullar    |
| Eylemler    | SpeechResponse "- Tamam, TV'yi açmak" | Kural koşulu doğru olduğunda yapılacak eylem |

## <a name="try-it-out"></a>Deneyin

Test sohbet panelini kullanarak davranışı test edin.

> [!div class="mx-imgBorder"]
> ![Web sohbeti ile test edin](media/custom-speech-commands/create-basic-test-chat.png)

- Yazın: "televizyonu açın"
- Beklenen yanıt: "Tamam, tv açma"

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Quickstart: Parametreler (Önizleme) ile Özel Komut oluşturma](./quickstart-custom-speech-commands-create-parameters.md)
