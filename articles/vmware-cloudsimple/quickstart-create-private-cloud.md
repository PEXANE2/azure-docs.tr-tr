---
title: CloudSimple hızlı başlangıç ile Azure VMware çözümü-özel bir bulut oluşturma
description: CloudSimple ile Azure VMware çözümü ile özel bir bulut oluşturma ve yapılandırma hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 6b68dcd47377ee56c4ebedc94905e1f0a8b70b38
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812345"
---
# <a name="quickstart---configure-a-private-cloud-environment"></a>Hızlı başlangıç-özel bir bulut ortamı yapılandırma

Bu makalede, CloudSimple özel bulutu oluşturmayı ve özel bulut ortamınızı ayarlamayı öğrenin.

## <a name="before-you-begin"></a>Başlamadan önce

Özel bulut için vSphere/vSAN alt ağları için bir CıDR aralığı ayırın. Özel bulut, bir vCenter sunucusu tarafından yönetilen yalıtılmış bir VMware yığını (ESXi Konakları, vCenter, vSAN ve NSX) ortamı olarak oluşturulur. Yönetim bileşenleri, vSphere/vSAN alt ağları CıDR için seçilen ağda dağıtılır. Ağ CıDR aralığı, dağıtım sırasında farklı alt ağlara bölünmüştür.  VSphere/vSAN alt ağ adresi alanı benzersiz olmalıdır. CloudSimple ortamıyla iletişim kuran herhangi bir ağla çakışmamalıdır.  CloudSimple ile iletişim kuran ağlarda şirket içi ağlar ve Azure sanal ağları bulunur.  VSphere/vSAN alt ağları hakkında daha fazla bilgi için bkz. [VLAN ve alt ağlara genel bakış](cloudsimple-vlans-subnets.md).

* Minimum vSphere/vSAN alt ağları CıDR aralığı ön eki:/24 
* Maksimum vSphere/vSAN alt ağları CıDR aralığı ön eki:/21

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-private-cloud"></a>Özel bulut oluşturma

