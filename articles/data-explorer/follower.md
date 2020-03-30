---
title: Azure Veri Gezgini'nde veritabanları eklemek için takipçi veritabanı özelliğini kullanma
description: Takipçi veritabanı özelliğini kullanarak Azure Veri Gezgini'nde veritabanlarının nasıl eklenebildiğini öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: gabilehner
ms.service: data-explorer
ms.topic: conceptual
ms.date: 11/07/2019
ms.openlocfilehash: f6dbdb54c1c5a5d477c3ccb988963758faab83b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79140023"
---
# <a name="use-follower-database-to-attach-databases-in-azure-data-explorer"></a>Azure Veri Gezgini'nde veritabanları eklemek için takipçi veritabanını kullanma

**Takipçi veritabanı** özelliği, Azure Veri Gezgini kümenize farklı bir kümede bulunan bir veritabanı eklemenize olanak tanır. **Takipçi veritabanı** *salt okunur* modunda eklenir, bu da verileri görüntülemeyi ve **lider veritabanına**yutulan verilerdeki sorguları çalıştırmayı mümkün kılar. Takipçi veritabanı, lider veritabanlarındaki değişiklikleri eşitler. Eşitleme nedeniyle, veri kullanılabilirliğinde birkaç saniye ile birkaç dakika arasında bir veri gecikmesi vardır. Gecikme süresinin uzunluğu, lider veritabanı meta verilerinin genel boyutuna bağlıdır. Lider ve takipçi veritabanları verileri almak için aynı depolama hesabını kullanır. Depolama lideri veritabanına aittir. Takipçi veritabanı, verileri yutmaya gerek kalmadan görüntüler. Ekteki veritabanı salt okunur veritabanı olduğundan, veritabanındaki veriler, tablolar ve ilkeler [önbelleğe alma ilkesi,](#configure-caching-policy) [ilkeleri](#manage-principals)ve [izinleri](#manage-permissions)dışında değiştirilemez. Ekli veritabanları silinemez. Bunlar lider veya takipçisi tarafından müstakil olmalı ve ancak o zaman silinebilir. 

Takipçi özelliğini kullanarak farklı bir kümeye veritabanı eklemek, kuruluşlar ve ekipler arasında veri paylaşmak için altyapı olarak kullanılır. Bu özellik, üretim ortamını üretim dışı kullanım durumlarına karşı korumak için bilgi işlem kaynaklarını ayırmak için yararlıdır. Takipçi, Azure Veri Gezgini kümesinin maliyetini verilerde sorgu çalıştıran tarafla ilişkilendirmek için de kullanılabilir.

## <a name="which-databases-are-followed"></a>Hangi veritabanları takip edilir?

* Küme, bir veritabanını, birkaç veritabanını veya bir lider kümesinin tüm veritabanlarını izleyebilir. 
* Tek bir küme, birden çok lider kümesinden veritabanlarını izleyebilir. 
* Küme hem takipçi veritabanları nı hem de lider veritabanlarını içerebilir

## <a name="prerequisites"></a>Ön koşullar

1. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)
1. Lider ve takipçi için [küme ve DB oluşturun.](/azure/data-explorer/create-cluster-database-portal)
1. [Yutma genel bakışta](/azure/data-explorer/ingest-data-overview)tartışılan çeşitli yöntemlerden birini kullanarak lider veritabanına veri [yutma.](/azure/data-explorer/ingest-sample-data)

## <a name="attach-a-database"></a>Veritabanı ekleme

Veritabanı eklemek için kullanabileceğiniz çeşitli yöntemler vardır. Bu makalede, C# veya Azure Kaynak Yöneticisi şablonu kullanarak bir veritabanı eklemeyi tartışıyoruz. Veritabanı eklemek için lider kümesi ve takipçi kümesinde izinlere sahip olmalısınız. İzinler hakkında daha fazla bilgi için, [izinleri yönetme'ye](#manage-permissions)bakın.

### <a name="attach-a-database-using-c"></a>C kullanarak veritabanı ekleme #

#### <a name="needed-nugets"></a>Gerekli Nugets

