---
title: Dinamik planlar oluşturmak için parametreleri kullanma
description: Statik ve dinamik parametreler ve güvenli ve dinamik planlar oluşturmak için bunları nasıl kullanacağınız hakkında bilgi edinin.
ms.date: 04/15/2020
ms.topic: conceptual
ms.openlocfilehash: e5953617d5fa27098380f3f0e95843c69800f823
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81458497"
---
# <a name="creating-dynamic-blueprints-through-parameters"></a>Parametreler aracılığıyla dinamik planlar oluşturma

Çeşitli yapılarla (kaynak grupları, Kaynak Yöneticisi şablonları, ilkeler veya rol atamaları gibi) tam olarak tanımlanmış bir plan, Azure'daki nesnelerin hızlı oluşturulmasını ve tutarlı oluşturulmasını sağlar. Azure Blueprints, bu yeniden kullanılabilir tasarım desenlerinin ve kapsayıcılarının esnek kullanımını etkinleştirmek için parametreleri destekler. Parametre, plan tarafından dağıtılan yapılarüzerindeki özellikleri değiştirmek için hem tanım hem de atama sırasında esneklik oluşturur.

Basit bir örnek kaynak grubu artifakıdır. Bir kaynak grubu oluşturulduğunda, sağlanması gereken iki gerekli değere sahiptir: ad ve konum. Planınıza bir kaynak grubu eklerken, parametreler yoksa, planın her kullanımı için bu adı ve konumu tanımlarsınız. Bu yineleme, aynı kaynak grubunda yapı oluşturmak için planın her kullanımına neden olur. Bu kaynak grubunun içindeki kaynaklar çoğaltılır ve bir çakışma neden olur.

> [!NOTE]
> İki farklı planın aynı ada sahip bir kaynak grubu içermesi sorun değildir.
> Plana dahil edilen bir kaynak grubu zaten varsa, plan bu kaynak grubunda ilgili yapıları oluşturmaya devam eder. Bu, aynı ada ve kaynak türüne sahip iki kaynak bir abonelik içinde var olamayacağından çakışma neden olabilir.

Bu sorunun çözümü parametrelerdir. Azure Blueprints, bir aboneliğe atama sırasında yapının her özelliğinin değerini tanımlamanıza olanak tanır. Parametre, çakışmadan tek bir abonelik içinde kaynak grubu ve diğer kaynaklar oluşturan bir planı yeniden kullanmayı mümkün kılar.

## <a name="blueprint-parameters"></a>Şema parametreleri

