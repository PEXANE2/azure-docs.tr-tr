---
title: "Öğretici: Azure Güvenlik Duvarı Yöneticisi önizlemesi kullanarak sanal WAN 'nizin güvenliğini sağlama"
description: Bu öğreticide, Azure portal kullanarak sanal WAN 'nizin güvenliğini Azure Güvenlik Duvarı Yöneticisi ile nasıl sağlayacağınızı öğreneceksiniz.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: tutorial
ms.date: 05/01/2020
ms.author: victorh
ms.openlocfilehash: b13f3b4eeb57c34f51152bb6d1914f6c80f31be1
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82691031"
---
# <a name="tutorial-secure-your-virtual-wan-using-azure-firewall-manager-preview"></a>Öğretici: Azure Güvenlik Duvarı Yöneticisi önizlemesi kullanarak sanal WAN 'nizin güvenliğini sağlama 

[!INCLUDE [Preview](../../includes/firewall-manager-preview-notice.md)]

Azure Güvenlik Duvarı Yöneticisi önizlemesi 'ni kullanarak, özel IP adreslerine, Azure PaaS 'ye ve Internet 'e yönelik bulut ağ trafiğinizi güvenli hale getirmek için güvenli sanal hub 'lar oluşturabilirsiniz. Güvenlik duvarındaki trafik yönlendirme otomatikleştirilmiştir, bu nedenle Kullanıcı tanımlı yollar (UDRs) oluşturmanız gerekmez.

![bulut ağının güvenliğini sağlama](media/secure-cloud-network/secure-cloud-network.png)

Güvenlik Duvarı Yöneticisi ayrıca bir hub sanal ağ mimarisini destekler. Güvenli sanal hub ve hub sanal ağ mimarisi türleri karşılaştırması için bkz [. Azure Güvenlik Duvarı Yöneticisi mimari seçenekleri nelerdir?](vhubs-and-vnets.md)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bağlı bileşen sanal ağını oluşturma
> * Güvenli sanal hub oluşturma
> * Hub ve bağlı bileşen VNET 'leri bağlama
> * Bir güvenlik duvarı ilkesi oluşturma ve merkezinizi güvenli hale getirme
> * Trafiği hub 'ınıza yönlendirin
> * Güvenlik duvarını test etme

## <a name="create-a-hub-and-spoke-architecture"></a>Hub ve bağlı bileşen mimarisi oluşturma

İlk olarak, sunucularınızı yerleştirebileceğiniz bir bağlı bileşen sanal ağı oluşturun.

### <a name="create-a-spoke-virtual-network-and-subnets"></a>Bağlı bileşen sanal ağı ve alt ağları oluşturma

1. Azure portal giriş sayfasında, **kaynak oluştur**' u seçin.
2. **Ağ**altında **sanal ağ**' ı seçin.
2. **Abonelik** bölümünde aboneliğinizi seçin.
1. **Kaynak grubu**için, **Yeni oluştur**' u seçin ve ad için **ILT-Manager** yazın ve **Tamam**' ı seçin.
2. **Ad**Için **bağlı bileşen-01**yazın.
3. **Bölge**Için **(US) Doğu ABD**seçin.
4. **İleri ' yi seçin: IP adresleri**.
1. **Adres alanı**için varsayılan **10.0.0.0/16**' yı kabul edin.
3. **Alt ağ adı**altında **varsayılan**' ı seçin.
4. Alt ağ adını **Iş yükü-sn**olarak değiştirin.
5. **Alt ağ adres aralığı**için **10.0.1.0/24**yazın.
6. **Kaydet**' i seçin.

Ardından, bir sıçrama sunucusu için bir alt ağ oluşturun.

1. **Alt ağ ekle**' yi seçin.
4. **Alt ağ adı**Için, **atsn**yazın.
5. **Alt ağ adres aralığı**için **10.0.2.0/24**yazın.
6. **Add (Ekle)** seçeneğini belirleyin.

Şimdi sanal ağı oluşturun.

1. **İncele ve oluştur**’u seçin.
2. **Oluştur**’u seçin.

### <a name="create-the-secured-virtual-hub"></a>Güvenli sanal hub 'ı oluşturma

Güvenlik Duvarı Yöneticisi 'Ni kullanarak güvenli sanal hub 'ınızı oluşturun.

