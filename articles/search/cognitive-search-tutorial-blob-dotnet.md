---
title: C#Dizin oluşturma işlem hattında Bilişsel Hizmetler API'si çağırma öğreticisi-Azure Search
description: Bu öğreticide veri ayıklama ve dönüştürme için Azure Search dizin oluşturma bölümünde veri ayıklama, doğal dil ve görüntü AI işleme örneğini adım adım izleyin.
manager: nitinme
author: MarkHeff
services: search
ms.service: search
ms.topic: tutorial
ms.date: 05/02/2019
ms.author: maheff
ms.openlocfilehash: b40cd63062e961848eb1ab6b956e63a83a634817
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71936947"
---
# <a name="c-tutorial-call-cognitive-services-apis-in-an-azure-search-indexing-pipeline"></a>C#Öğretici: Azure Search dizin oluşturma ardışık düzeninde Bilişsel Hizmetler API'si çağrısı yapın

Bu öğreticide, bilişsel becerileri kullanarak Azure Search veri zenginleştirme ile programlama *becerisini*öğrenirsiniz. Yetenekler, bilişsel hizmetler 'de doğal dil işleme (NLP) ve görüntü analizi özellikleri tarafından desteklenir. Beceri kompozisyonu ve yapılandırması aracılığıyla bir görüntünün veya taranan belge dosyasının metin ve metin gösterimlerini ayıklayabilirsiniz. Ayrıca dili, varlıkları, anahtar tümcecikleri ve daha fazlasını tespit edebilirsiniz. Nihai sonuç, AI destekli bir dizin oluşturma işlem hattı tarafından oluşturulan Azure Search dizinde zengin ek içerik sağlar.

Bu öğreticide, .NET SDK 'yı kullanarak aşağıdaki görevleri gerçekleştirebilirsiniz:

> [!div class="checklist"]
> * Bir dizine yönlendirmekte örnek verileri zenginleyen bir dizin oluşturma işlem hattı oluşturma
> * Yerleşik becerileri Uygula: optik karakter tanıma, metin merği, dil algılama, metin bölme, varlık tanıma, anahtar tümceciği ayıklama
> * Girişleri bir beceri içindeki çıkışlara eşleyerek yetenekleri birlikte nasıl zincirleyeceğinizi öğrenin
> * İstekleri Yürüt ve sonuçları İncele
> * Daha fazla geliştirme için dizini ve Dizin oluşturucuyu sıfırlayın

