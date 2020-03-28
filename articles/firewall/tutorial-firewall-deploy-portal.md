---
title: 'Öğretici: Azure portalını kullanarak Azure Güvenlik Duvarını dağıtma & yapılandırma'
description: Bu öğreticide Azure portalı kullanarak Azure Güvenlik Duvarı'nı dağıtmayı ve yapılandırmayı öğreneceksiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 02/21/2020
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 064fcf618914bca31ad9e7e60c76df8f599cd8bf
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79239577"
---
# <a name="tutorial-deploy-and-configure-azure-firewall-using-the-azure-portal"></a>Öğretici: Azure portalı kullanarak Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma

Giden ağ erişimini denetleme, genel ağ güvenlik planının önemli bir parçasıdır. Örneğin, web sitelerine erişimi sınırlamak isteyebilirsiniz. Veya, giden IP adreslerini ve erişilebilen bağlantı noktalarını sınırlamak isteyebilirsiniz.

Azure Güvenlik Duvarı, Azure alt ağından giden ağ erişimini denetlemenin bir yoludur. Azure Güvenlik Duvarı ile şunları yapılandırabilirsiniz:

* Bir alt ağdan erişilebilen tam etki alanı adlarını (FQDN) tanımlayan uygulama kuralları.
* Kaynak adres, protokol, hedef bağlantı noktası ve hedef adresini tanımlayan ağ kuralları.

Ağ trafiğinizi güvenlik duvarından alt ağın varsayılan ağ geçidi olarak yönlendirdiğinizde ağ trafiği yapılandırılan güvenlik duvarı kurallarına tabi tutulur.

Bu öğreticide kolay dağıtım için üç alt ağa sahip tek bir basitleştirilmiş sanal ağ oluşturursunuz. Üretim dağıtımları için hub [ve spoke modeli](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) önerilir. Güvenlik duvarı kendi VNet'indedir. İş yükü sunucuları, bir veya daha fazla alt ağla aynı bölgedeki eşlenmiş VNet'lerde bulunmaktadır.

* **AzureFirewallSubnet** - güvenlik duvarı bu alt ağdadır.
* **Workload-SN**: İş yükü sunucusu bu alt ağda yer alır. Bu alt ağın ağ trafiği güvenlik duvarından geçer.
* **Jump-SN**: "Atlama" sunucusu bu alt ağda yer alır. Atlama sunucusu, Uzak Masaüstü ile bağlanabileceğiniz genel IP adresine sahiptir. Buradan iş yükü sunucusuna bağlanabilirsiniz (başka bir Uzak Masaüstü oturumuyla).

![Öğretici ağı altyapısı](media/tutorial-firewall-rules-portal/Tutorial_network.png)

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Test amaçlı ağ ortamı oluşturma
> * Güvenlik duvarı dağıtma
> * Varsayılan rota oluşturma
> * www.google.com erişime izin verecek şekilde bir uygulama kuralını yapılandırma
> * Dış DNS sunucularına erişime izin vermek için ağ kuralı yapılandırma
> * Güvenlik duvarını test etme

Tercih ederseniz, bu öğreticiyi [Azure PowerShell](deploy-ps.md) kullanarak tamamlayabilirsiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="set-up-the-network"></a>Ağı ayarlama

İlk olarak güvenlik duvarını dağıtmak için gerekli olan kaynakları içerecek bir kaynak grubu oluşturun. Ardından sanal ağı, alt ağları ve test sunucularını oluşturun.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, bu öğreticideki tüm kaynakları içerir.

1. Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.
2. Azure portalı menüsünde **Kaynak gruplarını** seçin veya herhangi bir sayfadan *Kaynak gruplarını* arayın ve seçin. Ardından **Ekle**'yi seçin.
3. **Kaynak grup adı için** *Test-FW-RG*girin.
4. **Abonelik** bölümünde aboneliğinizi seçin.
5. **Kaynak grubu konumu** bölümünde bir konum seçin. Oluşturduğunuz diğer tüm kaynaklar aynı konumda olmalıdır.
6. **Oluştur'u**seçin.

### <a name="create-a-vnet"></a>Sanal ağ oluşturma

Bu sanal ağda üç alt ağ bulunacaktır.

