---
title: 'Quickstart: Portalı kullanarak doğrudan web trafiği'
titleSuffix: Azure Application Gateway
description: Web trafiğini arka uç havuzundaki sanal makinelere yönlendiren bir Azure Uygulama Ağ Geçidi oluşturmak için Azure portalını nasıl kullanacağınızı öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 03/09/2020
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 17adc800bd5a2ae53e27350c7e0d588eaeee4a8f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79241405"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-portal"></a>Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure portalı

Bu hızlı başlangıçta, bir uygulama ağ geçidi oluşturmak için Azure portalını kullanırsınız. Sonra doğru çalıştığından emin olmak için test edin. 

Uygulama ağ geçidi, uygulama web trafiğini arka uç havuzundaki belirli kaynaklara yönlendirir. Dinleyicileri bağlantı noktalarına atar, kurallar oluşturur ve arka uç havuzuna kaynak eklersiniz. Basitlik uğruna, bu makalede, bir ortak ön uç IP, uygulama ağ geçidi, temel bir istek yönlendirme kuralı ve arka uç havuzunda iki sanal makine tek bir site barındırmak için temel bir dinleyici ile basit bir kurulum kullanır.

Bu hızlı başlatmayı [Azure PowerShell](quick-create-powershell.md) veya [Azure CLI'yi](quick-create-cli.md)kullanarak da tamamlayabilirsiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]



## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Uygulama **ağ geçidi oluştur** sayfasındaki sekmeleri kullanarak uygulama ağ geçidini oluşturursunuz.

1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**’u seçin. **Yeni** pencere görüntülenir.

2. **Ağ Ing'i** seçin ve ardından **Öne Çıkanlar** listesinde **Uygulama Ağ Geçidi'ni** seçin.

### <a name="basics-tab"></a>Temel bilgiler sekmesi

1. Temel **bilgiler** sekmesinde, aşağıdaki uygulama ağ geçidi ayarları için aşağıdaki değerleri girin:

   - **Kaynak grubu**: Kaynak grubu için **myResourceGroupAG'ı** seçin. Yoksa, oluşturmak için **yeni oluştur'u** seçin.
   - **Uygulama ağ geçidi adı**: Uygulama ağ geçidinin adı için *myAppGateway'i* girin.

     ![Yeni uygulama ağ geçidi oluşturma: Temel bilgiler](./media/application-gateway-create-gateway-portal/application-gateway-create-basics.png)

2. Azure'un oluşturduğunuz kaynaklar arasında iletişim kurabilmesi için sanal bir ağa ihtiyacı vardır. Yeni bir sanal ağ oluşturabilir veya varolan bir ağ kullanabilirsiniz. Bu örnekte, uygulama ağ geçidini oluşturduğunuz anda yeni bir sanal ağ oluşturursunuz. Uygulama Ağ Geçidi örnekleri ayrı alt ağlarda oluşturulur. Bu örnekte iki alt ağ oluşturursunuz: biri uygulama ağ geçidi için, diğeri de arka uç sunucuları için.

    **Yapılandırılan sanal ağ**altında, **yeni oluştur'u**seçerek yeni bir sanal ağ oluşturun. Açılan **sanal ağ oluştur** penceresinde, sanal ağ ve iki alt ağ oluşturmak için aşağıdaki değerleri girin:

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
   > Uygulama Ağ Geçidi v2 SKU için, **bir Public** frontend IP yapılandırması olmalıdır. Hala hem Genel hem de Özel ön uç IP yapılandırmasına sahip olabilirsiniz, ancak özel yalnızca ön uç IP yapılandırması (Yalnızca ILB modu) şu anda v2 SKU için etkinleştirilmese de. 

2. **Genel IP adresi** için yeni **oluştur'u** seçin ve genel IP adresi adı için *myAGPublicIPAddress'i* girin ve ardından **Tamam'ı**seçin. 

     ![Yeni uygulama ağ geçidi oluşturma: frontends](./media/application-gateway-create-gateway-portal/application-gateway-create-frontends.png)

3. **Sonraki'ni Seçin: Arka uçlar.**

### <a name="backends-tab"></a>Arka uçlar sekmesi

Arka uç havuzu, istekleri sağlayan arka uç sunucularına yönlendirmek için kullanılır. Arka uç havuzları NIC'lerden, sanal makine ölçek kümelerinden, genel IP'lerden, dahili IP'lerden, tam nitelikli alan adlarından (FQDN) ve Azure Uygulama Hizmeti gibi çok kiracılı arka uçlardan oluşabilir. Bu örnekte, uygulama ağ geçidiniz le boş bir arka uç havuzu oluşturur ve sonra arka uç havuzuna arka uç hedefleri eklersiniz.

1. Arka **uçlar** sekmesinde **+Arka uç havuzu ekle'yi**seçin.

2. Açılan **bir arka uç havuzu ekle** penceresinde, boş bir arka uç havuzu oluşturmak için aşağıdaki değerleri girin:

    - **Adı**: Arka uç havuzunun adı için *myBackendPool'u* girin.
    - **Hedefsiz arka uç havuzu ekleyin**: Hedefsiz bir arka uç havuzu oluşturmak için **Evet'i** seçin. Uygulama ağ geçidini oluşturduktan sonra arka uç hedefleri eklersiniz.

3. Arka **uç havuzu ekle** penceresinde, arka uç havuzu yapılandırmasını kaydetmek ve **Arka Uçlar** sekmesine dönmek için **Ekle'yi** seçin.

     ![Yeni uygulama ağ geçidi oluşturma: arka uçlar](./media/application-gateway-create-gateway-portal/application-gateway-create-backends.png)

4. Arka **uçlar** sekmesinde **İleri: Yapılandırma'yı**seçin.

### <a name="configuration-tab"></a>Yapılandırma sekmesi

**Yapılandırma** sekmesinde, yönlendirme kuralı nı kullanarak oluşturduğunuz ön ve arka uç havuzunu bağlarsınız.

1. **Yönlendirme kuralları** sütununda **kural ekle'yi** seçin.

2. Açılan **yönlendirme kuralı** ekle penceresinde, **Kural adı**için *myRoutingRule'i* girin.

3. Yönlendirme kuralı dinleyici gerektirir. **Yönlendirme kuralı** ekle penceresindeki **Dinleyici** sekmesine, dinleyici için aşağıdaki değerleri girin:

    - **Dinleyici adı**: Dinleyicinin adı için *myListener* girin.
    - **Frontend IP**: Ön uç için oluşturduğunuz genel IP'yi seçmek için **Genel'i** seçin.
  
      **Dinleyici** sekmesindeki diğer ayarlar için varsayılan değerleri kabul edin ve yönlendirme kuralının geri kalanını yapılandırmak için **Arka Uç hedefleri** sekmesini seçin.

   ![Yeni uygulama ağ geçidi oluşturun: dinleyici](./media/application-gateway-create-gateway-portal/application-gateway-create-rule-listener.png)

4. Arka **uç hedefleri** sekmesinde, **Backend hedefi**için **myBackendPool'u** seçin.

5. HTTP **ayarı**için, yeni bir HTTP ayarı oluşturmak için **yeni oluştur'u** seçin. HTTP ayarı yönlendirme kuralının davranışını belirler. Açılan **bir HTTP ayar** penceresi ekle penceresinde, **HTTP ayar adı** için *httpayar* ve **Backend bağlantı noktası**için *80* girin. **HTTP ayar ekle** penceresindeki diğer ayarlar için varsayılan değerleri kabul edin ve ardından yönlendirme **kuralı** ekle penceresine dönmek için **Ekle'yi** seçin. 

     ![Yeni uygulama ağ geçidi oluşturma: HTTP ayarı](./media/application-gateway-create-gateway-portal/application-gateway-create-httpsetting.png)

6. Yönlendirme **kuralı ekle** penceresinde, yönlendirme kuralını kaydetmek ve **Yapılandırma** sekmesine dönmek için **Ekle'yi** seçin.

     ![Yeni uygulama ağ geçidi oluşturma: yönlendirme kuralı](./media/application-gateway-create-gateway-portal/application-gateway-create-rule-backends.png)

7. **Sonraki seçin: Etiketler** ve sonra **Sonraki: Gözden Geçirme + oluştur**.

### <a name="review--create-tab"></a>İnceleme + oluşturma sekmesi

**Gözden Geçir + oluştur** sekmesindeki ayarları gözden geçirin ve ardından sanal ağı, genel IP adresini ve uygulama ağ geçidini oluşturmak için **Oluştur'u** seçin. Azure'un uygulama ağ geçidini oluşturması birkaç dakika sürebilir. Sonraki bölüme geçmeden önce dağıtımın başarıyla bitmesini bekleyin.

## <a name="add-backend-targets"></a>Arka uç hedefleri ekleme

Bu örnekte, hedef arka uç olarak sanal makineleri kullanırsınız. Varolan sanal makineleri kullanabilir veya yenilerini oluşturabilirsiniz. Uygulama ağ geçidi için arka uç sunucusu olarak iki sanal makine oluşturursunuz.

Bunu yapmak için şunları yapacaksınız:

1. Arka uç sunucuları olarak kullanılmak üzere iki yeni VM, *myVM* ve *myVM2*oluşturun.
2. Uygulama ağ geçidinin başarıyla oluşturulduğunu doğrulamak için sanal makinelere IIS yükleyin.
3. Arka uç sunucularını arka uç havuzuna ekleyin.

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**’u seçin. **Yeni** pencere görüntülenir.
2. **Popülerler** listesinde **Windows Server 2016 Datacenter'ı** seçin. **Sanal makine oluştur** sayfası görüntülenir.<br>Uygulama Ağ Geçidi, trafiği arka uç havuzunda kullanılan herhangi bir sanal makine türüne yönlendirebilir. Bu örnekte, bir Windows Server 2016 Datacenter kullanıyorsunuz.
3. Aşağıdaki sanal makine ayarları için **Temeller** sekmesine bu değerleri girin:

    - **Kaynak grubu**: Kaynak grubu adı için **myResourceGroupAG'ı** seçin.
    - **Sanal makine adı**: Sanal makinenin adı için *myVM* girin.
    - **Bölge**: Uygulama ağ geçidini oluşturduğunuz bölgeyi seçin.
    - **Kullanıcı Adı**: Yönetici kullanıcı adı için *azureuser* yazın.
    - **Şifre**: Parola yazın.
4. Diğer varsayılanları kabul edin ve sonra **İleri: Diskler'i**seçin.  
5. **Diskler** sekmesi varsayılanlarını kabul edin ve sonra **Sonraki: Ağ'ı**seçin.
6. **Ağ** sekmesinde, **myVNet'in** **Sanal ağ** için seçildiğini ve **Alt Net'in** **myBackendSubnet**olarak ayarlı olduğunu doğrulayın. Diğer varsayılanları kabul edin ve sonra **Sonraki: Yönetim'i**seçin.<br>Uygulama Ağ Geçidi, içinde olduğu sanal ağın dışındaki örneklerle iletişim kurabilir, ancak IP bağlantısı olduğundan emin olmanız gerekir.
7. **Yönetim** sekmesinde, **Önyükleme tanılamasını** **Kapalı**olarak ayarlayın. Diğer varsayılanları kabul edin ve ardından **Gözden Geçir + oluştur'u**seçin.
8. Gözden **Geçir + oluştur** sekmesinde, ayarları gözden geçirin, doğrulama hatalarını düzeltin ve sonra **Oluştur'u**seçin.
9. Devam etmeden önce sanal makine oluşturmanın tamamlanmasını bekleyin.

### <a name="install-iis-for-testing"></a>Test için IIS'yi yükleyin

Bu örnekte, iIS'yi sanal makinelere yalnızca Azure'un uygulama ağ geçidini başarıyla oluşturduğunu doğrulamak için yüklersiniz.

1. Azure PowerShell'i açın. Azure portalının en üst teki gezinme çubuğundan **Cloud Shell'i** seçin ve ardından açılan listeden **PowerShell'i** seçin. 

    ![Özel uzantıyı yükleme](./media/application-gateway-create-gateway-portal/application-gateway-extension.png)

2. Sanal makineye IIS yüklemek için aşağıdaki komutu çalıştırın. Gerekirse *Konum* parametresini değiştirin: 

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

1. Azure portalı menüsünde **Tüm kaynakları** seçin veya Tüm kaynakları arayın ve *Tüm kaynakları*seçin. Sonra **myAppGateway**seçin.

2. Sol menüden **Arka Uç havuzları'nı** seçin.

3. **myBackendPool'u**seçin.

4. **Backend hedefleri**altında , **Hedef türü**, açılan listeden Sanal **makine** seçin.

5. **Hedef**altında, açılır listelerden **myVM** ve **myVM2** sanal makinelerini ve bunların ilişkili ağ arabirimlerini seçin.


   > [!div class="mx-imgBorder"]
   > ![Arka uç sunucuları ekleme](./media/application-gateway-create-gateway-portal/application-gateway-backend.png)

6. **Kaydet'i**seçin.

7. Bir sonraki adıma geçmeden önce dağıtımın tamamlanmasını bekleyin.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Uygulama ağ geçidini oluşturmak için IIS gerekmese de, Azure'un uygulama ağ geçidini başarıyla oluşturmuş olup olmadığını doğrulamak için bu hızlı başla başla yı yükledin. Uygulama ağ geçidini test etmek için IIS'yi kullanın:

1. **Genel Bakış** sayfasında uygulama ağ geçidinin genel IP adresini bulun. ![Kayıt uygulama ağ](./media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png) geçidi ortak IP adresi Veya, **Tüm kaynakları**seçebilir, arama kutusuna *myAGPublicIPAddress* girin ve sonra arama sonuçlarında seçin. Azure, **Genel Bakış** sayfasında genel IP adresini görüntüler.
2. Herkese açık IP adresini kopyalayın ve bu IP adresine göz atmak için tarayıcınızın adres çubuğuna yapıştırın.
3. Yanıtı kontrol edin. Geçerli bir yanıt, uygulama ağ geçidinin başarıyla oluşturulduğunu ve arka uçla başarılı bir şekilde bağlanabileceğini doğrular.

   ![Uygulama ağ geçidini test etme](./media/application-gateway-create-gateway-portal/application-gateway-iistest.png)

   Tarayıcıyı birden çok kez yenileyin ve hem myVM hem de myVM2 bağlantılarını görmeniz gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Uygulama ağ geçidinde oluşturduğunuz kaynaklara artık ihtiyacınız olmadığında kaynak grubunu silin. Kaynak grubunu sildiğinizde, uygulama ağ geçidini ve ilgili tüm kaynakları da kaldırırsınız.

Kaynak grubunu silmek için:

1. Azure portalı menüsünde **Kaynak gruplarını** seçin veya *Kaynak gruplarını*arayın ve seçin.
2. Kaynak **grupları** sayfasında, listede **myResourceGroupAG'ı** arayın ve ardından seçin.
3. Kaynak **grubu sayfasında**kaynak **grubunu sil'i**seçin.
4. **KAYNAK GRUP ADINI YAZIN** ve **Sil'i** seçmek için *myResourceGroupAG'ı* girin

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI kullanarak bir uygulama ağ geçidi ile web trafiğini yönetme](./tutorial-manage-web-traffic-cli.md)
