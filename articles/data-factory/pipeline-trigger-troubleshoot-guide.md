---
title: Azure Data Factory 'da işlem hattı düzenleme ve Tetikleyicileri sorunlarını giderme
description: Azure Data Factory içinde işlem hattı tetikleme sorunlarını gidermek için farklı yöntemler kullanın.
author: ssabat
ms.service: data-factory
ms.date: 12/15/2020
ms.topic: troubleshooting
ms.author: susabat
ms.reviewer: susabat
ms.openlocfilehash: 0ceee3c65e8c4df5d843bb441fb6426a0f4eb696
ms.sourcegitcommit: d59abc5bfad604909a107d05c5dc1b9a193214a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98220278"
---
# <a name="troubleshoot-pipeline-orchestration-and-triggers-in-azure-data-factory"></a>Azure Data Factory 'da işlem hattı düzenleme ve Tetikleyicileri sorunlarını giderme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Data Factory'de işlem hattı çalıştırması, bir işlem hattı yürütme örneğini tanımlar. Örneğin, 8:00, 9:00 ve 10:00 ' de çalışan bir işlem hattınızı varsayalım. Bu durumda, üç ayrı işlem hattı çalıştırması vardır. Her işlem hattı çalıştırması benzersiz bir işlem hattı çalıştırma kimliğine sahiptir. Çalıştırma KIMLIĞI, belirli bir işlem hattının çalıştırılacağını tanımlayan bir genel benzersiz tanımlayıcıdır (GUID).

İşlem hattı çalıştırmaları örneği genelde bağımsız değişkenlerin işlem hatlarında tanımladığınız parametrelere iletilmesiyle oluşturulur. Bir işlem hattını el ile veya bir tetikleyici kullanarak çalıştırabilirsiniz. Ayrıntılar için bkz. işlem [hattı yürütme ve tetikleyiciler Azure Data Factory](concepts-pipeline-execution-triggers.md) .

## <a name="common-issues-causes-and-solutions"></a>Yaygın sorunlar, nedenler ve çözümler

### <a name="an-azure-functions-app-pipeline-throws-an-error-with-private-endpoint-connectivity"></a>Azure Işlevleri uygulama işlem hattı, Özel uç nokta bağlantısı ile ilgili bir hata oluşturur
 
Data Factory ve özel bir uç noktada çalışan bir Azure işlev uygulamanız var. İşlev uygulamasıyla etkileşime giden bir işlem hattı çalıştırmaya çalışıyorsunuz. Üç farklı yöntem denediniz, ancak biri "Hatalı Istek" hatası döndürüyor ve diğer iki yöntem "103 hatası yasak" döndürüyor.

**Neden**: Data Factory Şu anda işlev uygulamaları için özel bir uç nokta bağlayıcısını desteklememektedir. Azure Işlevleri yalnızca özel bir bağlantıdan gelen bağlantılara izin verecek şekilde yapılandırıldığından çağrıları reddeder.

**Çözüm**: **privatelinkservice** uç noktası oluşturun ve işlev uygulamanızın DNS ' i belirtin.

### <a name="a-pipeline-run-is-canceled-but-the-monitor-still-shows-progress-status"></a>İşlem hattı çalıştırması iptal edildi ancak izleyici hala ilerleme durumunu gösteriyor

İşlem hattı çalıştırmasını iptal ettiğinizde, ardışık düzen izlemesi genellikle ilerleme durumunu gösterir. Bu, bir tarayıcı önbelleği sorunu nedeniyle oluşur. Ayrıca, doğru izleme filtrelerine sahip olmayabilirsiniz.

**Çözüm**: Tarayıcıyı yenileyin ve doğru izleme filtrelerini uygulayın.
 
### <a name="you-see-a-delimitedtextmorecolumnsthandefined-error-when-copying-a-pipeline"></a>İşlem hattını kopyalarken bir "Delimitedtextmorecolumnsıısdefined" hatası görürsünüz
 
Kopyaladığınız bir klasör, değişken sayısı, farklı sınırlayıcılar, quote char ayarları veya bazı veri sorunları gibi farklı şemalarla dosya içeriyorsa Data Factory işlem hattı bu hatayı oluşturabilir:

