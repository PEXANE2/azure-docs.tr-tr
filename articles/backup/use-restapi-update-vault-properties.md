---
title: REST API ile Kurtarma Hizmetleri kasa yapılandırması güncelleştirme
description: Bu makalede, REST API'yi kullanarak kasa yapılandırmasını nasıl güncelleştireceğimiz öğrenin.
ms.topic: conceptual
ms.date: 12/06/2019
ms.assetid: 9aafa5a0-1e57-4644-bf79-97124db27aa2
ms.openlocfilehash: 6cecbb18e0cd6f548e1688ef978f10dcee7d9fbc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252369"
---
# <a name="update-azure-recovery-services-vault-configurations-using-rest-api"></a>REST API'yi kullanarak Azure Kurtarma Hizmetleri Kasası yapılandırmalarını güncelleştirme

Bu makalede, REST API'yi kullanarak Azure Kurtarma Hizmetleri kasasında yedekleme yle ilgili yapılandırmaların nasıl güncelleştirilenanlatılmaktadır.

## <a name="soft-delete-state"></a>Yumuşak silme durumu

Korumalı bir öğenin yedeklerinin silmesi izlenmesi gereken önemli bir işlemdir. Azure Kurtarma Hizmetleri kasası, yanlışlıkla silinmelere karşı korunmak için yumuşak silme özelliğine sahiptir. Bu özellik, müşterilerin silinmiş yedeklemeleri gerekirse silinmeden sonraki bir süre içinde geri yüklemesine olanak tanır.

Ancak bu yeteneğin gerekli olmadığı senaryolar vardır. Azure Kurtarma Hizmetleri kasası, içinde yumuşak silinmiş öğeler bile olsa yedek öğeler varsa silinemez. Kasanın hemen silinmesi gerekiyorsa, bu sorun yaratabilir. Örneğin: dağıtım işlemleri genellikle aynı iş akışında oluşturulan kaynakları temizler. Dağıtım bir kasa oluşturabilir, bir öğeiçin yedeklemeleri yapılandırabilir, test geri yüklemesi yapabilir ve sonra yedekleme öğelerini ve kasayı silmeye devam edebilir. Kasa silme işlemi başarısız olursa, tüm dağıtım başarısız olabilir. Yumuşak silme işlemini devre dışı bırakmak, anında silinmesini garanti etmenin tek yoludur.

