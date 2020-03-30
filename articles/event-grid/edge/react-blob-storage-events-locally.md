---
title: Blob Depolama modülü etkinliklerine tepki - Azure Event Grid IoT Edge | Microsoft Dokümanlar
description: Blob Depolama modülü olaylarına tepki verme
author: arduppal
manager: brymat
ms.author: arduppal
ms.reviewer: spelluru
ms.date: 12/13/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 3360b92a1b71adcbf0364a16c197aecdab5700db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77086610"
---
# <a name="tutorial-react-to-blob-storage-events-on-iot-edge-preview"></a>Öğretici: IoT Edge'deki Blob Depolama etkinliklerine tepki ver (Önizleme)
Bu makalede, Blob oluşturma ve Blob silme olay Grid'e etkinlikler göndermek için bir Olay Izgara yayımcısı olarak hareket edecek Olan IoT modülünde Azure Blob Depolama sını nasıl dağıtacağınızı gösterir.  

IoT Edge'deki Azure Blob Depolama'ya genel bakış için [IoT Edge'deki Azure Blob Depolama'ya](../../iot-edge/how-to-store-data-blob.md) ve özelliklerine bakın.

> [!WARNING]
> Olay Izgaraile IoT Edge entegrasyonunda Azure Blob Depolama Önizleme'de

Bu öğreticiyi tamamlamak için şunları yapmanız gerekir:

