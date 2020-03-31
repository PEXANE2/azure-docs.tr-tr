---
title: Kaynak sağlayıcı kayıt hataları
description: Azure Kaynak Yöneticisi ile kaynak dağıtırken Azure kaynak sağlayıcısı kayıt hatalarının nasıl çözüleceğini açıklar.
ms.topic: troubleshooting
ms.date: 02/15/2019
ms.openlocfilehash: a9182be53cc91240a62ab201efc53d674f7cf427
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273780"
---
# <a name="resolve-errors-for-resource-provider-registration"></a>Kaynak sağlayıcısı kaydı hatalarını giderme

Bu makalede, aboneliğinizde daha önce kullanmadığınız bir kaynak sağlayıcısı kullanırken karşılaşabileceğiniz hatalar açıklanmaktadır.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="symptom"></a>Belirti

Kaynak dağıtırken, aşağıdaki hata kodunu ve iletiyi alabilirsiniz:

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

Veya, şu durumları belirten benzer bir ileti alabilirsiniz:

```
Code: MissingSubscriptionRegistration
Message: The subscription is not registered to use namespace {resource-provider-namespace}
```

Hata iletisi, desteklenen konumlar ve API sürümleri için öneriler vermelidir. Şablonunuzu önerilen değerlerden biriyle değiştirebilirsiniz. Çoğu sağlayıcı, Azure portalı veya kullanmakta olduğunuz komut satırı arabirimi tarafından otomatik olarak kaydedilir, ancak hepsi tarafından kaydedilmez. Belirli bir kaynak sağlayıcısını daha önce kullanmadıysanız, bu sağlayıcıyı kaydetmeniz gerekebilir.

Veya sanal makineler için otomatik kapatmadevre dışı bırakıldığında, benzer bir hata iletisi alabilirsiniz:

```
Code: AuthorizationFailed
Message: The client '<identifier>' with object id '<identifier>' does not have authorization to perform action 'Microsoft.Compute/virtualMachines/read' over scope ...
```

## <a name="cause"></a>Nedeni

Bu hatalardan birini şu nedenlerden dolayı alırsınız:

* Gerekli kaynak sağlayıcısı aboneliğiniz için kaydedilmedi
* Kaynak türü için DESTEKlenmeyen API sürümü
* Kaynak türü için desteklenmeyen konum
* VM'lerin otomatik olarak kapatılması için Microsoft.DevTestLab kaynak sağlayıcısının kaydedilmesi gerekir.

## <a name="solution-1---powershell"></a>Çözüm 1 - PowerShell

PowerShell için, kayıt durumunuzu görmek için **Get-AzResourceProvider'ı** kullanın.

```powershell
Get-AzResourceProvider -ListAvailable
```

Bir sağlayıcı kaydetmek için **Register-AzResourceProvider'ı** kullanın ve kaydolmak istediğiniz kaynak sağlayıcısının adını sağlayın.

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Cdn
```

Belirli bir kaynak türü için desteklenen konumları almak için şunları kullanın:

```powershell
((Get-AzResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Belirli bir kaynak türü için desteklenen API sürümlerini almak için şunları kullanın:

```powershell
((Get-AzResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

## <a name="solution-2---azure-cli"></a>Çözüm 2 - Azure CLI

Sağlayıcının kayıtlı olup olmadığını görmek `az provider list` için komutu kullanın.

```azurecli-interactive
az provider list
```

Bir kaynak sağlayıcısı nı `az provider register` kaydetmek için komutu kullanın ve kaydolmak için *ad alanını* belirtin.

```azurecli-interactive
az provider register --namespace Microsoft.Cdn
```

Kaynak türü için desteklenen konumları ve API sürümlerini görmek için şunları kullanın:

```azurecli-interactive
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="solution-3---azure-portal"></a>Çözüm 3 - Azure portalı

Kayıt durumunu görebilir ve portal üzerinden bir kaynak sağlayıcısı ad alanı kaydedebilirsiniz.

1. Portaldan Tüm **hizmetleri**seçin.

   ![Tüm hizmetleri seçin](./media/error-register-resource-provider/select-all-services.png)

1. **Abonelikleri**seçin.

   ![Abonelikleri seçin](./media/error-register-resource-provider/select-subscriptions.png)

1. Abonelikler listesinden, kaynak sağlayıcısını kaydetmek için kullanmak istediğiniz aboneliği seçin.

   ![Kaynak sağlayıcısını kaydetmek için aboneliği seçin](./media/error-register-resource-provider/select-subscription-to-register.png)

1. Aboneliğiniz için **Kaynak sağlayıcılarını**seçin.

   ![Kaynak sağlayıcılar seçin](./media/error-register-resource-provider/select-resource-provider.png)

1. Kaynak sağlayıcıları listesine bakın ve gerekirse dağıtmaya çalıştığınız türdeki kaynak sağlayıcısını kaydetmek için **Kayıt bağlantısını** seçin.

   ![Kaynak sağlayıcılar listele](./media/error-register-resource-provider/list-resource-providers.png)
