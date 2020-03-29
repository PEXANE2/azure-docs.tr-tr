---
title: Azure AD Uygulama Proxy'sine Yükseltme | Microsoft Dokümanlar
description: Microsoft Forefront veya Unified Access Gateway'den yükseltme yaparken hangi proxy çözümleceğinin en iyi olduğunu seçin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: mimart
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4790dc7ebeeee3407e89bcf38d7e3f25699ed328
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67108420"
---
# <a name="compare-remote-access-solutions"></a>Uzaktan erişim çözümlerini karşılaştırın

Azure Active Directory Application Proxy, Microsoft'un sunduğu iki uzaktan erişim çözümünden biridir. Diğer Web Application Proxy, şirket içi sürümüdür. Bu iki çözüm, Microsoft'un sunduğu önceki ürünlerin yerini alır: Microsoft Forefront Threat Management Gateway (TMG) ve Unified Access Gateway (UAG). Bu dört çözümün birbiriyle nasıl karşılaştırDığını anlamak için bu makaleyi kullanın. O sizin hala amortismana niçin TMG veya UAG çözümlerini kullanarak, Uygulama Proxy birine geçiş planlamak için bu makaleyi kullanın. 


## <a name="feature-comparison"></a>Özellik karşılaştırması

Tehdit Yönetimi Ağ Geçidi (TMG), Birleşik Erişim Ağ Geçidi (UAG), Web Application Proxy (WAP) ve Azure AD Application Proxy'nin (AP) birbirleriyle nasıl karşılaştırDıklarını anlamak için bu tabloyu kullanın.

| Özellik | Tmg | Uag | WAP | AP |
| ------- | --- | --- | --- | --- |
| Sertifika kimlik doğrulaması | Evet | Evet | - | - |
| Tarayıcı uygulamalarını seçişekilde yayımlayın | Evet | Evet | Evet | Evet |
| Ön kimlik doğrulama ve tek oturum açma | Evet | Evet | Evet | Evet | 
| Katman 2/3 güvenlik duvarı | Evet | Evet | - | - |
| İleri proxy yetenekleri | Evet | - | - | - |
| VPN özellikleri | Evet | Evet | - | - |
| Zengin protokol desteği | - | Evet | Evet, http üzerinden çalışıyorsa | Evet, HTTP üzerinden veya Uzak Masaüstü Ağ Geçidi üzerinden çalışıyorsanız |
| ADFS proxy sunucusu olarak hizmet vermektedir | - | Evet | Evet | - |
| Uygulama erişimi için tek portal | - | Evet | - | Evet |
| Yanıt gövdesi bağlantı çevirisi | Evet | Evet | - | Evet | 
| Üstbilgi ile kimlik doğrulama | - | Evet | - | Evet, PingAccess ile | 
| Bulut ölçekli güvenlik | - | - | - | Evet | 
| Koşullu Erişim | - | Evet | - | Evet |
| Arındırılmış bölgede (DMZ) bileşen yok | - | - | - | Evet |
| Gelen bağlantı yok | - | - | - | Evet |

Çoğu senaryoda, azure AD Application Proxy'yi modern çözüm olarak öneririz. Web Application Proxy yalnızca AD FS için proxy sunucusu gerektiren senaryolarda tercih edilir ve Azure Active Directory'de özel etki alanlarını kullanamazsınız. 

Azure AD Application Proxy, benzer ürünlerle karşılaştırıldığında şunları dahil olmak üzere benzersiz avantajlar sunar:

- Azure AD'yi şirket içi kaynaklara genişletme
   - Bulut ölçekli güvenlik ve koruma
   - Koşullu Erişim ve Çok Faktörlü Kimlik Doğrulama gibi özellikleri etkinleştirmek kolaydır
- Arındırılmış bölgede bileşen yok
- Gelen bağlantı gerekmez
- Kullanıcılarınızın O365, Azure AD entegre SaaS uygulamaları ve şirket içi web uygulamalarınız da dahil olmak üzere tüm uygulamaları için gidebileceği tek bir erişim paneli. 


## <a name="next-steps"></a>Sonraki adımlar

- [Şirket içi uygulamalara güvenli uzaktan erişim sağlamak için Azure AD Uygulamasını kullanın](application-proxy.md)
