---
title: CloudSimple tarafından Azure VMware Çözümü - VMware Site Kurtarma Yöneticisi'ni kullanarak Özel Bulut'u olağanüstü durum kurtarma sitesi olarak ayarlama
description: CloudSimple Private Cloud'unuzu şirket içi VMware iş yükleri için olağanüstü durum kurtarma sitesi olarak nasıl ayarlayişlerinizi açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: fa8b2da683d68a337df38e13726f22c5af43540a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77565936"
---
# <a name="set-up-private-cloud-as-a-disaster-recovery-target-with-vmware-site-recovery-manager"></a>Özel Bulut'u VMware Site Kurtarma Yöneticisi ile olağanüstü durum kurtarma hedefi olarak ayarlama

CloudSimple Private Cloud'unuzu şirket içi VMware iş yükleri için olağanüstü durum kurtarma (DR) sitesi olarak kullanabilirsiniz.

DR çözümü vSphere Çoğaltma ve VMware Site Kurtarma Yöneticisi (SRM) dayanmaktadır. Özel Bulut'unuzun şirket içi kurtarma siteniz tarafından korunan birincil site olmasını sağlamak için benzer bir yaklaşım izlenebilir.

CloudSimple çözümü:

* Dr için özel olarak bir veri merkezi kurma gereksinimini ortadan kaldırır.
* CloudSimple'ın dünya çapında coğrafi esneklik için dağıtıldığı Azure konumları'ndan yararlanmanızı sağlar.
* Dr'yi kurmak için dağıtım maliyetlerini ve toplam sahip olma maliyetini azaltma seçeneği sunar.

CloudSimple çözümü aşağıdakileri yapmanızgerektiğini gerektirir:

* Özel Bulut'unuzda vSphere Çoğaltma ve SRM'yi yükleyin, yapılandırın ve yönetin.
* Özel Bulut korunan site olduğunda SRM için kendi lisanslarınızı sağlayın. Kurtarma sitesi olarak kullanıldığında CloudSimple sitesi için ek SRM lisanslarına gerek yoktur.

Bu çözüm ile vSphere çoğaltma ve SRM üzerinde tam kontrole sahip siniz. Tanıdık UI, API ve CLI arabirimleri, varolan komut dosyalarınızın ve araçlarınızın kullanımını sağlar.

![Site Kurtarma Yöneticisi dağıtımı](media/srm-deployment.png)

Özel Bulut ve şirket içi ortamlarınızla uyumlu tüm vRA ve SRM sürümlerini kullanabilirsiniz. Bu kılavuzdaki örneklerde vRA 6.5 ve SRM 6.5 kullanılmaktadır. Bu sürümler CloudSimple tarafından desteklenen vSphere 6.5 ile uyumludur.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Aşağıdaki bölümlerde, Özel Bulut'unuzda SRM kullanarak bir DR çözümünüzün nasıl dağıtılanılasınız açıklanmıştır.

