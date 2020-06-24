---
title: İstemci uygulamasına özel komutlar etkinliği gönder
titleSuffix: Azure Cognitive Services
description: Bu makalede, özel komutlar uygulamasından konuşma SDK 'sını çalıştıran bir istemci uygulamasına nasıl etkinlik gönderileceğini öğreneceksiniz.
services: cognitive-services
author: xiaojul
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: xiaojul
ms.openlocfilehash: 0a3e3455615006c0e93cf32eebcdaedac9960a79
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85307922"
---
# <a name="send-custom-commands-activity-to-client-application"></a>İstemci uygulamasına özel komutlar etkinliği gönder

Bu makalede, özel komutlar uygulamasından konuşma SDK 'sını çalıştıran bir istemci uygulamasına nasıl etkinlik gönderileceğini öğreneceksiniz.

Aşağıdaki görevleri tamamlayabilirsiniz:

- Özel komutlar uygulamanızdan özel bir JSON yükü tanımlama ve gönderme
- C# UWP konuşma SDK 'Sı istemci uygulamasından özel JSON yükü içeriğini alma ve görselleştirme

## <a name="prerequisites"></a>Ön koşullar
> [!div class = "checklist"]
> * [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)
> * Konuşma hizmeti için bir Azure abonelik anahtarı: [ücretsiz olarak alın](get-started.md) veya [Azure Portal](https://portal.azure.com) oluşturun
> * Önceden [oluşturulmuş özel komutlar uygulaması](quickstart-custom-commands-application.md)
> * Bir konuşma SDK 'Sı etkin istemci uygulaması: [nasıl yapılır: konuşma SDK 'sını kullanarak istemci uygulamasıyla tümleştirme](./how-to-custom-commands-setup-speech-sdk.md)

## <a name="setup-send-activity-to-client"></a>Etkinliği istemciye Gönder ' i ayarla 
1. Daha önce oluşturduğunuz özel komutlar uygulamasını açın
1. **Turnonoff** komutunu seçin, tamamlanma kuralı altında **ConfirmationResponse** öğesini seçin, sonra **Eylem Ekle** ' yi seçin.
1. **Yeni eylem türü**altında, **etkinliği istemciye gönder** ' i seçin
1. Aşağıdaki JSON 'ı **etkinlik içeriğine** Kopyala
   ```json
   {
     "type": "event",
     "name": "UpdateDeviceState",
     "state": "{OnOff}",
     "device": "{SubjectDevice}"
   }
   ```
1. Etkinlik Gönder eylemiyle yeni bir kural oluşturmak için **Kaydet** ' e tıklayın

   > [!div class="mx-imgBorder"]
   > ![Etkinlik tamamlama kuralını gönder](media/custom-commands/send-activity-to-client-completion-rules.png)

## <a name="integrate-with-client-application"></a>İstemci uygulamasıyla tümleştirin

[Nasıl yapılır: istemci uygulamasını konuşma SDK 'sı (Önizleme) Ile ayarlama](./how-to-custom-commands-setup-speech-sdk.md)bölümünde, konuşma SDK 'sı ile gibi komutları ele alan bir UWP istemci uygulaması oluşturdunuz `turn on the tv` `turn off the fan` . Bazı görseller eklendikçe, bu komutların sonucunu görebilirsiniz.

Aşağıdaki **XML 'nin eklenmiş olduğunu belirten** metin içeren **off** etiketli kutular ekleyin`MainPage.xaml`

```xml
<StackPanel Orientation="Horizontal" HorizontalAlignment="Center" Margin="20">
    <Grid x:Name="Grid_TV" Margin="50, 0" Width="100" Height="100" Background="LightBlue">
        <StackPanel>
            <TextBlock Text="TV" Margin="0, 10" TextAlignment="Center"/>
            <TextBlock x:Name="State_TV" Text="off" TextAlignment="Center"/>
        </StackPanel>
    </Grid>
    <Grid x:Name="Grid_Fan" Margin="50, 0" Width="100" Height="100" Background="LightBlue">
        <StackPanel>
            <TextBlock Text="Fan" Margin="0, 10" TextAlignment="Center"/>
            <TextBlock x:Name="State_Fan" Text="off" TextAlignment="Center"/>
        </StackPanel>
    </Grid>
</StackPanel>
```

### <a name="add-reference-libraries"></a>Başvuru kitaplıkları ekleme

Bir JSON yükü oluşturduğunuzdan, seri durumdan çıkarmayı işlemek için [JSON.net](https://www.newtonsoft.com/json) kitaplığına bir başvuru eklemeniz gerekir.

1. Çözümünüz için sağ istemci.
1. **Çözüm Için NuGet Paketlerini Yönet**' i seçin, **yükler** ' i seçin 
1. Güncelleştirme listesinde **Newtonsoft.js** arama yapın, **Microsoft. Netcore. Üniversalwindowsplatform** 'U en yeni sürüme güncelleştirin

> [!div class="mx-imgBorder"]
> ![Etkinlik yükünü gönder](media/custom-commands/send-activity-to-client-json-nuget.png)

' MainPage. xaml. cs ' içinde, Ekle
- `using Newtonsoft.Json;` 
- `using Windows.ApplicationModel.Core;`

### <a name="handle-the-received-payload"></a>Alınan yükü işle

İçinde `InitializeDialogServiceConnector` , `ActivityReceived` olay işleyicisini aşağıdaki kodla değiştirin. Değiştirilen `ActivityReceived` olay işleyicisi yükü etkinlikten çıkaracaktır ve TV veya fanı 'ın görsel durumunu sırasıyla değiştirir.

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
   > ![Etkinlik yükünü gönder](media/custom-commands/send-activity-to-client-turn-on-tv.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Nasıl yapılır: Web uç noktalarını ayarlama (Önizleme)](./how-to-custom-commands-setup-web-endpoints.md)