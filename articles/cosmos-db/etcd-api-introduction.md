---
title: Azure Cosmos DB etcd API'ye giriş
description: Bu makale, Azure Cosmos DB'deki ETCD API'sinin genel görünümünü ve önemli avantajlarını sağlar
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: dech
ms.reviewer: sngun
ms.openlocfilehash: acd87fac5ec2edc40d27d98f073e13c0acae8d8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79498599"
---
# <a name="introduction-to-the-azure-cosmos-db-etcd-api-preview"></a>Azure Cosmos DB etcd API'ye giriş (önizleme)

Azure Cosmos DB, Microsoft'un görev açısından kritik uygulamalar için genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Anahtar teslimküresel dağıtım, iş ve depolamanın elastik ölçekletilmesi, yüzde 99'da tek haneli milisaniye gecikmeleri ve sektör lideri SLA'lar tarafından desteklenen yüksek kullanılabilirlik garantisi sunar.

[Etcd](https://github.com/etcd-io/etcd) dağıtılmış bir anahtar/değer deposudur. [Kubernetes,](https://kubernetes.io/)etcd devlet ve Kubernetes kümelerinin yapılandırma saklamak için kullanılır. ETCD'nin kullanılabilirliğini, güvenilirliğini ve performansını sağlamak, bir Kubernetes kümesinin genel küme sağlığı, ölçeklenebilirlik, elastikiyet kullanılabilirliği ve performansı için çok önemlidir. 

Azure Cosmos DB'deki ETCD API'sı Azure Cosmos DB'yi [Azure Kubernetes'in](../aks/index.yml)arka uç mağazası olarak kullanmanıza olanak tanır. Azure Cosmos DB'deki etcd API şu anda önizlemede. Azure Cosmos DB, etcd tel protokolünü uygular. Azure Cosmos DB'deki ETCD API ile geliştiriciler otomatik olarak son derece güvenilir, [kullanılabilir,](high-availability.md) [küresel olarak dağıtılmış](distribute-data-globally.md) Kubernetes alırsınız. Bu API, geliştiricilerin Kubernetes eyalet yönetimini tam olarak yönetilen bir bulut yerel PaaS hizmetinde ölçeklendirmesine olanak tanır. 

> [!NOTE]
> Azure Cosmos DB'deki diğer API'lerin aksine, Azure portalı, CLI veya SDK'lar aracılığıyla etcd API hesabı sağlayamazsınız. Yalnızca Kaynak Yöneticisi şablonunu dağıtarak bir ETCD API hesabı sağlayabilirsiniz; ayrıntılı adımlar için Azure [Cosmos DB makalesiyle Azure Kubernetes nasıl sağlanmaktadır'](bootstrap-kubernetes-cluster.md) a bakın. Azure Cosmos DB etcd API şu anda sınırlı önizlemededir. Kayıt formunu doldurarak [önizleme ye kaydolabilirsiniz.](https://aka.ms/cosmosetcdapi-signup)

## <a name="wire-level-compatibility"></a>Tel seviyesi uyumluluğu

Azure Cosmos DB, etcd sürüm 3'ün tel protokolünü uygular ve [ana düğümün](https://kubernetes.io/docs/concepts/overview/components/) API sunucularının azure cosmos DB'sini yerel olarak yüklenmiş bir ortamda olduğu gibi kullanmasına olanak tanır. ETCD API, TLS karşılıklı kimlik doğrulamasını destekler. 

Aşağıdaki diyagramda bir Kubernetes kümesinin bileşenleri gösterilmektedir. Küme üstalanında, API Sunucusu yerel olarak yüklenmiş etcd yerine Azure Cosmos DB etcd API kullanır. 

![Azure Cosmos DB etcd tel protokolü uygulama](./media/etcd-api-introduction/etcd-api-wire-protocol.png)

## <a name="key-benefits"></a>Önemli avantajlar

### <a name="no-etcd-operations-management"></a>Etcd operasyon yönetimi yok

Azure Cosmos DB, tam olarak yönetilen bir yerel bulut hizmeti olarak Kubernetes geliştiricilerinin etcd'yi kurma ve yönetme ihtiyacını ortadan kaldırır. Azure Cosmos DB'deki ETCD API ölçeklenebilir, yüksek kullanılabilir, hataya dayanıklıdır ve yüksek performans sunar. Birden çok düğümde çoğaltma ayarlama, yuvarlanma güncelleştirmeleri, güvenlik düzeltme yamaları gerçekleştirme ve etcd durumunuizleme yükü Azure Cosmos DB tarafından gerçekleştirilir.

### <a name="global-distribution--high-availability"></a>Küresel dağıtım & yüksek kullanılabilirlik 

Azure Cosmos DB, etcd API kullanarak tek bir bölgede okuma ve yazma için %99,99 kullanılabilirlik ve birden çok bölgede %99,999 kullanılabilirlik garanti eder. 

### <a name="elastic-scalability"></a>Elastik ölçeklenebilirlik

Azure Cosmos DB, farklı bölgelerdeki okuma yazma istekleri için elastik ölçeklenebilirlik sunar.
Kubernetes kümesi büyüdükçe, Azure Cosmos DB'deki etcd API hesabı herhangi bir kesinti olmaksızın elastik olarak ölçeklenir. Kubernetes ana düğümleri yerine Azure Cosmos DB'de etcd verilerinin depolanması da daha esnek ana düğüm ölçekleme sağlar. 

### <a name="security--enterprise-readiness"></a>Güvenlik & kuruluş alahazırlık

Etcd verileri Azure Cosmos DB'de depolandığında, Kubernetes geliştiricileri yerleşik [şifrelemeyi otomatik](database-encryption-at-rest.md)olarak dinlenme , [sertifikalar ve uyumluluk](compliance.md)ve Azure Cosmos DB tarafından desteklenen yedekleme ve geri yükleme [özelliklerine](../synapse-analytics/sql-data-warehouse/backup-and-restore.md) eriştir. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB ile Azure Kubernetes nasıl kullanılır?](bootstrap-kubernetes-cluster.md)
* [Azure Cosmos DB'nin temel avantajları](introduction.md)
* [AKS motor Quickstart kılavuzu](https://github.com/Azure/aks-engine/blob/master/docs/tutorials/quickstart.md)