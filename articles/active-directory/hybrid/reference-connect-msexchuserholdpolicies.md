---
title: 'Azure AD Connect: msExchUserHoldPolicies ve cloudMsExchUserHoldPolicies | Microsoft Dokümanlar'
description: Bu konu msExchUserHoldPolicies ve cloudMsExchUserHoldPolicies öznitelikleri öznitelik davranışı açıklar
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74213087"
---
# <a name="azure-ad-connect---msexchuserholdpolicies-and-cloudmsexchuserholdpolicies"></a>Azure AD Connect - msExchUserHoldPolicies ve cloudMsExchUserHoldPolicies
Aşağıdaki başvuru belgesi Exchange tarafından kullanılan bu öznitelikleri ve varsayılan eşitleme kurallarını düzenlemenin uygun yolunu açıklar.

## <a name="what-are-msexchuserholdpolicies-and-cloudmsexchuserholdpolicies"></a>msExchUserHoldPolicies ve cloudMsExchUserHoldPolicies nelerdir?
Exchange Sunucusu için iki tür [bekleme](https://docs.microsoft.com/Exchange/policy-and-compliance/holds/holds?view=exchserver-2019) vardır: Dava Tutma ve Yerinde Tutma. Dava Bekletme etkinleştirildiğinde, tüm posta kutusu tüm öğeler beklemeye alınır.  Yerinde Bekleme, yalnızca Yerinde eDiscovery aracını kullanarak tanımladığınız bir arama sorgusunun ölçütlerini karşılayan öğeleri korumak için kullanılır.

MsExchUserHoldPolcies ve cloudMsExchUserHoldPolicies öznitelikleri, şirket içi Exchange veya Exchange'i çevrimiçi olarak kullanıp kullanmadıklarına bağlı olarak hangi kullanıcıların bekleme altında olduğunu şirket içi AD ve Azure AD'ye izin verir.

## <a name="msexchuserholdpolicies-synchronization-flow"></a>msExchUserHoldPolicies senkronizasyon akışı
Varsayılan olarak MsExchUserHoldPolcies Azure AD Connect tarafından senkronize edilir doğrudan metaverse msExchUserHoldPolicies özniteliği ve daha sonra Azure AD msExchUserHoldPolices özniteliği

Aşağıdaki tablolar akışı açıklar:

Şirket içi Aktif Dizinden gelen:

|Etkin Dizin özniteliği|Öznitelik adı|Akış türü|Metaverse öznitelik|Eşitleme Kuralı|
|-----|-----|-----|-----|-----|
|Şirket içi Active Directory|msExchUserHoldPolitikalar|Direct|msExchUserHoldPolices|AD'den Girin - Kullanıcı Değişimi|

Azure AD'ye giden:

|Metaverse öznitelik|Öznitelik adı|Akış türü|Azure AD özniteliği|Eşitleme Kuralı|
|-----|-----|-----|-----|-----|
|Azure Active Directory|msExchUserHoldPolitikalar|Direct|msExchUserHoldPolitikalar|Out AAD için - UserExchangeOnline|

## <a name="cloudmsexchuserholdpolicies-synchronization-flow"></a>cloudMsExchUserHoldPolicies senkronizasyon akışı
Varsayılan buluttarafındanMsExchUserHoldPolicies Azure AD Connect tarafından senkronize edilir doğrudan bulutMsExchUserHoldPolicies özniteliği metaverse. Sonra, msExchUserHoldPolices metaverse null değilse, öznitelik Active Directory dışarı aktı.

Aşağıdaki tablolar akışı açıklar:

Azure AD'den gelen:

|Etkin Dizin özniteliği|Öznitelik adı|Akış türü|Metaverse öznitelik|Eşitleme Kuralı|
|-----|-----|-----|-----|-----|
|Şirket içi Active Directory|cloudMsExchUserHoldPolicies|Direct|cloudMsExchUserHoldPolicies|AAD'den Gelen - Kullanıcı Değişimi|

Şirket içi Aktif Dizine giden:

|Metaverse öznitelik|Öznitelik adı|Akış türü|Azure AD özniteliği|Eşitleme Kuralı|
|-----|-----|-----|-----|-----|
|Azure Active Directory|cloudMsExchUserHoldPolicies|IF(NULL DEĞİL)|msExchUserHoldPolitikalar|Ad'ye Out – UserExchangeOnline|

## <a name="information-on-the-attribute-behavior"></a>Öznitelik davranışı hakkında bilgi
msExchangeUserHoldPolicies tek bir yetki özniteliğidir.  Tek bir yetki özniteliği, şirket içi dizinde veya bulut dizininde bir nesne (bu durumda, kullanıcı nesnesi) üzerinde ayarlanabilir.  Yetki Başlangıcı kuralları, öznitelik şirket içinde eşitlenirse, Azure AD'nin bu özelliği güncelleştirmesine izin verilmeyeceğini belirtir.

Kullanıcıların buluttaki bir kullanıcı nesnesi üzerinde bekleme ilkesi ayarlamasına izin vermek için cloudMSExchangeUserHoldPolicies özniteliği kullanılır. Azure AD msExchangeUserHoldPolicies'ı doğrudan yukarıda açıklanan kurallara göre ayarlayamadığından bu özellik kullanılır.  MsExchangeUserHoldPolicies null değilse ve msExchangeUserHoldPolicies geçerli değerini değiştirirseniz, bu öznitelik daha sonra şirket içi dizine geri senkronize edecektir.

Belirli koşullar altında, örneğin, her ikisi de şirket içinde ve Azure'da aynı anda değiştirilirse, bu bazı sorunlara neden olabilir.  

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
