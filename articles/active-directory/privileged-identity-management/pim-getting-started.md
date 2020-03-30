---
title: PIM kullanmaya başlayın - Azure Active Directory | Microsoft Dokümanlar
description: Azure portalında Azure AD Ayrıcalıklı Kimlik Yönetimi'ni (PIM) nasıl etkinleştirdiğinizi ve kullanmaya nasıl başlayacaksınız öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: pim
ms.topic: conceptual
ms.workload: identity
ms.date: 03/13/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5cbb9b4340a7cdb9be5039722a8f75e09288ec48
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79472869"
---
# <a name="start-using-privileged-identity-management"></a>Privileged Identity Management'ı kullanmaya başlama

Ayrıcalıklı Kimlik Yönetimi (PIM) ile Azure Etkin Dizin (Azure AD) kuruluşunuzdaki erişimi yönetebilir, denetleyebilir ve izleyebilirsiniz. Bu kapsam, Azure kaynaklarına, Azure AD'ye ve Office 365 veya Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetlerine erişimi içerir.

Bu makalede, Ayrıcalıklı Kimlik Yönetimi'ni nasıl etkinleştirilip başlatılacak açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Ayrıcalıklı Kimlik Yönetimi'ni kullanmak için aşağıdaki lisanslardan birine sahip olmalısınız:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5

Daha fazla bilgi için, [Ayrıcalıklı Kimlik Yönetimi'ni kullanmak için Lisans gereksinimlerine](subscription-requirements.md)bakın.

## <a name="sign-up-pim-for-azure-ad-roles"></a>Azure AD rolleri için PIM'e kaydolun

Dizininiz için Ayrıcalıklı Kimlik Yönetimi'ni etkinleştirdikten sonra, Azure AD rollerini yönetmek için Ayrıcalıklı Kimlik Yönetimi'ne kaydolmanız gerekir.

1. **Azure AD Ayrıcalıklı Kimlik Yönetimi'ni**açın.

1. **Azure AD rollerini**seçin.

    ![Azure AD rolleri için Ayrıcalıklı Kimlik Yönetimi'ne kaydolun](./media/pim-getting-started/sign-up-pim-azure-ad-roles.png)

1. **Kaydol'u**seçin.

1. Görünen iletide, Azure AD rollerini yönetmek için Ayrıcalıklı Kimlik Yönetimi'ne kaydolmak için **Evet'i** tıklatın.

    ![Azure AD rolleri iletisi için Ayrıcalıklı Kimlik Yönetimi'ne kaydolun](./media/pim-getting-started/sign-up-pim-message.png)

    Kaydolma tamamlandığında Azure REKLAM seçenekleri etkinleştirilir. Portalı yenilemeniz gerekebilir.

    Ayrıcalıklı Kimlik Yönetimi ile korumak için Azure kaynaklarını nasıl keşfedip seçeceksiniz hakkında bilgi için, [Ayrıcalıklı Kimlik Yönetimi'nde yönetmek için Azure kaynaklarını keşfedin'e](pim-resource-roles-discover-resources.md)bakın.

## <a name="navigate-to-your-tasks"></a>Görevlerinize gitme

Ayrıcalıklı Kimlik Yönetimi kurulduktan sonra, kimlik yönetimi görevlerinizi başlatabilirsiniz.

![Görevleri ve Yönetme seçeneklerini gösteren Ayrıcalıklı Kimlik Yönetimi'nde gezinme penceresi](./media/pim-getting-started/pim-quickstart-tasks.png)

| Görev + Yönet | Açıklama |
| --- | --- |
| **Rollerim**  | Size atanan uygun ve etkin rollerin listesini görüntüler. Burada atanan uygun rolleri etkinleştirebilirsiniz. |
| **İsteklerim** | Uygun rol atamalarını etkinleştirmek için bekleyen isteklerinizi görüntüler. |
| **İstekleri onaylama** | Dizininizde onaylamak üzere atanmış kullanıcılar tarafından uygun rolleri etkinleştirme isteklerinin listesini görüntüler. |
| **Erişimi gözden geçirme** | İster kendiniz ister bir başkası için erişimi gözden geçiriyor olun, tamamlamak üzere atanmış olduğunuz etkin erişim yorumlarını listeler. |
| **Azure AD rolleri** | Azure AD rol atamalarını yönetmek için ayrıcalıklı rol yöneticilerinin bir pano ve ayarları görüntüler. Bu pano, ayrıcalıklı rol yöneticisi olmayan kullanıcılar için devre dışıdır. Bu kullanıcılar Görünümüm adlı özel bir panoya erişebilir. Görünümüm panosu, tüm kiracı değil yalnızca panoya erişen kullanıcı hakkında bilgileri görüntüler. |
| **Azure kaynakları** | Azure kaynak rol atamalarını yönetmek için ayrıcalıklı rol yöneticilerinin bir pano ve ayarları görüntüler. Bu pano, ayrıcalıklı rol yöneticisi olmayan kullanıcılar için devre dışıdır. Bu kullanıcılar Görünümüm adlı özel bir panoya erişebilir. Görünümüm panosu, tüm kiracı değil yalnızca panoya erişen kullanıcı hakkında bilgileri görüntüler. |

## <a name="add-a-pim-tile-to-the-dashboard"></a>Panoya PIM döşemesi ekleme

Ayrıcalıklı Kimlik Yönetimi'ni açmayı kolaylaştırmak için Azure portal panonuza Ayrıcalıklı Kimlik Yönetimi döşemesi ekleyin.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. **Tüm hizmetleri** seçin ve Azure AD Ayrıcalıklı **Kimlik Yönetimi** hizmetini bulun.

    ![Tüm hizmetlerde Azure AD Ayrıcalıklı Kimlik Yönetimi](./media/pim-getting-started/pim-all-services-find.png)

1. Ayrıcalıklı Kimlik Yönetimi Quickstart'ı seçin.

1. Ayrıcalıklı Kimlik Yönetimi Quickstart bıçağını panoya sabitlemek için **panoya Pin bıçağını** işaretleyin.

    ![İmtiyazlı Kimlik Yönetimi bıçağını panoya sabitlemek için pushpin simgesi](./media/pim-getting-started/pim-quickstart-pin-to-dashboard.png)

    Azure panosunda şöyle bir döşeme görürsünüz:

    ![Panelde Ayrıcalıklı Kimlik Yönetimi Quickstart döşemesi](./media/pim-getting-started/pim-quickstart-dashboard-tile.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetiminde Azure AD rollerini atama](pim-how-to-add-role-to-user.md)
- [Ayrıcalıklı Kimlik Yönetimi'nde yönetmek için Azure kaynaklarını keşfedin](pim-resource-roles-discover-resources.md)
