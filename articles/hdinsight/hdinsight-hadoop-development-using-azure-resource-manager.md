---
title: HDInsight için Azure Kaynak Yöneticisi araçlarına geçiş yapın
description: HDInsight kümeleri için Azure Kaynak Yöneticisi geliştirme araçlarına geçiş nasıl yapılır?
ms.reviewer: jasonh
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: hrasheed
ms.openlocfilehash: 76eb3a135f7a32a30cfa62546a644bc77cf39998
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934578"
---
# <a name="migrating-to-azure-resource-manager-based-development-tools-for-hdinsight-clusters"></a>HDInsight kümeleri için Azure Kaynak Yöneticisi tabanlı geliştirme araçlarına geçiş

HDInsight, HDInsight için Azure Servis Yöneticisi (ASM) tabanlı araçları küçümsüyor. HDInsight kümeleriyle çalışmak için Azure PowerShell, Azure Classic CLI veya HDInsight .NET SDK kullanıyorsanız, ileriye dönük olarak PowerShell, CLI ve .NET SDK'nın Azure Kaynak Yöneticisi sürümlerini kullanmanız için size tavsiye edilir. Bu makalede, yeni Kaynak Yöneticisi tabanlı yaklaşıma nasıl geçirilir hakkında işaretçiler sağlar. Bu belge, uygulananene ne olursa olsun, HDInsight için ASM ve Kaynak Yöneticisi yaklaşımları arasındaki farkları vurgular.

> [!IMPORTANT]  
> ASM tabanlı PowerShell, CLI ve .NET SDK desteği **1 Ocak 2017**tarihinde sona erecektir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="migrating-azure-classic-cli-to-azure-resource-manager"></a>Azure Klasik CLI'yi Azure Kaynak Yöneticisine Geçirme

> [!IMPORTANT]  
> Azure CLI, HDInsight kümeleriyle çalışmak için destek sağlamaz. HDInsight ile Azure Classic CLI'yi kullanmaya devam edebilirsiniz, ancak Azure Classic CLI amortismana hazırdır.

Azure CLassic CLI aracılığıyla HDInsight ile çalışmak için temel komutlar şunlardır:

* `azure hdinsight cluster create`- yeni bir HDInsight kümesi oluşturur
* `azure hdinsight cluster delete`- varolan bir HDInsight kümesini siler
* `azure hdinsight cluster show`- varolan bir küme yle ilgili bilgileri görüntüleme
* `azure hdinsight cluster list`- Azure aboneliğiniz için HDInsight kümelerini listeler

Her `-h` komut için mevcut parametreleri ve anahtarları incelemek için anahtarı kullanın.

### <a name="new-commands"></a>Yeni komutlar
Azure Kaynak Yöneticisi ile kullanılabilen yeni komutlar şunlardır:

* `azure hdinsight cluster resize`- kümedeki alt düğüm sayısını dinamik olarak değiştirir
* `azure hdinsight cluster enable-http-access`- HTTP'lerin kümeye erişimini sağlar (varsayılan olarak)
* `azure hdinsight cluster disable-http-access`- KÜMEYE HTTP erişimini devre dışı
* `azure hdinsight script-action`- bir kümede Komut Dosyası Eylemleri oluşturma/yönetme komutları sağlar
* `azure hdinsight config`- yapılandırma bilgileri sağlamak için `hdinsight cluster create` komutu ile kullanılabilecek bir yapılandırma dosyası oluşturmak için komutlar sağlar.

### <a name="deprecated-commands"></a>Amortismana karşı verilen komutlar
HdInsight kümenize iş göndermek için `azure hdinsight job` komutları kullanırsanız, bu komutlar Kaynak Yöneticisi komutları aracılığıyla kullanılamaz. Komut dosyalarından HDInsight'a iş göndermeniz gerekiyorsa, bunun yerine HDInsight tarafından sağlanan REST API'lerini kullanmanız gerekir. REST API'lerini kullanarak iş gönderme hakkında daha fazla bilgi için aşağıdaki belgelere bakın.

