---
title: Modül üzerinde blob Storage 'ı cihazınıza dağıtma-Azure IoT Edge
description: IOT Edge cihazınıza uçta veri depolamak için bir Azure Blob Depolama modülü dağıtın.
author: kgremban
ms.author: kgremban
ms.date: 12/13/2019
ms.topic: conceptual
ms.service: iot-edge
ms.reviewer: arduppal
ms.openlocfilehash: fe09fb47a75ff9d412ffab2daafaf241a43443b4
ms.sourcegitcommit: c32050b936e0ac9db136b05d4d696e92fefdf068
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75729616"
---
# <a name="deploy-the-azure-blob-storage-on-iot-edge-module-to-your-device"></a>IoT Edge modülünde Azure Blob Storage 'ı cihazınıza dağıtma

IoT Edge bir cihaza modül dağıtmanın birkaç yolu vardır ve bunların hepsi IoT Edge modüllerde Azure Blob depolama için çalışır. Visual Studio kod şablonları ve Azure Portalı'nı kullanmak için iki basit yöntemler şunlardır.

## <a name="prerequisites"></a>Ön koşullar

- Bir [IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md) Azure aboneliğinizdeki.
- Bir [IOT Edge cihazı](how-to-register-device.md) yüklü olan bir IOT Edge çalışma zamanı ile.
- [Visual Studio Code](https://code.visualstudio.com/) ve Visual Studio Code dağıtım yapıyorsanız [Azure IoT araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) .

## <a name="deploy-from-the-azure-portal"></a>Azure portal dağıtma

Azure portal, dağıtım bildirimi oluşturma ve dağıtımı bir IoT Edge cihazına gönderme işlemlerinde size rehberlik eder.

### <a name="select-your-device"></a>Cihazınızı seçin

1. Oturum [Azure portalında](https://portal.azure.com) ve IOT hub'ınıza gidin.
1. Seçin **IOT Edge** menüsünde.
1. Hedef cihazın cihazlar listesinden numarasını tıklayın.
1. **Modülleri Ayarlama**'yı seçin.

### <a name="configure-a-deployment-manifest"></a>Bir dağıtım bildirimi yapılandırma

Bir dağıtım bildirimi dağıtmak için modülleri ve modül ikizlerini istenen özellikleri arasında verilerin nasıl aktığını modüllerine açıklayan bir JSON belgesidir. Azure portal, JSON belgesini el ile oluşturmak yerine bir dağıtım bildirimi oluşturma konusunda size yol gösteren bir sihirbaza sahiptir. Sekmelerde düzenlenmiş üç adım vardır: **modüller**, **rotalar**ve **Gözden geçirme + oluştur**.

#### <a name="add-modules"></a>Modül Ekle

1. Sayfanın **IoT Edge modüller** **bölümünde açılan menü ekle '** ye tıklayın ve **IoT Edge modülü** ' nü seçerek **IoT Edge modülü Ekle** sayfasını görüntüleyin.

2. **Modül ayarları** sekmesinde, modül için bir ad sağlayın ve ardından KAPSAYıCı görüntüsü URI 'sini belirtin:

   Örnekler:
  
   - **IoT Edge modül adı**: `azureblobstorageoniotedge`
   - **Görüntü URI 'si**: `mcr.microsoft.com/azure-blob-storage:latest`

   ![Module Ikizi ayarları](./media/how-to-deploy-blob/addmodule-tab1.png)

   Bu yordamda açıklandığı gibi **Modül ayarlarında**, **kapsayıcı oluşturma seçeneklerinde**ve **module ikizi Settings** sekmelerinde değer belirtene kadar **Ekle** ' yi seçmeyin.

   > [!IMPORTANT]
   > Modüller için çağrılar yaptığınızda büyük/küçük harfe duyarlı Azure IoT Edge ve depolama SDK 'Sı varsayılan olarak küçük harfe duyarlıdır. [Azure Marketi](how-to-deploy-modules-portal.md#deploy-modules-from-azure-marketplace) 'ndeki modülün adı **AzureBlobStorageonIoTEdge**olsa da, adın küçük harfe değiştirilmesi IoT Edge modüldeki Azure Blob depolama ile olan bağlantılarınızın kesilmemesini sağlamaya yardımcı olur.

3. **Kapsayıcı oluşturma seçenekleri** sekmesinde, depolama hesabı bilgilerini ve cihazınızda depolama için bir bağlama sağlamak üzere JSON kodu sağlarsınız.

   ![Module Ikizi ayarları](./media/how-to-deploy-blob/addmodule-tab3.png)

   Sonraki adımda yer tutucu açıklamalarına başvurarak aşağıdaki JSON 'ı kopyalayıp kutuya yapıştırın.
  
   ```json
   {
     "Env":[
       "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
       "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
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

4. **Kapsayıcı oluşturma seçenekleri** IÇIN kopyaladığınız JSON 'ı aşağıdaki bilgilerle güncelleştirin:

   - `<your storage account name>`, anımsayabileceğiniz bir adla değiştirin. Hesap adları, küçük harf ve sayılarla 3 ile 24 karakter uzunluğunda olmalıdır. Boşluk yok.

   - `<your storage account key>`, 64 baytlık bir Base64 anahtarıyla değiştirin. Bir anahtar gibi araçlarla oluşturabilirsiniz [GeneratePlus](https://generate.plus/en/base64). Diğer modüllerden blob depolamaya erişmek için bu kimlik bilgilerini kullanacaksınız.

   - `<storage mount>`, kapsayıcı işletim sisteminize göre değiştirin. Adını sağlayın bir [birim](https://docs.docker.com/storage/volumes/) veya istediğiniz verileri depolamak için blob modülü IOT Edge Cihazınızda dizinine mutlak yolu. Depolama alanı, cihazınızdaki bir konumu modüldeki bir konum kümesine eşler.

     - Linux kapsayıcıları için, biçim *\<depolama yolu veya birim >:/blobroot*. Örneğin:
         - [birim bağlama](https://docs.docker.com/storage/volumes/)kullan: **My-Volume:/blobroot**
         - [bağlama bağlama](https://docs.docker.com/storage/bind-mounts/): **/SRV/containerdata:/blobroot**kullanın. [Kapsayıcı kullanıcısına dizin erişimi sağlamak](how-to-store-data-blob.md#granting-directory-access-to-container-user-on-linux) için adımları izlediğinizden emin olun
     - Windows kapsayıcıları için, biçim *\<depolama yolu veya birim >: C:/BlobRoot*. Örneğin:
         - [birim bağlama](https://docs.docker.com/storage/volumes/): **My-Volume: C:/blobroot**kullanın.
         - [bağlama bağlama](https://docs.docker.com/storage/bind-mounts/): **c:/Containerdata: c:/blobroot**kullanın.
         - Yerel sürücünüzü kullanmak yerine, SMB ağ konumunuzu eşleyebilirsiniz, daha fazla bilgi için bkz. [yerel depolama alanı olarak SMB paylaşımının kullanımı](how-to-store-data-blob.md#using-smb-share-as-your-local-storage)

     > [!IMPORTANT]
     > Depolama bağlama değerinin, modüldeki belirli bir konuma işaret eden ikinci yarısını değiştirmeyin. Depolama alanı, Linux kapsayıcıları için: **/blobroot** ve Windows kapsayıcıları için **C:/blobroot** ile her zaman bitmelidir.

5. **Modül Ikizi ayarları** sekmesinde, aşağıdaki JSON 'u kopyalayın ve kutuya yapıştırın.

   ![Module Ikizi ayarları](./media/how-to-deploy-blob/addmodule-tab4.png)

   Her özelliği, yer tutucularla gösterildiği gibi uygun bir değerle yapılandırın. IoT Edge simülatörü kullanıyorsanız, [Devicetoclouduploadproperties](how-to-store-data-blob.md#devicetoclouduploadproperties) ve [Deviceoto deleteproperties](how-to-store-data-blob.md#deviceautodeleteproperties)bölümünde açıklandığı gibi bu özellikler için değerleri ilgili ortam değişkenlerine ayarlayın.

   ```json
   {
     "deviceAutoDeleteProperties": {
       "deleteOn": <true, false>,
       "deleteAfterMinutes": <timeToLiveInMinutes>,
       "retainWhileUploading": <true,false>
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
       "deleteAfterUpload": <true,false>
     }
   }
   ```

   Modülünüzün dağıtılmasından sonra deviceToCloudUploadProperties ve Deviceoto Deleteproperties yapılandırma hakkında bilgi için bkz. [Ikizi Module düzenleme](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin). İstenen özellikler hakkında daha fazla bilgi için bkz. [istenen özellikleri tanımlama veya güncelleştirme](module-composition.md#define-or-update-desired-properties).

6. **Add (Ekle)** seçeneğini belirleyin.

7. **İleri ' yi seçin:** rotalar bölümüne devam etmek için yollar.

#### <a name="specify-routes"></a>Rota belirtme

Varsayılan yolları koruyun ve Ileri ' yi seçin: Gözden geçirme bölümüne devam etmek için, İnceleme **+ Oluştur** seçeneğini belirleyin.

#### <a name="review-deployment"></a>Dağıtım gözden geçirin

Oluşturulan gözden geçirme bölümü gösterir, JSON dağıtım bildirimi önceki iki bölümlerde seçimlerinize göre. Ayrıca, eklemediğiniz iki modül de vardır: **$edgeAgent** ve **$edgeHub**. Bu iki modül oluşturan [IOT Edge çalışma zamanı](iot-edge-runtime.md) ve her dağıtımda gerekli varsayılanlardır.

Dağıtım bilgilerinizi gözden geçirin ve ardından **Oluştur**' u seçin.

### <a name="verify-your-deployment"></a>Dağıtımınızı doğrulama

Dağıtımı oluşturduktan sonra IoT Hub 'ınızın **IoT Edge** sayfasına dönersiniz.

1. Bilgilerini açmak için dağıtıma hedeflenmiş IoT Edge cihazı seçin.
1. Cihaz ayrıntılarında, BLOB depolama modülünün hem **dağıtım** hem de **cihaz tarafından bildirilen**olarak listelendiğini doğrulayın.

Modülün cihazda başlatılması ve sonra IoT Hub geri bildirilmesi birkaç dakika sürebilir. Güncelleştirilmiş durumu görmek için sayfayı yenileyin.

## <a name="deploy-from-visual-studio-code"></a>Visual Studio Code dağıtma

Azure IOT Edge, uç çözümleri geliştirmenize yardımcı olması için Visual Studio code'da şablonları sağlar. Blob Storage modülü ile yeni bir IoT Edge çözümü oluşturmak ve dağıtım bildirimini yapılandırmak için aşağıdaki adımları kullanın.

1. Seçin **görünümü** > **komut paleti**.

1. Komut paletinde **Azure IoT Edge: New IoT Edge solution** komutunu girin ve çalıştırın.

   ![Yeni IoT Edge çözüm Çalıştır](./media/how-to-develop-csharp-module/new-solution.png)

   Çözümünüzü oluşturmak için komut paletindeki yönergeleri izleyin.

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Çözüm dosyalarını oluşturmak için Visual Studio Code geliştirme makinenizdeki konumunu seçin. |
   | Çözüm adı sağlayın | Çözümünüz için açıklayıcı bir ad girin veya varsayılan değerleri kabul **EdgeSolution**. |
   | Modül şablonunu seçin | **Var olan modülü seçin (tam görüntü URL 'Si girin)** . |
   | Modül adı sağlayın | Modülünüzün **azureblobstorageoniotedge**gibi bir tam küçük harf adı girin.<br/><br/>IoT Edge modülünde Azure Blob depolama için küçük harfli bir ad kullanılması önemlidir. IoT Edge modüllerle ilgili olarak büyük/küçük harfe duyarlıdır ve depolama SDK varsayılan olarak küçük harfe duyarlıdır. |
   | Modül için Docker görüntüsü sağlama | Görüntü URI 'sini sağlayın: **MCR.Microsoft.com/Azure-Blob-Storage:latest** |

   Visual Studio Code verdiğiniz bilgileri alır, bir IoT Edge çözümü oluşturur ve sonra yeni bir pencerede yükler. Çözüm şablonu, blob depolama modülü görüntüsünü içeren bir dağıtım bildirimi şablonu oluşturur, ancak yapılandırmanız gerekir modülün oluşturma seçenekleri.

1. Açık *deployment.template.json* yeni çözüm çalışma alanı ve Bul **modülleri** bölümü. Aşağıdaki yapılandırma değişikliklerini yapın:

   1. Bu dağıtım için gerekli olmadığından **SimulatedTemperatureSensor** modülünü silin.

   1. Aşağıdaki kodu kopyalayıp `createOptions` alanına yapıştırın:

      ```json
      "Env":[
       "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
       "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
      ],
      "HostConfig":{
        "Binds": ["<storage mount>"],
        "PortBindings":{
          "11002/tcp": [{"HostPort":"11002"}]
        }
      }
      ```

      ![Güncelleştirme modülü createOptions-Visual Studio Code](./media/how-to-deploy-blob/create-options.png)

1. `<your storage account name>`, anımsayabileceğiniz bir adla değiştirin. Hesap adları, küçük harf ve sayılarla 3 ile 24 karakter uzunluğunda olmalıdır. Boşluk yok.

1. `<your storage account key>`, 64 baytlık bir Base64 anahtarıyla değiştirin. Bir anahtar gibi araçlarla oluşturabilirsiniz [GeneratePlus](https://generate.plus/en/base64). Diğer modüllerden blob depolamaya erişmek için bu kimlik bilgilerini kullanacaksınız.

1. `<storage mount>`, kapsayıcı işletim sisteminize göre değiştirin. Adını sağlayın bir [birim](https://docs.docker.com/storage/volumes/) veya istediğiniz verileri depolamak için blob modülü IOT Edge Cihazınızda dizinine mutlak yolu. Depolama alanı, cihazınızdaki bir konumu modüldeki bir konum kümesine eşler.  

     - Linux kapsayıcıları için, biçim *\<depolama yolu veya birim >:/blobroot*. Örneğin:
         - [birim bağlama](https://docs.docker.com/storage/volumes/)kullan: **My-Volume:/blobroot**
         - [bağlama bağlama](https://docs.docker.com/storage/bind-mounts/): **/SRV/containerdata:/blobroot**kullanın. [Kapsayıcı kullanıcısına dizin erişimi sağlamak](how-to-store-data-blob.md#granting-directory-access-to-container-user-on-linux) için adımları izlediğinizden emin olun
     - Windows kapsayıcıları için, biçim *\<depolama yolu veya birim >: C:/BlobRoot*. Örneğin:
         - [birim bağlama](https://docs.docker.com/storage/volumes/): **My-Volume: C:/blobroot**kullanın.
         - [bağlama bağlama](https://docs.docker.com/storage/bind-mounts/): **c:/Containerdata: c:/blobroot**kullanın.
         - Yerel sürücünüzü kullanmak yerine, SMB ağ konumunuzu eşleyebilirsiniz, daha fazla bilgi için bkz. [yerel depolama alanı olarak SMB paylaşımının kullanımı](how-to-store-data-blob.md#using-smb-share-as-your-local-storage)

     > [!IMPORTANT]
     > Depolama bağlama değerinin, modüldeki belirli bir konuma işaret eden ikinci yarısını değiştirmeyin. Depolama alanı, Linux kapsayıcıları için: **/blobroot** ve Windows kapsayıcıları için **C:/blobroot** ile her zaman bitmelidir.

1. Aşağıdaki JSON 'ı *Deployment. Template. JSON* dosyasına ekleyerek modülünüzün [Devicetoclouduploadproperties](how-to-store-data-blob.md#devicetoclouduploadproperties) ve [deviceoto deleteproperties özelliklerini](how-to-store-data-blob.md#deviceautodeleteproperties) yapılandırın. Her bir özelliği uygun bir değerle yapılandırın ve dosyayı kaydedin. IoT Edge simülatörü kullanıyorsanız, bu özellikler için değerleri, [Devicetoclouduploadproperties](how-to-store-data-blob.md#devicetoclouduploadproperties) ve [deviceoto deleteproperties](how-to-store-data-blob.md#deviceautodeleteproperties) 'in Açıklama bölümünde bulabileceğiniz bu özellikler için ilgili ortam değişkenlerine ayarlayın.

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

IoT Edge modülünde Azure Blob Storage 'ın birden fazla örneğini dağıtmak istiyorsanız, farklı bir depolama yolu sağlamanız ve modülün bağlandığı `HostPort` değerini değiştirmeniz gerekir. Blob depolama modülleri, bağlantı noktasını 11002 kapsayıcıdaki her zaman kullanıma sunma, ancak konakta bağlı hangi bağlantı noktası bildirebilirsiniz.

`HostPort` değerini değiştirmek için **kapsayıcı oluşturma seçeneklerini** (Azure Portal) veya **createoptions** alanını (Visual Studio Code içindeki *Deployment. Template. JSON* dosyasında) düzenleyin:

```json
"PortBindings":{
  "11002/tcp": [{"HostPort":"<port number>"}]
}
```

Ek blob depolama modüllerini bağladığınızda, uç nokta güncel ana bilgisayar bağlantı noktasına işaret edecek şekilde değiştirin.

## <a name="next-steps"></a>Sonraki adımlar
[IoT Edge Azure Blob depolama](how-to-store-data-blob.md) hakkında daha fazla bilgi edinin

Nasıl iş dağıtım bildirimleri ve bunların nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [nasıl IOT Edge modülleri, yapılandırılmış, yeniden kaldırılabilir ve anlamak](module-composition.md).
