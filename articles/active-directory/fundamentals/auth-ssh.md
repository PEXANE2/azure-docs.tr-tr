---
title: Azure Active Directory ile SSH kimlik doğrulaması
description: Azure Active Directory ile SSH tümleştirmesi elde etmeye yönelik mimari rehberlik
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/19/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: eb766150339820f9356fe94311cd1ff33dda5480
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92462979"
---
# <a name="ssh"></a>SSH  

Secure Shell (SSH), güvenli olmayan bir ağ üzerinden güvenli bir şekilde işletim ağı Hizmetleri için şifreleme sağlayan bir ağ protokolüdür. SSH Ayrıca, bir komut satırı oturumu sağlar, uzak komutları yürütür ve dosyaları güvenli bir şekilde aktarır. Genellikle Linux® gibi UNIX tabanlı sistemlerde kullanılır. SSH, güvenli olmayan bir ağda şifreleme sağlamayan Telnet protokolünü değiştirir. 

Azure Active Directory (Azure AD), Azure üzerinde çalışan Linux® tabanlı sistemler için bir sanal makine (VM) uzantısı sağlar. 

## <a name="use-when"></a>Şu durumlarda kullanın 

* Uzaktan oturum açma gerektiren Linux® tabanlı VM 'lerle çalışma

* Linux® tabanlı sistemlerde uzak komutları yürütme

* Güvenli olmayan bir ağda dosyaları güvenle aktarın

![SSH protokolüyle Azure AD diyagramı](./media/authentication-patterns/ssh-auth.png)

Azure AD ile SSH

## <a name="components-of-system"></a>Sistem bileşenleri 

* **Kullanıcı**: Linux® VM 'lerle bir bağlantı kurmak için SSH istemcisini başlatır ve kimlik doğrulaması için kimlik bilgileri sağlar.

* **Web tarayıcısı**: kullanıcının etkileşime girdiği bileşen. Kullanıcıyı güvenli bir şekilde doğrulamak ve yetkilendirmek için kimlik sağlayıcısıyla (Azure AD) iletişim kurar.

* **SSH istemcisi**: bağlantı kurulum işlemini yürütür.

* **Azure AD**: cihaz akışını kullanarak Kullanıcı kimliğinin kimliğini doğrular ve Linux VM 'lerine belirteç verir.

* **LINUX VM**: belirteci kabul eder ve başarılı bağlantı sağlar.

## <a name="implement-ssh-with-azure-ad"></a>Azure AD ile SSH uygulama 

* [Azure Active Directory kimlik bilgileriyle Linux® VM 'de oturum açma-Azure sanal makineleri ](https://docs.microsoft.com/azure/virtual-machines/linux/login-using-aad) 

* [OAuth 2,0 cihaz kodu akışı-Microsoft Identity platform ](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-device-code)

* [Azure Active Directory ile tümleştirin (akamai.com)](https://learn.akamai.com/webhelp/enterprise-application-access/enterprise-application-access/GUID-6B16172C-86CC-48E8-B30D-8E678BF3325F.html)

 
