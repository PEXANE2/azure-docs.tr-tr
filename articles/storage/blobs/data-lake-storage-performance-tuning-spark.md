---
title: 'Performansı ayarlama: Spark, HDInsight & Azure Data Lake Storage 2. | Microsoft Docs'
description: Azure Data Lake Storage 2. Spark performansı ayarlama yönergeleri
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: a70b8112af201a49e7eece8b689e75102ec55880
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "74327558"
---
# <a name="tune-performance-spark-hdinsight--azure-data-lake-storage-gen2"></a>Performansı ayarlama: Spark, HDInsight & Azure Data Lake Storage 2.

Spark üzerinde performans ayarlaması yaparken, kümenizde çalıştırılacak uygulama sayısını göz önünde bulundurmanız gerekir.  Varsayılan olarak, HDI kümenizde 4 uygulamayı eşzamanlı olarak çalıştırabilirsiniz (Note: varsayılan ayar değişikliğe tabidir).  Varsayılan ayarları geçersiz kılabilmeniz ve bu uygulamalar için kümeyi daha fazla kullanabilmek için daha az uygulama kullanmaya karar verebilirsiniz.  

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Data Lake Storage 2. hesabı**. Bir oluşturma hakkında yönergeler için bkz. [hızlı başlangıç: Azure Data Lake Storage 2. depolama hesabı oluşturma](data-lake-storage-quickstart-create-account.md).
* Data Lake Storage 2. hesabına erişimi olan **Azure HDInsight kümesi** . Bkz. [Azure HDInsight kümeleri ile Azure Data Lake Storage 2. kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2). Küme için Uzak Masaüstü 'Nü etkinleştirdiğinizden emin olun.
* **Data Lake Storage 2. Spark kümesi çalıştırılıyor**.  Daha fazla bilgi için bkz. [Data Lake Storage 2. verileri çözümlemek Için HDInsight Spark kümesini kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-use-with-data-lake-store)
* **Data Lake Storage 2. performans ayarlama yönergeleri**.  Genel performans kavramları için bkz. [Data Lake Storage 2. performans ayarlama Kılavuzu](data-lake-storage-performance-tuning-guidance.md) 

## <a name="parameters"></a>Parametreler

Spark işlerini çalıştırırken, Data Lake Storage 2. performansı artırmak için ayarlanabilir en önemli ayarlar şunlardır:

* **Sayı yürüticileri** -yürütülebilecek eşzamanlı görevlerin sayısı.

* **Yürütücü-bellek** -her bir yürütücü için ayrılan bellek miktarı.

* **Yürütücü-çekirdekler** -her bir yürütücü için ayrılan çekirdek sayısı.                     

**Sayı yürüticileri** Sayı yürüticileri, paralel olarak çalışabilecek en fazla görev sayısını ayarlar.  Paralel olarak çalışabilecek görevlerin gerçek sayısı, kümenizde bulunan bellek ve CPU kaynaklarıyla sınırlıdır.

**Yürütücü-bellek** Bu, her bir yürütücü için ayrılan bellek miktarıdır.  Her yürütücü için gereken bellek, işe bağımlıdır.  Karmaşık işlemler için belleğin daha yüksek olması gerekir.  Okuma ve yazma gibi basit işlemler için bellek gereksinimleri daha düşük olacaktır.  Her bir yürütücü için bellek miktarı, ambarı 'nda görüntülenebilir.  Ambarı 'nda Spark ' a gidin ve configs sekmesini görüntüleyin.  

**Yürütücü-çekirdekler** Bu, her yürütücü için kullanılan çekirdek sayısını ayarlar ve bu, her yürütücü için çalıştırılabilen paralel iş parçacığı sayısını belirler.  Örneğin, yürütücü-çekirdekler = 2 ise, her yürütücü yürütücü içinde 2 paralel görev çalıştırabilir.  Gereken yürütücü-çekirdekler işe bağlı olacaktır.  G/ç ağır işleri görev başına büyük miktarda bellek gerektirmez, böylece her bir yürütücü daha fazla paralel görevi işleyebilir.

