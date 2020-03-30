---
title: Azure portalındaki Azure AD B2C tüketici kullanıcı hesaplarını silmek & oluşturun
description: Azure AD B2C dizininizde tüketici kullanıcıları oluşturmak ve silmek için Azure portalını nasıl kullanacağınızı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/09/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c8fad8b5bd2dccba36c0c6888712fa806f3cbfc8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78187228"
---
# <a name="use-the-azure-portal-to-create-and-delete-consumer-users-in-azure-ad-b2c"></a>Azure AD B2C'de tüketici kullanıcıları oluşturmak ve silmek için Azure portalını kullanın

Azure Active Directory B2C (Azure AD B2C) dizininizde tüketici hesaplarını el ile oluşturmak istediğiniz senaryolar olabilir. Azure AD B2C dizinindeki tüketici hesapları en yaygın olarak, kullanıcılar uygulamalarınızdan birini kullanmak için kaydolduklarında oluşturulsa da, bunları programlı ve Azure portalını kullanarak oluşturabilirsiniz. Bu makalede, kullanıcı oluşturma ve silme Azure portalı yöntemi üzerinde duruluyor.

Kullanıcı eklemek veya silmek için hesabınıza *Kullanıcı yöneticisi* veya Global *yönetici* rolü atanmalıdır.

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="types-of-user-accounts"></a>Kullanıcı hesabı türleri

[Azure AD B2C'deki kullanıcı hesaplarının genel görünümünde](user-overview.md)açıklandığı gibi, Bir Azure AD B2C dizininde oluşturulabilecek üç tür kullanıcı hesabı vardır:

* İş
* Konuk
* Tüketici

Bu makale, Azure portalındaki **tüketici hesaplarıyla** çalışmaya odaklanır. İş ve Konuk hesapları oluşturma ve silme hakkında bilgi için [Azure Etkin Dizini'ni kullanarak kullanıcı ekle veya silme](../active-directory/fundamentals/add-users-azure-active-directory.md)bölümüne bakın.

## <a name="create-a-consumer-user"></a>Tüketici kullanıcısı oluşturma

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
1. **Yeni kullanıcıyı**seçin.
1. **Azure AD B2C kullanıcıoluştur'u**seçin.
1. Oturum **Açma yöntemini** seçin ve yeni kullanıcı için bir **E-posta** adresi veya **Kullanıcı adı** girin. Burada seçtiğiniz oturum açma yöntemi, Azure AD B2C kiracınızın *Yerel hesap* kimlik sağlayıcısı için belirttiğiniz ayarla eşleştirmelidir (bkz. Azure AD B2C kiracınızda**Kimlik sağlayıcılarını** **yönet).** > 
1. Kullanıcı için bir **Ad** girin. Bu genellikle kullanıcının tam adı (verilen ve soyadı) olduğunu.
1. (İsteğe bağlı) Kullanıcının oturum açma özelliğini geciktirmek istiyorsanız **oturum açma'yı engelleyebilirsiniz.** Azure portalında kullanıcının **Profilini** düzenleyerek daha sonra oturum açmayı etkinleştirebilirsiniz.
1. **Otomatik oluşturma parolası** seçin veya **parola oluşturmama izin verin.**
1. Kullanıcının **Adı** ve **Soyadını**belirtin.
1. **Oluştur'u**seçin.

**Oturum aç'ı engellemeyi**seçmediğiniz sürece, kullanıcı artık belirttiğiniz oturum açma yöntemini (e-posta veya kullanıcı adı) kullanarak oturum açabilir.

## <a name="delete-a-consumer-user"></a>Bir tüketici kullanıcıyı silme

1. Azure AD B2C dizininizde **Kullanıcılar'ı**seçin ve ardından silmek istediğiniz kullanıcıyı seçin.
1. Silme işlemini onaylamak için **Sil'i**ve ardından **Evet'i** seçin.

Silme işleminden sonraki ilk 30 gün içinde bir kullanıcıyı geri yükleme veya bir kullanıcıyı kalıcı olarak silme hakkında daha fazla bilgi için, [Azure Etkin Dizinini kullanarak yeni silinen bir kullanıcıyı geri yükle'ye](../active-directory/fundamentals/active-directory-users-restore.md)bakın veya kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

Otomatik kullanıcı yönetimi senaryoları (örneğin, kullanıcıları başka bir kimlik sağlayıcısından Azure AD B2C dizininize geçirmek için [bkz.](user-migration.md)
