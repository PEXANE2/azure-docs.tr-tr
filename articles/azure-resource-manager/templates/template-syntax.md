---
title: Şablon yapısı ve sözdizimi
description: Bildirimsel JSON sözdizimini kullanarak Azure Kaynak Yöneticisi şablonlarının yapısını ve özelliklerini açıklar.
ms.topic: conceptual
ms.date: 03/16/2020
ms.openlocfilehash: 4e8334e4ddfaee52c5d1aa68fb8689fcde0a6cbf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79459999"
---
# <a name="understand-the-structure-and-syntax-of-arm-templates"></a>ARM şablonlarının yapısını ve sözdizimini anlama

Bu makalede, Bir Azure Kaynak Yöneticisi (ARM) şablonunun yapısı açıklanmaktadır. Şablonun farklı bölümlerini ve bu bölümlerde kullanılabilen özellikleri sunar.

Bu makale, ARM şablonları ile bazı aşinalık olan kullanıcılar için tasarlanmıştır. Şablonun yapısı hakkında ayrıntılı bilgi sağlar. Şablon oluşturma sürecinde size yol gösteren adım adım bir öğretici için [Bkz. Öğretici: İlk Azure Kaynak Yöneticisi şablonunuzu oluşturun ve dağıtın.](template-tutorial-create-first-template.md)

## <a name="template-format"></a>Şablon biçimi

