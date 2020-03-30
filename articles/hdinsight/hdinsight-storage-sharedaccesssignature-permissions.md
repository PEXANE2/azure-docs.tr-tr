---
title: Paylaşılan Erişim İmzalarını kullanarak erişimi kısıtlama - Azure HDInsight
description: HDInsight erişimini Azure depolama lekelerinde depolanan verilere kısıtlamak için Paylaşılan Erişim İmzalarını nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/13/2019
ms.openlocfilehash: 1a4ae0701174278203023c156a86aad8feb1ca4c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240614"
---
# <a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-in-hdinsight"></a>HDInsight ile verilere erişimi kısıtlamak için Azure Depolama Paylaşılan Erişim İmzaları kullanma

HDInsight, kümeyle ilişkili Azure Depolama hesaplarındaki verilere tam erişime sahiptir. Verilere erişimi kısıtlamak için blob kapsayıcıda Paylaşılan Erişim İmzalarını kullanabilirsiniz. Paylaşılan Erişim İmzaları (SAS), Verilere erişimi sınırlamanızı sağlayan Azure depolama hesaplarının bir özelliğidir. Örneğin, verilere salt okunur erişim sağlar.

> [!IMPORTANT]  
> Apache Ranger'ı kullanarak bir çözüm için etki alanına katılan HDInsight'ı kullanmayı düşünün. Daha fazla bilgi için [etki alanına birleştirilmiş HDInsight belgesini yapılandırın.](./domain-joined/apache-domain-joined-configure.md)