1. [VMware ürün sürümlerinin uyumlu olduğunu doğrulayın](#verify-that-vmware-product-versions-are-compatible)
2. [DR ortamınızın boyutunu tahmin edin](#estimate-the-size-of-your-dr-environment)
3. [Ortamınız için Özel Bulut Oluşturma](#create-a-private-cloud-for-your-environment)
4. [SRM çözümü için Özel Bulut ağ larını ayarlama](#set-up-private-cloud-networking-for-the-srm-solution)
5. [Şirket içi ağınız ile Özel Bulut arasında Siteden Siteye VPN bağlantısı kurun ve gerekli bağlantı noktalarını açın](#set-up-a-site-to-site-vpn-connection-between-your-on-premises-network-and-the-private-cloud-and-open-required-ports)
6. [Özel Bulut'unuzda altyapı hizmetleri ayarlama](#set-up-infrastructure-services-in-your-private-cloud)
7. [vSphere Çoğaltma cihazını şirket içi ortamınıza taşırınız](#install-vsphere-replication-appliance-in-your-on-premises-environment)
8. [Özel Bulut ortamınıza vSphere Çoğaltma cihazını yükleyin](#install-vsphere-replication-appliance-in-your-private-cloud-environment)
9. [SRM sunucusunu şirket içi ortamınızda yükleme](#install-srm-server-in-your-on-premises-environment)
10. [SRM sunucusunu Özel Bulut'unuzda yükleme](#install-srm-server-in-your-private-cloud)

### <a name="verify-that-vmware-product-versions-are-compatible"></a>VMware ürün sürümlerinin uyumlu olduğunu doğrulayın

Bu kılavuzdaki yapılandırmalar aşağıdaki uyumluluk gereksinimlerine tabidir:

* SRM'nin aynı sürümü Özel Bulut'unuzda ve şirket içi ortamınızda dağıtılmalıdır.
* vSphere Replication'Un aynı sürümü Özel Bulut'unuzda ve şirket içi ortamınızda dağıtılmalıdır.
* Platform Hizmetleri Denetleyicisi'nin (PSC) Özel Bulut'unuzdaki sürümleri ve şirket içi ortamınız uyumlu olmalıdır.
* Özel Bulut'unuzdaki vCenter sürümleri ve şirket içi ortamınız uyumlu olmalıdır.
* SRM ve vSphere çoğaltma sürümleri birbirleriyle ve PSC ve vCenter sürümleri ile uyumlu olmalıdır.

İlgili VMware belgelerine ve uyumluluk bilgilerine bağlantılar için [VMware Site Kurtarma Yöneticisi](https://docs.vmware.com/en/Site-Recovery-Manager/index.html) belgelerine gidin.

Özel Bulut'unuzda vCenter ve PSC sürümlerini öğrenmek için CloudSimple portalını açın. **Kaynaklar'a**gidin, Özel Bulut'unuzu seçin ve **vSphere Management Network** sekmesini tıklatın.

![özel bulutta vCenter & PSC sürümleri](media/srm-resources.png)

### <a name="estimate-the-size-of-your-dr-environment"></a>DR ortamınızın boyutunu tahmin edin

1. Tanımlanan şirket içi yapılandırmanızın desteklenen sınırlar içinde olduğunu doğrulayın. SRM 6.5 için, sınırlar [Site Kurtarma Yöneticisi 6.5 için operasyonel limitler](https://kb.vmware.com/s/article/2147110)VMware bilgi bankası makalesinde belgelenmiştir.
2. İş yükü boyutuve RPO gereksinimlerinizi karşılamak için yeterli ağ bant genişliğine sahip olduğundan emin olun. [vSphere Çoğaltma için bant genişliği gereksinimlerini hesaplama](https://docs.vmware.com/en/vSphere-Replication/6.5/com.vmware.vsphere.replication-admin.doc/GUID-4A34D0C9-8CC1-46C4-96FF-3BF7583D3C4F.html) üzerine VMware bilgi temel makale bant genişliği sınırları hakkında rehberlik sağlar.
3. Şirket içi ortamınızı korumak için DR sitenizde gereken kaynakları tahmin etmek için CloudSimple sizer aracını kullanın.

### <a name="create-a-private-cloud-for-your-environment"></a>Ortamınız için Özel Bulut Oluşturma

[Özel Bulut Oluştur'daki](create-private-cloud.md)yönergeleri ve boyutlandırma önerilerini izleyerek CloudSimple portalından özel bulut oluşturun.

### <a name="set-up-private-cloud-networking-for-the-srm-solution"></a>SRM çözümü için Özel Bulut ağ larını ayarlama

SRM çözümü için Özel Bulut ağı kurmak için CloudSimple portalına erişin.

SRM çözüm ağı için bir VLAN oluşturun ve bir alt ağ CIDR atayın. Talimatlar için [VLANs/Subnets oluştur ve yönetme'ye](create-vlan-subnet.md)bakın.

### <a name="set-up-a-site-to-site-vpn-connection-between-your-on-premises-network-and-the-private-cloud-and-open-required-ports"></a>Şirket içi ağınız ile Özel Bulut arasında Siteden Siteye VPN bağlantısı kurun ve gerekli bağlantı noktalarını açın

Şirket içi ağınızla Özel Bulut'unuz arasında Site-Site bağlantısı kurmak için. Talimatlar için bkz. [CloudSimple Private Cloud'unuza VPN bağlantısı yapılandır.](set-up-vpn.md)

### <a name="set-up-infrastructure-services-in-your-private-cloud"></a>Özel Bulut'unuzda altyapı hizmetleri ayarlama

İş yüklerinizi ve araçlarınızı yönetmeyi kolaylaştırmak için Özel Bulut'taki altyapı hizmetlerini yapılandırın.

Aşağıdakilerden herhangi birini yapmak istiyorsanız, [CloudSimple Private Cloud'daki vCenter için kimlik sağlayıcısı olarak Azure AD'yi kullan'da](azure-ad.md) açıklandığı gibi harici bir kimlik sağlayıcısı ekleyebilirsiniz:

* Özel Bulut'unuzda şirket içi Active Directory (AD) kullanıcıları belirleyin.
* Tüm kullanıcılar için Özel Bulut'unuzda bir REKLAM ayarlayın.
* Azure AD'yi kullanın.

Özel Bulut'taki iş yüklerinizle IP adresi araması, IP adresi yönetimi ve ad çözümleme hizmetleri sağlamak için, [CloudSimple Private Cloud'unuzda DNS ve DHCP uygulamalarını ve iş yüklerini](dns-dhcp-setup.md)ayarla'da açıklandığı şekilde bir DHCP ve DNS sunucusu ayarlayın.

*.cloudsimple.io etki alanı, Özel Bulut'unuzda yönetim VM'leri ve ana bilgisayarları tarafından kullanılır. Bu etki alanına gelen istekleri gidermek için, [Koşullu İletme Oluştur'da](on-premises-dns-setup.md#create-a-conditional-forwarder)açıklandığı gibi DNS sunucusunda DNS iletmesini yapılandırın.

### <a name="install-vsphere-replication-appliance-in-your-on-premises-environment"></a>vSphere Çoğaltma Cihazını şirket içi ortamınıza taşırınız

VMware belgelerini izleyerek şirket içi ortamınıza vSphere Çoğaltma Cihazı'nı (vRA) yükleyin. Yükleme şu üst düzey adımlardan oluşur:

1. Şirket içi ortamınızı vRA kurulumu için hazırlayın.

2. vmware.com'dan ITIBAREN VR ISO'daki OVF'yi kullanarak vRA'yı şirket içi ortamınızda dağıtın. vRA 6.5 için, [bu VMware blog](https://blogs.vmware.com/virtualblocks/2017/01/20/vr-65-ovf-choices) ilgili bilgilere sahiptir.

3. Şirket içi vRA'nızı şirket içi sitede vCenter Single Sign-On ile kaydedin. vSphere Replication 6.5 için ayrıntılı talimatlar için [VMware belge VMware vSphere Replication 6.5 Kurulum ve Yapılandırma](https://docs.vmware.com/en/vSphere-Replication/6.5/vsphere-replication-65-install.pdf)bakın.

## <a name="install-vsphere-replication-appliance-in-your-private-cloud-environment"></a>Özel Bulut ortamınıza vSphere Çoğaltma cihazını yükleyin

Başlamadan önce aşağıdakileri yaptığınızı doğrulayın:

* Şirket içi ortamınızdaki alt ağlardan Özel Bulut'un yönetim alt ağına IP erişimi
* Şirket içi vSphere ortamınızdaki çoğaltma alt netinden Özel Bulut'unuzun SRM çözüm alt ağına IP erişimi

Talimatlar için bkz. [CloudSimple Private Cloud'unuza VPN bağlantısı yapılandır.](set-up-vpn.md) Adımlar, şirket içi kurulumiçin benzer.

CloudSimple, vRA ve SRM yüklemesi sırasında IP adresleri yerine FQDN'ler kullanılmasını önerir. Özel Bulut'unuzda vCenter ve PSC'nin FQDN'sini öğrenmek için CloudSimple portalını açın. **Kaynaklar'a**gidin, Özel Bulut'unuzu seçin ve **vSphere Management Network** sekmesini tıklatın.

![Özel Bulutta vCenter/PSC'nin FQDN'sini bulma](media/srm-resources.png)

CloudSimple, varsayılan 'bulut sahibi' kullanıcısını kullanarak vRA ve SRM'yi yüklememenizi, bunun yerine yeni bir kullanıcı oluşturmanızı gerektirir. Bu, Özel Bulut vCenter ortamınız için yüksek çalışma süresi ve kullanılabilirlik sağlamaya yardımcı olmak için yapılır. Ancak, Private Cloud vCenter'daki varsayılan bulut sahibi kullanıcı, yönetim ayrıcalıklarına sahip yeni bir kullanıcı oluşturmak için yeterli ayrıcalıklara sahip değildir.

vRA ve SRM'yi yüklemeden önce, bulut sahibi kullanıcının vCenter ayrıcalıklarını yükseltmeniz ve ardından vCenter SSO etki alanında Yönetim ayrıcalıklarına sahip bir kullanıcı oluşturmanız gerekir. Varsayılan Özel Bulut kullanıcısı ve izin modeliyle ilgili ayrıntılar [için](learn-private-cloud-permissions.md)bkz.

Yükleme şu üst düzey adımlardan oluşur:

1. [Ayrıcalıkları artırın.](escalate-private-cloud-privileges.md)
2. vSphere Çoğaltma ve SRM yüklemesi için Özel Bulut'unuzda bir kullanıcı oluşturun. [Aşağıda vCenter UI'de açıklanmıştır: VRA & SRM kurulumu için Özel Bulut'ta bir kullanıcı oluşturun.](#vcenter-ui-create-a-user-in-private-cloud-for-vra-and-srm-installation)
3. Özel Bulut ortamınızı vRA yüklemesi için hazırlayın.
4. vmware.com'dan itibaren VR ISO'daki OVF'yi kullanarak ÖZEL Bulut'unuzda vRA dağıtın. vRA 6.5 [için, bu VMware blog](https://blogs.vmware.com/virtualblocks/2017/01/20/vr-65-ovf-choices) ilgili bilgilere sahiptir.
5. vRA için güvenlik duvarı kurallarını yapılandırın. [CloudSimple portalında aşağıda açıklanmıştır: VRA için Güvenlik Duvarı kurallarını yapılandırın.](#cloudsimple-portal-configure-firewall-rules-for-vra)
6. Özel Bulut sitesinde vCenter Tek Oturum Açma ile Özel Bulut vRA kaydolun.
7. İki cihaz arasındaki vSphere Çoğaltma bağlantılarını yapılandırın. Güvenlik duvarları boyunca gerekli bağlantı noktalarının açıldığından emin olun. VSphere Replication 6.5 için açık olması gereken bağlantı noktası numaralarının listesi için [bu VMware bilgi bankası makalesine](https://kb.vmware.com/s/article/2087769) bakın.

vSphere Replication 6.5 için ayrıntılı kurulum talimatları için [VMware belge VMware vSphere Çoğaltma 6.5 Kurulum ve Yapılandırma](https://docs.vmware.com/en/vSphere-Replication/6.5/vsphere-replication-65-install.pdf)bakın.

#### <a name="vcenter-ui-create-a-user-in-private-cloud-for-vra-and-srm-installation"></a>vCenter UI: vRA ve SRM yüklemesi için Özel Bulut'ta bir kullanıcı oluşturma

CloudSimple portalındaki ayrıcalıkları tırmandırdıktan sonra bulut sahibi kullanıcı kimlik bilgilerini kullanarak vCenter'da oturum açın.

vCenter'da `srm-soln-admin`yeni bir kullanıcı oluşturun ve vCenter'daki yönetici grubuna ekleyin.
bulut sahibi kullanıcısı olarak vCenter'dan çıkış ve *srm-soln-admin* kullanıcısı olarak oturum açın.

#### <a name="cloudsimple-portal-configure-firewall-rules-for-vra"></a>CloudSimple portalı: vRA için güvenlik duvarı kurallarını yapılandırın

Güvenlik duvarı kurallarını, güvenlik [duvarı tablolarını ve kurallarını](firewall.md) açıklandığı şekilde, bağlantı noktalarını açmak ve aralarındaki iletişimi sağlamak için yapılandırın:

* vRA SRM çözüm ağında ve vCenter ve ESXi yönetim ağında ana bilgisayarları.
* iki sitede vRA aletleri.

VSphere Replication 6.5 için açık olması gereken bağlantı noktası numaralarının listesi için bu [VMware bilgi bankası makalesine](https://kb.vmware.com/s/article/2087769) bakın.

### <a name="install-srm-server-in-your-on-premises-environment"></a>SRM sunucusunu şirket içi ortamınızda yükleme

Başlamadan önce aşağıdakileri doğrulayın:

* vSphere Replication Appliance şirket içi ve Özel Bulut ortamlarınıza yüklenir.
* Her iki sitedeki vSphere Çoğaltma Aletleri birbirine bağlıdır.
* Ön koşullar ve en iyi uygulamalar hakkındaki VMware bilgilerini incelediniz. SRM 6.5 [için, SRM 6.5 için](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-BB0C03E4-72BE-4C74-96C3-97AC6911B6B8.html)VMware belge Önkoşulları ve En İyi Uygulamalar bakabilirsiniz.

Bu VMWare belgesinde açıklandığı gibi dağıtım modeli 'Platform Hizmetleri Denetleyicisi Başına Bir vCenter Örneği ile İki Site Topolojisi' srm sunucu yüklemegerçekleştirmek için [VMware belgelerini](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-F474543A-88C5-4030-BB86-F7CC51DADE22.html)izleyin. SRM 6.5 için kurulum talimatları VMware belge [Yükleme Site Kurtarma Yöneticisi](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-437E1B65-A17B-4B4B-BA5B-C667C90FA418.html)mevcuttur.

### <a name="install-srm-server-in-your-private-cloud"></a>SRM sunucusunu Özel Bulut'unuzda yükleme

Başlamadan önce aşağıdakileri doğrulayın:

* vSphere Replication Appliance şirket içi ve Özel Bulut ortamlarınıza yüklenir.
* Her iki sitedeki vSphere Çoğaltma Aletleri birbirine bağlıdır.
* Ön koşullar ve en iyi uygulamalar hakkındaki VMware bilgilerini incelediniz. SRM 6.5 için Site [Kurtarma Yöneticisi 6.5 Sunucu Yüklemesi için Ön koşullar alabilirsiniz.](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-BB0C03E4-72BE-4C74-96C3-97AC6911B6B8.html)

Aşağıdaki adımlar, Özel Bulut SRM yüklemesini açıklar.

1. [vCenter UI: SRM yükle](#vcenter-ui-install-srm)
2. [CloudSimple portalı: SRM için güvenlik duvarı kurallarını yapılandırma](#cloudsimple-portal-configure-firewall-rules-for-srm)
3. [vCenter UI: SRM'yi yapılandır](#vcenter-ui-configure-srm)
4. [CloudSimple portalı: ayrıcalıkları tırmandırma](#cloudsimple-portal-de-escalate-privileges)

#### <a name="vcenter-ui-install-srm"></a>vCenter UI: SRM yükle

srm-soln-admin kimlik bilgilerini kullanarak vCenter'a giriş yaptıktan sonra, bu VMWare belgesinde açıklandığı gibi dağıtım modeli 'Platform Hizmetleri Denetleyicisi Başına Bir vCenter Örneği ile İki SiteLi Topoloji' içinde SRM sunucu yüklemesini gerçekleştirmek için [VMware belgelerini](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-F474543A-88C5-4030-BB86-F7CC51DADE22.html)izleyin. SRM 6.5 için kurulum talimatları VMware belge [Yükleme Site Kurtarma Yöneticisi](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-437E1B65-A17B-4B4B-BA5B-C667C90FA418.html)mevcuttur.

#### <a name="cloudsimple-portal-configure-firewall-rules-for-srm"></a>CloudSimple portalı: SRM için güvenlik duvarı kurallarını yapılandırma

[Güvenlik duvarı tablolarını ve kurallarını ayarla'da](firewall.md) açıklandığı gibi güvenlik duvarı kurallarını, şu ikisi arasında iletişime izin verecek şekilde yapılandırın:

SRM sunucusu ve özel bulutta vCenter / PSC.
Her iki sitedeki SRM sunucuları

VSphere Replication 6.5 için açık olması gereken bağlantı noktası numaralarının listesi için [bu VMware bilgi bankası makalesine](https://kb.vmware.com/s/article/2087769) bakın.

#### <a name="vcenter-ui-configure-srm"></a>vCenter UI: SRM'yi yapılandır

SRM özel buluta yüklendikten sonra, VMware Site Kurtarma Yöneticisi Yükleme ve Yapılandırma Kılavuzu bölümlerinde açıklandığı gibi aşağıdaki görevleri gerçekleştirin. SRM 6.5 için, talimatlar VMware belge [Yükleme Site Kurtarma Yöneticisi](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-437E1B65-A17B-4B4B-BA5B-C667C90FA418.html)mevcuttur.

1. Site Kurtarma Yöneticisi Sunucu Örneklerini Korumalı ve Kurtarma Sitelerinde bağlayın.
2. Uzak Site Kurtarma Yöneticisi Sunucu Örneği'ne bir İstemci Bağlantısı kurun.
3. Site Kurtarma Yöneticisi Lisans Anahtarını yükleyin.

#### <a name="cloudsimple-portal-de-escalate-privileges"></a>CloudSimple portalı: Ayrıcalıkları yükseltme

Ayrıcalıkları daha da arttırmak için, [ayrıcalıkları yükseltmebölümüne](escalate-private-cloud-privileges.md#de-escalate-privileges)bakın.

## <a name="ongoing-management-of-your-srm-solution"></a>SRM çözümünüzün sürekli yönetimi

Özel Bulut ortamınızda vSphere Replication ve SRM yazılımı üzerinde tam kontrole sahipsiniz ve gerekli yazılım yaşam döngüsü yönetimini gerçekleştirmeniz beklenir. vSphere Replication veya SRM'yi güncellemeden veya güncellemeden önce yazılımın yeni sürümünün Private Cloud vCenter ve PSC ile uyumlu olduğundan emin olun.

> [!NOTE]
> CloudSimple şu anda yönetilen bir DR hizmeti sunmak için seçenekleri araştırıyor. 

## <a name="multiple-replication-configuration"></a>Çoklu çoğaltma yapılandırması

 Hem dizi tabanlı çoğaltma hem de vSphere çoğaltma teknolojileri aynı anda [SRM ile birlikte kullanılabilir.](https://blogs.vmware.com/virtualblocks/2017/06/22/srm-array-based-replication-vs-vsphere-replication) Ancak, bunlar ayrı VM kümesine uygulanmalıdır (belirli bir VM dizi tabanlı çoğaltma veya vSphere çoğaltma ile korunabilir, ancak her ikisi birden değil). Ayrıca, CloudSimple sitesi birden çok korumalı site için bir kurtarma sitesi olarak yapılandırılabilir. Çok siteli yapılandırmalar hakkında bilgi için [SRM Çok Site seçeneklerine](https://blogs.vmware.com/virtualblocks/2016/07/28/srm-multisite/) bakın.

## <a name="references"></a>Başvurular

* [VMware Site Kurtarma Yöneticisi Belgeleri](https://docs.vmware.com/en/Site-Recovery-Manager/index.html)
* [Site Kurtarma Yöneticisi 6.5 için operasyonel sınırlar](https://kb.vmware.com/s/article/2147110)
* [vSphere Çoğaltma için bant genişliği gereksinimlerini hesaplama](https://docs.vmware.com/en/vSphere-Replication/6.5/com.vmware.vsphere.replication-admin.doc/GUID-4A34D0C9-8CC1-46C4-96FF-3BF7583D3C4F.html)
* [vSphere Çoğaltma 6.5 Dağıtırken OVF Seçenekleri](https://blogs.vmware.com/virtualblocks/2017/01/20/vr-65-ovf-choices/)
* [VMware vSphere Çoğaltma 6.5 Kurulum ve Yapılandırma](https://docs.vmware.com/en/vSphere-Replication/6.5/vsphere-replication-65-install.pdf)
* [SRM 6.5 için Ön Koşullar ve En İyi Uygulamalar](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-BB0C03E4-72BE-4C74-96C3-97AC6911B6B8.html)
* [Platform Hizmetleri Denetleyicisi Başına Bir vCenter Server Örneği ile İki Siteli Topolojide Site Kurtarma Yöneticisi](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-F474543A-88C5-4030-BB86-F7CC51DADE22.html)
* [VMware Site Kurtarma Yöneticisi 6.5 Kurulum ve Yapılandırma Kılavuzu](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-437E1B65-A17B-4B4B-BA5B-C667C90FA418.html)
* [VMware Blog SRM dizi tabanlı çoğaltma vs vSphere çoğaltma ile](https://blogs.vmware.com/virtualblocks/2017/06/22/srm-array-based-replication-vs-vsphere-replication)
* [SRM Çoklu site seçenekleri VMware blog](https://blogs.vmware.com/virtualblocks/2016/07/28/srm-multisite)
* [vSphere Çoğaltma 5.8.x, 6.x ve 8 için açık olması gereken bağlantı noktası numaraları](https://kb.vmware.com/s/article/2147112)
