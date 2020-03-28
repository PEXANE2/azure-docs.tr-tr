---
title: 'Öğretici: Portal kullanarak oluşturun - Web Uygulaması Güvenlik Duvarı'
description: Bu eğitimde, Azure portalını kullanarak Web Uygulaması Güvenlik Duvarı ile uygulama ağ geçidi oluşturmayı öğrenirsiniz.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.topic: tutorial
ms.date: 11/14/2019
ms.author: victorh
ms.openlocfilehash: 68a9f051bf3d59cbf32377cb503e9ded0a54d5e9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74049230"
---
# <a name="tutorial-create-an-application-gateway-with-a-web-application-firewall-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak Web Uygulaması Güvenlik Duvarı ile bir uygulama ağ geçidi oluşturma

Bu öğretici, Web Uygulaması Güvenlik Duvarı (WAF) ile bir Uygulama Ağ Geçidi oluşturmak için Azure portalını nasıl kullanacağınızı gösterir. WAF, uygulamanızı korumak için [OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) kurallarını kullanır. Bu kurallar SQL ekleme, siteler arası betik saldırıları ve oturum ele geçirme gibi saldırılara karşı korumayı içerir. Uygulama ağ geçidini oluşturduktan sonra, doğru çalıştığından emin olmak için test ekidirsiniz. Azure Application Gateway ile, dinleyicileri bağlantı noktalarına atayarak, kurallar oluşturarak ve arka uç havuzuna kaynak ekleyerek uygulama web trafiğinizi belirli kaynaklara yönlendirirsiniz. Basitlik uğruna, bu öğretici bir ortak ön uç IP, bu uygulama ağ geçidi, iki sanal makine arka uç havuzu için kullanılan tek bir site barındırmak için temel bir dinleyici ve temel bir istek yönlendirme kuralı ile basit bir kurulum kullanır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * WAF etkinken bir uygulama ağ geçidi oluşturma
> * Arka uç sunucusu olarak kullanılan sanal makineleri oluşturma
> * Bir depolama hesabı oluşturma ve tanılamaları yapılandırma
> * Uygulama ağ geçidini test etme

![Web uygulaması güvenlik duvarı örneği](../media/application-gateway-web-application-firewall-portal/scenario-waf.png)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

<!---If you prefer, you can complete this tutorial using [Azure PowerShell](tutorial-restrict-web-traffic-powershell.md) or [Azure CLI](tutorial-restrict-web-traffic-cli.md).--->

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Azure'un kaynaklar arasında iletişim kurabilmesi için sanal bir ağa ihtiyacı vardır. Yeni bir sanal ağ oluşturabilir veya varolan bir ağ kullanabilirsiniz. Bu örnekte, yeni bir sanal ağ oluşturursunuz. Uygulama ağ geçidini oluştururken aynı zamanda bir sanal makine oluşturabilirsiniz. Uygulama Ağ Geçidi örnekleri ayrı alt ağlarda oluşturulur. Bu örnekte iki alt ağ oluşturursunuz: biri uygulama ağ geçidi için, diğeri de arka uç sunucuları için.

Azure portalının sol menüsünde **kaynak oluştur'u** seçin. **Yeni** pencere görüntülenir.

**Ağ Ing'i** seçin ve ardından **Öne Çıkanlar** listesinde **Uygulama Ağ Geçidi'ni** seçin.

### <a name="basics-tab"></a>Temel bilgiler sekmesi

1. Temel **bilgiler** sekmesinde, aşağıdaki uygulama ağ geçidi ayarları için aşağıdaki değerleri girin:

   - **Kaynak grubu**: Kaynak grubu için **myResourceGroupAG'ı** seçin. Yoksa, oluşturmak için **yeni oluştur'u** seçin.
   - **Uygulama ağ geçidi adı**: Uygulama ağ geçidinin adı için *myAppGateway'i* girin.
   - **Katman**: **WAF V2'yi**seçin.

     ![Yeni uygulama ağ geçidi oluşturma: Temel bilgiler](../media/application-gateway-web-application-firewall-portal/application-gateway-create-basics.png)