* **Azure aboneliği** - Zaten hesabınız yoksa ücretsiz bir [hesap](https://azure.microsoft.com/free) oluşturun. 
* **Azure IoT Hub ve IoT Edge aygıtı** - Zaten sahip değilseniz [Linux](../../iot-edge/quickstart-linux.md) veya [Windows aygıtları](../../iot-edge/quickstart.md) için hızlı başlatma adımlarını izleyin.

## <a name="deploy-event-grid-iot-edge-module"></a>Olay Izgara IoT Edge modüllerini dağıtın

Modülleri bir IoT Edge aygıtına dağıtmanın çeşitli yolları vardır ve bunların tümü IoT Edge'deki Azure Olay Izgarası için çalışır. Bu makalede, Azure portalından IoT Edge'de Olay Izgarasını dağıtma adımları açıklanmaktadır.

>[!NOTE]
> Bu eğitimde, Etkinlik Izgara modüllerini kalıcılık olmadan dağıtmış olursunuz. Bu, modülü yeniden dağıttığınızda bu öğreticide oluşturduğunuz tüm konuların ve aboneliklerin silineceği anlamına gelir. Kalıcılığı nasıl düzenesin hakkında daha fazla bilgi için aşağıdaki makalelere bakın: [Linux'ta kalıcı durum](persist-state-linux.md) veya [Windows'da kalıcı durum.](persist-state-windows.md) Üretim iş yükleri için Olay Izgara modüllerini kalıcı olarak yüklemenizi öneririz.


### <a name="select-your-iot-edge-device"></a>IoT Edge cihazınızı seçin

1. [Azure portalında](https://portal.azure.com) oturum açın
1. IoT Hub'ınıza gidin.
1. **Otomatik Aygıt Yönetimi** bölümündeki menüden **IoT Edge'i** seçin. 
1. Aygıtlar listesinden hedef aygıtın kimliğine tıklayın
1. **Modülleri Ayarlama**'yı seçin. Sayfayı açık tutun. Sonraki bölümdeki adımlarla devam eceksiniz.

### <a name="configure-a-deployment-manifest"></a>Dağıtım bildirimini yapılandırma

Dağıtım bildirimi, hangi modüllerin dağıtılabildiğini, modüller arasında verilerin nasıl aktığını ve modülün istenilen özelliklerini açıklayan bir JSON belgesidir. Azure portalında, JSON belgesini el ile oluşturmak yerine dağıtım bildirimi oluşturmada size yol gösteren bir sihirbaz vardır.  Üç adımı vardır: **Modülekleme,** **Rotalar belirt**ve **dağıtımı gözden geçirin.**

### <a name="add-modules"></a>Modül ekle

1. Dağıtım **Modülleri** bölümünde Ekle'yi **seçin**
1. Açılan listedeki modül türlerine göre **IoT Edge Modülünü** seçin
1. Kapsayıcının adını, görüntüsünü, kapsayıcıyı oluşturma seçeneklerini sağlayın:

   * **Adı**: eventgridmodule
   * **Resim URI**:`mcr.microsoft.com/azure-event-grid/iotedge:latest`
   * **Konteyner Oluşturma Seçenekleri**:

    ```json
        {
          "Env": [
           "inbound__serverAuth__tlsPolicy=enabled",
           "inbound__clientAuth__clientCert__enabled=false"
          ],
          "HostConfig": {
            "PortBindings": {
              "4438/tcp": [
                {
                    "HostPort": "4438"
                }
              ]
            }
          }
        }
    ```    

 1. **Kaydet'i** tıklatın
 1. Azure Olay Izgara Abone modüllerini birlikte dağıtmadan önce eklemek için bir sonraki bölüme devam edin.

    >[!IMPORTANT]
    > Bu eğitimde, hem HTTP/HTTPs isteklerine, hem de istemci kimlik doğrulamasına devre dışı kılabilmesi için Olay Izgara modüllerini dağıtmayı öğreneceksiniz. Üretim iş yükleri için, yalnızca HTTP isteklerini ve istemci kimlik doğrulaması etkin leştirilmiş aboneleri etkinleştirmenizi öneririz. Olay Izgara modüllerini güvenli bir şekilde yapılandırma hakkında daha fazla bilgi için [Güvenlik ve kimlik doğrulama](security-authentication.md)sına bakın.
    

## <a name="deploy-event-grid-subscriber-iot-edge-module"></a>Olay Izgara Abone IoT Edge modüldağıtma

Bu bölümde, olayların teslim edilebildiği bir olay işleyicisi olarak hareket edecek başka bir IoT modülü nasıl dağıtılacanız gösterilmektedir.

### <a name="add-modules"></a>Modül ekle

1. Dağıtım **Modülleri** bölümünde yeniden **ekle'yi** seçin. 
1. Açılan listedeki modül türlerine göre **IoT Edge Modülünü** seçin
1. Kapsayıcının ad, görüntü ve kapsayıcı oluşturma seçeneklerini sağlayın:

   * **Ad**: abone
   * **Resim URI**:`mcr.microsoft.com/azure-event-grid/iotedge-samplesubscriber:latest`
   * **Kapsayıcı Oluşturma Seçenekleri**: Yok
1. **Kaydet'i** tıklatın
1. Azure Blob Depolama modüllerini eklemek için sonraki bölüme devam edin

## <a name="deploy-azure-blob-storage-module"></a>Azure Blob Depolama modüllerini dağıtma

Bu bölümde, Blob oluşturulan ve silinen olayları yayımlayan bir Olay Grid yayımcısı olarak hareket edecek olan Azure Blob Depolama modülü nasıl dağıtılacanız gösterilmektedir.

### <a name="add-modules"></a>Modül ekle

1. Dağıtım **Modülleri** bölümünde Ekle'yi **seçin**
2. Açılan listedeki modül türlerine göre **IoT Edge Modülünü** seçin
3. Kapsayıcının ad, görüntü ve kapsayıcı oluşturma seçeneklerini sağlayın:

   * **Adı**: azureblobstorageoniotedge
   * **Resim URI**: mcr.microsoft.com/azure-blob-storage:latest
   * **Konteyner Oluşturma Seçenekleri**:

   ```json
       {
         "Env":[
           "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
           "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>",
           "EVENTGRID_ENDPOINT=http://<event grid module name>:5888"
         ],
         "HostConfig":{
           "Binds":[
               "<storage mount>"
           ],
           "PortBindings":{
             "11002/tcp":[{"HostPort":"11002"}]
           }
         }
       }
   ```

   > [!IMPORTANT]
   > - Blob Depolama modülü hem HTTPS hem de HTTP kullanarak olayları yayımlayabilir. 
   > - EventGrid için istemci tabanlı kimlik doğrulamasını etkinleştirdiyseniz, aşağıdaki gibi https'ye izin `EVENTGRID_ENDPOINT=https://<event grid module name>:4438`vermek için EVENTGRID_ENDPOINT değerini güncelleştirdiğinizden emin olun: .
   > - Ayrıca yukarıdaki Json başka bir ortam değişkeni `AllowUnknownCertificateAuthority=true` ekleyin. EventGrid ile HTTPS üzerinden konuşurken, **AllowUnknownCertificateAuthority** depolama modülünün kendi imzaladığı EventGrid sunucu sertifikalarına güvenmesine olanak tanır.

4. Aşağıdaki bilgilerle kopyaladığınız JSON'u güncelleştirin:

   - Hatırlayabileceğiniz bir adla değiştirin. `<your storage account name>` Hesap adları 3 ila 24 karakter uzunluğunda, küçük harfler ve sayılarla olmalıdır. Yer yok.

   - 64 baytlık base64 tuşu ile değiştirin. `<your storage account key>` [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64)gibi araçlarla bir anahtar oluşturabilirsiniz. Bu kimlik bilgilerini diğer modüllerden blob depolamasına erişmek için kullanırsınız.

   - Olay `<event grid module name>` Izgara modülünüzün adı ile değiştirin.
   - Konteyner `<storage mount>` işletim sisteminize göre değiştirin.
     - Linux kapları için, **benim hacmim:/blobroot**
     - Windows kapsayıcıları için,**benim hacmim:C:/BlobRoot**

5. **Kaydet'i** tıklatın
6. Rotalar bölümüne devam etmek için **İleri'yi** tıklatın

    > [!NOTE]
    > Kenar aygıtı olarak bir Azure VM kullanıyorsanız, bu eğitimde kullanılan ana bilgisayar bağlantı noktalarında gelen trafiğin eve girmesine izin vermek için gelen bağlantı noktası kuralını ekleyin: 4438, 5888, 8080 ve 11002. Kuralı ekleme yönergeleri için, [bağlantı noktalarını VM'ye nasıl açacağına](../../virtual-machines/windows/nsg-quickstart-portal.md)bakın.

### <a name="setup-routes"></a>Kurulum rotaları

Varsayılan yolları tutun ve inceleme bölümüne devam etmek için **Sonraki'ni** seçin

### <a name="review-deployment"></a>Dağıtımı gözden geçirme

1. İnceleme bölümü, önceki bölümdeki seçimlerinize göre oluşturulan JSON dağıtım bildirimini gösterir. Aşağıdaki dört modülü gördüğünüzü onaylayın: **$edgeAgent,** **$edgeHub,** **eventgridmodule,** **abone** ve **azureblobstorageoniotedge** tüm dağıtılan.
2. Dağıtım bilgilerinizi gözden geçirin ve **ardından Gönder'i**seçin.

## <a name="verify-your-deployment"></a>Dağıtımınızı doğrulayın

1. Dağıtımı gönderdikten sonra, IoT hub'ınızın IoT Edge sayfasına geri dönersiniz.
2. Ayrıntıları açmak için dağıtımla hedeflediğiniz **IoT Edge aygıtını** seçin.
3. Aygıt ayrıntılarında, eventgridmodule, abone ve azureblobstorageoniotedge modüllerinin hem **dağıtımda belirtilen** hem de aygıt tarafından bildirilen modüller olarak listelendirilip **doğrulayın.**

   Modülün cihazda başlatılması ve ardından IoT Hub'a rapor edilmesi birkaç dakika sürebilir. Güncelleştirilmiş bir durumu görmek için sayfayı yenileyin.

## <a name="publish-blobcreated-and-blobdeleted-events"></a>BlobCreated ve BlobDeleted olaylarını yayımla

1. Bu modül otomatik olarak konu **MicrosoftStorage**oluşturur. Var olduğunu doğrulama
    ```sh
    curl -k -H "Content-Type: application/json" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/MicrosoftStorage?api-version=2019-01-01-preview
    ```

    Örnek çıktı:

    ```json
        [
          {
            "id": "/iotHubs/eg-iot-edge-hub/devices/eg-edge-device/modules/eventgridmodule/topics/MicrosoftStorage",
            "name": "MicrosoftStorage",
            "type": "Microsoft.EventGrid/topics",
            "properties": {
              "endpoint": "https://<edge-vm-ip>:4438/topics/MicrosoftStorage/events?api-version=2019-01-01-preview",
              "inputSchema": "EventGridSchema"
            }
          }
        ]
    ```

    > [!IMPORTANT]
    > - HTTPS akışı için, istemci kimlik doğrulaması SAS tuşu ile etkinleştirilirse, daha önce belirtilen SAS tuşu üstbilgi olarak eklenmelidir. Bu nedenle kıvırma isteği olacaktır:`curl -k -H "Content-Type: application/json" -H "aeg-sas-key: <your SAS key>" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/MicrosoftStorage?api-version=2019-01-01-preview`
    > - HTTPS akışı için, istemci kimlik doğrulaması sertifika ile etkinleştirilirse, kıvırma isteği aşağıdakiolacaktır:`curl -k -H "Content-Type: application/json" --cert <certificate file> --key <certificate private key file> -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/MicrosoftStorage?api-version=2019-01-01-preview`

2. Aboneler bir konuyla yayınlanan etkinlikler için kayıt yaptırabilirler. Herhangi bir olay almak için **MicrosoftStorage** konusu için bir Olay Izgara sıyrık aboneliği oluşturmanız gerekir.
    1. Aşağıdaki içerikle blobsubscription.json oluşturun. Yük hakkında ayrıntılı bilgi için [API belgelerimize](api.md) bakın

       ```json
        {
          "properties": {
            "destination": {
              "endpointType": "WebHook",
              "properties": {
                "endpointUrl": "https://subscriber:4430"
              }
            }
          }
        }
       ```

       >[!NOTE]
       > **EndpointType** özelliği, abonenin bir **Webhook**olduğunu belirtir.  **EndpointUrl,** abonenin olayları dinlediği URL'yi belirtir. Bu URL, daha önce dağıtdığınız Azure İşlevi örneğine karşılık gelir.

    2. Konu için abonelik oluşturmak için aşağıdaki komutu çalıştırın. HTTP durum kodunun . `200 OK`

       ```sh
       curl -k -H "Content-Type: application/json" -X PUT -g -d @blobsubscription.json https://<your-edge-device-public-ip-here>:4438/topics/MicrosoftStorage/eventSubscriptions/sampleSubscription5?api-version=2019-01-01-preview
       ```

       > [!IMPORTANT]
       > - HTTPS akışı için, istemci kimlik doğrulaması SAS tuşu ile etkinleştirilirse, daha önce belirtilen SAS tuşu üstbilgi olarak eklenmelidir. Bu nedenle kıvırma isteği olacaktır:`curl -k -H "Content-Type: application/json" -H "aeg-sas-key: <your SAS key>" -X PUT -g -d @blobsubscription.json https://<your-edge-device-public-ip-here>:4438/topics/MicrosoftStorage/eventSubscriptions/sampleSubscription5?api-version=2019-01-01-preview` 
       > - HTTPS akışı için, istemci kimlik doğrulaması sertifika ile etkinleştirilirse, kıvırma isteği aşağıdakiolacaktır:`curl -k -H "Content-Type: application/json" --cert <certificate file> --key <certificate private key file> -X PUT -g -d @blobsubscription.json https://<your-edge-device-public-ip-here>:4438/topics/MicrosoftStorage/eventSubscriptions/sampleSubscription5?api-version=2019-01-01-preview`

    3. Aboneliğin başarıyla oluşturulduğunu doğrulamak için aşağıdaki komutu çalıştırın. 200 OK HTTP Durum Kodu döndürülmelidir.

       ```sh
       curl -k -H "Content-Type: application/json" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/MicrosoftStorage/eventSubscriptions/sampleSubscription5?api-version=2019-01-01-preview
       ```

       Örnek çıktı:

       ```json
        {
          "id": "/iotHubs/eg-iot-edge-hub/devices/eg-edge-device/modules/eventgridmodule/topics/MicrosoftStorage/eventSubscriptions/sampleSubscription5",
          "type": "Microsoft.EventGrid/eventSubscriptions",
          "name": "sampleSubscription5",
          "properties": {
            "Topic": "MicrosoftStorage",
            "destination": {
              "endpointType": "WebHook",
              "properties": {
                "endpointUrl": "https://subscriber:4430"
              }
            }
          }
        }
       ```

       > [!IMPORTANT]
       > - HTTPS akışı için, istemci kimlik doğrulaması SAS tuşu ile etkinleştirilirse, daha önce belirtilen SAS tuşu üstbilgi olarak eklenmelidir. Bu nedenle kıvırma isteği olacaktır:`curl -k -H "Content-Type: application/json" -H "aeg-sas-key: <your SAS key>" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/MicrosoftStorage/eventSubscriptions/sampleSubscription5?api-version=2019-01-01-preview`
       > - HTTPS akışı için, istemci kimlik doğrulaması sertifika ile etkinleştirilirse, kıvırma isteği aşağıdakiolacaktır:`curl -k -H "Content-Type: application/json" --cert <certificate file> --key <certificate private key file> -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/MicrosoftStorage/eventSubscriptions/sampleSubscription5?api-version=2019-01-01-preview`

3. [Azure Depolama Gezgini'ni](https://azure.microsoft.com/features/storage-explorer/) indirin ve [yerel depolama alanınıza bağlayın](../../iot-edge/how-to-store-data-blob.md#connect-to-your-local-storage-with-azure-storage-explorer)

## <a name="verify-event-delivery"></a>Olay teslimini doğrulama

### <a name="verify-blobcreated-event-delivery"></a>BlobCreated olay teslimini doğrulayın

1. Azure Depolama Gezgini'nden yerel depolama alanına blok blobs olarak dosya yükleyin ve modül oluşturma olaylarını otomatik olarak yayımlayın. 
2. Olay oluşturma için abone günlüklerine göz atın. [Olay teslimini doğrulamak](pub-sub-events-webhook-local.md#verify-event-delivery) için adımları izleyin

    Örnek Çıktı:

    ```json
            Received Event:
            {
              "id": "d278f2aa-2558-41aa-816b-e6d8cc8fa140",
              "topic": "MicrosoftStorage",
              "subject": "/blobServices/default/containers/cont1/blobs/Team.jpg",
              "eventType": "Microsoft.Storage.BlobCreated",
              "eventTime": "2019-10-01T21:35:17.7219554Z",
              "dataVersion": "1.0",
              "metadataVersion": "1",
              "data": {
                "api": "PutBlob",
                "clientRequestId": "00000000-0000-0000-0000-000000000000",
                "requestId": "ef1c387b-4c3c-4ac0-8e04-ff73c859bfdc",
                "eTag": "0x8D746B740DA21FB",
                "url": "http://azureblobstorageoniotedge:11002/myaccount/cont1/Team.jpg",
                "contentType": "image/jpeg",
                "contentLength": 858129,
                "blobType": "BlockBlob"
              }
            }
    ```

### <a name="verify-blobdeleted-event-delivery"></a>BlobDeleted olay teslimini doğrula

1. Azure Depolama Gezgini'ni kullanarak yerel depolama deposundan lekeleri silin ve modül silme olaylarını otomatik olarak yayımlayacaktır. 
2. Silme olayı için abone günlüklerine göz atın. [Olay teslimini doğrulamak](pub-sub-events-webhook-local.md#verify-event-delivery) için adımları izleyin

    Örnek Çıktı:
    
    ```json
            Received Event:
            {
              "id": "ac669b6f-8b0a-41f3-a6be-812a3ce6ac6d",
              "topic": "MicrosoftStorage",
              "subject": "/blobServices/default/containers/cont1/blobs/Team.jpg",
              "eventType": "Microsoft.Storage.BlobDeleted",
              "eventTime": "2019-10-01T21:36:09.2562941Z",
              "dataVersion": "1.0",
              "metadataVersion": "1",
              "data": {
                "api": "DeleteBlob",
                "clientRequestId": "00000000-0000-0000-0000-000000000000",
                "requestId": "2996bbfb-c819-4d02-92b1-c468cc67d8c6",
                "eTag": "0x8D746B740DA21FB",
                "url": "http://azureblobstorageoniotedge:11002/myaccount/cont1/Team.jpg",
                "contentType": "image/jpeg",
                "contentLength": 858129,
                "blobType": "BlockBlob"
              }
            }
    ```

Tebrikler! Öğreticiyi tamamladınız. Aşağıdaki bölümlerde olay özellikleri hakkında ayrıntılı bilgi verilmiştir.

### <a name="event-properties"></a>Olay özellikleri

Desteklenen olay özelliklerinin listesi ve bunların türleri ve açıklamaları aşağıda verilmiştir. 

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | string | Olay kaynağına tam kaynak yolu. Bu alan yazılamaz. Event Grid bu değeri sağlar. |
| Konu | string | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| Eventtype | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın sağlayıcının UTC zamanına bağlı olarak oluşturulan süre. |
| id | string | Etkinlik için benzersiz tanımlayıcı. |
| veri | object | Blob depolama olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| API | string | Olayı tetikleyen işlem. Aşağıdaki değerlerden biri olabilir: <ul><li>BlobCreated - izin `PutBlob` verilen değerler şunlardır: ve`PutBlockList`</li><li>BlobDeleted - izin `DeleteBlob` `DeleteAfterUpload` verilen `AutoDelete`değerler ve . <p>DeleteAfterUpload `DeleteAfterUpload` istenilen özellik gerçek olarak ayarlandığı için blob otomatik olarak silindiğinde olay oluşturulur. </p><p>`AutoDelete`blob otomatik olarak silindiğinde olay oluşturulur çünkü deleteAfterMinutes istenen özellik değeri süresi doldu.</p></li></ul>|
| clientRequestId | string | depolama API işlemi için istemci tarafından sağlanan bir istek kimliği. Bu kimlik, günlüklerde "istemci-istek-id" alanını kullanarak Azure Depolama tanı günlükleri ile ilişkilendirmek için kullanılabilir ve "x-ms-client-request-id" üstbilgisini kullanarak istemci isteklerinde sağlanabilir. Ayrıntılar için [Günlük Biçimi'ne](/rest/api/storageservices/storage-analytics-log-format)bakın. |
| Requestıd | string | Depolama API işlemi için hizmet tarafından oluşturulan istek kimliği. Günlüklerde "istek-id-üstbilgi" alanını kullanarak Azure Depolama tanı günlükleri ile ilişkilendirmek için kullanılabilir ve 'x-ms-request-id' üstbilgisinde API çağrısını başlatmaktan döndürülür. [Bkz. Günlük Biçimi](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format). |
| Etag | string | İşlemleri koşullu olarak gerçekleştirmek için kullanabileceğiniz değer. |
| Contenttype | string | Blob için belirtilen içerik türü. |
| Contentlength | integer | Bayttaki lekenin boyutu. |
| blobType | string | Leke türü. Geçerli değerler "BlockBlob" veya "PageBlob" dır. |
| url | string | Blob'a giden yol. <br>İstemci Blob REST API kullanıyorsa, url bu yapıya sahiptir: * \<depolama-hesap adı\>.blob.core.windows.net/\<kapsayıcı\>/\<adı dosya adı.\>* <br>İstemci bir Veri Gölü Depolama REST API kullanıyorsa, url bu yapıya sahiptir: * \<depolama-hesap adı\>.dfs.core.windows.net/\<dosya-sistem adı\>/\<\>dosya adı.* |


## <a name="next-steps"></a>Sonraki adımlar

Blob Depolama belgelerinden aşağıdaki makalelere bakın:

- [Filtre Blob Depolama olayları](../../storage/blobs/storage-blob-event-overview.md#filtering-events)
- [Blob Depolama etkinliklerini tüketmek için önerilen uygulamalar](../../storage/blobs/storage-blob-event-overview.md#practices-for-consuming-events)

Bu eğitimde, bir Azure Blob Depolama alanında lekeler oluşturarak veya silerek olayları yayımladınız. Etkinlikleri buluta nasıl ileteceklerini öğrenmek için diğer öğreticilere bakın (Azure Etkinlik Hub'ı veya Azure IoT Hub'ı): 

- [Olayları Azure Event Grid’e iletme](forward-events-event-grid-cloud.md)
- [Olayları Azure IoT Hub’a iletme](forward-events-iothub.md)
