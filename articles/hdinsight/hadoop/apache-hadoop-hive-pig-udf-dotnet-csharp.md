---
title: C#, Apache Hive & Apache Pig üzerinde Apache Hadoop - Azure HDInsight
description: Azure HDInsight'ta Apache Hive ve Apache Pig akışıyla C# kullanıcı tanımlı işlevleri (UDF) nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/06/2019
ms.openlocfilehash: 9ef9eada9b9aec50642a8bf357edab0677868817
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74949398"
---
# <a name="use-c-user-defined-functions-with-apache-hive-and-apache-pig-on-apache-hadoop-in-hdinsight"></a>HDInsight'ta Apache Hadoop'ta Apache Hive ve Apache Pig ile C# kullanıcı tanımlı işlevleri ni kullanma

HDInsight'ta [Apache Hive](https://hive.apache.org) ve [Apache Pig](https://pig.apache.org) ile C# kullanıcı tanımlı işlevleri (UDF) nasıl kullanacağınızı öğrenin.

> [!IMPORTANT]
> Bu belgedeki adımlar Linux tabanlı HDInsight kümeleriyle çalışır. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için [HDInsight bileşen sürümüne](../hdinsight-component-versioning.md)bakın.

Hem Hive hem de Pig verileri işleme için harici uygulamalara aktarabilir. Bu işlem _akış_olarak bilinir. Bir .NET uygulaması kullanılırken, veriler STDIN'deki uygulamaya aktarılır ve uygulama STDOUT'taki sonuçları döndürür. STDIN ve STDOUT'tan okuma ve `Console.ReadLine()` yazma `Console.WriteLine()` için bir konsol uygulamasını kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* .NET Framework 4.5'i hedefleyen C# kodu yazma ve oluşturma aşinalığı.

    İstediğini kullan. Visual [Studio](https://www.visualstudio.com/vs) veya [Visual Studio Code'u](https://code.visualstudio.com/)öneriyoruz. Bu belgedeki adımlar Visual Studio 2019'u kullanır.

* .exe dosyalarını kümeye yüklemenin ve Pig and Hive işlerini çalıştırmanın bir yolu. Visual [Studio,](../../data-lake-analytics/data-lake-analytics-data-lake-tools-install.md) [Azure PowerShell](/powershell/azure)ve Azure [CLI](/cli/azure/install-azure-cli?view=azure-cli-latest)için Veri Gölü Araçları öneririz. Bu belgedeki adımlar, dosyaları yüklemek ve örnek Hive sorgusunu çalıştırmak için Visual Studio için Veri Gölü Araçlarını kullanır.

    Hive sorgularını çalıştırmanın diğer yolları hakkında bilgi için Azure [HDInsight'ta Apache Hive ve HiveQL nedir?](hdinsight-use-hive.md)

* HDInsight kümesi nde bir Hadoop. Küme oluşturma hakkında daha fazla bilgi için [bkz.](../hdinsight-hadoop-provision-linux-clusters.md)

## <a name="net-on-hdinsight"></a>.NET HDInsight üzerinde

*Linux tabanlı HDInsight* kümeleri [Monohttps://mono-project.com) 'yu kullanarak](https://mono-project.com) .NET uygulamalarını çalıştırAr. Mono sürüm 4.2.1 HDInsight sürüm 3.6 ile birlikte verilir.

.NET Framework sürümleriyle Mono uyumluluğu hakkında daha fazla bilgi [için](https://www.mono-project.com/docs/about-mono/compatibility/)bkz.

HDInsight sürümleriyle birlikte verilen .NET Framework ve Mono sürümü hakkında daha fazla bilgi için [HDInsight bileşen sürümlerine](../hdinsight-component-versioning.md)bakın.

## <a name="create-the-c-projects"></a>C\# projelerini oluşturma

Aşağıdaki bölümlerde Visual Studio'da Apache Hive UDF ve Apache Pig UDF için c# projesi nin nasıl oluşturulacağını açıklanmıştır.

### <a name="apache-hive-udf"></a>Apache Hive UDF

Apache Hive UDF için C# projesi oluşturmak için:

1. Visual Studio’yu başlatın.

2. **Yeni bir proje oluştur'u**seçin.

3. Yeni **proje oluşturma** penceresinde Konsol **Uygulaması (.NET Framework)** şablonu (C# sürümü) seçeneğini belirleyin. Ardından **İleri'yi**seçin.

4. Yeni **proje pencerenizi Yapılandır' da** *HiveCSharp'ın*bir **Proje adını** girin ve yeni projeyi kaydetmek için Bir **Konuma** gidin veya oluşturun. Ardından **Oluştur**’u seçin.

5. Visual Studio IDE'de *Program.cs* içeriğini aşağıdaki kodla değiştirin:

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

6. Menü çubuğundan, projeyi oluşturmak için **Yapı** > **Çözümü'nü** seçin.

7. Çözümü kapatın.

### <a name="apache-pig-udf"></a>Apaçi Domuz UDF

Apache Hive UDF için C# projesi oluşturmak için:

1. Visual Studio'yu açın.

2. **Başlat** penceresinde yeni **bir proje oluştur'u**seçin.

3. Yeni **proje oluşturma** penceresinde Konsol **Uygulaması (.NET Framework)** şablonu (C# sürümü) seçeneğini belirleyin. Ardından **İleri'yi**seçin.

4. Yeni **proje pencerenizi Yapılandır' da,** *PigUDF'un*bir **Proje adını** girin ve yeni projeyi kaydetmek için bir **Konum'a** gidin veya oluşturun. Ardından **Oluştur**’u seçin.

5. Visual Studio IDE'de *Program.cs* içeriğini aşağıdaki kodla değiştirin:

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

    Bu kod, Pig'den gönderilen satırları ayrıştırır `java.lang.Exception`ve ile başlayan yeniden biçimdeki satırları düzeltin.

6. Menü çubuğundan, projeyi oluşturmak için **Yapı** > **Çözüm'üne** bakın.

7. Çözümü açık bırakın.

## <a name="upload-to-storage"></a>Depolama alanına yükleme

Ardından, Hive ve Pig UDF uygulamalarını hdinsight kümesindeki depolama alanına yükleyin.

1. Visual Studio'da > **Sunucu**Gezgini'ni **Görüntüle'ye**gidin.

1. **Server Explorer'dan** **Azure'a**sağ tıklayın, Microsoft **Azure Aboneliğine Bağlan'ı**seçin ve oturum açma işlemini tamamlayın.

1. Bu uygulamayı dağıtmak istediğiniz HDInsight kümesini genişletin. Metnin **(Varsayılan Depolama Hesabı)** içeren bir giriş listelenir.

    ![Varsayılan depolama hesabı, HDInsight kümesi, Server Explorer](./media/apache-hadoop-hive-pig-udf-dotnet-csharp/hdinsight-storage-account.png)

    * Bu giriş genişletilebilirse, küme için varsayılan depolama alanı olarak bir **Azure Depolama Hesabı** kullanıyorsunuz. Kümeiçin varsayılan depolama daki dosyaları görüntülemek için girişi genişletin ve **ardından (Varsayılan Kapsayıcı) çift**tıklatın.

    * Bu giriş genişletilenemezse, küme için varsayılan depolama alanı olarak **Azure Veri Gölü Depolama'yı** kullanıyorsunuz. Kümenin varsayılan depolama alanındaki dosyaları görüntülemek için **(Varsayılan Depolama Hesabı)** girişini çift tıklatın.

1. .exe dosyalarını yüklemek için aşağıdaki yöntemlerden birini kullanın:

    * **Azure Depolama Hesabı**kullanıyorsanız, **Blob Yükle** simgesini seçin.

        ![Yeni proje için HDInsight yükleme simgesi](./media/apache-hadoop-hive-pig-udf-dotnet-csharp/hdinsight-upload-icon.png)

        Yeni **Dosya Yükle** iletişim kutusunda, **Dosya adı**altında **Gözat'ı**seçin. Upload **Blob** iletişim kutusunda, *HiveCSharp* projesi için *bin\debug* klasörüne gidin ve ardından *HiveCSharp.exe* dosyasını seçin. Son olarak, yüklemeyi tamamlamak için **Aç'ı** ve ardından **Tamam'ı** seçin.

    * **Azure Veri Gölü Depolama'yı**kullanıyorsanız, dosya listesindeboş bir alana sağ tıklayın ve ardından **Yükle'yi**seçin. Son olarak, *HiveCSharp.exe* dosyasını seçin ve **Aç'ı**seçin.

    *HiveCSharp.exe* yükleme sýnýrladýktan sonra *PigUDF.exe* dosyasý için yükleme işlemini tekrarlayın.

## <a name="run-an-apache-hive-query"></a>Apache Hive sorgusu nu çalıştırma

Artık Hive UDF uygulamanızı kullanan bir Hive sorgusu çalıştırabilirsiniz.

1. Visual Studio'da > **Sunucu**Gezgini'ni **Görüntüle'ye**gidin.

2. **Azure** seçeneğini ve sonra **HDInsight** seçeneğini genişletin.

3. *HiveCSharp* uygulamasını dağıttığınız kümeye sağ tıklayın ve ardından **Kovan Sorgusu Yaz'ı**seçin.

4. Kovan sorgusu için aşağıdaki metni kullanın:

    ```hiveql
    -- Uncomment the following if you are using Azure Storage
    -- add file wasbs:///HiveCSharp.exe;
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
    > Kümeniz `add file` için kullanılan varsayılan depolama alanı türüyle eşleşen ifadeyi açıklamayı bırakın.

    Bu `clientid`sorgu , `devicemake`, `devicemodel` ve `hivesampletable`alanları seçer ve sonra *hivecsharp.exe* uygulamasına alanları geçer. Sorgu, uygulamanın , , `clientid` `phoneLabel`ve `phoneHash`. olarak depolanan üç alanı döndürmesini bekler. Sorgu da varsayılan depolama kapsayıcıkökünde *HiveCSharp.exe* bulmak için bekliyor.

5. Varsayılan **Etkileşimli'yi** **Toplu İşlem'e**geçirin ve sonra işi HDInsight kümesine göndermek için **Gönder'i** seçin. **Kovan İş Özeti** penceresi açılır.

6. İş Durumu **Tamamlanana**göre değişene kadar özeti yenilemek için **Yenile'yi** seçin. **Job Status** İş çıktısını görüntülemek için **İş Çıktısı'nı**seçin.

## <a name="run-an-apache-pig-job"></a>Bir Apache Pig iş çalıştırın

Ayrıca Pig UDF uygulamanızı kullanan bir Pig işini de çalıştırabilirsiniz.

1. HDInsight kümenize bağlanmak için SSH'yi kullanın. (Örneğin, komutu `ssh sshuser@<clustername>-ssh.azurehdinsight.net`çalıştırın.) Daha fazla bilgi için Bkz. [SSH withHDInsight'ı kullanın.](../hdinsight-hadoop-linux-use-ssh-unix.md)

2. Pig komut satırını başlatmak için aşağıdaki komutu kullanın:

    ```shell
    pig
    ```

    Bir `grunt>` istem görüntülenir.

3. .NET Framework uygulamasını kullanan bir Pig işini çalıştırmak için aşağıdakileri girin:

    ```pig
    DEFINE streamer `PigUDF.exe` CACHE('/PigUDF.exe');
    LOGS = LOAD '/example/data/sample.log' as (LINE:chararray);
    LOG = FILTER LOGS by LINE is not null;
    DETAILS = STREAM LOG through streamer as (col1, col2, col3, col4, col5);
    DUMP DETAILS;
    ```

    `DEFINE` Deyim, *PigUDF.exe* uygulaması `CACHE` `streamer` için bir takma ad oluşturur ve küme için varsayılan depolamadan yükler. Daha `streamer` sonra, içinde `STREAM` bulunan tek satırları işlemek `LOG` ve verileri bir dizi sütun olarak döndürmek için işleçle birlikte kullanılır.

    > [!NOTE]
    > Akış için kullanılan uygulama adı, diğer \` ad verildiğinde (backtick) karakteri yle ve ' (tek teklif) `SHIP`karakteriyle birlikte kullanıldığında, ' (tek tırnak) karakteriyle çevrilmelidir.

4. Son satıra girdikten sonra, iş başlamalıdır. Aşağıdaki metne benzer çıktı döndürür:

    ```output
    (2019-07-15 16:43:25 SampleClass5 [WARN] problem finding id 1358451042 - java.lang.Exception)
    (2019-07-15 16:43:25 SampleClass5 [DEBUG] detail for id 1976092771)
    (2019-07-15 16:43:25 SampleClass5 [TRACE] verbose detail for id 1317358561)
    (2019-07-15 16:43:25 SampleClass5 [TRACE] verbose detail for id 1737534798)
    (2019-07-15 16:43:25 SampleClass7 [DEBUG] detail for id 1475865947)
    ```

5. Domuzçıkmak için kullanın. `exit`

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, HDInsight'ta Hive ve Pig'den bir .NET Framework uygulamasını nasıl kullanacağınızı öğrendiniz. Python'u Hive ve Pig ile nasıl kullanacağınızı öğrenmek istiyorsanız, [HDInsight'ta Apache Hive ve Apache Pig ile Python'u kullanın'](python-udf-hdinsight.md)a bakın.

Hive'ı kullanmanın diğer yolları ve MapReduce'ı kullanma hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [HDInsight ile Apache Hive'ı kullanma](hdinsight-use-hive.md)
* [HDInsight ile MapReduce'ı kullanın](hdinsight-use-mapreduce.md)
* [Domuz Latin Temelleri](https://pig.apache.org/docs/latest/basic.html)