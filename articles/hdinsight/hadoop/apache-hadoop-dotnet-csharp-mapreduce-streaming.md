---
title: HDInsight C# 'ta Hadoop 'Ta MapReduce ile birlikte kullanma-Azure
description: Azure HDInsight 'ta Apache Hadoop C# MapReduce çözümleri oluşturmak için kullanmayı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/15/2019
ms.author: hrasheed
ms.openlocfilehash: 5784fb4f4ab0f46d2db7e5e8cfe9deeafabb4e90
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71066951"
---
# <a name="use-c-with-mapreduce-streaming-on-apache-hadoop-in-hdinsight"></a>HDInsight C# 'Ta Apache Hadoop MapReduce streaming ile kullanma

HDInsight 'ta MapReduce çözümü oluşturmak için kullanmayı C# öğrenin.

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [HDInsight bileşen sürümü oluşturma](../hdinsight-component-versioning.md).

Apache Hadoop akışı, MapReduce işlerini bir betik veya yürütülebilir dosya kullanarak çalıştırmanıza olanak tanıyan bir yardımcı programdır. Bu örnekte, bir sözcük sayısı çözümü için Mapper ve Reducer uygulamak üzere .NET kullanılır.

## <a name="net-on-hdinsight"></a>HDInsight üzerinde .NET

