---
title: Azure Active Directory B2C'deki kullanıcı akışı sürümleri | Microsoft Dokümanlar
description: Azure Active Directory B2C'de kullanılabilen kullanıcı akışlarının sürümleri hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/25/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 40d21d3390396e0cb7e44d4e19598f9b0b691087
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78185633"
---
# <a name="user-flow-versions-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'deki kullanıcı akışı sürümleri

Azure Active Directory B2C'deki (Azure AD B2C) kullanıcı akışları, müşteri kimlik deneyimlerini tam olarak açıklayan ortak [ilkeler](user-flow-overview.md) ayarlamanıza yardımcı olur. Bu deneyimler arasında kaydolma, oturum açma, parola sıfırlama veya profil düzenleme yer almaktadır. Azure AD B2C'de, hem önerilen kullanıcı akışları hem de önizleme kullanıcı akışları koleksiyonundan seçim yapabilirsiniz.

Yeni kullanıcı akışları yeni sürümler olarak eklenir. Kullanıcı akışları kararlı hale geldikçe, kullanım için önerilir. Kullanıcı akışları, kapsamlı bir şekilde sınanmışsa **Önerilen** olarak işaretlenir. Kullanıcı akışları, önerilen olarak işaretlenene kadar önizlemede kabul edilir. Herhangi bir üretim uygulaması için önerilen kullanıcı akışını kullanın, ancak kullanılabilir olduğunda yeni işlevleri sınamak için diğer sürümlerden birini seçin. Önerilen kullanıcı akışlarının eski sürümlerini kullanmamalısınız.

>[!IMPORTANT]
> Bir kullanıcı akışı **Önerilen**olarak tanımlanmadığı sürece, *önizleme*olarak kabul edilir. Üretim uygulamalarınız için yalnızca önerilen kullanıcı akışlarını kullanmalısınız.

## <a name="v1"></a>V1

| Kullanıcı akışı | Önerilen | Açıklama |
| --------- | ----------- | ----------- |
| Parola sıfırlama | Evet | Bir kullanıcının e-postalarını doğruladıktan sonra yeni bir parola seçmesini sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Çok faktörlü kimlik doğrulama](custom-policy-multi-factor-authentication.md)</li><li>Belirteç uyumluluk ayarları</li><li>[Parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul> |
| Profil düzenleme | Evet | Bir kullanıcının kullanıcı özniteliklerini yapılandırmasını sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li></ul> |
| ROPC kullanarak oturum açma | Hayır | Yerel hesabı olan bir kullanıcının doğrudan yerel uygulamalarda oturum açmasını sağlar (tarayıcı gerekmez). Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li></ul> |
| Oturum aç | Hayır | Bir kullanıcının hesabında oturum açmasını sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Çok faktörlü kimlik doğrulama](custom-policy-multi-factor-authentication.md)</li><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>Oturum açma engelle</li><li>Parola sıfırlamayı zorlama</li><li>Beni İmzala (KMSI) Tut</ul><br>Bu kullanıcı akışıyla kullanıcı arabirimini özelleştiremezsiniz. |
| Kaydolma | Hayır | Bir kullanıcının hesap oluşturmasını sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Çok faktörlü kimlik doğrulama](custom-policy-multi-factor-authentication.md)</li><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>[Parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul> |
| Kaydolma ve oturum açma | Evet | Bir kullanıcının bir hesap oluşturmasına veya hesabında oturum açmasına olanak tanır. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Çok faktörlü kimlik doğrulama](custom-policy-multi-factor-authentication.md)</li><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>[Parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul>|

## <a name="v11"></a>V1.1

| Kullanıcı akışı | Önerilen | Açıklama |
| --------- | ----------- | ----------- |
| Parola sıfırlama v1.1 | Hayır | Kullanıcının e-postalarını doğruladıktan sonra yeni bir parola seçmesine izin verir (yeni sayfa düzeni kullanılabilir). Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Çok faktörlü kimlik doğrulama](custom-policy-multi-factor-authentication.md)</li><li>Belirteç uyumluluk ayarları</li><li>[Parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul> |

## <a name="v2"></a>V2

| Kullanıcı akışı | Önerilen | Açıklama |
| --------- | ----------- | ----------- |
| Parola sıfırlama v2 | Hayır | Bir kullanıcının e-postalarını doğruladıktan sonra yeni bir parola seçmesini sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Çok faktörlü kimlik doğrulama](custom-policy-multi-factor-authentication.md)</li><li>Belirteç uyumluluk ayarları</li><li>[Yaş sınırı](basic-age-gating.md)</li><li>[parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul> |
| Profil düzenleme v2 | Evet | Bir kullanıcının kullanıcı özniteliklerini yapılandırmasını sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li></ul> |
| V2'de oturum açın | Hayır | Bir kullanıcının hesabında oturum açmasını sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Çok faktörlü kimlik doğrulama](custom-policy-multi-factor-authentication.md)</li><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>[Yaş sınırı](basic-age-gating.md)</li><li>Oturum açma sayfası özelleştirme</li></ul> |
| V2'ye kaydolun | Hayır | Bir kullanıcının hesap oluşturmasını sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Çok faktörlü kimlik doğrulama](custom-policy-multi-factor-authentication.md)</li><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>[Yaş sınırı](basic-age-gating.md)</li><li>[Parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul> |
| Kaydolun ve v2'de oturum açın | Hayır | Bir kullanıcının bir hesap oluşturmasına veya hesabında oturum açmasına olanak tanır. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Çok faktörlü kimlik doğrulama](custom-policy-multi-factor-authentication.md)</li><li>[Yaş sınırı](basic-age-gating.md)</li><li>[Parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul> |
