---
title: Dahili yük dengeleyicisi (ILB) uç noktasını yapılandırma
titleSuffix: Azure Application Gateway
description: Bu makalede, özel bir ön uç IP adresi ile Uygulama Ağ Geçidi yapılandırmak için nasıl bilgi sağlar
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 01/30/2020
ms.author: victorh
ms.openlocfilehash: 809274aba35e9607aeacf7c6483ec3d10f899667
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81312361"
---
# <a name="configure-an-application-gateway-with-an-internal-load-balancer-ilb-endpoint"></a>Bir uygulama ağ geçidini dahili yük dengeleyicisi (ILB) uç noktası ile yapılandırma

Azure Uygulama Ağ Geçidi, Internet'e bakan bir VIP ile veya Internet'e maruz kalmamış dahili bir uç noktayla yapılandırılabilir. Dahili uç nokta, ön uç için özel bir IP adresi kullanır ve bu adres *dahili yük dengeleyicisi (ILB) uç noktası*olarak da bilinir.

Ön uçözel IP adresi kullanarak ağ geçidini yapılandırmak, Internet'e maruz olmayan şirket içi uygulamalar için yararlıdır. Ayrıca, Internet'e maruz kalmayan ancak daha önce Güvenli Soket katmanı (SSL), sonlandırma olarak bilinen round-robin yük dağıtımı, oturum yapışkanlığı veya Aktarım Katmanı Güvenliği (TLS) gerektiren bir güvenlik sınırında olan çok katmanlı bir uygulamadaki hizmetler ve katmanlar için de yararlıdır.

Bu makale, Azure portalını kullanarak bir uygulama ağ geçidini ön uçözel IP adresiyle yapılandırma adımları boyunca size yol gösterilmektedir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

<https://portal.azure.com> adresinden Azure portalında oturum açın.

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Azure'un oluşturduğunuz kaynaklar arasında iletişim kurabilmesi için sanal bir ağa ihtiyacı vardır. Yeni bir sanal ağ oluşturabilir veya varolan bir ağ kullanabilirsiniz. Bu örnekte, yeni bir sanal ağ oluşturursunuz. Uygulama ağ geçidini oluştururken aynı zamanda bir sanal makine oluşturabilirsiniz. Uygulama Ağ Geçidi örnekleri ayrı alt ağlarda oluşturulur. Bu örnekte iki alt ağ oluşturursunuz: biri uygulama ağ geçidi için, diğeri de arka uç sunucuları için.

1. Portal menüsünü genişletin ve **kaynak oluştur'u**seçin.
2. **Ağ** ve ardından Öne Çıkanlar listesinde **Application Gateway**’i seçin.
3. Uygulama ağ geçidinin adı için *myAppGateway'i* ve yeni kaynak grubu için *myResourceGroupAG'ı* girin.
4. **Bölge**için **(ABD) Orta ABD'yi**seçin.
5. **Katman**için **Standart'ı**seçin.
6. **Sanal ağı Yapılandırma** altında yeni **oluştur'u**seçin ve ardından sanal ağ için bu değerleri girin:
   - *myVNet* - Sanal ağın adı.
   - *10.0.0.0/16* - Sanal ağın adres alanı.
   - *myAGSubnet* - Alt ağın adı.
   - *10.0.0.0/24* - Alt ağın adres alanı.
   - *myBackendSubnet* - arka uç alt net adı için.
   - *10.0.1.0/24* - arka uç alt net adres alanı için.

    ![Sanal ağ oluşturma](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-1.png)

6. Sanal ağ ve alt ağ oluşturmak için **Tamam'ı** seçin.
7. **Sonraki'ni seçin:Frontends.**
8. **Frontend IP adresi türü için** **Özel'i**seçin.

   Varsayılan olarak, dinamik bir IP adresi ataması. Yapılandırılan alt ağın kullanılabilir ilk adresi ön uç IP adresi olarak atanır.
   > [!NOTE]
   > Ayrıldıktan sonra IP adresi türü (statik veya dinamik) daha sonra değiştirilemez.
9. **Sonraki:Backends'i**seçin.
10. **Arka uç havuzu ekle'yi**seçin.
11. **Ad**için , *appGatewayBackendPool*yazın.
12. **Hedefsiz arka uç havuzu ekle**için **Evet'i**seçin. Hedefleri daha sonra eklersiniz.
13. **Ekle'yi**seçin.
14. **Sonraki:Yapılandırma'yı**seçin.
15. **Yönlendirme kuralları altında,** **kural ekle'yi**seçin.
16. **Kural adı**için *Rrule-01*yazın.
17. **Dinleyici adı için,** *Listener-01*yazın.
18. **Frontend IP için** **Özel'i**seçin.
19. Kalan varsayılanları kabul edin ve **Arka Uç hedefleri** sekmesini seçin.
20. **Hedef türü için,** **Backend havuzu**seçin ve sonra **appGatewayBackendPool**seçin.
21. **HTTP ayarı için**yeni **oluştur'u**seçin.
22. **HTTP ayar adı için,** *http-setting-01*yazın.
23. **Arka uç protokolü**için **HTTP'yi**seçin.
24. **Backend bağlantı noktası**için, *80*yazın.
25. Kalan varsayılanları kabul edin ve **Ekle'yi**seçin.
26. Yönlendirme **kuralı** ekle sayfasında **Ekle'yi**seçin.
27. **Sonraki seçin: Etiketler**.
28. **Sonraki'ni Seçin: Gözden Geçirin + oluşturun.**
29. Özet sayfasındaki ayarları gözden geçirin ve ardından ağ kaynaklarını ve uygulama ağ geçidini oluşturmak için **Oluştur'u** seçin. Uygulama ağ geçidinin oluşturulması birkaç dakika sürebilir. Sonraki bölüme geçmeden önce dağıtımın başarıyla bitmesini bekleyin.

