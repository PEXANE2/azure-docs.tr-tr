---
title: 'Performans akort: Hive, HDInsight & Azure Veri Gölü Depolama Gen2 | Microsoft Dokümanlar'
description: Azure Veri Gölü Depolama Gen2 Kovan Performans Tuning Yönergeleri.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 66042568cede364c16302fbd85751de4113bbe0f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74327578"
---
# <a name="tune-performance-hive-hdinsight--azure-data-lake-storage-gen2"></a>Performans akort: Hive, HDInsight & Azure Veri Gölü Depolama Gen2

Varsayılan ayarlar, birçok farklı kullanım örneğinde iyi performans sağlamak üzere ayarlanmışt.)'ın.  G/Ç yoğun sorguları için Hive, Azure Veri Gölü Depolama Gen2 ile daha iyi performans elde etmek için ayarlanabilir.  

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Veri Gölü Depolama Gen2 hesabı**. Nasıl oluşturulacağına ilişkin talimatlar için [Bkz. Quickstart: Azure Veri Gölü Depolama Gen2 depolama hesabı oluşturma](data-lake-storage-quickstart-create-account.md)
* Veri Gölü Depolama Gen2 hesabına erişim içeren **Azure HDInsight kümesi.** Bkz. [Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi Kullan](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2)
* **HDInsight'ta Kovan Koşusu**.  HDInsight'ta Kovan işlerini çalıştırma hakkında bilgi edinmek için [HDInsight'ta Hive'ı Kullan](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-hive)
* **Veri Gölü Depolama Gen2'de performans alamı yönergeleri.**  Genel performans kavramları için Bkz. [Veri Gölü Depolama Gen2 Performans Tuning Kılavuzu](data-lake-storage-performance-tuning-guidance.md)

## <a name="parameters"></a>Parametreler

Geliştirilmiş Data Lake Storage Gen2 performansı için ayar yapmak için en önemli ayarlar şunlardır:

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
Data Lake Storage Gen2'yi kullanarak performansı artırmanın anahtarı eşzamanlılığı mümkün olduğunca artırmaktır.  Tez, oluşturulması gereken görev sayısını otomatik olarak hesaplar, böylece ayarlamanız gerekmez.   

## <a name="example-calculation"></a>Örnek hesaplama

8 düğüm D14 kümeniz olduğunu varsayalım.  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="further-information-on-hive-tuning"></a>Kovan atonlama hakkında daha fazla bilgi

Burada Hive sorguları ayarlamak yardımcı olacak birkaç bloglar şunlardır:
* [HDInsight'ta Hadoop için Hive sorgularını optimize edin](https://azure.microsoft.com/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [Sorun Giderme Kovanı sorgu performansı](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [HDInsight'ta optimize Hive'da konuşun](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)
