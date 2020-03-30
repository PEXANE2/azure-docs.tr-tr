---
title: Azure tarafından barındırılan bir API'yi PowerApps ve Microsoft Flow'a dışa aktarma
description: App Service'de barındırılan bir API'yi PowerApps ve Microsoft Flow'a nasıl göstereceğime genel bakış
ms.topic: conceptual
ms.date: 12/15/2017
ms.reviewer: sunayv
ms.openlocfilehash: 632818bf82e41e6be0a96d30cc1c4fa631718a3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74233084"
---
# <a name="exporting-an-azure-hosted-api-to-powerapps-and-microsoft-flow"></a>Azure tarafından barındırılan bir API'yi PowerApps ve Microsoft Flow'a dışa aktarma

[PowerApps,](https://powerapps.microsoft.com/guided-learning/learning-introducing-powerapps/) verilerinize bağlanan ve platformlar arasında çalışan özel iş uygulamaları oluşturmak ve kullanmak için bir hizmettir. [Microsoft Flow,](/learn/modules/get-started-with-flow/index) sık kullanılan uygulamalar ve hizmetler arasındaki iş akışlarını ve iş süreçlerini otomatikleştirmeyi kolaylaştırır. Hem PowerApps hem de Microsoft Flow, Office 365, Dynamics 365, Salesforce ve daha fazlası gibi veri kaynaklarına çeşitli yerleşik bağlayıcılarla birlikte gelir. Bazı durumlarda, uygulama ve akış oluşturucular da kendi kuruluşları tarafından oluşturulmuş veri kaynaklarına ve API'lere bağlanmak ister.

Benzer şekilde, API'lerini bir kuruluş içinde daha geniş bir şekilde ortaya çıkarmak isteyen geliştiriciler, API'lerini uygulama ve akış oluşturucuları için kullanılabilir hale getirebilir. Bu konu, [Azure İşlevleri](../azure-functions/functions-overview.md) veya Azure [Uygulama Hizmeti](../app-service/overview.md)ile oluşturulmuş bir API'yi nasıl dışa aktarabileceğinizi gösterir. İhraç edilen API, PowerApps ve Microsoft Flow'da yerleşik bir konektör gibi kullanılan özel bir *bağlayıcı*haline gelir.

> [!IMPORTANT]
> Bu makalede gösterilen API tanımı işlevi yalnızca Azure İşlevleri çalışma zamanı ve Uygulama Hizmetleri [uygulamalarının 1.x sürümü](functions-versions.md#creating-1x-apps) için desteklenir. İşlevlerin Sürüm 2.x'i, OpenAPI tanımlarını oluşturmak ve korumak için API Yönetimi ile tümleşir. Daha fazla bilgi edinmek için bkz. Azure [API Yönetimi'ne sahip bir işlev için OpenAPI tanımı oluştur.](functions-openapi-definition.md) 

## <a name="create-and-export-an-api-definition"></a>API tanımı oluşturma ve dışa aktarma
API dışa aktarmadan önce, ApI'yi OpenAPI tanımı (eski adıyla [Swagger](https://swagger.io/) dosyası olarak bilinir) kullanarak açıklamanız gerekir. Bu tanım, bir API’de hangi işlemlerin kullanılabildiğinin yanı sıra API için istek ve yanıt verilerinin nasıl yapılandırılması gerektiğiyle ilgili bilgileri içerir. PowerApps ve Microsoft Flow, herhangi bir OpenAPI 2.0 tanımı için özel bağlayıcılar oluşturabilir. Azure İşlevleri ve Azure Uygulama Hizmeti, OpenAPI tanımlarını oluşturmak, barındırmak ve yönetmek için yerleşik destek içerir. Daha fazla bilgi için Azure [Uygulama Hizmeti'nde CORS ile yeniden barındıran bir API Barındır'a](../app-service/app-service-web-tutorial-rest-api.md)bakın.

> [!NOTE]
> PowerApps ve Microsoft Flow UI'de OpenAPI tanımı kullanmadan özel bağlayıcılar da oluşturabilirsiniz. Daha fazla bilgi için özel [bir bağlayıcı (PowerApps) kaydolun ve kullanın](https://powerapps.microsoft.com/tutorials/register-custom-api/) ve özel bir bağlayıcı [(Microsoft Flow) kullanın.](/power-automate/developer/register-custom-api)

API tanımını dışa aktarmak için aşağıdaki adımları izleyin:

1. Azure [portalında](https://portal.azure.com)Azure İşlevlerinize veya başka bir Uygulama Hizmeti uygulamasına gidin.

    Azure İşlevlerini kullanıyorsanız, işlev uygulamanızı seçin, **Platform özelliklerini**seçin ve ardından **API tanımını**yapın.

    ![Azure İşlevler API tanımı](media/app-service-export-api-to-powerapps-and-flow/api-definition-function.png)

    Azure Uygulama Hizmeti kullanıyorsanız, ayarlar listesinden **API tanımını** seçin.

    ![Uygulama Hizmeti API tanımı](media/app-service-export-api-to-powerapps-and-flow/api-definition-app.png)

2. **PowerApps ' a Dışa Aktar + Microsoft Flow** düğmesi kullanılabilir olmalıdır (değilse, önce bir OpenAPI tanımı oluşturmanız gerekir). Dışa aktarma işlemine başlamak için bu düğmeyi tıklatın.

    ![PowerApps + Microsoft Flow düğmesine dışa aktarma](media/app-service-export-api-to-powerapps-and-flow/export-apps-flow.png)

3. **Dışa Aktarma Modunu**Seçin:

    **Express,** Azure portalı içinden özel bağlayıcı oluşturmanıza olanak tanır. PowerApps veya Microsoft Flow'da oturum açmanız ve hedef ortamda bağlayıcılar oluşturma iznine sahip olduğunuzu gerektirir. Bu iki gereksinim karşılanabiliyorsa, bu önerilen yaklaşımdır. Bu modu kullanıyorsanız, aşağıdaki [açık dışa vurylarını kullan'ı](#express) izleyin.

    **El kitabı,** PowerApps veya Microsoft Flow portallarını kullanarak içe aktardığınız API tanımını dışa aktarmanızı sağlar. Bu, azure kullanıcısı ve bağlayıcı oluşturma izni olan kullanıcı farklı kişilerse veya bağlayıcının başka bir Azure kiracısında oluşturulması gerekiyorsa önerilen yaklaşımdır. Bu modu kullanıyorsanız, aşağıdaki [kullanım kılavuzu dışa aktarma](#manual) yönergelerini izleyin.

    ![Dışa aktarma modu](media/app-service-export-api-to-powerapps-and-flow/export-mode.png)

> [!NOTE]
> Özel bağlayıcı API tanımının bir *kopyasını* kullanır, bu nedenle PowerApps ve Microsoft Flow uygulamada ve API tanımında değişiklik yapıp yaptığınızı hemen bilmeyecek. Değişiklik yaparsanız, yeni sürüm için dışa aktarma adımlarını yineleyin.

<a name="express"></a>
## <a name="use-express-export"></a>Ekspres dışa aktarma yı kullanma

**Dışa** aktarmayı Express modunda tamamlamak için aşağıdaki adımları izleyin:

1. Dışa aktarmak istediğiniz PowerApps veya Microsoft Flow kiracısında oturum açmış olduğunuzdan emin olun. 

2. Tabloda belirtilen ayarları kullanın.

    |Ayar|Açıklama|
    |--------|------------|
    |**Ortam**|Özel bağlayıcının kaydedilmesi gereken ortamı seçin. Daha fazla bilgi için bkz. [Ortamlara genel bakış](https://powerapps.microsoft.com/tutorials/environments-overview/).|
    |**Özel API Adı**|PowerApps ve Microsoft Flow oluşturucularının bağlayıcı listelerinde göreceği bir ad girin.|
    |**Güvenlik yapılandırması hazırlama**|Gerekirse, kullanıcılara API'nize erişim izni vermek için gereken güvenlik yapılandırma ayrıntılarını sağlayın. Bu örnekte bir API anahtarı gösterilmektedir. Daha fazla bilgi için aşağıdaki [kimlik doğrulama türünü belirt'e](#auth) bakın.|
 
    ![PowerApps ve Microsoft Flow'a ekspres dışa aktarma](media/app-service-export-api-to-powerapps-and-flow/export-express.png)

3. **Tamam**'a tıklayın. Özel bağlayıcı şimdi oluşturulur ve belirttiğiniz ortama eklenir.

<a name="manual"></a>
## <a name="use-manual-export"></a>El ile dışa aktarma kullanma

**Dışa aktarmayı El ile** modunda tamamlamak için aşağıdaki adımları izleyin:

1. **Dosyayı İndir** ve kaydet'i tıklatın veya kopyala düğmesini ve URL'yi kaydedin. İçe aktarma sırasında indirme dosyasını veya URL'yi kullanırsınız.
 
    ![PowerApps ve Microsoft Flow'a manuel dışa aktarma](media/app-service-export-api-to-powerapps-and-flow/export-manual.png)
 
2. API tanımınız herhangi bir güvenlik tanımı içeriyorsa, bunlar adım #2 olarak çağrılır. Alma sırasında PowerApps ve Microsoft Flow bunları algılar ve güvenlik bilgileri ister. Sonraki bölümde kullanılmak üzere her tanımla ilgili kimlik bilgilerini toplayın. Daha fazla bilgi için aşağıdaki [kimlik doğrulama türünü belirt'e](#auth) bakın.

    ![El ile dışa aktarma güvenliği](media/app-service-export-api-to-powerapps-and-flow/export-manual-security.png)

    Bu örnek, OpenAPI tanımına dahil edilen API anahtar güvenlik tanımını gösterir.

Artık API tanımını dışa aktardığınıza göre, PowerApps ve Microsoft Flow'da özel bir bağlayıcı oluşturmak için bu tanıma aktarAbilirsiniz. Özel bağlayıcılar iki hizmet arasında paylaşılır, bu nedenle tanımı yalnızca bir kez almanız gerekir.

API tanımını PowerApps ve Microsoft Flow'a aktarmak için aşağıdaki adımları izleyin:

1. [powerapps.com](https://web.powerapps.com) veya [flow.microsoft.com](https://flow.microsoft.com) adresine gidin.

2. Sağ üst köşede, vites simgesini tıklatın ve sonra **Özel bağlayıcılar**tıklatın.

   ![Hizmetteki dişli simgesi](media/app-service-export-api-to-powerapps-and-flow/icon-gear.png)

3. **Özel bağlayıcı oluştur'u**tıklatın, ardından **OpenAPI tanımını içe aktar'ı**tıklatın.

   ![Özel bağlayıcı oluşturma](media/app-service-export-api-to-powerapps-and-flow/flow-apps-create-connector.png)

4. Özel bağlayıcı için bir ad girin, ardından dışa aktardığınız OpenAPI tanımına gidin ve **Devam et'i**tıklatın.

   ![OpenAPI tanımını yükle](media/app-service-export-api-to-powerapps-and-flow/flow-apps-upload-definition.png)

4. **Genel** sekmesinde, OpenAPI tanımından gelen bilgileri gözden geçirin.

5. **Güvenlik** sekmesinde, kimlik doğrulama ayrıntıları vermeniz istenirse, kimlik doğrulama türüne uygun değerleri girin. **Devam**’a tıklayın.

    ![Güvenlik sekmesi](media/app-service-export-api-to-powerapps-and-flow/tab-security.png)

    Bu örnek, API anahtar kimlik doğrulaması için gerekli alanları gösterir. Alanlar kimlik doğrulama türüne bağlı olarak değişir.

6. **Tanımlar** sekmesinde, OpenAPI dosyanızda tanımlanan tüm işlemler otomatik olarak doldurulur. Tüm gerekli işlemleriniz tanımlanırsa, bir sonraki adıma geçebilirsiniz. Değilse, burada işlemleri ekleyebilir ve değiştirebilirsiniz.

    ![Tanımlar sekmesi](media/app-service-export-api-to-powerapps-and-flow/tab-definitions.png)

    Bu örnekte , `CalculateCosts`adı verilen bir işlem vardır. **Açıklama**gibi meta verilerin tümü OpenAPI dosyasından gelir.

7. Sayfanın üst kısmında **bağlayıcı oluştur'u** tıklatın.

Artık PowerApps ve Microsoft Flow'daki özel konektöre bağlanabilirsiniz. PowerApps ve Microsoft Flow portallarında bağlayıcıoluşturma hakkında daha fazla bilgi için özel [konektörünüzü (PowerApps) kaydedin](https://powerapps.microsoft.com/tutorials/register-custom-api/#register-your-custom-connector) ve [özel konektörünüzü (Microsoft Flow) kaydedin.](/power-automate/get-started-flow-dev#create-a-custom-connector)

<a name="auth"></a>
## <a name="specify-authentication-type"></a>Kimlik doğrulaması türünü belirtme

PowerApps ve Microsoft Flow, özel bağlayıcılar için kimlik doğrulaması sağlayan kimlik sağlayıcıları koleksiyonunu destekler. API'niz kimlik doğrulaması gerektiriyorsa, aşağıdaki örnekte olduğu gibi OpenAPI belgenizde _güvenlik tanımı_ olarak yakalandığından emin olun:

```json
"securityDefinitions": {
    "AAD": {
    "type": "oauth2",
    "flow": "accessCode",
    "authorizationUrl": "https://login.windows.net/common/oauth2/authorize",
    "scopes": {}
    }
}
``` 
Dışa aktarma sırasında, PowerApps ve Microsoft Flow'un kullanıcıların kimliğini doğrulamasını sağlayan yapılandırma değerleri sağlarsınız.

Bu **bölüm, Express** modunda desteklenen kimlik doğrulama türlerini kapsar: API anahtarı, Azure Etkin Dizini ve Genel OAuth 2.0. PowerApps ve Microsoft Flow ayrıca Dropbox, Facebook ve SalesForce gibi belirli hizmetler için Temel Kimlik Doğrulama ve OAuth 2.0'ı destekler.

### <a name="api-key"></a>API anahtarı
Bir API anahtarı kullanırken, bağlayıcınızın kullanıcılarından bağlantı oluşturduklarında anahtarı sağlamaları istenir. Hangi anahtarın gerekli olduğunu anlamalarına yardımcı olmak için bir API anahtar adı belirtirsiniz. Önceki örnekte, insanların API anahtarı hakkında nerede bilgi alacaklarını bilmeleri için adı `API Key (contact meganb@contoso.com)` kullanırız. Azure İşlevler için anahtar genellikle işlev uygulaması içindeki çeşitli işlevleri kapsayan ana bilgisayar anahtarlarından biridir.

### <a name="azure-active-directory-azure-ad"></a>Azure Active Directory (Azure AD)
Azure AD'yi kullanırken, biri API'nin kendisi, diğeri özel bağlayıcı için olmak üzere iki Azure AD uygulama kaydına ihtiyacınız vardır:

- API kaydını yapılandırmak için [Uygulama Hizmeti Kimlik Doğrulaması/Yetkilendirme](../app-service/configure-authentication-provider-aad.md) özelliğini kullanın.

- Bağlayıcı için kaydı yapılandırmak için Azure [AD uygulaması ekleme'deki](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)adımları izleyin. Kayıt, API'nize ve `https://msmanaged-na.consent.azure-apim.net/redirect`cevap URL'nize yetkive veda etmiş olmalıdır. 

Daha fazla bilgi için [PowerApps](https://powerapps.microsoft.com/tutorials/customapi-azure-resource-manager-tutorial/) ve [Microsoft Flow](https://docs.microsoft.com/connectors/custom-connectors/azure-active-directory-authentication)için Azure AD kaydı örneklerine bakın. Bu örnekler, API olarak Azure Kaynak Yöneticisi'ni kullanır; adımları izlerseniz API'nizi değiştirin.

Aşağıdaki yapılandırma değerleri gereklidir:
- **İstemci Kimliği** - bağlayıcınızın istemci kimliği Azure AD kaydı
- **İstemci sırrı** - bağlayıcınızın istemci sırrı Azure AD kaydı
- **Giriş URL'si** - Azure AD'nin temel URL'si. Azure'da bu genellikle `https://login.windows.net`.
- **Kiracı kimliği** - giriş için kullanılacak kiracının kimliği. Bu "ortak" veya bağlayıcı oluşturulan kiracının kimliği olmalıdır.
- **Kaynak** URL'si - API'niz için Azure REKLAM kaydının kaynak URL'si

> [!IMPORTANT]
> El ile akışın bir parçası olarak API tanımını PowerApps ve Microsoft Flow'a başka biri aktaracaksa, bu lara *bağlayıcı kaydının*istemci kimliğini ve istemci sırrını ve API'nizin kaynak URL'sini sağlamanız gerekir. Bu sırların güvenli bir şekilde yönetildiğinden emin olun. **API'nin güvenlik kimlik bilgilerini paylaşmayın.**

### <a name="generic-oauth-20"></a>Genel OAuth 2.0
Genel OAuth 2.0 kullanırken, herhangi bir OAuth 2.0 sağlayıcısı ile entegre edebilirsiniz. Bu, yerel olarak desteklenmeyen özel sağlayıcılarla çalışmanıza olanak tanır.

Aşağıdaki yapılandırma değerleri gereklidir:
- **İstemci Kimliği** - OAuth 2.0 istemci kimliği
- **Müşteri sırrı** - OAuth 2.0 istemci gizli
- **Yetkilendirme URL'si** - OAuth 2.0 yetkilendirme URL'si
- **Jeton URL** - OAuth 2.0 belirteç URL
- **URL'yi yenile** - OAuth 2.0 yenileme URL'si


