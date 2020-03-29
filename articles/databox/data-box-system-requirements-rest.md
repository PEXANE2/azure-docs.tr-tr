---
title: Microsoft Azure Veri Kutusu Blob depolama gereksinimleri| Microsoft Dokümanlar
description: Azure Veri Kutusu Blob depolama alanı için API'ler, SDK'lar ve istemci kitaplıkları için desteklenen sürümler hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 04/19/2019
ms.author: alkohli
ms.openlocfilehash: 71e0ebf7d7851ae65a6fba67a1695d755fd98bb1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61436503"
---
# <a name="azure-data-box-blob-storage-requirements"></a>Azure Veri Kutusu Blob depolama gereksinimleri

Bu makalede, Azure API'lerinin sürümleri, Azure istemci kitaplıkları ve Veri Kutusu Blob depolama alanıyla desteklenen araçlar listelenir. Veri Kutusu Blob depolama, Azure tutarlı semantikile blob yönetimi işlevselliği sağlar. Bu makalede, Azure Depolama hizmetlerinden bilinen Azure Veri Kutusu Blob depolama farklılıkları da özetlenmiştir.

Veri Kutusu Blob depolama alanına bağlanmadan önce bilgileri dikkatle incelemenizi ve sonra gerekli olduğu şekilde geri başvurmanızı öneririz.


## <a name="storage-differences"></a>Depolama farklılıkları

|     Özellik                                             |     Azure Storage                                     |     Data Box Blob depolama alanı |
|---------------------------------------------------------|-------------------------------------------------------|---------------------------|
|    Azure Dosya Depolama                                   |    Bulut tabanlı Kobİ dosya paylaşımları desteklendi              |    Desteklenmiyor      |
|    Rest'te veriler için hizmet şifrelemesi                  |    256 bit AES şifreleme                             |    256 bit AES şifreleme |
|    Depolama hesabı türü                                 |    Genel amaçlı ve Azure blob depolama hesapları    |    Yalnızca genel amaçlı v1|
|    Blob adı                                            |    1.024 karakter (2.048 bayt)                     |    880 karakter (1.760 bayt)|
|    Blob maksimum boyutu blok                              |    4.75 TB (100 MB X 50.000 blok)                   |    Azure Veri Kutusu v 1,8 için 4,75 TB (100 MB x 50.000 blok).|
|    Sayfa blob maksimum boyutu                               |    8 TB                                               |    1 TB                   |
|    Sayfa blob sayfa boyutu                                  |    512 bayt                                          |    4 KB                   |

## <a name="supported-api-versions"></a>Desteklenen API sürümleri

Azure Depolama hizmeti API'lerinin aşağıdaki sürümleri Veri Kutusu Blob depolama alanıyla desteklenir:

Azure Veri Kutusu 1.8'den itibaren

- [2017-11-09](/rest/api/storageservices/version-2017-11-09)
- [2017-07-29](/rest/api/storageservices/version-2017-07-29)
- [04.17.2017](/rest/api/storageservices/version-2017-04-17)
- [2016-05-31](/rest/api/storageservices/version-2016-05-31)
- [2015-12-11](/rest/api/storageservices/version-2015-12-11)
- [2015-07-08](/rest/api/storageservices/version-2015-07-08)
- [2015-04-05](/rest/api/storageservices/version-2015-04-05)|
## <a name="supported-azure-client-libraries"></a>Desteklenen Azure istemci kitaplıkları

