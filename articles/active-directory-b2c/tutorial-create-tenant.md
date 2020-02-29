---
title: Öğretici-Azure Active Directory B2C kiracı oluşturma
description: Azure portal kullanarak Azure Active Directory B2C kiracı oluşturarak uygulamalarınızı kaydetmeye nasıl hazırlanacağınızı öğrenin.
services: B2C
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/28/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c31f3c4c6688af7d2142180e8d9b7100965bad96
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78186412"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Öğretici: Azure Active Directory B2C kiracı oluşturma

Uygulamalarınızın Azure Active Directory B2C (Azure AD B2C) ile etkileşime girebilmesi için, yönettiğiniz bir kiracıda kayıtlı olmaları gerekir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C kiracısı oluşturma
> * Kiracınızı aboneliğinize bağlama
> * Azure AD B2C kiracınızı içeren dizine geçiş yapın
> * Azure AD B2C kaynağını Azure portal **Sık kullanılanlara** ekleme

Bir uygulamayı sonraki öğreticide nasıl kaydedeceğinizi öğrenirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-an-azure-ad-b2c-tenant"></a>Azure AD B2C kiracısı oluşturma

1. [Azure Portal](https://portal.azure.com/) oturum açın. Abonelik içinde en azından [katılımcı](../role-based-access-control/built-in-roles.md) rolü veya abonelik içindeki bir kaynak grubu atanmış bir Azure hesabıyla oturum açın.

1. Aboneliğinizi içeren dizini seçin.

    Azure portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından aboneliğinizi içeren dizini seçin. Bu dizin Azure AD B2C kiracınızı içerecek olan dizinden farklıdır.

    ![Abonelik kiracısı, Dizin + abonelik kiracısı seçiliyken abonelik filtresi](media/tutorial-create-tenant/portal-01-pick-directory.png)

1. Azure portal menüsünde veya **giriş** sayfasında, **kaynak oluştur**' u seçin.
1. **Azure Active Directory B2C**araması yapın ve ardından **Oluştur**' u seçin.
1. **Yeni Azure AD B2C Kiracısı oluştur**’u seçin.

    ![Azure portal ' de seçili yeni bir Azure AD B2C kiracı oluşturun](media/tutorial-create-tenant/portal-02-create-tenant.png)

1. Bir **kuruluş adı** ve **ilk etki alanı adı**girin. **Ülke veya bölgeyi** seçin (daha sonra değiştirilemez) ve ardından **Oluştur**' u seçin.

    Etki alanı adı, tam kiracı etki alanı adınızın bir parçası olarak kullanılır. Bu örnekte, kiracı adı *contosob2c.onmicrosoft.com*' dir:

    ![İçinde kiracı formunu örnek değerlerle oluşturun Azure portal](media/tutorial-create-tenant/portal-03-tenant-naming.png)

1. Kiracı oluşturma işlemi tamamlandıktan sonra, kiracı oluşturma sayfasının en üstündeki **yenı B2C kiracısı oluştur ' u veya mevcut kiracı** bağlantısı bağlantısını seçin.

    ![Azure portal bağlantı kiracı içerik haritası bağlantısı vurgulandı](media/tutorial-create-tenant/portal-04-select-link-sub-link.png)

1. **Mevcut bir Azure AD B2C kiracıyı Azure aboneliğime bağla**' yı seçin.

   ![Azure portal mevcut bir abonelik seçimini bağlama](media/tutorial-create-tenant/portal-05-link-subscription.png)

1. Oluşturduğunuz **Azure AD B2C kiracıyı** seçin, sonra **aboneliğinizi**seçin.

    **Kaynak grubu** için **Yeni oluştur**’u seçin. Kiracıyı içerecek kaynak grubu için bir **ad** girin, **kaynak grubu konumunu**seçin ve ardından **Oluştur**' u seçin.

    ![Azure portal abonelik ayarları formu bağlantısı](media/tutorial-create-tenant/portal-06-link-subscription-settings.png)

    Faturalama amacıyla birden çok Azure AD B2C kiracıyı tek bir Azure aboneliğine bağlayabilirsiniz.

## <a name="select-your-b2c-tenant-directory"></a>B2C kiracı dizininizi seçin

Yeni Azure AD B2C kiracınızı kullanmaya başlamak için kiracıyı içeren dizine geçmeniz gerekir.

Azure portal üst menüsünde **Dizin + abonelik** filtresi ' ni seçin ve Azure AD B2C kiracınızı içeren dizini seçin.

İlk olarak listede yeni Azure B2C kiracınızı görmüyorsanız tarayıcı pencerenizi yenileyin ve sonra üst menüdeki **Dizin + abonelik** filtresini yeniden seçin.

![B2C kiracısı-Azure portal ' de seçili dizin](media/tutorial-create-tenant/portal-07-select-tenant-directory.png)

## <a name="add-azure-ad-b2c-as-a-favorite-optional"></a>Sık kullanılan olarak Azure AD B2C Ekle (isteğe bağlı)

İsteğe bağlı bu adım, Azure AD B2C kiracınızı aşağıda ve sonraki tüm öğreticilerde seçmenizi kolaylaştırır.

Kiracınızla birlikte çalışmak istediğiniz her **durumda** *Azure AD B2C* için arama yapmak yerine kaynağı sık kullanılanlara ekleyebilirsiniz. Daha sonra, Azure AD B2C kiracınıza hızlıca gitmek için Portal menüsünün **Sık Kullanılanlar** bölümünden seçebilirsiniz.

Bu işlemi yalnızca bir kez gerçekleştirmeniz gerekir. Bu adımları gerçekleştirmeden önce, önceki bölümde açıklandığı gibi Azure AD B2C kiracınızı içeren dizine geçdiğinizden emin olun, [B2C kiracı dizininizi seçin](#select-your-b2c-tenant-directory).

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure portal menüsünde **tüm hizmetler**' i seçin.
1. **Tüm hizmetler** arama kutusunda **Azure AD B2C**arayın, arama sonucunun üzerine gelin ve ardından araç ipucunda yıldız simgesini seçin. **Azure AD B2C** artık **sık kullanılanlar**altındaki Azure Portal görüntülenir.
1. Yeni sık kullanılanlarınızın konumunu değiştirmek istiyorsanız, Azure portal menüsüne gidin, **Azure AD B2C**' i seçin ve ardından istediğiniz konuma aşağı veya aşağı sürükleyin.

    ![Azure AD B2C, Sık Kullanılanlar menüsü, Microsoft Azure portal](media/tutorial-create-tenant/portal-08-b2c-favorite.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, şu şekilde nasıl yapılacağını öğrendiniz:

> [!div class="checklist"]
> * Azure AD B2C kiracısı oluşturma
> * Kiracınızı aboneliğinize bağlama
> * Azure AD B2C kiracınızı içeren dizine geçiş yapın
> * Azure AD B2C kaynağını Azure portal **Sık kullanılanlara** ekleme

Ardından, yeni kiracınızda bir Web uygulamasını nasıl kaydedeceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Uygulamalarınızı kaydedin >](tutorial-register-applications.md)