> [!NOTE]
> AzureFirewallSubnet alt ağının boyutu /26'dır. Alt ağ boyutu hakkında daha fazla bilgi için [Azure Güvenlik Duvarı SSS'si'ne](firewall-faq.md#why-does-azure-firewall-need-a-26-subnet-size)bakın.

1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**’u seçin.
1. **Ağ** > **Sanal ağ**seçin.
1. **Ad** alanına **Test-FW-VN** yazın.
1. **Adres alanı** için **10.0.0.0/16** yazın.
1. **Abonelik** bölümünde aboneliğinizi seçin.
1. **Kaynak grubu için** **Test-FW-RG'yi**seçin.
1. **Konum** alanında önceden kullandığınız konumu seçin.
1. **Alt ağ** bölümünde **Ad** alanına **AzureFirewallSubnet** yazın. Güvenlik duvarı bu alt ağda yer alacaktır ve alt ağ adının **mutlaka** AzureFirewallSubnet olması gerekir.
1. **Adres aralığı**için, **10.0.1.0/26**yazın.
1. Diğer varsayılan ayarları kabul edin ve sonra **Oluştur'u**seçin.

### <a name="create-additional-subnets"></a>Ek alt ağ oluşturma

Bir sonraki adımda atlama sunucusu için alt ağlar ve iş yükü sunucuları için bir alt ağ oluşturun.

1. Azure portalı menüsünde **Kaynak gruplarını** seçin veya herhangi bir sayfadan *Kaynak gruplarını* arayın ve seçin. Sonra **Test-FW-RG**seçin.
2. **Test-FW-VN** sanal ağını seçin.
3. **Alt ağlar** > **+Subnet'i**seçin.
4. **Ad** alanına **Workload-SN** yazın.
5. **Adres aralığı** için **10.0.2.0/24** yazın.
6. **Tamam'ı**seçin.

**Jump-SN** adına ve **10.0.3.0/24** adres aralığına sahip bir alt ağ daha oluşturun.

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Şimdi atlama ve iş yükü sanal makinelerini oluşturup uygun alt ağlara yerleştirin.

1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**’u seçin.
2. **İşlem**’i ve sonra Öne Çıkanlar listesinde **Windows Server 2016 Datacenter**’ı seçin.
3. Sanal makine için şu değerleri girin:

   |Ayar  |Değer  |
   |---------|---------|
   |Kaynak grubu     |**Test-FW-RG**|
   |Sanal makine adı     |**Srv-Atlama**|
   |Bölge     |Öncekiyle aynı|
   |Yönetici kullanıcı adı     |**azureuser**|
   |Parola     |**Azure123456!**|

4. **Gelen bağlantı noktası kuralları altında,** Ortak gelen bağlantı **noktaları**için, **seçili bağlantı noktalarına izin ver'i**seçin.
5. **Gelen bağlantı noktalarını seçmek**için **RDP (3389)** seçeneğini belirleyin.

6. Diğer varsayılanları kabul edin ve **Sonraki: Diskler'i**seçin.
7. Disk varsayılanlarını kabul edin ve **Sonraki: Ağ' ı**seçin.
8. **Test-FW-VN'nin** sanal ağ için seçildiğinden ve alt ağın **Jump-SN**olduğundan emin olun.
9. **Genel IP**için varsayılan yeni genel ip adresi adını (Srv-Jump-ip) kabul edin.
11. Diğer varsayılanları kabul edin ve **Sonraki: Yönetim'i**seçin.
12. Önyükleme tanılamasını devre dışı kalmak için **Kapalı'yı** seçin. Diğer varsayılanları kabul edin ve **Gözden Geçir + oluştur'u**seçin.
13. Özet sayfasındaki ayarları gözden geçirin ve ardından **Oluştur'u**seçin.

**Srv-Work**adlı başka bir sanal makineyi yapılandırmak için aşağıdaki tablodaki bilgileri kullanın. Yapılandırmanın geri kalanı Srv-Jump sanal makinesiyle aynıdır.

|Ayar  |Değer  |
|---------|---------|
|Alt ağ|**Workload-SN**|
|Genel IP|**Yok**|
|Genel gelen bağlantı noktaları|**Yok**|

## <a name="deploy-the-firewall"></a>Güvenlik duvarını dağıtma

