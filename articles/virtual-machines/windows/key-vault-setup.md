---
title: Anahtar Kasası ayarlama
description: Bir sanal makine ile kullanmak için Key Vault ayarlama.
author: mimckitt
manager: vashan
ms.service: virtual-machines
ms.subservice: security
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 01/24/2017
ms.author: mimckitt
ms.openlocfilehash: e4bff4d1826d9586495207095eccf8f6c66164a0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81870019"
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Azure Resource Manager sanal makineler için Key Vault ayarlama

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

Azure Resource Manager yığınında, gizlilikler/sertifikalar, Key Vault kaynak sağlayıcısı tarafından sağlanmış kaynaklar olarak modellenir. Key Vault hakkında daha fazla bilgi edinmek için bkz. [Azure Key Vault nedir?](../../key-vault/general/overview.md)

> [!NOTE]
> 1. Key Vault, Azure Resource Manager sanal makinelerle birlikte kullanılabilmesi için, Key Vault üzerindeki **Enabledfordeployment** özelliğinin true olarak ayarlanması gerekir. Bunu çeşitli istemcilerde yapabilirsiniz.
> 2. Key Vault sanal makineyle aynı abonelikte ve konumda oluşturulması gerekir.
>
>

## <a name="use-powershell-to-set-up-key-vault"></a>PowerShell kullanarak Key Vault ayarlama
PowerShell kullanarak bir Anahtar Kasası oluşturmak için bkz. [PowerShell kullanarak Azure Key Vault bir gizli dizi ayarlama ve alma](../../key-vault/secrets/quick-create-powershell.md).

Yeni Anahtar kasaları için bu PowerShell cmdlet 'ini kullanabilirsiniz:

    New-AzKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Mevcut Anahtar kasaları için bu PowerShell cmdlet 'ini kullanabilirsiniz:

    Set-AzKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="use-cli-to-set-up-key-vault"></a>CLı kullanarak Key Vault ayarlama
Komut satırı arabirimini (CLı) kullanarak bir Anahtar Kasası oluşturmak için bkz. [CLI kullanarak Key Vault yönetme](../../key-vault/general/manage-with-cli2.md#create-a-key-vault).

CLı için, dağıtım ilkesini atamadan önce anahtar kasasını oluşturmanız gerekir. Aşağıdaki komutu kullanarak bunu yapabilirsiniz:

    az keyvault create --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --location "EastAsia"
    
Daha sonra Key Vault şablon dağıtımıyla kullanmak üzere etkinleştirmek için aşağıdaki komutu çalıştırın:

    az keyvault update --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --enabled-for-deployment "true"

## <a name="use-templates-to-set-up-key-vault"></a>Şablonları kullanarak Key Vault ayarlama
Şablon kullanırken, `enabledForDeployment` Key Vault kaynağı için özelliğini olarak ayarlamanız gerekir `true` .

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Şablonları kullanarak bir Anahtar Kasası oluştururken yapılandırabileceğiniz diğer seçenekler için bkz. [Anahtar Kasası oluşturma](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
