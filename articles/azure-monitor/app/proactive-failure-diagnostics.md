---
title: Uygulama Öngörüleri'nde Akıllı Algılama - arıza anomalileri | Microsoft Dokümanlar
description: Web uygulamanızda başarısız isteklerin hızında olağandışı değişiklikler eylediğiniz konusunda sizi uyarır ve tanılama analizi sağlar. Yapılandırma gerekmez.
ms.topic: conceptual
ms.date: 12/18/2018
ms.reviewer: yalavi
ms.openlocfilehash: a1bce3ab86748d8247a72da3bd70e0f2e8155dbf
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536820"
---
# <a name="smart-detection---failure-anomalies"></a>Akıllı Algılama - Arıza Anomalileri
[Uygulama Öngörüleri,](../../azure-monitor/app/app-insights-overview.md) web uygulamanız başarısız isteklerin hızında anormal bir artış görürse, sizi neredeyse gerçek zamanlı olarak otomatik olarak uyarır. Başarısız olduğu bildirilen HTTP istekleri veya bağımlılık çağrıları oranında olağandışı bir artış algılar. İstekler için, başarısız isteklerin genellikle 400 veya daha yüksek yanıt kodları vardır. Sorunu değerlendirmenize ve tanılamanıza yardımcı olmak için, uyarı ayrıntılarında hataların ve ilgili uygulama verilerinin özelliklerinin bir analizi sağlanır. Ayrıca daha fazla tanı için Application Insights portalına bağlantılar da vardır. Normal hata oranını tahmin etmek için makine öğrenimi algoritmaları kullandığından, özellik kurulum veya yapılandırma gerektirmez.

Bu özellik, bulutta veya kendi sunucularınızda barındırılan ve uygulama isteği veya bağımlılık verileri oluşturan herhangi bir web uygulaması için çalışır. Örneğin, [TrackRequest()](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest) veya [TrackDependency()](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency)çağıran bir alt rol varsa.

