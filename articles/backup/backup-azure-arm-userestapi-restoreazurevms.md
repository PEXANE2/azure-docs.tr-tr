---
title: REST API'sini kullanarak Azure VM'lerini geri yükleme
description: Bu makalede, REST API'yi kullanarak Azure Sanal Makine Yedekleme'nin geri yükleme işlemlerini nasıl yöneteceğimiz öğrenin.
ms.topic: conceptual
ms.date: 09/12/2018
ms.assetid: b8487516-7ac5-4435-9680-674d9ecf5642
ms.openlocfilehash: 4990d815721ddbdde8e6eb6ebf8d6d3b49adc700
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74173377"
---
# <a name="restore-azure-virtual-machines-using-rest-api"></a>REST API'yi kullanarak Azure Sanal makineleri geri yükleme

Azure Yedekleme'yi kullanan bir Azure sanal makinenin yedeklemesi tamamlandıktan sonra, aynı yedekleme kopyasından tüm Azure Sanal makineleri veya diskleri veya dosyaları geri yüklenebilir. Bu makalede, REST API kullanarak bir Azure VM veya diskler nasıl geri yükleyin açıklar.

Herhangi bir geri yükleme işlemi için, önce ilgili kurtarma noktasını tanımlamak gerekiyor.

## <a name="select-recovery-point"></a>Kurtarma noktasını seçin

