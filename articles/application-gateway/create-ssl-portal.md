---
title: 'Öğretici: TLS sonlandırmaişlemini portalda yapılandırın - Azure Uygulama Ağ Geçidi'
description: Bu eğitimde, bir uygulama ağ geçidini nasıl yapılandırabileceğinizi ve Azure portalını kullanarak TLS sonlandırma sertifikası eklemeyi öğrenirsiniz.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 11/13/2019
ms.author: victorh
ms.openlocfilehash: b40eb107fc975d2ef0170944892b936680de3c9f
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81312384"
---
# <a name="tutorial-configure-an-application-gateway-with-tls-termination-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak TLS sonlandırma ile bir uygulama ağ geçidini yapılandırın

Arka uç sunucuları için sanal makineler kullanan TLS sonlandırma sertifikasına sahip bir [uygulama ağ geçidini](overview.md) yapılandırmak için Azure portalını kullanabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Otomatik olarak imzalanan sertifika oluşturma
> * Sertifikalı bir uygulama ağ geçidi oluşturma
> * Arka uç sunucusu olarak kullanılan sanal makineleri oluşturma
> * Uygulama ağ geçidini test etme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum açın[https://portal.azure.com](https://portal.azure.com)

## <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma

Bu bölümde, kendi imzalı bir sertifika oluşturmak için [Yeni İmzalı Sertifika'yı](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate) kullanırsınız. Uygulama ağ geçidi için dinleyiciyi oluştururken sertifikayı Azure portalına yüklersiniz.

Yerel bilgisayarınızda yönetici olarak bir Windows PowerShell penceresi açın. Sertifikayı oluşturmak için aşağıdaki komutu çalıştırın:

```powershell
New-SelfSignedCertificate `
  -certstorelocation cert:\localmachine\my `
  -dnsname www.contoso.com
```

Bu yanıt gibi bir şey görmelisiniz:

```
PSParentPath: Microsoft.PowerShell.Security\Certificate::LocalMachine\my

Thumbprint                                Subject
----------                                -------
E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630  CN=www.contoso.com
```

Sertifikadan bir pfx dosyasını dışa aktarmak için döndürülen Parmak İzi ile [Export-PfxCertificate'ı](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate) kullanın:

> [!NOTE]
> .pfx dosya parolasınızda özel karakterler kullanmayın. Yalnızca alfasayısal karakterler desteklenir.

```powershell
$pwd = ConvertTo-SecureString -String "Azure123456" -Force -AsPlainText
Export-PfxCertificate `
  -cert cert:\localMachine\my\E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630 `
  -FilePath c:\appgwcert.pfx `
  -Password $pwd
```

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

1. Azure portalının sol menüsünde **kaynak oluştur'u** seçin. **Yeni** pencere görüntülenir.

2. **Ağ Ing'i** seçin ve ardından **Öne Çıkanlar** listesinde **Uygulama Ağ Geçidi'ni** seçin.

### <a name="basics-tab"></a>Temel bilgiler sekmesi

1. Temel **bilgiler** sekmesinde, aşağıdaki uygulama ağ geçidi ayarları için aşağıdaki değerleri girin:

   - **Kaynak grubu**: Kaynak grubu için **myResourceGroupAG'ı** seçin. Yoksa, oluşturmak için **yeni oluştur'u** seçin.
   - **Uygulama ağ geçidi adı**: Uygulama ağ geçidinin adı için *myAppGateway'i* girin.

        ![Yeni uygulama ağ geçidi oluşturma: Temel bilgiler](./media/application-gateway-create-gateway-portal/application-gateway-create-basics.png)

2.  Azure'un oluşturduğunuz kaynaklar arasında iletişim kurabilmesi için sanal bir ağa ihtiyacı vardır. Yeni bir sanal ağ oluşturabilir veya varolan bir ağ kullanabilirsiniz. Bu örnekte, uygulama ağ geçidini oluşturduğunuz anda yeni bir sanal ağ oluşturursunuz. Uygulama Ağ Geçidi örnekleri ayrı alt ağlarda oluşturulur. Bu örnekte iki alt ağ oluşturursunuz: biri uygulama ağ geçidi için, diğeri de arka uç sunucuları için.

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
   > Application Gateway v2 SKU için yalnızca **Public** frontend IP yapılandırmasını seçebilirsiniz. Bu v2 SKU için özel ön uç IP yapılandırması şu anda etkinleştirilemesin.

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
    - **Protokol**: **HTTPS'yi**seçin.
    - **Bağlantı Noktası**: Bağlantı noktası için 443 girilir.

   **HTTPS Sertifikası**Altında :

   - **PFX sertifika dosyası** - Daha önce oluşturduğunuz c:\appgwcert.pfx dosyasına göz atın ve seçin.
   - **Sertifika adı** - Sertifikanın adı için *mycert1* yazın.
   - **Parola** - Parola için *Azure123456* yazın.
  
        **Dinleyici** sekmesindeki diğer ayarlar için varsayılan değerleri kabul edin ve yönlendirme kuralının geri kalanını yapılandırmak için **Arka Uç hedefleri** sekmesini seçin.

   ![Yeni uygulama ağ geçidi oluşturun: dinleyici](./media/create-ssl-portal/application-gateway-create-rule-listener.png)

4. Arka **uç hedefleri** sekmesinde, **Backend hedefi**için **myBackendPool'u** seçin.

5. HTTP **ayarı**için, yeni bir HTTP ayarı oluşturmak için **yeni oluştur'u** seçin. HTTP ayarı yönlendirme kuralının davranışını belirler. Açılan **bir HTTP ayar penceresi ekle** penceresinde, HTTP ayar **adı**için *benimHTTPSetting* girin. **HTTP ayar ekle** penceresindeki diğer ayarlar için varsayılan değerleri kabul edin ve ardından yönlendirme **kuralı** ekle penceresine dönmek için **Ekle'yi** seçin. 

   ![Yeni uygulama ağ geçidi oluşturma: HTTP ayarı](./media/create-ssl-portal/application-gateway-create-httpsetting.png)

6. Yönlendirme **kuralı ekle** penceresinde, yönlendirme kuralını kaydetmek ve **Yapılandırma** sekmesine dönmek için **Ekle'yi** seçin.

   ![Yeni uygulama ağ geçidi oluşturma: yönlendirme kuralı](./media/application-gateway-create-gateway-portal/application-gateway-create-rule-backends.png)

7. **Sonraki seçin: Etiketler** ve sonra **Sonraki: Gözden Geçirme + oluştur**.

### <a name="review--create-tab"></a>İnceleme + oluşturma sekmesi

**Gözden Geçir + oluştur** sekmesindeki ayarları gözden geçirin ve ardından sanal ağı, genel IP adresini ve uygulama ağ geçidini oluşturmak için **Oluştur'u** seçin. Azure'un uygulama ağ geçidini oluşturması birkaç dakika sürebilir. Sonraki bölüme geçmeden önce dağıtımın başarıyla bitmesini bekleyin.

## <a name="add-backend-targets"></a>Arka uç hedefleri ekleme

Bu örnekte, hedef arka uç olarak sanal makineleri kullanırsınız. Varolan sanal makineleri kullanabilir veya yenilerini oluşturabilirsiniz. Azure'un uygulama ağ geçidi için arka uç sunucusu olarak kullandığı iki sanal makine oluşturursunuz.

Bunu yapmak için şunları yapacaksınız:

1. Arka uç sunucuları olarak kullanılmak üzere iki yeni VM, *myVM* ve *myVM2*oluşturun.
2. Uygulama ağ geçidinin başarıyla oluşturulduğunu doğrulamak için sanal makinelere IIS yükleyin.
3. Arka uç sunucularını arka uç havuzuna ekleyin.

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalında **kaynak oluştur'u**seçin. **Yeni** pencere görüntülenir.
2. **Popülerler** listesinde **Windows Server 2016 Datacenter'ı** seçin. **Sanal makine oluştur** sayfası görüntülenir.

   Uygulama Ağ Geçidi, trafiği arka uç havuzunda kullanılan herhangi bir sanal makine türüne yönlendirebilir. Bu örnekte, bir Windows Server 2016 Datacenter kullanıyorsunuz.

1. Aşağıdaki sanal makine ayarları için **Temeller** sekmesine bu değerleri girin:

    - **Kaynak grubu**: Kaynak grubu adı için **myResourceGroupAG'ı** seçin.
    - **Sanal makine adı**: Sanal makinenin adı için *myVM* girin.
    - **Kullanıcı Adı**: Yönetici kullanıcı adı için *azureuser* girin.
    - **Parola**: Yönetici parolası için *Azure123456'yı* girin.
4. Diğer varsayılanları kabul edin ve sonra **İleri: Diskler'i**seçin.  
5. **Diskler** sekmesi varsayılanlarını kabul edin ve sonra **Sonraki: Ağ'ı**seçin.
6. **Ağ** sekmesinde, **myVNet'in** **Sanal ağ** için seçildiğini ve **Alt Net'in** **myBackendSubnet**olarak ayarlı olduğunu doğrulayın. Diğer varsayılanları kabul edin ve sonra **Sonraki: Yönetim'i**seçin.

   Uygulama Ağ Geçidi, içinde olduğu sanal ağın dışındaki örneklerle iletişim kurabilir, ancak IP bağlantısı olduğundan emin olmanız gerekir.
1. **Yönetim** sekmesinde, **Önyükleme tanılamasını** **Kapalı**olarak ayarlayın. Diğer varsayılanları kabul edin ve ardından **Gözden Geçir + oluştur'u**seçin.
2. Gözden **Geçir + oluştur** sekmesinde, ayarları gözden geçirin, doğrulama hatalarını düzeltin ve sonra **Oluştur'u**seçin.
3. Devam etmeden önce dağıtımın tamamlanmasını bekleyin.

### <a name="install-iis-for-testing"></a>Test için IIS'yi yükleyin

Bu örnekte, iIS'yi sanal makinelere yalnızca Azure'un uygulama ağ geçidini başarıyla oluşturduğunu doğrulamak için yüklersiniz.

1. [Azure PowerShell'i](https://docs.microsoft.com/azure/cloud-shell/quickstart-powershell)açın. Bunu yapmak için Azure portalının en üst teki gezinme çubuğundan **Cloud Shell'i** seçin ve ardından açılan listeden **PowerShell'i** seçin. 

    ![Özel uzantıyı yükleme](./media/application-gateway-create-gateway-portal/application-gateway-extension.png)

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

    ![Arka uç sunucuları ekleme](./media/application-gateway-create-gateway-portal/application-gateway-backend.png)

6. **Kaydet'i**seçin.

7. Bir sonraki adıma geçmeden önce dağıtımın tamamlanmasını bekleyin.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

1. **Tüm kaynakları**seçin ve ardından **myAGPublicIPAddress'i**seçin.

    ![Uygulama ağ geçidi genel IP adresini kaydetme](./media/create-ssl-portal/application-gateway-ag-address.png)

2. Tarayıcınızın adres çubuğuna *uygulama\<ağ geçidi ip\>adresinizi https://* yazın.

   Kendi imzalı bir sertifika kullandıysanız güvenlik uyarısını kabul etmek için **Ayrıntılar'ı** (veya Chrome'da **Gelişmiş)** seçin ve ardından web sayfasına gidin:

    ![Güvenli uyarı](./media/create-ssl-portal/application-gateway-secure.png)

    Güvenli IIS siteniz, sonra aşağıdaki örnekte olduğu gibi görüntülenir:

    ![Temel URL’yi uygulama ağ geçidinde test etme](./media/create-ssl-portal/application-gateway-iistest.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Application Gateway TLS desteği hakkında daha fazla bilgi edinin](ssl-overview.md)
