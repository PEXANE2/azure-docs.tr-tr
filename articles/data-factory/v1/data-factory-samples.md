---
title: Azure Veri Fabrikası - Örnekler
description: Azure Veri Fabrikası hizmetiyle birlikte o gemi örnekleri hakkında ayrıntılı bilgi sağlar.
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
ms.openlocfilehash: 63db1810cbdd2133bc0577530325351997b31f30
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70139255"
---
# <a name="azure-data-factory---samples"></a>Azure Veri Fabrikası - Örnekler
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası'ndaki PowerShell örneklerine](../samples-powershell.md) ve [Azure Kodu Örnekleri galerisindeki kod örneklerine](https://azure.microsoft.com/resources/samples/?service=data-factory)bakın.


## <a name="samples-on-github"></a>GitHub’daki örnekler
The [GitHub Azure-DataFactory repository](https://github.com/azure/azure-datafactory) contains several samples that help you quickly ramp up with Azure Data Factory service (or) modify the scripts and use it in own application. Örnekler\JSON klasörü, sık karşılaşılan senaryolar için JSON parçacıkları içerir.

| Örnek | Açıklama |
|:--- |:--- |
| [ADF Geçiş](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/ADFWalkthrough) |Bu örnek, günlük dosyalarından öngörülere veri açmak için Azure Veri Fabrikası'nı kullanarak günlük dosyalarını işlemek için uçtan uca bir gözden geçirme sağlar. <br/><br/>Bu gözden geçirmede, Veri Fabrikası ardışık hattı örnek günlükleri toplar, süreçleri ve referans verileri ile günlükleri verileri zenginleştirir ve son zamanlarda başlatılan bir pazarlama kampanyasının etkinliğini değerlendirmek için verileri dönüştürür. |
| [JSON örnekleri](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/JSON) |Bu örnek, sık karşılaşılan senaryolar için JSON örnekleri sağlar. |
| [Http Veri İndirici Örneği](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/HttpDataDownloaderSample) |Bu örnek, özel .NET etkinliğini kullanarak bir HTTP bitiş noktasından Azure Blob Depolamasına veri indirmeyi sergiler. |
| [Çapraz AppDomain Nokta Net Etkinlik Örneği](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/CrossAppDomainDotNetActivitySample) |Bu örnek, ADF başlatıcısı tarafından kullanılan montaj sürümleriyle sınırlandırılmamış özel bir .NET etkinliği yazmanızı sağlar (Örneğin, WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x, vb.). |
| [R komut dosyalarını çalıştır](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/RunRScriptUsingADFSample) |Bu örnek, RScript.exe çağırmak için kullanılabilecek Veri Fabrikası özel etkinliği içerir. Bu örnek yalnızca üzerinde R Yüklü olan kendi (isteğe bağlı olmayan) HDInsight kümenizle çalışır. |
| [HDInsight Hadoop kümesinde Kıvılcım işlerini çağırma](https://docs.microsoft.com/azure/data-factory/tutorial-transform-data-spark-portal) |Bu örnek, bir Kıvılcım programını çağırmak için MapReduce etkinliğinin nasıl kullanılacağını gösterir. Kıvılcım programı yalnızca bir Azure Blob kapsayıcısından diğerine veri kopyalar. |
| [Azure Machine Learning Toplu Puanlama Etkinliği ile Twitter Analizi](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/TwitterAnalysisSample-AzureMLBatchScoringActivity) |Bu örnek, twitter duygu analizi, puanlama, tahmin vb. gerçekleştiren bir Azure Machine Learning modelini çağırmak için AzureMLBatchScoringActivity'in nasıl kullanılacağını gösterir. |
| [Özel etkinlik kullanarak Twitter Analizi](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/TwitterAnalysisSample-CustomC%23Activity) |Bu örnek, twitter duygu analizi, puanlama, tahmin vb. gerçekleştiren bir Azure Machine Learning modelini çağırmak için özel bir .NET etkinliğinin nasıl kullanılacağını gösterir. |
| [Azure Machine Learning için Parametreli Boru Hatları](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/ParameterizedPipelinesForAzureML) |Örnek, bölgelerin listesinin bu örnekle birlikte verilen bir parameters.txt dosyasından geldiği farklı bir bölge parametresi ile her birini puanlama ve yeniden eğitmek için N ardışık kodları dağıtmak için uçtan uca bir C# kodu sağlar. |
| [Azure Akış Analizi işleri için Referans Veri Yenileme](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/ReferenceDataRefreshForASAJobs) |Bu örnek, sorguları referans verileriyle çalıştırmak ve başvuru verileri için yenilemeyi bir zamanlamada kurmak için Azure Veri Fabrikası ve Azure Akış Analizi'nin nasıl kullanılacağını gösterir. |
| [On-premises Hortonworks Hadoop ile Hibrid Boru Hattı](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/HybridPipelineWithOnPremisesHortonworksHadoop) |Örnek, tıpkı bulutta HDInsight tabanlı Hadoop kümesi gibi diğer bilgi işlem hedeflerini eklergibi Veri Fabrikası'ndaki işleri çalıştırmak için şirket içi Hadoop kümesini bilgi işlem hedefi olarak kullanır. |
| [JSON Dönüşüm Aracı](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/JSONConversionTool) |Bu araç, JSON'ları 2015-07-01 önizlemesinden önceki sürümden en son veya 2015-07-01-preview (varsayılan) olarak dönüştürmenize olanak tanır. |
| [U-SQL örnek giriş dosyası](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/U-SQL%20Sample%20Input%20File) |Bu dosya, u-SQL etkinliği tarafından kullanılan örnek bir dosyadır. |
| [Blob dosyasi silme](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/DeleteBlobFileFolderCustomActivity) | Bu örnek, dosyalar kopyalandıktan sonra dosyaları kaynak Azure Blob konumundan silmek için ADF özel .net etkinliğinin bir parçası olarak kullanılabilecek bir C# dosyasını sergiler.|

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları
GitHub'da Veri Fabrikası için aşağıdaki Azure Kaynak Yöneticisi şablonlarını bulabilirsiniz.

| Şablon | Açıklama |
| --- | --- |
| [Azure Blob Depolama'dan Azure SQL Veritabanına Kopyala](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy) |Bu şablonu dağıtmak, belirtilen Azure blob depolamasından Azure SQL veritabanına verileri kopyalayan bir ardışık işlem aygıtına sahip bir Azure veri fabrikası oluşturur |
| [Salesforce'tan Azure Blob Depolamasına kopyala](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy) |Bu şablonu dağıtmak, belirtilen Salesforce hesabından Azure blob depolamasına verileri kopyalayan bir ardışık işlem aygıtına sahip bir Azure veri fabrikası oluşturur. |
| [Azure HDInsight kümesinde Hive komut dosyalarını çalıştırarak verileri dönüştürme](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation) |Bu şablonu dağıtmak, bir Azure HDInsight Hadoop kümesinde örnek Hive komut dosyasını çalıştırarak verileri dönüştüren bir veri aktarıcısı olan bir Azure veri fabrikası oluşturur. |

## <a name="samples-in-azure-portal"></a>Azure portalındaki örnekler
Örnek ardışık hatlar ve bunların ilişkili varlıklarını (veri kümeleri ve bağlantılı hizmetler) veri fabrikanıza dağıtmak için veri fabrikanızın ana sayfasındaki **Örnek ardışık lıklar** döşemesini kullanabilirsiniz.

1. Bir veri fabrikası oluşturun veya varolan bir veri fabrikasını açın. Bkz. Bir veri fabrikası oluşturmak için adımlar için [Veri Fabrikası'nı kullanarak Blob Depolama'dan SQL Veritabanına verileri kopyalayın.](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
2. Veri fabrikası için **DATA FACTORY** bıçak, Örnek **boru hatları** döşemesi tıklatın.

    ![Örnek boru hatları döşemesi](./media/data-factory-samples/SamplePipelinesTile.png)
3. Örnek **ardışık hatlar** bıçak, dağıtmak istediğiniz **örnek** tıklatın.

    ![Örnek boru hatları bıçak](./media/data-factory-samples/SampleTile.png)
4. Örnek için yapılandırma ayarlarını belirtin. Örneğin, Azure depolama hesap adınız ve hesap anahtarınız, Azure SQL sunucu adınız, veritabanınız, Kullanıcı Kimliğiniz ve parolanız vb.

    ![Örnek bıçak](./media/data-factory-samples/SampleBlade.png)
5. Yapılandırma ayarlarını belirtmeyi bitirdikten sonra, örnek ardışık hatlar ve ardışık hatlar tarafından kullanılan bağlantılı hizmetleri/tabloları oluşturmak/dağıtmak için **Oluştur'u** tıklatın.
6. **Örnek ardışık hatlar** bıçak üzerinde daha önce tıkladığınız örnek döşemede dağıtım durumunu görürsünüz.

    ![Dağıtım durumu](./media/data-factory-samples/DeploymentStatus.png)
7. **Dağıtım'ın** örnek için döşemede başarılı iletisini gördüğünüzde, **Örnek ardışık hatlar bıçaklarını** kapatın.  
8. **DATA FACTORY** bıçak üzerinde, bağlantılı hizmetlerin, veri kümelerinin ve boru hatlarının veri fabrikanıza eklenmediğini görürsünüz.  

    ![Data Factory dikey penceresi](./media/data-factory-samples/DataFactoryBladeAfter.png)

## <a name="samples-in-visual-studio"></a>Visual Studio Örnekleri
### <a name="prerequisites"></a>Ön koşullar
Bilgisayarınızda şunların yüklü olması gerekir:

* Visual Studio 2013 veya Visual Studio 2015
* Visual Studio 2013 veya Visual Studio 2015 için Azure SDK’sını indirin. [Azure İndirme Sayfası](https://azure.microsoft.com/downloads/)’na gidin ve **.NET** bölümündeki **VS 2013** veya **VS 2015**’e tıklayın.
* Visual Studio için en son Azure Data Factory eklentisini indirin: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) veya [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Visual Studio 2013 kullanıyorsanız, eklentiyi aşağıdaki adımları yaparak da güncelleyebilirsiniz: Menüde, **Araçlar** -> **Uzantıları ve Güncelleştirmeler** -> **Çevrimiçi** -> **Görsel Stüdyo Galerisi** -> **Microsoft Azure Veri Fabrikası Görsel Studio** -> **Güncellemesi**için araçları tıklatın.

### <a name="use-data-factory-templates"></a>Veri Fabrikası Şablonlarını Kullanma
1. Menüde **Dosya'yı** tıklatın, **Yeni'yi**işaret edin ve **Project'i**tıklatın.
2. **Yeni Proje** iletişim kutusunda aşağıdaki adımları uygulayın:

   1. **Şablonlar**altında **DataFactory'yi** seçin.
   2. Sağ bölmede **Veri Fabrikası Şablonları'nı** seçin.
   3. Proje için bir **ad** girin.
   4. Proje için bir **konum** seçin.
   5. **Tamam**'a tıklayın.

      ![Yeni proje iletişim kutusu](./media/data-factory-samples/vs-new-project-adf-templates.png)
3. Veri **Fabrikası Şablonları** iletişim kutusunda, Kullanım Örneği **Şablonları** bölümünden örnek şablonu seçin ve **İleri'yi**tıklatın. Aşağıdaki **adımlar, Müşteri Profil Oluşturma** şablonunu kullanarak size yol gösterir. Adımlar diğer örnekler için benzer.

    ![Veri Fabrikası Şablonları iletişim kutusu](./media/data-factory-samples/vs-data-factory-templates-dialog.png)
4. Veri **Fabrikası Yapılandırma** iletişim kutusunda, **Veri Fabrikası Temelleri** sayfasında **İleri'yi** tıklatın.
5. Veri **fabrikasını yapılandır** sayfasında aşağıdaki adımları yapın:
   1. **Yeni Veri Fabrikası Oluştur'u**seçin. **Varolan veri fabrikasını kullan'ı**da seçebilirsiniz.
   2. Veri fabrikası için bir **ad** girin.
   3. Veri fabrikasının oluşturulmasını istediğiniz **Azure aboneliğini** seçin.
   4. Veri fabrikası için **kaynak grubunu** seçin.
   5. **Bölge**için **Batı ABD,** **Doğu ABD**veya **Kuzey Avrupa'yı** seçin.
   6. **İleri**'ye tıklayın.
6. Veri **depolarını Yapılandır** sayfasında, varolan bir **Azure SQL veritabanı** nı ve Azure depolama **hesabını** (veya veritabanı/depolama alanı oluşturun) belirtin ve İleri'yi tıklatın.
7. **Yapıliş bilgi işlem** sayfasında varsayılanları seçin ve **İleri'yi**tıklatın.
8. **Özet** sayfasında, tüm ayarları gözden geçirin ve **İleri'yi**tıklatın.
9. Dağıtım **Durumu** sayfasında, dağıtım bitene kadar bekleyin ve **Bitir'i**tıklatın.
10. Çözüm Gezgini’nde projeye sağ tıklayın ve ardından **Yayımla**’ya tıklayın.
11. **Microsoft hesabınızda oturum açın** iletişim kutusunu görmezseniz, Azure aboneliğindeki kimlik bilgilerini hesap için girin ve **oturum aç**’a tıklayın.
12. Aşağıdaki iletişim kutusunu göreceksiniz:

    ![Yayımla iletişim kutusu](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)
13. **Data Factory’yi yapılandırma** sayfasında aşağıdaki adımları uygulayın:

    1. **Varolan veri fabrikası** seçeneğini kullanın' ı onaylayın.
    2. Şablonu kullanırken seçtiğiniz **veri fabrikasını** seçin.
    3. **Öğeleri Yayımla** sayfasına geçmek için **İleri**’ye tıklayın. (Ad alanından çıkmak için, **İleri** düğmesi devre dışıysa **SEKME** tuşuna basın.)
14. **Öğeleri Yayımla** sayfasında tüm Data Factory varlıklarının işaretli olmasını sağlayın ve **Özet** sayfasına geçmek için **İleri**’ye tıklayın.     
15. Özeti gözden geçirin, dağıtım işlemini başlatmak ve **Dağıtım Durumu**’nu görüntülemek için **İleri**’ye tıklayın.
16. **Dağıtım Durumu** sayfasında dağıtım sürecinin durumunu görmelisiniz. Dağıtımını gerçekleştirdikten sonra Son'a tıklayın.

Data Factory varlıklarına yazar olmak ve bunları Azure'da yayınlamak için Visual Studio'yı kullanma hakkında ayrıntılı bilgi için [ilk veri fabrikanızı (Visual Studio) oluşturun'a](data-factory-build-your-first-pipeline-using-vs.md) bakın.          