1. Azure portal giriş sayfasında, **tüm hizmetler**' i seçin.
2. Arama kutusuna **güvenlik duvarı Yöneticisi** yazın ve **güvenlik duvarı Yöneticisi**' ni seçin.
3. **Güvenlik duvarı Yöneticisi** sayfasında, **güvenli sanal hub 'ları görüntüle**' yi seçin.
4. **Güvenlik duvarı Yöneticisi 'nde | Güvenli sanal hub 'lar** sayfasında, **yeni güvenli sanal hub oluştur**' u seçin.
5. **Kaynak grubu**Için, **İlt-Manager**' ı seçin.
7. **Bölge**için **Doğu ABD**' yi seçin.
1. **Güvenli sanal hub adı**Için, **hub-01**yazın.
2. **Hub adres alanı**için **10.1.0.0/16**yazın.
3. Yeni vWAN adı için **vwan-01**yazın.
4. **Güvenilen güvenlik Iş ortaklarını etkinleştirmek IÇIN VPN ağ geçidini dahil et** onay kutusunu işaretsiz bırakın.
5. **İleri ' yi seçin: Azure Güvenlik Duvarı**.
6. Varsayılan **Azure Güvenlik Duvarı** **etkin** ayarını kabul edin ve ardından **İleri: güvenilen güvenlik ortağı**' nı seçin.
7. Varsayılan **güvenilir güvenlik Iş ortağı** **devre dışı** ayarını kabul edin ve ileri ' yi seçin **: gözden geçir + oluştur**.
8. **Oluştur**’u seçin. Dağıtımı yaklaşık 30 dakika sürer.

### <a name="connect-the-hub-and-spoke-vnets"></a>Hub ve bağlı bileşen VNET 'leri bağlama

Artık hub ve bağlı bileşen VNET 'leri de eşler.

1. **İlt-Manager** kaynak grubunu seçin ve ardından **vwan-01** sanal WAN ' ı seçin.
2. **Bağlantı**altında **sanal ağ bağlantıları**' nı seçin.
3. **Bağlantı ekle**' yi seçin.
4. **Bağlantı adı**için **hub-kol**yazın.
5. Hub **'lar**için **hub-01**' i seçin.
6. **Kaynak grubu**Için, **İlt-Manager**' ı seçin.
7. **Sanal ağ**Için **bağlı bileşen-01**' i seçin.
8. **Tamam**’ı seçin.

## <a name="create-a-firewall-policy-and-secure-your-hub"></a>Bir güvenlik duvarı ilkesi oluşturma ve merkezinizi güvenli hale getirme

Bir güvenlik duvarı ilkesi, trafiği bir veya daha fazla güvenli sanal hub üzerinde yönlendirmek için kural koleksiyonlarını tanımlar. Güvenlik Duvarı ilkenizi oluşturup merkezinizi güvenli hale getirin.

1. Güvenlik Duvarı Yöneticisi 'nden **Azure Güvenlik Duvarı Ilkelerini görüntüle**' yi seçin.
2. **Azure Güvenlik Duvarı Ilkesi oluştur**' u seçin.
3. **İlke ayrıntıları**' nın altında, **ad** türü **ilkesi-01** ve **bölge** için **Doğu ABD**seçin.
4. **Sonraki: kurallar**' ı seçin.
5. **Kurallar** sekmesinde **kural koleksiyonu Ekle**' yi seçin.
6. **Kural koleksiyonu Ekle** sayfasında, **ad**için **RC-01** yazın.
7. **Kural koleksiyonu türü**için **uygulama**' yı seçin.
8. **Öncelik**için **100**yazın.
9. **Kural toplama eyleminin** **izin ver**olduğundan emin olun.
10. Kural **adı** için **Allow-MSFT**yazın.
11. **Kaynak adresi**için yazın **\***.
12. **Protokol**için **http, https**yazın.
13. * * Hedef türünün **FQDN**olduğundan emin olun.
14. **Hedef**için ** \*. Microsoft.com**yazın.
15. **Add (Ekle)** seçeneğini belirleyin.
16. Ileri 'yi seçin **: hub**.
17. **Hub 'lar** sekmesinde **sanal hub 'ları ilişkilendir**' i seçin.
18. **Hub-01** ' i seçin ve ardından **Ekle**' yi seçin.
1. **İncele ve oluştur**’u seçin.
2. **Oluştur**’u seçin.

Bu işlem yaklaşık beş dakika veya daha uzun sürebilir.

## <a name="route-traffic-to-your-hub"></a>Trafiği hub 'ınıza yönlendirin

Artık ağ trafiğinin güvenlik duvarınızdan yönlendirildiğinden emin olmanız gerekir.

1. Güvenlik Duvarı Yöneticisi 'nden **güvenli sanal hub 'lar**' ı seçin.
2. **Hub-01**' i seçin.
3. **Ayarlar**altında **yol ayarları**' nı seçin.
4. **Internet trafiği**, **sanal ağlardan gelen trafik**altında **Azure Güvenlik Duvarı aracılığıyla gönder**' i seçin.
5. **Azure özel trafiği**, **sanal ağlara giden trafik**altında **Azure Güvenlik Duvarı aracılığıyla gönder**' i seçin.
6. **IP adresi ön eklerini Düzenle**' yi seçin.
8. Iş yükü alt ağının adresi olarak **10.0.1.0/24** yazın ve **Kaydet**' i seçin.
9. **Ayarlar**altında **Bağlantılar**' ı seçin.
10. **Hub-ışınsal** bağlantısı bağlantısının **Internet trafiğini** **güvenli**olarak gösterdiğini doğrulayın.


## <a name="test-your-firewall"></a>Güvenlik duvarınızı test etme

