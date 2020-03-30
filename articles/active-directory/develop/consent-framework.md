---
title: Azure AD onay çerçevesi
titleSuffix: Microsoft identity platform
description: Azure Etkin Dizin'deki onay çerçevesi ve çok kiracılı web ve yerel istemci uygulamalarıgeliştirmeyi nasıl kolaylaştırdığını öğrenin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/30/2018
ms.author: ryanwi
ms.reviewer: zachowd, lenalepa, jesakowi
ms.custom: aaddev
ms.openlocfilehash: cb9441e6ce19094ff72e902cdeea151041ceb963
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77161161"
---
# <a name="azure-active-directory-consent-framework"></a>Azure Active Directory onay çerçevesi

Azure Etkin Dizin (Azure AD) onay çerçevesi, çok kiracılı web ve yerel istemci uygulamaları geliştirmeyi kolaylaştırır. Bu uygulamalar, bir Azure AD kiracısının kullanıcı hesapları tarafından oturum açmalarına izin verir ve bu da uygulamanın kayıtlı olduğu uygulamadan farklıdır. Ayrıca, kendi web API'larınıza ek olarak Microsoft Graph API 'lerine (Azure AD, Intune ve Office 365'teki hizmetlere erişmek için) ve diğer Microsoft hizmetlerinin API'lerine de erişmeleri gerekebilir.

Çerçeve, dizin verilerine erişmeyi içerebilecek bir uygulamanın kendi dizinine kaydolmasını isteyen bir kullanıcıya veya yöneticiye dayanmaktadır. Örneğin, bir web istemcisi uygulamasının Office 365'ten kullanıcı yla ilgili takvim bilgilerini okuması gerekiyorsa, bu kullanıcının önce istemci uygulamasını kabul etmesi gerekir. Onay verildikten sonra, istemci uygulaması kullanıcı adına Microsoft Graph API'yi arayabilir ve takvim bilgilerini gerektiği gibi kullanabilir. [Microsoft Graph API,](https://developer.microsoft.com/graph) Office 365'teki verilere (Exchange'den gelen takvimler ve iletiler, SharePoint'ten siteler ve listeler, OneDrive'dan belgeler, OneNote'tan gelen not defterleri, Planlayıcı'dan görevler ve Excel'den çalışma kitapları gibi) yanı sıra Azure AD'deki kullanıcılara ve gruplara ve daha fazla Microsoft bulut hizmetinden gelen diğer veri nesnelerine erişim sağlar.

Onay çerçevesi, OAuth 2.0 ve yetki kodu hibesi ve müşteri kimlik bilgileri hibesi gibi çeşitli akışları üzerine, genel veya gizli istemciler kullanılarak oluşturulmuştür. Azure AD, OAuth 2.0'ı kullanarak telefon, tablet, sunucu veya web uygulaması gibi birçok farklı istemci uygulaması oluşturmayı ve gerekli kaynaklara erişmenizi mümkün kılar.

OAuth2.0 yetkilendirme hibeleriyle onay çerçevesini kullanma hakkında daha fazla bilgi için, [OAuth 2.0 ve](v2-oauth2-auth-code-flow.md) Azure AD için Azure AD ve [Kimlik Doğrulama senaryolarını](authentication-scenarios.md)kullanarak web uygulamalarına erişim yetkisi için bkz. Microsoft Graph aracılığıyla Office 365'e yetkili erişim alma hakkında daha fazla bilgi için [Microsoft Graph ile Uygulama kimlik doğrulaması'na](https://developer.microsoft.com/graph/docs/authorization/auth_overview)bakın.

## <a name="consent-experience---an-example"></a>İzin deneyimi - bir örnek

Aşağıdaki adımlar, onay deneyiminin hem uygulama geliştiricisi hem de kullanıcı için nasıl çalıştığını gösterir.

1. Bir kaynağa/API'ye erişmek için belirli izinler istemesi gereken bir web istemcisi uygulamanız olduğunu varsayalım. Bu yapılandırmayı bir sonraki bölümde nasıl yapacağınızı öğreneceksiniz, ancak esasolarak Azure portalı yapılandırma zamanında izin isteklerini bildirmek için kullanılır. Diğer yapılandırma ayarları gibi, uygulamanın Azure AD kaydının bir parçası olurlar:

    ![Diğer uygulamalara izinler](./media/consent-framework/permissions.png)

1. Uygulamanızın izinlerinin güncelleştirildiğini, uygulamanın çalıştığını ve kullanıcının bu izni ilk kez kullanmak üzere olduğunu düşünün. İlk olarak, uygulamanın Azure AD'nin `/authorize` bitiş noktasından bir yetkilendirme kodu alması gerekir. Yetkilendirme kodu daha sonra yeni bir erişim elde etmek ve belirteci yenilemek için kullanılabilir.

1. Kullanıcının kimliği henüz doğrulanmazsa, Azure `/authorize` AD'nin bitiş noktası kullanıcıdan oturum açmasını ister.

    ![Kullanıcı veya yönetici Azure AD'de oturum aç](./media/consent-framework/usersignin.png)

1. Kullanıcı oturum açsın sonra, Azure AD kullanıcıya bir onay sayfası gösterilmesi gerekip gerekmediğini belirler. Bu belirleme, kullanıcının (veya kuruluşun yöneticisinin) uygulama iznini zaten verip vermediğini temel eder. Onay verilmişse, Azure AD kullanıcıdan onay ister ve çalışması için gereken izinleri görüntüler. İzin iletişim kutusunda görüntülenen izinler kümesi, Azure portalında **Temsilciler tarafından seçilen izinlerle** eşleşir.

    ![İzin iletişim kutusunda görüntülenen izinlerin bir örneğini gösterir](./media/consent-framework/consent.png)

1. Kullanıcı izin verdikten sonra, bir erişim jetonu ve yenileme jetonu elde etmek için kullanılan bir yetkilendirme kodu başvurunuza iade edilir. Bu akış hakkında daha fazla bilgi için [OAuth 2.0 yetkilendirme kodu akışına](v2-oauth2-auth-code-flow.md)bakın.

1. Yönetici olarak, kiracınızdaki tüm kullanıcılar adına bir uygulamanın temsilciizinlerini de kabul edebilirsiniz. İdari onay, kiracıdaki her kullanıcı için onay iletişim kutusunun görünmesini engeller ve yönetici rolüne sahip kullanıcılar tarafından [Azure portalında](https://portal.azure.com) yapılabilir. Hangi yönetici rollerinin devralınan izinlere izin verebileceğini öğrenmek için Azure [AD'da Yönetici rol izinlerine](../users-groups-roles/directory-assign-admin-roles.md)bakın.

    **Bir uygulamanın temsilcilik izinlerini kabul etmek için**

   1. Uygulamanız için **API izinleri** sayfasına gidin
   1. **Grant admin onayı** düğmesini tıklatın.

      ![Açık yönetici onayı için izin verme](./media/consent-framework/grant-consent.png)

   > [!IMPORTANT]
   > **ADAL.js** kullanan tek sayfalı uygulamalar (SPA) için Hibe izinleri düğmesini kullanarak açık onay verilmesi gerekmektedir. Aksi takdirde, erişim belirteci istendiğinde uygulama başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir uygulamayı çok kiracı lı olarak nasıl dönüştüreceğinizi](howto-convert-app-to-be-multi-tenant.md) görün
* Daha fazla derinlik için, [yetkilendirme kodu hibe akışı sırasında OAuth 2.0 protokol katmanında onay ın nasıl desteklenedildiğini öğrenin.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code#request-an-authorization-code)
