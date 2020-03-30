---
title: Azure API Yönetimi'nde sayfa şablonları | Microsoft Dokümanlar
description: Azure API Yönetimi'nde bir dizi şablonkullanarak geliştirici portal sayfalarının içeriğini nasıl özelleştireceğimiz öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: e57df269-1019-4b74-b74d-53155b809d59
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/04/2019
ms.author: apimpm
ms.openlocfilehash: ce56c406c884471c445b25343d5c42f9edcbe4c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249548"
---
# <a name="page-templates-in-azure-api-management"></a>Azure API Yönetimi'nde sayfa şablonları
Azure API Yönetimi, geliştirici portal sayfalarının içeriğini, içeriklerini yapılandıran bir dizi şablon kullanarak özelleştirme olanağı sağlar. [DotLiquid](http://dotliquidmarkup.org/) sözdizimini ve [Tasarımcılar için DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers)gibi seçtiğiniz düzenleyiciyi ve sağlanan yerelleştirilmiş [String kaynakları,](api-management-template-resources.md#strings) [Glyph kaynakları](api-management-template-resources.md#glyphs)ve Sayfa [denetimleri](api-management-page-controls.md)kümesini kullanarak, bu şablonları kullanarak uygun gördüğünüz sayfaların içeriğini yapılandırmak için büyük bir esnekliğe sahipsiniz.  
  
 Bu bölümdeki şablonlar, oturum açma, kaydolma ve geliştirici portalında sayfa bulunmayan sayfalarıözelleştirmenize olanak tanır.  
  
-   [Oturum aç](#SignIn)  
  
-   [Kaydolma](#SignUp)  
  
-   [Sayfa bulunamadı](#PageNotFound)  
  
> [!NOTE]
>  Örnek varsayılan şablonlar aşağıdaki belgelere dahildir, ancak sürekli iyileştirmeler nedeniyle değiştirilebilir. İstenilen tek tek şablonlara yönlendirerek geliştirici portalındaki canlı varsayılan şablonları görüntüleyebilirsiniz. Şablonlarla çalışma hakkında daha fazla bilgi için, [şablonları kullanarak API Yönetimi geliştirici portalını nasıl özelleştireceğime](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/)bakın.  

[!INCLUDE [api-management-portal-legacy.md](../../includes/api-management-portal-legacy.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]
  
##  <a name="sign-in"></a><a name="SignIn"></a>Oturum Aç  
 **Oturum açma** şablonu, geliştirici portalındaki oturum açma sayfasını özelleştirmenize olanak tanır.  
  
 ![Oturum Aç Sayfası](./media/api-management-page-templates/APIM-Sign-In-Page-Developer-Portal-Templates.png "APIM Oturum Açma Sayfası Geliştirici Portalı Şablonları")  
  
### <a name="default-template"></a>Varsayılan şablon  
  
```xml  
<h2 class="text-center">{% localized "SigninStrings|WebAuthenticationSigninTitle" %}</h2>  
{% if registrationEnabled == true %}  
<p class="text-center">{% localized "SigninStrings|WebAuthenticationNotAMember" %}</p>  
{% endif %}  
  
<div class="row center-block ap-idp-container">  
  <div class="col-md-6">  
    {% if registrationEnabled == true %}  
        <p>{% localized "SigninStrings|WebAuthenticationSigininWithPassword" %}</p>  
    <basic-SignIn></basic-SignIn>  
    {% endif %}  
  </div>  
  
    {% if registrationEnabled != true and providers.size == 0 %}  
        {% localized "ProviderInfoStrings|TextboxExternalIdentitiesDisabled" %}  
  {% else %}  
        {% if providers.size > 0 %}  
      <div class="col-md-6">  
            <div class="providers-list">  
                <p class="text-left">  
                {% if registrationEnabled == true %}  
                    {% localized "ProviderInfoStrings|TextboxExternalIdentitiesSigninInvitation" %}  
                {% else %}  
                    {% localized "ProviderInfoStrings|TextboxExternalIdentitiesSigninInvitationPrimary" %}  
                {% endif %}  
                </p>  
        <providers></providers>  
            </div>  
    </div>  
        {% endif %}  
    {% endif %}  
  
  {% if userRegistrationTermsEnabled == true %}  
    <div class="col-md-6">  
        <div id="terms" class="modal" role="dialog" tabindex="-1">  
            <div class="modal-dialog">  
                <div class="modal-content">  
                    <div class="modal-header">  
                        <h4 class="modal-title">{% localized "SigninResources|DialogHeadingTermsOfUse" %}</h4>  
                    </div>  
                    <div class="modal-body break-all">{{userRegistrationTerms}}</div>  
                    <div class="modal-footer">  
                        <button type="button" class="btn btn-default" data-dismiss="modal">{% localized "CommonStrings|ButtonLabelClose" %}</button>  
                    </div>  
                </div>  
            </div>  
        </div>  
        <p>{% localized "SigninResources|TextblockUserRegistrationTermsProvided" %}</p>  
    </div>  
    {% endif %}  
</div>  
```  
  
### <a name="controls"></a>Denetimler  
 Bu şablon aşağıdaki [sayfa denetimlerini](api-management-page-controls.md)kullanabilir.  
  
-   [temel oturum açma](api-management-page-controls.md#basic-signin)  
  
-   [Sağlayıcı](api-management-page-controls.md#providers)  
  
### <a name="data-model"></a>Veri modeli  
 [Kullanıcı varlığında oturum açın.](api-management-template-data-model-reference.md#UseSignIn)  
  
### <a name="sample-template-data"></a>Örnek şablon verileri  
  
```json  
{
    "Email": null,
    "Password": null,
    "ReturnUrl": null,
    "RememberMe": false,
    "RegistrationEnabled": true,
    "DelegationEnabled": false,
    "DelegationUrl": null,
    "SsoSignUpUrl": null,
    "AuxServiceUrl": "https://portal.azure.com/#resource/subscriptions/{subscription ID}/resourceGroups/Api-Default-West-US/providers/Microsoft.ApiManagement/service/contoso5",
    "Providers": [  
        {  
            "Properties": {  
                "AuthenticationType": "Aad",  
                "Caption": "Azure Active Directory"  
            },  
            "AuthenticationType": "Aad",  
            "Caption": "Azure Active Directory"  
        }  
        ],
    "UserRegistrationTerms": null,
    "UserRegistrationTermsEnabled": false
}
```  
  
##  <a name="sign-up"></a><a name="SignUp"></a>Kaydol  
 **Kaydolma** şablonu, geliştirici portalındaki kayıt sayfasını özelleştirmenize olanak tanır.  
  
 ![Kayıt Sayfası](./media/api-management-page-templates/APIM-Sign-Up-Page-Developer-Portal-Templates.png "APIM Kayıt Sayfası Geliştirici Portal Şablonları")  
  
### <a name="default-template"></a>Varsayılan şablon  
  
```xml  
<h2 class="text-center">{% localized "SignupStrings|PageTitleSignup" %}</h2>  
<p class="text-center">  
  {% localized "SignupStrings|WebAuthenticationAlreadyAMember" %} <a href="/signin">{% localized "SignupStrings|WebAuthenticationSigninNow" %}</a>  
</p>  
  
<div class="row center-block ap-idp-container">  
  <div class="col-md-6">  
    <p>{% localized "SignupStrings|WebAuthenticationCreateNewAccount" %}</p>  
    <sign-up></sign-up>  
  </div>  
</div>  
```  
  
### <a name="controls"></a>Denetimler  
 Bu şablon aşağıdaki [sayfa denetimlerini](api-management-page-controls.md)kullanabilir.  
  
-   [Kayıt](api-management-page-controls.md#sign-up)  
  
### <a name="data-model"></a>Veri modeli  
 [Kullanıcı varlık kaydolun.](api-management-template-data-model-reference.md#UserSignUp)  
  
### <a name="sample-template-data"></a>Örnek şablon verileri  
  
```json  
{  
    "PasswordConfirm": null,  
    "Password": null,  
    "PasswordVerdictLevel": 0,  
    "UserRegistrationTerms": null,  
    "UserRegistrationTermsOptions": 0,  
    "ConsentAccepted": false,  
    "Email": null,  
    "FirstName": null,  
    "LastName": null,  
    "UserData": null,  
    "NameIdentifier": null,  
    "ProviderName": null  
}  
```  
  
##  <a name="page-not-found"></a><a name="PageNotFound"></a>Sayfa bulunamadı  
 **Şablon bulunamadı sayfa,** geliştirici portalında bulunmayan sayfayı özelleştirmenize olanak tanır.  
  
 ![Bulunamadı Sayfa](./media/api-management-page-templates/APIM-Not-Found-Page-Developer-Portal-Templates.png "APIM Bulunamadı Sayfa Geliştirici Portal Şablonları")  
  
### <a name="default-template"></a>Varsayılan şablon  
  
```xml  
<h2>{% localized "NotFoundStrings|PageTitleNotFound" %}</h2>  
  
<h3>{% localized "NotFoundStrings|TitlePotentialCause" %}</h3>  
<ul>  
  <li>{% localized "NotFoundStrings|TextblockPotentialCauseOldLink" %}</li>  
  <li>{% localized "NotFoundStrings|TextblockPotentialCauseMisspelledUrl" %}</li>  
</ul>  
  
<h3>{% localized "NotFoundStrings|TitlePotentialSolution" %}</h3>  
<ul>  
  <li>{% localized "NotFoundStrings|TextblockPotentialSolutionRetype" %}</li>  
  <li>  
    {% capture textPotentialSolutionStartOver %}{% localized "NotFoundStrings|TextblockPotentialSolutionStartOver" %}{% endcapture %}  
    {% capture homeLink %}<a href="/">{% localized "NotFoundStrings|LinkLabelHomePage" %}</a>{% endcapture %}  
    {% assign replaceString = '{0}' %}  
  
    {{ textPotentialSolutionStartOver | replace : replaceString, homeLink }}  
  </li>  
</ul>  
  
<p>  
  {% capture textReportProblem %}{% localized "NotFoundStrings|TextReportProblem" %}{% endcapture %}  
  {% capture emailLink %}<a href="mailto:apimgmt@microsoft.com" target="_self" title="API Management Support">{% localized "NotFoundStrings|LinkLabelSendUsEmail" %}</a>{% endcapture %}  
  {% assign replaceString = '{0}' %}  
  
  {{ textReportProblem | replace : replaceString, emailLink }}  
</p>  
```  
  
### <a name="controls"></a>Denetimler  
 Bu şablon herhangi bir [sayfa denetimi](api-management-page-controls.md)kullanmayabilir.  
  
### <a name="data-model"></a>Veri modeli  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|referenceCode|string|Bu sayfa bir iç hata sonucu görüntüleniyorsa oluşturulan kod.|  
|hataKodu|string|Bu sayfa bir iç hata sonucu görüntüleniyorsa oluşturulan kod.|  
|emailBody|string|Bu sayfa bir iç hata sonucu görüntüleniyorsa oluşturulan e-posta gövdesi.|  
|istenenUrl|string|Sayfa bulunamadında istenen URL.|  
|referrerUrl|string|İstenen URL'ye yönlendiren URL.|  
  
### <a name="sample-template-data"></a>Örnek şablon verileri  
  
```json  
{  
    "referenceCode": null,  
    "errorCode": null,  
    "emailBody": null,  
    "requestedUrl": "https://contoso5.portal.azure-api.net:443/NotFoundPage?startEditTemplate=NotFoundPage",  
    "referrerUrl": "https://contoso5.portal.azure-api.net/signup?startEditTemplate=SignUpTemplate"  
}  
```

## <a name="next-steps"></a>Sonraki adımlar
Şablonlarla çalışma hakkında daha fazla bilgi için, [şablonları kullanarak API Yönetimi geliştirici portalını nasıl özelleştireceğime](api-management-developer-portal-templates.md)bakın.
