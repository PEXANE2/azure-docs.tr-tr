---
title: Windows Server düğüm havuzları sınırlamaları
titleSuffix: Azure Kubernetes Service
description: Azure Kubernetes Service 'te (AKS) Windows Server düğüm havuzlarını ve uygulama iş yüklerini çalıştırırken oluşan bilinen sınırlamalar hakkında bilgi edinin
services: container-service
ms.topic: article
ms.date: 05/28/2020
ms.openlocfilehash: a86d6f0fe942a72a96c504a61d5030624f161cd5
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86507021"
---
# <a name="current-limitations-for-windows-server-node-pools-and-application-workloads-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) içindeki Windows Server düğüm havuzları ve uygulama iş yükleri için geçerli sınırlamalar

Azure Kubernetes Service (AKS) ' de, düğümlerde Konuk işletim sistemi olarak Windows Server çalıştıran bir düğüm havuzu oluşturabilirsiniz. Bu düğümler, .NET Framework yerleşik olanlar gibi yerel Windows kapsayıcı uygulamaları çalıştırabilir. Linux ve Windows işletim sisteminin kapsayıcı desteği sağladığı önemli farklılıklar söz konusu olduğunda, bazı yaygın Kubernetes ve pod ile ilgili özellikler Şu anda Windows düğüm havuzları için kullanılamaz.

Bu makalede, AKS 'deki Windows Server düğümlerine yönelik bazı sınırlamalar ve işletim sistemi kavramları özetlenmektedir.

## <a name="which-windows-operating-systems-are-supported"></a>Hangi Windows işletim sistemleri destekleniyor?

AKS, ana bilgisayar işletim sistemi sürümü olarak Windows Server 2019 kullanır ve yalnızca işlem yalıtımını destekler. Diğer Windows Server sürümleri kullanılarak oluşturulan kapsayıcı görüntüleri desteklenmez. [Windows kapsayıcı sürümü uyumluluğu][windows-container-compat]

## <a name="is-kubernetes-different-on-windows-and-linux"></a>Kubernetes, Windows ve Linux 'ta farklı midir?

Pencere sunucusu düğüm havuzu desteği, Kubernetes projesinde yukarı akış Windows Server 'ın bir parçası olan bazı sınırlamalar içerir. Bu sınırlamalar AKS 'e özgü değildir. Kubernetes 'te Windows Server için bu yukarı akış desteği hakkında daha fazla bilgi için, Kubernetes projesinden [Kubernetes belgesinde Windows desteği 'Ne giriş][intro-windows] konusunun [desteklenen işlevsellik ve sınırlamalar][upstream-limitations] bölümüne bakın.

Kubernetes, geçmişte Linux odaklı. Yukarı akış [Kubernetes.io][kubernetes] Web sitesinde kullanılan birçok örnek, Linux düğümlerinde kullanılmak üzere tasarlanmıştır. Windows Server kapsayıcıları kullanan dağıtımlar oluşturduğunuzda, işletim sistemi düzeyinde aşağıdaki noktalar geçerlidir:

- **Identity** -Linux bir kullanıcıyı bir tamsayı Kullanıcı TANıMLAYıCıSıNA (UID) göre tanımlar. Kullanıcının oturum açmak için alfasayısal bir Kullanıcı adı da vardır. Bu, Linux 'un Kullanıcı UID 'ye çeviri anlamına gelir. Benzer şekilde Linux bir kullanıcı grubunu bir tamsayı grubu tanımlayıcısı (GID) ile tanımlar ve bir grup adını karşılık gelen GID 'ye çevirir.
    - Windows Server, Windows güvenlik erişimi Yöneticisi (SAM) veritabanında depolanan daha büyük bir ikili güvenlik tanımlayıcısı (SID) kullanır. Bu veritabanı, konak ve kapsayıcılar ile ya da kapsayıcılar arasında paylaşılmaz.
- **Dosya izinleri** -Windows Server, bir bit MASKESI ve UID + GID yerine SID 'lere dayalı bir erişim denetim listesi kullanır
- **Dosya yolları** -Windows Server 'daki kural/yerine \ ' i kullanmaktır.
    - Birimleri takmaya yönelik Pod özellikleri, Windows Server kapsayıcıları için yolu doğru belirtin. Örneğin, bir Linux kapsayıcısında */mnt/Volume* bağlama noktası yerine, *K:* sürücüsü olarak takılacak bir sürücü harfi ve */k/birimi* gibi bir konum belirtin.

## <a name="what-kind-of-disks-are-supported-for-windows"></a>Windows için ne tür diskler destekleniyor?

Azure diskleri ve Azure dosyaları, Windows Server kapsayıcısında NTFS birimleri olarak erişilen desteklenen birim türleridir.

## <a name="can-i-run-windows-only-clusters-in-aks"></a>AKS 'de yalnızca Windows kümelerini çalıştırabilir miyim?

