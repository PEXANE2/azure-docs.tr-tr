---
title: Azure DevTest Labs için kurumsal başvuru mimarisi
description: Bu makalede, bir kuruluştaki Azure DevTest Labs için başvuru mimarisi kılavuzu sağlanmaktadır.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77132846"
---
# <a name="azure-devtest-labs-reference-architecture-for-enterprises"></a>Azure DevTest Labs şirketler için başvuru mimarisi
Bu makale, bir kuruluşta Azure DevTest Labs'ı temel alan bir çözüm dağıtmanıza yardımcı olacak başvuru mimarisi sağlar. Aşağıdakileri içerir:
- Azure ExpressRoute ile şirket içi bağlantı
- Sanal makinelerde uzaktan oturum açmak için uzak bir masaüstü ağ geçidi
- Özel eserler için bir yapı deposuna bağlantı
- Laboratuvarlarda kullanılan diğer PaaS hizmetleri

![Referans mimari diyagramı](./media/devtest-lab-reference-architecture/reference-architecture.png)

## <a name="architecture"></a>Mimari
Başvuru mimarisinin temel öğeleri şunlardır:

- **Azure Etkin Dizin (Azure AD)**: DevTest Labs [kimlik yönetimi için Azure AD hizmetini](../active-directory/fundamentals/active-directory-whatis.md)kullanır. Kullanıcılara DevTest Labs'ı temel alan bir ortama erişim verdiğinizde şu iki önemli yönü göz önünde bulundurun:
    - **Kaynak yönetimi**: Kaynakları yönetmek için Azure portalına erişim sağlar (sanal makineler oluşturmak; ortamlar oluşturmak; başlatma, durdurma, yeniden başlatma, silme ve uygulama; vb. ). Kaynak yönetimi, rol tabanlı erişim denetimi (RBAC) kullanılarak Azure'da yapılır. Kullanıcılara roller atar ve kaynak ve erişim düzeyi izinleri ayarlarsınız.
    - **Sanal makineler (ağ düzeyinde)**: Varsayılan yapılandırmada, sanal makineler yerel bir yönetici hesabı kullanır. Kullanılabilir bir etki alanı varsa[(Azure AD Etki Alanı Hizmetleri,](../active-directory-domain-services/overview.md)şirket içi etki alanı veya bulut tabanlı etki alanı), makineler etki alanına katılabilir. Kullanıcılar daha sonra VM'lere bağlanmak için etki alanı tabanlı kimliklerini kullanabilirler.
- **Şirket içi bağlantı**: Mimari diyagramımızda [ExpressRoute](../expressroute/expressroute-introduction.md) kullanılır. Ama aynı zamanda bir [site-to-site VPN](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md)kullanabilirsiniz. ExpressRoute DevTest Labs için gerekli olmasa da, genellikle işletmelerde kullanılır. ExpressRoute yalnızca şirket kaynaklarına erişmeniz gerekiyorsa gereklidir. Sık karşılaşılan senaryolar şunlardır:
    - Buluta taşınamaz şirket içi verileriniz var.
    - Laboratuvarın sanal makinelerine şirket içi alan alanına katılmayı tercih edersiniz.
    - Güvenlik/uyumluluk için şirket içi güvenlik duvarı aracılığıyla tüm ağ trafiğini bulut ortamına zorla ve dışarı ya da bulut ortamına zorlamak istiyorsunuz.
- **Ağ güvenlik grupları**: Kaynak ve hedef IP adreslerine dayalı olarak bulut ortamına (veya bulut ortamında) trafiği kısıtlamanın yaygın bir yolu bir [ağ güvenlik grubu](../virtual-network/security-overview.md)kullanmaktır. Örneğin, yalnızca şirket ağından gelen trafiğin laboratuvar ağlarına girmesine izin vermek istiyorsunuz.
- **Uzak masaüstü ağ geçidi**: İşletmeler genellikle şirket güvenlik duvarında giden uzak masaüstü bağlantılarını engeller. DevTest Labs'daki bulut tabanlı ortama bağlantı sağlamak için çeşitli seçenekler vardır:
  - Uzak bir [masaüstü ağ geçidi](/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture)kullanın ve ağ geçidi yük bakiyesinin statik IP adresine izin verin.
  - [Gelen tüm RDP trafiğini](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) ExpressRoute/siteden siteye VPN bağlantısı üzerinden yönlendirin. İşletmeler Bir DevTest Labs dağıtımı planladığında, bu işlevsellik yaygın bir husustur.
