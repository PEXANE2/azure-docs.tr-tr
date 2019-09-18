---
title: Öğretici-Azure Active Directory B2C kiracı oluşturma
description: Azure portal kullanarak Azure Active Directory B2C kiracı oluşturarak uygulamalarınızı kaydetmeye nasıl hazırlanacağınızı öğrenin.
services: B2C
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 06/07/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 57aed26fe2f2f03dca347836f21d1cad14a07642
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71063341"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Öğretici: Azure Active Directory B2C kiracı oluşturma

Uygulamalarınızın Azure Active Directory B2C (Azure AD B2C) ile etkileşime girebilmesi için, yönettiğiniz bir kiracıda kayıtlı olmaları gerekir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C kiracısı oluşturma
> * Kiracınızı aboneliğinize bağlama

Bir uygulamayı sonraki öğreticide nasıl kaydedeceğinizi öğrenirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-an-azure-ad-b2c-tenant"></a>Azure AD B2C kiracısı oluşturma

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Aboneliğinizi içeren dizini kullandığınızdan emin olun. Üstteki menüden **Dizin + abonelik** filtresini seçin ve ardından aboneliğinizi içeren dizini seçin. Bu dizin Azure AD B2C kiracınızı içerecek olan dizinden farklıdır.

    ![Abonelik kiracısı seçiliyken dizin ve abonelik filtresi](./media/tutorial-create-tenant/switch-directory-subscription.PNG)

3. Azure portal, sol üst köşedeki **kaynak oluştur ' a** tıklayın.
4. **Active Directory B2C**bulun ve seçin ve ardından **Oluştur**' a tıklayın.
5. **Yeni Azure AD B2C kiracı oluştur ' a** tıklayın ve bir kuruluş adı ve ilk etki alanı adı girin. Ülke/bölge seçin (daha sonra değiştirilemez) ve ardından **Oluştur**' a tıklayın.

    İlk etki alanı adı, kiracı adınızın bir parçası olarak kullanılır. Bu örnekte, kiracı adı *contoso0926Tenant.onmicrosoft.com*' dir:

    ![B2C kiracı oluşturma sayfası Azure portal](./media/tutorial-create-tenant/create-tenant.PNG)

6. **Yenı B2C kiracısı oluştur veya mevcut kiracıya bağla** sayfasında, **mevcut bir Azure AD B2C kiracıyı Azure aboneliğime bağla**' yı seçin.

    Oluşturduğunuz kiracıyı seçin ve aboneliğinizi seçin.

    Kaynak grubu için **Yeni oluştur**' u seçin. Kiracıyı içerecek kaynak grubu için bir ad girin, konumu seçin ve ardından **Oluştur**' a tıklayın.
1. Yeni kiracınızı kullanmaya başlamak için, üst menüdeki **Dizin + abonelik** filtresini seçip onu içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.

    İlk olarak listede yeni Azure B2C kiracınızı görmüyorsanız tarayıcı pencerenizi yenileyin ve sonra üst menüdeki **Dizin + abonelik** filtresini yeniden seçin.

    ![B2C kiracısı seçiliyken dizin ve abonelik filtresi](./media/tutorial-create-tenant/switch-directories.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, şu şekilde nasıl yapılacağını öğrendiniz:

> [!div class="checklist"]
> * Azure AD B2C kiracısı oluşturma
> * Kiracınızı aboneliğinize bağlama

Ardından, yeni kiracınızda bir Web uygulamasını nasıl kaydedeceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Uygulamalarınızı kaydedin >](tutorial-register-applications.md)
