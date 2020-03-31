---
title: Azure Veri Gölü Depolama Gen1 - performans ayarı
description: Performans için Azure Veri Gölü Depolama Gen1'i nasıl ayarlayabildiğini açıklar.
author: stewu
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 06/30/2017
ms.author: stewu
ms.openlocfilehash: 2521700e0f07691541ee6cbbf085a8be72f08129
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73904617"
---
# <a name="tune-azure-data-lake-storage-gen1-for-performance"></a>Performans için Azure Veri Gölü Depolama Gen1'i ayarlayın

Data Lake Storage Gen1, G/Ç yoğun analitik ve veri hareketi için yüksek iş gücü sağlar. Data Lake Storage Gen1'de, kullanılabilir tüm iş verilerini kullanarak (saniyede okunabilen veya yazilebilen veri miktarı) en iyi performansı elde etmek için önemlidir. Bu mümkün olduğunca paralel olarak çok okuma ve yazma gerçekleştirerek elde edilir.

![Veri Gölü Depolama Gen1 performansı](./media/data-lake-store-performance-tuning-guidance/throughput.png)

Veri Gölü Depolama Gen1, tüm analiz senaryosu için gerekli iş verisini sağlamak için ölçeklendirilebilir. Varsayılan olarak, Bir Veri Gölü Depolama Gen1 hesabı, geniş bir kullanım alanı nın gereksinimlerini karşılamak için otomatik olarak yeterli iş verisini sağlar. Müşterilerin varsayılan sınırla koştuğu durumlarda, Veri Gölü Depolama Gen1 hesabı Microsoft desteğine başvurarak daha fazla iş kaynağı sağlayacak şekilde yapılandırılabilir.

## <a name="data-ingestion"></a>Veri alımı

Bir kaynak sistemden Veri Gölü Depolama Gen1'e veri alırken, Kaynak donanımın, kaynak ağ donanımınve Veri Gölü Depolama Gen1'e ağ bağlantısının darboğaz olabileceğini göz önünde bulundurmanız önemlidir.

![Veri Gölü Depolama Gen1 performansı](./media/data-lake-store-performance-tuning-guidance/bottleneck.png)

Veri hareketinin bu faktörlerden etkilenmediğinden emin olmak önemlidir.

### <a name="source-hardware"></a>Kaynak donanım

İster azure'da şirket içi makineler veya VM'ler kullanıyor olun, uygun donanımı dikkatle seçmelisiniz. Kaynak Disk Donanımı için SSD'leri HDD'lere tercih edin ve daha hızlı iğ ile disk donanımı seçin. Kaynak Ağ Donanımı için mümkün olan en hızlı NIC'leri kullanın. Azure'da, uygun şekilde güçlü disk ve ağ donanımına sahip Azure D14 VM'leri öneririz.

### <a name="network-connectivity-to-data-lake-storage-gen1"></a>Veri Gölü Depolama Gen1'e ağ bağlantısı

Kaynak verileriniz ile Veri Gölü Depolama Gen1 arasındaki ağ bağlantısı bazen darboğaz olabilir. Kaynak verileriniz Şirket içindeyken, [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) ile özel bir bağlantı kullanmayı düşünün. Kaynak verileriniz Azure'daysa, veriler Veri Gölü Depolama Gen1 hesabıyla aynı Azure bölgesinde olduğunda performans en iyi sidir.

### <a name="configure-data-ingestion-tools-for-maximum-parallelization"></a>Maksimum paralelleştirme için veri alma araçlarını yapılandırma

Kaynak donanım ve ağ bağlantısı darboğazlarını ele aldıktan sonra, yutma araçlarınızı yapılandırmaya hazırsınız. Aşağıdaki tablo, birkaç popüler yutma aracının anahtar ayarlarını özetler ve bunlar için derinlemesine performans ayar makaleleri sağlar. Senaryonuz için hangi aracı kullanacağınız hakkında daha fazla bilgi edinmek için bu [makaleyi](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-data-scenarios)ziyaret edin.

