---
title: Azure Veri Fabrikası - Sık Sorulan Sorular
description: Azure Veri Fabrikası hakkında sık sorulan sorular.
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
ms.openlocfilehash: 1b042c0279e458cf83bd5c81147f4b1f25d7687d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645149"
---
# <a name="azure-data-factory---frequently-asked-questions"></a>Azure Veri Fabrikası - Sık Sorulan Sorular
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, sık sorulan soruya bakın [- Veri Fabrikası.](../frequently-asked-questions.md)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="general-questions"></a>Genel sorular
### <a name="what-is-azure-data-factory"></a>Azure Data Factory nedir?
Veri Fabrikası, verilerin hareketini ve **dönüşümunu otomatikleştiren**bulut tabanlı bir veri tümleştirme hizmetidir. Tıpkı hammadde almak ve bunları bitmiş mallara dönüştürmek için ekipman çalıştıran bir fabrika gibi, Veri Fabrikası da ham veri toplayan ve kullanıma hazır bilgilere dönüştüren mevcut hizmetleri yönetir.

Veri Fabrikası, verileri hem şirket içi hem de bulut veri depoları arasında taşımak için veri odaklı iş akışları oluşturmanıza ve Azure HDInsight ve Azure Veri Gölü Analizi gibi bilgi işlem hizmetlerini kullanarak verileri işleme/dönüştürmeye olanak tanır. İhtiyacınız olan eylemi gerçekleştiren bir ardışık düzende işlem yaptıktan sonra, düzenli aralıklarla çalışacak şekilde zamanlayabilirsiniz (saatlik, günlük, haftalık vb.).   

Daha fazla bilgi için [Temel Kavramlar&genel bakış](data-factory-introduction.md)bilgilerine bakın.

### <a name="where-can-i-find-pricing-details-for-azure-data-factory"></a>Azure Veri Fabrikası için fiyatlandırma ayrıntılarını nerede bulabilirim?
Azure Veri Fabrikası'nın fiyatlandırma ayrıntıları için [Veri Fabrikası Fiyatlandırma Ayrıntıları sayfasına][adf-pricing-details] bakın.  

### <a name="how-do-i-get-started-with-azure-data-factory"></a>Azure Veri Fabrikası'na nasıl başlarım?
* Azure Veri Fabrikası'na genel bakış için [bkz.](data-factory-introduction.md)
* Kopyalama Etkinliği'ni kullanarak verilerin nasıl **kopyalanır/taşınır'** ı hakkında bir öğretici [için](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)bkz.
* HDInsight Hive Activity kullanarak **verileri nasıl dönüştüreceklerine** ilişkin bir öğretici için. [Hadoop kümesinde Hive komut dosyalarını çalıştırarak İşlem verilerini](data-factory-build-your-first-pipeline.md) görün

### <a name="what-is-the-data-factorys-region-availability"></a>Veri Fabrikası'nın bölge durumu nedir?
Veri Fabrikası **ABD Batı** ve **Kuzey Avrupa'da**mevcuttur. Veri fabrikaları tarafından kullanılan bilgi işlem ve depolama hizmetleri başka bölgelerde olabilir. Bkz. [Desteklenen bölgeler.](data-factory-introduction.md#supported-regions)

