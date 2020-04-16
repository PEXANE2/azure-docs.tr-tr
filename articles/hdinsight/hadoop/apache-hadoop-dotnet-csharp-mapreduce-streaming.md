---
title: HDInsight'ta Hadoop'ta MapReduce ile C# kullanın - Azure
description: Azure HDInsight'ta Apache Hadoop ile MapReduce çözümleri oluşturmak için C# 'ı nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/15/2020
ms.openlocfilehash: 7a299ce16f6e9c7292cebf198c9c3077f8e05fcb
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81417601"
---
# <a name="use-c-with-mapreduce-streaming-on-apache-hadoop-in-hdinsight"></a>HDInsight'ta Apache Hadoop'ta MapReduce akışı yla C# kullanın

HDInsight'ta Bir MapReduce çözümü oluşturmak için C# nasıl kullanılacağını öğrenin.

Apache Hadoop akışı, mapreduce işlerini bir komut dosyası veya çalıştırılabilir kullanarak çalıştırmanızı sağlar. Burada, .NET bir sözcük sayısı çözümü için mapper ve redüktör uygulamak için kullanılır.

## <a name="net-on-hdinsight"></a>.NET HDInsight üzerinde

HDInsight kümeleri [Monohttps://mono-project.com) (.NET](https://mono-project.com) uygulamalarını çalıştırmak için) kullanır. Mono sürüm 4.2.1 HDInsight sürüm 3.6 ile birlikte verilir. HDInsight ile birlikte mono sürümü hakkında daha fazla bilgi için, [farklı HDInsight sürümleri ile kullanılabilir Apache Hadoop bileşenleri](../hdinsight-component-versioning.md#apache-hadoop-components-available-with-different-hdinsight-versions)bakın.

.NET Framework sürümleriyle Mono uyumluluğu hakkında daha fazla bilgi [için](https://www.mono-project.com/docs/about-mono/compatibility/)bkz.

## <a name="how-hadoop-streaming-works"></a>Hadoop akışı nasıl çalışır?

Bu belgede akış için kullanılan temel işlem aşağıdaki gibidir:

1. Hadoop, verileri STDIN'deki mapper'a (bu örnekte*mapper.exe)* aktar.
2. Mapper verileri işler ve sekme-sınırlandırılmış anahtar/değer çiftleri stdout yalar.
3. Çıktı Hadoop tarafından okunur ve daha sonra STDIN'deki redüktöre (bu örnekte*reducer.exe)* geçer.
4. Azaltıcı sekme-sınırlandırılmış anahtar/değer çiftleri okur, verileri işler ve sonra stdout üzerinde sekme-delimited anahtar/değer çiftleri olarak sonuç yayır.
5. Çıktı Hadoop tarafından okunur ve çıktı dizine yazılır.

Akış hakkında daha fazla bilgi için [Hadoop Streaming'e](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Visual Studio.

* .NET Framework 4.5'i hedefleyen C# kodu yazma ve oluşturma aşinalığı.

* .exe dosyalarını kümeye yüklemenin bir yolu. Bu belgedeki adımlar, dosyaları kümenin birincil depolama alanına yüklemek için Visual Studio için Veri Gölü Araçlarını kullanır.

* PowerShell kullanıyorsanız, [Az Modülü](https://docs.microsoft.com/powershell/azure/overview)gerekir.

* HDInsight'ta bir Apache Hadoop kümesi. [Linux'ta HDInsight ile başlayın](../hadoop/apache-hadoop-linux-tutorial-get-started.md)bakın.

* Kümeleriniz birincil depolama için [URI düzeni.](../hdinsight-hadoop-linux-information.md#URI-and-scheme) Bu şema `wasb://` Azure Depolama, `abfs://` Azure Veri Gölü `adl://` Depolama Gen2 veya Azure Veri Gölü Depolama Gen1 için olacaktır. Azure Depolama veya Veri Gölü Depolama Gen2 için güvenli `wasbs://` aktarım etkinleştirilirse, URI sırasıyla güvenli `abfss://` [aktarım](../../storage/common/storage-require-secure-transfer.md)olur.

## <a name="create-the-mapper"></a>Haritayı oluşturma

Visual Studio'da *mapper*adında yeni bir .NET Framework konsolu uygulaması oluşturun. Uygulama için aşağıdaki kodu kullanın:

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

Uygulamayı oluşturduktan sonra, proje dizinindeki */bin/Debug/mapper.exe* dosyasını oluşturmak için oluşturun.

## <a name="create-the-reducer"></a>Redüktörü oluşturun

Visual Studio'da, *redüktör*adlı yeni bir .NET Framework konsol uytucu uygulaması oluşturun. Uygulama için aşağıdaki kodu kullanın:

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

Uygulamayı oluşturduktan sonra, proje dizinindeki */bin/Debug/reducer.exe* dosyasını oluşturmak için oluşturun.

## <a name="upload-to-storage"></a>Depolama alanına yükleme

Ardından, *mapper* ve *redüktör* uygulamalarını HDInsight depolama alanına yüklemeniz gerekir.

1. Visual Studio'da**Sunucu Gezgini'ni** **Görüntüle'yi** > seçin.

1. Azure'a **Azure**sağ tıklayın, **Microsoft Azure Aboneliğine Bağlan'ı seçin...** ve oturum açma işlemini tamamlayın.

1. Bu uygulamayı dağıtmak istediğiniz HDInsight kümesini genişletin. Metnin **(Varsayılan Depolama Hesabı)** içeren bir giriş listelenir.

   ![Depolama hesabı, HDInsight kümesi, Server Explorer, Visual Studio](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/hdinsight-storage-account.png)

   * **(Varsayılan Depolama Hesabı)** girişi genişletilebilirse, küme için varsayılan depolama alanı olarak bir **Azure Depolama Hesabı** kullanıyorsunuz. Kümeiçin varsayılan depolama daki dosyaları görüntülemek için girişi genişletin ve ardından çift tıklatın **(Varsayılan Kapsayıcı)**.

   * **(Varsayılan Depolama Hesabı)** girişi genişletilenemezse, küme için varsayılan depolama alanı olarak **Azure Veri Gölü Depolama'yı** kullanıyorsunuz. Kümenin varsayılan depolama alanındaki dosyaları görüntülemek için **(Varsayılan Depolama Hesabı)** girişini çift tıklatın.

1. .exe dosyalarını yüklemek için aşağıdaki yöntemlerden birini kullanın:

    * **Azure Depolama Hesabı**kullanıyorsanız, **Blob Yükle** simgesini seçin.

        ![Mapper için HDInsight yükleme simgesi, Visual Studio](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/hdinsight-upload-icon.png)

        Yeni **Dosya Yükle** iletişim kutusunda, **Dosya adı**altında **Gözat'ı**seçin. **Blob Yükle** iletişim kutusunda, *mapper* projesi için *bin\debug* klasörüne gidin ve ardından *mapper.exe* dosyasını seçin. Son olarak, yüklemeyi tamamlamak için **Aç'ı** ve ardından **Tamam'ı** seçin.

    * **Azure Veri Gölü Depolaması**için, dosya listesindeboş bir alana sağ tıklayın ve ardından **Yükle'yi**seçin. Son olarak, *mapper.exe* dosyasını seçin ve sonra **Aç'ı**seçin.

    *mapper.exe* yükleme sýnýrý tamamladiktan sonra *reducer.exe* dosyasý için yükleme işlemini tekrarlayın.

## <a name="run-a-job-using-an-ssh-session"></a>İş çalıştırma: SSH oturumu kullanma

Aşağıdaki yordam, Bir SSH oturumu kullanarak MapReduce iş çalıştırmak için nasıl açıklanır:

1. Kümenize bağlanmak için [ssh komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME'yi kümenizin adıyla değiştirerek aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. MapReduce işini başlatmak için aşağıdaki komutlardan birini kullanın:

   * Varsayılan depolama alanı **Azure Depolama**ise:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar \
            -files wasbs:///mapper.exe,wasbs:///reducer.exe \
            -mapper mapper.exe \
            -reducer reducer.exe \
            -input /example/data/gutenberg/davinci.txt \
            -output /example/wordcountout
        ```

    * Varsayılan depolama **Veri Gölü Depolama Gen1**ise:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar \
            -files adl:///mapper.exe,adl:///reducer.exe \
            -mapper mapper.exe \
            -reducer reducer.exe \
            -input /example/data/gutenberg/davinci.txt \
            -output /example/wordcountout
        ```

   * Varsayılan depolama **Veri Gölü Depolama Gen2**ise:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar \
            -files abfs:///mapper.exe,abfs:///reducer.exe \
            -mapper mapper.exe \
            -reducer reducer.exe \
            -input /example/data/gutenberg/davinci.txt \
            -output /example/wordcountout
        ```

   Aşağıdaki liste, her parametre ve seçeneğin neyi temsil ettiğiaçıklanmaktadır:

   |Parametre | Açıklama |
   |---|---|
   |hadoop-streaming.jar|Akış Lı MapReduce işlevini içeren kavanoz dosyasını belirtir.|
   |-dosyalar|Bu iş için *mapper.exe* ve *reducer.exe* dosyalarını belirtir. Her `wasbs:///` `adl:///`dosyadan `abfs:///` önce , veya protokol bildirimi küme için varsayılan depolama köküne giden yoldur.|
   |-mapper|Haritayı uygulayan dosyayı belirtir.|
   |-redüktör|Azaltıcıyı uygulayan dosyayı belirtir.|
   |-giriş|Giriş verilerini belirtir.|
   |-çıkış|Çıktı dizini belirtir.|

1. MapReduce işi tamamlandıktan sonra, sonuçları görüntülemek için aşağıdaki komutu kullanın:

   ```bash
   hdfs dfs -text /example/wordcountout/part-00000
   ```

   Aşağıdaki metin, bu komut tarafından döndürülen verilere bir örnektir:

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

## <a name="run-a-job-using-powershell"></a>Bir iş çalıştırın: PowerShell kullanma

MapReduce işini çalıştırmak ve sonuçları indirmek için aşağıdaki PowerShell komut dosyasını kullanın.

[!code-powershell[main](../../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]

Bu komut dosyası, HDInsight küme adı ile birlikte küme giriş hesabı adı ve parolasını ister. İş tamamlandıktan sonra çıktı *output.txt*adlı bir dosyaya indirilir. Aşağıdaki metin `output.txt` dosyadaki verilere bir örnektir:

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

HDInsight ile MapReduce'ı kullanma hakkında daha fazla bilgi için, [HDInsight'ta Apache Hadoop'ta MapReduce'ı kullanın'](hdinsight-use-mapreduce.md)a bakın.

Hive ve Pig ile C# kullanma hakkında daha fazla bilgi için [bkz.](apache-hadoop-hive-pig-udf-dotnet-csharp.md)

HDInsight'ta Storm ile C# kullanma hakkında daha fazla bilgi için, [HDInsight'ta Apache Storm için C# topolojileri geliştir'e](../storm/apache-storm-develop-csharp-visual-studio-topology.md)bakın.
