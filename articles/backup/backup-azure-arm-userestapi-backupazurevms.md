---
title: REST API'sini kullanarak Azure VM'lerini yedekleme
description: Bu makalede, REST API'yi kullanarak Azure VM Yedeklemeyedekleme işlemlerini nasıl yapılandırılamayın, başlatın ve yönetebilirsiniz.
ms.topic: conceptual
ms.date: 08/03/2018
ms.assetid: b80b3a41-87bf-49ca-8ef2-68e43c04c1a3
ms.openlocfilehash: 4789ef1e0e09df521f8cab539d972e9e669e0a58
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248170"
---
# <a name="back-up-an-azure-vm-using-azure-backup-via-rest-api"></a>REST API ile Azure Yedekleme'yi kullanarak Azure VM'yi yedekleme

Bu makalede, REST API üzerinden Azure Yedekleme kullanarak bir Azure VM yedeklemenasıl yönetilir açıklanmaktadır. Korumayı daha önce korunmasız bir Azure VM için ilk kez yapılandırın, korumalı bir Azure VM için isteğe bağlı yedekleme tetikleyebilir ve burada açıklandığı gibi YEDEKlenmiş bir VM'nin yedekleme özelliklerini REST API ile değiştirin.

Yeni [kasalar](backup-azure-arm-userestapi-createorupdatevault.md) ve ilkeler oluşturmak için kasa oluşturmak ve [ilke](backup-azure-arm-userestapi-createorupdatepolicy.md) REST API öğreticiler oluşturmak için bakın.

Bir VM "testVM"i, varsayılan ilke ("DefaultPolicy" olarak adlandırılır) ile "testVaultRG" kaynak grubu içinde bulunan Kurtarma Hizmetleri kasası "testVault"a "testRG" altında korumak istediğinizi varsayalım.

## <a name="configure-backup-for-an-unprotected-azure-vm-using-rest-api"></a>REST API'yi kullanarak korumasız bir Azure VM için yedeklemeyi yapılandırma

### <a name="discover-unprotected-azure-vms"></a>Korumasız Azure VM'lerini keşfedin

İlk olarak, kasa Azure VM'yi tanımlayabilmeli. Bu, [yenileme işlemi](https://docs.microsoft.com/rest/api/backup/protectioncontainers/refresh)kullanılarak tetiklenir. Kasanın geçerli abonelikteki tüm korumasız VM'lerin en son listesini almasını ve bunları 'önbelleğe almasını' sağlayan bir eşzamanlı *POST* işlemidir. VM 'önbelleğe alındıktan' sonra, Kurtarma hizmetleri VM'ye erişebilir ve onu koruyabilir.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupname}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/refreshContainers?api-version=2016-12-01
```

POST URI'nin `{vaultName}` `{vaultresourceGroupName}` `{fabricName}` parametreleri vardır. `{subscriptionId}` "Azure" `{fabricName}` ise. Örneğimize göre, `{vaultName}` "testVault" `{vaultresourceGroupName}` ve "testVaultRG". URI'de gerekli tüm parametreler verildiğinden, ayrı bir istek organına gerek yoktur.

```http
POST https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/refreshContainers?api-version=2016-12-01
```

#### <a name="responses"></a>Yanıtlar

'Yenileme' işlemi eşzamanlı bir [işlemdir.](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) Bu işlem, ayrı olarak izlenmesi gereken başka bir işlem oluşturur anlamına gelir.

İki yanıt verir: başka bir işlem oluşturulduğunda 202 (Kabul) ve bu işlem tamamlandığında 200 (Tamam).

|Adı  |Tür  |Açıklama  |
|---------|---------|---------|
|204 İçerik Yok     |         |  Hiçbir içerik döndürülmeden Tamam      |
|202 Kabul Edildi     |         |     Accepted    |

##### <a name="example-responses"></a>Örnek yanıtlar

*POST* isteği gönderildikten sonra, 202 (Kabul edilen) bir yanıt döndürülür.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
X-Content-Type-Options: nosniff
x-ms-request-id: 43cf550d-e463-421c-8922-37e4766db27d
x-ms-client-request-id: 4910609f-bb9b-4c23-8527-eb6fa2d3253f; 4910609f-bb9b-4c23-8527-eb6fa2d3253f
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 43cf550d-e463-421c-8922-37e4766db27d
x-ms-routing-request-id: SOUTHINDIA:20180521T105701Z:43cf550d-e463-421c-8922-37e4766db27d
Cache-Control: no-cache
Date: Mon, 21 May 2018 10:57:00 GMT
Location: https://management.azure.com/subscriptions//00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/operationResults/aad204aa-a5cf-4be2-a7db-a224819e5890?api-version=2019-05-13
X-Powered-By: ASP.NET
```

