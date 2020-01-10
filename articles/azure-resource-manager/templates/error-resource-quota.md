---
title: Quota errors
description: Describes how to resolve resource quota errors when deploying resources with Azure Resource Manager.
ms.topic: troubleshooting
ms.date: 03/09/2018
ms.openlocfilehash: d36f7d12f432839760b68c85e1c5761e0caf8d85
ms.sourcegitcommit: 51ed913864f11e78a4a98599b55bbb036550d8a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2020
ms.locfileid: "75659364"
---
# <a name="resolve-errors-for-resource-quotas"></a>Resolve errors for resource quotas

This article describes quota errors you may encounter when deploying resources.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="symptom"></a>Belirti

If you deploy a template that creates resources that exceed your Azure quotas, you get a deployment error that looks like:

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

Or, you may see:

```
Code=ResourceQuotaExceeded
Message=Creating the resource of type <resource-type> would exceed the quota of <number>
resources of type <resource-type> per resource group. The current resource count is <number>,
please delete some resources of this type before creating a new one.
```

## <a name="cause"></a>Nedeni

Kotalar kaynak grubu başına, aboneliklere, hesaplara ve diğer kapsamlara uygulanır. Örneğin aboneliğiniz bir bölge için çekirdek sayısını sınırlandıracak şekilde yapılandırılmış olabilir. İzin verilen miktardan daha fazla çekirdeği olan bir sanal makine dağıtmaya çalışırsanız, kotanın aşıldığını belirten bir hata alırsınız.
For complete quota information, see [Azure subscription and service limits, quotas, and constraints](../../azure-resource-manager/management/azure-subscription-service-limits.md).

## <a name="troubleshooting"></a>Sorun giderme

### <a name="azure-cli"></a>Azure CLI

For Azure CLI, use the `az vm list-usage` command to find virtual machine quotas.

```azurecli
az vm list-usage --location "South Central US"
```

Which returns:

```azurecli
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

For PowerShell, use the **Get-AzVMUsage** command to find virtual machine quotas.

```powershell
Get-AzVMUsage -Location "South Central US"
```

Which returns:

```powershell
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional Cores                         0   100 Count
Virtual Machines                             0 10000 Count
```

## <a name="solution"></a>Çözüm

To request a quota increase, go to the portal and file a support issue. In the support issue, request an increase in your quota for the region into which you want to deploy.

> [!NOTE]
> Remember that for resource groups, the quota is for each individual region, not for the entire subscription. If you need to deploy 30 cores in West US, you have to ask for 30 Resource Manager cores in West US. If you need to deploy 30 cores in any of the regions to which you have access, you should ask for 30 Resource Manager cores in all regions.
>
>

1. **Abonelikler**'i seçin.

   ![Abonelikler](./media/error-resource-quota/subscriptions.png)

2. Kotasını artırmanız gereken aboneliği seçin.

   ![Abonelik seçme](./media/error-resource-quota/select-subscription.png)

3. Select **Usage + quotas**

   ![Select usage and quotas](./media/error-resource-quota/select-usage-quotas.png)

4. In the upper right corner, select **Request increase**.

   ![Request increase](./media/error-resource-quota/request-increase.png)

5. Artırmanız gereken kota türünün formlarını doldurun.

   ![Fill in form](./media/error-resource-quota/forms.png)