---
title: Bir istemci uygulaması kodlama
titleSuffix: Azure Digital Twins
description: .NET (C#) SDK 'SıNı kullanarak bir istemci uygulaması için en düşük kodu yazma öğreticisi.
author: cschormann
ms.author: cschorm
ms.date: 05/05/2020
ms.topic: tutorial
ms.service: digital-twins
ms.openlocfilehash: 6000bffb80c83ffce58605bcc7e5bfaf404329da
ms.sourcegitcommit: 25bb515efe62bfb8a8377293b56c3163f46122bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87986700"
---
# <a name="coding-with-the-azure-digital-twins-apis"></a>Azure dijital TWINS API 'Leri ile kodlama

Azure dijital TWINS ile çalışan geliştiriciler, Azure dijital TWINS hizmeti örneğiyle etkileşim kurmak üzere bir istemci uygulaması yazmak için yaygındır. Bu geliştirici odaklı öğreticide, [.net Için Azure IoT Digital ikizi istemci kitaplığı (C#)](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core)kullanılarak Azure dijital TWINS hizmetine karşı programlama hakkında bir giriş sunulmaktadır. Bir C# Konsolu istemci uygulaması adımını sıfırdan başlayarak adım adım yazma konusunda size kılavuzluk eder.

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici, kurulum ve proje çalışması için komut satırını kullanır. Bu nedenle, alýþtýrmalar üzerinde gezinmek için herhangi bir kod düzenleyicisini kullanabilirsiniz.

Başlamak için gerekenler:
* Herhangi bir kod Düzenleyicisi
* Geliştirme makinenizde **.NET Core 3,1** . Birden çok platform için .NET Core SDK bu sürümünü [.NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1)' i indirin.

[!INCLUDE [Azure Digital Twins tutorials: instance prereq](../../includes/digital-twins-tutorial-prereq-instance.md)]

## <a name="set-up-project"></a>Projeyi ayarla

Azure dijital TWINS örneğiniz ile çalışmaya hazırsanız, istemci uygulama projesini ayarlamaya başlayın. 

Makinenizde bir komut istemi veya başka bir konsol penceresi açın ve bu öğreticide işlerinizi depolamak istediğiniz boş bir proje dizini oluşturun. Dizine istediğiniz her şeyi adlandırın (örneğin, *Digitaltwınscodeöğreticisi*).

Yeni dizine gidin.

Proje dizininde bir kez, boş bir .NET konsol uygulaması projesi oluşturun. Konsol için en az C# projesi oluşturmak üzere komut penceresinde aşağıdaki komutu çalıştırın:

```cmd/sh
dotnet new console
```

Bu, kodunuzun büyük bir kısmını yazacağınız bir *program.cs* dahil olmak üzere, dizininiz içinde birkaç dosya oluşturur.

Ardından, Azure dijital TWINS ile çalışmak için gereken iki bağımlılığı ekleyin:

```cmd/sh
dotnet add package Azure.DigitalTwins.Core --version 1.0.0-preview.3
dotnet add package Azure.identity
```

İlk bağımlılık, [.net Için Azure IoT Digital ikizi istemci kitaplığı](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core)' dır. İkinci bağımlılık, Azure 'da kimlik doğrulamaya yardımcı olacak araçlar sağlar.

Öğreticinin tamamında kullanmaya devam edebileceksiniz, komut penceresini açık tutun.

## <a name="get-started-with-project-code"></a>Proje kodu ile çalışmaya başlama

Bu bölümde, Azure dijital TWINS ile çalışmak üzere yeni uygulama projeniz için kod yazmaya başlayacaksınız. Kapsanan eylemler şunları içerir:
* Hizmette kimlik doğrulaması yapılıyor
* Model karşıya yükleme
* Yakalama hataları
* Dijital TWINS oluşturma
* İlişki oluşturma
* Dijital TWINS sorgulama

Öğreticinin sonundaki kodun tamamını gösteren bir bölüm de vardır. Bunu, programınızı hareket halindeyken denetlemek için bir başvuru olarak kullanabilirsiniz.

Başlamak için, *program.cs* dosyasını herhangi bir kod düzenleyicisinde açın. Şunun gibi görünen minimal bir kod şablonu görürsünüz:

```csharp
using System;

namespace DigitalTwinsCodeTutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

İlk olarak, `using` gerekli bağımlılıkları çekmek için kodun üst kısmına bazı satırlar ekleyin.

```csharp
using Azure.DigitalTwins.Core;
using Azure.Identity;
```

Daha sonra, bazı işlevleri doldurmanız için bu dosyaya kod ekleyeceksiniz. 

### <a name="authenticate-against-the-service"></a>Hizmette kimlik doğrulama

Uygulamanızın yapması gereken ilk şey, Azure dijital TWINS hizmetinde kimlik doğrulaması gerçekleştirir. Ardından, SDK işlevlerine erişmek için bir hizmet istemci sınıfı oluşturabilirsiniz.

Kimlik doğrulamak için üç parça bilgi gerekir:
* Aboneliğiniz için *Dizin (kiracı) kimliği*
* Daha önce Azure Digital TWINS örneğini ayarlarken oluşturulan *uygulama (istemci) kimliği*
* Azure dijital TWINS örneğinizin *ana bilgisayar adı*

>[!TIP]
> *Dizin (kiracı) kimliğinizi*bilmiyorsanız bu komutu [Azure Cloud Shell](https://shell.azure.com)' de çalıştırarak edinebilirsiniz:
> 
> ```azurecli-interactive
> az account show --query tenantId
> ```

*Program.cs*' de, aşağıdaki kodu "Hello, World!" altına yapıştırın yöntemdeki çıktı satırı `Main` . Değerini `adtInstanceUrl` Azure dijital TWINS örneğiniz *ana bilgisayar adı*, `clientId` *Uygulama Kimliğiniz*ve `tenantId` *Dizin kimliğiniz*olarak ayarlayın.

```csharp
string clientId = "<your-application-ID>";
string tenantId = "<your-directory-ID>";
string adtInstanceUrl = "https://<your-Azure-Digital-Twins-instance-hostName>";
var credentials = new InteractiveBrowserCredential(tenantId, clientId);
DigitalTwinsClient client = new DigitalTwinsClient(new Uri(adtInstanceUrl), credentials);
Console.WriteLine($"Service client created – ready to go");
```

Dosyayı kaydedin. 

Bu örneğin etkileşimli bir tarayıcı kimlik bilgisi kullandığını unutmayın:
```csharp
var credentials = new InteractiveBrowserCredential(tenantId, clientId);
```

Bu kimlik bilgisi türü tarayıcı penceresinin açılmasını sağlar ve Azure kimlik bilgilerinizi vermenizi ister. 

>[!NOTE]
> Diğer kimlik bilgileri türleri hakkında daha fazla bilgi için [Microsoft Identity platform kimlik doğrulama kitaplıkları](../active-directory/develop/reference-v2-libraries.md)belgelerine bakın.

Komut pencerenizde şu komutla kodu çalıştırın: 

```cmd/sh
dotnet run
```

Bu işlem, ilk çalıştırmada bağımlılıkları geri yükler ve ardından programı yürütür. 
* Bir hata oluşursa, program *oluşturulan hizmet istemcisini Yazdır-başlamaya hazırlanın*.
* Bu projede henüz herhangi bir hata işleme olmadığından, bir şey yanlış olursa kod tarafından oluşturulan bir özel durum görürsünüz.

### <a name="upload-a-model"></a>Bir modeli karşıya yükleyin

Azure dijital TWINS 'in hiç iç etki alanı sözlüğü yok. Ortamınızdaki Azure dijital TWINS 'de temsil ettiğiniz öğelerin türleri, **modeller**kullanılarak sizin tarafınızdan tanımlanır. [Modeller](concepts-models.md) , nesne odaklı programlama dillerinde sınıflarla benzerdir; Bunlar, daha sonra izlemek ve örneklendirilecek [dijital TWINS](concepts-twins-graph.md) için Kullanıcı tanımlı şablonlar sağlar. Bunlar, **dijital TWINS tanım dili (DTDL)** adlı JSON benzeri bir dilde yazılır.

Azure dijital TWINS çözümü oluşturmanın ilk adımı, bir DTDL dosyasında en az bir model tanımlıyor.

Projenizi oluşturduğunuz dizinde, *üzerindeSampleModel.js*adlı yeni bir *. JSON* dosyası oluşturun. Aşağıdaki dosya gövdesine yapıştırın: 

```json
{
  "@id": "dtmi:com:contoso:SampleModel;1",
  "@type": "Interface",
  "displayName": "SampleModel",
  "contents": [
    {
      "@type": "Relationship",
      "name": "contains"
    },
    {
      "@type": "Property",
      "name": "data",
      "schema": "string"
    }
  ],
  "@context": "dtmi:dtdl:context;2"
}
```

> [!TIP]
> Bu öğretici için Visual Studio kullanıyorsanız, yeni oluşturulan JSON dosyasını seçmek ve Özellik denetçisindeki *Çıkış Dizinine Kopyala* özelliğini, *daha yeniyse* veya *her zaman*kopyalamak üzere ayarlamak isteyebilirsiniz. Bu, öğreticinin geri kalanı sırasında program 'yu **F5** ile çalıştırdığınızda, Visual Studio 'nun varsayılan yol ile json dosyasını bulmasını sağlayacaktır.

> [!TIP] 
> DTDL 'nin geçerli olduğundan emin olmak için model belgelerini denetlemek için kullanabileceğiniz dilden bağımsız bir [Dtdl doğrulayıcısı örneği](https://docs.microsoft.com/samples/azure-samples/dtdl-validator/dtdl-validator) vardır. Bu, [*nasıl yapılır: modellerinizi ayrıştırma ve doğrulama*](how-to-parse-models.md)hakkında daha fazla bilgi edinmek için dtdl ayrıştırıcısı kitaplığı üzerine kurulmuştur.

Daha sonra, yeni oluşturduğunuz modeli Azure dijital TWINS örneğinize yüklemek için *program.cs* 'e daha fazla kod ekleyin.

İlk olarak, `using` dosyanın en üstüne birkaç deyim ekleyin:

```csharp
using System.Threading.Tasks;
using System.IO;
using System.Collections.Generic;
using Azure;
```

Daha sonra, `Main` yöntem imzasını zaman uyumsuz yürütmeye izin verecek şekilde değiştirerek C# hizmet SDK 'sindeki zaman uyumsuz yöntemleri kullanmaya hazırlanın. 

```csharp
static async Task Main(string[] args)
```

> [!NOTE]
> `async`SDK, tüm çağrıların zaman uyumlu sürümlerini de sağladığından, kullanılması kesinlikle gerekli değildir. Bu öğreticiyi kullanma yöntemleri `async` .

Daha sonra, Azure dijital TWINS hizmetiyle etkileşim kuran kodun ilk biti gelir. Bu kod, diskinizde oluşturduğunuz DTDL dosyasını yükler ve Azure dijital TWINS hizmet örneğinize yükler. 

Daha önce eklediğiniz yetkilendirme kodu altına aşağıdaki kodu yapıştırın.

```csharp
Console.WriteLine();
Console.WriteLine($"Upload a model");
var typeList = new List<string>();
string dtdl = File.ReadAllText("SampleModel.json");
typeList.Add(dtdl);
// Upload the model to the service
await client.CreateModelsAsync(typeList);
```

Komut pencerenizde, programı şu komutla çalıştırın: 

```cmd/sh
dotnet run
```
"Bir modeli karşıya yükle" çıktıda yazdırılacaktır, ancak modellerin başarıyla yüklenip yüklenmediğini göstermek için henüz çıktı yok.

Modellerin gerçekten başarıyla karşıya yüklenip yüklenmediğini belirten bir yazdırma ekstresi eklemek için önceki bölümden sonra aşağıdaki kodu ekleyin:

```csharp
// Read a list of models back from the service
AsyncPageable<ModelData> modelDataList = client.GetModelsAsync();
await foreach (ModelData md in modelDataList)
{
    Console.WriteLine($"Type name: {md.DisplayName}: {md.Id}");
}
```

Bu yeni kodu test etmek için programı yeniden çalıştırmadan önce, programı son kez çalıştırdığınızda modelinizi zaten karşıya yüklediğinize geri çekin. Azure dijital TWINS aynı modeli iki kez karşıya yükleyemeyecek, bu nedenle programı yeniden çalıştırdığınızda bir özel durum görmeniz beklenir.

Şimdi, komut pencerenizde bu komutla programı yeniden çalıştırın:

```cmd/sh
dotnet run
```

Program bir özel durum oluşturması gerekir. Zaten karşıya yüklenmiş bir modeli karşıya yüklemeye çalıştığınızda hizmet, REST API aracılığıyla bir "Hatalı istek" hatası döndürür. Sonuç olarak, Azure Digital TWINS istemci SDK 'Sı, başarılı dışında her hizmet dönüş kodu için bir özel durum oluşturur. 

Sonraki bölümde, bu gibi özel durumlar ve kodunuzda nasıl ele alınacağını görülüyor.

### <a name="catch-errors"></a>Hataları yakala

Programın çökmesini önlemek için, model karşıya yükleme kodunun etrafına özel durum kodu ekleyebilirsiniz. Mevcut istemci çağrısını `client.CreateModelsAsync` bir Try/Catch işleyicisine kaydırın, örneğin:

```csharp
try {
    await client.CreateModelsAsync(typeList);
} catch (RequestFailedException rex) {
    Console.WriteLine($"Load model: {rex.Status}:{rex.Message}");
}
```

Programı `dotnet run` komut pencerenize Şimdi çalıştırırsanız, bir hata kodu geri almanızı göreceksiniz. Çıktı şuna benzer:

```cmd/sh
Hello World!
Service client created - ready to go

Upload a model
Load model: 409:Service request failed.
Status: 409 (Conflict)

Content:
{"error":{"code":"DocumentAlreadyExists","message":"A document with same identifier already exists.","details":[]}}

Headers:
api-supported-versions: REDACTED
Date: Tue, 05 May 2020 01:57:51 GMT
Content-Length: 115
Content-Type: application/json; charset=utf-8

Type name: : dtmi:com:contoso:SampleModel;1
```

Bu noktadan itibaren öğreticide, try/catch işleyicilerindeki hizmet yöntemlerine yapılan tüm çağrılar kaydırılır.

### <a name="create-digital-twins"></a>Dijital TWINS oluşturma

Azure dijital TWINS 'e bir model yüklediğinize göre, bu model tanımını **dijital TWINS**oluşturmak için kullanabilirsiniz. [Dijital TWINS](concepts-twins-graph.md) , bir modelin örnekleridir ve iş ortamınızdaki varlıkları, bir gruptaki sensörlerden, binadaki odaların veya bir otomobilde ışıkların bulunduğu şeyleri temsil eder. Bu bölümde, daha önce karşıya yüklediğiniz modele göre birkaç dijital TWINS oluşturulur.

`using`İçinde yerleşik .net JSON serileştiricisi gerekecektir, en üst kısımdaki yeni bir ifade ekleyin `System.Text.Json` :

```csharp
using System.Text.Json;
using Azure.DigitalTwins.Core.Serialization;
```

Ardından, `Main` Bu modele göre üç dijital TWINS oluşturmak ve başlatmak için yönteminin sonuna aşağıdaki kodu ekleyin.

```csharp
// Initialize twin data
BasicDigitalTwin twinData = new BasicDigitalTwin();
twinData.Metadata.ModelId = "dtmi:com:contoso:SampleModel;1";
twinData.CustomProperties.Add("data", $"Hello World!");

string prefix="sampleTwin-";
for(int i=0; i<3; i++) {
    try {
        twinData.Id = $"{prefix}{i}";
        await client.CreateDigitalTwinAsync($"{prefix}{i}", JsonSerializer.Serialize(twinData));
        Console.WriteLine($"Created twin: {prefix}{i}");
    } catch(RequestFailedException rex) {
        Console.WriteLine($"Create twin error: {rex.Status}:{rex.Message}");  
    }
}
```

Komut pencerenizde, programını ile çalıştırın `dotnet run` . Ardından, programı yeniden çalıştırmak için tekrarlayın. 

TWINS, ilk çalıştırmadan sonra zaten mevcut olsa bile, ikinci kez oluşturulan bir hata olmadığını unutmayın. Model oluşturma işleminden farklı olarak, ikizi oluşturma, *upsert* semantik bir *PUT* çağrısı olan Rest düzeyinde olur. Bu, bir ikizi zaten varsa, yeniden oluşturmaya çalışılması durumunda yalnızca bunun yerine geçecek olması anlamına gelir. Hata gerekli değil.

### <a name="create-relationships"></a>İlişki oluştur

Daha sonra, oluşturduğunuz TWINS arasında **ilişkiler** oluşturarak bunları bir **ikizi grafiğine**bağlayabilirsiniz. [İkizi grafikleri](concepts-twins-graph.md) , tüm ortamınızı temsil etmek için kullanılır.

İlişkiler oluşturabilmek için `using` SDK 'daki ilişki temel türü için bir ifade ekleyin: zaten eklendiyse bunu atlayın.
```csharp
using Azure.DigitalTwins.Core.Serialization;
```

Ardından, sınıfının altına yeni bir static yöntemi ekleyin `Program` `Main` :
```csharp
public async static Task CreateRelationship(DigitalTwinsClient client, string srcId, string targetId)
{
    var relationship = new BasicRelationship
    {
        TargetId = targetId,
        Name = "contains"
    };

    try
    {
        string relId = $"{srcId}-contains->{targetId}";
        await client.CreateRelationshipAsync(srcId, relId, JsonSerializer.Serialize(relationship));
        Console.WriteLine("Created relationship successfully");
    }
    catch (RequestFailedException rex) {
        Console.WriteLine($"Create relationship error: {rex.Status}:{rex.Message}");
    }
}
```

Daha sonra, `Main` kodu çağırmak için yönteminin sonuna aşağıdaki kodu ekleyin `CreateRelationship` :
```csharp
// Connect the twins with relationships
await CreateRelationship(client, "sampleTwin-0", "sampleTwin-1");
await CreateRelationship(client, "sampleTwin-0", "sampleTwin-2");
```

Komut pencerenizde, programını ile çalıştırın `dotnet run` .

Azure dijital TWINS 'in aynı KIMLIĞE sahip olan bir ilişki oluşturmanıza izin vermediğini unutmayın. programı birden çok kez çalıştırırsanız, ilişki oluşturma konusunda özel durumlar görürsünüz. Bu kod özel durumları yakalar ve bunları yoksayar. 

### <a name="list-relationships"></a>Liste ilişkileri

Ekleyeceğiniz bir sonraki kod, oluşturduğunuz ilişkilerin listesini görmenizi sağlar.

`Program` sınıfına aşağıdaki yeni yöntemi ekleyin:

```csharp
public async static Task ListRelationships(DigitalTwinsClient client, string srcId)
{
    try {
        AsyncPageable<string> results = client.GetRelationshipsAsync(srcId);
        Console.WriteLine($"Twin {srcId} is connected to:");
        await foreach (string rel in results)
        {
            var brel = JsonSerializer.Deserialize<BasicRelationship>(rel);
            Console.WriteLine($" -{brel.Name}->{brel.TargetId}");
        }
    } catch (RequestFailedException rex) {
        Console.WriteLine($"Relationship retrieval error: {rex.Status}:{rex.Message}");   
    }
}
```

Daha sonra, `Main` kodu çağırmak için yönteminin sonuna aşağıdaki kodu ekleyin `ListRelationships` :

```csharp
//List the relationships
await ListRelationships(client, "sampleTwin-0");
```

Komut pencerenizde, programını ile çalıştırın `dotnet run` . Oluşturduğunuz tüm ilişkilerin bir listesini görmeniz gerekir.

### <a name="query-digital-twins"></a>Dijital TWINS sorgula

Azure dijital TWINS 'in ana özelliği, ortamınız hakkında soruları yanıtlamak için ikizi grafınızı kolayca ve verimli bir şekilde [sorgulayabilir](concepts-query-language.md) .

Bu öğreticide eklenecek kodun son bölümü Azure dijital TWINS örneğinde bir sorgu çalıştırır. Bu örnekte kullanılan sorgu, örnekteki tüm dijital TWINS 'i döndürür.

Yönteminin sonuna aşağıdaki kodu ekleyin `Main` :

```csharp
// Run a query    
AsyncPageable<string> result = client.QueryAsync("Select * From DigitalTwins");
await foreach (string twin in result)
{
    object jsonObj = JsonSerializer.Deserialize<object>(twin);
    string prettyTwin = JsonSerializer.Serialize(jsonObj, new JsonSerializerOptions { WriteIndented = true });
    Console.WriteLine(prettyTwin);
    Console.WriteLine("---------------");
}
```

Komut pencerenizde, programını ile çalıştırın `dotnet run` . Çıktıda bu örnekteki tüm dijital TWINS ' i görmeniz gerekir.

## <a name="complete-code-example"></a>Kod örneğini doldurun

Öğreticide bu noktada, Azure dijital TWINS 'e yönelik temel eylemleri gerçekleştirmenize yönelik, tüm istemci uygulamanız vardır. Başvuru için, *program.cs* içindeki programın tam kodu aşağıda listelenmiştir:

```csharp
using System;
using Azure.DigitalTwins.Core;
using Azure.Identity;
using System.Threading.Tasks;
using System.IO;
using System.Collections.Generic;
using Azure;
using Azure.DigitalTwins.Core.Models;
using Azure.DigitalTwins.Core.Serialization;
using System.Text.Json;

namespace minimal
{
    class Program
    {
        static async Task Main(string[] args)
        {
            Console.WriteLine("Hello World!");
            
            string clientId = "<your-application-ID>";
            string tenantId = "<your-directory-ID>";
            string adtInstanceUrl = "https://<your-Azure-Digital-Twins-instance-hostName>";
            var credentials = new InteractiveBrowserCredential(tenantId, clientId);
            DigitalTwinsClient client = new DigitalTwinsClient(new Uri(adtInstanceUrl), credentials);
            Console.WriteLine($"Service client created – ready to go");

            Console.WriteLine();
            Console.WriteLine($"Upload a model");
            var typeList = new List<string>();
            string dtdl = File.ReadAllText("SampleModel.json");
            typeList.Add(dtdl);

            // Upload the model to the service
            try {
                await client.CreateModelsAsync(typeList);
            } catch (RequestFailedException rex) {
                Console.WriteLine($"Load model: {rex.Status}:{rex.Message}");
            }
            // Read a list of models back from the service
            AsyncPageable<ModelData> modelDataList = client.GetModelsAsync();
            await foreach (ModelData md in modelDataList)
            {
                Console.WriteLine($"Type name: {md.DisplayName}: {md.Id}");
            }

            // Initialize twin data
            BasicDigitalTwin twinData = new BasicDigitalTwin();
            twinData.Metadata.ModelId = "dtmi:com:contoso:SampleModel;1";
            twinData.CustomProperties.Add("data", $"Hello World!");
    
            string prefix="sampleTwin-";
            for(int i=0; i<3; i++) {
                try {
                    twinData.Id = $"{prefix}{i}";
                    await client.CreateDigitalTwinAsync($"{prefix}{i}", JsonSerializer.Serialize(twinData));
                    Console.WriteLine($"Created twin: {prefix}{i}");
                } catch(RequestFailedException rex) {
                    Console.WriteLine($"Create twin error: {rex.Status}:{rex.Message}");  
                }
            }

            // Connect the twins with relationships
            await CreateRelationship(client, "sampleTwin-0", "sampleTwin-1");
            await CreateRelationship(client, "sampleTwin-0", "sampleTwin-2");

            //List the relationships
            await ListRelationships(client, "sampleTwin-0");

            // Run a query    
            AsyncPageable<string> result = client.QueryAsync("Select * From DigitalTwins");
            await foreach (string twin in result)
            {
                object jsonObj = JsonSerializer.Deserialize<object>(twin);
                string prettyTwin = JsonSerializer.Serialize(jsonObj, new JsonSerializerOptions { WriteIndented = true });
                Console.WriteLine(prettyTwin);
                Console.WriteLine("---------------");
            }
        }

        public async static Task CreateRelationship(DigitalTwinsClient client, string srcId, string targetId)
        {
            var relationship = new BasicRelationship
            {
                TargetId = targetId,
                Name = "contains"
            };
        
            try
            {
                string relId = $"{srcId}-contains->{targetId}";
                await client.CreateRelationshipAsync(srcId, relId, JsonSerializer.Serialize(relationship));
                Console.WriteLine("Created relationship successfully");
            }
            catch (RequestFailedException rex) {
                Console.WriteLine($"Create relationship error: {rex.Status}:{rex.Message}");
            }
        }
        
        public async static Task ListRelationships(DigitalTwinsClient client, string srcId)
        {
            try {
                AsyncPageable<string> results = client.GetRelationshipsAsync(srcId);
                Console.WriteLine($"Twin {srcId} is connected to:");
                await foreach (string rel in results)
                {
                    var brel = JsonSerializer.Deserialize<BasicRelationship>(rel);
                    Console.WriteLine($" -{brel.Name}->{brel.TargetId}");
                }
            } catch (RequestFailedException rex) {
                Console.WriteLine($"Relationship retrieval error: {rex.Status}:{rex.Message}");   
            }
        }

    }
}
```
## <a name="clean-up-resources"></a>Kaynakları temizleme
 
Bu öğreticide kullanılan örnek, bir sonraki öğreticide yeniden kullanılabilir, [*öğretici: örnek bir istemci uygulamasıyla ilgili temel bilgileri keşfedebilir*](tutorial-command-line-app.md). Sonraki öğreticiye devam etmeyi planlıyorsanız, burada ayarladığınız Azure dijital TWINS örneğini koruyabilirsiniz.
 
Bu öğreticide oluşturulan kaynaklara artık ihtiyacınız yoksa, bunları silmek için aşağıdaki adımları izleyin.

[Azure Cloud Shell](https://shell.azure.com)kullanarak, [az Group Delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete) komutuyla bir kaynak grubundaki tüm Azure kaynaklarını silebilirsiniz. Bu, kaynak grubunu ve Azure dijital TWINS örneğini kaldırır.

> [!IMPORTANT]
> Silinen kaynak grupları geri alınamaz. Kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. 

Bir Azure Cloud Shell açın ve kaynak grubunu ve içerdiği her şeyi silmek için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name <your-resource-group>
```

Ardından, aşağıdaki komutla istemci uygulamanız için oluşturduğunuz Azure Active Directory Uygulama kaydını silin:

```azurecli
az ad app delete --id <your-application-ID>
```

Son olarak, yerel makinenizde oluşturduğunuz proje klasörünü silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, sıfırdan bir .NET konsol istemci uygulaması oluşturdunuz. Bu istemci uygulamanın kodunu bir Azure dijital TWINS örneği üzerinde temel eylemleri gerçekleştirecek şekilde yazmış olursunuz.

Örnek bir istemci uygulamasıyla yapabileceklerinizden birini araştırmak için sonraki öğreticiye geçin: 

> [!div class="nextstepaction"]
> [*Öğretici: örnek bir istemci uygulamasıyla ilgili temel bilgileri araştırma*](tutorial-command-line-app.md)

Bu öğreticide, nasıl yapılır makalelerinde daha fazla yönetim işlemi öğrenerek ya da öğreticide çalıştığınız öğeler hakkında daha fazla bilgi edinmek için kavram belgelerini aramaya başladığınızda bu öğreticide yazdığınız koda da eklenebilir.
* [*Nasıl yapılır: özel modelleri yönetme*](how-to-manage-model.md)
* [*Kavramlar: özel modeller*](concepts-models.md)
