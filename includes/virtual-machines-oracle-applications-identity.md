---
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 07/10/2019
ms.author: danlep
ms.openlocfilehash: 2b28cc742bb981e0db535d39cb0c7fbf027470ea
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83673773"
---
### <a name="identity-tier"></a>Kimlik katmanı 

Microsoft-Oracle ortaklığı, Azure, OCı ve Oracle uygulamanız arasında Birleşik bir kimlik ayarlamanıza olanak sağlar. JD EDBOL EnterpriseOne veya PeopleSoft uygulama paketi için, Azure AD ve Oracle ıDCS arasında çoklu oturum açmayı ayarlamak için Oracle HTTP sunucusu (OHS) örneği gereklidir.

OHS, uygulama katmanında ters ara sunucu görevi görür, bu da son uygulamalara yönelik tüm isteklerin bu şekilde gezinmesidir. Oracle Access Manager Web Gate, son uygulamaya giden her isteği izleyen bir OHS Web sunucusu eklentisidir. Erişilmekte olan bir kaynak korunuyorsa (kimliği doğrulanmış bir oturum gerektiriyorsa) WebGate, kimlik bulut hizmeti ile OıDC kimlik doğrulama akışını kullanıcının tarayıcısı aracılığıyla başlatır. OpenID Connect WebGate tarafından desteklenen akışlar hakkında daha fazla bilgi için bkz. [Oracle Access Manager belgeleri](https://docs.oracle.com/cd/E52734_01/oam/AIAAG/GUID-1E927D1B-FB83-425B-8768-85DB441821A4.htm#AIAAG7327).

Bu kurulumla, Azure AD 'de zaten oturum açmış bir Kullanıcı, Oracle Identity Cloud Service aracılığıyla yeniden oturum açmadan JD EDBOL EnterpriseOne veya PeopleSoft uygulamasına gidebilir. Bu çözümü dağıtan müşteriler tek bir kimlik bilgileri kümesi, geliştirilmiş bir oturum açma deneyimi, geliştirilmiş güvenlik ve daha az yardım masası maliyeti dahil olmak üzere çoklu oturum açma 'nın avantajlarından yararlanır.

Azure AD ile JD EDBOL EnterpriseOne veya PeopleSoft için çoklu oturum açma ayarlama hakkında daha fazla bilgi edinmek için bkz. ilişkili [Oracle teknik incelemesi](https://cloud.oracle.com/iaas/whitepapers/deploy_peoplesoft_jdedwards_across_oci_azure.pdf).