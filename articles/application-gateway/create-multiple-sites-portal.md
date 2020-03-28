---
title: 'Öğretici: Azure portalını kullanarak birden çok web sitesi barındıran'
titleSuffix: Azure Application Gateway
description: Bu eğitimde, Azure portalını kullanarak birden çok web sitesine ev sahipliği yapan bir uygulama ağ geçidioluşturmayı öğrenirsiniz.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 07/26/2019
ms.author: victorh
ms.openlocfilehash: ca6be666a9b77532b4f1c61f6e3391c239e82c91
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74075145"
---
# <a name="tutorial-create-and-configure-an-application-gateway-to-host-multiple-web-sites-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak birden çok web sitesi barındırmak için bir uygulama ağ geçidi oluşturma ve yapılandırma

Bir [uygulama ağ geçidi](overview.md)oluştururken birden çok web sitesi barındırma [yapılandırmak](multiple-site-overview.md) için Azure portalını kullanabilirsiniz. Bu öğreticide, sanal makineleri kullanarak arka uç adres havuzlarını tanımlarsınız. Ardından sahip olduğunuz dinleyicileri ve kuralları, web trafiğinin havuzlardaki uygun sunuculara ulaşması için yapılandırırsınız. Bu öğreticide birden çok etki alanına sahip olduğunuz varsayılır ve *www.contoso.com* ve *www.fabrikam.com* örnekleri kullanılır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Uygulama ağ geçidi oluşturma
> * Arka uç sunucuları için sanal makineler oluşturma
> * Arka uç sunucuları ile arka uç havuzları oluşturma
> * Arka uç dinleyicileri oluşturma
> * Yönlendirme kuralları oluşturma
> * Etki alanınızda bir CNAME kaydı oluşturma

