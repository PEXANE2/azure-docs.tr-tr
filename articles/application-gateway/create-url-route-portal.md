---
title: Öğretici-URL yolu tabanlı yönlendirme kurallarıyla uygulama ağ geçidi oluşturma-Azure portal
description: Bu öğreticide, Azure portal kullanarak bir uygulama ağ geçidi ve sanal makine ölçek kümesi için URL yol tabanlı yönlendirme kuralları oluşturmayı öğreneceksiniz.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 07/27/2019
ms.author: victorh
ms.openlocfilehash: 63a1faa79374e72eabfbee4ece454728c3b4cc05
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68597629"
---
# <a name="tutorial-create-an-application-gateway-with-path-based-routing-rules-using-the-azure-portal"></a>Öğretici: Azure portal kullanarak yol tabanlı yönlendirme kuralları ile uygulama ağ geçidi oluşturma

[Uygulama ağ geçidi](overview.md)oluştururken [URL yolu tabanlı yönlendirme kurallarını](url-route-overview.md) yapılandırmak için Azure Portal kullanabilirsiniz. Bu öğreticide, sanal makineleri kullanarak arka uç havuzları oluşturacaksınız. Daha sonra, Web trafiğinin havuzlardaki uygun sunuculara ulaştığınızdan emin olmak için yönlendirme kuralları oluşturursunuz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Uygulama ağ geçidi oluşturma
> * Arka uç sunucuları için sanal makineler oluşturma
> * Arka uç sunucularıyla arka uç havuzları oluşturma
> * Arka uç dinleyicisi oluşturma
> * Yol tabanlı yönlendirme kuralı oluşturma

![URL yönlendirme örneği](./media/create-url-route-portal/scenario.png)

Tercih ederseniz, [Azure CLI](tutorial-url-route-cli.md) veya [Azure PowerShell](tutorial-url-route-powershell.md)kullanarak bu öğreticiyi tamamlayabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

1. Azure portal sol menüsünde **kaynak oluştur** ' u seçin. **Yeni** pencere görüntülenir.

2. **Ağ** ' ı seçin ve ardından **öne çıkan** listede **Application Gateway** ' yi seçin.

### <a name="basics-tab"></a>Temel bilgiler sekmesi

1. **Temel bilgiler** sekmesinde, aşağıdaki uygulama ağ geçidi ayarları için şu değerleri girin:

   - **Kaynak grubu**: Kaynak grubu için **myResourceGroupAG** öğesini seçin. Yoksa, oluşturmak için **Yeni oluştur** ' u seçin.
   - **Uygulama ağ geçidi adı**: Uygulama ağ geçidinin adı için *Myappgateway* girin.

     ![Yeni uygulama ağ geçidi oluştur: Temel](./media/application-gateway-create-gateway-portal/application-gateway-create-basics.png)

2.  Azure 'un, oluşturduğunuz kaynaklar arasında iletişim kurması için bir sanal ağa ihtiyacı vardır. Yeni bir sanal ağ oluşturabilir veya var olan bir ağı kullanabilirsiniz. Bu örnekte, uygulama ağ geçidini oluşturduğunuz sırada yeni bir sanal ağ oluşturacaksınız. Application Gateway örnekleri ayrı alt ağlarda oluşturulur. Bu örnekte iki alt ağ oluşturursunuz: bir tane uygulama ağ geçidi ve arka uç sunucuları için bir diğeri.

    **Sanal ağı Yapılandır**altında yeni bir sanal ağ oluşturmak Için **Yeni oluştur** ' u seçin. Açılan **sanal ağ oluştur** penceresinde, sanal ağ ve iki alt ağ oluşturmak için aşağıdaki değerleri girin:

    - **Ad**: Sanal ağın adı için *Myvnet* girin.

    - **Alt ağ adı** (Application Gateway alt ağ): **Alt ağlar** Kılavuzu *varsayılan*olarak adlandırılan bir alt ağ gösterir. Bu alt ağın adını *Myagsubnet*olarak değiştirin.

      Application Gateway alt ağı yalnızca uygulama ağ geçitleri içerebilir. Başka hiçbir kaynağa izin verilmez.

    - **Alt ağ adı** (arka uç sunucusu alt ağı): **Alt ağlar** kılavuzunun ikinci satırında, **alt ağ adı** sütununa *mybackendsubnet* yazın.

    - **Adres aralığı** (arka uç sunucusu alt ağı): **Alt ağlar** kılavuzunun Ikinci satırına *Myagsubnet*adres aralığıyla çakışmayacak bir adres aralığı girin. Örneğin, *Myagsubnet* adres aralığı 10.0.0.0/24 Ise, *Mybackendsubnet*adres aralığı için *10.0.1.0/24* girin.

    **Sanal ağ oluştur** penceresini kapatmak ve sanal ağ ayarlarını kaydetmek için **Tamam ' ı** seçin.

     ![Yeni uygulama ağ geçidi oluştur: sanal ağ](./media/application-gateway-create-gateway-portal/application-gateway-create-vnet.png)
    
