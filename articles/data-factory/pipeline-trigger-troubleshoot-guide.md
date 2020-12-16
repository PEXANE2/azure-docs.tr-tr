---
title: ADF 'de işlem hattı düzenleme ve Tetikleyicileri sorunlarını giderme
description: ADF 'de işlem hattı tetikleme sorunlarını gidermek için farklı yöntemler kullanma
author: ssabat
ms.service: data-factory
ms.date: 12/15/2020
ms.topic: troubleshooting
ms.author: susabat
ms.reviewer: susabat
ms.openlocfilehash: ed3728513820da9f4ef85d44cac983dc09c3fc7d
ms.sourcegitcommit: 66479d7e55449b78ee587df14babb6321f7d1757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97521853"
---
# <a name="troubleshoot-pipeline-orchestration-and-triggers-in-adf"></a>ADF 'de işlem hattı düzenleme ve Tetikleyicileri sorunlarını giderme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Data Factory'de işlem hattı çalıştırması, bir işlem hattı yürütme örneğini tanımlar. Örneğin, 8:00, 9:00 ve 10:00 ' de yürütülen bir işlem hattına sahipsiniz. Bu durumda, işlem hattının veya işlem hattı çalıştırmalarının üç ayrı çalıştırması vardır. Her işlem hattı çalıştırması benzersiz bir işlem hattı çalıştırma kimliğine sahiptir. Çalıştırma KIMLIĞI, belirli bir işlem hattının çalıştırılacağını tanımlayan bir GUID (genel benzersiz tanımlayıcıdır).

İşlem hattı çalıştırmaları örneği genelde bağımsız değişkenlerin işlem hatlarında tanımladığınız parametrelere iletilmesiyle oluşturulur. İşlem hattını el ile veya bir tetikleyici kullanarak çalıştırabilirsiniz. Ayrıntılar için [Azure Data Factory Işlem hattı yürütme ve tetikleyicilere](concepts-pipeline-execution-triggers.md) bakın.

## <a name="common-issues-causes-and-solutions"></a>Yaygın sorunlar, nedenler ve çözümler

### <a name="pipeline-with-azure-function-throws-error-with-private-end-point-connectivity"></a>Azure Işlevi ile işlem hattı özel uç nokta bağlantısı ile hata oluşturuyor
 
#### <a name="issue"></a>Sorun
Bazı bağlamda, ADF ve Azure İşlev Uygulaması özel bir uç nokta üzerinde çalışıyor. Azure İşlev Uygulaması çalışmak üzere etkileşim kuran bir işlem hattı almaya çalışıyorsunuz. Üç farklı yöntem denediniz, ancak bir hata döndürüyor `Bad Request` , diğer iki yöntem geri döndürüyor `103 Error Forbidden` .

#### <a name="cause"></a>Nedeni 
ADF Şu anda Azure İşlev Uygulaması için özel bir uç nokta bağlayıcısını desteklemiyor. Bu nedenle, Azure İşlev Uygulaması, yalnızca özel bir bağlantıdan gelen bağlantılara izin verecek şekilde yapılandırıldığından çağrıları reddetme nedeni olmalıdır.

#### <a name="resolution"></a>Çözüm
**Privatelinkservice** türünde özel bir uç nokta oluşturabilir ve Işlev uygulamanızın DNS 'sini sağlayabilirsiniz ve bağlantı çalışır.

### <a name="pipeline-run-is-killed-but-the-monitor-still-shows-progress-status"></a>İşlem hattı çalıştırması sonlandırıldı, ancak izleyici hala ilerleme durumunu gösteriyor

#### <a name="issue"></a>Sorun
İşlem hattı çalıştırmasını genellikle sonlandırdığınızda, işlem hattı izleme hala ilerleme durumunu gösterir. Bu durum, tarayıcıda önbellek sorunu nedeniyle ve izleme için doğru filtreolmadığınızdan oluşur.

#### <a name="resolution"></a>Çözüm
Tarayıcıyı yenileyin ve izleme için sağ filtreler uygulayın.
 
### <a name="copy-pipeline-failure--found-more-columns-than-expected-column-count-delimitedtextmorecolumnsthandefined"></a>Kopyalama işlem hattı hatası – beklenen sütun sayısından daha fazla sütun bulundu (Delimitedtextmorecolumnsıısdefined tanımlandı)

#### <a name="issue"></a>Sorun  
Kopyaladığınız belirli bir klasördeki dosyalar, değişken sayısı, farklı sınırlayıcılar, quote char ayarları veya bazı veri sorunları gibi farklı şemalara sahip dosyalar içeriyorsa, ADF ardışık düzeni Bu hatada çalışmaya sona acaktır:

`
Operation on target Copy_sks  failed: Failure happened on 'Sink' side.
ErrorCode=DelimitedTextMoreColumnsThanDefined,
'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
Message=Error found when processing 'Csv/Tsv Format Text' source '0_2020_11_09_11_43_32.avro' with row number 53: found more columns than expected column count 27.,
Source=Microsoft.DataTransfer.Common,'
`

#### <a name="resolution"></a>Çözüm
Veri Kopyalama etkinliğini oluştururken "Ikili kopya" seçeneğini belirleyin. Bu şekilde, toplu kopyalama veya verilerinizi bir Data Lake diğerine geçirme için **ikili** SEÇENEKTE, ADF şemayı okumak için dosyaları açmaz, ancak her dosyayı ikili olarak değerlendirin ve diğer konuma kopyalayın.

### <a name="pipeline-run-fails-when-capacity-limit-of-integration-runtime-is-reached"></a>Tümleştirme çalışma zamanının kapasite sınırına ulaşıldığında işlem hattı çalıştırma başarısız olur

#### <a name="issue"></a>Sorun
Hata iletisi:

`
Type=Microsoft.DataTransfer.Execution.Core.ExecutionException,Message=There are substantial concurrent MappingDataflow executions which is causing failures due to throttling under Integration Runtime 'AutoResolveIntegrationRuntime'.
`

Hata, tümleştirme çalışma zamanı başına şu anda 50 olan sınırlamayı gösterir. Ayrıntılar için [sınırlara](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#version-2) bakın.

Aynı tümleştirme çalışma zamanını kullanarak büyük miktarda veri akışı çalıştırırsanız, bu tür bir hataya neden olabilir.

#### <a name="resolution"></a>Çözüm 
- Bu işlem hatlarını farklı tetikleyici süresi için ayırın.
- Yeni bir tümleştirme çalışma zamanı oluşturun ve bu işlem hatlarını birden çok tümleştirme çalışma zamanına ayırın.

### <a name="how-to-monitor-pipeline-failures-on-regular-interval"></a>Düzenli aralıklarla işlem hattı başarısızlıklarını izleme

#### <a name="issue"></a>Sorun
Genellikle ADF işlem hatlarını dakikalar içinde izlemeniz, 5 dakika söylemeniz gerekir. Uç nokta kullanarak bir veri fabrikasında işlem hattı çalıştırmalarını sorgulayabilir ve filtreleyebilirsiniz. 

#### <a name="recommendation"></a>Öneri
1. Her 5 dakikada bir başarısız olan tüm işlem hatlarını sorgulamak için bir Azure mantıksal uygulaması ayarlayın.
2. Daha sonra, olayları [Querybyfactory](https://docs.microsoft.com/rest/api/datafactory/pipelineruns/querybyfactory)başına anahtar oluşturma sistemimize rapor edebilirsiniz.

#### <a name="reference"></a>Başvuru
- [ADF 'den dış gönderme bildirimleri](https://www.mssqltips.com/sqlservertip/5962/send-notifications-from-an-azure-data-factory-pipeline--part-2/)

### <a name="how-to-handle-activity-level-errors-and-failures-in-pipelines"></a>İşlem hatlarında etkinlik düzeyindeki hataları ve hataları işleme

#### <a name="issue"></a>Sorun
Azure Data Factory düzenleme koşullu mantığa izin verir ve kullanıcının önceki bir etkinliğin sonuçlarını temel alarak farklı yollar almasına olanak tanır. Dört koşullu yola izin verir: "başarılı olduğunda (varsayılan geçiş)", "hatadan sonra", "tamamlandığında" ve "atlama sonrasında". Farklı yolların kullanılmasına izin verilir.

Azure Data Factory, işlem hattı çalıştırma başarısını ve başarısızlığını aşağıdaki şekilde tanımlar:

- Tüm yaprak düzeyindeki etkinliklerin sonucunu değerlendirin. Bir yaprak etkinlik atlandıysa bunun yerine üst etkinliğini değerlendiririz.
- Yalnızca tümü başarılı olursa işlem hattı sonucu başarılı olur.

#### <a name="recommendation"></a>Öneri
- İşlem [hattı hatalarının ve hatalarının nasıl işleneceğini](https://techcommunity.microsoft.com/t5/azure-data-factory/understanding-pipeline-failures-and-error-handling/ba-p/1630459)izleyerek etkinlik düzeyi denetimleri uygulayın.
- Azure Logic App kullanarak, işlem hatlarını [DataFactory tarafından sorgu]( https://docs.microsoft.com/rest/api/datafactory/pipelineruns/querybyfactory)izleyen düzenli aralıklarda izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla sorun giderme Yardım için şu kaynakları deneyin:

*  [Data Factory blogu](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Soru sayfası Microsoft Q&](/answers/topics/azure-data-factory.html)
*  [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)