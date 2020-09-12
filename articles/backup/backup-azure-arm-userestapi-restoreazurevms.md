---
title: REST API kullanarak Azure VM 'lerini geri yükleme
description: Bu makalede, REST API kullanarak Azure sanal makine yedekleme 'nin geri yükleme işlemlerini yönetmeyi öğrenin.
ms.topic: conceptual
ms.date: 09/12/2018
ms.assetid: b8487516-7ac5-4435-9680-674d9ecf5642
ms.openlocfilehash: ad60436d82ccc8049a4509ba5bf1e244bee150ea
ms.sourcegitcommit: 655e4b75fa6d7881a0a410679ec25c77de196ea3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2020
ms.locfileid: "89506687"
---
# <a name="restore-azure-virtual-machines-using-rest-api"></a>REST API kullanarak Azure sanal makinelerini geri yükleme

Azure Backup kullanarak bir Azure sanal makinesini yedekleme işlemi tamamlandıktan sonra, biri tüm Azure sanal makinelerini veya diskleri veya dosyalarını aynı yedek kopyadan geri yükleyebilir. Bu makalede REST API kullanarak bir Azure VM veya disklerinin nasıl geri yükleneceği açıklanmaktadır.

Herhangi bir geri yükleme işlemi için, bunlardan önce ilgili kurtarma noktasını tanımlaması gerekir.

## <a name="select-recovery-point"></a>Kurtarma noktası seçin

Bir yedekleme öğesinin kullanılabilir kurtarma noktaları, [Liste kurtarma noktası REST API](/rest/api/backup/recoverypoints/list)kullanılarak listelenebilir. Bu, tüm ilgili değerleri içeren basit bir *Get* işlemidir.

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints?api-version=2019-05-13
```

`{containerName}`Ve `{protectedItemName}` [burada](backup-azure-arm-userestapi-backupazurevms.md#example-responses-to-get-operation)oluşturulur. `{fabricName}` "Azure" dır.

*Get* URI 'sinin tüm gerekli parametreleri vardır. Ek bir istek gövdesi gerekmez.

### <a name="responses"></a>Yanıtlar

|Ad  |Tür  |Description  |
|---------|---------|---------|
|200 TAMAM     |   [RecoveryPointResourceList](/rest/api/backup/recoverypoints/list#recoverypointresourcelist)      |       Tamam  |

#### <a name="example-response"></a>Örnek yanıt

*Get* URI 'si gönderildikten sonra bir 200 (Tamam) yanıtı döndürülür.

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

Kurtarma noktası `{name}` Yukarıdaki yanıttaki alanla tanımlanır.

## <a name="restore-operations"></a>Geri yükleme işlemleri

[İlgili geri yükleme noktasını](#select-recovery-point)seçtikten sonra geri yükleme işlemini tetiklemeye devam edin.

***Yedekleme öğesindeki tüm geri yükleme işlemleri aynı *Post* API 'siyle gerçekleştirilir. Geri yükleme senaryolarıyla yalnızca istek gövdesi değişir.***

> [!IMPORTANT]
> Çeşitli geri yükleme seçenekleri ve bunların bağımlılıklarıyla ilgili tüm ayrıntılar [burada](https://docs.microsoft.com/azure/backup/backup-azure-arm-restore-vms#restore-options)belirtilmiştir. Lütfen bu işlemleri tetiklemeden önce gözden geçirin.

Geri yükleme işlemlerinin tetiklenmesi bir *Post* isteğidir. API hakkında daha fazla bilgi edinmek için, ["geri yüklemeyi Tetikle" REST API](/rest/api/backup/restores/trigger)bakın.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints/{recoveryPointId}/restore?api-version=2019-05-13
```

