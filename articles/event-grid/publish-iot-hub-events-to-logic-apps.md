---
title: Öğretici-tetiklenecek IoT Hub olaylarını kullanın Azure Logic Apps
description: Bu öğreticide, IoT Hub olaylara göre Azure Logic Apps eylemleri gerçekleştirmek üzere otomatik işlemler oluşturmak için Azure Event Grid olay yönlendirme hizmetinin nasıl kullanılacağı gösterilmektedir.
services: iot-hub, event-grid
author: robinsh
ms.service: iot-hub
ms.topic: tutorial
ms.date: 07/07/2020
ms.author: robinsh
ms.custom: devx-track-azurecli
ms.openlocfilehash: 35359c63b79d9eea6f8f6ad688bd040428a39eb8
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87503455"
---
# <a name="tutorial-send-email-notifications-about-azure-iot-hub-events-using-event-grid-and-logic-apps"></a>Öğretici: Event Grid ve Logic Apps kullanarak Azure IoT Hub olayları hakkında e-posta bildirimleri gönderme

Azure Event Grid, aşağı akış iş uygulamalarınızda eylemler tetikleyerek IoT Hub'daki olaylara karşılık vermenize olanak tanır.

Bu makalede IoT Hub ve Event Grid kullanan bir örnek yapılandırma gösterilmektedir. Son olarak, IoT Hub 'ınıza bir cihaz eklendiğinde bildirim e-postası gönderecek şekilde ayarlanmış bir Azure mantıksal uygulamanız vardır. 

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir Azure aboneliği. Aboneliğiniz yoksa [ücretsiz bir Azure hesabı oluşturabilirsiniz](https://azure.microsoft.com/pricing/free-trial/).

* Office 365 Outlook, Outlook.com veya Gmail gibi Azure Logic Apps tarafından desteklenen herhangi bir e-posta sağlayıcısından bir e-posta hesabı. Bu e-posta hesabı olay bildirimlerini göndermek için kullanılır. Desteklenen mantıksal uygulama bağlayıcılarının tüm listesi için bkz. [bağlayıcılara genel bakış](/connectors/).

  > [!IMPORTANT]
  > Gmail kullanmadan önce bir G-Suite iş hesabınız olup olmadığını (özel etki alanı olan e-posta adresi) veya Gmail tüketicisi hesabını (veya içeren e-posta adresini @gmail.com @googlemail.com ) denetleyin. Yalnızca G-Suite iş hesapları, mantıksal uygulamalarda kısıtlama olmadan Gmail bağlayıcısını diğer bağlayıcılarla kullanabilir. Gmail tüketicisi hesabınız varsa, Gmail bağlayıcısını yalnızca belirli Google onaylı hizmetlerle kullanabilir veya [kimlik doğrulaması için kullanmak üzere bir Google istemci uygulaması oluşturabilirsiniz](/connectors/gmail/#authentication-and-bring-your-own-application). Daha fazla bilgi için, bkz. [Azure Logic Apps Google bağlayıcıları Için veri güvenliği ve gizlilik ilkeleri](../connectors/connectors-google-data-security-privacy-policy.md).

* Azure'da bir IoT Hub'ı. Henüz oluşturmadıysanız, yönergeler için bkz. [IoT Hub'ı kullanmaya başlama](../iot-hub/quickstart-send-telemetry-dotnet.md).

## <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma

İlk olarak, bir mantıksal uygulama oluşturun ve sanal makinenizin kaynak grubunu izleyen bir olay Kılavuzu tetikleyicisi ekleyin. 

### <a name="create-a-logic-app-resource"></a>Mantıksal uygulama kaynağı oluşturma

1. [Azure Portal](https://portal.azure.com), **kaynak oluştur**' u seçin, sonra arama kutusuna "Logic App" yazın ve geri dön ' ü seçin. Sonuçlardan **Logic App** ' i seçin.

   ![Mantıksal uygulama oluşturma](./media/publish-iot-hub-events-to-logic-apps/select-logic-app.png)

1. Sonraki ekranda **Oluştur**' u seçin. 

1. Mantıksal uygulamanıza aboneliğiniz içinde benzersiz olan bir ad verin, ardından IoT Hub'ınızla aynı aboneliği, kaynak grubunu ve konumu seçin. 

   ![Mantıksal uygulama oluşturma alanları](./media/publish-iot-hub-events-to-logic-apps/create-logic-app-fields.png)

1. **Oluştur**’u seçin.

1. Kaynak oluşturulduktan sonra mantıksal uygulamanıza gidin. Bunu yapmak için **kaynak grupları**' nı seçin ve ardından Bu öğretici için oluşturduğunuz kaynak grubunu seçin. Ardından mantıksal uygulamayı kaynak listesinden bulun ve seçin. 

1. Logic Apps tasarımcısında, **şablonları**görmek için sayfayı aşağı kaydırın. Mantıksal uygulamanızı sıfırdan oluşturabilmeniz için **boş mantıksal uygulama** ' yı seçin.

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

### <a name="create-an-action"></a>Eylem oluşturma

Eylemler, tetikleyici mantıksal uygulama iş yükünü başlattıktan sonra gerçekleşen adımlardır. Bu öğreticide, e-posta sağlayıcınızdan bir e-posta bildirimi gönderme eylemi kullanılır. 

1. **Yeni adım**'ı seçin. Bu **işlem, bir eylemi seçmek**için bir pencere açar.

1. **E-posta**'yı arayın.

1. E-posta sağlayıcınıza uygun bağlayıcıyı bulun ve seçin. Bu öğreticide **Office 365 Outlook** kullanılır. Diğer e-posta sağlayıcılarının adımları da bunlara benzer. 

   ![E-posta sağlayıcısının bağlayıcısını seçme](./media/publish-iot-hub-events-to-logic-apps/o365-outlook.png)

1. **E-posta gönder** eylemini seçin. 

1. İstenirse, e-posta hesabınızda oturum açın. 

1. E-posta şablonunuzu oluşturun. 

   * **Kime**: Bildirim e-postalarını alacak e-posta adresini girin. Bu öğreticide, test için erişebileceğiniz bir e-posta hesabı kullanın. 

   * **Konu**: Konunun metnini girin. Konu metin kutusuna tıkladığınızda, içerilecek dinamik içerik ' i seçebilirsiniz. Örneğin, bu öğretici kullanır `IoT Hub alert: {event Type}` . Dinamik içeriği göremiyorsanız, **dinamik Içerik Ekle** köprüsünü seçin--Bu, açıp kapatır.

   * **Gövde**: E-postanız için metni yazın. Olay verilerine göre dinamik içerik eklemek için seçici aracından JSON özelliklerini seçin. Dinamik içeriği göremiyorsanız **gövde** metin kutusu altında **dinamik içerik Ekle** köprüsünü seçin. İstediğiniz alanları göstermezse, önceki eylemden alanları dahil etmek için dinamik içerik ekranında *daha fazla* ' ya tıklayın.

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

1. Azure portalında IoT Hub'ınıza gidin. Bunu, **kaynak grupları**' nı seçerek ve ardından Bu öğreticinin kaynak grubunu seçip kaynak listesinden IoT Hub 'ınızı seçerek yapabilirsiniz.

2. **Olaylar**'ı seçin.

   ![Event Grid ayrıntılarını açma](./media/publish-iot-hub-events-to-logic-apps/event-grid.png)

3. **Olay aboneliği**’ni seçin. 

   ![Yeni olay aboneliği oluşturma](./media/publish-iot-hub-events-to-logic-apps/event-subscription.png)

4. Olay aboneliğini aşağıdaki değerlerle oluşturun: 

    1. **Olay ABONELIĞI ayrıntıları** bölümünde aşağıdaki görevleri yapın:
        1. Olay aboneliği için bir **ad** sağlayın. 
        2. **Olay şeması**Için **Event Grid şeması** ' nı seçin. 
   2. **Konu ayrıntıları** bölümünde aşağıdaki görevleri yapın:
       1. **Konu türünün** **IoT Hub**olarak ayarlandığını onaylayın. 
       2. IoT Hub 'ın adının **kaynak kaynak** alanı için değer olarak ayarlandığını doğrulayın. 
       3. Sizin için oluşturulacak **Sistem konusu** için bir ad girin. Sistem konuları hakkında bilgi edinmek için bkz. [sistem konularına genel bakış](system-topics.md).
   3. **Olay türleri** bölümünde aşağıdaki görevleri yapın: 
        1. **Olay türlerine filtre uygulamak**Için, **cihaz oluşturma**dışında tüm seçimlerin işaretini kaldırın.

           ![abonelik olay türleri](./media/publish-iot-hub-events-to-logic-apps/subscription-event-types.png)
   4. **Uç nokta ayrıntıları** bölümünde aşağıdaki görevleri yapın: 
       1. **Uç nokta türünü** **Web kancası**olarak seçin.
       2. **Uç nokta seç**' e tıklayın, mantıksal UYGULAMANıZDAN kopyaladığınız URL 'yi yapıştırın ve seçimi onaylayın.

         ![uç nokta URL'si seçme](./media/publish-iot-hub-events-to-logic-apps/endpoint-webhook.png)

         İşiniz bittiğinde, bölme aşağıdaki örnekteki gibi görünmelidir: 

        ![Örnek olay aboneliği formu](./media/publish-iot-hub-events-to-logic-apps/subscription-form.png)

5. Olay aboneliğini buraya kaydedebilir ve IoT Hub'ınızda oluşturulan her cihaz için bildirimler alabilirsiniz. Bu öğreticide, belirli cihazları filtrelemek için isteğe bağlı alanları kullanalım. Bölmenin üst kısmındaki **Filtreler**’i seçin.

6. **Yeni filtre ekle**’yi seçin. Alanları şu değerlerle doldurun:

   * **Anahtar**: `Subject` öğesini seçin.

   * **İşleç**: `String begins with` öğesini seçin.

   * **Değer**: `devices/Building1_` 1 binasında cihaz olaylarını filtrelemek için girin.
  
   Şu değerlere sahip başka bir filtre ekleyin:

   * **Anahtar**: `Subject` öğesini seçin.

   * **İşleç**: `String ends with` öğesini seçin.

   * **Değer**: Sıcaklığa ilişkin cihaz olaylarını filtrelemek için `_Temperature` girin.

   Olay aboneliğinizin **Filtreler** sekmesi şu resme benzer şekilde görünmelidir:

   ![Olay aboneliğine filtreler ekleniyor](./media/publish-iot-hub-events-to-logic-apps/event-subscription-filters.png)

7. Olay aboneliğini kaydetmek için **Oluştur**’u seçin.

## <a name="create-a-new-device"></a>Yeni cihaz oluşturma

Bir olay bildirimi e-postası tetiklemek için yeni bir cihaz oluşturarak mantıksal uygulamanızı test edin. 

1. IoT Hub 'ınızdan **IoT cihazları**' nı seçin. 

2. **Yeni**’yi seçin.

3. **Cihaz kimliği** için, `Building1_Floor1_Room1_Light` girin.

4. **Kaydet**’i seçin. 

5. Olay abonelik filtrelerini test etmek için farklı cihaz kimlikleri olan birden çok cihaz ekleyebilirsiniz. Şu örnekleri deneyin: 

   * Building1_Floor1_Room1_Light
   * Building1_Floor2_Room2_Temperature
   * Building2_Floor1_Room1_Temperature
   * Building2_Floor1_Room1_Light

   Dört örneği eklediyseniz, IoT cihazları listenizin aşağıdaki görüntüde olduğu gibi görünmesi gerekir:

   ![IoT Hub cihaz listesi](./media/publish-iot-hub-events-to-logic-apps/iot-hub-device-list.png)

6. IoT Hub'ınıza birkaç cihaz ekledikten sonra, hangisinin mantıksal uygulamayı tetiklediğini görmek için e-postanızı gözden geçirin. 

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

Azure portalı kullanmak yerine, IoT Hub adımlarını Azure CLI'yi kullanarak gerçekleştirebilirsiniz. Ayrıntılar için, [olay aboneliği oluşturmak](/cli/azure/eventgrid/event-subscription) ve [IoT cihazı oluşturmak](/cli/azure/ext/azure-iot/iot/hub/device-identity)için Azure CLI sayfalarına bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide Azure aboneliğinize ücret uygulanmasına neden olan kaynaklar kullanılmıştır. Öğreticiyi denemeyi ve sonuçlarınızı test etmeyi tamamladığınızda, saklamak istemediğiniz kaynakları devre dışı bırakın veya silin. 

Bu öğreticide oluşturulan tüm kaynakları silmek için kaynak grubunu silin. 

1. **Kaynak grupları**' nı seçin ve ardından Bu öğretici için oluşturduğunuz kaynak grubunu seçin.

2. Kaynak grubu bölmesinde **kaynak grubunu sil**' i seçin. Kaynak grubu adını girmeniz istenir ve ardından onu silebilirsiniz. İçinde bulunan tüm kaynaklar da kaldırılır.

Tüm kaynakları kaldırmak istemiyorsanız, onları tek tek yönetebilirsiniz. 

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
