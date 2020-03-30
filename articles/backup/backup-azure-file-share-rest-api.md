---
title: REST API ile Azure dosya paylaşımlarını yedekleme
description: Kurtarma Hizmetleri Kasası'ndaki Azure dosya paylaşımlarını yedeklemek için REST API'yi nasıl kullanacağınızı öğrenin
ms.topic: conceptual
ms.date: 02/16/2020
ms.openlocfilehash: 2cf385830ec1be17cb62432e6ef9cba7d82a9db1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248105"
---
# <a name="backup-azure-file-share-using-azure-backup-via-rest-api"></a>Geri Ödeme API'si aracılığıyla Azure Yedekleme'yi kullanarak Azure dosya paylaşımını yedekleme

Bu makalede, REST API üzerinden Azure Yedekleme kullanarak bir Azure Dosyası paylaşımının nasıl yedeklenilen açıklanmaktadır.

Bu makalede, dosya paylaşımınız için yedekleme yapılandırmak için zaten bir kurtarma hizmetleri kasası ve ilkesi oluşturduğunuz varsayar. Yapmadıysanız, [tonoz oluşturma'ya](https://docs.microsoft.com/azure/backup/backup-azure-arm-userestapi-createorupdatevault) bakın ve yeni kasalar ve ilkeler oluşturmak için ilke REST API öğreticileri [oluşturun.](https://docs.microsoft.com/azure/backup/backup-azure-arm-userestapi-createorupdatepolicy)

Bu makale için aşağıdaki kaynakları kullanacağız:

- **RecoveryServicesVault**: *azurefilesvault*

- **İlke:** *zamanlama1*

- **Kaynak grubu**: *azurefiles*

- **Depolama Hesabı**: *testvault2*

- **Dosya Paylaş**: *testshare*

## <a name="configure-backup-for-an-unprotected-azure-file-share-using-rest-api"></a>REST API'yi kullanarak korumasız bir Azure dosya paylaşımı için yedeklemeyi yapılandırma

### <a name="discover-storage-accounts-with-unprotected-azure-file-shares"></a>Korumasız Azure dosya paylaşımlarıyla depolama hesaplarını keşfedin

Kasanın, Kurtarma Hizmetleri Kasası'na yedeklenebilen dosya paylaşımlarıyla abonelikteki tüm Azure depolama hesaplarını keşfetmesi gerekir. Bu, [yenileme işlemi](https://docs.microsoft.com/rest/api/backup/protectioncontainers/refresh)kullanılarak tetiklenir. Kasanın geçerli abonelikteki korumasız Azure Dosyası hisselerinin en son listesini almasını ve bunları 'önbelleğe almasını' sağlayan eşzamanlı bir *POST* işlemidir. Dosya paylaşımı 'önbelleğe alındıktan' sonra Kurtarma hizmetleri dosya paylaşımına erişebilir ve dosyayı koruyabilir.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupname}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/refreshContainers?api-version=2016-12-01&$filter={$filter}
```

POST URI, `{subscriptionId}` `{vaultName}`, `{vaultresourceGroupName}`, `{fabricName}` ve parametreleri vardır. Örneğimizde, farklı parametrelerin değeri aşağıdaki gibi olacaktır:

- `{fabricName}`*Azure'dur*

- `{vaultName}`*azurefilesvault* olduğunu

- `{vaultresourceGroupName}`*azurefiles*

- $filter=backupManagementType eq 'AzureStorage'

Uri'de gerekli tüm parametreler verildiğinden, ayrı bir istek organına gerek yoktur.

```http
POST https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/refreshContainers?api-version=2016-12-01&$filter=backupManagementType eq 'AzureStorage'
```

#### <a name="responses"></a>Yanıtlar

'Yenileme' işlemi eşzamanlı bir [işlemdir.](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) Bu işlem, ayrı olarak izlenmesi gereken başka bir işlem oluşturur anlamına gelir.

İki yanıt verir: başka bir işlem oluşturulduğunda 202 (Kabul) ve bu işlem tamamlandığında 200 (Tamam).

##### <a name="example-responses"></a>Örnek yanıtlar

*POST* isteği gönderildikten sonra, 202 (Kabul edilen) bir yanıt döndürülür.

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

Basit bir *GET* komutuyla "Konum" üstbilgisini kullanarak ortaya çıkan işlemi izleme

```http
GET https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/operationResults/cca47745-12d2-42f9-b3a4-75335f18fdf6?api-version=2016-12-01
```

Tüm Azure Depolama hesapları keşfedildikten sonra GET komutu 200 (İçerik Yok) yanıtını döndürür. Kasa artık abonelik içinde yedeklenebilen dosya paylaşımları olan herhangi bir depolama hesabını keşfedebilir.

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

### <a name="get-list-of-storage-accounts-that-can-be-protected-with-recovery-services-vault"></a>Kurtarma Hizmetleri kasası ile korunabilen depolama hesaplarının listesini alın

"Önbelleğe alma" yapıldığını doğrulamak için, abonelik altındaki tüm korunabilir depolama hesaplarını listele. Ardından yanıtta istenen depolama hesabını bulun. Bu, GET [ProtectableContainers](https://docs.microsoft.com/rest/api/backup/protectablecontainers/list) işlemi kullanılarak yapılır.

```http
GET https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectableContainers?api-version=2016-12-01&$filter=backupManagementType eq 'AzureStorage'
```

*GET* URI gerekli tüm parametrelere sahiptir. Ek istek gövdesi gerekmez.

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

*Testvault2* depolama hesabını dost adı ile yanıt gövdesinde bulabildiğimizden, yukarıda yapılan yenileme işlemi başarılı oldu. Kurtarma hizmetleri kasası artık aynı abonelikte korumasız dosya paylaşımları bulunan depolama hesaplarını başarıyla keşfedebilir.

### <a name="register-storage-account-with-recovery-services-vault"></a>Kurtarma Hizmetleri kasasına kayıt depolama hesabı

Bu adım, yalnızca depolama hesabını kasaya daha önce kaydetmediyseniz gereklidir. [Koruma Konteynerleri-Kayıt işlemi](https://docs.microsoft.com/rest/api/backup/protectioncontainers/register)ile kasayı kaydedebilirsiniz.

```http
PUT https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}?api-version=2016-12-01
```

URI değişkenlerini aşağıdaki gibi ayarlayın:

- {resourceGroupName} - *azurefiles*
- {fabricName} - *Azure*
- {vaultName} - *azurefilesvault*
- {containerName} - Get ProtectableContainers işleminin yanıt gövdesindeki ad özniteliğidir.
   Örneğimizde, *StorageContainer; Depolama; AzureDosyalar;testvault2*

>[!NOTE]
> Her zaman yanıtın ad özniteliğini alın ve bu istekle doldurun. Sabit kod lamayı veya kapsayıcı adı biçimini oluşturmayın. Oluşturur veya sabit kodlarsanız, kapsayıcı adı biçimi gelecekte değişirse API çağrısı başarısız olur.

<br>

```http
PUT https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AzureFiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;Storage;AzureFiles;testvault2?api-version=2016-12-01
```

Create istek gövdesi aşağıdaki gibidir:

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

İstek gövdesinin tanımlarının tam listesi ve diğer ayrıntılar için [ProtectionContainers-Register](https://docs.microsoft.com/rest/api/backup/protectioncontainers/register#azurestoragecontainer)bölümüne bakın.

Bu eşzamanlı bir işlemdir ve iki yanıt verir: "202 Kabul Edildi" işlemi kabul edildiğinde ve işlem tamamlandığında "200 Tamam".  İşlem durumunu izlemek için, işlemin en son durumunu almak için konum üstbilgisini kullanın.

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

Yanıt gövdesindeki *kayıt durumu* parametresinin değerinden kaydın başarılı olup olmadığını doğrulayabilirsiniz. Bizim durumumuzda, *testvault2*için kayıtlı olarak durumunu gösterir , bu nedenle kayıt işlemi başarılı oldu.

### <a name="inquire-all-unprotected-files-shares-under-a-storage-account"></a>Korumasız tüm dosya paylaşımlarını bir depolama hesabı altında sorgula

[Koruma Kapları-Sorgula](https://docs.microsoft.com/rest/api/backup/protectioncontainers/inquire) işlemini kullanarak bir depolama hesabındaki korunabilir öğeler hakkında bilgi alabilirsiniz. Bu bir eşzamanlı işlemdir ve sonuçlar konum üstbilgikullanılarak izlenmelidir.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/inquire?api-version=2016-12-01
```