* [Microsoft.Azure.Management.kusto'ya yükleyin.](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/)
* [Microsoft.Rest.ClientRuntime.Azure.Authentication kimlik doğrulaması için yükleyin.](https://www.nuget.org/packages/Microsoft.Rest.ClientRuntime.Azure.Authentication)

#### <a name="code-example"></a>Kod Örneği

```Csharp
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client secret
var leaderSubscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";
var followerSubscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";

var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, clientSecret);
var resourceManagementClient = new KustoManagementClient(serviceCreds){
    SubscriptionId = followerSubscriptionId
};

var followerResourceGroupName = "followerResouceGroup";
var leaderResourceGroup = "leaderResouceGroup";
var leaderClusterName = "leader";
var followerClusterName = "follower";
var attachedDatabaseConfigurationName = "uniqueNameForAttachedDatabaseConfiguration";
var databaseName = "db"; // Can be specific database name or * for all databases
var defaultPrincipalsModificationKind = "Union"; 
var location = "North Central US";

AttachedDatabaseConfiguration attachedDatabaseConfigurationProperties = new AttachedDatabaseConfiguration()
{
    ClusterResourceId = $"/subscriptions/{leaderSubscriptionId}/resourceGroups/{leaderResourceGroup}/providers/Microsoft.Kusto/Clusters/{leaderClusterName}",
    DatabaseName = databaseName,
    DefaultPrincipalsModificationKind = defaultPrincipalsModificationKind,
    Location = location
};

var attachedDatabaseConfigurations = resourceManagementClient.AttachedDatabaseConfigurations.CreateOrUpdate(followerResourceGroupName, followerClusterName, attachedDatabaseConfigurationName, attachedDatabaseConfigurationProperties);
```

### <a name="attach-a-database-using-python"></a>Python'u kullanarak veritabanı ekleme

#### <a name="needed-modules"></a>Gerekli Modüller

```
pip install azure-common
pip install azure-mgmt-kusto
```

#### <a name="code-example"></a>Kod Örneği

```python
from azure.mgmt.kusto import KustoManagementClient
from azure.mgmt.kusto.models import AttachedDatabaseConfiguration
from azure.common.credentials import ServicePrincipalCredentials
import datetime

#Directory (tenant) ID
tenant_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
#Application ID
client_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
#Client Secret
client_secret = "xxxxxxxxxxxxxx"
follower_subscription_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
leader_subscription_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
credentials = ServicePrincipalCredentials(
        client_id=client_id,
        secret=client_secret,
        tenant=tenant_id
    )
kusto_management_client = KustoManagementClient(credentials, follower_subscription_id)

follower_resource_group_name = "followerResouceGroup"
leader_resouce_group_name = "leaderResouceGroup"
follower_cluster_name = "follower"
leader_cluster_name = "leader"
attached_database_Configuration_name = "uniqueNameForAttachedDatabaseConfiguration"
database_name  = "db" # Can be specific database name or * for all databases
default_principals_modification_kind  = "Union"
location = "North Central US"
cluster_resource_id = "/subscriptions/" + leader_subscription_id + "/resourceGroups/" + leader_resouce_group_name + "/providers/Microsoft.Kusto/Clusters/" + leader_cluster_name

attached_database_configuration_properties = AttachedDatabaseConfiguration(cluster_resource_id = cluster_resource_id, database_name = database_name, default_principals_modification_kind = default_principals_modification_kind, location = location)

#Returns an instance of LROPoller, see https://docs.microsoft.com/python/api/msrest/msrest.polling.lropoller?view=azure-python
poller = kusto_management_client.attached_database_configurations.create_or_update(follower_resource_group_name, follower_cluster_name, attached_database_Configuration_name, attached_database_configuration_properties)
```

### <a name="attach-a-database-using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak veritabanı ekleme

Bu bölümde, [bir Azure Kaynak Yöneticisi şablonu](../azure-resource-manager/management/overview.md)kullanarak varolan bir cluser'a veritabanı eklemeyi öğrenirsiniz. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "followerClusterName": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "Name of the cluster to which the database will be attached."
            }
        },
        "attachedDatabaseConfigurationsName": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "Name of the attached database configurations to create."
            }
        },
        "databaseName": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The name of the database to follow. You can follow all databases by using '*'."
            }
        },
        "leaderClusterResourceId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The resource ID of the leader cluster."
            }
        },
        "defaultPrincipalsModificationKind": {
            "type": "string",
            "defaultValue": "Union",
            "metadata": {
                "description": "The default principal modification kind."
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "Location for all resources."
            }
        }
    },
    "variables": {},
    "resources": [
        {
            "name": "[concat(parameters('followerClusterName'), '/', parameters('attachedDatabaseConfigurationsName'))]",
            "type": "Microsoft.Kusto/clusters/attachedDatabaseConfigurations",
            "apiVersion": "2019-09-07",
            "location": "[parameters('location')]",
            "properties": {
                "databaseName": "[parameters('databaseName')]",
                "clusterResourceId": "[parameters('leaderClusterResourceId')]",
                "defaultPrincipalsModificationKind": "[parameters('defaultPrincipalsModificationKind')]"
            }
        }
    ]
}
```

### <a name="deploy-the-template"></a>Şablonu dağıtma 

Azure portalını kullanarak veya powershell'i kullanarak Azure Kaynak Yöneticisi [şablonuna](https://portal.azure.com) dağıtabilirsiniz.

   ![şablon dağıtımı](media/follower/template-deployment.png)


|**Ayar**  |**Açıklama**  |
|---------|---------|
|Takipçi Küme Adı     |  Takipçi kümesinin adı; şablonun dağıtılanacağı yer.  |
|Ekli Veritabanı Yapılandırmaları Adı    |    Ekteki veritabanı yapılandırmaları nesnenin adı. Ad küme düzeyinde benzersiz herhangi bir dize olabilir.     |
|Veritabanı Adı     |      İzlenecek veritabanının adı. Liderin tüm veritabanlarını takip etmek istiyorsanız , '*' kullanın.   |
|Lider Küme Kaynak Kimliği    |   Lider kümesinin kaynak kimliği.      |
|Varsayılan İlkeler Değişiklik Türü    |   Varsayılan temel değişiklik türü. Olabilir `Union`, `Replace` `None`ya da . Varsayılan temel değişiklik türü hakkında daha fazla bilgi [için, asıl değişiklik türü denetim komutuna](/azure/kusto/management/cluster-follower?branch=master#alter-follower-database-principals-modification-kind)bakın.      |
|Konum   |   Tüm kaynakların konumu. Lider ve takipçi aynı yerde olmalıdır.       |
 
### <a name="verify-that-the-database-was-successfully-attached"></a>Veritabanının başarıyla bağlı olduğunu doğrulama

Veritabanının başarıyla eklenmiş olduğunu doğrulamak için, ekteki veritabanlarınızı [Azure portalında](https://portal.azure.com)bulun. 

1. Takipçi kümesine gidin ve **Veritabanları'nı** seçin
1. Veritabanı listesinde yeni Salt Okunur veritabanlarını arayın.

    ![Salt okunur takipçi veritabanı](media/follower/read-only-follower-database.png)

Alternatif olarak:

1. Lider kümesine gidin ve **Veritabanları'nı** seçin
2. İlgili veritabanlarının **BAŞKALARıYLA PAYLAŞILDI** > OLARAK İşaretLENİlMeSInDen**denetlemeYin Evet**

    ![Ekli veritabanlarını okuma ve yazma](media/follower/read-write-databases-shared.png)

## <a name="detach-the-follower-database-using-c"></a>C kullanarak takipçi veritabanını ayırın # 

### <a name="detach-the-attached-follower-database-from-the-follower-cluster"></a>Ekteki takipçi veritabanını takipçi kümesinden ayırın

Takipçi kümesi bağlı herhangi bir veritabanını aşağıdaki gibi ayırabilir:

```csharp
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client secret
var leaderSubscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";
var followerSubscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";

