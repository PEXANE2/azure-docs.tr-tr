---
title: Azure Site Kurtarma ile Azure VM'lerine şirket içinde başarısız olun
description: Azure Site Kurtarma'yı kullanarak şirket içinde Azure'a geçilemeyip Azure'a bağlandıktan sonra Azure VM'lerine nasıl bağlanıştırılanın açıklanır
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/13/2019
ms.author: mayg
ms.openlocfilehash: f222cdd315b79503b1bdea032f495c71df4682b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281996"
---
# <a name="connect-to-azure-vms-after-failover-from-on-premises"></a>Şirket içinde başarısız olduktan sonra Azure VM'lerine bağlanın 


Bu makalede, başarısız olduktan sonra Azure VM'lere başarılı bir şekilde bağlanabilmeniz için bağlantının nasıl ayarlandığı açıklanmaktadır.

Azure'da şirket içi sanal makinelerin (VM'ler) ve fiziksel sunucuların olağanüstü kurtarma sını ayarladığınızda, [Azure Site Kurtarma](site-recovery-overview.md) makineleri Azure'a çoğaltmaya başlar. Daha sonra, kesintiler oluştuğunda, şirket içi sitenizden Azure'a geçemezsiniz. Başarısız olduğunda, Site Kurtarma, çoğaltılan şirket içi verileri kullanarak Azure Sanal M'leri oluşturur. Olağanüstü durum kurtarma planlamasının bir parçası olarak, başarısız olduktan sonra bu Azure VM'lerde çalışan uygulamalara nasıl bağlanabileceğinizi bulmanız gerekir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Başarısız olmadan önce tesis içi makineleri hazırlayın.
> * Başarısız olduktan sonra Azure VM'leri hazırlayın. 
> * Başarısız olduktan sonra Azure VM'lerinde IP adreslerini koruyun.
> * Başarısız olduktan sonra Azure VM'lerine yeni IP adresleri atayın.

## <a name="prepare-on-premises-machines"></a>Şirket içi makineleri hazırlayın

Azure VM'lere bağlantı sağlamak için, şirket içi makinelerinizi başarısız olmadan önce hazırlayın.

### <a name="prepare-windows-machines"></a>Windows makinelerini hazırlayın

Şirket içi Windows makinelerinde aşağıdakileri yapın:

