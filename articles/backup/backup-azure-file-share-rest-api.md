---
title: REST API ile Azure dosya paylaşımlarını yedekleme
description: Kurtarma Hizmetleri kasasındaki Azure dosya paylaşımlarını yedeklemek için REST API kullanmayı öğrenin
ms.topic: conceptual
ms.date: 02/16/2020
ms.openlocfilehash: 2cf385830ec1be17cb62432e6ef9cba7d82a9db1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79248105"
---
# <a name="backup-azure-file-share-using-azure-backup-via-rest-api"></a>REST API aracılığıyla Azure Backup kullanarak Azure dosya paylaşma 'yı yedekleme

Bu makalede, REST API aracılığıyla Azure Backup kullanarak bir Azure dosya paylaşımının nasıl yedekleneceği açıklanır.

Bu makalede, zaten bir kurtarma hizmetleri Kasası oluşturdunuz ve dosya paylaşımınızda yedeklemeyi yapılandırmak için ilke oluşturdunuz. Yapmadıysanız, yeni kasalar ve ilkeler oluşturmak için [kasa oluşturma](https://docs.microsoft.com/azure/backup/backup-azure-arm-userestapi-createorupdatevault) ve [ilke oluşturma](https://docs.microsoft.com/azure/backup/backup-azure-arm-userestapi-createorupdatepolicy) REST API öğreticiler bölümüne başvurun.

Bu makalede, aşağıdaki kaynakları kullanacağız:

- **Recoveryserviceskasası**: *azurefilesvault*

- **İlke:** *schedule1*

- **Kaynak grubu**: *azurefiles*

- **Depolama hesabı**: *testvault2*

- **Dosya paylaşma**: *TestShare*

## <a name="configure-backup-for-an-unprotected-azure-file-share-using-rest-api"></a>REST API kullanarak korumasız bir Azure dosya paylaşımının yedeklemesini yapılandırma

### <a name="discover-storage-accounts-with-unprotected-azure-file-shares"></a>Korumasız Azure dosya paylaşımlarına sahip depolama hesaplarını bulma

Kasanın, kurtarma hizmetleri kasasına yedeklenebilir dosya paylaşımları ile abonelikte bulunan tüm Azure depolama hesaplarını bulması gerekir. Bu, [yenileme işlemi](https://docs.microsoft.com/rest/api/backup/protectioncontainers/refresh)kullanılarak tetiklenir. Bu, kasasının geçerli abonelikteki tüm korumasız Azure dosya paylaşımlarının en son listesini almasını ve bu nesnelerin ' önbelleğe alınmasını sağlayan zaman uyumsuz bir *gönderi* işlemidir. Dosya paylaşımının ' önbelleğe alınması ' olduğunda, kurtarma hizmetleri dosya paylaşımında erişebilir ve koruma sağlayabilir.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupname}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/refreshContainers?api-version=2016-12-01&$filter={$filter}
```

Post URI 'sinin, `{subscriptionId}`, `{vaultName}`ve `{vaultresourceGroupName}` `{fabricName}` parametreleri vardır. Örneğimizde, farklı parametrelerin değeri aşağıdaki gibi olacaktır:

- `{fabricName}`*Azure*

- `{vaultName}`*azurefilesvault*

- `{vaultresourceGroupName}`*azurefiles*

- $filter = backupManagementType EQ ' AzureStorage '

URI 'de tüm gerekli parametreler verildiğinden, ayrı bir istek gövdesine gerek yoktur.

```http
POST https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/refreshContainers?api-version=2016-12-01&$filter=backupManagementType eq 'AzureStorage'
```

#### <a name="responses"></a>Yanıtlar

' Refresh ' işlemi [zaman uyumsuz bir işlemdir](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Bu işlemin Ayrıca izlenmesi gereken başka bir işlem oluşturduğu anlamına gelir.

Başka bir işlem oluşturulduğunda 202 (kabul edildi) ve bu işlem tamamlandığında 200 (Tamam) iki yanıt döndürür.

##### <a name="example-responses"></a>Örnek yanıtlar

*Post* isteği gönderildikten sonra, 202 (kabul edildi) yanıtı döndürülür.

```http
HTTP/1.1 202 Accepted
'Pragma': 'no-cache'
'Expires': '-1'
'Location': ‘https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/ResourceGroups
/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/operationResults/
cca47745-12d2-42f9-b3a4-75335f18fdf6?api-version=2016-12-01’
'Retry-After': '60'
'X-Content-Type-Options': 'nosniff'
'x-ms-request-id': '6cc12ceb-90a2-430d-a1ec-9b6b6fdea92b'
'x-ms-client-request-id': ‘3da383a5-d66d-4b7c-982a-bc8d94798d61,3da383a5-d66d-4b7c-982a-bc8d94798d61’
'Strict-Transport-Security': 'max-age=31536000; includeSubDomains'
'X-Powered-By': 'ASP.NET'
'x-ms-ratelimit-remaining-subscription-reads': '11996'
'x-ms-correlation-request-id': '6cc12ceb-90a2-430d-a1ec-9b6b6fdea92b'
'x-ms-routing-request-id': CENTRALUSEUAP:20200203T091326Z:6cc12ceb-90a2-430d-a1ec-9b6b6fdea92b'
'Date': 'Mon, 03 Feb 2020 09:13:25 GMT'
```

Basit bir *Get* komutuyla "konum" üst bilgisini kullanarak ortaya çıkan işlemi izleyin

```http
GET https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/operationResults/cca47745-12d2-42f9-b3a4-75335f18fdf6?api-version=2016-12-01
```

Tüm Azure depolama hesapları keşfedildiğinde GET komutu bir 200 (Içerik yok) yanıtı döndürür. Kasa artık, abonelik içinde yedeklenebilir dosya paylaşımları olan herhangi bir depolama hesabını keşfedebilecektir.

```http
HTTP/1.1 200 NoContent
Cache-Control  : no-cache
Pragma   : no-cache
X-Content-Type-Options  : nosniff
x-ms-request-id    : d9bdb266-8349-4dbd-9688-de52f07648b2
x-ms-client-request-id  : 3da383a5-d66d-4b7c-982a-bc8d94798d61,3da383a5-d66d-4b7c-982a-bc8d94798d61
Strict-Transport-Security  : max-age=31536000; includeSubDomains
X-Powered-By    : ASP.NET
x-ms-ratelimit-remaining-subscription-reads: 11933
x-ms-correlation-request-id   : d9bdb266-8349-4dbd-9688-de52f07648b2
x-ms-routing-request-id  : CENTRALUSEUAP:20200127T105304Z:d9bdb266-8349-4dbd-9688-de52f07648b2
Date   : Mon, 27 Jan 2020 10:53:04 GMT
```

### <a name="get-list-of-storage-accounts-that-can-be-protected-with-recovery-services-vault"></a>Kurtarma Hizmetleri kasası ile korunabilen depolama hesaplarının listesini al

"Önbelleğe alma" işleminin yapıldığını doğrulamak için, aboneliğin altındaki tüm korunabilir depolama hesaplarını listeleyin. Ardından, yanıttaki istenen depolama hesabını bulun. Bu işlem, [korunabilir kapsayıcı al](https://docs.microsoft.com/rest/api/backup/protectablecontainers/list) işlemi kullanılarak yapılır.

```http
GET https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectableContainers?api-version=2016-12-01&$filter=backupManagementType eq 'AzureStorage'
```

*Get* URI 'sinin tüm gerekli parametreleri vardır. Ek istek gövdesi gerekli değildir.

Yanıt gövdesi örneği:

```json
{

  "value": [

    {

      "id": "/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers
 /Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/

protectableContainers/StorageContainer;Storage;AzureFiles;testvault2",

      "name": "StorageContainer;Storage;AzureFiles;testvault2",

      "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectableContainers",

      "properties": {

        "friendlyName": "testvault2",

        "backupManagementType": "AzureStorage",

        "protectableContainerType": "StorageContainer",

        "healthStatus": "Healthy",

        "containerId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/
 AzureFiles/providers/Microsoft.Storage/storageAccounts/testvault2"

      }

    }

  ]

}
```

Yanıt gövdesinde kolay ada sahip *testvault2* Storage hesabını bulabileceğinizden, yukarıda gerçekleştirilen yenileme işlemi başarılı oldu. Kurtarma Hizmetleri Kasası artık aynı abonelikte korunmayan dosya paylaşımlarına sahip depolama hesaplarını başarıyla bulabilir.

### <a name="register-storage-account-with-recovery-services-vault"></a>Depolama hesabını kurtarma hizmetleri kasasıyla kaydetme

Bu adım yalnızca depolama hesabını daha önce kasaya kaydetmediyseniz gereklidir. Kasayı [Protectioncontainers-Register işlemi](https://docs.microsoft.com/rest/api/backup/protectioncontainers/register)aracılığıyla kaydedebilirsiniz.

```http
PUT https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}?api-version=2016-12-01
```

URI için değişkenleri aşağıdaki şekilde ayarlayın:

- {resourceGroupName}- *azurefiles*
- {fabricName}- *Azure*
- {vaultName}- *azurefilesvault*
- {containerName}-bu, korunabilir kapsayıcı al işleminin yanıt gövdesinde ad özniteliğidir.
   Bizim örneğimizde, *Storagecontainer; Depo AzureFiles; testvault2*

>[!NOTE]
> Yanıtın ad özniteliğini her zaman alın ve bu isteğe girin. Kapsayıcı adı biçimini sabit kodlamayın veya oluşturun. Bunu oluşturursanız veya sabit kodınızda, kapsayıcı adı biçimi gelecekte değişirse API çağrısı başarısız olur.

<br>

```http
PUT https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AzureFiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;Storage;AzureFiles;testvault2?api-version=2016-12-01
```

Oluşturma isteği gövdesi aşağıdaki gibidir:

```json
{

 "properties": {


  "containerType": "StorageContainer",


  "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/testvault2",


  "resourceGroup": "AzureFiles",


  "friendlyName": "testvault2",


  "backupManagementType": "AzureStorage"


 }
```

İstek gövdesinin ve diğer ayrıntıların tanımlarının tam listesi için, [Protectioncontainers-Register](https://docs.microsoft.com/rest/api/backup/protectioncontainers/register#azurestoragecontainer)bölümüne bakın.

Bu zaman uyumsuz bir işlemdir ve iki yanıt döndürür 202: işlem kabul edildiğinde "200 Tamam" ve işlem tamamlandığında "Tamam".  İşlem durumunu izlemek için, işlemin en son durumunu almak üzere konum üst bilgisini kullanın.

```http
GET https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AzureFiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;Storage;AzureFiles;testvault2/operationresults/1a3c8ee7-e0e5-43ed-b8b3-73cc992b6db9?api-version=2016-12-01
```

İşlem tamamlandığında yanıt gövdesi örneği:

```json
{
    "id": "/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AzureFiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/
protectionContainers/StorageContainer;Storage;AzureFiles;testvault2",
    "name": "StorageContainer;Storage;AzureFiles;testvault2",
    "properties": {
        "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/testvault2",
        "protectedItemCount": 0,
        "friendlyName": "testvault2",
        "backupManagementType": "AzureStorage",
        "registrationStatus": "Registered",
        "healthStatus": "Healthy",
        "containerType": "StorageContainer",
        "protectableObjectType": "StorageContainer"
    }
}
```

Yanıt gövdesinde, kayıt işleminin *registrationstatus* parametresinin değerinden başarılı olup olmadığını doğrulayabilirsiniz. Bu durumda, *testvault2*için kayıtlı durumu gösterir, bu nedenle kayıt işlemi başarılı oldu.

### <a name="inquire-all-unprotected-files-shares-under-a-storage-account"></a>Bir depolama hesabı altındaki tüm korumasız dosya paylaşımlarını sorgula

[Koruma kapsayıcıları-sorgulama](https://docs.microsoft.com/rest/api/backup/protectioncontainers/inquire) işlemini kullanarak bir depolama hesabındaki korunabilir öğeler hakkında sorgulama yapabilirsiniz. Bu, zaman uyumsuz bir işlemdir ve sonuçların konum üst bilgisi kullanılarak izlenmesi gerekir.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/inquire?api-version=2016-12-01
```

Yukarıdaki URI için değişkenleri aşağıdaki şekilde ayarlayın:

- {vaultName}- *azurefilesvault*
- {fabricName}- *Azure*
- {containerName}-korunabilir kapsayıcı al işleminin yanıt gövdesinde name özniteliğine bakın. Bizim örneğimizde, *Storagecontainer; Depo AzureFiles; testvault2*

```http
https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;Storage;AzureFiles;testvault2/inquire?api-version=2016-12-01
```

İstek başarılı olduktan sonra, "Tamam" durum kodunu döndürür

```http
Cache-Control : no-cache
Pragma   : no-cache
X-Content-Type-Options: nosniff
x-ms-request-id  : 68727f1e-b8cf-4bf1-bf92-8e03a9d96c46
x-ms-client-request-id  : 3da383a5-d66d-4b7c-982a-bc8d94798d61,3da383a5-d66d-4b7c-982a-bc8d94798d61
Strict-Transport-Security: max-age=31536000; includeSubDomains
Server  : Microsoft-IIS/10.0
X-Powered-B : ASP.NET
x-ms-ratelimit-remaining-subscription-reads: 11932
x-ms-correlation-request-id  : 68727f1e-b8cf-4bf1-bf92-8e03a9d96c46
x-ms-routing-request-id   : CENTRALUSEUAP:20200127T105305Z:68727f1e-b8cf-4bf1-bf92-8e03a9d96c46
Date  : Mon, 27 Jan 2020 10:53:05 GMT
```

### <a name="select-the-file-share-you-want-to-back-up"></a>Yedeklemek istediğiniz dosya payını seçin

Abonelik kapsamındaki tüm korunabilir öğeleri listeleyebilir ve [Backupkorunabilir bir Tableıtems al](https://docs.microsoft.com/rest/api/backup/backupprotectableitems/list) işlemi kullanılarak yedeklenecek istenen dosya paylaşımının yerini bulabilirsiniz.

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupProtectableItems?api-version=2016-12-01&$filter={$filter}
```

URI 'yi şu şekilde oluşturun:

- {vaultName}- *azurefilesvault*
- {$filter}- *Backupmanagementtype EQ ' AzureStorage '*

```http
GET https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupProtectableItems?$filter=backupManagementType eq 'AzureStorage'&api-version=2016-12-01
```

Örnek yanıt:

```json
Status Code:200

{
    "value": [
        {
            "id": "/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/storagecontainer;storage;azurefiles;afaccount1/protectableItems/azurefileshare;azurefiles1",
            "name": "azurefileshare;azurefiles1",
            "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectableItems",
            "properties": {
                "parentContainerFabricId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/afaccount1",
                "parentContainerFriendlyName": "afaccount1",
                "azureFileShareType": "XSMB",
                "backupManagementType": "AzureStorage",
                "workloadType": "AzureFileShare",
                "protectableItemType": "AzureFileShare",
                "friendlyName": "azurefiles1",
                "protectionState": "NotProtected"
            }
        },
        {
            "id": "/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/storagecontainer;storage;azurefiles;afsaccount/protectableItems/azurefileshare;afsresource",
            "name": "azurefileshare;afsresource",
            "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectableItems",
            "properties": {
                "parentContainerFabricId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/afsaccount",
                "parentContainerFriendlyName": "afsaccount",
                "azureFileShareType": "XSMB",
                "backupManagementType": "AzureStorage",
                "workloadType": "AzureFileShare",
                "protectableItemType": "AzureFileShare",
                "friendlyName": "afsresource",
                "protectionState": "NotProtected"
            }
        },
        {
            "id": "/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/storagecontainer;storage;azurefiles;testvault2/protectableItems/azurefileshare;testshare",
            "name": "azurefileshare;testshare",
            "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectableItems",
            "properties": {
                "parentContainerFabricId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/testvault2",
                "parentContainerFriendlyName": "testvault2",
                "azureFileShareType": "XSMB",
                "backupManagementType": "AzureStorage",
                "workloadType": "AzureFileShare",
                "protectableItemType": "AzureFileShare",
                "friendlyName": "testshare",
                "protectionState": "NotProtected"
            }
        }
    ]
}
```

Yanıt, tüm korumasız dosya paylaşımlarının listesini içerir ve Azure kurtarma hizmeti 'nin yedeklemeyi yapılandırmak için gereken tüm bilgileri içerir.

### <a name="enable-backup-for-the-file-share"></a>Dosya paylaşımında yedeklemeyi etkinleştir

İlgili dosya paylaşımının kolay adıyla "tanımlanması" durumunda, korunacak ilkeyi seçin. Kasadaki mevcut ilkeler hakkında daha fazla bilgi edinmek için [liste ILKESI API](https://docs.microsoft.com/rest/api/backup/backuppolicies/list)'sine bakın. Ardından ilke adına başvurarak [ilgili ilkeyi](https://docs.microsoft.com/rest/api/backup/protectionpolicies/get) seçin. İlke oluşturmak için [ilke oluşturma öğreticisi](https://docs.microsoft.com/azure/backup/backup-azure-arm-userestapi-createorupdatepolicy)' ne bakın.

Korumayı etkinleştirme, "korumalı öğe" oluşturan zaman uyumsuz bir *PUT* işlemidir.

```http
PUT https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}?api-version=2019-05-13
```

ContainerName ve korunabilir olan tablo öğelerinin yanıt gövdesinde ID özniteliğini kullanarak **ContainerName** ve **korunabilir dıtemname** değişkenlerini ayarlayın.

Bizim örneğimizde, korumak istediğimiz dosya paylaşımının KIMLIĞI:

```output
"/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/storagecontainer;storage;azurefiles;testvault2/protectableItems/azurefileshare;testshare
```

- {ContainerName}- *storagecontainer; depolama; azurefiles; testvault2*
- {Korunabilir}- *azurefileshare; TestShare*

Ya da koruma kapsayıcısının ve korunabilir öğe yanıtlarının **ad** özniteliğine başvurabilirsiniz.

>[!NOTE]
>Yanıtın ad özniteliğini her zaman alın ve bu isteğe girin. Kapsayıcı adı biçimini veya korumalı öğe adı biçimini sabit kodlamayın veya oluşturun. Bunu oluşturursanız veya sabit kodınızda, kapsayıcı adı biçimi veya korunan öğe adı biçimi gelecekte değişirse API çağrısı başarısız olur.

<br>

```http
PUT https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;Storage;AzureFiles;testvault2/protectedItems/azurefileshare;testshare?api-version=2016-12-01
```

İstek gövdesi oluştur:

Aşağıdaki istek gövdesi, korumalı bir öğe oluşturmak için gereken özellikleri tanımlar.

```json
{
  "properties": {
    "protectedItemType": "AzureFileShareProtectedItem",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/testvault2",
    "policyId": "/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupPolicies/schedule1"
  }
}
```

**Sourceresourceıd** , Get Backupkoruyucutablotems yanıtı olarak **parentcontainerfabricıd** 'dir.

Örnek Yanıt

Korumalı bir öğenin oluşturulması, izlenmesi gereken başka bir işlem oluşturan zaman uyumsuz bir işlemdir. Bu işlem tamamlandığında, başka bir işlem oluşturulduğunda ve 200 (Tamam) olduğunda iki yanıt döndürür: 202 (kabul edildi).

Korumalı öğe oluşturma veya güncelleştirme için *PUT* isteğini gönderdikten sonra, ilk yanıt bir konum üst bilgisi ile 202 (kabul edildi) olur.

```http
HTTP/1.1 202 Accepted
Cache-Control  : no-cache
Pragma  : no-cache
Location : https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;Storage;AzureFiles;testvault2/protectedItems/azurefileshare;testshare/operationResults/c3a52d1d-0853-4211-8141-477c65740264?api-version=2016-12-01
Retry-Afte  : 60
Azure-AsyncOperation  : https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;Storage;AzureFiles;testvault2/protectedItems/azurefileshare;testshare/operationResults/c3a52d1d-0853-4211-8141-477c65740264?api-version=2016-12-01
X-Content-Type-Options : nosniff
x-ms-request-id : b55527fa-f473-4f09-b169-9cc3a7a39065
x-ms-client-request-id: 3da383a5-d66d-4b7c-982a-bc8d94798d61,3da383a5-d66d-4b7c-982a-bc8d94798d61
Strict-Transport-Security : max-age=31536000; includeSubDomains
X-Powered-By  : ASP.NET
x-ms-ratelimit-remaining-subscription-writes: 1198
x-ms-correlation-request-id : b55527fa-f473-4f09-b169-9cc3a7a39065
x-ms-routing-request-id  : CENTRALUSEUAP:20200127T105412Z:b55527fa-f473-4f09-b169-9cc3a7a39065
Date : Mon, 27 Jan 2020 10:54:12 GMT
```

Ardından, *Get* komutuyla konum üstbilgisini veya Azure-AsyncOperation üstbilgisini kullanarak elde edilen işlemi izleyin.

```http
GET https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupOperations/c3a52d1d-0853-4211-8141-477c65740264?api-version=2016-12-01
```

İşlem tamamlandıktan sonra, yanıt gövdesinde korunan öğe içeriğiyle 200 (Tamam) döndürür.

Örnek yanıt gövdesi:

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

Bu, korumanın dosya paylaşımında etkinleştirildiğini ve ilk yedeklemenin ilke çizelgesine göre tetiklendiğini onaylar.

## <a name="trigger-an-on-demand-backup-for-file-share"></a>Dosya paylaşma için isteğe bağlı yedekleme tetikleyin

Yedekleme için bir Azure dosya paylaşımından yapılandırıldıktan sonra yedeklemeler, ilke zamanlamaya göre çalışır. Zamanlanan ilk yedeklemeyi bekleyebilir veya dilediğiniz zaman bir isteğe bağlı yedekleme tetikleyebilirsiniz.

İsteğe bağlı yedekleme tetiklenmesi bir POST işlemidir.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/backup?api-version=2016-12-01
```

Yedekleme etkinleştirilirken {containerName} ve {korunabilir} ve {korunabilir} yukarıda oluşturulmuş. Örneğimiz için şu şekilde çeviri yapar:

```http
POST https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;testvault2/protectedItems/AzureFileShare;testshare/backup?api-version=2017-07-01
```

### <a name="create-request-body"></a>İstek gövdesi oluştur

İsteğe bağlı bir yedeklemeyi tetiklemek için, istek gövdesinin bileşenleri aşağıda verilmiştir.

| Adı       | Tür                       | Açıklama                       |
| ---------- | -------------------------- | --------------------------------- |
| Özellikler | AzurefilesharebackupReques | BackupRequestResource özellikleri |

İstek gövdesinin ve diğer ayrıntıların tanımlarının tamamı listesi için bkz. [korumalı öğeler için tetikleyici yedeklemeleri REST API belgesi](https://docs.microsoft.com/rest/api/backup/backups/trigger#request-body).

İstek gövdesi örneği

```json
{

  "properties":{

   "objectType":"AzureFileShareBackupRequest",
    "recoveryPointExpiryTimeInUTC":"2020-03-07T18:29:59.000Z"
}

}
```

### <a name="responses"></a>Yanıtlar

İsteğe bağlı yedekleme tetiklenmesi [zaman uyumsuz bir işlemdir](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Bu işlemin Ayrıca izlenmesi gereken başka bir işlem oluşturduğu anlamına gelir.

Bu işlem tamamlandığında, başka bir işlem oluşturulduğunda ve 200 (Tamam) olduğunda iki yanıt döndürür: 202 (kabul edildi).

### <a name="example-responses"></a>Örnek yanıtlar

İstek üzerine yedekleme için *Post* isteğini gönderdikten sonra, ilk yanıt bir konum üst bilgisi veya Azure-Async-header ile 202 (kabul edilir) olur.

```http
'Cache-Control': 'no-cache'
'Pragma': 'no-cache'
'Expires': '-1'
'Location': https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;testvault2/protectedItems/AzureFileShare;testshare/operationResults/dc62d524-427a-4093-968d-e951c0a0726e?api-version=2017-07-01
'Retry-After': '60'
'Azure-AsyncOperation': https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;testvault2/protectedItems/AzureFileShare;testshare/operationsStatus/dc62d524-427a-4093-968d-e951c0a0726e?api-version=2017-07-01
'X-Content-Type-Options': 'nosniff'
'x-ms-request-id': '2e03b8d4-66b1-48cf-8094-aa8bff57e8fb'
'x-ms-client-request-id': 'a644712a-4895-11ea-ba57-0a580af42708, a644712a-4895-11ea-ba57-0a580af42708'
'Strict-Transport-Security': 'max-age=31536000; includeSubDomains'
'X-Powered-By': 'ASP.NET'
'x-ms-ratelimit-remaining-subscription-writes': '1199'
'x-ms-correlation-request-id': '2e03b8d4-66b1-48cf-8094-aa8bff57e8fb'
'x-ms-routing-request-id': 'WESTEUROPE:20200206T040339Z:2e03b8d4-66b1-48cf-8094-aa8bff57e8fb'
'Date': 'Thu, 06 Feb 2020 04:03:38 GMT'
'Content-Length': '0'
```

Ardından, *Get* komutuyla konum üstbilgisini veya Azure-AsyncOperation üstbilgisini kullanarak elde edilen işlemi izleyin.

```http
GET https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupOperations/dc62d524-427a-4093-968d-e951c0a0726e?api-version=2016-12-01
```

İşlem tamamlandıktan sonra, yanıt gövdesinde elde edilen yedekleme işinin KIMLIĞI ile 200 (Tamam) döndürür.

#### <a name="sample-response-body"></a>Örnek yanıt gövdesi

```json
{
    "id": "dc62d524-427a-4093-968d-e951c0a0726e",
    "name": "dc62d524-427a-4093-968d-e951c0a0726e",
    "status": "Succeeded",
    "startTime": "2020-02-06T11:06:02.1327954Z",
    "endTime": "2020-02-06T11:06:02.1327954Z",
    "properties": {
        "objectType": "OperationStatusJobExtendedInfo",
        "jobId": "39282261-cb52-43f5-9dd0-ffaf66beeaef"
    }
}
```

Yedekleme işi uzun süredir çalışan bir işlem olduğundan, [REST API belge kullanan izleme işlerinde](https://docs.microsoft.com/azure/backup/backup-azure-arm-userestapi-managejobs#tracking-the-job)açıklandığı şekilde izlenmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [REST API kullanarak Azure dosya paylaşımlarını geri yüklemeyi](restore-azure-file-share-rest-api.md)öğrenin.
