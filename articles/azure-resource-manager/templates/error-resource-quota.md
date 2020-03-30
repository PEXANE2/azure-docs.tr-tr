---
title: Kota hataları
description: Azure Kaynak Yöneticisi ile kaynak dağıtırken kaynak kotası hatalarının nasıl çözüleceğini açıklar.
ms.topic: troubleshooting
ms.date: 03/09/2018
ms.openlocfilehash: 410b086b39d63d03491d390364f4aec6300fc7c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273793"
---
# <a name="resolve-errors-for-resource-quotas"></a>Kaynak kotası hatalarını düzeltme

Bu makalede, kaynakları dağıtırken karşılaşabileceğiniz kota hataları açıklanmaktadır.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="symptom"></a>Belirti

Azure kotalarınızı aşan kaynaklar oluşturan bir şablon dağıtırsanız, şu na benzer bir dağıtım hatası alırsınız:

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

Veya şunları görebilirsiniz:

```
Code=ResourceQuotaExceeded
Message=Creating the resource of type <resource-type> would exceed the quota of <number>
resources of type <resource-type> per resource group. The current resource count is <number>,
please delete some resources of this type before creating a new one.
```

## <a name="cause"></a>Nedeni

Kotalar kaynak grubu başına, aboneliklere, hesaplara ve diğer kapsamlara uygulanır. Örneğin aboneliğiniz bir bölge için çekirdek sayısını sınırlandıracak şekilde yapılandırılmış olabilir. İzin verilen miktardan daha fazla çekirdeği olan bir sanal makine dağıtmaya çalışırsanız, kotanın aşıldığını belirten bir hata alırsınız.
Tam kota bilgileri için Azure [abonelik ve hizmet sınırları, kotalar ve kısıtlamalara](../../azure-resource-manager/management/azure-subscription-service-limits.md)bakın.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="azure-cli"></a>Azure CLI

Azure CLI için `az vm list-usage` sanal makine kotalarını bulmak için komutu kullanın.

```azurecli
az vm list-usage --location "South Central US"
```

Hangi döner:

```output
[
  {
    "currentValue": 0,
    "limit": 2000,
    "name": {
      "localizedValue": "Availability Sets",
      "value": "availabilitySets"
    }
  },
  ...
]
```

### <a name="powershell"></a>PowerShell

PowerShell için, sanal makine kotalarını bulmak için **Get-AzVMUsage** komutunu kullanın.

```powershell
Get-AzVMUsage -Location "South Central US"
```

Hangi döner:

```output
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional Cores                         0   100 Count
Virtual Machines                             0 10000 Count
```

## <a name="solution"></a>Çözüm

Kota artışı istemek için portala gidin ve bir destek sorunu dosyala. Destek sorununda, dağıtmak istediğiniz bölge için kotanızda bir artış isteyin.

> [!NOTE]
> Kaynak grupları için kotanın tüm abonelik için değil, her bir bölge için olduğunu unutmayın. Batı ABD'de 30 çekirdek dağıtmanız gerekiyorsa, Batı ABD'de 30 Kaynak Yöneticisi çekirdeği istemeniz gerekir. Erişiminiz olan bölgelerden herhangi birinde 30 çekirdek dağıtmanız gerekiyorsa, tüm bölgelerde 30 Kaynak Yöneticisi çekirdeği istemeniz gerekir.
>
>

1. **Abonelikleri**seçin.

   ![Abonelikler](./media/error-resource-quota/subscriptions.png)

2. Kotasını artırmanız gereken aboneliği seçin.

   ![Abonelik seçme](./media/error-resource-quota/select-subscription.png)

3. **Kullanım + kotaları** seçin

   ![Kullanımı ve kotaları seçin](./media/error-resource-quota/select-usage-quotas.png)

4. Sağ üst **köşede, Artış İste'yi**seçin.

   ![İstek artışı](./media/error-resource-quota/request-increase.png)

5. Artırmanız gereken kota türünün formlarını doldurun.

   ![Formu doldurun](./media/error-resource-quota/forms.png)