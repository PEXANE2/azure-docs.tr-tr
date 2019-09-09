---
title: HDInsight C# 'ta Apache Hadoop Apache Hive ve Apache Pig ile kullanma-Azure
description: Azure HDInsight 'ta Apache Hive C# ve Apache Pig akışıyla Kullanıcı tanımlı IŞLEVLERI (UDF) nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/15/2019
ms.author: hrasheed
ms.openlocfilehash: c05e72cd28c78b26b7c23a123b133d10147a4421
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70810775"
---
# <a name="use-c-user-defined-functions-with-apache-hive-and-apache-pig-on-apache-hadoop-in-hdinsight"></a>HDInsight C# 'ta Apache Hadoop üzerinde Apache Hive ve Apache Pig ile Kullanıcı tanımlı işlevler kullanma

HDInsight üzerinde Apache Hive ve C# Apache Pig ile Kullanıcı tanımlı IŞLEVLERI (UDF) nasıl kullanacağınızı öğrenin.

> [!IMPORTANT]
> Bu belgedeki adımlar hem Linux tabanlı hem de Windows tabanlı HDInsight kümeleriyle çalışır. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [HDInsight bileşen sürümü oluşturma](../hdinsight-component-versioning.md).

Hem Hive hem de Pig, işlenmek üzere dış uygulamalara veri geçirebilir. Bu işlem _akış_olarak bilinir. .NET uygulaması kullanılırken, veriler STDIN üzerindeki uygulamaya geçirilir ve uygulama STDOUT üzerinde sonuçları döndürür. STDIN ve STDOUT 'tan okumak ve yazmak için bir konsol uygulaması kullanabilirsiniz `Console.ReadLine()`. `Console.WriteLine()`

## <a name="prerequisites"></a>Önkoşullar

* .NET Framework 4,5 ' i hedefleyen kod C# yazma ve oluşturma hakkında bir benzerlik.

    * İstediğiniz IDE 'yi kullanın. [Visual Studio](https://www.visualstudio.com/vs) 2015, 2017 veya [Visual Studio Code](https://code.visualstudio.com/)önerilir. Bu belgedeki adımlarda Visual Studio 2017 kullanılır.

* . Exe dosyalarını kümeye yüklemek ve Pig ve Hive işlerini çalıştırmak için bir yol. Visual Studio, Azure PowerShell ve Azure klasik CLı için Data Lake araçları önerilir. Bu belgedeki adımlarda, dosyaları karşıya yüklemek ve örnek Hive sorgusunu çalıştırmak için Visual Studio için Data Lake araçları kullanılır.

    Hive sorguları ve Pig işleri çalıştırmanın diğer yolları hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

    * [HDInsight ile Apache Hive kullanma](hdinsight-use-hive.md)

    * [HDInsight ile Apache Pig kullanma](hdinsight-use-pig.md)

