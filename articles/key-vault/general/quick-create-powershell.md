---
title: Hızlı başlangıç-Azure PowerShell Azure Key Vault oluşturma
description: Azure PowerShell kullanarak Azure Key Vault oluşturmayı gösteren hızlı başlangıç
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.custom: mvc
ms.date: 11/08/2019
ms.author: mbaldwin
ms.openlocfilehash: e24f1e546de1ce01896e271dbc9155c47f6c2bb6
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87102351"
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

[New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)Ile bir Azure Kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

```azurepowershell-interactive
New-AzResourceGroup -Name 'myResourceGroup" -Location "EastUS"
```

## <a name="create-a-key-vault"></a>Anahtar kasası oluşturma

Önceki adımdan kaynak grubunda bir Key Vault oluşturun. Bazı bilgileri sağlamanız gerekir:

- Anahtar Kasası adı: yalnızca rakam (0-9), harf (A-z, A-Z) ve kısa çizgi (-) içerebilen 3 ile 24 karakter arasında bir dize

  > [!Important]
  > Her Anahtar Kasası benzersiz bir ada sahip olmalıdır. -Unique-keykasa-adı> <aşağıdaki örneklerde anahtar kasanızın adıyla değiştirin.

- Kaynak grubu adı: **Myresourcegroup**.
- Konum: **EastUS**.

```azurepowershell-interactive
New-AzKeyVault -Name "&lt;your-unique-key-vault-name&gt; -ResourceGroupName "myResourceGroup" -Location "East US"
```

Bu cmdlet’in çıktısı, yeni oluşturulan anahtar kasasının özelliklerini gösterir. Aşağıda listelenen iki özelliği not edin:

- **Kasa adı**: Yukarıdaki--name parametresine verdiğiniz addır.
- **Kasa URI 'si**: örnekte bu https://- &lt; Unique-keykasasına-Name &gt; . Vault.Azure.net/. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

Bu noktada Azure hesabınız, bu yeni anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkili olan tek hesaptır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer hızlı başlangıçlar ve öğreticiler bu hızlı başlangıcı temel alır. Diğer hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu kaynakları yerinde bırakmak isteyebilirsiniz.

Artık gerekli değilse, [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu Azure PowerShell kullanarak kaynak grubunu ve tüm ilgili kaynakları kaldırabilirsiniz.

```azurepowershell-interactive
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Key Vault oluşturup bir gizli dizi depoladınız. Key Vault ve uygulamalarınızla tümleştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere ilerleyin.

- [Azure Key Vault genel bakışını](overview.md) okuyun
- [Azure PowerShell Key Vault cmdlet 'lerine](/powershell/module/az.keyvault/?view=azps-2.6.0#key_vault) yönelik başvuruya bakın
- [En iyi uygulamaları](best-practices.md) gözden geçirin Azure Key Vault
