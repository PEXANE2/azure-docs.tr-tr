---
title: Azure Veri Gölü Depolama Gen1 performans ayarı - MapReduce
description: Azure Veri Gölü Depolama Gen1 MapReduce Performans Tuning Yönergeleri
author: stewu
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: a645049665bc1d51efa94a879b9d2e4e5529282f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73904584"
---
# <a name="performance-tuning-guidance-for-mapreduce-on-hdinsight-and-azure-data-lake-storage-gen1"></a>HDInsight ve Azure Veri Gölü Depolama Gen1'de MapReduce için performans ayarı kılavuzu

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Veri Gölü Depolama Gen1 hesabı.** Nasıl oluşturulacağına ilişkin talimatlar için [bkz.](data-lake-store-get-started-portal.md)
* Veri Gölü Depolama Gen1 hesabına erişim içeren **Azure HDInsight kümesi.** Bkz. [Veri Gölü Depolama Gen1 ile hdinsight kümesi oluşturun.](data-lake-store-hdinsight-hadoop-use-portal.md) Küme için Uzak Masaüstü'nü etkinleştirdiğinizden emin olun.
* **HDInsight'ta MapReduce'ı kullanma**. Daha fazla bilgi için, [HDInsight'ta Hadoop'ta MapReduce'i kullanın](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-mapreduce)
* **Veri Gölü Depolama Gen1 için performans tuning yönergelerigözden geçirin.** Genel performans kavramları için bkz: [Veri Gölü Depolama Gen1 Performans Tuning Kılavuzu](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-performance-tuning-guidance)

## <a name="parameters"></a>Parametreler

MapReduce işlerini çalıştırırken, Veri Gölü Depolama Gen1'de performansı artırmak için yapılandırabileceğiniz en önemli parametreler şunlardır:

|Parametre      | Açıklama  |
|---------|---------|
|`Mapreduce.map.memory.mb`  |  Her haritaciçin ayrılacak bellek miktarı.  |
|`Mapreduce.job.maps`     |  İş başına eşlemi görev sayısı.  |
|`Mapreduce.reduce.memory.mb`     |  Her redüktöre ayrılacak bellek miktarı.  |
|`Mapreduce.job.reduces`    |   İş başına düşen görev sayısını azaltın.  |

### <a name="mapreducemapmemory--mapreducereducememory"></a>Mapreduce.map.memory / Mapreduce.reduce.memory

Bu sayıyı, harita için ne kadar bellek gerektiğine göre ayarlayın ve/veya görevi azaltın. İplik yapılandırması aracılığıyla `mapreduce.map.memory` `mapreduce.reduce.memory` Ambari'nin varsayılan değerlerini görüntüleyebilirsiniz. Ambari'de, İplik'e gidin ve **Configs** sekmesini görüntüleyin. İplik belleği görüntülenir.

### <a name="mapreducejobmaps--mapreducejobreduces"></a>Mapreduce.job.maps / Mapreduce.job.reduces

Bu, oluşturulacak en fazla haritalayıcı veya indirgeyici sayısını belirler. Bölme sayısı, MapReduce işi için kaç harita oluşturun caydığını belirler. Bu nedenle, istenen harita oluşturucu sayısından daha az bölme varsa, istediğinizden daha az harita oluşturucu alabilirsiniz.

## <a name="guidance"></a>Rehber

### <a name="step-1-determine-number-of-jobs-running"></a>Adım 1: Çalışan iş sayısını belirleme

Varsayılan olarak, MapReduce işiniz için tüm kümeyi kullanır. Kullanılabilir kapsayıcılardan daha az harita oluşturucu kullanarak kümenin daha azını kullanabilirsiniz. Bu belgedeki kılavuz, uygulamanızın kümenizde çalışan tek uygulama olduğunu varsayar.

### <a name="step-2-set-mapreducemapmemorymapreducereducememory"></a>Adım 2: mapreduce.map.memory/mapreduce.reduce.memory'i ayarlayın.

Harita ve küçültme görevleri için belleğin boyutu, özel işinize bağlıdır. Eşzamanlılığı artırmak istiyorsanız bellek boyutunu azaltabilirsiniz. Aynı anda çalışan görev sayısı kapsayıcıların sayısına bağlıdır. Haritalayıcı veya azaltıcı başına bellek miktarını azaltarak, daha fazla haritacı veya indirgeyicinin aynı anda çalışmasını sağlayan daha fazla kapsayıcı oluşturulabilir. Bellek miktarını çok fazla azaltmak bazı işlemlerin bellek lerinin tükenmesine neden olabilir. İşinizi çalıştırırken yığın hatası alırsanız, haritalayıcı veya redüktör başına bellek artırın. Daha fazla kapsayıcı eklemenin her ek kapsayıcı için ekstra ek ek yükü eklediğini ve bunun performansı düşürebileceğini göz önünde bulundurun. Başka bir alternatif bellek daha yüksek miktarda veya kümedüğüm sayısını artırarak bir küme kullanarak daha fazla bellek elde etmektir. Daha fazla bellek daha fazla kapsayıcının kullanılmasını sağlar, bu da daha fazla eşzamanlılık anlamına gelir.

### <a name="step-3-determine-total-yarn-memory"></a>Adım 3: Toplam İplik belleği belirleme

mapreduce.job.maps/mapreduce.job.reduces'i ayarlamak için, kullanılabilir toplam İplik belleği miktarını göz önünde bulundurun. Bu bilgiler Ambari'de mevcuttur. İPLik'e gidin ve **Configs** sekmesini görüntüleyin. İplik belleği bu pencerede görüntülenir. Toplam İPlik belleği almak için IPN belleği kümenizdeki düğüm sayısıyla çarpın.

`Total YARN memory = nodes * YARN memory per node`

Boş bir küme kullanıyorsanız, bellek kümenizin toplam İplik belleği olabilir. Diğer uygulamalar bellek kullanıyorsa, haritalayıcı veya indirgeyici sayısını kullanmak istediğiniz kapsayıcı sayısına düşürerek kümenizin belleği yalnızca bir bölümünü kullanmayı seçebilirsiniz.

### <a name="step-4-calculate-number-of-yarn-containers"></a>Adım 4: İplik kaplarının sayısını hesaplama

İplik kapları, iş için kullanılabilir eşzamanlılık miktarını belirler. Toplam İplik belleği alın ve mapreduce.map.memory ile bölün.

`# of YARN containers = total YARN memory / mapreduce.map.memory`

### <a name="step-5-set-mapreducejobmapsmapreducejobreduces"></a>Adım 5: mapreduce.job.maps/mapreduce.job.reduces'ı ayarlayın

Mapreduce.job.maps/mapreduce.job'u ayarlayın.kullanılabilir kapsayıcı sayısı en az azaltır. Daha iyi performans elde edip edindiğinizi görmek için haritacıların ve azaltıcıların sayısını artırarak daha fazla deneme yapabilirsiniz. Çok fazla haritacısıolmasının performansı düşürebileceği için daha fazla haritacısının ek ek yükü olacağını unutmayın.

CPU zamanlama ve CPU yalıtımı varsayılan olarak kapatılır, böylece İPlik kapsayıcılarının sayısı bellekle sınırlandırılır.

## <a name="example-calculation"></a>Örnek hesaplama

Şu anda 8 D14 düğümünden oluşan bir kümeniz olduğunu ve Yoğun Bir G/Ç çalışması yapmak istediğinizi varsayalım. Yapmanız gereken hesaplamalar şunlardır:

### <a name="step-1-determine-number-of-jobs-running"></a>Adım 1: Çalışan iş sayısını belirleme

Örneğin, çalışan tek işin bizim işimiz olduğunu varsayıyoruz.

### <a name="step-2-set-mapreducemapmemorymapreducereducememory"></a>Adım 2: mapreduce.map.memory/mapreduce.reduce.memory'i ayarlayın.

Örneğin, G/Ç yoğun bir iş çalıştırıyorsanız ve harita görevleri için 3 GB bellek yeterli olduğuna karar verebilirsiniz.

`mapreduce.map.memory = 3GB`

### <a name="step-3-determine-total-yarn-memory"></a>Adım 3: Toplam İplik belleği belirleme

`total memory from the cluster is 8 nodes * 96GB of YARN memory for a D14 = 768GB`

### <a name="step-4-calculate--of-yarn-containers"></a>Adım 4: Hesap # İplik kapları

`# of YARN containers = 768 GB of available memory / 3 GB of memory = 256`

### <a name="step-5-set-mapreducejobmapsmapreducejobreduces"></a>Adım 5: mapreduce.job.maps/mapreduce.job.reduces'ı ayarlayın

`mapreduce.map.jobs = 256`

## <a name="limitations"></a>Sınırlamalar

**Veri Gölü Depolama Gen1 azaltma**

Çok kiracılı bir hizmet olarak, Data Lake Storage Gen1 hesap düzeyi bant genişliği sınırlarını belirler. Bu sınırlara ularsanız, görev hatalarını görmeye başlarsınız. Bu, görev günlüklerinde azaltma hataları gözlemleyerek tanımlanabilir. İşiniz için daha fazla bant genişliğine ihtiyacınız varsa, lütfen bize ulaşın.

Daraltılmış olup olmadığınızı kontrol etmek için istemci tarafında hata ayıklama günlüğe kaydetmeyi etkinleştirmeniz gerekir. Bunu şu şekilde yapabilirsiniz:

1. Ambari > YARN > Config > Advanced iplik-log4j: log4j.logger.com.microsoft.azure.datalake.store=DEBUG'daki log4j özelliklerine aşağıdaki özelliği koyun

2. Config'in etkili olması için tüm düğümleri/hizmeti yeniden başlatın.

3. Daralıyorsanız, İPN günlüğü dosyasında HTTP 429 hata kodunu görürsünüz. İplik günlük dosyası /tmp/&lt;&gt;kullanıcı /iplik.log

## <a name="examples-to-run"></a>Çalıştırılabilmek için örnekler

MapReduce'Nin Veri Gölü Depolama Gen1'de nasıl çalıştığını göstermek için aşağıdaki ayarlara sahip bir küme üzerinde çalıştırılabilen bazı örnek kodlar verilmiştir:

* 16 düğüm D14v2
* HDI 3.6 çalışan Hadoop kümesi

Bir başlangıç noktası için, mapreduce Teragen, Terasort ve Teravalidate çalıştırmak için bazı örnek komutları aşağıda verilmiştir. Bu komutları kaynaklarınıza göre ayarlayabilirsiniz.

### <a name="teragen"></a>Terajen

```
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 10000000000 adl://example/data/1TB-sort-input
```

### <a name="terasort"></a>Terasort

```
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 -Dmapreduce.job.reduces=512 -Dmapreduce.reduce.memory.mb=3072 adl://example/data/1TB-sort-input adl://example/data/1TB-sort-output
```

### <a name="teravalidate"></a>Teravalidate

```
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapreduce.job.maps=512 -Dmapreduce.map.memory.mb=3072 adl://example/data/1TB-sort-output adl://example/data/1TB-sort-validate
```
