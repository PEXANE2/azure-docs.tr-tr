---
title: Apache Hadoop teknoloji yığını nedir? -Azure HDInsight
description: HDInsight 'a giriş ve Apache Hadoop teknoloji yığını ve bileşenleri.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: overview
ms.custom: hdinsightactive,hdiseo17may2017,mvc,seodec18
ms.date: 02/27/2020
ms.openlocfilehash: e98a11d9aee1c4f76453dc1716f92514b021444f
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77917841"
---
# <a name="what-is-apache-hadoop-in-azure-hdinsight"></a>Azure HDInsight 'ta Apache Hadoop nedir?

Kümelerde büyük veri kümelerinin dağıtılmış işlenmesi ve analizine yönelik ilk açık kaynak çerçeve [Apache Hadoop](https://hadoop.apache.org/)’tu. Hadoop ekosistemi Apache Hive, Apache HBase, Spark, Kafka ve diğer birçok tane dahil olmak üzere ilgili yazılım ve yardımcı programları içerir.

Azure HDInsight, kuruluşlar için bulutta tam olarak yönetilen, tam bir açık kaynaklı analiz hizmetidir. Azure HDInsight 'taki Apache Hadoop kümesi türü, Batch verilerini paralel olarak işlemek ve analiz etmek için, bu kaynak yönetimini ve basit bir MapReduce programlama modelini kullanmanıza olanak sağlar.

HDInsight 'ta kullanılabilir Hadoop teknoloji yığını bileşenlerini görmek için bkz. [HDInsight ile kullanılabilen bileşenler ve sürümler](../hdinsight-component-versioning.md). HDInsight'ta Hadoop hakkında daha fazla bilgi edinmek için bkz. [HDInsight için Azure özellikleri sayfası](https://azure.microsoft.com/services/hdinsight/).

## <a name="what-is-mapreduce"></a>MapReduce nedir?

MapReduce Apache Hadoop, çok miktarda veriyi işleyen işleri yazmaya yönelik bir yazılım çerçevesidir. Giriş verileri bağımsız parçalara bölünür. Her bir öbek, kümenizdeki düğümler arasında paralel olarak işlenir. MapReduce işi iki işlevden oluşur:

* **Eşleyici**: giriş verilerini kullanır, analiz eder (genellikle filtre ve sıralama işlemleriyle birlikte) ve tanımlama gruplarını yayar (anahtar-değer çiftleri)

* **Reducer**: Eşleyici tarafından yayılan başlıkları kullanır ve Eşleyici verilerinden daha küçük ve birleştirilmiş bir sonuç oluşturan bir Özet işlem gerçekleştirir

Temel bir sözcük sayısı MapReduce iş örneği aşağıdaki diyagramda gösterilmiştir:

 ![HDI. WordCountDiagram](./media/apache-hadoop-introduction/hdi-word-count-diagram.gif)

Bu işin çıkışı, metinde her bir sözcüğün kaç kez oluştuğunu gösteren bir saydır.

* Eşleyici her satırı giriş metinden girdi olarak alır ve sözcüklere ayırır. Bir sözcüğün her oluşması durumunda 1 ' in ardından bir anahtar/değer çifti yayar. Çıktı, Reducer 'e gönderilmeden önce sıralanır.
* Reducer, her sözcük için bu ayrı sayıları toplar ve sonra tekrarlarının toplamına göre, sözcüğü içeren tek bir anahtar/değer çifti yayar.

MapReduce, çeşitli dillerde uygulanabilir. Java en yaygın uygulamasıdır ve bu belgede tanıtım amacıyla kullanılır.

## <a name="development-languages"></a>Geliştirme dilleri

Java ve Java Sanal Makinesi temel alan diller veya çerçeveler doğrudan MapReduce işi olarak çalıştırılabilir. Bu belgede kullanılan örnek bir Java MapReduce uygulamasıdır. C#, Python veya tek başına yürütülebilir dosyalar gibi Java olmayan dillerin **Hadoop akışını**kullanması gerekir.

Hadoop akışı Eşleyici ve Reducer üzerinden STDıN ve STDOUT üzerinden iletişim kurar. Eşleyici ve Reducer verileri STDIN 'den bir kerede bir satıra okur ve çıktıyı STDOUT 'a yazar. Eşleyici ve Reducer tarafından okunan veya yayılan her satır, bir sekme karakteriyle ayrılmış bir anahtar/değer çifti biçiminde olmalıdır:

    [key]/t[value]

Daha fazla bilgi için bkz. [Hadoop akışı](https://hadoop.apache.org/docs/current/hadoop-streaming/HadoopStreaming.html).

HDInsight ile Hadoop akışını kullanma örnekleri için aşağıdaki belgeye bakın:

* [MapReduce işleri geliştirme C#](apache-hadoop-dotnet-csharp-mapreduce-streaming.md)

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight 'ta Apache Hadoop kümesi oluşturma](apache-hadoop-linux-create-cluster-get-started-portal.md)
