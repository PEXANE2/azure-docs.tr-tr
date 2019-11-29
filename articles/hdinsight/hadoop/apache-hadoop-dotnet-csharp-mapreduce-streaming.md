---
title: HDInsight C# 'ta Hadoop 'Ta MapReduce ile birlikte kullanma-Azure
description: Azure HDInsight 'ta Apache Hadoop C# MapReduce çözümleri oluşturmak için kullanmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/22/2019
ms.openlocfilehash: 025b5c5c1e3b8543111e112202906ef6f1fdb482
ms.sourcegitcommit: c31dbf646682c0f9d731f8df8cfd43d36a041f85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2019
ms.locfileid: "74561794"
---
# <a name="use-c-with-mapreduce-streaming-on-apache-hadoop-in-hdinsight"></a>HDInsight C# 'Ta Apache Hadoop MapReduce streaming ile kullanma

HDInsight 'ta MapReduce çözümü oluşturmak için kullanmayı C# öğrenin.

Apache Hadoop akışı, MapReduce işlerini bir betik veya yürütülebilir dosya kullanarak çalıştırmanıza olanak tanıyan bir yardımcı programdır. Bu örnekte, bir sözcük sayısı çözümü için Mapper ve Reducer uygulamak üzere .NET kullanılır.

## <a name="net-on-hdinsight"></a>HDInsight üzerinde .NET

