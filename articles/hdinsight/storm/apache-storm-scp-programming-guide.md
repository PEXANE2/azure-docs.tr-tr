---
title: Azure HDInsight'ta Storm için SCP.NET programlama kılavuzu
description: Oluşturmak için SCP.NET nasıl kullanacağınızı öğrenin. Azure HDInsight'ta çalışan Storm ile kullanılmak üzere NET tabanlı Storm topolojileri.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/13/2020
ms.openlocfilehash: ddf69a75a39911293277a4a4189cf4e79256e09d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77186871"
---
# <a name="scp-programming-guide-for-apache-storm-in-azure-hdinsight"></a>Azure HDInsight'ta Apache Storm için SCP programlama kılavuzu

SCP, gerçek zamanlı, güvenilir, tutarlı ve yüksek performanslı veri işleme uygulamaları oluşturmak için bir platformdur. Açık kaynak yazılım toplulukları tarafından tasarlanmış bir akış işleme sistemi olan [Apache Storm'un](https://storm.incubator.apache.org/)üzerine inşa edilmiştir. Nathan Marz Storm'u yarattı. Twitter tarafından açık kaynak olarak yayınlandı. Fırtına [Apache ZooKeeper](https://zookeeper.apache.org/)kullanır , son derece güvenilir dağıtılmış koordinasyon ve devlet yönetimi sağlayan başka bir Apache projesidir.

SCP projesi yalnızca Windows'da Fırtına'yı değil, Windows ortamı için proje ekuzantıları ve özelleştirmeyi de işaretlemiştir. Uzantılar arasında .NET geliştirici deneyimi ve .NET kitaplıkları yer alır. Özelleştirme, Windows tabanlı dağıtımı içerir.

Uzantıları ve özelleştirme ile, açık kaynak yazılım projeleri çatal gerekmez. Storm'un üzerine inşa edilmiş türetilmiş ortamlar kullanabilirsiniz.

## <a name="processing-model"></a>İşleme modeli

SCP'deki veriler sürekli tuples akışları olarak modellenir. Tipik olarak, tuples:

1. Sıraya girin.
1. Bir Fırtına topolojisi içinde barındırılan iş mantığı tarafından alınır ve dönüştürülür.
1. Çıktıları başka bir SCP sistemine tuples olarak borulanmış veya dağıtılmış dosya sistemleri ve SQL Server gibi veritabanları gibi mağazalara adamıştır.

![Sırayla bir veri deposu besler işleme için bir kuyruk besleme veri diyagramı](./media/apache-storm-scp-programming-guide/queue-feeding-data-to-processing-to-data-store.png)

Storm'da bir uygulama topolojisi bir hesaplama grafiği tanımlar. Bir topolojideki her düğüm işleme mantığı içerir. Düğümler arasındaki bağlantılar veri akışını gösterir.

Giriş verilerini topolojiye enjekte eden _düğümlere spouts_denir. Verileri sıralamak için bunları kullanabilirsiniz. Giriş verileri dosya günlükleri, işlem veritabanı veya sistem performans sayacı gibi bir kaynaktan gelebilir.

Hem giriş hem de çıktı veri akışları olan _düğümlere cıvata_denir. Gerçek veri filtreleme, seçim ve toplama yı yaparlar.

SCP en iyi çabaları, en az bir kez ve tam olarak bir kez veri işleme destekler.

Dağıtılmış bir akış işleme uygulamasında, veri işleme sırasında hatalar meydana gelebilir. Bu tür hatalar arasında ağ kesintisi, makine hatası veya kodundaki bir hata yer almaktadır. En az bir kez işleme, bir hata olduğunda aynı verileri otomatik olarak yeniden oynatarak tüm verilerin en az bir kez işlenmesini sağlar.

En az bir kez işleme basit ve güvenilir, ve birçok uygulamalar uygun. Ancak bir uygulama tam sayma gerektirdiğinde, aynı veriler uygulama topolojisinde yeniden oynatılabileceğinden en az bir kez işleme yeterli değildir. Bu durumda, veriler birden çok kez yeniden oynandı ve işlendiğinde bile tam olarak bir kez işleme sonucunun doğru olduğundan emin olun.

SCP, .NET geliştiricilerin Storm ile java sanal makine (JVM) kullanırken gerçek zamanlı veri işleme uygulamaları oluşturmasına olanak tanır. Bir JVM ve .NET TCP yerel soketleri üzerinden iletişim kurar. Her bir spout/cıvata, kullanıcı mantığının bir .NET işleminde eklenti olarak çalıştığı bir .NET/Java işlem çiftidir.

SCP'nin üstüne bir veri işleme uygulaması oluşturmak için aşağıdaki adımları izleyin:

1. Kuyruklardan veri çekmek için spout'lar tasarla ve uygulayın.
1. Giriş verilerini işleyen ve veritabanı gibi harici mağazalara kaydeden cıvatalar tasarlayıp uygulayın.
1. Topolojiyi tasarlayın, sonra gönderin ve çalıştırın.

Topoloji tepe tepelerini ve aralarında akan verileri tanımlar. SCP bir topoloji belirtimi alır ve her tepe noktasının tek bir mantıksal düğüm üzerinde çalıştığı bir Fırtına kümesine dağır. Fırtına görev zamanlayıcısı başarısızlık ve ölçekleme ilgilenir.

Bu makalede, SCP ile veri işleme uygulamaları oluşturmak için nasıl yürümek için bazı basit örnekler kullanır.

## <a name="scp-plug-in-interface"></a>SCP eklenti arabirimi

SCP eklentileri bağımsız uygulamalardır. Geliştirme sırasında Visual Studio'da çalıştırılabilir ler ve üretim dağıtımından sonra Fırtına boru hattına bağlanabilirler.

SCP eklentisi yazmak, diğer Windows konsol uyrumu uygulamalarını yazmakla aynıdır. SCP.NET platformu emz/cıvata için bazı arabirimler bildirir. Eklenti kodunuz bu arabirimleri uygular. Bu tasarımın temel amacı, SCP.NET platformunun diğer şeyleri ele almalarına izin verirken iş mantığınıza odaklanmanızı sağlamaktır.

Eklenti kodunuz aşağıdaki arabirimlerden birini uygular. Hangi arabirim, topolojinin işlemsel mi yoksa işlem dışı mı olduğuna ve bileşenin bir emzme mi yoksa cıvata mı olduğuna bağlıdır.

* **ISCPSpout**
* **ISCPBolt**
* **ISCPTxSpout**
* **ISCPBatchBolt**

### <a name="iscpplugin"></a>ISCPPlugin

**ISCPPlugin** birçok eklentiiçin ortak arayüzdür.

```csharp
public interface ISCPPlugin
{
}
```

### <a name="iscpspout"></a>ISCPSpout

**ISCPSpout** işlem dışı bir emzit için arabirimdir.

```csharp
public interface ISCPSpout : ISCPPlugin
{
    void NextTuple(Dictionary<string, Object> parms);
    void Ack(long seqId, Dictionary<string, Object> parms); 
    void Fail(long seqId, Dictionary<string, Object> parms);
}
```

**NextTuple** çağrıldığında, C# kodunuz bir veya daha fazla tuples yarayabilir. Yayan bir şey yoksa, bu yöntem hiçbir şey yaymadan geri dönmelidir.

**NextTuple**, **Ack**ve **Fail** yöntemleri nin tümü, c# işleminin tek bir iş parçacığında sıkı bir döngü içinde çağrılır. Yayılacak tuples olmadığında, **NextTuple** uykusu 10 milisaniye gibi kısa bir süre için uyuyun. Bu uyku, CPU kullanılabilirliğini boşa harcamaya yardımcı olur.

**Ack** ve **Fail** yöntemleri yalnızca bir belirtim dosyası bildirim mekanizmasını etkinleştirdiğinde çağrılır. *SeqId* parametresi kabul edilen veya başarısız olan tuple'ı tanımlar. İşlem dışı bir topolojide bildirim etkinse, aşağıdaki **Emit** işlevi bir dışa doğru kullanılmalıdır:

```csharp
public abstract void Emit(string streamId, List<object> values, long seqId);
```

İşlem dışı bir topoloji bildirimi desteklemiyorsa, **Ack** ve **Fail** boş işlevler olarak bırakılabilir.

Bu işlevlerde *parms* giriş parametresi boş bir sözlük belirtir ve gelecekteki kullanım için ayrılmıştır.

### <a name="iscpbolt"></a>ISCPBolt

**ISCPBolt** işlem dışı bir cıvata için arabirimdir.

```csharp
public interface ISCPBolt : ISCPPlugin
{
void Execute(SCPTuple tuple);
}
```

Yeni bir tuple kullanılabilir olduğunda, **yürütme** işlevi onu işlemek için çağrılır.

### <a name="iscptxspout"></a>ISCPTxSpout

**ISCPTxSpout** bir işlem spout için arabirimdir.

```csharp
public interface ISCPTxSpout : ISCPPlugin
{
    void NextTx(out long seqId, Dictionary<string, Object> parms);  
    void Ack(long seqId, Dictionary<string, Object> parms);         
    void Fail(long seqId, Dictionary<string, Object> parms);        
}
```

İşlem dışı karşılıkları **NextTx**, **Ack**ve **Fail** gibi hepsi de c# işleminin tek bir iş parçacığında sıkı bir döngü içinde çağrılır. Yayılacak tuples olmadığında, **NextTx** 10 milisaniye gibi kısa bir süre için uyku var. Bu uyku, CPU kullanılabilirliğini boşa harcamaya yardımcı olur.

**NextTx** yeni bir işlem başlatmak için *çağrıldığında, seqId* çıkış parametresi hareketi tanımlar. Hareket, **Ack** ve **Fail'de**de kullanılır. **NextTx** yönteminiz Java tarafına veri yatabilir. Veriler, yeniden oynatmayı desteklemek için ZooKeeper'da depolanır. ZooKeeper sınırlı kapasiteye sahip olduğundan, kodunuz yalnızca meta veri yontmalı ve işlem seli içinde toplu veri yatmalıdır.

Storm başarısız bir hareketi otomatik olarak yeniden oynattığı için **Fail** genellikle çağrılmaz. Ancak SCP, bir işlem emdi tarafından yayılan meta verileri denetleyebilirse, meta veriler geçersiz olduğunda **Başarısız** olarak arayabilir.

Bu işlevlerde *parms* giriş parametresi boş bir sözlük belirtir ve gelecekteki kullanım için ayrılmıştır.

### <a name="iscpbatchbolt"></a>ISCPBatchBolt

**ISCPBatchBolt** bir işlem cıvataiçin arabirimdir.

```csharp
public interface ISCPBatchBolt : ISCPPlugin
{
    void Execute(SCPTuple tuple);
    void FinishBatch(Dictionary<string, Object> parms);  
}
```

**Cıvataya** yeni bir tuple geldiğinde Yürüt metodu çağrılır. Bu işlem sona erdiğinde **FinishBatch** yöntemi çağrılır. *Parms* giriş parametresi ileride kullanılmak üzere ayrılmıştır.

Bir işlem topolojisi için **StormTxAttempt** önemli bir sınıftır. İki üyesi vardır: **TxId** ve **AttemptId**. **TxId** üyesi belirli bir hareketi tanımlar. Bir hareket başarısız olursa ve yeniden oynatılırsa birden çok kez denenebilir.

SCP.NET, her **StormTxAttempt** nesnesini işlemek için yeni bir **ISCPBatchBolt** nesnesi oluşturur, tıpkı Storm'un Java'da yaptığı gibi. Bu tasarımın amacı paralel işlem işlemeyi desteklemektir. Bir işlem girişimi tamamlandıktan sonra, ilgili **ISCPBatchBolt** nesnesi yok edilir ve çöp toplanır.

## <a name="object-model"></a>Nesne modeli

SCP.NET ayrıca geliştiricilerin programla programlayabilmek için basit bir anahtar nesne kümesi de sağlar. Nesneler **Bağlam,** **StateStore**ve **SCPRuntime'dır.** Bu bölümde tartışılıyorlar.

### <a name="context"></a>Bağlam

**Bağlam** nesnesi bir uygulamaya çalışan bir ortam sağlar. **ISCPSpout,** **ISCPBolt,** **ISCPTxSpout**veya **ISCPBatchBolt'un** her **ISCPPlugin** örneğinin karşılık gelen bir **Bağlam** örneği vardır. **Bağlam** tarafından sağlanan işlevsellik şu iki bölüme ayrılır:

* Tüm C# işleminde bulunan statik parça
* Yalnızca belirli **Bağlam** örneği için kullanılabilen dinamik parça

### <a name="static-part"></a>Statik parça

```csharp
public static ILogger Logger = null;
public static SCPPluginType pluginType;
public static Config Config { get; set; }
public static TopologyContext TopologyContext { get; set; }  
```

**Logger** nesnesi günlük amacıyla sağlanır.

**PluginType nesnesi** C# işleminin eklenti türünü gösterir. İşlem Java olmadan yerel test modunda çalıştırılırsa, eklenti türü **SCP_NET_LOCAL.**

```csharp
public enum SCPPluginType 
{
    SCP_NET_LOCAL = 0,
    SCP_NET_SPOUT = 1,
    SCP_NET_BOLT = 2,
    SCP_NET_TX_SPOUT = 3,
    SCP_NET_BATCH_BOLT = 4  
    }
```

**Config** özelliği, C# eklentisi başharfe geçtiğinde bunları geçen Java tarafından yapılandırma parametrelerini alır. **Config** parametreleri iki bölüme ayrılır: **stormConf** ve **pluginConf**.

```csharp
public Dictionary<string, Object> stormConf { get; set; }  
public Dictionary<string, Object> pluginConf { get; set; }  
```

**StormConf** parçası Storm tarafından tanımlanan parametrelerdir ve **pluginConf** parçası SCP tarafından tanımlanan parametrelerdir. Bir örneği aşağıda verilmiştir:

```csharp
public class Constants
{
    … …

    // constant string for pluginConf
    public static readonly String NONTRANSACTIONAL_ENABLE_ACK = "nontransactional.ack.enabled";  

    // constant string for stormConf
    public static readonly String STORM_ZOOKEEPER_SERVERS = "storm.zookeeper.servers";
    public static readonly String STORM_ZOOKEEPER_PORT = "storm.zookeeper.port";
}
```

**TopolojiBağlam** türü topoloji bağlamını alır. Birden çok paralel bileşen için en kullanışlıdır. Bir örneği aşağıda verilmiştir:

```csharp
//demo how to get TopologyContext info
if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
{
    Context.Logger.Info("TopologyContext info:");
    TopologyContext topologyContext = Context.TopologyContext;
    Context.Logger.Info("taskId: {0}", topologyContext.GetThisTaskId());
    taskIndex = topologyContext.GetThisTaskIndex();
    Context.Logger.Info("taskIndex: {0}", taskIndex);
    string componentId = topologyContext.GetThisComponentId();
    Context.Logger.Info("componentId: {0}", componentId);
    List<int> componentTasks = topologyContext.GetComponentTasks(componentId);  
    Context.Logger.Info("taskNum: {0}", componentTasks.Count);
}
```

### <a name="dynamic-part"></a>Dinamik parça

Aşağıdaki arabirimler, SCP.NET platformu tarafından oluşturulan ve kodunuza geçirilen belirli bir **Bağlam** örneğiyle ilgilidir:

```csharp
// Declare the Output and Input Stream Schemas

public void DeclareComponentSchema(ComponentStreamSchema schema);

// Emit tuple to default stream.
public abstract void Emit(List<object> values);

// Emit tuple to the specific stream.
public abstract void Emit(string streamId, List<object> values);  
```

Bildirimi destekleyen işlem dışı bir emzme için aşağıdaki yöntem sağlanır:

```csharp
// for nontransactional spout that supports ack
public abstract void Emit(string streamId, List<object> values, long seqId);  
```

Bildirimi destekleyen işlem dışı bir cıvata, aldığı tuple ile açıkça **Ack** veya **Fail'i** aramalıdır. Yeni bir tuple yayan cıvata da tuple'S çapa belirtmelidir. Aşağıdaki yöntemler sağlanmaktadır:

```csharp
public abstract void Emit(string streamId, IEnumerable<SCPTuple> anchors, List<object> values);
public abstract void Ack(SCPTuple tuple);
public abstract void Fail(SCPTuple tuple);
```

### <a name="statestore"></a>StateStore

**StateStore** nesnesi meta veri hizmetleri, monoton sıra oluşturma ve beklemeden koordinasyon sağlar. **StateStore'da**daha üst düzey dağıtılmış eşzamanlılık soyutlamaları oluşturabilirsiniz. Bu soyutlamalar dağıtılmış kilitleri, dağıtılmış kuyrukları, engelleri ve işlem hizmetlerini içerir.

SCP uygulamaları [Apache ZooKeeper'daki](https://zookeeper.apache.org/)bilgileri serihale getirmek için **Durum** nesnesini kullanabilir. Bu yetenek özellikle işlemsel bir topoloji için değerlidir. Bir işlem spout yanıt durdurur ve yeniden başlatılırsa, **State** ZooKeeper gerekli bilgileri almak ve ardışık yeniden başlatabilirsiniz.

**StateStore** nesnesi şu temel yöntemlere sahiptir:

```csharp
/// <summary>
/// Static method to retrieve a state store of the given path and connStr 
/// </summary>
/// <param name="storePath">StateStore path</param>
/// <param name="connStr">StateStore address</param>
/// <returns>Instance of StateStore</returns>
public static StateStore Get(string storePath, string connStr);

/// <summary>
/// Create a new state object in this state store instance
/// </summary>
/// <returns>State from StateStore</returns>
public State Create();

/// <summary>
/// Retrieve all states that were previously uncommitted, excluding all exited states
/// </summary>
/// <returns>Uncommitted states</returns>
public IEnumerable<State> GetUnCommitted();

/// <summary>
/// Get all the states in the StateStore
/// </summary>
/// <returns>All the states</returns>
public IEnumerable<State> States();

/// <summary>
/// Get state or registry object
/// </summary>
/// <param name="info">Registry name (registry only)</param>
/// <typeparam name="T">Type, registry or state</typeparam>
/// <returns>Return registry or state</returns>
public T Get<T>(string info = null);

/// <summary>
/// List all the committed states
/// </summary>
/// <returns>Registries containing the committed state </returns>
public IEnumerable<Registry> Committed();

/// <summary>
/// List all the exited states in the StateStore
/// </summary>
/// <returns>Registries containing the exited states</returns>
public IEnumerable<Registry> Aborted();

/// <summary>
/// Retrieve an existing state object from this state store instance 
/// </summary>
/// <returns>State from StateStore</returns>
/// <typeparam name="T">stateId, id of the State</typeparam>
public State GetState(long stateId)
```

**Durum** nesnesi şu temel yöntemlere sahiptir:

```csharp
/// <summary>
/// Set the status of the state object to commit
/// </summary>
public void Commit(bool simpleMode = true);

/// <summary>
/// Set the status of the state object to exit
/// </summary>
public void Abort();

/// <summary>
/// Put an attribute value under the given key
/// </summary>
/// <param name="key">Key</param>
/// <param name="attribute">State attribute</param>
    public void PutAttribute<T>(string key, T attribute);

/// <summary>
/// Get the attribute value associated with the given key
/// </summary>
/// <param name="key">Key</param>
/// <returns>State attribute</returns>
    public T GetAttribute<T>(string key);
```

**simpleMode** **doğru**ayarlandığında, **Commit** yöntemi ZooKeeper'daki ilgili ZNode'yi siler. Aksi takdirde, yöntem geçerli ZNode'u siler ve\_COMMITTED PATH'e yeni bir düğüm ekler.

### <a name="scpruntime"></a>SCPRuntime

**SCPRuntime** sınıfı aşağıdaki iki yöntemi sağlar:

```csharp
public static void Initialize();

public static void LaunchPlugin(newSCPPlugin createDelegate);  
```

**Initialize** yöntemi SCP çalışma zamanı ortamını başlatmayı sağlar. Bu yöntemde, C# işlemi yapılandırma parametreleri ve topoloji bağlamı almak için Java tarafına bağlanır.

**LaunchPlugin** yöntemi ileti işleme döngüsüne başlar. Bu döngüde, C# eklentisi Java tarafından iletileri alır. Bu iletiler tuples ve kontrol sinyalleri içerir. Eklenti daha sonra, belki de kodunuz tarafından sağlanan arabirim yöntemini çağırarak iletileri işler.

**LaunchPlugin'in** giriş parametresi bir temsilcidir. Yöntem, **ISCPSpout,** **ISCPBolt**, **ISCPTxSpout**veya **ISCPBatchBolt** arabirimini uygulayan bir nesne döndürebilir.

```csharp
public delegate ISCPPlugin newSCPPlugin(Context ctx, Dictionary<string, Object> parms);
```

**ISCPBatchBolt**için, *parms* parametrebir **StormTxAttempt** nesne almak ve girişimi tekrar girişimi olup olmadığını yargılamak için kullanabilirsiniz. Yeniden oynatma denemesi için denetim genellikle commit cıvatada yapılır. HelloWorldTx örneği daha sonra bu makalede bu denetimi gösterir.

SCP eklentileri genellikle iki modda çalıştırılabilir: yerel test modu ve normal mod.

#### <a name="local-test-mode"></a>Yerel test modu

Bu modda, C# kodunuzdaki SCP eklentileri geliştirme aşamasında Visual Studio'da çalıştırılır. **ILocalContext** arabirimini bu modda kullanabilirsiniz. Arabirim, yayılan tuples'i yerel dosyalara seri hale getirmek ve bunları RAM'e geri okumak için yöntemler sağlar.

```csharp
public interface ILocalContext
{
    List<SCPTuple> RecvFromMsgQueue();
    void WriteMsgQueueToFile(string filepath, bool append = false);  
    void ReadFromFileToMsgQueue(string filepath);
}
```

#### <a name="regular-mode"></a>Normal mod

Bu modda, Storm Java işlemi SCP eklentilerini çalıştırır. Aşağıda bir örnek verilmiştir:

```csharp
namespace Scp.App.HelloWorld
{
public class Generator : ISCPSpout
{
    … …
    public static Generator Get(Context ctx, Dictionary<string, Object> parms)
    {
    return new Generator(ctx);
    }
}

class HelloWorld
{
    static void Main(string[] args)
    {
    /* Setting the environment variable here can change the log file name */
    System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "HelloWorld");

    SCPRuntime.Initialize();
    SCPRuntime.LaunchPlugin(new newSCPPlugin(Generator.Get));
    }
}
}
```

## <a name="topology-specification-language"></a>Topoloji Belirtimi dili

SCP Topoloji Belirtimi, SCP topolojilerini tanımlamak ve yapılandırmak için etki alanına özgü bir dildir (DSL). [Storm's Clojure DSL'ye](https://storm.incubator.apache.org/documentation/Clojure-DSL.html) dayanıyor ve SCP tarafından genişletildi.

Topoloji belirtimlerini **runSpec** komutu aracılığıyla yürütülmesi için doğrudan bir Fırtına kümesine gönderebilirsiniz.

SCP.NET işlem topolojilerini tanımlamak için aşağıdaki işlevleri ekledi:

| Yeni işlev | Parametreler | Açıklama |
| --- | --- | --- |
| **tx-topolopy** |*topoloji-adı*<br />*spout-harita*<br />*cıvata-harita* |Topoloji adı, spouts tanım haritası ve cıvata tanım haritası ile bir işlem topolojisi tanımlar. |
| **scp-tx-spout** |*exec-name*<br />*args*<br />*Alanları* |Bir işlem emzisi tanımlar. *İşlev, exec-name* ile belirtilen uygulamayı çalıştırıyor ve *args*kullanır.<br /><br />*Alanlar* parametresi, emzit için çıkış alanlarını belirtir. |
| **scp-tx-toplu cıvata** |*exec-name*<br />*args*<br />*Alanları* |İşlemsel toplu iş cıvatası tanımlar. *İşlev, exec-name* ile belirtilen uygulamayı çalıştırıyor ve *args kullanır.*<br /><br />*Alanlar* parametresi cıvata için çıkış alanlarını belirtir. |
| **scp-tx-commit-cıvata** |*exec-name*<br />*args*<br />*Alanları* |Bir işlem commit bolt tanımlar. *İşlev, exec-name* ile belirtilen uygulamayı çalıştırıyor ve *args*kullanır.<br /><br />*Alanlar* parametresi cıvata için çıkış alanlarını belirtir. |
| **nontx-topoloji** |*topoloji-adı*<br />*spout-harita*<br />*cıvata-harita* |Topoloji adı, spouts tanım haritası ve cıvata tanım haritası ile işlem dışı bir topoloji tanımlar. |
| **scp-spout** |*exec-name*<br />*args*<br />*Alanları*<br />*Parametre* |İşlem dışı bir emzit tanımlar. *İşlev, exec-name* ile belirtilen uygulamayı çalıştırıyor ve *args*kullanır.<br /><br />*Alanlar* parametresi, emzit için çıkış alanlarını belirtir.<br /><br />*Parametreler* parametresi isteğe bağlıdır. "Nontransactional.ack.enabled" gibi parametreleri belirtmek için kullanın. |
| **scp-cıvata** |*exec-name*<br />*args*<br />*Alanları*<br />*Parametre* |İşlem dışı bir cıvata tanımlar. *İşlev, exec-name* ile belirtilen uygulamayı çalıştırıyor ve *args*kullanır.<br /><br />*Alanlar* parametresi cıvata için çıkış alanlarını belirtir<br /><br />*Parametreler* parametresi isteğe bağlıdır. "Nontransactional.ack.enabled" gibi parametreleri belirtmek için kullanın. |

SCP.NET aşağıdaki anahtar kelimeleri tanımlar:

| Anahtar kelime | Açıklama |
| --- | --- |
| **:isim** |Topoloji adı |
| **:topoloji** |Bir önceki tablodaki işlevleri ve yerleşik işlevleri kullanan topoloji |
| **:p** |Her emzme veya cıvata için paralellik ipucu |
| **:config** |Parametreleri yapılandırmak veya varolanları güncelleştirmek mi |
| **:şema** |Derenin şeması |

SCP.NET ayrıca sık kullanılan bu parametreleri tanımlar:

| Parametre | Açıklama |
| --- | --- |
| "plugin.name" |C# eklentisinin .exe dosya adı |
| "plugin.args" |Eklenti bağımsız değişkenleri |
| "output.schema" |Çıkış şeması |
| "nontransactional.ack.enabled" |İşlem dışı bir topoloji için bildirimin etkin olup olmadığı |

**runSpec** komutu bitlerle birlikte dağıtılır. Burada komut kullanımı:

```csharp
.\bin\runSpec.cmd
usage: runSpec [spec-file target-dir [resource-dir] [-cp classpath]]
ex: runSpec examples\HelloWorld\HelloWorld.spec specs examples\HelloWorld\Target
```

*Kaynak-dir* parametresi isteğe bağlıdır. C# uygulamasını takmak istediğinizde belirtin. Belirtilen dizin, uygulama, bağımlılıklar ve yapılandırmaları içerir.

*Sınıf yolu* parametresi de isteğe bağlıdır. Belirtim dosyasında Java emzme veya cıvata varsa Java classpath'i belirtir.

## <a name="miscellaneous-features"></a>Çeşitli özellikler

### <a name="input-and-output-schema-declarations"></a>Giriş ve çıktı şema bildirimleri

C# işlemleriniz tuples yaçabilir. Bunu yapmak için platform tuples'ı **bayt[]** nesnelere serihale eder ve nesneleri Java tarafına aktarır. Fırtına daha sonra bu tuples'ı hedeflere aktarıyor.

Akış aşağı bileşenlerinde, C# işlemleri Java tarafından geri tuples alır ve platformun orijinal türlerine dönüştürür. Tüm bu işlemler platform tarafından gizlidir.

Serileştirme ve deserialization desteklemek için, kod giriş ve çıkış şemasını bildirmek gerekir. Şema sözlük olarak tanımlanır. Akış kimliği sözlük anahtarıdır. Anahtar değeri sütun türleridir. Bir bileşen birden çok akışı bildirebilir.

```csharp
public class ComponentStreamSchema
{
    public Dictionary<string, List<Type>> InputStreamSchema { get; set; }
    public Dictionary<string, List<Type>> OutputStreamSchema { get; set; }
    public ComponentStreamSchema(Dictionary<string, List<Type>> input, Dictionary<string, List<Type>> output)
    {
        InputStreamSchema = input;
        OutputStreamSchema = output;
    }
}
```

Bağlam **nesnesine** aşağıdaki işlev eklenir:

```csharp
public void DeclareComponentSchema(ComponentStreamSchema schema)
```

Geliştiriciler, yayılan tuples bir akış için tanımlanan şema itaat sağlamak gerekir. Aksi takdirde, sistem bir çalışma zamanı özel durum atar.

### <a name="multistream-support"></a>Çoklu akış desteği

SCP, kodunuzu aynı anda birden çok farklı akışa yatsın veya alır. **Bağlam** nesnesi, **Emit** yönteminin isteğe bağlı akış kimliği parametresi olarak bu desteği yansıtır.

SCP.NET **Bağlam** nesnesinde iki yöntem eklendi. Belirli akışlara bir veya daha fazla tuples yayırlar. *StreamId* parametresi bir dizedir. Değeri hem C# kodunda hem de topoloji tanım belirtiminde aynı olmalıdır.

```csharp
/* Emit tuple to the specific stream. */
public abstract void Emit(string streamId, List<object> values);

/* for nontransactional spout only */
public abstract void Emit(string streamId, List<object> values, long seqId);
```

Var olmayan bir akışa yayan çalışma zamanı özel durumlara neden olur.

### <a name="fields-grouping"></a>Alanlar gruplandırma

Storm'da gruplandırma yapan yerleşik alanlar SCP.NET'da düzgün çalışmıyor. Java proxy tarafında, tüm alanların veri türü aslında **bayt[]**. Alanlar gruplandırma, gruplandırma yapmak için **bayt[]** nesnenin karma kodunu kullanır. Karma kod, RAM'deki bu nesnenin adresidir. Bu nedenle gruplandırma, aynı içeriği paylaşan ancak aynı adresi paylaşmayan çok bayt nesneler için yanlış olacaktır.

SCP.NET özelleştirilmiş bir gruplandırma yöntemi ekler ve gruplandırma yapmak için **bayt[]** nesnesinin içeriğini kullanır. Belirtim dosyasında sözdizimi aşağıdaki örneğe benzer:

```csharp
(bolt-spec
    {
        "spout_test" (scp-field-group :non-tx [0,1])
    }
    …
)
```

Önceki belirtim dosyasında:

* `scp-field-group`gruplandırmanın SCP tarafından uygulanan özelleştirilmiş bir alan grubu olduğunu belirtir.
* `:tx`veya `:non-tx` topolojinin işlemsel olup olmadığını belirtir. Başlangıç dizini işlemsel ve işlem dışı topolojiler arasında farklı olduğundan bu bilgilere ihtiyacınız vardır.
* `[0,1]`sıfırla başlayan karma alan kimlikleri kümesini belirtir.

### <a name="hybrid-topology"></a>Hibrit topoloji

Yerli Fırtına kodu Java'da yazılır. SCP.NET iş mantığı işlemek için C # kodu yazmak için izin Storm geliştirdi. Ama SCP.NET aynı zamanda sadece C # spouts / cıvata değil, aynı zamanda Java spouts / cıvata içeren hibrid topolojileri destekler.

### <a name="specify-java-spoutbolt-in-a-specification-file"></a>Belirtim dosyasında Java spout/bolt belirtin

Java spouts ve cıvata belirtmek için bir belirtim dosyasında **scp-spout** ve **scp-bolt** kullanabilirsiniz. Bir örneği aşağıda verilmiştir:

```csharp
(spout-spec 
  (microsoft.scp.example.HybridTopology.Generator.)
  :p 1)
```

Burada `microsoft.scp.example.HybridTopology.Generator` Java spout sınıfının adıdır.

### <a name="specify-the-java-classpath-in-a-runspec-command"></a>runSpec komutundaki Java classpath'ini belirtin

Java spouts veya cıvata içeren topoloji göndermek istiyorsanız, ilk JAR dosyaları üretmek için bunları derlemek. Ardından topoloji gönderirken JAR dosyalarını içeren java sınıf yolunu belirtin. Bir örneği aşağıda verilmiştir:

```csharp
bin\runSpec.cmd examples\HybridTopology\HybridTopology.spec specs examples\HybridTopology\net\Target -cp examples\HybridTopology\java\target\*
```

Burada, `examples\HybridTopology\java\target\` Java spout/bolt JAR dosyasını içeren klasördür.

### <a name="serialization-and-deserialization-between-java-and-c"></a>Java ve C arasında serileştirme ve deserialization #

Bir SCP bileşeni Java tarafını ve C# tarafını içerir. Yerel Java spouts / cıvata ile etkileşim için, serileştirme ve deserialization Java tarafı ve C # tarafı arasında meydana gelmelidir, aşağıdaki grafikte gösterildiği gibi:

![SCP bileşenine gönderen Java bileşeninin diyagramı, daha sonra farklı bir Java bileşenine gönderir](./media/apache-storm-scp-programming-guide/java-compent-sending-to-scp-component-sending-to-java-component.png)

#### <a name="serialization-in-the-java-side-and-deserialization-in-the-c-side"></a>Java tarafında serileştirme ve C# tarafında deserialization

Önce Java tarafında serileştirme ve C# tarafında deserialization için varsayılan uygulama sağlar.

Bir belirtim dosyasında Java tarafının serileştirme yöntemini belirtin.

```csharp
(scp-bolt
    {
        "plugin.name" "HybridTopology.exe"
        "plugin.args" ["displayer"]
        "output.schema" {}
        "customized.java.serializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer"]
    })
```

C# kodunuzda C# tarafının deserialization yöntemini belirtin.

```csharp
Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
inputSchema.Add("default", new List<Type>() { typeof(Person) });
this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, null));
this.ctx.DeclareCustomizedDeserializer(new CustomizedInteropJSONDeserializer());
```  

Veri türü çok karmaşık değilse, bu varsayılan uygulama çoğu servis taleplerini ele almalıdır. Kendi uygulamanızı takabileceğiniz durumlar şunlardır:

* Veri türünüz varsayılan uygulama için çok karmaşıktır.
* Varsayılan uygulamanızın performansı gereksinimlerinizi karşılamıyor.

Java tarafındaki serileştirme arabirimi şu şekilde tanımlanır:

```csharp
public interface ICustomizedInteropJavaSerializer {
    public void prepare(String[] args);
    public List<ByteBuffer> serialize(List<Object> objectList);
}
```

C# tarafındaki deserialization arabirimi şu şekilde tanımlanır:

```csharp
public interface ICustomizedInteropCSharpDeserializer
{
    List<Object> Deserialize(List<byte[]> dataList, List<Type> targetTypes);
}
```

#### <a name="serialization-in-the-c-side-and-deserialization-in-the-java-side"></a>C# tarafında serileştirme ve Java tarafında deserialization

C# kodunuzda C# tarafının serileştirme yöntemini belirtin.

```csharp
this.ctx.DeclareCustomizedSerializer(new CustomizedInteropJSONSerializer()); 
```

Bir belirtim dosyasında Java tarafının deserialization yöntemini belirtin.

```csharp
(scp-spout
   {
     "plugin.name" "HybridTopology.exe"
     "plugin.args" ["generator"]
     "output.schema" {"default" ["person"]}
     "customized.java.deserializer" ["microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer" "microsoft.scp.example.HybridTopology.Person"]
   }
)
```

Burada, `"microsoft.scp.storm.multilang.CustomizedInteropJSONDeserializer"` deserializer adıdır ve `"microsoft.scp.example.HybridTopology.Person"` veri deserialized hedef sınıfıdır.

Ayrıca bir C# serializer ve Java deserializer kendi uygulama takabilirsiniz.

Bu kod C# serializer için arabirimdir:

```csharp
public interface ICustomizedInteropCSharpSerializer
{
    List<byte[]> Serialize(List<object> dataList);
}
```

Bu kod Java deserializer için arabirimdir:

```csharp
public interface ICustomizedInteropJavaDeserializer {
    public void prepare(String[] targetClassNames);
    public List<Object> Deserialize(List<ByteBuffer> dataList);
}
```

## <a name="scp-host-mode"></a>SCP ana bilgisayar modu

Bu modda, kodunuzu DLL olarak derleyebilir ve topoloji göndermek için SCPCP tarafından sağlanan SCPHost.exe'yi kullanabilirsiniz. Belirtim dosyası bu koda benzer:

```csharp
(scp-spout
  {
    "plugin.name" "SCPHost.exe"
    "plugin.args" ["HelloWorld.dll" "Scp.App.HelloWorld.Generator" "Get"]
    "output.schema" {"default" ["sentence"]}
  })
```

Burada, `"plugin.name"` SCP `"SCPHost.exe"`SDK tarafından sağlanan olarak belirtilir. SCPHost.exe aşağıdaki sırada üç parametre kabul eder:

1. Bu örnekte olan `"HelloWorld.dll"` DLL adı.
1. Bu `"Scp.App.HelloWorld.Generator"` örnekte olan sınıf adı.
1. **ISCPPlugin**örneğini almak için çağrılabilir ortak statik yöntem, adı.

Ana bilgisayar modunda, kodunuzu SCP platformu tarafından çağrılması için DLL olarak derleyin. Platform daha sonra tüm işleme mantığının tam denetimini alabildiği için, SCP ana bilgisayar modunda topoloji göndermenizi öneririz. Bunu yapmak geliştirme deneyimini kolaylaştırır. Ayrıca, daha sonraki sürümler için daha fazla esneklik ve daha iyi geri uyumluluk sağlar.

## <a name="scp-programming-examples"></a>SCP programlama örnekleri

### <a name="helloworld"></a>Helloworld

Aşağıdaki basit HelloWorld örneği SCP.NET bir tat gösterir. Bu bir emzme denilen bir işlem **generator** dışı topoloji ve **splitter** ve **sayaç**denilen iki cıvata kullanır. **Jeneratör** emzme rasgele cümleler oluşturur ve **splitter**için bu cümleler yontmak . **Bölünene** kadar cıvata cümleleri sözcüklere böler ve bu sözcükleri **karşı** cıvataya yayar. **Sayaç** cıvatası, her sözcüğün oluşumunu kaydetmek için bir sözlük kullanır.

Bu örnekte iki belirtim dosyası vardır:\_HelloWorld.spec ve HelloWorld EnableAck.spec. C# kodu, nesneyi Java tarafından alarak `pluginConf` bildirimin etkin olup olmadığını bulabilir.

```csharp
/* demo how to get pluginConf info */
if (Context.Config.pluginConf.ContainsKey(Constants.NONTRANSACTIONAL_ENABLE_ACK))
{
    enableAck = (bool)(Context.Config.pluginConf[Constants.NONTRANSACTIONAL_ENABLE_ACK]);
}
Context.Logger.Info("enableAck: {0}", enableAck);
```

Bildirim emzmede etkinleştirilirse, sözlük onaylanmamış tuples önbelleğe gelir. `Fail` Çağrıldığında, başarısız olan tuple yeniden oynandı.

```csharp
public void Fail(long seqId, Dictionary<string, Object> parms)
{
    Context.Logger.Info("Fail, seqId: {0}", seqId);
    if (cachedTuples.ContainsKey(seqId))
    {
        /* get the cached tuple */
        string sentence = cachedTuples[seqId];

        /* replay the failed tuple */
        Context.Logger.Info("Re-Emit: {0}, seqId: {1}", sentence, seqId);
        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), seqId);
    }
    else
    {
        Context.Logger.Warn("Fail(), can't find cached tuple for seqId {0}!", seqId);
    }
}
```

### <a name="helloworldtx"></a>MerhabaWorldTx

Aşağıdaki HelloWorldTx örneği, işlemsel topolojinin nasıl uygulanacağını göstermektedir. Örnekte **jeneratör**adı verilen bir spout, **kısmi sayım**adı verilen bir toplu cıvata ve **count-sum**adı verilen bir cıvata vardır. DataSource0.txt, DataSource1.txt ve DataSource2.txt olmak üzere üç tane varolan metin dosyası vardır.

Her işlemde, **jeneratör** emzme rasgele varolan üç dosyadan iki dosya seçer ve **kısmi sayı** cıvata iki dosya adları yalar. **Kısmi sayım** cıvatası:

1. Alınan tuple bir dosya adı alır.
1. İlgili dosyayı açar.
1. Dosyadaki sözcük sayısını sayar.
1. **Sayım-toplam** cıvatasına sayı sözcüğü yayır.

**Sayım toplamı** cıvatası toplam sayımı özetler.

Tam olarak bir kez semantik elde etmek için, **sayım toplamı** taahhüt cıvata bir yeniden bir işlem olup olmadığını yargılamak gerekir. Bu örnekte, aşağıdaki statik üye değişkenvardır:

```csharp
public static long lastCommittedTxId = -1; 
```

Bir **ISCPBatchBolt** örneği oluşturulduğunda, giriş parametrelerinden nesnenin `txAttempt` değerini alır.

```csharp
public static CountSum Get(Context ctx, Dictionary<string, Object> parms)
{
    /* for transactional topology, we can get txAttempt from the input parms */
    if (parms.ContainsKey(Constants.STORM_TX_ATTEMPT))
    {
        StormTxAttempt txAttempt = (StormTxAttempt)parms[Constants.STORM_TX_ATTEMPT];
        return new CountSum(ctx, txAttempt);
    }
    else
    {
        throw new Exception("null txAttempt");
    }
}
```

`FinishBatch` Çağrıldığında, `lastCommittedTxId` yeniden oynatılamamış sayılsa güncelleştirilir.

```csharp
public void FinishBatch(Dictionary<string, Object> parms)
{
    /* judge whether it is a replayed transaction */
    bool replay = (this.txAttempt.TxId <= lastCommittedTxId);

    if (!replay)
    {
        /* If it is not replayed, update the totalCount and lastCommittedTxId value */
        totalCount = totalCount + this.count;
        lastCommittedTxId = this.txAttempt.TxId;
    }
    … …
}
```

### <a name="hybridtopology"></a>Hibrid Topoloji

Bu topoloji bir Java emzisi ve C# cıvataiçerir. SCP platformu tarafından sağlanan varsayılan serileştirme ve deserialization uygulamasını kullanır. Belirtim dosyası ayrıntıları için HybridTopology klasörü örneklerinde\\HybridTopology.spec dosyasına bakın. Ayrıca Java classpath belirtmek için nasıl GönderTopology.bat bakın.

### <a name="scphostdemo"></a>SCPHostDemo

Bu örnek özünde HelloWorld ile aynıdır. Tek fark, kodunuzu bir DLL olarak derlenir ve topoloji SCPHost.exe kullanılarak gönderilir. Daha ayrıntılı bir açıklama için SCP ana bilgisayar modu bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

SCP kullanılarak oluşturulan Apache Storm topolojisi örnekleri için aşağıdaki makalelere bakın:

* [Visual Studio'yu kullanarak HDInsight'ta Apache Storm için C# topolojileri geliştirin](apache-storm-develop-csharp-visual-studio-topology.md)
* [HDInsight'ta Apache Storm ile Azure Etkinlik Hub'larından etkinlikleri işleme](apache-storm-develop-csharp-event-hub-topology.md)
* [HDInsight'ta Apache Storm kullanarak Olay Hub'larından gelen araç sensör verilerini işleme](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/IotExample)
* [Azure Etkinlik Hub'larından Apache HBase'e ayıklama, dönüştürme ve yükleme (ETL)](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample)
