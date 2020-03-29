---
title: Azure CLI ile Azure ölçek kümeleri için diskleri şifreleme
description: Windows sanal makine ölçeği kümesindeki VM örneklerini ve ekli diskleri şifrelemek için Azure PowerShell'i nasıl kullanacağınızı öğrenin
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: mbaldwin
ms.openlocfilehash: 557d5c023acbc7987d58c9e78bfe11e25f314879
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76279072"
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set-with-the-azure-cli"></a>Azure CLI ile ayarlanmış sanal makine ölçeğinde işletim sistemi ve bağlı veri disklerini şifreleme

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıç, sanal makine ölçeği kümesi oluşturmak ve şifrelemek için Azure CLI'yi nasıl kullanacağınızı gösterir. Azure Disk şifrelemesini sanal makine ölçeği kümesine uygulama hakkında daha fazla bilgi [için, Sanal Makine Ölçeği Kümeleri için Azure Disk](disk-encryption-overview.md)Şifrelemesi'ne bakın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu öğretici, Azure CLI sürümünü 2.0.31 veya daha sonra çalıştırdığınızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-scale-set"></a>Ölçek kümesi oluşturma

Ölçek kümesi oluşturabilmek için [az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Aşağıdaki örnek, *eastus* konumda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Bu adımda [az vmss create](/cli/azure/vmss) ile bir sanal makine ölçek kümesi oluşturun. Aşağıdaki örnek, değişiklikler uygulandıkça otomatik güncelleştirilecek şekilde ayarlanan *myScaleSet* adlı bir ölçek kümesi oluşturur ve *~/.ssh/id_rsa* içinde yoksa SSH anahtarları oluşturur. Her VM örneğine 32Gb veri diski eklenir ve az [vmss uzantı lı](/cli/azure/vmss/extension)veri disklerini hazırlamak için Azure [Özel Komut Dosyası Uzantısı](../virtual-machines/linux/extensions-customscript.md) kullanılır:

```azurecli-interactive
# Create a scale set with attached data disk
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --generate-ssh-keys \
  --data-disk-sizes-gb 32

# Prepare the data disk for use with the Custom Script Extension
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroup \
  --vmss-name myScaleSet \
  --settings '{"fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.sh"],"commandToExecute":"./prepare_vm_disks.sh"}'
```

Tüm ölçek kümesi kaynaklarının ve VM'lerin oluşturulup yapılandırılması birkaç dakika sürer.

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Disk şifrelemesi için etkin leştirilmiş bir Azure anahtar kasası oluşturma

Azure Key Vault, uygulamalarınızda ve hizmetlerinizde bunları güvenli bir şekilde uygulamanızı sağlayan anahtarları, sırları veya parolaları depolayabilir. Şifreleme anahtarları yazılım koruması kullanılarak Azure Key Vault'ta depolanır veya fips 140-2 seviye 2 standartlarına göre onaylı Donanım Güvenlik Modülleri'nde (HSM) anahtarlarınızı içe aktarabilir veya oluşturabilirsiniz. Bu şifreleme anahtarları, VM'nize bağlı sanal diskleri şifrelemek ve şifresini çözmek için kullanılır. Bu şifreleme anahtarlarının denetimini siz elde elabilirsiniz ve bunların kullanımını denetleyebilirsiniz.

Kendi benzersiz *keyvault_name*tanımlayın. Ardından, [az keyvault'un](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-create) ölçek kümesiyle aynı abonelik ve bölgede oluşturduğu bir KeyVault oluşturun ve *--disk için etkin* erişim ilkesini ayarlayın.

```azurecli-interactive
# Provide your own unique Key Vault name
keyvault_name=myuniquekeyvaultname

# Create Key Vault
az keyvault create --resource-group myResourceGroup --name $keyvault_name --enabled-for-disk-encryption
```

### <a name="use-an-existing-key-vault"></a>Varolan bir Anahtar Kasası kullanma

Bu adım yalnızca disk şifrelemeyle kullanmak istediğiniz varolan bir Key Vault'unuzun olması gerekir. Önceki bölümde bir Anahtar Kasası oluşturduysanız bu adımı atlayın.

Kendi benzersiz *keyvault_name*tanımlayın. Ardından KeyVault'unuzu [az keyvault güncelleştirmesiyle](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-update) güncelleştirin ve *disk için etkin* erişim ilkesini ayarlayın.

```azurecli-interactive
# Provide your own unique Key Vault name
keyvault_name=myuniquekeyvaultname

# Create Key Vault
az keyvault update --name $keyvault_name --enabled-for-disk-encryption
```

## <a name="enable-encryption"></a>Şifrelemeyi etkinleştirme

Bir ölçek kümesinde VM örneklerini şifrelemek için, önce [az keyvault gösterile](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-show)Key Vault kaynak kimliği hakkında bazı bilgiler alın. Bu değişkenler daha sonra [az vmss şifreleme etkinleştirme](/cli/azure/vmss/encryption#az-vmss-encryption-enable)ile şifreleme işlemini başlatmak için kullanılır:

```azurecli-interactive
# Get the resource ID of the Key Vault
vaultResourceId=$(az keyvault show --resource-group myResourceGroup --name $keyvault_name --query id -o tsv)

# Enable encryption of the data disks in a scale set
az vmss encryption enable \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --disk-encryption-keyvault $vaultResourceId \
    --volume-type DATA
```

Şifreleme işleminin başlaması bir veya iki dakika sürebilir.

Ölçek kümesi, daha önceki bir adımda oluşturulan ölçek kümesinde yükseltme ilkesi *otomatik*olarak ayarlanırken, VM örnekleri şifreleme işlemini otomatik olarak başlatır. Yükseltme ilkesinin el ile yapılacağı ölçek kümelerinde, [az vmss güncelleştirme örnekleriyle](/cli/azure/vmss#az-vmss-update-instances)VM örneklerinde şifreleme ilkesini başlatın.

### <a name="enable-encryption-using-kek-to-wrap-the-key"></a>Anahtarı sarmak için KEK kullanarak şifrelemeyi etkinleştirme

Sanal makine ölçeği kümesini şifrelerken ek güvenlik için bir Anahtar Şifreleme Anahtarı da kullanabilirsiniz.

```azurecli-interactive
# Get the resource ID of the Key Vault
vaultResourceId=$(az keyvault show --resource-group myResourceGroup --name $keyvault_name --query id -o tsv)

# Enable encryption of the data disks in a scale set
az vmss encryption enable \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --disk-encryption-keyvault $vaultResourceId \
    --key-encryption-key myKEK \
    --key-encryption-keyvault $vaultResourceId \
    --volume-type DATA
```

> [!NOTE]
>  Disk-şifreleme-anahtar tonoz parametresi değeri için sözdizimi tam tanımlayıcı dizedir:</br>
/subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br></br>
> Anahtar şifreleme anahtarı parametresinin değeri için sözdizimi, KEK'e tam URI'dir:</br>
https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id]

## <a name="check-encryption-progress"></a>Şifreleme ilerlemesini denetleme

Disk şifreleme durumunu kontrol etmek için [az vmss şifreleme gösterisini](/cli/azure/vmss/encryption#az-vmss-encryption-show)kullanın:

```azurecli-interactive
az vmss encryption show --resource-group myResourceGroup --name myScaleSet
```

VM örnekleri şifrelendiğinde, durum kodu aşağıdaki örnek çıktıda gösterildiği gibi *EncryptionState/encrypted*raporlanır:

```bash
[
  {
    "disks": [
      {
        "encryptionSettings": null,
        "name": "myScaleSet_myScaleSet_0_disk2_3f39c2019b174218b98b3dfae3424e69",
        "statuses": [
          {
            "additionalProperties": {},
            "code": "EncryptionState/encrypted",
            "displayStatus": "Encryption is enabled on disk",
            "level": "Info",
            "message": null,
            "time": null
          }
        ]
      }
    ],
    "id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualMachines/0",
    "resourceGroup": "MYRESOURCEGROUP"
  }
]
```

## <a name="disable-encryption"></a>Şifrelemeyi devre dışı

Artık şifreli VM örnekleri diskleri kullanmak istemiyorsanız, [az vmss şifreleme](/cli/azure/vmss/encryption?view=azure-cli-latest#az-vmss-encryption-disable) ile şifrelemeyi aşağıdaki gibi devre dışı kullanabilirsiniz:

```azurecli-interactive
az vmss encryption disable --resource-group myResourceGroup --name myScaleSet
```

## <a name="next-steps"></a>Sonraki adımlar

- Bu makalede, sanal makine ölçeği kümesini şifrelemek için Azure CLI'yi kullandınız. [Azure PowerShell](disk-encryption-powershell.md) veya [Azure Kaynak Yöneticisi şablonlarını](disk-encryption-azure-resource-manager.md)da kullanabilirsiniz.
- Azure Disk Şifreleme'nin başka bir uzantı sağlandıktan sonra uygulanmasını istiyorsanız, [uzantı sıralamasını](virtual-machine-scale-sets-extension-sequencing.md)kullanabilirsiniz. 
- Linux ölçeği kümesi veri diski şifrelemesi için uçuça bir toplu iş dosyası örneği [burada](https://gist.githubusercontent.com/ejarvi/7766dad1475d5f7078544ffbb449f29b/raw/03e5d990b798f62cf188706221ba6c0c7c2efb3f/enable-linux-vmss.bat)bulunabilir. Bu örnek bir kaynak grubu oluşturur, Linux ölçek kümesi, 5 GB'lık bir veri diski bağlar ve sanal makine ölçeği kümesini şifreler.
