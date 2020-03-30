---
title: Hizmet Kumaş Azure Kaynak Yöneticisi dağıtım korkulukları
description: Bu makalede, Bir Hizmet Dokusu kümesini Azure Kaynak Yöneticisi aracılığıyla dağıtırken yapılan yaygın hatalara ve bunlardan nasıl kaçınılacaklarına genel bir bakış sunulmaktadır.
services: service-fabric
documentationcenter: .net
author: peterpogorski
ms.topic: conceptual
ms.date: 02/13/2020
ms.author: pepogors
ms.openlocfilehash: a61b0cf30ca46eb77837eb09d6a9a0b6f30e89a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77368569"
---
# <a name="service-fabric-guardrails"></a>Servis Kumaş korkulukları 
Bir Hizmet Kumaşı kümesi dağıtılırken, zayıflayıcılar devreye alınır ve bu da geçersiz küme yapılandırması durumunda Azure Kaynak Yöneticisi dağıtımında başarısız olur. Aşağıdaki bölümler, sık karşılaşılan küme yapılandırma sorunlarına ve bu sorunları azaltmak için gereken adımlara genel bir bakış sağlar. 

## <a name="durability-mismatch"></a>Dayanıklılık uyumsuzluğu
### <a name="overview"></a>Genel Bakış
Hizmet Dokusu düğümü türü için dayanıklılık değeri, Azure Kaynak Yöneticisi şablonunun iki farklı bölümünde tanımlanır. Sanal Makine Ölçeği, Sanal Makine Ölçeği Kümesi kaynağının uzantı bölümü ve Hizmet Kumaşı küme kaynağının Düğüm Türü bölümü. Bu bölümlerdeki dayanıklılık değerinin eşleşmesi bir gereksinimdir, aksi takdirde kaynak dağıtımı başarısız olur.

Aşağıdaki bölümde, Sanal Makine Ölçeği Kümesi uzantı dayanıklılık ayarı ile Servis Kumaş Düğümü Tipi dayanıklılık ayarı arasındaki dayanıklılık uyuşmazlığı örneğini içerir:  

**Sanal Makine Ölçeği Ayarlı dayanıklılık ayarı**
```json 
{
  "extensions": [
    {
      "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
      "properties": {
        "type": "ServiceFabricNode",
        "publisher": "Microsoft.Azure.ServiceFabric",
        "settings": {
          "durabilityLevel": "Silver",
        }
      }
    }
  ]
}
```

**Servis Kumaş düğümü tipi dayanıklılık ayarı** 
```json
{
  "nodeTypes": [
    {
      "name": "[variables('vmNodeType0Name')]",
      "durabilityLevel": "Bronze",
      "isPrimary": true,
      "vmInstanceCount": "[parameters('nt0InstanceCount')]"
    }
  ]
}
```

### <a name="error-messages"></a>Hata iletileri
* Sanal Makine Ölçeği Ayardayanıklılık uyuşmazlığı geçerli Servis Kumaş Düğümü Tipi dayanıklılık düzeyiyle eşleşmiyor
* Sanal Makine Ölçeği Ayar dayanıklılığı hedef Servis Kumaş Düğümü Tipi dayanıklılık düzeyine uymuyor
* Sanal Makine Ölçeği Ayarlı dayanıklılık, geçerli Servis Kumaş dayanıklılık düzeyine veya hedef Servis Kumaş Düğümü Tipi dayanıklılık düzeyine uyuyor 

### <a name="mitigation"></a>Risk azaltma
Yukarıdaki hata iletilerinden herhangi biri tarafından belirtilen dayanıklılık uyuşmazlığını düzeltmek için:
1. Değerlerin eşleşdiğinden emin olmak için Azure Kaynak Yöneticisi şablonunun Sanal Makine Ölçeği Kümesi uzantısı veya Hizmet Kumaş Düğümü Türü bölümündeki dayanıklılık düzeyini güncelleştirin.
2. Azure Kaynak Yöneticisi şablonu güncelleştirilmiş değerlerle yeniden dağıtın.


## <a name="seed-node-deletion"></a>Tohum düğümü silme 
### <a name="overview"></a>Genel Bakış
Hizmet Kumaşı kümesi, kümenin birincil düğüm türünde çalışan sistem hizmetlerinin yineleme sayısını belirlemek için kullanılan bir [güvenilirlik katmanı](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-reliability-characteristics-of-the-cluster) özelliğine sahiptir. Gerekli yineleme lerin sayısı, kümenin birincil düğüm türünde tutulması gereken en az düğüm sayısını belirler. Birincil düğüm türündeki düğüm sayısı güvenilirlik katmanı için gereken minimumun altına düşerse, küme kararsız hale gelir.  

### <a name="error-messages"></a>Hata iletileri 
Tohum düğümü kaldırma işlemi algılandı ve reddedilecek. 
* Bu işlem, kümede kalan yalnızca {0} potansiyel tohum düğümlerinin {1} kalmasına neden olurken, en az olarak gereklidir.
* Tohum {0} düğümlerinin {1} çıkarılması, kümenin tohum düğümü yeter sayısının azalması nedeniyle düşmesine neden olur. Bir seferde çıkarılabilen en fazla tohum {2}düğümü sayısı.
 
### <a name="mitigation"></a>Risk azaltma 
Birincil düğüm türünizin kümenizde belirtilen güvenilirlik için yeterli Sanal Makineye sahip olduğundan emin olun. Sanal Makine Ölçeği Kümesi'ni verilen güvenilirlik katmanı için minimum düğüm sayısının altına getirecekse Sanal Makine'yi kaldıramazsınız.
* Güvenilirlik katmanı doğru bir şekilde belirtilirse, güvenilirlik katmanı için gerektiğinde birincil düğüm türünde yeterli düğüme sahip olduğundan emin olun. 
* Güvenilirlik katmanı yanlışsa, Sanal Makine Ölçeği Kümesi işlemlerini başlatmadan önce güvenilirlik düzeyini düşürmek için Hizmet Kumaşı kaynağında bir değişiklik başlatın ve tamamlanmasını bekleyin.
* Güvenilirlik katmanı Bronz ise, kümenizi incelikle küçültmek için lütfen aşağıdaki [adımları](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-scale-up-down#manually-remove-vms-from-a-node-typevirtual-machine-scale-set) izleyin.

## <a name="next-steps"></a>Sonraki adımlar
* Windows Server çalıştıran VM'lerde veya bilgisayarlarda küme [oluşturma: Windows Server için Hizmet Kumaşı küme oluşturma](service-fabric-cluster-creation-for-windows-server.md)
* Linux çalıştıran VM'lerde veya bilgisayarlarda küme oluşturma: [Linux kümesi oluşturma](service-fabric-cluster-creation-via-portal.md)
* Sorun Giderme Hizmet Kumaşı: [Sorun giderme kılavuzları](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides)