__Linux tabanlı HDInsight__ kümeleri mono kullanır [(https://mono-project.com) ](https://mono-project.com) .NET uygulamalarını çalıştırmak için). Tek bir sürüm 4.2.1, HDInsight sürüm 3,6 ' ye dahildir. HDInsight 'ta bulunan mono sürümü hakkında daha fazla bilgi için bkz. [HDInsight bileşen sürümleri](../hdinsight-component-versioning.md). 

.NET Framework sürümleriyle mono uyumluluğu hakkında daha fazla bilgi için bkz. [mono uyumluluk](https://www.mono-project.com/docs/about-mono/compatibility/).

## <a name="how-hadoop-streaming-works"></a>Hadoop akışı nasıl kullanılır?

Bu belgede akış için kullanılan temel işlem aşağıdaki gibidir:

1. Hadoop, STDIN 'de verileri eşleştiriciyle (Bu örnekteki Mapper. exe) geçirir.
2. Eşleyici, verileri işler ve STDOUT için sekmeyle ayrılmış anahtar/değer çiftlerini yayar.
3. Çıktı, Hadoop tarafından okunarak (Bu örnekte Reducer. exe) Reducer 'e geçirilir.
4. Reducer, sekmeyle ayrılmış anahtar/değer çiftlerini okur, verileri işler ve sonra sonucu STDOUT üzerinde sekmeyle ayrılmış anahtar/değer çiftleri olarak yayar.
5. Çıktı, Hadoop tarafından okunmakta ve çıkış dizinine yazılır.

Akış hakkında daha fazla bilgi için bkz. [Hadoop akışı](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).

## <a name="prerequisites"></a>Önkoşullar

* .NET Framework 4,5 ' i hedefleyen kod C# yazma ve oluşturma hakkında bir benzerlik. Bu belgedeki adımlarda Visual Studio 2017 kullanılır.

* . Exe dosyalarını kümeye yüklemek için bir yol. Bu belgedeki adımlarda, dosyaları küme için birincil depolamaya yüklemek üzere Visual Studio için Data Lake araçları kullanılır.

* Azure PowerShell veya SSH istemcisi.

* HDInsight kümesi üzerinde Hadoop. Küme oluşturma hakkında daha fazla bilgi için bkz. [HDInsight kümesi oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="create-the-mapper"></a>Eşleyici oluşturma

Visual Studio 'da __Eşleyici__adlı yeni bir __konsol uygulaması__ oluşturun. Uygulama için aşağıdaki kodu kullanın:

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

Uygulamayı oluşturduktan sonra, `/bin/Debug/mapper.exe` dosyayı proje dizininde oluşturmak için derleyin.

## <a name="create-the-reducer"></a>Reducer oluşturma

Visual Studio 'da, __Reducer__adlı yeni bir __konsol uygulaması__ oluşturun. Uygulama için aşağıdaki kodu kullanın:

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

Uygulamayı oluşturduktan sonra, `/bin/Debug/reducer.exe` dosyayı proje dizininde oluşturmak için derleyin.

## <a name="upload-to-storage"></a>Depolama alanına yükleme

1. Visual Studio 'da **Sunucu Gezgini**açın.

2. **Azure** seçeneğini ve sonra **HDInsight** seçeneğini genişletin.

3. İstenirse, Azure aboneliği kimlik bilgilerinizi girin ve **oturum aç**' a tıklayın.

4. Bu uygulamayı dağıtmak istediğiniz HDInsight kümesini genişletin. Metin __(varsayılan depolama hesabı)__ içeren bir giriş listelenir.

    ![Kümenin depolama hesabını gösteren Sunucu Gezgini](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/hdinsight-storage-account.png)

    * Bu giriş genişletilirse, küme için varsayılan depolama alanı olarak bir __Azure depolama hesabı__ kullanıyorsunuz demektir. Küme için varsayılan depolamada bulunan dosyaları görüntülemek için, girdiyi genişletin ve __(varsayılan kapsayıcı)__ öğesine çift tıklayın.

    * Bu girdi genişletilemez, küme için varsayılan depolama alanı olarak __Azure Data Lake Storage__ kullanırsınız. Küme için varsayılan depolamada bulunan dosyaları görüntülemek için __(varsayılan depolama hesabı)__ girişine çift tıklayın.

5. . Exe dosyalarını karşıya yüklemek için aşağıdaki yöntemlerden birini kullanın:

   * Bir __Azure depolama hesabı__kullanıyorsanız karşıya yükle simgesine tıklayın ve ardından **Eşleyici** projesi için **bin\Debug** klasörüne gidin. Son olarak, **Mapper. exe** dosyasını seçin ve **Tamam**' a tıklayın.

        ![Eşleyici için HDInsight karşıya yükleme simgesi](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/hdinsight-upload-icon.png)
    
   * __Azure Data Lake Storage__kullanıyorsanız, dosya listesinde boş bir alana sağ tıklayın ve ardından __karşıya yükle__' yi seçin. Son olarak, **Mapper. exe** dosyasını seçin ve **Aç**' a tıklayın.

     __Eşleyici. exe__ karşıya yükleme işlemi tamamlandıktan sonra, __Reducer. exe__ dosyası için karşıya yükleme işlemini tekrarlayın.

## <a name="run-a-job-using-an-ssh-session"></a>Bir iş çalıştırın: SSH oturumu kullanma

1. HDInsight kümesine bağlanmak için SSH kullanın. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. MapReduce işini başlatmak için aşağıdaki komutlardan birini kullanın:

   * __Data Lake Storage 2.__ varsayılan depolama alanı olarak kullanıyorsanız:

       ```bash
       yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files abfs:///mapper.exe,abfs:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
       ```

   * __Data Lake Storage 1.__ varsayılan depolama alanı olarak kullanıyorsanız:

       ```bash
       yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files adl:///mapper.exe,adl:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
       ```
    
   * __Azure Storage__ 'ı varsayılan depolama alanı olarak kullanıyorsanız:

       ```bash
       yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files wasb:///mapper.exe,wasb:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
       ```

     Aşağıdaki listede her parametrenin ne yaptığını açıklanmaktadır:

   * `hadoop-streaming.jar`: Akış MapReduce işlevini içeren jar dosyası.
   * `-files`: Bu işe `reducer.exe`vedosyalarınıekler. `mapper.exe` Her bir dosya, küme için varsayılan depolama kökünün yoludur. `abfs:///``adl:///` `wasb:///`
   * `-mapper`: Eşleyici 'yi uygulayan dosyayı belirtir.
   * `-reducer`: Hangi dosyanın Reducer uyguladığını belirtir.
   * `-input`: Giriş verileri.
   * `-output`: Çıkış dizini.

3. MapReduce işi tamamlandığında, sonuçları görüntülemek için aşağıdakileri kullanın:

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    Aşağıdaki metin, bu komutun döndürdüğü verilerin bir örneğidir:

        you     1128
        young   38
        younger 1
        youngest        1
        your    338
        yours   4
        yourself        34
        yourselves      3
        youth   17

## <a name="run-a-job-using-powershell"></a>Bir iş çalıştırın: PowerShell’i kullanma

Bir MapReduce işini çalıştırmak ve sonuçları indirmek için aşağıdaki PowerShell betiğini kullanın.

[!code-powershell[main](../../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]

Bu betik, HDInsight küme adı ile birlikte küme oturum açma hesabı adı ve parolasını ister. İş tamamlandıktan sonra, çıktı adlı `output.txt`bir dosyaya indirilir. Aşağıdaki metin `output.txt` dosyadaki verilerin bir örneğidir:

    you     1128
    young   38
    younger 1
    youngest        1
    your    338
    yours   4
    yourself        34
    yourselves      3
    youth   17

## <a name="next-steps"></a>Sonraki adımlar

HDInsight ile MapReduce kullanma hakkında daha fazla bilgi için bkz. [HDInsight Ile MapReduce kullanma](hdinsight-use-mapreduce.md).

Hive ve Pig ile C# kullanma hakkında bilgi için bkz. [Apache Hive ve C# Apache Pig Ile Kullanıcı tanımlı bir işlev kullanma](apache-hadoop-hive-pig-udf-dotnet-csharp.md).

HDInsight 'ta fırtınası ile C# kullanma hakkında bilgi için bkz. [HDInsight C# 'Ta Apache Storm için topolojiler geliştirme](../storm/apache-storm-develop-csharp-visual-studio-topology.md).
