---
title: Otomatik ölçeklendirme ve Alanlar arası yedekli Application Gateway v2
description: Bu makalede, Otomatik ölçekleme ve Bölge yedekli özellikleri içeren Azure Uygulama Standard_v2 ve WAF_v2 SKU tanıtın.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 03/24/2020
ms.author: victorh
ms.openlocfilehash: 4cd2969f9a56c96af2b2c6db216f6829a080260c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80371271"
---
# <a name="autoscaling-and-zone-redundant-application-gateway-v2"></a>Otomatik ölçeklendirme ve Alanlar arası yedekli Application Gateway v2 

Uygulama Ağ Geçidi ve Web Uygulama Güvenlik Duvarı (WAF) de bir Standard_v2 ve WAF_v2 SKU altında mevcuttur. v2 SKU performans geliştirmeleri sunar ve otomatik ölçekleme, bölge artıklığı ve statik VIP'ler için destek gibi kritik yeni özellikler için destek ekler. Standart ve WAF SKU kapsamındaki mevcut özellikler, [karşılaştırma](#differences-with-v1-sku) bölümünde listelenen birkaç istisna dışında yeni v2 SKU'da desteklenmeye devam etmektedir.

Yeni v2 SKU aşağıdaki geliştirmeleri içerir:

- **Otomatik ölçeklendirme**: Otomatik ölçeklendirme SKU altında Uygulama Ağ Geçidi veya WAF dağıtımları, değişen trafik yükü desenlerine bağlı olarak ölçeklenebilir veya küçültülebilir. Otomatik ölçeklendirme ayrıca sağlama sırasında dağıtım boyutu veya örnek sayısı seçme gereksinimini de ortadan kaldırır. Bu SKU gerçek esneklik sunar. Standard_v2 ve WAF_v2 SKU'da, Application Gateway hem sabit kapasitede (otomatikleştirilmiş devre denme) hem de otomatik ölçekleme etkin modunda çalışabilir. Sabit kapasite modu, tutarlı ve öngörülebilir iş yüklerine sahip senaryolar için yararlıdır. Otomatik ölçekleme modu, uygulama trafiğinde varyans gören uygulamalarda yararlıdır.
- **Bölge artıklığı**: Bir Uygulama Ağ Geçidi veya WAF dağıtımı, her bölgede bir Trafik Yöneticisi ile ayrı Uygulama Ağ Geçidi örnekleri sağlama gereksinimini ortadan kaldırarak birden çok Kullanılabilirlik Bölgesi'ni kapsayabilir. Uygulama Ağ Geçidi örneklerinin dağıtıldığı tek bir bölge veya birden çok bölge seçebilirsiniz, bu da onu bölge hatasına karşı daha esnek hale getirir. Uygulamalar için arka uç havuzu da benzer şekilde kullanılabilirlik bölgeleri arasında dağıtılabilir.

  Bölge artıklığı yalnızca Azure Bölgeleri'nin kullanılabildiği yerlerde kullanılabilir. Diğer bölgelerde, diğer tüm özellikler desteklenir. Daha fazla bilgi için [Azure'daki Kullanılabilirlik Bölgeleri nelerdir?](../availability-zones/az-overview.md#services-support-by-region)
- **Statik VIP**: Application Gateway v2 SKU sadece statik VIP türünü destekler. Bu, uygulama ağ geçidiyle ilişkili VIP'nin yeniden başlatmadan sonra bile dağıtımın yaşam döngüsü için değişmemesini sağlar.  v1'de statik bir VIP yoktur, bu nedenle uygulama ağ geçidi aracılığıyla Uygulama Hizmetleri'ne alan adı yönlendirmesi için IP adresi yerine uygulama ağ geçidi URL'sini kullanmanız gerekir.
- **Üstbilgi Yeniden Yazma**: Uygulama Ağ Geçidi, http istek ve yanıt üstbilgilerini v2 SKU ile eklemenize, kaldırmanıza veya güncelleştirmenize olanak tanır. Daha fazla bilgi için, [Uygulama Ağ Geçidi ile HTTP üstbilgilerini yeniden yazma](rewrite-http-headers.md)
- **Key Vault Tümleştirmesi**: Application Gateway v2, HTTPS özellikli dinleyicilere bağlı sunucu sertifikaları için Key Vault ile tümleştirmeyi destekler. Daha fazla bilgi için [Key Vault sertifikalarıile SSL sonlandırma'ya](key-vault-certs.md)bakın.
- **Azure Kubernetes Hizmet Giriş Denetleyicisi**: Uygulama Ağ Geçidi v2 Giriş Denetleyicisi, Azure Uygulama Ağ Geçidi'nin AKS Kümesi olarak bilinen Azure Kubernetes Hizmeti (AKS) için giriş olarak kullanılmasını sağlar. Daha fazla bilgi için [bkz: Uygulama Ağ Geçidi Giriş Denetleyicisi nedir?](ingress-controller-overview.md)
- **Performans geliştirmeleri**: v2 SKU, Standart/WAF SKU ile karşılaştırıldığında 5 kata kadar daha iyi SSL boşaltma performansı sunar.
- **Daha hızlı dağıtım ve güncelleştirme süresi** V2 SKU, Standart/WAF SKU ile karşılaştırıldığında daha hızlı dağıtım ve güncelleme süresi sağlar. Bu, WAF yapılandırma değişikliklerini de içerir.

![](./media/application-gateway-autoscaling-zone-redundant/application-gateway-autoscaling-zone-redundant.png)

## <a name="supported-regions"></a>Desteklenen bölgeler

sku Standard_v2 ve WAF_v2 aşağıdaki bölgelerde mevcuttur: Kuzey Orta ABD, Güney Orta ABD, Batı ABD, Batı ABD 2, Doğu ABD, Doğu ABD 2, Orta ABD, Kuzey Avrupa, Batı Avrupa, Güneydoğu Asya, Fransa Orta, İngiltere Batı, Japonya Doğu, Japonya Batı, Avustralya Doğu , Avustralya Güneydoğu, Brezilya Güney, Kanada Orta, Kanada Doğu, Doğu Asya, Kore Merkez, Kore Güney, İngiltere Güney, Orta Hindistan, Batı Hindistan, Güney Hindistan.

## <a name="pricing"></a>Fiyatlandırma

v2 SKU ile fiyatlandırma modeli tüketime göre yönlendirilir ve artık örnek sayılarına veya boyutlara bağlı değildir. v2 SKU fiyatlandırmaiki bileşenden oluşur:

- **Sabit fiyat** - Bu, bir Standard_v2 veya WAF_v2 Ağ Geçidi sağlamak için saatlik (veya kısmi saat) fiyattır. Lütfen 0 ek minimum örnek hala her zaman sabit fiyat ile birlikte hizmetin yüksek kullanılabilirlik sağlar unutmayın.
- **Kapasite Birim fiyatı** - Sabit maliyete ek olarak tahsil edilen tüketim eki maliyettir. Kapasite birim ücreti de saatlik veya kısmi saatlik olarak hesaplanır. Kapasite biriminin üç boyutu vardır : işlem birimi, kalıcı bağlantılar ve iş ortası. İşlem birimi, tüketilen işlemci kapasitesinin bir ölçüsüdür. Bilgi işlem birimini etkileyen etkenler TLS bağlantıları/sn, URL Yeniden Yazma hesaplamaları ve WAF kural işlemedir. Kalıcı bağlantı, belirli bir fatura aralığında uygulama ağ geçidine kurulan TCP bağlantılarının bir ölçüsüdür. İş verme işlemi, sistem tarafından belirli bir fatura aralığında işlenen ortalama Megabit/sn'dir.  Faturalandırma, ayrılmış örnek sayısının üzerindeki herhangi bir şey için Kapasite Birimi düzeyinde yapılır.

Her kapasite birimi en fazla oluşur: 1 işlem birimi veya 2500 kalıcı bağlantı veya 2,22 Mbps'lik iş ortası.

İşlem birimi kılavuzu:

- **Standard_v2** - RsA 2048 bit anahtar TLS sertifikası ile her işlem birimi saniyede yaklaşık 50 bağlantı kapasitesine sahiptir.
- **WAF_v2** - Her bilgi işlem birimi, %70-30 trafik karışımı için saniyede yaklaşık 10 eşzamanlı isteği destekleyebilir ve %70'i 2 KB GET/POST'tan daha az istekte bulunabilir ve daha yüksek kalır. WAF performansı şu anda yanıt boyutundan etkilenmez.

> [!NOTE]
> Her örnek şu anda yaklaşık 10 kapasite birimini destekleyebilir.
> Bir bilgi işlem biriminin işleyebilir isteklerinin sayısı TLS sertifika anahtar boyutu, anahtar değişimi algoritması, üstbilgi yeniden yazma ve WAF gelen istek boyutu durumunda gibi çeşitli ölçütlere bağlıdır. Bilgi işlem birimi başına istek oranını belirlemek için uygulama testleri yapmanızı öneririz. Hem kapasite birimi hem de işlem birimi, faturalandırma başlamadan önce metrik olarak kullanıma sunulacaktır.

Aşağıdaki tabloda örnek fiyatlar gösterilmektedir ve yalnızca illüstrasyon amaçlıdır.

**ABD Doğu Fiyatlandırma:**

|              SKU Adı                             | Sabit fiyat ($/sa)  | Kapasite Birim fiyatı ($/CU-hr)   |
| ------------------------------------------------- | ------------------- | ------------------------------- |
| Standard_v2                                       |    0,20             | 0.0080                          |
| WAF_v2                                            |    0.36             | 0.0144                          |

Daha fazla fiyatlandırma bilgisi için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/application-gateway/)bakın. 

**Örnek 1**

Bir Uygulama Ağ Geçidi Standard_v2, beş örnekli sabit kapasiteye sahip manuel ölçekleme modunda otomatik ölçekleme yapılmadan sağlanmıştır.

Sabit fiyat = 744(saat) * $0.20 = $148.8 <br>
Kapasite birimleri = 744 (saat) * Örnek başına 10 kapasite birimi * 5 örnek * kapasite birim saati başına $0,008 = $297.6

Toplam fiyat = $148.8 + $297.6 = $446.4

**Örnek 2**

Bir Uygulama Ağ Geçidi standard_v2, sıfır minimum örnekle bir ay süreyle sağlanır ve bu süre zarfında ortalama 8,88 Mbps veri aktarımı olan 25 yeni SSL bağlantısı/sn alır. Bağlantıların kısa ömürlü olduğunu varsayarsak, fiyatınız:

Sabit fiyat = 744(saat) * $0.20 = $148.8

Kapasite birim fiyatı = 744(saat) * Max (bağlantılar/sn için 25/50 işlem birimi, 8,88/2,22 kapasite birimi) * $0.008 = 744 * 4 * 0.008 = $23.81

Toplam fiyat = $148.8+23.81 = $172.61

Gördüğünüz gibi, tüm örnek için değil, yalnızca dört Kapasite Birimi için faturalandırılırsınız. 

> [!NOTE]
> Max işlevi, bir çift değerdeki en büyük değeri döndürür.


**Örnek 3**

Bir Uygulama Ağ Geçidi standard_v2, en az beş örnekle bir ay süreyle verilir. Trafik olmadığını ve bağlantıların kısa ömürlü olduğunu varsayarsak, fiyatınız:

Sabit fiyat = 744(saat) * $0.20 = $148.8

Kapasite birim fiyatı = 744(saat) * Max (bağlantılar/sn için 0/50 işlem birimi, işlem için 0/2,22 kapasite birimi) * $0.008 = 744 * 50 * 0.008 = $297.60

Toplam fiyat = $148.80+297.60 = $446.4

Bu durumda, trafik olmamasına rağmen beş örneğin tamamı için faturalandırılırsınız.

**Örnek 4**

Bir Uygulama Ağ Geçidi standard_v2, en az beş örnekle bir ay süreyle sağlanmıştır, ancak bu sefer ortalama 125 mbps veri aktarımı ve saniyede 25 SSL bağlantısı vardır. Trafik olmadığını ve bağlantıların kısa ömürlü olduğunu varsayarsak, fiyatınız:

Sabit fiyat = 744(saat) * $0.20 = $148.8

Kapasite birim fiyatı = 744(saat) * Max (bağlantılar/sn için 25/50 işlem birimi, 125/2,22 kapasite birimi, 125/2,22 kapasite birimi) * $0.008 = 744 * 57 * 0.008 = $339.26

Toplam fiyat = $148.80+339.26 = $488.06

Bu durumda, tam beş örnek için faturalandırılır, artı yedi Kapasite Birimleri (bir örneğin 7/10).  

**Örnek 5**

Bir Uygulama Ağ Geçidi WAF_v2 bir ay süreyle sağlanmaktadır. Bu süre zarfında, 25 yeni SSL bağlantısı/sn, ortalama 8,88 Mbps veri aktarımı alır ve saniyede 80 istekte bulunmaz. Bağlantıların kısa ömürlü olduğunu ve uygulama için bilgi işlem birimi hesaplamasının işlem birimi başına 10 RPS'yi desteklediğini varsayarsak, fiyatınız şu olacaktır:

Sabit fiyat = 744(saat) * $0.36 = $267.84

Kapasite birim fiyatı = 744(saat) * Max (iletişim birimi Max(25/50 bağlantılar/sn için, 80/10 WAF RPS), 8,88/2,22 kapasite birimi iş artışı için) * $0.0144 = 744 * 8 * 0.0144 = $85.71

Toplam fiyat = $267.84 + $85.71 = $353.55

> [!NOTE]
> Max işlevi, bir çift değerdeki en büyük değeri döndürür.

## <a name="scaling-application-gateway-and-waf-v2"></a>Ölçekleme Uygulama Ağ Geçidi ve WAF v2

Uygulama Ağ Geçidi ve WAF iki modda ölçeklenecek şekilde yapılandırılabilir:

- **Otomatik ölçeklendirme** - Otomatik ölçeklendirme etkinken, Uygulama Ağ Geçidi ve WAF v2 SKUs'lar uygulama trafiği gereksinimlerine göre yukarı veya aşağı ölçeklendirin. Bu mod, uygulamanız için daha iyi esneklik sağlar ve uygulama ağ geçidi boyutunu veya örnek sayısını tahmin etme gereksinimini ortadan kaldırır. Bu mod ayrıca, beklenen maksimum trafik yükü için en yüksek öngörülen kapasitede çalışmasını gerektirmeyerek maliyetten tasarruf etmenizi sağlar. En az ve isteğe bağlı olarak en yüksek örnek sayısı belirtmeniz gerekir. Minimum kapasite, Trafik olmamasında bile Uygulama Ağ Geçidi ve WAF v2'nin belirtilen minimum örnek sayısının altına düşmemesini sağlar. Her örnek 10 ek ayrılmış Kapasite Birimi olarak sayılır. Sıfır hiçbir ayrılmış kapasite anlamına eder ve doğada tamamen otomatikolarak. Sıfır ek minimum örneğin hala her zaman sabit fiyatla birlikte verilen hizmetin yüksek kullanılabilirliğini garanti ettiğini lütfen unutmayın. Ayrıca isteğe bağlı olarak, Uygulama Ağ Geçidi'nin belirtilen örnek sayısının ötesine geçmemesini sağlayan en büyük örnek sayısı da belirtebilirsiniz. Ağ Geçidi tarafından sunulan trafik miktarı için faturalandırılamaya devam eceksiniz. Örnek sayıları 0 ile 125 arasında değişebilir. En yüksek örnek sayısı için varsayılan değer belirtilmemişse 20'dir.
- **Manuel** - Ağ geçidinin otomatik ölçeklenmeyeceği Manuel modu alternatif olarak seçebilirsiniz. Bu modda, Uygulama Ağ Geçidi veya WAF'ın işleyebileceğinden daha fazla trafik varsa, trafik kaybına neden olabilir. Manuel modda, örnek sayısını belirtmek zorunludur. Örnek sayısı 1 ile 125 arasında değişebilir.

## <a name="feature-comparison-between-v1-sku-and-v2-sku"></a>v1 SKU ve v2 SKU arasında özellik karşılaştırması

Aşağıdaki tablo, her SKU ile kullanılabilir özellikleri karşılaştırır.

|                                                   | v1 SKU   | v2 SKU   |
| ------------------------------------------------- | -------- | -------- |
| Otomatik ölçeklendirme                                       |          | &#x2713; |
| Bölge artıklığı                                   |          | &#x2713; |
| Statik VIP                                        |          | &#x2713; |
| Azure Kubernetes Service (AKS) Giriş denetleyicisi |          | &#x2713; |
| Azure Anahtar Kasası tümleştirme                       |          | &#x2713; |
| HTTP(S) üstbilgilerini yeniden yazın                           |          | &#x2713; |
| URL tabanlı yönlendirme                                 | &#x2713; | &#x2713; |
| Birden çok site barındırma                             | &#x2713; | &#x2713; |
| Trafik yeniden yönlendirme                               | &#x2713; | &#x2713; |
| Web Uygulaması Güvenlik Duvarı (WAF)                    | &#x2713; | &#x2713; |
| WAF özel kuralları                                  |          | &#x2713; |
| Güvenli Yuva Katmanı (SSL) sonlandırma            | &#x2713; | &#x2713; |
| Uçuça SSL şifreleme                         | &#x2713; | &#x2713; |
| Oturum benzeşimi                                  | &#x2713; | &#x2713; |
| Özel hata sayfaları                                | &#x2713; | &#x2713; |
| WebSocket desteği                                 | &#x2713; | &#x2713; |
| HTTP/2 desteği                                    | &#x2713; | &#x2713; |
| Bağlantı boşaltma                               | &#x2713; | &#x2713; |

> [!NOTE]
> Otomatik ölçekleme v2 SKU şimdi otomatik olarak arka uç havuzunda tüm kaynakların sağlığını izlemek ve sağlıksız olarak kabul edilir bu arka uç üyeleri vurgulamak için [varsayılan sağlık sondaları](application-gateway-probe-overview.md#default-health-probe) destekler. Varsayılan sistem durumu sondası, özel sonda yapılandırması olmayan arka uçlar için otomatik olarak yapılandırılır. Daha fazla bilgi edinmek için [uygulama ağ geçidindeki sistem durumu sondalarına](application-gateway-probe-overview.md)bakın.

## <a name="differences-with-v1-sku"></a>v1 SKU ile farklar

|Fark|Ayrıntılar|
|--|--|
|Kimlik doğrulama sertifikası|Desteklenmiyor.<br>Daha fazla bilgi için, [Uygulama Ağ Geçidi ile SSL sonuna kadar Genel Bakış](ssl-overview.md#end-to-end-ssl-with-the-v2-sku)bakın.|
|Aynı alt ağda Standard_v2 ve Standart Uygulama Ağ Geçidi'ni karıştırma|Desteklenmiyor|
|Uygulama Ağ Geçidi alt netinde Kullanıcı Tanımlı Rota (UDR)|Desteklenen (belirli senaryolar). Önizlemede.<br> Desteklenen senaryolar hakkında daha fazla bilgi için Bkz. [Uygulama Ağ Geçidi yapılandırmagenel bakışı.](configuration-overview.md#user-defined-routes-supported-on-the-application-gateway-subnet)|
|Gelen bağlantı noktası aralığı için NSG| - Standard_v2 SKU için 65200 - 65535<br>- Standart SKU için 65503 - 65534.<br>Daha fazla bilgi için [SSS](application-gateway-faq.md#are-network-security-groups-supported-on-the-application-gateway-subnet)bölümüne bakın.|
|Azure tanılamada performans günlükleri|Desteklenmiyor.<br>Azure ölçümleri kullanılmalıdır.|
|Faturalandırma|Faturalandırmanın 1 Temmuz 2019'da başlaması planlanmaktadır.|
|FIPS modu|Bunlar şu anda desteklenmez.|
|ILB yalnızca modu|Bu şu anda desteklenmiyor. Ortak ve ILB modu birlikte desteklenir.|
|Netwatcher entegrasyonu|Desteklenmiyor.|
|Azure Güvenlik Merkezi tümleştirmesi|Henüz mevcut değil.

## <a name="migrate-from-v1-to-v2"></a>v1'den v2'ye geçiş

PowerShell galerisinde v1 Application Gateway/WAF'ınızdan v2 Otomatik Ölçekleme SKU'ya geçiş inizi yardımcı olacak bir Azure PowerShell komut dosyası mevcuttur. Bu komut dosyası, v1 ağ geçidinizden yapılandırmayı kopyalamanıza yardımcı olur. Trafik göçü hala sizin sorumluluğunuzdadır. Daha fazla bilgi için bkz: [Azure Uygulama Ağ Geçidi'ni v1'den v2'ye geçirin.](migrate-v1-v2.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı başlangıç: Azure Application Gateway ile doğrudan web trafiği - Azure portalı](quick-create-portal.md)
- [Azure PowerShell'i kullanarak ayrılmış sanal IP adresiyle otomatik ölçekleme, bölge yedekli uygulama ağ geçidi oluşturma](tutorial-autoscale-ps.md)
- [Uygulama Ağ Geçidi](overview.md)hakkında daha fazla bilgi edinin.
- [Azure Güvenlik Duvarı](../firewall/overview.md)hakkında daha fazla bilgi edinin.
