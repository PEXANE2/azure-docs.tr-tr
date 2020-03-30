---
title: REST API ile Azure dosya paylaşımlarını geri yükleme
description: Azure Yedekleme tarafından oluşturulan bir geri yükleme noktasından Azure dosya paylaşımlarını veya belirli dosyaları geri yüklemek için REST API'yi nasıl kullanacağınızı öğrenin
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 1c3160491ef92c62745af1468556e7d5c30437fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252512"
---
# <a name="restore-azure-file-shares-using-rest-api"></a>REST API'yi kullanarak Azure Dosya Paylaşımlarını Geri Yükleme

Bu makalede, REST API'sini kullanarak [Azure Yedekleme](https://docs.microsoft.com/azure/backup/backup-overview) tarafından oluşturulan bir geri yükleme noktasından tüm dosya paylaşımının veya belirli dosyaların nasıl geri yüklenilen geri yüklendiği açıklanmaktadır.

Bu makalenin sonunda, REST API'yi kullanarak aşağıdaki işlemleri nasıl gerçekleştireceğinizi öğreneceksiniz:

* Yedeklenmiş bir Azure dosya paylaşımı için geri yükleme noktalarını görüntüleyin.
* Tam bir Azure dosya paylaşımını geri yükleyin.
* Tek tek dosyaları veya klasörleri geri yükleyin.

## <a name="prerequisites"></a>Ön koşullar

