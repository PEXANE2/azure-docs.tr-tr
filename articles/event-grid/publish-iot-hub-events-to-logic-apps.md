---
title: Öğretici - Azure Mantık Uygulamalarını tetiklemek için IoT Hub olaylarını kullanma
description: Bu öğretici, Azure Olay Grid'inolay yönlendirme hizmetini nasıl kullanacağımı, IoT Hub etkinliklerini temel alan Azure Logic Apps eylemlerini gerçekleştirmek için otomatik işlemler oluşturmayı gösterir.
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: tutorial
ms.date: 11/21/2019
ms.author: robinsh
ms.openlocfilehash: 334b7b2c59b328e8eff3c7c2b9c3ed46bffc3442
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74706427"
---
# <a name="tutorial-send-email-notifications-about-azure-iot-hub-events-using-event-grid-and-logic-apps"></a>Öğretici: Olay Izgara ve Mantık Uygulamalarını kullanarak Azure IoT Hub olayları hakkında e-posta bildirimleri gönderme

Azure Event Grid, aşağı akış iş uygulamalarınızda eylemler tetikleyerek IoT Hub'daki olaylara karşılık vermenize olanak tanır.

Bu makale, IoT Hub ve Olay Grid kullanan bir örnek yapılandırma üzerinden yürür. Sonunda, IoT hub'ınıza her cihaz ekleninde bir bildirim e-postası göndermek üzere ayarlanmış bir Azure mantık uygulamanız vardır. 

## <a name="prerequisites"></a>Ön koşullar

