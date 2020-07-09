---
title: Azure Stream Analytics’e Genel Bakış
description: Nesnelerin İnterneti'nden (IoT) sağlanan akış verilerini gerçek zamanlı olarak analiz etmenize yardım eden bir yönetilen hizmet olan Stream Analytics hakkında bilgi edinin.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: overview
ms.custom: mvc
ms.date: 07/6/2020
ms.openlocfilehash: d62fd0a23a5f5553f27c7a399eb17d06d427a6f3
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86108796"
---
# <a name="what-is-azure-stream-analytics"></a>Azure Stream Analytics nedir?

Azure Stream Analytics, birden fazla kaynaktan aynı anda yüksek hacimli hızlı akış verilerini çözümlemek ve işlemek için tasarlanan gerçek zamanlı analiz ve karmaşık olay işleme altyapısıdır. Desenler ve ilişkiler cihazlar, sensörler, tıklama akışları, sosyal medya akışları ve uygulamalar dahil olmak üzere çeşitli giriş kaynaklarından ayıklanan bilgiler içinde tanımlanabilir. Bu desenler eylemleri tetiklemek ve uyarı oluşturma, bir raporlama aracına bilgi akışı veya dönüştürülen verileri daha sonra kullanmak üzere depolama gibi iş akışlarını başlatmak için kullanılabilir. Ayrıca, Stream Analytics Azure IoT Edge çalışma zamanında kullanılabilir ve IoT cihazlarındaki verilerin işlemesini etkinleştirir. 

Aşağıdaki senaryolar Azure Stream Analytics kullanabileceğiniz örnekler örneğidir:

* IoT cihazlarından gerçek zamanlı telemetri akışlarını çözümleme
* Web günlükleri/clickstream analizi
* Filo yönetimi ve sürücüsüz araçlar için jeo-uzamsal analiz
* Yüksek değerli varlıkların uzaktan izlenmesi ve tahmine dayalı Bakımı
* Envanter denetimi ve anomali algılama için Satış Noktasında gerçek zamanlı analizler

## <a name="how-does-stream-analytics-work"></a>Stream Analytics nasıl çalışır?

