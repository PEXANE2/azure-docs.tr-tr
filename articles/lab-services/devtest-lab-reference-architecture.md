---
title: Azure DevTest Labs için kurumsal başvuru mimarisi
description: Bu makale, bir kuruluşta Azure DevTest Labs için başvuru mimarisi kılavuzu sağlar.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: 77e6ab588f74c8b810f211e069c1c24043155111
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77132846"
---
# <a name="azure-devtest-labs-reference-architecture-for-enterprises"></a>Kuruluşlar için Azure DevTest Labs başvuru mimarisi
Bu makalede, bir kuruluştaki Azure DevTest Labs göre bir çözüm dağıtmanıza yardımcı olacak başvuru mimarisi sunulmaktadır. Şunları içerir:
- Azure ExpressRoute aracılığıyla şirket içi bağlantı
- Sanal makinelerde uzaktan oturum açmak için Uzak Masaüstü Ağ Geçidi
- Özel yapıtlar için yapıt deposuna bağlantı
- Laboratuvarlarda kullanılan diğer PaaS hizmetleri

![Başvuru mimarisi diyagramı](./media/devtest-lab-reference-architecture/reference-architecture.png)

## <a name="architecture"></a>Mimari
Başvuru mimarisinin temel öğeleri şunlardır:

- **Azure Active Directory (Azure AD)**: DevTest Labs [kimlik YÖNETIMI için Azure ad hizmetini](../active-directory/fundamentals/active-directory-whatis.md)kullanır. Kullanıcılara DevTest Labs tabanlı bir ortama erişim verdiğinizde, bu iki temel yönü göz önünde bulundurun:
    - **Kaynak yönetimi**: kaynakları yönetmek için Azure Portal erişim sağlar (sanal makineler oluşturun; ortam oluşturma; başlatma, durdurma, yeniden başlatma, silme ve yapıları uygulama; vb.). Kaynak yönetimi, Azure 'da rol tabanlı erişim denetimi (RBAC) kullanılarak yapılır. Kullanıcılara roller atarsınız ve kaynak ve erişim düzeyi izinlerini ayarlarsınız.
    - **Sanal makineler (ağ düzeyi)**: Varsayılan yapılandırmada, sanal makineler yerel bir yönetici hesabı kullanır. Kullanılabilir bir etki alanı ([Azure AD Domain Services](../active-directory-domain-services/overview.md), şirket içi etki alanı veya bulut tabanlı etki alanı) varsa, makineler etki alanına katılabilir. Kullanıcılar, VM 'lere bağlanmak için etki alanı tabanlı kimliklerini kullanabilir.
- **Şirket içi bağlantı**: mimari diyagramımızda [ExpressRoute](../expressroute/expressroute-introduction.md) kullanılır. Ancak [siteden sıteye VPN](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md)de kullanabilirsiniz. DevTest Labs için ExpressRoute gerekli olmasa da genellikle kuruluşlarda kullanılır. ExpressRoute yalnızca şirket kaynaklarına erişmeniz gerekiyorsa gereklidir. Yaygın senaryolar şunlardır:
    - Buluta taşınamayacak şirket içi verileriniz var.
    - Laboratuvarın sanal makinelerini şirket içi etki alanına katılmayı tercih edersiniz.
    - Güvenlik/uyumluluk için şirket içi bir güvenlik duvarı aracılığıyla bulut ortamının içindeki ve olmayan tüm ağ trafiğini zorlamak istiyorsunuz.
- **Ağ güvenlik grupları**: kaynak ve hedef IP adreslerine bağlı olarak bulut ortamıyla (veya bulut ortamında) trafiği kısıtlayan yaygın bir yolu, bir [ağ güvenlik grubu](../virtual-network/security-overview.md)kullanmaktır. Örneğin, yalnızca şirket ağından laboratuvar ağlarına giden trafiğe izin vermek istersiniz.
- **Uzak Masaüstü Ağ Geçidi**: kuruluşlar, genellikle şirket güvenlik duvarında giden uzak masaüstü bağlantılarını engeller. DevTest Labs 'de bulut tabanlı ortama bağlantıyı etkinleştirmek için çeşitli seçenekler vardır; örneğin:
  - Bir [Uzak Masaüstü Ağ Geçidi](/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture)kullanın ve ağ geçidi yük DENGELEYICININ statik IP adresine izin verin.
  - [Tüm gelen RDP trafiğini](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) ExpressRoute/sıteden siteye VPN bağlantısı üzerinden yönlendirin. Bu işlevsellik, kuruluşlar bir DevTest Labs dağıtımı planlarken yaygın bir noktadır.
