---
title: Azure Veri Gölü Depolama Gen1 Kıvılcım Performans Tuning Yönergeleri | Microsoft Dokümanlar
description: Azure Veri Gölü Depolama Gen1 Kıvılcım Performans Tuning Yönergeleri
services: data-lake-store
documentationcenter: ''
author: stewu
manager: amitkul
editor: stewu
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: dc92e7d2fcc911aeb6d92b91dd2d430af3c502ad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61436520"
---
# <a name="performance-tuning-guidance-for-spark-on-hdinsight-and-azure-data-lake-storage-gen1"></a>HDInsight ve Azure Veri Gölü Depolama Gen1'de Spark için performans alamı kılavuzu

Spark'ta performansı alarken, kümenizde çalışacak uygulama sayısını göz önünde bulundurmanız gerekir.  Varsayılan olarak, HDI kümenizde aynı anda 4 uygulama çalıştırabilirsiniz (Not: varsayılan ayar değişebilir).  Varsayılan ayarları geçersiz kılmak ve bu uygulamalar için kümenin daha fazlasını kullanabilmek için daha az uygulama kullanmaya karar verebilirsiniz.  

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Veri Gölü Depolama Gen1 hesabı.** Nasıl oluşturulacağına ilişkin talimatlar için [bkz.](data-lake-store-get-started-portal.md)
* Veri Gölü Depolama Gen1 hesabına erişim içeren **Azure HDInsight kümesi.** Bkz. [Veri Gölü Depolama Gen1 ile hdinsight kümesi oluşturun.](data-lake-store-hdinsight-hadoop-use-portal.md) Küme için Uzak Masaüstü'nü etkinleştirdiğinizden emin olun.
* **Veri Gölü Depolama Gen1 üzerinde Spark küme çalışıyor.**  Daha fazla bilgi için [Bkz. Veri Gölü Depolama Gen1'deki verileri analiz etmek için HDInsight Spark kümesini kullanın](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-use-with-data-lake-store)
* **Veri Gölü Depolama Gen1'de performans alamı yönergeleri.**  Genel performans kavramları için bkz: [Veri Gölü Depolama Gen1 Performans Tuning Kılavuzu](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-performance-tuning-guidance) 

## <a name="parameters"></a>Parametreler

Spark işlerini çalıştırırken, Veri Gölü Depolama Gen1'de performansı artırmak için ayarlanabilecek en önemli ayarlar şunlardır:

* **Num-yürütücüler** - Yürütülebilir eşzamanlı görevlerin sayısı.

* **Yürütme-bellek** - Her uygulayıcıya ayrılan bellek miktarı.

* **Uygulayıcı çekirdekler** - Her uygulayıcıya ayrılan çekirdek sayısı.                     

**Num-uygulayıcılar** Num-uygulayıcıları paralel olarak çalıştırılabilen en fazla görev sayısını ayarlar.  Paralel olarak çalıştırılabilen gerçek görev sayısı, kümenizde bulunan bellek ve CPU kaynaklarıyla sınırlanır.

**Yürütücü-bellek** Bu, her yürütücüye ayrılan bellek miktarıdır.  Her uygulayıcı için gereken bellek işe bağlıdır.  Karmaşık işlemler için belleğin daha yüksek olması gerekir.  Okuma yazma gibi basit işlemler için bellek gereksinimleri daha düşük olacaktır.  Her uygulayıcıiçin bellek miktarı Ambari'de görüntülenebilir.  Ambari'de Kıvılcım'a gidin ve Configs sekmesini görüntüleyin.  

**Executor çekirdekleri** Bu, uygulayıcı başına çalıştırılabilen paralel iş parçacığı sayısını belirleyen, uygulayıcı başına kullanılan çekirdek miktarını ayarlar.  Örneğin, yürütme çekirdekleri = 2 ise, her uygulayıcı sıyrıkta 2 paralel görev çalıştırabilir.  Gerekli uygulayıcı çekirdekler işe bağlı olacaktır.  G/Ç ağır işleri, her uygulayıcının daha paralel görevleri işleyebilir, böylece görev başına büyük miktarda bellek gerektirmez.

Varsayılan olarak, HDInsight'ta Kıvılcım çalıştırırken her fiziksel çekirdek için iki sanal YARN çekirdeği tanımlanır.  Bu sayı, eşzamanlılık ve birden çok iş parçacığı anahtarlama bağlam miktarı iyi bir denge sağlar.  

## <a name="guidance"></a>Rehber

Data Lake Storage Gen1'deki verilerle çalışmak için Spark analitik iş yüklerini çalıştırırken, Data Lake Storage Gen1 ile en iyi performansı elde etmek için en son HDInsight sürümünü kullanmanızı öneririz. İşiniz daha yoğun olduğunda, belirli parametreler performansı artırmak için yapılandırılabilir.  Data Lake Storage Gen1, yüksek iş verime işlenebilen yüksek ölçeklenebilir bir depolama platformudur.  İş esas olarak okuma veya yazmadan oluşuyorsa, Data Lake Storage Gen1'e ve Data Lake Storage Gen1'e görülcesini artırmak performansı artırabilir.

G/Ç yoğun işler için eşzamanlılığı artırmanın birkaç genel yolu vardır.

**Adım 1: Kümenizde kaç uygulamanın çalıştığını belirleyin** – Kümede geçerli uygulama da dahil olmak üzere kaç uygulamanın çalıştığını bilmeniz gerekir.  Her Kıvılcım ayarı için varsayılan değerler, aynı anda çalışan 4 uygulama olduğunu varsayar.  Bu nedenle, her uygulama için kümenin yalnızca %25'ine sahip olursunuz.  Daha iyi performans elde etmek için, uygulayıcıların sayısını değiştirerek varsayılanları geçersiz kılabilirsiniz.  

**Adım 2: Yürütme-bellek ayarlayın** - ayarlanan ilk şey yürütücü-bellek.  Bellek, çalıştıracağınız işe bağlı olacaktır.  Uygulayıcı başına daha az bellek ayırarak eşzamanlılığı artırabilirsiniz.  İşinizi çalıştırdığınızda bellek dışında özel durumlar görürseniz, bu parametrenin değerini artırmanız gerekir.  Bir alternatif bellek daha yüksek miktarda veya küme boyutunu artırarak bir küme kullanarak daha fazla bellek elde etmektir.  Daha fazla bellek, daha fazla yürütücü kullanılmasına olanak sağlar, bu da daha fazla eşzamanlılık anlamına gelir.

**Adım 3: Yürütme çekirdeklerini ayarlayın** – Karmaşık işlemleri olmayan G/Ç yoğun iş yükleri için, uygulayıcı başına paralel görev sayısını artırmak için çok sayıda yürütme çekirdeğiyle başlamak iyidir.  Uygulayıcı çekirdekleri 4'e ayarlamak iyi bir başlangıçtır.   

    executor-cores = 4
Uygulayıcı çekirdek lerin sayısını artırmak, farklı yürütücü çekirdeklerle denemeler yapabilmeniz için size daha fazla paralellik sağlayacaktır.  Daha karmaşık işlemleri olan işler için, uygulayıcı başına çekirdek sayısını azaltmanız gerekir.  Yürütme çekirdekleri 4'ten yüksek olarak ayarlanırsa, çöp toplama verimsiz hale gelebilir ve performansı düşürebilir.

**Adım 4: Kümedeki İplik belleği miktarını belirleyin** – Bu bilgiler Ambari'de mevcuttur.  İPLik'e gidin ve Configs sekmesini görüntüleyin.  İplik belleği bu pencerede görüntülenir.  
Penceredeyken varsayılan İplik kapsayıcı boyutunu da görebilirsiniz.  İPLIK kapsayıcı boyutu, yürütme parametresi başına bellekle aynıdır.

    Total YARN memory = nodes * YARN memory per node
**Adım 5: Num-uygulayıcıları hesaplama**

**Bellek kısıtlamasını hesapla** - Num-executors parametresi bellek veya CPU tarafından sınırlandırılır.  Bellek kısıtlaması, uygulamanız için kullanılabilir İplik belleği miktarına göre belirlenir.  Toplam İplik belleği almalı ve bunu uygulayıcı-bellek le bölmelisiniz.  Uygulama sayısı için kısıtlamanın ortadan kaldırılmasını gerekir, bu nedenle uygulama sayısına bölünür.

    Memory constraint = (total YARN memory / executor memory) / # of apps   
**CPU kısıtlamasını hesapla** - CPU kısıtlaması, uygulayıcı başına çekirdek sayısına bölünen toplam sanal çekirdek olarak hesaplanır.  Her fiziksel çekirdek için 2 sanal çekirdek vardır.  Bellek kısıtlamasına benzer şekilde, uygulama sayısına göre bölünürüz.

    virtual cores = (nodes in cluster * # of physical cores in node * 2)
    CPU constraint = (total virtual cores / # of cores per executor) / # of apps
**Set num-executors** – num-executors parametresi bellek kısıtlaması ve CPU kısıtlaması minimum alarak belirlenir. 

    num-executors = Min (total virtual Cores / # of cores per executor, available YARN memory / executor-memory)   
Daha fazla sayıda num-uygulayıcıayar mutlaka performansı artırmaz.  Daha fazla yürütücü eklemenin her ek uygulayıcı için ekstra ek yükü ekleyeceğini ve bunun performansı düşürebileceğini göz önünde bulundurmalısınız.  Num-yürütücüler küme kaynakları ile sınırlanır.    

## <a name="example-calculation"></a>Örnek Hesaplama

Şu anda çalıştırmak için gidiyoruz biri de dahil olmak üzere 2 uygulama çalışan 8 D4v2 düğümleri oluşan bir küme olduğunu varsayalım.  

**Adım 1: Kümenizde kaç uygulamanın çalıştığını belirleyin** – kümenizde çalıştıracağınız uygulama da dahil olmak üzere 2 uygulama nız olduğunu biliyorsunuz.  

**Adım 2: Uygulayıcı belleği ayarlayın** – bu örnek için, 6 GB yürütme belleği nin G/Ç yoğun iş için yeterli olacağını belirleriz.  

    executor-memory = 6GB
**Adım 3: Executor çekirdekleri ayarlayın** – Bu G/Ç yoğun bir iş olduğundan, her uygulayıcıiçin çekirdek sayısını 4 olarak ayarlayabiliriz.  Uygulayıcı başına çekirdekleri 4'ten büyük olarak ayarlamak çöp toplama sorunlarına neden olabilir.  

    executor-cores = 4
**Adım 4: Kümedeki İplik bellek miktarını belirleyin** – Her D4v2'nin 25 GB İplik belleğe sahip olduğunu öğrenmek için Ambari'ye gideceğiz.  8 düğüm olduğundan, kullanılabilir İplik belleği 8 ile çarpılır.

    Total YARN memory = nodes * YARN memory* per node
    Total YARN memory = 8 nodes * 25GB = 200GB
**Adım 5: num-executors hesaplamak** - num-executors parametresi bellek kısıtlaması ve CPU kısıtlaması # Spark üzerinde çalışan tarafından bölünerek minimum alarak belirlenir.    

**Bellek kısıtlamasını hesapla** – Bellek kısıtlaması, uygulayıcı başına bellek tarafından bölünen toplam İPlik bellek olarak hesaplanır.

    Memory constraint = (total YARN memory / executor memory) / # of apps   
    Memory constraint = (200GB / 6GB) / 2   
    Memory constraint = 16 (rounded)
**CPU kısıtlamasını hesapla** - CPU kısıtlaması, uygulayıcı başına çekirdek sayısına bölünen toplam iplik çekirdeği olarak hesaplanır.
    
    YARN cores = nodes in cluster * # of cores per node * 2   
    YARN cores = 8 nodes * 8 cores per D14 * 2 = 128
    CPU constraint = (total YARN cores / # of cores per executor) / # of apps
    CPU constraint = (128 / 4) / 2
    CPU constraint = 16
**Set num-executors**

    num-executors = Min (memory constraint, CPU constraint)
    num-executors = Min (16, 16)
    num-executors = 16    