> [!WARNING]  
> HDInsight küme için varsayılan depolama tam erişime sahip olmalıdır.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği.

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](./hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

* Varolan [bir depolama kapsayıcısı.](../storage/blobs/storage-quickstart-blobs-portal.md)  

* PowerShell kullanıyorsanız, [Az Modülü](https://docs.microsoft.com/powershell/azure/overview)gerekir.

* Azure CLI'yi kullanmak istiyorsanız ve henüz yüklemediyseniz, Azure [CLI'yi yükleyin'e](https://docs.microsoft.com/cli/azure/install-azure-cli)bakın.

* [Python](https://www.python.org/downloads/)kullanıyorsanız, sürüm 2.7 veya daha yüksek.

* C# kullanıyorsanız, Visual Studio sürüm 2013 veya daha yüksek olmalıdır.

* Depolama hesabınız için [URI şeması.](./hdinsight-hadoop-linux-information.md#URI-and-scheme) Bu, `wasb://` Azure Depolama, `abfs://` Azure Veri Gölü Depolama `adl://` Gen2 veya Azure Veri Gölü Depolama Gen1 için olacaktır. Azure Depolama için güvenli aktarım etkinleştirilirse, URI `wasbs://`. Ayrıca bakınız, [güvenli aktarım.](../storage/common/storage-require-secure-transfer.md)

* Paylaşılan Erişim İmzası eklemek için varolan bir HDInsight kümesi. Değilse, bir küme oluşturmak ve küme oluşturma sırasında Paylaşılan Erişim İmzası eklemek için Azure PowerShell'i kullanabilirsiniz.

* Örnek dosyalar [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Bu depo aşağıdaki öğeleri içerir:

  * HDInsight ile kullanılmak üzere bir depolama kapsayıcısı, depolanan ilke ve SAS oluşturabilen bir Visual Studio projesi
  * HDInsight ile kullanılmak üzere depolama kapsayıcısı, depolanan ilke ve SAS oluşturabilen bir Python komut dosyası
  * BIR HDInsight kümesi oluşturup SAS'ı kullanacak şekilde yapılandırabilen bir PowerShell komut dosyası. Daha sonra aşağıda güncelleştirilmiş bir sürüm kullanılmıştır.
  * Örnek bir dosya:`hdinsight-dotnet-python-azure-storage-shared-access-signature-master\sampledata\sample.log`

## <a name="shared-access-signatures"></a>Paylaşılan Erişim İmzaları

Paylaşılan Erişim İmzalarının iki biçimi vardır:

* Ad hoc: SAS'ın başlangıç saati, son kullanma tarihi ve izinlerinin tümü SAS URI'de belirtilir.

* Depolanan erişim ilkesi: Depolanan erişim ilkesi, bir kaynak kapsayıcısı üzerinde blob kapsayıcısı gibi tanımlanır. Bir veya daha fazla paylaşılan erişim imzasının kısıtlamalarını yönetmek için bir ilke kullanılabilir. Bir SAS'ı depolanmış bir erişim ilkesiyle ilişkilendirdiğinizde, SAS, depolanan erişim ilkesi için tanımlanan kısıtlamaları (başlangıç saati, son kullanma süresi ve izinler) devralır.

İki form arasındaki fark, bir anahtar senaryo için önemlidir: iptal. SAS bir URL'dir, bu nedenle SAS'ı alan herkes, kimden istediği ne olursa olsun, bu url'yi kullanabilir. Bir SAS herkese açık olarak yayınlanırsa, dünyadaki herkes tarafından kullanılabilir. Dağıtılan bir SAS, dört şeyden biri olana kadar geçerlidir:

1. SAS'ta belirtilen son kullanma süresine ulaşılır.

2. SAS tarafından başvurulan depolanan erişim ilkesinde belirtilen son kullanma süresine ulaşılır. Aşağıdaki senaryolar, son kullanma süresine ulaşılmaya neden olur:

    * Zaman aralığı geçti.
    * Depolanan erişim ilkesi geçmişte bir son kullanma süresi olacak şekilde değiştirilir. Son kullanma süresini değiştirmek, SAS'ı iptal etmenin bir yoludur.

3. SAS tarafından başvurulan depolanan erişim ilkesi silinir, bu da SAS'ı iptal etmenin başka bir yoludur. Depolanan erişim ilkesini aynı ada yeniden oluşturursanız, önceki ilkeiçin tüm SAS belirteçleri geçerlidir (SAS'taki son kullanma süresi geçmediyse). SAS'ı iptal etmek istiyorsanız, erişim ilkesini gelecekte bir son kullanma süresiyle yeniden oluşturursanız farklı bir ad kullandığınızdan emin olun.

4. SAS oluşturmak için kullanılan hesap anahtarı yeniden oluşturulur. Anahtarı niçin yeniden oluşturmak, kimlik doğrulamayı başarısız yapmak için önceki anahtarı kullanan tüm uygulamalara neden olur. Tüm bileşenleri yeni anahtara güncelleştirin.

> [!IMPORTANT]  
> Paylaşılan erişim imzası URI, imzayı oluşturmak için kullanılan hesap anahtarı ve ilişkili depolanan erişim ilkesiyle (varsa) ilişkilidir. Depolanmış erişim ilkesi belirtilmemişse, paylaşılan erişim imzasını iptal etmenin tek yolu hesap anahtarını değiştirmektir.

Depolanan erişim ilkelerini her zaman kullanmanızı öneririz. Depolanan ilkeleri kullanırken, imzaları iptal edebilir veya gerektiğinde son kullanma tarihini uzatabilirsiniz. Bu belgedeki adımlar, SAS oluşturmak için depolanan erişim ilkelerini kullanır.

Paylaşılan Erişim İmzaları hakkında daha fazla bilgi için Bkz. [SAS modelini anlama.](../storage/common/storage-dotnet-shared-access-signature-part-1.md)

## <a name="create-a-stored-policy-and-sas"></a>Depolanmış bir ilke ve SAS oluşturma

Her yöntemin sonunda üretilen SAS belirteci kaydedin. Belirteç aşağıdakilere benzer:

```output
?sv=2018-03-28&sr=c&si=myPolicyPS&sig=NAxefF%2BrR2ubjZtyUtuAvLQgt%2FJIN5aHJMj6OsDwyy4%3D
```

### <a name="using-powershell"></a>PowerShell’i kullanma

Mevcut depolama `STORAGECONTAINER` kapsayıcınız için uygun değerleri ve değiştirin. `RESOURCEGROUP` `STORAGEACCOUNT` Parametrenin `hdinsight-dotnet-python-azure-storage-shared-access-signature-master` dizini için mutlak yolu içerecek şekilde değiştirin veya gözden geçirin. `-File` `Set-AzStorageblobcontent` Aşağıdaki PowerShell komutunu girin:

```powershell
$resourceGroupName = "RESOURCEGROUP"
$storageAccountName = "STORAGEACCOUNT"
$containerName = "STORAGECONTAINER"
$policy = "myPolicyPS"

# Login to your Azure subscription
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# If you have multiple subscriptions, set the one to use
# Select-AzSubscription -SubscriptionId "<SUBSCRIPTIONID>"

# Get the access key for the Azure Storage account
$storageAccountKey = (Get-AzStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $storageAccountName)[0].Value

# Create an Azure Storage context
$storageContext = New-AzStorageContext `
                                -StorageAccountName $storageAccountName `
                                -StorageAccountKey $storageAccountKey

# Create a stored access policy for the Azure storage container
New-AzStorageContainerStoredAccessPolicy `
   -Container $containerName `
   -Policy $policy `
   -Permission "rl" `
   -ExpiryTime "12/31/2025 08:00:00" `
   -Context $storageContext

# Get the stored access policy or policies for the Azure storage container
Get-AzStorageContainerStoredAccessPolicy `
    -Container $containerName `
    -Context $storageContext

# Generates an SAS token for the Azure storage container
New-AzStorageContainerSASToken `
    -Name $containerName `
    -Policy $policy `
    -Context $storageContext

<# Removes a stored access policy from the Azure storage container
Remove-AzStorageContainerStoredAccessPolicy `
    -Container $containerName `
    -Policy $policy `
    -Context $storageContext
#>

# upload a file for a later example
Set-AzStorageblobcontent `
    -File "./sampledata/sample.log" `
    -Container $containerName `
    -Blob "samplePS.log" `
    -Context $storageContext
```

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma

Bu bölümdeki değişkenlerin kullanımı bir Windows ortamına dayanır. Bash veya diğer ortamlar için hafif varyasyonlar gerekecektir.

1. Mevcut `STORAGEACCOUNT`depolama `STORAGECONTAINER` kapsayıcınız için uygun değerleri değiştirin.

    ```azurecli
    # set variables
    set AZURE_STORAGE_ACCOUNT=STORAGEACCOUNT
    set AZURE_STORAGE_CONTAINER=STORAGECONTAINER

    #Login
    az login

    # If you have multiple subscriptions, set the one to use
    # az account set --subscription SUBSCRIPTION

    # Retrieve the primary key for the storage account
    az storage account keys list --account-name %AZURE_STORAGE_ACCOUNT% --query "[0].{PrimaryKey:value}" --output table
    ```

2. Alınan birincil anahtarı daha sonra kullanmak üzere bir değişkene ayarlayın. Önceki `PRIMARYKEY` adımda alınan değeri değiştirin ve aşağıdaki komutu girin:

    ```console
    #set variable for primary key
    set AZURE_STORAGE_KEY=PRIMARYKEY
    ```

3. Parametrenin `hdinsight-dotnet-python-azure-storage-shared-access-signature-master` dizini için mutlak yolu içerecek şekilde değiştirin veya gözden geçirin. `--file` `az storage blob upload` Kalan komutları yürütün:

    ```azurecli
    # Create stored access policy on the containing object
    az storage container policy create --container-name %AZURE_STORAGE_CONTAINER% --name myPolicyCLI --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT% --expiry 2025-12-31 --permissions rl

    # List stored access policies on a containing object
    az storage container policy list --container-name %AZURE_STORAGE_CONTAINER% --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT%

    # Generate a shared access signature for the container
    az storage container generate-sas --name myPolicyCLI --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT%

    # Reversal
    # az storage container policy delete --container-name %AZURE_STORAGE_CONTAINER% --name myPolicyCLI --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT%

    # upload a file for a later example
    az storage blob upload --container-name %AZURE_STORAGE_CONTAINER% --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT% --name sampleCLI.log --file "./sampledata/sample.log"
    ```

### <a name="using-python"></a>Python’u kullanma

Dosyayı `SASToken.py` açın `storage_account_name`ve `storage_account_key`varolan depolama kapsayıcınız için uygun `storage_container_name` değerlerle değiştirin ve ardından komut dosyasını çalıştırın.

Hata iletisini `ImportError: No module named azure.storage` `pip install --upgrade azure-storage` alırsanız yürütmeniz gerekebilir.

### <a name="using-c"></a>C# kullanma

1. Çözümü Visual Studio'da açın.

2. Çözüm Gezgini'nde, **SASExample** projesine sağ tıklayın ve **Özellikler'i**seçin.

3. **Ayarlar'ı** seçin ve aşağıdaki girişler için değerler ekleyin:

   * StorageConnectionString: Depolanmış bir ilke ve SAS oluşturmak istediğiniz depolama hesabının bağlantı dizesi. Biçim, depolama `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` `myaccount` hesabınızın adının olduğu ve `mykey` depolama hesabınızın anahtarı nın bulunduğu yerde olmalıdır.

   * ContainerName: Erişimi kısıtlamak istediğiniz depolama hesabındaki kapsayıcı.

   * SASPolicyName: Depolanan ilke oluşturmak için kullanılacak ad.

   * FileToUpload: Kapsayıcıya yüklenen bir dosyaya giden yol.

4. Projeyi çalıştırın. SAS ilke belirteci, depolama hesabı adı ve kapsayıcı adını kaydedin. Bu değerler, depolama hesabını HDInsight kümenizle ilişkilendirirken kullanılır.

## <a name="use-the-sas-with-hdinsight"></a>HDInsight ile SAS'ı kullanın

Bir HDInsight kümesi oluştururken, birincil depolama hesabı belirtmeniz gerekir ve isteğe bağlı olarak ek depolama hesapları belirtebilirsiniz. Depolama ekleme bu yöntemlerin her ikisi de depolama hesapları ve kullanılan kapsayıcılar tam erişim gerektirir.

Kapsayıcıya erişimi sınırlamak için Paylaşılan Erişim İmzası kullanmak için kümeiçin **çekirdek site** yapılandırmasına özel bir giriş ekleyin. Girişi PowerShell kullanarak veya Ambari kullanarak küme oluşturma sonrasında küme oluşturma sırasında ekleyebilirsiniz.

### <a name="create-a-cluster-that-uses-the-sas"></a>SAS kullanan bir küme oluşturma

Değiştir `CLUSTERNAME` `RESOURCEGROUP`, `DEFAULTSTORAGEACCOUNT` `STORAGECONTAINER`, `STORAGEACCOUNT`, `TOKEN` , , ve uygun değerlerle. PowerShell komutlarını girin:

```powershell
$clusterName = 'CLUSTERNAME'
$resourceGroupName = 'RESOURCEGROUP'

# Replace with the Azure data center you want to the cluster to live in
$location = 'eastus'

# Replace with the name of the default storage account TO BE CREATED
$defaultStorageAccountName = 'DEFAULTSTORAGEACCOUNT'

# Replace with the name of the SAS container CREATED EARLIER
$SASContainerName = 'STORAGECONTAINER'

# Replace with the name of the SAS storage account CREATED EARLIER
$SASStorageAccountName = 'STORAGEACCOUNT'

# Replace with the SAS token generated earlier
$SASToken = 'TOKEN'

# Default cluster size (# of worker nodes), version, and type
$clusterSizeInNodes = "4"
$clusterVersion = "3.6"
$clusterType = "Hadoop"

# Login to your Azure subscription
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# If you have multiple subscriptions, set the one to use
# Select-AzSubscription -SubscriptionId "<SUBSCRIPTIONID>"

# Create an Azure Storage account and container
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

# Create a blob container. This holds the default data store for the cluster.
New-AzStorageContainer `
    -Name $clusterName `
    -Context $defaultStorageContext

# Cluster login is used to secure HTTPS services hosted on the cluster
$httpCredential = Get-Credential `
    -Message "Enter Cluster login credentials" `
    -UserName "admin"

# SSH user is used to remotely connect to the cluster using SSH clients
$sshCredential = Get-Credential `
    -Message "Enter SSH user credentials" `
    -UserName "sshuser"

# Create the configuration for the cluster
$config = New-AzHDInsightClusterConfig

$config = $config | Add-AzHDInsightConfigValue `
    -Spark2Defaults @{} `
    -Core @{"fs.azure.sas.$SASContainerName.$SASStorageAccountName.blob.core.windows.net"=$SASToken}

# Create the HDInsight cluster
New-AzHDInsightCluster `
    -Config $config `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $clusterName `
    -Location $location `
    -ClusterSizeInNodes $clusterSizeInNodes `
    -ClusterType $clusterType `
    -OSType Linux `
    -Version $clusterVersion `
    -HttpCredential $httpCredential `
    -SshCredential $sshCredential `
    -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultStorageContainer $clusterName

<# REVERSAL
Remove-AzHDInsightCluster `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $clusterName

Remove-AzStorageContainer `
    -Name $clusterName `
    -Context $defaultStorageContext

Remove-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $defaultStorageAccountName

Remove-AzResourceGroup `
    -Name $resourceGroupName
#>
```

> [!IMPORTANT]  
> HTTP/s veya SSH kullanıcı adı ve şifresi istendiğinde, aşağıdaki ölçütleri karşılayan bir parola sağlamanız gerekir:
>
> * Uzunluğu en az 10 karakter olmalıdır.
> * En az bir basamak içermelidir.
> * Alfasayısal olmayan en az bir karakter içermelidir.
> * En az bir büyük veya küçük harf içermelidir.

Bu komut dosyasının tamamlanması biraz zaman alır, genellikle 15 dakika civarında. Komut dosyası hatasız tamamlandığında, küme oluşturuldu.

### <a name="use-the-sas-with-an-existing-cluster"></a>SAS'ı varolan bir kümeyle kullanma

Varolan bir kümeniz varsa, aşağıdaki adımları kullanarak SAS'yi **çekirdek site** yapılandırmasına ekleyebilirsiniz:

1. Kümeniz için Ambari web UI'sini açın. Bu sayfanın adresi `https://YOURCLUSTERNAME.azurehdinsight.net`. İstendiğinde, küme oluşturulurken kullandığınız yönetici adı (yönetici) ve parolayı kullanarak kümeye kimlik doğrulaması.

1. **HDFS** > **Configs** > **Gelişmiş** > Özel çekirdek**sitesine**gidin.

1. Özel **çekirdek site** bölümünü genişletin, sonuna kadar kaydırın ve ardından **özellik ekle'yi seçin...** **Anahtar** ve **Değer**için aşağıdaki değerleri kullanın:

    * **Anahtar**:`fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net`
    * **Değer**: SAS daha önce yürütülen yöntemlerden biri tarafından döndürülür.

    C# veya SAS uygulaması ile kullandığınız kapsayıcı adı ile değiştirin. `CONTAINERNAME` Kullandığınız `STORAGEACCOUNTNAME` depolama hesabı adı ile değiştirin.

    Bu anahtarı ve değeri kaydetmek için **Ekle'yi** seçin

1. Yapılandırma değişikliklerini kaydetmek için **Kaydet** düğmesini seçin. İstendiğinde, değişikliğin açıklamasını ekleyin (örneğin SAS depolama alanı erişimi ekleme) ve ardından **Kaydet'i**seçin.

    Değişiklikler tamamlandığında **Tamam'ı** seçin.

   > [!IMPORTANT]  
   > Değişiklik yürürlüğe girmeden önce birkaç hizmeti yeniden başlatmanız gerekir.

1. **Yeniden Başlatma** açılır listesi görüntülenir. Açılan listeden **Etkilenen Tümünü Yeniden Başlat'ı** ve Ardından __Tümünü Yeniden Başlat'ı__Onayla'yı seçin.

    **MapReduce2** ve **YARN**için bu işlemi tekrarlayın.

1. Hizmetler yeniden başlatıldıktan sonra, her birini seçin ve **Hizmet Eylemleri'nden** bakım modunu devre dışı bırakın.

## <a name="test-restricted-access"></a>Kısıtlı erişimi test edin

Yalnızca SAS depolama hesabındaki öğeleri okuyup listelebildiğinizi doğrulamak için aşağıdaki adımları kullanın.

1. Kümeye bağlanın. Kümenizin adıyla değiştirin `CLUSTERNAME` ve aşağıdaki komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Kapsayıcının içeriğini listelemek için istekten aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -ls wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    ```

    SAS depolama hesabı için oluşturulan kapsayıcının adı ile değiştirin. `SASCONTAINER` SAS için kullanılan depolama hesabının adı ile değiştirin. `SASACCOUNTNAME`

    Liste, kapsayıcı ve SAS oluşturulduğunda yüklenen dosyayı içerir.

3. Dosyanın içeriğini okuyabildiğinizi doğrulamak için aşağıdaki komutu kullanın. Önceki `SASCONTAINER` adımda olduğu gibi değiştirin. `SASACCOUNTNAME` Önceki `sample.log` komutta görüntülenen dosyanın adı ile değiştirin:

    ```bash
    hdfs dfs -text wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/sample.log
    ```

    Bu komut, dosyanın içeriğini listeler.

4. Dosyayı yerel dosya sistemine indirmek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -get wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/sample.log testfile.txt
    ```

    Bu komut dosyayı **testfile.txt**adlı yerel bir dosyaya indirir.

5. Yerel dosyayı SAS depolama sında **testupload.txt** adlı yeni bir dosyaya yüklemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -put testfile.txt wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    ```

    Aşağıdaki metne benzer bir ileti alırsınız:

        put: java.io.IOException

    Depolama konumu yalnızca okunur+liste olduğundan bu hata oluşur. Verileri küme için varsayılan depolama alanına koymak için aşağıdaki komutu kullanın, bu da yazılabilir:

    ```bash
    hdfs dfs -put testfile.txt wasbs:///testupload.txt
    ```

    Bu kez, işlem başarıyla tamamlanmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight kümenize sınırlı erişimli depolama alanı eklemeyi öğrendiğinize göre, kümenizdeki verilerle çalışmanın başka yollarını da öğrenin:

* [HDInsight ile Apache Hive'ı kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight ile MapReduce'ı kullanın](hadoop/hdinsight-use-mapreduce.md)