Güvenlik Duvarı kurallarınızı test etmek için, birkaç sunucu dağıtmanız gerekir. Güvenlik duvarı kurallarını test etmek için iş yükü-SN alt ağında Iş yükünü-SRV 'yi dağıtırsınız ve ardından Internet 'ten bağlanmak için Uzak Masaüstü 'Nü kullanabilmeniz ve sonra Iş yükü-SRV 'ye bağlanabilmek için.

### <a name="deploy-the-servers"></a>Sunucuları dağıtma

1. Azure portal **kaynak oluştur**' u seçin.
2. **Popüler** listede **Windows Server 2016 Datacenter** ' ı seçin.
3. Sanal makine için şu değerleri girin:

   |Ayar  |Değer  |
   |---------|---------|
   |Kaynak grubu     |**FW-yönetici**|
   |Sanal makine adı     |**Geç-SRV**|
   |Bölge     |**ABD Doğu ABD)**|
   |Yönetici Kullanıcı adı     |Kullanıcı adı yazın|
   |Parola     |parola yazın|

4. **Gelen bağlantı noktası kuralları**altında, **Genel gelen bağlantı noktaları**Için **Seçili bağlantı noktalarına izin ver**' i seçin
5. **Gelen bağlantı noktaları Seç**için **RDP (3389)** seçeneğini belirleyin.
6. Diğer varsayılanları kabul edin ve **İleri ' yi seçin: diskler**.
7. Disk varsayılanlarını kabul edin ve **İleri ' yi seçin: ağ**.
8. Sanal ağ için **bağlı bileşen-01** ' in seçildiğinden ve alt AĞıN **geç sn**olduğundan emin olun.
9. **Genel IP**için, varsayılan yeni genel IP adresi adını (at-SRV-IP) kabul edin.
11. Diğer varsayılanları kabul edin ve **İleri: yönetim**' i seçin.
12. Önyükleme tanılamayı devre dışı bırakmak için **Kapat** ' ı seçin. Diğer varsayılanları kabul edin ve **gözden geçir + oluştur**' u seçin.
13. Özet sayfasında ayarları gözden geçirin ve ardından **Oluştur**' u seçin.

**Iş yükü-SRV**adlı başka bir sanal makineyi yapılandırmak için aşağıdaki tablodaki bilgileri kullanın. Yapılandırmanın geri kalanı Srv-Jump sanal makinesiyle aynıdır.

|Ayar  |Değer  |
|---------|---------|
|Alt ağ|**Workload-SN**|
|Genel IP|**Yok**|
|Ortak gelen bağlantı noktaları|**Yok**|

### <a name="add-a-route-table-and-default-route"></a>Yol tablosu ve varsayılan yol ekleme

Bir Internet bağlantısı ile bağlantı kurulmasına izin vermek için, bir yol tablosu ve Internet 'e bir varsayılan ağ geçidi yolu **atlayın** .

1. Azure portal **kaynak oluştur**' u seçin.
2. Arama kutusuna **yol tablosu** yazın ve ardından **yol tablosu**' nu seçin.
3. **Oluştur**’u seçin.
4. **Ad**için **RT-01** yazın.
5. Kaynak grubunun ve **(US) bölge Doğu ABD** için aboneliğinizi, **İlt-Manager** ' ı seçin.
6. **Oluştur**’u seçin.
7. Dağıtım tamamlandığında **RT-01** yol tablosunu seçin.
8. **Rotalar** ' ı seçin ve ardından **Ekle**' yi seçin.
9. **Yol adı**için, **atlayın** yazın.
10. **Adres ön eki**için **0.0.0.0/0** yazın.
11. **Sonraki atlama türü**için **Internet** ' i seçin.
12. **Tamam**’ı seçin.
13. Dağıtım tamamlandığında, **alt ağlar**' ı seçin ve ardından **ilişkilendir**' i seçin.
14. **Sanal ağ**Için **bağlı bileşen-01** ' i seçin.
15. **Alt ağ**için **at-sn** ' i seçin.
16. **Tamam**’ı seçin.

### <a name="test-the-rules"></a>Kuralları test etme

Şimdi, beklendiği gibi çalıştığını doğrulamak için güvenlik duvarı kurallarını test edin.

1. Azure portal, **Iş yükü-SRV** sanal makinesi için ağ ayarlarını gözden geçirin ve özel IP adresini aklınızda edin.
2. Uzak bir masaüstünü, **Atsrv** sanal makinesine bağlayın ve oturum açın. Buradan, **Iş yükü-SRV** özel IP adresine bir Uzak Masaüstü bağlantısı açın.

3. Internet Explorer'ı açın ve https://www.microsoft.com adresine gidin.
4. Internet Explorer güvenlik uyarıları 'nda **Tamam** > **Kapat** ' ı seçin.

   Microsoft giriş sayfasını görmeniz gerekir.

5. https://www.google.com adresine gidin.

   Güvenlik duvarının engellemesi gerekir.

Artık Güvenlik Duvarı kurallarının çalıştığını doğruladınız:

* İzin verilen bir FQDN'ye göz atabilir ancak diğerlerine göz atamazsınız.



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Güvenilen güvenlik iş ortakları hakkında bilgi edinin](trusted-security-partners.md)
