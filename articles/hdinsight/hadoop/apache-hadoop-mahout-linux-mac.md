---
title: Azure HDInsight'ta Apache Mahout kullanarak öneriler oluşturun
description: HDInsight (Hadoop) ile film önerileri oluşturmak için Apache Mahout makine öğrenimi kitaplığını nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/03/2020
ms.openlocfilehash: 33110e9f1d45fcd11e5f4cad1b589ab929a9472d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75767645"
---
# <a name="generate-movie-recommendations-using-apache-mahout-with-apache-hadoop-in-hdinsight-ssh"></a>HDInsight (SSH) içinde Apache Hadoop ile Apache Mahout kullanarak film önerileri oluşturun

[!INCLUDE [mahout-selector](../../../includes/hdinsight-selector-mahout.md)]

Film önerileri oluşturmak için Azure HDInsight ile [Apache Mahout](https://mahout.apache.org) makine öğrenimi kitaplığını nasıl kullanacağınızı öğrenin.

Mahout, Apache Hadoop için bir [makine öğrenimi](https://en.wikipedia.org/wiki/Machine_learning) kütüphanesidir. Mahout, filtreleme, sınıflandırma ve kümeleme gibi verileri işlemek için algoritmalar içerir. Bu makalede, arkadaşlarınızın gördüğü filmleri temel alan film önerileri oluşturmak için bir öneri altyapısı kullanırsınız.

## <a name="prerequisites"></a>Ön koşullar

HDInsight'ta bir Apache Hadoop kümesi. [Linux'ta HDInsight ile başlayın](./apache-hadoop-linux-tutorial-get-started.md)bakın.

## <a name="apache-mahout-versioning"></a>Apache Mahout sürümü

HDInsight'ta Mahout sürümü hakkında daha fazla bilgi için [HDInsight sürümlerine ve Apache Hadoop bileşenlerine](../hdinsight-component-versioning.md)bakın.

## <a name="understanding-recommendations"></a>Önerileri anlama

Mahout tarafından sağlanan işlevlerden biri bir tavsiye motorudur. Bu altyapı `userID`, , `itemId`ve `prefValue` (öğe için tercih) biçiminde veri kabul eder. Mahout daha sonra belirlemek için ortak olay çözümlemesi gerçekleştirebilir: *bir öğe için tercihi olan kullanıcılar da bu diğer öğeler için bir tercihvar.* Mahout daha sonra, önerilerde bulunmak için kullanılabilecek öğe beğenme tercihlerine sahip kullanıcıları belirler.

Aşağıdaki iş akışı, film verilerini kullanan basitleştirilmiş bir örnektir:

* **Co-olay**: Joe, Alice ve Bob tüm *Star Wars*sevdim , Empire *Strikes Back*, ve Jedi *Dönüşü*. Mahout, bu filmlerden herhangi birini beğenen kullanıcıların da diğer ikisi gibi olduğunu belirler.

* **Co-occurrence**: Bob ve Alice de *Phantom Menace*sevdim , *Klonların Saldırı*, ve *Sith İntikamı*. Mahout, önceki üç filmi beğenen kullanıcıların da bu üç filmi beğendiğini belirler.

* **Benzerlik önerisi**: Joe ilk üç filmi beğendiği için Mahout, benzer tercihlere sahip olan ların beğendiği filmlere bakar, ancak Joe izlememiştir (beğenildi/derecelendirilmiştir). Bu durumda, Mahout *Hayalet Tehdit*önerir , *Klonların Saldırı*, ve *Sith İntikamı*.

### <a name="understanding-the-data"></a>Verileri anlama

[GroupLens Research,](https://grouplens.org/datasets/movielens/) filmler için Mahout ile uyumlu bir formatta derecelendirme verileri sağlar. Bu veriler kümenizin varsayılan depolama `/HdiSamples/HdiSamples/MahoutMovieData`'da kullanılabilir.

İki dosya var `moviedb.txt` `user-ratings.txt`ve. Dosya `user-ratings.txt` çözümleme sırasında kullanılır. Sonuçlar `moviedb.txt` görüntülenirken kullanıcı dostu metin bilgileri sağlamak için kullanılır.

İçerdiği `user-ratings.txt` veriler, `userID` `movieID` `userRating` `timestamp`her kullanıcının bir filmi ne kadar yüksek derecelendirdiğini gösteren bir yapıya sahiptir. Aşağıda verilerin bir örneği verilmiştir:

    196    242    3    881250949
    186    302    3    891717742
    22     377    1    878887116
    244    51     2    880606923
    166    346    1    886397596

## <a name="run-the-analysis"></a>Analizi çalıştırın

1. Kümenize bağlanmak için [ssh komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME'yi kümenizin adıyla değiştirerek aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Öneri işini çalıştırmak için aşağıdaki komutu kullanın:

    ```bash
    mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp
    ```

> [!NOTE]  
> İşin tamamlanması birkaç dakika sürebilir ve birden çok MapReduce işi çalıştırabilir.

## <a name="view-the-output"></a>Çıktıyı görüntüleme

1. İş tamamlandıktan sonra, oluşturulan çıktıyı görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -text /example/data/mahoutout/part-r-00000
    ```

    Çıktı aşağıdaki gibi görünür:

    ```output
    1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]
    ```

    İlk `userID`sütun. '[' ve ']' içinde `movieId`yer`recommendationScore`alan değerler şunlardır: .

2. Öneriler hakkında daha fazla bilgi sağlamak için moviedb.txt ile birlikte çıktıyı kullanabilirsiniz. İlk olarak, aşağıdaki komutları kullanarak dosyaları yerel olarak kopyalayın:

    ```bash
    hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
    hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .
    ```

    Bu komut, çıktı verilerini film veri dosyalarıyla birlikte geçerli dizindeki **recommendations.txt** adlı bir dosyaya kopyalar.

3. Öneriler çıktısındaki verilerin film adlarını arayan bir Python komut dosyası oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    nano show_recommendations.py
    ```

    Düzenleyici açıldığında, dosyanın içeriği olarak aşağıdaki metni kullanın:

   ```python
   #!/usr/bin/env python

   import sys

   if len(sys.argv) != 5:
        print "Arguments: userId userDataFilename movieFilename recommendationFilename"
        sys.exit(1)

   userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

   print "Reading Movies Descriptions"
   movieFile = open(movieFilename)
   movieById = {}
   for line in movieFile:
       tokens = line.split("|")
       movieById[tokens[0]] = tokens[1:]
   movieFile.close()

   print "Reading Rated Movies"
   userDataFile = open(userDataFilename)
   ratedMovieIds = []
   for line in userDataFile:
       tokens = line.split("\t")
       if tokens[0] == userId:
           ratedMovieIds.append((tokens[1],tokens[2]))
   userDataFile.close()

   print "Reading Recommendations"
   recommendationFile = open(recommendationFilename)
   recommendations = []
   for line in recommendationFile:
       tokens = line.split("\t")
       if tokens[0] == userId:
           movieIdAndScores = tokens[1].strip("[]\n").split(",")
           recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
           break
   recommendationFile.close()

   print "Rated Movies"
   print "------------------------"
   for movieId, rating in ratedMovieIds:
       print "%s, rating=%s" % (movieById[movieId][0], rating)
   print "------------------------"

   print "Recommended Movies"
   print "------------------------"
   for movieId, score in recommendations:
       print "%s, score=%s" % (movieById[movieId][0], score)
   print "------------------------"
   ```

    **Ctrl-X**, **Y**ve son olarak verileri kaydetmek için **girin'** e basın.

4. Python komut dosyasını çalıştırın. Aşağıdaki komut, tüm dosyaların indirildiği dizinde olduğunuzu varsayar:

    ```bash
    python show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt
    ```

    Bu komut, kullanıcı kimliği 4 için oluşturulan önerilere bakar.

   * **User-ratings.txt** dosyası, derecelendirilen filmleri almak için kullanılır.

   * **Moviedb.txt** dosyası filmlerin adlarını almak için kullanılır.

   * **Recommendations.txt** bu kullanıcı için film önerilerini almak için kullanılır.

     Bu komutun çıktısı aşağıdaki metne benzer:

        ```output
        Seven Years in Tibet (1997), score=5.0
        Indiana Jones and the Last Crusade (1989), score=5.0
        Jaws (1975), score=5.0
        Sense and Sensibility (1995), score=5.0
        Independence Day (ID4) (1996), score=5.0
        My Best Friend's Wedding (1997), score=5.0
        Jerry Maguire (1996), score=5.0
        Scream 2 (1997), score=5.0
        Time to Kill, A (1996), score=5.0
        ```

## <a name="delete-temporary-data"></a>Geçici verileri silme

Mahout işleri, işi işlerken oluşturulan geçici verileri kaldırmaz. Parametre, `--tempDir` geçici dosyaları kolay silme için belirli bir yola yalıtmak için örnek iş belirtilir. Geçici dosyaları kaldırmak için aşağıdaki komutu kullanın:

```bash
hdfs dfs -rm -f -r /temp/mahouttemp
```

> [!WARNING]  
> Komutu yeniden çalıştırmak istiyorsanız, çıktı dizini de silmeniz gerekir. Bu dizini silmek için aşağıdakileri kullanın:
>
> `hdfs dfs -rm -f -r /example/data/mahoutout`

## <a name="next-steps"></a>Sonraki adımlar

Mahout'u nasıl kullanacağınızı öğrendiğiniz için, HDInsight'taki verilerle çalışmanın başka yollarını keşfedin:

* [HDInsight ile Apache Hive](hdinsight-use-hive.md)
* [HDInsight ile MapReduce](hdinsight-use-mapreduce.md)