var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, clientSecret);
var resourceManagementClient = new KustoManagementClient(serviceCreds){
    SubscriptionId = followerSubscriptionId
};

var followerResourceGroupName = "testrg";
//The cluster and database that are created as part of the prerequisites
var followerClusterName = "follower";
var attachedDatabaseConfigurationsName = "uniqueName";

resourceManagementClient.AttachedDatabaseConfigurations.Delete(followerResourceGroupName, followerClusterName, attachedDatabaseConfigurationsName);
```

### <a name="detach-the-attached-follower-database-from-the-leader-cluster"></a>Bağlı takipçi veritabanını lider kümesinden ayırma

Lider kümesi ekli herhangi bir veritabanını aşağıdaki gibi ayırabilir:

```csharp
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client secret
var leaderSubscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";
var followerSubscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";

var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, clientSecret);
var resourceManagementClient = new KustoManagementClient(serviceCreds){
    SubscriptionId = leaderSubscriptionId
};

var leaderResourceGroupName = "testrg";
var followerResourceGroupName = "followerResouceGroup";
var leaderClusterName = "leader";
var followerClusterName = "follower";
//The cluster and database that are created as part of the Prerequisites
var followerDatabaseDefinition = new FollowerDatabaseDefinition()
    {
        AttachedDatabaseConfigurationName = "uniqueName",
        ClusterResourceId = $"/subscriptions/{followerSubscriptionId}/resourceGroups/{followerResourceGroupName}/providers/Microsoft.Kusto/Clusters/{followerClusterName}"
    };

