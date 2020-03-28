---
title: 'Öğretici: Azure lekeleri üzerinde C# ve AI'
titleSuffix: Azure Cognitive Search
description: C# ve Azure Bilişsel Arama .NET SDK'yı kullanarak Blob depolamadaki içerik üzerinden metin çıkarma ve doğal dil işleme örneğini inceleyerek.
manager: nitinme
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 02/27/2020
ms.openlocfilehash: 169a33d12e98235dcb4e4f317dbb8d91eb7446a4
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78851144"
---
# <a name="tutorial-use-c-and-ai-to-generate-searchable-content-from-azure-blobs"></a>Öğretici: Azure lekelerinden aranabilir içerik oluşturmak için C# ve AI'yi kullanın

Azure Blob depolama alanında yapılandırılmamış metin veya resimlervarsa, bir [AI zenginleştirme ardışık aygıtı](cognitive-search-concept-intro.md) bilgi ayıklayabilir ve tam metin arama veya bilgi madenciliği senaryoları için yararlı olan yeni içerik oluşturabilir. Bu C# öğreticisinde, görüntülere Optik Karakter Tanıma (OCR) uygulayın ve sorgularda, farklı şekillerde ve filtrelerde yararlanabileceğiniz yeni alanlar oluşturmak için doğal dil işleme gerçekleştirin.

Bu öğretici, aşağıdaki görevleri gerçekleştirmek için C# ve [.NET SDK'yı](https://aka.ms/search-sdk) kullanır:

> [!div class="checklist"]
> * Azure Blob depolama alanında uygulama dosyaları ve görüntüleriyle başlayın.
> * OCR, metin çıkarma, dil algılama, varlık ve anahtar sözcük tanıma eklemek için bir ardışık sözcülük tanımlayın.
> * Çıktıyı depolamak için bir dizin tanımlayın (ham içerik, artı boru hattı tarafından oluşturulan ad değeri çiftleri).
> * Dönüşümleri ve çözümlemesi başlatmak ve dizini oluşturmak ve yüklemek için ardışık hattı çalıştırın.
> * Tam metin aramave zengin bir sorgu sözdizimini kullanarak sonuçları keşfedin.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap açın.

## <a name="prerequisites"></a>Ön koşullar