Bu nedenle, müşterinin senaryoya bağlı olarak belirli bir kasa için yumuşak silmeyi devre dışı edip etmeyeceğini dikkatle seçmesi gerekir. Daha fazla bilgi için [yumuşak silme makalesine](backup-azure-security-feature-cloud.md#soft-delete)bakın.

### <a name="fetch-soft-delete-state-using-rest-api"></a>REST API'yi kullanarak yumuşak silme durumunu getir

Varsayılan olarak, yeni oluşturulan Kurtarma Hizmetleri kasası için yumuşak silme durumu etkinleştirilir. Bir kasa için yumuşak silme durumunu almak/güncellemek için, yedek kasanın config ile ilgili [REST API belgesini](https://docs.microsoft.com/rest/api/backup/backupresourcevaultconfigs) kullanın

Bir kasa için geçerli yumuşak silme durumunu almak için aşağıdaki *GET* işlemini kullanın

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupconfig/vaultconfig?api-version=2019-05-13
```

GET URI'nin `{vaultName}` `{vaultresourceGroupName}` parametreleri vardır. `{subscriptionId}` Bu örnekte, `{vaultName}` "testVault" `{vaultresourceGroupName}` ve "testVaultRG". URI'de gerekli tüm parametreler verildiğinden, ayrı bir istek organına gerek yoktur.

```http
GET https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupconfig/vaultconfig?api-version=2019-05-13
```

#### <a name="responses"></a>Yanıtlar

'GET' işlemi için başarılı yanıt aşağıda gösterilmiştir:

|Adı  |Tür  |Açıklama  |
|---------|---------|---------|
|200 TAMAM     |   [BackupResourceVaultConfig](https://docs.microsoft.com/rest/api/backup/backupresourcevaultconfigs/get#backupresourcevaultconfigresource)      | Tamam        |

##### <a name="example-response"></a>Örnek yanıt

'GET' isteği gönderildikten sonra, 200 (başarılı) yanıt döndürülür.

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testvaultRG/providers/Microsoft.RecoveryServices/vaults/testvault/backupconfig/vaultconfig",
  "name": "vaultconfig",
  "type": "Microsoft.RecoveryServices/vaults/backupconfig",
  "properties": {
    "enhancedSecurityState": "Enabled",
    "softDeleteFeatureState": "Enabled"
  }
}
```

### <a name="update-soft-delete-state-using-rest-api"></a>REST API'yi kullanarak yumuşak silme durumunu güncelleştirme

REST API kullanarak kurtarma hizmetleri kasasının yumuşak silme durumunu güncelleştirmek için aşağıdaki *PATCH işlemini* kullanın

```http
PATCH https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupconfig/vaultconfig?api-version=2019-05-13
```

PATCH URI'nin `{vaultName}` `{vaultresourceGroupName}` parametreleri vardır. `{subscriptionId}` Bu örnekte, `{vaultName}` "testVault" `{vaultresourceGroupName}` ve "testVaultRG". Eğer URI'yi yukarıdaki değerlerle değiştirirsek, URI şu şekilde görünecektir.

```http
PATCH https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupconfig/vaultconfig?api-version=2019-05-13
```

#### <a name="create-the-request-body"></a>İstek gövdesini oluşturma

Ortak tanımlardan sonra bir istek gövdesi oluşturmak için kullanılır

Daha fazla bilgi için [REST API belgelerine](https://docs.microsoft.com/rest/api/backup/backupresourcevaultconfigs/update#request-body) bakın

|Adı  |Gerekli  |Tür  |Açıklama  |
|---------|---------|---------|---------|
|Etag     |         |   Dize      |  İsteğe bağlı eTag       |
|location     |  true       |Dize         |   Kaynak konumu      |
|properties     |         | [Kasa Özellikleri](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#vaultproperties)        |  Kasanın özellikleri       |
|etiketler     |         | Nesne        |     Kaynak etiketleri    |

#### <a name="example-request-body"></a>Örnek istek gövdesi

Aşağıdaki örnek, yumuşak silme durumunu 'devre dışı' olarak güncelleştirmek için kullanılır.

```json
{
  "properties": {
    "enhancedSecurityState": "Enabled",
    "softDeleteFeatureState": "Disabled"
  }
}
```

#### <a name="responses"></a>Yanıtlar

'PATCH' işlemi için başarılı yanıt aşağıda gösterilmiştir:

|Adı  |Tür  |Açıklama  |
|---------|---------|---------|
|200 TAMAM     |   [BackupResourceVaultConfig](https://docs.microsoft.com/rest/api/backup/backupresourcevaultconfigs/get#backupresourcevaultconfigresource)      | Tamam        |

##### <a name="example-response"></a>Örnek yanıt

'PATCH' isteği gönderildikten sonra, 200 (başarılı) yanıt döndürülür.

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testvaultRG/providers/Microsoft.RecoveryServices/vaults/testvault/backupconfig/vaultconfig",
  "name": "vaultconfig",
  "type": "Microsoft.RecoveryServices/vaults/backupconfig",
  "properties": {
    "enhancedSecurityState": "Enabled",
    "softDeleteFeatureState": "Disabled"
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

[Bu kasada bir Azure VM'yi yedeklemek için bir yedekleme ilkesi oluşturun.](backup-azure-arm-userestapi-createorupdatepolicy.md)

Azure REST API'leri hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

- [Azure Kurtarma Hizmetleri sağlayıcısı REST API](/rest/api/recoveryservices/)
- [Azure REST API’yi kullanmaya başlayın](/rest/api/azure/)
