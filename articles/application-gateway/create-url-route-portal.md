---
title: 'Öğretici: Portalı kullanarak URL yol tabanlı yönlendirme kuralları - Azure Uygulama Ağ Geçidi'
description: Bu eğitimde, Azure portalını kullanarak bir uygulama ağ geçidi ve sanal makine ölçeği kümesi için URL yol tabanlı yönlendirme kurallarının nasıl oluşturulabileceğinizi öğrenirsiniz.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 11/14/2019
ms.author: victorh
ms.openlocfilehash: bc810ac7901d83f03d3f3ac2199561225326d261
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74048133"
---
# <a name="tutorial-create-an-application-gateway-with-path-based-routing-rules-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak yol tabanlı yönlendirme kurallarıiçeren bir uygulama ağ geçidi oluşturun

Bir [uygulama ağ geçidi](application-gateway-introduction.md)oluştururken URL yol tabanlı yönlendirme [kurallarını](application-gateway-url-route-overview.md) yapılandırmak için Azure portalını kullanabilirsiniz. Bu öğreticide, sanal makineleri kullanarak arka uç havuzları oluşturursunuz. Ardından, web trafiğinin havuzlarda uygun sunuculara ulaştığından emin olmak için yönlendirme kuralları oluşturursunuz.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Uygulama ağ geçidi oluşturma
> * Arka uç sunucuları için sanal makineler oluşturma
> * Arka uç sunucuları ile arka uç havuzları oluşturma
> * Arka uç dinleyicisi oluşturma
> * Yol tabanlı yönlendirme kuralı oluşturma

