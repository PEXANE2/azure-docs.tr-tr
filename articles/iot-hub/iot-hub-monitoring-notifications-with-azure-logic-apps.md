---
title: Azure Logic Apps ile IoT uzaktan izleme ve bildirimler | Microsoft Dokümanlar
description: IoT hub'ınızda IoT sıcaklık izleme için Azure Logic Apps'ı kullanın ve algılanan anormallikler için posta kutunuza otomatik olarak e-posta bildirimleri gönderin.
author: robinsh
keywords: iot izleme, iot bildirimleri, iot sıcaklık izleme
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 07/18/2019
ms.author: robinsh
ms.openlocfilehash: 2720f9acfa308294b30f9203ba80e3f9b426e1e9
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680714"
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a>IoT hub'ınızı ve posta kutunuzu bağlayan Azure Logic Apps ile IoT uzaktan izleme ve bildirimler

![Uçuça diyagram](media/iot-hub-monitoring-notifications-with-azure-logic-apps/iot-hub-e2e-logic-apps.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[Azure Logic Apps,](https://docs.microsoft.com/azure/logic-apps/) şirket içi ve bulut hizmetleri, bir veya daha fazla kuruluş ve çeşitli protokoller arasında iş akışları düzenlemenize yardımcı olabilir. Bir mantık uygulaması bir tetikleyici yle başlar ve ardından koşullar ve yineleyiciler gibi yerleşik denetimler kullanılarak sıralanabilecek bir veya daha fazla eylem izlenir. Bu esneklik, Logic Apps'ı IoT izleme senaryoları için ideal bir IoT çözümü haline getirir. Örneğin, bir aygıttan bir Aygıttan gelen telemetri verilerinin Bir IoT Hub bitiş noktasına gelmesi, verileri Azure Depolama blob'unda depolamak için mantık uygulaması iş akışları başlatabilir, veri anormallikleri konusunda uyarmak için e-posta uyarıları gönderebilir, bir aygıt arıza bildiriyorsa teknisyen ziyareti zamanlayabilir ve benzeri.

## <a name="what-you-learn"></a>Öğrenecekleriniz

IoT hub'ınızı ve posta kutunuzu sıcaklık izleme ve bildirimler için birbirine bağlayan bir mantık uygulaması oluşturmayı öğrenirsiniz.

Aygıtınızda çalışan istemci kodu, `temperatureAlert`IoT hub'ınıza gönderdiği her telemetri iletisine bir uygulama özelliği ayarlar. İstemci kodu 30 C'nin üzerindeki bir sıcaklığı `true`algıladığında, bu özelliği ; aksi takdirde, özelliği `false`.

IoT hub'ınıza gelen iletiler, gövdede bulunan telemetri verileri ve `temperatureAlert` uygulama özelliklerinde bulunan özellik (sistem özellikleri gösterilmez):

```json
{
  "body": {
    "messageId": 18,
    "deviceId": "Raspberry Pi Web Client",
    "temperature": 27.796111770668457,
    "humidity": 66.77637926438427
  },
  "applicationProperties": {
    "temperatureAlert": "false"
  }
}
```

IoT Hub ileti biçimi hakkında daha fazla bilgi edinmek için [IoT Hub iletileri oluştur ve okuyun.](iot-hub-devguide-messages-construct.md)

Bu konuda, `temperatureAlert` özelliğin bir Hizmet Veri Merkezi bitiş noktasına olduğu iletileri `true` göndermek için IoT hub'ınızda yönlendirme yi ayarlarsınız. Daha sonra, Servis Veri Servisi bitiş noktasına gelen iletileri tetikleyen ve size bir e-posta bildirimi gönderen bir mantık uygulaması ayarlarsınız.

## <a name="what-you-do"></a>Ne yaparsınız

* Bir Hizmet Veri Servisi ad alanı oluşturun ve buna bir Hizmet Veri Servisi kuyruğu ekleyin.
* Servis Veri Yolu kuyruğuna sıcaklık uyarısı içeren iletileri yönlendirmek için IoT hub'ınıza özel bir bitiş noktası ve yönlendirme kuralı ekleyin.
* Hizmet Veri Servisi kuyruğunuzdaki iletileri tüketmek ve istenen alıcıya bildirim e-postaları göndermek için bir mantık uygulaması oluşturun, yapılandırın ve test edin.

## <a name="what-you-need"></a>Ne gerekiyor

* [Raspberry Pi online simülatör](iot-hub-raspberry-pi-web-simulator-get-started.md) öğretici veya cihaz öğreticiler birini tamamlayın; örneğin, [ahududu Pi düğüm ile.js](iot-hub-raspberry-pi-kit-node-get-started.md). Bunlar aşağıdaki gereksinimleri kapsar:

  * Etkin bir Azure aboneliği.
  * Aboneliğiniz altında bir Azure IoT hub'ı.
  * Cihazınızda çalışan ve Azure IoT hub'ınıza telemetri iletileri gönderen bir istemci uygulaması.

## <a name="create-service-bus-namespace-and-queue"></a>Hizmet Veri Günü ad alanı ve sıra oluşturma

Service Bus ad alanı ve kuyruğu oluşturun. Bu konunun ilerleyen saatlerinde, IoT hub'ınızda, Hizmet Veri Servisi sırasına sıcaklık uyarısı içeren iletileri yönlendirmek için bir yönlendirme kuralı oluşturursunuz ve burada bir mantık uygulaması tarafından alınır ve bir bildirim e-postası göndermek için tetiklersiniz.

### <a name="create-a-service-bus-namespace"></a>Service Bus ad alanı oluşturma

1. Azure [portalında](https://portal.azure.com/), + Kaynak > **Tümleştirme** > Hizmeti Veri **Yolunda'nı seçin.****Service Bus**

1. Ad **alanı oluştur** bölmesine aşağıdaki bilgileri sağlayın:

   **Adı**: Servis veri günü ad alanının adı. Ad alanı Azure'da benzersiz olmalıdır.

   **Fiyatlandırma katmanı**: Açılan listeden **Temel'i** seçin. Temel katman bu öğretici için yeterlidir.

   **Kaynak grubu**: IoT hub'ınızın kullandığı kaynak grubunu kullanın.

   **Konum**: IoT hub'ınızın kullandığı konumu kullanın.

   ![Azure portalında servis veri günü ad alanı oluşturma](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1-create-service-bus-namespace-azure-portal.png)

1. **Oluştur**’u seçin. Bir sonraki adıma geçmeden önce dağıtımın tamamlanmasını bekleyin.

### <a name="add-a-service-bus-queue-to-the-namespace"></a>Ad alanına Hizmet Veri Servisi kuyruğu ekleme

1. Servis Veri Günü ad alanını açın. Hizmet Veri Yolu ad alanına gitmenin en kolay yolu kaynak bölmesinden **Kaynak gruplarını** seçmek, kaynak grubunuzu seçmek ve ardından kaynak listesinden Hizmet Veri Yolu ad alanını seçmektir.

1. Service **Bus Namespace** bölmesine **+ Sıra'yı**seçin.

1. Sıra için bir ad girin ve sonra **Oluştur'u**seçin. Sıra başarıyla oluşturulduğunda, **sıra oluştur** bölmesi kapanır.

   ![Azure portalında servis veri hizmeti sırası ekleme](media/iot-hub-monitoring-notifications-with-azure-logic-apps/create-service-bus-queue.png)

1. **Hizmet Veri Servisi Ad Alanı** bölmesine geri dön, **Varlıklar**altında, **Kuyruklar'ı**seçin. Hizmet Veri Servisi sırasını listeden açın ve ardından **Paylaşılan erişim ilkeleri** > **+ Ekle'yi**seçin.

1. İlke için bir ad girin, **Yönet'i**denetleyin ve sonra **Oluştur'u**seçin.

   ![Azure portalında servis veri günü sırası ilkesi ekleme](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2-add-service-bus-queue-azure-portal.png)

## <a name="add-a-custom-endpoint-and-routing-rule-to-your-iot-hub"></a>IoT hub'ınıza özel bir uç nokta ve yönlendirme kuralı ekleme

IoT hub'ınıza Servis Veri Merkezi kuyruğu için özel bir bitiş noktası ekleyin ve bu uç noktaya sıcaklık uyarısı içeren iletileri yönlendirmek için bir ileti yönlendirme kuralı oluşturun ve bunların mantık uygulamanız tarafından alınacağı. Yönlendirme kuralı, `temperatureAlert = "true"`aygıtta çalışan istemci kodu tarafından ayarlanan `temperatureAlert` uygulama özelliğinin değerine göre iletileri iletmek için bir yönlendirme sorgusu kullanır. Daha fazla bilgi edinmek [için, ileti özelliklerine göre İleti yönlendirme sorgusuna](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax#message-routing-query-based-on-message-properties)bakın.

### <a name="add-a-custom-endpoint"></a>Özel bir bitiş noktası ekleme

1. IoT hub'ınızı açın. IoT hub'ına gitmenin en kolay yolu kaynak bölmesinden **Kaynak gruplarını** seçmek, kaynak grubunuzu seçmek ve ardından kaynaklar listesinden IoT hub'ınızı seçmektir.

1. **İleti altında**İleti **yönlendirme'yi**seçin. İleti **yönlendirme** bölmesine Özel uç **noktaları** sekmesini seçin ve sonra **+ Ekle'yi**seçin. Açılan listeden **Servis veri günü kuyruğunu**seçin.

   ![Azure portalındaki IoT hub'ınıza bir bitiş noktası ekleme](media/iot-hub-monitoring-notifications-with-azure-logic-apps/select-iot-hub-custom-endpoint.png)

1. Servis **veri yolunda uç nokta** bölmesine aşağıdaki bilgileri girin:

   **Bitiş noktası adı**: Bitiş noktasının adı.

   **Servis veri günü ad alanı**: Oluşturduğunuz ad alanını seçin.

   **Servis veri servisi sırası**: Oluşturduğunuz sırayı seçin.

   ![Azure portalındaki IoT hub'ınıza bir bitiş noktası ekleme](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3-add-iot-hub-endpoint-azure-portal.png)

1. **Oluştur**’u seçin. Bitiş noktası başarıyla oluşturulduktan sonra bir sonraki adıma geçin.

### <a name="add-a-routing-rule"></a>Yönlendirme kuralı ekleme

1. **İleti yönlendirme** bölmesine geri dön, **Yollar** sekmesini seçin ve sonra **+ Ekle'yi**seçin.

1. **Rota** ekle bölmesine aşağıdaki bilgileri girin:

   **Adı**: Yönlendirme kuralının adı.

   **Bitiş Noktası**: Oluşturduğunuz bitiş noktasını seçin.

   **Veri kaynağı**: **Aygıt Telemetri İletilerini**seçin.

   **Yönlendirme sorgusu**: `temperatureAlert = "true"`Girin .

   ![Azure portalında yönlendirme kuralı ekleme](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4-add-routing-rule-azure-portal.png)

1. **Kaydet**’i seçin. **İleti yönlendirme** bölmesini kapatabilirsiniz.

## <a name="create-and-configure-a-logic-app"></a>Mantık Uygulaması oluşturma ve yapılandırma

Önceki bölümde, IoT hub'ınızı Servis Veri Yolu kuyruğa sıcaklık uyarısı içeren iletileri yönlendirmek için ayarlarsınız. Şimdi, Hizmet Veri Servisi sırasını izlemek ve sıraya bir ileti eklendiğinde bir e-posta bildirimi göndermek için bir mantık uygulaması ayarlayın.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma

1. Kaynak > **Tümleştirme** > **Mantığı Uygulaması** **Oluştur'u**seçin.

1. Aşağıdaki bilgileri girin:

   **Adı**: Mantık uygulamasının adı.

   **Kaynak grubu**: IoT hub'ınızın kullandığı kaynak grubunu kullanın.

   **Konum**: IoT hub'ınızın kullandığı konumu kullanın.

   ![Azure portalında bir mantık uygulaması oluşturma](media/iot-hub-monitoring-notifications-with-azure-logic-apps/create-a-logic-app.png)

1. **Oluştur**’u seçin.

### <a name="configure-the-logic-app-trigger"></a>Mantık uygulaması tetikleyicisini yapılandırma

1. Mantık uygulamasını açın. Mantık uygulamasına gitmenin en kolay yolu kaynak bölmesinden **Kaynak gruplarını** seçmek, kaynak grubunuzu seçmek ve ardından kaynak listesinden mantık uygulamanızı seçmektir. Mantık uygulamasını seçtiğinizde, Logic Apps Designer açılır.

1. Logic Apps Designer'da **Şablonlar'a** gidin ve **Boş Mantık Uygulaması'nı**seçin.

   ![Azure portalında boş bir mantık uygulamasıyla başlayın](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5-start-with-blank-logic-app-azure-portal.png)

1. **Tümler** sekmesini seçin ve ardından **Servis Veri Yolunda'nı**seçin.

   ![Azure portalında mantık uygulamanızı oluşturmaya başlamak için Servis Veri Servisi'ni seçin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6-select-service-bus-when-creating-blank-logic-app-azure-portal.png)

1. **Tetikleyiciler**altında, **bir veya daha fazla ileti sıraya geldiğinde (otomatik tamamlama) seçeneğini belirleyin.**

   ![Azure portalında mantık uygulamanızın tetikleyicisini seçin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/select-service-bus-trigger.png)

1. Servis veri günü bağlantısı oluşturun.
   1. Bir bağlantı adı girin ve listeden Servis Veri Aracı ad alanınızı seçin. Bir sonraki ekran açılır.

      ![Azure portalında mantık uygulamanız için servis veri servisi bağlantısı oluşturma](media/iot-hub-monitoring-notifications-with-azure-logic-apps/create-service-bus-connection-1.png)

   1. Servis veri servisi ilkesini (RootManageSharedAccessKey) seçin. Ardından **Oluştur'u**seçin.

      ![Azure portalında mantık uygulamanız için servis veri servisi bağlantısı oluşturma](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7-create-service-bus-connection-in-logic-app-azure-portal.png)

   1. Son ekranda, **Sıra adı**için açılan dan oluşturduğunuz sırayı seçin. Maksimum `175` **ileti sayısı**için girin.

      ![Mantık uygulamanızda servis veri servisi bağlantısı için maksimum ileti sayısını belirtin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8-specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)

   1. Değişikliklerinizi kaydetmek için Logic Apps Designer'ın üst kısmındaki menüden **Kaydet'i** seçin.

### <a name="configure-the-logic-app-action"></a>Mantık uygulaması eylemini yapılandırma

1. Bir SMTP servis bağlantısı oluşturun.

   1. **Yeni adım**'ı seçin. **Bir eylem seçin,** **Tümü** sekmesini seçin.

   1. Arama `smtp` kutusuna yazın, arama sonucuna **SMTP** hizmetini seçin ve ardından **E-posta Gönder'i**seçin.

      ![Azure portalındaki mantık uygulamanızda Bir SMTP bağlantısı oluşturma](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9-create-smtp-connection-logic-app-azure-portal.png)

   1. Posta kutunuz için SMTP bilgilerini girin ve ardından **Oluştur'u**seçin.

      ![Azure portalındaki mantık uygulamanıza SMTP bağlantı bilgilerini girin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10-enter-smtp-connection-info-logic-app-azure-portal.png)

      [Hotmail/Outlook.com,](https://support.office.com/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970) [Gmail](https://support.google.com/a/answer/176600?hl=en)ve [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html)için SMTP bilgilerini alın.

      > [!NOTE]
      > Bağlantıyı kurmak için TLS/SSL'yi devre dışı bmalısınız. Bu durumda ve bağlantı kurulduktan sonra TLS'yi yeniden etkinleştirmek istiyorsanız, bu bölümün sonundaki isteğe bağlı adıma bakın.

   1. **E-posta Gönder** adımında **yeni parametre** açılır ekle'den, **From**, **To**, **Subject** and **Body**'yi seçin. Seçim kutusunu kapatmak için ekranın herhangi bir yerine tıklayın veya dokunun.

      ![SMTP bağlantı e-posta alanlarını seçin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/smtp-connection-choose-fields.png)

   1. **From** ve **To**ve **Subject** `High temperature detected` and Body için e-posta **adresinizi**girin. Bu akış iletişim kutusunda **kullanılan uygulamalardan ve bağlayıcılardan dinamik içerik ekle açılırsa,** kapatmak için **Gizle'yi** seçin. Bu öğreticide dinamik içerik kullanmazsınız.

      ![Doldurma SMTP bağlantı e-posta alanları](media/iot-hub-monitoring-notifications-with-azure-logic-apps/fill-in-smtp-connection-fields.png)

   1. SMTP bağlantısını kaydetmek için **Kaydet'i** seçin.

1. (İsteğe bağlı) E-posta sağlayıcınızla bağlantı kurmak için TLS'yi devre dışı kaldırmanız gerekiyorsa ve yeniden etkinleştirmek istiyorsanız aşağıdaki adımları izleyin:

   1. Mantık **uygulama** bölmesinde, **Geliştirme Araçları**altında, **API bağlantılarını**seçin.

   1. API bağlantıları listesinden SMTP bağlantısını seçin.

   1. **SMTP API Bağlantı** bölmesinde, **Genel**altında, **API'yi Edit bağlantısını**seçin.

   1. **API Bağlantısını Edit** bölmesine, **SSL'yi etkinleştir'i**seçin, e-posta hesabınız için parolayı yeniden girin ve **Kaydet'i**seçin.

      ![Azure portalındaki mantık uygulamanızda SMTP API bağlantısını edin](media/iot-hub-monitoring-notifications-with-azure-logic-apps/re-enable-smtp-connection-ssl.png)

Mantık uygulamanız artık Servis Veri Servisi kuyruğundan sıcaklık uyarılarını işlemeye ve e-posta hesabınıza bildirim göndermeye hazırdır.

## <a name="test-the-logic-app"></a>Mantıksal uygulamayı test etme

1. Cihazınızda istemci uygulamasını başlatın.

1. Fiziksel bir cihaz kullanıyorsanız, sıcaklık 30 dereceyi aşana kadar ısı sensörüne dikkatlice bir ısı kaynağı getirin. Çevrimiçi simülatörü kullanıyorsanız, istemci kodu rasgele 30 C'yi aşan telemetri iletileri çıkaracaktır.

1. Mantık uygulaması tarafından gönderilen e-posta bildirimlerini almaya başlamalısınız.

   > [!NOTE]
   > E-posta hizmet sağlayıcınızın e-postayı gönderenin siz olduğundan emin olmak için gönderen inkimliğini doğrulaması gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

IoT hub'ınızı ve posta kutunuzu sıcaklık izleme ve bildirimler için bağlayan bir mantık uygulamasını başarıyla oluşturdunuz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
