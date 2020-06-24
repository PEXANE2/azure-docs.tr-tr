---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 72e4318bea7245e440db4c7d95bf7f1f38bbe268
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85081534"
---
### <a name="send-a-test-notification"></a>Test bildirimi gönderme

1. [Postman](https://www.postman.com/downloads/)'da yeni bir sekme açın.

1. **Gönderi**isteği olarak ayarlayın ve şu adresi girin:

    ```xml
    https://<app_name>.azurewebsites.net/api/notifications/requests
    ```

1. [API anahtarı kullanarak Istemci kimlik doğrulaması](#authenticate-clients-using-an-api-key-optional) ' nı tamamlamayı seçtiyseniz, istek üst bilgilerini **apikey** değerini içerecek şekilde yapılandırmayı unutmayın.

   | Anahtar                            | Değer                          |
   | ------------------------------ | ------------------------------ |
   | apikey                         | <your_api_key>                 |

   > [!NOTE]
   > Bu, **Pushdemo** projesi içinden **config. local_secrets. cs** dosyasında bulunabilir.

1. **Gövde**için **Ham** seçeneğini belirleyin, sonra Biçim seçenekleri listesinden **JSON** ' ı seçin ve sonra bazı yer tutucu **JSON** içeriğini ekleyin:

    ```json
    {
        "text": "Message from Postman!",
        "action": "action_a"
    }
    ```

1. Pencerenin sağ üst köşesindeki **Kaydet** düğmesinin altında bulunan **kod** düğmesini seçin. İstek, **HTML** için görüntülendiğinde aşağıdaki örneğe benzer olmalıdır ( **apikey** üst bilgisini dahil ettiğinize bağlı olarak):

    ```html
    POST /api/notifications/requests HTTP/1.1
    Host: https://<app_name>.azurewebsites.net
    apikey: <your_api_key>
    Content-Type: application/json

    {
        "text": "Message from backend service",
        "action": "action_a"
    }
    ```

1. **Pushdemo** uygulamasını hedef platformların bir veya her ikisinde de çalıştırın (**Android** ve **iOS**).

    > [!NOTE]
    > **Android** üzerinde test ediyorsanız, **hata ayıklama**aşamasında çalıştırtığınızdan veya uygulamanın uygulama çalıştırılarak dağıtıldığından emin olun, uygulamayı kapatmaya ve sonra yeniden başlatıcısını başlatmaya zorlayın.

1. **Pushdemo** uygulamasında **Kaydet** düğmesine dokunun.

1. **[Postman](https://www.postman.com/downloads)**'a geri döndüğünüzde **kod parçacıkları oluştur** penceresini kapatın (henüz yapmadıysanız) **Gönder** düğmesine tıklayın.

1. **[Postman](https://www.postman.com/downloads)** 'da **200 Tamam** yanıtı aldığınızı ve uyarının, **eylem alındı eylemi**gösteren uygulamada göründüğünü doğrulayın.  

1. **Pushdemo** uygulamasını kapatın ve ardından **[Postman](https://www.postman.com/downloads)**'da **Gönder** düğmesine tekrar tıklayın.

1. **[Postman](https://www.postman.com/downloads)** 'da yeniden bir **200 Tamam** yanıtı edindiğini doğrulayın. Doğru ileti ile **Pushdemo** uygulamasının bildirim alanında bir bildirimin göründüğünü doğrulayın.

1. Uygulamayı açacağını doğrulamak için bildirime dokunun ve **eylem alındı** Uyarısı ' nı görüntülendiğini doğrulayın.

1. **[Postman](https://www.postman.com/downloads)**'da geri döndüğünüzde, önceki istek gövdesini, **eylem** değeri için *action_a* yerine *action_b* belirten sessiz bir bildirim gönderecek şekilde değiştirin.

    ```json
    {
        "action": "action_b",
        "silent": true
    }
    ```

1. Uygulama hala açıkken **[Postman](https://www.postman.com/downloads)**'daki **Gönder** düğmesine tıklayın.

1. **[Postman](https://www.postman.com/downloads)** 'da 200 bir **Tamam** yanıtı aldığınızı ve uyarının **eylem**alındı eylemi yerine **alınan actionb eylemini** gösteren uygulamada göründüğünü doğrulayın.

1. **Pushdemo** uygulamasını kapatın ve ardından **[Postman](https://www.postman.com/downloads)**'da **Gönder** düğmesine tekrar tıklayın.

1. **[Postman](https://www.postman.com/downloads)** 'da **200 ok** yanıtı edindiğini ve sessiz bildirimin bildirim alanında görünmediğini doğrulayın.
