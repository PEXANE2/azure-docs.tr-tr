---
title: Azure Dosya paylaşımı yedeklemesini Rest API ile yönetme
description: Azure Yedekleme tarafından yedeklenen Azure dosya paylaşımlarını yönetmek ve izlemek için REST API'yi nasıl kullanacağınızı öğrenin.
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 9d29b226aff568c91de8e1f19ddc0c64f8169e4d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77444738"
---
# <a name="manage-azure-file-share-backup-with-rest-api"></a>AZURE Dosya paylaşımı yedeklemesini REST API ile yönetme

Bu makalede, [Azure Yedekleme](https://docs.microsoft.com/azure/backup/backup-overview)tarafından yedeklenen Azure dosya paylaşımlarını yönetmek ve izlemek için görevlerin nasıl gerçekleştirililizleyeceği açıklanmaktadır.

## <a name="monitor-jobs"></a>İşleri izleme

Azure Yedekleme hizmeti arka planda çalışan işleri tetikler. Buna yedeklemeyi tetikleme, işlemleri geri yükleme ve yedeklemeyi devre dışı bırakma gibi senaryolar dahildir. Bu işler kimliklerini kullanarak izlenebilir.

### <a name="fetch-job-information-from-operations"></a>Operasyonlardan iş bilgileri alma

Yedeklemeyi tetikleme gibi bir işlem yanıtta her zaman bir jobID döndürecektir.

Örneğin, [bir tetikleyici yedekleme REST API](backup-azure-file-share-rest-api.md#trigger-an-on-demand-backup-for-file-share) işleminin son yanıtı aşağıdaki gibidir:

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

Azure dosya paylaşımı yedekleme işi **jobId** alanı tarafından tanımlanır ve [get](https://docs.microsoft.com/rest/api/backup/jobdetails/) isteği kullanılarak burada belirtildiği gibi izlenebilir.

### <a name="tracking-the-job"></a>İşi izleme

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupJobs/{jobName}?api-version=2019-05-13
```

{jobName} yukarıda belirtilen "jobId" dir. Yanıt her zaman "200 Tamam" **durum** alanı işin durumunu gösteren. "Tamamlandı" veya "Tamamlanmış Uyarılar" olduğunda, **genişletilmiş Bilgi** bölümü iş hakkında daha fazla ayrıntı ortaya çıkarır.

```http
GET https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupJobs/e2ca2cf4-2eb9-4d4b-b16a-8e592d2a658b?api-version=2019-05-13'
```

#### <a name="response"></a>Yanıt

Adı  | Tür  |  Açıklama
--- | --- | ----
200 TAMAM |  İş Kaynağı  | Tamam

#### <a name="response-example"></a>Yanıt örneği

*GET* URI gönderildikten sonra 200 yanıt döndürülür.

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

## <a name="modify-policy"></a>İlkeyi değiştir

Dosya paylaşımının korunduğu ilkeyi değiştirmek için, korumayı etkinleştirmek için aynı biçimi kullanabilirsiniz. İstek ilkesinde yeni ilke kimliğini sağlaman ve isteği göndermeniz.

Örneğin: *Testshare'in* koruma ilkesini *zamanlama1'dan* *zamanlama2'ye*değiştirmek için istek gövdesinde *zamanlama2* kimliği sağlayın.

```json
{
  "properties": {
    "protectedItemType": "AzureFileShareProtectedItem",
    "sourceResourceId": "/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/testvault2",
    "policyId": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupPolicies/schedule2"
  }
}
```

## <a name="stop-protection-but-retain-existing-data"></a>Korumayı durdurun, ancak varolan verileri koruyun

Korumalı bir dosya paylaşımında korumayı kaldırabilir, ancak zaten yedeklenmiş verileri saklayabilirsiniz. Bunu yapmak için,[yedeklemeyi etkinleştirmek](backup-azure-file-share-rest-api.md#enable-backup-for-the-file-share) ve isteği göndermek için kullandığınız istek gövdesindeki ilkeyi kaldırın. İlkeyle ilişkilendirme kaldırıldıktan sonra yedeklemeler artık tetiklenmez ve yeni kurtarma noktaları oluşturulmaz.

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

Dosya paylaşımı için korumayı durdurma, eşzamanlı bir işlemdir. İşlem, izlenmesi gereken başka bir işlem oluşturur. İki yanıt verir: başka bir işlem oluşturulduğunda 202 (Kabul) ve bu işlem tamamlandığında 200.

İşlem başarıyla kabul edildiğinde yanıt üstbilgi:

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

Ardından, get komutuyla konum üstbilgisini veya Azure-AsyncOperation üstbilgisini kullanarak ortaya çıkan işlemi izleyin:

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

Korumalı bir dosya paylaşımındaki korumayı kaldırmak ve yedekleme verilerini de silmek [için, burada](https://docs.microsoft.com/rest/api/backup/protecteditems/delete)ayrıntılı olarak bir silme işlemi gerçekleştirin.

```http
DELETE https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}?api-version=2019-05-13
```

{containerName} ve {protectedItemName} parametreleri [burada](restore-azure-file-share-rest-api.md#fetch-containername-and-protecteditemname)ayarlanan gibidir.

Aşağıdaki örnekte, *azurefilesvault*ile korunan *testshare* dosya paylaşımı için korumayı durdurmak için bir işlem tetikler.

```http
DELETE https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;Storage;AzureFiles;testvault2/protectedItems/azurefileshare;testshare?api-version=2016-12-01
```

### <a name="responses"></a>Yanıtlar

Silme koruması eşzamanlı bir işlemdir. İşlem, ayrı olarak izlenmesi gereken başka bir işlem oluşturur.
İki yanıt verir: başka bir işlem oluşturulduğunda 202 (Kabul) ve bu işlem tamamlandığında 204 (NoContent).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Dosyası paylaşımları için yedeklemeyi yapılandırırken sorunları](troubleshoot-azure-files.md)nasıl gideriz öğrenin.