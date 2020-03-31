---
title: Visual Studio ve C# ile Apache Storm topolojileri - Azure HDInsight
description: C#'da Fırtına topolojisi oluşturmayı öğrenin. Visual Studio için Hadoop araçlarını kullanarak Visual Studio'da bir kelime sayısı topolojisi oluşturun.
ROBOTS: NOINDEX
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/31/2019
ms.openlocfilehash: 1903c2faab865152d1f3666f3c9dadd745058b56
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75612300"
---
# <a name="develop-c-topologies-for-apache-storm-by-using-the-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake araçlarını kullanarak Apache Storm için C# topolojileri geliştirin

Visual Studio için Azure Veri Gölü (Apache Hadoop) araçlarını kullanarak C# Apache Storm topolojisi oluşturmayı öğrenin. Bu belge Visual Studio'da bir Fırtına projesi oluşturma, yerel olarak test etme ve Azure HDInsight kümesinde bir Apache Storm'a dağıtma sürecinden geçer.

Ayrıca, C# ve Java bileşenlerini kullanan hibrit topolojileri nasıl oluşturabileceğinizi de öğrenirsiniz.

C# topolojileri .NET 4.5'i kullanır ve HDInsight kümesinde çalışmak için Mono'yu kullanır. Olası uyumsuzluklar hakkında bilgi için [Mono uyumluluğu'na](https://www.mono-project.com/docs/about-mono/compatibility/)bakın. C# topolojisini kullanmak için, `Microsoft.SCP.Net.SDK` projeniz tarafından kullanılan NuGet paketini 0.10.0.6 veya sonraki sürümlere güncelleştirmeniz gerekir. Paketin sürümünün ayrıca HDInsight üzerinde yüklü olan Storm ana sürümüyle eşleşmesi gerekir.

| HDInsight sürümü | Apache Storm sürümü | SCP.NET sürümü | Varsayılan Mono sürümü |
|:-----------------:|:-------------:|:---------------:|:--------------------:|
| 3.4 | 0.10.0.x | 0.10.0.x | 3.2.8 |
| 3,5 | 1.0.2.x | 1.0.0.x | 4.2.1 |
| 3.6 | 1.1.0.x | 1.0.0.x | 4.2.8 |

## <a name="prerequisite"></a>Önkoşul

HDInsight'ta bir Apache Storm kümesi. Bkz. [Azure portalını kullanarak Apache Hadoop kümeleri oluşturun](../hdinsight-hadoop-create-linux-clusters-portal.md) ve Küme türü için **Fırtına'yı**seçin. **Storm**

## <a name="install-visual-studio"></a>Visual Studio yükleme

