---
title: Azure VMware çözümleri (AVS)-VMware Site Recovery Manager kullanarak AVS özel bulutunu olağanüstü durum kurtarma sitesi olarak ayarlama
description: Şirket içi VMware iş yükleri için AVS özel bulutunuzu bir olağanüstü durum kurtarma sitesi olarak ayarlamayı açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: ccdf385d2581923e1fad4fa5c42f351c4f0947ca
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2020
ms.locfileid: "77083159"
---
# <a name="set-up-avs-private-cloud-as-a-disaster-recovery-target-with-vmware-site-recovery-manager"></a>Bir VMware Site Recovery Manager ile AVS özel bulutunu olağanüstü durum kurtarma hedefi olarak ayarlama

AVS özel bulutunuzu, şirket içi VMware iş yükleri için olağanüstü durum kurtarma (DR) sitesi olarak kullanabilirsiniz.

DR çözümü, vSphere çoğaltmasını, VMware Site Recovery Manager 'ı (SRM) temel alır. Benzer bir yaklaşım, AVS özel bulutunuzu şirket içi kurtarma siteniz tarafından korunan bir birincil site olarak etkinleştirmek için izlenebilir.

AVS çözümü:

* Özellikle DR için bir veri merkezi ayarlama gereksinimini ortadan kaldırır.
* , Dünya çapındaki coğrafi esnekliği için AVS 'nin dağıtıldığı Azure konumlarından yararlanmanızı sağlar.
* , DR oluşturmaya yönelik dağıtım maliyetlerini ve toplam sahiplik maliyetini azaltmaya yönelik bir seçenek sunar.

AVS çözümü şunları yapmanızı gerektirir:

* Sanal Sphere çoğaltmasını ve SRM 'yi AVS özel bulutunuzda yönetin, yapılandırın ve yönetin.
* AVS özel bulutu korumalı site olduğunda, SRM için kendi lisanslarınızı sağlayın. Kurtarma sitesi olarak kullanıldığında AVS sitesi için ek bir lisans gerekmez.

Bu çözümle, vSphere çoğaltma ve SRM üzerinde tam denetime sahip olursunuz. Tanıdık UI, API ve CLı arabirimleri, mevcut betiklerinizin ve araçlarınızın kullanımını sağlar.

![Site Recovery Manager dağıtımı](media/srm-deployment.png)

Tüm vRA ve SRM sürümlerini, AVS özel bulutunuz ve şirket içi ortamlarınızla uyumlu olacak şekilde kullanabilirsiniz. Bu kılavuzdaki örneklerde vRA 6,5 ve SRM 6,5 kullanılır. Bu sürümler, AVS tarafından desteklenen vSphere 6,5 ile uyumludur.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Aşağıdaki bölümlerde, AVS özel bulutunuzda SRM kullanarak bir DR çözümünün nasıl dağıtılacağı açıklanır.

