---
title: Azure Hizmet Kumaş Platformu Seviye İzleme
description: Azure Hizmet Dokusu kümelerini izlemek ve tanılamak için kullanılan platform düzeyindeki olaylar ve günlükler hakkında bilgi edinin.
author: srrengar
ms.topic: conceptual
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: 720cc157111293146b796f8567f94a4f1f4830c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75376945"
---
# <a name="monitoring-the-cluster"></a>Kümenin izlenmesi

Donanımınızın ve kümenizin beklendiği gibi davranıp davranmadığını belirlemek için küme düzeyinde izlemek önemlidir. Hizmet Kumaşı bir donanım hatası sırasında uygulamaları çalışır durumda tutabilir, ancak yine de bir uygulamada mı yoksa altta yatan altyapıda mı bir hata oluştuğunu tanılamanız gerekir. Ayrıca, donanım ekleme veya kaldırma ile ilgili kararlarda yardımcı olmak için kapasite için daha iyi plan yapmak için kümelerinizi izlemeniz gerekir.

Service Fabric, [EventStore](service-fabric-diagnostics-events.md)ve çeşitli günlük kanalları aracılığıyla Service Fabric etkinlikleri gibi çeşitli yapılandırılmış platform olaylarını ortaya çıkarır. 

Windows'da, Service Fabric etkinlikleri, Operasyonel ve Veri & `logLevelKeywordFilters` Mesajlaşma kanalları arasında seçim yapmak için kullanılan alakalı bir dizi ile tek bir ETW sağlayıcısından edinilebilir - bu, giden Hizmet Dokusu olaylarını gerektiğinde filtrelenecek şekilde ayırma şeklimizdir.

* **Operasyonel** Service Fabric ve küme tarafından gerçekleştirilen üst düzey işlemler, bir düğüm için olaylar da dahil olmak üzere yaklaşan, yeni bir uygulama dağıtılan veya bir yükseltme geri alma, vb. [Burada](service-fabric-diagnostics-event-generation-operational.md)olayların tam listesine bakın.  

* **Operasyonel - ayrıntılı**  
Sağlık raporları ve yük dengeleme kararları.

İşlem kanalına ETW/Windows Olay Günlükleri, [EventStore](service-fabric-diagnostics-eventstore.md) (Windows 6.2 sürümlerinde ve daha sonra Windows kümeleri için kullanılabilir) dahil olmak üzere çeşitli yollarla erişilebilir. EventStore, kümenizin olaylarına varlık bazında (küme, düğümler, uygulamalar, hizmetler, bölümler, yinelemeler ve kapsayıcılar dahil olmak üzere varlıklar) erişmenizi sağlar ve bunları REST API'leri ve Service Fabric istemci kitaplığı aracılığıyla ortaya çıkarır. Geliştirme/test kümelerinizi izlemek ve üretim kümelerinizin durumunu zaman içinde anlamak için EventStore'u kullanın.

* **Veri & Mesajlaşma**  
İletide (şu anda yalnızca Ters Proxy) ve veri yolu (güvenilir hizmet modelleri) oluşturulan kritik günlükler ve olaylar.

* **Veri & Mesajlaşma - ayrıntılı**  
Kümedeki veri ve iletiden gelen tüm kritik olmayan günlükleri içeren verbose kanalı (bu kanal çok yüksek bir olay hacmine sahiptir).

Bunlara ek olarak, sağlanan iki yapılandırılmış EventSource kanalının yanı sıra destek amacıyla topladığımız günlükler de vardır.

* [Reliable Services olayları](service-fabric-reliable-services-diagnostics.md)  
Programlama modeli belirli olaylar.

* [Reliable Actors olayları](service-fabric-reliable-actors-diagnostics.md)  
Programlama modeli belirli olaylar ve performans sayaçları.

* Destek günlükleri  
Service Fabric tarafından oluşturulan sistem günlükleri sadece destek sağlarken bizim kullandığımız tarafından kullanılacak.

Bu çeşitli kanallar tavsiye edilen platform düzeyi günlüğe kaydetmenin çoğunu kapsar. Platform düzeyinde günlüğe kaydetmeyi geliştirmek için, sağlık modelini daha iyi anlamak ve özel sağlık raporları eklemek ve hizmetlerinizin ve uygulamalarınızın küme üzerindeki etkisini gerçek zamanlı olarak anlamak için özel **Performans Sayaçları** eklemeye yatırım yapmayı düşünün.