1. Windows ayarlarını yapılandırın. Bunlar arasında statik kalıcı yolları veya WinHTTP proxy'sini kaldırmak ve disk SAN ilkesini **OnlineTümü'ne**ayarlamak yer alır. Bu talimatları [izleyin.](../virtual-machines/windows/prepare-for-upload-vhd-image.md#set-windows-configurations-for-azure)

2. Bu [hizmetlerin](../virtual-machines/windows/prepare-for-upload-vhd-image.md#check-the-windows-services) çalıştığını unutmayın.

3. Şirket içi makineye uzaktan bağlantı sağlamak için uzak masaüstünü (RDP) etkinleştirin. PowerShell ile RDP'yi nasıl etkinleştireceklerini [öğrenin.](../virtual-machines/windows/prepare-for-upload-vhd-image.md#update-remote-desktop-registry-settings)

4. Bir Azure VM'ye başarısız olduktan sonra internet üzerinden erişmek için, şirket içi makinedeki Windows Güvenlik Duvarı'nda TCP ve UDP'ye Genel profilde izin verin ve RDP'yi tüm profiller için izin verilen bir uygulama olarak ayarlayın.

5. Bir Azure VM'ine, şirket içi makinedeki Windows Güvenlik Duvarı'nda, siteden siteye VPN üzerinden erişmek istiyorsanız, Etki Alanı ve Özel profiller için RDP'ye izin verin. RDP trafiğine nasıl izin verebilirsiniz [öğrenin.](../virtual-machines/windows/prepare-for-upload-vhd-image.md#configure-windows-firewall-rules)
6. Bir arızayı tetiklediğinde şirket içi VM'de bekleyen Windows güncelleştirmeleri olmadığından emin olun. Varsa, güncellemeler başarısız olduktan sonra Azure VM'de yüklenmeye başlayabilir ve güncelleştirmeler bitene kadar VM'de oturum açamazsınız.

### <a name="prepare-linux-machines"></a>Linux makinelerini hazırlayın

Şirket içi Linux makinelerinde aşağıdakileri yapın:

1. Secure Shell hizmetinin sistem önyüklemede otomatik olarak başlayıp başlatılmaya ayarlı olduğundan kontrol edin.
2. Güvenlik duvarı kurallarının SSH bağlantısına izin verdiğinden emin olun.


## <a name="configure-azure-vms-after-failover"></a>Azure VM'lerini başarısız olduktan sonra yapılandırma

Başarısız olduktan sonra, oluşturulan Azure VM'lerinde aşağıdakileri yapın.

1. VM'ye internet üzerinden bağlanmak için VM'ye genel bir IP adresi atayın. Şirket içi makineniz için kullandığınız Azure VM için aynı genel IP adresini kullanamazsınız. [Daha fazlasını öğrenin](../virtual-network/virtual-network-public-ip-address.md)
2. VM'deki ağ güvenlik grubu (NSG) kurallarının RDP veya SSH bağlantı noktasına gelen bağlantılara izin verdiğini denetleyin.
3. VM'yi görüntülemek için [Önyükleme tanılamalarını](../virtual-machines/troubleshooting/boot-diagnostics.md#enable-boot-diagnostics-on-existing-virtual-machine) denetleyin.


> [!NOTE]
> Azure Bastion hizmeti, Azure VM'lerine özel RDP ve SSH erişimi sunar. Bu hizmet hakkında [daha fazla bilgi edinin.](../bastion/bastion-overview.md)

## <a name="set-a-public-ip-address"></a>Genel bir IP adresi ayarlama

Genel bir IP adresini azure VM'e el ile atamaya alternatif olarak, site [kurtarma kurtarma planında](site-recovery-create-recovery-plans.md)komut dosyası veya Azure otomasyon runbook'u kullanarak başarısız olma sırasında adresi atayabilir veya Azure Trafik Yöneticisi'ni kullanarak DNS düzeyinde yönlendirme ayarlayabilirsiniz. Herkese açık bir adres ayarlama hakkında [daha fazla bilgi edinin.](concepts-public-ip-address-with-site-recovery.md)


## <a name="assign-an-internal-address"></a>Dahili bir adres atama

Bir Azure VM'nin dahili IP adresini başarısız olduktan sonra ayarlamak için birkaç seçeneğiniz var:

- **Aynı IP adresini koruyun**: Azure VM'de şirket içi makineye ayrılan IP adresiyle aynı IP adresini kullanabilirsiniz.
- **Farklı IP adresi kullanın**: Azure VM için farklı bir IP adresi kullanabilirsiniz.


## <a name="retain-ip-addresses"></a>IP adreslerini koruyun

Site Kurtarma, Azure'da başarısız olduğunuzda aynı IP adreslerini korumanızı sağlar. Aynı IP adresini korumak, başarısız olduktan sonra olası ağ sorunlarını önler, ancak bazı karmaşıklıklar ortaya çıkarmaz.

- Hedef Azure VM, şirket içi sitenizle aynı IP adresini/alt netini kullanıyorsa, adresçileççle çakıştığı için siteden siteye VPN bağlantısı veya ExpressRoute kullanarak aralarında bağlantı kuramazsınız. Alt ağlar benzersiz olmalıdır.
- Uygulamaların Azure VM'lerinde kullanılabilmesi için, başarısız olduktan sonra şirket içinde Azure'a bağlantı kurmanız gerekir. Azure uzatılmış VN'leri desteklemez, bu nedenle IP adreslerini korumak istiyorsanız, şirket içi makineye ek olarak tüm alt ağ üzerinde başarısız olarak IP alanını Azure'a götürmeniz gerekir.
- Alt ağ başarısız olması, belirli bir alt ağın şirket içinde ve Azure'da aynı anda kullanılmamasından emin dir.

IP adreslerinin istinat için aşağıdaki adımlar gerektirir:

- Yinelenen öğenin & Ağı'nın İşlem özelliğinde, hedef Azure VM'nin şirket içi ayarı yansıtması için ağ ve IP adresleme ayarlayın.
- Alt ağlar olağanüstü durum kurtarma sürecinin bir parçası olarak yönetilmelidir. Şirket içi ağa eşleşecek bir Azure VNet'e ihtiyacınız vardır ve başarısız ağ rotalarından sonra alt ağın Azure'a ve yeni IP adresi konumlarına taşındığını yansıtacak şekilde değiştirilmelidir.  

### <a name="failover-example"></a>Failover örneği

Şimdi örneği inceleyelim.

- Hayali şirket Woodgrove Bank, iş uygulamalarını şirket içinde barındırır Mobil uygulamalarını Azure'da barındırır.
- Siteden siteye VPN üzerinden şirket içinde Azure'a bağlanır. 
- Woodgrove, şirket içi makineleri Azure'a çoğaltmak için Site Kurtarma'yı kullanıyor.
- Şirket içi uygulamaları sabit kodlu IP adresleri kullanır, bu nedenle Azure'da aynı IP adreslerini saklamak isterler.
- Uygulamaları çalıştıran makineler şirket içinde üç alt ağda çalışır:
    - 192.168.1.0/24.
    - 192.168.2.0/24
    - 192.168.3.0/24
- Azure'da çalışan uygulamaları Azure VNet **Azure Ağı'nda** iki alt ağda bulunur:
- 172.16.1.0/24
- 172.16.2.0/24.

Adresleri korumak için, işte yaptıkları şey.

1. Çoğaltmayı etkinleştirdiklerinde, makinelerin **Azure Ağı'nda**çoğaltılması gerektiğini belirtirler.
2. Azure'da **Kurtarma Ağı** oluştururlar. Bu VNet, 192.168.1.0/24 alt netini şirket içi ağlarında yansıttır.
3. Woodgrove iki ağ arasında [VNet-to-VNet bağlantısı](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) kurar. 

    > [!NOTE]
    > Uygulama gereksinimlerine bağlı olarak, bir Site Kurtarma [kurtarma planında](site-recovery-create-recovery-plans.md)veya başarısız olduktan sonra el ile adım/komut dosyası adımı/Azure otomasyon runbook'u olarak, başarısız olmadan önce bir VNet-to-VNet bağlantısı ayarlanabilir.

4. Başarısız olmadan önce, Site Kurtarma'daki makine özelliklerinde, hedef IP adresini bir sonraki yordamda açıklandığı gibi şirket içi makinenin adresine ayarlarlar.
5. Azure VM'ler aynı IP adresiyle oluşturulur. Woodgrove, VNet eşlemesi (aktarım bağlantısı etkin) kullanarak **Azure Ağı'ndan** Recovery **Network** VNet'e bağlanır.
6. Woodgrove'un, 192.168.1.0/24'ün Azure'a taşındığını yansıtacak şekilde rotaları değiştirmek de dahil olmak üzere ağ değişiklikleri yapması gerekir.  

**Başarısız olmadan önce altyapı**

![Subnet başarısız olmadan önce](./media/site-recovery-network-design/network-design7.png)


**Başarısız olduktan sonra altyapı**

![Subnet failover sonra](./media/site-recovery-network-design/network-design9.png)


### <a name="set-target-network-settings"></a>Hedef ağ ayarlarını ayarlama

Başarısız olmadan önce, hedef Azure VM'nin ağ ayarlarını ve IP adresini belirtin.

1.  Kurtarma Hizmetleri kasasında -> **Çoğaltılan öğeler,** şirket içi makineyi seçin.
2. Makinenin **Bilgi İşlem ve Ağ** sayfasında, hedef Azure VM için ağ ve bağdaştırıcı ayarlarını yapılandırmak için **Edit'i**tıklatın.
3. **Ağ özelliklerinde,** Azure VM'nin başarısız olduktan sonra oluşturulduğunda bulunacağı hedef ağı seçin.
4. **Ağ arabirimlerinde,** hedef ağdaki ağ bağdaştırıcılarını yapılandırın. Varsayılan olarak Site Kurtarma, şirket içi makinede algılanan tüm NIC'leri gösterir.
    - **Hedef ağ arabirimi türünde,** her NIC'i **Birincil**, **İkincil**olarak ayarlayabilirsiniz veya hedef ağda belirli bir NIC'ye ihtiyacınız yoksa **oluşturmayın.** Bir ağ bağdaştırıcısı, başarısız olmak için birincil olarak ayarlanmalıdır. Hedef ağı değiştirmenin Azure VM için tüm NIC'leri etkilediğini unutmayın.
    - Azure VM'nin dağıtılanacağı alt ağı belirtmek için NIC adını tıklatın.
    - Azure VM'yi hedeflemek için atamak istediğiniz özel IP adresiyle **Dinamik'in** üzerine yazın. Bir IP adresi belirtilmemişse Site Kurtarma, alt ağdaki bir sonraki kullanılabilir IP adresini başarısız olduğu anda NIC'ye atar.
    - Şirket içi Azure'da başarısız olmak için NIC'leri yönetme hakkında [daha fazla bilgi edinin.](site-recovery-manage-network-interfaces-on-premises-to-azure.md)


## <a name="get-new-ip-addresses"></a>Yeni IP adresleri alın

Bu senaryoda, Azure VM başarısız olduktan sonra yeni bir IP adresi alır. Azure VM'nin IP adresine işaret eden makineler üzerinde başarısız olan kayıtları güncelleştirmek için bir DNS güncelleştirmesi.



## <a name="next-steps"></a>Sonraki adımlar
Şirket içi Active Directory ve DNS'yi Azure'da çoğaltma [hakkında bilgi edinin.](site-recovery-active-directory.md)


