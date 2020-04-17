---
title: "Öğretici: Azure HDInsight'ta satış öngörüleri elde etmek için uçlardan uca bir ETL ardışık kaynak oluşturma"
description: Spark on-demand kümelerini ve Power BI'yi kullanarak satış verilerinden öngörüler elde etmek için Azure HDInsight ile ETL ardışık hatlar oluşturma'yı nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.custom: hdinsightactive
ms.date: 04/15/2020
ms.openlocfilehash: c213b0089af0af295d44afd38bbc5c17b6db159d
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81535239"
---
# <a name="tutorial-create-an-end-to-end-data-pipeline-to-derive-sales-insights-in-azure-hdinsight"></a>Öğretici: Azure HDInsight'ta satış öngörüleri elde etmek için uçlardan uca veri ardışık bir kaynak oluşturma

Bu öğreticide, ayıklama, dönüştürme ve yükleme (ETL) işlemlerini gerçekleştiren uçdan uca bir veri ardışık yapı oluşturacaksınız. Ardışık iş, verileri sorgulamak ve işlemek için Azure HDInsight'ta çalışan [Apache Spark](./spark/apache-spark-overview.md) ve Apache Hive kümelerini kullanır. Ayrıca veri depolama için Azure Veri Gölü Depolama Gen2 ve görselleştirme için Power BI gibi teknolojileri de kullanırsınız.

Bu veri ardışık alanı çeşitli mağazalardaki verileri birleştirir, istenmeyen verileri kaldırır, yeni verileri birleştirir ve iş öngörülerini görselleştirmek için tüm bunları depolama alanınıza yükler. [Ekstresi, dönüştürme ve yük (ETL) ölçeğinde](./hadoop/apache-hadoop-etl-at-scale.md)ETL boru hatları hakkında daha fazla bilgi edinin.

