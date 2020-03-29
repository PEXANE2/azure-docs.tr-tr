---
title: Bölgeler arasında güvenli dağıtım - Azure Dağıtım Yöneticisi
description: Azure Dağıtım Yöneticisi ile bir hizmetin birçok bölgeye nasıl dağıtılangerektiğini açıklar. Tüm bölgelere gönderilmeden önce dağıtımınızın kararlılığını doğrulamak için güvenli dağıtım uygulamalarını gösterir.
ms.topic: conceptual
ms.date: 11/21/2019
ms.custom: seodec18
ms.openlocfilehash: 424cd79a6c63200e1f101cf178b1fd2c9083161e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76152536"
---
# <a name="enable-safe-deployment-practices-with-azure-deployment-manager-public-preview"></a>Azure Dağıtım Yöneticisi (Genel önizleme) ile güvenli dağıtım uygulamalarını etkinleştirin

Hizmetinizi birçok bölgeye dağıtmak ve her bölgede beklendiği gibi çalışmasını sağlamak için, hizmetin aşamalı bir ürün dağıtımını koordine etmek için Azure Dağıtım Yöneticisi'ni kullanabilirsiniz. Herhangi bir Azure dağıtımında olduğu gibi, [Kaynak Yöneticisi şablonlarında](template-syntax.md)hizmetinizin kaynaklarını tanımlarsınız. Şablonları oluşturduktan sonra, hizmetinizin topolojisini ve nasıl kullanıma sunulması gerektiğini açıklamak için Dağıtım Yöneticisi'ni kullanırsınız.

Deployment Manager, Kaynak Yöneticisi'nin bir özelliğidir. Dağıtım sırasında yeteneklerinizi genişletir. Birkaç bölgeye dağıtılması gereken karmaşık bir hizmetinolduğunda Dağıtım Yöneticisi'ni kullanın. Hizmetinizi aşamalı kullanıma sunarak, tüm bölgelere dağıtılmadan önce olası sorunları bulabilirsiniz. Aşamalı bir kullanıma yönelik ek önlemlere ihtiyacınız yoksa, Kaynak Yöneticisi için standart [dağıtım seçeneklerini](deploy-portal.md) kullanın. Deployment Manager, sürekli tümleştirme ve sürekli teslim (CI/CD) teklifleri gibi Kaynak Yöneticisi dağıtımlarını destekleyen tüm mevcut üçüncü taraf araçlarıyla sorunsuz bir şekilde tümleştirir.

