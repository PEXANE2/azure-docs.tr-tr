---
title: Azure Cosmos DB etcd API 'sine giriş
description: Bu makalede, Azure Cosmos DB ' de etcd API 'sinin genel bakış ve temel avantajları sunulmaktadır
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2020
ms.author: dech
ms.reviewer: sngun
ms.openlocfilehash: 208f0d73b4c8bb11adc9c73796ebc6a622f5ed50
ms.sourcegitcommit: 23604d54077318f34062099ed1128d447989eea8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2020
ms.locfileid: "85118177"
---
# <a name="introduction-to-the-azure-cosmos-db-etcd-api-preview"></a>Azure Cosmos DB etcd API 'sine giriş (Önizleme)

Azure Cosmos DB, Microsoft'un görev açısından kritik uygulamalar için genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Kullanıma hazır küresel dağıtım, verimlilik ve depolama için esnek ölçeklendirme, 99. yüzdede tek basamaklı milisaniyelik gecikme süreleri ve hepsi sektörde önde gelen SLA 'lar tarafından desteklenen garantili yüksek kullanılabilirlik sağlar.

[Etcd](https://github.com/etcd-io/etcd) , dağıtılmış bir anahtar/değer deposudur. [Kubernetes](https://kubernetes.io/)'de, Kubernetes kümelerinin durumunu ve yapılandırmasını depolamak için etcd kullanılır. Bir Kubernetes kümesinin kullanılabilirlik, güvenilirlik ve performansının yanı sıra tüm küme durumu, ölçeklenebilirlik, esneklik kullanılabilirliği ve performansı açısından önemlidir.

Azure Cosmos DB ' deki etcd API 'SI, Azure Kubernetes için arka uç deposu olarak Azure Cosmos DB kullanmanıza olanak tanır. Azure Cosmos DB ' deki etcd API 'SI Şu anda önizlemededir. Azure Cosmos DB, etcd tel protokolünü uygular. Azure Cosmos DB ' deki etcd API 'siyle, geliştiriciler otomatik olarak güvenilir, [kullanılabilir](high-availability.md)ve [Global olarak dağıtılmış](distribute-data-globally.md) Kubernetes sağlar. Bu API, geliştiricilerin tamamen yönetilen bir bulut Yerel PaaS hizmetinde Kubernetes durum yönetimini ölçeklendirmesine olanak tanır. 

> [!NOTE]
> Azure Cosmos DB diğer API 'lerden farklı olarak, Azure portal, CLı veya SDK 'Lar aracılığıyla bir etcd API hesabı sağlayamazsınız. Yalnızca Kaynak Yöneticisi şablonunu dağıtarak bir etcd API hesabı sağlayabilirsiniz; ayrıntılı adımlar için bkz. [Azure Kubernetes 'i Azure Cosmos DB makalesiyle sağlama](bootstrap-kubernetes-cluster.md) . Azure Cosmos DB etcd API 'SI Şu anda sınırlı önizlemededir. [Ön izleme için](https://aka.ms/cosmosetcdapi-signup)Kaydolma formunu doldurarak kayıt yapabilirsiniz.

## <a name="wire-level-compatibility"></a>Tel düzeyi uyumluluğu

Azure Cosmos DB, etcd sürüm 3 ' ün kablo protokolünü uygular ve [ana düğümün](https://kubernetes.io/docs/concepts/overview/components/) API sunucularının, yerel olarak yüklenmiş bir etcd ortamında olduğu gibi Azure Cosmos DB kullanmasına izin verir. Etcd API 'SI, TLS karşılıklı kimlik doğrulamasını destekler. 

Aşağıdaki diyagramda bir Kubernetes kümesinin bileşenleri gösterilmektedir. Küme yöneticisinde, API sunucusu yerel olarak yüklü etcd yerine Azure Cosmos DB etcd API 'sini kullanır. 

:::image type="content" source="./media/etcd-api-introduction/etcd-api-wire-protocol.png" alt-text="Etcd tel protokolünü uygulama Azure Cosmos DB" border="false":::

## <a name="key-benefits"></a>Önemli avantajlar

### <a name="no-etcd-operations-management"></a>Etcd işlem yönetimi yok

Tam olarak yönetilen bir yerel bulut hizmeti olan Azure Cosmos DB, Kubernetes geliştiricilerinin, etcd 'yi ayarlama ve yönetme ihtiyacını ortadan kaldırır. Azure Cosmos DB ' deki etcd API 'SI ölçeklendirilebilir, yüksek oranda kullanılabilir, hataya dayanıklıdır ve yüksek performans sunar. Birden çok düğüm arasında çoğaltmayı ayarlama, sıralı güncelleştirmeler gerçekleştirme, güvenlik düzeltme ekleri ve etcd durumunun izlenmesi, Azure Cosmos DB tarafından işlenir.

### <a name="global-distribution--high-availability"></a>Küresel dağıtım & yüksek kullanılabilirlik 

Azure Cosmos DB, etcd API 'sini kullanarak, tek bir bölgedeki veri okuma ve yazma işlemleri için% 99,99 kullanılabilirliği ve birden çok bölgede% 99,999 kullanılabilirliği garanti eder. 

### <a name="elastic-scalability"></a>Elastik ölçeklenebilirlik

Azure Cosmos DB, farklı bölgelerde okuma ve yazma istekleri için esnek ölçeklenebilirlik sunar.
Kubernetes kümesi büyüdükçe, Azure Cosmos DB esnek ' deki etcd API hesabı herhangi bir kesinti olmadan ölçeklenir. Kubernetes ana düğümleri yerine etcd verilerini Azure Cosmos DB depolama, daha esnek ana düğüm Ölçeklendirmesi de sağlar. 

### <a name="security--enterprise-readiness"></a>Güvenlik & kurumsal hazırlık

Etcd verileri Azure Cosmos DB depolandığında, Kubernetes geliştiricileri, [yerleşik şifrelemeyi Rest](database-encryption-at-rest.md), sertifikalar, [Uyumluluk](compliance.md)ve Azure Cosmos DB tarafından desteklenen [yedekleme ve geri yükleme özelliklerine](online-backup-and-restore.md) otomatik olarak alır. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB ile Azure Kubernetes 'i kullanma](bootstrap-kubernetes-cluster.md)
* [Azure Cosmos DB temel avantajları](introduction.md)
* [AKS altyapısı hızlı başlangıç kılavuzu](https://github.com/Azure/aks-engine/blob/master/docs/tutorials/quickstart.md)