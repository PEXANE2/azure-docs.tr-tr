---
title: Azure CLı ile Azure kanıtlama ayarlama
description: Azure CLı kullanarak bir kanıtlama sağlayıcısı ayarlama ve yapılandırma.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: quickstart
ms.date: 11/20/2020
ms.author: mbaldwin
ms.openlocfilehash: e5ffcd56be1ce7427697cd3a75bd7c4232cc3b8a
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94667623"
---
# <a name="quickstart-set-up-azure-attestation-with-azure-cli"></a>Hızlı başlangıç: Azure CLı ile Azure kanıtlama ayarlama

Kanıtlama ayarlamak için Azure CLı kullanarak Azure kanıtlama ile çalışmaya başlayın.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

## <a name="get-started"></a>başlarken

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

Bir kanıtlama sağlayıcısı oluşturmak ve yönetmek için bu yordamı izleyin.

1. Bir kanıtlama sağlayıcısı oluşturmak için [az kanıtlama Create](/cli/azure/ext/attestation/attestation#ext_attestation_az_attestation_create) komutunu çalıştırın:

   ```azurecli
   az attestation create --resource-group attestationrg --name attestationProvider --location uksouth \
      --attestation-policy SgxDisableDebugMode --certs-input-path C:\test\policySignersCertificates.pem
   ```

   **--CERT-Input-Path** parametresi bir güvenilen İmzalama anahtarları kümesini belirtir. Bu parametre için bir dosya adı belirtirseniz, kanıtlama sağlayıcısının yalnızca imzalı JWT biçimindeki ilkelerle yapılandırılması gerekir. Aksi takdirde, ilke metin veya işaretsiz JWT biçiminde yapılandırılabilir. JWT hakkında daha fazla bilgi için bkz. [temel kavramlar](basic-concepts.md). Sertifika örnekleri için bkz. [bir kanıtlama ilkesi imzalayan sertifikası örnekleri](policy-signer-examples.md).

1. Status ve AttestURI gibi kanıtlama sağlayıcısı özelliklerini almak için [az kanıtlama Show](/cli/azure/ext/attestation/attestation#ext_attestation_az_attestation_show) komutunu çalıştırın:

   ```azurecli
   az attestation show --resource-group attestationrg --name attestationProvider
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

[Az kanıtlama Delete](/cli/azure/ext/attestation/attestation#ext_attestation_az_attestation_delete) komutunu kullanarak bir kanıtlama sağlayıcısını silebilirsiniz:

```azurecli
az attestation delete --resource-group attestationrg --name attestationProvider
```

## <a name="policy-management"></a>İlke yönetimi

İlkeleri yönetmek için bir Azure AD kullanıcısı için aşağıdaki izinleri gerektirir `Actions` :

- `Microsoft.Attestation/attestationProviders/attestation/read`
- `Microsoft.Attestation/attestationProviders/attestation/write`
- `Microsoft.Attestation/attestationProviders/attestation/delete`

Bu izinler, `Owner` (joker karakter izinleri), `Contributor` (joker karakter izinleri) veya `Attestation Contributor` (yalnızca Azure kanıtlama için özel izinler) gibi bir rol aracılığıyla bir Azure AD kullanıcısına atanabilir.  

İlkeleri okumak için bir Azure AD kullanıcısı için aşağıdaki izinleri gerektirir `Actions` :

- `Microsoft.Attestation/attestationProviders/attestation/read`

Bu izin, `Reader` (joker karakter izinleri) veya `Attestation Reader` (yalnızca Azure kanıtlama için özel izinler) gibi bir rol aracılığıyla BIR Azure AD kullanıcısına atanabilir.

Bir kanıtlama sağlayıcısına yönelik ilke yönetimi sağlamak için burada açıklanan komutları kullanın, tek seferde bir t.

[Az kanıtlama ilkesi Show](/cli/azure/ext/attestation/attestation/policy#ext_attestation_az_attestation_policy_show) komutu, belirtilen t için geçerli ilkeyi döndürür:

```azurecli
az attestation policy show --resource-group attestationrg --name attestationProvider --tee SgxEnclave
```

> [!NOTE]
> Komut, ilkeyi hem metin hem de JWT biçiminde görüntüler.

Aşağıda desteklenen t türleri verilmiştir:

- `CyResComponent`
- `OpenEnclave`
- `SgxEnclave`
- `VSMEnclave`

Belirtilen t için yeni bir ilke ayarlamak için [az kanıtlama ilkesi set](/cli/azure/ext/attestation/attestation/policy#ext_attestation_az_attestation_policy_set) komutunu kullanın.

```azurecli
az attestation policy set --resource-group attestationrg --name attestationProvider --tee SgxEnclave \
   --new-attestation-policy newAttestationPolicyname
```

JWT biçimindeki kanıtlama ilkesinin adlı bir talep içermesi gerekir `AttestationPolicy` . İmzalı bir ilke, mevcut ilke imzalayan sertifikalara karşılık gelen bir anahtarla imzalanmalıdır.

İlke örnekleri için bkz. [kanıtlama Ilkesi örnekleri](policy-examples.md).

[Az kanıtlama ilkesi sıfırlama](/cli/azure/ext/attestation/attestation/policy#ext_attestation_az_attestation_policy_reset) komutu belirtilen t için yeni bir ilke ayarlar.

```azurecli
az attestation policy reset --resource-group attestationrg --name attestationProvider --tee SgxEnclave \
   --policy-jws "eyJhbGciOiJub25lIn0.."
```

## <a name="policy-signer-certificates-management"></a>İlke imzalayan sertifikaları yönetimi

Bir kanıtlama sağlayıcısına yönelik ilke imzalayan sertifikalarını yönetmek için aşağıdaki komutları kullanın:

```azurecli
az attestation signer list --resource-group attestationrg --name attestationProvider

az attestation signer add --resource-group attestationrg --name attestationProvider \
   --signer "eyAiYWxnIjoiUlMyNTYiLCAie..."

az attestation signer remove --resource-group attestationrg --name attestationProvider \
   --signer "eyAiYWxnIjoiUlMyNTYiLCAie..."
```

İlke imzalayan sertifikası, adlı bir talebe sahip imzalı bir JWT `maa-policyCertificate` . Talebin değeri, eklenecek güvenilir imzalama anahtarını içeren bir JWK değeridir. JWT, mevcut ilke imzalayan sertifikalarına karşılık gelen bir özel anahtarla imzalanmalıdır. JWT ve JWK hakkında daha fazla bilgi için bkz. [temel kavramlar](basic-concepts.md).

İlke imzalayan sertifikasının tüm anlam düzenlemesi, Azure CLı dışında yapılmalıdır. Azure CLı ile ilgili olduğu kadar basit bir dizedir.

Sertifika örnekleri için bkz. [bir kanıtlama ilkesi imzalayan sertifikası örnekleri](policy-signer-examples.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Kanıtlama ilkesi yazma ve imzalama](author-sign-policy.md)
- [Kod örneklerini kullanarak bir SGX şifrelemesi ile kanıtlama uygulama](/samples/browse/?expanded=azure&terms=attestation)
