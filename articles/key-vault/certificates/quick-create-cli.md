---
title: 'Hızlı başlangıç: & görüntüleme Azure Key Vault sertifikaları ayarlama-Azure CLı'
description: Azure CLı kullanarak Azure Key Vault bir sertifikanın nasıl ayarlanacağını ve alınacağını gösteren hızlı başlangıç
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: quickstart
ms.custom: mvc, seo-javascript-september2019, seo-javascript-october2019
ms.date: 09/03/2019
ms.author: mbaldwin
ms.openlocfilehash: e845ce859994aaac7d225ce3411c280582300408
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86536496"
---
# <a name="quickstart-set-and-retrieve-a-certificate-from-azure-key-vault-using-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak Azure Key Vault bir sertifikayı ayarlama ve alma

Bu hızlı başlangıçta Azure CLı ile Azure Key Vault bir Anahtar Kasası oluşturacaksınız. Azure Key Vault, güvenli bir gizli dizi deposu olarak çalışan bir bulut hizmetidir. Anahtarları, parolaları, sertifikaları ve diğer gizli dizileri güvenli bir şekilde depolayabilirsiniz. Key Vault hakkında daha fazla bilgi için [genel bakışı](../general/overview.md)gözden geçirebilirsiniz. Azure CLI, komut veya betikler kullanarak Azure kaynakları oluşturup yönetmek için kullanılır. Bunu tamamladıktan sonra bir sertifika depolayacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

CLı kullanarak Azure 'da oturum açmak için şunu yazabilirsiniz:

```azurecli
az login
```

CLı aracılığıyla oturum açma seçenekleri hakkında daha fazla bilgi için [Azure CLI ile oturum açma](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) bölümüne göz atın

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *ContosoResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli
az group create --name "ContosoResourceGroup" --location eastus
```

## <a name="create-a-key-vault"></a>Anahtar kasası oluşturma

Daha sonra, önceki adımda oluşturulan kaynak grubunda bir Key Vault oluşturacaksınız. Bazı bilgileri sağlamanız gerekir:

- Bu hızlı başlangıç için **Contoso-vault2** kullanılır. Testinizde benzersiz bir ad sağlamanız gerekir.
- Kaynak grubu adı **Contosoresourcegroup**.
- **Doğu ABD** konumu.

```azurecli
az keyvault create --name "Contoso-Vault2" --resource-group "ContosoResourceGroup" --location eastus
```

Bu cmdlet’in çıktısı, yeni oluşturulan Key Vault’un özelliklerini gösterir. Aşağıda listelenen iki özelliği not edin:

- **Kasa Adı**: Örnekte bu değer **Contoso-Vault2** şeklindedir. Bu adı diğer Key Vault komutları için kullanacaksınız.
- **Kasa URI’si**: Örnekte bu: https://contoso-vault2.vault.azure.net/. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

Bu noktada Azure hesabınız, bu yeni anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkili olan tek hesaptır.

## <a name="add-a-certificate-to-key-vault"></a>Key Vault bir sertifika ekleyin

Kasaya bir sertifika eklemek için, birkaç ek adım yapmanız yeterlidir. Bu sertifika bir uygulama tarafından kullanılabilir. 

**Örnek sertifika** adlı varsayılan ilkeyle otomatik olarak imzalanan bir sertifika oluşturmak için aşağıdaki komutları yazın:

```azurecli
az keyvault certificate create --vault-name "Contoso-Vault2" -n ExampleCertificate -p "$(az keyvault certificate get-default-policy)"
```

Artık Azure Key Vault ' a eklediğiniz sertifikaya, URI 'sini kullanarak başvurabilirsiniz. **https://Contoso-Vault2.vault.azure.net/certificates/ExampleCertificate**Geçerli sürümü almak için kullanın. 

Daha önce depolanan sertifikayı görüntülemek için:

```azurecli

az keyvault certificate show --name "ExampleCertificate" --vault-name "Contoso-Vault2"
```

Şimdi bir Key Vault oluşturdunuz, bir sertifikayı depolamıştır ve geri almıştır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer hızlı başlangıçlar ve öğreticiler bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu kaynakları yerinde bırakmanız yararlı olabilir.
Artık gerekli değilse, [az group delete](/cli/azure/group) komutunu kullanarak kaynak grubunu ve tüm ilgili kaynakları kaldırabilirsiniz. Kaynakları aşağıda gösterildiği gibi silebilirsiniz:

```azurecli
az group delete --name ContosoResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Key Vault oluşturup içinde bir sertifika depoladığınız. Key Vault ve uygulamalarınızla tümleştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere ilerleyin.

- [Azure Key Vault genel bakışını](../general/overview.md) okuyun
- Azure CLı için başvuruya bakın [az keykasa komutları](/cli/azure/keyvault?view=azure-cli-latest)
- [En iyi uygulamaları](../general/best-practices.md) gözden geçirin Azure Key Vault
