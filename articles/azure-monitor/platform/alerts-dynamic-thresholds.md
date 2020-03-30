---
title: Azure Monitör'de Dinamik Eşiklerle Uyarı Oluşturma
description: Makine öğrenimine dayalı Dinamik Eşiklerle Uyarılar Oluşturma
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 02/16/2020
ms.openlocfilehash: 9345138e948d84e0ea3c804dbd7a4b3c21daca2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77668154"
---
# <a name="metric-alerts-with-dynamic-thresholds-in-azure-monitor"></a>Azure Monitörde Dinamik Eşiklere Sahip Metrik Uyarılar

Dinamik Eşikler algılamasına sahip Metrik Uyarı, ölçümlerin geçmiş davranışlarını öğrenmek, olası hizmet sorunlarını gösteren kalıpları ve anormallikleri belirlemek için gelişmiş makine öğreniminden (ML) yararlanır. Kullanıcıların uyarı kurallarını Azure Kaynak Yöneticisi API aracılığıyla tam otomatik bir şekilde yapılandırmalarına izin vererek hem basit bir kullanıcı aracı hem de ölçekteki işlemleri destekler.

Bir uyarı kuralı oluşturulduktan sonra, yalnızca izlenen metrik, özel eşiklerine bağlı olarak beklendiği gibi şekilde durmadığı zaman ateşlenir.

Biz geribildirim duymak isteriz, geliyor <azurealertsfeedback@microsoft.com>tutmak .

## <a name="why-and-when-is-using-dynamic-condition-type-recommended"></a>Neden ve ne zaman kullanılması önerilen dinamik durum türü?

1. **Ölçeklenebilir Uyarı** - Dinamik eşik uyarı kuralları, aynı anda yüzlerce metrik serisi için özel eşikler oluşturabilir, ancak tek bir metrikte uyarı kuralını tanımlama da aynı kolaylığı sağlar. Oluşturmanız ve yönetmeniz için size daha az uyarı verirler. Bunları oluşturmak için Azure portalını veya Azure Kaynak Yöneticisi API'sini kullanabilirsiniz. Ölçeklenebilir yaklaşım, metrik boyutlarla uğraşırken veya tüm abonelik kaynakları gibi birden çok kaynağa uygulanırken özellikle yararlıdır.  [Şablonları kullanarak Dinamik Eşiklerle Metrik Uyarıları nasıl yapılandıracağınız hakkında daha fazla bilgi edinin.](alerts-metric-create-templates.md)

1. **Akıllı Metrik Örüntü Tanıma** - ML teknolojimizi kullanarak, metrik desenleri otomatik olarak algılayabilir ve zaman içinde genellikle mevsimsellik (saatlik / günlük / haftalık) içerebilir metrik değişikliklere uyum sağlayabiliriz. Ölçümlerin zaman daki davranışına uyum sağlamak ve desenindeki sapmalara göre uyarı vermek, her bir metrik için "doğru" eşiğini bilmenin yükünü hafifletir. Dinamik Eşiklerde kullanılan ML algoritması, beklenen deseni olmayan gürültülü (düşük hassasiyetli) veya geniş (düşük geri çağırma) eşikleri önlemek için tasarlanmıştır.

1. **Sezgisel Yapılandırma** – Dinamik Eşikler, üst düzey kavramları kullanarak metrik uyarıların ayarlanmasına olanak vererek, metrik hakkında kapsamlı etki alanı bilgisine sahip olması gereksinimini hafifletiyor.

## <a name="how-to-configure-alerts-rules-with-dynamic-thresholds"></a>Dinamik Eşiklerle uyarı kuralları nasıl yapılandırılatır?

Dinamik Eşiklere sahip uyarılar Azure Monitor'daki Metrik Uyarılar aracılığıyla yapılandırılabilir. [Metrik Uyarıları yapılandırma hakkında daha fazla bilgi edinin.](alerts-metric.md)

