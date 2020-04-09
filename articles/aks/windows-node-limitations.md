---
title: Windows Server düğüm havuzları sınırlamaları
titleSuffix: Azure Kubernetes Service
description: Azure Kubernetes Hizmeti'nde (AKS) Windows Server düğüm havuzlarını ve uygulama iş yüklerini çalıştırdığınızda bilinen sınırlamalar hakkında bilgi edinin
services: container-service
ms.topic: article
ms.date: 12/18/2019
ms.openlocfilehash: 934acf06a779c1c3b0b13e74b196b174dd944e66
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886679"
---
# <a name="current-limitations-for-windows-server-node-pools-and-application-workloads-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) Windows Server düğüm havuzları ve uygulama iş yükleri için geçerli sınırlamalar

Azure Kubernetes Hizmeti'nde (AKS), düğümlerde konuk işletim sistemi olarak Windows Server'ı çalıştıran bir düğüm havuzu oluşturabilirsiniz. Bu düğümler,.NET Framework'de yerleşik olanlar gibi yerel Windows kapsayıcı uygulamalarını çalıştırabilir. Linux ve Windows işletim sistemi kapsayıcı desteği sağlar nasıl büyük farklılıklar olduğundan, bazı ortak Kubernetes ve pod ile ilgili özellikler şu anda Windows düğüm havuzları için kullanılabilir değildir.

Bu makalede, AKS'deki Windows Server düğümleri için bazı sınırlamalar ve işletim sistemi kavramları özetlenmiştir. Windows Server için düğüm havuzları şu anda önizlemededir.

> [!IMPORTANT]
> AKS önizleme özellikleri self servis tercihidir. Önizlemeler "olduğu gibi" ve "mevcut olduğu gibi" sağlanır ve hizmet düzeyi anlaşmaları ve sınırlı garanti dışında dır. AKS Önizlemeler kısmen en iyi çaba temelinde müşteri desteği tarafından karşılanır. Bu nedenle, bu özellikler üretim kullanımı için değildir. Daha fazla bilgi için lütfen aşağıdaki destek makalelerini görün:
>
> * [AKS Destek Politikaları][aks-support-policies]
> * [Azure Destek SSS][aks-faq]

## <a name="which-windows-operating-systems-are-supported"></a>Hangi Windows işletim sistemleri desteklenir?

AKS, Windows Server 2019'u ana bilgisayar işletim sistemi sürümü olarak kullanır ve yalnızca işlem yalıtımını destekler. Diğer Windows Server sürümleri kullanılarak oluşturulmuş kapsayıcı görüntüler desteklenmez. [Windows kapsayıcı sürüm uyumluluğu][windows-container-compat]

## <a name="is-kubernetes-different-on-windows-and-linux"></a>Windows ve Linux'ta Kubernetes farklı mı?