### <a name="what-are-the-limits-on-number-of-data-factoriespipelinesactivitiesdatasets"></a>Veri fabrikaları/boru hatları/etkinlikleri/veri kümeleri sayısının sınırları nelerdir?
Azure Abonelik ve Hizmet [Sınırları, Kotalar ve Kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md#data-factory-limits) makalesinin Azure Veri Fabrikası **Sınırları** bölümüne bakın.

### <a name="what-is-the-authoringdeveloper-experience-with-azure-data-factory-service"></a>Azure Veri Fabrikası hizmetinde yazma/geliştirici deneyimi nedir?
Aşağıdaki araçlardan/SDK'lardan birini kullanarak veri fabrikaları yazabilir/oluşturabilirsiniz:

* **Görsel Stüdyo** Azure veri fabrikası oluşturmak için Visual Studio'yı kullanabilirsiniz. Ayrıntılar için [Visual Studio'u kullanarak ilk veri ardışık alanınızı oluşturun'a](data-factory-build-your-first-pipeline-using-vs.md) bakın.
* **Azure PowerShell** PowerShell'i kullanarak bir veri fabrikası oluşturmak için bir öğretici/walkthrough için [Azure PowerShell'i kullanarak Azure Veri Fabrikası](data-factory-build-your-first-pipeline-using-powershell.md) Oluştur ve izleyin. Veri Fabrikası cmdlets kapsamlı bir dokümantasyon için MSDN Kütüphanesi'nde [Veri Fabrikası Cmdlet Referans][adf-powershell-reference] içeriğine bakın.
* **.NET Sınıf Kütüphanesi** Veri Fabrikası .NET SDK'yı kullanarak programlı bir şekilde veri fabrikaları oluşturabilirsiniz. Bkz. .NET SDK'yı kullanarak bir veri fabrikası oluşturmak için [.NET SDK kullanarak veri fabrikaları oluşturun, izleyin ve yönetin.](data-factory-create-data-factories-programmatically.md) Veri Fabrikası .NET SDK kapsamlı bir belge için [Veri Fabrikası Sınıf Kitaplığı Başvurusu'na][msdn-class-library-reference] bakın.
* **REST API** Veri fabrikaları oluşturmak ve dağıtmak için Azure Veri Fabrikası hizmeti tarafından açığa çıkarılan REST API'sini de kullanabilirsiniz. Veri Fabrikası REST API kapsamlı bir belgeleme için [Veri Fabrikası REST API Başvuru][msdn-rest-api-reference] bakın.
* **Azure Kaynak Yöneticisi Şablonu** Bkz. [Öğretici: Azure Kaynak Yöneticisi şablonu fo ayrıntılarını kullanarak ilk Azure veri fabrikanızı oluşturun.](data-factory-build-your-first-pipeline-using-arm.md)

### <a name="can-i-rename-a-data-factory"></a>Bir veri fabrikasını yeniden adlandırabilir miyim?
Hayır. Diğer Azure kaynakları gibi, Bir Azure veri fabrikasının adı değiştirilemez.

### <a name="can-i-move-a-data-factory-from-one-azure-subscription-to-another"></a>Bir veri fabrikasını bir Azure aboneliğinden diğerine taşıyabilir miyim?
Evet. Aşağıdaki diyagramda gösterildiği gibi veri fabrikası bıçak üzerinde **Taşı** düğmesini kullanın:

![Veri fabrikasını taşıma](media/data-factory-faq/move-data-factory.png)

### <a name="what-are-the-compute-environments-supported-by-data-factory"></a>Veri Fabrikası tarafından desteklenen bilgi işlem ortamları nelerdir?
Aşağıdaki tablo, Veri Fabrikası tarafından desteklenen bilgi işlem ortamlarının ve bunların üzerinde çalıştırılabilecek etkinliklerin bir listesini sağlar.

| İşlem ortamı | etkinlikler |
| --- | --- |
| [İsteğe bağlı HDInsight kümesi](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) veya [kendi HDInsight kümeniz](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) |[DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Domuz](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [Hadoop Streaming](data-factory-hadoop-streaming-activity.md) |
| [Azure Batch](data-factory-compute-linked-services.md#azure-batch-linked-service) |[Dotnet](data-factory-use-custom-activities.md) |
| [Azure Makine Öğrenimi](data-factory-compute-linked-services.md#azure-machine-learning-linked-service) |[Machine Learning etkinlikleri: Toplu Yürütme ve Kaynak Güncelleştirme](data-factory-azure-ml-batch-execution-activity.md) |
| [Azure Data Lake Analytics](data-factory-compute-linked-services.md#azure-data-lake-analytics-linked-service) |[Data Lake Analytics U-SQL](data-factory-usql-activity.md) |
| [Azure SQL](data-factory-compute-linked-services.md#azure-sql-linked-service), [Azure SQL Veri Ambarı](data-factory-compute-linked-services.md#azure-sql-data-warehouse-linked-service), [SQL Server](data-factory-compute-linked-services.md#sql-server-linked-service) |[Depolanan Yordam](data-factory-stored-proc-activity.md) |

### <a name="how-does-azure-data-factory-compare-with-sql-server-integration-services-ssis"></a>Azure Veri Fabrikası, SQL Server Tümleştirme Hizmetleri (SSIS) ile karşılaştırıldığında nasıl? 
MVP'lerimizden (En Değerli Profesyoneller) Birinden [Azure Veri Fabrikası ve SSIS](https://www.sqlbits.com/Sessions/Event15/Azure_Data_Factory_vs_SSIS) sunumuna bakın: Rıza Rad. Veri Fabrikası'ndaki son değişikliklerden bazıları slayt güvertesinde listelenmemiş olabilir. Azure Veri Fabrikası'na sürekli olarak daha fazla özellik ekliyoruz. Azure Veri Fabrikası'na sürekli olarak daha fazla özellik ekliyoruz. Bu güncelleştirmeleri, bu yılın ilerleyen zamanlarında Microsoft'un veri tümleştirme teknolojilerinin karşılaştırılmasına dahil edeceğiz.   

## <a name="activities---faq"></a>Etkinlikler - SSS
### <a name="what-are-the-different-types-of-activities-you-can-use-in-a-data-factory-pipeline"></a>Veri Fabrikası ardışık hattında kullanabileceğiniz farklı etkinlik türleri nelerdir?
* Verileri taşımak için [Veri Hareketi Etkinlikleri.](data-factory-data-movement-activities.md)
* Verileri işlemek/dönüştürmek için [Veri Dönüştürme Faaliyetleri.](data-factory-data-transformation-activities.md)

### <a name="when-does-an-activity-run"></a>Bir etkinlik ne zaman çalışır?
Çıktı veri tablosundaki **kullanılabilirlik** yapılandırma ayarı, etkinliğin ne zaman çalıştırılırken belirler. Giriş veri kümeleri belirtilirse, etkinlik tüm giriş veri bağımlılıklarının (diğer bir deyişle **Hazır** durum) çalışmaya başlamadan önce karşılanıp karşılanmayacağını denetler.

## <a name="copy-activity---faq"></a>Kopyalama Etkinliği - SSS
### <a name="is-it-better-to-have-a-pipeline-with-multiple-activities-or-a-separate-pipeline-for-each-activity"></a>Birden çok faaliyeti olan bir boru hattı veya her etkinlik için ayrı bir boru hattı olması daha mı iyi?
Boru hatlarının ilgili faaliyetleri bir araya alması gerekir. Bunları bağlayan veri kümeleri, ardışık alan dışında başka bir etkinlik tarafından tüketilmezse, etkinlikleri tek bir ardışık alanda tutabilirsiniz. Bu şekilde, boru hattı etkin dönemleri zincirlemeniz gerekmez, böylece bunlar birbiriyle hizalanır. Ayrıca, ardışık etki hattı nın iç tablolarındaki veri bütünlüğü, ardışık etki hattı güncellenirken daha iyi korunur. Pipeline update, ardışık hatlar içindeki tüm etkinlikleri durdurur, kaldırır ve yeniden oluşturur. Yazma açısından bakıldığında, ilgili etkinlikler içindeki veri akışını, ardışık hatlar için bir JSON dosyasında görmek de daha kolay olabilir.

### <a name="what-are-the-supported-data-stores"></a>Desteklenen veri depoları nelerdir?
Data Factory’deki Kopyalama Etkinliği bir kaynak veri deposundan havuz veri deposuna verileri kopyalar. Data Factory aşağıdaki veri depolarını destekler. Herhangi bir kaynaktan gelen veriler herhangi bir havuza yazılabilir. Bir depoya veya depodan veri kopyalama hakkında bilgi edinmek için veri deposuna tıklayın.

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> * taşıyan veri depoları şirket içi veya Azure IaaS üzerinde olabilir ve bir şirket içi/Azure IaaS makinesine [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) yüklemenizi gerektirir.

### <a name="what-are-the-supported-file-formats"></a>Desteklenen dosya biçimleri nelerdir?
[!INCLUDE [data-factory-file-format](../../../includes/data-factory-file-format.md)]

### <a name="where-is-the-copy-operation-performed"></a>Kopyalama işlemi nerede gerçekleştirilir?
Ayrıntılar için [Genel olarak kullanılabilir veri hareketi](data-factory-data-movement-activities.md#global) bölümüne bakın. Kısacası, şirket içi bir veri deposu söz konusu olduğunda, kopyalama işlemi şirket içi ortamınızda Veri Yönetimi Ağ Geçidi tarafından gerçekleştirilir. Ayrıca, veri hareketi iki bulut deposu arasında olduğunda, kopyalama işlemi aynı coğrafyada lavabo konumuna en yakın bölgede gerçekleştirilir.

## <a name="hdinsight-activity---faq"></a>HDInsight Etkinliği - SSS
### <a name="what-regions-are-supported-by-hdinsight"></a>HDInsight tarafından hangi bölgeler desteklenir?
Aşağıdaki makalede Coğrafi Kullanılabilirlik bölümüne bakın: veya [HDInsight Fiyatlandırma Ayrıntıları.][hdinsight-supported-regions]

### <a name="what-region-is-used-by-an-on-demand-hdinsight-cluster"></a>İsteğe bağlı HDInsight kümesi tarafından hangi bölge kullanılır?
İsteğe bağlı HDInsight kümesi, kümeyle birlikte kullanılacağını belirttiğiniz depolama alanının bulunduğu bölgede oluşturulur.    

### <a name="how-to-associate-additional-storage-accounts-to-your-hdinsight-cluster"></a>Ek depolama hesaplarını HDInsight kümenizle nasıl ilişkilendirin?
Kendi HDInsight Cluster'ınızı (BYOC - Kendi Kümenizi Getir) kullanıyorsanız, aşağıdaki konulara bakın:

* [Alternatif Depolama Hesapları ve Metastore'lar içeren BIR HDInsight Kümesi kullanma][hdinsight-alternate-storage]
* [HDInsight Hive ile Ek Depolama Hesapları Kullanma][hdinsight-alternate-storage-2]

Veri Fabrikası hizmeti tarafından oluşturulan isteğe bağlı bir küme kullanıyorsanız, Veri Fabrikası hizmetinin bunları sizin adınıza kaydedilebilmeleri için HDInsight bağlantılı hizmet için ek depolama hesapları belirtin. İsteğe bağlı hizmetin JSON tanımında, aşağıdaki JSON parçacığında gösterildiği gibi alternatif depolama hesapları belirtmek için **ek LinkedServiceNames** özelliğini kullanın:

```JSON
{
    "name": "MyHDInsightOnDemandLinkedService",
    "properties":
    {
        "type": "HDInsightOnDemandLinkedService",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "LinkedService-SampleData",
            "additionalLinkedServiceNames": [ "otherLinkedServiceName1", "otherLinkedServiceName2" ]
        }
    }
}
```
Yukarıdaki örnekte, diğer LinkedServiceName1 ve diğer LinkedServiceName2, tanımları HDInsight kümesinin alternatif depolama hesaplarına erişmesi için gereken kimlik bilgilerini içeren bağlantılı hizmetleri temsil eder.

## <a name="slices---faq"></a>Dilimler - SSS
### <a name="why-are-my-input-slices-not-in-ready-state"></a>Giriş dilimlerim neden Hazır durumunda değil?
Giriş verileri veri fabrikasının dışında olduğunda (veri fabrikası tarafından üretilmedi) giriş veri kümesinde dış **özelliği** **doğru** şekilde ayarlamamak yaygın bir hatadır.

Aşağıdaki örnekte, yalnızca **dataset1'de** **hariciyi** true'ya ayarlamanız gerekir.  

**DataFactory1** Pipeline 1: dataset1 -> etkinlik1 -> dataset2 -> etkinlik2 -> dataset3 Pipeline 2: dataset3-> etkinlik3 -> dataset4

Dataset4 (veri fabrikası 1'de boru hattı 2 tarafından üretilen) alan bir veri boru hattına sahip başka bir veri fabrikanız varsa, veri kümesi farklı bir veri fabrikası (DataFactory2 değil DataFactory2) tarafından üretildiği için dataset4'ü harici bir veri kümesi olarak işaretleyin.  

**DataFactory2**    
Pipeline 1: dataset4->etkinliği4->dataset5

Dış özellik düzgün ayarlanmışsa, giriş veri kümesi tanımında belirtilen konumda giriş verilerinin bulunup bulunmadığından doğrulayın.

### <a name="how-to-run-a-slice-at-another-time-than-midnight-when-the-slice-is-being-produced-daily"></a>Dilim günlük olarak üretilirken gece yarısından başka bir zamanda dilim nasıl çalıştırılır?
Dilimin üretilmesini istediğiniz zamanı belirtmek için **ofset** özelliğini kullanın. Bu özellik hakkında ayrıntılar için [Dataset kullanılabilirlik](data-factory-create-datasets.md#dataset-availability) bölümüne bakın. İşte hızlı bir örnek:

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
Günlük dilimler varsayılan gece yarısı yerine **06:00'da** başlar.     

### <a name="how-can-i-rerun-a-slice"></a>Bir dilimi nasıl yeniden çalıştırabilirim?
Bir dilimi aşağıdaki yollardan birinde yeniden çalıştırabilirsiniz:

* Etkinlik penceresini veya dilimini yeniden çalıştırmak için Monitör ve Yönet Uygulamasını kullanın. Bkz. Yönergeler için [seçili etkinlik pencerelerini Yeniden Çalıştır.](data-factory-monitor-manage-app.md#perform-batch-actions)   
* Azure portalındaki dilim için **DATA SLICE** bıçak üzerindeki komut çubuğunda **Çalıştır'ı** tıklatın.
* **Set-AzDataFactorySliceStatus** cmdlet'i, dilimin **Bekleme** durumuna göre çalıştırın.   

    ```powershell
    Set-AzDataFactorySliceStatus -Status Waiting -ResourceGroupName $ResourceGroup -DataFactoryName $df -TableName $table -StartDateTime "02/26/2015 19:00:00" -EndDateTime "02/26/2015 20:00:00"
    ```
  Cmdlet hakkında ayrıntılı bilgi için [Set-AzDataFactorySliceStatus'e][set-azure-datafactory-slice-status] bakın.

### <a name="how-long-did-it-take-to-process-a-slice"></a>Bir dilimin işlenmesi ne kadar sürdü?
Bir veri dilimini işlemenin ne kadar sürdüğünü bilmek için İzleme & Yönet Uygulamasında Etkinlik Penceresi Gezgini'ni kullanın. Ayrıntılar için [Etkinlik Penceresi Gezgini'ne](data-factory-monitor-manage-app.md#activity-window-explorer) bakın.

Azure portalında aşağıdakileri de yapabilirsiniz:  

1. Veri fabrikanız için **DATA FACTORY** bıçak üzerinde **Veri setleri** döşemesini tıklatın.
2. **Datasets** bıçağındaki belirli veri kümesini tıklatın.
3. **TABLE** bıçağındaki **Son dilimler** listesinden ilgilendiğiniz dilimi seçin.
4. **DATA SLICE** bıçak üzerindeki **Etkinlik Çalıştır'lar** listesinden etkinlik çalıştır'ı tıklatın.
5. **EtkİnLİk KAÇ IŞBİlGİLERi** bıçağındaki **Özellikler** döşemesini tıklatın.
6. BIR değer ile **DURATION** alanını görmeniz gerekir. Bu değer, dilimin işlenmesi için geçen süredir.   

### <a name="how-to-stop-a-running-slice"></a>Çalışan bir dilim nasıl durdurulabilir?
Ardışık hattın yürütülmesini durdurmanız gerekiyorsa, [Askıya Alma-AzDataFactoryPipeline](/powershell/module/az.datafactory/suspend-azdatafactorypipeline) cmdlet'i kullanabilirsiniz. Şu anda, ardışık alan daki askıya alma, devam eden dilim yürütmelerini durdurmaz. Devam eden yürütmeler bittikten sonra, ekstra dilim alınmaz.

Eğer gerçekten hemen tüm yürütmeleri durdurmak istiyorsanız, tek yolu boru hattı silmek ve yeniden oluşturmak olacaktır. Ardışık düzen hattını silmeyi seçerseniz, ardışık düzen tarafından kullanılan tabloları ve bağlantılı hizmetleri silmeniz gerekmez.

[create-factory-using-dotnet-sdk]: data-factory-create-data-factories-programmatically.md
[msdn-class-library-reference]: /dotnet/api/microsoft.azure.management.datafactories.models
[msdn-rest-api-reference]: /rest/api/datafactory/

[adf-powershell-reference]: /powershell/module/az.datafactory/
[azure-portal]: https://portal.azure.com
[set-azure-datafactory-slice-status]: /powershell/module/az.datafactory/set-Azdatafactoryslicestatus

[adf-pricing-details]: https://go.microsoft.com/fwlink/?LinkId=517777
[hdinsight-supported-regions]: https://azure.microsoft.com/pricing/details/hdinsight/
[hdinsight-alternate-storage]: https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx
[hdinsight-alternate-storage-2]: https://blogs.msdn.com/b/cindygross/archive/2014/05/05/use-additional-storage-accounts-with-hdinsight-hive.aspx
