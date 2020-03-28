---
title: "Öğretici: Azure Güvenlik Duvarı Yöneticisi önizlemesini kullanarak sanal WAN'ınızı güvenli hale"
description: Bu eğitimde, Azure portalını kullanarak Azure Güvenlik Duvarı Yöneticisi ile sanal WAN'ınızı nasıl güvene alacağınız öğrenilir.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: tutorial
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: 3dc94a8be265682fbe2128f2e5870dfdf5850a2d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77443066"
---
# <a name="tutorial-secure-your-virtual-wan-using-azure-firewall-manager-preview"></a>Öğretici: Azure Güvenlik Duvarı Yöneticisi önizlemesini kullanarak sanal WAN'ınızı güvenli hale 

[!INCLUDE [Preview](../../includes/firewall-manager-preview-notice.md)]

Azure Güvenlik Duvarı Yöneticisi Önizleme'yi kullanarak, özel IP adreslerine, Azure PaaS'a ve Internet'e yönelik bulut ağı trafiğinizi güvence altına almak için güvenli sanal hub'lar oluşturabilirsiniz. Güvenlik duvarına giden trafik yönlendirmesi otomatiktir, bu nedenle kullanıcı tanımlı rotalar (ÜDR) oluşturmaya gerek yoktur.

![bulut ağını güvenli hale](media/secure-cloud-network/secure-cloud-network.png)

