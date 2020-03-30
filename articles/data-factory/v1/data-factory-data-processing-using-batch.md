---
title: Veri Fabrikası ve Toplu İşi'ni kullanarak büyük ölçekli veri kümelerini işleme
description: Azure Toplu İşlem'in paralel işleme yeteneğini kullanarak Bir Azure Veri Fabrikası ardışık hattında büyük miktarda verinin nasıl işlenirolduğunu açıklar.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: afc7a7406831568304c2ebd8d9a6c72b497e04e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75972875"
---
# <a name="process-large-scale-datasets-by-using-data-factory-and-batch"></a>Veri Fabrikası ve Toplu İşi'ni kullanarak büyük ölçekli veri kümelerini işleme
> [!NOTE]
> Bu makale, Azure Data Factory’nin genel kullanıma açık olan 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası'ndaki Özel etkinliklere](../transform-data-using-dotnet-custom-activity.md)bakın.

Bu makalede, büyük ölçekli veri kümelerini otomatik ve zamanlanmış bir şekilde hareket ettiren ve işleyen bir örnek çözüm mimarisi açıklanmaktadır. Ayrıca, Veri Fabrikası ve Azure Toplu İş'i kullanarak çözümü uygulamak için uçtan uca bir gözden geçirme sağlar.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu makale, tüm örnek çözümünün bir gözden geçirimi içerdiğinden, tipik bir makaleden daha uzundur. Toplu İşlem ve Veri Fabrikası'nda yeniyseniz, bu hizmetler ve bunların birlikte nasıl çalıştıkları hakkında bilgi edinebilirsiniz. Hizmetler hakkında bir şeyler biliyorsanız ve bir çözüm tasarlıyorsanız/tasarlıyorsanız, makalenin mimari bölümüne odaklanabilirsiniz. Bir prototip veya çözüm geliştiriyorsanız, izbinde adım adım yönergeleri denemek isteyebilirsiniz. Bu içerik ve nasıl kullandığınız la ilgili yorumlarınızı davet ediyoruz.

İlk olarak, Veri Fabrikası ve Toplu İşlem hizmetlerinin buluttaki büyük veri kümelerini işlemenize nasıl yardımcı olabileceğine bakalım.     


## <a name="why-azure-batch"></a>Neden Azure Toplu?
 Büyük ölçekli paralel ve yüksek performanslı bilgi işlem (HPC) uygulamalarını bulutta verimli bir şekilde çalıştırmak için Toplu İşlem'i kullanabilirsiniz. Yönetilen sanal makineler (VM) koleksiyonunda çalışacak bilgi işlem yoğun çalışmayı zamanlayan bir platform hizmetidir. İşlerinizi karşılamak için otomatik olarak hesaplama kaynaklarını ölçeklendirebilir.

Batch hizmetiyle, uygulamalarınızı paralel olarak ve ölçekte yürütmek için Azure işlem kaynaklarını tanımlayın. İsteğe bağlı veya zamanlanmış işler çalıştırabilirsiniz. Bir HPC kümesini, tek tek Sanal Ağlar'ı, sanal ağları veya karmaşık bir iş ve görev planlama altyapısını el ile oluşturmanız, yapılandırmanız ve yönetmeniz gerekmez.

 Toplu İşlem'e aşina değilseniz, aşağıdaki makaleler bu makalede açıklanan çözümün mimarisini/uygulamasını anlamanıza yardımcı olur:   

* [Toplu İşlemin Temelleri](../../batch/batch-technical-overview.md)
* [Batch özelliklerine genel bakış](../../batch/batch-api-basics.md)

