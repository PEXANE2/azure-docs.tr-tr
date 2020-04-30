---
title: Azure API Management ilk API 'nizi içeri aktarma ve yayımlama
description: Openapı belirtim API 'sini Azure API Management içeri aktarmayı ve API 'nizi Azure portal test yapmayı öğrenin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 04/20/2020
ms.author: apimpm
ms.openlocfilehash: 0b5fbb49e2f60f101f16988538af86c2caf550eb
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82202868"
---
# <a name="import-and-publish-your-first-api"></a>İlk API’nizi içeri aktarma ve yayımlama

Bu öğreticide, JSON biçiminde bir Openapı belirtim arka uç API 'sinin Azure API Management 'e nasıl aktarılacağı gösterilmektedir. Microsoft, arka uç API 'sini sağlar ve üzerinde Azure 'da [https://conferenceapi.azurewebsites.net?format=json](https://conferenceapi.azurewebsites.net?format=json)barındırır.

Arka uç API 'sini API Management 'e aktardığınızda, API Management API 'niz arka uç API 'SI için bir façlade olur. Façlade 'yi, arka uç API 'sine dokunmadan API Management gereksinimlerinize göre özelleştirebilirsiniz. Daha fazla bilgi için bkz. [API’nizi dönüştürme ve koruma](transform-api.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir API 'YI API Management içeri aktarma
> * Azure portalında API’yi test etme

![Yeni API](./media/api-management-import-and-publish/created-api.png)

## <a name="prerequisites"></a>Ön koşullar

- [Azure API Management terminolojisini](api-management-terminology.md)anlayın.
- [Azure API Management örneği oluşturun](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="import-and-publish-a-backend-api"></a><a name="create-api"> </a>Arka uç API 'sini içeri aktarma ve yayımlama

Bu bölümde, bir Openapı belirtim arka uç API 'sini içeri ve nasıl yayımlayacağınız gösterilmektedir.

1. API Management örneğinizin sol gezinti bölmesinde **API Management** bölümünden **API 'ler** ' i seçin.
1. **Openapı** kutucuğunu seçin ve ardından açılır ekranda **tam** ' ı seçin.
1. **Openapı belirtimini oluştur** EKRANıNDA, API 'nizi oluşturmak için aşağıdaki tablodaki değerleri kullanın.

   Formdaki bir alanın yanındaki kırmızı bir yıldız alanın gerekli olduğunu gösterir. **Ayarlar** sekmesine giderek, oluşturma sırasında veya daha sonra API değerlerini ayarlayabilirsiniz.

   ![Bir API oluşturma](./media/api-management-import-and-publish/create-api.png)

   |Ayar|Değer|Açıklama|
   |-------|-----|-----------|
   |**Openapı belirtimi**|*https:\//conferenceapi.azurewebsites.net? Format = JSON*|API 'YI uygulayan hizmet. API Management istekleri bu adrese iletir.|
   |**Görünen ad**|Önceki hizmet URL 'sini girdikten sonra, API Management bu alanı JSON 'a göre doldurur.|Geliştirici Portalında görünen ad.|
   |**Adı**|Önceki hizmet URL 'sini girdikten sonra, API Management bu alanı JSON 'a göre doldurur.|API için benzersiz bir ad.|
   |**Açıklama**|Önceki hizmet URL 'sini girdikten sonra, API Management bu alanı JSON 'a göre doldurur.|API 'nin isteğe bağlı bir açıklaması.|
   |**URL düzeni**|**'DIR**|API 'ye erişmek için hangi protokoller kullanılabilir.|
   |**API URL’si soneki**|*conference*|API Management hizmeti için temel URL 'ye eklenen sonek. API Management, API 'Lerini kendi sonekine göre ayırır. bu nedenle, belirli bir yayımcının her API 'SI için sonekin benzersiz olması gerekir.|
   |**Etiketler**| |Arama, gruplama veya filtreleme için API 'Leri düzenlemek için Etiketler.|
   |**Ürünler**|**Sınırsız**|Bir veya daha fazla API 'nin ilişkilendirmesi. Her bir API Management örneği iki örnek ürünle gelir: **Başlangıç** ve **sınırsız**. API 'yi bir ürünle ilişkilendirerek, bu örnekte **sınırsız** bir API yayımlayın.<br/>Bir ürüne çeşitli API 'Ler dahil edebilir ve bunları geliştirici portalı aracılığıyla geliştiricilere sunabilirsiniz. Bu API 'yi başka bir ürüne eklemek için ürün adını yazın veya seçin. API 'yi birden çok ürüne eklemek için bu adımı tekrarlayın. Ayrıca, daha sonra **Ayarlar** sayfasından, ürünlere API 'ler ekleyebilirsiniz.<br/>API’ye erişim elde etmek için geliştiricilerin önce bir ürüne abone olması gerekir. Abone olduklarında, söz konusu üründeki API 'leri için iyi bir abonelik anahtarı alırlar. <br/>API Management örneğini oluşturduysanız, zaten bir yönetici olursunuz, bu nedenle örnekteki her ürüne abone olursunuz.|
   |**Ağ geçitleri**|**Lebilmesi**|API 'YI kullanıma sunan API Gateway 'leri. Bu alan yalnızca **Geliştirici** ve **Premium** katman hizmetlerinde kullanılabilir.<br/>**Managed** gateway, API Management hizmetinde yerleşik olarak bulunan ve Azure 'da Microsoft tarafından barındırılan ağ geçidini gösterir. Diğer ağ geçitleri [kendi kendine barındırılan ağ geçitlerdir](self-hosted-gateway-overview.md) ve yalnızca Premium ve geliştirici hizmeti katmanlarında kullanılabilir. Bunları, şirket içinde veya diğer bulutlarda dağıtabilirsiniz.<br/>Ağ Geçidi seçilmezse, API kullanılamaz ve API istekleriniz başarılı olmayacaktır.|
   |**Bu API'nin sürümü oluşturulsun mu?**|Seç veya seçimi kaldır|Sürüm oluşturma hakkında daha fazla bilgi için bkz. [API 'nizin birden fazla sürümünü yayımlama](api-management-get-started-publish-versions.md).|

   > [!NOTE]
   > API 'yi API tüketicilerine yayımlamak için bir ürünle ilişkilendirmeniz gerekir.

2. **Oluştur**’u seçin.

Bir API tanımını içeri aktarma sorunları yaşıyorsanız, [bilinen sorunlar ve kısıtlamalar listesine](api-management-api-import-restrictions.md)bakın.

## <a name="test-the-new-api-in-the-azure-portal"></a>Azure portal yeni API 'YI test etme

İşlemleri görüntülemek ve test etmek için kullanışlı bir yol sağlayan, API işlemlerini doğrudan Azure portal çağırabilirsiniz.

1. API Management örneğinizin sol gezinti bölmesinde **API Management** bölümünden **API 'ler** ' i seçin ve ardından **Tanıtım Konferansı API 'si**' ni seçin.
1. **Test** sekmesini seçin ve ardından **gethoparlörler**' i seçin. Sayfa, varsa **sorgu parametrelerini** ve **üst bilgileri**gösterir. **OCP-apim-Subscription-Key** , bu API ile ilişkili abonelik anahtarı için otomatik olarak doldurulur.
1. **Gönder**’i seçin.

   ![API haritasını test etme](./media/api-management-import-and-publish/01-import-first-api-01.png)

   Arka uç **200 OK** şeklinde ve bazı verilerle yanıt verir.

## <a name="next-steps"></a><a name="next-steps"> </a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * İlk API’nizi içeri aktarma
> * Azure portalında API’yi test etme

Bir ürünü oluşturma ve yayımlama hakkında bilgi edinmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Ürün oluşturma ve yayımlama](api-management-howto-add-products.md)
