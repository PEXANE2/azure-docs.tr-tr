---
author: sethmanheim
ms.service: service-bus
ms.topic: include
ms.date: 11/09/2018
ms.author: sethm
ms.openlocfilehash: 7fd161c90234d45a6751f173ba3685ee8c392c87
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74260910"
---
## <a name="webapi-project"></a>WebAPI Projesi

1. Visual Studio'da, **Kullanıcıları Bildir** öğreticisinde oluşturduğunuz **AppBackend** projesini açın.
2. Notifications.cs, **bildirimler** sınıfının tamamını aşağıdaki kodla değiştirin. Yer tutucuları, bildirim hub'ınız ve hub adınız için bağlantı dizenizi (tam erişimli) değiştirdiğinizden emin olun. Bu değerleri [Azure portalından](https://portal.azure.com)edinebilirsiniz. Bu modül artık gönderilecek farklı güvenli bildirimleri temsil eder. Tam bir uygulamada, bildirimler bir veritabanında depolanır; basitlik için, bu durumda biz bellekte saklayın.
   
   ```csharp
    public class Notification
    {
        public int Id { get; set; }
        public string Payload { get; set; }
        public bool Read { get; set; }
    }

    public class Notifications
    {
        public static Notifications Instance = new Notifications();

        private List<Notification> notifications = new List<Notification>();

        public NotificationHubClient Hub { get; set; }

        private Notifications() {
            Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",     "{hub name}");
        }

        public Notification CreateNotification(string payload)
        {
            var notification = new Notification() {
            Id = notifications.Count,
            Payload = payload,
            Read = false
            };

            notifications.Add(notification);

            return notification;
        }

        public Notification ReadNotification(int id)
        {
            return notifications.ElementAt(id);
        }
    }
    ```

1. NotificationsController.cs, **NotificationsController** sınıf tanımının içindeki kodu aşağıdaki kodla değiştirin. Bu bileşen, aygıtın bildirimi güvenli bir şekilde alması için bir yol uygular ve ayrıca aygıtlarınıza güvenli bir itme tetiklemenin bir yolunu (bu öğreticinin amaçları için) sağlar. Bildirim hub'ına bildirim gönderirken, yalnızca bildirimin kimliği (ve gerçek bir ileti olmadan) ham bir bildirim gönderdiğimizi unutmayın:
   
   ```csharp
    public NotificationsController()
    {
        Notifications.Instance.CreateNotification("This is a secure notification!");
    }

    // GET api/notifications/id
    public Notification Get(int id)
    {
        return Notifications.Instance.ReadNotification(id);
    }

    public async Task<HttpResponseMessage> Post()
    {
        var secureNotificationInTheBackend = Notifications.Instance.CreateNotification("Secure confirmation.");
        var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;

        // windows
        var rawNotificationToBeSent = new Microsoft.Azure.NotificationHubs.WindowsNotification(secureNotificationInTheBackend.Id.ToString(),
                        new Dictionary<string, string> {
                            {"X-WNS-Type", "wns/raw"}
                        });
        await Notifications.Instance.Hub.SendNotificationAsync(rawNotificationToBeSent, usernameTag);

        // apns
        await Notifications.Instance.Hub.SendAppleNativeNotificationAsync("{\"aps\": {\"content-available\": 1}, \"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}", usernameTag);

        // gcm
        await Notifications.Instance.Hub.SendGcmNativeNotificationAsync("{\"data\": {\"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}}", usernameTag);

        return Request.CreateResponse(HttpStatusCode.OK);
    }
    ```

Yöntemin `Post` artık tost bildirimi göndermediğini unutmayın. Yalnızca bildirim kimliği içeren ham bir bildirim gönderir ve herhangi bir hassas içerik gönderir. Ayrıca, bildirim merkezinizde kimlik bilgileri yapılandırılan olmayan platformlar için gönderme işlemini yorumladığınızdan emin olun, çünkü bunlar hatalara neden olacaktır.

1. Şimdi bu uygulamayı tüm cihazlardan erişilebilir hale getirmek için bir Azure Web Sitesine yeniden dağıtacağız. **AppBackend** projesine sağ tıklayıp **Yayımla**’yı seçin.
2. Yayımlama hedefiniz olarak Azure Web Sitesini seçin. Azure hesabınızla oturum açın ve varolan veya yeni bir Web Sitesi seçin ve **Bağlantı** sekmesinde **hedef URL** özelliğine dikkat edin. Bu URL'yi daha sonra bu öğreticide *arka uç bitiş noktanız* olarak adlandıracağız. **Yayımla**’ta tıklayın.
