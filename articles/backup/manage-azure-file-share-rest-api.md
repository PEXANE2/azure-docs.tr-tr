---
title: REST API ile Azure dosya paylaşma yedeklemesini yönetme
description: Azure Backup tarafından yedeklenen Azure dosya paylaşımlarını yönetmek ve izlemek için REST API nasıl kullanacağınızı öğrenin.
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 1e1d3463aa5d6ee10782e2ee17a7c17ffd64cb61
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82184920"
---
# <a name="manage-azure-file-share-backup-with-rest-api"></a>Azure dosya paylaşma yedeklemesini REST API ile yönetme

Bu makalede, [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview)tarafından yedeklenen Azure dosya paylaşımlarını yönetmek ve izlemek için görevlerin nasıl gerçekleştirileceği açıklanır.

## <a name="monitor-jobs"></a>İşleri izleme

Azure Backup hizmeti arka planda çalışan işleri tetikler. Bu, yedeklemeyi tetikleme, işlemleri geri yükleme ve yedeklemeyi devre dışı bırakma gibi senaryolar içerir. Bu işler kimlikleri kullanılarak izlenebilir.

### <a name="fetch-job-information-from-operations"></a>İşlemlerden iş bilgilerini getir

Yedeklemenin tetiklenmesi gibi bir işlem, yanıtta her zaman bir iş kimliği döndürülür.

