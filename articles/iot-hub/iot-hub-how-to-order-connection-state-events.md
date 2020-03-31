---
title: Sipariş cihaz bağlantı olayları fr Azure IoT Hub w/Azure Cosmos DB
description: Bu makalede, en son bağlantı durumunu korumak için Azure Cosmos DB kullanarak Azure IoT Hub'dan aygıt bağlantı olaylarının nasıl sıralanıp kaydedilen
services: iot-hub
ms.service: iot-hub
author: ash2017
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: asrastog
ms.openlocfilehash: 210c2e74305ba99b4ac3a12625d0b7f5fc47ba43
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73954262"
---
# <a name="order-device-connection-events-from-azure-iot-hub-using-azure-cosmos-db"></a>Azure Cosmos DB'yi kullanarak Azure IoT Hub cihaz bağlantısı olaylarını sıralama

Azure Olay Ağıl, etkinlik tabanlı uygulamalar oluşturmanıza ve IoT olaylarını iş çözümlerinize kolayca entegre etmenize yardımcı olur. Bu makale, Cosmos DB'deki en son aygıt bağlantı durumunu izlemek ve depolamak için kullanılabilecek bir kurulumda size yol açabilirsiniz. Aygıt Bağlantısı ve Aygıt Bağlantısı Kesilen olaylarda bulunan sıra numarasını kullanacağız ve en son durumu Cosmos DB'de depolarız. Cosmos DB'deki bir koleksiyona karşı uygulanan bir uygulama mantığı olan depolanmış bir yordam kullanacağız.

