---
title: Azure Kaynak Yöneticisi ile dağıtma ve yükseltme
description: Azure Kaynak Yöneticisi şablonu kullanarak uygulamaları ve hizmetleri Hizmet Dokusu kümesine nasıl dağıtabileceğinizi öğrenin.
ms.topic: conceptual
ms.date: 12/06/2017
ms.openlocfilehash: a2dfe54bf2c6b4fa8814f10c10576a73727a7417
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75610259"
---
# <a name="manage-applications-and-services-as-azure-resource-manager-resources"></a>Azure Kaynak Yöneticisi kaynakları olarak uygulamaları ve hizmetleri yönetme

Azure Kaynak Yöneticisi aracılığıyla uygulamaları ve hizmetleri Hizmet Kumaşı kümenize dağıtabilirsiniz. Bu, kümenin hazır olmasını bekledikten sonra PowerShell veya CLI üzerinden uygulamaları dağıtmak ve yönetmek yerine, artık JSON'daki uygulamaları ve hizmetleri ifade edebilir ve bunları kümenizle aynı Kaynak Yöneticisi şablonunda dağıtabilirsiniz. Uygulama kaydı, sağlama ve dağıtım işlemi tek bir adımda gerçekleşir.

Bu, kümenizde gereksinim duyduğunuz herhangi bir kurulum, yönetim veya küme yönetimi uygulamalarını dağıtmanız için önerilen yoldur. Buna, Diğer uygulamalar veya hizmetler dağıtılmadan önce kümenizde çalışması gereken [Yama Düzenleme Uygulaması,](service-fabric-patch-orchestration-application.md)İzleme Örgütleri veya uygulamaları içerir. 

Uygulanabilir olduğunda, uygulamalarınızı geliştirmek için Kaynak Yöneticisi kaynakları olarak yönetin:
* Denetim izi: Kaynak Yöneticisi her işlemi denetler ve bu uygulamalarda ve kümenizde yapılan değişiklikleri izlemenize yardımcı olabilecek ayrıntılı bir *Etkinlik Günlüğü* tutar.
* Rol tabanlı erişim denetimi (RBAC): Kümelere ve kümeye dağıtılan uygulamalara erişimi yönetme aynı Kaynak Yöneticisi şablonu üzerinden yapılabilir.
* Azure Kaynak Yöneticisi (Azure portalı üzerinden), küme ve kritik uygulama dağıtımlarınızı yönetmek için tek durak haline gelir.

Aşağıdaki parçacık, şablon aracılığıyla yönetilebilen farklı kaynak türlerini gösterir:

```json
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications/services",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
    "location": "[variables('clusterLocation')]"
}
```


## <a name="add-a-new-application-to-your-resource-manager-template"></a>Kaynak Yöneticisi şablonunuza yeni bir uygulama ekleme

