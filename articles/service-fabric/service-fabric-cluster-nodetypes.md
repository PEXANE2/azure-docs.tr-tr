---
title: Düğüm türleri ve sanal makine ölçek kümeleri
description: Azure Hizmet Kumaş düğümü türlerinin sanal makine ölçek kümeleri ile nasıl ilişkili olduğunu ve ölçek kümesi örneğine veya küme düğümüne uzaktan nasıl bağlanış bekleyeceğinizi öğrenin.
ms.topic: conceptual
ms.date: 03/23/2018
ms.author: pepogors
ms.custom: sfrev
ms.openlocfilehash: 37d4c27d3033545c523cefc2f317073af531f095
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78199725"
---
# <a name="azure-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Azure Hizmet Kumaş düğümü türleri ve sanal makine ölçek setleri

[Sanal makine ölçek kümeleri](/azure/virtual-machine-scale-sets) bir Azure bilgi işlem kaynağıdır. Sanal makineler koleksiyonunu bir küme olarak dağıtmak ve yönetmek için ölçek kümelerini kullanabilirsiniz. Azure Hizmet Kumaşı kümesinde tanımladığınız her düğüm türü tam olarak bir ölçek kümesi ayarlar: birden çok düğüm türü aynı ölçek kümesi tarafından desteklenemez ve bir düğüm türü (çoğu durumda) birden çok ölçek kümeleri tarafından desteklenmemelidir. Bunun bir istisnası, yinelemeler orijinalden yükseltilmiş ölçek kümesine geçirilirken geçici olarak `nodeTypeRef` aynı değere sahip iki ölçek kümeniz olduğunda, bir düğüm türünü [dikey ölçeklendirme](service-fabric-best-practices-capacity-scaling.md#vertical-scaling-considerations) nin nadir bir durumudur.

Service Fabric çalışma süresi, *Microsoft.Azure.ServiceFabric* Virtual Machine uzantısı tarafından belirlenen ölçekteher sanal makineye yüklenir. Bağımsız olarak her düğüm türünü yukarı veya aşağı ölçeklendirebilir, her küme düğümünde çalışan OS SKU'yu değiştirebilir, farklı bağlantı noktaları kümelerini açabilir ve farklı kapasite ölçümleri kullanabilirsiniz.

Aşağıdaki şekilde *FrontEnd* ve *BackEnd*adlı iki düğüm türü olan bir küme gösterir. Her düğüm türübeş düğüm vardır.

![İki düğüm türü olan bir küme][NodeTypes]

## <a name="map-virtual-machine-scale-set-instances-to-nodes"></a>Sanal makine ölçeğini düğümlere ayarla

Önceki şekilde gösterildiği gibi, ölçek kümesi örnekleri örneğinde başlar 0 ve sonra 1 artar. Numaralandırma düğüm adlarında yansıtılır. Örneğin, düğüm BackEnd_0 BackEnd ölçek kümesinin örnek 0'dır. Bu özel ölçek kümesi, BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 ve BackEnd_4 adlı beş örneği vardır.

Bir ölçek kümesini ölçeklendirdiğinizde, yeni bir örnek oluşturulur. Yeni ölçek kümesi örnek adı genellikle ölçek kümesi adı artı bir sonraki örnek numarasıdır. Bizim örneğimizde, BackEnd_5.

## <a name="map-scale-set-load-balancers-to-node-types-and-scale-sets"></a>Harita ölçeği, yük dengeleyicilerini düğüm türlerine ve ölçek kümelerine göre ayarla

Kümenizi Azure portalında dağıttıysanız veya örnek Azure Kaynak Yöneticisi şablonunu kullandıysanız, kaynak grubu altındaki tüm kaynaklar listelenir. Her ölçek kümesi veya düğüm türü için yük dengeleyicilerini görebilirsiniz. Yük dengeleyici adı aşağıdaki biçimi kullanır: **&lt;LB-&gt;düğüm türü adı.** Aşağıdaki şekilde gösterildiği gibi LB-sfcluster4doc-0 örnek:

![Kaynaklar][Resources]

## <a name="service-fabric-virtual-machine-extension"></a>Servis Kumaş Sanal Makine Uzantısı

Service Fabric Virtual Machine Extension, Service Fabric'i Azure Sanal Makinelere takmak ve Düğüm Güvenliğini yapılandırmak için kullanılır.

Aşağıdaki Hizmet Kumaş Sanal Makine uzantısı bir snippet olduğunu:

```json
"extensions": [
  {
    "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
    "properties": {
      "type": "ServiceFabricLinuxNode",
      "autoUpgradeMinorVersion": true,
      "protectedSettings": {
        "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
       },
       "publisher": "Microsoft.Azure.ServiceFabric",
       "settings": {
         "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
         "nodeTypeRef": "[variables('vmNodeType0Name')]",
         "durabilityLevel": "Silver",
         "enableParallelJobs": true,
         "nicPrefixOverride": "[variables('subnet0Prefix')]",
         "dataPath": "D:\\\\SvcFab",
         "certificate": {
           "commonNames": [
             "[parameters('certificateCommonName')]"
           ],
           "x509StoreName": "[parameters('certificateStoreValue')]"
         }
       },
       "typeHandlerVersion": "1.1"
     }
   },
```

Özellik açıklamaları şunlardır:

| **Adı** | **İzin Verilen Değerler** | **Rehberlik veya Kısa Açıklama** |
| --- | --- | --- | --- |
| ad | string | Uzantı için benzersiz ad |
| type | "ServiceFabricLinuxNode" veya "ServiceFabricWindowsNode" | OS Hizmet Kumaşı'nın |
| autoUpgradeMinorVersion | true veya false | SF Runtime Minor Sürümlerinin Otomatik Yükseltmesini Etkinleştir |
| yayımcı | Microsoft.Azure.ServiceFabric | Service Fabric uzantıcısının adı |
| kümeEndpont | string | URI:PORT'dan Yönetim bitiş noktasına |
| düğümTypeRef | string | düğümTürü adı |
| dayanıklılıkSeviye | bronz, gümüş, altın, platin | Değişmez Azure Altyapısının duraklatılması için izin verilen süre |
| parallelJobs'ı etkinleştirme | true veya false | VM'yi kaldırma ve VM'yi paralel olarak aynı ölçekte yeniden başlatma gibi İşlem Paralel İşler'i etkinleştirme |
| nicPrefixOverride | string | "10.0.0.0/24" gibi alt ağ öneki |
| commonNames | dize[] | Yüklü küme sertifikalarının ortak adları |
| x509StoreName | string | Yüklü küme sertifikasının bulunduğu Mağazanın Adı |
| typeHandlerVersion | 1.1 | Uzantı sürümü. 1.0 klasik uzantısı sürümü 1.1 yükseltmek için tavsiye edilir |
| dataPath | string | Service Fabric sistem hizmetleri ve uygulama verileri için durumu kaydetmek için kullanılan sürücüye giden yol.

## <a name="next-steps"></a>Sonraki adımlar

* ["Herhangi bir yerde dağıt" özelliğine genel bakışı ve Azure tarafından yönetilen kümelerle karşılaştırmaya](service-fabric-deploy-anywhere.md)bakın.
* Küme [güvenliği](service-fabric-cluster-security.md)hakkında bilgi edinin.
* Belirli bir ölçek kümesi örneğine [uzaktan bağlanma](service-fabric-cluster-remote-connect-to-azure-cluster-node.md)
* Dağıtımdan sonra küme [VM'lerde RDP bağlantı noktası aralığı değerlerini güncelleştirme](./scripts/service-fabric-powershell-change-rdp-port-range.md)
* Küme VM'leri için [yönetici kullanıcı adını ve parolasını değiştirme](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md)

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