Güvenlik Duvarı Yöneticisi ayrıca hub sanal ağ mimarisini de destekler. Güvenli sanal hub ve hub sanal ağ mimarisi türlerinin karşılaştırması için Azure [Güvenlik Duvarı Yöneticisi mimari seçenekleri nelerdir'](vhubs-and-vnets.md) e bakın?

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Konuşan sanal ağı oluşturma
> * Güvenli bir sanal hub oluşturma
> * Hub'ı ve kollu VNet'leri bağlayın
> * Güvenlik duvarı ilkesi oluşturun ve hub'ınızı güvenli hale
> * Trafiği merkezinize yönlendirme
> * Güvenlik duvarını test etme

## <a name="create-a-hub-and-spoke-architecture"></a>Hub ve kollu mimari oluşturma

İlk olarak, sunucularınızı yerleştirebileceğiniz bir kollu VNet oluşturun.

### <a name="create-a-spoke-vnet-and-subnets"></a>Kollu VNet ve alt ağlar oluşturma

1. Azure portalı giriş sayfasından **kaynak oluştur'u**seçin.
2. **Ağ**altında, **Sanal ağ**seçin.
4. **Ad**için , **spoke-01**yazın.
5. **Adres alanı** için **10.0.0.0/16** yazın.
6. **Abonelik** bölümünde aboneliğinizi seçin.
7. **Kaynak grubu**için **yeni oluştur'u**seçin ve ad için **FW-Manager** yazın ve **Tamam'ı**seçin.
8. **Konum**için **(ABD) Doğu ABD'yi**seçin.
9. **Alt net**altında, **Ad** türü **İş Yükü-SN**için.
10. **Adres aralığı** için **10.0.1.0/24** yazın.
11. Diğer varsayılan ayarları kabul edin ve sonra **Oluştur'u**seçin.

Ardından, atlama sunucusu için bir alt ağ oluşturun.

1. Azure portalı giriş sayfasında Kaynak **grupları** > **FW-Manager'ı**seçin.
2. **Spoke-01** sanal ağını seçin.
3. **Alt ağlar** > **+Subnet'i**seçin.
4. **Ad**için , **Jump-SN**yazın.
5. **Adres aralığı** için **10.0.2.0/24** yazın.
6. **Tamam'ı**seçin.

### <a name="create-the-secured-virtual-hub"></a>Güvenli sanal hub'ı oluşturma

Güvenlik Duvarı Yöneticisi'ni kullanarak güvenli sanal hub'ınızı oluşturun.

1. Azure portalı giriş sayfasından **Tüm hizmetleri**seçin.
2. Arama kutusunda, **Güvenlik Duvarı Yöneticisi** yazın ve Güvenlik Duvarı **Yöneticisi'ni**seçin.
3. Güvenlik **Duvarı Yöneticisi** sayfasında **Güvenli Sanal Hub Oluştur'u**seçin.
4. Yeni **Güvenli Sanal Hub Oluştur** sayfasında aboneliğinizi ve **FW-Manager** kaynak grubunu seçin.
5. Güvenli **sanal hub adı için** **Hub-01**yazın.
6. **Konum**için Doğu **ABD'yi**seçin.
7. **Hub adresi alanı**için **10.1.0.0/16**yazın.
8. Yeni vWAN adı için **vwan-01**yazın.
9. Güvenilir Güvenlik Ortakları onay kutusunun **temizlenmesini sağlamak için VPN ekle ağ geçidini** bırakın.
10. **Sonraki'ni seçin:Azure Güvenlik Duvarı.**
11. Varsayılan **Azure Güvenlik Duvarı** **Etkin** ayarını kabul edin ve ardından **Sonraki: Güvenilen Güvenlik Ortağı'nı**seçin.
12. Varsayılan **Güvenilen Güvenlik Ortağı** **Devre Dışı ayarını** kabul edin ve **Sonraki: Gözden Geçir + oluştur'u**seçin.
13. **Oluştur'u**seçin. Konuşlandırmak yaklaşık 30 dakika sürer.

### <a name="connect-the-hub-and-spoke-vnets"></a>Hub'ı ve kollu VNet'leri bağlayın

Artık hub'ı eşleyebilir ve VNets'i konuşturabilirsiniz.

1. **FW-Manager** kaynak grubunu seçin ve **ardından vwan-01** sanal WAN'ı seçin.
2. **Bağlantı**altında, **Sanal ağ bağlantılarını**seçin.
3. **Bağlantı Ekle'yi**seçin.
4. **Bağlantı adı**için **hub-spoke**yazın.
5. **Hub'lar**için **Hub-01'i**seçin.
6. **Sanal ağ**için **Spoke-01'i**seçin.
7. **Tamam'ı**seçin.

## <a name="create-a-firewall-policy-and-secure-your-hub"></a>Güvenlik duvarı ilkesi oluşturun ve hub'ınızı güvenli hale

Güvenlik duvarı ilkesi, bir veya daha fazla Güvenli sanal hub'daki trafiği yönlendirmek için kural koleksiyonlarını tanımlar. Güvenlik duvarı ilkenizi oluşturur ve hub'ınızı güvenli hale getirirsiniz.

1. Güvenlik Duvarı Yöneticisi'nden **Azure Güvenlik Duvarı İlkesi Oluştur'u**seçin.
2. Aboneliğinizi seçin ve ardından **FW-Manager** kaynak grubunu seçin.
3. **İlke ayrıntıları**altında, **Ad** türü **İlke-01** ve **Bölge** için **Doğu ABD**seçin.
4. **Sonraki:Kuralları**seçin.
5. **Kurallar** sekmesinde, **kural koleksiyonu ekle'yi**seçin.
6. Kural **toplama** sayfası ekle'de **Ad**için **RC-01** yazın.
7. **Kural toplama türü için** **Uygulama'yı**seçin.
8. **Öncelik**için, **100**yazın.
9. **Kural toplama eyleminin** **İzin Ver**olduğundan emin olun.
10. Kural **Adı** türü için **İzin ver-msft**.
11. **Kaynak adresi**için **\***, yazın.
12. **Protokol**için , yazın **http,https**.
13. **Hedef türü **FQDN**olduğundan emin olun.
14. **Hedef**için ** \*,microsoft.com**yazın.
15. **Ekle'yi**seçin.
16. **Sonraki'ni Seçin: Güvenli sanal hub'lar.**
17. Güvenli **sanal hub'lar** sekmesinde **Hub-01'i**seçin.
19. **İncele ve oluştur**’u seçin.
20. **Oluştur'u**seçin.

Bu tamamlamak için yaklaşık beş dakika veya daha fazla sürebilir.

## <a name="route-traffic-to-your-hub"></a>Trafiği merkezinize yönlendirme

Artık ağ trafiğinin güvenlik duvarınızdan yönlendirildiğından emin olmalısınız.

1. Güvenlik Duvarı Yöneticisi'nden **Güvenli sanal hub'ları**seçin.
2. **Hub-01'i**seçin.
3. **Ayarlar**altında **Rota ayarlarını**seçin.
4. **Internet trafiği**altında , **Sanal Ağlardan Trafik**, Azure Güvenlik Duvarı üzerinden **gönder'i**seçin.
5. **Azure özel trafiği**altında , Sanal **Ağlara Trafik**, Azure Güvenlik Duvarı üzerinden **gönder'i**seçin.
6. **IP adresi öneki(es)** seçeneğini belirleyin.
7. **IP adresi öneki ekle'yi**seçin.
8. İş Yükü alt ağının adresi olarak **10.0.1.0/24** yazın ve **Kaydet'i**seçin.
9. **Ayarlar** **altında, Bağlantılar'ı**seçin.
10. **Hub'da konuşan** bağlantıyı seçin ve ardından **Internet trafiğini güvenli** olarak seçin ve ardından **Tamam'ı**seçin.


## <a name="test-your-firewall"></a>Güvenlik duvarınızı test edin

Güvenlik duvarı kurallarınızı sınamak için birkaç sunucu dağıtmanız gerekir. Güvenlik duvarı kurallarını test etmek için Workload-SN alt ağına İş Yükü-Srv'yi ve Internet'ten bağlanmak için Uzak Masaüstü'nü kullanıp ardından İş Yükü-Srv'ye bağlanmak için Jump-Srv'yi dağıtacaksınız.

### <a name="deploy-the-servers"></a>Sunucuları dağıtma

1. Azure portalında **kaynak oluştur'u**seçin.
2. **Popülerler** listesinde **Windows Server 2016 Datacenter'ı** seçin.
3. Sanal makine için şu değerleri girin:

   |Ayar  |Değer  |
   |---------|---------|
   |Kaynak grubu     |**FW Yöneticisi**|
   |Sanal makine adı     |**Jump-Srv**|
   |Bölge     |**(Abd) Doğu ABD)**|
   |Yönetici kullanıcı adı     |**azureuser**|
   |Parola     |parolanızı yazın|

