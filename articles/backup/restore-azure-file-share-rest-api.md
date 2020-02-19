---
title: Azure dosya paylaşımlarını REST API geri yükleme
description: Azure dosya paylaşımlarını veya belirli dosyaları Azure Backup tarafından oluşturulan bir geri yükleme noktasından geri yüklemek için REST API nasıl kullanacağınızı öğrenin
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 1c3160491ef92c62745af1468556e7d5c30437fc
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77444816"
---
# <a name="restore-azure-file-shares-using-rest-api"></a>REST API kullanarak Azure dosya paylaşımlarını geri yükleme

Bu makalede, REST API kullanılarak [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview) tarafından oluşturulan bir geri yükleme noktasından tüm dosya paylaşımının veya belirli dosyaların nasıl geri yükleneceği açıklanmaktadır.

Bu makalenin sonuna kadar, REST API kullanarak aşağıdaki işlemleri gerçekleştirmeyi öğreneceksiniz:

* Yedeklenen bir Azure dosya paylaşımının geri yükleme noktalarını görüntüleyin.
* Tam bir Azure dosya paylaşımının geri yüklenmesi.
* Dosya veya klasörleri tek tek geri yükleyin.

## <a name="prerequisites"></a>Önkoşullar

Daha önce geri yüklemek istediğiniz yedeklenmiş bir dosya paylaşımınız olduğunu varsayalım. Bunu yapmazsanız, bir tane oluşturmayı öğrenmek için [REST API kullanarak yedekleme Azure dosya paylaşımının](backup-azure-file-share-rest-api.md) olup olmadığını denetleyin.

Bu makalede, aşağıdaki kaynakları kullanacağız:

* **Recoveryserviceskasası**: *azurefilesvault*
* **Kaynak grubu**: *azurefiles*
* **Depolama hesabı**: *afsaccount*
* **Dosya paylaşma**: *azurefiles*

## <a name="fetch-containername-and-protecteditemname"></a>ContainerName ve korunabilir Dıtemname öğesini getir

