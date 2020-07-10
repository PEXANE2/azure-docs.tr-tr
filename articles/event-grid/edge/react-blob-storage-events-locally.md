---
title: BLOB depolama modülü olaylarına tepki verme-Azure Event Grid IoT Edge | Microsoft Docs
description: BLOB depolama modülü olaylarına tepki verme
author: arduppal
manager: brymat
ms.author: arduppal
ms.reviewer: spelluru
ms.date: 07/08/2020
ms.topic: article
ms.openlocfilehash: 9389e0aff04baa18cb216f2a7ab6da42eb7031f2
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86171440"
---
# <a name="tutorial-react-to-blob-storage-events-on-iot-edge-preview"></a>Öğretici: IoT Edge BLOB depolama olaylarına tepki verme (Önizleme)
Bu makalede, Azure Blob depolama 'yı IoT modülü 'nde dağıtma işlemi gösterilmektedir. Bu, blob oluşturma ve BLOB silme hakkında olay göndermek için Event Grid yayımcı görevi gören Event Grid.  

IoT Edge Azure Blob depolama 'ya genel bakış için, bkz. IoT Edge ve özellikleri [üzerinde Azure Blob depolama](../../iot-edge/how-to-store-data-blob.md) .

> [!WARNING]
> Event Grid ile IoT Edge tümleştirme üzerinde Azure Blob depolama, önizlemededir

Bu öğreticiyi tamamlayabilmeniz için şunlar gerekir:

* **Azure aboneliği** -henüz bir [hesabınız yoksa ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun. 
* **Azure IoT Hub ve IoT Edge cihazı** -henüz yoksa [Linux](../../iot-edge/quickstart-linux.md) veya [Windows cihazları](../../iot-edge/quickstart.md) için Hızlı Başlangıç bölümündeki adımları izleyin.

## <a name="deploy-event-grid-iot-edge-module"></a>Event Grid IoT Edge modülünü dağıt

IoT Edge bir cihaza modül dağıtmanın birkaç yolu vardır ve bunların hepsi IoT Edge Azure Event Grid için çalışır. Bu makalede Azure portal IoT Edge Event Grid dağıtma adımları açıklanmaktadır.

>[!NOTE]
> Bu öğreticide, Event Grid modülünü kalıcı olmadan dağıtacaksınız. Bu öğreticide oluşturduğunuz tüm konuların ve aboneliklerin, modülü yeniden dağıtırken silineceği anlamına gelir. Kalıcılığı ayarlama hakkında daha fazla bilgi için şu makalelere bakın: [Linux 'ta durumu kalıcı hale](persist-state-linux.md) getirin veya [Windows 'da durumu devam](persist-state-windows.md)ettir. Üretim iş yükleri için Event Grid modülünü Kalıcılık ile yüklemenizi öneririz.


### <a name="select-your-iot-edge-device"></a>IoT Edge cihazınızı seçin

1. [Azure portalında](https://portal.azure.com) oturum açın
1. IoT Hub gidin.
1. **Otomatik cihaz yönetimi** bölümündeki menüden **IoT Edge** ' yi seçin. 
1. Cihaz listesinden hedef cihazın KIMLIĞINE tıklayın
1. **Modülleri Ayarlama**'yı seçin. Sayfayı açık tutun. Sonraki bölümde bulunan adımlarla devam edersiniz.

### <a name="configure-a-deployment-manifest"></a>Dağıtım bildirimi yapılandırma

Dağıtım bildirimi, hangi modüllerin dağıtılacağını, modüller arasında verilerin nasıl akacağını ve modül TWINS 'in istenen özelliklerini tanımlayan bir JSON belgesidir. Azure portal, JSON belgesini el ile oluşturmak yerine bir dağıtım bildirimi oluşturma konusunda size yol gösteren bir sihirbaza sahiptir.  Üç adım vardır: **modüller ekleme**, **rotalar belirtme**ve **dağıtımı İnceleme**.

### <a name="add-modules"></a>Modül ekle

1. **Dağıtım modülleri** bölümünde **Ekle** ' yi seçin.
1. Açılan listedeki modül türlerinden **IoT Edge modül** ' ı seçin.
1. Kapsayıcının adını, görüntüsünü, kapsayıcı oluşturma seçeneklerini belirtin:

   * **Ad**: eventgridmodule
   * **Görüntü URI 'si**:`mcr.microsoft.com/azure-event-grid/iotedge:latest`
   * **Kapsayıcı oluşturma seçenekleri**:

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

 1. **Kaydet**’e tıklayın
 1. Azure Event Grid abone modülünü birlikte dağıtmadan önce eklemek için sonraki bölüme geçin.

    >[!IMPORTANT]
    > Bu öğreticide, istemci kimlik doğrulamasının devre dışı bırakılması için Event Grid modülünü dağıtmayı öğreneceksiniz. Üretim iş yükleri için, istemci kimlik doğrulaması etkinleştirilmiş olarak yalnızca HTTPs isteklerini ve aboneleri etkinleştirmenizi öneririz. Event Grid modülünü güvenli şekilde yapılandırma hakkında daha fazla bilgi için bkz. [güvenlik ve kimlik doğrulaması](security-authentication.md).
    

## <a name="deploy-event-grid-subscriber-iot-edge-module"></a>Event Grid abonesi IoT Edge modülünü dağıtma

Bu bölümde, olayların sunulabileceği bir olay işleyicisi olarak görev yapacak başka bir IoT modülünün nasıl dağıtılacağı gösterilmektedir.

### <a name="add-modules"></a>Modül ekle

1. **Dağıtım modülleri** bölümünde yeniden **Ekle** ' yi seçin. 
1. Açılan listedeki modül türlerinden **IoT Edge modül** ' ı seçin.
1. Kapsayıcının adını, görüntüsünü ve kapsayıcı oluşturma seçeneklerini belirtin:

   * **Ad**: abone
   * **Görüntü URI 'si**:`mcr.microsoft.com/azure-event-grid/iotedge-samplesubscriber:latest`
   * **Kapsayıcı oluşturma seçenekleri**: yok
1. **Kaydet**’e tıklayın
1. Azure Blob depolama modülünü eklemek için sonraki bölüme geçin

## <a name="deploy-azure-blob-storage-module"></a>Azure Blob depolama modülünü dağıtma

Bu bölümde, Event Grid yayımcı yayımlama blobu oluşturma ve silinen olaylar olarak görev yapacak Azure Blob depolama modülünün nasıl dağıtılacağı gösterilmektedir.

### <a name="add-modules"></a>Modül ekle

1. **Dağıtım modülleri** bölümünde **Ekle** ' yi seçin.
2. Açılan listedeki modül türlerinden **IoT Edge modül** ' ı seçin.
3. Kapsayıcının adını, görüntüsünü ve kapsayıcı oluşturma seçeneklerini belirtin:

   * **Ad**: azureblobstorageoniotedge
   * **Görüntü URI 'si**: MCR.Microsoft.com/Azure-Blob-Storage:latest
   * **Kapsayıcı oluşturma seçenekleri**:

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
   > - Blob Storage modülü, HTTPS ve HTTP kullanarak olayları yayımlayabilir. 
   > - EventGrid için istemci tabanlı kimlik doğrulamasını etkinleştirdiyseniz, EVENTGRID_ENDPOINT değerini https 'ye izin vermek için şu şekilde güncelleştirdiğinizden emin olun: `EVENTGRID_ENDPOINT=https://<event grid module name>:4438` .
   > - Ayrıca yukarıdaki JSON 'a başka bir ortam değişkeni de ekleyin `AllowUnknownCertificateAuthority=true` . HTTP üzerinden EventGrid ile görüşülürken, **Allowunknowncertificateauthority** depolama modülünün otomatik olarak Imzalanan eventgrid sunucu sertifikalarına güvenmesini sağlar.

4. Aşağıdaki bilgilerle kopyaladığınız JSON 'yi güncelleştirin:

   - `<your storage account name>`Hatırlayabileceğiniz bir adla değiştirin. Hesap adları, küçük harf ve sayılarla 3 ile 24 karakter uzunluğunda olmalıdır. Boşluk yok.

   - `<your storage account key>`64 baytlık bir Base64 anahtarıyla değiştirin. [Generateplus](https://generate.plus/en/base64?gp_base64_base[length]=64)gibi araçlarla bir anahtar oluşturabilirsiniz. Diğer modüllerden blob depolamaya erişmek için bu kimlik bilgilerini kullanacaksınız.

   - `<event grid module name>`Event Grid modülünüzün adıyla değiştirin.
   - Öğesini `<storage mount>` kapsayıcı işletim sisteminize göre değiştirin.
     - Linux kapsayıcıları için, **My-Volume:/blobroot**
     - Windows kapsayıcıları için,**My-Volume: C:/BlobRoot**

5. **Kaydet**’e tıklayın
6. Yönlendirmeler bölümüne devam etmek için **İleri** 'ye tıklayın

    > [!NOTE]
    > Sınır aygıtı olarak bir Azure VM kullanıyorsanız, bu öğreticide kullanılan ana bilgisayar bağlantı noktalarında gelen trafiğe izin vermek için bir gelen bağlantı noktası kuralı ekleyin: 4438, 5888, 8080 ve 11002. Kuralı ekleme hakkında yönergeler için bkz. [BIR VM 'ye bağlantı noktalarını açma](../../virtual-machines/windows/nsg-quickstart-portal.md).

### <a name="setup-routes"></a>Kurulum yolları

Varsayılan yolları koruyun ve gözden geçirme bölümüne devam etmek için **İleri** ' yi seçin.

### <a name="review-deployment"></a>Dağıtımı gözden geçir

1. İnceleme Bölümü, önceki bölümde yaptığınız seçimlere göre oluşturulan JSON dağıtım bildirimini gösterir. Aşağıdaki dört modülü gördüistediğinizi onaylayın: **$edgeAgent**, **$edgeHub**, **eventgridmodule**, **abone** ve **azureblobstorageoniotedge** tüm dağıtılan.
2. Dağıtım bilgilerinizi gözden geçirin ve ardından **Gönder**' i seçin.

## <a name="verify-your-deployment"></a>Dağıtımınızı doğrulama

1. Dağıtımı gönderdikten sonra IoT Hub 'ınızın IoT Edge sayfasına dönersiniz.
2. Bilgilerini açmak için dağıtıma hedeflenmiş **IoT Edge cihazı** seçin.
3. Cihaz ayrıntılarında, eventgridmodule, abone ve azureblobstorageoniotedge modüllerinin hem **dağıtım** hem de **cihaz tarafından raporlanarak**listelendiğini doğrulayın.

   Modülün cihazda başlatılması ve sonra IoT Hub geri bildirilmesi birkaç dakika sürebilir. Güncelleştirilmiş durumu görmek için sayfayı yenileyin.

## <a name="publish-blobcreated-and-blobdeleted-events"></a>BlobCreated ve BlobDeleted olaylarını Yayımla

1. Bu modül, **Microsoftstorage**konusunu otomatik olarak oluşturur. Var olduğunu doğrulayın
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
    > - HTTPS akışı için, istemci kimlik doğrulaması SAS anahtarı aracılığıyla etkinleştirildiyse, daha önce belirtilen SAS anahtarı üst bilgi olarak eklenmelidir. Bu nedenle, kıvrımlı istek şu şekilde olacaktır:`curl -k -H "Content-Type: application/json" -H "aeg-sas-key: <your SAS key>" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/MicrosoftStorage?api-version=2019-01-01-preview`
    > - HTTPS akışı için, istemci kimlik doğrulaması sertifika aracılığıyla etkinleştirildiyse, kıvrımlı istek şu şekilde olur:`curl -k -H "Content-Type: application/json" --cert <certificate file> --key <certificate private key file> -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/MicrosoftStorage?api-version=2019-01-01-preview`

2. Aboneler, bir konuya yayımlanan olaylara kaydolabilirler. Herhangi bir olay almak için, **Microsoftstorage** konusu için bir Event Grid aboneliği oluşturmanız gerekir.
    1. Aşağıdaki içerikle birlikte blobsubscription.jsoluşturun. Yük hakkında daha fazla bilgi için [API belgelerimize](api.md) bakın

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
       > **EndpointType** özelliği, abonenin bir **Web kancası**olduğunu belirtir.  **EndpointUrl** , abonenin olayları dinlediği URL 'yi belirtir. Bu URL, daha önce dağıttığınız Azure Işlev örneğine karşılık gelir.

    2. Konusu için bir abonelik oluşturmak üzere aşağıdaki komutu çalıştırın. HTTP durum kodunu görtığınızdan emin olun `200 OK` .

       ```sh
       curl -k -H "Content-Type: application/json" -X PUT -g -d @blobsubscription.json https://<your-edge-device-public-ip-here>:4438/topics/MicrosoftStorage/eventSubscriptions/sampleSubscription5?api-version=2019-01-01-preview
       ```

       > [!IMPORTANT]
       > - HTTPS akışı için, istemci kimlik doğrulaması SAS anahtarı aracılığıyla etkinleştirildiyse, daha önce belirtilen SAS anahtarı üst bilgi olarak eklenmelidir. Bu nedenle, kıvrımlı istek şu şekilde olacaktır:`curl -k -H "Content-Type: application/json" -H "aeg-sas-key: <your SAS key>" -X PUT -g -d @blobsubscription.json https://<your-edge-device-public-ip-here>:4438/topics/MicrosoftStorage/eventSubscriptions/sampleSubscription5?api-version=2019-01-01-preview` 
       > - HTTPS akışı için, istemci kimlik doğrulaması sertifika aracılığıyla etkinleştirildiyse, kıvrımlı istek şu şekilde olur:`curl -k -H "Content-Type: application/json" --cert <certificate file> --key <certificate private key file> -X PUT -g -d @blobsubscription.json https://<your-edge-device-public-ip-here>:4438/topics/MicrosoftStorage/eventSubscriptions/sampleSubscription5?api-version=2019-01-01-preview`

    3. Aboneliğin başarıyla oluşturulduğunu doğrulamak için şu komutu çalıştırın. 200 Tamam HTTP durum kodu döndürülmelidir.

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
       > - HTTPS akışı için, istemci kimlik doğrulaması SAS anahtarı aracılığıyla etkinleştirildiyse, daha önce belirtilen SAS anahtarı üst bilgi olarak eklenmelidir. Bu nedenle, kıvrımlı istek şu şekilde olacaktır:`curl -k -H "Content-Type: application/json" -H "aeg-sas-key: <your SAS key>" -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/MicrosoftStorage/eventSubscriptions/sampleSubscription5?api-version=2019-01-01-preview`
       > - HTTPS akışı için, istemci kimlik doğrulaması sertifika aracılığıyla etkinleştirildiyse, kıvrımlı istek şu şekilde olur:`curl -k -H "Content-Type: application/json" --cert <certificate file> --key <certificate private key file> -X GET -g https://<your-edge-device-public-ip-here>:4438/topics/MicrosoftStorage/eventSubscriptions/sampleSubscription5?api-version=2019-01-01-preview`

3. [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) indirin ve [yerel depolama birimine bağlayın](../../iot-edge/how-to-store-data-blob.md#connect-to-your-local-storage-with-azure-storage-explorer)

## <a name="verify-event-delivery"></a>Olay teslimini doğrulama

### <a name="verify-blobcreated-event-delivery"></a>BlobCreated olay teslimini doğrulama

1. Dosyaları blok Blobları olarak Azure Depolama Gezgini yerel depolamaya yükleyin ve modül oluşturma olaylarını otomatik olarak yayımlar. 
2. Oluşturma olayı için abone günlüklerine göz atın. [Olay teslimini doğrulamak](pub-sub-events-webhook-local.md#verify-event-delivery) için adımları izleyin

    Örnek çıktı:

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

1. Azure Depolama Gezgini kullanarak blob 'ları yerel depodan silin ve modül silme olaylarını otomatik olarak yayımlar. 
2. Silme olayı için abone günlüklerine göz atın. [Olay teslimini doğrulamak](pub-sub-events-webhook-local.md#verify-event-delivery) için adımları izleyin

    Örnek çıktı:
    
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

Tebrikler! Öğreticiyi tamamladınız. Aşağıdaki bölümler olay özellikleriyle ilgili ayrıntıları sağlar.

### <a name="event-properties"></a>Olay özellikleri

Desteklenen olay özelliklerinin listesi ve bunların türleri ve açıklamaları aşağıda verilmiştir. 

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | string | Olay kaynağının tam kaynak yolu. Bu alan yazılabilir değil. Event Grid bu değeri sağlar. |
| subject | string | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| eventType | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Etkinliğin UTC saatine göre oluşturulduğu zaman. |
| kimlik | string | Etkinliğin benzersiz tanımlayıcısı. |
| veriler | nesne | BLOB depolama olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| api | string | Olayı tetikleyen işlem. Aşağıdaki değerlerden biri olabilir: <ul><li>BlobCreated-izin verilen değerler: `PutBlob` ve`PutBlockList`</li><li>BlobDeleted-izin verilen değerler `DeleteBlob` , `DeleteAfterUpload` ve `AutoDelete` . <p>`DeleteAfterUpload`DeleteAfterUpload istenen özelliği true olarak ayarlandığından, blob otomatik olarak silindiğinde olay oluşturulur. </p><p>`AutoDelete`Deleteafutes istenen özellik değerinin geçerliliği aşıldığı için blob otomatik olarak silindiğinde olay oluşturulur.</p></li></ul>|
| Clientrequestıd 'ye sahip | string | depolama API 'SI işlemi için istemci tarafından sağlanmış bir istek KIMLIĞI. Bu KIMLIK, günlüklerdeki "istemci-istek-kimliği" alanı kullanılarak Azure depolama tanılama günlükleri ile ilişkilendirmek için kullanılabilir ve "x-MS-Client-Request-ID" üst bilgisi kullanılarak istemci isteklerinde sağlanabilirler. Ayrıntılar için bkz. [günlük biçimi](/rest/api/storageservices/storage-analytics-log-format). |
| No | string | Depolama API 'SI işlemi için hizmet tarafından oluşturulan istek KIMLIĞI. , Günlüklerdeki "istek-kimliği-üst bilgi" alanı kullanılarak Azure depolama tanılama günlükleri ile ilişkilendirmek için kullanılabilir ve ' x-MS-Request-id ' üst bilgisinde API çağrısını başlatma işleminden döndürülür. [Günlük biçimine](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format)bakın. |
| Özelliği | string | İşlemleri koşullu olarak gerçekleştirmek için kullanabileceğiniz değer. |
| contentType | string | Blob için belirtilen içerik türü. |
| contentLength | integer | Blobun bayt cinsinden boyutu. |
| blobType | string | Blob türü. Geçerli değerler "BlockBlob" ya da "PageBlob". |
| url | string | Blobun yolu. <br>İstemci bir blob REST API kullanıyorsa, URL bu yapıya sahiptir: * \<storage-account-name\> . blob.Core.Windows.net/ \<container-name\> / \<file-name\> *. <br>İstemci bir Data Lake Storage REST API kullanıyorsa, URL bu yapıya sahiptir: * \<storage-account-name\> . DFS.Core.Windows.net/ \<file-system-name\> / \<file-name\> *. |


## <a name="next-steps"></a>Sonraki adımlar

BLOB depolama belgelerindeki aşağıdaki makalelere bakın:

- [Blob Depolama olaylarını filtrele](../../storage/blobs/storage-blob-event-overview.md#filtering-events)
- [Blob Depolama olaylarını tüketen önerilen uygulamalar](../../storage/blobs/storage-blob-event-overview.md#practices-for-consuming-events)

Bu öğreticide, bir Azure Blob depolamada blob oluşturarak veya silerek olayları yayımladınız. Olayları buluta iletme hakkında bilgi edinmek için bkz. diğer öğreticiler (Azure Olay Hub 'ı veya Azure IoT Hub): 

- [Olayları Azure Event Grid’e iletme](forward-events-event-grid-cloud.md)
- [Olayları Azure IoT Hub’a iletme](forward-events-iothub.md)
