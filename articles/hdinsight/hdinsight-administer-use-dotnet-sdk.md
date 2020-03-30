---
title: .NET SDK ile HDInsight'ta Apache Hadoop kümelerini yönetme - Azure
description: HDInsight .NET SDK'yı kullanarak HDInsight'taki Apache Hadoop kümeleri için yönetim görevlerini nasıl gerçekleştireceğinizi öğrenin.
ms.reviewer: jasonh
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: hrasheed
ms.openlocfilehash: 36a77d49b507d3d0158d1b4b492d0141350de50f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240630"
---
# <a name="manage-apache-hadoop-clusters-in-hdinsight-by-using-net-sdk"></a>.NET SDK'sını kullanarak HDInsight'taki Apache Hadoop kümelerini yönetme

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

[HDInsight.NET SDK'yı](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight)kullanarak HDInsight kümelerini nasıl yönetebilirsiniz öğrenin.

**Ön koşullar**

Bu makaleye başlamadan önce aşağıdakilere sahip olmanız ve aşağıdaki işlemleri yapmış olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="connect-to-azure-hdinsight"></a>Azure HDInsight'a bağlanın

Aşağıdaki NuGet paketlerine ihtiyacınız var:

```powershell
Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
Install-Package Microsoft.Azure.Management.ResourceManager -Pre
Install-Package Microsoft.Azure.Management.HDInsight
```

Aşağıdaki kod örneği, Azure aboneliğiniz altında HDInsight kümelerini yönetebilmeniz için Azure'a nasıl bağlanabileceğinizi gösterir.

```csharp
using System;
using Microsoft.Azure;
using Microsoft.Azure.Management.HDInsight;
using Microsoft.Azure.Management.HDInsight.Models;
using Microsoft.Azure.Management.ResourceManager;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Rest;
using Microsoft.Rest.Azure.Authentication;

namespace HDInsightManagement
{
    class Program
    {
        private static HDInsightManagementClient _hdiManagementClient;
        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId; 
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";

        static void Main(string[] args)
        {
            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            // insert code here

            System.Console.WriteLine("Press ENTER to continue");
            System.Console.ReadLine();
        }

        /// <summary>
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
        {
            var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
            var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/", 
                ClientId, 
                new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                PromptBehavior.Always, 
                UserIdentifier.AnyUser);
            return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
        }
        /// <summary>
        /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
        /// </summary>
        /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
        /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
        /// <param name="authToken">An authentication token for your Azure subscription</param>
        static void EnableHDInsight(TokenCloudCredentials authToken)
        {
            // Create a client for the Resource manager and set the subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register the HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }
    }
}
```

Bu programı çalıştırdığınızda bir istem görürsünüz.  İstemi görmek istemiyorsanız, [etkileşimli olmayan kimlik doğrulama .NET HDInsight uygulamaları oluştur'a](hdinsight-create-non-interactive-authentication-dotnet-applications.md)bakın.


## <a name="list-clusters"></a>Liste kümeleri

Aşağıdaki kod snippet listeler kümeleri ve bazı özellikleri:

```csharp
var results = _hdiManagementClient.Clusters.List();
foreach (var name in results.Clusters) {
    Console.WriteLine("Cluster Name: " + name.Name);
    Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
    Console.WriteLine("\t Cluster location: " + name.Location);
    Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
}
```

## <a name="delete-clusters"></a>Kümeleri silme

Bir kümeyi eşzamanlı veya eşzamanlı olarak silmek için aşağıdaki kod parçacıklarını kullanın: 

```csharp
_hdiManagementClient.Clusters.Delete("<Resource Group Name>", "<Cluster Name>");
_hdiManagementClient.Clusters.DeleteAsync("<Resource Group Name>", "<Cluster Name>");
```

## <a name="scale-clusters"></a>Ölçek kümeleri

Küme ölçekleme özelliği, kümeyi yeniden oluşturmak zorunda kalmadan Azure HDInsight'ta çalışan bir küme tarafından kullanılan alt düğüm sayısını değiştirmenize olanak tanır.

> [!NOTE]  
> Yalnızca HDInsight sürüm 3.1.3 veya üzeri olan kümeler desteklenir. Kümenizin sürümünden emin değilseniz, Özellikler sayfasını kontrol edebilirsiniz.  Bkz. [Liste ve gösteri kümeleri.](hdinsight-administer-use-portal-linux.md#showClusters)

HDInsight tarafından desteklenen her küme türü için veri düğümü sayısını değiştirmenin etkisi:

* Apache Hadoop
  
    Bekleyen veya çalışan işleri etkilemeden çalışan bir Hadoop kümesindeki işçi düğümlerinin sayısını sorunsuz bir şekilde artırabilirsiniz. Operasyon devam ederken yeni işler de sunulabilir. Ölçeklendirme işlemindeki hatalar, kümenin her zaman işlevsel bir durumda kalması için incelikle işlenir.
  
    Bir Hadoop kümesi veri düğümlerinin sayısını azaltarak küçültüldüğünde, kümedeki bazı hizmetler yeniden başlatılır. Bu, tüm çalışan ve bekleyen işlerin ölçekleme işlemi tamamlandığında başarısız lığa neden olur. Ancak, işlem tamamlandıktan sonra işleri yeniden gönderebilirsiniz.
* Apache HBase
  
    Çalışırken HBase kümenize sorunsuz bir şekilde düğüm ekleyebilir veya kaldırabilirsiniz. Bölgesel Sunucular, ölçekleme işlemini tamamladıktan sonra birkaç dakika içinde otomatik olarak dengelenir. Ancak, kümenin headnode oturum açarak ve bir komut istemi penceresinden aşağıdaki komutları çalıştırarak bölgesel sunucuları el ile dengeleyebilirsiniz:
  

    ```bash
    >pushd %HBASE_HOME%\bin
    >hbase shell
    >balancer
    ```

* Apache Storm
  
    Çalışırken Fırtına kümenize sorunsuz bir şekilde veri düğümleri ekleyebilir veya kaldırabilirsiniz. Ancak ölçekleme işleminin başarıyla tamamlanmasından sonra, topolojiyi yeniden dengelemeniz gerekir.
  
    Yeniden dengeleme iki şekilde gerçekleştirilebilir:
  
  * Fırtına web UI
  * Komut satırı arabirimi (CLI) aracı
    
    Daha fazla bilgi için lütfen [Apache Storm belgelerine](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) bakın.
    
    Storm web UI HDInsight kümesinde kullanılabilir:
    
    ![HDInsight Fırtına ölçeği yeniden dengeleme](./media/hdinsight-administer-use-powershell/hdinsight-portal-scale-cluster-storm-rebalance.png)
    
    Fırtına topolojisini yeniden dengelemek için CLI komutunu nasıl kullanacağına bir örnek aşağıda verilmiştir:
    

    ```console
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

Aşağıdaki kod snippet eşzamanlı veya eşzamanlı bir küme yeniden boyutlandırmak için nasıl gösterir:

```csharp
_hdiManagementClient.Clusters.Resize("<Resource Group Name>", "<Cluster Name>", <New Size>);   
_hdiManagementClient.Clusters.ResizeAsync("<Resource Group Name>", "<Cluster Name>", <New Size>);   
```

## <a name="grantrevoke-access"></a>Erişimi verme/iptal et

HDInsight kümeleri aşağıdaki HTTP web hizmetlerine sahiptir (bu hizmetlerin tümü nde restful uç noktaları vardır):

* ODBC
* JDBC
* Apaçi Ambari
* Apaçi Oozie
* Apaçi Tapınağı

Varsayılan olarak, bu hizmetlererişim için verilir. Erişimi iptal edebilir/verebilirsiniz. İptal etmek için:

```csharp
var httpParams = new HttpSettingsParameters
{
    HttpUserEnabled = false,
    HttpUsername = "admin",
    HttpPassword = "*******",
};
_hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);
```

Vermek için:

```csharp
var httpParams = new HttpSettingsParameters
{
    HttpUserEnabled = enable,
    HttpUsername = "admin",
    HttpPassword = "*******",
};
_hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);
```

> [!NOTE]  
> Erişimi vererek/iptal ederek küme kullanıcı adını ve parolasını sıfırlarsınız.

Bu işlem Portal üzerinden de yapılabilir. [Azure portalını kullanarak HDInsight'ta Apache Hadoop kümelerini yönet'e](hdinsight-administer-use-portal-linux.md)bakın.

## <a name="update-http-user-credentials"></a>HTTP kullanıcı kimlik bilgilerini güncelleştirme

Bu, HTTP erişimini Hibe/iptal etmekle aynı yordamdır.  Kümeye HTTP erişimi verilmişse, önce kümeyi iptal etmelisiniz.  Ve sonra yeni HTTP kullanıcı kimlik bilgileri ile erişim vermek.

## <a name="find-the-default-storage-account"></a>Varsayılan depolama hesabını bulma

Aşağıdaki kod snippet varsayılan depolama hesabı adı ve bir küme için varsayılan depolama hesabı anahtarı almak için nasıl gösterir.

```csharp
var results = _hdiManagementClient.Clusters.GetClusterConfigurations(<Resource Group Name>, <Cluster Name>, "core-site");
foreach (var key in results.Configuration.Keys)
{
    Console.WriteLine(String.Format("{0} => {1}", key, results.Configuration[key]));
}
```

## <a name="submit-jobs"></a>İş gönderme

**MapReduce işlerini göndermek için**

Bkz. [Haritayı ÇalıştırHDInsight'ta örnekleri azaltın.](hadoop/apache-hadoop-run-samples-linux.md)

**Apache Hive işlerini göndermek için** 

Bkz. [.NET SDK'yı kullanarak Apache Hive sorgularını çalıştırın.](hadoop/apache-hadoop-use-hive-dotnet-sdk.md)

**Apache Sqoop işleri göndermek için**

Bkz. [HDInsight ile Apache Sqoop kullanın.](hadoop/apache-hadoop-use-sqoop-dotnet-sdk.md)

**Apache Oozie işleri göndermek için**

HdInsight'ta [bir iş akışını tanımlamak ve çalıştırmak için Hadoop ile Apache Oozie'yi kullanın'a](hdinsight-use-oozie-linux-mac.md)bakın.

## <a name="upload-data-to-azure-blob-storage"></a>Azure Blob depolama alanına veri yükleme

Bkz. [HDInsight'a veri yükleme][hdinsight-upload-data].

## <a name="see-also"></a>Ayrıca Bkz.

* [HDInsight .NET SDK referans belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight)
* [Azure portalını kullanarak HDInsight'ta Apache Hadoop kümelerini yönetme](hdinsight-administer-use-portal-linux.md)
* [Komut satırı arabirimi kullanarak HDInsight'ı yönetme][hdinsight-admin-cli]
* [HDInsight kümeleri oluşturma][hdinsight-provision]
* [HDInsight'a veri yükleme][hdinsight-upload-data]
* [Azure HDInsight ile başlayın][hdinsight-get-started]

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-provision-custom-options]: hdinsight-hadoop-provision-linux-clusters.md#configuration
[hdinsight-submit-jobs]:hadoop/submit-apache-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-mapreduce]:hadoop/hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
