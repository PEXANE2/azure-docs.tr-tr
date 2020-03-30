---
title: Genel Bakış - Azure Mantık Uygulamaları için dağıtımı otomatikleştirin
description: Azure Mantık Uygulamaları için dağıtımı otomatikleştirmek için Azure Kaynak Yöneticisi şablonları hakkında bilgi edinin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 07/25/2019
ms.openlocfilehash: 920d8bfbcef33464d528306113abe6223d752889
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477757"
---
# <a name="overview-automate-deployment-for-azure-logic-apps-by-using-azure-resource-manager-templates"></a>Genel Bakış: Azure Kaynak Yöneticisi şablonlarını kullanarak Azure Mantık Uygulamaları için dağıtımı otomatikleştirin

Mantık uygulamanızı otomatikleştirmek ve dağıtmak için hazır olduğunuzda, mantık uygulamanızın temel iş akışı tanımını Azure [Kaynak Yöneticisi şablonuna](../azure-resource-manager/management/overview.md)genişletebilirsiniz. Bu şablon, mantık uygulamanızı sağlama ve dağıtma için altyapıyı, kaynakları, parametreleri ve diğer bilgileri tanımlar. *Parametrelendirme*olarak da bilinen dağıtım sırasında değişen değerler için parametrelertanımlayarak, farklı dağıtım gereksinimlerine göre mantık uygulamalarını tekrar tekrar ve tutarlı bir şekilde dağıtabilirsiniz.

