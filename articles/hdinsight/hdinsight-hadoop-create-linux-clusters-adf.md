---
title: "Öğretici: Veri Fabrikası ile Azure HDInsight'ta isteğe bağlı kümeler"
description: Öğretici - Azure Veri Fabrikası'nı kullanarak HDInsight'ta isteğe bağlı Apache Hadoop kümelerini nasıl oluşturacağınızhakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.date: 03/18/2020
ms.openlocfilehash: b184a42c52384440445181ac44c616c3139e064f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80130681"
---
# <a name="tutorial-create-on-demand-apache-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a>Öğretici: Azure Veri Fabrikası'nı kullanarak HDInsight'ta isteğe bağlı Apache Hadoop kümeleri oluşturun

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Bu eğitimde, Azure Veri Fabrikası'nı kullanarak Azure HDInsight'ta isteğe bağlı [olarak Bir Apache Hadoop](./hadoop/apache-hadoop-introduction.md) kümesioluşturmayı öğrenirsiniz. Ardından, Hive işlerini çalıştırmak ve kümeyi silmek için Azure Veri Fabrikası'ndaki veri ardışık düzenlerini kullanırsınız. Bu öğreticinin sonunda, küme oluşturma, iş çalıştırma ve küme silme işleminin bir zamanlamada gerçekleştirildiği büyük bir veri iş çalışmasını nasıl işlevselleştirebileceğinizi öğrenirsiniz.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Azure Storage hesabı oluşturma
> * Azure Veri Fabrikası etkinliğini anlama
> * Azure portalLarını kullanarak veri fabrikası oluşturma
> * Bağlı hizmetler oluşturma
> * İşlem hattı oluşturma
> * İşlem hattını tetikleme
> * İşlem hattını izleme
> * Çıktıyı doğrulama

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

