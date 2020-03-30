---
title: "Hızlı başlangıç: Azure PowerShell ile Azure HDInsight'ta Apache Spark kümesi oluşturun"
description: Bu hızlı başlangıçta, Azure PowerShell kullanılarak nasıl Azure HDInsight’ta Apache Spark kümesi oluşturulacağı ve basit bir Spark SQL sorgusu çalıştırılacağı gösterilmektedir.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.date: 06/12/2019
ms.custom: mvc
ms.openlocfilehash: 7416c25128da8dcaf803a9f03144110941200ab2
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77049127"
---
# <a name="quickstart-create-apache-spark-cluster-in-azure-hdinsight-using-powershell"></a>Quickstart: PowerShell'i kullanarak Azure HDInsight'ta Apache Spark kümesi oluşturma

Bu hızlı başlangıçta, Azure HDInsight'ta bir Apache Spark kümesi oluşturmak için Azure PowerShell'i kullanırsınız. Daha sonra bir Jupyter dizüstü bilgisayar oluşturun ve Apache Hive tablolarına karşı Sql sorguları çalıştırmak için kullanabilirsiniz. Azure HDInsight kuruluşlara yönelik, yönetilen, tam spektrumlu ve açık kaynaklı bir analiz hizmetidir. Azure HDInsight için Apache Spark çerçevesi, bellek içi işleme yi kullanarak hızlı veri analitiği ve küme hesaplaması sağlar. Jupyter dizüstü bilgisayarınız verilerinizle etkileşimkurmanızı, kodu işaretleme metniyle birleştirmenizi ve basit görselleştirmeler yapmanızı sağlar.

