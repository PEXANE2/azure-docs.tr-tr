---
title: Azure'da işlev uygulama ayarlarını yapılandırma
description: Azure işlev uygulama ayarlarını nasıl yapılandırıyarıştıracaklarını öğrenin.
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.topic: conceptual
ms.date: 08/14/2019
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 662a04dbcc39f3fa95b0098eb8fe556b18b3495b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79276952"
---
# <a name="manage-your-function-app"></a>İşlev uygulamanızı yönetme 

Azure İşlevler'de bir işlev uygulaması, bireysel işlevleriniz için yürütme bağlamını sağlar. İşlev uygulaması davranışları, belirli bir işlev uygulaması tarafından barındırılan tüm işlevler için geçerlidir. Bir işlev uygulamasındaki tüm işlevler aynı [dilde](supported-languages.md)olmalıdır. 

Bir işlev uygulamasındaki tek tek işlevler birlikte dağıtılır ve birlikte ölçeklendirilir. İşlev uygulaması ölçeklendikçe, aynı işlev uygulamasındaki tüm işlevler, örnek başına kaynakları paylaşır. 

Bağlantı dizeleri, ortam değişkenleri ve diğer uygulama ayarları her işlev uygulaması için ayrı ayrı tanımlanır. İşlev uygulamaları arasında paylaşılması gereken tüm veriler, kalıcı bir depoda harici olarak depolanmalıdır.

Bu makalede, işlev uygulamalarınızı nasıl yapılandırıştırıp yönetileyişiniz açıklanmaktadır. 

> [!TIP]  
> Birçok yapılandırma seçeneği, [Azure CLI]kullanılarak da yönetilebilir. 

## <a name="get-started-in-the-azure-portal"></a>Azure portalında kullanmaya başlama

Başlangıç olarak Azure [portalına] gidin ve Azure hesabınızda oturum açın. Portalın en üstündeki arama çubuğunda, işlev uygulamanızın adını yazın ve uygulamayı listeden seçin. İşlev uygulamanızı seçtikten sonra aşağıdaki sayfayı görürsünüz:

![Azure portalında işlev uygulamasına genel bakış](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

İşlev uygulamanızı yönetmek için ihtiyacınız olan her şeye genel bakış sayfasından, özellikle **[de Uygulama ayarları](#settings)** ve Platform **[özelliklerine](#platform-features)** gidebilirsiniz.

## <a name="application-settings"></a><a name="settings"></a>Uygulama ayarları

**Uygulama Ayarları** sekmesi, işlev uygulamanız tarafından kullanılan ayarları korur. Bu ayarlar şifreli olarak depolanır ve portaldaki değerleri görmek için **değerleri göster'i** seçmeniz gerekir. Azure CLI'yi kullanarak uygulama ayarlarına da erişebilirsiniz.

### <a name="portal"></a>Portal

Portala ayar eklemek için **Yeni uygulama ayarını** seçin ve yeni anahtar değeri çiftini ekleyin.

![Azure portalındaki işlev uygulaması ayarları.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

### <a name="azure-cli"></a>Azure CLI

Komut, [`az functionapp config appsettings list`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-list) aşağıdaki örnekte olduğu gibi varolan uygulama ayarlarını döndürür:

```azurecli-interactive
az functionapp config appsettings list --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME>
```

Komut [`az functionapp config appsettings set`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) bir uygulama ayarı ekler veya güncelleştirir. Aşağıdaki örnek, adlı `CUSTOM_FUNCTION_APP_SETTING` bir anahtar ve bir `12345`değeri olan bir ayar oluşturur:


```azurecli-interactive
az functionapp config appsettings set --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME> \
--settings CUSTOM_FUNCTION_APP_SETTING=12345
```

### <a name="use-application-settings"></a>Uygulama ayarlarını kullanma

[!INCLUDE [functions-environment-variables](../../includes/functions-environment-variables.md)]

Yerel olarak bir işlev uygulaması geliştirdiğinizde, bu değerlerin yerel kopyalarını local.settings.json project dosyasında tutmanız gerekir. Daha fazla bilgi için [Yerel ayarlar dosyasına](functions-run-local.md#local-settings-file)bakın.

## <a name="platform-features"></a>Platform özellikleri

![Fonksiyon uygulama platformu özellikleri sekmesi.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

İşlev uygulamaları Azure Uygulama Hizmeti platformu tarafından çalıştırılır ve korunur. Bu nedenle, işlev uygulamalarınız Azure'un temel web barındırma platformunun özelliklerinin çoğuna erişebilir. **Platform özellikleri** sekmesi, uygulama uygulamalarınızda kullanabileceğiniz Uygulama Hizmeti platformunun birçok özelliğine eriştiğiniz yerdir. 

> [!NOTE]
> Bir işlev uygulaması Tüketim barındırma planında çalıştığında tüm App Service özellikleri kullanılamaz.

Bu makalenin geri kalanı, Azure portalında Işlevler için yararlı olan aşağıdaki Uygulama Hizmeti özelliklerine odaklanır:

+ [Uygulama Hizmeti editörü](#editor)
+ [Konsol](#console)
+ [Gelişmiş araçlar (Kudu)](#kudu)
+ [Dağıtım seçenekleri](#deployment)
+ [CORS](#cors)
+ [Kimlik doğrulaması](#auth)

Uygulama Hizmeti ayarlarıyla nasıl çalışacağınız hakkında daha fazla bilgi için Azure [Uygulama Hizmeti Ayarlarını Yapılandır'a](../app-service/configure-common.md)bakın.

### <a name="app-service-editor"></a><a name="editor"></a>Uygulama Servis Editörü

![Uygulama Hizmeti düzenleyicisi](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

App Service düzenleyicisi, JSON yapılandırma dosyalarını ve kod dosyalarını değiştirmek için kullanabileceğiniz gelişmiş bir portal içi düzenleyicidir. Bu seçeneği seçmek, temel bir düzenleyiciile ayrı bir tarayıcı sekmesi ni başlatır. Bu, Git deposuyla tümleştirmenize, kodu çalıştırmanıza ve hata ayıklamanıza ve işlev uygulaması ayarlarını değiştirmenize olanak tanır. Bu düzenleyici, yerleşik işlev düzenleyicisiyle karşılaştırıldığında işlevleriniz için gelişmiş bir geliştirme ortamı sağlar.  

Yerel bilgisayarınızda işlevlerinizi geliştirmeyi düşünmenizi öneririz. Yerel olarak geliştirip Azure'da yayımladığınızda, proje dosyalarınız portalda salt okunur. Daha fazla bilgi edinmek için [Kod'a bakın ve Azure İşlevlerini yerel olarak test edin.](functions-develop-local.md)

### <a name="console"></a><a name="console"></a>Konsol

![Fonksiyon uygulama konsolu](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

Portal konsolu, komut satırından işlev uygulamanızla etkileşimde çalışmayı tercih ettiğinizde ideal bir geliştirici aracıdır. Ortak komutlar dizin ve dosya oluşturma ve gezinme yanı sıra toplu dosya ve komut yürütme içerir. 

Yerel olarak geliştirirken, [Azure İşlevler Temel Araçları](functions-run-local.md) ve [Azure CLI'yi]kullanmanızı öneririz.

### <a name="advanced-tools-kudu"></a><a name="kudu"></a>Gelişmiş araçlar (Kudu)

![Kudu'u yapılandır](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)

App Service için gelişmiş araçlar (Kudu olarak da bilinir) işlev uygulamanızın gelişmiş yönetim özelliklerine erişim sağlar. Kudu'dan sistem bilgilerini, uygulama ayarlarını, ortam değişkenlerini, site uzantılarını, HTTP üstbilgilerini ve sunucu değişkenlerini yönetirsiniz. Ayrıca, işlev uygulamanız için SCM bitiş noktasına göz atarak **Kudu'yu** başlatabilirsiniz, örneğin`https://<myfunctionapp>.scm.azurewebsites.net/` 


### <a name="deployment-center"></a><a name="deployment"></a>Dağıtım Merkezi

İşlev kodunuzu geliştirmek ve korumak için bir kaynak denetimi çözümü kullandığınızda, Dağıtım Merkezi kaynak denetiminden oluşturmanıza ve dağıtmanıza olanak tanır. Güncelleştirmeler yaptığınızda projeniz oluşturulur ve Azure'a dağıtılır. Daha fazla bilgi için Azure [İşlevlerinde Dağıtım teknolojilerine](functions-deployment-technologies.md)bakın.

### <a name="cross-origin-resource-sharing"></a><a name="cors"></a>Çıkış noktaları arası kaynak paylaşma

İstemcide kötü amaçlı kod yürütülmesini önlemek için, modern tarayıcılar web uygulamalarından ayrı bir etki alanında çalışan kaynaklara isteklerini engeller. [Başlangıçlar arası kaynak paylaşımı (CORS),](https://developer.mozilla.org/docs/Web/HTTP/CORS) üstbilginin `Access-Control-Allow-Origin` işlev uygulamanızda uç noktaları aramasına hangi kökenlerin izin verildiğini bildirmesine olanak tanır.

#### <a name="portal"></a>Portal

İşlev uygulamanız için **İzin Verilen başlangıçlar** `Access-Control-Allow-Origin` listesini yapılandırdığınızda, üstbilgi işlev uygulamanızdaki HTTP uç noktalarından gelen tüm yanıtlara otomatik olarak eklenir. 

![Fonksiyon uygulamasının CORS listesini yapılandırma](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

Joker karakter (`*`) kullanıldığında, diğer tüm etki alanları yoksayılır. 

İzin [`az functionapp cors add`](/cli/azure/functionapp/cors#az-functionapp-cors-add) verilen başlangıçlar listesine etki alanı eklemek için komutu kullanın. Aşağıdaki örnek, contoso.com etki alanını ekler:

```azurecli-interactive
az functionapp cors add --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME> \
--allowed-origins https://contoso.com
```

Geçerli [`az functionapp cors show`](/cli/azure/functionapp/cors#az-functionapp-cors-show) izin verilen kökenleri listelemek için komutu kullanın.

### <a name="authentication"></a><a name="auth"></a>Kimlik doğrulaması

![İşlev uygulaması için kimlik doğrulamayı yapılandırma](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)

Işlevler bir HTTP tetikleyicisi kullandığında, çağrıların önce kimlik doğrulaması yapılmasını gerektirebilirsiniz. Uygulama Hizmeti, Azure Active Directory kimlik doğrulamasını ve Facebook, Microsoft ve Twitter gibi sosyal sağlayıcılarla oturum açmayı destekler. Belirli kimlik doğrulama sağlayıcılarının yapılandırılmasıyla ilgili ayrıntılar için [Azure Uygulama Hizmeti kimlik doğrulama genel bakışı'na](../app-service/overview-authentication-authorization.md)bakın. 


## <a name="next-steps"></a>Sonraki adımlar

+ [Azure Uygulama Hizmeti Ayarlarını Yapılandırma](../app-service/configure-common.md)
+ [Azure İşlevler için sürekli dağıtım](functions-continuous-deployment.md)

[Azure CLI]: /cli/azure/
[Azure portalında]: https://portal.azure.com
