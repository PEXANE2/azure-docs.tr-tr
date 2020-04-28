---
title: Bölgeler arasında güvenli dağıtım-Azure Dağıtım Yöneticisi
description: Azure Dağıtım Yöneticisi ile birçok bölge üzerinden bir hizmetin nasıl dağıtılacağını açıklar. Tüm bölgelere geçmeden önce dağıtımınızın kararlılığını doğrulamak üzere güvenli dağıtım uygulamalarını gösterir.
ms.topic: conceptual
ms.date: 11/21/2019
ms.custom: seodec18
ms.openlocfilehash: 424cd79a6c63200e1f101cf178b1fd2c9083161e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76152536"
---
# <a name="enable-safe-deployment-practices-with-azure-deployment-manager-public-preview"></a>Azure Dağıtım Yöneticisi ile güvenli dağıtım uygulamalarını etkinleştirme (Genel Önizleme)

Hizmetinizi birçok bölgede dağıtmak ve her bölgede beklendiği gibi çalıştığından emin olmak için Azure Dağıtım Yöneticisi kullanarak hizmetin aşamalı bir dağıtımını koordine edebilirsiniz. Her türlü Azure dağıtımında olduğu gibi, hizmetinize yönelik kaynakları [Kaynak Yöneticisi şablonlarda](template-syntax.md)tanımlarsınız. Şablonları oluşturduktan sonra, hizmetinize yönelik topolojiyi ve bu hizmetin nasıl kullanıma alınacağını anlatmak için Dağıtım Yöneticisi kullanırsınız.

Dağıtım Yöneticisi bir Kaynak Yöneticisi özelliğidir. Dağıtım sırasında olanaklarınızı genişletir. Birkaç bölgeye dağıtılması gereken karmaşık bir hizmetiniz varsa Dağıtım Yöneticisi kullanın. Hizmetinizi aşamalı kullanıma sunarak, tüm bölgelere dağıtılmadan önce olası sorunları bulabilirsiniz. Hazırlanmış bir dağıtım için ek önlemler gerekmiyorsa, Kaynak Yöneticisi için standart [dağıtım seçeneklerini](deploy-portal.md) kullanın. Dağıtım Yöneticisi, sürekli tümleştirme ve sürekli teslim (CI/CD) teklifleri gibi Kaynak Yöneticisi dağıtımlarını destekleyen mevcut tüm üçüncü taraf araçlarla sorunsuz bir şekilde tümleşir.

