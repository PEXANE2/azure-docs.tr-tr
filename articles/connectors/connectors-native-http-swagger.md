---
title: Azure Mantık Uygulamalarından REST uç noktalarına bağlanın
description: Azure Mantık Uygulamalarını kullanarak otomatik görevlerde, işlemlerde ve iş akışlarında REST uç noktalarını izleyin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 11/01/2019
tags: connectors
ms.openlocfilehash: b34fdc36bd0b1ce294a92b2ae8fa5da01568e5a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74787378"
---
# <a name="call-rest-endpoints-by-using-azure-logic-apps"></a>Azure Logic Apps'ı kullanarak REST uç noktalarını arayın

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve yerleşik HTTP + Swagger konektörü ile, mantık uygulamaları oluşturarak herhangi bir REST uç noktasını düzenli olarak bir [Swagger dosyası](https://swagger.io) üzerinden arayan iş akışlarını otomatikleştirebilirsiniz. HTTP + Swagger tetikleyici ve eylem [http tetikleme ve eylem](connectors-native-http.md) aynı çalışır ama ApI yapısı ve Çıkışları Swagger dosyası tarafından açıklanan açığa çıkararak Mantık App Designer daha iyi bir deneyim sağlar. Bir yoklama tetikleyicisi uygulamak [için, mantık uygulamalarından diğer API'leri, hizmetleri ve sistemleri çağırmak için özel API'ler oluştur'da](../logic-apps/logic-apps-create-api-app.md#polling-triggers)açıklanan yoklama desenini izleyin.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Hedef REST bitiş noktasını açıklayan Swagger (OpenAPI değil) dosyasının URL'si

  Genellikle, REST bitiş noktası bağlayıcının çalışması için şu ölçütleri karşılamalıdır:

  * Swagger dosyası, herkesin erişebileceği bir HTTPS URL'de barındırılmalıdır.

  * Swagger dosyası çapraz [kaynak paylaşımı (CORS)](https://docs.microsoft.com/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) etkin olmalıdır.

  Barındırılan olmayan veya güvenlik ve çapraz kaynak gereksinimlerini karşılamayan bir Swagger dosyasına başvurmak için, [Swagger dosyasını Azure depolama hesabındaki bir blob kapsayıcısına yükleyebilir](#host-swagger)ve dosyaya başvuruyapabilmeniz için bu depolama hesabında CORS'ü etkinleştirebilirsiniz.

  Bu konudaki örnekler, Bilişsel Hizmetler hesabı ve [erişim anahtarı](../cognitive-services/cognitive-services-apis-create-account.md)gerektiren Bilişsel Hizmetler Yüz [API'sını](https://docs.microsoft.com/azure/cognitive-services/face/overview)kullanır.

* [Mantık uygulamaları oluşturmak için nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md)temel bilgi . Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

* Hedef bitiş noktasını aramak istediğiniz yerden gelen mantık uygulaması. HTTP + Swagger tetikleyicisi ile başlamak için [boş bir mantık uygulaması oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md) HTTP + Swagger eylemini kullanmak için mantık uygulamanızı istediğiniz tetikleyiciyle başlatın. Bu örnekte ilk adım olarak HTTP + Swagger tetikleyicisi kullanır.

## <a name="add-an-http--swagger-trigger"></a>HTTP + Swagger tetikleyicisi ekleme

Bu yerleşik tetikleyici, REST API'yi açıklayan ve bu dosyanın içeriğini içeren bir yanıtı döndüren bir Swagger dosyasının URL'sine BIR HTTP isteği gönderir.

1. [Azure portalında](https://portal.azure.com)oturum açın. Boş mantık uygulamanızı Logic App Designer'da açın.

1. Tasarımcıda, arama kutusuna filtreniz olarak "swagger" girin. **Tetikleyiciler** listesinden HTTP **+ Swagger** tetikleyicisini seçin.

   ![HTTP + Swagger tetikleyicisi seçin](./media/connectors-native-http-swagger/select-http-swagger-trigger.png)

1. **SWAGGER ENDPOINT URL** kutusuna, Swagger dosyasının URL'sini girin ve **İleri'yi**seçin.

   Bu örnek, [Bilişsel Hizmetler Yüz API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)için Batı ABD bölgesinde bulunan Swagger URL kullanır:

   `https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/export?DocumentFormat=Swagger&ApiName=Face%20API%20-%20V1.0`

   ![Swagger bitiş noktası için URL girin](./media/connectors-native-http-swagger/http-swagger-trigger-parameters.png)

1. Tasarımcı Swagger dosyası tarafından açıklanan işlemleri gösterdiğinde, kullanmak istediğiniz işlemi seçin.

   ![Swagger dosyasındaki işlemler](./media/connectors-native-http-swagger/http-swagger-trigger-operations.png)

1. Bitiş noktası çağrısına eklemek istediğiniz, seçili işlem temel alından değişen tetikleyici parametreleri için değerleri sağlayın. Tetikleyicinin bitiş noktasını aramasını ne sıklıkta istediğinize göre yinelemeyi ayarlayın.

   Bu örnek, adımın daha açıklayıcı bir ada sahip olması için tetikleyiciyi "HTTP + Swagger tetikleyicisi: Yüz - Algılama" olarak yeniden adlandırır.

   ![İşlem ayrıntıları](./media/connectors-native-http-swagger/http-swagger-trigger-operation-details.png)

1. Kullanılabilir diğer parametrelereklemek için **yeni parametre** ekle listesini açın ve istediğiniz parametreleri seçin.

   HTTP + Swagger için kullanılabilir kimlik doğrulama türleri hakkında daha fazla bilgi için [bkz.](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)

1. Tetikleyici ateşlendiğinde çalışan eylemlerle mantık uygulamanızın iş akışını oluşturmaya devam edin.

1. Işiniz bittiğinde, mantık uygulamanızı kaydetmeyi unutmayın. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

## <a name="add-an-http--swagger-action"></a>HTTP + Swagger eylemi ekleme

Bu yerleşik eylem, REST API'yi açıklayan ve bu dosyanın içeriğini içeren bir yanıtı döndüren Swagger dosyasının URL'sine BIR HTTP isteği nde bulunmaz.

1. [Azure portalında](https://portal.azure.com)oturum açın. Mantık uygulamanızı Logic App Designer'da açın.

1. HTTP + Swagger eylemini eklemek istediğiniz adımın altında **Yeni adımı**seçin.

   Adımlar arasında eylem eklemek için işaretçinizin üzerine adımların arasında ilerleyin. Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

1. Tasarımcıda, arama kutusuna filtreniz olarak "swagger" girin. **Eylemler** listesinden HTTP **+ Swagger** eylemini seçin.

    ![HTTP + Swagger eylemini seçin](./media/connectors-native-http-swagger/select-http-swagger-action.png)

1. **SWAGGER ENDPOINT URL** kutusuna, Swagger dosyasının URL'sini girin ve **İleri'yi**seçin.

   Bu örnek, [Bilişsel Hizmetler Yüz API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)için Batı ABD bölgesinde bulunan Swagger URL kullanır:

   `https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/export?DocumentFormat=Swagger&ApiName=Face%20API%20-%20V1.0`

   ![Swagger bitiş noktası için URL girin](./media/connectors-native-http-swagger/http-swagger-action-parameters.png)

1. Tasarımcı Swagger dosyası tarafından açıklanan işlemleri gösterdiğinde, kullanmak istediğiniz işlemi seçin.

   ![Swagger dosyasındaki işlemler](./media/connectors-native-http-swagger/http-swagger-action-operations.png)

1. Bitiş noktası çağrısına eklemek istediğiniz, seçili işlem temel alından değişen eylem parametreleri için değerleri sağlayın.

   Bu örnekte parametre yoktur, ancak eylemin adı daha açıklayıcı bir ada sahip olacak şekilde "HTTP + Swagger action: Face - Identify" olarak yeniden adlandırır.

   ![İşlem ayrıntıları](./media/connectors-native-http-swagger/http-swagger-action-operation-details.png)

1. Kullanılabilir diğer parametrelereklemek için **yeni parametre** ekle listesini açın ve istediğiniz parametreleri seçin.

   HTTP + Swagger için kullanılabilir kimlik doğrulama türleri hakkında daha fazla bilgi için [bkz.](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)

1. Işiniz bittiğinde, mantık uygulamanızı kaydetmeyi unutmayın. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

<a name="host-swagger"></a>

## <a name="host-swagger-in-azure-storage"></a>Azure Depolama'da Host Swagger

Barındırılan olmayan veya güvenlik ve çapraz kaynak gereksinimlerini karşılamayan bir Swagger dosyasına, bu dosyayı bir Azure depolama hesabındaki blob kapsayıcısına yükleyerek ve bu depolama hesabında CORS'i etkinleştirerek başvuruda bulunabilirsiniz. Swagger dosyalarını Oluşturmak, kurmak ve Depolama alanı oluşturmak için aşağıdaki adımları izleyin:

1. [Bir Azure depolama hesabı oluşturun.](../storage/common/storage-create-storage-account.md)

1. Şimdi blob için CORS etkinleştirin. Depolama hesabınızın menüsünde **CORS'i**seçin. **Blob hizmet** sekmesinde, bu değerleri belirtin ve sonra **Kaydet'i**seçin.

   | Özellik | Değer |
   |----------|-------|
   | **İzin verilen çıkış noktaları** | `*` |
   | **İzin verilen yöntemler** | `GET`, `HEAD`, `PUT` |
   | **İzin verilen üst bilgiler** | `*` |
   | **Kullanıma sunulan üst bilgiler** | `*` |
   | **Maksimum yaş** (saniye cinsinden) | `200` |
   |||

   Bu [örnekAzure portalını](https://portal.azure.com)kullansa da, [Azure Depolama Gezgini](https://storageexplorer.com/)gibi bir araç kullanabilir veya bu örnek [PowerShell komut dosyasını](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1)kullanarak bu ayarı otomatik olarak yapılandırabilirsiniz.

1. [Bir blob kapsayıcı oluşturun.](../storage/blobs/storage-quickstart-blobs-portal.md) Kapsayıcının Genel **Bakış** bölmesine **erişim düzeyini değiştir'i**seçin. Genel **erişim düzeyi** listesinden **Blob'u (yalnızca bloblar için anonim okuma erişimi)** seçin ve **Tamam'ı**seçin.

1. [Swagger dosyasını](../storage/blobs/storage-quickstart-blobs-portal.md#upload-a-block-blob)Azure portalı veya [Azure](https://portal.azure.com) Depolama [Gezgini](https://storageexplorer.com/)aracılığıyla blob kapsayıcısına yükleyin.

1. Blob kapsayıcısındaki dosyaya başvurmak için, büyük/küçük harf duyarlı olan bu biçimi izleyen bir HTTPS bağlantısı kullanın:

   `https://<storage-account-name>.blob.core.windows.net/<blob-container-name>/<swagger-file-name>`

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Burada bir HTTP + Swagger tetikleyici veya eylem çıkışları hakkında daha fazla bilgi. HTTP + Swagger araması bu bilgileri döndürür:

| Özellik adı | Tür | Açıklama |
|---------------|------|-------------|
| Üstbilgi | object | İstekteki üstbilgi |
| body | object | JSON nesnesi | İstekten gövde içeriğine sahip nesne |
| durum kodu | int | İstekteki durum kodu |
|||

| Durum kodu | Açıklama |
|-------------|-------------|
| 200 | Tamam |
| 202 | Accepted |
| 400 | Kötü istek |
| 401 | Yetkisiz |
| 403 | Yasak |
| 404 | Bulunamadı |
| 500 | İç sunucu hatası. Bilinmeyen hata oluştu. |
|||

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
