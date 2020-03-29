---
title: Şablonları kullanarak API Yönetimi geliştirici portalını özelleştirme
titleSuffix: Azure API Management
description: Şablonları kullanarak Azure API Yönetimi geliştirici portalını nasıl özelleştireceğimiz öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: a195675b-f7d0-4fc9-90bf-860e6f17ccf7
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/04/2019
ms.author: apimpm
ms.openlocfilehash: 7a8c348340be143f7059ce7e64a1c66b66074a45
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75430783"
---
# <a name="how-to-customize-the-azure-api-management-developer-portal-using-templates"></a>Şablonları kullanarak Azure API Yönetimi geliştirici portalını özelleştirme

Azure API Management'ta geliştirici portalını özelleştirmek için kullanılabilecek üç temel yöntem vardır:

* [Statik sayfaların ve sayfa düzeni öğelerinin içeriğini düzenleme][modify-content-layout]
* [Geliştirici portalının tamamında sayfa öğeleri için kullanılan stilleri güncelleştirme][customize-styles]
* [Portal tarafından oluşturulan sayfalar için kullanılan şablonları değiştirme][portal-templates] (bu kılavuzda açıklanmıştır)

Şablonlar, sistem tarafından oluşturulan geliştirici portal sayfalarının içeriğini özelleştirmek için kullanılır (örneğin, API dokümanları, ürünler, kullanıcı kimlik doğrulaması, vb.). [DotLiquid](http://dotliquidmarkup.org/) sözdizimini ve sağlanan yerelleştirilmiş dize kaynakları, simgeler ve sayfa denetimleri kümesini kullanarak, sayfaların içeriğini uygun gördüğünüz şekilde yapılandırmak için büyük bir esnekliğe sahipsiniz.

[!INCLUDE [api-management-portal-legacy.md](../../includes/api-management-portal-legacy.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="developer-portal-templates-overview"></a>Geliştirici portalı şablonlarına genel bakış

Düzenleme şablonları, yönetici olarak oturum açarken **Geliştirici portalından** yapılır. Oraya ulaşmak için önce Azure portalını açın ve API Yönetimi örneğinizin servis araç çubuğundan **Geliştirici portalına** tıklayın.

Geliştirici portalı şablonlarına erişmek için, özelleştirme menüsünü görüntülemek için soldaki özelleştirme simgesini tıklatın ve **Şablonlar'ı**tıklatın.

![Geliştirici portalı şablonları][api-management-customize-menu]

Şablonlar listesi, geliştirici portalındaki farklı sayfaları kapsayan çeşitli şablon kategorileri görüntüler. Her şablon farklıdır, ancak bunları düzenlenecek ve değişiklikleri yayımlama adımları aynıdır. Şablonu yeniden yapmak için şablonun adını tıklatın.

![Geliştirici portalı şablonları][api-management-templates-menu]

Şablonu tıklattığınızda, bu şablon tarafından özelleştirilebilir geliştirici portalı sayfasına götürür. Bu örnekte, **Ürün listesi** şablonu görüntülenir. **Ürün listesi** şablonu, kırmızı dikdörtgentarafından belirtilen ekranın alanını denetler.

![Ürünler listesi şablonu][api-management-developer-portal-templates-overview]

**Kullanıcı Profili** şablonları gibi bazı şablonlar, aynı sayfanın farklı bölümlerini özelleştirin.

![Kullanıcı profili şablonları][api-management-user-profile-templates]

Her geliştirici portalı şablonunun düzenleyicisinin sayfanın alt kısmında görüntülenen iki bölümü vardır. Sol tarafta şablon için düzenleme bölmesi görüntülenirken, sağ tarafta şabloniçin veri modeli görüntülenir.

Şablon düzenleme bölmesi, geliştirici portalında ilgili sayfanın görünümünü ve davranışını kontrol eden biçimlendirmeyi içerir. Şablondaki biçimlendirme [DotLiquid](http://dotliquidmarkup.org/) sözdizimini kullanır. DotLiquid için popüler bir editör [Tasarımcılar için DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers)olduğunu. Düzenleme sırasında şablonda yapılan değişiklikler tarayıcıda gerçek zamanlı olarak görüntülenir, ancak siz [şablonu kaydedip](#to-save-a-template) [yayımlayına](#to-publish-a-template) kadar müşterileriniz tarafından görülemez.

![Şablon biçimlendirmesi][api-management-template]

**Şablon veri** bölmesi, belirli bir şablonda kullanılabilir varlıklar için veri modeli için bir kılavuz sağlar. Şu anda geliştirici portalında görüntülenen canlı verileri görüntüleyerek bu kılavuzu sağlar. **Şablon veri** bölmesinin sağ üst köşesindeki dikdörtgeni tıklatarak şablon bölmelerini genişletebilirsiniz.

![Şablon veri modeli][api-management-template-data]

Önceki örnekte, aşağıdaki örnekte gösterildiği gibi, Geliştirici portalında **Şablon veri** bölmesinde görüntülenen verilerden alınan iki ürün vardır:

```json
{
    "Paging": {
        "Page": 1,
        "PageSize": 10,
        "TotalItemCount": 2,
        "ShowAll": false,
        "PageCount": 1
    },
    "Filtering": {
        "Pattern": null,
        "Placeholder": "Search products"
    },
    "Products": [
        {
            "Id": "56ec64c380ed850042060001",
            "Title": "Starter",
            "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",
            "Terms": "",
            "ProductState": 1,
            "AllowMultipleSubscriptions": false,
            "MultipleSubscriptionsCount": 1
        },
        {
            "Id": "56ec64c380ed850042060002",
            "Title": "Unlimited",
            "Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",
            "Terms": null,
            "ProductState": 1,
            "AllowMultipleSubscriptions": false,
            "MultipleSubscriptionsCount": 1
        }
    ]
}
```

**Ürün listesi** şablonundaki biçimlendirme, bilgileri görüntülemek için ürün koleksiyonunu ve her bir ürüne bir bağlantı sunarak istenen çıktıyı sağlamak için verileri işler. `<search-control>` Biçimlendirmedeki `<page-control>` öğeleri ve öğeleri not edin. Bunlar, sayfadaki arama ve sayfalama denetimlerinin ekranını denetler. `ProductsStrings|PageTitleProducts`sayfanın üstbilgi metnini `h2` içeren yerelleştirilmiş bir dize başvurusudur. Geliştirici portal şablonlarında kullanılabilecek dize kaynakları, sayfa denetimleri ve simgelerin listesi için [bkz.](api-management-developer-portal-templates-reference.md)

```html
<search-control></search-control>
<div class="row">
    <div class="col-md-9">
        <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>
    </div>
</div>
<div class="row">
    <div class="col-md-12">
    {% if products.size > 0 %}
    <ul class="list-unstyled">
    {% for product in products %}
        <li>
            <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>
            {{product.description}}
        </li>
    {% endfor %}
    </ul>
    <paging-control></paging-control>
    {% else %}
    {% localized "CommonResources|NoItemsToDisplay" %}
    {% endif %}
    </div>
</div>
```

## <a name="to-save-a-template"></a>Şablonkaydetmek için
Şablonkaydetmek için şablon düzenleyicisine kaydet'i tıklatın.

![Şablonu kaydetme][api-management-save-template]

Kaydedilen değişiklikler yayımlanana kadar geliştirici portalında yayınlanmaz.

## <a name="to-publish-a-template"></a>Şablon yayımlamak için
Kaydedilen şablonlar tek tek veya hep birlikte yayımlanabilir. Tek bir şablon yayımlamak için şablon düzenleyicisinde yayımla'yı tıklatın.

![Şablonu yayımlama][api-management-publish-template]

Şablonu doğrulamak ve geliştirici portalında canlı hale getirmek için **Evet'i** tıklatın.

![Yayımlamayı onayla][api-management-publish-template-confirm]

Şu anda yayınlanmamış tüm şablon sürümlerini yayımlamak için şablonlar listesinde **Yayımla'yı** tıklatın. Yayımlanmamış şablonlar, şablon adını izleyen bir yıldız işaretiyle belirlenir. Bu örnekte, **Ürün listesi** ve **Ürün** şablonları yayımlanmaktadır.

![Şablonları yayımlama][api-management-publish-templates]

Onaylamak için **özelleştirmeleri yayımla'yı** tıklatın.

![Yayımlamayı onayla][api-management-publish-customizations]

Yeni yayınlanan şablonlar geliştirici portalında hemen geçerlidir.

## <a name="to-revert-a-template-to-the-previous-version"></a>Şablonu önceki sürüme geri getirmek için
Bir şablonu önceki yayımlanmış sürüme geri getirmek için şablon düzenleyicisinde geri çevir'i tıklatın.

![Şablonu geri][api-management-revert-template]

Onaylamak için **Evet**’e tıklayın.

![Confirm][api-management-revert-template-confirm]

Şablonun önceden yayımlanmış sürümü, geri alma işlemi tamamlandıktan sonra geliştirici portalında canlı olarak yayınlanır.

## <a name="to-restore-a-template-to-the-default-version"></a>Şablonu varsayılan sürüme geri yüklemek için
Şablonları varsayılan sürümüne geri getirmek iki adımlı bir işlemdir. Önce şablonlar geri yüklenmeli ve sonra geri yüklenen sürümler yayımlanmalıdır.

Tek bir şablonu varsayılan sürüme geri yüklemek için şablon düzenleyicisinde geri yükle'yi tıklatın.

![Şablonu geri][api-management-reset-template]

Onaylamak için **Evet**’e tıklayın.

![Confirm][api-management-reset-template-confirm]

Tüm şablonları varsayılan sürümlerine geri yüklemek için şablon **listesindevarsayılan şablonları geri yükle'yi** tıklatın.

![Şablonları geri yükleme][api-management-restore-templates]

Geri yüklenen şablonlar daha sonra [şablon yayımlamak için](#to-publish-a-template)aşağıdaki adımları izleyerek tek tek veya tümüyle tek tek veya tümüyle yayımlanmalıdır.

## <a name="next-steps"></a>Sonraki adımlar
Geliştirici portal şablonları, dize kaynakları, simgeler ve sayfa denetimleri için başvuru bilgileri için [bkz.](api-management-developer-portal-templates-reference.md)

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[api-management-customize-menu]: ./media/api-management-developer-portal-templates/api-management-customize-menu.png
[api-management-templates-menu]: ./media/api-management-developer-portal-templates/api-management-templates-menu.png
[api-management-developer-portal-templates-overview]: ./media/api-management-developer-portal-templates/api-management-developer-portal-templates-overview.png
[api-management-template]: ./media/api-management-developer-portal-templates/api-management-template.png
[api-management-template-data]: ./media/api-management-developer-portal-templates/api-management-template-data.png
[api-management-developer-portal-menu]: ./media/api-management-developer-portal-templates/api-management-developer-portal-menu.png
[api-management-management-console]: ./media/api-management-developer-portal-templates/api-management-management-console.png
[api-management-browse]: ./media/api-management-developer-portal-templates/api-management-browse.png
[api-management-user-profile-templates]: ./media/api-management-developer-portal-templates/api-management-user-profile-templates.png
[api-management-save-template]: ./media/api-management-developer-portal-templates/api-management-save-template.png
[api-management-publish-template]: ./media/api-management-developer-portal-templates/api-management-publish-template.png
[api-management-publish-template-confirm]: ./media/api-management-developer-portal-templates/api-management-publish-template-confirm.png
[api-management-publish-templates]: ./media/api-management-developer-portal-templates/api-management-publish-templates.png
[api-management-publish-customizations]: ./media/api-management-developer-portal-templates/api-management-publish-customizations.png
[api-management-revert-template]: ./media/api-management-developer-portal-templates/api-management-revert-template.png
[api-management-revert-template-confirm]: ./media/api-management-developer-portal-templates/api-management-revert-template-confirm.png
[api-management-reset-template]: ./media/api-management-developer-portal-templates/api-management-reset-template.png
[api-management-reset-template-confirm]: ./media/api-management-developer-portal-templates/api-management-reset-template-confirm.png
[api-management-restore-templates]: ./media/api-management-developer-portal-templates/api-management-restore-templates.png