![ETL mimarisi](./media/hdinsight-sales-insights-etl/architecture.png)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Azure CLI - en az sürüm 2.2.0. Bkz. [Azure CLI'yi yükleyin.](https://docs.microsoft.com/cli/azure/install-azure-cli)

* jq, bir komut satırı JSON işlemci.  Bkz. [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).

* Azure yerleşik [rolünün](../role-based-access-control/built-in-roles.md)bir üyesi - sahibi .

* Veri Fabrikası ardışık hattını tetiklemek için PowerShell kullanıyorsanız, [Az Modülü'ne](https://docs.microsoft.com/powershell/azure/overview)ihtiyacınız vardır.

* Bu öğreticinin sonunda iş öngörülerini görselleştirmek için [BI Masaüstü'nü](https://aka.ms/pbiSingleInstaller) güçleyin.

## <a name="create-resources"></a>Kaynak oluşturma

### <a name="clone-the-repository-with-scripts-and-data"></a>Depoyu komut dosyaları ve verilerle klonlama

1. Azure aboneliğinizde oturum açın. Azure Bulut Su Şurası'nı kullanmayı planlıyorsanız, kod bloğunun sağ üst köşesinde **deneyin'i** seçin. Else, aşağıdaki komutu girin:

    ```azurecli-interactive
    az login

    # If you have multiple subscriptions, set the one to use
    # az account set --subscription "SUBSCRIPTIONID"
    ```

1. Azure rol [sahibinin](../role-based-access-control/built-in-roles.md)bir üyesi olduğunuzdan emin olun. Hesabınızla değiştirin `user@contoso.com` ve ardından komutu girin:

    ```azurecli
    az role assignment list \
    --assignee "user@contoso.com" \
    --role "Owner"
    ```

    Hiçbir kayıt döndürülmezse, üye olmazsınız ve bu öğreticiyi tamamlayamazsınız.

1. [HDInsight satış istatistikleri ETL deposundan](https://github.com/Azure-Samples/hdinsight-sales-insights-etl)bu öğretici için veri ve komut indirin. Aşağıdaki komutu girin:

    ```bash
    git clone https://github.com/Azure-Samples/hdinsight-sales-insights-etl.git
    cd hdinsight-sales-insights-etl
    ```

1. Oluşturulduğundan emin olun. `salesdata scripts templates` Aşağıdaki komutla doğrulayın:

   ```bash
   ls
   ```

### <a name="deploy-azure-resources-needed-for-the-pipeline"></a>Ardışık iş tonu için gereken Azure kaynaklarını dağıtma

1. Girerek tüm komut dosyaları için yürütme izinleri ekleyin:

    ```bash
    chmod +x scripts/*.sh
    ````

1. Kaynak grubu için değişkeni ayarlayın. Varolan veya yeni bir kaynak grubunun adıyla değiştirin `RESOURCE_GROUP_NAME` ve ardından komutu girin:

    ```bash
    resourceGroup="RESOURCE_GROUP_NAME"
    ```

1. Betiği yürütün. İstenilen bir değerle değiştirin `LOCATION` ve komutu girin:

    ```bash
    ./scripts/resources.sh $resourceGroup LOCATION
    ```

    Hangi bölgeyi belirtebileceğinizden emin değilseniz, [az hesap listesi konumları](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-list-locations) komutuyla aboneliğiniz için desteklenen bölgelerin listesini alabilirsiniz.

    Komut aşağıdaki kaynakları dağıtacak:

    * Azure Blob depolama hesabı. Bu hesap, şirket satış verilerini tutar.
    * Bir Azure Veri Gölü Depolama Gen2 hesabı. Bu hesap, her iki HDInsight kümeleri için depolama hesabı olarak hizmet verecektir. [Data Lake Storage Gen2 ile Azure HDInsight entegrasyonunda](https://azure.microsoft.com/blog/azure-hdinsight-integration-with-data-lake-storage-gen-2-preview-acl-and-security-update/)HDInsight ve Data Lake Storage Gen2 hakkında daha fazla bilgi edinin.
    * Kullanıcı tarafından atanan yönetilen kimlik. Bu hesap, HDInsight kümelerinin Veri Gölü Depolama Gen2 hesabına erişmesini sağlar.
    * Bir Apache Kıvılcım kümesi. Bu küme, ham verileri temizlemek ve dönüştürmek için kullanılacaktır.
    * Bir Apache Hive [Interactive Query](./interactive-query/apache-interactive-query-get-started.md) kümesi. Bu küme, satış verilerini sorgulamaya ve Power BI ile görselleştirmeye olanak sağlar.
    * Ağ güvenlik grubu (NSG) kuralları tarafından desteklenen bir Azure sanal ağı. Bu sanal ağ kümelerin iletişim kurmasını sağlar ve iletişimlerini güvence altına alar.

Küme oluşturma yaklaşık 20 dakika sürebilir.

Kümelere SSH erişimi için varsayılan `Thisisapassword1`parola. Parolayı değiştirmek istiyorsanız, `./templates/resourcesparameters_remainder.json` dosyaya gidin ve `sparksshPassword`, , `sparkClusterLoginPassword` `llapClusterLoginPassword`ve `llapsshPassword` parametreleriçin parolayı değiştirin.

### <a name="verify-deployment-and-collect-resource-information"></a>Dağıtımı doğrulama ve kaynak bilgilerini toplama

1. Dağıtımınızın durumunu denetlemek istiyorsanız, Azure portalındaki kaynak grubuna gidin. **Ayarlar** **altında, Dağıtımlar'ı**ve dağıtımınızı seçin. Burada başarıyla dağıtılan kaynakları ve halen devam etmekte olan kaynakları görebilirsiniz.

1. Kümelerin adlarını görüntülemek için aşağıdaki komutu girin:

    ```bash
    sparkClusterName=$(cat resourcesoutputs_remainder.json | jq -r '.properties.outputs.sparkClusterName.value')
    llapClusterName=$(cat resourcesoutputs_remainder.json | jq -r '.properties.outputs.llapClusterName.value')

    echo "Spark Cluster" $sparkClusterName
    echo "LLAP cluster" $llapClusterName
    ```

1. Azure depolama hesabını ve erişim anahtarını görüntülemek için aşağıdaki komutu girin:

    ```azurecli
    blobStorageName=$(cat resourcesoutputs_storage.json | jq -r '.properties.outputs.blobStorageName.value')

    blobKey=$(az storage account keys list \
        --account-name $blobStorageName \
        --resource-group $resourceGroup \
        --query [0].value -o tsv)

    echo $blobStorageName
    echo $blobKey
    ```

1. Veri Gölü Depolama Gen2 hesabını ve erişim anahtarını görüntülemek için aşağıdaki komutu girin:

    ```azurecli
    ADLSGen2StorageName=$(cat resourcesoutputs_storage.json | jq -r '.properties.outputs.adlsGen2StorageName.value')

    adlsKey=$(az storage account keys list \
        --account-name $ADLSGen2StorageName \
        --resource-group $resourceGroup \
        --query [0].value -o tsv)

    echo $ADLSGen2StorageName
    echo $adlsKey
    ```

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

Azure Veri Fabrikası, Azure Ardışık Alanları'nın otomatiklenmesine yardımcı olan bir araçtır. Bu görevleri gerçekleştirmenin tek yolu bu değildir, ancak süreçleri otomatikleştirmek için harika bir yoldur. Azure Veri Fabrikası hakkında daha fazla bilgi için [Azure Veri Fabrikası belgelerine](https://azure.microsoft.com/services/data-factory/)bakın.

Bu veri fabrikası, iki faaliyetleri ile bir boru hattı olacak:

* İlk etkinlik, veri alımını taklit etmek için Azure Blob depolamadaki verileri Veri Gölü Depolama Gen 2 depolama hesabına kopyalar.
* İkinci etkinlik, Kıvılcım kümesindeki verileri dönüştürür. Komut dosyası, istenmeyen sütunları kaldırarak verileri dönüştürür. Ayrıca, tek bir işlemin oluşturduğu geliri hesaplayan yeni bir sütun ekler.

Azure Veri Fabrikası ardışık hattınızı ayarlamak için aşağıdaki komutu uygulayın.  Hala dizinde `hdinsight-sales-insights-etl` olmalısın.

```bash
blobStorageName=$(cat resourcesoutputs_storage.json | jq -r '.properties.outputs.blobStorageName.value')
ADLSGen2StorageName=$(cat resourcesoutputs_storage.json | jq -r '.properties.outputs.adlsGen2StorageName.value')

./scripts/adf.sh $resourceGroup $ADLSGen2StorageName $blobStorageName
```

Bu komut dosyası aşağıdaki şeyleri yapar:

1. Data Lake Storage `Storage Blob Data Contributor` Gen2 depolama hesabında izinleri olan bir hizmet ilkesi oluşturur.
1. Post isteklerini [Data Lake Storage Gen2 dosya sistemi REST API'ye](https://docs.microsoft.com/rest/api/storageservices/datalakestoragegen2/filesystem/create)yetkilendirmek için bir kimlik doğrulama belirteci elde eder.
1. Veri Gölü Depolama Gen2 depolama hesabınızın gerçek adını `sparktransform.py` `query.hql` ve dosyalardaki bilgileri doldurur.
1. Veri Gölü Depolama Gen2 ve Blob depolama hesapları için depolama anahtarları alır.
1. İlişkili bağlantılı hizmetleri ve etkinlikleriyle birlikte bir Azure Veri Fabrikası ardışık hattı oluşturmak için başka bir kaynak dağıtımı oluşturur. Bağlı hizmetlerin depolama hesaplarına doğru şekilde erişebilmeleri için depolama anahtarlarını şablon dosyasına parametre olarak geçirir.

## <a name="run-the-data-pipeline"></a>Veri ardışık lığını çalıştırma

### <a name="trigger-the-data-factory-activities"></a>Veri Fabrikası faaliyetlerini tetikle

Oluşturduğunuz Veri Fabrikası ardışık hattındaki ilk etkinlik, verileri Blob depolamadan Veri Gölü Depolama Gen2'ye taşır. İkinci etkinlik, verilerdeki Kıvılcım dönüşümlerini uygular ve dönüştürülen .csv dosyalarını yeni bir konuma kaydeder. Tüm boru hattının tamamlanması birkaç dakika sürebilir.

Veri Fabrikası adını almak için aşağıdaki komutu girin:

```azurecli
cat resourcesoutputs_adf.json | jq -r '.properties.outputs.factoryName.value'
```

Ardışık mayıda tetiklemek için şunları yapabilirsiniz:

* PowerShell'deki Veri Fabrikası boru hattını tetikle. Değiştirin `RESOURCEGROUP` `DataFactoryName` ve uygun değerlerle aşağıdaki komutları çalıştırın:

    ```powershell
    # If you have multiple subscriptions, set the one to use
    # Select-AzSubscription -SubscriptionId "<SUBSCRIPTIONID>"

    $resourceGroup="RESOURCEGROUP"
    $dataFactory="DataFactoryName"

    $pipeline =Invoke-AzDataFactoryV2Pipeline `
        -ResourceGroupName $resourceGroup `
        -DataFactory $dataFactory `
        -PipelineName "IngestAndTransform"

    Get-AzDataFactoryV2PipelineRun `
        -ResourceGroupName $resourceGroup  `
        -DataFactoryName $dataFactory `
        -PipelineRunId $pipeline
    ```

    İlerlemeyi `Get-AzDataFactoryV2PipelineRun` izlemek için gerektiği gibi yeniden yürütün.

    Veya

* Veri fabrikasını açın ve **Yazar & Monitör'ü**seçin. Geçitten `IngestAndTransform` boru hattını tetikle. Portal üzerinden boru hatlarıtetikleme hakkında daha fazla bilgi için, [Azure Veri Fabrikası'nı kullanarak HDInsight'ta isteğe bağlı Apache Hadoop kümeleri oluştur'a](hdinsight-hadoop-create-linux-clusters-adf.md#trigger-a-pipeline)bakın.

Ardışık hattın çalıştırDığını doğrulamak için aşağıdaki adımlardan birini atabilirsiniz:

* Portal aracılığıyla veri fabrikanızdaki **Monitör** bölümüne gidin.
* Azure Depolama Gezgini'nde, Veri Gölü Depolama Gen 2 depolama hesabınıza gidin. `files` Dosya sistemine gidin ve ardından klasöre `transformed` gidin ve ardışık düzende başarılı olup olmadığını görmek için içeriğini kontrol edin.

HDInsight'ı kullanarak verileri dönüştürmenin diğer yolları için [Jupyter Notebook'u kullanma yla ilgili bu makaleye](/azure/hdinsight/spark/apache-spark-load-data-run-query)bakın.

### <a name="create-a-table-on-the-interactive-query-cluster-to-view-data-on-power-bi"></a>Power BI'deki verileri görüntülemek için Etkileşimli Sorgu kümesinde bir tablo oluşturma

1. SCP `query.hql` kullanarak dosyayı LLAP kümesine kopyalayın. Komutu girin:

    ```bash
    llapClusterName=$(cat resourcesoutputs_remainder.json | jq -r '.properties.outputs.llapClusterName.value')
    scp scripts/query.hql sshuser@$llapClusterName-ssh.azurehdinsight.net:/home/sshuser/
    ```

    Hatırlatma: Varsayılan `Thisisapassword1`parola.

1. LLAP kümesine erişmek için SSH'yi kullanın. Komutu girin:

    ```bash
    ssh sshuser@$llapClusterName-ssh.azurehdinsight.net
    ```

1. Komut dosyasını çalıştırmak için aşağıdaki komutu kullanın:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f query.hql
    ```

    Bu komut dosyası, Etkileşimli Sorgu kümesinde Power BI'den erişebileceğiniz yönetilen bir tablo oluşturur.

### <a name="create-a-power-bi-dashboard-from-sales-data"></a>Satış verilerinden Güç BI panosu oluşturma

1. Power BI Desktop’ı açın.

1. Menüden **Veri** > **Al'a gidin...**  >  **Azure**Azure > **HDInsight Etkileşimli Sorgu**.

1. **Bağlan**’ı seçin.

1. **HDInsight Interactive Query** iletişim kutusundan:
    1. **Sunucu** metin kutusuna LLAP kümenizin adını ' `https://LLAPCLUSTERNAME.azurehdinsight.net`ın biçiminde girin.
    1. **Veritabanı** metin kutusuna `default`girin.
    1. **Tamam'ı**seçin.

1. **AzureHive** iletişim kutusundan:
    1. Kullanıcı **adı** metin kutusuna. `admin`
    1. **Parola** metin kutusuna. `Thisisapassword1`
    1. **Bağlan**’ı seçin.

1. **Navigator'dan** `sales`verileri seçmek `sales_raw` ve/veya önizlemek için seçin. Veriler yüklendikten sonra oluşturmak istediğiniz panoyu deneyebilirsiniz. Power BI panoları ile başlamak için aşağıdaki bağlantılara bakın:

* [Power BI tasarımcıları için panolara giriş](https://docs.microsoft.com/power-bi/service-dashboards)
* [Öğretici: Power BI hizmeti ile başlayın](https://docs.microsoft.com/power-bi/service-get-started)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz, bunlar için ücretlendirilmemek için aşağıdaki komutu kullanarak tüm kaynakları silin.

1. Kaynak grubunu kaldırmak için komutu girin:

    ```azurecli
    az group delete -n $resourceGroup
    ```

1. Hizmet ilkesini kaldırmak için komutları girin:

    ```azurecli
    servicePrincipal=$(cat serviceprincipal.json | jq -r '.name')
    az ad sp delete --id $servicePrincipal
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Ölçekte ayıklama, dönüştürme ve yükleme (ETL)](./hadoop/apache-hadoop-etl-at-scale.md)
