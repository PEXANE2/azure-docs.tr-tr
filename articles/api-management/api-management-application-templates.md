---
title: Azure API Yönetimi'nde uygulama şablonları | Microsoft Dokümanlar
description: Azure API Yönetimi'ndeki geliştirici portalındaki Uygulama sayfalarının içeriğini nasıl özelleştireceğimiz öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: f3122c4d-e10e-4cdf-977b-36e8f4133fc8
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/04/2019
ms.author: apimpm
ms.openlocfilehash: d635950c8b34986cd5824660166017317948cbd8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73176917"
---
# <a name="application-templates-in-azure-api-management"></a>Azure API Yönetimi'nde uygulama şablonları
Azure API Yönetimi, geliştirici portal sayfalarının içeriğini, içeriklerini yapılandıran bir dizi şablon kullanarak özelleştirme olanağı sağlar. [DotLiquid](http://dotliquidmarkup.org/) sözdizimini ve [Tasarımcılar için DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers)gibi seçtiğiniz düzenleyiciyi ve sağlanan yerelleştirilmiş [String kaynakları,](api-management-template-resources.md#strings) [Glyph kaynakları](api-management-template-resources.md#glyphs)ve Sayfa [denetimleri](api-management-page-controls.md)kümesini kullanarak, bu şablonları kullanarak uygun gördüğünüz sayfaların içeriğini yapılandırmak için büyük bir esnekliğe sahipsiniz.  
  
 Bu bölümdeki şablonlar, geliştirici portalındaki Uygulama sayfalarının içeriğini özelleştirmenize olanak tanır.  
  
-   [Başvuru listesi](#ProductList)  
  
-   [Uygulama](#Application)  
  
> [!NOTE]
>  Örnek varsayılan şablonlar aşağıdaki belgelere dahildir, ancak sürekli iyileştirmeler nedeniyle değiştirilebilir. İstenilen tek tek şablonlara yönlendirerek geliştirici portalındaki canlı varsayılan şablonları görüntüleyebilirsiniz. Şablonlarla çalışma hakkında daha fazla bilgi için, [şablonları kullanarak API Yönetimi geliştirici portalını nasıl özelleştireceğime](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/)bakın.  

[!INCLUDE [api-management-portal-legacy.md](../../includes/api-management-portal-legacy.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]
  
##  <a name="application-list"></a><a name="ProductList"></a>Başvuru listesi  
 **Uygulama listesi** şablonu, geliştirici portalındaki uygulama listesi sayfasının gövdesini özelleştirmenize olanak tanır.  
  
 ![Uygulama Listesi Sayfa Geliştirici Portal Şablonları](./media/api-management-application-templates/APIM-Application-List-Page-Developer-Portal-Templates.png "APIM Uygulama Listesi Sayfa Geliştirici Portalı Şablonları")  
  
### <a name="default-template"></a>Varsayılan şablon  
  
```xml  
<div class="row">  
    <div class="col-md-9">  
        <h2>{% localized "AppStrings|WebApplicationsHeader" %}</h2>  
    </div>  
</div>  
<div class="row">  
    <div class="col-md-12">  
    {% if applications.size > 0 %}  
        <ul class="list-unstyled">  
        {% for app in applications %}  
            <li>  
            {% if app.application.icon.url != "" %}  
                <aside>  
                    <a href="/applications/details/{{app.application.id}}"><img src="{{app.application.icon.url}}" alt="App Icon" /></a>  
                </aside>  
            {% endif %}  
                <h3><a href="/applications/details/{{app.application.id}}">{{app.application.title}}</a></h3>  
                {{app.application.description}}  
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
  
### <a name="data-model"></a>Veri modeli  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Paging`|[Sayfalama](api-management-template-data-model-reference.md#Paging) varlığı.|Uygulamalar koleksiyonu için sayfalama bilgileri.|  
|`Applications`|[Uygulama](api-management-template-data-model-reference.md#Application) varlıklarının toplanması.|Geçerli kullanıcı tarafından görülebilen uygulamalar.|  
|`CategoryName`|string|Uygulama kategorisi.|  
  
### <a name="sample-template-data"></a>Örnek şablon verileri  
  
```json  
{  
    "Paging": {  
        "Page": 1,  
        "PageSize": 10,  
        "TotalItemCount": 1,  
        "ShowAll": false,  
        "PageCount": 1  
    },  
    "Applications": [  
        {  
            "Application": {  
                "Id": "5702b96fb16653124c8f9ba8",  
                "Title": "Contoso Calculator",  
                "Description": "A simple online calculator.",  
                "Url": null,  
                "Version": null,  
                "Requirements": "Free application with no requirements.",  
                "State": 2,  
                "RegistrationDate": "2016-04-04T18:59:00",  
                "CategoryId": 5,  
                "DeveloperId": "5702b5b0b16653124c8f9ba4",  
                "Attachments": [  
                    {  
                        "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
                        "Url": "https://apimgmtst65gdjvjrgdbfhr4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
                        "Type": "Icon",  
                        "ContentType": "image/png"  
                    },  
                    {  
                        "UniqueId": "2b4fa5dd-00ff-4a8f-b1b7-51e715849ede",  
                        "Url": "https://apimgmtst65gdjvjrgdbfhr4.blob.core.windows.net/content/applications/2b4fa5dd-00ff-4a8f-b1b7-51e715849ede.png",  
                        "Type": "Screenshot",  
                        "ContentType": "image/png"  
                    }  
                ],  
                "Icon": {  
                    "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
                    "Url": "https://apimgmtst65gdjvjrgdbfhr4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
                    "Type": "Icon",  
                    "ContentType": "image/png"  
                }  
            },  
            "CategoryName": "Finance"  
        }  
    ]  
}  
```  
  
##  <a name="application"></a><a name="Application"></a>Uygulama  
 **Uygulama** şablonu, geliştirici portalındaki uygulama sayfasının gövdesini özelleştirmenize olanak tanır.  
  
 ![Uygulama Sayfası Geliştirici Portal Şablonları](./media/api-management-application-templates/APIM-Application-Page-Developer-Portal-Templates.png "APIM Uygulama Sayfası Geliştirici Portalı Şablonları")  
  
### <a name="default-template"></a>Varsayılan şablon  
  
```xml  
<h2>{{title}}</h2>  
{% if icon.url != "" %}  
<aside class="applications_aside">  
  <div class="image-placeholder">  
    <img src="{{icon.url}}" alt="Application Icon" />  
  </div>  
</aside>  
{% endif %}  
  
<article>  
  {% if url != "" %}  
    <a target="_blank" href="{{url}}">{{url}}</a>  
  {% endif %}  
  
  <p>{{description}}</p>  
  
  {% if requirements != null %}  
  <h3>{% localized "AppDetailsStrings|WebApplicationsRequirementsHeader" %}</h3>  
  <p>{{requirements}}</p>  
  {% endif %}  
  
  {% if attachments.size > 0 %}  
  <h3>{% localized "AppDetailsStrings|WebApplicationsScreenshotsHeader" %}</h3>  
    {% for screenshot in attachments %}  
      {% if screenshot.type != "Icon" %}  
      <a href="{{screenshot.url}}" data-lightbox="example-set">  
          <img src="/Developer/Applications/Thumbnail?url={{screenshot.url}}" alt='{% localized "AppDetailsStrings|WebApplicationsScreenshotAlt" %}' />  
      </a>  
      {% endif %}  
    {% endfor %}  
  {% endif %}  
</article>  
  
```  
  
### <a name="controls"></a>Denetimler  
 Şablon, `Application` herhangi bir sayfa [denetiminin](api-management-page-controls.md)kullanılmasına izin vermez.  
  
### <a name="data-model"></a>Veri modeli  
 [Uygulama](api-management-template-data-model-reference.md#Application) varlığı.  
  
### <a name="sample-template-data"></a>Örnek şablon verileri  
  
```json  
{  
    "Id": "5702b96fb16653124c8f9ba8",  
    "Title": "Contoso Calculator",  
    "Description": "A simple online calculator.",  
    "Url": null,  
    "Version": null,  
    "Requirements": "Free application with no requirements.",  
    "State": 2,  
    "RegistrationDate": "2016-04-04T18:59:00",  
    "CategoryId": 5,  
    "DeveloperId": "5702b5b0b16653124c8f9ba4",  
    "Attachments": [  
        {  
            "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
            "Url": "https://apimgmtst3aybshdqqcqrle4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
            "Type": "Icon",  
            "ContentType": "image/png"  
        },  
        {  
            "UniqueId": "2b4fa5dd-00ff-4a8f-b1b7-51e715849ede",  
            "Url": "https://apimgmtst3aybshdqqcqrle4.blob.core.windows.net/content/applications/2b4fa5dd-00ff-4a8f-b1b7-51e715849ede.png",  
            "Type": "Screenshot",  
            "ContentType": "image/png"  
        }  
    ],  
    "Icon": {  
        "UniqueId": "a58af001-e6c3-45fd-8bc9-c60a1875c3f6",  
        "Url": "https://apimgmtst3aybshdqqcqrle4.blob.core.windows.net/content/applications/a58af001-e6c3-45fd-8bc9-c60a1875c3f6.png",  
        "Type": "Icon",  
        "ContentType": "image/png"  
    }  
}  
```

## <a name="next-steps"></a>Sonraki adımlar
Şablonlarla çalışma hakkında daha fazla bilgi için, [şablonları kullanarak API Yönetimi geliştirici portalını nasıl özelleştireceğime](api-management-developer-portal-templates.md)bakın.