REST API aracılığıyla, parametreler planın kendisi üzerinde oluşturulabilir. Bu parametreler, desteklenen yapıtların her birinin parametrelerinden farklıdır. Planda bir parametre oluşturulduğunda, bu plandaki yapılar tarafından kullanılabilir. Kaynak grubunun adlandırılması için önek bir örnek olabilir. Yapı, "çoğunlukla dinamik" bir parametre oluşturmak için plan parametresini kullanabilir. Parametre atama sırasında da tanımlanabildiği için, bu desen adlandırma kurallarına uyacak bir tutarlılık sağlar. Adımlar için statik [parametreleri ayarlama - plan düzeyi parametresi.](#blueprint-level-parameter)

### <a name="using-securestring-and-secureobject-parameters"></a>secureString ve secureObject parametrelerini kullanma

Kaynak Yöneticisi _şablonu yapı,_ **secureString** ve **secureObject** türlerinin parametrelerini desteklerken, Azure Planları her birinin bir Azure Anahtar Kasası ile bağlanmasını gerektirir. Bu güvenlik önlemi, Blueprint ile birlikte sırları depolamanın güvensiz bir şekilde uygulanmasını önler ve güvenli desenlerin istihdamını teşvik eder. Azure Blueprints, bir Kaynak Yöneticisi _şablonu ekskayında_güvenli parametrenin eklenmesini algılayarak bu güvenlik önlemini destekler. Hizmet daha sonra algılanan güvenli parametre başına aşağıdaki Key Vault özellikleri için atama sırasında ister:

- Anahtar Vault kaynak kimliği
- Anahtar Vault gizli adı
- Key Vault gizli sürümü

Plan ataması **sistem tarafından atanmış yönetilen**bir kimlik kullanıyorsa, başvurulan Anahtar Kasası, plan tanımının atandığı aynı abonelikte _bulunmalıdır._

Plan ataması **kullanıcı tarafından atanan yönetilen**bir kimlik kullanıyorsa, başvurulan Anahtar Kasası merkezi bir abonelikte _bulunabilir._ Yönetilen kimlik, plan atamasından önce Key Vault'ta uygun haklara hak tanımalıdır.

> [!IMPORTANT]
> Her iki durumda da, **Access ilkeleri** sayfasında yapılandırılan şablon dağıtımı için Anahtar Kasası'nın Azure **Kaynak Yöneticisi'ne erişimi etkinleştirme** olmalıdır. Bu özelliğin nasıl etkinleştirilen yol tarifi için [Bkz. Anahtar Kasası - Şablon dağıtımını etkinleştir.](../../../azure-resource-manager/managed-applications/key-vault-access.md#enable-template-deployment)

Azure Key Vault hakkında daha fazla bilgi için [Key Vault Genel Bakış'a](../../../key-vault/general/overview.md)bakın.

## <a name="parameter-types"></a>Parametre türleri

### <a name="static-parameters"></a>Statik parametreler

Planın tanımında tanımlanan bir parametre **değeristatik parametre**olarak adlandırılır, çünkü planın her kullanımı bu statik değeri kullanarak yapıyı dağıtacaktır. Kaynak grubu örneğinde, kaynak grubunun adı anlamlı olmasa da, konum için anlamlı olabilir. Daha sonra, planın her ataması, atama sırasında adı ne olursa olsun, aynı konumda kaynak grubu oluşturur. Bu esneklik, atama sırasında değiştirilebilenlere karşı gerekli olarak tanımladığınız şeyde seçici olmanızı sağlar.

#### <a name="setting-static-parameters-in-the-portal"></a>Portalda statik parametrelerin ayarlanması

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki sayfadan **Blueprint tanımlarını** seçin.

1. Varolan bir plana tıklayın ve ardından **planı Edit** OR + **Plan Oluştur'u** tıklatın ve **Temel Bilgiler** sekmesindeki bilgileri doldurun.

1. **Sonraki'yi tıklatın: Eserler** VEYA **Eserler** sekmesine tıklayın.

1. Parametre seçenekleri olan plana eklenen yapılar, **Parametreler** sütununda **doldurulan Y parametrelerinin X'ini** görüntüler. Yapı parametrelerini yeniden diziletmek için yapı satırına tıklayın.

   :::image type="content" source="../media/parameters/parameter-column.png" alt-text="Plan tanımındaki plan parametreleri" border="false":::

1. **Yapıyı Edit** sayfası, tıklanan yapıya uygun değer seçeneklerini görüntüler. Yapıdaki her parametrenin bir başlığı, değer kutusu ve onay kutusu vardır. Kutuyu statik bir **parametre**yapmak için işaretlenmemiş olarak ayarlayın. Aşağıdaki örnekte, yalnızca _Konum,_ işaretlenmeden ve _Kaynak Grubu Adı_ denetlenirken statik bir **parametredir.**

   :::image type="content" source="../media/parameters/static-parameter.png" alt-text="Bir plan artifakı üzerindeki statik parametreleri planla" border="false":::

#### <a name="setting-static-parameters-from-rest-api"></a>REST API'den statik parametreleri ayarlama

Her bir REST API URI'sinde kendi değerlerinizle değiştirmeniz gereken değişkenler bulunur:

- `{YourMG}` - Yönetim grubunuzun adıyla değiştirin
- `{subscriptionId}` - Abonelik kimliğinizle değiştirin

##### <a name="blueprint-level-parameter"></a>Plan düzeyi parametresi

REST API ile bir plan oluştururken, [plan parametreleri oluşturmak mümkündür.](#blueprint-parameters) Bunu yapmak için aşağıdaki REST API URI ve gövde biçimini kullanın:

- REST API URI'si

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint?api-version=2018-11-01-preview
  ```

- İstek Gövdesi

  ```json
  {
      "properties": {
          "description": "This blueprint has blueprint level parameters.",
          "targetScope": "subscription",
          "parameters": {
              "owners": {
                  "type": "array",
                  "metadata": {
                      "description": "List of AAD object IDs that is assigned Owner role at the resource group"
                  }
              }
          },
          "resourceGroups": {
              "storageRG": {
                  "description": "Contains the resource template deployment and a role assignment."
              }
          }
      }
  }
  ```

Bir plan düzeyi parametresi oluşturulduktan sonra, bu plana eklenen yapılarda kullanılabilir.
Aşağıdaki REST API örneği, planüzerinde bir rol atama artifakı oluşturur ve plan düzeyi parametresini kullanır.

- REST API URI'si

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint/artifacts/roleOwner?api-version=2018-11-01-preview
  ```

- İstek Gövdesi

  ```json
  {
      "kind": "roleAssignment",
      "properties": {
          "resourceGroup": "storageRG",
          "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
          "principalIds": "[parameters('owners')]"
      }
  }
  ```

Bu örnekte, **principalIds** özelliği **sahipleri** plan düzeyi parametre `[parameters('owners')]`sini kullanarak . Plan düzeyi parametresini kullanarak bir yapı üzerinde parametre ayarlama hala statik bir **parametre**örneğidir. Plan düzeyi parametresi plan ataması sırasında ayarlanamaz ve her atamada aynı değerde olacaktır.

##### <a name="artifact-level-parameter"></a>Yapı düzeyi parametresi

Bir yapı üzerinde **statik parametreler** oluşturmak benzerdir, ancak `parameters()` işlevi kullanmak yerine düz bir değer alır. Aşağıdaki örnekte iki statik parametre oluşturur, **tagName** ve **tagValue**. Her birinin değeri doğrudan sağlanır ve işlev çağrısı kullanmaz.

- REST API URI'si

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint/artifacts/policyStorageTags?api-version=2018-11-01-preview
  ```

- İstek Gövdesi

  ```json
  {
      "kind": "policyAssignment",
      "properties": {
          "description": "Apply storage tag and the parameter also used by the template to resource groups",
          "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/49c88fc8-6fd1-46fd-a676-f12d1d3a4c71",
          "parameters": {
              "tagName": {
                  "value": "StorageType"
              },
              "tagValue": {
                  "value": "Premium_LRS"
              }
          }
      }
  }
  ```

### <a name="dynamic-parameters"></a>Dinamik parametreler

Statik parametrenin tersi dinamik bir **parametredir.** Bu parametre planda tanımlanmaz, ancak bunun yerine planın her ataması sırasında tanımlanır. Kaynak grubu örneğinde, dinamik bir **parametrenin** kullanılması kaynak grubu adı için anlamlıdır. Bu plan her atama için farklı bir ad sağlar. Plan işlevleri listesi için [plan işlevleri](../reference/blueprint-functions.md) başvurusuna bakın.

#### <a name="setting-dynamic-parameters-in-the-portal"></a>Portalda dinamik parametrelerin ayarlanması

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki sayfadan **Blueprint tanımlarını** seçin.

1. Atamak istediğiniz plana sağ tıklayın. **Blueprint Atla'yı** seçin veya atamak istediğiniz plana tıklayın, ardından **Plan** Atla düğmesini tıklatın.

1. Plan **Atama** sayfasında, **Artefakt parametreleri** bölümünü bulun. En az bir **dinamik parametreye** sahip her yapı, yapıyı ve yapılandırma seçeneklerini görüntüler. Planı atamadan önce parametrelere gerekli değerleri sağlayın. Aşağıdaki örnekte, _Ad_ plan atamasını tamamlamak için tanımlanması gereken dinamik bir **parametredir.**

   :::image type="content" source="../media/parameters/dynamic-parameter.png" alt-text="Plan ataması sırasında plan dinamik parametresi" border="false":::

#### <a name="setting-dynamic-parameters-from-rest-api"></a>REST API'den dinamik parametrelerin ayarlanması

Atama sırasında **dinamik parametrelerin** ayarlanması doğrudan değer girilerek yapılır. [Parametreler()](../reference/blueprint-functions.md#parameters)gibi bir işlev kullanmak yerine, sağlanan değer uygun bir dizedir. Kaynak grubu için yapılar "şablon adı", **adı**ve **konum** özellikleriyle tanımlanır. Dahil edilen yapı için diğer tüm parametreler ** \<\> ** bir ad ve **değer** anahtar çifti ile **parametreler** altında tanımlanır. Plan, atama sırasında sağlanmayan dinamik bir parametre için yapılandırılırsa, atama başarısız olur.

- REST API URI'si

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Blueprint/blueprintAssignments/assignMyBlueprint?api-version=2018-11-01-preview
  ```

- İstek Gövdesi

  ```json
  {
      "properties": {
          "blueprintId": "/providers/Microsoft.Management/managementGroups/{YourMG}  /providers/Microsoft.Blueprint/blueprints/MyBlueprint",
          "resourceGroups": {
              "storageRG": {
                  "name": "StorageAccount",
                  "location": "eastus2"
              }
          },
          "parameters": {
              "storageAccountType": {
                  "value": "Standard_GRS"
              },
              "tagName": {
                  "value": "CostCenter"
              },
              "tagValue": {
                  "value": "ContosoIT"
              },
              "contributors": {
                  "value": [
                      "7be2f100-3af5-4c15-bcb7-27ee43784a1f",
                      "38833b56-194d-420b-90ce-cff578296714"
                  ]
                },
              "owners": {
                  "value": [
                      "44254d2b-a0c7-405f-959c-f829ee31c2e7",
                      "316deb5f-7187-4512-9dd4-21e7798b0ef9"
                  ]
              }
          }
      },
      "identity": {
          "type": "systemAssigned"
      },
      "location": "westus"
  }
  ```

## <a name="next-steps"></a>Sonraki adımlar

- [Plan işlevleri](../reference/blueprint-functions.md)listesine bakın.
- [Şema yaşam döngüsü](lifecycle.md) hakkında bilgi edinin.
- [Şema sıralama düzenini](sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../how-to/update-existing-assignments.md) öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.