Geri yüklemek istediğiniz yedeklenmiş bir dosya paylaşımınız olduğunu varsayıyoruz. Bunu yapmazsanız, nasıl oluşturulabileceğinizi öğrenmek için [REST API'yi kullanarak Yedekleme Azure dosya paylaşımını](backup-azure-file-share-rest-api.md) denetleyin.

Bu makale için aşağıdaki kaynakları kullanacağız:

* **RecoveryServicesVault**: *azurefilesvault*
* **Kaynak grubu**: *azurefiles*
* **Depolama Hesabı**: *afsaccount*
* **Dosya Paylaş**: *azurefiles*

## <a name="fetch-containername-and-protecteditemname"></a>ContainerName ve ProtectedItemName getir

Geri yükleme ile ilgili API çağrılarının çoğunda{containerName} ve {protectedItemName} URI parametreleri için değerleri geçmeniz gerekir. Bu parametreleriçin değerleri almak için [GET backupprotectableitems](https://docs.microsoft.com/rest/api/backup/protecteditems/get) işleminin yanıt gövdesindeki kimlik özniteliğini kullanın. Örneğimizde, korumak istediğimiz dosya paylaşımının kimliği:

`"/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/storagecontainer;storage;azurefiles;afsaccount/protectableItems/azurefileshare;azurefiles`

Yani değerler aşağıdaki gibi çevirir:

* {containername} - *storagecontainer;storage;azurefiles;afsaccount*
* {protectedItemName} - *azurefileshare;azurefiles*

## <a name="fetch-recovery-points-for-backed-up-azure-file-share"></a>Yedeklenen Azure dosya paylaşımı için kurtarma noktaları getir

Yedeklenmiş dosya paylaşımını veya dosyalarını geri yüklemek için önce geri yükleme işlemini gerçekleştirmek için bir kurtarma noktası seçin. Yedeklenmiş bir öğenin kullanılabilir kurtarma [noktaları, Kurtarma Noktası Listesi](https://docs.microsoft.com/rest/api/site-recovery/recoverypoints/listbyreplicationprotecteditems) REST API çağrısı kullanılarak listelenebilir. Tüm ilgili değerlere sahip bir GET operasyonu.

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints?api-version=2019-05-13&$filter={$filter}
```

URI değerlerini aşağıdaki gibi ayarlayın:

* {fabricName}: *Azure*
* {vaultName}: *azurefilesvault*
* {containername}: *storagecontainer;storage;azurefiles;afsaccount*
* {protectedItemName}: *azurefileshare;azurefiles*
* {ResourceGroupName}: *azurefiles*

GET URI gerekli tüm parametrelere sahiptir. Ek bir istek organına gerek yok.

```http
GET https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;afsaccount/protectedItems/AzureFileShare;azurefiles/recoveryPoints?api-version=2019-05-13
```

### <a name="example-response"></a>Örnek yanıt

GET URI gönderildikten sonra, 200 yanıt döndürülür:

```http
HTTP/1.1" 200 None
'Cache-Control': 'no-cache'
'Pragma': 'no-cache'
'Transfer-Encoding': 'chunked'
'Content-Type': 'application/json'
'Content-Encoding': 'gzip'
'Expires': '-1'
'Vary': 'Accept-Encoding'
'X-Content-Type-Options': 'nosniff'
'x-ms-request-id': 'd68d7951-7d97-4c49-9a2d-7fbaab55233a'
'x-ms-client-request-id': '4edb5a58-47ea-11ea-a27a-0a580af41908, 4edb5a58-47ea-11ea-a27a-0a580af41908'
'Strict-Transport-Security': 'max-age=31536000; includeSubDomains'
'Server': 'Microsoft-IIS/10.0'
'X-Powered-By': 'ASP.NET'
'x-ms-ratelimit-remaining-subscription-reads': '11998'
'x-ms-correlation-request-id': 'd68d7951-7d97-4c49-9a2d-7fbaab55233a'
'x-ms-routing-request-id': 'WESTEUROPE:20200205T073708Z:d68d7951-7d97-4c49-9a2d-7fbaab55233a'
'Date': 'Wed, 05 Feb 2020 07:37:08 GMT'
{
“value”:[
  {
    "eTag": null,
    "id": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;afsaccount/protectedItems/AzureFileShare;azurefiles/recoveryPoints/932881138555802864",
    "location": null,
    "name": "932881138555802864",
    "properties": {
      "fileShareSnapshotUri": "https://afsaccount.file.core.windows.net/azurefiles?sharesnapshot=2020-02-04T08:01:35.0000000Z",
      "objectType": "AzureFileShareRecoveryPoint",
      "recoveryPointSizeInGb": 1,
      "recoveryPointTime": "2020-02-04T08:01:35+00:00",
      "recoveryPointType": "FileSystemConsistent"
    },
    "resourceGroup": "azurefiles",
    "tags": null,
    "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints"
  },
  {
    "eTag": null,
    "id": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;afsaccount/protectedItems/AzureFileShare;azurefiles/recoveryPoints/932878582606969225",
    "location": null,
    "name": "932878582606969225",
    "properties": {
      "fileShareSnapshotUri": "https://afsaccount.file.core.windows.net/azurefiles?sharesnapshot=2020-02-03T08:05:30.0000000Z",
      "objectType": "AzureFileShareRecoveryPoint",
      "recoveryPointSizeInGb": 1,
      "recoveryPointTime": "2020-02-03T08:05:30+00:00",
      "recoveryPointType": "FileSystemConsistent"
    },
    "resourceGroup": "azurefiles",
    "tags": null,
    "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints"
  },
  {
    "eTag": null,
    "id": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;afsaccount/protectedItems/AzureFileShare;azurefiles/recoveryPoints/932890167574511261",
    "location": null,
    "name": "932890167574511261",
    "properties": {
      "fileShareSnapshotUri": "https://afsaccount.file.core.windows.net/azurefiles?sharesnapshot=2020-02-02T08:03:50.0000000Z",
      "objectType": "AzureFileShareRecoveryPoint",
      "recoveryPointSizeInGb": 1,
      "recoveryPointTime": "2020-02-02T08:03:50+00:00",
      "recoveryPointType": "FileSystemConsistent"
    },
    "resourceGroup": "azurefiles",
    "tags": null,
    "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints"
  },
```

Kurtarma noktası yukarıdaki yanıtta {name} alanı ile tanımlanır.

## <a name="full-share-recovery-using-rest-api"></a>REST API'yi kullanarak tam hisse kurtarma

Orijinal veya alternatif bir konumda tam dosya paylaşımını geri yüklemek için bu geri yükleme seçeneğini kullanın.
Geri yükleme tetikleme bir POST isteği ve [tetikleyici geri yükleme](https://docs.microsoft.com/rest/api/backup/restores/trigger) REST API kullanarak bu işlemi gerçekleştirebilirsiniz.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints/{recoveryPointId}/restore?api-version=2019-05-13
```

{containerName} ve {protectedItemName} değerleri [burada](#fetch-containername-and-protecteditemname) ayarlanan gibidir ve recoveryPointID yukarıda belirtilen kurtarma noktasının {name} alanıdır.

```http
POST https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;afsaccount/protectedItems/AzureFileShare%3Bazurefiles/recoveryPoints/932886657837421071/restore?api-version=2019-05-13'
```

### <a name="create-request-body"></a>İstek gövdesi oluşturma

Azure dosya paylaşımı için geri yüklemeyi tetiklemek için istek gövdesinin bileşenleri şunlardır:

Adı |  Tür   |   Açıklama
--- | ---- | ----
Özellikler | AzureFileShareRestoreRequest | Geri YüklemeRequestResource özellikleri

İstek gövdesinin tanımlarının tam listesi ve diğer ayrıntılar [için, tetikleyici REST API belgesine](https://docs.microsoft.com/rest/api/backup/restores/trigger#request-body)bakın.

### <a name="restore-to-original-location"></a>Özgün konuma geri yükleme

#### <a name="request-body-example"></a>İstek gövdesi örneği

Aşağıdaki istek gövdesi, Azure dosya paylaşımı geri yüklemesini tetiklemek için gereken özellikleri tanımlar:

```json
{
   "properties":{
      "objectType":"AzureFileShareRestoreRequest",
      "recoveryType":"OriginalLocation",
      "sourceResourceId":"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/afsaccount",
      "copyOptions":"Overwrite",
      "restoreRequestType":"FullShareRestore"
}
}
```

### <a name="restore-to-alternate-location"></a>Alternatif konuma geri yükleme

Alternatif konum kurtarma için aşağıdaki parametreleri belirtin:

* **targetResourceId**: Yedeklenen içeriğin geri yüklendiği depolama hesabı. Hedef depolama hesabı kasayla aynı konumda olmalıdır.
* **ad**: Yedeklenen içeriğin geri yüklendiği hedef depolama hesabındaki dosya paylaşımı.
* **targetFolderPath**: Dosya payının altındaki klasör, verilerin geri yüklendiği klasördür.

#### <a name="request-body-example"></a>İstek gövdesi örneği

Aşağıdaki istek *gövdesi, afsaccount* depolama hesabındaki *azurefiles* dosya paylaşımını *afaccount1* depolama hesabındaki *azurefiles1* dosya paylaşımına geri yükler.

```json
{
   "properties":{
      "objectType":"AzureFileShareRestoreRequest",
      "recoveryType":"AlternateLocation",
      "sourceResourceId":"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/afsaccount",
      "copyOptions":"Overwrite",
      "restoreRequestType":"FullShareRestore",
      "restoreFileSpecs":[
         {
            "targetFolderPath":"restoredata"
}
],
      "targetDetails":{
         "name":"azurefiles1",
         "targetResourceId":"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/afaccount1"
}
}
}
```

### <a name="response"></a>Yanıt

Geri yükleme işleminin tetikleme işlemi [bir eşzamanlı işlemdir.](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) Bu işlem, ayrı olarak izlenmesi gereken başka bir işlem oluşturur.
İki yanıt verir: başka bir işlem oluşturulduğunda 202 (Kabul) ve bu işlem tamamlandığında 200 (Tamam).

#### <a name="response-example"></a>Yanıt örneği

*Post* URI'yi geri yüklemeyi tetiklemek için gönderdikten sonra, ilk yanıt 202 (Kabul) ve konum üstbilgisini veya Azure-async-üstbilgisini gösterir.

```http
HTTP/1.1" 202
'Cache-Control': 'no-cache'
'Pragma': 'no-cache'
'Expires': '-1'
'Location': 'https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;afsaccount/protectedItems/AzureFileShare;azurefiles/operationResults/68ccfbc1-a64f-4b29-b955-314b5790cfa9?api-version=2019-05-13'
'Retry-After': '60'
'Azure-AsyncOperation': 'https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;afsaccount/protectedItems/AzureFileShare;azurefiles/operationsStatus/68ccfbc1-a64f-4b29-b955-314b5790cfa9?api-version=2019-05-13'
'X-Content-Type-Options': 'nosniff'
'x-ms-request-id': '2426777d-c5ec-44b6-a324-384f8947460c'
'x-ms-client-request-id': '3c743096-47eb-11ea-ae90-0a580af41908, 3c743096-47eb-11ea-ae90-0a580af41908'
'Strict-Transport-Security': 'max-age=31536000; includeSubDomains'
'X-Powered-By': 'ASP.NET'
'x-ms-ratelimit-remaining-subscription-writes': '1198'
'x-ms-correlation-request-id': '2426777d-c5ec-44b6-a324-384f8947460c'
'x-ms-routing-request-id': 'WESTEUROPE:20200205T074347Z:2426777d-c5ec-44b6-a324-384f8947460c'
'Date': 'Wed, 05 Feb 2020 07:43:47 GMT'
```

Ardından, ortaya çıkan işlemi, GET komutuyla konum üstbilgisini veya Azure-AsyncOperation üstbilgisini kullanarak izleyin.

```http
GET https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupOperations/68ccfbc1-a64f-4b29-b955-314b5790cfa9?api-version=2016-12-01
```

İşlem tamamlandıktan sonra, yanıt gövdesinde ortaya çıkan geri yükleme işinin kimliğiyle birlikte 200 (Tamam) döndürür.

```http
HTTP/1.1" 200
'Cache-Control': 'no-cache'
'Pragma': 'no-cache'
'Transfer-Encoding': 'chunked'
'Content-Type': 'application/json'
'Content-Encoding': 'gzip'
'Expires': '-1'
'Vary': 'Accept-Encoding'
'X-Content-Type-Options': 'nosniff'
'x-ms-request-id': '41ee89b2-3be4-40d8-8ff6-f5592c2571e3'
'x-ms-client-request-id': '3c743096-47eb-11ea-ae90-0a580af41908, 3c743096-47eb-11ea-ae90-0a580af41908'
'Strict-Transport-Security': 'max-age=31536000; includeSubDomains'
'Server': 'Microsoft-IIS/10.0'
'X-Powered-By': 'ASP.NET'
'x-ms-ratelimit-remaining-subscription-reads': '11998'
'x-ms-correlation-request-id': '41ee89b2-3be4-40d8-8ff6-f5592c2571e3'
'x-ms-routing-request-id': 'WESTEUROPE:20200205T074348Z:41ee89b2-3be4-40d8-8ff6-f5592c2571e3'
'Date': 'Wed, 05 Feb 2020 07:43:47 GMT'
{
  "id": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupJobs/a7e97e42-4e54-4d4b-b449-26fcf946f42c",
  "location": null,
  "name": "a7e97e42-4e54-4d4b-b449-26fcf946f42c",
  "properties": {
    "actionsInfo": [
      "Cancellable"
    ],
    "activityId": "3c743096-47eb-11ea-ae90-0a580af41908",
    "backupManagementType": "AzureStorage",
    "duration": "0:00:01.863098",
    "endTime": null,
    "entityFriendlyName": "azurefiles",
    "errorDetails": null,
    "extendedInfo": {
      "dynamicErrorMessage": null,
      "propertyBag": {},
      "tasksList": []
    },
    "jobType": "AzureStorageJob",
    "operation": "Restore",
    "startTime": "2020-02-05T07:43:47.144961+00:00",
    "status": "InProgress",
    "storageAccountName": "afsaccount",
    "storageAccountVersion": "Storage"
  },
  "resourceGroup": "azurefiles",
  "tags": null,
  "type": "Microsoft.RecoveryServices/vaults/backupJobs"
}
```

Alternatif konum kurtarma için yanıt gövdesi aşağıdaki gibi olacaktır:

```http
{
  "id": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupJobs/7e0ee41e-6e31-4728-a25c-98ff6b777641",
  "location": null,
  "name": "7e0ee41e-6e31-4728-a25c-98ff6b777641",
  "properties": {
    "actionsInfo": [
      "Cancellable"
    ],
    "activityId": "6077be6e-483a-11ea-a915-0a580af4ad72",
    "backupManagementType": "AzureStorage",
    "duration": "0:00:02.171965",
    "endTime": null,
    "entityFriendlyName": "azurefiles",
    "errorDetails": null,
    "extendedInfo": {
      "dynamicErrorMessage": null,
      "propertyBag": {
        "Data Transferred (in MB)": "0",
        "Job Type": "Recover to an alternate file share",
        "Number Of Failed Files": "0",
        "Number Of Restored Files": "0",
        "Number Of Skipped Files": "0",
        "RestoreDestination": "afaccount1/azurefiles1/restoredata",
        "Source File Share Name": "azurefiles",
        "Source Storage Account Name": "afsaccount",
        "Target File Share Name": "azurefiles1",
        "Target Storage Account Name": "afaccount1"
      },
      "tasksList": []
    },
    "jobType": "AzureStorageJob",
    "operation": "Restore",
    "startTime": "2020-02-05T17:10:18.106532+00:00",
    "status": "InProgress",
    "storageAccountName": "afsaccount",
    "storageAccountVersion": "ClassicCompute"
  },
  "resourceGroup": "azurefiles",
  "tags": null,
  "type": "Microsoft.RecoveryServices/vaults/backupJobs"
}
```

Yedekleme işi uzun süren bir işlem olduğundan, [REST API belgesini kullanarak monitör işlerinde](https://docs.microsoft.com/azure/backup/backup-azure-arm-userestapi-managejobs#tracking-the-job)açıklandığı gibi izlenmelidir.

## <a name="item-level-recovery-using-rest-api"></a>REST API'yi kullanarak madde düzeyi kurtarma

Özgün veya alternatif bir konumda tek tek dosyaları veya klasörleri geri yüklemek için bu geri yükleme seçeneğini kullanabilirsiniz.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints/{recoveryPointId}/restore?api-version=2019-05-13
```

{containerName} ve {protectedItemName} değerleri [burada](#fetch-containername-and-protecteditemname) ayarlanan gibidir ve recoveryPointID yukarıda belirtilen kurtarma noktasının {name} alanıdır.

```http
POST https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;afsaccount/protectedItems/AzureFileShare%3Bazurefiles/recoveryPoints/932886657837421071/restore?api-version=2019-05-13'
```

### <a name="create-request-body"></a>İstek gövdesi oluşturma

Azure dosya paylaşımı için geri yüklemeyi tetiklemek için istek gövdesinin bileşenleri şunlardır:

Adı |  Tür   |   Açıklama
--- | ---- | ----
Özellikler | AzureFileShareRestoreRequest | Geri YüklemeRequestResource özellikleri

İstek gövdesinin tanımlarının tam listesi ve diğer ayrıntılar [için, tetikleyici REST API belgesine](https://docs.microsoft.com/rest/api/backup/restores/trigger#request-body)bakın.

### <a name="restore-to-original-location"></a>Özgün konuma geri yükleme

Aşağıdaki istek *gövdesi, afsaccount* depolama *hesabındaki azurefiles* dosya paylaşımındaki *Restoretest.txt* dosyasını geri yüklemektir.

İstek Gövdesi Oluştur

```json
{
   "properties":{
      "objectType":"AzureFileShareRestoreRequest",
      "copyOptions":"Overwrite",
      "recoveryType":"OriginalLocation",
      "restoreFileSpecs":[
         {
            "fileSpecType":"File",
            "path":"RestoreTest.txt",
            "targetFolderPath":null
}
],
      "restoreRequestType":"ItemLevelRestore",
      "sourceResourceId":"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.storage/storageAccounts/afsaccount",
      "targetDetails":null
}
}
```

### <a name="restore-to-alternate-location"></a>Alternatif konuma geri yükleme

Aşağıdaki istek gövdesi, *afsaccount* depolama hesabındaki *azurefiles* dosya paylaşımındaki *Restoretest.txt* dosyasını, *afaccount1* depolama hesabındaki *azurefiles1* dosya paylaşımının *geri yükleme veri* klasörüne geri yüklemektir.

İstek gövdesi oluşturma

```json
{
   "properties":{
      "objectType":"AzureFileShareRestoreRequest",
      "recoveryType":"AlternateLocation",
      "sourceResourceId":"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/afsaccount",
      "copyOptions":"Overwrite",
      "restoreRequestType":"ItemLevelRestore",
      "restoreFileSpecs":[
         {
            "path":"Restore/RestoreTest.txt",
            "fileSpecType":"File",
            "targetFolderPath":"restoredata"
}
],
      "targetDetails":{
         "name":"azurefiles1",
         "targetResourceId":"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/afaccount1"
}
}
}
```

Yanıt [tam hisse geri yüklemeleri](#full-share-recovery-using-rest-api)için yukarıda açıklandığı şekilde ele alınmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* [Rest API'yi kullanarak Azure dosya paylaşımlarını yedeklemeyi nasıl yöneteceğimiz](manage-azure-file-share-rest-api.md)öğrenin.