Azure Dağıtım Yöneticisi önizlemede. [Geri bildirim](https://aka.ms/admfeedback)sağlayarak özelliği geliştirmemize yardımcı olun.

Deployment Manager'ı kullanmak için dört dosya oluşturmanız gerekir:

* Topoloji şablonu
* Kullanıma alma şablonu
* Topoloji için parametre dosyası
* Kullanıma alma için parametre dosyası

Kullanıma alma şablonuna dağıtmadan önce topoloji şablonuna dağıtın.

Ek kaynaklar:

- [Azure Dağıtım Yöneticisi REST API başvurusu.](https://docs.microsoft.com/rest/api/deploymentmanager/)
- [Öğretici: Kaynak Yöneticisi şablonlarıyla Azure Dağıtım Yöneticisi'ni kullanın.](./deployment-manager-tutorial.md)
- [Öğretici: Azure Dağıtım Yöneticisi'nde sistem durumu denetimini kullanın.](./deployment-manager-tutorial-health-check.md)
- [Azure Dağıtım Yöneticisi örneği.](https://github.com/Azure-Samples/adm-quickstart)

## <a name="identity-and-access"></a>Kimlik ve erişim

Deployment Manager [ile, kullanıcı tarafından atanan yönetilen kimlik](../../active-directory/managed-identities-azure-resources/overview.md) dağıtım eylemlerini gerçekleştirir. Dağıtımınızı başlatmadan önce bu kimliği oluşturursunuz. Hizmeti dağıttığınız aboneye erişimi ve dağıtımı tamamlamak için yeterli izni olmalıdır. Roller aracılığıyla verilen eylemler hakkında bilgi için [Azure kaynakları için Yerleşik rollere](../../role-based-access-control/built-in-roles.md)bakın.

Kimlik, kullanıma sunulması ile aynı konumda olmalıdır.

## <a name="topology-template"></a>Topoloji şablonu

Topoloji şablonu, hizmetinizi oluşturan Azure kaynaklarını ve bunları nerede dağıtacağınız açıklanır. Aşağıdaki resim, örnek bir hizmet için topolojiyi gösterir:

![Hizmet topolojisinden hizmetlere ve hizmet birimlerine hiyerarşi](./media/deployment-manager-overview/service-topology.png)

Topoloji şablonu aşağıdaki kaynakları içerir:

* Yapı kaynağı - Kaynak Yöneticisi şablonlarınızın ve parametrelerinizin depolandığı
* Hizmet topolojisi - artifakı kaynağına işaret
  * Hizmetler - konum ve Azure abonelik kimliğini belirtir
    * Hizmet birimleri - kaynak grubu, dağıtım modu ve şablon ve parametre dosyasına giden yolu belirtir

Her düzeyde neler olduğunu anlamak için, hangi değerleri sağladığınızı görmek yararlıdır.

![Her düzey için değerler](./media/deployment-manager-overview/topology-values.png)

### <a name="artifact-source-for-templates"></a>Şablonlar için yapı kaynağı

Topoloji şablonunuzda, şablonları ve parametrelerdosyalarını tutan bir yapı kaynağı oluşturursunuz. Yapı kaynağı dağıtım için dosyaları çekmenin bir yoludur. Bu makalede daha sonra ikili ler için başka bir eser kaynağı görürsünüz.

Aşağıdaki örnek, yapı kaynağının genel biçimini gösterir.

```json
{
  "type": "Microsoft.DeploymentManager/artifactSources",
  "apiVersion": "2018-09-01-preview",
  "name": "<artifact-source-name>",
  "location": "<artifact-source-location>",
  "properties": {
    "sourceType": "AzureStorage",
    "artifactRoot": "<root-folder-for-templates>",
    "authentication": {
      "type": "SAS",
      "properties": {
        "sasUri": "<SAS-URI-for-storage-container>"
      }
    }
  }
}
```

Daha fazla bilgi için [bkz: artifactSources şablonu başvurusu.](/azure/templates/Microsoft.DeploymentManager/artifactSources)

### <a name="service-topology"></a>Hizmet topolojisi

Aşağıdaki örnekte, hizmet topolojisi kaynağının genel biçimi gösterilmektedir. Şablonları ve parametre dosyalarını tutan yapı kaynağının kaynak kimliğini sağlarsınız. Hizmet topolojisi tüm hizmet kaynaklarını içerir. Yapı kaynağının kullanılabilir olduğundan emin olmak için, hizmet topolojisi buna bağlıdır.

```json
{
  "type": "Microsoft.DeploymentManager/serviceTopologies",
  "apiVersion": "2018-09-01-preview",
  "name": "<topology-name>",
  "location": "<topology-location>",
  "dependsOn": [
    "<artifact-source>"
  ],
  "properties": {
    "artifactSourceId": "<resource-ID-artifact-source>"
  },
  "resources": [
    {
      "type": "services",
        ...
        }
    ]
}
```

Daha fazla bilgi için [serviceTopologies şablonu başvurusuna](/azure/templates/Microsoft.DeploymentManager/serviceTopologies)bakın.

### <a name="services"></a>Hizmetler

Aşağıdaki örnekte, hizmet kaynağının genel biçimi gösterilmektedir. Her hizmette, hizmetinizi dağıtmak için kullanılacak konum ve Azure abonelik kimliği sağlarsınız. Birkaç bölgeye dağıtmak için her bölge için bir hizmet tanımlarsınız. Hizmet servis topolojisi bağlıdır.

```json
{
  "type": "services",
  "apiVersion": "2018-09-01-preview",
  "name": "<service-name>",
  "location": "<service-location>",
  "dependsOn": [
      "<service-topology>"
  ],
  "properties": {
    "targetSubscriptionId": "<subscription-ID>",
    "targetLocation": "<location-of-deployed-service>"
  },
  "resources": [
    {
      "type": "serviceUnits",
            ...
        }
    ]
}
```

Daha fazla bilgi için [hizmetler şablonu başvurusuna](/azure/templates/Microsoft.DeploymentManager/serviceTopologies/services)bakın.

### <a name="service-units"></a>Hizmet Birimleri

Aşağıdaki örnekte, hizmet birimleri kaynağının genel biçimi gösterilmektedir. Her hizmet biriminde kaynak grubunu, dağıtım için kullanılacak [dağıtım modunu](deployment-modes.md) ve şablon ve parametre dosyasına giden yolu belirtirsiniz. Şablon ve parametreler için göreli bir yol belirtirseniz, tam yol yapılı rekler kaynağındaki kök klasöründen oluşturulur. Şablon ve parametreler için mutlak bir yol belirtebilirsiniz, ancak sürümlerinizi kolayca sürümleme yeteneğini kaybedersiniz. Servis birimi hizmete bağlıdır.

```json
{
  "type": "serviceUnits",
  "apiVersion": "2018-09-01-preview",
  "name": "<service-unit-name>",
  "location": "<service-unit-location>",
  "dependsOn": [
    "<service>"
  ],
  "tags": {
    "serviceType": "Service West US Web App"
  },
  "properties": {
    "targetResourceGroup": "<resource-group-name>",
    "deploymentMode": "Incremental",
    "artifacts": {
      "templateArtifactSourceRelativePath": "<relative-path-to-template>",
      "parametersArtifactSourceRelativePath": "<relative-path-to-parameter-file>"
    }
  }
}
```

Her şablon, dağıtmak istediğiniz ilgili kaynakları bir adımda içermelidir. Örneğin, bir hizmet biriminde hizmetin ön ucu için tüm kaynakları dağıtan bir şablon olabilir.

Daha fazla bilgi için [serviceUnits şablon ubaşvurusuna](/azure/templates/Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits)bakın.

## <a name="rollout-template"></a>Kullanıma alma şablonu

Kullanıma alma şablonu, hizmetinizi dağıtırken atılacak adımları açıklar. Kullanılacak hizmet topolojisini belirtir ve hizmet birimlerini dağıtma sırasını tanımlarsınız. Dağıtım için ikili leri depolamak için bir yapı kaynağı içerir. Kullanıma alma şablonunuzda aşağıdaki hiyerarşiyi tanımlarsınız:

* Artefakt kaynağı
* Adım
* Rollout
  * Adım grupları
    * Dağıtım işlemleri

Aşağıdaki resim, kullanıma alma şablonunun hiyerarşisini gösterir:

![Kullanıma sunulmasından adımlara hiyerarşi](./media/deployment-manager-overview/Rollout.png)

Her kullanıma birçok adım grubu olabilir. Her adım grubunda, hizmet topolojisindeki bir servis birimine işaret eden bir dağıtım işlemi vardır.

### <a name="artifact-source-for-binaries"></a>İkili için eser kaynağı

Kullanıma alma şablonunda, hizmete dağıtmanız gereken ikililer için bir yapı kaynağı oluşturursunuz. Bu yapı kaynağı, komut dosyalarını, web sayfalarını, derlenmiş kodu veya hizmetinizin gerektirdiği diğer dosyaları içerse bile, [şablonlar için yapı kaynağına](#artifact-source-for-templates)benzer.

### <a name="steps"></a>Adımlar

Dağıtım işleminden önce veya sonra gerçekleştirmek için bir adım tanımlayabilirsiniz. Şu anda `wait` yalnızca adım ve 'healthCheck' adımı mevcuttur.

Bekleme adımı devam etmeden önce dağıtımı duraklatıyor. Bir sonraki hizmet birimini dağıtmadan önce hizmetinizin beklendiği gibi çalıştığını doğrulamanızı sağlar. Aşağıdaki örnek, bekleme adımının genel biçimini gösterir.

```json
{
    "type": "Microsoft.DeploymentManager/steps",
    "apiVersion": "2018-09-01-preview",
    "name": "waitStep",
    "location": "<step-location>",
    "properties": {
        "stepType": "wait",
        "attributes": {
          "duration": "PT1M"
        }
    }
},
```

Özellik, [ISO 8601 standardını](https://en.wikipedia.org/wiki/ISO_8601#Durations)kullanır. Önceki örnekte bir dakikalık bekleme belirtilmesi yer alıyor.

Sistem durumu denetimi adımı hakkında daha fazla bilgi için [bkz.](./deployment-manager-health-check.md) [Tutorial: Use health check in Azure Deployment Manager](./deployment-manager-tutorial-health-check.md)

Daha fazla bilgi için [adımlar şablonu başvurusuna](/azure/templates/Microsoft.DeploymentManager/steps)bakın.

### <a name="rollouts"></a>Piyasaya çıkarmalar

Yapı kaynağının kullanılabilir olduğundan emin olmak için, kullanıma sunulması buna bağlıdır. Kullanıma alma, dağıtılan her hizmet birimi için adım gruplarını tanımlar. Dağıtımdan önce veya sonra yapılacak eylemleri tanımlayabilirsiniz. Örneğin, hizmet birimi dağıtıldıktan sonra dağıtım beklemesini belirtebilirsiniz. Adım gruplarının sırasını tanımlayabilirsiniz.

Kimlik nesnesi, dağıtım eylemlerini gerçekleştiren [kullanıcı tarafından atanan yönetilen kimliği](#identity-and-access) belirtir.

Aşağıdaki örnek, kullanıma genel biçimini gösterir.

```json
{
  "type": "Microsoft.DeploymentManager/rollouts",
  "apiVersion": "2018-09-01-preview",
  "name": "<rollout-name>",
  "location": "<rollout-location>",
  "Identity": {
    "type": "userAssigned",
    "identityIds": [
      "<managed-identity-ID>"
    ]
  },
  "dependsOn": [
    "<artifact-source>"
  ],
  "properties": {
    "buildVersion": "1.0.0.0",
    "artifactSourceId": "<artifact-source-ID>",
    "targetServiceTopologyId": "<service-topology-ID>",
    "stepGroups": [
      {
        "name": "stepGroup1",
        "dependsOnStepGroups": ["<step-group-name>"],
        "preDeploymentSteps": ["<step-ID>"],
        "deploymentTargetId":
          "<service-unit-ID>",
        "postDeploymentSteps": ["<step-ID>"]
      },
            ...
        ]
    }
}
```

Daha fazla bilgi için [rollouts şablonu başvurusuna](/azure/templates/Microsoft.DeploymentManager/rollouts)bakın.

## <a name="parameter-file"></a>Parametre dosyası

İki parametre dosyası oluşturursunuz. Bir parametre dosyası, hizmet topolojisi dağıtılırken kullanılır ve diğeri kullanıma sunma dağıtımı için kullanılır. Her iki parametre dosyasında da aynı olduğundan emin olmanız gereken bazı değerler vardır.

## <a name="containerroot-variable"></a>containerRoot değişkeni

Sürümlü dağıtımlarla, yapılarınızın yolu her yeni sürümle birlikte değişir. Bir dağıtımı ilk çalıştırdığınızda yol `https://<base-uri-blob-container>/binaries/1.0.0.0`. İkinci sefer olabilir. `https://<base-uri-blob-container>/binaries/1.0.0.1` Deployment Manager `$containerRoot` değişkeni kullanarak geçerli dağıtım için doğru kök yolu elde kolaylaştırır. Bu değer her sürümle değişir ve dağıtımdan önce bilinmemektedir.

Azure `$containerRoot` kaynaklarını dağıtmak için şablon için parametre dosyasındaki değişkeni kullanın. Dağıtım sırasında, bu değişken kullanıma alınan gerçek değerlerle değiştirilir.

Örneğin, ürün lansmanı sırasında ikili yapılar için bir yapı kaynağı oluşturursunuz.

```json
{
  "type": "Microsoft.DeploymentManager/artifactSources",
  "apiVersion": "2018-09-01-preview",
  "name": "[variables('rolloutArtifactSource').name]",
  "location": "[parameters('azureResourceLocation')]",
  "properties": {
    "sourceType": "AzureStorage",
      "artifactRoot": "[parameters('binaryArtifactRoot')]",
      "authentication" :
        {
          "type": "SAS",
          "properties": {
            "sasUri": "[parameters('artifactSourceSASLocation')]"
          }
        }
  }
},
```

Dikkat `artifactRoot` edin ve `sasUri` özellikleri. Artefakt kökü gibi `binaries/1.0.0.0`bir değere ayarlanabilir. SAS URI, erişim için bir SAS belirteci ile depolama kabına URI'dir. Deployment Manager `$containerRoot` değişkenin değerini otomatik olarak kurar. Bu değerleri biçimde `<container>/<artifactRoot>`birleştirir.

Şablon ve parametre dosyanızın sürümlü ikilileri almak için doğru yolu bilmesi gerekir. Örneğin, bir web uygulaması için dosyaları dağıtmak için, $containerRoot değişkeni ile aşağıdaki parametre dosyasını oluşturun. İlk bir kaçış karakteri`\\`olduğu için yol için iki backslashes ( ) kullanmanız gerekir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "deployPackageUri": {
      "value": "$containerRoot\\helloWorldWebAppWUS.zip"
    }
  }
}
```

Ardından, şablonunuzdaki bu parametreyi kullanın:

```json
{
  "name": "MSDeploy",
  "apiVersion": "2015-08-01",
  "type": "extensions",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Web/sites/', parameters('WebAppName'))]"
  ],
  "tags": {
    "displayName": "WebAppMSDeploy"
  },
  "properties": {
    "packageUri": "[parameters('deployPackageURI')]"
  }
}
```

Sürümdeki dağıtımları, yeni klasörler oluşturarak ve kullanıma sunulması sırasında bu kökten geçerek yönetirsiniz. Yol, kaynakları dağıtan şablona akar.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Dağıtım Yöneticisi'ni öğrendiniz. Dağıtım Yöneticisi ile nasıl dağıtılacağınızı öğrenmek için bir sonraki makaleye devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Kaynak Yöneticisi şablonlarıyla Azure Dağıtım Yöneticisi'ni kullanma](./deployment-manager-tutorial.md)
>
> [Hızlı başlangıç: Azure Dağıtım Yöneticisi'ni sadece birkaç dakika içinde deneyin](https://github.com/Azure-Samples/adm-quickstart)
