---
title: BLOB depolama gereksinimlerini Data Box Microsoft Azure | Microsoft Docs
description: Azure Data Box blob depolaması için API 'Ler, SDK 'Lar ve istemci kitaplıkları için desteklenen sürümler hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 04/19/2019
ms.author: alkohli
ms.openlocfilehash: 71e0ebf7d7851ae65a6fba67a1695d755fd98bb1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "61436503"
---
# <a name="azure-data-box-blob-storage-requirements"></a>Azure Data Box BLOB depolama gereksinimleri

Bu makalede, Azure API 'Leri, Azure istemci kitaplıkları ve Data Box blob depolamayla desteklenen araçların sürümleri listelenir. Data Box BLOB depolama, Azure ile tutarlı semantiklerle blob yönetim işlevleri sağlar. Bu makalede ayrıca Azure depolama hizmetlerinden bilinen Azure Data Box BLOB depolama farklılıkları özetlenmektedir.

Data Box BLOB depolama alanına bağlanmadan önce bilgileri dikkatlice incelemenizi ve sonra gerektiği gibi geri başvurmalarını öneririz.


## <a name="storage-differences"></a>Depolama farklılıkları

|     Özellik                                             |     Azure Depolama                                     |     Data Box Blob depolama alanı |
|---------------------------------------------------------|-------------------------------------------------------|---------------------------|
|    Azure Dosya Depolama                                   |    Bulut tabanlı SMB dosya paylaşımları destekleniyor              |    Desteklenmiyor      |
|    Bekleyen veriler için hizmet şifrelemesi                  |    256 bit AES şifrelemesi                             |    256 bit AES şifrelemesi |
|    Depolama hesabı türü                                 |    Genel amaçlı ve Azure Blob depolama hesapları    |    Yalnızca genel amaçlı v1|
|    Blob adı                                            |    1.024 karakter (2.048 bayt)                     |    880 karakter (1.760 bayt)|
|    Blok Blobu en büyük boyutu                              |    4,75 TB (100 MB X 50.000 blok)                   |    Azure Data Box v 1,8 ve sonraki sürümleri için 4,75 TB (100 MB x 50.000 blok).|
|    Sayfa Blobu en büyük boyut                               |    8 TB                                               |    1 TB                   |
|    Sayfa Blobu sayfa boyutu                                  |    512 bayt                                          |    4 KB                   |

## <a name="supported-api-versions"></a>Desteklenen API sürümleri

Azure Storage Service API 'Lerinin aşağıdaki sürümleri Data Box BLOB depolama ile desteklenir:

Azure Data Box 1,8 sürümleri

- [2017-11-09](/rest/api/storageservices/version-2017-11-09)
- [2017-07-29](/rest/api/storageservices/version-2017-07-29)
- [04.17.2017](/rest/api/storageservices/version-2017-04-17)
- [2016-05-31](/rest/api/storageservices/version-2016-05-31)
- [2015-12-11](/rest/api/storageservices/version-2015-12-11)
- [2015-07-08](/rest/api/storageservices/version-2015-07-08)
- [2015-04-05](/rest/api/storageservices/version-2015-04-05)|
## <a name="supported-azure-client-libraries"></a>Desteklenen Azure istemci kitaplıkları

