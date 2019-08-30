---
title: C#Dizin oluşturma işlem hattında Bilişsel Hizmetler API'si çağırma öğreticisi-Azure Search
description: Bu öğreticide, Azure Search veri ayıklama için dizin oluşturma ve dönüştürme işlemindeki veri ayıklama, doğal dil ve görüntü AI işleme örneği adımlarını izleyin.
manager: nitinme
author: MarkHeff
services: search
ms.service: search
ms.subservice: cognitive-search
ms.topic: tutorial
ms.date: 05/02/2019
ms.author: maheff
ms.openlocfilehash: ad0710a3492eeecd56989e627211da5d4a5e0e0a
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70186246"
---
# <a name="c-tutorial-call-cognitive-services-apis-in-an-azure-search-indexing-pipeline"></a>C#Öğreticide Azure Search dizin oluşturma ardışık düzeninde Bilişsel Hizmetler API'si çağrısı

Bu öğreticide, *bilişsel becerileri* kullanarak Azure Search’te programlama veri zenginleştirmesi mekanizmasını öğrenirsiniz. Yetenekler, bilişsel hizmetler 'de doğal dil işleme (NLP) ve görüntü analizi özellikleri tarafından desteklenir. Beceri kompozisyonu ve yapılandırması aracılığıyla bir görüntünün veya taranan belge dosyasının metin ve metin gösterimlerini ayıklayabilirsiniz. Ayrıca dili, varlıkları, anahtar tümcecikleri ve daha fazlasını tespit edebilirsiniz. Nihai sonuç, AI destekli bir dizin oluşturma işlem hattı tarafından oluşturulan Azure Search dizinde zengin ek içerik sağlar.

Bu öğreticide, .NET SDK 'yı kullanarak aşağıdaki görevleri gerçekleştirebilirsiniz:

> [!div class="checklist"]
> * Bir dizine giden yolda örnek verileri zenginleştiren dizin oluşturma işlem hattı oluşturma
> * Yerleşik becerileri Uygula: optik karakter tanıma, metin merği, dil algılama, metin bölme, varlık tanıma, anahtar tümceciği ayıklama
> * Bir beceri kümesindeki çıktılara girişleri eşleyerek becerilerin nasıl birbirine zincirleneceğini öğrenme
> * İstekleri yürütme ve sonuçları gözden geçirme
> * Daha fazla geliştirme için dizini ve dizin oluşturucuları sıfırlama

