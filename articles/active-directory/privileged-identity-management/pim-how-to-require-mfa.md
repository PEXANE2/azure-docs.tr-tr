---
title: Multi-Factor Authentication (MFA) ve PıM-Azure Active Directory | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) çok faktörlü kimlik doğrulamasını (MFA) nasıl doğrulayacağını öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 08/31/2018
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 686c39c9fb1f1ff7c0ecf068a6612f530620d5dc
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804306"
---
# <a name="multi-factor-authentication-mfa-and-pim"></a>Multi-Factor Authentication (MFA) ve PıM

Tüm yöneticileriniz için Multi-Factor Authentication (MFA) gerektirmenizi öneririz. Bu, güvenliği aşılmış bir parola nedeniyle saldırının riskini azaltır.

Kullanıcıların oturum açtıklarında MFA sınamasını tamamlamasını zorunlu kılabilirsiniz. Ayrıca, kullanıcıların Azure Active Directory (Azure AD) Privileged Identity Management (PıM) içinde bir rol etkinleştirdiklerinde MFA sınamasını tamamlamasını de isteyebilirsiniz. Bu şekilde, Kullanıcı oturum açtıklarında bir MFA sınamasını tamamlamadıysa, bu kullanıcıların bunu PıM tarafından yapması istenir.

> [!IMPORTANT]
> Azure MFA, Microsoft hesaplarıyla değil yalnızca iş veya okul hesaplarıyla çalışır (genellikle Skype, Xbox, Outlook.com vb. gibi Microsoft hizmetlerinde oturum açmak için kullanılan bir kişisel hesaptır). Bu nedenle, Microsoft hesabı kullanan herkes rollerini etkinleştirmek için MFA kullanamadığından uygun bir yönetici olamaz. Bu kullanıcıların bir Microsoft hesabı kullanarak iş yüklerini yönetmeye devam etmesi gerekiyorsa, bunları şimdilik kalıcı yöneticilerle yükseltin.

## <a name="how-pim-validates-mfa"></a>PıM, MFA 'yı nasıl doğrular

Kullanıcı bir rolü etkinleştirdiğinde MFA 'yı doğrulamak için iki seçenek vardır.

En basit seçenek, ayrıcalıklı bir rolü etkinleştiren kullanıcılar için Azure MFA 'yı temel alır. Bunu yapmak için önce bu kullanıcıların lisanslanmış olduğunu, gerekirse ve Azure MFA için kaydolmadığını denetleyin. Azure MFA 'yı dağıtma hakkında daha fazla bilgi için bkz. [bulut tabanlı Azure Multi-Factor Authentication dağıtma](../authentication/howto-mfa-getstarted.md). Azure AD 'yi, oturum açtıklarında bu kullanıcılar için MFA 'yı zorlamak üzere yapılandırmanız önerilir, ancak gerekli değildir. Bunun nedeni, MFA denetimlerinin PıM 'nin kendisi tarafından yapılmesidir.

Alternatif olarak, kullanıcılar şirket içinde kimlik doğrulamasından getir, kimlik sağlayıcınızı MFA 'dan sorumlu olabilir. Örneğin, Azure AD 'ye erişmeden önce akıllı kart tabanlı kimlik doğrulaması gerektirmek için AD Federasyon Hizmetleri 'ni yapılandırdıysanız, [bulut kaynaklarının azure Multi-Factor Authentication ve AD FS güvenliğini](../authentication/howto-mfa-adfs.md) sağlamak için AD FS yapılandırma yönergelerini içerir. talepleri Azure AD 'ye gönderin. Bir Kullanıcı bir rolü etkinleştirmeye çalıştığında, PıM, uygun talepler alındıktan sonra MFA 'nın Kullanıcı için zaten onaylanmış olduğunu kabul eder.

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de Azure AD rol ayarlarını yapılandırma](pim-how-to-change-default-settings.md)
- [PıM 'de Azure Kaynak rolü ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
