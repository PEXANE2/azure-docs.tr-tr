---
title: Azure Blob 'larında AI kullanan C# öğreticisi
titleSuffix: Azure Cognitive Search
description: C# ve Azure Bilişsel Arama .NET SDK kullanarak blob depolamada içerik üzerinde metin ayıklama ve doğal dil işleme örneğini adım adım yapın.
manager: nitinme
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 05/05/2020
ms.openlocfilehash: 57cb68726adf8818f9ef0c8804be9c388ea39ff5
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82872312"
---
# <a name="tutorial-ai-generated-searchable-content-from-azure-blobs-using-the-net-sdk"></a>Öğretici: .NET SDK kullanarak Azure Bloblarından AI tarafından oluşturulan aranabilir içerik

Azure Blob depolamada yapılandırılmamış metin veya görüntü varsa, bir [AI zenginleştirme işlem hattı](cognitive-search-concept-intro.md) bilgileri ayıklayabilir ve tam metin araması veya bilgi araştırma senaryoları için faydalı yeni içerik oluşturabilir. Bu C# öğreticisinde, resimlerde optik karakter tanıma (OCR) uygulayın ve sorgularda, modellerinizde ve filtrelerde kullanabileceğiniz yeni alanlar oluşturmak için doğal dil işleme gerçekleştirin.

Bu öğretici aşağıdaki görevleri gerçekleştirmek için C# ve [.NET SDK](https://aka.ms/search-sdk) kullanır:

> [!div class="checklist"]
> * Azure Blob depolamada uygulama dosyaları ve görüntüleriyle başlayın.
> * OCR, metin ayıklama, dil algılama, varlık ve anahtar tümceciği tanıma eklemek için bir işlem hattı tanımlayın.
> * Çıktıyı depolamak için bir dizin tanımlayın (ham içerik ve ardışık düzen tarafından oluşturulan ad-değer çiftleri).
> * Dönüşümleri ve Analizi başlatmak ve dizini oluşturmak ve yüklemek için işlem hattını yürütün.
> * Tam metin aramasını ve zengin sorgu söz dizimini kullanarak sonuçları keşfedebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) açın.

## <a name="prerequisites"></a>Ön koşullar