Veri Kutusu Blob depolama için, belirli istemci kitaplıkları ve belirli bitiş noktası sonek gereksinimleri vardır. Veri Kutusu Blob depolama bitiş noktaları, Azure Blob Depolama REST API'nın en son sürümüyle tam eşitliğe sahip değildir, [Azure Veri Kutusu 1.8'in desteklenen sürümlerine](#supported-api-versions)bakın. Depolama istemcisi kitaplıkları için, REST API ile uyumlu sürümün farkında olmanız gerekir.

### <a name="azure-data-box-18-onwards"></a>Azure Veri Kutusu 1.8'den itibaren

| İstemci kitaplığı     |Veri Kutusu Blob depolama destekli sürümü     | Bağlantı   |     Bitiş noktası belirtimi      |
|--------------------|--------------------------------------------|--------|---------------------------------|
|    .NET                |    9.2.0                                           |    Nuget paketi:https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0    <br>GitHub sürümü:https://github.com/Azure/azure-storage-net/releases/tag/v9.2.0                                                                                                                                                                                               |    app.config dosyası                 |
|    Java                |    7.0.0                                           |    Maven paketi:https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/6.1.0   <br>GitHub sürümü:https://github.com/Azure/azure-storage-java/releases/tag/v7.0.0                                                                                                                                                                              |    Bağlantı dizesi kurulumu         |
|    Node.js             |    2.8.3                                           |    NPM bağlantısı: https://www.npmjs.com/package/azure-storage (Çalıştır: `npm install azure-storage@2.7.0`)   <br>GitHub sürümü:https://github.com/Azure/azure-storage-node/releases/tag/v2.8.3                                                                                                                                                                        |    Hizmet örneği bildirimi    |
|    C++                 |    5.2.0                                           |    Nuget paketi:https://www.nuget.org/packages/wastorage.v140/5.2.0   <br>GitHub sürümü:https://github.com/Azure/azure-storage-cpp/releases/tag/v5.2.0                                                                                                                                                                                                     |    Bağlantı dizesi kurulumu         |
|    PHP                 |    1.2.0                                           |    GitHub sürümü:<br>Ortak:https://github.com/Azure/azure-storage-php/releases/tag/v1.2.0-common   <br>Blob:https://github.com/Azure/azure-storage-php/releases/tag/v1.2.0-blob      <br>Besteci aracılığıyla yükleyin (Daha fazla bilgi için aşağıdaki ayrıntılara bakın.)                                                                                                             |    Bağlantı dizesi kurulumu         |
|    Python              |    1.1.0                                           |    GitHub sürümü:<br>Ortak:https://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-common <br>Blob:https://github.com/Azure/azure-storage-python/releases/tag/v1.1.0-blob                                                                                                                                                                          |    Hizmet örneği bildirimi    |
|    Ruby                |    1.0.1                                           |    RubyGems paketi:<br>Ortak:https://rubygems.org/gems/azure-storage-common/versions/1.0.1   <br>Blob:https://rubygems.org/gems/azure-storage-blob/versions/1.0.1         <br>GitHub sürümü:<br>Ortak:https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-common   <br>Blob:https://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-blob          |    Bağlantı dizesi kurulumu         |



### <a name="install-php-client-via-composer---current"></a>Composer üzerinden PHP istemcisi yükleyin - güncel

Besteci üzerinden yüklemek için: (örnek olarak blob almak).
1. Projenin kökünde composer.json adlı bir dosya oluşturun:

    ```
    {
    "require": {
    "Microsoft/azure-storage-blob":"1.2.0"
    }
    ```

2. Proje `composer.phar` köküne indirin.

3. Çalıştırın: php composer.phar yükleyin.

### <a name="endpoint-declaration"></a>Bitiş noktası bildirimi

Azure Veri Kutusu Blob depolama bitiş noktası iki bölümden oluşur: bir bölgenin adı ve Veri Kutusu etki alanı. Veri Kutusu Blob depolama SDK, varsayılan `\<serial no. of the device>.microsoftdatabox.com`bitiş noktası.  Blob hizmeti bitiş noktası hakkında daha fazla bilgi için [Veri Kutusu Blob depolama yoluyla Bağlan'a](data-box-deploy-copy-data-via-rest.md)gidin.
 
## <a name="examples"></a>Örnekler

### <a name="net"></a>.NET

Veri Kutusu Blob depolama için, son nokta sonek `app.config` dosyada belirtilir:

```
<add key="StorageConnectionString"
value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;
EndpointSuffix=<<serial no. of the device>.microsoftdatabox.com  />
```

### <a name="java"></a>Java

Veri Kutusu Blob depolama için, bağlantı dizesinin kurulumunda uç nokta sonek belirtilir:

```
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key;" +
    "EndpointSuffix=<serial no. of the device>.microsoftdatabox.com ";
```

### <a name="nodejs"></a>Node.js

Veri Kutusu Blob depolama için, son nokta sonek bildirim örneğinde belirtilir:

```
var blobSvc = azure.createBlobService('myaccount', 'mykey',
'myaccount.blob. <serial no. of the device>.microsoftdatabox.com ');
```

### <a name="c"></a>C++

Veri Kutusu Blob depolama için, bağlantı dizesinin kurulumunda uç nokta sonek belirtilir:

```
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;
AccountName=your_storage_account;
AccountKey=your_storage_account_key;
EndpointSuffix=<serial no. of the device>.microsoftdatabox.com "));
```

### <a name="php"></a>PHP

Veri Kutusu Blob depolama için, bağlantı dizesinin kurulumunda uç nokta sonek belirtilir:

```
$connectionString = 'BlobEndpoint=http://<storage account name>.blob.<serial no. of the device>.microsoftdatabox.com /;
AccountName=<storage account name>;AccountKey=<storage account key>'
```

### <a name="python"></a>Python

Veri Kutusu Blob depolama için, son nokta sonek bildirim örneğinde belirtilir:

```
block_blob_service = BlockBlobService(account_name='myaccount',
account_key='mykey',
endpoint_suffix=’<serial no. of the device>.microsoftdatabox.com’)
```

### <a name="ruby"></a>Ruby

Veri Kutusu Blob depolama için, bağlantı dizesinin kurulumunda uç nokta sonek belirtilir:

```
set
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;
AccountName=myaccount;
AccountKey=mykey;
EndpointSuffix=<serial no. of the device>.microsoftdatabox.com
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Kutunuzu Dağıtma](data-box-deploy-ordered.md)