Yedek öğenin kullanılabilir kurtarma noktaları liste [kurtarma noktası REST API](https://docs.microsoft.com/rest/api/backup/recoverypoints/list)kullanılarak listelenebilir. Tüm ilgili değerlere sahip basit bir *GET* işlemidir.

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints?api-version=2019-05-13
```

ve burada inşa edilmiş gibi . [here](backup-azure-arm-userestapi-backupazurevms.md#example-responses-1) `{containerName}` `{protectedItemName}` `{fabricName}`"Azure" ise.

*GET* URI gerekli tüm parametrelere sahiptir. Ek bir istek gövdesine gerek yoktur

### <a name="responses"></a>Yanıtlar

|Adı  |Tür  |Açıklama  |
|---------|---------|---------|
|200 TAMAM     |   [RecoveryPointResourceList](https://docs.microsoft.com/rest/api/backup/recoverypoints/list#recoverypointresourcelist)      |       Tamam  |

#### <a name="example-response"></a>Örnek yanıt

*GET* URI gönderildikten sonra 200 (Tamam) yanıtı döndürülür.

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: 03453538-2f8d-46de-8374-143ccdf60f40
x-ms-client-request-id: c48f4436-ce3f-42da-b537-12710d4d1a24; c48f4436-ce3f-42da-b537-12710d4d1a24
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14998
x-ms-correlation-request-id: 03453538-2f8d-46de-8374-143ccdf60f40
x-ms-routing-request-id: SOUTHINDIA:20180604T071851Z:03453538-2f8d-46de-8374-143ccdf60f40
Cache-Control: no-cache
Date: Mon, 04 Jun 2018 07:18:51 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/VM;testRG;testVM/recoveryPoints/20982486783671",
      "name": "20982486783671",
      "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints",
      "properties": {
        "objectType": "IaasVMRecoveryPoint",
        "recoveryPointType": "AppConsistent",
        "recoveryPointTime": "2018-06-04T06:06:00.5121087Z",
        "recoveryPointAdditionalInfo": "",
        "sourceVMStorageType": "NormalStorage",
        "isSourceVMEncrypted": false,
        "isInstantIlrSessionActive": false,
        "recoveryPointTierDetails": [
          {
            "type": 1,
            "status": 1
          },
          {
            "type": 2,
            "status": 1
          }
        ],
        "isManagedVirtualMachine": true,
        "virtualMachineSize": "Standard_A1_v2",
        "originalStorageAccountOption": false
      }
    },
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/VM;testRG;testVM/recoveryPoints/23358112038108",
      "name": "23358112038108",
      "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints",
      "properties": {
        "objectType": "IaasVMRecoveryPoint",
        "recoveryPointType": "CrashConsistent",
        "recoveryPointTime": "2018-06-03T06:20:56.3043878Z",
        "recoveryPointAdditionalInfo": "",
        "sourceVMStorageType": "NormalStorage",
        "isSourceVMEncrypted": false,
        "isInstantIlrSessionActive": false,
        "recoveryPointTierDetails": [
          {
            "type": 1,
            "status": 1
          },
          {
            "type": 2,
            "status": 1
          }
        ],
        "isManagedVirtualMachine": true,
        "virtualMachineSize": "Standard_A1_v2",
        "originalStorageAccountOption": false
      }
    },
......
```

Kurtarma noktası yukarıdaki yanıtta `{name}` alanla tanımlanır.

## <a name="restore-disks"></a>Diskleri geri yükleme

Yedekleme verilerinden bir VM oluşturulmasını özelleştirmek gerekirse, diskleri seçilen bir depolama hesabına geri yükleyebilir ve gereksinimlerine göre bu disklerden bir VM oluşturabilirsiniz. Depolama hesabı kurtarma hizmetleri kasası ile aynı bölgede olmalıdır ve bölge gereksiz olmamalıdır. Diskler ve yedeklenmiş VM yapılandırması ("vmconfig.json") verilen depolama hesabında depolanır.

Geri yükleme disklerini tetiklemek bir *POST* isteğidir. Geri Yükleme diskleri işlemi hakkında daha fazla bilgi için ["tetikleyici geri yükleme" REST API'ye](https://docs.microsoft.com/rest/api/backup/restores/trigger)bakın.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints/{recoveryPointId}/restore?api-version=2019-05-13
```

ve burada inşa edilmiş gibi . [here](backup-azure-arm-userestapi-backupazurevms.md#example-responses-1) `{containerName}` `{protectedItemName}` `{fabricName}`"Azure" `{recoveryPointId}` ve [yukarıda](#example-response) `{name}` belirtilen kurtarma noktasının alanıdır.

### <a name="create-request-body"></a>İstek gövdesi oluşturma

Azure VM yedeklemesinden disk geri yüklemesini tetiklemek için istek gövdesinin bileşenleri şunlardır.

|Adı  |Tür  |Açıklama  |
|---------|---------|---------|
|properties     | [IaaSVMRestoreRequest](https://docs.microsoft.com/rest/api/backup/restores/trigger#iaasvmrestorerequest)        |    Geri Yüklemeİstek Kaynak Özellikleri     |

İstek gövdesinin tanımlarının tam listesi ve diğer ayrıntılar için REST [API belgesini geri yükleme'yi tetiklemeye](https://docs.microsoft.com/rest/api/backup/restores/trigger#request-body)bakın.

#### <a name="example-request"></a>Örnek istek

Aşağıdaki istek gövdesi, disk geri yüklemesini tetiklemek için gereken özellikleri tanımlar.

```json
{
  "properties": {
    "objectType": "IaasVMRestoreRequest",
    "recoveryPointId": "20982486783671",
    "recoveryType": "RestoreDisks",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "storageAccountId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Storage/storageAccounts/testAccount",
    "region": "westus",
    "createNewCloudService": false,
    "originalStorageAccountOption": false,
    "encryptionDetails": {
      "encryptionEnabled": false
    }
  }
}
```

### <a name="response"></a>Yanıt

Geri yükleme diskinin tetikleme [bir eşzamanlı işlemdir.](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) Bu işlem, ayrı olarak izlenmesi gereken başka bir işlem oluşturur anlamına gelir.

İki yanıt verir: başka bir işlem oluşturulduğunda 202 (Kabul) ve bu işlem tamamlandığında 200 (Tamam).

|Adı  |Tür  |Açıklama  |
|---------|---------|---------|
|202 Kabul Edildi     |         |     Accepted    |

#### <a name="example-responses"></a>Örnek yanıtlar

Geri yükleme disklerini tetiklemek için *POST* URI'yi gönderdikten sonra, ilk yanıt 202 (Kabul) ve konum üstbilgisini veya Azure-async-üstbilgisini gösterir.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/781a0f18-e250-4d73-b059-5e9ffed4069e?api-version=2019-05-13
X-Content-Type-Options: nosniff
x-ms-request-id: 893fe372-8d6c-4c56-b589-45a95eeef95f
x-ms-client-request-id: a15ce064-25bd-4ac6-87e5-e3bc6ec65c0b; a15ce064-25bd-4ac6-87e5-e3bc6ec65c0b
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1198
x-ms-correlation-request-id: 893fe372-8d6c-4c56-b589-45a95eeef95f
x-ms-routing-request-id: SOUTHINDIA:20180604T130003Z:893fe372-8d6c-4c56-b589-45a95eeef95f
Cache-Control: no-cache
Date: Mon, 04 Jun 2018 13:00:03 GMT
Location: https://management.azure.com/subscriptions//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationResults/781a0f18-e250-4d73-b059-5e9ffed4069e?api-version=2019-05-13
X-Powered-By: ASP.NET
```

Ardından, basit bir *GET* komutuyla konum üstbilgisini veya Azure-AsyncOperation üstbilgisini kullanarak ortaya çıkan işlemi izleyin.

```http
GET https://management.azure.com/subscriptions//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationResults/781a0f18-e250-4d73-b059-5e9ffed4069e?api-version=2019-05-13
```

İşlem tamamlandıktan sonra, yanıt gövdesinde ortaya çıkan geri yükleme işinin kimliğiyle birlikte 200 (Tamam) döndürür.

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: ea2a8011-eb83-4a4b-9ed2-e694070a966a
x-ms-client-request-id: a7f3a144-ed80-41ee-bffe-ae6a90c35a2f; a7f3a144-ed80-41ee-bffe-ae6a90c35a2f
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14973
x-ms-correlation-request-id: ea2a8011-eb83-4a4b-9ed2-e694070a966a
x-ms-routing-request-id: SOUTHINDIA:20180604T130917Z:ea2a8011-eb83-4a4b-9ed2-e694070a966a
Cache-Control: no-cache
Date: Mon, 04 Jun 2018 13:09:17 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "id": "781a0f18-e250-4d73-b059-5e9ffed4069e",
  "name": "781a0f18-e250-4d73-b059-5e9ffed4069e",
  "status": "Succeeded",
  "startTime": "2018-06-04T13:00:03.8068176Z",
  "endTime": "2018-06-04T13:00:03.8068176Z",
  "properties": {
    "objectType": "OperationStatusJobExtendedInfo",
    "jobId": "3021262a-fb3a-4538-9b37-e3e97a386093"
  }
}
```

Yedekleme işi uzun süren bir işlem olduğundan, [REST API belgesini kullanarak monitör işlerinde](backup-azure-arm-userestapi-managejobs.md#tracking-the-job)açıklandığı gibi izlenmelidir.

Uzun süren iş tamamlandıktan sonra, diskler ve yedeklenen sanal makinenin yapılandırması ("VMConfig.json") verilen depolama hesabında bulunur.

## <a name="restore-as-another-virtual-machine"></a>Başka bir sanal makine olarak geri yükleme

[Kurtarma noktasını seçin](#select-recovery-point) ve kurtarma noktasından gelen verileri içeren başka bir Azure Sanal makinesi oluşturmak için aşağıda belirtildiği gibi istek gövdesioluşturun.

Aşağıdaki istek gövdesi, sanal makine geri yüklemesini tetiklemek için gereken özellikleri tanımlar.

```json
{
  "parameters": {
        "subscriptionId": "00000000-0000-0000-0000-000000000000",
        "resourceGroupName": "testVaultRG",
        "vaultName": "testVault",
        "fabricName": "Azure",
        "containerName": "IaasVMContainer;iaasvmcontainerv2;testRG;testVM",
        "protectedItemName": "VM;iaasvmcontainerv2;testRG;testVM",
        "recoveryPointId": "348916168024334",
        "api-version": "2019-05-13",
      "parameters": {
        "properties": {
          "objectType":  "IaasVMRestoreRequest",
          "recoveryPointId": "348916168024334",
          "recoveryType": "AlternateLocation",
          "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
          "targetVirtualMachineId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG/providers/Microsoft.Compute/virtualmachines/targetVM",
          "targetResourceGroupId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG",
          "storageAccountId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Storage/storageAccounts/testingAccount",
          "virtualNetworkId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG/providers/Microsoft.Network/virtualNetworks/testNet",
          "subnetId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG/providers/Microsoft.Network/virtualNetworks/testNet/subnets/default",
          "region": "westus",
          "createNewCloudService": false,
          "originalStorageAccountOption": false,
          "encryptionDetails": {
            "encryptionEnabled": false
          }
        }
      }
    }
}
```

Yanıt, [diskleri geri için yukarıda açıklandığı](#response)şekilde ele alınmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Azure Yedekleme REST API'leri hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

- [Azure Kurtarma Hizmetleri sağlayıcısı REST API](/rest/api/recoveryservices/)
- [Azure REST API’yi kullanmaya başlayın](/rest/api/azure/)
