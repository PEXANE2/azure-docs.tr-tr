---
title: 'Öğretici: portalı kullanarak gelen Internet trafiğini Azure Güvenlik Duvarı ile filtreleme DNAT'
description: Bu öğreticide Azure portalını kullanarak Azure Güvenlik Duvarı DNAT’yi dağıtmayı ve yapılandırmayı öğreneceksiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 03/01/2021
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: a1d3bdae1e870b094472a63d4b808d9df95c129d
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101741914"
---
# <a name="tutorial-filter-inbound-internet-traffic-with-azure-firewall-dnat-using-the-azure-portal"></a>Öğretici: Azure portal kullanarak gelen Internet trafiğini Azure Güvenlik Duvarı ile filtreleme DNAT

Alt ağlarınıza gelen internet trafiğini çevirmek ve filtrelemek için Azure Güvenlik Duvarı Hedef Ağ Adresi Çevirisi’ni (DNAT) yapılandırabilirsiniz. DNAT yapılandırdığınızda, NAT kuralı toplama eylemi **DNAT** olarak ayarlanır. NAT kuralı koleksiyonundaki her bir kural, güvenlik duvarı genel IP adresini ve bağlantı noktasını özel bir IP adresine ve bağlantı noktasına dönüştürmek için kullanılabilir. DNAT kuralları, çevrilen trafiğe izin verecek ilgili ağ kuralını örtük olarak ekler. Güvenlik nedenleriyle önerilen yaklaşım, ağ için DNAT erişimine izin vermek ve joker karakter kullanmaktan kaçınmak üzere belirli bir Internet kaynağı eklemektir. Azure Güvenlik Duvarı kural işleme mantığı hakkında daha fazla bilgi için bkz: [Azure Güvenlik Duvarı kural işleme mantığı](rule-processing.md).

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
4. **Abonelik** bölümünde aboneliğinizi seçin.
1. **Kaynak grubu adı** alanına **RG-DNAT-Test** yazın.
5. **Bölge** için bir bölge seçin. Oluşturduğunuz diğer tüm kaynakların aynı bölgede olması gerekir.
6. **Gözden geçir ve oluştur**’u seçin.
1. **Oluştur**’u seçin.

## <a name="set-up-the-network-environment"></a>Ağ ortamını oluşturma

Bu öğretici için eşlenen iki sanal ağ oluşturuyorsunuz:

- **VN-Hub** - güvenlik duvarı bu sanal ağ içindedir.
- **VN-Spoke** - iş yükü sunucusu bu sanal ağ içindedir.

Önce sanal ağları oluşturun, sonra da bunları eşleyin.

### <a name="create-the-hub-vnet"></a>Hub sanal ağını oluşturma

1. Azure portal giriş sayfasında, **tüm hizmetler**' i seçin.
2. **Ağ** altında **sanal ağlar**' ı seçin.
3. **Add (Ekle)** seçeneğini belirleyin.
7. **Kaynak grubu** için **RG-DNAT-test**' i seçin.
1. **Ad** için **VN-Hub** yazın.
1. **Bölge** için, daha önce kullandığınız bölgeyi seçin.
1. **İleri ' yi seçin: IP adresleri**.
1. **IPv4 adres alanı** için varsayılan **10.0.0.0/16** adresini kabul edin.
1. **Alt ağ adı** altında varsayılan ' ı seçin.
1. **Alt ağ adını** düzenleyin ve **AzureFirewallSubnet** yazın.

     Güvenlik duvarı bu alt ağda yer alacaktır ve alt ağ adının **mutlaka** AzureFirewallSubnet olması gerekir.
     > [!NOTE]
     > AzureFirewallSubnet alt ağının boyutu/26 ' dır. Alt ağ boyutu hakkında daha fazla bilgi için bkz. [Azure Güvenlik DUVARı SSS](firewall-faq.yml#why-does-azure-firewall-need-a--26-subnet-size).

10. **Alt ağ adres aralığı** için **10.0.1.0/26** yazın.
11. **Kaydet**’i seçin.
1. **Gözden geçir ve oluştur**’u seçin.
1. **Oluştur**’u seçin.

### <a name="create-a-spoke-vnet"></a>Uç sanal ağını oluşturma