Data Box BLOB depolama için belirli istemci kitaplıkları ve belirli uç nokta soneki gereksinimleri vardır. Data Box BLOB depolama uç noktaları, Azure Blob depolama REST API en son sürümü ile tam eşlik içermez; [Azure Data Box 1,8 ve sonraki sürümleri için desteklenen sürümlere](#supported-api-versions)bakın. Depolama istemci kitaplıkları için, REST API uyumlu sürümü bilmeniz gerekir.

### <a name="azure-data-box-18-onwards"></a>Azure Data Box 1,8 sürümleri

| İstemci kitaplığı     |Data Box BLOB depolama desteklenen sürümü     | Bağlantı   |     Uç nokta belirtimi      |
|--------------------|--------------------------------------------|--------|---------------------------------|
|    .NET                |    9.2.0                                           |    NuGet paketi:https://www.nuget.org/packages/WindowsAzure.Storage/9.2.0    <br>GitHub sürümü:https://github.com/Azure/azure-storage-net/releases/tag/v9.2.0                                                                                                                                                                                               |    app.config dosyası                 |
|    Java                |    7.0.0                                           |    Maven paketi:https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage/6.1.0   <br>GitHub sürümü:https://github.com/Azure/azure-storage-java/releases/tag/v7.0.0                                                                                                                                                                              |    Bağlantı dizesi kurulumu         |
|    Node.js             |    2.8.3                                           |    NPM bağlantısı: https://www.npmjs.com/package/azure-storage (çalıştırma: `npm install azure-storage@2.7.0` )   <br>GitHub sürümü:https://github.com/Azure/azure-storage-node/releases/tag/v2.8.3                                                                                                                                                                        |    Hizmet örneği bildirimi    |
|    C++                 |    5.2.0                                           |    NuGet paketi:https://www.nuget.org/packages/wastorage.v140/5.2.0   <br>GitHub sürümü:https://github.com/Azure/azure-storage-cpp/releases/tag/v5.2.0                                                                                                                                                                                                     |    Bağlantı dizesi kurulumu         |
|    PHP                 |    1.2.0                                           |    GitHub sürümü:<br>Birçokhttps://github.com/Azure/azure-storage-php/releases/tag/v1.2.0-common   <br>Bunhttps://github.com/Azure/azure-storage-php/releases/tag/v1.2.0-blob      <br>Besteci aracılığıyla Install (daha fazla bilgi Için aşağıdaki ayrıntılara bakın.)                                                                                                             |    Bağlantı dizesi kurulumu         |
|    Python              |    1.1.0                                           |    GitHub sürümü:<br>Birçokhttps://github.com/Azure/azure-storage-python/releases/tag/v1.0.0-common <br>Bunhttps://github.com/Azure/azure-storage-python/releases/tag/v1.1.0-blob                                                                                                                                                                          |    Hizmet örneği bildirimi    |
|    Ruby                |    1.0.1                                           |    RubyGems paketi:<br>Birçokhttps://rubygems.org/gems/azure-storage-common/versions/1.0.1   <br>Bunhttps://rubygems.org/gems/azure-storage-blob/versions/1.0.1         <br>GitHub sürümü:<br>Birçokhttps://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-common   <br>Bunhttps://github.com/Azure/azure-storage-ruby/releases/tag/v1.0.1-blob          |    Bağlantı dizesi kurulumu         |



### <a name="install-php-client-via-composer---current"></a>PHP istemcisini besteci aracılığıyla Install-Current

Besteci aracılığıyla yüklemek için: (blobu örnek olarak al).
1. Aşağıdaki kodla projenin kökünde composer.jsadlı bir dosya oluşturun:

    ```
    {
    "require": {
    "Microsoft/azure-storage-blob":"1.2.0"
    }
    ```

2. `composer.phar`Proje köküne indirin.

3. Şunu çalıştırın: php besteci. phar Install.

### <a name="endpoint-declaration"></a>Uç nokta bildirimi

Azure Data Box BLOB depolama uç noktası iki bölümden oluşur: bir bölgenin adı ve Data Box etki alanı. Data Box blob Storage SDK 'sında varsayılan uç nokta ' dır `\<serial no. of the device>.microsoftdatabox.com` .  Blob hizmeti uç noktası hakkında daha fazla bilgi için [Data Box BLOB depolama aracılığıyla bağlanma](data-box-deploy-copy-data-via-rest.md)bölümüne gidin.
 
## <a name="examples"></a>Örnekler

### <a name="net"></a>.NET

Data Box BLOB depolama için, uç nokta soneki `app.config` dosyada belirtilir:

```
<add key="StorageConnectionString"
value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;
EndpointSuffix=<<serial no. of the device>.microsoftdatabox.com  />
```

### <a name="java"></a>Java

Data Box blob depolaması için, uç nokta soneki bağlantı dizesinin kurulumunda belirtilir:

```
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key;" +
    "EndpointSuffix=<serial no. of the device>.microsoftdatabox.com ";
```

### <a name="nodejs"></a>Node.js

Data Box blob depolaması için, uç nokta soneki bildirim örneğinde belirtilir:

```
var blobSvc = azure.createBlobService('myaccount', 'mykey',
'myaccount.blob. <serial no. of the device>.microsoftdatabox.com ');
```

### <a name="c"></a>C++

Data Box blob depolaması için, uç nokta soneki bağlantı dizesinin kurulumunda belirtilir:

```
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;
AccountName=your_storage_account;
AccountKey=your_storage_account_key;
EndpointSuffix=<serial no. of the device>.microsoftdatabox.com "));
```

### <a name="php"></a>PHP

Data Box blob depolaması için, uç nokta soneki bağlantı dizesinin kurulumunda belirtilir:

```
$connectionString = 'BlobEndpoint=http://<storage account name>.blob.<serial no. of the device>.microsoftdatabox.com /;
AccountName=<storage account name>;AccountKey=<storage account key>'
```

### <a name="python"></a>Python

Data Box blob depolaması için, uç nokta soneki bildirim örneğinde belirtilir:

```
block_blob_service = BlockBlobService(account_name='myaccount',
account_key='mykey',
endpoint_suffix=’<serial no. of the device>.microsoftdatabox.com’)
```

### <a name="ruby"></a>Ruby

Data Box blob depolaması için, uç nokta soneki bağlantı dizesinin kurulumunda belirtilir:

```
set
AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;
AccountName=myaccount;
AccountKey=mykey;
EndpointSuffix=<serial no. of the device>.microsoftdatabox.com
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Box dağıtın](data-box-deploy-ordered.md)
