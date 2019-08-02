---
title: Azure Blob Storage modülünü cihazlara dağıtma-Azure IoT Edge | Microsoft Docs
description: IOT Edge cihazınıza uçta veri depolamak için bir Azure Blob Depolama modülü dağıtın.
author: arduppal
ms.author: arduppal
ms.date: 06/19/2019
ms.topic: article
ms.service: iot-edge
ms.custom: seodec18
ms.reviewer: arduppal
manager: mchad
ms.openlocfilehash: 4511510dec6f488e1a6ea9a6842b771d2a298fec
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68640664"
---
# <a name="deploy-the-azure-blob-storage-on-iot-edge-module-to-your-device"></a>IoT Edge modülünde Azure Blob Storage 'ı cihazınıza dağıtma

IoT Edge bir cihaza modül dağıtmanın birkaç yolu vardır ve bunların hepsi IoT Edge modüllerde Azure Blob depolama için çalışır. Visual Studio kod şablonları ve Azure Portalı'nı kullanmak için iki basit yöntemler şunlardır.

> [!NOTE]
> IOT Edge üzerinde Azure Blob Depolama, içinde [genel Önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

- Bir [IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md) Azure aboneliğinizdeki.
- Bir [IOT Edge cihazı](how-to-register-device-portal.md) yüklü olan bir IOT Edge çalışma zamanı ile.
- [Visual Studio Code](https://code.visualstudio.com/) ve Visual Studio Code dağıtım yapıyorsanız [Azure IoT araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) .

## <a name="deploy-from-the-azure-portal"></a>Azure portal dağıtma

Azure portal, dağıtım bildirimi oluşturma ve dağıtımı bir IoT Edge cihazına gönderme işlemlerinde size rehberlik eder.

### <a name="select-your-device"></a>Cihazınızı seçin

1. Oturum [Azure portalında](https://portal.azure.com) ve IOT hub'ınıza gidin.
1. Seçin **IOT Edge** menüsünde.
1. Hedef cihazın cihazlar listesinden numarasını tıklayın.
1. **Modülleri Ayarlama**'yı seçin.

### <a name="configure-a-deployment-manifest"></a>Bir dağıtım bildirimi yapılandırma

Bir dağıtım bildirimi dağıtmak için modülleri ve modül ikizlerini istenen özellikleri arasında verilerin nasıl aktığını modüllerine açıklayan bir JSON belgesidir. Azure portal, JSON belgesini el ile oluşturmak yerine bir dağıtım bildirimi oluşturma konusunda size yol gösteren bir sihirbaza sahiptir. Üç adım vardır: **Modüller ekleyin**, **rotalar belirtin**ve **dağıtımı gözden geçirin**.

#### <a name="add-modules"></a>Modül Ekle

1. İçinde **dağıtım modülleri** sayfasında bölümünü **Ekle**.

1. Açılan listedeki modül türlerinden **IoT Edge modül**' ı seçin.

1. Modül için bir ad girin ve ardından kapsayıcı görüntüsünü belirtin:

   - **Ad** -azureblobstorageoniotedge
   - **Görüntü URI 'si** -MCR.Microsoft.com/Azure-Blob-Storage:latest

   > [!IMPORTANT]
   > Modüller için çağrılar yaptığınızda büyük/küçük harfe duyarlı Azure IoT Edge ve depolama SDK 'Sı varsayılan olarak küçük harfe duyarlıdır. [Azure Marketi](how-to-deploy-modules-portal.md#deploy-modules-from-azure-marketplace) 'ndeki modülün adı **AzureBlobStorageonIoTEdge**olsa da, adın küçük harfe değiştirilmesi IoT Edge modüldeki Azure Blob depolama ile olan bağlantılarınızın kesilmemesini sağlamaya yardımcı olur.

1. Varsayılan **kapsayıcı oluşturma seçenekleri** değerleri, kapsayıcınıza ait bağlantı noktası bağlamalarını tanımlar, ancak depolama hesabı bilgilerinizi ve cihazınızda depolama dizini için bir bağlama eklemeniz gerekir. Portalda varsayılan JSON değerini aşağıdaki JSON ile değiştirin:

   ```json
   {
     "Env":[
       "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
       "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
     ],
     "HostConfig":{
       "Binds":[
           "<storage directory bind>"
       ],
     "PortBindings":{
       "11002/tcp":[{"HostPort":"11002"}]
       }
     }
   }
   ```

1. Aşağıdaki bilgilerle kopyaladığınız JSON 'yi güncelleştirin:

   - Hatırlayabileceğiniz bir adla değiştirin `<your storage account name>` . Hesap adları, küçük harf ve sayılarla 3 ile 24 karakter uzunluğunda olmalıdır. Boşluk yok.

   - 64 `<your storage account key>` baytlık bir Base64 anahtarıyla değiştirin. Bir anahtar gibi araçlarla oluşturabilirsiniz [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). Diğer modüllerden blob depolamaya erişmek için bu kimlik bilgilerini kullanacaksınız.

   - Öğesini `<storage directory bind>` kapsayıcı işletim sisteminize göre değiştirin. Adını sağlayın bir [birim](https://docs.docker.com/storage/volumes/) veya istediğiniz verileri depolamak için blob modülü IOT Edge Cihazınızda dizinine mutlak yolu. Depolama Dizin bağlaması, cihazınızdaki bir konumu modüldeki bir küme konumuna eşler.

     - Linux kapsayıcıları için, biçim  *\<depolama yolu >:/blobroot*. Örneğin, **/SRV/containerdata:/blobroot** veya **My-Volume:/blobroot**.
     - Windows kapsayıcıları için, biçim  *\<>: C:/blobroot depolama yoludur*. Örneğin, **c:/containerdata: c:/blobroot** veya **My-Volume: c:/blobroot**. Yerel sürücünüzü kullanmak yerine, SMB ağ konumunuzu eşleyebilirsiniz, daha fazla bilgi için bkz. [yerel depolama alanı olarak SMB paylaşımının kullanımı](how-to-store-data-blob.md#using-smb-share-as-your-local-storage)

     > [!IMPORTANT]
     > Depolama dizini bağlama değerinin, modüldeki belirli bir konuma işaret eden ikinci yarısını değiştirmeyin. Depolama dizini bağlaması her zaman şu şekilde bitmelidir: Linux kapsayıcıları için **/blobroot** ve: Windows kapsayıcıları için **C:/blobroot** .

1. Modülünüzün [Devicetoclouduploadproperties](how-to-store-data-blob.md#devicetoclouduploadproperties) ve [deviceoto deleteproperties](how-to-store-data-blob.md#deviceautodeleteproperties) özelliklerini ayarlayarak aşağıdaki JSON 'yi kopyalayıp, **set Module ikizi 'in istenen özellikler** kutusuna yapıştırın. Her bir özelliği uygun bir değerle yapılandırın, kaydedin ve dağıtıma devam edin.

   ```json
   {
     "properties.desired": {
       "deviceAutoDeleteProperties": {
         "deleteOn": <true, false>,
         "deleteAfterMinutes": <timeToLiveInMinutes>,
         "retainWhileUploading":<true,false>
       },
       "deviceToCloudUploadProperties": {
         "uploadOn": <true, false>,
         "uploadOrder": "<NewestFirst, OldestFirst>",
         "cloudStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>; EndpointSuffix=<your end point suffix>",
         "storageContainersForUpload": {
           "<source container name1>": {
             "target": "<target container name1>"
           }
         },
         "deleteAfterUpload":<true,false>
       }
     }
   }

      ```

   ![kapsayıcı oluşturma seçeneklerini ayarlama, Deviceoto Deleteproperties ve deviceToCloudUploadProperties özellikleri](./media/how-to-deploy-blob/iotedge-custom-module.png)

   Modülünüzün dağıtılmasından sonra deviceToCloudUploadProperties ve Deviceoto Deleteproperties yapılandırma hakkında bilgi için bkz. [Ikizi Module düzenleme](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin). İstenen özellikler hakkında daha fazla bilgi için bkz. [istenen özellikleri tanımlama veya güncelleştirme](module-composition.md#define-or-update-desired-properties).

1. **Kaydet**’i seçin.

1. Seçin **sonraki** yollar bölüme geçmek için.

#### <a name="specify-routes"></a>Rota belirtme

Varsayılan yolları koruyun ve gözden geçirme bölümüne devam etmek için **İleri** ' yi seçin.

#### <a name="review-deployment"></a>Dağıtım gözden geçirin

Oluşturulan gözden geçirme bölümü gösterir, JSON dağıtım bildirimi önceki iki bölümlerde seçimlerinize göre. Ayrıca, eklemediğiniz iki modül de vardır: **$edgeAgent** ve **$edgeHub**. Bu iki modül oluşturan [IOT Edge çalışma zamanı](iot-edge-runtime.md) ve her dağıtımda gerekli varsayılanlardır.

Dağıtım bilgilerinizi gözden geçirin ve ardından **Gönder**.

### <a name="verify-your-deployment"></a>Dağıtımınızı doğrulama

Dağıtımı gönderdikten sonra IoT Hub 'ınızın **IoT Edge** sayfasına dönersiniz.

1. Bilgilerini açmak için dağıtıma hedeflenmiş IoT Edge cihazı seçin.
1. Cihaz ayrıntılarında, BLOB depolama modülünün hem **dağıtım** hem de **cihaz tarafından bildirilen**olarak listelendiğini doğrulayın.

Modülün cihazda başlatılması ve sonra IoT Hub geri bildirilmesi birkaç dakika sürebilir. Güncelleştirilmiş durumu görmek için sayfayı yenileyin.

## <a name="deploy-from-visual-studio-code"></a>Visual Studio Code dağıtma

Azure IOT Edge, uç çözümleri geliştirmenize yardımcı olması için Visual Studio code'da şablonları sağlar. Blob Storage modülü ile yeni bir IoT Edge çözümü oluşturmak ve dağıtım bildirimini yapılandırmak için aşağıdaki adımları kullanın.

1. Seçin **görünümü** > **komut paleti**.

1. Komut paletinde, komutunu **girin ve Azure IoT Edge çalıştırın: Yeni IoT Edge çözümü**.

   ![Yeni IoT Edge çözüm Çalıştır](./media/how-to-develop-csharp-module/new-solution.png)

   Çözümünüzü oluşturmak için komut paletindeki yönergeleri izleyin.

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Çözüm dosyalarını oluşturmak için Visual Studio Code geliştirme makinenizdeki konumunu seçin. |
   | Çözüm adı sağlayın | Çözümünüz için açıklayıcı bir ad girin veya varsayılan değerleri kabul **EdgeSolution**. |
   | Modül şablonunu seçin | **Var olan modülü seçin (tam görüntü URL 'Si girin)** . |
   | Modül adı sağlayın | Modülünüzün **azureblobstorageoniotedge**gibi bir tam küçük harf adı girin.<br /><br />IoT Edge modülünde Azure Blob depolama için küçük harfli bir ad kullanılması önemlidir. IoT Edge modüllerle ilgili olarak büyük/küçük harfe duyarlıdır ve depolama SDK varsayılan olarak küçük harfe duyarlıdır. |
   | Modül için Docker görüntüsü sağlama | Görüntü URI 'sini sağlayın: **MCR.Microsoft.com/Azure-Blob-Storage:latest** |

   Visual Studio Code verdiğiniz bilgileri alır, bir IoT Edge çözümü oluşturur ve sonra yeni bir pencerede yükler. Çözüm şablonu, blob depolama modülü görüntüsünü içeren bir dağıtım bildirimi şablonu oluşturur, ancak yapılandırmanız gerekir modülün oluşturma seçenekleri.

1. Açık *deployment.template.json* yeni çözüm çalışma alanı ve Bul **modülleri** bölümü. Aşağıdaki yapılandırma değişikliklerini yapın:

   1. Silme **tempSensor** haliyle modülü, bu dağıtım için gerekli değildir.

   1. Aşağıdaki kodu `createOptions` kopyalayıp alanına yapıştırın:

      ```json
      "Env":[
       "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
       "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
      ],
      "HostConfig":{
        "Binds": ["<storage directory bind>"],
        "PortBindings":{
          "11002/tcp": [{"HostPort":"11002"}]
        }
      }
      ```

      ![Güncelleştirme modülü createOptions-Visual Studio Code](./media/how-to-deploy-blob/create-options.png)

1. Hatırlayabileceğiniz bir adla değiştirin `<your storage account name>` . Hesap adları, küçük harf ve sayılarla 3 ile 24 karakter uzunluğunda olmalıdır. Boşluk yok.

1. 64 `<your storage account key>` baytlık bir Base64 anahtarıyla değiştirin. Bir anahtar gibi araçlarla oluşturabilirsiniz [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). Diğer modüllerden blob depolamaya erişmek için bu kimlik bilgilerini kullanacaksınız.

1. Öğesini `<storage directory bind>` kapsayıcı işletim sisteminize göre değiştirin. Adını sağlayın bir [birim](https://docs.docker.com/storage/volumes/) veya istediğiniz verileri depolamak için blob modülü IOT Edge Cihazınızda dizinine mutlak yolu. Depolama Dizin bağlaması, cihazınızdaki bir konumu modüldeki bir küme konumuna eşler.  

      - Linux kapsayıcıları için, biçim  *\<depolama yolu >:/blobroot*. Örneğin, **/SRV/containerdata:/blobroot** veya **My-Volume:/blobroot**.
      - Windows kapsayıcıları için, biçim  *\<>: C:/blobroot depolama yoludur*. Örneğin, **c:/containerdata: c:/blobroot** veya **My-Volume: c:/blobroot**.  Yerel sürücünüzü kullanmak yerine, SMB ağ konumunuzu eşleyebilirsiniz, daha fazla bilgi için bkz. [yerel depolama alanı olarak SMB paylaşımının kullanımı](how-to-store-data-blob.md#using-smb-share-as-your-local-storage)

      > [!IMPORTANT]
      > Depolama dizini bağlama değerinin, modüldeki belirli bir konuma işaret eden ikinci yarısını değiştirmeyin. Depolama dizini bağlaması her zaman şu şekilde bitmelidir: Linux kapsayıcıları için **/blobroot** ve: Windows kapsayıcıları için **C:/blobroot** .

1. Aşağıdaki JSON 'ı *Deployment. Template. JSON* dosyasına ekleyerek modülünüzün [Devicetoclouduploadproperties](how-to-store-data-blob.md#devicetoclouduploadproperties) ve [deviceoto deleteproperties özelliklerini](how-to-store-data-blob.md#deviceautodeleteproperties) yapılandırın. Her bir özelliği uygun bir değerle yapılandırın ve dosyayı kaydedin.

   ```json
   "<your azureblobstorageoniotedge module name>":{
     "properties.desired": {
       "deviceAutoDeleteProperties": {
         "deleteOn": <true, false>,
         "deleteAfterMinutes": <timeToLiveInMinutes>,
         "retainWhileUploading": <true, false>
       },
       "deviceToCloudUploadProperties": {
         "uploadOn": <true, false>,
         "uploadOrder": "<NewestFirst, OldestFirst>",
         "cloudStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>",
         "storageContainersForUpload": {
           "<source container name1>": {
             "target": "<target container name1>"
           }
         },
         "deleteAfterUpload": <true, false>
       }
     }
   }
   ```

   ![azureblobstorageoniotedge-Visual Studio Code için istenen özellikleri ayarlama](./media/how-to-deploy-blob/devicetocloud-deviceautodelete.png)

   Modülünüzün dağıtılmasından sonra deviceToCloudUploadProperties ve Deviceoto Deleteproperties yapılandırma hakkında bilgi için bkz. [Ikizi Module düzenleme](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin). Kapsayıcı oluşturma seçenekleri, yeniden başlatma ilkesi ve istenen durum hakkında daha fazla bilgi için bkz. [Edgeagent istenen özellikleri](module-edgeagent-edgehub.md#edgeagent-desired-properties).

1. *deployment.template.json* dosyasını kaydedin.

1. Sağ **deployment.template.json** seçip **oluşturmak IOT Edge dağıtım bildirimi**.

1. Visual Studio Code, *Deployment. Template. JSON* içinde verdiğiniz bilgileri alır ve yeni bir dağıtım bildirim dosyası oluşturmak için kullanır. Yeni bir dağıtım bildirimi oluşturulan **config** çözüm çalışma alanınızda bir klasör. Bu dosyayı açtıktan sonra adımları izleyebilirsiniz [Visual Studio Code için Azure IOT Edge'e dağıtma modüllerden](how-to-deploy-modules-vscode.md) veya [Azure CLI 2.0 ile Azure IOT Edge'e dağıtma modülleri](how-to-deploy-modules-cli.md).

## <a name="deploy-multiple-module-instances"></a>Birden çok modül örneği dağıtma

IoT Edge modülünde Azure Blob Storage 'ın birden fazla örneğini dağıtmak istiyorsanız, farklı bir depolama yolu sağlamanız ve modülün bağlandığı `HostPort` değeri değiştirmeniz gerekir. Blob depolama modülleri, bağlantı noktasını 11002 kapsayıcıdaki her zaman kullanıma sunma, ancak konakta bağlı hangi bağlantı noktası bildirebilirsiniz.

Değeri değiştirmek için **kapsayıcı oluşturma seçeneklerini** (Azure Portal) veya **createOptions** alanını ( Visual Studio Code içindeki Deployment. Template. json dosyasında) düzenleyin: `HostPort`

```json
"PortBindings":{
  "11002/tcp": [{"HostPort":"<port number>"}]
}
```

Ek blob depolama modüllerini bağladığınızda, uç nokta güncel ana bilgisayar bağlantı noktasına işaret edecek şekilde değiştirin.

## <a name="next-steps"></a>Sonraki adımlar
[IoT Edge Azure Blob depolama](how-to-store-data-blob.md) hakkında daha fazla bilgi edinin

[IoT Edge blogundaki Azure Blob depolamada](https://aka.ms/abs-iot-blogpost) en son güncelleştirmeler ve duyuru ile güncel kalın

Nasıl iş dağıtım bildirimleri ve bunların nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [nasıl IOT Edge modülleri, yapılandırılmış, yeniden kaldırılabilir ve anlamak](module-composition.md).
