---
title: Hızlı başlangıç-Azure CLı ile Azure Key Vault oluşturma
description: Azure CLı kullanarak Azure Key Vault oluşturmayı gösteren hızlı başlangıç
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: quickstart
ms.date: 07/20/2020
ms.author: mbaldwin
ms.openlocfilehash: 2c71e96dc1c92fc94d403e7c721b2b9f3149b8e9
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87102240"
---
# <a name="quickstart-create-a-key-vault-using-the-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak bir Anahtar Kasası oluşturma

Azure Key Vault [anahtarlar](../keys/index.yml), [gizli](../secrets/index.yml)diziler ve [Sertifikalar](../certificates/index.yml)için güvenli bir mağaza sağlayan bir bulut hizmetidir. Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault hakkında](overview.md); anahtar kasasında nelerin depolanabileceği hakkında daha fazla bilgi için bkz. [anahtarlar, gizlilikler ve sertifikalar hakkında](about-keys-secrets-certificates.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Bu hızlı başlangıçta, [Azure CLI](/cli/azure/)ile bir Anahtar Kasası oluşturacaksınız. Azure CLı, komutları veya betikleri kullanarak Azure kaynakları oluşturmak ve yönetmek için kullanılır.  CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

CLı kullanarak Azure 'da oturum açmak için şunu yazabilirsiniz:

```azurecli
az login
```

CLı aracılığıyla oturum açma seçenekleri hakkında daha fazla bilgi için [Azure CLI ile oturum açma](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) bölümüne göz atın

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *ContosoResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli
az group create --name "myResourceGroup" -l "EastUS"
```

## <a name="create-a-key-vault"></a>Anahtar kasası oluşturma

Önceki adımdan kaynak grubunda bir Key Vault oluşturun. Bazı bilgileri sağlamanız gerekir:

- Anahtar Kasası adı: yalnızca rakam (0-9), harf (A-z, A-Z) ve kısa çizgi (-) içerebilen 3 ile 24 karakter arasında bir dize

  > [!Important]
  > Her Anahtar Kasası benzersiz bir ada sahip olmalıdır. -Unique-keykasa-adı> <aşağıdaki örneklerde anahtar kasanızın adıyla değiştirin.

- Kaynak grubu adı: **Myresourcegroup**.
- Konum: **EastUS**.

```azurecli
az keyvault create --name "<your-unique-keyvault-name>" --resource-group "myResourceGroup" --location "EastUS"
```

Bu cmdlet’in çıktısı, yeni oluşturulan anahtar kasasının özelliklerini gösterir. Aşağıda listelenen iki özelliği not edin:

- **Kasa adı**: Yukarıdaki--name parametresine verdiğiniz addır.
- **Kasa URI 'si**: örnekte bu https://- &lt; Unique-keykasasına-Name &gt; . Vault.Azure.net/. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

Bu noktada Azure hesabınız, bu yeni anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkili olan tek hesaptır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer hızlı başlangıçlar ve öğreticiler bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu kaynakları yerinde bırakmanız yararlı olabilir.

Artık gerekli değilse, [az group delete](/cli/azure/group) komutunu kullanarak kaynak grubunu ve tüm ilgili kaynakları kaldırabilirsiniz. Kaynakları aşağıda gösterildiği gibi silebilirsiniz:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Key Vault oluşturup silmiş olursunuz. Key Vault ve uygulamalarınızla tümleştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere ilerleyin.

- [Azure Key Vault genel bakışını](overview.md) okuyun
- Azure CLı için başvuruya bakın [az keykasa komutları](/cli/azure/keyvault?view=azure-cli-latest)
- [En iyi uygulamaları](best-practices.md) gözden geçirin Azure Key Vault