Çıktı, Azure Search üzerinde tam metin araması yapılabilir bir dizindir. [Eş anlamlılar](search-synonyms.md), [puanlama profilleri](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [çözümleyiciler](search-analyzers.md) ve [filtreler](search-filters.md) gibi diğer standart özelliklerle dizini geliştirebilirsiniz.

Bu öğretici ücretsiz hizmette çalışır, ancak ücretsiz işlem sayısı günde 20 belge ile sınırlıdır. Bu öğreticiyi aynı günde birden çok kez çalıştırmak istiyorsanız, daha fazla çalıştırmaya uyabilmeniz için daha küçük bir dosya kümesi kullanın.

> [!NOTE]
> İşlem sıklığını artırarak, daha fazla belge ekleyerek veya daha fazla AI algoritması ekleyerek kapsamı genişlettikten sonra faturalandırılabilir bilişsel hizmetler kaynağı eklemeniz gerekir. Bilişsel hizmetlerde API 'Leri çağırırken ve Azure Search içinde belge çözme aşamasının bir parçası olarak görüntü ayıklama için tahakkuk ücretleri. Belgelerden metin ayıklama için herhangi bir ücret alınmaz.
>
> Yerleşik yeteneklerin yürütülmesi, mevcut bilişsel [Hizmetler Kullandıkça Öde fiyatı](https://azure.microsoft.com/pricing/details/cognitive-services/) üzerinden ücretlendirilir. Görüntü ayıklama fiyatlandırması [Azure Search fiyatlandırma sayfasında](https://go.microsoft.com/fwlink/?linkid=2042400)açıklanmaktadır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki hizmetler, Araçlar ve veriler bu öğreticide kullanılır. 

+ Örnek verileri depolamak için [bir Azure depolama hesabı oluşturun](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) . Depolama hesabının Azure Search aynı bölgede bulunduğundan emin olun.

+ [Örnek veriler](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4) , küçük bir dosya kümesi farklı türlerden oluşur. 

+ IDE olarak kullanmak için [Visual Studio 'Yu yükler](https://visualstudio.microsoft.com/) .

+ Geçerli aboneliğinizde [bir Azure Search hizmeti oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) . Bu öğretici için ücretsiz bir hizmet kullanabilirsiniz.

## <a name="get-a-key-and-url"></a>Anahtar ve URL al

Azure Search hizmetinize etkileşimde bulunmak için hizmet URL 'SI ve erişim anahtarı gerekir. İkisini de içeren bir arama hizmeti oluşturulur. Bu nedenle aboneliğinize Azure Search hizmetini eklediyseniz gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure Portal oturum açın](https://portal.azure.com/)ve arama hizmetine **genel bakış** sayfasında URL 'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. **Ayarlar** > **anahtarlar**' da, hizmette tam haklar için bir yönetici anahtarı alın. Üzerinde bir tane almanız gereken iş sürekliliği için iki adet değiştirilebilir yönetici anahtarı vardır. Nesneleri eklemek, değiştirmek ve silmek için isteklerde birincil veya ikincil anahtarı kullanabilirsiniz.

   ![Http uç noktası ve erişim anahtarı al](media/search-get-started-postman/get-url-key.png "Http uç noktası ve erişim anahtarı al")

İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="prepare-sample-data"></a>Örnek verileri hazırlama

Zenginleştirme işlem hattı, Azure veri kaynaklarından çekme işlemi yapar. Kaynak veriler, [Azure Search dizin oluşturucunun](search-indexer-overview.md) desteklenen bir veri kaynağı türünden gelmelidir. Azure Tablo Depolaması bilişsel arama için desteklenmiyor. Bu alıştırmada, birden çok içerik türünü göstermek için blob depolama kullanırız.

1. [Azure Portal oturum açın](https://portal.azure.com), Azure depolama hesabınıza gidin, Bloblar ' a tıklayınve ardından **+ Container**' a tıklayın.

1. Örnek veri içeren [bir blob kapsayıcısı oluşturun](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) . Ortak erişim düzeyini geçerli değerlerinden herhangi birine ayarlayabilirsiniz. Bu öğreticide kapsayıcı adının "temel-demo-Data-PR" olduğu varsayılır.

1. Kapsayıcı oluşturulduktan sonra dosyayı açın ve [örnek verileri](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4)karşıya yüklemek için komut çubuğunda **karşıya yükle** ' yi seçin.

   ![Azure blob depolamadaki kaynak dosyalar](./media/cognitive-search-quickstart-blob/sample-data.png)

1. Örnek dosyalar yüklendikten sonra Blob depolamanız için bir bağlantı dizesi ve kapsayıcı adını alın. Bunu, Azure portal depolama hesabınıza giderek, **erişim anahtarları**' nı seçerek ve ardından **bağlantı dizesi** alanını kopyalayarak yapabilirsiniz.

   Bağlantı dizesi, aşağıdaki örneğe benzer bir URL olmalıdır:

      ```http
      DefaultEndpointsProtocol=https;AccountName=cogsrchdemostorage;AccountKey=<your account key>;EndpointSuffix=core.windows.net
      ```

Paylaşılan erişim imzası sağlama gibi, bağlantı dizesini belirtmenin başka birçok yolu vardır. Veri kaynağı kimlik bilgileri hakkında daha fazla bilgi edinmek için bkz. [Azure Blob Depolama Alanı dizinini oluşturma](search-howto-indexing-azure-blob-storage.md#Credentials).

## <a name="set-up-your-environment"></a>Ortamınızı ayarlama

Visual Studio 'Yu açıp .NET Core üzerinde çalışabilen yeni bir konsol uygulama projesi oluşturarak başlayın.

### <a name="install-nuget-packages"></a>NuGet paketlerini yükleme

[Azure Search .NET SDK](https://aka.ms/search-sdk) , dizinlerinizi, veri kaynaklarınızı, Dizin Oluşturucularınızı ve becerileri yönetmenizi sağlayan birkaç istemci kitaplığı içerir. Ayrıca, http ve JSON ayrıntıları ile uğraşmak zorunda kalmadan belgeleri karşıya yükleyebilir ve yönetebilir ve sorguları yürütebilirsiniz. . Bu istemci kitaplıklarının hepsi NuGet paketleri olarak dağıtılır.

Bu proje için `Microsoft.Azure.Search` NuGet paketinin 9. sürümünü ve en son `Microsoft.Extensions.Configuration.Json` NuGet paketini yüklemeniz gerekir.

Visual Studio 'da Paket Yöneticisi konsolu 'nu kullanarak NuGetpaketiniyükler.`Microsoft.Azure.Search` Paket Yöneticisi konsolu 'nu açmak için **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin. Çalıştırılacak komutu almak için [Microsoft. Azure. Search NuGet paketi sayfasına](https://www.nuget.org/packages/Microsoft.Azure.Search)gidin, sürüm 9 ' ı seçin ve Paket Yöneticisi komutunu kopyalayın. Paket Yöneticisi konsolunda, bu komutu çalıştırın.

`Microsoft.Extensions.Configuration.Json` NuGet paketini Visual Studio 'ya yüklemek için **Araçlar** > **NuGet Paket Yöneticisi** > **çözüm için NuGet Paketlerini Yönet...** seçeneğini belirleyin. `Microsoft.Extensions.Configuration.Json` NuGet paketi için araştır ve ara ' yı seçin. Onu bulduktan sonra, paketi seçin, projenizi seçin, sürümün en son kararlı sürümü olduğunu onaylayın ve ardından Install ' ı seçin.

## <a name="add-azure-search-service-information"></a>Azure Search hizmeti bilgileri ekleme

Azure Search hizmetinize bağlanabilmek için, arama hizmeti bilgilerini projenize eklemeniz gerekir. Çözüm Gezgini projenize sağ tıklayıp yeni öğe **Ekle** >  **...** seçeneğini belirleyin. Dosyayı `appsettings.json` adlandırın ve **Ekle**' yi seçin. 

Bu dosyanın, çıkış dizininize dahil olması gerekir. Bunu yapmak için, sağ tıklayın `appsettings.json` ve **Özellikler**' i seçin. **Çıkış Dizinine Kopyala** değerini **daha yeni bir kopya**olacak şekilde değiştirin.

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

Arama hizmeti bilgilerinizi Azure portal arama hesabı sayfasından alabilirsiniz. Hesap adı ana sayfada yer alır ve anahtarlar seçilerek anahtarlar bulunabilir.

Blob bağlantı dizesini Azure portal, **erişim anahtarları**' nı seçerek ve ardından **bağlantı dizesi** alanını kopyalayarak depolama hesabınıza giderek alabilirsiniz.

## <a name="add-namespaces"></a>Ad alanı Ekle

Bu öğretici çeşitli ad alanlarından birçok farklı tür kullanır. Bu türleri kullanabilmek için aşağıdakileri öğesine `Program.cs`ekleyin.

```csharp
using System;
using System.Collections.Generic;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Extensions.Configuration;
```

## <a name="create-a-client"></a>İstemci oluşturma

`SearchServiceClient` Sınıfının bir örneğini oluşturun.

```csharp
IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
IConfigurationRoot configuration = builder.Build();
SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);
```

`CreateSearchServiceClient`uygulamanın yapılandırma dosyasında `SearchServiceClient` (appSettings. JSON) depolanan değerleri kullanarak yeni bir oluşturur.

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
> `SearchServiceClient` sınıfı, arama hizmetinize yönelik bağlantıları yönetir. Çok fazla bağlantı açmayı önlemek için, mümkünse uygulamanızda tek bir `SearchServiceClient` örneği paylaşmaya çalışmanız gerekir. Yöntemlerinin iş parçacığı bu tür paylaşımları etkinleştirmek için güvenlidir.
> 
> 

## <a name="create-a-data-source"></a>Bir veri kaynağı oluşturun

`DataSource` Çağırarak`DataSource.AzureBlobStorage`yeni bir örnek oluşturun. `DataSource.AzureBlobStorage`veri kaynağı adını, bağlantı dizesini ve BLOB kapsayıcısı adını belirtmenizi gerektirir.

Bu öğreticide kullanılmıyor olsa da, bir geçici silme ilkesi, silinen Blobları bir geçici silme sütununun değerine göre belirlemek için kullanılır. Aşağıdaki ilke, bir blob 'un değeri `IsDeleted` `true`olan bir Metadata özelliği varsa, silinecek bir blobu kabul eder.

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

`DataSource` Nesneyi artık başlattığını, veri kaynağını oluşturun. `SearchServiceClient`, `DataSources` özelliğine sahiptir. Bu özellik Azure Search veri kaynaklarını oluşturmak, listelemek, güncelleştirmek veya silmek için gereken tüm yöntemleri sağlar.

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

Bu ilk isteğiniz olduğundan, veri kaynağının Azure Search’te oluşturulduğunu onaylamak için Azure portalını denetleyin. Arama hizmeti panosu sayfasında, Veri Kaynakları kutucuğunun yeni bir öğe içerdiğini doğrulayın. Portal sayfasının yenilenmesi için birkaç dakika beklemeniz gerekebilir.

  ![Portaldaki veri kaynakları kutucuğu](./media/cognitive-search-tutorial-blob/data-source-tile.png "Portaldaki veri kaynakları kutucuğu")

## <a name="create-a-skillset"></a>Beceri kümesi oluşturma

Bu bölümde, verilerinize uygulamak istediğiniz bir zenginleştirme adımları kümesi tanımlarsınız. Her bir zenginleştirme adımına bir *yetenek* ve bir *beceri*için bir dizi adım denir. Bu öğreticide, beceri kümesi için [önceden tanımlanmış bilişsel beceriler](cognitive-search-predefined-skills.md) kullanılmaktadır:

+ Görüntü dosyalarında yazdırılmış ve el yazısı metinleri tanımak için [optik karakter tanıma](cognitive-search-skill-ocr.md) .

+ Bir alanlar koleksiyonundan tek bir alanda metin birleştirmek için [metin Merkli](cognitive-search-skill-textmerger.md) .

+ İçeriğin dilini tanımlamak için [Dil Algılama](cognitive-search-skill-language-detection.md).

+ Anahtar ifadesi ayıklama yeteneği ve varlık tanıma yeteneği çağrılmadan önce büyük içeriği daha küçük parçalara bölmek için [metin bölme](cognitive-search-skill-textsplit.md) . Anahtar tümceciği ayıklama ve varlık tanıma, 50.000 veya daha az karakter girişi kabul eder. Bu sınıra uymak için örnek dosyaların birkaç tanesinin bölünmesi gerekir.

+ Blob kapsayıcısında bulunan içerikten kuruluşların adlarını ayıklamak için [varlık tanıma](cognitive-search-skill-entity-recognition.md) .

+ Üst anahtar tümcecikleri çekmek için [Anahtar İfade Ayıklama](cognitive-search-skill-keyphrases.md).

İlk işlem sırasında, Azure Search her belgeyi, farklı dosya biçimlerinden içeriği okumak için bir kez daha gizler. Kaynak dosyadan gelen, bulunan metin, oluşturulan ```content``` alanına her belge için birer birer yerleştirilir. Bu nedenle, bu metni kullanmak için olarak ```"/document/content"``` girdisini ayarlayın. 

Çıktılar bir dizine eşlenebilir, aşağı akış becerisine yönelik giriş olarak kullanılır veya dil kodunda olduğu gibi her iki şekilde de kullanılabilir. Dizinde bir dil kodu, filtreleme için yararlıdır. Giriş olarak dil kodu, sözcük bölünmesiyle ilgili dilbilgisi kurallarını bildirmek için metin analizi becerileri tarafından kullanılır.

Beceri kümesi temelleri hakkında daha fazla bilgi için bkz. [Beceri kümesini tanımlama](cognitive-search-defining-skillset.md).

### <a name="ocr-skill"></a>OCR becerisi

**OCR** becerisi görüntülerden metin ayıklar. Bu beceri bir normalized_images alanının bulunduğunu varsayar. Bu alanı oluşturmak için, öğreticide daha sonra, Dizin Oluşturucu tanımındaki ```"imageAction"``` yapılandırmayı olarak ```"generateNormalizedImages"```ayarlayacağız.

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

Aşağıdaki **bölünmüş** Beceri, metni sayfalara göre böler ve sayfa uzunluğu ile `String.Length`4.000 karakter arasında bir şekilde sınırlayacaktır. Algoritma, metni en fazla `maximumPageLength` boyuttaki parçalara bölmeye çalışır. Bu durumda, algoritma bir cümle sınırında tümceyi bölmek en iyi şekilde yapılır, bu nedenle öbek boyutu daha az `maximumPageLength`olabilir.

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

Bu `EntityRecognitionSkill` örnek kategori türünü `organization`tanımak üzere ayarlanır. **Varlık tanıma** yeteneği de kategori türlerini `person` ve `location`kullanabilirsiniz.

"Bağlam" alanının bir yıldız işaretiyle ayarlanmış ```"/document/pages/*"``` olduğuna ve altındaki ```"/document/pages"```her sayfa için zenginleştirme adımının çağrıldığı anlamına gelir.

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

Yeni oluşturulan örnek gibi anahtar ifade ayıklama Beceri, belgenin her sayfası için çağrılır. `EntityRecognitionSkill`

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

Oluşturduğunuz becerileri `Skillset` kullanarak oluşturun.

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

Bu bölümde, aranabilir dizine dahil edilecek alanları ve her bir alana ilişkin arama özniteliklerini belirterek dizin şemasını tanımlarsınız. Alanlar bir türe sahiptir ve alanın nasıl kullanıldığını (aranabilir, sıralanabilir vb.) belirleyen öznitelikleri alabilir. Bir dizindeki alan adlarının, kaynaktaki alan adlarıyla tamamen aynı olması gerekmez. Sonraki bir adımda, kaynak-hedef alanlarını bağlamak için dizin oluşturucuda alan eşlemeleri eklersiniz. Bu adım için, arama uygulamanızla ilgili alan adlandırma kurallarını kullanarak dizini tanımlayın.

Bu çalışmada aşağıdaki alanlar ve alan türleri kullanılır:

| alan adları: | `id`       | content   | languageCode | keyPhrases         | organizations     |
|--------------|----------|-------|----------|--------------------|-------------------|
| field-types: | Edm.String|Edm.String| Edm.String| List<Edm.String>  | List<Edm.String>  |


### <a name="create-demoindex-class"></a>Demoındex sınıfı oluşturma

Bu dizinin alanları bir model sınıfı kullanılarak tanımlanır. Model sınıfının her özelliği karşılık gelen dizin alanının aramayla ilgili davranışlarını belirleyen özniteliklere sahiptir. 

Model sınıfını yeni C# bir dosyaya ekleyeceğiz. Projenize sağ tıklayın ve yeni öğe **Ekle** >  **...** seçeneğini belirleyin, "sınıf" seçeneğini belirleyip dosyayı `DemoIndex.cs`adlandırın ve ardından **Ekle**' yi seçin.

`Microsoft.Azure.Search` Ve`Microsoft.Azure.Search.Models` ad alanlarından türleri kullanmak istediğinizi belirttiğinizden emin olun.

```csharp
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
```

Aşağıdaki model sınıfı tanımını öğesine `DemoIndex.cs` ekleyin ve dizini oluşturacağınız aynı ad alanına ekleyin.

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

Artık bir model sınıfı tanımladığınıza göre, bir dizin tanımını `Program.cs` oldukça kolay bir şekilde oluşturabilirsiniz. Bu dizinin adı "demoındex" olacaktır.

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

Dizin tanımlama hakkında daha fazla bilgi için bkz. [Dizin Oluşturma (Azure Search REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index).

## <a name="create-an-indexer-map-fields-and-execute-transformations"></a>Dizin oluşturucu oluşturma, alanları eşleme ve dönüştürmeler yürütme

Şu ana kadar bir veri kaynağı, beceri kümesi ve dizin oluşturdunuz. Bu üç bileşen, her bir parçayı birlikte tek bir çok aşamalı işleme çeken [dizin oluşturucunun](search-indexer-overview.md) parçası olur. Bunları bir dizin oluşturucu birbirine bağlamak için alan eşlemeleri tanımlamanız gerekir.

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

Dizin oluşturucunun oluşturulması için biraz zaman süreceğini beklemeniz gerekir. Veri kümesi küçük olsa da, analiz becerileri bilgi işlem açısından yoğundur. Görüntü analizi gibi bazı beceriler uzun sürer.

> [!TIP]
> Bir dizin oluşturucu oluşturulduğunda, işlem hattı çağrılır. Verilere ulaşılırken, eşleme girişleri ve çıktıları veya işlemlerin sırası ile ilgili sorun olursa bunlar bu aşamada görüntülenir.

### <a name="explore-creating-the-indexer"></a>Dizin oluşturucuyu oluşturmayı keşfet

Kodu-1 ```"maxFailedItems"``` olarak ayarlanır. Bu, dizin oluşturma altyapısının veri içeri aktarma sırasında hataları yoksaymasına olanak verir. Demo veri kaynağında çok az belge olduğundan bu yararlıdır. Daha büyük bir veri kaynağı için değeri, 0’dan daha büyük bir değere ayarlarsınız.

Ayrıca, ```"dataToExtract"``` olarak ```"contentAndMetadata"```ayarlandığını unutmayın. Bu deyim, dizin oluşturucuya, farklı dosya biçimlerinden içeriği ve her bir dosyayla ilgili meta verileri otomatik olarak ayıklamasını bildirir.

İçerik ayıklandığında, veri kaynağında bulunan görüntülerden metni ayıklamak için `imageAction` değerini ayarlayabilirsiniz. Yapılandırma ```"imageAction"``` olarak```"generateNormalizedImages"``` ayarlanan, OCR yeteneği ve metin birleştirme beceriyle birlikte, dizin oluşturucunun görüntüden metin ayıklamasını söyler (örneğin, trafiğin "Durdur" sözcüğünün oturum açmasını durdurur) ve içerik alanının bir parçası olarak katıştırmasını söyler. Bu davranış hem belgelerde gömülü olan görüntüler (örneğin, bir PDF’teki görüntü) hem de veri kaynağında bulunan görüntüler (örneğin, bir JPG dosyası) için geçerlidir.

## <a name="check-indexer-status"></a>Dizin oluşturucu durumunu denetleme

Dizin oluşturucu tanımlandıktan sonra, isteği gönderdiğinizde otomatik olarak çalıştırılır. Tanımladığınız bilişsel becerilere bağlı olarak dizin oluşturma beklediğinizden uzun sürebilir. Dizin oluşturucunun hala çalışır durumda olup olmadığını öğrenmek için `GetStatus` yöntemini kullanın.

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

`IndexerExecutionInfo`bir dizin oluşturucunun geçerli durum ve yürütme geçmişini temsil eder.

Uyarılar bazı kaynak dosya ve beceri birleşimleri için geneldir ve her zaman bir sorunu belirtmez. Bu öğreticide, uyarılar zararsızdır (örneğin, JPEG dosyalarında bir metin girişi yok).
 
## <a name="query-your-index"></a>Dizininizi sorgulama

Dizin oluşturma işlemi tamamlandıktan sonra, tek tek alanların içeriğini döndüren sorguları çalıştırabilirsiniz. Azure Search varsayılan olarak ilk 50 sonucu döndürür. Varsayılan değerin düzgün çalışması için örnek veriler küçüktür. Ancak, büyük veri kümeleriyle çalışırken, daha fazla sonuç döndürmek için sorgu dizesine parametreleri dahil etmeniz gerekebilir. Yönergeler için bkz. [Azure Search’te sonuçların sayfasını oluşturma](search-pagination-page-layout.md).

Doğrulama adımı olarak, tüm alanlar için dizini sorgulayın.

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

`CreateSearchIndexClient`uygulamanın yapılandırma dosyasında `SearchIndexClient` (appSettings. JSON) depolanan değerleri kullanarak yeni bir oluşturur. Yönetim anahtarı için arama hizmeti sorgu API anahtarının kullanıldığını ve olduğunu unutmayın.

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
   string searchServiceName = configuration["SearchServiceName"];
   string queryApiKey = configuration["SearchServiceQueryApiKey"];

   SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "demoindex", new SearchCredentials(queryApiKey));
   return indexClient;
}
```

Çıktı, her bir alanın adını, türünü ve özniteliklerini içeren dizin şemasıdır.

`organizations` gibi tek bir alanın tüm içeriklerini döndürmek için ikinci bir `"*"` sorgusu gönderin.

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

Bu alıştırmada içerik, languageCode, keyPhrases ve kuruluşlar için yineleyin. Virgülle ayrılmış bir liste kullanarak `$select` aracılığıyla birden fazla alan döndürebilirsiniz.

<a name="reset"></a>

## <a name="reset-and-rerun"></a>Sıfırlama ve yeniden çalıştırma

Geliştirmede erken deneysel aşamalarda, tasarım yinelemeleri için en pratik yaklaşım, nesneleri Azure Search silmek ve kodunuzun bunları yeniden oluşturması için izin verir. Kaynak adları benzersizdir. Bir nesneyi sildiğinizde, aynı adı kullanarak nesneyi yeniden oluşturabilirsiniz.

Bu öğreticide, var olan dizin oluşturucularının ve dizinlerin denetlenmesi ve kodunuzu yeniden çalıştırabilmeniz için zaten mevcut olmaları durumunda silinmeleri ele alınır.

Ayrıca, dizinleri, Dizin oluşturucuyu ve becerileri silmek için portalını de kullanabilirsiniz.

Kodunuz geliştikçe bir yeniden derleme stratejisini iyileştirmek isteyebilirsiniz. Daha fazla bilgi için bkz. [Yeniden dizin derleme](search-howto-reindex.md).

## <a name="takeaways"></a>Paketler

Bu öğreticide, bileşen bölümlerinin oluşturulması yoluyla zenginleştirilmiş bir dizin oluşturma işlem hattı oluşturmaya yönelik temel adımlar gösterilmiştir: bir veri kaynağı, Beceri, dizin ve Dizin Oluşturucu.

Girişler ve çıktılar yoluyla becerileri zincirleme mekanizmalarının ve beceri kümesi tanımının yanı sıra [önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md) sunulmuştur. İşlem hattındaki zenginleştirilmiş değerleri bir Azure Search hizmetinde aranabilir bir dizine yönlendirmek için dizin oluşturucu tanımında `outputFieldMappings` gerektiğini öğrendiniz.

Son olarak, daha fazla yineleme için sonuçların nasıl test edileceğini ve sistemin nasıl sıfırlanacağını öğrendiniz. Dizine karşı sorgular düzenlendiğinde, zenginleştirilmiş dizin oluşturma işlem hattı tarafından oluşturulan çıktının döndürüldüğünü öğrendiniz. Ayrıca dizin oluşturucu durumunun nasıl denetleneceğini ve işlem hattı yeniden çalıştırılmadan önce hangi nesnelerin silineceğini de öğrendiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir öğretici tamamlandıktan sonra temizlemenin en hızlı yolu, Azure Search hizmetini ve Azure Blob hizmetini içeren kaynak grubunu silmektir. Her iki hizmeti de aynı gruba koyduğunuz varsayılarak, şimdi bu öğreticide oluşturduğunuz depolanan içerikler ve hizmetler de dahil olmak üzere, kaynak grubunun içindeki her şeyi silmek için kaynak grubunu silin. Portalda kaynak grubu adı, her bir hizmetin Genel Bakış sayfasındadır.

## <a name="next-steps"></a>Sonraki adımlar

Özel becerilerle işlem hattını özelleştirin veya genişletin. Özel bir beceri oluşturup bir beceri kümesine eklemeniz, kendi yazdığınız metin veya görüntü analizini eklemenize olanak sağlar.

> [!div class="nextstepaction"]
> [Örnek: Bilişsel arama için özel bir yetenek oluşturma](cognitive-search-create-custom-skill-example.md)
