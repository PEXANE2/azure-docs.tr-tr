---
title: 'Performans akort: MapReduce, HDInsight & Azure Veri Gölü Depolama Gen2 | Microsoft Dokümanlar'
description: Azure Veri Gölü Depolama Gen2 MapReduce Performans Tuning Yönergeleri
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: a3ea6858355d6cb921f629bf36134d96371f6244
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74327921"
---
# <a name="tune-performance-mapreduce-hdinsight--azure-data-lake-storage-gen2"></a>Performans akort: MapReduce, HDInsight & Azure Veri Gölü Depolama Gen2

Harita Küçültme işlerinin performansını ayarlarken göz önünde bulundurmanız gereken faktörleri anlayın. Bu makalede, performans tuning yönergeleri bir dizi kapsar.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Veri Gölü Depolama Gen2 hesabı.** Nasıl oluşturulacağına ilişkin talimatlar için [Bkz. Quickstart: Azure Veri Gölü Depolama Gen2 depolama hesabı oluşturun.](data-lake-storage-quickstart-create-account.md)
* Veri Gölü Depolama Gen2 hesabına erişim içeren **Azure HDInsight kümesi.** Bkz. [Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi Kullan](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2)
* **HDInsight'ta MapReduce'ı kullanma**.  Daha fazla bilgi için, [HDInsight'ta Hadoop'ta MapReduce'i kullanın](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-mapreduce)
* **Veri Gölü Depolama Gen2'de performans alamı yönergeleri.**  Genel performans kavramları için Bkz. [Veri Gölü Depolama Gen2 Performans Tuning Kılavuzu](data-lake-storage-performance-tuning-guidance.md)

## <a name="parameters"></a>Parametreler

MapReduce işlerini çalıştırırken, Veri Gölü Depolama Gen2'de performansı artırmak için yapılandırabileceğiniz parametreler şunlardır:

* **Mapreduce.map.memory.mb** – Her mapper'a ayrılacak bellek miktarı
* **Mapreduce.job.maps** – İş başına harita görevi sayısı
* **Mapreduce.reduce.memory.mb** – Her redüktöre ayrılacak bellek miktarı
* **Mapreduce.job.reduces –** İş başına düşen görev sayısını azaltma

**Mapreduce.map.memory / Mapreduce.reduce.memory** Bu sayı, harita için ne kadar bellek gerektiğine göre ayarlanmalıdır ve/veya görevi azaltmalıdır.  mapreduce.map.memory ve mapreduce.reduce.memory varsayılan değerleri İplik yapılandırması ile Ambari'de görüntülenebilir.  Ambari'de, İplik'e gidin ve Configs sekmesini görüntüleyin.  İplik belleği görüntülenir.  

**Mapreduce.job.maps / Mapreduce.job.reduces** Bu, oluşturulacak en fazla haritalayıcı veya redüktör sayısını belirler.  Bölme sayısı, MapReduce işi için kaç harita oluşturacağını belirler.  Bu nedenle, istenen harita oluşturucu sayısından daha az bölme varsa, istediğinizden daha az harita oluşturucu alabilirsiniz.       

## <a name="guidance"></a>Rehber

> [!NOTE]
> Bu belgedeki kılavuz, uygulamanızın kümenizde çalışan tek uygulama olduğunu varsayar.

**Adım 1: Çalışan iş sayısını belirleme**

Varsayılan olarak, MapReduce işiniz için tüm kümeyi kullanır.  Kullanılabilir kapsayıcılardan daha az harita oluşturucu kullanarak kümenin daha azını kullanabilirsiniz.        

**Adım 2: mapreduce.map.memory/mapreduce.reduce.memory'i ayarlayın.**

Harita ve küçültme görevleri için belleğin boyutu, özel işinize bağlıdır.  Eşzamanlılığı artırmak istiyorsanız bellek boyutunu azaltabilirsiniz.  Aynı anda çalışan görev sayısı kapsayıcıların sayısına bağlıdır.  Haritalayıcı veya azaltıcı başına bellek miktarını azaltarak, daha fazla haritacı veya indirgeyicinin aynı anda çalışmasını sağlayan daha fazla kapsayıcı oluşturulabilir.  Bellek miktarını çok fazla azaltmak bazı işlemlerin bellek lerinin tükenmesine neden olabilir.  İşinizi çalıştırırken yığın hatası alırsanız, haritalayıcı veya redüktör başına bellek artırmanız gerekir.  Daha fazla kapsayıcı eklemenin her ek kapsayıcı için ekstra ek ek yükü ekleyeceğini ve bunun performansı düşürebileceğini göz önünde bulundurmalısınız.  Başka bir alternatif bellek daha yüksek miktarda veya kümedüğüm sayısını artırarak bir küme kullanarak daha fazla bellek elde etmektir.  Daha fazla bellek daha fazla kapsayıcının kullanılmasını sağlar, bu da daha fazla eşzamanlılık anlamına gelir.  

**Adım 3: Toplam İplik belleği belirleme**

mapreduce.job.maps/mapreduce.job.reduces'ı ayarlamak için, kullanılabilir toplam İplik bellek miktarını göz önünde bulundurmalısınız.  Bu bilgiler Ambari'de mevcuttur.  İPLik'e gidin ve Configs sekmesini görüntüleyin.  İplik belleği bu pencerede görüntülenir.  Toplam İPlik belleği almak için IPN belleği kümenizdeki düğüm sayısı ile çarpmalısınız.

    Total YARN memory = nodes * YARN memory per node

Boş bir küme kullanıyorsanız, bellek kümenizin toplam İplik belleği olabilir.  Diğer uygulamalar bellek kullanıyorsa, haritalayıcı veya indirgeyici sayısını kullanmak istediğiniz kapsayıcı sayısına düşürerek kümenizin belleği yalnızca bir bölümünü kullanmayı seçebilirsiniz.  

**Adım 4: İplik kaplarının sayısını hesaplama**

İplik kapları, iş için kullanılabilir eşzamanlılık miktarını belirler.  Toplam İplik belleği alın ve mapreduce.map.memory ile bölün.  

    # of YARN containers = total YARN memory / mapreduce.map.memory

**Adım 5: mapreduce.job.maps/mapreduce.job.reduces'ı ayarlayın**

Mapreduce.job.maps/mapreduce.job'u ayarlayın.kullanılabilir kapsayıcı sayısı en az azaltır.  Daha iyi performans elde edip edindiğinizi görmek için haritacıların ve azaltıcıların sayısını artırarak daha fazla deneme yapabilirsiniz.  Çok fazla haritacısıolmasının performansı düşürebileceği için daha fazla haritacısının ek ek yükü olacağını unutmayın.  

CPU zamanlama ve CPU yalıtımı varsayılan olarak kapatılır, böylece İPlik kapsayıcılarının sayısı bellekle sınırlandırılır.

## <a name="example-calculation"></a>Örnek hesaplama

8 D14 düğümünden oluşan bir kümemiz olduğunu ve Yoğun Bir G/Ç çalışması yapmak istediğimizi varsayalım.  Yapmanız gereken hesaplamalar şunlardır:

**Adım 1: Çalışan iş sayısını belirleme**

Bu örnekte, çalışan tek işin işimiz olduğunu varsayalım.  

**Adım 2: mapreduce.map.memory/mapreduce.reduce.memory'i ayarlayın.**

Bu örnekte, G/Ç yoğun bir iş yürütüyoruz ve harita görevleri için 3 GB belleğin yeterli olacağına karar verdik.

    mapreduce.map.memory = 3GB

**Adım 3: Toplam İplik belleği belirleme**

    Total memory from the cluster is 8 nodes * 96GB of YARN memory for a D14 = 768GB
**Adım 4: Hesap # İplik kapları**

    # of YARN containers = 768GB of available memory / 3 GB of memory =   256

**Adım 5: mapreduce.job.maps/mapreduce.job.reduces'ı ayarlayın**

    mapreduce.map.jobs = 256

## <a name="examples-to-run"></a>Çalıştırılabilmek için örnekler

MapReduce'ın Veri Gölü Depolama Gen2'de nasıl çalıştığını göstermek için aşağıdaki ayarlara sahip bir küme üzerinde çalıştırılabilen bazı örnek kod verilmiştir:

* 16 düğüm D14v2
* HDI 3.6 çalışan Hadoop kümesi

Bir başlangıç noktası için, mapreduce Teragen, Terasort ve Teravalidate çalıştırmak için bazı örnek komutları aşağıda verilmiştir.  Bu komutları kaynaklarınıza göre ayarlayabilirsiniz.

**Terajen**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 10000000000 abfs://example/data/1TB-sort-input

**Terasort**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 -Dmapreduce.job.reduces=512 -Dmapreduce.reduce.memory.mb=3072 abfs://example/data/1TB-sort-input abfs://example/data/1TB-sort-output

**Teravalidate**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapreduce.job.maps=512 -Dmapreduce.map.memory.mb=3072 abfs://example/data/1TB-sort-output abfs://example/data/1TB-sort-validate
