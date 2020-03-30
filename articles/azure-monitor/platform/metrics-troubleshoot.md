---
title: Azure Monitörü metrik grafiklerinde sorun giderme
description: Metrik grafikler oluşturma, özelleştirme veya yorumlama ile ilgili sorunları giderme
author: vgorbenko
services: azure-monitor
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: e1ad4e53596b8228bdef5beb18aa250a9512c49f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77659671"
---
# <a name="troubleshooting-metrics-charts"></a>Ölçüm grafikleriyle ilgili sorunları giderme

Azure ölçümleri gezgininde grafik oluşturma, özelleştirme veya yorumlama sorunlarıyla karşınıza çıktıysanız bu makaleyi kullanın. Ölçümlerde yeniyseniz, ölçümler explorer ve [ölçümler explorer'ın gelişmiş özellikleriyle](metrics-charts.md) [başlama](metrics-getting-started.md) hakkında bilgi edinin. Yapılandırılan metrik grafiklerin [örneklerini](metric-chart-samples.md) de görebilirsiniz.

## <a name="cant-find-your-resource-to-select-it"></a>Kaynağınızı seçmek için bulamıyorum

**Kaynak seç** düğmesine tıkladınız ama kaynak seçici iletişim kutusunda kaynağı görmüyorsunuz.

**Çözüm:** Metrics explorer, kullanılabilir kaynakları listelemeden önce abonelikleri ve kaynak gruplarını seçmeniz gerekir. Kaynağınızı göremiyorsanız:

1. **Abonelik** açılan listesinde doğru aboneliği seçtiğinizden emin olun. Aboneliğiniz listelenmiyorsa **Dizin + Abonelik ayarları**'na tıklayın ve kaynağınızla bir abonelik ekleyin.

1. Doğru kaynak grubunu seçtiğinizden emin olun.
    > [!WARNING]
    > En iyi performansı elde etmek için, ölçüm gezginini ilk açtığınızda **Kaynak grubu** açılan listesinde önceden seçilmiş kaynak grubu yoktur. Herhangi bir kaynak görmek için önce en az bir grup seçmelisiniz.

## <a name="chart-shows-no-data"></a>Grafik hiçbir veri gösterir

Bazen grafikler, doğru kaynakları ve ölçümleri seçtikten sonra veri göstermeyebilir. Bu davranış, aşağıdaki nedenlerden birkaçı neden olabilir:

### <a name="microsoftinsights-resource-provider-isnt-registered-for-your-subscription"></a>Aboneliğiniz için Microsoft.Insights kaynak sağlayıcısı kaydedilmedi

Ölçümlerin incelenmesi için aboneliğinizde *Microsoft.Insights* kaynak sağlayıcısının kaydedilmesi gerekir. Çoğu durumda otomatik olarak kaydedilir (başka bir deyişle siz bir uyarı kuralını yapılandırdıktan, herhangi bir kaynak için tanılama ayarlarını özelleştirdikten veya otomatik ölçeklendirme kuralı yapılandırdıktan sonra). Microsoft.Insights kaynak sağlayıcısı kayıtlı değilse, [Azure kaynak sağlayıcıları nda ve türlerinde](../../azure-resource-manager/management/resource-providers-and-types.md)açıklanan adımları izleyerek bu sağlayıcıyı el ile kaydetmeniz gerekir.

**Çözüm:** **Açık Abonelikler,** **Kaynak sağlayıcıları** sekmesi ve *Microsoft.Insights'ın* aboneliğiniz için kayıtlı olduğunu doğrulayın.

### <a name="you-dont-have-sufficient-access-rights-to-your-resource"></a>Kaynağınız üzerinde yeterli erişim haklarınız yok

Azure'da ölçümlere erişim [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md) tarafından denetlenir. Herhangi bir kaynağın ölçümlerini incelemek için [izleme okuyucusu](../../role-based-access-control/built-in-roles.md#monitoring-reader), [izleme katkıda bulunanı](../../role-based-access-control/built-in-roles.md#monitoring-contributor) veya [katkıda bulunan](../../role-based-access-control/built-in-roles.md#contributor) üyesi olmalısınız.

**Çözüm:** Ölçümleri keşfettiğiniz kaynak için yeterli izinlere sahip olduğundan emin olun.

### <a name="your-resource-didnt-emit-metrics-during-the-selected-time-range"></a>Kaynağınız seçilen zaman aralığı boyunca ölçüm göstermedi

Bazı kaynaklar ölçümlerini sürekli göstermez. Örneğin Azure durdurulan sanal makineler için ölçüm toplamayacaktır. Diğer kaynaklar ancak bazı koşullar oluştuğunda ölçümlerini gösterebilir. Örneğin işlemin işleme süresini gösteren ölçüm için en az bir işlem gerekir. Seçilen zaman aralığı içinde işlem yoksa grafik doğal olarak boş olacaktır. Bunun yanı sıra Azure'daki ölçümlerin çoğu her dakika toplansa da, bazıları daha seyrek toplanır. İncelemeye çalıştığınız ölçüm hakkındaki diğer ayrıntıları almak için ölçüm belgelerine bakın.

**Çözüm:** Grafiğin saatini daha geniş bir aralıkla değiştirin. "Son 30 gün"ten daha büyük bir zaman parçalılık (veya "Otomatik zaman tanecikli" seçeneğine güvenerek) başlayabilirsiniz.

### <a name="you-picked-a-time-range-greater-than-30-days"></a>30 günden uzun bir zaman aralığı seçtiniz

[Azure'daki ölçümlerin çoğu 93 gün boyunca depolanır](data-platform-metrics.md#retention-of-metrics). Bununla birlikte tek grafikte en çok 30 günlük verileri sorgulayabilirsiniz. Bu sınırlama [günlük tabanlı ölçümler](../app/pre-aggregated-metrics-log-metrics.md#log-based-metrics) için geçerli değildir.

**Çözüm:** Boş bir grafik görürseniz veya grafiğiniz metrik verilerin yalnızca bir kısmını görüntülerse, zaman seçicideki başlangıç ve bitiş tarihleri arasındaki farkın 30 günlük aralığı aşmadığını doğrulayın.

### <a name="all-metric-values-were-outside-of-the-locked-y-axis-range"></a>Tüm ölçüm değerleri kilitli y ekseni aralığının dışındaydı

[Grafiğin y ekseninin sınırlarını kilitleyerek](metrics-charts.md#lock-boundaries-of-chart-y-axis) istemeden grafik görüntüleme alanının grafik çizgisini göstermemesini sağlayabilirsiniz. Örneğin y ekseni %0 ile %50 arasına kilitlendiyse ve ölçümün %100 olan bir sabit değeri varsa, çizgi her zaman görünür alanın dışında işlenir ve grafik boş görünür.

**Çözüm:** Grafiğin y ekseni sınırlarının metrik değerlerin aralığının dışında kilitli olmadığını doğrulayın. Y ekseni sınırları kilitlendiyse, ölçüm değerlerinin grafik aralığının dışında kalmaması için bunları geçici olarak sıfırlamak isteyebilirsiniz. **Sum**, **min** ve **max** toplaması olan grafiklerde y ekseni aralığının otomatik ayrıntı düzeyiyle kilitlenmesi önerilmez çünkü tarayıcı penceresi yeniden boyutlandırıldığında veya bir ekran çözünürlüğünden diğerine geçildiğinde bunların değerleri ayrıntı düzeyiyle birlikte değişir. Ayrıntı düzeyinde geçiş yapmak grafiğinizin görüntü alanını boş bırakabilir.

### <a name="you-are-looking-at-a-guest-os-metric-but-didnt-enable-azure-diagnostic-extension"></a>Konuk İşletim Sistemi ölçümüne bakıyorsunuz, ancak Azure Tanı lama Uzantısı'nı etkinleştirmedin

**Konuk İşletim Sistemi** ölçümleri koleksiyonu Azure Tanılama Uzantısı'nın yapılandırılmasını veya kaynağınız için **Tanılama Ayarları** paneli kullanılarak etkinleştirilmesini gerektirir.

**Çözüm:** Azure Tanı Uzantısı etkinse ancak ölçümlerinizi hala göremiyorsanız, [Azure TanıLama Uzantısı sorun giderme kılavuzunda](diagnostics-extension-troubleshooting.md#metric-data-doesnt-appear-in-the-azure-portal)özetlenen adımları izleyin. [Konuk İşletim Sistemi ad alanını ve ölçümlerini seçemez](metrics-troubleshoot.md#cannot-pick-guest-os-namespace-and-metrics) ler için sorun giderme adımlarını da görün

## <a name="error-retrieving-data-message-on-dashboard"></a>Panoda "hata alma verisi" iletisi

Panonuz daha sonra kullanım dışı bırakılan ve Azure'dan kaldırılan bir ölçümle oluşturulduğunda bu hata oluşabilir. Durumun bu olduğunu doğrulamak için kaynağınızın **Ölçümler** sekmesini açın ve ölçüm seçicide kullanılabilir ölçümleri gözden geçirin. Ölçüm gösterilmiyorsa Azure'dan kaldırılmış demektir. Genellikle bir ölçüm kullanım dışı bırakıldığında kaynak durumuyla ilgili benzer bir perspektif sağlayan yeni ve daha iyi bir ölçüm vardır.

**Çözüm:** Panodaki grafiğiniz için alternatif bir metrik seçerek başarısız döşemeyi güncelleştirin. [Azure hizmetleri için kullanılabilir ölçümlerin listesini gözden geçirebilirsiniz](metrics-supported.md).

## <a name="chart-shows-dashed-line"></a>Grafik kesik çizgi gösterir

Azure ölçüm grafikleri, bilinen iki zaman greni veri noktası arasında eksik bir değer ("null value" olarak da bilinir) olduğunu belirtmek için kesik çizgi stilini kullanır. Örneğin, zaman seçicide "1 dakika" zaman parçalı olarak seçtiyseniz, ancak metrik 07:26, 07:27, 07:29 ve 07:30 'da raporlandıysa (ikinci ve üçüncü veri noktaları arasında bir dakika boşluk not edin), kesik li bir çizgi 07:27 ve 07:29'u bağlayacak ve katı bir çizgi bağlanır diğer tüm veri noktaları. Kesik li çizgi, metrik **sayım** ve **toplam** toplamayı kullandığında sıfıra düşer. **Avg,** **min** veya **max** toplamalar için, kesik çizgi bilinen en yakın iki veri noktasını bağlar. Ayrıca grafiğin en sağ veya en sol ucunda veri eksik olduğunda, kesikli çizgi eksik veri noktasının yönünde genişletilir.
  ![ölçüm resmi](./media/metrics-troubleshoot/missing-data-point-line-chart.png)

**Çözüm:** Bu davranış tasarım gereğidir. Eksik veri noktalarını belirlemek için kullanışlıdır. Çizgi grafiği, yüksek yoğunluklu ölçümlerin eğilimlerini görselleştirmek için üstün bir seçimdir, ancak özellikle zaman tanesiyle değerleri birleştirmek önemli olduğunda, seyrek değerlere sahip ölçümler için yorumlamak zor olabilir. Kesikli çizgi bu grafiklerin okunmasını kolaylaştırabilir ama grafiğiniz yine de belirsiz görünüyorsa ölçümlerinizi farklı bir grafik türünde görüntülemeyi göz önünde bulundurun. Örneğin, aynı metrik için dağınık bir çizim grafiği, her zaman tanesini, değer olduğunda yalnızca bir noktayı görselleştirerek ve ![değer eksik olduğunda veri noktasını tamamen atlayarak açıkça gösterir: metrik görüntü](./media/metrics-troubleshoot/missing-data-point-scatter-chart.png)

   > [!NOTE]
   > Ölçümünüz için yine de çizgi grafiği tercih ediyorsanız, fareyi grafiğin üzerinde hareket ettirmek zaman dilimini değerlendirmenize yardımcı olabilir çünkü fare işaretçisinin bulunduğu konumdaki veri noktası vurgulanır.

## <a name="chart-shows-unexpected-drop-in-values"></a>Grafik, değerlerde beklenmeyen düşüşü gösterir

Çoğu durumda ölçüm değerlerinde algılanan düşüş grafikte gösterilen verilerin yanlış anlaşılmasından kaynaklanır. Son ölçüm verileri Azure tarafından henüz alınmadığı veya işlenmediği için grafikte en yakın dakikalar gösterildiğinde toplamlardaki veya sayılardaki bir düşüş sizi yanıltabilir. Hizmete bağlı olarak ölçümlerin işlenmesindeki gecikme süresi birkaç dakikalık bir süre olabilir. 1 veya 5 dakikalık parçalı bir son zaman aralığını gösteren grafikler için, son birkaç dakikadaki değer ![düşüşü daha belirgin hale gelir: metrik görüntü](./media/metrics-troubleshoot/drop-in-values.png)

**Çözüm:** Bu davranış tasarım gereğidir. Veriler *kısmi* veya *eksik* bile olsa, verileri aldığımız anda göstermenin yararlı olduğuna inanıyoruz. Bu sayede önemli sonuçlara daha çabuk varabilir ve araştırmayı hemen başlatabilirsiniz. Örneğin, hata sayısını gösteren bir ölçüm için kısmı X değerinin gösterilmesi, belirli bir dakikada en az X hata olduğunu anlamanızı sağlar. Söz konusu dakikada gerçekleşen tam hata sayısını (üstelik bu sayı o kadar da önemli olmayabilir) görmek için beklemek yerine sorunu araştırmaya hemen başlayabilirsiniz. Veri kümesinin tamamını aldığımızda grafik güncelleştirilir ama o zaman da daha yakın dakikalarda elde edilen yeni eksik veri noktaları gösterilebilir.

## <a name="cannot-pick-guest-os-namespace-and-metrics"></a>Konuk İşletim Sistemi ad alanı ve ölçümleri seçemez

Sanal makineler ve sanal makine ölçek kümeleri iki ölçüm kategorisine sahiptir: Azure barındırma ortamı tarafından toplanan **Sanal Makine Ana Bilgisayar** ölçümleri ve sanal makinelerinizde çalışan izleme [aracısı](agents-overview.md) tarafından toplanan **Konuk İşLet (klasik)** ölçümleri. İzleme aracısını, [Azure Tanılama Uzantısı](diagnostics-extension-overview.md)'nı etkinleştirerek yüklersiniz.

Varsayılan olarak Konuk İşletim Sistemi ölçümleri, kaynağınızın **Tanılama ayarları** sekmesinde seçtiğiniz Azure Depolama hesabında depolanır. Konuk İşletim Sistemi ölçümleri toplanmıyorsa veya ölçüm gezgini bunlara erişemiyorsa yalnızca **Sanal Makine Konağı** ölçüm ad alanını görürsünüz:

![ölçüm resmi](./media/metrics-troubleshoot/cannot-pick-guest-os-namespace.png)

**Çözüm:** **Konuk İşletim Sistemi (klasik)** ad alanı ve ölçümleri ölçümler explorer'da görmüyorsanız:

1. [Azure Tanılama Uzantısı](diagnostics-extension-overview.md)'nın etkinleştirildiğini ve ölçümleri toplamak için yapılandırıldığını onaylayın.
    > [!WARNING]
    > **Konuk İşletim Sistemi**'ni depolama hesabına göndermek için [Log Analytics aracısını](agents-overview.md#log-analytics-agent) (Microsoft İzleyici Aracısı veya "MMA" olarak da adlandırılır) kullanamazsınız.

1. **Microsoft.Insights** kaynak sağlayıcısının [aboneliğiniz için kayıtlı](metrics-troubleshoot.md#microsoftinsights-resource-provider-isnt-registered-for-your-subscription)olduğundan emin olun.

1. Depolama hesabının güvenlik duvarı tarafından korunmadığını doğrulayın. Azure portalının ölçüm verilerini almak ve grafikleri çizmek için depolama hesabına erişmesi gerekiyor.

1. Depolama hesabına ölçüm akışının sağlandığını doğrulamak için [Azure depolama gezginini](https://azure.microsoft.com/features/storage-explorer/) kullanın. Ölçümler toplanmıyorsa [Azure Tanılama Uzantısı sorun giderme kılavuzunu](diagnostics-extension-troubleshooting.md#metric-data-doesnt-appear-in-the-azure-portal) izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Metric Explorer ile başlama hakkında bilgi edinin](metrics-getting-started.md)
* [Metric Explorer'ın gelişmiş özellikleri hakkında bilgi edinin](metrics-charts.md)
* [Azure hizmetleri için kullanılabilir ölçümlerin listesine bakın](metrics-supported.md)
* [Yapılandırılmış grafik örneklerine bakın](metric-chart-samples.md)
