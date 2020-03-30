---
title: Öğretici - Azure Etkin Dizin B2C kiracı oluşturma
description: Azure portalını kullanarak bir Azure Active Directory B2C kiracısı oluşturarak uygulamalarınızı kaydetmeye nasıl hazırlanacağınızı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78186412"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Öğretici: Azure Active Directory B2C kiracısı oluşturma

Uygulamalarınız Azure Active Directory B2C (Azure AD B2C) ile etkileşime girebilmesi için, bunların yönettiğiniz bir kiracıya kaydedilmesi gerekir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C kiracısı oluşturma
> * Kiracınızı aboneliğinize bağlama
> * Azure AD B2C kiracınızı içeren dizine geçme
> * Azure portalında **Sık Kullanılan** olarak Azure AD B2C kaynağını ekleyin

Bir sonraki öğreticide bir uygulamayı nasıl kaydedebilirsiniz öğrenirsiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-an-azure-ad-b2c-tenant"></a>Azure AD B2C kiracısı oluşturma

1. [Azure portalında](https://portal.azure.com/)oturum açın. Abonelik teki en az [Katılımcı](../role-based-access-control/built-in-roles.md) rolüne veya abonelik içindeki bir kaynak grubuna atanmış bir Azure hesabıyla oturum açın.

1. Aboneliğinizi içeren dizini seçin.

    Azure portalı araç çubuğunda **Dizin + Abonelik** simgesini seçin ve ardından aboneliğinizi içeren dizin'i seçin. Bu dizin, Azure AD B2C kiracınızı içeren dizinden farklıdır.

    ![Abonelik kiracı, Dizin + Abonelik filtresi abonelik kiracı seçilen](media/tutorial-create-tenant/portal-01-pick-directory.png)

1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**’u seçin.
1. Azure **Etkin Dizin B2C'yi**arayın ve ardından **Oluştur'u**seçin.
1. **Yeni Azure AD B2C Kiracısı oluştur**’u seçin.

    ![Azure portalında seçilen yeni bir Azure AD B2C kiracısı oluşturun](media/tutorial-create-tenant/portal-02-create-tenant.png)

1. Bir **Kuruluş adı** ve **İlk etki alanı adı**girin. Ülke **veya bölgeyi** seçin (daha sonra değiştirilemez) ve sonra **Oluştur'u**seçin.

    Alan adı, tam kiracı alan adınızın bir parçası olarak kullanılır. Bu örnekte, kiracı adı *contosob2c.onmicrosoft.com:*

    ![Azure portalında örnek değerlerle kiracı formu oluşturma](media/tutorial-create-tenant/portal-03-tenant-naming.png)

1. Kiracı oluşturma tamamlandıktan sonra, kiracı oluşturma sayfasının üst **kısmındaki yeni B2C Kiracı oluştur veya varolan Kiracı bağlantısına bağlantı** oluşturun'u seçin.

    ![Azure portalında vurgulanan kiracı kırıntısı bağlantısını bağlantı](media/tutorial-create-tenant/portal-04-select-link-sub-link.png)

1. **Varolan bir Azure AD B2C Kiracısını Azure aboneliğime**bağla'yı seçin.

   ![Azure portalında varolan bir abonelik seçimini bağlama](media/tutorial-create-tenant/portal-05-link-subscription.png)

1. Oluşturduğunuz **Azure AD B2C Kiracı'yı** seçin ve ardından **Aboneliğinizi**seçin.

    **Kaynak grubu** için **Yeni oluştur**’u seçin. Kiracıyı içerecek kaynak grubu **için** ad girin, Kaynak **grup konumunu**seçin ve sonra **Oluştur'u**seçin.

    ![Azure portalında abonelik ayarlarını bağlama](media/tutorial-create-tenant/portal-06-link-subscription-settings.png)

    Birden çok Azure AD B2C kiracısını faturalandırma amacıyla tek bir Azure aboneliğine bağlayabilirsiniz.

## <a name="select-your-b2c-tenant-directory"></a>B2C kiracı dizini seçin

Yeni Azure AD B2C kiracınızı kullanmaya başlamak için kiracıyı içeren dizine geçmeniz gerekir.

Azure portalının üst menüsünde **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.

İlk başta yeni Azure B2C kiracınızı listede görmüyorsanız, tarayıcı pencerenizi yenileyin ve üst menüde **dizin + abonelik** filtresini yeniden seçin.

![Azure portalında seçilen B2C kiracı içeren dizini](media/tutorial-create-tenant/portal-07-select-tenant-directory.png)

## <a name="add-azure-ad-b2c-as-a-favorite-optional"></a>Azure AD B2C'yi sık kullanılan olarak ekleyin (isteğe bağlı)

Bu isteğe bağlı adım, aşağıdaki ve sonraki tüm öğreticilerde Azure AD B2C kiracınızı seçmenizi kolaylaştırır.

Kiracınızla çalışmak istediğiniz her seferde **Tüm hizmetlerde** *Azure AD B2C'yi* aramak yerine, kaynağınızı sıkabilirsiniz. Ardından, Azure AD B2C kiracınıza hızlıca göz atmak için portal menüsünün **Sık Kullanılanlar** bölümünden seçebilirsiniz.

Bu işlemi yalnızca bir kez gerçekleştirmeniz gerekir. Bu adımları gerçekleştirmeden önce, önceki bölümde açıklandığı gibi Azure AD B2C kiracınızı içeren dizine geçtiğinizden emin olun, [B2C kiracı dizininizi seçin.](#select-your-b2c-tenant-directory)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Azure portalı menüsünde **Tüm hizmetler'i**seçin.
1. Tüm **hizmetler** arama kutusunda, **Azure AD B2C'yi**arayın , arama sonucunun üzerine tıklayın ve ardından araç ucundaki yıldız simgesini seçin. **Azure AD B2C** artık Azure portalında **Sık Kullanılanlar**altında görünür.
1. Yeni favorinizin konumunu değiştirmek istiyorsanız, Azure portal menüsüne gidin, **Azure AD B2C'yi**seçin ve ardından istediğiniz konuma yukarı veya aşağı sürükleyin.

    ![Azure AD B2C, Sık Kullanılanlar menüsü, Microsoft Azure portalı](media/tutorial-create-tenant/portal-08-b2c-favorite.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, nasıl öğrendim:

> [!div class="checklist"]
> * Azure AD B2C kiracısı oluşturma
> * Kiracınızı aboneliğinize bağlama
> * Azure AD B2C kiracınızı içeren dizine geçme
> * Azure portalında **Sık Kullanılan** olarak Azure AD B2C kaynağını ekleyin

Ardından, yeni kiracınızda bir web uygulamasını nasıl kaydedebilirsiniz öğrenin.

> [!div class="nextstepaction"]
> [Başvurularınızı >kaydedin](tutorial-register-applications.md)
