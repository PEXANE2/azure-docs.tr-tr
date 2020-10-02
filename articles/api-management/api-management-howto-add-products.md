---
title: Öğretici-Azure API Management bir ürün oluşturma ve yayımlama
description: Bu öğreticide, Azure API Management bir ürün oluşturup yayımlayabilirsiniz. Yayımlandıktan sonra, geliştiriciler ürünün API 'Lerini kullanmaya başlayabilir.
author: mikebudzynski
ms.service: api-management
ms.topic: tutorial
ms.date: 09/30/2020
ms.author: apimpm
ms.openlocfilehash: 2f298f240d8aa7a38b42a8c78ee3c90fe3423d10
ms.sourcegitcommit: d479ad7ae4b6c2c416049cb0e0221ce15470acf6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91630599"
---
# <a name="tutorial-create-and-publish-a-product"></a>Öğretici: ürün oluşturma ve yayımlama  

Azure API Management, bir [*ürün*](api-management-terminology.md#term-definitions) bir veya daha fazla API ve kullanım koşulları ve kullanım koşulları içerir. Ürün yayımlandığında, geliştiriciler ürüne abone olabilir ve ürünün API'lerini kullanmaya başlayabilir.  

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Ürün oluşturma ve yayımlama
> * Ürüne bir API ekleme

:::image type="content" source="media/api-management-howto-add-products/added-product.png" alt-text="Portalda API Management ürünleri":::


## <a name="prerequisites"></a>Önkoşullar

+ [Azure API Management terminolojisini](api-management-terminology.md) öğrenin.
+ Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Ayrıca, şu öğreticiyi tamamlayın: [İlk API'nizi içeri aktarma ve yayımlama](import-and-publish.md).

## <a name="create-and-publish-a-product"></a>Ürün oluşturma ve yayımlama

1. Azure portal oturum açın ve API Management örneğinize gidin.
1. Sol gezinti bölmesinde, **Ürünler**  >  **+ Ekle**' yi seçin.
1.  Ürün **Ekle** penceresinde, ürününüzü oluşturmak için aşağıdaki tabloda açıklanan değerleri girin.

    :::image type="content" source="media/api-management-howto-add-products/02-create-publish-product-01.png" alt-text="Portalda API Management ürünleri":::

    | Ad                     | Açıklama                                                                                                                                                                                                                                                                                                             |
    |--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Görünen ad             | [Geliştirici Portalında](api-management-howto-developer-portal.md)görünmesini istediğiniz ad.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Açıklama              | Ürün hakkında, amacı, erişimi sağladığı API 'Ler ve diğer ayrıntılar gibi bilgiler sağlayın.                                                                                                                                               |
    | Durum                    | Ürünü yayımlamak istiyorsanız **yayımlandı** ' i seçin. Bir üründeki API'lerin çağrılabilmesi için önce ürünün yayımlanması gerekir. Varsayılan olarak, yeni ürünler yayımdan kaldırılır ve yalnızca  **Yöneticiler** grubu tarafından görülebilir.                                                                                      |
    | Abonelik gerektirir    | Ürünü kullanmak için bir kullanıcının abone olması gerekiyorsa seçin.                                                                                                                                                                                                                                   |
    | Onay gerekiyor        | Yöneticinin bu ürüne yönelik abonelik denemelerini gözden geçirmesini ve kabul etmesini ya da reddetmesini istiyorsanız seçin. Seçilmezse, abonelik girişimleri otomatik olarak onaylanır.                                                                                                                         |
    | Abonelik sayısı limiti | İsteğe bağlı olarak, birden çok eş zamanlı aboneliğin sayısını sınırlayın.                                                                                                                                                                                                                                |
    | Yasal koşullar              | Abonelerin ürünü kullanmak için kabul etmek zorunda olduğu ürün kullanım koşullarını ekleyebilirsiniz.                                                                                                                                                                                                             |
    | API'ler                     | Bir veya daha fazla API seçin. Ayrıca, ürünü oluşturduktan sonra API 'Leri ekleyebilirsiniz. Daha fazla bilgi için, bu makalede daha sonra [bir ürüne API ekleme](#add-apis-to-a-product) bölümüne bakın. |

3. Yeni ürünü oluşturmak için **Oluştur** ' u seçin.

### <a name="add-more-configurations"></a>Daha fazla yapılandırma ekleme

Ürünü kaydettikten sonra yapılandırmaya devam edin. API Management Örneğinizde, **Ürünler** penceresinden ürünü seçin. Ekle veya güncelleştir:


|Öğe   |Açıklama  |
|---------|---------|
|Ayarlar     |    Ürün meta verileri ve durumu     |
|API'ler     |  Ürünle ilişkili API 'Ler       |
|[İlkeler](api-management-howto-policies.md)     |  Ürün API 'Lerine uygulanan ilkeler      |
|Erişim denetimi     |  Geliştiriciler veya Konuklar için ürün görünürlüğü       |
|[Abonelikler](api-management-subscriptions.md)    |    Ürün aboneleri     |

## <a name="add-apis-to-a-product"></a>Bir ürüne API ekleme

Ürünler bir veya daha fazla API arasındaki ilişkilendirmelerdir. Bir dizi API ekleyebilir ve geliştirici portalı aracılığıyla geliştiricilere sunabilirsiniz. Ürün oluşturma sırasında, bir veya daha fazla var olan API ekleyebilirsiniz. Ayrıca, ürün **ayarları** sayfasından veya bir API oluştururken, daha sonra da ürüne API 'ler ekleyebilirsiniz.

Geliştiricilerin bir API’ye erişebilmesi için önce ürüne abone olması gerekir. Abone olduklarında, ilgili üründeki tüm API’ler için geçerli olan bir abonelik anahtarı edinirler. APIM örneğini siz oluşturduysanız zaten bir yöneticisinizdir ve varsayılan olarak tüm ürünlere abone olmuşsunuz demektir.

### <a name="add-an-api-to-an-existing-product"></a>Mevcut bir ürüne API ekleme


1. API Management örneğinizin sol gezinti bölmesinde **Ürünler**' i seçin.
1. Bir ürün seçin ve ardından **API 'ler**' i seçin.
1. **+ Ekle**'yi seçin.
1. Bir veya daha fazla API seçin ve ardından öğesini **seçin**.

:::image type="content" source="media/api-management-howto-add-products/02-create-publish-product-02.png" alt-text="Portalda API Management ürünleri":::

> [!TIP]
> Bir kullanıcının aboneliğini, bir [REST API](/rest/api/apimanagement/2019-12-01/subscription/createorupdate) veya PowerShell komutu aracılığıyla özel abonelik anahtarlarıyla bir ürüne oluşturabilir veya güncelleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Ürün oluşturma ve yayımlama
> * Ürüne bir API ekleme

Sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Boş bir API ve sahte API yanıtları oluşturma](mock-api-responses.md)
