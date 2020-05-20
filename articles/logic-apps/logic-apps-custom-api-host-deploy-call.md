---
title: REST API 'Leri & özel Web API 'Lerini çağırma
description: REST API 'Leri & kendi Web API 'lerinizi çağırın Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: jonfan, logicappspm
ms.topic: article
ms.date: 05/13/2020
ms.openlocfilehash: 7b4d00e8c0366d10fddafa66db699c1a59fd9ad7
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83659780"
---
# <a name="deploy-and-call-custom-apis-from-workflows-in-azure-logic-apps"></a>Azure Logic Apps iş akışlarından özel API 'Leri dağıtma ve çağırma

Mantıksal uygulama iş akışlarınızda kullanmak üzere [kendi API 'lerinizi](./logic-apps-create-api-app.md) oluşturduktan sonra, bu API 'leri çağırabilmeniz için önce bunları dağıtmanız gerekir. API 'lerinizi [Web Apps](../app-service/overview.md)olarak dağıtabilir, ancak API 'lerinizi bulutta ve şirket Içinde API 'ler oluştururken, barındırdığınızda ve kullandığınızda işinizi kolaylaştırmak için [API Apps](../app-service/app-service-web-tutorial-rest-api.md)olarak dağıtmayı düşünebilirsiniz. API 'lerinizin herhangi bir kodunu değiştirmeniz gerekmez; kodunuzu bir API uygulamasına dağıtmanız yeterlidir. API 'lerinizi, yüksek düzeyde ölçeklenebilir ve kolay API barındırma sağlayan bir hizmet olarak platform (PaaS) teklifi olan [Azure App Service](../app-service/overview.md)barındırabilirsiniz.

Bir mantıksal uygulamadaki tüm API 'leri çağırabilseniz de en iyi deneyim için API 'nizin işlemlerini ve parametrelerini açıklayan [Swagger meta verilerini](https://swagger.io/specification/) ekleyin. Bu Swagger belgesi, API 'nizin daha kolay ve Logic Apps ile daha iyi çalışmasına yardımcı olur.

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a>API 'nizi bir Web uygulaması veya API uygulaması olarak dağıtın

Bir mantıksal uygulamadan özel API 'nizi çağırabilmeniz için, API 'nizi bir Web uygulaması veya API uygulaması olarak Azure App Service için dağıtın. Swagger belgenizi Logic Apps Tasarımcısı tarafından okunabilir hale getirmek için API tanımı özelliklerini ayarlayın ve Web uygulamanız veya API uygulamanız için [çıkış noktaları arası kaynak paylaşımı 'nı (CORS)](../app-service/overview.md) etkinleştirin.

1. [Azure Portal](https://portal.azure.com)Web UYGULAMANıZı veya API uygulamanızı seçin.

2. Açılan uygulama menüsünde, **API**altında **API tanımı**' nı seçin. **API tanımı konumunu** Swagger. JSON dosyanızın URL 'si olarak ayarlayın.

   Genellikle, URL şu biçimde görünür:`https://{name}.azurewebsites.net/swagger/docs/v1)`

   ![Özel API 'niz için Swagger belgesine bağlantı](./media/logic-apps-custom-api-deploy-call/custom-api-swagger-url.png)

3. **API**altında **CORS**' yi seçin. **Izin verilen çıkış noktaları** için CORS ilkesini **' * '** olarak ayarlayın (tümüne izin ver).

   Bu ayar Logic App Designer 'daki isteklere izin verir.

   ![Logic App Designer 'dan özel API 'nize yönelik isteklere izin verme](./media/logic-apps-custom-api-deploy-call/custom-api-cors.png)

Daha fazla bilgi için bkz. [Azure App Service IÇINDE CORS Ile Restuz API barındırma](../app-service/app-service-web-tutorial-rest-api.md).

## <a name="call-your-custom-api-from-logic-app-workflows"></a>Mantıksal uygulama iş akışlarından özel API 'nizi çağırma

API tanımı özelliklerini ve CORS 'yi ayarladıktan sonra, özel API 'nizin Tetikleyicileri ve eylemleri, mantıksal uygulama iş akışınıza dahil etmeniz için kullanılabilir olmalıdır. 

*  Openapı URL 'Lerinin bulunduğu Web sitelerini görüntülemek için, Logic Apps tasarımcısında abonelik Web sitelerinize gidebilirsiniz.

*  Swagger belgesine işaret ederek kullanılabilir eylemleri ve girişleri görüntülemek için [http + Swagger eylemini](../connectors/connectors-native-http-swagger.md)kullanın.

*  Swagger belgesi bulunmayan veya kullanıma sunmaz API 'Ler dahil herhangi bir API 'yi çağırmak için, her zaman [http eylemiyle](../connectors/connectors-native-http.md)bir istek oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel bağlayıcıya genel bakış](../logic-apps/custom-connector-overview.md)