Çıktı, Azure Search üzerindeki tam metin aranabilir bir dizindir. Dizini [eş anlamlılar](search-synonyms.md), [Puanlama profilleri](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [çözümleyiciler](search-analyzers.md)ve [Filtreler](search-filters.md)gibi diğer standart yetenekler ile geliştirebilirsiniz.

Bu öğretici ücretsiz hizmette çalışır, ancak ücretsiz işlem sayısı günde 20 belge ile sınırlıdır. Bu öğreticiyi aynı günde birden çok kez çalıştırmak istiyorsanız, daha fazla çalıştırmaya uyabilmeniz için daha küçük bir dosya kümesi kullanın.

> [!NOTE]
> İşlem sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla AI algoritması ekleyerek kapsamı genişlettikten sonra faturalandırılabilir bilişsel hizmetler kaynağı eklemeniz gerekir. Bilişsel hizmetlerde API 'Leri çağırırken ve Azure Search içinde belge çözme aşamasının bir parçası olarak görüntü ayıklama için tahakkuk ücretleri. Belgelerden metin ayıklama için herhangi bir ücret alınmaz.
>
> Yerleşik yeteneklerin yürütülmesi, mevcut bilişsel [Hizmetler Kullandıkça Öde fiyatı](https://azure.microsoft.com/pricing/details/cognitive-services/) üzerinden ücretlendirilir. Görüntü ayıklama fiyatlandırması [Azure Search fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmaktadır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Prerequisites

Aşağıdaki hizmetler, Araçlar ve veriler bu öğreticide kullanılır. 

+ Örnek verileri depolamak için [bir Azure depolama hesabı oluşturun](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) . Depolama hesabının Azure Search aynı bölgede bulunduğundan emin olun.

+ [Örnek veriler](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4) , küçük bir dosya kümesi farklı türlerden oluşur. 

+ IDE olarak kullanmak için [Visual Studio 'Yu yükler](https://visualstudio.microsoft.com/) .

+ Geçerli aboneliğinizde [bir Azure Search hizmeti oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) . Bu öğretici için ücretsiz bir hizmet kullanabilirsiniz.

## <a name="get-a-key-and-url"></a>Anahtar ve URL al

Azure Search hizmetinize etkileşimde bulunmak için hizmet URL 'SI ve erişim anahtarı gerekir. Her ikisiyle de bir arama hizmeti oluşturulur. bu nedenle aboneliğinize Azure Search eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure Portal oturum açın](https://portal.azure.com/)ve arama hizmetine **genel bakış** sayfasında URL 'yi alın. Örnek bir uç nokta `https://mydemo.search.windows.net` gibi görünebilir.

1. **Ayarlar** > **anahtarlar**' da, hizmette tam haklar için bir yönetici anahtarı alın. Üzerinde bir tane almanız gereken iş sürekliliği için iki adet değiştirilebilir yönetici anahtarı vardır. Nesneleri eklemek, değiştirmek ve silmek için isteklerde birincil veya ikincil anahtarı kullanabilirsiniz.

   HTTP uç noktası ![ve erişim anahtarı al](media/search-get-started-postman/get-url-key.png "http uç noktası ve erişim anahtarı al")

Geçerli bir anahtara sahip olmak, istek başına, isteği gönderen uygulama ve onu işleyen hizmet arasında güven oluşturur.

## <a name="prepare-sample-data"></a>Örnek verileri hazırlama

Zenginleştirme işlem hattı, Azure veri kaynaklarından çeker. Kaynak veriler [Azure Search dizin oluşturucunun](search-indexer-overview.md)desteklenen bir veri kaynağı türünden kaynaklanmalıdır. Bu alıştırmada, birden çok içerik türünü göstermek için blob depolamayı kullanırız.

1. [Azure Portal oturum açın](https://portal.azure.com), Azure depolama hesabınıza gidin, **Bloblar**' a tıklayın ve ardından **+ Container**' a tıklayın.

1. Örnek veri içeren [bir blob kapsayıcısı oluşturun](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) . Ortak erişim düzeyini geçerli değerlerinden herhangi birine ayarlayabilirsiniz. Bu öğreticide kapsayıcı adının "temel-demo-Data-PR" olduğu varsayılır.

1. Kapsayıcı oluşturulduktan sonra dosyayı açın ve [örnek verileri](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4)karşıya yüklemek için komut çubuğunda **karşıya yükle** ' yi seçin.

   ![Azure Blob depolamada kaynak dosyaları](./media/cognitive-search-quickstart-blob/sample-data.png)

1. Örnek dosyalar yüklendikten sonra, BLOB depolama alanınızı için kapsayıcı adını ve bir bağlantı dizesini alın. Bunu, Azure portal depolama hesabınıza giderek, **erişim anahtarları**' nı seçerek ve ardından **bağlantı dizesi** alanını kopyalayarak yapabilirsiniz.

   Bağlantı dizesi, aşağıdaki örneğe benzer bir URL olmalıdır:

      ```http
      DefaultEndpointsProtocol=https;AccountName=cogsrchdemostorage;AccountKey=<your account key>;EndpointSuffix=core.windows.net
      ```

Bağlantı dizesini belirtmek için, paylaşılan erişim imzası sağlamak gibi başka yollar vardır. Veri kaynağı kimlik bilgileri hakkında daha fazla bilgi edinmek için bkz. [Azure Blob depolamayı dizin oluşturma](search-howto-indexing-azure-blob-storage.md#Credentials).

## <a name="set-up-your-environment"></a>Ortamınızı ayarlama

Visual Studio 'Yu açıp .NET Core üzerinde çalışabilen yeni bir konsol uygulama projesi oluşturarak başlayın.

### <a name="install-nuget-packages"></a>NuGet paketlerini yükler

[Azure Search .NET SDK](https://aka.ms/search-sdk) , dizinlerinizi, veri kaynaklarınızı, Dizin Oluşturucularınızı ve becerileri yönetmenizi sağlayan birkaç istemci kitaplığı içerir. Ayrıca, http ve JSON ayrıntıları ile uğraşmak zorunda kalmadan belgeleri karşıya yükleyebilir ve yönetebilir ve sorguları yürütebilirsiniz. . Bu istemci kitaplıklarının hepsi NuGet paketleri olarak dağıtılır.

Bu proje için, `Microsoft.Azure.Search` NuGet paketinin 9. sürümünü ve en son @no__t 1 NuGet paketini yüklemeniz gerekir.

Visual Studio 'da Paket Yöneticisi konsolu 'nu kullanarak `Microsoft.Azure.Search` NuGet paketini yükler. Paket Yöneticisi konsolunu açmak için **araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin. Çalıştırılacak komutu almak için [Microsoft. Azure. Search NuGet paketi sayfasına](https://www.nuget.org/packages/Microsoft.Azure.Search)gidin, sürüm 9 ' ı seçin ve Paket Yöneticisi komutunu kopyalayın. Paket Yöneticisi konsolunda, bu komutu çalıştırın.

Visual Studio 'da `Microsoft.Extensions.Configuration.Json` NuGet paketini yüklemek için **araçlar** > **nuget Paket Yöneticisi** > **çözüm için NuGet Paketlerini Yönet**' i seçin... @No__t-6 NuGet paketi için araştır ve ara ' yı seçin. Onu bulduktan sonra, paketi seçin, projenizi seçin, sürümün en son kararlı sürümü olduğunu onaylayın ve ardından Install ' ı seçin.

## <a name="add-azure-search-service-information"></a>Azure Search hizmeti bilgileri ekleme

Azure Search hizmetinize bağlanabilmek için, arama hizmeti bilgilerini projenize eklemeniz gerekir. Çözüm Gezgini projenize sağ tıklayıp  > **Yeni öğe Ekle...** seçeneğini belirleyin. Dosyayı `appsettings.json` olarak adlandırın ve **Ekle**' yi seçin. 

Bu dosyanın, çıkış dizininize dahil olması gerekir. Bunu yapmak için `appsettings.json` ' a sağ tıklayıp **Özellikler**' i seçin. **Çıkış Dizinine Kopyala** değerini **daha yeni bir kopya**olacak şekilde değiştirin.

Aşağıdaki JSON dosyasını yeni JSON dosyanıza kopyalayın.

```json
{
  "SearchServiceName": "Put your search service name here",
  "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
  "SearchServiceQueryApiKey": "Put your query API key here",
  "AzureBlobConnectionString": "Put your Azure Blob connection string here",
}
```

Arama hizmetinizi ve BLOB depolama hesabı bilgilerinizi ekleyin.

Arama hizmeti bilgilerinizi Azure portal arama hesabı sayfasından alabilirsiniz. Hesap adı ana sayfada yer alır **ve anahtarlar seçilerek anahtarlar bulunabilir.**

Blob bağlantı dizesini Azure portal, **erişim anahtarları**' nı seçerek ve ardından **bağlantı dizesi** alanını kopyalayarak depolama hesabınıza giderek alabilirsiniz.

## <a name="add-namespaces"></a>Ad alanı Ekle

Bu öğretici çeşitli ad alanlarından birçok farklı tür kullanır. Bu türleri kullanabilmek için aşağıdakini `Program.cs` ' a ekleyin.

```csharp
using System;
using System.Collections.Generic;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Extensions.Configuration;
```

## <a name="create-a-client"></a>İstemci oluşturma

Öğesinin bir örneğini oluşturur `SearchServiceClient` sınıfı.

```csharp
IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
IConfigurationRoot configuration = builder.Build();
SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);
```

`CreateSearchServiceClient`, uygulamanın yapılandırma dosyasında (appSettings. JSON) depolanan değerleri kullanarak yeni bir `SearchServiceClient` oluşturur.

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
   string searchServiceName = configuration["SearchServiceName"];
   string adminApiKey = configuration["SearchServiceAdminApiKey"];

   SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
   return serviceClient;
}
```

> [!NOTE]
> @No__t-0 sınıfı, arama hizmetinize bağlantıları yönetir. Çok fazla bağlantı açılmasını önlemek için, mümkünse `SearchServiceClient` ' ın tek bir örneğini paylaşmayı denemelisiniz. Bu tür paylaşımları etkinleştirmek için iş parçacığı güvenlidir.
> 
> 

## <a name="create-a-data-source"></a>Veri kaynağı oluşturma

@No__t-1 çağırarak yeni bir `DataSource` örneği oluşturun. `DataSource.AzureBlobStorage`, veri kaynağı adı, bağlantı dizesi ve BLOB kapsayıcısı adını belirtmenizi gerektirir.

Bu öğreticide kullanılmıyor olsa da, bir geçici silme ilkesi, silinen Blobları bir geçici silme sütununun değerine göre belirlemek için kullanılır. Aşağıdaki ilke, `true` değerine sahip `IsDeleted` meta veri özelliği varsa, bir Blobun silineceğini kabul eder.

```csharp
DataSource dataSource = DataSource.AzureBlobStorage(
    name: "demodata",
    storageConnectionString: configuration["AzureBlobConnectionString"],
    containerName: "basic-demo-data-pr",
    deletionDetectionPolicy: new SoftDeleteColumnDeletionDetectionPolicy(
        softDeleteColumnName: "IsDeleted",
        softDeleteMarkerValue: "true"),
    description: "Demo files to demonstrate cognitive search capabilities.");
```

@No__t-0 nesnesini başlattığını artık veri kaynağını oluşturun. `SearchServiceClient` ' a bir `DataSources` özelliği vardır. Bu özellik Azure Search veri kaynaklarını oluşturmak, listelemek, güncelleştirmek veya silmek için gereken tüm yöntemleri sağlar.

Başarılı bir istek için, yöntem oluşturulan veri kaynağını döndürür. İstekle ilgili bir sorun varsa (geçersiz parametre gibi), yöntem bir özel durum oluşturur.

```csharp
try
{
    serviceClient.DataSources.CreateOrUpdate(dataSource);
}
catch (Exception e)
{
    // Handle the exception
}
```

Bu ilk isteğiniz olduğundan, Azure Search veri kaynağının oluşturulduğunu onaylamak için Azure portal denetleyin. Arama hizmeti panosu sayfasında, veri kaynakları kutucuğunun yeni bir öğe olduğunu doğrulayın. Portal sayfasının yenilenmesi için birkaç dakika beklemeniz gerekebilir.

  (./media/cognitive-search-tutorial-blob/data-source-tile.png "Portalda Portal veri kaynakları kutucuğunda") ![veri kaynakları kutucuğu]

## <a name="create-a-skillset"></a>Beceri oluşturma

Bu bölümde, verilerinize uygulamak istediğiniz bir zenginleştirme adımları kümesi tanımlarsınız. Her bir zenginleştirme adımına bir *yetenek* ve bir *beceri*için bir dizi adım denir. Bu öğretici, beceri için [önceden tanımlanmış](cognitive-search-predefined-skills.md) bilişsel becerileri kullanır:

+ Görüntü dosyalarında yazdırılmış ve el yazısı metinleri tanımak için [optik karakter tanıma](cognitive-search-skill-ocr.md) .

+ Bir alanlar koleksiyonundan tek bir alanda metin birleştirmek için [metin Merkli](cognitive-search-skill-textmerger.md) .

+ İçeriğin dilini belirlemek için [dil algılama](cognitive-search-skill-language-detection.md) .

+ Anahtar ifadesi ayıklama yeteneği ve varlık tanıma yeteneği çağrılmadan önce büyük içeriği daha küçük parçalara bölmek için [metin bölme](cognitive-search-skill-textsplit.md) . Anahtar tümceciği ayıklama ve varlık tanıma, 50.000 veya daha az karakter girişi kabul eder. Örnek dosyalardan birkaçı, bu sınıra sığacak kadar bölünmekte olmalıdır.

+ Blob kapsayıcısında bulunan içerikten kuruluşların adlarını ayıklamak için [varlık tanıma](cognitive-search-skill-entity-recognition.md) .

+ Üst anahtar tümceciklerini çekmek için [anahtar ifade ayıklama](cognitive-search-skill-keyphrases.md) .

İlk işlem sırasında, Azure Search her belgeyi, farklı dosya biçimlerinden içeriği okumak için bir kez daha gizler. Kaynak dosyada oluşan bulunan metin, her belge için bir oluşturulan ```content``` alanına yerleştirilir. Bu şekilde, bu metni kullanmak için girişi @no__t olarak ayarlayın. 

Çıktılar bir dizin ile eşleştirilebilir, bir aşağı akış beceriye giriş olarak veya her ikisi de dil kodu ile aynı durum olabilir. Dizinde, bir dil kodu filtrelemeye yarar. Bir giriş olarak dil kodu, metin analizi becerileri tarafından, sözcük bölünmesi etrafında dilsel kuralları bilgilendirmek için kullanılır.

Beceri temelleri hakkında daha fazla bilgi için bkz. [nasıl tanımlanır bir beceri](cognitive-search-defining-skillset.md).

### <a name="ocr-skill"></a>OCR becerisi

**OCR** becerisi görüntülerden metin ayıklar. Bu beceri bir normalized_images alanının bulunduğunu varsayar. Bu alanı oluşturmak için, öğreticide daha sonra, Dizin Oluşturucu tanımındaki ```"imageAction"``` yapılandırmasını ```"generateNormalizedImages"``` olarak ayarlayacağız.

```csharp
List<InputFieldMappingEntry> inputMappings = new List<InputFieldMappingEntry>();
inputMappings.Add(new InputFieldMappingEntry(
    name: "image",
    source: "/document/normalized_images/*"));

List<OutputFieldMappingEntry> outputMappings = new List<OutputFieldMappingEntry>();
outputMappings.Add(new OutputFieldMappingEntry(
    name: "text",
    targetName: "text"));

OcrSkill ocrSkill = new OcrSkill(
    description: "Extract text (plain and structured) from image",
    context: "/document/normalized_images/*",
    inputs: inputMappings,
    outputs: outputMappings,
    defaultLanguageCode: OcrSkillLanguage.En,
    shouldDetectOrientation: true);
```

### <a name="merge-skill"></a>Birleştirme yeteneği

Bu bölümde, belge içeriği alanını OCR becerisi tarafından üretilen metinle birleştiren bir **birleştirme** yeteneği oluşturacaksınız.

```csharp
List<InputFieldMappingEntry> inputMappings = new List<InputFieldMappingEntry>();
inputMappings.Add(new InputFieldMappingEntry(
    name: "text",
    source: "/document/content"));
inputMappings.Add(new InputFieldMappingEntry(
    name: "itemsToInsert",
    source: "/document/normalized_images/*/text"));
inputMappings.Add(new InputFieldMappingEntry(
    name: "offsets",
    source: "/document/normalized_images/*/contentOffset"));

List<OutputFieldMappingEntry> outputMappings = new List<OutputFieldMappingEntry>();
outputMappings.Add(new OutputFieldMappingEntry(
    name: "mergedText",
    targetName: "merged_text"));

MergeSkill mergeSkill = new MergeSkill(
    description: "Create merged_text which includes all the textual representation of each image inserted at the right location in the content field.",
    context: "/document",
    inputs: inputMappings,
    outputs: outputMappings,
    insertPreTag: " ",
    insertPostTag: " ");
```

### <a name="language-detection-skill"></a>Dil algılama yeteneği

**Dil algılama** Beceri, giriş metninin dilini algılar ve istekte gönderilen her belge için tek bir dil kodu bildirir. **Dil algılama** beceriye ait çıktıyı **metin bölünmüş** beceriye girdinin bir parçası olarak kullanacağız.

```csharp
List<InputFieldMappingEntry> inputMappings = new List<InputFieldMappingEntry>();
inputMappings.Add(new InputFieldMappingEntry(
    name: "text",
    source: "/document/merged_text"));

List<OutputFieldMappingEntry> outputMappings = new List<OutputFieldMappingEntry>();
outputMappings.Add(new OutputFieldMappingEntry(
    name: "languageCode",
    targetName: "languageCode"));

LanguageDetectionSkill languageDetectionSkill = new LanguageDetectionSkill(
    description: "Detect the language used in the document",
    context: "/document",
    inputs: inputMappings,
    outputs: outputMappings);
```

### <a name="text-split-skill"></a>Metin bölünmüş yetenek

Aşağıdaki **bölünmüş** Beceri, metni sayfalara böler ve sayfa uzunluğu `String.Length` ile ölçülen 4.000 karakter ile sınırlandırır. Algoritma, metni en çok `maximumPageLength` olan öbeklere bölmeye çalışır. Bu durumda, algoritma bir cümle sınırında tümceyi bölmek için en iyi şekilde yapılır, bu nedenle öbek boyutu `maximumPageLength` ' dan biraz daha az olabilir.

```csharp
List<InputFieldMappingEntry> inputMappings = new List<InputFieldMappingEntry>();
inputMappings.Add(new InputFieldMappingEntry(
    name: "text",
    source: "/document/merged_text"));
inputMappings.Add(new InputFieldMappingEntry(
    name: "languageCode",
    source: "/document/languageCode"));

List<OutputFieldMappingEntry> outputMappings = new List<OutputFieldMappingEntry>();
outputMappings.Add(new OutputFieldMappingEntry(
    name: "textItems",
    targetName: "pages"));

SplitSkill splitSkill = new SplitSkill(
    description: "Split content into pages",
    context: "/document",
    inputs: inputMappings,
    outputs: outputMappings,
    textSplitMode: TextSplitMode.Pages,
    maximumPageLength: 4000);
```

### <a name="entity-recognition-skill"></a>Varlık tanıma yeteneği

Bu `EntityRecognitionSkill` örneği, `organization` kategori türünü tanıyacak şekilde ayarlanır. **Varlık tanıma** yeteneği, `person` ve `location` kategori türlerini de tanıyabilir.

"Bağlam" alanının bir yıldız işaretiyle ```"/document/pages/*"``` olarak ayarlandığını unutmayın. Bu, enzenginleştirme adımının ```"/document/pages"``` altındaki her sayfa için çağrıldığı anlamına gelir.

```csharp
List<InputFieldMappingEntry> inputMappings = new List<InputFieldMappingEntry>();
inputMappings.Add(new InputFieldMappingEntry(
    name: "text",
    source: "/document/pages/*"));
    
List<OutputFieldMappingEntry> outputMappings = new List<OutputFieldMappingEntry>();
outputMappings.Add(new OutputFieldMappingEntry(
    name: "organizations",
    targetName: "organizations"));

List<EntityCategory> entityCategory = new List<EntityCategory>();
entityCategory.Add(EntityCategory.Organization);
    
EntityRecognitionSkill entityRecognitionSkill = new EntityRecognitionSkill(
    description: "Recognize organizations",
    context: "/document/pages/*",
    inputs: inputMappings,
    outputs: outputMappings,
    categories: entityCategory,
    defaultLanguageCode: EntityRecognitionSkillLanguage.En);
```

### <a name="key-phrase-extraction-skill"></a>Anahtar tümceciği ayıklama yeteneği

Yeni oluşturulan `EntityRecognitionSkill` örneğinde olduğu gibi, belgenin her sayfası için **anahtar ifade ayıklama** becerisi çağırılır.

```csharp
List<InputFieldMappingEntry> inputMappings = new List<InputFieldMappingEntry>();
inputMappings.Add(new InputFieldMappingEntry(
    name: "text",
    source: "/document/pages/*"));
inputMappings.Add(new InputFieldMappingEntry(
    name: "languageCode",
    source: "/document/languageCode"));

List<OutputFieldMappingEntry> outputMappings = new List<OutputFieldMappingEntry>();
outputMappings.Add(new OutputFieldMappingEntry(
    name: "keyPhrases",
    targetName: "keyPhrases"));

KeyPhraseExtractionSkill keyPhraseExtractionSkill = new KeyPhraseExtractionSkill(
    description: "Extract the key phrases",
    context: "/document/pages/*",
    inputs: inputMappings,
    outputs: outputMappings);
```

### <a name="build-and-create-the-skillset"></a>Beceri oluşturma ve oluşturma

Oluşturduğunuz becerileri kullanarak `Skillset` oluşturun.

```csharp
List<Skill> skills = new List<Skill>();
skills.Add(ocrSkill);
skills.Add(mergeSkill);
skills.Add(languageDetectionSkill);
skills.Add(splitSkill);
skills.Add(entityRecognitionSkill);
skills.Add(keyPhraseExtractionSkill);

Skillset skillset = new Skillset(
    name: "demoskillset",
    description: "Demo skillset",
    skills: skills);
```

Arama hizmetinizde beceri oluşturun.

```csharp
try
{
    serviceClient.Skillsets.CreateOrUpdate(skillset);
}
catch (Exception e)
{
    // Handle exception
}
```

## <a name="create-an-index"></a>Dizin oluşturma

Bu bölümde, aranabilir dizine hangi alanların ekleneceğini ve her bir alanın arama özniteliklerini belirterek Dizin şemasını tanımlarsınız. Alanlar bir türe sahiptir ve alanın nasıl kullanıldığını (aranabilir, sıralanabilir, vb.) tespit eden öznitelikler alabilir. Dizindeki alan adlarının, kaynaktaki alan adlarıyla aynı olması için gerekli değildir. Sonraki bir adımda, kaynak-hedef alanlarını bağlamak için bir dizin oluşturucuda alan eşlemeleri eklersiniz. Bu adım için, arama uygulamanız ile ilgili alan adlandırma kurallarını kullanarak dizini tanımlayın.

Bu alıştırma aşağıdaki alanları ve alan türlerini kullanır:

| alan adları: | `id`       | içerik   | languageCode | keyPhrases         | ları     |
|--------------|----------|-------|----------|--------------------|-------------------|
| alan türleri: | EDM. String|EDM. String| EDM. String| < Edm. String > listeleyin  | < Edm. String > listeleyin  |


### <a name="create-demoindex-class"></a>Demoındex sınıfı oluşturma

Bu dizinin alanları bir model sınıfı kullanılarak tanımlanır. Model sınıfının her özelliğinin, karşılık gelen dizin alanının aramayla ilgili davranışlarını belirleyen öznitelikleri vardır. 

Model sınıfını yeni C# bir dosyaya ekleyeceğiz. Projenize sağ tıklayıp  > **Yeni öğe Ekle...** öğesini seçin, "sınıf" öğesini seçin ve dosyayı `DemoIndex.cs` olarak adlandırın, sonra **Ekle**' yi seçin.

@No__t-0 ve `Microsoft.Azure.Search.Models` ad alanlarından türleri kullanmak istediğinizi belirttiğinizden emin olun.

```csharp
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
```

Aşağıdaki model sınıfı tanımını `DemoIndex.cs` ' a ekleyin ve dizini oluşturacağınız aynı ad alanına ekleyin.

```csharp
// The SerializePropertyNamesAsCamelCase attribute is defined in the Azure Search .NET SDK.
// It ensures that Pascal-case property names in the model class are mapped to camel-case
// field names in the index.
[SerializePropertyNamesAsCamelCase]
public class DemoIndex
{
    [System.ComponentModel.DataAnnotations.Key]
    [IsSearchable, IsSortable]
    public string Id { get; set; }

    [IsSearchable]
    public string Content { get; set; }

    [IsSearchable]
    public string LanguageCode { get; set; }

    [IsSearchable]
    public string[] KeyPhrases { get; set; }

    [IsSearchable]
    public string[] Organizations { get; set; }
}
```

Artık bir model sınıfı tanımladığınıza göre `Program.cs` ' da bir dizin tanımı oluşturabilirsiniz. Bu dizinin adı "demoındex" olacaktır.

```csharp
var index = new Index()
{
    Name = "demoindex",
    Fields = FieldBuilder.BuildForType<DemoIndex>()
};
```

Sınama sırasında, dizini birden çok kez oluşturmayı denediğinizden emin olabilirsiniz. Bu nedenle, oluşturmak üzere olduğunuz dizinin oluşturmayı denemeden önce zaten var olup olmadığını denetleyin.

```csharp
try
{
    bool exists = serviceClient.Indexes.Exists(index.Name);

    if (exists)
    {
        serviceClient.Indexes.Delete(index.Name);
    }

    serviceClient.Indexes.Create(index);
}
catch (Exception e)
{
    // Handle exception
}
```

Dizin tanımlama hakkında daha fazla bilgi edinmek için bkz. [Dizin oluşturma (Azure Search REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index).

## <a name="create-an-indexer-map-fields-and-execute-transformations"></a>Dizin Oluşturucu oluşturun, alanları eşleyin ve dönüştürmeleri yürütün

Şimdiye kadar bir veri kaynağı, bir beceri ve bir dizin oluşturdunuz. Bu üç bileşen, her parçayı birlikte tek bir çoklu aşamalı işlemde çeken bir [dizin oluşturucunun](search-indexer-overview.md) parçası haline gelir. Bunları bir dizin oluşturucuda birbirine bağlamak için alan eşlemelerini tanımlamanız gerekir.

+ FieldMappings, Beceri öğesinden önce işlenir, kaynak alanları veri kaynağından bir dizindeki hedef alanlara eşleniyor. Alan adları ve türleri her iki uçta da aynıysa, hiçbir eşleme gerekmez.

+ OutputFieldMappings, Beceri sonrasında işlenir ve belge çözme veya zenginleştirene kadar mevcut olmayan sourceFieldNames öğesine başvuruda bulunur. TargetFieldName, dizindeki bir alandır.

Çıkışlara girişlerin takılmalarının yanı sıra, veri yapılarını düzleştirmek için alan eşlemelerini de kullanabilirsiniz. Daha fazla bilgi için bkz. [zenginleştirilmiş alanları aranabilir bir dizine eşleme](cognitive-search-output-field-mapping.md).

```csharp
IDictionary<string, object> config = new Dictionary<string, object>();
config.Add(
    key: "dataToExtract",
    value: "contentAndMetadata");
config.Add(
    key: "imageAction",
    value: "generateNormalizedImages");

List<FieldMapping> fieldMappings = new List<FieldMapping>();
fieldMappings.Add(new FieldMapping(
    sourceFieldName: "metadata_storage_path",
    targetFieldName: "id",
    mappingFunction: new FieldMappingFunction(
        name: "base64Encode")));
fieldMappings.Add(new FieldMapping(
    sourceFieldName: "content",
    targetFieldName: "content"));

List<FieldMapping> outputMappings = new List<FieldMapping>();
outputMappings.Add(new FieldMapping(
    sourceFieldName: "/document/pages/*/organizations/*",
    targetFieldName: "organizations"));
outputMappings.Add(new FieldMapping(
    sourceFieldName: "/document/pages/*/keyPhrases/*",
    targetFieldName: "keyPhrases"));
outputMappings.Add(new FieldMapping(
    sourceFieldName: "/document/languageCode",
    targetFieldName: "languageCode"));

Indexer indexer = new Indexer(
    name: "demoindexer",
    dataSourceName: dataSource.Name,
    targetIndexName: index.Name,
    description: "Demo Indexer",
    skillsetName: skillSet.Name,
    parameters: new IndexingParameters(
        maxFailedItems: -1,
        maxFailedItemsPerBatch: -1,
        configuration: config),
    fieldMappings: fieldMappings,
    outputFieldMappings: outputMappings);

try
{
    bool exists = serviceClient.Indexers.Exists(indexer.Name);

    if (exists)
    {
        serviceClient.Indexers.Delete(indexer.Name);
    }

    serviceClient.Indexers.Create(indexer);
}
catch (Exception e)
{
    // Handle exception
}
```

Dizin oluşturucunun oluşturulması için biraz zaman süreceğini beklemeniz gerekir. Veri kümesi küçük olsa da, analitik yetenekler hesaplama açısından yoğun bir işlemdir. Görüntü analizi gibi bazı yetenekler uzun süredir çalışıyor.

> [!TIP]
> Dizin Oluşturucu oluşturma işlem hattını çağırır. Verilere ulaşan sorunlar varsa, girdileri ve çıkışları eşleme ya da işlem sırası varsa, bu aşamada görünürler.

### <a name="explore-creating-the-indexer"></a>Dizin oluşturucuyu oluşturmayı keşfet

Kod ```"maxFailedItems"```-1 olarak ayarlanır. Bu, dizin oluşturma altyapısının veri içeri aktarma sırasında hataları yoksaymasına olanak verir. Bu, tanıtım verileri kaynağında çok az sayıda belge olduğu için yararlıdır. Daha büyük bir veri kaynağı için değeri 0 ' dan büyük olarak ayarlarsınız.

Ayrıca ```"dataToExtract"``` ' ın ```"contentAndMetadata"``` olarak ayarlandığını fark edebilirsiniz. Bu ifade, dizin oluşturucunun içeriği farklı dosya biçimlerinden ve her bir dosyayla ilgili meta verilerin otomatik olarak ayıklamasını söyler.

İçerik ayıklandığında, veri kaynağında bulunan görüntülerden metin ayıklamak için `imageAction` ayarlayabilirsiniz. @No__t-0, OCR yeteneği ve metin birleştirme beceriyle birlikte ```"generateNormalizedImages"``` yapılandırması olarak ayarlanmıştır, dizin oluşturucunun görüntüden metin ayıklamasını söyler (örneğin, bir trafiğin "Durdur" sözcüğü, oturum açma durdurma işareti) ve içeriği içerik alanının bir parçası olarak katıştırın. Bu davranış, bir JPG dosyası gibi, belgelerde (PDF içindeki bir görüntüyü göz önünde bulundurun) ve veri kaynağında bulunan görüntüleri de katıştırılmış görüntüler için geçerlidir.

## <a name="check-indexer-status"></a>Dizin Oluşturucu durumunu denetle

Dizin Oluşturucu tanımlandıktan sonra, isteği gönderdiğinizde otomatik olarak çalışır. Tanımladığınız bilişsel becerilere bağlı olarak, dizin oluşturma beklediğinizden daha uzun sürebilir. Dizin oluşturucunun hala çalışır durumda olup olmadığını öğrenmek için `GetStatus` yöntemini kullanın.

```csharp
try
{
    IndexerExecutionInfo demoIndexerExecutionInfo = serviceClient.Indexers.GetStatus(indexer.Name);

    switch (demoIndexerExecutionInfo.Status)
    {
        case IndexerStatus.Error:
            Console.WriteLine("Indexer has error status");
            break;
        case IndexerStatus.Running:
            Console.WriteLine("Indexer is running");
            break;
        case IndexerStatus.Unknown:
            Console.WriteLine("Indexer status is unknown");
            break;
        default:
            Console.WriteLine("No indexer information");
            break;
    }
}
catch (Exception e)
{
    // Handle exception
}
```

`IndexerExecutionInfo`, bir dizin oluşturucunun geçerli durum ve yürütme geçmişini temsil eder.

Uyarılar, bazı kaynak dosya ve yetenek birleşimleri ile ortaktır ve her zaman bir sorun göstermez. Bu öğreticide, uyarılar zararsız (örneğin, JPEG dosyalarından metin girişi yoktur).
 
## <a name="query-your-index"></a>Dizininizi sorgulama

Dizin oluşturma işlemi tamamlandıktan sonra, tek tek alanların içeriğini döndüren sorguları çalıştırabilirsiniz. Varsayılan olarak, Azure Search en üstteki 50 sonucunu döndürür. Örnek veriler küçüktür, bu nedenle varsayılan değer iyi bir şekilde çalışacaktır. Ancak, daha büyük veri kümeleriyle çalışırken, daha fazla sonuç döndürmek için sorgu dizesine parametreleri eklemeniz gerekebilir. Yönergeler için bkz. [nasıl yapılır sayfa sonuçları Azure Search](search-pagination-page-layout.md).

Doğrulama adımı olarak, tüm alanların dizinini sorgulayın.

```csharp
DocumentSearchResult<DemoIndex> results;

ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

try
{
    results = indexClientForQueries.Documents.Search<DemoIndex>("*");
}
catch (Exception e)
{
    // Handle exception
}
```

`CreateSearchIndexClient`, uygulamanın yapılandırma dosyasında (appSettings. JSON) depolanan değerleri kullanarak yeni bir `SearchIndexClient` oluşturur. Yönetim anahtarı için arama hizmeti sorgu API anahtarının kullanıldığını ve olduğunu unutmayın.

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
   string searchServiceName = configuration["SearchServiceName"];
   string queryApiKey = configuration["SearchServiceQueryApiKey"];

   SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "demoindex", new SearchCredentials(queryApiKey));
   return indexClient;
}
```

Çıktı, her alanın adı, türü ve öznitelikleri ile dizin şemadır.

@No__t-1 gibi tek bir alanın tüm içeriğini döndürmek için `"*"` için ikinci bir sorgu gönder.

```csharp
SearchParameters parameters =
    new SearchParameters
    {
        Select = new[] { "organizations" }
    };

try
{
    results = indexClientForQueries.Documents.Search<DemoIndex>("*", parameters);
}
catch (Exception e)
{
    // Handle exception
}
```

Bu alıştırmada içerik, languageCode, keyPhrases ve kuruluşlar için yineleyin. Virgülle ayrılmış bir liste kullanarak birden çok alanı `$select` yoluyla döndürebilirsiniz.

<a name="reset"></a>

## <a name="reset-and-rerun"></a>Sıfırla ve yeniden çalıştır

Geliştirmede erken deneysel aşamalarda, tasarım yinelemeleri için en pratik yaklaşım, nesneleri Azure Search silmek ve kodunuzun bunları yeniden oluşturması için izin verir. Kaynak adları benzersizdir. Bir nesne silindiğinde, aynı adı kullanarak yeniden oluşturmanız kolaylaşır.

Bu öğreticide, var olan dizin oluşturucularının ve dizinlerin denetlenmesi ve kodunuzu yeniden çalıştırabilmeniz için zaten mevcut olmaları durumunda silinmeleri ele alınır.

Ayrıca, dizinleri, Dizin oluşturucuyu ve becerileri silmek için portalını de kullanabilirsiniz.

Kodunuz geliştikçe, yeniden oluşturma stratejisini iyileştirmek isteyebilirsiniz. Daha fazla bilgi için bkz. [Dizin oluşturma](search-howto-reindex.md).

## <a name="takeaways"></a>Dersler

Bu öğreticide, bileşen bölümlerinin oluşturulması yoluyla zenginleştirilmiş bir dizin oluşturma işlem hattı oluşturmaya yönelik temel adımlar gösterilmiştir: bir veri kaynağı, Beceri, dizin ve Dizin Oluşturucu.

[Önceden tanımlanmış yetenekler](cognitive-search-predefined-skills.md) , Beceri tanımıyla birlikte sunulan beceriler ve girişler ve çıktılar aracılığıyla bir araya getirilen becerilerin yanı sıra tanıtılmıştır. Ayrıca, işlem hattı tanımındaki `outputFieldMappings` ' ın işlem hattından Azure Search bir hizmette aranabilir bir dizine yönlendirilmesini sağlayan bir dizin oluşturma için gerekli olduğunu öğrendiniz.

Son olarak, sonuçları test etme ve daha fazla yineleme için sistemi sıfırlama hakkında öğrenirsiniz. Dizine yönelik sorgu verme işleminin, zenginleştirilmiş dizin oluşturma işlem hattı tarafından oluşturulan çıktıyı döndürdüğünü öğrendiniz. Ayrıca, Dizin Oluşturucu durumunu denetlemeyi ve bir işlem hattını yeniden çalıştırmadan önce hangi nesnelerin silineceğini öğrenirsiniz.

## <a name="clean-up-resources"></a>Kaynakları Temizleme

Öğreticiden sonra temizlemenin en hızlı yolu, Azure Search hizmetini ve Azure Blob hizmetini içeren kaynak grubunu silmelidir. Her iki hizmeti de aynı gruba yerleştirolduğunuza göre, bu öğreticide oluşturduğunuz hizmetler ve depolanan tüm içerikler dahil olmak üzere kaynak grubunu şimdi silin. Portalda, kaynak grubu adı her bir hizmetin genel bakış sayfaalıdır.

## <a name="next-steps"></a>Sonraki adımlar

İşlem hattını özel yetenekler ile özelleştirin veya genişletin. Özel bir beceri oluşturmak ve bunu bir beceri eklemek, kendi yazdığınız metin veya görüntü analizini eklemenizi sağlar.

> [!div class="nextstepaction"]
> [Örnek: bilişsel arama için özel bir yetenek oluşturma](cognitive-search-create-custom-skill-example.md)