2.  Azure'un oluşturduğunuz kaynaklar arasında iletişim kurabilmesi için sanal bir ağa ihtiyacı vardır. Yeni bir sanal ağ oluşturabilir veya varolan bir ağ kullanabilirsiniz. Bu örnekte, uygulama ağ geçidini oluşturduğunuz anda yeni bir sanal ağ oluşturursunuz. Uygulama Ağ Geçidi örnekleri ayrı alt ağlarda oluşturulur. Bu örnekte iki alt ağ oluşturursunuz: biri uygulama ağ geçidi için, diğeri de arka uç sunucuları için.

    **Yapılandırılan sanal ağ**altında, **yeni oluştur'u**seçerek yeni bir sanal ağ oluşturun. Açılan **sanal ağ oluştur** penceresinde, sanal ağ ve iki alt ağ oluşturmak için aşağıdaki değerleri girin:

    - **Adı**: Sanal ağ adı için *myVNet* girin.

    - **Alt ağ adı** (Application Gateway subnet): **Alt ağlar** *ızgarası Varsayılan*adlı bir alt ağ gösterir. Bu alt netin adını *myAGSubnet*olarak değiştirin.<br>Uygulama ağ geçidi alt ağı yalnızca uygulama ağ geçitleri içerebilir. Başka kaynaklara izin verilmez.

    - **Alt ağ adı** (arka uç sunucu alt ağı): **Alt ağlar** ızgarasının ikinci satırında, **Subnet ad** sütununa *myBackendSubnet* girin.

    - **Adres aralığı** (arka uç sunucu alt ağı): **Alt ağlar** ızgarasının ikinci satırında, *myAGSubnet'in*adres aralığıyla çakışan bir adres aralığı girin. Örneğin, *myAGSubnet'in* adres aralığı 10.0.0.0/24 ise, *myBackendSubnet*adres aralığı için *10.0.1.0/24* girin.

    **Sanal ağ oluştur** penceresini kapatmak ve sanal ağ ayarlarını kaydetmek için **Tamam'ı** seçin.

     ![Yeni uygulama ağ geçidi oluşturun: sanal ağ](../media/application-gateway-web-application-firewall-portal/application-gateway-create-vnet.png)
    
3. Temel **Ayarlar** sekmesinde, diğer ayarlar için varsayılan değerleri kabul edin ve sonra **İleri: Frontends'i**seçin.

### <a name="frontends-tab"></a>Frontends sekmesi

1. **Frontends** sekmesinde, **Frontend IP adresi türünün** **Genel**olarak ayarlı olduğunu doğrulayın. <br>Frontend IP'yi kullanım durumunuza göre Herkese Açık veya Özel olarak yapılandırabilirsiniz. Bu örnekte, bir Public Frontend IP'si seçersiniz.
   > [!NOTE]
   > Application Gateway v2 SKU için yalnızca **Public** frontend IP yapılandırmasını seçebilirsiniz. Bu v2 SKU için özel ön uç IP yapılandırması şu anda etkinleştirilemesin.

2. **Genel IP adresi** için yeni **oluştur'u** seçin ve genel IP adresi adı için *myAGPublicIPAddress'i* girin ve ardından **Tamam'ı**seçin. 

     ![Yeni uygulama ağ geçidi oluşturma: frontends](../media/application-gateway-web-application-firewall-portal/application-gateway-create-frontends.png)

3. **Sonraki'ni Seçin: Arka uçlar.**

### <a name="backends-tab"></a>Arka uçlar sekmesi

Arka uç havuzu, istekleri sağlayan arka uç sunucularına yönlendirmek için kullanılır. Arka uç havuzları NIC'lerden, sanal makine ölçek kümelerinden, genel IP'lerden, dahili IP'lerden, tam nitelikli alan adlarından (FQDN) ve Azure Uygulama Hizmeti gibi çok kiracılı arka uçlardan oluşabilir. Bu örnekte, uygulama ağ geçidiniz le boş bir arka uç havuzu oluşturur ve sonra arka uç havuzuna arka uç hedefleri eklersiniz.

1. Arka **uçlar** sekmesinde **+Arka uç havuzu ekle'yi**seçin.

2. Açılan **bir arka uç havuzu ekle** penceresinde, boş bir arka uç havuzu oluşturmak için aşağıdaki değerleri girin:

    - **Adı**: Arka uç havuzunun adı için *myBackendPool'u* girin.
    - **Hedefsiz arka uç havuzu ekleyin**: Hedefsiz bir arka uç havuzu oluşturmak için **Evet'i** seçin. Uygulama ağ geçidini oluşturduktan sonra arka uç hedefleri eklersiniz.