## <a name="how-are-the-thresholds-calculated"></a>Eşikler nasıl hesaplanır?

Dinamik Eşikler, metrik serilerin verilerini sürekli olarak öğrenir ve bir dizi algoritma ve yöntem kullanarak modellemeye çalışır. Mevsimsellik (Saatlik / Günlük / Haftalık) gibi verilerdeki desenleri algılar ve gürültülü ölçümleri (makine işlemcisi veya bellek gibi) ve düşük dağılıma sahip ölçümleri (kullanılabilirlik ve hata oranı gibi) işleyebilir.

Eşikler, bu eşiklerden sapmanın metrik davranışta bir anormallik olduğunu belirtebilecek şekilde seçilir.

> [!NOTE]
> Mevsimsel desen algılama bir saat, gün veya hafta aralığıolarak ayarlanır. Bu, bisaatlik desen veya yarı haftalık gibi diğer desenlerin algılanamayabileceği anlamına gelir.

## <a name="what-does-sensitivity-setting-in-dynamic-thresholds-mean"></a>Dinamik Eşiklerde 'Duyarlılık' ayarı ne anlama gelir?

Uyarı eşiği duyarlılığı, bir uyarıyı tetiklemek için gereken metrik davranıştan sapma miktarını kontrol eden üst düzey bir kavramdır.
Bu seçenek, statik eşik gibi metrik hakkında etki alanı bilgisi gerektirmez. Şu seçenekleri kullanabilirsiniz:

- Yüksek – Eşikler sıkı ve metrik seri desenine yakın olacaktır. En küçük sapmada bir uyarı kuralı tetiklenir ve bu da daha fazla uyarıyla sonuçlanır.
- Orta – Daha az sıkı ve daha dengeli eşikler, yüksek hassasiyet (varsayılan) ile daha az uyarı.
- Düşük – Eşikler metrik seri deseninden daha fazla mesafede gevşek olacaktır. Bir uyarı kuralı yalnızca büyük sapmaları tetikler ve bu da daha az uyarıyla sonuçlanır.

## <a name="what-are-the-operator-setting-options-in-dynamic-thresholds"></a>Dinamik Eşiklerde 'Operatör' ayarı seçenekleri nelerdir?

Dinamik Eşikuyarıları kuralı, aynı uyarı kuralını kullanarak hem üst hem de alt sınırlar için metrik davranışa dayalı özel eşikler oluşturabilir.
Aşağıdaki üç koşuldan birinde tetiklenecek uyarıyı seçebilirsiniz:

- Üst eşiğe veya alt eşiğe (varsayılan) daha düşük
- Üst eşiğe göre
- Alt eşiğe göre daha düşük.

## <a name="what-do-the-advanced-settings-in-dynamic-thresholds-mean"></a>Dinamik Eşikler'deki gelişmiş ayarlar ne anlama gelir?

**Başarısız Dönemler** - Dinamik Eşikler, sistemin bir uyarı yıkması için belirli bir zaman penceresi içinde gereken en az sapma sayısı olan "Uyarıyı tetiklemek için sayı ihlalleri" yapılandırmanızı da sağlar (varsayılan zaman penceresi 20 dakika içinde dört sapmadır). Kullanıcı, başarısız dönemleri yapılandırabilir ve başarısız dönemleri ve zaman penceresini değiştirerek ne hakkında uyarı verileceğini seçebilir. Bu yetenek, geçici ani artışlar tarafından oluşturulan uyarı gürültüsünü azaltır. Örnek:

Sorun 20 dakika boyunca sürekli olduğunda, 5 dakikalık belirli bir periyotta art arda 4 kez bir uyarıyı tetiklemek için aşağıdaki ayarları kullanın:

![20 dakika boyunca sürekli sorun için başarısız dönem ayarları, 5 dakikalık belirli bir periyot gruplandırmasında 4 kez ardışık](media/alerts-dynamic-thresholds/0008.png)

