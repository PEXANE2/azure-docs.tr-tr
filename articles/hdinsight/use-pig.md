---
title: Apache Pig kullanma
titleSuffix: Azure HDInsight
description: HDInsight'ta Apache Hadoop ile Pig'i nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/28/2020
ms.openlocfilehash: ea960a92aee1c9447bb12d27cffdc42de9fd907a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77672132"
---
# <a name="use-apache-pig-with-apache-hadoop-on-hdinsight"></a>HDInsight'ta Apache Hadoop ile Apache Pig kullanın

HDInsight ile [Apache Pig'i](https://pig.apache.org/) nasıl kullanacağınızı öğrenin.

Apache Pig *Domuz Latince*olarak bilinen bir prosedür dili kullanarak Apache Hadoop için programlar oluşturmak için bir platformdur. Pig, *MapReduce* çözümleri oluşturmak için Java'ya alternatiftir ve Azure HDInsight'a dahildir. Pig'in HDInsight ile kullanılabileceğinin çeşitli yollarını keşfetmek için aşağıdaki tabloyu kullanın:

## <a name="why-use-apache-pig"></a><a id="why"></a>Neden Apache Pig kullanın

MapReduce in Hadoop'ta kullanarak verileri işlemenin zorluklarından biri, yalnızca bir harita ve azaltma işlevi kullanarak işlem mantığınızı uygulamaktır. Karmaşık işleme için, istenilen sonucu elde etmek için sık sık işlemeyi, birlikte zincirlenmiş birden çok MapReduce işlemine ayırmanız gerekir.

Pig, işlemeyi, verilerin istenilen çıktıyı üretmek için aktığı bir dizi dönüşüm olarak tanımlamanızı sağlar.

Pig Latin dili, istenilen çıktıyı üretmek için bir veya daha fazla dönüşüm yoluyla ham girişten gelen veri akışını tanımlamanızı sağlar. Domuz Latin programları bu genel desen izleyin:

* **Load**: Dosya sisteminden kullanılacak verileri okuyun.

* **Transform**: Verileri manipüle edin.

* **Dökümü veya depola :** Çıktı verilerini ekrana veya işleme için saklayın.

### <a name="user-defined-functions"></a>Kullanıcı tanımlı işlevler

Pig Latin ayrıca, Pig Latin'de modellenebilen mantığı uygulayan dış bileşenleri çağırmanızı sağlayan kullanıcı tanımlı işlevleri (UDF) da destekler.

Domuz Latin hakkında daha fazla bilgi için, [Domuz Latin Referans Kılavuzu 1](https://archive.cloudera.com/cdh/3/pig/piglatin_ref1.html) ve Domuz Latin Referans Kılavuzu [2](https://archive.cloudera.com/cdh/3/pig/piglatin_ref2.html)bakın.

## <a name="example-data"></a><a id="data"></a>Örnek veriler

HDInsight, dizinlerde `/example/data` ve `/HdiSamples` dizinlerde depolanan çeşitli örnek veri kümeleri sağlar. Bu dizinler kümeniz için varsayılan depolama alanındadır. Bu belgedeki Pig örneği *log4j* `/example/data/sample.log`dosyasını kullanır.

Dosyaiçindeki her günlük, örneğin türü ve `[LOG LEVEL]` önem derecesini gösteren bir alan içeren bir alan satırından oluşur:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Önceki örnekte, günlük düzeyi HATA'dır.

> [!NOTE]  
> Ayrıca [Apache Log4j](https://en.wikipedia.org/wiki/Log4j) günlük aracını kullanarak bir log4j dosyası oluşturabilir ve sonra bu dosyayı blob'unuza yükleyebilirsiniz. Talimatlar için [HDInsight'a Veri Yükle'ye](hdinsight-upload-data.md) bakın. HDInsight ile Azure depolamasındaki lekelerin nasıl kullanıldığı hakkında daha fazla bilgi için [bkz.](hdinsight-hadoop-use-blob-storage.md)

## <a name="example-job"></a><a id="job"></a>Örnek iş

Aşağıdaki Pig Latin işi, DOSYAYI `sample.log` HDInsight kümeniz için varsayılan depolama dan yükler. Daha sonra, giriş verilerinde her günlük düzeyinin kaç kez oluştuğunu saymakla sonuçlanan bir dizi dönüşüm gerçekleştirir. Sonuçlar STDOUT'a yazılır.

    ```
    LOGS = LOAD 'wasb:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;
    ```

Aşağıdaki resim, her dönüşümün verilere ne yaptığının bir özetini gösterir.

![Dönüşümlerin grafikgösterimi][image-hdi-pig-data-transformation]

## <a name="run-the-pig-latin-job"></a><a id="run"></a>Domuz Latin iş çalıştırın

HDInsight çeşitli yöntemler kullanarak Pig Latin işleri çalıştırabilirsiniz. Hangi yöntemin sizin için doğru olduğuna karar vermek için aşağıdaki tabloyu kullanın ve ardından bir gözden geçirme için bağlantıyı izleyin.

## <a name="pig-and-sql-server-integration-services"></a>Pig ve SQL Server Entegrasyon Hizmetleri

Bir Pig işini çalıştırmak için SQL Server Integration Services 'ı (SSIS) kullanabilirsiniz. SSIS için Azure Özellik Paketi, HDInsight'ta Pig işleri ile çalışan aşağıdaki bileşenleri sağlar.

* [Azure HDInsight Pig Görev][pigtask]

* [Azure Abonelik Bağlantı Yöneticisi][connectionmanager]

SSIS için Azure Özellik Paketi hakkında daha fazla bilgi için [buradan][ssispack]edinebilirsiniz.

## <a name="next-steps"></a><a id="nextsteps"></a>Sonraki adımlar

HDInsight ile Pig'i kullanmayı öğrendiğiniz için, Azure HDInsight ile çalışmanın diğer yollarını keşfetmek için aşağıdaki bağlantıları kullanın.

* [HDInsight'a veri yükleme](hdinsight-upload-data.md)
* [HDInsight ile Apache Hive'ı kullanma](./hadoop/hdinsight-use-hive.md)
* [HDInsight ile Apache Sqoop kullanın](hdinsight-use-sqoop.md)
* [HDInsight ile MapReduce işleri kullanma](./hadoop/hdinsight-use-mapreduce.md)

[apachepig-home]: https://pig.apache.org/
[putty]: https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: https://curl.haxx.se/
[pigtask]: https://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: https://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: https://msdn.microsoft.com/library/mt146770(v=sql.120).aspx
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]:../hdinsight-use-hive.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[powershell-start]: https://technet.microsoft.com/library/hh847889.aspx


[image-hdi-pig-data-transformation]: ./media/use-pig/hdi-data-transformation.gif