En basit yapısında şablon aşağıdaki öğelere sahiptir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "",
  "apiProfile": "",
  "parameters": {  },
  "variables": {  },
  "functions": [  ],
  "resources": [  ],
  "outputs": {  }
}
```

| Öğe adı | Gerekli | Açıklama |
|:--- |:--- |:--- |
| $schema |Evet |Şablon dilinin sürümünü açıklayan JSON şema dosyasının konumu. Kullandığınız sürüm numarası dağıtımın kapsamına ve JSON düzenleyicinize bağlıdır.<br><br>[Azure Kaynak Yöneticisi araçları uzantısı ile VS Kodu](use-vs-code-to-create-template.md)kullanıyorsanız, kaynak grubu dağıtımları için en son sürümü kullanın:<br>`https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#`<br><br>Diğer editörler (Visual Studio dahil) bu şemayı işleyebilir. Bu editörler için şunları kullanın:<br>`https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#`<br><br>Abonelik dağıtımları için şunları kullanın:<br>`https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#`<br><br>Yönetim grubu dağıtımları için şunları kullanın:<br>`https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#`<br><br>Kiracı dağıtımları için şunları kullanın:<br>`https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentTemplate.json#` |
| içerikVersiyon |Evet |Şablonun sürümü (1.0.0.0 gibi). Bu öğe için herhangi bir değer sağlayabilir. Şablonunuzdaönemli değişiklikleri belgelemek için bu değeri kullanın. Şablonu kullanarak kaynakları dağıtırken, bu değer doğru şablonun kullanıldığından emin olmak için kullanılabilir. |
| apiProfil |Hayır | Kaynak türleri için API sürümleri koleksiyonu olarak hizmet veren bir API sürümü. Şablondaki her kaynak için API sürümlerini belirtmek zorunda kalmamak için bu değeri kullanın. Bir API profil sürümü belirttiğiniz ve kaynak türü için bir API sürümü belirtmediğinizde, Kaynak Yöneticisi profilde tanımlanan bu kaynak türü için API sürümünü kullanır.<br><br>API profil özelliği, özellikle bir şablonu Azure Yığını ve genel Azure gibi farklı ortamlara dağıtırken yararlıdır. Şablonunuzun her iki ortamda da desteklenen sürümleri otomatik olarak kullandığından emin olmak için API profil sürümünü kullanın. Geçerli API profil sürümlerinin ve profilde tanımlanan kaynakların API sürümlerinin listesi için [BKZ.](https://github.com/Azure/azure-rest-api-specs/tree/master/profile)<br><br>Daha fazla bilgi için [API profillerini kullanarak sürümlerini izleyin'e](templates-cloud-consistency.md#track-versions-using-api-profiles)bakın. |
| [Parametre](#parameters) |Hayır |Kaynak dağıtımını özelleştirmek için dağıtım yürütüldüğünde sağlanan değerler. |
| [Değişken](#variables) |Hayır |Şablon dil ifadelerini basitleştirmek için şablonda JSON parçaları olarak kullanılan değerler. |
| [Işlev](#functions) |Hayır |Şablon içinde kullanılabilen kullanıcı tanımlı işlevler. |
| [Kaynak](#resources) |Evet |Kaynak grubunda veya abonelikte dağıtılan veya güncelleştirilen kaynak türleri. |
| [Çıkış](#outputs) |Hayır |Dağıtımdan sonra döndürülen değerler. |

Her öğenin ayarlayabileceğiniz özellikleri vardır. Bu makalede, şablonun bölümleri daha ayrıntılı olarak açıklanmaktadır.

## <a name="parameters"></a>Parametreler

Şablonun parametreler bölümünde, kaynakları dağıtırken hangi değerleri giriş yapabileceğinizi belirtirsiniz. Bir şablonda 256 parametreyle sınırlısınız. Birden çok özellik içeren nesneler kullanarak parametre sayısını azaltabilirsiniz.

Bir parametre için kullanılabilir özellikler şunlardır:

```json
"parameters": {
  "<parameter-name>" : {
    "type" : "<type-of-parameter-value>",
    "defaultValue": "<default-value-of-parameter>",
    "allowedValues": [ "<array-of-allowed-values>" ],
    "minValue": <minimum-value-for-int>,
    "maxValue": <maximum-value-for-int>,
    "minLength": <minimum-length-for-string-or-array>,
    "maxLength": <maximum-length-for-string-or-array-parameters>,
    "metadata": {
      "description": "<description-of-the parameter>"
    }
  }
}
```

| Öğe adı | Gerekli | Açıklama |
|:--- |:--- |:--- |
| parametre adı |Evet |Parametrenin adı. Geçerli bir JavaScript tanımlayıcısı olmalıdır. |
| type |Evet |Parametre değerinin türü. İzin verilen türleri ve değerleri **dize**, **securestring**, **int**, **bool**, **nesne**, **secureObject**, ve **dizi**. Bkz. [Veri türleri.](#data-types) |
| defaultDeğer |Hayır |Parametre için değer sağlanmadıysa, parametre için varsayılan değer. |
| Allowedvalues |Hayır |Doğru değerin sağlandıklarından emin olmak için parametre için izin verilen değerler dizisi. |
| Minvalue |Hayır |Int türü parametreleri için minimum değer, bu değer kapsayıcıdır. |
| Maxvalue |Hayır |Int türü parametreleri için maksimum değer, bu değer kapsayıcıdır. |
| Minlength |Hayır |Dize, güvenli dize ve dizi türü parametreleri için minimum uzunluk, bu değer kapsayıcıdır. |
| Maxlength |Hayır |Dize, güvenli dize ve dizi türü parametreleri için maksimum uzunluk, bu değer kapsayıcıdır. |
| açıklama |Hayır |Portal üzerinden kullanıcılara görüntülenen parametrenin açıklaması. Daha fazla bilgi için [şablondaki Açıklamalar'a](#comments)bakın. |

Parametrelerin nasıl kullanılacağına örnekler için Azure [Kaynak Yöneticisi şablonlarında Parametreler'e](template-parameters.md)bakın.

### <a name="data-types"></a>Veri türleri

Satır içi parametreler olarak geçirilen tümsalar için, değerler aralığı dağıtım için kullandığınız SDK veya komut satırı aracıyla sınırlanabilir. Örneğin, bir şablon dağıtmak için PowerShell'i kullanırken, tamsayı türleri -2147483648 ile 2147483647 arasında değişebilir. Bu sınırlamayı önlemek için [parametre dosyasında](parameter-files.md)büyük tamsayı değerlerini belirtin. Kaynak türleri, arasttırıç özellikleri için kendi sınırlarını uygular.

Şablonunuzda boolean ve tamsayı değerleri belirtirken, değeri tırnak işaretleriyle çevrelemayın. Çift tırnak işaretleriyle dize değerlerini başlatın ve sonlandırmayın.

Nesneler sol ayraçla başlar ve sağ ayraçla biter. Diziler sol bir parantezle başlar ve sağ bir ayraçla sona erer.

Güvenli dizeleri ve güvenli nesneler kaynak dağıtımından sonra okunamıyor.

Veri türlerini biçimlendirme örnekleri için [Parametre türü biçimlerine](parameter-files.md#parameter-type-formats)bakın.

## <a name="variables"></a>Değişkenler

Değişkenler bölümünde, şablonunuzun her yerinde kullanılabilecek değerler oluşturuyorsunuz. Değişkenleri tanımlamanız gerekmez, ancak bunlar genellikle karmaşık ifadeleri azaltarak şablonunuzu basitleştirir.

Aşağıdaki örnekte, bir değişkeni tanımlamak için kullanılabilir seçenekler gösterilmektedir:

```json
"variables": {
  "<variable-name>": "<variable-value>",
  "<variable-name>": {
    <variable-complex-type-value>
  },
  "<variable-object-name>": {
    "copy": [
      {
        "name": "<name-of-array-property>",
        "count": <number-of-iterations>,
        "input": <object-or-value-to-repeat>
      }
    ]
  },
  "copy": [
    {
      "name": "<variable-array-name>",
      "count": <number-of-iterations>,
      "input": <object-or-value-to-repeat>
    }
  ]
}
```

Bir değişken `copy` için çeşitli değerler oluşturmak için kullanma hakkında bilgi için Değişken [yineleme'ye](copy-variables.md)bakın.

Değişkenlerin nasıl kullanılacağına örnekler için [Azure Kaynak Yöneticisi şablonundaki Değişkenler bölümüne](template-variables.md)bakın.

## <a name="functions"></a>İşlevler

Şablonunuzda kendi işlevlerinizi oluşturabilirsiniz. Bu işlevler şablonunuzda kullanılabilir. Genellikle, şablonunuzun her içinde yinelemek istemediğiniz karmaşık ifadeler tanımlarsınız. Şablonlarda desteklenen ifadeler [den](template-functions.md) kullanıcı tanımlı işlevleri oluşturursunuz.

Bir kullanıcı işlevini tanımlarken bazı kısıtlamalar vardır:

* İşlev değişkenlere erişemez.
* İşlev yalnızca işlevde tanımlanan parametreleri kullanabilir. Kullanıcı tanımlı bir işlev içindeki [parametreler işlevini](template-functions-deployment.md#parameters) kullandığınızda, bu işlevin parametreleri ile sınırlısınız.
* İşlev, kullanıcı tarafından tanımlanan diğer işlevleri çağıramaz.
* İşlev [başvuru işlevini](template-functions-resource.md#reference)kullanamaz.
* İşlev için parametreler varsayılan değerlere sahip olamaz.

```json
"functions": [
  {
    "namespace": "<namespace-for-functions>",
    "members": {
      "<function-name>": {
        "parameters": [
          {
            "name": "<parameter-name>",
            "type": "<type-of-parameter-value>"
          }
        ],
        "output": {
          "type": "<type-of-output-value>",
          "value": "<function-return-value>"
        }
      }
    }
  }
],
```

| Öğe adı | Gerekli | Açıklama |
|:--- |:--- |:--- |
| ad alanı |Evet |Özel işlevler için ad alanı. Şablon işlevleriyle çakışmaları adlandırmayı önlemek için kullanın. |
| fonksiyon adı |Evet |Özel işlevin adı. İşlev iþlemi aramadan, işlev adini ad alani ile birleştirin. Örneğin, namespace contoso uniqueName adlı bir işlevi `"[contoso.uniqueName()]"`çağırmak için kullanın. |
| parametre adı |Hayır |Özel işlev içinde kullanılacak parametrenin adı. |
| parametre değeri |Hayır |Parametre değerinin türü. İzin verilen türleri ve değerleri **dize**, **securestring**, **int**, **bool**, **nesne**, **secureObject**, ve **dizi**. |
| çıkış türü |Evet |Çıkış değerinin türü. Çıkış değerleri işlev giriş parametreleri ile aynı türleri destekler. |
| çıkış değeri |Evet |Değerlendirilen ve işlevden döndürülen şablon dil ifadesi. |

Özel işlevlerin nasıl kullanılacağına örnekler için Azure [Kaynak Yöneticisi şablonundaki Kullanıcı tanımlı işlevlere](template-user-defined-functions.md)bakın.

## <a name="resources"></a>Kaynaklar

Kaynaklar bölümünde, dağıtılan veya güncelleştirilen kaynakları tanımlarsınız.

Kaynakları aşağıdaki yapıyla tanımlarsınız:

```json
"resources": [
  {
      "condition": "<true-to-deploy-this-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "apiVersion": "<api-version-of-resource>",
      "name": "<name-of-the-resource>",
      "comments": "<your-reference-notes>",
      "location": "<location-of-resource>",
      "dependsOn": [
          "<array-of-related-resource-names>"
      ],
      "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
      },
      "sku": {
          "name": "<sku-name>",
          "tier": "<sku-tier>",
          "size": "<sku-size>",
          "family": "<sku-family>",
          "capacity": <sku-capacity>
      },
      "kind": "<type-of-resource>",
      "copy": {
          "name": "<name-of-copy-loop>",
          "count": <number-of-iterations>,
          "mode": "<serial-or-parallel>",
          "batchSize": <number-to-deploy-serially>
      },
      "plan": {
          "name": "<plan-name>",
          "promotionCode": "<plan-promotion-code>",
          "publisher": "<plan-publisher>",
          "product": "<plan-product>",
          "version": "<plan-version>"
      },
      "properties": {
          "<settings-for-the-resource>",
          "copy": [
              {
                  "name": ,
                  "count": ,
                  "input": {}
              }
          ]
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| Öğe adı | Gerekli | Açıklama |
|:--- |:--- |:--- |
| Durum | Hayır | Kaynağın bu dağıtım sırasında sağlanıp sağlanmayacağını gösteren boolean değeri. Kaynak `true`dağıtım sırasında oluşturulduğunda. Ne `false`zaman , kaynak bu dağıtım için atlanır. [Bkz. koşul](conditional-resource-deployment.md). |
| type |Evet |Kaynağın türü. Bu değer, kaynak sağlayıcısının ad alanı nın ve kaynak türünün **(Microsoft.Storage/storageAccounts**gibi) bir leşimidir. Kullanılabilir değerleri belirlemek için [şablon başvurusuna](/azure/templates/)bakın. Alt kaynak için, türün biçimi üst kaynağın içinde iç içe mi yoksa üst kaynağın dışında mı tanımlandığına bağlıdır. Bkz. [Alt kaynaklar için ad ve yazı yı ayarla.](child-resource-name-type.md) |
| apiVersion |Evet |Kaynak oluşturmak için kullanmak IÇIN REST API sürümü. Kullanılabilir değerleri belirlemek için [şablon başvurusuna](/azure/templates/)bakın. |
| ad |Evet |Kaynağın adı. Ad, RFC3986'da tanımlanan URI bileşen kısıtlamalarını izlemelidir. Kaynak adını dış taraflara ifşa eden Azure hizmetleri, başka bir kimliği taklit etme girişimi olmadığından emin olmak için adı doğrular. Alt kaynak için, adın biçimi üst kaynağın içinde iç içe mi yoksa üst kaynağın dışında mı tanımlandığına bağlıdır. Bkz. [Alt kaynaklar için ad ve yazı yı ayarla.](child-resource-name-type.md) |
| Yorum |Hayır |Şablonunuzdaki kaynakları belgelemek için notlarınız. Daha fazla bilgi için [şablondaki Açıklamalar'a](template-syntax.md#comments)bakın. |
| location |Değişir |Sağlanan kaynağın desteklenen coğrafi konumları. Kullanılabilir konumlardan herhangi birini seçebilirsiniz, ancak genellikle kullanıcılarınıza yakın bir konum seçmek mantıklıdır. Genellikle, aynı bölgede birbirleriyle etkileşim kaynakları yerleştirmek de mantıklı. Çoğu kaynak türü bir konum gerektirir, ancak bazı türler (rol ataması gibi) bir konum gerektirmez. Bkz. [Kaynak konumunu ayarla.](resource-location.md) |
| dependsOn |Hayır |Bu kaynak dağıtılmadan önce dağıtılması gereken kaynaklar. Kaynak Yöneticisi, kaynaklar arasındaki bağımlılıkları değerlendirir ve bunları doğru sırada dağıtır. Kaynaklar birbirine bağımlı olmadığında, paralel olarak dağıtılırlar. Değer, kaynak adlarının veya kaynak benzersiz tanımlayıcılarının virgülle ayrılmış bir listesi olabilir. Yalnızca bu şablonda dağıtılan kaynakları listeleyin. Bu şablonda tanımlanmayan kaynaklar zaten var olmalıdır. Dağıtımınızı yavaşlatıp dairesel bağımlılıklar oluşturabilecekleri için gereksiz bağımlılıklar eklemekten kaçının. Bağımlılıkları ayarlama kılavuzu için bkz: [Azure Kaynak Yöneticisi şablonlarında bağımlılıkları tanımlama.](define-resource-dependency.md) |
| etiketler |Hayır |Kaynakla ilişkili etiketler. Aboneliğinizdeki kaynakları mantıksal olarak düzenlemek için etiketler uygulayın. |
| Sku | Hayır | Bazı kaynaklar, SKU'yu tanımlayan değerlerin dağıtılmasına izin verir. Örneğin, bir depolama hesabıiçin artıklık türünü belirtebilirsiniz. |
| Tür | Hayır | Bazı kaynaklar, dağıttEttiğiniz kaynak türünü tanımlayan bir değere izin verir. Örneğin, oluşturmak için Cosmos DB türünü belirtebilirsiniz. |
| copy |Hayır |Birden fazla örnek gerekiyorsa, oluşturulacak kaynak sayısı. Varsayılan mod paraleldir. Tümün veya kaynakların aynı anda dağıtılmasını istemediğinde seri modunu belirtin. Daha fazla bilgi için azure [kaynak yöneticisinde birkaç kaynak örneği oluşturma bölümüne](copy-resources.md)bakın. |
| plan | Hayır | Bazı kaynaklar, dağıtımı için planı tanımlayan değerlere izin verir. Örneğin, sanal bir makine için pazar görüntüsünü belirtebilirsiniz. |
| properties |Hayır |Kaynağa özel yapılandırma ayarları. Özelliklerin değerleri, kaynağı oluşturmak için REST API işlemi (PUT yöntemi) için istek gövdesinde sağladığınız değerlerle aynıdır. Ayrıca, bir özelliğin birkaç örneğini oluşturmak için bir kopya dizisi de belirtebilirsiniz. Kullanılabilir değerleri belirlemek için [şablon başvurusuna](/azure/templates/)bakın. |
| kaynaklar |Hayır |Tanımlanan kaynağa bağlı alt kaynaklar. Yalnızca ana kaynağın şeması tarafından izin verilen kaynak türleri sağlayın. Üst kaynağa bağımlılık ima edilmez. Bu bağımlılığı açıkça tanımlamanız gerekir. Bkz. [Alt kaynaklar için ad ve yazı yı ayarla.](child-resource-name-type.md) |

## <a name="outputs"></a>Çıkışlar

Çıktılar bölümünde, dağıtımdan döndürülen değerleri belirtirsiniz. Genellikle, dağıtılan kaynaklardan değerleri döndürür.

Aşağıdaki örnek, bir çıktı tanımının yapısını gösterir:

```json
"outputs": {
  "<output-name>": {
    "condition": "<boolean-value-whether-to-output-value>",
    "type": "<type-of-output-value>",
    "value": "<output-value-expression>",
    "copy": {
      "count": <number-of-iterations>,
      "input": <values-for-the-variable>
    }
  }
}
```

| Öğe adı | Gerekli | Açıklama |
|:--- |:--- |:--- |
| çıktı adı |Evet |Çıktı değerinin adı. Geçerli bir JavaScript tanımlayıcısı olmalıdır. |
| Durum |Hayır | Bu çıktı değerinin döndürülüp döndürülmediğini gösteren boolean değeri. Ne `true`zaman , değer dağıtım için çıktı dahil edilir. Ne `false`zaman , çıktı değeri bu dağıtım için atlanır. Belirtilmediğinde, varsayılan değer `true`. |
| type |Evet |Çıkış değerinin türü. Çıktı değerleri şablon giriş parametreleri ile aynı türleri destekler. Çıktı türü için **güvenli dize** belirtirseniz, değer dağıtım geçmişinde görüntülenmez ve başka bir şablondan alınamaz. Birden fazla şablonda gizli bir değer kullanmak için, sırrı Bir Anahtar Kasası'nda saklayın ve parametre dosyasındaki gizliye başvurun. Daha fazla bilgi için, [dağıtım sırasında güvenli parametre değerini geçmek için Azure Anahtar Kasası'nı kullanın'a](key-vault-parameter.md)bakın. |
| value |Hayır |Çıktı değeri olarak değerlendirilen ve döndürülen şablon dil ifadesi. **Değer** veya **kopya**belirtin. |
| copy |Hayır | Bir çıktı için birden fazla değer döndürmek için kullanılır. **Değer** veya **kopya**belirtin. Daha fazla bilgi için Azure [Kaynak Yöneticisi şablonlarında Çıktı yinelemesi'ne](copy-outputs.md)bakın. |

Çıktıların nasıl kullanılacağına örnekler için Azure [Kaynak Yöneticisi şablonundaki Çıktılar'a](template-outputs.md)bakın.

<a id="comments" />

## <a name="comments-and-metadata"></a>Yorumlar ve meta veriler

Şablonunuza yorum ve meta veri eklemek için birkaç seçeneğiniz var.

### <a name="comments"></a>Yorumlar

Satır satırlı yorumlar için, `//` `/* ... */` ya da kullanabilirsiniz, ancak bu sözdizimi tüm araçlarla çalışmıyor. Satır ara yorumları olan şablonlar üzerinde çalışmak için portal şablondüzenleyicisini kullanamazsınız. Bu yorum stilini eklerseniz, kullandığınız araçların satır satırlı JSON yorumlarını desteklediğinden emin olun.

> [!NOTE]
> Azure CLI kullanarak şablonları açıklamalarla dağıtmak için `--handle-extended-json-format` anahtarı kullanmanız gerekir.

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2018-10-01",
  "name": "[variables('vmName')]", // to customize name, change it in variables
  "location": "[parameters('location')]", //defaults to resource group location
  "dependsOn": [ /* storage account and network interface must be deployed first */
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
  ],
```

Visual Studio Code'da, [Azure Kaynak Yöneticisi Araçları uzantısı](use-vs-code-to-create-template.md#install-resource-manager-tools-extension) Kaynak Yöneticisi şablonunu otomatik olarak algılayabilir ve dil modunu buna göre değiştirebilir. VS Code'un sağ alt köşesinde **Azure Kaynak Yöneticisi Şablonu'nu** görürseniz, satır içi açıklamaları kullanabilirsiniz. Satır satır açıklamaları artık geçersiz olarak işaretlenmez.

![Visual Studio Code Azure Kaynak Yöneticisi şablon modu](./media/template-syntax/resource-manager-template-editor-mode.png)

### <a name="metadata"></a>Meta Veriler

Şablonunuzda hemen `metadata` hemen her yere bir nesne ekleyebilirsiniz. Kaynak Yöneticisi nesneyi yoksa, ancak JSON düzenleyiciniz özelliğin geçerli olmadığı konusunda sizi uyarabilir. Nesnede, gereksinim duyduğunuz özellikleri tanımlayın.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "metadata": {
    "comments": "This template was developed for demonstration purposes.",
    "author": "Example Name"
  },
```

**Parametreler için,** `metadata` özelliği olan `description` bir nesne ekleyin.

```json
"parameters": {
  "adminUsername": {
    "type": "string",
    "metadata": {
      "description": "User name for the Virtual Machine."
    }
  },
```

Şablonu portal üzerinden dağıtırken, açıklamada sağladığınız metin otomatik olarak bu parametre için ipucu olarak kullanılır.

![Parametre ucunu göster](./media/template-syntax/show-parameter-tip.png)

**Kaynaklar**için bir `comments` öğe veya meta veri nesnesi ekleyin. Aşağıdaki örnekte hem yorum öğesi hem de meta veri nesnesi gösterilmektedir.

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2018-07-01",
    "name": "[concat('storage', uniqueString(resourceGroup().id))]",
    "comments": "Storage account used to store VM disks",
    "location": "[parameters('location')]",
    "metadata": {
      "comments": "These tags are needed for policy compliance."
    },
    "tags": {
      "Dept": "[parameters('deptName')]",
      "Environment": "[parameters('environment')]"
    },
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
  }
]
```

**Çıktılar**için çıktı değerine bir meta veri nesnesi ekleyin.

```json
"outputs": {
  "hostname": {
    "type": "string",
    "value": "[reference(variables('publicIPAddressName')).dnsSettings.fqdn]",
    "metadata": {
      "comments": "Return the fully qualified domain name"
    }
  },
```

Kullanıcı tanımlı işlevlere meta veri nesnesi ekemezsiniz.

## <a name="multi-line-strings"></a>Çok çizgili dizeleri

Bir dizeyi birden çok satıra kırabilirsiniz. Örneğin, aşağıdaki JSON örneğindeki konum özelliğine ve yorumlardan birine bakın.

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2018-10-01",
  "name": "[variables('vmName')]", // to customize name, change it in variables
  "location": "[
    parameters('location')
    ]", //defaults to resource group location
  /*
    storage account and network interface
    must be deployed first
  */
  "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
  ],
```

Azure CLI kullanarak şablonları çok satırlı dizeleri dağıtmak `--handle-extended-json-format` için anahtarı kullanmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* Farklı türlerde çözümler için tam şablonları görüntülemek üzere bkz. [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/documentation/templates/).
* Şablon un içinden kullanabileceğiniz işlevler hakkında ayrıntılı bilgi için [Azure Kaynak Yöneticisi Şablon İşlevlerini](template-functions.md)görün.
* Dağıtım sırasında birkaç şablonu birleştirmek için [bkz.](linked-templates.md)
* Şablon oluşturma yla ilgili öneriler için Azure [Kaynak Yöneticisi şablonu en iyi uygulamaları](template-best-practices.md)görün.
* Tüm Azure ortamlarında ve Azure Yığını'nda kullanabileceğiniz Kaynak Yöneticisi şablonları oluşturma yla ilgili öneriler için bulut [tutarlılığı için Azure Kaynak Yöneticisi şablonları geliştir'e](templates-cloud-consistency.md)bakın.