[Genel Bakış: Azure HDInsight](apache-spark-overview.md) | [Apache Spark](https://spark.apache.org/) | [Apache Hive](https://hive.apache.org/) | [Jupyter Notebook'ta](https://jupyter.org/) Apache Spark

## <a name="prerequisite"></a>Önkoşul

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- [PowerShell Az modülü.](https://docs.microsoft.com/powershell/azure/install-az-ps)

## <a name="create-an-apache-spark-cluster-in-hdinsight"></a>HDInsight'ta Bir Apache Spark kümesi oluşturma

> [!IMPORTANT]  
> İster kullanın, ister kullanmayın, HDInsight kümeleri faturalaması dakika başına eşit olarak dağıtılmıştır. Kullanmayı bitirdikten sonra kümenizi sildiğinizden emin olun. Daha fazla bilgi için bu makalenin [Kaynakları temizleme](#clean-up-resources) bölümüne bakın.

HDInsight kümesi oluşturma işlemi, aşağıdaki Azure nesnelerinin ve kaynaklarının oluşturulmasını kapsar:

- Bir Azure kaynak grubu. Azure kaynak grubu, Azure kaynakları için bir kapsayıcıdır.
- Azure depolama hesabı veya Azure Veri Gölü Depolama.  Her HDInsight kümesi için bağımlı bir veri depolama alanı gerekir. Bu hızlı başlatmada, küme depolama alanı olarak Azure Depolama Blobs kullanan bir küme oluşturursunuz. Data Lake Storage Gen2'yi kullanma hakkında daha fazla bilgi için bkz. [Hızlı başlangıç: HDInsight'ta kümeleri ayarlama](../../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).
- HDInsight'ta farklı küme türlerinin kümesi.  Bu hızlı başlangıçta bir Spark 2.3 kümesi oluşturursunuz.

Kaynakları oluşturmak için bir PowerShell betiği kullanırsınız. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

PowerShell komut dosyasını çalıştırdığınızda, aşağıdaki değerleri girmeniz istenir:

|Parametre|Değer|
|------|------|
|Azure kaynak grubu adı | Kaynak grubu için benzersiz bir ad girin.|
|Konum| Azure bölgesini belirtin (örneğin, 'Orta ABD'). |
|Varsayılan depolama hesabı adı | Depolama hesabına benzersiz bir ad verin. |
|Küme adı | HDInsight kümesi için benzersiz bir ad sağlayın.|
|Küme oturum açma kimlik bilgileri | Hızlı başlangıcın ilerleyen kısmında küme panosuna bağlanmak için bu hesabı kullanırsınız.|
|SSH kullanıcı kimlik bilgileri | SSH istemcileri HDInsight'taki kümelerle uzak bir komut satırı oturumu oluşturmak için kullanılabilir.|

1. [Azure Bulut Uyp'ı](../../cloud-shell/overview.md)açmak için aşağıdaki kod bloğu için sağ üst köşede **Dene'yi** seçin ve ardından Azure'a bağlanmak için yönergeleri izleyin.

2. Bulut Kabuğu'nda aşağıdaki PowerShell komut dosyasını kopyalayıp yapıştırın.

    ```azurepowershell-interactive
    ### Create a Spark 2.3 cluster in Azure HDInsight

    # Default cluster size (# of worker nodes), version, and type
    $clusterSizeInNodes = "1"
    $clusterVersion = "3.6"
    $clusterType = "Spark"

    # Create the resource group
    $resourceGroupName = Read-Host -Prompt "Enter the resource group name"
    $location = Read-Host -Prompt "Enter the Azure region to create resources in, such as 'Central US'"
    $defaultStorageAccountName = Read-Host -Prompt "Enter the default storage account name"

    New-AzResourceGroup -Name $resourceGroupName -Location $location

    # Create an Azure storage account and container
    # Note: Storage account kind BlobStorage can only be used as secondary storage for HDInsight clusters.
    New-AzStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -SkuName Standard_LRS `
        -Kind StorageV2 `
        -EnableHttpsTrafficOnly 1

    $defaultStorageAccountKey = (Get-AzStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value

    $defaultStorageContext = New-AzStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey

    # Create a Spark 2.3 cluster
    $clusterName = Read-Host -Prompt "Enter the name of the HDInsight cluster"

    # Cluster login is used to secure HTTPS services hosted on the cluster
    $httpCredential = Get-Credential -Message "Enter Cluster login credentials" -UserName "admin"

    # SSH user is used to remotely connect to the cluster using SSH clients
    $sshCredentials = Get-Credential -Message "Enter SSH user credentials" -UserName "sshuser"

    # Set the storage container name to the cluster name
    $defaultBlobContainerName = $clusterName

    # Create a blob container. This holds the default data store for the cluster.
    New-AzStorageContainer `
        -Name $clusterName `
        -Context $defaultStorageContext

    $sparkConfig = New-Object "System.Collections.Generic.Dictionary``2[System.String,System.String]"
    $sparkConfig.Add("spark", "2.3")

    # Create the cluster in HDInsight
    New-AzHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName `
        -Location $location `
        -ClusterSizeInNodes $clusterSizeInNodes `
        -ClusterType $clusterType `
        -OSType "Linux" `
        -Version $clusterVersion `
        -ComponentVersion $sparkConfig `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $clusterName `
        -SshCredential $sshCredentials

    Get-AzHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName
    ```

   Kümenin oluşturulması yaklaşık 20 dakika sürer. Sonraki oturumuna devam etmeden önce küme oluşturulması gerekir.

HDInsight kümeleri oluşturma yla ilgili bir sorunla karşılaştıysanız, bunu yapmak için doğru izinlere sahip olmamuş olabilirsiniz. Daha fazla bilgi için bkz. [Erişim denetimi gereksinimleri](../hdinsight-hadoop-customize-cluster-linux.md#access-control).

## <a name="create-a-jupyter-notebook"></a>Jupyter not defteri oluşturma

[Jupyter Notebook,](https://jupyter.org/) çeşitli programlama dillerini destekleyen etkileşimli bir dizüstü bilgisayar ortamıdır. Not defteri, verilerle etkileşim kurmanıza, kodu markdown metniyle birleştirmenize ve basit görselleştirmeler gerçekleştirmenize olanak sağlar.

1. Azure [portalında](https://portal.azure.com) **HDInsight kümelerini**arayın ve seçin.
   
   ![Azure portalında HDInsight kümesini açın](./media/apache-spark-jupyter-spark-sql-use-powershell/azure-portal-search-hdinsight-cluster.png)
   
1. Listeden oluşturduğunuz kümeyi seçin.
   
   ![Azure portalında HDInsight kümesini açın](./media/apache-spark-jupyter-spark-sql-use-powershell/azure-portal-open-hdinsight-cluster.png)
   
1. Kümeye **Genel Bakış** sayfasında, **Küme panoları'nı**seçin ve ardından **Jupyter Notebook'u**seçin. İstendiğinde, küme için küme oturum açma kimlik bilgilerini girin.

   ![Etkileşimli Spark SQL sorgusunu çalıştırmak için Jupyter Notebook'u açın](./media/apache-spark-jupyter-spark-sql-use-powershell/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Etkileşimli Spark SQL sorgusunu çalıştırmak için Jupyter Notebook'u açın")

1. Not defteri oluşturmak için **Yeni** > **PySpark'ı** seçin.

   ![Etkileşimli Spark SQL sorgusunu çalıştırmak için bir Jupyter Notebook oluşturma](./media/apache-spark-jupyter-spark-sql-use-powershell/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png "Etkileşimli Spark SQL sorgusunu çalıştırmak için bir Jupyter Notebook oluşturma")

   Untitled(Untitled.pynb) adıyla yeni bir not defteri oluşturulur ve açılır.

## <a name="run-apache-spark-sql-statements"></a>Apache Spark SQL deyimlerini çalıştırın

SQL (Yapılandırılmış Sorgu Dili), veri sorgulama ve tanımlama için en çok kullanılan dildir. Bilinen SQL söz dizimini kullanan Spark SQL, yapısal verileri işleyen bir Apache Spark uzantısı olarak çalışır.

1. Çekirdeğin hazır olduğunu doğrulayın. Not defterinde çekirdek adının yanında boş bir daire görmeniz, çekirdeğin hazır olduğu anlamına gelir. Dolu daire, çekirdeğin meşgul olduğunu belirtir.

    ![çekirdek durumu](./media/apache-spark-jupyter-spark-sql/jupyter-spark-kernel-status.png "çekirdek durumu")

    Not defterini ilk kez başlattığınızda, çekirdek arka planda birkaç görev gerçekleştirir. Çekirdeğin hazır olmasını bekleyin. 
1. Aşağıdaki kodu boş bir hücreye yapıştırın ve kodu çalıştırmak için **SHIFT + ENTER** tuşlarına basın. Komut, kümedeki Hive tablolarını listeler:

    ```PySpark
    %%sql
    SHOW TABLES
    ```

    HDInsight'ta Spark kümenizi içeren bir Jupyter Notebook kullandığınızda, Spark SQL kullanarak Hive sorgularını çalıştırmak için kullanabileceğiniz bir ön ayar `sqlContext` elde elabilirsiniz. `%%sql`, Hive sorgusunu çalıştırmak için Jupyter Not Defteri’ne `sqlContext` ön ayarını kullanmasını söyler. Sorgu, varsayılan olarak tüm HDInsight kümelerinde sağlanan Hive tablosundaki (**hivesampletable**) ilk 10 satırı getirir. Sonuçları almak 30 saniye kadar sürer. Çıkış aşağıdakine benzer olacaktır:

    ![HDInsight'ta Spark'ta Apache Hive sorgusu](./media/apache-spark-jupyter-spark-sql-use-powershell/hdinsight-spark-get-started-hive-query.png "HDInsight Spark'ta kovan sorgusu")

    Jupyter’de bir sorguyu her çalıştırdığınızda web tarayıcınızın pencere başlığında not defteri başlığı ile birlikte **(Meşgul)** durumu gösterilir. Ayrıca sağ üst köşedeki **PySpark** metninin yanında içi dolu bir daire görürsünüz.

1. `hivesampletable` komutundaki verileri görmek için başka bir sorgu çalıştırın.

    ```PySpark
    %%sql
    SELECT * FROM hivesampletable LIMIT 10
    ```

    Sorgu çıkışının görüntülenmesi için ekranın yenilenmesi gerekir.

    ![HDInsight'ta kovan sorgu çıktısı](./media/apache-spark-jupyter-spark-sql-use-powershell/hdinsight-spark-get-started-hive-query-output.png "HDInsight'ta kovan sorgu çıktısı")

1. Not defterindeki **Dosya** menüsünden **Kapat ve Durdur**’u seçin. Not defterini kapatmak, küme kaynaklarını serbest bırakır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

HDInsight, verilerinizi Azure Depolama’da veya Azure Data Lake Storage’da depolar, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır. [Sonraki adımlar](#next-steps) içinde listelenen öğretici üzerinde hemen çalışmayı planlıyorsanız, kümeyi tutmak isteyebilirsiniz.

Azure portalına geri dönüp **Sil**’i seçin.

![Azure portalı bir HDInsight kümesini silme](./media/apache-spark-jupyter-spark-sql-use-powershell/hdinsight-azure-portal-delete-cluster.png "HDInsight kümesini silme")

Kaynak grubu adını seçerek de kaynak grubu sayfasını açabilir ve sonra **Kaynak grubunu sil**’i seçebilirsiniz. Kaynak grubunu silerek, hem HDInsight kümesini hem de varsayılan depolama hesabını silersiniz.

### <a name="piecemeal-clean-up-with-powershell-az-module"></a>PowerShell Az modülü ile parça parça temizleyin

```powershell
# Removes the specified HDInsight cluster from the current subscription.
Remove-AzHDInsightCluster `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $clusterName

# Removes the specified storage container.
Remove-AzStorageContainer `
    -Name $clusterName `
    -Context $defaultStorageContext

# Removes a Storage account from Azure.
Remove-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $defaultStorageAccountName

# Removes a resource group.
Remove-AzResourceGroup `
    -Name $resourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, HDInsight'ta bir Apache Spark kümesi oluşturmayı ve temel bir Spark SQL sorgusu çalıştırmayı öğrendiniz. Örnek verilerüzerinde etkileşimli sorguları çalıştırmak için HDInsight kümesini nasıl kullanacağınızı öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Apache Spark'ta etkileşimli sorgular çalıştırma](./apache-spark-load-data-run-query.md)
