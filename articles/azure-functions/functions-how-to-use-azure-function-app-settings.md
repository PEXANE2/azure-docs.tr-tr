---
title: Azure İşlev Uygulaması ayarlarını yapılandırma | Microsoft Docs
description: Azure işlevi uygulama ayarlarını yapılandırmayı öğrenin.
author: ggailey777
manager: gwallace
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.service: azure-functions
ms.topic: conceptual
ms.date: 03/28/2018
ms.author: glenga
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 27e56c3bcaa08fee25c362cdfed64c61b92873cb
ms.sourcegitcommit: 6d2a147a7e729f05d65ea4735b880c005f62530f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69982651"
---
# <a name="how-to-manage-a-function-app"></a>İşlev uygulamasını yönetme

Azure Işlevlerinde bir işlev uygulaması, bireysel işlevleriniz için yürütme bağlamı sağlar. İşlev uygulaması davranışları, belirli bir işlev uygulaması tarafından barındırılan tüm işlevler için geçerlidir. Bu konuda, Azure portal işlev uygulamalarınızın nasıl yapılandırılacağı ve yönetileceği açıklanmaktadır.

Başlamak için [Azure Portal](https://portal.azure.com) gidin ve Azure hesabınızda oturum açın. Portalın en üstündeki arama çubuğunda, işlev uygulamanızın adını yazın ve uygulamayı listeden seçin. İşlev uygulamanızı seçtikten sonra aşağıdaki sayfayı görürsünüz:

![Azure portal işlev uygulamasına genel bakış](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

Özel bakış sayfasından, özellikle **[uygulama ayarları](#settings)** ve **[platform özellikleri](#platform-features)** ' nde, işlev uygulamanızı yönetmek için ihtiyacınız olan her şeye gidebilirsiniz.

## <a name="settings"></a>Uygulama ayarları

**Uygulama ayarları** sekmesi, işlev uygulamanız tarafından kullanılan ayarları korur.

![Azure portal işlev uygulaması ayarları.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

Bu ayarlar şifreli olarak depolanır ve portalda değerleri görmek için **değerleri göster** ' i seçmeniz gerekir.

Bir ayar eklemek için **Yeni uygulama ayarı** ' nı seçin ve yeni anahtar-değer çiftini ekleyin.

[!INCLUDE [functions-environment-variables](../../includes/functions-environment-variables.md)]

Yerel olarak bir işlev uygulaması geliştirirken, bu değerler yerel. Settings. JSON proje dosyasında tutulur.

## <a name="platform-features"></a>Platform özellikleri

![İşlev uygulaması platform özellikleri sekmesi.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

İşlev uygulamaları ' de çalışır ve Azure App Service platformu tarafından korunur. Bu nedenle, işlev uygulamalarınızın Azure 'un çekirdek Web barındırma platformu özelliklerinin çoğuna erişimi vardır. **Platform özellikleri** sekmesi, işlev uygulamalarınızda kullanabileceğiniz App Service platformunun birçok özelliğine erişirsiniz. 

> [!NOTE]
> Bir işlev uygulaması tüketim barındırma planı üzerinde çalıştırıldığında tüm App Service özellikleri kullanılamaz.

Bu konunun geri kalanı, Işlevler için yararlı olan Azure portal aşağıdaki App Service özelliklerine odaklanmaktadır:

+ [App Service Düzenleyicisi](#editor)
+ [Console](#console)
+ [Gelişmiş araçlar (kudu)](#kudu)
+ [Dağıtım seçenekleri](#deployment)
+ [CORS](#cors)
+ [Kimlik Doğrulaması](#auth)
+ [API tanımı](#swagger)

App Service ayarları ile çalışma hakkında daha fazla bilgi için bkz. [Configure Azure App Service Settings](../app-service/configure-common.md).

### <a name="editor"></a>App Service Düzenleyicisi

| | |
|-|-|
| ![İşlev uygulaması App Service Düzenleyicisi.](./media/functions-how-to-use-azure-function-app-settings/function-app-appsvc-editor.png)  | App Service Düzenleyicisi, JSON yapılandırma dosyalarını ve kod dosyalarını benzer şekilde değiştirmek için kullanabileceğiniz, gelişmiş bir yerleşik düzenleyici düzenleyicidir. Bu seçeneğin belirlenmesi, temel bir düzenleyici ile ayrı bir tarayıcı sekmesi başlatır. Bu, git deposu ile tümleştirmenize, kod çalıştırmanıza ve hata ayıklamanıza ve işlev uygulaması ayarlarını değiştirmenize olanak sağlar. Bu düzenleyici, işlevleriniz için gelişmiş bir geliştirme ortamı sağlar ve varsayılan işlev uygulama dikey penceresi ile karşılaştırılır.    |

![App Service Düzenleyicisi](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

### <a name="console"></a>Konsola

| | |
|-|-|
| ![Azure portal işlev uygulaması konsolu](./media/functions-how-to-use-azure-function-app-settings/function-app-console.png) | Portal içi konsol, komut satırından işlev uygulamanız ile etkileşime geçmeyi tercih ettiğiniz durumlarda ideal bir geliştirici aracıdır. Ortak komutlar dizin ve dosya oluşturma ve gezinmeyi ve toplu iş dosyalarını ve betikleri yürütmeyi içerir. |

![İşlev uygulaması konsolu](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

### <a name="kudu"></a>Gelişmiş araçlar (kudu)

| | |
|-|-|
| ![Azure portal işlev uygulaması kudu](./media/functions-how-to-use-azure-function-app-settings/function-app-advanced-tools.png) | App Service için gelişmiş araçlar (kudu olarak da bilinir), işlev uygulamanızın gelişmiş yönetim özelliklerine erişim sağlar. Kudu 'den sistem bilgilerini, uygulama ayarlarını, ortam değişkenlerini, site uzantılarını, HTTP üstbilgilerini ve sunucu değişkenlerini yönetirsiniz. Ayrıca, işlev uygulamanız için SCM uç noktasına göz atarak **kudu** 'yi de başlatabilirsiniz.`https://<myfunctionapp>.scm.azurewebsites.net/` |

![Kudu yapılandırma](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)


### <a name="a-namedeploymentdeployment-options"></a><a name="deployment">Dağıtım seçenekleri

| | |
|-|-|
| ![Azure portal işlev uygulaması dağıtım seçenekleri](./media/functions-how-to-use-azure-function-app-settings/function-app-deployment-source.png) | İşlevler, yerel makinenizde işlev kodunuzu geliştirmenize olanak sağlar. Sonra yerel işlev uygulama projenizi Azure 'a yükleyebilirsiniz. Geleneksel FTP karşıya yükleme işleminin yanı sıra, Işlevler GitHub, Azure DevOps, Dropbox, Bitbucket ve diğerleri gibi popüler sürekli tümleştirme çözümlerini kullanarak işlev uygulamanızı dağıtmanıza imkan tanır. Daha fazla bilgi için bkz. [Azure işlevleri Için sürekli dağıtım](functions-continuous-deployment.md). FTP veya yerel Git kullanarak el ile yüklemek için, [dağıtım kimlik bilgilerinizi de yapılandırmanız](functions-continuous-deployment.md#credentials)gerekir. |


### <a name="cors"></a>CORS

| | |
|-|-|
| ![Azure portal işlev uygulaması CORS](./media/functions-how-to-use-azure-function-app-settings/function-app-cors.png) | Hizmetlerinizin kötü amaçlı kod yürütmeyi engellemek için, App Service dış kaynaklardaki işlev uygulamalarınıza yapılan çağrıları engeller. İşlevler, işlevlerinizin uzak istekleri kabul edebileceği izin verilen bir "beyaz liste" listesi tanımlamanızı sağlamak için çıkış noktaları arası kaynak paylaşımını (CORS) destekler.  |

![İşlev Uygulaması CORS 'yi yapılandırma](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

### <a name="auth"></a>Yetkilendirmesi

| | |
|-|-|
| ![Azure portal işlev uygulaması kimlik doğrulaması](./media/functions-how-to-use-azure-function-app-settings/function-app-authentication.png) | İşlevler bir HTTP tetikleyicisi kullanırken, önce kimlik doğrulamasından geçmek için çağrılar yapmanız gerekebilir. App Service, Azure Active Directory kimlik doğrulamasını destekler ve Facebook, Microsoft ve Twitter gibi sosyal sağlayıcılarla oturum açın. Belirli kimlik doğrulama sağlayıcılarını yapılandırma hakkında ayrıntılı bilgi için bkz. [Azure App Service kimlik doğrulamasına genel bakış](../app-service/overview-authentication-authorization.md). |

![İşlev uygulaması için kimlik doğrulamasını yapılandırma](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)


### <a name="swagger"></a>API tanımı

| | |
|-|-|
| ![Azure portal işlev uygulaması API Swagger tanımı](./media/functions-how-to-use-azure-function-app-settings/function-app-api-definition.png) | İşlevler, istemcilerin HTTP ile tetiklenen işlevlerinizi daha kolay kullanmasını sağlamak için Swagger 'yi destekler. Swagger ile API tanımları oluşturma hakkında daha fazla bilgi için, [Azure App Service IÇINDE CORS Ile Restuz BIR API](../app-service/app-service-web-tutorial-rest-api.md)'yi ziyaret edin. Aynı zamanda birden çok işlev için tek bir API yüzeyi tanımlamak üzere Işlev proxy 'Leri de kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure işlev proxy'leri çalışma](functions-proxies.md). |

![İşlev Uygulaması API 'sini yapılandırma](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-apidef.png)



## <a name="next-steps"></a>Sonraki adımlar

+ [Azure App Service ayarlarını yapılandırma](../app-service/configure-common.md)
+ [Azure İşlevleri için sürekli dağıtım](functions-continuous-deployment.md)



