---
title: "Hızlı başlangıç: Azure Key Vault'tan gizli dizi ayarlama ve alma"
description: Azure CLI kullanarak Azure Key Vault'tan gizli dizi ayarlamayı ve almayı gösteren hızlı başlangıç
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.custom: mvc, seo-javascript-september2019, seo-javascript-october2019
ms.date: 09/03/2019
ms.author: mbaldwin
ms.openlocfilehash: 6f71dd0f928f75deff0a483dda0aed621d6ead19
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78197615"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-azure-cli"></a>Hızlı başlangıç: Azure CLI kullanarak Azure Key Vault'tan gizli dizi ayarlama ve alma

Bu hızlı başlangıçta, Azure CLI ile Azure Anahtar Kasası'nda önemli bir kasa oluşturursunuz. Azure Key Vault, güvenli bir gizli dizi deposu olarak çalışan bir bulut hizmetidir. Anahtarları, parolaları, sertifikaları ve diğer gizli dizileri güvenli bir şekilde depolayabilirsiniz. Key Vault hakkında daha fazla bilgi için [Genel Bakış'ı](key-vault-overview.md)inceleyebilirsiniz. Azure CLI, komut veya betikler kullanarak Azure kaynakları oluşturup yönetmek için kullanılır. Bu işlemi tamamladıktan sonra bir gizli dizli depolayacaksınız.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

CLI'yi kullanarak Azure'da oturum açabilmek için şunları yazabilirsiniz:

```azurecli
az login
```

CLI üzerinden giriş seçenekleri hakkında daha fazla bilgi için [Azure CLI ile oturum açma'ya](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) bir göz atın

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *ContosoResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli
az group create --name "ContosoResourceGroup" --location eastus
```

## <a name="create-a-key-vault"></a>Anahtar kasası oluşturma

Daha sonra, önceki adımda oluşturulan kaynak grubunda bir Key Vault oluşturacaksınız. Bazı bilgileri sağlamanız gerekir:

- Bu hızlı başlangıç için **Contoso-vault2** kullanılır. Testinizde benzersiz bir ad sağlamanız gerekir.
- Kaynak grubu adı **ContosoResourceGroup**.
- **Doğu ABD** konumu.

```azurecli
az keyvault create --name "Contoso-Vault2" --resource-group "ContosoResourceGroup" --location eastus
```

Bu cmdlet’in çıktısı, yeni oluşturulan Key Vault’un özelliklerini gösterir. Aşağıda listelenen iki özelliği not edin:

- **Kasa Adı**: Örnekte bu değer **Contoso-Vault2** şeklindedir. Bu adı diğer Key Vault komutları için kullanacaksınız.
- **Kasa URI’si**: Örnekte bu: https://contoso-vault2.vault.azure.net/. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

Bu noktada Azure hesabınız, bu yeni anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkili olan tek hesaptır.

## <a name="add-a-secret-to-key-vault"></a>Key Vault’a gizli dizi ekleme

Kasaya bir gizli dizi eklemek için birkaç ek adım uygulamanız gerekir. Bu parola bir uygulama tarafından kullanılabilir. Şifre **ExamplePassword** olarak adlandırılacak ve **içinde hVFkkk965BuUv** değerini saklar.

Anahtar **Vault'ta** **hVFkk965BuUv** değerini depolayacak ExamplePassword adlı bir sır oluşturmak için aşağıdaki komutları yazın:

```azurecli
az keyvault secret set --vault-name "Contoso-Vault2" --name "ExamplePassword" --value "hVFkk965BuUv"
```

Artık Azure Key Vault'a eklediğiniz bu parolaya URI'sini kullanarak başvurabilirsiniz. Geçerli **https://Contoso-Vault2.vault.azure.net/secrets/ExamplePassword** sürümü almak için kullanın. 

Gizli dizi içindeki değeri düz metin olarak görüntülemek için:

```azurecli
az keyvault secret show --name "ExamplePassword" --vault-name "Contoso-Vault2"
```

Artık bir Key Vault oluşturdunuz, bir gizli dizli depoladınız ve bunu aldınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer hızlı başlangıçlar ve öğreticiler bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu kaynakları yerinde bırakmanız yararlı olabilir.
Artık gerekli değilse, [az group delete](/cli/azure/group) komutunu kullanarak kaynak grubunu ve tüm ilgili kaynakları kaldırabilirsiniz. Kaynakları aşağıda gösterildiği gibi silebilirsiniz:

```azurecli
az group delete --name ContosoResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç size bir Key Vault oluşturdu ve içinde bir sır saklanmış. Key Vault ve uygulamalarınızla nasıl entegre edilebildiğini öğrenmek için aşağıdaki makalelere devam edin.

- Azure [Anahtar Kasasına Genel Bakış](key-vault-overview.md)
- [Azure CLI az keyvault komutları](/cli/azure/keyvault?view=azure-cli-latest) için başvuruya bakın
- [Anahtarlar, sırlar ve sertifikalar](about-keys-secrets-and-certificates.md) hakkında bilgi edinin
- Azure Key Vault en iyi uygulamalarını gözden [geçirin](key-vault-best-practices.md)
