---
title: Azure Application ınsights'ı uygulama verileri görüntüleme | Microsoft Docs
description: Performans sorunlarını tanılamak ve Application Insights ile izlenen uygulamanızla kullanıcıların ne yaptığını anlamak için Application Insights Bağlayıcısı çözümü kullanabilirsiniz.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/13/2019
ms.openlocfilehash: c143d8aa24d3479f4619ea2c220d4a0c593f9cb1
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77665174"
---
# <a name="application-insights-connector-management-solution-deprecated"></a>Application Insights Bağlayıcısı Management çözümü (kullanım dışı)

![Application Insights simgesi](./media/app-insights-connector/app-insights-connector-symbol.png)

>[!NOTE]
> [Çapraz kaynak sorguları](../../azure-monitor/log-query/cross-workspace-query.md)desteğiyle Application Insights Bağlayıcısı yönetim çözümü artık gerekli değildir. Azure ticari bulut için 15 Ocak 2019 tarihinde kullanımdan kaldırılan OMS portalı ile birlikte kullanımdan kaldırılmıştır ve Azure Marketi 'nden kaldırılmıştır. Azure ABD Kamu Bulutu için 30 Mart 2019 ' de kullanımdan kaldırılacaktır.
>
>Mevcut bağlantılar 30 Haziran 2019 ' e kadar çalışmaya devam edecektir.  OMS portalının kullanımdan kaldırılması sayesinde, mevcut bağlantıları portaldan yapılandırmanın ve kaldırmanın bir yolu yoktur. Mevcut bağlantıları kaldırmak için PowerShell 'i kullanma konusunda bir betik için aşağıdaki [PowerShell ile bağlayıcıyı kaldırma](#removing-the-connector-with-powershell) bölümüne bakın.
>
>Birden çok uygulama için Application Insights günlük verilerini sorgulama konusunda rehberlik için bkz. [birden çok Azure izleyicisini Application Insights kaynağı](../log-query/unify-app-resource-data.md)birleştirme. OMS portalının kullanımdan kaldırılması hakkında daha fazla bilgi için bkz. [OMS portalı Azure 'a taşınıyor](../../azure-monitor/platform/oms-portal-transition.md).
>
> 

Uygulama öngörüleri Bağlayıcısı çözümü, performans sorunlarını tanılamanıza ve [Application Insights](../../azure-monitor/app/app-insights-overview.md)ile izlendiğinde hangi kullanıcıların uygulamanızla ne yaptığını anlamanıza yardımcı olur. Log Analytics'te Application Insights'ta geliştiricilerin gördüğü aynı uygulama telemetrisini görünümlerini kullanılabilir. Ancak, Application Insights uygulamalarınızı Log Analytics ile tümleştirdiğinizde, uygulamalarınızın görünürlüğünü işlemi ve uygulama verilerini tek bir yerde sağlayarak artar. Aynı görünümleri olan, uygulama geliştiricilere işbirliği yapmasına yardımcı olur. Sık kullanılan görünümler, algılamak ve hem uygulama hem de platformu sorunları çözmek için gereken süreyi azaltmaya yardımcı olabilir.

Çözüm kullandığınızda şunları yapabilirsiniz:

- Farklı Azure aboneliklerinde olduklarında bile tüm Application Insights uygulamalarınızı tek bir yerde görüntüleyin.
- Uygulama verileri altyapı verilerle ilişkilendirmek
- Günlük araması'nda Perspektifler ile uygulama verileri Görselleştirme
- Application ınsights'ı uygulamanıza Azure portalında log Analytics verilerinden Özet


[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="connected-sources"></a>Bağlı kaynaklar

Çoğu diğer Log Analytics çözümleri, aracıları tarafından için Application Insights Bağlayıcısı verileri toplanmaz. Çözüm tarafından kullanılan tüm verileri doğrudan Azure gelir.

| Bağlı Kaynak | Destekleniyor | Açıklama |
| --- | --- | --- |
| [Windows aracıları](../../azure-monitor/platform/agent-windows.md) | Hayır | Çözüm, Windows aracılarından bilgi toplamaz. |
| [Linux aracıları](../../azure-monitor/learn/quick-collect-linux-computer.md) | Hayır | Çözüm, Linux aracılarından bilgi toplamaz. |
| [SCOM yönetim grubu](../../azure-monitor/platform/om-agents.md) | Hayır | Bir bağlı SCOM yönetim grubundaki aracılardan çözüm herhangi bir bilgi toplamaz. |
| [Azure depolama hesabı](collect-azure-metrics-logs.md) | Hayır | Çözüm, Azure depolama biriminden bilgilerin toplanması yapar. |

## <a name="prerequisites"></a>Önkoşullar

- Application Insights Bağlayıcısı bilgilere erişmek için bir Azure aboneliğinizin olması gerekir
- Yapılandırılan en az bir Application Insights kaynağı olmalıdır.
- Sahibi veya katkıda bulunan Application Insights kaynağına ait olmalıdır.

## <a name="configuration"></a>Yapılandırma

1. Azure Web Apps Analytics çözümünü [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AppInsights?tab=Overview) 'nden veya [Çözüm Galerisi Log Analytics çözümleri ekleme](../../azure-monitor/insights/solutions.md)bölümünde açıklanan işlemi kullanarak etkinleştirin.
2. [Azure portala](https://portal.azure.com) gidin. Application Insights açmak için **tüm hizmetler** ' i seçin. Ardından Application ınsights'ı arayın. 
3. **Abonelikler**' in altında Application Insights kaynaklara sahip bir abonelik seçin ve ardından **ad**' ın altında bir veya daha fazla uygulama seçin.
4. **Kaydet**’e tıklayın.

Yaklaşık 30 dakika içinde veriler kullanılabilir duruma gelir ve Application Insights kutucuğunu aşağıdaki görüntü gibi verilerle güncelleştirilir:

![Application Insights kutucuğunu](./media/app-insights-connector/app-insights-tile.png)

Akılda tutulması gereken diğer noktalar:

- Bu gibi durumlarda, Application Insights uygulamaları yalnızca bir Log Analytics çalışma alanına bağlayabilirsiniz.
- [Temel veya kurumsal Application Insights kaynaklarını](https://azure.microsoft.com/pricing/details/application-insights) yalnızca Log Analytics bağlayabilirsiniz. Bununla birlikte, ücretsiz katmanda Log analytics'in kullanabilirsiniz.

## <a name="management-packs"></a>Yönetim paketleri

Bu çözüm, bağlı yönetim gruplarında yönetim paketleri yüklemez.

## <a name="use-the-solution"></a>Çözüm kullanın

Aşağıdaki bölümlerde, görüntülemek ve uygulamalarınızdan verilerle etkileşimde bulunmak için Application Insights panosunda gösterilen dikey pencereleri nasıl kullanabileceğiniz açıklanmaktadır.

### <a name="view-application-insights-connector-information"></a>Application Insights Bağlayıcısı bilgilerini görüntüle

Aşağıdaki dikey pencereleri görmek için **Application Insights** panosunu açmak üzere **Application Insights** kutucuğuna tıklayın.

![Application Insights Panosu](./media/app-insights-connector/app-insights-dash01.png)

![Application Insights Panosu](./media/app-insights-connector/app-insights-dash02.png)

Pano tablosunda gösterilen dikey pencereleri içerir. Her dikey pencerede, dikey pencerenin belirtilen kapsam ve zaman aralığına yönelik ölçütleriyle eşleşen en fazla 10 öğe listelenir. Dikey pencerenin en altında bulunan **Tümünü göster** ' e tıkladığınızda veya dikey pencere başlığına tıkladığınızda tüm kayıtları döndüren bir günlük araması çalıştırabilirsiniz.


| **Sütunuyla** | **Açıklama** |
| --- | --- |
| Uygulamalar - uygulama sayısı | Uygulama kaynaklarında uygulama sayısını gösterir. Ayrıca uygulama adlarını listeler ve her uygulama kayıt sayısını verir. <code>ApplicationInsights &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName</code> günlük araması çalıştırmak için sayıya tıklayın <br><br>  Uygulama kayıtları başına konak, telemetri türüne göre kayıtlar ve tüm verileri (son gününe göre) türüne göre gösteren uygulama için bir günlük araması gerçekleştirmek için bir uygulama adına tıklayın. |
| Veri hacmi – veri gönderen konaklar | Veri gönderen barındıran bilgisayar sayısını gösterir. Ayrıca, ana bilgisayar ve her konak için kayıt sayısı listelenir. <code>ApplicationInsights &#124; summarize AggregatedValue = sum(SampledCount) by Host</code> günlük araması çalıştırmak için sayıya tıklayın <br><br> Uygulama kayıtları başına konak, telemetri türüne göre kayıtlar ve tüm verileri (son gününe göre) türüne göre gösteren konak için bir günlük araması gerçekleştirmek için bir bilgisayar adına tıklayın. |
| Kullanılabilirlik – Web testi sonuçları | İçin web test sonuçları, Geçti veya başarısız gösteren bir halka grafik gösterir. <code>ApplicationInsights &#124; where TelemetryType == "Availability" &#124; summarize AggregatedValue = sum(SampledCount) by AvailabilityResult</code> günlük araması çalıştırmak için grafiğe tıklayın <br><br> Sonuçları geçişleri ve tüm testler için hataları sayısını gösterir. Bu, son dakika için trafiği ile tüm Web uygulamaları gösterir. Başarısız web testi ayrıntılarını gösteren bir günlük araması'nı görüntülemek için bir uygulama adına tıklayın. |
| Sunucu istekleri – saat başına istek | Çeşitli uygulamalar için saat başına sunucu isteklerinin bir çizgi grafik gösterir. Zaman noktasına yönelik istekleri almaya ilk 3 uygulama görmek için grafikteki bir satır üzerine gelin. Ayrıca istek ve istek sayısı, seçilen süre için alma uygulamaların bir listesini gösterir. <br><br>Çeşitli uygulamalar için saat başına sunucu isteklerinin daha ayrıntılı bir çizgi grafiğini gösteren <code>ApplicationInsights &#124; where TelemetryType == "Request" &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName, bin(TimeGenerated, 1h)</code> için bir günlük araması çalıştırmak için grafiğe tıklayın. <br><br> İstek listesini, zaman içindeki isteklerin ve istek süresi ve istek yanıt kodlarının bir listesini gösteren <code>ApplicationInsights &#124; where ApplicationName == "yourapplicationname" and TelemetryType == "Request" and iff(isnotnull(toint(RequestSuccess)), RequestSuccess == false, RequestSuccess == "false") == true</code> bir günlük araması çalıştırmak için listedeki bir uygulamaya tıklayın.   |
| Hataları – saat başına başarısız istek | Saat başına başarısız uygulama isteklerinin bir çizgi grafik gösterir. Başarısız istekler için bir nokta ile ilk 3 uygulamalarını görmek için grafik üzerinde gezdirin. Ayrıca, her biri için başarısız istek sayısı ile uygulamaların bir listesini gösterir. Başarısız uygulama isteklerinin daha ayrıntılı bir çizgi grafiğini gösteren <code>ApplicationInsights &#124; where TelemetryType == "Request" and iff(isnotnull(toint(RequestSuccess)), RequestSuccess == false, RequestSuccess == "false") == true &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName, bin(TimeGenerated, 1h)</code> için bir günlük araması çalıştırmak için grafiğe tıklayın. <br><br>Başarısız istekleri gösteren <code>ApplicationInsights &#124; where ApplicationName == "yourapplicationname" and TelemetryType == "Request" and iff(isnotnull(toint(RequestSuccess)), RequestSuccess == false, RequestSuccess == "false") == true</code>, zaman içinde başarısız isteklere yönelik grafikleri ve başarısız istek yanıt kodlarının listesini gösteren için bir öğe seçin. |
| Özel durumlar: özel durumlar / saat | Saat başına özel durum içeren bir çizgi grafik gösterir. Bir nokta için özel durumlar üst 3 uygulamaları görmek için grafik üzerinde gezdirin. Ayrıca, her biri için özel durumların sayısı ile uygulamaların bir listesini gösterir. Özel durumların daha ayrıntılı bir bağlantı grafiğini gösteren <code>ApplicationInsights &#124; where TelemetryType == "Exception" &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName, bin(TimeGenerated, 1h)</code> için bir günlük araması çalıştırmak için grafiğe tıklayın. <br><br>Özel durumların bir listesini, zaman ve başarısız isteklerde özel durumlar için grafikleri ve özel durum türlerinin bir listesini gösteren <code>ApplicationInsights &#124; where ApplicationName == "yourapplicationname" and TelemetryType == "Exception"</code> bir günlük araması çalıştırmak için listedeki bir öğeye tıklayın.  |

### <a name="view-the-application-insights-perspective-with-log-search"></a>Günlük araması ile Application Insights perspektif görüntüleyin

Pano herhangi bir öğeye tıkladığınızda, arama gösterilen bir Application Insights perspektif bakın. Perspektif seçilen telemetri türüne göre genişletilmiş bir görselleştirme sağlar. Bu nedenle, farklı telemetri türleri için görselleştirme içeriğini değiştirir.

Uygulamalar dikey penceresinde herhangi bir yere tıkladığınızda varsayılan **uygulamaların** perspektifini görürsünüz.

![Application Insights uygulamaları perspektifi](./media/app-insights-connector/applications-blade-drill-search.png)

Seçtiğiniz uygulamanın genel bir bakış açısından gösterir.

**Kullanılabilirlik** dikey penceresinde Web testi sonuçlarını ve ilgili başarısız istekleri görebileceğiniz farklı bir perspektif görünümü gösterilmektedir.

![Application Insights kullanılabilirlik perspektifi](./media/app-insights-connector/availability-blade-drill-search.png)

**Sunucu istekleri** veya **arızaların** dikey penceresinde herhangi bir yere tıkladığınızda, perspektif bileşenleri, isteklerle ilgili bir görselleştirme sağlamak üzere değişir.

![Application Insights hataları dikey penceresi](./media/app-insights-connector/server-requests-failures-drill-search.png)

**Özel durumlar** dikey penceresinde herhangi bir yere tıkladığınızda, özel durumlara uyarlanmış bir görselleştirme görürsünüz.

![Application Insights özel durumları dikey penceresi](./media/app-insights-connector/exceptions-blade-drill-search.png)

Tek bir **Application Insights Bağlayıcısı** panosuna tıklamasından bağımsız olarak, **arama** sayfasının kendisi içinde Application Insights verileri döndüren herhangi bir sorgu Application Insights perspektifini gösterir. Örneğin, Application Insights verileri görüntülüyorsanız, bir **&#42;** sorgu aşağıdaki görüntüde olduğu gibi perspektif sekmesini de gösterir:

![Application Insights](./media/app-insights-connector/app-insights-search.png)

Perspektif bileşenleri arama sorgusu bağlı olarak güncelleştirilir. Başka bir deyişle, verileri görme olanağı sunan herhangi bir arama alanını kullanarak sonuçları filtreleyebilirsiniz:

- Tüm uygulamalarınız
- Tek bir seçili uygulama
- Uygulama grubu

### <a name="pivot-to-an-app-in-the-azure-portal"></a>Azure Portalı'nda bir uygulama için Özet

Application Insights Bağlayıcısı Blade *, Azure Portal kullandığınızda*seçili Application Insights uygulamasına Özet uygulamanızı sağlamak üzere tasarlanmıştır. Çözüm, sorunu giderme yardımcı olan üst düzey bir izleme platformu olarak kullanabilirsiniz. Olası bir soruna bağlı uygulamalarınızı hiçbirinde gördüğünüzde, ya da ayrıntıya, Log Analytics arama yapabilir veya doğrudan Application Insights uygulama Özet.

Özet 'e, her satırın sonunda görüntülenen üç noktaya ( **...** ) tıklayın ve **Application Insights aç**' ı seçin.

>[!NOTE]
>**Application Insights Içinde aç** Azure portal kullanılamıyor.

![Application Insights'ta aç](./media/app-insights-connector/open-in-app-insights.png)

### <a name="sample-corrected-data"></a>Veri örnek düzeltildi

Application Insights telemetri trafiğini azaltmaya yardımcı olmak için *[örnekleme düzeltmesi](../../azure-monitor/app/sampling.md)* sağlar. Application Insights uygulamanızı örnekleme etkinleştirdiğinizde, azaltılmış bir Application ınsights ve Log Analytics içinde depolanan girdi sayısı alın. **Application Insights Bağlayıcısı** sayfasında ve perspektiflerde veri tutarlılığı korunurken, özel sorgularınız için örneklenmiş verileri el ile düzeltmeniz gerekir.

Örnekleme düzeltme günlük arama sorgusu içindeki bir örnek aşağıda verilmiştir:

```
ApplicationInsights | summarize AggregatedValue = sum(SampledCount) by TelemetryType
```

**Örneklenmiş sayı** alanı tüm girdilerde bulunur ve girişin temsil ettiği veri noktalarının sayısını gösterir. Application Insights uygulamanız için örnekleme açarsanız, **örneklenmiş sayı** 1 ' den büyük olur. Uygulamanızın oluşturduğu girişlerin gerçek sayısını saymak için **örneklenmiş sayı** alanlarını toplayın.

Örnekleme, uygulamanızı oluşturan girişleri yalnızca toplam sayısını etkiler. Bu alanlar, temsil edilen girdilerin ortalamasını gösterdiği için **RequestDuration** veya **kullanılabilirlik süresi** gibi ölçüm alanları için örnekleme düzeltmeniz gerekmez.

## <a name="input-data"></a>Giriş verileri

Çözüme bağlı Application Insights uygulamalarınızdan aşağıdaki veri telemetri türlerini alır:

- Kullanılabilirlik
- Özel durumlar
- İstekler
- Sayfa görünümleri – sayfa görüntülemesi almak bir çalışma alanınız için bu bilgileri toplamak için uygulamalarınızı yapılandırmanız gerekir. Daha fazla bilgi için bkz. [PageViews](../../azure-monitor/app/api-custom-events-metrics.md#page-views).
- Özel olaylar – özel olaylarını almak bir çalışma alanınız için bu bilgileri toplamak için uygulamalarınızı yapılandırmanız gerekir. Daha fazla bilgi için bkz. [Trackevent](../../azure-monitor/app/api-custom-events-metrics.md#trackevent).

Uygun olduğunda verilerini Application ınsights'tan Log Analytics tarafından alınır.

## <a name="output-data"></a>Çıktı verileri

Her giriş verisi türü için, *ApplicationInsights* *türünde* bir kayıt oluşturulur. Applicationınsights kayıtları, aşağıdaki bölümlerde gösterilen özelliklere sahiptir:

### <a name="generic-fields"></a>Genel alanlar

| Özellik | Açıklama |
| --- | --- |
| Tür | ApplicationInsights |
| ClientIP |   |
| TimeGenerated | Kaydın zamanı |
| ApplicationId | Application Insights uygulama izleme anahtarı |
| ApplicationName | Application ınsights'ı adını uygulama |
| Roleınstance | Sunucu ana bilgisayar kimliği |
| DeviceType | İstemci cihazı |
| ScreenResolution |   |
| Kıta | İsteğin geldiği kıta |
| Ülke | İsteğin kaynaklandığı ülke/bölge |
| İl | Bölge, eyalet veya yerel ayar isteği geldiği |
| Şehir | Şehir veya isteğin geldiği Şehir |
| isSynthetic | İstek bir kullanıcı veya otomatikleştirilmiş bir yöntem tarafından oluşturulup oluşturulmadığını belirtir. True = otomatik Yöntem veya yanlış = Kullanıcı tarafından oluşturulan |
| SamplingRate | Portala gönderilen SDK'sı tarafından oluşturulan telemetri yüzdesi. 0,0-100.0 aralığı. |
| SampledCount | 100/(SamplingRate). Örneğin, 4 =&gt; %25 |
| Isauthenticated durumunda olmasını gerektirir | True veya false |
| OperationID | Aynı işlem kimliği ilişkili öğeleri portalda gösterilen olan öğeler. Genellikle istek kimliği |
| ParentOperationID | Üst işlem kimliği |
| için abonelik sınırlarını aştıysanız Hizmet Azaltma gerçekleşir |   |
| oturum kimliği | İsteğin oluşturulduğu oturum benzersiz olarak tanımlayan GUID |
| SourceSystem | ApplicationInsights |

### <a name="availability-specific-fields"></a>Kullanılabilirlik özgü alanlar

| Özellik | Açıklama |
| --- | --- |
| TelemetryType | Kullanılabilirlik |
| AvailabilityTestName | Web testinin adı |
| AvailabilityRunLocation | Http isteğinin coğrafi kaynak |
| AvailabilityResult | Web testi başarı sonucunu gösterir |
| AvailabilityMessage | Web testine bağlı ileti |
| AvailabilityCount | 100 /(Sampling Rate). Örneğin, 4 =&gt; %25 |
| DataSizeMetricValue | 1.0 ya da 0.0 |
| DataSizeMetricCount | 100 /(Sampling Rate). Örneğin, 4 =&gt; %25 |
| AvailabilityDuration | Web test süresinin milisaniye cinsinden süre |
| AvailabilityDurationCount | 100 /(Sampling Rate). Örneğin, 4 =&gt; %25 |
| AvailabilityValue |   |
| AvailabilityMetricCount |   |
| AvailabilityTestId | Web testi için benzersiz GUID |
| AvailabilityTimestamp | Kullanılabilirlik testi, tam zaman damgası |
| AvailabilityDurationMin | Örneklenen kayıtlar için bu alan gösterilen veri noktaları için en düşük web test süresi (milisaniye cinsinden) gösterir. |
| AvailabilityDurationMax | Örneklenen kayıtlar için bu alan gösterilen veri noktaları için en fazla web test süresi (milisaniye cinsinden) gösterir. |
| AvailabilityDurationStdDev | Örneklenen kayıtlar için bu alan gösterilen veri noktaları için tüm web test süre (milisaniye) arasında standart sapma gösterir. |
| AvailabilityMin |   |
| AvailabilityMax |   |
| AvailabilityStdDev | &nbsp;  |

### <a name="exception-specific-fields"></a>Özel durum özgü alanlar

| Tür | ApplicationInsights |
| --- | --- |
| TelemetryType | Özel durum |
| ExceptionType | Özel durumun türü |
| ExceptionMethod | Özel durum oluşturan yöntemi |
| ExceptionAssembly | Ortak anahtar belirteci yanı sıra framework ve sürümünün derleme içerir |
| ExceptionGroup | Özel durumun türü |
| ExceptionHandledAt | Özel durumun işlenip düzeyini gösterir |
| ExceptionCount | 100 /(Sampling Rate). Örneğin, 4 =&gt; %25 |
| ExceptionMessage | Özel durum iletisi |
| ExceptionStack | Özel durumun tam bir yığın |
| ExceptionHasStack | Özel durum yığın varsa TRUE |



### <a name="request-specific-fields"></a>İstek özgü alanlar

| Özellik | Açıklama |
| --- | --- |
| Tür | ApplicationInsights |
| TelemetryType | İstek |
| ResponseCode | İstemciye gönderilen HTTP yanıtı |
| RequestSuccess | Başarı veya başarısızlık gösterir. TRUE veya false. |
| RequestId | İstek benzersiz olarak tanımlanabilmesi için kimliği |
| RequestName | GET/POST + URL'si temeli |
| RequestDuration | İstek süresini saniye cinsinden süre |
| URL'si | Konak içermeden isteğin URL'si |
| Host | Web sunucusu ana bilgisayarı |
| URLBase | İstek tam URL'si |
| ApplicationProtocol | Uygulama tarafından kullanılan protokol türü |
| RequestCount | 100 /(Sampling Rate). Örneğin, 4 =&gt; %25 |
| RequestDurationCount | 100 /(Sampling Rate). Örneğin, 4 =&gt; %25 |
| RequestDurationMin | Örneklenen kayıt için bu alan için gösterilen veri noktalarının en az bir istek süresi (milisaniye cinsinden) gösterir. |
| RequestDurationMax | Örneklenen kayıtları için bu alan en fazla istek süresi (milisaniye) için gösterilen veri noktalarını gösterir. |
| RequestDurationStdDev | Örneklenen kayıtlar için tüm istek süre (milisaniye) arasında standart sapma Bu alan, gösterilen veri noktalarını gösterir. |

## <a name="sample-log-searches"></a>Örnek günlük aramaları

Bu çözüm panosunda gösterilen örnek günlük aramaları kümesi yok. Ancak, açıklama içeren örnek günlük arama sorguları, [görüntüleme Application Insights Bağlayıcısı bilgileri](#view-application-insights-connector-information) bölümünde gösterilir.

## <a name="removing-the-connector-with-powershell"></a>Bağlayıcı PowerShell ile kaldırılıyor
OMS portalının kullanımdan kaldırılması sayesinde, mevcut bağlantıları portaldan yapılandırmanın ve kaldırmanın bir yolu yoktur. Mevcut bağlantıları aşağıdaki PowerShell betiğinden kaldırabilirsiniz. Bu işlemi gerçekleştirmek için çalışma alanı ve Application Insights kaynak okuyucu sahibi veya katılımcısı olmanız gerekir.

```powershell
$Subscription_app = "App Subscription Name"
$ResourceGroup_app = "App ResourceGroup"
$Application = "Application Name"
$Subscription_workspace = "Workspace Subscription Name"
$ResourceGroup_workspace = "Workspace ResourceGroup"
$Workspace = "Workspace Name"

Connect-AzAccount
Set-AzContext -SubscriptionId $Subscription_app
$AIApp = Get-AzApplicationInsights -ResourceGroupName $ResourceGroup_app -Name $Application 
Set-AzContext -SubscriptionId $Subscription_workspace
Remove-AzOperationalInsightsDataSource -WorkspaceName $Workspace -ResourceGroupName $ResourceGroup_workspace -Name $AIApp.Id
```

Bir REST API çağrısını çağıran aşağıdaki PowerShell betiğini kullanarak uygulamaların listesini alabilirsiniz. 

```powershell
Connect-AzAccount
$Tenant = "TenantId"
$Subscription_workspace = "Workspace Subscription Name"
$ResourceGroup_workspace = "Workspace ResourceGroup"
$Workspace = "Workspace Name"
$AccessToken = "AAD Authentication Token" 

Set-AzContext -SubscriptionId $Subscription_workspace
$LAWorkspace = Get-AzOperationalInsightsWorkspace -ResourceGroupName $ResourceGroup_workspace -Name $Workspace

$Headers = @{
    "Authorization" = "Bearer $($AccessToken)"
    "x-ms-client-tenant-id" = $Tenant
}

$Connections = Invoke-RestMethod -Method "GET" -Uri "https://management.azure.com$($LAWorkspace.ResourceId)/dataSources/?%24filter=kind%20eq%20'ApplicationInsights'&api-version=2015-11-01-preview" -Headers $Headers
$ConnectionsJson = $Connections | ConvertTo-Json
```
Bu betik, Azure Active Directory karşı kimlik doğrulaması için bir taşıyıcı kimlik doğrulama belirteci gerektirir. Bu belirteci almanın bir yolu [REST API belgeleri sitesindeki](https://docs.microsoft.com/rest/api/loganalytics/datasources/createorupdate)bir makaleyi kullanmaktır. **Dene** ' ye tıklayın ve Azure aboneliğinizde oturum açın. Aşağıdaki görüntüde gösterildiği gibi, **Istek önizlemeden** taşıyıcı belirtecini kopyalayabilirsiniz.


![Taşıyıcı belirteci](media/app-insights-connector/bearer-token.png)


Ayrıca, bir günlük sorgusu kullanan uygulamaların bir listesini alabilirsiniz:

```Kusto
ApplicationInsights | summarize by ApplicationName
```

## <a name="next-steps"></a>Sonraki adımlar

- Application Insights uygulamalarınızın ayrıntılı bilgilerini görüntülemek için [günlük araması](../../azure-monitor/log-query/log-query-overview.md) 'nı kullanın.
