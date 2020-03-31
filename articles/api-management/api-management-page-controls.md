---
title: Azure API Yönetimi sayfa denetimleri | Microsoft Dokümanlar
description: Azure API Yönetimi'ndeki geliştirici portal şablonlarında kullanılmak üzere kullanılabilen sayfa denetimleri hakkında bilgi edinin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244023"
---
# <a name="azure-api-management-page-controls"></a>Azure API Yönetimi sayfa denetimleri
Azure API Yönetimi, geliştirici portalı şablonlarında kullanılmak üzere aşağıdaki denetimleri sağlar.  
  
Denetimi kullanmak için, geliştirici portalı şablonunda istenilen konuma yerleştirin. [Uygulama eylemleri](#app-actions) denetimi gibi bazı denetimlerde, aşağıdaki örnekte gösterildiği gibi parametreler vardır:  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
Parametrelerin değerleri şablon için veri modelinin bir parçası olarak geçirilir. Çoğu durumda, doğru çalışması için her denetim için sağlanan örneğe yapıştırabilirsiniz. Parametre değerleri hakkında daha fazla bilgi için, denetimin kullanılabildiği her şablon için veri modeli bölümünü görebilirsiniz.  

Şablonlarla çalışma hakkında daha fazla bilgi için, [şablonları kullanarak API Yönetimi geliştirici portalını nasıl özelleştireceğime](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/)bakın.  

[!INCLUDE [api-management-portal-legacy.md](../../includes/api-management-portal-legacy.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]
  
## <a name="developer-portal-template-page-controls"></a>Geliştirici portalı şablon uymaktadır sayfa denetimleri  
  
-   [uygulama eylemleri](#app-actions)  
-   [temel oturum açma](#basic-signin)  
-   [sayfalama-kontrol](#paging-control)  
-   [Sağlayıcı](#providers)  
-   [arama denetimi](#search-control)  
-   [Kayıt](#sign-up)  
-   [abone düğmesi](#subscribe-button)  
-   [abonelik iptal](#subscription-cancel)  
  
##  <a name="app-actions"></a><a name="app-actions"></a>uygulama eylemleri  
 Denetim, `app-actions` geliştirici portalındaki kullanıcı profili sayfasındaki uygulamalarla etkileşim kurmak için bir kullanıcı arabirimi sağlar.  
  
 ![uygulama&#45;eylem kontrolü](./media/api-management-page-controls/APIM-app-actions-control.png "APIM uygulama eylemleri kontrolü")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
### <a name="parameters"></a>Parametreler  
  
|Parametre|Açıklama|  
|---------------|-----------------|  
|appId|Başvurunun kimliği.|  
  
### <a name="developer-portal-templates"></a>Geliştirici portalı şablonları  
 Denetim `app-actions` aşağıdaki geliştirici portalı şablonlarında kullanılabilir:  
  
-   [Uygulamalar](api-management-user-profile-templates.md#Applications)  
  
##  <a name="basic-signin"></a><a name="basic-signin"></a>temel oturum açma  
 Denetim, `basic-signin` geliştirici portalındaki oturum açma sayfasında kullanıcı oturum açma bilgilerini toplamak için bir denetim sağlar.  
  
 ![temel&#45;işaret kontrolü](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM temel oturum açma kontrolü")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<basic-SignIn></basic-SignIn>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici portalı şablonları  
 Denetim `basic-signin` aşağıdaki geliştirici portalı şablonlarında kullanılabilir:  
  
-   [Oturum aç](api-management-page-templates.md#SignIn)  
  
##  <a name="paging-control"></a><a name="paging-control"></a>sayfalama-kontrol  
 Öğelerin `paging-control` listesini görüntüleyen geliştirici portal sayfalarında sayfalama işlevi sağlar.  
  
 ![sayfalama kontrolü](./media/api-management-page-controls/APIM-paging-control.png "APIM sayfalama kontrolü")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<paging-control></paging-control>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici portalı şablonları  
 Denetim `paging-control` aşağıdaki geliştirici portalı şablonlarında kullanılabilir:  
  
-   [API listesi](api-management-api-templates.md#APIList)  
  
-   [Sorun listesi](api-management-issue-templates.md#IssueList)  
  
-   [Ürün listesi](api-management-product-templates.md#ProductList)  
  
##  <a name="providers"></a><a name="providers"></a>Sağlayıcı  
 Denetim, `providers` geliştirici portalındaki oturum açma sayfasında kimlik doğrulama sağlayıcılarının seçimi için bir denetim sağlar.  
  
 ![sağlayıcılar kontrol](./media/api-management-page-controls/APIM-providers-control.png "APIM sağlayıcıları kontrolü")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<providers></providers>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici portalı şablonları  
 Denetim `providers` aşağıdaki geliştirici portalı şablonlarında kullanılabilir:  
  
-   [Oturum aç](api-management-page-templates.md#SignIn)  
  
##  <a name="search-control"></a><a name="search-control"></a>arama denetimi  
 Öğelerin `search-control` listesini görüntüleyen geliştirici portal sayfalarında arama işlevselliği sağlar.  
  
 ![arama denetimi](./media/api-management-page-controls/APIM-search-control.png "APIM arama kontrolü")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<search-control></search-control>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici portalı şablonları  
 Denetim `search-control` aşağıdaki geliştirici portalı şablonlarında kullanılabilir:  
  
-   [API listesi](api-management-api-templates.md#APIList)  
  
-   [Ürün listesi](api-management-product-templates.md#ProductList)  
  
##  <a name="sign-up"></a><a name="sign-up"></a>Kayıt  
 Denetim, `sign-up` geliştirici portalındaki kayıt sayfasında kullanıcı profili bilgilerini toplamak için bir denetim sağlar.  
  
 ![&#45;kayıt kontrolü](./media/api-management-page-controls/APIM-sign-up-control.png "APIM kayıt kontrolü")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<sign-up></sign-up>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici portalı şablonları  
 Denetim `sign-up` aşağıdaki geliştirici portalı şablonlarında kullanılabilir:  
  
-   [Kaydolma](api-management-page-templates.md#SignUp)  
  
##  <a name="subscribe-button"></a><a name="subscribe-button"></a>abone düğmesi  
 Kullanıcıyı `subscribe-button` bir ürüne abone etmek için bir denetim sağlar.  
  
 ![&#45;düğmesine abone olun](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM abone düğmesi kontrolü")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<subscribe-button></subscribe-button>  
```  
  
### <a name="parameters"></a>Parametreler  
 Yok.  
  
### <a name="developer-portal-templates"></a>Geliştirici portalı şablonları  
 Denetim `subscribe-button` aşağıdaki geliştirici portalı şablonlarında kullanılabilir:  
  
-   [Ürün](api-management-product-templates.md#Product)  
  
##  <a name="subscription-cancel"></a><a name="subscription-cancel"></a>abonelik iptal  
 Denetim, `subscription-cancel` geliştirici portalındaki kullanıcı profili sayfasındaki bir ürünün aboneliğini iptal etmek için bir denetim sağlar.  
  
 ![denetimi iptal&#45;abonelik](./media/api-management-page-controls/APIM-subscription-cancel-control.png "APIM abonelik-iptal kontrolü")  
  
### <a name="usage"></a>Kullanım  
  
```xml  
<subscription-cancel params="{ subscriptionId: '{{subscription.id}}', cancelUrl: '{{subscription.cancelUrl}}' }">  
</subscription-cancel>  
  
```  
  
### <a name="parameters"></a>Parametreler  
  
|Parametre|Açıklama|  
|---------------|-----------------|  
|subscriptionId|İptal etmek için aboneliğin kimliği.|  
|cancelUrl|Abonelik URL'yi iptal eder.|  
  
### <a name="developer-portal-templates"></a>Geliştirici portalı şablonları  
 Denetim `subscription-cancel` aşağıdaki geliştirici portalı şablonlarında kullanılabilir:  
  
-   [Ürün](api-management-product-templates.md#Product)

## <a name="next-steps"></a>Sonraki adımlar
Şablonlarla çalışma hakkında daha fazla bilgi için, [şablonları kullanarak API Yönetimi geliştirici portalını nasıl özelleştireceğime](api-management-developer-portal-templates.md)bakın.