- **Ağ Hizmetleri (sanal ağlar, alt ağlar)**: [Azure ağ](../networking/networking-overview.md) topolojisi, DevTest Labs mimarisinde başka bir anahtar öğesidir. Laboratuvardaki kaynakların, şirket içi ve internet erişimi olup olmadığını denetler. Mimari diyagramımızda, müşterilerin DevTest Labs 'i kullanma konusunda en yaygın yolları vardır: tüm laboratuvarlar, şirket içi ExpressRoute/siteden siteye VPN bağlantısına bir [hub-ışınsal modeli](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) kullanarak [sanal ağ eşlemesi](../virtual-network/virtual-network-peering-overview.md) aracılığıyla bağlanır. Ancak DevTest Labs, Azure sanal ağını doğrudan kullanır, bu nedenle ağ altyapısını ayarlama konusunda herhangi bir kısıtlama yoktur.
- **DevTest Labs**: DevTest Labs, genel mimarinin önemli bir parçasıdır. Hizmet hakkında daha fazla bilgi edinmek için bkz. [DevTest Labs hakkında](devtest-lab-overview.md).
- **Sanal makineler ve diğer kaynaklar (SaaS, PaaS, IaaS)**: sanal makineler, DevTest Labs 'In diğer Azure kaynaklarıyla birlikte desteklediği önemli bir iş yüküdür. DevTest Labs, bir kuruluşun Azure kaynaklarına (VM 'Ler ve diğer Azure kaynakları dahil) erişim sağlamasına yönelik daha kolay ve hızlı bir şekilde erişmenizi sağlar. [Geliştiriciler](devtest-lab-developer-lab.md) ve [test ediciler](devtest-lab-test-env.md)için Azure 'a erişim hakkında daha fazla bilgi edinin.

## <a name="scalability-considerations"></a>Ölçeklenebilirlik konusunda dikkat edilmesi gerekenler
DevTest Labs yerleşik kotalar veya sınırlara sahip olmasa da, bir laboratuvarın tipik işleminde kullanılan diğer Azure kaynaklarının [abonelik düzeyinde kotaları](../azure-resource-manager/management/azure-subscription-service-limits.md)vardır. Bu nedenle, tipik bir kurumsal dağıtımda, büyük bir DevTest Labs dağıtımını kapsayan birden çok Azure aboneliğine sahip olmanız gerekir. İşletmelerin en yaygın olarak ulaşabileceği kotalar şunlardır:

- **Kaynak grupları**: Varsayılan yapılandırmada, DevTest Labs her yeni sanal makine için bir kaynak grubu oluşturur veya Kullanıcı Hizmeti kullanarak bir ortam oluşturur. Abonelikler, [en fazla 980 kaynak grubu](../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits)içerebilir. Bu nedenle, bir abonelikteki sanal makinelerin ve ortamların sınırı vardır. Dikkate almanız gereken iki farklı yapılandırma vardır:
    - **[Tüm sanal makineler aynı kaynak grubuna gider](resource-group-control.md)**: Bu kurulum, kaynak grubu sınırını karşılamanıza yardımcı olmakla birlikte kaynak-tür-kaynak grubu sınırını da etkiler.
    - **Paylaşılan genel IP 'Leri kullanma**: aynı boyut ve bölgeye ait tüm VM 'ler aynı kaynak grubuna gider. Bu yapılandırma, sanal makinelerin genel IP adreslerine sahip olmasına izin veriliyorsa kaynak grup kotaları ve kaynak-tür-kaynak grubu kotaları arasında "orta zemin" dır.
