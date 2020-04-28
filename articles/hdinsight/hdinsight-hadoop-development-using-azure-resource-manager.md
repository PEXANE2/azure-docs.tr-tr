---
title: HDInsight için Azure Resource Manager araçlarına geçiş
description: HDInsight kümeleri için Azure Resource Manager geliştirme araçlarına geçiş yapma
ms.reviewer: jasonh
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: hrasheed
ms.openlocfilehash: 76eb3a135f7a32a30cfa62546a644bc77cf39998
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75934578"
---
# <a name="migrating-to-azure-resource-manager-based-development-tools-for-hdinsight-clusters"></a>HDInsight kümeleri için Azure Resource Manager tabanlı geliştirme araçlarına geçiş

HDInsight, HDInsight için Azure Service Manager (ASM) tabanlı araçları kullanımdan kaldırır. HDInsight kümeleriyle çalışmak için Azure PowerShell, Azure klasik CLı veya HDInsight .NET SDK kullanıyorsanız, ileri git, PowerShell, CLı ve .NET SDK 'nın Azure Resource Manager sürümlerini kullanmanız önerilir. Bu makalede, yeni Kaynak Yöneticisi tabanlı yaklaşımın nasıl geçirileceğiyle ilgili işaretçiler sunulmaktadır. Bu belgede, uygun olan yerlerde, HDInsight için ASM ve Kaynak Yöneticisi yaklaşımları arasındaki farklılıklar vurgulanmıştır.

> [!IMPORTANT]  
> ASM tabanlı PowerShell, CLı ve .NET SDK desteği **1 ocak 2017**tarihinde sona erecek.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="migrating-azure-classic-cli-to-azure-resource-manager"></a>Azure klasik CLı Azure Resource Manager 'ye geçiriliyor

> [!IMPORTANT]  
> Azure CLı, HDInsight kümeleri ile çalışma desteği sağlamaz. HDInsight ile Azure klasik CLı kullanmaya devam edebilirsiniz, ancak Azure klasik CLı kullanım dışıdır.

Azure klasik CLı aracılığıyla HDInsight ile çalışmaya yönelik temel komutlar aşağıda verilmiştir:

* `azure hdinsight cluster create`-Yeni bir HDInsight kümesi oluşturur
* `azure hdinsight cluster delete`-var olan bir HDInsight kümesini siler
* `azure hdinsight cluster show`-var olan bir kümeyle ilgili bilgileri görüntüle
* `azure hdinsight cluster list`-Azure aboneliğiniz için HDInsight kümelerini listeler

Her komut `-h` için kullanılabilen parametreleri ve anahtarları incelemek için anahtarını kullanın.

### <a name="new-commands"></a>Yeni komutlar
Azure Resource Manager ile kullanılabilen yeni komutlar şunlardır:

* `azure hdinsight cluster resize`-kümedeki çalışan düğümlerinin sayısını dinamik olarak değiştirir
* `azure hdinsight cluster enable-http-access`-kümeye HTTPs erişimi sağlar (varsayılan olarak açık)
* `azure hdinsight cluster disable-http-access`-kümeye HTTPs erişimini devre dışı bırakır
* `azure hdinsight script-action`-bir kümede betik eylemleri oluşturmak/yönetmek için komutlar sağlar
* `azure hdinsight config`-yapılandırma bilgilerini sağlamak için `hdinsight cluster create` komutuyla kullanılabilecek bir yapılandırma dosyası oluşturmaya yönelik komutlar sağlar.

### <a name="deprecated-commands"></a>Kullanım dışı komutlar
HDInsight kümenize iş göndermek `azure hdinsight job` için komutları kullanırsanız, bu komutlar Kaynak Yöneticisi komutları aracılığıyla kullanılamaz. İş dosyalarından iş aracılığıyla HDInsight 'a iş göndermeniz gerekiyorsa, bunun yerine HDInsight tarafından sağlanmış REST API 'Lerini kullanmanız gerekir. REST API 'Leri kullanarak işleri gönderme hakkında daha fazla bilgi için aşağıdaki belgelere bakın.

