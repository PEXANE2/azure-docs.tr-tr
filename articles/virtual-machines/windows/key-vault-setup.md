---
title: Azure Kaynak Yöneticisi'nde Windows VM'ler için Anahtar Kasası ayarlama
description: Key Vault'u Azure Kaynak Yöneticisi sanal makinesiyle kullanmak üzere nasıl ayarlayın?
services: virtual-machines-windows
documentationcenter: ''
author: mimckitt
manager: vashan
editor: ''
tags: azure-resource-manager
ms.assetid: 33a483e2-cfbc-4c62-a588-5d9fd52491e2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 01/24/2017
ms.author: mimckitt
ms.openlocfilehash: 3dcf82a8d4bc698c1900903649a2dd5a383f64b4
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878417"
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Azure Kaynak Yöneticisi'nde sanal makineler için Anahtar Kasası'nı ayarlama

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

Azure Kaynak Yöneticisi yığınında, sırlar/sertifikalar Key Vault'un kaynak sağlayıcısı tarafından sağlanan kaynaklar olarak modellenir. Key Vault hakkında daha fazla bilgi edinmek için [Azure Anahtar Kasası nedir?](../../key-vault/key-vault-overview.md)

> [!NOTE]
> 1. Key Vault'un Azure Kaynak Yöneticisi sanal makineleriyle kullanılabilmesi için Key Vault'taki **EnabledForDeployment** özelliğinin gerçek olarak ayarlanması gerekir. Bunu çeşitli istemcilerde yapabilirsiniz.
> 2. Anahtar Kasası'nın Sanal Makine ile aynı abonelik te ve konumda oluşturulması gerekir.
>
>

## <a name="use-powershell-to-set-up-key-vault"></a>Key Vault'u kurmak için PowerShell'i kullanın
PowerShell'i kullanarak anahtar kasası oluşturmak [için, PowerShell'i kullanarak Azure Key Vault'tan bir sır elde etme tarihine bakın.](../../key-vault/quick-create-powershell.md)

Yeni anahtar kasaları için bu PowerShell cmdlet'i kullanabilirsiniz:

    New-AzKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Mevcut anahtar kasaları için bu PowerShell cmdlet'i kullanabilirsiniz:

    Set-AzKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="use-cli-to-set-up-key-vault"></a>Key Vault'u ayarlamak için CLI'yi kullanın
Komut satırı arabirimini (CLI) kullanarak bir anahtar kasası oluşturmak için [CLI kullanarak Anahtar Kasasını Yönet'e](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault)bakın.

CLI için, dağıtım ilkesini atamadan önce anahtar kasasını oluşturmanız gerekir. Aşağıdaki komutu kullanarak bunu yapabilirsiniz:

    az keyvault create --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --location "EastAsia"
    
Ardından, şablon dağıtımında kullanılmak üzere Key Vault'u etkinleştirmek için aşağıdaki komutu çalıştırın:

    az keyvault update --name "ContosoKeyVault" --resource-group "ContosoResourceGroup" --enabled-for-deployment "true"

## <a name="use-templates-to-set-up-key-vault"></a>Key Vault'u ayarlamak için şablonları kullanma
Şablon kullanırken, `enabledForDeployment` özelliği Key Vault `true` kaynağıiçin ayarlamanız gerekir.

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

Şablonları kullanarak bir anahtar kasası oluşturduğunuzda yapılandırabileceğiniz diğer seçenekler [için](https://azure.microsoft.com/documentation/templates/101-key-vault-create/)bkz.
