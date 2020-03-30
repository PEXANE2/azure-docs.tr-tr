---
title: Office 365 güvenilen parti güveni için imza karma algoritması değiştirme - Azure
description: Bu sayfa, Office 365 ile federasyon güveni için SHA algoritması değiştirme yönergeleri sağlar
keywords: SHA1,SHA256,O365,federasyon,aadconnect,adfs,reklam fs,değişim sha,federasyon güven, güvenen parti güven
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: cf6880e2-af78-4cc9-91bc-b64de4428bbd
ms.service: active-directory
ms.subservice: hybrid
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2233b434fda628dcf812a62f06541fc4b0296aba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76897344"
---
# <a name="change-signature-hash-algorithm-for-office-365-relying-party-trust"></a>Office 365 güvenilen parti güveni için imza karma algoritması değiştirme
## <a name="overview"></a>Genel Bakış
Active Directory Federation Services (AD FS), değiştirilemediğinden emin olmak için belirteçlerini Microsoft Azure Etkin Dizini'ne imzalar. Bu imza SHA1 veya SHA256'ya dayalı olabilir. Azure Etkin Dizin artık SHA256 algoritmasıyla imzalanmış belirteçleri destekler ve en yüksek güvenlik düzeyi için belirteç imzalama algoritmasını SHA256'ya ayarlamanızı öneririz. Bu makalede, belirteç imzalama algoritmasını daha güvenli SHA256 düzeyine ayarlamak için gereken adımlar açıklanmaktadır.

>[!NOTE]
>Microsoft, SHA1'den daha güvenli olduğu için belirteçleri imzalama algoritması olarak SHA256'nın kullanımını önerir, ancak SHA1 hala desteklenen bir seçenek olmaya devam etmektedir.

## <a name="change-the-token-signing-algorithm"></a>Belirteç imzalama algoritmasını değiştirme
Aşağıdaki iki işlemden biriyle imza algoritmasını ayarladıktan sonra, AD FS SHA256 ile Office 365 güvenen parti güveni için belirteçleri imzalar. Ek yapılandırma değişiklikleri yapmanız gerekmez ve bu değişikliğin Office 365 veya diğer Azure AD uygulamalarına erişme yeteneğinizi etkilemez.

### <a name="ad-fs-management-console"></a>AD FS yönetim konsolu
1. Birincil AD FS sunucusunda AD FS yönetim konsolu açın.
2. AD FS düğümlerini genişletin ve **Güvenerek Parti Güvenleri'ni**tıklatın.
3. Office 365/Azure güvenerek parti güveninizi sağ tıklatın ve **Özellikler'i**seçin.
4. **Gelişmiş** sekmesini seçin ve güvenli karma algoritmasha256 seçin.
5. **Tamam**'a tıklayın.

![SHA256 imzalama algoritması--MMC](./media/how-to-connect-fed-sha256-guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>AD FS PowerShell cmdlets
1. Herhangi bir AD FS sunucusunda PowerShell'i yönetici ayrıcalıkları altında açın.
2. **Set-AdfsRelyingPartyTrust** cmdlet'i kullanarak güvenli karma algoritmasını ayarlayın.
   
   <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'https://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Ayrıca okuyun
* [Azure AD Connect ile Office 365 güvenini onarım](how-to-connect-fed-management.md#repairthetrust)

