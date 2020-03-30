---
title: Azure API Yönetimi'nde ürün şablonları | Microsoft Dokümanlar
description: Azure API Yönetimi geliştirici portalındaki ürün sayfalarının içeriğini nasıl özelleştireceğimiz öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 49f9254c-4c5f-4ed4-9c8d-798f44e805ee
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/04/2019
ms.author: apimpm
ms.openlocfilehash: 393563427e936e07315cd44b78cb793d4292b352
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79243932"
---
# <a name="product-templates-in-azure-api-management"></a>Azure API Yönetimi'nde ürün şablonları

Azure API Yönetimi, geliştirici portal sayfalarının içeriğini, içeriklerini yapılandıran bir dizi şablon kullanarak özelleştirme olanağı sağlar. [DotLiquid](http://dotliquidmarkup.org/) sözdizimini ve [Tasarımcılar için DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers)gibi seçtiğiniz düzenleyiciyi ve sağlanan yerelleştirilmiş [String kaynakları,](api-management-template-resources.md#strings) [Glyph kaynakları](api-management-template-resources.md#glyphs)ve Sayfa [denetimleri](api-management-page-controls.md)kümesini kullanarak, bu şablonları kullanarak uygun gördüğünüz sayfaların içeriğini yapılandırmak için büyük bir esnekliğe sahipsiniz.  
  
 Bu bölümdeki şablonlar, geliştirici portalındaki ürün sayfalarının içeriğini özelleştirmenize olanak tanır.  
  
-   [Ürün listesi](#ProductList)  
  
-   [Ürün](#Product)  
  
> [!NOTE]
>  Örnek varsayılan şablonlar aşağıdaki belgelere dahildir, ancak sürekli iyileştirmeler nedeniyle değiştirilebilir. İstenilen tek tek şablonlara yönlendirerek geliştirici portalındaki canlı varsayılan şablonları görüntüleyebilirsiniz. Şablonlarla çalışma hakkında daha fazla bilgi için, [şablonları kullanarak API Yönetimi geliştirici portalını nasıl özelleştireceğime](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/)bakın.  

[!INCLUDE [api-management-portal-legacy.md](../../includes/api-management-portal-legacy.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]
  
##  <a name="product-list"></a><a name="ProductList"></a>Ürün listesi  
 **Ürün listesi** şablonu, geliştirici portalındaki ürün listesi sayfasının gövdesini özelleştirmenize olanak tanır.  
  
 ![Ürün listesi](./media/api-management-product-templates/APIM_ProductsListTemplatePage.png "APIM_ProductsListTemplatePage")  
  
### <a name="default-template"></a>Varsayılan şablon  
  
```xml  
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
  
### <a name="controls"></a>Denetimler  
 Şablon `Product list` aşağıdaki sayfa [denetimlerini](api-management-page-controls.md)kullanabilir.  
  
-   [sayfalama-kontrol](api-management-page-controls.md#paging-control)  
  
-   [arama denetimi](api-management-page-controls.md#search-control)  
  
### <a name="data-model"></a>Veri modeli  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Sayfalama|[Sayfalama](api-management-template-data-model-reference.md#Paging) varlığı.|Ürün koleksiyonu için sayfalama bilgileri.|  
|Filtreleme|[Filtreleme](api-management-template-data-model-reference.md#Filtering) varlığı.|Ürünler listesi sayfasının filtreleme bilgileri.|  
|Ürünler|[Ürün](api-management-template-data-model-reference.md#Product) varlıklarının toplanması.|Geçerli kullanıcı tarafından görülebilen ürünler.|  
  
### <a name="sample-template-data"></a>Örnek şablon verileri  
  
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
            "Id": "56f9445ffaf7560049060001",  
            "Title": "Starter",  
            "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
            "Terms": "",  
            "ProductState": 1,  
            "AllowMultipleSubscriptions": false,  
            "MultipleSubscriptionsCount": 1  
        },  
        {  
            "Id": "56f9445ffaf7560049060002",  
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
  
##  <a name="product"></a><a name="Product"></a>Ürün  
 **Ürün** şablonu, geliştirici portalındaki ürün sayfasının gövdesini özelleştirmenize olanak tanır.  
  
 ![Geliştirici portalı ürün sayfası](./media/api-management-product-templates/APIM_ProductPage.png "APIM_ProductPage")  
  
### <a name="default-template"></a>Varsayılan şablon  
  
```xml  
<h2>{{Product.Title}}</h2>  
<p>{{Product.Description}}</p>  
  
{% assign replaceString0 = '{0}' %}  
  
{% if Limits and Limits.size > 0 %}  
<h3>{% localized "ProductDetailsStrings|WebProductsUsageLimitsHeader"%}</h3>  
<ul>  
  {% for limit in Limits %}  
  <li>{{limit.DisplayName}}</li>  
  {% endfor %}  
</ul>  
{% endif %}  
  
{% if apis.size > 0 %}  
<p>  
  <b>  
    {% if apis.size == 1 %}  
    {% capture apisCountText %}{% localized "ProductDetailsStrings|TextblockSingleApisCount" %}{% endcapture %}  
    {% else %}  
    {% capture apisCountText %}{% localized "ProductDetailsStrings|TextblockMultipleApisCount" %}{% endcapture %}  
    {% endif %}  
  
    {% capture apisCount %}{{apis.size}}{% endcapture %}  
    {{ apisCountText | replace : replaceString0, apisCount }}  
  </b>  
</p>  
  
<ul>  
  {% for api in Apis %}  
  <li>  
    <a href="/docs/services/{{api.Id}}">{{api.Name}}</a>  
  </li>  
  {% endfor %}  
</ul>  
{% endif %}  
  
{% if subscriptions.size > 0 %}  
<p>  
  <b>  
    {% if subscriptions.size == 1 %}  
    {% capture subscriptionsCountText %}{% localized "ProductDetailsStrings|TextblockSingleSubscriptionsCount" %}{% endcapture %}  
    {% else %}  
    {% capture subscriptionsCountText %}{% localized "ProductDetailsStrings|TextblockMultipleSubscriptionsCount" %}{% endcapture %}  
    {% endif %}  
  
    {% capture subscriptionsCount %}{{subscriptions.size}}{% endcapture %}  
    {{ subscriptionsCountText | replace : replaceString0, subscriptionsCount }}  
  </b>  
</p>  
  
<ul>  
  {% for subscription in subscriptions %}  
  <li>  
    <a href="/developer#{{subscription.Id}}">{{subscription.DisplayName}}</a>  
  </li>  
  {% endfor %}  
</ul>  
{% endif %}  
{% if CannotAddBecauseSubscriptionNumberLimitReached %}  
<b>{% localized "ProductDetailsStrings|TextblockSubscriptionLimitReached" %}</b>  
{% elsif CannotAddBecauseMultipleSubscriptionsNotAllowed == false %}  
<subscribe-button></subscribe-button>  
{% endif %}  
```  
  
### <a name="controls"></a>Denetimler  
 Şablon `Product list` aşağıdaki sayfa [denetimlerini](api-management-page-controls.md)kullanabilir.  
  
-   [abone düğmesi](api-management-page-controls.md#subscribe-button)  
  
### <a name="data-model"></a>Veri modeli  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Ürün|[Ürün](api-management-template-data-model-reference.md#Product)|Belirtilen ürün.|  
|IsDeveloperAbone|boole|Geçerli kullanıcının bu ürüne abone olup olmadığı.|  
|Abonelik Durumu|number|Aboneliğin durumu. Olası durumlar şunlardır:<br /><br /> -   `0 - suspended`– abonelik engellenir ve abone ürünün herhangi bir API'sini arayamaz.<br />-   `1 - active`– abonelik etkindir.<br />-   `2 - expired`– aboneliğin son kullanma tarihine ulaştı ve devre dışı bırakıldı.<br />-   `3 - submitted`– abonelik isteği geliştirici tarafından yapılmış, ancak henüz onaylanmamış veya reddedilmemiştir.<br />-   `4 - rejected`– abonelik isteği bir yönetici tarafından reddedildi.<br />-   `5 - cancelled`– abonelik geliştirici veya yönetici tarafından iptal edilmiştir.|  
|Sınırlar|array|Bu özellik amortismana sokulmadı ve kullanılmamalıdır.|  
|TemsilciLikAbonelik Etkin|boole|Bu abonelik için [temsilcilik](https://azure.microsoft.com/documentation/articles/api-management-howto-setup-delegation/) etkinleştirilip etkinleştirilemediği.|  
|TemsilciAbonelikUrl|string|Temsilci etkinse, devredilen abonelik URL'si.|  
|IsAgreed|boole|Ürünün şartları varsa, geçerli kullanıcının şartları kabul edip etmediği.|  
|Abonelikler|Abonelik [özet](api-management-template-data-model-reference.md#SubscriptionSummary) varlıklarının toplanması.|Ürünün abonelikleri.|  
|Apı 'leri|[API](api-management-template-data-model-reference.md#API) varlıklarının toplanması.|Bu üründeki API'ler.|  
|CannotaddBecauseSubscriptionNumberLimitReached|boole|Geçerli kullanıcının abonelik limitiyle ilgili olarak bu ürüne abone olup olmadığı.|  
|CannotAddBecauseMultipleSubscriptionsNotAllowed|boole|Geçerli kullanıcının, birden fazla aboneliğe izin verilip verilmemesi ile ilgili olarak bu ürüne abone olmaya uygun olup olmadığı.|  
  
### <a name="sample-template-data"></a>Örnek şablon verileri  
  
```json  
{  
    "Product": {  
        "Id": "56f9445ffaf7560049060001",  
        "Title": "Starter",  
        "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
        "Terms": "",  
        "ProductState": 1,  
        "AllowMultipleSubscriptions": false,  
        "MultipleSubscriptionsCount": 1  
    },  
    "IsDeveloperSubscribed": true,  
    "SubscriptionState": 1,  
    "Limits": [],  
    "DelegatedSubscriptionEnabled": false,  
    "DelegatedSubscriptionUrl": null,  
    "IsAgreed": false,  
    "Subscriptions": [  
        {  
            "Id": "56f9445ffaf7560049070001",  
            "DisplayName": "Starter  (default)"  
        }  
    ],  
    "Apis": [  
        {  
            "id": "56f9445ffaf7560049040001",  
            "name": "Echo API",  
            "description": null,  
            "serviceUrl": "http://echoapi.cloudapp.net/api",  
            "path": "echo",  
            "protocols": [  
                2  
            ],  
            "authenticationSettings": null,  
            "subscriptionKeyParameterNames": null  
        }  
    ],  
    "CannotAddBecauseSubscriptionNumberLimitReached": false,  
    "CannotAddBecauseMultipleSubscriptionsNotAllowed": true  
}  
```

## <a name="next-steps"></a>Sonraki adımlar
Şablonlarla çalışma hakkında daha fazla bilgi için, [şablonları kullanarak API Yönetimi geliştirici portalını nasıl özelleştireceğime](api-management-developer-portal-templates.md)bakın.
