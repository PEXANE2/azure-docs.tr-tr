---
title: Güvenilmeyen ağlardan MFA gerektirir - Azure Etkin Dizini
description: Azure Etkin Dizini'nde (Azure AD) koşullu erişim ilkesini, güvenilmeyen ağlardan erişim girişimleri için nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4cedec7f0bd51460796d8138f8d481d2982098f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74379999"
---
# <a name="how-to-require-mfa-for-access-from-untrusted-networks-with-conditional-access"></a>Nasıl kullanılır: Koşullu Erişimile güvenilmeyen ağlardan erişim için MFA gerektirir   

Azure Active Directory (Azure AD), cihazlara, uygulamalara ve hizmetlere her yerden tek oturum açma olanağı sağlar. Kullanıcılarınız bulut uygulamalarınıza yalnızca kuruluşunuzun ağından değil, güvenilmeyen herhangi bir Internet konumundan da erişebilir. Güvenilmeyen ağlardan erişim için en iyi uygulama, çok faktörlü kimlik doğrulaması (MFA) gerektirmektir.

Bu makalede, güvenilmeyen ağlardan erişim için MFA gerektiren koşullu erişim ilkesi yapılandırmak için gereken bilgileri sağlar. 

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, aşina olduğunuzu varsayar: 

- Azure AD Koşullu Erişim'in [temel kavramları](overview.md) 
- Azure portalında Koşullu Erişim ilkelerini yapılandırmak için [en iyi uygulamalar](best-practices.md)

## <a name="scenario-description"></a>Senaryo açıklaması

Güvenlik ve üretkenlik arasındaki dengeye hakim olmak için, kuruluşunuzun ağından oturum açma için yalnızca bir parola gerektirmeniz yeterli olabilir. Ancak, güvenilmeyen bir ağ konumundan erişim için, oturum açmaların yasal kullanıcılar tarafından gerçekleştirilme riski artar. Bu sorunu gidermek için güvenilmeyen ağlara erişimi engelleyebilirsiniz. Alternatif olarak, hesabın yasal sahibi tarafından bir girişimyapıldığına dair ek güvence almak için çok faktörlü kimlik doğrulama (MFA) da izleyebilirsiniz. 

Azure AD Koşullu Erişim ile bu gereksinimi erişim sağlayan tek bir ilkeyle giderebilirsiniz: 

- Seçili bulut uygulamalarına
- Seçili kullanıcılar ve gruplar için  
- Çok faktörlü kimlik doğrulama gerektiren 
- Erişim kaynağı olduğunda: 
   - Güvenilmeyen bir konum

## <a name="implementation"></a>Uygulama

Bu senaryonun zorluğu, *güvenilmeyen bir ağ konumundan erişimi* Koşullu Erişim durumuna çevirmektir. Koşullu Erişim ilkesinde, [konum koşulunu](location-condition.md) ağ konumları ile ilgili senaryoları ele alacak şekilde yapılandırabilirsiniz. Konum koşulu, IP adresi aralıklarının, ülkelerinve bölgelerin mantıksal gruplandırmaları olan adlandırılmış konumları seçmenize olanak tanır.  

Genellikle, kuruluşunuz 199.30.16.0 - 199.30.16.15 gibi bir veya daha fazla adres aralığına sahiptir.
Adlandırılmış bir konumu şu şekilde yapılandırabilirsiniz:

- Bu aralığı belirtme (199.30.16.0/28) 
- **Kurumsal Ağ** gibi açıklayıcı bir ad atama 

Güvenilmeyen tüm konumları tanımlamaya çalışmak yerine şunları yapabilirsiniz:

- Herhangi bir konum ekleme 

   ![Koşullu Erişim](./media/untrusted-networks/02.png)

- Güvenilen tüm konumları hariç tut 

   ![Koşullu Erişim](./media/untrusted-networks/01.png)

## <a name="policy-deployment"></a>İlke dağıtımı

Bu makalede özetlenen yaklaşımla, artık güvenilmeyen konumlar için Koşullu Erişim ilkesini yapılandırabilirsiniz. İlkenizin beklendiği gibi çalıştığından emin olmak için, önerilen en iyi uygulama, politikayı üretime dönüştürmeden önce test etmektir. İdeal olarak, yeni politikanızın beklendiği gibi çalışıp çalışmadığını doğrulamak için bir test kiracısı kullanın. Daha fazla bilgi için [bkz.](best-practices.md#how-should-you-deploy-a-new-policy) 

## <a name="next-steps"></a>Sonraki adımlar

Koşullu Erişim hakkında daha fazla bilgi edinmek istiyorsanız, [Azure Etkin Dizinde Koşullu Erişim nedir?](../active-directory-conditional-access-azure-portal.md)
