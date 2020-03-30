---
title: Bir Azure Data Factory işlem hattında özel etkinlikler kullanma
description: Azure Veri Fabrikası ardışık bir ardışık alanda özel etkinlikler oluşturmayı ve bunları nasıl kullanacağınızı öğrenin.
services: data-factory
documentationcenter: ''
ms.assetid: 8dd7ba14-15d2-4fd9-9ada-0b2c684327e9
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
author: nabhishek
ms.author: abnarain
manager: anandsub
robots: noindex
ms.openlocfilehash: 54cb06f1c77ab68818d8531b57d6eb936deda8d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265733"
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a>Bir Azure Data Factory işlem hattında özel etkinlikler kullanma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-use-custom-activities.md)
> * [Sürüm 2 (geçerli sürüm)](../transform-data-using-dotnet-custom-activity.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki Özel etkinliklere](../transform-data-using-dotnet-custom-activity.md)bakın.

Bir Azure Veri Fabrikası ardışık hattında kullanabileceğiniz iki tür etkinlik vardır.

- [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) [desteklenen kaynak ve lavabo veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats)arasında veri taşımak için.
- Azure HDInsight, Azure Toplu İş ve Azure Machine Learning gibi bilgi işlem hizmetlerini kullanarak verileri dönüştürmek için [Veri Dönüştürme Etkinlikleri.](data-factory-data-transformation-activities.md)

Verileri Veri Fabrikası'nın desteklemediği bir veri deposuna/veri deposundan taşımak için, kendi veri hareketi mantığınızla özel bir **etkinlik** oluşturun ve etkinliği bir ardışık ardışık ardışık alanda kullanın. Benzer şekilde, verileri Veri Fabrikası tarafından desteklenmeyen bir şekilde dönüştürmek/işlemek için, kendi veri dönüştürme mantığınızla özel bir etkinlik oluşturun ve etkinliği bir ardışık ardışık işlemde kullanın.

Özel bir etkinliği, Azure Toplu **Toplu Sanal** Makineler havuzunda çalışacak şekilde yapılandırabilirsiniz. Azure Toplu İş'i kullanırken yalnızca varolan bir Azure Toplu İş havuzuk kullanabilirsiniz.

Aşağıdaki izleme, özel bir .NET etkinliği oluşturmak ve bir ardışık ardışık alanda özel etkinliği kullanmak için adım adım yönergeler sağlar. Bu gözden geçirme işlemi, **Azure Toplu İş** bağlantılı bir hizmet kullanır.

> [!IMPORTANT]
> - Şirket içi veri kaynaklarına erişmek için özel bir etkinlikten veri yönetimi ağ geçidi kullanmak mümkün değildir. Şu anda, [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) yalnızca Veri Fabrikası'ndaki kopyalama etkinliğini ve depolanan yordam etkinliğini destekler.