Sıra numarası, hexadecimal bir sayının dize gösterimidir. Daha büyük sayıyı tanımlamak için dize karşılaştırması kullanabilirsiniz. Dizeyi hex'e dönüştürüyorsanız, sayı 256 bitlik olacaktır. Sıra numarası kesinlikle artıyor ve en son olay diğer olaylardan daha yüksek bir sayıya sahip olacak. Bu, sık sık aygıt bağlantılarını ve bağlantı larını salar ve Azure Olay Düzeni olayların sırasını desteklemediği için yalnızca en son olayın bir akış aşağı eylemini tetiklemek için kullanıldığından emin olmak istiyorsanız yararlıdır.

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir Azure hesabı. Hesabınız yoksa, [ücretsiz bir hesap oluşturabilirsiniz.](https://azure.microsoft.com/pricing/free-trial/)

* Etkin bir Azure Cosmos DB SQL API hesabı. Henüz bir tane oluşturmadıysanız, [bkz.](../cosmos-db/create-sql-api-java.md#create-a-database-account)

* Veritabanınızda bir koleksiyon. Bkz. Bir gözden geçirme için [koleksiyon ekle.](../cosmos-db/create-sql-api-java.md#add-a-container) Koleksiyonunuzu oluştururken, `/id` bölüm anahtarını kullanın.

* Azure'da bir IoT Hub'ı. Henüz oluşturmadıysanız, yönergeler için bkz. [IoT Hub'ı kullanmaya başlama](iot-hub-csharp-csharp-getstarted.md).

## <a name="create-a-stored-procedure"></a>Saklı yordam oluşturma

İlk olarak, depolanmış bir yordam oluşturun ve gelen olayların sıra numaralarını karşılaştıran ve veritabanında aygıt başına en son olayı kaydeden bir mantık çalıştırmak için ayarlayın.

1. Cosmos DB SQL API'nizde **Veri Gezgini** > **Öğeleri** > **Yeni Depolanan Yordam'ı**seçin.

   ![Depolanmış yordam oluşturma](./media/iot-hub-how-to-order-connection-state-events/create-stored-procedure.png)

2. Saklanan yordam kimliği için **En SonDeviceConnectionState'i** girin ve **Saklı Yordam gövdesine**aşağıdakileri yapıştırın. Bu kodun depolanan yordam gövdesindeki varolan kodun yerini alması gerektiğini unutmayın. Bu kod, aygıt kimliği başına bir satır tutar ve en yüksek sıra numarasını tanımlayarak bu aygıt kimliğinin en son bağlantı durumunu kaydeder.

    ```javascript
    // SAMPLE STORED PROCEDURE
    function UpdateDevice(deviceId, moduleId, hubName, connectionState, connectionStateUpdatedTime, sequenceNumber) {
      var collection = getContext().getCollection();
      var response = {};

      var docLink = getDocumentLink(deviceId, moduleId);

      var isAccepted = collection.readDocument(docLink, function(err, doc) {
        if (err) {
          console.log('Cannot find device ' + docLink + ' - ');
          createDocument();
        } else {
          console.log('Document Found - ');
          replaceDocument(doc);
        }
      });

      function replaceDocument(document) {
        console.log(
          'Old Seq :' +
            document.sequenceNumber +
            ' New Seq: ' +
            sequenceNumber +
            ' - '
        );
        if (sequenceNumber > document.sequenceNumber) {
          document.connectionState = connectionState;
          document.connectionStateUpdatedTime = connectionStateUpdatedTime;
          document.sequenceNumber = sequenceNumber;

          console.log('replace doc - ');

          isAccepted = collection.replaceDocument(docLink, document, function(
            err,
            updated
          ) {
            if (err) {
              getContext()
                .getResponse()
                .setBody(err);
            } else {
              getContext()
                .getResponse()
                .setBody(updated);
            }
          });
        } else {
          getContext()
            .getResponse()
            .setBody('Old Event - current: ' + document.sequenceNumber + ' Incoming: ' + sequenceNumber);
        }
      }
      function createDocument() {
        document = {
          id: deviceId + '-' + moduleId,
          deviceId: deviceId,
          moduleId: moduleId,
          hubName: hubName,
          connectionState: connectionState,
          connectionStateUpdatedTime: connectionStateUpdatedTime,
          sequenceNumber: sequenceNumber
        };
        console.log('Add new device - ' + collection.getAltLink());
        isAccepted = collection.createDocument(
          collection.getAltLink(),
          document,
          function(err, doc) {
            if (err) {
              getContext()
                .getResponse()
                .setBody(err);
            } else {
              getContext()
                .getResponse()
                .setBody(doc);
            }
          }
        );
      }

      function getDocumentLink(deviceId, moduleId) {
        return collection.getAltLink() + '/docs/' + deviceId + '-' + moduleId;
      }
    }
    ```

3. Saklanan yordamı kaydedin:

    ![saklanan yordamı kaydetme](./media/iot-hub-how-to-order-connection-state-events/save-stored-procedure.png)

## <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma

İlk olarak, bir mantıksal uygulama oluşturun ve sanal makineniz için kaynak grubunu izleyen bir Event Grid tetikleyicisi ekleyin.

### <a name="create-a-logic-app-resource"></a>Mantıksal uygulama kaynağı oluşturma

1. Azure [portalında](https://portal.azure.com) **+Kaynak Oluştur'** seçeneğini belirleyin, **Tümleştirme'yi** seçin ve ardından **Mantık Uygulaması'nı**seçin.

   ![Mantıksal uygulama oluşturma](./media/iot-hub-how-to-order-connection-state-events/select-logic-app.png)

2. Mantıksal uygulamanıza aboneliğiniz içinde benzersiz olan bir ad verin, ardından IoT Hub'ınızla aynı aboneliği, kaynak grubunu ve konumu seçin.

   ![Yeni mantık uygulaması](./media/iot-hub-how-to-order-connection-state-events/new-logic-app.png)

3. Mantık uygulamasını oluşturmak için **Oluştur'u** seçin.

   Mantıksal uygulamanız için bir Azure kaynağı oluşturdunuz. Azure mantıksal uygulamanızı dağıttıktan sonra Logic Apps Tasarımcısı'nda hızlı bir başlangıç yapmanıza yardımcı olacak ortak desen şablonları gösterilir.

   > [!NOTE]
   > Mantık uygulamanızı yeniden bulmak ve açmak için **Kaynak gruplarını** seçin ve bu nasıl yapılır için kullandığınız kaynak grubunu seçin. Ardından yeni mantık uygulamanızı seçin. Bu Mantık App Designer açılır.

4. Mantık Uygulama Tasarımcısı'nda, ortak tetikleyicileri görene kadar sağa kaydırın. **Şablonlar**altında, mantık uygulamanızı sıfırdan oluşturabilmeniz için **Boş Mantık Uygulaması'nı** seçin.

### <a name="select-a-trigger"></a>Tetikleyici seçme

Tetikleyici, mantıksal uygulamanızı başlatan belirli bir olaydır. Bu öğreticide, iş akışını başlatan tetikleyici HTTP üzerinden bir istek alır.

1. Bağlayıcılar ve tetikleyiciler arama çubuğunda **HTTP** yazın ve Enter tuşuna basın.

2. Tetikleyici olarak **İstek - Bir HTTP isteği alındığında**'yı seçin.

   ![HTTP istek tetikleyicisini seçme](./media/iot-hub-how-to-order-connection-state-events/http-request-trigger.png)

3. **Şema oluşturmak için örnek yük kullanma** öğesini seçin.

   ![Şema oluşturmak için örnek yükü kullanma](./media/iot-hub-how-to-order-connection-state-events/sample-payload.png)

4. Aşağıdaki örnek JSON kodunu metin kutusuna yapıştırın ve **Bitti**'yi seçin:

   ```json
   [{
    "id": "fbfd8ee1-cf78-74c6-dbcf-e1c58638ccbd",
    "topic":
      "/SUBSCRIPTIONS/DEMO5CDD-8DAB-4CF4-9B2F-C22E8A755472/RESOURCEGROUPS/EGTESTRG/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/MYIOTHUB",
    "subject": "devices/Demo-Device-1",
    "eventType": "Microsoft.Devices.DeviceConnected",
    "eventTime": "2018-07-03T23:20:11.6921933+00:00",
    "data": {
      "deviceConnectionStateEventInfo": {
        "sequenceNumber":
          "000000000000000001D4132452F67CE200000002000000000000000000000001"
      },
      "hubName": "MYIOTHUB",
      "deviceId": "48e44e11-1437-4907-83b1-4a8d7e89859e",
      "moduleId": ""
    },
    "dataVersion": "1",
    "metadataVersion": "1"
   }]
   ```

   ![Örnek JSON yükünü yapıştır](./media/iot-hub-how-to-order-connection-state-events/paste-sample-payload.png)

5. **İsteğinize Uygulama/JSON olarak ayarlanmış bir Content-Type üst bilgisi eklemeyi unutmayın** önerisinin bulunduğu bir açılan bildirim alabilirsiniz. Bu öneriyi güvenle yoksayabilir ve sonraki bölüme geçebilirsiniz.

### <a name="create-a-condition"></a>Koşul oluşturma

Mantık uygulaması iş akışınızda, koşullar belirli bir koşulu geçtikten sonra belirli eylemlerin çalıştırılmasına yardımcı olur. Koşul karşılandıktan sonra, istenen bir eylem tanımlanabilir. Bu öğretici için koşul, eventType aygıta bağlı veya aygıt bağlantısı nın bağlı olup olmadığını denetlemektir. Eylem veritabanınızda depolanan yordamı yürütmek olacaktır.

1. Seçin **+ Yeni adım** sonra **Dahili**, sonra bul ve **Durum**seçin . Bir **değer seç'i** tıklatın ve dinamik içeriği gösteren bir kutu açılır -- seçilebilen alanlar. Yalnızca Aygıta Bağlı ve Aygıt Bağlantısı Kesilen olaylar için bunu yürütmek için aşağıda gösterildiği alanları doldurun:

   * Bir değer seçin: **eventType** -- bu alana tıkladığınızda görünen dinamik içerikteki alanlardan bunu seçin.
   * "eşittir" **ile biter.**
   * Bir değer seçin: **nected**.

     ![Dolgu Durumu](./media/iot-hub-how-to-order-connection-state-events/condition-detail.png)

2. If **true** iletişim kutusunda, **eylem ekle'yi**tıklatın.
  
   ![Doğruysa eylem ekleme](./media/iot-hub-how-to-order-connection-state-events/action-if-true.png)

3. Cosmos DB'yi arayın ve **Azure Cosmos DB'yi seçin - Depolanan yordamı çalıştırın**

   ![CosmosDB ara](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-search.png)

4. **Bağlantı Adı** için **cosmosdb bağlantısını** doldurun ve tablodaki girişi seçin, ardından **Oluştur'u**seçin. **Yürüt** yordamı panelini görürsünüz. Alanların değerlerini girin:

   **Veritabanı Kimliği**: Todolist

   **Tahsilat Kimliği**: Öğeler

   **Sproc ID**: LatestDeviceConnectionState

5. **Yeni parametre ekle'yi**seçin. Görünen açılır durumda, **depolanan yordam için**Bölüm **tuşu** ve Parametreler'in yanındaki kutuları işaretleyin, ardından ekranda başka bir yere tıklayın; bölüm anahtar değeri için bir alan ve depolanan yordam için parametreler için bir alan ekler.

   ![mantık uygulaması eylem doldurmak](./media/iot-hub-how-to-order-connection-state-events/logicapp-stored-procedure.png)

6. Şimdi aşağıda gösterildiği gibi bölüm anahtar değeri ve parametreleri girin. Parantez ve çift tırnak gösterildiği gibi koymak emin olun. Burada kullanabileceğiniz geçerli değerleri almak için **dinamik içerik ekle'yi** tıklatmanız gerekebilir.

   ![mantık uygulaması eylem doldurmak](./media/iot-hub-how-to-order-connection-state-events/logicapp-stored-procedure-2.png)

7. **Her biri için**yazdığı bölmenin üst kısmında, önceki adımlardan bir çıktı seçin altında, **Gövde'nin** **seçildiğinden**emin olun.

   ![her biri için mantık uygulamasını doldurmak](./media/iot-hub-how-to-order-connection-state-events/logicapp-foreach-body.png)

8. Mantıksal uygulamanızı kaydedin.

### <a name="copy-the-http-url"></a>HTTP URL'sini kopyalama

Logic Apps Designer'dan ayrılmadan önce, mantık uygulamanızın tetikleyici olarak dinlediği URL'yi kopyalayın. Bu URL'yi, Event Grid'i yapılandırmak için kullanırsınız.

1. **Bir HTTP isteği alındığında** tetikleyici yapılandırma kutusunu tıklayarak genişletin.

2. Yanındaki kopyala düğmesini seçerek **HTTP POST URL** değerini kopyalayın.

   ![HTTP POST URL değerini kopyalama](./media/iot-hub-how-to-order-connection-state-events/copy-url.png)

3. Sonraki bölümde başvurabilmek için bu URL'yi saklayın.

## <a name="configure-subscription-for-iot-hub-events"></a>IoT Hub olayları için aboneliği yapılandırma

Bu bölümde, IoT Hub'ınızı gerçekleşen olayları yayımlamak için yapılandıracaksınız.

1. Azure portalında IoT Hub'ınıza gidin.

2. **Olaylar**'ı seçin.

   ![Event Grid ayrıntılarını açma](./media/iot-hub-how-to-order-connection-state-events/event-grid.png)

3. + **Etkinlik aboneliğini**seçin.

   ![Yeni olay aboneliği oluşturma](./media/iot-hub-how-to-order-connection-state-events/event-subscription.png)

4. Olay **Abonelik Ayrıntılarını**Doldurun : Açıklayıcı bir ad sağlayın ve **Olay Izgara Şeması'nı**seçin.

5. **Olay Türleri** alanlarını doldurun. Açılan listede, yalnızca Yalnızca **Bağlı Aygıt** ve Aygıt **Bağlantısının Kesildiğini** seçin. Listeyi kapatmak ve seçimlerinizi kaydetmek için ekrandaki herhangi bir yere tıklayın.

   ![Etkinlik türlerini arayacak şekilde ayarlama](./media/iot-hub-how-to-order-connection-state-events/set-event-types.png)

6. **Bitiş Noktası Ayrıntıları**için, Web **Kancası** olarak Bitiş Noktası Türü'nü seçin ve bitiş noktasını seçin ve mantık uygulamanızdan kopyaladığınız URL'yi yapıştırın ve seçimi onaylayın.

   ![Bitiş noktası url'si seçin](./media/iot-hub-how-to-order-connection-state-events/endpoint-select.png)

7. Form şimdi aşağıdaki örneğe benzer olmalıdır:

   ![Örnek olay aboneliği formu](./media/iot-hub-how-to-order-connection-state-events/subscription-form.png)

   Olay aboneliğini kaydetmek için **Oluştur**'u seçin.

## <a name="observe-events"></a>Olayları gözlemle

Artık etkinlik aboneliğiniz ayarlandığına göre, bir aygıtı bağlayarak test edelim.

### <a name="register-a-device-in-iot-hub"></a>Aygıtı IoT Hub'ına kaydetme

1. IoT Hub'ınızda **IoT Cihazları**'nı seçin.

2. Bölmenin üst kısmında **+Ekle'yi** seçin.

3. **Cihaz Kimliği** için `Demo-Device-1` girin.

4. **Kaydet'i**seçin.

5. Farklı aygıt tbm'leri ile birden çok aygıt ekleyebilirsiniz.

   ![Hub'a eklenen aygıtlar](./media/iot-hub-how-to-order-connection-state-events/AddIoTDevice.png)

6. Cihaza tekrar tıklayın; şimdi bağlantı dizeleri ve anahtarları doldurulacaktır. Bağlantı **dizesini kopyalayın --** daha sonra kullanmak için birincil anahtar.

   ![Aygıt için ConnectionString](./media/iot-hub-how-to-order-connection-state-events/DeviceConnString.png)

### <a name="start-raspberry-pi-simulator"></a>Raspberry Pi simülatörü başlatın

Cihaz bağlantısını simüle etmek için Raspberry Pi web simülatörünü kullanalım.

[Raspberry Pi simülatörü başlatın](https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted)

### <a name="run-a-sample-application-on-the-raspberry-pi-web-simulator"></a>Raspberry Pi web simülatöründe örnek bir uygulama çalıştırın

Bu, aygıta bağlı bir olayı tetikler.

1. Kodlama alanında, Satır 15'teki yer tutucuyu önceki bölümün sonunda kaydettiğiniz Azure IoT Hub aygıt bağlantı dizenizile değiştirin.

   ![Aygıt bağlantı dizesinde yapıştır](./media/iot-hub-how-to-order-connection-state-events/raspconnstring.png)

2. Çalıştır'ı seçerek uygulamayı **çalıştırın.**

Sensör verilerini ve IoT hub'ınıza gönderilen iletileri gösteren aşağıdaki çıktıya benzer bir şey görürsünüz.

   ![Uygulamayı çalıştırma](./media/iot-hub-how-to-order-connection-state-events/raspmsg.png)

   Simülatörü durdurmak ve **Aygıt Bağlantısı Kesilen** olayı tetiklemek için **Durdur'u** tıklatın.

Şimdi sensör verileri toplamak ve IoT hub'ınıza göndermek için bir örnek uygulama çalıştırın.

### <a name="observe-events-in-cosmos-db"></a>Cosmos DB'deki olayları gözlemleyin

Cosmos DB belgenizde çalıştırılan saklı yordamın sonuçlarını görebilirsiniz. Aşağıdaki görüntüde olduğu gibi görünmelidir. Her satır, aygıt başına en son aygıt bağlantı durumunu içerir.

   ![Nasıl sonuç](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-outcome.png)

## <a name="use-the-azure-cli"></a>Azure CLI kullanma

[Azure portalını](https://portal.azure.com)kullanmak yerine, Azure CLI'yi kullanarak IoT Hub adımlarını gerçekleştirebilirsiniz. Ayrıntılar için, etkinlik aboneliği ve [bir IoT aygıtı](/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity#ext-azure-cli-iot-ext-az-iot-hub-device-identity-create) [oluşturmak](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) için Azure CLI sayfalarına bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide Azure aboneliğinize ücret uygulanmasına neden olan kaynaklar kullanılmıştır. Öğreticiyi denemeyi ve sonuçlarınızı test etmeyi bitirdiğinizde, tutmak istemediğiniz kaynakları devre dışı bırakıp silin.

Mantıksal uygulamanızda yapılan çalışmayı kaybetmek istemiyorsanız, bunu silmek yerine devre dışı bırakın.

1. Mantıksal uygulamanıza gidin.

2. Genel **Bakış** bıçağında **Sil** veya **Devre Dışı'** yı seçin.

    Her aboneliğin tek bir ücretsiz IoT Hub'ı olabilir. Bu öğretici için ücretsiz bir hub oluşturduysanız, ücretleri önlemek için bunu silmeniz gerekmez.

3. IoT Hub'ınıza gidin.

4. Genel **Bakış** bıçağında **Sil'i**seçin.

    IoT Hub'ınızı korusanız bile, oluşturduğunuz olay aboneliğini silmek isteyebilirsiniz.

5. IoT Hub'ınızda **Event Grid**'i seçin.

6. Kaldırmak istediğiniz olay aboneliğini seçin.

7. **Sil**’i seçin.

Azure portalından bir Azure Cosmos DB hesabını kaldırmak için hesap adını sağ tıklatın ve **Hesabı Sil'i**tıklatın. [Azure Cosmos DB hesabını silme](https://docs.microsoft.com/azure/cosmos-db/manage-account)yle ilgili ayrıntılı yönergelere bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Eylemleri tetiklemek için Olay Grid'i kullanarak IoT Hub olaylarına tepki](../iot-hub/iot-hub-event-grid.md) verme hakkında daha fazla bilgi edinin

* [IoT Hub etkinlikleri öğreticisini deneyin](../event-grid/publish-iot-hub-events-to-logic-apps.md)

* [Olay Izgara'da](../event-grid/overview.md) başka neler yapabileceğiniz hakkında bilgi edinin