3. **Temel bilgiler** sekmesinde, diğer ayarlar için varsayılan değerleri kabul edin ve ardından İleri ' yi **seçin: Ön uçlar**.

### <a name="frontends-tab"></a>Ön uçlar sekmesi

1. Ön **uçlar** sekmesinde, **ön uç IP adresi türünün** **genel**olarak ayarlandığını doğrulayın. <br>Ön uç IP 'sini kullanım çalışmanıza göre genel veya özel olacak şekilde yapılandırabilirsiniz. Bu örnekte, genel ön uç IP 'si seçersiniz.
   > [!NOTE]
   > Application Gateway v2 SKU 'SU için yalnızca **genel** ön uç IP yapılandırması ' nı seçebilirsiniz. Özel ön uç IP yapılandırması şu anda bu v2 SKU 'SU için etkin değil.

2. **Genel IP** adresi Için **Yeni oluştur** ' u seçin ve genel IP adresi adı Için *myagpublicıpaddress* girin ve **Tamam**' ı seçin. 

     ![Yeni uygulama ağ geçidi oluştur: ön uçlar](./media/application-gateway-create-gateway-portal/application-gateway-create-frontends.png)

3. İleri **' yi seçin: Arka uçlar**.

### <a name="backends-tab"></a>Backends sekmesi

Arka uç havuzu, isteği sunan arka uç sunucularına istekleri yönlendirmek için kullanılır. Arka uç havuzları NIC 'Ler, sanal makine ölçek kümeleri, genel IP 'Ler, iç IP 'Ler, tam etki alanı adları (FQDN) ve Azure App Service gibi çok kiracılı arka uçlar olabilir. Bu örnekte, uygulama ağ geçidiniz ile boş bir arka uç havuzu oluşturacak ve arka uç havuzuna arka uç hedefleri ekleyeceğiz.

1. **Backends** sekmesinde **+ arka uç Havuzu Ekle**' yi seçin.

2. Açılan **bir arka uç havuzu ekleyin** penceresinde, boş bir arka uç havuzu oluşturmak için aşağıdaki değerleri girin:

    - **Ad**: Arka uç havuzunun adı için *Appgatewaybackendpool* girin.
    - **Hedefleri olmayan arka uç Havuzu Ekle**: Hedefi olmayan bir arka uç havuzu oluşturmak için **Evet** ' i seçin. Uygulama ağ geçidini oluşturduktan sonra arka uç hedefleri ekleyeceksiniz.

3. Arka uç havuzu **Ekle** penceresinde, arka uç havuzu yapılandırmasını kaydetmek ve **backends** sekmesine dönmek için **Ekle** ' yi seçin.
4. Şimdi *ımaizbackendpool*ve *videobackendpool*adlı iki arka uç havuzu ekleyin.

     ![Yeni uygulama ağ geçidi oluştur: arka uçları](./media/create-url-route-portal/backends.png)

4. **Backends** sekmesinde, ileri ' yi **seçin: Yapılandırma**.

### <a name="configuration-tab"></a>Yapılandırma sekmesi

**Yapılandırma** sekmesinde, yönlendirme kuralları kullanarak oluşturduğunuz ön uç ve arka uç havuzlarını bağlayacaksınız.

1. **Yönlendirme kuralları** sütununda **Kural Ekle** ' yi seçin.
2. Açılan **yönlendirme kuralı ekle** penceresinde, **kural adı**için *Rule1* girin.
3. Yönlendirme kuralı bir dinleyici gerektirir. **Yönlendirme kuralı ekle** penceresinin içindeki **dinleyici** sekmesinde, dinleyici için aşağıdaki değerleri girin:

    - **Dinleyici adı**: Dinleyicinin adı için *Defaultlistener* girin.
    - **Ön uç IP 'si**: Ön uç için oluşturduğunuz ortak IP 'yi seçmek için **genel** ' i seçin.

   **Dinleyici** sekmesinde diğer ayarlar için varsayılan değerleri kabul edin, sonra yönlendirme kuralının geri kalanını yapılandırmak Için **arka uç hedefleri** sekmesini seçin.
4. **Arka** uç hedefi sekmesinde, **arka uç hedefi**Için **appgatewaybackendpool** ' u seçin.

