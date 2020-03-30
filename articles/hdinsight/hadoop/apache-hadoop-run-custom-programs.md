---
title: Özel MapReduce programlarını çalıştırın - Azure HDInsight
description: Azure HDInsight kümelerinde özel Apache MapReduce programlarıne ne zaman ve nasıl çalıştırılanın.
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/01/2020
ms.openlocfilehash: 78623f738285e781cb561a3844db8fbf37226929
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645030"
---
# <a name="run-custom-mapreduce-programs"></a>Özel MapReduce programlarını çalıştırma

HDInsight gibi Apache Hadoop tabanlı büyük veri sistemleri, çok çeşitli araçlar ve teknolojiler kullanarak veri işlemeyi sağlar. Aşağıdaki tabloda her biri için temel avantajlar ve hususlar açıklanmaktadır.

| Sorgu mekanizması | Yararları | Dikkat edilmesi gerekenler |
| --- | --- | --- |
| **HiveQL kullanarak Apache Hive** | <ul><li>Toplu işişleme ve değişmez veri büyük miktarda analizi, veri özetleme ve isteğe bağlı sorgulama için mükemmel bir çözüm. Tanıdık bir SQL benzeri sözdizimi kullanır.</li><li>Kolayca bölümlenebilir ve dizinlenebilir kalıcı veri tabloları üretmek için kullanılabilir.</li><li>Aynı veriler üzerinde birden çok dış tablo ve görünüm oluşturulabilir.</li><li>Veri depolama ve işleme için büyük ölçekleme ve hata toleransı özellikleri sağlayan basit bir veri ambarı uygulamasını destekler.</li></ul> | <ul><li>Kaynak verilerin en azından bazı tanımlanabilir bir yapıya sahip olmasını gerektirir.</li><li>Gerçek zamanlı sorgular ve satır düzeyi güncelleştirmeleri için uygun değildir. En iyi veri büyük kümeleri üzerinde toplu işler için kullanılır.</li><li>Bazı karmaşık işleme görevlerini gerçekleştiremeyebilir.</li></ul> |
| **Domuz Latince kullanarak Apache Pig** | <ul><li>Verileri küme ler olarak işlemek, veri kümelerini birleştirmek ve filtrelemek, kayıtları veya kayıt gruplarına işlevleri uygulamak ve sütunları tanımlayarak, değerleri gruplayarak veya sütunları satırlara dönüştürerek verileri yeniden yapılandırmak için mükemmel bir çözümdür.</li><li>Veriler üzerinde bir dizi işlem olarak iş akışı tabanlı bir yaklaşım kullanabilirsiniz.</li></ul> | <ul><li>SQL kullanıcıları Pig Latin'in HiveQL'den daha az tanıdık ve kullanımı daha zor olduğunu görebilir.</li><li>Varsayılan çıktı genellikle bir metin dosyasıdır ve bu nedenle Excel gibi görselleştirme araçlarıyla kullanımı daha zor olabilir. Genellikle çıktının üzerine bir Hive tablosu katmanı olursunuz.</li></ul> |
| **Özel harita/azaltma** | <ul><li>Harita üzerinde tam denetim sağlar ve aşamaları ve yürütmeyi azaltır.</li><li>Kümeden maksimum performans elde etmek veya sunucular ve ağüzerindeki yükü en aza indirmek için sorguların en iyi duruma getirilmesini sağlar.</li><li>Bileşenler çeşitli iyi bilinen dillerde yazılabilir.</li></ul> | <ul><li>Kendi haritanızı oluşturmanız ve bileşenleri azaltmanız gerektiğinden Pig or Hive'ı kullanmaktan daha zordur.</li><li>Veri kümelerinin birleştirilmesini gerektiren işlemlerin uygulanması daha zordur.</li><li>Test çerçeveleri olsa da, hata ayıklama kodu normal bir uygulamadan daha karmaşıktır, çünkü kod Hadoop iş zamanlayıcısının denetimi altında toplu iş olarak çalışır.</li></ul> |
| **Apache HCatalog** | <ul><li>Depolamanın yol ayrıntılarını özetler, yönetimi kolaylaştırır ve kullanıcıların verilerin nerede depolanır satını bilme ihtiyacını ortadan kaldırır.</li><li>Veri kullanılabilirliği gibi olayların bildirimini sağlayarak, Oozie gibi diğer araçların işlemlerin ne zaman gerçekleştiğini algılamasını sağlar.</li><li>Anahtarla bölümleme de dahil olmak üzere verilerin ilişkisel görünümünü ortaya çıkarır ve verilere erişimi kolaylaştırır.</li></ul> | <ul><li>RCFile, CSV metni, JSON metni, SequenceFile ve ORC dosya biçimlerini varsayılan olarak destekler, ancak diğer biçimler için özel bir SerDe yazmanız gerekebilir.</li><li>HCatalog iş parçacığı için güvenli değildir.</li><li>Pig komut dosyalarında HCatalog yükleyicisini kullanırken sütunlar için veri türlerinde bazı kısıtlamalar vardır. Daha fazla bilgi için Apache HCatalog belgelerindeki [HCatLoader Veri Türleri'ne](https://cwiki.apache.org/confluence/display/Hive/HCatalog%20LoadStore#HCatalogLoadStore-HCatLoaderDataTypes) bakın.</li></ul> |

Genellikle, gereksinim duyduğunuz sonuçları sağlayabilecek bu yaklaşımların en basitini kullanırsınız. Örneğin, sadece Hive kullanarak bu tür sonuçlar elde etmek mümkün olabilir, ancak daha karmaşık senaryolar için Pig kullanmak, hatta kendi harita yazmak ve bileşenleri azaltmak gerekebilir. Ayrıca, Hive veya Pig ile denemeler yaptıktan sonra, özel harita ve bileşenleri azaltmanın, işleme ince ayar ve optimize etme olanağı sağlayarak daha iyi performans sağlayacağına karar verebilirsiniz.

## <a name="custom-mapreduce-components"></a>Özel harita/bileşenleri küçültme

Harita/küçültme **kodu, harita** olarak uygulanan iki ayrı işlevden oluşur ve bileşenleri **azaltır.** **Eşleme** bileşeni, düğümün kendi veri alt kümesine eşlemi uygulayan her düğüm, birden çok küme düğümü üzerinde paralel olarak çalıştırılır. **Azaltma** bileşeni harmanlar ve tüm harita işlevlerinin sonuçlarını özetler. Bu iki bileşen hakkında daha fazla bilgi için, [HDInsight'ta Hadoop'ta MapReduce'i kullanın'](hdinsight-use-mapreduce.md)a bakın.

Çoğu HDInsight işleme senaryosunda, Pig veya Hive gibi daha üst düzey bir soyutlama kullanmak daha basit ve daha verimlidir. Ayrıca, daha karmaşık işleme gerçekleştirmek için özel harita oluşturabilir ve Hive komut dosyaları içinde kullanılmak üzere bileşenleri azaltabilirsiniz.

Özel harita/azaltma bileşenleri genellikle Java'da yazılır. Hadoop, C#, F#, Visual Basic, Python ve JavaScript gibi diğer dillerde geliştirilen bileşenlerin kullanılmasına da izin veren bir akış arabirimi sağlar.

* Özel Java MapReduce programları geliştirme konusunda bir walkthrough için, [HDInsight Hadoop için Java MapReduce programları geliştirin](apache-hadoop-develop-deploy-java-mapreduce-linux.md)bakın.

Kendi haritanızı oluşturmayı düşünün ve aşağıdaki koşullar için bileşenleri azaltın:

* Verileri ayrıştırarak ve yapılandırılmış bilgi elde etmek için özel mantık kullanarak tamamen yapılandırılmamış verileri işlemeniz gerekir.
* Bir UDF oluşturma başvurmadan Pig veya Hive ifade etmek zor (veya imkansız) karmaşık görevleri gerçekleştirmek istiyorum. Örneğin, kaynak verilerdeki enlem ve boylam koordinatlarını veya IP adreslerini coğrafi konum adlarına dönüştürmek için harici bir coğrafi kodlama hizmeti kullanmanız gerekebilir.
* Varolan .NET, Python veya JavaScript kodunuzu haritada yeniden kullanmak/hadoop akış arabirimini kullanarak bileşenleri azaltmak istiyorsunuz.

## <a name="upload-and-run-your-custom-mapreduce-program"></a>Özel MapReduce programınızı yükleyin ve çalıştırın

En yaygın MapReduce programları Java'da yazılır ve bir kavanoz dosyasına derlenir.

1. MapReduce programınızı geliştirdikten, derledikten ve test ettikten `scp` sonra, kavanoz dosyanızı kafa düğümüne yüklemek için komutu kullanın.

    ```cmd
    scp mycustomprogram.jar sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    CLUSTERNAME'yi küme adı ile değiştirin. SSH hesabını güvenli hale getirmek için bir parola kullandıysanız, parolayı girmeniz istenir. Bir sertifika kullandıysanız, özel anahtar `-i` dosyasını belirtmek için parametreyi kullanmanız gerekebilir.

1. Kümenize bağlanmak için [ssh komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME'yi kümenizin adıyla değiştirerek aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. SSH oturumundan MapReduce programınızı YARN aracılığıyla çalıştırın.

    ```bash
    yarn jar mycustomprogram.jar mynamespace.myclass /example/data/sample.log /example/data/logoutput
    ```

    Bu komut, MapReduce işini İPN'e gönderir. Giriş dosyası `/example/data/sample.log`ve çıktı dizini . `/example/data/logoutput` Giriş dosyası ve çıktı dosyaları küme için varsayılan depolama alanına depolanır.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight'ta Apache Hadoop'ta MapReduce akışı yla C# kullanın](apache-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [HDInsight'ta Apache Hadoop için Java MapReduce programları geliştirin](apache-hadoop-develop-deploy-java-mapreduce-linux.md)
* [Bir HDInsight kümesi için Apache Spark uygulamaları oluşturmak için Eclipse için Azure Araç Kiti'ni kullanın](../spark/apache-spark-eclipse-tool-plugin.md)
* [HDInsight'ta Apache Hive ve Apache Pig ile Python Kullanıcı Tanımlı Fonksiyonlar (UDF) kullanın](python-udf-hdinsight.md)
