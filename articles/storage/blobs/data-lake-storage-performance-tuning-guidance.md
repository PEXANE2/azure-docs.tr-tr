---
title: Performans için Azure Veri Gölü Depolama Gen2'yi optimize edin | Microsoft Dokümanlar
description: Azure Veri Gölü Depolama Gen2 Performans Tuning Yönergeleri
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: f1a16228b72d7e0f45048669ade94a0c78d9ac52
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74327936"
---
# <a name="optimize-azure-data-lake-storage-gen2-for-performance"></a>Performans için Azure Veri Gölü Depolama Gen2'yi optimize edin

Azure Veri Gölü Depolama Gen2, Yoğun G/Ç yoğun analitik ve veri hareketi için yüksek iş gücü sağlar.  Data Lake Storage Gen2'de, kullanılabilir tüm iş verilerini kullanarak (saniyede okunabilen veya yazilebilen veri miktarı) en iyi performansı elde etmek için önemlidir.  Bu mümkün olduğunca paralel olarak çok okuma ve yazma gerçekleştirerek elde edilir.

![Veri Gölü Depolama Gen2 performansı](./media/data-lake-storage-performance-tuning-guidance/throughput.png)

Veri Gölü Depolama Gen2, tüm analiz senaryosu için gerekli iş verisini sağlamak için ölçeklendirilebilir. Varsayılan olarak, Bir Veri Gölü Depolama Gen2 hesabı, geniş bir kullanım alanı nın gereksinimlerini karşılamak için otomatik olarak yeterli iş verisini sağlar. Müşterilerin varsayılan sınırla koştuğu durumlarda, Veri Gölü Depolama Gen2 hesabı [Azure Desteği'ne](https://azure.microsoft.com/support/faq/)başvurarak daha fazla iş kaynağı sağlayacak şekilde yapılandırılabilir.

## <a name="data-ingestion"></a>Veri alımı

Bir kaynak sistemden Veri Gölü Depolama Gen2'ye veri alırken, Kaynak donanımın, kaynak ağ donanımınve Veri Gölü Depolama Gen2'ye ağ bağlantısının darboğaz olabileceğini göz önünde bulundurmak önemlidir.  

![Veri Gölü Depolama Gen2 performansı](./media/data-lake-storage-performance-tuning-guidance/bottleneck.png)

Veri hareketinin bu faktörlerden etkilenmediğinden emin olmak önemlidir.

### <a name="source-hardware"></a>Kaynak donanım

Azure'da şirket içi makineleri veya VM'leri kullanıyor olun, uygun donanımı dikkatle seçmelisiniz. Kaynak Disk Donanımı için SSD'leri HDD'lere tercih edin ve daha hızlı iğ ile disk donanımı seçin. Kaynak Ağ Donanımı için mümkün olan en hızlı NIC'leri kullanın.  Azure'da, uygun şekilde güçlü disk ve ağ donanımına sahip Azure D14 VM'leri öneririz.

### <a name="network-connectivity-to-data-lake-storage-gen2"></a>Veri Gölü Depolama Gen2'ye ağ bağlantısı

Kaynak verileriniz ile Veri Gölü Depolama Gen2 arasındaki ağ bağlantısı bazen darboğaz olabilir. Kaynak verileriniz Şirket içindeyken, [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) ile özel bir bağlantı kullanmayı düşünün. Kaynak verileriniz Azure'daysa, veriler Veri Gölü Depolama Gen2 hesabıyla aynı Azure bölgesinde olduğunda performans en iyi sidir.

### <a name="configure-data-ingestion-tools-for-maximum-parallelization"></a>Maksimum paralelleştirme için veri alma araçlarını yapılandırma

Yukarıdaki kaynak donanım ve ağ bağlantısı darboğazlarını ele aldıktan sonra, yutma araçlarınızı yapılandırmaya hazırsınız. Aşağıdaki tablo, birkaç popüler yutma aracının anahtar ayarlarını özetler ve bunlar için derinlemesine performans ayar makaleleri sağlar.  Senaryonuz için hangi aracı kullanacağınız hakkında daha fazla bilgi edinmek için bu [makaleyi](data-lake-storage-data-scenarios.md)ziyaret edin.

| Araç               | Ayarlar     | Daha Fazla Ayrıntı                                                                 |
|--------------------|------------------------------------------------------|------------------------------|
| DistCp            | -m (mapper)   | [Bağlantı](data-lake-storage-use-distcp.md#performance-considerations-while-using-distcp)                             |
| Azure Data Factory| parallelKopya    | [Bağlantı](../../data-factory/copy-activity-performance.md)                          |
| Sqoop           | fs.azure.block.size, -m (mapper)    |   [Bağlantı](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/)        |

## <a name="structure-your-data-set"></a>Veri kümenizi yapıla

Veriler Veri Gölü Depolama Gen2'de depolandığında, dosya boyutu, dosya sayısı ve klasör yapısı performans üzerinde bir etkiye sahiptir.  Aşağıdaki bölümde bu alanlardaki en iyi uygulamalar açıklanmaktadır.  

### <a name="file-size"></a>Dosya boyutu

Tipik olarak, HDInsight ve Azure Data Lake Analytics gibi analiz motorlarının dosya başına yükü vardır. Verilerinizi çok sayıda küçük dosya olarak depolarsanız, bu performansı olumsuz etkileyebilir. Genel olarak, daha iyi performans için verilerinizi daha büyük boyutlu dosyalar halinde düzenleyin (256MB ile 100 GB boyutu). Bazı motorlar ve uygulamalar, boyutu 100 GB'dan büyük dosyaları verimli bir şekilde işlemekte sorun olabilir.

Bazen, veri ardışık lıkları çok sayıda küçük dosyaya sahip ham veriler üzerinde sınırlı denetime sahiptir. Akış aşağı uygulamaları için kullanılacak daha büyük dosyalar üreten bir "pişirme" işlemine sahip olması önerilir.

### <a name="organizing-time-series-data-in-folders"></a>Zaman serisi verilerini klasörlerde düzenleme

Hive iş yükleri için, zaman serisi verilerinin bölüm budama bazı sorgular performansı artırır verilerin yalnızca bir alt kümesi okumayardımcı olabilir.    

Zaman serisi verilerini sindiren bu ardışık hatlar, genellikle dosyalarını dosya ve klasörler için çok yapılandırılmış bir adlandırma yla yerlebir eder. Tarihe göre yapılandırılan veriler için gördüğümüz çok yaygın bir örnek aşağıda verilmiştir:

    \DataSet\YYYY\MM\DD\datafile_YYYY_MM_DD.tsv

Tarih saati bilgilerinin hem klasör olarak hem de dosya adında göründüğüne dikkat edin.

Tarih ve saat için aşağıdaki ortak bir desendir

    \DataSet\YYYY\MM\DD\HH\mm\datafile_YYYY_MM_DD_HH_mm.tsv

Yine, klasör ve dosya organizasyonu ile yaptığınız seçim, daha büyük dosya boyutları ve her klasörde dosyaların makul sayıda için optimize etmelidir.

## <a name="optimizing-io-intensive-jobs-on-hadoop-and-spark-workloads-on-hdinsight"></a>HDInsight'ta Hadoop ve Spark iş yüklerinde G/Ç yoğun işlerinin optimizasyonu

İşler aşağıdaki üç kategoriden birine girer:

* **CPU yoğun.**  Bu işlerin en az G/Ç süreleri ile uzun hesaplama süreleri var.  Örnekler makine öğrenimi ve doğal dil işleme işleri içerir.  
* **Hafıza yoğun.**  Bu işler çok fazla bellek kullanır.  Örnek olarak PageRank ve gerçek zamanlı analiz işleri verilebilir.  
* **G/Ç yoğun.**  Bu işler zamanlarının çoğunu G/Ç yaparak geçirirler.  Yaygın bir örnek, yalnızca okuma ve yazma işlemleri yapan bir kopyalama işidir.  Diğer örnekler arasında çok fazla veri okuyan, bazı veri dönüşümü gerçekleştiren ve sonra verileri depoya geri yazan veri hazırlama işleri yer almaktadır.  

Aşağıdaki kılavuz yalnızca G/Ç yoğun işler için geçerlidir.

## <a name="general-considerations"></a>Dikkat edilmesi gereken temel noktalar

Tek bir işlemde 100 MB'a kadar okuyan veya yazan bir işiniz olabilir, ancak bu boyuttaki bir arabellek performansı tehlikeye atabilir.
Performansı en iyi duruma getirmek için, 4MB ile 16MB arasında bir G/Ç işleminin boyutunu korumaya çalışın.

### <a name="general-considerations-for-an-hdinsight-cluster"></a>BIR HDInsight kümesi için genel hususlar

* **HDInsight sürümleri.** En iyi performans için HDInsight'ın en son sürümünden yararlanın.
* **Bölge.** Veri Gölü Depolama Gen2 hesabını HDInsight kümesiyle aynı bölgeye yerleştirin.  

HDInsight kümesi iki baş düğümü ve bazı alt düğümlerden oluşur. Her alt düğüm, VM türütarafından belirlenen belirli sayıda çekirdek ve bellek sağlar.  Bir işi çalıştırırken, IPN kullanılabilir belleği ve çekirdekleri kapsayıcı oluşturmak için ayıran kaynak arabulucusudur.  Her kapsayıcı işi tamamlamak için gereken görevleri çalıştırın.  Kapsayıcılar, görevleri hızlı bir şekilde işlemek için paralel olarak çalışır. Bu nedenle, performans mümkün olduğunca çok paralel kapsayıcı çalıştırılarak geliştirilir.

Bir HDInsight kümesiiçinde kapsayıcı sayısını artırmak ve kullanılabilir tüm iş artışlarını kullanacak şekilde ayarlanabilen üç katman vardır.  

* **Fiziksel katman**
* **İplik tabakası**
* **İş yükü katmanı**

### <a name="physical-layer"></a>Fiziksel Katman

**Daha fazla düğüm ve/veya daha büyük boyutlu VM'ler içeren küme çalıştırın.**  Daha büyük bir küme, aşağıdaki resimde gösterildiği gibi daha fazla İplik kapsayıcısı çalıştırmanızı sağlar.

![Veri Gölü Depolama Gen2 performansı](./media/data-lake-storage-performance-tuning-guidance/VM.png)

**Daha fazla ağ bant genişliğine sahip VM'leri kullanın.**  Veri Gölü Depolama Gen2 iş kaynağından daha az ağ bant genişliği varsa, ağ bant genişliği miktarı bir darboğaz olabilir.  Farklı VM'ler farklı ağ bant genişliği boyutlarına sahip olacaktır.  Mümkün olan en büyük ağ bant genişliğine sahip bir VM türü seçin.

### <a name="yarn-layer"></a>İplik Tabakası

**Daha küçük İplik kapları kullanın.**  Aynı miktarda kaynakiçeren daha fazla kapsayıcı oluşturmak için her İPLIK kapsayıcısının boyutunu küçültün.

![Veri Gölü Depolama Gen2 performansı](./media/data-lake-storage-performance-tuning-guidance/small-containers.png)

İş yükünüze bağlı olarak, her zaman gereken minimum İplik konteyner boyutu olacaktır. Çok küçük bir kapsayıcı seçerseniz, işleriniz bellek dışı sorunlarla karşınıza çıkar. Genellikle İplik kapları 1GB'dan küçük olmamalıdır. 3GB İplik kapları görmek yaygındır. Bazı iş yükleri için daha büyük İplik kaplarına ihtiyacınız olabilir.  

**İplik kabı başına çekirdekleri artırın.**  Her kapsayıcıda çalışan paralel görev sayısını artırmak için her kapsayıcıya ayrılan çekirdek sayısını artırın.  Bu, kapsayıcı başına birden çok görev çalıştıran Spark gibi uygulamalar için çalışır.  Her kapta tek bir iş parçacığı çalıştırmak Hive gibi uygulamalar için, konteyner başına daha fazla çekirdek yerine daha fazla kapsayıcı olması daha iyidir.

### <a name="workload-layer"></a>İş Yükü Katmanı

**Kullanılabilir tüm kapsayıcıları kullanın.**  Tüm kaynakların kullanılabilecek şekilde kullanılabilir kapsayıcı ların sayısına eşit veya daha büyük olacak şekilde görev sayısını ayarlayın.

![Veri Gölü Depolama Gen2 performansı](./media/data-lake-storage-performance-tuning-guidance/use-containers.png)

**Başarısız görevler pahalıya mal olur.** Her görevin işlenmesi gereken büyük miktarda verivarsa, görevin başarısız olması pahalı bir yeniden denemeyle sonuçlanır.  Bu nedenle, her biri az miktarda veriyi işleyen daha fazla görev oluşturmak daha iyidir.

Yukarıdaki genel yönergelere ek olarak, her uygulamanın bu belirli uygulama için ayarlamak için farklı parametreleri vardır. Aşağıdaki tabloda, her uygulama için performans atonlama ile başlamak için bazı parametreler ve bağlantılar listelenizdir.

| İş yükü | Görevleri ayarlamak için parametre |
|----------|------------------------|
| [HDInsight’ta Spark](data-lake-storage-performance-tuning-spark.md) | <ul><li>Num-uygulayıcılar</li><li>Yürütücü-bellek</li><li>Executor çekirdekleri</li></ul> |
| [HDInsight'ta Kovan](data-lake-storage-performance-tuning-hive.md) | <ul><li>kovan.tez.container.size</li></ul> |
| [HDInsight'ta MapReduce](data-lake-storage-performance-tuning-mapreduce.md) | <ul><li>Mapreduce.map.memory</li><li>Mapreduce.job.maps</li><li>Mapreduce.reduce.memory</li><li>Mapreduce.job.azaltır</li></ul> |
| [HDInsight üzerinde Storm](data-lake-storage-performance-tuning-storm.md)| <ul><li>İşçi işlemlerinin sayısı</li><li>Emzit uygulayıcı örneklerinin sayısı</li><li>Cıvata uygulayıcı örneklerinin sayısı </li><li>Emzit görevlerinin sayısı</li><li>Cıvata görev sayısı</li></ul>|

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Veri Gölü Depolama Gen2'ye Genel Bakış](data-lake-storage-introduction.md)