Güvenlik duvarını sanal ağa dağıtın.

1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**’u seçin.
2. Arama kutusuna **güvenlik duvarı** yazın ve **Enter**tuşuna basın.
3. **Güvenlik Duvarı'nı** seçin ve ardından **Oluştur'u**seçin.
4. **Güvenlik duvarı oluştur** sayfasında aşağıdaki ayarları kullanarak güvenlik duvarını yapılandırın:

   |Ayar  |Değer  |
   |---------|---------|
   |Abonelik     |\<aboneliğiniz\>|
   |Kaynak grubu     |**Test-FW-RG** |
   |Adı     |**Test-FW01**|
   |Konum     |Önceden kullandığınız konumu seçin|
   |Bir sanal ağ seçin     |**Mevcut kullanın**: **Test-FW-VN**|
   |Genel IP adresi     |**Yeni ekle.** Genel IP adresinin türü Standart SKU olmalıdır.|

5. **İncele ve oluştur**’u seçin.
6. Özeti gözden geçirin ve ardından güvenlik duvarını oluşturmak için **Oluştur'u** seçin.

   Dağıtma işlemi birkaç dakika sürebilir.
7. Dağıtım tamamlandıktan sonra **Test-FW-RG** kaynak grubuna gidin ve **Test-FW01** güvenlik duvarını seçin.
8. Özel IP adresini not edin. Varsayılan rotayı oluştururken bu adresi kullanacaksınız.

## <a name="create-a-default-route"></a>Varsayılan rota oluşturma

**Workload-SN** alt ağında varsayılan giden rotayı güvenlik duvarından geçecek şekilde yapılandırın.

1. Azure portalı menüsünde **Tüm hizmetleri** seçin veya herhangi bir sayfadan *Tüm hizmetleri* arayın ve seçin.
2. **Ağ** **altında, Rota tabloları**seçin.
3. **Ekle'yi**seçin.
4. **Ad** alanına **Firewall-route** yazın.
5. **Abonelik** bölümünde aboneliğinizi seçin.
6. **Kaynak grubu için** **Test-FW-RG'yi**seçin.
7. **Konum** alanında önceden kullandığınız konumu seçin.
8. **Oluştur'u**seçin.
9. **Yenile'yi**seçin ve ardından **Güvenlik Duvarı rota** rota tablosunu seçin.
10. **Alt Ağlar'ı** seçin ve ardından **Ilişkilendir'i**seçin.
11. **Sanal ağ** > **Test-FW-VN'yi**seçin.
12. **Alt ağ** **için, İş Yükü-SN'yi**seçin. Bu rota için yalnızca **İş Yükü-SN** alt netini seçtiğinizden emin olun, aksi takdirde güvenlik duvarınız düzgün çalışmaz.

13. **Tamam'ı**seçin.
14. **Rotalar'ı** seçin ve ardından **Ekle'yi**seçin.
15. **Rota adı**için **fw-dg**yazın.
16. **Adres ön eki** alanına **0.0.0.0/0** yazın.
17. **Sonraki atlama türü** için **Sanal gereç**'i seçin.

    Azure Güvenlik Duvarı, normalde yönetilen bir hizmettir ancak bu durumda sanal gereç kullanılabilir.
18. **Sonraki atlama adresi** alanına önceden not ettiğiniz güvenlik duvarı özel IP adresini yazın.
19. **Tamam'ı**seçin.

## <a name="configure-an-application-rule"></a>Uygulama kuralı yapılandırma

Bu, giden erişimin www.google.com izin veren uygulama kuralıdır.

1. **Test-FW-RG'yi**açın ve **Test-FW01** güvenlik duvarını seçin.
2. **Test-FW01** sayfasında, **Ayarlar** **altında, Kurallar'ı**seçin.
3. Uygulama **kural koleksiyonu** sekmesini seçin.
4. **Uygulama kuralı koleksiyonu ekle'yi**seçin.
5. **Ad** alanına **App-Coll01** yazın.
6. **Öncelik** alanına **200** yazın.
7. **Eylem** alanında **İzin ver**'i seçin.
8. **Kurallar**altında , **Hedef FQDNs**, **Ad**için , türü **İzin Ver-Google**.
9. **Kaynak türü için**IP **adresini**seçin.
10. **Kaynak**için , tip **10.0.2.0/24**.
11. **Protokol:bağlantı noktası** alanına **http, https** yazın.
12. **Hedef FQDNS**için, **www.google.com** yazın
13. **Ekle'yi**seçin.

