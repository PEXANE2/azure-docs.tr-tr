---
title: Azure İnternet Çözümleyicisi | Microsoft Dokümanlar
description: Azure Internet Çözümleyicisi hakkında bilgi edinin
services: internet-analyzer
author: megan-beatty
ms.service: internet-analyzer
ms.topic: overview
ms.date: 10/16/2019
ms.author: mebeatty
ms.openlocfilehash: 3d9d1240e31d40a3d9c75d56314acc1721d90b99
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73501949"
---
# <a name="what-is-internet-analyzer-preview"></a>İnternet Çözümleyicisi Nedir? (Önizleme)

Internet Analyzer, ağ altyapısı ndaki değişikliklerin müşterilerinizin performansını nasıl etkilediğini test eden istemci tarafı tarafından bir ölçüm platformudur. Şirket içinden Azure'a geçerken veya yeni bir Azure hizmetini değerlendirirken, Internet Analyzer kullanıcılarınızın verilerinden ve Microsoft'un zengin analiz özelliklerinden edindiğiniz bilgilerle geçiş öncesinde Azure'ın yardımıyla ağ mimarinizi daha iyi anlamanızı ve iyileştirmenizi sağlar.

Internet Analyzer, son kullanıcılarınızdan seçtiğiniz ağ hedef kümesine gecikme süresini ölçmek için Web uygulamanıza gömülü küçük bir JavaScript _istemcisi_kullanır, biz uç noktaları diyoruz. Internet Analyzer, altyapınız ve müşteri gereksinimleriniz geliştikçe çeşitli senaryoları değerlendirmenize olanak tanıyan birden çok yan yana test ayarlamanızı sağlar. Internet Analyzer, son kullanıcılarınız için güvenilir performans kararları almanız için hem kolaylık hem de esneklik sağlayarak özel ve önceden yapılandırılmış uç noktalar sağlar. 


> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="quick--customizable-tests"></a>Hızlı & özelleştirilebilir testler

