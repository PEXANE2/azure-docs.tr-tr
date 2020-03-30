---
title: 'Öğretici: Bir uygulamayı kaydedin'
titleSuffix: Azure AD B2C
description: Azure portalını kullanarak Azure Active Directory B2C'de bir web uygulamasını nasıl kaydedebilirsiniz öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 10/16/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: a688f5e75f7513d0ea4308b751f87f75a2c9510a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78183100"
---
# <a name="tutorial-register-an-application-in-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C'de bir uygulama kaydedin

[Uygulamalarınız](application-types.md) Azure Active Directory B2C (Azure AD B2C) ile etkileşime girebilmesi için, bunların yönettiğiniz bir kiracıya kaydedilmesi gerekir. Bu öğretici, Azure portalını kullanarak bir web uygulamasını nasıl kaydedebilirsiniz gösterir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Web uygulaması kaydetme
> * İstemci sırrı oluşturma

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Kendi [Azure AD B2C Kiracınızı](tutorial-create-tenant.md)henüz oluşturmadıysanız, şimdi bir tane oluşturun. Varolan bir Azure AD B2C kiracısı kullanabilirsiniz.

## <a name="register-a-web-application"></a>Web uygulaması kaydetme

Azure AD B2C kiracınızda bir uygulama kaydettirmek için geçerli **Uygulamalar** deneyimini veya yeni birleştirilmiş **Uygulama kayıtları (Önizleme)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin.](https://aka.ms/b2cappregintro)

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Portal araç çubuğundaki **Dizin + Abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure **portalında, Azure AD B2C'yi**arayın ve seçin.
1. **Uygulamalar'ı**seçin ve sonra **Ekle'yi**seçin.
1. Uygulama için bir ad girin. Örneğin, *webapp1*.
1. **Web app/ web API ekle** ve **örtük akışa izin vermek**için **Evet'i**seçin.
1. **YanıtURL'si için,** Azure AD B2C'nin uygulamanızın istediği belirteçleri döndürmesi gereken bir bitiş noktası girin. Örneğin, `https://localhost:44316`yerel olarak dinlemek için ayarlayabilirsiniz. Bağlantı noktası numarasını henüz bilmiyorsanız, bir yer tutucu değeri girebilir ve daha sonra değiştirebilirsiniz.

    Bu öğretici gibi test amaçları için, denetim için bir belirteç içeriğini görüntüler `https://jwt.ms` hangi ayarlayabilirsiniz. Bu öğretici için **YanıtLA** `https://jwt.ms`URL'sini .

    Url'leri yanıtlamak için aşağıdaki kısıtlamalar geçerlidir:

    * Yanıt URL'si şema `https`ile başlamalıdır.
    * Yanıt URL'si büyük/küçük harf duyarlıdır. Bu durumda çalışan uygulamanızın URL yolu durumda eşleşmesi gerekir. Örneğin, uygulamanız yolunun `.../abc/response-oidc`bir parçası olarak içeriyorsa, yanıt URL'sinde belirtmeyin. `.../ABC/response-oidc` Web tarayıcısı yolları büyük/küçük harf duyarlı `.../abc/response-oidc` olarak ele aldığı için, büyük/küçük harf eşleşmesi `.../ABC/response-oidc` url'ye yönlendirilirse ilişkili tanımlama bilgileri hariç tutulabilir.

1. Uygulama kaydını tamamlamak için **Oluştur'u** seçin.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Portal araç çubuğundaki **Dizin + Abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure **portalında, Azure AD B2C'yi**arayın ve seçin.
1. **Uygulama kayıtlarını (Önizleme)** seçin ve ardından **Yeni kayıt'ı**seçin.
1. Uygulama için bir **Ad** girin. Örneğin, *webapp1*.
1. **Herhangi bir kuruluş dizininde veya herhangi bir kimlik sağlayıcısında Hesapları**seçin.
1. **Uri'yi Yönlendirme**altında **Web'i**seçin ve ardından URL metin kutusuna girin. `https://jwt.ms`

    Yeniden yönlendirme URI, kullanıcıyla etkileşimini tamamladıktan sonra kullanıcının yetkilendirme sunucusu (Azure AD B2C, bu durumda) tarafından gönderildiği ve başarılı yetkilendirme üzerine bir erişim belirteci veya yetkilendirme kodunun gönderildiği bitiş noktasıdır. Bir üretim uygulamasında, genellikle uygulamanızın çalıştığı genel olarak erişilebilir bir bitiş `https://contoso.com/auth-response`noktasıdır. Bu öğretici gibi sınama amacıyla, `https://jwt.ms`bir belirteç (belirteç içeriği tarayıcınızdan asla çıkmadan) deşifre içeriğini görüntüleyen Microsoft'a ait bir web uygulaması olarak ayarlayabilirsiniz. Uygulama geliştirme sırasında, uygulamanızın yerel olarak dinlediği bitiş `https://localhost:5000`noktasını ekleyebilirsiniz, örneğin. Kayıtlı uygulamalarınızda istediğiniz zaman yönlendirme URI'leri ekleyebilir ve değiştirebilirsiniz.

    Uri'leri yeniden yönlendirmek için aşağıdaki kısıtlamalar geçerlidir:

    * Yanıt URL'si şema `https`ile başlamalıdır.
    * Yanıt URL'si büyük/küçük harf duyarlıdır. Bu durumda çalışan uygulamanızın URL yolu durumda eşleşmesi gerekir. Örneğin, uygulamanız yolunun `.../abc/response-oidc`bir parçası olarak içeriyorsa, yanıt URL'sinde belirtmeyin. `.../ABC/response-oidc` Web tarayıcısı yolları büyük/küçük harf duyarlı `.../abc/response-oidc` olarak ele aldığı için, büyük/küçük harf eşleşmesi `.../ABC/response-oidc` url'ye yönlendirilirse ilişkili tanımlama bilgileri hariç tutulabilir.

1. **İzinler**altında, *openid ve offline_access izinleri* onay kutusunu açmak için Grant yöneticisi onayını seçin.
1. **Kaydol**’u seçin.

Başvuru kaydı tamamlandıktan sonra, örtülü hibe akışını etkinleştirin:

1. **Yönet**altında, **Kimlik Doğrulama'yı**seçin.
1. **Yeni deneyimi deneyin (gösteriliyorsa)** seçin.
1. **Örtülü hibe**altında, hem **Erişim belirteçleri** hem de **kimlik belirteçleri** onay kutularını seçin.
1. **Kaydet'i**seçin.

* * *

## <a name="create-a-client-secret"></a>İstemci sırrı oluşturma

Uygulamanız bir erişim jetonu için bir yetkilendirme kodu değiştirirse, bir uygulama sırrı oluşturmanız gerekir.

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. Azure **AD B2C - Uygulamalar** sayfasında, oluşturduğunuz uygulamayı seçin( örneğin *webapp1.*
1. **Tuşları** seçin ve ardından **Oluştur tuşunu**seçin.
1. Anahtarı görüntülemek için **Kaydet'i** seçin. **Uygulama anahtarı** değerini not edin. Bu değeri uygulamanızın kodunda uygulama sırrı olarak kullanırsınız.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. Azure **AD B2C - Uygulama kayıtları (Önizleme)** sayfasında, oluşturduğunuz uygulamayı seçin( örneğin *webapp1.*
1. **Yönet**altında, **Sertifikalar & sırları**seçin.
1. **Yeni istemci gizli dizisi**’ni seçin.
1. **Açıklama** kutusuna istemci sırrı için bir açıklama girin. Örneğin, *istemcisecret1*.
1. **Sona Erme ler**altında, sırrın geçerli olduğu bir süre seçin ve sonra **Ekle'yi**seçin.
1. Sırrın **Değerini**kaydedin. Bu değeri uygulamanızın kodunda uygulama sırrı olarak kullanırsınız.

* * *

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, nasıl öğrendim:

> [!div class="checklist"]
> * Web uygulaması kaydetme
> * İstemci sırrı oluşturma

Ardından, kullanıcılarınızın kaydolmasını, oturum kurmasını ve profillerini yönetmesini sağlamak için kullanıcı akışlarının nasıl oluşturulmasını öğrenin.

> [!div class="nextstepaction"]
> [Azure Active Directory B2C >kullanıcı akışları oluşturma](tutorial-create-user-flows.md)
