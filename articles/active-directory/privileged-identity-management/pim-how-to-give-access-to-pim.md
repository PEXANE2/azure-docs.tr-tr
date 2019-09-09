---
title: PıM-Azure Active Directory yönetmek için diğer yöneticilere erişim verme | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) yönetmek için diğer yönetimlere nasıl erişim sağlayacağınızı öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: f3a0173108b6c884994ca25fd0495e9cb8d45186
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804355"
---
# <a name="grant-access-to-other-administrators-to-manage-pim"></a>PıM 'yi yönetmek için diğer yöneticilere erişim izni verme

Bir kuruluş için Azure Active Directory (Azure AD) Privileged Identity Management (PıM) etkinleştiren genel yönetici, rol atamalarını otomatik olarak alır ve PıM 'ye erişin. Diğer genel yöneticiler dahil, ancak varsayılan olarak yazma erişimi alınmaz. Diğer genel Yöneticiler, güvenlik yöneticileri ve güvenlik okuyucuları PıM 'ye salt okuma erişimi vardır. PıM 'ye erişim izni vermek için, ilk Kullanıcı diğerlerini **ayrıcalıklı rol yöneticisi** rolüne atayabilir.

> [!NOTE]
> PıM 'yi yönetmek için Azure MFA gerekir. Microsoft hesapları Azure MFA için kaydettirilemediğinden, Microsoft hesabı ile oturum açan bir Kullanıcı PıM 'ye erişemez.

Bir kullanıcının kilitli olması veya hesaplarının silinmesi durumunda, ayrıcalıklı rol yöneticisi rolünde her zaman en az iki kullanıcı bulunduğundan emin olun.

## <a name="grant-access-to-manage-pim"></a>PıM 'yi yönetmek için erişim izni verme

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure AD rolleri**' ne tıklayın.

1. **Roller**' e tıklayın.

    ![PıM Azure AD rolleri-roller](./media/pim-how-to-give-access-to-pim/pim-directory-roles-roles.png)

1. Üyeler sayfasını açmak için **ayrıcalıklı rol yöneticisi** rolüne tıklayın.

    ![Ayrıcalıklı rol yöneticisi-Üyeler](./media/pim-how-to-give-access-to-pim/pim-pra-members.png)

1. Yönetilen Üyeler Ekle bölmesini açmak için **üye Ekle** ' ye tıklayın.

1. Üyeleri Seç bölmesini açmak için **üyeleri Seç** ' e tıklayın.

    ![Ayrıcalıklı rol yöneticisi-üyeleri seçin](./media/pim-how-to-give-access-to-pim/pim-pra-select-members.png)

1. Bir üye seçip **Seç**' e tıklayın.

1. Üyeyi **ayrıcalıklı rol yöneticisi** rolüne uygun hale getirmek için **Tamam** ' ı tıklatın.

    PıM 'de birine yeni bir rol atadığınızda, bu, rolü etkinleştirmek için **uygun** şekilde otomatik olarak yapılandırılır.

1. Üyeyi kalıcı hale getirmek için ayrıcalıklı rol yöneticisi üye listesinden kullanıcıya tıklayın.

1. **Daha fazla** ' ya tıklayın ve sonra atamanın kalıcı hale getirmek Için **kalıcı hale getirin** .

    ![Ayrıcalıklı rol yöneticisi-kalıcı yap](./media/pim-how-to-give-access-to-pim/pim-pra-make-permanent.png)

1. Kullanıcıya [PIM 'yi kullanmaya başlamak](pim-getting-started.md)için bir bağlantı gönderin.

## <a name="remove-access-to-manage-pim"></a>PıM 'yi yönetmek için erişimi kaldır

Ayrıcalıklı rol Yöneticisi rolünden birini kaldırmadan önce, en az iki kullanıcı atanmış olduğundan emin olun.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure AD rolleri**' ne tıklayın.

1. **Roller**' e tıklayın.

1. Üyeler sayfasını açmak için **ayrıcalıklı rol yöneticisi** rolüne tıklayın.

1. Kaldırmak istediğiniz kullanıcının yanına bir onay işareti ekleyin ve ardından **üyeyi kaldır**' a tıklayın.

    ![Ayrıcalıklı rol yöneticisi-üyeyi kaldır](./media/pim-how-to-give-access-to-pim/pim-pra-remove-member.png)

1. Üyeyi rolden kaldırmak isteyip istemediğinizi soran görüntülenen iletide, **Evet**' e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [PIM kullanmaya başlama](pim-getting-started.md)