Örneğin, bir [tetikleyici yedekleme REST API](backup-azure-file-share-rest-api.md#trigger-an-on-demand-backup-for-file-share) işleminin son yanıtı aşağıdaki gibidir:

```json
{
    "id": "c3a52d1d-0853-4211-8141-477c65740264",
    "name": "c3a52d1d-0853-4211-8141-477c65740264",
    "status": "Succeeded",
    "startTime": "2020-02-03T18:10:48.296012Z",
    "endTime": "2020-02-03T18:10:48.296012Z",
    "properties": {
        "objectType": "OperationStatusJobExtendedInfo",
        "jobId": "e2ca2cf4-2eb9-4d4b-b16a-8e592d2a658b"
    }
}
```

Azure dosya paylaşma yedekleme işi, **JobId** alanı tarafından tanımlanır ve [burada](https://docs.microsoft.com/rest/api/backup/jobdetails/) belirtildiği gibi bir get isteği kullanılarak izlenebilir.

### <a name="tracking-the-job"></a>İşi izleme

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupJobs/{jobName}?api-version=2019-05-13
```

{JobName}, yukarıda bahsedilen "JobId" dır. Yanıt her zaman "200 Tamam" dır. bu **durum** , işin durumunu gösteren durum alanıdır. "Completed" veya "Completedwithuyarılar" olduktan sonra **Extendeınfo** bölümü iş hakkında daha fazla ayrıntı gösterir.

```http
GET https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupJobs/e2ca2cf4-2eb9-4d4b-b16a-8e592d2a658b?api-version=2019-05-13'
```

#### <a name="response"></a>Yanıt

Name  | Tür  |  Description
--- | --- | ----
200 TAMAM |  JobResource  | Tamam

#### <a name="response-example"></a>Yanıt örneği

URI 'yi *Al* gönderildikten sonra 200 yanıtı döndürülür.

```http
HTTP/1.1" 200
'Cache-Control': 'no-cache'
'Pragma': 'no-cache'
'Transfer-Encoding': 'chunked'
'Content-Type': 'application/json'
'Content-Encoding': 'gzip'
'Expires': '-1'
'Vary': 'Accept-Encoding'
'Server': 'Microsoft-IIS/10.0, Microsoft-IIS/10.0'
'X-Content-Type-Options': 'nosniff'
'x-ms-request-id': 'dba43f00-5cdb-43b1-a9ec-23e419db67c5'
'x-ms-client-request-id': 'a644712a-4895-11ea-ba57-0a580af42708, a644712a-4895-11ea-ba57-0a580af42708'
'X-Powered-By': 'ASP.NET'
'Strict-Transport-Security': 'max-age=31536000; includeSubDomains'
'x-ms-ratelimit-remaining-subscription-reads': '11999'
'x-ms-correlation-request-id': 'dba43f00-5cdb-43b1-a9ec-23e419db67c5'
'x-ms-routing-request-id': 'WESTEUROPE:20200206T040341Z:dba43f00-5cdb-43b1-a9ec-23e419db67c5'
'Date': 'Thu, 06 Feb 2020 04:03:40 GMT'
{
    "id": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupJobs/e2ca2cf4-2eb9-4d4b-b16a-8e592d2a658b",
    "name": "e2ca2cf4-2eb9-4d4b-b16a-8e592d2a658b",
    "type": "Microsoft.RecoveryServices/vaults/backupJobs",
    "properties": {
        "jobType": "AzureStorageJob",
        "duration": "00:00:43.1809140",
        "storageAccountName": "testvault2",
        "storageAccountVersion": "Storage",
        "extendedInfo": {
            "tasksList": [],
            "propertyBag": {
                "File Share Name": "testshare",
                "Storage Account Name": "testvault2",
                "Policy Name": "schedule1"
            }
        },
        "entityFriendlyName": "testshare",
        "backupManagementType": "AzureStorage",
        "operation": "ConfigureBackup",
        "status": "Completed",
        "startTime": "2020-02-03T18:10:48.296012Z",
        "endTime": "2020-02-03T18:11:31.476926Z",
        "activityId": "3677cec0-942d-4eac-921f-8f3c873140d7"
    }
}
```

## <a name="modify-policy"></a>İlkeyi Değiştir

Dosya paylaşımının korunduğu ilkeyi değiştirmek için, korumayı etkinleştirmek üzere aynı biçimi kullanabilirsiniz. İstek ilkesinde yeni ilke KIMLIĞINI sağlamanız ve isteği göndermeniz yeterlidir.

Örneğin: *TestShare* öğesinin koruma ilkesini *schedule1* ' den *schedule2*' ye değiştirmek için, istek gövdesinde *schedule2* kimliği sağlayın.

```json
{
  "properties": {
    "protectedItemType": "AzureFileShareProtectedItem",
    "sourceResourceId": "/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/testvault2",
    "policyId": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupPolicies/schedule2"
  }
}
```

## <a name="stop-protection-but-retain-existing-data"></a>Korumayı durdurun, ancak mevcut verileri koruyun

Korumalı bir dosya paylaşımında korumayı kaldırabilir, ancak zaten yedeklenmiş olan verileri koruyabilirsiniz. Bunu yapmak için,[yedeklemeyi etkinleştirmek](backup-azure-file-share-rest-api.md#enable-backup-for-the-file-share) ve isteği göndermek üzere kullandığınız istek gövdesinde ilkeyi kaldırın. İlkeyle ilişkilendirme kaldırıldıktan sonra, yedeklemeler artık tetiklenmez ve yeni kurtarma noktası oluşturulmaz.

```json
{
  "properties": {
    "protectedItemType": "AzureFileShareProtectedItem",
    "sourceResourceId": "/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/testvault2",
    "policyId": “" ,
“protectionState”:”ProtectionStopped”
  }
}
```

### <a name="sample-response"></a>Örnek yanıt

Bir dosya paylaşımının korumasını durdurmak zaman uyumsuz bir işlemdir. İşlem izlenmesi gereken başka bir işlem oluşturur. Başka bir işlem oluşturulduğunda 202 (kabul edildi) ve bu işlem tamamlandığında 200 olan iki yanıt döndürür.

İşlem başarıyla kabul edildiğinde yanıt üst bilgisi:

```http
HTTP/1.1" 202
'Cache-Control': 'no-cache'
'Pragma': 'no-cache'
'Expires': '-1'
'Location': 'https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;testvault2/protectedItems/AzureFileShare;testshare/operationResults/b300922a-ad9c-4181-b4cd-d42ea780ad77?api-version=2019-05-13'
'Retry-After': '60'
msrest.http_logger :     'Azure-AsyncOperation': 'https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;testvault2/protectedItems/AzureFileShare;testshare/operationsStatus/b300922a-ad9c-4181-b4cd-d42ea780ad77?api-version=2019-05-13'
'X-Content-Type-Options': 'nosniff'
'x-ms-request-id': '3895e8a1-e4b9-4da5-bec7-2cf0266405f8'
'x-ms-client-request-id': 'd331c15e-48ab-11ea-84c0-0a580af46a50, d331c15e-48ab-11ea-84c0-0a580af46a50'
'Strict-Transport-Security': 'max-age=31536000; includeSubDomains'
'X-Powered-By': 'ASP.NET'
'x-ms-ratelimit-remaining-subscription-writes': '1199'
'x-ms-correlation-request-id': '3895e8a1-e4b9-4da5-bec7-2cf0266405f8'
'x-ms-routing-request-id': 'WESTEUROPE:20200206T064224Z:3895e8a1-e4b9-4da5-bec7-2cf0266405f8'
'Date': 'Thu, 06 Feb 2020 06:42:24 GMT'
'Content-Length': '0'
```

Ardından, GET komutuyla konum üstbilgisini veya Azure-AsyncOperation üstbilgisini kullanarak elde edilen işlemi izleyin:

```http
GET https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupoperations/b300922a-ad9c-4181-b4cd-d42ea780ad77?api-version=2016-12-01
```

### <a name="response-body"></a>Yanıt gövdesi

```json
{
    "id": "b300922a-ad9c-4181-b4cd-d42ea780ad77",
    "name": "b300922a-ad9c-4181-b4cd-d42ea780ad77",
    "status": "Succeeded",
    "startTime": "2020-02-06T06:42:24.4001299Z",
    "endTime": "2020-02-06T06:42:24.4001299Z",
    "properties": {
        "objectType": "OperationStatusJobExtendedInfo",
        "jobId": "7816fca8-d5be-4c41-b911-1bbd922e5826"
    }
}
```

## <a name="stop-protection-and-delete-data"></a>Korumayı durdurma ve verileri silme

Korumalı bir dosya paylaşımındaki korumayı kaldırmak ve yedekleme verilerini silmek için, [burada](https://docs.microsoft.com/rest/api/backup/protecteditems/delete)ayrıntılı olarak bir silme işlemi gerçekleştirin.

```http
DELETE https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}?api-version=2019-05-13
```

{ContainerName} ve {Koruyucuteditemname} parametreleri [burada](restore-azure-file-share-rest-api.md#fetch-containername-and-protecteditemname)ayarlanmış.

Aşağıdaki örnek, *azurefilesvault*ile korunan *TestShare* dosya paylaşımının korumasını durdurmak için bir işlemi tetikler.

```http
DELETE https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;Storage;AzureFiles;testvault2/protectedItems/azurefileshare;testshare?api-version=2016-12-01
```

### <a name="responses"></a>Yanıtlar

Korumayı silme işlemi zaman uyumsuz bir işlemdir. İşlem ayrıca izlenmesi gereken başka bir işlem oluşturur.
Bu işlem tamamlandığında, başka bir işlem oluşturulduğunda ve 204 (NoContent) olduğunda iki yanıt döndürür: 202 (kabul edildi).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure dosya paylaşımları için yedeklemeyi yapılandırırken sorunları nasıl giderebileceğinizi](troubleshoot-azure-files.md)öğrenin.