[Projeniz için Uygulama Öngörüleri'ni](../../azure-monitor/app/app-insights-overview.md)ayarladıktan sonra ve uygulamanız belirli bir minimum miktarda veri oluşturuyorsa, uygulamanızın normal davranışını öğrenmeniz ve uyarı göndermeden önce Akıllı Hata anormalliklerinin algılanması 24 saat sürer.

Örnek bir uyarı aşağıda veda edebilirsiniz:

[![](./media/proactive-failure-diagnostics/013.png "Sample smart detection alert showing cluster analysis around failure")](./media/proactive-failure-diagnostics/013.png#lightbox)

Uyarı ayrıntıları size şunları söyleyecektir:

* Normal uygulama davranışıyla karşılaştırıldığında hata oranı.
* Kaç kullanıcı etkilenir - bu yüzden ne kadar endişelenmeniz gerektiğini biliyorsunuz.
* Hatalarla ilişkili karakteristik bir desen. Bu örnekte, belirli bir yanıt kodu, istek adı (işlem) ve uygulama sürümü vardır. Bu, kodunuza bakmaya başlamanız için size hemen bir yol olur. Diğer olasılıklar belirli bir tarayıcı veya istemci işletim sistemi olabilir.
* Özel durum, günlük izlemeleri ve bağımlılık hatası (veritabanları veya diğer dış bileşenler) karakterize hataları ile ilişkili gibi görünüyor.
* Uygulama Öngörüleri'ndeki verilerdeki ilgili aramalara doğrudan bağlantılar.

## <a name="benefits-of-smart-detection"></a>Akıllı Algılamanın Faydaları
Sıradan [metrik uyarılar](../../azure-monitor/app/alerts.md) size bir sorun olabileceğini söyler. Ancak Smart Detection, aksi takdirde kendiniz yapmanız gereken analizleri gerçekleştirecek şekilde tanılama işini sizin için başlatır. Sonuçları düzgün bir şekilde paketleyip sorunun köküne hızlı bir şekilde girmenize yardımcı olur.

## <a name="how-it-works"></a>Nasıl çalışır?
Akıllı Algılama, uygulamanızdan alınan verileri ve özellikle de hata oranlarını izler. Bu kural, `Successful request` özelliğin yanlış olduğu istek sayısını ve `Successful call` özelliğin yanlış olduğu bağımlılık çağrılarının sayısını sayar. İstekler için, `Successful request == (resultCode < 400)` varsayılan olarak, (kendi [TrackRequest](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest) çağrılarınızı [filtrelemek](../../azure-monitor/app/api-filtering-sampling.md#filtering) veya oluşturmak için özel kod yazmadıkça). 

Uygulamanızın performansı tipik bir davranış deseni vardır. Bazı istekler veya bağımlılık çağrıları diğerlerinden daha başarısız lığa yatkın olacaktır; ve genel arıza oranı yük arttıkça artabilir. Akıllı Algılama bu anomalileri bulmak için makine öğrenimi kullanır.

Veriler web uygulamanızdan Uygulama Öngörüleri'ne girerken, Smart Detection mevcut davranışı son birkaç gün içinde görülen desenlerle karşılaştırır. Önceki performansla karşılaştırıldığında başarısızlık oranında anormal bir artış görülürse, bir çözümleme tetiklenir.

Bir çözümleme tetiklendiğinde, hizmet hataları karakterize değerleri bir desen tanımlamak için denemek için başarısız istek bir küme çözümleme gerçekleştirir. 

Yukarıdaki örnekte, çözümleme çoğu hataları belirli bir sonuç kodu, istek adı, Sunucu URL ana bilgisayar ve rol örneği hakkında olduğunu keşfetti. 

Hizmetiniz bu çağrılarla ayin yapıldığında, çözümleyici, tanımladığı kümedeki isteklerle ilişkili bir özel durum ve bağımlılık hatası ve bu isteklerle ilişkili izleme günlüklerinin bir örneği arar.

Elde edilen çözümleme, yapılandırmadığınız sürece uyarı olarak size gönderilir.

El [ile ayarladığınız uyarılar](../../azure-monitor/app/alerts.md)gibi, sorun giderilirse çözülebilecek olan ateşle uyaran uyarının durumunu inceleyebilirsiniz. Uygulama Öngörüleri kaynağınızın Uyarılar sayfasındaki uyarı kurallarını yapılandırın. Ancak diğer uyarıların aksine, Smart Detection'ı ayarlamanız veya yapılandırmanız gerekmez. İsterseniz, devre dışı kalabilir veya hedef e-posta adreslerini değiştirebilirsiniz.

### <a name="alert-logic-details"></a>Uyarı mantık ayrıntıları

Uyarılar, özel makine öğrenimi algoritmamız tarafından tetiklenir, böylece tam uygulama ayrıntılarını paylaşamayız. Bununla birlikte, bazen altta yatan mantığın nasıl çalıştığı hakkında daha fazla bilgi sahibi olmanız gerektiğini anlıyoruz. Bir uyarının tetiklenip tetiklenmediğini belirlemek için değerlendirilen birincil faktörler şunlardır: 

* 20 dakikalık bir yuvarlanma süresi penceresinde isteklerin/bağımlılıkların başarısızlık yüzdesinin analizi.
* Son 20 dakikanın başarısızlık yüzdesinin son 40 dakika ve son yedi gündeki oranile karşılaştırılması ve standart sapmanın X katını aşan önemli sapmalar aranması.
* Uygulamanın istek/bağımlılık hacmine bağlı olarak değişen minimum hata yüzdesi için uyarlanabilir bir sınır kullanılması.
* Sorun artık 8-24 saat boyunca algılanmamışsa, uyarı izleme koşuluotomatik olarak çözülebilecek bir mantık vardır.

## <a name="configure-alerts"></a>Uyarı yapılandırma

Akıllı Algılama uyarı kuralını portaldan veya Azure Kaynak Yöneticisi'ni kullanarak devre dışı kullanabilirsiniz[(bkz. şablon örneği).](https://docs.microsoft.com/azure/azure-monitor/app/proactive-arm-config)

Bu uyarı kuralı, e-posta ve webhook eylemleri içeren "Uygulama Öngörüleri Akıllı Algılama" adlı ilişkili bir [Eylem Grubu](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) ile oluşturulur ve uyarı çıktığında ek eylemleri tetiklemek için genişletilebilir.

> [!NOTE]
> Bu uyarı kuralından gönderilen e-posta bildirimleri artık varsayılan olarak aboneliğin İzleme Okuyucusu ve İzleme Katılımcısı rolleriyle ilişkili kullanıcılara gönderilir. Bu konuda daha fazla bilgi [burada](https://docs.microsoft.com/azure/azure-monitor/app/proactive-email-notification)bulabilirsiniz.
> Bu uyarı kuralından gönderilen [bildirimler, ortak uyarı şemasını](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema)izler.
>

Uyarılar sayfasını açın. Hata Anomalileri uyarı kuralları, el ile ayarladığınız tüm uyarılarla birlikte dahildir ve şu anda uyarı durumunda olup olmadığını görebilirsiniz.

[![](./media/proactive-failure-diagnostics/021.png "On the Application Insights resource page, click 'Alerts' tile, then 'Manage alert rules'")](./media/proactive-failure-diagnostics/021.png#lightbox)

Yapılandırmak için uyarıyı tıklatın.

[![](./media/proactive-failure-diagnostics/032.png "Rule configuration screen")](./media/proactive-failure-diagnostics/032.png#lightbox)

Bir Hata Anomalileri uyarı kuralını devre dışı kabileceğinize veya silebileceğinize, ancak aynı Application Insights kaynağında başka bir kural oluşturamadığınıza dikkat edin.

## <a name="example-of-failure-anomalies-alert-webhook-payload"></a>Arıza Anomalileri uyarı webhook yük örneği

```json
{
    "properties": {
        "essentials": {
            "severity": "Sev3",
            "signalType": "Log",
            "alertState": "New",
            "monitorCondition": "Resolved",
            "monitorService": "Smart Detector",
            "targetResource": "/subscriptions/4f9b81be-fa32-4f96-aeb3-fc5c3f678df9/resourcegroups/test-group/providers/microsoft.insights/components/test-rule",
            "targetResourceName": "test-rule",
            "targetResourceGroup": "test-group",
            "targetResourceType": "microsoft.insights/components",
            "sourceCreatedId": "1a0a5b6436a9b2a13377f5c89a3477855276f8208982e0f167697a2b45fcbb3e",
            "alertRule": "/subscriptions/4f9b81be-fa32-4f96-aeb3-fc5c3f678df9/resourcegroups/test-group/providers/microsoft.alertsmanagement/smartdetectoralertrules/failure anomalies - test-rule",
            "startDateTime": "2019-10-30T17:52:32.5802978Z",
            "lastModifiedDateTime": "2019-10-30T18:25:23.1072443Z",
            "monitorConditionResolvedDateTime": "2019-10-30T18:25:26.4440603Z",
            "lastModifiedUserName": "System",
            "actionStatus": {
                "isSuppressed": false
            },
            "description": "Failure Anomalies notifies you of an unusual rise in the rate of failed HTTP requests or dependency calls."
        },
        "context": {
            "DetectionSummary": "An abnormal rise in failed request rate",
            "FormattedOccurenceTime": "2019-10-30T17:50:00Z",
            "DetectedFailureRate": "50.0% (200/400 requests)",
            "NormalFailureRate": "0.0% (over the last 30 minutes)",
            "FailureRateChart": [["2019-10-30T05:20:00Z",
            0],
            ["2019-10-30T05:40:00Z",
            100],
            ["2019-10-30T06:00:00Z",
            0],
            ["2019-10-30T06:20:00Z",
            0],
            ["2019-10-30T06:40:00Z",
            100],
            ["2019-10-30T07:00:00Z",
            0],
            ["2019-10-30T07:20:00Z",
            0],
            ["2019-10-30T07:40:00Z",
            100],
            ["2019-10-30T08:00:00Z",
            0],
            ["2019-10-30T08:20:00Z",
            0],
            ["2019-10-30T08:40:00Z",
            100],
            ["2019-10-30T17:00:00Z",
            0],
            ["2019-10-30T17:20:00Z",
            0],
            ["2019-10-30T09:00:00Z",
            0],
            ["2019-10-30T09:20:00Z",
            0],
            ["2019-10-30T09:40:00Z",
            100],
            ["2019-10-30T10:00:00Z",
            0],
            ["2019-10-30T10:20:00Z",
            0],
            ["2019-10-30T10:40:00Z",
            100],
            ["2019-10-30T11:00:00Z",
            0],
            ["2019-10-30T11:20:00Z",
            0],
            ["2019-10-30T11:40:00Z",
            100],
            ["2019-10-30T12:00:00Z",
            0],
            ["2019-10-30T12:20:00Z",
            0],
            ["2019-10-30T12:40:00Z",
            100],
            ["2019-10-30T13:00:00Z",
            0],
            ["2019-10-30T13:20:00Z",
            0],
            ["2019-10-30T13:40:00Z",
            100],
            ["2019-10-30T14:00:00Z",
            0],
            ["2019-10-30T14:20:00Z",
            0],
            ["2019-10-30T14:40:00Z",
            100],
            ["2019-10-30T15:00:00Z",
            0],
            ["2019-10-30T15:20:00Z",
            0],
            ["2019-10-30T15:40:00Z",
            100],
            ["2019-10-30T16:00:00Z",
            0],
            ["2019-10-30T16:20:00Z",
            0],
            ["2019-10-30T16:40:00Z",
            100],
            ["2019-10-30T17:30:00Z",
            50]],
            "ArmSystemEventsRequest": "/subscriptions/4f9b81be-fa32-4f96-aeb3-fc5c3f678df9/resourceGroups/test-group/providers/microsoft.insights/components/test-rule/query?query=%0d%0a++++++++++++++++systemEvents%0d%0a++++++++++++++++%7c+where+timestamp+%3e%3d+datetime(%272019-10-30T17%3a20%3a00.0000000Z%27)+%0d%0a++++++++++++++++%7c+where+itemType+%3d%3d+%27systemEvent%27+and+name+%3d%3d+%27ProactiveDetectionInsight%27+%0d%0a++++++++++++++++%7c+where+dimensions.InsightType+in+(%275%27%2c+%277%27)+%0d%0a++++++++++++++++%7c+where+dimensions.InsightDocumentId+%3d%3d+%27718fb0c3-425b-4185-be33-4311dfb4deeb%27+%0d%0a++++++++++++++++%7c+project+dimensions.InsightOneClassTable%2c+%0d%0a++++++++++++++++++++++++++dimensions.InsightExceptionCorrelationTable%2c+%0d%0a++++++++++++++++++++++++++dimensions.InsightDependencyCorrelationTable%2c+%0d%0a++++++++++++++++++++++++++dimensions.InsightRequestCorrelationTable%2c+%0d%0a++++++++++++++++++++++++++dimensions.InsightTraceCorrelationTable%0d%0a++++++++++++&api-version=2018-04-20",
            "LinksTable": [{
                "Link": "<a href=\"https://portal.azure.com/#blade/AppInsightsExtension/ProactiveDetectionFeedBlade/ComponentId/{\"SubscriptionId\":\"4f9b81be-fa32-4f96-aeb3-fc5c3f678df9\",\"ResourceGroup\":\"test-group\",\"Name\":\"test-rule\"}/SelectedItemGroup/718fb0c3-425b-4185-be33-4311dfb4deeb/SelectedItemTime/2019-10-30T17:50:00Z/InsightType/5\" target=\"_blank\">View full details in Application Insights</a>"
            }],
            "SmartDetectorId": "FailureAnomaliesDetector",
            "SmartDetectorName": "Failure Anomalies",
            "AnalysisTimestamp": "2019-10-30T17:52:32.5802978Z"
        },
        "egressConfig": {
            "displayConfig": [{
                "rootJsonNode": null,
                "sectionName": null,
                "displayControls": [{
                    "property": "DetectionSummary",
                    "displayName": "What was detected?",
                    "type": "Text",
                    "isOptional": false,
                    "isPropertySerialized": false
                },
                {
                    "property": "FormattedOccurenceTime",
                    "displayName": "When did this occur?",
                    "type": "Text",
                    "isOptional": false,
                    "isPropertySerialized": false
                },
                {
                    "property": "DetectedFailureRate",
                    "displayName": "Detected failure rate",
                    "type": "Text",
                    "isOptional": false,
                    "isPropertySerialized": false
                },
                {
                    "property": "NormalFailureRate",
                    "displayName": "Normal failure rate",
                    "type": "Text",
                    "isOptional": false,
                    "isPropertySerialized": false
                },
                {
                    "chartType": "Line",
                    "xAxisType": "Date",
                    "yAxisType": "Percentage",
                    "xAxisName": "",
                    "yAxisName": "",
                    "property": "FailureRateChart",
                    "displayName": "Failure rate over last 12 hours",
                    "type": "Chart",
                    "isOptional": false,
                    "isPropertySerialized": false
                },
                {
                    "defaultLoad": true,
                    "displayConfig": [{
                        "rootJsonNode": null,
                        "sectionName": null,
                        "displayControls": [{
                            "showHeader": false,
                            "columns": [{
                                "property": "Name",
                                "displayName": "Name"
                            },
                            {
                                "property": "Value",
                                "displayName": "Value"
                            }],
                            "property": "tables[0].rows[0][0]",
                            "displayName": "All of the failed requests had these characteristics:",
                            "type": "Table",
                            "isOptional": false,
                            "isPropertySerialized": true
                        }]
                    }],
                    "property": "ArmSystemEventsRequest",
                    "displayName": "",
                    "type": "ARMRequest",
                    "isOptional": false,
                    "isPropertySerialized": false
                },
                {
                    "showHeader": false,
                    "columns": [{
                        "property": "Link",
                        "displayName": "Link"
                    }],
                    "property": "LinksTable",
                    "displayName": "Links",
                    "type": "Table",
                    "isOptional": false,
                    "isPropertySerialized": false
                }]
            }]
        }
    },
    "id": "/subscriptions/4f9b81be-fa32-4f96-aeb3-fc5c3f678df9/resourcegroups/test-group/providers/microsoft.insights/components/test-rule/providers/Microsoft.AlertsManagement/alerts/7daf8739-ca8a-4562-b69a-ff28db4ba0a5",
    "type": "Microsoft.AlertsManagement/alerts",
    "name": "Failure Anomalies - test-rule"
}
```

## <a name="triage-and-diagnose-an-alert"></a>Bir uyarıyı triyaj ve tanı

Bir uyarı, başarısız istek hızında anormal bir artış algılandığını gösterir. Uygulamanızla veya ortamıyla ilgili bir sorun olması olasıdır.

Daha fazla araştırma yapmak için, 'Uygulama Öngörüleri'ndeki tüm ayrıntıları görüntüleyin'e tıklayın bu sayfadaki bağlantılar sizi doğrudan ilgili isteklere, özel durumlara, bağımlılıklara veya izlemelere filtrelenmiş bir [arama sayfasına](../../azure-monitor/app/diagnostic-search.md) götürür. 

Ayrıca [Azure portalını](https://portal.azure.com)açabilir, uygulamanız için Uygulama Öngörüleri kaynağına gidebilir ve Hatalar sayfasını açabilirsiniz.

'Hataları tanılamak' seçeneğini tıklatmak, daha fazla ayrıntı almanıza ve sorunu çözmenize yardımcı olur.

[![](./media/proactive-failure-diagnostics/051.png "Diagnostic search")](./media/proactive-failure-diagnostics/051.png#lightbox)

İsteklerin yüzdesi ve etkilenen kullanıcı sayısı, sorunun ne kadar acil olduğuna karar verebilirsiniz. Yukarıdaki örnekte, %78,5'lik başarısızlık oranı normal %2,2'lik bir oran ile karşılaştırılır ve kötü bir şey olduğunu gösterir. Öte yandan, sadece 46 kullanıcı etkilendi. Eğer bu sizin uygulamanız olsaydı, bunun ne kadar ciddi olduğunu değerlendirebilirdin.

Çoğu durumda, sorunu istek adından, özel durumdan, bağımlılık hatasından ve sağlanan verileri izlemeden hızlı bir şekilde tanılayabilirsiniz.

Bu örnekte, istek sınırına ulaşılan nedeniyle SQL veritabanından bir özel durum vardı.

[![](./media/proactive-failure-diagnostics/052.png "Failed request details")](./media/proactive-failure-diagnostics/052.png#lightbox)

## <a name="review-recent-alerts"></a>Son uyarıları gözden geçirin

En son ateşlenen uyarılara ulaşmak için Application Insights kaynak sayfasındaki **Uyarılar'ı** tıklatın:

[![](./media/proactive-failure-diagnostics/070.png "Alerts summary")](./media/proactive-failure-diagnostics/070.png#lightbox)

## <a name="whats-the-difference-"></a>Ne fark ...
Akıllı Hata anomalilerinin algılanması, Application Insights'ın diğer benzer ama farklı özelliklerini tamamlar.

* [Metrik Uyarılar](../../azure-monitor/app/alerts.md) sizin tarafından ayarlanır ve CPU doluluk, istek oranları, sayfa yükleme süreleri ve benzeri gibi ölçümler geniş bir yelpazede izleyebilirsiniz. Örneğin, daha fazla kaynak eklemeniz gerekiyorsa, bunları sizi uyarmak için kullanabilirsiniz. Buna karşılık, Akıllı Hata Anomalileri, web uygulamanızın başarısız istek oranı web uygulamasının normal davranışına göre arttığında sizi neredeyse gerçek zamanlı olarak bilgilendirmek üzere tasarlanmış küçük bir kritik ölçüm aralığını (şu anda yalnızca başarısız istek oranı) kapsar. Metrik uyarıların aksine, Smart Detection davranıştaki yanıt değişikliklerinde eşikleri otomatik olarak ayarlar ve güncelleştirir. Akıllı Algılama aynı zamanda sizin için tanılama çalışmalarını başlatarak sorunları çözmede zamandan tasarruf sağlar.

* [Performans anomalilerinin Akıllı Algılaması,](proactive-performance-diagnostics.md) ölçümlerinizdeki olağandışı desenleri keşfetmek için makine zekasını da kullanır ve sizin için herhangi bir yapılandırma gerekmez. Ancak Smart Detection of failure anomalilerinin aksine, performans anomalilerinin Akıllı Algılama'sının amacı, örneğin belirli bir tarayıcı türündeki belirli sayfalarda kötü hizmet verilebilecek kullanım manifoldunuzun bölümlerini bulmaktır. Analiz günlük olarak gerçekleştirilir ve herhangi bir sonuç bulunursa, bir uyarıdan çok daha az acil olması olasıdır. Buna karşılık, hata anormallikleri için çözümleme sürekli gelen uygulama verileri üzerinde gerçekleştirilir ve sunucu hata oranları beklenenden daha büyükse dakika içinde bildirilir.

## <a name="if-you-receive-a-smart-detection-alert"></a>Akıllı Algılama uyarısı alırsanız
*Bu uyarıyı neden aldım?*

* Bir önceki dönemin normal taban çizgisine kıyasla başarısız istek oranında anormal bir artış tespit ettik. Hataların ve ilişkili uygulama verilerinin analizinden sonra, bakmanız gereken bir sorun olduğunu düşünüyoruz.

*Bildirim kesinlikle bir sorun var anlamına mı geliyor?*

* Uygulama nın bozulması veya bozulması konusunda uyarıda bulunabiliriz, ancak semantik ve uygulama veya kullanıcılar üzerindeki etkisini ancak siz tam olarak anlayabilirsiniz.

*Başvuru verilerime mi bakıyorsun?*

* Hayır. Hizmet tamamen otomatiktir. Bildirimleri sadece siz alırsınız. Verileriniz [özeldir.](../../azure-monitor/app/data-retention-privacy.md)

*Bu uyarıya abone olmak zorunda mıyım?*

* Hayır. İstek verilerini gönderen her uygulamanın Akıllı Algılama uyarı kuralı vardır.

*Bunun yerine aboneliğimi iptal edebilir veya bildirimleri iş arkadaşlarıma göndertebilir miyim?*

* Evet, Uyarı kurallarında, yapılandırmak için Akıllı Algılama kuralını tıklatın. Uyarıyı devre dışı kılabilir veya uyarı için alıcıları değiştirebilirsiniz.

*E-postayı kaybettim. Bildirimleri portalda nerede bulabilirim?*

* Etkinlik günlüklerinde. Azure'da, uygulamanız için Application Insights kaynağını açın ve ardından Etkinlik günlüklerini seçin.

*Bazı uyarılar bilinen sorunlar hakkında ve bunları almak istemiyorum.*

* [Uyarı eylem kuralları](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-action-rules) bastırma özelliğini kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu tanı araçları, uygulamanızdaki verileri incelemenize yardımcı olur:

* [Metrik kaşif](../../azure-monitor/platform/metrics-charts.md)
* [Arama gezgini](../../azure-monitor/app/diagnostic-search.md)
* [Analitik - güçlü sorgu dili](../../azure-monitor/log-query/get-started-portal.md)

Akıllı algılamalar otomatiktir. Ama belki biraz daha uyarı kurmak istersin?

* [El ile yapılandırılan metrik uyarılar](../../azure-monitor/app/alerts.md)
* [Kullanılabilirlik web testleri](../../azure-monitor/app/monitor-web-app-availability.md)