+ [Azure Storage](https://azure.microsoft.com/services/storage/)
+ [Visual Studio](https://visualstudio.microsoft.com/downloads/)
+ [Mevcut bir arama hizmeti](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) [oluşturun](search-create-service-portal.md) veya bulun 

> [!Note]
> Bu öğretici için ücretsiz hizmeti kullanabilirsiniz. Ücretsiz arama hizmeti, sizi üç Dizin, üç Dizin Oluşturucu ve üç veri kaynağı ile sınırlandırır. Bu öğreticide hepsinden birer tane oluşturulur. Başlamadan önce, hizmetinize yeni kaynakları kabul etmek için yeriniz olduğundan emin olun.

## <a name="download-files"></a>Dosyaları indirme

1. Bu [OneDrive klasörünü](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4) açın ve sol üst köşedeki dosyaları bilgisayarınıza kopyalamak için **İndir** ' e tıklayın. 

1. ZIP dosyasına sağ tıklayın ve **Tümünü Ayıkla**' yı seçin. Çeşitli türlerde 14 dosya vardır. Bu alıştırma için 7 kullanacaksınız.

Ayrıca, Bu öğreticinin kaynak kodunu indirebilirsiniz. Kaynak kodu, [Azure-Search-DotNet-Samples](https://github.com/Azure-Samples/azure-search-dotnet-samples) deposundaki öğretici-AI-zenginleştirme klasöründedir.

## <a name="1---create-services"></a>1-hizmet oluşturma

Bu öğreticide dizin oluşturma ve sorgular, AI zenginleştirme için arka uçta bilişsel hizmetler ve verileri sağlamak üzere Azure Blob depolama için Azure Bilişsel Arama kullanılmaktadır. Bu öğretici, bilişsel hizmetler 'de her gün Dizin Oluşturucu başına 20 işlem için ücretsiz ayırma kapsamında kalır, bu nedenle yalnızca oluşturmanız gereken hizmetler arama ve depolama amaçlıdır.

Mümkünse, yakınlık ve yönetilebilirlik için aynı bölgede ve kaynak grubunda her ikisini de oluşturun. Uygulamada, Azure depolama hesabınız herhangi bir bölgede olabilir.

### <a name="start-with-azure-storage"></a>Azure Storage 'ı kullanmaya başlama

1. [Azure Portal oturum açın](https://portal.azure.com/) ve **+ kaynak oluştur**' a tıklayın.

1. *Depolama hesabı* araması yapın ve Microsoft 'un depolama hesabı teklifi ' ni seçin.

   ![Depolama hesabı oluştur](media/cognitive-search-tutorial-blob/storage-account.png "Depolama hesabı oluştur")

1. Temel bilgiler sekmesinde, aşağıdaki öğeler gereklidir. Diğer her şey için varsayılanları kabul edin.

   + **Kaynak grubu**. Mevcut bir tane seçin veya yeni bir tane oluşturun, ancak bunları topluca yönetebilmeniz için tüm hizmetler için aynı grubu kullanın.

   + **Depolama hesabı adı**. Aynı türde birden fazla kaynağınız olabileceğini düşünüyorsanız, tür ve bölgeye göre belirsizliği ortadan kaldırmak için adı kullanın, örneğin *blobstoragewestus*. 

   + **Konum**. Mümkünse, Azure Bilişsel Arama ve bilişsel hizmetler için kullanılan aynı konumu seçin. Tek bir konum, bant genişliği ücretlerini oylar.

   + **Hesap türü**. Varsayılan, *StorageV2 (genel amaçlı v2)* seçeneğini belirleyin.

1. Hizmeti oluşturmak için **gözden geçir + oluştur** ' a tıklayın.

1. Oluşturulduktan sonra genel bakış sayfasını açmak için **Kaynağa Git** ' e tıklayın.

1. **Bloblar** hizmeti ' ne tıklayın.

1. Bir kapsayıcı oluşturmak ve *COG-Search-demo*olarak adlandırmak Için **+ kapsayıcı** ' ya tıklayın.

1. *COG-Search-demo* ' i seçin ve ardından yükleme dosyalarını kaydettiğiniz klasörü açmak Için **karşıya yükle** ' ye tıklayın. Tüm on dört dosyayı seçip karşıya yüklemek üzere **Tamam** ' a tıklayın.

   ![Örnek dosyaları karşıya yükle](media/cognitive-search-quickstart-blob/sample-data.png "Örnek dosyaları karşıya yükle")

1. Azure depolama alanını kapatmadan önce Azure Bilişsel Arama bir bağlantıyı formülleştirmek için bir bağlantı dizesi alın. 

   1. Depolama hesabınızın genel bakış sayfasına geri gidin (örnek olarak *blobstoragewestus* kullandık). 
   
   1. Sol gezinti bölmesinde **erişim anahtarları** ' nı seçin ve bağlantı dizelerinden birini kopyalayın. 

   Bağlantı dizesi, aşağıdaki örneğe benzer bir URL 'dir:

      ```http
      DefaultEndpointsProtocol=https;AccountName=blobstoragewestus;AccountKey=<your account key>;EndpointSuffix=core.windows.net
      ```

1. Bağlantı dizesini Not defteri 'ne kaydedin. Daha sonra veri kaynağı bağlantısını ayarlarken gerekecektir.

### <a name="cognitive-services"></a>Bilişsel Hizmetler

AI zenginleştirme, doğal dil ve görüntü işleme için Metin Analizi ve Görüntü İşleme dahil bilişsel hizmetler tarafından desteklenir. Amacınız gerçek bir prototipi veya projeyi tamamlayacaksa, bu noktada bilişsel hizmetler sağlama (Azure Bilişsel Arama ile aynı bölgede), böylece dizin oluşturma işlemlerine iliştirebilirsiniz.

Bununla birlikte, Azure Bilişsel Arama, arka planda bilişsel hizmetlere bağlanıp Dizin Oluşturucu başına 20 ücretsiz işlem sunabileceğinden kaynak sağlamayı atlayabilirsiniz. Bu öğretici 14 işlem kullandığından, ücretsiz ayırma yeterlidir. Daha büyük projeler için, Kullandıkça öde, bu hizmetleri, Kullandıkça öde, bu hizmetler için sağlama bölümüne planlayın. Daha fazla bilgi için bkz. bilişsel [Hizmetler iliştirme](cognitive-search-attach-cognitive-services.md).

### <a name="azure-cognitive-search"></a>Azure Bilişsel Arama

Üçüncü bileşen, [portalda oluşturabileceğiniz](search-create-service-portal.md)Azure bilişsel arama. Bu izlenecek yolu tamamlamak için ücretsiz katmanı kullanabilirsiniz. 

### <a name="get-an-admin-api-key-and-url-for-azure-cognitive-search"></a>Azure Bilişsel Arama yönelik bir yönetici API anahtarı ve URL 'SI alın

Azure Bilişsel Arama hizmetiyle etkileşim kurmak için hizmet URL 'SI ve erişim anahtarı gerekir. Her ikisiyle de bir arama hizmeti oluşturulur. bu nedenle, aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure Portal oturum açın](https://portal.azure.com/)ve arama hizmetine **genel bakış** sayfasında URL 'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. **Ayarlar** > **anahtarlar**' da, hizmette tam haklar için bir yönetici anahtarı alın. Üzerinde bir tane almanız gereken iş sürekliliği için iki adet değiştirilebilir yönetici anahtarı vardır. Nesneleri eklemek, değiştirmek ve silmek için isteklerde birincil veya ikincil anahtarı kullanabilirsiniz.

   Sorgu anahtarını da alın. Salt okuma erişimiyle sorgu istekleri vermek en iyi uygulamadır.

   ![Hizmet adı ve yönetici ve sorgu anahtarlarını alın](media/search-get-started-nodejs/service-name-and-keys.png)

İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="2---set-up-your-environment"></a>2-ortamınızı ayarlama

Visual Studio 'Yu açıp .NET Core üzerinde çalışabilen yeni bir konsol uygulama projesi oluşturarak başlayın.

### <a name="install-nuget-packages"></a>NuGet paketlerini yükleme

[Azure bilişsel arama .NET SDK](https://aka.ms/search-sdk) , dizinlerinizi, veri kaynaklarınızı, Dizin Oluşturucularınızı ve becerileri yönetmenizi sağlayan birkaç istemci kitaplığı içerir. Ayrıca, http ve JSON ayrıntıları ile uğraşmak zorunda kalmadan belgeleri karşıya yükleyip yönetebilir ve sorguları yürütmenize imkan tanır. Bu istemci kitaplıklarının hepsi NuGet paketleri olarak dağıtılır.

Bu proje için `Microsoft.Azure.Search` NuGet paketinin 9 veya sonraki bir sürümünü yüklemelisiniz.

1. Bir tarayıcıda [Microsoft. Azure. Search NuGet paketi sayfasına](https://www.nuget.org/packages/Microsoft.Azure.Search)gidin.

1. En son sürümü (9 veya üzeri) seçin.

1. Paket Yöneticisi komutunu kopyalayın.

1. Paket Yöneticisi konsolunu açın. **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu**' nu seçin. 

1. Önceki adımda kopyaladığınız komutu yapıştırın ve çalıştırın.

Sonra, en son `Microsoft.Extensions.Configuration.Json` NuGet paketini yükler.

1. **Araçlar** > **NuGet Paket Yöneticisi** > **çözüm için NuGet Paketlerini Yönet...** seçeneğini belirleyin. 

1. **Araştır** ' a tıklayın ve `Microsoft.Extensions.Configuration.Json` NuGet paketini arayın. 

1. Paketi seçin, projenizi seçin, sürümün en son kararlı sürümü olduğunu onaylayın ve ardından **Install**' a tıklayın.

### <a name="add-service-connection-information"></a>Hizmet bağlantı bilgileri ekleme

1. Çözüm Gezgini projenize sağ tıklayıp yeni öğe **Ekle** > **...** seçeneğini belirleyin. 

1. Dosyayı `appsettings.json` adlandırın ve **Ekle**' yi seçin. 

1. Bu dosyayı çıkış dizininize dahil edin.
    1. Sağ tıklayın `appsettings.json` ve **Özellikler**' i seçin. 
    1. **Daha yeniyse**, **çıkış dizinine kopyalanacak kopya** değerini değiştirin.

1. Aşağıdaki JSON dosyasını yeni JSON dosyanıza kopyalayın.

    ```json
    {
      "SearchServiceName": "Put your search service name here",
      "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
      "SearchServiceQueryApiKey": "Put your query API key here",
      "AzureBlobConnectionString": "Put your Azure Blob connection string here",
    }
    ```
    
Arama hizmetinizi ve BLOB depolama hesabı bilgilerinizi ekleyin. Bu bilgileri, önceki bölümde belirtilen hizmet sağlama adımlarından alabileceğiniz şekilde geri çekin.

**SearchServiceName**için, tam URL 'yi değil, kısa hizmet adını girin.

### <a name="add-namespaces"></a>Ad alanı Ekle

İçinde `Program.cs`, aşağıdaki ad alanlarını ekleyin.

```csharp
using System;
using System.Collections.Generic;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Extensions.Configuration;

namespace EnrichwithAI
```

### <a name="create-a-client"></a>İstemci oluşturma

Altında `SearchServiceClient` `Main`sınıfının bir örneğini oluşturun.

```csharp
public static void Main(string[] args)
{
    // Create service client
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

### <a name="add-function-to-exit-the-program-during-failure"></a>Hata sırasında programdan çıkmak için işlev Ekle

Bu öğretici, dizin oluşturma işlem hattının her adımını anlamanıza yardımcı olmak için tasarlanmıştır. Programın veri kaynağı, Beceri, dizin veya dizin oluşturucuyu oluşturmasını önleyen kritik bir sorun varsa, sorun anlaşılması ve giderilmesi için, program hata iletisini ve çıkış çıkışını çıktı olarak çıkar.

Programın `ExitProgram` çıkmasına `Main` gerek duyduğu senaryoları işlemek için öğesine ekleyin.

```csharp
private static void ExitProgram(string message)
{
    Console.WriteLine("{0}", message);
    Console.WriteLine("Press any key to exit the program...");
    Console.ReadKey();
    Environment.Exit(0);
}
```

## <a name="3---create-the-pipeline"></a>3-işlem hattını oluşturma

Azure Bilişsel Arama 'de, dizin oluşturma (veya veri alımı) sırasında AI işleme oluşur. İzlenecek yolun bu bölümü dört nesne oluşturur: veri kaynağı, Dizin tanımı, Beceri, Dizin Oluşturucu. 

### <a name="step-1-create-a-data-source"></a>1. Adım: Veri kaynağı oluşturma

`SearchServiceClient`, `DataSources` özelliğine sahiptir. Bu özellik, Azure Bilişsel Arama veri kaynaklarını oluşturmak, listelemek, güncelleştirmek veya silmek için gereken tüm yöntemleri sağlar.

Çağırarak `DataSource` `serviceClient.DataSources.CreateOrUpdate(dataSource)`yeni bir örnek oluşturun. `DataSource.AzureBlobStorage`veri kaynağı adını, bağlantı dizesini ve BLOB kapsayıcısı adını belirtmenizi gerektirir.

```csharp
private static DataSource CreateOrUpdateDataSource(SearchServiceClient serviceClient, IConfigurationRoot configuration)
{
    DataSource dataSource = DataSource.AzureBlobStorage(
        name: "demodata",
        storageConnectionString: configuration["AzureBlobConnectionString"],
        containerName: "cog-search-demo",
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

Başarılı bir istek için, yöntem oluşturulan veri kaynağını döndürür. İstekle ilgili bir sorun varsa (geçersiz parametre gibi), yöntem bir özel durum oluşturur.

Şimdi yeni eklediğiniz `CreateOrUpdateDataSource` işlevi çağırmak `Main` için içine bir satır ekleyin.

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

Çözümü derleyin ve çalıştırın. Bu ilk isteğiniz olduğundan, veri kaynağının Azure Bilişsel Arama oluşturulduğunu onaylamak için Azure portal denetleyin. Arama hizmeti panosu sayfasında, Veri Kaynakları kutucuğunun yeni bir öğe içerdiğini doğrulayın. Portal sayfasının yenilenmesi için birkaç dakika beklemeniz gerekebilir.

  ![Portalda veri kaynakları kutucuğu](./media/cognitive-search-tutorial-blob/data-source-tile.png "Portalda veri kaynakları kutucuğu")

### <a name="step-2-create-a-skillset"></a>2. Adım: beceri oluşturma

Bu bölümde, verilerinize uygulamak istediğiniz bir zenginleştirme adımları kümesi tanımlarsınız. Her bir zenginleştirme adımına bir *yetenek* ve bir *beceri*için bir dizi adım denir. Bu öğretici, beceri için yerleşik bilişsel [becerileri](cognitive-search-predefined-skills.md) kullanır:

+ Görüntü dosyalarında yazdırılmış ve el yazısı metinleri tanımak için [optik karakter tanıma](cognitive-search-skill-ocr.md) .

+ Bir alanlar koleksiyonundan tek bir alanda metin birleştirmek için [metin Merkli](cognitive-search-skill-textmerger.md) .

+ İçeriğin dilini tanımlamak için [Dil Algılama](cognitive-search-skill-language-detection.md).

+ Anahtar ifadesi ayıklama yeteneği ve varlık tanıma yeteneği çağrılmadan önce büyük içeriği daha küçük parçalara bölmek için [metin bölme](cognitive-search-skill-textsplit.md) . Anahtar tümceciği ayıklama ve varlık tanıma, 50.000 veya daha az karakter girişi kabul eder. Bu sınıra uymak için örnek dosyaların birkaç tanesinin bölünmesi gerekir.

+ Blob kapsayıcısında bulunan içerikten kuruluşların adlarını ayıklamak için [varlık tanıma](cognitive-search-skill-entity-recognition.md) .

+ Üst anahtar tümcecikleri çekmek için [Anahtar İfade Ayıklama](cognitive-search-skill-keyphrases.md).

İlk işlem sırasında Azure Bilişsel Arama, farklı dosya biçimlerinden içerik okumak için her bir belgeyi ister. Kaynak dosyadan gelen, bulunan metin, oluşturulan ```content``` alanına her belge için birer birer yerleştirilir. Bu nedenle, girdiyi ```"/document/content"``` bu metni kullanacak şekilde ayarlayın. 

Çıktılar bir dizine eşlenebilir, aşağı akış becerisine yönelik giriş olarak kullanılır veya dil kodunda olduğu gibi her iki şekilde de kullanılabilir. Dizinde bir dil kodu, filtreleme için yararlıdır. Giriş olarak dil kodu, sözcük bölünmesiyle ilgili dilbilgisi kurallarını bildirmek için metin analizi becerileri tarafından kullanılır.

Beceri kümesi temelleri hakkında daha fazla bilgi için bkz. [Beceri kümesini tanımlama](cognitive-search-defining-skillset.md).

### <a name="ocr-skill"></a>OCR becerisi

**OCR** becerisi görüntülerden metin ayıklar. Bu beceri bir normalized_images alanının bulunduğunu varsayar. Bu alanı oluşturmak için, öğreticide daha sonra, Dizin Oluşturucu tanımındaki ```"imageAction"``` yapılandırmayı olarak ```"generateNormalizedImages"```ayarlayacağız.

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

### <a name="merge-skill"></a>Birleştirme yeteneği

Bu bölümde, belge içeriği alanını OCR becerisi tarafından üretilen metinle birleştiren bir **birleştirme** yeteneği oluşturacaksınız.

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

### <a name="language-detection-skill"></a>Dil algılama yeteneği

**Dil algılama** Beceri, giriş metninin dilini algılar ve istekte gönderilen her belge için tek bir dil kodu bildirir. **Dil algılama** beceriye ait çıktıyı **metin bölünmüş** beceriye girdinin bir parçası olarak kullanacağız.

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

### <a name="text-split-skill"></a>Metin bölünmüş yetenek

Aşağıdaki **bölünmüş** Beceri, metni sayfalara göre böler ve sayfa uzunluğu ile `String.Length`4.000 karakter arasında bir şekilde sınırlayacaktır. Algoritma, metni en fazla `maximumPageLength` boyuttaki parçalara bölmeye çalışır. Bu durumda, algoritma bir cümle sınırında tümceyi bölmek en iyi şekilde yapılır, bu nedenle öbek boyutu daha az olabilir `maximumPageLength`.

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

### <a name="entity-recognition-skill"></a>Varlık tanıma yeteneği

Bu `EntityRecognitionSkill` örnek kategori türünü `organization`tanımak üzere ayarlanır. **Varlık tanıma** yeteneği de kategori türlerini `person` ve `location`kullanabilirsiniz.

"Bağlam" alanının bir yıldız işaretiyle ayarlanmış ```"/document/pages/*"``` olduğuna ve altındaki ```"/document/pages"```her sayfa için zenginleştirme adımının çağrıldığı anlamına gelir.

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

### <a name="key-phrase-extraction-skill"></a>Anahtar tümceciği ayıklama yeteneği

Yeni oluşturulan `EntityRecognitionSkill` örnek gibi **anahtar ifade ayıklama** Beceri, belgenin her sayfası için çağrılır.

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

Oluşturduğunuz becerileri `Skillset` kullanarak oluşturun.

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

Aşağıdaki satırları öğesine `Main`ekleyin.

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

### <a name="step-3-create-an-index"></a>3. Adım: Dizin oluşturma

Bu bölümde, aranabilir dizine dahil edilecek alanları ve her bir alana ilişkin arama özniteliklerini belirterek dizin şemasını tanımlarsınız. Alanlar bir türe sahiptir ve alanın nasıl kullanıldığını (aranabilir, sıralanabilir vb.) belirleyen öznitelikleri alabilir. Bir dizindeki alan adlarının, kaynaktaki alan adlarıyla tamamen aynı olması gerekmez. Sonraki bir adımda, kaynak-hedef alanlarını bağlamak için dizin oluşturucuda alan eşlemeleri eklersiniz. Bu adım için, arama uygulamanızla ilgili alan adlandırma kurallarını kullanarak dizini tanımlayın.

Bu çalışmada aşağıdaki alanlar ve alan türleri kullanılır:

| alan adları: | `id`       | content   | languageCode | keyPhrases         | organizations     |
|--------------|----------|-------|----------|--------------------|-------------------|
| field-types: | Edm.String|Edm.String| Edm.String| List<Edm.String>  | List<Edm.String>  |


#### <a name="create-demoindex-class"></a>Demoındex sınıfı oluşturma

Bu dizinin alanları bir model sınıfı kullanılarak tanımlanır. Model sınıfının her özelliği karşılık gelen dizin alanının aramayla ilgili davranışlarını belirleyen özniteliklere sahiptir. 

Model sınıfını yeni bir C# dosyasına ekleyeceğiz. Projenize sağ tıklayın ve yeni öğe **Ekle** > **...** seçeneğini belirleyin, "sınıf" seçeneğini belirleyip dosyayı `DemoIndex.cs`adlandırın ve ardından **Ekle**' yi seçin.

`Microsoft.Azure.Search` Ve `Microsoft.Azure.Search.Models` ad alanlarından türleri kullanmak istediğinizi belirttiğinizden emin olun.

Aşağıdaki model sınıfı tanımını öğesine `DemoIndex.cs` ekleyin ve dizini oluşturacağınız aynı ad alanına ekleyin.

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

Artık bir model sınıfı tanımladığınıza göre, bir dizin tanımını `Program.cs` oldukça kolay bir şekilde oluşturabilirsiniz. Bu dizinin adı olacaktır `demoindex`. Bu ada sahip bir dizin zaten varsa, silinir.

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

Sınama sırasında, dizini birden çok kez oluşturmayı denediğinizden emin olabilirsiniz. Bu nedenle, oluşturmak üzere olduğunuz dizinin oluşturmayı denemeden önce zaten var olup olmadığını denetleyin.

Aşağıdaki satırları öğesine `Main`ekleyin.

```csharp
    // Create the index
    Console.WriteLine("Creating the index...");
    Microsoft.Azure.Search.Models.Index demoIndex = CreateDemoIndex(serviceClient);
```

Ayırt eden başvuruyu çözümlemek için aşağıdaki using ifadesini ekleyin.

```csharp
using Index = Microsoft.Azure.Search.Models.Index;
```

Dizin tanımlama hakkında daha fazla bilgi edinmek için bkz. [Dizin oluşturma (Azure Bilişsel Arama REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index).

### <a name="step-4-create-and-run-an-indexer"></a>4. Adım: Dizin Oluşturucu oluşturma ve çalıştırma

Şu ana kadar bir veri kaynağı, beceri kümesi ve dizin oluşturdunuz. Bu üç bileşen, her bir parçayı birlikte tek bir çok aşamalı işleme çeken [dizin oluşturucunun](search-indexer-overview.md) parçası olur. Bunları bir dizin oluşturucu birbirine bağlamak için alan eşlemeleri tanımlamanız gerekir.

+ FieldMappings, Beceri öğesinden önce işlenir, kaynak alanları veri kaynağından bir dizindeki hedef alanlara eşleniyor. Alan adları ve türleri her iki uçta da aynıysa, hiçbir eşleme gerekmez.

+ OutputFieldMappings, Beceri sonrasında işlenir ve belge çözme veya zenginleştirene kadar mevcut olmayan sourceFieldNames öğesine başvuruda bulunur. TargetFieldName, dizindeki bir alandır.

Çıkışlara girişlerin takılmalarının yanı sıra, veri yapılarını düzleştirmek için alan eşlemelerini de kullanabilirsiniz. Daha fazla bilgi için bkz. [zenginleştirilmiş alanları aranabilir bir dizine eşleme](cognitive-search-output-field-mapping.md).

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
Aşağıdaki satırları öğesine `Main`ekleyin.

```csharp
    // Create the indexer, map fields, and execute transformations
    Console.WriteLine("Creating the indexer and executing the pipeline...");
    Indexer demoIndexer = CreateDemoIndexer(serviceClient, dataSource, skillset, demoIndex);
```

Dizin oluşturucunun oluşturulması için biraz zaman süreceğini beklemeniz gerekir. Veri kümesi küçük olsa da, analiz becerileri bilgi işlem açısından yoğundur. Görüntü analizi gibi bazı beceriler uzun sürer.

> [!TIP]
> Bir dizin oluşturucu oluşturulduğunda, işlem hattı çağrılır. Verilere ulaşılırken, eşleme girişleri ve çıktıları veya işlemlerin sırası ile ilgili sorun olursa bunlar bu aşamada görüntülenir.

### <a name="explore-creating-the-indexer"></a>Dizin oluşturucuyu oluşturmayı keşfet

Kodu-1 ```"maxFailedItems"``` olarak ayarlanır. Bu, dizin oluşturma altyapısının veri içeri aktarma sırasında hataları yoksaymasına olanak verir. Demo veri kaynağında çok az belge olduğundan bu yararlıdır. Daha büyük bir veri kaynağı için değeri, 0’dan daha büyük bir değere ayarlarsınız.

Ayrıca, ```"dataToExtract"``` olarak ```"contentAndMetadata"```ayarlandığını unutmayın. Bu deyim, dizin oluşturucuya, farklı dosya biçimlerinden içeriği ve her bir dosyayla ilgili meta verileri otomatik olarak ayıklamasını bildirir.

İçerik ayıklandığında, veri kaynağında bulunan görüntülerden metni ayıklamak için `imageAction` değerini ayarlayabilirsiniz. Yapılandırma ```"imageAction"``` olarak ```"generateNormalizedImages"``` ayarlanan, OCR yeteneği ve metin birleştirme beceriyle birlikte, dizin oluşturucunun görüntüden metin ayıklamasını söyler (örneğin, trafiğin "Durdur" sözcüğünün oturum açmasını durdurur) ve içerik alanının bir parçası olarak katıştırmasını söyler. Bu davranış hem belgelerde gömülü olan görüntüler (örneğin, bir PDF’teki görüntü) hem de veri kaynağında bulunan görüntüler (örneğin, bir JPG dosyası) için geçerlidir.

<a name="check-indexer-status"></a>

## <a name="4---monitor-indexing"></a>4-dizin oluşturmayı izleme

Dizin oluşturucu tanımlandıktan sonra, isteği gönderdiğinizde otomatik olarak çalıştırılır. Tanımladığınız bilişsel becerilere bağlı olarak dizin oluşturma beklediğinizden uzun sürebilir. Dizin oluşturucunun hala çalışır durumda olup olmadığını öğrenmek için `GetStatus` yöntemini kullanın.

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

`IndexerExecutionInfo`bir dizin oluşturucunun geçerli durum ve yürütme geçmişini temsil eder.

Uyarılar bazı kaynak dosya ve beceri birleşimleri için geneldir ve her zaman bir sorunu belirtmez. Bu öğreticide, uyarılar zararsızdır (örneğin, JPEG dosyalarında bir metin girişi yok).

Aşağıdaki satırları öğesine `Main`ekleyin.

```csharp
    // Check indexer overall status
    Console.WriteLine("Check the indexer overall status...");
    CheckIndexerOverallStatus(serviceClient, demoIndexer);
```
 
## <a name="5---search"></a>5-arama

Dizin oluşturma işlemi tamamlandıktan sonra, tek tek alanların içeriğini döndüren sorguları çalıştırabilirsiniz. Azure Bilişsel Arama, varsayılan olarak ilk 50 sonucunu döndürür. Varsayılan değerin düzgün çalışması için örnek veriler küçüktür. Ancak, büyük veri kümeleriyle çalışırken, daha fazla sonuç döndürmek için sorgu dizesine parametreleri dahil etmeniz gerekebilir. Yönergeler için bkz. [Azure bilişsel arama 'daki sayfa sonuçları](search-pagination-page-layout.md).

Doğrulama adımı olarak, tüm alanlar için dizini sorgulayın.

Aşağıdaki satırları öğesine `Main`ekleyin.

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

`CreateSearchIndexClient`uygulamanın yapılandırma dosyasında `SearchIndexClient` (appSettings. JSON) depolanan değerleri kullanarak yeni bir oluşturur. Yönetim anahtarı için arama hizmeti sorgu API anahtarının kullanıldığını ve bu anahtarın kullanılmadığını unutmayın.

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
   string searchServiceName = configuration["SearchServiceName"];
   string queryApiKey = configuration["SearchServiceQueryApiKey"];

   SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "demoindex", new SearchCredentials(queryApiKey));
   return indexClient;
}
```

Aşağıdaki kodu `Main` içine ekleyin. İlk try-catch, her alanın adı, türü ve öznitelikleri ile dizin tanımını döndürür. İkincisi, örneğin `Select` `organizations`sonuçlara dahil edilecek alanları belirten parametreli bir sorgudur. Bir arama dizesi, `"*"` tek bir alanın tüm içeriğini döndürür.

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

Bu alıştırmada içerik, languageCode, keyPhrases ve kuruluşlar için yineleyin. Virgülle ayrılmış bir liste kullanarak, [Select](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters.select?view=azure-dotnet) özelliği aracılığıyla birden çok alan döndürebilirsiniz.

<a name="reset"></a>

## <a name="reset-and-rerun"></a>Sıfırlama ve yeniden çalıştırma

Geliştirmede erken deneysel aşamalarda, tasarım yinelemesi için en pratik yaklaşım, nesneleri Azure Bilişsel Arama silmek ve kodunuzun bunları yeniden oluşturması için izin verir. Kaynak adları benzersizdir. Bir nesneyi sildiğinizde, aynı adı kullanarak nesneyi yeniden oluşturabilirsiniz.

Bu öğreticinin örnek kodu mevcut nesneleri denetler ve kodunuzu yeniden çalıştırabilmeniz için onları siler.

Ayrıca, dizinleri, Dizin oluşturucuyu, veri kaynaklarını ve becerileri silmek için portalını de kullanabilirsiniz.

## <a name="takeaways"></a>Paketler

Bu öğreticide, bileşen bölümlerinin oluşturulması yoluyla zenginleştirilmiş bir dizin oluşturma işlem hattı oluşturmaya yönelik temel adımlar gösterilmiştir: bir veri kaynağı, Beceri, dizin ve Dizin Oluşturucu.

[Yerleşik yetenekler](cognitive-search-predefined-skills.md) , Beceri tanımıyla birlikte tanıtılmıştır ve giriş ve çıkışlarla becerilerin zincirlerinin bir araya getirilmiştir. Ayrıca, bir Azure `outputFieldMappings` bilişsel arama hizmetinde işlem hattındaki aranabilir bir dizine yönlendirme değerlerini yönlendirme için Dizin Oluşturucu tanımında gerekli olduğunu öğrenmiş olursunuz.

Son olarak, daha fazla yineleme için sonuçların nasıl test edileceğini ve sistemin nasıl sıfırlanacağını öğrendiniz. Dizine karşı sorgular düzenlendiğinde, zenginleştirilmiş dizin oluşturma işlem hattı tarafından oluşturulan çıktının döndürüldüğünü öğrendiniz. Ayrıca dizin oluşturucu durumunun nasıl denetleneceğini ve işlem hattı yeniden çalıştırılmadan önce hangi nesnelerin silineceğini de öğrendiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir projenin sonunda kendi aboneliğinizde çalışırken, artık ihtiyaç duyulmadığınızda kaynakları kaldırmak iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol gezinti bölmesindeki tüm kaynaklar veya kaynak grupları bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık bir AI zenginleştirme işlem hattındaki tüm nesneler hakkında bilgi sahibi olduğunuza göre, Beceri tanımlarına ve bireysel becerilere daha yakından göz atalım.

> [!div class="nextstepaction"]
> [Beceri oluşturma](cognitive-search-defining-skillset.md)
