---
title: Tutorial - Use IoT Hub events to trigger Azure Logic Apps
description: Tutorial - Using the event routing service of Azure Event Grid, create automated processes to perform Azure Logic Apps actions based on IoT Hub events.
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: tutorial
ms.date: 11/21/2019
ms.author: robinsh
ms.openlocfilehash: 70ad74715446a54605a23a049ebc92a81d7ee673
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74423642"
---
# <a name="tutorial-send-email-notifications-about-azure-iot-hub-events-using-event-grid-and-logic-apps"></a>Tutorial: Send email notifications about Azure IoT Hub events using Event Grid and Logic Apps

Azure Event Grid, aşağı akış iş uygulamalarınızda eylemler tetikleyerek IoT Hub'daki olaylara karşılık vermenize olanak tanır.

This article walks through a sample configuration that uses IoT Hub and Event Grid. At the end, you have an Azure logic app set up to send a notification email every time a device is added to your IoT hub. 

## <a name="prerequisites"></a>Önkoşullar

* Azure Logic Apps tarafından desteklenen Office 365 Outlook, Outlook.com veya Gmail gibi bir e-posta sağlayıcısından alınmış e-posta hesabı. Bu e-posta hesabı olay bildirimlerini göndermek için kullanılır. Desteklenen Logic App bağlayıcılarının tam listesi için bkz. [Bağlayıcılara genel bakış](https://docs.microsoft.com/connectors/)
* Etkin bir Azure hesabı. Hesabınız yoksa [ücretsiz bir hesap oluşturabilirsiniz](https://azure.microsoft.com/pricing/free-trial/).
* Azure'da bir IoT Hub'ı. Henüz oluşturmadıysanız, yönergeler için bkz. [IoT Hub'ı kullanmaya başlama](../iot-hub/iot-hub-csharp-csharp-getstarted.md). 

## <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma

First, create a logic app and add an event grid trigger that monitors the resource group for your virtual machine. 

### <a name="create-a-logic-app-resource"></a>Mantıksal uygulama kaynağı oluşturma

1. In the [Azure portal](https://portal.azure.com), select **Create a resource**, then type "logic app" in the search box and select return. Select **Logic App** from the results.

   ![Mantıksal uygulama oluşturma](./media/publish-iot-hub-events-to-logic-apps/select-logic-app.png)

1. On the next screen, select **Create**. 

1. Mantıksal uygulamanıza aboneliğiniz içinde benzersiz olan bir ad verin, ardından IoT Hub'ınızla aynı aboneliği, kaynak grubunu ve konumu seçin. 

   ![Fields for create logic app](./media/publish-iot-hub-events-to-logic-apps/create-logic-app-fields.png)

1. **Oluştur**'u seçin.

1. Once the resource is created, navigate to your logic app. To do this, select **Resource groups**, then select the resource group you created for this tutorial. Then find the logic app in the list of resources and select it. 

1. In the Logic Apps Designer, page down to see **Templates**. Choose **Blank Logic App** so that you can build your logic app from scratch.

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

1. **Yeni adım**'ı seçin. This opens a window to **Choose an action**.

1. **E-posta**'yı arayın.

1. E-posta sağlayıcınıza uygun bağlayıcıyı bulun ve seçin. Bu öğreticide **Office 365 Outlook** kullanılır. Diğer e-posta sağlayıcılarının adımları da bunlara benzer. 

   ![E-posta sağlayıcısının bağlayıcısını seçme](./media/publish-iot-hub-events-to-logic-apps/o365-outlook.png)

1. **E-posta gönder** eylemini seçin. 

1. İstenirse, e-posta hesabınızda oturum açın. 

1. E-posta şablonunuzu oluşturun. 

   * **Kime**: Bildirim e-postalarını alacak olan e-posta adresini girin. Bu öğreticide, test etmek için erişebileceğiniz bir e-posta hesabı kullanın. 

   * **Subject**: Fill in the text for the subject. When you click on the Subject text box, you can select dynamic content to include. For example, this tutorial uses `IoT Hub alert: {event Type}`. If you can't see Dynamic content, select the **Add dynamic content** hyperlink -- this toggles it on and off.

   * **Body**: Write the text for your email. Olay verileri temelinde dinamik içerik eklemek için seçici aracından JSON özelliklerini seçin. If you can't see the Dynamic content, select the **Add dynamic content** hyperlink under the **Body** text box. If it doesn't show you the fields you want, click *more* in the Dynamic content screen to include the fields from the previous action.

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

1. Azure portalında IoT Hub'ınıza gidin. You can do this by selecting **Resource groups**, then select the resource group for this tutorial, and then select your IoT hub from the list of resources.

2. **Olaylar**'ı seçin.

   ![Event Grid ayrıntılarını açma](./media/publish-iot-hub-events-to-logic-apps/event-grid.png)

3. **Olay aboneliği**’ni seçin. 

   ![Yeni olay aboneliği oluşturma](./media/publish-iot-hub-events-to-logic-apps/event-subscription.png)

4. Olay aboneliğini aşağıdaki değerlerle oluşturun: 

   * **Event Subscription Details**: Provide a descriptive name and select **Event Grid Schema**.

   * **Event Types**: In the **Filter to Event Types**, uncheck all of the choices except **Device Created**.

       ![subscription event types](./media/publish-iot-hub-events-to-logic-apps/subscription-event-types.png)

   * **Endpoint Details**: Select Endpoint Type as **Web Hook** and select *select an endpoint* and paste the URL that you copied from your logic app and confirm selection.

     ![uç nokta URL'si seçme](./media/publish-iot-hub-events-to-logic-apps/endpoint-webhook.png)

   When you're done, the pane should look like the following example: 

    ![Örnek olay aboneliği formu](./media/publish-iot-hub-events-to-logic-apps/subscription-form.png)

5. Olay aboneliğini buraya kaydedebilir ve IoT Hub'ınızda oluşturulan her cihaz için bildirimler alabilirsiniz. For this tutorial, though, let's use the optional fields to filter for specific devices. Select **Filters** at the top of the pane.

6. Select **Add new filter**. Fill in the fields with these values:

   * **Key**: Select `Subject`.

   * **Operator**: Select `String begins with`.

   * **Value**:  Enter `devices/Building1_` to filter for device events in building 1.
  
   Add another filter with these values:

   * **Key**: Select `Subject`.

   * **Operator**: Select `String ends with`.

   * **Value**: Enter `_Temperature` to filter for device events related to temperature.

   The **Filters** tab of your event subscription should now look similar to this image:

   ![Adding filters to event subscription](./media/publish-iot-hub-events-to-logic-apps/event-subscription-filters.png)

7. Olay aboneliğini kaydetmek için **Oluştur**'u seçin.

## <a name="create-a-new-device"></a>Yeni cihaz oluşturma

Olay bildirim e-postasını tetiklemek için yeni bir cihaz oluşturarak mantıksal uygulamanızı test edin. 

1. IoT Hub'ınızda **IoT Cihazları**'nı seçin. 

2. **Yeni**'yi seçin.

3. **Cihaz Kimliği** için `Building1_Floor1_Room1_Light` girin.

4. **Kaydet**’i seçin. 

5. Olay abonelik filtrelerini test etmek için farklı cihaz kimlikleri olan birden çok cihaz ekleyebilirsiniz. Şu örnekleri deneyin: 

   * Building1_Floor1_Room1_Light
   * Building1_Floor2_Room2_Temperature
   * Building2_Floor1_Room1_Temperature
   * Building2_Floor1_Room1_Light

   If you added the four examples, your list of IoT devices should look like the following image:

   ![IoT Hub device list](./media/publish-iot-hub-events-to-logic-apps/iot-hub-device-list.png)

6. IoT Hub'ınıza birkaç cihaz ekledikten sonra, hangisinin mantıksal uygulamayı tetiklediğini görmek için e-postanızı gözden geçirin. 

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

Azure portalı kullanmak yerine, IoT Hub adımlarını Azure CLI'yi kullanarak gerçekleştirebilirsiniz. For details, see the Azure CLI pages for [creating an event subscription](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) and [creating an IoT device](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide Azure aboneliğinize ücret uygulanmasına neden olan kaynaklar kullanılmıştır. When you're finished trying out the tutorial and testing your results, disable or delete resources that you don't want to keep. 

To delete all of the resources created in this tutorial, delete the resource group. 

1. Select **Resource groups**, then select the resource group you created for this tutorial.

2. On the Resource group pane, select **Delete resource group**. You are prompted to enter the resource group name, and then you can delete it. All of the resources contained therein are also removed.

If you don't want to remove all of the resources, you can manage them one by one. 

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