---
title: Hızlı başlangıç-Azure PowerShell Azure Key Vault oluşturma
description: Azure PowerShell kullanarak Azure Key Vault oluşturmayı gösteren hızlı başlangıç
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: quickstart
ms.date: 01/27/2021
ms.author: mbaldwin
ms.openlocfilehash: e77493bc73bc2d6f590d9bdcf891171fbd71f74e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99070224"
---
# <a name="quickstart-create-a-key-vault-using-powershell"></a>Hızlı başlangıç: PowerShell kullanarak bir Anahtar Kasası oluşturma

Azure Key Vault [anahtarlar](../keys/index.yml), [gizli](../secrets/index.yml)diziler ve [Sertifikalar](../certificates/index.yml)için güvenli bir mağaza sağlayan bir bulut hizmetidir. Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault hakkında](overview.md); anahtar kasasında nelerin depolanabileceği hakkında daha fazla bilgi için bkz. [anahtarlar, gizlilikler ve sertifikalar hakkında](about-keys-secrets-certificates.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Bu hızlı başlangıçta [Azure PowerShell](/powershell/azure/)bir Anahtar Kasası oluşturacaksınız. PowerShell 'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü sürümü 1.0.0 veya üzerini gerektirir. `$PSVersionTable.PSVersion`Sürümü bulmak için yazın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzAccount` komutunu da çalıştırmanız gerekir.

```azurepowershell-interactive
Login-AzAccount
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create a resource group](../../../includes/key-vault-powershell-rg-creation.md)]

## <a name="create-a-key-vault"></a>Anahtar kasası oluşturma

[!INCLUDE [Create a key vault](../../../includes/key-vault-powershell-kv-creation.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Create a key vault](../../../includes/key-vault-powershell-delete-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure PowerShell kullanarak bir Key Vault oluşturdunuz. Key Vault ve uygulamalarınızla tümleştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere ilerleyin.

- [Azure Key Vault genel bakışını](overview.md) okuyun
- [Azure PowerShell Key Vault cmdlet 'lerine](/powershell/module/az.keyvault/) yönelik başvuruya bakın
- [Azure Key Vault güvenliğine genel bakış](security-overview.md) konusunu gözden geçirin