| Araç          | Ayarlar | Daha Fazla Ayrıntı                                                                 |
|--------------------|------------------------------------------------------|------------------------------|
| PowerShell       | PerFileThreadCount, EşzamanlıDosya Sayısı | [Bağlantı](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-powershell) |
| AdlCopy    | Azure Veri Gölü Analizi birimleri | [Bağlantı](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob#performance-considerations-for-using-adlcopy)         |
| DistCp            | -m (mapper) | [Bağlantı](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-wasb-distcp#performance-considerations-while-using-distcp)                             |
| Azure Data Factory| parallelKopya | [Bağlantı](../data-factory/copy-activity-performance.md)                          |
| Sqoop           | fs.azure.block.size, -m (mapper) | [Bağlantı](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/)        |

## <a name="structure-your-data-set"></a>Veri kümenizi yapıla

Veriler Veri Gölü Depolama Gen1'de depolandığında, dosya boyutu, dosya sayısı ve klasör yapısı performansı etkiler. Aşağıdaki bölümde bu alanlardaki en iyi uygulamalar açıklanmaktadır.

### <a name="file-size"></a>Dosya boyutu

Tipik olarak, HDInsight ve Azure Data Lake Analytics gibi analiz motorlarının dosya başına yükü vardır. Verilerinizi çok sayıda küçük dosya olarak depolarsanız, bu performansı olumsuz etkileyebilir.

Genel olarak, daha iyi performans için verilerinizi daha büyük boyutlu dosyalar halinde düzenleyin. Genel bir kural olarak, 256 MB veya daha büyük dosyalarda veri kümelerini düzenleyin. Görüntüler ve ikili veriler gibi bazı durumlarda, bunları paralel olarak işlemek mümkün değildir. Bu gibi durumlarda, tek tek dosyaları 2 GB altında tutmak için önerilir.

Bazen, veri ardışık lıkları çok sayıda küçük dosyaya sahip ham veriler üzerinde sınırlı denetime sahiptir. Akış aşağı uygulamaları için kullanılacak daha büyük dosyalar üreten bir "pişirme" işlemine sahip olması önerilir.

### <a name="organize-time-series-data-in-folders"></a>Zaman serisi verilerini klasörlerde düzenleme

Hive ve ADLA iş yükleri için, zaman serisi verilerin bölüm budama bazı sorgular performansı artırır verilerin yalnızca bir alt kümesi okumayardımcı olabilir.

Zaman serisi verilerini sindiren bu ardışık hatlar, genellikle dosyalarını dosya ve klasörler için yapılandırılmış bir adlandırmayla yerlebir eder. Tarihe göre yapılandırılan veriler için gördüğümüz yaygın bir örnek aşağıda verilmiştir:

    \DataSet\YYYY\MM\DD\datafile_YYYY_MM_DD.tsv

Tarih saati bilgilerinin hem klasör olarak hem de dosya adında göründüğüne dikkat edin.

Tarih ve saat için aşağıdaki ortak bir desendir

    \DataSet\YYYY\MM\DD\HH\mm\datafile_YYYY_MM_DD_HH_mm.tsv

Yine, klasör ve dosya organizasyonu ile yaptığınız seçim, daha büyük dosya boyutları ve her klasörde dosyaların makul sayıda için optimize etmelidir.

## <a name="optimize-io-intensive-jobs-on-hadoop-and-spark-workloads-on-hdinsight"></a>HDInsight'ta Hadoop ve Spark iş yüklerinde G/Ç yoğun işleri optimize edin

İşler aşağıdaki üç kategoriden birine girer:

* **CPU yoğun.** Bu işlerin en az G/Ç süreleri ile uzun hesaplama süreleri var. Örnekler makine öğrenimi ve doğal dil işleme işleri içerir.
* **Hafıza yoğun.** Bu işler çok fazla bellek kullanır. Örnek olarak PageRank ve gerçek zamanlı analiz işleri verilebilir.
* **G/Ç yoğun.** Bu işler zamanlarının çoğunu G/Ç yaparak geçirirler. Yaygın bir örnek, yalnızca okuma ve yazma işlemleri yapan bir kopyalama işidir. Diğer örnekler arasında çok sayıda veri okuyan, bazı veri dönüşümü gerçekleştiren ve verileri depoya geri yazan veri hazırlama işleri yer almaktadır.

Aşağıdaki kılavuz yalnızca G/Ç yoğun işler için geçerlidir.

### <a name="general-considerations-for-an-hdinsight-cluster"></a>BIR HDInsight kümesi için genel hususlar

* **HDInsight sürümleri.** En iyi performans için HDInsight'ın en son sürümünden yararlanın.
* **Bölge.** Veri Gölü Depolama Gen1 hesabını HDInsight kümesiyle aynı bölgeye yerleştirin.

HDInsight kümesi iki baş düğümü ve bazı alt düğümlerden oluşur. Her alt düğüm, VM türütarafından belirlenen belirli sayıda çekirdek ve bellek sağlar. Bir işi çalıştırırken, IPN kullanılabilir belleği ve çekirdekleri kapsayıcı oluşturmak için ayıran kaynak arabulucusudur. Her kapsayıcı işi tamamlamak için gereken görevleri çalıştırın. Kapsayıcılar, görevleri hızlı bir şekilde işlemek için paralel olarak çalışır. Bu nedenle, performans mümkün olduğunca çok paralel kapsayıcı çalıştırılarak geliştirilir.

Bir HDInsight kümesiiçinde kapsayıcı sayısını artırmak ve kullanılabilir tüm iş artışlarını kullanacak şekilde ayarlanabilen üç katman vardır.

* **Fiziksel katman**
* **İplik tabakası**
* **İş yükü katmanı**

### <a name="physical-layer"></a>Fiziksel katman

**Daha fazla düğüm ve/veya daha büyük boyutlu VM'ler içeren küme çalıştırın.** Daha büyük bir küme, aşağıdaki resimde gösterildiği gibi daha fazla İplik kapsayıcısı çalıştırmanızı sağlar.

![Veri Gölü Depolama Gen1 performansı](./media/data-lake-store-performance-tuning-guidance/VM.png)

**Daha fazla ağ bant genişliğine sahip VM'leri kullanın.** Veri Gölü Depolama Gen1 iş kaynağından daha az ağ bant genişliği varsa, ağ bant genişliği miktarı bir darboğaz olabilir. Farklı VM'ler farklı ağ bant genişliği boyutlarına sahip olacaktır. Mümkün olan en büyük ağ bant genişliğine sahip bir VM türü seçin.

### <a name="yarn-layer"></a>İplik tabakası

**Daha küçük İplik kapları kullanın.** Aynı miktarda kaynakiçeren daha fazla kapsayıcı oluşturmak için her İPLIK kapsayıcısının boyutunu küçültün.

![Veri Gölü Depolama Gen1 performansı](./media/data-lake-store-performance-tuning-guidance/small-containers.png)

İş yükünüze bağlı olarak, her zaman gereken minimum İplik konteyner boyutu olacaktır. Çok küçük bir kapsayıcı seçerseniz, işleriniz bellek dışı sorunlarla karşınıza çıkar. Genellikle İplik kapları 1 GB'dan küçük olmamalıdır. 3 GB İplik kapları görmek yaygındır. Bazı iş yükleri için daha büyük İplik kaplarına ihtiyacınız olabilir.

**İplik kabı başına çekirdekleri artırın.** Her kapsayıcıda çalışan paralel görev sayısını artırmak için her kapsayıcıya ayrılan çekirdek sayısını artırın. Bu, kapsayıcı başına birden çok görev çalıştıran Spark gibi uygulamalar için çalışır. Her kapta tek bir iş parçacığı çalıştıran Hive gibi uygulamalar için, konteyner başına daha fazla çekirdek yerine daha fazla kapsayıcı olması daha iyidir.

### <a name="workload-layer"></a>İş yükü katmanı

**Kullanılabilir tüm kapsayıcıları kullanın.** Tüm kaynakların kullanılabileceğini, kullanılabilir kapsayıcı ların sayısının eşit veya daha büyük olmasını sağlayın.

![Veri Gölü Depolama Gen1 performansı](./media/data-lake-store-performance-tuning-guidance/use-containers.png)

**Başarısız görevler pahalıya mal olur.** Her görevin işlenmesi gereken büyük miktarda verivarsa, görevin başarısız olması pahalı bir yeniden denemeyle sonuçlanır. Bu nedenle, her biri az miktarda veriyi işleyen daha fazla görev oluşturmak daha iyidir.

Yukarıdaki genel yönergelere ek olarak, her uygulamanın bu belirli uygulama için ayarlamak için farklı parametreleri vardır. Aşağıdaki tabloda, her uygulama için performans atonlama ile başlamak için bazı parametreler ve bağlantılar listelenizdir.

| İş yükü               | Görevleri ayarlamak için parametre                                                         |
|--------------------|-------------------------------------------------------------------------------------|
| [HDInsight’ta Spark](data-lake-store-performance-tuning-spark.md)  | <ul><li>Num-uygulayıcılar</li><li>Yürütücü-bellek</li><li>Executor çekirdekleri</li></ul> |
| [HDInsight'ta Kovan](data-lake-store-performance-tuning-hive.md)    | <ul><li>kovan.tez.container.size</li></ul>         |
| [HDInsight'ta MapReduce](data-lake-store-performance-tuning-mapreduce.md)            | <ul><li>Mapreduce.map.memory</li><li>Mapreduce.job.maps</li><li>Mapreduce.reduce.memory</li><li>Mapreduce.job.azaltır</li></ul> |
| [HDInsight üzerinde Storm](data-lake-store-performance-tuning-storm.md)| <ul><li>İşçi işlemlerinin sayısı</li><li>Emzit uygulayıcı örneklerinin sayısı</li><li>Cıvata uygulayıcı örneklerinin sayısı </li><li>Emzit görevlerinin sayısı</li><li>Cıvata görev sayısı</li></ul>|

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Veri Gölü Depolama Gen1'e Genel Bakış](data-lake-store-overview.md)
* [Azure Data Lake Analytics ile Çalışmaya Başlama](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