resourceManagementClient.Clusters.DetachFollowerDatabases(leaderResourceGroupName, leaderClusterName, followerDatabaseDefinition);
```

## <a name="detach-the-follower-database-using-python"></a>Python'u kullanarak takipçi veritabanını ayırın

### <a name="detach-the-attached-follower-database-from-the-follower-cluster"></a>Ekteki takipçi veritabanını takipçi kümesinden ayırın

Takipçi kümesi bağlı herhangi bir veritabanını aşağıdaki gibi ayırabilir:

```python
from azure.mgmt.kusto import KustoManagementClient
from azure.common.credentials import ServicePrincipalCredentials
import datetime

#Directory (tenant) ID
tenant_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
#Application ID
client_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
#Client Secret
client_secret = "xxxxxxxxxxxxxx"
follower_subscription_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
credentials = ServicePrincipalCredentials(
        client_id=client_id,
        secret=client_secret,
        tenant=tenant_id
    )
kusto_management_client = KustoManagementClient(credentials, follower_subscription_id)

follower_resource_group_name = "followerResouceGroup"
follower_cluster_name = "follower"
attached_database_configurationName = "uniqueName"

#Returns an instance of LROPoller, see https://docs.microsoft.com/python/api/msrest/msrest.polling.lropoller?view=azure-python
poller = kusto_management_client.attached_database_configurations.delete(follower_resource_group_name, follower_cluster_name, attached_database_configurationName)
```

### <a name="detach-the-attached-follower-database-from-the-leader-cluster"></a>Bağlı takipçi veritabanını lider kümesinden ayırma

Lider kümesi ekli herhangi bir veritabanını aşağıdaki gibi ayırabilir:

```python

from azure.mgmt.kusto import KustoManagementClient
from azure.mgmt.kusto.models import FollowerDatabaseDefinition
from azure.common.credentials import ServicePrincipalCredentials
import datetime

#Directory (tenant) ID
tenant_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
#Application ID
client_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
#Client Secret
client_secret = "xxxxxxxxxxxxxx"
follower_subscription_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
leader_subscription_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
credentials = ServicePrincipalCredentials(
        client_id=client_id,
        secret=client_secret,
        tenant=tenant_id
    )
kusto_management_client = KustoManagementClient(credentials, follower_subscription_id)

follower_resource_group_name = "followerResourceGroup"
leader_resource_group_name = "leaderResourceGroup"
follower_cluster_name = "follower"
leader_cluster_name = "leader"
attached_database_configuration_name = "uniqueName"
location = "North Central US"
cluster_resource_id = "/subscriptions/" + follower_subscription_id + "/resourceGroups/" + follower_resource_group_name + "/providers/Microsoft.Kusto/Clusters/" + follower_cluster_name