5 dakikalık bir süre ile son 30 dakika içinde 20 dakika içinde Dinamik Eşikler bir ihlali olduğunda bir uyarı tetiklemek için aşağıdaki ayarları kullanın:

![Son 30 dakikanın 20 dakikası için 5 dakikalık periyot gruplandırması ile başarısız periyot ayarları](media/alerts-dynamic-thresholds/0009.png)

**Önce verileri yoksay** - Kullanıcılar isteğe bağlı olarak sistemin eşikleri hesaplamaya başlaması gereken bir başlangıç tarihi de tanımlayabilir. Bir kaynak bir sınama modunda çalışırken ve şimdi bir üretim iş yüküne hizmet etmek üzere tanıtıldığında tipik bir kullanım örneği oluşabilir ve bu nedenle sınama aşamasındaki herhangi bir metnin davranışı göz ardı edilmelidir.

## <a name="how-do-you-find-out-why-a-dynamic-thresholds-alert-was-triggered"></a>Dinamik Eşikler uyarısının neden tetiklandığını nasıl öğrenebilirsiniz?

Uyarıların Azure portalındaki görünümünü görmek için e-posta veya kısa mesajdaki bağlantıyı tıklatarak veya tarayıcıyı tıklayarak uyarılar görünümünde tetiklenmiş uyarı örneklerini keşfedebilirsiniz. [Uyarıların görünümü hakkında daha fazla bilgi edinin.](alerts-overview.md#alerts-experience)

Uyarı görünümü görüntüler:

- Dinamik Eşikler uyarısı şu anda tüm metrik detayları ateşledi.
- Uyarının tetiklendiği dönemin grafiği, o anda kullanılan Dinamik Eşikleri içerir.
- Dinamik Eşikler uyarısı ve uyarılar gelecekteki algılamaları geliştirebilecek deneyimi görüntüleme hakkında geri bildirim sağlama becerisi.

## <a name="will-slow-behavior-changes-in-the-metric-trigger-an-alert"></a>Metrikteki yavaş davranış değişiklikleri bir uyarı tetikler mi?

Muhtemelen hayır. Dinamik Eşikler, yavaş yavaş gelişen sorunlar yerine önemli sapmaları algılamak için iyidir.

## <a name="how-much-data-is-used-to-preview-and-then-calculate-thresholds"></a>Eşikleri önizlemek ve hesaplamak için ne kadar veri kullanılır?

Bir uyarı kuralı ilk oluşturulduğunda, grafikte görünen eşikler saat veya günlük mevsimsel desenleri (10 gün) hesaplamak için yeterli geçmiş verilere göre hesaplanır. Bir uyarı kuralı oluşturulduktan sonra, Dinamik Eşikler kullanılabilir olan tüm gerekli geçmiş verilerini kullanır ve eşikleri daha doğru hale getirmek için yeni verilere dayalı olarak sürekli olarak öğrenir ve uyarlanır. Bu, bu hesaplamadan sonra grafiğin haftalık desenleri de göstereceği anlamına gelir.

## <a name="how-much-data-is-needed-to-trigger-an-alert"></a>Bir uyarıyı tetiklemek için ne kadar veri gerekir?

Yeni bir kaynağınız varsa veya metrik verileriniz eksikse, Dinamik Eşikler üç gün veya doğru eşikleri sağlamak için 30 metrik veri örneği kullanılabilir olmadan önce uyarıları tetiklemez.

## <a name="dynamic-thresholds-best-practices"></a>Dinamik Eşikler en iyi uygulamalar

Dinamik Eşikler Azure Monitor'daki herhangi bir platforma veya özel ölçümlere uygulanabilir ve ortak uygulama ve altyapı ölçümleri için de ayarlanmıştır.
Aşağıdaki öğeler, Dinamik Eşikleri kullanarak bu ölçümlerin bazılarında uyarıları yapılandırmakonusunda en iyi uygulamalardır.

### <a name="dynamic-thresholds-on-virtual-machine-cpu-percentage-metrics"></a>Sanal makine CPU yüzdesi ölçümlerinde dinamik eşikler

1. [Azure portalında,](https://portal.azure.com) **Monitör'e**tıklayın. Monitör görünümü, tüm izleme ayarlarınızı ve verilerinizi tek bir görünümde birleştirir.

2. **Uyarılar'ı** tıklatın ve **+Yeni uyarı kuralını**tıklatın.

    > [!TIP]
    > Çoğu kaynak bıçakları da **İzleme**altında kendi kaynak menüsünde **Uyarılar** var , siz de oradan uyarılar oluşturabilirsiniz.

3. Yüklenen bağlam bölmesinde **hedefi seç'i**tıklatın, uyarmak istediğiniz hedef kaynağı seçin. İzlemek istediğiniz kaynağı bulmak için **Abonelik** ve **'Sanal Makineler' Kaynak türü** açılır düşüşlerini kullanın. Kaynağınızı bulmak için arama çubuğunu da kullanabilirsiniz.

4. Hedef bir kaynak seçtikten sonra **Ekle koşulunu**tıklatın.

5. **'CPU Yüzdesi'ni**seçin.

6. İsteğe bağlı olarak, **Dönem** ve **Toplama**ayarlayarak metrik rafine. Davranışı daha az temsilcisi olduğu için bu metrik türü için 'Maksimum' toplama türü kullanılması önerilmez. 'Maksimum' toplama türü statik eşik için belki daha uygun.

7. Son 6 saat için metrik için bir grafik görürsünüz. Uyarı parametrelerini tanımlayın:
    1. **Koşul Türü** - 'Dinamik' seçeneğini seçin.
    1. **Hassasiyet** - Uyarı gürültüsünü azaltmak için Orta/Düşük hassasiyeti seçin.
    1. **İşleç** - Davranış uygulama kullanımını temsil etmediği sürece 'Büyük' seçeneğini belirleyin.
    1. **Sıklık** - Uyarının iş etkisine göre düşürmeyi düşünün.
    1. **Başarısız Dönemler** (Gelişmiş Seçenek) - Arka pencereye bakmak en az 15 dakika olmalıdır. Örneğin, dönem beş dakika olarak ayarlanırsa, başarısız dönemler en az üç veya daha fazla olmalıdır.

8. Metrik grafik, son verilere dayalı olarak hesaplanan eşikleri görüntüler.

9. **Bitti**’ye tıklayın.

10. **Uyarı Kural Adı,** **Açıklama**ve **Önem**Derecesi gibi **Uyarı ayrıntılarını** doldurun.

11. Varolan bir eylem grubu seçerek veya yeni bir eylem grubu oluşturarak bir eylem grubu uyarıya ekleyin.

12. Metrik uyarı kuralını kaydetmek için **Bitti'yi** tıklatın.

> [!NOTE]
> Portal aracılığıyla oluşturulan metrik uyarı kuralları, hedef kaynakla aynı kaynak grubunda oluşturulur.

### <a name="dynamic-thresholds-on-application-insights-http-request-execution-time"></a>Uygulama Öngörüleri'nde Dinamik Eşikler HTTP yürütme süresi isteği

1. [Azure portalında,](https://portal.azure.com) **Monitör'e**tıklayın. Monitör görünümü, tüm izleme ayarlarınızı ve verilerinizi tek bir görünümde birleştirir.

2. **Uyarılar'ı** tıklatın ve **+Yeni uyarı kuralını**tıklatın.

    > [!TIP]
    > Çoğu kaynak bıçakları da **İzleme**altında kendi kaynak menüsünde **Uyarılar** var , siz de oradan uyarılar oluşturabilirsiniz.

3. Yüklenen bağlam bölmesinde **hedefi seç'i**tıklatın, uyarmak istediğiniz hedef kaynağı seçin. İzlemek istediğiniz kaynağı bulmak için **Abonelik** ve **'Uygulama Öngörüleri' Kaynak türü** açılır düşüşlerini kullanın. Kaynağınızı bulmak için arama çubuğunu da kullanabilirsiniz.

4. Hedef bir kaynak seçtikten sonra **Ekle koşulunu**tıklatın.

5. **'HTTP isteği yürütme süresi'** seçin.

6. İsteğe bağlı olarak, **Dönem** ve **Toplama**ayarlayarak metrik rafine. Davranışı daha az temsilcisi olduğu için bu metrik türü için 'Maksimum' toplama türü kullanılması önerilmez. 'Maksimum' toplama türü statik eşik için belki daha uygun.

7. Son 6 saat için metrik için bir grafik görürsünüz. Uyarı parametrelerini tanımlayın:
    1. **Koşul Türü** - 'Dinamik' seçeneğini seçin.
    1. **Operatör** - Süre iyileştirmeye yönelik uyarıları azaltmak için 'Büyükten Büyük' seçeneğini belirleyin.
    1. **Sıklık** - Uyarının iş etkisine göre düşürmeyi düşünün.

8. Metrik grafik, son verilere dayalı olarak hesaplanan eşikleri görüntüler.

9. **Bitti**’ye tıklayın.

10. **Uyarı Kural Adı,** **Açıklama**ve **Önem**Derecesi gibi **Uyarı ayrıntılarını** doldurun.

11. Varolan bir eylem grubu seçerek veya yeni bir eylem grubu oluşturarak bir eylem grubu uyarıya ekleyin.

12. Metrik uyarı kuralını kaydetmek için **Bitti'yi** tıklatın.

> [!NOTE]
> Portal aracılığıyla oluşturulan metrik uyarı kuralları, hedef kaynakla aynı kaynak grubunda oluşturulur.

## <a name="interpreting-dynamic-threshold-charts"></a>Dinamik Eşik grafiklerini yorumlama

Aşağıda bir metrik gösteren bir grafik, dinamik eşik sınırları ve bazı uyarılar değer izin verilen eşiklerin dışında olduğunda ateşlendi.

![Metrik Uyarıları yapılandırma hakkında daha fazla bilgi edinin](media/alerts-dynamic-thresholds/threshold-picture-8bit.png)

Önceki grafiği yorumlamak için aşağıdaki bilgileri kullanın.

- **Mavi çizgi** - Zaman içinde gerçek ölçülen metrik.
- **Mavi gölgeli alan** - Metrik için izin verilen aralığı gösterir. Metrik değerler bu aralıkta kaldığı sürece uyarı oluşmaz.
- **Mavi nokta -** Grafiğin bir kısmına tıklamayı bıraktıysanız ve ardından mavi çizginin üzerinde gezinirseniz, imlecin altında tek bir toplu metrik değeri gösteren mavi bir nokta görürsünüz.
- **Mavi noktalı açılır** pencere - Ölçülen metrik değeri (mavi nokta) ve izin verilen aralığın üst ve alt değerlerini gösterir.  
- **Siyah daireli kırmızı nokta** - İzin verilen aralıktaki ilk metrik değeri gösterir. Bu, metrik uyarıyı ateşleyen ve etkin bir duruma getiren değerdir.
- **Kırmızı noktalar**- İzin verilen aralığın dışında ölçülen ek değerleri belirtin. Ek metrik uyarıları ateşlemezler, ancak uyarı etkin durumda kalır.
- **Kırmızı alan** - Metrik değerin izin verilen aralığın dışında olduğu zamanı gösterir. Sonraki ölçülen değerler izin verilen aralığın dışında olduğu sürece uyarı etkin durumda kalır, ancak yeni uyarılar ateşlendirilmez.
- **Kırmızı alanın sonu** - Mavi çizgi izin verilen değerlerin içine geri döndüğünde, kırmızı alan durur ve ölçülen değer çizgisi maviye döner. Siyah anahatlı kırmızı nokta nın zamanında ateşlenen metrik uyarının durumu çözülecek şekilde ayarlanır. 
