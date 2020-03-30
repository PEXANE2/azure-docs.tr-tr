---
title: Azure Veri Gölü Depolama Gen2'deki dosyalar & AÇ'lar için JavaScript'i kullanma
description: Hiyerarşik ad alanı (HNS) etkinleştirilmiş depolama hesaplarında dizinleri ve dosya ve dizin erişim denetim listelerini (ACL) yönetmek için JavaScript için Azure Depolama Veri Gölü istemci kitaplığını kullanın.
author: normesta
ms.service: storage
ms.date: 03/20/2020
ms.author: normesta
ms.topic: conceptual
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.openlocfilehash: 04d0d23bdbdaeda6a4823c900badb3133ba9eeae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061549"
---
# <a name="use-javascript-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2"></a>Azure Veri Gölü Depolama Gen2'deki dizinleri, dosyaları ve ACD'leri yönetmek için JavaScript'i kullanın

Bu makalede, hiyerarşik ad alanı (HNS) etkin leştirilmiş depolama hesaplarında dizinler, dosyalar ve izinler oluşturmak ve yönetmek için JavaScript'in nasıl kullanılacağı gösterilmektedir. 

[Paket (Düğüm Paket Yöneticisi)](https://www.npmjs.com/package/@azure/storage-file-datalake) | [Örnekleri](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-file-datalake/samples) | [Geri Bildirimde](https://github.com/Azure/azure-sdk-for-java/issues)

## <a name="prerequisites"></a>Ön koşullar

> [!div class="checklist"]
> * Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
> * Hiyerarşik ad alanı (HNS) etkinleştirilmiş bir depolama hesabı. Oluşturmak için [bu](data-lake-storage-quickstart-create-account.md) yönergeleri izleyin.
> * Bu paketi bir Düğüm.js uygulamasında kullanıyorsanız, Node.js 8.0.0 veya daha yüksek bir

## <a name="set-up-your-project"></a>Projenizi ayarlama

Bir terminal penceresi açarak ve ardından aşağıdaki komutu yazarak JavaScript için Veri Gölü istemci kitaplığını yükleyin.

```javascript
npm install @azure/storage-file-datalake
```

Bu `storage-file-datalake` deyimi kod dosyanızın en üstüne yerleştirerek paketi içeri aktarın. 

```javascript
const AzureStorageDataLake = require("@azure/storage-file-datalake");
```

## <a name="connect-to-the-account"></a>Hesaba bağlanın 

Bu makaledeki parçacıkları kullanmak için depolama hesabını temsil eden bir **DataLakeServiceClient** örneği oluşturmanız gerekir. 

### <a name="connect-by-using-an-account-key"></a>Hesap anahtarı nı kullanarak bağlanma

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
> Bu yetkilendirme yöntemi yalnızca Node.js uygulamaları için çalışır. Kodunuzu bir tarayıcıda çalıştırmayı planlıyorsanız, Azure Active Directory (AD) kullanarak yetki verebilirsiniz. 

### <a name="connect-by-using-azure-active-directory-ad"></a>Azure Etkin Dizin (AD) kullanarak bağlanın

Uygulamanızın kimliğini Azure AD ile doğrulamak [için JS için Azure kimlik istemcisi kitaplığını](https://www.npmjs.com/package/@azure/identity) kullanabilirsiniz.

Bu örnek, bir istemci kimliği, istemci sırrı ve kiracı kimliği kullanarak bir **DataLakeServiceClient** örneği oluşturur.  Bu değerleri almak için, [istemci uygulamasından gelen istekleri yetkilendirmek için Azure AD'den bir belirteç edinin'e](../common/storage-auth-aad-app.md)bakın.

```javascript
function GetDataLakeServiceClientAD(accountName, clientID, clientSecret, tenantID) {

  const credential = new ClientSecretCredential(tenantID, clientID, clientSecret);
  
  const datalakeServiceClient = new DataLakeServiceClient(
      `https://${accountName}.dfs.core.windows.net`, credential);

  return datalakeServiceClient;             
}
```

> [!NOTE]
> Daha fazla örnek için, JS belgeleri [için Azure kimlik istemcisi kitaplığına](https://www.npmjs.com/package/@azure/identity) bakın.

## <a name="create-a-file-system"></a>Dosya sistemi oluşturma

Dosya sistemi, dosyalarınız için bir kapsayıcı görevi görür. **Bir FileSystemClient** örneği alarak ve sonra **FileSystemClient.Create** yöntemini arayarak bir tane oluşturabilirsiniz.

Bu örnek, .. `my-file-system` 

```javascript
async function CreateFileSystem(datalakeServiceClient) {

  const fileSystemName = "my-file-system";
  
  const fileSystemClient = datalakeServiceClient.getFileSystemClient(fileSystemName);

  const createResponse = await fileSystemClient.create();
        
}
```

## <a name="create-a-directory"></a>Dizin oluşturma

**Dizin Istemcisi** örneği alarak ve ardından **DirectoryClient.create** yöntemini arayarak bir dizin başvurusu oluşturun.

Bu örnek, dosya `my-directory` sistemine adlandırılmış bir dizin ekler. 

```javascript
async function CreateDirectory(fileSystemClient) {
   
  const directoryClient = fileSystemClient.getDirectoryClient("my-directory");
  
  await directoryClient.create();

}
```

## <a name="rename-or-move-a-directory"></a>Bir dizini yeniden adlandırma veya taşıma

**Dizin Istemcisi.yeniden adlandırma** yöntemini arayarak bir dizini yeniden adlandırın veya taşıyın. İstenilen dizin bir parametre yolunu geçirin. 

Bu örnek, bir alt dizinin adını `my-directory-renamed`yeniden adlandırır.

```javascript
async function RenameDirectory(fileSystemClient) {

  const directoryClient = fileSystemClient.getDirectoryClient("my-directory"); 
  await directoryClient.move("my-directory-renamed");

}
```

Bu örnek, adlı `my-directory-renamed` `my-directory-2`bir dizinin alt dizinine adlı bir dizin taşır. 

```javascript
async function MoveDirectory(fileSystemClient) {

  const directoryClient = fileSystemClient.getDirectoryClient("my-directory-renamed"); 
  await directoryClient.move("my-directory-2/my-directory-renamed");      

}
```

## <a name="delete-a-directory"></a>Bir dizini silme

**DizinIstemci.delete** yöntemini arayarak bir dizini silin.

Bu örnek, adlı `my-directory`bir dizini siler.   

```javascript
async function DeleteDirectory(fileSystemClient) {

  const directoryClient = fileSystemClient.getDirectoryClient("my-directory"); 
  await directoryClient.delete();

}
```

## <a name="manage-a-directory-acl"></a>Bir dizin ACL yönetme

Bu örnek alır ve sonra adlı `my-directory`bir dizinin ACL ayarlar. Bu örnek, sahibi kullanıcıya okuma, yazma ve izinleri yürütme izni verir, sahip grubuna yalnızca okuma ve yürütme izinleri verir ve tüm diğerlerine okuma erişimi sağlar.

> [!NOTE]
> Uygulamanız Azure Etkin Dizini 'ni (Azure AD) kullanarak erişime izin veriyorsa, uygulamanızın erişimi yetkilendirmek için kullandığı güvenlik ilkesine [Depolama Blob Veri Sahibi rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunları değiştirmenin etkileri hakkında daha fazla bilgi edinmek için [Azure Veri Gölü Depolama Gen2'de Erişim denetimine](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)bakın.

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

## <a name="upload-a-file-to-a-directory"></a>Dosyayı dizine yükleme

İlk olarak, bir dosya okuyun. Bu örnek, Düğüm.js `fs` modüllerini kullanır. Ardından, FileClient örneğini oluşturarak ve ardından **FileClient.create** yöntemini arayarak hedef dizinde bir dosya başvurusu **oluşturun.** **FileClient.append** yöntemini arayarak dosya yükleyin. **FileClient.flush** yöntemini arayarak yüklemeyi tamamladığından emin olun.

Bu örnek, bir metin dosyasını `my-directory`adlı bir dizine yükler.

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

## <a name="manage-a-file-acl"></a>Dosyayı ACL yönetme

Bu örnek alır ve sonra adlı `upload-file.txt`bir dosyanın ACL ayarlar. Bu örnek, sahibi kullanıcıya okuma, yazma ve izinleri yürütme izni verir, sahip grubuna yalnızca okuma ve yürütme izinleri verir ve tüm diğerlerine okuma erişimi sağlar.

> [!NOTE]
> Uygulamanız Azure Etkin Dizini 'ni (Azure AD) kullanarak erişime izin veriyorsa, uygulamanızın erişimi yetkilendirmek için kullandığı güvenlik ilkesine [Depolama Blob Veri Sahibi rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunları değiştirmenin etkileri hakkında daha fazla bilgi edinmek için [Azure Veri Gölü Depolama Gen2'de Erişim denetimine](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)bakın.

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

## <a name="download-from-a-directory"></a>Bir dizinden indirin

İlk olarak, indirmek istediğiniz dosyayı temsil eden bir **FileSystemClient** örneği oluşturun. Dosyayı okumak için **FileSystemClient.read** yöntemini kullanın. O zaman dosyayı yaz. Bu örnek, bunu yapmak `fs` için Düğüm.js modüllerini kullanır. 

> [!NOTE]
> Bu dosyayı indirme yöntemi yalnızca Node.js uygulamaları için çalışır. Kodunuzu bir tarayıcıda çalıştırmayı planlıyorsanız, bunu bir tarayıcıda nasıl yapacağınıza bir örnek olarak JavaScript okuma dosyası [için Azure Depolama Dosyası Veri Gölü istemci kitaplığına](https://www.npmjs.com/package/@azure/storage-file-datalake) bakın. 

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

Bu örnek, adlı `my-directory`bir dizinde bulunan her dizin ve dosyanın adlarını yazdırır.

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

* [Paket (Düğüm Paket Yöneticisi)](https://www.npmjs.com/package/@azure/storage-file-datalake)
* [Örnekler](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-file-datalake/samples)
* [Geri bildirimde](https://github.com/Azure/azure-sdk-for-java/issues)