Basit bir *GET* komutuyla "Konum" üstbilgisini kullanarak ortaya çıkan işlemi izleme

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/operationResults/aad204aa-a5cf-4be2-a7db-a224819e5890?api-version=2019-05-13
```

Tüm Azure VM'leri keşfedildikten sonra GET komutu 204 (İçerik Yok) yanıtını döndürür. Kasa artık abonelik içinde herhangi bir VM keşfedebilir.

```http
HTTP/1.1 204 NoContent
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: cf6cd73b-9189-4942-a61d-878fcf76b1c1
x-ms-client-request-id: 25bb6345-f9fc-4406-be1a-dc6db0eefafe; 25bb6345-f9fc-4406-be1a-dc6db0eefafe
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14997
x-ms-correlation-request-id: cf6cd73b-9189-4942-a61d-878fcf76b1c1
x-ms-routing-request-id: SOUTHINDIA:20180521T105825Z:cf6cd73b-9189-4942-a61d-878fcf76b1c1
Cache-Control: no-cache
Date: Mon, 21 May 2018 10:58:25 GMT
X-Powered-By: ASP.NET
```

### <a name="selecting-the-relevant-azure-vm"></a>İlgili Azure VM'yi seçme

 "Önbelleğe alma"nın, abonelik altındaki [tüm korunabilir öğeleri listeleyerek](https://docs.microsoft.com/rest/api/backup/backupprotectableitems/list) yapıldığını onaylayabilir ve yanıtta istenen VM'yi bulabilirsiniz. [Bu işlemin yanıtı,](#example-responses-1) Kurtarma Hizmetlerinin bir VM'yi nasıl tanımladığı hakkında da bilgi verir.  Deseni tanıdıktan sonra, bu adımı atlayabilir ve doğrudan [korumayı etkinleştirmeye devam edebilirsiniz.](#enabling-protection-for-the-azure-vm)

Bu işlem bir *GET* işlemidir.

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupProtectableItems?api-version=2016-12-01&$filter=backupManagementType eq 'AzureIaasVM'
```

*GET* URI gerekli tüm parametrelere sahiptir. Ek istek gövdesi gerekmez.

#### <a name="responses"></a><a name="responses-1"></a>Yanıtlar

|Adı  |Tür  |Açıklama  |
|---------|---------|---------|
|200 TAMAM     | [İş YüküProtectableItemResourceList](https://docs.microsoft.com/rest/api/backup/backupprotectableitems/list#workloadprotectableitemresourcelist)        |       Tamam |

#### <a name="example-responses"></a><a name="example-responses-1"></a>Örnek yanıtlar

*GET* isteği gönderildikten sonra, 200 (Tamam) yanıtı döndürülür.

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: 7c2cf56a-e6be-4345-96df-c27ed849fe36
x-ms-client-request-id: 40c8601a-c217-4c68-87da-01db8dac93dd; 40c8601a-c217-4c68-87da-01db8dac93dd
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14979
x-ms-correlation-request-id: 7c2cf56a-e6be-4345-96df-c27ed849fe36
x-ms-routing-request-id: SOUTHINDIA:20180521T071408Z:7c2cf56a-e6be-4345-96df-c27ed849fe36
Cache-Control: no-cache
Date: Mon, 21 May 2018 07:14:08 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainerv2;testRG;testVM/protectableItems/vm;iaasvmcontainerv2;testRG;testVM",
      "name": "iaasvmcontainerv2;testRG;testVM",
      "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectableItems",
      "properties": {
        "virtualMachineId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
        "virtualMachineVersion": "Compute",
        "resourceGroup": "testRG",
        "backupManagementType": "AzureIaasVM",
        "protectableItemType": "Microsoft.Compute/virtualMachines",
        "friendlyName": "testVM",
        "protectionState": "NotProtected"
      }
    },……………..