## <a name="walkthrough-create-a-custom-activity"></a>Walkthrough: özel bir etkinlik oluşturma
### <a name="prerequisites"></a>Ön koşullar
* Visual Studio 2012/2013/2015/2017
* [Azure .NET SDK'yı](https://azure.microsoft.com/downloads/) indirip yükleyin

### <a name="azure-batch-prerequisites"></a>Azure Toplu İşlem ön koşulları
Walkthrough'da, azure toplu işlem kaynağını kullanarak özel .NET etkinliklerinizi çalıştırırsınız. **Azure Toplu İşlem,** büyük ölçekli paralel ve yüksek performanslı bilgi işlem (HPC) uygulamalarını bulutta verimli bir şekilde çalıştırmak için bir platform hizmetidir. Azure Toplu İşlem, yönetilen sanal **makineler koleksiyonunda**çalışacak bilgi işlem yoğun çalışmayı planlar ve işlerinizi gereksinimlerini karşılamak için bilgi işlem kaynaklarını otomatik olarak ölçeklendirebilir. Azure Toplu İş hizmetine ayrıntılı bir genel bakış için [Azure Toplu İş temelleri][batch-technical-overview] makalesine bakın.

Öğretici için, VM havuzuiçeren bir Azure Toplu İş hesabı oluşturun. Adımlar aşağıdaki gibidir:

1. [Azure portalını](https://portal.azure.com)kullanarak bir **Azure Toplu İş hesabı** oluşturun. Bkz. Talimatlar için bir Azure Toplu İş hesabı makalesi [oluşturun ve yönetin.][batch-create-account]
2. Azure Toplu Iş hesabı adını, hesap anahtarını, URI'yi ve havuz adını not edin. Azure Toplu İş bağlantılı bir hizmet oluşturmak için bunlara ihtiyacınız var.
    1. Azure Toplu İş hesabının ana sayfasında **URL** aşağıdaki biçimde `https://myaccount.westus.batch.azure.com`bir URL görürsünüz: . Bu örnekte, **hesabım** Azure Toplu İş hesabının adıdır. Bağlantılı hizmet tanımında kullandığınız URI, hesabın adı olmayan URL'dir. Örneğin: `https://<region>.batch.azure.com`.
    2. Sol menüdeki **Tuşlar'ı** tıklatın ve **BIRINCIL ERİşİm ANAHTARINI**kopyalayın.
    3. Varolan bir havuzu kullanmak için menüdeki **Havuzlar'ı** tıklatın ve havuzun **kimliğini** not edin. Varolan bir havuzunuz yoksa, bir sonraki adıma geçin.
2. Azure **Toplu İş havuzu**oluşturun.

   1. Azure [portalında,](https://portal.azure.com)sol menüde **Gözat'ı** tıklatın ve **Toplu Hesaplar'ı**tıklatın.
   2. **Toplu İş Hesabı'nı** açmak için Azure Toplu İş hesabınızı seçin.
   3. **Havuzlar döşemesi'ni** tıklatın.
   4. **Havuzlar** bıçağında, havuz eklemek için araç çubuğundaki Ekle düğmesini tıklatın.
      1. Havuz için bir kimlik girin (Havuz Kimliği). **Havuzun kimliğine dikkat edin;** Veri Fabrikası çözümlerini oluştururken ihtiyacınız var.
      2. İşletim Sistemi Ailesi ayarı için **Windows Server 2012 R2'yi** belirtin.
      3. Düğüm **fiyatlandırma katmanı**seçin.
      4. **Hedef Adanmış** ayar için değer olarak **2** girin.
      5. Düğüm ayarı **başına Max görevleri** için değer olarak **2** girin.
   5. Havuzu oluşturmak için **Tamam**'a tıklayın.
   6. Havuzun **kimliğini** not edin.

### <a name="high-level-steps"></a>Üst düzey adımlar
Bu gözden geçirme nin bir parçası olarak gerçekleştirdiğiniz iki üst düzey adım şunlardır:

1. Basit veri dönüştürme/işleme mantığı içeren özel bir etkinlik oluşturun.
2. Özel etkinliği kullanan bir ardışık hat olan bir Azure veri fabrikası oluşturun.

### <a name="create-a-custom-activity"></a>Özel etkinlik oluşturma
Bir .NET özel etkinliği oluşturmak için, **IDotNetActivity** arabirimini uygulayan bir sınıfa sahip bir **.NET Sınıf Kitaplığı** projesi oluşturun. Bu arabirimin tek bir yöntemi vardır: [Yürüt ve](https://msdn.microsoft.com/library/azure/mt603945.aspx) imzası:

```csharp
public IDictionary<string, string> Execute(
    IEnumerable<LinkedService> linkedServices,
    IEnumerable<Dataset> datasets,
    Activity activity,
    IActivityLogger logger)
```

Yöntem dört parametre alır:

- **linkedServices**. Bu özellik, etkinlik için giriş/çıktı veri kümeleri tarafından başvurulan Veri Deposu bağlantılı hizmetlerin sayısal bir listesidir.
- **veri kümeleri**. Bu özellik, etkinlik için giriş/çıktı veri kümelerinin sayısal bir listesidir. Giriş ve çıktı veri kümeleri tarafından tanımlanan konumları ve şemaları elde etmek için bu parametreyi kullanabilirsiniz.
- **etkinliği**. Bu özellik geçerli etkinliği temsil eder. Özel etkinlikle ilişkili genişletilmiş özelliklere erişmek için kullanılabilir. Ayrıntılar için [Genişletilmiş Özelliklere Erişim'e](#access-extended-properties) bakın.
- **logger**. Bu nesne, ardışık işlem için kullanıcı günlüğüne yüzey debug açıklamaları yazmanızı sağlar.

Yöntem, gelecekte özel etkinlikleri birbirine zincirlemek için kullanılabilecek bir sözlük döndürür. Bu özellik henüz uygulanmadı, bu nedenle yöntemden boş bir sözlük döndürün.

### <a name="procedure"></a>Yordam
1. Bir **.NET Sınıf Kitaplığı** projesi oluşturun.
   <ol type="a">
     <li>Visual Studio’yu başlatın.</li>
     <li><b>Dosya</b>’ya tıklayın, <b>Yeni</b>’nin üzerine gelin ve <b>Proje</b>’ye tıklayın.</li>
     <li><b>Şablonlar</b>’ı genişletin ve <b>Visual C#</b> seçeneğini belirleyin. Bu izbarada C#'ı kullanırsınız, ancak özel etkinliği geliştirmek için herhangi bir .NET dilini kullanabilirsiniz.</li>
     <li>Sağdaki proje türleri listesinden <b>Sınıf Kitaplığı'nı</b> seçin. Visual Studio'da <b>Sınıf Kitaplığı 'nı (.NET Framework)</b> seçin </li>
     <li><b>Ad</b>için <b>MyDotNetActivity</b> girin.</li>
     <li><b>Konum</b>için <b>C:\ADFBaşlangıç'i</b> seçin.</li>
     <li>Projeyi oluşturmak için <b>Tamam</b>'a tıklayın.</li>
   </ol>

2. **Araçlar**'a tıklayın, **NuGet Paket Yöneticisi**'nin üzerine gelin ve ardından **Paket Yöneticisi Konsolu**'na tıklayın.

3. Paket Yöneticisi Konsolunda, **Microsoft.Azure.Management.DataFactorys'i**almak için aşağıdaki komutu uygulayın.

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. Azure **Depolama** NuGet paketini projeye aktarın.

    ```powershell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    > [!IMPORTANT]
    > Veri Fabrikası hizmet başlatıcısı WindowsAzure.Storage 4.3 sürümünü gerektirir. Özel etkinlik projenizde Azure Depolama derlemesinin daha sonraki bir sürümüne bir başvuru eklerseniz, etkinlik yürütüldüğünde bir hata görürsünüz. Hatayı gidermek için [AppDomain yalıtımı](#appdomain-isolation) bölümüne bakın.
5. Projedeki kaynak dosyaya aşağıdaki ifadeleri **kullanarak** ekleyin.

    ```csharp

    // Comment these lines if using VS 2017
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    // --------------------

    // Comment these lines if using <= VS 2015
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    // ---------------------

    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. **Ad alanının** adını **MyDotNetActivityNS**olarak değiştirin.

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. Sınıfın adını **MyDotNetActivity** olarak değiştirin ve aşağıdaki kod parçacığında gösterildiği gibi **IDotNetActivity** arabiriminden türetin:

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. **IDotNetActivity** arabiriminin **Yürütme** yöntemini **MyDotNetActivity** sınıfına uygulayın (Ekle) ve aşağıdaki örnek kodu yönteme kopyalayın.

    Aşağıdaki örnek, bir veri dilimiyle ilişkili her blob'daki arama teriminin ("Microsoft") oluşum sayısını sayar.

    ```csharp
    /// <summary>
    /// Execute method is the only method of IDotNetActivity interface you must implement.
    /// In this sample, the method invokes the Calculate method to perform the core logic.
    /// </summary>

    public IDictionary<string, string> Execute(
        IEnumerable<LinkedService> linkedServices,
        IEnumerable<Dataset> datasets,
        Activity activity,
        IActivityLogger logger)
    {
        // get extended properties defined in activity JSON definition
        // (for example: SliceStart)
        DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
        string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];

        // to log information, use the logger object
        // log all extended properties
        IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
        logger.Write("Logging extended properties if any...");
        foreach (KeyValuePair<string, string> entry in extendedProperties)
        {
            logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
        }

        // linked service for input and output data stores
        // in this example, same storage is used for both input/output
        AzureStorageLinkedService inputLinkedService;

        // get the input dataset
        Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);

        // declare variables to hold type properties of input/output datasets
        AzureBlobDataset inputTypeProperties, outputTypeProperties;

        // get type properties from the dataset object
        inputTypeProperties = inputDataset.Properties.TypeProperties as AzureBlobDataset;

        // log linked services passed in linkedServices parameter
        // you will see two linked services of type: AzureStorage
        // one for input dataset and the other for output dataset
        foreach (LinkedService ls in linkedServices)
            logger.Write("linkedService.Name {0}", ls.Name);

        // get the first Azure Storage linked service from linkedServices object
        // using First method instead of Single since we are using the same
        // Azure Storage linked service for input and output.
        inputLinkedService = linkedServices.First(
            linkedService =>
            linkedService.Name ==
            inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
            as AzureStorageLinkedService;

        // get the connection string in the linked service
        string connectionString = inputLinkedService.ConnectionString;

        // get the folder path from the input dataset definition
        string folderPath = GetFolderPath(inputDataset);
        string output = string.Empty; // for use later.

        // create storage client for input. Pass the connection string.
        CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
        CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();

        // initialize the continuation token before using it in the do-while loop.
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

            // Calculate method returns the number of occurrences of
            // the search term (“Microsoft”) in each blob associated
            // with the data slice. definition of the method is shown in the next step.

            output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");

        } while (continuationToken != null);

        // get the output dataset using the name of the dataset matched to a name in the Activity output collection.
        Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

        // get type properties for the output dataset
        outputTypeProperties = outputDataset.Properties.TypeProperties as AzureBlobDataset;

        // get the folder path from the output dataset definition
        folderPath = GetFolderPath(outputDataset);

        // log the output folder path
        logger.Write("Writing blob to the folder: {0}", folderPath);

        // create a storage object for the output blob.
        CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
        // write the name of the file.
        Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));

        // log the output file name
        logger.Write("output blob URI: {0}", outputBlobUri.ToString());

        // create a blob and upload the output text.
        CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
        logger.Write("Writing {0} to the output blob", output);
        outputBlob.UploadText(output);

        // The dictionary can be used to chain custom activities together in the future.
        // This feature is not implemented yet, so just return an empty dictionary.

        return new Dictionary<string, string>();
    }
    ```
9. Aşağıdaki yardımcı yöntemleri ekleyin:

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

        // get type properties of the dataset
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }

        // return the folder path found in the type properties
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

        // get type properties of the dataset
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }

        // return the blob/file name in the type properties
        return blobDataset.FileName;
    }

    /// <summary>
    /// Iterates through each blob (file) in the folder, counts the number of instances of search term in the file,
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

    GetFolderPath yöntemi, yolu veri kümesinin işaret ettiği klasöre döndürür ve GetFileName yöntemi veri kümesinin işaret ettiği blob/dosyanın adını döndürür. Klasörünüz varsaPath tanımlar {Year}, {Month}, {Day} vb. gibi değişkenleri kullanarak tanımlar, yöntem dizeyi çalışma zamanı değerleriyle değiştirmeden olduğu gibi döndürür. SliceStart, SliceEnd vb. erişimle ilgili ayrıntılar için [Genişletilmiş Özelliklere Erişim](#access-extended-properties) bölümüne bakın.

    ```JSON
    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "adftutorial/inputfolder/",
    ```

    Hesaplama yöntemi, giriş dosyalarındaki (klasördeki lekeler) Microsoft anahtar kelimesinin örnek sayısını hesaplar. Arama terimi ("Microsoft") kodda sabit kodlanır.
10. Projeyi derle. Menüden **Oluştur'u** tıklatın ve **Çözüm Oluştur'u**tıklatın.

    > [!IMPORTANT]
    > Projeniziçin hedef çerçeve olarak .NET Framework'ün 4.5.2 sürümünü ayarlayın: projeyi sağ tıklatın ve hedef çerçeveyi ayarlamak için **Özellikler'i** tıklatın. Veri Fabrikası, 4.5.2'den sonra .NET Framework sürümlerine karşı derlenen özel etkinlikleri desteklemez.

11. **Windows Gezgini'ni**başlatın ve yapının türüne bağlı olarak **bin\debug** veya **bin\release** klasörüne gidin.
12. Proje klasöründe \bin\Debug klasöründeki\>tüm ikilileri içeren **myDotNetActivity.zip** zip zip dosyası oluşturun. \< Kaynak kodunda bir hata olduğunda soruna neden olan satır numarası gibi ek ayrıntılar elde etmek için **MyDotNetActivity.pdb** dosyasını ekleyin.

    > [!IMPORTANT]
    > Özel etkinliğin zip dosyasındaki tüm dosyalar alt klasör olmadan **en üst düzeyde** olmalıdır.

    ![İkili çıktı dosyaları](./media/data-factory-use-custom-activities/Binaries.png)
14. Zaten yoksa özel **etkinlik kapsayıcı** adlı bir blob kapsayıcı oluşturun.
15. AzureStorageLinkedService tarafından yönlendirilen **genel amaçlı** Azure blob depolamasında (sıcak/havalı Blob depolama alanı değil) özel etkinlik konteynerine blob olarak MyDotNetActivity.zip yükleyin.

> [!IMPORTANT]
> Bu .NET etkinlik projesini Visual Studio'da Veri Fabrikası projesi içeren bir çözüme ekler ve Veri Fabrikası uygulama projesinden .NET etkinlik projesine bir başvuru eklerseniz, zip'i el ile oluşturmanın son iki adımını gerçekleştirmeniz gerekmez dosya ve genel amaçlı Azure blob depolama yükleyerek. Visual Studio'yu kullanarak Veri Fabrikası varlıklarını yayımladığınızda, bu adımlar yayımlama işlemi tarafından otomatik olarak yapılır. Daha fazla bilgi için [Visual Studio bölümündeki Veri Fabrikası projesine](#data-factory-project-in-visual-studio) bakın.

## <a name="create-a-pipeline-with-custom-activity"></a>Özel etkinlik içeren bir ardışık hatlar oluşturma
Özel bir etkinlik oluşturdunuz ve zip dosyasını ikili dosyayı **genel amaçlı** bir Azure Depolama Hesabı'ndaki bir blob kapsayıcısına yükledin. Bu bölümde, özel etkinliği kullanan bir ardışık işlem ile bir Azure veri fabrikası oluşturursunuz.

Özel etkinlik için giriş veri kümesi, blob depolamasındaki adftutorial kapsayıcısının customactivityinput klasöründeki blobları (dosyaları) temsil eder. Etkinlik için çıktı veri kümesi, blob depolamasındaki adftutorial kapsayıcısının özeletkinlik çıktı klasöründeki çıktı bloblarını temsil eder.

Aşağıdaki içerikle **file.txt** dosyasını oluşturun ve **adftutorial** kapsayıcısının **customactivityinput** klasörüne yükleyin. Zaten yoksa adftutorial kapsayıcıoluşturun.

```
test custom activity Microsoft test custom activity Microsoft
```

Giriş klasörü, klasörde iki veya daha fazla dosya olsa bile Azure Veri Fabrikası'ndaki bir dilime karşılık gelir. Her dilim ardışık işlem le işlendiğinde, özel etkinlik o dilimin giriş klasöründeki tüm lekeleri yineler.

Adftutorial\customactivityoutput klasöründe bir veya daha fazla satıriçeren bir çıktı dosyası görürsünüz (giriş klasöründeki blob sayısıyla aynıdır):

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2016-11-16-00/file.txt.
```


Bu bölümde gerçekleştirdiğiniz adımlar şunlardır:

1. Bir **veri fabrikası**oluşturun.
2. Özel etkinliğin çalıştığı Azure Toplu İşlem havuzu ve giriş/çıktı lekelerini barındıran Azure Depolama için **Bağlantılı hizmetler** oluşturun.
3. Özel etkinliğin giriş ve çıktısını temsil eden giriş ve çıktı **veri kümeleri** oluşturun.
4. Özel etkinliği kullanan bir **ardışık hatlar** oluştur.

> [!NOTE]
> **file.txt'yi** oluşturun ve daha önce yapmadıysanız blob kapsayıcısına yükleyin. Yukarıdaki bölümdeki talimatlara bakın.

### <a name="step-1-create-the-data-factory"></a>Adım 1: Veri fabrikasını oluşturma
1. Azure portalına giriş yaptıktan sonra aşağıdaki adımları yapın:
   1. Sol menüde **kaynak oluştur'u** tıklatın.
   2. **Yeni** bıçakta **Veri + Analitik'i** tıklatın.
   3. **Veri analizi** dikey penceresinde **Data Factory**’ye tıklayın.

      ![Yeni Azure Veri Fabrikası menüsü](media/data-factory-use-custom-activities/new-azure-data-factory-menu.png)
2. Yeni **veri fabrikası** bıçağında, Ad için **CustomActivityFactory'yi** girin. Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Hata alırsanız: **Veri fabrikası adı "CustomActivityFactory" kullanılamıyor,** veri fabrikasının adını değiştirin (örneğin, **adınızCustomActivityFactory)** ve yeniden oluşturmayı deneyin.

    ![Yeni Azure Veri Fabrikası bıçağı](media/data-factory-use-custom-activities/new-azure-data-factory-blade.png)
3. **KAYNAK GRUBU AD'Ini**tıklatın ve varolan bir kaynak grubu seçin veya bir kaynak grubu oluşturun.
4. Veri fabrikasının oluşturulmasını istediğiniz doğru **aboneliği** ve **bölgeyi** kullandığınızı doğrulayın.
5. **Yeni data factory** dikey penceresinde **Oluştur**’a tıklayın.
6. Azure portalının **Panosunda** oluşturulan veri fabrikasını görürsünüz.
7. Veri fabrikası başarıyla oluşturulduktan sonra, veri fabrikasının içeriğini gösteren Veri Fabrikası bıçaklarını görürsünüz.

    ![Data Factory dikey penceresi](media/data-factory-use-custom-activities/data-factory-blade.png)

### <a name="step-2-create-linked-services"></a>Adım 2: Bağlantılı hizmetler oluşturma
Bağlı hizmetler veri depolarını veya işlem hizmetlerini Azure data factory’ye bağlar. Bu adımda, Azure Depolama hesabınızı ve Azure Toplu Iş hesabınızı veri fabrikanıza bağlarsınız.

#### <a name="create-azure-storage-linked-service"></a>Azure Storage bağlı hizmeti oluşturma
1. Author'ı tıklatın ve **CustomActivityFactory**için **DATA FACTORY** bıçağına fayans **dağıtın.** Data Factory Düzenleyicisi’ni görürsünüz.
2. Komut çubuğunda **Yeni veri deposu'nu** tıklatın ve **Azure depolama yı**seçin. Düzenleyicide Azure Storage bağlı hizmeti oluşturmak için JSON betiğini görmeniz gerekir.

    ![Yeni veri deposu - Azure Depolama](media/data-factory-use-custom-activities/new-data-store-menu.png)
3. Azure `<accountname>` depolama hesabınızın adıyla `<accountkey>` ve Azure depolama hesabının erişim anahtarıyla değiştirin. Depolama erişim anahtarınızı nasıl alacağınızı öğrenmek için [bkz.](../../storage/common/storage-account-keys-manage.md)

    ![Azure Depolama hizmeti beğendi](media/data-factory-use-custom-activities/azure-storage-linked-service.png)
4. Bağlı hizmeti dağıtmak için komut çubuğunda **Dağıt**’a tıklayın.

#### <a name="create-azure-batch-linked-service"></a>Azure Toplu Iş bağlantılı hizmet oluşturma
1. Veri Fabrikası Düzenleyicisi'nde... ** **Komut çubuğunda daha fazla bilgi için **Yeni bilgi işlem'i**tıklatın ve ardından menüden **Azure Toplu İş'i** seçin.

    ![Yeni işlem - Azure Toplu İşlemi](media/data-factory-use-custom-activities/new-azure-compute-batch.png)
2. JSON komut dosyasında aşağıdaki değişiklikleri yapın:

   1. **Hesap Adı** özelliği için Azure Toplu Iş hesabı adını belirtin. Azure Toplu **İş hesabı nın** `http://accountname.region.batch.azure.com` **URL'si** aşağıdaki biçimdedir: . JSON'daki **batchUri** özelliği için URL'den kaldırmanız `accountname.` ve `accountname` JSON özelliğini `accountName` kullanmanız gerekir.
   2. **accessKey** özelliği için Azure Toplu İş hesabı anahtarını belirtin.
   3. **PoolName** özelliği için ön koşulların bir parçası olarak oluşturduğunuz havuzun adını belirtin. Havuzun adı yerine havuzun kimliğini de belirtebilirsiniz.
   4. **BatchUri** özelliği için Azure Toplu URI'yi belirtin. Örnek: `https://westus.batch.azure.com`.
   5. **LinkedServiceName** özelliği için **AzureStorageLinkedService'i** belirtin.

        ```json
        {
          "name": "AzureBatchLinkedService",
          "properties": {
            "type": "AzureBatch",
            "typeProperties": {
              "accountName": "myazurebatchaccount",
              "batchUri": "https://westus.batch.azure.com",
              "accessKey": "<yourbatchaccountkey>",
              "poolName": "myazurebatchpool",
              "linkedServiceName": "AzureStorageLinkedService"
            }
          }
        }
        ```

       **poolName** özelliği için, havuzun adı yerine havuzun kimliğini de belirtebilirsiniz.

### <a name="step-3-create-datasets"></a>Adım 3: Veri kümeleri oluşturma
Bu adımda, giriş ve çıktı verilerini temsil edecek veri kümeleri oluşturursunuz.

#### <a name="create-input-dataset"></a>Girdi veri kümesi oluşturma
1. Veri Fabrikası **Editörü,** tıklayın **... **Komut çubuğunda daha fazla bilgi için **Yeni veri kümesini**tıklatın ve ardından açılan menüden **Azure Blob depolama alanını** seçin.
2. JSON'u sağ bölmedeki aşağıdaki JSON parçacığı ile değiştirin:

    ```json
    {
        "name": "InputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/customactivityinput/",
                "format": {
                    "type": "TextFormat"
                }
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

   Daha sonra bu geçiş saatinde başlangıç saati ile bir boru hattı oluşturursunuz: 2016-11-16T00:00:00Z ve bitiş saati: 2016-11-16T05:00:00Z. Saatlik veri üretmek için zamanlanır, bu nedenle beş giriş/çıkış dilimi vardır **(00 :00:00**-> **05**:00:00 arasında).

   Giriş veri kümesinin **sıklığı** ve **aralığı** **Saat** ve **1**olarak ayarlanır, bu da giriş diliminin saatlik olarak kullanılabilir olduğu anlamına gelir. Bu örnekte, intputklasöründeki aynı dosyadır (file.txt).

   Yukarıdaki JSON parçacığında SliceStart sistem değişkeni ile temsil edilen her dilimin başlangıç saatleri aşağıda veda edilmiştir.
3. **InputDataset'i**oluşturmak ve dağıtmak için araç çubuğunu **dağıt'ı** tıklatın. Düzenleyici’nin başlık çubuğunda **TABLO BAŞARIYLA OLUŞTURULDU** iletisini gördüğünüzü onaylayın.

#### <a name="create-an-output-dataset"></a>Çıktı veri kümesi oluşturma
1. Veri **Fabrikası editörü**olarak , tıklayın **... **Komut çubuğunda daha fazla bilgi için **Yeni veri kümesini**tıklatın ve ardından **Azure Blob depolama alanını**seçin.
2. Sağ bölmedeki JSON komut dosyasını aşağıdaki JSON komut dosyasıyla değiştirin:

    ```JSON
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "{slice}.txt",
                "folderPath": "adftutorial/customactivityoutput/",
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

     Çıkış konumu **adftutorial/customactivityoutput/** ve çıkış dosya adı yyyy-MM-dd-HH.txt nerede yyyy-MM-dd-HH yıl, ay, tarih ve saat dilim üretilmektedir. Ayrıntılar için [Geliştirici Başvurusu'na][adf-developer-reference] bakın.

    Her giriş dilimi için bir çıktı blob/dosya oluşturulur. Çıktı dosyasının her dilim için nasıl adlandırıldığını aşağıda göreiz. Tüm çıktı dosyaları tek bir çıktı klasöründe oluşturulur: **adftutorial\customactivityoutput**.

   | Dilim | Başlangıç saati | Çıktı dosyası |
   |:--- |:--- |:--- |
   | 1 |2016-11-16T00:00:00 |2016-11-16-00.txt |
   | 2 |2016-11-16T01:00:00 |2016-11-16-01.txt |
   | 3 |2016-11-16T02:00:00 |2016-11-16-02.txt |
   | 4 |2016-11-16T03:00:00 |2016-11-16-03.txt |
   | 5 |2016-11-16T04:00:00 |2016-11-16-04.txt |

    Giriş klasöründeki tüm dosyaların, yukarıda belirtilen başlangıç süreleriyle birlikte bir dilimin parçası olduğunu unutmayın. Bu dilim işlendiğinde, özel etkinlik her dosyayı tarar ve çıktı dosyasında arama teriminin ("Microsoft") oluşum sayısını içeren bir satır üretir. Giriş klasöründe üç dosya varsa, çıkış dosyasında her saatlik dilim için üç satır vardır: 2016-11-16-00.txt, 2016-11-16:01:01:00:00.txt vb.
3. **OutputDataset'i**dağıtmak için komut çubuğuna **Dağıt'ı** tıklatın.

### <a name="create-and-run-a-pipeline-that-uses-the-custom-activity"></a>Özel etkinliği kullanan bir ardışık hatlar oluşturma ve çalıştırma
1. Veri Fabrikası Düzenleyicisi'nde... ** Daha fazla**ve ardından komut çubuğunda **Yeni ardışık iş tonu** seçin.
2. Sağ bölmedeki JSON'u aşağıdaki JSON komut dosyasıyla değiştirin:

    ```JSON
    {
      "name": "ADFTutorialPipelineCustom",
      "properties": {
        "description": "Use custom activity",
        "activities": [
          {
            "Name": "MyDotNetActivity",
            "Type": "DotNetActivity",
            "Inputs": [
              {
                "Name": "InputDataset"
              }
            ],
            "Outputs": [
              {
                "Name": "OutputDataset"
              }
            ],
            "LinkedServiceName": "AzureBatchLinkedService",
            "typeProperties": {
              "AssemblyName": "MyDotNetActivity.dll",
              "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
              "PackageLinkedService": "AzureStorageLinkedService",
              "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
              "extendedProperties": {
                "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
              }
            },
            "Policy": {
              "Concurrency": 2,
              "ExecutionPriorityOrder": "OldestFirst",
              "Retry": 3,
              "Timeout": "00:30:00",
              "Delay": "00:00:00"
            }
          }
        ],
        "start": "2016-11-16T00:00:00Z",
        "end": "2016-11-16T05:00:00Z",
        "isPaused": false
      }
    }
    ```

    Aşağıdaki noktalara dikkat edin:

   * **Eşzamanlılık,** Azure Toplu İşlem havuzunda iki dilimin 2 VM ile paralel olarak işlenmesi için **2** olarak ayarlanır.
   * Etkinlikler bölümünde bir etkinlik vardır ve bu türdür: **DotNetActivity**.
   * **AssemblyName** DLL adına ayarlanır: **MyDotnetActivity.dll**.
   * **EntryPoint** **MyDotNetActivityNS.MyDotNetActivity**ayarlanır.
   * **PackageLinkedService,** özel etkinlik zip dosyasını içeren blob depolamasını gösteren **AzureStorageLinkedService** olarak ayarlanır. Giriş/çıkış dosyaları ve özel etkinlik zip dosyası için farklı Azure Depolama hesapları kullanıyorsanız, başka bir Azure Depolama bağlantılı hizmet oluşturursunuz. Bu makalede, aynı Azure Depolama hesabı kullandığınızvarsayar.
   * **PackageFile** **customactivitycontainer/MyDotNetActivity.zip**olarak ayarlanır. Bu formatta: containerforthezip/nameofthezip.zip.
   * Özel etkinlik, giriş olarak **InputDataset'i** ve çıktı olarak **ÇıktıDataset'i** alır.
   * Özel etkinliğin linkedServiceName özelliği, Azure Veri Fabrikası'na özel etkinliğin Azure Toplu İşlem Sanal M'lerinde çalışması gerektiğini söyleyen **AzureBatchLinkedService'e**işaret ediyor.
   * **isPaused** özelliği varsayılan olarak **false** olarak ayarlanır. Dilimler geçmişte başladığı için ardışık işlem hemen bu örnekte çalışır. Bu özelliği, ardışık alanı duraklatmak için doğru olarak ayarlayabilir ve yeniden başlatmak için false'a geri ayarlayabilirsiniz.
   * **Başlangıç** ve **bitiş** saatleri **beş** saat arayla ve dilimler saatlik olarak üretilir, böylece boru hattı tarafından beş dilim üretilir.
3. Ardışık komut u dağıtmak için komut çubuğuna **Dağıt'ı** tıklatın.

### <a name="monitor-the-pipeline"></a>İşlem hattını izleme
1. Azure portalındaki Veri Fabrikası bıçaklarında **Diyagram'ı**tıklatın.

    ![Diyagram kutucuğu](./media/data-factory-use-custom-activities/DataFactoryBlade.png)
2. Diyagram Görünümü'nde çıktıveri kümesini tıklatın.

    ![Diyagram görünümü](./media/data-factory-use-custom-activities/diagram.png)
3. Beş çıktı diliminin Hazır durumunda olduğunu görmelisiniz. Hazır durumunda değillerse, henüz üretilmediler.

   ![Çıkış dilimleri](./media/data-factory-use-custom-activities/OutputSlices.png)
4. Çıktı dosyalarının **adftutorial** kapsayıcısındaki blob depolama alanında oluşturulduğunu doğrulayın.

   ![özel etkinlikten çıktı][image-data-factory-output-from-custom-activity]
5. Çıktı dosyasını açarsanız, çıktıyı aşağıdaki çıktıya benzer görmeniz gerekir:

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2016-11-16-00/file.txt.
    ```
6. Veri fabrikanızı, boru hattılarınızı ve veri kümelerinizi izlemek için [Azure portalını][azure-preview-portal] veya Azure PowerShell cmdlets'i kullanın. **ActivityLogger'dan** portaldan indirebileceğiniz günlüklerde (özellikle kullanıcı-0.log) veya cmdlets kullanarak özel etkinlik kodunda bulunan iletileri görebilirsiniz.

   ![özel etkinlikten günlükleri indirme][image-data-factory-download-logs-from-custom-activity]

Veri kümelerini ve ardışık hatları izlemek için ayrıntılı adımlar için [Denetimli Hatları İzle ve Yönet'](data-factory-monitor-manage-pipelines.md) e bakın.

## <a name="data-factory-project-in-visual-studio"></a>Visual Studio'da Veri Fabrikası projesi
Azure portalı kullanmak yerine Visual Studio'u kullanarak Veri Fabrikası varlıkları oluşturabilir ve yayımlayabilirsiniz. Visual Studio'u kullanarak Veri Fabrikası varlıkları oluşturma ve yayımlama hakkında ayrıntılı bilgi için Visual [Studio'u kullanarak ilk ardışık alanınızı oluşturun](data-factory-build-your-first-pipeline-using-vs.md) ve Azure [Blob'dan Azure SQL makalelerine verileri kopyalayın.](data-factory-copy-activity-tutorial-using-visual-studio.md)

Visual Studio'da Veri Fabrikası projesi oluşturuyorsanız aşağıdaki ek adımları yapın:

1. Veri Fabrikası projesini özel etkinlik projesini içeren Visual Studio çözümüne ekleyin.
2. Veri Fabrikası projesinden .NET etkinlik projesine bir başvuru ekleyin. Veri Fabrikası projesine sağ tıklayın, **Ekle'ye**işaret edin ve ardından **Başvuru'ya**tıklayın.
3. Başvuru **Ekle** iletişim kutusunda **MyDotNetActivity** projesini seçin ve **Tamam'ı**tıklatın.
4. Çözümü oluşturun ve yayınlayın.

    > [!IMPORTANT]
    > Veri Fabrikası varlıklarını yayımladığınızda, sizin için otomatik olarak bir zip dosyası oluşturulur ve blob kapsayıcısına yüklenir: özel aktivite kapsayıcısı. Blob kapsayıcı yoksa, otomatik olarak çok oluşturulur.

## <a name="data-factory-and-batch-integration"></a>Veri Fabrikası ve Toplu Entegrasyon
Veri Fabrikası hizmeti, Azure Toplu İş'te adla bir iş oluşturur: **adf-poolname: iş-xxx**. Sol menüden **İşler'i** tıklatın.

![Azure Veri Fabrikası - Toplu işler](media/data-factory-use-custom-activities/data-factory-batch-jobs.png)

Bir dilimin her etkinlik çalışması için bir görev oluşturulur. İşlenmeye hazır beş dilim varsa, bu işte beş görev oluşturulur. Toplu İşlem havuzunda birden çok işlem düğümü varsa, iki veya daha fazla dilim paralel olarak çalıştırılabilir. İşlem düğümü başına en yüksek görevler 1 > olarak ayarlanmışsa, aynı işlemde birden fazla dilimin çalışmasını da sağlayabilirsiniz.

![Azure Veri Fabrikası - Toplu iş görevleri](media/data-factory-use-custom-activities/data-factory-batch-job-tasks.png)

Aşağıdaki diyagram, Azure Veri Fabrikası ve Toplu İş görevleri arasındaki ilişkiyi göstermektedir.

![Veri Fabrikası & Toplu](./media/data-factory-use-custom-activities/DataFactoryAndBatch.png)

## <a name="troubleshoot-failures"></a>Sorun giderme hataları
Sorun giderme birkaç temel tekniklerden oluşur:

1. Aşağıdaki hatayı görürseniz, genel amaçlı bir Azure blob depolama alanı kullanmak yerine Hot/Cool blob depolama alanı kullanıyor olabilirsiniz. Zip dosyasını genel amaçlı bir **Azure Depolama Hesabına yükleyin.**

    ```
    Error in Activity: Job encountered scheduling error. Code: BlobDownloadMiscError Category: ServerError Message: Miscellaneous error encountered while downloading one of the specified Azure Blob(s).
    ```
2. Aşağıdaki hatayı görürseniz, CS dosyasındaki sınıfın adının JSON ardışık ardışık ardışık ardışık alandaki **EntryPoint** özelliği için belirttiğiniz adla eşleştiğini onaylayın. Izbeden, sınıfın adı: MyDotNetActivity ve JSON'daki EntryPoint: MyDotNetActivityNS. **MyDotNetActivity**.

    ```
    MyDotNetActivity assembly does not exist or doesn't implement the type Microsoft.DataFactories.Runtime.IDotNetActivity properly
    ```

   Adlar eşleşirse, tüm ikililerin zip dosyasının **kök klasöründe** olduğunu onaylayın. Diğer bir deyişle, zip dosyasını açtığınızda, alt klasörlerde değil, kök klasöründeki tüm dosyaları görmeniz gerekir.
3. Giriş dilimi **Hazır**olarak ayarlanmazsa, giriş klasörü yapısının doğru olduğunu ve giriş klasörlerinde **file.txt'nin** olduğunu onaylayın.
3. Özel etkinliğinizin **Yürüt metodunda,** sorunları gidermenize yardımcı olan bilgileri günlüğe kaydetmek için **IActivityLogger** nesnesini kullanın. Günlüğe kaydedilen iletiler kullanıcı günlüğü dosyalarında (kullanıcı-0.log, user-1.log, user-2.log, vb.) adlı bir veya daha fazla dosyada gösterilmektedir.

   **OutputDataset** bıçakta, o dilimin **DATA SLICE** bıçağını görmek için dilime tıklayın. Bu dilim için **etkinlik çalışır** bakın. Dilim için çalışan bir etkinlik görmeniz gerekir. Komut çubuğunda Çalıştır'ı tıklattığınızda, aynı dilim için başka bir etkinlik çalıştırma başlatabilirsiniz.

   Etkinlik çalıştır'ı tıklattığınızda, günlük dosyalarının listesini içeren **EtkİnLİk KAÇ BİlGİLER** b. user_0.log dosyasında günlüğe kaydedilmiş iletileri görürsünüz. Bir hata oluştuğunda, yeniden deneme sayısı ardışık hatlar/etkinlik JSON'da 3 olarak ayarlandığından üç etkinlik çalışır. Etkinlik çalıştır'ı tıklattığınızda, hatayı gidermek için gözden geçirebileceğiniz günlük dosyalarını görürsünüz.

   Günlük dosyaları **listesinde, kullanıcı-0.log'u**tıklatın. Sağ panelde **IActivityLogger.Write** yöntemini kullanarak sonuçları vardır. Tüm iletileri görmüyorsanız, user_1.log, user_2.log vb. adlı daha fazla günlük dosyanız olup olmadığını kontrol edin. Aksi takdirde, kod son günlüğe kaydedilmiş iletiden sonra başarısız olmuş olabilir.

   Buna ek olarak, sistem hata iletileri ve özel durumlar için **sistem-0.log'u** denetleyin.
4. Hata ayrıntıları bir hata oluştuğunda **arama yığını** gibi bilgilere sahip olacak şekilde ZIP dosyasına **PDB** dosyasını ekleyin.
5. Özel etkinliğin zip dosyasındaki tüm dosyalar alt klasör olmadan **en üst düzeyde** olmalıdır.
6. **AssemblyName** (MyDotNetActivity.dll), **entryPoint**(MyDotNetActivity.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip) ve **packageLinkedService(zip** dosyasını içeren **genel amaçlı**Azure blob depolama sını işaret etmelidir) değerleri düzeltecek şekilde ayarlandığından emin olun.
7. Bir hatayı düzelttiyseniz ve dilimi yeniden işlemek istiyorsanız **OutputDataset** dikey penceresindeki dilime sağ tıklayın ve **Çalıştır**’a tıklayın.
8. Aşağıdaki hatayı görürseniz, 4.3.0 sürümü > Azure Depolama paketini kullanıyorsunuz. Veri Fabrikası hizmet başlatıcısı WindowsAzure.Storage 4.3 sürümünü gerektirir. Azure Depolama derlemesinin sonraki sürümünü kullanmanız gerekiyorsa, geçici çözüm için [Appdomain yalıtımı](#appdomain-isolation) bölümüne bakın.

    ```
    Error in Activity: Unknown error in module: System.Reflection.TargetInvocationException: Exception has been thrown by the target of an invocation. ---> System.TypeLoadException: Could not load type 'Microsoft.WindowsAzure.Storage.Blob.CloudBlob' from assembly 'Microsoft.WindowsAzure.Storage, Version=4.3.0.0, Culture=neutral,
    ```

    Azure Depolama paketinin 4.3.0 sürümünü kullanabiliyorsanız, 4.3.0 sürümü> Azure Depolama paketine yapılan mevcut başvuruyu kaldırın. Ardından NuGet Package Manager Console'dan aşağıdaki komutu çalıştırın.

    ```powershell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    Projeyi derleyin. Sürüm > 4.3.0 sürümünün Azure.Storage derlemesini bin\Debug klasöründen silin. İkili ve PDB dosyası içeren bir zip dosyası oluşturun. Eski zip dosyasını blob kapsayıcısındaki (özel etkinlik kabı) bu dosyayla değiştirin. Başarısız olan dilimleri yeniden tıklatın (sağ tıklatın dilim ve Çalıştır'ı tıklatın).
8. Özel etkinlik paketinizdeki **app.config** dosyasını kullanmaz. Bu nedenle, kodunuz yapılandırma dosyasından herhangi bir bağlantı dizeleri okursa, çalışma zamanında çalışmaz. Azure Toplu İş'i kullanırken en iyi yöntem, bir **Azure KeyVault'ta**herhangi bir sır saklamak, **anahtar kasasını**korumak için sertifika tabanlı bir hizmet ilkesi kullanmak ve sertifikayı Azure Toplu İşlem havuzuna dağıtmaktır. Böylece .NET özel etkinliği çalıştırma sırasında KeyVault’tan parolalara erişebilir. Bu çözüm genel bir çözümdür ve yalnızca bağlantı dizesini değil, her türlü gizliyi ölçeklendirebilir.

   Daha kolay bir geçici çözüm (ancak en iyi yöntem değildir): bağlantı dize ayarlarına sahip bir **Azure SQL bağlantılı hizmet** oluşturabilir, bağlantılı hizmeti kullanan bir veri kümesi oluşturabilir ve veri kümesini özel .NET etkinliğine sahte giriş veri kümesi olarak zincirleyebilirsiniz. Daha sonra, özel etkinlik kodunda bağlantılı hizmetin bağlantı dizelerine erişebilirsiniz.

## <a name="update-custom-activity"></a>Özel etkinliği güncelleştirme
Özel etkinliğin kodunu güncellerseniz, oluşturun ve blob depolamasına yeni ikililer içeren zip dosyasını yükleyin.

## <a name="appdomain-isolation"></a>Appdomain yalıtımı
Bkz. Veri Fabrikası başlatıcısı tarafından kullanılan montaj sürümleriyle sınırlandırılmamış özel bir etkinliğin nasıl oluşturulabileceğinizi gösteren [Çapraz AppEtki Örneği](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/CrossAppDomainDotNetActivitySample) (örneğin: WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x, vb.).

## <a name="access-extended-properties"></a>Genişletilmiş özelliklere erişin
Aşağıdaki örnekte gösterildiği gibi etkinlik JSON genişletilmiş özellikleri bildirebilirsiniz:

```JSON
"typeProperties": {
  "AssemblyName": "MyDotNetActivity.dll",
  "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
  "PackageLinkedService": "AzureStorageLinkedService",
  "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
  "extendedProperties": {
    "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))",
    "DataFactoryName": "CustomActivityFactory"
  }
},
```

Örnekte, iki genişletilmiş özellikleri vardır: **SliceStart** ve **DataFactoryName**. SliceStart değeri SliceStart sistem değişkenini temel alar. Desteklenen sistem değişkenlerinin listesi için [Sistem Değişkenleri'ne](data-factory-functions-variables.md) bakın. DataFactoryName değeri CustomActivityFactory için sabit kodlanır.

**Yürüt metodundaki** bu genişletilmiş özelliklere erişmek için aşağıdaki koda benzer kod kullanın:

```csharp
// to get extended properties (for example: SliceStart)
DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];

// to log all extended properties
IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
logger.Write("Logging extended properties if any...");
foreach (KeyValuePair<string, string> entry in extendedProperties)
{
    logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
}
```

## <a name="auto-scaling-of-azure-batch"></a>Azure Toplu İş'in otomatik ölçekleme
**Otomatik ölçeklendirme** özelliğine sahip bir Azure Toplu İş havuzu da oluşturabilirsiniz. Örneğin, 0 özel VM'ler içeren masmavi bir toplu iş havuzu ve bekleyen görev sayısını temel alan otomatik ölçeklendirme formülü oluşturabilirsiniz.

Buradaki örnek formül aşağıdaki davranışı elde eder: Havuz başlangıçta oluşturulduğunda, 1 VM ile başlar. $PendingTasks ölçümü, çalışan + etkin (sıralanmış) durumdaki görev sayısını tanımlar.  Formül, son 180 saniyeiçinde bekleyen görevlerin ortalama sayısını bulur ve TargetDedicated'ı buna göre ayarlar. TargetDedicated'In asla 25 VM'yi geçmemesini sağlar. Böylece, yeni görevler gönderildikçe, havuz otomatik olarak büyür ve görevler tamamlandıkça, VM'ler birer birer ücretsiz hale gelir ve otomatik ölçekleme bu VM'leri küçültür. başlangıçNumberOfVMs ve maxNumberofVMs ihtiyaçlarınıza göre ayarlanabilir.

Otomatik ölçekformülü:

```
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
```

Ayrıntılar için [bir Azure Toplu İş havuzunda otomatik olarak hesap düğümlerini ölçeklendirin.](../../batch/batch-automatic-scaling.md)

Havuz varsayılan [otomatik ÖlçeklendirmeAralığı](https://msdn.microsoft.com/library/azure/dn820173.aspx)kullanıyorsa, Toplu İşlem hizmetinin özel etkinliği çalıştırmadan önce VM'yi hazırlaması 15-30 dakika sürebilir.  Havuz farklı bir otomatik ÖlçeklendirmeInterval kullanıyorsa, Toplu Işlem hizmeti otomatik ÖlçeklendirmeAralığı + 10 dakika sürebilir.


## <a name="create-a-custom-activity-by-using-net-sdk"></a>.NET SDK'yı kullanarak özel bir etkinlik oluşturma
Bu makaledeki izbinde, Azure portalını kullanarak özel etkinliği kullanan bir ardışık işlem ile bir veri fabrikası oluşturursunuz. Aşağıdaki kod, bunun yerine .NET SDK'yı kullanarak veri fabrikasının nasıl oluşturulabileceğinizi gösterir. .NET API makalesini [kullanarak kopya etkinliği olan bir ardışık ardışık](data-factory-copy-activity-tutorial-using-dotnet-api.md) yol hattı oluşturmak için SDK'yı kullanma hakkında daha fazla ayrıntı bulabilirsiniz.

```csharp
using System;
using System.Configuration;
using System.Collections.ObjectModel;
using System.Threading;
using System.Threading.Tasks;

using Microsoft.Azure;
using Microsoft.Azure.Management.DataFactories;
using Microsoft.Azure.Management.DataFactories.Models;
using Microsoft.Azure.Management.DataFactories.Common.Models;

using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Collections.Generic;

namespace DataFactoryAPITestApp
{
    class Program
    {
        static void Main(string[] args)
        {
            // create data factory management client

            // TODO: replace ADFTutorialResourceGroup with the name of your resource group.
            string resourceGroupName = "ADFTutorialResourceGroup";

            // TODO: replace APITutorialFactory with a name that is globally unique. For example: APITutorialFactory04212017
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
                ConfigurationManager.AppSettings["SubscriptionId"],
                GetAuthorizationHeader().Result);

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties()
                    }
                }
            );

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            // TODO: Replace <accountname> and <accountkey> with name and key of your Azure Storage account.
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure Batch linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureBatchLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            // TODO: replace <batchaccountname> and <yourbatchaccountkey> with name and key of your Azure Batch account
                            new AzureBatchLinkedService("<batchaccountname>", "https://westus.batch.azure.com", "<yourbatchaccountkey>", "myazurebatchpool", "AzureStorageLinkedService")
                        )
                    }
                }
            );

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "InputDataset";
            string Dataset_Destination = "OutputDataset";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,

                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Source,
                        Properties = new DatasetProperties()
                        {
                            LinkedServiceName = "AzureStorageLinkedService",
                            TypeProperties = new AzureBlobDataset()
                            {
                                FolderPath = "adftutorial/customactivityinput/",
                                Format = new TextFormat()
                            },
                            External = true,
                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },

                            Policy = new Policy() { }
                        }
                    }
                });

            Console.WriteLine("Creating output dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            LinkedServiceName = "AzureStorageLinkedService",
                            TypeProperties = new AzureBlobDataset()
                            {
                                FileName = "{slice}.txt",
                                FolderPath = "adftutorial/customactivityoutput/",
                                PartitionedBy = new List<Partition>()
                                {
                                    new Partition()
                                    {
                                        Name = "slice",
                                        Value = new DateTimePartitionValue()
                                        {
                                            Date = "SliceStart",
                                            Format = "yyyy-MM-dd-HH"
                                        }
                                    }
                                }
                            },
                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

            Console.WriteLine("Creating a custom activity pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2017, 3, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipelineCustom";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Use custom activity",

                            // Initial value for pipeline's active period. With this, you won't need to set slice status
                            Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,
                            IsPaused = false,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "MyDotNetActivity",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    LinkedServiceName = "AzureBatchLinkedService",
                                    TypeProperties = new DotNetActivity()
                                    {
                                        AssemblyName = "MyDotNetActivity.dll",
                                        EntryPoint = "MyDotNetActivityNS.MyDotNetActivity",
                                        PackageLinkedService = "AzureStorageLinkedService",
                                        PackageFile = "customactivitycontainer/MyDotNetActivity.zip",
                                        ExtendedProperties = new Dictionary<string, string>()
                                        {
                                            { "SliceStart", "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"}
                                        }
                                    },
                                    Policy = new ActivityPolicy()
                                    {
                                        Concurrency = 2,
                                        ExecutionPriorityOrder = "OldestFirst",
                                        Retry = 3,
                                        Timeout = new TimeSpan(0,0,30,0),
                                        Delay = new TimeSpan()
                                    }
                                }
                            }
                        }
                    }
                });
        }

        public static async Task<string> GetAuthorizationHeader()
        {
            AuthenticationContext context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
            ClientCredential credential = new ClientCredential(
                ConfigurationManager.AppSettings["ApplicationId"],
                ConfigurationManager.AppSettings["Password"]);
            AuthenticationResult result = await context.AcquireTokenAsync(
                resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                clientCredential: credential);

            if (result != null)
                return result.AccessToken;

            throw new InvalidOperationException("Failed to acquire token");
        }
    }
}
```

## <a name="debug-custom-activity-in-visual-studio"></a>Visual Studio'da hata ayıklama özel etkinliği
[Azure Veri Fabrikası -](https://github.com/gbrueckl/Azure.DataFactory.LocalEnvironment) GitHub'daki yerel ortam örneği, Visual Studio'daki özel .NET etkinliklerini hata ayıklamanızı sağlayan bir araç içerir.

## <a name="sample-custom-activities-on-github"></a>GitHub'da örnek özel etkinlikler
| Örnek | Özel etkinlik ne işe yarar? |
| --- | --- |
| [HTTP Veri İndirici](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/HttpDataDownloaderSample). |Veri Fabrikası'ndaki özel C# Etkinliği'ni kullanarak http Endpoint'ten Azure Blob Depolamasına veri indirir. |
| [Twitter Duygu Analizi örnek](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/TwitterAnalysisSample-CustomC%23Activity) |Azure Machine Learning stüdyo modelini çağırır ve duyarlılık analizi, puanlama, tahmin vb. yapar. |
| [R Komut Dosyası çalıştırın.](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/RunRScriptUsingADFSample) |Üzerinde Zaten R Yüklü olan HDInsight kümenizde RScript.exe çalıştırarak R komut dosyalarını çağırır. |
| [Çapraz AppDomain .NET Etkinliği](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/CrossAppDomainDotNetActivitySample) |Veri Fabrikası başlatıcısı tarafından kullanılanlardan farklı montaj sürümleri kullanır |
| [Azure Çözümleme Hizmetlerinde bir modeli yeniden işleme](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/AzureAnalysisServicesProcessSample) |  Azure Çözümleme Hizmetleri'nde bir modeli yeniden işler. |

[batch-net-library]: ../../batch/batch-dotnet-get-started.md
[batch-create-account]: ../../batch/batch-account-create-portal.md
[batch-technical-overview]: ../../batch/batch-technical-overview.md
[batch-get-started]: ../../batch/batch-dotnet-get-started.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[data-factory-introduction]: data-factory-introduction.md
[azure-powershell-install]: https://github.com/Azure/azure-sdk-tools/releases


[developer-reference]: https://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: https://go.microsoft.com/fwlink/?LinkId=517456

[new-azure-batch-account]: https://msdn.microsoft.com/library/mt125880.aspx
[new-azure-batch-pool]: https://msdn.microsoft.com/library/mt125936.aspx
[azure-batch-blog]: https://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx

[nuget-package]: https://go.microsoft.com/fwlink/?LinkId=517478
[adf-developer-reference]: https://go.microsoft.com/fwlink/?LinkId=516908
[azure-preview-portal]: https://portal.azure.com/

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[hivewalkthrough]: data-factory-data-transformation-activities.md

[image-data-factory-output-from-custom-activity]: ./media/data-factory-use-custom-activities/OutputFilesFromCustomActivity.png

[image-data-factory-download-logs-from-custom-activity]: ./media/data-factory-use-custom-activities/DownloadLogsFromCustomActivity.png