Azure Güvenlik Duvarı'nda varsayılan olarak izin verilen altyapı FQDN'leri için yerleşik bir kural koleksiyonu bulunur. Bu FQDN'ler platforma özgüdür ve başka amaçlarla kullanılamaz. Daha fazla bilgi için bkz. [Altyapı FQDN'leri](infrastructure-fqdns.md).

## <a name="configure-a-network-rule"></a>Ağ kuralını yapılandırma

Bu, bağlantı noktası 53’deki (DNS) iki IP adresine giden erişime izin veren ağ kuralıdır.

1. Ağ **kural koleksiyonu** sekmesini seçin.
2. **Ağ kuralı koleksiyonu ekle'yi**seçin.
3. **Ad** alanına **Net-Coll01** yazın.
4. **Öncelik** alanına **200** yazın.
5. **Eylem** alanında **İzin ver**'i seçin.
6. **Kurallar**altında , **IP adresleri**, **Ad**için , türü **İzin Ver-DNS**.
7. **Protokol** alanında **UDP**'yi seçin.
9. **Kaynak türü için**IP **adresini**seçin.
1. **Kaynak**için , tip **10.0.2.0/24**.
2. **Hedef adresi**için, **209.244.0.3,209.244.0.4** yazın

   Bunlar CenturyLink tarafından işletilen genel DNS sunucularıdır.
1. **Hedef Bağlantı Noktaları** için **53** yazın.
2. **Ekle'yi**seçin.

### <a name="change-the-primary-and-secondary-dns-address-for-the-srv-work-network-interface"></a>**Srv-Work** ağ arabiriminin birincil ve ikincil DNS adresini değiştirme

Bu öğreticide sınama amacıyla, sunucunun birincil ve ikincil DNS adreslerini yapılandırın. Bu genel bir Azure Güvenlik Duvarı gereksinimi değildir.

1. Azure portalı menüsünde **Kaynak gruplarını** seçin veya herhangi bir sayfadan *Kaynak gruplarını* arayın ve seçin. **Test-FW-RG** kaynak grubunu seçin.
2. **Srv-Work** sanal makinesinin ağ arabirimini seçin.
3. **Ayarlar** **altında, DNS sunucularını**seçin.
4. **DNS sunucuları**altında, **Özel**seçin.
5. **DNS sunucusu ekle** metin kutusuna **209.244.0.3**, sonraki metin kutusuna da **209.244.0.4** yazın.
6. **Kaydet'i**seçin.
7. **Srv-Work** sanal makinesini yeniden başlatın.

## <a name="test-the-firewall"></a>Güvenlik duvarını test etme

Beklendiği gibi çalıştığını doğrulamak için güvenlik duvarını test edin.

1. Azure portalda **Srv-Work** sanal makinesinin ağ ayarlarını gözden geçirin ve özel IP adresini not edin.
2. Uzak bir masaüstünü **Srv-Jump** sanal makinesine bağlayın ve oturum açın. Buradan **Srv-Work** özel IP adresine uzak bir masaüstü bağlantısı açın.
3. Internet Explorer'ı açın ve https://www.google.com adresine gidin.
4. Internet Explorer güvenlik uyarılarında **Tamam** > **Kapat'ı** seçin.

   Google giriş sayfasını görmelisiniz.

5. https://www.microsoft.com adresine gidin.

   Güvenlik duvarının engellemesi gerekir.

Şimdi güvenlik duvarı kurallarının işe yaradığını doğruladınız:

* İzin verilen bir FQDN'ye göz atabilir ancak diğerlerine göz atamazsınız.
* Yapılandırılmış dış DNS sunucusunu kullanarak DNS adlarını çözümleyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik duvarı kaynaklarını bir sonraki öğretici için tutabilirsiniz veya artık gerekli değilse **Test-FW-RG** kaynak grubunu silerek güvenlik duvarıyla ilgili tüm kaynakları silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Azure Güvenlik Duvarı günlüklerini izleme](./tutorial-diagnostics.md)