Pencere Sunucusu düğüm havuzu desteği, Kubernetes projesinde yukarı akışlı Windows Server'ın bir parçası olan bazı sınırlamalar içerir. Bu sınırlamalar AKS'ye özgü değildir. Kubernetes'teki Windows Server'a yönelik bu yukarı akış desteği hakkında daha fazla bilgi için [Kubernetes projesinden Kubernetes belgesinde Intro'dan Windows desteğine][intro-windows] [Desteklenen İşlevsellik ve Sınırlamalar][upstream-limitations] bölümüne bakın.

Kubernetes tarihsel olarak Linux odaklıdır. Upstream [Kubernetes.io][kubernetes] web sitesinde kullanılan birçok örnek Linux düğümleri üzerinde kullanılmak üzere tasarlanmıştır. Windows Server kapsayıcıları kullanan dağıtımlar oluşturduğunuzda, işletim sistemi düzeyinde aşağıdaki hususlar uygulanır:

- **Kimlik** - Linux, bir tamsayı kullanıcı tanımlayıcısı (UID) ile kullanıcıyı tanımlar. Bir kullanıcı da linux kullanıcının UID çevirir oturum açmak için alfasayısal bir kullanıcı adı vardır. Benzer şekilde Linux, bir kullanıcı grubunu bir tamsayı grubu tanımlayıcısı (GID) ile tanımlar ve bir grup adını karşılık gelen GID'e çevirir.
    - Windows Server, Windows Security Access Manager (SAM) veritabanında depolanan daha büyük bir ikili güvenlik tanımlayıcısı (SID) kullanır. Bu veritabanı ana bilgisayar ve kapsayıcılar arasında veya kapsayıcılar arasında paylaşılmaz.
- **Dosya izinleri** - Windows Server, izinlerin ve UID+GID'in bir bitmask'i yerine, SID'leri temel alan bir erişim denetim listesi kullanır
- **Dosya yolları** - Windows Server'da kural kuralı \yerine /.
    - Birim monte eden bölme özelliklerinde, Windows Server kapsayıcıları için yolu doğru şekilde belirtin. Örneğin, Linux kapsayıcısında */mnt/volume* montaj noktası yerine, *K:* sürücü olarak monte etmek için */K/Volume* gibi bir sürücü harfi ve konumu belirtin.

## <a name="what-kind-of-disks-are-supported-for-windows"></a>Windows için ne tür diskler desteklenir?

Azure Diskleri ve Azure Dosyaları, Windows Server kapsayıcısında NTFS birimleri olarak erişilen desteklenen birim türleridir.

## <a name="can-i-run-windows-only-clusters-in-aks"></a>Windows'u yalnızca AKS kümeleri çalıştırabilir miyim?

Bir AKS kümesindeki ana düğümler (kontrol düzlemi) AKS hizmeti tarafından barındırılır, ana bileşenleri barındıran düğümlerin işletim sistemine maruz kalırsınız. Tüm AKS kümesi, Linux tabanlı varsayılan ilk düğüm havuzuyla oluşturulur. Bu düğüm havuzu, kümenin çalışması için gerekli olan sistem hizmetlerini içerir. Kümenizin güvenilirliğini ve küme işlemlerini yapma yeteneğini sağlamak için ilk düğüm havuzunda en az iki düğüm çalıştırması önerilir. AKS kümesi nin kendisi silinmedikçe ilk Linux tabanlı düğüm havuzu silinemez.

## <a name="what-network-plug-ins-are-supported"></a>Hangi ağ eklentileri desteklenir?

Windows düğüm havuzlarına sahip AKS kümeleri Azure CNI (gelişmiş) ağ modelini kullanmalıdır. Kubenet (temel) ağ desteklenmez. Ağ modelleriarasındaki farklar hakkında daha fazla bilgi için [AKS'deki uygulamalar için Ağ kavramlarına][azure-network-models]bakın. - Azure CNI ağ modeli, IP adresi yönetimi için ek planlama ve değerlendirmeler gerektirir. Azure CNI'nin nasıl planlanıp uygulanacağı hakkında daha fazla bilgi için, [AKS'de Azure CNI ağlarını yapılandırın'][configure-azure-cni]a bakın.

## <a name="can-i-change-the-max--of-pods-per-node"></a>Limiti değiştirebilir miyim? # düğüm başına bakla?

Evet. Kullanılabilir etkileri ve seçenekleri için, [bkz.][maximum-number-of-pods]

## <a name="how-do-patch-my-windows-nodes"></a>Windows düğümlerimi nasıl yamalar?

AKS'deki Windows Server düğümlerinin en son düzeltmeleri ve güncelleştirmeleri almak için *yükseltilmesi* gerekir. Windows Güncelleştirmeleri AKS'deki düğümlerde etkinleştirilir. AKS en kısa sürede yamalar kullanılabilir yeni düğüm havuzu görüntüleri bültenleri, bu yamalar ve hotfix güncel kalmak için düğüm havuzları yükseltmek için müşterilerin sorumluluğundadır. Bu, kullanılan Kubernetes sürümü için de geçerlidir. AKS sürüm notları yeni sürümlerin ne zaman kullanılabileceğini gösterir. Windows Server düğüm havuzunun yükseltilmesi hakkında daha fazla bilgi için [AKS'de düğüm havuzuyükseltme bölümüne][nodepool-upgrade]bakın.

> [!NOTE]
> Güncelleştirilmiş Windows Server görüntüsü yalnızca düğüm havuzunu yükseltmeden önce bir küme yükseltmesi (denetim düzlemi yükseltmesi) gerçekleştirilmişse kullanılır
>

## <a name="how-do-i-rotate-the-service-principal-for-my-windows-node-pool"></a>Windows düğüm havuzum için hizmet ilkesini nasıl döndürebilirim?

Önizleme sırasında, Windows düğüm havuzları bir önizleme sınırlaması olarak hizmet analık döndürme desteklemez. Hizmet ilkesini güncelleştirmek için yeni bir Windows düğümü havuzu oluşturun ve bölmelerinizi eski havuzdan yenihavuza geçirin. Bu işlem tamamlandıktan sonra, eski düğüm havuzunu silin.

## <a name="how-many-node-pools-can-i-create"></a>Kaç düğüm havuzu oluşturabilirim?

AKS kümesien fazla 10 düğüm havuzuna sahip olabilir. Bu düğüm havuzları arasında en fazla 1000 düğüm olabilir. [Düğüm havuzu sınırlamaları.][nodepool-limitations]

## <a name="what-can-i-name-my-windows-node-pools"></a>Windows düğüm havuzlarıma ne ad verebilirim?

Adı en fazla 6 (altı) karakterde tutmanız gerekir. Bu AKS geçerli bir sınırlamadır.

## <a name="are-all-features-supported-with-windows-nodes"></a>Tüm özellikler Windows düğümleriyle mi desteklenir?

Ağ ilkeleri ve kubenet şu anda Windows düğümleri ile desteklenmez. 

## <a name="can-i-run-ingress-controllers-on-windows-nodes"></a>Windows düğümlerinde giriş denetleyicilerini çalıştırabilir miyim?

Evet, Windows Server kapsayıcılarını destekleyen bir giriş denetleyicisi AKS'de Windows düğümlerinde çalıştırılabilir.

## <a name="can-i-use-azure-dev-spaces-with-windows-nodes"></a>Windows düğümlü Azure Dev Spaces kullanabilir miyim?

Azure Dev Spaces şu anda yalnızca Linux tabanlı düğüm havuzları için kullanılabilir.

## <a name="can-my-windows-server-containers-use-gmsa"></a>Windows Server kapsayıcılarım gMSA kullanabilir mi?

Grup yönetilen hizmet hesapları (gMSA) desteği şu anda AKS'de kullanılamıyor.

## <a name="can-i-use-azure-monitor-for-containers-with-windows-nodes-and-containers"></a>Windows düğümleri ve kapsayıcıları olan kapsayıcılar için Azure Monitor kullanabilir miyim?

Evet, ancak Azure Monitor Windows kapsayıcılarından günlükleri (stdout) toplamaz. Bir Windows kapsayıcısından canlı akış akışına ekleyebilirsiniz.

## <a name="what-if-i-need-a-feature-which-is-not-supported"></a>Desteklenmeyen bir özelliğe ihtiyacım olursa ne olur?

AKS'de ihtiyacınız olan tüm özellikleri Windows'a getirmek için çok çalışıyoruz, ancak boşluklarla karşılaşırsanız, açık kaynak, akış yukarı [aks motoru][aks-engine] projesi, Windows desteği de dahil olmak üzere Azure'da Kubernete'leri çalıştırmanın kolay ve tamamen özelleştirilebilir bir yolunu sağlar. Lütfen [AKS yol haritası][aks-roadmap]geliyor özellikleri bizim yol haritası kontrol ettiğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

AKS'deki Windows Server kapsayıcılarıyla başlamak [için, AKS'de Windows Server'ı çalıştıran bir düğüm havuzu oluşturun.][windows-node-cli]

<!-- LINKS - external -->
[kubernetes]: https://kubernetes.io
[aks-engine]: https://github.com/azure/aks-engine
[upstream-limitations]: https://kubernetes.io/docs/setup/production-environment/windows/intro-windows-in-kubernetes/#supported-functionality-and-limitations
[intro-windows]: https://kubernetes.io/docs/setup/production-environment/windows/intro-windows-in-kubernetes/
[aks-roadmap]: https://github.com/Azure/AKS/projects/1

<!-- LINKS - internal -->
[azure-network-models]: concepts-network.md#azure-virtual-networks
[configure-azure-cni]: configure-azure-cni.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[windows-node-cli]: windows-container-cli.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[azure-outbound-traffic]: ../load-balancer/load-balancer-outbound-connections.md#defaultsnat
[nodepool-limitations]: use-multiple-node-pools.md#limitations
[preview-support]: support-policies.md#preview-features-or-feature-flags
[windows-container-compat]: /virtualization/windowscontainers/deploy-containers/version-compatibility?tabs=windows-server-2019%2Cwindows-10-1909
[maximum-number-of-pods]: configure-azure-cni.md#maximum-pods-per-node
