---
title: Azure CLI ile Windows VM oluşturma ve şifreleme
description: Bu hızlı başlangıçta, bir Windows sanal makinesi oluşturmak ve şifrelemek için Azure CLI'yi nasıl kullanacağınızı öğrenirsiniz
author: msmbaldwin
ms.author: mbaldwin
ms.service: virtual-machines-windows
ms.subservice: security
ms.topic: quickstart
ms.date: 05/17/2019
ms.openlocfilehash: aa0dd0bf55d51800d6a9b4283aa9e653887bf2f1
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82082807"
---
# <a name="quickstart-create-and-encrypt-a-windows-vm-with-the-azure-cli"></a>Hızlı başlatma: Azure CLI ile bir Windows VM oluşturma ve şifreleme

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıç, Bir Windows Server 2016 sanal makinesi (VM) oluşturmak ve şifrelemek için Azure CLI'yi nasıl kullanacağınızı gösterir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Azure CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu hızlı başlatma, Azure CLI sürümünü 2.0.30 veya daha yeni bir sürüm olarak çalıştırmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group?view=azure-cli-latest#az-group-create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek, *eastus* konumda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

[az vm create](/cli/azure/vm?view=azure-cli-latest#az-vm-create) ile bir VM oluşturun. Aşağıdaki *örnekmyVM*adlı bir VM oluşturur. Bu örnekte yönetici kullanıcı için *azureuser* kullanıcı adı ve *myPassword12* parolası kullanılır.

```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image win2016datacenter \
    --admin-username azureuser \
    --admin-password myPassword12
```

VM’yi ve destekleyici kaynakları oluşturmak birkaç dakika sürer. Aşağıdaki örnekte VM oluşturma işleminin başarılı olduğu gösterilmektedir.

```
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="create-a-key-vault-configured-for-encryption-keys"></a>Şifreleme anahtarları için yapılandırılan bir Key Vault oluşturma

Azure disk şifrelemesi şifreleme anahtarını Azure Anahtar Kasası'nda saklar. [Az keyvault oluşturmak](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create)ile bir Anahtar Vault oluşturun. Anahtar Kasası'nın şifreleme anahtarlarını depolamasını sağlamak için disk için etkin leştirilmiş parametreyi kullanın.
> [!Important]
> Her Key Vault'un benzersiz bir adı olmalıdır. Aşağıdaki örnek, *myKV*adlı bir Anahtar Vault oluşturur, ancak farklı bir şey adlandırmanız gerekir.

```azurecli-interactive
az keyvault create --name "myKV" --resource-group "myResourceGroup" --location eastus --enabled-for-disk-encryption
```

## <a name="encrypt-the-virtual-machine"></a>Sanal makineyi şifreleme

VM'nizi [az vm şifreleme](/cli/azure/vm/encryption?view=azure-cli-latest)ile şifreleyin ve disk-şifreleme-keyvault parametresine benzersiz Key Vault adınızı sağlar.

```azurecli-interactive
az vm encryption enable -g MyResourceGroup --name MyVM --disk-encryption-keyvault myKV
```

[Az vm show](/cli/azure/vm/encryption#az-vm-encryption-show) ile VM'nizde şifrelemenin etkin olduğunu doğrulayabilirsiniz

```azurecli-interactive
az vm show --name MyVM -g MyResourceGroup
```

Döndürülen çıktıda aşağıdakileri görürsünüz:

```
"EncryptionOperation": "EnableEncryption"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubu, VM ve Key Vault'u kaldırmak için [az grubu silme](/cli/azure/group) komutunu kullanabilirsiniz.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, sanal bir makine oluşturdunuz, şifreleme anahtarları için etkinleştirilen bir Key Vault oluşturdunuz ve VM'yi şifrelediniz.  IaaS VM'leri için Azure Disk Şifrelemesi önkoşulları hakkında daha fazla bilgi edinmek üzere bir sonraki makaleye geçin.

> [!div class="nextstepaction"]
> [Azure Disk Şifreleme'ye genel bakış](disk-encryption-overview.md)