5. **Http ayarı**Için yeni **Oluştur** ' u seçerek yeni bir http ayarı oluşturun. HTTP ayarı, yönlendirme kuralının davranışını tespit eder. Açılan **http ayarı Ekle** penceresinde **http ayar adı**için *myhttpsetting* girin. **Http ayarı Ekle** penceresinde diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Ekle** ' yi seçerek **yönlendirme kuralı ekle** penceresine dönün. 

6. Yönlendirme kuralı **Ekle** penceresinde, yönlendirme kuralını kaydetmek ve **yapılandırma** sekmesine dönmek için **Ekle** ' yi seçin.



1. **Yönlendirme kuralları** sütununda **Kural Ekle** ' yi seçin.

2. Açılan **yönlendirme kuralı ekle** penceresinde, **kural adı**için *bağlanma2* girin.

3. Yönlendirme kuralı bir dinleyici gerektirir. **Yönlendirme kuralı ekle** penceresinin içindeki **dinleyici** sekmesinde, dinleyici için aşağıdaki değerleri girin:

    - **Dinleyici adı**: Dinleyicinin adı için *Mybackendlistener* girin.
    - **Ön uç IP 'si**: Ön uç için oluşturduğunuz ortak IP 'yi seçmek için **genel** ' i seçin.
    - **Bağlantı noktası**: 8080

   **Ek ayarlar**altında:
   - **Dinleyici türü**: Temel

   **Dinleyici** sekmesinde diğer ayarlar için varsayılan değerleri kabul edin, sonra yönlendirme kuralının geri kalanını yapılandırmak Için **arka uç hedefleri** sekmesini seçin.

4. **Arka** uç hedefi sekmesinde, **arka uç hedefi**Için **appgatewaybackendpool** ' u seçin.

5. **Http ayarı**Için *myhttpsetting*' i seçin. **Http ayarı Ekle** penceresinde diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Ekle** ' yi seçerek **yönlendirme kuralı ekle** penceresine dönün. 

1. **Yol tabanlı yönlendirme**altında, **yol tabanlı bir kural oluşturmak için birden çok hedef Ekle**' yi seçin.
2. **Yol kuralı ekle** penceresinde, yol kuralı için aşağıdaki değerleri girin:

   - **Yol**: */images/\**
   - **Yol kuralı adı**: *Görüntüler*
   - **Http ayarı**: *myhttpsetting* seçin
   - **Arka uç hedefi**: *ımabackendpool*
9. **Add (Ekle)** seçeneğini belirleyin.
10. *Video*adlı, */video/\**  Path ve *videobackendpool*ile başka bir yol kuralı ekleyin.
11. **Değişiklikleri Kaydet ' i seçin ve yönlendirme kuralları ' na geri dönün**.

    ![Yönlendirme kuralı ekle](media/create-url-route-portal/add-routing-rule.png)

12. **Add (Ekle)** seçeneğini belirleyin.

7. İleri **' yi seçin: Etiketler** ve sonra **ileri: Gözden geçir +** oluştur.

### <a name="review--create-tab"></a>Gözden geçir + Oluştur sekmesi

**Gözden geçir + oluştur** sekmesindeki ayarları gözden geçirin ve ardından **Oluştur** ' u seçerek sanal ağı, genel IP adresini ve uygulama ağ geçidini oluşturun. Azure 'da uygulama ağ geçidini oluşturmak birkaç dakika sürebilir.

Bir sonraki bölüme geçmeden önce Dağıtım başarıyla bitene kadar bekleyin.


## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bu örnekte, uygulama ağ geçidi için arka uç sunucular olarak kullanılacak üç sanal makine oluşturursunuz. Ayrıca, uygulama ağ geçidinin başarıyla oluşturulduğunu doğrulamak için sanal makinelere IIS yüklersiniz.

1. Azure portal **kaynak oluştur**' u seçin. **Yeni** pencere görüntülenir.
2. **İşlem** ' i seçin ve ardından **popüler** listede **Windows Server 2016 Datacenter** ' ı seçin. **Sanal makine oluştur** sayfası görüntülenir.

   Application Gateway, arka uç havuzunda kullanılan herhangi bir sanal makine türüne trafiği yönlendirebilir. Bu örnekte, bir Windows Server 2016 Datacenter kullanırsınız.
1. Aşağıdaki sanal makine ayarları için **temel bilgiler** sekmesine bu değerleri girin:

    - **Kaynak grubu**: Kaynak grubu adı için **myResourceGroupAG** öğesini seçin.
    - **Sanal makine adı**: Sanal makinenin adı için *myVM1* girin.
    - **Kullanıcı adı**: Yönetici Kullanıcı adı için *azureuser* girin.
    - **Parola**: *Azure123456 girin!* Yönetici parolası için.
