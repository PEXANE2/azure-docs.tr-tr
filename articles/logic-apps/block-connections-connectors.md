---
title: Belirli API bağlayıcıları için bağlantıları engelleme
description: Azure Logic Apps API bağlantılarını oluşturmayı ve kullanmayı kısıtla
services: logic-apps
ms.suite: integration
ms.reviewer: deli, logicappspm
ms.topic: conceptual
ms.date: 06/19/2020
ms.openlocfilehash: 6563f3e263867387332940db58abff62e085cded
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86187702"
---
# <a name="block-connections-created-by-connectors-in-azure-logic-apps"></a>Azure Logic Apps içinde bağlayıcılar tarafından oluşturulan bağlantıları engelle

Kuruluşunuz, Azure Logic Apps bağlayıcılarını kullanarak kısıtlı veya onaylanmamış kaynaklara bağlanmasına izin vermezse, bu bağlantıları mantıksal uygulama iş akışlarında oluşturma ve kullanma özelliğini engelleyebilirsiniz. [Azure ilkesiyle](../governance/policy/overview.md), engellemek istediğiniz bağlayıcılar için bağlantı oluşturulmasını veya kullanımını önleyen [ilkeler](../governance/policy/overview.md#policy-definition) tanımlayabilir ve uygulayabilirsiniz. Örneğin, güvenlik nedenleriyle, belirli sosyal medya platformları veya diğer hizmet ve sistemlerle bağlantıları engellemek isteyebilirsiniz.

Bu konuda, Azure portal kullanarak belirli bağlantıları engelleyen bir ilkenin nasıl ayarlanacağı gösterilmektedir, ancak örneğin Azure REST API, Azure PowerShell, Azure CLı ve Azure Resource Manager şablonları aracılığıyla başka yollarla ilke tanımları oluşturabilirsiniz. Daha fazla bilgi için bkz. [öğretici: uyumluluğu zorlamak için Ilke oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa, başlamadan önce [ücretsiz bir Azure hesabı oluşturun](https://azure.microsoft.com/free/) .

* Engellemek istediğiniz bağlayıcının başvuru Kımlığı. Daha fazla bilgi için bkz. [bağlayıcı başvuru kimliğini bulma](#connector-reference-ID).

<a name="connector-reference-ID"></a>

## <a name="find-connector-reference-id"></a>Bağlayıcı başvuru KIMLIĞINI bul

Engellemek istediğiniz bağlantıya sahip bir mantıksal uygulamanız zaten varsa [Azure Portal ilgili adımları](#connector-ID-portal)izleyin. Erişemiyorsanız şu adımları uygulayın:

1. [Logic Apps bağlayıcıları listesini](https://docs.microsoft.com/connectors/connector-reference/connector-reference-logicapps-connectors)ziyaret edin.

1. Engellemek istediğiniz bağlayıcının başvuru sayfasını bulun.

   Örneğin, Instagram bağlayıcısını engellemek istiyorsanız bu sayfaya gidin: 
   
   `https://docs.microsoft.com/connectors/instagram/`

1. Sayfanın URL 'sinden bağlayıcı başvuru KIMLIĞI ' ni, eğik çizgi () olmadan sonundaki şekilde kopyalayın ve kaydedin `/` `instagram` .

   Daha sonra, ilke tanımınızı oluşturduğunuzda, bu KIMLIĞI tanımın koşul ifadesinde kullanırsınız, örneğin:

   `"like": "*managedApis/instagram"`

<a name="connector-ID-portal"></a>

### <a name="azure-portal"></a>Azure portal

1. [Azure Portal](https://portal.azure.com), mantıksal uygulamanızı bulun ve açın.

1. Mantıksal uygulama menüsünde mantıksal uygulama **kod görünümü** ' nü seçerek MANTıKSAL uygulamanızın JSON tanımını görüntüleyebilirsiniz.

   ![Bağlayıcı KIMLIĞINI bulmak için "Logic App Code görünümünü" açın](./media/block-connections-connectors/code-view-connector-id.png)

1. `parameters` `$connections` `{connection-name}` Mantıksal uygulamanızdaki her bağlantı için bir nesne içeren nesnesini içeren nesnesini bulun ve bu bağlantıyla ilgili bilgileri belirtin:

   ```json
   {
      "parameters": {
         "$connections": {
            "value" : {
               "{connection-name}": {
                  "connectionId": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group-name}/providers/Microsoft.Web/connections/{connection-name}",
                  "connectionName": "{connection-name}",
                  "id": "/subscriptions/{Azure-subscription-ID}/providers/Microsoft.Web/locations/{Azure-region}/managedApis/{connection-name}"
               }
            }
         }
      }
   }
   ```

   Örneğin, bir Instagram Bağlayıcısı için, `instagram` bir Instagram bağlantısını tanımlayan nesnesini bulun:

   ```json
   {
      "parameters": {
         "$connections": {
            "value" : {
               "instagram": {
                  "connectionId": "/subscriptions/xxxxxXXXXXxxxxxXXXXXxxxxxXXXXX/resourceGroups/MyLogicApp-RG/providers/Microsoft.Web/connections/instagram",
                  "connectionName": "instagram",
                  "id": "/subscriptions/xxxxxXXXXXxxxxxXXXXXxxxxxXXXXX/providers/Microsoft.Web/locations/westus/managedApis/instagram"
               }
            }
         }
      }
   }
   ```

1. Engellemek istediğiniz bağlantı için, `id` Bu biçimi izleyen özelliği ve değeri bulun: 

   `"id": "/subscriptions/{Azure-subscription-ID}/providers/Microsoft.Web/locations/{Azure-region}/managedApis/{connection-name}"`

   Örneğin, `id` bir Instagram bağlantısının özelliği ve değeri aşağıda verilmiştir:

   `"id": "/subscriptions/xxxxxXXXXXxxxxxXXXXXxxxxxXXXXX/providers/Microsoft.Web/locations/westus/managedApis/instagram"`

1. `id`Özellik değeri ' nden bağlayıcı başvuru kimliği ' ni kopyalayın ve sonunda, örneğin, `instagram` .

   Daha sonra, ilke tanımınızı oluşturduğunuzda, bu KIMLIĞI tanımın koşul ifadesinde kullanırsınız, örneğin:

   `"like": "*managedApis/instagram"`

<a name="create-policy-connections"></a>

## <a name="create-policy-to-block-creating-connections"></a>Bağlantı oluşturmayı engellemek için ilke oluştur

Bir mantıksal uygulamada bağlantı oluşturmayı tamamen engellemek için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın. Portal arama kutusuna `policy` , girin ve **ilke**' yi seçin.

   ![Azure portal, "ilke" bulup seçin](./media/block-connections-connectors/find-select-azure-policy.png)

1. **İlke** menüsünde, **yazma**altında, **tanımlar**  >  **+ ilke tanımı**' nı seçin.

   !["Tanımlar" > "+ Ilke tanımı" nı seçin](./media/block-connections-connectors/add-new-policy-definition.png)

1. **İlke tanımı**altında, örnek altında açıklanan özelliklere göre ilke tanımınızın bilgilerini sağlayın:

   ![İlke tanımı özellikleri](./media/block-connections-connectors/policy-definition-create-connections-1.png)

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Tanım konumu** | Evet | <*Azure-abonelik-adı*> | İlke tanımı için kullanılacak Azure aboneliği <p><p>1. aboneliğinizi bulmak için üç nokta (**...**) düğmesini seçin. <br>2. **abonelik** listesinden aboneliğinizi bulun ve seçin. <br>3. işiniz bittiğinde **Seç**' i seçin. |
   | **Ad** | Evet | <*ilke-tanım-adı*> | İlke tanımı için kullanılacak ad |
   | **Açıklama** | Hayır | <*ilke-tanım-adı*> | İlke tanımı için bir açıklama |
   | **Kategori** | Evet | **Logic Apps** | İlke tanımı için mevcut bir kategorinin veya yeni kategorinin adı |
   | **İlke zorlama** | Evet | **Etkin** | Bu ayar, işinizi kaydettiğinizde ilke tanımının etkinleştirilip etkinleştirilmeyeceğini veya devre dışı bırakılacağını belirtir. |
   ||||

1. **Ilke kuralı**bölümünde JSON düzenleme kutusu bir ilke tanımı şablonuyla önceden doldurulur. Aşağıdaki tabloda açıklanan özelliklere ve bu söz dizimini izleyerek bu şablonu [ilke tanımınızda](../governance/policy/concepts/definition-structure.md) değiştirin:

   ```json
   {
      "mode": "All",
      "policyRule": {
         "if": {
            "field": "Microsoft.Web/connections/api.id",
            "like": "*managedApis/{connector-name}"
         },
         "then": {
            "effect": "deny"
         }
      },
      "parameters": {}
    }
    ```

   | Özellik | Değer | Açıklama |
   |----------|-------|-------------|
   | `mode` | `All` | İlkenin değerlendirdiği kaynak türlerini belirleyen mod. <p><p>Bu senaryo `mode` `All` , ilkeyi Azure Kaynak grupları, abonelikleri ve tüm kaynak türlerine uygulayan olarak ayarlar. <p><p>Daha fazla bilgi için bkz. [ilke tanımı yapısı-modu](../governance/policy/concepts/definition-structure.md#mode). |
   | `if` | `{condition-to-evaluate}` | İlke kuralının ne zaman zorlanacağını belirleyen koşul <p><p>Bu senaryoda, `{condition-to-evaluate}` `api.id` `Microsoft.Web/connections/api.id` `*managedApis/{connector-name}` bir joker karakter (*) değerini belirten, ' deki değerin eşleşip eşleşmediğini belirler. <p><p>Daha fazla bilgi için bkz. [ilke tanımı yapısı-ilke kuralı](../governance/policy/concepts/definition-structure.md#policy-rule). |
   | `field` | `Microsoft.Web/connections/api.id` | `field`Koşula göre karşılaştırılacak değer <p><p>Bu senaryoda,, `field` bağlayıcı özelliğindeki değere erişmek için,, [*diğer adını*](../governance/policy/concepts/definition-structure.md#aliases)kullanır `Microsoft.Web/connections/api.id` `api.id` . |
   | `like` | `*managedApis/{connector-name}` | Değeri karşılaştırmak için kullanılacak mantıksal işleç ve değer `field` <p><p>Bu senaryoda, `like` işleç ve joker karakter (*) karakterinin her ikisi de kuralın bölgeden bağımsız olarak çalıştığından emin olun ve dize, `*managedApis/{connector-name}` engellemek ISTEDIĞINIZ bağlayıcının kimliği olduğu yerde eşleşir `{connector-name}` . <p><p>Örneğin, sosyal medya platformları veya veritabanlarına bağlantı oluşturulmasını engellemek istediğinizi varsayalım: <p><p>Twitter`twitter` <br>Instagram`instagram` <br>'A`facebook` <br>Pinterest`pinterest` <br>-SQL Server veya Azure SQL:`sql` <p><p>Bu bağlayıcı kimliklerini bulmak için, bu konunun önceki kısımlarında yer alarak [bağlayıcı başvuru Kimliği bulma](#connector-reference-ID) bölümüne bakın. |
   | `then` | `{effect-to-apply}` | `if`Koşul karşılandığında uygulanacak efekt <p><p>Bu senaryoda, `{effect-to-apply}` ilke ile uyumlu olmayan bir isteği veya işlemi engellenemez ve başarısız olur. <p><p>Daha fazla bilgi için bkz. [ilke tanımı yapısı-ilke kuralı](../governance/policy/concepts/definition-structure.md#policy-rule). |
   | `effect` | `deny` | , `effect` Belirtilen bağlantıyı oluşturmak için gereken isteği engellenemez <p><p>Daha fazla bilgi için bkz. [Azure ilke efektlerini anlama-reddetme](../governance/policy/concepts/effects.md#deny). |
   ||||

   Örneğin, Instagram bağlayıcısıyla bağlantı oluşturmayı engellemek istediğinizi varsayalım. Kullanabileceğiniz ilke tanımı aşağıda verilmiştir:

   ```json
   {
      "mode": "All",
      "policyRule": {
         "if": {
            "field": "Microsoft.Web/connections/api.id",
            "like": "*managedApis/instagram"
         },
         "then": {
            "effect": "deny"
         }
      },
      "parameters": {}
   }
   ```

   **Ilke kuralı** kutusunun görünme şekli aşağıda verilmiştir:

   ![İlke tanımı için kural](./media/block-connections-connectors/policy-definition-create-connections-2.png)

   Birden çok bağlayıcı için daha fazla koşul ekleyebilirsiniz, örneğin:

   ```json
   {
      "mode": "All",
      "policyRule": {
         "if": {
            "anyOf": [
               {
                  "field": "Microsoft.Web/connections/api.id",
                  "like": "*managedApis/instagram"
               },
               {
                  "field": "Microsoft.Web/connections/api.id",
                  "like": "*managedApis/twitter"
               },
               {
                  "field": "Microsoft.Web/connections/api.id",
                  "like": "*managedApis/facebook"
               },
               {
                  "field": "Microsoft.Web/connections/api.id",
                  "like": "*managedApis/pinterest"
               }
            ]
         },
         "then": {
            "effect": "deny"
         }
      },
      "parameters": {}
    }
    ```

1. İşiniz bittiğinde **Kaydet**'i seçin. İlke tanımını kaydettikten sonra Azure Ilkesi, ilke tanımına daha fazla özellik değeri oluşturur ve ekler.

1. Sonra, ilkeyi zorlamak istediğiniz ilke tanımını atamak için [bir ilke ataması oluşturun](#create-policy-assignment).

Azure ilke tanımları hakkında daha fazla bilgi için şu konulara bakın:

* [İlke yapısı tanımı](../governance/policy/concepts/definition-structure.md)
* [Öğretici: uyumluluğu zorlamak için ilke oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md)
* [Azure Logic Apps için Azure Ilkesi yerleşik ilke tanımları](../logic-apps/policy-samples.md)

<a name="create-policy-connector-usage"></a>

## <a name="create-policy-to-block-using-connections"></a>Bağlantıları kullanarak engellenecek ilke oluşturma

Bir mantıksal uygulama içinde bir bağlantı oluşturduğunuzda, bu bağlantı ayrı bir Azure kaynağı olarak bulunur. Yalnızca mantıksal uygulamayı silerseniz, bağlantı otomatik olarak silinmez ve silinene kadar mevcut olmaya devam eder. Bağlantının zaten varolduğu veya bir mantıksal uygulama dışında kullanım için bağlantı oluşturmanız gereken bir senaryoya sahip olabilirsiniz. Kısıtlı veya onaylanmamış bağlantısı olan mantıksal uygulamaların kaydedilmesini önleyen bir ilke oluşturarak bir mantıksal uygulamada mevcut bir bağlantıyı kullanma özelliğini hala engelleyebilirsiniz.

1. [Azure Portal](https://portal.azure.com) oturum açın. Portal arama kutusuna `policy` , girin ve **ilke**' yi seçin.

   ![Azure portal, "ilke" bulup seçin](./media/block-connections-connectors/find-select-azure-policy.png)

1. **İlke** menüsünde, **yazma**altında, **tanımlar**  >  **+ ilke tanımı**' nı seçin.

   !["Tanımlar" > "+ Ilke tanımı" nı seçin](./media/block-connections-connectors/add-new-policy-definition.png)

1. **İlke tanımı**' nın altında, örnek altında açıklanan özelliklere göre ilke tanımınızın bilgilerini sağlayın

   ![İlke tanımı özellikleri](./media/block-connections-connectors/policy-definition-using-connections-1.png)

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Tanım konumu** | Evet | <*Azure-abonelik-adı*> | İlke tanımı için kullanılacak Azure aboneliği <p><p>1. aboneliğinizi bulmak için üç nokta (**...**) düğmesini seçin. <br>2. **abonelik** listesinden aboneliğinizi bulun ve seçin. <br>3. işiniz bittiğinde **Seç**' i seçin. |
   | **Ad** | Evet | <*ilke-tanım-adı*> | İlke tanımı için kullanılacak ad |
   | **Açıklama** | Hayır | <*ilke-tanım-adı*> | İlke tanımı için bir açıklama |
   | **Kategori** | Evet | **Logic Apps** | İlke tanımı için mevcut bir kategorinin veya yeni kategorinin adı |
   | **İlke zorlama** | Evet | **Etkin** | Bu ayar, işinizi kaydettiğinizde ilke tanımının etkinleştirilip etkinleştirilmeyeceğini veya devre dışı bırakılacağını belirtir. |
   ||||

1. **Ilke kuralı**bölümünde JSON düzenleme kutusu bir ilke tanımı şablonuyla önceden doldurulur. Aşağıdaki tabloda açıklanan özelliklere ve bu söz dizimini izleyerek bu şablonu [ilke tanımınızda](../governance/policy/concepts/definition-structure.md) değiştirin:

   ```json
   {
      "mode": "All",
      "policyRule": {
         "if": {
            "value": "[string(field('Microsoft.Logic/workflows/parameters'))]",
            "contains": "{connector-name}"
         },
         "then": {
            "effect": "deny"
         }
      },
      "parameters": {}
    }
    ```

   | Özellik | Değer | Açıklama |
   |----------|-------|-------------|
   | `mode` | `All` | İlkenin değerlendirdiği kaynak türlerini belirleyen mod. <p><p>Bu senaryo `mode` `All` , ilkeyi Azure Kaynak grupları, abonelikleri ve tüm kaynak türlerine uygulayan olarak ayarlar. <p><p>Daha fazla bilgi için bkz. [ilke tanımı yapısı-modu](../governance/policy/concepts/definition-structure.md#mode). |
   | `if` | `{condition-to-evaluate}` | İlke kuralının ne zaman zorlanacağını belirleyen koşul <p><p>Bu senaryoda, `{condition-to-evaluate}` dize çıkışının öğesinden, dizesini içerip içermediğini belirler `[string(field('Microsoft.Logic/workflows/parameters'))]` `{connector-name}` . <p><p>Daha fazla bilgi için bkz. [ilke tanımı yapısı-ilke kuralı](../governance/policy/concepts/definition-structure.md#policy-rule). |
   | `value` | `[string(field('Microsoft.Logic/workflows/parameters'))]` | Koşula göre karşılaştırılacak değer <p><p>Bu senaryoda, nesnesi `value` `[string(field('Microsoft.Logic/workflows/parameters'))]` `$connectors` içindeki nesnesini bir dizeye dönüştüren dize çıkışı `Microsoft.Logic/workflows/parameters` . |
   | `contains` | `{connector-name}` | Özelliği ile karşılaştırmak için kullanılacak mantıksal işleç ve değer `value` <p><p>Bu senaryoda `contains` operatör, dizenin nerede göründüğüne bakılmaksızın, `{connector-name}` dizenin nerede göründüğünü, `{connector-name}` kısıtlamak veya engellemek istediğiniz bağlayıcının kimliği olduğunu dikkate alır. <p><p>Örneğin, sosyal medya platformları veya veritabanlarına yönelik bağlantıları kullanmayı engellemek istediğinizi varsayalım: <p><p>Twitter`twitter` <br>Instagram`instagram` <br>'A`facebook` <br>Pinterest`pinterest` <br>-SQL Server veya Azure SQL:`sql` <p><p>Bu bağlayıcı kimliklerini bulmak için, bu konunun önceki kısımlarında yer alarak [bağlayıcı başvuru Kimliği bulma](#connector-reference-ID) bölümüne bakın. |
   | `then` | `{effect-to-apply}` | `if`Koşul karşılandığında uygulanacak efekt <p><p>Bu senaryoda, `{effect-to-apply}` ilke ile uyumlu olmayan bir istek ya da işlemi engellenemez ve başarısız olur. <p><p>Daha fazla bilgi için bkz. [ilke tanımı yapısı-ilke kuralı](../governance/policy/concepts/definition-structure.md#policy-rule). |
   | `effect` | `deny` | , `effect` `deny` Belirtilen bağlantıyı kullanan bir mantıksal uygulamayı kaydetme isteğini engelliyor veya engelleyecek <p><p>Daha fazla bilgi için bkz. [Azure ilke efektlerini anlama-reddetme](../governance/policy/concepts/effects.md#deny). |
   ||||

   Örneğin, Instagram bağlantıları kullanan mantıksal uygulamaların kaydedilmesini engellemek istediğinizi varsayalım. Kullanabileceğiniz ilke tanımı aşağıda verilmiştir:

   ```json
   {
      "mode": "All",
      "policyRule": {
         "if": {
            "value": "[string(field('Microsoft.Logic/workflows/parameters'))]",
            "contains": "instagram"
         },
         "then": {
            "effect": "deny"
         }
      },
      "parameters": {}
    }
    ```

   **Ilke kuralı** kutusunun görünme şekli aşağıda verilmiştir:

   ![İlke tanımı için kural](./media/block-connections-connectors/policy-definition-using-connections-2.png)

1. İşiniz bittiğinde **Kaydet**'i seçin. İlke tanımını kaydettikten sonra Azure Ilkesi, ilke tanımına daha fazla özellik değeri oluşturur ve ekler.

1. Sonra, ilkeyi zorlamak istediğiniz ilke tanımını atamak için [bir ilke ataması oluşturun](#create-policy-assignment).

Azure ilke tanımları hakkında daha fazla bilgi için şu konulara bakın:

* [İlke yapısı tanımı](../governance/policy/concepts/definition-structure.md)
* [Öğretici: uyumluluğu zorlamak için ilke oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md)
* [Azure Logic Apps için Azure Ilkesi yerleşik ilke tanımları](../logic-apps/policy-samples.md)

<a name="create-policy-assignment"></a>

## <a name="create-policy-assignment"></a>İlke ataması oluştur

Ardından, ilkeyi zorlamak istediğiniz ilke tanımını atamanız gerekir, örneğin, tek bir kaynak grubuna, birden çok kaynak grubuna, Azure Active Directory (Azure AD) kiracıya veya Azure aboneliğine. Bu görev için, bir ilke ataması oluşturmak için aşağıdaki adımları izleyin:

1. Oturumunuzu kapatırsanız [Azure Portal](https://portal.azure.com)tekrar oturum açın. Portal arama kutusuna `policy` , girin ve **ilke**' yi seçin.

   ![Azure portal, "Ilke" bulup seçin](./media/block-connections-connectors/find-select-azure-policy.png)

1. **İlke** menüsünde, **yazma**altında, **atamalar**  >  **ilke ata**' yı seçin.

   !["Atama" > "atamalar" ı seçin](./media/block-connections-connectors/add-new-policy-assignment.png)

1. **Temel bilgiler**bölümünde, ilke ataması için şu bilgileri sağlayın:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Kapsam** | Evet | İlke atamasını zorlamak istediğiniz kaynaklar. <p><p>1. **kapsam** kutusunun yanındaki üç nokta (**...**) düğmesini seçin. <br>2. **abonelik** listesinden Azure aboneliğini seçin. <br>3. isteğe bağlı olarak, **kaynak grubu** listesinden kaynak grubunu seçin. <br>4. işiniz bittiğinde **Seç**' i seçin. |
   | **Dış** | Hayır | İlke atamasından çıkarılacak tüm Azure kaynakları. <p><p>1. **Dışlamalar** kutusunun yanındaki üç nokta (**...**) düğmesini seçin. <br>2. **kaynak** listesinden, **seçili kapsama eklemek**> kaynak seçin. <br>3. işiniz bittiğinde **Kaydet**' i seçin. |
   | **İlke tanımı** | Evet | Atamak ve zorlamak istediğiniz ilke tanımının adı. Bu örnek, "Instagram bağlantılarını engelle" adlı örnek bir "%. <p><p>1. **ilke tanımı** kutusunun yanındaki üç nokta (**...**) düğmesini seçin. <br>2. **tür** filtresi veya **arama** kutusunu kullanarak ilke tanımını bulun ve seçin. <br>3. işiniz bittiğinde **Seç**' i seçin. |
   | **Atama adı** | Evet | İlke tanımından farklıysa, ilke ataması için kullanılacak ad |
   | **Atama KIMLIĞI** | Evet | İlke ataması için otomatik olarak oluşturulan KIMLIK |
   | **Açıklama** | Hayır | İlke ataması için bir açıklama |
   | **İlke zorlama** | Evet | İlke atamasını sağlayan veya devre dışı bırakan ayar |
   | **Atayan** | Hayır | İlke atamasını oluşturan ve uygulamış olan kişinin adı |
   ||||

   Örneğin, bir Azure Kaynak grubuna bir ilkeyi bir Azure Kaynak grubuna atamak için şu örneği kullanın:

   ![İlke atama özellikleri](./media/block-connections-connectors/policy-assignment-basics.png)

1. İşiniz bittiğinde, **gözden geçir + oluştur**' u seçin.

   Bir ilke oluşturduktan sonra, ilkenin yürürlüğe geçmesi için 15 dakika kadar beklemeniz gerekebilir. Değişikliklerin de benzer gecikmeli etkileri olabilir.

1. İlke etkinleştiğinden, [ilkenizi test](#test-policy)edebilirsiniz.

Daha fazla bilgi için bkz. [hızlı başlangıç: uyumlu olmayan kaynakları belirlemek için bir ilke ataması oluşturma](../governance/policy/assign-policy-portal.md).

<a name="test-policy"></a>

## <a name="test-the-policy"></a>İlkeyi test etme

İlkenizi denemek için, mantıksal uygulama tasarımcısında şimdi kısıtlanmış bağlayıcıyı kullanarak bir bağlantı oluşturmaya başlayın. Bir Instagram örneği ile devam ettiğinizde, bu hatayı, mantıksal uygulamanızın bağlantıyı oluşturmamasına ilişkin başarısız olduğunu alırsınız:

![Uygulanan ilke nedeniyle bağlantı hatası](./media/block-connections-connectors/connection-failure-message.png)

İleti şu bilgileri içerir:

| Açıklama | İçerik |
|---|---|
| Hatanın nedeni | `"Resource 'instagram' was disallowed by policy."` |
| Atama adı | `"Block Instagram connections"` |
| Atama KIMLIĞI | `"/subscriptions/xxxxxXXXXXxxxxxXXXXXxxxxxXXXXX/resourceGroups/MyLogicApp-RG/providers/Microsoft.Authorization/policyAssignments/4231890fc3bd4352acb0b673"` |
| İlke tanımı KIMLIĞI | `"/subscriptions/xxxxxXXXXXxxxxxXXXXXxxxxxXXXXX/providers/Microsoft.Authorization/policyDefinitions/b5ddcfec-1b24-4cac-a353-360846a59f24"` |
|||

## <a name="next-steps"></a>Sonraki adımlar

* [Azure ilkesi](../governance/policy/overview.md) hakkında daha fazla bilgi edinin