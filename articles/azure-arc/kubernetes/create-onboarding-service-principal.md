---
title: Azure Arc etkin Kubernetes için bir ekleme hizmeti sorumlusu oluşturma
services: azure-arc
ms.service: azure-arc
ms.date: 03/03/2021
ms.topic: article
author: mlearned
ms.author: mlearned
description: 'Azure Arc etkin ekleme hizmeti sorumlusu oluşturma '
keywords: Kubernetes, yay, Azure, kapsayıcılar
ms.openlocfilehash: 610b355073473f0e492350753a523b7943666f13
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102121754"
---
# <a name="create-an-onboarding-service-principal-for-azure-arc-enabled-kubernetes"></a>Azure Arc etkin Kubernetes için bir ekleme hizmeti sorumlusu oluşturma

## <a name="overview"></a>Genel Bakış

Kubernetes kümelerini, sınırlı ayrıcalıklı rol atamaları olan hizmet sorumlularını kullanarak Azure yaya bağlayabilirsiniz. Bu özellik, Azure Pipelines ve GitHub eylemleri gibi sürekli tümleştirme ve sürekli dağıtım (CI/CD) Işlem hatları için yararlıdır.

Kubernetes kümelerini Azure yaya bağlamak için hizmet sorumlularını nasıl kullanacağınızı öğrenmek için aşağıdaki adımları izleyin.

## <a name="create-a-new-service-principal"></a>Yeni bir hizmet sorumlusu oluşturun

Azure Active Directory kiracınız için benzersiz olan bilgilendirici bir adla yeni bir hizmet sorumlusu oluşturun.

```console
az ad sp create-for-RBAC --skip-assignment --name "https://azure-arc-for-k8s-onboarding"
```

**Çıktıların**

```console
{
  "appId": "22cc2695-54b9-49c1-9a73-2269592103d8",
  "displayName": "azure-arc-for-k8s-onboarding",
  "name": "https://azure-arc-for-k8s-onboarding",
  "password": "09d3a928-b223-4dfe-80e8-fed13baa3b3d",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47"
}
```

## <a name="assign-permissions"></a>İzinler atama

Yeni oluşturulan hizmet sorumlusuna "Kubernetes kümesi-Azure Arc ekleme" rolünü atayın. Sınırlı izinlerle bu yerleşik Azure rolü yalnızca sorumlunun kümeleri Azure 'a kaydetmesini sağlar. Bu atanan role sahip asıl öğe, aboneliğin içindeki diğer kümeleri veya kaynakları güncelleştiremez, silemez veya değiştiremezler.

Müşteriler, sınırlı yetenekler verildiğinde, bu sorumluyu birden çok küme eklemek için kolayca yeniden kullanabilir.

Rolü atarken uygun bağımsız değişkeni geçirerek, izinleri daha fazla sınırlandırabilirsiniz `--scope` . Bu, yöneticilerin küme kaydını abonelik veya kaynak grubu kapsamıyla kısıtlayasağlar. Aşağıdaki senaryolar çeşitli parametreler tarafından desteklenir `--scope` :

| Kaynak  | `scope` bağımsız değişkeni| Etki |
| ------------- | ------------- | ------------- |
| Abonelik | `--scope /subscriptions/0b1f6471-1bf0-4dda-aec3-111122223333` | Hizmet sorumlusu, kümeyi bu abonelik kapsamındaki herhangi bir kaynak grubuna kaydedebilir. |
| Kaynak Grubu | `--scope /subscriptions/0b1f6471-1bf0-4dda-aec3-111122223333/resourceGroups/myGroup`  | Hizmet sorumlusu __yalnızca__ kaynak grubundaki kümeleri kaydedebilir `myGroup` . |

```console
az role assignment create \
    --role 34e09817-6cbe-4d01-b1a2-e0eac5743d41 \      # this is the id for the built-in role
    --assignee 22cc2695-54b9-49c1-9a73-2269592103d8 \  # use the appId from the new SP
    --scope /subscriptions/<<SUBSCRIPTION_ID>>         # apply the appropriate scope
```

**Çıktıların**

```console
{
  "canDelegate": null,
  "id": "/subscriptions/<<SUBSCRIPTION_ID>>/providers/Microsoft.Authorization/roleAssignments/fbd819a9-01e8-486b-9eb9-f177ba400ba6",
  "name": "fbd819a9-01e8-486b-9eb9-f177ba400ba6",
  "principalId": "ddb0ddb4-ba84-4cde-b936-affc272a4b90",
  "principalType": "ServicePrincipal",
  "roleDefinitionId": "/subscriptions/<<SUBSCRIPTION_ID>>/providers/Microsoft.Authorization/roleDefinitions/34e09817-6cbe-4d01-b1a2-e0eac5743d41",
  "scope": "/subscriptions/<<SUBSCRIPTION_ID>>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="use-service-principal-with-the-azure-cli"></a>Azure CLı ile hizmet sorumlusu kullanma

Yeni oluşturulan hizmet sorumlusuna aşağıdaki komutlarla başvurun:

```azurecli
az login --service-principal -u mySpnClientId -p mySpnClientSecret --tenant myTenantID
az connectedk8s connect -n myConnectedClusterName -g myResoureGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure ilkesi 'ni kullanarak](./use-azure-policy.md)küme yapılandırmanızı yönetir.
