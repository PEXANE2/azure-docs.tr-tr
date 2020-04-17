---
title: Azure Stream Analytics’e Genel Bakış
description: Nesnelerin İnterneti'nden (IoT) sağlanan akış verilerini gerçek zamanlı olarak analiz etmenize yardım eden bir yönetilen hizmet olan Stream Analytics hakkında bilgi edinin.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: overview
ms.custom: mvc
ms.date: 06/21/2019
ms.openlocfilehash: 673c6e2ed0aedcc186baf8c7e1884dd537f9567d
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81459857"
---
# <a name="what-is-azure-stream-analytics"></a>Azure Stream Analytics nedir?

Azure Akış Analizi, aynı anda birden fazla kaynaktan gelen yüksek hacimli hızlı akış verilerini analiz etmek ve işlemek için tasarlanmış gerçek zamanlı bir analiz ve karmaşık olay işleme motorudur. Desenler ve ilişkiler, aygıtlar, sensörler, tıklama akışları, sosyal medya akışları ve uygulamalar da dahil olmak üzere bir dizi giriş kaynağından elde edilen bilgilerde tanımlanabilir. Bu desenler eylemleri tetiklemek ve uyarılar oluşturma, raporlama aracına bilgi verme veya daha sonra kullanmak üzere dönüştürülmüş verileri depolama gibi iş akışlarını başlatmak için kullanılabilir. Ayrıca, Stream Analytics Azure IoT Edge çalışma zamanında kullanılabilir ve bulutla aynı dili veya sözdizimini destekler. 

Aşağıdaki senaryolar, Azure Akış Analizi'ni ne zaman kullanabileceğinize örnektir:

* IoT aygıtlarından gerçek zamanlı telemetri akışlarını analiz etme
* Web günlükleri/clickstream analizi
* Filo yönetimi ve sürücüsüz araçlar için jeo-uzamsal analiz
* Yüksek değerli varlıkların uzaktan izleme ve tahmine dayalı bakımı
* Envanter denetimi ve anomali algılama için Satış Noktasında gerçek zamanlı analizler

## <a name="how-does-stream-analytics-work"></a>Stream Analytics nasıl çalışır?

Azure Akış Analizi işi bir giriş, sorgu ve çıktıdan oluşur. Akış Analizi, Azure Etkinlik Hub'larından, Azure IoT Hub'ından veya Azure Blob Depolama'dan veri alıyor. SQL sorgu dilini temel alan sorgu, akış verilerini belirli bir süre boyunca kolayca filtrelemek, sıralamak, toplamak ve birleştirmek için kullanılabilir. Bu SQL dilini JavaScript ve C# kullanıcı tanımlı işlevleri (UDFs) ile de genişletebilirsiniz. Basit dil yapıları ve/veya yapılandırmaları aracılığıyla toplama işlemlerini önceden şekillendirirken olay sıralama seçeneklerini ve zaman pencerelerinin süresini kolayca ayarlayabilirsiniz.

Her işin dönüştürülmüş veriler için bir çıktısı vardır ve analiz ettiğiniz bilgilere yanıt olarak neler olacağını denetleyebilirsiniz. Örneğin, şunları yapabilirsiniz:

* İletişimi veya özel iş akışlarını tetiklemek için Azure İşlerinden, Hizmet Veri Götürme Konularına veya Kuyruklara gibi hizmetlere veri gönderin.
* Gerçek zamanlı pano için Power BI panosuna veri gönderin.
* Geçmiş verilere dayalı bir makine öğrenimi modeli eğitmek veya toplu iş analitiği gerçekleştirmek için verileri diğer Azure depolama hizmetlerinde depolayın.

Aşağıdaki resim, verilerin Akış Analizi'ne nasıl gönderildiğini, analiz edildiğini ve depolama veya sunu gibi diğer eylemler için nasıl gönderildiğini gösterir:

![Stream Analytics giriş boru hattı](./media/stream-analytics-introduction/stream-analytics-e2e-pipeline.png)

## <a name="key-capabilities-and-benefits"></a>Temel işlevler ve avantajlar

Azure Stream Analytics kullanımı kolay, esnek, güvenilir ve her boyuttaki iş için ölçeklenebilir olacak şekilde tasarlanmıştır. Birden çok Azure bölgesinde kullanılabilir. Aşağıdaki resim, Azure Akış Analizi'nin temel özelliklerini göstermektedir:

![Stream Analytics temel özellikleri](./media/stream-analytics-introduction/stream-analytics-key-capabilities.png)

## <a name="ease-of-getting-started"></a>Başlama kolaylığı

