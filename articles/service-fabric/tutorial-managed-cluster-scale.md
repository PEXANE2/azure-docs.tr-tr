---
title: Service Fabric yönetilen kümenin ölçeğini genişletme (Önizleme)
description: Bu öğreticide, Service Fabric yönetilen bir kümenin düğüm türünü nasıl ölçeklendireceğinizi öğrenin.
ms.topic: tutorial
ms.date: 09/28/2020
ms.openlocfilehash: a6777743932f3cc73db973c4d9935d4b5827700b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91410465"
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

## <a name="prerequisites"></a>Önkoşullar

* Service Fabric yönetilen bir küme (bkz. [*yönetilen küme dağıtma*](tutorial-managed-cluster-deploy.md)).
* [Azure PowerShell 4.7.0](https://docs.microsoft.com/powershell/azure/release-notes-azureps?view=azps-4.7.0&preserve-view=true#azservicefabric) veya üzeri (bkz. [*Install Azure PowerShell*](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-4.7.0&preserve-view=true)).

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
