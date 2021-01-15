---
title: Azure Işlevlerinde işlev uygulaması ayarlarını yapılandırma
description: Azure Işlevleri 'nde işlev uygulaması ayarlarını yapılandırmayı öğrenin.
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.topic: conceptual
ms.date: 04/13/2020
ms.custom: cc996988-fb4f-47, devx-track-azurecli
ms.openlocfilehash: 70aecc2613fbe21d34e36f9487d7ba383e140bc8
ms.sourcegitcommit: d59abc5bfad604909a107d05c5dc1b9a193214a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98217371"
---
# <a name="manage-your-function-app"></a>İşlev uygulamanızı yönetme 

Azure Işlevlerinde bir işlev uygulaması, bireysel işlevleriniz için yürütme bağlamı sağlar. İşlev uygulaması davranışları, belirli bir işlev uygulaması tarafından barındırılan tüm işlevler için geçerlidir. Bir işlev uygulamasındaki tüm işlevler aynı [dilde](supported-languages.md)olmalıdır. 

Bir işlev uygulamasındaki tek işlevler birlikte dağıtılır ve birlikte ölçeklendirilir. İşlev uygulaması ölçeklenirken, aynı işlev uygulamasındaki tüm işlevler, örnek başına kaynakları paylaşır. 

Bağlantı dizeleri, ortam değişkenleri ve diğer uygulama ayarları her bir işlev uygulaması için ayrı olarak tanımlanır. İşlev uygulamaları arasında paylaşılması gereken tüm veriler, kalıcı bir mağazada dışarıdan depolanmalıdır.

## <a name="get-started-in-the-azure-portal"></a>Azure portalında kullanmaya başlama

1. Başlamak için [Azure Portal] gidin ve Azure hesabınızda oturum açın. Portalın üst kısmındaki arama çubuğunda, işlev uygulamanızın adını girin ve listeden seçin. 

2. Sol bölmedeki **Ayarlar** ' ın altında **yapılandırma**' yı seçin.

    :::image type="content" source="./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png" alt-text="Azure portal işlev uygulamasına genel bakış":::

