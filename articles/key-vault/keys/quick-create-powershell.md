---
title: "Quickstart: Azure Key Vault'tan bir anahtar ayarlayın ve alın"
description: Azure PowerShell'i kullanarak Azure Key Vault'tan bir anahtarın nasıl ayarlanıp alınsüreceğini gösteren hızlı başlangıç
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: quickstart
ms.date: 03/30/2020
ms.author: mbaldwin
ms.openlocfilehash: c407c10327a80de6b3df18a3db3978468c9f8da0
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424191"
---
# <a name="quickstart-set-and-retrieve-a-key-from-azure-key-vault-using-azure-powershell"></a>Hızlı başlatma: Azure PowerShell'i kullanarak Azure Key Vault'tan bir anahtar ayarlayın ve alın

Bu hızlı başlangıçta, Azure PowerShell ile Azure Key Vault'ta önemli bir kasa oluşturursunuz. Azure Key Vault, güvenli bir gizli dizi deposu olarak çalışan bir bulut hizmetidir. Anahtarları, parolaları, sertifikaları ve diğer gizli dizileri güvenli bir şekilde depolayabilirsiniz. Key Vault hakkında daha fazla bilgi için [Genel Bakış'ı](../general/overview.md)inceleyebilirsiniz. Azure PowerShell, komutları veya komutları kullanarak Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bunu tamamladıktan sonra, bir anahtar saklarsınız.

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

## <a name="add-a-key-to-key-vault"></a>Key Vault'a anahtar ekleme

Kasaya bir anahtar eklemek için birkaç ek adım atmanız yeterlidir. Bu anahtar bir uygulama tarafından kullanılabilir. 

**ExampleKey** adlı bir oluşturmak için aşağıdaki komutları yazın:

```azurepowershell-interactive
Add-AzKeyVaultKey -VaultName 'Contoso-Vault2' -Name 'ExampleKey' -Destination 'Software'
```

Artık Azure Key Vault'a eklediğiniz bu anahtara URI'sini kullanarak başvuruda bulunabilirsiniz. Geçerli **https://Contoso-Vault2.vault.azure.net/keys/ExampleKey** sürümü almak için kullanın. 

Daha önce depolanan anahtarı görüntülemek için:

```azurepowershell-interactive
Get-AzKeyVaultKey -VaultName 'Contoso-Vault2' -KeyName 'ExampleKey'
```

Şimdi, bir Anahtar Kasası oluşturdunuz, bir anahtar depoladınız ve geri aldınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer hızlı başlangıçlar ve öğreticiler bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu kaynakları yerinde bırakmanız yararlı olabilir.
Artık gerekmediğinde, kaynak grubunu ve ilgili tüm kaynakları kaldırmak için [Kaldır-AzKaynak Grubu](/powershell/module/az.resources/remove-azresourcegroup) komutunu kullanabilirsiniz. Kaynakları aşağıda gösterildiği gibi silebilirsiniz:

```azurepowershell-interactive
Remove-AzResourceGroup -Name ContosoResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Anahtar Kasası oluşturdunuz ve içinde bir sertifika depolağı nız oldu. Key Vault ve uygulamalarınızla nasıl entegre edilebildiğini öğrenmek için aşağıdaki makalelere devam edin.

- Azure [Anahtar Kasasına Genel Bakış](../general/overview.md)
- [Azure PowerShell Anahtar Kasası cmdlets](/powershell/module/az.keyvault/) için başvuruya bakın
- Azure Key Vault en iyi uygulamalarını gözden [geçirin](../general/best-practices.md)
