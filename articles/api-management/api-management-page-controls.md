---
title: Azure API Management sayfa denetimleri | Microsoft Docs
description: Azure API Management 'de geliştirici portalı şablonlarında kullanılabilecek sayfa denetimleri hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/04/2019
ms.author: apimpm
ms.openlocfilehash: ff608cac9786ebc0d3908ce4d88fb501e621be6c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84690192"
---
# <a name="azure-api-management-page-controls"></a>Azure API Management sayfa denetimleri
Azure API Management, geliştirici portalı şablonlarında kullanılmak üzere aşağıdaki denetimleri sağlar.  
  
Bir denetimi kullanmak için, uygulamayı geliştirici portalı şablonunda istenen konuma yerleştirin. Aşağıdaki örnekte gösterildiği gibi, [uygulama eylemleri](#app-actions) denetimi gibi bazı denetimlerin parametreleri vardır:  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
Parametrelerin değerleri, şablon için veri modelinin bir parçası olarak geçirilir. Çoğu durumda, doğru çalışması için her bir denetim için yalnızca belirtilen örneğe yapıştırabilirsiniz. Parametre değerleri hakkında daha fazla bilgi için, bir denetimin kullanılabileceği her bir şablon için veri modeli bölümünü görebilirsiniz.  

Şablonlarla çalışma hakkında daha fazla bilgi için bkz. [şablonları kullanarak API Management Geliştirici Portalını Özelleştirme](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  

[!INCLUDE [api-management-portal-legacy.md](../../includes/api-management-portal-legacy.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]
  
## <a name="developer-portal-template-page-controls"></a>Geliştirici Portalı şablon sayfası denetimleri  
  
-   [Uygulama-eylemler](#app-actions)  
-   [temel-oturum açma](#basic-signin)  
-   [sayfalama denetimi](#paging-control)  
-   [sağlayıcılarla](#providers)  
-   [arama denetimi](#search-control)  
-   [Kaydolma](#sign-up)  
-   [abone ol-düğme](#subscribe-button)  
-   [abonelik-iptal et](#subscription-cancel)  
  
##  <a name="app-actions"></a><a name="app-actions"></a>Uygulama-eylemler  
 `app-actions`Denetim, geliştirici portalındaki Kullanıcı profili sayfasında uygulamalarla etkileşim kurmak için bir kullanıcı arabirimi sağlar.  
  
 ![Uygulama&#45;eylemleri denetimi](./media/api-management-page-controls/APIM-app-actions-control.png "APıM uygulaması-eylemler denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
### <a name="parameters"></a>Parametreler  
  
|Parametre|Açıklama|  
|---------------|-----------------|  
|appId|Uygulamanın KIMLIĞI.|  
  
### <a name="developer-portal-templates"></a>Geliştirici portalı şablonları  
 `app-actions`Denetim aşağıdaki geliştirici portalı şablonlarında kullanılabilir:  
  
-   [Uygulamalar](api-management-user-profile-templates.md#Applications)  
  
##  <a name="basic-signin"></a><a name="basic-signin"></a>temel-oturum açma  
 `basic-signin`Denetim, geliştirici portalındaki oturum açma sayfasında Kullanıcı oturum açma bilgilerini toplamaya yönelik bir denetim sağlar.  
  
 ![temel&#45;oturum açma denetimi](./media/api-management-page-controls/APIM-basic-signin-control.png "APıM temel-oturum açma denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<basic-SignIn></basic-SignIn>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici portalı şablonları  
 `basic-signin`Denetim aşağıdaki geliştirici portalı şablonlarında kullanılabilir:  
  
-   [Oturum aç](api-management-page-templates.md#SignIn)  
  
##  <a name="paging-control"></a><a name="paging-control"></a>sayfalama denetimi  
 , `paging-control` Bir öğe listesini görüntüleyen geliştirici portalı sayfalarında sayfalama işlevselliği sağlar.  
  
 ![sayfalama denetimi](./media/api-management-page-controls/APIM-paging-control.png "APıM sayfalama denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<paging-control></paging-control>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici portalı şablonları  
 `paging-control`Denetim aşağıdaki geliştirici portalı şablonlarında kullanılabilir:  
  
-   [API listesi](api-management-api-templates.md#APIList)  
  
-   [Sorun listesi](api-management-issue-templates.md#IssueList)  
  
-   [Ürün listesi](api-management-product-templates.md#ProductList)  
  
##  <a name="providers"></a><a name="providers"></a>sağlayıcılarla  
 `providers`Denetim, geliştirici portalındaki oturum açma sayfasında kimlik doğrulama sağlayıcılarının seçimine yönelik bir denetim sağlar.  
  
 ![sağlayıcılar denetimi](./media/api-management-page-controls/APIM-providers-control.png "APıM sağlayıcıları denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<providers></providers>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici portalı şablonları  
 `providers`Denetim aşağıdaki geliştirici portalı şablonlarında kullanılabilir:  
  
-   [Oturum aç](api-management-page-templates.md#SignIn)  
  
##  <a name="search-control"></a><a name="search-control"></a>arama denetimi  
 , `search-control` Bir öğe listesini görüntüleyen geliştirici portalı sayfalarında arama işlevi sağlar.  
  
 ![arama denetimi](./media/api-management-page-controls/APIM-search-control.png "APıM arama denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<search-control></search-control>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici portalı şablonları  
 `search-control`Denetim aşağıdaki geliştirici portalı şablonlarında kullanılabilir:  
  
-   [API listesi](api-management-api-templates.md#APIList)  
  
-   [Ürün listesi](api-management-product-templates.md#ProductList)  
  
##  <a name="sign-up"></a><a name="sign-up"></a>Kaydolma  
 `sign-up`Denetim, geliştirici portalındaki kaydolma sayfasında Kullanıcı profili bilgilerini toplamaya yönelik bir denetim sağlar.  
  
 ![&#45;denetimini imzala](./media/api-management-page-controls/APIM-sign-up-control.png "APıM kaydolma denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<sign-up></sign-up>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici portalı şablonları  
 `sign-up`Denetim aşağıdaki geliştirici portalı şablonlarında kullanılabilir:  
  
-   [Kaydol](api-management-page-templates.md#SignUp)  
  
##  <a name="subscribe-button"></a><a name="subscribe-button"></a>abone ol-düğme  
 , `subscribe-button` Bir kullanıcıya bir ürüne abone olmak için bir denetim sağlar.  
  
 ![abone ol&#45;Button denetimi](./media/api-management-page-controls/APIM-subscribe-button-control.png "APıM Subscribe-düğme denetimi")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<subscribe-button></subscribe-button>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici portalı şablonları  
 `subscribe-button`Denetim aşağıdaki geliştirici portalı şablonlarında kullanılabilir:  
  
-   [Ürün](api-management-product-templates.md#Product)  
  
##  <a name="subscription-cancel"></a><a name="subscription-cancel"></a>abonelik-iptal et  
 `subscription-cancel`Denetim, geliştirici portalındaki Kullanıcı profili sayfasında bir ürüne aboneliği iptal etmek için bir denetim sağlar.  
  
 ![abonelik&#45;iptal denetimi](./media/api-management-page-controls/APIM-subscription-cancel-control.png "APıM aboneliği-denetimi iptal et")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<subscription-cancel params="{ subscriptionId: '{{subscription.id}}', cancelUrl: '{{subscription.cancelUrl}}' }">  
</subscription-cancel>  
  
```  
  
### <a name="parameters"></a>Parametreler  
  
|Parametre|Açıklama|  
|---------------|-----------------|  
|subscriptionId|İptal edilecek aboneliğin KIMLIĞI.|  
|cancelUrl 'Si|Abonelik URL 'YI iptal eder.|  
  
### <a name="developer-portal-templates"></a>Geliştirici portalı şablonları  
 `subscription-cancel`Denetim aşağıdaki geliştirici portalı şablonlarında kullanılabilir:  
  
-   [Ürün](api-management-product-templates.md#Product)

## <a name="next-steps"></a>Sonraki adımlar
Şablonlarla çalışma hakkında daha fazla bilgi için bkz. [şablonları kullanarak API Management Geliştirici Portalını Özelleştirme](api-management-developer-portal-templates.md).