![Çok siteli yönlendirme örneği](./media/create-multiple-sites-portal/scenario.png)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum açın[https://portal.azure.com](https://portal.azure.com)

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

1. Azure portalının sol menüsünde **kaynak oluştur'u** seçin. **Yeni** pencere görüntülenir.

2. **Ağ Ing'i** seçin ve ardından **Öne Çıkanlar** listesinde **Uygulama Ağ Geçidi'ni** seçin.

### <a name="basics-tab"></a>Temel bilgiler sekmesi

1. Temel **bilgiler** sekmesinde, aşağıdaki uygulama ağ geçidi ayarları için aşağıdaki değerleri girin:

   - **Kaynak grubu**: Kaynak grubu için **myResourceGroupAG'ı** seçin. Yoksa, oluşturmak için **yeni oluştur'u** seçin.
   - **Uygulama ağ geçidi adı**: Uygulama ağ geçidinin adı için *myAppGateway'i* girin.

     ![Yeni uygulama ağ geçidi oluşturma: Temel bilgiler](./media/application-gateway-create-gateway-portal/application-gateway-create-basics.png)

2.  Azure'un oluşturduğunuz kaynaklar arasında iletişim kurabilmesi için sanal bir ağa ihtiyacı vardır. Yeni bir sanal ağ oluşturabilir veya varolan bir ağ kullanabilirsiniz. Bu örnekte, uygulama ağ geçidini oluşturduğunuz anda yeni bir sanal ağ oluşturursunuz. Uygulama Ağ Geçidi örnekleri ayrı alt ağlarda oluşturulur. Bu örnekte iki alt ağ oluşturursunuz: biri uygulama ağ geçidi için, diğeri de arka uç sunucuları için.

    **Sanal ağı yapılandırın**, yeni bir sanal ağ oluşturmak için **yeni oluştur'u** seçin. Açılan **sanal ağ oluştur** penceresinde, sanal ağ ve iki alt ağ oluşturmak için aşağıdaki değerleri girin:

    - **Adı**: Sanal ağ adı için *myVNet* girin.

    - **Alt ağ adı** (Application Gateway subnet): **Alt ağlar** *ızgarası Varsayılan*adlı bir alt ağ gösterir. Bu alt netin adını *myAGSubnet*olarak değiştirin.<br>Uygulama ağ geçidi alt ağı yalnızca uygulama ağ geçitleri içerebilir. Başka kaynaklara izin verilmez.

    - **Alt ağ adı** (arka uç sunucu alt ağı): **Alt ağlar** ızgarasının ikinci satırında, **Subnet ad** sütununa *myBackendSubnet* girin.

    - **Adres aralığı** (arka uç sunucu alt ağı): **Alt ağlar** ızgarasının ikinci satırında, *myAGSubnet'in*adres aralığıyla çakışan bir adres aralığı girin. Örneğin, *myAGSubnet'in* adres aralığı 10.0.0.0/24 ise, *myBackendSubnet*adres aralığı için *10.0.1.0/24* girin.

    **Sanal ağ oluştur** penceresini kapatmak ve sanal ağ ayarlarını kaydetmek için **Tamam'ı** seçin.

     ![Yeni uygulama ağ geçidi oluşturun: sanal ağ](./media/application-gateway-create-gateway-portal/application-gateway-create-vnet.png)
    
3. Temel **Ayarlar** sekmesinde, diğer ayarlar için varsayılan değerleri kabul edin ve sonra **İleri: Frontends'i**seçin.

### <a name="frontends-tab"></a>Frontends sekmesi

1. **Frontends** sekmesinde, **Frontend IP adresi türünün** **Genel**olarak ayarlı olduğunu doğrulayın. <br>Frontend IP'yi kullanım durumunuza göre Herkese Açık veya Özel olarak yapılandırabilirsiniz. Bu örnekte, bir Public Frontend IP'si seçersiniz.
   > [!NOTE]
   > Application Gateway v2 SKU için yalnızca **Public** frontend IP yapılandırmasını seçebilirsiniz. Bu v2 SKU için özel ön uç IP yapılandırması şu anda etkinleştirilemesin.

2. **Genel IP adresi** için yeni **oluştur'u** seçin ve genel IP adresi adı için *myAGPublicIPAddress'i* girin ve ardından **Tamam'ı**seçin. 

     ![Yeni uygulama ağ geçidi oluşturma: frontends](./media/application-gateway-create-gateway-portal/application-gateway-create-frontends.png)

3. **Sonraki'ni Seçin: Arka uçlar.**

### <a name="backends-tab"></a>Arka uçlar sekmesi

Arka uç havuzu, istekleri sağlayan arka uç sunucularına yönlendirmek için kullanılır. Arka uç havuzları NIC'ler, sanal makine ölçek kümeleri, genel IP'ler, dahili IP'ler, tam nitelikli alan adları (FQDN) ve Azure Uygulama Hizmeti gibi çok kiracılı arka uçlar olabilir. Bu örnekte, uygulama ağ geçidiniz le boş bir arka uç havuzu oluşturur ve sonra arka uç havuzuna arka uç hedefleri eklersiniz.

1. Arka **uçlar** sekmesinde **+Arka uç havuzu ekle'yi**seçin.

2. Açılan **bir arka uç havuzu ekle** penceresinde, boş bir arka uç havuzu oluşturmak için aşağıdaki değerleri girin:

    - **Adı**: Arka uç havuzunun adı için *contosoPool* girin.
    - **Hedefsiz arka uç havuzu ekleyin**: Hedefsiz bir arka uç havuzu oluşturmak için **Evet'i** seçin. Uygulama ağ geçidini oluşturduktan sonra arka uç hedefleri eklersiniz.

3. Arka **uç havuzu ekle** penceresinde, arka uç havuzu yapılandırmasını kaydetmek ve **Arka Uçlar** sekmesine dönmek için **Ekle'yi** seçin.
4. Şimdi *fabrikamPool*denilen başka bir arka uç havuzu ekleyin.

     ![Yeni uygulama ağ geçidi oluşturma: arka uçlar](./media/create-multiple-sites-portal/backend-pools.png)

4. Arka **uçlar** sekmesinde **İleri: Yapılandırma'yı**seçin.

### <a name="configuration-tab"></a>Yapılandırma sekmesi

**Yapılandırma** sekmesinde, yönlendirme kuralı nı kullanarak oluşturduğunuz ön uç ve arka uç havuzlarını bağlarsınız.

1. **Yönlendirme kuralları** sütununda **kural ekle'yi** seçin.

2. Açılan **yönlendirme kuralı** ekle penceresinde, **Kural adı**için *contosoRule* girin.

3. Yönlendirme kuralı dinleyici gerektirir. **Yönlendirme kuralı** ekle penceresindeki **Dinleyici** sekmesine, dinleyici için aşağıdaki değerleri girin:

    - **Dinleyici adı**: Dinleyicinin adı için *contosoListener* girin.
    - **Frontend IP**: Ön uç için oluşturduğunuz genel IP'yi seçmek için **Genel'i** seçin.

   **Ek ayarların**altında:
   - **Dinleyici türü**: Birden fazla site
   - **Ev Sahibi Adı**: **www.contoso.com**

   **Dinleyici** sekmesindeki diğer ayarlar için varsayılan değerleri kabul edin ve yönlendirme kuralının geri kalanını yapılandırmak için **Arka Uç hedefleri** sekmesini seçin.

   ![Yeni uygulama ağ geçidi oluşturun: dinleyici](./media/create-multiple-sites-portal/routing-rule.png)

4. **Backend hedefleri** sekmesinde, **Backend hedefi**için **contosoPool'u** seçin.

5. HTTP **ayarı**için, yeni bir HTTP ayarı oluşturmak için **yeni oluştur'u** seçin. HTTP ayarı yönlendirme kuralının davranışını belirler. Açılan **bir HTTP ayar penceresi ekle** penceresinde, HTTP ayar **adı**için *contosoHTTPSetting* girin. **HTTP ayar ekle** penceresindeki diğer ayarlar için varsayılan değerleri kabul edin ve ardından yönlendirme **kuralı** ekle penceresine dönmek için **Ekle'yi** seçin. 

6. Yönlendirme **kuralı ekle** penceresinde, yönlendirme kuralını kaydetmek ve **Yapılandırma** sekmesine dönmek için **Ekle'yi** seçin.
7. Fabrikam için **kural ekle** ve benzer bir kural, dinleyici, arka uç hedefi ve HTTP ayarı ekleyin'i seçin.

     ![Yeni uygulama ağ geçidi oluşturma: yönlendirme kuralı](./media/create-multiple-sites-portal/fabrikamRule.png)

7. **Sonraki seçin: Etiketler** ve sonra **Sonraki: Gözden Geçirme + oluştur**.

### <a name="review--create-tab"></a>İnceleme + oluşturma sekmesi

**Gözden Geçir + oluştur** sekmesindeki ayarları gözden geçirin ve ardından sanal ağı, genel IP adresini ve uygulama ağ geçidini oluşturmak için **Oluştur'u** seçin. Azure'un uygulama ağ geçidini oluşturması birkaç dakika sürebilir.

Sonraki bölüme geçmeden önce dağıtımın başarıyla bitmesini bekleyin.

## <a name="add-backend-targets"></a>Arka uç hedefleri ekleme

Bu örnekte, hedef arka uç olarak sanal makineleri kullanırsınız. Varolan sanal makineleri kullanabilir veya yenilerini oluşturabilirsiniz. Azure'un uygulama ağ geçidi için arka uç sunucusu olarak kullandığı iki sanal makine oluşturursunuz.

Arka uç hedefleri eklemek için şunları yapacaksınız:

1. İki yeni VMs, *contosoVM* ve *fabrikamVM*, arka uç sunucuları olarak kullanılmak üzere oluşturun.
2. Uygulama ağ geçidinin başarıyla oluşturulduğunu doğrulamak için sanal makinelere IIS yükleyin.
3. Arka uç sunucularını arka uç havuzlarına ekleyin.

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalında **kaynak oluştur'u**seçin. **Yeni** pencere görüntülenir.
2. **Popülerler** listesinde **Bilgi İşlem'i** ve ardından **Windows Server 2016 Datacenter'ı** seçin. **Sanal makine oluştur** sayfası görüntülenir.<br>Uygulama Ağ Geçidi, trafiği arka uç havuzunda kullanılan herhangi bir sanal makine türüne yönlendirebilir. Bu örnekte, bir Windows Server 2016 Datacenter kullanıyorsunuz.
3. Aşağıdaki sanal makine ayarları için **Temeller** sekmesine bu değerleri girin:

    - **Kaynak grubu**: Kaynak grubu adı için **myResourceGroupAG'ı** seçin.
    - **Sanal makine adı**: Sanal makinenin adı için *contosoVM* girin.
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

    ![Özel uzantıyı yükleme](./media/application-gateway-create-gateway-portal/application-gateway-extension.png)

2. Sanal makineye IIS yüklemek için aşağıdaki komutu çalıştırın: 

    ```azurepowershell-interactive
    Set-AzVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -ExtensionName IIS `
      -VMName contosoVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
      -Location EastUS
    ```

3. İkinci bir sanal makine oluşturun ve daha önce tamamladığınız adımları kullanarak IIS'yi yükleyin. Sanal makine adı ve **Set-AzVMExtension** cmdlet'in **VMName** ayarı için *fabrikamVM'yi* kullanın.

### <a name="add-backend-servers-to-backend-pools"></a>Arka uç havuzlarına arka uç sunucuları ekleme

1. **Tüm kaynakları**seçin ve ardından **myAppGateway'i**seçin.

2. Sol menüden **Arka Uç havuzları'nı** seçin.

3. **contosoPool'u**seçin.

4. **Hedefler**altında açılan listeden **Sanal makineyi** seçin.

5. **VIRTUAL MACHINE** ve **NETWORK INTERFACES**altında, **contosoVM** sanal makine seçin ve açılan listelerden ilişkili ağ arayüzü.

    ![Arka uç sunucuları ekleme](./media/create-multiple-sites-portal/edit-backend-pool.png)

6. **Kaydet'i**seçin.
7. *FabrikamVM* ve arayüz *fabrikamPool*eklemek için tekrarlayın.

Bir sonraki adıma geçmeden önce dağıtımın tamamlanmasını bekleyin.

## <a name="create-a-www-a-record-in-your-domains"></a>Etki alanlarınızda bir www A kaydı oluşturma

Uygulama ağ geçidi genel IP adresiyle oluşturulduktan sonra, IP adresini alabilir ve etki alanlarınızda Bir Kayıt oluşturmak için kullanabilirsiniz. 

1. **Tüm kaynakları**tıklatın ve sonra **myAGPublicIPAddress'i**tıklatın.

    ![Kayıt uygulama ağ geçidi DNS adresi](./media/create-multiple-sites-portal/public-ip.png)

2. IP adresini kopyalayın ve etki alanlarınızdaki yeni bir *www* A kaydının değeri olarak kullanın.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

1. Tarayıcınızın adres çubuğuna, etki alanı adınızı girin. Örneğin `http://www.contoso.com`.

    ![Uygulama ağ geçidinde contoso test etme](./media/create-multiple-sites-portal/application-gateway-iistest.png)

2. Adresi diğer etki alanınızla değiştirin, aşağıdaki örneğe benzer bir şey görmeniz gerekir:

    ![Uygulama ağ geçidinde fabrikam sitesini test etme](./media/create-multiple-sites-portal/application-gateway-iistest2.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Uygulama ağ geçidiyle oluşturduğunuz kaynaklara artık ihtiyacınız olmadığında, kaynak grubunu kaldırın. Kaynak grubunu kaldırdığınızda, uygulama ağ geçidini ve ilgili tüm kaynaklarını da kaldırırsınız.

Kaynak grubunu kaldırmak için:

1. Azure portalının sol menüsünde **Kaynak gruplarını**seçin.
2. Kaynak **grupları** sayfasında, listede **myResourceGroupAG'ı** arayın ve ardından seçin.
3. Kaynak **grubu sayfasında**kaynak **grubunu sil'i**seçin.
4. **KAYNAK GRUP ADINI YAZIN** ve **Sil'i** seçmek için *myResourceGroupAG'ı* girin

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Uygulama Ağ Geçidi ile yapabilecekler hakkında daha fazla bilgi edinin](application-gateway-introduction.md)