4. Diğer varsayılanları kabul edin ve ardından İleri **' yi seçin: Diskler**.  
5. **Diskler** sekmesi varsayılan değerlerini kabul edin ve ardından **ileri ' yi seçin: Ağ**iletişimi.
6. **Ağ** sekmesinde, **sanal ağ** için **Myvnet** öğesinin seçildiğini ve **alt ağın** **mybackendsubnet**olarak ayarlandığını doğrulayın. Diğer varsayılanları kabul edin ve ardından İleri **' yi seçin: Yönetim**.

   Application Gateway, içinde bulunduğu sanal ağ dışındaki örneklerle iletişim kurabilir, ancak IP bağlantısı olduğundan emin olmanız gerekir.
1. **Yönetim** sekmesinde, **önyükleme tanılamayı** **kapalı**olarak ayarlayın. Diğer varsayılanları kabul edin ve ardından **gözden geçir + oluştur**' u seçin.
2. **Gözden geçir + oluştur** sekmesinde ayarları gözden geçirin, doğrulama hatalarını düzeltin ve ardından **Oluştur**' u seçin.
3. Devam etmeden önce sanal makine oluşturma işleminin tamamlanmasını bekleyin.

### <a name="install-iis"></a>IIS yükleme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Etkileşimli kabuğu açın ve **PowerShell**'e ayarlandığından emin olun.

    ![Özel uzantıyı yükleme](./media/create-url-route-portal/application-gateway-extension.png)

2. Sanal makineye IIS yüklemek için aşağıdaki komutu çalıştırın: 

    ```azurepowershell-interactive
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

3. İki sanal makine oluşturun ve az önce tamamladığınız adımları kullanarak IIS 'yi yükleyebilirsiniz. Sanal makine adları ve set-Azvmexgerin içindeki **VMName** değerleri için *myVM2* ve *myVM3* kullanın.

## <a name="add-backend-servers-to-backend-pools"></a>Arka uç sunucularını arka uç havuzlarına ekleme

1. **Tüm kaynaklar**' ı ve ardından **myappgateway**' i seçin.

2. Sol menüden **arka uç havuzları** ' nı seçin.

3. Seçin **appGatewayBackendPool**.

4. **Hedefler**' in altında, açılan listeden **sanal makine** ' yi seçin.

5. **Sanal makine** ve **ağ arabirimleri**altında, açılan listelerden **myVM1** sanal makinesini ve onunla ilişkili ağ arabirimini seçin.

    ![Arka uç sunucuları ekleme](./media/create-url-route-portal/backend-pool.png)

6. **Kaydet**’i seçin.
7. *Imabbackendpool*'a *myVM2* ve arabirim eklemek için, ardından *MyVM3* ve arayüzünü *videobackendpool*'a ekleyin.

Sonraki adıma geçmeden önce dağıtımın tamamlanmasını bekleyin.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

1. **Tüm kaynaklar**' ı seçin ve ardından **Myagpublicıpaddress**' i seçin.

    ![Uygulama ağ geçidi genel IP adresini kaydetme](./media/create-url-route-portal/application-gateway-record-ag-address.png)

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. Örneğin http://40.121.222.19.

    ![Temel URL’yi uygulama ağ geçidinde test etme](./media/create-url-route-portal/application-gateway-iistest.png)

3. URL 'yi http://&lt;IP-adresi&gt;: 8080/images/test.htm olarak değiştirin&gt; , &lt;IP adresini IP adresinizle değiştirin ve aşağıdaki örneğe benzer bir şey görmeniz gerekir:

    ![Görüntü URL’sini uygulama ağ geçidinde test etme](./media/create-url-route-portal/application-gateway-iistest-images.png)

4. URL 'yi http://&lt;IP-adresi&gt;: 8080/video/test.htm olarak değiştirin&gt; , &lt;IP adresini IP adresinizle değiştirin ve aşağıdaki örneğe benzer bir şey görmeniz gerekir:

    ![Video URL’sini uygulama ağ geçidinde test etme](./media/create-url-route-portal/application-gateway-iistest-video.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Uygulama ağ geçidiyle oluşturduğunuz kaynaklara artık ihtiyacınız kalmadığında, kaynak grubunu kaldırın. Kaynak grubunu kaldırarak, uygulama ağ geçidini ve ilgili tüm kaynakları da kaldırırsınız. 

Kaynak grubunu kaldırmak için:

1. Azure portal sol menüsünde **kaynak grupları**' nı seçin.
2. **Kaynak grupları** sayfasında, listede **myResourceGroupAG** araması yapın ve ardından seçin.
3. **Kaynak grubu sayfasında**, **kaynak grubunu sil**' i seçin.
4. **Kaynak grubu adını yazmak** için *MyResourceGroupAG* girin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Application Gateway ile yapabilecekleriniz hakkında daha fazla bilgi edinin](application-gateway-introduction.md)