* HDInsight kümesi üzerinde Hadoop. Küme oluşturma hakkında daha fazla bilgi için bkz. [HDInsight kümesi oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="net-on-hdinsight"></a>HDInsight üzerinde .NET

* Mono kullanan __Linux tabanlı HDInsight__ kümeleri [(https://mono-project.com) ](https://mono-project.com) .NET uygulamalarını çalıştırmak için). Tek bir sürüm 4.2.1, HDInsight sürüm 3,6 ' ye dahildir.

    .NET Framework sürümleriyle mono uyumluluğu hakkında daha fazla bilgi için bkz. [mono uyumluluk](https://www.mono-project.com/docs/about-mono/compatibility/).

* __Windows tabanlı HDInsight__ kümeleri, .NET uygulamalarını ÇALıŞTıRMAK için CLR Microsoft .net kullanır.

.NET Framework sürümü ve HDInsight sürümlerine dahil olan Mono hakkında daha fazla bilgi için bkz. [HDInsight bileşen sürümleri](../hdinsight-component-versioning.md).

## <a name="create-the-c-projects"></a>C\# projelerini oluşturma

### <a name="apache-hive-udf"></a>Apache Hive UDF

1. Visual Studio 'Yu açın ve bir çözüm oluşturun. Proje türü için **konsol uygulaması (.NET Framework)** öğesini seçin ve yeni proje **hivecsharp**olarak adlandırın.

    > [!IMPORTANT]
    > Linux tabanlı HDInsight kümesi kullanıyorsanız __.NET Framework 4,5__ ' u seçin. .NET Framework sürümleriyle mono uyumluluğu hakkında daha fazla bilgi için bkz. [mono uyumluluk](https://www.mono-project.com/docs/about-mono/compatibility/).

2. **Program.cs** içeriğini aşağıdaki kodla değiştirin:

    ```csharp
    using System;
    using System.Security.Cryptography;
    using System.Text;
    using System.Threading.Tasks;

    namespace HiveCSharp
    {
        class Program
        {
            static void Main(string[] args)
            {
                string line;
                // Read stdin in a loop
                while ((line = Console.ReadLine()) != null)
                {
                    // Parse the string, trimming line feeds
                    // and splitting fields at tabs
                    line = line.TrimEnd('\n');
                    string[] field = line.Split('\t');
                    string phoneLabel = field[1] + ' ' + field[2];
                    // Emit new data to stdout, delimited by tabs
                    Console.WriteLine("{0}\t{1}\t{2}", field[0], phoneLabel, GetMD5Hash(phoneLabel));
                }
            }
            /// <summary>
            /// Returns an MD5 hash for the given string
            /// </summary>
            /// <param name="input">string value</param>
            /// <returns>an MD5 hash</returns>
            static string GetMD5Hash(string input)
            {
                // Step 1, calculate MD5 hash from input
                MD5 md5 = System.Security.Cryptography.MD5.Create();
                byte[] inputBytes = System.Text.Encoding.ASCII.GetBytes(input);
                byte[] hash = md5.ComputeHash(inputBytes);

                // Step 2, convert byte array to hex string
                StringBuilder sb = new StringBuilder();
                for (int i = 0; i < hash.Length; i++)
                {
                    sb.Append(hash[i].ToString("x2"));
                }
                return sb.ToString();
            }
        }
    }
    ```

3. Projeyi oluşturun.

### <a name="apache-pig-udf"></a>Apache Pig UDF

1. Visual Studio 'Yu açın ve bir çözüm oluşturun. Proje türü için **konsol uygulaması**' nı seçin ve yeni projeyi **pigudf**olarak adlandırın.

2. **Program.cs** dosyasının içeriğini aşağıdaki kodla değiştirin:

    ```csharp
    using System;

    namespace PigUDF
    {
        class Program
        {
            static void Main(string[] args)
            {
                string line;
                // Read stdin in a loop
                while ((line = Console.ReadLine()) != null)
                {
                    // Fix formatting on lines that begin with an exception
                    if(line.StartsWith("java.lang.Exception"))
                    {
                        // Trim the error info off the beginning and add a note to the end of the line
                        line = line.Remove(0, 21) + " - java.lang.Exception";
                    }
                    // Split the fields apart at tab characters
                    string[] field = line.Split('\t');
                    // Put fields back together for writing
                    Console.WriteLine(String.Join("\t",field));
                }
            }
        }
    }
    ```

    Bu kod, Pig adresinden gönderilen satırları ayrıştırır ve ile `java.lang.Exception`başlayan satırları yeniden biçimlendirir.

3. **Program.cs**kaydedin ve projeyi derleyin.

## <a name="upload-to-storage"></a>Depolama alanına yükleme

1. Visual Studio 'da **Sunucu Gezgini**açın.

2. **Azure** seçeneğini ve sonra **HDInsight** seçeneğini genişletin.

3. İstenirse, Azure aboneliği kimlik bilgilerinizi girin ve **oturum aç**' a tıklayın.

4. Bu uygulamayı dağıtmak istediğiniz HDInsight kümesini genişletin. Metin __(varsayılan depolama hesabı)__ içeren bir giriş listelenir.

    ![Kümenin depolama hesabını gösteren Sunucu Gezgini](./media/apache-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * Bu giriş genişletilirse, küme için varsayılan depolama alanı olarak bir __Azure depolama hesabı__ kullanıyorsunuz demektir. Küme için varsayılan depolamada bulunan dosyaları görüntülemek için, girdiyi genişletin ve __(varsayılan kapsayıcı)__ öğesine çift tıklayın.

    * Bu girdi genişletilemez, küme için varsayılan depolama alanı olarak __Azure Data Lake Storage__ kullanırsınız. Küme için varsayılan depolamada bulunan dosyaları görüntülemek için __(varsayılan depolama hesabı)__ girişine çift tıklayın.

6. . Exe dosyalarını karşıya yüklemek için aşağıdaki yöntemlerden birini kullanın:

   * Bir __Azure depolama hesabı__kullanıyorsanız karşıya yükle simgesine tıklayın ve ardından **hivecsharp** proje için **bin\Debug** klasörüne gidin. Son olarak, **Hivecsharp. exe** dosyasını seçip **Tamam**' a tıklayın.

       ![karşıya yükle simgesi](./media/apache-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
   * __Azure Data Lake Storage__kullanıyorsanız, dosya listesinde boş bir alana sağ tıklayın ve ardından __karşıya yükle__' yi seçin. Son olarak, **Hivecsharp. exe** dosyasını seçin ve **Aç**' a tıklayın.

     __Hivecsharp. exe__ karşıya yüklemesi tamamlandıktan sonra, __pigudf. exe__ dosyası için karşıya yükleme işlemini tekrarlayın.

## <a name="run-an-apache-hive-query"></a>Bir Apache Hive sorgusu çalıştırma

1. Visual Studio 'da **Sunucu Gezgini**açın.

2. **Azure** seçeneğini ve sonra **HDInsight** seçeneğini genişletin.

3. **Hivecsharp** uygulamasını dağıttığınız kümeye sağ tıklayın ve ardından **Hive sorgusu yaz**' ı seçin.

4. Hive sorgusu için aşağıdaki metni kullanın:

    ```hiveql
    -- Uncomment the following if you are using Azure Storage
    -- add file wasb:///HiveCSharp.exe;
    -- Uncomment the following if you are using Azure Data Lake Storage Gen1
    -- add file adl:///HiveCSharp.exe;
    -- Uncomment the following if you are using Azure Data Lake Storage Gen2
    -- add file abfs:///HiveCSharp.exe;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'HiveCSharp.exe' AS
    (clientid string, phoneLabel string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;
    ```

    > [!IMPORTANT]
    > Kümeniz için kullanılan varsayılan depolama türüyle eşleşen deyiminaçıklamasınıkaldırın.`add file`

    Bu sorgu,, `clientid`ve `devicemake` `devicemodel` alanlarını`hivesampletable`seçer ve alanları hivecsharp. exe uygulamasına geçirir. Sorgu, `clientid` `phoneLabel`uygulamanın, ve `phoneHash`olarak depolanan üç alanı döndürmesini bekler. Sorgu ayrıca varsayılan depolama kapsayıcısının kökünde HiveCSharp. exe ' yi bulmayı bekler.

5. İşi HDInsight kümesine göndermek için **Gönder** ' e tıklayın. **Hive Iş Özeti** penceresi açılır.

6. **Iş durumu** **tamamlandı**olarak değişene kadar Özeti yenilemek için **Yenile** ' ye tıklayın. İş çıktısını görüntülemek için **Iş çıktısı**' na tıklayın.

## <a name="run-an-apache-pig-job"></a>Apache Pig işini çalıştırma

1. HDInsight kümenize bağlanmak için SSH kullanın. Örneğin: `ssh sshuser@mycluster-ssh.azurehdinsight.net`. Daha fazla bilgi için bkz. [SSH Withhdınsight kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md)

2. Pig komut satırını başlatmak için aşağıdaki komutu kullanın:

        pig

    > [!IMPORTANT]
    > Windows tabanlı bir küme kullanıyorsanız, bunun yerine aşağıdaki komutları kullanın:
    > ```
    > cd %PIG_HOME%
    > bin\pig
    > ```

    Bir `grunt>` istem görüntülenir.

3. .NET Framework uygulamasını kullanan bir Pig işini çalıştırmak için aşağıdakileri girin:

        DEFINE streamer `PigUDF.exe` CACHE('/PigUDF.exe');
        LOGS = LOAD '/example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = STREAM LOG through streamer as (col1, col2, col3, col4, col5);
        DUMP DETAILS;

    İfade, pigudf. `streamer` exe uygulamaları için bir diğer ad oluşturur ve `CACHE` küme için varsayılan depolama alanından yükler. `DEFINE` Daha sonra `streamer` , günlükte bulunan tek `STREAM` satırları işlemek ve verileri bir dizi sütun olarak döndürmek için işleciyle birlikte kullanılır.

    > [!NOTE]
    > Akış \` için kullanılan uygulama adı, diğer ad olduğunda (backtick) karakteri ile birlikte `SHIP`kullanıldığında ' (tek tırnak) arasına alınmalıdır.

4. Son satırı girdikten sonra iş başlamalıdır. Aşağıdaki metne benzer bir çıktı döndürür:

        (2012-02-03 20:11:56 SampleClass5 [WARN] problem finding id 1358451042 - java.lang.Exception)
        (2012-02-03 20:11:56 SampleClass5 [DEBUG] detail for id 1976092771)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1317358561)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1737534798)
        (2012-02-03 20:11:56 SampleClass7 [DEBUG] detail for id 1475865947)

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede Hive ve Pig ' den HDInsight üzerinde bir .NET Framework uygulamasının nasıl kullanılacağını öğrendiniz. Hive ve Pig ile Python kullanmayı öğrenmek isterseniz bkz. [HDInsight 'ta Apache Hive ve Apache Pig Ile Python kullanma](python-udf-hdinsight.md).

Pig ve Hive kullanmanın diğer yolları için ve MapReduce kullanma hakkında bilgi edinmek için aşağıdaki belgelere bakın:

* [HDInsight ile Apache Hive kullanma](hdinsight-use-hive.md)
* [HDInsight ile Apache Pig kullanma](hdinsight-use-pig.md)
* [HDInsight ile MapReduce kullanma](hdinsight-use-mapreduce.md)
