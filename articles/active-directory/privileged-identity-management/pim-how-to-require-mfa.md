---
title: Multi-Factor Authentication (MFA) ve Privileged Identity Management-Azure Active Directory | Microsoft Docs
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
ms.openlocfilehash: 12b6cd17fac77361fa20d3b3e048e64228a2ed3d
ms.sourcegitcommit: ec2b75b1fc667c4e893686dbd8e119e7c757333a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72809017"
---
# <a name="multi-factor-authentication-and-privileged-identity-management"></a>Multi-Factor Authentication ve Privileged Identity Management

Tüm yöneticileriniz için Multi-Factor Authentication (MFA) gerektirmenizi öneririz. Bu, güvenliği aşılmış bir parola nedeniyle saldırının riskini azaltır.

Kullanıcıların oturum açtıklarında bir Multi = Factor Authentication sınamasını tamamlamasını zorunlu kılabilirsiniz. Ayrıca, Azure Active Directory (Azure AD) Privileged Identity Management (PıM) ' de bir rolü etkinleştirdiklerinde kullanıcıların Multi = Factor Authentication sınamasını tamamlamasını de isteyebilirsiniz. Bu şekilde, Kullanıcı oturum açtıklarında bir Multi = Factor Authentication sınaması tamammadıysa, Privileged Identity Management tarafından bunu yapması istenir.

> [!IMPORTANT]
> Azure Multi-Factor Authentication, Microsoft kişisel hesapları değil yalnızca iş veya okul hesaplarıyla çalışır (genellikle Skype, Xbox veya Outlook.com gibi Microsoft hizmetlerinde oturum açmak için kullanılan bir kişisel hesaptır). Bu nedenle, kişisel hesap kullanan herkes, rollerini etkinleştirmek için Multi-Factor Authentication 'ı kullanamadığından uygun yönetici olamaz. Bu kullanıcıların bir Microsoft hesabı kullanarak iş yüklerini yönetmeye devam etmesi gerekiyorsa, bunları şimdilik kalıcı yöneticilerle yükseltin.

## <a name="how-pim-validates-mfa"></a>PıM, MFA 'yı nasıl doğrular

Kullanıcı bir rolü etkinleştirdiğinde Multi-Factor Authentication 'ı doğrulamaya yönelik iki seçenek vardır.

En basit seçenek, ayrıcalıklı bir rolü etkinleştiren kullanıcılar için Azure Multi-Factor Authentication 'yi temel alır. Bunu yapmak için önce bu kullanıcıların lisanslanmış olup olmadığını ve Azure Multi-Factor Authentication için kaydolmadığını kontrol edin. Azure Multi-Factor Authentication dağıtma hakkında daha fazla bilgi için bkz. [bulut tabanlı azure Multi-Factor Authentication dağıtma](../authentication/howto-mfa-getstarted.md). Azure AD 'yi, oturum açtıklarında bu kullanıcılar için Multi-Factor Authentication 'ı zorlamak üzere yapılandırmanız önerilir, ancak gerekli değildir. Bunun nedeni, Multi-Factor Authentication denetimlerinin Privileged Identity Management tarafından yapılması olacaktır.

Alternatif olarak, kullanıcılar şirket içinde kimlik doğrulamasından getir, kimlik sağlayıcınızın Multi-Factor Authentication 'dan sorumlu olmasını sağlayabilirsiniz. Örneğin, Azure AD 'ye erişmeden önce akıllı kart tabanlı kimlik doğrulaması gerektirmek için AD Federasyon Hizmetleri 'ni yapılandırdıysanız, [bulut kaynaklarının azure Multi-Factor Authentication ve AD FS güvenliğini](../authentication/howto-mfa-adfs.md) sağlamak için AD FS yapılandırma yönergelerini içerir. talepleri Azure AD 'ye gönderin. Bir Kullanıcı bir rolü etkinleştirmeye çalıştığında, uygun talepler alındıktan sonra Privileged Identity Management, Kullanıcı için bu Multi-Factor Authentication 'ın zaten doğrulanmasını kabul eder.

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management Azure AD rol ayarlarını yapılandırma](pim-how-to-change-default-settings.md)
- [Privileged Identity Management Azure Kaynak rolü ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
