---
title: Azure Depolama ile buluta görüntü verileri yükleme | Microsoft Docs
description: Uygulama verilerini depolamak için bir Web uygulamasıyla Azure Blob depolamayı kullanma
author: mhopkins-msft
ms.service: storage
ms.subservice: blobs
ms.topic: tutorial
ms.date: 03/06/2020
ms.author: mhopkins
ms.reviewer: dineshm
ms.openlocfilehash: 3c475787eafde4ba847b292df57e4b0d18cfe5d0
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83196043"
---
# <a name="tutorial-upload-image-data-in-the-cloud-with-azure-storage"></a>Öğretici: Azure depolama ile buluta görüntü verileri yükleme

Bu öğretici, bir dizinin birinci bölümüdür. Bu öğreticide, bir depolama hesabına görüntü yüklemek için Azure Blob depolama istemci kitaplığı 'nı kullanan bir Web uygulamasını dağıtmayı öğreneceksiniz. İşiniz bittiğinde, Azure depolama 'daki görüntüleri depolayan ve görüntüleyen bir Web uygulamasına sahip olursunuz.

# <a name="net-v12"></a>[\.NET V12](#tab/dotnet)
![.NET 'teki görüntü yeniden Oluşturucu uygulaması](media/storage-upload-process-images/figure2.png)

# <a name="nodejs-v10"></a>[Node. js ile v10 arasındaki](#tab/nodejsv10)
![Node. js Ile v10 arasındaki içinde görüntü yeniden Oluşturucu uygulaması](media/storage-upload-process-images/upload-app-nodejs-thumb.png)

---

Serinin birinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Depolama hesabı oluşturma
> * Kapsayıcı oluşturma ve izinleri ayarlama
> * Erişim anahtarı alma
> * Bir Web uygulamasını Azure 'a dağıtma
> * Uygulama ayarlarını yapılandırma
> * Web uygulamasıyla etkileşim kurma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için bir Azure aboneliğinizin olması gerekir. Başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yüklemek ve kullanmak için bu öğreticide, Azure CLı sürüm 2.0.4 veya üstünü çalıştırmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.  

Aşağıdaki örnek `myResourceGroup` adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location southeastasia
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Örnek, görüntüleri bir Azure depolama hesabındaki blob kapsayıcısına yükler. Depolama hesabı, Azure Storage veri nesnelerinizi depolamak ve bunlara erişmek için benzersiz ad alanı sağlar. Oluşturduğunuz kaynak grubunda [az storage account create](/cli/azure/storage/account) komutunu kullanarak bir depolama hesabı oluşturun.

> [!IMPORTANT]
> Öğreticinin 2. bölümünde blob depolamayla Azure Event Grid kullanırsınız. Depolama hesabınızı, Event Grid destekleyen bir Azure bölgesinde oluşturduğunuzdan emin olun. Desteklenen bölgelerin listesi için bkz. [bölgeye göre Azure ürünleri](https://azure.microsoft.com/global-infrastructure/services/?products=event-grid&regions=all).

Aşağıdaki komutta, yer tutucuyu gördüğünüz BLOB depolama hesabı için kendi genel benzersiz adınızı değiştirin `<blob_storage_account>` .

```azurecli-interactive
blobStorageAccount="<blob_storage_account>"

az storage account create --name $blobStorageAccount --location southeastasia \
  --resource-group myResourceGroup --sku Standard_LRS --kind StorageV2 --access-tier hot
```

## <a name="create-blob-storage-containers"></a>BLOB depolama kapsayıcıları oluşturma

Uygulama, Blob depolama hesabında iki kapsayıcı kullanır. Kapsayıcılar, klasörlere ve depo bloblarına benzer. *Images* kapsayıcısı, uygulamanın tam çözünürlüklü görüntüleri yüklediği yerdir. Serinin sonraki bölümlerinde bir Azure işlev uygulaması, yeniden boyutlandırılan küçük resimleri *thumbnails* kapsayıcısına yükler.

[az storage account keys list](/cli/azure/storage/account/keys) komutunu kullanarak depolama hesabı anahtarını alın. Ardından, [az Storage Container Create](/cli/azure/storage/container) komutuyla iki kapsayıcı oluşturmak için bu anahtarı kullanın.

*Görüntüler* kapsayıcısının genel erişimi olarak ayarlanır `off` . *Küçük resim* kapsayıcısının genel erişimi olarak ayarlanır `container` . `container`Genel erişim ayarı, Web sayfasını ziyaret eden kullanıcıların küçük resimleri görüntülemesini sağlar.

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

BLOB depolama hesabı adınızı ve anahtarınızı bir yere göz önüne alın. Örnek uygulama, görüntüleri karşıya yüklemek için depolama hesabına bağlanmak üzere bu ayarları kullanır. 

## <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[App Service planı](../../app-service/overview-hosting-plans.md), uygulamanızı barındıran web sunucusu grubunun konumunu, boyutunu ve özelliklerini belirtir.

[az appservice plan create](/cli/azure/appservice/plan) komutu ile bir App Service planı oluşturun.

Aşağıdaki örnekte, **Ücretsiz** fiyatlandırma katmanı kullanılarak `myAppServicePlan` adlı bir App Service planı oluşturulmaktadır:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku Free
```

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

Web uygulaması, GitHub örnek deposundan dağıtılan örnek uygulama kodu için bir barındırma alanı sağlar. [az webapp create](/cli/azure/webapp) komutuyla `myAppServicePlan` App Service planında bir [web uygulaması](../../app-service/overview.md) oluşturun.  

Aşağıdaki komutta, öğesini `<web_app>` benzersiz bir adla değiştirin. Geçerli karakterler: `a-z`, `0-9`, ve `-`. `<web_app>` benzersiz değilse *Belirtilen `<web_app>` adına sahip web sitesi zaten var* hata iletisiyle karşılaşırsınız. Web uygulamasının varsayılan URL'si `https://<web_app>.azurewebsites.net` şeklindedir.  

```azurecli-interactive
webapp="<web_app>"

az webapp create --name $webapp --resource-group myResourceGroup --plan myAppServicePlan
```

## <a name="deploy-the-sample-app-from-the-github-repository"></a>GitHub deposundan örnek uygulamayı dağıtma

# <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

App Service bir web uygulamasına içerik dağıtmanın birkaç yolunu destekler. Bu öğreticide, web uygulamasını bir [genel GitHub örnek deposundan](https://github.com/Azure-Samples/storage-blob-upload-from-webapp) dağıtırsınız. [az webapp deployment source config](/cli/azure/webapp/deployment/source) komutuyla Git dağıtımını web uygulamasında gerçekleşecek şekilde yapılandırın.

Örnek proje bir [ASP.NET MVC](https://www.asp.net/mvc) uygulaması içerir. Uygulama bir görüntüyü kabul eder, bir depolama hesabına kaydeder ve küçük resim kapsayıcılarından görüntüleri görüntüler. Web uygulaması, Azure depolama hizmetiyle etkileşim kurmak için [Azure. Storage](/dotnet/api/azure.storage), [Azure. Storage. blob 'ları](/dotnet/api/azure.storage.blobs)ve [Azure. Storage. blob. model](/dotnet/api/azure.storage.blobs.models) ad alanlarını kullanır.

```azurecli-interactive
az webapp deployment source config --name $webapp --resource-group myResourceGroup \
  --branch master --manual-integration \
  --repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
```

# <a name="nodejs-v10"></a>[Node. js ile v10 arasındaki](#tab/nodejsv10)
App Service bir web uygulamasına içerik dağıtmanın birkaç yolunu destekler. Bu öğreticide, web uygulamasını bir [genel GitHub örnek deposundan](https://github.com/Azure-Samples/storage-blob-upload-from-webapp-node-v10) dağıtırsınız. [az webapp deployment source config](/cli/azure/webapp/deployment/source) komutuyla Git dağıtımını web uygulamasında gerçekleşecek şekilde yapılandırın.

```azurecli-interactive
az webapp deployment source config --name $webapp --resource-group myResourceGroup \
  --branch master --manual-integration \
  --repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp-node-v10
```

---

## <a name="configure-web-app-settings"></a>Web uygulaması ayarlarını yapılandırma

# <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

Örnek Web uygulaması, görüntüleri karşıya yüklemek için [.net Için Azure depolama API 'lerini](/dotnet/api/overview/azure/storage) kullanır. Depolama hesabı kimlik bilgileri, Web uygulaması için uygulama ayarları 'nda ayarlanır. Uygulama ayarlarını, [az WebApp config appSettings set](/cli/azure/webapp/config/appsettings) komutuyla dağıtılan uygulamaya ekleyin.

```azurecli-interactive
az webapp config appsettings set --name $webapp --resource-group myResourceGroup \
  --settings AzureStorageConfig__AccountName=$blobStorageAccount \
    AzureStorageConfig__ImageContainer=images \
    AzureStorageConfig__ThumbnailContainer=thumbnails \
    AzureStorageConfig__AccountKey=$blobStorageAccountKey
```

# <a name="nodejs-v10"></a>[Node. js ile v10 arasındaki](#tab/nodejsv10)

Örnek web uygulaması, görüntüleri karşıya yüklemek için kullanılan erişim belirteçlerini istemek için [Azure Depolama İstemci Kitaplığı](https://github.com/Azure/azure-storage-js)’nı kullanır. Depolama SDK 'Sı tarafından kullanılan depolama hesabı kimlik bilgileri, Web uygulaması için uygulama ayarları 'nda ayarlanır. Uygulama ayarlarını, [az WebApp config appSettings set](/cli/azure/webapp/config/appsettings) komutuyla dağıtılan uygulamaya ekleyin.

```azurecli-interactive
az webapp config appsettings set --name $webapp --resource-group myResourceGroup \
  --settings AZURE_STORAGE_ACCOUNT_NAME=$blobStorageAccount \
    AZURE_STORAGE_ACCOUNT_ACCESS_KEY=$blobStorageAccountKey
```

---

Web uygulamasını dağıttıktan ve yapılandırdıktan sonra, uygulamanın görüntü karşıya yükleme işlevini test edebilirsiniz.

## <a name="upload-an-image"></a>Bir görüntüyü karşıya yükleme

Web uygulamasını test etmek için, yayımlanan uygulamanızın URL'sine gidin. Web uygulamasının varsayılan URL'si `https://<web_app>.azurewebsites.net` şeklindedir.

# <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

Bir dosya belirtmek ve karşıya yüklemek için **Fotoğrafları karşıya yükle** bölgesini seçin ya da bir dosyayı bölgenin üzerine sürükleyin. Görüntü başarıyla karşıya yüklenirse kaybolur. **Oluşturulan küçük resimler** bölümü, bu konunun ilerleyen kısımlarında Test edilene kadar boş kalır.

![.NET 'e fotoğraf yükleme](media/storage-upload-process-images/figure1.png)

Örnek kodda, `UploadFileToStorage` *Storagehelper.cs* dosyasındaki görev, görüntüleri [uploadasync](/dotnet/api/azure.storage.blobs.blobclient.uploadasync) yöntemi kullanılarak depolama hesabı içindeki *görüntüler* kapsayıcısına yüklemek için kullanılır. Aşağıdaki kod örneği `UploadFileToStorage` görevini içerir.

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
| [Kullanılmamışsa](/dotnet/api/system.uri) | [URI Oluşturucusu](/dotnet/api/system.uri.-ctor) |
| [StorageSharedKeyCredential](/dotnet/api/azure.storage.storagesharedkeycredential) | [StorageSharedKeyCredential (dize, dize) Oluşturucusu](/dotnet/api/azure.storage.storagesharedkeycredential.-ctor) |
| [BlobClient](/dotnet/api/azure.storage.blobs.blobclient) | [UploadAsync](/dotnet/api/azure.storage.blobs.blobclient.uploadasync) |

# <a name="nodejs-v10"></a>[Node. js ile v10 arasındaki](#tab/nodejsv10)

Dosya **Seç ' i seçin ve** ardından **görüntüyü karşıya yükle**' ye tıklayın. **Oluşturulan küçük resimler** bölümü, bu konunun ilerleyen kısımlarında Test edilene kadar boş kalır. 

![Node. js Ile v10 arasındaki 'a fotoğraf yükleme](media/storage-upload-process-images/upload-app-nodejs.png)

Örnek kodda `post` yolu, görüntüyü blob kapsayıcısına yüklemeden sorumludur. Yol, modülleri karşıya yükleme işlemine yardımcı olması için kullanır:

- [Multer](https://github.com/expressjs/multer) , yol işleyicisi için karşıya yükleme stratejisi uygular.
- [Stream](https://github.com/sindresorhus/into-stream) , [Createblockblobfromstream](https://azure.github.io/azure-sdk-for-node/azure-storage-legacy/latest/BlobService.html)için gereken şekilde arabelleği bir akışa dönüştürür.

Dosya rotaya gönderilirken dosyanın içeriği, dosya BLOB kapsayıcısına yükleninceye kadar bellekte kalır.

> [!IMPORTANT]
> Büyük dosyaların belleğe yüklenmesi Web uygulamanızın performansı üzerinde olumsuz bir etkiye sahip olabilir. Kullanıcıların büyük dosyaları nakletmesini beklemeniz durumunda, Web sunucusu dosya sisteminde dosyaları hazırlama ve sonra blob depolamaya yükleme zamanlama işlemlerini düşünebilirsiniz. Dosyalar BLOB depolama alanından olduktan sonra bunları sunucu dosya sisteminden kaldırabilirsiniz.

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

[Azure Portal](https://portal.azure.com) oturum açın. Soldaki menüden **Depolama hesapları**’nı ve sonra depolama hesabınızın adını seçin. **Kapsayıcılar**' ı seçin ve ardından **görüntüler** kapsayıcısını seçin.

Kapsayıcıda görüntünün gösterildiğini doğrulayın.

![Görüntüler kapsayıcısının listesini Azure portal](media/storage-upload-process-images/figure13.png)

## <a name="test-thumbnail-viewing"></a>Küçük resim görüntülemeyi test etme

Küçük resim görüntülemeyi test etmek için **, küçük resim kapsayıcısına bir** görüntü yükleyerek uygulamanın **küçük** resim kapsayıcısını okuyup okuyamadığını kontrol edebilirsiniz.

[Azure Portal](https://portal.azure.com) oturum açın. Soldaki menüden **Depolama hesapları**’nı ve sonra depolama hesabınızın adını seçin. **Kapsayıcılar**' ı seçin ve ardından **küçük resim** kapsayıcısını seçin. **Karşıya Yükle**’yi seçerek **Blobu karşıya yükle** bölmesini açın.

Dosya seçicisine sahip bir dosya seçin ve **karşıya yükle**' yi seçin.

**thumbnails** kapsayıcısına yüklenen görüntünün görünür olduğunu doğrulamak için uygulamanıza geri gidin.

# <a name="net-v12"></a>[\.NET V12](#tab/dotnet)
![Yeni görüntü görüntülenirken .NET görüntü yeniden Oluşturucu uygulaması](media/storage-upload-process-images/figure2.png)

# <a name="nodejs-v10"></a>[Node. js ile v10 arasındaki](#tab/nodejsv10)
![Yeni görüntü görüntülenirken Node. js Ile v10 arasındaki görüntü yeniden Oluşturucu uygulaması](media/storage-upload-process-images/upload-app-nodejs-thumb.png)

---

Serinin ikinci bölümünde, küçük resim oluşturmayı otomatikleştirerek bu görüntüye ihtiyacınız olmaz. Azure portalındaki **thumbnails** kapsayıcısında, karşıya yüklediğiniz görüntüyü seçin ve **Sil**’i seçerek görüntüyü silin. 

Azure Depolama hesabınızdaki içerikleri önbelleğe almak için Content Delivery Network (CDN) etkinleştirebilirsiniz. Azure depolama hesabınızla CDN 'yi etkinleştirme hakkında daha fazla bilgi için bkz. [Azure Storage hesabını Azure CDN Ile tümleştirme](../../cdn/cdn-create-a-storage-account-with-cdn.md).

## <a name="next-steps"></a>Sonraki adımlar

Serinin birinci bölümünde, bir Web uygulamasını depolama ile etkileşimde bulunmak üzere nasıl yapılandıracağınızı öğrendiniz.

Bir görüntüyü yeniden boyutlandırmak üzere bir Azure işlevi tetiklemek için Event Grid kullanmayı öğrenmek üzere serinin ikinci bölümüne gidin.

> [!div class="nextstepaction"]
> [Karşıya yüklenen bir görüntüyü yeniden boyutlandırmak üzere bir Azure İşlevi’ni tetiklemek için Event Grid kullanma](../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
