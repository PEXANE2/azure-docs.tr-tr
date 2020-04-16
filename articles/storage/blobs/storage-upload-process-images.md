---
title: Azure Depolama ile buluta görüntü verileri yükleme | Microsoft Docs
description: Uygulama verilerini depolamak için Azure Blob depolama sını bir web uygulamasıyla kullanma
author: mhopkins-msft
ms.service: storage
ms.subservice: blobs
ms.topic: tutorial
ms.date: 03/06/2020
ms.author: mhopkins
ms.reviewer: dineshm
ms.openlocfilehash: e2ee959fb6fc7e8454919c71cfa20e2bb9055dfb
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393857"
---
# <a name="tutorial-upload-image-data-in-the-cloud-with-azure-storage"></a>Öğretici: Azure Depolama ile görüntü verilerini buluta yükleyin

Bu öğretici, bir dizinin birinci bölümüdür. Bu eğitimde, görüntüleri bir depolama hesabına yüklemek için Azure Blob depolama istemcikitaplığını kullanan bir web uygulamasının nasıl dağıtılanacağını öğreneceksiniz. İşi nizi bitirdiğinde, Azure depolama sundaki görüntüleri depolayan ve görüntüleyen bir web uygulamanız olur.

# <a name="net-v12-sdk"></a>[\.NET v12 SDK](#tab/dotnet)
![.NET'te görüntü rezer Uygulaması](media/storage-upload-process-images/figure2.png)

# <a name="nodejs-v10-sdk"></a>[Düğüm.js v10 SDK](#tab/nodejsv10)
![Düğüm.js V10 görüntü resizer uygulaması](media/storage-upload-process-images/upload-app-nodejs-thumb.png)

---

Serinin birinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Depolama hesabı oluşturma
> * Kapsayıcı oluşturma ve izinleri ayarlama
> * Erişim anahtarı alma
> * Azure'a bir web uygulaması dağıtma
> * Uygulama ayarlarını yapılandırma
> * Web uygulamasıyla etkileşim

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için bir Azure aboneliğinizin olması gerekir. Başlamadan önce ücretsiz bir [hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemek ve kullanmak için bu öğretici, Azure CLI sürüm 2.0.4 veya daha sonraçalıştırılmasını gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.  

Aşağıdaki örnek `myResourceGroup` adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location southeastasia
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Örnek, görüntüleri Azure depolama hesabındaki bir blob kapsayıcısına yükler. Depolama hesabı, Azure Storage veri nesnelerinizi depolamak ve bunlara erişmek için benzersiz ad alanı sağlar. Oluşturduğunuz kaynak grubunda [az storage account create](/cli/azure/storage/account) komutunu kullanarak bir depolama hesabı oluşturun.

> [!IMPORTANT]
> Öğreticinin 2. Etkinlik Ağıtını destekleyen bir Azure bölgesinde depolama hesabınızı oluşturduğunuzdan emin olun. Desteklenen bölgelerin listesi için [bölgeye göre Azure ürünlerine](https://azure.microsoft.com/global-infrastructure/services/?products=event-grid&regions=all)bakın.

Aşağıdaki komutta, `<blob_storage_account>` yer tutucuyu gördüğünüz Blob depolama hesabı için kendi genel olarak benzersiz adınızı değiştirin.

```azurecli-interactive
blobStorageAccount="<blob_storage_account>"

az storage account create --name $blobStorageAccount --location southeastasia \
  --resource-group myResourceGroup --sku Standard_LRS --kind StorageV2 --access-tier hot
```

## <a name="create-blob-storage-containers"></a>Blob depolama kapları oluşturma

Uygulama, Blob depolama hesabında iki kapsayıcı kullanır. Kapsayıcılar klasörlere benzer ve lekeleri depolar. *Images* kapsayıcısı, uygulamanın tam çözünürlüklü görüntüleri yüklediği yerdir. Serinin sonraki bölümlerinde bir Azure işlev uygulaması, yeniden boyutlandırılan küçük resimleri *thumbnails* kapsayıcısına yükler.

[az storage account keys list](/cli/azure/storage/account/keys) komutunu kullanarak depolama hesabı anahtarını alın. Ardından, [az depolama kapsayıcısı oluşturma](/cli/azure/storage/container) komutu ile iki kapsayıcı oluşturmak için bu anahtarı kullanın.

*Görüntüler* konteynerinin genel erişimi `off`. *Küçük resimler* konteynerinin genel erişimi `container`. Genel `container` erişim ayarı, web sayfasını ziyaret eden kullanıcıların küçük resimleri görüntülemesine izin verir.

```azurecli-interactive
blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
  -n $blobStorageAccount --query "[0].value" --output tsv)

az storage container create -n images --account-name $blobStorageAccount \
  --account-key $blobStorageAccountKey --public-access off

az storage container create -n thumbnails --account-name $blobStorageAccount \
  --account-key $blobStorageAccountKey --public-access container

echo "Make a note of your Blob storage account key..."
echo $blobStorageAccountKey
```

Blob depolama hesap adınızı ve anahtarınızı not alın. Örnek uygulama, görüntüleri yüklemek için depolama hesabına bağlanmak için bu ayarları kullanır. 

## <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[App Service planı](../../app-service/overview-hosting-plans.md), uygulamanızı barındıran web sunucusu grubunun konumunu, boyutunu ve özelliklerini belirtir.

[az appservice plan create](/cli/azure/appservice/plan) komutu ile bir App Service planı oluşturun.

Aşağıdaki örnekte, **Ücretsiz** fiyatlandırma katmanı kullanılarak `myAppServicePlan` adlı bir App Service planı oluşturulmaktadır:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku Free
```

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

Web uygulaması, GitHub örnek deposundan dağıtılan örnek uygulama kodu için bir barındırma alanı sağlar. [az webapp create](/cli/azure/webapp) komutuyla `myAppServicePlan` App Service planında bir [web uygulaması](../../app-service/overview.md) oluşturun.  

Aşağıdaki komutta, `<web_app>` benzersiz bir adla değiştirin. Geçerli karakterler: `a-z`, `0-9`, ve `-`. `<web_app>` benzersiz değilse *Belirtilen `<web_app>` adına sahip web sitesi zaten var* hata iletisiyle karşılaşırsınız. Web uygulamasının varsayılan URL'si `https://<web_app>.azurewebsites.net` şeklindedir.  

```azurecli-interactive
webapp="<web_app>"

az webapp create --name $webapp --resource-group myResourceGroup --plan myAppServicePlan
```

## <a name="deploy-the-sample-app-from-the-github-repository"></a>GitHub deposundan örnek uygulamayı dağıtma

# <a name="net-v12-sdk"></a>[\.NET v12 SDK](#tab/dotnet)

App Service bir web uygulamasına içerik dağıtmanın birkaç yolunu destekler. Bu öğreticide, web uygulamasını bir [genel GitHub örnek deposundan](https://github.com/Azure-Samples/storage-blob-upload-from-webapp) dağıtırsınız. [az webapp deployment source config](/cli/azure/webapp/deployment/source) komutuyla Git dağıtımını web uygulamasında gerçekleşecek şekilde yapılandırın.

Örnek proje ASP.NET bir [MVC](https://www.asp.net/mvc) uygulaması içerir. Uygulama bir resmi kabul eder, bir depolama hesabına kaydeder ve küçük resim kapsayıcısından görüntüleri görüntüler. Web uygulaması Azure Depolama hizmetiyle etkileşimde kalmak için [Azure.Storage,](/dotnet/api/azure.storage) [Azure.Storage.Blobs](/dotnet/api/azure.storage.blobs)ve [Azure.Storage.Blobs.Models](/dotnet/api/azure.storage.blobs.models) ad alanlarını kullanır.

```azurecli-interactive
az webapp deployment source config --name $webapp --resource-group myResourceGroup \
  --branch master --manual-integration \
  --repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
```

# <a name="nodejs-v10-sdk"></a>[Düğüm.js v10 SDK](#tab/nodejsv10)
App Service bir web uygulamasına içerik dağıtmanın birkaç yolunu destekler. Bu öğreticide, web uygulamasını bir [genel GitHub örnek deposundan](https://github.com/Azure-Samples/storage-blob-upload-from-webapp-node-v10) dağıtırsınız. [az webapp deployment source config](/cli/azure/webapp/deployment/source) komutuyla Git dağıtımını web uygulamasında gerçekleşecek şekilde yapılandırın.

```azurecli-interactive
az webapp deployment source config --name $webapp --resource-group myResourceGroup \
  --branch master --manual-integration \
  --repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp-node-v10
```

---

## <a name="configure-web-app-settings"></a>Web uygulaması ayarlarını yapılandırma

# <a name="net-v12-sdk"></a>[\.NET v12 SDK](#tab/dotnet)

Örnek web uygulaması, görüntüleri yüklemek [için .NET için Azure Depolama API'lerini](/dotnet/api/overview/azure/storage) kullanır. Depolama hesabı kimlik bilgileri web uygulamasının uygulama ayarlarında ayarlanır. [Az webapp config appsettings set](/cli/azure/webapp/config/appsettings) komutu ile dağıtılan uygulamaya uygulama ayarları ekleyin.

```azurecli-interactive
az webapp config appsettings set --name $webapp --resource-group myResourceGroup \
  --settings AzureStorageConfig__AccountName=$blobStorageAccount \
    AzureStorageConfig__ImageContainer=images \
    AzureStorageConfig__ThumbnailContainer=thumbnails \
    AzureStorageConfig__AccountKey=$blobStorageAccountKey
```

# <a name="nodejs-v10-sdk"></a>[Düğüm.js v10 SDK](#tab/nodejsv10)

Örnek web uygulaması, görüntüleri karşıya yüklemek için kullanılan erişim belirteçlerini istemek için [Azure Depolama İstemci Kitaplığı](https://github.com/Azure/azure-storage-js)’nı kullanır. Depolama SDK tarafından kullanılan depolama hesabı kimlik bilgileri web uygulaması nın uygulama ayarlarında ayarlanır. [Az webapp config appsettings set](/cli/azure/webapp/config/appsettings) komutu ile dağıtılan uygulamaya uygulama ayarları ekleyin.

```azurecli-interactive
az webapp config appsettings set --name $webapp --resource-group myResourceGroup \
  --settings AZURE_STORAGE_ACCOUNT_NAME=$blobStorageAccount \
    AZURE_STORAGE_ACCOUNT_ACCESS_KEY=$blobStorageAccountKey
```

---

Web uygulamasını dağıttıktan ve yapılandırdıktan sonra, uygulamadaki resim yükleme işlevini test edebilirsiniz.

## <a name="upload-an-image"></a>Bir görüntüyü karşıya yükleme

Web uygulamasını test etmek için, yayımlanan uygulamanızın URL'sine gidin. Web uygulamasının varsayılan URL'si `https://<web_app>.azurewebsites.net` şeklindedir.

# <a name="net-v12-sdk"></a>[\.NET v12 SDK](#tab/dotnet)

Dosya belirtmek ve yüklemek için **Fotoğrafları Yükle** bölgesini seçin veya bir dosyayı bölgeye sürükleyin. Görüntü başarıyla karşıya yüklenirse kaybolur. **Oluşturulan Küçük Resimler** bölümü, bu konuda daha sonra test edene kadar boş kalır.

![.NET'e Fotoğraf Yükle](media/storage-upload-process-images/figure1.png)

Örnek kodda, `UploadFileToStorage` *Storagehelper.cs* dosyasındaki görev, [uploadasync](/dotnet/api/azure.storage.blobs.blobclient.uploadasync) yöntemini kullanarak görüntüleri depolama hesabı içindeki *resim* kapsayıcısına yüklemek için kullanılır. Aşağıdaki kod örneği `UploadFileToStorage` görevini içerir.

```csharp
public static async Task<bool> UploadFileToStorage(Stream fileStream, string fileName,
                                                    AzureStorageConfig _storageConfig)
{
    // Create a URI to the blob
    Uri blobUri = new Uri("https://" +
                          _storageConfig.AccountName +
                          ".blob.core.windows.net/" +
                          _storageConfig.ImageContainer +
                          "/" + fileName);

    // Create StorageSharedKeyCredentials object by reading
    // the values from the configuration (appsettings.json)
    StorageSharedKeyCredential storageCredentials =
        new StorageSharedKeyCredential(_storageConfig.AccountName, _storageConfig.AccountKey);

    // Create the blob client.
    BlobClient blobClient = new BlobClient(blobUri, storageCredentials);

    // Upload the file
    await blobClient.UploadAsync(fileStream);

    return await Task.FromResult(true);
}
```

Aşağıdaki sınıflar ve yöntemler, yukarıdaki görevde kullanılır:

| Sınıf    | Yöntem   |
|----------|----------|
| [Urı](/dotnet/api/system.uri) | [Uri yapıcı](/dotnet/api/system.uri.-ctor) |
| [DepolamaSharedKeyCredential](/dotnet/api/azure.storage.storagesharedkeycredential) | [StorageSharedKeyCredential(String, String) oluşturucu](/dotnet/api/azure.storage.storagesharedkeycredential.-ctor) |
| [BlobClient](/dotnet/api/azure.storage.blobs.blobclient) | [UploadAsync](/dotnet/api/azure.storage.blobs.blobclient.uploadasync) |

# <a name="nodejs-v10-sdk"></a>[Düğüm.js v10 SDK](#tab/nodejsv10)

Dosya seçmek için **Dosyayı Seçin'i** seçin ve ardından **Resmi Yükle'yi**tıklatın. **Oluşturulan Küçük Resimler** bölümü, bu konuda daha sonra test edene kadar boş kalır. 

![Node.js V10'a Fotoğraf Yükle](media/storage-upload-process-images/upload-app-nodejs.png)

Örnek kodda `post` yolu, görüntüyü blob kapsayıcısına yüklemeden sorumludur. Yol, modülleri karşıya yükleme işlemine yardımcı olması için kullanır:

- [multer](https://github.com/expressjs/multer) rota işleyicisi için yükleme stratejisini uygular.
- [into-stream,](https://github.com/sindresorhus/into-stream) [arabelleği createBlockBlobFromStream'in](https://azure.github.io/azure-sdk-for-node/azure-storage-legacy/latest/BlobService.html)gerektirdiği şekilde bir akışa dönüştürür.

Dosya rotaya gönderildiğinde, dosyanın içeriği blob kapsayıcısına yüklenene kadar bellekte kalır.

> [!IMPORTANT]
> Büyük dosyaların belleğe yüklenmesi web uygulamanızın performansını olumsuz etkileyebilir. Kullanıcıların büyük dosyalar göndermesini bekliyorsanız, web sunucusu dosya sisteminde dosyaları düzenlemeyi ve ardından Blob depolama alanına yüklemeleri zamanlamayı düşünebilirsiniz. Dosyalar Blob depolama alanına geldikten sonra, bunları sunucu dosya sisteminden kaldırabilirsiniz.

```javascript
const {
  Aborter,
  BlobURL,
  BlockBlobURL,
  ContainerURL,
  ServiceURL,
  StorageURL,
  SharedKeyCredential,
  uploadStreamToBlockBlob
} = require('@azure/storage-blob');

const express = require('express');
const router = express.Router();
const multer = require('multer');
const inMemoryStorage = multer.memoryStorage();
const uploadStrategy = multer({ storage: inMemoryStorage }).single('image');
const getStream = require('into-stream');
const containerName = 'images';
const ONE_MEGABYTE = 1024 * 1024;
const uploadOptions = { bufferSize: 4 * ONE_MEGABYTE, maxBuffers: 20 };
const ONE_MINUTE = 60 * 1000;
const aborter = Aborter.timeout(30 * ONE_MINUTE);

const sharedKeyCredential = new SharedKeyCredential(
  process.env.AZURE_STORAGE_ACCOUNT_NAME,
  process.env.AZURE_STORAGE_ACCOUNT_ACCESS_KEY);
const pipeline = StorageURL.newPipeline(sharedKeyCredential);
const serviceURL = new ServiceURL(
  `https://${process.env.AZURE_STORAGE_ACCOUNT_NAME}.blob.core.windows.net`,
  pipeline
);

const getBlobName = originalName => {
  // Use a random number to generate a unique file name, 
  // removing "0." from the start of the string.
  const identifier = Math.random().toString().replace(/0\./, ''); 
  return `${identifier}-${originalName}`;
};

router.post('/', uploadStrategy, async (req, res) => {

    const blobName = getBlobName(req.file.originalname);
    const stream = getStream(req.file.buffer);
    const containerURL = ContainerURL.fromServiceURL(serviceURL, containerName);
    const blobURL = BlobURL.fromContainerURL(containerURL, blobName);
    const blockBlobURL = BlockBlobURL.fromBlobURL(blobURL);

    try {
      
      await uploadStreamToBlockBlob(aborter, stream,
        blockBlobURL, uploadOptions.bufferSize, uploadOptions.maxBuffers);

      res.render('success', { message: 'File uploaded to Azure Blob storage.' });   

    } catch (err) {

      res.render('error', { message: 'Something went wrong.' });

    }
});
```
---

## <a name="verify-the-image-is-shown-in-the-storage-account"></a>Depolama hesabında görüntünün gösterildiğini doğrulayın

[Azure Portal](https://portal.azure.com) oturum açın. Soldaki menüden **Depolama hesapları**’nı ve sonra depolama hesabınızın adını seçin. **Kapsayıcılar'ı**seçin, ardından **resim** kapsayıcısını seçin.

Kapsayıcıda görüntünün gösterildiğini doğrulayın.

![Resim konteynerinin Azure Portal listesi](media/storage-upload-process-images/figure13.png)

## <a name="test-thumbnail-viewing"></a>Küçük resim görüntülemeyi test etme

Küçük resim görüntülemeyi test etmek için, uygulamanın **küçük resim** kabını okuyup okuyamadığını kontrol etmek için **küçük resim** konteynerine bir resim yüklersiniz.

[Azure Portal](https://portal.azure.com) oturum açın. Soldaki menüden **Depolama hesapları**’nı ve sonra depolama hesabınızın adını seçin. **Kapsayıcılar'ı**seçin, ardından **küçük küçük resim** kapsayıcısını seçin. **Karşıya Yükle**’yi seçerek **Blobu karşıya yükle** bölmesini açın.

Dosya seçiciile bir dosya seçin ve **Yükle'yi**seçin.

**thumbnails** kapsayıcısına yüklenen görüntünün görünür olduğunu doğrulamak için uygulamanıza geri gidin.

# <a name="net-v12-sdk"></a>[\.NET v12 SDK](#tab/dotnet)
![Yeni görüntü görüntülenen .NET görüntü resizer uygulaması](media/storage-upload-process-images/figure2.png)

# <a name="nodejs-v10-sdk"></a>[Düğüm.js v10 SDK](#tab/nodejsv10)
![Yeni görüntü görüntülenen Node.js V10 görüntü resizer uygulaması](media/storage-upload-process-images/upload-app-nodejs-thumb.png)

---

Serinin ikinci bölümünde, bu görüntüye gerek kalmadan küçük resim oluşturmayı otomatikleştirirsiniz. Azure portalındaki **thumbnails** kapsayıcısında, karşıya yüklediğiniz görüntüyü seçin ve **Sil**’i seçerek görüntüyü silin. 

Azure depolama hesabınızdan içeriği önbelleğe almak için İçerik Dağıtım Ağı'nı (CDN) etkinleştirebilirsiniz. Azure depolama hesabınızla CDN'yi etkinleştirme hakkında daha fazla bilgi için [bkz.](../../cdn/cdn-create-a-storage-account-with-cdn.md)

## <a name="next-steps"></a>Sonraki adımlar

Serinin birinci bölümünde, depolama yla etkileşimde olacak bir web uygulamasını nasıl yapılandırabileceğinizi öğrendiniz.

Görüntüyü yeniden boyutlandırmak için bir Azure işlevini tetiklemek için Olay Izgarasını kullanma hakkında bilgi edinmek için serinin ikinci bölümüne gidin.

> [!div class="nextstepaction"]
> [Karşıya yüklenen bir görüntüyü yeniden boyutlandırmak üzere bir Azure İşlevi’ni tetiklemek için Event Grid kullanma](../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