```

> [!TIP]
> *GET* yanıtındaki değer sayısı bir 'sayfa' için 200 ile sınırlıdır. Sonraki yanıt kümesinin URL'sini almak için 'nextLink' alanını kullanın.

Yanıt, korumasız tüm Azure VM'lerinin `{value}` listesini içerir ve her biri yedeklemeyi yapılandırmak için Azure Kurtarma Hizmeti tarafından gereken tüm bilgileri içerir. Yedeklemeyi yapılandırmak için, bölümdeki `{name}` `{virtualMachineId}` `{properties}` alanı ve alanı not edin. Aşağıda belirtildiği gibi bu alan değerlerinden iki değişken oluşturun.

- containerName = "iaasvmcontainer;" +`{name}`
- protectedItemName = "vm;" +`{name}`
- `{virtualMachineId}`[istek gövdesinde](#example-request-body) daha sonra kullanılır

Örnekte, yukarıdaki değerler şu şekilde çevrilebilir:

- containerName = "iaasvmcontainer;iaasvmcontainerv2;testRG;testVM"
- protectedItemName = "vm;iaasvmcontainerv2;testRG;testVM"

### <a name="enabling-protection-for-the-azure-vm"></a>Azure VM için koruma sağlama

İlgili VM "önbelleğe alındıktan" ve "tanımlandıktan" sonra, korumak için ilkeyi seçin. Kasadaki varolan ilkeler hakkında daha fazla bilgi için [Liste İlkesi API'sine](https://docs.microsoft.com/rest/api/backup/backuppolicies/list)bakın. Ardından, ilke adına atıfta bulunarak [ilgili ilkeyi](/rest/api/backup/protectionpolicies/get) seçin. İlkeler oluşturmak [için, ilke öğreticisi oluşturmak](backup-azure-arm-userestapi-createorupdatepolicy.md)için bakın. Aşağıdaki örnekte "DefaultPolicy" seçilir.

Korumayı etkinleştirmek, 'korumalı öğe' oluşturan eşzamanlı bir *PUT* işlemidir.

```http
https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}?api-version=2019-05-13
```

`{containerName}` ve `{protectedItemName}` yukarıda inşa edilmiş gibi. "Azure" `{fabricName}` ise. Örneğimiz için, bu çevirir:

```http
PUT https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;iaasvmcontainerv2;testRG;testVM?api-version=2019-05-13
```

#### <a name="create-the-request-body"></a>İstek gövdesini oluşturma

Korumalı bir öğe oluşturmak için, istek gövdesinin bileşenleri aşağıda veda edilir.

|Adı  |Tür  |Açıklama  |
|---------|---------|---------|
|properties     | AzureIaaSVMProtectedItem        |ProtectedItem Kaynak özellikleri         |

İstek gövdesinin tanımlarının tam listesi ve diğer ayrıntılar [için, korumalı madde REST API belgesi oluşturmaya](https://docs.microsoft.com/rest/api/backup/protecteditems/createorupdate#request-body)bakın.

##### <a name="example-request-body"></a>Örnek istek gövdesi

Aşağıdaki istek gövdesi, korumalı bir öğe oluşturmak için gereken özellikleri tanımlar.

```json
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/DefaultPolicy"
  }
}
```

Liste `{sourceResourceId}` `{virtualMachineId}` [korunabilir öğelerin yanıtı](#example-responses-1)yukarıda belirtilen .

#### <a name="responses"></a>Yanıtlar

Korumalı bir öğenin oluşturulması [bir eşzamanlı işlemdir.](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) Bu işlem, ayrı olarak izlenmesi gereken başka bir işlem oluşturur anlamına gelir.

İki yanıt verir: başka bir işlem oluşturulduğunda 202 (Kabul) ve bu işlem tamamlandığında 200 (Tamam).

|Adı  |Tür  |Açıklama  |
|---------|---------|---------|
|200 TAMAM     |    [Korumalı Madde Kaynağı](https://docs.microsoft.com/rest/api/backup/protecteditemoperationresults/get#protecteditemresource)     |  Tamam       |
|202 Kabul Edildi     |         |     Accepted    |

##### <a name="example-responses"></a>Örnek yanıtlar

Korumalı madde oluşturma veya güncelleştirme için *PUT* isteğini gönderdikten sonra, ilk yanıt 202 (Kabul edilir) ve bir konum üstbilgisi veya Azure-async-üstbilgidir.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2019-05-13
X-Content-Type-Options: nosniff
x-ms-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-client-request-id: e1f94eef-9b2d-45c4-85b8-151e12b07d03; e1f94eef-9b2d-45c4-85b8-151e12b07d03
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-routing-request-id: SOUTHINDIA:20180521T073907Z:db785be0-bb20-4598-bc9f-70c9428b170b
Cache-Control: no-cache
Date: Mon, 21 May 2018 07:39:06 GMT
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationResults/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2019-05-13
X-Powered-By: ASP.NET
```