Azure Dağıtım Yöneticisi önizlemededir. [Geri bildirim](https://aka.ms/admfeedback)sağlayarak özelliği geliştirmemize yardımcı olun.

Dağıtım Yöneticisi kullanmak için dört dosya oluşturmanız gerekir:

* Topoloji şablonu
* Piyasaya çıkma şablonu
* Topoloji için parametre dosyası
* Dağıtım için parametre dosyası

Dağıtım şablonunu dağıtmadan önce topoloji şablonunu dağıtırsınız.

Ek kaynaklar:

- [Azure Dağıtım Yöneticisi REST API başvurusu](https://docs.microsoft.com/rest/api/deploymentmanager/).
- [Öğretici: Azure Dağıtım Yöneticisi kaynak yöneticisi şablonlarıyla kullanın](./deployment-manager-tutorial.md).
- [Öğretici: Azure dağıtım Yöneticisi 'da sistem durumu denetimi kullanın](./deployment-manager-tutorial-health-check.md).
- [Azure dağıtım Yöneticisi örneği](https://github.com/Azure-Samples/adm-quickstart).

## <a name="identity-and-access"></a>Kimlik ve erişim

Dağıtım Yöneticisi, [Kullanıcı tarafından atanan yönetilen kimlik](../../active-directory/managed-identities-azure-resources/overview.md) dağıtım eylemlerini gerçekleştirir. Dağıtımınızı başlatmadan önce bu kimliği oluşturursunuz. Hizmetin dağıttığınız aboneliğe erişimi olması ve dağıtımı tamamlamaya yetecek izinleri olması gerekir. Roller aracılığıyla verilen eylemler hakkında daha fazla bilgi için bkz. [Azure kaynakları Için yerleşik roller](../../role-based-access-control/built-in-roles.md).

Kimliğin dağıtım ile aynı konumda bulunması gerekir.

## <a name="topology-template"></a>Topoloji şablonu

Topoloji şablonu, hizmetinizi oluşturan Azure kaynaklarını ve bunların nereye dağıtılacağını açıklar. Aşağıdaki görüntüde örnek bir hizmetin topolojisi gösterilmektedir:

![Hizmet topolojisinden hizmetlere hizmet birimlerine hiyerarşi](./media/deployment-manager-overview/service-topology.png)

Topoloji şablonu aşağıdaki kaynakları içerir:

* Yapıt kaynağı-Kaynak Yöneticisi şablonlarınızın ve parametrelerinin depolandığı yer
* Hizmet topolojisi-yapıt kaynağına işaret eder
  * Hizmetler-konumu ve Azure abonelik KIMLIĞINI belirtir
    * Hizmet birimleri-kaynak grubunu, Dağıtım modunu ve şablon ve parametre dosyasının yolunu belirtir

Her düzeyde ne olduğunu anlamak için hangi değerleri sağlayacağınızı görmeniz yararlı olur.

![Her düzey için değerler](./media/deployment-manager-overview/topology-values.png)

### <a name="artifact-source-for-templates"></a>Şablonlar için yapıt kaynağı

Topoloji şablonunuzda, şablon ve parametre dosyalarını tutan bir yapıt kaynağı oluşturursunuz. Yapıt kaynağı, dosyaları dağıtıma çekmek için bir yoldur. Bu makalenin ilerleyen kısımlarında ikili dosyalar için başka bir yapıt kaynağı görürsünüz.

Aşağıdaki örnekte, yapıt kaynağının genel biçimi gösterilmektedir.

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

Daha fazla bilgi için bkz. [Artifactsources şablon başvurusu](/azure/templates/Microsoft.DeploymentManager/artifactSources).

### <a name="service-topology"></a>Hizmet topolojisi

Aşağıdaki örnekte, hizmet topolojisi kaynağının genel biçimi gösterilmektedir. Şablon ve parametre dosyalarını tutan yapıt kaynağının kaynak KIMLIĞINI sağlarsınız. Hizmet topolojisi tüm hizmet kaynaklarını içerir. Yapıt kaynağının kullanılabilir olduğundan emin olmak için hizmet topolojisi buna bağlıdır.

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

Daha fazla bilgi için bkz. [Servicetopolojileri şablon başvurusu](/azure/templates/Microsoft.DeploymentManager/serviceTopologies).

### <a name="services"></a>Hizmetler

Aşağıdaki örnek, hizmetler kaynağının Genel biçimini gösterir. Her hizmette, hizmetinizi dağıtmak için kullanılacak konumu ve Azure abonelik KIMLIĞINI sağlarsınız. Çeşitli bölgelere dağıtım yapmak için her bölge için bir hizmet tanımlarsınız. Hizmet topolojiye bağlıdır.

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

Daha fazla bilgi için bkz. [Hizmetler şablon başvurusu](/azure/templates/Microsoft.DeploymentManager/serviceTopologies/services).

### <a name="service-units"></a>Hizmet Birimleri

Aşağıdaki örnekte, hizmet birimleri kaynağının genel biçimi gösterilmektedir. Her bir hizmet biriminde, kaynak grubunu, dağıtım için kullanılacak [Dağıtım modunu](deployment-modes.md) ve şablon ile parametre dosyasının yolunu belirtirsiniz. Şablon ve parametreler için göreli bir yol belirtirseniz, tam yol yapılar kaynağındaki kök klasörden oluşturulur. Şablon ve parametreler için mutlak bir yol belirtebilirsiniz, ancak yayınlarınızın kolayca sürümü için bu özelliği kaybedersiniz. Hizmet birimi hizmete bağlıdır.

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

Her şablon, tek bir adımda dağıtmak istediğiniz ilgili kaynakları içermelidir. Örneğin, bir hizmet birimi, hizmetinizin ön ucuna ait tüm kaynakları dağıtan bir şablona sahip olabilir.

Daha fazla bilgi için bkz. [Serviceunits şablon başvurusu](/azure/templates/Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits).

## <a name="rollout-template"></a>Piyasaya çıkma şablonu

Piyasaya çıkma şablonu, hizmetinizi dağıttığınızda yapmanız gereken adımları açıklar. Kullanılacak hizmet topolojisini belirtirsiniz ve hizmet birimlerinin dağıtım sırasını tanımlayabilirsiniz. Dağıtım için ikili dosyaları depolamak üzere bir yapıt kaynağı içerir. Dağıtım şablonunuzda, aşağıdaki hiyerarşiyi tanımlarsınız:

* Yapıt kaynağı
* Adım
* Çıkarma
  * Adım grupları
    * Dağıtım işlemleri

Aşağıdaki görüntüde, piyasaya çıkma şablonunun hiyerarşisi gösterilmektedir:

![Piyasaya adımla olan hiyerarşi](./media/deployment-manager-overview/Rollout.png)

Her bir dağıtım birçok adım grubuna sahip olabilir. Her adım grubu, hizmet topolojisinde bir hizmet birimini işaret eden bir dağıtım işlemine sahiptir.

### <a name="artifact-source-for-binaries"></a>İkililer için yapıt kaynağı

Dağıtım şablonunda, hizmete dağıtmanız gereken ikili dosyalar için bir yapıt kaynağı oluşturursunuz. Bu yapıt kaynağı [Şablonlar için yapıt kaynağına](#artifact-source-for-templates)benzer, ancak betikler, Web sayfaları, derlenmiş kod veya hizmetiniz için gereken diğer dosyaları içerir.

### <a name="steps"></a>Adımlar

Dağıtım işleminizi daha önce veya sonra gerçekleştirmek üzere bir adım tanımlayabilirsiniz. Şu anda yalnızca `wait` adım ve ' healthcheck ' adımı kullanılabilir.

Bekleme adımı devam etmeden önce dağıtımı duraklatır. Sonraki hizmet birimini dağıtılmadan önce hizmetinizin beklendiği gibi çalıştığını doğrulamanızı sağlar. Aşağıdaki örnek, bir bekleme adımının Genel biçimini gösterir.

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

Duration özelliği [ıso 8601 standardını](https://en.wikipedia.org/wiki/ISO_8601#Durations)kullanır. Yukarıdaki örnek bir dakikalık beklemeyi belirtir.

Sistem durumu denetimi adımı hakkında daha fazla bilgi için bkz. [Azure 'da sistem durumu tümleştirmesi dağıtımı dağıtım Yöneticisi](./deployment-manager-health-check.md) ve [öğretici: Azure dağıtım Yöneticisi 'da sistem durumu denetimi kullanma](./deployment-manager-tutorial-health-check.md).

Daha fazla bilgi için bkz. [adımlar şablon başvurusu](/azure/templates/Microsoft.DeploymentManager/steps).

### <a name="rollouts"></a>Piyasaya çıkarmalar

Yapıt kaynağının kullanılabilir olduğundan emin olmak için, dağıtım buna bağlıdır. Dağıtım, dağıtılan her bir hizmet birimi için adım gruplarını tanımlar. Dağıtımdan önce veya sonra gerçekleştirilecek eylemleri tanımlayabilirsiniz. Örneğin, hizmet birimi dağıtıldıktan sonra dağıtımın beklemesini belirtebilirsiniz. Adım gruplarının sırasını tanımlayabilirsiniz.

Identity nesnesi, dağıtım eylemlerini gerçekleştiren [Kullanıcı tarafından atanan yönetilen kimliği](#identity-and-access) belirtir.

Aşağıdaki örnekte, piyasaya çıkın genel biçimi gösterilmektedir.

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

Daha fazla bilgi için bkz. [piyasaya çıkarma Template Reference](/azure/templates/Microsoft.DeploymentManager/rollouts).

## <a name="parameter-file"></a>Parametre dosyası

İki parametre dosyası oluşturursunuz. Hizmet topolojisi dağıtımında bir parametre dosyası kullanılır ve diğeri dağıtım dağıtımı için kullanılır. Her iki parametre dosyasında da aynı olduğundan emin olmak için ihtiyaç duyduğunuz bazı değerler vardır.

## <a name="containerroot-variable"></a>containerRoot değişkeni

Sürümlü dağıtımlar ile, yapıtlarınızın yolu her yeni sürümle değişir. Bir dağıtımı ilk kez çalıştırdığınızda yol olabilir `https://<base-uri-blob-container>/binaries/1.0.0.0`. İkinci kez olabilir `https://<base-uri-blob-container>/binaries/1.0.0.1`. Dağıtım Yöneticisi, `$containerRoot` değişkeni kullanarak geçerli dağıtım için doğru kök yolu almayı basitleştirir. Bu değer her sürümle birlikte değişir ve dağıtımdan önce bilinmez.

Azure kaynaklarını `$containerRoot` dağıtmak için şablon için parametre dosyasındaki değişkenini kullanın. Dağıtım zamanında bu değişken, piyasaya geçen gerçek değerlerle değiştirilmiştir.

Örneğin, dağıtım sırasında ikili yapıtlar için bir yapıt kaynağı oluşturursunuz.

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

`artifactRoot` Ve `sasUri` özelliklerine dikkat edin. Yapıt kökü, gibi `binaries/1.0.0.0`bir değere ayarlanabilir. SAS URI 'si, erişim için SAS belirtecine sahip depolama kapsayıcılarınızın URI 'sidir. Dağıtım Yöneticisi, `$containerRoot` değişkenin değerini otomatik olarak oluşturur. Bu değerleri biçiminde `<container>/<artifactRoot>`birleştirir.

Şablon ve parametre dosyanızın sürümlenmiş ikilileri almak için doğru yolu bilmeleri gerekir. Örneğin, bir Web uygulaması için dosyaları dağıtmak için, $containerRoot değişkeniyle aşağıdaki parametre dosyasını oluşturun. İlk kaçış karakteri olduğu için yol`\\`için iki ters eğik çizgi () kullanmanız gerekir.

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

Ardından, şablonunuzda bu parametreyi kullanın:

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

Sürümlü dağıtımları yeni klasörler oluşturarak ve dağıtım sırasında Bu köke geçirerek yönetirsiniz. Yol, kaynakları dağıtan şablona akar.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede Dağıtım Yöneticisi hakkında bilgi edindiniz. Dağıtım Yöneticisi ile dağıtmayı öğrenmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [Öğretici: Azure Dağıtım Yöneticisi Kaynak Yöneticisi şablonlarıyla kullanma](./deployment-manager-tutorial.md)
>
> [Hızlı başlangıç: Azure Dağıtım Yöneticisi yalnızca birkaç dakika içinde deneyin](https://github.com/Azure-Samples/adm-quickstart)
