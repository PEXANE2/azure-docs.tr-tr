---
title: Windows Server düğüm havuzları hakkında SSS
titleSuffix: Azure Kubernetes Service
description: Azure Kubernetes Service 'te (AKS) Windows Server düğüm havuzlarını ve uygulama iş yüklerini çalıştırdığınızda sık sorulan sorular bölümüne bakın.
services: container-service
ms.topic: article
ms.date: 07/29/2020
ms.openlocfilehash: df9a4dd546ddc5944d9a282e74c2444a5161b862
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87927574"
---
# <a name="frequently-asked-questions-for-windows-server-node-pools-in-aks"></a>AKS 'deki Windows Server düğüm havuzları hakkında sık sorulan sorular

Azure Kubernetes Service (AKS) ' de, düğümlerde Konuk işletim sistemi olarak Windows Server çalıştıran bir düğüm havuzu oluşturabilirsiniz. Bu düğümler, .NET Framework yerleşik olanlar gibi yerel Windows kapsayıcı uygulamaları çalıştırabilir. Linux ve Windows işletim sisteminin kapsayıcı desteği sağladığı farklar vardır. Bazı yaygın Linux Kubernetes ve pod ile ilgili özellikler Şu anda Windows düğüm havuzları için kullanılamaz.

Bu makalede, AKS 'deki Windows Server düğümleri için sık sorulan sorulardan ve işletim sistemi kavramlarından bazıları özetlenmektedir.

## <a name="which-windows-operating-systems-are-supported"></a>Hangi Windows işletim sistemleri destekleniyor?

AKS, ana bilgisayar işletim sistemi sürümü olarak Windows Server 2019 kullanır ve yalnızca işlem yalıtımını destekler. Diğer Windows Server sürümleri kullanılarak oluşturulan kapsayıcı görüntüleri desteklenmez. Daha fazla bilgi için bkz. [Windows kapsayıcı sürümü uyumluluğu][windows-container-compat].

## <a name="is-kubernetes-different-on-windows-and-linux"></a>Kubernetes, Windows ve Linux 'ta farklı midir?

Pencere sunucusu düğüm havuzu desteği, Kubernetes projesinde yukarı akış Windows Server 'ın bir parçası olan bazı sınırlamalar içerir. Bu sınırlamalar AKS 'e özgü değildir. Kubernetes 'te Windows Server için bu yukarı akış desteği hakkında daha fazla bilgi için, Kubernetes projesinden [Kubernetes belgesinde Windows desteği 'Ne giriş][intro-windows] konusunun [desteklenen işlevsellik ve sınırlamalar][upstream-limitations] bölümüne bakın.

Kubernetes, geçmişte Linux odaklı. Yukarı akış [Kubernetes.io][kubernetes] Web sitesinde kullanılan birçok örnek, Linux düğümlerinde kullanılmak üzere tasarlanmıştır. Windows Server kapsayıcıları kullanan dağıtımlar oluşturduğunuzda, işletim sistemi düzeyinde aşağıdaki noktalar geçerlidir:

- **Identity** -Linux bir kullanıcıyı bir tamsayı Kullanıcı TANıMLAYıCıSıNA (UID) göre tanımlar. Kullanıcının oturum açmak için alfasayısal bir Kullanıcı adı da vardır. Bu, Linux 'un Kullanıcı UID 'ye çeviri anlamına gelir. Benzer şekilde Linux bir kullanıcı grubunu bir tamsayı grubu tanımlayıcısı (GID) ile tanımlar ve bir grup adını karşılık gelen GID 'ye çevirir.
    - Windows Server, Windows güvenlik erişimi Yöneticisi (SAM) veritabanında depolanan daha büyük bir ikili güvenlik tanımlayıcısı (SID) kullanır. Bu veritabanı, konak ve kapsayıcılar ile ya da kapsayıcılar arasında paylaşılmaz.
