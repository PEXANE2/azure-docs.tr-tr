---
title: Azure portal ile Mantıksal Uygulamayı bir API olarak içeri aktarma | Microsoft Docs
description: Bu öğreticide, Mantıksal Uygulamayı bir API olarak içeri aktarmak için API Management’ın (APIM) nasıl kullanılacağı gösterilir.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 08/01/2019
ms.author: apimpm
ms.openlocfilehash: 4077187fe04e3be914a6f7fba84c03df1b79d06a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74108399"
---
# <a name="import-a-logic-app-as-an-api"></a>Mantıksal Uygulamayı API olarak içeri aktarma

Bu makalede, bir Mantık Uygulaması nın API olarak nasıl içe aktarılabildiğini ve içe aktarılan API'yi nasıl sınandırılabildiğinizi gösterir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> -   Mantıksal Uygulamayı API olarak içeri aktarma
> -   Azure portalında API’yi test etme
> -   Geliştirici portalında API’yi test etme

## <a name="prerequisites"></a>Ön koşullar

-   Aşağıdaki hızlı başlatmayı tamamlayın: [Azure API Yönetimi örneği oluşturun](get-started-create-service-instance.md)
-   Aboneliğinizde bir HTTP bitiş noktasını ortaya çıkaran bir Mantık Uygulaması olduğundan emin olun. Daha fazla bilgi [için, HTTP uç noktalarıyla iş akışlarını tetikle](../logic-apps/logic-apps-http-endpoint.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="import-and-publish-a-back-end-api"></a><a name="create-api"> </a>Arka uç API'sini içeri aktarma ve yayımlama

1. **API YÖNETİMİ** bölümünden **API’ler** öğesini seçin.
2. **Yeni API ekleyin** listesinden **Mantıksal Uygulama**’yı seçin.

    ![Mantıksal uygulama](./media/import-logic-app-as-api/logic-app-api.png)

3. Aboneliğinizde HTTP tetikleyicisi olan Mantık Uygulamaları listesini görmek için **Gözat'a** basın. (HTTP tetikleyicisi olmayan Mantık Uygulamaları'nın listede görünmeyeceğini unutmayın.)
4. Uygulamayı seçin. API Yönetimi, seçili uygulamayla ilişkili gösteriyi bulur, onu getirir ve içeri iter.
5. API URL'si soneki ekleyin. Sonek, bu belirli API’yi bu API Management örneğinde tanımlayan bir addır. Bu API Yönetimi örneğinde benzersiz olmalıdır.
6. API’yi bir ürünle ilişkilendirerek yayımlayın. Bu durumda "_Sınırsız_" ürün kullanılır. API’nin yayımlanmasını ve geliştiricilerin kullanımına sunulmasını istiyorsanız API’yi bir ürüne ekleyin. Bunu API oluşturması sırasında yapabilir ya da daha sonra ayarlayabilirsiniz.

    Ürünler bir veya daha fazla API arasındaki ilişkilendirmelerdir. Bir dizi API ekleyebilir ve geliştirici portalı aracılığıyla geliştiricilere sunabilirsiniz. Geliştiricilerin bir API’ye erişebilmesi için önce ürüne abone olması gerekir. Abone olduklarında, ilgili üründeki tüm API’ler için geçerli olan bir abonelik anahtarı edinirler. API Yönetimi örneğini oluşturduysanız, zaten bir yöneticisiniz, bu nedenle varsayılan olarak her ürüne abone siniz.

    Varsayılan olarak, her bir API Management örneği iki örnek ürün ile birlikte gelir:

    - **Başlangıç**
    - **Sınırsız**

7. **Oluştur'u**seçin.

## <a name="test-the-api-in-the-azure-portal"></a>Azure portalında API’yi test etme

İşlemler doğrudan bir API’nin işlemlerini görüntülemek ve test etmek için kullanışlı bir yol sağlayan Azure portalından çağrılabilir.

1. Önceki adımda oluşturduğunuz API’yi seçin.
2. **Test** sekmesine basın.
3. Bir işlem seçin.

    Sayfa, sorgu parametrelerinin ve üst bilgilerin alanlarını görüntüler. Bu API ile ilişkilendirilmiş ürünün abonelik anahtarı için, üst bilgilerden biri "Ocp-Apim-Subscription-Key" üst bilgisidir. API Management örneğini siz oluşturduysanız zaten bir yöneticisinizdir ve anahtar otomatik olarak doldurulur.

4. **Gönder**’e basın.

    Arka uç, **200 OK** ve bazı verilerle yanıt verir.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

>[!NOTE]
>Her Mantıksal Uygulama, **el ile çağırma** işlemine sahiptir. API’nizin birden çok mantıksal uygulamadan oluşmasını istiyorsanız, çakışma olmaması için işlevi yeniden adlandırmanız gerekir.

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>
> [Yayımlanmış bir api'yi dönüştürün ve koruyun](transform-api.md)
