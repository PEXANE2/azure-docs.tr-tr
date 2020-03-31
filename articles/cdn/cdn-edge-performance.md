---
title: Azure CDN'de kenar düğümü performansını analiz edin | Microsoft Dokümanlar
description: Microsoft Azure CDN'de kenar düğümü performansını analiz edin. Edge Performance Analytics, CDN için ayrıntılı bilgi trafiği ve bant genişliği kullanımı sağlar.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: 8cc596a7-3e01-4f76-af7b-a05a1421517e
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: b8a65d4ae6aaac78e642c851a66b745a940fa0ad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67593910"
---
# <a name="analyze-edge-node-performance-in-microsoft-azure-cdn"></a>Microsoft Azure CDN’de kenar düğümü performansını çözümleme
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Genel Bakış
Kenar performans analitiği, CDN için parçalı bilgi trafiği ve bant genişliği kullanımı sağlar. Bu bilgiler daha sonra, varlıklarınızın önbelleğe alınıp müşterilerinize nasıl teslim edildiği hakkında fikir edinmenize olanak tanıyan popüler istatistikler oluşturmak için kullanılabilir. Buna karşılık, bu, içeriğinizin teslimini optimize etme ve CDN'den daha iyi yararlanmak için hangi sorunların ele alınması gerektiğini belirleme konusunda bir strateji oluşturmanıza olanak tanır. Sonuç olarak, yalnızca veri teslim performansını artırmakla kalmamış, CDN maliyetlerinizi de azaltabilirsiniz.

> [!NOTE]
> Bir tarih/saat belirtirken tüm raporlar UTC/GMT gösterimini kullanır.
> 
> 

## <a name="reports-and-log-collection"></a>Raporlar ve günlük toplama
CDN etkinlik verileri, hakkında rapor oluşturabilmesi için Edge Performance Analytics modülü tarafından toplanmalıdır. Bu toplama işlemi günde bir kez gerçekleşir ve önceki gün gerçekleşen etkinliği kapsar. Bu, bir raporun istatistiklerinin işlenirken günün istatistiklerinin bir örneğini temsil ettiği ve geçerli günün tüm veri kümesini içermediği anlamına gelir. Bu raporların birincil işlevi performansı değerlendirmektir. Bunlar faturalandırma amaçları veya tam sayısal istatistikler için kullanılmamalıdır.

> [!NOTE]
> Edge Performance Analytic raporlarının oluşturulduğu ham veriler en az 90 gün boyunca kullanılabilir.
> 
> 

## <a name="dashboard"></a>Pano
Edge Performance Analytics panosu, geçerli ve geçmiş CDN trafiğini bir grafik ve istatistikler aracılığıyla izler. Hesabınız için CDN trafiğinin performansıyla ilgili son ve uzun vadeli eğilimleri algılamak için bu panoyu kullanın.

Bu pano aşağıdakilerden oluşur:

* Önemli ölçümlerin ve eğilimlerin görselleştirilmesine olanak tanıyan etkileşimli bir grafik.
* Önemli ölçümler ve eğilimler için uzun vadeli desenler duygusu sağlayan bir zaman çizelgesi.
* CDN ağımızın genel performans, kullanım ve verimlilikle ölçüldüğü üzere site trafiğini nasıl iyileştirdüğü hakkında temel ölçümler ve istatistiksel bilgiler.

### <a name="accessing-the-edge-performance-dashboard"></a>Kenar performans panosuna erişim
1. CDN profil bıçağından **Yönet** düğmesini tıklatın.
   
    ![CDN profil blade yönetme düğmesi](./media/cdn-edge-performance/cdn-manage-btn.png)
   
    CDN yönetim portalı açılır.
2. **Analytics** sekmesinin üzerine, ardından Edge **Performance Analytics** flyout'un üzerinde gezin.  **Pano'ya**tıklayın.
   
    Kenar düğümü analiz panosu görüntülenir.

### <a name="chart"></a>Grafik
Pano, zaman çizelgesinde seçilen zaman çizelgesinde doğrudan altında görünen bir metrik izleyen bir grafik içerir.  CDN etkinliğinin son iki yılını gösteren bir zaman çizelgesi grafiğin hemen altında görüntülenir.

