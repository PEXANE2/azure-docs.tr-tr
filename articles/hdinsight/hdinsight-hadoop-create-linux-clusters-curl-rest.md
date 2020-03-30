---
title: Azure REST API'sini kullanarak Apache Hadoop kümeleri oluşturma - Azure
description: Azure Kaynak Yöneticisi şablonlarını Azure REST API'sine göndererek HDInsight kümelerini nasıl oluşturabilirsiniz öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/10/2019
ms.openlocfilehash: 2680304bd73bdbae35b29b89f38ae2665615f5e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80239930"
---
# <a name="create-apache-hadoop-clusters-using-the-azure-rest-api"></a>Azure REST API'sini kullanarak Apache Hadoop kümeleri oluşturma

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure Kaynak Yöneticisi şablonu ve Azure REST API'sini kullanarak nasıl HDInsight kümesi oluşturabilirsiniz öğrenin.

Azure REST API, HDInsight kümeleri gibi yeni kaynakların oluşturulması da dahil olmak üzere Azure platformunda barındırılan hizmetlerde yönetim işlemleri gerçekleştirmenize olanak tanır.

> [!NOTE]  
> Bu belgedeki adımlar, Azure REST API'si ile iletişim kurmak için [bukleyihttps://curl.haxx.se/) (yardımcı](https://curl.haxx.se/) programı) kullanır.

## <a name="create-a-template"></a>Şablon oluşturma

Azure Kaynak Yöneticisi şablonları, bir **kaynak grubunu** ve tüm kaynakları açıklayan JSON belgeleridir (HDInsight gibi.) Bu şablon tabanlı yaklaşım, HDInsight için ihtiyacınız olan kaynakları tek bir şablonda tanımlamanıza olanak tanır.

Aşağıdaki JSON [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password)belgesi, Şablon ve parametreler dosyalarının birleşmesidir ve ssh kullanıcı hesabını güvenli hale getirmek için parola kullanarak Linux tabanlı bir küme oluşturur.

   ```json
   {
       "properties": {
           "template": {
               "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
               "contentVersion": "1.0.0.0",
               "parameters": {
                   "clusterType": {
                       "type": "string",
                       "allowedValues": ["hadoop",
                       "hbase",
                       "storm",
                       "spark"],
                       "metadata": {
                           "description": "The type of the HDInsight cluster to create."
                       }
                   },
                   "clusterName": {
                       "type": "string",
                       "metadata": {
                           "description": "The name of the HDInsight cluster to create."
                       }
                   },
                   "clusterLoginUserName": {
                       "type": "string",
                       "metadata": {
                           "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
                       }
                   },
                   "clusterLoginPassword": {
                       "type": "securestring",
                       "metadata": {
                           "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                       }
                   },
                   "sshUserName": {
                       "type": "string",
                       "metadata": {
                           "description": "These credentials can be used to remotely access the cluster."
                       }
                   },
                   "sshPassword": {
                       "type": "securestring",
                       "metadata": {
                           "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                       }
                   },
                   "clusterStorageAccountName": {
                       "type": "string",
                       "metadata": {
                           "description": "The name of the storage account to be created and be used as the cluster's storage."
                       }
                   },
                   "clusterWorkerNodeCount": {
                       "type": "int",
                       "defaultValue": 4,
                       "metadata": {
                           "description": "The number of nodes in the HDInsight cluster."
                       }
                   }
               },
               "variables": {
                   "defaultApiVersion": "2015-05-01-preview",
                   "clusterApiVersion": "2015-03-01-preview"
               },
               "resources": [{
                   "name": "[parameters('clusterStorageAccountName')]",
                   "type": "Microsoft.Storage/storageAccounts",
                   "location": "[resourceGroup().location]",
                   "apiVersion": "[variables('defaultApiVersion')]",
                   "dependsOn": [],
                   "tags": {

                   },
                   "properties": {
                       "accountType": "Standard_LRS"
                   }
               },
               {
                   "name": "[parameters('clusterName')]",
                   "type": "Microsoft.HDInsight/clusters",
                   "location": "[resourceGroup().location]",
                   "apiVersion": "[variables('clusterApiVersion')]",
                   "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                   "tags": {

                   },
                   "properties": {
                       "clusterVersion": "3.6",
                       "osType": "Linux",
                       "clusterDefinition": {
                           "kind": "[parameters('clusterType')]",
                           "configurations": {
                               "gateway": {
                                   "restAuthCredential.isEnabled": true,
                                   "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                   "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                               }
                           }
                       },
                       "storageProfile": {
                           "storageaccounts": [{
                               "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                               "isDefault": true,
                               "container": "[parameters('clusterName')]",
                               "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                           }]
                       },
                       "computeProfile": {
                           "roles": [{
                               "name": "headnode",
                               "targetInstanceCount": "2",
                               "hardwareProfile": {
                                   "vmSize": "{}" 
                               },
                               "osProfile": {
                                   "linuxOperatingSystemProfile": {
                                       "username": "[parameters('sshUserName')]",
                                       "password": "[parameters('sshPassword')]"
                                   }
                               }
                           },
                           {
                               "name": "workernode",
                               "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                               "hardwareProfile": {
                                   "vmSize": "{}"
                               },
                               "osProfile": {
                                   "linuxOperatingSystemProfile": {
                                       "username": "[parameters('sshUserName')]",
                                       "password": "[parameters('sshPassword')]"
                                   }
                               }
                           }]
                       }
                   }
               }],
               "outputs": {
                   "cluster": {
                       "type": "object",
                       "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                   }
               }
           },
           "mode": "incremental",
           "Parameters": {
               "clusterName": {
                   "value": "newclustername"
               },
               "clusterType": {
                   "value": "hadoop"
               },
               "clusterStorageAccountName": {
                   "value": "newstoragename"
               },
               "clusterLoginUserName": {
                   "value": "admin"
               },
               "clusterLoginPassword": {
                   "value": "changeme"
               },
               "sshUserName": {
                   "value": "sshuser"
               },
               "sshPassword": {
                   "value": "changeme"
               }
           }
       }
   }
   ```