- **Ağ hizmetleri (sanal ağlar, alt ağlar)**: [Azure ağ](../networking/networking-overview.md) topolojisi DevTest Labs mimarisinin bir diğer önemli öğesidir. Laboratuvardaki kaynakların iletişim kurup kuramayacağını ve şirket içi ve internete erişip erişemeyeceğini kontrol eder. Mimari diyagramımız, müşterilerin DevTest Labs'ı kullanmasının en yaygın yollarını içerir: Tüm laboratuvarlar, şirket içi ExpressRoute/site-to-site VPN bağlantısına [hub ile konuşan](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) bir model kullanarak sanal ağ [üzerinden](../virtual-network/virtual-network-peering-overview.md) bağlanır. Ancak DevTest Labs doğrudan Azure Sanal Ağı'nı kullanır, bu nedenle ağ altyapısını nasıl kurduğunuzkonusunda herhangi bir kısıtlama yoktur.
- **DevTest Labs**: DevTest Labs genel mimarinin önemli bir parçasıdır. Hizmet hakkında daha fazla bilgi edinmek için [DevTest Labs hakkında](devtest-lab-overview.md)bilgi edinin.
- **Sanal makineler ve diğer kaynaklar (SaaS, PaaS, IaaS)**: Sanal makineler, DevTest Labs'ın diğer Azure kaynaklarıyla birlikte desteklediği önemli bir iş yükütür. DevTest Labs, bir işletmenin Azure kaynaklarına (VM'ler ve diğer Azure kaynakları dahil) erişim sağlamasını kolay ve hızlı hale getirir. [Geliştiriciler](devtest-lab-developer-lab.md) ve [sınayıcılar](devtest-lab-test-env.md)için Azure'a erişim hakkında daha fazla bilgi edinin.

## <a name="scalability-considerations"></a>Ölçeklenebilirlik konusunda dikkat edilmesi gerekenler
DevTest Labs'ın yerleşik kotaları veya sınırları olmasa da, bir laboratuvarın tipik çalışmasında kullanılan diğer Azure kaynaklarının [abonelik düzeyi kotaları](../azure-resource-manager/management/azure-subscription-service-limits.md)vardır. Bu nedenle, tipik bir kurumsal dağıtımda, DevTest Labs'ın büyük bir dağıtımını karşılamak için birden çok Azure aboneliğine ihtiyacınız vardır. İşletmelerin en sık ulaştığı kotalar şunlardır:

- **Kaynak grupları**: Varsayılan yapılandırmada, DevTest Labs her yeni sanal makine için bir kaynak grubu oluşturur veya kullanıcı hizmeti kullanarak bir ortam oluşturur. Abonelikler [en fazla 980 kaynak grubu](../azure-resource-manager/management/azure-subscription-service-limits.md#subscription-limits)içerebilir. Yani, bu bir abonelik sanal makine ve ortamların sınırıdır. Göz önünde bulundurmanız gereken iki yapılandırma daha vardır:
    - **[Tüm sanal makineler aynı kaynak grubuna gider](resource-group-control.md)**: Bu kurulum kaynak grubu sınırını karşılamanıza yardımcı olsa da, kaynak türü grup başına sınırını etkiler.
    - **Paylaşılan Genel IP'lerin kullanılması**: Aynı boyutta ve bölgede yer alan tüm VM'ler aynı kaynak grubuna girer. Bu yapılandırma, sanal makinelerin ortak IP adreslerine sahip olması için izin veriliyorsa, kaynak grubu kotaları ile kaynak türü başına kotalar arasında bir "orta yol"dur.
- **Kaynak türü başına kaynak grubu başına**kaynaklar : Kaynak türü başına kaynak grubu başına kaynaklar için varsayılan sınır [800'dür.](../azure-resource-manager/management/azure-subscription-service-limits.md#resource-group-limits)  *Tüm VM'leri kullandığınızda aynı kaynak grubu yapılandırmasına gidin,* kullanıcılar bu abonelik sınırına çok daha erken basar, özellikle de VM'lerde çok sayıda ek disk varsa.
- **Depolama hesapları**: DevTest Labs'daki bir laboratuvarda bir depolama hesabı bulunur. Abonelik başına bölge başına depolama hesabı sayısı için Azure [kotası 250'dir.](../azure-resource-manager/management/azure-subscription-service-limits.md#storage-limits) Aynı bölgedeki en fazla DevTest Labs sayısı da 250'dir.
- **Rol atamaları**: Rol ataması, bir kaynağa (sahip, kaynak, izin düzeyi) kullanıcıya veya asıl erişime nasıl erişilendir. Azure'da, abonelik [başına 2.000 rol ataması sınırı](../azure-resource-manager/management/azure-subscription-service-limits.md#role-based-access-control-limits)vardır. Varsayılan olarak, DevTest Labs hizmeti her VM için bir kaynak grubu oluşturur. Sahibine DevTest Labs VM için *sahip* izni ve kaynak grubuna *okuyucu* izni verilir. Bu şekilde, oluşturduğunuz her yeni VM, kullanıcılara laboratuvara izin verdiğinizde kullanılan atamalara ek olarak iki rol ataması kullanır.
- **API okur/yazar:** REST API'leri, PowerShell, Azure CLI ve Azure SDK dahil olmak üzere Azure ve DevTest Laboratuvarlarını otomatikleştirmenin çeşitli yolları vardır. Otomasyon sayesinde, API isteklerinde başka bir sınıra ulaşabilirsiniz: Her abonelik [saatte 12.000'e kadar okuma isteğine ve 1.200 yazma isteğine](../azure-resource-manager/management/request-limits-and-throttling.md)izin verir. DevTest Labs'ı otomatikleştirdiğinizde bu sınıra dikkat edin.

## <a name="manageability-considerations"></a>Yönetilebilirlik konusunda dikkat edilmesi gerekenler
DevTest Labs tek bir laboratuvar ile çalışmak için büyük bir idari kullanıcı arayüzü vardır. Ancak bir kuruluşta, büyük olasılıkla birden çok Azure aboneliğiniz ve birçok laboratuvarınız vardır. Tüm laboratuvarlarınızda tutarlı değişiklikler yapmak için komut dosyası oluşturma/otomasyon gerektirir. DevTest Labs dağıtımı için bazı örnekler ve en iyi yönetim uygulamaları aşağıda verilmiştir:

- **Laboratuar ayarlarında yapılan değişiklikler**: Yaygın bir senaryo, dağıtımdaki tüm laboratuarlarda belirli bir laboratuvar ayarını güncelleştirmektir. Örneğin, yeni bir VM örnek boyutu kullanılabilir ve tüm laboratuvarların buna izin verecek şekilde güncelleştirilmesi gerekir. PowerShell komut dosyaları, CLI veya REST API'lerini kullanarak bu değişiklikleri otomatikleştirmek en iyisidir.  
- **Artefakt deposu kişisel erişim belirteci**: Genellikle, bir Git deposu için kişisel erişim belirteçleri 90 gün, bir yıl veya iki yıl içinde sona erer. Sürekliliği sağlamak için, kişisel erişim jetonunu genişletmek veya yeni bir tane oluşturmak önemlidir. Ardından, yeni kişisel erişim jetonunu tüm laboratuvarlara uygulamak için otomasyonu kullanın.
- **Laboratuvar ayarındaki değişiklikleri kısıtlama**: Genellikle belirli bir ayar sınırlandırılmalıdır (örneğin, pazar yeri görüntülerinin kullanımına izin vermek gibi). Kaynak türünde yapılan değişiklikleri önlemek için Azure İlkesi'ni kullanabilirsiniz. Veya özel bir rol oluşturabilir ve kullanıcılara laboratuvar sahibi *rolü* yerine bu rolü verebilirsiniz. Bunu laboratuvardaki çoğu ayar için yapabilirsiniz (dahili destek, laboratuvar duyurusu, izin verilen VM boyutları vb.).
- **VM'lerin bir adlandırma kuralını izlemesini zorunlu kılmasını sorun**: Yöneticiler genellikle bulut tabanlı geliştirme ve test ortamının parçası olan VM'leri kolayca tanımlamak ister. Bunu [Azure İlkesi'ni](https://github.com/Azure/azure-policy/tree/master/samples/TextPatterns/allow-multiple-name-patterns)kullanarak yapabilirsiniz.

DevTest Labs'ın, ağ, diskler, bilgi işlem ve benzeri şekilde yönetilen temel Azure kaynaklarını kullandığını unutmayın. Örneğin, Azure İlkesi laboratuvarda oluşturulan sanal makineler için geçerlidir. Azure Güvenlik Merkezi VM uyumluluğu hakkında rapor verebilir. Azure Yedekleme hizmeti, laboratuvardaki VM'ler için düzenli yedekleme sağlayabilir.

## <a name="security-considerations"></a>Güvenlik konuları
Azure DevTest Labs, Azure'daki varolan kaynakları (bilgi işlem, ağ vb.) kullanır. Bu nedenle platformda yerleşik güvenlik özelliklerinden otomatik olarak yararlanır. Örneğin, gelen uzak masaüstü bağlantılarının yalnızca şirket ağından kaynaklanmalarını gerektirmek için, uzak masaüstü ağ geçidindeki sanal ağa bir ağ güvenlik grubu eklemeniz yeterlidir. Tek ek güvenlik göz önünde bulundurulması gereken, laboratuvarları günlük olarak kullanan ekip üyelerine vermiş olduğunuz izinlerin düzeyidir. En yaygın izinler [ *sahibi* ve *kullanıcısıdır.*](devtest-lab-add-devtest-user.md) Bu roller hakkında daha fazla bilgi için Azure [DevTest Labs'daki sahiplerini ve kullanıcılarını ekle'ye](devtest-lab-add-devtest-user.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Bu serinin bir sonraki makalesine bakın: [Azure DevTest Labs altyapınızı ölçeklendirin.](devtest-lab-guidance-scale.md)
