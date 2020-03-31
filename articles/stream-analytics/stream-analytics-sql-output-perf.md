---
title: Azure Akış Analizi çıktısı Azure SQL Veritabanına
description: Azure Akış Analizi'nden SQL Azure'a veri çıkarma hakkında bilgi edinin ve daha yüksek yazma iş akışı oranları elde edin.
author: chetanmsft
ms.author: chetang
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/18/2019
ms.openlocfilehash: f68f973882af28d80b3a27bc4591c5ee932404a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75443612"
---
# <a name="azure-stream-analytics-output-to-azure-sql-database"></a>Azure Akış Analizi çıktısı Azure SQL Veritabanına

Bu makalede, Azure Akış Analitiği kullanarak SQL Azure Veritabanı'na veri yüklerken daha iyi yazma iş akışı performansı elde etmek için ipuçları anlatılmaktadır.

Azure Akış Analytics'teki SQL çıktısı, seçenek olarak paralel yazmayı destekler. Bu seçenek, birden çok çıktı bölümü paralel olarak hedef tabloya yazıyor, [tam paralel](stream-analytics-parallelization.md#embarrassingly-parallel-jobs) iş topolojileri için izin verir. Ancak, SQL Azure veritabanı yapılandırmanıza ve tablo şemanıza önemli ölçüde bağlı olduğundan, bu seçeneği Azure Akış Analizi'nde etkinleştirmek daha yüksek iş akışları elde etmek için yeterli olmayabilir. Dizin seçimi, kümeleme anahtarı, dizin dolgu faktörü ve sıkıştırma tabloları yüklemek için zaman üzerinde bir etkisi vardır. İç kıyaslamalara dayalı sorgu ve yük performansını geliştirmek için SQL Azure veritabanınızı nasıl optimize edeceğiniz hakkında daha fazla bilgi için [SQL veritabanı performans kılavuzuna](../sql-database/sql-database-performance-guidance.md)bakın. SQL Azure Veritabanı'na paralel olarak yazarken yazma sırası garanti edilmez.

Her hizmette, çözümünüzün genel iş hacmini iyileştirmeye yardımcı olabilecek bazı yapılandırmalar ve bunlar aşağıda veda edebilirsiniz.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

