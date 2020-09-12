---
title: Özel modelleri yönetme
titleSuffix: Azure Digital Twins
description: Bkz. Azure dijital TWINS 'te model oluşturma, düzenleme ve silme.
author: baanders
ms.author: baanders
ms.date: 3/12/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 3deb7c0802dbfcdb65bcff6cb2653e73017651f1
ms.sourcegitcommit: c52e50ea04dfb8d4da0e18735477b80cafccc2cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89536464"
---
# <a name="manage-azure-digital-twins-models"></a>Azure dijital TWINS modellerini yönetme

Azure Digital TWINS örneğinizin, [**Digitaltwınsmodeller API 'lerini**](how-to-use-apis-sdks.md), [.net (C#) SDK 'Sını](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core)veya [Azure dijital TWINS CLI](how-to-use-cli.md)'sını kullanma hakkında bilgi sahibi olan [modelleri](concepts-models.md) yönetebilirsiniz. 

Yönetim işlemlerine, modellerin karşıya yüklenmesi, doğrulanması, alınması ve silinmesi dahildir. 

## <a name="create-models"></a>Model oluşturma

Azure dijital TWINS modelleri DTDL 'de yazılmıştır ve *. JSON* dosyaları olarak kaydedilir. Ayrıca, [Visual Studio Code](https://code.visualstudio.com/)için kullanılabilir bir [dtdl uzantısı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-dtdl) vardır ve bu da dtdl belgelerinin yazılmasını kolaylaştırmak için sözdizimi doğrulaması ve diğer özellikleri sağlar.

Bir hastanın odalarını dijital olarak göstermek istediği örneği göz önünde bulundurun. Her odada, bir yandan trafiği izlemek için bir akıllı SOAP dağıtıcısı ve oda üzerinden trafiği izlemek için algılayıcılar yer alır.

Çözüme doğru ilk adım, hastanın yönlerini temsil eden modeller oluşturmaktır. Bu senaryodaki bir hasta odası şöyle açıklanabilir:

```json
{
  "@id": "dtmi:com:contoso:PatientRoom;1",
  "@type": "Interface",
  "@context": "dtmi:dtdl:context;2",
  "displayName": "Patient Room",
  "contents": [
    {
      "@type": "Property",
      "name": "visitorCount",
      "schema": "double"
    },
    {
      "@type": "Property",
      "name": "handWashCount",
      "schema": "double"
    },
    {
      "@type": "Property",
      "name": "handWashPercentage",
      "schema": "double"
    },
    {
      "@type": "Relationship",
      "name": "hasDevices"
    }
  ]
}
```

> [!NOTE]
> Bu, bir modelin bir istemci projesinin parçası olarak yüklenmesi ve kaydedildiği bir. JSON dosyası için örnek gövdedir. Diğer taraftan REST API çağrısı, yukarıdaki gibi bir model tanımları dizisi alır ( `IEnumerable<string>` .NET SDK içinde bir ile eşleştirilir). Bu modeli REST API doğrudan kullanabilmek için köşeli ayraç ile çevreleyin.

Bu model, hasta odası için bir ad ve benzersiz KIMLIĞI ve ziyaretçi sayısını ve el-yıkama durumunu temsil edecek özellikleri tanımlar (Bu sayaçlar hareket sensörleri ve akıllı SOAP dağıtıcılarından güncelleştirilir ve bir *handyıkama yüzdesi* özelliği hesaplamak için birlikte kullanılır). Model Ayrıca, bu *Oda* modeline bağlı olarak herhangi bir [dijital TWINS](concepts-twins-graph.md) 'i gerçek cihazlara bağlamak için kullanılacak olan bir ilişki *hasdevices*'i tanımlar.

Bu yöntemi izleyerek, hospstanonun için modeller, bölgeler veya hastanın kendisi için modeller tanımlama bölümüne geçebilirsiniz.

### <a name="validate-syntax"></a>Sözdizimini doğrula

[!INCLUDE [Azure Digital Twins: validate models info](../../includes/digital-twins-validate.md)]