Yukarıdaki URI değişkenlerini aşağıdaki gibi ayarlayın:

- {vaultName} - *azurefilesvault*
- {fabricName} - *Azure*
- {containerName}- GET ProtectableContainers işleminin yanıt gövdesindeki ad özniteliğine bakın. Örneğimizde, *StorageContainer; Depolama; AzureDosyalar;testvault2*

```http
https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;Storage;AzureFiles;testvault2/inquire?api-version=2016-12-01
```

İstek başarılı olduktan sonra , "Tamam" durum kodunu döndürür

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

### <a name="select-the-file-share-you-want-to-back-up"></a>Yedeklemek istediğiniz dosya paylaşımını seçin

Abonelik altında tüm korunabilir öğeleri listeleyebilir ve [GET backupprotectableItems](https://docs.microsoft.com/rest/api/backup/backupprotectableitems/list) işlemini kullanarak yedeklenecek istenilen dosya paylaşımını bulabilirsiniz.

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupProtectableItems?api-version=2016-12-01&$filter={$filter}
```

URI'yi aşağıdaki gibi inşa edin:

- {vaultName} - *azurefilesvault*
- {$filter} - *backupManagementType eq 'AzureStorage'*

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

Yanıt, tüm korumasız dosya paylaşımlarının listesini içerir ve yedeklemeyi yapılandırmak için Azure Kurtarma Hizmeti tarafından gerekli tüm bilgileri içerir.

### <a name="enable-backup-for-the-file-share"></a>Dosya paylaşımı için yedeklemeyi etkinleştirme

İlgili dosya paylaşımı dost adı ile "tanımlanır" sonra, korumak için ilke seçin. Kasadaki varolan ilkeler hakkında daha fazla bilgi edinmek için [İlke API listesine](https://docs.microsoft.com/rest/api/backup/backuppolicies/list)bakın. Ardından, ilke adına atıfta bulunarak [ilgili ilkeyi](https://docs.microsoft.com/rest/api/backup/protectionpolicies/get) seçin. İlkeler oluşturmak [için, ilke öğreticisi oluşturmak](https://docs.microsoft.com/azure/backup/backup-azure-arm-userestapi-createorupdatepolicy)için bakın.

Korumayı etkinleştirmek, "korunan öğe" oluşturan eşzamanlı bir *PUT* işlemidir.

```http
PUT https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}?api-version=2019-05-13
```

GET backupprotectableitems işleminin yanıt gövdesindeki kimlik özniteliğini kullanarak **kapsayıcı adı** ve **korumalı madde adı** değişkenlerini ayarlayın.

Örneğimizde, korumak istediğimiz dosya paylaşımının kimliği:

```output
"/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/storagecontainer;storage;azurefiles;testvault2/protectableItems/azurefileshare;testshare
```

- {containername} - *storagecontainer;storage;azurefiles;testvault2*
- {protectedItemName} - *azurefileshare;testshare*

Veya koruma kapsayıcısının **ad** özniteliğine ve korunabilir madde yanıtlarına başvurabilirsiniz.

>[!NOTE]
>Her zaman yanıtın ad özniteliğini alın ve bu istekle doldurun. Kapsayıcı adı biçimini veya korumalı madde adı biçimini sabit kodlamayı veya oluşturmayın. Oluşturur veya sabit kodlarsanız, kapsayıcı adı biçimi veya korumalı madde adı biçimi gelecekte değişirse API çağrısı başarısız olur.

<br>

```http
PUT https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;Storage;AzureFiles;testvault2/protectedItems/azurefileshare;testshare?api-version=2016-12-01
```

İstek gövdesi oluşturun:

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

**SourceResourceId,** GET backupprotectableItems yanıtı olarak **parentcontainerFabricID'dir.**

Örnek Yanıt

Korumalı bir öğenin oluşturulması, izlenmesi gereken başka bir işlem oluşturan eşzamanlı bir işlemdir. İki yanıt verir: başka bir işlem oluşturulduğunda 202 (Kabul) ve bu işlem tamamlandığında 200 (Tamam).

Korumalı madde oluşturma veya güncelleştirme için *PUT* isteğini gönderdikten veya güncelleştirseniz, ilk yanıt 202 (Kabul edilir) ve bir konum üstbilgisidir.

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

Ardından, get *komutuyla* konum üstbilgisini veya Azure-AsyncOperation üstbilgisini kullanarak ortaya çıkan işlemi izleyin.

```http
GET https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupOperations/c3a52d1d-0853-4211-8141-477c65740264?api-version=2016-12-01
```

İşlem tamamlandıktan sonra, yanıt gövdesindeki korumalı öğe içeriğiyle birlikte 200 (Tamam) döndürür.

Örnek Yanıt Gövdesi:

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

Bu, dosya paylaşımı için korumanın etkin olduğunu ve ilk yedeklemenin ilke zamanlamasına göre tetikleneceğini doğrular.

## <a name="trigger-an-on-demand-backup-for-file-share"></a>Dosya paylaşımı için isteğe bağlı yedeklemeyi tetikleme

Bir Azure dosya paylaşımı yedekleme için yapılandırıldıktan sonra, yedeklemeler ilke zamanlamasına göre çalışır. İlk zamanlanmış yedeklemeyi bekleyebilir veya isteğe bağlı yedeklemeyi istediğiniz zaman tetikleyebilirsiniz.

İsteğe bağlı yedeklemeyi tetiklemek bir POST işlemidir.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/backup?api-version=2016-12-01
```