- **Dosya izinleri** -Windows Server, bir bit MASKESI ve UID + GID yerine SID 'lere dayalı bir erişim denetim listesi kullanır
- **Dosya yolları** -Windows Server 'daki kural/yerine \ ' i kullanmaktır.
    - Birimleri takmaya yönelik Pod özellikleri, Windows Server kapsayıcıları için yolu doğru belirtin. Örneğin, bir Linux kapsayıcısında */mnt/Volume* bağlama noktası yerine, *K:* sürücüsü olarak takılacak bir sürücü harfi ve */k/birimi* gibi bir konum belirtin.

## <a name="what-kind-of-disks-are-supported-for-windows"></a>Windows için ne tür diskler destekleniyor?

Azure diskleri ve Azure dosyaları desteklenen birim türleridir. Bunlara Windows Server kapsayıcısında NTFS birimleri olarak erişilir.

## <a name="can-i-run-windows-only-clusters-in-aks"></a>AKS 'de yalnızca Windows kümelerini çalıştırabilir miyim?

Bir AKS kümesindeki ana düğümler (denetim düzlemi) hizmet tarafından barındırılır, ana bileşenleri barındıran düğümlerin işletim sistemine maruz olmayacaktır. Tüm AKS kümeleri, Linux tabanlı olan varsayılan ilk düğüm havuzu ile oluşturulur. Bu düğüm havuzu, kümenin çalışması için gerekli olan sistem hizmetlerini içerir. Kümenizin güvenilirliğini ve küme işlemlerini yapma yeteneğini sağlamak için ilk düğüm havuzunda en az iki düğüm çalıştırmanız önerilir. AKS kümesinin kendisi silinmediği takdirde ilk Linux tabanlı düğüm havuzu silinemez.

## <a name="how-do-i-patch-my-windows-nodes"></a>Windows düğümlerimi Nasıl yaparım? yama yapılsın mı?

