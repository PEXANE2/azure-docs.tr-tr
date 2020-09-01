---
title: Kaynak sağlayıcısı kayıt hataları
description: Azure Resource Manager ile kaynak dağıtılırken Azure Kaynak sağlayıcısı kayıt hatalarının nasıl çözümlendiğini açıklar.
ms.topic: troubleshooting
ms.date: 02/15/2019
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 143cf03a33739f43a29af94fc2f8a336fb3aef8f
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89076676"
---
# <a name="resolve-errors-for-resource-provider-registration"></a>Kaynak sağlayıcısı kaydı hatalarını giderme

Bu makalede, aboneliğinizde daha önce kullanmadığınız bir kaynak sağlayıcısını kullanırken karşılaşabileceğiniz hatalar açıklanır.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="symptom"></a>Belirti

Kaynak dağıtıldığında aşağıdaki hata kodunu ve iletisini alabilirsiniz:

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

Ya da şunları belirten benzer bir ileti alabilirsiniz:

```
Code: MissingSubscriptionRegistration
Message: The subscription is not registered to use namespace {resource-provider-namespace}
```

Hata iletisi, desteklenen konumlar ve API sürümleri için öneriler vermelidir. Şablonunuzun önerilen değerlerden birine göre değişiklik yapabilirsiniz. Çoğu sağlayıcı, kullandığınız Azure portal veya komut satırı arabirimi tarafından otomatik olarak kaydedilir, ancak hepsini değil. Daha önce belirli bir kaynak sağlayıcısını kullanmadıysanız, bu sağlayıcıyı kaydetmeniz gerekebilir.

Ya da sanal makineler için otomatik kapanmaya devre dışı bıraktığınızda şuna benzer bir hata iletisi alabilirsiniz:

```
Code: AuthorizationFailed
Message: The client '<identifier>' with object id '<identifier>' does not have authorization to perform action 'Microsoft.Compute/virtualMachines/read' over scope ...
```

## <a name="cause"></a>Nedeni

Şu nedenlerden biri için bu hataları alırsınız:

* Gerekli kaynak sağlayıcısı aboneliğiniz için kayıtlı değil
* Kaynak türü için API sürümü desteklenmiyor
* Kaynak türü için konum desteklenmiyor
* VM 'lerin otomatik olarak kapatılmasını için, Microsoft. DevTestLab kaynak sağlayıcısının kayıtlı olması gerekir.

## <a name="solution-1---powershell"></a>Çözüm 1-PowerShell

PowerShell için, kayıt durumunuzu görmek için **Get-AzResourceProvider** ' ı kullanın.

```powershell
Get-AzResourceProvider -ListAvailable
```

Bir sağlayıcıyı kaydetmek için **register-AzResourceProvider** ' ı kullanın ve kaydetmek istediğiniz kaynak sağlayıcının adını sağlayın.

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Cdn
```

Belirli bir kaynak türü için desteklenen konumları almak için şunu kullanın:

```powershell
((Get-AzResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Belirli bir kaynak türü için desteklenen API sürümlerini almak için şunu kullanın:

```powershell
((Get-AzResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

## <a name="solution-2---azure-cli"></a>Çözüm 2-Azure CLı

Sağlayıcının kayıtlı olup olmadığını görmek için `az provider list` komutunu kullanın.

```azurecli-interactive
az provider list
```

Bir kaynak sağlayıcısını kaydetmek için `az provider register` komutunu kullanın ve kaydolmak üzere *ad alanını* belirtin.

```azurecli-interactive
az provider register --namespace Microsoft.Cdn
```

Kaynak türü için desteklenen konumları ve API sürümlerini görmek için şunu kullanın:

```azurecli-interactive
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="solution-3---azure-portal"></a>Çözüm 3-Azure portal

Kayıt durumunu görebilir ve Portal aracılığıyla bir kaynak sağlayıcısı ad alanı kaydedebilirsiniz.

1. Portaldan **tüm hizmetler**' i seçin.

   ![Tüm hizmetleri seçin](./media/error-register-resource-provider/select-all-services.png)

1. **Abonelikler**' i seçin.

   ![Abonelikleri Seç](./media/error-register-resource-provider/select-subscriptions.png)

1. Abonelikler listesinden, kaynak sağlayıcısını kaydetmek için kullanmak istediğiniz aboneliği seçin.

   ![Kaynak sağlayıcısını kaydetmek için abonelik seçin](./media/error-register-resource-provider/select-subscription-to-register.png)

1. Aboneliğiniz için **kaynak sağlayıcıları**' nı seçin.

   ![Kaynak sağlayıcılarını seçin](./media/error-register-resource-provider/select-resource-provider.png)

1. Kaynak sağlayıcıları listesine bakın ve gerekirse, dağıtmaya çalıştığınız türün kaynak sağlayıcısını kaydetmek için **Kaydet** bağlantısını seçin.

   ![Kaynak sağlayıcılarını listeleme](./media/error-register-resource-provider/list-resource-providers.png)