## <a name="add-backend-pool"></a>Arka uç havuzu ekleme

Arka uç havuzu, istekleri sağlayan arka uç sunucularına yönlendirmek için kullanılır. Arka uç NIC'lerden, sanal makine ölçek kümelerinden, genel IP adreslerinden, dahili IP adreslerinden, tam nitelikli alan adlarından (FQDN) ve Azure Uygulama Hizmeti gibi çok kiracılı arka uçlardan oluşabilir. Bu örnekte, hedef arka uç olarak sanal makineleri kullanın. Varolan sanal makineleri kullanabilir veya yenilerini oluşturabilirsiniz. Bu örnekte, Azure'un uygulama ağ geçidi için arka uç sunucusu olarak kullandığı iki sanal makine oluşturursunuz.

Bunu yapmak için, siz:

1. İki yeni sanal makineler, *myVM* ve *myVM2,* arka uç sunucuları olarak kullanılan oluşturun.
2. Uygulama ağ geçidinin başarıyla oluşturulduğunu doğrulamak için sanal makinelere IIS yükleyin.
3. Arka uç sunucularını arka uç havuzuna ekleyin.

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. **Kaynak oluştur**’u seçin.
2. **Bilgi İşlem'i** seçin ve ardından **Sanal makine'yi**seçin.
4. Sanal makine için şu değerleri girin:
   - Kaynak grubu için **Resource group** *myResourceGroupAG'ı* seçin.
   - *myVM* - **Sanal makine adı**için .
   - **Görüntü**için **Windows Server 2019 Datacenter'ı** seçin.
   - *azureadmin* - **Kullanıcı adı**için .
   - *Azure123456!* **Şifre**için .
5. Kalan varsayılanları kabul edin ve **Sonraki : Diskler'i**seçin.
6. Varsayılanları kabul edin ve **Sonraki : Ağ' ı**seçin.
7. Sanal ağ için **myVNet** öğesinin seçili olduğundan ve alt ağın **myBackendSubnet** olduğundan emin olun.
8. Kalan varsayılanları kabul edin ve **Sonraki : Yönetim'i**seçin.
9. Önyükleme tanılamasını devre dışı kalmak için **Kapalı'yı** seçin.
10. Kalan varsayılanları kabul edin ve **İleri : İleri 'yi**seçin.
11. Sonraki seçin **: Etiketler**.
12. Sonraki'ni Seçin **: Gözden Geçirin + oluşturun**.
13. Özet sayfasındaki ayarları gözden geçirin ve ardından **Oluştur'u**seçin. VM'nin oluşturulması birkaç dakika sürebilir. Sonraki bölüme geçmeden önce dağıtımın başarıyla bitmesini bekleyin.

### <a name="install-iis"></a>IIS yükleme

1. Bulut Kabuğu'nu açın ve **PowerShell**olarak ayarlandığından emin olun.
    ![özel-frontendip-3](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-3.png)
2. Sanal makineye IIS yüklemek için aşağıdaki komutu çalıştırın:

   ```azurepowershell
   Set-AzVMExtension `
   
     -ResourceGroupName myResourceGroupAG `
   
     -ExtensionName IIS `
   
     -VMName myVM `
   
     -Publisher Microsoft.Compute `
   
     -ExtensionType CustomScriptExtension `
   
     -TypeHandlerVersion 1.4 `

     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `

     -Location CentralUS `

   ```



3. İkinci bir sanal makine oluşturun ve yeni tamamladığınız adımları kullanarak IIS yükleyin. MyVM2'yi adı ve VMName için Set-AzVMExtension'a girin.

### <a name="add-backend-servers-to-backend-pool"></a>Arka uç havuzuna arka uç sunucuları ekleme

1. **Tüm kaynakları**seçin ve ardından **myAppGateway'i**seçin.
2. **Arka uç havuzları**seçin. **AppGatewayBackendPool**seçin.
3. **Hedef türü** altında **Sanal makine** seçin ve **Hedef**altında, myVM ile ilişkili vNIC seçin.
4. MyVM2 eklemek için tekrarlayın.
   ![özel-frontendip-4](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-4.png)
5. **Kaydet'i seçin.**

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

1. Portaldaki **Frontend IP Yapılandırmaları** sayfasına tıklayarak atanan ön uç IP'nizi kontrol edin.
    ![özel-frontendip-5](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-5.png)
2. Özel IP adresini kopyalayın ve ardından aynı VNet'teki bir VM'deki tarayıcı adresi çubuğuna veya bu VNet'e bağlantısı olan şirket içi bağlantıya kopyalayın ve Uygulama Ağ Geçidi'ne erişmeye çalışın.

## <a name="next-steps"></a>Sonraki adımlar

Arka uçlarınızın durumunu izlemek istiyorsanız, [Uygulama Ağ Geçidi için Arka uç sağlık ve tanıgünlüklerine](application-gateway-diagnostics.md)bakın.
