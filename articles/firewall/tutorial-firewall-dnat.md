---
title: 'Öğretici: portalı kullanarak gelen Internet trafiğini Azure Güvenlik Duvarı ile filtreleme DNAT'
description: Bu öğreticide Azure portalını kullanarak Azure Güvenlik Duvarı DNAT’yi dağıtmayı ve yapılandırmayı öğreneceksiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 08/28/2020
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 8f528c6be68258400cb3e29582943f1d657c557d
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89069281"
---
# <a name="tutorial-filter-inbound-internet-traffic-with-azure-firewall-dnat-using-the-azure-portal"></a>Öğretici: Azure portal kullanarak gelen Internet trafiğini Azure Güvenlik Duvarı ile filtreleme DNAT

Alt ağlarınıza gelen internet trafiğini çevirmek ve filtrelemek için Azure Güvenlik Duvarı Hedef Ağ Adresi Çevirisi’ni (DNAT) yapılandırabilirsiniz. DNAT yapılandırdığınızda, NAT kuralı toplama eylemi **DNAT**olarak ayarlanır. Daha sonra NAT kural koleksiyonundaki her kural, güvenlik duvarı ortak IP'nizi ve bağlantı noktanızı özel bir IP'ye ve bağlantı noktasına çevirmek için kullanılabilir. DNAT kuralları, çevrilen trafiğe izin verecek ilgili ağ kuralını örtük olarak ekler. Bu davranışı, çevrilen trafikle eşleşen reddetme kuralları olan bir ağ kural koleksiyonunu açıkça ekleyerek geçersiz kılabilirsiniz. Azure Güvenlik Duvarı kural işleme mantığı hakkında daha fazla bilgi için bkz: [Azure Güvenlik Duvarı kural işleme mantığı](rule-processing.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Test amaçlı ağ ortamı oluşturma
> * Güvenlik duvarı dağıtma
> * Varsayılan rota oluşturma
> * DNAT kuralını yapılandırma
> * Güvenlik duvarını test etme

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.



## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.
2. Azure portal giriş sayfasında **kaynak grupları**' nı ve ardından **Ekle**' yi seçin.
3. **Kaynak grubu adı** alanına **RG-DNAT-Test** yazın.
4. **Abonelik** bölümünde aboneliğinizi seçin.
5. **Kaynak grubu konumu** bölümünde bir konum seçin. Bundan sonra oluşturacağınız tüm kaynakların aynı konumda olması gerekir.
6. **Oluştur**’u seçin.

## <a name="set-up-the-network-environment"></a>Ağ ortamını oluşturma

Bu öğretici için eşlenen iki sanal ağ oluşturuyorsunuz:

- **VN-Hub** - güvenlik duvarı bu sanal ağ içindedir.
- **VN-Spoke** - iş yükü sunucusu bu sanal ağ içindedir.

Önce sanal ağları oluşturun, sonra da bunları eşleyin.

### <a name="create-the-hub-vnet"></a>Hub sanal ağını oluşturma

1. Azure portal giriş sayfasında, **tüm hizmetler**' i seçin.
2. **Ağ**altında **sanal ağlar**' ı seçin.
3. **Add (Ekle)** seçeneğini belirleyin.
4. **Ad** için **VN-Hub** yazın.
5. **Adres alanı** için **10.0.0.0/16** yazın.
6. **Abonelik** bölümünde aboneliğinizi seçin.
7. **Kaynak grubu** için **Var olanı kullan**’ı ve **RG-DNAT-Test** girişini seçin.
8. **Konum** alanında önceden kullandığınız konumu seçin.
9. **Alt ağ** bölümünde **Ad** alanına **AzureFirewallSubnet** yazın.

     Güvenlik duvarı bu alt ağda yer alacaktır ve alt ağ adının **mutlaka** AzureFirewallSubnet olması gerekir.
     > [!NOTE]
     > AzureFirewallSubnet alt ağının boyutu/26 ' dır. Alt ağ boyutu hakkında daha fazla bilgi için bkz. [Azure Güvenlik DUVARı SSS](firewall-faq.md#why-does-azure-firewall-need-a-26-subnet-size).

10. **Adres aralığı**için **10.0.1.0/26**yazın.
11. Diğer varsayılan ayarları kullanın ve **Oluştur**' u seçin.

### <a name="create-a-spoke-vnet"></a>Uç sanal ağını oluşturma

1. Azure portal giriş sayfasında, **tüm hizmetler**' i seçin.
2. **Ağ**altında **sanal ağlar**' ı seçin.
3. **Add (Ekle)** seçeneğini belirleyin.
4. **Ad** için **VN-Spoke** yazın.
5. **Adres alanı** için **192.168.0.0/16** yazın.
6. **Abonelik** bölümünde aboneliğinizi seçin.
7. **Kaynak grubu** için **Var olanı kullan**’ı ve **RG-DNAT-Test** girişini seçin.
8. **Konum** alanında önceden kullandığınız konumu seçin.
9. **Alt ağ** altında, **Ad** için **SN-Workload** yazın.

    Sunucu bu alt ağda yer alacaktır.
10. **Adres aralığı** için **192.168.1.0/24** yazın.
11. Diğer varsayılan ayarları kullanın ve **Oluştur**' u seçin.

### <a name="peer-the-vnets"></a>Sanal ağları eşleme

Şimdi iki sanal ağı eşleyin.

1. **Vn hub** sanal ağını seçin.
2. **Ayarlar**altında, eşlemeler ' **i seçin.**
3. **Add (Ekle)** seçeneğini belirleyin.
4. **Vn-hub ile VN-ışınsal arası eşleme adı**için **eşdüzey-hubbağlı bileşen** yazın.
5. Sanal ağ olarak **VN-Spoke**’u seçin.
6. **VN-hub Ile vn hub 'a arasındaki eşleme adı**için **eşdüzey-spokehub** yazın.
7. **VNET 'ten vn-hub ' a yönlendirilen trafiğe Izin ver** için **etkin**' i seçin.
8. **Tamam**’ı seçin.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

İş yükü sanal makinesi oluşturun ve bunu **SN-Workload** alt ağına yerleştirin.

1. Azure portalı menüsünden **Kaynak oluştur**'u seçin.
2. **Popüler**bölümünde **Windows Server 2016 Datacenter**' u seçin.

**Temel Bilgiler**

1. **Abonelik** bölümünde aboneliğinizi seçin.
1. **Kaynak grubu** için **Var olanı kullan**’ı ve **RG-DNAT-Test** girişini seçin.
1. **Sanal makine adı**Için, **SRV-iş yükü**yazın.
1. **Bölge**için, daha önce kullandığınız konumu seçin.
1. Bir kullanıcı adı ve parola girin.
1. **İleri ' yi seçin: diskler**.

**Diskler**
1. **İleri ' yi seçin: ağ**.

**Ağ**

1. **Sanal ağ**Için, **vn-kol**' ı seçin.
2. **Alt ağ** için **SN-Workload**'u seçin.
3. **Genel IP adresi** için **hiçbiri**' ni seçin.
4. **Ortak gelen bağlantı noktaları**için **hiçbiri**' ni seçin. 
2. Diğer varsayılan ayarları bırakın ve Ileri ' **yi seçin: yönetim**.

**Yönetim**

1. **Önyükleme tanılaması**için **kapalı**' yı seçin.
1. **Gözden geçir + oluştur**' u seçin.

**Gözden geçir + oluştur**

Özeti gözden geçirin ve ardından **Oluştur**' u seçin. İşlemin tamamlanması birkaç dakika sürebilir.

Dağıtım bittikten sonra sanal makineyle ilişkili özel IP adresini not alın. Daha sonra güvenlik duvarını yapılandırdığınızda bu adres kullanılacaktır. Sanal makine adını seçin ve **Ayarlar**' ın altında, özel IP adresini bulmak için **ağ** ' ı seçin.

## <a name="deploy-the-firewall"></a>Güvenlik duvarını dağıtma

1. Portal giriş sayfasında, **kaynak oluştur**' u seçin.
2. **Ağ**' ı seçin ve **öne**Çıkdıktan sonra **Tümünü göster**' i seçin.
3. **Güvenlik duvarı**' nı seçin ve ardından **Oluştur**' u seçin. 
4. **Güvenlik duvarı oluştur** sayfasında aşağıdaki ayarları kullanarak güvenlik duvarını yapılandırın:

   |Ayar  |Değer  |
   |---------|---------|
   |Ad     |FW-DNAT-test|
   |Abonelik     |\<your subscription\>|
   |Kaynak grubu     |**Var olanı kullan**: RG-DNAT-Test |
   |Konum     |Önceden kullandığınız konumu seçin|
   |Bir sanal ağ seçin     |**Var olanı kullan**: VN-Hub|
   |Genel IP adresi     |**Yeni oluştur**. Genel IP adresinin türü Standart SKU olmalıdır.|

5. **Gözden geçir ve oluştur**’u seçin.
6. Özeti gözden geçirin ve ardından güvenlik duvarını oluşturmak için **Oluştur** ' u seçin.

   Dağıtma işlemi birkaç dakika sürebilir.
7. Dağıtım tamamlandıktan sonra, **RG-DNAT-test** kaynak grubuna gidin ve **ILT-DNAT-test** güvenlik duvarını seçin.
8. Özel IP adresini not edin. Varsayılan rotayı oluştururken bu adresi kullanacaksınız.

## <a name="create-a-default-route"></a>Varsayılan rota oluşturma

**SN-Workload** alt ağında varsayılan giden rotayı güvenlik duvarından geçecek şekilde yapılandıracaksınız.

1. Azure portal giriş sayfasında, **tüm hizmetler**' i seçin.
2. **Ağ**altında, **Rota tabloları**' nı seçin.
3. **Add (Ekle)** seçeneğini belirleyin.
4. **Ad** için **RT-FWroute** yazın.
5. **Abonelik** bölümünde aboneliğinizi seçin.
6. **Kaynak grubu** için **Var olanı kullan**’ı ve **RG-DNAT-Test** girişini seçin.
7. **Konum** alanında önceden kullandığınız konumu seçin.
8. **Oluştur**’u seçin.
9. **Yenile**' yi seçin ve ardından **RT-fwroute** yol tablosu ' nu seçin.
10. **Alt ağlar**' ı seçin ve ardından **ilişkilendir**' i seçin.
11. **Sanal ağ**' ı seçin ve ardından **vn-kol**' ı seçin.
12. **Alt ağ** için **SN-Workload**'u seçin.
13. **Tamam**’ı seçin.
14. **Rotalar**' ı seçin ve ardından **Ekle**' yi seçin.
15. **Rota adı** alanına **FW-DG** yazın.
16. **Adres ön eki** alanına **0.0.0.0/0** yazın.
17. **Sonraki atlama türü** için **Sanal gereç**'i seçin.

    Azure Güvenlik Duvarı, normalde yönetilen bir hizmettir ancak bu durumda sanal gereç kullanılabilir.
18. **Sonraki atlama adresi** alanına önceden not ettiğiniz güvenlik duvarı özel IP adresini yazın.
19. **Tamam**’ı seçin.

## <a name="configure-a-nat-rule"></a>NAT kuralı yapılandırma

1. **RG-DNAT-test**' i açın ve **ILT-DNAT-test** güvenlik duvarını seçin. 
2. **İlt-DNAT-test** sayfasında, **Ayarlar**' ın altında **kurallar**' ı seçin. 
3. **NAT kuralı koleksiyonu Ekle**' yi seçin. 
4. **Ad** için **RC-DNAT-01** yazın. 
5. **Öncelik** alanına **200** yazın. 
6. **Kurallar**'ın altında, **Ad** için **RL-01** yazın.
7. **Protokol** alanında **TCP**'yi seçin.
8. **Kaynak Adresler** için * yazın. 
9. **Hedef Adresler** için güvenlik duvarının ortak IP adresini yazın. 
10. **Hedef bağlantı noktaları** için **3389** yazın. 
11. **Çevrilmiş Adres** için Srv-Workload sanal makinesinin özel IP adresini yazın. 
12. **Çevrilmiş bağlantı noktası** için **3389** yazın. 
13. **Add (Ekle)** seçeneğini belirleyin. 

## <a name="test-the-firewall"></a>Güvenlik duvarını test etme

1. Güvenlik duvarı genel IP adresine bir uzak masaüstü bağlayın. **Srv-Workload** sanal makinesine bağlı olmalısınız.
2. Uzak masaüstünü kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik duvarı kaynaklarını bir sonraki öğretici için tutabilirsiniz veya artık gerekli değilse **RG-DNAT-Test** kaynak grubunu silerek güvenlik duvarıyla ilgili tüm kaynakları silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Test amaçlı ağ ortamı oluşturma
> * Güvenlik duvarı dağıtma
> * Varsayılan rota oluşturma
> * DNAT kuralını yapılandırma
> * Güvenlik duvarını test etme

Şimdi Azure Güvenlik Duvarı günlüklerini izleyebilirsiniz.

> [!div class="nextstepaction"]
> [Öğretici: Azure Güvenlik Duvarı günlüklerini izleme](./tutorial-diagnostics.md)
