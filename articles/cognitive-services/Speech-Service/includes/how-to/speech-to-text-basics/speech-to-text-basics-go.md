---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 09/15/2020
ms.author: trbye
ms.openlocfilehash: bced384e8ba88fb83499e78c4e0d60e811ae32df
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99213811"
---
Konuşma hizmetinin temel özelliklerinden biri de insan konuşmanızı tanıyabilme ve (genellikle konuşma-metin olarak adlandırılır). Bu hızlı başlangıçta, uygulama ve ürünlerinize yönelik konuşma SDK 'sını kullanarak yüksek kaliteli bir konuşmayı metne dönüştürme işlemini nasıl gerçekleştireceğinizi öğreneceksiniz.

## <a name="skip-to-samples-on-github"></a>GitHub 'da örneklere atlayın

Örnek koda doğrudan atlamak istiyorsanız, GitHub 'daki [Go hızlı başlangıç örneklerine](https://github.com/microsoft/cognitive-services-speech-sdk-go/tree/master/samples/recognizer) bakın.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede bir Azure hesabınız ve konuşma hizmeti aboneliğiniz olduğunu varsaymaktadır. Hesabınız ve aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](../../../overview.md#try-the-speech-service-for-free).

## <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

Herhangi bir şey yapabilmeniz [için önce go Için konuşma SDK 'sını](../../../quickstarts/setup-platform.md?pivots=programming-language-go&tabs=dotnet%252cwindows%252cjre%252cbrowser)yüklemeniz gerekir.

## <a name="speech-to-text-from-microphone"></a>Mikrofondan konuşmaya metin

Konuşma tanımayı varsayılan cihaz mikrofonunuzdan çalıştırmak için aşağıdaki kod örneğini kullanın. Değişkenleri `subscription` ve `region` abonelik ve bölge anahtarlarınız ile değiştirin. Betiği çalıştırmak, varsayılan mikrofonunuzun ve çıkış metninde bir tanıma oturumu başlatır.

```go
package main

import (
    "bufio"
    "fmt"
    "os"

    "github.com/Microsoft/cognitive-services-speech-sdk-go/audio"
    "github.com/Microsoft/cognitive-services-speech-sdk-go/speech"
)

func sessionStartedHandler(event speech.SessionEventArgs) {
    defer event.Close()
    fmt.Println("Session Started (ID=", event.SessionID, ")")
}

func sessionStoppedHandler(event speech.SessionEventArgs) {
    defer event.Close()
    fmt.Println("Session Stopped (ID=", event.SessionID, ")")
}

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
    speechRecognizer.SessionStarted(sessionStartedHandler)
    speechRecognizer.SessionStopped(sessionStoppedHandler)
    speechRecognizer.Recognizing(recognizingHandler)
    speechRecognizer.Recognized(recognizedHandler)
    speechRecognizer.Canceled(cancelledHandler)
    speechRecognizer.StartContinuousRecognitionAsync()
    defer speechRecognizer.StopContinuousRecognitionAsync()
    bufio.NewReader(os.Stdin).ReadBytes('\n')
}
```

GitHub 'da barındırılan bileşenlere bağlanan bir go. mod dosyası oluşturmak için aşağıdaki komutları çalıştırın.

```cmd
go mod init quickstart
go get github.com/Microsoft/cognitive-services-speech-sdk-go
```

Şimdi kodu derleyin ve çalıştırın.

```cmd
go build
go run quickstart
```

Ve sınıfları hakkında ayrıntılı bilgi için başvuru belgelerine bakın [`SpeechConfig`](https://pkg.go.dev/github.com/Microsoft/cognitive-services-speech-sdk-go@v1.15.0/speech#SpeechConfig) [`SpeechRecognizer`](https://pkg.go.dev/github.com/Microsoft/cognitive-services-speech-sdk-go@v1.15.0/speech#SpeechRecognizer) .

## <a name="speech-to-text-from-audio-file"></a>Ses dosyasından konuşmayı metne dönüştürme

Konuşma tanımayı bir ses dosyasından çalıştırmak için aşağıdaki örneği kullanın. Değişkenleri `subscription` ve `region` abonelik ve bölge anahtarlarınız ile değiştirin. Ayrıca, değişkenini bir `file` . wav dosyası yoluyla değiştirin. Betiği çalıştırmak dosyadaki konuşmayı algılar ve metin sonucunu çıktı olarak görür.

```go
package main

import (
    "fmt"
    "time"

    "github.com/Microsoft/cognitive-services-speech-sdk-go/audio"
    "github.com/Microsoft/cognitive-services-speech-sdk-go/speech"
)

func main() {
    subscription :=  "YOUR_SUBSCRIPTION_KEY"
    region := "YOUR_SUBSCRIPTIONKEY_REGION"
    file := "path/to/file.wav"

    audioConfig, err := audio.NewAudioConfigFromWavFileInput(file)
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
    speechRecognizer.SessionStarted(func(event speech.SessionEventArgs) {
        defer event.Close()
        fmt.Println("Session Started (ID=", event.SessionID, ")")
    })
    speechRecognizer.SessionStopped(func(event speech.SessionEventArgs) {
        defer event.Close()
        fmt.Println("Session Stopped (ID=", event.SessionID, ")")
    })

    task := speechRecognizer.RecognizeOnceAsync()
    var outcome speech.SpeechRecognitionOutcome
    select {
    case outcome = <-task:
    case <-time.After(5 * time.Second):
        fmt.Println("Timed out")
        return
    }
    defer outcome.Close()
    if outcome.Error != nil {
        fmt.Println("Got an error: ", outcome.Error)
    }
    fmt.Println("Got a recognition!")
    fmt.Println(outcome.Result.Text)
}
```

GitHub 'da barındırılan bileşenlere bağlanan bir go. mod dosyası oluşturmak için aşağıdaki komutları çalıştırın.

```cmd
go mod init quickstart
go get github.com/Microsoft/cognitive-services-speech-sdk-go
```

Şimdi kodu derleyin ve çalıştırın.

```cmd
go build
go run quickstart
```

Ve sınıfları hakkında ayrıntılı bilgi için başvuru belgelerine bakın [`SpeechConfig`](https://pkg.go.dev/github.com/Microsoft/cognitive-services-speech-sdk-go@v1.15.0/speech#SpeechConfig) [`SpeechRecognizer`](https://pkg.go.dev/github.com/Microsoft/cognitive-services-speech-sdk-go@v1.15.0/speech#SpeechRecognizer) .