Varsayılan olarak, HDInsight 'ta Spark çalıştırılırken her fiziksel çekirdek için iki sanal YARN çekirdeği tanımlanmıştır.  Bu sayı, birden fazla iş parçacığından eşzamanlılık ve bağlam değiştirme miktarının iyi bir bakiyesini sağlar.  

## <a name="guidance"></a>Rehber

Spark analitik iş yüklerini Data Lake Storage 2. verilerle çalışacak şekilde çalıştırırken en son HDInsight sürümünü kullanarak Data Lake Storage 2. en iyi performansı elde etmenizi öneririz. İşiniz daha yoğun olan g/ç olduğunda, performansı artırmak için bazı parametreler yapılandırılabilir.  Data Lake Storage 2. yüksek aktarım hızını işleyebilen, yüksek düzeyde ölçeklenebilir bir depolama platformudur.  İş, genellikle okuma veya yazma işlemleri içeriyorsa, g/ç için eşzamanlılık kullanımını Data Lake Storage 2., performansı artırabilir.

G/ç yoğun işleri için eşzamanlılık artırmanın birkaç genel yolu vardır.

**1. Adım: kümenizde kaç uygulama çalıştığını belirleme** -kümede, geçerli olanı de içeren kaç uygulama çalıştığını bilmeniz gerekir.  Her Spark ayarının varsayılan değerleri, eşzamanlı olarak çalışan 4 uygulamanın olduğunu varsayar.  Bu nedenle, kümenin yalnızca %25 ' i her bir uygulama için kullanılabilir olacak.  Daha iyi performans sağlamak için, yürüticileri sayısını değiştirerek Varsayılanları geçersiz kılabilirsiniz.  

**2. Adım: yürütücü-bellek ayarlama** – ayarlanacak ilk şey, yürütücü-bellek ' dir.  Bellek, çalıştıracaksınız iş üzerine bağlı olacaktır.  Her yürütücü için daha az bellek ayırarak eşzamanlılık artırabilirsiniz.  İşinizi çalıştırdığınızda bellek dışında özel durumlar görürseniz, bu parametre için değeri artırmanız gerekir.  Diğer bir seçenek de daha yüksek miktarda belleğe sahip olan veya kümenizin boyutunu artıran bir küme kullanarak daha fazla bellek edinmaktır.  Daha fazla bellek, daha fazla eşzamanlılık sağlamak için daha fazla yürütme kullanılmasını sağlayacaktır.

**3. Adım: yürütücü-çekirdekleri ayarlama** – karmaşık işlemleri olmayan g/ç yoğunluklu iş yükleri için, yürütücü başına paralel görev sayısını artırmak için yüksek sayıda yürütücü çekirdeği ile başlamak iyi olur.  Yürütücü-çekirdekleri 4 olarak ayarlamak iyi bir başlangıç.   

    executor-cores = 4
Yürütücü sayısının artırılması, farklı yürütücü çekirdekleriyle denemeler yapabilmeniz için size daha paralellik verir.  Daha karmaşık işlemlere sahip işler için, yürütücü başına çekirdek sayısını azaltmalısınız.  Yürütücü-çekirdekleri 4 ' ten yüksek ayarlandıysa, çöp toplama verimsiz hale gelebilir ve performans düşebilir.

**4. Adım: kümede YARN bellek miktarını belirleme** – bu bilgiler, ambarı 'nda mevcuttur.  YARN 'ye gidin ve configs sekmesini görüntüleyin.  YARN belleği Bu pencerede görüntülenir.  
Göz atın, Ayrıca, varsayılan YARN kapsayıcı boyutunu da görebilirsiniz.  YARN kapsayıcı boyutu, yürütücü parametresi başına bellek ile aynıdır.

    Total YARN memory = nodes * YARN memory per node
**5. Adım: sayı yürüticileri hesaplama**

**Bellek kısıtlamasını hesapla** -NUM-eXecutors parametresi, bellek veya CPU tarafından sınırlandırılır.  Bellek kısıtlaması, uygulamanız için kullanılabilir YARN bellek miktarına göre belirlenir.  Toplam YARN belleği almanız ve bunu yürütücü-bellek ile bölmeniz gerekir.  Kısıtlama, uygulama sayısına göre bölünebilmemiz için uygulamanın sayısı için de ölçeklendirilmesi gerekir.

    Memory constraint = (total YARN memory / executor memory) / # of apps   
