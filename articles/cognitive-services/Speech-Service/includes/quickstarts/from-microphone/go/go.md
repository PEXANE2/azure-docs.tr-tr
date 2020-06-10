---
author: trrwilson
ms.service: cognitive-services
ms.topic: include
ms.date: 05/25/2020
ms.author: trrwilson
ms.openlocfilehash: c43194db6d1d629003079db7248223aee52ffa70
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84609649"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:

> [!div class="checklist"]
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı ayarlama ve boş bir proje oluşturma](../../../../quickstarts/setup-platform.md)
> * Ses yakalama için bir mikrofona erişiminizin olduğundan emin olun

## <a name="setup-your-environment"></a>Ortamınızı ayarlama

Bu satırı ekleyerek go. Mod dosyasını en son SDK sürümüyle güncelleştirin
```sh
require (
    github.com/Microsoft/cognitive-services-speech-sdk-go v1.12.1
)
```

## <a name="start-with-some-boilerplate-code"></a>Bazı demirbaş kodla başlayın
1. Kaynak dosyanızın içeriğini (ör. `sr-quickstart.go` ) aşağıdaki gibi değiştirin:

- "ana" paket tanımı
- Konuşma SDK 'sından gerekli modüller içeri aktarılıyor
- Bu hızlı başlangıçta daha sonra verilecek abonelik bilgilerini depolamak için değişkenler
- Ses girişi için mikrofonu kullanan basit bir uygulama
- konuşma tanıma sırasında gerçekleşen çeşitli olaylara yönelik olay işleyicileri

```sh
package recognizer

import (
    "bufio"
    "fmt"
    "os"

    "github.com/Microsoft/cognitive-services-speech-sdk-go/audio"
    "github.com/Microsoft/cognitive-services-speech-sdk-go/speech"
)

func recognizingHandler(event speech.SpeechRecognitionEventArgs) {
    defer event.Close()
    fmt.Println("Recognizing:", event.Result.Text)
}

func recognizedHandler(event speech.SpeechRecognitionEventArgs) {
    defer event.Close()
    fmt.Println("Recognized:", event.Result.Text)
}

func cancelledHandler(event speech.SpeechRecognitionCanceledEventArgs) {
    defer event.Close()
    fmt.Println("Received a cancellation: ", event.ErrorDetails)
}

func main() {
    subscription :=  "YOUR_SUBSCRIPTION_KEY"
    region := "YOUR_SUBSCRIPTIONKEY_REGION"

    audioConfig, err := audio.NewAudioConfigFromDefaultMicrophoneInput()
    if err != nil {
        fmt.Println("Got an error: ", err)
        return
    }
    defer audioConfig.Close()
    config, err := speech.NewSpeechConfigFromSubscription(subscription, region)
    if err != nil {
        fmt.Println("Got an error: ", err)
        return
    }
    defer config.Close()
    speechRecognizer, err := speech.NewSpeechRecognizerFromConfig(config, audioConfig)
    if err != nil {
        fmt.Println("Got an error: ", err)
        return
    }
    defer speechRecognizer.Close()
    speechRecognizer.Recognizing(recognizingHandler)
    speechRecognizer.Recognized(recognizedHandler)
    speechRecognizer.Canceled(cancelledHandler)
    speechRecognizer.StartContinuousRecognitionAsync()
    defer speechRecognizer.StopContinuousRecognitionAsync()
    bufio.NewReader(os.Stdin).ReadBytes('\n')
}
```

`YOUR_SUBSCRIPTION_KEY`Ve `YOUR_SUBSCRIPTIONKEY_REGION` değerlerini konuşma kaynağından gerçek değerlerle değiştirin.

- Azure portal gidin ve konuşma kaynağını açın
- Sol taraftaki **anahtarlar** altında, kullanılabilir iki abonelik anahtarı vardır
    - Değer değiştirme olarak bir tane kullanın `YOUR_SUBSCRIPTION_KEY`
- Sol taraftaki **genel bakış** altında, bölgeye göz atın ve bölge tanımlayıcısıyla eşleyin
- Bölge tanımlayıcısını değer değiştirme olarak kullanın `YOUR_SUBSCRIPTIONKEY_REGION` , örneğin: `"westus"` **Batı ABD** için

## <a name="code-explanation"></a>Kod açıklaması
Konuşma yapılandırma nesnesi oluşturmak için konuşma aboneliği anahtarı ve bölgesi gereklidir. Yapılandırma nesnesi, bir konuşma tanıyıcı nesnesinin örneğini oluşturmak için gereklidir.

Tanıyıcı örneği, konuşmayı tanımak için birçok yol sunar. Bu örnekte, konuşma sürekli olarak tanınır. Bu işlevsellik, konuşma hizmetinin tanıma için çok sayıda tümcecik gönderdiğini ve programın konuşmayı tanımayı durdurmak için ne zaman sonlandırdığını bilmesini sağlar. Sonuçlar sonuçlandırılan, kod bunları konsola yazar.

## <a name="build-and-run"></a>Derleme ve çalıştırma
Artık, konuşma hizmetini kullanarak projenizi oluşturmak ve konuşma tanımayı test etmek için hazırsınız.
1. Projenizi derleyin, ör. **"derlemeyi git"**
2. Modülü çalıştırın ve cihazınızın mikrofonuna bir tümcecik veya tümce konuşun. Konuşma, konuşma hizmetine iletilir ve çıktıda görüntülenen metne aktarılır.


> [!NOTE]
> Konuşma SDK 'Sı, dil için en-US kullanarak varsayılan olarak tanıma yapılır, kaynak dili seçme hakkında bilgi için bkz. [konuşmayı için kaynak dilini belirtme](../../../../how-to-specify-source-language.md) .


## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
