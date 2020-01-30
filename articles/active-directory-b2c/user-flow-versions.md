---
title: Azure Active Directory B2C 'daki Kullanıcı akışı sürümleri | Microsoft Docs
description: Azure Active Directory B2C ' de kullanılabilen Kullanıcı akışlarının sürümleri hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/25/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: d7e174245755659494dfe1243c39619ae37f0f33
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76840086"
---
# <a name="user-flow-versions-in-azure-active-directory-b2c"></a>Azure Active Directory B2C 'daki Kullanıcı akışı sürümleri

Azure Active Directory B2C (Azure AD B2C) Kullanıcı akışları, müşteri kimlik deneyimlerini tam olarak tanımlayan ortak [ilkeleri](user-flow-overview.md) ayarlamanıza yardımcı olur. Bu deneyimlere kaydolma, oturum açma, parola sıfırlama veya profil düzenlemesi dahildir. Azure AD B2C, hem önerilen Kullanıcı akışlarının bir koleksiyonundan seçim yapabilir hem de Kullanıcı akışlarını önizleyebilirsiniz.

Yeni Kullanıcı akışları yeni sürümler olarak eklenir. Kullanıcı akışları kararlı hale geldiğinde kullanım için tavsiye edilir. Kullanıcı akışları, tamamen test edilmiş olmaları durumunda **Önerilen** olarak işaretlenir. Kullanıcı akışları, önerilen olarak işaretlenene kadar önizleme olarak değerlendirilir. Herhangi bir üretim uygulaması için önerilen bir Kullanıcı akışı kullanın, ancak kullanılabilir hale geldiğinde yeni işlevselliği test etmek için diğer sürümlerden birini seçin. Önerilen Kullanıcı akışlarının eski sürümlerini kullanmamanız gerekir.

>[!IMPORTANT]
> Bir Kullanıcı akışı **önerilmediği**sürece, *Önizleme*aşamasında olduğu kabul edilir. Üretim uygulamalarınız için yalnızca önerilen Kullanıcı akışlarını kullanmanız gerekir.

## <a name="v1"></a>V1

| Kullanıcı akışı | Önerilen | Açıklama |
| --------- | ----------- | ----------- |
| Parola sıfırlama | Evet | Kullanıcının e-postalarını doğruladıktan sonra yeni bir parola seçmesini sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Multi-Factor Authentication](custom-policy-multi-factor-authentication.md)</li><li>Belirteç uyumluluk ayarları</li><li>[Parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul> |
| Profil düzenlemesi | Evet | Kullanıcının kullanıcı özniteliklerini yapılandırmasına olanak sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li></ul> |
| ROPC kullanarak oturum açın | Hayır | Yerel bir hesaba sahip olan bir kullanıcının doğrudan yerel uygulamalarda oturum açmasını sağlar (tarayıcı gerekmez). Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li></ul> |
| Oturum aç | Hayır | Kullanıcının hesabında oturum açmasını sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Multi-Factor Authentication](custom-policy-multi-factor-authentication.md)</li><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>Oturum açmayı engelle</li><li>Parola sıfırlamayı zorla</li><li>Oturumumu Açık tut (KMSı)</ul><br>Kullanıcı arabirimini bu kullanıcı akışıyla özelleştiremezsiniz. |
| Kaydol | Hayır | Kullanıcının hesap oluşturmasını sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Multi-Factor Authentication](custom-policy-multi-factor-authentication.md)</li><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>[Parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul> |
| Kaydolma ve oturum açma | Evet | Kullanıcının hesabı oluşturmasını veya hesabında oturum açmasını sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Multi-Factor Authentication](custom-policy-multi-factor-authentication.md)</li><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>[Parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul>|

## <a name="v11"></a>V 1.1

| Kullanıcı akışı | Önerilen | Açıklama |
| --------- | ----------- | ----------- |
| Parola sıfırlama v 1.1 | Hayır | Kullanıcının e-postalarını doğruladıktan sonra yeni bir parola seçmesini sağlar (yeni sayfa düzeni kullanılabilir). Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Multi-Factor Authentication](custom-policy-multi-factor-authentication.md)</li><li>Belirteç uyumluluk ayarları</li><li>[Parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul> |

## <a name="v2"></a>V2

| Kullanıcı akışı | Önerilen | Açıklama |
| --------- | ----------- | ----------- |
| Parola sıfırlama v2 | Hayır | Kullanıcının e-postalarını doğruladıktan sonra yeni bir parola seçmesini sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Multi-Factor Authentication](custom-policy-multi-factor-authentication.md)</li><li>Belirteç uyumluluk ayarları</li><li>[Yaş aşımları](basic-age-gating.md)</li><li>[parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul> |
| Profil düzenlemesi v2 | Evet | Kullanıcının kullanıcı özniteliklerini yapılandırmasına olanak sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li></ul> |
| Oturum açma v2 | Hayır | Kullanıcının hesabında oturum açmasını sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Multi-Factor Authentication](custom-policy-multi-factor-authentication.md)</li><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>[Yaş aşımları](basic-age-gating.md)</li><li>Oturum açma sayfası özelleştirmesi</li></ul> |
| Kaydolma v2 | Hayır | Kullanıcının hesap oluşturmasını sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Multi-Factor Authentication](custom-policy-multi-factor-authentication.md)</li><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>[Yaş aşımları](basic-age-gating.md)</li><li>[Parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul> |
| Kaydolun ve oturum açın v2 | Hayır | Kullanıcının hesabında hesap oluşturmasını veya oturum açmasını sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Multi-Factor Authentication](custom-policy-multi-factor-authentication.md)</li><li>[Yaş aşımları](basic-age-gating.md)</li><li>[Parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul> |
