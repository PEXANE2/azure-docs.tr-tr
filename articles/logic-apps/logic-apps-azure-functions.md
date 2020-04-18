---
title: Azure Mantık Uygulamalarından Azure Fonksiyonları ekleme ve arama
description: Azure Mantıksal Uygulamaları'ndaki otomatik görevlerden ve iş akışlarından Azure Fonksiyonlarınızda özel kod çağırma ve çalıştırma
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 10/01/2019
ms.openlocfilehash: 29713622be90ea280bff3c002be746bf1615718f
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81605912"
---
# <a name="call-azure-functions-from-azure-logic-apps"></a>Azure Logic Apps'tan Azure işlevlerini arayın

Mantıksal uygulamalarınızda belirli bir işi gerçekleştiren kod çalıştırmak istediğinizde, [Azure İşlevlerini](../azure-functions/functions-overview.md)kullanarak kendi işlevinizi oluşturabilirsiniz. Bu hizmet, kod çalıştırmak için tam bir uygulama veya altyapı oluşturmak zorunda kalmamak için Düğüm.js, C# ve F# işlevleri oluşturmanıza yardımcı olur. [Azure işlevlerinin içinden mantık uygulamaları](#call-logic-app)da arayabilirsiniz. Azure İşlevleri bulutta sunucusuz bilgi işlem sağlar ve aşağıdaki örnekler gibi görevleri gerçekleştirmek için yararlıdır:

* Mantık uygulamanızın davranışını Düğüm.js veya C#'daki işlevlerle genişletin.
* Mantık uygulaması iş akışınızda hesaplamalar gerçekleştirin.
* Mantık uygulamalarınızda gelişmiş biçimlendirme veya bilgi işlem alanları uygulayın.

Azure işlevleri oluşturmadan kod parçacıkları çalıştırmak için [satır içinde kod eklemeyi ve çalıştırmayı](../logic-apps/logic-apps-add-run-inline-code.md)öğrenin.

> [!NOTE]
> Mantık Uygulamaları ve Azure İşlevleri arasındaki tümleştirme şu anda Etkin Slotlarla çalışmıyor.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Azure işlevlerinizin yanı sıra Azure işlevleri için bir kapsayıcı olan bir Azure işlevi uygulaması. Bir işlev uygulamanız yoksa, [önce işlev uygulamanızı oluşturun.](../azure-functions/functions-create-first-azure-function.md) Daha sonra, işlevinizi Azure portalında mantık uygulamanızın dışında veya Mantık Uygulaması Tasarımcısı'ndaki [mantık uygulamanızın içinden](#create-function-designer) oluşturabilirsiniz.