* PowerShell [Az Modülü](https://docs.microsoft.com/powershell/azure/overview) kuruldu.

* Azure Etkin Dizin hizmet ilkesi. Hizmet ilkesini oluşturduktan sonra, bağlantılı makaledeki yönergeleri kullanarak **uygulama kimliği** ve **kimlik doğrulama anahtarını** aldığınızdan emin olun. Bu değerler daha sonra bu öğretici gerekir. Ayrıca, hizmet sorumlusunun aboneliğin veya kümenin oluşturulduğu kaynak grubunun *Katılımcı* rolünün bir üyesi olduğundan emin olun. Gerekli değerleri almak ve doğru rolleri atamak için [yönergeler](../active-directory/develop/howto-create-service-principal-portal.md)için bir Azure Etkin Dizin hizmeti ilkesi oluştur'a bakın.

## <a name="create-preliminary-azure-objects"></a>Ön Azure nesneleri oluşturma

Bu bölümde, isteğe bağlı olarak oluşturduğunuz HDInsight kümesi için kullanılacak çeşitli nesneler oluşturursunuz. Oluşturulan depolama hesabı, `partitionweblogs.hql`küme üzerinde çalışan bir örnek Apache Hive iş simüle etmek için kullandığınız örnek [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) komut dosyası içerir.

Bu bölümde, depolama hesabı oluşturmak ve depolama hesabı içinde gerekli dosyaları kopyalamak için bir Azure PowerShell komut dosyası kullanır. Bu bölümdeki Azure PowerShell örnek komut dosyası aşağıdaki görevleri gerçekleştirir:

1. Azure'da kaydolur.
2. Bir Azure kaynak grubu oluşturur.
3. Azure Depolama hesabı oluşturur.
4. Depolama hesabında bir Blob kapsayıcısı oluşturur
5. Kopyaları örnek HiveQL komut dosyası (**partitionweblogs.hql**) Blob konteyner. Komut dosyası ' [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql)ndan edinilebilir. Örnek komut dosyası zaten başka bir ortak Blob kapsayıcımevcuttur. Aşağıdaki PowerShell komut dosyası, bu dosyaların bir kopyasını oluşturduğu Azure Depolama hesabına dönüştürür.

### <a name="create-storage-account-and-copy-files"></a>Depolama hesabı oluşturma ve dosyaları kopyalama

> [!IMPORTANT]  
> Azure kaynak grubu ve komut dosyası tarafından oluşturulacak Azure depolama hesabı için adlar belirtin.
> Kaynak **grup adını,** **depolama hesabı adını**ve komut dosyası tarafından çıktı alınan depolama hesabı **anahtarını** yazın. Bir sonraki bölümde onlara ihtiyacın var.

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfv2hiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# If you have multiple subscriptions, set the one to use
# Select-AzSubscription -SubscriptionId "<SUBSCRIPTIONID>"

#endregion

####################################
# Create a resource group, storage, and container
####################################

#region - create Azure resources
Write-Host "`nCreating resource group, storage account and blob container ..." -ForegroundColor Green

New-AzResourceGroup `
    -Name $resourceGroupName `
    -Location $location

New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName `
    -Kind StorageV2 `
    -Location $location `
    -SkuName Standard_LRS `
    -EnableHttpsTrafficOnly 1

$destStorageAccountKey = (Get-AzStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName)[0].Value

$sourceContext = New-AzStorageContext `
    -StorageAccountName $sourceStorageAccountName `
    -Anonymous

$destContext = New-AzStorageContext `
    -StorageAccountName $destStorageAccountName `
    -StorageAccountKey $destStorageAccountKey

New-AzStorageContainer `
    -Name $destContainerName `
    -Context $destContext
#endregion

####################################
# Copy files
####################################
#region - copy files
Write-Host "`nCopying files ..." -ForegroundColor Green

$blobs = Get-AzStorageBlob `
    -Context $sourceContext `
    -Container $sourceContainerName `
    -Blob "hivescripts\hivescript.hql"

$blobs|Start-AzStorageBlobCopy `
    -DestContext $destContext `
    -DestContainer $destContainerName `
    -DestBlob "hivescripts\partitionweblogs.hql"

Write-Host "`nCopied files ..." -ForegroundColor Green
Get-AzStorageBlob `
    -Context $destContext `
    -Container $destContainerName
#endregion

Write-host "`nYou will use the following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

### <a name="verify-storage-account"></a>Depolama hesabını doğrulama

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Soldan, **Tüm hizmetler** > **Genel** > **Kaynak gruplarına**gidin.
1. PowerShell komut dosyanızda oluşturduğunuz kaynak grubu adını seçin. Listelenen çok fazla kaynak grubunuz varsa filtreyi kullanın.
1. Genel **Bakış** görünümünden, kaynak grubunu diğer projelerle paylaşmadığınız sürece listelenen bir kaynak görürsünüz. Bu kaynak, daha önce belirttiğiniz adı içeren depolama hesabıdır. Depolama hesabı adını seçin.
1. **Kapsayıcılar** döşemesini seçin.
1. **adfgetstarted** kapsayıcısını seçin. **Kovan yazıtları**adlı bir klasör görüyorsunuz.
1. Klasörü açın ve örnek komut dosyası dosyası, **partitionweblogs.hql**içerdiğinden emin olun.

## <a name="understand-the-azure-data-factory-activity"></a>Azure Veri Fabrikası etkinliğini anlama

[Azure Veri Fabrikası,](../data-factory/introduction.md) verilerin hareketini ve dönüşümlerini yönetir ve otomatikleştirir. Azure Veri Fabrikası, giriş veri dilimini işlemek ve işleme tamamlandığında kümeyi silmek için tam zamanında bir HDInsight Hadoop kümesi oluşturabilir.

Azure Veri Fabrikası'nda, bir veri fabrikasında bir veya daha fazla veri aktarEciliği olabilir. Veri ardışık bir veya daha fazla etkinlik vardır. İki tür etkinlik vardır:

* [Veri Hareketi Etkinlikleri](../data-factory/copy-activity-overview.md). Verileri kaynak veri deposundan hedef veri deposuna taşımak için veri hareketi etkinliklerini kullanırsınız.
* [Veri Dönüştürme Faaliyetleri](../data-factory/transform-data.md). Verileri dönüştürmek/işlemek için veri dönüştürme etkinliklerini kullanırsınız. HDInsight Kovan Etkinliği, Veri Fabrikası tarafından desteklenen dönüşüm faaliyetlerinden biridir. Bu öğreticide Hive dönüşüm etkinliğini kullanırsınız.

Bu makalede, Hive etkinliğini isteğe bağlı HDInsight Hadoop kümesi oluşturmak üzere yapılandırırsınız. Etkinlik verileri işlemek için çalıştığında, aşağıdaki gibi olur:

1. Bir HDInsight Hadoop kümesi, dilimi işlemek için tam zamanında sizin için otomatik olarak oluşturulur.

2. Giriş verileri kümeüzerinde bir HiveQL komut dosyası çalıştırılarak işlenir. Bu öğreticide, kovan etkinliğiyle ilişkili HiveQL komut dosyası aşağıdaki eylemleri gerçekleştirir:

    * Başka bir tablo **HiveSampleOut**oluşturmak için varolan tabloyu *(kovan örnekleme)* kullanır.
    * **HiveSampleOut** tablosunu özgün *kovan örnek tablosundan*yalnızca belirli sütunlarla doldurur.

3. HDInsight Hadoop kümesi işlem tamamlandıktan sonra silinir ve küme yapılandırılan süre (timeToLive ayarı) için boşta kalır. Bir sonraki veri dilimi bu zaman tolive boşta zaman ile işlemek için kullanılabilir ise, aynı küme dilimi işlemek için kullanılır.  

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. Sol menüden + **Kaynak** > **Analitik** > **Veri Fabrikası**Oluştur'a gidin.

    ![Portalda Azure Veri Fabrikası](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-azure-portal.png "Portalda Azure Veri Fabrikası")

3. **Yeni veri fabrikası** döşemesi için aşağıdaki değerleri girin veya seçin:

    |Özellik  |Değer  |
    |---------|---------|
    |Adı | Veri fabrikası için bir ad girin. Bu adın küresel olarak benzersiz olması gerekir.|
    |Sürüm | **V2'de**bırak. |
    |Abonelik | Azure aboneliğinizi seçin. |
    |Kaynak grubu | PowerShell komut dosyasını kullanarak oluşturduğunuz kaynak grubunu seçin. |
    |Konum | Konum, kaynak grubunu daha önce oluştururken belirttiğiniz konuma otomatik olarak ayarlanır. Bu öğretici için, konum **Doğu ABD**olarak ayarlanır. |
    |GIT'i etkinleştir|Bu kutunun işaretlerini kaldırın.|

    ![Azure portalLarını kullanarak Azure Veri Fabrikası oluşturma](./media/hdinsight-hadoop-create-linux-clusters-adf/azure-portal-create-data-factory.png "Azure portalLarını kullanarak Azure Veri Fabrikası oluşturma")

4. **Oluştur'u**seçin. Bir veri fabrikası oluşturmak 2 ila 4 dakika arasında sürebilir.

5. Veri fabrikası oluşturulduktan sonra, **kaynağa Git** düğmesiyle **dağıtım başarılı** bir bildirim alırsınız.  Veri Fabrikası varsayılan görünümünü açmak için **kaynağa Git'i** seçin.

6. Azure Veri Fabrikası yazma ve izleme portalını başlatmak için **Author & Monitor'u** seçin.

    ![Azure Veri Fabrikası portalına genel bakış](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-portal-overview.png "Azure Veri Fabrikası'na genel bakış")

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma

Bu bölümde, veri fabrikanızda iki bağlantılı hizmet yazarsınız.

* Bir Azure depolama hesabını veri fabrikasına bağlayan **Azure Depolama bağlı hizmeti**. Bu depolama alanı, isteğe bağlı HDInsight kümesi tarafından kullanılır. Ayrıca küme üzerinde çalıştırılmaya başgösteren Hive komut dosyasını da içerir.
* **İsteğe bağlı HDInsight bağlı hizmeti**. Azure Veri Fabrikası otomatik olarak bir HDInsight kümesi oluşturur ve Hive komut dosyasını çalıştırAr. Daha sonra, küme önceden yapılandırılmış bir süre boyunca boşta kaldığında HDInsight kümesini siler.

### <a name="create-an-azure-storage-linked-service"></a>Azure Depolama bağlı hizmeti oluşturma

1. **Başlatalım** sayfasının sol bölmesinden **Yazar** simgesini seçin.

    ![Azure Veri Fabrikası bağlantılı bir hizmet oluşturma](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-edit-tab.png "Azure Veri Fabrikası bağlantılı bir hizmet oluşturma")

2. Pencerenin sol alt köşesinden **Bağlantılar'ı** seçin ve ardından **+Yeni'yi**seçin.

    ![Azure Veri Fabrikası'nda bağlantı oluşturma](./media/hdinsight-hadoop-create-linux-clusters-adf/data-factory-create-new-connection.png "Azure Veri Fabrikası'nda bağlantı oluşturma")

3. Yeni **Bağlantılı Hizmet** iletişim kutusunda **Azure Blob Depolama'yı** seçin ve ardından **Devam et'i**seçin.

    ![Veri Fabrikası için Azure Depolama bağlantılı hizmet oluşturun](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service.png "Veri Fabrikası için Azure Depolama bağlantılı hizmet oluşturun")

4. Depolamabağlantılı hizmet için aşağıdaki değerleri sağlayın:

    |Özellik |Değer |
    |---|---|
    |Adı |`HDIStorageLinkedService` yazın.|
    |Azure aboneliği |Açılan listeden aboneliğinizi seçin.|
    |Depolama hesabı adı |PowerShell komut dosyasının bir parçası olarak oluşturduğunuz Azure Depolama hesabını seçin.|

    **Test bağlantısını** seçin ve başarılı olursa, sonra **Oluştur'u**seçin.

    ![Azure Depolama bağlantılı hizmet için ad sağlama](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-storage-linked-service-details.png "Azure Depolama bağlantılı hizmet için ad sağlama")

### <a name="create-an-on-demand-hdinsight-linked-service"></a>İsteğe bağlı bir HDInsight bağlı hizmeti oluşturma

1. Başka bir bağlı hizmet oluşturmak için **+ Yeni** düğmesini tekrar seçin.

2. Yeni **Bağlantılı Hizmet** **penceresinde, İşlem** sekmesini seçin.

3. **Azure HDInsight'ı**seçin ve ardından **Devam et'i**seçin.

    ![Azure Veri Fabrikası için HDInsight bağlantılı hizmet oluşturun](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service.png "Azure Veri Fabrikası için HDInsight bağlantılı hizmet oluşturun")

4. Yeni **Bağlantılı Hizmet** penceresinde, aşağıdaki değerleri girin ve geri kalanını varsayılan olarak bırakın:

    | Özellik | Değer |
    | --- | --- |
    | Adı | `HDInsightLinkedService` yazın.|
    | Tür | **İsteğe bağlı HDInsight'ı**seçin. |
    | Azure Storage Bağlı Hizmeti | `HDIStorageLinkedService` öğesini seçin. |
    | Küme türü | **Hadoop** seçin |
    | Yaşam süresi | HDInsight kümesinin otomatik olarak silinmeden önce kullanılabilir olmasını istediğiniz süreyi sağlayın.|
    | Hizmet müdürü kimliği | Ön koşulların bir parçası olarak oluşturduğunuz Azure Active Directory hizmet yöneticisinin uygulama kimliğini sağlayın. |
    | Hizmet temel anahtarı | Azure Etkin Dizin hizmet yöneticisinin kimlik doğrulama anahtarını sağlayın. |
    | Küme adı öneki | Veri fabrikası tarafından oluşturulan tüm küme türlerine önceden belirlenmiş bir değer sağlayın. |
    |Abonelik |Açılan listeden aboneliğinizi seçin.|
    | Kaynak grubu seçin | Daha önce kullandığınız PowerShell komut dosyasının bir parçası olarak oluşturduğunuz kaynak grubunu seçin.|
    | İşletim sistemi türü/Cluster SSH kullanıcı adı | Genellikle bir SSH kullanıcı `sshuser`adı girin. |
    | İşletim sistemi türü/Küme SSH parolası | SSH kullanıcısı için parola sağlama |
    | İşletim sistemi türü/Küme kullanıcı adı | Genellikle bir küme kullanıcı `admin`adı girin. |
    | İşletim sistemi türü/Küme parolası | Küme kullanıcısı için bir parola sağlayın. |

    Ardından **Oluştur**’u seçin.

    ![HDInsight bağlantılı hizmet için değerler sağlama](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-linked-service-details.png "HDInsight bağlantılı hizmet için değerler sağlama")

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

1. (artı) düğmesini **+** seçin ve ardından **Pipeline'ı**seçin.

    ![Azure Veri Fabrikası'nda bir ardışık hat lar oluşturma](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-create-pipeline.png "Azure Veri Fabrikası'nda bir ardışık hat lar oluşturma")

1. **Etkinlikler** araç **kutusunda, HDInsight'ı**genişletin ve **Hive** etkinliğini boru hattı tasarımcısı yüzeyine sürükleyin. **Genel** sekmesinde, etkinlik için bir ad sağlayın.

    ![Veri Fabrikası ardışık hattına etkinlik ekleme](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-add-hive-pipeline.png "Veri Fabrikası ardışık hattına etkinlik ekleme")

1. Hive etkinliğinin seçildiğinden emin olun, **HDI Küme** sekmesini seçin ve **HDInsight Bağlantılı Hizmet** açılır listesinden, HDInsight için daha önce oluşturduğunuz bağlantılı hizmeti ( **HDInsightLinkedService)** seçin.

    ![Ardışık hatlar için HDInsight küme ayrıntılarını sağlayın](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-hive-activity-select-hdinsight-linked-service.png "Ardışık hatlar için HDInsight küme ayrıntılarını sağlayın")

1. Komut **Dosyası** sekmesini seçin ve aşağıdaki adımları tamamlayın:

    1. **Komut Dosyası Bağlantılı Hizmet**için açılan listeden **HDIStorageLinkedService'i** seçin. Bu değer, daha önce oluşturduğunuz depolama bağlantılı hizmettir.

    1. **Dosya Yolu** **için, Depolamaya Gözat'ı** seçin ve örnek Hive komut dosyasının bulunduğu konuma gidin. PowerShell komut dosyasını daha önce çalıştırdattıysanız, bu konum `adfgetstarted/hivescripts/partitionweblogs.hql`.

        ![Boru hattı için Hive komut dosyası ayrıntılarını sağlayın](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-path.png "Boru hattı için Hive komut dosyası ayrıntılarını sağlayın")

    1. **Gelişmiş** > **Parametreler**altında, **komut dosyasından Otomatik doldurma'yı**seçin. Bu seçenek, hive komut dosyasında çalışma zamanında değer gerektiren parametreleri arar.

    1. **Değer** metin kutusuna, varolan klasörü `wasbs://adfgetstarted@<StorageAccount>.blob.core.windows.net/outputfolder/`biçiminde ekleyin. Bu yol büyük/küçük harfe duyarlıdır. Bu, komut dosyasının çıktısının depolandığı yoldur. Depolama `wasbs` hesapları artık varsayılan olarak etkinleştirilmiş güvenli aktarım alabildiği için şema gereklidir.

        ![Hive komut dosyası için parametreler sağlayın](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-provide-script-parameters.png "Hive komut dosyası için parametreler sağlayın")

1. Ardışık hattı doğrulamak için **Doğrula'yı** seçin. Doğrulama **>>** penceresini kapatmak için (sağ ok) düğmesini seçin.

    ![Azure Veri Fabrikası ardışık hattını doğrulama](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-validate-all.png "Azure Veri Fabrikası ardışık hattını doğrulama")

1. Son olarak, yapıları Azure Veri Fabrikası'nda yayımlamak için **Tümünü Yayımla'yı** seçin.

    ![Azure Veri Fabrikası ardışık hattını yayımlama](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-publish-pipeline.png "Azure Veri Fabrikası ardışık hattını yayımlama")

## <a name="trigger-a-pipeline"></a>İşlem hattını tetikleme

1. Tasarımcı yüzeyindeki araç çubuğundan Tetik > **tetikle**şimdi **ekle'yi**seçin.

    ![Azure Veri Fabrikası ardışık hattını tetikle](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-trigger-pipeline.png "Azure Veri Fabrikası ardışık hattını tetikle")

2. Açılan yan çubukta **Tamam'ı** seçin.

## <a name="monitor-a-pipeline"></a>İşlem hattını izleme

1. Soldaki **İzleyici** sekmesine geçin. **İşlem Hattı Çalıştırmaları** listesinde bir işlem hattı çalıştırması görürsünüz. **Durum** sütunu altındaki çalıştırın durumuna dikkat edin.

    ![Azure Veri Fabrikası ardışık hattını izleme](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline.png "Azure Veri Fabrikası ardışık hattını izleme")

1. Durumu yenilemek için **Yenile**’yi seçin.

1. Ayrıca, ardışık alanla ilişkili etkinliğin çalıştırını görmek için **Etkinlik Çalışır'ı Görüntüle** simgesini de seçebilirsiniz. Aşağıdaki ekran görüntüsünde, oluşturduğunuz ardışık alanda yalnızca bir etkinlik olduğundan yalnızca bir etkinlik çalıştırılır. Önceki görünüme geçmek için Sayfanın üst kısmındaki **Ardışık Hatlar'ı** seçin.

    ![Azure Veri Fabrikası boru hattı etkinliğini izleme](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-monitor-pipeline-activity.png "Azure Veri Fabrikası boru hattı etkinliğini izleme")

## <a name="verify-the-output"></a>Çıktıyı doğrulama

1. Çıktıyı doğrulamak için Azure portalında bu öğretici için kullandığınız depolama hesabına gidin. Aşağıdaki klasörleri veya kapsayıcıları görmeniz gerekir:

    * Ardışık nedenlerden biri olarak çalıştırılmayan Hive komut dosyasının çıktısını içeren bir **adfgerstarted/outputfolder** görürsünüz.

    * Bir **adfhdidatafactory-\<bağlı hizmet adı>-\<zaman damgası>** konteyner bakın. Bu kapsayıcı, ardışık işlem yürütmenin bir parçası olarak oluşturulan HDInsight kümesinin varsayılan depolama konumudur.

    * Azure Veri Fabrikası iş günlükleri olan bir **adfjobs** kapsayıcısı görürsünüz.  

        ![Azure Veri Fabrikası boru hattı çıktısını doğrulama](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-data-factory-verify-output.png "Azure Veri Fabrikası boru hattı çıktısını doğrulama")

## <a name="clean-up-resources"></a>Kaynakları temizleme

İsteğe bağlı HDInsight küme oluşturma ile HDInsight kümesini açıkça silmeniz gerekmez. Küme, ardışık yapı oluştururken sağladığınız yapılandırmaya göre silinir. Ancak, küme silindikten sonra bile, kümeyle ilişkili depolama hesapları var olmaya devam eder. Bu davranış, verilerinizi sağlam tutabilmeniz için tasarım gereğidir. Ancak, verileri kalıcı olarak sürdürmek istemiyorsanız, oluşturduğunuz depolama hesabını silebilirsiniz.

Alternatif olarak, bu öğretici için oluşturduğunuz tüm kaynak grubunu silebilirsiniz. Bu, depolama hesabını ve oluşturduğunuz Azure Veri Fabrikası'nı siler.

### <a name="delete-the-resource-group"></a>Kaynak grubunu silme

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Sol bölmedeki **Kaynak gruplarını** seçin.
1. PowerShell komut dosyanızda oluşturduğunuz kaynak grubu adını seçin. Listelenen çok fazla kaynak grubunuz varsa filtreyi kullanın. Kaynak grubunu açar.
1. **Kaynaklar** döşemesi üzerinde, kaynak grubunu diğer projelerle paylaşmadığınız sürece varsayılan depolama hesabı ve veri fabrikası listelenir.
1. **Kaynak grubunu sil**'i seçin. Bunu yapmak, depolama hesabını ve depolama hesabında depolanan verileri siler.

    ![Azure portalı kaynak grubunu silme](./media/hdinsight-hadoop-create-linux-clusters-adf/delete-resource-group.png "Kaynak grubunu silme")

1. Silme işlemini onaylamak için kaynak grubu adını girin ve sonra **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, isteğe bağlı HDInsight kümesi oluşturmak ve [Apache Hive](https://hive.apache.org/) işlerini çalıştırmak için Azure Veri Fabrikası'nı nasıl kullanacağınızı öğrendiniz. Özel yapılandırmayla HDInsight kümelerinin nasıl oluşturulabildiğini öğrenmek için bir sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [Özel yapılandırmayla Azure HDInsight kümeleri oluşturun](hdinsight-hadoop-provision-linux-clusters.md)
