---
title: Azure API Yönetimi'nde ilk API'nizi alma ve yayımlama
description: Bir OpenAPI Belirtim API'sini Azure API Yönetimi'ne nasıl aktarıp API'nizi Azure portalında nasıl test edebilirsiniz öğrenin.
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
ms.date: 02/27/2020
ms.author: apimpm
ms.openlocfilehash: 886063dcf886d79ac960814f20b3789e8e3b6839
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78163528"
---
# <a name="import-and-publish-your-first-api"></a>İlk API’nizi içeri aktarma ve yayımlama 

Bu öğretici, JSON formatında bir OpenAPI Belirtimi arka uç API'sini Azure API Yönetimine nasıl aktarılacağını gösterir. Microsoft arka uç API'sini sağlar [https://conferenceapi.azurewebsites.net?format=json](https://conferenceapi.azurewebsites.net?format=json)ve Azure'da 'da barındırAbilir.

Arka uç API'sini API Yönetimi'ne aktardıktan sonra, API Management API'niz arka uç API'sinin bir ön cephesi haline gelir. Arka uç API'sine dokunmadan API Yönetimi'nde ön cepheyi ihtiyaçlarınıza göre özelleştirebilirsiniz. Daha fazla bilgi için bkz. [API’nizi dönüştürme ve koruma](transform-api.md). 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * API Yönetimine API alma
> * Azure portalında API’yi test etme

![Yeni API](./media/api-management-import-and-publish/created-api.png)

## <a name="prerequisites"></a>Ön koşullar

- [Azure API Yönetimi terminolojisi](api-management-terminology.md)anlayın.
- [Azure API Yönetimi örneği oluşturun.](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="import-and-publish-a-backend-api"></a><a name="create-api"> </a>Arka uç API'si alma ve yayımlama

Bu bölümde, openapi belirtimi arka uç API'si nasıl içe aktarılacağını ve yayımlanılacağını gösterir.
 
1. API Yönetimi örneğinizin sol daki gezintisinde, **API Yönetimi** bölümünden **API'leri** seçin.
1. **OpenAPI** döşemesini seçin ve açılan ekranda **Tam'ı** seçin.
1. **OpenAPI belirtimi ekranından** Oluştur'da, API'nizi oluşturmak için aşağıdaki tablodaki değerleri kullanın.
   
   Formdaki bir alanın yanındaki kırmızı yıldız, alanın gerekli olduğunu gösterir. Oluşturma sırasında veya daha sonra **Ayarlar** sekmesine giderek API değerlerini ayarlayabilirsiniz. 
   
   ![Bir API oluşturma](./media/api-management-import-and-publish/create-api.png)
   
   |Ayar|Değer|Açıklama|
   |-------|-----|-----------|
   |**OpenAPI belirtimi**|*https:\//conferenceapi.azurewebsites.net?format=json*|API'yi uygulayan hizmet. API Management istekleri bu adrese iletir.|
   |**Görünen ad**|Önceki hizmet URL'sini girdikten sonra, API Yönetimi bu alanı JSON'a göre doldurur.|Geliştirici portalında görüntülenen ad.|
   |**Adı**|Önceki hizmet URL'sini girdikten sonra, API Yönetimi bu alanı JSON'a göre doldurur.|API için benzersiz bir ad.|
   |**Açıklama**|Önceki hizmet URL'sini girdikten sonra, API Yönetimi bu alanı JSON'a göre doldurur.|API'nin isteğe bağlı açıklaması.|
   |**URL düzeni**|**HTTPS**|API'ye erişmek için hangi protokoller kullanılabilir.|
   |**API URL’si soneki**|*conference*|API Yönetimi hizmetinin temel URL'sine eklenen sonek. API Yönetimi API'leri soneklerine göre ayırt eder, bu nedenle sonek belirli bir yayımcı için her API için benzersiz olmalıdır.|
   |**Ürünler**|**Sınırsız**|Bir veya daha fazla API'nin birilişkisi. Her API Yönetimi örneği iki örnek ürünle birlikte gelir: **Başlangıç** ve **Sınırsız.** Bu **örnekte,** API'yi Sınırsız bir ürünle ilişkilendirerek bir API yayımlarsınız.<br/>Bir ürüne birkaç API ekleyebilir ve bunları geliştirici portalı aracılığıyla geliştiricilere sunabilirsiniz. Bu API'yi başka bir ürüne eklemek için ürün adını yazın veya seçin. API'yi birden çok ürüne eklemek için bu adımı yineleyin. Ürünlere daha sonra **Ayarlar** sayfasından API'ler de ekleyebilirsiniz.<br/>API’ye erişim elde etmek için geliştiricilerin önce bir ürüne abone olması gerekir. Abone olduklarında, bu üründeki tüm API'ler için iyi olan bir abonelik anahtarı alırlar. <br/>API Yönetimi örneğini oluşturduysanız, zaten bir yöneticisiniz, bu nedenle örnekteki her ürüne abone siniz.|
   |**Etiketler**| |Arama, gruplandırma veya filtreleme için API'leri düzenlemek için etiketler.|
   |**Bu API'nin sürümü oluşturulsun mu?**|Seçin veya seçin|Sürüm hakkında daha fazla bilgi için [bkz.](api-management-get-started-publish-versions.md)|
   
   > [!NOTE]
   > API’yi yayımlamak için bir ürünle ilişkilendirmeniz gerekir. Bunu **Ayarlar** sayfasından yapabilirsiniz.
   
1. **Oluştur'u**seçin.

API tanımını almakta sorun yaşıyorsanız, [bilinen sorunlar ve kısıtlamaların listesine](api-management-api-import-restrictions.md)bakın.

## <a name="test-the-new-api-in-the-azure-portal"></a>Azure portalında yeni API'yi test edin

API işlemlerini, işlemleri görüntülemek ve test etmek için kullanışlı bir yol sağlayan Azure portalından doğrudan arayabilirsiniz.

1. API Yönetimi örneğinizin sol daki navigasyonunda, **API Yönetimi** bölümünden **API'leri** seçin ve ardından **Demo Konferans API'sini**seçin.
1. **Test** sekmesini seçin ve ardından **GetSpeakers'ı**seçin. Sayfa **sorgu parametrelerini** ve varsa **üstbilgilerini**gösterir. **Ocp-Apim-Abonelik-Anahtar,** bu API ile ilişkili abonelik anahtarı için otomatik olarak doldurulur.
1. **Gönder**’i seçin.
   
   ![API haritasını test etme](./media/api-management-import-and-publish/01-import-first-api-01.png)
   
   Arka uç **200 OK** şeklinde ve bazı verilerle yanıt verir.

## <a name="next-steps"></a><a name="next-steps"> </a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * İlk API’nizi içeri aktarma
> * Azure portalında API’yi test etme

Bir ürünü nasıl oluşturup yayınlayacağınızı öğrenmek için bir sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Ürün oluşturma ve yayımlama](api-management-howto-add-products.md)