Bu günlüklerden yararlanmak için Azure Portalı'nda küme oluşturma sırasında "Tanılama"yı etkin bırakmanız önerilir. Windows Azure Diagnostics, küme dağıtıldığında tanılamayı açarak Operasyonel, Güvenilir Hizmetler ve Güvenilir aktörler kanallarını kabul eder ve verileri [Azure Tanılama ile Toplu etkinliklerde](service-fabric-diagnostics-event-aggregation-wad.md)açıklandığı gibi depolayabiliyor.

## <a name="azure-service-fabric-health-and-load-reporting"></a>Azure Hizmet Kumaşı sağlık ve yük raporlama

Service Fabric bu makalelerde ayrıntılı olarak açıklanan kendi sağlık modeli vardır:

- [Service Fabric sistem durumu izlemeye giriş](service-fabric-health-introduction.md)
- [Hizmet durumunu raporlama ve denetleme](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
- [Özel Hizmet Kumaşı sağlık raporları ekleme](service-fabric-report-health.md)
- [Hizmet Kumaşı sağlık raporlarını görüntüleyin](service-fabric-view-entities-aggregated-health.md)

Sistem durumu izleme, özellikle bir uygulama yükseltmesi sırasında bir hizmeti çalıştırmanın birden çok yönü için önemlidir. Hizmetin her yükseltme etki alanı yükseltildikten sonra, dağıtım bir sonraki yükseltme etki alanına geçmeden önce yükseltme etki alanının sistem durumu denetimlerinden geçmesi gerekir. Tamam sistem durumu elde edilemiyorsa, uygulama bilinen bir TAMAM durumunda kalacak şekilde dağıtım geri alınır. Bazı müşteriler hizmetler geri alınamadan önce etkilenebilir, ancak çoğu müşteri bir sorunla karşılaşmayabilir. Ayrıca, bir çözüm nispeten hızlı bir insan operatöreylem beklemek zorunda kalmadan oluşur. Kodunuza ne kadar çok sistem durumu denetimi eklenirse, hizmetiniz dağıtım sorunlarına o kadar esnek olur.

Hizmet sağlığının bir diğer yönü de hizmetten ölçümleri raporlamaktır. Kaynak kullanımını dengelemek için kullanıldığından, Hizmet Kumaşı'nda ölçümler önemlidir. Ölçümler, sistem sağlığının bir göstergesi de olabilir. Örneğin, çok sayıda hizmeti olan bir uygulamanız olabilir ve her örnek saniyede bir istek (RPS) ölçümü raporeder. Bir hizmet başka bir hizmetten daha fazla kaynak kullanıyorsa, Hizmet Kumaşı, kaynak kullanımını eşit tutmak için hizmet örneklerini kümenin etrafında taşır. Kaynak kullanımının nasıl çalıştığına ilişkin daha ayrıntılı bir açıklama için [bkz.](service-fabric-cluster-resource-manager-metrics.md)

Ölçümler ayrıca, hizmetinizin nasıl performans gösterdiği hakkında bilgi vermenize de yardımcı olabilir. Zaman içinde, hizmetin beklenen parametreler dahilinde çalıştığını denetlemek için ölçümleri kullanabilirsiniz. Örneğin, eğilimler Pazartesi sabahı saat 9'da ortalama RPS'nin 1.000 olduğunu gösteriyorsa, RPS 500'ün altında veya 1.500'ün üzerindeyse sizi uyaran bir sağlık raporu ayarlayabilirsiniz. Her şey mükemmel iyi olabilir, ama müşterilerinizin büyük bir deneyim yaşıyorsanız emin olmak için bir göz değer olabilir. Hizmetiniz, sistem durumu denetimi amacıyla bildirilebilen, ancak kümenin kaynak dengelemesini etkilemeyen bir ölçüm kümesi tanımlayabilir. Bunu yapmak için metrik ağırlığı sıfıra ayarlayın. Tüm ölçümleri sıfır ağırlığıyla başlatmanızı ve ölçümlerin ağırlığının kümeniz için kaynak dengelemeyi nasıl etkilediğini anlayana kadar ağırlığı artırmamanızı öneririz.

> [!TIP]
> Çok fazla ağırlıklı ölçüm kullanmayın. Hizmet örneklerinin dengeleme için neden taşındığını anlamak zor olabilir. Birkaç ölçümler uzun bir yol olabilir!

Uygulamanızın durumunu ve performansını gösteren tüm bilgiler ölçümler ve sistem durumu raporları için bir adaydır. **CPU performans sayacı düğümünüzün nasıl kullanılacağını size söyleyebilir, ancak belirli bir hizmetin sağlıklı olup olmadığını söylemez, çünkü birden çok hizmet tek bir düğümüzerinde çalışıyor olabilir.** Ancak, RPS, işlenen öğeler ve istek gecikmesi gibi ölçümler, belirli bir hizmetin durumunu gösterebilir.

## <a name="service-fabric-support-logs"></a>Servis Kumaş destek günlükleri

Azure Hizmet Kumaşı kümenizle ilgili yardım için Microsoft desteğine başvurmanız gerekiyorsa, destek günlüklerinin hemen hemen her zaman olması gerekir. Kümeniz Azure'da barındırılıyorsa, destek günlükleri otomatik olarak yapılandırılır ve küme oluşturmanın bir parçası olarak toplanır. Günlükler kümenizin kaynak grubunda özel bir depolama hesabında depolanır. Depolama hesabının sabit bir adı yoktur, ancak *hesapta, kumaşla*başlayan adlara sahip blob kapları ve tablolar görürsünüz. Tek başına bir küme için günlük koleksiyonları ayarlama hakkında bilgi için, bağımsız bir Windows kümesi için [bağımsız bir Azure Hizmet Kumaşı kümesi](service-fabric-cluster-creation-for-windows-server.md) ve Yapılandırma [ayarlarını](service-fabric-cluster-manifest.md)oluştur ve yönetin. Bağımsız Hizmet Kumaşı örnekleri için günlükler yerel bir dosya paylaşımına gönderilmelidir. Destek için bu günlüklere sahip olmanız **gerekir,** ancak microsoft müşteri destek ekibi dışındaki herkes tarafından kullanılabilir olması amaçlanmamıştır.

## <a name="measuring-performance"></a>Performansı ölçme

Kümenizin performansını ölçmek, kümenizi ölçekleme etrafında ki yük ve sürücü kararlarını nasıl işleyebilir yardımcı olur [(Azure'da](service-fabric-cluster-scale-up-down.md)veya [şirket içinde](service-fabric-cluster-windows-server-add-remove-nodes.md)bir kümeölçekleme hakkında daha fazla bilgiyi görün). Performans verileri, sizin veya uygulamalarınızın ve hizmetlerinizin gelecekte günlükleri analiz ederken yapmış olabileceği eylemlerle karşılaştırıldığında da yararlıdır. 

Servis Kumaşı kullanırken toplanacak performans sayaçlarının listesi için [Servis Kumaşındaki Performans Sayaçları'na](service-fabric-diagnostics-event-generation-perf.md) bakın

Kümeniz için performans verilerini toplamayı ayarlamanın iki yaygın yolu şunlardır:

* **Aracı kullanma**  
Aracılar genellikle toplanabilir olası performans ölçümleri listesi olduğundan ve toplamak veya değiştirmek istediğiniz ölçümleri seçmek nispeten kolay bir işlemolduğundan, bu, bir makineden performans toplamanın tercih edilen yoludur. Azure Monitörü hakkında Oku, Service Fabric'in [Azure Monitor günlükleri tümleştirmesinde](service-fabric-diagnostics-event-analysis-oms.md) azure monitörgünlükleri sunar ve Log [Analytics aracısını](../log-analytics/log-analytics-windows-agent.md) küme VM'leri ve dağıtılmış kapsayıcılar için performans verilerini alabilen bu tür bir izleme aracısı olan Log Analytics aracısı hakkında daha fazla bilgi edinmek için ayarlama.

* **Azure Tablo Depolamasına performans sayaçları**  
Performans ölçümlerini olaylarla aynı tablo depolama alanına da gönderebilirsiniz. Bu, kümenizdeki VM'lerden uygun performans sayaçlarını almak için Azure Tanılama yapılandırmasını değiştirmeyi ve herhangi bir kapsayıcı dağıtacaksanız docker istatistiklerini almasını sağlamayı gerektirir. Performans sayacı koleksiyonunu ayarlamak için [HIZMET Kumaşında WAD'da Performans Sayaçları](service-fabric-diagnostics-event-aggregation-wad.md) yapılandırma hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

* Küme tanılamaları toplamak ve özel sorgular ve uyarılar oluşturmak için Service Fabric'in [Azure Monitörü günlükleri tümleştirmesi](service-fabric-diagnostics-event-analysis-oms.md) hakkında bilgi edinin
* Service Fabric'in yerleşik tanılama deneyimi hakkında bilgi edinin, [EventStore](service-fabric-diagnostics-eventstore.md)
* Service Fabric'te [bazı yaygın tanılama senaryolarında](service-fabric-diagnostics-common-scenarios.md) yürüyün
