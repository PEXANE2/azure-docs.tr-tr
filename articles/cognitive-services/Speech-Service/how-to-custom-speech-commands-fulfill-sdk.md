---
title: Konuşma SDK ile bir müşteriden komutları yerine getirmek için nasıl
titleSuffix: Azure Cognitive Services
description: Bu makalede, Konuşma SDK ile bir istemci üzerinde Özel Komutlar faaliyetleri nasıl ele açıklayacağız.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/12/2020
ms.author: donkim
ms.openlocfilehash: e109955774722da7f55defe1417de35ff202cce8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79367758"
---
# <a name="fulfill-commands-from-a-client-with-the-speech-sdk-preview"></a>Konuşma SDK (Önizleme) ile bir istemciden komutları yerine getirin

Özel Komutlar uygulamasını kullanarak görevleri tamamlamak için bağlı bir istemci aygıtına özel yükler gönderebilirsiniz.

Bu makalede, şunları yapacaksınız:

- Özel Komutlar uygulamanızdan özel bir JSON yükü tanımlayın ve gönderin
- C# UWP Speech SDK istemci uygulamasından özel JSON yük içeriğini alın ve görselleştirin

## <a name="prerequisites"></a>Ön koşullar

- [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)
- Konuşma hizmeti için Azure abonelik anahtarı
  - [Ücretsiz bir tane alın](get-started.md) veya [Azure portalında](https://portal.azure.com) oluşturun
- Daha önce oluşturulmuş bir Özel Komutuygulaması
  - [Quickstart: Parametreler (Önizleme) ile Özel Komut oluşturma](./quickstart-custom-speech-commands-create-parameters.md)
- Konuşma SDK istemci uygulamasını etkinleştirildi
  - [Quickstart: Konuşma SDK (Önizleme) ile Özel Komut uygulamasına bağlanın](./quickstart-custom-speech-commands-speech-sdk.md)

## <a name="optional-get-started-fast"></a>İsteğe bağlı: Hızlı başlayın

Bu makalede, adım adım, Özel Komutlar uygulama konuşmak için bir istemci uygulaması yapmak nasıl açıklanır. Sağa dalmayı tercih ederseniz, bu makalede kullanılan tam, derlenmeye hazır kaynak kodu [Konuşma SDK Örnekleri'nde](https://aka.ms/csspeech/samples)mevcuttur.

## <a name="fulfill-with-json-payload"></a>JSON yükü ile yerine getirin

1. [Konuşma Stüdyosundan](https://speech.microsoft.com/) daha önce oluşturulmuş Özel Komutlar uygulamanızı açın
1. Kullanıcıya yanıt veren daha önce oluşturulmuş kuralın olduğundan emin olmak için **Tamamlanma Kuralları** bölümünü denetleyin
1. Bir yükü doğrudan istemciye göndermek için, Etkinlik Gönder eylemiyle yeni bir kural oluşturun

   > [!div class="mx-imgBorder"]
   > ![Etkinlik tamamlama kuralı nı gönder](media/custom-speech-commands/fulfill-sdk-completion-rule.png)

   | Ayar | Önerilen değer | Açıklama |
   | ------- | --------------- | ----------- |
   | Kural Adı | GüncellemeDeviceState | Kuralın amacını açıklayan bir ad |
   | Koşullar | Gerekli Parametre `OnOff` - ve`SubjectDevice` | Kuralın ne zaman çalıştırılacakolabileceğini belirleyen koşullar |
   | Eylemler | `SendActivity`(aşağıya bakın) | Kural koşulu doğru olduğunda yapılacak eylem |

   > [!div class="mx-imgBorder"]
   > ![Etkinlik yükünü Gönder](media/custom-speech-commands/fulfill-sdk-send-activity-action.png)

   ```json
   {
     "type": "event",
     "name": "UpdateDeviceState",
     "state": "{OnOff}",
     "device": "{SubjectDevice}"
   }
   ```

## <a name="create-visuals-for-device-on-or-off-state"></a>Durum içinde veya dışında aygıt için görsel oluşturma

[Quickstart: Konuşma SDK (Önizleme) ile özel komut uygulamasına bağlanın](./quickstart-custom-speech-commands-speech-sdk.md) gibi `turn on the tv`komutları işleyen bir `turn off the fan`Konuşma SDK istemci uygulaması oluşturdunuz. Şimdi bu komutların sonucunu görebilmeniz için bazı görseller ekleyin.

Aşağıdaki XML'yi kullanarak **Açık** veya **Kapalı'yı** gösteren metin içeren etiketli kutular ekleme`MainPage.xaml.cs`

```xml
<StackPanel Orientation="Horizontal" HorizontalAlignment="Center" Margin="20">
    <Grid x:Name="Grid_TV" Margin="50, 0" Width="100" Height="100" Background="LightBlue">
        <StackPanel>
            <TextBlock Text="TV" Margin="0, 10" TextAlignment="Center"/>
            <TextBlock x:Name="State_TV" Text="Off" TextAlignment="Center"/>
        </StackPanel>
    </Grid>
    <Grid x:Name="Grid_Fan" Margin="50, 0" Width="100" Height="100" Background="LightBlue">
        <StackPanel>
            <TextBlock Text="Fan" Margin="0, 10" TextAlignment="Center"/>
            <TextBlock x:Name="State_Fan" Text="Off" TextAlignment="Center"/>
        </StackPanel>
    </Grid>
</StackPanel>
```

## <a name="handle-customizable-payload"></a>Özelleştirilebilir yükü işleme

Artık bir JSON yükü oluşturduğunuza göre, seri oluşturmayı işlemek için [JSON.NET](https://www.newtonsoft.com/json) kitaplığına bir başvuru ekleyebilirsiniz.

> [!div class="mx-imgBorder"]
> ![Etkinlik yükünü Gönder](media/custom-speech-commands/fulfill-sdk-json-nuget.png)

Olay `InitializeDialogServiceConnector` işleyicinize `ActivityReceived` aşağıdakileri ekleyin. Ek kod, yükü etkinlikten ayıklar ve buna göre televizyonun veya fanın görsel durumunu değiştirir.

```C#
connector.ActivityReceived += async (sender, activityReceivedEventArgs) =>
{
    NotifyUser($"Activity received, hasAudio={activityReceivedEventArgs.HasAudio} activity={activityReceivedEventArgs.Activity}");

    dynamic activity = JsonConvert.DeserializeObject(activityReceivedEventArgs.Activity);

    if(activity?.name == "SetDeviceState")
    {
        var state = activity?.state;
        var device = activity?.device;
        switch(device)
        {
            case "tv":
                State_TV.Text = state;
                break;
            case "fan":
                State_Fan.Text = state;
                break;
            default:
                NotifyUser($"Received request to set unsupported device {device} to {state}");
                break;
        }
    }

    if (activityReceivedEventArgs.HasAudio)
    {
        SynchronouslyPlayActivityAudio(activityReceivedEventArgs.Audio);
    }
};
```

## <a name="try-it-out"></a>Deneyin

1. Uygulamayı başlatma
1. Mikrofonu Etkinleştir'i seçin
1. Konuş düğmesini seçin
1. Söyle`turn on the tv`
1. Televizyonun görsel durumu "On" olarak değiştirilmelidir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Nasıl yapılır: Özel Komut parametrelerine doğrulama ekleme (önizleme)](./how-to-custom-speech-commands-validations.md)
