---
title: CloudSimple tarafından Azure VMware Çözümü - VMware Horizon kullanarak sanal bir masaüstü altyapısı barındırmak için Özel Bulut sitesini kullanın
description: VMware Horizon'u kullanarak sanal bir masaüstü altyapısıbarındırmak için CloudSimple Private Cloud sitenizi nasıl kullanabileceğinizi açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 723821a78ecae308443c93567402e3b232c036f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025257"
---
# <a name="use-cloudsimple-private-cloud-site-to-host-a-virtual-desktop-infrastructure-using-vmware-horizon"></a>VMware Horizon'ı kullanarak sanal bir masaüstü altyapısıbarındırmak için CloudSimple Private Cloud sitesini kullanın

CloudSimple Private Cloud sitenizi VMware Horizon 7.x'i kullanarak sanal bir masaüstü altyapısını (VDI) barındırmak için kullanabilirsiniz. Aşağıdaki şekil VDI için mantıksal çözüm mimarisini gösterir.

![Ufuk dağıtımı](media/horizon-deployment.png)

Bu çözümle Horizon View Manager ve Uygulama Hacmi üzerinde tam kontrole sahip siniz. Tanıdık UI, API ve CLI arabirimleri, varolan komut dosyalarınızın ve araçlarınızın kullanımını sağlar.

CloudSimple çözümü aşağıdakileri yapmanızgerektiğini gerektirir:

* Özel Bulut'unuzda VMware Horizon 7.x'i yükleyin, yapılandırın ve yönetin.
* Kendi Horizon lisanslarınızı sağlayın.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Aşağıdaki bölümlerde, Özel Bulutunuzda Horizon'u kullanarak bir VDI çözümünüzün nasıl dağıtılanılasınız açıklanmıştır.