Azure Akış Analizi'ni başlatmak kolaydır. Birden çok kaynağa ve lavaboya bağlanmak için yalnızca birkaç tıklama alır ve uçtan uca bir ardışık kaynak oluşturur. Akış Analizi, geçmiş verileri almak için [Azure Event Hub'larına](/azure/event-hubs/) ve [Azure IoT](/azure/iot-hub/) Hub'ına ve azure [Blob depolama sına](/azure/storage/common/storage-introduction) bağlanabilir. İş girişi, arama işlemleri gerçekleştirmek için veri akışına katılabileceğiniz Azure Blob depolama veya [SQL Veritabanı'ndan](stream-analytics-use-reference-data.md#azure-sql-database) statik veya yavaş değişen başvuru verilerini de içerebilir.

Akış Analizi, iş çıktısını [Azure Blob depolama,](/azure/storage/common/storage-introduction) [Azure SQL Veritabanı,](/azure/sql-database/) [Azure Veri Gölü Deposu](/azure/data-lake-store/)ve Azure [CosmosDB](/azure/cosmos-db/introduction)gibi birçok depolama sistemine yönlendirebilir. Azure HDInsight ile depolanan çıktıda toplu iş analitiği çalıştırabilir veya çıktıyı tüketim için Olay Hub'ları veya gerçek zamanlı görselleştirme için [Power BI](https://docs.microsoft.com/power-bi/) gibi başka bir hizmete gönderebilirsiniz.

Akış Analizi çıktılarının tüm listesi için Azure [Akış Analizi'nden çıkışları anlayın](stream-analytics-define-outputs.md)bölümüne bakın.

## <a name="programmer-productivity"></a>Programcı üretkenliği

Azure Akış Analizi, hareket halindeki verileri analiz etmek için güçlü zamansal kısıtlamalarla artırılmış basit bir SQL tabanlı sorgu dili kullanır. İş dönüşümlerini tanımlamak için, basit SQL yapıları kullanarak karmaşık geçici sorgular ve analizler yazmanıza olanak tanıyan basit, bildirim temelli bir [Stream Analytics sorgu dili](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference) kullanırsınız. Akış Analizi sorgu dili SQL diliyle tutarlı olduğundan, SQL'e aşinalık iş oluşturmaya başlamak için yeterlidir. Ayrıca Azure PowerShell, [Stream Analytics Visual Studio araçları,](stream-analytics-tools-for-visual-studio-install.md)Stream [Analytics Visual Studio Kodu uzantısı](quick-create-vs-code.md)veya Azure Kaynak Yöneticisi şablonları gibi geliştirici araçlarını kullanarak iş oluşturabilirsiniz. Geliştirici araçlarının kullanılması, çevrimdışı ortamda dönüşüm sorguları geliştirmenize ve [CI/CD işlem hattı](stream-analytics-tools-for-visual-studio-cicd.md) kullanarak Azure’a iş göndermenize olanak tanır.

Akış Analizi sorgu dili, akış verilerini analiz etmek ve işlemek için çok çeşitli işlevler sunar. Bu sorgu dili basit veri işleme, toplama işlevleri ve karmaşık jeouzamsal işlevleri destekler. Portaldaki sorguları düzenleyebilir ve canlı akıştan çıkarılan örnek verileri kullanarak bunları test edebilirsiniz.

Ek işlevler tanımlayıp çağırarak sorgu dilinin yapabileceklerini artırabilirsiniz. Azure Machine Learning çözümlerinden yararlanmak için Azure Machine Learning'deki işlev çağrılarını tanımlayabilir ve Akış Analizi sorgusunun bir parçası olarak karmaşık hesaplamalar gerçekleştirmek için JavaScript veya C# kullanıcı tanımlı işlevleri (UDF' ler) veya kullanıcı tanımlı agregaları entegre edebilirsiniz.

## <a name="fully-managed"></a>Tam olarak yönetilir

Azure Stream Analytics, Azure üzerinde tam olarak yönetilen bir sunucusuz (PaaS) tekliftir. İşlerinizi çalıştırmak için herhangi bir donanım sağlamanız veya kümeleri yönetmeniz gerekmez. Azure Akış Analizi, bulutta karmaşık bilgi işlem kümeleri oluşturarak ve işi yürütmek için gereken performans ayarlamasına dikkat ederek işinizi tam olarak yönetir. Azure Etkinlik Hub'ları ve Azure IoT Hub ile tümleştirme, işinizin bağlı aygıtları, tıklama akışlarını ve günlük dosyalarını içerecek şekilde, bir dizi kaynaktan gelen saniyede milyonlarca olayı yutmasına olanak tanır. Olay Hub'larının bölümleme özelliğini kullanarak, hesaplamaları ölçeklenebilirliği artırmak için daha fazla bölümleme yeteneğine sahip mantıksal adımlara bölümlere ayrılabilirsiniz.

## <a name="run-in-the-cloud-or-on-the-intelligent-edge"></a>Bulutta veya akıllı kenarda çalıştırın

Azure Akış Analitiği bulutta, büyük ölçekli analitik için veya ultra düşük gecikme sonu analitiği için IoT Edge'de çalıştırılabilir. Azure Akış Analizi, hem bulutta hem de kenarda aynı sorgu dilini kullanır ve geliştiricilerin akış işleme için gerçekten karma mimariler oluşturmasına olanak tanır. 

## <a name="low-total-cost-of-ownership"></a>Düşük toplam sahip olma maliyeti

Bir bulut hizmeti olan Stream Analytics, maliyet için iyileştirilmiştir. Herhangi bir ön maliyet söz konusu değildir - yalnızca [tükettiğiniz akış birimleri](stream-analytics-streaming-unit-consumption.md)ve işlenen veri miktarı için ödeme yapılır. Herhangi bir taahhüt veya küme sağlama gerekli değildir ve iş gereksinimlerinize göre işi yukarı veya aşağı ölçeklendirebilirsiniz.

## <a name="mission-critical-ready"></a>Görev kritik hazır

Azure Akış Analizi, dünya çapında birden fazla bölgede kullanılabilir ve güvenilirlik, güvenlik ve uyumluluk gereksinimlerini destekleyerek görev açısından kritik iş yüklerini çalıştırmak üzere tasarlanmıştır.

### <a name="reliability"></a>Güvenilirlik

Azure Akış Analizi, olayların tam olarak bir kez işlenmesini ve en az bir kez etkinlikleri teslim ini garanti eder, böylece olaylar asla kaybolmaz. [Olay Teslim Garantileri'nde](/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics)açıklandığı gibi, tam olarak bir kez işleme, seçili çıktı ile garanti edilir.

Azure Akış Analizi, bir etkinliğin tesliminin başarısız olması durumunda yerleşik kurtarma özelliklerine sahiptir. Akış Analizi ayrıca işinizin durumunu korumak için yerleşik denetim noktaları sağlar ve yinelenebilir sonuçlar sağlar.

Yönetilen bir hizmet olarak Stream Analytics, etkinlik işlemeyi dakika parçalı bir düzeyde %99,9 kullanılabilirlik ile garanti eder. Daha fazla bilgi için [Stream Analytics SLA](https://azure.microsoft.com/support/legal/sla/stream-analytics/v1_0/) sayfasına bakın. 

### <a name="security"></a>Güvenlik

Azure Stream Analytics, güvenlik açısından gelen ve giden tüm iletişimleri şifreler ve TLS 1.2'yi destekler. Yerleşik denetim noktaları da şifrelenir. Akış Analizi, tüm işlemler bellekte yapıldığından gelen verileri depolamaz.

### <a name="compliance"></a>Uyumluluk

Azure Akış Analizi, Azure uyumluluğuna [genel bakışta](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942)açıklandığı gibi birden çok uyumluluk sertifikası izler. 

## <a name="performance"></a>Performans

Akış Analizi saniyede milyonlarca olayı işleyebilir ve ultra düşük gecikmelerle sonuçlar sunabilir. Büyük gerçek zamanlı ve karmaşık olay işleme uygulamalarını işlemek için ölçeklendirmeve ölçeklendirme yapmanızı sağlar. Akış Analizi, karmaşık sorguların birden çok akış düğümünde paralelleştirilmesine ve yürütülmesine olanak sağlayarak bölümleme yaparak daha yüksek performansı destekler. Azure Akış Analizi, Microsoft Research ile işbirliği içinde geliştirilen yüksek performanslı bellek içi akış analitiği motoru [Trill](https://github.com/Microsoft/Trill)üzerine kurulmuştur.

## <a name="next-steps"></a>Sonraki adımlar

Azure Stream Analytics’e genel bakışı gördünüz. Bundan sonra derinlere inerek ilk Stream Analytics işinizi oluşturabilirsiniz:

* [Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md).
* [Azure PowerShell kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-powershell.md).
* [Visual Studio'u kullanarak Bir Stream Analytics işi oluşturun.](stream-analytics-quick-create-vs.md)
* [Visual Studio Code'u kullanarak Bir Akış Analizi işi oluşturun.](quick-create-vs-code.md)
