---
title: Portalı kullanarak özel bir sonda oluşturma
titleSuffix: Azure Application Gateway
description: Portalı kullanarak Uygulama Ağ Geçidi için özel bir sonda oluşturmayı öğrenin
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2019
ms.author: victorh
ms.openlocfilehash: 15daf47a1cb44635932311e60b3690af9ff58677
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74074601"
---
# <a name="create-a-custom-probe-for-application-gateway-by-using-the-portal"></a>Portalı kullanarak Uygulama Ağ Geçidi için özel bir sonda oluşturma

> [!div class="op_single_selector"]
> * [Azure portalında](application-gateway-create-probe-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-probe-ps.md)
> * [Azure Klasik PowerShell](application-gateway-create-probe-classic-ps.md)

Bu makalede, Azure portalı üzerinden varolan bir uygulama ağ geçidine özel bir sistem durumu sondası eklersiniz. Azure Application Gateway, sistem durumu sondalarını kullanarak arka uç havuzundaki kaynakların durumunu izler.

## <a name="before-you-begin"></a>Başlamadan önce

Zaten bir uygulama ağ geçidiniz yoksa, çalışmak için bir uygulama ağ geçidi oluşturmak için [Bir Uygulama Ağ Geçidi Oluştur'u](application-gateway-create-gateway-portal.md) ziyaret edin.

## <a name="create-probe-for-application-gateway-v2-sku"></a>Uygulama Ağ Geçidi v2 SKU için sonda oluşturma

Sondalar portal üzerinden iki aşamalı bir süreç içinde yapılandırılır. İlk adım, sonda yapılandırması için gereken değerleri girmektir. İkinci adımda, bu sonda yapılandırmasını kullanarak arka uç durumunu test edin ve sondayı kaydedin. 

### <a name="enter-probe-properties"></a><a name="createprobe"></a>Sonda özelliklerini girin

1. [Azure portalında](https://portal.azure.com)oturum açın. Zaten bir hesabınız yoksa, [bir aylık ücretsiz deneme sürümüne](https://azure.microsoft.com/free) kaydolabilirsiniz

2. Azure Portal Sık Kullanılanlar bölmesinde, Tüm kaynaklar’a tıklayın. Tüm kaynaklar bıçak uygulama ağ geçidi ni tıklatın. Seçili abonelikte zaten çeşitli kaynaklar varsa, DNS bölgesine kolaylıkla erişmek için Ada göre filtrele... kutusuna partners.contoso.net girebilirsiniz.

3. **Sistem Durumu sondalarını** seçin ve ardından yeni bir sistem durumu sondası eklemek için **Ekle'yi** seçin.

   ![Yeni sonda ekleme][4]

4. Sistem **durumu sondası** ekle sayfasında, sonda için gerekli bilgileri doldurun ve tamamlandığında **Tamam'ı**seçin.

   |**Ayar** | **Değer** | **Şey**|
   |---|---|---|
   |**Adı**|customProbe|Bu değer, portalda erişilebilen sondaya verilen dostça bir addır.|
   |**Protokolü**|HTTP veya HTTPS | Sağlık sondasının kullandığı protokol. |
   |**Konak**|Yani contoso.com|Bu değer, uygulama sunucusunda çalışan sanal ana bilgisayar (VM ana bilgisayar adından farklı) adıdır. Sonda (protocol)://(ana bilgisayar adı):(port from httpsetting)/urlPath'e gönderilir.  Bu, Uygulama Ağ Geçidi'nde çok siteli yapılandırıldığınızda geçerlidir. Uygulama Ağ Geçidi tek bir site için yapılandırıldıysa, '127.0.0.1' girin.|
   |**Arka uç HTTP ayarlarından ana bilgisayar adını seçme**|Evet veya Hayır|Sondadaki *ana bilgi nakış,* bu sondanın ilişkili olduğu HTTP Ayarı ile ilişkili arka uç havuzundaki arka uç kaynağının ana bilgisayar adına ayarlar. Azure uygulama hizmeti gibi çok kiracılı arka uçlar için özel olarak gereklidir. [Daha fazlasını öğrenin](https://docs.microsoft.com/azure/application-gateway/configuration-overview#pick-host-name-from-back-end-address)|
   |**Yol**|/ veya başka bir yol|Özel sonda için tam url geri kalanı. Geçerli bir yol '/' ile başlar. http varsayılan yolu için:\//contoso.com sadece '/' kullanın |
   |**Aralık (saniye)**|30|Sonda sağlık kontrolü için ne sıklıkta çalıştırılır. 30 saniyeden daha düşük ayarlamak için tavsiye edilmez.|
   |**Zaman arası (saniye)**|30|Sondanın zamanlamadan önce bekleme süresi. Bu zaman dışında geçen süre içinde geçerli bir yanıt alınmazsa, sonda başarısız olarak işaretlenir. Zaman aralığı, arka uç sistem durumu sayfasının kullanılabilir olduğundan emin olmak için bir http aramasının yapılabildiği kadar yüksek olmalıdır. Zaman aralığı değerinin bu sonda ayarında kullanılan 'Interval' değerinden veya bu sondayla ilişkilendirilecek HTTP ayarındaki 'İstek zaman aşımı' değerinden fazla olmaması gerektiğini unutmayın.|
|**Sağlıksız durum eşiği**|3|Art arda başarısız olan girişimlerin sayısı sağlıksız olarak kabul edilecek. Eşik 1 veya daha fazla ayarlanabilir.|
   |**Sonda eşleştirme koşullarını kullanma**|Evet veya Hayır|Varsayılan olarak, 200 ve 399 arasında durum kodu ile bir HTTP(S) yanıtı sağlıklı olarak kabul edilir. Kabul edilebilir arka uç yanıt kodu veya arka uç yanıt gövdesi aralığını değiştirebilirsiniz. [Daha fazlasını öğrenin](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#probe-matching)|
   |**HTTP Ayarları**|açılır dan seçim|Sonda burada seçilen HTTP ayarı(lar) ile ilişkili alırsınız ve bu nedenle, seçilen HTTP ayarı ile ilişkili olan bu arka uç havuzunun durumunu izler. Sonda isteği için seçilen HTTP ayarında kullanılan bağlantı noktası yla aynı bağlantı noktasını kullanır. Yalnızca başka bir özel sondayla ilişkili olmayan HTTP ayarını(lar) seçebilirsiniz. <br>Yalnızca bu HTTP ayarı(lar) bu sonda yapılandırmasında seçilen protokolile aynı protokole sahip olan ve *Arka Uç http ayar anahtarından Ana Bilgisayar Adı Seç* için aynı duruma sahip ilişkilendirme için kullanılabilir olduğunu unutmayın.|
   
   > [!IMPORTANT]
   > Sonda, yalnızca bir veya daha fazla HTTP Ayarı(lar) ile ilişkilendirildiğinde arka uç takiben in sağlığını izler. Bu sonda ile ilişkili HTTP ayarı(lar) ile ilişkili bu arka uç havuzları nın arka uç kaynaklarını izler. Sonda isteği http://(ana bilgisayar adı):(port from httpsetting)/urlPath adresine gönderilecektir.

### <a name="test-backend-health-with-the-probe"></a>Sonda ile arka uç sağlığını test edin

Sonda özelliklerini girdikten sonra, sonda yapılandırmasının doğru olduğunu ve arka uç kaynaklarının beklendiği gibi çalıştığını doğrulamak için arka uç kaynaklarının sistem durumunu sınayabilirsiniz.

1. **Test'i** seçin ve sondanın sonucunu not edin. Uygulama ağ geçidi, bu sonda için kullanılan HTTP Ayarı(lar) ile ilişkili arka uç havuzlarında tüm arka uç kaynaklarının durumunu sınar. 

   ![Arka uç sağlığını test edin][5]

2. Sağlıksız arka uç kaynakları varsa, kaynağın sağlıksız durumunu anlamak için **Ayrıntılar** sütununa bakın. Kaynak yanlış bir sonda yapılandırması nedeniyle sağlıksız işaretlenmişse, **sonda bağlantısına geri dön'ünü** seçin ve sonda yapılandırmasını düzeltin. Aksi takdirde, kaynak arka uçla ilgili bir sorun nedeniyle sağlıksız olarak işaretlenmişse, arka uç kaynağıyla ilgili sorunları giderin ve ardından **sonda bağlantısına geri dön** bağlantısını seçerek arka uçla tekrar test edin ve **Test'i**seçin.

   > [!NOTE]
   > Sondayı sağlıksız arka uç kaynaklarıyla bile kaydetmeyi seçebilirsiniz, ancak bu önerilmez. Bunun nedeni, Uygulama Ağ Geçidi'nin sonda tarafından sağlıksız olduğu belirlenen arka uç kaynaklarını arka uç havuzundan kaldırmasıdır. Arka uç havuzunda sağlıklı kaynak yoksa, başvurunuza erişemeseniz de 502 hatası alırsınız.

   ![Sonda sonucunu görüntüleme][6]

3. Sondayı kaydetmek için **Ekle'yi** seçin. 

## <a name="create-probe-for-application-gateway-v1-sku"></a>Uygulama Ağ Geçidi v1 SKU için sonda oluşturma

Sondalar portal üzerinden iki aşamalı bir süreç içinde yapılandırılır. İlk adım sonda oluşturmaktır. İkinci adımda, sondayı uygulama ağ geçidinin arka uç http ayarlarına eklersiniz.

### <a name="create-the-probe"></a><a name="createprobe"></a>Sondayı oluşturma

1. [Azure portalında](https://portal.azure.com)oturum açın. Zaten bir hesabınız yoksa, [bir aylık ücretsiz deneme sürümüne](https://azure.microsoft.com/free) kaydolabilirsiniz

2. Azure Portal Sık Kullanılanlar bölmesinde, **Tüm kaynaklar**’ı seçin. **Tüm kaynaklar** sayfasındaki uygulama ağ geçidini seçin. Seçili abonelikte zaten çeşitli kaynaklar varsa, DNS bölgesine kolaylıkla erişmek için Ada göre filtrele... kutusuna partners.contoso.net girebilirsiniz.

3. **Problar'ı** seçin ve ardından sonda eklemek için **Ekle'yi** seçin.

   ![Doldurulan bilgilerle Sonda bıçağı ekle][1]

4. Sağlık **sondası ekle** bıçağında, sonda için gerekli bilgileri doldurun ve tamamlandığında **Tamam'ı**seçin.

   |**Ayar** | **Değer** | **Şey**|
   |---|---|---|
   |**Adı**|customProbe|Bu değer, portalda erişilebilen sondaya verilen dostça bir addır.|
   |**Protokolü**|HTTP veya HTTPS | Sağlık sondasının kullandığı protokol. |
   |**Konak**|Yani contoso.com|Bu değer, uygulama sunucusunda çalışan sanal ana bilgisayar (VM ana bilgisayar adından farklı) adıdır. Sonda (protocol)://(ana bilgisayar adı):(port from httpsetting)/urlPath'e gönderilir.  Bu, Uygulama Ağ Geçidi'nde çok siteli yapılandırıldığınızda geçerlidir. Uygulama Ağ Geçidi tek bir site için yapılandırıldıysa, '127.0.0.1' girin.|
   |**Arka uç HTTP ayarlarından ana bilgisayar adını seçme**|Evet veya Hayır|Sondadaki *ana bilgi nakış,* bu sondanın ilişkili olduğu HTTP Ayarı ile ilişkili arka uç havuzundaki arka uç kaynağının ana bilgisayar adına ayarlar. Azure uygulama hizmeti gibi çok kiracılı arka uçlar için özel olarak gereklidir. [Daha fazlasını öğrenin](https://docs.microsoft.com/azure/application-gateway/configuration-overview#pick-host-name-from-back-end-address)|
   |**Yol**|/ veya başka bir yol|Özel sonda için tam url geri kalanı. Geçerli bir yol '/' ile başlar. http varsayılan yolu için:\//contoso.com sadece '/' kullanın |
   |**Aralık (saniye)**|30|Sonda sağlık kontrolü için ne sıklıkta çalıştırılır. 30 saniyeden daha düşük ayarlamak için tavsiye edilmez.|
   |**Zaman arası (saniye)**|30|Sondanın zamanlamadan önce bekleme süresi. Bu zaman dışında geçen süre içinde geçerli bir yanıt alınmazsa, sonda başarısız olarak işaretlenir. Zaman aralığı, arka uç sistem durumu sayfasının kullanılabilir olduğundan emin olmak için bir http aramasının yapılabildiği kadar yüksek olmalıdır. Zaman aralığı değerinin bu sonda ayarında kullanılan 'Interval' değerinden veya bu sondayla ilişkilendirilecek HTTP ayarındaki 'İstek zaman aşımı' değerinden fazla olmaması gerektiğini unutmayın.|
|**Sağlıksız durum eşiği**|3|Art arda başarısız olan girişimlerin sayısı sağlıksız olarak kabul edilecek. Eşik 1 veya daha fazla ayarlanabilir.|
   |**Sonda eşleştirme koşullarını kullanma**|Evet veya Hayır|Varsayılan olarak, 200 ve 399 arasında durum kodu ile bir HTTP(S) yanıtı sağlıklı olarak kabul edilir. Kabul edilebilir arka uç yanıt kodu veya arka uç yanıt gövdesi aralığını değiştirebilirsiniz. [Daha fazlasını öğrenin](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#probe-matching)|

   > [!IMPORTANT]
   > Ana bilgisayar adı sunucu adı ile aynı değildir. Bu değer, uygulama sunucusunda çalışan sanal ana bilgisayarın adıdır. Sonda http://(ana bilgisayar adı):(port için httpsetting)/urlPath gönderilir

### <a name="add-probe-to-the-gateway"></a>Ağ geçidine sonda ekleme

Sonda oluşturulduğuna göre, onu ağ geçidine ekleme nin zamanı geldi. Sonda ayarları uygulama ağ geçidinin arka uç http ayarlarında ayarlanır.

1. Yapılandırma bıçak getirmek için uygulama ağ geçidinde **HTTP ayarlarını** tıklatın pencerede listelenen geçerli arka uç http ayarları tıklayın.

   ![https ayarlar penceresi][2]

2. **AppGatewayBackEndHttpSettings** ayarları sayfasında, **Özel sonda** yı keskit kutusunu kullanın ve Özel **sonda** açılır durumda [sonda](#createprobe) yı oluştur bölümünde oluşturulan sondayı seçin.
   Tamamlandığında **Kaydet'i** tıklatın ve ayarlar uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

Arka uç sistem durumu görünümünü kullanarak sonda tarafından belirlenen arka uç kaynaklarının [durumunu görüntüleyin.](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#back-end-health)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png
[4]: ./media/application-gateway-create-probe-portal/figure4.png
[5]: ./media/application-gateway-create-probe-portal/figure5.png
[6]: ./media/application-gateway-create-probe-portal/figure6.png
