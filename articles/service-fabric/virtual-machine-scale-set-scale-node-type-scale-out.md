---
title: Azure Hizmet Kumaşı kümesine düğüm türü ekleme
description: Sanal Makine Ölçeği Kümesi ekleyerek Service Fabric kümesini nasıl ölçeklendireceklerini öğrenin.
ms.topic: article
ms.date: 02/13/2019
ms.openlocfilehash: 1e7bae89561d43d717eb4d15e95183761b077443
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75463980"
---
# <a name="scale-a-service-fabric-cluster-out-by-adding-a-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi ekleyerek Service Fabric kümesini ölçeklendirin
Bu makalede, varolan bir kümeye yeni bir düğüm türü ekleyerek Azure Hizmet Kumaşı kümesinin nasıl ölçeklendirileni açıklanmaktadır. Service Fabric kümesi, mikro hizmetlerinizin dağıtıldığı ve yönetildiği ağa bağlı sanal veya fiziksel makineler kümesidir. Kümenin bir parçası olan bir makine ye veya VM'ye düğüm denir. Sanal makine ölçek kümeleri, sanal makine koleksiyonunu küme olarak dağıtmak ve yönetmek için kullandığınız bir Azure bilgi işlem kaynağıdır. Azure kümesinde tanımlanan her düğüm türü [ayrı bir ölçek kümesi olarak ayarlanır.](service-fabric-cluster-nodetypes.md) Her düğüm türü daha sonra ayrı ayrı yönetilebilir. Hizmet Kumaşı kümesi oluşturduktan sonra, varolan bir kümeye yeni bir düğüm türü (sanal makine ölçeği kümesi) ekleyerek kümeyi yatay olarak ölçeklendirebilirsiniz.  Kümede iş yükleri çalışıyor olsa bile, kümeyi istediğiniz zaman ölçeklendirebilirsiniz.  Küme ölçeklendikçe, uygulamalarınız da otomatik olarak ölçeklenir.

## <a name="add-an-additional-scale-set-to-an-existing-cluster"></a>Varolan bir kümeye ek ölçek kümesi ekleme
Varolan bir kümeye yeni bir düğüm türü (sanal makine ölçeği kümesi tarafından desteklenen) eklemek, aynı DüğümTypeRef'i kullanmamanız dışında [birincil düğüm türünün yükseltilmesine](service-fabric-scale-up-node-type.md)benzer; açıkçası herhangi bir aktif olarak kullanılan sanal makine ölçek kümeleri devre dışı bırakma olmaz ve birincil düğüm türünü güncelleştirmezseniz küme kullanılabilirliğini kaybetmezsiniz. 

NodeTypeRef özelliği sanal makine ölçeği içinde ilan edilir Hizmet Kumaş uzantısı özellikleri ayarlayın:
```json
<snip>
"publisher": "Microsoft.Azure.ServiceFabric",
     "settings": {
     "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
     "nodeTypeRef": "[parameters('vmNodeType2Name')]",
     "dataPath": "D:\\\\SvcFab",
     "durabilityLevel": "Silver",
<snip>
```

Ayrıca, Service Fabric küme kaynağınıza bu yeni düğüm türünü eklemeniz gerekir:

```json
<snip>
"nodeTypes": [
      {
      "name": "[parameters('vmNodeType2Name')]",
      "applicationPorts": {
                "endPort": "[parameters('nt2applicationEndPort')]",
                "startPort": "[parameters('nt2applicationStartPort')]"
      },
      "clientConnectionEndpointPort": "[parameters('nt2fabricTcpGatewayPort')]",
      "durabilityLevel": "Silver",
       "ephemeralPorts": {
                "endPort": "[parameters('nt2ephemeralEndPort')]",
                "startPort": "[parameters('nt2ephemeralStartPort')]"
      },
      "httpGatewayEndpointPort": "[parameters('nt2fabricHttpGatewayPort')]",
      "isPrimary": false,
      "vmInstanceCount": "[parameters('nt2InstanceCount')]"
},
<snip>
```

## <a name="next-steps"></a>Sonraki adımlar
* Birincil düğüm türünü nasıl [ölçeklendireceklerini](service-fabric-scale-up-node-type.md) öğrenin
* Uygulama [ölçeklenebilirliği](service-fabric-concepts-scalability.md)hakkında bilgi edinin.
* [Azure kümesini içinde veya dışında ölçeklendirin.](service-fabric-tutorial-scale-cluster.md)
* Akıcı Azure işlem SDK'sını kullanarak [bir Azure kümesini programlı olarak](service-fabric-cluster-programmatic-scaling.md) ölçeklendirin.
* [Bağımsız bir kümeyi içinde veya dışında ölçeklendirin.](service-fabric-cluster-windows-server-add-remove-nodes.md)

