---
title: "Quickstart: PowerShell kullanarak Key Vault'tan bir sır almak & ayarlayın"
description: Bu hızlı başlangıçta, Azure PowerShell'i kullanarak Azure Key Vault'tan nasıl sır oluşturup, alınve silinmeyi öğrenin.
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
ms.openlocfilehash: 3bac3cc2a5cedbd4b963a0759e6c8b940d2ca924
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424989"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-powershell"></a>Hızlı başlangıç: PowerShell kullanarak Azure Key Vault'tan gizli dizi ayarlama ve alma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Key Vault, güvenli bir gizli dizi deposu olarak çalışan bir bulut hizmetidir. Anahtarları, parolaları, sertifikaları ve diğer gizli dizileri güvenli bir şekilde depolayabilirsiniz. Key Vault hakkında daha fazla bilgi için [Genel Bakış](../general/overview.md) bölümünü inceleyebilirsiniz. Bu hızlı başlangıçta PowerShell kullanarak bir anahtar kasası oluşturacaksınız. Ardından yeni oluşturulan kasada bir gizli dizi depolayacaksınız.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu öğretici için Azure PowerShell modülü sürüm 1.0.0 veya sonrası gerekir. Sürümü `$PSVersionTable.PSVersion` bulmak için yazın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzAccount` komutunu da çalıştırmanız gerekir.

```azurepowershell-interactive
Login-AzAccount
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[Yeni-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)ile bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

```azurepowershell-interactive
New-AzResourceGroup -Name ContosoResourceGroup -Location EastUS
```

## <a name="create-a-key-vault"></a>Anahtar kasası oluşturma

Ardından bir Key Vault oluşturacaksınız. Bu adımı uygularken bazı bilgiler gereklidir:

Bu hızlı başlangıç boyunca Key Vault'umuz için "Contoso KeyVault2"yi kullansak da, benzersiz bir ad kullanmalısınız.

- **Kasa adı** Contoso-Vault2.
- **Kaynak grubu adı** ContosoResourceGroup.
- **Konum** Doğu ABD.

```azurepowershell-interactive
New-AzKeyVault -Name 'Contoso-Vault2' -ResourceGroupName 'ContosoResourceGroup' -Location 'East US'
```

Bu cmdlet’in çıktısı, yeni oluşturulan anahtar kasasının özelliklerini gösterir. Aşağıda listelenen iki özelliği not edin:

* **Kasa Adı**: Örnekte bu değer **Contoso-Vault2** şeklindedir. Bu adı diğer Anahtar Kasası cmdlet'leri için kullanacaksınız.
* **Kasa URI’si**: Örnekte bu değer https://Contoso-Vault2.vault.azure.net/ şeklindedir. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

Kasa oluşturma sonrasında Azure hesabınız bu yeni kasa üzerinde herhangi bir işlem yapmasına izin verilen tek hesaptır.

![Anahtar Kasası oluşturma komutu tamamlandıktan sonraki çıktı](../media/quick-create-powershell/output-after-creating-keyvault.png)

## <a name="adding-a-secret-to-key-vault"></a>Key Vault’a gizli dizi ekleme

Kasaya bir gizli dizi eklemek için birkaç adım uygulamanız gerekir. Bu örnekte, bir uygulama tarafından kullanılabilecek bir parola ekleyeceksiniz. Parola **examplePassword** olarak adlandırılır ve **içinde hVFkkk965BuUv** değerini depolar.

Önce **hVFkk965BuUv** değerini yazarak güvenli bir dize dönüştürün:

```azurepowershell-interactive
$secretvalue = ConvertTo-SecureString 'hVFkk965BuUv' -AsPlainText -Force
```

Daha sonra, anahtar vault değeri **hVFkk965BuUv** ile **ExamplePassword** adlı bir sır oluşturmak için aşağıdaki PowerShell komutları yazın:

```azurepowershell-interactive
$secret = Set-AzKeyVaultSecret -VaultName 'Contoso-Vault2' -Name 'ExamplePassword' -SecretValue $secretvalue
```

Gizli dizi içindeki değeri düz metin olarak görüntülemek için:

```azurepowershell-interactive
(Get-AzKeyVaultSecret -vaultName "Contoso-Vault2" -name "ExamplePassword").SecretValueText
```

Artık bir Key Vault oluşturdunuz, bir gizli dizli depoladınız ve bunu aldınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

 Bu koleksiyondaki diğer hızlı başlangıçlar ve öğreticiler bu hızlı başlangıcı temel alır. Diğer hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu kaynakları yerinde bırakmak isteyebilirsiniz.

Artık gerekmediğinde, kaynak grubunu, Anahtar Kasasını ve ilgili tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kullanabilirsiniz.

```azurepowershell-interactive
Remove-AzResourceGroup -Name ContosoResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç size bir Key Vault oluşturdu ve içinde bir sır saklanmış. Key Vault ve uygulamalarınızla nasıl entegre edilebildiğini öğrenmek için aşağıdaki makalelere devam edin.

- Azure [Anahtar Kasasına Genel Bakış](../general/overview.md)
- [Azure PowerShell Anahtar Kasası cmdlets](/powershell/module/az.keyvault/?view=azps-2.6.0#key_vault) için başvuruya bakın
- Azure Key Vault en iyi uygulamalarını gözden [geçirin](../general/best-practices.md)
