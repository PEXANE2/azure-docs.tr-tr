---
title: PORTALDA HTTP'den HTTPS'ye yeniden yönlendirme - Azure Uygulama Ağ Geçidi
description: Azure portalını kullanarak HTTP'den HTTPS'ye yönlendirilmiş trafikiçeren bir uygulama ağ geçidini nasıl oluşturabilirsiniz öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/13/2019
ms.author: victorh
ms.openlocfilehash: 51c191a7815bb64243e2324e150c00c2dcb7ec4c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76705335"
---
# <a name="create-an-application-gateway-with-http-to-https-redirection-using-the-azure-portal"></a>Azure portalını kullanarak HTTP ile HTTPS yeniden yönlendirmesi ile bir uygulama ağ geçidi oluşturma

Azure portalını, SSL sonlandırma sertifikasına sahip bir [uygulama ağ geçidi](overview.md) oluşturmak için kullanabilirsiniz. Yönlendirme kuralı, HTTP trafiğini uygulama ağ geçidinizdeki HTTPS bağlantı noktasına yönlendirmek için kullanılır. Bu örnekte, uygulama ağ geçidinin arka uç havuzu için iki sanal makine örneği içeren sanal bir [makine ölçeği kümesi](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) de oluşturursunuz.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Otomatik olarak imzalanan sertifika oluşturma
> * Ağ ayarlama
> * Sertifikalı bir uygulama ağ geçidi oluşturma
> * Dinleyici ve yeniden yönlendirme kuralı ekleme
> * Varsayılan arka uç havuzuyla bir sanal makine ölçek kümesi oluşturma

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğretici, bir sertifika oluşturmak ve IIS yüklemek için Azure PowerShell modülü sürüm 1.0.0 veya daha sonra gerektirir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). Bu öğreticideki komutları çalıştırmak için Azure `Login-AzAccount` ile bağlantı oluşturmak için de çalıştırmanız gerekir.

## <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma

Üretim kullanımı için, güvenilir bir sağlayıcı tarafından imzalanmış geçerli bir sertifika almanız gerekir. Bu öğretici için [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate) komutunu kullanarak otomatik olarak imzalanan bir sertifika oluşturursunuz. Sertifikadan pfx dosyası dışarı aktarmak için döndürülen Parmak izi ile [Export-PfxCertificate](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate) komutunu kullanabilirsiniz.

```powershell
New-SelfSignedCertificate `
  -certstorelocation cert:\localmachine\my `
  -dnsname www.contoso.com
```

Bu sonuca benzer bir şey görmeniz gerekir:

```
PSParentPath: Microsoft.PowerShell.Security\Certificate::LocalMachine\my

Thumbprint                                Subject
----------                                -------
E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630  CN=www.contoso.com
```

pfx dosyasını oluşturmak için parmak izini kullanın:

```powershell
$pwd = ConvertTo-SecureString -String "Azure123456!" -Force -AsPlainText
Export-PfxCertificate `
  -cert cert:\localMachine\my\E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630 `
  -FilePath c:\appgwcert.pfx `
  -Password $pwd