3. Arka **uç havuzu ekle** penceresinde, arka uç havuzu yapılandırmasını kaydetmek ve **Arka Uçlar** sekmesine dönmek için **Ekle'yi** seçin.

     ![Yeni uygulama ağ geçidi oluşturma: arka uçlar](../media/application-gateway-web-application-firewall-portal/application-gateway-create-backends.png)

4. Arka **uçlar** sekmesinde **İleri: Yapılandırma'yı**seçin.

### <a name="configuration-tab"></a>Yapılandırma sekmesi

**Yapılandırma** sekmesinde, yönlendirme kuralı nı kullanarak oluşturduğunuz ön ve arka uç havuzunu bağlarsınız.

1. **Yönlendirme kuralları** sütununda **kural ekle'yi** seçin.

2. Açılan **yönlendirme kuralı** ekle penceresinde, **Kural adı**için *myRoutingRule'i* girin.

3. Yönlendirme kuralı dinleyici gerektirir. **Yönlendirme kuralı** ekle penceresindeki **Dinleyici** sekmesine, dinleyici için aşağıdaki değerleri girin:

    - **Dinleyici adı**: Dinleyicinin adı için *myListener* girin.
    - **Frontend IP**: Ön uç için oluşturduğunuz genel IP'yi seçmek için **Genel'i** seçin.
  
      **Dinleyici** sekmesindeki diğer ayarlar için varsayılan değerleri kabul edin ve yönlendirme kuralının geri kalanını yapılandırmak için **Arka Uç hedefleri** sekmesini seçin.

   ![Yeni uygulama ağ geçidi oluşturun: dinleyici](../media/application-gateway-web-application-firewall-portal/application-gateway-create-rule-listener.png)

4. Arka **uç hedefleri** sekmesinde, **Backend hedefi**için **myBackendPool'u** seçin.

5. HTTP **ayarı**için, yeni bir HTTP ayarı oluşturmak için **yeni oluştur'u** seçin. HTTP ayarı yönlendirme kuralının davranışını belirler. Açılan **bir HTTP ayar penceresi ekle** penceresinde, HTTP ayar **adı**için *benimHTTPSetting* girin. **HTTP ayar ekle** penceresindeki diğer ayarlar için varsayılan değerleri kabul edin ve ardından yönlendirme **kuralı** ekle penceresine dönmek için **Ekle'yi** seçin. 

     ![Yeni uygulama ağ geçidi oluşturma: HTTP ayarı](../media/application-gateway-web-application-firewall-portal/application-gateway-create-httpsetting.png)

6. Yönlendirme **kuralı ekle** penceresinde, yönlendirme kuralını kaydetmek ve **Yapılandırma** sekmesine dönmek için **Ekle'yi** seçin.

     ![Yeni uygulama ağ geçidi oluşturma: yönlendirme kuralı](../media/application-gateway-web-application-firewall-portal/application-gateway-create-rule-backends.png)

7. **Sonraki seçin: Etiketler** ve sonra **Sonraki: Gözden Geçirme + oluştur**.

### <a name="review--create-tab"></a>İnceleme + oluşturma sekmesi

**Gözden Geçir + oluştur** sekmesindeki ayarları gözden geçirin ve ardından sanal ağı, genel IP adresini ve uygulama ağ geçidini oluşturmak için **Oluştur'u** seçin. Azure'un uygulama ağ geçidini oluşturması birkaç dakika sürebilir. 

Sonraki bölüme geçmeden önce dağıtımın başarıyla bitmesini bekleyin.

## <a name="add-backend-targets"></a>Arka uç hedefleri ekleme

Bu örnekte, hedef arka uç olarak sanal makineleri kullanırsınız. Varolan sanal makineleri kullanabilir veya yenilerini oluşturabilirsiniz. Azure'un uygulama ağ geçidi için arka uç sunucusu olarak kullandığı iki sanal makine oluşturursunuz.

Bunu yapmak için şunları yapacaksınız:

1. Arka uç sunucuları olarak kullanılmak üzere iki yeni VM, *myVM* ve *myVM2*oluşturun.
2. Uygulama ağ geçidinin başarıyla oluşturulduğunu doğrulamak için sanal makinelere IIS yükleyin.
3. Arka uç sunucularını arka uç havuzuna ekleyin.

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalında **kaynak oluştur'u**seçin. **Yeni** pencere görüntülenir.
2. **Popülerler** listesinde **Windows Server 2016 Datacenter'ı** seçin. **Sanal makine oluştur** sayfası görüntülenir.<br>Uygulama Ağ Geçidi, trafiği arka uç havuzunda kullanılan herhangi bir sanal makine türüne yönlendirebilir. Bu örnekte, bir Windows Server 2016 Datacenter kullanıyorsunuz.
3. Aşağıdaki sanal makine ayarları için **Temeller** sekmesine bu değerleri girin:

    - **Kaynak grubu**: Kaynak grubu adı için **myResourceGroupAG'ı** seçin.
    - **Sanal makine adı**: Sanal makinenin adı için *myVM* girin.
    - **Kullanıcı Adı**: Yönetici kullanıcı adı için *azureuser* girin.
    - **Şifre**: *Azure123456 girin!* yönetici parolası için.
4. Diğer varsayılanları kabul edin ve sonra **İleri: Diskler'i**seçin.  
5. **Diskler** sekmesi varsayılanlarını kabul edin ve sonra **Sonraki: Ağ'ı**seçin.
6. **Ağ** sekmesinde, **myVNet'in** **Sanal ağ** için seçildiğini ve **Alt Net'in** **myBackendSubnet**olarak ayarlı olduğunu doğrulayın. Diğer varsayılanları kabul edin ve sonra **Sonraki: Yönetim'i**seçin.<br>Uygulama Ağ Geçidi, içinde olduğu sanal ağın dışındaki örneklerle iletişim kurabilir, ancak IP bağlantısı olduğundan emin olmanız gerekir.
7. **Yönetim** sekmesinde, **Önyükleme tanılamasını** **Kapalı**olarak ayarlayın. Diğer varsayılanları kabul edin ve ardından **Gözden Geçir + oluştur'u**seçin.
8. Gözden **Geçir + oluştur** sekmesinde, ayarları gözden geçirin, doğrulama hatalarını düzeltin ve sonra **Oluştur'u**seçin.
9. Devam etmeden önce sanal makine oluşturmanın tamamlanmasını bekleyin.

### <a name="install-iis-for-testing"></a>Test için IIS'yi yükleyin

Bu örnekte, iIS'yi sanal makinelere yalnızca Azure'un uygulama ağ geçidini başarıyla oluşturduğunu doğrulamak için yüklersiniz.

