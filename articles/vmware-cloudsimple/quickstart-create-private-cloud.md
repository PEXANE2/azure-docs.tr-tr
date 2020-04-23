---
title: 'Hızlı Başlangıç: Özel Bulut Oluşturma'
titleSuffix: Azure VMware Solutions by CloudSimple
description: CloudSimple ile Azure VMware Çözümleri ile Özel Bulut'u nasıl oluşturup yapılandırılamayı öğrenin
author: sharaths-cs
ms.author: dikamath
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: a2fe7daf1476f19f6a6eea83174342fa1273fe14
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81867972"
---
# <a name="quickstart---configure-a-private-cloud-environment"></a>Quickstart - Özel Bulut ortamını yapılandırma

Bu makalede, CloudSimple Private Cloud'u nasıl oluşturup Özel Bulut ortamınızı nasıl kurabilirsiniz öğrenin.

## <a name="before-you-begin"></a>Başlamadan önce

Ağ Önkoşulları Gözden [Geçirin.](cloudsimple-network-checklist.md)

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-private-cloud"></a>Özel Bulut oluşturma

Özel Bulut, ESXi ana bilgisayarları, vCenter, vSAN ve NSX'i destekleyen yalıtılmış bir VMware yığınıdır.

Özel Bulutlar CloudSimple portalı üzerinden yönetilir. Kendi yönetim etki alanında kendi vCenter sunucuları vardır. Yığın özel düğümler ve yalıtılmış çıplak metal donanım düğümleri üzerinde çalışır.

1. **Tüm Hizmetler**’i seçin.
2. **CloudSimple Services'ı**arayın.
3. Özel Bulut'unuzu oluşturmak istediğiniz CloudSimple hizmetini seçin.
4. **Genel Bakış'tan**CloudSimple portalı için yeni bir tarayıcı sekmesi açmak için Özel **Bulut Oluştur'u** tıklatın.  İstenirse, Azure oturum açınızın kimlik bilgileriyle oturum açın.  

    ![Azure'dan Özel Bulut Oluşturma](media/create-private-cloud-from-azure.png)

5. CloudSimple portalında, Özel Bulut'unuzun adını belirtin.
6. Özel Bulutunuzun **Konumunu** seçin.
7. **Azure'da**sağlananlarla tutarlı düğüm türünü seçin.
8. **Düğüm sayısını**belirtin.  Özel Bulut oluşturmak için en az üç düğüm gerekir.

    ![Özel Bulut Oluşturma - Temel bilgiler](media/create-private-cloud-basic-info.png)

9. **İleri'yi tıklatın: Gelişmiş seçenekler.**
10. vSphere/vSAN alt ağları için CIDR aralığını girin. CIDR aralığının şirket içi veya diğer Azure alt ağlarınızın (sanal ağlar) veya ağ geçidi alt ağıyla örtüşmediğinden emin olun.

    **CIDR aralığı seçenekleri:** /24, /23, /22 veya /21. Bir /24 CIDR aralığı 26 düğüme kadar destekler, bir /23 CIDR aralığı 58 düğüme kadar destekler ve /22 ve /21 CIDR aralığı 64 düğümü destekler (Özel Buluttaki maksimum düğüm sayısı).  Daha fazla bilgi edinmek ve VUN'lar ve alt ağlar için [VUN'lara ve alt ağlara genel bakışa](cloudsimple-vlans-subnets.md)bakın.

      > [!IMPORTANT]
      > vSphere/vSAN CIDR serisindeki IP adresleri Özel Bulut altyapısı tarafından kullanılmak üzere ayrılmıştır.  Bu aralıktaki IP adresini herhangi bir sanal makinede kullanmayın.

11. **İleri'yi tıklatın: Gözden geçirin ve oluşturun.**
12. Ayarları gözden geçirin. Herhangi bir ayarı değiştirmeniz gerekiyorsa, **Önceki'yi**tıklatın.
13. **Oluştur'u**tıklatın.