* [CURL kullanarak HDInsight'ta Hadoop ile MapReduce işlerini çalıştırın](hadoop/apache-hadoop-use-mapreduce-curl.md)
* [CURL kullanarak HDInsight'ta Apache Hadoop ile Apache Hive sorgularını çalıştırın](hadoop/apache-hadoop-use-hive-curl.md)


Apache Hadoop MapReduce, Apache Hive ve Apache Pig'i etkileşimli olarak çalıştırmanın diğer yolları hakkında bilgi için, [HDInsight'ta Hadoop ile MapReduce'ı kullanın](hadoop/hdinsight-use-mapreduce.md), [HDInsight'ta Apache Hadoop ile Apache Hive kullanın](hadoop/hdinsight-use-hive.md)ve [HDInsight'ta Apache Hadoop ile Apache Pig kullanın.](hadoop/hdinsight-use-pig.md)

### <a name="examples"></a>Örnekler
**Küme oluşturma**

* Eski komut (ASM) -`azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`
* Yeni komut -`azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`

**Küme silme**

* Eski komut (ASM) -`azure hdinsight cluster delete myhdicluster`
* Yeni komut -`azure hdinsight cluster delete mycluster -g myresourcegroup`

**Liste kümeleri**

* Eski komut (ASM) -`azure hdinsight cluster list`
* Yeni komut -`azure hdinsight cluster list`

> [!NOTE]  
> Liste komutu için, kaynak grubunu `-g` kullanan belirtilmesi yalnızca belirtilen kaynak grubundaki kümeleri döndürecektir.

**Küme bilgilerini göster**

* Eski komut (ASM) -`azure hdinsight cluster show myhdicluster`
* Yeni komut -`azure hdinsight cluster show myhdicluster -g myresourcegroup`

## <a name="migrating-azure-powershell-to-azure-resource-manager"></a>Azure PowerShell'i Azure Kaynak Yöneticisine Geçirme
Azure Kaynak Yöneticisi modundaAzure PowerShell hakkındaki genel bilgileri [Azure Kaynak Yöneticisi ile Azure PowerShell'i Kullanma'da](../powershell-azure-resource-manager.md)bulabilirsiniz.

Azure PowerShell Kaynak Yöneticisi cmdlets ASM cmdlets ile yan yana takılabilir. İki moddaki cmdletler adlarıyla ayırt edilebilir.  Kaynak Yöneticisi modunda, ASM modunda *AzureHDInsight* ile karşılaştıran cmdlet adlarında *AzHDInsight* vardır.  Örneğin, *New-AzHDInsightCluster* vs. *New-AzureHDInsightCluster*. Parametreler ve anahtarlar haber adları olabilir ve Kaynak Yöneticisi kullanırken birçok yeni parametre kullanılabilir.  Örneğin, birkaç cmdlets *-ResourceGroupName*adlı yeni bir anahtar gerektirir. 

HDInsight cmdlets'i kullanabilmek için önce Azure hesabınıza bağlanmanız ve yeni bir kaynak grubu oluşturmanız gerekir:

* [Bağlan-AzHesap](/powershell/module/az.accounts/connect-azaccount)
* [New-AzResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx)

### <a name="renamed-cmdlets"></a>Yeniden adlandırılmış cmdlets
Windows PowerShell konsolundaki HDInsight ASM cmdlets'i listelemek için:

    help *azurehdinsight*

Aşağıdaki tabloda ASM cmdlets ve adlarını Kaynak Yöneticisi modunda listeler:

| ASM cmdlets | Kaynak Yöneticisi cmdlets |
| --- | --- |
| Ekle-AzureHDInsightConfigValue |[Ekle-AzHDInsightConfigValue](https://docs.microsoft.com/powershell/module/az.hdinsight/add-azhdinsightconfigvalue) |
| Add-AzureHDInsightMetastore |[Ekle-AzHDInsightMetastore](https://docs.microsoft.com/powershell/module/az.hdinsight/add-azhdinsightmetastore) |
| Add-AzureHDInsightScriptAction |[Ekle-AzHDInsightScriptAction](https://docs.microsoft.com/powershell/module/az.hdinsight/add-azhdinsightscriptaction) |
| Add-AzureHDInsightStorage |[Ekle-AzHDInsightStorage](https://docs.microsoft.com/powershell/module/az.hdinsight/add-azhdinsightstorage) |
| Get-AzureHDInsightCluster |[Al-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsightcluster) |
| Get-AzureHDInsightJob |[Get-AzHDInsightJob](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsightjob) |
| Get-AzureHDInsightJobOutput |[Get-AzHDInsightJobOutput](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsightjoboutput) |
| AzureHDInsightÖzelliği edinin |[Get-AzHDInsightProperty](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsightproperty) |
| Grant-AzureHDInsightHttpServicesAccess |[Grant-AzureRmHDInsightHttpServicesAccess](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/grant-azurermhdinsighthttpservicesaccess) |
| Grant-AzureHdinsightRdpAccess |[Grant-AzHDInsightRdpServicesAccess](https://docs.microsoft.com/powershell/module/az.hdinsight/grant-azhdinsightrdpservicesaccess) |
| Invoke-AzureHDInsightHiveJob |[Invoke-AzHDInsightHiveJob](https://docs.microsoft.com/powershell/module/az.hdinsight/invoke-azhdinsighthivejob) |
| New-AzureHDInsightCluster |[Yeni-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) |
| New-AzureHDInsightClusterConfig |[Yeni-AzHDInsightClusterConfig](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightclusterconfig) |
| New-AzureHDInsightHiveJobDefinition |[Yeni-AzHDInsightHiveJobDefinition](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsighthivejobdefinition) |
| New-AzureHDInsightMapReduceJobDefinition |[Yeni-AzHDInsightMapReduceJobDefinition](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightmapreducejobdefinition) |
| New-AzureHDInsightPigJobDefinition |[Yeni-AzHDInsightPigJobDefinition](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightpigjobdefinition) |
| New-AzureHDInsightSqoopJobDefinition |[Yeni-AzHDInsightSqoopJobDefinition](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightsqoopjobdefinition) |
| New-AzureHDInsightStreamingMapReduceJobDefinition |[Yeni-AzHDInsightStreamingMapReduceJobDefinition](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightstreamingmapreducejobdefinition) |
| Remove-AzureHDInsightCluster |[Remove-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/remove-azhdinsightcluster) |
| Revoke-AzureHDInsightHttpServicesAccess |[Revoke-AzHDInsightHttpServicesAccess](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/revoke-azurermhdinsighthttpservicesaccess) |
| Revoke-AzureHdinsightRdpAccess |[Revoke-AzHDInsightRdpServicesAccess](https://docs.microsoft.com/powershell/module/az.hdinsight/revoke-azhdinsightrdpservicesaccess) |
| Set-AzureHDInsightClusterSize |[Set-AzHDInsightClusterSize](https://docs.microsoft.com/powershell/module/az.hdinsight/set-azhdinsightclustersize) |
| Set-AzureHDInsightDefaultStorage |[Set-AzHDInsightDefaultStorage](https://docs.microsoft.com/powershell/module/az.hdinsight/set-azhdinsightdefaultstorage) |
| Start-AzureHDInsightJob |[Başlangıç-AzHDInsightJob](https://docs.microsoft.com/powershell/module/az.hdinsight/start-azhdinsightjob) |
| Stop-AzureHDInsightJob |[Stop-AzHDInsightJob](https://docs.microsoft.com/powershell/module/az.hdinsight/stop-azhdinsightjob) |
| Use-AzureHDInsightCluster |[Kullanım-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/use-azhdinsightcluster) |
| Wait-AzureHDInsightJob |[Bekleyin-AzHDInsightJob](https://docs.microsoft.com/powershell/module/az.hdinsight/wait-azhdinsightjob) |

### <a name="new-cmdlets"></a>Yeni cmdlet’ler
Aşağıda yalnızca Kaynak Yöneticisi modunda kullanılabilen yeni cmdletler vebu yenilikler ve bu cmdletler vebu yenilikler ve bu cmdletler vebu yenilikler yer almaktadır. 

**Script eylem ile ilgili cmdlets:**

* **Get-AzHDInsightPersistedScriptAction**: Bir küme için kalıcı komut dosyası eylemlerini alır ve bunları kronolojik sırada listeler veya belirli bir kalıcı komut dosyası eylemi için ayrıntıları alır. 
* **Get-AzHDInsightScriptActionHistory**: Bir kümenin komut dosyası eylem geçmişini alır ve ters kronolojik sırada listeler veya daha önce yürütülmüş bir komut dosyası eyleminin ayrıntılarını alır. 
* **Remove-AzHDInsightPersistedScriptAction**: Bir HDInsight kümesinden kalıcı bir komut dosyası eylemini kaldırır.
* **Set-AzHDInsightPersistedScriptAction**: Daha önce yürütülmüş bir komut dosyası eylemini kalıcı bir komut dosyası eylemi olarak ayarlar.
* **Gönder-AzHDInsightScriptAction**: Bir Azure HDInsight kümesine yeni bir komut dosyası eylemi gönderir. 

Ek kullanım bilgileri için Bkz. [Komut Dosyası Eylemi'ni kullanarak Linux tabanlı HDInsight kümelerini özelleştirin.](hdinsight-hadoop-customize-cluster-linux.md)

**Küme kimliği ile ilgili cmdlets:**

* **Add-AzHDInsightClusterIdentity**: HDInsight kümesinin Azure Veri Gölü Depolamasına erişebilmeleri için küme yapılandırma nesnesine küme kimliği ekler. Bkz. [Azure PowerShell kullanarak Veri Gölü Depolama ile hdinsight kümesi oluşturun.](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)

### <a name="examples"></a>Örnekler
**Küme oluşturma**

Eski komut (ASM): 

    New-AzureHDInsightCluster `
        -Name $clusterName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainerName $containerName `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -Credential $httpCredential `
        -SshCredential $sshCredential

Yeni komut:

    New-AzHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainer $containerName  `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -HttpCredential $httpcredentials `
        -SshCredential $sshCredentials


**Kümeyi silme**

Eski komut (ASM):

    Remove-AzureHDInsightCluster -name $clusterName 

Yeni komut:

    Remove-AzHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName 

**Liste kümesi**

Eski komut (ASM):

    Get-AzureHDInsightCluster

Yeni komut:

    Get-AzHDInsightCluster 

**Kümeyi göster**

Eski komut (ASM):

    Get-AzureHDInsightCluster -Name $clusterName

Yeni komut:

    Get-AzHDInsightCluster -ResourceGroupName $resourceGroupName -clusterName $clusterName


#### <a name="other-samples"></a>Diğer örnekler
* [HDInsight kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [Apache Hive işlerini gönder](hadoop/apache-hadoop-use-hive-powershell.md)
* [Apache Sqoop işleri gönder](hadoop/apache-hadoop-use-sqoop-powershell.md)

## <a name="migrating-to-the-new-hdinsight-net-sdk"></a>Yeni HDInsight .NET SDK'ya geçiş
Azure Hizmet Yönetimi tabanlı [(ASM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) artık amortismana hazır. Azure Kaynak Yönetimi tabanlı Kaynak Yöneticisi [tabanlı HDInsight .NET SDK'yı](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight)kullanmanız tavsiye edilir. Aşağıdaki ASM tabanlı HDInsight paketleri küçümsenmektedir.

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`

Bu bölümde, Kaynak Yöneticisi tabanlı SDK'yı kullanarak belirli görevlerin nasıl gerçekleştirileceğinden ilgili daha fazla bilgi için işaretçiler sağlanmıştır.

| Nasıl Yapılır... Kaynak Yöneticisi tabanlı HDInsight SDK'yı kullanma | Bağlantılar |
| --- | --- |
| .NET için Azure HDInsight SDK'sı|[.NET için Azure HDInsight SDK'ya](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight?view=azure-dotnet) bakın |
| .NET SDK ile Azure Active Directory kullanarak uygulamaları etkileşimli olarak doğrula |Bkz. [.NET SDK'yı kullanarak Apache Hive sorgularını çalıştırın.](hadoop/apache-hadoop-use-hive-dotnet-sdk.md) Bu makaledeki kod parçacığı etkileşimli kimlik doğrulama yaklaşımını kullanır. |
| .NET SDK ile Azure Active Directory kullanarak uygulamaların kimliğini nitalsiz olarak doğrula |Bkz. [HDInsight için etkileşimli olmayan uygulamalar oluşturun](hdinsight-create-non-interactive-authentication-dotnet-applications.md) |
| .NET SDK'yı kullanarak Apache Hive işi gönderme |Bkz. [Apache Hive İşlerini gönder](hadoop/apache-hadoop-use-hive-dotnet-sdk.md) |
| .NET SDK kullanarak bir Apache Sqoop iş gönderin |Bkz. [Apache Sqoop işlerini Gönder](hadoop/apache-hadoop-use-sqoop-dotnet-sdk.md) |
| .NET SDK kullanarak HDInsight kümelerini listele |Bkz. [HDInsight kümeleri listesi](hdinsight-administer-use-dotnet-sdk.md#list-clusters) |
| .NET SDK kullanarak HDInsight kümelerini ölçeklendirin |[Bkz. ÖLÇEK HDInsight kümeleri](hdinsight-administer-use-dotnet-sdk.md#scale-clusters) |
| .NET SDK'yı kullanarak HDInsight kümelerine erişimi verme/iptal etme |Bkz. [HDInsight kümelerine erişim izni verme/iptal etme](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access) |
| .NET SDK kullanarak HDInsight kümeleri için HTTP kullanıcı kimlik bilgilerini güncelleştirin |[HDInsight kümeleri için HTTP kullanıcı kimlik bilgilerini güncelleştir](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials) |
| .NET SDK'yı kullanarak HDInsight kümeleri için varsayılan depolama hesabını bulun |Bkz. [HDInsight kümeleri için varsayılan depolama hesabını bul](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account) |
| .NET SDK kullanarak HDInsight kümelerini silme |Bkz. [HDInsight kümelerini sil](hdinsight-administer-use-dotnet-sdk.md#delete-clusters) |

### <a name="examples"></a>Örnekler
Aşağıda, ASM tabanlı SDK ve Kaynak Yöneticisi tabanlı SDK'nın eşdeğer kod parçacığı kullanılarak bir işlemin nasıl gerçekleştirildiği ile ilgili bazı örnekler verilmiştir.

**Küme CRUD istemcisi oluşturma**

* Eski komut (ASM)
  
        //Certificate auth
        //This logs the application in using a subscription administration certificate, which is not offered in Azure Resource Manager
  
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);
* Yeni komut (Hizmet ana yetkisi)
  
        //Service principal auth
        //This will log the application in as itself, rather than on behalf of a specific user.
        //For details, including how to set up the application, see:
        //   https://azure.microsoft.com/documentation/articles/hdinsight-create-non-interactive-authentication-dotnet-applications/
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);
* Yeni komut (Kullanıcı yetkilendirmesi)
  
        //User auth
        //This will log the application in on behalf of the user.
        //The end-user will see a login popup.
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.User, Id = username };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);

**Küme oluşturma**

* Eski komut (ASM)
  
        var clusterInfo = new ClusterCreateParameters
                    {
                        Name = dnsName,
                        DefaultStorageAccountKey = key,
                        DefaultStorageContainer = defaultStorageContainer,
                        DefaultStorageAccountName = storageAccountDnsName,
                        ClusterSizeInNodes = 1,
                        ClusterType = type,
                        Location = "West US",
                        UserName = "admin",
                        Password = "*******",
                        Version = version,
                        HeadNodeSize = NodeVMSize.Large,
                    };
        clusterInfo.CoreConfiguration.Add(new KeyValuePair<string, string>("config1", "value1"));
        client.CreateCluster(clusterInfo);
* Yeni komut
  
        var clusterCreateParameters = new ClusterCreateParameters
            {
                Location = "West US",
                ClusterType = "Hadoop",
                Version = "3.1",
                OSType = OSType.Linux,
                DefaultStorageAccountName = "mystorage.blob.core.windows.net",
                DefaultStorageAccountKey =
                    "O9EQvp3A3AjXq/W27rst1GQfLllhp0gUeiUUn2D8zX2lU3taiXSSfqkZlcPv+nQcYUxYw==",
                UserName = "hadoopuser",
                Password = "*******",
                HeadNodeSize = "ExtraLarge",
                RdpUsername = "hdirp",
                RdpPassword = ""*******",
                RdpAccessExpiry = new DateTime(2025, 3, 1),
                ClusterSizeInNodes = 5
            };
        var coreConfigs = new Dictionary<string, string> {{"config1", "value1"}};
        clusterCreateParameters.Configurations.Add(ConfigurationKey.CoreSite, coreConfigs);

**HTTP erişimini etkinleştirme**

* Eski komut (ASM)
  
        client.EnableHttp(dnsName, "West US", "admin", "*******");
* Yeni komut
  
        var httpParams = new HttpSettingsParameters
        {
               HttpUserEnabled = true,
               HttpUsername = "admin",
               HttpPassword = "*******",
        };
        client.Clusters.ConfigureHttpSettings(resourceGroup, dnsname, httpParams);

**Küme silme**

* Eski komut (ASM)
  
        client.DeleteCluster(dnsName);
* Yeni komut
  
        client.Clusters.Delete(resourceGroup, dnsname);

