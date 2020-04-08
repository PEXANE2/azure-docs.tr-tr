---
title: Modüldeki blob depolama alanını cihazınıza dağıtma - Azure IoT Edge
description: Verileri kenarda depolamak için IoT Edge cihazınıza bir Azure Blob Depolama modülü dağıtın.
author: kgremban
ms.author: kgremban
ms.date: 3/10/2020
ms.topic: conceptual
ms.service: iot-edge
ms.reviewer: arduppal
ms.openlocfilehash: da163e902d06bd98ac47a24256cb809cb222173b
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804631"
---
# <a name="deploy-the-azure-blob-storage-on-iot-edge-module-to-your-device"></a>IoT Edge modülünde Azure Blob Depolama'yı cihazınıza dağıtma

Modülleri bir IoT Edge aygıtına dağıtmanın çeşitli yolları vardır ve bunların tümü IoT Edge modüllerindeki Azure Blob Depolamaiçin çalışır. En basit iki yöntem, Azure portalı veya Visual Studio Code şablonlarını kullanmak.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğinizde bir [IoT hub'ı.](../iot-hub/iot-hub-create-through-portal.md)
- IoT Edge çalışma zamanı yüklü bir [IoT Edge aygıtı.](how-to-register-device.md)
- [Visual Studio Code'dan](https://code.visualstudio.com/) dağıtım varsa Visual Studio Kodu ve [Azure IoT Araçları.](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)

## <a name="deploy-from-the-azure-portal"></a>Azure portalından dağıtma

Azure portalı, bir dağıtım bildirimi oluşturma ve dağıtımı bir IoT Edge aygıtına itme yoluyla size yol gösterir.

### <a name="select-your-device"></a>Cihazınızı seçin

1. [Azure portalında](https://portal.azure.com) oturum açın ve IoT hub'ınıza gidin.
1. Menüden **IoT Edge'i** seçin.
1. Aygıtlar listesinden hedef aygıtın kimliğine tıklayın.'
1. **Modülleri Ayarlama**'yı seçin.

### <a name="configure-a-deployment-manifest"></a>Dağıtım bildirimini yapılandırma

Dağıtım bildirimi, hangi modüllerin dağıtılabildiğini, modüller arasında verilerin nasıl aktığını ve modülün istenilen özelliklerini açıklayan bir JSON belgesidir. Azure portalında, dağıtım bildirimi oluşturmada size yol gösteren bir sihirbaz vardır. Bu sekmeler halinde düzenlenen üç adım vardır: **Modüller**, **Rotalar**ve **Gözden Geçirme + Oluştur**.

#### <a name="add-modules"></a>Modül ekle

1. Sayfanın **IoT Edge Modülleri** bölümünde, Açılan **Ekle'yi** tıklatın ve **IoT Edge Modülü Ekle** sayfasını görüntülemek için **IoT Edge Modülü'nü** seçin.

2. Modül **Ayarları** sekmesinde, modül için bir ad girin ve ardından konteyner görüntüsü URI'yi belirtin:

   Örnekler:
  
   - **IoT Edge Modül Adı**:`azureblobstorageoniotedge`
   - **Resim URI**:`mcr.microsoft.com/azure-blob-storage:latest`

   ![Modül İkiz Ayarları](./media/how-to-deploy-blob/addmodule-tab1.png)

   **Modül Ayarları,** **Kapsayıcı Oluşturma Seçenekleri**ve Modül **İkiz Ayarları** sekmelerinde bu yordamda açıklandığı gibi değerleri belirtmeden **Ekle'yi** seçmeyin.

   > [!IMPORTANT]
   > Azure IoT Edge, modüllere arama yaptığınızda büyük/küçük harf duyarlıdır ve Depolama SDK'sı da varsayılan olarak küçük harfe değer tanır. [Azure](how-to-deploy-modules-portal.md#deploy-modules-from-azure-marketplace) Marketi'ndeki modülün adı **AzureBlobStorageonIoTEdge**olmasına rağmen, adı küçük harfle değiştirmek, IoT Edge modülündeki Azure Blob Depolama'ya bağlantılarınızın kesintiye uğramamasını sağlamaya yardımcı olur.

3. Kapsayıcı **Oluşturma Seçenekleri** sekmesini açın.

   ![Modül İkiz Ayarları](./media/how-to-deploy-blob/addmodule-tab3.png)

   Depolama hesabı bilgilerini ve aygıtınızdaki depolama alanı için bir montaj sağlamak için aşağıdaki JSON'u kutuya kopyalayıp yapıştırın.
  
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

4. **Kapsayıcı Oluşturma Seçenekleri'ne** kopyaladığınız JSON'u aşağıdaki bilgilerle güncelleştirin:

   - Hatırlayabileceğiniz bir adla değiştirin. `<your storage account name>` Hesap adları 3 ila 24 karakter uzunluğunda, küçük harfler ve sayılarla olmalıdır. Yer yok.

   - 64 baytlık base64 tuşu ile değiştirin. `<your storage account key>` [GeneratePlus](https://generate.plus/en/base64)gibi araçlarla bir anahtar oluşturabilirsiniz. Bu kimlik bilgilerini diğer modüllerden blob depolamasına erişmek için kullanırsınız.

   - Konteyner `<storage mount>` işletim sisteminize göre değiştirin. Blob modülünün verilerini depoladığı IoT Edge aygıtınızdaki varolan bir dizinin adını [veya](https://docs.docker.com/storage/volumes/) mutlak yolunu sağlayın. Depolama yuvası, cihazınızda modülde ayarlanmış bir konuma sağladığınız bir konumu eşler.

     - Linux kapsayıcıları için biçim depolama ** \<yolunuz veya>:/blobroot'** dur. Örneğin:
         - [hacim montaj](https://docs.docker.com/storage/volumes/)kullanın :`my-volume:/blobroot`
         - [bind montaj](https://docs.docker.com/storage/bind-mounts/) `/srv/containerdata:/blobroot`kullanın : . [Kapsayıcı kullanıcısına dizin erişimi sağlamak için](how-to-store-data-blob.md#granting-directory-access-to-container-user-on-linux) aşağıdaki adımları izlediğinden emin olun
     - Windows kapsayıcıları için biçim depolama ** \<yolunuz veya>:C:/BlobRoot'** dur. Örneğin:
         - [hacim montaj](https://docs.docker.com/storage/volumes/) `my-volume:C:/BlobRoot`kullanın : .
         - [bind montaj](https://docs.docker.com/storage/bind-mounts/) `C:/ContainerData:C:/BlobRoot`kullanın : .
         - Yerel sürücünüzü kullanmak yerine, daha fazla bilgi için Kobİ ağ konumunuzu eşleyebilir, [yerel depolama alanınız olarak Kobİ paylaşımını kullanarak](how-to-store-data-blob.md#using-smb-share-as-your-local-storage) görebilirsiniz

     > [!IMPORTANT]
     > Depolama montaj değerinin ikinci yarısını değiştirmeyin, bu da IoT Edge modülündeki Blob Depolama'da belirli bir konuma işaret ediyor. Depolama montaj her zaman ile sona ermelidir **:/blobroot** Linux kapsayıcılar ve **:C:/BlobRoot** Windows kapsayıcılar için.

5. Modül **İkiz Ayarları** sekmesinde, aşağıdaki JSON'u kopyalayın ve kutuya yapıştırın.

   ![Modül İkiz Ayarları](./media/how-to-deploy-blob/addmodule-tab4.png)

   Yer tutucular tarafından belirtildiği gibi, her özelliği uygun bir değerle yapılandırın. IoT Edge simülatörü kullanıyorsanız, [deviceToCloudUploadProperties](how-to-store-data-blob.md#devicetoclouduploadproperties) ve [deviceAutoDeleteProperties](how-to-store-data-blob.md#deviceautodeleteproperties)tarafından açıklandığı gibi bu özellikler için ilgili ortam değişkenlerine değerleri ayarlayın.

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

   Cihazınızın yapılandırılması hakkında bilgi içinToCloudUploadProperties ve deviceAutoDeleteProperties modülünüz dağıtıldıktan sonra, Modül [İkiz'i Düzenle 'ye](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin)bakın. İstenilen özellikler hakkında daha fazla bilgi için [bkz.](module-composition.md#define-or-update-desired-properties)

6. **Ekle'yi**seçin.

7. **Sonraki'ni seçin: Rotalar** bölümüne devam edecek rotalar.

#### <a name="specify-routes"></a>Rotaları belirtin

Varsayılan yolları tutun ve gözden geçirme bölümüne devam etmek için **İleri: Gözden Geçir + oluştur'u** seçin.

#### <a name="review-deployment"></a>Dağıtımı gözden geçirme

İnceleme bölümü, önceki iki bölümdeki seçimlerinize göre oluşturulan JSON dağıtım bildirimini gösterir. Ayrıca eklemediğiniz ibareleri beyan edilen iki modül de vardır: **$edgeAgent** ve **$edgeHub.** Bu iki modül [IoT Edge çalışma süresini](iot-edge-runtime.md) oluşturan ve her dağıtımda varsayılan olarak gereklidir.

Dağıtım bilgilerinizi gözden geçirin ve ardından **Oluştur'u**seçin.

### <a name="verify-your-deployment"></a>Dağıtımınızı doğrulayın

Dağıtımı oluşturduktan sonra, IoT hub'ınızın **IoT Edge** sayfasına geri dönersiniz.

1. Ayrıntıları açmak için dağıtımla hedeflediğiniz IoT Edge aygıtını seçin.
1. Aygıt ayrıntılarında, blob depolama modülünün hem **dağıtımda belirtilen** hem de **aygıt tarafından bildirilen**olarak listelenmiş olduğunu doğrulayın.

Modülün cihazda başlatılması ve ardından IoT Hub'a rapor edilmesi birkaç dakika sürebilir. Güncelleştirilmiş bir durumu görmek için sayfayı yenileyin.

## <a name="deploy-from-visual-studio-code"></a>Visual Studio Kodundan Dağıtma

Azure IoT Edge, kenar çözümleri geliştirmenize yardımcı olmak için Visual Studio Code'da şablonlar sağlar. Blob depolama modülüne sahip yeni bir IoT Edge çözümü oluşturmak ve dağıtım bildirimini yapılandırmak için aşağıdaki adımları kullanın.

1. **Komut Paletini** **Görüntüle'yi** > seçin.

1. Komut paletinde **Azure IoT Edge: New IoT Edge solution** komutunu girin ve çalıştırın.

   ![Yeni IoT Edge Çözümlerini Çalıştırın](./media/how-to-develop-csharp-module/new-solution.png)

   Çözümünüzü oluşturmak için komut paletindeki yönergeleri izleyin.

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Çözüm dosyalarını oluşturmak için Visual Studio Code için geliştirme makinenizdeki konumu seçin. |
   | Çözüm adı sağlayın | Çözümünüz için açıklayıcı bir ad girin veya varsayılan **EdgeSolution'ı**kabul edin. |
   | Modül şablonunu seçin | Varolan Modülü Seçin **(Tam resim URL'sini girin)**. |
   | Modül adı sağlayın | **Azureblobstorageoniotedge**gibi modülünüz için çok küçük bir ad girin.<br/><br/>IoT Edge modülündeki Azure Blob Depolama alanı için küçük bir ad kullanmak önemlidir. IoT Edge modüllere atıfta bulunduğunda büyük/küçük harfduyarlıdır ve Depolama SDK varsayılan olarak küçük harfe karşı dır. |
   | Modül için Docker görüntüsü sağlayın | Görüntü URI sağlayın: **mcr.microsoft.com/azure-blob-storage:latest** |

   Visual Studio Code sağladığınız bilgileri alır, bir IoT Edge çözümü oluşturur ve sonra yeni bir pencereye yükler. Çözüm şablonu, blob depolama modülü görüntünüzü içeren bir dağıtım bildirimi şablonu oluşturur, ancak modülün oluşturma seçeneklerini yapılandırmanız gerekir.

1. Yeni çözüm çalışma alanınızda *deployment.template.json'u* açın ve **modüller** bölümünü bulun. Aşağıdaki yapılandırma değişikliklerini yapın:

   1. Bu dağıtım için gerekli olmadığı için **Simüle Edilen Sıcaklık Sensörü** modülünü silin.

   1. Aşağıdaki kodu kopyalayıp alana `createOptions` yapıştırın:

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

      ![Modül oluşturma seçeneklerini güncelleştirin - Visual Studio Code](./media/how-to-deploy-blob/create-options.png)

1. Hatırlayabileceğiniz bir adla değiştirin. `<your storage account name>` Hesap adları 3 ila 24 karakter uzunluğunda, küçük harfler ve sayılarla olmalıdır. Yer yok.

1. 64 baytlık base64 tuşu ile değiştirin. `<your storage account key>` [GeneratePlus](https://generate.plus/en/base64)gibi araçlarla bir anahtar oluşturabilirsiniz. Bu kimlik bilgilerini diğer modüllerden blob depolamasına erişmek için kullanırsınız.

1. Konteyner `<storage mount>` işletim sisteminize göre değiştirin. Blob modülünün verilerini depolamasını istediğiniz IoT Edge aygıtınızdaki bir [birimin](https://docs.docker.com/storage/volumes/) adını veya mutlak yolunu sağlayın. Depolama yuvası, cihazınızda modülde ayarlanmış bir konuma sağladığınız bir konumu eşler.  

     - Linux kapsayıcıları için biçim depolama ** \<yolunuz veya>:/blobroot'** dur. Örneğin:
         - [hacim montaj](https://docs.docker.com/storage/volumes/)kullanın :`my-volume:/blobroot`
         - [bind montaj](https://docs.docker.com/storage/bind-mounts/) `/srv/containerdata:/blobroot`kullanın : . [Kapsayıcı kullanıcısına dizin erişimi sağlamak için](how-to-store-data-blob.md#granting-directory-access-to-container-user-on-linux) aşağıdaki adımları izlediğinden emin olun
     - Windows kapsayıcıları için biçim depolama ** \<yolunuz veya>:C:/BlobRoot'** dur. Örneğin:
         - [hacim montaj](https://docs.docker.com/storage/volumes/) `my-volume:C:/BlobRoot`kullanın : .
         - [bind montaj](https://docs.docker.com/storage/bind-mounts/) `C:/ContainerData:C:/BlobRoot`kullanın : .
         - Yerel sürücünüzü kullanmak yerine, Kobİ ağ konumunuzu eşleyebilir, daha fazla bilgi için [yerel depolama alanınız olarak Kobİ paylaşımını kullanarak](how-to-store-data-blob.md#using-smb-share-as-your-local-storage) bkz.

     > [!IMPORTANT]
     > Depolama montaj değerinin ikinci yarısını değiştirmeyin, bu da IoT Edge modülündeki Blob Depolama'da belirli bir konuma işaret ediyor. Depolama montaj her zaman ile sona ermelidir **:/blobroot** Linux kapsayıcılar ve **:C:/BlobRoot** Windows kapsayıcılar için.

1. *Deployment.template.json* dosyasına aşağıdaki JSON'u ekleyerek [deviceToCloudUploadProperties](how-to-store-data-blob.md#devicetoclouduploadproperties) ve [deviceAutoDeleteProperties'i](how-to-store-data-blob.md#deviceautodeleteproperties) modülünüz için yapılandırın. Her özelliği uygun bir değerle yapılandırın ve dosyayı kaydedin. IoT Edge simülatörü kullanıyorsanız, bu özellikler için ilgili ortam değişkenleri değerlerini ayarlayın, [deviceToCloudUploadProperties](how-to-store-data-blob.md#devicetoclouduploadproperties) ve [deviceAutoDeleteProperties](how-to-store-data-blob.md#deviceautodeleteproperties) açıklama bölümünde bulabilirsiniz

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

   ![azureblobstorageoniotedge için istenilen özellikleri ayarlama - Visual Studio Code](./media/how-to-deploy-blob/devicetocloud-deviceautodelete.png)

   Cihazınızın yapılandırılması hakkında bilgi içinToCloudUploadProperties ve deviceAutoDeleteProperties modülünüz dağıtıldıktan sonra, Modül [İkiz'i Düzenle 'ye](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin)bakın. Kapsayıcı oluşturma seçenekleri, yeniden başlatma ilkesi ve istenen durum hakkında daha fazla bilgi için [EdgeAgent istenen özelliklere](module-edgeagent-edgehub.md#edgeagent-desired-properties)bakın.

1. *deployment.template.json* dosyasını kaydedin.

1. **Deployment.template.json'a** sağ tıklayın ve **IoT Edge dağıtım bildirimi ni oluştur'u**seçin.

1. Visual Studio *Code, deployment.template.json'da* sağladığınız bilgileri alır ve yeni bir dağıtım bildirimi dosyası oluşturmak için kullanır. Dağıtım bildirimi, çözüm çalışma alanınızdaki yeni bir **config** klasöründe oluşturulur. Bu dosyaya sahip olduktan sonra, [Visual Studio Code'tan Azure IoT Edge modüllerini dağıt'taki](how-to-deploy-modules-vscode.md) adımları izleyebilir veya [Azure CLI 2.0 ile Azure IoT Edge modüllerini dağıtabilirsiniz.](how-to-deploy-modules-cli.md)

## <a name="deploy-multiple-module-instances"></a>Birden çok modül örneğini dağıtma

Azure Blob Depolama'nın birden çok örneğini IoT Edge modülüne dağıtmak istiyorsanız, farklı `HostPort` bir depolama yolu sağlamanız ve modülün bağlandığı değeri değiştirmeniz gerekir. Blob depolama modülleri her zaman konteynerde 11002 bağlantı noktasını ortaya çıkarır, ancak ana bilgisayarda hangi bağlantı noktasına bağlı olduğunu bildirebilirsiniz.

Değeri değiştirmek için **Kapsayıcı Oluşturma Seçeneklerini** (Azure portalında) veya **createOptions** alanını (Visual Studio Code'daki `HostPort` *deployment.template.json* dosyasında) edin:

```json
"PortBindings":{
  "11002/tcp": [{"HostPort":"<port number>"}]
}
```

Ek blob depolama modüllerine bağlandığınızda, bitiş noktasını güncelleştirilmiş ana bilgisayar bağlantı noktasına çevirin.

## <a name="configure-proxy-support"></a>Proxy desteğini yapılandırma

Kuruluşunuz bir proxy sunucusu kullanıyorsa, edgeAgent ve edgeHub çalışma zamanı modülleri için proxy desteğini yapılandırmanız gerekir. Bu işlem iki görev içerir:

- Aygıttaki çalışma zamanı daemon'larını ve IoT Edge aracısını yapılandırın.
- Dağıtım bildirimi JSON dosyasındaki modüller için HTTPS_PROXY ortamı değişkenini ayarlayın.

Bu işlem, [bir proxy sunucusu üzerinden iletişim kurmak için bir IoT Edge aygıtı yapılandırmada](how-to-configure-proxy-support.md)açıklanmıştır.

Buna ek olarak, bir blob depolama modülü de HTTPS_PROXY ayarı manifest dağıtım dosyasında gerektirir. Dağıtım bildirimi dosyasını doğrudan dinleyebilir veya Azure portalını kullanabilirsiniz.

1. Azure portalındaki Iot hub'ınıza gidin ve sol bölme menüsünden **Iot Edge'i** seçin.

1. Yapılandırmak için modüle sahip aygıtı seçin.

1. **Modülleri Ayarlama**'yı seçin.

1. Sayfanın **IoT Edge Modülleri** bölümünde blob depolama modülünü seçin.

1. **IoT Edge Modülünü Güncelleştir** sayfasında **Çevre Değişkenleri** sekmesini seçin.

1. Ad `HTTPS_PROXY` ve **Name** **Değer**için proxy URL'niz için ekleyin.

      ![Ortam değişkenini HTTPS_PROXY ayarlama](./media/how-to-deploy-blob/https-proxy-config.png)

1. **Güncelleştir'i**tıklatın, ardından **Gözden Geçir + Oluştur**.

1. Proxy'nin dağıtım bildirimindeki modüle ekolduğunu unutmayın ve **Oluştur'u**seçin.

1. Aygıt ayrıntıları sayfasından modülü seçerek ayarı doğrulayın ve **IoT Edge Modülleri Ayrıntılar** sayfasının alt kısmında **Çevre Değişkenleri** sekmesini seçin.

      ![Ortam değişkenini HTTPS_PROXY ayarlama](./media/how-to-deploy-blob/verify-proxy-config.png)

## <a name="next-steps"></a>Sonraki adımlar

[IoT Edge'deki Azure Blob Depolama](how-to-store-data-blob.md)hakkında daha fazla bilgi edinin.

Dağıtım bildirimlerinin nasıl çalıştığı ve bunların nasıl oluşturulabileceği hakkında daha fazla bilgi için [bkz.](module-composition.md)