1. [Azure PowerShell'i](https://docs.microsoft.com/azure/cloud-shell/quickstart-powershell)açın. Bunu yapmak için Azure portalının en üst teki gezinme çubuğundan **Cloud Shell'i** seçin ve ardından açılan listeden **PowerShell'i** seçin. 

    ![Özel uzantıyı yükleme](../media/application-gateway-web-application-firewall-portal/application-gateway-extension.png)

2. Sanal makineye IIS yüklemek için aşağıdaki komutu çalıştırın: 

    ```azurepowershell-interactive
    Set-AzVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -ExtensionName IIS `
      -VMName myVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
      -Location EastUS
    ```

3. İkinci bir sanal makine oluşturun ve daha önce tamamladığınız adımları kullanarak IIS'yi yükleyin. *MyVM2'yi* sanal makine adı ve **Set-AzVMExtension** cmdlet'in **VMName** ayarı için kullanın.

### <a name="add-backend-servers-to-backend-pool"></a>Arka uç havuzuna arka uç sunucuları ekleme

1. **Tüm kaynakları**seçin ve ardından **myAppGateway'i**seçin.

2. Sol menüden **Arka Uç havuzları'nı** seçin.

3. **myBackendPool'u**seçin.

4. **Hedefler**altında açılan listeden **Sanal makineyi** seçin.

5. **SANAL MACHINE** VE **NETWORK INTERFACES**altında, açılan listelerden **myVM** ve **myVM2** sanal makineleri ve ilişkili ağ arayüzlerini seçin.

    ![Arka uç sunucuları ekleme](../media/application-gateway-web-application-firewall-portal/application-gateway-backend.png)

6. **Kaydet'i**seçin.

7. Bir sonraki adıma geçmeden önce dağıtımın tamamlanmasını bekleyin.

## <a name="create-a-storage-account-and-configure-diagnostics"></a>Bir depolama hesabı oluşturma ve tanılamaları yapılandırma

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Bu makale için, uygulama ağ geçidi algılama ve önleme amacıyla verileri depolamak için bir depolama hesabı kullanır. Verileri kaydetmek için Azure Monitor günlüklerini veya Etkinlik Hub'Larını da kullanabilirsiniz.

1. Azure portalının sol üst köşesinde **kaynak oluştur'u** seçin.
1. **Depolama'yı**seçin ve ardından **Depolama hesabı'nı**seçin.
1. *Kaynak grubu*için kaynak grubu için **myResourceGroupAG'ı** seçin.
1. Depolama hesabının adı için *myagstore1* yazın.
1. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Gözden Geçir + Oluştur'u**seçin.
1. Ayarları gözden geçirin ve ardından **Oluştur'u**seçin.

### <a name="configure-diagnostics"></a>Tanılama yapılandırma

Tanılamayı ApplicationGatewayAccessLog, ApplicationGatewayPerformanceLog ve ApplicationGatewayFirewallLog günlüklerine verileri kaydedecek şekilde yapılandırın.

1. Sol menüde **Tüm kaynaklar'ı**seçin ve ardından *myAppGateway'i*seçin.
2. İzleme altında **Tanılama ayarlarını**seçin.
3. **tanılama ayarını ekle'yi**seçin.
4. Tanılama ayarlarının adı olarak *myDiagnosticsSettings'i* girin.
5. **Depolama hesabına Arşiv'i**seçin ve daha önce oluşturduğunuz *myagstore1* depolama hesabını seçmek için **Yapıya'yı** seçin ve ardından **Tamam'ı**seçin.
6. Toplamak ve tutmak için uygulama ağ geçidi günlüklerini seçin.
7. **Kaydet'i**seçin.

    ![Tanılama yapılandırma](../media/application-gateway-web-application-firewall-portal/application-gateway-diagnostics.png)

## <a name="create-and-link-a-web-application-firewall-policy"></a>Bir Web Uygulaması Güvenlik Duvarı ilkesi oluşturma ve bağlama

TÜM WAF özelleştirmeleri ve ayarları, WAF İlkesi adı verilen ayrı bir nesnededir. İlke, Uygulama Ağ Geçidinizle ilişkilendirilmelidir. WAF İlkesi oluşturmak için [bkz.](create-waf-policy-ag.md) Oluşturulduktan sonra, politikayı **Ilişkili Uygulama Ağ Geçitleri** sekmesindeki WAF İlkesi'nden WAF'ınızla (veya tek bir dinleyiciyle) ilişkilendirebilirsiniz. 

![İlişkili uygulama ağ geçitleri](../media/application-gateway-web-application-firewall-portal/associated-application-gateways.png)

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Uygulama ağ geçidini oluşturmak için IIS gerekmese de, Azure'un uygulama ağ geçidini başarıyla oluşturup oluşturmadığını doğrulamak için bu ağ geçidini yükledin. Uygulama ağ geçidini test etmek için IIS'yi kullanın:

1. **Genel Bakış** sayfasında uygulama ağ geçidinin genel IP adresini bulun. ![Kayıt uygulama ağ geçidi ortak IP adresi](../media/application-gateway-web-application-firewall-portal/application-gateway-record-ag-address.png) 

   Veya **Tüm kaynakları**seçebilir, arama kutusuna *myAGPublicIPAddress'i* girebilir ve ardından arama sonuçlarında seçebilirsiniz. Azure, **Genel Bakış** sayfasında genel IP adresini görüntüler.
1. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.
1. Yanıtı kontrol edin. Geçerli bir yanıt, uygulama ağ geçidinin başarıyla oluşturulduğunu ve arka uçla başarıyla bağlanabileceğini doğrular.

   ![Uygulama ağ geçidini test etme](../media/application-gateway-web-application-firewall-portal/application-gateway-iistest.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Uygulama ağ geçidiyle oluşturduğunuz kaynaklara artık ihtiyacınız olmadığında, kaynak grubunu kaldırın. Kaynak grubunu kaldırarak, uygulama ağ geçidini ve ilgili tüm kaynaklarını da kaldırırsınız. 

Kaynak grubunu kaldırmak için:

1. Azure portalının sol menüsünde **Kaynak gruplarını**seçin.
2. Kaynak **grupları** sayfasında, listede **myResourceGroupAG'ı** arayın ve ardından seçin.
3. Kaynak **grubu sayfasında**kaynak **grubunu sil'i**seçin.
4. **KAYNAK GRUP ADINI YAZIN** ve Sonra **Sil'i**seçmek için *myResourceGroupAG'ı* girin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web Uygulaması Güvenlik Duvarı hakkında daha fazla bilgi edinin](../overview.md)
