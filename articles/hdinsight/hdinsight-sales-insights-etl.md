---
title: "Öğretici: Azure HDInsight'ta satış öngörüleri elde etmek için uçlardan uca bir ETL ardışık kaynak oluşturma"
description: Spark on-demand kümelerini ve Power BI'yi kullanarak satış verilerinden öngörüler elde etmek için Azure HDInsight ile ETL ardışık hatlar oluşturma'yı nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.custom: hdinsightactive
ms.date: 03/24/2020
ms.openlocfilehash: a4df99c45b27ad662133010422cae2e30e36e584
ms.sourcegitcommit: 940e16ff194d5163f277f98d038833b1055a1a3e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80247274"
---
# <a name="tutorial-create-an-end-to-end-data-pipeline-to-derive-sales-insights-in-azure-hdinsight"></a>Öğretici: Azure HDInsight'ta satış öngörüleri elde etmek için uçlardan uca veri ardışık bir kaynak oluşturma

Bu öğreticide, ayıklama, dönüştürme ve yükleme (ETL) işlemlerini gerçekleştiren uçdan uca bir veri ardışık yapı oluşturacaksınız. Ardışık iş, verileri sorgulamak ve işlemek için Azure HDInsight'ta çalışan [Apache Spark](./spark/apache-spark-overview.md) ve Apache Hive kümelerini kullanır. Ayrıca veri depolama için Azure Veri Gölü Depolama Gen2 ve görselleştirme için Power BI gibi teknolojileri de kullanırsınız.

Bu veri ardışık alanı çeşitli mağazalardaki verileri birleştirir, istenmeyen verileri kaldırır, yeni verileri birleştirir ve iş öngörülerini görselleştirmek için tüm bunları depolama alanınıza yükler. [Ekstresi, dönüştürme ve yük (ETL) ölçeğinde](./hadoop/apache-hadoop-etl-at-scale.md)ETL boru hatları hakkında daha fazla bilgi edinin.