+ [Azure Depolama](https://azure.microsoft.com/services/storage/)
+ [Visual Studio](https://visualstudio.microsoft.com/downloads/)
+ [Varolan bir arama hizmeti](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) [oluşturma](search-create-service-portal.md) veya bulma 

> [!Note]
> Bu öğretici için ücretsiz hizmeti kullanabilirsiniz. Ücretsiz bir arama hizmeti sizi üç dizin, üç dizin leyici ve üç veri kaynağıyla sınırlar. Bu öğreticide hepsinden birer tane oluşturulur. Başlamadan önce, yeni kaynakları kabul etmek için hizmetinizde yer olduğundan emin olun.

## <a name="download-files"></a>Dosyaları indirme

1. Bu [OneDrive klasörünü](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4) açın ve sol üst köşede dosyaları bilgisayarınıza kopyalamak için **İndir'i** tıklatın. 

1. Zip dosyasına sağ tıklayın ve **Tümünü Ayıkla'yı**seçin. Çeşitli türlerde 14 dosya vardır. Bu öğretici için hepsini kullanın.

## <a name="1---create-services"></a>1 - Hizmet oluşturma

Bu öğretici, dizin oluşturma ve sorgular için Azure Bilişsel Arama, AI zenginleştirme için arka uçtaki Bilişsel Hizmetler ve verileri sağlamak için Azure Blob depolamasını kullanır. Bu öğretici, Bilişsel Hizmetler'de her gün endeksleyici başına 20 işlemin ücretsiz tahsisi altında kalır, bu nedenle oluşturmanız gereken tek hizmetler arama ve depolamadır.

Mümkünse, yakınlık ve yönetilebilirlik için hem aynı bölgede hem de kaynak grubunda oluşturun. Uygulamada, Azure Depolama hesabınız herhangi bir bölgede olabilir.

### <a name="start-with-azure-storage"></a>Azure Depolama ile başlayın

1. [Azure portalında oturum açın](https://portal.azure.com/) ve **+ Kaynak Oluştur'a**tıklayın.

1. Depolama *hesabı* arayın ve Microsoft'un Depolama Hesabı teklifini seçin.

   ![Depolama hesabı oluşturma](media/cognitive-search-tutorial-blob/storage-account.png "Depolama hesabı oluşturma")

1. Temel Bilgiler sekmesinde aşağıdaki öğeler gereklidir. Diğer her şey için varsayılanları kabul edin.

   + **Kaynak grubu.** Varolan bir tane seçin veya yeni bir tane oluşturun, ancak toplu olarak yönetebilmeniz için tüm hizmetler için aynı grubu kullanın.

   + **Depolama hesabı adı.** Aynı türde birden fazla kaynağınız olabileceğini düşünüyorsanız, örneğin *blobstoragewestus*gibi türüne ve bölgesine göre ayrıştırmak için adı kullanın. 

   + **Konum**. Mümkünse, Azure Bilişsel Arama ve Bilişsel Hizmetler için kullanılan aynı konumu seçin. Tek bir konum bant genişliği ücretlerini geçersiz kılar.

   + **Hesap Türü**. Varsayılan, *StorageV2 (genel amaçlı v2)* seçin.

1. Hizmeti oluşturmak için **Gözden Geçir + Oluştur'u** tıklatın.

1. Oluşturulduktan sonra, Genel Bakış sayfasını açmak **için kaynağa git'i** tıklatın.

1. **Blobs** hizmetini tıklatın.

1. Bir kapsayıcı oluşturmak ve *temel-demo-data-pr*adını **+ Kapsayıcı** tıklayın.

1. *Temel demo-data-pr'ı* seçin ve ardından indirme dosyalarını kaydettiğiniz klasörü açmak için **Yükle'yi** tıklatın. On dört dosyanın tümünü seçin ve yüklemek için **Tamam'ı** tıklatın.

   ![Örnek dosyaları yükleme](media/cognitive-search-quickstart-blob/sample-data.png "Örnek dosyaları yükleme")

1. Azure Depolama'dan ayrılmadan önce, Azure Bilişsel Arama'da bağlantı formüle edebilmeniz için bir bağlantı dizesi alın. 

   1. Depolama hesabınızın Genel Bakış sayfasına göz atın (örnek olarak *blobstragewestus* kullandık). 
   
   1. Sol gezinti bölmesinde **Access tuşlarını** seçin ve bağlantı dizelerinden birini kopyalayın. 

   Bağlantı dizesi aşağıdaki örneğe benzer bir URL'dir:

      ```http
      DefaultEndpointsProtocol=https;AccountName=cogsrchdemostorage;AccountKey=<your account key>;EndpointSuffix=core.windows.net
      ```

1. Bağlantı dizesini Notepad'e kaydedin. Veri kaynağı bağlantısını kurarken daha sonra ihtiyacınız olacak.

### <a name="cognitive-services"></a>Bilişsel hizmetler

AI zenginleştirme, doğal dil ve görüntü işleme için Metin Analizi ve Bilgisayar Vizyonu da dahil olmak üzere Bilişsel Hizmetler tarafından desteklenmektedir. Amacınız gerçek bir prototipi veya projeyi tamamlamaksa, bu noktada Bilişsel Hizmetler'i (Azure Bilişsel Arama ile aynı bölgede) dizin oluşturma işlemlerine ekleyebilmeniz için sayılırdınız.

Ancak bu alıştırma için kaynak sağlama işlemini atlayabilirsiniz, çünkü Azure Bilişsel Arama arka planda Bilişsel Hizmetler'e bağlanabilir ve dizin oluşturma işlemi başına 20 ücretsiz işlem sağlayabilir. Bu öğretici 7 işlem kullandığından, ücretsiz ayırma yeterlidir. Daha büyük projeler için, bilişsel hizmetleri öde-as-you-go S0 katmanında sağlama yı planlayın. Daha fazla bilgi için [bkz.](cognitive-search-attach-cognitive-services.md)

### <a name="azure-cognitive-search"></a>Azure Bilişsel Arama

Üçüncü bileşen, [portalda oluşturabileceğiniz](search-create-service-portal.md)Azure Bilişsel Arama'dır. Bu izbiyi tamamlamak için Ücretsiz katmanı kullanabilirsiniz. 

### <a name="get-an-admin-api-key-and-url-for-azure-cognitive-search"></a>Azure Bilişsel Arama için yönetici api anahtarı ve URL'si alın

Azure Bilişsel Arama hizmetinizle etkileşimde kalmak için hizmet URL'sine ve erişim anahtarına ihtiyacınız olacaktır. Her ikisiyle de bir arama hizmeti oluşturulur, bu nedenle aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure portalında oturum açın](https://portal.azure.com/)ve arama hizmetinize **Genel Bakış** sayfanızda URL'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. **Ayarlar** > **Tuşları'nda,** hizmetteki tüm haklar için bir yönetici anahtarı alın. İki değiştirilebilir yönetici anahtarları, bir üzerinde rulo gerekir durumda iş sürekliliği için sağlanan vardır. Nesneleri ekleme, değiştirme ve silme isteklerinde birincil veya ikincil anahtarı kullanabilirsiniz.

   Sorgu anahtarını da alın. Salt okunur erişimle sorgu isteklerini vermek en iyi yöntemdir.

   ![Hizmet adını ve yöneticive sorgu anahtarlarını alın](media/search-get-started-nodejs/service-name-and-keys.png)

İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="2---set-up-your-environment"></a>2 - Ortamınızı ayarlayın

Visual Studio'yı açarak ve .NET Core'da çalıştırılabilen yeni bir Konsol Uygulaması projesi oluşturarak başlayın.

### <a name="install-nuget-packages"></a>NuGet paketlerini yükleme

[Azure Bilişsel Arama .NET SDK,](https://aka.ms/search-sdk) dizinlerinizi, veri kaynaklarınızı, dizin lerinizi ve beceri kümelerinizi yönetmenize ve http ve JSON ayrıntılarıyla uğraşmak zorunda kalmadan belgeleri yüklemenize ve yönetmenize ve sorguları yürütmenize olanak tanıyan birkaç istemci kitaplığından oluşur. Bu istemci kitaplıklarının tümü NuGet paketleri olarak dağıtılır.

Bu proje için NuGet paketinin `Microsoft.Azure.Search` 9 veya sonraki sürümünü yükleyin.

1. Paket Yöneticisi Konsolu'nu açın. **Araçları** > **nuget paket yöneticisi** > **paket yöneticisi konsolseçin.** 

1. [Microsoft.Azure.Search NuGet paket sayfasına](https://www.nuget.org/packages/Microsoft.Azure.Search)gidin.

1. En son sürümü seçin (9 veya daha sonra).

1. Paket Yöneticisi komutunu kopyalayın.

1. Paket Yöneticisi konsoluna dönün ve önceki adımda kopyaladığınız komutu çalıştırın.

Ardından, en `Microsoft.Extensions.Configuration.Json` son NuGet paketini yükleyin.

1. **Select Tools** > **NuGet Paket Yöneticisi** > **Çözüm için NuGet Paketlerini Yönetin...**. 

1. **NuGet paketine Gözat'ı** tıklayın ve arayın. `Microsoft.Extensions.Configuration.Json` 

1. Paketi seçin, projenizi seçin, sürümün en son kararlı sürüm olduğunu onaylayın, ardından **Yükle'yi**tıklatın.

### <a name="add-service-connection-information"></a>Servis bağlantısı bilgileri ekleme

1. Çözüm Gezgini'nde projenize sağ tıklayın ve > **Yeni Öğe Ekle'yi seçin...** seçeneğini belirleyin. **Add** 

1. Dosyayı `appsettings.json` adlandırın ve **Ekle'yi**seçin. 

1. Bu dosyayı çıktı dizine ekleyin.
    1. Sağ tıklayın `appsettings.json` ve **Özellikler**seçin. 
    1. Daha **yeniyse,** **Kopyadeğerini Çıktı Dizini** olarak değiştirin.

1. Aşağıdaki JSON dosyanızı yeni JSON dosyanıza kopyalayın.

    ```json
    {
      "SearchServiceName": "Put your search service name here",
      "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
      "SearchServiceQueryApiKey": "Put your query API key here",
      "AzureBlobConnectionString": "Put your Azure Blob connection string here",
    }
    ```

Arama hizmetinizi ve blob depolama hesap bilgilerinizi ekleyin. Bu bilgileri önceki bölümde belirtilen hizmet sağlama adımlarından alabileceğinizi hatırlayın.

### <a name="add-namespaces"></a>Ad alanları ekleme

In `Program.cs`, aşağıdaki ad boşluklarını ekleyin.

```csharp
using System;
using System.Collections.Generic;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Extensions.Configuration;

namespace EnrichwithAI
```

### <a name="create-a-client"></a>İstemci oluşturma

`Main`Altında `SearchServiceClient` sınıfın bir örneğini oluşturun.

```csharp
public static void Main(string[] args)
{
    // Create service client
    IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
    IConfigurationRoot configuration = builder.Build();
    SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);
```

`CreateSearchServiceClient`uygulamanın config dosyasında (appsettings.json) depolanan yeni `SearchServiceClient` bir kullanarak değerler oluşturur.

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

### <a name="add-function-to-exit-the-program-during-failure"></a>Hata sırasında programdan çıkmak için işlev ekleme

Bu öğretici, dizin oluşturma ardışık dizinin her adımını anlamanıza yardımcı olmak içindir. Programın veri kaynağı, skillset, index veya indexer oluşturmasını engelleyen kritik bir sorun varsa, program hata iletisi ve çıkış böylece sorunu anlaşılabilir ve ele çıktı çıktı.

Programın çıkmasını gerektiren senaryoları işlemek için ekleyin. `ExitProgram` `Main`

```csharp
private static void ExitProgram(string message)
{
    Console.WriteLine("{0}", message);
    Console.WriteLine("Press any key to exit the program...");
    Console.ReadKey();
    Environment.Exit(0);
}
```

## <a name="3---create-the-pipeline"></a>3 - Boru hattını oluşturun

Azure Bilişsel Arama'da, AI işleme dizin oluşturma (veya veri alımı) sırasında gerçekleşir. İzleyicinin bu bölümü dört nesne oluşturur: veri kaynağı, dizin tanımı, skillset, indexer. 

### <a name="step-1-create-a-data-source"></a>1. Adım: Veri kaynağı oluşturma

`SearchServiceClient`, `DataSources` özelliğine sahiptir. Bu özellik, Azure Bilişsel Arama veri kaynakları oluşturmak, listelemek, güncelleştirmek veya silmek için gereken tüm yöntemleri sağlar.

Arayarak `serviceClient.DataSources.CreateOrUpdate(dataSource)` `DataSource` yeni bir örnek oluşturun. `DataSource.AzureBlobStorage`veri kaynağı adını, bağlantı dizesini ve blob kapsayıcı adını belirtmenizi gerektirir.

```csharp
private static DataSource CreateOrUpdateDataSource(SearchServiceClient serviceClient, IConfigurationRoot configuration)
{
    DataSource dataSource = DataSource.AzureBlobStorage(
        name: "demodata",
        storageConnectionString: configuration["AzureBlobConnectionString"],
        containerName: "basic-demo-data-pr",
        description: "Demo files to demonstrate cognitive search capabilities.");

    // The data source does not need to be deleted if it was already created
    // since we are using the CreateOrUpdate method
    try
    {
        serviceClient.DataSources.CreateOrUpdate(dataSource);
    }
    catch (Exception e)
    {
        Console.WriteLine("Failed to create or update the data source\n Exception message: {0}\n", e.Message);
        ExitProgram("Cannot continue without a data source");
    }

    return dataSource;
}
```

Başarılı bir istek için yöntem oluşturulan veri kaynağını döndürecek. Geçersiz bir parametre gibi istekle ilgili bir sorun varsa, yöntem bir özel durum oluşturur.

Şimdi az `Main` önce eklediğiniz `CreateOrUpdateDataSource` işlevi aramak için bir satır ekleyin.

```csharp
public static void Main(string[] args)
{
    // Create service client
    IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
    IConfigurationRoot configuration = builder.Build();
    SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

    // Create or Update the data source
    Console.WriteLine("Creating or updating the data source...");
    DataSource dataSource = CreateOrUpdateDataSource(serviceClient, configuration);
```


<!-- 
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

Now that you have initialized the `DataSource` object, create the data source. `SearchServiceClient` has a `DataSources` property. This property provides all the methods you need to create, list, update, or delete Azure Cognitive Search data sources.

For a successful request, the method will return the data source that was created. If there is a problem with the request, such as an invalid parameter, the method will throw an exception.

```csharp
try
{
    serviceClient.DataSources.CreateOrUpdate(dataSource);
}
catch (Exception e)
{
    // Handle the exception
}
``` -->

Çözümü derleyin ve çalıştırın. Bu ilk isteğiniz olduğundan, Azure Bilişsel Arama'da veri kaynağının oluşturulduğunu doğrulamak için Azure portalını denetleyin. Arama hizmeti panosu sayfasında, Veri Kaynakları kutucuğunun yeni bir öğe içerdiğini doğrulayın. Portal sayfasının yenilenmesi için birkaç dakika beklemeniz gerekebilir.

  ![Portaldaki veri kaynakları döşemesi](./media/cognitive-search-tutorial-blob/data-source-tile.png "Portaldaki veri kaynakları döşemesi")

### <a name="step-2-create-a-skillset"></a>Adım 2: Bir beceri oluşturun

Bu bölümde, verilerinize uygulamak istediğiniz bir zenginleştirme adımları kümesi tanımlarsınız. Her zenginleştirme adım bir *beceri* denir ve zenginleştirme adımları kümesi bir *skillset*. Bu öğretici, skillset için [yerleşik bilişsel becerileri](cognitive-search-predefined-skills.md) kullanır:

+ Görüntü dosyalarında basılı ve el yazısıyla yazılmış metni tanımak için [Optik Karakter Tanıma.](cognitive-search-skill-ocr.md)

+ [Metin Birleştirme,](cognitive-search-skill-textmerger.md) alan koleksiyonundan metni tek bir alana birleştirmek için.

+ İçeriğin dilini tanımlamak için [Dil Algılama](cognitive-search-skill-language-detection.md).

+ [Metin Bölme](cognitive-search-skill-textsplit.md) anahtar tümcecik çıkarma beceri ve varlık tanıma beceri aramadan önce küçük parçalar halinde büyük içerik kırmak için. Anahtar tümcecik çıkarma ve varlık tanıma 50.000 karakter veya daha az girişkabul. Bu sınıra uymak için örnek dosyaların birkaç tanesinin bölünmesi gerekir.

+ Blob kapsayıcısındaki içerikten kuruluşların adlarını ayıklamak için [Varlık Tanıma.](cognitive-search-skill-entity-recognition.md)

+ Üst anahtar tümcecikleri çekmek için [Anahtar İfade Ayıklama](cognitive-search-skill-keyphrases.md).

Azure Bilişsel Arama, ilk işlem sırasında her belgeyi farklı dosya biçimlerinden gelen içeriği okumak için çatlaklar. Kaynak dosyadan gelen, bulunan metin, oluşturulan ```content``` alanına her belge için birer birer yerleştirilir. Bu nedenle, girişi bu ```"/document/content"``` metni kullanacak şekilde ayarlayın. 

Çıktılar bir dizine eşlenebilir, aşağı akış becerisine yönelik giriş olarak kullanılır veya dil kodunda olduğu gibi her iki şekilde de kullanılabilir. Dizinde bir dil kodu, filtreleme için yararlıdır. Giriş olarak dil kodu, sözcük bölünmesiyle ilgili dilbilgisi kurallarını bildirmek için metin analizi becerileri tarafından kullanılır.

Beceri kümesi temelleri hakkında daha fazla bilgi için bkz. [Beceri kümesini tanımlama](cognitive-search-defining-skillset.md).

### <a name="ocr-skill"></a>OCR becerisi

**OCR** becerisi resimlerden metin ayıklar. Bu beceri, normalized_images bir alanın var olduğunu varsayar. Bu alanı oluşturmak için, daha sonra öğreticide dizinleyici tanımındaki yapılandırmayı ```"imageAction"``` . ```"generateNormalizedImages"```

```csharp
private static OcrSkill CreateOcrSkill()
{
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

    return ocrSkill;
}
```

### <a name="merge-skill"></a>Birleştirme becerisi

Bu bölümde, belge içeriği alanını OCR becerisi tarafından üretilen metinle birleştiren **birleştirme** becerisi oluşturacaksınız.

```csharp
private static MergeSkill CreateMergeSkill()
{
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

    return mergeSkill;
}
```

### <a name="language-detection-skill"></a>Dil algılama becerisi

**Dil Algılama** becerisi giriş metninin dilini algılar ve istek üzerine gönderilen her belge için tek bir dil kodu bildirir. **Metin Bölme** becerisine girişin bir parçası olarak **Dil Algılama** becerisinin çıktısını kullanacağız.

```csharp
private static LanguageDetectionSkill CreateLanguageDetectionSkill()
{
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

    return languageDetectionSkill;
}
```

### <a name="text-split-skill"></a>Metin bölme becerisi

Aşağıdaki **Split** becerisi metni sayfalara göre böler ve sayfa uzunluğunu 4.000 karakterle sınırlandıracaktır. `String.Length` Algoritma, metni en fazla `maximumPageLength` boyutta olan parçalara bölmeye çalışır. Bu durumda, algoritma cümleyi bir cümle sınırında kırmak için elinden geleni yapacaktır, bu nedenle `maximumPageLength`öss boyutu biraz daha az olabilir.

```csharp
private static SplitSkill CreateSplitSkill()
{
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

    return splitSkill;
}
```

### <a name="entity-recognition-skill"></a>Varlık tanıma becerisi

Bu `EntityRecognitionSkill` örnek kategori türünü `organization`tanıyacak şekilde ayarlanır. **Varlık Tanıma** becerisi kategori türlerini `person` ve `location`.

"Bağlam" alanının bir yıldız ```"/document/pages/*"``` işaretiyle ayarlı olduğuna dikkat edin, yani ```"/document/pages"```zenginleştirme adımı altındaki her sayfa için çağrılır.

```csharp
private static EntityRecognitionSkill CreateEntityRecognitionSkill()
{
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

    return entityRecognitionSkill;
}
```

### <a name="key-phrase-extraction-skill"></a>Anahtar tümcecik çıkarma becerisi

Yeni `EntityRecognitionSkill` oluşturulan örnek gibi, **Anahtar Tümcecik Ayıklama** becerisi de belgenin her sayfası için çağrılır.

```csharp
private static KeyPhraseExtractionSkill CreateKeyPhraseExtractionSkill()
{
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

    return keyPhraseExtractionSkill;
}
```

### <a name="build-and-create-the-skillset"></a>Beceri oluşturma ve oluşturma

Oluşturduğunuz `Skillset` becerileri kullanarak oluşturun.

```csharp
private static Skillset CreateOrUpdateDemoSkillSet(SearchServiceClient serviceClient, IList<Skill> skills)
{
    Skillset skillset = new Skillset(
        name: "demoskillset",
        description: "Demo skillset",
        skills: skills);

    // Create the skillset in your search service.
    // The skillset does not need to be deleted if it was already created
    // since we are using the CreateOrUpdate method
    try
    {
        serviceClient.Skillsets.CreateOrUpdate(skillset);
    }
    catch (Exception e)
    {
        Console.WriteLine("Failed to create the skillset\n Exception message: {0}\n", e.Message);
        ExitProgram("Cannot continue without a skillset");
    }

    return skillset;
}
```

Aşağıdaki satırları `Main`ekleyin.

```csharp
    // Create the skills
    Console.WriteLine("Creating the skills...");
    OcrSkill ocrSkill = CreateOcrSkill();
    MergeSkill mergeSkill = CreateMergeSkill();
    EntityRecognitionSkill entityRecognitionSkill = CreateEntityRecognitionSkill();
    LanguageDetectionSkill languageDetectionSkill = CreateLanguageDetectionSkill();
    SplitSkill splitSkill = CreateSplitSkill();
    KeyPhraseExtractionSkill keyPhraseExtractionSkill = CreateKeyPhraseExtractionSkill();

    // Create the skillset
    Console.WriteLine("Creating or updating the skillset...");
    List<Skill> skills = new List<Skill>();
    skills.Add(ocrSkill);
    skills.Add(mergeSkill);
    skills.Add(languageDetectionSkill);
    skills.Add(splitSkill);
    skills.Add(entityRecognitionSkill);
    skills.Add(keyPhraseExtractionSkill);

    Skillset skillset = CreateOrUpdateDemoSkillSet(serviceClient, skills);
```

### <a name="step-3-create-an-index"></a>Adım 3: Dizin oluşturma

Bu bölümde, aranabilir dizine dahil edilecek alanları ve her bir alana ilişkin arama özniteliklerini belirterek dizin şemasını tanımlarsınız. Alanlar bir türe sahiptir ve alanın nasıl kullanıldığını (aranabilir, sıralanabilir vb.) belirleyen öznitelikleri alabilir. Bir dizindeki alan adlarının, kaynaktaki alan adlarıyla tamamen aynı olması gerekmez. Sonraki bir adımda, kaynak-hedef alanlarını bağlamak için dizin oluşturucuda alan eşlemeleri eklersiniz. Bu adım için, arama uygulamanızla ilgili alan adlandırma kurallarını kullanarak dizini tanımlayın.

Bu çalışmada aşağıdaki alanlar ve alan türleri kullanılır:

| alan adları: | `id`       | content   | languageCode | keyPhrases         | organizations     |
|--------------|----------|-------|----------|--------------------|-------------------|
| field-types: | Edm.String|Edm.String| Edm.String| List<Edm.String>  | List<Edm.String>  |


#### <a name="create-demoindex-class"></a>DemoIndex Sınıfı Oluştur

Bu dizin için alanlar bir model sınıfı kullanılarak tanımlanır. Model sınıfının her özelliği karşılık gelen dizin alanının aramayla ilgili davranışlarını belirleyen özniteliklere sahiptir. 

Model sınıfını yeni bir C# dosyasına ekleyeceğiz. Projenize sağ tıklayın ve > **Yeni Öğe Ekle'yi seçin...** `DemoIndex.cs`, "Sınıf" seçin ve dosyayı adlandırın, ardından **Ekle'yi**seçin. **Add**

Ve `Microsoft.Azure.Search` `Microsoft.Azure.Search.Models` ad alanlarından türleri kullanmak istediğinizden emin olun.

Aşağıdaki model sınıfı tanımını ekleyin `DemoIndex.cs` ve dizini oluşturacağınız aynı ad alanına ekleyin.

```csharp
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;

namespace EnrichwithAI
{
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
}
```

<!-- Add the below model class definition to `DemoIndex.cs` and include it in the same namespace where you'll create the index.

```csharp
// The SerializePropertyNamesAsCamelCase attribute is defined in the Azure Cognitive Search .NET SDK.
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
``` -->

Artık bir model sınıfı tanımladığınıza göre, geri dönüşte `Program.cs` oldukça kolay bir dizin tanımı oluşturabilirsiniz. Bu dizinin adı `demoindex`. Bu adla zaten bir dizin varsa, silinir.

```csharp
private static Index CreateDemoIndex(SearchServiceClient serviceClient)
{
    var index = new Index()
    {
        Name = "demoindex",
        Fields = FieldBuilder.BuildForType<DemoIndex>()
    };

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
        Console.WriteLine("Failed to create the index\n Exception message: {0}\n", e.Message);
        ExitProgram("Cannot continue without an index");
    }

    return index;
}
```

Test sırasında dizin oluşturmaya çalıştığınızı görebilirsiniz. Bu nedenle, oluşturmak üzere olduğunuz dizinin oluşturmayı denemeden önce zaten var olup olmadığını denetleyin.

Aşağıdaki satırları `Main`ekleyin.

```csharp
    // Create the index
    Console.WriteLine("Creating the index...");
    Index demoIndex = CreateDemoIndex(serviceClient);
```

<!-- ```csharp
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
 -->

Bir dizin tanımlama hakkında daha fazla bilgi edinmek için [bkz.](https://docs.microsoft.com/rest/api/searchservice/create-index)

### <a name="step-4-create-and-run-an-indexer"></a>Adım 4: Bir dizin oluştur ve çalıştır

Şu ana kadar bir veri kaynağı, beceri kümesi ve dizin oluşturdunuz. Bu üç bileşen, her bir parçayı birlikte tek bir çok aşamalı işleme çeken [dizin oluşturucunun](search-indexer-overview.md) parçası olur. Bunları bir dizin oluşturucu birbirine bağlamak için alan eşlemeleri tanımlamanız gerekir.

+ Alan Haritalamalar skillset önce işlenir, bir dizindeki hedef alanlara veri kaynağından kaynak alanları eşleme. Alan adları ve türleri her iki uçta da aynıysa, eşleme gerekmez.

+ OutputFieldMappings skillset sonra işlenir, belge çatlama veya zenginleştirme onları oluşturur kadar var olmayan sourceFieldNames başvurulan. TargetFieldName bir dizinde bir alandır.

Girdileri çıktılara bağlamanın yanı sıra, veri yapılarını düzleştirmek için alan eşlemelerini de kullanabilirsiniz. Daha fazla bilgi [için, zenginleştirilmiş alanları aranabilir bir dizine nasıl eşlendirilir'](cognitive-search-output-field-mapping.md)e bakın.

```csharp
private static Indexer CreateDemoIndexer(SearchServiceClient serviceClient, DataSource dataSource, Skillset skillSet, Index index)
{
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
        Console.WriteLine("Failed to create the indexer\n Exception message: {0}\n", e.Message);
        ExitProgram("Cannot continue without creating an indexer");
    }

    return indexer;
}
```
Aşağıdaki satırları `Main`ekleyin.

```csharp
    // Create the indexer, map fields, and execute transformations
    Console.WriteLine("Creating the indexer...");
    Indexer demoIndexer = CreateDemoIndexer(serviceClient, dataSource, skillset, demoIndex);
```

Dizin oluşturucuyu oluşturmanın tamamlanmasının biraz zaman alacağını bekleyin. Veri kümesi küçük olsa da, analiz becerileri bilgi işlem açısından yoğundur. Görüntü analizi gibi bazı beceriler uzun sürer.

> [!TIP]
> Bir dizin oluşturucu oluşturulduğunda, işlem hattı çağrılır. Verilere ulaşılırken, eşleme girişleri ve çıktıları veya işlemlerin sırası ile ilgili sorun olursa bunlar bu aşamada görüntülenir.

### <a name="explore-creating-the-indexer"></a>Dizin oluşturucuyu keşfedin

Kod -1 olarak ayarlar ```"maxFailedItems"``` ve dizin oluşturma motoruna veri alma sırasındaki hataları yoksaymasını bildirir. Demo veri kaynağında çok az belge olduğundan bu yararlıdır. Daha büyük bir veri kaynağı için değeri, 0’dan daha büyük bir değere ayarlarsınız.

Ayrıca dikkat ```"dataToExtract"``` edin ```"contentAndMetadata"```ayarlanır . Bu deyim, dizin oluşturucuya, farklı dosya biçimlerinden içeriği ve her bir dosyayla ilgili meta verileri otomatik olarak ayıklamasını bildirir.

İçerik ayıklandığında, veri kaynağında bulunan görüntülerden metni ayıklamak için `imageAction` değerini ayarlayabilirsiniz. ```"generateNormalizedImages"``` Yapılandırma ```"imageAction"``` kümesi, OCR Beceri ve Metin Birleştirme Becerisi ile birlikte, dizinleyiciye resimlerden metin ayıklamasını (örneğin, trafik Durdur işaretinden "dur" sözcüğü) ayıklamasını ve içerik alanının bir parçası olarak gömmesini söyler. Bu davranış hem belgelerde gömülü olan görüntüler (örneğin, bir PDF’teki görüntü) hem de veri kaynağında bulunan görüntüler (örneğin, bir JPG dosyası) için geçerlidir.

<a name="check-indexer-status"></a>

## <a name="4---monitor-indexing"></a>4 - İzleme indeksleme

Dizin oluşturucu tanımlandıktan sonra, isteği gönderdiğinizde otomatik olarak çalıştırılır. Tanımladığınız bilişsel becerilere bağlı olarak dizin oluşturma beklediğinizden uzun sürebilir. Dizinleyicinin hala çalışıp çalışmadığını `GetStatus` öğrenmek için yöntemi kullanın.

```csharp
private static void CheckIndexerOverallStatus(SearchServiceClient serviceClient, Indexer indexer)
{
    try
    {
        IndexerExecutionInfo demoIndexerExecutionInfo = serviceClient.Indexers.GetStatus(indexer.Name);

        switch (demoIndexerExecutionInfo.Status)
        {
            case IndexerStatus.Error:
                ExitProgram("Indexer has error status. Check the Azure Portal to further understand the error.");
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
        Console.WriteLine("Failed to get indexer overall status\n Exception message: {0}\n", e.Message);
    }
}
```

`IndexerExecutionInfo`dizinleyicinin geçerli durumunu ve yürütme geçmişini temsil eder.

Uyarılar bazı kaynak dosya ve beceri birleşimleri için geneldir ve her zaman bir sorunu belirtmez. Bu öğreticide, uyarılar zararsızdır (örneğin, JPEG dosyalarında bir metin girişi yok).

Aşağıdaki satırları `Main`ekleyin.

```csharp
    // Check indexer overall status
    Console.WriteLine("Check the indexer overall status...");
    CheckIndexerOverallStatus(serviceClient, demoIndexer);
```
 
## <a name="5---search"></a>5 - Arama

Dizin oluşturma tamamlandıktan sonra, tek tek alanların içeriğini döndüren sorguları çalıştırabilirsiniz. Varsayılan olarak, Azure Bilişsel Arama en iyi 50 sonucu döndürür. Varsayılan değerin düzgün çalışması için örnek veriler küçüktür. Ancak, büyük veri kümeleriyle çalışırken, daha fazla sonuç döndürmek için sorgu dizesine parametreleri dahil etmeniz gerekebilir. Talimatlar için Azure [Bilişsel Arama'da sonuçları sayfalamak için bkz.](search-pagination-page-layout.md)

Doğrulama adımı olarak, tüm alanlar için dizini sorgulayın.

Aşağıdaki satırları `Main`ekleyin.

```csharp
DocumentSearchResult<DemoIndex> results;

ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

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

`CreateSearchIndexClient`uygulamanın config dosyasında (appsettings.json) depolanan yeni `SearchIndexClient` bir kullanarak değerler oluşturur. Arama hizmeti sorgusu API anahtarının yönetici anahtarı nın değil de kullanıldığına dikkat edin.

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
   string searchServiceName = configuration["SearchServiceName"];
   string queryApiKey = configuration["SearchServiceQueryApiKey"];

   SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "demoindex", new SearchCredentials(queryApiKey));
   return indexClient;
}
```

Aşağıdaki kodu `Main` içine ekleyin. İlk deneme yakalama, her alanın adı, türü ve öznitelikleriyle dizin tanımını döndürür. İkincisi, örneğin `organizations`sonuçlara hangi `Select` alanların dahil edileli olarak belirtildiği parametreli bir sorgudur. Tek bir `"*"` alanın tüm içeriğini döndürür bir arama dizesi.

```csharp
//Verify content is returned after indexing is finished
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

try
{
    results = indexClientForQueries.Documents.Search<DemoIndex>("*");
    Console.WriteLine("First query succeeded with a result count of {0}", results.Results.Count);
}
catch (Exception e)
{
    Console.WriteLine("First query failed\n Exception message: {0}\n", e.Message);
}

SearchParameters parameters =
    new SearchParameters
    {
        Select = new[] { "organizations" }
    };

try
{
    results = indexClientForQueries.Documents.Search<DemoIndex>("*", parameters);
    Console.WriteLine("Second query succeeded with a result count of {0}", results.Results.Count);
}
catch (Exception e)
{
    Console.WriteLine("Second query failed\n Exception message: {0}\n", e.Message);
}
```

Ek alanlar için yineleyin: bu alıştırmada içerik, languageCode, keyPhrases ve kuruluşlar. Virgülle sınırlı bir liste kullanarak [Seç](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters.select?view=azure-dotnet) özelliği üzerinden birden çok alanı döndürebilirsiniz.

<a name="reset"></a>

## <a name="reset-and-rerun"></a>Sıfırlama ve yeniden çalıştırma

Geliştirmenin ilk deneysel aşamalarında, tasarım yinelemesi için en pratik yaklaşım, nesneleri Azure Bilişsel Arama'dan silmek ve kodunuzu yeniden oluşturmasına izin vermektir. Kaynak adları benzersizdir. Bir nesneyi sildiğinizde, aynı adı kullanarak nesneyi yeniden oluşturabilirsiniz.

Bu öğreticinin örnek kodu varolan nesneleri denetler ve kodunuzu yeniden çalıştırmak için bunları siler.

Portalı dizinleri, dizinleyicileri, veri kaynaklarını ve beceri kümelerini silmek için de kullanabilirsiniz.

## <a name="takeaways"></a>Paketler

Bu öğretici, bileşen parçalarının oluşturulması yoluyla zenginleştirilmiş bir dizin oluşturma için temel adımları göstermiştir: bir veri kaynağı, skillset, index ve indexer.

[Yerleşik beceriler,](cognitive-search-predefined-skills.md) skillset tanımı ve zincirleme becerilerin mekaniği ile birlikte giriş ve çıktılar yoluyla bir araya getirilmiştir. Ayrıca, dizin leyici `outputFieldMappings` tanımında, zenginleştirilmiş değerleri ardışık kaynaktan azure bilişsel arama hizmetinde aranabilir bir dizine yönlendirmenin gerekli olduğunu da öğrendiniz.

Son olarak, daha fazla yineleme için sonuçların nasıl test edileceğini ve sistemin nasıl sıfırlanacağını öğrendiniz. Dizine karşı sorgular düzenlendiğinde, zenginleştirilmiş dizin oluşturma işlem hattı tarafından oluşturulan çıktının döndürüldüğünü öğrendiniz. Ayrıca dizin oluşturucu durumunun nasıl denetleneceğini ve işlem hattı yeniden çalıştırılmadan önce hangi nesnelerin silineceğini de öğrendiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda, artık ihtiyacınız olmayan kaynakları kaldırmak iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol navigasyon bölmesindeki Tüm kaynaklar veya Kaynak grupları bağlantısını kullanarak portaldaki kaynakları bulabilir ve yönetebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık bir AI zenginleştirme boru hattındaki tüm nesnelere aşina olduğunuza göre, beceri tanımlarına ve bireysel becerilere daha yakından bakalım.

> [!div class="nextstepaction"]
> [Nasıl bir skillset oluşturmak için](cognitive-search-defining-skillset.md)