![URL yönlendirme örneği](./media/application-gateway-create-url-route-portal/scenario.png)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum açın[https://portal.azure.com](https://portal.azure.com)

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bu örnekte, uygulama ağ geçidi için arka uç sunucuolarak kullanılmak üzere üç sanal makine oluşturursunuz. Uygulama ağ geçidinin beklendiği gibi çalıştığını doğrulamak için sanal makinelere IIS'yi de yüklersiniz.

1. Azure portalında **kaynak oluştur'u**seçin.
2. Popülerler listesinde **Windows Server 2016 Datacenter'ı** seçin.
3. Sanal makine için şu değerleri girin:

    - **Kaynak grubu,** **yeni oluştur'u**seçin ve sonra *myResourceGroupAG*yazın.
    - **Sanal makine adı**: *myVM1*
    - **Bölge**: *(ABD) Doğu ABD*
    - **Kullanıcı adı**: *azureuser*
    - **Şifre**: *Azure123456!*


4. **Sonraki:Diskleri**seçin.
5. **Sonraki'ni Seçin:Ağ**
6. **Sanal ağ**için **yeni oluştur'u** seçin ve ardından sanal ağ için bu değerleri yazın:

   - *myVNet* - Sanal ağın adı.
   - *10.0.0.0/16* - Sanal ağın adres alanı.
   - ilk subnet adı için *myBackendSubnet*
   - *10.0.1.0/24* - alt net adres alanı için.
   - *myAGSubnet* - ikinci subnet adı için.
   - *10.0.0.0/24* - Alt ağın adres alanı.
7. **Tamam'ı**seçin.

8. **Ağ Arabirimi**altında **myBackendSubnet'in** alt ağ için seçildiğinden emin olun ve sonra **Sonraki: Yönetim'i**seçin.
9. Önyükleme tanılamasını devre dışı kalmak için **Kapalı'yı** seçin.
10. **Gözden Geçir + Oluştur'u**tıklatın, özet sayfasındaki ayarları gözden geçirin ve ardından **Oluştur'u**seçin.
11. *MyVM2* ve *myVM3* olmak üzere iki sanal makine daha oluşturun ve bunları *MyVNet* sanal ağına ve *myBackendSubnet* alt ağına yerleştirin.

### <a name="install-iis"></a>IIS yükleme

1. Etkileşimli kabuğu açın ve **PowerShell**olarak ayarlandıklarından emin olun.

    ![Özel uzantıyı yükleme](./media/application-gateway-create-url-route-portal/application-gateway-extension.png)

2. Sanal makineye IIS yüklemek için aşağıdaki komutu çalıştırın: 

    ```azurepowershell
         $publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1");  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }

        Set-AzVMExtension `
         -ResourceGroupName myResourceGroupAG `
         -Location eastus `
         -ExtensionName IIS `
         -VMName myVM1 `
         -Publisher Microsoft.Compute `
         -ExtensionType CustomScriptExtension `
         -TypeHandlerVersion 1.4 `
         -Settings $publicSettings
    ```

3. İki sanal makine daha oluşturun ve yeni bitirdiğiniz adımları kullanarak IIS'yi yükleyin. Set-AzVMExtension'daki VMName değerleri ve adları için *myVM2* ve *myVM3* adlarını girin.

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

1. Azure portalının sol menüsünde **kaynak oluştur'u** seçin. **Yeni** pencere görüntülenir.

2. **Ağ Ing'i** seçin ve ardından **Öne Çıkanlar** listesinde **Uygulama Ağ Geçidi'ni** seçin.

### <a name="basics-tab"></a>Temel bilgiler sekmesi

1. Temel **bilgiler** sekmesinde, aşağıdaki uygulama ağ geçidi ayarları için aşağıdaki değerleri girin:

   - **Kaynak grubu**: Kaynak grubu için **myResourceGroupAG'ı** seçin.
   - **Uygulama ağ geçidi adı**: Uygulama ağ geçidinin adı için *myAppGateway'i* girin.
   - **Bölge** - Seçin **(ABD) Doğu ABD**.

        ![Yeni uygulama ağ geçidi oluşturma: Temel bilgiler](./media/application-gateway-create-gateway-portal/application-gateway-create-basics.png)

2.  **Sanal ağı yapılandırın**, sanal ağ adı için **myVNet'i** seçin.
3. Alt ağ için **myAGSubnet'i** seçin.
3. Diğer ayarlar için varsayılan değerleri kabul edin ve sonra **İleri: Frontends'i**seçin.

### <a name="frontends-tab"></a>Frontends sekmesi

1. **Frontends** sekmesinde, **Frontend IP adresi türünün** **Genel**olarak ayarlı olduğunu doğrulayın.

   > [!NOTE]
   > Application Gateway v2 SKU için yalnızca **Public** frontend IP yapılandırmasını seçebilirsiniz. Bu v2 SKU için özel ön uç IP yapılandırması şu anda etkinleştirilemesin.

2. **Genel IP adresi** için yeni **oluştur'u** seçin ve genel IP adresi adı için *myAGPublicIPAddress'i* girin ve ardından **Tamam'ı**seçin. 
3. **Sonraki'ni Seçin: Arka uçlar.**

### <a name="backends-tab"></a>Arka uçlar sekmesi

Arka uç havuzu, istekleri sağlayan arka uç sunucularına yönlendirmek için kullanılır. Arka uç havuzları NIC'lerden, sanal makine ölçek kümelerinden, genel IP'lerden, dahili IP'lerden, tam nitelikli alan adlarından (FQDN) ve Azure Uygulama Hizmeti gibi çok kiracılı arka uçlardan oluşabilir.

1. Arka **uçlar** sekmesinde **+Arka uç havuzu ekle'yi**seçin.

2. Açılan **bir arka uç havuzu ekle** penceresinde, boş bir arka uç havuzu oluşturmak için aşağıdaki değerleri girin:

    - **Adı**: Arka uç havuzunun adı için *myBackendPool'u* girin.
3. **Backend Targets**, **Hedef türü**altında, açılan listeden Sanal **makine** seçin.

5. **Hedef** altında **myVM1**için ağ arabirimi seçin.
6. **Ekle'yi**seçin.
7. Hedef olarak *myVM2* içeren bir *Görüntüler* arka uç havuzu ve hedef olarak *myVM3* içeren bir *Video* arka uç havuzu eklemek için tekrarlayın.
8. Arka uç havuzu yapılandırmasını kaydetmek ve **Arka Uçlar** sekmesine dönmek için **Ekle'yi** seçin.

4. Arka **uçlar** sekmesinde **İleri: Yapılandırma'yı**seçin.

### <a name="configuration-tab"></a>Yapılandırma sekmesi

**Yapılandırma** sekmesinde, yönlendirme kuralı nı kullanarak oluşturduğunuz ön ve arka uç havuzunu bağlarsınız.

1. **Yönlendirme kuralları** sütununda **kural ekle'yi** seçin.

2. Açılan **yönlendirme kuralı** ekle penceresinde, **Kural adı**için *myRoutingRule'i* girin.

3. Yönlendirme kuralı dinleyici gerektirir. **Yönlendirme kuralı** ekle penceresindeki **Dinleyici** sekmesine, dinleyici için aşağıdaki değerleri girin:

    - **Dinleyici adı**: Dinleyicinin adı için *myListener* girin.
    - **Frontend IP**: Ön uç için oluşturduğunuz genel IP'yi seçmek için **Genel'i** seçin.
    - **Bağlantı Noktası**: Type *8080*
  
        **Dinleyici** sekmesindeki diğer ayarlar için varsayılan değerleri kabul edin ve yönlendirme kuralının geri kalanını yapılandırmak için **Arka Uç hedefleri** sekmesini seçin.

4. Arka **uç hedefleri** sekmesinde, **Backend hedefi**için **myBackendPool'u** seçin.

5. HTTP **ayarı**için, yeni bir HTTP ayarı oluşturmak için **yeni oluştur'u** seçin. HTTP ayarı yönlendirme kuralının davranışını belirler. 

6. Açılan **bir HTTP ayar penceresi ekle** penceresinde, HTTP ayar **adı**için *benimHTTPSetting* girin. **HTTP ayar ekle** penceresindeki diğer ayarlar için varsayılan değerleri kabul edin ve ardından yönlendirme **kuralı** ekle penceresine dönmek için **Ekle'yi** seçin.
7. **Yol tabanlı yönlendirme**altında, **yol tabanlı bir kural oluşturmak için birden çok hedef ekle'yi**seçin.
8. **Yol**için , yazın */images/*\*.
9. **Yol kural adı için,** *Görüntüler*yazın.
10. **HTTP ayarı**için **myHTTPSetting'i** seçin
11. **Arka uç hedefi için,** **Görüntüler'i**seçin.
12. Yol kuralını kaydetmek için **Ekle'yi** seçin ve **yönlendirme kuralı ekle** sekmesine dönün.
13. Video için başka bir kural eklemek için yineleyin.
14. Yönlendirme kuralını eklemek ve **Yapılandırma** sekmesine dönmek için **Ekle'yi** seçin.
15. **Sonraki seçin: Etiketler** ve sonra **Sonraki: Gözden Geçirme + oluştur**.

> [!NOTE]
> Varsayılan servis taleplerini işlemek */* için özel * yol kuralı eklemeniz gerekmez. Bu, varsayılan arka uç havuzu tarafından otomatik olarak işlenir.

### <a name="review--create-tab"></a>İnceleme + oluşturma sekmesi

**Gözden Geçir + oluştur** sekmesindeki ayarları gözden geçirin ve ardından sanal ağı, genel IP adresini ve uygulama ağ geçidini oluşturmak için **Oluştur'u** seçin. Azure'un uygulama ağ geçidini oluşturması birkaç dakika sürebilir. Sonraki bölüme geçmeden önce dağıtımın başarıyla bitmesini bekleyin.


## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

1. **Tüm kaynakları**seçin ve ardından **myAppGateway'i**seçin.

    ![Uygulama ağ geçidi genel IP adresini kaydetme](./media/application-gateway-create-url-route-portal/application-gateway-record-ag-address.png)

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. Örneğin, http:\//52.188.72.175:8080.

    ![Temel URL’yi uygulama ağ geçidinde test etme](./media/application-gateway-create-url-route-portal/application-gateway-iistest.png)

   Bağlantı noktası 8080'deki dinleyici bu isteği varsayılan arka uç havuzuna yönlendirir.

3. URL'yi *ip&lt;adresini&gt;http:// :8080/images/test.htm* &lt;olarak değiştirin , ip adresinizi&gt; IP adresinizle değiştirin ve aşağıdaki örnek gibi bir şey görmeniz gerekir:

    ![Görüntü URL’sini uygulama ağ geçidinde test etme](./media/application-gateway-create-url-route-portal/application-gateway-iistest-images.png)

   Bağlantı noktası 8080'deki dinleyici bu isteği *Görüntüler* arka uç havuzuna yönlendirir.

4. URL'yi *ip&lt;adresinizi&gt;http:// :8080/video/test.htm*olarak&gt; değiştirin , ip adresinizi &lt;IP adresinizle değiştirin ve aşağıdaki örnek gibi bir şey görmeniz gerekir:

    ![Video URL’sini uygulama ağ geçidinde test etme](./media/application-gateway-create-url-route-portal/application-gateway-iistest-video.png)

   Bağlantı noktası 8080'deki dinleyici bu isteği *Video* arka uç havuzuna yönlendirir.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Uygulama Ağ Geçidi'nde SSL'nin uçmasını etkinleştirme](application-gateway-backend-ssl.md)
