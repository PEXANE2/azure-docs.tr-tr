---
title: Service Fabric yönetilen kümeler (Önizleme)
description: Service Fabric yönetilen kümeler, dağıtım ve küme yönetimini kolaylaştıran Azure Service Fabric kümesi kaynak modelinin bir gelişmidir.
ms.topic: overview
ms.date: 09/28/2020
ms.openlocfilehash: 3d26a92126491662d5c51b3c4e8900ffa547f830
ms.sourcegitcommit: b48e8a62a63a6ea99812e0a2279b83102e082b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2020
ms.locfileid: "91410512"
---
# <a name="service-fabric-managed-clusters-preview"></a>Service Fabric yönetilen kümeler (Önizleme)

Service Fabric yönetilen kümeler, dağıtım ve küme yönetimi deneyiminizi kolaylaştıran Azure Service Fabric kümesi kaynak modelinin bir gelişmidir.

Geleneksel Service Fabric kümelerine yönelik Azure kaynak modeli (ARM) şablonu, kümenin ve hizmetlerinizin düzgün çalışması için bir dizi destekleyici kaynağın yanı sıra, tümünün "bağlı ve kümenin yaşam döngüsü boyunca" olması gereken bir küme kaynağı tanımlamanızı gerektirir. Buna karşılık, Service Fabric yönetilen kümeler için kapsülleme modeli, tek bir *Service Fabric yönetilen küme* kaynağı içerir. Kümenin tüm temel kaynakları, sizin adınıza Azure tarafından soyutlanarak yönetilir.

**Service Fabric geleneksel küme modeli** 
 ![ Service Fabric geleneksel küme modeli][sf-composition]

**Service Fabric yönetilen küme modeli** 
 ![ Service Fabric Encapsulated küme modeli][sf-encapsulation]

Boyut ve karmaşıklık açısından, Service Fabric yönetilen bir küme için ARM şablonu, 100 satırlık bir şablondur ve tipik bir Service Fabric kümesini tanımlamak için bazı 1000 çizgilere karşılık olur:

| Service Fabric kaynakları | Yönetilen küme kaynaklarını Service Fabric |
|----------|-----------|
| Service Fabric kümesi | Service Fabric yönetilen küme |
| Sanal makine ölçek kümesi | |
| Yük dengeleyici | |
| Genel IP adresi | |
| Depolama hesapları | |
| Sanal ağ | |

Service Fabric yönetilen kümeler geleneksel kümeler üzerinde birçok avantaj sağlar:

**Basitleştirilmiş küme dağıtımı ve yönetimi**
- Tek bir Azure kaynağı dağıtma ve yönetme
- Sertifika yönetimi ve oto döndürme
- Basitleştirilmiş ölçeklendirme işlemleri

**İşlem hatalarını önleme**
- Temel alınan kaynaklarla yapılandırma uyuşmazlıklarını engelle
- Güvenli olmayan işlemleri engelle (çekirdek düğümünü silme gibi)

**Varsayılan olarak en iyi uygulamalar**
- Basitleştirilmiş güvenilirlik ve dayanıklılık ayarları

Service Fabric yönetilen kümeler için, küme için gereken temel kaynakların maliyetinin ötesinde ek bir ücret yoktur.

## <a name="service-fabric-managed-cluster-skus"></a>Yönetilen küme SKU 'Larını Service Fabric

Service Fabric yönetilen kümeler hem temel hem de standart SKU 'Larda kullanılabilir.

| Öne çıkan özelliği | Temel | Standart |
| ------- | ----- | -------- |
| Ağ kaynağı ( [Load Balancer](../load-balancer/skus.md)için SKU, [genel IP](../virtual-network/public-ip-addresses.md)) | Temel | Standart |
| Min düğümü (VM örneği) sayısı | 3 | 5 |
| Düğüm türü başına en fazla düğüm sayısı | 100 | 100 |
| En fazla düğüm türü sayısı | 1 | 20 |
| Düğüm türlerini Ekle/Kaldır | Hayır | Yes |
| Bölge yedekliliği | Hayır | Yes |

## <a name="service-fabric-managed-cluster-feature-roadmap"></a>Service Fabric yönetilen küme özelliği yol haritası
Bu, Service Fabric yönetilen kümelerin erken önizleme sürümü olduğu için farkında olacak bazı boşluklar vardır. Bu özellikler gelecek sürümlerde kullanılabilir olacaktır. 

* Uygulamaları doğrudan Visual Studio 'dan kümelere yayımlama
* Yönetilen kimlikler 
* ARM uygulama dağıtımları 
* Kullanılabilirlik Alanları 
* Ters proxy 
* Otomatik ölçeklendirme 
* NSG kuralları güncelleştiriliyor 
* Otomatik işletim sistemi yükseltmeleri

## <a name="next-steps"></a>Sonraki adımlar

Service Fabric yönetilen kümeler ile çalışmaya başlamak için hızlı başlangıcı deneyin:

> [!div class="nextstepaction"]
> [Service Fabric yönetilen kümesi oluşturma (Önizleme)](quickstart-managed-cluster-template.md)


[sf-composition]: ./media/overview-managed-cluster/sfrp-composition-resource.png
[sf-encapsulation]: ./media/overview-managed-cluster/sfrp-encapsulated-resource.png