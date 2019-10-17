---
title: Güvenli erişim ve veri Azure Logic Apps
description: Azure Logic Apps ' de parametre girişlerini, HTTP isteği tetikleyicilerini, çalıştırma geçmişini, mantıksal uygulama işlemlerini ve diğer hizmetlere bağlantıları koruyun
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 06/28/2019
ms.openlocfilehash: c7b6abb9874c8b6bb2469595b9ce53541656834f
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72430135"
---
# <a name="secure-access-and-data-in-azure-logic-apps"></a>Azure Logic Apps 'da güvenli erişim ve veriler

Azure Logic Apps erişimi denetlemek ve verileri korumak için bu alanlarda güvenliği ayarlayabilirsiniz:

* [HTTP istek tetikleyicilerine erişim](#secure-triggers)
* [Mantıksal uygulama işlemlerine erişim](#secure-operations)
* [Çalışma geçmişi girdilerine ve çıkışlarına erişim](#secure-run-history)
* [Parametre girdilerine erişim](#secure-action-parameters)
* [Logic Apps 'ten çağrılan hizmetlere ve sistemlere erişim](#secure-requests)

<a name="secure-triggers"></a>

## <a name="access-to-http-request-triggers"></a>HTTP istek tetikleyicilerine erişim

Mantıksal uygulamanız [istek](../connectors/connectors-native-reqres.md) veya [Web KANCASı](../connectors/connectors-native-webhook.md) tetikleyicisi gibi http istek tabanlı bir tetikleyici kullandığında, erişimi yalnızca yetkili istemcilerin başlatabilmesi için sınırlayabilirsiniz. Bir mantıksal uygulama tarafından alınan tüm istekler Güvenli Yuva Katmanı (SSL) protokolü ile şifrelenir ve güvenli hale getirilir. 

Bu tetikleyici türüne erişimi güvenli hale getirmek için kullanabileceğiniz yollar şunlardır:

* [Paylaşılan erişim imzaları oluştur](#sas)
* [Gelen IP adreslerini kısıtla](#restrict-incoming-ip-addresses)
* [Azure Active Directory, OAuth veya diğer güvenlik ekleme](#add-authentication)

<a name="sas"></a>

### <a name="generate-shared-access-signatures-sas"></a>Paylaşılan erişim imzaları oluşturma (SAS)

Bir mantıksal uygulamadaki her istek uç noktası, bitiş noktasının URL 'sinde, bu biçimi izleyen bir [paylaşılan erişim imzasına (SAS)](https://docs.microsoft.com/rest/api/storageservices/constructing-a-service-sas) sahiptir:

`https://<request-endpoint-URI>sp=<permissions>sv=<SAS-version>sig=<signature>`

Her URL, bu tabloda açıklandığı gibi bir `sp`, `sv` ve `sig` sorgu parametresi içerir:

| Sorgu parametresi | Açıklama |
|-----------------|-------------|
| `sp` | İzin verilen HTTP yöntemlerinin kullanması için izinleri belirtir. |
| `sv` | İmzayı oluşturmak için kullanılacak SAS sürümünü belirtir. |
| `sig` | Tetikleyiciye erişim kimliğini doğrulamak için kullanılacak imzayı belirtir. Bu imza, tüm URL yollarında ve özelliklerde gizli bir erişim anahtarı ile SHA256 algoritması kullanılarak oluşturulur. Hiçbir koşulda veya yayımlanmadıysa, bu anahtar şifrelenir ve mantıksal uygulamayla birlikte depolanır. Mantıksal uygulamanız yalnızca gizli anahtar ile oluşturulmuş geçerli bir imza içeren Tetikleyicileri yetkilendirir. |
|||

Paylaşılan erişim Imzasıyla erişimi güvenli hale getirme hakkında daha fazla bilgi için bu konudaki şu bölümlere bakın:

* [Erişim anahtarlarını yeniden oluştur](#access-keys)
* [Süresi dolan geri arama URL 'Leri oluşturma](#expiring-urls)
* [Birincil veya ikincil anahtarla URL oluşturma](#primary-secondary-key)

<a name="access-keys"></a>

#### <a name="regenerate-access-keys"></a>Erişim anahtarlarını yeniden oluştur

Dilediğiniz zaman yeni bir güvenli erişim anahtarı oluşturmak için Azure REST API veya Azure portal kullanın. Eski anahtarı kullanan önceden oluşturulmuş tüm URL 'Ler geçersiz kılınır ve mantıksal uygulamayı tetiklemek için artık yetkilendirmeye sahip olmaz. Yeniden oluşturma işleminden sonra aldığınız URL 'Ler yeni erişim anahtarıyla imzalanır.

1. Azure portal, yeniden oluşturmak istediğiniz anahtarı içeren mantıksal uygulamayı açın.

1. Mantıksal uygulamanın menüsünde, **Ayarlar**altında **erişim anahtarları**' nı seçin.

1. Yeniden oluşturmak istediğiniz anahtarı seçin ve işlemi sona erdirin.

<a name="expiring-urls"></a>

#### <a name="create-expiring-callback-urls"></a>Süresi dolan geri arama URL 'Leri oluşturma

Diğer kişilerle HTTP istek tabanlı bir tetikleyicinin uç nokta URL 'sini paylaşırsanız, belirli anahtarları kullanan ve sona erme tarihleri olan geri çağırma URL 'Leri oluşturabilirsiniz. Bu şekilde, belirli bir TimeSpan temelinde, mantıksal uygulamanızı tetikleyerek anahtarları sorunsuzca alabilir veya erişimi kısıtlayabilirsiniz. Bir URL için sona erme tarihi belirtmek için [Logic Apps REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers)kullanın, örneğin:

``` http
POST /subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<workflow-name>/triggers/<trigger-name>/listCallbackUrl?api-version=2016-06-01
```

Gövdesinde, JSON tarih dizesi kullanarak `NotAfter`özelliğini ekleyin. Bu özellik yalnızca `NotAfter` Tarih ve saatine kadar geçerli olan bir geri çağırma URL 'SI döndürür.

<a name="primary-secondary-key"></a>

#### <a name="create-urls-with-primary-or-secondary-secret-key"></a>Birincil veya ikincil gizli anahtar ile URL oluşturma

HTTP istek tabanlı tetikleyiciler için geri çağırma URL 'Leri oluştururken veya listeuyorsanız, URL 'YI imzalamak için kullanılacak anahtarı belirtebilirsiniz. Belirli bir anahtarla imzalanmış bir URL oluşturmak için [Logic Apps REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers)kullanın, örneğin:

``` http
POST /subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<workflow-name>/triggers/<trigger-name>/listCallbackUrl?api-version=2016-06-01
```

Gövdede `KeyType` özelliğini `Primary` veya `Secondary` olarak ekleyin. Bu özellik, belirtilen güvenli anahtar tarafından imzalanmış bir URL döndürür.

<a name="restrict-incoming-ip"></a>

### <a name="restrict-incoming-ip-addresses"></a>Gelen IP adreslerini kısıtla

Paylaşılan erişim Imzasıyla birlikte, mantıksal uygulamanızı çağırabilecek belirli istemcileri kısıtlamak isteyebilirsiniz. Örneğin, Azure API Management kullanarak istek uç noktanızı yönetiyorsanız, mantıksal uygulamanızı yalnızca API Management örneği için IP adresinden istekleri kabul edecek şekilde kısıtlayabilirsiniz.

#### <a name="restrict-incoming-ip-ranges-in-azure-portal"></a>Azure portal gelen IP aralıklarını kısıtla

1. Azure portal mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

1. Mantıksal uygulamanızın menüsünde, **Ayarlar**' ın altında, **iş akışı ayarları**' nı seçin.

1. **Erişim denetimi yapılandırması** > **ızın verilen gelen IP adresleri**altında, **belirli IP aralıklarını**seçin.

1. **Tetikleyiciler Için IP aralıkları**altında, tetikleyicinin kabul ettiği IP adresi aralıklarını belirtin.

   Geçerli bir IP aralığı şu biçimleri kullanır: *x. x. x. x/x* veya *x. x. x. x-x. x. x.* x

Mantıksal uygulamanızın yalnızca iç içe geçmiş bir mantıksal uygulama olarak tetikleyeceğini istiyorsanız, **Izin verilen gelen IP adresleri** listesinden **yalnızca diğer Logic Apps**' ı seçin. Bu seçenek, mantıksal uygulama kaynağınızın boş bir dizisini yazar. Bu şekilde, yalnızca Logic Apps hizmetinden (üst mantıksal uygulamalar) yapılan çağrılar iç içe geçmiş mantıksal uygulamayı tetikleyebilir.

> [!NOTE]
> IP adresi ne olursa olsun, Azure REST API veya API Management aracılığıyla `/triggers/<trigger-name>/run` kullanarak HTTP istek tabanlı tetikleyicisine sahip bir mantıksal uygulamayı çalıştırmaya devam edebilirsiniz. Ancak, bu senaryo Azure REST API için de kimlik doğrulaması gerektirir. Tüm olaylar Azure denetim günlüğünde görüntülenir. Erişim denetim ilkelerini uygun şekilde ayarladığınızdan emin olun.

#### <a name="restrict-incoming-ip-ranges-in-azure-resource-manager-template"></a>Azure Resource Manager şablonda gelen IP aralıklarını kısıtla

Mantıksal uygulama dağıtımlarını bir [Azure Resource Manager şablonu](../logic-apps/logic-apps-create-deploy-template.md)kullanarak otomatikleştirin, mantıksal uygulamanızın kaynak tanımındaki `triggers` bölümü ile `accessControl` bölümünü kullanarak IP aralıklarını belirtebilirsiniz, örneğin:

``` json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {},
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {<workflow-definition>},
            "parameters": {},
            "accessControl": {
               "triggers": {
                  "allowedCallerIpAddresses": [
                     {
                        "addressRange": "192.168.12.0/23"
                     },
                     {
                        "addressRange": "2001:0db8::/64"
                     }
                  ]
               }
            }
         }
      }
   ],
   "outputs": {}
}
```

<a name="add-authentication"></a>

### <a name="add-azure-active-directory-oauth-or-other-security"></a>Azure Active Directory, OAuth veya diğer güvenlik ekleme

Mantıksal uygulamanıza daha fazla Yetkilendirme Protokolü eklemek için [Azure API Management](../api-management/api-management-key-concepts.md)kullanmayı göz önünde bulundurun. Bu hizmet, mantıksal uygulamanızı bir API olarak kullanıma sunma yeteneği sağlar ve herhangi bir uç nokta için zengin izleme, güvenlik, ilke ve belge sağlar. API Management mantıksal uygulamanız için ortak veya özel bir uç nokta sunabilir, bu da Azure Active Directory, OAuth, sertifika veya diğer güvenlik standartlarını kullanabilir. API Management bir istek aldığında, hizmet, isteği mantıksal uygulamanıza gönderir ve aynı zamanda gerekli dönüştürmeleri veya kısıtlamaları da yapar. Mantıksal uygulamanızı yalnızca API Management tetikleyebilmeniz için mantıksal uygulamanızın gelen IP aralığı ayarlarını kullanabilirsiniz.

<a name="secure-operations"></a>

## <a name="access-to-logic-app-operations"></a>Mantıksal uygulama işlemlerine erişim

Mantıksal uygulamaları yönetme, düzenleme ve görüntüleme gibi belirli işlemleri yalnızca belirli kullanıcıların veya grupların çalıştırmasına izin verebilirsiniz. İzinlerini denetlemek için, Azure aboneliğinizdeki üyelere özelleştirilmiş veya yerleşik roller atamak üzere [Azure rol tabanlı Access Control (RBAC)](../role-based-access-control/role-assignments-portal.md) kullanın:

* [Mantıksal uygulama katılımcısı](../role-based-access-control/built-in-roles.md#logic-app-contributor): mantıksal uygulamaları yönetmenizi sağlar, ancak bunlara erişimi değiştiremezsiniz.

* [Logic App operatörü](../role-based-access-control/built-in-roles.md#logic-app-operator): Logic Apps 'i okumanızı, etkinleştirmenizi ve devre dışı bırakmanızı sağlar, ancak bunları düzenleyemez veya güncelleştiremezsiniz.

Başkalarının mantıksal uygulamanızı değiştirmesini veya silmesini engellemek için, [Azure Kaynak kilidi](../azure-resource-manager/resource-group-lock-resources.md)'ni kullanarak başkalarının üretim kaynaklarını değiştirmesini veya silmesini önler.

<a name="secure-run-history"></a>

## <a name="access-to-run-history-data"></a>Çalışma geçmişi verilerine erişim

Mantıksal uygulama çalıştırma sırasında, Aktarım Katmanı Güvenliği (TLS) ve [bekleyen](../security/fundamentals/encryption-atrest.md)' i kullanarak tüm veriler [geçiş sırasında şifrelenir](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit) . Mantıksal uygulamanız çalışmayı bitirdiğinde, her eylem için durum, süre, girişler ve çıkışlarla birlikte çalışan adımlar dahil olmak üzere o çalıştırmaya ilişkin geçmişi görüntüleyebilirsiniz. Bu zengin ayrıntı, mantıksal uygulamanızın nasıl çalıştığı ve ortaya çıkan tüm sorunları gidermeye başlayabileceğiniz hakkında öngörüler sağlar.

Mantıksal uygulamanızın çalıştırma geçmişine eriştiğinizde Logic Apps erişiminizin kimliğini doğrular ve mantıksal uygulamanızın çalıştırmasında isteklerinizin ve yanıtların giriş ve çıkışları için bağlantılar sağlar. Ancak, herhangi bir parolayı, parolayı, anahtarı veya diğer hassas bilgileri işleyen eylemler için başkalarının bu verilere bakmasını ve erişimini engellemek isteyebilirsiniz. Örneğin, mantıksal uygulamanız bir HTTP eyleminin kimlik doğrulaması sırasında kullanmak üzere [Azure Key Vault](../key-vault/key-vault-overview.md) bir gizli dizi alırsa, bu gizli anahtarı görünümden gizlemek istersiniz.

Mantıksal uygulamanızın çalıştırma geçmişinde giriş ve çıkışlara erişimi denetlemek için şu seçeneklere sahipsiniz:

* [IP adresi aralığına göre erişimi kısıtlayın](#restrict-ip).

  Bu seçenek, belirli bir IP adres aralığından gelen isteklere göre çalışma geçmişine erişimi güvenli hale getirmeye olanak sağlar.

* Gizleme [kullanarak çalıştırma geçmişinden verileri gizleyin](#obfuscate).

  Birçok tetikleyici ve eylem içinde, girişlerini, çıkışları veya her ikisini de bir mantıksal uygulamanın çalıştırma geçmişinden gizleyebilirsiniz.

<a name="restrict-ip"></a>

### <a name="restrict-access-by-ip-address-range"></a>IP adresi aralığına göre erişimi kısıtla

Yalnızca belirli IP adresi aralıklarından gelen isteklerin bu verileri görüntülemesi için mantıksal uygulamanızın çalıştırma geçmişinde giriş ve çıkışlara erişimi sınırlayabilirsiniz. Örneğin, herhangi bir kişinin girişlere ve çıkışlara erişmesini engellemek için `0.0.0.0-0.0.0.0` gibi bir IP adresi aralığı belirtin. Yalnızca yönetici izinlerine sahip bir kişi, mantıksal uygulamanızın verilerine "tam zamanında" erişim olanağı sunan bu kısıtlamayı kaldırabilir. Azure portal veya mantıksal uygulama dağıtımı için kullandığınız bir Azure Resource Manager şablonunda kısıtlamak için IP aralıklarını belirtebilirsiniz.

#### <a name="restrict-ip-ranges-in-azure-portal"></a>Azure portal IP aralıklarını kısıtla

1. Azure portal mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

1. Mantıksal uygulamanızın menüsünde, **Ayarlar**' ın altında, **iş akışı ayarları**' nı seçin.

1. **Erişim denetimi yapılandırması** > **ızın verilen gelen IP adresleri**altında, **belirli IP aralıklarını**seçin.

1. İçerik **Için IP aralıkları**altında girişler ve çıkışlardan IÇERIĞE erişebilen IP adresi aralıklarını belirtin. 

   Geçerli bir IP aralığı şu biçimleri kullanır: *x. x. x. x/x* veya *x. x. x. x-x. x. x.* x

#### <a name="restrict-ip-ranges-in-azure-resource-manager-template"></a>Azure Resource Manager şablonundaki IP aralıklarını kısıtlama

Mantıksal uygulama dağıtımlarını bir [Azure Resource Manager şablonu](../logic-apps/logic-apps-create-deploy-template.md)kullanarak otomatikleştirin, mantıksal uygulamanızın kaynak tanımındaki `contents` bölümü ile `accessControl` bölümünü kullanarak IP aralıklarını belirtebilirsiniz, örneğin:

``` json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {},
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {<workflow-definition>},
            "parameters": {},
            "accessControl": {
               "contents": {
                  "allowedCallerIpAddresses": [
                     {
                        "addressRange": "192.168.12.0/23"
                     },
                     {
                        "addressRange": "2001:0db8::/64"
                     }
                  ]
               }
            }
         }
      }
   ],
   "outputs": {}
}
```

<a name="obfuscate"></a>

### <a name="hide-data-from-run-history-by-using-obfuscation"></a>Gizleme kullanarak çalıştırma geçmişinden verileri gizleme

Birçok tetikleyici ve eylemin, girdileri, çıkışları veya her ikisini de mantıksal uygulamanın çalıştırma geçmişinden gizlemek için ayarları vardır. Bu verileri güvenli hale getirmek için bu ayarları kullandığınızda [göz önünde bulundurmanız gereken bazı noktalar](#obfuscation-considerations) aşağıda verilmiştir.

#### <a name="secure-inputs-and-outputs-in-the-designer"></a>Tasarımcıda güvenli girişler ve çıktılar

1. Mantıksal uygulamanız [Azure Portal](https://portal.azure.com)zaten açık değilse, mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

   ![Örnek mantıksal uygulamayı aç](media/logic-apps-securing-a-logic-app/sample-logic-app.png)

1. Verilerin güvenliğini sağlamak istediğiniz tetikleyici veya eylemde, üç nokta ( **...** ) düğmesini ve ardından **Ayarlar**' ı seçin.

   !["Ayarlar" öğesini aç](media/logic-apps-securing-a-logic-app/open-settings.png)

1. **Güvenli girişleri**, **güvenli çıktıları**veya her ikisini açın. İşiniz bittiğinde **Bitti**'yi seçin.

   ![Güvenli girişleri veya çıkışları açma](media/logic-apps-securing-a-logic-app/turn-on-secure-inputs-outputs.png)

   Eylem veya tetikleyici artık başlık çubuğunda bir kilit simgesi gösterir.

   ![Başlık çubuğunda kilit simgesi](media/logic-apps-securing-a-logic-app/title-bar-lock-icon.png)

   Önceki eylemlerden gelen güvenli çıkışları temsil eden belirteçler de kilit simgeleri gösterir. Örneğin, bir eylemde kullanmak üzere dinamik içerik listesinden böyle bir çıktı seçtiğinizde, bu belirteç bir kilit simgesi gösterir.

   ![Çıkış seçin](media/logic-apps-securing-a-logic-app/select-secured-token.png)

1. Mantıksal uygulama çalıştıktan sonra, o çalıştırmanın geçmişini görüntüleyebilirsiniz.

   1. Mantıksal uygulamanın **genel bakış** bölmesinde, görüntülemek istediğiniz çalıştırmayı seçin.

   1. **Mantıksal uygulama çalıştırma** bölmesinde, gözden geçirmek istediğiniz eylemleri genişletin.

      Hem giriş hem de çıkışları güvenli hale getirmeyi seçerseniz, bu değerler artık gizli olarak görünür.

      ![Çalışma geçmişinde gizli veriler](media/logic-apps-securing-a-logic-app/hidden-data-run-history.png)

<a name="secure-data-code-view"></a>

#### <a name="secure-inputs-and-outputs-in-code-view"></a>Kod görünümünde güvenli girişler ve çıktılar

Temel tetikleyici veya eylem tanımında, `runtimeConfiguration.secureData.properties` diziyi bu değerlerden birini veya her ikisini de ekleyin veya güncelleştirin:

* `"inputs"`: çalıştırma geçmişindeki girişlerin güvenliğini sağlar.
* `"outputs"`: çalışma geçmişinde çıkışları güvenli bir şekilde korur.

Bu verileri güvenli hale getirmek için bu ayarları kullandığınızda [göz önünde bulundurmanız gereken bazı noktalar](#obfuscation-considerations) aşağıda verilmiştir.

```json
"<trigger-or-action-name>": {
   "type": "<trigger-or-action-type>",
   "inputs": {
      <trigger-or-action-inputs>
   },
   "runtimeConfiguration": {
      "secureData": {
         "properties": [
            "inputs",
            "outputs"
         ]
      }
   },
   <other-attributes>
}
```

<a name="obfuscation-considerations"></a>

#### <a name="considerations-when-hiding-inputs-and-outputs"></a>Girişleri ve çıkışları gizleme konuları

* Bir tetikleyici veya eylemde girişlerin veya çıkışların güvenliğini uyguladığınızda Logic Apps, güvenli verileri Azure Log Analytics 'a göndermez. Ayrıca, izleme için bu tetikleyiciye veya eyleme [izlenen Özellikler](logic-apps-monitor-your-logic-apps.md#azure-diagnostics-event-settings-and-details) ekleyemezsiniz.

* [İş akışı geçmişini işlemek için Logic Apps API 'si](https://docs.microsoft.com/rest/api/logic/) güvenli çıktılar döndürmez.

* Girişlerin güvenliğini sağlayan veya güvenli çıkışları açık olarak kullanan bir eylemde ve bu eylem, bu güvenli verileri içeren çıkışlarla bir yanıt döndürürse, bu çıktıları güvenli hale getirmek için bu eylemde el ile **güvenli çıktılar** açmanız gerekir.

* Bu verilerin güvenliğini sağlamak için çalıştırma geçmişinin beklendiğini bekleyen aşağı akış eylemlerinde **güvenli girişleri** veya **güvenli çıkışları** etkinleştirdiğinizden emin olun.

  **Güvenli çıkışlar ayarı**

  Bir tetikleyici veya eylemde **güvenli çıkışları** el ile açtığınızda Logic Apps, bu çıkışların çalıştırma geçmişinde güvenliğini sağlar. Bir aşağı akış eylemi açık olarak bu güvenli çıkışları giriş olarak kullanıyorsa Logic Apps, bu eylemin çalışma geçmişinde girişlerini gizler, ancak eylemin **güvenli girişler** ayarını *etkinleştirmez* .

  ![Birçok eylemin giriş ve aşağı akış etkisi olarak güvenli çıktılar](media/logic-apps-securing-a-logic-app/secure-outputs-as-inputs-flow.png)

  Oluşturma, ayrıştırma JSON ve yanıt eylemlerinin yalnızca **güvenli girişler** ayarı vardır. Açık olduğunda, ayar bu eylemlerin çıkışlarını da gizler. Bu eylemler, yukarı akış güvenli çıkışlarını giriş olarak açıkça kullanıyorsa, Logic Apps bu eylemlerin girişlerini ve çıkışlarını gizler, ancak bu eylemlerin **güvenli girişler** ayarını *etkinleştirmez* . Bir aşağı akış eylemi, giriş olarak oluşturma, ayrıştırma JSON veya Yanıt eylemlerdeki gizli çıkışları açıkça kullanıyorsa, Logic Apps *Bu aşağı akış eyleminin giriş veya çıkışlarını gizlemez*.

  ![Belirli eylemlerdeki aşağı akış etkisi olan giriş olarak güvenli çıktılar](media/logic-apps-securing-a-logic-app/secure-outputs-as-inputs-flow-special.png)

  **Güvenli girişler ayarı**

  Bir tetikleyici veya eylemde **güvenli girişleri** el ile açtığınızda Logic Apps, bu girişlerin çalıştırma geçmişinde güvenliğini sağlar. Bir aşağı akış eylemi, giriş olarak bu tetikleyici veya eylemden görünür çıkışları açıkça kullanıyorsa, Logic Apps bu aşağı akış eyleminin çalıştırma geçmişinde girişlerini gizler, ancak bu eylemde güvenli girişleri etkinleştirmez ve bu eylemin **gizlenmemesini** *sağlar* çıkışı.

  ![Birçok eylemin güvenli girişler ve aşağı akış etkileri](media/logic-apps-securing-a-logic-app/secure-inputs-flow.png)

  Oluşturma, ayrıştırma JSON ve yanıt eylemleri, tetikleyici veya eylemden güvenli girişler içeren görünür çıktıları açıkça kullanıyorsa, Logic Apps bu eylemlerin girişlerini ve çıkışlarını gizler, ancak bu eylemin **güvenli girişlerini** *etkinleştirmez* ayarlanmasını. Bir aşağı akış eylemi, giriş olarak oluşturma, ayrıştırma JSON veya Yanıt eylemlerdeki gizli çıkışları açıkça kullanıyorsa, Logic Apps *Bu aşağı akış eyleminin giriş veya çıkışlarını gizlemez*.

  ![Belirli eylemlerdeki güvenli girişler ve aşağı akış etkisi](media/logic-apps-securing-a-logic-app/secure-inputs-flow-special.png)

<a name="secure-action-parameters"></a>

## <a name="access-to-parameter-inputs"></a>Parametre girdilerine erişim

Farklı ortamlara dağıtırsanız, iş akışı tanımınızdaki bu ortamlara göre farklılık gösteren değerleri parametreleştirmeyi göz önünde bulundurun. Bu şekilde, mantıksal uygulamanızı dağıtmak, güvenli parametreleri tanımlayarak gizli bilgileri korumak ve bir parametre kullanarak bu parametre girişlerini şablon parametreleri aracılığıyla ayrı olarak sağlamak için bir [Azure Resource Manager şablonu](../azure-resource-manager/resource-group-authoring-templates.md#parameters) kullanabilirsiniz [ dosya](../azure-resource-manager/resource-group-template-deploy.md#pass-parameter-values).

Örneğin, [Azure Active Directory](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication)ile http eylemlerinin kimliğini doğruladıysanız, kimlik doğrulaması için kullanılan istemci kimliğini ve istemci gizli anahtarını kabul eden parametreleri tanımlayabilir ve güvenli hale getirebilirsiniz. Mantıksal uygulamanız için bu parametreleri tanımlamak üzere mantıksal uygulamanızın iş akışı tanımının içindeki `parameters` bölümünü kullanın. Mantıksal uygulamanızı düzenlenirken veya çalıştırma geçmişini görüntülerken görüntülenmesini istemediğiniz parametre değerlerini korumak için `securestring` veya `secureobject` türünü kullanarak parametreleri tanımlayın ve kodlamayı gerektiği şekilde kullanın. Bu türe sahip parametreler kaynak tanımıyla döndürülmez ve dağıtımdan sonra kaynak görüntülenirken erişilebilir değildir. Çalışma zamanı sırasında bu parametre değerlerine erişmek için, iş akışı tanımınızın içindeki `@parameters('<parameter-name>')` ifadesini kullanın. Bu ifade yalnızca çalışma zamanında değerlendirilir ve [Iş akışı Tanım Dili](../logic-apps/logic-apps-workflow-definition-language.md)tarafından açıklanmıştır.

> [!NOTE]
> Bir HTTP isteğinin üst bilgilerinde veya gövdesinde bir parametre kullanırsanız, mantıksal uygulamanızın çalıştırma geçmişini ve giden HTTP isteğini görüntülediğinizde bu parametre görülebilir. İçerik erişim ilkelerinizi da uygun şekilde ayarladığınızdan emin olun. Yetkilendirme üstbilgileri, girişler veya çıktılar aracılığıyla hiçbir şekilde görünmez. Bu nedenle, bir gizli dizi kullanılırsa gizli anahtar alınmaz.

Daha fazla bilgi için bu konunun ilerleyen kısımlarında [yer alarak iş akışı tanımlarında güvenli parametreler](#secure-parameters-workflow) ' i inceleyin.

[Azure Resource Manager şablonlarıyla](../azure-resource-manager/resource-group-authoring-templates.md#parameters)dağıtımları otomatikleştirdiğiniz zaman, `securestring` ve `secureobject` türlerini kullanarak dağıtımda değerlendirilen güvenli şablon parametrelerini tanımlayabilirsiniz. Şablon parametrelerini tanımlamak için, şablonunuzun en üst düzey `parameters` bölümünü kullanın ve bu, iş akışı tanımınızın `parameters` bölümünden farklıdır. Şablon parametrelerinin değerlerini sağlamak için ayrı bir [parametre dosyası](../azure-resource-manager/resource-group-template-deploy.md#pass-parameter-values)kullanın.

Örneğin, gizli dizileri kullanıyorsanız, bu gizli dizileri dağıtım [Azure Key Vault](../key-vault/key-vault-overview.md) ' den alan güvenli şablon parametrelerini tanımlayabilir ve kullanabilirsiniz. Daha sonra, parametre dosyanızdaki anahtar kasasına ve gizli dizi ile gizli dizi oluşturabilirsiniz. Daha fazla bilgi için şu konulara bakın:

* [Dağıtım sırasında güvenli parametre değerlerini geçirmek için Azure Key Vault kullanma](../azure-resource-manager/resource-manager-keyvault-parameter.md)
* Bu konunun ilerleyen kısımlarında [Azure Resource Manager şablonlarda güvenli parametreler](#secure-parameters-deployment-template)

<a name="secure-parameters-workflow"></a>

### <a name="secure-parameters-in-workflow-definitions"></a>İş akışı tanımlarında güvenli parametreler

Mantıksal uygulamanızın iş akışı tanımındaki hassas bilgileri korumak için, mantıksal uygulamanızı kaydettikten sonra bu bilgilerin görünür olmaması için güvenli parametreleri kullanın. Örneğin, bir HTTP eyleminin, bir Kullanıcı adı ve parola kullanan temel kimlik doğrulaması gerektirdiğini varsayalım. @No__t-0 bölümü, iş akışı tanımında, `securestring` türünü kullanarak `basicAuthPasswordParam` ve `basicAuthUsernameParam` parametrelerini tanımlar. Eylem tanımı daha sonra `authentication` bölümünde bu parametrelere başvurur.

```json
"definition": {
   "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
   "actions": {
      "HTTP": {
         "type": "Http",
         "inputs": {
            "method": "GET",
            "uri": "https://www.microsoft.com",
            "authentication": {
               "type": "Basic",
               "username": "@parameters('basicAuthUsernameParam')",
               "password": "@parameters('basicAuthPasswordParam')"
            }
         },
         "runAfter": {}
      }
   },
   "parameters": {
      "basicAuthPasswordParam": {
         "type": "securestring"
      },
      "basicAuthUsernameParam": {
         "type": "securestring"
      }
   },
   "triggers": {
      "manual": {
         "type": "Request",
         "kind": "Http",
         "inputs": {
            "schema": {}
         }
      }
   },
   "contentVersion": "1.0.0.0",
   "outputs": {}
}
```

<a name="secure-parameters-deployment-template"></a>

### <a name="secure-parameters-in-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarda güvenli parametreler

Mantıksal uygulama için Kaynak Yöneticisi şablonda birden çok `parameters` bölümü vardır. Parolaları, anahtarları, sırları ve diğer hassas bilgileri korumak için, `securestring` veya `secureobject` türünü kullanarak şablon düzeyinde ve iş akışı tanımı düzeyinde güvenli parametreleri tanımlayın. Daha sonra bu değerleri [Azure Key Vault](../key-vault/key-vault-overview.md) saklayabilir ve anahtar kasası ve gizli dizi için [parametre dosyasını](../azure-resource-manager/resource-group-template-deploy.md#pass-parameter-values) kullanabilirsiniz. Şablonunuz daha sonra bu bilgileri dağıtımda alır. Daha fazla bilgi için bkz. [dağıtım sırasında güvenli parametre değerlerini geçirmek için Azure Key Vault kullanma](../azure-resource-manager/resource-manager-keyvault-parameter.md).

Bu `parameters` bölümleri hakkında daha fazla bilgi aşağıdadır:

* Şablonun en üst düzeyinde `parameters` bölümü, şablonun *dağıtımda*kullandığı değerler için parametreleri tanımlar. Örneğin, bu değerler belirli bir dağıtım ortamı için bağlantı dizeleri içerebilir. Daha sonra bu değerleri ayrı bir [parametre dosyasında](../azure-resource-manager/resource-group-template-deploy.md#pass-parameter-values)saklayabilirsiniz, bu da bu değerlerin değiştirilmesini kolaylaştırır.

* Mantıksal uygulamanızın kaynak tanımında, ancak iş akışı tanımınızın dışında bir `parameters` bölümü, iş akışı tanımınızın parametrelerinin değerlerini belirtir. Bu bölümde, şablonunuzun parametrelerine başvuran şablon ifadelerini kullanarak bu değerleri atayabilirsiniz. Bu ifadeler dağıtımda değerlendirilir.

* İş akışı tanımınızın içinde, bir `parameters` bölümü mantıksal uygulamanızın çalışma zamanında kullandığı parametreleri tanımlar. Daha sonra bu parametrelere, çalışma zamanında değerlendirilen iş akışı Tanım ifadelerini kullanarak mantıksal uygulamanızın iş akışının içinde başvurabilirsiniz.

@No__t-0 türünü kullanan birden çok güvenli parametre tanımına sahip bu örnek şablon:

| Parametre adı | Açıklama |
|----------------|-------------|
| `TemplatePasswordParam` | Daha sonra iş akışı tanımının `basicAuthPasswordParam` parametresine geçirilen bir parolayı kabul eden bir şablon parametresi |
| `TemplateUsernameParam` | Daha sonra iş akışı tanımının `basicAuthUserNameParam` parametresine geçirilen bir kullanıcı adını kabul eden bir şablon parametresi |
| `basicAuthPasswordParam` | HTTP eyleminde temel kimlik doğrulama parolasını kabul eden bir iş akışı Tanım parametresi |
| `basicAuthUserNameParam` | HTTP eyleminde temel kimlik doğrulaması için Kullanıcı adını kabul eden bir iş akışı Tanım parametresi |
|||

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "LogicAppName": {
         "type": "string",
         "minLength": 1,
         "maxLength": 80,
         "metadata": {
            "description": "Name of the Logic App."
         }
      },
      "TemplatePasswordParam": {
         "type": "securestring"
      },
      "TemplateUsernameParam": {
         "type": "securestring"
      },
      "LogicAppLocation": {
         "type": "string",
         "defaultValue": "[resourceGroup().location]",
         "allowedValues": [
            "[resourceGroup().location]",
            "eastasia",
            "southeastasia",
            "centralus",
            "eastus",
            "eastus2",
            "westus",
            "northcentralus",
            "southcentralus",
            "northeurope",
            "westeurope",
            "japanwest",
            "japaneast",
            "brazilsouth",
            "australiaeast",
            "australiasoutheast",
            "southindia",
            "centralindia",
            "westindia",
            "canadacentral",
            "canadaeast",
            "uksouth",
            "ukwest",
            "westcentralus",
            "westus2"
         ],
         "metadata": {
            "description": "Location of the Logic App."
         }
      }
   },
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {
               "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-0601/workflowdefinition.json#",
               "actions": {
                  "HTTP": {
                     "type": "Http",
                     "inputs": {
                        "method": "GET",
                        "uri": "https://www.microsoft.com",
                        "authentication": {
                           "type": "Basic",
                           "username": "@parameters('basicAuthUsernameParam')",
                           "password": "@parameters('basicAuthPasswordParam')"
                        }
                     },
                  "runAfter": {}
                  }
               },
               "parameters": {
                  "basicAuthPasswordParam": {
                     "type": "securestring"
                  },
                  "basicAuthUsernameParam": {
                     "type": "securestring"
                  }
               },
               "triggers": {
                  "manual": {
                     "type": "Request",
                     "kind": "Http",
                     "inputs": {
                        "schema": {}
                     }
                  }
               },
               "contentVersion": "1.0.0.0",
               "outputs": {}
            },
            "parameters": {
               "basicAuthPasswordParam": {
                  "value": "[parameters('TemplatePasswordParam')]"
               },
               "basicAuthUsernameParam": {
                  "value": "[parameters('TemplateUsernameParam')]"
               }
            }
         }
      }
   ],
   "outputs": {}
}
```

<a name="secure-requests"></a>

## <a name="access-to-services-and-systems-called-from-logic-apps"></a>Logic Apps 'ten çağrılan hizmetlere ve sistemlere erişim

Mantıksal uygulamanızın gönderme isteklerine erişmesi gereken uç noktaları güvenli hale getirmek için kullanabileceğiniz bazı yollar şunlardır:

* Giden istekler üzerinde kimlik doğrulaması ekleyin.

  Bir HTTP, HTTP + Swagger (Open API) veya Web kancası eylemiyle çalışırken, mantıksal uygulamanız tarafından gönderilen isteğe kimlik doğrulaması ekleyebilirsiniz. Örneğin, temel kimlik doğrulaması, sertifika kimlik doğrulaması veya Azure Active Directory kimlik doğrulaması kullanabilirsiniz. Daha fazla bilgi için bkz. [Tetikleyiciler veya eylemlerin kimlik doğrulaması](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

* Mantıksal uygulama IP adreslerinden erişimi kısıtlayın.

  Mantıksal uygulamalardan gelen uç noktalara yapılan tüm çağrılar, mantıksal uygulamalarınızın bölgelerini temel alan belirli IP adreslerinden kaynaklardır. Yalnızca bu IP adreslerinden istekleri kabul eden filtre ekleyebilirsiniz. Bu IP adreslerini almak için bkz. [Azure Logic Apps Için sınırlar ve yapılandırma](logic-apps-limits-and-config.md#configuration).

* Yönetilen kimlikler olarak kimlik doğrulaması ve kaynaklara erişme.

  Diğer Azure Active Directory (Azure AD) kiracılarındaki kaynaklara erişmek için, mantıksal uygulamanız kimlik bilgileri veya gizli dizileri yerine yönetilen bir kimlik (eski adıyla Yönetilen Hizmet Kimliği veya MSI) kullanarak oturum açmadan kimliğinizi doğrulayabilir. Azure bu kimliği sizin için yönetir ve gizli dizileri sağlamanız veya döndürmenize gerek olmadığından kimlik bilgilerinizin güvenliğini sağlamaya yardımcı olur. Mantıksal uygulamanız için sistem tarafından atanan yönetilen bir kimlik ayarlama ve kullanma hakkında daha fazla bilgi için, bkz. [Azure Logic Apps yönetilen kimliklerle kaynak kimlik doğrulaması ve erişim](../logic-apps/create-managed-service-identity.md).

* Şirket içi sistemlere güvenli bağlantılar.

  Azure Logic Apps, güvenli ve güvenilir şirket içi iletişim için bu hizmetlerle tümleştirme sağlar.

  * Şirket içi veri ağ geçidi

    Azure Logic Apps ' de birçok yönetilen bağlayıcı, dosya sistemi, SQL, SharePoint ve DB2 gibi şirket içi sistemlere güvenli bağlantılar sağlar. Ağ Geçidi, Azure Service Bus aracılığıyla şifrelenmiş kanallardaki şirket içi kaynaklardaki verileri gönderir. Tüm trafik ağ geçidi aracısından güvenli giden trafik olarak gelir. [Şirket içi veri ağ geçidinin nasıl çalıştığını](logic-apps-gateway-install.md#gateway-cloud-service)öğrenin.

  * Azure API Management ile bağlanma

    [Azure API Management](../api-management/api-management-key-concepts.md) , siteden siteye sanal özel ağ ve güvenli proxy Için ExpressRoute tümleştirmesi ve şirket içi sistemlerle iletişim gibi şirket içi bağlantı seçenekleri sağlar. Mantıksal uygulama Tasarımcısı ' nda mantıksal uygulamanızın iş akışından, şirket içi sistemlere hızlı erişim sağlayan API Management tarafından açığa çıkarılan bir API seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Dağıtım şablonları oluşturma](logic-apps-create-deploy-template.md)  
* [Mantıksal uygulamalarınızı izleyin](logic-apps-monitor-your-logic-apps.md)  
* [Mantıksal uygulama başarısızlıklarını ve sorunlarını tanılama](logic-apps-diagnosing-failures.md)  
* [Mantıksal uygulama dağıtımını otomatikleştirme](logic-apps-azure-resource-manager-templates-overview.md)
