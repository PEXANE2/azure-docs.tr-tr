---
title: Kaynak sağlayıcısı kayıt hataları
description: Azure Resource Manager ile kaynak dağıtılırken Azure Kaynak sağlayıcısı kayıt hatalarının nasıl çözümlendiğini açıklar.
ms.topic: troubleshooting
ms.date: 02/15/2019
ms.openlocfilehash: a9182be53cc91240a62ab201efc53d674f7cf427
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78390064"
---
# <a name="resolve-errors-for-resource-provider-registration"></a>Kaynak Sağlayıcısı kaydı için hataları çözümleyin

Bu makalede aboneliğinizde daha önce kullanmadığınız bir kaynak sağlayıcısı kullanırken karşılaşabileceğiniz hatalar açıklanır.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="symptom"></a>Belirti

Kaynak dağıtım yaparken, aşağıdaki hata kodu ve şu iletiyle alabilirsiniz:

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

Veya bildiren benzer bir ileti alabilirsiniz:

```
Code: MissingSubscriptionRegistration
Message: The subscription is not registered to use namespace {resource-provider-namespace}
```

Hata iletisi desteklenen konumları ve API sürümleri için öneriler vermeniz gerekir. Önerilen değerler birine şablonunuzu değiştirebilirsiniz. Azure portalı veya komut satırı arabirimi, kullanmakta olduğunuz tarafından otomatik olarak kayıtlı ancak tüm çoğu sağlayıcıları. Bir kaynak sağlayıcısı önce kullanmadıysanız, bu sağlayıcıyı kaydetmek gerekebilir.

Ya da sanal makineler için otomatik kapanmaya devre dışı bıraktığınızda şuna benzer bir hata iletisi alabilirsiniz:

```
Code: AuthorizationFailed
Message: The client '<identifier>' with object id '<identifier>' does not have authorization to perform action 'Microsoft.Compute/virtualMachines/read' over scope ...
```

## <a name="cause"></a>Nedeni

Şu nedenlerden biri için bu hataları alırsınız:

* Gerekli kaynak sağlayıcısı aboneliğiniz için kayıtlı değil
* Kaynak türü için desteklenmeyen API sürümü
* Konum kaynak türü için desteklenmiyor
* VM 'lerin otomatik olarak kapatılmasını için, Microsoft. DevTestLab kaynak sağlayıcısının kayıtlı olması gerekir.

## <a name="solution-1---powershell"></a>Çözüm 1 - PowerShell

PowerShell için, kayıt durumunuzu görmek için **Get-AzResourceProvider** ' ı kullanın.

```powershell
Get-AzResourceProvider -ListAvailable
```

Bir sağlayıcıyı kaydetmek için **register-AzResourceProvider** ' ı kullanın ve kaydetmek istediğiniz kaynak sağlayıcının adını sağlayın.

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Cdn
```

Belirli bir kaynak türü için desteklenen konumlar almak için kullanın:

```powershell
((Get-AzResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Belirli bir kaynak türü için desteklenen API sürümlerini almak için kullanın:

```powershell
((Get-AzResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

## <a name="solution-2---azure-cli"></a>Çözüm 2 - Azure CLI

Sağlayıcının kayıtlı olup olmadığını görmek için `az provider list` komutunu kullanın.

```azurecli-interactive
az provider list
```

Bir kaynak sağlayıcısını kaydetmek için `az provider register` komutunu kullanın ve kaydolmak için *ad alanını* belirtin.

```azurecli-interactive
az provider register --namespace Microsoft.Cdn
```

Desteklenen konumlar ve bir kaynak türü için API sürümlerini görmek için bu seçeneği kullanın:

```azurecli-interactive
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="solution-3---azure-portal"></a>Çözüm 3 - Azure portalı

Kayıt durumunu görmek ve bir kaynak sağlayıcısı ad alanı Portalı aracılığıyla kaydolun.

1. Portaldan **tüm hizmetler**' i seçin.

   ![Tüm hizmetleri seçin](./media/error-register-resource-provider/select-all-services.png)

1. **Abonelikler**'i seçin.

   ![Abonelikleri seçin](./media/error-register-resource-provider/select-subscriptions.png)

1. Abonelikler listesinden, kaynak sağlayıcısını kaydetmek için kullanmak istediğiniz aboneliği seçin.

   ![Kaynak sağlayıcısını kaydetmek için bir abonelik seçin](./media/error-register-resource-provider/select-subscription-to-register.png)

1. Aboneliğiniz için **kaynak sağlayıcıları**' nı seçin.

   ![Kaynak sağlayıcılarını seçin](./media/error-register-resource-provider/select-resource-provider.png)

1. Kaynak sağlayıcıları listesine bakın ve gerekirse, dağıtmaya çalıştığınız türün kaynak sağlayıcısını kaydetmek için **Kaydet** bağlantısını seçin.

   ![Kaynak sağlayıcıları listesi](./media/error-register-resource-provider/list-resource-providers.png)
