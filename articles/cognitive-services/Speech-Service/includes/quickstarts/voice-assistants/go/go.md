---
author: trrwilson
ms.service: cognitive-services
ms.topic: include
ms.date: 05/25/2020
ms.author: trrwilson
ms.openlocfilehash: ee6b6c29c89c39c72144371af8268760da843170
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87375229"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:

> [!div class="checklist"]
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurun ve boş bir proje oluşturun](../../../../quickstarts/setup-platform.md)
> * [Doğrudan hat konuşma kanalına](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech) bağlı bir bot oluşturma
> * Ses yakalama için bir mikrofona erişiminizin olduğundan emin olun
>
  > [!NOTE]
  > Lütfen [sesli Yardımcılar için desteklenen bölgeler listesine](~/articles/cognitive-services/speech-service/regions.md#voice-assistants) başvurun ve kaynaklarınızın bu bölgelerden birinde dağıtıldığından emin olun.

## <a name="setup-your-environment"></a>Ortamınızı ayarlama

Bu satırı ekleyerek go. Mod dosyasını en son SDK sürümüyle güncelleştirin
```sh
require (
    github.com/Microsoft/cognitive-services-speech-sdk-go v1.13.0
)
```

## <a name="start-with-some-boilerplate-code"></a>Bazı demirbaş kodla başlayın
Kaynak dosyanızın içeriğini (ör. `quickstart.go` ) aşağıdaki gibi değiştirin:

- "ana" paket tanımı
- Konuşma SDK 'sından gerekli modüller içeri aktarılıyor
- Bu hızlı başlangıçta daha sonra verilecek bot bilgilerini depolamak için değişkenler
- Ses girişi için mikrofonu kullanan basit bir uygulama
- konuşma etkileşimi sırasında gerçekleşen çeşitli olaylara yönelik olay işleyicileri

```sh
package main

import (
    "fmt"
    "time"

    "github.com/Microsoft/cognitive-services-speech-sdk-go/audio"
    "github.com/Microsoft/cognitive-services-speech-sdk-go/dialog"
    "github.com/Microsoft/cognitive-services-speech-sdk-go/speech"
)

func main() {
    subscription :=  "YOUR_SUBSCRIPTION_KEY"
    region := "YOUR_BOT_REGION"

    audioConfig, err := audio.NewAudioConfigFromDefaultMicrophoneInput()
    if err != nil {
        fmt.Println("Got an error: ", err)
        return
    }
    defer audioConfig.Close()
    config, err := dialog.NewBotFrameworkConfigFromSubscription(subscription, region)
    if err != nil {
        fmt.Println("Got an error: ", err)
        return
    }
    defer config.Close()
    connector, err := dialog.NewDialogServiceConnectorFromConfig(config, audioConfig)
    if err != nil {
        fmt.Println("Got an error: ", err)
        return
    }
    defer connector.Close()
    activityReceivedHandler := func(event dialog.ActivityReceivedEventArgs) {
        defer event.Close()
        fmt.Println("Received an activity.")
    }
    connector.ActivityReceived(activityReceivedHandler)
    recognizedHandle := func(event speech.SpeechRecognitionEventArgs) {
        defer event.Close()
        fmt.Println("Recognized ", event.Result.Text)
    }
    connector.Recognized(recognizedHandle)
    recognizingHandler := func(event speech.SpeechRecognitionEventArgs) {
        defer event.Close()
        fmt.Println("Recognizing ", event.Result.Text)
    }
    connector.Recognizing(recognizingHandler)
    connector.ListenOnceAsync()
    <-time.After(10 * time.Second)
}
```

`YOUR_SUBSCRIPTION_KEY`Ve `YOUR_BOT_REGION` değerlerini konuşma kaynağından gerçek değerlerle değiştirin.

- Azure portal gidin ve konuşma kaynağını açın
- Sol taraftaki **anahtarlar ve uç nokta** altında, kullanılabilir iki abonelik anahtarı vardır
    - Değer değiştirme olarak bir tane kullanın `YOUR_SUBSCRIPTION_KEY`
- Sol taraftaki **genel bakış** altında, bölgeye göz atın ve bölge tanımlayıcısıyla eşleyin
    - Bölge tanımlayıcısını değer değiştirme olarak kullanın `YOUR_BOT_REGION` , örneğin: `"westus"` **Batı ABD** için

   > [!NOTE]
   > Lütfen [sesli Yardımcılar için desteklenen bölgeler listesine](~/articles/cognitive-services/speech-service/regions.md#voice-assistants) başvurun ve kaynaklarınızın bu bölgelerden birinde dağıtıldığından emin olun.

   > [!NOTE]
   > Botunuzu yapılandırma hakkında daha fazla bilgi için, [doğrudan hat konuşma kanalının](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)bot Framework belgelerine bakın.

## <a name="code-explanation"></a>Kod açıklaması
Konuşma yapılandırma nesnesi oluşturmak için konuşma aboneliği anahtarı ve bölgesi gereklidir. Yapılandırma nesnesi, bir konuşma tanıyıcı nesnesinin örneğini oluşturmak için gereklidir.

Tanıyıcı örneği, konuşmayı tanımak için birçok yol sunar. Bu örnekte, konuşma sürekli olarak tanınır. Bu işlevsellik, konuşma hizmetinin tanıma için çok sayıda tümcecik gönderdiğini ve programın konuşmayı tanımayı durdurmak için ne zaman sonlandırdığını bilmesini sağlar. Sonuçlar sonuçlandırılan, kod bunları konsola yazar.

## <a name="build-and-run"></a>Derleme ve çalıştırma
Artık projeyi derlemek ve konuşma hizmetini kullanarak özel ses yardımcınızı test etmek için hazırsınız.
1. Projenizi derleyin, ör. **"derlemeyi git"**
2. Modülü çalıştırın ve cihazınızın mikrofonuna bir tümcecik veya tümce konuşun. Konuşma, doğrudan hat konuşma kanalına iletilir ve çıkış olarak görünen metne gönderilir.


> [!NOTE]
> Konuşma SDK 'Sı, dil için en-US kullanarak varsayılan olarak tanıma yapılır, kaynak dili seçme hakkında bilgi için bkz. [konuşmayı için kaynak dilini belirtme](../../../../how-to-specify-source-language.md) .

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
