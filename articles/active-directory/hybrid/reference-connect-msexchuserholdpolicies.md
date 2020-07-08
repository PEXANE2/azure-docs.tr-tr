---
title: 'Azure AD Connect: msExchUserHoldPolicies ve cloudMsExchUserHoldPolicies | Microsoft Docs'
description: Bu konuda, msExchUserHoldPolicies ve cloudMsExchUserHoldPolicies özniteliklerinin öznitelik davranışı açıklanmaktadır
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 08/23/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f4c637a01825616334cda8faa594efd08f29de8d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74213087"
---
# <a name="azure-ad-connect---msexchuserholdpolicies-and-cloudmsexchuserholdpolicies"></a>Azure AD Connect-msExchUserHoldPolicies ve cloudMsExchUserHoldPolicies
Aşağıdaki başvuru belgesi, Exchange tarafından kullanılan bu öznitelikleri ve varsayılan eşitleme kurallarını düzenlemenin doğru yolunu açıklar.

## <a name="what-are-msexchuserholdpolicies-and-cloudmsexchuserholdpolicies"></a>MsExchUserHoldPolicies ve cloudMsExchUserHoldPolicies nedir?
Bir Exchange sunucusu için kullanılabilecek iki tür [saklama](https://docs.microsoft.com/Exchange/policy-and-compliance/holds/holds?view=exchserver-2019) vardır: dava tutma ve yerinde saklama. Dava bekletme etkin olduğunda tüm posta kutusu tüm öğeler beklemeye yerleştirilir.  Yerinde saklama, yalnızca yerinde eBulma aracını kullanarak tanımladığınız bir arama sorgusunun ölçütlerine uyan öğeleri korumak için kullanılır.

Msexchuserholdpolleve cloudMsExchUserHoldPolicies öznitelikleri şirket içi AD ve Azure AD 'ye, şirket içi Exchange veya Exchange 'in çevrimiçi mi olduğunu bağlı olarak hangi kullanıcıların bir bekletme kapsamında olduğunu belirlemesini sağlar.

## <a name="msexchuserholdpolicies-synchronization-flow"></a>msExchUserHoldPolicies eşitleme akışı
Varsayılan olarak, Msexchuserholdpoli, meta veri deposundaki msExchUserHoldPolicies özniteliği ve sonra Azure AD 'deki Msexchuserholdpolicies özniteliğine Azure AD Connect tarafından eşitlenir

Aşağıdaki tablolar akışı anlatmaktadır:

Şirket içi Active Directory gelen:

|Active Directory özniteliği|Öznitelik adı|Akış türü|Metaverse özniteliği|Eşitleme kuralı|
|-----|-----|-----|-----|-----|
|Şirket içi Active Directory|msExchUserHoldPolicies|Direct|Msexchuserholdilkeleri|AD 'den Kullanıcı değişimi ' nde|

Azure AD 'ye giden:

|Metaverse özniteliği|Öznitelik adı|Akış türü|Azure AD özniteliği|Eşitleme kuralı|
|-----|-----|-----|-----|-----|
|Azure Active Directory|msExchUserHoldPolicies|Direct|msExchUserHoldPolicies|AAD 'ye kadar – Kullanıcıexchangeonline|

## <a name="cloudmsexchuserholdpolicies-synchronization-flow"></a>cloudMsExchUserHoldPolicies eşitleme akışı
Varsayılan olarak cloudMsExchUserHoldPolicies, meta veri deposundaki cloudMsExchUserHoldPolicies özniteliğine doğrudan Azure AD Connect tarafından eşitlenir. Daha sonra, meta veri deposunda Msexchuserholdilkeleri null değilse, Active Directory dışarı aktarılan öznitelik.

Aşağıdaki tablolar akışı anlatmaktadır:

Azure AD 'den gelen:

|Active Directory özniteliği|Öznitelik adı|Akış türü|Metaverse özniteliği|Eşitleme kuralı|
|-----|-----|-----|-----|-----|
|Şirket içi Active Directory|cloudMsExchUserHoldPolicies|Direct|cloudMsExchUserHoldPolicies|AAD 'den içinde-Kullanıcı değişimi|

Şirket içi Active Directory giden:

|Metaverse özniteliği|Öznitelik adı|Akış türü|Azure AD özniteliği|Eşitleme kuralı|
|-----|-----|-----|-----|-----|
|Azure Active Directory|cloudMsExchUserHoldPolicies|ıF (NULL DEĞIL)|msExchUserHoldPolicies|AD – UserExchangeOnline|

## <a name="information-on-the-attribute-behavior"></a>Öznitelik davranışı hakkında bilgi
MsExchangeUserHoldPolicies tek bir yetkili özniteliğidir.  Tek bir yetkili özniteliği, şirket içi dizindeki veya bulut dizinindeki bir nesne (Bu durumda, Kullanıcı nesnesi) üzerinde ayarlanabilir.  Yetki başlangıcı kuralları, özniteliği Şirket içinden eşitlendiğinde, Azure AD 'nin bu özniteliği güncelleştirmesine izin verilmeyeceğini belirler.

Kullanıcıların buluttaki bir kullanıcı nesnesi üzerinde bir saklama ilkesi ayarlamaya izin vermek için, cloudMSExchangeUserHoldPolicies özniteliği kullanılır. Bu öznitelik, Azure AD yukarıda açıklanan kurallara göre doğrudan msExchangeUserHoldPolicies ayarlayamadığından kullanılır.  Bu öznitelik daha sonra, msExchangeUserHoldPolicies null değil ve msExchangeUserHoldPolicies geçerli değerini değiştirmek için şirket içi dizine geri eşitlenir.

Belirli koşullarda, örneğin, her ikisi de şirket içinde ve Azure 'da aynı anda değiştirildiyse, bu bazı sorunlara neden olabilir.  

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
