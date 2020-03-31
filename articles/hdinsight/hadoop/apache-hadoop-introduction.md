---
title: Apache Hadoop ve MapReduce nedir - Azure HDInsight
description: HDInsight'a giriş ve Apache Hadoop teknoloji yığını ve bileşenleri.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: overview
ms.custom: hdinsightactive,hdiseo17may2017,mvc,seodec18
ms.date: 02/27/2020
ms.openlocfilehash: 7e8dd69b7c58e090c30ea1aa59feddab610dd3c5
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78244881"
---
# <a name="what-is-apache-hadoop-in-azure-hdinsight"></a>Azure HDInsight'ta Apache Hadoop nedir?

Kümelerde büyük veri kümelerinin dağıtılmış işlenmesi ve analizine yönelik ilk açık kaynak çerçeve [Apache Hadoop](https://hadoop.apache.org/)’tu. Hadoop ekosistemi, Apache Hive, Apache HBase, Spark, Kafka ve diğerleri dahil olmak üzere ilgili yazılım ve yardımcı programları içerir.

Azure HDInsight, işletmeler için bulutta tam olarak yönetilen, tam spektrumlu, açık kaynak kodlu bir analiz hizmetidir. Azure HDInsight'taki Apache Hadoop küme türü, toplu verileri paralel olarak işlemek ve analiz etmek için HDFS, YARN kaynak yönetimi ve basit bir MapReduce programlama modelini kullanmanıza olanak tanır.

HDInsight üzerindeki kullanılabilir Hadoop teknolojisi yığını bileşenlerini görmek için, bkz. [HDInsight ile sağlanan bileşenler ve sürümler](../hdinsight-component-versioning.md). HDInsight'ta Hadoop hakkında daha fazla bilgi edinmek için bkz. [HDInsight için Azure özellikleri sayfası](https://azure.microsoft.com/services/hdinsight/).

## <a name="what-is-mapreduce"></a>MapReduce nedir

Apache Hadoop MapReduce, büyük miktarda veriyi işleyen işler yazmak için bir yazılım çerçevesidir. Giriş verileri bağımsız parçalara ayrılır. Her bir yığın kümenizdeki düğümler arasında paralel olarak işlenir. MapReduce işi iki işlevden oluşur:

* **Mapper**: Giriş verilerini tüketir, analiz eder (genellikle filtre ve sıralama işlemleriyle) ve tupül (anahtar değer çiftleri) yayır

* **Azaltıcı**: Mapper tarafından yayılan tuples tüketir ve Mapper verilerinden daha küçük, birleşik bir sonuç oluşturan bir özet işlemi gerçekleştirir

Temel sözcük sayısı MapReduce iş örneği aşağıdaki diyagramda gösterilmiştir:

 ![Hdı. WordCountDiyagram](./media/apache-hadoop-introduction/hdi-word-count-diagram.gif)

Bu işin çıktısı, metinde her sözcüğün kaç kez oluştuğunu n sayısıdır.

* Haritaoluşturucu giriş metninden her satırı giriş olarak alır ve sözcüklere ayırır. Sözcüğün bir sözcüğü her oluştuğunda bir anahtar/değer çifti yayır ve ardından 1. Çıktı, indirgeme cile göndermeden önce sıralanır.
* Azaltıcı, bu tek tek sayıları her sözcük için toplamlar ve sözcüğü ve ardından oluşumlarının toplamını içeren tek bir anahtar/değer çifti yayar.

MapReduce çeşitli dillerde uygulanabilir. Java en yaygın uygulamadır ve bu belgede gösteri amacıyla kullanılır.

## <a name="development-languages"></a>Geliştirme dilleri

Java ve Java Sanal Makine'yi temel alan diller veya çerçeveler doğrudan MapReduce işi olarak çalıştırılabilir. Bu belgede kullanılan örnek bir Java MapReduce uygulamasıdır. C#, Python veya bağımsız yürütülebilir ler gibi Java olmayan **dillerhadoop akışını**kullanmalıdır.

Hadoop akışı, STDIN ve STDOUT üzerinden mapper ve redüktör ile iletişim kurar. Haritacısı ve azaltıcı verileri STDIN'den bir defada bir satır okur ve çıktıyı STDOUT'a yazar. Mapper ve redüktör tarafından okunan veya yayılan her satır, sekme karakteriyle sınırlandırılmış bir anahtar/değer çifti biçiminde olmalıdır:

    [key]/t[value]

Daha fazla bilgi için [Hadoop Streaming'e](https://hadoop.apache.org/docs/current/hadoop-streaming/HadoopStreaming.html)bakın.

HDInsight ile Hadoop akış kullanma örnekleri için aşağıdaki belgeye bakın:

* [C# MapKüçülün işleri geliştirin](apache-hadoop-dotnet-csharp-mapreduce-streaming.md)

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight'ta Apache Hadoop kümesi oluşturma](apache-hadoop-linux-create-cluster-get-started-portal.md)
