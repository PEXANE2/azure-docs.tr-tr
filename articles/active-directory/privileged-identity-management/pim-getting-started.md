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
ms.date: 04/09/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 018cc54a770a0d18fa9af3d93fec2f5f720ce17b
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804522"
---
# <a name="start-using-pim"></a>PIM kullanmaya başlama

Azure Active Directory (Azure AD) Privileged Identity Management (PıM) sayesinde, kuruluşunuzda erişimi yönetebilir, denetleyebilir ve izleyebilirsiniz. Azure kaynaklarına, Azure AD’ye ve Office 365 ya da Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetlerine erişim de bu kapsama dahildir.

Bu makalede, PıM 'yi etkinleştirme ve kullanmaya başlama işlemlerinin nasıl yapılacağı açıklanır.

## <a name="prerequisites"></a>Önkoşullar

PıM 'yi kullanmak için aşağıdaki lisanslardan birine sahip olmanız gerekir:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5

Daha fazla bilgi için bkz. [PIM 'yi kullanmak Için lisans gereksinimleri](subscription-requirements.md).

## <a name="first-person-to-use-pim"></a>PıM 'yi kullanmak için ilk kişi

Dizininizde PıM 'yi ilk kez kullanmaya başladıysanız, otomatik olarak dizinde [Güvenlik Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#security-administrator) ve [ayrıcalıklı rol yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rolleri atanır. Yalnızca ayrıcalıklı rol yöneticileri, kullanıcıların Azure AD rol atamalarını yönetebilir. Ayrıca, ilk bulma ve atama deneyiminde size yol gösteren [Güvenlik sihirbazını](pim-security-wizard.md) çalıştırmayı tercih edebilirsiniz.

## <a name="enable-pim"></a>PıM 'yi etkinleştir

Dizininizde PıM 'yi kullanmaya başlamak için önce PıM 'yi etkinleştirmeniz gerekir.

1. [Azure Portal](https://portal.azure.com/) , dizininizin genel yöneticisi olarak oturum açın.

    Bir dizin için PIM 'yi etkinleştirmek üzere bir Microsoft hesabı değil ( @yourdomain.com @outlook.comÖrneğin,) bir kuruluş hesabı (örneğin,) ile genel yönetici olmanız gerekir.

1. **Tüm hizmetler** ' e tıklayın ve **Azure AD Privileged Identity Management** hizmeti bulun.

    ![Tüm hizmetlerde Azure AD Privileged Identity Management](./media/pim-getting-started/pim-all-services-find.png)

1. PıM hızlı başlangıcı 'nı açmak için tıklayın.

1. Listede **PIM Için izin**' yı tıklatın.

    ![PIM 'yi etkinleştirmek için PıM 'ye onay](./media/pim-getting-started/consent-pim.png)

1. Azure MFA ile kimliğinizi doğrulamak için **kimliğimi doğrula** ' ya tıklayın. Bir hesap seçmeniz istenir.

    ![Kimliğinizi doğrulamak için bir hesap penceresi seçin](./media/pim-getting-started/pick-account.png)

1. Doğrulama için daha fazla bilgi gerekiyorsa, işlem boyunca size kılavuzluk edilir. Daha fazla bilgi için bkz. [iki adımlı doğrulamayla ilgili yardım alın](https://go.microsoft.com/fwlink/p/?LinkId=708614).

    ![Kuruluşunuzun daha fazla bilgi ihtiyacı varsa, daha fazla bilgi gerekli penceresi](./media/pim-getting-started/more-information-required.png)

    Örneğin, telefon doğrulaması sağlamanız istenebilir.

    ![Sizinle nasıl iletişim kurasoran ek güvenlik doğrulama sayfası](./media/pim-getting-started/additional-security-verification.png)

1. Doğrulama işlemini tamamladıktan sonra **onay** düğmesine tıklayın.

1. Görüntülenen iletide, PıM hizmetini kabul etmek için **Evet** ' e tıklayın.

    ![Onay işlemini tamamlamaya yönelik PıM iletisine onay](./media/pim-getting-started/consent-pim-message.png)

## <a name="sign-up-pim-for-azure-ad-roles"></a>Azure AD rolleri için PıM kaydolma

Dizininiz için PıM 'yi etkinleştirdikten sonra, Azure AD rollerini yönetmek için PıM 'ye kaydolmanız gerekir.

1. **Azure AD Privileged Identity Management**açın.

1. **Azure AD rolleri**' ne tıklayın.

    ![Azure AD rolleri için PıM kaydolma](./media/pim-getting-started/sign-up-pim-azure-ad-roles.png)

1. **Kaydol**' a tıklayın.

1. Görüntülenen iletide, Azure AD rollerini yönetmek için PıM 'ye kaydolmak üzere **Evet** ' e tıklayın.

    ![Azure AD rolleri için PıM 'yi kaydolma iletisi](./media/pim-getting-started/sign-up-pim-message.png)

    Kaydolma tamamlandığında, Azure AD seçenekleri etkinleştirilir. Portalı yenilemeniz gerekebilir.

    PıM ile korunacak Azure kaynaklarını bulma ve seçme hakkında daha fazla bilgi için bkz. [PIM 'de yönetilecek Azure kaynaklarını bulma](pim-resource-roles-discover-resources.md).

## <a name="navigate-to-your-tasks"></a>Görevlerinize gitme

PıM kurulduktan sonra kimlik yönetimi görevlerinizi gerçekleştirebilirsiniz.

![PıM 'de görevleri gösteren gezinti penceresi ve seçenekleri yönetme](./media/pim-getting-started/pim-quickstart-tasks.png)

| Görev + Yönet | Açıklama |
| --- | --- |
| **Rollerim**  | Size atanan uygun ve etkin rollerin listesini görüntüler. Burada atanan uygun rolleri etkinleştirebilirsiniz. |
| **İsteklerim** | Uygun rol atamalarını etkinleştirmek için bekleyen isteklerinizi görüntüler. |
| **İstekleri onaylama** | Dizininizdeki, onaylamanız için belirlediğiniz kullanıcılara uygun rolleri etkinleştirme isteklerinin listesini görüntüler. |
| **Erişimi gözden geçir** | Kendinize veya başka birine yönelik erişimi gözden geçirirken, tamamlanmak üzere atadığınız etkin erişim incelemelerini listeler. |
| **Azure AD rolleri** | Azure AD rol atamalarını yönetmek için ayrıcalıklı rol yöneticilerinin bir panosunu ve ayarlarını görüntüler. Bu pano, ayrıcalıklı rol yöneticisi olmayan kullanıcılar için devre dışıdır. Bu kullanıcılar Görünümüm adlı özel bir panoya erişebilir. Görünümüm panosu, tüm kiracı değil yalnızca panoya erişen kullanıcı hakkında bilgileri görüntüler. |
| **Azure kaynakları** | Azure Kaynak rolü atamalarını yönetmek için ayrıcalıklı rol yöneticilerinin bir panosunu ve ayarlarını görüntüler. Bu pano, ayrıcalıklı rol yöneticisi olmayan kullanıcılar için devre dışıdır. Bu kullanıcılar Görünümüm adlı özel bir panoya erişebilir. Görünümüm panosu, tüm kiracı değil yalnızca panoya erişen kullanıcı hakkında bilgileri görüntüler. |

## <a name="add-a-pim-tile-to-the-dashboard"></a>Panoya PıM kutucuğu ekleme

PıM 'yi açmayı kolaylaştırmak için Azure portal panonuza bir PıM kutucuğu eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. **Tüm hizmetler** ' e tıklayın ve **Azure AD Privileged Identity Management** hizmeti bulun.

    ![Tüm hizmetlerde Azure AD Privileged Identity Management](./media/pim-getting-started/pim-all-services-find.png)

1. PıM hızlı başlangıcı 'nı açmak için tıklayın.

1. PıM hızlı başlangıç dikey penceresini panoya sabitlemek için **panoya sabitle dikey penceresini** işaretleyin.

    ![PıM dikey penceresini panoya sabitlemek için raptiye simgesi](./media/pim-getting-started/pim-quickstart-pin-to-dashboard.png)

    Azure panosu 'nda şöyle bir kutucuk göreceksiniz:

    ![Panoda PıM hızlı başlangıç kutucuğu](./media/pim-getting-started/pim-quickstart-dashboard-tile.png)

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de Azure AD rolleri atama](pim-how-to-add-role-to-user.md)
- [PıM 'de yönetilecek Azure kaynaklarını bulma](pim-resource-roles-discover-resources.md)
