---
title: Apache Hadoop Haritasını ÇALıŞTıRHDInsight'taki örnekleri azaltın - Azure
description: HDInsight'ta yer alan kavanoz dosyalarındaki MapReduce örneklerini kullanmaya başlayın. Kümeye bağlanmak için SSH'yi kullanın ve örnek işleri çalıştırmak için Hadoop komutunu kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 12/12/2019
ms.openlocfilehash: 58f7d99af638c8d03bbce46b7fcf8204aaca11d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75435744"
---
# <a name="run-the-mapreduce-examples-included-in-hdinsight"></a>HDInsight'ta yer alan MapReduce örneklerini çalıştırın

[!INCLUDE [samples-selector](../../../includes/hdinsight-run-samples-selector.md)]

HDInsight'ta Apache Hadoop ile birlikte verilen MapReduce örneklerini nasıl çalıştıracağınız hakkında bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight'ta bir Apache Hadoop kümesi. [Linux'ta HDInsight ile başlayın](./apache-hadoop-linux-tutorial-get-started.md)bakın.

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](../hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

## <a name="the-mapreduce-examples"></a>MapReduce örnekleri

Örnekler HDInsight kümesinde yer `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`alır. Bu örneklerin kaynak kodu HDInsight kümesinde `/usr/hdp/current/hadoop-client/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`yer alan .

Aşağıdaki örnekler bu arşivde yer almaktadır:

|Örnek |Açıklama |
|---|---|
|toplam kelime sayısı|Giriş dosyalarındaki sözcükleri sayar.|
|aggregatewordhist|Giriş dosyalarındaki sözcüklerin histogramını hesaplar.|
|Bbp|Pi'nin tam rakamlarını hesaplamak için Bailey-Borwein-Plouffe'u kullanır.|
|dbsayısı|Veritabanında depolanan sayfa görünümü günlüklerini sayar.|
|distbbp|Pi'nin tam bitlerini hesaplamak için BBP tipi bir formül kullanır.|
|grep|Girişteki bir regex'in eşleşmelerini sayar.|
|join|Sıralanmış, eşit bölümlenmiş veri kümeleri üzerinde birbirleştirme gerçekleştirir.|
|multifilewc|Çeşitli dosyalardaki sözcükleri sayar.|
|pentomino|Kiremit döşeme programı pentomino sorunlarına çözüm bulmak için.|
|Pi|Pi'yi yarı Monte Carlo yöntemiyle tahmin eder.|
|randomtextwriter|Düğüm başına 10 GB rasgele metin seli yazar.|
|randomwriter|Düğüm başına 10 GB rasgele veri yazar.|
|ikincil sıralama|Azaltma aşamasına ikincil bir sıralama tanımlar.|
|sort|Rasgele yazar tarafından yazılan verileri sıralar.|
|Sudoku|Bir sudoku çözücüsü.|
|terajen|Terasort için veri oluşturun.|
|terasort|Terasort'u çalıştırın.|
|teravalidate|Terasort sonuçlarını kontrol ediyorum.|
|sözcük sayısı|Giriş dosyalarındaki sözcükleri sayar.|
|kelime anlamı|Giriş dosyalarındaki sözcüklerin ortalama uzunluğunu sayar.|
|kelime median|Giriş dosyalarındaki sözcüklerin ortanca uzunluğunu sayar.|
|kelime standart disinasyonu|Giriş dosyalarındaki sözcüklerin uzunluğunun standart sapması sayar.|

## <a name="run-the-wordcount-example"></a>Sözcük sayısı örneğini çalıştırma

1. SSH'yi kullanarak HDInsight'a bağlanın. Kümenizin adıyla değiştirin `CLUSTER` ve ardından aşağıdaki komutu girin:

    ```cmd
    ssh sshuser@CLUSTER-ssh.azurehdinsight.net
    ```

2. SSH oturumundan, örnekleri listelemek için aşağıdaki komutu kullanın:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar
    ```

    Bu komut, bu belgenin önceki bölümünden örnek listesini oluşturur.

3. Belirli bir örnek için yardım almak için aşağıdaki komutu kullanın. Bu durumda, **sözcük sayısı** örneği:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount
    ```

    Aşağıdaki iletiyi alırsınız:

    ```output
    Usage: wordcount <in> [<in>...] <out>
    ```

    Bu ileti, kaynak belgeler için birkaç giriş yolu sağlayabileceğinizi gösterir. Son yol, çıktının (kaynak belgelerdeki sözcük sayısı) depolandığı yerdir.

4. Kümenizde örnek veri olarak sağlanan Leonardo da Vinci'nin Not Defterlerinde yer alan tüm sözcükleri saymak için aşağıdakileri kullanın:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount
    ```

    Bu iş için giriş `/example/data/gutenberg/davinci.txt`okunur. Bu örnekteki çıktı `/example/data/davinciwordcount`. Her iki yol da yerel dosya sistemi için değil, küme için varsayılan depolama da bulunur.

   > [!NOTE]  
   > Wordcount örneği için yardım belirtildiği gibi, birden çok giriş dosyaları belirtebilirsiniz. Örneğin, `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` hem davinci.txt hem de ulysses.txt sözcükleri sayar.

5. İş tamamlandıktan sonra çıktıyı görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -cat /example/data/davinciwordcount/*
    ```

    Bu komut, iş tarafından üretilen tüm çıktı dosyalarını birleştirir. Çıkışı konsola gösterir. Çıktı aşağıdaki metne benzer:

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    Her satır bir sözcüğü ve giriş verilerinde kaç kez oluştuğunu temsil eder.

## <a name="the-sudoku-example"></a>Sudoku örneği

[Sudoku](https://en.wikipedia.org/wiki/Sudoku) dokuz 3x3 ızgaralardan oluşan bir mantık bulmacasi. Kılavuzdaki bazı hücrelerin numaraları bulunurken, diğerleri boştur ve amaç boş hücreler için çözmektir. Önceki bağlantı bulmaca hakkında daha fazla bilgiye sahiptir, ancak bu örneğin amacı boş hücreler için çözmektir. Yani bizim giriş aşağıdaki biçimde bir dosya olmalıdır:

* Dokuz sütundan oluşan dokuz satır
* Her sütun bir sayı `?` içerebilir veya (boş bir hücreyi gösterir)
* Hücreler bir boşlukla ayrılır

Sudoku bulmacalar oluşturmak için belirli bir yolu vardır; bir sütunda veya satırda bir sayıyı yineleyemezsiniz. HDInsight kümesinde düzgün bir şekilde oluşturulmuş bir örnek var. Bu yer alır `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` ve aşağıdaki metni içerir:

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

Bu örnek sorunu Sudoku örneğinde çalıştırmak için aşağıdaki komutu kullanın:

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta
```

Sonuçlar aşağıdaki metne benzer görünür:

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi--example"></a>Pi (π) örneği

Pi örneği pi değerini tahmin etmek için istatistiksel (quasi-Monte Carlo) yöntemi kullanır. Noktalar bir birim kare rastgele yerleştirilir. Kare de bir daire içerir. Noktaların daire nin içine düşme olasılığı, pi/4 olan dairenin alanına eşittir. pi değeri 4R değerinden tahmin edilebilir. R, daire içinde bulunan nokta sayısının kare içindeki toplam nokta sayısına oranıdır. Kullanılan noktaların örneği ne kadar büyükse, tahmin o kadar iyi.

Bu örneği çalıştırmak için aşağıdaki komutu kullanın. Bu komut pi değerini tahmin etmek için her biri 10.000.000 örnek içeren 16 harita kullanır:

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000
```

Bu komut tarafından döndürülen değer 3.1415915500000000000000000000'e benzer. **3.14159155000000000000** Referanslar için pi'nin ilk 10 ondalık yeri 3.1415926535'tir.

## <a name="10-gb-graysort-example"></a>10 GB GraySort örneği

GraySort bir kıyaslama sıralamasıdır. Metrik, genellikle en az 100 TB olan büyük miktarda veriyi sıralarken elde edilen sıralama oranıdır (TB/dakika).

Bu örnek, nispeten hızlı bir şekilde çalıştırılabilmek için mütevazı bir 10 GB veri kullanır. Owen O'Malley ve Arun Murthy tarafından geliştirilen MapReduce uygulamalarını kullanır. Bu uygulamalar 2009 yılında 0,578 TB/dk (173 dakikada 100 TB) oranıyla yıllık genel amaçlı ("Daytona") terabayt sıralama kriterini kazandı. Bu ve diğer sıralama kriterleri hakkında daha fazla bilgi [için, Sıralama Kıyaslama](https://sortbenchmark.org/) sitesine bakın.

Bu örnek, mapreduce programlarıüç set kullanır:

* **TeraGen**: Sıralamak için veri satırları üreten bir MapReduce programı

* **TeraSort**: Giriş verilerini örnekler ve verileri toplam sıraya sıralamak için MapReduce'i kullanır

    TeraSort standart bir MapReduce sıralama, özel bir bölümleyici dışında. Bölümleyici, her azaltma için anahtar aralığını tanımlayan N-1 örneklenmiş anahtarların sıralanmış bir listesini kullanır. Özellikle, tüm tuşları gibi örnek[i-1] <= anahtar < örnek[i] i azaltmak için gönderilir. Bu bölümleyici, i azaltma çıktılarının i+1'in çıktılarından daha az olduğunu garanti eder.

* **TeraValidate**: Çıktının genel olarak sıralanmış olduğunu doğrulayan bir MapReduce programı

    Çıktı dizininde dosya başına bir harita oluşturur ve her harita, her anahtarın bir öncekinden daha az veya eşit olmasını sağlar. Eşleþme işlevi, her dosyanın ilk ve son anahtarlarının kayıtlarını çiretir. Azaltma işlevi, dosya i'nin ilk anahtarının i-1 dosyasının son anahtarından daha büyük olmasını sağlar. Herhangi bir sorun, sıra dışı anahtarlarla azaltma aşamasının çıktısı olarak bildirilir.

Veri oluşturmak, sıralamak ve sonra çıktıyı doğrulamak için aşağıdaki adımları kullanın:

1. HDInsight kümesinin varsayılan depolama alanına depolanan 10 GB veri `/example/data/10GB-sort-input`oluşturun:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input
    ```

    Hadoop'a `-Dmapred.map.tasks` bu iş için kaç harita görevi kullanılacağını söyler. Son iki parametre, işi 10 GB veri oluşturmave `/example/data/10GB-sort-input`'de depolamak için talimat verir.

2. Verileri sıralamak için aşağıdaki komutu kullanın:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output
    ```

    Hadoop'a `-Dmapred.reduce.tasks` iş için kullanılacak görevleri kaç kişi azalttığını söyler. Son iki parametre veri için giriş ve çıkış konumları dır.

3. Sıralama tarafından oluşturulan verileri doğrulamak için aşağıdakileri kullanın:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Linux tabanlı HDInsight kümeleri ile birlikte verilen örnekleri nasıl çalıştırabileceğinizi öğrendiniz. HDInsight ile Pig, Hive ve MapReduce'i kullanma yla ilgili eğitimler için aşağıdaki konulara bakın:

* [HDInsight'ta Apache Hadoop ile Apache Hive kullanın](hdinsight-use-hive.md)
* [HDInsight'ta Apache Hadoop ile MapReduce'ı kullanın](hdinsight-use-mapreduce.md)