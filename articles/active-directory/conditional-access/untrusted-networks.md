---
title: Güvenilmeyen ağlardan MFA gerektir-Azure Active Directory
description: Güvenilir olmayan ağlardan erişim girişimleri için Azure Active Directory (Azure AD) ' de koşullu erişim ilkesini nasıl yapılandıracağınızı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 10/16/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: ab9d544cc205d2971b2097110d8c36c5d85fa1fd
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92145000"
---
# <a name="how-to-require-mfa-for-access-from-untrusted-networks-with-conditional-access"></a>Nasıl yapılır: koşullu erişimi olan güvenilmeyen ağlardan erişim için MFA gerektirme   

Azure Active Directory (Azure AD) her yerden cihazlarda, uygulamalarda ve hizmetlerde çoklu oturum açma imkanı sağlar. Kullanıcılarınız, bulut uygulamalarınıza yalnızca kuruluşunuzun ağından değil, ayrıca güvenilmeyen Internet konumlarından de erişebilirler. Güvenilmeyen ağlardan erişim için en iyi yöntem, çok faktörlü kimlik doğrulaması (MFA) gerektirir.

Bu makale, güvenilmeyen ağlardan erişim için MFA gerektiren bir koşullu erişim ilkesini yapılandırmak için gereken bilgileri sağlar. 

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, Koşullu erişimin [temel kavramlarıyla](overview.md) ilgili bilgi sahibi olduğunuz varsayılır. 

## <a name="scenario-description"></a>Senaryo açıklaması

Güvenlik ve üretkenlik arasındaki dengeyi temel almak için, kuruluşunuzun ağından yalnızca oturum açma işlemleri için bir parola gerekli olabilir. Ancak, güvenilmeyen bir ağ konumundan erişim için, oturum açma işlemlerinin meşru kullanıcılar tarafından gerçekleştirilmediğinden daha fazla risk vardır. Bu sorunu gidermek için güvenilmeyen ağlardan erişimi engelleyebilirsiniz. Alternatif olarak, hesabın meşru sahibi tarafından yapılan bir girişimin daha fazla güvence kazanması için çok faktörlü kimlik doğrulaması (MFA) de gerekebilir. 

Azure AD koşullu erişimi sayesinde, erişim izni veren tek bir ilkeyle bu gereksinimi ele alabilirsiniz: 

- Seçili bulut uygulamalarına
- Seçili kullanıcılar ve gruplar için  
- Multi-Factor Authentication gerektirme 
- Erişim kaynağı: 
   - Güvenilir olmayan bir konum

## <a name="implementation"></a>Uygulama

Bu senaryonun çekişmesi, *güvenilir olmayan bir ağ konumundan* bir koşullu erişim koşuluna erişimi çevirmektir. Bir koşullu erişim ilkesinde, [konum koşulunu](location-condition.md) ağ konumlarıyla ilgili senaryolara yönelik olarak yapılandırmak için yapılandırabilirsiniz. Konumlar koşulu, IP adresi aralıklarının, ülkelerin ve bölgelerin mantıksal gruplandırmaları olan adlandırılmış konumları seçmenize olanak sağlar.  

Genellikle, kuruluşunuz bir veya daha fazla adres aralığına sahip olur, örneğin, 199.30.16.0-199.30.16.15.
Adlandırılmış bir konumu şu şekilde yapılandırabilirsiniz:

- Bu aralığı belirtme (199.30.16.0/28) 
- **Şirket ağı** gibi açıklayıcı bir ad atama 

Güvenilir olmayan tüm konumların ne olduğunu tanımlamaya çalışmak yerine şunları yapabilirsiniz:

- Herhangi bir konum Ekle 

   ![Koşullu Erişim](./media/untrusted-networks/02.png)

- Tüm güvenilen konumları Dışla 

   ![Koşullu Erişim](./media/untrusted-networks/01.png)

## <a name="policy-deployment"></a>İlke dağıtımı

Bu makalede açıklanan yaklaşımla birlikte, artık güvenilmeyen konumlar için bir koşullu erişim ilkesi yapılandırabilirsiniz. İlkenizin beklenildiği gibi çalıştığından emin olmak için önerilen en iyi yöntem, üretime geçmeden önce test sağlamaktır. İdeal olarak, yeni ilkenizin istendiği gibi çalışıp çalışmadığını doğrulamak için bir test kiracısı kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Koşullu erişim hakkında daha fazla bilgi edinmek istiyorsanız bkz. [Azure Active Directory Koşullu erişim nedir?](./overview.md)
