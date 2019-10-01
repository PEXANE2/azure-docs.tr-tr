---
title: "Öğretici: Sales Insights 'ı türetmek için uçtan uca ayıklama, dönüştürme, yükleme (ETL) işlem hattı oluşturma"
description: Spark isteğe bağlı kümelerini ve Power BI kullanarak satış verilerinden Öngörüler elde etmek için Azure HDInsight ile ETL işlem hatları oluşturma ' yı nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 09/30/2019
ms.author: hrasheed
ms.openlocfilehash: 52509f886ac9882c372de401ca163ccef418837f
ms.sourcegitcommit: 8bae7afb0011a98e82cbd76c50bc9f08be9ebe06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71695823"
---
# <a name="tutorial-create-an-end-to-end-data-pipeline-to-derive-sales-insights"></a>Öğretici: Sales Insights 'ı türetmek için uçtan uca veri işlem hattı oluşturma

Bu öğreticide, ayıklama, dönüştürme ve yükleme işlemlerini gerçekleştiren uçtan uca bir veri işlem hattı oluşturacaksınız. İşlem hattı, verileri sorgulamak ve işlemek için Azure HDInsight üzerinde çalışan Apache Spark ve Apache Hive kümelerini kullanır. Kullanılan diğer teknolojiler, veri depolama için Data Lake Storage 2. ve görselleştirme için Power BI içerir.

Bu veri ardışık düzeni, tüm farklı depolardaki verileri birleştirir, istenmeyen verileri kaldırır, yeni verileri ekler ve iş öngörülerini görselleştirmek için bunu depolama alanına geri yükler. ETL işlem hatları hakkında daha fazla bilgi için bkz. [Ölçek, dönüştürme ve yükleme (ETL)](./hadoop/apache-hadoop-etl-at-scale.md).

![ETL mimarisi](./media/hdinsight-sales-insights-etl/architecture.png)

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

