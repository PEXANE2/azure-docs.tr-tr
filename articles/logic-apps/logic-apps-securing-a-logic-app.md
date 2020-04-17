---
title: Erişimin ve verilerin güvenliğini sağlama
description: Azure Logic Apps'taki girişlere, çıktılara, istek tabanlı tetikleyicilere, çalışma geçmişine, yönetim görevlerine ve diğer kaynaklara erişimi güvenli hale getirme
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 02/04/2020
ms.openlocfilehash: e591a7035db82425952a16f5c4c220e25d8517fe
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457187"
---
# <a name="secure-access-and-data-in-azure-logic-apps"></a>Azure Logic Apps'ta güvenli erişim ve veri

Azure Logic Apps'taki verileri denetlemek ve verileri korumak için şu alanlarda güvenlik ayarlayabilirsiniz:

* [İstek tabanlı tetikleyicilere erişim](#secure-triggers)
* [Mantık uygulaması işlemlerine erişim](#secure-operations)
* [Geçmiş giriş ve çıktılarını çalıştırmaya erişim](#secure-run-history)
* [Parametre girişlerine erişim](#secure-action-parameters)
* [Mantık uygulamalarından çağrılan hizmet ve sistemlere erişim](#secure-requests)

<a name="secure-triggers"></a>

## <a name="access-to-request-based-triggers"></a>İstek tabanlı tetikleyicilere erişim

Mantık uygulamanız, [Istek](../connectors/connectors-native-reqres.md) veya [Webhook](../connectors/connectors-native-webhook.md) tetikleyicisi gibi gelen çağrıları veya istekleri alan istek tabanlı bir tetikleyici kullanıyorsa, erişimi yalnızca yetkili istemcilerin mantık uygulamanızı arayabilmeleri için sınırlandırabilirsiniz. Bir mantık uygulaması tarafından alınan tüm istekler, daha önce Güvenli Soketkatmanı (SSL) protokolü olarak bilinen Transport Layer Security (TLS) ile şifrelenir ve güvenli hale alınır.

Bu tetikleyici türüne erişimi güvenli hale almanıza yardımcı olabilecek seçenekler şunlardır:

* [Paylaşılan erişim imzaları oluşturma](#sas)
* [Gelen IP adreslerini kısıtlama](#restrict-inbound-ip-addresses)
* [Azure Etkin Dizini OAuth veya diğer güvenlik ekleme](#add-authentication)

<a name="sas"></a>

### <a name="generate-shared-access-signatures-sas"></a>Paylaşılan erişim imzaları oluşturma (SAS)

Bir mantık uygulamasındaki her istek bitiş noktasının bitiş noktasının URL'sinde bu biçimi izleyen bir [Paylaşılan Erişim İmzası (SAS)](https://docs.microsoft.com/rest/api/storageservices/constructing-a-service-sas) vardır:

`https://<request-endpoint-URI>sp=<permissions>sv=<SAS-version>sig=<signature>`

Her URL, `sp` `sv`bu `sig` tabloda açıklandığı gibi , ve sorgu parametresi içerir:

| Sorgu parametresi | Açıklama |
|-----------------|-------------|
| `sp` | İzin verilen HTTP yöntemlerinin kullanımına izin verir. |
| `sv` | İmza oluşturmak için kullanılacak SAS sürümünü belirtir. |
| `sig` | Tetikleyiciye erişimin doğruluğunu doğrulamak için kullanılacak imzayı belirtir. Bu imza, tüm URL yollarında ve özelliklerinde gizli bir erişim anahtarına sahip SHA256 algoritması kullanılarak oluşturulur. Hiçbir zaman açığa çıkarılan veya yayınlanan bu anahtar şifrelenir ve mantık uygulaması ile saklanır. Mantık uygulamanız yalnızca gizli anahtarla oluşturulan geçerli bir imza içeren tetikleyicilere yetki vetir. |
|||

SAS ile erişimi güvence altına alma hakkında daha fazla bilgi için bu konuyla ilgili şu bölümlere bakın:

* [Erişim anahtarlarını yeniden oluşturma](#access-keys)
* [Süresi dolan geri arama URL'leri oluşturma](#expiring-urls)
* [Birincil veya ikincil anahtarla URL'ler oluşturma](#primary-secondary-key)

<a name="access-keys"></a>

#### <a name="regenerate-access-keys"></a>Erişim anahtarlarını yeniden oluşturma

İstediğiniz zaman yeni bir güvenlik erişim anahtarı oluşturmak için Azure REST API veya Azure portalını kullanın. Eski anahtarı kullanan daha önce oluşturulmuş tüm URL'ler geçersiz kılınur ve artık mantık uygulamasını tetikleme yetkisiyoktur. Yenilemeden sonra aldığınız URL'ler yeni erişim anahtarıyla imzalanır.

1. Azure [portalında,](https://portal.azure.com)yeniden oluşturmak istediğiniz anahtara sahip mantık uygulamasını açın.

1. Mantık uygulamasının menüsünde Ayarlar **altında** **Erişim Tuşları'nı**seçin.

1. Yeniden oluşturmak istediğiniz anahtarı seçin ve işlemi tamamla.

<a name="expiring-urls"></a>

#### <a name="create-expiring-callback-urls"></a>Süresi dolan geri arama URL'leri oluşturma

İstek tabanlı bir tetikleyicinin bitiş noktası URL'sini diğer taraflarla paylaşırsanız, belirli anahtarları kullanan ve son kullanma tarihleri olan geri arama URL'leri oluşturabilirsiniz. Bu şekilde, belirli bir zaman aralığına bağlı olarak tuşları sorunsuz bir şekilde atabilir veya mantık uygulamanızı tetikleme erişimini kısıtlayabilirsiniz. Bir URL için son kullanma tarihi belirtmek [için, Örneğin Logic Apps REST API'sını](https://docs.microsoft.com/rest/api/logic/workflowtriggers)kullanın:

``` http
POST /subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<workflow-name>/triggers/<trigger-name>/listCallbackUrl?api-version=2016-06-01
```

Gövdede, bir `NotAfter`JSON tarih dizekullanarak özelliği içerir. Bu özellik, yalnızca tarih ve saate `NotAfter` kadar geçerli olan bir geri arama URL'si döndürür.

<a name="primary-secondary-key"></a>

#### <a name="create-urls-with-primary-or-secondary-secret-key"></a>Birincil veya ikincil gizli anahtarla URL'ler oluşturma

İstek tabanlı bir tetikleyici için geri arama URL'leri oluşturduğunuzda veya listelediğinizde, URL'yi imzalamak için kullanılacak anahtarı belirtebilirsiniz. Belirli bir anahtar tarafından imzalanmış bir URL oluşturmak için, örneğin [Logic Apps REST API'sını](https://docs.microsoft.com/rest/api/logic/workflowtriggers)kullanın:

``` http
POST /subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<workflow-name>/triggers/<trigger-name>/listCallbackUrl?api-version=2016-06-01
```

Vücutta, ya da `KeyType` `Primary` `Secondary`olarak özelliği içerir. Bu özellik, belirtilen güvenlik anahtarı tarafından imzalanmış bir URL döndürür.

<a name="restrict-inbound-ip"></a>

### <a name="restrict-inbound-ip-addresses"></a>Gelen IP adreslerini kısıtlama

Paylaşılan Erişim İmzası (SAS) ile birlikte, mantık uygulamanızı çağırabilecek istemcileri özellikle sınırlamak isteyebilirsiniz. Örneğin, Azure API Yönetimi'ni kullanarak istek bitiş noktanızı yönetiyorsanız, mantık uygulamanızı yalnızca API Yönetimi örneğinin IP adresinden gelen istekleri kabul etmesi için kısıtlayabilirsiniz.

#### <a name="restrict-inbound-ip-ranges-in-azure-portal"></a>Azure portalında gelen IP aralıklarını kısıtlama

1. Azure [portalında](https://portal.azure.com)mantık uygulamanızı Logic App Designer'da açın.

1. Mantık uygulamanızın menüsünde Ayarlar **altında** **İş Akışı ayarlarını**seçin.

1. **Erişim denetimi yapılandırması** > altında**İzin verilen gelen IP adresleri,** **Belirli IP aralıklarını**seçin.

1. **Tetikleyiciler için IP aralıkları**altında, tetikleyicinin kabul ettiği IP adresi aralıklarını belirtin.

   A valid IP range uses these formats: *x.x.x.x/x* or *x.x.x.x-x.x.x.x*

Mantık uygulamanızın yalnızca iç içe açılan bir mantık uygulaması olarak tetiklebilmesini istiyorsanız, **İzin Verilen gelen IP adresleri** listesinden Yalnızca Diğer Mantık **Uygulamaları'nı**seçin. Bu seçenek, mantık uygulaması kaynağınıza boş bir dizi yazar. Bu şekilde, yalnızca Logic Apps hizmetinden gelen aramalar (üst mantık uygulamaları) iç içe olan mantık uygulamasını tetikleyebilir.

> [!NOTE]
> IP adresi ne olursa olsun, Azure REST API'sini kullanarak `/triggers/<trigger-name>/run` veya API Yönetimi aracılığıyla istek tabanlı tetikleyiciye sahip bir mantık uygulaması çalıştırmaya devam edebilirsiniz. Ancak, bu senaryo yine de Azure REST API karşı kimlik doğrulaması gerektirir. Tüm etkinlikler Azure Denetim Günlüğü'nde görünür. Erişim denetimi ilkelerini buna göre ayarladığınızdan emin olun.

#### <a name="restrict-inbound-ip-ranges-in-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonundaki gelen IP aralıklarını kısıtlama

Kaynak [Yöneticisi şablonlarını kullanarak mantık uygulamaları için dağıtımı](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)otomatikleştirirseniz, örneğin `accessControl` mantık `triggers` uygulamanızın kaynak tanımındaki bölümü kullanarak IP aralıklarını belirtebilirsiniz:

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

### <a name="add-azure-active-directory-oauth-or-other-security"></a>Azure Etkin Dizini OAuth veya diğer güvenlik ekleme

Mantık uygulamanıza daha fazla yetkilendirme protokolü eklemek için [Azure API Yönetimi](../api-management/api-management-key-concepts.md) hizmetini kullanmayı düşünün. Bu hizmet, mantık uygulamanızı api olarak ortaya çıkarmanıza yardımcı olur ve herhangi bir uç nokta için zengin izleme, güvenlik, ilke ve dokümantasyon sunar. API Yönetimi, mantık uygulamanız için genel veya özel bir bitiş noktasını ortaya çıkarabilir. Bu bitiş noktasına erişimi yetkilendirmek için, bu uç noktaya erişimi yetkilendirmek için [Azure Active Directory OAuth,](#azure-active-directory-oauth-authentication) [istemci sertifikası](#client-certificate-authentication)veya diğer güvenlik standartlarını kullanabilirsiniz. API Yönetimi bir istek aldığında, hizmet isteği mantık uygulamanıza gönderir ve yol boyunca gerekli dönüşümleri veya kısıtlamaları yapar. Yalnızca API Yönetimi'nin mantık uygulamanızı tetikletmelerine izin vermek için mantık uygulamanızın gelen IP aralığı ayarlarını kullanabilirsiniz.

<a name="secure-operations"></a>

## <a name="access-to-logic-app-operations"></a>Mantık uygulaması işlemlerine erişim

Yalnızca belirli kullanıcıların veya grupların mantık uygulamalarını yönetme, düzenleme ve görüntüleme gibi belirli görevleri çalıştırabılmasına izin verebilirsiniz. İzinlerini denetlemek için Azure Aboneliğinizdeki üyelere özelleştirilmiş veya yerleşik roller atayabilmek için [Azure Role Tabanlı Erişim Denetimi'ni (RBAC)](../role-based-access-control/role-assignments-portal.md) kullanın:

* [Mantık Uygulaması Katılımcısı](../role-based-access-control/built-in-roles.md#logic-app-contributor): Mantık uygulamalarını yönetmenize olanak tanır, ancak bunlara erişimi değiştiremezsiniz.

* [Mantık Uygulama Operatörü](../role-based-access-control/built-in-roles.md#logic-app-operator): Mantık uygulamalarını okumanızı, etkinleştirmenizi ve devre dışı etmenizi sağlar, ancak bunları güncelleştiremezsiniz veya güncelleştiremezsiniz.

Başkalarının mantık uygulamanızı değiştirmesini veya silemesini önlemek için [Azure Kaynak Kilidi'ni](../azure-resource-manager/management/lock-resources.md)kullanabilirsiniz. Bu özellik, başkalarının üretim kaynaklarını değiştirmesini veya siler.

<a name="secure-run-history"></a>

## <a name="access-to-run-history-data"></a>Çalışma geçmişi verilerine erişim

Bir mantık uygulaması çalıştırılırken, tüm veriler aktarım katmanı güvenliği (TLS) kullanılarak ve [istirahatte](../security/fundamentals/encryption-atrest.md) [aktarım sırasında şifrelenir.](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit) Mantık uygulamanız çalışma süresi bittiğinde, her eylem için durum, süre, giriş ler ve çıktılarla birlikte çalışan adımlar da dahil olmak üzere, o çalıştırmanın geçmişini görüntüleyebilirsiniz. Bu zengin ayrıntı, mantık uygulamanızın nasıl çalıştığı ve ortaya çıkan sorunları nerede gidermeye başleyebileceğiniz hakkında bilgi sağlar.

Mantık uygulamanızın çalışma geçmişini görüntülediğinizde, Logic Apps erişiminizi doğrular ve ardından her çalıştırma için istek ve yanıtlar için giriş ve çıktılara bağlantılar sağlar. Ancak, parolaları, sırları, anahtarları veya diğer hassas bilgileri işleyen eylemler için, başkalarının bu verileri görüntülemesini ve bu verilere erişmesini engellemek istersiniz. Örneğin, mantık uygulamanız bir HTTP eyleminin kimliğini doğrularken kullanmak üzere [Azure Key Vault'tan](../key-vault/general/overview.md) bir sır alırsa, bu sırrı görünümden gizlemek istersiniz.

Mantık uygulamanızın çalışma geçmişindeki giriş ve çıktılara erişimi denetlemek için şu seçeneklere sahipsiniz:

* [IP adresi aralığına göre erişimi kısıtlayın.](#restrict-ip)

  Bu seçenek, belirli bir IP adresi aralığından gelen isteklere göre geçmişi çalıştırmak için erişimi güvenli hale yardımcı olur.

* [Gizleme kullanarak çalışma geçmişindeki verileri gizleyin.](#obfuscate)

  Birçok tetikleyici ve eylemde, girişlerini, çıktılarını veya her ikisini de bir mantık uygulamasının çalışma geçmişinden gizleyebilirsiniz.

<a name="restrict-ip"></a>

### <a name="restrict-access-by-ip-address-range"></a>IP adres aralığına göre erişimi kısıtlama

Yalnızca belirli IP adresi aralıklarından gelen isteklerin bu verileri görüntüleyebilmeleri için mantık uygulamanızın çalışma geçmişindeki giriş ve çıktılara erişimi sınırlandırabilirsiniz. Örneğin, herhangi birinin giriş ve çıkışlara erişmesini engellemek için, `0.0.0.0-0.0.0.0`', .. Yalnızca yönetici izinlerine sahip bir kişi, mantık uygulamanızın verilerine "tam zamanında" erişim olanağı sağlayan bu kısıtlamayı kaldırabilir. Ip aralıklarını, Azure portalını kullanarak veya mantıksal uygulama dağıtımı için kullandığınız bir Azure Kaynak Yöneticisi şablonunda kısıtlamak için belirtebilirsiniz.

#### <a name="restrict-ip-ranges-in-azure-portal"></a>Azure portalında IP aralıklarını kısıtlama

1. Azure portalında mantık uygulamanızı Logic App Designer'da açın.

1. Mantık uygulamanızın menüsünde Ayarlar **altında** **İş Akışı ayarlarını**seçin.

1. **Erişim denetimi yapılandırması** > altında**İzin verilen gelen IP adresleri,** **Belirli IP aralıklarını**seçin.

1. **İçerik için IP aralıkları**altında, giriş ve çıktılardan içeriğe erişebilen IP adres aralıklarını belirtin. 

   A valid IP range uses these formats: *x.x.x.x/x* or *x.x.x.x-x.x.x.x*

#### <a name="restrict-ip-ranges-in-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonundaki IP aralıklarını kısıtlama

Kaynak [Yöneticisi şablonlarını kullanarak mantık uygulamaları için dağıtımı](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)otomatikleştirirseniz, örneğin `accessControl` mantık `contents` uygulamanızın kaynak tanımındaki bölümü kullanarak IP aralıklarını belirtebilirsiniz:

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

### <a name="hide-data-from-run-history-by-using-obfuscation"></a>Gizleme kullanarak verileri çalışma geçmişinden gizleme

Birçok tetikleyici ve eylemin girişleri, çıktıları veya her ikisini de bir mantık uygulamasının çalışma geçmişinden gizlemek için ayarları vardır. Bu verileri güvenli hale getirmek için bu ayarları kullandığınızda [gözden geçirmeniz gereken](#obfuscation-considerations) bazı noktalar aşağıda verilmiştir.

#### <a name="hide-inputs-and-outputs-in-the-designer"></a>Tasarımcıdaki giriş ve çıktıları gizleme

1. Azure [portalında](https://portal.azure.com)mantık uygulamanızı Logic App Designer'da açın.

   ![Logic App Designer'da açık mantık uygulaması](./media/logic-apps-securing-a-logic-app/open-sample-logic-app-in-designer.png)

1. Hassas verileri gizlemek istediğiniz tetikleyici veya eylemde elipsler (**...**) düğmesini seçin ve ardından **Ayarlar'ı**seçin.

   ![Tetikleyici veya eylem ayarlarını açma](./media/logic-apps-securing-a-logic-app/open-action-trigger-settings.png)

1. **Güvenli Girişler, Güvenli** **Çıktılar**veya her ikisini de açın. İşiniz bittiğinde **Bitti**'yi seçin.

   !["Güvenli Girişler" veya "Güvenli Çıktılar"](./media/logic-apps-securing-a-logic-app/turn-on-secure-inputs-outputs.png)

   Eylem veya tetikleyici artık başlık çubuğunda bir kilit simgesi gösterir.

   ![Eylem veya tetikleyici başlık çubuğu kilit simgesini gösterir](./media/logic-apps-securing-a-logic-app/lock-icon-action-trigger-title-bar.png)

   Önceki eylemlerden güvenli çıktıları temsil eden belirteçler de kilit simgelerini gösterir. Örneğin, bir eylemde kullanılacak dinamik içerik listesinden böyle bir çıktı seçtiğinizde, bu belirteç bir kilit simgesi gösterir.

   ![Güvenli çıktı için belirteç seçin](./media/logic-apps-securing-a-logic-app/select-secured-token.png)

1. Mantık uygulaması çalıştırdıktan sonra, bu çalıştırmanın geçmişini görüntüleyebilirsiniz.

   1. Mantık uygulamasının **Genel Bakış** bölmesine, görüntülemek istediğiniz çalıştır'ı seçin.

   1. Mantık **uygulaması çalıştır** bölmesinde, gözden geçirmek istediğiniz eylemleri genişletin.

      Hem girişleri hem de çıktıları gizlemeye seçtiyseniz, bu değerler artık gizli görünür.

      ![Çalışma geçmişindeki gizli giriş ler ve çıktılar](./media/logic-apps-securing-a-logic-app/hidden-data-run-history.png)

<a name="secure-data-code-view"></a>

#### <a name="hide-inputs-and-outputs-in-code-view"></a>Giriş ve çıktıları kod görünümünde gizleme

Temel tetikleyici veya eylem tanımında, `runtimeConfiguration.secureData.properties` diziyi bu değerlerden biriyle veya her ikisiyle ekleyin veya güncelleştirin:

* `"inputs"`: Çalışma geçmişindeki girişleri güvenli hale getirir.
* `"outputs"`: Çalışma geçmişindeki çıktıları güvence altına alır.

Bu verileri güvenli hale getirmek için bu ayarları kullandığınızda [gözden geçirmeniz gereken](#obfuscation-considerations) bazı noktalar aşağıda verilmiştir.

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

#### <a name="considerations-when-hiding-inputs-and-outputs"></a>Giriş ve çıktıları gizlerken dikkat edilmesi gerekenler

* Bir tetikleyici veya eylemdeki girdileri veya çıktıları gizlediğinizde, Logic Apps güvenli verileri Azure Log Analytics'e göndermez. Ayrıca, bu tetikleyiciye [izlenen özellikleri](../logic-apps/monitor-logic-apps-log-analytics.md#extend-data) veya izleme eylemi ekemezsiniz.

* [İş akışı geçmişini işlemek için Logic Apps API](https://docs.microsoft.com/rest/api/logic/) güvenli çıktıları döndürmez.

* Girişleri gizleyen veya çıktıları açıkça gizleyen bir eylemden çıktıları gizlemek için, bu eylemde **Güvenli Çıktıları** el ile açın.

* Çalışma geçmişinin bu verileri gizlemesini beklediğiniz akış aşağı eylemlerinde **Güvenli Girişleri** veya **Güvenli Çıktıları** açtığınızdan emin olun.

  **Güvenli Çıktılar ayarı**

  Bir tetikleyici veya eylemde **Güvenli Çıktıları** el ile açtığınızda, Logic Apps bu çıktıları çalışma geçmişinde güvence altına alır. Bir akış aşağı eylem açıkça giriş olarak bu güvenli çıkışları kullanırsa, Logic Apps bu eylemin girişlerini çalışma geçmişinde gizler, ancak eylemin **Güvenli Girişler** ayarını *etkinleştiremez.*

  ![Girdiler ve akış aşağı etkisi çoğu eylem üzerinde güvenli çıkışlar](./media/logic-apps-securing-a-logic-app/secure-outputs-as-inputs-flow.png)

  Oluşturma, Ayrıştırık JSON ve Yanıt eylemleri yalnızca **Güvenli Giriş ayarına** sahiptir. Ayarı açıklandığında, bu eylemlerin çıktılarını da gizler. Bu eylemler, akış yukarı güvenli çıktıları giriş olarak açıkça kullanıyorsa, Logic Apps bu eylemlerin giriş ve çıktılarını gizler, ancak bu eylemlerin **Güvenli Giriş** ayarını *etkinleştiremez.* Bir akış aşağı eylem açıkça Giriş olarak Compose, Parse JSON veya Yanıt eylemleri gizli çıkışları kullanırsa, Logic Apps *bu downstream eylem girişveya çıkışları gizlemek değildir.*

  ![Belirli eylemler üzerinde aşağı akım etkisi olan girdiler olarak güvenli çıktılar](./media/logic-apps-securing-a-logic-app/secure-outputs-as-inputs-flow-special.png)

  **Güvenli Girişler ayarı**

  Bir tetikleyici veya eylemde **Güvenli Girişleri** el ile açtığınızda, Logic Apps çalışma geçmişinde bu girişleri güvence altına alar. Bir akış aşağı eylem açıkça giriş olarak bu tetikleyici veya eylem görünür çıkışları kullanırsa, Logic Apps bu akış aşağı eylem girdileri çalışma geçmişinde gizler, ancak bu eylem **güvenli girişleri** *etkinleştirmiyor* ve bu eylemin çıktılarını gizlemiyor.

  ![Güvenli girişler ve çoğu eylem üzerinde akış aşağı etkisi](./media/logic-apps-securing-a-logic-app/secure-inputs-impact-on-downstream.png)

  Oluşturma, Ayrıştırma JSON ve Yanıt eylemleri, güvenli girişleri olan tetikleyici veya eylemden görünen çıktıları açıkça kullanıyorsa, Logic Apps bu eylemlerin giriş ve çıktılarını gizler, ancak bu eylemin **Güvenli Giriş ayarı** *etkinleştirmez.* Bir akış aşağı eylem açıkça Giriş olarak Compose, Parse JSON veya Yanıt eylemleri gizli çıkışları kullanırsa, Logic Apps *bu downstream eylem girişveya çıkışları gizlemek değildir.*

  ![Güvenli girişler ve belirli eylemler üzerindeki aşağı akış etkisi](./media/logic-apps-securing-a-logic-app/secure-inputs-flow-special.png)

<a name="secure-action-parameters"></a>

## <a name="access-to-parameter-inputs"></a>Parametre girişlerine erişim

Farklı ortamlar arasında dağıtıyorsanız, iş akışı tanımınızdaki bu ortamlara göre değişen değerleri parametrelendirmeyi düşünün. Bu şekilde, mantıksal uygulamanızı dağıtmak için bir [Azure Kaynak Yöneticisi şablonu](../azure-resource-manager/templates/overview.md) kullanarak sabit kodlu verilerden kaçınabilir, güvenli parametreleri tanımlayarak hassas verileri koruyabilir ve bir [parametre dosyası](../azure-resource-manager/templates/parameter-files.md)kullanarak [şablonun parametreleri](../azure-resource-manager/templates/template-parameters.md) aracılığıyla bu verileri ayrı girdiler olarak geçirebilirsiniz.

Örneğin, [Azure Etkin Dizin OAuth](#azure-active-directory-oauth-authentication)ile HTTP eylemlerinin kimliğini doğrularsanız, kimlik doğrulama için kullanılan istemci kimliğini ve istemci sırrını kabul eden parametreleri tanımlayabilir ve gizleyebilirsiniz. Mantık uygulamanızda bu parametreleri tanımlamak `parameters` için, mantık uygulamanızın iş akışı tanımındaki bölümü ve dağıtım için Kaynak Yöneticisi şablonunu kullanın. Mantık uygulamanızı düzenlerken veya çalışma geçmişini görüntülerken gösterilmesini istemediğiniz parametre değerlerini `securestring` gizlemek `secureobject` için, parametreleri kullanarak veya türü kullanarak tanımlayın ve gerektiğinde kodlamayı kullanın. Bu türdeki parametreler kaynak tanımıyla döndürülemez ve dağıtımdan sonra kaynağı görüntülerken erişilemez. Çalışma zamanı sırasında bu parametre değerlerine erişmek için iş akışı tanımınızdaki ifadeyi `@parameters('<parameter-name>')` kullanın. Bu ifade yalnızca çalışma zamanında değerlendirilir ve [İş Akışı Tanımı Dili](../logic-apps/logic-apps-workflow-definition-language.md)tarafından açıklanır.

> [!NOTE]
> İstek üstbilgisinde veya gövdesinde bir parametre kullanıyorsanız, mantık uygulamanızın çalışma geçmişini ve giden HTTP isteğini görüntülediğinizde bu parametre görülebilir. İçerik erişim ilkelerinizi de buna göre ayarladığınızdan emin olun. Çalışma geçmişinizdeki giriş ve çıktıları gizlemek için [gizleme](#obfuscate) de kullanabilirsiniz. Yetkilendirme üstbilgisi girişler veya çıktılar aracılığıyla hiçbir zaman görünmez. Yani orada bir sır kullanılırsa, bu sır geri alınamaz.

Daha fazla bilgi için bu konuyla ilgili bölümlere bakın:

* [İş akışı tanımlarında güvenli parametreler](#secure-parameters-workflow)
* [Gizleme kullanarak verileri çalışma geçmişinden gizleme](#obfuscate)

Kaynak [Yöneticisi şablonlarını kullanarak mantık uygulamaları için dağıtımı otomatikleştirirseniz,](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)dağıtımsırasında değerlendirilen güvenli şablon `securestring` parametrelerini, bunları ve `secureobject` türlerini kullanarak tanımlayabilirsiniz. [template parameters](../azure-resource-manager/templates/template-parameters.md) Şablon parametrelerini tanımlamak için, şablonunuzun iş akışı tanımınızın `parameters` `parameters` bölümünden ayrı ve farklı olan üst düzey bölümünü kullanın. Şablon parametreleri için değerleri sağlamak için ayrı bir [parametre dosyası](../azure-resource-manager/templates/parameter-files.md)kullanın.

Örneğin, sırları kullanırsanız, dağıtım sırasında Azure Anahtar Kasası'ndan [Azure Key Vault](../key-vault/general/overview.md) bu sırları alan güvenli şablon parametrelerini tanımlayabilir ve kullanabilirsiniz. Daha sonra parametre dosyanızda anahtar kasasına ve gizliye başvuruyapabilirsiniz. Daha fazla bilgi için şu konulara bakın:

* [Azure Anahtar Kasası'nı kullanarak dağıtımsırasında hassas değerleri geçirin](../azure-resource-manager/templates/key-vault-parameter.md)
* [Azure Kaynak Yöneticisi şablonlarında güvenli parametreler](#secure-parameters-deployment-template) daha sonra bu konuda

<a name="secure-parameters-workflow"></a>

### <a name="secure-parameters-in-workflow-definitions"></a>İş akışı tanımlarında güvenli parametreler

Mantık uygulamanızın iş akışı tanımındaki hassas bilgileri korumak için, mantık uygulamanızı kurtardıktan sonra bu bilgilerin görünmeyebilmesi için güvenli parametreleri kullanın. Örneğin, bir HTTP eyleminin kullanıcı adı ve parola kullanan temel kimlik doğrulaması gerektirdiğini varsayalım. İş akışı tanımında, `parameters` bölüm `basicAuthPasswordParam` `securestring` türü `basicAuthUsernameParam` kullanarak parametreleri ve parametreleri tanımlar. Eylem tanımı daha sonra bu `authentication` parametreleri bölüme başvurur.

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

### <a name="secure-parameters-in-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonlarında güvenli parametreler

Bir mantık uygulaması için Kaynak `parameters` Yöneticisi [şablonu](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md) birden çok bölümden oluşur. Parolaları, anahtarları, sırları ve diğer hassas bilgileri korumak için, şablon düzeyinde güvenli parametreleri `securestring` `secureobject` ve iş akışı tanımı düzeyinde veya türünü kullanarak tanımlayın. Daha sonra bu değerleri [Azure Key Vault'ta](../key-vault/general/overview.md) saklayabilir ve anahtar kasasına ve gizliye başvurmak için [parametre dosyasını](../azure-resource-manager/templates/parameter-files.md) kullanabilirsiniz. Şablonunuzun ardından bu bilgileri dağıtım sırasında alır. Daha fazla bilgi için Azure [Anahtar Kasası'nı kullanarak dağıtımsırasında duyarlı değerleri geç' e](../azure-resource-manager/templates/key-vault-parameter.md)bakın.

Bu `parameters` bölümler hakkında daha fazla bilgi aşağıda verilmiştir:

* Şablonun en üst düzeyinde, `parameters` bir bölüm şablonun *dağıtımda*kullandığı değerlerin parametrelerini tanımlar. Örneğin, bu değerler belirli bir dağıtım ortamı için bağlantı dizeleri içerebilir. Daha sonra bu değerleri ayrı bir [parametre dosyasında](../azure-resource-manager/templates/parameter-files.md)depolayabilirsiniz , bu da bu değerleri değiştirmeyi kolaylaştırır.

* Mantık uygulamanızın kaynak tanımının içinde, ancak iş `parameters` akışı tanımınızın dışında, bir bölüm iş akışı tanımınızın parametrelerinin değerlerini belirtir. Bu bölümde, şablonunuzun parametrelerine başvuran şablon ifadeleri kullanarak bu değerleri atayabilirsiniz. Bu ifadeler dağıtım sırasında değerlendirilir.

* İş akışı tanımınızın `parameters` içinde, bir bölüm mantık uygulamanızın çalışma zamanında kullandığı parametreleri tanımlar. Daha sonra, çalışma zamanında değerlendirilen iş akışı tanımı ifadelerini kullanarak, mantık uygulamanızın iş akışı içinde bu parametrelere başvuruyapabilirsiniz.

Türü kullanan `securestring` birden çok güvenli parametre tanımına sahip olan bu örnek şablon:

| Parametre adı | Açıklama |
|----------------|-------------|
| `TemplatePasswordParam` | İş akışı tanımının `basicAuthPasswordParam` parametresine geçirilen bir parolayı kabul eden şablon parametresi |
| `TemplateUsernameParam` | İş akışı tanımının `basicAuthUserNameParam` parametresine geçirilen bir kullanıcı adını kabul eden şablon parametresi |
| `basicAuthPasswordParam` | Bir HTTP eyleminde temel kimlik doğrulama parolasını kabul eden bir iş akışı tanımı parametresi |
| `basicAuthUserNameParam` | Bir HTTP eyleminde temel kimlik doğrulama sı için kullanıcı adını kabul eden iş akışı tanımı parametresi |
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

## <a name="access-to-services-and-systems-called-from-logic-apps"></a>Mantık uygulamalarından çağrılan hizmet ve sistemlere erişim

Mantık uygulamanızdan arama veya istek alan uç noktaların güvenliğini sağlamanın bazı yolları şunlardır:

* Giden isteklere kimlik doğrulama ekleyin.

  HTTP, HTTP + Swagger veya Webhook gibi giden aramalar yapan HTTP tabanlı bir tetikleyici veya eylemle çalışırken, mantık uygulamanız tarafından gönderilen isteğe kimlik doğrulama ekleyebilirsiniz. Örneğin, bu kimlik doğrulama türlerini kullanabilirsiniz:

  * [Temel kimlik doğrulaması](#basic-authentication)

  * [İstemci sertifikası kimlik doğrulaması](#client-certificate-authentication)

  * [Active Directory OAuth kimlik doğrulaması](#azure-active-directory-oauth-authentication)

  * [Yönetilen kimlik doğrulaması](#managed-identity-authentication)
  
  Daha fazla bilgi için [bkz.](#add-authentication-outbound)

* Mantık uygulaması IP adreslerinden erişimi kısıtlayın.

  Mantık uygulamalarından uç noktalara yapılan tüm çağrılar, mantık uygulamalarınızın bölgelerini temel alan belirli belirlenmiş IP adreslerinden kaynaklanır. Yalnızca bu IP adreslerinden gelen istekleri kabul eden filtreleme ekleyebilirsiniz. Bu IP adreslerini almak [için Azure Mantık Uygulamaları için Sınırlar ve yapılandırma ya](logic-apps-limits-and-config.md#configuration)da yapılandırma ya da

* Şirket içi sistemlere bağlantılar için güvenliği artırın.

  Azure Logic Apps, daha güvenli ve güvenilir şirket içi iletişim sağlamaya yardımcı olmak için bu hizmetlerle entegrasyon sağlar.

  * Şirket içi veri ağ geçidi

    Azure Logic Apps'taki birçok yönetilen bağlayıcı, Dosya Sistemi, SQL, SharePoint ve DB2 gibi şirket içi sistemlere güvenli bağlantılar sağlar. Ağ geçidi, şirket içi kaynaklardan gelen verileri Azure Hizmet Veri Servisi aracılığıyla şifreli kanallardan gönderir. Tüm trafik ağ geçidi aracısından güvenli giden trafik olarak kaynaklanır. [Şirket içi veri ağ geçidinin nasıl çalıştığını](logic-apps-gateway-install.md#gateway-cloud-service)öğrenin.

  * Azure API Yönetimi ile bağlantı kurun

    [Azure API Yönetimi,](../api-management/api-management-key-concepts.md) siteden siteye sanal özel ağ ve şirket içi sistemlere güvenli proxy ve iletişim için ExpressRoute tümleştirmesi gibi şirket içi bağlantı seçenekleri sağlar. Mantık uygulamanızın Logic App Designer'daki iş akışından, api yönetimi tarafından açığa çıkarılan ve şirket içi sistemlere hızlı erişim sağlayan bir API seçebilirsiniz.

<a name="add-authentication-outbound"></a>

## <a name="add-authentication-to-outbound-calls"></a>Giden aramalara kimlik doğrulama ekleme

HTTP ve HTTPS uç noktaları çeşitli kimlik doğrulama türlerini destekler. Giden aramaları veya bu uç noktalara erişen isteklerde kullanmak için kullandığınız tetikleyiciye veya eyleme bağlı olarak, farklı kimlik doğrulama türleri aralıklarından seçim yapabilirsiniz. Mantık uygulamanızın işlediği hassas bilgileri koruduğunuzdan emin olmak için güvenli parametreleri kullanın ve gerektiğinde verileri kodlayın. Parametreleri kullanma ve güvence altına alma hakkında daha fazla bilgi [için bkz.](#secure-action-parameters)

> [!NOTE]
> Logic App Designer'da **Kimlik Doğrulama** özelliği, kimlik doğrulama türünü belirtebileceğiniz bazı tetikleyiciler ve eylemlerde gizlenmiş olabilir. Özelliğin bu gibi durumlarda görünmesini sağlamak için, tetikleyici veya eylemde, **yeni parametre ekle** listesini açın ve **Kimlik Doğrulama'yı**seçin. Daha fazla bilgi için bkz: [Yönetilen kimlikle erişimi kimlik doğrulaması.](../logic-apps/create-managed-service-identity.md#authenticate-access-with-identity)

| Kimlik doğrulaması türü | Tarafından desteklenir |
|---------------------|--------------|
| [Temel](#basic-authentication) | Azure API Yönetimi, Azure Uygulama Hizmetleri, HTTP, HTTP + Swagger, HTTP Webhook |
| [Müşteri Sertifikası](#client-certificate-authentication) | Azure API Yönetimi, Azure Uygulama Hizmetleri, HTTP, HTTP + Swagger, HTTP Webhook |
| [Aktif Dizin OAuth](#azure-active-directory-oauth-authentication) | Azure API Yönetimi, Azure Uygulama Hizmetleri, Azure Fonksiyonları, HTTP, HTTP + Swagger, HTTP Webhook |
| [Ham](#raw-authentication) | Azure API Yönetimi, Azure Uygulama Hizmetleri, Azure Fonksiyonları, HTTP, HTTP + Swagger, HTTP Webhook |
| [Yönetilen kimlik](#managed-identity-authentication) | Azure API Yönetimi, Azure Uygulama Hizmetleri, Azure Fonksiyonları, HTTP, HTTP + Swagger, HTTP Webhook |
|||

<a name="basic-authentication"></a>

### <a name="basic-authentication"></a>Temel kimlik doğrulama

[Temel](../active-directory-b2c/secure-rest-api.md) seçenek kullanılabilirse, aşağıdaki özellik değerlerini belirtin:

| Özellik (tasarımcı) | Emlak (JSON) | Gerekli | Değer | Açıklama |
|---------------------|-----------------|----------|-------|-------------|
| **Kimlik Doğrulaması** | `type` | Evet | Temel | Kullanılacak kimlik doğrulama türü |
| **Username** | `username` | Evet | <*kullanıcı adı*>| Hedef hizmet bitiş noktasına erişimin doğruluğunu doğrulamak için kullanıcı adı |
| **Parola** | `password` | Evet | <*Parola*> | Hedef hizmet bitiş noktasına erişim kimlik doğrulaması için parola |
||||||

Örneğin, [dağıtımı otomatikleştirmek için bir Azure Kaynak Yöneticisi şablonunda,](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)hassas bilgileri işlemek ve korumak için [güvenli parametreleri](#secure-action-parameters) kullandığınızda, bu parametre değerlerine çalışma zamanında erişmek için ifadeleri kullanabilirsiniz. Bu örnek HTTP eylem tanımı, `type` kimlik `Basic` doğrulamasını olarak belirtir ve parametre değerlerini almak için [parametreler() işlevini](../logic-apps/workflow-definition-language-functions-reference.md#parameters) kullanır:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "Basic",
         "username": "@parameters('userNameParam')",
         "password": "@parameters('passwordParam')"
      }
  },
  "runAfter": {}
}
```

<a name="client-certificate-authentication"></a>

### <a name="client-certificate-authentication"></a>İstemci Sertifikası kimlik doğrulaması

[İstemci Sertifikası](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md) seçeneği varsa, aşağıdaki özellik değerlerini belirtin:

| Özellik (tasarımcı) | Emlak (JSON) | Gerekli | Değer | Açıklama |
|---------------------|-----------------|----------|-------|-------------|
| **Kimlik Doğrulaması** | `type` | Evet | **Müşteri Sertifikası** <br>or <br>`ClientCertificate` | TLS/SSL istemci sertifikaları için kullanılacak kimlik doğrulama türü. Kendi imzalı sertifikalar desteklenirken, TLS/SSL için kendi imzalı sertifikalar desteklenmez. |
| **Pfx** | `pfx` | Evet | <*kodlanmış-pfx-dosya-içerik*> | Kişisel Bilgi Alışverişi (PFX) dosyasından temel 64 kodlanmış içerik <p><p>PFX dosyasını base64 kodlanmış biçime dönüştürmek için aşağıdaki adımları izleyerek PowerShell'i kullanabilirsiniz: <p>1. Sertifika içeriğini bir değişkene kaydedin: <p>   `$pfx_cert = get-content 'c:\certificate.pfx' -Encoding Byte` <p>2. İşlevi kullanarak sertifika `ToBase64String()` içeriğini dönüştürün ve bu içeriği bir metin dosyasına kaydedin: <p>   `[System.Convert]::ToBase64String($pfx_cert) | Out-File 'pfx-encoded-bytes.txt'` |
| **Parola** | `password`| Hayır | <*pfx dosyası için şifre*> | PFX dosyasına erişmek için parola |
|||||

Örneğin, [dağıtımı otomatikleştirmek için bir Azure Kaynak Yöneticisi şablonunda,](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)hassas bilgileri işlemek ve korumak için [güvenli parametreleri](#secure-action-parameters) kullandığınızda, bu parametre değerlerine çalışma zamanında erişmek için ifadeleri kullanabilirsiniz. Bu örnek HTTP eylem tanımı, `type` kimlik `ClientCertificate` doğrulamasını olarak belirtir ve parametre değerlerini almak için [parametreler() işlevini](../logic-apps/workflow-definition-language-functions-reference.md#parameters) kullanır:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "ClientCertificate",
         "pfx": "@parameters('pfxParam')",
         "password": "@parameters('passwordParam')"
      }
   },
   "runAfter": {}
}
```

İstemci sertifikası kimlik doğrulamasını kullanarak hizmetlerin güvenliğini sağlama hakkında daha fazla bilgi için şu konulara bakın:

* [Azure API Yönetimi'nde istemci sertifikası kimlik doğrulamasını kullanarak API'ler için güvenliği artırın](../api-management/api-management-howto-mutual-certificates-for-clients.md)
* [Azure API Yönetimi'nde istemci sertifikası kimlik doğrulamasını kullanarak arka uç hizmetlerinin güvenliğini artırın](../api-management/api-management-howto-mutual-certificates.md)
* [İstemci sertifikalarını kullanarak RESTfuL hizmetinizin güvenliğini artırın](../active-directory-b2c/secure-rest-api.md)
* [Uygulama kimlik doğrulaması için sertifika kimlik bilgileri](../active-directory/develop/active-directory-certificate-credentials.md)
* [Azure Uygulama Hizmeti'nde kodunuzda TLS/SSL sertifikası kullanma](../app-service/configure-ssl-certificate-in-code.md)

<a name="azure-active-directory-oauth-authentication"></a>

### <a name="azure-active-directory-oauth-authentication"></a>Azure Active Directory OAuth kimlik doğrulaması

Active [Directory OAuth](../active-directory/develop/about-microsoft-identity-platform.md) seçeneği varsa, aşağıdaki özellik değerlerini belirtin:

| Özellik (tasarımcı) | Emlak (JSON) | Gerekli | Değer | Açıklama |
|---------------------|-----------------|----------|-------|-------------|
| **Kimlik Doğrulaması** | `type` | Evet | **Aktif Dizin OAuth** <br>or <br>`ActiveDirectoryOAuth` | Kullanılacak kimlik doğrulama türü. Mantık Uygulamaları şu anda [OAuth 2.0 protokolünü](../active-directory/develop/v2-overview.md)takip etmektedir. |
| **Yetkili** | `authority` | Hayır | <*Yetki için URL-belirteç veren*> | Kimlik doğrulama belirteci sağlayan yetkili nin URL'si. Varsayılan olarak, bu `https://login.windows.net`değer . |
| **Kiracı** | `tenant` | Evet | <*kiracı kimliği*> | Azure AD kiracısının kiracı kimliği |
| **Hedef kitle** | `audience` | Evet | <*kaynak-yetkilendirme*> | Yetkilendirme için kullanmak istediğiniz kaynak, örneğin,`https://management.core.windows.net/` |
| **İstemci Kimliği** | `clientId` | Evet | <*istemci kimliği*> | Yetkilendirme isteyen uygulamanın istemci kimliği |
| **Kimlik Bilgisi Türü** | `credentialType` | Evet | Sertifika <br>or <br>Gizli dizi | İstemcinin yetkilendirme istemek için kullandığı kimlik bilgisi türü. Bu özellik ve değer mantık uygulamanızın temel tanımında görünmez, ancak seçili kimlik bilgisi türü için görünen özellikleri belirler. |
| **Gizli dizi** | `secret` | Evet, ama sadece "Gizli" kimlik bilgisi türü için | <*istemci-gizli*> | Yetkilendirme istemek için istemci sırrı |
| **Pfx** | `pfx` | Evet, ancak yalnızca "Sertifika" kimlik bilgisi türü için | <*kodlanmış-pfx-dosya-içerik*> | Kişisel Bilgi Alışverişi (PFX) dosyasından temel 64 kodlanmış içerik |
| **Parola** | `password` | Evet, ancak yalnızca "Sertifika" kimlik bilgisi türü için | <*pfx dosyası için şifre*> | PFX dosyasına erişmek için parola |
|||||

Örneğin, [dağıtımı otomatikleştirmek için bir Azure Kaynak Yöneticisi şablonunda,](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)hassas bilgileri işlemek ve korumak için [güvenli parametreleri](#secure-action-parameters) kullandığınızda, bu parametre değerlerine çalışma zamanında erişmek için ifadeleri kullanabilirsiniz. Bu örnek HTTP eylem tanımı, `type` kimlik `ActiveDirectoryOAuth`doğrulamasını kimlik bilgisi `Secret`türü olarak belirtir ve parametre değerlerini almak için [parametreler() işlevini](../logic-apps/workflow-definition-language-functions-reference.md#parameters) kullanır:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "ActiveDirectoryOAuth",
         "tenant": "@parameters('tenantIdParam')",
         "audience": "https://management.core.windows.net/",
         "clientId": "@parameters('clientIdParam')",
         "credentialType": "Secret",
         "secret": "@parameters('secretParam')"
     }
   },
   "runAfter": {}
}
```

<a name="raw-authentication"></a>

### <a name="raw-authentication"></a>Ham kimlik doğrulama

**Raw** seçeneği varsa, [OAuth 2.0 protokolünü](https://oauth.net/2/)izlemeyen [kimlik doğrulama şemalarını](https://iana.org/assignments/http-authschemes/http-authschemes.xhtml) kullanmanız gerektiğinde bu kimlik doğrulama türünü kullanabilirsiniz. Bu türle, giden istekle gönderdiğiniz yetkilendirme üstbilgi değerini el ile oluşturur ve tetikleyicinizde veya eyleminizde üstbilgi değerini belirtirsiniz.

Örneğin, [OAuth 1.0 protokolünü](https://tools.ietf.org/html/rfc5849)izleyen bir HTTPS isteği için örnek üstbilgi aşağıda verilmiştir:

```text
Authorization: OAuth realm="Photos",
   oauth_consumer_key="dpf43f3p2l4k3l03",
   oauth_signature_method="HMAC-SHA1",
   oauth_timestamp="137131200",
   oauth_nonce="wIjqoS",
   oauth_callback="http%3A%2F%2Fprinter.example.com%2Fready",
   oauth_signature="74KNZJeDHnMBp0EMJ9ZHt%2FXKycU%3D"
```

Ham kimlik doğrulamasını destekleyen tetikleyici veya eylemde şu özellik değerlerini belirtin:

| Özellik (tasarımcı) | Emlak (JSON) | Gerekli | Değer | Açıklama |
|---------------------|-----------------|----------|-------|-------------|
| **Kimlik Doğrulaması** | `type` | Evet | Ham | Kullanılacak kimlik doğrulama türü |
| **Değer** | `value` | Evet | <*yetkilendirme-üstbilgi-değer*> | Kimlik doğrulama için kullanılacak yetkilendirme üstbilgi değeri |
||||||

Örneğin, [dağıtımı otomatikleştirmek için bir Azure Kaynak Yöneticisi şablonunda,](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)hassas bilgileri işlemek ve korumak için [güvenli parametreleri](#secure-action-parameters) kullandığınızda, bu parametre değerlerine çalışma zamanında erişmek için ifadeleri kullanabilirsiniz. Bu örnek HTTP eylem tanımı, `type` kimlik `Raw`doğrulamasını , olarak belirtir ve parametre değerlerini almak için [parametreler() işlevini](../logic-apps/workflow-definition-language-functions-reference.md#parameters) kullanır:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "Raw",
         "value": "@parameters('authHeaderParam')"
      }
   },
   "runAfter": {}
}
```

<a name="managed-identity-authentication"></a>

### <a name="managed-identity-authentication"></a>Yönetilen kimlik doğrulaması

Yönetilen [Kimlik](../active-directory/managed-identities-azure-resources/overview.md) seçeneği varsa, mantık uygulamanız oturum açmadan diğer Azure Etkin Dizin (Azure AD) kiracılarından kaynaklara erişim inkimlik doğrulaması yapmak için sistem tarafından atanan kimliği veya *el* ile oluşturulmuş tek bir kullanıcı tarafından oluşturulan kimliği kullanabilir. Azure bu kimliği sizin için yönetir ve sırları sağlamak veya döndürmek zorunda olmadığınız için kimlik bilgilerinizin güvenliğini sağlamaya yardımcı olur. [Azure AD kimlik doğrulaması için yönetilen kimlikleri destekleyen Azure hizmetleri](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)hakkında daha fazla bilgi edinin.

1. Mantık uygulamanız yönetilen bir kimliği kullanabilmek için Azure [Mantıksal Uygulamaları'nda yönetilen kimlikleri kullanarak Azure kaynaklarına Doğrula erişimi](../logic-apps/create-managed-service-identity.md)ne kadar doğrulayın adımlarını izleyin. Bu adımlar, mantık uygulamanızda yönetilen kimliği etkinleştirin ve bu kimliğin hedef Azure kaynağına erişimini ayarlar.

1. Azure işlevi yönetilen bir kimliği kullanmadan önce, önce [Azure işlevleri için kimlik doğrulamayı etkinleştirin.](../logic-apps/logic-apps-azure-functions.md#enable-authentication-for-azure-functions)

1. Yönetilen kimliği kullanmak istediğiniz tetikleyici veya eylemde şu özellik değerlerini belirtin:

   | Özellik (tasarımcı) | Emlak (JSON) | Gerekli | Değer | Açıklama |
   |---------------------|-----------------|----------|-------|-------------|
   | **Kimlik Doğrulaması** | `type` | Evet | **Yönetilen Kimlik** <br>or <br>`ManagedServiceIdentity` | Kullanılacak kimlik doğrulama türü |
   | **Yönetilen Kimlik** | `identity` | Evet | * **Yönetilen Kimlik Atanan Sistem** <br>or <br>`SystemAssigned` <p><p>* <*kullanıcı tarafından atanan-kimlik-adı*> | Kullanılacak yönetilen kimlik |
   | **Hedef kitle** | `audience` | Evet | <*hedef-kaynak-ID*> | Erişmek istediğiniz hedef kaynağın kaynak kimliği. <p>Örneğin, `https://storage.azure.com/` kimlik doğrulama için erişim belirteçleri tüm depolama hesapları için geçerli kılar. Ancak, belirli bir depolama hesabı gibi `https://fabrikamstorageaccount.blob.core.windows.net` bir kök hizmet URL'si de belirtebilirsiniz. <p>**Not**: **Hedef Kitle** özelliği bazı tetikleyicilerde veya eylemlerde gizlenmiş olabilir. Bu özelliği görünür hale getirmek için tetikleyici veya eylemde **yeni parametre** ekle listesini açın ve **Hedef Kitle'yi**seçin. <p><p>**Önemli**: Bu hedef kaynak kimliğinin, gerekli sondaki kesikler de dahil olmak üzere Azure AD'nin beklediği değerle *tam olarak eşleştiğinden* emin olun. Bu nedenle, tüm Azure Blob Depolama hesaplarının `https://storage.azure.com/` kaynak kimliği, sondalı bir kesik gerektirir. Ancak, belirli bir depolama hesabının kaynak kimliği, sondalı bir kesik gerektirmez. Bu kaynak adlarını bulmak için [Azure AD'yi destekleyen Azure hizmetlerine](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)bakın. |
   |||||

   Örneğin, [dağıtımı otomatikleştirmek için bir Azure Kaynak Yöneticisi şablonunda,](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)hassas bilgileri işlemek ve korumak için [güvenli parametreleri](#secure-action-parameters) kullandığınızda, bu parametre değerlerine çalışma zamanında erişmek için ifadeleri kullanabilirsiniz. Bu örnek HTTP eylem tanımı, `type` kimlik `ManagedServiceIdentity` doğrulamasını olarak belirtir ve parametre değerlerini almak için [parametreler() işlevini](../logic-apps/workflow-definition-language-functions-reference.md#parameters) kullanır:

   ```json
   "HTTP": {
      "type": "Http",
      "inputs": {
         "method": "GET",
         "uri": "@parameters('endpointUrlParam')",
         "authentication": {
            "type": "ManagedServiceIdentity",
            "identity": "SystemAssigned",
            "audience": "https://management.azure.com/"
         },
      },
      "runAfter": {}
   }
   ```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Mantık Uygulamaları için dağıtımı otomatikleştirin](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)  
* [Mantıksal uygulamaları izleme](../logic-apps/monitor-logic-apps-log-analytics.md)  
* [Mantık uygulaması hatalarını ve sorunlarını tanılama](../logic-apps/logic-apps-diagnosing-failures.md)  
* [Mantıksal uygulama dağıtımı otomatikleştirme](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)