#### <a name="using-the-chart"></a>Grafiği kullanma
* Varsayılan olarak, son 30 gün için önbellek verimlilik oranı grafiklenir.
* Bu grafik, günlük olarak harmanlanmış verilerden oluşturulur.
* Satır grafiğinde bir gün boyunca gezinmek, o tarihteki bir tarihi ve metnin değerini gösterir.
* Hafta sonlarını grafikte temsil eden açık gri dikey çubukların bir bindirmesini geçişyapmak için Hafta Sonlarını Vurgula'yı tıklatın. Bu tür bir kaplama, hafta sonları trafik düzenini belirlemek için yararlıdır.
* Bir önceki yılın tümlemesini aynı zaman diliminde grafiğe kaydırmak için Bir Yıl Önce Görüntüle'yi tıklatın. Bu tür karşılaştırmalar, uzun vadeli CDN kullanım kalıplarına ilişkin içgörü sağlar. Grafiğin sağ üst köşesinde, her satır grafiği için renk kodunu gösteren bir gösterge içerir.

#### <a name="updating-the-chart"></a>Grafiği güncelleştirme
* Zaman Aralığı: Aşağıdakilerden birini gerçekleştirin:
  * Zaman çizelgesinde istenilen bölgeyi seçin. Grafik, seçilen döneme karşılık gelen verilerle güncelleştirilir.
  * Mevcut tüm geçmiş verilerini en fazla iki yıla kadar görüntülemek için grafiğe çift tıklayın.
* Metrik: İstenilen metnin yanında görünen grafik simgesini tıklatın. Grafik ve zaman çizelgesi, ilgili metrik için verilerle yenilenir.

### <a name="key-metrics-and-statistics"></a>Temel ölçümler ve istatistikler
#### <a name="efficiency-metrics"></a>Verimlilik ölçümleri
Bu ölçümlerin amacı önbellek verimliliğinin artırılıp geliştirilemeyeceğini görmektir. Önbellek verimliliğinden elde edilen başlıca faydalar şunlardır:

* Kaynak sunucusundaki azaltılmış yük:
  * Daha iyi web sunucusu performansı.
  * Azaltılmış işletme maliyetleri.
* Daha fazla istek doğrudan CDN'den sunulacak olduğundan, geliştirilmiş veri teslim ivmesi.

| Alan | Açıklama |
| --- | --- |
| Önbellek Verimliliği |Önbellekten aktarılan veri yüzdesini gösterir. Bu metrik, istenen içeriğin önbelleğe alınmış bir sürümünün doğrudan CDN'den (kenar sunucuları) istekte bulundurculara (örn. web tarayıcısı) sunulduğunda ölçer |
| Isabet Oranı |Önbellekten sunulan isteklerin yüzdesini gösterir. Bu metrik, istenen içeriğin önbelleğe alınmış bir sürümünün doğrudan CDN'den (kenar sunucuları) talep edenlere (örn. web tarayıcısı) sunulduğunu ölçer. |
| Uzak Baytların %'si - Önbellek Config yok |Başlangıç sunucularından CdN'ye (kenar sunucuları) sunulan ve Bypass Önbellek özelliği (HTTP Rules Engine) sonucunda önbelleğe alınmayacak trafik yüzdesini gösterir. |
| Uzak Baytların %'si - Süresi Dolmuş Önbellek |Eski içerik yeniden validasyonu sonucunda kaynak sunucularından CDN'ye (kenar sunucuları) sunulan trafiğin yüzdesini gösterir. |

#### <a name="usage-metrics"></a>Ölçümleri kullanma
Bu ölçümlerin amacı, aşağıdaki maliyet düşürücü önlemlerhakkında bilgi sağlamaktır:

* CDN ile işletme maliyetlerini en aza indirmek.
* Önbellek verimliliği ve sıkıştırma yoluyla CDN harcamalarını azaltma.

> [!NOTE]
> Trafik hacmi numaraları, oranların ve yüzdelerin hesaplamalarında kullanılan trafiği temsil eder ve yüksek hacimli müşteriler için toplam trafiğin yalnızca bir kısmını gösterebilir.
> 
> 