1. Kümenizin Kaynak Yöneticisi şablonunu dağıtımiçin hazırlayın. Bu konuda daha fazla bilgi için [Azure Kaynak Yöneticisi'ni kullanarak Hizmet Dokusu Oluştur kümesine](service-fabric-cluster-creation-via-arm.md) bakın.
2. Kümede dağıtmayı planladığınız bazı uygulamaları düşünün. Her zaman diğer uygulamalar üzerinde bağımlılıklar alabilir çalışan olacak herhangi bir var mı? Herhangi bir küme yönetim veya kurulum uygulaması dağıtmayı planlıyor musunuz? Bu tür uygulamalar, yukarıda belirtildiği gibi, en iyi Kaynak Yöneticisi şablonu aracılığıyla yönetilir. 
3. Bu şekilde hangi uygulamaların dağıtılmasını istediğinizi anladıktan sonra, uygulamaların paketlenmeli, sıkıştırılmalı ve dosya paylaşımına konmalıdır. Azure Kaynak Yöneticisi'nin dağıtım sırasında tüketmesi için paylaşımın BIR REST bitiş noktası üzerinden erişilmesi gerekir.
4. Kaynak Yöneticisi şablonunuzda, küme bildirgenizin altında her uygulamanın özelliklerini açıklayın. Bu özellikler çoğaltma veya örnek sayısı ve kaynaklar (diğer uygulamalar veya hizmetler) arasındaki bağımlılık zincirlerini içerir. Kapsamlı özelliklerin bir listesi [için, REST API Swagger Spec](https://aka.ms/sfrpswaggerspec)bakın. Bunun Uygulama veya Hizmet bildirimlerinin yerini alamadığını, kümenin Kaynak Yöneticisi şablonunun bir parçası olarak içinde bulunanların bazılarını açıkladığını unutmayın. Aşağıda, *uygulama1'in* bir parçası olarak bir durum hizmeti Service1 ve *Application1*bir stateful *serviceService2* dağıtmayı içeren bir örnek şablon uymaktadır:

   ```json
   {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "clusterName": {
        "type": "string",
        "defaultValue": "Cluster",
        "metadata": {
          "description": "Name of your cluster - Between 3 and 23 characters. Letters and numbers only."
        }
      },
      "applicationTypeName": {
        "type": "string",
        "defaultValue": "ApplicationType",
        "metadata": {
          "description": "The application type name."
        }
      },
      "applicationTypeVersion": {
        "type": "string",
        "defaultValue": "1",
        "metadata": {
          "description": "The application type version."
        }
      },
      "appPackageUrl": {
        "type": "string",
        "metadata": {
          "description": "The URL to the application package sfpkg file."
        }
      },
      "applicationName": {
        "type": "string",
        "defaultValue": "Application1",
        "metadata": {
          "description": "The name of the application resource."
        }
      },
      "serviceName": {
        "type": "string",
        "defaultValue": "Application1~Service1",
        "metadata": {
          "description": "The name of the service resource in the format of {applicationName}~{serviceName}."
        }
      },
      "serviceTypeName": {
        "type": "string",
        "defaultValue": "Service1Type",
        "metadata": {
          "description": "The name of the service type."
        }
      },
      "serviceName2": {
        "type": "string",
        "defaultValue": "Application1~Service2",
        "metadata": {
          "description": "The name of the service resource in the format of {applicationName}~{serviceName}."
        }
      },
      "serviceTypeName2": {
        "type": "string",
        "defaultValue": "Service2Type",
        "metadata": {
          "description": "The name of the service type."
        }
      }
    },
    "variables": {
      "clusterLocation": "[resourcegroup().location]"
    },
    "resources": [
      {
        "apiVersion": "2019-03-01",
        "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [],
        "properties": {
          "provisioningState": "Default"
        }
      },
      {
        "apiVersion": "2019-03-01",
        "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applicationTypes/', parameters('applicationTypeName'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "appPackageUrl": "[parameters('appPackageUrl')]"
        }
      },
      {
        "apiVersion": "2019-03-01",
        "type": "Microsoft.ServiceFabric/clusters/applications",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applicationTypes/', parameters('applicationTypeName'), '/versions/', parameters('applicationTypeVersion'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "typeName": "[parameters('applicationTypeName')]",
          "typeVersion": "[parameters('applicationTypeVersion')]",
          "parameters": {},
          "upgradePolicy": {
            "upgradeReplicaSetCheckTimeout": "01:00:00.0",
            "forceRestart": "false",
            "rollingUpgradeMonitoringPolicy": {
              "healthCheckWaitDuration": "00:02:00.0",
              "healthCheckStableDuration": "00:05:00.0",
              "healthCheckRetryTimeout": "00:10:00.0",
              "upgradeTimeout": "01:00:00.0",
              "upgradeDomainTimeout": "00:20:00.0"
            },
            "applicationHealthPolicy": {
              "considerWarningAsError": "false",
              "maxPercentUnhealthyDeployedApplications": "50",
              "defaultServiceTypeHealthPolicy": {
                "maxPercentUnhealthyServices": "50",
                "maxPercentUnhealthyPartitionsPerService": "50",
                "maxPercentUnhealthyReplicasPerPartition": "50"
              }
            }
          }
        }
      },
      {
        "apiVersion": "2019-03-01",
        "type": "Microsoft.ServiceFabric/clusters/applications/services",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applications/', parameters('applicationName'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "serviceKind": "Stateless",
          "serviceTypeName": "[parameters('serviceTypeName')]",
          "instanceCount": "-1",
          "partitionDescription": {
            "partitionScheme": "Singleton"
          },
          "correlationScheme": [],
          "serviceLoadMetrics": [],
          "servicePlacementPolicies": []
        }
      },
      {
        "apiVersion": "2019-03-01",
        "type": "Microsoft.ServiceFabric/clusters/applications/services",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName2'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applications/', parameters('applicationName'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "serviceKind": "Stateful",
          "serviceTypeName": "[parameters('serviceTypeName2')]",
          "targetReplicaSetSize": "3",
          "minReplicaSetSize": "2",
          "replicaRestartWaitDuration": "00:01:00.0",
          "quorumLossWaitDuration": "00:02:00.0",
          "standByReplicaKeepDuration": "00:00:30.0",
          "partitionDescription": {
            "partitionScheme": "UniformInt64Range",
            "count": "5",
            "lowKey": "1",
            "highKey": "5"
          },
          "hasPersistedState": "true",
          "correlationScheme": [],
          "serviceLoadMetrics": [],
          "servicePlacementPolicies": [],
          "defaultMoveCost": "Low"
        }
      }
    ]
   }
   ```

   > [!NOTE] 
   > *ApiVersion'un* `"2019-03-01"`. Bu şablon, küme zaten dağıtılmış olduğu sürece kümeden bağımsız olarak da dağıtılabilir.

5. Dağıtmak! 

## <a name="remove-service-fabric-resource-provider-application-resource"></a>Hizmet Kumaş Kaynak Sağlayıcı Uygulama kaynağını kaldırma
Aşağıdakiler uygulama paketinin kümeden un-provisioned olmasını tetikler ve bu kullanılan disk alanını temizler:
```powershell
Get-AzureRmResource -ResourceId /subscriptions/{sid}/resourceGroups/{rg}/providers/Microsoft.ServiceFabric/clusters/{cluster}/applicationTypes/{apptType}/versions/{version} -ApiVersion "2019-03-01" | Remove-AzureRmResource -Force -ApiVersion "2017-07-01-preview"
```
Microsoft.ServiceFabric/clusters/application'ı ARM şablonunuzdan kaldırmanız, Uygulamayı

>[!NOTE]
> Kaldırma işlemi tamamlandıktan sonra artık SFX veya ARM'da paket sürümünü görmemelisiniz. Uygulamanın birlikte yürüttüğü uygulama türü sürüm kaynağını silemezsiniz; ARM/SFRP bunu önleyecektir. Çalışan paketi unprovision çalışırsanız, SF çalışma süresi bunu engeller.


## <a name="manage-an-existing-application-via-resource-manager"></a>Kaynak Yöneticisi aracılığıyla varolan bir uygulamayı yönetme

Kümeniz zaten hazırsa ve Kaynak Yöneticisi kaynakları olarak yönetmek istediğiniz bazı uygulamalar uygulamaları kaldırmak ve yeniden dağıtmak yerine zaten dağıtılmışsa, uygulamaların alınması için aynı API'leri kullanarak bir PUT çağrısı kullanabilirsiniz Kaynak Yöneticisi kaynakları olarak kabul edilebistır. Daha fazla bilgi için [Service Fabric uygulama kaynak modeli nedir?](https://docs.microsoft.com/azure/service-fabric/service-fabric-concept-resource-model)

> [!NOTE]
> Bir küme yükseltmesinin sağlıksız uygulamaları yok sayması için müşteri "yükseltmeAçıklaması/healthPolicy" bölümünde "maxPercentUnhealthyApplications: 100" belirtebilir; tüm ayarları için ayrıntılı açıklamalar [Hizmet Kumaşlar REST API Küme Yükseltme İlkesi dokümantasyon](https://docs.microsoft.com/rest/api/servicefabric/sfrp-model-clusterupgradepolicy)bulunmaktadır.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer uygulamaları kümenize dağıtmak için [Service Fabric CLI](service-fabric-cli.md) veya [PowerShell'i](service-fabric-deploy-remove-applications.md) kullanın. 
* [Hizmet Kumaşı kümenizi yükseltin](service-fabric-cluster-upgrade.md)

