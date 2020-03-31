---
title: Azure API Yönetimi'nde sorun şablonları | Microsoft Dokümanlar
description: Azure API Yönetimi'ndeki geliştirici portalındaki Sorun sayfalarının içeriğini nasıl özelleştireceğimiz öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 47da4bb2-426e-4e53-8fa7-214ee2e3ab37
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/04/2019
ms.author: apimpm
ms.openlocfilehash: 1dac90053797caf66af79e458b9dbb95b682cd17
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249587"
---
# <a name="issue-templates-in-azure-api-management"></a>Azure API Yönetimi'nde sorun şablonları
Azure API Yönetimi, geliştirici portal sayfalarının içeriğini, içeriklerini yapılandıran bir dizi şablon kullanarak özelleştirme olanağı sağlar. [DotLiquid](http://dotliquidmarkup.org/) sözdizimini ve [Tasarımcılar için DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers)gibi seçtiğiniz düzenleyiciyi ve sağlanan yerelleştirilmiş [String kaynakları,](api-management-template-resources.md#strings) [Glyph kaynakları](api-management-template-resources.md#glyphs)ve Sayfa [denetimleri](api-management-page-controls.md)kümesini kullanarak, bu şablonları kullanarak uygun gördüğünüz sayfaların içeriğini yapılandırmak için büyük bir esnekliğe sahipsiniz.  
  
 Bu bölümdeki şablonlar, geliştirici portalındaki Sorun sayfalarının içeriğini özelleştirmenize olanak tanır.  
  
-   [Sorun listesi](#IssueList)  
  
> [!NOTE]
>  Örnek varsayılan şablonlar aşağıdaki belgelere dahildir, ancak sürekli iyileştirmeler nedeniyle değiştirilebilir. İstenilen tek tek şablonlara yönlendirerek geliştirici portalındaki canlı varsayılan şablonları görüntüleyebilirsiniz. Şablonlarla çalışma hakkında daha fazla bilgi için, [şablonları kullanarak API Yönetimi geliştirici portalını nasıl özelleştireceğime](api-management-developer-portal-templates.md)bakın.  

[!INCLUDE [api-management-portal-legacy.md](../../includes/api-management-portal-legacy.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]
  
##  <a name="issue-list"></a><a name="IssueList"></a>Sorun listesi  
 **Sorun listesi** şablonu, geliştirici portalındaki sorun listesi sayfasının gövdesini özelleştirmenize olanak tanır.  
  
 ![Sorun Listesi Geliştirici Portalı](./media/api-management-issue-templates/APIM-Issue-List-Developer-Portal.png "APIM Konu Listesi Geliştirici Portalı")  
  
### <a name="default-template"></a>Varsayılan şablon  
  
```xml
<div class="row">
  <div class="col-md-9">
    <h2>{% localized "IssuesStrings|WebIssuesIndexTitle" %}</h2>
  </div>
</div>
<div class="row">
  <div class="col-md-12">
    {% if issues.size > 0 %}
    <ul class="list-unstyled">
      {% capture reportedBy %}{% localized "IssuesStrings|WebIssuesStatusReportedBy" %}{% endcapture %}
      {% assign replaceString0 = '{0}' %}
      {% assign replaceString1 = '{1}' %}
      {% for issue in issues %}
      <li>
        <h3>
          <a href="/issues/{{issue.id}}">{{issue.title}}</a>
        </h3>
        <p>{{issue.description}}</p>
        <em>
          {% capture state %}{{issue.issueState}}{% endcapture %}
          {% capture devName %}{{issue.subscriptionDeveloperName}}{% endcapture %}
          {% capture str1 %}{{ reportedBy | replace : replaceString0, state }}{% endcapture %}
          {{ str1 | replace : replaceString1, devName }}
          <span class="UtcDateElement">{{ issue.reportedOn | date: "r" }}</span>
        </em>
      </li>
      {% endfor %}
    </ul>
    <paging-control></paging-control>
    {% else %}
    {% localized "CommonResources|NoItemsToDisplay" %}
    {% endif %}
    {% if canReportIssue %}
    <a class="btn btn-primary" id="createIssue" href="/Issues/Create">{% localized "IssuesStrings|WebIssuesReportIssueButton" %}</a>
    {% elsif isAuthenticated %}
    <hr />
    <p>{% localized "IssuesStrings|WebIssuesNoActiveSubscriptions" %}</p>
    {% else %}
    <hr />
    <p>
      {% capture signIntext %}{% localized "IssuesStrings|WebIssuesNotSignin" %}{% endcapture %}
      {% capture link %}<a href="/signin">{% localized "IssuesStrings|WebIssuesSignIn" %}</a>{% endcapture %}
      {{ signIntext | replace : replaceString0, link }}
    </p>
    {% endif %}
  </div>
</div>
```
  
### <a name="controls"></a>Denetimler  
 Şablon `Issue list` aşağıdaki sayfa [denetimlerini](api-management-page-controls.md)kullanabilir.  
  
-   [sayfalama-kontrol](api-management-page-controls.md#paging-control)  
  
### <a name="data-model"></a>Veri modeli  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|`Issues`|[İhraç](api-management-template-data-model-reference.md#Issue) varlıklarının toplanması.|Geçerli kullanıcı tarafından görülebilen sorunlar.|  
|`Paging`|[Sayfalama](api-management-template-data-model-reference.md#Paging) varlığı.|Uygulamalar koleksiyonu için sayfalama bilgileri.|  
|`IsAuthenticated`|boole|Geçerli kullanıcının geliştirici portalında oturum açıp açmayacağı.|  
|`CanReportIssues`|boole|Geçerli kullanıcının bir sorun dosyası açma izinleri olup olmadığı.|  
|`Search`|string|Bu özellik amortismana sokulmadı ve kullanılmamalıdır.|  
  
### <a name="sample-template-data"></a>Örnek şablon verileri  
  
```json
{
    "Issues": [
        {
            "Id": "5702b68bb16653124c8f9ba7",
            "ApiId": "570275f1b16653124c8f9ba3",
            "Title": "I couldn't figure out how to connect my application to the API",
            "Description": "I'm having trouble connecting my application to the backend API.",
            "SubscriptionDeveloperName": "Clayton",
            "IssueState": "Proposed",
            "ReportedOn": "2016-04-04T18:46:35.64",
            "Comments": null,
            "Attachments": null,
            "Services": null
        }
    ],
    "Paging": {
        "Page": 1,
        "PageSize": 10,
        "TotalItemCount": 1,
        "ShowAll": false,
        "PageCount": 1
    },
    "IsAuthenticated": true,
    "CanReportIssue": true,
    "Search": null
}
```

## <a name="next-steps"></a>Sonraki adımlar
Şablonlarla çalışma hakkında daha fazla bilgi için, [şablonları kullanarak API Yönetimi geliştirici portalını nasıl özelleştireceğime](api-management-developer-portal-templates.md)bakın.