Örneğin, geliştirme, sınama ve üretim ortamlarına dağıtıyorsanız, büyük olasılıkla her ortam için farklı bağlantı dizeleri kullanırsınız. Farklı bağlantı dizeleri kabul eden şablon parametrelerini bildirebilir ve bu dizeleri ayrı bir [parametre dosyasında](../azure-resource-manager/templates/parameter-files.md)depolayabilirsiniz. Bu şekilde, şablonu güncelleştirmek ve yeniden dağıtmak zorunda kalmadan bu değerleri değiştirebilirsiniz. Parolalar ve sırlar gibi hassas veya güvenli olması gereken parametre değerlerine sahip olduğunuz senaryolarda, bu değerleri [Azure Key Vault'ta](../azure-resource-manager/templates/key-vault-parameter.md) depolayabilir ve parametreler dosyanızın bu değerleri geri almasına sahip olabilirsiniz. Ancak, bu senaryolarda, geçerli değerleri almak için yeniden dağıtılırsınız.

Bu genel bakış, bir mantık uygulaması iş akışı tanımı içeren bir Kaynak Yöneticisi şablonundaki öznitelikleri açıklar. Hem şablon hem de iş akışı tanımıjson sözdizimini kullanır, ancak iş akışı tanımı da [İş Akışı Tanımı Dili şemasını](../logic-apps/logic-apps-workflow-definition-language.md)izlediğinden bazı farklılıklar vardır. Örneğin, şablon ifadeleri ve iş akışı tanımı ifadeleri [parametrelere](#parameter-references) nasıl başvuruldıkları ve kabul edebilecekleri değerler açısından farklılık gösterir.

> [!TIP]
> Çoğunlukla dağıtıma hazır geçerli bir parametreli mantık uygulaması şablonu almanın en kolay yolu için Visual Studio (ücretsiz Topluluk sürümü veya daha büyük) ve Visual Studio için Azure Mantıksal Uygulama Araçları'nı kullanın. Daha sonra [Visual Studio'da mantık uygulamanızı oluşturabilir](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md) veya [Azure'dan Visual Studio'ya varolan bir mantık uygulamasını bulup indirebilirsiniz.](../logic-apps/manage-logic-apps-with-visual-studio.md)
>
> Veya [LogicAppTemplate modülü ile Azure PowerShell'i](../logic-apps/logic-apps-create-azure-resource-manager-templates.md#azure-powershell)kullanarak mantık uygulama şablonları oluşturabilirsiniz.

Bu konudaki örnek mantık uygulaması, yeni bir e-posta geldiğinde devreye saçan bir [Office 365 Outlook tetikleyicisi](/connectors/office365/) ve e-posta gövdesi için bir leke oluşturan ve bu blob'u bir Azure depolama kapsayıcısına yükleyen bir [Azure Blob Depolama eylemi](/connectors/azureblob/) kullanır. Örnekler, dağıtım sırasında değişen değerleri nasıl parametreize edilebildiğini de gösterir.

Kaynak Yöneticisi şablonları hakkında daha fazla bilgi için şu konulara bakın:

* [Azure Kaynak Yöneticisi şablon yapısı ve sözdizimi](../azure-resource-manager/templates/template-syntax.md)
* [Resource Manager şablonu en iyi yöntemleri](../azure-resource-manager/templates/template-best-practices.md)
* [Bulut tutarlılığı için Azure Resource Manager şablonları geliştirme](../azure-resource-manager/templates/templates-cloud-consistency.md)

Örnek mantık uygulaması şablonları için aşağıdaki örneklere bakın:

* Bu konunun örnekleri için kullanılan [tam şablon](#full-example-template)
* GitHub'da [hızlı başlatma mantığı uygulaması şablonu örneği](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create)

Mantık uygulamalarına, tümleştirme hesaplarına ve tümleştirme hesabı yapılarına özgü şablon kaynak bilgileri için [Microsoft.Logic kaynak türlerine](https://docs.microsoft.com/azure/templates/microsoft.logic/allversions)bakın.

<a name="template-structure"></a>

## <a name="template-structure"></a>Şablon yapısı

En üst düzeyde, Kaynak Yöneticisi şablonu, Azure Kaynak [Yöneticisi şablon yapısı ve sözdizimi](../azure-resource-manager/templates/template-syntax.md) konusunda tam olarak açıklanan bu yapıyı izler:

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {},
   "variables": {},
   "functions": [],
   "resources": [],
   "outputs": {}
}
```

Bir mantık uygulaması şablonu için öncelikle şu şablon nesneleri ile çalışırsınız:

| Öznitelik | Açıklama |
|-----------|-------------|
| `parameters` | Azure'da dağıtım için kaynak oluştururken ve bunları özelleştirirken kullanılacak değerleri kabul etmek için [şablon parametrelerini](../azure-resource-manager/templates/template-syntax.md#parameters) bildirir. Örneğin, bu parametreler mantık uygulamanızın adı ve konumu, bağlantıları ve dağıtım için gerekli diğer kaynaklar için gereken değerleri kabul eder. Bu parametre değerlerini, daha sonra bu konuda açıklanan bir [parametre dosyasında](#template-parameter-files)depolayabilirsiniz. Genel ayrıntılar için [bkz: Parametreler - Kaynak Yöneticisi şablon yapısı ve sözdizimi.](../azure-resource-manager/templates/template-syntax.md#parameters) |
| `resources` | Mantık uygulamanız, bağlantılarınız, Azure depolama hesaplarınız gibi bir Azure kaynak grubu oluşturmak veya güncelleştirmek ve dağıtmak için [kaynakları](../azure-resource-manager/templates/template-syntax.md#resources) tanımlar. Genel ayrıntılar için [kaynaklar - Kaynak Yöneticisi şablon yapısı ve sözdizimine](../azure-resource-manager/templates/template-syntax.md#resources)bakın. |
||||

Mantık uygulama şablonunuzun bu dosya adı biçimini kullanması:

**<*mantık-app-name*>.json**

> [!IMPORTANT]
> Şablon sözdizimi büyük/küçük harf duyarlıdır, bu nedenle tutarlı kasa kullandığınızdan emin olun. 

<a name="template-parameters"></a>

## <a name="template-parameters"></a>Şablon parametreleri

Bir mantık uygulaması `parameters` şablonu, farklı düzeylerde var olan ve farklı işlevleri gerçekleştiren birden çok nesneye sahiptir. Örneğin, en üst düzeyde, örneğin Azure'da kaynak oluştururken ve dağıtırken dağıtımsırasında kabul edilecek ve kullanılacak değerler için [şablon parametrelerini](../azure-resource-manager/templates/template-syntax.md#parameters) bildirebilirsiniz:

* Mantık uygulamanız
* Mantığınızın [yönetilen konektörler](../connectors/apis-list.md) aracılığıyla diğer hizmetlere ve sistemlere erişmek için kullandığı bağlantılar
* Mantık uygulamanızın dağıtım için ihtiyaç duyduğu diğer kaynaklar

  Örneğin, mantık uygulamanız işletmelerarası (B2B) senaryoları için bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) kullanıyorsa, şablonun üst düzey `parameters` nesnesi bu tümleştirme hesabının kaynak kimliğini kabul eden parametreyi bildirir.

Parametre tanımının genel yapısı ve sözdizimi, Parametreler [- Kaynak Yöneticisi şablon yapısı ve sözdizimi](../azure-resource-manager/templates/template-syntax.md#parameters)ile tam olarak açıklanmıştır:

```json
"<parameter-name>": {
   "type": "<parameter-type>",
   "defaultValue": <default-parameter-value>,
   <other-parameter-attributes>,
   "metadata": {
      "description": "<parameter-description>"
   }
},
```

Bu örnekte, bu kaynakları oluşturmak ve dağıtmak için kullanılan değerlerin şablon parametreleri Azure'da gösterilmektedir:

* Mantık uygulamanızın adı ve konumu
* Mantık uygulamasına bağlı bir tümleştirme hesabı için kullanılacak kimlik

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   // Template parameters
   "parameters": {
      "LogicAppName": {
         "type": "string",
         "minLength": 1,
         "maxLength": 80,
         "defaultValue": "MyLogicApp",
         "metadata": {
            "description": "The resource name for the logic app"
         }
      },
      "LogicAppLocation": {
         "type": "string",
         "minLength": 1,
         "defaultValue": "[resourceGroup().location]",
         "metadata": {
            "description": "The resource location for the logic app"
         }
      },
      "LogicAppIntegrationAccount": {
         "type":"string",
         "minLength": 1,
         "defaultValue": "/subscriptions/<Azure-subscription-ID>/resourceGroups/fabrikam-integration-account-rg/providers/Microsoft.Logic/integrationAccounts/fabrikam-integration-account",
         "metadata": {
            "description": "The ID to use for the integration account"
         }
      }
   },
   "variables": {},
   "functions": [],
   "resources": [],
   "outputs": {}
}
```

Kullanıcı adları, parolalar ve sırlar gibi hassas veya güvenli değerler işleyen parametreler dışında, `defaultValue` bazı durumlarda varsayılan değerler boş değerler olmasına rağmen tüm bu parametreler öznitelikleri içerir. Bu şablon parametreleri için kullanılacak dağıtım değerleri, bu konuda daha sonra açıklanan örnek [parametreler dosyası](#template-parameter-files) tarafından sağlanır.

Şablon parametrelerinin güvenliğini sağlama hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Şablon parametreleri için güvenlik önerileri](../azure-resource-manager/templates/template-best-practices.md#parameters)
* [Şablon parametreleri için güvenliği artırma](../logic-apps/logic-apps-securing-a-logic-app.md#secure-parameters-deployment-template)
* [Azure Anahtar Kasası ile güvenli parametre değerlerini geçirin](../azure-resource-manager/templates/key-vault-parameter.md)

Diğer şablon nesneleri genellikle şablon parametrelerinden geçen değerleri kullanabilmeleri için şablon parametrelerine başvururlar, örneğin:

* [Şablonunuzun kaynak nesnesi](#template-resources)( bu konunun daha sonra açıklanan), [mantıksal uygulamanızın kaynak tanımı](#logic-app-resource-definition)gibi oluşturmak ve dağıtmak istediğiniz Her kaynağı Azure'da tanımlar. Bu kaynaklar genellikle mantık uygulamanızın adı, konumu ve bağlantı bilgileri gibi şablon parametre değerlerini kullanır.

* Mantık uygulamanızın kaynak tanımında daha derin bir düzeyde, [iş akışı tanımınızın parametreler](#workflow-definition-parameters) nesnesi, mantık uygulamanızın çalışma zamanında kullanılacak değerler için parametreler bildirir. Örneğin, bir HTTP tetikleyicisinin kimlik doğrulama için kullandığı kullanıcı adı ve parola için iş akışı tanımı parametrelerini bildirebilirsiniz. İş akışı tanımı parametreleri için değerleri `parameters` belirtmek için, iş akışı tanımınızın *dışında* bulunan ancak yine de mantık uygulamanızın kaynak *tanımının içinde* bulunan nesneyi kullanın. Bu dış `parameters` nesnede, bir parametre dosyasından dağıtımda değerleri kabul edebilen daha önce bildirilen şablon parametrelerine başvuruyapabilirsiniz.

Parametrelere başvururken, şablon ifadeleri ve işlevleri farklı sözdizimi kullanır ve iş akışı tanımı ifadelerinden ve işlevlerinden farklı davranın. Bu farklar hakkında daha fazla bilgi için, bu konunun ilerleyen saatlerinde [parametrelere yapılan başvurulara](#parameter-references) bakın.

<a name="best-practices-template-parameters"></a>

## <a name="best-practices---template-parameters"></a>En iyi uygulamalar - şablon parametreleri

Parametreleri tanımlamak için en iyi uygulamalar şunlardır:

* Parametreleri yalnızca dağıtım gereksinimlerinize bağlı olarak değişen değerler için bildirin. Farklı dağıtım gereksinimleri arasında aynı kalan değerler için parametreleri bildirmeyin.

* Hassas `defaultValue` veya güvenli olması gereken değerler dışında tüm parametreler için boş değerler belirtebilen özniteliği ekleyin. Kullanıcı adları, parolalar ve sırlar için her zaman güvenli parametrelerkullanın. Hassas parametre değerlerini gizlemek veya korumak için aşağıdaki konulardaki kılavuzu izleyin:

  * [Şablon parametreleri için güvenlik önerileri](../azure-resource-manager/templates/template-best-practices.md#parameters)

  * [Şablon parametreleri için güvenliği artırma](../logic-apps/logic-apps-securing-a-logic-app.md#secure-parameters-deployment-template)

  * [Azure Anahtar Kasası ile güvenli parametre değerlerini geçirin](../azure-resource-manager/templates/key-vault-parameter.md)

* Şablon parametre adlarını iş akışı tanımı parametre adlarından ayırt etmek için açıklayıcı şablon parametre adlarını kullanabilirsiniz, örneğin:`TemplateFabrikamPassword`

Daha fazla şablon en iyi uygulamaları için [şablon parametreleri için en iyi uygulamalara](../azure-resource-manager/templates/template-best-practices.md#parameters)bakın.

<a name="template-parameter-files"></a>

## <a name="template-parameters-file"></a>Şablon parametreleri dosyası

Şablon parametreleri için değerleri sağlamak için, bu değerleri bir [parametre dosyasında](../azure-resource-manager/templates/parameter-files.md)saklayın. Bu şekilde, dağıtım gereksinimlerinize bağlı olarak farklı parametreler dosyaları kullanabilirsiniz. Kullanılacak dosya adı biçimi aşağıda veda edilebistir:

* Mantık uygulaması şablon dosyası adı: ** < *mantık-app-name*>.json**
* Parametreler dosya adı: ** < *mantık-uygulama adı*>.parameters.json**

[Azure Anahtar Kasası ile güvenli bir parametre değerini geçirmek](../azure-resource-manager/templates/key-vault-parameter.md)için önemli bir kasa referansı içeren parametre dosyasının içindeki yapı aşağıda verilmiştir:

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
   "contentVersion": "1.0.0.0",
   // Template parameter values
   "parameters": {
      "<parameter-name-1>": {
         "value": "<parameter-value>"
      },
      "<parameter-name-2>": {
         "value": "<parameter-value>"
      },
      "<secured-parameter-name>": {
         "reference": {
            "keyVault": {
               "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/Microsoft.KeyVault/vaults/<key-vault-name>",
            },
            "secretName: "<secret-name>"
         }
      },
      <other-parameter-values>
   }
}
```

Bu örnek parametreler dosyası, bu konuda daha önce bildirilen şablon parametreleri için değerleri belirtir:

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
   "contentVersion": "1.0.0.0",
   // Template parameter values
   "parameters": {
      "LogicAppName": {
         "value": "Email-Processor-Logic-App"
      },
      "LogicAppLocation": {
         "value": "westeurope"
      }
   }
}
```

<a name="template-resources"></a>

## <a name="template-resources"></a>Şablon kaynakları

Şablonunuzun, `resources` [mantıksal uygulamanızın kaynak tanımı,](#logic-app-resource-definition) [bağlantı kaynağı tanımları](#connection-resource-definitions)ve mantık uygulamanızın dağıtım için ihtiyaç duyduğu diğer kaynaklar gibi Azure'da oluşturulacak ve dağıtılabilen her kaynak için tanımlar içeren bir nesne vardır.

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {<template-parameters>},
   "variables": {},
   "functions": [],
   "resources": [
      {
         <logic-app-resource-definition>
      },
      // Start connection resource definitions
      {
         <connection-resource-definition-1>
      },
      {
         <connection-resource-definition-2>
      }
   ],
   "outputs": {}
}
```

> [!NOTE]
> Şablonlar birden çok mantık uygulaması için kaynak tanımları içerebilir, bu nedenle tüm mantık uygulama kaynaklarınızın aynı Azure kaynak grubunu belirttiğinden emin olun. Visual Studio'yu kullanarak şablonu bir Azure kaynak grubuna dağıttığınızda, hangi mantık uygulamasını açmak istediğiniz istenir. Ayrıca, Azure kaynak grubu projeniz birden fazla şablon içerebilir, bu nedenle istendiğinde doğru parametreler dosyasını seçtiğinizden emin olun.

Şablon kaynakları ve öznitelikleri hakkında genel bilgi için şu konulara bakın:

* [Kaynaklar - Kaynak Yöneticisi şablon yapısı ve sözdizimi](../azure-resource-manager/templates/template-syntax.md#resources)
* [Şablon kaynakları için en iyi uygulamalar](../azure-resource-manager/templates/template-best-practices.md#resources)

<a name="logic-app-resource-definition"></a>

### <a name="logic-app-resource-definition"></a>Mantık uygulaması kaynak tanımı

Mantık uygulamanızın kaynak tanımı aşağıdaki `properties` bilgileri içeren nesneyle başlar:

* Mantıksal uygulamanızın dağıtımdaki durumu
* Mantık uygulamanız tarafından kullanılan tüm entegrasyon hesabının kimliği
* Mantık uygulamanızın iş akışı tanımı
* Çalışma `parameters` zamanında kullanılacak değerleri ayarlayan bir nesne
* Mantık uygulamanızla ilgili ad, tür, konum gibi diğer kaynak bilgileri

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {<template-parameters>},
   "variables": {},
   "functions": [],
   "resources": [
      {
         // Start logic app resource definition
         "properties": {
            "state": "<Enabled or Disabled>",
            "integrationAccount": {
               "id": "[parameters('LogicAppIntegrationAccount')]" // Template parameter reference
            },
            "definition": {<workflow-definition>},
            "parameters": {<workflow-definition-parameter-values>},
            "accessControl": {}
         },
         "name": "[parameters('LogicAppName')]", // Template parameter reference
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]", // Template parameter reference
         "tags": {
           "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "dependsOn": [
         ]
      }
      // End logic app resource definition
   ],
   "outputs": {}
}
```

Mantık uygulaması kaynak tanımınıza özgü öznitelikler şunlardır:

| Öznitelik | Gerekli | Tür | Açıklama |
|-----------|----------|------|-------------|
| `state` | Evet | Dize | Mantık uygulamanızın dağıtımdaki durumu, mantık uygulamanızın `Disabled` canlı olduğu ve mantık uygulamanızın etkin olmadığı `Enabled` anlamına gelir. Örneğin, mantık uygulamanızın yayına geçmesine hazır değilseniz ancak taslak bir sürüm dağıtmak `Disabled` istiyorsanız, bu seçeneği kullanabilirsiniz. |
| `integrationAccount` | Hayır | Nesne | Mantık uygulamanız, yapılarını işletmeden işletmeye (B2B) senaryolar için depolayan bir `id` tümleştirme hesabı kullanıyorsa, bu nesne tümleştirme hesabının kimliğini belirten özniteliği içerir. |
| `definition` | Evet | Nesne | Mantık uygulamanızın, kod görünümünde görünen ve İş Akışı Tanımı Dili konusu için [Şema başvurusunda](../logic-apps/logic-apps-workflow-definition-language.md) tam olarak açıklanan nesne olan temel iş akışı tanımı. Bu iş akışı tanımında, `parameters` nesne mantık uygulaması çalışma zamanında kullanılacak değerler için parametreleri bildirir. Daha fazla bilgi için [İş Akışı tanımı ve parametreleri](#workflow-definition-parameters)bkz. <p><p>Mantık uygulamanızın iş akışı tanımındaki öznitelikleri görüntülemek için Azure portalında veya Visual Studio'da "tasarım görünümü"nden "kod görünümüne" geçiş yaparak veya [Azure Kaynak Gezgini](https://resources.azure.com)gibi bir araç kullanarak. |
| `parameters` | Hayır | Nesne | Mantık uygulaması çalışma zamanında kullanılacak [iş akışı tanımı parametre değerleri.](#workflow-definition-parameters) Bu değerleriçin parametre tanımları [iş akışı tanımınızın parametreler nesnesi](#workflow-definition-parameters)içinde görünür. Ayrıca, mantık uygulamanız diğer hizmetlere ve sistemlere erişmek için yönetilen `$connections` [bağlayıcılar](../connectors/apis-list.md) kullanıyorsa, bu nesne çalışma zamanında kullanılacak bağlantı değerlerini ayarlayan bir nesne içerir. |
| `accessControl` | Hayır | Nesne | İstek tetikleyicileri veya geçmiş giriş ve çıkışları çalıştırmak için IP erişimini kısıtlamak gibi mantık uygulamanız için güvenlik özniteliklerini belirtmek için. Daha fazla bilgi için [bkz.](../logic-apps/logic-apps-securing-a-logic-app.md) |
||||

Mantık uygulamalarına, tümleştirme hesaplarına ve tümleştirme hesabı yapılarına özgü şablon kaynak bilgileri için [Microsoft.Logic kaynak türlerine](https://docs.microsoft.com/azure/templates/microsoft.logic/allversions)bakın.

<a name="workflow-definition-parameters"></a>

## <a name="workflow-definition-and-parameters"></a>İş akışı tanımı ve parametreleri

Mantık uygulamanızın iş akışı tanımı, `definition` mantık uygulamanızın kaynak `properties` tanımının içindeki nesnede görünen nesnede görünür. Bu `definition` nesne, kod görünümünde görünen ve İş Akışı Tanımı Dili konusu [için Şema başvurusunda](../logic-apps/logic-apps-workflow-definition-language.md) tam olarak açıklanan nesnedir. İş akışı tanımınız, `parameters` çalışma zamanında iş akışı tanımınız tarafından kullanılan değerler için yeni parametreler tanımlayabileceğiniz veya varolan parametreleri düzenlemeyapabileceğiniz bir iç bildirim nesnesi içerir. Daha sonra bu parametreleri tetikleyici nin veya iş akışınızdaki eylemlerin içinde referans verebilirsiniz. Varsayılan olarak, `parameters` mantık uygulamanız [yönetilen bağlayıcılar](../connectors/apis-list.md)aracılığıyla diğer hizmetlere ve sistemlere bağlantılar oluşturmadığı sürece bu nesne boştur.

İş akışı tanımı parametreleri için değerleri `parameters` ayarlamak için, iş akışı tanımınızın *dışında* bulunan ancak yine de mantık uygulamanızın kaynak *tanımının içinde* bulunan nesneyi kullanın. Bu dış `parameters` nesnede, daha sonra bir parametre dosyasından dağıtım değerlerini kabul edebilir önceden bildirilen şablon parametreleri, başvuru yapabilirsiniz.

> [!TIP]
>
> En iyi uygulama olarak, iş akışı tanımının içinden dağıtım sırasında değerlendirilen şablon parametrelerine doğrudan başvuruyapmayın. Bunun yerine, iş akışı tanımınızın *dışında* ancak yine `parameters` de mantık uygulamanızın kaynak tanımının *içinde* bulunan nesneye ayarlayabileceğiniz bir iş akışı tanımı parametresi bildirin. Daha fazla bilgi [için parametrelere Başvurular'a](#parameter-references)bakın.

Bu sözdizimi, şablon ve iş akışı tanımı parametrelerine başvurarak bu parametre değerlerini ayarlayabileceğiniz parametreleri hem şablon hem de iş akışı tanımı düzeylerinde bildirebileceğiniz yeri gösterir:

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   // Template parameters
   "parameters": {
      "<template-parameter-name>": {
         "type": "<parameter-type>",
         "defaultValue": "<parameter-default-value>",
         "metadata": {
            "description": "<parameter-description>"
         }
      }
   },
   "variables": {},
   "functions": [],
   "resources": [
      {
         // Start logic app resource definition
         "properties": {
            <other-logic-app-resource-properties>,
            "definition": {
               "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
               "actions": {<action-definitions>},
               // Workflow definition parameters
               "parameters": {
                  "<workflow-definition-parameter-name>": {
                     "type": "<parameter-type>",
                     "defaultValue": "<parameter-default-value>",
                     "metadata": {
                        "description": "<parameter-description>"
                     }
                  }
               },
               "triggers": {
                  "<trigger-name>": {
                     "type": "<trigger-type>",
                     "inputs": {
                         // Workflow definition parameter reference
                         "<attribute-name>": "@parameters('<workflow-definition-parameter-name')"
                     }
                  }
               },
               <...>
            },
            // Workflow definition parameter value
            "parameters": {
               "<workflow-definition-parameter-name>": "[parameters('<template-parameter-name>')]"
            },
            "accessControl": {}
         },
         <other-logic-app-resource-definition-attributes>
      }
      // End logic app resource definition
   ],
   "outputs": {}
}
```

<a name="secure-workflow-definition-parmameters"></a>

### <a name="secure-workflow-definition-parameters"></a>Güvenli iş akışı tanımı parametreleri

Çalışma zamanında hassas bilgileri, parolaları, erişim anahtarlarını veya sırları işleyen bir iş akışı tanımı parametresi `securestring` `secureobject` için, parametreyi veya parametre türünü kullanmak üzere bildirin veya değiştirin. Bu parametreye iş akışı tanımıboyunca ve içinde başvuruda bulunabilirsiniz. Şablonun en üst düzeyinde, dağıtımsırasında bu bilgileri işlemek için aynı türe sahip bir parametre bildirin.

İş akışı tanımı parametresinin değerini ayarlamak `parameters` için, şablon parametresine başvurmak için iş akışı tanımınızın *dışında* olan ancak yine de mantık uygulama kaynak tanımınızın *içinde* olan nesneyi kullanın. Son olarak, dağıtımısırasında şablon parametrenize değeri geçirmek için bu değeri [Azure Anahtar Kasası'nda](../azure-resource-manager/templates/key-vault-parameter.md) depolayın ve dağıtımsırasında şablonunuz tarafından kullanılan [parametre dosyasında](#template-parameter-files) bu anahtar kasasına başvurun.

Bu örnek şablon, değerlerini Azure Anahtar Kasası'nda depolayabilmeniz için gerektiğinde güvenli parametreleri tanımlayarak bu görevleri nasıl tamamlayabileceğinizi gösterir:

* Erişimi doğrulamak için kullanılan değerler için güvenli parametreleri bildirin.
* Bu değerleri hem şablon hem de iş akışı tanım düzeylerinde kullanın.
* Bir parametre dosyası kullanarak bu değerleri sağlayın.

**Şablon**

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {
      <previously-defined-template-parameters>,
      // Additional template parameters for passing values to use in workflow definition
      "TemplateAuthenticationType": {
         "type": "string",
         "defaultValue": "",
         "metadata": {
            "description": "The type of authentication used for the Fabrikam portal"
         }
      },
      "TemplateFabrikamPassword": {
         "type": "securestring",
         "metadata": {
            "description": "The password for the Fabrikam portal"
         }
      },
      "TemplateFabrikamUserName": {
         "type": "securestring",
         "metadata": {
            "description": "The username for the Fabrikam portal"
         }
      }
   },
   "variables": {},
   "functions": [],
   "resources": [
      {
         // Start logic app resource definition
         "properties": {
            <other-logic-app-resource-properties>,
            // Start workflow definition
            "definition": {
               "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
               "actions": {<action-definitions>},
               // Workflow definition parameters
               "parameters": {
                  "authenticationType": {
                     "type": "string",
                     "defaultValue": "",
                     "metadata": {
                        "description": "The type of authentication used for the Fabrikam portal"
                     }
                  },
                  "fabrikamPassword": {
                     "type": "securestring",
                     "metadata": {
                        "description": "The password for the Fabrikam portal"
                     }
                  },
                  "fabrikamUserName": {
                     "type": "securestring",
                     "metadata": {
                        "description": "The username for the Fabrikam portal"
                     }
                  }
               },
               "triggers": {
                  "HTTP": {
                     "inputs": {
                        "authentication": {
                           // Reference workflow definition parameters
                           "password": "@parameters('fabrikamPassword')",
                           "type": "@parameters('authenticationType')",
                           "username": "@parameters('fabrikamUserName')"
                        }
                     },
                     "recurrence": {<...>},
                     "type": "Http"
                  }
               },
               <...>
            },
            // End workflow definition
            // Start workflow definition parameter values
            "parameters": {
               "authenticationType": "[parameters('TemplateAuthenticationType')]", // Template parameter reference
               "fabrikamPassword": "[parameters('TemplateFabrikamPassword')]", // Template parameter reference
               "fabrikamUserName": "[parameters('TemplateFabrikamUserName')]" // Template parameter reference
            },
            "accessControl": {}
         },
         <other-logic-app-resource-attributes>
      }
      // End logic app resource definition
   ],
   "outputs": {}
}
```

**Parametreler dosyası**

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
   "contentVersion": "1.0.0.0",
   // Template parameter values
   "parameters": {
      <previously-defined-template-parameter-values>,
     "TemplateAuthenticationType": {
        "value": "Basic"
     },
     "TemplateFabrikamPassword": {
        "reference": {
           "keyVault": {
              "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/Microsoft.KeyVault/vaults/fabrikam-key-vault"
           },
           "secretName": "FabrikamPassword"
        }
     },
     "TemplateFabrikamUserName": {
        "reference": {
           "keyVault": {
              "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/Microsoft.KeyVault/vaults/fabrikam-key-vault"
           },
           "secretName": "FabrikamUserName"
        }
     }
   }
}
```

<a name="best-practices-workflow-definition-parameters"></a>

## <a name="best-practices---workflow-definition-parameters"></a>En iyi uygulamalar - iş akışı tanımı parametreleri

Mantık Uygulama Tasarımcısı'nın iş akışı tanımı parametrelerini doğru şekilde gösterebilmesini sağlamak için aşağıdaki en iyi uygulamaları izleyin:

* Hassas `defaultValue` veya güvenli olması gereken değerler dışında tüm parametreler için boş değerler belirtebilen özniteliği ekleyin.

* Kullanıcı adları, parolalar ve sırlar için her zaman güvenli parametrelerkullanın. Hassas parametre değerlerini gizlemek veya korumak için aşağıdaki konulardaki kılavuzu izleyin:

  * [Eylem parametreleri için güvenlik önerileri](../logic-apps/logic-apps-securing-a-logic-app.md#secure-action-parameters)

  * [İş akışı tanımlarında parametreler için güvenlik önerileri](../logic-apps/logic-apps-securing-a-logic-app.md#secure-parameters-workflow)

  * [Azure Anahtar Kasası ile güvenli parametre değerlerini geçirin](../azure-resource-manager/templates/key-vault-parameter.md)

İş akışı tanımı parametreleri hakkında daha fazla bilgi için [bkz: Parametreler - İş Akışı Tanımı Dili.](../logic-apps/logic-apps-workflow-definition-language.md#parameters)

<a name="connection-resource-definitions"></a>

## <a name="connection-resource-definitions"></a>Bağlantı kaynağı tanımları

Mantık uygulamanız [yönetilen bağlayıcıları](../connectors/apis-list.md)kullanarak diğer hizmetlere ve sisteme bağlantılar oluşturduğunda `resources` ve kullandığında, şablonunuzun nesnesi bu bağlantılar için kaynak tanımlarını içerir.

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {<template-parameters>},
   "variables": {},
   "functions": [],
   "resources": [
      {
         <logic-app-resource-definition>
      },
      // Start connection resource definitions
      {
         <connection-resource-definition-1>
      },
      {
         <connection-resource-definition-2>
      }
   ],
   "outputs": {}
}
```

Bağlantı kaynağı tanımları, şablonun değerleri için üst düzey parametrelerine başvurur, bu da bu değerleri bir parametre dosyası kullanarak dağıtımsırasında sağlayabileceğiniz anlamına gelir. Bağlantıların mantık uygulamanızla aynı Azure kaynak grubunu ve konumu kullandığından emin olun.

Aşağıda, Office 365 Outlook bağlantısı ve ilgili şablon parametreleri için örnek bir kaynak tanımı verilmiştir:

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   // Template parameters
   "parameters": {
      "LogicAppName": {<parameter-definition>},
      "LogicAppLocation": {<parameter-definition>},
      "office365_1_Connection_Name": {
         "type": "string",
         "defaultValue": "office365",
         "metadata": {
            "description": "The resource name for the Office 365 Outlook connection"
         }
      },
      "office365_1_Connection_DisplayName": {
         "type": "string",
         "defaultValue": "",
         "metadata": {
            "description": "The display name for the Office 365 Outlook connection"
         }
      }
   },
   "variables": {},
   "functions": [],
   "resources": [
      {
         <logic-app-resource-definition>
      },
      // Office 365 Outlook API connection resource definition
      {
         "type": "MICROSOFT.WEB/CONNECTIONS",
         "apiVersion": "2016-06-01",
         // Template parameter reference for connection name
         "name": "[parameters('office365_1_Connection_Name')]",
         // Template parameter reference for connection resource location. Must match logic app location.
         "location": "[parameters('LogicAppLocation')]",
         "properties": {
            "api": {
               // Connector ID
               "id": "[concat(subscription().id, '/providers/Microsoft.Web/locations/', parameters('LogicAppLocation'), '/managedApis/', 'office365')]"
            },
            // Template parameter reference for connection display name
            "displayName": "[parameters('office365_1_Connection_DisplayName')]"
         }
      }
   ],
   "outputs": {}
}
```

Mantık uygulamanızın kaynak tanımı da bağlantı kaynağı tanımlarıyla şu şekilde çalışır:

* İş akışı tanımınızın `parameters` içinde, nesne `$connections` mantık uygulaması çalışma zamanında kullanılacak bağlantı değerleri için bir parametre bildirir. Ayrıca, bağlantı oluşturan tetikleyici veya eylem, bu `$connections` parametreden geçen karşılık gelen değerleri kullanır.

* İş akışı tanımınızın *dışında* ancak yine de mantık uygulamanızın kaynak *tanımının içinde* olan başka bir `parameters` nesne, ilgili şablon parametrelerine başvurarak `$connections` parametrenin çalışma zamanında kullanılacak değerleri ayarlar. Bu değerler, mantık uygulamanızdaki bağlantıların meta verilerini güvenli bir şekilde depolayan kaynaklara başvurmak için şablon ifadelerini kullanır.

  Örneğin, meta veriler, Azure Anahtar Kasası'nda depolayabildiğiniz [Azure Key Vault](../azure-resource-manager/templates/key-vault-parameter.md)bağlantı dizeleri ve erişim belirteçleri içerebilir. Bu değerleri şablon parametrelerinize geçirmek için, dağıtım sırasında şablonunuz tarafından kullanılan [parametre dosyasındaki](#template-parameter-files) bu anahtar kasasına başvurursunuz. Başvuru parametrelerindeki farklılıklar hakkında daha fazla bilgi için, bu konunun ilerleyen saatlerinde [parametrelere yapılan başvurulara](#parameter-references) bakın.

  Mantıksal uygulamanızın iş akışı tanımını Azure portalı veya Visual Studio üzerinden `$connections` kod görünümünde açtığınızda, nesne iş akışı tanımınızın dışında ancak aynı düzeyde görünür. Kod görünümündeki bu sıralama, iş akışı tanımını el ile güncelleştirdiğinizde bu parametrelerin başvurulmasını kolaylaştırır:

  ```json
  {
     "$connections": {<workflow-definition-parameter-connection-values-runtime},
     "definition": {<workflow-definition>}
  }
  ```

* Mantık uygulamanızın kaynak tanımında, mantık uygulamanızın kullandığı bağlantılara bağımlılıkları belirten bir `dependsOn` nesne vardır.

Oluşturduğunuz her bağlantının Azure'da benzersiz bir adı vardır. Aynı hizmet veya sisteme birden çok bağlantı oluşturduğunuzda, her bağlantı adı, örneğin, `office365`, , `office365-1`ve benzeri olarak oluşturulan her yeni bağlantıyla birlikte artımlar içeren bir sayıyla eklenir.

Bu örnek, mantık uygulamanızın kaynak tanımı ile Office 365 Outlook için bağlantı kaynağı tanımı arasındaki etkileşimleri gösterir:

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   // Template parameters
   "parameters": {
      "LogicAppName": {<parameter-definition>},
      "LogicAppLocation": {<parameter-definition>},
      "office365_1_Connection_Name": {<parameter-definition>},
      "office365_1_Connection_DisplayName": {<parameter-definition>}
   },
   "variables": {},
   "functions": [],
   "resources": [
      {
         // Start logic app resource definition
         "properties": {
            <...>,
            "definition": {
               <...>,
               "parameters": {
                  // Workflow definition "$connections" parameter
                  "$connections": {
                     "defaultValue": {},
                     "type": "Object"
                  }
               },
               <...>
            },
            "parameters": {
               // Workflow definition "$connections" parameter values to use at runtime
               "$connections": {
                  "value": {
                     "office365": {
                        // Template parameter references
                        "id": "[concat(subscription().id, '/providers/Microsoft.Web/locations/', parameters('LogicAppLocation'), '/managedApis/', 'office365')]",
                        "connectionId": "[resourceId('Microsoft.Web/connections', parameters('office365_1_Connection_Name'))]",
                        "connectionName": "[parameters('office365_1_Connection_Name')]"
                     }
                  }
               }
            },
            <other-logic-app-resource-information>,
            "dependsOn": [
               "[resourceId('Microsoft.Web/connections', parameters('office365_1_Connection_Name'))]"
            ]
         }
         // End logic app resource definition
      },
      // Office 365 Outlook API connection resource definition
      {
         "type": "MICROSOFT.WEB/CONNECTIONS",
         "apiVersion": "2016-06-01",
         // Template parameter reference for connection name
         "name": "[parameters('office365_1_Connection_Name')]",
         // Template parameter reference for connection resource location. Must match logic app location.
         "location": "[parameters('LogicAppLocation')]",
         "properties": {
            "api": {
               // Connector ID
               "id": "[concat(subscription().id, '/providers/Microsoft.Web/locations/', parameters('LogicAppLocation'), '/managedApis/', 'office365')]"
            },
            // Template parameter reference for connection display name
            "displayName": "[parameters('office365_1_Connection_DisplayName')]"
         }
      }
   ],
   "outputs": {}
}
```

<a name="secure-connection-parameters"></a>

### <a name="secure-connection-parameters"></a>Güvenli bağlantı parametreleri

Hassas bilgileri, parolaları, erişim anahtarlarını veya sırları işleyen bir bağlantı parametresi `parameterValues` için, bağlantının kaynak tanımı bu değerleri ad değeri çifti biçiminde belirten bir nesne içerir. Bu bilgileri gizlemek için, bu değerler için şablon parametrelerini `securestring` `secureobject` veya parametre türlerini kullanarak bildirebilir veya düzenleme yapabilirsiniz. Daha sonra bu bilgileri [Azure Key Vault'ta](../azure-resource-manager/templates/key-vault-parameter.md)depolayabilirsiniz. Bu değerleri şablon parametrelerinize geçirmek için, dağıtım sırasında şablonunuz tarafından kullanılan [parametre dosyasındaki](#template-parameter-files) bu anahtar kasasına başvurursunuz.

Azure Blob Depolama bağlantısı için hesap adı ve erişim anahtarını sağlayan bir örnek aşağıda verilmiştir:

**Parametreler dosyası**

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
   "contentVersion": "1.0.0.0",
   // Template parameter values
   "parameters": {
      "LogicAppName": {
         "value": "Email-Processor-Logic-App"
      },
      "LogicAppLocation": {
         "value": "westeurope"
      },
      "azureblob_1_Connection_Name": {
         "value": "Fabrikam-Azure-Blob-Storage-Connection"
      },
      "azureblob_1_Connection_DisplayName": {
         "value": "Fabrikam-Storage"
      },
      "azureblob_1_accountName": {
         "value": "Fabrikam-Storage-Account"
      },
      "azureblob_1_accessKey": {
         "reference": {
            "keyVault": {
               "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/Microsoft.KeyVault/vaults/fabrikam-key-vault"
            },
            "secretName": "FabrikamStorageKey"
         }
      }
   }
}
```

**Şablon**

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   // Template parameters
   "parameters": {
      "LogicAppName": {<parameter-definition>},
      "LogicAppLocation": {<parameter-definition>},
      "azureblob_1_Connection_Name": {<parameter-definition>},
      "azureblob_1_Connection_DisplayName": {<parameter-definition>},
      "azureblob_1_accountName": {
         "type": "string",
         "defaultValue": "",
         "metadata": {
            "description": "Name of the storage account the connector should use."
         }
      },
      "azureblob_1_accessKey": {
         "type": "secureobject",
         "metadata": {
            "description": "Specify a valid primary/secondary storage account access key."
         }
      }
   },
   "variables": {},
   "functions": [],
   "resources": [
      {
         "properties": {
            "state": "Disabled",
            "definition": {
               "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
               "actions": {
                  // Azure Blob Storage action
                  "Create_blob": {
                     "type": "ApiConnection",
                     "inputs": {
                        "host": {
                           "connection": {
                              // Workflow definition parameter reference for values to use at runtime
                              "name": "@parameters('$connections')['azureblob']['connectionId']"
                           }
                        },
                     },
                     "method": "post",
                     "body": "@triggerBody()?['Body']",
                     "path": "/datasets/default/files",
                     "queries": {
                        "folderPath": "/emails",
                        "name": "@triggerBody()?['Subject']",
                        "queryParametersSingleEncoded": true
                     },
                     "runAfter": {},
                     "runtimeConfiguration": {
                        "contentTransfer": {
                           "transferMode": "Chunked"
                        }
                     }
                  }
               },
               "parameters": {
                  // Workflow definition parameter for values to use at runtime
                  "$connections": {
                     "defaultValue": {},
                     "type": "Object"
                  }
               },
               "triggers": {<trigger-definition>},
               "contentVersion": "1.0.0.0",
               "outputs": {}
            },
            "parameters": {
               "$connections": {
                  "value": {
                     // Template parameter references for values to use at runtime
                     "azureblob": {
                        "id": "[concat(subscription().id, '/providers/Microsoft.Web/locations/', parameters('LogicAppLocation'), '/managedApis/', 'azureblob')]",
                        "connectionId": "[resourceId('Microsoft.Web/connections', parameters('azureblob_1_Connection_Name'))]",
                        "connectionName": "[parameters('azureblob_1_Connection_Name')]"
                    }
                  }
               }
            },
            "name": "[parameters('LogicAppName')]",
            "type": "Microsoft.Logic/workflows",
            "location": "[parameters('LogicAppLocation')]",
            "tags": {
               "displayName": "LogicApp"
            },
            "apiVersion": "2016-06-01",
            // Template parameter reference for value to use at deployment
            "dependsOn": [
               "[resourceId('Microsoft.Web/connections', parameters('azureblob_1_Connection_Name'))]"
            ]
         }
      },
      // Azure Blob Storage API connection resource definition
      {
         "type": "MICROSOFT.WEB/CONNECTIONS",
         "apiVersion": "2016-06-01",
         "name": "[parameters('azureblob_1_Connection_Name')]",
         "location": "[parameters('LogicAppLocation')]",
         "properties": {
            "api": {
               "id": "[concat(subscription().id, '/providers/Microsoft.Web/locations/', parameters('LogicAppLocation'), '/managedApis/', 'azureblob')]"
            },
            "displayName": "[parameters('azureblob_1_Connection_DisplayName')]",
            // Template parameter reference for values to use at deployment
            "parameterValues": {
               "accountName": "[parameters('azureblob_1_accountName')]",
               "accessKey": "[parameters('azureblob_1_accessKey')]"
            }
         }
      }
   ],
   "outputs": {}
}
```

<a name="authenticate-connections"></a>

### <a name="authenticate-connections"></a>Bağlantıları doğrulat

Dağıtımdan sonra, mantık uygulamanız geçerli parametrelerle uçtan uca çalışır. Ancak, [kimlik bilgilerinizin doğruluğunu doğrulamak](../active-directory/develop/authentication-scenarios.md)için geçerli erişim belirteçleri oluşturmak için herhangi bir OAuth bağlantısına yine de yetki vermelisiniz. Daha fazla bilgi için Bkz. [OAuth bağlantılarını Yetkilendirme.](../logic-apps/logic-apps-deploy-azure-resource-manager-templates.md#authorize-oauth-connections)

Bazı bağlantılar, [Azure AD'de kayıtlı](../active-directory/develop/quickstart-register-app.md)bir mantık uygulaması için bağlantıları yetkilendirmek için bir Azure Etkin Dizin (Azure AD) hizmet [yöneticisini](../active-directory/develop/app-objects-and-service-principals.md) kullanmayı destekler. Örneğin, bu Azure Veri Gölü bağlantı kaynağı tanımı, hizmet sorumlusunun bilgilerini işleyen şablon parametrelerine nasıl başvurulacağı nızı ve şablonun bu parametreleri nasıl beyan ettiğini gösterir:

**Bağlantı kaynağı tanımı**

```json
{
   <other-template-objects>
   "type": "MICROSOFT.WEB/CONNECTIONS",
   "apiVersion": "2016-06-01",
   "name": "[parameters('azuredatalake_1_Connection_Name')]",
   "location": "[parameters('LogicAppLocation')]",
   "properties": {
      "api": {
         "id": "[concat(subscription().id, '/providers/Microsoft.Web/locations/', 'resourceGroup().location', '/managedApis/', 'azuredatalake')]"
      },
      "displayName": "[parameters('azuredatalake_1_Connection_DisplayName')]",
      "parameterValues": {
         "token:clientId": "[parameters('azuredatalake_1_token:clientId')]",
         "token:clientSecret": "[parameters('azuredatalake_1_token:clientSecret')]",
         "token:TenantId": "[parameters('azuredatalake_1_token:TenantId')]",
         "token:grantType": "[parameters('azuredatalake_1_token:grantType')]"
      }
   }
}
```

| Öznitelik | Açıklama |
|-----------|-------------|
| `token:clientId` | Hizmet müdürünüzle ilişkili uygulama veya istemci kimliği |
| `token:clientSecret` | Hizmet temelinizle ilişkili temel değer |
| `token:TenantId` | Azure AD kiracınızın dizin kimliği |
| `token:grantType` | İstenen hibe türü, `client_credentials`hangi olmalıdır . Daha fazla bilgi için [Microsoft kimlik platformuna ve OAuth 2.0 istemci kimlik bilgileri akışına](../active-directory/develop/v2-oauth2-client-creds-grant-flow.md)bakın. |
|||

**Şablon parametre tanımları**

Şablonun üst düzey `parameters` nesnesi örnek bağlantı için bu parametreleri bildirir:

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "azuredatalake_1_Connection_Name": {
        "type": "string",
        "defaultValue": "azuredatalake"
      },
      "azuredatalake_1_Connection_DisplayName": {
        "type": "string",
        "defaultValue": "<connection-name>"
      },
      "azuredatalake_1_token:clientId": {
        "type": "securestring",
        "metadata": {
          "description": "Client (or Application) ID of the Azure Active Directory application."
        }
      },
      "azuredatalake_1_token:clientSecret": {
        "type": "securestring",
        "metadata": {
          "description": "Client secret of the Azure Active Directory application."
        }
      },
      "azuredatalake_1_token:TenantId": {
        "type": "securestring",
        "metadata": {
          "description": "The tenant ID of for the Azure Active Directory application."
        }
      },
      "azuredatalake_1_token:resourceUri": {
        "type": "string",
        "metadata": {
          "description": "The resource you are requesting authorization to use."
        }
      },
      "azuredatalake_1_token:grantType": {
        "type": "string",
        "metadata": {
          "description": "Grant type"
        },
        "defaultValue": "client_credentials",
        "allowedValues": [
          "client_credentials"
        ]
      },
      // Other template parameters
   }
   // Other template objects
}
```

Hizmet ilkeleriyle çalışma hakkında daha fazla bilgi için şu konulara bakın:

* [Azure portalını kullanarak bir hizmet ilkesi oluşturma](../active-directory/develop/howto-create-service-principal-portal.md)
* [Azure PowerShell'i kullanarak bir Azure hizmet ilkesi oluşturun](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps)
* [Azure PowerShell'i kullanarak sertifikalı bir hizmet ilkesi oluşturma](../active-directory/develop/howto-authenticate-service-principal-powershell.md)

<a name="parameter-references"></a>

## <a name="references-to-parameters"></a>Parametrelere başvurular

Şablon parametrelerine başvurmak için, dağıtımsırasında değerlendirilen [şablon işlevlerine](../azure-resource-manager/templates/template-functions.md)sahip şablon ifadeleri kullanabilirsiniz. Şablon ifadeleri kare parantez **([]** kullanır):

`"<attribute-name>": "[parameters('<template-parameter-name>')]"`

İş akışı tanımı parametrelerine başvurmak için, çalışma zamanında değerlendirilen [İş Akışı Tanımı Dil ifadelerini ve işlevlerini](../logic-apps/workflow-definition-language-functions-reference.md)kullanırsınız. Bazı şablon işlevlerinin ve iş akışı tanım işlevlerinin aynı ada sahip olduğunu fark edebilirsiniz. İş akışı tanımı ifadeleri "at" sembolü**@** ile başlar ( ):

`"<attribute-name>": "@parameters('<workflow-definition-parameter-name>')"`

Çalışma zamanında kullanmak üzere mantık uygulamanız için şablon parametre değerlerini iş akışı tanımınıza geçirebilirsiniz. Ancak, Mantık Uygulama Tasarımcısı şablon öğelerini desteklemediği için iş akışı tanımınızda şablon parametrelerini, ifadelerini ve sözdizimini kullanmaktan kaçının. Ayrıca, şablon sözdizimi ve ifadeler, ifadelerin ne zaman değerlendirildiği arasındaki farklılıklar nedeniyle kodunuzu zorlaştırabilir.

Bunun yerine, çalışma zamanında kullanılacak iş akışı tanımı parametrelerini bildirmek ve başvurmak, dağıtımsırasında kullanılacak şablon parametrelerini bildirmek ve başvurmak ve bir parametre dosyasını kullanarak dağıtımsırasında geçirilen değerleri belirtmek için aşağıdaki genel adımları izleyin. Tüm ayrıntılar için, bu konunun daha önceki [İş Akışı tanımı ve parametreleri](#workflow-definition-parameters) bölümüne bakın.

1. Şablonunuzu oluşturun ve dağıtımsırasında kabul edilecek ve kullanılacak değerler için şablon parametrelerini bildirin.

1. İş akışı tanımınızda, çalışma zamanında kabul edilecek ve kullanılacak değerler için parametreleri bildirin. Daha sonra bu değerlere iş akışı tanımıboyunca ve içinde başvuruda bulunabilirsiniz.

1. İş `parameters` akışı tanımınızın *dışında* olan ancak yine de mantık uygulamanızın kaynak *tanımının içinde* bulunan nesnede, ilgili şablon parametrelerine başvurarak iş akışı tanımı parametrelerinizin değerlerini ayarlayın. Bu şekilde, şablon parametre değerlerini iş akışı tanımı parametrelerinize geçirebilirsiniz.

1. Parametreler dosyasında, şablonunuzun dağıtımsırasında kullanacağı değerleri belirtin.

<a name="full-example-template"></a>

## <a name="full-example-template"></a>Tam örnek şablonu

Bu konunun örnekleri tarafından kullanılan parametreli örnek şablon aşağıda verilmiştir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
   "parameters": {
      "LogicAppName": {
         "type": "string",
         "minLength": 1,
         "maxLength": 80,
         "defaultValue": "MyLogicApp",
         "metadata": {
            "description": "The resource name to use for the logic app"
         }
      },
      "LogicAppLocation": {
         "type": "string",
         "minLength": 1,
         "defaultValue": "[resourceGroup().location]",
         "metadata": {
            "description": "The resource location to use for the logic app"
         }
      },
      "office365_1_Connection_Name": {
         "type": "string",
         "defaultValue": "office365",
         "metadata": {
            "description": "The resource name to use for the Office 365 Outlook connection"
         }
      },
      "office365_1_Connection_DisplayName": {
         "type": "string",
         "defaultValue": "",
         "metadata": {
            "description": "The display name to use for the Office 365 Outlook connection"
         }
      },
      "azureblob_1_Connection_Name": {
         "type": "string",
         "defaultValue": "azureblob",
         "metadata": {
            "description": "The resource name to use for the Azure Blob storage account connection"
         }
      },
      "azureblob_1_Connection_DisplayName": {
         "type": "string",
         "defaultValue": "",
         "metadata": {
            "description": "Name of the storage account the connector should use."
         },

      },
      "azureblob_1_accountName": {
         "type": "string",
         "defaultValue": "",
         "metadata": {
            "description": "Name of the storage account the connector should use."
         }
      },
      "azureblob_1_accessKey": {
         "type": "securestring",
         "metadata": {
            "description": "Specify a valid primary/secondary storage account access key."
         }
      },
      "LogicAppIntegrationAccount": {
         "type":"string",
         "minLength": 1,
         "defaultValue": "/subscriptions/<Azure-subscription-ID>/resourceGroups/fabrikam-integration-account-rg/providers/Microsoft.Logic/integrationAccounts/fabrikam-integration-account",
         "metadata": {
            "description": "The ID to use for the integration account"
         }
      }
   },
   "variables": {},
   "resources": [
      {
         "properties": {
            "state": "Disabled",
            "integrationAccount": {
              "id": "[parameters('LogicAppIntegrationAccount')]"
            },
            "definition": {
               "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
               "actions": {
                  "Create_blob": {
                     "type": "ApiConnection",
                     "inputs": {
                        "host": {
                           "connection": {
                              "name": "@parameters('$connections')['azureblob']['connectionId']"
                           }
                        },
                     },
                     "method": "post",
                     "body": "@triggerBody()?['Body']",
                     "path": "/datasets/default/files",
                     "queries": {
                        "folderPath": "/emails",
                        "name": "@triggerBody()?['Subject']",
                        "queryParametersSingleEncoded": true
                     },
                     "runAfter": {},
                     "runtimeConfiguration": {
                        "contentTransfer": {
                           "transferMode": "Chunked"
                        }
                     }
                  }
               },
               "parameters": {
                  "$connections": {
                     "defaultValue": {},
                     "type": "Object"
                  }
               },
               "triggers": {
                  "When_a_new_email_arrives": {
                     "type": "ApiConnection",
                     "inputs": {
                        "host": {
                           "connection": {
                              "name": "@parameters('$connections')['office365']['connectionId']"
                           }
                        },
                        "method": "get",
                        "path": "/Mail/OnNewEmail",
                        "queries": {
                           "folderPath": "Inbox",
                           "importance": "Any",
                           "fetchOnlyWithAttachment": false,
                           "includeAttachments": false
                        }
                     },
                     "recurrence": {
                        "frequency": "Day",
                        "interval": 1
                     },
                     "splitOn": "@triggerBody()?['value']"
                  }
               },
               "contentVersion": "1.0.0.0",
               "outputs": {}
            },
            "parameters": {
               "$connections": {
                  "value": {
                     "azureblob": {
                        "id": "[concat(subscription().id, '/providers/Microsoft.Web/locations/', parameters('LogicAppLocation'), '/managedApis/', 'azureblob')]",
                        "connectionId": "[resourceId('Microsoft.Web/connections', parameters('azureblob_1_Connection_Name'))]",
                        "connectionName": "[parameters('azureblob_1_Connection_Name')]"
                     },
                     "office365": {
                        "id": "[concat(subscription().id, '/providers/Microsoft.Web/locations/', parameters('LogicAppLocation'), '/managedApis/', 'office365')]",
                        "connectionId": "[resourceId('Microsoft.Web/connections', parameters('office365_1_Connection_Name'))]",
                        "connectionName": "[parameters('office365_1_Connection_Name')]"
                     }
                  }
               }
            },
            "accessControl": {}
         },
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "dependsOn": [
            "[resourceId('Microsoft.Web/connections', parameters('azureblob_1_Connection_Name'))]",
            "[resourceId('Microsoft.Web/connections', parameters('office365_1_Connection_Name'))]"
         ]
      },
      {
         "type": "MICROSOFT.WEB/CONNECTIONS",
         "apiVersion": "2016-06-01",
         "name": "[parameters('office365_1_Connection_Name')]",
         "location": "[parameters('LogicAppLocation')]",
         "properties": {
            "api": {
                "id": "[concat(subscription().id, '/providers/Microsoft.Web/locations/', parameters('LogicAppLocation'), '/managedApis/', 'office365')]"
            },
            "displayName": "[parameters('office365_1_Connection_DisplayName')]"
         }
      },
      {
         "type": "MICROSOFT.WEB/CONNECTIONS",
         "apiVersion": "2016-06-01",
         "name": "[parameters('azureblob_1_Connection_Name')]",
         "location": "[parameters('LogicAppLocation')]",
         "properties": {
            "api": {
               "id": "[concat(subscription().id, '/providers/Microsoft.Web/locations/', parameters('LogicAppLocation'), '/managedApis/', 'azureblob')]"
            },
            "displayName": "[parameters('azureblob_1_Connection_DisplayName')]",
            "parameterValues": {
               "accountName": "[parameters('azureblob_1_accountName')]",
               "accessKey": "[parameters('azureblob_1_accessKey')]"
            }
         }
      }
   ],
   "outputs": {}
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Mantıksal uygulama şablonları oluşturma](../logic-apps/logic-apps-create-azure-resource-manager-templates.md)