Bu örnek, bu belgedeki adımlarda kullanılır. **Parametreler** bölümündeki örnek *değerleri* kümenizin değerleriyle değiştirin.

> [!IMPORTANT]  
> Şablon, bir HDInsight kümesi için varsayılan alt düğüm sayısını (4) kullanır. 32'den fazla işçi düğümü planlıyorsanız, en az 8 çekirdekli ve 14 GB ram'lik bir baş düğümü boyutu seçmeniz gerekir.
>
> Düğüm boyutları ve ilişkili maliyetler hakkında daha fazla bilgi için bkz. [HDInsight fiyatlandırması](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="sign-in-to-your-azure-subscription"></a>Azure aboneliğinizde oturum açın

[Azure CLI ile başlatılan get'te](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) belgelenen adımları `az login` izleyin ve komutu kullanarak aboneliğinize bağlanın.

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

> [!NOTE]  
> Bu adımlar, kaynaklar belgesine erişmek için bir hizmet [ilkesi oluşturmak için Azure CLI'yi kullan'ın](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md) parola bölümüyle birlikte Hizmet Oluştur *ilkesinin* kısaltılmış bir sürümüdür. Bu adımlar, Azure REST API'sinin kimliğini doğrulamak için kullanılan bir hizmet ilkesi oluşturur.

1. Komut satırından Azure aboneliklerinizi listelemek için aşağıdaki komutu kullanın.

   ```azurecli
   az account list --query '[].{Subscription_ID:id,Tenant_ID:tenantId,Name:name}'  --output table
   ```

    Listede, kullanmak istediğiniz aboneliği seçin ve **Subscription_ID** ve __Tenant_ID__ sütunlarına dikkat edin. Bu değerleri kaydedin.

2. Azure Etkin Dizini'nde bir uygulama oluşturmak için aşağıdaki komutu kullanın.

   ```azurecli
   az ad app create --display-name "exampleapp" --homepage "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your password> --query 'appId'
   ```

    `--display-name`", ve `--homepage` `--identifier-uris` kendi değerleriniz için" değerlerini değiştirin. Yeni Active Directory girişi için bir parola sağlayın.

   > [!NOTE]  
   > `--home-page` Ve `--identifier-uris` değerleri internet üzerinde barındırılan gerçek bir web sayfası başvuru gerekmez. Benzersiz URI'ler olmalılar.

   Bu komuttan döndürülen değer, yeni uygulamanın __Uygulama Kimliği'dir.__ Bu değeri kaydedin.

3. **App ID'yi**kullanarak bir hizmet sorumlusu oluşturmak için aşağıdaki komutu kullanın.

   ```azurecli
   az ad sp create --id <App ID> --query 'objectId'
   ```

     Bu komuttan döndürülen değer __Nesne Kimliği'dir.__ Bu değeri kaydedin.

4. **Nesne Kimliği** değerini kullanarak Hizmet sorumlusuna **Sahip** rolünü atayın. Daha önce aldığınız **abonelik kimliğini** kullanın.

   ```azurecli
   az role assignment create --assignee <Object ID> --role Owner --scope /subscriptions/<Subscription ID>/
   ```

## <a name="get-an-authentication-token"></a>Kimlik doğrulama belirteci alın

Kimlik doğrulama belirteci almak için aşağıdaki komutu kullanın:

```bash
curl -X "POST" "https://login.microsoftonline.com/$TENANTID/oauth2/token" \
-H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "client_id=$APPID" \
--data-urlencode "grant_type=client_credentials" \
--data-urlencode "client_secret=$PASSWORD" \
--data-urlencode "resource=https://management.azure.com/"
```

Set `$TENANTID` `$APPID`, `$PASSWORD` , ve daha önce elde edilen veya kullanılan değerleri.

Bu istek başarılı olursa, 200 seri yanıt alırsınız ve yanıt gövdesi bir JSON belgesi içerir.

Bu istek tarafından döndürülen JSON belgesi **access_token**adlı bir öğe içerir. **access_token** değeri, REST API'deki kimlik doğrulama istekleri için kullanılır.

```json
{
    "token_type":"Bearer",
    "expires_in":"3599",
    "expires_on":"1463409994",
    "not_before":"1463406094",
    "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
}
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu oluşturmak için aşağıdakileri kullanın.

* Hizmet `$SUBSCRIPTIONID` ilkesi oluşturulurken alınan abonelik kimliğine ayarlayın.
* Önceki `$ACCESSTOKEN` adımda alınan erişim belirteci için ayarlayın.
* Kaynak `DATACENTERLOCATION` grubunu ve kaynakları oluşturmak istediğiniz veri merkeziyle değiştirin. Örneğin, 'Güney Orta ABD'.
* Bu `$RESOURCEGROUPNAME` grup için kullanmak istediğiniz ada ayarlayın:

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME?api-version=2015-01-01" \
    -H "Authorization: Bearer $ACCESSTOKEN" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DATACENTERLOCATION"
}'
```

Bu istek başarılı olursa, 200 seri yanıt alırsınız ve yanıt gövdesi grup hakkında bilgi içeren bir JSON belgesi içerir. Öğe `"provisioningState"` bir değer `"Succeeded"`içerir.

## <a name="create-a-deployment"></a>Dağıtım oluşturma

Şablonu kaynak grubuna dağıtmak için aşağıdaki komutu kullanın.

* Bu `$DEPLOYMENTNAME` dağıtım için kullanmak istediğiniz ada ayarlayın.

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [!NOTE]  
> Şablonu bir dosyaya kaydettiyseniz, aşağıdaki komutu `-d "{ template and parameters}"`kullanabilirsiniz:
>
> `--data-binary "@/path/to/file.json"`

Bu istek başarılı olursa, 200 seri yanıt alırsınız ve yanıt gövdesi dağıtım işlemi yle ilgili bilgileri içeren bir JSON belgesi içerir.

> [!IMPORTANT]  
> Dağıtım gönderildi, ancak tamamlanmadı. Dağıtımın tamamlanması genellikle 15 dakika civarında birkaç dakika sürebilir.

## <a name="check-the-status-of-a-deployment"></a>Dağıtım durumunu denetleme

Dağıtım durumunu denetlemek için aşağıdaki komutu kullanın:

```bash
curl -X "GET" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json"
```

Bu komut, dağıtım işlemi hakkında bilgi içeren bir JSON belgesi döndürür. Öğe `"provisioningState"` dağıtım durumunu içerir. Bu öğe `"Succeeded"`, bir değer içeriyorsa, dağıtım başarıyla tamamlandı.

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](./hdinsight-hadoop-customize-cluster-linux.md#access-control).

## <a name="next-steps"></a>Sonraki adımlar

Bir HDInsight kümesini başarıyla oluşturduğunuza göre, kümenizle nasıl çalışacağınızı öğrenmek için aşağıdakileri kullanın.

### <a name="apache-hadoop-clusters"></a>Apaçi Hadoop kümeleri

* [HDInsight ile Apache Hive'ı kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight ile MapReduce'ı kullanın](hadoop/hdinsight-use-mapreduce.md)

### <a name="apache-hbase-clusters"></a>Apache HBase kümeleri

* [HDInsight'ta Apache HBase ile başlayın](hbase/apache-hbase-tutorial-get-started-linux.md)
* [HDInsight'ta Apache HBase için Java uygulamaları geliştirin](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="apache-storm-clusters"></a>Apaçi Fırtına kümeleri

* [HDInsight'ta Apache Storm için Java topolojileri geliştirin](storm/apache-storm-develop-java-topology.md)
* [HDInsight'ta Apache Storm'da Python bileşenlerini kullanma](storm/apache-storm-develop-python-topology.md)
* [HDInsight'ta Apache Storm ile topolojileri dağıtın ve izleyin](storm/apache-storm-deploy-monitor-topology-linux.md)