| Alan | Açıklama |
| --- | --- |
| Ave Bayt Dışarı |CDN'den (kenar sunucuları) istenene (örn. web tarayıcısı) sunulan her istek için aktarılan ortalama bayt sayısını gösterir. |
| Önbellek Config Bayt Oranı Yok |CDN'den (kenar sunucuları) Bypass Önbellek özelliği nedeniyle önbelleğe alınmayacak olan istekte cisime (örn. web tarayıcısı) sunulan trafik yüzdesini gösterir. |
| Sıkıştırılmış Bayt Oranı |CDN'den (kenar sunucuları) sıkıştırılmış biçimde istekte bulunanlara (örn. web tarayıcısı) gönderilen trafik yüzdesini gösterir. |
| Bayt Çıkış |CDN'den (kenar sunucuları) istekte bulunana (örn. web tarayıcısı) teslim edilen baytlarda bulunan veri miktarını gösterir. |
| Bayt Lar |İstekçilerden CDN'ye (kenar sunucuları) gönderilen baytlarda bulunan veri miktarını gösterir. |
| Bayt Uzaktan Kumanda |CDN ve müşteri kaynağı sunucularından CDN'ye (kenar sunucuları) gönderilen baytlarda bulunan veri miktarını gösterir. |

#### <a name="performance-metrics"></a>Performans Ölçümleri
Bu ölçümlerin amacı, trafiğiniz için genel CDN performansını izlemektir.

| Alan | Açıklama |
| --- | --- |
| Transfer Oranı |İçeriğin CDN'den istekte bulunan bir kullanıcıya aktarıldığı ortalama oranı gösterir. |
| Süre |Bir varlığı bir istekte bulunana (örn. web tarayıcısı) teslim etmek için gereken ortalama süreyi milisaniye cinsinden gösterir. |
| Sıkıştırılmış İstek Oranı |CDN'den (kenar sunucuları) sıkıştırılmış bir biçimde istekte bulunana (örn. web tarayıcısı) teslim edilen isabet lerin yüzdesini gösterir. |
| 4xx Hata Oranı |4xx durum kodu oluşturulan isabet yüzdesini gösterir. |
| 5xx Hata Oranı |5xx durum kodu oluşturulan isabet yüzdesini gösterir. |
| İsabetler |CDN içeriği için istek sayısını gösterir. |

#### <a name="secure-traffic-metrics"></a>Güvenli Trafik Ölçümleri
Bu ölçümlerin amacı, HTTPS trafiği için CDN performansını izlemektir.

| Alan | Açıklama |
| --- | --- |
| Güvenli Önbellek Verimliliği |Önbellekten sunulan HTTPS istekleri için aktarılan verilerin yüzdesini gösterir. Bu metrik, istenen içeriğin önbelleğe alınmış bir sürümünün doğrudan CDN'den (kenar sunucuları) HTTPS üzerinden talepte bulundurculara (örn. web tarayıcısı) sunulduğunu ölçer. |
| Güvenli Aktarım Hızı |İçeriğin CDN'den (kenar sunucuları) HTTPS üzerinden istekte bulunanlara (örn. web sunucuları) aktarıldığı ortalama oranı gösterir. |
| Ortalama Güvenli Süre |Bir varlığı https üzerinden bir istekte bulunana (örn. web tarayıcısı) teslim etmek için gereken ortalama süreyi milisaniye cinsinden belirtir. |
| Güvenli Vuruşlar |CDN içeriği için HTTPS isteklerinin sayısını gösterir. |
| Güvenli Bayt Lar Dışarı |CDN'den (kenar sunucuları) istekte bulunana (örn. web tarayıcısı) teslim edilen BAYtlar halindeki HTTPS trafiğinin miktarını gösterir. |