`
Operation on target Copy_sks  failed: Failure happened on 'Sink' side.
ErrorCode=DelimitedTextMoreColumnsThanDefined,
'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
Message=Error found when processing 'Csv/Tsv Format Text' source '0_2020_11_09_11_43_32.avro' with row number 53: found more columns than expected column count 27.,
Source=Microsoft.DataTransfer.Common,'
`

**Çözüm**: kopyalama etkinliğini oluştururken **ikili kopya** seçeneğini belirleyin. Bu şekilde, toplu kopyalar veya verilerinizi bir Data Lake 'ten diğerine geçirmek için Data Factory şemayı okumak için dosyaları açmaz. Bunun yerine, Data Factory her dosyayı ikili olarak değerlendirir ve diğer konuma kopyalar.

### <a name="a-pipeline-run-fails-when-you-reach-the-capacity-limit-of-the-integration-runtime"></a>Tümleştirme çalışma zamanının kapasite sınırına ulaştığınızda işlem hattı çalıştırması başarısız olur

Hata iletisi:

`
Type=Microsoft.DataTransfer.Execution.Core.ExecutionException,Message=There are substantial concurrent MappingDataflow executions which is causing failures due to throttling under Integration Runtime 'AutoResolveIntegrationRuntime'.
`

**Neden**: Integration Runtime 'ın kapasite sınırına ulaştınız. Aynı tümleştirme çalışma zamanını aynı anda kullanarak büyük miktarda veri akışı çalıştırıyor olabilirsiniz. Ayrıntılar için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#version-2) .

**Çözüm**:
 
- İşlem hatlarınızı farklı tetikleyici saatlerinde çalıştırın.
- Yeni bir tümleştirme çalışma zamanı oluşturun ve işlem hatlarınızı birden çok tümleştirme çalışma zamanına ayırın.

### <a name="you-have-activity-level-errors-and-failures-in-pipelines"></a>İşlem hatlarında etkinlik düzeyindeki hatalar ve hatalar var

Azure Data Factory düzenleme koşullu mantığa izin verir ve kullanıcıların önceki bir etkinliğin sonucuna göre farklı yollar almasına olanak tanır. Dört koşullu yol sağlar: **başarılı** olduğunda (varsayılan geçiş), **hatadan** sonra, **tamamlandığında ve atlamada**.  

Azure Data Factory tüm yaprak düzeyindeki etkinliklerin sonucunu değerlendirir. İşlem hattı sonuçları yalnızca tümü başarılı olursa başarılı olur. Bir yaprak etkinlik atlandıysa bunun yerine üst etkinliğini değerlendiririz. 

**Çözünürlük**

1. İşlem [hattı hatalarının ve hatalarının nasıl işleneceğini](https://techcommunity.microsoft.com/t5/azure-data-factory/understanding-pipeline-failures-and-error-handling/ba-p/1630459)izleyerek etkinlik düzeyi denetimleri uygulayın.
1. İşlem hatlarını [fabrika tarafından sorgu](https://docs.microsoft.com/rest/api/datafactory/pipelineruns/querybyfactory)izleyen düzenli aralıklarla izlemek için Azure Logic Apps kullanın.

## <a name="monitor-pipeline-failures-in-regular-intervals"></a>Düzenli aralıklarla işlem hattı başarısızlıklarını izleme

Başarısız Data Factory işlem hatlarını dakikalar içinde izlemeniz, 5 dakika söylemeniz gerekebilir. Uç noktasını kullanarak bir veri fabrikasında işlem hattı çalıştırmalarını sorgulayabilir ve filtreleyebilirsiniz. 

[Fabrika tarafından sorgulama](https://docs.microsoft.com/rest/api/datafactory/pipelineruns/querybyfactory)bölümünde açıklandığı gibi, tüm başarısız işlem hatlarını 5 dakikada bir sorgulamak Için bir Azure mantıksal uygulaması ayarlayın. Daha sonra, olayları, anahtar oluşturma sistemimize rapor edebilirsiniz.

Daha fazla bilgi için [Data Factory, Bölüm 2 ' den bildirim gönder '](https://www.mssqltips.com/sqlservertip/5962/send-notifications-from-an-azure-data-factory-pipeline--part-2/)e gidin.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla sorun giderme Yardım için şu kaynakları deneyin:

*  [Data Factory blogu](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Soru sayfası Microsoft Q&](/answers/topics/azure-data-factory.html)
*  [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)
