---
title: MFA veya 2FA ve Ayrıcalıklı Kimlik Yönetimi - Azure AD | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nin (PIM) çok faktörlü kimlik doğrulamayı (MFA) nasıl doğruladığını öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6df593909c3ae5962e413eb84e64196fade0326b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74022149"
---
# <a name="multi-factor-authentication-and-privileged-identity-management"></a>Çok faktörlü kimlik doğrulama ve Ayrıcalıklı Kimlik Yönetimi

Tüm yöneticileriniz için çok faktörlü kimlik doğrulama (MFA) yapmanızı öneririz. Bu, gizliliği ihlal edilen bir parola nedeniyle saldırı riskini azaltır.

Kullanıcıların oturum açtıklarında çok faktörlü bir kimlik doğrulama yarışmasını tamamlamalarını gerektirebilirsiniz. Ayrıca, kullanıcıların Azure Etkin Dizin (Azure AD) Ayrıcalıklı Kimlik Yönetimi (PIM) bir rolü etkinleştirdiklerinde çok faktörlü bir kimlik doğrulama yarışmasını tamamlamalarını da gerektirebilirsiniz. Bu şekilde, kullanıcı oturum açtıklarında çok faktörlü bir kimlik doğrulama yarışmasını tamamlamadıysa, ayrıcalıklı kimlik yönetimi tarafından istenir.

> [!IMPORTANT]
> Şu anda Azure Çok Faktörlü Kimlik Doğrulama, Microsoft kişisel hesaplarıyla değil, yalnızca iş veya okul hesaplarıyla çalışır (genellikle Skype, Xbox veya Outlook.com gibi Microsoft hizmetlerinde oturum açmada kullanılan kişisel bir hesap). Bu nedenle, kişisel bir hesap kullanan herkes, rollerini etkinleştirmek için çok faktörlü kimlik doğrulamasını kullanamadığından uygun bir yönetici olamaz. Bu kullanıcıların bir Microsoft hesabı kullanarak iş yüklerini yönetmeye devam etmeleri gerekiyorsa, şimdilik bunları kalıcı yöneticilere yükseltin.

## <a name="how-pim-validates-mfa"></a>PIM MFA'yı nasıl doğrular?

Bir kullanıcı bir rolü etkinleştirdiğinde çok faktörlü kimlik doğrulamayı doğrulamak için iki seçenek vardır.

En basit seçenek, ayrıcalıklı bir rolü etkinleştiren kullanıcılar için Azure Çok Faktörlü Kimlik Doğrulaması'na güvenmektir. Bunu yapmak için öncelikle bu kullanıcıların gerekirse lisanslı olup olmadığını ve Azure Çok Faktörlü Kimlik Doğrulaması'na kaydolup olmadığını kontrol edin. Azure Çok Faktörlü Kimlik Doğrulama'yı nasıl dağıtılayınız hakkında daha fazla bilgi için bulut [tabanlı Azure Çok Faktörlü Kimlik Doğrulaması'nı dağıt'a](../authentication/howto-mfa-getstarted.md)bakın. Oturum açtıklarında bu kullanıcılar için çok faktörlü kimlik doğrulaması uygulamak için Azure AD'yi yapılandırmanız önerilir, ancak gerekli değildir. Bunun nedeni, çok faktörlü kimlik doğrulama denetimlerinin Ayrıcalıklı Kimlik Yönetimi'nin kendisi tarafından yapılacak olmasıdır.

Alternatif olarak, kullanıcılar şirket içinde kimlik doğrulama yaparsa, kimlik sağlayıcınızın çok faktörlü kimlik doğrulamadan sorumlu olmasını sağlayabilirsiniz. Örneğin, Azure AD'ye erişmeden önce AD Federation Hizmetlerini akıllı kart tabanlı kimlik doğrulaması gerektirecek şekilde yapılandırıldıysanız, [Azure Çok Faktörlü Kimlik Doğrulaması ve AD FS ile bulut kaynaklarını güvence altına](../authentication/howto-mfa-adfs.md) almak, Azure AD'ye hak talebinde bulunan AD FS'yi yapılandırma yönergeleri içerir. Bir kullanıcı bir rolü etkinleştirmeye çalıştığında, Ayrıcalıklı Kimlik Yönetimi, uygun talepleri aldıktan sonra çok faktörlü kimlik doğrulamanın kullanıcı için zaten doğrulandığını kabul eder.

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'nde Azure AD rol ayarlarını yapılandırma](pim-how-to-change-default-settings.md)
- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