## <a name="reports"></a>Reports
Bu modüldeki her rapor, farklı ölçüm türleri (örneğin, HTTP durum kodları, önbellek durum kodları, istek URL'si, vb.) için bant genişliği ve trafik kullanımına ilişkin bir grafik ve istatistik içerir. Bu bilgiler, içeriğin müşterilerinize nasıl sunulduğuhakkında daha derin bilgi vermek ve veri teslim performansını artırmak için CDN davranışında ince ayar yapmak için kullanılabilir.

### <a name="accessing-the-edge-performance-reports"></a>Kenar performans raporlarına erişim
1. CDN profil bıçağından **Yönet** düğmesini tıklatın.
   
    ![CDN profil blade yönetme düğmesi](./media/cdn-edge-performance/cdn-manage-btn.png)
   
    CDN yönetim portalı açılır.
2. **Analytics** sekmesinin üzerine, ardından Edge **Performance Analytics** flyout'un üzerinde gezin.  HTTP **Büyük Nesne'ye**tıklayın.
   
    Kenar düğümü analiz raporları ekranı görüntülenir.

| Rapor | Açıklama |
| --- | --- |
| Günlük Özet |Belirli bir zaman dilimi içinde günlük trafik eğilimlerini görüntülemenizi sağlar. Bu grafikteki her çubuk belirli bir tarihi temsil eder. Çubuğun boyutu, o tarihte gerçekleşen toplam isabet sayısını gösterir. |
| Saatlik Özet |Belirli bir zaman dilimi içinde saatlik trafik eğilimlerini görüntülemenizi sağlar. Bu grafikteki her çubuk belirli bir tarihteki tek bir saati temsil eder. Çubuğun boyutu, o saat içinde gerçekleşen toplam isabet sayısını gösterir. |
| Protokoller |HTTP ve HTTPS protokolleri arasındaki trafiğin dökümünü görüntüler. Donut grafiği, her protokol türü için gerçekleşen isabet yüzdesini gösterir. |
| HTTP Yöntemleri |Verilerinizi istemek için HTTP yöntemlerinin kullanıldığı hakkında hızlı bir fikir edinmenizi sağlar. Genellikle, en yaygın HTTP istek yöntemleri GET, HEAD ve POST'tür. Donut grafiği, her BIR HTTP istek yöntemi türü için gerçekleşen isabet yüzdesini gösterir. |
| URL'ler |İstenilen en iyi 10 URL'yi görüntüleyen bir grafik içerir. Her URL için bir çubuk görüntülenir. Çubuğun yüksekliği, belirli bir URL'nin raporun kapsadığı zaman aralığında kaç isabet oluşturduğunu gösterir. İstenen ilk 100 URL'nin istatistikleri bu grafiğin hemen altında görüntülenir. |
| KNAMEs |Bir raporun zaman aralığında varlık istemek için kullanılan en iyi 10 CNAM'ı görüntüleyen bir grafik içerir. İstenen ilk 100 CNAM'ın istatistikleri bu grafiğin hemen altında görüntülenir. |
| Kökeni |Varlıkların belirli bir süre içinde istendiği en iyi 10 CDN veya müşteri kökenli sunucuları görüntüleyen bir grafik içerir. İstenen ilk 100 CDN veya müşteri kaynağı sunucunun istatistikleri bu grafiğin hemen altında görüntülenir. Müşteri kökenli sunucular, Dizin Adı seçeneğinde tanımlanan adla tanımlanır. |
| Geo POP'lar |Trafiğinizin ne kadarının belirli bir durum noktası (POP) üzerinden yönlendirildiğini gösterir. Üç harfli kısaltma, CDN ağımızda bir POP'u temsil eder. |
| İstemciler |Belirli bir süre içinde varlıkları isteyen en iyi 10 istemciyi görüntüleyen bir grafik içerir. Bu raporun amaçları doğrultusunda, aynı IP adresinden gelen tüm istekler aynı istemciden gelmiş olarak kabul edilir. En iyi 100 istemcinin istatistikleri doğrudan bu grafiğin altında görüntülenir. Bu rapor, en iyi müşterileriniz için indirme etkinlik modellerini belirlemek için yararlıdır. |
| Önbellek Durumları |Genel son kullanıcı deneyimini geliştirmeye yönelik yaklaşımları ortaya çıkarabilecek önbellek davranışının ayrıntılı bir dökümünü verir. En hızlı performans önbellek isabetlerinden geldiği için, önbellek eksikliklerini ve süresi dolmuş önbellek isabetlerini en aza indirerek veri teslim hızlarını en iyi duruma getirebilirsiniz. |
| YOK Ayrıntılar |Önbellek içeriği tazeliğinin belirli bir süre boyunca denetlenmediğinin varlıkları için en iyi 10 URL'yi görüntüleyen bir grafik içerir. Bu tür varlıklar için en iyi 100 URL'nin istatistikleri bu grafiğin hemen altında görüntülenir. |
| CONFIG_NOCACHE Detayları |Müşterinin CDN yapılandırması nedeniyle önbelleğe alınmayan varlıklar için en iyi 10 URL'yi görüntüleyen bir grafik içerir. Bu tür varlıklar doğrudan kaynak sunucusundan sunuldu. Bu tür varlıklar için en iyi 100 URL'nin istatistikleri bu grafiğin hemen altında görüntülenir. |
| ÖNBELLEMEZ Ayrıntılar |İstek üstbilgisi verileri nedeniyle önbelleğe alınamayan varlıklar için en iyi 10 URL'yi görüntüleyen bir grafik içerir. Bu tür varlıklar için en iyi 100 URL'nin istatistikleri bu grafiğin hemen altında görüntülenir. |
| TCP_HIT Detayları |Önbellekten hemen servis edilen varlıklar için en iyi 10 URL'yi görüntüleyen bir grafik içerir. Bu tür varlıklar için en iyi 100 URL'nin istatistikleri bu grafiğin hemen altında görüntülenir. |
| TCP_MISS Detayları |önbellek durumu TCP_MISS olan varlıklar için en iyi 10 URL'yi görüntüleyen bir grafik içerir. Bu tür varlıklar için en iyi 100 URL'nin istatistikleri bu grafiğin hemen altında görüntülenir. |
| TCP_EXPIRED_HIT Detayları |Doğrudan POP'tan sunulan eski varlıklar için en iyi 10 URL'yi görüntüleyen bir grafik içerir. Bu tür varlıklar için en iyi 100 URL'nin istatistikleri bu grafiğin hemen altında görüntülenir. |
| TCP_EXPIRED_MISS Detayları |Yeni bir sürümün kaynak sunucudan alınması gereken eski varlıklar için en iyi 10 URL'yi görüntüleyen bir grafik içerir. Bu tür varlıklar için en iyi 100 URL'nin istatistikleri bu grafiğin hemen altında görüntülenir. |
| TCP_CLIENT_REFRESH_MISS Detayları |İstemciden gelen önbellek yok isteği nedeniyle bir başlangıç sunucusundan alınan varlıkların en iyi 10 URL'sini görüntüleyen bir çubuk grafik içerir. Bu tür istekler için en iyi 100 URL'nin istatistikleri doğrudan bu grafiğin altında görüntülenir. |
| İstemci İstek Türleri |HTTP istemcileri (örn. tarayıcılar) tarafından yapılan istek türünü gösterir. Bu rapor, isteklerin nasıl işlendiğine dair bir fikir veren bir donut grafiği içerir. Her istek türü için bant genişliği ve trafik bilgileri grafiğin altında görüntülenir. |
| Kullanıcı Aracısı |CDN'miz aracılığıyla içeriğinizi istemek için en iyi 10 kullanıcı aracısını görüntüleyen bir çubuk grafik içerir. Genellikle, bir kullanıcı aracısı bir web tarayıcısı, medya oynatıcı veya bir cep telefonu tarayıcısıdır. En iyi 100 kullanıcı aracısının istatistikleri bu grafiğin hemen altında görüntülenir. |
| Referanslar |CDN'miz aracılığıyla erişilen içeriğe ilk 10 atıfta bulunanları gösteren bir çubuk grafik içerir. Genellikle, bir başvuru, web sayfasının veya içeriğinize bağlantı veren kaynağın URL'sidir. En iyi 100 başvurucu için grafiğin altında ayrıntılı bilgi verilmiştir. |
| Sıkıştırma Türleri |İstenen varlıkları kenar sunucularımız tarafından sıkıştırılıp sıkıştırılmadığına göre ayıran bir donut grafiği içerir. Sıkıştırılmış varlıkların yüzdesi kullanılan sıkıştırma türüne göre bölünur. Her sıkıştırma türü ve durumu için grafiğin altında ayrıntılı bilgi verilmiştir. |
| Dosya Türleri |Hesabınız için CDN aracılığıyla istenen en iyi 10 dosya türünü görüntüleyen bir çubuk grafiği içerir. Bu raporun amaçları doğrultusunda, bir dosya türü varlığın dosya adı uzantısı ve Internet ortam türü \[(örn.\].html \[text/html\], .htm text/html , .aspx \[text/html,\]vb.) tarafından tanımlanır. En iyi 100 dosya türü için grafiğin altında ayrıntılı bilgi sağlanır. |
| Benzersiz Dosyalar |Belirli bir süre içinde belirli bir günde istenen benzersiz varlıkların toplam sayısını çizen bir grafik içerir. |
| Token Auth Özeti |İstenen varlıkların Token Tabanlı Kimlik Doğrulama tarafından korunup korunmadığına ilişkin hızlı bir genel bakış sağlayan bir pasta grafiği içerir. Korumalı varlıklar, kimlik doğrulama denemelerinin sonuçlarına göre grafikte görüntülenir. |
| Token Auth Ayrıntılarını Reddet |Belirteç Tabanlı Kimlik Doğrulama nedeniyle reddedilen en iyi 10 isteği görüntülemenizi sağlayan bir çubuk grafik içerir. |
| HTTP Yanıt Kodları |En son sunucularımız tarafından HTTP istemcilerinize teslim edilen HTTP durum kodlarının (örneğin, 200 Ok, 403 Forbidden, 404 Bulunamadı, vb.) dökümünü sağlar. Pasta grafiği, varlıklarınızın nasıl sunulduğunun hızla değerlendirilmesini sağlar. Grafiğin altındaki her yanıt kodu için ayrıntılı istatistiksel veriler sağlanır. |
| 404 Hatalar |404 Bulunamadı yanıt koduyla sonuçlanan en iyi 10 isteği görüntülemenize olanak tanıyan bir çubuk grafik içerir. |
| 403 Hatalar |403 Yasak yanıt koduyla sonuçlanan en iyi 10 isteği görüntülemenize olanak tanıyan bir çubuk grafik içerir. 403 Yasak yanıt kodu, bir istek müşteri kökenli bir sunucu veya POP'umuzdaki bir kenar sunucusu tarafından reddedildiğinde oluşur. |
| 4xx Hatalar |400 aralığında yanıt koduyla sonuçlanan en iyi 10 isteği görüntülemenize olanak tanıyan bir çubuk grafik içerir. Bu raporun dışında 403 Bulunamadı ve 404 Yasak yanıt kodları vardır. Genellikle, bir istek bir istemci hatası sonucu reddedildiğinde bir 4xx yanıt kodu oluşur. |
| 504 Hatalar |504 Ağ Geçidi Zaman Sonu yanıt koduyla sonuçlanan en iyi 10 isteği görüntülemenize olanak tanıyan bir çubuk grafiği içerir. Bir HTTP proxy başka bir sunucu ile iletişim kurmaya çalışırken bir zaman dışarı oluştuğunda bir 504 Ağ Geçidi Zaman Çıkış yanıt kodu oluşur. CDN'miz söz konusu olduğunda, bir kenar sunucusu müşteri kökenli bir sunucuyla iletişim kuramayınca genellikle 504 Ağ Geçidi Zaman Sınırı yanıt kodu oluşur. |
| 502 Hatalar |502 Kötü Ağ Geçidi yanıt koduyla sonuçlanan en iyi 10 isteği görüntülemenize olanak tanıyan bir çubuk grafik içerir. Bir 502 Kötü Ağ Geçidi yanıt kodu, bir SUNUCU ve bir HTTP proxy arasında bir HTTP iletişim kuralı hatası oluştuğunda oluşur. CDN'miz söz konusu olduğunda, genellikle bir müşteri kaynağı sunucusu kenar sunucusuna geçersiz bir yanıt verdiğinde 502 Kötü Ağ Geçidi yanıt kodu oluşur. Bir yanıt ayrıştırılamazsa veya eksikse geçersizdir. |
| 5xx Hatalar |500 aralığında yanıt koduyla sonuçlanan en iyi 10 isteği görüntülemenize olanak tanıyan bir çubuk grafik içerir.  Bu raporun dışında 502 Bad Gateway ve 504 Ağ Geçidi Zaman Adak yanıt kodları vardır. |

## <a name="see-also"></a>Ayrıca bkz.
* [Azure CDN'ye Genel Bakış](cdn-overview.md)
* [Microsoft Azure CDN'de gerçek zamanlı istatistikler](cdn-real-time-stats.md)
* [Kurallar altyapısını kullanarak varsayılan HTTP davranışını geçersiz kılma](cdn-rules-engine.md)
* [Gelişmiş HTTP Raporları](cdn-advanced-http-reports.md)