1. Azure portal giriş sayfasında, **tüm hizmetler**' i seçin.
2. **Ağ** altında **sanal ağlar**' ı seçin.
3. **Add (Ekle)** seçeneğini belirleyin.
1. **Kaynak grubu** için **RG-DNAT-test**' i seçin.
1. **Ad** için **VN-Spoke** yazın.
1. **Bölge** için, daha önce kullandığınız bölgeyi seçin.
1. **İleri ' yi seçin: IP adresleri**.
1. **IPv4 adres alanı** için, Varsayılanı düzenleyin ve **192.168.0.0/16** yazın.
1. **Alt ağ ekle**' yi seçin.
1. **Alt ağ adı** için **sn-iş yükü**.
10. **Alt ağ adres aralığı** için **192.168.1.0/24** yazın.
11. **Add (Ekle)** seçeneğini belirleyin.
1. **Gözden geçir ve oluştur**’u seçin.
1. **Oluştur**’u seçin.

### <a name="peer-the-vnets"></a>Sanal ağları eşleme

Şimdi iki sanal ağı eşleyin.

1. **Vn hub** sanal ağını seçin.
2. **Ayarlar** altında, eşlemeler ' **i seçin.**
3. **Add (Ekle)** seçeneğini belirleyin.
4. **Bu sanal ağ** altında, **eşleme bağlantı adı** Için **eşdüzey-hubışınsal** yazın.
5. **Uzak sanal ağ** altında, **eşleme bağlantı adı** Için **eş-spokehub** yazın. 
1. Sanal ağ olarak **VN-Spoke**’u seçin.
1. Diğer tüm varsayılanları kabul edin ve ardından **Ekle**' yi seçin.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

İş yükü sanal makinesi oluşturun ve bunu **SN-Workload** alt ağına yerleştirin.

1. Azure portalı menüsünden **Kaynak oluştur**'u seçin.
2. **Popüler** bölümünde **Windows Server 2016 Datacenter**' u seçin.

**Temel Bilgiler**

1. **Abonelik** bölümünde aboneliğinizi seçin.
1. **Kaynak grubu** için **RG-DNAT-test**' i seçin.
1. **Sanal makine adı** Için, **SRV-iş yükü** yazın.
1. **Bölge** için, daha önce kullandığınız konumu seçin.
1. Bir kullanıcı adı ve parola girin.
1. **İleri ' yi seçin: diskler**.

**Diskler**
1. **Sonraki: Ağ**’ı seçin.

**Ağ**

1. **Sanal ağ** Için, **vn-kol**' ı seçin.
2. **Alt ağ** için **SN-Workload**'u seçin.
3. **Genel IP** için **hiçbiri**' ni seçin.
4. **Ortak gelen bağlantı noktaları** için **hiçbiri**' ni seçin. 
2. Diğer varsayılan ayarları bırakın ve Ileri ' **yi seçin: yönetim**.

**Yönetim**

1. **Önyükleme tanılaması** Için **devre dışı bırak**' ı seçin.
1. **Gözden geçir + Oluştur**’u seçin.

**Gözden geçir + oluştur**

Özeti gözden geçirin ve ardından **Oluştur**' u seçin. İşlemin tamamlanması birkaç dakika sürebilir.

Dağıtım bittikten sonra sanal makineyle ilişkili özel IP adresini not alın. Daha sonra güvenlik duvarını yapılandırdığınızda bu adres kullanılacaktır. Sanal makine adını seçin ve **Ayarlar**' ın altında, özel IP adresini bulmak için **ağ** ' ı seçin.

## <a name="deploy-the-firewall"></a>Güvenlik duvarını dağıtma

1. Portal giriş sayfasında, **kaynak oluştur**' u seçin.
1. **Güvenlik duvarı** araması yapın ve ardından **güvenlik duvarı**' nı seçin.
1. **Oluştur**’u seçin. 
1. **Güvenlik duvarı oluştur** sayfasında aşağıdaki ayarları kullanarak güvenlik duvarını yapılandırın:

   |Ayar  |Değer  |
   |---------|---------|
   |Abonelik     |\<your subscription\>|
   |Kaynak grubu     |**RG-DNAT-test** ' i seçin |
   |Name     |**FW-DNAT-test**|
   |Region     |Önceden kullandığınız konumu seçin|
   |Güvenlik Duvarı yönetimi|**Bu güvenlik duvarını yönetmek için güvenlik duvarı kuralları (klasik) kullanın**|
   |Bir sanal ağ seçin     |**Var olanı kullan**: VN-Hub|
   |Genel IP adresi     |**New**, Name: **FW-PI** ekleyin.|

