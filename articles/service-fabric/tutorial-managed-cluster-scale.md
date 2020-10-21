---
title: Service Fabric yönetilen kümenin ölçeğini genişletme (Önizleme)
description: Bu öğreticide, Service Fabric yönetilen bir kümenin düğüm türünü nasıl ölçeklendireceğinizi öğrenin.
ms.topic: tutorial
ms.date: 09/28/2020
ms.openlocfilehash: 01b299744d462496296884211eff08b7a9c64687
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92316133"
---
# <a name="tutorial-scale-out-a-service-fabric-managed-cluster-preview"></a>Öğretici: Service Fabric yönetilen bir kümeyi genişletme (Önizleme)

Bu öğretici serisinde şunları tartışacağız:

> [!div class="checklist"]
> * [Service Fabric yönetilen bir küme nasıl dağıtılır.](tutorial-managed-cluster-deploy.md)
> * Service Fabric yönetilen bir kümeyi daraltma
> * [Service Fabric yönetilen bir kümede düğüm türlerini ekleme ve kaldırma](tutorial-managed-cluster-add-remove-node-type.md)
> * [Service Fabric yönetilen bir kümeye uygulama dağıtma](tutorial-managed-cluster-deploy-app.md)

Serinin bu bölümü aşağıdaki konuları içerir:

> [!div class="checklist"]
> * Service Fabric yönetilen bir küme düğümünü ölçeklendirme

## <a name="prerequisites"></a>Ön koşullar

* Service Fabric yönetilen bir küme (bkz. [*yönetilen küme dağıtma*](tutorial-managed-cluster-deploy.md)).
* [Azure PowerShell 4.7.0](/powershell/azure/release-notes-azureps?preserve-view=true&view=azps-4.7.0#azservicefabric) veya üzeri (bkz. [*Install Azure PowerShell*](/powershell/azure/install-az-ps?preserve-view=true&view=azps-4.7.0)).

## <a name="scale-a-service-fabric-managed-cluster"></a>Service Fabric yönetilen bir kümeyi ölçeklendirme
Örnek sayısını, ölçeklendirmek istediğiniz düğüm türündeki düğümlerin sayısını artırmak veya azaltmak için değiştirin. Düğüm türü adlarını, küme dağıtımınızdaki veya Service Fabric Explorer Azure Resource Manager şablonunda (ARM şablonunda) bulabilirsiniz.  

> [!NOTE]
> Düğüm türü birincili ise, temel bir SKU kümesi için 3 düğüm ve standart SKU kümesi için 5 düğüm altına gidemeyeceksiniz.

```powershell
$resourceGroup = "myResourceGroup"
$clusterName = "mysfcluster"
$nodeTypeName = "FE"
$instanceCount = "7"

Set-AzServiceFabricManagedNodeType -ResourceGroupName $resourceGroup -ClusterName $clusterName -name $nodeTypeName -InstanceCount $instanceCount -Verbose
```

Küme otomatik olarak yükseltmeye başlayacaktır ve birkaç dakika sonra ek düğümleri görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

Bu adımda Service Fabric yönetilen bir kümede düğüm türünü ölçeklendirdik. Düğüm türlerini ekleme ve kaldırma hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Service Fabric yönetilen bir kümeye düğüm türleri ekleme ve kaldırma](tutorial-managed-cluster-add-remove-node-type.md)