Bir AKS kümesindeki ana düğümler (denetim düzlemi) hizmet tarafından barındırılır, ana bileşenleri barındıran düğümlerin işletim sistemine maruz olmayacaktır. Tüm AKS kümesi, Linux tabanlı olan varsayılan ilk düğüm havuzu ile oluşturulur. Bu düğüm havuzu, kümenin çalışması için gerekli olan sistem hizmetlerini içerir. Kümenizin güvenilirliğini ve küme işlemlerini yapma yeteneğini sağlamak için ilk düğüm havuzunda en az iki düğüm çalıştırmanız önerilir. AKS kümesinin kendisi silinmediği takdirde ilk Linux tabanlı düğüm havuzu silinemez.

## <a name="what-network-plug-ins-are-supported"></a>Hangi ağ eklentileri destekleniyor?

Windows node havuzlarıyla AKS kümelerinin Azure CNı (Gelişmiş) ağ modelini kullanması gerekir. Kubenet (temel) ağı desteklenmez. Ağ modellerindeki farklılıklar hakkında daha fazla bilgi için bkz. [AKS 'teki uygulamalar Için ağ kavramları][azure-network-models]. Azure CNı ağ modeli, IP adresi yönetimi için ek planlama ve dikkat edilecek noktalar gerektirir. Azure CNı 'nin nasıl planlanacağı ve uygulanacağı hakkında daha fazla bilgi için bkz. [AKS 'de Azure CNI ağını yapılandırma][configure-azure-cni].

## <a name="is-preserving-the-client-source-ip-supported"></a>İstemci kaynak IP 'si destekleniyor mu?

Şu anda, [istemci kaynak IP koruması][client-source-ip] Windows düğümlerinde desteklenmez.

## <a name="can-i-change-the-max--of-pods-per-node"></a>Maksimum değeri değiştirebilir miyim? düğüm başına düşen öğe sayısı?

Evet. Kullanılabilir etkileri ve seçenekleri için bkz. [maksimum sayıda Pod][maximum-number-of-pods].

## <a name="how-do-patch-my-windows-nodes"></a>Windows düğümlerimi nasıl yaalabilirim?

En son düzeltme eki düzeltmelerini ve güncelleştirmelerini almak için AKS 'deki Windows Server düğümlerinin *yükseltilmesi* gerekir. Windows güncelleştirmeleri AKS içindeki düğümlerde etkin değildir. AKS yeni düğüm havuzu görüntülerini yayınlar, ancak düzeltme ekleri kullanılabilir olduğunda, bu durumda müşterilerin düğüm havuzlarını yükselterek düzeltme ekleri ve düzeltmede güncel kalmasını sağlar. Bu, aynı zamanda kullanılan Kubernetes sürümü için de geçerlidir. AKS sürüm notları, yeni sürümlerin ne zaman kullanılabilir olduğunu gösterir. Bir Windows Server düğüm havuzunu yükseltme hakkında daha fazla bilgi için bkz. [AKS 'de düğüm havuzunu yükseltme][nodepool-upgrade].

> [!NOTE]
> Güncelleştirilmiş Windows Server görüntüsü, düğüm havuzu yükseltmeden önce bir küme yükseltmesi (denetim düzlemi yükseltmesi) gerçekleştirildiğinde kullanılacaktır
>

## <a name="why-am-i-seeing-an-error-when-i-try-to-create-a-new-windows-agent-pool"></a>Yeni bir Windows Agent havuzu oluşturmaya çalıştığımda neden hata görüyorum?

Kümenizi 2020 Şubat 'tan önce oluşturduysanız ve hiçbir şekilde hiçbir küme yükseltme işlemi yapmadıysanız, küme hala eski bir Windows görüntüsü kullanır. Aşağıdakine benzer bir hata görmüş olabilirsiniz:

"Dağıtım şablonundan başvurulan aşağıdaki görüntülerin listesi bulunamadı: Yayımcı: MicrosoftWindowsServer, teklif: WindowsServer, SKU: 2019-Datacenter-Core-smalldisk-2004, sürüm: Latest. https://docs.microsoft.com/azure/virtual-machines/windows/cli-ps-findimageKullanılabilir görüntüleri bulma yönergeleri için lütfen adresine bakın. "

Bu hatayı onarmak için:

1. [Küme denetim düzlemi][upgrade-cluster-cp]' ni yükseltin. Bu işlem, görüntü teklifini ve yayımcıyı güncelleştirir.
1. Yeni Windows Agent havuzları oluşturun.
1. Windows yığınlarını var olan Windows Agent havuzlarından yeni Windows Agent havuzlarına taşıyın.
1. Eski Windows Agent havuzlarını silin.

## <a name="how-do-i-rotate-the-service-principal-for-my-windows-node-pool"></a>Windows düğüm havuzum için hizmet sorumlusunu Nasıl yaparım? döndürün?

Windows düğüm havuzları hizmet sorumlusu döndürmesini desteklemez. Hizmet sorumlusunu güncelleştirmek için yeni bir Windows düğüm havuzu oluşturun ve eski havuzdan yığınlarınızı yeni bir havuza geçirin. Bu işlem tamamlandıktan sonra eski düğüm havuzunu silin.

## <a name="how-many-node-pools-can-i-create"></a>Kaç düğüm havuzu oluşturabilirim?

AKS kümesinde en fazla 10 düğüm havuzu olabilir. Bu düğüm havuzlarının üzerinde en fazla 1000 düğümünüz olabilir. [Düğüm havuzu sınırlamaları][nodepool-limitations].

## <a name="what-can-i-name-my-windows-node-pools"></a>Windows node havuzlarımı nasıl kullanabilirim?

Adı en fazla 6 (altı) karaktere saklamanız gerekir. Bu, AKS 'in geçerli bir sınırlamasıdır.

## <a name="are-all-features-supported-with-windows-nodes"></a>Tüm özellikler Windows düğümleriyle destekleniyor mu?

Ağ ilkeleri ve Kubernetes kullanan Şu anda Windows düğümleri ile desteklenmemektedir.

## <a name="can-i-run-ingress-controllers-on-windows-nodes"></a>Giriş denetleyicilerini Windows düğümlerinde çalıştırabilir miyim?

Evet, Windows Server kapsayıcıları destekleyen bir giriş denetleyicisi AKS 'deki Windows düğümlerinde çalıştırılabilir.

## <a name="can-i-use-azure-dev-spaces-with-windows-nodes"></a>Windows düğümleri ile Azure Dev Spaces kullanabilir miyim?

Azure Dev Spaces Şu anda yalnızca Linux tabanlı düğüm havuzları için kullanılabilir.

## <a name="can-my-windows-server-containers-use-gmsa"></a>Windows Server kapsayıcılarım gMSA kullanıyor mu?

Grup tarafından yönetilen hizmet hesapları (gMSA) desteği şu anda AKS 'de kullanılamıyor.

## <a name="can-i-use-azure-monitor-for-containers-with-windows-nodes-and-containers"></a>Windows düğümleri ve kapsayıcılarıyla kapsayıcılar için Azure Izleyici kullanabilir miyim?

Evet, ancak Azure Izleyici, Windows kapsayıcılarından günlükleri (stdout, stderr) ve ölçümleri toplamak için genel önizlemede bulunabilir. Ayrıca, bir Windows kapsayıcısından stdout günlüklerinin canlı akışına de ekleyebilirsiniz.

## <a name="are-there-any-limitations-on-the-number-of-services-on-a-cluster-with-windows-nodes"></a>Windows düğümlerine sahip bir kümedeki hizmet sayısıyla ilgili sınırlamalar var mı?

Windows düğümlerine sahip bir küme, bağlantı noktası tükenmesi ile karşılaşmadan önce yaklaşık 500 hizmete sahip olabilir.

## <a name="can-i-use-the-kubernetes-web-dashboard-with-windows-containers"></a>Kubernetes web panosunu Windows kapsayıcılarıyla kullanabilir miyim?

Evet, [Kubernetes web panosunu][kubernetes-dashboard] Windows kapsayıcıları hakkındaki bilgilere erişmek için kullanabilirsiniz, ancak şu anda *kubectl exec* 'i çalışan bir Windows kapsayıcısına doğrudan Kubernetes Web panosundan çalıştıramazsınız. Çalışan Windows kapsayıcınıza bağlanma hakkında daha fazla bilgi için bkz. [Azure Kubernetes Service (AKS) küme Windows Server düğümlerine bakım veya sorun giderme IÇIN RDP Ile bağlanma][windows-rdp].

## <a name="what-if-i-need-a-feature-which-is-not-supported"></a>Desteklenmeyen bir özelliğe ihtiyacım varsa ne olur?

AKS 'de Windows 'a ihtiyacınız olan tüm özellikleri getirmek için çok çalıştık, ancak boşluklar yaşarsanız açık kaynaklı, yukarı akış [aks-motor][aks-engine] projesi, Windows desteği dahil olmak üzere Azure 'Da Kubernetes 'in kolay ve tamamen özelleştirilebilir bir yolunu sağlar. Lütfen kendi [aks yol haritasını][aks-roadmap]inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

AKS 'de Windows Server kapsayıcıları kullanmaya başlamak için [AKS 'de Windows Server çalıştıran bir düğüm havuzu oluşturun][windows-node-cli].

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
[upgrade-cluster]: upgrade-cluster.md
[upgrade-cluster-cp]: use-multiple-node-pools.md#upgrade-a-cluster-control-plane-with-multiple-node-pools
[azure-outbound-traffic]: ../load-balancer/load-balancer-outbound-connections.md#defaultsnat
[nodepool-limitations]: use-multiple-node-pools.md#limitations
[windows-container-compat]: /virtualization/windowscontainers/deploy-containers/version-compatibility?tabs=windows-server-2019%2Cwindows-10-1909
[maximum-number-of-pods]: configure-azure-cni.md#maximum-pods-per-node
[azure-monitor]: ../azure-monitor/insights/container-insights-overview.md#what-does-azure-monitor-for-containers-provide
[client-source-ip]: concepts-network.md#ingress-controllers
[kubernetes-dashboard]: kubernetes-dashboard.md
[windows-rdp]: rdp.md