* Mantıksal uygulamalarla çalışırken, aynı gereksinimler işlev uygulamaları ve işlevleri için geçerli olup olmadığı mevcut veya yeni dir:

  * İşlev uygulamanız ve mantık uygulamanız aynı Azure aboneliğini kullanmalıdır.

  * Yeni işlev uygulamaları çalışma zamanı yığını olarak .NET veya JavaScript'i kullanmalıdır. Varolan işlev uygulamalarına yeni bir işlev eklediğinizde, C# veya JavaScript'i seçebilirsiniz.

  * Işleviniz **HTTP tetikleyici** şablonunu kullanır.

    HTTP tetikleyici şablonu, mantık `application/json` uygulamanızdan türü olan içeriği kabul edebilir. Mantık uygulamanıza bir Azure işlevi eklediğinizde, Mantık Uygulama Tasarımcısı Azure aboneliğinizde bu şablondan oluşturulan özel işlevleri gösterir.

  * [OpenAPI tanımı](../azure-functions/functions-openapi-definition.md) (eski adıyla [Swagger dosyası](https://swagger.io/)olarak bilinir) tanımlamamadığınız sürece işleviniz özel yollar kullanmaz.

  * Işleviniz için bir OpenAPI tanımınız varsa, Logic Apps Tasarımcısı işlev parametreleri ile yaptığınız iş size daha zengin bir deneyim sunar. Mantık uygulamanız OpenAPI tanımlarına sahip işlevleri bulup erişemeden [önce, aşağıdaki adımları izleyerek işlev uygulamanızı ayarlayın.](#function-swagger)

* Mantık uygulamanızın ilk adımı olarak bir [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts) de dahil olmak üzere işlevi eklemek istediğiniz mantık uygulaması

  İşlevleri çalıştıran eylemler eklemeden önce, mantık uygulamanızın bir tetikleyiciyle başlaması gerekir. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları ve](../logic-apps/logic-apps-overview.md) Quickstart nedir'yi inceleyin: İlk [mantık uygulamanızı oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="function-swagger"></a>

## <a name="find-functions-that-have-openapi-descriptions"></a>OpenAPI açıklamalarıolan işlevleri bulma

Logic Apps Designer'daki işlev parametreleri ile çalışırken daha zengin bir deneyim için, işleviniz için eski adıyla [Swagger dosyası](https://swagger.io/)olarak bilinen [bir OpenAPI tanımı oluşturun.](../azure-functions/functions-openapi-definition.md) Mantık uygulamanızın Swagger açıklamalarına sahip işlevleri bulup kullanabilmesi için işlev uygulamanızı ayarlamak için aşağıdaki adımları izleyin:

1. İşlev uygulamanızın etkin olarak çalıştığından emin olun.

1. İşlev uygulamanızda, tüm kökenlere aşağıdaki adımları izleyerek izin verilebilecek şekilde [Çapraz Başlangıç Kaynak Paylaşımı 'nı (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) ayarlayın:

   1. **İşlev Uygulamaları** listesinden işlev uygulamanızı seçin. Sağ bölmede, **Platform özellikleri** > **CORS'u**seçin.

      !["Platform özellikleri" > "CORS" > fonksiyon uygulamanızı seçin](./media/logic-apps-azure-functions/function-platform-features-cors.png)

   1. **CORS**altında, yıldız işareti**`*`**( ) joker karakter ekleyin, ancak listedeki diğer tüm kökenleri kaldırın ve **Kaydet'i**seçin.

      !["CORS* joker karakter "*" olarak ayarlayın](./media/logic-apps-azure-functions/function-platform-features-cors-origins.png)

## <a name="access-property-values-inside-http-requests"></a>HTTP istekleri içinde özellik değerlerine erişin

Webhook işlevleri HTTP isteklerini giriş olarak kabul edebilir ve bu istekleri diğer işlevlere geçirebilir. Örneğin, Logic Apps [DateTime değerlerini dönüştüren işlevlere](../logic-apps/workflow-definition-language-functions-reference.md)sahip olsa da, bu temel örnek JavaScript işlevi, işleve geçen bir istek nesnesi içindeki bir bilete nasıl erişebileceğinizi ve bu özellik değerinde işlemleri nasıl gerçekleştirebileceğinizi gösterir. Nesnelerin içindeki özelliklere erişmek için, bu örnek [nokta (.) işleci](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/Property_accessors)kullanır:

```javascript
function convertToDateString(request, response){
   var data = request.body;
   response = {
      body: data.date.ToDateString();
   }
}
```

Bu işlevin içinde şu şekilde olur:

1. İşlev bir `data` değişken oluşturur ve `body` nesnenin `request` içindeki nesneyi bu değişkene atar. İşlev nesnenin içindeki nesneye `body` başvurmak için nokta `request` (.) işleci kullanır:

   ```javascript
   var data = request.body;
   ```

1. İşlev artık `date` `data` değişken üzerinden özellik erişebilir ve `ToDateString()` işlevi çağırarak DateTime türünden DateString türüne bu özellik değerini dönüştürebilirsiniz. İşlev ayrıca, işlevin `body` yanıtındaki özellik aracılığıyla sonucu döndürür:

   ```javascript
   body: data.date.ToDateString();
   ```

Azure işlevinizi oluşturduğunuza göre, [mantıksal uygulamalara işlev ekleme](#add-function-logic-app)adımlarını izleyin.

<a name="create-function-designer"></a>

## <a name="create-functions-inside-logic-apps"></a>Mantık uygulamaları içinde işlevler oluşturma

Logic App Designer'daki yerleşik Azure İşlevler eylemini kullanarak doğrudan mantık uygulamanızın iş akışından Azure işlevleri oluşturabilirsiniz, ancak bu yöntemi yalnızca JavaScript'te yazılmış Azure işlevleri için kullanabilirsiniz. Diğer dilleriçin Azure portalındaki Azure İşlevler deneyimi aracılığıyla Azure işlevleri oluşturabilirsiniz. Daha fazla bilgi için bkz. Azure [portalında ilk işlevinizi oluştur.](../azure-functions/functions-create-first-azure-function.md)

Ancak, herhangi bir Azure işlevi oluşturabilmeniz için, işlevleriniz için bir kapsayıcı olan bir Azure işlev uygulamasına zaten sahip olmalısınız. Bir işlev uygulamanız yoksa, önce bu işlev uygulamasını oluşturun. Bkz. [Azure portalında ilk işlevinizi oluşturun.](../azure-functions/functions-create-first-azure-function.md)

1. Azure [portalında](https://portal.azure.com)mantık uygulamanızı Logic App Designer'da açın.

1. İşlevinizi oluşturmak ve eklemek için senaryonuza uygulanan adımı izleyin:

   * Mantık uygulamanızın iş akışındaki son adımda **Yeni adımı**seçin.

   * Mantık uygulamanızın iş akışındaki varolan adımlar arasında farenizi okun üzerinde hareket ettirin, artı (+) işaretini seçin ve ardından **eylem ekle'yi**seçin.

1. Arama kutusuna filtreniz olarak "azure işlevleri" girin. Eylemler listesinden, örneğin bir Azure işlev eylemi **seçin:**

   !["Azure işlevlerini" bulma](./media/logic-apps-azure-functions/find-azure-functions-action.png)

1. İşlev uygulamaları listesinden işlev uygulamanızı seçin. Eylemler listesi açıldıktan sonra şu eylemi seçin: **Yeni İşlev Oluştur**

   ![İşlev uygulamanızı seçin](./media/logic-apps-azure-functions/select-function-app-create-function.png)

1. İşlev tanımı düzenleyicisinde, işlevinizi tanımlayın:

   1. **İşlev adı** kutusunda, işleviniz için bir ad sağlayın.

   1. **Kod** kutusunda, işleviniz çalışmaya devam ettikten sonra mantık uygulamanıza döndürülmesini istediğiniz yanıt ve yük de dahil olmak üzere kodunuzu işlev şablonuna ekleyin. İşiniz bittiğinde **Oluştur**’u seçin.

   Örneğin:

   ![İşlevinizi tanımlayın](./media/logic-apps-azure-functions/add-code-function-definition.png)

   Şablonun kodunda * `context` nesne,* mantık uygulamanızın daha sonraki bir adımda **İstek Gövdesi** alanı üzerinden gönderdiği iletiyi ifade eder. Nesnenin `context` özelliklerine işlevinizin içinden erişmek için şu sözdizimini kullanın:

   `context.body.<property-name>`

   Örneğin, nesnenin `content` `context` içindeki özelliği başvurmak için şu sözdizimini kullanın:

   `context.body.content`

   Şablon kodu, işlevinizin bu değer üzerinde `data` işlem yapabilmesi için parametreden değeri depolayan bir `input` değişken de içerir. JavaScript işlevleri `data` içinde, değişken de `context.body`için bir kısayol.

   > [!NOTE]
   > Buradaki `body` özellik `context` nesne için geçerlidir ve bir eylemin çıktısından gelen **Gövde** belirteciyle aynı değildir ve bu özelliği de işlevinize aktarabilirsiniz.

1. İstek **Gövdesi** kutusunda, javascript nesne gösterimi (JSON) nesnesi olarak biçimlendirilmiş olması gereken işlevinizin girişini sağlayın.

   Bu giriş, mantık uygulamanızın işlevinize gönderdiği *bağlam nesnesi* veya iletisidir. **İstek Gövdesi** alanını tıklattığınızda, önceki adımlardan çıktılar için belirteçleri seçebilmeniz için dinamik içerik listesi görüntülenir. Bu örnek, bağlam yükünün e-posta `content` tetikleyicisinden **Gelen** belirteç değerini içeren bir özellik içerdiğini belirtir.

   !["İstek Gövdesi" örneği - bağlam nesne yükü](./media/logic-apps-azure-functions/function-request-body-example.png)

   Burada, bağlam nesnesi bir dize olarak döküm değil, bu nedenle nesnenin içeriği doğrudan JSON yüküne eklenir. Ancak, bağlam nesnesi bir dize, json nesnesi veya JSON dizisinden geçen bir JSON belirteci değilse, bir hata alırsınız. Bu nedenle, bu örnekte **Alınan Zaman** belirteci yerine kullanıldıysa, çift tırnak işaretleri ekleyerek bağlam nesnesini bir dize olarak atabilirsiniz.

   ![Nesneyi dize olarak dökme](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

1. Kullanılacak yöntem, üstbilgi veya sorgu parametreleri veya kimlik doğrulama gibi diğer ayrıntıları belirtmek için **yeni parametre** ekle listesini açın ve istediğiniz seçenekleri seçin. Kimlik doğrulama için seçenekleriniz seçtiğiniz işleve göre değişir. Bkz. [Azure işlevleri için kimlik doğrulamayı etkinleştir.](#enable-authentication-functions)

<a name="add-function-logic-app"></a>

## <a name="add-existing-functions-to-logic-apps"></a>Varolan işlevleri mantık uygulamalarına ekleme

Mantık uygulamalarınızdan varolan Azure işlevlerini çağırmak için, Logic App Designer'daki diğer eylemler gibi Azure işlevleri ekleyebilirsiniz.

1. Azure [portalında](https://portal.azure.com)mantık uygulamanızı Logic App Designer'da açın.

1. İşlev eklemek istediğiniz adımın altında **Yeni adımı**seçin.

1. Arama **kutusuna bir eylem seçin**altında, filtreniz olarak "azure işlevleri" girin. Eylemler listesinden Azure **işlev** eylemini seç'i seçin.

   !["Azure işlevlerini" bulma](./media/logic-apps-azure-functions/find-azure-functions-action.png)

1. İşlev uygulamaları listesinden işlev uygulamanızı seçin. İşlevler listesi göründükten sonra işlevinizi seçin.

   ![İşlev uygulamanızı ve Azure işlevini seçin](./media/logic-apps-azure-functions/select-function-app-existing-function.png)

   API tanımlarına (Swagger açıklamaları) sahip ve [mantık uygulamanızın bu işlevleri bulup erişebileceği](#function-swagger)şekilde ayarlanmış işlevler için **Swagger eylemlerini**seçebilirsiniz.

   ![İşlev uygulamanızı, "Swagger eylemleri"ni ve Azure işlevinizi seçin](./media/logic-apps-azure-functions/select-function-app-existing-function-swagger.png)

1. İstek **Gövdesi** kutusunda, javascript nesne gösterimi (JSON) nesnesi olarak biçimlendirilmiş olması gereken işlevinizin girişini sağlayın.

   Bu giriş, mantık uygulamanızın işlevinize gönderdiği *bağlam nesnesi* veya iletisidir. **İstek Gövdesi** alanını tıklattığınızda, önceki adımlardan çıktılar için belirteçleri seçebilmeniz için dinamik içerik listesi görüntülenir. Bu örnek, bağlam yükünün e-posta `content` tetikleyicisinden **Gelen** belirteç değerini içeren bir özellik içerdiğini belirtir.

   !["İstek Gövdesi" örneği - bağlam nesne yükü](./media/logic-apps-azure-functions/function-request-body-example.png)

   Burada, bağlam nesnesi bir dize olarak döküm değil, bu nedenle nesnenin içeriği doğrudan JSON yüküne eklenir. Ancak, bağlam nesnesi bir dize, json nesnesi veya JSON dizisinden geçen bir JSON belirteci değilse, bir hata alırsınız. Bu nedenle, bu örnekte **Alınan Zaman** belirteci kullanıldıysa, çift tırnak işaretleri ekleyerek bağlam nesnesini dize olarak atabilirsiniz:

   ![Nesneyi dize olarak dökme](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

1. Kullanılacak yöntem, istek üstbilgileri, sorgu parametreleri veya kimlik doğrulama gibi diğer ayrıntıları belirtmek için **yeni parametre** ekle listesini açın ve istediğiniz seçenekleri seçin. Kimlik doğrulama için seçenekleriniz seçtiğiniz işleve göre değişir. Bkz. [Azure işlevlerinde kimlik doğrulamayı etkinleştir.](#enable-authentication-functions)

<a name="call-logic-app"></a>

## <a name="call-logic-apps-from-azure-functions"></a>Azure işlevlerinden mantık uygulamalarını arama

Bir Azure işlevinin içinden bir mantık uygulamasını tetiklemek istediğinizde, mantık uygulaması çağrılabilir bir bitiş noktası sağlayan bir tetikleyiciyle başlamalıdır. Örneğin, mantık uygulamasını **HTTP**, **İstek**, **Azure Kuyrukları**veya **Olay Izgara tetikleyicisi** ile başlatabilirsiniz. İşlevinizin içinde, tetikleyicinin URL'sine bir HTTP POST isteği gönderin ve bu mantık uygulamasının işlemesini istediğiniz yükü ekleyin. Daha fazla bilgi için [bkz.](../logic-apps/logic-apps-http-endpoint.md)

<a name="enable-authentication-functions"></a>

## <a name="enable-authentication-for-azure-functions"></a>Azure işlevleri için kimlik doğrulamayı etkinleştirme

Diğer Azure Etkin Dizini (Azure AD) kiracılarında oturum açmave kimlik bilgileri veya sırlar sağlamak zorunda kalmadan kaynaklara erişimi doğrulamak için, mantık uygulamanız yönetilen bir [kimliği](../active-directory/managed-identities-azure-resources/overview.md) (eski adıyla Yönetilen Hizmet Kimliği veya MSI) kullanabilir. Azure bu kimliği sizin için yönetir ve sırları sağlamak veya döndürmek zorunda olmadığınız için kimlik bilgilerinizin güvenliğini sağlamaya yardımcı olur. [Azure AD kimlik doğrulaması için yönetilen kimlikleri destekleyen Azure hizmetleri](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)hakkında daha fazla bilgi edinin.

Mantık uygulamanızı sistem tarafından atanan kimliği veya el ile oluşturulmuş kullanıcı tarafından atanmış bir kimliği kullanacak şekilde ayarlarsanız, mantık uygulamanızdaki Azure işlevleri de kimlik doğrulama için aynı kimliği kullanabilir. Mantıksal uygulamalarda Azure işlevleri için kimlik doğrulama desteği hakkında daha fazla bilgi için [bkz.](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)

Yönetilen kimliği işlevinizle ayarlamak ve kullanmak için aşağıdaki adımları izleyin:

1. Mantık uygulamanızda yönetilen kimliği etkinleştirin ve bu kimliğin hedef kaynağa erişimini ayarlayın. Bkz. [Azure Mantık Uygulamaları'nda yönetilen kimlikleri kullanarak Azure kaynaklarına doğru doğrulama erişimi.](../logic-apps/create-managed-service-identity.md)

1. Aşağıdaki adımları izleyerek Azure işlev ve işlev uygulamanızda kimlik doğrulamayı etkinleştirin:

   * [İşlevinizde anonim kimlik doğrulaması ayarlama](#set-authentication-function-app)
   * [İşlev uygulamanızda Azure AD kimlik doğrulaması ayarlama](#set-azure-ad-authentication)

<a name="set-authentication-function-app"></a>

### <a name="set-up-anonymous-authentication-in-your-function"></a>İşlevinizde anonim kimlik doğrulaması ayarlama

Azure işlevinizde mantık uygulamanızın yönetilen kimliğini kullanmak için işlevinizin kimlik doğrulama düzeyini anonim olarak ayarladınız. Aksi takdirde, mantık uygulamanız bir "BadRequest" hatası atar.

1. Azure [portalında](https://portal.azure.com)işlev uygulamanızı bulun ve seçin. Bu adımlar örnek fonksiyon uygulaması olarak "FabrikamFunctionApp"ı kullanır.

1. İşlev uygulama bölmesine **Platform özelliklerini**seçin. **Geliştirme araçları**altında Gelişmiş **araçları (Kudu)** seçin.

   ![Kudu için gelişmiş araçlar açın](./media/logic-apps-azure-functions/open-advanced-tools-kudu.png)

1. Kudu web sitesinin başlık çubuğunda, **Hata Ayıklama Konsolu** menüsünden **CMD'yi**seçin.

   ![Hata ayıklama konsolu menüsünden "CMD" seçeneğini belirleyin](./media/logic-apps-azure-functions/open-debug-console-kudu.png)

1. Bir sonraki sayfa göründükten sonra, klasör listesinden, **wwwroot** > *işlevinizin***wwwroot'unu** > seçin. Bu adımlar örnek işlev olarak "FabrikamAzureFunction" kullanır.

   ![İşlevinizi > "site" > "wwwroot" seçeneğini belirleyin](./media/logic-apps-azure-functions/select-site-wwwroot-function-folder.png)

1. Düzenleme `function.json` için dosyayı açın.

   !["function.json" dosyası için edit'i tıklatın](./media/logic-apps-azure-functions/edit-function-json-file.png)

1. Nesnede, `bindings` özelliğin `authLevel` var olup olmadığını denetleyin. Özellik varsa, özellik değerini ' `anonymous`ye ayarlayın. Aksi takdirde, bu özelliği ekleyin ve değeri ayarlayın.

   !["authLevel" özelliği ekleyin ve "anonim" olarak ayarlayın](./media/logic-apps-azure-functions/set-authentication-level-function-app.png)

1. İşinizi bitirdiğinizde, ayarlarınızı kaydedin ve bir sonraki bölüme devam edin.

<a name="set-azure-ad-authentication"></a>

### <a name="set-up-azure-ad-authentication-for-your-function-app"></a>İşlev uygulamanız için Azure AD kimlik doğrulaması ayarlama

Bu göreve başlamadan önce, daha sonra kullanmak üzere bu değerleri bulun ve bir kenara koyun:

* Mantık uygulamanızı temsil eden sistem tarafından atanan kimlik için oluşturulan nesne kimliği

  * Bu nesne kimliğini oluşturmak için [mantık uygulamanızın sistemle atanmış kimliğini etkinleştirin.](../logic-apps/create-managed-service-identity.md#azure-portal-system-logic-app)

  * Aksi takdirde, bu nesne kimliğini bulmak için Mantık Uygulama Tasarımcısı'nda mantık uygulamanızı açın. Mantık uygulaması menüsünde Ayarlar **altında,** atanan **Kimlik** > **Sistemi'ni**seçin.

* Azure Etkin Dizin (Azure AD)'deki kiracınızın dizin kimliği

  Kiracınızın dizin kimliğini almak için Powershell [`Get-AzureAccount`](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azureaccount) komutunu çalıştırabilirsiniz. Veya Azure portalında aşağıdaki adımları izleyin:

  1. Azure [portalında](https://portal.azure.com)işlev uygulamanızı bulun ve seçin.

  1. Azure AD kiracınızı bulun ve seçin. Bu adımlar örnek kiracı olarak "Fabrikam" kullanın.

  1. Kiracının menüsünde, **Yönet**altında, **Özellikleri**seçin.

  1. Örneğin, kiracınızın dizin kimliğini kopyalayın ve bu kimliği daha sonra kullanmak üzere kaydedin.

     ![Azure AD kiracısının dizin kimliğini bulma ve kopyalama](./media/logic-apps-azure-functions/azure-active-directory-tenant-id.png)

* Erişmek istediğiniz hedef kaynağın kaynak kimliği

  * Bu kaynak adlarını bulmak için [Azure AD'yi destekleyen Azure hizmetlerini](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)inceleyin.

  > [!IMPORTANT]
  > Bu kaynak kimliği, gerekli sondaki kesikler de dahil olmak üzere Azure AD'nin beklediği değerle tam olarak eşleşmelidir.

  Bu kaynak kimliği, sistem tarafından [atanan kimliği kullanmak üzere işlev eyleminizi ayarladığınızda](../logic-apps/create-managed-service-identity.md#authenticate-access-with-identity)daha sonra Hedef **Kitle** özelliğinde kullandığınız değerle aynıdır.

Artık işlev uygulamanız için Azure AD kimlik doğrulaması ayarlamaya hazırsınız.

1. Azure [portalında](https://portal.azure.com)işlev uygulamanızı bulun ve seçin.

1. İşlev uygulama bölmesine **Platform özelliklerini**seçin. **Ağ**altında, **Kimlik Doğrulama / Yetkilendirme**seçin.

   ![Kimlik doğrulama ve yetkilendirme ayarlarını görüntüleme](./media/logic-apps-azure-functions/view-authentication-authorization-settings.png)

1. Uygulama **Hizmeti Kimlik Doğrulama** ayarını **Ayarı'nı A'da**olarak değiştirin. İstek **doğrudoğrulanmıyorsa yapılacak Eylem'den** **Azure Etkin Dizinile Oturum Aç'ı**seçin. **Kimlik Doğrulama Sağlayıcıları**altında Azure Etkin **Dizini'ni**seçin.

   ![Azure AD ile kimlik doğrulamayı açma](./media/logic-apps-azure-functions/turn-on-authentication-azure-active-directory.png)

1. Azure **Etkin Dizin Ayarları** bölmesine aşağıdaki adımları izleyin:

   1. **Yönetim modunu** **Gelişmiş**olarak ayarlayın.

   1. **İstemci Kimliği** özelliğine, mantık uygulamanızın sistemle atanmış kimliği için nesne kimliğini girin.

   1. **İhraçÇı Url** özelliğine `https://sts.windows.net/` URL girin ve Azure AD kiracınızın dizin kimliğini ekle.

      `https://sts.windows.net/<Azure-AD-tenant-directory-ID>`

   1. İzin **Verilen Belirteç Hedef Kitleleri** özelliğine erişmek istediğiniz hedef kaynağın kaynak kimliğini girin.

      Bu kaynak kimliği, sistem tarafından atanan kimliği kullanmak [üzere işlev eyleminizi ayarladığınızda](../logic-apps/create-managed-service-identity.md#authenticate-access-with-identity)daha sonra **Hedef Kitle** özelliğinde kullandığınız değerle aynıdır.

   Bu noktada, sürümünüz bu örneğe benzer:

   ![Azure Etkin Dizin kimlik doğrulama ayarları](./media/logic-apps-azure-functions/azure-active-directory-authentication-settings.png)

1. İşiniz bittiğinde **Tamam**’ı seçin.

1. Mantık Uygulama Tasarımcısı'na dönün ve [yönetilen kimlikle erişimi doğrulamak için aşağıdaki adımları](../logic-apps/create-managed-service-identity.md#authenticate-access-with-identity)izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Logic [Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