- **Devralma Partitioning** – Bu SQL çıkış yapılandırma seçeneği, önceki sorgu adımınızın veya girişinizin bölümleme düzeninin devralılmasını sağlar. Bu etkinle, disk tabanlı bir tabloya yazma ve işiniz için [tamamen paralel](stream-analytics-parallelization.md#embarrassingly-parallel-jobs) bir topolojiye sahip, daha iyi iş araları görmeyi bekleyin. Bu bölümleme zaten otomatik olarak diğer birçok [çıktıiçin](stream-analytics-parallelization.md#partitions-in-sources-and-sinks)olur. Tablo kilitleme (TABLOCK) da bu seçenek ile yapılan toplu ekler için devre dışı bırakılır.

> [!NOTE] 
> 8'den fazla giriş bölümü olduğunda, giriş bölümleme düzenini devralmak uygun bir seçim olmayabilir. Bu üst sınır, tek bir kimlik sütunu ve kümelenmiş dizin içeren bir tabloda gözlendi. Bu durumda, çıktı yazarlarının sayısını açıkça belirtmek için sorgunuzda [INTO](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics#into-shard-count) 8'i kullanmayı düşünün. Şemanıza ve dizin seçiminize bağlı olarak, gözlemleriniz değişebilir.

- **Toplu İşlem Boyutu** - SQL çıktı yapılandırması, hedef tablonuzun/iş yükünüzün yapısına bağlı olarak bir Azure Akışı Analytics SQL çıkışında maksimum toplu iş boyutunu belirtmenize olanak tanır. Toplu iş boyutu, her toplu ekleme hareketiyle gönderilen en yüksek kayıt sayısıdır. Kümelenmiş sütun deposu dizinlerinde, [100K](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-data-loading-guidance) civarındaki toplu iş boyutları daha fazla paralelleştirme, en az günlüğe kaydetme ve kilitleme optimizasyonlarına olanak sağlar. Disk tabanlı tablolarda, daha yüksek toplu iş boyutları toplu kesici uçlar sırasında kilit yükselmesini tetikleebileceğiiçin, 10K (varsayılan) veya daha düşük çözüm için en uygun olabilir.

- **Giriş İleti Tuning** - Devralma bölümleme ve toplu iş boyutu kullanarak en iyi duruma geldiyseniz, bölüm başına ileti başına giriş olayı sayısını artırmak yazma iş inizi daha da zorlamanıza yardımcı olur. Giriş iletisi atoması, Azure Akış Analizi'ndeki toplu iş boyutlarının belirtilen Toplu İşlem Boyutuna kadar olmasını sağlayarak iş akışını artırır. Bu, EventHub veya Blob'da [sıkıştırma](stream-analytics-define-inputs.md) veya giriş iletisi boyutlarını artırarak elde edilebilir.

## <a name="sql-azure"></a>SQL Azure

- **Bölümlenmiş Tablo ve Dizinler** – [Bölümlenmiş](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes?view=sql-server-2017) BIR SQL tablosu ve bölümleme dizinleri kullanarak bölüm anahtarı (örneğin, PartitionId) ile aynı sütun ile tabloda yazma sırasında bölümler arasındaki çekişmeleri önemli ölçüde azaltabilir. Bölümlenmiş bir tablo için, BIRINCIL dosya grubunda bir [bölüm işlevi](https://docs.microsoft.com/sql/t-sql/statements/create-partition-function-transact-sql?view=sql-server-2017) ve bir [bölüm düzeni](https://docs.microsoft.com/sql/t-sql/statements/create-partition-scheme-transact-sql?view=sql-server-2017) oluşturmanız gerekir. Bu, yeni veriler yüklenirken varolan verilerin kullanılabilirliğini de artırır. Günlük IO sınırı, SKU yükseltilerek artırılabilen bölüm sayısına bağlı olarak vurulabilir.

- **Benzersiz anahtar ihlallerinden kaçının** - Azure Akışı Analizi Etkinlik Günlüğü'nde [birden çok önemli ihlal uyarı iletisi](stream-analytics-troubleshoot-output.md#key-violation-warning-with-azure-sql-database-output) alırsanız, işinizin kurtarma durumlarda meydana gelebilecek benzersiz kısıtlama ihlallerinden etkilenmediğinden emin olun. Dizinlerinizde [DUP\_\_KEY](stream-analytics-troubleshoot-output.md#key-violation-warning-with-azure-sql-database-output) seçeneğini ayarlayarak bu önlenebilir.

## <a name="azure-data-factory-and-in-memory-tables"></a>Azure Veri Fabrikası ve Bellek İçi Tablolar

- **Geçici tablo olarak Bellek İçi Tablo** – [Bellek İçi tablolar](/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) çok yüksek hızlı veri yüklerine izin verir, ancak verilerin belleğe sığması gerekir. Kıyaslamalar, bellek içi tablodan disk tabanlı tabloya toplu yüklemenin, kimlik sütunu ve kümelenmiş diziniçeren tek bir yazar kullanarak doğrudan toplu eklemeden yaklaşık 10 kat daha hızlı olduğunu gösterir. Bu toplu ekleme performansından yararlanmak için, bellek teki tablodan disk tabanlı tabloya verileri kopyalayan [Azure Veri Fabrikası'nı kullanarak](../data-factory/connector-azure-sql-database.md) bir kopyalama işi ayarlayın.

## <a name="avoiding-performance-pitfalls"></a>Performans Tuzaklarından Kaçınma
Toplu veri ekleme, verileri tek eklerile yüklemekten çok daha hızlıdır, çünkü verileri aktaran, ekss ekstresini ayrıştıran, ekstreyi çalıştıran ve işlem kaydı veren yinelenen ek yükü önlenir. Bunun yerine, verileri akışı için depolama motoruna daha verimli bir yol kullanılır. Ancak, bu yolun kurulum maliyeti, disk tabanlı tablodaki tek bir ekleme deyiminden çok daha yüksektir. Başa baş noktası genellikle 100 satır civarındadır ve bunun ötesinde toplu yükleme hemen hemen her zaman daha verimlidir. 

Gelen olay oranı düşükse, toplu eklemeyi verimsiz hale getiren ve çok fazla disk alanı kullanan 100 satırdan daha düşük toplu boyutlar oluşturabilir. Bu sınırlamayı aşmak için aşağıdaki eylemlerden birini yapabilirsiniz:
* Her satır için basit kesici uç kullanmak için YERINE [tetikleyici](/sql/t-sql/statements/create-trigger-transact-sql) oluşturun.
* Önceki bölümde açıklandığı gibi Bellek içi geçici tablo kullanın.

Başka bir tür senaryo, kümelenmiş olmayan bir sütun deposu dizinine (NCCI) yazarken oluşur ve burada küçük toplu ekler çok fazla kesim oluşturabilir ve bu durum dizini çökertebilir. Bu durumda, öneri yerine Kümelenmiş Sütun deposu dizini kullanmaktır.

## <a name="summary"></a>Özet

Özetle, SQL çıktısı için Azure Stream Analytics'teki bölümlenmiş çıktı özelliği yle, işinizin SQL Azure'da bölümlenmiş bir tabloyla hizalanmış paralelleştirilmesi size önemli iş akışı iyileştirmeleri sunmalıdır. Bellek içi tablodan Disk tabanlı tablolara veri hareketi düzenlemek için Azure Veri Fabrikası'ndan yararlanmak, büyüklük elde etme kazançları nın sırası verebilir. Mümkünse, ileti yoğunluğunu artırmak da genel iş buzuniyileştirilmesinde önemli bir faktör olabilir.
