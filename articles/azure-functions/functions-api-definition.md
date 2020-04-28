---
title: Azure İşlerinde API meta verilerini açma
description: Azure İşlevlerini Açmak için openapi desteğine genel bakış
author: alexkarcher-msft
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alkarche
ms.openlocfilehash: cbfd0e36307210851070c22e74acb0a858446ce1
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81866712"
---
# <a name="openapi-20-metadata-support-in-azure-functions-preview"></a>Azure İşlerinde OpenAPI 2.0 meta veri desteği (önizleme)
Azure İşlevleri'ndeki OpenAPI 2.0 (eski adıyla Swagger) meta veri desteği, bir işlev uygulamasının içine OpenAPI 2.0 tanımı yazmak için kullanabileceğiniz bir önizleme özelliğidir. Daha sonra işlev uygulamasını kullanarak bu dosyayı barındırabilirsiniz.

> [!IMPORTANT]
> OpenAPI önizleme özelliği şu anda yalnızca 1.x çalışma zamanında kullanılabilir. 1.x işlev uygulaması oluşturma hakkında bilgi [burada bulunabilir](./functions-versions.md#creating-1x-apps).

[OpenAPI meta verileri,](https://swagger.io/) rest API barındıran bir işlevin çok çeşitli yazılımlar tarafından tüketilmesine olanak tanır. Bu yazılım PowerApps ve Azure [App Service API Apps özelliği](../app-service/overview.md)gibi Microsoft teklifleri içerir, [Postman](https://www.getpostman.com/docs/importing_swagger)gibi üçüncü taraf geliştirici araçları, ve [daha birçok paket.](https://swagger.io/tools/)

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

>[!TIP]
>[Öğreticiye başlamakve](./functions-api-definition-getting-started.md) daha sonra belirli özellikler hakkında daha fazla bilgi edinmek için bu belgeye geri dönmenizi öneririz.

## <a name="enable-openapi-definition-support"></a><a name="enable"></a>OpenAPI tanım desteğini etkinleştirme
İşlev uygulamanızın **Platform özelliklerindeki** **API Tanımı** sayfasındaki tüm OpenAPI ayarlarını yapılandırabilirsiniz.

> [!NOTE]
> İşlev API tanım özelliği şu anda beta çalışma süresi için desteklenmez.

Barındırılan OpenAPI tanımının ve hızlı başlangıç tanımının oluşumunu etkinleştirmek **için, API tanım kaynağını** **İşlev (Önizleme)** olarak ayarlayın. **Harici URL,** işlevinizin başka bir yerde barındırılan bir OpenAPI tanımını kullanmasına olanak tanır.

## <a name="generate-a-swagger-skeleton-from-your-functions-metadata"></a><a name="generate-definition"></a>İşlevinizin meta verilerinden bir Swagger iskeleti oluşturma
Şablon, ilk OpenAPI tanımınızı yazmaya başlamanıza yardımcı olabilir. Tanım şablonu özelliği, HTTP tetikleyici işlevlerinizin her biri için function.json dosyasındaki tüm meta verileri kullanarak seyrek bir OpenAPI tanımı oluşturur. İstek ve yanıt şablonları gibi [OpenAPI belirtiminden](https://swagger.io/specification/)API'niz hakkında daha fazla bilgi doldurmanız gerekir.

Adım adım talimatlar için, [başlangıç öğreticibakın.](./functions-api-definition-getting-started.md)

### <a name="available-templates"></a><a name="templates"></a>Kullanılabilir şablonlar

|Adı| Açıklama |
|:-----|:-----|
|Oluşturulan Tanım|İşlevin varolan meta verilerinden çıkarılabilen maksimum bilgi miktarına sahip bir OpenAPI tanımı.|

### <a name="included-metadata-in-the-generated-definition"></a><a name="quickstart-details"></a>Oluşturulan tanıma meta veriler dahil

Aşağıdaki tablo, oluşturulan Swagger iskeletine eşlenen Azure portal ayarlarını ve function.json'daki ilgili verileri temsil eder.

|Swagger.json|Portal UI|Fonksiyon.json|
|:----|:-----|:-----|
|[Konak](https://swagger.io/specification/#fixed-fields-15)|**İşlev uygulama ayarları** > **Uygulama Hizmeti ayarları** > **Genel Bakış** > **URL**|*Mevcut değil*
|[Yollar](https://swagger.io/specification/#paths-object-29)|**Integrate** > **Seçili HTTP yöntemlerini** tümleştir|Ciltler: Rota
|[Yol Öğesi](https://swagger.io/specification/#path-item-object-32)|**Integrate** > **Rota şablonuna** tümleştir|Ciltler: Yöntemler
|[Güvenlik](https://swagger.io/specification/#security-scheme-object-112)|**Anahtarlar**|*Mevcut değil*|
|operationID*|**Rota + İzin verilen fiiller**|Rota + İzin Verilen Fiiller|

\*Operasyon kimliği yalnızca PowerApps ve Flow ile tümleştirmek için gereklidir.
> [!NOTE]
> X-ms-özet uzantısı Logic Apps, PowerApps ve Flow'da bir görüntü adı sağlar.
>
> Daha fazla bilgi için [PowerApps için Swagger tanımınızı özelleştir'e](https://docs.microsoft.com/connectors/custom-connectors/openapi-extensions)bakın.

## <a name="use-cicd-to-set-an-api-definition"></a><a name="CICD"></a>API tanımı nı ayarlamak için CI/CD'yi kullanma

 KAYNAK denetimini etkinleştirmeden önce portalda API tanımı barındırmayı etkinleştirmeniz gerekir. Aşağıdaki talimatları izleyin:

1. İşlev uygulama ayarlarınızda **API Tanımına (önizleme)** göz atın.
   1. **API tanım kaynağını** **İşlev**olarak ayarlayın.
   1. Daha sonra değiştirmek için bir şablon tanımı oluşturmak için **API tanım şablonu oluştur'u** ve ardından **Kaydet'i** tıklatın.
   1. API tanım URL'nizi ve anahtarınızı not edin.
1. [Sürekli tümleştirme/sürekli dağıtım (CI/CD) ayarlayın.](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment#requirements-for-continuous-deployment)
2. \site\wwwroot\.azurefunctions\swagger\swagger.json adresindekaynak denetiminde swagger.json'u değiştirin.

Şimdi, deponuzdaki swagger.json değişiklikleri, adım 1.c'de belirttiğiniz API tanım URL'sinde ve anahtarında işlev uygulamanız tarafından barındırılır.

## <a name="next-steps"></a>Sonraki adımlar
* [Öğretici başlarken](functions-api-definition-getting-started.md). OpenAPI tanımını iş başında görmek için gözden geçirme mizi deneyin.
* [Azure İşlevler GitHub deposu.](https://github.com/Azure/Azure-Functions/) API tanımı destek önizlemesi hakkında bize geri bildirimde yer almak için Fonksiyonlar deposuna göz atın. Güncelleştirilmiş görmek istediğiniz her şey için Bir GitHub sorunu yapın.
* [Azure Fonksiyonları geliştirici başvurusu.](functions-reference.md) Kodlama işlevleri ve tetikleyicileri ve bağlamaları tanımlama hakkında bilgi edinin.