Internet Analyzer, bulut geçişi, yeni veya ek Azure bölgelerine dağıtım veya [Azure Ön Kapısı](https://azure.microsoft.com/services/frontdoor/) ve Microsoft Azure [CDN](https://azure.microsoft.com/services/cdn/)gibi Azure'da yeni uygulama ve içerik teslim platformlarını test etme için performansla ilgili soruları ele alır. 

Internet Analyzer'da oluşturduğunuz her test iki uç noktadan oluşur —Bitiş Noktası A ve Bitiş Noktası B. Bitiş Noktası B'nin performansı Bitiş Noktası A'ya göre analiz edilir. 

Kendi özel bitiş noktanızı yapılandırabilir veya önceden yapılandırılmış çeşitli Azure uç noktalarından seçim yapabilirsiniz. Şirket içi iş yüklerini, diğer bulut sağlayıcılarındaki örneklerinizi veya özel Azure yapılandırmalarınızı değerlendirmek için özel uç noktalar kullanılmalıdır. Testler iki özel uç noktadan oluşabilir; ancak Azure'da en az bir özel bitiş noktası barındırılmalıdır. Önceden yapılandırılmış Azure uç noktaları, Azure Ön Kapı, Azure Trafik Yöneticisi ve Azure CDN gibi popüler Azure ağ platformlarının performansını değerlendirmenin hızlı ve kolay bir yoludur. 

Önizleme sırasında, önceden yapılandırılmış uç noktalar kullanılabilir: 

* **Azure bölgeleri**
    * Güney Brezilya
    * Orta Hindistan
    * Orta ABD
    * Doğu Asya
    * Doğu ABD
    * Batı Japonya
    * Kuzey Avrupa
    * Güney Afrika Kuzey
    * Güneydoğu Asya 
    * BAE Kuzey
    * Batı Birleşik Krallık  
    * Batı Avrupa
    * Batı ABD 
    * Batı ABD 2
* **Birden çok Azure bölge birleşimi** 
    * Doğu ABD, Brezilya Güney 
    * Doğu ABD, Doğu Asya 
    * Batı Avrupa, Brezilya Güney
    * Batı Avrupa, Güneydoğu Asya
    * Batı Avrupa, BAE Kuzey
    * Batı ABD, Doğu ABD 
    * Batı ABD, Batı Avrupa
    * Batı ABD, BAE Kuzey
    * Batı Avrupa, BAE Kuzey, Güneydoğu Asya
    * Batı ABD, Batı Avrupa, Doğu Asya
    * Batı ABD, Kuzey Avrupa, Güneydoğu Asya, BAE Kuzey, Güney Afrika Kuzey 
* **Azure + Azure Ön Kapı** - yukarıda listelenen tek veya birden çok Azure bölge birleşiminde dağıtıldı
* **Microsoft'tan Azure + Azure CDN** - yukarıda listelenen tek bir Azure bölge birleşiminde dağıtıldı
* **Azure + Azure Trafik Yöneticisi** - yukarıda listelenen herhangi bir birden çok Azure bölge birleşiminde dağıtıldı

## <a name="suggested-test-scenarios"></a>Önerilen test senaryoları 

Müşterileriniz için en iyi performans kararlarını almanıza yardımcı olmak için, Internet Analyzer belirli son kullanıcı popülasyonunuz için iki uç noktayı değerlendirmenize olanak tanır. 

Internet Analyzer çok sayıda soruyu yanıtlayabilirken, en yaygın sorulardan bazıları şunlardır: 
* Buluta geçişin performans etkisi nedir? 
    * *Önerilen Test: Özel (mevcut şirket içi altyapınız) ile Azure (önceden yapılandırılmış bitiş noktası)*
* Verilerimi bir veri merkezine koymanın değeri nedir? 
    *  *Önerilen Test: Azure ve Azure Ön Kapı, Azure ve Azure CDN Microsoft'tan*
* Azure Ön Kapı'nın performans avantajı nedir?
    *  *Önerilen Test: Özel/ Azure/ CDN ve Azure Ön Kapı*
* Azure CDN'nin Microsoft'un performans avantajı nedir? 
    *  *Önerilen Test: Microsoft'tan Özel/ Azure/ AFD ve Azure CDN*
* Microsoft'un Azure CDN'si nasıl birikir? 
    *  *Önerilen Test: Microsoft'tan Özel (diğer CDN bitiş noktası) ve Azure CDN*
* Her bölgedeki son kullanıcı popülasyonunuz için en iyi bulut nedir? 
    *  *Önerilen Test: Özel (diğer bulut hizmeti) ile Azure (önceden yapılandırılmış bitiş noktası)*

## <a name="how-it-works"></a>Nasıl çalışır?

Internet Analyzer'ı kullanmak için Microsoft Azure portalında bir Internet Analyzer kaynağı ayarlayın ve küçük JavaScript istemcisini uygulamanızda yükleyin. İstemci, https üzerinden tek piksellik bir görüntü indirerek son kullanıcılarınızdan seçtiğiniz uç noktalara kadar olan gecikme süresini ölçer. Gecikme ölçer ölçümleri topladıktan sonra, istemci ölçüm verilerini Internet Analyzer'a gönderir.

Bir kullanıcı Web uygulamasını ziyaret ettiğinde, JavaScript istemcisi tüm yapılandırılmış testlerarasında ölçmek için iki uç nokta seçer. Her uç nokta için, istemci _soğuk_ ve _sıcak_ bir ölçüm gerçekleştirir. _Soğuk_ ölçüm, dns çözünürlüğü, TCP bağlantı el sıkışması ve SSL/TLS anlaşması gibi kullanıcı ile bitiş noktası arasındaki saf ağ gecikmesinin yanında ek gecikmeye neden olur. _Sıcak_ ölçüm, _soğuk_ ölçüm tamamlandıktan hemen sonra gerçekleşir ve uçtan uca gecikmenin doğru bir ölçüsünü almak için modern tarayıcıların kalıcı TCP bağlantı yönetiminden yararlanır. Kullanıcının tarayıcısı tarafından desteklendiğinde, W3C kaynak zamanlama API doğru ölçüm zamanlaması için kullanılır. Şu anda analiz için yalnızca sıcak gecikme ölçümleri kullanılmaktadır.

![architecture](./media/ia-overview/architecture.png)


## <a name="scorecards"></a>Karne 

Bir test başladıktan sonra, Telemetri verileri Karne sekmesinin altındaki Internet Analyzer kaynağınızda görünür. Bu veriler her zaman toplanır. Gördüğünüz verilerin görünümünü değiştirmek için aşağıdaki filtreleri kullanın: 

* **Test:** Sonuçları görüntülemek istediğiniz testi seçin. Test verileri, çoğu durumda 24 saat içinde, analizi tamamlamak için yeterli veri olduğunda görünür. 
* **Bitiş tarihinden & süre:** Internet Analyzer günde üç karne oluşturur – her karne farklı bir toplama süresini yansıtır – 24 saat önce (gün), yedi gün önce (hafta) ve 30 gün önceki (ay). Görmek istediğiniz zaman dilimini seçmek için "Bitiş Tarihi" filtresini kullanın. 
* **Ülke:** Bir ülkede ikamet eden son kullanıcılara özgü verileri görüntülemek için bu filtreyi kullanın. Genel filtre tüm coğrafyalarda verileri gösterir.  

Karneler hakkında daha fazla bilgiyi [karnenizi Yorumlama](internet-analyzer-scorecard.md) sayfasında bulabilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

* [İlk Internet Analyzer kaynağınızı](internet-analyzer-create-test-portal.md)nasıl oluşturabilirsiniz öğrenin.
* Internet [Analyzer SSS](internet-analyzer-faq.md)okuyun. 
