---
title: Azure CLı ile Azure kanıtlama ayarlama
description: Azure CLı kullanarak bir kanıtlama sağlayıcısı ayarlama ve yapılandırma.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: quickstart
ms.date: 11/20/2020
ms.author: mbaldwin
ms.openlocfilehash: df1064128d6a4eca8497ebf3ea0c6ae8cd42255c
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98208446"
---
# <a name="quickstart-set-up-azure-attestation-with-azure-cli"></a>Hızlı başlangıç: Azure CLı ile Azure kanıtlama ayarlama

Azure [CLI kullanarak Azure kanıtlama](/cli/azure/ext/attestation/attestation?view=azure-cli-latest)ile çalışmaya başlayın.

## <a name="get-started"></a>başlarken

1. Aşağıdaki CLı komutunu kullanarak bu uzantıyı yükler

   ```azurecli
   az extension add --name attestation
   ```
   
1. Sürümü denetleyin

   ```azurecli
   az extension show --name attestation --query version
   ```

1. Azure 'da oturum açmak için aşağıdaki komutu kullanın:

   ```azurecli
   az login
   ```

1. Gerekirse, Azure kanıtlama için aboneliğe geçin:

   ```azurecli
   az account set --subscription 00000000-0000-0000-0000-000000000000
   ```

1. [Az Provider Register](/cli/azure/provider#az_provider_register) komutuyla abonelikte Microsoft. kanıtlama kaynak sağlayıcısını kaydedin:

   ```azurecli
   az provider register --name Microsoft.Attestation
   ```

   Azure kaynak sağlayıcıları ve bunları yapılandırma ve yönetme hakkında daha fazla bilgi için bkz. [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md).

   > [!NOTE]
   > Bir abonelik için yalnızca bir kaynak sağlayıcısını kaydetmeniz gerekir.

1. Kanıtlama sağlayıcısı için bir kaynak grubu oluşturun. Diğer Azure kaynaklarını, istemci uygulaması örneği olan bir sanal makine dahil olmak üzere aynı kaynak grubuna yerleştirebilirsiniz. Bir kaynak grubu oluşturmak için [az Group Create](/cli/azure/group#az_group_create) komutunu çalıştırın veya var olan bir kaynak grubunu kullanın:

   ```azurecli
   az group create --name attestationrg --location uksouth
   ```

## <a name="create-and-manage-an-attestation-provider"></a>Kanıtlama sağlayıcısı oluşturma ve yönetme

Kanıtlama sağlayıcısı oluşturmak ve yönetmek için kullanabileceğiniz komutlar şunlardır:

1. İlke imzalama gereksinimi olmadan bir kanıtlama sağlayıcısı oluşturmak için [az kanıtlama Create](/cli/azure/ext/attestation/attestation?view=azure-cli-latest#ext_attestation_az_attestation_create) komutunu çalıştırın:

   ```azurecli
   az attestation create --name "myattestationprovider" --resource-group "MyResourceGroup" --location westus
   ```
   
1. Status ve AttestURI gibi kanıtlama sağlayıcısı özelliklerini almak için [az kanıtlama Show](/cli/azure/ext/attestation/attestation?view=azure-cli-latest#ext_attestation_az_attestation_show) komutunu çalıştırın:

   ```azurecli
   az attestation show --name "myattestationprovider" --resource-group "MyResourceGroup"
   ```

   Bu komut aşağıdaki çıktı gibi değerleri görüntüler:

   ```output
   Id:/subscriptions/MySubscriptionID/resourceGroups/MyResourceGroup/providers/Microsoft.Attestation/attestationProviders/MyAttestationProvider
   Location: MyLocation
   ResourceGroupName: MyResourceGroup
   Name: MyAttestationProvider
   Status: Ready
   TrustModel: AAD
   AttestUri: https://MyAttestationProvider.us.attest.azure.net
   Tags:
   TagsTable:
   ```

[Az kanıtlama Delete](/cli/azure/ext/attestation/attestation?view=azure-cli-latest#ext_attestation_az_attestation_delete) komutunu kullanarak bir kanıtlama sağlayıcısını silebilirsiniz:

```azurecli
az attestation delete --name "myattestationprovider" --resource-group "sample-resource-group"
```

## <a name="policy-management"></a>İlke yönetimi

Bir kanıtlama sağlayıcısı için ilke yönetimi sağlamak için burada açıklanan komutları ve tek seferde bir kanıtlama türünü kullanın.

[Az kanıtlama ilkesi Show](/cli/azure/ext/attestation/attestation/policy?view=azure-cli-latest#ext_attestation_az_attestation_policy_show) komutu, belirtilen t için geçerli ilkeyi döndürür:

```azurecli
az attestation policy show --name "myattestationprovider" --resource-group "MyResourceGroup" --attestation-type SGX-IntelSDK
```

> [!NOTE]
> Komut, ilkeyi hem metin hem de JWT biçiminde görüntüler.

Aşağıda desteklenen t türleri verilmiştir:

- `SGX-IntelSDK`
- `SGX-OpenEnclaveSDK`
- `TPM`

Belirtilen kanıtlama türü için yeni bir ilke ayarlamak için [az kanıtlama ilkesi set](/cli/azure/ext/attestation/attestation/policy?view=azure-cli-latest#ext_attestation_az_attestation_policy_set) komutunu kullanın.

Dosya yolunu kullanarak belirli bir kanıtlama türü türünün metin biçiminde ilke ayarlamak için:

```azurecli
az attestation policy set --name testatt1 --resource-group testrg --attestation-type SGX-IntelSDK --new-attestation-policy-file "{file_path}"
```

Dosya yolunu kullanarak belirli bir kanıtlama türü türünün JWT biçiminde ilke ayarlamak için:

```azurecli
az attestation policy set --name "myattestationprovider" --resource-group "MyResourceGroup" \
--attestation-type SGX-IntelSDK -f "{file_path}" --policy-format JWT
```

## <a name="next-steps"></a>Sonraki adımlar

- [Kanıtlama ilkesi yazma ve imzalama](author-sign-policy.md)
- [Kod örneklerini kullanarak bir SGX şifrelemesi ile kanıtlama uygulama](/samples/browse/?expanded=azure&terms=attestation)