* Azure Logic Apps tarafından desteklenen Office 365 Outlook, Outlook.com veya Gmail gibi bir e-posta sağlayıcısından alınmış e-posta hesabı. Bu e-posta hesabı olay bildirimlerini göndermek için kullanılır. Desteklenen Logic App bağlayıcılarının tam listesi için bkz. [Bağlayıcılara genel bakış](https://docs.microsoft.com/connectors/)
* Etkin bir Azure hesabı. Hesabınız yoksa, [ücretsiz bir hesap oluşturabilirsiniz.](https://azure.microsoft.com/pricing/free-trial/)
* Azure'da bir IoT Hub'ı. Henüz oluşturmadıysanız, yönergeler için bkz. [IoT Hub'ı kullanmaya başlama](../iot-hub/iot-hub-csharp-csharp-getstarted.md). 

## <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma

İlk olarak, bir mantık uygulaması oluşturun ve sanal makinenizin kaynak grubunu izleyen bir olay ızgara tetikleyicisi ekleyin. 

### <a name="create-a-logic-app-resource"></a>Mantıksal uygulama kaynağı oluşturma

1. Azure [portalında](https://portal.azure.com) **kaynak oluştur'u**seçin, ardından arama kutusuna "mantık uygulaması" yazın ve return'i seçin. Sonuçlardan **Mantık Uygulaması'nı** seçin.

   ![Mantıksal uygulama oluşturma](./media/publish-iot-hub-events-to-logic-apps/select-logic-app.png)

1. Bir sonraki ekranda **Oluştur'u**seçin. 

1. Mantıksal uygulamanıza aboneliğiniz içinde benzersiz olan bir ad verin, ardından IoT Hub'ınızla aynı aboneliği, kaynak grubunu ve konumu seçin. 

   ![Mantık uygulaması oluşturmak için alanlar](./media/publish-iot-hub-events-to-logic-apps/create-logic-app-fields.png)

1. **Oluştur'u**seçin.

1. Kaynak oluşturulduktan sonra mantık uygulamanıza gidin. Bunu yapmak için **Kaynak gruplarını**seçin ve ardından bu öğretici için oluşturduğunuz kaynak grubunu seçin. Ardından kaynak listesindeki mantık uygulamasını bulun ve seçin. 

1. Logic Apps Designer'da **Şablonları**görmek için sayfa aşağı. Mantık **uygulamanızı** sıfırdan oluşturabilmeniz için Boş Mantık Uygulaması'nı seçin.

### <a name="select-a-trigger"></a>Tetikleyici seçme

Tetikleyici, mantıksal uygulamanızı başlatan belirli bir olaydır. Bu öğreticide, iş akışını başlatan tetikleyici HTTP üzerinden bir istek alır.  

1. Bağlayıcılar ve tetikleyiciler arama çubuğunda **HTTP** yazın.

1. Tetikleyici olarak **İstek - Bir HTTP isteği alındığında**'yı seçin. 

   ![HTTP istek tetikleyicisini seçme](./media/publish-iot-hub-events-to-logic-apps/http-request-trigger.png)

1. **Şema oluşturmak için örnek yük kullanma** öğesini seçin. 

   ![HTTP istek tetikleyicisini seçme](./media/publish-iot-hub-events-to-logic-apps/sample-payload.png)

1. Aşağıdaki örnek JSON kodunu metin kutusuna yapıştırın ve **Bitti**'yi seçin:

   ```json
   [{
     "id": "56afc886-767b-d359-d59e-0da7877166b2",
     "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
     "subject": "devices/LogicAppTestDevice",
     "eventType": "Microsoft.Devices.DeviceCreated",
     "eventTime": "2018-01-02T19:17:44.4383997Z",
     "data": {
       "twin": {
         "deviceId": "LogicAppTestDevice",
         "etag": "AAAAAAAAAAE=",
         "deviceEtag": "null",
         "status": "enabled",
         "statusUpdateTime": "0001-01-01T00:00:00",
         "connectionState": "Disconnected",
         "lastActivityTime": "0001-01-01T00:00:00",
         "cloudToDeviceMessageCount": 0,
         "authenticationType": "sas",
         "x509Thumbprint": {
           "primaryThumbprint": null,
           "secondaryThumbprint": null
         },
         "version": 2,
         "properties": {
           "desired": {
             "$metadata": {
               "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
             },
             "$version": 1
           },
           "reported": {
             "$metadata": {
               "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
             },
             "$version": 1
           }
         }
       },
       "hubName": "egtesthub1",
       "deviceId": "LogicAppTestDevice"
     },
     "dataVersion": "1",
     "metadataVersion": "1"
   }]
   ```

1. **İsteğinize Uygulama/JSON olarak ayarlanmış bir Content-Type üst bilgisi eklemeyi unutmayın** önerisinin bulunduğu bir açılan bildirim alabilirsiniz. Bu öneriyi güvenle yoksayabilir ve sonraki bölüme geçebilirsiniz. 

### <a name="create-an-action"></a>Bir eylem oluşturun

Eylemler, tetikleyici mantıksal uygulama iş yükünü başlattıktan sonra gerçekleşen adımlardır. Bu öğreticide, e-posta sağlayıcınızdan bir e-posta bildirimi gönderme eylemi kullanılır. 

1. **Yeni adım**'ı seçin. Bu, bir **eylem seçmek**için bir pencere açar.

1. **E-posta**'yı arayın.

1. E-posta sağlayıcınıza uygun bağlayıcıyı bulun ve seçin. Bu öğreticide **Office 365 Outlook** kullanılır. Diğer e-posta sağlayıcılarının adımları da bunlara benzer. 

   ![E-posta sağlayıcısının bağlayıcısını seçme](./media/publish-iot-hub-events-to-logic-apps/o365-outlook.png)

1. **E-posta gönder** eylemini seçin. 

1. İstenirse, e-posta hesabınızda oturum açın. 

1. E-posta şablonunuzu oluşturun. 

   * **Kime**: Bildirim e-postalarını alacak olan e-posta adresini girin. Bu öğreticide, test etmek için erişebileceğiniz bir e-posta hesabı kullanın. 

   * **Konu**: Konu yla ilgili metni doldurun. Konu metin kutusunu tıklattığınızda, eklemek üzere dinamik içerik seçebilirsiniz. Örneğin, bu öğretici `IoT Hub alert: {event Type}`kullanır. Dinamik içeriği göremiyorsanız, **Dinamik İçerik Ekle** bağlantısını seçin - bu da içeriği niçin ve arayla salar.

   * **Gövde**: E-postanızın metnini yazın. Olay verileri temelinde dinamik içerik eklemek için seçici aracından JSON özelliklerini seçin. Dinamik içeriği göremiyorsanız, **Gövde** metin kutusunun altındaki **Dinamik İçerik Ekle** köprüünü seçin. Size istediğiniz alanları göstermiyorsa, önceki eylemdeki alanları eklemek için Dinamik içerik ekranında *daha fazlasını* tıklatın.

   E-posta şablonunuz, aşağıdaki örneğe benzer görünebilir:

   ![E-posta bilgilerini doldurma](./media/publish-iot-hub-events-to-logic-apps/email-content.png)

1. Mantıksal uygulamanızı kaydedin. 

### <a name="copy-the-http-url"></a>HTTP URL'sini kopyalama

Logic Apps Tasarımcısı'ndan çıkmadan önce, mantıksal uygulamalarınızın tetikleyici için dinlediği URL'yi kopyalayın. Bu URL'yi, Event Grid'i yapılandırmak için kullanırsınız. 

1. **Bir HTTP isteği alındığında** tetikleyici yapılandırma kutusunu tıklayarak genişletin. 

1. Yanındaki kopyala düğmesini seçerek **HTTP POST URL** değerini kopyalayın. 

   ![HTTP POST URL değerini kopyalama](./media/publish-iot-hub-events-to-logic-apps/copy-url.png)

1. Sonraki bölümde başvurabilmek için bu URL'yi saklayın. 

## <a name="configure-subscription-for-iot-hub-events"></a>IoT Hub olayları için aboneliği yapılandırma

Bu bölümde, IoT Hub'ınızı gerçekleşen olayları yayımlamak için yapılandıracaksınız. 

1. Azure portalında IoT Hub'ınıza gidin. Bunu **Kaynak gruplarını**seçerek, ardından bu öğretici için kaynak grubunu seçebilir ve ardından kaynaklar listesinden IoT hub'ınızı seçebilirsiniz.

2. **Olaylar**'ı seçin.

   ![Event Grid ayrıntılarını açma](./media/publish-iot-hub-events-to-logic-apps/event-grid.png)

3. **Olay aboneliği**’ni seçin. 

   ![Yeni olay aboneliği oluşturma](./media/publish-iot-hub-events-to-logic-apps/event-subscription.png)

4. Olay aboneliğini aşağıdaki değerlerle oluşturun: 

   * **Olay Abonelik Ayrıntıları**: Açıklayıcı bir ad sağlayın ve **Olay Izgara Şeması'nı**seçin.

   * **Olay Türleri**: Olay **Türlerine**Filtre'de, **Oluşturulan Aygıt**dışındaki tüm seçeneklerin işaretlerini kaldırın.

       ![abonelik olay türleri](./media/publish-iot-hub-events-to-logic-apps/subscription-event-types.png)

   * **Bitiş Noktası Ayrıntıları**: **Web Hook** olarak Bitiş Noktası Türü'nü seçin ve bir bitiş *noktası seçin* ve mantık uygulamanızdan kopyaladığınız URL'yi yapıştırın ve seçimi onaylayın.

     ![uç nokta URL'si seçme](./media/publish-iot-hub-events-to-logic-apps/endpoint-webhook.png)

   Bittiğinde, bölme aşağıdaki örnek gibi görünmelidir: 

    ![Örnek olay aboneliği formu](./media/publish-iot-hub-events-to-logic-apps/subscription-form.png)

5. Olay aboneliğini buraya kaydedebilir ve IoT Hub'ınızda oluşturulan her cihaz için bildirimler alabilirsiniz. Ancak bu öğretici için, belirli aygıtlar için filtre lemek için isteğe bağlı alanları kullanalım. Bölmenin üst kısmında **filtreler'i** seçin.

6. **Yeni filtre ekle'yi**seçin. Alanları şu değerlerle doldurun:

   * **Anahtar**: `Subject`Seçin .

   * **Operatör**: `String begins with`Seç .

   * **Değer**: `devices/Building1_` Bina 1'deki aygıt olayları için filtreye girin.
  
   Bu değerlerle başka bir filtre ekleyin:

   * **Anahtar**: `Subject`Seçin .

   * **Operatör**: `String ends with`Seç .

   * **Değer**: `_Temperature` Sıcaklıkla ilgili cihaz olayları için filtreye girin.

   Olay aboneliğinizin **Filtreler** sekmesi artık bu resme benzer olmalıdır:

   ![Olay aboneliğine filtre ekleme](./media/publish-iot-hub-events-to-logic-apps/event-subscription-filters.png)

7. Olay aboneliğini kaydetmek için **Oluştur**'u seçin.

## <a name="create-a-new-device"></a>Yeni cihaz oluşturma

Olay bildirim e-postasını tetiklemek için yeni bir cihaz oluşturarak mantıksal uygulamanızı test edin. 

1. IoT Hub'ınızda **IoT Cihazları**'nı seçin. 

2. **Yeni'yi**seçin.

3. **Cihaz Kimliği** için `Building1_Floor1_Room1_Light` girin.

4. **Kaydet'i**seçin. 

5. Olay abonelik filtrelerini test etmek için farklı cihaz kimlikleri olan birden çok cihaz ekleyebilirsiniz. Şu örnekleri deneyin: 

   * Building1_Floor1_Room1_Light
   * Building1_Floor2_Room2_Temperature
   * Building2_Floor1_Room1_Temperature
   * Building2_Floor1_Room1_Light

   Dört örnek eklediyseniz, IoT aygıtları listeniz aşağıdaki resim gibi görünmelidir:

   ![IoT Hub cihaz listesi](./media/publish-iot-hub-events-to-logic-apps/iot-hub-device-list.png)

6. IoT Hub'ınıza birkaç cihaz ekledikten sonra, hangisinin mantıksal uygulamayı tetiklediğini görmek için e-postanızı gözden geçirin. 

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

Azure portalı kullanmak yerine, IoT Hub adımlarını Azure CLI'yi kullanarak gerçekleştirebilirsiniz. Ayrıntılar için, etkinlik aboneliği ve [bir IoT aygıtı](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity) [oluşturmak](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) için Azure CLI sayfalarına bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide Azure aboneliğinize ücret uygulanmasına neden olan kaynaklar kullanılmıştır. Öğreticiyi denemeyi ve sonuçlarınızı test etmeyi bitirdiğinizde, tutmak istemediğiniz kaynakları devre dışı bırakıp silin. 

Bu öğreticide oluşturulan tüm kaynakları silmek için kaynak grubunu silin. 

1. **Kaynak gruplarını**seçin ve ardından bu öğretici için oluşturduğunuz kaynak grubunu seçin.

2. Kaynak grubu bölmesine kaynak **grubunu sil'i**seçin. Kaynak grubu adını girmeniz istenir ve ardından silebilirsiniz. Burada bulunan tüm kaynaklar da kaldırılır.

Tüm kaynakları kaldırmak istemiyorsanız, bunları tek tek yönetebilirsiniz. 

Mantıksal uygulamanızda yapılan çalışmayı kaybetmek istemiyorsanız, bunu silmek yerine devre dışı bırakın. 

1. Mantıksal uygulamanıza gidin.

2. **Genel Bakış** dikey penceresinde **Sil**'i veya **Devre Dışı Bırak**'ı seçin. 

Her aboneliğin tek bir ücretsiz IoT Hub'ı olabilir. Bu öğretici için ücretsiz bir hub oluşturduysanız, ücretleri önlemek için bunu silmeniz gerekmez.

1. IoT Hub'ınıza gidin. 

2. **Genel Bakış** dikey penceresinde **Sil**'i seçin. 

IoT Hub'ınızı korusanız bile, oluşturduğunuz olay aboneliğini silmek isteyebilirsiniz. 

1. IoT Hub'ınızda **Event Grid**'i seçin.

2. Kaldırmak istediğiniz olay aboneliğini seçin. 

3. **Sil**’i seçin. 

## <a name="next-steps"></a>Sonraki adımlar

* [Event Grid kullanıp olayları tetikleyerek IoT Hub'ı olaylarına tepki verme](../iot-hub/iot-hub-event-grid.md) hakkında daha fazla bilgi edinin.
* [Cihazla bağlantılı ve bağlantısız olayları nasıl sıralayacağınızı öğrenin](../iot-hub/iot-hub-how-to-order-connection-state-events.md)
* [Event Grid](overview.md) ile başka neler yapabileceğinizi öğrenin.