```

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Oluşturduğunuz kaynaklar arasındaki iletişim için sanal ağ gereklidir. Bu örnekte iki alt ağ oluşturulmuştur: biri uygulama ağ geçidi ve diğeri de arka uç sunucuları içindir. Uygulama ağ geçidini oluştururken aynı zamanda bir sanal makine oluşturabilirsiniz.

1. Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.
2. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
3. **Ağ** ve ardından Öne Çıkanlar listesinde **Application Gateway**’i seçin.
4. Uygulama ağ geçidi için şu değerleri girin:

   - *myAppGateway* - Uygulama ağ geçidinin adı.
   - *myResourceGroupAG* - Yeni kaynak grubu.

     ![Yeni uygulama ağ geçidi oluşturma](./media/create-url-route-portal/application-gateway-create.png)

5. Diğer ayarların varsayılan değerlerini kabul edin ve sonra **Tamam**’a tıklayın.
6. **Sanal ağ seç'i**tıklatın, **yeni oluştur'u**tıklatın ve ardından sanal ağ için bu değerleri girin:

   - *myVNet* - Sanal ağın adı.
   - *10.0.0.0/16* - Sanal ağın adres alanı.
   - *myAGSubnet* - Alt ağın adı.
   - *10.0.0.0/24* - Alt ağın adres alanı.

     ![Sanal ağ oluşturma](./media/create-url-route-portal/application-gateway-vnet.png)

7. Sanal ağı ve alt ağı oluşturmak için **Tamam**’a tıklayın.
8. **Frontend IP yapılandırması**altında, **IP adresi türünün** **Genel**olduğundan emin olun ve **Yeni Oluştur** seçilir. Ad için *myAGPublicIPAddress* girin. Diğer ayarların varsayılan değerlerini kabul edin ve sonra **Tamam**’a tıklayın.
9. **Dinleyici yapılandırması**altında **HTTPS'yi**seçin, ardından **dosya seçin** ve *c:\appgwcert.pfx* dosyasına gidin ve **Aç'ı**seçin.
10. Cert adı ve *Azure123456* için *appgwcert* yazın! girin.
11. Web uygulaması güvenlik duvarını devre dışı bırakın ve ardından **Tamam'ı**seçin.
12. Özet sayfasındaki ayarları gözden geçirin ve ağ kaynaklarını ve uygulama ağ geçidini oluşturmak için **Tamam'ı** seçin. Uygulama ağ geçidinin oluşturulması birkaç dakika sürebilir, bir sonraki bölüme geçmeden önce dağıtım ın başarıyla bitmesini bekleyin.

### <a name="add-a-subnet"></a>Alt ağ ekleme

1. Sol menüdeki **Tüm kaynakları** seçin ve ardından kaynaklar listesinden **myVNet'i** seçin.
2. **Alt Ağlar'ı**seçin ve ardından **Subnet'i**tıklatın.

    ![Alt ağ oluşturma](./media/create-url-route-portal/application-gateway-subnet.png)

3. Alt net adı için *myBackendSubnet* yazın.
4. Adres aralığı için *10.0.2.0/24* yazın ve ardından **Tamam'ı**seçin.

## <a name="add-a-listener-and-redirection-rule"></a>Dinleyici ve yeniden yönlendirme kuralı ekleme

### <a name="add-the-listener"></a>Dinleyiciyi ekle

İlk olarak, bağlantı noktası 80 için *myListener* adlı dinleyici ekleyin.

1. **myResourceGroupAG** kaynak grubunu açın ve **myAppGateway'i**seçin.
2. **Dinleyicileri** seçin ve ardından **+ Basic'i**seçin.
3. Ad için *MyListener* yazın.
4. Yeni ön uç bağlantı noktası adı için *httpPort* ve bağlantı noktası için *80* yazın.
5. Protokolün **HTTP**olarak ayarlandığından emin olun ve ardından **Tamam'ı**seçin.

### <a name="add-a-routing-rule-with-a-redirection-configuration"></a>Yeniden yönlendirme yapılandırması ile yönlendirme kuralı ekleme

1. **myAppGateway'de** **Kurallar'ı** seçin ve **ardından +İstek yönlendirme kuralını**seçin.
2. Kural **adı için** *Kural2*yazın.
3. Dinleyici için **MyListener'ın** seçildiğinden emin olun.
4. **Backend hedefleri** sekmesine tıklayın ve *Yeniden Yönlendirme*olarak Hedef **türünü** seçin.
5. **Yeniden Yönlendirme türü için** **Kalıcı'yı**seçin.
6. **Yeniden Yönlendirme hedefi**için **Dinleyici'yi**seçin.
7. Hedef **dinleyicinin** **GatewayHttpListener uygulamasına**ayarlandığından emin olun.
8. Sorgu **dizesi ekle** ve **Ekle yolu** için *Evet'i*seçin.
9. **Ekle'yi**seçin.

## <a name="create-a-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi oluşturma

Bu örnekte uygulama ağ geçidinde arka uç havuzu için sunucu sağlayan bir sanal makine ölçek kümesi oluşturacaksınız.

1. Portalın sol üst köşesinde **+Kaynak oluştur'u**seçin.
2. **İşlem**’i seçin.
3. Arama kutusunda, *ölçek kümesini* yazın ve Enter tuşuna basın.
4. **Sanal makine ölçek kümesini**seçin ve ardından **Oluştur'u**seçin.
5. **Sanal makine ölçeği kümesi adı için,** *myvmss*yazın.
6. İşletim sistemi disk görüntüsü için,** **Windows Server 2016 Datacenter'ın** seçildiğinden emin olun.
7. **Kaynak grubu için** **myResourceGroupAG'ı**seçin.
8. **Kullanıcı adı**için , *azureuser*yazın.
9. **Parola**için , *Azure123456 yazın!* ve şifreyi onaylayın.
10. **Örneğin sayısı**için, **değerin 2**olduğundan emin olun.
11. **Örnek boyutu**için, **D2s_v3**seçin.
12. **Ağ**altında, **Yük dengeleme seçeneklerini** **seç'in Uygulama Ağ Geçidi**olarak ayarlı olduğundan emin olun.
13. **Uygulama ağ geçidinin** **myAppGateway'e**ayarlandığından emin olun.
14. **Subnet** **myBackendSubnet**ayarlanır emin olun.
15. **Oluştur'u**seçin.

### <a name="associate-the-scale-set-with-the-proper-backend-pool"></a>Ölçek kümesini uygun arka uç havuzuyla ilişkilendirin

Sanal makine ölçeği kümesi portalı UI, ölçek kümesi için yeni bir arka uç havuzu oluşturur, ancak bunu mevcut uygulamaGatewayBackendPool ile ilişkilendirmek istersiniz.

1. **myResourceGroupAg** kaynak grubunu açın.
2. **myAppGateway'i**seçin.
3. **Arka uç havuzları**seçin.
4. **myAppGatewaymyvmss'i**seçin.
5. Arka **uç havuzundan tüm hedefleri kaldır'ı**seçin.
6. **Kaydet'i**seçin.
7. Bu işlem tamamlandıktan sonra **myAppGatewaymyvmss** arka uç havuzunu seçin, **Sil'i** seçin ve onaylamak için **Tamam'ı** seçin.
8. **AppGatewayBackendPool**seçin.
9. **Hedefler**altında **VMSS'yi**seçin.
10. **VMSS**altında, **myvmss**seçin.
11. **Ağ Arabirimi Yapılandırmaları** **altında, myvmssNic**seçin.
12. **Kaydet'i**seçin.

### <a name="upgrade-the-scale-set"></a>Ölçek kümesini yükseltme

Son olarak, bu değişikliklerle birlikte ölçek kümesini yükseltmeniz gerekir.

1. **Myvmss** ölçek kümesini seçin.
2. **Ayarlar**'ın altında **Örnekler**’i seçin.
3. Her iki örneği de seçin ve sonra **Yükseltme'yi**seçin.
4. Onaylamak için **Evet**'i seçin.
5. Bu tamamlandıktan **sonra, myAppGateway'e** geri dön ve **Backend havuzlarını**seçin. Şimdi **appGatewayBackendPool** iki hedef olduğunu görmeniz gerekir ve **myAppGatewaymyvmss** sıfır hedefleri vardır.
6. **myAppGatewaymyvmss'i**seçin ve sonra **Sil'i**seçin.
7. Onaylamak için **Tamam**’ı seçin.

### <a name="install-iis"></a>IIS yükleme

IIS'yi ölçek kümesine yüklemenin kolay bir yolu PowerShell'i kullanmaktır. Portaldan Cloud Shell simgesini tıklatın ve **PowerShell'in** seçildiğinden emin olun.

Aşağıdaki kodu PowerShell penceresine yapıştırın ve Enter tuşuna basın.

```azurepowershell
$publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1"); 
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
$vmss = Get-AzVmss -ResourceGroupName myResourceGroupAG -VMScaleSetName myvmss
Add-AzVmssExtension -VirtualMachineScaleSet $vmss `
  -Name "customScript" `
  -Publisher "Microsoft.Compute" `
  -Type "CustomScriptExtension" `
  -TypeHandlerVersion 1.8 `
  -Setting $publicSettings
Update-AzVmss `
  -ResourceGroupName myResourceGroupAG `
  -Name myvmss `
  -VirtualMachineScaleSet $vmss