![ETL mimarisi](./media/hdinsight-sales-insights-etl/architecture.png)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Azure CLI. Bkz. [Azure CLI'yi yükleyin.](https://docs.microsoft.com/cli/azure/install-azure-cli)

* Azure yerleşik [rolünün](../role-based-access-control/built-in-roles.md)bir üyesi - sahibi .

* Bu öğreticinin sonunda iş öngörülerini görselleştirmek için [BI Masaüstü'nü](https://www.microsoft.com/download/details.aspx?id=45331) güçleyin.

## <a name="create-resources"></a>Kaynak oluşturma

### <a name="clone-the-repository-with-scripts-and-data"></a>Depoyu komut dosyaları ve verilerle klonlama

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Üst menü çubuğundan Azure Bulut Kabuğu'nu açın. Cloud Shell'in size bir dosya paylaşımı oluşturması için aboneliğinizi seçin.

   ![Azure Cloud Shell’i açma](./media/hdinsight-sales-insights-etl/hdinsight-sales-insights-etl-click-cloud-shell.png)

1. **Ortam** açılır menüsünde **Bash'i**seçin.

1. Azure rol [sahibinin](../role-based-access-control/built-in-roles.md)bir üyesi olduğunuzdan emin olun. Hesabınızla değiştirin `user@contoso.com` ve ardından komutu girin:

    ```azurecli
    az role assignment list \
    --assignee "user@contoso.com" \
    --role "Owner"
    ```

    Hiçbir kayıt döndürülmezse, üye olmazsınız ve bu öğreticiyi tamamlayamazsınız.

1. Komutu girerek aboneliklerinizi listele:

    ```azurecli
    az account list --output table
    ```

    Bu proje için kullanacağınız aboneliğin kimliğine dikkat edin.

1. Bu proje için kullanacağınız aboneliği ayarlayın. Gerçek `SUBSCRIPTIONID` değerle değiştirin ve komutu girin.

    ```azurecli
    subscriptionID="SUBSCRIPTIONID"
    az account set --subscription $subscriptionID
    ```

1. Proje için yeni bir kaynak grubu oluşturun. İstenilen adla değiştirin `RESOURCEGROUP` ve komutu girin.

    ```azurecli
    resourceGroup="RESOURCEGROUP"
    az group create --name $resourceGroup --location westus
    ```

1. [HDInsight satış istatistikleri ETL deposundan](https://github.com/Azure-Samples/hdinsight-sales-insights-etl)bu öğretici için veri ve komut indirin.  Aşağıdaki komutu girin:

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

1. Betiği yürütün. `RESOURCE_GROUP_NAME` Değiştirin `LOCATION` ve ilgili değerlerle değiştirin ve ardından komutu girin:

    ```bash
    ./scripts/resources.sh RESOURCE_GROUP_NAME LOCATION
    ```

    Komut aşağıdaki kaynakları dağıtacak:

    * Azure Blob depolama hesabı. Bu hesap, şirket satış verilerini tutar.
    * Bir Azure Veri Gölü Depolama Gen2 hesabı. Bu hesap, her iki HDInsight kümeleri için depolama hesabı olarak hizmet verecektir. [Data Lake Storage Gen2 ile Azure HDInsight entegrasyonunda](https://azure.microsoft.com/blog/azure-hdinsight-integration-with-data-lake-storage-gen-2-preview-acl-and-security-update/)HDInsight ve Data Lake Storage Gen2 hakkında daha fazla bilgi edinin.
    * Kullanıcı tarafından atanan yönetilen kimlik. Bu hesap, HDInsight kümelerinin Veri Gölü Depolama Gen2 hesabına erişmesini sağlar.
    * Bir Apache Kıvılcım kümesi. Bu küme, ham verileri temizlemek ve dönüştürmek için kullanılacaktır.
    * Bir Apache Hive [Interactive Query](./interactive-query/apache-interactive-query-get-started.md) kümesi. Bu küme, satış verilerini sorgulamaya ve Power BI ile görselleştirmeye olanak sağlar.
    * Ağ güvenlik grubu (NSG) kuralları tarafından desteklenen bir Azure sanal ağı. Bu sanal ağ kümelerin iletişim kurmasını sağlar ve iletişimlerini güvence altına alar.

Küme oluşturma yaklaşık 20 dakika sürebilir.

`resources.sh` Komut dosyası aşağıdaki komutları içerir. Önceki adımda komut dosyasını zaten çalıştırdıysanız, bu komutları çalıştırmanız gerekmez.

* `az group deployment create`- Bu komut, istenen yapılandırmaile belirtilen kaynakları oluşturmak için bir Azure Kaynak Yöneticisi şablonu (`resourcestemplate.json`) kullanır.

    ```azurecli
    az group deployment create --name ResourcesDeployment \
        --resource-group $resourceGroup \
        --template-file resourcestemplate.json \
        --parameters "@resourceparameters.json"
    ```

* `az storage blob upload-batch`- Bu komut, satış verilerini .csv dosyalarını bu komutu kullanarak yeni oluşturulan Blob depolama hesabına yükler:

    ```azurecli
    az storage blob upload-batch -d rawdata \
        --account-name <BLOB STORAGE NAME> -s ./ --pattern *.csv
    ```

Kümelere SSH erişimi için varsayılan `Thisisapassword1`parola. Parolayı değiştirmek istiyorsanız, `resourcesparameters.json` dosyaya gidin ve `sparksshPassword`, , `sparkClusterLoginPassword` `llapClusterLoginPassword`ve `llapsshPassword` parametreleriçin parolayı değiştirin.

### <a name="verify-deployment-and-collect-resource-information"></a>Dağıtımı doğrulama ve kaynak bilgilerini toplama

1. Dağıtımınızın durumunu denetlemek istiyorsanız, Azure portalındaki kaynak grubuna gidin. **Ayarlar**altında **Dağıtımlar'ı** seçin. Dağıtımınızın adını seçin. `ResourcesDeployment` Burada başarıyla dağıtılan kaynakları ve halen devam etmekte olan kaynakları görebilirsiniz.

1. Kümelerin adlarını görüntülemek için aşağıdaki komutu girin:

    ```azurecli
    sparkCluster=$(az hdinsight list \
        --resource-group $resourceGroup \
        --query "[?contains(name,'spark')].{clusterName:name}" -o tsv)

    llapCluster=$(az hdinsight list \
        --resource-group $resourceGroup \
        --query "[?contains(name,'llap')].{clusterName:name}" -o tsv)

    echo $sparkCluster
    echo $llapCluster
    ```

1. Azure depolama hesabını ve erişim anahtarını görüntülemek için aşağıdaki komutu girin:

    ```azurecli
    blobStorageName=$(cat resourcesoutputs.json | jq -r '.properties.outputs.blobStorageName.value')

    blobKey=$(az storage account keys list \
        --account-name $blobStorageName \
        --resource-group $resourceGroup \
        --query [0].value -o tsv)

    echo $blobStorageName
    echo $blobKey
    ```

1. Veri Gölü Depolama Gen2 hesabını ve erişim anahtarını görüntülemek için aşağıdaki komutu girin:

    ```azurecli
    ADLSGen2StorageName=$(cat resourcesoutputs.json | jq -r '.properties.outputs.adlsGen2StorageName.value')

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

Azure Veri Fabrikası ardışık sisteminizi ayarlamak için aşağıdaki komutu uygulayın:

```bash
./scripts/adf.sh
```

Bu komut dosyası aşağıdaki şeyleri yapar:

1. Data Lake Storage `Storage Blob Data Contributor` Gen2 depolama hesabında izinleri olan bir hizmet ilkesi oluşturur.
1. Post isteklerini [Data Lake Storage Gen2 dosya sistemi REST API'ye](https://docs.microsoft.com/rest/api/storageservices/datalakestoragegen2/filesystem/create)yetkilendirmek için bir kimlik doğrulama belirteci elde eder.
1. Veri Gölü Depolama Gen2 depolama hesabınızın gerçek adını `sparktransform.py` `query.hql` ve dosyalardaki bilgileri doldurur.
1. Veri Gölü Depolama Gen2 ve Blob depolama hesapları için depolama anahtarları alır.
1. İlişkili bağlantılı hizmetleri ve etkinlikleriyle birlikte bir Azure Veri Fabrikası ardışık hattı oluşturmak için başka bir kaynak dağıtımı oluşturur. Bağlı hizmetlerin depolama hesaplarına doğru şekilde erişebilmeleri için depolama anahtarlarını şablon dosyasına parametre olarak geçirir.

Veri Fabrikası ardışık hattı aşağıdaki komut aracılığıyla dağıtılır:

```azurecli-interactive
az group deployment create --name ADFDeployment \
    --resource-group $resourceGroup \
    --template-file adftemplate.json \
    --parameters "@adfparameters.json"
```

## <a name="run-the-data-pipeline"></a>Veri ardışık lığını çalıştırma

### <a name="trigger-the-data-factory-activities"></a>Veri Fabrikası faaliyetlerini tetikle

Oluşturduğunuz Veri Fabrikası ardışık hattındaki ilk etkinlik, verileri Blob depolamadan Veri Gölü Depolama Gen2'ye taşır. İkinci etkinlik, verilerdeki Kıvılcım dönüşümlerini uygular ve dönüştürülen .csv dosyalarını yeni bir konuma kaydeder. Tüm boru hattının tamamlanması birkaç dakika sürebilir.

Ardışık lıkları tetiklemek için şunları yapabilirsiniz:

* PowerShell'deki Veri Fabrikası boru hatlarını tetikle. Gerçek `DataFactoryName` Veri Fabrikası adı ile değiştirin ve ardından aşağıdaki komutları çalıştırın:

    ```powershell
    Invoke-AzDataFactoryV2Pipeline -DataFactory DataFactoryName -PipelineName "CopyPipeline_k8z"
    Invoke-AzDataFactoryV2Pipeline -DataFactory DataFactoryName -PipelineName "sparkTransformPipeline"
    ```

    Veya

* Veri fabrikasını açın ve **Yazar & Monitör'ü**seçin. Kopya ardışık hattını ve sonra portaldan Spark ardışık hattını tetikle. Portal üzerinden boru hatlarıtetikleme hakkında daha fazla bilgi için, [Azure Veri Fabrikası'nı kullanarak HDInsight'ta isteğe bağlı Apache Hadoop kümeleri oluştur'a](hdinsight-hadoop-create-linux-clusters-adf.md#trigger-a-pipeline)bakın.

Ardışık hatların çalıştırılıştını doğrulamak için aşağıdaki adımlardan birini atabilirsiniz:

* Portal aracılığıyla veri fabrikanızdaki **Monitör** bölümüne gidin.
* Azure Depolama Gezgini'nde, Veri Gölü Depolama Gen 2 depolama hesabınıza gidin. `files` Dosya sistemine gidin ve ardından klasöre `transformed` gidin ve ardışık düzende başarılı olup olmadığını görmek için içeriğini kontrol edin.

HDInsight'ı kullanarak verileri dönüştürmenin diğer yolları için [Jupyter Notebook'u kullanma yla ilgili bu makaleye](/azure/hdinsight/spark/apache-spark-load-data-run-query)bakın.

### <a name="create-a-table-on-the-interactive-query-cluster-to-view-data-on-power-bi"></a>Power BI'deki verileri görüntülemek için Etkileşimli Sorgu kümesinde bir tablo oluşturma

1. SCP `query.hql` kullanarak dosyayı LLAP kümesine kopyalayın. Gerçek `LLAPCLUSTERNAME` adla değiştirin ve komutu girin:

    ```bash
    scp scripts/query.hql sshuser@LLAPCLUSTERNAME-ssh.azurehdinsight.net:/home/sshuser/
    ```

2. LLAP kümesine erişmek için SSH'yi kullanın. Gerçek `LLAPCLUSTERNAME` adla değiştirin ve komutu girin. Dosyayı `resourcesparameters.json` değiştirmediyseniz, parola `Thisisapassword1`.

    ```bash
    ssh sshuser@LLAPCLUSTERNAME-ssh.azurehdinsight.net
    ```

3. Komut dosyasını çalıştırmak için aşağıdaki komutu kullanın:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f query.hql
    ```

Bu komut dosyası, Etkileşimli Sorgu kümesinde Power BI'den erişebileceğiniz yönetilen bir tablo oluşturur.

### <a name="create-a-power-bi-dashboard-from-sales-data"></a>Satış verilerinden Güç BI panosu oluşturma

1. Power BI Desktop’ı açın.
1. **Veri Al**’ı seçin.
1. **HDInsight İnteraktif Sorgu kümesini**arayın.
1. Orada küme için URI yapıştırın. `https://LLAPCLUSTERNAME.azurehdinsight.net` biçiminde olmalıdır.

   Veritabanı `default` için girin.
1. Kümeye erişmek için kullandığınız kullanıcı adını ve parolayı girin.

Veriler yüklendikten sonra oluşturmak istediğiniz panoyu deneyebilirsiniz. Power BI panoları ile başlamak için aşağıdaki bağlantılara bakın:

* [Power BI tasarımcıları için panolara giriş](https://docs.microsoft.com/power-bi/service-dashboards)
* [Öğretici: Power BI hizmeti ile başlayın](https://docs.microsoft.com/power-bi/service-get-started)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz, bunlar için ücretlendirilmemek için aşağıdaki komutu kullanarak tüm kaynakları silin.

```azurecli-interactive
az group delete -n $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Ölçekte ayıklama, dönüştürme ve yükleme (ETL)](./hadoop/apache-hadoop-etl-at-scale.md)
