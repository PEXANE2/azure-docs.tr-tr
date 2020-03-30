---
title: Azure API Yönetimi'ndeki grupları kullanarak geliştirici hesaplarını yönetme
titleSuffix: Azure API Management
description: Azure API Yönetimi'ndeki grupları kullanarak geliştirici hesaplarını nasıl yöneteceğimize öğrenin
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 02/13/2018
ms.author: apimpm
ms.openlocfilehash: 182f0ea93ddfb2ab64de1c15b36958537fa5431f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75430719"
---
# <a name="how-to-create-and-use-groups-to-manage-developer-accounts-in-azure-api-management"></a>Azure API Management'ta geliştirici hesaplarını yönetmek için grup oluşturma ve kullanma

API Management’te, ürünlerin geliştiricilere görünürlüğünü yönetmek için gruplar kullanılır. Ürünler önce gruplar tarafından görünür hale getirilir ve daha sonra bu gruplardaki geliştiriciler gruplarla ilişkili ürünleri görüntüleyebilir ve bunlara abone olabilir. 

API Management şu sabit sistem gruplarına sahiptir:

* **Yöneticiler**: Azure aboneliği yöneticileri bu grubun üyesidir. Yöneticiler, geliştiriciler tarafından kullanılan API’leri, işlemleri ve ürünleri oluşturarak API Management hizmet örneklerini yönetir.
* **Geliştiriciler**: Kimliği doğrulanmış geliştirici portalı kullanıcıları bu gruba girer. Geliştiriciler, API'lerinizi kullanarak uygulama oluşturan müşterilerdir. Geliştiriciler, geliştirici portalına erişim iznine sahiptir ve bir API’nin işlemlerini çağıran uygulamalar oluşturur.
* **Konuklar**: Kimliği doğrulanmamış geliştirici portalı kullanıcıları (bir API Management örneğinin geliştirici portalını ziyaret eden olası müşteriler gibi) bu gruba girer. Bunlara API’leri görüntüleyebilme ancak çağıramama gibi bazı salt okunur erişimler verilebilir.

Bu sistem gruplarına ek olarak, yöneticiler özel gruplar oluşturabilir veya [ilişkili Azure Active Directory kiracılarındaki dış grupları kullanabilir][leverage external groups in associated Azure Active Directory tenants]. Özel ve dış gruplar, geliştiricilere görünürlük ve API ürünlerine erişim sağlayan sistem gruplarıyla birlikte kullanılabilir. Örneğin, belirli bir iş ortağı kuruluşla ilişkili geliştiriciler için özel bir grup oluşturabilir ve bunlara yalnızca ilgili API'leri içeren bir üründen API'lere erişim izni verebilirsiniz. Bir kullanıcı birden fazla grubun üyesi olabilir.

Bu kılavuz, bir API Yönetimi örneğinin yöneticilerinin nasıl yeni gruplar ekleyebileceğini ve bunları ürünler ve geliştiricilerle nasıl ilişkilendirebileceğini gösterir.

Yayımcı portalında gruplar oluşturmanın ve yönetmeye ek olarak, API Management REST API [Group](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-group-entity) varlığını kullanarak gruplarınızı oluşturabilir ve yönetebilirsiniz.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki görevleri tamamlayın: [Azure API Yönetimi örneği oluşturun.](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-a-group"></a><a name="create-group"> </a>Grup oluşturma

Bu bölümde, API Yönetimi hesabınıza nasıl yeni bir grup ekleyeceğiniz gösterilmektedir.

1. Ekranın solundaki **Gruplar** sekmesini seçin.
2. **+Ekle'yi**tıklatın.
3. Grup için benzersiz bir ad ve isteğe bağlı bir açıklama girin.
4. **Oluştur**’a basın.

    ![Yeni grup ekleme](./media/api-management-howto-create-groups/groups001.png)

Grup oluşturulduktan sonra **Gruplar** listesine eklenir. <br/>Grubun **Adını** veya **Açıklamasını** yeniden yapmak için grubun adını ve **Ayarlar'ı**tıklatın.<br/>Grubu silmek için grubun adını tıklatın ve **Sil'e**basın.

Artık grup oluşturulduğuna göre, ürünler ve geliştiricilerle ilişkilendirilebilir.

## <a name="associate-a-group-with-a-product"></a><a name="associate-group-product"> </a>Grubu bir ürünle ilişkilendirme

1. Soldaki **Ürünler** sekmesini seçin.
2. İstenilen ürünün adını tıklatın.
3. **Access denetimine**basın.
4. **+ Ekle grubu'na**tıklayın.

    ![Grubu bir ürünle ilişkilendirme](./media/api-management-howto-create-groups/groups002.png)
5. Eklemek istediğiniz grubu seçin.

    ![Grubu bir ürünle ilişkilendirme](./media/api-management-howto-create-groups/groups003.png)

    Bir grubu üründen kaldırmak için **Sil'i**tıklatın.

    ![Bir grubu silme](./media/api-management-howto-create-groups/groups004.png)

Bir ürün bir grupla ilişkilendirilince, bu gruptaki geliştiriciler ürünü görüntüleyebilir ve bunlara abone olabilir.

> [!NOTE]
> Azure Etkin Dizin grupları eklemek için Azure [API Yönetimi'nde Azure Etkin Dizinini kullanarak geliştirici hesaplarını nasıl yetkilendireceğimize](api-management-howto-aad.md)bakın.

## <a name="associate-groups-with-developers"></a><a name="associate-group-developer"> </a>Grupları geliştiricilerle ilişkilendirme

Bu bölümde, gruplarla üyelerle nasıl ilişkilendirilir gösterilmektedir.

1. Ekranın solundaki **Gruplar** sekmesini seçin.
2. **Üyeler**’i seçin.

    ![Üye ekleme](./media/api-management-howto-create-groups/groups005.png)
3. **+Ekle** ve üye seç' tuşuna basın.

    ![Üye ekleme](./media/api-management-howto-create-groups/groups006.png)
4. **Seç**tuşuna basın.

Geliştirici ve grup arasında ilişkilendirme eklendikten sonra, **bunu Kullanıcılar** sekmesinde görüntüleyebilirsiniz.

## <a name="next-steps"></a><a name="next-steps"> </a>Sonraki adımlar

* Bir geliştirici gruba eklendikten sonra, bu grupla ilişkili ürünleri görüntüleyebilir ve bunlara abone olabilir. Daha fazla bilgi için Azure [API Yönetimi'nde bir ürünü nasıl oluşturup yayımlayabildiğini][How create and publish a product in Azure API Management]görün.
* Yayımcı portalında gruplar oluşturmanın ve yönetmeye ek olarak, API Management REST API [Group](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-group-entity) varlığını kullanarak gruplarınızı oluşturabilir ve yönetebilirsiniz.

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[How create and publish a product in Azure API Management]: api-management-howto-add-products.md

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
[leverage external groups in associated Azure Active Directory tenants]: api-management-howto-aad.md