* [Kıvrımlı kullanarak HDInsight 'ta Hadoop ile MapReduce işleri çalıştırma](hadoop/apache-hadoop-use-mapreduce-curl.md)
* [Apache Hive sorguları, HDInsight kullanarak HDInsight üzerinde Apache Hadoop çalıştırma](hadoop/apache-hadoop-use-hive-curl.md)


MapReduce, Apache Hive ve Apache Pig etkileşimli Apache Hadoop diğer yolları hakkında daha fazla bilgi için bkz. HDInsight 'ta [Hadoop Ile MapReduce kullanma](hadoop/hdinsight-use-mapreduce.md), [HDInsight 'ta Apache Hadoop ile Apache Hive kullanma](hadoop/hdinsight-use-hive.md)ve [HDInsight üzerinde Apache Hadoop ile Apache Pig kullanma](hadoop/hdinsight-use-pig.md).

### <a name="examples"></a>Örnekler
**Küme oluşturma**

* Eski komut (ASM)-`azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`
* Yeni komut-`azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`

**Küme silme**

* Eski komut (ASM)-`azure hdinsight cluster delete myhdicluster`
* Yeni komut-`azure hdinsight cluster delete mycluster -g myresourcegroup`

**Kümeleri Listele**

* Eski komut (ASM)-`azure hdinsight cluster list`
* Yeni komut-`azure hdinsight cluster list`

> [!NOTE]  
> Liste komutu için, kullanarak `-g` kaynak grubunu belirtmek yalnızca belirtilen kaynak grubundaki kümeleri döndürür.

**Küme bilgilerini göster**

* Eski komut (ASM)-`azure hdinsight cluster show myhdicluster`
* Yeni komut-`azure hdinsight cluster show myhdicluster -g myresourcegroup`

## <a name="migrating-azure-powershell-to-azure-resource-manager"></a>Azure PowerShell Azure Resource Manager geçiriliyor
Azure Resource Manager modundaki Azure PowerShell hakkındaki genel bilgiler [Azure Resource Manager ile Azure PowerShell kullanma](../powershell-azure-resource-manager.md)bölümünde bulunabilir.

Azure PowerShell Kaynak Yöneticisi cmdlet 'leri ASM cmdlet 'leri ile yan yana yüklenebilir. İki moddan gelen cmdlet 'ler adlarıyla ayırt edilebilir.  Kaynak Yöneticisi modu, { *AzureHDInsight* ' de ASM modunda karşılaştıran cmdlet adlarında *azhdınsight* 'a sahiptir.  Örneğin, *New-AzHDInsightCluster* vs. *New-AzureHDInsightCluster*. Parametreler ve anahtarlar haber adlarına sahip olabilir ve Kaynak Yöneticisi kullanırken kullanılabilecek birçok yeni parametre vardır.  Örneğin, birkaç cmdlet *-resourcegroupname*adlı yeni bir anahtar gerektirir. 

HDInsight cmdlet 'lerini kullanabilmeniz için önce Azure hesabınıza bağlanmanız ve yeni bir kaynak grubu oluşturmanız gerekir:

* [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount)
* [New-AzResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx)

### <a name="renamed-cmdlets"></a>Yeniden adlandırılan cmdlet 'ler
Windows PowerShell konsolunda HDInsight ASM cmdlet 'lerini listelemek için:

    help *azurehdinsight*

Aşağıdaki tabloda, Kaynak Yöneticisi modunda ASM cmdlet 'leri ve bunların adları listelenmektedir:

| ASM cmdlet 'leri | Kaynak Yöneticisi cmdlet 'leri |
| --- | --- |
| Add-AzureHDInsightConfigValue |[Add-AzHDInsightConfigValue](https://docs.microsoft.com/powershell/module/az.hdinsight/add-azhdinsightconfigvalue) |
| Add-AzureHDInsightMetastore |[Add-AzHDInsightMetastore](https://docs.microsoft.com/powershell/module/az.hdinsight/add-azhdinsightmetastore) |
| Add-AzureHDInsightScriptAction |[Add-AzHDInsightScriptAction](https://docs.microsoft.com/powershell/module/az.hdinsight/add-azhdinsightscriptaction) |
| Add-AzureHDInsightStorage |[Add-AzHDInsightStorage](https://docs.microsoft.com/powershell/module/az.hdinsight/add-azhdinsightstorage) |
| Get-AzureHDInsightCluster |[Get-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsightcluster) |
| Get-AzureHDInsightJob |[Get-AzHDInsightJob](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsightjob) |
| Get-AzureHDInsightJobOutput |[Get-AzHDInsightJobOutput](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsightjoboutput) |
| Get-AzureHDInsightProperty |[Get-AzHDInsightProperty](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsightproperty) |
| Grant-AzureHDInsightHttpServicesAccess |[Grant-AzureRmHDInsightHttpServicesAccess](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/grant-azurermhdinsighthttpservicesaccess) |
| Grant-AzureHdinsightRdpAccess |[Grant-AzHDInsightRdpServicesAccess](https://docs.microsoft.com/powershell/module/az.hdinsight/grant-azhdinsightrdpservicesaccess) |
| Invoke-AzureHDInsightHiveJob |[Invoke-AzHDInsightHiveJob](https://docs.microsoft.com/powershell/module/az.hdinsight/invoke-azhdinsighthivejob) |
| New-AzureHDInsightCluster |[New-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) |
| New-AzureHDInsightClusterConfig |[New-AzHDInsightClusterConfig](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightclusterconfig) |
| New-AzureHDInsightHiveJobDefinition |[New-AzHDInsightHiveJobDefinition](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsighthivejobdefinition) |
| New-AzureHDInsightMapReduceJobDefinition |[New-AzHDInsightMapReduceJobDefinition](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightmapreducejobdefinition) |
| New-AzureHDInsightPigJobDefinition |[New-AzHDInsightPigJobDefinition](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightpigjobdefinition) |
| New-AzureHDInsightSqoopJobDefinition |[New-AzHDInsightSqoopJobDefinition](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightsqoopjobdefinition) |
| New-AzureHDInsightStreamingMapReduceJobDefinition |[New-AzHDInsightStreamingMapReduceJobDefinition](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightstreamingmapreducejobdefinition) |
| Remove-AzureHDInsightCluster |[Remove-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/remove-azhdinsightcluster) |
| Revoke-AzureHDInsightHttpServicesAccess |[Revoke-AzHDInsightHttpServicesAccess](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/revoke-azurermhdinsighthttpservicesaccess) |
| Revoke-AzureHdinsightRdpAccess |[Revoke-AzHDInsightRdpServicesAccess](https://docs.microsoft.com/powershell/module/az.hdinsight/revoke-azhdinsightrdpservicesaccess) |
| Set-AzureHDInsightClusterSize |[Set-AzHDInsightClusterSize](https://docs.microsoft.com/powershell/module/az.hdinsight/set-azhdinsightclustersize) |
| Set-AzureHDInsightDefaultStorage |[Set-AzHDInsightDefaultStorage](https://docs.microsoft.com/powershell/module/az.hdinsight/set-azhdinsightdefaultstorage) |
| Start-AzureHDInsightJob |[Start-AzHDInsightJob](https://docs.microsoft.com/powershell/module/az.hdinsight/start-azhdinsightjob) |
| Stop-AzureHDInsightJob |[Stop-AzHDInsightJob](https://docs.microsoft.com/powershell/module/az.hdinsight/stop-azhdinsightjob) |
| Use-AzureHDInsightCluster |[Use-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/use-azhdinsightcluster) |
| Wait-AzureHDInsightJob |[Wait-AzHDInsightJob](https://docs.microsoft.com/powershell/module/az.hdinsight/wait-azhdinsightjob) |

### <a name="new-cmdlets"></a>Yeni cmdlet’ler
Aşağıda yalnızca Kaynak Yöneticisi modunda kullanılabilen yeni cmdlet 'ler verilmiştir. 

**Betik eylemiyle ilgili cmdlet 'ler:**

* **Get-AzHDInsightPersistedScriptAction**: bir küme için kalıcı betik eylemlerini alır ve bunları kronolojik sırada listeler veya belirtilen kalıcı betik eyleminin ayrıntılarını alır. 
* **Get-AzHDInsightScriptActionHistory**: bir küme için betik eylem geçmişini alır ve bunu ters kronolojik düzende listeler veya daha önce yürütülen bir betik eyleminin ayrıntılarını alır. 
* **Remove-AzHDInsightPersistedScriptAction**: bir HDInsight kümesinden kalıcı betik eylemini kaldırır.
* **Set-AzHDInsightPersistedScriptAction**: daha önce yürütülen bir betik eylemini kalıcı betik eylemi olacak şekilde ayarlar.
* **Gönderme-AzHDInsightScriptAction**: bir Azure HDInsight kümesine yeni bir betik eylemi gönderir. 

Ek kullanım bilgileri için bkz. [betik eylemi kullanarak Linux tabanlı HDInsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md).

**Küme kimliğiyle ilgili cmdlet 'ler:**

* **Add-AzHDInsightClusterIdentity**: hdınsight kümesinin Azure Data Lake Storage erişebilmesi için bir küme yapılandırma nesnesine bir küme kimliği ekler. Bkz. [Azure PowerShell kullanarak Data Lake Storage HDInsight kümesi oluşturma](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).

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

**Küme listesi**

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
* [Apache Hive işleri gönder](hadoop/apache-hadoop-use-hive-powershell.md)
* [Apache Sqoop işleri gönder](hadoop/apache-hadoop-use-sqoop-powershell.md)

## <a name="migrating-to-the-new-hdinsight-net-sdk"></a>Yeni HDInsight .NET SDK 'sına geçiş
Azure hizmet yönetimi tabanlı [(asm) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) artık kullanım dışıdır. Azure Kaynak yönetimi tabanlı [Kaynak Yöneticisi tabanlı HDInsight .NET SDK 'sını](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight)kullanmanız önerilir. Aşağıdaki ASM tabanlı HDInsight paketleri kullanım dışı bırakılıyor.

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`

Bu bölümde Kaynak Yöneticisi tabanlı SDK kullanarak belirli görevleri gerçekleştirme hakkında daha fazla bilgi için işaretçiler sunulmaktadır.

| Nasıl Yapılır... Kaynak Yöneticisi tabanlı HDInsight SDK 'sını kullanma | Bağlantılar |
| --- | --- |
| .NET için Azure HDInsight SDK'sı|Bkz. [.net Için Azure HDıNSIGHT SDK](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight?view=azure-dotnet) |
| .NET SDK ile Azure Active Directory kullanarak etkileşimli uygulamaların kimliğini doğrulama |Bkz. [.NET SDK kullanarak Apache Hive sorguları çalıştırma](hadoop/apache-hadoop-use-hive-dotnet-sdk.md). Bu makaledeki kod parçacığı etkileşimli kimlik doğrulama yaklaşımını kullanır. |
| .NET SDK ile Azure Active Directory kullanarak etkileşimli olmayan uygulamaların kimliğini doğrulama |Bkz. [HDInsight için etkileşimli olmayan uygulamalar oluşturma](hdinsight-create-non-interactive-authentication-dotnet-applications.md) |
| .NET SDK kullanarak Apache Hive işi gönderme |Bkz. [Apache Hive Işleri gönderme](hadoop/apache-hadoop-use-hive-dotnet-sdk.md) |
| .NET SDK kullanarak bir Apache Sqoop işi gönderme |Bkz. [Apache Sqoop Işlerini gönderme](hadoop/apache-hadoop-use-sqoop-dotnet-sdk.md) |
| .NET SDK kullanarak HDInsight kümelerini listeleme |Bkz. [HDInsight kümelerini listeleme](hdinsight-administer-use-dotnet-sdk.md#list-clusters) |
| .NET SDK kullanarak HDInsight kümelerini ölçeklendirme |Bkz. [HDInsight kümelerini ölçeklendirme](hdinsight-administer-use-dotnet-sdk.md#scale-clusters) |
| .NET SDK kullanarak HDInsight kümelerine erişim verme/iptal etme |Bkz. [HDInsight kümelerine erişim verme/iptal etme](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access) |
| .NET SDK kullanarak HDInsight kümeleri için HTTP Kullanıcı kimlik bilgilerini güncelleştirme |Bkz. [HDInsight kümeleri IÇIN http Kullanıcı kimlik bilgilerini güncelleştirme](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials) |
| .NET SDK kullanarak HDInsight kümeleri için varsayılan depolama hesabını bulma |Bkz [. HDInsight kümeleri için varsayılan depolama hesabını bulma](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account) |
| .NET SDK kullanarak HDInsight kümelerini silme |Bkz. [HDInsight kümelerini silme](hdinsight-administer-use-dotnet-sdk.md#delete-clusters) |

### <a name="examples"></a>Örnekler
Aşağıda, bir işlemin, ASM tabanlı SDK ve Kaynak Yöneticisi tabanlı SDK için eşdeğer kod parçacığı kullanarak nasıl gerçekleştirildiği hakkında bazı örnekler verilmiştir.

**Küme CRUD istemcisi oluşturma**

* Eski komut (ASM)
  
        //Certificate auth
        //This logs the application in using a subscription administration certificate, which is not offered in Azure Resource Manager
  
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);
* Yeni komut (hizmet sorumlusu yetkilendirmesi)
  
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
* Yeni komut (kullanıcı yetkilendirmesi)
  
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