Azure Stream Analytics iş bir giriş, sorgu ve bir çıktıdan oluşur. Azure Event Hubs (Apache Kafka 'den Azure Event Hubs dahil), Azure IoT Hub veya Azure Blob depolama 'dan verileri Stream Analytics. SQL sorgu diline dayalı sorgu, akış verilerini kolayca filtrelemek, sıralamak, toplamak ve bir süre boyunca birleştirmek için kullanılabilir. Bu SQL dilini JavaScript ve C# Kullanıcı tanımlı işlevleri (UDF 'ler) ile de genişletebilirsiniz. Basit dil yapıları ve/veya yapılandırmalarında toplama işlemleri yaparken, olay sıralama seçeneklerini ve zaman pencerelerinin süresini kolayca ayarlayabilirsiniz.

Her işin dönüştürülmüş veriler için bir veya birkaç çıkışı vardır ve analiz ettiğiniz bilgilere yanıt olarak neler olduğunu denetleyebilirsiniz. Örneğin, şunları yapabilirsiniz:

* İletişimleri veya özel iş akışlarını aşağı yönde tetiklemek için Azure Işlevleri, Service Bus konuları veya kuyrukları gibi hizmetlere veri gönderin.
* Gerçek zamanlı bir taslak oluşturma için Power BI panoya veri gönderme.
* Bir makine öğrenimi modelini geçmiş verilere göre eğitme veya Batch Analytics 'i gerçekleştirmeye yönelik diğer Azure depolama hizmetlerinde (ör. Azure Data Lake, Azure SYNAPSE Analytics vb.) veri depolayın.

Aşağıdaki görüntüde, verilerin depolama veya sunum gibi diğer eylemler için Stream Analytics, analiz edildi ve gönderildi olarak nasıl gönderildiği gösterilmektedir:

![Stream Analytics giriş işlem hattı](./media/stream-analytics-introduction/stream-analytics-e2e-pipeline.png)

## <a name="key-capabilities-and-benefits"></a>Temel işlevler ve avantajlar

Azure Stream Analytics kullanımı kolay, esnek, güvenilir ve her boyuttaki iş için ölçeklenebilir olacak şekilde tasarlanmıştır. Birden çok Azure bölgesinde mevcuttur. Aşağıdaki görüntüde Azure Stream Analytics temel özellikleri gösterilmektedir:

![Stream Analytics temel özellikleri](./media/stream-analytics-introduction/stream-analytics-key-capabilities.png)

## <a name="ease-of-getting-started"></a>Başlama kolaylığı

Azure Stream Analytics kolayca başlatılabilir. Birden çok kaynağa ve havuza bağlanmak için, uçtan uca bir işlem hattı oluşturarak yalnızca birkaç tıklama sürer. Stream Analytics, veri alımı için Azure [Event Hubs](/azure/event-hubs/) ve [Azure IoT Hub](/azure/iot-hub/) ve geçmiş verileri almak için [Azure Blob depolama alanı](/azure/storage/common/storage-introduction) 'na bağlanabilir. İş girişi, Azure Blob depolama alanından veya arama işlemlerini gerçekleştirmek için akış verilerine katılabilmeniz gereken [SQL veritabanından](stream-analytics-use-reference-data.md#azure-sql-database) statik veya yavaş değişen başvuru verilerini de içerebilir.

Stream Analytics, iş çıkışını [Azure Blob depolama](/azure/storage/common/storage-introduction), [Azure SQL veritabanı](/azure/sql-database/), [Azure Data Lake Store](/azure/data-lake-store/)ve [Azure cosmosdb](/azure/cosmos-db/introduction)gibi birçok depolama sistemine yönlendirebilir. Azure HDInsight ile saklı çıktıda Batch Analytics 'i çalıştırabilir ya da çıktıyı, tüketim [Power BI](https://docs.microsoft.com/power-bi/) veya gerçek zamanlı görselleştirme için Event Hubs gibi başka bir hizmete gönderebilirsiniz.

Tüm Stream Analytics çıktıları listesi için bkz. [Azure Stream Analytics çıktılarını anlama](stream-analytics-define-outputs.md).

## <a name="programmer-productivity"></a>Programcı verimliliği

Azure Stream Analytics, hareket halindeki verileri çözümlemek için güçlü bir zamana bağlı kısıtlamalarla genişletilmiş basit bir SQL tabanlı sorgu dili kullanır. İş dönüşümlerini tanımlamak için, basit SQL yapıları kullanarak karmaşık geçici sorgular ve analizler yazmanıza olanak tanıyan basit, bildirim temelli bir [Stream Analytics sorgu dili](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference) kullanırsınız. Stream Analytics sorgu dili SQL diline uygun olduğundan, SQL 'e alışkın olan işleri oluşturmaya başlamak yeterlidir. Azure PowerShell, [Visual Studio araçları Stream Analytics](stream-analytics-tools-for-visual-studio-install.md), [Stream Analytics Visual Studio Code uzantısı](quick-create-vs-code.md)veya Azure Resource Manager şablonları gibi geliştirici araçlarını kullanarak da işler oluşturabilirsiniz. Geliştirici araçlarının kullanılması, çevrimdışı ortamda dönüşüm sorguları geliştirmenize ve [CI/CD işlem hattı](stream-analytics-tools-for-visual-studio-cicd.md) kullanarak Azure’a iş göndermenize olanak tanır.

Stream Analytics sorgu dili, akış verilerini analiz etmek ve işlemek için geniş bir işlevler dizisi sunar. Bu sorgu dili basit veri işleme, toplama ve analiz işlevlerini, [Jeo uzamsal işlevleri](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-geospatial-functions), [model eşleştirmeyi](https://docs.microsoft.com/stream-analytics-query/match-recognize-stream-analytics) ve [anomali algılamayı](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-machine-learning-anomaly-detection)destekler. Portalda sorguları düzenleyebilir ve canlı bir akıştan ayıklanan örnek verileri kullanarak test edebilirsiniz.

Ek işlevler tanımlayıp çağırarak sorgu dilinin yapabileceklerini artırabilirsiniz. Azure Machine Learning çözümlerinin avantajlarından yararlanmak için Azure Machine Learning işlev çağrıları tanımlayabilir ve bir Stream Analytics sorgusunun parçası olarak karmaşık hesaplamalar gerçekleştirmek için JavaScript veya C# Kullanıcı tanımlı işlevleri (UDF 'ler) veya Kullanıcı tanımlı toplamaları tümleştirebilirsiniz.

## <a name="fully-managed"></a>Tam olarak yönetilir

Azure Stream Analytics, Azure üzerinde tam olarak yönetilen bir sunucusuz (PaaS) tekliftir. Herhangi bir donanım sağlamanız, işlerinizi çalıştırmak için kümeleri yönetmeniz veya işletim sistemini veya yazılımı güncelleştirmeniz gerekmez. Azure Stream Analytics, işinizi tamamen yönetir, bu sayede altyapıya değil iş mantığınıza odaklanırsınız.

## <a name="run-in-the-cloud-or-on-the-intelligent-edge"></a>Bulutta veya akıllı kenarda çalıştırın

Azure Stream Analytics bulutta çalışabilir, büyük ölçekli analizler için veya ultra düşük gecikmeli analizler için IoT Edge çalıştırabilirsiniz. Azure Stream Analytics hem bulutta hem de kenarda aynı araçları ve sorgu dilini kullanır, geliştiricilerin Stream işleme için gerçekten karma mimariler oluşturmalarına olanak tanır. 

## <a name="low-total-cost-of-ownership"></a>Düşük toplam sahip olma maliyeti

Bir bulut hizmeti olan Stream Analytics, maliyet için iyileştirilmiştir. Dahil ön maliyet yoktur; yalnızca kullandığınız [akış birimleri](stream-analytics-streaming-unit-consumption.md)için ödeme yaparsınız. Taahhüt veya küme sağlama gerekmez ve iş gereksinimlerinize göre işi yukarı veya aşağı ölçeklendirebilirsiniz.

## <a name="mission-critical-ready"></a>Görev açısından kritik hazırlık

Azure Stream Analytics dünya çapındaki birden çok bölgede kullanılabilir ve güvenilirlik, güvenlik ve uyumluluk gereksinimlerini destekleyerek görev açısından kritik iş yüklerini çalıştırmak için tasarlanmıştır.

### <a name="reliability"></a>Güvenilirlik

Azure Stream Analytics, tek bir olay işleme ve olayların en az bir kez teslim edilmesini garanti eder, bu nedenle olaylar hiçbir zaman kaybedilmez. Tam bir kez işleme, [olay teslim garantisi](/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics)bölümünde açıklandığı gibi seçili çıkışlarla garanti edilir.

Azure Stream Analytics, bir olay tesliminin başarısız olması durumunda yerleşik kurtarma özellikleri vardır. Stream Analytics Ayrıca, işinizin durumunu korumak için yerleşik kontrol noktaları sağlar ve yinelenebilir sonuçlar sağlar.

Yönetilen bir hizmet olarak Stream Analytics, bir dakikalık ayrıntı düzeyi ile% 99,9 kullanılabilirlik ile olay işlemeyi güvence altına alır. Daha fazla bilgi için [Stream ANALYTICS SLA](https://azure.microsoft.com/support/legal/sla/stream-analytics/v1_0/) sayfasına bakın. 

### <a name="security"></a>Güvenlik

Güvenlik açısından, Azure Stream Analytics tüm gelen ve giden iletişimleri şifreler ve TLS 1,2 ' yi destekler. Yerleşik kontrol noktaları da şifrelenir. Tüm işlemler bellek içinde yapıldığından, Stream Analytics gelen verileri depolamaz.

### <a name="compliance"></a>Uyumluluk

Azure Stream Analytics, [Azure uyumluluğuna genel bakış](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942)bölümünde açıklandığı gibi birden çok uyumluluk sertifikası izler. 

## <a name="performance"></a>Performans

Stream Analytics her saniye milyonlarca olayı işleyebilir ve çok fazla gecikme süreleriyle sonuç sunabilir. Büyük gerçek zamanlı ve karmaşık olay işleme uygulamalarını işlemek için ölçeği ölçeklendirmenize ve ölçeklendirmenize olanak tanır. Stream Analytics bölümleyerek daha yüksek performansı destekler, böylece karmaşık sorguların paralelleştirilmesine ve birden çok akış düğümünde yürütülmesine izin verir. Azure Stream Analytics, Microsoft Research ile işbirliği içinde geliştirilen yüksek performanslı, bellek içi akış analizi altyapısı, [popüler bir yapıda](https://github.com/Microsoft/Trill)oluşturulmuştur.

## <a name="next-steps"></a>Sonraki adımlar

Azure Stream Analytics’e genel bakışı gördünüz. Bundan sonra derinlere inerek ilk Stream Analytics işinizi oluşturabilirsiniz:

* [Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md).
* [Azure PowerShell kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-powershell.md).
* [Visual Studio 'yu kullanarak bir Stream Analytics Işi oluşturun](stream-analytics-quick-create-vs.md).
* [Visual Studio Code kullanarak Stream Analytics bir Iş oluşturun](quick-create-vs-code.md).