HDInsight kümeleri, .NET uygulamalarını çalıştırmak için [Mono (https://mono-project.com)](https://mono-project.com) kullanır. Tek bir sürüm 4.2.1, HDInsight sürüm 3,6 ' ye dahildir. HDInsight 'ta bulunan mono sürümü hakkında daha fazla bilgi için bkz. [farklı HDInsight sürümleriyle kullanılabilen Apache Hadoop bileşenleri](../hdinsight-component-versioning.md#apache-hadoop-components-available-with-different-hdinsight-versions).

.NET Framework sürümleriyle mono uyumluluğu hakkında daha fazla bilgi için bkz. [mono uyumluluk](https://www.mono-project.com/docs/about-mono/compatibility/).

## <a name="how-hadoop-streaming-works"></a>Hadoop akışı nasıl kullanılır?

Bu belgede akış için kullanılan temel işlem aşağıdaki gibidir:

1. Hadoop, STDIN 'de verileri eşleştiriciyle (Bu örnekteki*Mapper. exe* ) geçirir.
2. Eşleyici, verileri işler ve STDOUT için sekmeyle ayrılmış anahtar/değer çiftlerini yayar.
3. Çıktı, Hadoop tarafından okunarak (Bu örnekte*Reducer. exe* ) Reducer 'e geçirilir.
4. Reducer, sekmeyle ayrılmış anahtar/değer çiftlerini okur, verileri işler ve sonra sonucu STDOUT üzerinde sekmeyle ayrılmış anahtar/değer çiftleri olarak yayar.
5. Çıktı, Hadoop tarafından okunmakta ve çıkış dizinine yazılır.

Akış hakkında daha fazla bilgi için bkz. [Hadoop akışı](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).

## <a name="prerequisites"></a>Önkoşullar

* Visual Studio.

* .NET Framework 4,5 ' i hedefleyen kod C# yazma ve oluşturma hakkında bir benzerlik.

* . Exe dosyalarını kümeye yüklemek için bir yol. Bu belgedeki adımlarda, dosyaları küme için birincil depolamaya yüklemek üzere Visual Studio için Data Lake araçları kullanılır.

* PowerShell kullanıyorsanız [az Module](https://docs.microsoft.com/powershell/azure/overview)gerekecektir.

* Bir SSH istemcisi (isteğe bağlı). Daha fazla bilgi için bkz. [SSH kullanarak HDInsight 'A bağlanma (Apache Hadoop)](../hdinsight-hadoop-linux-use-ssh-unix.md).

* HDInsight üzerinde bir Apache Hadoop kümesi. Bkz. [Linux 'Ta HDInsight kullanmaya başlama](../hadoop/apache-hadoop-linux-tutorial-get-started.md).

* Kümelerinizin birincil depolama alanı için [URI şeması](../hdinsight-hadoop-linux-information.md#URI-and-scheme) . Bu, Azure depolama için `wasb://`, Azure Data Lake Storage 1. için Azure Data Lake Storage 2. veya `adl://` `abfs://`. Azure depolama veya Data Lake Storage 2. için güvenli aktarım etkinse, URI `wasbs://` veya `abfss://`sırasıyla, [Güvenli aktarım](../../storage/common/storage-require-secure-transfer.md)' i de görür.


## <a name="create-the-mapper"></a>Eşleyici oluşturma

Visual Studio 'da *Eşleyici*adlı yeni bir .NET Framework konsol uygulaması oluşturun. Uygulama için aşağıdaki kodu kullanın:

```csharp
using System;
using System.Text.RegularExpressions;

namespace mapper
{
    class Program
    {
        static void Main(string[] args)
        {
            string line;
            //Hadoop passes data to the mapper on STDIN
            while((line = Console.ReadLine()) != null)
            {
                // We only want words, so strip out punctuation, numbers, etc.
                var onlyText = Regex.Replace(line, @"\.|;|:|,|[0-9]|'", "");
                // Split at whitespace.
                var words = Regex.Matches(onlyText, @"[\w]+");
                // Loop over the words
                foreach(var word in words)
                {
                    //Emit tab-delimited key/value pairs.
                    //In this case, a word and a count of 1.
                    Console.WriteLine("{0}\t1",word);
                }
            }
        }
    }
}
```

Uygulamayı oluşturduktan sonra, */bin/Debug/Mapper.exe* dosyasını proje dizininde oluşturmak için oluşturun.

## <a name="create-the-reducer"></a>Reducer oluşturma

Visual Studio 'da, *Reducer*adlı yeni bir .NET Framework konsol uygulaması oluşturun. Uygulama için aşağıdaki kodu kullanın:

```csharp
using System;
using System.Collections.Generic;

namespace reducer
{
    class Program
    {
        static void Main(string[] args)
        {
            //Dictionary for holding a count of words
            Dictionary<string, int> words = new Dictionary<string, int>();

            string line;
            //Read from STDIN
            while ((line = Console.ReadLine()) != null)
            {
                // Data from Hadoop is tab-delimited key/value pairs
                var sArr = line.Split('\t');
                // Get the word
                string word = sArr[0];
                // Get the count
                int count = Convert.ToInt32(sArr[1]);

                //Do we already have a count for the word?
                if(words.ContainsKey(word))
                {
                    //If so, increment the count
                    words[word] += count;
                } else
                {
                    //Add the key to the collection
                    words.Add(word, count);
                }
            }
            //Finally, emit each word and count
            foreach (var word in words)
            {
                //Emit tab-delimited key/value pairs.
                //In this case, a word and a count of 1.
                Console.WriteLine("{0}\t{1}", word.Key, word.Value);
            }
        }
    }
}
```

Uygulamayı oluşturduktan sonra, */bin/Debug/Reducer.exe* dosyasını proje dizininde oluşturmak için oluşturun.

## <a name="upload-to-storage"></a>Depolama alanına yükleme

Ardından, *Eşleyici* ve *Reducer* uygulamalarını HDInsight depolama alanına yüklemeniz gerekir.

1. Visual Studio 'da > Sunucu Gezgini **görüntüle** 'yi seçin.

1. **Azure**' a sağ tıklayın, **Microsoft Azure aboneliğine Bağlan...** öğesini seçin ve oturum açma işlemini doldurun.

1. Bu uygulamayı dağıtmak istediğiniz HDInsight kümesini genişletin. Metin **(varsayılan depolama hesabı)** içeren bir giriş listelenir.

   ![Depolama hesabı, HDInsight kümesi, Sunucu Gezgini, Visual Studio](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/hdinsight-storage-account.png)

   * **(Varsayılan depolama hesabı)** girişi genişletilirse, küme için varsayılan depolama alanı olarak bir **Azure depolama hesabı** kullanıyorsunuz demektir. Küme için varsayılan depolamada bulunan dosyaları görüntülemek için, girdiyi genişletin ve **(varsayılan kapsayıcı)** öğesine çift tıklayın.

   * **(Varsayılan depolama hesabı)** girdisi genişletilirse, küme için varsayılan depolama alanı olarak **Azure Data Lake Storage** kullanırsınız. Küme için varsayılan depolamada bulunan dosyaları görüntülemek için **(varsayılan depolama hesabı)** girişine çift tıklayın.

1. . Exe dosyalarını karşıya yüklemek için aşağıdaki yöntemlerden birini kullanın:

    * Bir **Azure depolama hesabı**kullanıyorsanız, **karşıya yükleme blobu** simgesini seçin.

        ![Eşleyici için HDInsight karşıya yükleme simgesi, Visual Studio](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/hdinsight-upload-icon.png)

        **Yeni dosyayı karşıya yükle** Iletişim kutusundaki **dosya adı**altında, **Araştır**' ı seçin. **Blobu karşıya yükle** iletişim kutusunda *Eşleyici* projesi için *bin\Debug* klasörüne gidin ve ardından *Mapper. exe* dosyasını seçin. Son olarak, **Aç** ' ı ve ardından **Tamam** ' ı seçerek karşıya yüklemeyi tamamlayabilirsiniz.

    * **Azure Data Lake Storage**için, dosya listesinde boş bir alana sağ tıklayın ve ardından **karşıya yükle**' yi seçin. Son olarak, *Mapper. exe* dosyasını seçin ve sonra **Aç**' ı seçin.

    *Eşleyici. exe* karşıya yükleme işlemi tamamlandıktan sonra, *Reducer. exe* dosyası için karşıya yükleme işlemini tekrarlayın.

## <a name="run-a-job-using-an-ssh-session"></a>İş çalıştırma: SSH oturumu kullanma

Aşağıdaki yordam bir SSH oturumu kullanarak MapReduce işinin nasıl çalıştırılacağını açıklamaktadır:

1. Kümenize bağlanmak için [SSH komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME öğesini kümenizin adıyla değiştirerek aşağıdaki komutu düzenleyin ve ardından şu komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. MapReduce işini başlatmak için aşağıdaki komutlardan birini kullanın:

   * Varsayılan depolama alanı **Azure depolama**ise:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar \
            -files wasbs:///mapper.exe,wasbs:///reducer.exe \
            -mapper mapper.exe \
            -reducer reducer.exe \
            -input /example/data/gutenberg/davinci.txt \
            -output /example/wordcountout
        ```

    * Varsayılan depolama **Data Lake Storage 1.** :

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar \
            -files adl:///mapper.exe,adl:///reducer.exe \
            -mapper mapper.exe \
            -reducer reducer.exe \
            -input /example/data/gutenberg/davinci.txt \
            -output /example/wordcountout
        ```

   * Varsayılan depolama **Data Lake Storage 2.** :

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar \
            -files abfs:///mapper.exe,abfs:///reducer.exe \
            -mapper mapper.exe \
            -reducer reducer.exe \
            -input /example/data/gutenberg/davinci.txt \
            -output /example/wordcountout
        ```

   Aşağıdaki listede her bir parametre ve seçeneğin neyi gösterdiği açıklanmaktadır:

   * *Hadoop-streaming. jar*: akış MapReduce işlevini içeren jar dosyasını belirtir.
   * `-files`: Bu iş için *Mapper. exe* ve *Reducer. exe* dosyalarını belirtir. Her dosyadan önce `wasbs:///`, `adl:///`veya `abfs:///` protokol bildirimi, kümenin varsayılan depolama kökünün yoludur.
   * `-mapper`: Eşleyici 'yi uygulayan dosyayı belirtir.
   * `-reducer`: Reducer uygulayan dosyayı belirtir.
   * `-input`: giriş verilerini belirtir.
   * `-output`: çıkış dizinini belirtir.

3. MapReduce işi tamamlandığında, sonuçları görüntülemek için aşağıdaki komutu kullanın:

   ```bash
   hdfs dfs -text /example/wordcountout/part-00000
   ```

   Aşağıdaki metin, bu komutun döndürdüğü verilerin bir örneğidir:

   ```output
   you     1128
   young   38
   younger 1
   youngest        1
   your    338
   yours   4
   yourself        34
   yourselves      3
   youth   17
   ```

## <a name="run-a-job-using-powershell"></a>İş çalıştırma: PowerShell kullanma

Bir MapReduce işini çalıştırmak ve sonuçları indirmek için aşağıdaki PowerShell betiğini kullanın.

[!code-powershell[main](../../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]

Bu betik, HDInsight küme adı ile birlikte küme oturum açma hesabı adı ve parolasını ister. İş tamamlandıktan sonra çıktı *çıktı. txt*adlı bir dosyaya indirilir. Aşağıdaki metin `output.txt` dosyadaki verilerin bir örneğidir:

```output
you     1128
young   38
younger 1
youngest        1
your    338
yours   4
yourself        34
yourselves      3
youth   17
```

## <a name="next-steps"></a>Sonraki adımlar

HDInsight ile MapReduce kullanma hakkında daha fazla bilgi için bkz. [HDInsight 'ta Apache Hadoop MapReduce](hdinsight-use-mapreduce.md)kullanma.

Hive ve Pig ile C# kullanma hakkında bilgi için bkz. [Apache Hive ve C# Apache Pig Ile Kullanıcı tanımlı bir işlev kullanma](apache-hadoop-hive-pig-udf-dotnet-csharp.md).

HDInsight 'ta fırtınası ile C# kullanma hakkında bilgi için bkz. [HDInsight C# 'Ta Apache Storm için topolojiler geliştirme](../storm/apache-storm-develop-csharp-visual-studio-topology.md).