İsteğe bağlı olarak, Toplu İşlem hakkında daha fazla bilgi edinmek için [Toplu İşlem belgelerine](https://docs.microsoft.com/azure/batch/)bakın.

## <a name="why-azure-data-factory"></a>Neden Azure Veri Fabrikası?
Data Factory, verilerin taşınmasını ve dönüştürülmesini düzenleyen ve otomatikleştiren bulut tabanlı bir veri tümleştirme hizmetidir. Verileri şirket içi ve bulut veri depolarından merkezi bir veri deposuna taşıyan yönetilen veri ardışık alanları oluşturmak için Veri Fabrikası'nı kullanabilirsiniz. Azure Blob depolama sına örnek olarak örnek olarak sunulur. Azure HDInsight ve Azure Machine Learning gibi hizmetleri kullanarak verileri işlemek/dönüştürmek için Veri Fabrikası'nı kullanabilirsiniz. Veri ardışık düzeneçlerini zamanlanmış bir şekilde (örneğin, saatlik, günlük ve haftalık) çalışacak şekilde zamanlayabilirsiniz. Sorunları belirlemek ve harekete geçmek için boru hatlarını bir bakışta izleyebilir ve yönetebilirsiniz.

  Veri Fabrikası'nı bilmiyorsanız, aşağıdaki makaleler bu makalede açıklanan çözümün mimarisini/uygulamasını anlamanıza yardımcı olur:  

* [Data Factory'ye Giriş](data-factory-introduction.md)
* [İlk veri ardışık sisteminizi oluşturun](data-factory-build-your-first-pipeline.md)   

İsteğe bağlı olarak, Veri Fabrikası hakkında daha fazla bilgi edinmek için [Veri Fabrikası belgelerine](https://docs.microsoft.com/rest/api/datafactory/v1/data-factory-data-factory)bakın.

## <a name="data-factory-and-batch-together"></a>Veri Fabrikası ve Toplu İşlem birlikte
Veri Fabrikası yerleşik faaliyetleri içerir. Örneğin, Kopyala etkinliği verileri bir kaynak veri deposundan hedef veri deposuna kopyalamak/taşımak için kullanılır. Hive etkinliği, Azure'daki Hadoop kümelerini (HDInsight) kullanarak verileri işlemek için kullanılır. Desteklenen dönüşüm etkinlikleri listesi için [Bkz. Veri dönüştürme etkinlikleri.](data-factory-data-transformation-activities.md)

Ayrıca, verileri kendi mantığınızla taşımak veya işlemek için özel .NET etkinlikleri de oluşturabilirsiniz. Bu etkinlikleri bir HDInsight kümesinde veya Toplu İşlem kümesinde çalıştırabilirsiniz. Toplu İş'i kullandığınızda, sağladığınız bir formüle göre havuzu otomatik ölçeklendirecek (iş yüküne göre VM ekleme veya kaldırma) göre yapılandırabilirsiniz.     

## <a name="architecture-of-a-sample-solution"></a>Örnek çözeltinin mimarisi
  Bu makalede açıklanan mimari basit bir çözüm içindir. Ayrıca finansal hizmetler, görüntü işleme ve işleme ve genomik analiz tarafından risk modelleme gibi karmaşık senaryolar ile ilgilidir.

Diyagram, Veri Fabrikası'nın veri hareketini ve işlemeyi nasıl düzenlediğini göstermektedir. Ayrıca, Toplu İşlem'in verileri paralel bir şekilde nasıl işlediğini de gösterir. Kolay başvuru için diyagramı indirin ve yazdırın (11 x 17 inç veya A3 boyutu). Diyagramı yazdırabilmeniz için [Toplu İşlem ve Veri Fabrikası'nı kullanarak HPC ve veri düzenleme](https://go.microsoft.com/fwlink/?LinkId=717686)bilgisine bakın.

[![Büyük ölçekli veri işleme diyagramı](./media/data-factory-data-processing-using-batch/image1.png)](https://go.microsoft.com/fwlink/?LinkId=717686)

Aşağıdaki liste işlemin temel adımlarını sağlar. Çözüm, uçtan uca çözümü oluşturmak için kod ve açıklamalar içerir.

* **Toplu Işlemi bir bilgi işlem düğümleri (VM) havuzuyla yapılandırın.** Düğüm sayısını ve her düğümün boyutunu belirtebilirsiniz.

* Blob depolamasını, Toplu işlem hizmetini, girdi/çıktı verilerini ve verileri hareket ettiren ve dönüştüren etkinlikleri içeren bir iş akışı/ardışık kuruluşla yapılandırılan **bir Veri Fabrikası örneği oluşturun.**

* **Veri Fabrikası ardışık alanında özel bir .NET etkinliği oluşturun.** Etkinlik, Toplu İşlem havuzunda çalışan kullanıcı kodunuzdur.

* **Azure Depolama'da büyük miktarda giriş verilerini blob olarak depolayın.** Veriler mantıksal dilimlere (genellikle zamana göre) ayrılır.

* Veri Fabrikası, ikincil konuma **paralel olarak işlenen verileri kopyalar.**

* **Veri Fabrikası, Toplu İşlem tarafından ayrılan havuzu kullanarak özel etkinliği çalıştırır.** Veri Fabrikası etkinlikleri aynı anda çalıştırabilir. Her etkinlik bir veri dilimini işler. Sonuçlar depoda saklanır.

* **Veri Fabrikası, bir** uygulama üzerinden dağıtım veya diğer araçlarla daha fazla işlem için nihai sonuçları üçüncü bir konuma taşır.

## <a name="implementation-of-the-sample-solution"></a>Örnek çözümün uygulanması
Örnek çözüm kasıtlı olarak basittir. Veri kümelerini işlemek için Veri Fabrikası ve Toplu İşlem'i birlikte nasıl kullanacağınızı göstermek için tasarlanmıştır. Çözüm, bir zaman serisinde düzenlenen giriş dosyalarında "Microsoft" arama teriminin oluşum sayısını sayar. Daha sonra çıkış dosyalarına sayım çıktıları.

**Zaman:** Azure, Veri Fabrikası ve Toplu İşlem'in temellerini biliyorsanız ve aşağıdaki ön koşulları tamamladıysanız, bu çözümün tamamlanması bir ila iki saat sürer.

### <a name="prerequisites"></a>Ön koşullar
#### <a name="azure-subscription"></a>Azure aboneliği
Azure aboneliğiniz yoksa, hızlı bir şekilde ücretsiz bir deneme hesabı oluşturabilirsiniz. Daha fazla bilgi için [Ücretsiz deneme sürümüne](https://azure.microsoft.com/pricing/free-trial/)bakın.

#### <a name="azure-storage-account"></a>Azure depolama hesabı
Verileri bu öğreticide depolamak için bir depolama hesabı kullanırsınız. Depolama hesabınız yoksa, [bkz.](../../storage/common/storage-account-create.md) Örnek çözüm blob depolama kullanır.

#### <a name="azure-batch-account"></a>Azure Batch hesabı
[Azure portalını](https://portal.azure.com/)kullanarak toplu iş hesabı oluşturun. Daha fazla bilgi için toplu [iş hesabı oluştur ve yönetme'ye](../../batch/batch-account-create-portal.md)bakın. Toplu iş hesabı adını ve hesap anahtarını not edin. Toplu Hesap oluşturmak için [Yeni-AzBatchAccount](https://docs.microsoft.com/powershell/module/az.batch/new-azbatchaccount) cmdlet'ini de kullanabilirsiniz. Bu cmdlet nasıl kullanılacağı ile ilgili talimatlar için, [Toplu PowerShell cmdlets ile başlayın](../../batch/batch-powershell-cmdlets-get-started.md)bakın.

Örnek çözüm, verileri bilgi işlem düğümleri havuzunda (yönetilen bir VM koleksiyonu) paralel bir şekilde işlemek için Toplu Iş (dolaylı olarak veri fabrikası boru hattı yoluyla) kullanır.

#### <a name="azure-batch-pool-of-virtual-machines"></a>Sanal makinelerin Azure Toplu Seri havuzu
En az iki işlem düğümü içeren bir Toplu Iş havuzu oluşturun.

1. Azure [portalında,](https://portal.azure.com)sol menüde **Gözat'ı** ve **Toplu Hesaplar'ı**seçin.

1. Toplu Hesap bıçağını açmak için Toplu İş **hesabınızı** seçin.

1. **Havuzlar** döşemesini seçin.

1. **Pools** bıtır'da, havuz eklemek için araç çubuğundaki **Ekle** düğmesini seçin.

   a. Havuz için bir kimlik girin (**Havuz Kimliği**). Havuzun kimliğine dikkat edin. Veri fabrikası çözümlerini oluştururken ihtiyacınız olur.

   b. **İşletim Sistemi Ailesi** ayarı için **Windows Server 2012 R2'yi** belirtin.

   c. Düğüm **fiyatlandırma katmanı**seçin.

   d. **Hedef Adanmış** ayar için değer olarak **2** girin.

   e. Düğüm ayarı başına **Max görevleri** için değer olarak **2** girin.

   f. Havuzu oluşturmak için **Tamam'ı** seçin.

#### <a name="azure-storage-explorer"></a>Azure Storage Gezgini
Depolama projelerinizdeki verileri incelemek ve değiştirmek için [Azure Storage Explorer 6](https://azurestorageexplorer.codeplex.com/) veya [CloudXplorer'ı](https://clumsyleaf.com/products/cloudxplorer) (ClumsyLeaf Software'den) kullanırsınız. Ayrıca, bulut barındırılan uygulamalarınızın günlüklerinde verileri inceleyebilir ve değiştirebilirsiniz.

1. Özel erişim (anonim erişim **yok) ile mycontainer** adlı bir kapsayıcı oluşturun.

1. CloudXplorer kullanıyorsanız, aşağıdaki yapıya sahip klasörler ve alt klasörler oluşturun:

   ![Klasör ve alt klasör yapısı](./media/data-factory-data-processing-using-batch/image3.png)

   `Inputfolder`ve `outputfolder` üst düzey klasörler `mycontainer`. Klasörde `inputfolder` tarih damgalı alt klasörler (YYYY-MM-DD-HH) bulunur.

   Depolama Gezgini kullanıyorsanız, bir sonraki adımda, aşağıdaki `inputfolder/2015-11-16-00/file.txt`adlarla dosya yüklersiniz: , , `inputfolder/2015-11-16-01/file.txt`ve benzeri. Bu adım, klasörleri otomatik olarak oluşturur.

1. **Microsoft**anahtar kelimesine sahip içerikle makinenizde bir metin **dosyası dosyası dosyası.txt** oluşturun. Bir örnek "test özel etkinlik Microsoft test özel etkinlik Microsoft."

1. Dosyayı blob depolamasındaki aşağıdaki giriş klasörlerine yükleyin:

   ![Giriş klasörleri](./media/data-factory-data-processing-using-batch/image4.png)

   Storage Explorer kullanıyorsanız, **file.txt** dosyasını **mycontainer'a**yükleyin. Blob'un bir kopyasını oluşturmak için araç çubuğunda **Kopyala'yı** seçin. **Blob kopyala** iletişim **kutusunda, hedef blob** `inputfolder/2015-11-16-00/file.txt`adını ' da değiştirin. Oluşturmak `inputfolder/2015-11-16-01/file.txt`için bu `inputfolder/2015-11-16-02/file.txt`adımı `inputfolder/2015-11-16-03/file.txt` `inputfolder/2015-11-16-04/file.txt`tekrarlayın , , , ve benzeri. Bu eylem klasörleri otomatik olarak oluşturur.

1. Adlı `customactivitycontainer`başka bir kapsayıcı oluşturun. Özel etkinlik zip dosyasını bu kapsayıcıya yükleyin.

#### <a name="visual-studio"></a>Visual Studio
Veri fabrikası çözümünde kullanılacak özel Toplu İşlem etkinliğini oluşturmak için Visual Studio 2012 veya daha sonra yükleyin.

### <a name="high-level-steps-to-create-the-solution"></a>Çözümü oluşturmak için üst düzey adımlar
1. Veri işleme mantığını içeren özel bir etkinlik oluşturun.

1. Özel etkinliği kullanan bir veri fabrikası oluşturun.

### <a name="create-the-custom-activity"></a>Özel etkinliği oluşturma
Veri fabrikası özel etkinliği bu örnek çözümün kalbidir. Örnek çözüm, özel etkinliği çalıştırmak için Toplu İşlem'i kullanır. Özel etkinliklerin nasıl geliştirilen ve bunları veri fabrikası ardışık hatlarında nasıl kullanacağıhakkında bilgi [için](data-factory-use-custom-activities.md)bkz.

Veri fabrikası ardışık alanında kullanabileceğiniz bir .NET özel etkinlik oluşturmak için, IDotNetActivity arabirimini uygulayan bir sınıfa sahip bir .NET sınıfı kitaplık projesi oluşturursunuz. Bu arabirimin tek bir yöntemi vardır: Yürüt. İşte yöntemin imzası:

```csharp
public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
```

Yöntem, anlamanız gereken birkaç temel bileşene sahiptir:

* Yöntem dört parametre alır:

  * **linkedServices**. Bu parametre, giriş/çıktı veri kaynaklarını (örneğin, blob depolama) veri fabrikasına bağlayan bağlantılı hizmetlerin sayısal bir listesidir. Bu örnekte, hem giriş hem de çıktı için kullanılan Azure Depolama türünün yalnızca bir bağlantılı hizmeti vardır.
  * **veri kümeleri**. Bu parametre, veri kümelerinin sayısal bir listesidir. Giriş ve çıktı veri kümeleri tarafından tanımlanan konumları ve şemaları elde etmek için bu parametreyi kullanabilirsiniz.
  * **etkinliği**. Bu parametre geçerli işlem varlığını temsil eder. Bu durumda, bu bir Toplu Iş hizmetidir.
  * **logger**. Veri metodaracı, ardışık hatlar için "Kullanıcı" günlüğü olarak ortaya çıkan hata ayıklama yorumlarını yazmak için kullanabilirsiniz.
* Yöntem, gelecekte özel etkinlikleri birbirine zincirlemek için kullanılabilecek bir sözlük döndürür. Bu özellik henüz uygulanmadı, bu nedenle yöntemden boş bir sözlük döndürmeniz kaldı.

#### <a name="procedure-create-the-custom-activity"></a>Yordam: Özel etkinliği oluşturma
1. Visual Studio'da bir .NET sınıfı kitaplık projesi oluşturun.

   a. Visual Studio 2012/2013/2015'i başlatın.

   b. **Dosya** > **Yeni** > **Proje'yi**seçin.

   c. **Şablonları**genişletin ve **Visual\#C'yi**seçin. Bu izbarada C,,\#ancak özel etkinliği geliştirmek için herhangi bir .NET dilini kullanabilirsiniz.

   d. Sağdaki proje türleri listesinden **Sınıf Kitaplığı'nı** seçin.

   e. **Ad**için **MyDotNetActivity** girin.

   f. **Konum**için **C:\\ADF'yi** seçin. Yoksa **ADF** klasörünü oluşturun.

   g. Projeyi oluşturmak için **Tamam**'ı seçin.

1. **Araçları** > **nuget paket yöneticisi** > **paket yöneticisi konsolseçin.**

1. Paket Yöneticisi Konsolunda, Microsoft.Azure.Management.DataFactorys'i almak için aşağıdaki komutu uygulayın:

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
1. Azure **Depolama** NuGet paketini projeye aktarın. Bu örnekte Blob Depolama API'sini kullandığınıziçin bu pakete ihtiyacınız var:

    ```powershell
    Install-Package Az.Storage
    ```
1. Projedeki kaynak dosyaya yönergeleri kullanarak aşağıdaki yönergeleri ekleyin:

    ```csharp
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;

    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
1. Ad alanının adını **MyDotNetActivityNS**olarak değiştirin.

    ```csharp
    namespace MyDotNetActivityNS
    ```
1. Sınıfın adını **MyDotNetActivity**olarak değiştirin ve gösterildiği gibi **IDotNetActivity** arabiriminden türetin:

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
1. **IDotNetActivity** arabiriminin **Yürütme** yöntemini **MyDotNetActivity** sınıfına uygulayın (ekleyin). Aşağıdaki örnek kodu yönteme kopyalayın. Bu yöntemde kullanılan mantığın açıklaması için [Yürüt metoduna](#execute-method) bakın.

    ```csharp
    /// <summary>
    /// The Execute method is the only method of IDotNetActivity interface you must implement.
    /// In this sample, the method invokes the Calculate method to perform the core logic.  
    /// </summary>
    public IDictionary<string, string> Execute(
       IEnumerable<LinkedService> linkedServices,
       IEnumerable<Dataset> datasets,
       Activity activity,
       IActivityLogger logger)
    {

       // Declare types for the input and output data stores.
       AzureStorageLinkedService inputLinkedService;

       Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);

       foreach (LinkedService ls in linkedServices)
           logger.Write("linkedService.Name {0}", ls.Name);

       // Use the First method instead of Single because we are using the same
       // Azure Storage linked service for input and output.
       inputLinkedService = linkedServices.First(
           linkedService =>
           linkedService.Name ==
           inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
           as AzureStorageLinkedService;

       string connectionString = inputLinkedService.ConnectionString; // To create an input storage client.
       string folderPath = GetFolderPath(inputDataset);
       string output = string.Empty; // for use later.

       // Create the storage client for input. Pass the connection string.
       CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
       CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();

       // Initialize the continuation token before using it in the do-while loop.
       BlobContinuationToken continuationToken = null;
       do
       {   // get the list of input blobs from the input storage client object.
           BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                    true,
                                    BlobListingDetails.Metadata,
                                    null,
                                    continuationToken,
                                    null,
                                    null);

           // The Calculate method returns the number of occurrences of
           // the search term "Microsoft" in each blob associated
           // with the data slice.
           //
           // The definition of the method is shown in the next step.
           output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");

       } while (continuationToken != null);

       // Get the output dataset by using the name of the dataset matched to a name in the Activity output collection.
       Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

       folderPath = GetFolderPath(outputDataset);

       logger.Write("Writing blob to the folder: {0}", folderPath);

       // Create a storage object for the output blob.
       CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
       // Write the name of the file.
       Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));

       logger.Write("output blob URI: {0}", outputBlobUri.ToString());
       // Create a blob and upload the output text.
       CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
       logger.Write("Writing {0} to the output blob", output);
       outputBlob.UploadText(output);

       // The dictionary can be used to chain custom activities together in the future.
       // This feature is not implemented yet, so just return an empty dictionary.
       return new Dictionary<string, string>();
    }
    ```
1. Sınıfa aşağıdaki yardımcı yöntemleri ekleyin. Bu yöntemler **Yürüt** yöntemi tarafından çağrılır. En önemlisi, **Hesapla** yöntemi her blob aracılığıyla yineleyen kodu yalıtlar.

    ```csharp
    /// <summary>
    /// Gets the folderPath value from the input/output dataset.
    /// </summary>
    private static string GetFolderPath(Dataset dataArtifact)
    {
       if (dataArtifact == null || dataArtifact.Properties == null)
       {
           return null;
       }

       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }

       return blobDataset.FolderPath;
    }

    /// <summary>
    /// Gets the fileName value from the input/output dataset.
    /// </summary>

    private static string GetFileName(Dataset dataArtifact)
    {
       if (dataArtifact == null || dataArtifact.Properties == null)
       {
           return null;
       }

       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }

       return blobDataset.FileName;
    }

    /// <summary>
    /// Iterates through each blob (file) in the folder, counts the number of instances of the search term in the file,
    /// and prepares the output text that is written to the output blob.
    /// </summary>

    public static string Calculate(BlobResultSegment Bresult, IActivityLogger logger, string folderPath, ref BlobContinuationToken token, string searchTerm)
    {
       string output = string.Empty;
       logger.Write("number of blobs found: {0}", Bresult.Results.Count<IListBlobItem>());
       foreach (IListBlobItem listBlobItem in Bresult.Results)
       {
           CloudBlockBlob inputBlob = listBlobItem as CloudBlockBlob;
           if ((inputBlob != null) && (inputBlob.Name.IndexOf("$$$.$$$") == -1))
           {
               string blobText = inputBlob.DownloadText(Encoding.ASCII, null, null, null);
               logger.Write("input blob text: {0}", blobText);
               string[] source = blobText.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);
               var matchQuery = from word in source
                                where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()
                                select word;
               int wordCount = matchQuery.Count();
               output += string.Format("{0} occurrences(s) of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
           }
       }
       return output;
    }
    ```
    GetFolderPath yöntemi, yolu veri kümesinin işaret ettiği klasöre döndürür ve GetFileName yöntemi veri kümesinin işaret ettiği blob/dosyanın adını döndürür.

    ```csharp

    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
    ```

    Hesapla yöntemi, giriş dosyalarındaki (klasördeki lekeler) "Microsoft" anahtar kelimesinin örnek sayısını hesaplar. Arama terimi "Microsoft" kodda sabit kodlanır.

1. Projeyi derle. Menüden **Oluştur'u** seçin ve ardından **Çözüm Oluştur'u**seçin.

1. Windows Gezgini'ni başlatın ve **çöp\\kutusu hata ayıklama** veya **depo gözü\\sürümü klasörüne** gidin. Klasör seçimi yapının türüne bağlıdır.

1. Bin Hata Ayıklama klasöründeki tüm ikilileri içeren bir zip dosyası **MyDotNetActivity.zip** oluşturun. ** \\\\** MyDotNetActivity'i eklemek isteyebilirsiniz. bir hata oluştuğunda soruna neden kaynak kodundaki satır numarası gibi ek ayrıntılar almak için **pdb** dosyası.

   ![Bin\Debug klasör listesi](./media/data-factory-data-processing-using-batch/image5.png)

1. ADFTutorialDataFactory'deki StorageLinkedService bağlantılı hizmetin `customactivitycontainer` kullandığı blob depolama sındaki blob kabına blob olarak **MyDotNetActivity.zip** yükleyin. Zaten yoksa blob kapsayıcıoluşturun. `customactivitycontainer`

#### <a name="execute-method"></a>Yürütme yöntemi
Bu bölümde Yürüt metodundaki kod hakkında daha fazla ayrıntı verilmektedir.

1. Giriş koleksiyonu nda yinelenegelen üyeler [Microsoft.WindowsAzure.Storage.Blob](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob) ad alanında bulunur. Blob koleksiyonunda tekrarlamak için **BlobContinuationToken** sınıfını kullanmanız gerekir. Özünde, döngü çıkmak için mekanizma olarak belirteç ile bir do-while döngü kullanmanız gerekir. Daha fazla bilgi için [bkz.](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) Temel bir döngü burada gösterilmiştir:

    ```csharp
    // Initialize the continuation token.
    BlobContinuationToken continuationToken = null;
    do
    {
    // Get the list of input blobs from the input storage client object.
    BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,

                         true,
                                   BlobListingDetails.Metadata,
                                   null,
                                   continuationToken,
                                   null,
                                   null);
    // Return a string derived from parsing each blob.

     output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");

    } while (continuationToken != null);

    ```
   Daha fazla bilgi için [ListBlobsSegmented](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.listblobssegmented) yöntemiiçin belgelere bakın.

1. Blobs kümesi ile çalışma kodu mantıksal do-while döngü içinde gider. **Yürüt** metodunda, yap döngüsü bloblistesini **Hesapla**adlı bir yönteme geçirir. Yöntem, segmentteki tüm lekeler arasında yinelenebilen bir dize **değişkeni,** adı verilen bir çıktı döndürür.

   **Hesapla** yöntemine geçen blob'daki "Microsoft" arama teriminin oluşum sayısını döndürür.

    ```csharp
    output += string.Format("{0} occurrences of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
    ```
1. **Hesapla** yöntemi tamamlandıktan sonra, yeni bir blob yazılmalıdır. İşlenen her blob seti için, sonuçlarla birlikte yeni bir leke yazılabilir. Yeni bir blob yazmak için, ilk çıktı veri kümesini bulun.

    ```csharp
    // Get the output dataset by using the name of the dataset matched to a name in the Activity output collection.
    Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    ```
1. Kod ayrıca klasör yolunu (depolama kapsayıcısı adı) almak için yardımcı yöntem **GetFolderPath** çağırır.

    ```csharp
    folderPath = GetFolderPath(outputDataset);
    ```
   GetFolderPath yöntemi, DataSet nesnesini FolderPath adında bir özelliğe sahip olan AzureBlobDataSet'e atar.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;

    return blobDataset.FolderPath;
    ```
1. Kod, dosya adını (blob adı) almak için **GetFileName** yöntemini çağırır. Kod, klasör yolunu almak için kullanılan önceki koda benzer.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;

    return blobDataset.FileName;
    ```
1. Dosyanın adı bir URI nesnesi oluşturarak yazılır. URI oluşturucu, kapsayıcı adını döndürmek için **BlobEndpoint** özelliğini kullanır. Çıkış blob URI oluşturmak için klasör yolu ve dosya adı eklenir.  

    ```csharp
    // Write the name of the file.
    Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    ```
1. Dosyanın adı yazıldıktan sonra, hesapla **yönteminden** çıktı dizesini yeni bir blob'a yazabilirsiniz:

    ```csharp
    // Create a blob and upload the output text.
    CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
    logger.Write("Writing {0} to the output blob", output);
    outputBlob.UploadText(output);
    ```

### <a name="create-the-data-factory"></a>Veri fabrikasını oluşturma
Özel [etkinlik oluştur](#create-the-custom-activity) bölümünde, özel bir etkinlik oluşturdunuz ve ikili zip dosyasını ve PDB dosyasını bir blob kapsayıcısına yükledin. Bu bölümde, özel etkinliği kullanan bir ardışık işlem ile bir veri fabrikası oluşturursunuz.

Özel etkinlik için giriş veri kümesi, blob depolamasındaki giriş klasöründeki () blobs (dosyaları)`mycontainer\\inputfolder`temsil eder. Etkinlik için çıktı veri kümesi, blob depolamasındaki`mycontainer\\outputfolder`çıkış klasöründeki çıktı bloblarını temsil eder.

Giriş klasörlerine bir veya daha fazla dosya bırakın:

```
mycontainer -\> inputfolder
    2015-11-16-00
    2015-11-16-01
    2015-11-16-02
    2015-11-16-03
    2015-11-16-04
```

Örneğin, aşağıdaki içeriği içeren bir dosyayı (file.txt) klasörlerin her birine bırakın:

```
test custom activity Microsoft test custom activity Microsoft
```

Her giriş klasörü, klasörün iki veya daha fazla dosyası olsa bile veri fabrikasındaki bir dilime karşılık gelir. Her dilim ardışık işlem le işlendiğinde, özel etkinlik o dilimin giriş klasöründeki tüm lekeleri yineler.

Aynı içeriğe sahip beş çıktı dosyası görürsünüz. Örneğin, 2015-11-16-00 klasöründe dosyayı işleyen çıktı dosyası aşağıdaki içeriğe sahiptir:

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
```

Giriş klasörüne aynı içeriğe sahip birden çok dosyayı (file.txt, file2.txt, file3.txt) düşürürseniz, çıktı dosyasında aşağıdaki içeriği görürsünüz. Her klasör (2015-11-16-00, vb.) klasöründe birden çok giriş dosyası olsa bile bu örnekteki bir dilime karşılık gelir.

```csharp
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file2.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file3.txt.
```

Çıktı dosyası, dilimle ilişkili klasördeki (2015-11-16-00) her giriş dosyası (blob) için bir tane olmak üzere üç satıra sahiptir.

Her etkinlik çalışması için bir görev oluşturulur. Bu örnekte, boru hattında yalnızca bir etkinlik vardır. Bir dilim ardışık işlem tarafından işlendiğinde, özel etkinlik dilimişlemek için Toplu İşlem'de çalışır. Beş dilim olduğundan (her dilimde birden çok blob veya dosya olabilir), Toplu İşlem'de beş görev oluşturulur. Bir görev Toplu İşlem'de çalıştığında, çalışan özel etkinliktir.

Aşağıdaki izlik ek ayrıntılar sağlar.

#### <a name="step-1-create-the-data-factory"></a>Adım 1: Veri fabrikasını oluşturma
1. [Azure portalında](https://portal.azure.com/)oturum açmadıktan sonra aşağıdaki adımları izleyin:

   a. Sol menüde **Yenİ'yi** seçin.

   b. **Yeni** bıçakta **Veri + Analitik'i** seçin.

   c. **Veri analizi** bıçağında **Veri Fabrikası'nı** seçin.

1. Yeni **veri fabrikası** bıçağında, ad için **CustomActivityFactory'yi** girin. Veri fabrikasının adı genel olarak benzersiz olmalıdır. "Veri fabrikası adı CustomActivityFactory kullanılamıyor" hatasını alırsanız, veri fabrikasının adını değiştirin. Örneğin, adınızı KullanınCustomActivityFactory ve yeniden veri fabrikası oluşturun.

1. **KAYNAK GRUBU Ad'ı**seçin ve varolan bir kaynak grubu seçin veya bir kaynak grubu oluşturun.

1. Abonelik ve veri fabrikasının oluşturulmasını istediğiniz bölgenin doğru olduğunu doğrulayın.

1. Yeni veri **fabrikası** bıçağında **Oluştur'u** seçin.

1. Veri fabrikası portalın panosunda oluşturulur.

1. Veri fabrikası başarıyla oluşturulduktan sonra, veri **fabrikasının** içeriğini gösteren Veri fabrikası sayfasını görürsünüz.

   ![Veri fabrikası sayfası](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a>Adım 2: Bağlantılı hizmetler oluşturma
Bağlantılı hizmetler, veri depolarını veya bilgi işlem hizmetlerini bir veri fabrikasına bağlar. Bu adımda, depolama hesabınızı ve Toplu iş hesabınızı veri fabrikanıza bağlarsınız.

#### <a name="create-an-azure-storage-linked-service"></a>Azure Depolama bağlı hizmeti oluşturma
1. Author'ı seçin ve **CustomActivityFactory**için **Veri fabrika** bıçağına **fayans dağıtın.** Veri Fabrikası Düzenleyicisi görüntülenir.

1. Komut çubuğunda **Yeni veri deposu'nu** seçin ve **Azure depolama alanını seçin.** Düzenleyicide Depolama bağlantılı bir hizmet oluşturmak için kullandığınız JSON komut dosyası görüntülenir.

   ![Yeni veri deposu](./media/data-factory-data-processing-using-batch/image7.png)

1. **Hesap adı** değerini depolama hesabınızın adıyla değiştirin. **Hesap anahtarı** değerini depolama hesabının erişim anahtarıyla değiştirin. Depolama erişim anahtarınızı nasıl alacağınızı öğrenmek için [bkz.](../../storage/common/storage-account-keys-manage.md)

1. Bağlı hizmeti dağıtmak için komut çubuğunda **Dağıt**’ı seçin.

   ![Dağıtma](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-an-azure-batch-linked-service"></a>Azure Toplu İş bağlantılı bir hizmet oluşturma
Bu adımda, Toplu Iş hesabınız için veri fabrikası özel etkinliğini çalıştırmak için kullanılan bağlantılı bir hizmet oluşturursunuz.

1. Komut çubuğunda **Yeni işlem'i** seçin ve **Azure Toplu İş'i seçin.** Düzenleyicide Toplu Iş bağlantısı hizmeti oluşturmak için kullandığınız JSON komut dosyası görüntülenir.

1. JSON komut dosyasında:

   a. **Hesap adını** Toplu İşlem hesabınızın adıile değiştirin.

   b. **Erişim anahtarını** Toplu İşlem hesabının erişim anahtarıyla değiştirin.

   c. **PoolName** özelliği için havuzun kimliğini girin. Bu özellik için havuz adını veya havuz kimliğini belirtebilirsiniz.

   d. **BatchUri** JSON özelliği için toplu URI girin.

      > [!IMPORTANT]
      > **Toplu Hesap** bıçağının URL'si aşağıdaki \<biçimdedir: hesap adı.\> \<bölge\>.batch.azure.com. JSON komut dosyasındaki **batchUri** özelliği için a88"hesap adını kaldırmanız gerekir." ** URL'den. `"batchUri": "https://eastus.batch.azure.com"` bunun bir örneğidir.
      >
      >

      ![Toplu Hesap bıçağı](./media/data-factory-data-processing-using-batch/image9.png)

      **poolName** özelliği için, havuzun adı yerine havuzun kimliğini de belirtebilirsiniz.

      > [!NOTE]
      > Veri Fabrikası hizmeti, HDInsight için olduğu gibi Toplu İşlem için isteğe bağlı bir seçeneği desteklemez. Bir veri fabrikasında yalnızca kendi Toplu İşlem havuzunuzu kullanabilirsiniz.
      >
      >

   e. **LinkedServiceName** özelliği için **StorageLinkedService'i** belirtin. Bu bağlantılı hizmeti önceki adımda oluşturdunuz. Bu depolama dosyaları ve günlükleri için bir hazırlama alanı olarak kullanılır.

1. Bağlı hizmeti dağıtmak için komut çubuğunda **Dağıt**’ı seçin.

#### <a name="step-3-create-datasets"></a>Adım 3: Veri kümeleri oluşturma
Bu adımda, giriş ve çıktı verilerini temsil edecek veri kümeleri oluşturursunuz.

#### <a name="create-the-input-dataset"></a>Girdi veri kümesini oluşturma
1. Veri Fabrikası Düzenleyicisi'nde araç çubuğundaki **Yeni veri kümesi** düğmesini seçin. Açılan listeden **Azure Blob depolama alanını** seçin.

1. Sağ bölmedeki JSON komut dosyasını aşağıdaki JSON parçacığıyla değiştirin:

    ```json
    {
       "name": "InputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
               "format": {
                   "type": "TextFormat"
               },
               "partitionedBy": [
                   {
                       "name": "Year",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "yyyy"
                       }
                   },
                   {
                       "name": "Month",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "MM"
                       }
                   },
                   {
                       "name": "Day",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "dd"
                       }
                   },
                   {
                       "name": "Hour",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "HH"
                       }
                   }
               ]
           },
           "availability": {
               "frequency": "Hour",
               "interval": 1
           },
           "external": true,
           "policy": {}
       }
    }
    ```

    Daha sonra bu izne 2015-11-16T00:00:00Z ve bitiş saati 2015-11-16T05:00:00Z ile bir boru hattı oluşturursunuz. Saatlik veri üretmek üzere zamanlanır, bu nedenle beş giriş/çıkış dilimi vardır **(00:00:00**-\> **05**:00:00 arasında).

    Giriş veri kümesinin **sıklığı** ve **aralığı** **Saat** ve **1**olarak ayarlanır, bu da giriş diliminin saatlik olarak kullanılabilir olduğu anlamına gelir.

    Her dilimin başlangıç saati, önceki JSON parçacığındaki **SliceStart** sistem değişkeni tarafından temsil edilir. Her dilimin başlangıç saatleri aşağıda veda edinebilirsiniz.

    | **Dilim** | **Başlangıç saati**          |
    |-----------|-------------------------|
    | 1         | 2015-11-16T**00**:00:00 |
    | 2         | 2015-11-16T**01**:00:00 |
    | 3         | 2015-11-16T**02**:00:00 |
    | 4         | 2015-11-16T**03**:00:00 |
    | 5         | 2015-11-16T**04**:00:00 |

    **FolderPath,** dilim başlangıç saatinin yıl, ay, gün ve saat kısmı **(SliceStart)** kullanılarak hesaplanır. Giriş klasörü bir dilime nasıl eşlenir.

    | **Dilim** | **Başlangıç saati**          | **Giriş klasörü**  |
    |-----------|-------------------------|-------------------|
    | 1         | 2015-11-16T**00**:00:00 | 2015-11-16-**00** |
    | 2         | 2015-11-16T**01**:00:00 | 2015-11-16-**01** |
    | 3         | 2015-11-16T**02**:00:00 | 2015-11-16-**02** |
    | 4         | 2015-11-16T**03**:00:00 | 2015-11-16-**03** |
    | 5         | 2015-11-16T**04**:00:00 | 2015-11-16-**04** |

1. **InputDataset** tablosunu oluşturmak ve dağıtmak için araç çubuğunda **Dağıt'ı** seçin.

#### <a name="create-the-output-dataset"></a>Çıktı veri kümesini oluşturma
Bu adımda, çıktı verilerini temsil etmek için AzureBlob türünden başka bir veri kümesi oluşturursunuz.

1. Veri Fabrikası Düzenleyicisi'nde araç çubuğundaki **Yeni veri kümesi** düğmesini seçin. Açılan listeden **Azure Blob depolama alanını** seçin.

1. Sağ bölmedeki JSON komut dosyasını aşağıdaki JSON parçacığıyla değiştirin:

    ```json
    {
       "name": "OutputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "fileName": "{slice}.txt",
               "folderPath": "mycontainer/outputfolder",
               "partitionedBy": [
                   {
                       "name": "slice",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "yyyy-MM-dd-HH"
                       }
                   }
               ]
           },
           "availability": {
               "frequency": "Hour",
               "interval": 1
           }
       }
    }
    ```

    Her giriş dilimi için bir çıktı blob/dosya oluşturulur. Çıktı dosyasının her dilim için nasıl adlandırıldığını aşağıda göreiz. Tüm çıktı dosyaları tek bir çıktı `mycontainer\\outputfolder`klasöründe oluşturulur.

    | **Dilim** | **Başlangıç saati**          | **Çıktı dosyası**       |
    |-----------|-------------------------|-----------------------|
    | 1         | 2015-11-16T**00**:00:00 | 2015-11-16-**00.txt** |
    | 2         | 2015-11-16T**01**:00:00 | 2015-11-16-**01.txt** |
    | 3         | 2015-11-16T**02**:00:00 | 2015-11-16-**02.txt** |
    | 4         | 2015-11-16T**03**:00:00 | 2015-11-16-**03.txt** |
    | 5         | 2015-11-16T**04**:00:00 | 2015-11-16-**04.txt** |

    Giriş klasöründeki tüm dosyaların (örneğin, 2015-11-16-00) 2015-11-16-00 başlangıç saatine sahip bir dilimin parçası olduğunu unutmayın. Bu dilim işlendiğinde, özel etkinlik her dosyayı tarar ve çıktı dosyasında "Microsoft" arama teriminin oluşum sayısıyla bir satır üretir. Klasör2015-11-16-00 üç dosya varsa, çıkış dosyasında üç satır vardır 2015-11-16-00.txt.

1. **OutputDataset'i**oluşturmak ve dağıtmak için araç çubuğunda **Dağıt'ı** seçin.

#### <a name="step-4-create-and-run-the-pipeline-with-a-custom-activity"></a>Adım 4: Özel bir etkinlikle ardışık hatlar oluşturma ve çalıştırma
Bu adımda, tek bir etkinlik, daha önce oluşturduğunuz özel etkinlik içeren bir ardışık işlem oluşturursunuz.

> [!IMPORTANT]
> Blob kapsayıcısındaki klasörlere **file.txt** yüklemediyseniz, bunu ardışık nokta oluşturmadan önce yapın. **IsPaused** özelliği, json ardışık ardışık boru hattında false olarak ayarlanır, bu nedenle **başlangıç** tarihi geçmişte olduğu için boru hattı hemen çalışır.
>
>

1. Veri Fabrikası Düzenleyicisi'nde komut çubuğunda **Yeni ardışık hatlar'ı** seçin. Komutu görmüyorsanız, görüntülemek için elips simgesini seçin.

1. Sağ bölmedeki JSON komut dosyasını aşağıdaki JSON parçacığıyla değiştirin:

    ```json
    {
       "name": "PipelineCustom",
       "properties": {
           "description": "Use custom activity",
           "activities": [
               {
                   "type": "DotNetActivity",
                   "typeProperties": {
                       "assemblyName": "MyDotNetActivity.dll",
                       "entryPoint": "MyDotNetActivityNS.MyDotNetActivity",
                       "packageLinkedService": "AzureStorageLinkedService",
                       "packageFile": "customactivitycontainer/MyDotNetActivity.zip"
                   },
                   "inputs": [
                       {
                           "name": "InputDataset"
                       }
                   ],
                   "outputs": [
                       {
                           "name": "OutputDataset"
                       }
                   ],
                   "policy": {
                       "timeout": "00:30:00",
                       "concurrency": 5,
                       "retry": 3
                   },
                   "scheduler": {
                       "frequency": "Hour",
                       "interval": 1
                   },
                   "name": "MyDotNetActivity",
                   "linkedServiceName": "AzureBatchLinkedService"
               }
           ],
           "start": "2015-11-16T00:00:00Z",
           "end": "2015-11-16T05:00:00Z",
           "isPaused": false
      }
    }
    ```
   Aşağıdaki noktalara dikkat edin:

   * Boru hattında yalnızca bir etkinlik var ve bu tür **DotNetActivity'den.**
   * **AssemblyName** DLL **MyDotNetActivity.dll**adına ayarlanır.
   * **EntryPoint** **MyDotNetActivityNS.MyDotNetActivity**ayarlanır. Temelde \<isim alanı.\> \<kodunuzda\> sınıf adı.
   * **PackageLinkedService,** özel etkinlik zip dosyasını içeren blob depolamasını işaret eden **StorageLinkedService**olarak ayarlanır. Giriş/çıkış dosyaları ve özel etkinlik zip dosyası için farklı depolama hesapları kullanıyorsanız, başka bir Depolama bağlantılı hizmet oluşturmanız gerekir. Bu makalede, aynı depolama hesabı kullandığınızvarsayar.
   * **PackageFile** **customactivitycontainer/MyDotNetActivity.zip**olarak ayarlanır. Bu \<format konteyner içindethezip\>/\<nameofthezip.zip\>.
   * Özel etkinlik, giriş olarak **InputDataset'i** ve çıktı olarak **ÇıktıDataset'i** alır.
   * Özel etkinliğin **linkedServiceName** özelliği, Veri Fabrikası'na özel etkinliğin Toplu İşlem'de çalışması gerektiğini söyleyen **AzureBatchLinkedService'e**işaret ediyor.
   * **Eşzamanlılık** ayarı önemlidir. Toplu İşlem havuzunda iki veya daha fazla işlem düğümü olsa bile varsayılan değeri 1 olarak kullanırsanız, dilimler birbiri ardına işlenir. Bu nedenle, Toplu iş paralel işleme yeteneğinden yararlanamam. **Eşzamanlılığı** daha yüksek bir değere ayarlarsanız, örneğin 2, iki dilimin (Toplu İşlem'deki iki görevle karşılık gelir) aynı anda işlenebilir anlamına gelir. Bu durumda, Toplu İşlem havuzundaki her iki VM kullanılır. Eşzamanlılık özelliğini uygun şekilde ayarlayın.
   * Varsayılan olarak herhangi bir noktada VM'de yalnızca bir görev (dilim) yürütülür. Varsayılan olarak, **VM başına maksimum görevler** Toplu Iş havuzu için 1 olarak ayarlanır. Ön koşulların bir parçası olarak, bu özellik 2 olarak ayarlanmış bir havuz oluşturdunuz. Bu nedenle, iki veri fabrikası dilimi aynı anda bir VM üzerinde çalıştırılabilir.
     - **IsPaused** özelliği varsayılan olarak false olarak ayarlanır. Dilimler geçmişte başladığı için ardışık işlem hemen bu örnekte çalışır. Bu özelliği, ardışık alanı duraklatmak için **doğru** olarak ayarlayabilir ve yeniden başlatmak için **false'a** geri ayarlayabilirsiniz.
     -   **Başlangıç** ve **bitiş** saatleri beş saat arayla. Dilimler saatlik olarak üretilir, böylece boru hattı tarafından beş dilim üretilir.

1. İşlem hattını dağıtmak için komut çubuğundan **Dağıt**’ı seçin.

#### <a name="step-5-test-the-pipeline"></a>Adım 5: Boru hattını test edin
Bu adımda, giriş klasörlerine dosyaları bırakarak ardışık hattı sınarsınız. Her giriş klasörü için bir dosya ile ardışık hatlar geçirerek başlayın.

1. Azure portalındaki **Veri fabrika** bıçağında **Diyagram'ı**seçin.

   ![Diyagram](./media/data-factory-data-processing-using-batch/image10.png)

1. **Diyagram** görünümünde, giriş veri kümesi **InputDataset'i**çift tıklatın.

   ![GirişDataset](./media/data-factory-data-processing-using-batch/image11.png)

1. **InputDataset** bıçağı beş dilimin tümü hazır görünür. Her dilim için **DILIM BAŞLANGıÇ SAATine** ve **DILIM BITIŞ SAATINe** dikkat edin.

   ![Giriş dilimi başlangıç ve bitiş saatleri](./media/data-factory-data-processing-using-batch/image12.png)

1. **Diyagram** görünümünde **OutputDataset'i**seçin.

1. Beş çıktı dilimi, üretildiklerinde **Hazır** durumunda görünür.

   ![Çıkış dilimi başlangıç ve bitiş saatleri](./media/data-factory-data-processing-using-batch/image13.png)

1. Dilimlerle ilişkili görevleri görüntülemek ve her dilimin hangi VM üzerinde koştuğuna görmek için portalı kullanın. Daha fazla bilgi için [Veri Fabrikası ve Toplu Tümleştirme](#data-factory-and-batch-integration) bölümüne bakın.

1. Çıktı dosyaları blob `outputfolder` depolama altında görünür. `mycontainer`

   ![Depolamadaki çıktı dosyaları](./media/data-factory-data-processing-using-batch/image15.png)

   Her giriş dilimi için bir tane olmak üzere beş çıktı dosyası listelenir. Çıktı dosyalarının her biri aşağıdaki çıktıya benzer içeriğe sahiptir:

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
    ```
   Aşağıdaki diyagram, veri fabrikası dilimlerinin Toplu İş'teki görevlerle nasıl eşleşiş gösterdiğini göstermektedir. Bu örnekte, bir dilimin yalnızca bir çalışması vardır.

   ![Dilim eşleme diyagramı](./media/data-factory-data-processing-using-batch/image16.png)

1. Şimdi bir klasörde birden çok dosya ile deneyin. **Dosya file2.txt**, **file3.txt**, **file4.txt**ve **file5.txt** dosya.txt klasöründe **2015-11-06-01**ile aynı içerikle dosya oluşturun .

1. Çıktı klasöründe, çıkış dosyasını silin **2015-11-16-01.txt**.

1. **OutputDataset** bıçağında, **16/11/2015 01:00:00 OLARAK**AYARLANAN **SLICE START TIME** ile dilime sağ tıklayın. Dilimi yeniden çalıştırmak/yeniden işlemek için **Çalıştır'ı** seçin. Dilimde artık bir dosya yerine beş dosya bulunuyor.

    ![Çalıştırın](./media/data-factory-data-processing-using-batch/image17.png)

1. Dilim çalıştırılan ve durumu **Hazır**olduktan sonra, bu dilimin çıktı dosyasındaki içeriği doğrulayın (**2015-11-16-01.txt**). Çıktı dosyası blob `mycontainer` `outputfolder` depolama altında görünür. Dilimin her dosyası için bir satır olmalıdır.

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file2.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file3.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file4.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file5.txt.
    ```

> [!NOTE]
> Beş giriş dosyasıyla denemeden önce 2015-11-16-01.txt çıktı dosyasını silmediyseniz, önceki dilim çalıştırAn bir satır ve geçerli dilim çalıştırAn beş satır görürsünüz. Varsayılan olarak, içerik zaten varsa çıktı dosyasına eklenir.
>
>

#### <a name="data-factory-and-batch-integration"></a>Veri Fabrikası ve Toplu Entegrasyon
Veri Fabrikası hizmeti, Toplu Iş'te `adf-poolname:job-xxx`adı olan bir iş oluşturur.

![Toplu işler](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

Bir dilimin her etkinlik çalışması için işteki bir görev oluşturulur. 10 dilim işlemeye hazırsa, işte 10 görev oluşturulur. Havuzda birden çok işlem düğümü varsa, birden fazla dilimin paralel olarak çalışmasını sağlayabilirsiniz. İşlem düğümü başına en fazla görev sayısı birden büyük olarak ayarlanmışsa, aynı işlemde birden fazla dilim çalıştırılabilir.

Bu örnekte, beş dilim vardır, bu nedenle Toplu Iş'te beş görev vardır. Veri fabrikasında json boru hattında **5'e** ayarlanmış **eşzamanlılık** ve **2** VM'li Toplu İşlem havuzunda **2'ye** ayarlanan **VM başına maksimum görevler** ile görevler hızlı çalışır. (Görevlerin başlangıç ve bitiş saatlerini denetleyin.)

Toplu Iş'i ve dilimlerle ilişkili görevlerini görüntülemek ve her dilimin hangi VM'de koştuğunu görmek için portalı kullanın.

![Toplu iş görevleri](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-the-pipeline"></a>İşlem hattında hata ayıklama
Hata ayıklama birkaç temel tekniklerden oluşur.

1. Giriş dilimi **Hazır**olarak ayarlanmıyorsa, giriş klasörü yapısının doğru olduğunu ve giriş klasörlerinde file.txt'nin olduğunu onaylayın.

   ![Giriş klasörü yapısı](./media/data-factory-data-processing-using-batch/image3.png)

1. Özel etkinliğinizin **Yürüt metodunda,** sorunları gidermenize yardımcı olan bilgileri günlüğe kaydetmek için **IActivityLogger** nesnesini kullanın. Günlüğe kaydedilen iletiler kullanıcı\_0.log dosyasında gösteriş.

   **OutputDataset** bıçak üzerinde, o dilim için **Veri dilimi** bıçak görmek için dilim seçin. **Etkinlik çalışır**altında, dilim için çalışan bir etkinlik görürsünüz. Komut çubuğunda **Çalıştır'ı** seçerseniz, aynı dilim için başka bir etkinlik çalıştırma başlatabilirsiniz.

   Etkinlik çalıştır'ı seçtiğinizde, günlük dosyalarının bir listesini içeren **Etkinlik çalıştır ayrıntıları** bıçak bakın. Kullanıcı\_0.log dosyasında günlüğe kaydedilmiş iletileri görürsünüz. Bir hata oluştuğunda, yeniden deneme sayısı ardışık hatlar/etkinlik JSON'da 3 olarak ayarlandığından üç etkinlik çalışır. Etkinlik çalıştır'ı seçtiğinizde, hatayı gidermek için gözden geçirebileceğiniz günlük dosyalarını görürsünüz.

   ![OutputDataset ve Veri dilim bıçakları](./media/data-factory-data-processing-using-batch/image18.png)

   Günlük dosyaları **listesinde, user-0.log'u**seçin. Sağ panelde, **IActivityLogger.Write** yöntemini kullanmanın sonuçları görüntülenir.

   ![Etkinlik çalıştırma ayrıntıları bıçak](./media/data-factory-data-processing-using-batch/image19.png)

   Sistem hata iletileri ve özel durumlar için sistem-0.log'u denetleyin.

    ```
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...

    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...

    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module

    Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully
    ```
1. Hata ayrıntıları bir hata oluştuğunda arama yığını gibi bilgilere sahip olacak şekilde ZIP dosyasına **PDB** dosyasını ekleyin.

1. Özel etkinlik için zip dosyasındaki tüm dosyalar alt klasörolmadan en üst düzeyde olmalıdır.

   ![Özel etkinlik zip dosya listesi](./media/data-factory-data-processing-using-batch/image20.png)

1. **AssemblyName** (MyDotNetActivity.dll), **entryPoint** (MyDotNetActivity.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip) ve **packageLinkedService(zip** dosyasını içeren blob depolamayı işaret etmelidir) doğru değerlere ayarlanmış olduğundan emin olun.

1. Bir hatayı düzelttiyseniz ve dilimi yeniden işlemek istiyorsanız, **OutputDataset** bıçağındaki dilime sağ tıklayın ve **Çalıştır'ı**seçin.

   ![OutputDataset bıçak Çalıştırma seçeneği](./media/data-factory-data-processing-using-batch/image21.png)

   > [!NOTE]
   > Blob depolama alanınızda bir `adfjobs`kapsayıcı var. Bu kapsayıcı otomatik olarak silinmez, ancak çözümü test ettikten sonra güvenli bir şekilde silebilirsiniz. Benzer şekilde, veri fabrikası çözümü adlı `adf-\<pool ID/name\>:job-0000000001`bir Toplu iş oluşturur. İsterseniz çözümü test ettikten sonra bu işi silebilirsiniz.
   >
   >
1. Özel etkinlik, paketinizdeki **app.config** dosyasını kullanmaz. Bu nedenle, kodunuz yapılandırma dosyasından herhangi bir bağlantı dizeleri okursa, çalışma zamanında çalışmaz. Toplu İşlem'i kullandığınızda en iyi uygulama Azure Key Vault'ta herhangi bir sır saklamaktır. Ardından, anahtar kasasını korumak ve sertifikayı Toplu İşlem havuzuna dağıtmak için sertifika tabanlı bir hizmet sorumlusu kullanın. .NET özel etkinliği, çalışma zamanındaki anahtar kasasından sırlara erişebilir. Bu genel çözüm, yalnızca bir bağlantı dizesini değil, herhangi bir gizli türe ölçeklenebilir.

    Daha kolay bir geçici çözüm vardır, ancak en iyi uygulama değildir. Bağlantı dizesi ayarlarına sahip bir SQL veritabanı bağlantılı hizmet oluşturabilirsiniz. Ardından, bağlı hizmeti kullanan bir veri kümesi oluşturabilir ve veri kümesini özel .NET etkinliğine sahte giriş veri kümesi olarak zincirleyebilirsiniz. Daha sonra, özel etkinlik kodunda bağlantılı hizmetin bağlantı dizelerine erişebilirsiniz. Çalışma zamanında iyi çalışır.  

#### <a name="extend-the-sample"></a>Örneği uzatın
Veri Fabrikası ve Toplu İşlem özellikleri hakkında daha fazla bilgi edinmek için bu örneği genişletebilirsiniz. Örneğin, dilimleri farklı bir zaman aralığında işlemek için aşağıdaki adımları izleyin:

1. Aşağıdaki alt klasörleri `inputfolder`ekleyin: 2015-11-16-05, 2015-11-16-06, 201-11-16-07, 2011-11-16-08 ve 2015-11-16-09. Giriş dosyalarını bu klasörlere yerleştirin. Boru hattının bitiş saatini `2015-11-16T05:00:00Z` ' `2015-11-16T10:00:00Z`den' e değiştirin **Diyagram** görünümünde, **GirişDataset'i** çift tıklatın ve giriş dilimlerinin hazır olduğunu onaylayın. Çıktı dilimlerinin durumunu görmek için **OutputDataset'i** çift tıklatın. **Hazır** durumundalarsa, çıktı dosyaları için çıktı klasörünü denetleyin.

1. Çözümünüzünüzün performansını, özellikle toplu toplu iş üzerinde gerçekleşen işlemleri nasıl etkilediğini anlamak için **eşzamanlılık** ayarını artırın veya azaltın. **Eşzamanlılık** ayarı hakkında daha fazla bilgi için bkz: "Adım 4: Özel bir etkinlikle ardışık hatlar oluşturma ve çalıştırma."

1. **VM başına**daha yüksek/düşük Maksimum görevleri olan bir havuz oluşturun. Oluşturduğunuz yeni havuzu kullanmak için veri fabrikası çözümünde Toplu Iş bağlantılı hizmeti güncelleştirin. **VM** ayarı başına maksimum görevler hakkında daha fazla bilgi için bkz: "Adım 4: Özel bir etkinlikle ardışık hatlar oluşturma ve çalıştırma."

1. **Otomatik ölçeközelliğine** sahip bir Toplu Iş havuzu oluşturun. Toplu iş havuzundaki otomatik ölçekleme işlem düğümleri, uygulamanız tarafından kullanılan işlem gücünün dinamik ayarıdır.

    Örnek formül burada aşağıdaki davranışı elde eder. Havuz başlangıçta oluşturulduğunda, bir VM ile başlar. $PendingTasks ölçümü, çalışan ve etkin (sıralanmış) durumlardaki görev sayısını tanımlar. Formül, son 180 saniyeiçinde bekleyen görevlerin ortalama sayısını bulur ve TargetDedicated'ı buna göre ayarlar. TargetDedicated'In asla 25 VM'yi geçmemesini sağlar. Yeni görevler gönderildikçe, havuz otomatik olarak büyür. Görevler tamamlandıkça, VM'ler birer birer serbest kalır ve otomatik ölçekleme bu VM'leri küçültür. BaşlangıçNumberOfVM'leri ve maxNumberofVM'leri ihtiyaçlarınıza göre ayarlayabilirsiniz.

    Otomatik ölçekformülü:

    ```
    startingNumberOfVMs = 1;
    maxNumberofVMs = 25;
    pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
    pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
    $TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
    ```

   Daha fazla bilgi için bkz. [Toplu İşlem havuzunda otomatik olarak hesap düğümlerini ölçeklendirin.](../../batch/batch-automatic-scaling.md)

   Havuz varsayılan otomatik [ÖlçeklendirmeAralığı](https://msdn.microsoft.com/library/azure/dn820173.aspx)kullanıyorsa, Toplu İşlem hizmetinin özel etkinliği çalıştırmadan önce VM'yi hazırlaması 15 ila 30 dakika sürebilir. Havuz farklı bir otomatik ÖlçeklendirmeAralığı kullanıyorsa, Toplu İşlem hizmeti otomatik ÖlçeklendirmeAralığı artı 10 dakika sürebilir.

1. Örnek çözümde, **Yürüt metoduna** çıktı veri dilimi oluşturmak için giriş veri dilimini işleyen Hesapla **yöntemi** çağırır. Giriş verilerini işlemek için kendi yönteminizi yazabilir ve **Yürüt** metodundaki **Hesapla** yöntemi çağrısını yönteminizin çağrısıyla değiştirebilirsiniz.

### <a name="next-steps-consume-the-data"></a>Sonraki adımlar: Verileri tüketin
Verileri işledikten sonra, Power BI gibi çevrimiçi araçlarla tüketebilirsiniz. Power BI'yi anlamanıza yardımcı olacak bağlantılar ve Azure'da nasıl kullanılacağı aşağıda açıklanmıştır:

* [Power BI'de bir veri kümesini keşfedin](https://powerbi.microsoft.com/documentation/powerbi-service-get-data/)
* [Power BI Desktop ile başlayın](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)
* [Power BI'de verileri yenileyin](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/)
* [Azure ve Power BI: Temel genel bakış](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a>Başvurular
* [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)

  * [Veri Fabrikası hizmetine giriş](data-factory-introduction.md)
  * [Veri Fabrikası ile başlayın](data-factory-build-your-first-pipeline.md)
  * [Veri Fabrikası ardışık hattında özel etkinlikler kullanma](data-factory-use-custom-activities.md)
* [Azure Batch](https://azure.microsoft.com/documentation/services/batch/)

  * [Toplu İşlemin Temelleri](../../batch/batch-technical-overview.md)
  * [Toplu İşlem özelliklerine Genel Bakış](../../batch/batch-api-basics.md)
  * [Azure portalında Toplu İşlem hesabı oluşturma ve yönetme](../../batch/batch-account-create-portal.md)
  * [.NET için Toplu İstemci kitaplığı ile başlayın](../../batch/quick-run-dotnet.md)

[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: https://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