- Kaynak türü başına kaynak **grubu**başına kaynak sayısı: kaynak [türü başına kaynak grubu başına kaynaklar](../azure-resource-manager/management/azure-subscription-service-limits.md#resource-group-limits)için varsayılan sınır 800 ' dir.  *Tüm VM 'leri kullandığınızda, aynı kaynak grubu yapılandırmasına git* , özellikle de VM 'lerde çok fazla disk varsa, kullanıcılar bu abonelik sınırına çok daha erken gider.
- **Depolama hesapları**: DevTest Labs 'deki bir laboratuvar bir depolama hesabıyla birlikte gelir. [Abonelik başına bölge başına depolama hesabı sayısı](../azure-resource-manager/management/azure-subscription-service-limits.md#storage-limits)için Azure kotası 250 ' dir. Aynı bölgedeki en fazla DevTest Labs sayısı da 250 ' dir.
- **Rol atamaları**: bir rol ataması, bir kaynağa (sahip, kaynak, izin düzeyi) bir kullanıcı veya sorumlu erişimi verme işlemi. Azure 'da, [abonelik başına 2.000 rol ataması sınırlaması](../azure-resource-manager/management/azure-subscription-service-limits.md#role-based-access-control-limits)vardır. Varsayılan olarak, DevTest Labs hizmeti her VM için bir kaynak grubu oluşturur. Sahibe, kaynak grubuna DevTest Labs sanal makinesi ve *okuyucu* izni *için sahip izni verilir.* Bu şekilde, oluşturduğunuz her yeni VM, kullanıcıya laboratuvar izni verdiğinizde kullanılan atamalara ek olarak iki rol ataması kullanır.
- **API okuma/yazma**: REST API 'Ler, PowerShell, Azure CLI ve Azure SDK dahil olmak üzere Azure ve DevTest Labs 'i otomatikleştirmek için çeşitli yollar vardır. Otomasyon sayesinde API istekleri için başka bir sınıra ulaşırsınız: her abonelik, [saat başına 12.000 okuma isteği ve 1.200 yazma isteği](../azure-resource-manager/management/request-limits-and-throttling.md)sağlar. DevTest Labs 'i otomatikleştirdiğiniz zaman bu sınırı unutmayın.

## <a name="manageability-considerations"></a>Yönetilebilirlik konusunda dikkat edilmesi gerekenler
DevTest Labs, tek bir laboratuvarda çalışmak için harika bir yönetim kullanıcı arabirimine sahiptir. Ancak bir kuruluşta, büyük olasılıkla birden çok Azure aboneliğiniz ve birçok laboratuvarı vardır. Tüm laboratuvarlarınızın sürekli olarak değişiklik yapma betiği/Otomasyonu gerektirir. Aşağıda, bir DevTest Labs dağıtımı için bazı örnekler ve en iyi yönetim uygulamaları verilmiştir:

- **Laboratuvar ayarlarındaki değişiklikler**: ortak bir senaryo, dağıtımdaki tüm laboratuvarlar genelinde belirli bir laboratuvar ayarını güncelleştirmedir. Örneğin, yeni bir sanal makine örneği boyutu kullanılabilir ve tüm laboratuvarlara izin vermek için güncelleştirilmeleri gerekir. PowerShell betikleri, CLı veya REST API 'Leri kullanarak bu değişiklikleri otomatikleştirmek en iyisidir.  
- **Yapıt deposu kişisel erişim belirteci**: genellikle, bir git deposunun kişisel erişim belirteçleri 90 gün, bir yıl veya iki yıl içinde sona erer. Devamlılığını sağlamak için, kişisel erişim belirtecini genişletmek veya yeni bir tane oluşturmak önemlidir. Ardından Otomasyon kullanarak yeni kişisel erişim belirtecini tüm laboratuvarlara uygulayın.
- **Laboratuvar ayarlarına yapılan değişiklikleri kısıtla**: genellikle belirli bir ayar kısıtlı olmalıdır (Market görüntülerinin kullanılmasına izin verme gibi). Kaynak türünde değişiklik yapılmasını engellemek için Azure Ilkesi ' ni kullanabilirsiniz. Ya da özel bir rol oluşturabilir ve kullanıcıya laboratuvar için *sahip* rolü yerine bu rolü verebilirsiniz. Bunu laboratuvardaki çoğu ayar için yapabilirsiniz (iç destek, laboratuvar duyurusu, izin verilen VM boyutları vb.).
- **VM 'lerin bir adlandırma kuralını Izlemesini gerektir**: Yöneticiler, bulut tabanlı bir geliştirme ve test ortamının bir parçası olan sanal makineleri genellikle kolayca belirlemek ister. Bunu [Azure ilkesi](https://github.com/Azure/azure-policy/tree/master/samples/TextPatterns/allow-multiple-name-patterns)kullanarak yapabilirsiniz.

DevTest Labs 'in aynı şekilde yönetilen temel Azure kaynaklarını kullandığını unutmayın: ağ, diskler, işlem, vb. Örneğin, Azure Ilkesi laboratuarda oluşturulan sanal makinelere uygulanır. Azure Güvenlik Merkezi, VM uyumluluğunu rapor edebilir. Azure Backup hizmeti, laboratuvardaki VM 'Ler için düzenli yedeklemeler sağlayabilir.

## <a name="security-considerations"></a>Güvenlik konuları
Azure DevTest Labs Azure 'da mevcut kaynakları (işlem, ağ vb.) kullanır. Bu nedenle, platformda yerleşik olarak bulunan güvenlik özelliklerinden otomatik olarak yararlanır. Örneğin, gelen Uzak Masaüstü bağlantılarının yalnızca kurumsal ağdan kaynaklanacak olması için, Uzak Masaüstü ağ geçidinde bulunan sanal ağa bir ağ güvenlik grubu eklemeniz yeterlidir. Tek ek güvenlik değerlendirmesi, laboratuvarları gündelik bir şekilde kullanan takım üyelerine verdiğiniz izinlerin düzeyidir. En yaygın izinler [ *sahip* ve *Kullanıcı*](devtest-lab-add-devtest-user.md). Bu roller hakkında daha fazla bilgi için, bkz. [Azure DevTest Labs sahip ve Kullanıcı ekleme](devtest-lab-add-devtest-user.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu serinin sonraki makalesine bakın: [Azure DevTest Labs altyapınızı ölçeklendirin](devtest-lab-guidance-scale.md).