Özel bakış sayfasından, özellikle **[uygulama ayarları](#settings)** ve **[platform özellikleri](#platform-features)**' nde, işlev uygulamanızı yönetmek için ihtiyacınız olan her şeye gidebilirsiniz.

## <a name="work-with-application-settings"></a><a name="settings"></a>Uygulama ayarlarıyla çalışma

Uygulama ayarları [Azure Portal](functions-how-to-use-azure-function-app-settings.md?tabs=portal#settings) ve [Azure CLI](functions-how-to-use-azure-function-app-settings.md?tabs=azurecli#settings) ve [Azure PowerShell](functions-how-to-use-azure-function-app-settings.md?tabs=powershell#settings)kullanılarak yönetilebilir. Uygulama ayarlarını, [Visual Studio Code](functions-develop-vs-code.md#application-settings-in-azure) ve [Visual Studio](functions-develop-vs.md#function-app-settings)'dan da yönetebilirsiniz. 

Bu ayarlar şifreli olarak depolanır. Daha fazla bilgi için bkz. [uygulama ayarları güvenliği](security-concepts.md#application-settings).

# <a name="portal"></a>[Portal](#tab/portal)

**Uygulama ayarları** sekmesi, işlev uygulamanız tarafından kullanılan ayarları korur. Portalda değerleri görmek için **değerleri göster** ' i seçmelisiniz. Portalda bir ayar eklemek için **Yeni uygulama ayarı** ' nı seçin ve yeni anahtar-değer çiftini ekleyin.

![Azure portal işlev uygulaması ayarları.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

# <a name="azure-cli"></a>[Azure CLI](#tab/azurecli)

[`az functionapp config appsettings list`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-list)Komut, aşağıdaki örnekte olduğu gibi var olan uygulama ayarlarını döndürür:

```azurecli-interactive
az functionapp config appsettings list --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME>
```

[`az functionapp config appsettings set`](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set)Komut bir uygulama ayarı ekler veya güncelleştirir. Aşağıdaki örnek, adında bir anahtar ve değeri olan bir ayar oluşturur `CUSTOM_FUNCTION_APP_SETTING` `12345` :


```azurecli-interactive
az functionapp config appsettings set --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME> \
--settings CUSTOM_FUNCTION_APP_SETTING=12345
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

[`Get-AzFunctionAppSetting`](/powershell/module/az.functions/get-azfunctionappsetting)Cmdlet 'i, aşağıdaki örnekte olduğu gibi var olan uygulama ayarlarını döndürür: 

```azurepowershell-interactive
Get-AzFunctionAppSetting -Name <FUNCTION_APP_NAME> -ResourceGroupName <RESOURCE_GROUP_NAME>
```

[`Update-AzFunctionAppSetting`](/powershell/module/az.functions/update-azfunctionappsetting)Komut bir uygulama ayarı ekler veya güncelleştirir. Aşağıdaki örnek, adında bir anahtar ve değeri olan bir ayar oluşturur `CUSTOM_FUNCTION_APP_SETTING` `12345` :

```azurepowershell-interactive
Update-AzFunctionAppSetting -Name <FUNCTION_APP_NAME> -ResourceGroupName <RESOURCE_GROUP_NAME> -AppSetting @{"CUSTOM_FUNCTION_APP_SETTING" = "12345"}
```

---

### <a name="use-application-settings"></a>Uygulama ayarlarını kullanma

[!INCLUDE [functions-environment-variables](../../includes/functions-environment-variables.md)]

Yerel olarak bir işlev uygulaması geliştirirken, bu değerlerin yerel kopyalarını proje dosyasında local.settings.jskorumanız gerekir. Daha fazla bilgi için bkz. [yerel ayarlar dosyası](functions-run-local.md#local-settings-file).

## <a name="hosting-plan-type"></a>Barındırma planı türü

Bir işlev uygulaması oluşturduğunuzda, uygulamanın çalıştırıldığı App Service bir barındırma planı da oluşturursunuz. Bir planda bir veya daha fazla işlev uygulaması olabilir. İşlevlerinizin işlevselliği, ölçeklendirilmesi ve fiyatları plan türüne bağlıdır. Daha fazla bilgi edinmek için bkz. [Azure işlevleri fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/functions/).

İşlev uygulamanız tarafından kullanılan planın türünü Azure portal veya Azure CLı veya Azure PowerShell API 'Lerini kullanarak belirleyebilirsiniz. 

Aşağıdaki değerler plan türünü gösterir:

| Plan türü | Portal | Azure CLı/PowerShell |
| --- | --- | --- |
| [Tüketim](consumption-plan.md) | **Tüketim** | `Dynamic` |
| [Premium](functions-premium-plan.md) | **Elaun Premium** | `ElasticPremium` |
| [Adanmış (App Service)](dedicated-plan.md) | Türlerini | Türlerini |

# <a name="portal"></a>[Portal](#tab/portal)

İşlev uygulamanız tarafından kullanılan planın türünü öğrenmek için, [Azure Portal](https://portal.azure.com)işlev uygulaması Için **genel bakış** sekmesinde **App Service plan** bölümüne bakın. Fiyatlandırma katmanını görmek için **App Service planının** adını seçin ve ardından sol bölmedeki **Özellikler** ' i seçin.

![Portalda ölçeklendirme planını görüntüleme](./media/functions-scale/function-app-overview-portal.png)

# <a name="azure-cli"></a>[Azure CLI](#tab/azurecli)

Barındırma planı türünü almak için aşağıdaki Azure CLı komutunu çalıştırın:

```azurecli-interactive
functionApp=<FUNCTION_APP_NAME>
resourceGroup=FunctionMonitoringExamples
appServicePlanId=$(az functionapp show --name $functionApp --resource-group $resourceGroup --query appServicePlanId --output tsv)
az appservice plan list --query "[?id=='$appServicePlanId'].sku.tier" --output tsv

```  

Önceki örnekte `<RESOURCE_GROUP>` `<FUNCTION_APP_NAME>` , ve ile ilgili kaynak grubu ve işlev uygulama adları ile değiştirin. 

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

Barındırma planı türünü almak için aşağıdaki Azure PowerShell komutunu çalıştırın:

```azurepowershell-interactive
$FunctionApp = '<FUNCTION_APP_NAME>'
$ResourceGroup = '<RESOURCE_GROUP>'

$PlanID = (Get-AzFunctionApp -ResourceGroupName $ResourceGroup -Name $FunctionApp).AppServicePlan
(Get-AzFunctionAppPlan -Name $PlanID -ResourceGroupName $ResourceGroup).SkuTier
```
Önceki örnekte `<RESOURCE_GROUP>` `<FUNCTION_APP_NAME>` , ve ile ilgili kaynak grubu ve işlev uygulama adları ile değiştirin. 

---


## <a name="platform-features"></a>Platform özellikleri

İşlev uygulamaları içinde çalışır ve, Azure App Service platformu tarafından korunur. Bu nedenle, işlev uygulamalarınızın Azure 'un çekirdek Web barındırma platformu özelliklerinin çoğuna erişimi vardır. Sol bölmede, işlev uygulamalarınızda kullanabileceğiniz App Service platformunun birçok özelliğine erişirsiniz. 

> [!NOTE]
> Bir işlev uygulaması tüketim barındırma planı üzerinde çalıştırıldığında tüm App Service özellikleri kullanılamaz.

Bu makalenin geri kalanı, Işlevler için yararlı olan Azure portal aşağıdaki App Service özelliklerine odaklanmaktadır:

+ [App Service Düzenleyicisi](#editor)
+ [Konsol](#console)
+ [Gelişmiş araçlar (kudu)](#kudu)
+ [Dağıtım seçenekleri](#deployment)
+ [CORS](#cors)
+ [Kimlik Doğrulaması](#auth)

App Service ayarları ile çalışma hakkında daha fazla bilgi için bkz. [Configure Azure App Service Settings](../app-service/configure-common.md).

### <a name="app-service-editor"></a><a name="editor"></a>App Service Düzenleyicisi

![App Service Düzenleyicisi](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

App Service Düzenleyicisi, JSON yapılandırma dosyalarını ve kod dosyalarını benzer şekilde değiştirmek için kullanabileceğiniz, gelişmiş bir yerleşik düzenleyici düzenleyicidir. Bu seçeneğin belirlenmesi, temel bir düzenleyici ile ayrı bir tarayıcı sekmesi başlatır. Bu, git deposu ile tümleştirmenize, kod çalıştırmanıza ve hata ayıklamanıza ve işlev uygulaması ayarlarını değiştirmenize olanak sağlar. Bu düzenleyici, işlevleriniz için yerleşik işlev Düzenleyicisi ile karşılaştırıldığında gelişmiş bir geliştirme ortamı sağlar.  

Yerel bilgisayarınızda işlevlerinizi geliştirmeyi düşünmenizi öneririz. Yerel olarak geliştirip Azure 'da yayımladığınızda, proje dosyalarınız portalda salt okunurdur. Daha fazla bilgi edinmek için bkz. [Azure işlevlerini yerel olarak kod ve test](functions-develop-local.md)etme.

### <a name="console"></a><a name="console"></a>Konsol

![İşlev uygulaması konsolu](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

Portal içi konsol, komut satırından işlev uygulamanız ile etkileşime geçmeyi tercih ettiğiniz durumlarda ideal bir geliştirici aracıdır. Ortak komutlar dizin ve dosya oluşturma ve gezinmeyi ve toplu iş dosyalarını ve betikleri yürütmeyi içerir. 

Yerel olarak geliştirilirken [Azure Functions Core Tools](functions-run-local.md) ve [Azure CLI]kullanmanızı öneririz.

### <a name="advanced-tools-kudu"></a><a name="kudu"></a>Gelişmiş araçlar (kudu)

![Kudu yapılandırma](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)

App Service için gelişmiş araçlar (kudu olarak da bilinir), işlev uygulamanızın gelişmiş yönetim özelliklerine erişim sağlar. Kudu 'den sistem bilgilerini, uygulama ayarlarını, ortam değişkenlerini, site uzantılarını, HTTP üstbilgilerini ve sunucu değişkenlerini yönetirsiniz. Ayrıca, işlev uygulamanız için SCM uç noktasına göz atarak **kudu** 'yi de başlatabilirsiniz. `https://<myfunctionapp>.scm.azurewebsites.net/` 


### <a name="deployment-center"></a><a name="deployment"></a>Dağıtım Merkezi

İşlev kodunuzu geliştirmek ve sürdürmek için bir kaynak denetimi çözümü kullandığınızda, dağıtım merkezi kaynak denetiminden derleme ve dağıtım yapmanızı sağlar. Projeniz, güncelleştirmeler yaptığınızda Azure 'a oluşturulup dağıtılır. Daha fazla bilgi için bkz. [Azure Işlevlerinde dağıtım teknolojileri](functions-deployment-technologies.md).

### <a name="cross-origin-resource-sharing"></a><a name="cors"></a>Çıkış noktaları arası kaynak paylaşma

Modern tarayıcılar istemcide kötü amaçlı kod yürütmeyi engellemek için Web uygulamalarından gelen istekleri ayrı bir etki alanında çalışan kaynaklara engeller. [Çıkış noktaları arası kaynak paylaşımı (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS) , bir `Access-Control-Allow-Origin` üst bilginin, hangi kaynakların işlev uygulamanızda bitiş noktaları çağırabileceği hakkında bilgi tanımlamasına izin verir.

#### <a name="portal"></a>Portal

İşlev uygulamanız için **Izin verilen** kaynaklar listesini yapılandırdığınızda, `Access-Control-Allow-Origin` üst bilgi, işlev uygulamanızdaki HTTP uç noktalarından gelen tüm yanıtlara otomatik olarak eklenir. 

![İşlev uygulamasının CORS listesini yapılandır](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

Joker karakteri ( `*` ) kullanıldığında, diğer tüm etki alanları yok sayılır. 

[`az functionapp cors add`](/cli/azure/functionapp/cors#az-functionapp-cors-add)İzin verilen çıkış noktaları listesine bir etki alanı eklemek için komutunu kullanın. Aşağıdaki örnek contoso.com etki alanını ekler:

```azurecli-interactive
az functionapp cors add --name <FUNCTION_APP_NAME> \
--resource-group <RESOURCE_GROUP_NAME> \
--allowed-origins https://contoso.com
```

[`az functionapp cors show`](/cli/azure/functionapp/cors#az-functionapp-cors-show)Mevcut izin verilen kaynakları listelemek için komutunu kullanın.

### <a name="authentication"></a><a name="auth"></a>Kimlik Doğrulaması

![İşlev uygulaması için kimlik doğrulamasını yapılandırma](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)

İşlevler bir HTTP tetikleyicisi kullanırken, önce kimlik doğrulamasından geçmek için çağrılar yapmanız gerekebilir. App Service, Facebook, Microsoft ve Twitter gibi sosyal sağlayıcılarla Azure Active Directory kimlik doğrulamasını ve oturum açmayı destekler. Belirli kimlik doğrulama sağlayıcılarını yapılandırma hakkında ayrıntılı bilgi için bkz. [Azure App Service kimlik doğrulamasına genel bakış](../app-service/overview-authentication-authorization.md). 


## <a name="next-steps"></a>Sonraki adımlar

+ [Azure App Service ayarlarını yapılandırma](../app-service/configure-common.md)
+ [Azure Işlevleri için sürekli dağıtım](functions-continuous-deployment.md)

[Azure CLI]: /cli/azure/
[Azure portalı]: https://portal.azure.com