[Visual Studio'yı](https://visualstudio.microsoft.com/downloads/)kullanarak SCP.NET ile C# topolojileri geliştirebilirsiniz. Buradaki talimatlar Visual Studio 2019'u kullanır, ancak Visual Studio'nun önceki sürümlerini de kullanabilirsiniz.

## <a name="install-data-lake-tools-for-visual-studio"></a>Visual Studio için Veri Gölü araçlarını yükleyin

Visual Studio için Veri Gölü araçlarını yüklemek [için, Visual Studio için Veri Gölü araçlarını kullanmaya başlayın'daki](../hadoop/apache-hadoop-visual-studio-tools-get-started.md#install-data-lake-tools-for-visual-studio)adımları izleyin.

## <a name="install-java"></a>Java'yı Yükle

Visual Studio'dan bir Storm topolojisi gönderdiğinde, SCP.NET topoloji ve bağımlılıkları içeren bir zip dosyası oluşturur. Java, Linux tabanlı kümelerle daha uyumlu bir biçim kullandığından, bu zip dosyalarını oluşturmak için kullanılır.

1. Geliştirme ortamınıza Java Geliştirici Kiti7 veya daha sonra yükleyin. [Oracle](https://openjdk.java.net/)Oracle'dan Oracle JDK alabilirsiniz. Diğer Java [dağıtımlarını](/java/azure/jdk/)da kullanabilirsiniz.

2. Ortam `JAVA_HOME` değişkenini Java içeren dizine ayarlayın.

3. Ortam `PATH` değişkenini `%JAVA_HOME%\bin` dizini içerecek şekilde ayarlayın.

Java ve JDK'nın doğru yüklenmiş olduğunu doğrulamak için aşağıdaki C# konsol uygulamasını oluşturabilir ve çalıştırabilirsiniz:

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

## <a name="apache-storm-templates"></a>Apaçi Fırtınası şablonları

Visual Studio için Veri Gölü araçları aşağıdaki şablonları sağlar:

| Proje türü | Gösteriler |
| --- | --- |
| Fırtına Uygulaması |Boş bir Fırtına topoloji projesi. |
| Storm Azure SQL Yazar Örneği |Azure SQL Veritabanına nasıl yazılır? |
| Storm Azure Cosmos DB Okuyucu Örneği |Azure Cosmos DB'den okuma. |
| Storm Azure Cosmos DB Yazar Örneği |Azure Cosmos DB'ye nasıl yazılır? |
| Fırtına EventHub Okuyucu Örneği |Azure Etkinlik Hub'larından okuma. |
| Fırtına EventHub Yazar Örneği |Azure Etkinlik Hub'larına yazma. |
| Fırtına HBase Okuyucu Örneği |HDInsight kümelerinde HBase'den okuma. |
| Fırtına HBase Yazar Örnek |HDInsight kümelerinde HBase'e nasıl yazılır? |
| Fırtına Hibrid Örnek |Java bileşeni nasıl kullanılır? |
| Fırtına Örneği |Temel bir kelime sayısı topoloji. |

> [!WARNING]  
> Tüm şablonlar Linux tabanlı HDInsight ile çalışmaz. Şablonlar tarafından kullanılan NuGet paketleri Mono ile uyumlu olmayabilir. Olası sorunları belirlemek için [Mono uyumluluğu'na](https://www.mono-project.com/docs/about-mono/compatibility/) bakın ve [.NET Taşınabilirlik Çözümleyicisini](../hdinsight-hadoop-migrate-dotnet-to-linux.md#automated-portability-analysis)kullanın.

Bu belgedeki adımlarda, bir topoloji oluşturmak için temel Fırtına Uygulaması proje türünü kullanırsınız.

### <a name="apache-hbase-templates"></a>Apache HBase şablonları

HBase okuyucu ve yazar şablonları HDInsight kümesinde bir HBase ile iletişim kurmak için HBase JAVA API değil, HBase REST API kullanın.

### <a name="eventhub-templates"></a>EventHub şablonları

> [!IMPORTANT]  
> EventHub Reader şablonuna dahil olan Java tabanlı EventHub spout bileşeni, HDInsight sürüm 3.5 veya sonraki sürümde Storm ile çalışmayabilir. Bu bileşenin güncelleştirilmiş bir sürümü [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/HDI3.5/lib)mevcuttur.

Bu bileşeni kullanan ve HDInsight 3.5'te Storm ile çalışan bir örnek topoloji için [GitHub'a](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub)bakın.

## <a name="create-a-c-topology"></a>C# topolojisi oluşturma

Visual Studio'da bir C# topoloji projesi oluşturmak için:

1. Visual Studio'yu açın.

1. **Başlat** penceresinde yeni **bir proje oluştur'u**seçin.

1. Yeni **bir proje oluştur** penceresinde, **Fırtına Uygulaması'na**kaydırın ve seçin, ardından **İleri'yi**seçin.

1. Yeni **proje pencerenizi Yapılandır' da** *WordCount'un* **proje adını** girin, projeye gidin veya proje için **Konum** dizini yolu oluşturun ve ardından **Oluştur'u**seçin.

    ![Fırtına uygulaması, Yeni proje iletişim kutusu, Visual Studio yapıla](./media/apache-storm-develop-csharp-visual-studio-topology/apache-storm-new-project.png)

Projeyi oluşturduktan sonra aşağıdaki dosyalara sahip olmalısınız:

* *Program.cs*: Projenizin topoloji tanımı. Varsayılan olarak bir emzme ve bir cıvatadan oluşan varsayılan topoloji oluşturulur.

* *Spout.cs*: Rastgele sayılar yayılan bir örnek.

* *Bolt.cs*: Emkitarafından yayılan sayıların sayısını tutan örnek bir cıvatadır.

Projeyi oluşturduğunuzda, NuGet en son [SCP.NET paketini](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/)indirir.

### <a name="implement-the-spout"></a>Spout'u uygulayın

Ardından, topolojideki verileri harici bir kaynaktan okumak için kullanılan emzitiçin kodu ekleyin. Bu spout rastgele topoloji içine bir cümle yalar.

1. Açık *Spout.cs*. Bir emzit için ana bileşenler şunlardır:

   * `NextTuple`: Emzin yeni tuples yatmasına izin verildiğinde Fırtına tarafından çağrılır.

   * `Ack`(yalnızca işlemsel topoloji): Topolojideki diğer bileşenler tarafından, pout'tan gönderilen tupüller için başlatılan bildirimleri işler. Bir tuple'ı kabul etmek, supout'un akış aşağı bileşenleri tarafından başarılı bir şekilde işlenmiş olduğunu bilmesini sağlar.

   * `Fail`(yalnızca işlemsel topoloji): Topolojideki diğer bileşenleri başarısız işleyen tupülleri işler. Bir `Fail` yöntem uygulamak, yeniden işlenebilecek şekilde tuple'ı yeniden yayılabilmenizi sağlar.

2. `Spout` Sınıfın içeriğini aşağıdaki metinle değiştirin:

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

### <a name="implement-the-bolts"></a>Cıvataları uygulayın

Şimdi bu örnekte iki Storm cıvata oluşturun:

1. Varolan *Bolt.cs* dosyasını projeden silin.

2. **Çözüm Gezgini'nde**projeyi sağ tıklatın ve**Yeni öğe** **ekle'yi** > seçin. Listeden Storm **Bolt'u**seçin ve ad olarak *Splitter.cs* girin. Yeni dosyanın kodunda, ad alanı adını `WordCount`' la değiştirin. Daha sonra *Counter.cs*adlı ikinci bir cıvata oluşturmak için bu işlemi tekrarlayın.

   * *Splitter.cs*: Cümleleri tek tek sözcüklere bölen ve yeni bir kelime akışı yayar.

   * *Counter.cs*: Her sözcüğü sayan bir cıvata uygular ve her kelime için yeni bir kelime akışı ve sayı yakar.

     > [!NOTE]  
     > Bu cıvatalar akışları okumak ve yazmak, ancak bir veritabanı veya hizmet gibi kaynaklarla iletişim kurmak için bir cıvata kullanabilirsiniz.

3. Açık *Splitter.cs*. Varsayılan olarak tek bir `Execute`yöntemi vardır: . Cıvata `Execute` işleme için bir tuple aldığında yöntem denir. Burada, gelen tuples okuyabilir ve işleyebilir ve giden tuples yontma.

4. `Splitter` Sınıfın içeriğini aşağıdaki kodla değiştirin:

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

5. *Counter.cs*açın ve sınıf içeriğini aşağıdaki kodla değiştirin:

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

### <a name="define-the-topology"></a>Topolojiyi tanımlayın

Dışa doğru lar ve cıvatalar, verilerin bileşenler arasında nasıl aktığını tanımlayan bir grafikte düzenlenir. Bu topoloji için grafik aşağıdaki gibidir:

![Emzit ve cıvata bileşen düzenleme diyagramı, Fırtına topolojisi](./media/apache-storm-develop-csharp-visual-studio-topology/word-count-topology1.png)

Püskürtme, Splitter cıvataörneklerine dağıtılan cümleler yayar. Splitter cıvatası cümleleri Sayaç cıvatasına dağıtılan sözcüklere böler.

Sayaç örneği sözcük sayısını yerel olarak tuttuğundan, belirli sözcüklerin aynı Sayaç cıvata örneğine aktığından emin olmak istersiniz. Her örnek belirli sözcükleri izler. Splitter cıvatası hiçbir durumu korumadığından, bölücünün hangi cümleyi aldığı gerçekten önemli değildir.

Açık *Program.cs*. Önemli `GetTopologyBuilder`yöntem, Fırtına'ya gönderilen topolojiyi tanımlamak için kullanılır. Daha önce `GetTopologyBuilder` açıklanan topolojiyi uygulamak için içeriği aşağıdaki kodla değiştirin:

```csharp
// Create a new topology named 'WordCount'
TopologyBuilder topologyBuilder = new TopologyBuilder(
    "WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

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

## <a name="submit-the-topology"></a>Topolojiyi gönderin

Topolojiyi HDInsight kümenize göndermeye hazırsınız.

1. **Sunucu Gezginini** **Görüntüle'ye** > gidin.

1. Azure'a **Azure**sağ tıklayın, **Microsoft Azure Aboneliğine Bağlan'ı seçin...** ve oturum açma işlemini tamamlayın.

1. **Solution Explorer'da**projeyi sağ tıklatın ve **HDInsight'ta Fırtınaya Gönder'i**seçin.

1. **Topoloji Gönder** iletişim kutusunda, **Fırtına Kümesi** açılır listesinin altında, HDInsight kümesinde Fırtına'nızı seçin ve ardından **Gönder'i**seçin. **Çıktı** bölmesini görüntüleyerek gönderinin başarılı olup olmadığını kontrol edebilirsiniz.

    Topoloji başarıyla gönderildiğinde, küme için **Fırtına Topolojileri Görünümü** penceresi görünmelidir. Çalışan topoloji hakkındaki bilgileri görüntülemek için listeden **WordCount** topolojisini seçin.

    ![Fırtına topolojisi görüş penceresi, HDInsight kümesi, Visual Studio](./media/apache-storm-develop-csharp-visual-studio-topology/storm-topology-view.png)

    > [!NOTE]  
    > Ayrıca Server **Explorer'dan Fırtına Topolojilerini** de görüntüleyebilirsiniz. **Server Explorer** **Azure** > **HDInsight'ı**genişletin, HDInsight kümesinde Bir Fırtına'ya sağ tıklayın ve ardından Fırtına **Topolojilerini Görüntüle'yi**seçin.

    Topolojideki bileşenler hakkındaki bilgileri görüntülemek için diyagramda bir bileşen seçin.

1. **Topoloji Özeti** bölümünde, topolojiyi durdurmak için **Öldür'i** seçin.

    > [!NOTE]  
    > Fırtına topolojileri devre dışı bırakılınceye veya küme silinene kadar çalışmaya devam ederler.

## <a name="transactional-topology"></a>İşlemsel topoloji

Önceki topoloji işlem dışıdır. Topolojideki bileşenler iletileri yeniden oynatma işlevini uygulamaz. İşlemsel topoloji örneği için bir proje oluşturun ve proje türü olarak **Fırtına Örneği'ni** seçin.

İşlemsel topolojiler, verilerin yeniden oynatını desteklemek için aşağıdakileri uygular:

* **Meta veri önbelleğe alma**: Bir hata oluşursa verilerin alınabilmesi ve yeniden yayımlanabilmesi için, emdirme, yayılan veriler le ilgili meta verileri depolamalıdır. Örnek tarafından yayılan veriler küçük olduğundan, her tuple için ham veri yeniden oynatmak için bir sözlükte depolanır.

* **Ack**: Topolojideki her `this.ctx.Ack(tuple)` cıvata, bir tuple'ı başarıyla işlettiğini kabul etmek için arayabilir. Tüm cıvatalar tuple'ı kabul `Ack` ettiğinde, emzit yöntemi çağrılır. Yöntem, `Ack` emzin yeniden yürütülür için önbelleğe alınmış verileri kaldırmasına olanak tanır.

* **Fail**: Her `this.ctx.Fail(tuple)` cıvata, işlemin bir tuple için başarısız olduğunu belirtmek için arayabilir. Hata, önbelleğe `Fail` alınan meta veriler kullanılarak tuple'ın yeniden oynatılabildiği emme yöntemine yayılır.

* **SıraLı ID**: Bir tuple yayan bir zaman, benzersiz bir sıra kimliği belirtilebilir. Bu değer, yeniden oynatma (ve`Ack` `Fail`) işleme için tuple'ı tanımlar. Örneğin, **Fırtına Örneği** projesindeki emzme veri yayan aşağıdaki yöntem çağrısını kullanır:

  `this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);`

  Bu kod, 'de bulunan `lastSeqId`sıralı kimlik değeri yle birlikte varsayılan akışa bir cümle içeren bir tuple yayır. Bu örnek, `lastSeqId` yayılan her tuple için artımlı.

**Storm Sample** projesinde gösterildiği gibi, bir bileşenin işlem olup olmadığı yapılandırmaya bağlı olarak çalışma zamanında ayarlanabilir.

## <a name="hybrid-topology-with-c-and-java"></a>C# ve Java ile hibrit topoloji

Ayrıca, bazı bileşenlerin C# ve diğerlerinin Java olduğu karma topolojiler oluşturmak için Visual Studio için Veri Gölü araçlarını da kullanabilirsiniz.

Hibrit topoloji örneği için bir proje oluşturun ve **Storm Hybrid Sample'ı**seçin. Bu örneklem türü aşağıdaki kavramları gösterir:

* **Java spout** ve **C#** cıvata `HybridTopology_javaSpout_csharpBolt` : Sınıfta tanımlanır.

  `HybridTopologyTx_javaSpout_csharpBolt` Sınıfta bir işlem sürümü tanımlanır.

* **C# spout** ve **Java**cıvata `HybridTopology_csharpSpout_javaBolt` : Sınıfta tanımlanır.

  `HybridTopologyTx_csharpSpout_javaBolt` Sınıfta bir işlem sürümü tanımlanır.

  > [!NOTE]  
  > Bu sürüm ayrıca, bir metin dosyasından Clojure kodunun Java bileşeni olarak nasıl kullanılacağını da gösterir.

Proje gönderildiğinde kullanılan topolojiyi değiştirmek için, `[Active(true)]` deyimi kümeye göndermeden önce kullanmak istediğiniz topolojiye taşıyın.

> [!NOTE]  
> Gerekli olan tüm Java dosyaları *JavaDependency* klasöründe bu projenin bir parçası olarak sağlanır.

Hibrit bir topoloji oluştururken ve gönderirken aşağıdakileri göz önünde bulundurun:

* Bir `JavaComponentConstructor` emzme veya cıvata için Java sınıfının bir örneğini oluşturmak için kullanın.

* Java `microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer` nesnelerinden JSON'a kadar Java bileşenlerine verileri seri hale getirmek için kullanın.

* Topolojiyi sunucuya gönderirken, **Java Dosyası yollarını**belirtmek için Ek **yapılandırmalar** seçeneğini kullanmanız gerekir. Belirtilen yol, Java sınıflarınızı içeren JAR dosyalarına sahip dizini olmalıdır.

### <a name="azure-event-hubs"></a>Azure Event Hubs

0.9.4.203 sürümüSCP.NET, özellikle Event Hub spout (Olay Hub'larından okunan bir Java emzmesi) ile çalışmak için yeni bir sınıf ve yöntem sunar. Bir Olay Hub'ı (örneğin, **Storm EventHub Reader Sample** şablonunu kullanarak) kullanan bir topoloji oluşturduğunuzda, aşağıdaki API'leri kullanın:

* `EventHubSpoutConfig`sınıf: Emzit bileşeni nin yapılandırmasını içeren bir nesne oluşturur.

* `TopologyBuilder.SetEventHubSpout`yöntem: Olay Hub'ı emzme bileşenini topolojiye ekler.

> [!NOTE]  
> Yine de `CustomizedInteropJSONSerializer` emzit tarafından üretilen verileri serihale getirmek için kullanmanız gerekir.

## <a name="use-configurationmanager"></a>ConfigurationManager'ı kullan

Cıvata ve emzme bileşenlerinden yapılandırma değerlerini almak için **ConfigurationManager'u** kullanmayın. Bunu yapmak null işaretçi özel bir durum neden olabilir. Bunun yerine, projenizin yapılandırmasını topoloji bağlamında anahtar ve değer çifti olarak Fırtına topolojisine geçirin. Yapılandırma değerlerine dayanan her bileşen, başlatma sırasında bunları bağlamından almalıdır.

Aşağıdaki kod, bu değerlerin nasıl alınabildiğini gösterir:

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
            this.configuration = parms[Constants.USER_CONFIG] 
                as System.Configuration.Configuration;
        }
        // Retrieve the value of "Foo" from configuration
        var foo = this.configuration.AppSettings.Settings["Foo"].Value;
    }
    ...
}
```

Bileşeninizin `Get` bir örneğini döndürmek için bir yöntem kullanıyorsanız, `Context` bileşeninizin hem de `Dictionary<string, Object>` parametrelerini oluşturucuya geçtiğinden emin olmalısınız. Aşağıdaki örnek, bu `Get` değerleri düzgün bir şekilde geçen temel bir yöntemdir:

```csharp
public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
{
    return new MyComponent(ctx, parms);
}
```

## <a name="how-to-update-scpnet"></a>SCP.NET nasıl güncellenir?

NuGet aracılığıyla SCP.NET destek paketi yükseltme son sürümleri. Yeni bir güncelleştirme kullanılabilir olduğunda, bir yükseltme bildirimi alırsınız. Yükseltmeyi el ile denetlemek için aşağıdaki adımları izleyin:

1. **Çözüm Gezgini**’nde projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.

2. Paket yöneticisinden **Güncelleştirmeler'i**seçin. SCP.NET destek paketi için bir güncelleştirme varsa, listelenir. Paket için **Güncelleştir'i** seçin ve ardından **Değişiklikleri Önizleme** iletişim kutusunda yüklemek için **Tamam'ı** seçin.

> [!IMPORTANT]  
> Projeniz NuGet kullanmayan SCP.NET önceki bir sürümüyle oluşturulduysa, daha yeni bir sürüme güncelleştirmek için aşağıdaki adımları gerçekleştirmeniz gerekir:
>
> 1. **Çözüm Gezgini**’nde projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
> 2. **Arama** alanını kullanarak projeye arama yapın `Microsoft.SCP.Net.SDK` ve sonra ekleyin.

## <a name="troubleshoot-common-issues-with-topologies"></a>Topolojilerle ilgili sık karşılaşılan sorunları giderme

### <a name="null-pointer-exceptions"></a>Null işaretçi özel durumlar

Linux tabanlı HDInsight kümesine sahip bir C# topolojisi kullanıyorsanız, çalışma zamanında yapılandırma ayarlarını okumak için **ConfigurationManager'u** kullanan cıvata ve spout bileşenleri null işaretçi özel durumları döndürebilir.

Projenizin yapılandırması, topoloji bağlamında anahtar ve değer çifti olarak Fırtına topolojisine aktarılır. Başharfe çevrildiğinde bileşenlerinize geçirilen sözlük nesnesinden alınabilir.

Daha fazla bilgi için bu belgenin [ConfigurationManager kullan](#use-configurationmanager) bölümüne bakın.

### <a name="systemtypeloadexception"></a>System.TypeLoadException

Linux tabanlı HDInsight kümesine sahip bir C# topolojisi kullanıyorsanız, aşağıdaki hatayla karşılaşabilirsiniz:

`System.TypeLoadException: Failure has occurred while loading a type.`

Bu hata, Mono'nun desteklediği .NET sürümüyle uyumlu olmayan bir ikili kullandığınızda oluşur.

Linux tabanlı HDInsight kümeleri için, projenizin .NET 4.5 için derlenmiş ikili kullanımlar kullandığından emin olun.

### <a name="test-a-topology-locally"></a>Bir topolojiyi yerel olarak test edin

Bir kümeye topoloji dağıtmak kolay olsa da, bazı durumlarda bir topolojiyi yerel olarak test etmeniz gerekebilir. Bu makalede örnek topolojiyi geliştirme ortamınızda yerel olarak çalıştırmak ve test etmek için aşağıdaki adımları kullanın.

> [!WARNING]  
> Yerel testler yalnızca temel, C#-sadece topolojiler için çalışır. Birden çok akış kullanan hibrit topolojiler veya topolojiler için yerel testleri kullanamazsınız.

1. **Çözüm Gezgini'nde**projeyi sağ tıklatın ve **Özellikler'i**seçin. Proje özelliklerinde. Daha sonra **Çıktı türünü** Konsol **Uygulaması**olarak değiştirin.

   ![HDInsight Storm uygulaması, proje özellikleri, Çıkış türü](./media/apache-storm-develop-csharp-visual-studio-topology/hdi-output-type-window.png)

   > [!NOTE]
   > Topolojiyi bir kümeye dağıtmadan önce **Çıktı türünü** **Sınıf Kitaplığı'na** değiştirmeyi unutmayın.

1. **Çözüm Gezgini'nde,** projeyi sağ tıklatın ve ardından**Yeni Öğe** **Ekle'yi** > seçin. **Sınıf'ı**seçin ve sınıf adı olarak *LocalTest.cs* girin. Son olarak, **Ekle'yi**seçin.

1. *LocalTest.cs*açın ve en `using` üstte aşağıdaki ifadeyi ekleyin:

    ```csharp
    using Microsoft.SCP;
    ```

1. `LocalTest` Sınıfın içeriği olarak aşağıdaki kodu kullanın:

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

    Kod yorumlarını okumak için bir dakikaayırın. Bu kod, geliştirme ortamındaki bileşenleri çalıştırmak için kullanır. `LocalContext` Bileşenler arasındaki veri akışını yerel sürücüdeki metin dosyalarına kadar devam eder.

1. *Program.cs*açın ve `Main` yönteme aşağıdaki kodu ekleyin:

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

1. Değişiklikleri kaydedin ve ardından **F5'i** seçin veya projeyi başlatmak için **Hata Ayıklama** > **Hata Ayıklama'yı** seçin. Bir konsol penceresi görünmeli ve testler ilerledikçe durumu günlüğe kaydedin. `Tests finished` Göründüğünde, pencereyi kapatmak için herhangi bir tuşu seçin.

1. Projenizi içeren dizini bulmak için **Windows Gezgini'ni** kullanın. (Örneğin: *C:\\\\\<Kullanıcılar \\your_user_name\\>\\kaynak\\repos WordCount WordCount*.) Daha sonra bu dizinde, *Bin'i*açın ve hata *ayıklama'yı*seçin. Testler çalıştırıldığında üretilen metin dosyalarını görmelisiniz: *sentences.txt*, *counter.txt*, ve *splitter.txt*. Her metin dosyasını açın ve verileri inceleyin.

   > [!NOTE]  
   > Dize verileri, bu dosyalarda ondalık değerler dizisi olarak kalır. Örneğin, `[[97,103,111]]` **splitter.txt** dosyasında sözcüğü *önceki*sözcüğü temsil eder.

> [!NOTE]  
> HDInsight kümesinde Bir Fırtına'ya dağıtmadan önce proje özelliklerinde **Proje türünü** **Sınıf Kitaplığı'na** geri ayarladığınızda emin olun.

### <a name="log-information"></a>Günlük bilgileri

Topoloji bileşenlerinizdeki bilgileri kullanarak kolayca `Context.Logger`günlüğe kaydedebilirsiniz. Örneğin, aşağıdaki komut bir bilgi günlüğü girişi oluşturur:

`Context.Logger.Info("Component started");`

Günlüğe kaydedilmiş bilgiler **Server Explorer'da**bulunan **Hadoop Hizmet Günlüğü'nden**görüntülenebilir. HDInsight kümenizde Storm'unuzun girişini genişletin ve **hadoop Hizmet Günlüğü'nüz'i**genişletin. Son olarak, görüntülemek için günlük dosyasını seçin.

> [!NOTE]  
> Günlükler, kümeniz tarafından kullanılan Azure depolama hesabında depolanır. Visual Studio'daki günlükleri görüntülemek için depolama hesabının sahibi olan Azure aboneliğinde oturum açmanız gerekir.

### <a name="view-error-information"></a>Hata bilgilerini görüntüleme

Çalışan bir topolojide oluşan hataları görüntülemek için aşağıdaki adımları kullanın:

1. **Server Explorer'dan**HDInsight kümesinde Fırtına'ya sağ tıklayın ve **Fırtına Topolojilerini Görüntüle'yi**seçin.

   **Spout** ve **Cıvatalar**için **Son Hata** sütunu son hata yla ilgili bilgiler içerir.

2. Hata listelenen bileşen için **Spout KIMLIĞI** veya **Bolt Kimliği'ni** seçin. Ayrıntılar sayfası, sayfanın altındaki **Hatalar** bölümünde ek hata bilgilerini görüntüler.

3. Daha fazla bilgi edinmek için, son birkaç dakikaiçin Fırtına alt günlüğü görmek için sayfanın **Uygulayıcılar** bölümünden bir **Bağlantı Noktası** seçin.

### <a name="errors-submitting-topologies"></a>Topoloji gönderen hatalar

HDInsight'a bir topoloji gönderen hatalarla karşılaşırsanız, HDInsight kümenizde topoloji gönderimi işleyen sunucu tarafındaki bileşenlerin günlüklerini bulabilirsiniz. Bu günlükleri indirmek için bir komut satırından aşağıdaki komutu kullanın:

```cmd
scp sshuser@clustername-ssh.azurehdinsight.net:/var/log/hdinsight-scpwebapi/hdinsight-scpwebapi.out .
```

Küme için *sshuser* kullanıcısını SSH kullanıcı hesabıyla değiştirin. *Clustername'yi* HDInsight kümesinin adıyla değiştirin. HDInsight kullanımı `scp` ve `ssh` hdinsight ile ilgili daha fazla bilgi için, [HDInsight ile SSH'yi kullan'a](../hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

Gönderimler birden çok nedenden dolayı başarısız olabilir:

* JDK yüklü değil veya yolda değil.
* Gerekli Java bağımlılıkları gönderime dahil değildir.
* Bağımlılıklar uyumsuzdur.
* Topoloji adları çoğaltılır.

*hdinsight-scpwebapi.out* log dosyası nda `FileNotFoundException`bir , özel durum aşağıdaki koşullardan kaynaklanabilir:

* JDK geliştirme ortamında yolda değil. JDK'nın geliştirme ortamına yüklü olduğunu ve `%JAVA_HOME%/bin` bu yolun içinde olduğunu doğrulayın.
* Java bağımlılığını kaçırıyorsun. Gönderinin bir parçası olarak gerekli .jar dosyalarını dahil ettiğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Olay Hub'larından gelen verileri işleme örneği için, [HDInsight'ta Storm ile Azure Etkinlik Hub'larından İşlem etkinliklerine](apache-storm-develop-csharp-event-hub-topology.md)bakın.

Akış verilerini birden çok akışa bölen Bir C# [topolojisi örneği için Bkz. C# Storm örneğine](https://github.com/Blackmist/csharp-storm-example)bakın.

C# topolojileri oluşturma hakkında daha fazla bilgi bulmak için [GitHub'a](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md)bakın.

HDInsight ile çalışmanın daha fazla yolu ve HDInsight örneklerinde daha fazla Storm için aşağıdaki belgelere bakın:

**Microsoft SCP.NET**

* [Azure HDInsight'ta Apache Storm için SCP programlama kılavuzu](apache-storm-scp-programming-guide.md)

**HDInsight üzerinde Apache Storm**

* [Azure HDInsight'ta Apache Storm topolojilerini dağıtın ve yönetin](apache-storm-deploy-monitor-topology-linux.md)
* [Örnek Apache Storm, Azure HDInsight'ta topolojiler](apache-storm-example-topology.md)

**Apache Hadoop HDInsight üzerinde**

* [Azure HDInsight'ta Apache Hive ve HiveQL nedir?](../hadoop/hdinsight-use-hive.md)
* [HDInsight üzerinde Apache Hadoop ile MapReduce'u kullanma](../hadoop/hdinsight-use-mapreduce.md)

**HDInsight üzerinde Apache HBase**

* [Azure HDInsight’ta Apache HBase kullanma](../hbase/apache-hbase-tutorial-get-started-linux.md)
