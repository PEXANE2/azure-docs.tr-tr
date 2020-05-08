---
title: Konuşma SDK 'Sı ile bir istemciden komutları yerine getirmek
titleSuffix: Azure Cognitive Services
description: Bu makalede, konuşma SDK 'Sı ile bir istemcide özel komut etkinliklerinin nasıl işleneceğini açıkladık.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: trbye
ms.openlocfilehash: f11f5f3c2ad4c9f0241d34edeb664f739f88d15c
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82871763"
---
# <a name="fulfill-commands-from-a-client-with-the-speech-sdk-preview"></a>Konuşma SDK 'Sı (Önizleme) ile bir istemciden komutları yerine getirmek

Özel bir komut uygulaması kullanarak görevleri gerçekleştirmek için, bağlı bir istemci cihazına özel yük gönderebilirsiniz.

Bu makalede şunları yapmanız gerekir:

- Özel komutlar uygulamanızdan özel bir JSON yükü tanımlama ve gönderme
- C# UWP konuşma SDK 'Sı istemci uygulamasından özel JSON yükü içeriğini alma ve görselleştirme

## <a name="prerequisites"></a>Ön koşullar
> [!div class = "checklist"]
> * [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)
> * Konuşma hizmeti için bir Azure abonelik anahtarı: [ücretsiz olarak alın](get-started.md) veya [Azure Portal](https://portal.azure.com) oluşturun
> * Önceden oluşturulmuş özel komutlar uygulaması: [hızlı başlangıç: parametrelerle özel komut oluşturma (Önizleme)](./quickstart-custom-speech-commands-create-parameters.md)
> * Bir konuşma SDK 'Sı etkin istemci uygulaması: [hızlı başlangıç: konuşma SDK 'sı Ile özel bir komut uygulamasına bağlanma (Önizleme)](./quickstart-custom-speech-commands-speech-sdk.md)

## <a name="optional-get-started-fast"></a>İsteğe bağlı: hızlı başlangıç

Bu makalede, adım adım, bir istemci uygulamanın özel komutlar uygulamanızla nasıl konuştuğu nasıl yapılacağı açıklanmaktadır. Hemen ' yi kullanmayı tercih ediyorsanız, bu makalede kullanılan tam, kullanıma hazır kaynak kodu [konuşma SDK örnekleri](https://aka.ms/csspeech/samples)bölümünde bulunur.

## <a name="fulfill-with-json-payload"></a>JSON yükü ile yerine

1. Daha önce hızlı başlangıçlardan oluşturduğunuz özel komutlar uygulamasını açın [: parametrelerle özel komut oluşturma](./quickstart-custom-speech-commands-create-parameters.md)
1. Kullanıcıya geri yanıt veren daha önce oluşturulmuş kurala sahip olduğunuzdan emin olmak için **tamamlama kuralları** bölümüne bakın
1. Doğrudan istemciye yük göndermek için, etkinlik Gönder eylemiyle yeni bir kural oluşturun

   > [!div class="mx-imgBorder"]
   > ![Etkinlik tamamlama kuralını gönder](media/custom-speech-commands/fulfill-sdk-completion-rule.png)

   | Ayar | Önerilen değer | Açıklama |
   | ------- | --------------- | ----------- |
   | Kural Adı | Güncelleştirme Devemlak | Kuralın amacını açıklayan bir ad |
   | Koşullar | Gerekli parametre- `OnOff` ve`SubjectDevice` | Kuralın ne zaman çalıştırılabilmesine belirleme koşulları |
   | Eylemler | `SendActivity`(aşağıya bakın) | Kural koşulu true olduğunda gerçekleştirilecek eylem |

1. Aşağıdaki JSON 'ı **etkinlik içeriğine** Kopyala
   ```json
   {
     "type": "event",
     "name": "UpdateDeviceState",
     "state": "{OnOff}",
     "device": "{SubjectDevice}"
   }
   ```
   > [!div class="mx-imgBorder"]
   > ![Etkinlik yükünü gönder](media/custom-speech-commands/fulfill-sdk-send-activity-action.png)

