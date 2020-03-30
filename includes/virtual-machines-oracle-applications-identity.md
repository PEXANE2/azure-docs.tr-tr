---
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 07/10/2019
ms.author: danlep
ms.openlocfilehash: c1cca99a04e78c40cdd1061e52e49f45a73c73eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68361563"
---
### <a name="identity-tier"></a>Kimlik katmanı 

Microsoft-Oracle ortaklığı, Azure, OCI ve Oracle uygulamanızda birleşik bir kimlik ayarlamanızı sağlar. JD Edwards EnterpriseOne veya PeopleSoft uygulama paketi için, Azure AD ve Oracle IDCS arasında tek oturum açma ayarlamak için Oracle HTTP Server (OHS) örneği gereklidir.

İsG, uygulama katmanına ters proxy görevi görür, bu da son uygulamalara yapılan tüm isteklerin bu katmandan geçtiği anlamına gelir. Oracle Access Manager WebGate, son uygulamaya giden her isteği engelleyen bir OHS web sunucusu eklentisidir. Erişilen bir kaynak korunuyorsa (kimlik doğrulaması oturumu gerektiriyorsa), WebGate kullanıcının tarayıcısı aracılığıyla Identity Cloud Service ile OIDC kimlik doğrulama akışını başlatır. OpenID Connect WebGate tarafından desteklenen akışlar hakkında daha fazla bilgi için [Oracle Access Manager belgelerine](https://docs.oracle.com/en/middleware/idm/access-manager/12.2.1.3/aiaag/integrating-webgate-oidc-server.html)bakın.

Bu kurulumla, Azure AD'ye giriş yapmış bir kullanıcı, Oracle Identity Cloud Service aracılığıyla tekrar oturum açmadan JD Edwards EnterpriseOne veya PeopleSoft uygulamasına gidebilir. Bu çözümü dağıtan müşteriler, tek bir kimlik bilgileri kümesi, geliştirilmiş oturum açma deneyimi, geliştirilmiş güvenlik ve daha az yardım masası maliyeti dahil olmak üzere tek oturum açma avantajlarından yararlanır.

Azure AD ile JD Edwards EnterpriseOne veya PeopleSoft için tek oturum açma hakkında daha fazla bilgi edinmek için ilgili [Oracle teknik incelemesine](https://cloud.oracle.com/iaas/whitepapers/deploy_peoplesoft_jdedwards_across_oci_azure.pdf)bakın.