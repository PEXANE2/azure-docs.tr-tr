---
title: Web uç noktalarını ayarlama (Önizleme)
titleSuffix: Azure Cognitive Services
description: özel komutlar için Web uç noktalarını ayarlama
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
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86027690"
---
# <a name="set-up-web-endpoints"></a>Web uç noktalarını ayarlama

Bu makalede, bir istemci uygulamasından HTTP istekleri yapmanıza imkan tanıyan özel bir komutlar uygulamasındaki Web uç noktalarını ayarlamayı öğreneceksiniz. Aşağıdaki görevleri tamamlayacaksınız:

- Özel komutlar uygulamasındaki Web uç noktalarını ayarlama
- Özel komutlar uygulamasındaki Web uç noktalarını çağır
- Web uç noktaları yanıtını alma 
- Web uç noktaları yanıtını özel bir JSON yüküne tümleştirme, bir C# UWP konuşma SDK 'Sı istemci uygulamasından gönderme ve görselleştirme

## <a name="prerequisites"></a>Ön koşullar
> [!div class = "checklist"]
> * [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)
> * Konuşma hizmeti için bir Azure abonelik anahtarı: [ücretsiz olarak alın](get-started.md) veya [Azure Portal](https://portal.azure.com) oluşturun
> * Önceden [oluşturulmuş özel komutlar uygulaması](quickstart-custom-commands-application.md)
> * Bir konuşma SDK 'Sı etkin istemci uygulaması: [nasıl yapılır: etkinliği istemci uygulamasına sonlandırma](./how-to-custom-commands-setup-speech-sdk.md)

## <a name="setup-web-endpoints"></a>Web uç noktalarını ayarlama

1. Daha önce oluşturduğunuz özel komutlar uygulamasını açın. 
1. "Web uç noktaları" na gidin, "yeni Web uç noktası" seçeneğine tıklayın.

   > [!div class="mx-imgBorder"]
   > ![Yeni Web uç noktası](media/custom-commands/setup-web-endpoint-new-endpoint.png)

   | Ayar | Önerilen değer | Açıklama |
   | ------- | --------------- | ----------- |
   | Name | Güncelleştirme Devemlak | Web uç noktasının adı. |
   | URL | https://webendpointexample.azurewebsites.net/api/DeviceState | Özel komut uygulamanızın konuştuğunu istediğiniz uç noktanın URL 'SI. |
   | Yöntem | POST | Uç noktanızla izin verilen etkileşimler (GET, POST gibi).|
   | Üst bilgiler | Anahtar: uygulama, değer: ApplicationId 'nizin ilk 8 basamağını alın | İstek üst bilgisine eklenecek üst bilgi parametreleri.|

    > [!NOTE]
    > - TV ve fanı 'nin cihaz durumunu kaydeden veritabanı ile birlikte gelen [Azure işlevi](https://docs.microsoft.com/azure/azure-functions/)kullanılarak oluşturulan örnek Web uç noktası
    > - Önerilen üst bilgi yalnızca örnek uç nokta için gereklidir
    > - Üstbilgi değerinin örnek uç noktanızda benzersiz olduğundan emin olmak için, ApplicationId 'nizin ilk 8 basamağını alın
    > - Gerçek dünyada Web uç noktası, cihazlarınızı yöneten [IoT Hub 'ına](https://docs.microsoft.com/azure/iot-hub/about-iot-hub) ait uç nokta olabilir

1. **Kaydet**’e tıklayın.

## <a name="call-web-endpoints"></a>Web uç noktalarını çağır

1. **Turnonoff** komutuna gidin, tamamlanma kuralı altında **ConfirmationResponse** öğesini seçin, sonra **Eylem Ekle**' yi seçin.
1. **Yeni eylem türü**altında, **Web uç noktası çağır** ' ı seçin.
1. **Düzenleme eylemi-uç noktalarında**, oluşturduğumuz web uç noktası olan **updatedevicemlak**' yi seçin.  
1. **Yapılandırma**bölümünde aşağıdaki değerleri yerleştirin: 
   > [!div class="mx-imgBorder"]
   > ![Web uç noktaları eylem parametrelerini çağır](media/custom-commands/setup-web-endpoint-edit-action-parameters.png)

   | Ayar | Önerilen değer | Açıklama |
   | ------- | --------------- | ----------- |
   | Uç Noktalar | Güncelleştirme Devemlak | Bu eylemde çağırmak istediğiniz Web uç noktası. |
   | Sorgu parametreleri | Item = {SubjectDevice} &&değer = {OnOff} | Web uç noktası URL 'sine eklenecek sorgu parametreleri.  |
   | Gövde içeriği | YOK | İsteğin gövde içeriği. |

    > [!NOTE]
    > - Önerilen sorgu parametreleri yalnızca örnek uç nokta için gereklidir

1. **Yürütmeye yönelik Işlemin başarılı**olması durumunda **konuşma yanıtı gönder**' i seçin.
    
    **Basit düzenleyicide**, girin `{SubjectDevice} is {OnOff}` .
   
   > [!div class="mx-imgBorder"]
   > ![Başarı durumunda Web uç noktaları çağrısı eylemi](media/custom-commands/setup-web-endpoint-edit-action-on-success-send-response.png)

   | Ayar | Önerilen değer | Açıklama |
   | ------- | --------------- | ----------- |
   | Yürütülecek eylem | Konuşma yanıtı gönder | Web uç noktasına istek başarılı olursa yürütülecek eylem |
   
   > [!NOTE]
   > - Ayrıca, kullanarak HTTP yanıtındaki alanlara doğrudan erişebilirsiniz `{YourWebEndpointName.FieldName}` . Örneğin, `{UpdateDeviceState.TV}`

1. **Yürütülmeye yönelik eylem sayfasında**, **konuşma yanıtı gönder** ' i seçin.

    **Basit düzenleyicide**, girin `Sorry, {WebEndpointErrorMessage}` .

   > [!div class="mx-imgBorder"]
   > ![Hata durumunda Web uç noktaları çağrısı eylemi](media/custom-commands/setup-web-endpoint-edit-action-on-fail.png)

   | Ayar | Önerilen değer | Açıklama |
   | ------- | --------------- | ----------- |
   | Yürütülecek eylem | Konuşma yanıtı gönder | Web uç noktasına istek başarısız olursa yürütülecek eylem |

   > [!NOTE]
   > - `{WebEndpointErrorMessage}` isteğe bağlıdır. Herhangi bir hata iletisini açığa çıkarmak istemiyorsanız, kaldırma ücretsizdir.
   > - Örnek uç noktanızda, eksik başlık parametreleri gibi yaygın hatalar için ayrıntılı hata iletileriyle http yanıtı geri gönderiyoruz. 

### <a name="try-it-out-in-test-portal"></a>Test portalında deneyin
- Başarılı yanıt \
Kaydet, eğitme ve test
   > [!div class="mx-imgBorder"]
   > ![Başarı durumunda Web uç noktaları çağrısı eylemi](media/custom-commands/setup-web-endpoint-on-success-response.png)
- Başarısız yanıt durumunda \
Sorgu parametrelerinden birini kaldırma, kaydetme, yeniden eğitme ve test etme
   > [!div class="mx-imgBorder"]
   > ![Başarı durumunda Web uç noktaları çağrısı eylemi](media/custom-commands/setup-web-endpoint-on-fail-response.png)

## <a name="integrate-with-client-application"></a>İstemci uygulamasıyla tümleştirin

[Nasıl yapılır: etkinliği istemci uygulamasına gönderme (Önizleme)](./how-to-custom-commands-send-activity-to-client.md)bölümünde, **Istemciye bir etkinlik Gönder** eylemi eklediniz. Etkinlik, **Web bitiş noktası** eyleminin başarılı olup olmadığına bakılmaksızın istemci uygulamasına gönderilir.
Ancak, çoğu durumda yalnızca Web uç noktasına yapılan çağrı başarılı olduğunda etkinliği istemci uygulamasına göndermek istersiniz. Bu örnekte cihaz durumunun başarıyla güncelleştirildiği durumdur.

1. Daha önce eklediğiniz **Istemciye gönderilen etkinliği** silme eylemini silin.
1. Çağrı Web uç noktasını Düzenle: 
    1. **Yapılandırma**bölümünde **sorgu parametrelerinin** olduğundan emin olun`item={SubjectDevice}&&value={OnOff}`
    1. **Başarı**durumunda, **etkinliği Istemciye göndermek** için **işlemi yürütülecek şekilde** değiştirin
    1. Aşağıdaki JSON 'ı **etkinlik içeriğine** kopyalayın
   ```json
   {
     "type": "event",
     "name": "UpdateDeviceState",
     "state": "{OnOff}",
     "device": "{SubjectDevice}"
   }
   ```
    > [!div class="mx-imgBorder"]
    > ![Başarı durumunda etkinlik Gönder](media/custom-commands/setup-web-endpoint-edit-action-on-success-send-activity.png)
   
Artık Web uç noktasına istek başarılı olduğunda yalnızca istemciye etkinlik gönderirsiniz.

### <a name="create-visuals-for-syncing-device-state"></a>Cihaz durumunu eşitlemek için görseller oluşturma
Aşağıdaki XML 'i `MainPage.xaml` bloğun üzerine ekleyin `"EnableMicrophoneButton"` .

```xml
<Button x:Name="SyncDeviceStateButton" Content="Sync Device State"
        Margin="0,10,10,0" Click="SyncDeviceState_ButtonClicked"
        Height="35"/>
<Button x:Name="EnableMicrophoneButton" ......
        .........../>
```

### <a name="sync-device-state"></a>Cihaz durumunu Eşitle 

İçinde `MainPage.xaml.cs` , başvurusunu ekleyin `using Windows.Web.Http;` . Sınıfına aşağıdaki kodu ekleyin `MainPage` . Bu yöntem, örnek uç noktaya bir GET isteği gönderir ve uygulamanızın geçerli cihaz durumunu ayıklar. `<your_app_name>`Özel komut uç noktasındaki **üst bilgide** kullandığınız şekilde değişdiğinizden emin olun

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
1. Cihaz durumunu Eşitle ' ye tıklayın. \
Uygulamayı önceki bölümde test ederseniz TV programlarını `turn on tv` "açık" olarak görürsünüz.
    > [!div class="mx-imgBorder"]
    > ![Cihaz durumunu Eşitle](media/custom-commands/setup-web-endpoint-sync-device-state.png)
1. Mikrofonu etkinleştir ' i seçin
1. Konuşma düğmesini seçin
1. Komutunu`turn on the fan`
1. Fanı 'ın görsel durumu "açık" olarak değiştirilmelidir
    > [!div class="mx-imgBorder"]
    > ![Fanı aç](media/custom-commands/setup-web-endpoint-turn-on-fan.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel komutlar uygulamanız için bir CI/CD işlemini etkinleştirme](./how-to-custom-commands-deploy-cicd.md)