```

### <a name="upgrade-the-scale-set"></a>Ölçek kümesini yükseltme

IIS ile örnekleri değiştirdikten sonra, bu değişiklikle ölçek kümesini bir kez daha yükseltmeniz gerekir.

1. **Myvmss** ölçek kümesini seçin.
2. **Ayarlar**'ın altında **Örnekler**’i seçin.
3. Her iki örneği de seçin ve sonra **Yükseltme'yi**seçin.
4. Onaylamak için **Evet**'i seçin.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Uygulama genel IP adresini uygulama ağ geçidine Genel Bakış sayfasından alabilirsiniz.

1. **myAppGateway'i**seçin.
2. Genel **Bakış** sayfasında, **Frontend genel IP adresinin**altındaki IP adresine dikkat edin.

3. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. Örneğin, http://52.170.203.149

   ![Güvenli uyarı](./media/redirect-http-to-https-powershell/application-gateway-secure.png)

4. Kendi imzalı bir sertifika kullandıysanız güvenlik uyarısını kabul etmek için **Ayrıntılar'ı** seçin ve **ardından web sayfasına gidin.** Güvenli IIS siteniz, sonra aşağıdaki örnekte olduğu gibi görüntülenir:

   ![Temel URL’yi uygulama ağ geçidinde test etme](./media/redirect-http-to-https-powershell/application-gateway-iistest.png)

## <a name="next-steps"></a>Sonraki adımlar

[Dahili yeniden yönlendirme yle uygulama ağ geçidi](redirect-internal-site-powershell.md)oluşturmayı öğrenin.