## <a name="create-visuals-for-device-on-or-off-state"></a>Cihaz üzerinde veya Kapalı durumuna görsel oluşturma

[Hızlı başlangıç: konuşma SDK 'sı Ile özel bir komut uygulamasına bağlanma](./quickstart-custom-speech-commands-speech-sdk.md), gibi komutları `turn on the tv`ele alan bir konuşma SDK 'sı istemci uygulaması oluşturdunuz. `turn off the fan` Bazı görseller eklendikçe, bu komutların sonucunu görebilirsiniz.

Aşağıdaki **XML 'nin eklenmiş olduğunu belirten** metin içeren **Off** etiketli kutular ekleyin`MainPage.xaml`

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

## <a name="handle-customizable-payload"></a>Özelleştirilebilir yükü işle
### <a name="add-reference-libraries"></a>Başvuru kitaplıkları ekleme

Bir JSON yükü oluşturduğunuzdan, seri durumdan çıkarmayı işlemek için [JSON.net](https://www.newtonsoft.com/json) kitaplığına bir başvuru eklemeniz gerekir.
- Çözümünüz için sağ istemci.
- **Çözüm Için NuGet Paketlerini Yönet**' i seçin, **yükler** ' i seçin 
- **Newtonsoft. JSON** için arama yapın listesinde **Microsoft. Netcore. Üniversalwindowsplatform** ' ı en yeni sürüme güncelleştirin

> [!div class="mx-imgBorder"]
> ![Etkinlik yükünü gönder](media/custom-speech-commands/fulfill-sdk-json-nuget.png)

' MainPage. xaml. cs ' içinde, Ekle
- `using Newtonsoft.Json;` 
- `using Windows.ApplicationModel.Core;`

### <a name="handle-received-payload"></a>Tanıtıcı alınan yük

İçinde `InitializeDialogServiceConnector`, `ActivityReceived` olay işleyicisini aşağıdaki kodla değiştirin. Değiştirilen `ActivityReceived` olay işleyicisi, yükü etkinlikten çıkaracaktır ve TV veya fan 'nin görsel durumunu uygun şekilde değiştirir.

```C#
connector.ActivityReceived += async (sender, activityReceivedEventArgs) =>
{
    NotifyUser($"Activity received, hasAudio={activityReceivedEventArgs.HasAudio} activity={activityReceivedEventArgs.Activity}");

    dynamic activity = JsonConvert.DeserializeObject(activityReceivedEventArgs.Activity);
    var name = activity?.name != null ? activity.name.ToString() : string.Empty;

    if (name.Equals("UpdateDeviceState"))
    {
        Debug.WriteLine("Here");
        var state = activity?.device != null ? activity.state.ToString() : string.Empty;
        var device = activity?.device != null ? activity.device.ToString() : string.Empty;

        if (state.Equals("on") || state.Equals("off"))
        {
            switch (device)
            {
                case "tv":
                    await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
                        CoreDispatcherPriority.Normal, () => { State_TV.Text = state; });
                    break;
                case "fan":
                    await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
                        CoreDispatcherPriority.Normal, () => { State_Fan.Text = state; });
                    break;
                default:
                    NotifyUser($"Received request to set unsupported device {device} to {state}");
                    break;
            }
        }
        else { 
            NotifyUser($"Received request to set unsupported state {state}");
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
1. Mikrofonu etkinleştir ' i seçin
1. Konuşma düğmesini seçin
1. Komutunu`turn on the tv`
1. TV 'nin görsel durumu "açık" olarak değiştirilmelidir
   > [!div class="mx-imgBorder"]
   > ![Etkinlik yükünü gönder](media/custom-speech-commands/fulfill-sdk-turn-on-tv.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Nasıl yapılır: özel komut parametrelerine doğrulama ekleme (Önizleme)](./how-to-custom-speech-commands-validations.md)
