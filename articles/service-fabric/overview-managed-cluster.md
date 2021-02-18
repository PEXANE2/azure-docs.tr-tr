---
title: Service Fabric yönetilen kümeler (Önizleme)
description: Service Fabric yönetilen kümeler, dağıtım ve küme yönetimini kolaylaştıran Azure Service Fabric kümesi kaynak modelinin bir gelişmidir.
ms.topic: overview
ms.date: 02/15/2021
ms.openlocfilehash: 8b90b73ef5f3202788ac7857875427673ebb562b
ms.sourcegitcommit: 58ff80474cd8b3b30b0e29be78b8bf559ab0caa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100635154"
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

| Özellik | Temel | Standart |
| ------- | ----- | -------- |
| Ağ kaynağı ( [Load Balancer](../load-balancer/skus.md)için SKU, [genel IP](../virtual-network/public-ip-addresses.md)) | Temel | Standart |
| Min düğümü (VM örneği) sayısı | 3 | 5 |
| Düğüm türü başına en fazla düğüm sayısı | 100 | 100 |
| En fazla düğüm türü sayısı | 1 | 20 |
| Düğüm türlerini Ekle/Kaldır | Hayır | Yes |
| Bölge yedekliliği | Hayır | Yes |

## <a name="whats-new-for-service-fabric-managed-clusters"></a>Service Fabric yönetilen kümeler için yenilikler

Service Fabric yönetilen kümeler önizlemesi için en son özellikler şunları destekler:

* [ARM şablonları kullanarak uygulama dağıtma](how-to-managed-cluster-app-deployment-template.md)
* [Otomatik işletim sistemi yükseltmeleri](how-to-managed-cluster-configuration.md#enable-automatic-os-image-upgrades)
* [Disk şifrelemesi](how-to-enable-managed-cluster-disk-encryption.md)
* [NSG kuralları uygulanıyor](how-to-managed-cluster-configuration.md#apply-nsg-rules)

Yaklaşan sürümlerde eklenecek özellikler şunlardır:

* Visual Studio kullanarak uygulama dağıtma
* Yönetilen kimlikler desteği
* Kullanılabilirlik Alanları
* Ters proxy
* Otomatik ölçeklendirme

## <a name="next-steps"></a>Sonraki adımlar

Service Fabric yönetilen kümeler ile çalışmaya başlamak için hızlı başlangıcı deneyin:

> [!div class="nextstepaction"]
> [Service Fabric yönetilen kümesi oluşturma (Önizleme)](quickstart-managed-cluster-template.md)


[sf-composition]: ./media/overview-managed-cluster/sfrp-composition-resource.png
[sf-encapsulation]: ./media/overview-managed-cluster/sfrp-encapsulated-resource.png