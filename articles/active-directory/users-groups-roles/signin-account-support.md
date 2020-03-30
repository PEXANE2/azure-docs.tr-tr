---
title: Azure AD oturum açma sayfam Microsoft hesaplarını kabul ediyor mu | Microsoft Dokümanlar
description: Ekrandaki mesajlaşma, oturum açma sırasında kullanıcı adı aramasını nasıl yansıtır?
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 221ab7c50a84650f1b2adf3fdb2b284365795f42
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74024277"
---
# <a name="sign-in-options-for-microsoft-accounts-in-azure-active-directory"></a>Azure Active Directory'deki Microsoft hesapları için oturum açma seçenekleri

Azure Etkin Diziniçin Microsoft 365 oturum açma sayfası (Azure AD), iş veya okul hesaplarını ve Microsoft hesaplarını destekler, ancak kullanıcının durumuna bağlı olarak, bu sayfa bir veya diğer veya her ikisi de olabilir. Örneğin, Azure AD oturum açma sayfası şunları destekler:

* Her iki hesap türünden oturum açma kabul eden uygulamalar
* Misafir kabul eden kuruluşlar

## <a name="identification"></a>Kimlik
Kuruluşunuzun kullandığı oturum açma sayfasının kullanıcı adı alanındaki ipucu metnine bakarak Microsoft hesaplarını destekleyip desteklemedisini anlayabilirsiniz. İpucu metninde "E-posta, telefon veya Skype" yazıyorsa, oturum açma sayfası Microsoft hesaplarını destekler.

![Hesap oturum açma sayfaları arasındaki fark](./media/signin-account-support/ui-prompt.png)

[Ek oturum açma seçenekleri yalnızca kişisel Microsoft hesaplarında çalışır,](https://azure.microsoft.com/updates/microsoft-account-signin-options/ ) ancak iş veya okul hesabı kaynaklarında oturum açma için kullanılamaz.

## <a name="next-steps"></a>Sonraki adımlar

[Oturum açma markanızı özelleştirin](../fundamentals/add-custom-domain.md)