1. **Tüm Hizmetler**’i seçin.
2. **Cloudsimple Hizmetleri**için arama yapın.
3. Özel bulutunuzu oluşturmak istediğiniz CloudSimple hizmetini seçin.
4. **Özel bulut oluştur** ' a tıklayarak cloudsimple portalı için yeni bir tarayıcı sekmesi açın.  İstenirse, Azure oturum açma kimlik bilgilerinizle oturum açın.  

    ![Azure 'dan özel bulut oluşturma](media/create-private-cloud-from-azure.png)

5. CloudSimple Portal 'da, özel bulutunuz için bir ad sağlayın
6. Özel bulutunuzun **konumunu** seçin
7. Azure 'da sağladığınız **düğüm türünü** seçin.  [CS28 veya CS36 seçeneğini](cloudsimple-node.md#vmware-solution-by-cloudsimple-nodes-sku)belirleyebilirsiniz. İkinci seçenek, en yüksek işlem ve bellek kapasitesini içerir.
8. **Düğüm sayısını**belirtin.  Özel bir bulut oluşturmak için en az üç düğüm gerekir

    ![Özel bulut oluşturma-temel bilgi](media/create-private-cloud-basic-info.png)

9. İleri **' ye tıklayın: Gelişmiş Seçenekler**.
10. VSphere/vSAN alt ağları için CıDR aralığını girin. CıDR aralığının, şirket içi veya diğer Azure alt ağlarınız ile çakışmadığından emin olun.

    ![Özel bulut oluşturma-Gelişmiş Seçenekler](media/create-private-cloud-advanced-options.png)

11. İleri **' yi seçin: Gözden geçirin ve**oluşturun.
12. Ayarları gözden geçirin. Herhangi bir ayarı değiştirmeniz gerekiyorsa, **önceki**' ye tıklayın.
13.           **Oluştur**'a tıklayın.

Özel bulut sağlama işlemi başlatılacak.  Özel bulutun sağlanması iki saate kadar sürebilir.

## <a name="launch-cloudsimple-portal"></a>CloudSimple portalını Başlat

Azure portal 'ten CloudSimple portalına erişebilirsiniz.  CloudSimple Portal, çoklu oturum açma (SSO) ile Azure oturum açma kimlik bilgilerinizle başlatılır.  CloudSimple portalına erişmek için **Cloudsimple hizmet Yetkilendirme** uygulamasını yetkilendirmeniz gerekir.  İzinleri verme hakkında daha fazla bilgi için bkz. [CloudSimple hizmet Yetkilendirme uygulamasına izin](https://docs.azure.cloudsimple.com/access-cloudsimple-portal/#consent-to-cloudsimple-service-authorization-application) verme

1. **Tüm Hizmetler**’i seçin.
2. **Cloudsimple Hizmetleri**için arama yapın.
3. Özel bulutunuzu oluşturmak istediğiniz CloudSimple hizmetini seçin.
4. Genel bakışta, cloudsimple portalına **Git** ' e tıklayarak cloudsimple portalı için yeni bir tarayıcı sekmesi açın.  İstenirse, Azure oturum açma kimlik bilgilerinizle oturum açın.  

    ![CloudSimple portalını Başlat](media/launch-cloudsimple-portal.png)

## <a name="create-point-to-site-vpn"></a>Noktadan siteye VPN oluşturma

Noktadan siteye VPN bağlantısı, bilgisayarınızdan özel bulutunuzu bağlamak için en kolay yoldur. Özel buluta uzaktan bağlanıyorsanız Noktadan siteye VPN bağlantısı kullanın.  Özel bulutunuz için hızlı erişim için aşağıdaki adımları izleyin.  Şirket içi ağınızdan CloudSimple bölgesine erişim, [siteden sıteye VPN](https://docs.azure.cloudsimple.com/vpn-gateway/) veya [Azure ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection/)kullanılarak yapılabilir.

### <a name="create-gateway"></a>Ağ Geçidi oluştur

1. CloudSimple portalını başlatın ve **ağ**' ı seçin.
2. **VPN Gateway**seçin.
3. **Yeni VPN Gateway**' ye tıklayın.

    ![VPN ağ geçidi oluşturma](media/create-vpn-gateway.png)

4. **Ağ geçidi yapılandırması**için aşağıdaki ayarları belirtin ve **İleri**' ye tıklayın.

    * Ağ Geçidi türü olarak **noktadan sıteye VPN ' yi** seçin.
    * Ağ geçidini tanımlamak için bir ad girin.
    * CloudSimple hizmetinizin dağıtıldığı Azure konumunu seçin.
    * Noktadan siteye ağ geçidi için istemci alt ağını belirtin.  Bağlandığınızda, DHCP adresleri bu alt ağdan verilecek.

5. **Bağlantı/Kullanıcı**için aşağıdaki ayarları belirtin ve **İleri**' ye tıklayın.

    * Tüm geçerli ve gelecekteki kullanıcıların bu noktadan siteye ağ geçidi aracılığıyla özel buluta erişmesine otomatik olarak izin vermek için **tüm kullanıcıları otomatik olarak ekle**' yi seçin. Bu seçeneği belirlediğinizde, Kullanıcı listesindeki tüm kullanıcılar otomatik olarak seçilir. Listedeki bireysel kullanıcıların seçimini kaldırarak otomatik seçeneği geçersiz kılabilirsiniz.
    * Yalnızca bireysel kullanıcıları seçmek için Kullanıcı listesindeki onay kutularına tıklayın.

6. VLAN/alt ağlar bölümü, ağ geçidi ve bağlantılar için yönetim ve Kullanıcı VLAN 'Ları/alt ağları belirtmenize olanak tanır.

    * **Otomatik olarak ekle** seçeneği bu ağ geçidinin genel ilkesini ayarlar. Ayarlar geçerli ağ geçidi için geçerlidir. Ayarlar **seçim** alanında geçersiz kılınabilir.
    * **Özel bulutların yönetim VLAN 'ları/alt ağlarını Ekle**' yi seçin. 
    * Kullanıcı tanımlı tüm VLAN 'Ları/alt ağları eklemek için **Kullanıcı tanımlı VLAN 'lar/alt ağlar Ekle**' ye tıklayın. 
    * **Seçim** ayarları **otomatik olarak ekle**altındaki genel ayarları geçersiz kılar. 

7. Ayarları gözden geçirmek için **İleri** 'ye tıklayın. Herhangi bir değişiklik yapmak için düzenleme simgeleri ' ne tıklayın.
8. VPN ağ geçidini oluşturmak için **Oluştur** ' a tıklayın.

### <a name="connect-to-cloudsimple-using-point-to-site-vpn"></a>Noktadan siteye VPN kullanarak CloudSimple 'a bağlanma

VPN istemcisi, bilgisayarınızdan CloudSimple 'a bağlanmak için gereklidir.  MacOS ve OS X için Windows veya [viscosity](https://www.sparklabs.com/viscosity/download/) Için [OpenVPN istemcisini](https://openvpn.net/community-downloads/) indirin.

1. CloudSimple portalını başlatın ve **ağ**' ı seçin.
2. **VPN Gateway**seçin.
3. VPN ağ geçitleri listesinden Noktadan siteye VPN Gateway ' e tıklayın.
4. **Kullanıcıları**seçin.
5. **VPN yapılandırması 'Nı indir** 'e tıklayın

    ![VPN yapılandırmasını indir](media/download-p2s-vpn-configuration.png)

6. VPN istemcinizdeki yapılandırmayı içeri aktarın

    * [Windows istemcisinde yapılandırmayı içeri aktarma](https://openvpn.net/vpn-server-resources/connecting-to-access-server-with-windows/#openvpn-open-source-openvpn-gui-program) yönergeleri
    * [MacOS veya OS X üzerinde yapılandırmayı içeri aktarma](https://www.sparklabs.com/support/kb/article/getting-started-with-viscosity-mac/#creating-your-first-connection) yönergeleri

7. CloudSimple 'a bağlanma

## <a name="create-a-vlan-for-your-workload-vms"></a>İş yükü sanal makinelerinize yönelik bir VLAN oluşturma

Özel bir bulut oluşturduktan sonra, iş yükünüzü/uygulama VM 'lerinizi dağıtacağınız bir VLAN oluşturun.

1. CloudSimple portalında **ağ**' ı seçin.
2. **VLAN/alt ağlar**' a tıklayın.
3. **VLAN/subnet oluştur** öğesine tıklayın

    ![VLAN/subnet oluştur](media/create-new-vlan-subnet.png)

4. Yeni VLAN/alt ağ için **özel bulutu** seçin.
5. Listeden bir VLAN KIMLIĞI seçin.  
6. Alt ağı tanımlamak için bir alt ağ adı girin.
7. CıDR aralığını ve maskesini belirtin.  Bu Aralık varolan alt ağlarla çakışmamalıdır.
8. **Gönder**'e tıklayın.

    ![VLAN/subnet ayrıntıları oluştur](media/create-new-vlan-subnet-details.png)

VLAN/subnet oluşturulacak.  Artık özel bulut vCenter 'unuzda dağıtılmış bir bağlantı noktası grubu oluşturmak için bu VLAN KIMLIĞINI kullanabilirsiniz. 

## <a name="connect-your-environment-to-an-azure-virtual-network"></a>Ortamınızı bir Azure sanal ağına bağlama

CloudSimple, özel bulutunuz için bir ExpressRoute devresi sağlar. Azure 'daki Sanal ağınızı ExpressRoute devresine bağlayabilirsiniz. Bağlantıyı ayarlamayla ilgili tam Ayrıntılar için [ExpressRoute kullanarak Azure sanal ağ bağlantısı](https://docs.azure.cloudsimple.com/cloudsimple-azure-network-connection/) 'ndaki adımları izleyin

## <a name="sign-in-to-vcenter"></a>VCenter 'da oturum açın

Artık sanal makineleri ve ilkeleri ayarlamak için vCenter 'da oturum açabilirsiniz.

1. VCenter 'a erişmek için CloudSimple portalından başlatın. Giriş sayfasında, **ortak görevler**altında, **vSphere istemcisini Başlat**' a tıklayın.  Özel bulutu seçin ve ardından özel bulutta **vSphere Istemcisini Başlat** ' a tıklayın.

    ![VSphere Istemcisini Başlat](media/launch-vcenter-from-cloudsimple-portal.png)

2. VCenter 'a erişmek için tercih ettiğiniz vSphere istemcinizi seçin ve Kullanıcı adınızla parolanızla oturum açın.  Varsayılanlar şunlardır:
    * Kullanıcı adı: **CloudOwner@cloudsimple.local**
    * Parola: **CloudSimple123!**  

Sonraki yordamlardaki vCenter ekranları vSphere (HTML5) istemcisinden alınır.

## <a name="change-your-vcenter-password"></a>VCenter parolanızı değiştirme

CloudSimple, vCenter 'da ilk kez oturum açtığınızda parolanızı değiştirmenizi önerir.  
Ayarladığınız parolanın aşağıdaki gereksinimleri karşılaması gerekir:

* En yüksek yaşam süresi: Parolanın her 365 günde bir değiştirilmesi gerekir
* Yeniden kullanımı kısıtla: Kullanıcılar önceki beş parolanın hiçbirini yeniden kullanmıyorum
* Uzunluk: 8-20 karakter
* Özel karakter: En az bir özel karakter
* Alfabetik karakterler: En az bir büyük harf, A-Z ve en az bir küçük harf, a-z
* Sayılarının En az bir sayısal karakter, 0-9
* Aynı bitişik karakterlerin üst sınırı: Üç

    Örnek: CC veya CCC parolanın bir parçası olarak kabul edilebilir, ancak CCCC değildir.

Gereksinimleri karşılamayan bir parola ayarlarsanız:

* vSphere Flash Istemcisini kullanıyorsanız bir hata bildiriyor
* HTML5 istemcisini kullanıyorsanız bir hata raporlamaz. İstemci değişikliği kabul etmez ve eski parola çalışmaya devam eder.

## <a name="change-nsx-administrator-password"></a>NSX yönetici parolasını değiştirme

NSX Yöneticisi varsayılan parolayla dağıtılır.  Özel bulutunuzu oluşturduktan sonra parolayı değiştirmenizi öneririz.

   * Kullanıcı adı: **yönetici**
   * Parola: **CloudSimple123!**

CloudSimple portalında tam etki alanı adını (FQDN) ve NSX Manager IP adresini bulabilirsiniz.

1. CloudSimple portalını başlatın ve **kaynakları**seçin.
2. Kullanmak istediğiniz özel buluta tıklayın.
3. **VSphere yönetim ağını** seçin
4. **NSX Manager** 'ıN FQDN 'SINI veya IP adresini kullanın ve bir Web tarayıcısı kullanarak bağlanın. 

    ![NSX Manager FQDN bulun](media/private-cloud-nsx-manager-fqdn.png)

Parolayı değiştirmek için [kullanıcının parolasını yönetme](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/2.4/administration/GUID-DB31B304-66A5-4516-9E55-2712D12B4F27.html)bölümündeki yönergeleri izleyin.

> [!WARNING]
> Varsayılan olarak, NSX Yönetici parolasının süresi 90 gün sonra dolar.

## <a name="create-a-port-group"></a>Bağlantı noktası grubu oluşturma

VSphere içinde dağıtılmış bir bağlantı noktası grubu oluşturmak için:

1. [VSphere ağ kılavuzunda](https://docs.vmware.com/en/VMware-vSphere/6.5/vsphere-esxi-vcenter-server-65-networking-guide.pdf)"dağıtılmış bir bağlantı noktası grubu ekleme" bölümündeki yönergeleri izleyin.
2. Dağıtılmış bağlantı noktası grubunu ayarlarken, [Iş yükü sanal makinelerinize VLAN oluşturma](#create-a-vlan-for-your-workload-vms)bölümünde oluşturulan VLAN kimliğini sağlayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'da VMware VM 'lerini kullanma](https://docs.azure.cloudsimple.com/quickstart-create-vmware-virtual-machine)
* [Azure 'da VMware VM 'lerini kullanma](quickstart-create-vmware-virtual-machine.md)
* [Azure ExpressRoute kullanarak şirket içi ağa bağlanma](https://docs.azure.cloudsimple.com/on-premises-connection/)
* [Şirket içinden siteden siteye VPN kurma](https://docs.azure.cloudsimple.com/vpn-gateway/)
