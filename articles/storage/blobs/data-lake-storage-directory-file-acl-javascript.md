---
title: Azure Data Lake Storage 2. & ACL 'Leri için JavaScript kullanma
description: Hiyerarşik ad alanı (HNS) etkin olan depolama hesaplarında dizinleri ve dosya ve Dizin erişim denetim listelerini (ACL) yönetmek için JavaScript için Azure depolama Data Lake istemci kitaplığı 'nı kullanın.
author: normesta
ms.service: storage
ms.date: 03/20/2020
ms.author: normesta
ms.topic: how-to
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.openlocfilehash: 506d9cd9581172d6eb1f36921ab96e8731ea3803
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87089439"
---
# <a name="use-javascript-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. içinde dizinleri, dosyaları ve ACL 'Leri yönetmek için JavaScript kullanın

Bu makalede, hiyerarşik ad alanı (HNS) etkin olan depolama hesaplarında Dizin, dosya ve izinleri oluşturmak ve yönetmek için JavaScript 'In nasıl kullanılacağı gösterilmektedir. 

[Paket (düğüm Paket Yöneticisi)](https://www.npmjs.com/package/@azure/storage-file-datalake)  |  [Örnekler](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-file-datalake/samples)  |  [Geri bildirimde](https://github.com/Azure/azure-sdk-for-java/issues) bulunun

## <a name="prerequisites"></a>Önkoşullar

> [!div class="checklist"]
> * Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
> * Hiyerarşik ad alanı (HNS) etkin olan bir depolama hesabı. Bir tane oluşturmak için [Bu](data-lake-storage-quickstart-create-account.md) yönergeleri izleyin.
> * Bu paketi bir Node.js uygulamasında kullanıyorsanız, Node.js 8.0.0 veya daha yüksek bir sürüme sahip olmanız gerekir.

## <a name="set-up-your-project"></a>Projenizi ayarlama

Bir Terminal penceresi açarak ve ardından aşağıdaki komutu yazarak JavaScript için Data Lake istemci kitaplığı 'nı yüklersiniz.

```javascript
npm install @azure/storage-file-datalake
```

`storage-file-datalake`Bu ifadeyi kod dosyanızın en üstüne yerleştirerek paketi içeri aktarın. 

```javascript
const AzureStorageDataLake = require("@azure/storage-file-datalake");
```

## <a name="connect-to-the-account"></a>Hesaba Bağlan 

Bu makaledeki kod parçacıklarını kullanmak için depolama hesabını temsil eden bir **DataLakeServiceClient** örneği oluşturmanız gerekir. 

### <a name="connect-by-using-an-account-key"></a>Hesap anahtarı kullanarak bağlan

Bu, bir hesaba bağlanmanın en kolay yoludur. 

Bu örnek, bir hesap anahtarı kullanarak bir **DataLakeServiceClient** örneği oluşturur.

```javascript

function GetDataLakeServiceClient(accountName, accountKey) {

  const sharedKeyCredential = 
     new StorageSharedKeyCredential(accountName, accountKey);
  
  const datalakeServiceClient = new DataLakeServiceClient(
      `https://${accountName}.dfs.core.windows.net`, sharedKeyCredential);

  return datalakeServiceClient;             
}      