1. [VMware ürün sürümlerinin uyumlu olduğunu doğrulayın](#verify-that-vmware-product-versions-are-compatible)
2. [Masaüstü ortamınızın boyutunu tahmin edin](#estimate-the-size-of-your-desktop-environment)
3. [Ortamınız için Özel Bulut Oluşturma](#create-a-private-cloud-for-your-environment)
4. [Özel Bulutunuzda VMware Horizon'u yükleyin](#install-vmware-horizon-in-your-private-cloud)

### <a name="verify-that-vmware-product-versions-are-compatible"></a>VMware ürün sürümlerinin uyumlu olduğunu doğrulayın

* Horizon, App Volumes, Unified Access Gateway ve User Environment Manager'ın mevcut ve planlı sürümlerinin birbiriyle ve Özel Bulut'taki vCenter ve PSC ile uyumlu olduğunu doğrulayın. Uyumluluk bilgileri için [Horizon 7.5 için VMware Uyumluluk Matrisi'ne](https://www.vmware.com/resources/compatibility/sim/interop_matrix.php#interop&260=2877&0=)bakın.
* Özel Bulut'unuzda vCenter ve PSC'nin geçerli sürümlerini öğrenmek için [CloudSimple portalındaki](access-cloudsimple-portal.md) **Kaynaklar** portalına gidin, Özel Bulut'unuzu seçin ve **vSphere Management Network** sekmesini tıklatın.

![vCenter ve PSC sürümleri](media/private-cloud-vsphere-versions.png)

### <a name="estimate-the-size-of-your-desktop-environment"></a>Masaüstü ortamınızın boyutunu tahmin edin

* Tanımlanan yapılandırmanızın VMware işletim sınırları içinde olduğunu doğrulayın.
* Tüm masaüstü bilgisayarlarınız ve Horizon yönetim bileşenleriniz için gereken kaynakları tahmin edin.

### <a name="create-a-private-cloud-for-your-environment"></a>Ortamınız için Özel Bulut Oluşturma

1. [Özel Bulut ortamını Yapılandır'daki](quickstart-create-private-cloud.md)yönergeleri izleyerek CloudSimple portalından özel bulut oluşturun.  CloudSimple, yeni oluşturulan her Özel Bulut'ta 'cloudowner' adında varsayılan bir vCenter kullanıcısı oluşturur. Varsayılan Özel Bulut kullanıcısı ve izin modeliyle ilgili ayrıntılar [için](learn-private-cloud-permissions.md)bkz.
2. Horizon yönetim düzlemi için Özel Bulut'unuzda bir VLAN oluşturun ve bir alt ağ CIDR atayın. Talimatlar için [VLANs/Subnets oluştur ve yönetme'ye](create-vlan-subnet.md)bakın. Bu ağ, tüm çözüm bileşenlerinin (Unified Access Gateway, Connection Server, App Volume Server ve User Environment Manager sunucuları) yüklendiği ağdır.
3. Private Cloud vCenter'ınızla harici bir kimlik sağlayıcısı kullanmak isteyip istemediğinize karar verin. Evet ise, şu seçeneklerden birini seçin:
    * Şirket içi Active Directory'nizi dış kimlik sağlayıcısı olarak kullanın. Talimatlar için [vCenter Kimlik Kaynakları'na](set-vcenter-identity.md)bakın.
    * Harici kimlik sağlayıcınız olarak kullanmak üzere Horizon yönetim düzlemi VLAN'da Özel Bulut'ta bir Active Directory sunucusu ayarlayın. Talimatlar için [vCenter Kimlik Kaynakları'na](set-vcenter-identity.md)bakın.
    * Horizon yönetim düzlemi VLAN'da Özel Bulut'ta bir DHCP ve DNS sunucusu ayarlayın. Talimatlar için bkz. [CloudSimple Private Cloud'unuzda DNS ve DHCP uygulamalarını ve iş yüklerini ayarla.](dns-dhcp-setup.md)
4. Özel Bulut'ta yüklü olan DNS sunucusunda DNS iletmesini yapılandırın. Yönergeler için [bkz.](on-premises-dns-setup.md#create-a-conditional-forwarder)

### <a name="install-vmware-horizon-in-your-private-cloud"></a>Özel Bulutunuzda VMware Horizon'u yükleyin

Aşağıdaki dağıtım diyagramı, Özel Bulut'ta dağıtılan bir Horizon çözümlerini görüntüler. Birleşik Erişim Ağ Geçidi, AD/DC, Görünüm ve App Volume Server, kullanıcı tarafından oluşturulan VLAN 234'e yüklenir. Unified Access Gateway Internet'ten erişilebilen atanmış bir genel IP adresine sahiptir. Horizon masaüstü havuzu VM'leri, ek yalıtım ve güvenlik sağlamak için VLAN 235'te dağıtılır.

![Özel Bulut'ta Ufuk dağıtımı](media/horizon-private-cloud.png)

Aşağıdaki bölümlerde, şekilde gösterilene benzer bir dağıtım ayarlama yönergeleri sıralanır. Başlamadan önce aşağıdakileri yaptığınızı doğrulayın:

* Masaüstü havuzlarınızı çalıştırmak için yeterli kapasiteye sahip CloudSimple portalı kullanılarak oluşturulan Özel Bulut.
* Masaüstülerinizin ağ trafiğini desteklemek için şirket ortamınız ile Özel Bulut ortamı arasında yeterli bant genişliği.
* Şirket içi veri merkeziniz ile Özel Bulut arasında siteden siteye VPN tüneli kuruldu.
* Şirket içi ortamınızdaki son kullanıcı alt ağlarından CloudSimple Private Cloud alt ağlarına IP erişimi.
* Özel Bulut'unuzun için AD/DHCP/DNS yüklendi.

#### <a name="cloudsimple-portal-create-a-dedicated-vlansubnet-for-desktop-pools"></a>CloudSimple portalı: Masaüstü havuzları için özel bir VLAN/subnet oluşturun

Horizon masaüstü havuzları için bir VLAN oluşturun ve bir alt ağ CIDR atayın. Talimatlar için [VLANs/Subnets oluştur ve yönetme'ye](create-vlan-subnet.md)bakın. Bu, tüm masaüstü sanal makinelerin çalışacağı ağdır.

Horizon dağıtımınızı güvence altına almak için standart güvenlik en iyi uygulamalarını izleyin:

* Masaüstü VM'lerinize yalnızca masaüstü RDP trafiğine / SSH trafiğine izin verin.
* Horizon yönetim düzlemi VLAN ve masaüstü havuzu VLAN arasında yalnızca yönetim trafiğine izin verin.
* Şirket içi ağdan yalnızca yönetim trafiğine izin verin.

CloudSimple portalından güvenlik duvarı [kurallarını](firewall.md) yapılandırarak bu en iyi uygulamaları uygulayabilirsiniz.

#### <a name="cloudsimple-portal-configure-firewall-rules-to-secure-horizon-management-plane"></a>CloudSimple portalı: Horizon yönetim düzlemini güvenli hale getirmek için güvenlik duvarı kurallarını yapılandırın

CloudSimple portalında aşağıdaki kuralları ayarlayın. Yönergeler için [bkz.](firewall.md)

1. CloudSimple N-S güvenlik duvarındaki güvenlik duvarı kurallarını, yalnızca VMware belge [Horizon bağlantı noktası listesinde](https://docs.vmware.com/en/VMware-Horizon-7/7.1/com.vmware.horizon-client-agent.security.doc/GUID-52807839-6BB0-4727-A9C7-EA73DE61ADAB.html) listelenen ağ bağlantı noktalarına izin verebilmek için şirket içi alt ağlar ve Horizon yönetimi VLAN arasındaki iletişime izin verecek şekilde yapılandırın.

2. Horizon yönetimi VLAN ile Özel Bulut'taki masaüstü havuzu VLAN arasında E-W güvenlik duvarı kuralları oluşturun.

#### <a name="cloudsimple-portal-create-a-public-ip-address-for-unified-access-gateway"></a>CloudSimple portalı: Unified Access Gateway için genel bir IP adresi oluşturma

Internet'ten masaüstü istemci bağlantılarını etkinleştirmek için Birleştirilmiş Erişim Ağ Geçidi cihazı için genel bir IP adresi oluşturun. Yönergeler için [bkz.](public-ips.md)

Kurulum tamamlandığında, genel IP adresi atanır ve Genel IP'ler sayfasında listelenir.

#### <a name="cloudsimple-portal-escalate-privileges"></a>CloudSimple portalı: Ayrıcalıkları artırın

Varsayılan 'bulut sahibi' kullanıcısı, Horizon'u yüklemek için Private Cloud vCenter'da yeterli ayrıcalıklara sahip değildir, bu nedenle kullanıcının vCenter ayrıcalıklarının yükseltilmesi gerekir. Daha fazla bilgi için [bkz.](escalate-private-cloud-privileges.md)

#### <a name="vcenter-ui-create-a-user-in-private-cloud-for-horizon-installation"></a>vCenter UI: Horizon kurulumu için Private Cloud'da bir kullanıcı oluşturma

1. 'Cloudowner' kullanıcı kimlik bilgilerini kullanarak vCenter'da oturum açın.
2. vCenter'da yeni bir kullanıcı olan 'horizon-soln-admin' oluşturun ve kullanıcıyı vCenter'daki yönetici grubuna ekleyin.
3. vCenter'ı 'bulut sahibi' kullanıcısı olarak kaydedin ve 'horizon-soln-admin' kullanıcısı olarak oturum açın.

#### <a name="vcenter-ui-install-vmware-horizon"></a>vCenter UI: VMware Horizon'ı yükleyin

Önceki mantıksal mimari bölümünde belirtildiği gibi, Horizon çözüm aşağıdaki bileşenleri vardır:

* VMware Horizon Görünümü
* VMware Birleşik Erişim Ağ Geçidi
* VMware Uygulama Ses Yöneticisi
* VMware Kullanıcı Ortamı Yöneticisi

Bileşenleri aşağıdaki gibi yükleyin:

1. VMware belgesini dağıtma ve yapılandırma da verilen yönergeleri izleyerek Birleşik Erişim Ağ Geçidi'ni yükleyin [ve yapılandırın.](https://docs.vmware.com/en/Unified-Access-Gateway/3.3.1/com.vmware.uag-331-deploy-config.doc/GUID-F5CE0D5E-BE85-4FA5-BBCF-0F86C9AB8A70.html)

2. [Yükleme Kılavuzu'ndaki](https://docs.vmware.com/en/VMware-Horizon-7/7.4/horizon-installation/GUID-37D39B4F-5870-4188-8B11-B6C41AE9133C.html)yönergeleri izleyerek Horizon Görünümü Özel Bulut'a yükleyin.

3. [VMware Uygulama Birimlerini Yükle ve Yapılandırma](https://docs.vmware.com/en/VMware-App-Volumes/2.10/com.vmware.appvolumes.user.doc/GUID-5E8BAF8C-F5A6-412C-9424-266BA7109BA4.html)yönergelerini izleyerek Uygulama Ses Yöneticisi'ni yükleyin.

4. [VMware Kullanıcı Ortamı Yöneticisi'ni Yükleme ve Yapılandırma Hakkında'daki](https://docs.vmware.com/en/VMware-User-Environment-Manager/9.4/com.vmware.user.environment.manager-install-config/GUID-DBBC82E4-483F-4B28-9D49-4D28E08715BC.html)yönergeleri izleyerek Kullanıcı Ortamı Yöneticisi'ni yükleyin ve yapılandırın.

#### <a name="file-a-support-request-to-upload-vmware-horizon-pre-packaged-app-volumes"></a>VMware Horizon önceden paketlenmiş uygulama birimlerini yüklemek için bir destek isteği dosyala

Yükleme işleminin bir parçası olarak, App Volume Manager uygulama yığınları ve yazılabilir birimleri sağlamak için önceden paketlenmiş birimleri kullanır. Bu birimler, uygulama yığınları ve yazılabilir birimler için şablon görevi görehizmet eder.

Birimleri Private Cloud veri deposuna yüklemek için ESXi kök parolası gereklidir. Yardım için bir [destek isteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)gönderin. CloudSimple destek personelinin şablonları Özel Bulut ortamınıza yükleyebilmeleri için AppVolumes yükleyici paketini ekleyin.

#### <a name="cloudsimple-portal-de-escalate-privileges"></a>CloudSimple portalı: Ayrıcalıkları yükseltme

Artık 'bulut sahibi' [kullanıcısının ayrıcalıklarını artırabilirsiniz.](escalate-private-cloud-privileges.md#de-escalate-privileges)

## <a name="ongoing-management-of-your-horizon-solution"></a>Horizon çözümünüzün sürekli yönetimi

Özel Bulut ortamınızda Horizon ve App Volume Manager yazılımı üzerinde tam kontrole sahipsiniz ve gerekli yazılım yaşam döngüsü yönetimini gerçekleştirmeniz beklenir. Horizon veya App Volume'i güncellemeden veya güncellemeden önce yazılımın yeni sürümlerinin Private Cloud vCenter ve PSC ile uyumlu olduğundan emin olun.
