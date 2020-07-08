---
title: Azure Kubernetes hizmeti (AKS) için en iyi yöntemler
description: Azure Kubernetes Service (AKS) içinde uygulama derlemek ve yönetmek için küme operatörü ve geliştirici en iyi uygulamaları koleksiyonu
services: container-service
ms.topic: article
ms.date: 12/07/2018
ms.openlocfilehash: 627eeffd18a479486e5a682da06bf89cd5f8f0e1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77596326"
---
# <a name="cluster-operator-and-developer-best-practices-to-build-and-manage-applications-on-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) üzerinde uygulama derlemek ve yönetmek için küme operatörü ve geliştirici en iyi uygulamaları

Azure Kubernetes Service 'te (AKS) uygulamaları başarılı bir şekilde derlemek ve çalıştırmak için anlaşılması ve uygulanması gereken önemli noktalar vardır. Bu alanlarda çok kiracılı ve Zamanlayıcı özellikleri, küme ve pod güvenliği, iş sürekliliği ve olağanüstü durum kurtarma dahildir. Aşağıdaki en iyi yöntemler, küme İşletmenleri ve geliştiricilerin bu alanların her birine ilişkin konuları anlamasına ve uygun özellikleri uygulamasına yardımcı olmak üzere gruplandırılır.

Bu en iyi yöntemler ve kavramsal makaleler, AKS ürün grubu, mühendislik takımları ve küresel siyah belts (GB 'ler) dahil alan ekipleriyle birlikte yazılmıştır.

## <a name="cluster-operator-best-practices"></a>Küme işleci en iyi uygulamaları

Bir küme operatörü olarak, ihtiyaçlarını anlamak için uygulama sahipleri ve geliştiricilerle birlikte çalışın. Daha sonra, AKS kümelerinizi gerektiği şekilde yapılandırmak için aşağıdaki en iyi yöntemleri kullanabilirsiniz.

**Çok kiracılı**

* [Küme yalıtımı için en iyi deneyimler](operator-best-practices-cluster-isolation.md)
    * Çok kiracılı çekirdek bileşenleri ve ad alanlarıyla mantıksal yalıtım içerir.
* [Temel zamanlayıcı özellikleri için en iyi deneyimler](operator-best-practices-scheduler.md)
    * Kaynak kotalarını ve pod kesinti bütçelerini kullanmayı içerir.
* [Gelişmiş zamanlayıcı özellikleri için en iyi deneyimler](operator-best-practices-advanced-scheduler.md)
    * Talara ve toleranlara, düğüm seçicilerini ve benzeşimine ve yerleşik olmayan benzeşim ve benzeşim kullanımını içerir.
* [Kimlik doğrulaması ve yetkilendirme için en iyi yöntemler](operator-best-practices-identity.md)
    * Rol tabanlı erişim denetimleri (RBAC) ve pod kimliklerini kullanarak Azure Active Directory tümleştirme içerir.

**Güvenlik**

* [Küme güvenliği ve yükseltmeler için en iyi yöntemler](operator-best-practices-cluster-security.md)
    * API sunucusuna erişimin güvenliğini sağlama, kapsayıcı erişimini sınırlandırma ve yükseltmeleri ve düğüm yeniden başlatmaları yönetme işlemlerini içerir.
* [Kapsayıcı görüntüsü yönetimi ve güvenlik için en iyi uygulamalar](operator-best-practices-container-image-management.md)
    * Temel görüntü güncelleştirmelerinde görüntünün ve çalışma zamanlarının ve otomatikleştirilmiş derlemelerin güvenliğini sağlama içerir.
* [Pod güvenliği için en iyi uygulamalar](developer-best-practices-pod-security.md)
    * Kaynaklara erişimin güvenliğini sağlama, kimlik bilgisi pozlamasını sınırlandırma ve pod kimliklerini ve dijital anahtar kasalarını kullanmayı içerir.

**Ağ ve depolama**

* [Ağ bağlantısı için en iyi uygulamalar](operator-best-practices-network.md)
    * , Giriş ve Web uygulaması güvenlik duvarları (WAF) kullanarak farklı ağ modelleri içerir ve düğüm SSH erişiminin güvenliğini sağlama.
* [Depolama ve yedekleme için en iyi yöntemler](operator-best-practices-storage.md)
    * Uygun depolama türü ve düğüm boyutu, dinamik olarak sağlama birimleri ve veri yedeklemeleri seçmeyi içerir.

**Kurumsal özellikli iş yüklerini çalıştırma**

* [İş sürekliliği ve olağanüstü durum kurtarma için en iyi uygulamalar](operator-best-practices-multi-region.md)
    * Bölge çiftlerinin kullanımını, Azure Traffic Manager ile birden çok kümeyi ve kapsayıcı görüntülerinin coğrafi olarak çoğaltılmasını içerir.

## <a name="developer-best-practices"></a>Geliştirici en iyi uygulamaları

Geliştirici veya uygulama sahibi olarak, geliştirme deneyiminizi basitleştirebilir ve uygulama performansı ihtiyaçlarını gerekli kılabilirsiniz.

* [Uygulama geliştiriciler için kaynak yönetimine yönelik en iyi yöntemler](developer-best-practices-resource-management.md)
    * Pod kaynak isteklerini ve sınırlarını tanımlamayı, geliştirme araçlarını yapılandırmayı ve uygulama sorunları olup olmadığını içerir.
* [Pod güvenliği için en iyi uygulamalar](developer-best-practices-pod-security.md)
    * Kaynaklara erişimin güvenliğini sağlama, kimlik bilgisi pozlamasını sınırlandırma ve pod kimliklerini ve dijital anahtar kasalarını kullanmayı içerir.

## <a name="kubernetes--aks-concepts"></a>Kubernetes/AKS kavramları

Bu en iyi yöntemlerin bazı özellik ve bileşenlerinden bazılarının anlaşılmasına yardımcı olmak için Azure Kubernetes Service (AKS) içindeki kümeler için aşağıdaki kavramsal makaleleri de görebilirsiniz:

* [Kubernetes temel kavramları](concepts-clusters-workloads.md)
* [Erişim ve kimlik](concepts-identity.md)
* [Güvenlik kavramları](concepts-security.md)
* [Ağ kavramları](concepts-network.md)
* [Depolama seçenekleri](concepts-storage.md)
* [Ölçeklendirme seçenekleri](concepts-scale.md)

## <a name="next-steps"></a>Sonraki adımlar

AKS 'yi kullanmaya başlamak istiyorsanız, [Azure CLI](kubernetes-walkthrough.md) veya [Azure Portal](kubernetes-walkthrough-portal.md)kullanarak bir Azure Kubernetes hizmeti (aks) kümesi dağıtmak için hızlı başlangıçlardan birini izleyin.
