---
title: Visual Studio ve C# -Azure hdınsight ile Apache Storm topolojileri
description: "' Da C#fırtınası topolojileri oluşturmayı öğrenin. Visual Studio için Hadoop araçlarını kullanarak Visual Studio 'da basit bir sözcük sayısı topolojisi oluşturun."
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 11/27/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 828ec2b925535df3f925093466556447e703cd76
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2019
ms.locfileid: "71003808"
---
# <a name="develop-c-topologies-for-apache-storm-by-using-the-data-lake-tools-for-visual-studio"></a>Visual C# Studio için Data Lake araçları 'nı kullanarak Apache Storm için topolojiler geliştirin

Visual Studio için Azure Data Lake ( C# Apache Hadoop) araçlarını kullanarak Apache Storm topoloji oluşturmayı öğrenin. Bu belgede, Visual Studio 'da bir fırtınası projesi oluşturma, yerel olarak test etme ve Azure HDInsight kümesinde bir Apache Storm dağıtma işlemi gösterilmektedir.

Ayrıca, ve Java bileşenlerini kullanan C# karma topolojiler oluşturmayı öğreneceksiniz.

> [!NOTE]  
> Bu belgedeki adımlar Visual Studio ile bir Windows geliştirme ortamını temel alırken, derlenen proje bir Linux veya Windows tabanlı HDInsight kümesine gönderilebilir. Yalnızca 28 Ekim 2016 ' den sonra oluşturulan Linux tabanlı kümeler, SCP.NET topolojilerini destekler.

Linux tabanlı bir C# kümeyle bir topoloji kullanmak için, projeniz tarafından kullanılan MICROSOFT. SCP. net. SDK NuGet paketini 0.10.0.6 veya üzeri sürümüne güncelleştirmeniz gerekir. Paketin sürümünün ayrıca HDInsight üzerinde yüklü olan Storm ana sürümüyle eşleşmesi gerekir.

| HDInsight sürümü | Apache Storm sürümü | SCP.NET sürümü | Varsayılan mono sürümü |
|:-----------------:|:-------------:|:---------------:|:--------------------:|
| 3.3 |0.10. x |0.10.x.x</br>(yalnızca Windows tabanlı HDInsight 'ta) | NA |
| 3.4 | 0.10.0.x | 0.10.0.x | 3.2.8 |
| 3,5 | 1.0.2. x | 1.0.0. x | 4.2.1 |
| 3.6 | 1.1.0.x | 1.0.0. x | 4.2.8 |

> [!IMPORTANT]  
> Linux tabanlı kümelerdeki C# topolojilerinin .NET 4.5 kullanması ve HDInsight kümesi üzerinde çalışması için Mono kullanması gerekir. Olası uyumsuzluklar için [mono uyumluluğunu](https://www.mono-project.com/docs/about-mono/compatibility/) denetleyin.

## <a name="install-visual-studio"></a>Visual Studio'yu yükleme

Aşağıdaki Visual Studio C# sürümlerinden birini kullanarak SCP.NET ile topolojiler geliştirebilirsiniz:

* Güncelleştirme 4 ile Visual Studio 2012

* Güncelleştirme 4 veya [Visual Studio 2013 topluluğuyla](https://go.microsoft.com/fwlink/?LinkId=517284) Visual Studio 2013

* Visual Studio 2015 veya [Visual studio 2015 Community](https://go.microsoft.com/fwlink/?LinkId=532606)

* Visual Studio 2017 (herhangi bir sürüm)

## <a name="install-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake araçları 'nı yükler

Visual Studio için Data Lake araçları 'nı yüklemek için, [Visual Studio için Data Lake araçları 'nı kullanmaya başlama](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)bölümündeki adımları izleyin.

## <a name="install-java"></a>Java 'Yı yükler

Visual Studio 'dan bir fırtınası topolojisi gönderdiğinizde, SCP.NET, topolojiyi ve bağımlılıklarını içeren bir ZIP dosyası oluşturur. Java, Linux tabanlı kümeler ile daha uyumlu bir biçim kullandığından, bu ZIP dosyalarını oluşturmak için kullanılır.

1. Geliştirme ortamınıza Java geliştirici seti (JDK) 7 veya üstünü yükler. Oracle JDK 'yi [Oracle](https://aka.ms/azure-jdks)'dan edinebilirsiniz. [Diğer Java dağıtımlarını](https://openjdk.java.net/)da kullanabilirsiniz.

2. `JAVA_HOME` Ortam değişkeni, Java içeren dizine işaret etmelidir.

3. Ortam değişkeni, `%JAVA_HOME%\bin` dizini içermelidir. `PATH`

Java ve JDK 'nin C# doğru yüklendiğini doğrulamak için aşağıdaki konsol uygulamasını kullanabilirsiniz:

```csharp
using System;
using System.IO;
namespace ConsoleApplication2
{
   class Program
   {
       static void Main(string[] args)
       {
           string javaHome = Environment.GetEnvironmentVariable("JAVA_HOME");
           if (!string.IsNullOrEmpty(javaHome))
           {
               string jarExe = Path.Combine(javaHome + @"\bin", "jar.exe");
               if (File.Exists(jarExe))
               {
                   Console.WriteLine("JAVA Is Installed properly");
                    return;
               }
               else
               {
                   Console.WriteLine("A valid JAVA JDK is not found. Looks like JRE is installed instead of JDK.");
               }
           }
           else
           {
             Console.WriteLine("A valid JAVA JDK is not found. JAVA_HOME environment variable is not set.");
           }
       }  
   }
}
```

## <a name="apache-storm-templates"></a>Apache Storm şablonları

Visual Studio için Data Lake araçları aşağıdaki şablonları sağlar:

| Proje türü | Gösteriler |
| --- | --- |
| Fırtınası uygulaması |Boş bir fırtınası topolojisi projesi. |
| Fırtınası Azure SQL yazıcı örneği |Azure SQL veritabanı 'na yazma. |
| Fırtınası Azure Cosmos DB okuyucu örneği |Azure Cosmos DB nasıl okunmalıdır. |
| Fırtınası Azure Cosmos DB yazıcı örneği |Azure Cosmos DB yazma. |
| Fırtınası EventHub okuyucu örneği |Azure Event Hubs nasıl okunmalıdır. |
| Fırtınası EventHub yazıcı örneği |Azure Event Hubs yazma. |
| Fırtınası HBase okuyucu örneği |HDInsight kümelerinde HBase 'den okuma. |
| Fırtınası HBase yazıcı örneği |HDInsight kümelerinde HBase 'e yazma. |
| Fırtınası karma örneği |Java bileşeni kullanma. |
| Fırtınası örneği |Temel bir sözcük sayısı topolojisi. |

> [!WARNING]  
> Tüm şablonlar Linux tabanlı HDInsight ile çalışmaz. Şablonlar tarafından kullanılan NuGet paketleri Mono ile uyumlu olmayabilir. [Mono uyumluluk](https://www.mono-project.com/docs/about-mono/compatibility/) belgesini denetleyin ve olası sorunları belirlemek Için [.net taşınabilirlik Çözümleyicisi](../hdinsight-hadoop-migrate-dotnet-to-linux.md#automated-portability-analysis) 'ni kullanın.

Bu belgedeki adımlarda, bir topoloji oluşturmak için temel fırtınası uygulaması proje türünü kullanırsınız.

### <a name="apache-hbase-templates-notes"></a>Apache HBase şablonları notları

HBase okuyucu ve yazıcı şablonları, HDInsight kümesinde HBase ile iletişim kurmak için HBase Java API 'sini değil HBase REST API kullanır.

### <a name="eventhub-templates-notes"></a>EventHub şablonları notları

> [!IMPORTANT]  
> EventHub okuyucu şablonuna dahil edilen Java tabanlı EventHub Spout bileşeni, HDInsight sürüm 3,5 veya sonraki bir sürümü üzerinde geçiş ile çalışmayabilir. Bu bileşenin güncelleştirilmiş bir sürümü [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/HDI3.5/lib)' da bulunabilir.

Bu bileşeni kullanan ve HDInsight 3,5 üzerinde fırtınası ile birlikte çalışabilen örnek bir topoloji için bkz. [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).

## <a name="create-a-c-topology"></a>C# Topoloji oluşturma

1. Visual Studio 'yu açın, **Dosya** > **Yeni**' yi seçin ve ardından **Proje**' yi seçin.

2. **Yeni proje** penceresinde, **yüklü** > **Şablonlar**' ı genişletin ve **Azure Data Lake**' yi seçin. Şablonlar listesinden, **fırtınası uygulaması**' nı seçin. Ekranın alt kısmında, **WORDCOUNT** değerini uygulamanın adı olarak girin.

    ![Yeni proje penceresinin ekran görüntüsü](./media/apache-storm-develop-csharp-visual-studio-topology/apache-storm-new-project.png)

3. Projeyi oluşturduktan sonra, aşağıdaki dosyalara sahip olmanız gerekir:

   * **Program.cs**: Bu dosya, projeniz için topolojiyi tanımlar. Bir Spout ve bir sürgüden oluşan varsayılan topoloji varsayılan olarak oluşturulur.

   * **Spout.cs**: Rastgele sayılar sağlayan bir Spout örneği.

   * **Bolt.cs**: Spout tarafından yayılan sayı sayısını tutan örnek bir sürgüsü.

     Projeyi oluşturduğunuzda, NuGet en son [SCP.net paketini](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/)indirir.

     [!INCLUDE [scp.net version important](../../../includes/hdinsight-storm-scpdotnet-version.md)]

### <a name="implement-the-spout"></a>Spout 'yi uygulama

1. **Spout.cs**'i açın. Biriktirmalar, bir dış kaynaktan alınan verileri bir topolojide okumak için kullanılır. Spout ana bileşenleri şunlardır:

   * **Nexttuple**: Spout 'in yeni tanımlama gruplarını yaymak için izin verildiğinde, fırtınası tarafından çağırılır.

   * **ACK** (yalnızca işlem topolojisi): Spout 'dan gönderilen tanımlama grupları için topolojideki diğer bileşenler tarafından başlatılan bildirimleri işler. Bir tanımlama grubunun ele alındığı, biriktirme listesini aşağı akış bileşenleri tarafından başarıyla işlendiğini bildirir.

   * **Başarısız** (yalnızca işlem topolojisi): Başarısız olan ve topolojideki diğer bileşenleri işleyen tanımlama gruplarını işler. Bir başarısızlık yönteminin uygulanması, kayıt düzeninin yeniden işlenebilmesi için yeniden yaymasına olanak tanır.

2. **Spout** sınıfının içeriğini aşağıdaki metinle değiştirin: Bu Spout, topolojiye rastgele bir tümce yayar.

    ```csharp
    private Context ctx;
    private Random r = new Random();
    string[] sentences = new string[] {
        "the cow jumped over the moon",
        "an apple a day keeps the doctor away",
        "four score and seven years ago",
        "snow white and the seven dwarfs",
        "i am at two with nature"
    };

    public Spout(Context ctx)
    {
        // Set the instance context
        this.ctx = ctx;

        Context.Logger.Info("Generator constructor called");

        // Declare Output schema
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // The schema for the default output stream is
        // a tuple that contains a string field
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
    }

    // Get an instance of the spout
    public static Spout Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Spout(ctx);
    }

    public void NextTuple(Dictionary<string, Object> parms)
    {
        Context.Logger.Info("NextTuple enter");
        // The sentence to be emitted
        string sentence;

        // Get a random sentence
        sentence = sentences[r.Next(0, sentences.Length - 1)];
        Context.Logger.Info("Emit: {0}", sentence);
        // Emit it
        this.ctx.Emit(new Values(sentence));

        Context.Logger.Info("NextTuple exit");
    }

    public void Ack(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }
    ```

### <a name="implement-the-bolts"></a>Cıvatları uygulama

1. Varolan **Bolt.cs** dosyasını projeden silin.

2. **Çözüm Gezgini**, projeye sağ tıklayın ve**Yeni öğe** **Ekle** > ' yi seçin. Listeden **fırtınası**' ı seçin ve ad olarak **Splitter.cs** girin. **Counter.cs**adlı ikinci bir sürgüü oluşturmak için bu işlemi tekrarlayın.

   * **Splitter.cs**: Cümleleri bağımsız sözcüklere ayıran bir sürgüler uygular ve yeni bir sözcük akışı yayar.

   * **Counter.cs**: Her bir sözcüğü sayan ve her sözcük için yeni bir sözcük akışı ve sayı getiren bir sürgüsü uygular.

     > [!NOTE]  
     > Bu cıvatları, akışları okur ve yazar, ancak bir veritabanı veya hizmet gibi kaynaklarla iletişim kurmak için de bir cıvata kullanabilirsiniz.

3. **Splitter.cs**'i açın. Varsayılan olarak yalnızca bir yönteme sahiptir: **Yürütün**. Sürgülü işleme için bir demet aldığında Execute yöntemi çağrılır. Burada, gelen tanımlama gruplarını okuyabilir ve işleyebilir ve giden tanımlama gruplarını görüntüleyebilirsiniz.

4. **Splitter** sınıfının içeriğini şu kodla değiştirin:

    ```csharp
    private Context ctx;

    // Constructor
    public Splitter(Context ctx)
    {
        Context.Logger.Info("Splitter constructor called");
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // Input contains a tuple with a string field (the sentence)
        inputSchema.Add("default", new List<Type>() { typeof(string) });
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // Outbound contains a tuple with a string field (the word)
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance of the bolt
    public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Splitter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get the sentence from the tuple
        string sentence = tuple.GetString(0);
        // Split at space characters
        foreach (string word in sentence.Split(' '))
        {
            Context.Logger.Info("Emit: {0}", word);
            //Emit each word
            this.ctx.Emit(new Values(word));
        }

        Context.Logger.Info("Execute exit");
    }
    ```

5. **Counter.cs**'i açın ve sınıf içeriğini şu kodla değiştirin:

    ```csharp
    private Context ctx;

    // Dictionary for holding words and counts
    private Dictionary<string, int> counts = new Dictionary<string, int>();

    // Constructor
    public Counter(Context ctx)
    {
        Context.Logger.Info("Counter constructor called");
        // Set instance context
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string field - the word
        inputSchema.Add("default", new List<Type>() { typeof(string) });

        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string and integer field - the word and the word count
        outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance
    public static Counter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Counter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get the word from the tuple
        string word = tuple.GetString(0);
        // Do we already have an entry for the word in the dictionary?
        // If no, create one with a count of 0
        int count = counts.ContainsKey(word) ? counts[word] : 0;
        // Increment the count
        count++;
        // Update the count in the dictionary
        counts[word] = count;

        Context.Logger.Info("Emit: {0}, count: {1}", word, count);
        // Emit the word and count information
        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
        Context.Logger.Info("Execute exit");
    }
    ```

### <a name="define-the-topology"></a>Topolojiyi tanımlama

Spout ve cıvatları, verilerin bileşenler arasında nasıl akacağını tanımlayan bir grafikte düzenlenir. Bu topoloji için grafik aşağıdaki gibidir:

![Bileşenlerin nasıl düzenlendiğine ilişkin diyagram](./media/apache-storm-develop-csharp-visual-studio-topology/word-count-topology1.png)

Cümleler Spout 'den dağıtılır ve Splitter sürgüsü örneklerine dağıtılır. Bölümlendirici, tümce, sayaç ayraçına dağıtılan kelimelerin içine alınır.

Sözcük sayısı sayaç örneğinde yerel olarak tutulduğundan, belirli sözcüklerin aynı sayaç sürgüsü örneğine akmasını sağlamak istersiniz. Her örnek belirli sözcükleri izler. Splitter sürgüsü durum içermediğinden, bu, ayırıcının hangi örneğinin hangi cümleyi alacağını oldukça önemlidir.

**Program.cs**'i açın. Önemli yöntem, fırtınası 'ya gönderilen topolojiyi tanımlamak için kullanılan **Gettopologyıbuilder**' dır. Daha önce açıklanan topolojiyi uygulamak için **Gettopologybuilder** 'ın içeriğini aşağıdaki kodla değiştirin:

```csharp
// Create a new topology named 'WordCount'
TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

// Add the spout to the topology.
// Name the component 'sentences'
// Name the field that is emitted as 'sentence'
topologyBuilder.SetSpout(
    "sentences",
    Spout.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
    },
    1);
// Add the splitter bolt to the topology.
// Name the component 'splitter'
// Name the field that is emitted 'word'
// Use suffleGrouping to distribute incoming tuples
//   from the 'sentences' spout across instances
//   of the splitter
topologyBuilder.SetBolt(
    "splitter",
    Splitter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
    },
    1).shuffleGrouping("sentences");

// Add the counter bolt to the topology.
// Name the component 'counter'
// Name the fields that are emitted 'word' and 'count'
// Use fieldsGrouping to ensure that tuples are routed
//   to counter instances based on the contents of field
//   position 0 (the word). This could also have been
//   List<string>(){"word"}.
//   This ensures that the word 'jumped', for example, will always
//   go to the same instance
topologyBuilder.SetBolt(
    "counter",
    Counter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
    },
    1).fieldsGrouping("splitter", new List<int>() { 0 });

// Add topology config
topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
{
    {"topology.kryo.register","[\"[B\"]"}
});

return topologyBuilder;
```

## <a name="submit-the-topology"></a>Topolojiyi gönder

1. **Çözüm Gezgini**, projeye sağ tıklayın ve **HDInsight 'Ta fırtınası 'ya gönder**' i seçin.

   > [!NOTE]  
   > İstenirse, Azure aboneliğinizin kimlik bilgilerini girin. Birden fazla aboneliğiniz varsa, HDInsight kümesi için bir fırtınası içeren bir oturum açın.

2. Aşağı **fırtınası** kümesi aşağı açılan listesinden HDInsight kümesi ' ni seçin ve ardından **Gönder**' i seçin. **Çıkış** penceresini kullanarak gönderimin başarılı olup olmadığını izleyebilirsiniz.

3. Topoloji başarıyla gönderildiğinde, kümenin **fırtınası topolojileri** görünmelidir. Çalışan topoloji hakkındaki bilgileri görüntülemek için listeden **WORDCOUNT** topolojisini seçin.

   > [!NOTE]  
   > Ayrıca, **Sunucu Gezgini**karşı **fırtınası topolojilerini** görüntüleyebilirsiniz. **Azure** > **HDInsight**' ı genişletin, HDInsight kümesinde bir fırtınası ' a sağ tıklayın ve ardından **fırtınası topolojilerini görüntüle**' yi seçin.

    Topolojideki bileşenler hakkında bilgi görüntülemek için diyagramdaki bileşene çift tıklayın.

4. Topolojiyi durdurmak için **topoloji Özeti** görünümünden **KILL** ' e tıklayın.

   > [!NOTE]  
   > Fırtınası topolojileri devre dışı bırakılıncaya veya küme silinene kadar çalışmaya devam eder.

## <a name="transactional-topology"></a>İşlem topolojisi

Önceki topoloji işlem dışı. Topolojideki bileşenler, iletileri yeniden almak için işlevsellik uygulamaz. İşlem topolojisine bir örnek için, bir proje oluşturun ve proje türü olarak **fırtınası örneği** ' ni seçin.

İşlem topolojileri, verileri yeniden yürütmeyi desteklemek için aşağıdakileri uygular:

* **Meta veri önbelleğe alma**: Spout, verileri elde etmek ve bir hata oluşursa tekrar yayınlanabilmesi için, oluşturulan verilerle ilgili meta verileri depolamalıdır. Örnek tarafından yayılan veriler küçük olduğundan, her bir demet için ham veriler yeniden oynatma için bir sözlükte depolanır.

* **ACK**: Topolojideki her bir sürgülü bir `this.ctx.Ack(tuple)` kayıt düzeni başarıyla işlendiğini bildirmek için çağrı yapabilir. Tüm cıvatları, kayıt kümesini kabul eder, `Ack` Spout yöntemi çağrılır. `Ack` Yöntemi, Spout 'ın yeniden oynatma için önbelleğe alınmış verileri kaldırmasına izin verir.

* **Başarısız**: Her bir sürgülü `this.ctx.Fail(tuple)` , işlemenin bir tanımlama grubu için başarısız olduğunu göstermek için çağırabilir. Hata, kayıt düzeninin önbelleğe `Fail` alınmış meta veriler kullanılarak yeniden çalınabilecek Spout yöntemine yayar.

* **Sıra kimliği**: Bir tanımlama grubu yayırken benzersiz bir sıra KIMLIĞI belirtilebilir. Bu değer, yeniden yürütme (ACK ve fail) işleme için tanımlama grubu tanımlar. Örneğin, **fırtınası örnek** projesindeki Spout, verileri yayırken aşağıdakileri kullanır:

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    Bu kod, **Lastseqıd**içinde yer alan sıra kimliği değeri ile varsayılan akışa bir cümle içeren bir tanımlama grubu yayar. Bu örnek için, **Lastseqıd** her bir tanımlama grubu için artırılır.

**Fırtınası örnek** projesinde gösterildiği gibi, bir bileşenin hareketsel olup olmadığı, yapılandırmaya göre çalışma zamanında ayarlanabilir.

## <a name="hybrid-topology-with-c-and-java"></a>Ve Java ile C# karma topolojisi

Visual Studio için Data Lake araçları 'nı, bazı bileşenlerin olduğu C# ve diğer kullanıcıların Java olan karma topolojiler oluşturmak için de kullanabilirsiniz.

Karma topolojinin bir örneği için bir proje oluşturun ve sonra da **fırtınası karma örneği**' ni seçin. Bu örnek türü aşağıdaki kavramları gösterir:

* **Java Spout** ve  **C# sürgüsü**: **HybridTopology_javaSpout_csharpBolt**içinde tanımlanmıştır.

    * İşlem sürümü **HybridTopologyTx_javaSpout_csharpBolt**içinde tanımlanır.

* Spout ve **Java sürgüsü**: **C#** **HybridTopology_csharpSpout_javaBolt**içinde tanımlanmıştır.

    * İşlem sürümü **HybridTopologyTx_csharpSpout_javaBolt**içinde tanımlanır.

  > [!NOTE]  
  > Bu sürüm ayrıca bir metin dosyasındaki Clojure kodunun Java bileşeni olarak nasıl kullanılacağını gösterir.

Proje gönderildiğinde kullanılan topolojiyi değiştirmek için, `[Active(true)]` ifadeyi kümeye göndermeden önce, kullanmak istediğiniz topolojiye taşıyın.

> [!NOTE]  
> Gerekli olan tüm Java dosyaları **Javadependency** klasöründe bu projenin bir parçası olarak sağlanır.

Karma topoloji oluştururken ve gönderirken aşağıdakileri göz önünde bulundurun:

* Bir Spout veya cıvam için Java sınıfının bir örneğini oluşturmak üzere **Javacomponentconstructor** kullanın.

* Java nesnelerinden JSON 'a veri seri hale getirmek veya bu bileşenleri kapatmak için **Microsoft. SCP. fırtınası. Multilang. CustomizedInteropJSONSerializer** kullanın.

* Topoloji sunucuya gönderilirken, **Java dosya yollarını**belirtmek için **ek konfigürasyonlar** seçeneğini kullanmanız gerekir. Belirtilen yol, Java sınıflarınızı içeren JAR dosyalarını içeren dizin olmalıdır.

### <a name="azure-event-hubs"></a>Azure Event Hubs

SCP.NET Version 0.9.4.203, özellikle Olay Hub 'ı (Event Hubs okuyan bir Java Spout) ile birlikte çalışmak için yeni bir sınıf ve yöntem sunar. Bir olay hub 'ı kullanan bir topoloji oluşturduğunuzda aşağıdaki yöntemleri kullanın:

* **Eventhubspoutconfig** Sınıfı: Spout bileşeni yapılandırmasını içeren bir nesne oluşturur.

* **Topologybuilder. SetEventHubSpout** yöntemi: Olay Hub 'ı Spout bileşenini topolojiye ekler.

> [!NOTE]  
> Spout tarafından üretilen verileri seri hale getirmek için yine de **CustomizedInteropJSONSerializer** kullanmanız gerekir.

## <a id="configurationmanager"></a>ConfigurationManager kullanın

Cıvam ve Spout bileşenlerinden yapılandırma değerlerini almak için **ConfigurationManager** kullanmayın. Bunun yapılması null işaretçi özel durumuna neden olabilir. Bunun yerine, projenizin yapılandırması, topoloji bağlamına anahtar ve değer çifti olarak fırtınası topolojisine geçirilir. Yapılandırma değerlerini kullanan her bir bileşen, başlatma sırasında bunları bağlamdan almalıdır.

Aşağıdaki kod, bu değerlerin nasıl alınacağını gösterir:

```csharp
public class MyComponent : ISCPBolt
{
    // To hold configuration information loaded from context
    Configuration configuration;
    ...
    public MyComponent(Context ctx, Dictionary<string, Object> parms)
    {
        // Save a copy of the context for this component instance
        this.ctx = ctx;
        // If it exists, load the configuration for the component
        if(parms.ContainsKey(Constants.USER_CONFIG))
        {
            this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
        }
        // Retrieve the value of "Foo" from configuration
        var foo = this.configuration.AppSettings.Settings["Foo"].Value;
    }
    ...
}
```

Bileşeninizin bir örneğini `Get` döndürmek için bir yöntem kullanıyorsanız, oluşturucuya `Context` hem hem de `Dictionary<string, Object>` parametrelerini geçirdiğinden emin olmanız gerekir. Aşağıdaki örnek, bu değerleri düzgün `Get` şekilde geçiren temel bir yöntemdir:

```csharp
public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
{
    return new MyComponent(ctx, parms);
}
```

## <a name="how-to-update-scpnet"></a>SCP.NET 'i güncelleştirme

SCP.NET Destek paketinin son sürümleri NuGet aracılığıyla yükseltilir. Yeni bir güncelleştirme kullanılabilir olduğunda, bir yükseltme bildirimi alırsınız. Bir yükseltmeyi el ile denetlemek için şu adımları izleyin:

1. **Çözüm Gezgini**’nde projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.

2. Paket Yöneticisi ' nden **güncelleştirmeler**' i seçin. Bir güncelleştirme varsa, bu listelenir. Paketi yüklemek için **Güncelleştir** ' e tıklayın.

> [!IMPORTANT]  
> Projeniz NuGet kullanmayan bir SCP.NET önceki sürümüyle oluşturulduysa, daha yeni bir sürüme güncelleştirmek için aşağıdaki adımları gerçekleştirmeniz gerekir:
>
> 1. **Çözüm Gezgini**’nde projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
> 2. **Arama** alanını kullanarak, için **Microsoft. SCP. net. SDK** ' yı arayın ve projeye ekleyin.

## <a name="troubleshoot-common-issues-with-topologies"></a>Topolojilerle ilgili yaygın sorunları giderme

### <a name="null-pointer-exceptions"></a>Null işaretçi özel durumları

Linux tabanlı HDInsight kümesiyle bir C# topoloji kullanırken, çalışma zamanında yapılandırma ayarlarını okumak için **ConfigurationManager** kullanan cıvam ve Spout bileşenleri null işaretçi özel durumları döndürebilir.

Projenizin yapılandırması, topoloji bağlamına anahtar ve değer çifti olarak fırtınası topolojisine geçirilir. Başlatıldıklarında, bileşenlerinizi geçirilen sözlük nesnesinden alınabilir.

Daha fazla bilgi için bu belgenin [ConfigurationManager](#configurationmanager) bölümüne bakın.

### <a name="systemtypeloadexception"></a>System. TypeLoadException

Linux tabanlı HDInsight kümesiyle bir C# topoloji kullanırken aşağıdaki hatayla karşılaşabilirsiniz:

    System.TypeLoadException: Failure has occurred while loading a type.

Bu hata, mono 'nin desteklediği .NET sürümü ile uyumlu olmayan bir ikili kullandığınızda oluşur.

Linux tabanlı HDInsight kümeleri için, projenizin .NET 4,5 için derlenmiş ikili dosyaları kullandığından emin olun.

### <a name="test-a-topology-locally"></a>Topolojiyi yerel olarak test etme

Bir kümeye topoloji dağıtmak kolay olsa da, bazı durumlarda bir topolojiyi yerel olarak test etmeniz gerekebilir. Bu makaledeki örnek topolojiyi geliştirme ortamınızda yerel olarak çalıştırmak ve test etmek için aşağıdaki adımları kullanın.

> [!WARNING]  
> Yerel test yalnızca temel, C#-yalnızca topolojiler için geçerlidir. Birden çok akış kullanan karma topolojiler veya topolojiler için yerel test kullanamazsınız.

1. **Çözüm Gezgini**, projeye sağ tıklayın ve **Özellikler**' i seçin. Proje Özellikleri ' nde, **çıkış türünü** **konsol uygulaması**olarak değiştirin.

    ![Çıktı türü vurgulanmış olarak proje özelliklerinin ekran görüntüsü](./media/apache-storm-develop-csharp-visual-studio-topology/hdi-output-type-window.png)

   > [!NOTE]
   > Topolojiyi bir kümeye dağıtmadan önce, **çıkış türünü** yeniden **sınıf kitaplığına** değiştirmeyi unutmayın.

1. **Çözüm Gezgini**, projeye sağ tıklayın ve ardından**Yeni öğe** **Ekle** > ' yi seçin. **Sınıf**' ı seçin ve sınıf adı olarak **LocalTest.cs** girin. Son olarak, **Ekle**' ye tıklayın.

1. **LocalTest.cs**açın ve aşağıdaki **using** ifadesini en üste ekleyin:

    ```csharp
    using Microsoft.SCP;
    ```

1. **Localtest** sınıfının içeriği olarak aşağıdaki kodu kullanın:

    ```csharp
    // Drives the topology components
    public void RunTestCase()
    {
        // An empty dictionary for use when creating components
        Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

        #region Test the spout
        {
            Console.WriteLine("Starting spout");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext spoutCtx = LocalContext.Get();
            // Get a new instance of the spout, using the local context
            Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

            // Emit 10 tuples
            for (int i = 0; i < 10; i++)
            {
                sentences.NextTuple(emptyDictionary);
            }
            // Use LocalContext to persist the data stream to file
            spoutCtx.WriteMsgQueueToFile("sentences.txt");
            Console.WriteLine("Spout finished");
        }
        #endregion

        #region Test the splitter bolt
        {
            Console.WriteLine("Starting splitter bolt");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext splitterCtx = LocalContext.Get();
            // Get a new instance of the bolt
            Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);

            // Set the data stream to the data created by the spout
            splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
            // Get a batch of tuples from the stream
            List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
            // Process each tuple in the batch
            foreach (SCPTuple tuple in batch)
            {
                splitter.Execute(tuple);
            }
            // Use LocalContext to persist the data stream to file
            splitterCtx.WriteMsgQueueToFile("splitter.txt");
            Console.WriteLine("Splitter bolt finished");
        }
        #endregion

        #region Test the counter bolt
        {
            Console.WriteLine("Starting counter bolt");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext counterCtx = LocalContext.Get();
            // Get a new instance of the bolt
            Counter counter = Counter.Get(counterCtx, emptyDictionary);

            // Set the data stream to the data created by splitter bolt
            counterCtx.ReadFromFileToMsgQueue("splitter.txt");
            // Get a batch of tuples from the stream
            List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
            // Process each tuple in the batch
            foreach (SCPTuple tuple in batch)
            {
                counter.Execute(tuple);
            }
            // Use LocalContext to persist the data stream to file
            counterCtx.WriteMsgQueueToFile("counter.txt");
            Console.WriteLine("Counter bolt finished");
        }
        #endregion
    }
    ```

    Kod açıklamalarını okumak için bir dakikanızı ayırın. Bu kod, geliştirme ortamındaki bileşenleri çalıştırmak için **Localcontext** kullanır ve bileşenler arasında yerel sürücüdeki metin dosyalarına veri akışını sürdürür.

1. **Program.cs**açın ve aşağıdakini **Main** yöntemine ekleyin:

    ```csharp
    Console.WriteLine("Starting tests");
    System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
    // Initialize the runtime
    SCPRuntime.Initialize();

    //If we are not running under the local context, throw an error
    if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
    {
        throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
    }
    // Create test instance
    LocalTest tests = new LocalTest();
    // Run tests
    tests.RunTestCase();
    Console.WriteLine("Tests finished");
    Console.ReadKey();
    ```

1. Değişiklikleri kaydedin ve ardından **F5** ' e tıklayın veya projeyi başlatmak için hata**ayıklamayı Başlat** ' **ı seçin.**  >  Bir konsol penceresi görünür ve test ilerleme durumu olarak günlüğe kaydedilir. **Sınamalar tamamlandığında** , pencereyi kapatmak için herhangi bir tuşa basın.

1. Projenizi içeren dizini bulmak için **Windows Gezgini** 'ni kullanın. Örneğin: **C:\Users\<your_user_name > \ Studio 2013 \ projeler\wordcount\wordcount**. Bu dizinde, **bin**' i açın ve ardından **Hata Ayıkla**' ya tıklayın. Testler çalıştırıldığında üretilen metin dosyalarını görmeniz gerekir: cümleler. txt, Counter. txt ve Splitter. txt. Her metin dosyasını açın ve verileri inceleyin.

   > [!NOTE]  
   > Dize verileri, bu dosyalardaki ondalık değerlerin bir dizisi olarak devam ettirir. Örneğin, [ \[97.103.111]], **Splitter. txt** dosyasında *ve*kelimedir.

> [!NOTE]  
> HDInsight kümesindeki bir fırtınası üzerine dağıtım yapmadan önce **proje türünü** **sınıf kitaplığına** geri ayarladığınızdan emin olun.

### <a name="log-information"></a>Günlük bilgileri

Kullanarak `Context.Logger`topoloji bileşenlerinizin bilgilerini kolayca günlüğe kaydedebilirsiniz. Örneğin, aşağıdaki komut bir bilgi günlüğü girişi oluşturur:

```csharp
Context.Logger.Info("Component started");
```

Günlüğe kaydedilen bilgiler, **Sunucu Gezgini**bulunan **Hadoop hizmeti günlüğünden**görüntülenebilir. HDInsight kümesinde fırtınası için girişi genişletin ve ardından **Hadoop hizmeti günlüğü**' ne genişletin. Son olarak, görüntülenecek günlük dosyasını seçin.

> [!NOTE]  
> Günlükler, kümeniz tarafından kullanılan Azure depolama hesabında depolanır. Günlükleri Visual Studio 'da görüntülemek için, depolama hesabının sahibi olan Azure aboneliğinde oturum açmanız gerekir.

### <a name="view-error-information"></a>Hata bilgilerini görüntüle

Çalışan bir topolojide oluşan hataları görüntülemek için aşağıdaki adımları kullanın:

1. **Sunucu Gezgini**, HDInsight kümesindeki fırtınası kümesine sağ tıklayın ve **fırtınası topolojilerini görüntüle**' yi seçin.

2. **Spout** ve **Cıvatları**için **son hata** sütunu son hata hakkındaki bilgileri içerir.

3. Listelenen bir hata içeren bileşen için **Spout kimliği** veya **cıvatıd kimliğini** seçin. Görüntülenen ayrıntılar sayfasında, sayfanın altındaki **hatalar** bölümünde ek hata bilgileri listelenir.

4. Daha fazla bilgi edinmek için, sayfanın **Yürüticileri** bölümünde bir **bağlantı noktası** seçin.

### <a name="errors-submitting-topologies"></a>Topolojileri gönderme hataları

HDInsight 'a topoloji gönderme hatalarıyla karşılaşırsanız, HDInsight kümenizdeki topoloji gönderimini işleyen sunucu tarafı bileşenlerine yönelik Günlükler bulabilirsiniz. Bu günlükleri almak için komut satırından aşağıdaki komutu kullanın:

    scp sshuser@clustername-ssh.azurehdinsight.net:/var/log/hdinsight-scpwebapi/hdinsight-scpwebapi.out .

__Sshuser__ ÖĞESINI kümenin SSH kullanıcı hesabı ile değiştirin. __Clustername__ değerini HDInsight kümesinin adıyla değiştirin. `scp` Ve`ssh` HDInsight ile kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

Gönderimler birden çok nedenden dolayı başarısız olabilir:

* JDK yüklü değil veya yolda değil.
* Gerekli Java bağımlılıkları gönderimi içermez.
* Uyumsuz bağımlılıklar.
* Yinelenen topoloji adları.

`hdinsight-scpwebapi.out` Günlük bir`FileNotFoundException`içeriyorsa, bu durum aşağıdaki koşullardan kaynaklanabilir:

* JDK, geliştirme ortamındaki yolda değil. JDK 'nin geliştirme ortamında yüklü olduğunu ve bu `%JAVA_HOME%/bin` yolda olduğunu doğrulayın.
* Java bağımlılığı eksik. Gönderim kapsamında gerekli. jar dosyalarını dahil ettiğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs verilerinin işlenmesiyle ilgili bir örnek için bkz. [HDInsight 'Ta fırtınası Ile Azure Event Hubs işleme olayları](apache-storm-develop-csharp-event-hub-topology.md).

Akış verilerini birden çok akışa C# ayıran topolojinin bir örneği için bkz [ C# . fırtınası örneği](https://github.com/Blackmist/csharp-storm-example).

Topolojiler oluşturma C# hakkında daha fazla bilgi edinmek için bkz. [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).

HDInsight ve HDInsight örnekleri üzerinde daha fazla fırtınası ile çalışmanın daha fazla yolu için aşağıdaki belgelere bakın:

**Microsoft SCP.NET**

* [SCP Programlama Kılavuzu](apache-storm-scp-programming-guide.md)

**HDInsight üzerinde Apache Storm**

* [HDInsight üzerinde Apache Storm topolojilerini dağıtma ve izleme](apache-storm-deploy-monitor-topology.md)
* [HDInsight üzerinde Apache Storm için örnek topolojiler](apache-storm-example-topology.md)

**HDInsight üzerinde Apache Hadoop**

* [HDInsight üzerinde Apache Hadoop ile Apache Hive kullanma](../hadoop/hdinsight-use-hive.md)
* [HDInsight üzerinde Apache Hadoop Apache Pig kullanma](../hadoop/hdinsight-use-pig.md)
* [HDInsight üzerinde Apache Hadoop MapReduce Apache Hadoop kullanma](../hadoop/hdinsight-use-mapreduce.md)

**HDInsight üzerinde Apache HBase**

* [HDInsight 'ta Apache HBase ile çalışmaya başlama](../hbase/apache-hbase-tutorial-get-started-linux.md)
