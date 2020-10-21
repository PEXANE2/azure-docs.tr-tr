---
title: Service Fabric yönetilen kümenin düğüm türlerini ekleme ve kaldırma (Önizleme)
description: Bu öğreticide, Service Fabric yönetilen bir kümenin düğüm türlerini ekleme ve kaldırma hakkında bilgi edinin.
ms.topic: tutorial
ms.date: 09/28/2020
ms.openlocfilehash: 51cc83b4accae5f2791ce378e30f6fa692446b1c
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92316203"
---
# <a name="tutorial-add-and-remove-node-types-from-a-service-fabric-managed-cluster-preview"></a>Öğretici: Service Fabric yönetilen bir kümeden düğüm türleri ekleme ve kaldırma (Önizleme)

Bu öğretici serisinde şunları tartışacağız:

> [!div class="checklist"]
> * [Service Fabric yönetilen bir küme dağıtma](tutorial-managed-cluster-deploy.md)
> * [Service Fabric yönetilen bir kümeyi daraltma](tutorial-managed-cluster-scale.md)
> * Service Fabric yönetilen bir kümede düğüm ekleme ve kaldırma
> * [Service Fabric yönetilen bir kümeye uygulama dağıtma](tutorial-managed-cluster-deploy-app.md)

Serinin bu bölümü aşağıdaki konuları içerir:

> [!div class="checklist"]
> * Service Fabric yönetilen bir kümeye düğüm türü ekleme
> * Service Fabric yönetilen bir kümeden düğüm türünü silme

## <a name="prerequisites"></a>Ön koşullar

* Service Fabric yönetilen bir küme (bkz. [*yönetilen küme dağıtma*](tutorial-managed-cluster-deploy.md)).
* [Azure PowerShell 4.7.0](/powershell/azure/release-notes-azureps?preserve-view=true&view=azps-4.7.0#azservicefabric) veya üzeri (bkz. [*Install Azure PowerShell*](/powershell/azure/install-az-ps?preserve-view=true&view=azps-4.7.0)).

## <a name="add-a-node-type-to-a-service-fabric-managed-cluster"></a>Service Fabric yönetilen bir kümeye düğüm türü ekleme

Bir Azure Resource Manager şablonu, PowerShell veya CLı aracılığıyla Service Fabric yönetilen bir kümeye düğüm türü ekleyebilirsiniz. Bu öğreticide, Azure PowerShell kullanarak bir düğüm türü ekleyeceğiz.

Yeni bir düğüm türü oluşturmak için üç özellik tanımlamanız gerekir:

* **Düğüm türü adı**: kümedeki var olan herhangi bir düğüm türünden benzersiz olan addır.
* **Örnek sayısı**: yeni düğüm türünün ilk düğüm sayısı.
* **VM boyutu**: düğümler IÇIN VM SKU 'su. Belirtilmemişse, varsayılan değer *Standard_D2* kullanılır.

> [!NOTE]
> Eklenmekte olan düğüm türü kümedeki ilk veya tek düğüm türüdür, birincil özellik kullanılmalıdır.

```powershell
$resourceGroup = "myResourceGroup"
$clusterName = "mysfcluster"
$nodeTypeName = "nt2"
$vmSize = "Standard_D2_v2"

New-AzServiceFabricManagedNodeType -ResourceGroupName $resourceGroup -ClusterName $clusterName -Name $nodeTypeName -InstanceCount 3 -vmSize $vmSize
```

## <a name="remove-a-node-type-from-a-service-fabric-managed-cluster"></a>Service Fabric yönetilen kümeden düğüm türünü kaldırma

Service Fabric yönetilen bir kümeden düğüm türünü kaldırmak için PowerShell veya CLı kullanmanız gerekir. Bu öğreticide, Azure PowerShell kullanarak bir düğüm türünü kaldıracağız.

> [!NOTE]
> Kümedeki tek birincil düğüm türü ise, birincil düğüm türünü kaldırmak mümkün değildir.  

Düğüm türünü kaldırmak için:

```powershell
$resourceGroup = "myResourceGroup"
$clusterName = "myCluster"
$nodeTypeName = "nt2"

Remove-AzServiceFabricManagedNodeType -ResourceGroupName $resourceGroup -ClusterName $clusterName  -Name $nodeTypeName
```

## <a name="next-steps"></a>Sonraki adımlar

 Bu bölümde, düğüm türlerini ekledik ve sildik. Service Fabric yönetilen bir kümeye uygulama dağıtmayı öğrenmek için bkz.:

> [!div class="nextstepaction"]
> [Service Fabric yönetilen bir kümeye uygulama dağıtma](tutorial-managed-cluster-deploy-app.md)