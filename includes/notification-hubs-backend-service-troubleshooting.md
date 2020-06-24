---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 343fef941870b6deaad777fac9b9d258d454aa2b
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85081539"
---
#### <a name="no-response-from-the-backend-service"></a>Arka uç hizmetinden yanıt yok

Yerel olarak test edilirken, arka uç hizmetinin çalıştığından ve doğru bağlantı noktasını kullandığından emin olun.

**Azure API uygulamasında**test yapıyorsanız hizmetin çalıştığını ve dağıtılıp dağıtılmadığını ve hatasız başlatıldığını denetleyin.

İstemci aracılığıyla test ederken, **[Postman](https://www.postman.com/downloads)** 'da veya mobil uygulama yapılandırmasında temel adresi doğru belirttiğinizden emin olun. Temel adres, `https://<api_name>.azurewebsites.net/` `https://localhost:5001/` yerel olarak veya test edilirken olmalıdır.

#### <a name="not-receiving-notifications-on-android-after-starting-or-stopping-a-debug-session"></a>Hata ayıklama oturumunu başlattıktan veya durdurduktan sonra Android 'de bildirim almıyor

Bir hata ayıklama oturumunu başlattıktan veya durdurduktan sonra tekrar kaydolduğundan emin olun. Hata ayıklayıcı yeni bir **Firebase** belirtecinin oluşturulmasına neden olur. Bildirim Hub 'ı yüklemesinin de güncelleştirilmeleri gerekir.

#### <a name="receiving-a-401-status-code-from-the-backend-service"></a>Arka uç hizmetinden 401 durum kodu alma

**Apikey** istek üst bilgisini ayarladığınız ve bu değer arka uç hizmeti için yapılandırdığınız ile eşleşen bir doğrulama olduğunu doğrulayın.

Yerel olarak test ederken bu hatayı alırsanız, istemci yapılandırmasında tanımladığınız anahtar değerinin, [API](#create-the-api-app)tarafından kullanılan **kimlik doğrulaması: apikey** Kullanıcı ayarı değeri ile eşleştiğinden emin olun.

Bir **API**uygulamasıyla test ediyorsanız, istemci yapılandırma dosyasındaki anahtar değerin [API](#create-the-api-app)uygulamasında kullandığınız **kimlik doğrulaması: apikey** uygulama ayarı ile eşleştiğinden emin olun.

> [!NOTE]
> Arka uç hizmetini dağıttıktan sonra bu ayarı oluşturduysanız veya değiştirdiyseniz, etkin olması için hizmeti yeniden başlatmanız gerekir.

[API anahtarı kullanarak Istemci kimlik doğrulaması](#authenticate-clients-using-an-api-key-optional) ' nı tamamlamamayı seçtiyseniz, **Yetkilendirme** özniteliğini **notificationscontroller** sınıfına uygulamatığınızdan emin olun.

#### <a name="receiving-a-404-status-code-from-the-backend-service"></a>Arka uç hizmetinden 404 durum kodu alma

Endpoint ve HTTP istek yönteminin doğru olduğunu doğrulayın. Örneğin, uç noktalar şu şekilde olmalıdır:

- **[Put]**`https://<api_name>.azurewebsites.net/api/notifications/installations`
- **[Sil]**`https://<api_name>.azurewebsites.net/api/notifications/installations/<installation_id>`
- **[Gönderi]**`https://<api_name>.azurewebsites.net/api/notifications/requests`

Ya da yerel olarak test edilirken:

- **[Put]**`https://localhost:5001/api/notifications/installations`
- **[Sil]**`https://localhost:5001/api/notifications/installations/<installation_id>`
- **[Gönderi]**`https://localhost:5001/api/notifications/requests`

İstemci uygulamasında temel adresi belirtirken, bir ile bitdiğinden emin olun `/` . Temel adres, `https://<api_name>.azurewebsites.net/` `https://localhost:5001/` yerel olarak veya test edilirken olmalıdır.

#### <a name="unable-to-register-and-a-notification-hub-error-message-is-displayed"></a>Kayıt yapılamıyor ve bir Bildirim Hub 'ı hata iletisi görüntüleniyor

Sınama cihazının ağ bağlantısı olduğunu doğrulayın. Ardından, **HttpResponse**içindeki **StatusCode** özelliğinin değerini incelemek için bir kesme noktası ayarlayarak http yanıt durum kodunu saptayın.

Durum koduna göre geçerli olan yerlerde, önceki sorun giderme önerilerini gözden geçirin.

İlgili API için bu özel durum kodlarını döndüren satırlarda bir kesme noktası ayarlayın. Ardından, yerel olarak hata ayıklarken arka uç hizmetini çağırmayı deneyin.

Arka uç hizmetinin, uygun yükü kullanarak **[Postman](https://www.postman.com/downloads)** aracılığıyla beklendiği gibi çalıştığını doğrulayın. Söz konusu platform için istemci kodu tarafından oluşturulan gerçek yükü kullanın.

Hiçbir adım kaçırılkullanılmadığından emin olmak için platforma özgü yapılandırma bölümlerini gözden geçirin. `installation id`Uygun değerlerin ve ilgili platform için değişkenlerin çözümlenmekte olduğunu denetleyin `token` .

#### <a name="unable-to-resolve-an-id-for-the-device-error-message-is-displayed"></a>Cihazın KIMLIĞI çözümlenemedi hata iletisi görüntüleniyor

Hiçbir adım kaçırılkullanılmadığından emin olmak için platforma özgü yapılandırma bölümlerini gözden geçirin.
