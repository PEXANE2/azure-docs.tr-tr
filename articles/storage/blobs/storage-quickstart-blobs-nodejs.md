---
title: 'Quickstart: Azure Blob depolama kitaplığı v12 - JavaScript'
description: Bu hızlı başlangıçta, Blob (nesne) depolama alanında bir kapsayıcı ve bir blob oluşturmak için JavaScript için Azure Blob depolama istemcisi kitaplık 12 sürümünü nasıl kullanacağınızı öğrenirsiniz. Ardından, blob’u yerel bilgisayarınıza indirmeyi ve bir kapsayıcıdaki tüm blobların listesini görüntülemeyi öğreneceksiniz.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 01/24/2020
ms.service: storage
ms.subservice: blobs
ms.topic: quickstart
ms.openlocfilehash: e68e91d90846ab77b994b53be7a84a9dd8bc5a25
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79241041"
---
# <a name="quickstart-manage-blobs-with-javascript-v12-sdk-in-nodejs"></a>Quickstart: Node.js JavaScript v12 SDK ile blobs yönetin

Bu hızlı başlangıçta, Node.js kullanarak lekeleri yönetmeyi öğrenirsiniz. Blobs, resimler, belgeler, akışlı ortam ve arşiv verileri de dahil olmak üzere büyük miktarda metin veya ikili veri tutabilen nesnelerdir. Blob'ları yükler, indirir ve listelersiniz ve kapsayıcılar oluşturup silebilirsiniz.

