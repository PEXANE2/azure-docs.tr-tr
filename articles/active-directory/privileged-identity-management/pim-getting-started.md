---
title: PıM-Azure Active Directory kullanmaya başlama | Microsoft Docs
description: Azure portal Azure AD Privileged Identity Management (PıM) kullanmayı nasıl etkinleştireceğinizi ve kullanmaya başlamanızı öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: pim
ms.topic: conceptual
ms.workload: identity
ms.date: 04/23/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 46f6c77916c7be62247c69b12dff1982e5781aff
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82112227"
---
# <a name="start-using-privileged-identity-management"></a>Privileged Identity Management'ı kullanmaya başlama

Bu makalede Privileged Identity Management (PıM) etkinleştirmeyi ve kullanmaya başlamanızı açıklamaktadır.

Azure Active Directory (Azure AD) kuruluşunuzda erişimi yönetmek, denetlemek ve izlemek için Privileged Identity Management (PıM) kullanın. PıM ile Azure kaynakları, Azure AD kaynakları ve Office 365 ya da Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetler için gerekli ve anında erişim sağlayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Privileged Identity Management kullanmak için aşağıdaki lisanslardan birine sahip olmanız gerekir:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5

Daha fazla bilgi için bkz. [Privileged Identity Management kullanılacak lisans gereksinimleri](subscription-requirements.md).

## <a name="prepare-pim-for-azure-ad-roles"></a>Azure AD rolleri için PıM 'yi hazırlama

Dizininiz için Privileged Identity Management etkinleştirildikten sonra, Azure AD rollerini yönetmek için Privileged Identity Management hazırlayabilirsiniz.

Azure AD rolleri için aşağıdaki sırayla hazırlanmanız için önerdiğimiz görevler şunlardır:

1. [Azure AD rol ayarlarını yapılandırın](pim-how-to-change-default-settings.md).
1. [Uygun atamalar sunun](pim-how-to-add-role-to-user.md).
1. [Uygun kullanıcıların Azure AD rollerini tam zamanında etkinleştirmesine Izin verin](pim-how-to-activate-role.md).

## <a name="prepare-pim-for-azure-roles"></a>Azure rolleri için PıM 'yi hazırlama

Dizininiz için Privileged Identity Management etkinleştirdikten sonra, bir abonelikte Azure kaynak erişimi için Azure rollerini yönetmeye Privileged Identity Management hazırlanabilirsiniz.

Azure rollerine hazırlanmanız için önerdiğimiz görevler sırayla aşağıda verilmiştir:

1. [Azure kaynaklarını bulma](pim-resource-roles-discover-resources.md)
1. [Azure rol ayarlarını yapılandırın](pim-resource-roles-configure-role-settings.md).
1. [Uygun atamalar sunun](pim-resource-roles-assign-roles.md).
1. [Uygun kullanıcıların Azure rollerini tam zamanında etkinleştirmesine Izin verin](pim-resource-roles-activate-your-roles.md).

## <a name="navigate-to-your-tasks"></a>Görevlerinize gitme

Privileged Identity Management kurulduktan sonra, bunun nasıl yapılacağını öğrenebilirsiniz.

![Görevleri gösterme ve seçenekleri yönetme Privileged Identity Management gezinti penceresi](./media/pim-getting-started/pim-quickstart-tasks.png)

| Görev + Yönet | Açıklama |
| --- | --- |
| **Rollerim**  | Size atanan uygun ve etkin rollerin listesini görüntüler. Burada atanan uygun rolleri etkinleştirebilirsiniz. |
| **İsteklerim** | Uygun rol atamalarını etkinleştirmek için bekleyen isteklerinizi görüntüler. |
| **İstekleri onaylama** | Dizininizdeki, onaylamanız için belirlediğiniz kullanıcılara uygun rolleri etkinleştirme isteklerinin listesini görüntüler. |
| **Erişimi gözden geçirme** | Kendinize veya başka birine yönelik erişimi gözden geçirirken, tamamlanmak üzere atadığınız etkin erişim incelemelerini listeler. |
| **Azure AD rolleri** | Azure AD rol atamalarını yönetmek için ayrıcalıklı rol yöneticilerinin bir panosunu ve ayarlarını görüntüler. Bu pano, ayrıcalıklı rol yöneticisi olmayan kullanıcılar için devre dışıdır. Bu kullanıcılar Görünümüm adlı özel bir panoya erişebilir. Görünümüm panosu, tüm kiracı değil yalnızca panoya erişen kullanıcı hakkında bilgileri görüntüler. |
| **Azure kaynakları** | Azure Kaynak rolü atamalarını yönetmek için ayrıcalıklı rol yöneticilerinin bir panosunu ve ayarlarını görüntüler. Bu pano, ayrıcalıklı rol yöneticisi olmayan kullanıcılar için devre dışıdır. Bu kullanıcılar Görünümüm adlı özel bir panoya erişebilir. Görünümüm panosu, tüm kiracı değil yalnızca panoya erişen kullanıcı hakkında bilgileri görüntüler. |

## <a name="add-a-pim-tile-to-the-dashboard"></a>Panoya PıM kutucuğu ekleme

Privileged Identity Management açmayı kolaylaştırmak için Azure portal panonuza bir PıM kutucuğu ekleyin.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. **Tüm hizmetler** ' i seçin ve **Azure AD Privileged Identity Management** hizmeti bulun.

    ![Tüm hizmetlerde Azure AD Privileged Identity Management](./media/pim-getting-started/pim-all-services-find.png)

1. Privileged Identity Management hızlı başlangıcı ' nı seçin.

1. Privileged Identity Management hızlı başlangıç dikey penceresini panoya sabitlemek için **panoya sabitle dikey penceresini** işaretleyin.

    ![Privileged Identity Management dikey pencereyi panoya sabitlemek için raptiye simgesi](./media/pim-getting-started/pim-quickstart-pin-to-dashboard.png)

    Azure panosu 'nda şöyle bir kutucuk göreceksiniz:

    ![Panoda Privileged Identity Management hızlı başlangıç kutucuğu](./media/pim-getting-started/pim-quickstart-dashboard-tile.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management Azure AD rolleri atama](pim-how-to-add-role-to-user.md)
- [Privileged Identity Management Azure kaynak erişimini yönetme](pim-resource-roles-discover-resources.md)