1. [VMware ürün sürümlerinin uyumlu olduğunu doğrulama](#verify-that-vmware-product-versions-are-compatible)
2. [DR ortamınızın boyutunu tahmin etme](#estimate-the-size-of-your-dr-environment)
3. [Ortamınız için bir AVS özel bulutu oluşturun](#create-an-avs-private-cloud-for-your-environment)
4. [SRM çözümü için AVS özel bulut ağını ayarlama](#set-up-avs-private-cloud-networking-for-the-srm-solution)
5. [Şirket içi ağınız ile AVS özel bulutu arasında siteden siteye VPN bağlantısı kurun ve gerekli bağlantı noktalarını açın](#set-up-a-site-to-site-vpn-connection-between-your-on-premises-network-and-the-avs-private-cloud-and-open-required-ports)
6. [AVS özel bulutunuzda altyapı hizmetlerini ayarlama](#set-up-infrastructure-services-in-your-avs-private-cloud)
7. [Şirket içi ortamınıza vSphere çoğaltma gereci yüklemesi](#install-vsphere-replication-appliance-in-your-on-premises-environment)
8. [AVS özel bulut ortamınıza vSphere çoğaltma gereci yüklemesi](#install-vsphere-replication-appliance-in-your-avs-private-cloud-environment)
9. [Şirket içi ortamınıza SRM sunucusu 'nı yüklemeyin](#install-srm-server-in-your-on-premises-environment)
10. [AVS özel bulutunuzda SRM sunucusunu yükler](#install-srm-server-in-your-avs-private-cloud)

### <a name="verify-that-vmware-product-versions-are-compatible"></a>VMware ürün sürümlerinin uyumlu olduğunu doğrulama

Bu kılavuzdaki konfigürasyonlar aşağıdaki uyumluluk gereksinimlerine tabidir:

* Aynı SRM sürümünün AVS özel bulutunuzda ve şirket içi ortamınızda dağıtılması gerekir.
* Aynı vSphere çoğaltma sürümünün AVS özel bulutunuzda ve şirket içi ortamınızda dağıtılması gerekir.
* AVS özel bulutunuzda ve şirket içi ortamınızda bulunan Platform Hizmetleri denetleyicisi 'nin (PSC) sürümleri uyumlu olmalıdır.
* AVS 'nin özel bulutunuzda ve şirket içi ortamınızda vCenter 'ın sürümleri uyumlu olmalıdır.
* SRM ve vSphere çoğaltmasının sürümleri birbirleriyle ve PSC ve vCenter sürümleriyle uyumlu olmalıdır.

İlgili VMware belgelerinin ve uyumluluk bilgilerinin bağlantıları için [VMware Site Recovery Manager](https://docs.vmware.com/en/Site-Recovery-Manager/index.html) belgeleri ' ne gidin.

AVS özel bulutunuzda vCenter ve PSC sürümlerini bulmak için AVS portalını açın. **Kaynaklar**' a gıdın, AVS özel bulutunuzu seçin ve **vSphere yönetim ağı** sekmesine tıklayın.

![AVS özel bulutu 'nda vCenter & PSC sürümleri](media/srm-resources.png)

### <a name="estimate-the-size-of-your-dr-environment"></a>DR ortamınızın boyutunu tahmin etme

1. Tanımlı şirket içi yapılandırmanızın desteklenen sınırlar dahilinde olduğunu doğrulayın. SRM 6,5 için sınırlar, VMware Bilgi Bankası makalesinde [Site Recovery Manager 6,5 için işlemsel sınırlara](https://kb.vmware.com/s/article/2147110)göre belgelenmiştir.
2. İş yükü boyutunuzu ve RPO gereksinimlerinizi karşılayacak yeterli ağ bant genişliğine sahip olduğunuzdan emin olun. [VSphere çoğaltması için bant genişliği gereksinimlerinin hesaplanmasındaki](https://docs.vmware.com/en/vSphere-Replication/6.5/com.vmware.vsphere.replication-admin.doc/GUID-4A34D0C9-8CC1-46C4-96FF-3BF7583D3C4F.html) VMware Bilgi Bankası makalesi bant genişliği sınırları hakkında rehberlik sağlar.
3. Şirket içi ortamınızı korumak için DR sitenizde gereken kaynakları tahmin etmek üzere AVS sizer aracını kullanın.

### <a name="create-an-avs-private-cloud-for-your-environment"></a>Ortamınız için bir AVS özel bulutu oluşturun

AVS [özel bulutu oluşturma](create-private-cloud.md)konusundaki yönergeleri ve boyutlandırma önerilerini izleyerek AVS PORTALıNDAN bir AVS özel bulutu oluşturun.

### <a name="set-up-avs-private-cloud-networking-for-the-srm-solution"></a>SRM çözümü için AVS özel bulut ağını ayarlama

Bir SRM çözümü için AVS özel bulut ağını ayarlamak üzere AVS portalına erişin.

SRM çözümü ağı için bir VLAN oluşturun ve bir alt ağ CıDR atayın. Yönergeler için bkz. [VLAN/alt ağlar oluşturma ve yönetme](create-vlan-subnet.md).

### <a name="set-up-a-site-to-site-vpn-connection-between-your-on-premises-network-and-the-avs-private-cloud-and-open-required-ports"></a>Şirket içi ağınız ile AVS özel bulutu arasında siteden siteye VPN bağlantısı kurun ve gerekli bağlantı noktalarını açın

Şirket içi ağınız ve AVS özel bulutunuz arasında siteden siteye bağlantı kurmak için. Yönergeler için bkz. [AVS özel bulutunuzun VPN bağlantısını yapılandırma](set-up-vpn.md).

### <a name="set-up-infrastructure-services-in-your-avs-private-cloud"></a>AVS özel bulutunuzda altyapı hizmetlerini ayarlama

İş yüklerinizi ve araçlarınızı yönetmeyi kolaylaştırmak için AVS özel bulutundaki altyapı hizmetlerini yapılandırın.

Aşağıdakilerden birini yapmak istiyorsanız, [Azure AD 'YI AVS özel bulutu 'Nda vCenter için kimlik sağlayıcısı olarak kullanma](azure-ad.md) bölümünde açıklandığı gibi bir dış kimlik sağlayıcısı ekleyebilirsiniz:

* Şirket içi Active Directory (AD), AVS özel bulutunuzda kullanıcıları belirler.
* Tüm kullanıcılar için AVS özel bulutunuzda bir AD ayarlayın.
* Azure AD 'yi kullanın.

AVS özel bulutundaki iş yükleriniz için IP adresi araması, IP adresi yönetimi ve ad çözümleme hizmetleri sağlamak istiyorsanız, [AVS özel BULUTUNUZDA DNS ve DHCP uygulamaları ve iş yüklerini ayarlama](dns-dhcp-setup.md)bölümünde açıklandığı gıbı bir DHCP ve DNS sunucusu ayarlayın.

*. Cloudsimple.io etki alanı, AVS özel bulutunuzda Yönetim sanal makineleri ve ana bilgisayarlar tarafından kullanılır. Bu etki alanına yönelik istekleri çözümlemek için DNS sunucusunda DNS iletmeyi, [koşullu Iletici oluşturma](on-premises-dns-setup.md#create-a-conditional-forwarder)bölümünde açıklandığı gibi yapılandırın.

### <a name="install-vsphere-replication-appliance-in-your-on-premises-environment"></a>Şirket içi ortamınıza vSphere çoğaltma gereci yüklemesi

VMware belgelerini izleyerek şirket içi ortamınıza vSphere çoğaltma gereci (vRA) yüklemesini yapın. Yükleme bu üst düzey adımlardan oluşur:

1. Şirket içi ortamınızı vRA yüklemesi için hazırlayın.

2. Vmware.com adresinden VR ISO 'daki OVF 'yi kullanarak şirket içi ortamınızda vRA 'yi dağıtın. VRA 6,5 için, [Bu VMware blogu](https://blogs.vmware.com/virtualblocks/2017/01/20/vr-65-ovf-choices) ilgili bilgileri içerir.

3. Şirket içi vRA 'yi, şirket içi sitede vCenter çoklu oturum açma ile kaydedin. VSphere çoğaltma 6,5 hakkında ayrıntılı yönergeler için bkz. VMware Document [VMware vSphere çoğaltma 6,5 yükleme ve yapılandırma](https://docs.vmware.com/en/vSphere-Replication/6.5/vsphere-replication-65-install.pdf).

## <a name="install-vsphere-replication-appliance-in-your-avs-private-cloud-environment"></a>AVS özel bulut ortamınıza vSphere çoğaltma gereci yüklemesi

Başlamadan önce, aşağıdakilere sahip olduğunuzu doğrulayın:

* Şirket içi ortamınızdaki alt ağlardan IP erişimi, AVS özel bulutunuzun yönetim alt ağına ulaşılabilirlik
* Şirket içi vSphere ortamınızdaki çoğaltma alt ağından IP erişimi, AVS özel bulutunuzun SRM çözüm alt ağına ulaşılabilirlik

Yönergeler için bkz. [AVS özel bulutunuzun VPN bağlantısını yapılandırma](set-up-vpn.md). Adımlar, şirket içi yüklemede olanlarla benzerdir.

AVS, vRA ve SRM yüklemesi sırasında IP adresleri yerine FQDN 'lerin kullanılmasını önerir. AVS özel bulutunuzda vCenter ve PSC 'nin FQDN 'sini bulmak için, AVS portalını açın. **Kaynaklar**' a gıdın, AVS özel bulutunuzu seçin ve **vSphere yönetim ağı** sekmesine tıklayın.

![AVS özel bulutu 'nda vCenter/PSC 'nin FQDN 'sini bulma](media/srm-resources.png)

AVS, varsayılan ' cloudowner ' kullanıcısını kullanarak vRA ve SRM yüklememeyi gerektirir, bunun yerine yeni bir kullanıcı oluşturur. Bu işlem, AVS özel bulut vCenter ortamınız için yüksek çalışma süresi ve kullanılabilirlik sağlanmasına yardımcı olmak üzere yapılır. Ancak, AVS özel bulut vCenter 'daki varsayılan cloudowner kullanıcısı, yönetici ayrıcalıklarına sahip yeni bir kullanıcı oluşturmak için yeterli ayrıcalıklara sahip değildir.

VRA ve SRM yüklemeden önce, cloudowner kullanıcısının vCenter ayrıcalıklarını ilerletin ve sonra vCenter SSO etki alanında yönetici ayrıcalıklarına sahip bir kullanıcı oluşturmanız gerekir. Varsayılan AVS özel bulut kullanıcısı ve izin modeli hakkında daha fazla bilgi için bkz. [AVS özel bulut izin modelini öğrenme](learn-private-cloud-permissions.md).

Yükleme bu üst düzey adımlardan oluşur:

1. [Ayrıcalıkları ilerletin](escalate-private-cloud-privileges.md).
2. VSphere çoğaltma ve SRM yüklemesi için AVS özel bulutunuzda bir kullanıcı oluşturun. [VCenter Kullanıcı arabiriminde aşağıda açıklanmıştır: vRA &AMP; SRM yüklemesi IÇIN AVS özel bulutu 'nda bir kullanıcı oluşturun](#vcenter-ui-create-a-user-in-the-avs-private-cloud-for-vra-and-srm-installation).
3. AVS özel bulut ortamınızı vRA yüklemesi için hazırlayın.
4. Vmware.com adresinden VR ISO 'da OVF 'yi kullanarak AVS özel bulutunuzda vRA 'yi dağıtın. VRA 6,5 için, [Bu VMware bloguna](https://blogs.vmware.com/virtualblocks/2017/01/20/vr-65-ovf-choices) ilgili bilgiler vardır.
5. VRA için güvenlik duvarı kurallarını yapılandırın. [AVS portalında aşağıda açıklanmıştır: vRA Için güvenlik duvarı kurallarını yapılandırın](#avs-portal-configure-firewall-rules-for-vra).
6. AVS özel bulut vRA 'yi, AVS özel bulut sitesinde vCenter Single Sign-on ile kaydedin.
7. İki gereç arasında vSphere çoğaltma bağlantılarını yapılandırma. Güvenlik duvarları genelinde gerekli bağlantı noktalarının açıldığından emin olun. VSphere çoğaltma 6,5 için açık olması gereken bağlantı noktası numaraları listesi için [Bu VMware Bilgi Bankası makalesine](https://kb.vmware.com/s/article/2087769) bakın.

VSphere çoğaltma 6,5 için ayrıntılı yükleme yönergeleri için bkz. VMware Document [VMware vSphere replication 6,5 yükleme ve yapılandırma](https://docs.vmware.com/en/vSphere-Replication/6.5/vsphere-replication-65-install.pdf).

#### <a name="vcenter-ui-create-a-user-in-the-avs-private-cloud-for-vra-and-srm-installation"></a>vCenter Kullanıcı arabirimi: AVS özel bulutunda vRA ve SRM yüklemesi için Kullanıcı oluşturma

, AVS portalından ayrıcalıklardan yararlanarak cloudowner Kullanıcı kimlik bilgilerini kullanarak vCenter 'da oturum açın.

VCenter içinde `srm-soln-admin`yeni bir kullanıcı oluşturun ve vCenter 'daki Yöneticiler grubuna ekleyin.
Cloudowner kullanıcısı olarak vCenter oturumunu kapatın ve *SRM-soln-yönetici* kullanıcısı olarak oturum açın.

#### <a name="avs-portal-configure-firewall-rules-for-vra"></a>AVS Portalı: vRA için güvenlik duvarı kurallarını yapılandırma

Aralarında iletişim sağlamak için bağlantı noktalarını açmak üzere [güvenlik duvarı tablolarını ve kuralları ayarlama](firewall.md) bölümünde açıklandığı gibi güvenlik duvarı kurallarını yapılandırın:

* SRM çözümü ağı ve vCenter ve ESXi yönetim ağında bulunan vRA.
* iki sitede vRA gereçleri.

VSphere çoğaltma 6,5 için açık olması gereken bağlantı noktası numaraları listesi için bu [VMware Bilgi Bankası makalesine](https://kb.vmware.com/s/article/2087769) bakın.

### <a name="install-srm-server-in-your-on-premises-environment"></a>Şirket içi ortamınıza SRM sunucusu 'nı yüklemeyin

Başlamadan önce aşağıdakileri doğrulayın:

* vSphere çoğaltma gereci, şirket içi ve AVS özel bulut ortamlarınıza yüklenir.
* Her iki sitedeki vSphere çoğaltma araçları birbirlerine bağlanır.
* Ön koşullar ve en iyi uygulamalar hakkında VMware bilgilerini gözden geçirdiniz. SRM 6,5 için, VMware belge [Önkoşulları ve SRM 6,5 Için En Iyi uygulamalar](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-BB0C03E4-72BE-4C74-96C3-97AC6911B6B8.html)bölümüne başvurabilirsiniz.

Bu [VMware belgesinde](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-F474543A-88C5-4030-BB86-F7CC51DADE22.html)açıklandığı gibi, dağıtım modelinde "platform hizmetleri denetleyicisi başına bir vCenter örneği Ile Iki site topolojisi" olan bir SRM sunucu yüklemesi gerçekleştirmek için VMware belgelerini izleyin. SRM 6,5 yükleme yönergeleri [Site Recovery Manager 'ı yükleyen](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-437E1B65-A17B-4B4B-BA5B-C667C90FA418.html)VMware belgesinde bulunabilir.

### <a name="install-srm-server-in-your-avs-private-cloud"></a>AVS özel bulutunuzda SRM sunucusunu yükler

Başlamadan önce aşağıdakileri doğrulayın:

* vSphere çoğaltma gereci, şirket içi ve AVS özel bulut ortamlarınıza yüklenir.
* Her iki sitedeki vSphere çoğaltma araçları birbirlerine bağlanır.
* Ön koşullar ve en iyi uygulamalar hakkında VMware bilgilerini gözden geçirdiniz. SRM 6,5 için [Site Recovery Manager 6,5 sunucu yüklemesi Için önkoşulları ve En Iyi yöntemlere](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-BB0C03E4-72BE-4C74-96C3-97AC6911B6B8.html)başvurabilirsiniz.

Aşağıdaki adımlarda, AVS özel bulut SRM yüklemesi açıklanır.

1. [vCenter Kullanıcı arabirimi: SRM yüklemesi](#vcenter-ui-install-srm)
2. [AVS Portalı: SRM için güvenlik duvarı kurallarını yapılandırma](#avs-portal-configure-firewall-rules-for-srm)
3. [vCenter Kullanıcı arabirimi: SRM yapılandırma](#vcenter-ui-configure-srm)
4. [AVS Portalı: yükseltme ayrıcalıkları](#avs-portal-de-escalate-privileges)

#### <a name="vcenter-ui-install-srm"></a>vCenter Kullanıcı arabirimi: SRM yüklemesi

SRM-soln-yönetici kimlik bilgilerini kullanarak vCenter 'a oturum açtıktan sonra, bu [VMware belgesinde](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-F474543A-88C5-4030-BB86-F7CC51DADE22.html)açıklandığı gibi dağıtım modelinde ' Iki site topolojisi, platform hizmetleri denetleyicisi başına bir vCenter örneğiyle birlikte bir SRM 6,5 yükleme yönergeleri [Site Recovery Manager 'ı yükleyen](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-437E1B65-A17B-4B4B-BA5B-C667C90FA418.html)VMware belgesinde bulunabilir.

#### <a name="avs-portal-configure-firewall-rules-for-srm"></a>AVS Portalı: SRM için güvenlik duvarı kurallarını yapılandırma

Güvenlik duvarı kurallarını, aralarında iletişime izin vermek için [güvenlik duvarı tabloları ve kuralları ayarlama](firewall.md) bölümünde açıklandığı şekilde yapılandırın:

AVS özel bulutundaki SRM sunucusu ve vCenter/PSC.
Her iki sitedeki SRM sunucuları

VSphere çoğaltma 6,5 için açık olması gereken bağlantı noktası numaraları listesi için [Bu VMware Bilgi Bankası makalesine](https://kb.vmware.com/s/article/2087769) bakın.

#### <a name="vcenter-ui-configure-srm"></a>vCenter Kullanıcı arabirimi: SRM yapılandırma

AVS özel bulutuna SRM yüklendikten sonra, VMware Site Recovery Manager yükleme ve yapılandırma kılavuzunun bölümlerinde açıklandığı gibi aşağıdaki görevleri gerçekleştirin. SRM 6,5 için, yönergeler [Site Recovery Manager 'ı yükleyen](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-437E1B65-A17B-4B4B-BA5B-C667C90FA418.html)VMware belgesinde kullanılabilir.

1. Korunan ve kurtarma sitelerindeki Site Recovery Manager sunucusu örneklerini bağlayın.
2. Uzak Site Recovery Manager sunucu örneğine Istemci bağlantısı kurun.
3. Site Recovery Manager lisans anahtarını yükler.

#### <a name="avs-portal-de-escalate-privileges"></a>AVS Portalı: yükseltme ayrıcalıkları

Ayrıcalıkları devre dışı bırakmak için bkz. [yükseltme ayrıcalıkları](escalate-private-cloud-privileges.md#de-escalate-privileges).

## <a name="ongoing-management-of-your-srm-solution"></a>SRM çözümünüzün devam eden yönetimi

AVS özel bulut ortamınızda vSphere çoğaltma ve SRM yazılımı üzerinde tam denetime sahip olursunuz ve gerekli yazılım yaşam döngüsü yönetimini gerçekleştirmesi beklenir. VSphere çoğaltmasını veya SRM 'i güncelleştirmeden veya yükseltmeden önce yazılımın yeni bir sürümünün AVS özel bulut vCenter ve PSC ile uyumlu olduğundan emin olun.

> [!NOTE]
> AVS, şu anda yönetilen bir DR hizmeti sunma seçeneklerini araştırmakta. 

## <a name="multiple-replication-configuration"></a>Birden çok çoğaltma yapılandırması

 [Dizi tabanlı çoğaltma ve vSphere çoğaltma teknolojileri aynı anda SRM ile birlikte kullanılabilir](https://blogs.vmware.com/virtualblocks/2017/06/22/srm-array-based-replication-vs-vsphere-replication) . Ancak, bunların ayrı bir VM kümesine uygulanmaları gerekir (belirli bir VM, dizi tabanlı çoğaltma veya vSphere çoğaltması tarafından korunabilir, ancak her ikisi birden değil). Ayrıca, AVS sitesi birden çok korumalı site için bir kurtarma sitesi olarak yapılandırılabilir. Birden çok site yapılandırması hakkında bilgi için bkz. [SRM çok siteli seçenekler](https://blogs.vmware.com/virtualblocks/2016/07/28/srm-multisite/) .

## <a name="references"></a>Başvurular

* [VMware Site Recovery Manager belgeleri](https://docs.vmware.com/en/Site-Recovery-Manager/index.html)
* [Site Recovery Manager 6,5 için işlemsel sınırlar](https://kb.vmware.com/s/article/2147110)
* [VSphere çoğaltması için bant genişliği gereksinimleri hesaplanıyor](https://kb.vmware.com/s/article/2037268)
* [VSphere çoğaltma 6,5 ' i dağıttığınızda OVF seçimleri](https://blogs.vmware.com/virtualblocks/2017/01/20/vr-65-ovf-choices/)
* [VMware vSphere çoğaltma 6,5 yükleme ve yapılandırma](https://docs.vmware.com/en/vSphere-Replication/6.5/vsphere-replication-65-install.pdf)
* [SRM 6,5 için Önkoşullar ve En Iyi uygulamalar](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-BB0C03E4-72BE-4C74-96C3-97AC6911B6B8.html)
* [Platform Hizmetleri denetleyicisi başına bir vCenter Server örneği ile Iki siteli topolojide Site Recovery Manager](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-F474543A-88C5-4030-BB86-F7CC51DADE22.html)
* [VMware Site Recovery Manager 6,5 yükleme ve yapılandırma kılavuzu](https://docs.vmware.com/en/Site-Recovery-Manager/6.5/com.vmware.srm.install_config.doc/GUID-437E1B65-A17B-4B4B-BA5B-C667C90FA418.html)
* [Dizi tabanlı çoğaltma ve vSphere çoğaltma ile SRM üzerinde VMware blogu](https://blogs.vmware.com/virtualblocks/2017/06/22/srm-array-based-replication-vs-vsphere-replication)
* [SRM çoklu site seçeneklerinde VMware blogu](https://blogs.vmware.com/virtualblocks/2016/07/28/srm-multisite)
* [VSphere çoğaltması 5,8. x, 6. x ve 8 için açık olması gereken bağlantı noktası numaraları](https://kb.vmware.com/s/article/2147112)