Geri yükleme ile ilgili API çağrılarının çoğu için, {containerName} ve {korunabilir} URI parametreleri için değerleri geçirmeniz gerekir. Bu parametrelerin değerlerini almak için [backupkorunabilir tableıtems alma](https://docs.microsoft.com/rest/api/backup/protecteditems/get) işleminin yanıt gövdesinde ID özniteliğini kullanın. Bizim örneğimizde, korumak istediğimiz dosya paylaşımının KIMLIĞI:

`"/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/storagecontainer;storage;azurefiles;afsaccount/protectableItems/azurefileshare;azurefiles`

Bu nedenle değerler aşağıdaki gibi çeviri yapar:

* {ContainerName}- *storagecontainer; depolama; azurefiles; afsaccount*
* {Korunabilir}- *azurefileshare; azurefiles*

## <a name="fetch-recovery-points-for-backed-up-azure-file-share"></a>Yedeklenen Azure dosya paylaşımının kurtarma noktalarını getir

Yedeklenen dosya paylaşımının veya dosyalarının geri yüklenmesi için önce geri yükleme işlemini gerçekleştirmek üzere bir kurtarma noktası seçin. Yedeklenmiş bir öğenin kullanılabilir kurtarma noktaları, [Kurtarma noktası listesi](https://docs.microsoft.com/rest/api/site-recovery/recoverypoints/listbyreplicationprotecteditems) REST API çağrısı kullanılarak listelenebilir. Bu, tüm ilgili değerleri içeren bir GET işlemidir.

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints?api-version=2019-05-13&$filter={$filter}
```

URI değerlerini şu şekilde ayarlayın:

* {fabricName}: *Azure*
* {vaultName}: *azurefilesvault*
* {ContainerName}: *storagecontainer; depolama; azurefiles; afsaccount*
* {Korunabilir}: *azurefileshare; azurefiles*
* {ResourceGroupName}: *azurefiles*

GET URI 'sinin tüm gerekli parametreleri vardır. Ek bir istek gövdesi gerekmez.

```http
GET https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;afsaccount/protectedItems/AzureFileShare;azurefiles/recoveryPoints?api-version=2019-05-13
```

### <a name="example-response"></a>Örnek yanıt

URI 'yi al gönderildikten sonra 200 yanıtı döndürülür:

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

Kurtarma noktası, yukarıdaki yanıtta {Name} alanı ile tanımlanır.

## <a name="full-share-recovery-using-rest-api"></a>REST API kullanarak tam paylaşma kurtarması

Özgün dosya paylaşımının tamamını veya alternatif bir konuma geri yüklemek için bu geri yükleme seçeneğini kullanın.
Geri yükleme tetikleme bir POST isteğidir ve bu işlemi [tetikleyici geri yükleme](https://docs.microsoft.com/rest/api/backup/restores/trigger) REST API kullanarak gerçekleştirebilirsiniz.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints/{recoveryPointId}/restore?api-version=2019-05-13
```

{ContainerName} ve {korunabilir} değerleri [burada](#fetch-containername-and-protecteditemname) ayarlanmış ve Recoverypointıd yukarıda belirtilen kurtarma noktasının {Name} alanıdır.

```http
POST https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;afsaccount/protectedItems/AzureFileShare%3Bazurefiles/recoveryPoints/932886657837421071/restore?api-version=2019-05-13'
```

### <a name="create-request-body"></a>İstek gövdesi oluştur

Azure dosya paylaşımında geri yükleme tetiklenmesi için, istek gövdesinin bileşenleri aşağıda verilmiştir:

Adı |  Tür   |   Açıklama
--- | ---- | ----
Özellikler | AzureFileShareRestoreRequest | RestoreRequestResource özellikleri

İstek gövdesinin ve diğer ayrıntıların tanımlarının tüm listesi için, [tetikleyici geri yükleme REST API belgesine](https://docs.microsoft.com/rest/api/backup/restores/trigger#request-body)bakın.

### <a name="restore-to-original-location"></a>Özgün konuma geri yükleme

#### <a name="request-body-example"></a>İstek gövdesi örneği

Aşağıdaki istek gövdesi, bir Azure dosya paylaşımının geri yüklemesini tetiklemek için gereken özellikleri tanımlar:

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

### <a name="restore-to-alternate-location"></a>Alternatif konuma geri yükle

Alternatif konum kurtarma için aşağıdaki parametreleri belirtin:

* **Targetresourceıd**: yedeklenen içeriğin geri yüklendiği depolama hesabı. Hedef depolama hesabı kasayla aynı konumda olmalıdır.
* **ad**: yedeklenen içeriğin geri yüklendiği hedef depolama hesabı içindeki dosya paylaşma.
* **Targetfolderpath**: verilerin geri yüklendiği dosya paylaşımının altındaki klasör.

#### <a name="request-body-example"></a>İstek gövdesi örneği

Aşağıdaki istek gövdesi, *afsaccount* depolama hesabındaki *azurefiles* dosya paylaşımının *afaccount1* depolama hesabındaki *azurefiles1* dosya paylaşımında geri yükler.

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

Geri yükleme işleminin tetiklenmesi [zaman uyumsuz bir işlemdir](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Bu işlem ayrı olarak izlenmesi gereken başka bir işlem oluşturur.
Başka bir işlem oluşturulduğunda 202 (kabul edildi) ve bu işlem tamamlandığında 200 (Tamam) iki yanıt döndürür.

#### <a name="response-example"></a>Yanıt örneği

Bir geri yüklemeyi tetiklemek için *Post* URI 'sini gönderdikten sonra, ilk yanıt bir konum üst bilgisi veya Azure-Async-header ile 202 (kabul edilir) olur.

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

Ardından, bir GET komutuyla konum üst bilgisini veya Azure-AsyncOperation üstbilgisini kullanarak elde edilen işlemi izleyin.

```http
GET https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupOperations/68ccfbc1-a64f-4b29-b955-314b5790cfa9?api-version=2016-12-01
```

İşlem tamamlandıktan sonra, yanıt gövdesinde elde edilen geri yükleme işinin KIMLIĞI ile 200 (Tamam) döndürür.

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

Alternatif konum kurtarma için yanıt gövdesi şöyle olacaktır:

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

Yedekleme işi uzun süredir çalışan bir işlem olduğundan, [REST API belge kullanan izleme işlerinde](https://docs.microsoft.com/azure/backup/backup-azure-arm-userestapi-managejobs#tracking-the-job)açıklandığı şekilde izlenmelidir.

## <a name="item-level-recovery-using-rest-api"></a>REST API kullanarak öğe düzeyinde kurtarma

Özgün veya alternatif konumdaki tek tek dosyaları veya klasörleri geri yüklemek için bu geri yükleme seçeneğini kullanabilirsiniz.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints/{recoveryPointId}/restore?api-version=2019-05-13
```

{ContainerName} ve {korunabilir} değerleri [burada](#fetch-containername-and-protecteditemname) ayarlanmış ve Recoverypointıd yukarıda belirtilen kurtarma noktasının {Name} alanıdır.

```http
POST https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;afsaccount/protectedItems/AzureFileShare%3Bazurefiles/recoveryPoints/932886657837421071/restore?api-version=2019-05-13'
```

### <a name="create-request-body"></a>İstek gövdesi oluştur

Azure dosya paylaşımında geri yükleme tetiklenmesi için, istek gövdesinin bileşenleri aşağıda verilmiştir:

Adı |  Tür   |   Açıklama
--- | ---- | ----
Özellikler | AzureFileShareRestoreRequest | RestoreRequestResource özellikleri

İstek gövdesinin ve diğer ayrıntıların tanımlarının tüm listesi için, [tetikleyici geri yükleme REST API belgesine](https://docs.microsoft.com/rest/api/backup/restores/trigger#request-body)bakın.

### <a name="restore-to-original-location"></a>Özgün konuma geri yükleme

Aşağıdaki istek gövdesi, restore *. txt* dosyasını *afsaccount* depolama hesabındaki *azurefiles* dosya paylaşımında geri yüklemektir.

Istek gövdesi oluştur

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

### <a name="restore-to-alternate-location"></a>Alternatif konuma geri yükle

Aşağıdaki istek gövdesi, *afsaccount* depolama hesabındaki *Azurefiles* dosya paylaşımında *restoretest. txt* dosyasını *afaccount1* depolama hesabındaki *azurefiles1* dosya paylaşımının *restoredata* klasörüne geri yüklemektir.

İstek gövdesi oluştur

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

Yanıtın [tam paylaşma geri yüklemeleri](#full-share-recovery-using-rest-api)için yukarıda açıklanacak şekilde işlenmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [REST API kullanarak Azure dosya paylaşımları yedeklemesini yönetme](manage-azure-file-share-rest-api.md)hakkında bilgi edinin.
