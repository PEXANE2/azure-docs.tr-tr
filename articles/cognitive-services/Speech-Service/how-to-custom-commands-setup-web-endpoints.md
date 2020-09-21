---
title: Web uç noktası ayarlama (Önizleme)
titleSuffix: Azure Cognitive Services
description: özel komutlar için web uç noktası ayarlama
services: cognitive-services
author: xiaojul
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: xiaojul
ms.openlocfilehash: 0197bb81fdba8bab20742d95aebaa2028bb90c18
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86027690"
---
# <a name="set-up-web-endpoints"></a>Web uç noktalarını ayarlama

Bu makalede istemci uygulamasından HTTP isteği oluşturmak için Özel Komutlar uygulamasında web uç noktası ayarlamayı öğreneceksiniz. Şu görevleri tamamlayacaksınız:

- Özel Komutlar uygulamasında web uç noktalarını ayarlama
- Özel Komutlar uygulamasındaki web uç noktalarını çağırma
- Web uç noktalarından yanıt alma 
- Web uç noktalarından gelen yanıtı özel bir JSON yüküyle tümleştirme, gönderme ve C# UWP Konuşma SDK'sı istemci uygulamasından görselleştirme

## <a name="prerequisites"></a>Önkoşullar
> [!div class = "checklist"]
> * [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)
> * Konuşma hizmeti için Azure abonelik anahtarı: [Ücretsiz edinin](get-started.md) veya [Azure portalından](https://portal.azure.com) oluşturun
> * Önceden oluşturulmuş [Özel Komutlar uygulaması](quickstart-custom-commands-application.md)
> * Konuşma SDK'sı destekli istemci uygulaması: [Nasıl yapılır: İstemci uygulamasına etkinlik gönderme](./how-to-custom-commands-setup-speech-sdk.md)

## <a name="setup-web-endpoints"></a>Web uç noktalarını ayarlama

1. Önceden oluşturduğunuz Özel Komutlar uygulamasını açın. 
1. "Web uç noktaları"na gidip "Yeni web uç noktası"na tıklayın.

   > [!div class="mx-imgBorder"]
   > ![Yeni web uç noktası](media/custom-commands/setup-web-endpoint-new-endpoint.png)

   | Ayar | Önerilen değer | Açıklama |
   | ------- | --------------- | ----------- |
   | Ad | UpdateDeviceState | Web uç noktasının adı. |
   | URL | https://webendpointexample.azurewebsites.net/api/DeviceState | Özel komut uygulamanızın iletişim kurmasını istediğiniz uç noktanın URL'si. |
   | Yöntem | POST | Uç noktanız için izin verilen etkileşimler (GET, POST gibi).|
   | Üst Bilgiler | Anahtar: app, Değer: applicationId değerinin ilk 8 basamağını kullanın | İstek üst bilgisine eklenecek üst bilgi parametreleri.|

    > [!NOTE]
    > - Örnek web uç noktası [Azure İşlevi](https://docs.microsoft.com/azure/azure-functions/) kullanılarak oluşturulmuştur ve televizyon ile vantilatörün cihaz durumunu kaydeden bir veritabanına bağlıdır
    > - Önerilen üst bilgi yalnızca örnek uç nokta için gereklidir
    > - Üst bilgi değerinin örnek uç noktada benzersiz olmasını sağlamak için applicationId değerinizin ilk 8 basamağını kullanın
    > - Gerçek dünyada web uç noktası, cihazlarınızı yöneten [IOT hub'ının](https://docs.microsoft.com/azure/iot-hub/about-iot-hub) uç noktası olabilir

1. **Kaydet**’e tıklayın.

## <a name="call-web-endpoints"></a>Web uç noktalarını çağırma

1. **TurnOnOff** komutuna gidin, tamamlama kuralının altında **ConfirmationResponse** öğesini ve ardından **Eylem ekle**'yi seçin.
1. **Yeni eylem türü**'nün altında **Web uç noktasını çağır**'ı seçin
1. **Eylemi Düzenle - Uç Noktaları** bölümünde oluşturduğumuz web uç noktası olan **UpdateDeviceState** girişini seçin.  
1. **Yapılandırma** alanına aşağıdaki değerleri girin: 
   > [!div class="mx-imgBorder"]
   > ![Web uç noktası çağırma - eylem parametreleri](media/custom-commands/setup-web-endpoint-edit-action-parameters.png)

   | Ayar | Önerilen değer | Açıklama |
   | ------- | --------------- | ----------- |
   | Uç Noktalar | UpdateDeviceState | Bu eylemde çağırmak istediğiniz web uç noktası. |
   | Sorgu parametreleri | item={SubjectDevice}&&value={OnOff} | Web uç noktası URL'sine eklenecek sorgu parametreleri.  |
   | Gövde içeriği | Yok | İsteğin gövde içeriği. |

    > [!NOTE]
    > - Önerilen sorgu parametreleri yalnızca örnek uç nokta için gereklidir

1. **Başarılı Olduğunda - Yürütülecek eylem** bölümünde **Konuşma yanıtı gönder**'i seçin.
    
    **Basit düzenleyici**'ye `{SubjectDevice} is {OnOff}` yazın.
   
   > [!div class="mx-imgBorder"]
   > ![Web uç noktası çağırma - Başarılı Olduğunda](media/custom-commands/setup-web-endpoint-edit-action-on-success-send-response.png)

   | Ayar | Önerilen değer | Açıklama |
   | ------- | --------------- | ----------- |
   | Yürütülecek eylem | Konuşma yanıtı gönder | Web uç noktasına gönderilen isteğin başarılı olması durumunda yürütülecek eylem |
   
   > [!NOTE]
   > - Dilerseniz `{YourWebEndpointName.FieldName}` kullanarak HTTP yanıtındaki alanlara doğrudan erişim sağlayabilirsiniz. Örnek: `{UpdateDeviceState.TV}`

1. **Başarısız Olduğunda - Yürütülecek eylem** bölümünde **Konuşma yanıtı gönder**'i seçin

    **Basit düzenleyici**'ye `Sorry, {WebEndpointErrorMessage}` yazın.

   > [!div class="mx-imgBorder"]
   > ![Web uç noktası çağırma - Başarısız Olduğunda](media/custom-commands/setup-web-endpoint-edit-action-on-fail.png)

   | Ayar | Önerilen değer | Açıklama |
   | ------- | --------------- | ----------- |
   | Yürütülecek eylem | Konuşma yanıtı gönder | Web uç noktasına gönderilen isteğin başarısız olması durumunda yürütülecek eylem |

   > [!NOTE]
   > - `{WebEndpointErrorMessage}` isteğe bağlıdır. Hata iletisi görmek istemiyorsanız bunu kaldırabilirsiniz.
   > - Örnek uç noktamızda eksik üst bilgi parametreleri gibi yaygın hatalar için ayrıntılı hata iletisi içeren bir HTTP yanıtı gönderiyoruz. 

### <a name="try-it-out-in-test-portal"></a>Test portalında deneyin
- Başarılı Olduğunda yanıt\
Kaydet, eğit ve test et
   > [!div class="mx-imgBorder"]
   > ![Web uç noktası çağırma - Başarılı Olduğunda](media/custom-commands/setup-web-endpoint-on-success-response.png)
- Başarısız Olduğunda yanıt\
Sorgu parametrelerinden birini kaldır, kaydet, yeniden eğit ve test et
   > [!div class="mx-imgBorder"]
   > ![Web uç noktası çağırma - Başarılı Olduğunda](media/custom-commands/setup-web-endpoint-on-fail-response.png)

## <a name="integrate-with-client-application"></a>İstemci uygulamasıyla tümleştirme

[Nasıl yapılır: Etkinliği istemci uygulamasına gönderme (Önizleme)](./how-to-custom-commands-send-activity-to-client.md) bölümünde **Etkinliği istemciye gönder** eylemi eklediniz. Eylem, **Web uç noktasını çağırma** eyleminin başarılı olup olmadığına bakılmaksızın istemci uygulamasına gönderilir.
Ancak çoğu durumda etkinliği istemci uygulamasına yalnızca web uç noktasına yapılan çağrının başarılı olması durumunda göndermek istersiniz. Bu örnekte bu işlem, cihazın durumunun başarıyla güncelleştirilmesidir.

1. Önceden eklediğiniz **Etkinliği istemciye gönder** eylemini silin.
1. Çağrı web uç noktasını düzenleyin: 
    1. **Yapılandırma** bölümünde **Sorgu Parametreleri** değerinin `item={SubjectDevice}&&value={OnOff}` olduğundan emin olun
    1. **Başarılı Olduğunda** bölümünde **Yürütülecek eylem** değerini **Etkinliği istemciye gönder** olarak değiştirin
    1. Aşağıdaki JSON kodunu **Etkinlik İçeriği**'ne kopyalayın
   ```json
   {
     "type": "event",
     "name": "UpdateDeviceState",
     "state": "{OnOff}",
     "device": "{SubjectDevice}"
   }
   ```
    > [!div class="mx-imgBorder"]
    > ![Başarılı olduğunda etkinliği gönder](media/custom-commands/setup-web-endpoint-edit-action-on-success-send-activity.png)
   
Artık yalnızca web uç noktasına yapılan istek başarılı olduğunda istemciye etkinlik gönderiyorsunuz.

### <a name="create-visuals-for-syncing-device-state"></a>Cihaz durumunu eşitlemek için görsel oluşturma
Aşağıdaki XML kodunu `MainPage.xaml` dosyasına, `"EnableMicrophoneButton"` bloğunun üstüne ekleyin.

```xml
<Button x:Name="SyncDeviceStateButton" Content="Sync Device State"
        Margin="0,10,10,0" Click="SyncDeviceState_ButtonClicked"
        Height="35"/>
<Button x:Name="EnableMicrophoneButton" ......
        .........../>
```

### <a name="sync-device-state"></a>Cihaz durumunu eşitleme 

`MainPage.xaml.cs` dosyasına `using Windows.Web.Http;` başvurusunu ekleyin. Aşağıdaki kodu `MainPage` sınıfına ekleyin. Bu yöntem, örnek uç noktaya bir GET isteği gönderir ve uygulamanız için geçerli cihaz durumunu ayıklar. `<your_app_name>` yerine Özel Komut web uç noktasındaki **üst bilgide** kullandığınız değeri yazın

```C#
private async void SyncDeviceState_ButtonClicked(object sender, RoutedEventArgs e)
{
    //Create an HTTP client object
    var httpClient = new HttpClient();

    //Add a user-agent header to the GET request. 
    var your_app_name = "<your-app-name>";

    Uri endpoint = new Uri("https://webendpointexample.azurewebsites.net/api/DeviceState");
    var requestMessage = new HttpRequestMessage(HttpMethod.Get, endpoint);
    requestMessage.Headers.Add("app", $"{your_app_name}");

    try
    {
        //Send the GET request
        var httpResponse = await httpClient.SendRequestAsync(requestMessage);
        httpResponse.EnsureSuccessStatusCode();
        var httpResponseBody = await httpResponse.Content.ReadAsStringAsync();
        dynamic deviceState = JsonConvert.DeserializeObject(httpResponseBody);
        var TVState = deviceState.TV.ToString();
        var FanState = deviceState.Fan.ToString();
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
            CoreDispatcherPriority.Normal,
            () =>
            {
                State_TV.Text = TVState;
                State_Fan.Text = FanState;
            });
    }
    catch (Exception ex)
    {
        NotifyUser(
            $"Unable to sync device status: {ex.Message}");
    }
}
```

## <a name="try-it-out"></a>Deneyin

1. Uygulamayı başlatma
1. Cihaz Eşitleme Durumu'na tıklayın.\
Uygulamayı önceki bölümde `turn on tv` ile test ettiyseniz televizyonun "on" (açık) olduğunu görmüş olmanız gerekir.
    > [!div class="mx-imgBorder"]
    > ![Cihaz durumunu eşitleme](media/custom-commands/setup-web-endpoint-sync-device-state.png)
1. Mikrofonu etkinleştir'i seçin
1. Konuş düğmesini seçin
1. `turn on the fan` deyin
1. Vantilatörün görsel durumu "on" (açık) hale gelmelidir
    > [!div class="mx-imgBorder"]
    > ![Vantilatörü aç](media/custom-commands/setup-web-endpoint-turn-on-fan.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel Komutlar uygulamanız için CI/CD sürecini etkinleştirme](./how-to-custom-commands-deploy-cicd.md)