`{containerName}`Ve `{protectedItemName}` [burada](backup-azure-arm-userestapi-backupazurevms.md#example-responses-to-get-operation)oluşturulur. `{fabricName}` , `{recoveryPointId}` `{name}` [yukarıda](#example-response)belirtilen kurtarma noktasının alanı olan "Azure" dır.

Kurtarma noktası alındıktan sonra ilgili geri yükleme senaryosu için istek gövdesini oluşturacağız. Aşağıdaki bölümlerde her senaryonun istek gövdesi ana hatlarıyla verilmiştir.

- [Diskleri geri yükleme](#restore-disks)
- [Diskleri Değiştir](#replace-disks-in-a-backed-up-virtual-machine)
- [Yeni bir sanal makine olarak geri yükle](#restore-as-another-virtual-machine)

### <a name="restore-response"></a>Geri yükleme yanıtı

Herhangi bir geri yükleme işleminin tetiklenmesi [zaman uyumsuz bir işlemdir](../azure-resource-manager/management/async-operations.md). Bu işlemin Ayrıca izlenmesi gereken başka bir işlem oluşturduğu anlamına gelir.

Başka bir işlem oluşturulduğunda 202 (kabul edildi) ve bu işlem tamamlandığında 200 (Tamam) iki yanıt döndürür.

|Ad  |Tür  |Description  |
|---------|---------|---------|
|202 kabul edildi     |         |     Kabul edildi    |

#### <a name="example-responses"></a>Örnek yanıtlar

Geri yükleme disklerini tetiklemek için *Post* URI 'sini gönderdikten sonra, ilk yanıt bir konum üst bilgisi veya Azure-Async-header ile 202 (kabul edilir) olur.

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

Ardından, bir basit *Get* komutuyla konum üstbilgisini veya Azure-AsyncOperation üst bilgisini kullanarak elde edilen işlemi izleyin.

```http
GET https://management.azure.com/subscriptions//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationResults/781a0f18-e250-4d73-b059-5e9ffed4069e?api-version=2019-05-13
```

İşlem tamamlandıktan sonra, yanıt gövdesinde elde edilen geri yükleme işinin KIMLIĞI ile 200 (Tamam) döndürür.

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

Geri yükleme işi uzun süredir çalışan bir işlem olduğundan, [REST API belge kullanan izleme işlerinde](backup-azure-arm-userestapi-managejobs.md#tracking-the-job)açıklandığı şekilde izlenmelidir.

### <a name="restore-disks"></a>Diskleri geri yükleme

Bir VM 'nin yedekleme verilerinden oluşturulmasını özelleştirmeniz gerekiyorsa, diskleri seçilen bir depolama hesabına geri yükleyebilir ve bu disklerden bir VM 'yi gereksinimlere göre oluşturabilirsiniz. Depolama hesabının, kurtarma hizmetleri kasasıyla aynı bölgede olması ve bölge yedekli olmaması gerekir. Diskler, Ayrıca, yedeklenen VM 'nin ("vmconfig.jsüzerinde") yapılandırılması, belirtilen depolama hesabında depolanır. [Yukarıda](#restore-operations)açıklandığı gibi, geri yükleme diskleri için ilgili istek gövdesi aşağıda verilmiştir.

#### <a name="create-request-body"></a>İstek gövdesi oluştur

Azure VM yedeğinden bir disk geri yükleme tetiklenmesi için, istek gövdesinin bileşenleri aşağıda verilmiştir.

|Ad  |Tür  |Description  |
|---------|---------|---------|
|properties     | [IaaSVMRestoreRequest](/rest/api/backup/restores/trigger#iaasvmrestorerequest)        |    RestoreRequestResourceProperties     |

İstek gövdesinin ve diğer ayrıntıların tanımlarının tamamı listesi için [tetikleyici geri yükleme REST API belgesine](/rest/api/backup/restores/trigger#request-body)bakın.

##### <a name="example-request"></a>Örnek istek

Aşağıdaki istek gövdesi, disk geri yükleme tetiklenmesi için gereken özellikleri tanımlar.

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

### <a name="restore-disks-selectively"></a>Diskleri seçmeli olarak geri yükle

[Diskleri seçmeli olarak yedekliyorsanız](backup-azure-arm-userestapi-backupazurevms.md#excluding-disks-in-azure-vm-backup), geçerli yedeklenen disk listesi, [Kurtarma noktası özetinde](#select-recovery-point) ve [ayrıntılı yanıtta](https://docs.microsoft.com/rest/api/backup/recoverypoints/get)sağlanır. Ayrıca, diskleri seçmeli olarak geri yükleyebilir ve [burada](selective-disk-backup-restore.md#selective-disk-restore)daha fazla ayrıntı sağlanır. Yedeklenen disklerin listesi arasında bir diski seçmeli olarak geri yüklemek için, kurtarma noktası yanıtından diskin LUN 'unu bulun ve aşağıdaki şekilde [Yukarıdaki istek gövdesine](#example-request) **Restoredisklunlist** özelliğini ekleyin.

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
        },
        "restoreDiskLunList" : [0]
    }
}

```

[Yukarıda](#responses)açıklandığı gibi yanıtı izlerken ve uzun süre çalışan iş tamamlandıktan sonra, diskler ve yedeklenen sanal makinenin ("VMConfig.jsüzerinde") yapılandırması belirtilen depolama hesabında mevcut olacaktır.

### <a name="replace-disks-in-a-backed-up-virtual-machine"></a>Yedeklenen bir sanal makinede bulunan diskleri değiştirme

Diskleri geri yükleme, kurtarma noktasından diskler oluşturduğunda, diskleri Değiştir, yedeklenen VM 'nin geçerli disklerinin kurtarma noktasındaki disklerle yerini alır. [Yukarıda](#restore-operations)açıklandığı gibi, diskleri değiştirmeye yönelik ilgili istek gövdesi aşağıda verilmiştir.

#### <a name="create-request-body"></a>İstek gövdesi oluştur

Azure VM yedeğinden bir disk değişikliği tetiklenmesi için, istek gövdesinin bileşenleri aşağıda verilmiştir.

|Ad  |Tür  |Description  |
|---------|---------|---------|
|properties     | [IaaSVMRestoreRequest](/rest/api/backup/restores/trigger#iaasvmrestorerequest)        |    RestoreRequestResourceProperties     |

İstek gövdesinin ve diğer ayrıntıların tanımlarının tamamı listesi için [tetikleyici geri yükleme REST API belgesine](/rest/api/backup/restores/trigger#request-body)bakın.

#### <a name="example-request"></a>Örnek istek

Aşağıdaki istek gövdesi, disk geri yükleme tetiklenmesi için gereken özellikleri tanımlar.

```json
{
    "properties": {
        "objectType": "IaasVMRestoreRequest",
        "recoveryPointId": "20982486783671",
        "recoveryType": "OriginalLocation",
        "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
        "storageAccountId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Storage/storageAccounts/testAccount",  
        "region": "westus",
        "createNewCloudService": false,
        "originalStorageAccountOption": false,
        "affinityGroup": "",
        "diskEncryptionSetId": null,
        "subnetId": null,
        "targetDomainNameId": null,
        "targetResourceGroupId": null,
        "targetVirtualMachineId": null,
        "virtualNetworkId": null
     }
}

```

### <a name="restore-as-another-virtual-machine"></a>Başka bir sanal makine olarak geri yükle

[Yukarıda](#restore-operations)açıklandığı gibi, aşağıdaki istek gövdesi bir sanal makine geri yüklemesi tetiklemek için gereken özellikleri tanımlar.

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

Yanıt, [diskleri geri yüklemek için yukarıda açıklanacak](#responses)şekilde işlenmelidir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Backup REST API 'Leri hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

- [Azure kurtarma hizmetleri sağlayıcısı REST API](/rest/api/recoveryservices/)
- [Azure REST API’yi kullanmaya başlayın](/rest/api/azure/)
