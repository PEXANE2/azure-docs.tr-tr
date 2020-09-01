---
title: Disk şifrelemesi etkinleştirilmiş havuz oluşturma
description: Platform tarafından yönetilen bir anahtarla düğümleri şifrelemek için disk şifreleme yapılandırması 'nı nasıl kullanacağınızı öğrenin.
author: pkshultz
ms.topic: how-to
ms.date: 08/25/2020
ms.author: peshultz
ms.custom: references_regions
ms.openlocfilehash: 9b0f7f9963ee0edd3986f7ec808a8a4060d857f8
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89267062"
---
# <a name="create-a-pool-with-disk-encryption-enabled"></a>Disk şifrelemesi etkinleştirilmiş havuz oluşturma

Sanal makine yapılandırmasını kullanarak bir Azure Batch havuzu oluşturduğunuzda, disk şifrelemesi yapılandırmasını belirterek, havuzdaki işlem düğümlerini platform tarafından yönetilen bir anahtarla şifreleyebilirsiniz.

Bu makalede, disk şifrelemesi etkin olan bir Batch havuzunun nasıl oluşturulacağı açıklanmaktadır.

## <a name="why-use-a-pool-with-disk-encryption-configuration"></a>Disk şifreleme yapılandırması ile neden havuz kullanılmalıdır?

Bir Batch havuzu ile, işlem düğümünün işletim sistemi ve geçici disklerinde verilere erişebilir ve bunları kaydedebilirsiniz. Sunucu tarafı disk, platform tarafından yönetilen bir anahtarla şifrelenmek, bu verileri düşük ek yük ve kolaylık sağlayacak şekilde korumanıza yöneliktir.  

Batch, havuz yapılandırmasına ve bölgesel desteklenebilirine göre işlem düğümlerinde bu disk şifreleme teknolojilerinden birini uygular.

- [Platform tarafından yönetilen anahtarlarla bekleyen yönetilen disk şifrelemesi](../virtual-machines/windows/disk-encryption.md#platform-managed-keys)
- [Platform tarafından yönetilen anahtar kullanarak konakta şifreleme](../virtual-machines/windows/disk-encryption.md#encryption-at-host---end-to-end-encryption-for-your-vm-data)
- [Azure Disk Şifrelemesi](../security/fundamentals/azure-disk-encryption-vms-vmss.md)

> [!IMPORTANT]
> Azure Batch ' de platform tarafından yönetilen anahtar kullanılarak konakta şifreleme desteği şu anda Doğu ABD, Batı ABD 2, Orta Güney ABD, US Gov Virginia ve US Gov Arizona bölgeleri için genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Havuzunuzdaki düğümlere hangi şifreleme yönteminin uygulanacağını belirleyemeyeceksiniz. Bunun yerine, düğümlerinde şifrelemek istediğiniz hedef diskleri sağlarsınız ve toplu Işlem, belirtilen disklerin işlem düğümünde şifrelendiğinden emin olmak için uygun şifreleme yöntemini seçebilirler.
 
## <a name="azure-portal"></a>Azure portal 

Azure portal bir Batch havuzu oluştururken, **disk şifreleme yapılandırması**altında **TemporaryDisk** veya **OsAndTemporaryDisk** seçeneklerinden birini belirleyin.

:::image type="content" source="media/disk-encryption/portal-view.png" alt-text="Azure portal disk şifrelemesi yapılandırma seçeneğinin ekran görüntüsü.":::

Havuz oluşturulduktan sonra, havuzun **Özellikler** bölümünde disk şifrelemesi yapılandırma hedeflerini görebilirsiniz.

:::image type="content" source="media/disk-encryption/configuration-target.png" alt-text="Azure portal disk şifrelemesi yapılandırma hedeflerini gösteren ekran görüntüsü.":::

## <a name="examples"></a>Örnekler

Aşağıdaki örneklerde, Batch .NET SDK, Batch REST API ve Azure CLı kullanılarak Batch havuzunda işletim sistemi ve geçici disklerin nasıl şifrelenmesi gösterilmektedir.

### <a name="batch-net-sdk"></a>Batch .NET SDK'sı

```csharp
pool.VirtualMachineConfiguration.DiskEncryptionConfiguration = new DiskEncryptionConfiguration(
    targets: new List<DiskEncryptionTarget> { DiskEncryptionTarget.OsDisk, DiskEncryptionTarget.TemporaryDisk }
    );
```

### <a name="batch-rest-api"></a>Batch REST API

REST API URL 'SI:
```
POST {batchURL}/pools?api-version=2020-03-01.11.0
client-request-id: 00000000-0000-0000-0000-000000000000
```
İstek gövdesi:
```
"pool": {
    "id": "pool2",
    "vmSize": "standard_a1",
    "virtualMachineConfiguration": {
        "imageReference": {
            "publisher": "Canonical",
            "offer": "UbuntuServer",
            "sku": "18.04-LTS"
        },
        "diskEncryptionConfiguration": {
            "targets": [
                "OsDisk",
                "TemporaryDisk"
            ]
        }
        "nodeAgentSKUId": "batch.node.ubuntu 18.04"
    },
    "resizeTimeout": "PT15M",
    "targetDedicatedNodes": 5,
    "targetLowPriorityNodes": 0,
    "maxTasksPerNode": 3,
    "enableAutoScale": false,
    "enableInterNodeCommunication": false
}
```

### <a name="azure-cli"></a>Azure CLI’si

```azurecli-interactive
az batch pool create \
    --id diskencryptionPool \
    --vm-size Standard_DS1_V2 \
    --target-dedicated-nodes 2 \
    --image canonical:ubuntuserver:18.04-LTS \
    --node-agent-sku-id "batch.node.ubuntu 18.04" \
    --disk-encryption-targets OsDisk TemporaryDisk
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure disk depolama sunucu tarafı şifrelemesi](../virtual-machines/windows/disk-encryption.md)hakkında daha fazla bilgi edinin.
- Toplu Iş hakkında ayrıntılı genel bakış için bkz. [Batch hizmeti iş akışı ve kaynaklar](batch-service-workflow-features.md).