Ardından, basit bir *GET* komutuyla konum üstbilgisini veya Azure-AsyncOperation üstbilgisini kullanarak ortaya çıkan işlemi izleyin.

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2019-05-13
```

İşlem tamamlandıktan sonra, yanıt gövdesindeki korumalı öğe içeriğiyle birlikte 200 (Tamam) döndürür.

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM",
  "name": "VM;testRG;testVM",
  "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems",
  "properties": {
    "friendlyName": "testVM",
    "virtualMachineId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "protectionStatus": "Healthy",
    "protectionState": "IRPending",
    "healthStatus": "Passed",
    "lastBackupStatus": "",
    "lastBackupTime": "2001-01-01T00:00:00Z",
    "protectedItemDataId": "17592691116891",
    "extendedInfo": {
      "recoveryPointCount": 0,
      "policyInconsistent": false
    },
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "backupManagementType": "AzureIaasVM",
    "workloadType": "VM",
    "containerName": "iaasvmcontainerv2;testRG;testVM",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/DefaultPolicy",
    "policyName": "DefaultPolicy"
  }
}
```

Bu, VM için korumanın etkinleştirildiğini ve ilk yedeklemenin ilke zamanlamasına göre tetikleneceğini doğrular.

## <a name="trigger-an-on-demand-backup-for-a-protected-azure-vm"></a>Korumalı bir Azure VM için isteğe bağlı yedekleme yi tetikleme

Bir Azure VM yedekleme için yapılandırıldıktan sonra, yedeklemeler ilke zamanlamasına göre gerçekleşir. İlk zamanlanmış yedeklemeyi bekleyebilir veya isteğe bağlı yedeklemeyi istediğiniz zaman tetikleyebilirsiniz. İsteğe bağlı yedeklemeler için bekletme, yedekleme ilkesinin bekletilmesinden ayrıdır ve belirli bir tarih saatine belirtilebilir. Belirtilmemişse, isteğe bağlı yedeklemenin tetiklediği günden itibaren 30 gün olduğu varsayılır.

İsteğe bağlı yedeklemeyi tetiklemek bir *POST* işlemidir.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/backup?api-version=2016-12-01
```

ve yukarıda inşa edilmiş gibi . [above](#responses-1) `{containerName}` `{protectedItemName}` "Azure" `{fabricName}` ise. Örneğimiz için, bu çevirir:

```http
POST https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;iaasvmcontainerv2;testRG;testVM/backup?api-version=2016-12-01
```

### <a name="create-the-request-body"></a>İstek gövdesini oluşturma

İsteğe bağlı yedeklemeyi tetiklemek için, istek gövdesinin bileşenleri aşağıda veda edilir.

|Adı  |Tür  |Açıklama  |
|---------|---------|---------|
|properties     | [IaaSVMBackupRequest](https://docs.microsoft.com/rest/api/backup/backups/trigger#iaasvmbackuprequest)        |BackupRequestResource özellikleri         |

İstek gövdesinin tanımlarının tam listesi ve diğer ayrıntılar [için, korumalı öğeler REST API belgesi için tetikleyici yedeklemelere](https://docs.microsoft.com/rest/api/backup/backups/trigger#request-body)bakın.

#### <a name="example-request-body"></a>Örnek istek gövdesi

Aşağıdaki istek gövdesi, korumalı bir öğe için yedeklemeyi tetiklemek için gereken özellikleri tanımlar. Bekletme belirtilmemişse, yedekleme işinin tetiklenme tarihinden itibaren 30 gün boyunca saklanır.

```json
{
   "properties": {
    "objectType": "IaasVMBackupRequest",
    "recoveryPointExpiryTimeInUTC": "2018-12-01T02:16:20.3156909Z"
  }
}
```

### <a name="responses"></a>Yanıtlar

İsteğe bağlı yedeklemeyi tetiklemek eşzamanlı bir [işlemdir.](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) Bu işlem, ayrı olarak izlenmesi gereken başka bir işlem oluşturur anlamına gelir.

İki yanıt verir: başka bir işlem oluşturulduğunda 202 (Kabul) ve bu işlem tamamlandığında 200 (Tamam).

|Adı  |Tür  |Açıklama  |
|---------|---------|---------|
|202 Kabul Edildi     |         |     Accepted    |

#### <a name="example-responses"></a><a name="example-responses-3"></a>Örnek yanıtlar

*İsteğe* bağlı yedekleme için POST isteğini gönderdikten sonra, ilk yanıt 202 (Kabul edilir) ve bir konum üstbilgisini veya Azure-async-üstbilgisini gösterir.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testVaultRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/b8daecaa-f8f5-44ed-9f18-491a9e9ba01f?api-version=2019-05-13
X-Content-Type-Options: nosniff
x-ms-request-id: 7885ca75-c7c6-43fb-a38c-c0cc437d8810
x-ms-client-request-id: 7df8e874-1d66-4f81-8e91-da2fe054811d; 7df8e874-1d66-4f81-8e91-da2fe054811d
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 7885ca75-c7c6-43fb-a38c-c0cc437d8810
x-ms-routing-request-id: SOUTHINDIA:20180521T083541Z:7885ca75-c7c6-43fb-a38c-c0cc437d8810
Cache-Control: no-cache
Date: Mon, 21 May 2018 08:35:41 GMT
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testVaultRG;testVM/protectedItems/vm;testRG;testVM/operationResults/b8daecaa-f8f5-44ed-9f18-491a9e9ba01f?api-version=2019-05-13
X-Powered-By: ASP.NET
```