#Returns an instance of LROPoller, see https://docs.microsoft.com/python/api/msrest/msrest.polling.lropoller?view=azure-python
poller = kusto_management_client.clusters.detach_follower_databases(resource_group_name = leader_resource_group_name, cluster_name = leader_cluster_name, cluster_resource_id = cluster_resource_id, attached_database_configuration_name = attached_database_configuration_name)
```

## <a name="manage-principals-permissions-and-caching-policy"></a>İlkeleri, izinleri ve önbelleğe alma ilkesini yönetme

### <a name="manage-principals"></a>Müdürleri yönetme

Veritabanı nı iliştirirken **"varsayılan ilkeler değişikliği türünü"** belirtin. Varsayılan, [yetkili ilkelerin](/azure/kusto/management/access-control/index#authorization) lider veritabanı koleksiyonunu tutmaktır

|**Tür** |**Açıklama**  |
|---------|---------|
|**Birleşim**     |   Ekli veritabanı ilkeleri her zaman özgün veritabanı ilkeleri artı takipçi veritabanına eklenen ek yeni ilkeler içerecektir.      |
|**Değiştirmek**   |    Orijinal veritabanından ilkeler devri yok. Ekteki veritabanı için yeni ilkeler oluşturulmalıdır.     |
|**Yok**   |   Ekli veritabanı ilkeleri, yalnızca ek ilkeleri olan özgün veritabanının ilkelerini içerir.      |

Yetkili ilkeleri yapılandırmak için denetim komutlarını kullanma hakkında daha fazla bilgi [için, bir takipçi kümesini yönetmek için Denetim komutlarına](/azure/kusto/management/cluster-follower)bakın.

### <a name="manage-permissions"></a>İzinleri yönet

Salt okunur veritabanı izninin yönetilmesi tüm veritabanı türleri için aynıdır. [Azure portalında izinleri yönetme'ye](/azure/data-explorer/manage-database-permissions#manage-permissions-in-the-azure-portal)bakın.

### <a name="configure-caching-policy"></a>Önbelleğe alma ilkesini yapılandırma

Takipçi veritabanı yöneticisi, ekteki veritabanının [önbelleğe alma ilkesini](/azure/kusto/management/cache-policy) veya barındırma kümesindeki tablolarından herhangi birini değiştirebilir. Varsayılan veritabanı ve tablo düzeyinde önbelleğe alma ilkeleri nin lider veritabanı koleksiyonunu tutmaktır. Örneğin, yalnızca sorun giderme için son verileri sorgulamak için, aylık raporlama yı çalıştırmak için lider veritabanında 30 günlük önbelleğe alma ilkesi ve takipçi veritabanında üç günlük önbelleğe alma ilkesiniz olabilir. Takipçi veritabanı veya tablodaki önbelleğe alma ilkesini yapılandırmak için denetim komutlarını kullanma hakkında daha fazla bilgi için, [takipçi kümesini yönetmek için Denetim komutlarına](/azure/kusto/management/cluster-follower)bakın.

## <a name="limitations"></a>Sınırlamalar

* Takipçi ve lider kümeleri aynı bölgede olmalıdır.
* [Akış akışı,](/azure/data-explorer/ingest-data-streaming) izlenen bir veritabanında kullanılamaz.
* [Müşteri yönetilen anahtarları](/azure/data-explorer/security#customer-managed-keys-with-azure-key-vault) kullanarak veri şifreleme hem lider hem de takipçi kümelerinde desteklenmez. 
* Ayırmadan önce farklı bir kümeye bağlı bir veritabanını silemezsiniz.
* Ayırmadan önce farklı bir kümeye bağlı veritabanı olan bir kümeyi silemezsiniz.
* Takipçi veya lider veritabanı(lar) eklenmiş bir kümeyi durduramazsınız. 

## <a name="next-steps"></a>Sonraki adımlar

* Takipçi küme yapılandırması hakkında bilgi için, [takipçi kümesini yönetmek için Denetim komutlarına](/azure/kusto/management/cluster-follower)bakın.