**CPU kısıtlaması hesapla** -CPU kısıtlaması, toplam sanal çekirdekler için, yürütücü başına çekirdek sayısı olarak hesaplanır.  Her fiziksel çekirdek için 2 sanal çekirdek vardır.  Bellek kısıtlamasına benzer şekilde, uygulama sayısına göre bölmemiz gerekir.

    virtual cores = (nodes in cluster * # of physical cores in node * 2)
    CPU constraint = (total virtual cores / # of cores per executor) / # of apps
**Num-Yürüticileri ayarlama** – NUM-eXecutors parametresi, en az bellek KıSıTLAMASı ve CPU kısıtlaması alınarak belirlenir. 

    num-executors = Min (total virtual Cores / # of cores per executor, available YARN memory / executor-memory)   
Daha yüksek sayıda NUM yürüticilerinin ayarlanması performansı artırmayabilir.  Daha fazla yürütme eklemenin, performansı düşürebilecek her ek yürütücü için ek yük eklemesini göz önünde bulundurmanız gerekir.  Sayı yürüticileri, küme kaynaklarıyla sınırlıdır.    

## <a name="example-calculation"></a>Örnek hesaplama

Şu anda, çalıştırmakta olduğunuz bir dahil olmak üzere 2 uygulama çalıştıran 8 D4v2 düğümden oluşan bir kümeniz olduğunu varsayalım.  

**1. Adım: kümenizde kaç uygulama çalıştığını belirleme** -kümenizde çalıştırdığınız bir de dahil olmak üzere, kümenizde 2 uygulama olduğunu bilirsiniz.  

**2. Adım: yürütücü-bellek ayarlama** – Bu örnek IÇIN, 6GB 'ın yoğun çalışan iş için yeterli olacağını belirledik.  

    executor-memory = 6GB
**3. Adım: yürütücü-çekirdekleri ayarlama** – bu bir g/ç yoğun iş olduğundan, her bir yürütücü için çekirdek sayısını 4 ' e ayarlayabiliriz.  Yürütücü başına çekirdekleri 4 ' ten büyük olarak ayarlamak çöp toplama sorunlarına neden olabilir.  

    executor-cores = 4
**4. Adım: kümede Yarn bellek miktarını belirleme** – her bir D4V2 'ıN Yarn belleği 25 GB sahip olduğunu öğrenmek için ambarı 'na gittik.  8 düğüm olduğundan, kullanılabilir YARN belleği 8 ile çarpılır.

    Total YARN memory = nodes * YARN memory* per node
    Total YARN memory = 8 nodes * 25GB = 200GB
**5. Adım: sayı Yürüticileri hesaplama** – NUM-eXecutors parametresi, bellek kısıtlamasının minimum değeri ve Spark üzerinde çalışan # of Apps tarafından ayrılmış CPU kısıtlaması alınarak belirlenir.    

**Bellek kısıtlamasını hesapla** – bellek kısıtlaması, her yürütücü için belleğe bölünen toplam Yarn belleği olarak hesaplanır.

    Memory constraint = (total YARN memory / executor memory) / # of apps   
    Memory constraint = (200GB / 6GB) / 2   
    Memory constraint = 16 (rounded)
**CPU kısıtlamasını hesapla** -CPU kısıtlaması, toplam Yarn çekirdeği, yürütücü başına çekirdek sayısına bölünen şekilde hesaplanır.
    
    YARN cores = nodes in cluster * # of cores per node * 2   
    YARN cores = 8 nodes * 8 cores per D14 * 2 = 128
    CPU constraint = (total YARN cores / # of cores per executor) / # of apps
    CPU constraint = (128 / 4) / 2
    CPU constraint = 16
**Sayı yürüticileri ayarlama**

    num-executors = Min (memory constraint, CPU constraint)
    num-executors = Min (16, 16)
    num-executors = 16    