5. Diğer varsayılanları kabul edin ve ardından **gözden geçir + oluştur**' u seçin.
6. Özeti gözden geçirin ve ardından güvenlik duvarını oluşturmak için **Oluştur** ' u seçin.

   Dağıtma işlemi birkaç dakika sürebilir.
7. Dağıtım tamamlandıktan sonra, **RG-DNAT-test** kaynak grubuna gidin ve **ILT-DNAT-test** güvenlik duvarını seçin.
8. Güvenlik duvarının özel ve genel IP adreslerini aklınızda edin. Bunları daha sonra varsayılan yol ve NAT kuralını oluşturduğunuzda kullanacaksınız.

## <a name="create-a-default-route"></a>Varsayılan rota oluşturma

**SN-Workload** alt ağında varsayılan giden rotayı güvenlik duvarından geçecek şekilde yapılandıracaksınız.

1. Azure portal giriş sayfasında, **tüm hizmetler**' i seçin.
2. **Ağ** altında, **Rota tabloları**' nı seçin.
3. **Add (Ekle)** seçeneğini belirleyin.
5. **Abonelik** bölümünde aboneliğinizi seçin.
1. **Kaynak grubu** için **RG-DNAT-test**' i seçin.
1. **Bölge** için, daha önce kullandığınız bölgeyi seçin.
1. **Ad** için **RT-FWroute** yazın.
1. **Gözden geçir ve oluştur**’u seçin.
1. **Oluştur**’u seçin.
1. **Kaynağa git**’i seçin.
1. **Alt ağlar**' ı seçin ve ardından **ilişkilendir**' i seçin.
1. **Sanal ağ** Için, **vn-kol**' ı seçin.
1. **Alt ağ** için **SN-Workload**'u seçin.
1. **Tamam**’ı seçin.
1. **Rotalar**' ı seçin ve ardından **Ekle**' yi seçin.
1. **Rota adı** alanına **FW-DG** yazın.
1. **Adres ön eki** alanına **0.0.0.0/0** yazın.
1. **Sonraki atlama türü** için **Sanal gereç**'i seçin.

    Azure Güvenlik Duvarı, normalde yönetilen bir hizmettir ancak bu durumda sanal gereç kullanılabilir.
18. **Sonraki atlama adresi** alanına önceden not ettiğiniz güvenlik duvarı özel IP adresini yazın.
19. **Tamam**’ı seçin.

## <a name="configure-a-nat-rule"></a>NAT kuralı yapılandırma

1. **RG-DNAT-test** kaynak grubunu açın ve **ILT-DNAT-test** güvenlik duvarını seçin. 
2. **İlt-DNAT-test** sayfasında, **Ayarlar**' ın altında, kurallar ' ı **(klasik)** seçin. 
3. **NAT kuralı koleksiyonu Ekle**' yi seçin. 
4. **Ad** için **RC-DNAT-01** yazın. 
5. **Öncelik** alanına **200** yazın. 
6. **Kurallar**'ın altında, **Ad** için **RL-01** yazın.
7. **Protokol** alanında **TCP**'yi seçin.
1. **Kaynak türü** için **IP adresi**' ni seçin.
1. **Kaynak** için * yazın. 
1. **Hedef adresleri** için güvenlik DUVARıNıN genel IP adresini yazın. 
1. **Hedef bağlantı noktaları** için **3389** yazın. 
1. **Çevrilmiş Adres** için Srv-Workload sanal makinesinin özel IP adresini yazın. 
1. **Çevrilmiş bağlantı noktası** için **3389** yazın. 
1. **Add (Ekle)** seçeneğini belirleyin. İşlemin tamamlanması birkaç dakika sürebilir.

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
> [Öğretici: Azure Güvenlik Duvarı günlüklerini izleme](./firewall-diagnostics.md)