Windows düğümlerine yönelik en son düzeltme eklerini almak için [düğüm havuzunu yükseltebilir][nodepool-upgrade] veya [düğüm görüntüsünü yükseltebilirsiniz][upgrade-node-image]. Windows güncelleştirmeleri AKS içindeki düğümlerde etkin değildir. AKS, etkin olmayan yeni düğüm havuzu görüntülerini yayınlar ve düzeltme ekleri ve düzeltmeleri üzerinde güncel kalmak için kullanıcının düğüm havuzlarını yükseltme sorumluluğundadır. Bu, aynı zamanda kullanılan Kubernetes sürümü için de geçerlidir. [Aks sürüm notları][aks-release-notes] yeni sürümlerin ne zaman kullanılabilir olduğunu gösterir. Tüm Windows Server düğüm havuzunu yükseltme hakkında daha fazla bilgi için bkz. [AKS 'de düğüm havuzunu yükseltme][nodepool-upgrade]. Yalnızca düğüm görüntüsünü güncelleştirmeyi ilgileniyorsanız, bkz. [aks node Image yükseltmeleri][upgrade-node-image].

> [!NOTE]
> Güncelleştirilmiş Windows Server görüntüsü, düğüm havuzunu yükseltmeden önce bir küme yükseltmesi (denetim düzlemi yükseltmesi) gerçekleştirildiğinde kullanılacaktır.
>

## <a name="what-network-plug-ins-are-supported"></a>Hangi ağ eklentileri destekleniyor?

Windows node havuzlarıyla AKS kümelerinin Azure CNı (Gelişmiş) ağ modelini kullanması gerekir. Kubenet (temel) ağı desteklenmez. Ağ modellerindeki farklılıklar hakkında daha fazla bilgi için bkz. [AKS 'teki uygulamalar Için ağ kavramları][azure-network-models]. Azure CNı ağ modeli, IP adresi yönetimi için ek planlama ve dikkat edilecek noktalar gerektirir. Azure CNı 'nin nasıl planlanacağı ve uygulanacağı hakkında daha fazla bilgi için bkz. [AKS 'de Azure CNI ağını yapılandırma][configure-azure-cni].

## <a name="is-preserving-the-client-source-ip-supported"></a>İstemci kaynak IP 'si destekleniyor mu?

Şu anda, [istemci kaynak IP koruması][client-source-ip] Windows düğümlerinde desteklenmez.

## <a name="can-i-change-the-max--of-pods-per-node"></a>Maksimum değeri değiştirebilir miyim? düğüm başına düşen öğe sayısı?

Evet. Kullanılabilir etkileri ve seçenekleri için bkz. [maksimum sayıda Pod][maximum-number-of-pods].

## <a name="why-am-i-seeing-an-error-when-i-try-to-create-a-new-windows-agent-pool"></a>Yeni bir Windows Agent havuzu oluşturmaya çalıştığımda neden hata görüyorum?

Kümenizi 2020 Şubat 'tan önce oluşturduysanız ve hiçbir türlü küme yükseltme işlemini yapmadıysanız, küme hala eski bir Windows görüntüsü kullanır. Aşağıdakine benzer bir hata görmüş olabilirsiniz:

"Dağıtım şablonundan başvurulan aşağıdaki görüntülerin listesi bulunamadı: Yayımcı: MicrosoftWindowsServer, teklif: WindowsServer, SKU: 2019-Datacenter-Core-smalldisk-2004, sürüm: Latest. https://docs.microsoft.com/azure/virtual-machines/windows/cli-ps-findimageKullanılabilir görüntüleri bulma yönergeleri için lütfen adresine bakın. "

Bu hatayı onarmak için:

1. Görüntü teklifini ve yayımcıyı güncelleştirmek için [küme denetim düzlemi][upgrade-cluster-cp] ' ni yükseltin.
1. Yeni Windows Agent havuzları oluşturun.
1. Windows yığınlarını var olan Windows Agent havuzlarından yeni Windows Agent havuzlarına taşıyın.
1. Eski Windows Agent havuzlarını silin.

## <a name="how-do-i-rotate-the-service-principal-for-my-windows-node-pool"></a>Windows düğüm havuzum için hizmet sorumlusunu Nasıl yaparım? döndürün?

Windows düğüm havuzları hizmet sorumlusu döndürmesini desteklemez. Hizmet sorumlusunu güncelleştirmek için yeni bir Windows düğüm havuzu oluşturun ve eski havuzdan yığınlarınızı yeni bir havuza geçirin. Bu işlem tamamlandıktan sonra eski düğüm havuzunu silin.

Bunun yerine, hizmet sorumluları etrafında temel sarmalayıcılar olan yönetilen kimlikler kullanın. Daha fazla bilgi için bkz. [Azure Kubernetes hizmetinde Yönetilen kimlikler kullanma][managed-identity].

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

## <a name="what-if-i-need-a-feature-thats-not-supported"></a>Desteklenmeyen bir özelliğe ihtiyacım varsa ne yapmalıyım?

AKS 'de Windows 'a ihtiyacınız olan tüm özellikleri getirmek için çok çalıştık, ancak boşluklar yaşarsanız açık kaynaklı, yukarı akış [aks-motor][aks-engine] projesi, Windows desteği dahil olmak üzere Azure 'Da Kubernetes 'in kolay ve tamamen özelleştirilebilir bir yolunu sağlar. Çok sayıda özellik [yol haritasını][aks-roadmap]kullanıma sunduğunuzdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

AKS 'de Windows Server kapsayıcıları kullanmaya başlamak için [AKS 'de Windows Server çalıştıran bir düğüm havuzu oluşturun][windows-node-cli].

<!-- LINKS - external -->
[kubernetes]: https://kubernetes.io
[aks-engine]: https://github.com/azure/aks-engine
[upstream-limitations]: https://kubernetes.io/docs/setup/production-environment/windows/intro-windows-in-kubernetes/#supported-functionality-and-limitations
[intro-windows]: https://kubernetes.io/docs/setup/production-environment/windows/intro-windows-in-kubernetes/
[aks-roadmap]: https://github.com/Azure/AKS/projects/1
[aks-release-notes]: https://github.com/Azure/AKS/releases

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
[upgrade-node-image]: node-image-upgrade.md
[managed-identity]: use-managed-identity.md