```
> [!NOTE]
> Bu yetkilendirme yöntemi yalnızca Node.js uygulamalar için geçerlidir. Kodunuzu bir tarayıcıda çalıştırmayı planlıyorsanız, Azure Active Directory (AD) kullanarak yetkilendirme yapabilirsiniz. 

### <a name="connect-by-using-azure-active-directory-ad"></a>Azure Active Directory kullanarak bağlanma (AD)

Uygulamanızın kimliğini Azure AD ile doğrulamak için [Azure Identity Client Library for js](https://www.npmjs.com/package/@azure/identity) ' i kullanabilirsiniz.

Bu örnek, bir istemci KIMLIĞI, bir istemci parolası ve bir kiracı KIMLIĞI kullanarak bir **DataLakeServiceClient** örneği oluşturur.  Bu değerleri almak için bkz. [bir istemci uygulamasından istekleri yetkilendirmek Için Azure AD 'den belirteç alma](../common/storage-auth-aad-app.md).

```javascript
function GetDataLakeServiceClientAD(accountName, clientID, clientSecret, tenantID) {

  const credential = new ClientSecretCredential(tenantID, clientID, clientSecret);
  
  const datalakeServiceClient = new DataLakeServiceClient(
      `https://${accountName}.dfs.core.windows.net`, credential);

  return datalakeServiceClient;             
}
```

> [!NOTE]
> Daha fazla örnek için bkz. [js Için Azure kimlik istemci kitaplığı](https://www.npmjs.com/package/@azure/identity) belgeleri.

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bir kapsayıcı dosyalarınız için bir dosya sistemi görevi görür. Bir **filesystemclient** örneği alarak ve sonra **Filesystemclient. Create** metodunu çağırarak bir tane oluşturabilirsiniz.

Bu örnek adlı bir kapsayıcı oluşturur `my-file-system` . 

```javascript
async function CreateFileSystem(datalakeServiceClient) {

  const fileSystemName = "my-file-system";
  
  const fileSystemClient = datalakeServiceClient.getFileSystemClient(fileSystemName);

  const createResponse = await fileSystemClient.create();
        
}
```

## <a name="create-a-directory"></a>Dizin oluşturma

Bir **directoryclient** örneği alarak ve sonra **directoryclient. Create** metodunu çağırarak bir dizin başvurusu oluşturun.

Bu örnek, bir kapsayıcıya adlı bir dizin ekler `my-directory` . 

```javascript
async function CreateDirectory(fileSystemClient) {
   
  const directoryClient = fileSystemClient.getDirectoryClient("my-directory");
  
  await directoryClient.create();

}
```

## <a name="rename-or-move-a-directory"></a>Bir dizini yeniden adlandırma veya taşıma

**Directoryclient. Rename** metodunu çağırarak bir dizini yeniden adlandırın veya taşıyın. İstenen dizinin yolunu bir parametre olarak geçirin. 

Bu örnek, bir alt dizini ada yeniden adlandırır `my-directory-renamed` .

```javascript
async function RenameDirectory(fileSystemClient) {

  const directoryClient = fileSystemClient.getDirectoryClient("my-directory"); 
  await directoryClient.move("my-directory-renamed");

}
```

Bu örnek adlı dizini adlı bir `my-directory-renamed` dizinin alt dizinine taşır `my-directory-2` . 

```javascript
async function MoveDirectory(fileSystemClient) {

  const directoryClient = fileSystemClient.getDirectoryClient("my-directory-renamed"); 
  await directoryClient.move("my-directory-2/my-directory-renamed");      

}
```

## <a name="delete-a-directory"></a>Bir dizini silme

**Directoryclient. Delete** yöntemini çağırarak bir dizini silin.

Bu örnek adlı bir dizini siler `my-directory` .   

```javascript
async function DeleteDirectory(fileSystemClient) {

  const directoryClient = fileSystemClient.getDirectoryClient("my-directory"); 
  await directoryClient.delete();

}
```

## <a name="manage-a-directory-acl"></a>Dizin ACL 'sini yönetme

Bu örnek, adlı bir dizinin ACL 'sini alır ve ayarlar `my-directory` . Bu örnek, sahip olan kullanıcıya okuma, yazma ve yürütme izinlerini verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve diğerlerinin tüm okuma erişimini sağlar.

> [!NOTE]
> Uygulamanız Azure Active Directory (Azure AD) kullanarak erişim yetkisi alıyorsa, uygulamanızın erişim yetkisi vermek için kullandığı güvenlik sorumlusuna [Depolama Blobu veri sahibi rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunların nasıl değiştirileceği hakkında daha fazla bilgi edinmek için [Azure Data Lake Storage 2. erişim denetimi](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)' ne bakın.

```javascript
async function ManageDirectoryACLs(fileSystemClient) {

    const directoryClient = fileSystemClient.getDirectoryClient("my-directory"); 
    const permissions = await directoryClient.getAccessControl();

    console.log(permissions.acl);

    const acl = [
    {
      accessControlType: "user",
      entityId: "",
      defaultScope: false,
      permissions: {
        read: true,
        write: true,
        execute: true
      }
    },
    {
      accessControlType: "group",
      entityId: "",
      defaultScope: false,
      permissions: {
        read: true,
        write: false,
        execute: true
      }
    },
    {
      accessControlType: "other",
      entityId: "",
      defaultScope: false,
      permissions: {
        read: true,
        write: true,
        execute: false
      }

    }

  ];

  await directoryClient.setAccessControl(acl);
}
```

Ayrıca, bir kapsayıcının kök dizininin ACL 'sini de alabilir ve ayarlayabilirsiniz. Kök dizini almak için `/` **Datalakefilesystemclient. getdirectoryclient** metoduna boş bir dize () geçirin.

## <a name="upload-a-file-to-a-directory"></a>Dizine dosya yükleme

İlk olarak bir dosya okuyun. Bu örnek Node.js modülünü kullanır `fs` . Ardından, bir **fileclient** örneği oluşturup, sonra **fileclient. Create** metodunu çağırarak hedef dizinde bir dosya başvurusu oluşturun. **Fileclient. Append** metodunu çağırarak bir dosyayı karşıya yükleyin. **Fileclient. Flush** yöntemini çağırarak karşıya yüklemeyi tamamladığınızdan emin olun.

Bu örnek, bir metin dosyasını adlı bir dizine yükler `my-directory` . '

```javascript
async function UploadFile(fileSystemClient) {

  const fs = require('fs') 

  var content = "";
  
  fs.readFile('mytestfile.txt', (err, data) => { 
      if (err) throw err; 

      content = data.toString();

  }) 
  
  const fileClient = fileSystemClient.getFileClient("my-directory/uploaded-file.txt");
  await fileClient.create();
  await fileClient.append(content, 0, content.length);
  await fileClient.flush(content.length);

}
```

## <a name="manage-a-file-acl"></a>Dosya ACL 'sini yönetme

Bu örnek, adlı bir dosyanın ACL 'sini alır ve ayarlar `upload-file.txt` . Bu örnek, sahip olan kullanıcıya okuma, yazma ve yürütme izinlerini verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve diğerlerinin tüm okuma erişimini sağlar.

> [!NOTE]
> Uygulamanız Azure Active Directory (Azure AD) kullanarak erişim yetkisi alıyorsa, uygulamanızın erişim yetkisi vermek için kullandığı güvenlik sorumlusuna [Depolama Blobu veri sahibi rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunların nasıl değiştirileceği hakkında daha fazla bilgi edinmek için [Azure Data Lake Storage 2. erişim denetimi](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)' ne bakın.

```javascript
async function ManageFileACLs(fileSystemClient) {

  const fileClient = fileSystemClient.getFileClient("my-directory/uploaded-file.txt"); 
  const permissions = await fileClient.getAccessControl();

  console.log(permissions.acl);

  const acl = [
  {
    accessControlType: "user",
    entityId: "",
    defaultScope: false,
    permissions: {
      read: true,
      write: true,
      execute: true
    }
  },
  {
    accessControlType: "group",
    entityId: "",
    defaultScope: false,
    permissions: {
      read: true,
      write: false,
      execute: true
    }
  },
  {
    accessControlType: "other",
    entityId: "",
    defaultScope: false,
    permissions: {
      read: true,
      write: true,
      execute: false
    }

  }

];

await fileClient.setAccessControl(acl);        
}
```

## <a name="download-from-a-directory"></a>Bir dizinden indir

İlk olarak, indirmek istediğiniz dosyayı temsil eden bir **Filesystemclient** örneği oluşturun. Dosyayı okumak için **Filesystemclient. Read** metodunu kullanın. Sonra dosyayı yazın. Bu örnek `fs` bunu yapmak için Node.js modülünü kullanır. 

> [!NOTE]
> Dosya indirme yöntemi yalnızca Node.js uygulamalar için geçerlidir. Kodunuzu bir tarayıcıda çalıştırmayı planlıyorsanız, bunu bir tarayıcıda nasıl yapacağınızı gösteren bir örnek için bkz. [JavaScript için istemci kitaplığı Data Lake Azure Storage dosyası](https://www.npmjs.com/package/@azure/storage-file-datalake) . 

```javascript
async function DownloadFile(fileSystemClient) {

  const fileClient = fileSystemClient.getFileClient("my-directory/uploaded-file.txt");

  const downloadResponse = await fileClient.read();

  const downloaded = await streamToString(downloadResponse.readableStreamBody);
 
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
  
  const fs = require('fs');

  fs.writeFile('mytestfiledownloaded.txt', downloaded, (err) => {
    if (err) throw err;
  });
}

```

## <a name="list-directory-contents"></a>Dizin içeriğini listeleme

Bu örnek, adlı bir dizinde bulunan her bir dizin ve dosyanın adlarını yazdırır `my-directory` .

```javascript
async function ListFilesInDirectory(fileSystemClient) {
  
  let i = 1;

  let iter = await fileSystemClient.listPaths({path: "my-directory", recursive: true});

  for await (const path of iter) {
    
    console.log(`Path ${i++}: ${path.name}, is directory: ${path.isDirectory}`);
  }

}
```

## <a name="see-also"></a>Ayrıca bkz.

* [Paket (düğüm Paket Yöneticisi)](https://www.npmjs.com/package/@azure/storage-file-datalake)
* [Örnekler](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-file-datalake/samples)
* [Geri bildirimde bulunun](https://github.com/Azure/azure-sdk-for-java/issues)