[API referans belgeleri](/javascript/api/@azure/storage-blob) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-blob) | [Paketi (Düğüm Paket Yöneticisi)](https://www.npmjs.com/package/@azure/storage-blob) | [Örnekleri](https://docs.microsoft.com/azure/storage/common/storage-samples-javascript?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-samples)

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- Azure Depolama hesabı. [Bir depolama hesabı oluşturun.](../common/storage-account-create.md)
- [Düğüm.js](https://nodejs.org/en/download/).

> [!NOTE]
> Önceki SDK sürümüile başlamak için [Quickstart: Node.js'de JavaScript v10 SDK ile blobları yönetin.](storage-quickstart-blobs-nodejs-legacy.md)

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="setting-up"></a>Ayarlama

Bu bölüm, JavaScript için Azure Blob depolama istemcisi kitaplığı v12 ile çalışmak üzere bir proje hazırlamakonusunda size yol sunar.

### <a name="create-the-project"></a>Proje oluşturma

*blob-quickstart-v12*adlı bir JavaScript uygulaması oluşturun.

1. Konsol penceresinde (cmd, PowerShell veya Bash gibi), proje için yeni bir dizin oluşturun.

    ```console
    mkdir blob-quickstart-v12
    ```

1. Yeni oluşturulan *blob-quickstart-v12* dizinine geçin.

    ```console
    cd blob-quickstart-v12
    ```

1. *package.json*adlı yeni bir metin dosyası oluşturun. Bu dosya Düğüm.js projesini tanımlar. Bu dosyayı *blob-quickstart-v12* dizinine kaydedin. İşte dosyanın içeriği:

    ```json
    {
        "name": "blob-quickstart-v12",
        "version": "1.0.0",
        "description": "Use the @azure/storage-blob SDK version 12 to interact with Azure Blob storage",
        "main": "blob-quickstart-v12.js",
        "scripts": {
            "start": "node blob-quickstart-v12.js"
        },
        "author": "Your Name",
        "license": "MIT",
        "dependencies": {
            "@azure/storage-blob": "^12.0.0",
            "@types/dotenv": "^4.0.3",
            "dotenv": "^6.0.0"
        }
    }
    ```
    
    İstersen sahaya kendi adını `author` koyabilirsin.
   
### <a name="install-the-package"></a>Paketi yükleyin

*Blob-quickstart-v12* dizinindeyken, komutu kullanarak JavaScript paketi için Azure Blob `npm install` depolama istemcisi kitaplığını yükleyin. Bu komut *package.json* dosyasını okur ve JavaScript paketi için Azure Blob depolama istemcisi kitaplığını v12'yi ve bağlı olduğu tüm kitaplıkları yükler.

```console
npm install
```

### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

Proje dizininden:

1. Kod düzenleyicinizde başka bir yeni metin dosyası açma
1. Azure `require` ve Node.js modüllerini yüklemek için arama ekleme
1. Temel özel durum işleme de dahil olmak üzere program için yapı oluşturma

    İşte kod:

    ```javascript
    const { BlobServiceClient } = require('@azure/storage-blob');
    const uuidv1 = require('uuid/v1');
    
    async function main() {
        console.log('Azure Blob storage v12 - JavaScript quickstart sample');
        // Quick start code goes here
    }
    
    main().then(() => console.log('Done')).catch((ex) => console.log(ex.message));
    ```

1. Blob-quickstart-v12 dizininde yeni dosyayı *blob-quickstart-v12.js* olarak kaydedin. *blob-quickstart-v12*

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

## <a name="object-model"></a>Nesne modeli

Azure Blob depolama, büyük miktarda yapılandırılmamış veri depolamak için optimize edilebiyi sunar. Yapılandırılmamış veriler, metin veya ikili veriler gibi belirli bir veri modeline veya tanıma bağlı olmayan verilerdir. Blob depolama üç tür kaynak sunar:

* Depolama hesabı
* Depolama hesabındaki bir kapsayıcı
* Konteynerde bir leke

Aşağıdaki diyagramda bu kaynaklar arasındaki ilişki gösterilmektedir.

![Blob depolama mimarisi diyagramı](./media/storage-blobs-introduction/blob1.png)

Bu kaynaklarla etkileşimde kalmak için aşağıdaki JavaScript sınıflarını kullanın:

* [BlobServiceClient](/javascript/api/@azure/storage-blob/blobserviceclient): `BlobServiceClient` Sınıf, Azure Depolama kaynaklarını ve blob kapsayıcılarını işlemenizi sağlar.
* [ContainerClient](/javascript/api/@azure/storage-blob/containerclient): `ContainerClient` Sınıf, Azure Depolama kapsayıcılarını ve bunların lekelerini işlemenizi sağlar.
* [BlobClient](/javascript/api/@azure/storage-blob/blobclient): `BlobClient` Sınıf, Azure Depolama lekelerini işlemenizi sağlar.

## <a name="code-examples"></a>Kod örnekleri

Bu örnek kod parçacıkları, JavaScript için Azure Blob depolama istemcisi kitaplığıyla aşağıdakileri nasıl gerçekleştireceklerini gösterir:

* [Bağlantı dizesini alma](#get-the-connection-string)
* [Bir kapsayıcı oluşturma](#create-a-container)
* [Blob'ları bir kapsayıcıya yükleme](#upload-blobs-to-a-container)
* [Blob’ları bir kapsayıcıda listeleme](#list-the-blobs-in-a-container)
* [Blob’ları indirme](#download-blobs)
* [Kapsayıcı silme](#delete-a-container)

### <a name="get-the-connection-string"></a>Bağlantı dizesini alma

Aşağıdaki kod, [depolama bağlantı dizesi bölümüyapılanınoluşturulan](#configure-your-storage-connection-string) ortam değişkeninden depolama hesabının bağlantı dizesini alır.

Bu kodu işlevin `main` içine ekleyin:

```javascript
// Retrieve the connection string for use with the application. The storage
// connection string is stored in an environment variable on the machine
// running the application called AZURE_STORAGE_CONNECTION_STRING. If the
// environment variable is created after the application is launched in a
// console or with Visual Studio, the shell or application needs to be closed
// and reloaded to take the environment variable into account.
const AZURE_STORAGE_CONNECTION_STRING = process.env.AZURE_STORAGE_CONNECTION_STRING;
```

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Yeni kapsayıcı için bir ad belirleyin. Aşağıdaki kod, benzersiz olduğundan emin olmak için kapsayıcı adına bir UUID değerini ekler.

> [!IMPORTANT]
> Kapsayıcı adlarının küçük harfle yazılması gerekir. Kapsayıcıları ve blobları adlandırma hakkında daha fazla bilgi için bkz. [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

[FromConnectionString](/javascript/api/@azure/storage-blob/blobserviceclient#fromconnectionstring-string--storagepipelineoptions-) yöntemini arayarak [BlobServiceClient](/javascript/api/@azure/storage-blob/blobserviceclient) sınıfının bir örneğini oluşturun. Ardından, bir kapsayıcıya başvuru almak için [getContainerClient](/javascript/api/@azure/storage-blob/blobserviceclient#getcontainerclient-string-) yöntemini arayın. Son olarak, depolama hesabınızdaki kapsayıcıyı oluşturmak için [arama oluşturun.](/javascript/api/@azure/storage-blob/containerclient#create-containercreateoptions-)

Bu kodu `main` işlevin sonuna ekleyin:

```javascript
// Create the BlobServiceClient object which will be used to create a container client
const blobServiceClient = await BlobServiceClient.fromConnectionString(AZURE_STORAGE_CONNECTION_STRING);

// Create a unique name for the container
const containerName = 'quickstart' + uuidv1();

console.log('\nCreating container...');
console.log('\t', containerName);

// Get a reference to a container
const containerClient = await blobServiceClient.getContainerClient(containerName);

// Create the container
const createContainerResponse = await containerClient.create();
console.log("Container was created successfully. requestId: ", createContainerResponse.requestId);
```

### <a name="upload-blobs-to-a-container"></a>Blob'ları bir kapsayıcıya yükleme

Aşağıdaki kod parçacığı:

1. Blob'a yüklemek için bir metin dizesi oluşturur.
1. [Kapsayıcı Oluştur](#create-a-container) bölümünden [ContainerClient'daki](/javascript/api/@azure/storage-blob/containerclient) [getBlockBlobClient](/javascript/api/@azure/storage-blob/containerclient#getblockblobclient-string-) yöntemini arayarak [BlockBlobClient](/javascript/api/@azure/storage-blob/blockblobclient) nesnesine başvuru alır.
1. [Yükleme](/javascript/api/@azure/storage-blob/blockblobclient#upload-httprequestbody--number--blockblobuploadoptions-) yöntemini arayarak metin dize verilerini blob'a yükler.

Bu kodu `main` işlevin sonuna ekleyin:

```javascript
// Create a unique name for the blob
const blobName = 'quickstart' + uuidv1() + '.txt';

// Get a block blob client
const blockBlobClient = containerClient.getBlockBlobClient(blobName);

console.log('\nUploading to Azure storage as blob:\n\t', blobName);

// Upload data to the blob
const data = 'Hello, World!';
const uploadBlobResponse = await blockBlobClient.upload(data, data.length);
console.log("Blob was uploaded successfully. requestId: ", uploadBlobResponse.requestId);
```

### <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

[ListeBlobsFlat](/javascript/api/@azure/storage-blob/containerclient#listblobsflat-containerlistblobsoptions-) yöntemini arayarak konteynerdeki lekeleri listeleyin. Bu durumda, kapsayıcıya yalnızca bir blob eklenmiştir, bu nedenle listeleme işlemi yalnızca bir blob döndürür.

Bu kodu `main` işlevin sonuna ekleyin:

```javascript
console.log('\nListing blobs...');

// List the blob(s) in the container.
for await (const blob of containerClient.listBlobsFlat()) {
    console.log('\t', blob.name);
}
```

### <a name="download-blobs"></a>Blob’ları indirme

[İndirme](/javascript/api/@azure/storage-blob/blockblobclient#download-undefined---number--undefined---number--blobdownloadoptions-) yöntemini arayarak daha önce oluşturulmuş blob'u indirin. Örnek kod, bir dize `streamToString`içine bir Düğüm.js okunabilir akışı okumak için kullanılan , adlı bir yardımcı işlevi içerir.

Bu kodu `main` işlevin sonuna ekleyin:

```javascript
// Get blob content from position 0 to the end
// In Node.js, get downloaded data by accessing downloadBlockBlobResponse.readableStreamBody
// In browsers, get downloaded data by accessing downloadBlockBlobResponse.blobBody
const downloadBlockBlobResponse = await blockBlobClient.download(0);
console.log('\nDownloaded blob content...');
console.log('\t', await streamToString(downloadBlockBlobResponse.readableStreamBody));
```

*İşlevden* `main` sonra bu yardımcı işlevini ekleyin:

```javascript
// A helper function used to read a Node.js readable stream into a string
async function streamToString(readableStream) {
  return new Promise((resolve, reject) => {
    const chunks = [];
    readableStream.on("data", (data) => {
      chunks.push(data.toString());
    });
    readableStream.on("end", () => {
      resolve(chunks.join(""));
    });
    readableStream.on("error", reject);
  });
}
```

### <a name="delete-a-container"></a>Kapsayıcı silme

Aşağıdaki kod, [silme](/javascript/api/@azure/storage-blob/containerclient#delete-containerdeletemethodoptions-) yöntemini kullanarak tüm kapsayıcıyı kaldırarak uygulamanın oluşturduğu kaynakları temizler. İsterseniz yerel dosyaları da silebilirsiniz.

Bu kodu `main` işlevin sonuna ekleyin:

```javascript
console.log('\nDeleting container...');

// Delete container
const deleteContainerResponse = await containerClient.delete();
console.log("Container was deleted successfully. requestId: ", deleteContainerResponse.requestId);
```

## <a name="run-the-code"></a>Kodu çalıştırma

Bu uygulama bir metin dizesi oluşturur ve Blob depolama ya yükler. Örnek daha sonra kapsayıcıdaki blob(lar)ı listeler, blob'u karşıdan yükler ve indirilen verileri görüntüler.

Konsol isteminden, *blob-quickstart-v12.py* dosyasını içeren dizine gidin ve `node` uygulamayı çalıştırmak için aşağıdaki komutu uygulayın.

```console
node blob-quickstart-v12.js
```

Uygulamanın çıktısı aşağıdaki örneğe benzer:

```output
Azure Blob storage v12 - JavaScript quickstart sample

Creating container...
         quickstart4a0780c0-fb72-11e9-b7b9-b387d3c488da

Uploading to Azure Storage as blob:
         quickstart4a3128d0-fb72-11e9-b7b9-b387d3c488da.txt

Listing blobs...
         quickstart4a3128d0-fb72-11e9-b7b9-b387d3c488da.txt

Downloaded blob content...
         Hello, World!

Deleting container...
Done
```

Hata ayıklayıcınızdaki kodu gözden geçirin ve işlem boyunca [Azure portalınızı](https://portal.azure.com) kontrol edin. Kapsayıcının oluşturulmakta olup olmadığını denetleyin. Kabın içindeki lekeyi açabilir ve içindekileri görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, JavaScript'i kullanarak blob'ları yüklemeyi, indirmeyi ve listelemeyi öğrendiniz.

Öğreticiler, örnekler, hızlı başlangıçlar ve diğer belgeler için şu adresi ziyaret edin:

> [!div class="nextstepaction"]
> [JavaScript belgeleri için Azure](/azure/javascript/)

* Daha fazla bilgi için [JavaScript için Azure Blob depolama istemcisi kitaplığına](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/storage/storage-blob)bakın.
* Blob depolama örnek uygulamalarını görmek için [Azure Blob depolama istemci kitaplığı v12 JavaScript örneklerine](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-blob/samples)devam edin.