Bu öğreticinin sonunda iş öngörülerini görselleştirmek için [Power BI Desktop](https://www.microsoft.com/download/details.aspx?id=45331) indirin.

## <a name="create-resources"></a>Kaynak oluşturma

### <a name="clone-the-repository-with-scripts-and-data"></a>Depoyu betikler ve verilerle kopyalayın

1. [Azure portalda](https://portal.azure.com) oturum açma
1. Üstteki menü çubuğundan Cloud Shell açın. Azure Cloud Shell istenirse bir dosya paylaşma oluşturmak için aboneliğinizi seçin.

    ![Azure Cloud Shell 'i açın](./media/hdinsight-sales-insights-etl/hdinsight-sales-insights-etl-click-cloud-shell.png)
1. "Ortam Seç" açılan menüsünden "Bash" seçeneğini belirleyin.
1. Azure hesabınızda oturum açın ve aboneliği ayarlayın. 
1. Proje için kaynak grubunu ayarlayın.
    1. Benzersiz bir kaynak grubu adı seçin.
    1. Sonraki adımlarda kullanılacak değişkenleri ayarlamak için Azure Cloud Shell aşağıdaki kod parçacığını çalıştırın

        ```azurecli-interactive 
        resourceGroup="<RESOURCE GROUP NAME>"
        subscriptionID="<SUBSCRIPTION ID>"
        
        az account set --subscription $subscriptionID
        az group create --name $resourceGroup --location westus
        ```

1. Cloud Shell aşağıdaki komutları yazarak [HDInsight Sales ıNSIGHTS ETL deposundan](https://github.com/Azure-Samples/hdinsight-sales-insights-etl) Bu öğreticinin verilerini ve komut dosyalarını indirin:

        ```azurecli-interactive 
        git clone https://github.com/Azure-Samples/hdinsight-sales-insights-etl.git
        cd hdinsight-sales-insights-etl
        ```

1. Aşağıdaki dosyaların ve dizinlerin oluşturulduğunu görmek için Kabuk isteminde `ls` yazın:

    ```output
    /salesdata/
    /scripts/
    /templates/
    ```

### <a name="deploy-azure-resources-needed-for-the-pipeline"></a>İşlem hattı için gereken Azure kaynaklarını dağıtma 

1. Betik için yürütme izinleri ekleme `chmod +x scripts/*.sh`
1. Aşağıdaki kaynakları Azure 'da dağıtmak üzere betiği çalıştırmak için `./scripts/resources.sh <RESOURCE_GROUP_NAME> <LOCATION>` komutunu kullanın:

    1. Azure Blob depolama hesabı-bu hesap şirket satışları verilerini tutar
    2. Bir Azure Data Lake Storage 2. hesabı-bu hesap, her iki HDInsight kümesi için de depolama hesabı olarak görev yapar. HDInsight ve [Azure hdınsight Data Lake Storage 2. Data Lake Storage 2. ile tümleştirme](https://azure.microsoft.com/blog/azure-hdinsight-integration-with-data-lake-storage-gen-2-preview-acl-and-security-update/)hakkında daha fazla bilgi edinin.
    3. Kullanıcı tarafından atanan yönetilen kimlik-bu hesap, HDInsight kümelerinin Data Lake Storage 2. hesabına erişmesini sağlar.
    4. Apache Spark Luster-bu küme, ham verileri temizlemek ve dönüştürmek için kullanılacaktır
    5. Apache Hive etkileşimli sorgu kümesi-bu küme, satış verilerinin sorgulanmasını Power BI ile görselleştirmeye olanak tanır
    6. Ağ güvenlik grubu (NSG) kuralları tarafından desteklenen bir Azure sanal ağı-bu sanal ağ, kümelerin iletişim kurmasına ve ayrıca iletişimlerini güvenlik altına almasına izin verir. 

Küme oluşturma 20 dakika sürebilir.

@No__t-0 betiği, istenen yapılandırmayla belirtilen kaynakları oluşturmak için bir Kaynak Yöneticisi şablonu (`resourcestemplate.json`) kullanan aşağıdaki komutu içerir.

```azurecli-interactive 
az group deployment create --name ResourcesDeployment \
    --resource-group $resourceGroup \
    --template-file resourcestemplate.json \
    --parameters "@resourceparameters.json"
```

@No__t-0 betiği, şu komutu kullanarak Sales Data CSV dosyalarını yeni oluşturulan BLOB depolama hesabına da yükler:

```
az storage blob upload-batch -d rawdata \
    --account-name <BLOB STORAGE NAME> -s ./ --pattern *.csv
```

Kümelere SSH erişimi için kullanılan varsayılan parola `Thisisapassword1` ' dır. Parolayı değiştirmek istiyorsanız, `resourcesparameters.json` dosyasına gidin ve `sparksshPassword`, `sparkClusterLoginPassword`, `llapClusterLoginPassword` ve `llapsshPassword` parametrelerinin parolasını değiştirin.

### <a name="verify-deployment-and-collect-resource-information"></a>Dağıtımı doğrulama ve kaynak bilgilerini toplama

1. Dağıtımınızın durumunu denetlemek isterseniz Azure portal kaynak grubu ' na gidin. **Ayarlar**altında **dağıtımlar** ' a tıklayın. Dağıtımınızın adına tıklayın `ResourcesDeployment`. Burada, başarıyla dağıtılan ve hala sürmekte olan kaynakları görebilirsiniz.
1. Dağıtım tamamlandıktan sonra, Azure portal > **kaynak gruplarına** > < RESOURCE_GROUP_NAME >
1. Satış dosyalarını depolamak için oluşturulan yeni Azure Depolama hesabını bulun. Depolama hesabının adı `blob` ile başlar ve sonra rastgele bir dize içerir. 
    1. Daha sonra kullanmak üzere depolama hesabı adını bir yere göz önüne alın.
    1. BLOB depolama hesabının adına tıklayın.
    1. Portalın sol tarafındaki **Ayarlar**altında **erişim anahtarları**' na tıklayın.
    1. **KEY1** Box içindeki dizeyi kopyalayın ve daha sonra kullanmak üzere kaydedin.
1. HDInsight kümeleri için depolama olarak oluşturulan Data Lake Storage 2. hesabını bulun. Bu hesap, BLOB depolama hesabıyla aynı kaynak grubunda bulunur, ancak `adlsgen2` ile başlar.
    1. Data Lake Storage 2. hesabının adını bir yere göz önüne alın.
    1. Data Lake Storage 2. hesabının adına tıklayın.
    1. Portalın sol tarafında, **Ayarlar**' ın altında, **erişim tuşları** ' na tıklayın.
    1. **KEY1** Box içindeki dizeyi kopyalayın ve daha sonra kullanmak üzere kaydedin.

> [!Note]
> Depolama hesaplarının adlarını öğrendikten sonra, Azure Cloud Shell isteminde aşağıdaki komutu kullanarak hesap anahtarlarını da alabilirsiniz:
> ```azurecli-interactive
> az storage account keys list \
>    --account-name <STORAGE NAME> \
>    --resource-group $rg \
>    --output table
> ```

### <a name="create-an-azure-data-factory"></a>Azure Data Factory oluşturma

Azure Data Factory, Azure Pipelines otomatik hale getirmeye yardımcı olan bir araçtır. Bu görevleri gerçekleştirmenin tek yolu değildir, ancak bu işlemleri otomatik hale getirmek için harika bir yoldur. Azure Data Factory hakkında daha fazla bilgi için bkz. [Azure Data Factory](https://azure.microsoft.com/services/data-factory/)hakkında daha fazla bilgi edinin. 

Bu Azure Data Factory iki etkinlikle bir işlem hattı olacaktır: 

1. İlk etkinlik verileri Azure Blob depolama alanından Data Lake Storage Gen 2 depolama hesabına kopyalayarak veri alımı taklit eder.
2. İkinci etkinlik Spark kümesindeki verileri dönüştürür. Betik, istenmeyen sütunları kaldırarak ve tek bir işlem tarafından oluşturulan geliri hesaplayan yeni bir sütun ekleyerek verileri dönüştürür.

Azure Data Factory işlem hattınızı ayarlamak için, komut dosyasını çalıştırın `adf.sh`:

1. @No__t kullanarak dosya üzerinde yürütme izinleri ekleme-0
1. `./adf.sh` komutuyla betiği yürütün 

Bu betik aşağıdaki işlemleri yapar:

1. Data Lake Storage 2. depolama hesabında `Storage Blob Data Contributor` izinleriyle bir hizmet sorumlusu oluşturur.
1. [Data Lake Storage 2. dosya SISTEMINE](https://docs.microsoft.com/rest/api/storageservices/datalakestoragegen2/filesystem/create)post isteklerini yetkilendirmek için bir kimlik doğrulama belirteci edinir REST API.
1. @No__t-0 ve `query.hql` dosyalarındaki Data Lake Storage 2. depolama hesabınızın gerçek adını doldurur.
1. Data Lake Storage 2. ve BLOB depolama hesabı için depolama anahtarlarını alır.
1. , İlişkili bağlı hizmetleri ve etkinlikleri ile bir Azure Data Factory işlem hattı oluşturmak için başka bir kaynak dağıtımı oluşturur. Bağlı hizmetlerin depolama hesaplarına doğru şekilde erişebilmeleri için depolama anahtarlarını şablon dosyasına parametreler olarak geçirir.

ADF işlem hattı aşağıdaki komut kullanılarak dağıtılır:

```azurecli-interactive
az group deployment create --name ADFDeployment \
    --resource-group $resourceGroup \
    --template-file adftemplate.json \
    --parameters "@adfparameters.json"
```

## <a name="run-the-data-pipeline"></a>Veri işlem hattını çalıştırma

### <a name="trigger-the-adf-activities"></a>ADF etkinliklerini tetikleyin

ADF 'nin oluşturduğu ADF işlem hattının ilk etkinliği verileri blob depolamadan Data Lake Storage 2. İkinci etkinlik, verilere Spark dönüştürmelerini uygular ve dönüştürülen CSV dosyalarını yeni bir konuma kaydeder. Tüm işlem hattının tamamlanması birkaç dakika sürebilir.

İşlem hatlarını tetiklemek için şunlardan birini yapabilirsiniz:

1. PowerShell 'de ADF işlem hatlarını tetiklemek için aşağıdaki komutları çalıştırın. 

    ```powershell
    Invoke-AzDataFactoryV2Pipeline -DataFactory $df -PipelineName "CopyPipeline_k8z" 
    Invoke-AzDataFactoryV2Pipeline -DataFactory $df -PipelineName "sparkTransformPipeline"
    ```

1. Ayrıca, Data Factory açıp yazar & Izleyicisi ' ni seçip kopyalama işlem hattını, ardından portaldan Spark işlem hattını tetikleyebilirsiniz. Portal aracılığıyla işlem hatlarını tetikleme hakkında bilgi için bkz. [HDInsight 'ta isteğe bağlı Apache Hadoop kümeleri oluşturma Azure Data Factory](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-create-linux-clusters-adf#trigger-a-pipeline) .

İşlem hatları yürütüldüğünü doğrulamak için, aşağıdaki adımlardan birini yapabilirsiniz:

1. Portal üzerinden Azure Data Factory **izleyici** bölümüne gidin.
2. Data Lake Storage Gen 2 depolama hesabı Depolama Gezgini ' ne gidin, `files` dosya sistemine gidin ve `transformed` klasörüne gidip işlem hattının başarılı olup olmadığını görmek için içeriğini denetleyin.

HDInsight kullanarak verileri dönüştürmenin diğer yolları için [Jupyter Not defterini](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-load-data-run-query) kullanma hakkında bu makaleye göz atın

### <a name="create-a-table-on-the-interactive-query-cluster-to-view-data-on-power-bi"></a>Power BI verileri görüntülemek için etkileşimli sorgu kümesinde tablo oluşturma

1. SCP kullanarak Query. HQL dosyasını LLAP kümesine kopyalayın:

    ```
    scp scripts/query.hql sshuser@<clustername>-ssh.azurehdinsight.net:/home/sshuser/
    ```

2. Aşağıdaki komutu kullanarak LLAP kümesine SSH yapın ve parolanızı girin. @No__t-0 dosyasını değiştirmediyse, parola `Thisisapassword1` ' dir.

    ```
    ssh sshuser@<clustername>-ssh.azurehdinsight.net
    ```

3. Betiği yürütmek için aşağıdaki komutu çalıştırın

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f query.hql
    ```

Bu betik, etkileşimli sorgu kümesinde Power BI erişebileceğiniz bir yönetilen tablo oluşturur. 

### <a name="create-a-power-bi-dashboard-from-sales-data"></a>Satış verilerinden Power BI panosu oluşturma

1. Power BI Desktop açın
1. **Veri Al**’ı seçin.
1. **HDInsight etkileşimli sorgu kümesi**araması yapın.
1. Kümenizin URI 'sini buraya yapıştırın. Veritabanı için `https://<LLAP CLUSTER NAME>.azurehdinsight.net` türünde `default` biçiminde olmalıdır.
1. Kümeye erişmek için kullandığınız kullanıcı adını ve parolayı girin.

Veriler yüklendikten sonra, oluşturmak istediğiniz panoyu deneyebilirsiniz. Power BI panoları kullanmaya başlamak için aşağıdaki bağlantılara bakın:

* [Power BI tasarımcıları için panolara giriş](https://docs.microsoft.com/power-bi/service-dashboards)
* [Öğretici: Power BI hizmeti kullanmaya başlayın](https://docs.microsoft.com/power-bi/service-get-started)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam edemeyecekecekseniz, bunlar için ücretlendirilmemek üzere aşağıdaki adımlarla tüm kaynakları silin.

```azurecli-interactive 
az group delete -n $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Ölçeklendirerek ayıklama, dönüştürme ve yükleme (ETL)](./hadoop/apache-hadoop-etl-at-scale.md)