4. **Gelen bağlantı noktası kuralları altında,** Ortak gelen bağlantı **noktaları**için, **seçili bağlantı noktalarına izin ver'i**seçin.
5. **Gelen bağlantı noktalarını seçmek**için **RDP (3389)** seçeneğini belirleyin.

6. Diğer varsayılanları kabul edin ve **Sonraki: Diskler'i**seçin.
7. Disk varsayılanlarını kabul edin ve **Sonraki: Ağ' ı**seçin.
8. **Spoke-01'in** sanal ağ için seçildiğinden ve alt ağın **Jump-SN**olduğundan emin olun.
9. **Genel IP**için varsayılan yeni genel ip adresi adını (Jump-Srv-ip) kabul edin.
11. Diğer varsayılanları kabul edin ve **Sonraki: Yönetim'i**seçin.
12. Önyükleme tanılamasını devre dışı kalmak için **Kapalı'yı** seçin. Diğer varsayılanları kabul edin ve **Gözden Geçir + oluştur'u**seçin.
13. Özet sayfasındaki ayarları gözden geçirin ve ardından **Oluştur'u**seçin.

**İş Yükü-Srv**adlı başka bir sanal makineyi yapılandırmak için aşağıdaki tablodaki bilgileri kullanın. Yapılandırmanın geri kalanı Srv-Jump sanal makinesiyle aynıdır.

|Ayar  |Değer  |
|---------|---------|
|Alt ağ|**Workload-SN**|
|Genel IP|**Yok**|
|Genel gelen bağlantı noktaları|**Yok**|

### <a name="add-a-route-table-and-default-route"></a>Rota tablosu ve varsayılan rota ekleme

Jump-Srv'ye Internet bağlantısı sağlamak için, **Jump-SN** alt sonundan Bir rota tablosu ve Internet'e varsayılan ağ geçidi rotası oluşturmanız gerekir.

1. Azure portalında **kaynak oluştur'u**seçin.
2. Arama kutusuna **rota tablosu** yazın ve ardından **Rota tablosunu**seçin.
3. **Oluştur'u**seçin.
4. **Ad**için **RT-01** yazın.
5. Aboneliğinizi, kaynak grubu için **FW-Manager'ı** ve bölge için **(ABD) Doğu ABD'yi** seçin.
6. **Oluştur'u**seçin.
7. Dağıtım tamamlandığında **RT-01** rota tablosunu seçin.
8. **Rotalar'ı** seçin ve ardından **Ekle'yi**seçin.
9. **Rota adı**için **jump-to-inet** yazın.
10. **Adres öneki**için **0.0.0.0/0** yazın.
11. Sonraki atlama türü için **Internet'i**seçin. **Internet**
12. **Tamam'ı**seçin.
13. Dağıtım tamamlandığında, Alt **Ağlar'ı**seçin ve ardından **Ilişkilendir'i**seçin.
14. Sanal ağ için **Virtual network** **Spoke-01'i** seçin.
15. **Subnet**için **Jump-SN'yi** seçin.
16. **Tamam'ı**seçin.

### <a name="test-the-rules"></a>Kuralları test edin

Beklendiği gibi çalıştığını doğrulamak için güvenlik duvarı kurallarını test edin.

1. Azure **portalından, İş Yükü-Srv** sanal makinesinin ağ ayarlarını inceleyin ve özel IP adresine dikkat edin.
2. Uzak bir masaüstünü **Jump-Srv** sanal makinesine bağlayın ve oturum açın. Buradan, **Workload-Srv** özel IP adresine uzak bir masaüstü bağlantısı açın.

3. Internet Explorer'ı açın ve https://www.microsoft.com adresine gidin.
4. Internet Explorer güvenlik uyarılarında **Tamam** > **Kapat'ı** seçin.

   Microsoft giriş sayfasını görmeniz gerekir.

5. https://www.google.com adresine gidin.

   Güvenlik duvarının engellemesi gerekir.

Şimdi güvenlik duvarı kurallarının işe yaradığını doğruladınız:

* İzin verilen bir FQDN'ye göz atabilir ancak diğerlerine göz atamazsınız.



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Güvenilir güvenlik ortakları hakkında bilgi edinin](trusted-security-partners.md)
