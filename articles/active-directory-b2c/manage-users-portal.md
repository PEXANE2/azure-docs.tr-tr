---
title: Azure portal Azure AD B2C tüketici Kullanıcı hesapları oluşturma & silme
description: Azure AD B2C dizininizde tüketici kullanıcılarını oluşturmak ve silmek için Azure portal nasıl kullanacağınızı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 11/09/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 5d6d6169f8662c9b973fb7f624a590322f62b0b5
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85387533"
---
# <a name="use-the-azure-portal-to-create-and-delete-consumer-users-in-azure-ad-b2c"></a>Azure AD B2C ' de tüketici kullanıcıları oluşturmak ve silmek için Azure portal kullanın

Azure Active Directory B2C (Azure AD B2C) dizininizde tüketici hesaplarını el ile oluşturmak istediğiniz senaryolar olabilir. Bir Azure AD B2C dizindeki tüketici hesapları en yaygın olarak, kullanıcılar uygulamalarınızdan birini kullanmak üzere kaydolduğunuzda, bunları programlı bir şekilde ve Azure portal kullanarak oluşturabilirsiniz. Bu makalede, Kullanıcı oluşturma ve silmenin Azure portal yöntemi ele alınmaktadır.

Kullanıcı eklemek veya silmek için hesabınıza *Kullanıcı Yöneticisi* veya *genel yönetici* rolü atanmalıdır.

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="types-of-user-accounts"></a>Kullanıcı hesabı türleri

[Azure AD B2C Kullanıcı hesaplarına genel bakış](user-overview.md)bölümünde açıklandığı gibi, bir Azure AD B2C dizininde oluşturulabilecek üç tür Kullanıcı hesabı vardır:

* İş
* Konuk
* İsini

Bu makale, Azure portal **Tüketici hesaplarıyla** çalışmaya odaklanır. Iş ve Konuk hesapları oluşturma ve silme hakkında daha fazla bilgi için, bkz. [Azure Active Directory kullanarak Kullanıcı ekleme veya silme](../active-directory/fundamentals/add-users-azure-active-directory.md).

## <a name="create-a-consumer-user"></a>Tüketici kullanıcısı oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Sol menüden **Azure AD B2C**' yi seçin. Ya da **tüm hizmetler** ' i seçin ve **Azure AD B2C**seçin.
1. **Yönet** bölümünde **Kullanıcılar**'ı seçin.
1. **Yeni Kullanıcı**' yı seçin.
1. **Azure AD B2C kullanıcı oluştur**' u seçin.
1. **Oturum açma yöntemi** seçin ve Yeni Kullanıcı Için bir **e-posta** adresi ya da **Kullanıcı adı** girin. Burada seçtiğiniz oturum açma yöntemi, Azure AD B2C kiracının *yerel hesap* kimliği sağlayıcısı için belirttiğiniz ayarla aynı olmalıdır (bkz **Manage**  >  . Azure AD B2C kiracınızdaki**kimlik sağlayıcılarını** yönetme).
1. Kullanıcı için bir **ad** girin. Bu genellikle kullanıcının tam adı (verilen ve soyadı).
1. Seçim Kullanıcının oturum açma yeteneğini geciktirmek istiyorsanız, **oturum açmayı engelleyebilirsiniz** . Daha sonra Azure portal kullanıcının **profilini** düzenleyerek oturum açmayı etkinleştirebilirsiniz.
1. **Parolayı otomatik oluştur** ' ı veya **parola oluşturmama izin ver**' i seçin.
1. Kullanıcının **adını** ve **soyadını**belirtin.
1. **Oluştur**'u seçin.

**Engelle oturum açma**seçeneğini seçmediyseniz, Kullanıcı artık belirttiğiniz oturum açma yöntemini (e-posta veya Kullanıcı adı) kullanarak oturum açabilir.

## <a name="delete-a-consumer-user"></a>Tüketici kullanıcısını silme

1. Azure AD B2C dizininizde **Kullanıcılar**' ı seçin ve ardından silmek istediğiniz kullanıcıyı seçin.
1. **Sil**' i ve sonra silme işlemini onaylamak için **Evet** ' i seçin.

Silinmeden önce ilk 30 gün içinde bir kullanıcıyı geri yükleme veya bir kullanıcıyı kalıcı olarak silme hakkında ayrıntılar için, bkz. [Azure Active Directory kullanarak son silinen bir kullanıcıyı geri yükleme veya kaldırma](../active-directory/fundamentals/active-directory-users-restore.md).

## <a name="next-steps"></a>Sonraki adımlar

Otomatik Kullanıcı yönetimi senaryolarında, örneğin başka bir kimlik sağlayıcısından Kullanıcı Azure AD B2C dizininize geçiş yapmak için, bkz. [Azure AD B2C: Kullanıcı geçişi](user-migration.md).
