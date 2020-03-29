---
title: Azure Logic Apps'tan web API'lerini & REST API'lerini dağıtın ve arayın
description: Azure Mantık Uygulamaları'ndaki sistem tümleştirme iş akışları için web API'lerini & REST API'lerini dağıtın ve arayın
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 05/26/2017
ms.openlocfilehash: d1305be54a22b1460000a357074cbb1f67123bd6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74790761"
---
# <a name="deploy-and-call-custom-apis-from-workflows-in-azure-logic-apps"></a>Azure Logic Apps'taki iş akışlarından özel API'leri dağıtma ve arama

Mantık uygulaması iş akışlarında kullanılmak üzere [özel API'ler oluşturduktan](./logic-apps-create-api-app.md) sonra, bunları aramadan önce API'lerinizi dağıtmanız gerekir. API'lerinizi web [uygulamaları](../app-service/overview.md)olarak dağıtabilirsiniz, ancak [API'lerinizi bulutta](../app-service/app-service-web-tutorial-rest-api.md)ve şirket içinde API'ler oluşturduğunuzda, barındırırken ve tükettiğinizde işinizi kolaylaştıran API uygulamaları olarak dağıtmayı düşünebilirsiniz. API'lerinizdeki herhangi bir kodu değiştirmeniz gerekmesin - kodunuzu bir API uygulamasına dağıtmanız gerekir. API'lerinizi, yüksek ölçeklenebilir ve kolay API barındırma sağlayan hizmet olarak platform (PaaS) sunan [Azure Uygulama Hizmeti'nde](../app-service/overview.md)barındırabilirsiniz.

Bir mantık uygulamasından herhangi bir API'yi arayabilirsiniz, ancak en iyi deneyim için API'nizin işlemlerini ve parametrelerini açıklayan [OpenAPI (daha önce Swagger) meta verilerini](https://swagger.io/specification/) ekleyin. Bu OpenAPI dosyası, API'nizin daha kolay tümleştirmenize ve mantık uygulamalarıyla daha iyi çalışmasına yardımcı olur.

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a>API'nizi web uygulaması veya API uygulaması olarak dağıtma

Bir mantık uygulamasından özel API'nizi aramadan önce, API'nizi web uygulaması veya API uygulaması olarak Azure Uygulama Hizmeti'ne dağıtın. Ayrıca, OpenAPI dosyanızı Logic Apps Designer tarafından okunabilir hale getirmek için API tanım özelliklerini ayarlayın ve web uygulamanız veya API uygulamanız için [başlangıçlar arası kaynak paylaşımını (CORS)](../app-service/overview.md) açın.

1. Azure [portalında](https://portal.azure.com)web uygulamanızı veya API uygulamanızı seçin.

2. API altında açılan uygulama **API**menüsünde **API tanımını**seçin. **API tanım konumunu** OpenAPI swagger.json dosyanız için URL olarak ayarlayın.

   Genellikle, URL bu biçimde görünür:`https://{name}.azurewebsites.net/swagger/docs/v1)`

   ![Özel API'niz için OpenAPI dosyasına bağlantı](./media/logic-apps-custom-api-deploy-call/custom-api-swagger-url.png)

3. **API**altında, **CORS**seçin. **İzin verilen origins** için CORS ilkesini **'*'** olarak ayarlayın (tümüne izin verin).

   Bu ayar, Logic App Designer'dan gelen isteklere izin verir.

   ![Logic App Designer'dan özel API'nize izin istekleri](./media/logic-apps-custom-api-deploy-call/custom-api-cors.png)

Daha fazla bilgi için Azure [Uygulama Hizmeti'nde CORS ile yeniden barındıran bir API Barındır'a](../app-service/app-service-web-tutorial-rest-api.md)bakın.

## <a name="call-your-custom-api-from-logic-app-workflows"></a>Mantık uygulaması iş akışlarından özel API'nizi arayın

API tanım özelliklerini ve CORS'u ayarladıktan sonra, özel API tetikleyicileriniz ve eylemlerinizin mantık uygulaması iş akışınıza eklemeniz için kullanılabilir olması gerekir. 

*  OpenAPI URL'leri olan web sitelerini görüntülemek için Logic Apps Designer'daki abonelik web sitelerinize göz atabilirsiniz.

*  Bir OpenAPI belgesini işaret ederek kullanılabilir eylemleri ve girişleri görüntülemek için [HTTP + Swagger eylemini](../connectors/connectors-native-http-swagger.md)kullanın.

*  OpenAPI belgesi olmayan veya açığa çıkarmayan API'ler de dahil olmak üzere herhangi bir API'yi aramak için, HER ZAMAN [HTTP eylemi](../connectors/connectors-native-http.md)yle bir istek oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel bağlayıcıya genel bakış](../logic-apps/custom-connector-overview.md)