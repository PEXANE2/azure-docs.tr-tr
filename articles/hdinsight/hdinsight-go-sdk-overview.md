---
title: Azure HDInsight SDK Go için
description: Azure HDInsight SDK for Go ve Apache Hadoop kümelerini kullanmak için başvuru malzemesi
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seodec18
ms.date: 01/03/2020
ms.openlocfilehash: 292496c4d458621213fe62105149ac845d78891e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79479595"
---
# <a name="hdinsight-sdk-for-go-preview"></a>HDInsight SDK Go için (Önizleme)

## <a name="overview"></a>Genel Bakış
HDInsight SDK for Go, HDInsight kümelerinizi yönetmenize olanak tanıyan sınıflar ve işlevler sağlar. HdInsight kümelerinin özelliklerini oluşturmak, silmek, güncellemek, listelemek, yeniden boyutlandırmak, komut dosyası eylemlerini yürütmek, izlemek, almak ve daha fazlasını yapmak için işlemleri içerir.

> [!NOTE]  
>Bu SDK için GoDoc referans malzemesi de [burada mevcuttur.](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Bir [ `go get` araç](https://github.com/golang/go/wiki/GoGetTools).
* [Git.](https://golang.org/dl/)

## <a name="sdk-installation"></a>SDK kurulumu

GOPATH konumunuzdan,`go get github.com/Azure/azure-sdk-for-go/tree/master/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight`

## <a name="authentication"></a>Kimlik doğrulaması

SDK'nın öncelikle Azure aboneliğinizle kimlik doğrulaması yapılması gerekir.  Bir hizmet ilkesi oluşturmak ve kimlik doğrulaması için kullanmak için aşağıdaki örneği izleyin. Bu işlem yapıldıktan sonra, yönetim işlemleri `ClustersClient`gerçekleştirmek için kullanılabilecek birçok işlev (aşağıda bölümlerde özetlenen) içeren bir , bir örneğiniz olacaktır.

> [!NOTE]  
> İhtiyaçlarınız için daha uygun olabilecek aşağıdaki örnek dışında kimlik doğrulamanın başka yolları da vardır. Tüm işlevler burada özetlenmiştir: [Azure SDK'da Kimlik doğrulama işlevleri](https://docs.microsoft.com/azure/go/azure-sdk-go-authorization)

### <a name="authentication-example-using-a-service-principal"></a>Bir hizmet ilkesini kullanarak kimlik doğrulama örneği

İlk olarak, [Azure Bulut BulutU'ya](https://shell.azure.com/bash)giriş yapın. Hizmet sorumlusunun oluşturulmasını istediğiniz aboneliği şu anda kullandığınızı doğrulayın.

```azurecli-interactive
az account show
```

Abonelik bilgileriniz JSON olarak görüntülenir.

```json
{
  "environmentName": "AzureCloud",
  "id": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "isDefault": true,
  "name": "XXXXXXX",
  "state": "Enabled",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "user": {
    "cloudShellID": true,
    "name": "XXX@XXX.XXX",
    "type": "user"
  }
}
```

Doğru aboneliğe giriş yapmadıysanız, çalıştırarak doğru aboneliği seçin: 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]  
> HDInsight Kaynak Sağlayıcısını başka bir işlevle kaydettirmediyseniz (örneğin, Azure portalı üzerinden bir HDInsight Kümesi oluşturarak), kimliğini niçin doğrulayabilmeniz için bunu bir kez yapmanız gerekir. Bu, Aşağıdaki komutu çalıştırarak [Azure Bulut Kabuğu'ndan](https://shell.azure.com/bash) yapılabilir:
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

Ardından, hizmet müdürünüz için bir ad seçin ve aşağıdaki komutla oluşturun:

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

Hizmet temel bilgileri JSON olarak görüntülenir.

```json
{
  "clientId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "clientSecret": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "subscriptionId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```

Aşağıdaki snippet'i kopyalayın `CLIENT_ID` `CLIENT_SECRET`ve `SUBSCRIPTION_ID` hizmet ilkesini oluşturmak için komutu çalıştırdıktan sonra döndürülen JSON dizeleri ile doldurun. `TENANT_ID`

```golang
package main

import (
    "context"
    "github.com/Azure/go-autorest/autorest/azure/auth"
    hdi "github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight"
    "github.com/Azure/go-autorest/autorest/to"
)

func main() {
    var err error

    // Tenant ID for your Azure Subscription
    var TENANT_ID = ""
    // Your Service Principal App Client ID
    var CLIENT_ID = ""
    // Your Service Principal Client Secret
    var CLIENT_SECRET = ""
    // Azure Subscription ID
    var SUBSCRIPTION_ID = ""

    var credentials = auth.NewClientCredentialsConfig(CLIENT_ID, CLIENT_SECRET, TENANT_ID)
    var client = hdi.NewClustersClient(SUBSCRIPTION_ID)

    client.Authorizer, err = credentials.Authorizer()
    if (err != nil) {
        fmt.Println("Error: ", err)
    }
```

## <a name="cluster-management"></a>Küme yönetimi

> [!NOTE]  
> Bu bölüm, bir `ClusterClient` örneğin imi önceden doğruladığınızı ve inşa `client`ettiğinizi ve onu . Kimlik doğrulama ve kimlik doğrulama `ClusterClient` yönergeleri yukarıdaki Kimlik Doğrulama bölümünde bulunabilir.

### <a name="create-a-cluster"></a>Küme oluşturma

Yeni bir küme çağırarak `client.Create()`oluşturulabilir. 

#### <a name="example"></a>Örnek

Bu örnek, iki baş düğümü ve bir alt düğüm ile bir [Apache Spark](https://spark.apache.org/) kümesi oluşturmak için nasıl gösterir.

> [!NOTE]  
> Öncelikle aşağıda açıklandığı gibi bir Kaynak Grubu ve Depolama Hesabı oluşturmanız gerekir. Bunları zaten oluşturduysanız, bu adımları atlayabilirsiniz.

##### <a name="creating-a-resource-group"></a>Kaynak grubu oluşturma

[Azure Bulut Kabuğu'nu](https://shell.azure.com/bash) kullanarak bir kaynak grubu oluşturabilirsiniz

```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```

##### <a name="creating-a-storage-account"></a>Depolama hesabı oluşturma

Aşağıdakileri çalıştırarak Azure [Bulut Kabuğu'nu](https://shell.azure.com/bash) kullanarak bir depolama hesabı oluşturabilirsiniz:

```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```

Artık depolama hesabınız için anahtarı almak için aşağıdaki komutu çalıştırın (küme oluşturmak için buna ihtiyacınız olacak):

```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```

---
Aşağıdaki Go snippet iki baş düğümleri ve bir işçi düğümü ile bir Kıvılcım kümesi oluşturur. Yorumlarda açıklandığı gibi boş değişkenleri doldurun ve diğer parametreleri özel ihtiyaçlarınıza göre değiştirmekten çekinmeyin.

```golang
// The name for the cluster you are creating
var clusterName = "";
// The name of your existing Resource Group
var resourceGroupName = "";
// Choose a username
var username = "";
// Choose a password
var password = "";
// Replace <> with the name of your storage account
var storageAccount = "<>.blob.core.windows.net";
// Storage account key you obtained above
var storageAccountKey = "";
// Choose a region
var location = "";
var container = "default";

var parameters = hdi.ClusterCreateParametersExtended {
    Location: to.StringPtr(location),
    Tags: make(map[string]*string),
    Properties: &hdi.ClusterCreateProperties {
        ClusterVersion: to.StringPtr("3.6"),
        OsType: hdi.Linux,
        ClusterDefinition: &hdi.ClusterDefinition {
            Kind: to.StringPtr("spark"),
            Configurations: map[string]map[string]interface{}{
                "gateway": {
                    "restAuthCredential.isEnabled": "True",
                    "restAuthCredential.username":  username,
                    "restAuthCredential.password":  password,
                },
            },
        },
        Tier: hdi.Standard,
        ComputeProfile: &hdi.ComputeProfile {
            Roles: &[]hdi.Role {
                hdi.Role {
                    Name: to.StringPtr("headnode"),
                    TargetInstanceCount: to.Int32Ptr(2),
                    HardwareProfile: &hdi.HardwareProfile {
                        VMSize: to.StringPtr("Large"),
                    },
                    OsProfile: &hdi.OsProfile {
                        LinuxOperatingSystemProfile: &hdi.LinuxOperatingSystemProfile {
                            Username: to.StringPtr(username),
                            Password: to.StringPtr(password),
                        },
                    },
                },
                hdi.Role {
                    Name: to.StringPtr("workernode"),
                    TargetInstanceCount: to.Int32Ptr(1),
                    HardwareProfile: &hdi.HardwareProfile {
                        VMSize: to.StringPtr("Large"),
                    },
                    OsProfile: &hdi.OsProfile {
                        LinuxOperatingSystemProfile: &hdi.LinuxOperatingSystemProfile {
                            Username: to.StringPtr(username),
                            Password: to.StringPtr(password),
                        },
                    },
                },
            },
        },
        StorageProfile: &hdi.StorageProfile {
            Storageaccounts: &[]hdi.StorageAccount {
                hdi.StorageAccount {
                    Name: to.StringPtr(storageAccount),
                    Key: to.StringPtr(storageAccountKey),
                    Container: to.StringPtr(container),
                    IsDefault: to.BoolPtr(true),
                },
            },
        },
    },
}
client.Create(context.Background(), resourceGroupName, clusterName, parameters)
```

### <a name="get-cluster-details"></a>Küme ayrıntılarını alın

Belirli bir küme için özellikler almak için:

```golang
client.Get(context.Background(), "<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>Örnek

Kümenizi `get` başarıyla oluşturduğunuzu doğrulamak için kullanabilirsiniz.

```golang
cluster, err := client.Get(context.Background(), resourceGroupName, clusterName)
if (err != nil) {
    fmt.Println("Error: ", err)
}
fmt.Println(*cluster.Name)
fmt.Println(*cluster.ID
```

Çıktı aşağıdaki gibi görünmelidir:

```
<Cluster Name>
/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>
```

### <a name="list-clusters"></a>Liste kümeleri

#### <a name="list-clusters-under-the-subscription"></a>Abonelik altındaki liste kümeleri

```golang
client.List()
```

#### <a name="list-clusters-by-resource-group"></a>Kaynak grubuna göre liste kümeleri

```golang
client.ListByResourceGroup("<Resource Group Name>")
```

> [!NOTE]  
> Her `List()` `ListByResourceGroup()` ikisi `ClusterListResultPage` de ve bir yapı dönmek. Bir sonraki sayfayı almak `Next()`için. Bu, aşağıdaki `ClusterListResultPage.NotDone()` örnekte gösterildiği gibi, döndürüne `false`kadar yinelenebilir.

#### <a name="example"></a>Örnek

Aşağıdaki örnek, geçerli abonelik için tüm kümelerin özelliklerini yazdırır:

```golang
page, err := client.List(context.Background())
if (err != nil) {
    fmt.Println("Error: ", err)
}
for (page.NotDone()) {
    for _, cluster := range page.Values() {
        fmt.Println(*cluster.Name)
    }
    err = page.Next();
    if (err != nil) {
        fmt.Println("Error: ", err)
    }
}
```

### <a name="delete-a-cluster"></a>Küme silme

Bir kümeyi silmek için:

```golang
client.Delete(context.Background(), "<Resource Group Name>", "<Cluster Name>")
```

### <a name="update-cluster-tags"></a>Küme etiketlerini güncelleştir

Belirli bir kümenin etiketlerini şu şekilde güncelleştirebilirsiniz:

```golang
client.Update(context.Background(), "<Resource Group Name>", "<Cluster Name>", hdi.ClusterPatchParameters{<map[string]*string} of Tags>)
```

#### <a name="example"></a>Örnek

```golang
client.Update(context.Background(), "SDKTestRG", "SDKTest", hdi.ClusterPatchParameters{map[string]*string{"tag1Name" : to.StringPtr("tag1Value"), "tag2Name" : to.StringPtr("tag2Value")}})
```

### <a name="resize-cluster"></a>Kümeyi yeniden boyutlandırma

Aşağıdaki gibi yeni bir boyut belirterek belirli bir kümenin alt düğüm sayısını yeniden boyutlandırabilirsiniz:

```golang
client.Resize(context.Background(), "<Resource Group Name>", "<Cluster Name>", hdi.ClusterResizeParameters{<Num of Worker Nodes (int)>})
```

## <a name="cluster-monitoring"></a>Küme izleme

HDInsight Management SDK, Operations Management Suite (OMS) aracılığıyla kümelerinizdeki izleme işlemlerini yönetmek için de kullanılabilir.

Yönetim işlemleri için `ClusterClient` nasıl kullanılacağına benzer şekilde, izleme `ExtensionClient` işlemleri için kullanılacak bir şey oluşturmanız gerekir. Yukarıdaki Kimlik Doğrulama bölümünü tamamladıktan sonra, aşağıdakileri gibi bir `ExtensionClient` şey oluşturabilirsiniz:

```golang
extClient := hdi.NewExtensionsClient(SUBSCRIPTION_ID)
extClient.Authorizer, _ = credentials.Authorizer()
```

> [!NOTE]  
> Aşağıdaki izleme örnekleri, bir `ExtensionClient` çağrıyı `extClient` zaten başharfe adlandırdığınızı ve yukarıda gösterildiği `Authorizer` şekilde ayarladığınızı varsayar.

### <a name="enable-oms-monitoring"></a>OMS izlemeyi etkinleştirme

> [!NOTE]  
> OMS İzleme'yi etkinleştirmek için varolan bir Log Analytics çalışma alanınız olması gerekir. Daha önce bir tane oluşturmadıysanız, bunu nasıl yapacağınızı buradan öğrenebilirsiniz: [Azure portalında Bir Log Analytics çalışma alanı oluşturun.](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace)

Kümenizde OMS İzleme'yi etkinleştirmek için:

```golang
extClient.EnableMonitoring(context.Background(), "<Resource Group Name", "Cluster Name", hdi.ClusterMonitoringRequest {WorkspaceID: to.StringPtr("<Workspace Id>")})
```

### <a name="view-status-of-oms-monitoring"></a>OMS izlemedurumunu görüntüleme

Kümenizdeki OMS durumunu almak için:

```golang
extClient.GetMonitoringStatus(context.Background(), "<Resource Group Name", "Cluster Name")
```

### <a name="disable-oms-monitoring"></a>OMS izlemeyi devre dışı

Kümenizdeki OMS'yi devre dışı kömek için:

```golang
extClient.DisableMonitoring(context.Background(), "<Resource Group Name", "Cluster Name")
```

## <a name="script-actions"></a>Komut dosyası eylemleri

HDInsight, kümeyi özelleştirmek için özel komut dosyaları çağıran komut dosyası eylemleri adlı bir yapılandırma işlevi sağlar.

> [!NOTE]  
> Komut dosyası eylemlerinin nasıl kullanılacağı hakkında daha fazla bilgiyi burada bulabilirsiniz: [Komut dosyası eylemlerini kullanarak Linux tabanlı HDInsight kümelerini özelleştirin](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)

### <a name="execute-script-actions"></a>Komut dosyası eylemlerini yürütme

Belirli bir kümede komut dosyası eylemlerini şu şekilde yürütebilirsiniz:

```golang
var scriptAction1 = hdi.RuntimeScriptAction{Name: to.StringPtr("<Script Name>"), URI: to.StringPtr("<URL To Script>"), Roles: <&[]string of roles>} //valid roles are "headnode", "workernode", "zookeepernode", and "edgenode"
client.ExecuteScriptActions(context.Background(), "<Resource Group Name>", "<Cluster Name>", hdi.ExecuteScriptActionParameters{PersistOnSuccess: to.BoolPtr(true), ScriptActions: &[]hdi.RuntimeScriptAction{scriptAction1}}) //add more RuntimeScriptActions to the list to execute multiple scripts
```

'Komut Dosyası Eylemini Sil' ve 'Kalıcı Komut Dosyası Eylemlerini `ScriptActionsClient`Listele' işlemleri `ClusterClient` için, yönetim işlemleri için nasıl oluşturduğunuza benzer bir Yukarıdaki Kimlik Doğrulama bölümünü tamamladıktan sonra, aşağıdakileri gibi bir `ScriptActionsClient` şey oluşturabilirsiniz:

```golang
scriptActionsClient := hdi.NewScriptActionsClient(SUBSCRIPTION_ID)
scriptActionsClient.Authorizer, _ = credentials.Authorizer()
```

> [!NOTE]  
> Aşağıdaki komut dosyası eylemleri örnekleri, bir `ScriptActionsClient` `scriptActionsClient` çağrıyı `Authorizer` zaten başlangıç olarak adlandırdığınızı ve yukarıda gösterildiği şekilde ayarladığınızı varsayar.

### <a name="delete-script-action"></a>Komut dosyası eylemini silme

Belirli bir kümede belirtilen kalıcı komut dosyası eylemini silmek için:

```golang
scriptActionsClient.Delete(context.Background(), "<Resource Group Name>", "<Cluster Name>", "<Script Name>")
```

### <a name="list-persisted-script-actions"></a>Kalıcı komut dosyası eylemlerini listele

> [!NOTE]  
> Her `ListByCluster()` ikisi `ScriptActionsListPage` de bir yapı döndürür. Bir sonraki sayfayı almak `Next()`için. Bu, aşağıdaki `ClusterListResultPage.NotDone()` örnekte gösterildiği gibi, döndürüne `false`kadar yinelenebilir.

Belirtilen küme için tüm kalıcı komut dosyası eylemlerini listelemek için:
```golang
scriptActionsClient.ListByCluster(context.Background(), "<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>Örnek

```golang
page, err := scriptActionsClient.ListByCluster(context.Background(), resourceGroupName, clusterName)
if (err != nil) {
    fmt.Println("Error: ", err)
}
for (page.NotDone()) {
    for _, script := range page.Values() {
        fmt.Println(*script.Name) //There are functions to get other properties of RuntimeScriptActionDetail besides Name, such as Status, Operation, StartTime, EndTime, etc. See reference documentation.
    }
    err = page.Next();
    if (err != nil) {
        fmt.Println("Error: ", err)
    }
}
```

### <a name="list-all-scripts-execution-history"></a>Tüm komut dosyalarının yürütme geçmişini listele

Bu işlem için, yönetim `ScriptExecutionHistoryClient`işlemleri için nasıl oluşturduğunuza `ClusterClient` benzer bir Yukarıdaki Kimlik Doğrulama bölümünü tamamladıktan sonra, aşağıdakileri gibi bir `ScriptActionsClient` şey oluşturabilirsiniz:

```golang
scriptExecutionHistoryClient := hdi.NewScriptExecutionHistoryClient(SUBSCRIPTION_ID)
scriptExecutionHistoryClient.Authorizer, _ = credentials.Authorizer()
```

> [!NOTE]  
> Aşağıda zaten bir `ScriptExecutionHistoryClient` çağrı `scriptExecutionHistoryClient` nın başlatılması nı `Authorizer` ve yukarıda gösterildiği şekilde ayarladığınızı varsayar.

Belirtilen küme için tüm komut dosyalarının yürütme geçmişini listelemek için:

```golang
scriptExecutionHistoryClient.ListByCluster(context.Background(), "<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>Örnek

Bu örnek, geçmiş komut dosyası yürütmeleri için tüm ayrıntıları yazdırır.

```golang
page, err := scriptExecutionHistoryClient.ListByCluster(context.Background(), resourceGroupName, clusterName)
if (err != nil) {
    fmt.Println("Error: ", err)
}
for (page.NotDone()) {
    for _, script := range page.Values() {
        fmt.Println(*script.Name) //There are functions to get other properties of RuntimeScriptActionDetail besides Name, such as Status, Operation, StartTime, EndTime, etc. See reference documentation.
    }
    err = page.Next();
    if (err != nil) {
        fmt.Println("Error: ", err)
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

[GoDoc referans malzemesini](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/preview/hdinsight/mgmt/2018-06-01-preview/hdinsight)keşfedin. GoDocs, SDK'daki tüm işlevler için başvuru belgeleri sağlar.
