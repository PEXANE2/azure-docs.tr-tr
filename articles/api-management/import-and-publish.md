---
title: Öğretici-Azure API Management ilk API 'nizi Içeri aktarma ve yayımlama
description: Bu öğreticide, bir Openapı belirtim API 'sini Azure API Management 'a aktarır ve sonra API 'nizi Azure portal test edersiniz.
author: mikebudzynski
ms.service: api-management
ms.custom: mvc
ms.topic: tutorial
ms.date: 09/30/2020
ms.author: apimpm
ms.openlocfilehash: 9ff64f57e61002101b4e2c560bdcd91863cc461e
ms.sourcegitcommit: d479ad7ae4b6c2c416049cb0e0221ce15470acf6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91626984"
---
# <a name="tutorial-import-and-publish-your-first-api"></a>Öğretici: ilk API 'nizi Içeri aktarma ve yayımlama

Bu öğreticide, JSON biçiminde bir Openapı belirtim arka uç API 'sinin Azure API Management 'e nasıl aktarılacağı gösterilmektedir. Microsoft, bu örnekte kullanılan arka uç API 'sini sağlar ve ' ı Azure 'da barındırır [https://conferenceapi.azurewebsites.net?format=json](https://conferenceapi.azurewebsites.net?format=json) .

Arka uç API 'sini API Management 'e aktardığınızda, API Management API 'niz arka uç API 'SI için bir façlade olur. Façlade 'yi, arka uç API 'sine dokunmadan API Management gereksinimlerinize göre özelleştirebilirsiniz. Daha fazla bilgi için bkz. [API’nizi dönüştürme ve koruma](transform-api.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir API 'YI API Management içeri aktarma
> * Azure portalında API’yi test etme

İçeri aktardıktan sonra, Azure portal API 'yi yönetebilirsiniz.

:::image type="content" source="media/import-and-publish/created-api.png" alt-text="API Management yeni API":::

## <a name="prerequisites"></a>Önkoşullar

- [Azure API Management terminolojisini](api-management-terminology.md)anlayın.
- [Azure API Management örneği oluşturun](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="import-and-publish-a-backend-api"></a>Arka uç API 'sini içeri aktarma ve yayımlama

Bu bölümde, bir Openapı belirtim arka uç API 'sini içeri ve nasıl yayımlayacağınız gösterilmektedir.

1. API Management örneğinizin sol gezinti bölmesinde **API 'ler**' i seçin.
1. **Openapı** kutucuğunu seçin.
1. **Openapı belirtimini oluştur** penceresinde **tam**' ı seçin.
1. Aşağıdaki tablodaki değerleri girin. Ardından, API 'nizi oluşturmak için **Oluştur** ' u seçin.

   **Ayarlar** sekmesine giderek, oluşturma sırasında veya daha sonra API değerlerini ayarlayabilirsiniz.

   :::image type="content" source="media/import-and-publish/create-api.png" alt-text="API Management yeni API":::


   |Ayar|Değer|Açıklama|
   |-------|-----|-----------|
   |**Openapı belirtimi**|*https: \/ /conferenceapi.azurewebsites.net? Format = JSON*|API 'YI uygulayan hizmet. İstekleri bu adrese iletir API Management.|
   |**Görünen ad**|Önceki hizmet URL 'sini girdikten sonra, API Management bu alanı JSON 'a göre doldurur.|[Geliştirici Portalında](api-management-howto-developer-portal.md)görünen ad.|
   |**Ad**|Önceki hizmet URL 'sini girdikten sonra, API Management bu alanı JSON 'a göre doldurur.|API için benzersiz bir ad.|
   |**Açıklama**|Önceki hizmet URL 'sini girdikten sonra, API Management bu alanı JSON 'a göre doldurur.|API 'nin isteğe bağlı bir açıklaması.|
   |**URL düzeni**|**HTTPS**|API 'ye erişebilen protokoller.|
   |**API URL’si soneki**|*conference*|API Management hizmeti için temel URL 'ye eklenen sonek. API Management, API 'Lerini kendi sonekine göre ayırır. bu nedenle, belirli bir yayımcının her API 'SI için sonekin benzersiz olması gerekir.|
   |**Etiketler**| |Arama, gruplama veya filtreleme için API 'Leri düzenlemek için Etiketler.|
   |**Ürünler**|**Sınırsız**|Bir veya daha fazla API 'nin ilişkilendirmesi. Her bir API Management örneği iki örnek ürünle gelir: **Başlangıç** ve **sınırsız**. API 'yi bir ürünle ilişkilendirerek, bu örnekte **sınırsız** bir API yayımlayın.<br/><br/> Bir ürüne çeşitli API 'Ler dahil edebilir ve bunları geliştirici portalı aracılığıyla geliştiricilere sunabilirsiniz. Bu API 'yi başka bir ürüne eklemek için ürün adını yazın veya seçin. API 'yi birden çok ürüne eklemek için bu adımı tekrarlayın. Ayrıca, daha sonra **Ayarlar** sayfasından, ürünlere API 'ler ekleyebilirsiniz.<br/><br/>  Ürünler hakkında daha fazla bilgi için bkz. [ürün oluşturma ve yayımlama](api-management-howto-add-products.md).|
   |**Ağ geçitleri**|**Yönetilen**|API 'YI kullanıma sunan API Gateway 'leri. Bu alan yalnızca **Geliştirici** ve **Premium** katman hizmetlerinde kullanılabilir.<br/><br/>**Yönetilen** , API Management hizmetine yerleştirilmiş ve Azure 'da Microsoft tarafından barındırılan ağ geçidini gösterir. [Şirket içinde barındırılan ağ geçitleri](self-hosted-gateway-overview.md) yalnızca Premium ve geliştirici hizmet katmanlarında kullanılabilir. Bunları şirket içinde veya diğer bulutlarda dağıtabilirsiniz.<br/><br/> Ağ Geçidi seçilmezse, API kullanılamaz ve API istekleriniz başarılı olmayacaktır.|
   |**Bu API'nin sürümü oluşturulsun mu?**|Seç veya seçimi kaldır|Daha fazla bilgi için bkz. [API 'nizin birden fazla sürümünü yayımlama](api-management-get-started-publish-versions.md).|

   > [!NOTE]
   > API 'yi API tüketicilerine yayımlamak için bir ürünle ilişkilendirmeniz gerekir.

2. **Oluştur**’u seçin.

Bir API tanımını içeri aktarma sorunları yaşıyorsanız, [bilinen sorunlar ve kısıtlamalar listesine](api-management-api-import-restrictions.md)bakın.

## <a name="test-the-new-api-in-the-azure-portal"></a>Azure portal yeni API 'YI test etme

İşlemleri görüntülemek ve test etmek için kullanışlı bir yol sağlayan, API işlemlerini doğrudan Azure portal çağırabilirsiniz.

1. API Management örneğinizin sol gezinti bölmesinde, **API**  >  **demo Konferansı API 'si**' ni seçin.
1. **Test** sekmesini seçin ve ardından **gethoparlörler**' i seçin. Sayfa, varsa **sorgu parametrelerini** ve **üst bilgileri**gösterir. **OCP-apim-Subscription-Key** , bu API ile ilişkili abonelik anahtarı için otomatik olarak doldurulur.
1. **Gönder**’i seçin.

   :::image type="content" source="media/import-and-publish/01-import-first-api-01.png" alt-text="API Management yeni API":::

   Arka uç **200 Tamam** ve bazı veriler ile yanıt verir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * İlk API’nizi içeri aktarma
> * Azure portalında API’yi test etme

Bir ürünü oluşturma ve yayımlama hakkında bilgi edinmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Ürün oluşturma ve yayımlama](api-management-howto-add-products.md)
