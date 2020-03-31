---
title: PIM'i yönetmeye erişim izni verme - Azure Active Directory | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'ni (PIM) yönetmek için diğer yönetimlere nasıl erişim izni verdiğinizi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7424e92f8520d13137b6ac8787523095058a005f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74022122"
---
# <a name="grant-access-to-other-administrators-to-manage-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimini yönetmek için diğer yöneticilere erişim izni verme

Bir kuruluş için Ayrıcalıklı Kimlik Yönetimi'ne (PIM) olanak tanıyan Global yönetici, otomatik olarak rol atamaları ve Ayrıcalıklı Kimlik Yönetimi'ne erişim sağlar. Azure Etkin Dizin (Azure AD) kuruluşunuzdaki hiç kimse, diğer Global yöneticiler de dahil olmak üzere varsayılan olarak yazma erişimi almaz. Diğer Global yöneticiler, Güvenlik yöneticileri ve Güvenlik okuyucuları ayrıcalıklı kimlik yönetimine salt okunur erişime sahiptir. Ayrıcalıklı Kimlik Yönetimi'ne erişim sağlamak için, ilk kullanıcı diğerlerini **Ayrıcalıklı Rol Yöneticisi** rolüne atayabilir.

> [!NOTE]
> Ayrıcalıklı Kimlik Yönetimi'ni yönetmek için Azure Çok Faktörlü Kimlik Doğrulama gerekir. Microsoft hesapları Azure Çok Faktörlü Kimlik Doğrulama'ya kaydolamadığı için, microsoft hesabıyla giriş yapan bir kullanıcı Ayrıcalıklı Kimlik Yönetimi'ne erişemez.

Bir kullanıcının kilitlenmiş olması veya hesabının silinmesi durumunda, Ayrıcalıklı Rol Yöneticisi rolünde her zaman en az iki kullanıcı olduğundan emin olun.

## <a name="grant-access-to-manage-pim"></a>PIM'yi yönetmeye erişim izni verme

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. Azure AD'de **Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure AD rollerini**seçin.

1. **Roller'i**seçin.

    ![Ayrıcalıklı Kimlik Yönetimi Azure AD rolleri - Roller](./media/pim-how-to-give-access-to-pim/pim-directory-roles-roles.png)

1. Üyeler sayfasını açmak için **Ayrıcalıklı Rol Yöneticisi** rolünü seçin.

    ![Ayrıcalıklı Rol Yöneticisi - Üyeler](./media/pim-how-to-give-access-to-pim/pim-pra-members.png)

1. Yönetilen üye ekle bölmesini açmak için **üye ekle'yi** seçin.

1. Üyeleri Seç bölmesini açmak için **üyeleri seç'i** seçin.

    ![Ayrıcalıklı Rol Yöneticisi - Üye seçin](./media/pim-how-to-give-access-to-pim/pim-pra-select-members.png)

1. Bir üye seçin ve sonra **Seç'i**tıklatın.

1. Üyenin **Ayrıcalıklı Rol Yöneticisi** rolüne uygun olması için **Tamam'ı** seçin.

    Ayrıcalıklı Kimlik Yönetimi'ndeki birine yeni bir rol atadığınızda, bu rol otomatik olarak rolü etkinleştirmek için **Uygun** olarak yapılandırılır.

1. Üyeyi kalıcı hale getirmek için, Ayrıcalıklı Rol Yöneticisi üye listesindeki kullanıcıyı seçin.

1. **Atamayı** kalıcı hale getirmek için Daha Fazla'yı seçin ve sonra **kalıcı olun.**

    ![Ayrıcalıklı Rol Yöneticisi - Kalıcı olun](./media/pim-how-to-give-access-to-pim/pim-pra-make-permanent.png)

1. Kullanıcıya [Ayrıcalıklı Kimlik Yönetimini Kullanmaya Başla](pim-getting-started.md)bağlantısı gönderin.

## <a name="remove-access-to-manage-pim"></a>PIM'i yönetmek için erişimi kaldırma

Birini Ayrıcalıklı Rol Yöneticisi rolünden kaldırmadan önce, her zaman en az iki kullanıcının bu roliçin atandığından emin olun.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure AD rollerini**seçin.

1. **Roller'i**seçin.

1. Üyeler sayfasını açmak için **Ayrıcalıklı Rol Yöneticisi** rolünü seçin.

1. Kaldırmak istediğiniz kullanıcının yanındaki onay kutusunu seçin ve ardından **Üyeyi Kaldır'ı**seçin.

    ![Ayrıcalıklı Rol Yöneticisi - Üyeyi Kaldır](./media/pim-how-to-give-access-to-pim/pim-pra-remove-member.png)

1. Üyeyi rolden kaldırmak istediğinizi onaylamanız istendiğinde **Evet'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management'ı kullanmaya başlama](pim-getting-started.md)
