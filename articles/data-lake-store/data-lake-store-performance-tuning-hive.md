---
title: Azure Veri Gölü Depolama Gen1 Hive Performans Tuning Yönergeleri | Microsoft Dokümanlar
description: Azure Veri Gölü Depolama Gen1 Hive Performans Tuning Yönergeleri
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
ms.openlocfilehash: 433c6b7d70cea9406b67d65e23cc357939cb5aa0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61437285"
---
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-storage-gen1"></a>HDInsight ve Azure Veri Gölü Depolama Gen1'de Hive için performans alamı kılavuzu

Varsayılan ayarlar, birçok farklı kullanım örneğinde iyi performans sağlamak üzere ayarlanmışt.)'ın.  G/Ç yoğun sorguları için Hive, Azure Veri Gölü Depolama Gen1 ile daha iyi performans elde etmek için ayarlanabilir.  

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Veri Gölü Depolama Gen1 hesabı**. Nasıl oluşturulacağına ilişkin talimatlar için [bkz.](data-lake-store-get-started-portal.md)
* Veri Gölü Depolama Gen1 hesabına erişim içeren **Azure HDInsight kümesi.** Bkz. [Veri Gölü Depolama Gen1 ile hdinsight kümesi oluşturun.](data-lake-store-hdinsight-hadoop-use-portal.md) Küme için Uzak Masaüstü'nü etkinleştirdiğinizden emin olun.
* **HDInsight'ta Kovan Koşusu**.  HDInsight'ta Kovan işlerini çalıştırma hakkında bilgi edinmek için [HDInsight'ta Hive'ı Kullan](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-hive)
* **Veri Gölü Depolama Gen1'de performans alamı yönergeleri.**  Genel performans kavramları için bkz: [Veri Gölü Depolama Gen1 Performans Tuning Kılavuzu](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-performance-tuning-guidance)

## <a name="parameters"></a>Parametreler

Geliştirilmiş Data Lake Storage Gen1 performansı için ayar yapmak için en önemli ayarlar şunlardır:

* **hive.tez.container.size** – her görev tarafından kullanılan bellek miktarı

* **tez.grouping.min-size** – her mapper minimum boyutu

* **tez.grouping.max-size** – her mapper maksimum boyutu

* **hive.exec.reducer.bytes.per.reducer** – her redüktörün boyutu

**hive.tez.container.size** - Kapsayıcı boyutu her görev için ne kadar bellek kullanılabilir belirler.  Bu, Kovan'daki eşzamanlılığı denetlemek için ana girdidir.  

**tez.grouping.min-size** – Bu parametre her mapper'ın minimum boyutunu ayarlamanızı sağlar.  Tez'in seçtiği haritaoluşturucu sayısı bu parametrenin değerinden daha küçükse, Tez burada ayarlanan değeri kullanır.

**tez.grouping.max-size** – Parametre her mapper maksimum boyutunu ayarlamanızı sağlar.  Tez'in seçtiği haritaoluşturucu sayısı bu parametrenin değerinden daha büyükse, Tez burada ayarlanan değeri kullanır.

**hive.exec.reducer.bytes.per.reducer** – Bu parametre her bir redüktörün boyutunu ayarlar.  Varsayılan olarak, her indirici 256MB'dır.  

## <a name="guidance"></a>Rehber

**Set hive.exec.reducer.bytes.per.reducer** – Varsayılan değer veri sıkıştırılmamış olduğunda iyi çalışır.  Sıkıştırılmış veriler için, redüktörboyutunu azaltmanız gerekir.  

**Set hive.tez.container.size** – Her düğümde bellek iplik.nodemanager.resource.memory-mb tarafından belirtilir ve varsayılan olarak HDI kümesinde doğru olarak ayarlanmalıdır.  İplik'te uygun belleği ayarlama hakkında daha fazla bilgi için bu [yazıya](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom)bakın.

G/Ç yoğun iş yükleri Tez konteyner boyutunu azaltarak daha fazla paralellik yararlanabilir. Bu, kullanıcıya eşzamanlılığı artıran daha fazla kapsayıcı sağlar.  Ancak, bazı Hive sorguları önemli miktarda bellek gerektirir (örn. MapJoin).  Görevyeterli belleğe sahip değilse, çalışma süresi sırasında bellek dışında bir özel durum alırsınız.  Bellek dışında özel durumlar alırsanız, o zaman bellek artırmanız gerekir.   

Eşzamanlı çalışan görev sayısı veya paralellik toplam İplik belleği ile sınırlanır.  İplik kapsayıcılarının sayısı, eşzamanlı görevlerin kaç tane çalıştırılabildiğini belirler.  Düğüm başına İplik bellek bulmak için, Ambari gidebilirsiniz.  İPLik'e gidin ve Configs sekmesini görüntüleyin.  İplik belleği bu pencerede görüntülenir.  

        Total YARN memory = nodes * YARN memory per node
        # of YARN containers = Total YARN memory / Tez container size
Data Lake Storage Gen1'i kullanarak performansı artırmanın anahtarı eşzamanlılığı mümkün olduğunca artırmaktır.  Tez, oluşturulması gereken görev sayısını otomatik olarak hesaplar, böylece ayarlamanız gerekmez.   

## <a name="example-calculation"></a>Örnek Hesaplama

8 düğüm D14 kümeniz olduğunu varsayalım.  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="limitations"></a>Sınırlamalar

**Veri Gölü Depolama Gen1 azaltma** 

Data Lake Storage Gen1 tarafından sağlanan bant genişliği sınırlarına ularsanız, görev hatalarını görmeye başlarsınız. Bu, görev günlüklerinde azaltma hataları gözlemleyerek tanımlanabilir.  Tez konteyner boyutunu artırarak paralelliği azaltabilirsiniz.  İşiniz için daha fazla eşzamanlılık istiyorsanız, lütfen bize ulaşın.

Daraltılmış olup olmadığınızı kontrol etmek için istemci tarafında hata ayıklama günlüğe kaydetmeyi etkinleştirmeniz gerekir. Bunu şu şekilde yapabilirsiniz:

1. Hive config içinde log4j özellikleri aşağıdaki özelliği koyun. Bu Ambari görünümünden yapılabilir: log4j.logger.com.microsoft.azure.datalake.store=DEBUG Config'in etkili olması için tüm düğümleri/hizmeti yeniden başlatın.

2. Daralıyorsanız, kovan günlüğü dosyasında HTTP 429 hata kodunu görürsünüz. Kovan günlüğü dosyası /tmp/&lt;&gt;kullanıcı /hive.log

## <a name="further-information-on-hive-tuning"></a>Kovan atonlama hakkında daha fazla bilgi

Burada Hive sorguları ayarlamak yardımcı olacak birkaç bloglar şunlardır:
* [HDInsight'ta Hadoop için Hive sorgularını optimize edin](https://azure.microsoft.com/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [Sorun Giderme Kovanı sorgu performansı](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [HDInsight'ta optimize Hive'da konuşun](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)
