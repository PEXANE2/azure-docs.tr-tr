---
title: Azure Depolama'yı izleyin, tanıkoyun ve sorun giderin | Microsoft Dokümanlar
description: Azure Depolama ile ilgili sorunları tanımlamak, tanılamak ve gidermek için depolama analitiği, istemci tarafı günlüğü ve diğer üçüncü taraf araçları gibi özellikleri kullanın.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 3d5f3ade3ef3b79ddb3996b5bf2d609b11aff8a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255970"
---
# <a name="monitor-diagnose-and-troubleshoot-microsoft-azure-storage"></a>Microsoft Azure Storage izleme, tanılama ve sorun giderme
[!INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## <a name="overview"></a>Genel Bakış
Bulut ortamında barındırılan dağıtılmış bir uygulamada tanılama ve sorun giderme sorunları geleneksel ortamlardan daha karmaşık olabilir. Uygulamalar PaaS veya IaaS altyapısında, şirket içinde, mobil cihazda veya bu ortamların bir birleşiminde dağıtılabilir. Tipik olarak, uygulamanızın ağ trafiği genel ve özel ağlarda geçiş yapabilir ve uygulamanız ilişkisel gibi diğer veri depolarına ek olarak Microsoft Azure Depolama Tabloları, Blobs, Kuyruklar veya Dosyalar gibi birden çok depolama teknolojisi kullanabilir ve belge veritabanları.

Bu tür uygulamaları başarılı bir şekilde yönetmek için bunları proaktif olarak izlemeli ve bunların tüm yönlerini ve bağımlı teknolojilerini nasıl tanılayacağınızı ve sorun gidereceklerini anlamalısınız. Azure Depolama hizmetlerinin bir kullanıcısı olarak, uygulamanızın beklenmeyen davranışlarda (normalden daha yavaş yanıt süreleri gibi) için kullandığı Depolama hizmetlerini sürekli olarak izlemeli ve daha ayrıntılı veri toplamak ve bir sorunu çözümlemek için günlüğe kaydetmeyi kullanmalısınız. Derinlik. Hem izleme hem de günlüğe kaydetmeden elde ettiğiniz tanılama bilgileri, uygulamanızın karşılaştığı sorunun temel nedenini belirlemenize yardımcı olur. Ardından sorunu giderebilir ve düzeltimi için atabileceğiniz uygun adımları belirleyebilirsiniz. Azure Depolama temel bir Azure hizmetidir ve müşterilerin Azure altyapısına dağıtan çözümlerin çoğunun önemli bir parçasını oluşturur. Azure Depolama, bulut tabanlı uygulamalarınızda izleme, tanılama ve sorun giderme depolama sorunlarını basitleştiren özellikler içerir.

> [!NOTE]
> Azure Files şu anda günlüğe kaydetmeyi desteklemez.
>

Azure Depolama uygulamalarında uçtan uca sorun giderme için uygulamalı bir kılavuz için, [Azure Depolama Ölçümleri ve Günlük, AzCopy ve İleti Çözümleyicisini kullanarak Uçtan Uca Sorun Giderme'ye](../storage-e2e-troubleshooting.md)bakın.

* [Giriş]
  * [Bu kılavuz nasıl düzenlenir?]
* [Depolama hizmetinizi izleme]
  * [Hizmet durumunun izlenmesi]
  * [İzleme kapasitesi]
  * [Kullanılabilirliği izleme]
  * [Performansı izleme]
* [Depolama sorunlarını tanılama]
  * [Hizmet sağlığı sorunları]
  * [Performans sorunları]
  * [Hataları tanılama]
  * [Depolama emülatör sorunları]
  * [Depolama günlüğü araçları]
  * [Ağ günlüğü araçlarını kullanma]
* [Uçuça izleme]
  * [Günlük verilerini ilişkilendirme]
  * [İstemci istek kimliği]
  * [Sunucu istek Kimliği]
  * [Zaman damgaları]
* [Sorun giderme kılavuzu]
  * [Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor]
  * [Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor, ancak istemci yüksek gecikme durumu yaşıyor]
  * [Ölçümler yüksek AverageServerLatency gösteriyor]
  * [Kuyrukta ileti tesliminde beklenmedik gecikmeler yaşıyorsunuz]
  * [Ölçümler PercentThrottlingError’da artış gösteriyor]
  * [Ölçümler PercentTimeoutError’da artış gösteriyor]
  * [Ölçümler PercentNetworkError’da artış gösteriyor]
  * [İstemci HTTP 403 (Yasak) iletileri alıyor]
  * [İstemci HTTP 404 (Bulunamadı) iletileri alıyor]
  * [İstemci HTTP 409 (Çakışma) iletileri alıyor]
  * [Ölçümler düşük YüzdeBaşarı veya analitik günlük girişleri ClientOtherErrors işlem durumu ile işlemleri var]
  * [Kapasite ölçümleri depolama kapasitesi kullanımında beklenmeyen bir artış gösterir]
  * [Sorununuzun geliştirme veya test için depolama emülatörü kullanarak ortaya çıkar]
  * [.NET için Azure SDK'yı yüklerken sorunlarla karşılaşabilirsiniz]
  * [Depolama hizmetiyle ilgili farklı bir sorununuzun var]
  * [Windows sanal makinelerde VHD'ler sorun giderme](../../virtual-machines/windows/troubleshoot-vhds.md)   
  * [Linux sanal makinelerde VHD'ler sorun giderme](../../virtual-machines/linux/troubleshoot-vhds.md)
  * [Windows ile Azure Dosyaları sorunlarını giderme](../files/storage-troubleshoot-windows-file-connection-problems.md)   
  * [Linux ile Azure Dosyaları sorunlarını giderme](../files/storage-troubleshoot-linux-file-connection-problems.md)
* [Ekler]
  * [Ek 1: HTTP ve HTTPS trafiğini yakalamak için Fiddler'ı kullanma]
  * [Ek 2: Ağ trafiğini yakalamak için Wireshark kullanma]
  * [Ek 3: Ağ trafiğini yakalamak için Microsoft İleti Çözümleyicisini kullanma]
  * [Ek 4: Ölçümleri ve günlük verilerini görüntülemek için Excel'i kullanma]
  * [Ek 5: Azure DevOps için Uygulama Öngörüleri ile İzleme]

## <a name="introduction"></a><a name="introduction"></a>Giriş
Bu kılavuz, Azure Depolama İstemci Kitaplığı'nda müşteri tarafından günlüğe kaydetme ve Azure Depolama ile ilgili sorunları tanımlamak, tanılamak ve sorun gidermek için diğer üçüncü taraf araçlarını nasıl kullanacağınızı gösterir.

![][1]

Bu kılavuz, öncelikle Azure Depolama Hizmetleri ve BT Uzmanları'nı kullanan çevrimiçi hizmetlerin geliştiricileri tarafından okunması amaçlanmıştır. Bu kılavuzun hedefleri şunlardır:

* Azure Depolama hesaplarınızın sistem durumunu ve performansını korumanıza yardımcı olmak için.
* Bir uygulamadaki bir sorunun veya sorunun Azure Depolama ile ilgili olup olmadığına karar vermenize yardımcı olacak gerekli işlemleri ve araçları sağlamak için.
* Azure Depolama ile ilgili sorunları çözmek için size uygulanabilir yönergeler sağlamak için.

### <a name="how-this-guide-is-organized"></a><a name="how-this-guide-is-organized"></a>Bu kılavuz nasıl düzenlenir?
"Depolama[hizmetinizi izlemek"]bölümü, Azure Depolama Analizi Ölçümleri (Depolama Ölçümleri) kullanarak Azure Depolama hizmetlerinizin durumunu ve performansını nasıl izleyeceğinizi açıklar.

"Depolama[sorunları tanılama"]bölümünde, Azure Depolama Analizi Günlüğe Kaydetme (Depolama Günlüğü) kullanarak sorunların nasıl tanılanır. Ayrıca, .NET için Depolama İstemci Kitaplığı veya Java için Azure SDK gibi istemci kitaplıklarından birinde bulunan tesisleri kullanarak istemci tarafı oturum açmayı nasıl etkinleştirin.

["Uçlardan uca izleme"]bölümünde, çeşitli günlük dosyaları ve ölçümler verilerinde bulunan bilgileri nasıl ilişkilendirebileceğiniz açıklanmaktadır.

"[Sorun Giderme kılavuzu]" bölümü, karşılaşabileceğiniz depolamayla ilgili bazı yaygın sorunlar için sorun giderme kılavuzu sağlar.

"[Ekler]", ağ paketi verilerini analiz etmek için Wireshark ve Netmon, HTTP/HTTPS iletilerini analiz etmek için Fiddler ve günlük verilerini ilişkilendiren Microsoft İleti Çözümleyicisi gibi diğer araçları kullanma hakkında bilgi içerir.

## <a name="monitoring-your-storage-service"></a><a name="monitoring-your-storage-service"></a>Depolama hizmetinizi izleme
Windows performans izleme yi biliyorsanız, Depolama Ölçümleri'nin Windows Performance Monitor sayaçlarının Azure Depolama eşdeğeri olduğunu düşünebilirsiniz. Depolama Ölçümleri'nde, hizmet kullanılabilirliği, toplam hizmet isteği sayısı veya hizmete yapılan başarılı isteklerin yüzdesi gibi kapsamlı bir ölçüm kümesi (Windows Performans İzleyicisi terminolojisinde sayaçlar) bulacaksınız. Kullanılabilir ölçümlerin tam listesi için Bkz. [Depolama Analizi Ölçümleri Tablo Şeması.](https://msdn.microsoft.com/library/azure/hh343264.aspx) Depolama hizmetinin her saat veya her dakika ölçümleri toplayıp toplamasını isteyip istemediğinizbelirtebilirsiniz. Ölçümleri etkinleştirme ve depolama hesaplarınızı izleme hakkında daha fazla bilgi için [bkz.](https://go.microsoft.com/fwlink/?LinkId=510865)

[Azure portalında](https://portal.azure.com) hangi saatlik ölçümleri görüntülemek istediğinizi seçebilir ve saatlik bir ölçüm belirli bir eşiği aştığında yöneticileri e-posta yla bildiren kuralları yapılandırabilirsiniz. Daha fazla bilgi için [Uyarı Bildirimleri Al'a](/azure/monitoring-and-diagnostics/monitoring-overview-alerts)bakın.

Depolama için [Azure Monitörü'ne](../../azure-monitor/insights/storage-insights-overview.md) (önizleme) incelemenizi öneririz. Azure Depolama hizmetleri performansınızın, kapasitenizin ve kullanılabilirliğinizin birleşik bir görünümünü sunarak Azure Depolama hesaplarınızın kapsamlı bir şekilde izlenmesini sunan Azure Monitor özelliğidir. Herhangi bir şeyi etkinleştirmenizi veya yapılandırmanızı gerektirmez ve bu ölçümleri önceden tanımlanmış etkileşimli grafiklerden ve dahil olan diğer görselleştirmelerden hemen görüntüleyebilirsiniz.

Depolama hizmeti, en iyi çabayı kullanarak ölçümler toplar, ancak her depolama işlemini kayda almayabilir.

Azure portalında, bir depolama hesabı için kullanılabilirlik, toplam istekler ve ortalama gecikme sayıları gibi ölçümleri görüntüleyebilirsiniz. Kullanılabilirlik belirli bir düzeyin altına düşerse, yöneticiyi uyarmak için bir bildirim kuralı da ayarlanmışolur. Bu verileri görüntülemekten, olası bir araştırma alanı tablo hizmeti başarı yüzdesinin %100'ün altında olmasıdır (daha fazla bilgi için "[Ölçümler düşük YüzdeBaşarı gösterir veya analitik günlük girişlerinde ClientOtherErrors'ın işlem durumu olan işlemler var"]bölümüne bakın).

Aşağıdakiler tarafından beklendiği gibi sağlıklı ve performans gösteren sağlıklı olduğundan emin olmak için Azure uygulamalarınızı sürekli olarak izlemeniz gerekir:

* Geçerli verileri karşılaştırmanızı ve Azure depolama ve uygulamanızın davranışındaki önemli değişiklikleri belirlemenizi sağlayacak uygulama için bazı temel ölçümler oluşturma. Temel ölçümlerinizin değerleri, çoğu durumda uygulamaya özgü olacaktır ve uygulamanızı performans testi yaparken bunları oluşturmanız gerekir.
* Dakika ölçümlerini kaydetmek ve hata sayılarında veya istek oranlarındaki ani artışlar gibi beklenmeyen hatalar ve anormallikler için etkin olarak izlemek için bunları kullanmak.
* Saatlik ölçümleri kaydetme ve ortalama hata sayıları ve istek oranları gibi ortalama değerleri izlemek için bunları kullanma.
* Daha sonra "[Depolama sorunlarının tanılanması"]bölümünde ele alındığı gibi tanılama araçlarını kullanarak olası sorunları araştırma.

Aşağıdaki resimdeki grafikler, saatlik ölçümler için gerçekleşen ortalamanın etkinlikteki ani artışları nasıl gizleyebildiğini göstermektedir. Saatlik ölçümler sabit bir istek oranını gösterirken, dakika ölçümleri gerçekte gerçekleşen dalgalanmaları gösterir.

![][3]

Bu bölümün geri kalanı, hangi ölçümleri izlemeniz gerektiğini ve neden izlemeniz gerektiğini açıklar.

### <a name="monitoring-service-health"></a><a name="monitoring-service-health"></a>Hizmet durumunun izlenmesi
Azure [portalını,](https://portal.azure.com) depolama hizmetinin (ve diğer Azure hizmetlerinin) tüm dünyadaki tüm Azure bölgelerindeki sistem durumunu görüntülemek için kullanabilirsiniz. İzleme, denetiminiz dışında bir sorunun uygulamanız için kullandığınız bölgedeki Depolama hizmetini etkileyip etkilemeyeyolmadığını hemen görmenizi sağlar.

[Azure portalı,](https://portal.azure.com) çeşitli Azure hizmetlerini etkileyen olaylarla ilgili bildirimler de sağlayabilir.
Not: Bu bilgiler, Azure [Hizmet Panosu'nda](https://status.azure.com)geçmiş verilerle birlikte daha önce mevcuttu.

Azure [portalı,](https://portal.azure.com) Azure veri merkezlerinin içinden sistem durumu bilgileri toplarken (iç-dış izleme), Azure tarafından barındırılan web uygulamanıza birden çok konumdan düzenli olarak erişen sentetik işlemler oluşturmak için dışarıdan bir yaklaşım benimsemeyi de düşünebilirsiniz. [Dynatrace](https://www.dynatrace.com/en/synthetic-monitoring) ve Azure DevOps'ler için Uygulama Öngörüleri tarafından sunulan hizmetler bu yaklaşımın örnekleridir. Azure DevOps'ler için Uygulama Öngörüleri hakkında daha fazla bilgi için "[Ek 5: Azure DevOps'ler için Uygulama Öngörüleri ile İzleme](#appendix-5)" ekine bakın.

### <a name="monitoring-capacity"></a><a name="monitoring-capacity"></a>İzleme kapasitesi
Depolama Ölçümleri yalnızca blob hizmeti için kapasite ölçümlerini depolar, çünkü blob'lar genellikle depolanan verilerin en büyük oranını hesaba katar (yazma sırasında, tablolarınızın ve kuyruklarınızın kapasitesini izlemek için Depolama Ölçümleri kullanmak mümkün değildir). Blob hizmeti için izlemeyi etkinleştirdiyseniz, bu verileri **$MetricsCapacityBlob** tablosunda bulabilirsiniz. Depolama Ölçümleri bu verileri günde bir kez kaydeder ve satırın kullanıcı verileriyle (değer **verileri)** veya analitik verilerle (değer **analitiği)** ilgili bir varlık bulunup bulunmadığını belirlemek için **RowKey** değerini kullanabilirsiniz. Depolanan her varlık, kullanılan depolama miktarı **(Bayt** cinsinden ölçülen kapasite) ve depolama hesabında kullanılan geçerli kapsayıcı sayısı **(ContainerCount)** ve blobs **(ObjectCount)** hakkında bilgi içerir. **$MetricsCapacityBlob** tablosunda depolanan kapasite ölçümleri hakkında daha fazla bilgi için Depolama [Analizi Ölçümleri Tablosu Şeması'na](https://msdn.microsoft.com/library/azure/hh343264.aspx)bakın.

> [!NOTE]
> Depolama hesabınızın kapasite sınırlarına yaklaştığınıza dair erken bir uyarı için bu değerleri izlemeniz gerekir. Azure portalında, toplu depolama alanı kullanımı belirttiğiniz eşikleri aşarsa veya altına düşerse sizi bilgilendirmek için uyarı kuralları ekleyebilirsiniz.
>
>

Blobs gibi çeşitli depolama nesnelerinin boyutunu tahmin etmede yardımcı olmak için, [Azure Depolama Faturalandırmasını Anlama – Bant Genişliği, İşlemler ve Kapasiteyi Anlama](https://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx)adlı blog gönderisini görün.

### <a name="monitoring-availability"></a><a name="monitoring-availability"></a>Kullanılabilirliği izleme
$MetricsHourPrimaryTransactionsBlob **$MetricsMinutePrimaryTransactionsBlob**, **$MetricsHourPrimaryTransactionsTable**, $MetricsHourPrimaryTransactionsQueue $MetricsMinutePrimaryTransactionsBlob , $MetricsMinutePrimaryTransactionsTable , **$MetricsMinutePrimaryTransactionsQueue**, $ - $MetricsHourPrimaryTransactionsBlob , **$MetricsHourPrimaryTransactionsTable,** **$MetricsMinutePrimaryTransactionsBlob,** **$MetricsMinutePrimaryTransactionsTable**, $ **Kullanılabilirlik** sütunundaki değeri izleyerek depolama hesabınızdaki depolama hizmetlerinin kullanılabilirliğini izlemelisiniz ** MetricsCapacityBlob**. **Kullanılabilirlik** sütunu, hizmetin kullanılabilirliğini veya satır tarafından temsil edilen API işlemini gösteren bir yüzde değeri içerir **(Satır,** hizmetin bir bütün olarak veya belirli bir API işlemi için ölçümler içeriyorsa RowKey gösterir).

%100'den küçük herhangi bir değer, bazı depolama isteklerinin başarısız olduğunu gösterir. **ServerTimeoutError**gibi farklı hata türlerine sahip istek lerin sayısını gösteren metrik verilerdeki diğer sütunları inceleyerek neden başarısız olduklarını görebilirsiniz. Hizmet bölümleri daha iyi yük dengesi isteğine taşırken geçici sunucu zaman zaman ları gibi nedenlerle **Kullanılabilirlik'in** geçici olarak %100'ün altına düştüğünü görmeyi beklemelisiniz; istemci uygulamanızdaki yeniden deneme mantığı bu tür aralıklı koşulları ele almalıdır. Makale [Depolama Analitik Oturum Açmış İşlemler ve Durum İletileri,](https://msdn.microsoft.com/library/azure/hh343260.aspx) Depolama Ölçümleri'nin **Kullanılabilirlik** hesaplamasında yer aldığı işlem türlerini listeler.

Azure [portalında,](https://portal.azure.com)bir hizmetin **kullanılabilirliği** belirttiğiniz eşiğin altına düşerse sizi bilgilendirmek için uyarı kuralları ekleyebilirsiniz.

Bu kılavuzun "[Sorun Giderme kılavuzu]" bölümünde, kullanılabilirlikle ilgili bazı yaygın depolama hizmeti sorunları açıklanmaktadır.

### <a name="monitoring-performance"></a><a name="monitoring-performance"></a>Performansı izleme
Depolama hizmetlerinin performansını izlemek için saatlik ve dakika ölçümleri tablolarından aşağıdaki ölçümleri kullanabilirsiniz.

* **AverageE2ELatency** ve **AverageServerLatency** sütunlarında yer alan değerler, depolama hizmetinin veya API işlem türünün istekleri işlemek için aldığı ortalama süreyi gösterir. **AverageE2ELatency,** isteği okumak ve isteği işlemek için geçen süreye ek olarak yanıtı göndermek için geçen süreyi içeren uçtan uca gecikme süresinin ölçüsüdür (bu nedenle istek depolama hizmetine ulaştığında ağ gecikmesini içerir); **AverageServerLatency** yalnızca işlem süresinin bir ölçüsüdür ve bu nedenle istemciyle iletişim kurmayla ilgili herhangi bir ağ gecikmesini hariç tutar. Bu iki değer arasında neden önemli bir fark olabileceği ne kadar tartışılması için bu kılavuzda "[Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency "]bölümünü izleyin.
* **TotalIngress** ve **TotalEgress** sütunlarında yer alan değerler, depolama hizmetinize gelen ve çıkan veya belirli bir API işlem türü nden gelen ve giden toplam veri miktarını gösterir.
* **TotalRequests** sütunundaki değerler, API işleminin depolama hizmetinin aldığı toplam istek sayısını gösterir. **TotalRequests,** depolama hizmetinin aldığı toplam istek sayısıdır.

Genellikle, araştırma gerektiren bir sorununuzun olduğunun bir göstergesi olarak bu değerlerden herhangi birinde beklenmeyen değişiklikleri izlersiniz.

Azure [portalında,](https://portal.azure.com)bu hizmetin performans ölçümlerinden herhangi birinin belirttiğiniz bir eşiğin altına düşmesi veya aşması durumunda sizi bilgilendirmek için uyarı kuralları ekleyebilirsiniz.

Bu kılavuzun "[Sorun Giderme kılavuzu]" bölümünde performansla ilgili bazı yaygın depolama hizmeti sorunları açıklanmaktadır.

## <a name="diagnosing-storage-issues"></a><a name="diagnosing-storage-issues"></a>Depolama sorunlarını tanılama
Uygulamanızdaki bir sorun veya sorundan haberdar olmanın birkaç yolu vardır:

* Uygulamanın çökmesine veya çalışmayı durdurmasına neden olan büyük bir hata.
* Önceki bölümde açıklandığı gibi, izlediğiniz ölçümlerdeki temel değerlerdeki önemli değişiklikler "[Depolama hizmetinizi izleme."]
* Uygulamanızın kullanıcılarından, belirli bir işlemin beklendiği gibi tamamlanmadığını veya bazı özelliğin çalışmadığını bildirir.
* Günlük dosyalarında veya başka bir bildirim yöntemiyle görünen uygulamanızda oluşturulan hatalar.

Genellikle, Azure depolama hizmetleriyle ilgili sorunlar dört geniş kategoriden birine girer:

* Uygulamanızın, kullanıcılarınız tarafından bildirilen veya performans ölçümlerinde yapılan değişikliklerle ortaya çıkan bir performans sorunu vardır.
* Bir veya daha fazla bölgede Azure Depolama altyapısıyla ilgili bir sorun var.
* Uygulamanız, kullanıcılarınız tarafından bildirilen veya izlediğiniz hata sayısı ölçümlerinden birinde bir artışla ortaya çıkan bir hatayla karşılaşıyor.
* Geliştirme ve test sırasında, yerel depolama emülatörü kullanıyor olabilirsiniz; özellikle depolama emülatörünün kullanımıyla ilgili bazı sorunlarla karşılaşabilirsiniz.

Aşağıdaki bölümler, bu dört kategorinin her birinde sorunları tanılamak ve sorun giderme için izlemeniz gereken adımları sıralar. Bu kılavuzda daha sonra yer alan "[Sorun Giderme kılavuzu]" bölümü, karşılaşabileceğiniz bazı sık karşılaşılan sorunlar için daha fazla ayrıntı sağlar.

### <a name="service-health-issues"></a><a name="service-health-issues"></a>Hizmet sağlığı sorunları
Hizmet sağlığı sorunları genellikle sizin denetiminiz dışındadır. [Azure portalı,](https://portal.azure.com) depolama hizmetleri de dahil olmak üzere Azure hizmetleriyle ilgili devam eden sorunlar hakkında bilgi sağlar. Depolama hesabınızı oluşturduğunuzda Oku-Erişim Coğrafi Yedekli Depolama'yı seçtiyseniz, verileriniz birincil konumda kullanılamıyorsa, uygulamanız geçici olarak ikincil konumdaki salt okunur kopyaya geçebilir. İkincilden okumak için, uygulamanızın birincil ve ikincil depolama konumlarını kullanma arasında geçiş yapabilmesi ve salt okunur verilerle daha az işlevsellik modunda çalışabilmesi gerekir. Azure Depolama İstemci kitaplıkları, birincil depolama alanından okunan bir okumanın başarısız olması durumunda ikincil depolama dan okuyabilen bir yeniden deneme ilkesi tanımlamanıza olanak tanır. Uygulamanızın ayrıca ikincil konumdaki verilerin sonunda tutarlı olduğunu bilmesi gerekir. Daha fazla bilgi için Azure Depolama Artıklığı Seçenekleri blog yazısına bakın [ve Access Geo Yedekli Depolama'yı okuyun.](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/)

### <a name="performance-issues"></a><a name="performance-issues"></a>Performans sorunları
Bir uygulamanın performansı, özellikle kullanıcının bakış açısıyla, öznel olabilir. Bu nedenle, bir performans sorunu olabilecek yerleri belirlemek için temel ölçümlere sahip olmak önemlidir. Birçok etken, istemci uygulaması açısından bir Azure depolama hizmetinin performansını etkileyebilir. Bu etkenler depolama hizmetinde, istemcide veya ağ altyapısında çalışabilir; bu nedenle performans sorununun kaynağını belirlemek için bir stratejiye sahip olmak önemlidir.

Performans sorununun nedeninin olası konumunu ölçümlerden belirledikten sonra, sorunu daha fazla tanılamak ve sorun gidermek için ayrıntılı bilgileri bulmak için günlük dosyalarını kullanabilirsiniz.

Bu kılavuzda yer alan "[Sorun Giderme kılavuzu"]bölümü, karşılaşabileceğiniz bazı yaygın performans sorunları hakkında daha fazla bilgi sağlar.

### <a name="diagnosing-errors"></a><a name="diagnosing-errors"></a>Hataları tanılama
Uygulamanızın kullanıcıları, istemci uygulaması tarafından bildirilen hataları size bildirebilir. Depolama Ölçümleri ayrıca **NetworkError,** **ClientTimeoutError**veya **AuthorizationError**gibi depolama hizmetlerinizden farklı hata türlerinin sayısını da kaydeder. Depolama Ölçümleri yalnızca farklı hata türlerinin sayısını kaydederken, sunucu tarafı, istemci tarafı ve ağ günlüklerini inceleyerek tek tek istekler hakkında daha fazla ayrıntı elde edebilirsiniz. Genellikle, depolama hizmeti tarafından döndürülen HTTP durum kodu, isteğin neden başarısız olduğunu gösterir.

> [!NOTE]
> Bazı aralıklı hatalar görmeyi beklemeniz gerektiğini unutmayın: örneğin, geçici ağ koşullarından kaynaklanan hatalar veya uygulama hataları.
>
>

Aşağıdaki kaynaklar, depolamayla ilgili durum ve hata kodlarının anlaşılması konusunda yararlıdır:

* [Ortak REST API Hata Kodları](https://msdn.microsoft.com/library/azure/dd179357.aspx)
* [Blob Servis Hata Kodları](https://msdn.microsoft.com/library/azure/dd179439.aspx)
* [Sıra Hizmeti Hata Kodları](https://msdn.microsoft.com/library/azure/dd179446.aspx)
* [Tablo Servisi Hata Kodları](https://msdn.microsoft.com/library/azure/dd179438.aspx)
* [Dosya Hizmeti Hata Kodları](https://msdn.microsoft.com/library/azure/dn690119.aspx)

### <a name="storage-emulator-issues"></a><a name="storage-emulator-issues"></a>Depolama emülatör sorunları
Azure SDK, geliştirme iş istasyonunda çalıştırabileceğiniz bir depolama emülatörü içerir. Bu emülatör, Azure depolama hizmetlerinin davranışlarının çoğunu simüle eder ve geliştirme ve test sırasında kullanışlıdır ve Azure depolama hizmetlerini kullanan uygulamaları Azure aboneliğine ve Azure depolama hesabına gerek kalmadan çalıştırmanızı sağlar.

Bu kılavuzun "[Sorun Giderme kılavuzu]" bölümü, depolama emülatörü kullanılarak karşılaşılan bazı sık karşılaşılan sorunları açıklar.

### <a name="storage-logging-tools"></a><a name="storage-logging-tools"></a>Depolama günlüğü araçları
Depolama Günlüğü, Azure depolama hesabınızda depolama isteklerinin sunucu tarafından günlüğe kaydedilmesini sağlar. Sunucu tarafında günlük oturum açmayı etkinleştirme ve günlük verilerine nasıl erişici lik hakkında daha fazla bilgi [için](https://go.microsoft.com/fwlink/?LinkId=510867)bkz.

.NET için Depolama İstemci Kitaplığı, uygulamanız tarafından gerçekleştirilen depolama işlemleriyle ilgili istemci tarafı günlük verilerini toplamanızı sağlar. Daha fazla bilgi için bkz. [.NET Depolama İstemci Kitaplığı ile İstemci Tarafı Günlük Kaydı](https://go.microsoft.com/fwlink/?LinkId=510868).

> [!NOTE]
> Bazı durumlarda (SAS yetkilendirme hataları gibi), bir kullanıcı sunucu tarafındaki Depolama günlüklerinde istek verisi bulamayacağınız bir hata bildirebilir. Sorunun nedeni istemci üzerinde olup olmadığını araştırmak veya ağı araştırmak için ağ izleme araçlarını kullanmak için Depolama İstemci Kitaplığı'nın günlük özelliklerini kullanabilirsiniz.
>
>

### <a name="using-network-logging-tools"></a><a name="using-network-logging-tools"></a>Ağ günlüğü araçlarını kullanma
İstemcinin ve sunucunun değiş tokuşu olan veriler ve temel ağ koşulları hakkında ayrıntılı bilgi sağlamak için istemci ve sunucu arasındaki trafiği yakalayabilirsiniz. Yararlı ağ günlüğe kaydetme araçları şunlardır:

* [Fiddler,](https://www.telerik.com/fiddler) HTTP ve HTTPS istek ve yanıt iletilerinin üstbilgi ve yük verilerini incelemenize olanak tanıyan ücretsiz bir web hata ayıklama proxy'sidir. Daha fazla bilgi için ek [1: HTTP ve HTTPS trafiğini yakalamak için Fiddler'ı kullanma bölümüne](#appendix-1)bakın.
* [Microsoft Ağ İzleyicisi (Netmon)](https://www.microsoft.com/download/details.aspx?id=4865) ve [Wireshark,](https://www.wireshark.org/) çok çeşitli ağ protokolleri için ayrıntılı paket bilgilerini görüntülemenizi sağlayan ücretsiz ağ protokolü çözümleyicileridir. Wireshark hakkında daha fazla bilgi için bkz: "[Ek 2: Ağ trafiğini yakalamak için Wireshark kullanma](#appendix-2)".
* Microsoft İleti Çözümleyicisi, Microsoft'un Netmon'un yerini alan ve ağ paketi verilerini yakalamanın yanı sıra diğer araçlardan yakalanan günlük verilerini görüntülemenize ve çözümlemenize yardımcı olan bir araçtır. Daha fazla bilgi için bkz: "[Ek 3: Ağ trafiğini yakalamak için Microsoft İleti Çözümleyicisini kullanma](#appendix-3)".
* İstemci makinenizin ağ üzerinden Azure depolama hizmetine bağlanıp bağlanamayacağını kontrol etmek için temel bir bağlantı testi gerçekleştirmek istiyorsanız, bunu istemcideki standart **ping** aracını kullanarak yapamazsınız. Ancak, bağlantı kontrol etmek için [ **tcping** aracını](https://www.elifulkerson.com/projects/tcping.php) kullanabilirsiniz.

Çoğu durumda, Depolama Günlüğü Günlüğü ve Depolama İstemci Kitaplığı'ndan gelen günlük verileri bir sorunu tanılamak için yeterli olacaktır, ancak bazı senaryolarda, bu ağ günlüğe kaydetme araçlarının sağlayabileceği daha ayrıntılı bilgilere ihtiyacınız olabilir. Örneğin, HTTP ve HTTPS iletilerini görüntülemek için Fiddler'ı kullanmak, depolama hizmetlerine gönderilen üstbilgi ve yük verilerini görüntülemenize olanak tanır ve bu da istemci uygulamasının depolama işlemlerini nasıl yeniden denediği üzerinde çalışmanızı sağlar. Wireshark gibi protokol çözümleyicileri, kayıp paketleri ve bağlantı sorunlarını gidermenize olanak tanıyan TCP verilerini görüntülemenizi sağlayan paket düzeyinde çalışır. İleti Çözümleyicisi hem HTTP hem de TCP katmanlarında çalışabilir.

## <a name="end-to-end-tracing"></a><a name="end-to-end-tracing"></a>Uçuça izleme
Çeşitli günlük dosyaları kullanarak uçtan uca izleme, olası sorunları araştırmak için yararlı bir tekniktir. Ölçüm verilerinizdeki tarih/saat bilgilerini, sorunu gidermenize yardımcı olacak ayrıntılı bilgiler için günlük dosyalarında aramaya başlayacağınızın bir göstergesi olarak kullanabilirsiniz.

### <a name="correlating-log-data"></a><a name="correlating-log-data"></a>Günlük verilerini ilişkilendirme
İstemci uygulamalarından, ağ izlerinden ve sunucu tarafındaki depolama günlüğe kaydetme günlüklerini görüntülerken, istekleri farklı günlük dosyaları arasında ilişkilendirebilmek çok önemlidir. Günlük dosyaları, korelasyon tanımlayıcıları olarak yararlı olan bir dizi farklı alan içerir. İstemci istek kimliği, farklı günlüklerde girişleri ilişkilendirmek için kullanılacak en yararlı alandır. Ancak bazen, sunucu istek kimliği veya zaman damgaları kullanmak yararlı olabilir. Aşağıdaki bölümlerde bu seçenekler hakkında daha fazla ayrıntı sağlay.)

### <a name="client-request-id"></a><a name="client-request-id"></a>İstemci istek kimliği
Depolama İstemci Kitaplığı, her istek için otomatik olarak benzersiz bir istemci istek kimliği oluşturur.

* Depolama İstemci Kitaplığı'nın oluşturduğu istemci tarafı günlüğünde, istemci istek kimliği, istekle ilgili her günlük girişinin **İstemci İstek Kimliği** alanında görünür.
* Fiddler tarafından yakalanan bir ağ izleme gibi bir ağ izleme, istemci istek kimliği **x-ms-client-request-id** http üstbilgi değeri olarak istek iletileri görünür.
* Sunucu tarafındaki Depolama Günlüğü günlüğünde, istemci istek kimliği İstemci istek kimliği sütununda görünür.

> [!NOTE]
> İstemci bu değeri atayabileceğinden (Depolama İstemci Kitaplığı otomatik olarak yeni bir değer atasa da) birden çok isteğin aynı istemci isteği kimliğini paylaşması mümkündür. İstemci yeniden çalıştığında, tüm denemeler aynı istemci isteği kimliğini paylaşır. İstemciden gönderilen bir toplu iş durumunda, toplu iş tek bir istemci istek kimliği vardır.
>
>

### <a name="server-request-id"></a><a name="server-request-id"></a>Sunucu istek Kimliği
Depolama hizmeti otomatik olarak sunucu istek titreşmeleri oluşturur.

* Sunucu tarafındaki Depolama Günlüğü günlüğünde, sunucu istek kimliği **İstek Kimliği üstbilgi** sütununda görünür.
* Fiddler tarafından yakalanan bir ağ izleme gibi bir ağ izleme, sunucu istek kimliği **x-ms-request-id** HTTP üstbilgi değeri olarak yanıt iletileri görünür.
* Depolama İstemci Kitaplığı'nın oluşturduğu istemci tarafı günlüğünde, sunucu isteği kimliği, sunucu yanıtının ayrıntılarını gösteren günlük girişi için **İşlem Metni** sütununda görüntülenir.

> [!NOTE]
> Depolama hizmeti her zaman aldığı her isteğe benzersiz bir sunucu istek kimliği atar, bu nedenle istemciden gelen her yeniden deneme girişimi ve toplu iş partisinde yer alan her işlem benzersiz bir sunucu istek kimliğine sahiptir.
>
>

Depolama İstemci Kitaplığı istemciye bir **Depolama Özel Durum** atarsa, **RequestInformation** özelliği **ServiceRequestID** özelliğini içeren bir **RequestResult** nesnesi içerir. Ayrıca, **Bir İşlem Bağlamı** örneğinden bir **RequestResult** nesnesi erişebilirsiniz.

Aşağıdaki kod örneği, **bir OperationContext** nesnesini depolama hizmetine ekleyerek özel bir **ClientRequestId** değerinin nasıl ayarlanıştuğa gösteriş verir. Ayrıca, yanıt iletisinden **ServerRequestId** değerinin nasıl alındığını da gösterir.

```csharp
//Parse the connection string for the storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Create an Operation Context that includes custom ClientRequestId string based on constants defined within the application along with a Guid.
OperationContext oc = new OperationContext();
oc.ClientRequestID = String.Format("{0} {1} {2} {3}", HOSTNAME, APPNAME, USERID, Guid.NewGuid().ToString());

try
{
    CloudBlobContainer container = blobClient.GetContainerReference("democontainer");
    ICloudBlob blob = container.GetBlobReferenceFromServer("testImage.jpg", null, null, oc);  
    var downloadToPath = string.Format("./{0}", blob.Name);
    using (var fs = File.OpenWrite(downloadToPath))
    {
        blob.DownloadToStream(fs, null, null, oc);
        Console.WriteLine("\t Blob downloaded to file: {0}", downloadToPath);
    }
}
catch (StorageException storageException)
{
    Console.WriteLine("Storage exception {0} occurred", storageException.Message);
    // Multiple results may exist due to client side retry logic - each retried operation will have a unique ServiceRequestId
    foreach (var result in oc.RequestResults)
    {
            Console.WriteLine("HttpStatus: {0}, ServiceRequestId {1}", result.HttpStatusCode, result.ServiceRequestID);
    }
}
```

### <a name="timestamps"></a><a name="timestamps"></a>Zaman damgaları
İlgili günlük girişlerini bulmak için zaman damgalarını da kullanabilirsiniz, ancak istemci ve sunucu arasında var olabilecek herhangi bir saat çarpıklığı konusunda dikkatli olun. İstemcinin zaman damgasını temel alan sunucu tarafındaki girişleri eşleştirmek için artı veya eksi 15 dakika arama yapın. Ölçümler içeren lekeler için blob meta verilerinin, blob'da depolanan ölçümlerin zaman aralığını gösterdiğini unutmayın. Bu zaman aralığı, aynı dakika veya saat için birçok metrik blobs varsa yararlıdır.

## <a name="troubleshooting-guidance"></a><a name="troubleshooting-guidance"></a>Sorun giderme kılavuzu
Bu bölüm, Azure depolama hizmetlerini kullanırken uygulamanızın karşılaşabileceği sık karşılaşılan bazı sorunlardan bazılarının tanılanması ve sorun giderme konusunda size yardımcı olacaktır. Özel sorununla ilgili bilgileri bulmak için aşağıdaki listeyi kullanın.

**Sorun Giderme Karar Ağacı**

---
Sorununuzun depolama hizmetlerinden birinin performansıyla bir ilgisi var mı?

* [Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor]
* [Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor, ancak istemci yüksek gecikme durumu yaşıyor]
* [Ölçümler yüksek AverageServerLatency gösteriyor]
* [Kuyrukta ileti tesliminde beklenmedik gecikmeler yaşıyorsunuz]

---
Sorununuzun depolama hizmetlerinden birinin kullanılabilirliğiyle bir ilgisi var mı?

* [Ölçümler PercentThrottlingError’da artış gösteriyor]
* [Ölçümler PercentTimeoutError’da artış gösteriyor]
* [Ölçümler PercentNetworkError’da artış gösteriyor]

---
 İstemci başvurunuz bir depolama hizmetinden HTTP 4XX (404 gibi) yanıt alıyor mu?

* [İstemci HTTP 403 (Yasak) iletileri alıyor]
* [İstemci HTTP 404 (Bulunamadı) iletileri alıyor]
* [İstemci HTTP 409 (Çakışma) iletileri alıyor]

---
[Ölçümler düşük YüzdeBaşarı veya analitik günlük girişleri ClientOtherErrors işlem durumu ile işlemleri var]

---
[Kapasite ölçümleri depolama kapasitesi kullanımında beklenmeyen bir artış gösterir]

---
[Çok sayıda bağlı VHD'ye sahip Sanal Makinelerin beklenmeyen yeniden başlatmalarını yaşıyorsunuz]

---
[Sorununuzun geliştirme veya test için depolama emülatörü kullanarak ortaya çıkar]

---
[.NET için Azure SDK'yı yüklerken sorunlarla karşılaşabilirsiniz]

---
[Depolama hizmetiyle ilgili farklı bir sorununuzun var]

---
### <a name="metrics-show-high-averagee2elatency-and-low-averageserverlatency"></a><a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor
[Azure portal](https://portal.azure.com) izleme aracından aşağıdaki **resimde, AverageE2ELatency'nin** **AverageServerLatency'den**önemli ölçüde daha yüksek olduğu bir örnek gösterilmektedir.

![][4]

Depolama hizmeti yalnızca başarılı istekler için metrik **AverageE2ELatency'yi** hesaplar ve **AverageServerLatency'nin**aksine, istemcinin verileri göndermek ve depolama hizmetinden onay almak için gereken süreyi içerir. Bu nedenle, **AverageE2ELatency** ve **AverageServerLatency** arasındaki fark, istemci uygulamasının yanıt vermede yavaş olması veya ağdaki koşullar nedeniyle olabilir.

> [!NOTE]
> Ayrıca Depolama Günlüğü günlüğü verilerinde tek tek depolama işlemleri için **E2ELatency** ve **ServerLatency'yi** görüntüleyebilirsiniz.
>
>

#### <a name="investigating-client-performance-issues"></a>İstemci performans sorunlarını araştırma
İstemcinin yavaş yanıt vermesinin olası nedenleri arasında sınırlı sayıda kullanılabilir bağlantı veya iş parçacığına sahip olmak veya CPU, bellek veya ağ bant genişliği gibi kaynaklarda düşük olmak sayılabilir. İstemci kodunu daha verimli olacak şekilde değiştirerek (örneğin depolama hizmetine eşzamanlı çağrılar kullanarak) veya daha büyük bir Sanal Makine (daha fazla çekirdek ve daha fazla bellek içeren) kullanarak sorunu çözebilirsiniz.

Tablo ve sıra hizmetleri için, Nagle algoritması da **AverageServerLatency**ile karşılaştırıldığında yüksek **AverageE2ELatency** neden olabilir: daha fazla bilgi için, sonrası [Nagle Algoritması Küçük İsteklere Karşı Dostu değildir](https://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx)bakın. **System.Net** ad alanında **ServicePointManager** sınıfını kullanarak Nagle algoritmasını kod olarak devre dışı kullanabilirsiniz. Bu zaten açık bağlantıları etkilemez, çünkü uygulamanızda tablo veya kuyruk hizmetleri herhangi bir arama yapmadan önce bunu yapmalısınız. Aşağıdaki örnek, bir işçi rolündeki **Application_Start** yönteminden gelir.

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);
ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
tableServicePoint.UseNagleAlgorithm = false;
ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
queueServicePoint.UseNagleAlgorithm = false;
```

İstemci uygulamanızın kaç istek gönderdiğini görmek için istemci tarafındaki günlükleri kontrol etmeli ve istemcinizde CPU, .NET çöp toplama, ağ kullanımı veya bellek gibi genel .NET ile ilgili performans darboğazlarını kontrol etmelisiniz. .NET istemci uygulamaları nın sorun giderilmesi için bir başlangıç noktası olarak, [Bkz. Hata Ayıklama, İzleme ve Profil Oluşturma.](https://msdn.microsoft.com/library/7fe0dd2y)

#### <a name="investigating-network-latency-issues"></a>Ağ gecikmesi sorunlarını araştırma
Genellikle, ağın neden olduğu yüksek uç-uç gecikme geçici koşullarnedeniyle. Wireshark veya Microsoft İleti Çözümleyicisi gibi araçları kullanarak bırakılan paketler gibi hem geçici hem de kalıcı ağ sorunlarını araştırabilirsiniz.

Ağ sorunlarını gidermek için Wireshark'ı kullanma hakkında daha fazla bilgi için bkz: "[Ek 2: Ağ trafiğini yakalamak için Wireshark'ı kullanma."]

Ağ sorunlarını gidermek için Microsoft İleti Çözümleyicisi'ni kullanma hakkında daha fazla bilgi için bkz.[Appendix 3: Using Microsoft Message Analyzer to capture network traffic]

### <a name="metrics-show-low-averagee2elatency-and-low-averageserverlatency-but-the-client-is-experiencing-high-latency"></a><a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor, ancak istemci yüksek gecikme durumu yaşıyor
Bu senaryoda, en olası neden depolama hizmetine ulaşan depolama isteklerindeki gecikmedir. İstemciden gelen isteklerin neden blob hizmetine geçmediğini araştırmanız gerekir.

İstemcinin istek göndermeyi geciktirmesinin olası nedenlerinden biri, sınırlı sayıda kullanılabilir bağlantı veya iş parçacığı olmasıdır.

Ayrıca istemcinin birden çok yeniden deneme yapıp yaptığını kontrol edin ve bunun nedenini araştırın. İstemcinin birden çok yeniden deneme yapıp yaptığını belirlemek için şunları yapabilirsiniz:

* Depolama Analizi günlüklerini inceleyin. Birden çok yeniden deneme gerçekleşiyorsa, aynı istemci istek kimliğine sahip, ancak farklı sunucu istek kimlikleri ne olursa o kadar çok işlem görürsünüz.
* İstemci günlüklerini inceleyin. Verbose günlüğe kaydetme, yeniden denemenin oluştuğunu gösterir.
* Kodunuzu hata ayıklayın ve istekle ilişkili **İşlem Bağlamı** nesnesinin özelliklerini denetleyin. İşlem yeniden denenmişse, **RequestResults** özelliği birden çok benzersiz sunucu istek tonu içerir. Ayrıca, her isteğin başlangıç ve bitiş saatlerini de kontrol edebilirsiniz. Daha fazla bilgi [için, sunucu istek kimliği]bölümündeki kod örneğine bakın.

İstemcide sorun yoksa, paket kaybı gibi olası ağ sorunlarını araştırmanız gerekir. Ağ sorunlarını araştırmak için Wireshark veya Microsoft İleti Çözümleyicisi gibi araçları kullanabilirsiniz.

Ağ sorunlarını gidermek için Wireshark'ı kullanma hakkında daha fazla bilgi için bkz: "[Ek 2: Ağ trafiğini yakalamak için Wireshark'ı kullanma."]

Ağ sorunlarını gidermek için Microsoft İleti Çözümleyicisi'ni kullanma hakkında daha fazla bilgi için bkz.[Appendix 3: Using Microsoft Message Analyzer to capture network traffic]

### <a name="metrics-show-high-averageserverlatency"></a><a name="metrics-show-high-AverageServerLatency"></a>Ölçümler yüksek AverageServerLatency gösteriyor
Blob indirme istekleri için yüksek **AverageServerLatency** durumunda, aynı blob (veya blobs kümesi) için tekrarlanan istekler olup olmadığını görmek için Depolama Günlük günlükleri kullanmalısınız. Blob yükleme istekleri için, istemcinin hangi blok boyutunu kullandığını araştırmanız gerekir (örneğin, 64 K'den küçük bloklar, okumalar da 64 K'den az olduğu sürece genel giderlere neden olabilir) ve birden çok istemci blokları aynı blob'a yüklüyorsa Paralel. Ayrıca, saniye başına ölçeklenebilirlik hedeflerinin aşılmasıyla sonuçlanan istek sayısındaki ani artışlar için dakika başına ölçümleri de kontrol etmelisiniz: ayrıca bkz: "[Ölçümler Yüzde Zaman Atlama Hatasında bir artış gösterir."]

Aynı blob veya blob kümesi nin tekrarlanan istekleri olduğunda blob indirme istekleri için yüksek **AverageServerLatency** görüyorsanız, bu lekeleri Azure Önbelleği veya Azure İçerik Dağıtım Ağı 'nı (CDN) kullanarak önbelleğe almayı düşünmelisiniz. Yükleme istekleri için, daha büyük bir blok boyutu kullanarak iş boyutunu artırabilirsiniz. Tablolara yapılan sorgular için, aynı sorgu işlemlerini gerçekleştiren ve verilerin sık sık değişmediği istemcilerde istemci tarafı önbelleğe alma işlemi de mümkündür.

Yüksek **AverageServerLatency** değerleri, tarama işlemleriyle sonuçlanan veya ek/prepend anti-deseni izleyen kötü tasarlanmış tabloların veya sorguların bir belirtisi de olabilir. Daha fazla bilgi için bkz: "[Ölçümler YüzdeThrottlingError'ta bir artış gösterir]".

> [!NOTE]
> Kapsamlı bir denetim listesi performans denetim listesini burada bulabilirsiniz: [Microsoft Azure Depolama Performansı ve Ölçeklenebilirlik Denetim Listesi.](storage-performance-checklist.md)
>
>

### <a name="you-are-experiencing-unexpected-delays-in-message-delivery-on-a-queue"></a><a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>Kuyrukta ileti tesliminde beklenmedik gecikmeler yaşıyorsunuz
Bir uygulamanın kuyruğa ileti eklediği saat ile kuyruktan okunabilen süre arasında bir gecikme yaşıyorsanız, sorunu tanılamak için aşağıdaki adımları uygulamanız gerekir:

* Uygulamanın iletileri sıraya başarıyla eklediğini doğrulayın. Uygulamanın başarılı olmadan önce **AddMessage** yöntemini birkaç kez yeniden denemediğini denetleyin. Depolama İstemci Kitaplığı günlükleri depolama işlemleri tekrarlanan retries gösterecektir.
* İletiyi kuyruğa ekleyen alt rol ile işlemde bir gecikme varmış gibi görünmesini sağlayan sıradaki iletiyi okuyan alt rol arasında saat eğriliği olmadığını doğrulayın.
* Kuyruktaki iletileri okuyan alt rolün başarısız olup olmadığını denetleyin. Bir kuyruk istemcisi **GetMessage** yöntemini çağırır ancak bir bildirimle yanıt vermezse, **görünmezlikZaman aşımı** süresi dolana kadar ileti kuyrukta görünmez kalır. Bu noktada, ileti yeniden işlenmek üzere kullanılabilir hale gelir.
* Sıra uzunluğunun zaman içinde büyüyüp büyümeyip büyümeyip büyümeyip büyümeyonu kontrol edin. Bu, diğer çalışanların sıraya yerleştiren tüm iletileri işlemek için yeterli sayıda çalışanyoksa oluşabilir. Ayrıca silme isteklerinin başarısız olup olmadığını görmek için ölçümleri ve iletileri silmeye yönelik tekrarlanan başarısız girişimleri gösteren iletilerin sırasını silme sayısını da denetleyin.
* Normalden daha uzun bir süre boyunca beklenenden daha yüksek **E2ELatency** ve **ServerLatency** değerlerine sahip tüm sıra işlemleri için Depolama Günlüğü günlüklerini inceleyin.

### <a name="metrics-show-an-increase-in-percentthrottlingerror"></a><a name="metrics-show-an-increase-in-PercentThrottlingError"></a>Ölçümler PercentThrottlingError’da artış gösteriyor
Bir depolama hizmetinin ölçeklenebilirlik hedeflerini aştığınızda azaltma hataları oluşur. Depolama hizmeti, tek bir istemcinin veya kiracının hizmeti başkalarının pahasına kullanamadığından emin olmak için daraltır. Daha fazla bilgi için, depolama hesapları için ölçeklenebilirlik hedefleri ve depolama hesaplarındaki bölümler için performans hedefleri hakkında ayrıntılar için standart depolama hesapları için [Ölçeklenebilirlik](scalability-targets-standard-account.md) ve performans hedeflerine bakın.

**YüzdeThrottlingError** ölçümü, azaltma hatasıyla başarısız olan isteklerin yüzdesinde bir artış gösteriyorsa, iki senaryodan birini araştırmanız gerekir:

* [YüzdethrottlingError'ta geçici artış]
* [Yüzdelik ArtmaThrottlingError hatası]

**YüzdeThrottlingError'ta** bir artış genellikle depolama isteklerinin sayısındaki artışla aynı anda veya uygulamanızı ilk yükleme testi yaparken oluşur. Bu durum istemcide depolama işlemlerinden gelen "503 Server Busy" veya "500 Operation Timetime" HTTP durum iletileri olarak da kendini gösterebilir.

#### <a name="transient-increase-in-percentthrottlingerror"></a><a name="transient-increase-in-PercentThrottlingError"></a>YüzdethrottlingError'ta geçici artış
Uygulama için yüksek etkinlik dönemlerine denk gelen **YüzdeThrottlingError** değerinde ani artışlar görüyorsanız, istemcinizde yeniden denemeler için üstel (doğrusal olmayan) bir geri dönüş stratejisi uygularsınız. Geri dönüş yeniden denemeleri, bölümüzerindeki hemen yükü azaltır ve uygulamanızın trafikteki ani artışları düzeltmesine yardımcı olur. Depolama İstemci Kitaplığı'nı kullanarak yeniden deneme ilkeleri nin nasıl uygulanacağı hakkında daha fazla bilgi için [Microsoft.Azure.Storage.RetryPolicies ad alanına](/dotnet/api/microsoft.azure.storage.retrypolicies)bakın.

> [!NOTE]
> Ayrıca uygulama için yüksek faaliyet dönemleri ile çakışmayan **YüzdeThrottlingError** değerinde ani artışlar görebilirsiniz: burada en olası nedeni depolama hizmeti yük dengeleme geliştirmek için bölümleri hareketli.
>
>

#### <a name="permanent-increase-in-percentthrottlingerror-error"></a><a name="permanent-increase-in-PercentThrottlingError"></a>Yüzdelik ArtmaThrottlingError hatası
İşlem hacimlerinizdeki kalıcı bir artışı takiben **YüzdeThrottlingError** için sürekli olarak yüksek bir değer görüyorsanız veya uygulamanızda ilk yük testlerinizi gerçekleştirirken, uygulamanızın depolama bölümlerini nasıl kullandığını ve bir depolama hesabı için ölçeklenebilirlik hedeflerine yaklaşıp yaklaşmadığını değerlendirmeniz gerekir. Örneğin, bir kuyrukta azaltma hataları görüyorsanız (tek bir bölüm olarak sayılır), hareketleri birden çok bölüme yaymak için ek kuyruklar kullanmayı düşünmelisiniz. Tabloda azaltma hataları görüyorsanız, daha geniş bir bölüm anahtar değerleri aralığı nı kullanarak işlemlerinizi birden çok bölüme yaymak için farklı bir bölümleme düzeni kullanmayı düşünmeniz gerekir. Bu sorunun yaygın bir nedeni, bölümü anahtar olarak tarihi seçtiğiniz ve belirli bir günde tüm verilerin tek bir bölüme yazıldığı prepend/ek anti-desendir: yük altında, bu bir yazma darboğazına neden olabilir. Ya farklı bir bölümleme tasarımı düşünün veya blob depolama kullanarak daha iyi bir çözüm olabilir olup olmadığını değerlendirmek. Ayrıca, trafiğinizdeki ani artışlar sonucunda azaltmanın meydana gelip gelmediğini kontrol edin ve istek düzeninizi düzeltmenin yollarını araştırın.

Hareketlerinizi birden çok bölüme dağıtırsanız, depolama hesabı için ayarlanan ölçeklenebilirlik sınırlarının farkında olmalısınız. Örneğin, her biri saniyede en fazla 2.000 1KB iletisi işleyen on sıra kullandıysanız, depolama hesabı için saniyede 20.000 iletinin toplam sınırında olursunuz. Saniyede 20.000'den fazla varlığı işlemeniz gerekiyorsa, birden çok depolama hesabı kullanmayı düşünmelisiniz. Ayrıca, depolama hizmetinin müşterilerinizi azaltması üzerinde isteklerinizin ve varlıkların izinin boyutunun bir etkisi olduğunu da unutmamalısınız: daha büyük istekleriniz ve varlıklarınız varsa, daha erken azaltılabilirsiniz.

Verimsiz sorgu tasarımı, tablo bölümleri için ölçeklenebilirlik sınırlarına basmanıza da neden olabilir. Örneğin, bir bölümdeki varlıkların yalnızca yüzde birini seçen ancak bir bölümdeki tüm varlıkları tarayan bir filtreye sahip bir sorgunun her varlığa erişebilmek için olması gerekir. Okunan her varlık, söz söz le ilgili bölümdeki toplam hareket sayısına dahil edilecektir; bu nedenle ölçeklenebilirlik hedeflerine kolayca ulaşabilirsiniz.

> [!NOTE]
> Performans testiniz, uygulamanızdaki verimsiz sorgu tasarımlarını ortaya çıkarmalıdır.
>
>

### <a name="metrics-show-an-increase-in-percenttimeouterror"></a><a name="metrics-show-an-increase-in-PercentTimeoutError"></a>Ölçümler PercentTimeoutError’da artış gösteriyor
Ölçümleriniz, depolama hizmetlerinizden biri için **YüzdeTimeoutError'ta** bir artış gösterir. Aynı zamanda, istemci depolama işlemlerinden yüksek hacimli "500 Operasyon Zaman Azamanı" HTTP durum iletileri alır.

> [!NOTE]
> Depolama hizmeti yükü yeni bir sunucuya bir bölüm taşıyarak istekleri dengeler gibi zaman zaman ası hataları geçici olarak görebilirsiniz.
>
>

**PercentTimeoutError** metrik aşağıdaki ölçümlerin bir toplama: **ClientTimeoutError**, **AnonymousClientTimeoutError**, **SASClientTimeoutError**, **ServerTimeoutError**, **AnonymousServerTimeoutError**, ve **SASServerTimeoutError**.

Sunucu zaman ekmeleri sunucudaki bir hatadan kaynaklanır. Sunucudaki bir işlem istemci tarafından belirtilen zaman aşımını aştığından istemci zaman aşımı gerçekleşir; örneğin, Depolama İstemci Kitaplığı'nı kullanan bir istemci, **QueueRequestOptions** sınıfının **ServerTimeout** özelliğini kullanarak bir işlem için bir zaman ara sı caydırabilir.

Sunucu zaman zaman ları, depolama hizmetiyle ilgili daha fazla araştırma gerektiren bir sorun olduğunu gösterir. Hizmetin ölçeklenebilirlik sınırlarını vurup aşdığınızı görmek ve trafikte bu soruna neden olabilecek ani artışları belirlemek için ölçümleri kullanabilirsiniz. Sorun aralıklı ise, hizmetteki yük dengeleme etkinliğinden kaynaklanıyor olabilir. Sorun kalıcıysa ve uygulamanızın hizmetin ölçeklenebilirlik sınırlarına çarpmasından kaynaklaşmıyorsa, bir destek sorunu yükseltmeniz gerekir. İstemci zaman zaman zaman ları için, zaman adedinin istemcide uygun bir değere ayarlanıp ayarlanacağına karar vermeli ve istemcide ayarlanan zaman alameti değerini değiştirmeli veya örneğin depolama hizmetindeki işlemlerin performansını nasıl iyileştirebileceğinizi araştırmanız gerekir. tablo sorgularınız veya iletilerinizin boyutunu küçültmek.

### <a name="metrics-show-an-increase-in-percentnetworkerror"></a><a name="metrics-show-an-increase-in-PercentNetworkError"></a>Ölçümler PercentNetworkError’da artış gösteriyor
Ölçümleriniz, depolama hizmetlerinizden biri için **YüzdeAğ Hatası'nda** bir artış gösterir. **PercentNetworkError** ölçümü aşağıdaki ölçümlerin bir toplamasidır: **NetworkError**, **AnonymousNetworkError**ve **SASNetworkError**. Bunlar, istemci bir depolama isteği yaptığında depolama hizmeti bir ağ hatası algıladığında oluşur.

Bu hatanın en yaygın nedeni, depolama hizmetinde bir zaman aşımı süresi dolmadan önce istemcinin bağlantısını kesmektir. İstemcinin depolama hizmetiyle bağlantısını neden ve ne zaman kestiğini anlamak için istemcinizdeki kodu araştırın. İstemcinin ağ bağlantısı sorunlarını araştırmak için Wireshark, Microsoft Message Analyzer veya Tcping'i de kullanabilirsiniz. Bu araçlar [Eklerde]açıklanmıştır.

### <a name="the-client-is-receiving-http-403-forbidden-messages"></a><a name="the-client-is-receiving-403-messages"></a>İstemci HTTP 403 (Yasak) iletileri alıyor
İstemci uygulamanızda HTTP 403 (Yasak) hataları oluşuyorsa büyük ihtimalle istemci depolama isteği gönderirken süresi dolmuş bir Paylaşılan Erişim İmzası kullandığı içindir (saat sapması, geçersiz anahtarlar ve boş üst bilgiler gibi başka olası nedenler de vardır). Sorun süresi dolmuş bir SAS anahtarından kaynaklanıyorsa sunucu tarafı Depolama Günlük Kaydı günlük verilerinde herhangi bir giriş görmezsiniz. Aşağıdaki tablo, Depolama İstemci Kitaplığı tarafından oluşturulan istemci tarafı günlüğünden oluşan ve bu sorunun oluştuğunu gösteren bir örneği gösterir:

| Kaynak | Ayrıntı Düzeyi | Ayrıntı Düzeyi | İstemci istek kimliği | İşlem metni |
| --- | --- | --- | --- | --- |
| Microsoft.Azure.Depolama |Bilgi |3 |85d077ab-... |Konum başına Birincil konum modu PrimaryIle çalıştırma ya. |
| Microsoft.Azure.Depolama |Bilgi |3 |85d077ab -... |Senkron isteğin başlatılması<https://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&sr=c&si=mypolicy&sig=OFnd4Rd7z01fIvh%2BmcR6zbudIH2F5Ikm%2FyhNYZEmJNQ%3D&api-version=2014-02-14> |
| Microsoft.Azure.Depolama |Bilgi |3 |85d077ab -... |Yanıt bekliyorum. |
| Microsoft.Azure.Depolama |Uyarı |2 |85d077ab -... |Yanıt beklerken atılan özel durum: Uzak sunucu bir hata döndürür: (403) Yasaktır. |
| Microsoft.Azure.Depolama |Bilgi |3 |85d077ab -... |Yanıt alındı. Durum kodu = 403, İstek Kimliği = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d, İçerik-MD5 = , ETag = . |
| Microsoft.Azure.Depolama |Uyarı |2 |85d077ab -... |İşlem sırasında atılan özel durum: Uzak sunucu bir hata döndürür: (403) Yasak... |
| Microsoft.Azure.Depolama |Bilgi |3 |85d077ab -... |İşlemin yeniden denenip denenmemesi gerektiğini kontrol edin. Retry count = 0, HTTP durum kodu = 403, Özel Durum = Uzak sunucu bir hata döndü: (403) Yasak... |
| Microsoft.Azure.Depolama |Bilgi |3 |85d077ab -... |Sonraki konum, konum moduna göre Birincil olarak ayarlandı. |
| Microsoft.Azure.Depolama |Hata |1 |85d077ab -... |Yeniden deneme ilkesi yeniden denemeye izin vermedi. Uzak sunucu ile başarısız bir hata döndü: (403) Yasak. |

Bu senaryoda, istemci belirteci sunucuya göndermeden önce SAS belirteci süresinin neden dolduğunu araştırmanız gerekir:

* Genellikle bir istemcinin hemen kullanması için SAS oluştururken başlangıç zamanı ayarlamamalısınız. Geçerli zamanı kullanarak SAS belirtecini oluşturan konak ile depolama hizmeti arasında küçük saat farklılıkları varsa depolama hizmeti henüz geçerli olmayan bir SAS alabilir.
* Bir SAS belirtecinin sona erme süresini çık kısa ayarlamayın. Ayrıca, SAS belirtecini oluşturan konakla depolama hizmeti arasındaki küçük saat farklılıkları, bir SAS belirtecinin süresinin beklenenden erken dolmuş gibi görünmesine de neden olabilir.
* SAS anahtarındaki sürüm parametresi (örneğin **sv=2015-04-05)** kullandığınız Depolama İstemci Kitaplığı sürümüyle eşleşir mi? [Depolama İstemci Kitaplığı'nın](https://www.nuget.org/packages/WindowsAzure.Storage/)en son sürümünü her zaman kullanmanızı öneririz.
* Depolama erişim anahtarlarınızı yeniden oluşturursanız mevcut SAS belirteçleri geçerliliğini kaybedebilir. İstemci uygulamalarının önbelleğe alması için sona erme süresi uzun olan SAS belirteçleri oluşturursanız bu sorunla karşılaşabilirsiniz.

SAS belirteçleri oluşturmak için Depolama İstemcisi Kitaplığı’nı kullanıyorsanız geçerli bir belirteç oluşturmak kolaydır. Ancak, Storage REST API'sini kullanıyorsanız ve SAS belirteçlerini elle oluşturuyorsanız, [Erişim'i Paylaşılan Erişim İmzasıyla Atama](https://msdn.microsoft.com/library/azure/ee395415.aspx)bölümüne bakın.

### <a name="the-client-is-receiving-http-404-not-found-messages"></a><a name="the-client-is-receiving-404-messages"></a>İstemci HTTP 404 (Bulunamadı) iletileri alıyor
İstemci uygulaması sunucudan HTTP 404 (Bulunamadı) iletisi alırsa istemcinin kullanmaya çalıştığı nesne (bir varlık, tablo, blob, kapsayıcı veya kuyruk gibi) depolama hizmetinde mevcut değil demektir. Bunun aşağıdaki gibi çeşitli olası nedenleri vardır:

* [İstemci veya başka bir işlem daha önce nesneyi silmiş]
* [Paylaşılan Erişim İmzası (SAS) yetkilendirme sorunu]
* [İstemci tarafı JavaScript kodunun nesneye erişim izni yok]
* [Ağ hatası]

#### <a name="the-client-or-another-process-previously-deleted-the-object"></a><a name="client-previously-deleted-the-object"></a>İstemci veya başka bir işlem daha önce nesneyi silmiş
İstemcinin bir depolama hizmetindeki verileri okumaya, güncelleştirmeye veya silmeye çalıştığı senaryolarda, söz konusu nesneyi depolama hizmetinden silen önceki bir işlemi sunucu tarafında tanımlaması genellikle kolaydır. Genellikle, günlük verileri başka bir kullanıcının veya işlemin nesneyi sildiğini gösterir. Sunucu tarafındaki Depolama Günlüğü günlüğünde, işlem türü ve istenen nesne anahtarı sütunları, istemci bir nesneyi sildiğinde gösterir.

İstemci bir nesne eklemeye çalıştığı senaryoda, istemcinin yeni bir nesne oluşturduğu göz önüne alındığında bunun neden bir HTTP 404 (bulunamadı) yanıtıyla sonuçlandığı hemen açık olmayabilir. Ancak, istemci bir blob oluşturuyorsa blob kapsayıcısını bulabilmeli, istemci bir ileti oluşturuyorsa bir sıra bulabilmeli ve istemci bir satır ekliyorsa tabloyu bulabilmeli.

İstemcinin depolama hizmetine belirli istekleri ne zaman gönderdiğini daha ayrıntılı bir şekilde anlamak için Depolama İstemci Kitaplığı'ndaki istemci tarafı günlüğünü kullanabilirsiniz.

Depolama İstemci kitaplığı tarafından oluşturulan aşağıdaki istemci tarafı günlüğü, istemci oluşturduğu blob için kapsayıcı bulamıyorsa sorunu gösterir. Bu günlük, aşağıdaki depolama işlemlerinin ayrıntılarını içerir:

| İstek Kimliği | İşlem |
| --- | --- |
| 07b26a5d-... |**DeleteIfExists** yöntemi blob kapsayıcısını silmek için. Bu işlem, kapsayıcının varlığını denetlemek için bir **HEAD** isteği içerdiğini unutmayın. |
| e2d06d78... |Blob kapsayıcıoluşturmak için **CreateIfNotExists** yöntemi. Bu işlem, kapsayıcının varlığını denetleyen bir **HEAD** isteği içerdiğini unutmayın. **HEAD** bir 404 iletisi döndürür, ancak devam ediyor. |
| de8b1c3c-... |Blob oluşturmak için **UploadFromStream** yöntemi. **PUT** isteği 404 iletisiyle başarısız olur |

Günlük girişleri:

| İstek Kimliği | İşlem Metni |
| --- | --- |
| 07b26a5d-... |Senkron isteği niçin https://domemaildist.blob.core.windows.net/azuremmblobcontainerbaşlatılın. |
| 07b26a5d-... |StringToSign = HEAD............ x-ms-istemci-istek-id:07b26a5d-.... x-ms-date:Sal, 03 Jun 2014 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| 07b26a5d-... |Yanıt bekliyorum. |
| 07b26a5d-... |Yanıt alındı. Durum kodu = 200, İstek Kimliği = eeead849-... İçerik-MD5 = , &quot;ETag = 0x8D14D2DC63D059B&quot;. |
| 07b26a5d-... |Yanıt başlıkları başarıyla işlendi ve operasyonun geri kalanı yla devam etti. |
| 07b26a5d-... |Yanıt gövdesiindir. |
| 07b26a5d-... |İşlem başarıyla tamamlandı. |
| 07b26a5d-... |Senkron isteği niçin https://domemaildist.blob.core.windows.net/azuremmblobcontainerbaşlatılın. |
| 07b26a5d-... |StringToSign = DELETE............ x-ms-istemci-istek-id:07b26a5d-.... x-ms-date:Sal, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| 07b26a5d-... |Yanıt bekliyorum. |
| 07b26a5d-... |Yanıt alındı. Durum kodu = 202, İstek Kimliği = 6ab2a4cf-..., İçerik-MD5 = , ETag = . |
| 07b26a5d-... |Yanıt başlıkları başarıyla işlendi ve operasyonun geri kalanı yla devam etti. |
| 07b26a5d-... |Yanıt gövdesiindir. |
| 07b26a5d-... |İşlem başarıyla tamamlandı. |
| e2d06d78-... |Asynchronous isteği https://domemaildist.blob.core.windows.net/azuremmblobcontainernin başlatılması.</td> |
| e2d06d78-... |StringToSign = HEAD............ x-ms-istemci-istek-id:e2d06d78-.... x-ms-date:Sal, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| e2d06d78-... |Yanıt bekliyorum. |
| de8b1c3c-... |Senkron isteği niçin https://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txtbaşlatılın. |
| de8b1c3c-... |StringToSign = PUT... 64.qCmF+TQLPhq/YYK50mP9ZQ==........ x-ms-blob-tipi:BlockBlob.x-ms-client-request-id:de8b1c3c-.... x-ms-tarih:Sal, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer/blobCreated.txt. |
| de8b1c3c-... |İstek verilerini yazmaya hazırlanıyorum. |
| e2d06d78-... |Yanıt beklerken atılan özel durum: Uzak sunucu bir hata döndürür: (404) Bulunamadı... |
| e2d06d78-... |Yanıt alındı. Durum kodu = 404, İstek Kimliği = 353ae3bc-..., İçerik-MD5 = , ETag = . |
| e2d06d78-... |Yanıt başlıkları başarıyla işlendi ve operasyonun geri kalanı yla devam etti. |
| e2d06d78-... |Yanıt gövdesiindir. |
| e2d06d78-... |İşlem başarıyla tamamlandı. |
| e2d06d78-... |Asynchronous isteği https://domemaildist.blob.core.windows.net/azuremmblobcontainernin başlatılması. |
| e2d06d78-... |StringToSign = PUT... 0.........x-ms-istemci-istek-id:e2d06d78-.... x-ms-date:Sal, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| e2d06d78-... |Yanıt bekliyorum. |
| de8b1c3c-... |İstek verilerini yazma. |
| de8b1c3c-... |Yanıt bekliyorum. |
| e2d06d78-... |Yanıt beklerken atılan özel durum: Uzak sunucu bir hata döndürür: (409) Çakışma... |
| e2d06d78-... |Yanıt alındı. Durum kodu = 409, İstek Kimliği = c27da20e-..., İçerik-MD5 = , ETag = . |
| e2d06d78-... |Hata yanıt gövdesi indirilir. |
| de8b1c3c-... |Yanıt beklerken atılan özel durum: Uzak sunucu bir hata döndürür: (404) Bulunamadı... |
| de8b1c3c-... |Yanıt alındı. Durum kodu = 404, İstek Kimliği = 0eaeab3e-..., İçerik-MD5 = , ETag = . |
| de8b1c3c-... |İşlem sırasında atılan özel durum: Uzak sunucu bir hata döndürür: (404) Bulunamadı... |
| de8b1c3c-... |Yeniden deneme ilkesi yeniden denemeye izin vermedi. Uzak sunucu ile başarısız bir hata döndü: (404) Bulunamadı... |
| e2d06d78-... |Yeniden deneme ilkesi yeniden denemeye izin vermedi. Uzak sunucu ile başarısız bir hata döndü: (409) Çakışma... |

Bu örnekte, günlük, istemcinin **UploadFromStream** yönteminden (de8b1c3c-... gelen isteklerle **CreateIfNotExists** yönteminden (request ID e2d06d78...) gelen istekleri birarada bıraktığını gösterir. İstemci uygulaması bu yöntemleri eşit bir şekilde çağırdığından, bu ara ayrılma gerçekleşir. Bu kapsayıcıdaki bir blob'a herhangi bir veri yüklemeye çalışmadan önce kapsayıcıyı oluşturduğundan emin olmak için istemcideki eşzamanlı kodu değiştirin. İdeal olarak, önceden tüm konteynerler oluşturmalısınız.

#### <a name="a-shared-access-signature-sas-authorization-issue"></a><a name="SAS-authorization-issue"></a>Paylaşılan Erişim İmzası (SAS) yetkilendirme sorunu
İstemci uygulaması işlem için gerekli izinleri içermeyen bir SAS anahtarı kullanmaya çalışırsa, depolama hizmeti istemciye bir HTTP 404 (bulunamadı) iletisi döndürür. Aynı zamanda, ölçümlerde **SASAuthorizationError** için sıfır olmayan bir değer de görürsünüz.

Aşağıdaki tablo, Depolama Günlüğü günlüğü dosyasından örnek bir sunucu tarafı günlük iletisini gösterir:

| Adı | Değer |
| --- | --- |
| Başlangıç saati isteme | 2014-05-30T06:17:48.4473697Z |
| İşlem türü     | GetBlobProperties            |
| İstek durumu     | SASAuthorizationError        |
| HTTP durum kodu   | 404                          |
| Kimlik doğrulaması türü| Sas                          |
| Hizmet türü       | Blob                         |
| İstek URL'si        | https://domemaildist.blob.core.windows.net/azureimblobcontainer/blobCreatedViaSAS.txt |
| &nbsp;                 |   ?sv=2014-02-14&sr=c&si=mypolicy&;&sig=XXXXX api-version=2014-02-14 |
| Kimlik üstbilgisi isteği  | a1f348d5-8032-4912-93ef-b393e5252a3b |
| İstemci istek kimliği  | 2d064953-8436-4ee0-aa0c-65cb874f7929 |


İstemci uygulamanızın neden izin verilmediği bir işlemi gerçekleştirmeye çalıştığını araştırın.

#### <a name="client-side-javascript-code-does-not-have-permission-to-access-the-object"></a><a name="JavaScript-code-does-not-have-permission"></a>İstemci tarafı JavaScript kodunun nesneye erişim izni yok
Bir JavaScript istemcisi kullanıyorsanız ve depolama hizmeti HTTP 404 iletilerini iade ediyorsa, tarayıcıda aşağıdaki JavaScript hatalarını denetlersiniz:

```
SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.
```

> [!NOTE]
> İstemci tarafındaki JavaScript sorunlarını giderirken tarayıcı ile depolama hizmeti arasında değiş tokuş edilen iletileri izlemek için Internet Explorer'daki F12 Geliştirici Araçlarını kullanabilirsiniz.
>
>

Bu hatalar, web tarayıcısının bir web sayfasının geldiği etki alanından farklı bir etki alanında API aramasını engelleyen [aynı kaynak ilkesi güvenlik](https://www.w3.org/Security/wiki/Same_Origin_Policy) kısıtlamasını uygulaması nedeniyle oluşur.

JavaScript sorununu çözmek için, istemcinin eriştiği depolama hizmeti için Çapraz Kaynak Paylaşımını (CORS) yıkılayabilir. Daha fazla bilgi için Azure [Depolama Hizmetleri için BaşlangıçLar Arası Kaynak Paylaşımı (CORS) Desteği'ne](https://msdn.microsoft.com/library/azure/dn535601.aspx)bakın.

Aşağıdaki kod örneği, Contoso etki alanında çalışan JavaScript'in blob depolama hizmetinizdeki bir blob'a erişmesine izin verecek şekilde blob hizmetinizi nasıl yapılandırıştırır:

```csharp
CloudBlobClient client = new CloudBlobClient(blobEndpoint, new StorageCredentials(accountName, accountKey));
// Set the service properties.
ServiceProperties sp = client.GetServiceProperties();
sp.DefaultServiceVersion = "2013-08-15";
CorsRule cr = new CorsRule();
cr.AllowedHeaders.Add("*");
cr.AllowedMethods = CorsHttpMethods.Get | CorsHttpMethods.Put;
cr.AllowedOrigins.Add("http://www.contoso.com");
cr.ExposedHeaders.Add("x-ms-*");
cr.MaxAgeInSeconds = 5;
sp.Cors.CorsRules.Clear();
sp.Cors.CorsRules.Add(cr);
client.SetServiceProperties(sp);
```

#### <a name="network-failure"></a><a name="network-failure"></a>Ağ Hatası
Bazı durumlarda, kayıp ağ paketleri depolama hizmetinin http 404 iletilerini istemciye geri döndürmesine neden olabilir. Örneğin, istemci uygulamanız tablo hizmetinden bir varlığı silerken, istemcinin tablo hizmetinden "HTTP 404 (Bulunamadı)" durum iletisi bildiren bir depolama özel durumu attığını görürsünüz. Tablo depolama hizmetindeki tabloyu araştırdığınızda, hizmetin varlığı istenildiği gibi sildiğine görürsünüz.

İstemcideki özel durum ayrıntıları, istek için tablo hizmeti tarafından atanan istek kimliğini (7e84f12d...) içerir: bu bilgileri, günlük dosyasındaki **istek kimliği üstbilgi** sütununda arama yaparak sunucu tarafındaki depolama günlüklerinde istek ayrıntılarını bulmak için kullanabilirsiniz. Bu gibi hataların ne zaman oluştuğunu belirlemek ve ardından ölçümlerin bu hatayı kaydettiği zamana göre günlük dosyalarını aramak için ölçümleri de kullanabilirsiniz. Bu günlük girişi, silmenin "HTTP (404) İstemci Diğer Hata" durum iletisiyle başarısız olduğunu gösterir. Aynı günlük girişi, **istemci-istek-id** sütununda (813ea74f...) istemci tarafından oluşturulan istek kimliğini de içerir.

Sunucu tarafı günlüğü, aynı varlık için başarılı bir silme işlemi için ve aynı **istemciden** aynı istemci isteği kimliği değerine (813ea74f...) sahip başka bir giriş de içerir. Bu başarılı silme işlemi, başarısız silme isteğinden çok kısa bir süre önce gerçekleşti.

Bu senaryonun en olası nedeni, istemcinin tablo hizmetine varlık için bir silme isteği göndermesidir, bu da başarılı, ancak sunucudan bir onay almamış olmasıdır (belki de geçici bir ağ sorunu nedeniyle). İstemci daha sonra işlemi otomatik olarak yeniden denedi (aynı **istemci isteği-kimliği**kullanılarak) ve varlık zaten silindiği için bu yeniden deneme başarısız oldu.

Bu sorun sık sık oluşuyorsa, istemcinin tablo hizmetinden neden onay almadığını araştırmanız gerekir. Sorun aralıklı ysa, "HTTP (404) Bulunamadı" hatasını bindirmeli ve istemcide oturum açmalı, ancak istemcinin devam etmesine izin vermelisiniz.

### <a name="the-client-is-receiving-http-409-conflict-messages"></a><a name="the-client-is-receiving-409-messages"></a>İstemci HTTP 409 (Çakışma) iletileri alıyor
Aşağıdaki tablo, iki istemci işlemi için sunucu tarafındaki günlükten bir ekstresi gösterir: **DeleteIfExists** hemen ardından aynı blob kapsayıcı adını kullanarak **CreateIfNotExists'ı takip edecektir.** Her istemci işlemi sunucuya gönderilen iki istekle sonuçlanır, önce kapsayıcının var olup olmadığını denetlemek için **getcontainerproperties** isteği, ardından **DeleteContainer** veya **CreateContainer** isteği.

| Zaman damgası | İşlem | Sonuç | Kapsayıcı adı | İstemci istek kimliği |
| --- | --- | --- | --- | --- |
| 05:10:13.7167225 |GetContainer Özellikleri |200 |mmcont |c9f52c89-... |
| 05:10:13.8167325 |Silme Kapsayıcısı |202 |mmcont |c9f52c89-... |
| 05:10:13.8987407 |GetContainer Özellikleri |404 |mmcont |bc881924-... |
| 05:10:14.2147723 |Oluşturma Kapsayıcısı |409 |mmcont |bc881924-... |

İstemci uygulamasındaki kod siler ve hemen aynı adı kullanarak bir blob kapsayıcısını yeniden oluşturur: **CreateIfNotExists** yöntemi (İstemci istek kimliği bc881924-...) sonunda HTTP 409 (Çakışma) hatasıyla başarısız olur. Bir istemci blob kapsayıcılarını, tabloları veya kuyrukları sildiğinde adın yeniden kullanılabilir hale gelmesi biraz zaman alır.

Silme/yeniden oluşturma düzeni genelse istemci uygulaması yeni kapsayıcı oluştururken benzersiz kapsayıcı adları kullanmalıdır.

### <a name="metrics-show-low-percentsuccess-or-analytics-log-entries-have-operations-with-transaction-status-of-clientothererrors"></a><a name="metrics-show-low-percent-success"></a>Ölçümler düşük YüzdeBaşarı veya analitik günlük girişleri ClientOtherErrors işlem durumu ile işlemleri var
**YüzdeBaşarı** ölçümü, HTTP Durum Kodu'na göre başarılı olan işlemlerin yüzdesini yakalar. 2XX durum kodlarına sahip işlemler başarılı sayılırken, 3XX, 4XX ve 5XX aralıklarında durum kodlarına sahip işlemler başarısız sayılır ve **Yüzde Başarı** metrik değerini düşürür. Sunucu tarafındaki depolama günlüğü dosyalarında, bu işlemler **ClientOtherErrors'ın**işlem durumuyla kaydedilir.

Bu işlemlerin başarıyla tamamlandığını ve bu nedenle kullanılabilirlik gibi diğer ölçümleri etkilemediğini unutmayın. Başarılı bir şekilde yürütülmesine karşın başarısız HTTP durum kodlarına neden olabilecek bazı işlemler örnekleri şunlardır:

* **ResourceNotFound** (Bulunamadı 404), örneğin bir GET isteğinden var olmayan bir blob'a.
* **KaynakZatenVar** (Çakış409), örneğin kaynağın zaten var olduğu **createifnotexist** işleminden.
* **ConditionNotMet** (Değiştirilmemiş 304), örneğin bir istemcinin bir **ETag** değeri ve bir HTTP **If-None-Match** üstbilgisini yalnızca son işlemden beri güncelleştirilmiş bir görüntü istemek için göndermesi gibi koşullu bir işlemden elde edilmiştir.

Depolama hizmetlerinin [ortak REST API Hata Kodları](https://msdn.microsoft.com/library/azure/dd179357.aspx)sayfasında döndürdebildiği yaygın REST API hata kodlarının bir listesini bulabilirsiniz.

### <a name="capacity-metrics-show-an-unexpected-increase-in-storage-capacity-usage"></a><a name="capacity-metrics-show-an-unexpected-increase"></a>Kapasite ölçümleri depolama kapasitesi kullanımında beklenmeyen bir artış gösterir
Depolama hesabınızda kapasite kullanımında ani ve beklenmeyen değişiklikler görürseniz, önce kullanılabilirlik ölçümlerinize bakarak nedenleri araştırabilirsiniz; örneğin, başarısız silme isteklerinin sayısındaki artış, uygulamaya özgü temizleme işlemleri olarak kullandığınız blob depolama miktarında bir artışa neden olabilir, alanı boşaltmayı beklediğiniz gibi çalışmayabilir (örneğin, , yer açmak için kullanılan SAS belirteçlerinin süresi dolduğundan).

### <a name="your-issue-arises-from-using-the-storage-emulator-for-development-or-test"></a><a name="your-issue-arises-from-using-the-storage-emulator"></a>Sorununuzun geliştirme veya test için depolama emülatörü kullanarak ortaya çıkar
Bir Azure depolama hesabı gereksinimini önlemek için genellikle geliştirme ve test sırasında depolama emülatörü kullanırsınız. Depolama emülatörü kullanırken oluşabilecek sık karşılaşılan sorunlar şunlardır:

* [Özellik "X" depolama emülatörü çalışmıyor]
* [Depolama emülatörü kullanırken hata "HTTP üstbilgilerinden birinin değeri doğru biçimde değildir" hatası]
* [Depolama emülatörün çalıştırılması, yönetim ayrıcalıkları gerektirir]

#### <a name="feature-x-is-not-working-in-the-storage-emulator"></a><a name="feature-X-is-not-working"></a>Özellik "X" depolama emülatörü çalışmıyor
Depolama emülatörü, dosya hizmeti gibi Azure depolama hizmetlerinin tüm özelliklerini desteklemez. Daha fazla bilgi için [bkz.](storage-use-emulator.md)

Depolama emülatörü desteklemediği özellikler için buluttaki Azure depolama hizmetini kullanın.

#### <a name="error-the-value-for-one-of-the-http-headers-is-not-in-the-correct-format-when-using-the-storage-emulator"></a><a name="error-HTTP-header-not-correct-format"></a>Depolama emülatörü kullanırken hata "HTTP üstbilgilerinden birinin değeri doğru biçimde değildir" hatası
Depolama İstemci Kitaplığı'nı kullanan uygulamanızı yerel depolama emülatörüyle karşı test ediyorsanız ve **CreateIfNotExists** gibi yöntem çağrıları "HTTP üstbilgilerinden birinin değeri doğru biçimde değildir" hata iletisiyle başarısız olur. Bu, kullanmakta olduğunuz depolama emülatörü sürümünün kullandığınız depolama istemcisi kitaplığı sürümünü desteklemediğini gösterir. Depolama İstemci Kitaplığı, üstbilgi **x-ms sürümünü** yaptığı tüm isteklere ekler. Depolama emülatörü **x-ms sürümü** üstbilgisindeki değeri tanımıyorsa, isteği reddeder.

Gönderdiği **x-ms sürüm üstbilgisinin** değerini görmek için Depolama Kitaplığı İstemci günlüklerini kullanabilirsiniz. İstemci uygulamanızdan gelen istekleri izlemek için Fiddler kullanıyorsanız **x-ms-sürüm üstbilgisinin** değerini de görebilirsiniz.

Bu senaryo genellikle depolama istemcisi kitaplığını niçin depolama emülatörü güncelleştirmeden yükler ve kullanırsanız oluşur. Depolama emülatörünün en son sürümünü yüklemeniz veya geliştirme ve sınama için emülatör yerine bulut depolamayı kullanmanız gerekir.

#### <a name="running-the-storage-emulator-requires-administrative-privileges"></a><a name="storage-emulator-requires-administrative-privileges"></a>Depolama emülatörün çalıştırılması, yönetim ayrıcalıkları gerektirir
Depolama emülatörü çalıştırdığınızda yönetici kimlik bilgileri istenir. Bu yalnızca depolama emülatörü ilk kez başlatılması nda oluşur. Depolama emülatörü nin başlatılmasını yaptıktan sonra, yeniden çalıştırmak için yönetim ayrıcalıklarına gerek yoktur.

Daha fazla bilgi için [bkz.](storage-use-emulator.md) Ayrıca Visual Studio'da depolama emülatörü nün başlatılmasını da sağlayabilirsiniz, bu da yönetim ayrıcalıkları gerektirir.

### <a name="you-are-encountering-problems-installing-the-azure-sdk-for-net"></a><a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>.NET için Azure SDK'yı yüklerken sorunlarla karşılaşabilirsiniz
SDK'yı yüklemeye çalıştığınızda, depolama emülatörü yerel makinenize yüklemeye çalışırken başarısız olur. Yükleme günlüğü aşağıdaki iletilerden birini içerir:

* CAQuietExec: Hata: SQL örneğine erişilemiyor
* CAQuietExec: Hata: Veritabanı oluşturulamıyor

Nedeni, varolan LocalDB yüklemesiyle ilgili bir sorundur. Varsayılan olarak, depolama emülatörü Azure depolama hizmetlerini simüle ettiğinde verileri sürdürmek için LocalDB kullanır. SDK'yı yüklemeye çalışmadan önce aşağıdaki komutları komut istemi penceresinde çalıştırarak LocalDB örneğini sıfırlayabilirsiniz.

```
sqllocaldb stop v11.0
sqllocaldb delete v11.0
delete %USERPROFILE%\WAStorageEmulatorDb3*.*
sqllocaldb create v11.0
```

**Silme** komutu, depolama emülatörüönceki yüklemeleri herhangi bir eski veritabanı dosyaları kaldırır.

### <a name="you-have-a-different-issue-with-a-storage-service"></a><a name="you-have-a-different-issue-with-a-storage-service"></a>Depolama hizmetiyle ilgili farklı bir sorununuzun var
Önceki sorun giderme bölümleri bir depolama hizmetiyle ilgili sorununuzu içermiyorsa, sorununuzu tanılamak ve sorun giderme için aşağıdaki yaklaşımı benimsemeniz gerekir.

* Beklenen taban çizgisi davranışınızda herhangi bir değişiklik olup olmadığını görmek için ölçümlerinizi denetleyin. Ölçümlerden, sorunun geçici veya kalıcı olup olmadığını ve sorunun hangi depolama işlemlerini etkilediğini belirleyebilirsiniz.
* Meydana gelen hatalar hakkında daha ayrıntılı bilgi için sunucu tarafındaki günlük verilerinizde arama yapmanıza yardımcı olmak için metrik bilgilerini kullanabilirsiniz. Bu bilgiler sorunu gidermenize ve çözmenize yardımcı olabilir.
* Sunucu tarafındaki günlüklerde yer alan bilgiler sorunu başarıyla gidermek için yeterli değilse, istemci uygulamanızın davranışını ve Fiddler, Wireshark ve Microsoft gibi araçları araştırmak için Depolama İstemci Kitaplığı istemci tarafı günlüklerini kullanabilirsiniz Ağınızı araştırmak için İleti Çözümleyicisi.

Fiddler kullanma hakkında daha fazla bilgi için bkz: "[Ek 1: HTTP ve HTTPS trafiğini yakalamak için Fiddler'ı kullanma."]

Wireshark kullanımı hakkında daha fazla bilgi için bkz: "[Ek 2: Ağ trafiğini yakalamak için Wireshark kullanma."]

Microsoft İleti Çözümleyicisi'ni kullanma hakkında daha fazla bilgi için bkz: "[Ek 3: Ağ trafiğini yakalamak için Microsoft İleti Çözümleyicisi'ni kullanma."]

## <a name="appendices"></a><a name="appendices"></a>Ekler
Ekler, Azure Depolama (ve diğer hizmetler) ile ilgili sorunları tanılarken ve sorun giderirken yararlı bulabileceğiniz birkaç aracı açıklar. Bu araçlar Azure Depolama'nın bir parçası değildir ve bazıları üçüncü taraf ürünlerdir. Bu nedenle, bu eklerde tartışılan araçlar Microsoft Azure veya Azure Depolama ile yapmış olabileceğiniz herhangi bir destek sözleşmesi kapsamında değildir ve bu nedenle değerlendirme sürecinizin bir parçası olarak, bu araçların sağlayıcıları.

### <a name="appendix-1-using-fiddler-to-capture-http-and-https-traffic"></a><a name="appendix-1"></a>Ek 1: HTTP ve HTTPS trafiğini yakalamak için Fiddler'ı kullanma
[Fiddler,](https://www.telerik.com/fiddler) istemci uygulamanız ile kullanmakta olduğunuz Azure depolama hizmeti arasındaki HTTP ve HTTPS trafiğini çözümlemeniz için kullanışlı bir araçtır.

> [!NOTE]
> Fiddler HTTPS trafiğini çözebilir; Bunu nasıl yaptığını ve güvenlik sonuçlarını anlamak için Fiddler belgelerini dikkatle okumalısınız.
>
>

Bu ek, Fiddler'ı yüklediğiniz yerel makine ile Azure depolama hizmetleri arasındaki trafiği yakalamak için Fiddler'ı nasıl yapılandıracağınız hakkında kısa bir gözden geçirme sağlar.

Fiddler'ı başlattıktan sonra, yerel makinenizde HTTP ve HTTPS trafiğini yakalamaya başlayacaktır. Fiddler'ı denetlemek için bazı yararlı komutlar şunlardır:

* Durun ve trafiği yakalamaya başlayın. Ana **menüde, Dosya'ya** gidin ve ardından yakalama ve kapatma geçişini sağlamak için **Trafiği Kapat'ı** tıklatın.
* Yakalanan trafik verilerini kaydedin. Ana menüde **Dosya'ya**gidin , **Kaydet'i**tıklatın ve ardından Tüm **Oturumlar'ı**tıklatın: bu, oturum arşivi dosyasındaki trafiği kaydetmenizi sağlar. Oturum Arşivini daha sonra çözümleme için yeniden yükleyebilir veya microsoft desteğine istenirse gönderebilirsiniz.

Fiddler'ın yakaladığı trafik miktarını sınırlamak için **Filtreler** sekmesinde yapılandırdığınız filtreleri kullanabilirsiniz. Aşağıdaki ekran görüntüsü, **yalnızca contosoemaildist.table.core.windows.net** depolama bitiş noktasına gönderilen trafiği yakalayan bir filtreyi gösterir:

![][5]

### <a name="appendix-2-using-wireshark-to-capture-network-traffic"></a><a name="appendix-2"></a>Ek 2: Ağ trafiğini yakalamak için Wireshark kullanma
[Wireshark,](https://www.wireshark.org/) çok çeşitli ağ protokolleri için ayrıntılı paket bilgilerini görüntülemenizi sağlayan bir ağ protokolü çözümleyicisidir.

Aşağıdaki yordam, Wireshark'ı yüklediğiniz yerel makineden Azure depolama hesabınızdaki tablo hizmetine trafik için ayrıntılı paket bilgilerini nasıl yakaladığınızı gösterir.

1. Wireshark'ı yerel makinenize fırlatın.
2. **Başlangıç** bölümünde, internete bağlı yerel ağ arabirimini veya arabirimleri seçin.
3. **Yakalama Seçenekleri'ni**tıklatın.
4. **Yakalama Filtresi** metin kutusuna bir filtre ekleyin. Örneğin, **ana bilgisayar contosoemaildist.table.core.windows.net** Wireshark'ı yalnızca **contosoemaildist** depolama hesabındaki tablo hizmeti bitiş noktasına veya tablo hizmeti bitiş noktasından gönderilen paketleri yakalamak için yapılandıracaktır. [Yakalama Filtreleri'nin tam listesine](https://wiki.wireshark.org/CaptureFilters)göz atın.

   ![][6]
5. **Başlat**'a tıklayın. Wireshark artık yerel makinenizde istemci uygulamanızı kullanırken tablo hizmeti bitiş noktasına veya tablo hizmeti bitiş noktasından gönderen tüm paketleri yakalayacak.
6. Bitirdikten sonra, ana menüde **Capture** Yakala'yı tıklatın ve ardından **Durdurun'u tıklatın.**
7. Yakalanan verileri Wireshark Yakalama Dosyasında kaydetmek için ana menüde **Dosyayı** tıklatın ve sonra **Kaydet'** i tıklatın.

WireShark **paket listesi** penceresinde var olan hataları vurgular. Hataların ve uyarıların özetini görüntülemek için **Uzman Bilgileri** penceresini **(Çözümle,** ardından **Uzman Bilgilerini**Analiz et'i) de kullanabilirsiniz.

![][7]

Ayrıca, uygulama katmanı TCP verilerine sağ tıklayarak ve **TCP Akışını Takip Et'i**seçerek TCP verilerini görür ken görüntülemeyi de seçebilirsiniz. Bu, çöplüğünüyakalama filtresi olmadan yakaladıysanız kullanışlıdır. Daha fazla bilgi için Bkz. [TCP Akışları'nı Takip Edin.](https://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html)

![][8]

> [!NOTE]
> Wireshark kullanma hakkında daha fazla bilgi için [Wireshark Kullanıcı Kılavuzu'na](https://www.wireshark.org/docs/wsug_html_chunked)bakın.
>
>

### <a name="appendix-3-using-microsoft-message-analyzer-to-capture-network-traffic"></a><a name="appendix-3"></a>Ek 3: Ağ trafiğini yakalamak için Microsoft İleti Çözümleyicisini kullanma
Http ve HTTPS trafiğini Fiddler'a benzer bir şekilde yakalamak ve ağ trafiğini Wireshark'a benzer bir şekilde yakalamak için Microsoft İleti Çözümleyicisi'ni kullanabilirsiniz.

#### <a name="configure-a-web-tracing-session-using-microsoft-message-analyzer"></a>Microsoft İleti Çözümleyicisi'ni kullanarak bir web izleme oturumu yapılandırma
Microsoft İleti Çözümleyicisi'ni kullanarak HTTP ve HTTPS trafiği için bir web izleme oturumu yapılandırmak için Microsoft İleti Çözümleyicisi uygulamasını çalıştırın ve **ardından Dosya** menüsünde **Capture/Trace'i**tıklatın. Kullanılabilir izleme senaryoları listesinde **Web Proxy'yi**seçin. Ardından, **İzleme Senaryosu Yapılandırma** panelinde, **HostnameFilter** textbox'ına, depolama uç noktalarınızın adlarını ekleyin (bu adları [Azure portalında](https://portal.azure.com)arayabilirsiniz). Örneğin, Azure depolama hesabınızın adı **contosodata**ise, **HostnameFilter** metin kutusuna aşağıdakileri eklemeniz gerekir:

```
contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net
```

> [!NOTE]
> Boşluk karakteri ana bilgisayar adlarını ayırır.
>
>

İzleme verileri toplamaya başlamaya hazır olduğunuzda, **Ile Başlat** düğmesini tıklatın.

Microsoft İleti **Çözümleyicisi Web Proxy** izleme hakkında daha fazla bilgi için [Microsoft-PEF-WebProxy Sağlayıcısı'na](https://technet.microsoft.com/library/jj674814.aspx)bakın.

Microsoft İleti Çözümleyicisi'ndeki yerleşik **Web Proxy** izleme Fiddler'ı temel adatır; istemci tarafındaki HTTPS trafiğini yakalayabilir ve şifrelenmemiş HTTPS iletilerini görüntüleyebilir. **Web Proxy** izleme, şifrelenmemiş iletilere erişim sağlayan tüm HTTP ve HTTPS trafiği için yerel bir proxy yapılandırarak çalışır.

#### <a name="diagnosing-network-issues-using-microsoft-message-analyzer"></a>Microsoft İleti Çözümleyicisi'ni kullanarak ağ sorunlarını tanılama
İstemci uygulaması ve depolama hizmeti arasındaki HTTP/HTTPs trafiğinin ayrıntılarını yakalamak için Microsoft İleti **Çözümleyicisi Web Proxy** izlemesini kullanmanın yanı sıra, ağ paketi bilgilerini yakalamak için yerleşik **Yerel Bağlantı Katmanı** izlemesini de kullanabilirsiniz. Bu, Wireshark ile yakalayabileceğiniz verilere benzer verileri yakalamanızı ve bırakılan paketler gibi ağ sorunlarını tanılamanızı sağlar.

Aşağıdaki ekran görüntüsü, **DiagnosisTypes** sütunundaki bazı **bilgilendirme** iletileriyle birlikte yerel **bağlantı katmanı** izleme sini gösterir. **DiagnosisTypes** sütunundaki bir simgeyi tıklattığınızda iletinin ayrıntıları gösterilir. Bu örnekte, sunucu istemciden bir onay almadığı için #305 iletiyi yeniden aktardı:

![][9]

Microsoft İleti Çözümleyicisi'nde izleme oturumu oluşturduğunuzda, izlemedeki gürültü miktarını azaltmak için filtreler belirtebilirsiniz. İzlemeyi tanımladığınız **Yakalama / İzleme** sayfasında, **Microsoft-Windows-NDIS-PacketCapture'in**yanındaki **Yapıla** bağlantısını tıklatın. Aşağıdaki ekran görüntüsü, üç depolama hizmetinin IP adresleri için TCP trafiğini filtreleyen bir yapılandırmayı gösterir:

![][10]

Microsoft İleti Çözümleyicisi Yerel Bağlantı Katmanı izleme hakkında daha fazla bilgi için [Microsoft-PEF-NDIS-PacketCapture Provider'a](https://technet.microsoft.com/library/jj659264.aspx)bakın.

### <a name="appendix-4-using-excel-to-view-metrics-and-log-data"></a><a name="appendix-4"></a>Ek 4: Ölçümleri ve günlük verilerini görüntülemek için Excel'i kullanma
Birçok araç, Depolama Ölçümleri verilerini Azure tablo depolama alanından, görüntüleme ve analiz için verileri Excel'e yüklemeyi kolaylaştıran sınırlı bir biçimde indirmenize olanak tanır. Azure blob depolamadan Depolama Günlüğü verileri, Excel'e yükleyebileceğiniz sınırlı bir biçimde zaten. Ancak, [Depolama Analytics Log Format](https://msdn.microsoft.com/library/azure/hh343259.aspx) ve Storage [Analytics Metrics Table Schema'daki](https://msdn.microsoft.com/library/azure/hh343264.aspx)bilgilere dayalı uygun sütun başlıkları eklemeniz gerekir.

Depolama Günlüğü verilerinizi blob depolamadan indirdikten sonra Excel'e almak için:

* **Veriler** menüsünde **Metinden'i**tıklatın.
* Görüntülemek istediğiniz günlük dosyasına göz atın ve **İçe Aktar'ı**tıklatın.
* **Metin Alma Sihirbazı'nın**1. **Delimited**

**Metin Alma Sihirbazı'nın**1. **Semicolon** **Text qualifier** Ardından **Finish'i** tıklatın ve verileri çalışma kitabınıza nereye yerleştireceğinizi seçin.

### <a name="appendix-5-monitoring-with-application-insights-for-azure-devops"></a><a name="appendix-5"></a>Ek 5: Azure DevOps için Uygulama Öngörüleri ile İzleme
Azure DevOps için Uygulama Öngörüleri özelliğini, performans ve kullanılabilirlik izlemenizin bir parçası olarak da kullanabilirsiniz. Bu araç şunları yapabilir:

* Web hizmetinizin kullanılabilir ve yanıt veren olduğundan emin olun. Uygulamanız ister bir web sitesi ister bir web hizmeti kullanan bir cihaz uygulaması olsun, URL'nizi dünyanın dört bir yanındaki konumlardan birkaç dakikada bir sınayabilir ve bir sorun olup olmadığını size bildirebilir.
* Web hizmetinizdeki performans sorunlarını veya özel durumlarını hızla tanıtın. CPU veya diğer kaynakların uzatılMakta olup olmadığını öğrenin, özel durumlardan yığın izlemeleri alın ve günlük izlerinde kolayca arama yapın. Uygulamanın performansı kabul edilebilir sınırların altına düşerse, Microsoft size bir e-posta gönderebilir. Hem .NET hem de Java web hizmetlerini izleyebilirsiniz.

[Uygulama Öngörüleri Nedir](../../azure-monitor/app/app-insights-overview.md)hakkında daha fazla bilgi bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Depolama'da analitik hakkında daha fazla bilgi için şu kaynaklara bakın:

* [Azure portalında depolama hesabını izleme](storage-monitor-storage-account.md)
* [Depolama analitiği](storage-analytics.md)
* [Depolama analizi ölçümleri](storage-analytics-metrics.md)
* [Depolama analizi ölçümleri tablo şeması](/rest/api/storageservices/storage-analytics-metrics-table-schema)
* [Depolama analizi günlükleri](storage-analytics-logging.md)
* [Depolama analizi günlük biçimi](/rest/api/storageservices/storage-analytics-log-format)

<!--Anchors-->
[Giriş]: #introduction
[Bu kılavuz nasıl düzenlenir?]: #how-this-guide-is-organized

[Depolama hizmetinizi izleme]: #monitoring-your-storage-service
[Hizmet durumunun izlenmesi]: #monitoring-service-health
[İzleme kapasitesi]: #monitoring-capacity
[Kullanılabilirliği izleme]: #monitoring-availability
[Performansı izleme]: #monitoring-performance

[Depolama sorunlarını tanılama]: #diagnosing-storage-issues
[Hizmet sağlığı sorunları]: #service-health-issues
[Performans sorunları]: #performance-issues
[Hataları tanılama]: #diagnosing-errors
[Depolama emülatör sorunları]: #storage-emulator-issues
[Depolama günlüğü araçları]: #storage-logging-tools
[Ağ günlüğü araçlarını kullanma]: #using-network-logging-tools

[Uçuça izleme]: #end-to-end-tracing
[Günlük verilerini ilişkilendirme]: #correlating-log-data
[İstemci istek kimliği]: #client-request-id
[Sunucu istek Kimliği]: #server-request-id
[Zaman damgaları]: #timestamps

[Sorun giderme kılavuzu]: #troubleshooting-guidance
[Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[Ölçümler yüksek AverageE2ELatency ve düşük AverageServerLatency gösteriyor, ancak istemci yüksek gecikme durumu yaşıyor]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[Ölçümler yüksek AverageServerLatency gösteriyor]: #metrics-show-high-AverageServerLatency
[Kuyrukta ileti tesliminde beklenmedik gecikmeler yaşıyorsunuz]: #you-are-experiencing-unexpected-delays-in-message-delivery

[Ölçümler PercentThrottlingError’da artış gösteriyor]: #metrics-show-an-increase-in-PercentThrottlingError
[YüzdethrottlingError'ta geçici artış]: #transient-increase-in-PercentThrottlingError
[Yüzdelik ArtmaThrottlingError hatası]: #permanent-increase-in-PercentThrottlingError
[Ölçümler PercentTimeoutError’da artış gösteriyor]: #metrics-show-an-increase-in-PercentTimeoutError
[Ölçümler PercentNetworkError’da artış gösteriyor]: #metrics-show-an-increase-in-PercentNetworkError

[İstemci HTTP 403 (Yasak) iletileri alıyor]: #the-client-is-receiving-403-messages
[İstemci HTTP 404 (Bulunamadı) iletileri alıyor]: #the-client-is-receiving-404-messages
[İstemci veya başka bir işlem daha önce nesneyi silmiş]: #client-previously-deleted-the-object
[Paylaşılan Erişim İmzası (SAS) yetkilendirme sorunu]: #SAS-authorization-issue
[İstemci tarafı JavaScript kodunun nesneye erişim izni yok]: #JavaScript-code-does-not-have-permission
[Ağ hatası]: #network-failure
[İstemci HTTP 409 (Çakışma) iletileri alıyor]: #the-client-is-receiving-409-messages

[Ölçümler düşük YüzdeBaşarı veya analitik günlük girişleri ClientOtherErrors işlem durumu ile işlemleri var]: #metrics-show-low-percent-success
[Kapasite ölçümleri depolama kapasitesi kullanımında beklenmeyen bir artış gösterir]: #capacity-metrics-show-an-unexpected-increase
[Sorununuzun geliştirme veya test için depolama emülatörü kullanarak ortaya çıkar]: #your-issue-arises-from-using-the-storage-emulator
[Özellik "X" depolama emülatörü çalışmıyor]: #feature-X-is-not-working
[Depolama emülatörü kullanırken hata "HTTP üstbilgilerinden birinin değeri doğru biçimde değildir" hatası]: #error-HTTP-header-not-correct-format
[Depolama emülatörün çalıştırılması, yönetim ayrıcalıkları gerektirir]: #storage-emulator-requires-administrative-privileges
[.NET için Azure SDK'yı yüklerken sorunlarla karşılaşabilirsiniz]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[Depolama hizmetiyle ilgili farklı bir sorununuzun var]: #you-have-a-different-issue-with-a-storage-service

[Ekler]: #appendices
[Ek 1: HTTP ve HTTPS trafiğini yakalamak için Fiddler'ı kullanma]: #appendix-1
[Ek 2: Ağ trafiğini yakalamak için Wireshark kullanma]: #appendix-2
[Ek 3: Ağ trafiğini yakalamak için Microsoft İleti Çözümleyicisini kullanma]: #appendix-3
[Ek 4: Ölçümleri ve günlük verilerini görüntülemek için Excel'i kullanma]: #appendix-4
[Ek 5: Azure DevOps için Uygulama Öngörüleri ile İzleme]: #appendix-5

<!--Image references-->
[1]: ./media/storage-monitoring-diagnosing-troubleshooting/overview.png
[3]: ./media/storage-monitoring-diagnosing-troubleshooting/hour-minute-metrics.png
[4]: ./media/storage-monitoring-diagnosing-troubleshooting/high-e2e-latency.png
[5]: ./media/storage-monitoring-diagnosing-troubleshooting/fiddler-screenshot.png
[6]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-1.png
[7]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-2.png
[8]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-3.png
[9]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-1.png
[10]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-2.png
