---
title: Web API 'Leri & REST API 'Leri dağıtma ve çağırma Azure Logic Apps
description: Azure Logic Apps içindeki sistem tümleştirme iş akışları için REST API 'Leri & Web API 'Lerini dağıtın ve çağırın
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 05/26/2017
ms.openlocfilehash: d1305be54a22b1460000a357074cbb1f67123bd6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74790761"
---
# <a name="deploy-and-call-custom-apis-from-workflows-in-azure-logic-apps"></a>Azure Logic Apps iş akışlarından özel API 'Leri dağıtma ve çağırma

Mantıksal uygulama iş akışlarında kullanılmak üzere [özel API 'ler](./logic-apps-create-api-app.md) oluşturduktan sonra, bunları çağırabilmeniz Için önce API 'lerinizi dağıtmanız gerekir. API 'lerinizi [Web Apps](../app-service/overview.md)olarak dağıtabilir, ancak API 'lerinizi bulutta ve şirket Içinde API 'ler oluştururken, barındırdığınızda ve kullandığınızda işinizi kolaylaştırmak için [API Apps](../app-service/app-service-web-tutorial-rest-api.md)olarak dağıtmayı düşünebilirsiniz. API 'lerinizin herhangi bir kodunu değiştirmeniz gerekmez; kodunuzu bir API uygulamasına dağıtmanız yeterlidir. API 'lerinizi, yüksek düzeyde ölçeklenebilir ve kolay API barındırma sağlayan bir hizmet olarak platform (PaaS) teklifi olan [Azure App Service](../app-service/overview.md)barındırabilirsiniz.

Bir mantıksal uygulamadaki tüm API 'leri çağırabilseniz de en iyi deneyim için API 'nizin işlemlerini ve parametrelerini açıklayan [Openapı (daha önce Swagger) meta verileri](https://swagger.io/specification/) ekleyin. Bu Openapı dosyası, API 'nizin daha kolay ve Logic Apps ile daha iyi çalışmasına yardımcı olur.

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a>API 'nizi bir Web uygulaması veya API uygulaması olarak dağıtın

Bir mantıksal uygulamadan özel API 'nizi çağırabilmeniz için, API 'nizi bir Web uygulaması veya API uygulaması olarak Azure App Service için dağıtın. Ayrıca, Openapı dosyanızı Logic Apps Tasarımcısı tarafından okunabilir hale getirmek için API tanımı özelliklerini ayarlayın ve Web uygulamanız veya API uygulamanız için [çıkış noktaları arası kaynak paylaşımı 'nı (CORS)](../app-service/overview.md) etkinleştirin.

1. [Azure Portal](https://portal.azure.com)Web UYGULAMANıZı veya API uygulamanızı seçin.

2. Açılan uygulama menüsünde, **API**altında **API tanımı**' nı seçin. Openapı Swagger. JSON dosyanız için **API tanımı konumunu** URL olarak ayarlayın.

   Genellikle, URL şu biçimde görünür:`https://{name}.azurewebsites.net/swagger/docs/v1)`

   ![Özel API 'niz için Openapı dosyasına bağlantı](./media/logic-apps-custom-api-deploy-call/custom-api-swagger-url.png)

3. **API**altında **CORS**' yi seçin. **Izin verilen çıkış noktaları** için CORS ilkesini **' * '** olarak ayarlayın (tümüne izin ver).

   Bu ayar Logic App Designer 'daki isteklere izin verir.

   ![Logic App Designer 'dan özel API 'nize yönelik isteklere izin verme](./media/logic-apps-custom-api-deploy-call/custom-api-cors.png)

Daha fazla bilgi için bkz. [Azure App Service IÇINDE CORS Ile Restuz API barındırma](../app-service/app-service-web-tutorial-rest-api.md).

## <a name="call-your-custom-api-from-logic-app-workflows"></a>Mantıksal uygulama iş akışlarından özel API 'nizi çağırma

API tanımı özelliklerini ve CORS 'yi ayarladıktan sonra, özel API 'nizin Tetikleyicileri ve eylemleri, mantıksal uygulama iş akışınıza dahil etmeniz için kullanılabilir olmalıdır. 

*  Openapı URL 'Lerinin bulunduğu Web sitelerini görüntülemek için, Logic Apps tasarımcısında abonelik Web sitelerinize gidebilirsiniz.

*  Bir Openapı belgesine işaret ederek kullanılabilir eylemleri ve girişleri görüntülemek için [http + Swagger eylemini](../connectors/connectors-native-http-swagger.md)kullanın.

*  Bir Openapı belgesi bulunmayan veya sunan API 'Ler dahil olmak üzere herhangi bir API 'yi çağırmak için, her zaman [http eylemiyle](../connectors/connectors-native-http.md)bir istek oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel bağlayıcıya genel bakış](../logic-apps/custom-connector-overview.md)