{containerName} ve {protectedItemName} yedeklemeyi etkinleştirirken yukarıda oluşturulmuştur. Örneğimiz için, bu çevirir:

```http
POST https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;testvault2/protectedItems/AzureFileShare;testshare/backup?api-version=2017-07-01
```

### <a name="create-request-body"></a>İstek gövdesi oluşturma

İsteğe bağlı yedeklemeyi tetiklemek için, istek gövdesinin bileşenleri aşağıda veda edilir.

| Adı       | Tür                       | Açıklama                       |
| ---------- | -------------------------- | --------------------------------- |
| Özellikler | AzurefilesharebackupReques | BackupRequestResource özellikleri |

İstek gövdesinin tanımlarının tam listesi ve diğer ayrıntılar [için, korumalı öğeler REST API belgesi için tetikleyici yedeklemelere](https://docs.microsoft.com/rest/api/backup/backups/trigger#request-body)bakın.

İstek Gövdesi örneği

```json
{

  "properties":{

   "objectType":"AzureFileShareBackupRequest",
    "recoveryPointExpiryTimeInUTC":"2020-03-07T18:29:59.000Z"
}

}
```

### <a name="responses"></a>Yanıtlar

İsteğe bağlı yedeklemeyi tetiklemek eşzamanlı bir [işlemdir.](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) Bu işlem, ayrı olarak izlenmesi gereken başka bir işlem oluşturur anlamına gelir.

İki yanıt verir: başka bir işlem oluşturulduğunda 202 (Kabul) ve bu işlem tamamlandığında 200 (Tamam).

### <a name="example-responses"></a>Örnek yanıtlar

*İsteğe* bağlı yedekleme için POST isteğini gönderdikten sonra, ilk yanıt 202 (Kabul edilir) ve bir konum üstbilgisini veya Azure-async-üstbilgisini gösterir.

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

Ardından, get *komutuyla* konum üstbilgisini veya Azure-AsyncOperation üstbilgisini kullanarak ortaya çıkan işlemi izleyin.

```http
GET https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupOperations/dc62d524-427a-4093-968d-e951c0a0726e?api-version=2016-12-01
```

İşlem tamamlandıktan sonra, yanıt gövdesinde ortaya çıkan yedekleme işinin kimliğiyle birlikte 200 (Tamam) döndürür.

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

Yedekleme işi uzun süren bir işlem olduğundan, [REST API belgesini kullanarak monitör işlerinde](https://docs.microsoft.com/azure/backup/backup-azure-arm-userestapi-managejobs#tracking-the-job)açıklandığı gibi izlenmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Rest API'yi kullanarak Azure dosya paylaşımlarını nasıl geri yükleyeceğimiz](restore-azure-file-share-rest-api.md)hakkında bilgi edinin.
