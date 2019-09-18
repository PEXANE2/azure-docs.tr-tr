---
title: Öğretici-uygulamayı kaydetme-Azure Active Directory B2C
description: Azure portal kullanarak bir Web uygulamasını Azure Active Directory B2C nasıl kaydedeceğinizi öğrenin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 08/23/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 06b2c273f41bfa74ee968b6fd6676e83767ce8b2
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71063263"
---
# <a name="tutorial-register-an-application-in-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C bir uygulamayı kaydetme

[Uygulamalarınızın](active-directory-b2c-apps.md) Azure Active Directory B2C (Azure AD B2C) ile etkileşime girebilmesi için, yönettiğiniz bir kiracıda kayıtlı olmaları gerekir. Bu öğreticide, Azure portal kullanarak bir Web uygulamasının nasıl kaydedileceği gösterilmektedir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Web uygulaması kaydetme
> * İstemci parolası oluşturma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Kendi [Azure AD B2C kiracınızı](tutorial-create-tenant.md)önceden oluşturmadıysanız, şimdi bir tane oluşturun. Mevcut bir Azure AD B2C kiracı kullanabilirsiniz.

## <a name="register-a-web-application"></a>Web uygulaması kaydetme

1. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
1. **Uygulamalar**' ı seçin ve ardından **Ekle**' yi seçin.
1. Uygulama için bir ad girin. Örneğin, *WebApp1*.
1. **Web uygulaması/Web API 'Si Ekle** ve **örtük akışa Izin ver**için **Evet**' i seçin.
1. **Yanıt URL 'si**için Azure AD B2C uygulamanızın isteklerinizin belirteçleri döndürmesi gereken bir uç nokta girin. Örneğin, bunu yerel olarak `https://localhost:44316`dinlemek için ayarlayabilirsiniz. Bağlantı noktası numarasını henüz bilmiyorsanız, bir yer tutucu değeri girip daha sonra değiştirebilirsiniz.

    Bu öğreticide olduğu gibi test amacıyla, inceleme için bir `https://jwt.ms` belirtecin içeriğini görüntüleyen olarak ayarlayabilirsiniz. Bu öğreticide, **yanıt URL** 'sini olarak `https://jwt.ms`ayarlayın.

    Yanıt URL 'Leri için aşağıdaki kısıtlamalar geçerlidir:

    * Yanıt URL 'SI, şemayla `https`başlamalıdır.
    * Yanıt URL 'SI, büyük/küçük harfe duyarlıdır. Büyük/küçük harf durumu, çalışan uygulamanızın URL yolu ile aynı olmalıdır. Örneğin, uygulamanız yolunun `.../abc/response-oidc`bir parçası olarak içeriyorsa, yanıt URL 'sinde belirtmeyin. `.../ABC/response-oidc` Web tarayıcısı yollara büyük/küçük harfe duyarlı olarak davrandığı için, bununla `.../abc/response-oidc` ilişkili tanımlama bilgileri, büyük/küçük harfe eşleşmeyen `.../ABC/response-oidc` URL 'ye yönlendiriliyorsa dışlanamaz.

1. Uygulama kaydını tamamlayabilmeniz için **Oluştur** ' a tıklayın.

## <a name="create-a-client-secret"></a>İstemci parolası oluşturma

Uygulamanız belirteç için bir kod alışverişi yaptıysanız, bir uygulama gizli anahtarı oluşturmanız gerekir.

1. **Azure AD B2C-uygulamalar** sayfasında, oluşturduğunuz uygulamayı seçin, örneğin, *WebApp1*.
1. **Anahtarlar** ' ı seçin ve ardından **anahtar oluştur**' u seçin.
1. Anahtarı görüntülemek için **Kaydet** ' i seçin. **Uygulama anahtarı** değerini not edin. Bu değeri, uygulamanızın kodunda uygulama gizli anahtarı olarak kullanırsınız.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, şu şekilde nasıl yapılacağını öğrendiniz:

> [!div class="checklist"]
> * Web uygulaması kaydetme
> * İstemci parolası oluşturma

Ardından, kullanıcılarınızın kaydolmalarına, oturum açmalarına ve profillerini yönetmesine olanak tanımak için Kullanıcı akışları oluşturmayı öğrenin.

> [!div class="nextstepaction"]
> [Azure Active Directory B2C > Kullanıcı akışları oluşturma](tutorial-create-user-flows.md)
