---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/21/2020
ms.author: glenga
ms.openlocfilehash: 380878fedaa2f7ea6160c3c4801c8dece6e1a9ff
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83648907"
---
#### <a name="enable-app-service-authenticationauthorization"></a>Kimlik doğrulama/yetkilendirmeyi App Service etkinleştirme

App Service platformu, istemcilerin kimliğini doğrulamak için Azure Active Directory (AAD) ve çeşitli üçüncü taraf kimlik sağlayıcılarını kullanmanıza olanak sağlar. İşlevleriniz için özel yetkilendirme kuralları uygulamak üzere bu stratejiyi kullanabilir ve işlev kodunuzda kullanıcı bilgileriyle çalışabilirsiniz. Daha fazla bilgi için bkz. [Azure App Service 'Da kimlik doğrulama ve yetkilendirme](../articles/app-service/overview-authentication-authorization.md) ve [istemci kimlikleriyle çalışma](../articles/azure-functions/functions-bindings-http-webhook-trigger.md#working-with-client-identities).

#### <a name="use-azure-api-management-apim-to-authenticate-requests"></a>İsteklerin kimliğini doğrulamak için Azure API Management (APıM) kullanma

APıM, gelen istekler için çeşitli API güvenliği seçenekleri sağlar. Daha fazla bilgi için bkz. [API Management kimlik doğrulama ilkeleri](../articles/api-management/api-management-authentication-policies.md). APıM ile, işlev uygulamanızı yalnızca APıM örneğinizin IP adresinden gelen istekleri kabul edecek şekilde yapılandırabilirsiniz. Daha fazla bilgi için bkz. [IP adresi kısıtlamaları](../articles/azure-functions/ip-addresses.md#ip-address-restrictions).
