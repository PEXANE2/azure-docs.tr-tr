---
title: Web sitelerinin kullanılabilirlik ve yanıt hızını izleme | Microsoft Docs
description: Application Insights’ta web testleri ayarlayın. Web sitesi kullanılamaz duruma gelirse veya yavaş yanıt verirse uyarı alın.
ms.topic: conceptual
ms.date: 09/16/2019
ms.reviewer: sdash
ms.openlocfilehash: 61358051a8ddc32bc01ec5e231f4c28ebfa18ee0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670041"
---
# <a name="monitor-the-availability-of-any-website"></a>Herhangi bir web sitesinin kullanılabilirliğini izleme

Web uygulamanızı/web sitenizi dağıttıktan sonra, kullanılabilirliği ve yanıt verme durumunu izlemek için yinelenen testler ayarlayabilirsiniz. [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md), dünyanın her yerindeki noktalarından uygulamanıza düzenli aralıklarla web istekleri gönderir. Uygulamanız yanıt vermiyorsa veya çok yavaş yanıt veriyorsa sizi uyarabilir.

Genel İnternet'ten erişilebilen herhangi bir HTTP veya HTTPS uç noktası için kullanılabilirlik testleri ayarlayabilirsiniz. Test ettiğiniz web sitesinde herhangi bir değişiklik yapmak zorunda değilsiniz. Aslında, hatta kendi bir site olmak zorunda değildir. Hizmetinizin bağlı olduğu bir REST API'sinin kullanılabilirliğini test edebilirsiniz.

### <a name="types-of-availability-tests"></a>Kullanılabilirlik testleri türleri:

Üç tür kullanılabilirlik testi vardır:

* [URL ping testi](#create-a-url-ping-test): Azure portalında oluşturabileceğiniz basit bir test.
* [Çok aşamalı web testi](availability-multistep.md): Daha karmaşık senaryoları test etmek için oynatılabilen bir dizi web isteğinin kaydı. Çok aşamalı web testleri Visual Studio Enterprise oluşturulur ve yürütme için portala yüklenir.
* [Özel Parça Kullanılabilirlik Testleri](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability?view=azure-dotnet): Kullanılabilirlik testlerini çalıştırmak `TrackAvailability()` için özel bir uygulama oluşturmaya karar verirseniz, sonuçları Application Insights'a göndermek için yöntem kullanılabilir.

**Application Insights kaynağı başına en fazla 100 kullanılabilirlik testi oluşturabilirsiniz.**

## <a name="create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma

Kullanılabilirlik testi oluşturmak için öncelikle bir Application Insights kaynağı oluşturmanız gerekir. Zaten bir kaynak oluşturduysanız, bir URL [Ping testi oluşturmak](#create-a-url-ping-test)için bir sonraki bölüme gidin.

Azure portalından, **kaynak** > **Geliştirici Araçları** > **Uygulama Öngörüleri** Oluştur'u seçin ve [bir Uygulama Öngörüleri kaynağı oluşturun.](create-new-resource.md)

## <a name="create-a-url-ping-test"></a>URL ping testi oluşturma

Adı "URL ping testi" bir yanlış isim biraz. Açık olmak gerekirse, bu test sitenizin kullanılabilirliğini kontrol etmek için ICMP (Internet Control Message Protocol) herhangi bir kullanım yapmıyor. Bunun yerine, bir bitiş noktasının yanıt verip vermediğini doğrulamak için daha gelişmiş HTTP isteği işlevini kullanır. Ayrıca, bu yanıtla ilişkili performansı ölçer ve bağımlı istekleri ayrıştma ve yeniden denemelere izin verme gibi daha gelişmiş özelliklerle birleştiğinde özel başarı ölçütlerini belirleme olanağı nı ekler.

İlk kullanılabilirlik isteğinizi oluşturmak için Kullanılabilirlik bölmesini açın ve **Test Oluştur'u**seçin.

![En azından web sitenizin URL'sini doldurma](./media/monitor-web-app-availability/availability-create-test-001.png)

### <a name="create-a-test"></a>Test oluşturma

|Ayar| Açıklama
|----|----|----|
|**URL** |  URL, test etmek istediğiniz herhangi bir web sayfası olabilir, ancak ortak İnternet’te görünür olmalıdır. URL bir sorgu dizesi içerebilir. Bu nedenle, örneğin, veritabanınızla biraz alıştırma yapabilirsiniz. URL yeniden yönlendirme adresine çözümlenirse, en fazla 10 yeniden yönlendirmeyi izleriz.|
|**Bağımlı istekleri ayrıştın**| Test, test altındaki web sayfasının parçası olan görüntüleri, komut dosyalarını, stil dosyalarını ve diğer dosyaları ister. Kayıtlı yanıt süresi, bu dosyaları almak için geçen süreyi içerir. Bu kaynaklardan herhangi biri tüm test için zaman açısından başarılı şekilde indirilemiyorsa sınav başarısız olur. Seçenek işaretlenmezse, test yalnızca belirttiğiniz URL’deki dosyayı ister. Bu seçeneği etkinleştirmek daha sıkı bir denetimle sonuçlanır. Test, sitede el ile gezinirken fark edilemeyebilir durumlarda başarısız olabilir.
|**Yeniden denemeleri etkinleştirme**|test başarısız olduğunda, kısa bir aradan sonra yeniden denener. Art arda üç deneme başarısız olursa bir hata bildirilir. Sonraki testler bundan sonra her zamanki test sıklığında gerçekleştirilir. Bir sonraki başarılı olana kadar yeniden deneme geçici olarak askıya alınır. Bu kural her test konuma bağımsız olarak uygulanır. **Bu seçeneği öneririz.** Ortalama olarak hataların yaklaşık %80’i yeniden deneme sırasında kaybolur.|
|**Test frekansı**| Testin her test konumundan ne sıklıkta çalıştırıldığını ayarlar. Beş dakikalık varsayılan sıklıkta ve beş test konumuyla, siteniz ortalama olarak dakikada bir test edilir.|
|**Test konumları**| Sunucularımızın URL'nize web istekleri gönderdiği yerlerdir. Web sitenizdeki sorunları ağ sorunlarından ayırt edebileceğinizden emin olmak için **önerilen minimum test yeri sayısı beştir.** En fazla 16 konum seçebilirsiniz.

**URL'niz genel internetten görünmüyorsa, güvenlik duvarınızı seçerek açerek yalnızca test işlemlerine izin vermeyi seçebilirsiniz.** Kullanılabilirlik test aracılarımız için güvenlik duvarı özel durumları hakkında daha fazla bilgi edinmek için [IP adresi kılavuzuna başvurun.](https://docs.microsoft.com/azure/azure-monitor/app/ip-addresses#availability-tests)

> [!NOTE]
> En az beş konuma sahip birden fazla **konumdan**test yapmanızı şiddetle öneririz. Bu, belirli bir konumla ilgili geçici sorunlardan kaynaklanabilir yanlış alarmları önlemek içindir. Buna ek olarak, en uygun yapılandırmanın **test konumlarının sayısının uyarı konumu eşiğine + 2'ye eşit olması olduğunu**bulduk.

### <a name="success-criteria"></a>Başarı kriterleri

|Ayar| Açıklama
|----|----|----|
| **Test zaman** |Yavaş yanıtlar hakkında uyarılmak için bu değeri azaltın. Yanıtlar sitenizden bu süre içinde alınmadıysa test başarısız sayılır. **Ayrıştırık bağımlı isteklerini**seçtiyseniz, bu dönemde tüm resimler, stil dosyaları, komut dosyaları ve diğer bağımlı kaynaklar alınmış olmalıdır.|
| **HTTP yanıtı** | Başarı olarak sayılan döndürülen durum kodu. 200, normal web sayfası döndürüldüğünü belirten koddur.|
| **İçerik eşleşmesi** | "Hoş geldin!" gibi bir ip. Her yanıtta büyük küçük harfe duyarlı bir tam eşleşme oluştuğunu test edebiliriz. Joker karakter bulunmayan düz bir dize olmalıdır. Sayfanızın içeriği değişirse bunu güncelleştirmeniz gerektiğini unutmayın. **Yalnızca İngilizce karakterler içerik eşleştirmesi ile desteklenir** |

### <a name="alerts"></a>Uyarılar

|Ayar| Açıklama
|----|----|----|
|**Gerçek zamanlı ya da yakın (Önizleme)** | Yakın zamanlı uyarıları kullanmanızı öneririz. Bu tür bir uyarının yapılandırılması, kullanılabilirlik testiniz oluşturulduktan sonra yapılır.  |
|**Klasik** | Artık yeni kullanılabilirlik testleri için klasik uyarıları kullanmamızı önermiyoruz.|
|**Uyarı konum eşiği**|En az 3/5 konum öneririz. Uyarı konum eşiği ile test konumlarının sayısı arasındaki en uygun ilişki, en az beş test konumuna sahip test **konumlarının uyarı konum eşik** = **sayısıdır - 2.**|

## <a name="see-your-availability-test-results"></a>Kullanılabilirlik testi sonuçlarınızı görme

Kullanılabilirlik test sonuçları hem satır hem de dağılım çizimi görünümleriyle görselleştirilebilir.

Birkaç dakika sonra, test sonuçlarınızı görmek için **Yenile'yi** tıklatın.

![Satır görünümü](./media/monitor-web-app-availability/availability-refresh-002.png)

Scatterplot görünümü, tanılama test adımı ayrıntısı bulunan test sonuçlarının örneklerini gösterir. Test altyapısı, hata içeren testler için tanılama ayrıntılarını depolar. Başarılı testlerde, yürütmelerin bir alt kümesi için tanılama ayrıntıları depolanır. Testi, test adını ve konumu görmek için yeşil/kırmızı noktalardan herhangi birinin üzerine titreyin.

![Satır görünümü](./media/monitor-web-app-availability/availability-scatter-plot-003.png)

Belirli bir testi veya konumu seçin ya da ilgilendiğiniz dönemle ilgili daha fazla sonuç görmek için zaman dilimini küçültün. Arama Gezgini’ni kullanarak tüm yürütmelerden alınan sonuçları görün veya Analytics sorgularını kullanarak bu veriler üzerinde özel raporlar çalıştırın.

## <a name="inspect-and-edit-tests"></a> Testleri inceleme ve düzenleme

Bir test adının yanındaki elipsleri düzeltmek, geçici olarak devre dışı etmek veya silmek için. Yapılandırma değişikliklerinin bir değişiklik yapıldıktan sonra tüm test temsilcilerine yayılması 20 dakika kadar sürebilir.

![Test ayrıntılarını görüntüleyin. Web testini edin ve devre dışı](./media/monitor-web-app-availability/edit.png)

Hizmetinizde bakım gerçekleştirdiğiniz sırada kullanılabilirlik testlerini veya bunlarla ilişkili uyarıları devre dışı bırakmak isteyebilirsiniz.

## <a name="if-you-see-failures"></a>Hata görürseniz

Kırmızı noktaya tıklayın.

![Kırmızı noktaya tıklama](./media/monitor-web-app-availability/open-instance-3.png)

Kullanılabilirlik testi sonucu, tüm bileşenler arasında hareket ayrıntılarını görebilirsiniz. Burada şunları yapabilirsiniz:

* Sunucunuzdan alınan yanıtı denetleme.
* Başarısız kullanılabilirlik testini işlerken toplanan ilişkili sunucu tarafı telemetrisi ile başarısızlığı tanılayın.
* Sorunu izlemek için bir sorunu veya iş öğesini Git veya Azure Panoları'nda günlüğe kaydedin. Hata, bu olayın bir bağlantısını içerir.
* Web testi sonucunu Visual Studio’da açın.

[Burada](../../azure-monitor/app/transaction-diagnostics.md)son işlem tanılama deneyimi sonuna kadar hakkında daha fazla bilgi edinin.

Sentetik kullanılabilirlik testinin başarısız olmasıyla ilgili sunucu tarafındaki özel durum ayrıntılarını görmek için özel durum satırını tıklatın. Ayrıca, daha zengin kod düzeyi tanılama için [hata ayıklama anlık görüntüsünü](../../azure-monitor/app/snapshot-debugger.md) de alabilirsiniz.

![Sunucu tarafı tanılama](./media/monitor-web-app-availability/open-instance-4.png)

Ham sonuçlara ek olarak, [Metrics Explorer'da](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started)iki önemli Kullanılabilirlik ölçümlerini de görüntüleyebilirsiniz:

1. Kullanılabilirlik: Tüm test yürütmelerinde başarılı olan testlerin yüzdelik oranı.
2. Test Süresi: Tüm test yürütmelerinde ortalama test süresi.

## <a name="automation"></a>Automation

* Otomatik olarak [kullanılabilirlik testi ayarlamak için PowerShell betiklerini kullanın](../../azure-monitor/app/powershell.md#add-an-availability-test).
* Bir uyarı ortaya çıktığında çağrılan bir [web kancası](../../azure-monitor/platform/alerts-webhooks.md) ayarlayın.

## <a name="troubleshooting"></a>Sorun giderme

Özel [sorun giderme makalesi](troubleshoot-availability.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanılabilirlik Uyarıları](availability-alerts.md)
* [Çok adımlı web testleri](availability-multistep.md)