Özel Bulut sağlama işlemi başlar.  Özel Bulut'un sağlanması iki saat kadar sürebilir.

## <a name="launch-cloudsimple-portal"></a>CloudSimple portalına başlat

CloudSimple portalına Azure portalından erişebilirsiniz.  CloudSimple portalı, Tek Oturum Açma (SSO) kullanılarak Azure oturumunuzda oturum açar.  CloudSimple portalına erişmek için **CloudSimple Hizmet Yetkilendirme** uygulamasına yetki vermeniz gerekmektedir.  İzin verme hakkında daha fazla bilgi için [bkz.](access-cloudsimple-portal.md#consent-to-cloudsimple-service-authorization-application)

1. **Tüm Hizmetler**’i seçin.
2. **CloudSimple Services'ı**arayın.
3. Özel Bulut'unuzu oluşturmak istediğiniz CloudSimple hizmetini seçin.
4. Genel bakışta, CloudSimple portalı için yeni bir tarayıcı sekmesi açmak için **CloudSimple portalına git'i** tıklatın.  İstenirse, Azure oturum açınızın kimlik bilgileriyle oturum açın.  

    ![CloudSimple portalına başlat](media/launch-cloudsimple-portal.png)

## <a name="create-point-to-site-vpn"></a>Noktadan Siteye VPN Oluşturma

Bir Noktadan Siteye VPN bağlantısı bilgisayarınızdan Özel Bulut bağlanmak için en basit yoludur. Özel Bulut'a uzaktan bağlanıyorsanız, Site'ye Noktalı VPN bağlantısını kullanın.  Özel Bulut'unuza hızlı erişim için aşağıdaki adımları izleyin.  Şirket içi ağınızdan CloudSimple bölgesine erişim [Siteden Siteye VPN](vpn-gateway.md) veya Azure [ExpressRoute](on-premises-connection.md)kullanılarak yapılabilir.

### <a name="create-gateway"></a>Ağ geçidi oluşturma

1. CloudSimple portalını başlatın ve **Ağ'ı**seçin.
2. **VPN Ağ Geçidi'ni**seçin.
3. **Yeni VPN Ağ Geçidi'ni**tıklatın.

    ![VPN ağ geçidi oluşturma](media/create-vpn-gateway.png)

4. **Ağ Geçidi yapılandırması**için aşağıdaki ayarları belirtin ve **İleri'yi**tıklatın.

    * Ağ geçidi türü olarak **Site'ye Nokta VPN'i** seçin.
    * Ağ geçidini tanımlamak için bir ad girin.
    * CloudSimple hizmetinizin dağıtıldığı Azure konumunu seçin.
    * Siteye Giriş ağ geçidi için istemci alt netini belirtin.  Bağlandığınızda BU alt ağdan DHCP adresleri verilecektir.

5. **Bağlantı/Kullanıcı**için aşağıdaki ayarları belirtin ve **İleri'yi**tıklatın.

    * Tüm mevcut ve gelecekteki kullanıcıların Bu Siteye Noktadan Bağımsız Ağ Geçidi'nden Özel Bulut'a otomatik olarak erişmesine izin vermek için, **tüm kullanıcıları otomatik olarak ekleyin'i**seçin. Bu seçeneği seçtiğinizde, Kullanıcı listesindeki tüm kullanıcılar otomatik olarak seçilir. Listedeki tek tek kullanıcıları seçerek otomatik seçeneği geçersiz kılabilirsiniz.
    * Yalnızca tek tek kullanıcıları seçmek için, Kullanıcı listesindeki onay kutularını tıklatın.

6. VN'ler/Alt Ağlar bölümü, ağ geçidi ve bağlantılar için yönetim ve kullanıcı VLAN'leri/alt ağlarını belirtmenize olanak tanır.

    * **Otomatik olarak ekleme** seçenekleri bu ağ geçidi için genel ilkeyi ayarlar. Ayarlar geçerli ağ geçidine uygulanır. Ayarlar **Seç** alanında geçersiz kılınabilir.
    * **Özel Bulutların Yönetim VLAN'leri/Alt Ağlarını Ekle'yi**seçin.
    * Tüm kullanıcı tanımlı VLAN'lar/alt ağlar eklemek **için, kullanıcı tanımlı VLAN/Subnet ekle'yi**tıklatın.
    * **Select** **ayarları, Otomatik olarak ekle'nin**altındaki genel ayarları geçersiz kılar.

7. Ayarları gözden geçirmek için **İleri'yi** tıklatın. Değişiklik yapmak için Simgeleri Edit'i tıklatın.
8. VPN ağ geçidini oluşturmak için **Oluştur'u** tıklatın.

### <a name="connect-to-cloudsimple-using-point-to-site-vpn"></a>Siteye Göre VPN kullanarak CloudSimple'a bağlanın

Bilgisayarınızdan CloudSimple'a bağlanmak için VPN istemcisi gereklidir.  macOS ve OS X için Windows veya [Viskozite](https://www.sparklabs.com/viscosity/download/) için [OpenVPN istemcisini](https://openvpn.net/community-downloads/) indirin.

1. CloudSimple portalını başlatın ve **Ağ'ı**seçin.
2. **VPN Ağ Geçidi'ni**seçin.
3. VPN ağ geçitleri listesinden, Siteye Giriş VPN ağ geçidini tıklatın.
4. **Kullanıcılar**’ı seçin.
5. **VPN yapılandırmamı indir'e**tıklayın.

    ![VPN yapılandırmasını indirme](media/download-p2s-vpn-configuration.png)

6. Yapılandırmayı VPN istemcinizde aktarın.

    * Windows [istemcisi üzerinde yapılandırma alma](https://openvpn.net/vpn-server-resources/connecting-to-access-server-with-windows/#openvpn-open-source-openvpn-gui-program) yönergeleri
    * [macOS veya OS X'te yapılandırma alma](https://www.sparklabs.com/support/kb/article/getting-started-with-viscosity-mac/#creating-your-first-connection) yönergeleri

7. CloudSimple'a bağlanın.

## <a name="create-a-vlan-for-your-workload-vms"></a>İş yükünüz için VLAN oluşturma

Özel Bulut oluşturduktan sonra, iş yükünüzü/uygulama VM'lerinizi dağıtacağınız bir VLAN oluşturun.

1. CloudSimple portalında **Ağ'ı**seçin.
2. **VLAN/Alt Ağlar'ı**tıklatın.
3. **VLAN/Subnet Oluştur'u**tıklatın.

    ![VLAN/Subnet oluşturma](media/create-new-vlan-subnet.png)

4. Yeni VLAN/alt net için **Özel Bulut'u** seçin.
5. Listeden bir VLAN Kimliği seçin.  
6. Alt ağı tanımlamak için bir alt ağ adı girin.
7. Alt ağ CIDR aralığını ve maskesini belirtin.  Bu aralık varolan alt ağlarla çakışmamalıdır.
8. **Gönder'i**tıklatın.

    ![VLAN/Subnet ayrıntıları oluşturma](media/create-new-vlan-subnet-details.png)

VLAN/alt ağ oluşturulacak.  Artık bu VLAN Kimliğini, Özel Bulut vCenter'ınızda dağıtılmış bir bağlantı noktası grubu oluşturmak için kullanabilirsiniz.

## <a name="connect-your-environment-to-an-azure-virtual-network"></a>Ortamınızı azure sanal ağına bağlayın

CloudSimple, Özel Bulut'unuzun bir ExpressRoute devresi sağlar. Azure'daki sanal ağınızı ExpressRoute devresine bağlayabilirsiniz. Bağlantıyı kurma yla ilgili tüm ayrıntılar için [ExpressRoute'u kullanarak Azure Sanal Ağ Bağlantısı'ndaki](https://docs.microsoft.com/azure/vmware-cloudsimple/cloudsimple-azure-network-connection)adımları izleyin.

## <a name="sign-in-to-vcenter"></a>vCenter'da oturum açın

Artık sanal makineler ve ilkeler ayarlamak için vCenter'da oturum açabilirsiniz.

1. vCenter'a erişmek için CloudSimple portalından başlayın. Ana sayfada, **Ortak Görevler**altında, **başlat vSphere İstemci'yi**tıklatın.  Özel Bulut'u seçin ve ardından Özel Bulut'ta **VSphere İstemci** Başlat'ı tıklatın.

    ![VSphere İstemciyi Başlat](media/launch-vcenter-from-cloudsimple-portal.png)

2. vCenter'a erişmek ve kullanıcı adınız ve şifrenizle oturum açın.  Varsayılan lar şunlardır:
    * Kullanıcı adı:`CloudOwner@cloudsimple.local`
    * Parola: `CloudSimple123!`

Sonraki yordamlarda vCenter ekranları vSphere (HTML5) istemcisinden.

## <a name="change-your-vcenter-password"></a>vCenter parolanızı değiştirme

CloudSimple, vCenter'da ilk oturum açtığınızda parolanızı değiştirmenizi önerir.  
Belirlediğiniz parola aşağıdaki gereksinimleri karşılamalıdır:

* Maksimum kullanım ömrü: Parola her 365 günde bir değiştirilmelidir
* Yeniden kullanımı kısıtlama: Kullanıcılar önceki beş parolanın hiçbirini yeniden kullanamaz
* Uzunluk: 8 - 20 karakter
* Özel karakter: En az bir özel karakter
* Alfabetik karakterler: En az bir büyük harf karakter, A-Z ve en az bir küçük karakter, a-z
* Sayılar: En az bir sayısal karakter, 0-9
* Maksimum özdeş bitişik karakterler: Üç

    Örnek: CC veya CCC parolanın bir parçası olarak kabul edilebilir, ancak CCCC değildir.

Gereksinimleri karşılamayan bir parola ayarlarsanız:

* vSphere Flash İstemciyi kullanırsanız, bir hata bildirir
* HTML5 istemcisini kullanıyorsanız, bir hata bildirmez. İstemci değişikliği kabul etmez ve eski parola çalışmaya devam ediyor.

## <a name="access-nsx-manager"></a>NSX yöneticisine erişin

NSX yöneticisi varsayılan bir parolayla dağıtılır. 

* Kullanıcı adı: **admin**
* Şifre: **CloudSimple123!**

CloudSimple portalında NSX yöneticisinin tam nitelikli alan adını (FQDN) ve IP adresini bulabilirsiniz.

1. CloudSimple portalını başlatın ve **Kaynakları**seçin.
2. Kullanmak istediğiniz Özel Bulut'u tıklatın.
3. **vSphere yönetim ağını** seçin
4. **NSX Manager'ın** FQDN veya IP adresini kullanın ve bir web tarayıcısı kullanarak bağlanın.

    ![NSX Yöneticisi FQDN bul](media/private-cloud-nsx-manager-fqdn.png)

## <a name="create-a-port-group"></a>Bağlantı noktası grubu oluşturma

vSphere'de dağıtılmış bir bağlantı noktası grubu oluşturmak için:

1. [vSphere Ağ Kılavuzu'ndaki](https://docs.vmware.com/en/VMware-vSphere/6.5/vsphere-esxi-vcenter-server-65-networking-guide.pdf)"Dağıtılmış bağlantı noktası grubu ekle" yönergelerini izleyin.
2. Dağıtılmış bağlantı noktası grubunu ayarlarken, İş [Yükü VM'leriniz için VLAN Oluştur'da](#create-a-vlan-for-your-workload-vms)oluşturulan VLAN Kimliğini sağlayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da VMware sanal makinelerini kullanma](quickstart-create-vmware-virtual-machine.md)
* [Azure ExpressRoute'u kullanarak şirket içi ağa bağlanma](on-premises-connection.md)
* [Şirket içinde Siteden Siteye VPN'i ayarlama](vpn-gateway.md)