Ardından, basit bir *GET* komutuyla konum üstbilgisini veya Azure-AsyncOperation üstbilgisini kullanarak ortaya çıkan işlemi izleyin.

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2019-05-13
```

İşlem tamamlandıktan sonra, yanıt gövdesinde ortaya çıkan yedekleme işinin kimliğiyle birlikte 200 (Tamam) döndürür.

```json
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: a8b13524-2c95-445f-b107-920806f385c1
x-ms-client-request-id: 5a63209d-3708-4e69-a75f-9499f4c8977c; 5a63209d-3708-4e69-a75f-9499f4c8977c
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14995
x-ms-correlation-request-id: a8b13524-2c95-445f-b107-920806f385c1
x-ms-routing-request-id: SOUTHINDIA:20180521T083723Z:a8b13524-2c95-445f-b107-920806f385c1
Cache-Control: no-cache
Date: Mon, 21 May 2018 08:37:22 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "00000000-0000-0000-0000-000000000000",
  "status": "Succeeded",
  "startTime": "2018-05-21T08:35:40.9488967Z",
  "endTime": "2018-05-21T08:35:40.9488967Z",
  "properties": {
    "objectType": "OperationStatusJobExtendedInfo",
    "jobId": "7ddead57-bcb9-4269-ac31-6a1b57588700"
  }
}
```

Yedekleme işi uzun süren bir işlem olduğundan, [REST API belgesini kullanarak monitör işlerinde](backup-azure-arm-userestapi-managejobs.md#tracking-the-job)açıklandığı gibi izlenmesi gerekir.

## <a name="modify-the-backup-configuration-for-a-protected-azure-vm"></a>Korumalı bir Azure VM için yedekleme yapılandırmasını değiştirme

### <a name="changing-the-policy-of-protection"></a>Koruma ilkesini değiştirme

VM'nin korunduğu ilkeyi değiştirmek [için, korumayı etkinleştirmek](#enabling-protection-for-the-azure-vm)için aynı biçimi kullanabilirsiniz. [İstek gövdesinde](#example-request-body) yeni ilke kimliğini sağlaman ve isteği göndermeniz. Örneğin: testVM ilkesini 'DefaultPolicy'den 'ProdPolicy'ye değiştirmek için istek gövdesinde 'ProdPolicy' kimliğini sağlayın.

```http
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/ProdPolicy"
  }
}
```

Yanıt, [korumayı etkinleştirmek için](#responses-2) bahsedilen formatı izleyecek

### <a name="stop-protection-but-retain-existing-data"></a>Korumayı durdurun, ancak varolan verileri koruyun

Korumalı bir VM'de korumayı kaldırmak, ancak zaten yedeklenmiş verileri korumak için istek gövdesindeki ilkeyi kaldırın ve isteği gönderin. İlkeyle ilişkilendirme kaldırıldıktan sonra yedeklemeler artık tetiklenmez ve yeni kurtarma noktaları oluşturulmaz.

```http
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": ""
  }
}
```

Yanıt, [isteğe bağlı yedeklemeyi tetiklemek için](#example-responses-3)bahsedilen biçimi izler. Ortaya çıkan [iş, REST API belgesi kullanılarak monitör işlerinde](backup-azure-arm-userestapi-managejobs.md#tracking-the-job)açıklandığı gibi izlenmelidir.

### <a name="stop-protection-and-delete-data"></a>Korumayı durdurma ve verileri silme

Korumalı bir VM'deki korumayı kaldırmak ve yedekleme verilerini de silmek [için, burada](https://docs.microsoft.com/rest/api/backup/protecteditems/delete)ayrıntılı olarak belirtildiği gibi bir silme işlemi gerçekleştirin.

Korumayı durdurma ve verileri silme işlemibir *DELETE* işlemidir.

```http
DELETE https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}?api-version=2019-05-13
```

ve yukarıda inşa edilmiş gibi . [above](#responses-1) `{containerName}` `{protectedItemName}` `{fabricName}`"Azure" ise. Örneğimiz için, bu çevirir:

```http
DELETE https://management.azure.com//Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;iaasvmcontainerv2;testRG;testVM?api-version=2019-05-13
```

#### <a name="responses"></a><a name="responses-2"></a>Yanıtlar

*DELETE* koruması [eşzamanlı](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations)bir işlemdir. Bu işlem, ayrı olarak izlenmesi gereken başka bir işlem oluşturur anlamına gelir.

İki yanıt verir: başka bir işlem oluşturulduğunda 202 (Kabul) ve bu işlem tamamlandığında 204 (NoContent).

|Adı  |Tür  |Açıklama  |
|---------|---------|---------|
|204 NoContent     |         |  İçerik Yok       |
|202 Kabul Edildi     |         |     Accepted    |

> [!IMPORTANT]
> Yanlışlıkla silme senaryolarına karşı korunmak için Kurtarma hizmetleri kasası için [bir yumuşak silme özelliği vardır.](use-restapi-update-vault-properties.md#soft-delete-state) Kasanın yumuşak silme durumu etkin olarak ayarlanmışsa, silme işlemi verileri hemen silmez. 14 gün boyunca saklanacak ve sonra kalıcı olarak tasfiye edilecek. Müşteri bu 14 günlük süre için depolama için ücret alınmaz. Silme işlemini geri almak için, [geri-silme bölümüne](#undo-the-stop-protection-and-delete-data)bakın.

### <a name="undo-the-stop-protection-and-delete-data"></a>Durdurma korumasını geri alave ve verileri silme

Yanlışlıkla silme geri alma yedekleme öğesi oluşturmaya benzer. Silme işlemini geri aldıktan sonra öğe korunur, ancak gelecekteki yedeklemeler tetiklenmez.

Silme silme işlemi, [ilkeyi değiştirmeye](#changing-the-policy-of-protection) ve/veya [korumayı etkinleştirmeye](#enabling-protection-for-the-azure-vm)çok benzeyen bir *PUT* işlemidir. Sadece [istek gövdesinde](#example-request-body) değişken *isRehydrate* ile silme geri almak ve istek gönderin niyeti sağlayın. Örneğin: testVM için silme işlemini geri almak için aşağıdaki istek gövdesi kullanılmalıdır.

```http
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "protectionState": "ProtectionStopped",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "isRehydrate": true
  }
}
```

Yanıt, [isteğe bağlı yedeklemeyi tetiklemek için](#example-responses-3)bahsedilen biçimi izler. Ortaya çıkan [iş, REST API belgesi kullanılarak monitör işlerinde](backup-azure-arm-userestapi-managejobs.md#tracking-the-job)açıklandığı gibi izlenmelidir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Sanal makine yedeklemesinden veri geri yükleyin.](backup-azure-arm-userestapi-restoreazurevms.md)

Azure Yedekleme REST API'leri hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

- [Azure Kurtarma Hizmetleri sağlayıcısı REST API](/rest/api/recoveryservices/)
- [Azure REST API’yi kullanmaya başlayın](/rest/api/azure/)