## <a name="manage-models-with-apis"></a>Modelleri API 'lerle yönetme

Aşağıdaki bölümlerde, [Azure Digital TWINS API 'leri ve SDK 'ları](how-to-use-apis-sdks.md)kullanılarak farklı model yönetimi işlemlerinin nasıl tamamlanacağı gösterilmektedir.

> [!NOTE]
> Aşağıdaki örneklerde, breçekimi için hata işleme yoktur. Ancak, try/catch bloklarındaki hizmet çağrılarını kaydırmak için projelerinizde kesinlikle önerilir.

> [!TIP] 
> Tüm SDK yöntemlerinin zaman uyumlu ve zaman uyumsuz sürümlerde yer aldığını unutmayın. Disk belleği çağrılarında zaman uyumsuz yöntemler, `AsyncPageable<T>` zaman uyumlu sürümler geri dönirken döndürülür `Pageable<T>` .

### <a name="upload-models"></a>Modelleri karşıya yükle

Modeller oluşturulduktan sonra Azure dijital TWINS örneğine yükleyebilirsiniz.

> [!TIP]
> Azure dijital TWINS örneğinizi karşıya yüklemeden önce modellerinizi çevrimdışı doğrulamanız önerilir. [Modeldl istemci tarafı ayrıştırıcı kitaplığı](https://nuget.org/packages/Microsoft.Azure.DigitalTwins.Parser/) ve nasıl yapılır: modellerinizi hizmete yüklemeden önce bunları denetlemek için [*modelleri denetlemek için modeller ve doğrulama*](how-to-parse-models.md) bölümünde açıklanan [dtdl doğrulayıcısı örneğini](https://docs.microsoft.com/samples/azure-samples/dtdl-validator/dtdl-validator) kullanabilirsiniz.

Bir modeli karşıya yüklemeye hazırsanız, aşağıdaki kod parçacığını kullanabilirsiniz:

```csharp
// 'client' is an instance of DigitalTwinsClient
// Read model file into string (not part of SDK)
StreamReader r = new StreamReader("MyModelFile.json");
string dtdl = r.ReadToEnd(); r.Close();
string[] dtdls = new string[] { dtdl };
client.CreateModels(dtdls);
```

`CreateModels`Yöntemin tek bir işlemde birden çok dosya kabul ettiğini gözlemleyin. Şunları gösteren bir örnek aşağıda verilmiştir:

```csharp
var dtdlFiles = Directory.EnumerateFiles(sourceDirectory, "*.json");

List<string> dtdlStrings = new List<string>();
foreach (string fileName in dtdlFiles)
{
    // Read model file into string (not part of SDK)
    StreamReader r = new StreamReader(fileName);
    string dtdl = r.ReadToEnd(); r.Close();
    dtdlStrings.Add(dtdl);
}
client.CreateModels(dtdlStrings);
```

Model dosyaları tek bir modelden daha fazla bulunabilir. Bu durumda, modellerin bir JSON dizisine yerleştirilmesi gerekir. Örneğin:

```json
[
  {
    "@id": "dtmi:com:contoso:Planet",
    "@type": "Interface",
    //...
  },
  {
    "@id": "dtmi:com:contoso:Moon",
    "@type": "Interface",
    //...
  }
]
```
 
Karşıya yükleme sırasında model dosyaları hizmet tarafından onaylanır.

### <a name="retrieve-models"></a>Modelleri al

Azure dijital TWINS örneğiniz üzerinde depolanan modelleri listeleyebilir ve alabilirsiniz. 

Seçenekleriniz şunlardır:
* Tüm modelleri al
* Tek bir model alma
* Bağımlılıklar içeren tek bir modeli alma
* Modeller için meta verileri alma

Aşağıda bazı örnek çağrılar verilmiştir:

```csharp
// 'client' is a valid DigitalTwinsClient object

// Get a single model, metadata and data
ModelData md1 = client.GetModel(id);

// Get a list of the metadata of all available models
Pageable<ModelData> pmd2 = client.GetModels();

// Get a list of metadata and full model definitions
Pageable<ModelData> pmd3 = client.GetModels(null, true);

// Get models and metadata for a model ID, including all dependencies (models that it inherits from, components it references)
Pageable<ModelData> pmd4 = client.GetModels(new string[] { modelId }, true);
```

API 'leri, tüm dönüş nesnelerini almak için çağırır `ModelData` . `ModelData` Azure dijital TWINS örneğinde depolanan model hakkında ad, DTMı ve model oluşturma tarihi gibi meta verileri içerir. `ModelData`Nesne Ayrıca, isteğe bağlı olarak modelin kendisini de içerir. Parametrelere bağlı olarak, yalnızca meta verileri almak için alma çağrılarını kullanabilirsiniz (örneğin, kullanılabilir araçların Kullanıcı arabirimi listesini göstermek istediğiniz senaryolarda faydalıdır) veya modelin tamamı.

`RetrieveModelWithDependencies`Çağrı yalnızca istenen modeli değil, istenen modelin bağımlı olduğu tüm modelleri de döndürür.

Modeller, içinde karşıya yüklendikleri belge formunda tam olarak döndürülmeyebilir. Azure dijital TWINS yalnızca dönüş formunun anlamsal olarak eşdeğer olmasını güvence altına alır. 

### <a name="update-models"></a>Modelleri Güncelleştir

Azure dijital TWINS örneğinize bir model yüklendikten sonra model arabiriminin tamamı sabittir. Bu, modellerin geleneksel "düzenlemesi" olmadığı anlamına gelir. Azure dijital TWINS ayrıca aynı modelin yeniden karşıya yüklenmesine izin vermez.

Bunun yerine, bir modelde değişiklik yapmak istiyorsanız — Örneğin, güncelleştirme `displayName` veya `description` — bunu yapmanın yolu, modelin **daha yeni bir sürümünü** karşıya yüklemedir. 

#### <a name="model-versioning"></a>Model sürümü oluşturma

Varolan bir modelin yeni bir sürümünü oluşturmak için özgün modelin DTDL ile başlayın. Değiştirmek istediğiniz alanları güncelleştirin.

>[!NOTE]
>Önizleme süresince, bir model sürümünün ilerleme özelliği yalnızca yeni alanlar eklemenize izin verir, bu da var olanları kaldırmaz. Alanları kaldırmak için yalnızca [Yeni bir marka modeli oluşturmanız](#create-models)gerekir.

Ardından, modelin alanını güncelleştirerek bunu modelin daha yeni bir sürümü olarak işaretleyin `id` . Model KIMLIĞININ son bölümü, öğesinden sonra, `;` model numarasını temsil eder. Bu modelin daha güncel bir sürümünü olduğunu göstermek için, değerin sonundaki sayıyı `id` geçerli sürüm numarasından daha büyük bir sayı olarak artırın.

Örneğin, önceki model KIMLIĞINIZ şöyle görünüyorsa:

```json
"@id": "dtmi:com:contoso:PatientRoom;1",
```

Bu modelin 2. sürümü şöyle görünebilir:

```json
"@id": "dtmi:com:contoso:PatientRoom;2",
```

Ardından, modelin yeni sürümünü örneğinize yükleyin. 

Daha sonra modelin bu sürümü, dijital TWINS için kullanmak üzere Örneğinizde kullanılabilir olacaktır. Modelin önceki sürümlerinin üzerine yazmaz **, bu nedenle** modelin birden çok sürümü siz [onları](#remove-models)kaldırana kadar Örneğinizde birlikte kalır.

#### <a name="impact-on-twins"></a>TWINS üzerindeki etki

Yeni bir ikizi oluşturduğunuzda, yeni model sürümü ve eski model sürümü birlikte kullanıldığında, yeni ikizi modelin yeni sürümünü veya eski sürümünü kullanabilir.

Bu Ayrıca, bir modelin yeni bir sürümünün karşıya yüklenmesi mevcut TWINS 'in otomatik olarak etkilenmeyeceği anlamına gelir. Mevcut TWINS yalnızca eski model sürümünün örneklerini kalacak.

*Nasıl yapılır: dijital TWINS 'ı yönetme*konusunun [*dijital Ikizi 'nin modelini güncelleştirme*](how-to-manage-twin.md#update-a-digital-twins-model) bölümünde açıklandığı gibi, var olan mevcut TWINS sürümlerini yeni model sürümüne güncelleştirebilirsiniz. Aynı düzeltme ekinde, hem **model kimliğini** (yeni sürüme) hem de **ikizi üzerinde değiştirilmesi gereken tüm alanları yeni modele uygun hale getirmek için**güncelleştirmeniz gerekir.

### <a name="remove-models"></a>Modelleri kaldır

Modeller, iki şekilde de hizmetten kaldırılabilir:
* **Yetki alma** : bir model kullanımdan kaldırıldıktan sonra yeni dijital TWINS oluşturmak için artık bunu kullanamazsınız. Bu modeli zaten kullanan var olan dijital TWINS etkilenmemiştir, bu nedenle bunları Özellik değişiklikleri gibi şeyler ile güncelleştirebilir ve ilişkileri ekleme veya silme işlemleri yapabilirsiniz.
* **Silme** : Bu işlem, modeli çözümden tamamen kaldırır. Bu modeli kullanan her türlü TWINS artık geçerli bir modelle ilişkili değildir, bu nedenle bir modeli hiç olmasa da, kabul edilir. Bu TWINS 'i yine de okuyabilirsiniz, ancak farklı bir modele yeniden atanana kadar hiçbir güncelleştirme yapamayacağız.

Bunlar ayrı özelliklerdir ve bir modeli kademeli olarak kaldırmak için birlikte kullanılabilmesine rağmen birbirini etkilemez. 

#### <a name="decommissioning"></a>Yetkisini alma

Bir modelin yetkisini alma kodu aşağıda verilmiştir:

```csharp
// 'client' is a valid DigitalTwinsClient  
client.DecommissionModel(dtmiOfPlanetInterface);
// Write some code that deletes or transitions digital twins
//...
```

Modelin yetki alma durumu, `ModelData` model alma API 'leri tarafından döndürülen kayıtlara dahildir.

#### <a name="deletion"></a>Silme

Örneğinizdeki tüm modelleri aynı anda silebilir veya tek bir temelinde yapabilirsiniz.

Tüm modellerin nasıl silineceği hakkında bir örnek için öğreticide kullanılan örnek uygulamayı indirin [*: örnek bir istemci uygulamasıyla ilgili temel bilgileri araştırma*](tutorial-command-line-app.md). *CommandLoop.cs* dosyası bunu bir `CommandDeleteAllModels` işlevde yapar.

Bu bölümün geri kalanı, model silme işlemini daha yakından ayrıntılandırır ve tek bir model için nasıl yapılacağını gösterir.

##### <a name="before-deletion-deletion-requirements"></a>Silinmeden önce: silme gereksinimleri

Genellikle, modeller herhangi bir zamanda silinebilir.

Özel durum, diğer modellerin bir `extends` ilişki ya da bir bileşen olarak bağımlı olduğu modeldir. Örneğin, bir *conferenceroom* modeli bir *Oda* modelini genişletiyor ve bileşen olarak bir *Acunit* modeli içeriyorsa, *conferenceroom* Ilgili başvuruları kaldırana kadar *Oda* veya *acunit* 'i silemezsiniz. 

Bağımlılıkları kaldırmak veya bağımlı modeli tamamen silmek için bağımlı modeli güncelleştirerek bunu yapabilirsiniz.

##### <a name="during-deletion-deletion-process"></a>Silme sırasında: silme işlemi

Bir model hemen silme gereksinimlerini karşılasa bile, arka planda kalan TWINS için istenmeyen sonuçlara engel olmak üzere birkaç adımdan önce gezinmek isteyebilirsiniz. Bu işlemi yönetmenize yardımcı olabilecek bazı adımlar aşağıda verilmiştir:
1. İlk olarak, modelin yetkisini alın
2. Hizmetin kullanımdan önce gönderilen son dakikalık ikizi oluşturma isteklerini işlemediğinden emin olmak için birkaç dakika bekleyin
3. Şimdi kullanımdan kaldırılan modeli kullanan tüm TWINS 'i görmek için modele göre sorgu
4. Artık gerekmiyorsa TWINS 'i silin veya gerekirse yeni bir modele yama yapın. Ayrıca, bu durumda, model silindikten sonra Modelsiz olarak ikleşecek şekilde, bunları tek başına bırakmayı tercih edebilirsiniz. Bu durumun etkileri için sonraki bölüme bakın.
5. Değişikliklerin bir süre içinde korunduğundan emin olmak için birkaç dakika bekleyin
6. Modeli Sil 

Bir modeli silmek için şu çağrıyı kullanın:
```csharp
// 'client' is a valid DigitalTwinsClient
await client.DeleteModelAsync(IDToDelete);
```

##### <a name="after-deletion-twins-without-models"></a>Silinmeden sonra: modeller olmadan TWINS

Model silindikten sonra, modeli kullanan herhangi bir dijital TWINS artık model olmadan kabul edilir. Bu durumda her türlü TWINS 'in bir listesini verebileceğinize karşın, TWINS 'in hangi tnelerin etkilendiğini görmek için silinen model *tarafından hala sorgu yapabileceğinize* izin vereceğimizi unutmayın.

Bir modeli olmayan TWINS ile neler yapabileceğinize ilişkin genel bir bakış aşağıda verilmiştir.

**Şunları yapabilirsiniz** :
* İkizi sorgulama
* Okuma özellikleri
* Giden ilişkileri oku
* Gelen ilişkileri ekleme ve silme (' de olduğu gibi, diğer TWINS *Bu ikizi* ilişki yine de kullanılabilir)
  - `target`İlişki tanımındaki öğesinde, silinen modelin dtmı 'ı yansıtması devam edebilir. Tanımlı hedef olmayan bir ilişki de burada çalışabilir.
* İlişkileri Sil
* İkizi silme

**Yapabileceğinize** dikkat edin:
* Giden ilişkileri Düzenle (içinde olduğu gibi, bu ikizi ile diğer TWINS *arasındaki* ilişkiler)
* Özellikleri düzenle

##### <a name="after-deletion-re-uploading-a-model"></a>Silinmeden sonra: bir modeli yeniden yükleme

Bir model silindikten sonra, sildikten sonra aynı KIMLIĞE sahip yeni bir modeli karşıya yüklemeye karar verebilirsiniz. Bu durumda ne olur?
* Çözüm mağazasının perspektifinden, bu, tamamen yeni bir modeli karşıya yüklemeyle aynıdır. Hizmet, eski bir hizmetin karşıya yüklendiğini hatırlıyor.   
* Grafikte silinen modele başvuran bir TWINS varsa, bunlar artık sahipsiz kalmaz; Bu model KIMLIĞI yeni tanımıyla yeniden geçerlidir. Ancak, modelin yeni tanımı silinen model tanımından farklıysa, bu TWINS, silinen tanımıyla eşleşen özellikler ve ilişkiler içerebilir ve yeni bir ile geçerli değildir.

Azure dijital TWINS bu durumu engellemez, bu nedenle, model tanımı anahtarı aracılığıyla geçerli kalmasını sağlamak için TWINS 'i uygun şekilde düzeltme ekine dikkat edin.

## <a name="manage-models-with-cli"></a>CLı ile modelleri yönetme

Modeller ayrıca Azure dijital TWINS CLı kullanılarak yönetilebilir. Komutları [*nasıl yapılır: Azure dijital TWINS CLI 'Sını kullanma*](how-to-use-cli.md)bölümünde bulabilirsiniz.

[!INCLUDE [digital-twins-known-issue-cloud-shell](../../includes/digital-twins-known-issue-cloud-shell.md)]

## <a name="next-steps"></a>Sonraki adımlar

Modellerinize göre dijital TWINS oluşturma ve yönetme hakkında bilgi için bkz..
* [*Nasıl yapılır: dijital TWINS 'i yönetme*](how-to-manage-twin.md)