---
title: Azure Kubernetes Hizmeti (AKS) için en iyi uygulamalar
description: Azure Kubernetes Hizmeti 'nde (AKS) uygulamaları oluşturmak ve yönetmek için küme operatörü ve geliştirici en iyi uygulamaları toplama
services: container-service
ms.topic: article
ms.date: 12/07/2018
ms.openlocfilehash: 627eeffd18a479486e5a682da06bf89cd5f8f0e1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77596326"
---
# <a name="cluster-operator-and-developer-best-practices-to-build-and-manage-applications-on-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti (AKS) uygulamaları oluşturmak ve yönetmek için küme operatörü ve geliştirici en iyi uygulamaları

Azure Kubernetes Hizmeti'nde (AKS) uygulamaları başarıyla oluşturmak ve çalıştırmak için anlaşılması ve uygulanması gereken bazı önemli noktalar vardır. Bu alanlar arasında çoklu kira ve zamanlayıcı özellikleri, küme ve bakla güvenliği veya iş sürekliliği ve olağanüstü durum kurtarma yer almaktadır. Küme operatörlerinin ve geliştiricilerin bu alanların her biri için gerekenleri anlamalarına ve uygun özellikleri uygulamalarına yardımcı olmak için aşağıdaki en iyi uygulamalar gruplandırılır.

Bu en iyi uygulamalar ve kavramsal makaleler AKS ürün grubu, mühendislik ekipleri ve küresel siyah kuşaklar (GBBs) dahil saha ekipleri ile birlikte yazılmıştır.

## <a name="cluster-operator-best-practices"></a>Küme operatörü en iyi uygulamalar

Küme işleci olarak, gereksinimlerini anlamak için uygulama sahipleri ve geliştiricilerle birlikte çalışın. Daha sonra aks kümelerinizi gerektiği gibi yapılandırmak için aşağıdaki en iyi uygulamaları kullanabilirsiniz.

**Çok kiracılı**

* [Küme yalıtımı için en iyi deneyimler](operator-best-practices-cluster-isolation.md)
    * Çok kiralı temel bileşenleri ve ad boşlukları ile mantıksal yalıtım içerir.
* [Temel zamanlayıcı özellikleri için en iyi deneyimler](operator-best-practices-scheduler.md)
    * Kaynak kotaları ve bölme kesintisi bütçelerini kullanmayı içerir.
* [Gelişmiş zamanlayıcı özellikleri için en iyi deneyimler](operator-best-practices-advanced-scheduler.md)
    * Leke ve tolerasyonlar, düğüm seçiciler ve yakınlık ve inter-pod afinite ve anti-afiity kullanarak içerir.
* [Kimlik doğrulaması ve yetkilendirme için en iyi yöntemler](operator-best-practices-identity.md)
    * Rol tabanlı erişim denetimleri (RBAC) ve pod kimliklerini kullanarak Azure Etkin Dizini ile tümleştirmeyi içerir.

**Güvenlik**

* [Küme güvenliği ve yükseltmeler için en iyi yöntemler](operator-best-practices-cluster-security.md)
    * API sunucusuna erişimi güvence altına almak, kapsayıcı erişimini sınırlamak ve yükseltmeleri ve düğüm yeniden başlatmalarını yönetmeyi içerir.
* [Konteyner görüntü yönetimi ve güvenliği için en iyi uygulamalar](operator-best-practices-container-image-management.md)
    * Temel görüntü güncelleştirmelerinde görüntünün ve çalışma saatlerinin ve otomatik yapının güvenliğini içerir.
* [Pod güvenliği için en iyi uygulamalar](developer-best-practices-pod-security.md)
    * Kaynaklara erişimi güvence altına almak, kimlik bilgisi maruziyetini sınırlamak ve bölme kimlikleri ve dijital anahtar kasalarını kullanmayı içerir.

**Ağ ve depolama**

* [Ağ bağlantısı için en iyi uygulamalar](operator-best-practices-network.md)
    * Giriş ve web uygulaması güvenlik duvarlarını (WAF) kullanarak ve düğüm SSH erişimini güvence altına alan farklı ağ modellerini içerir.
* [Depolama ve yedekleme için en iyi yöntemler](operator-best-practices-storage.md)
    * Uygun depolama türü ve düğüm boyutunu seçmeyi, birimleri dinamik olarak sağlamayı ve veri yedeklemelerini içerir.

**Kuruluşa hazır iş yüklerini çalıştırma**

* [İş sürekliliği ve olağanüstü durum kurtarma için en iyi uygulamalar](operator-best-practices-multi-region.md)
    * Bölge çiftleri, Azure Trafik Yöneticisi ile birden çok küme ve kapsayıcı görüntülerinin coğrafi çoğaltılması içerir.

## <a name="developer-best-practices"></a>Geliştirici en iyi uygulamaları

Geliştirici veya uygulama sahibi olarak, geliştirme deneyiminizi basitleştirebilir ve uygulama performansı gereksinimlerini tanımlayabilirsiniz.

* [Uygulama geliştiriciler için kaynak yönetimine yönelik en iyi yöntemler](developer-best-practices-resource-management.md)
    * Pod kaynak isteklerini ve sınırlarını tanımlamayı, geliştirme araçlarını yapılandırmayı ve uygulama sorunlarını denetlemeyi içerir.
* [Pod güvenliği için en iyi uygulamalar](developer-best-practices-pod-security.md)
    * Kaynaklara erişimi güvence altına almak, kimlik bilgisi maruziyetini sınırlamak ve bölme kimlikleri ve dijital anahtar kasalarını kullanmayı içerir.

## <a name="kubernetes--aks-concepts"></a>Kubernetes / AKS kavramları

Bu en iyi uygulamaların bazı özelliklerini ve bileşenlerini anlamanıza yardımcı olmak için, Azure Kubernetes Hizmeti'ndeki (AKS) kümeler için aşağıdaki kavramsal makaleleri de görebilirsiniz:

* [Kubernetes temel kavramları](concepts-clusters-workloads.md)
* [Erişim ve kimlik](concepts-identity.md)
* [Güvenlik kavramları](concepts-security.md)
* [Ağ kavramları](concepts-network.md)
* [Depolama seçenekleri](concepts-storage.md)
* [Ölçekleme seçenekleri](concepts-scale.md)

## <a name="next-steps"></a>Sonraki adımlar

AKS'ye başlamanız gerekiyorsa, [Azure CLI](kubernetes-walkthrough.md) veya [Azure portalını](kubernetes-walkthrough-portal.md)kullanarak bir Azure Kubernetes Hizmeti (AKS) kümesini dağıtmaya başlama hızlarından birini izleyin.
