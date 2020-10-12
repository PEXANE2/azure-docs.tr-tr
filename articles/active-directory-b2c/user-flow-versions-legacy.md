---
title: Azure Active Directory B2C eski Kullanıcı akışı sürümleri | Microsoft Docs
description: Azure Active Directory B2C ' de kullanılabilen Kullanıcı akışlarının eski sürümleri hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 07/30/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 3471b9988b154ccb38dc56545e769a7179a4026f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91258736"
---
# <a name="legacy-user-flow-versions-in-azure-active-directory-b2c"></a>Azure Active Directory B2C eski Kullanıcı akışı sürümleri

> [!IMPORTANT]
> Bu makalede, Kullanıcı akışlarının v1 (üretime yönelik) sürümleri ve V 1.1 ve v2 (Önizleme) sürümlerini sunan Kullanıcı akışları için eski sürüm oluşturma yöntemi açıklanmaktadır. Azure genel bulutu dışındaki ortamlar, bu eski sürüm oluşturma yöntemini kullanmaya devam edecektir. Azure genel bulutu 'nda bu yöntem, [ **Önerilen** yeni ve **Önizleme** sürümleri](user-flow-versions.md)ile değiştirilmiştir.
> 
Azure Active Directory B2C (Azure AD B2C) Kullanıcı akışları, müşteri kimlik deneyimlerini tam olarak tanımlayan ortak [ilkeleri](user-flow-overview.md) ayarlamanıza yardımcı olur. Bu deneyimlere kaydolma, oturum açma, parola sıfırlama veya profil düzenlemesi dahildir.

Aşağıdaki tabloda, bir Kullanıcı akışı **önerildiği**gibi tanımlanmadığı sürece *Önizleme*aşamasında olduğu kabul edilir. Üretim uygulamalarınız için yalnızca önerilen Kullanıcı akışlarını kullanmanız gerekir.

## <a name="v1"></a>V1

| Kullanıcı akışı | Önerilen | Açıklama |
| --------- | ----------- | ----------- |
| Parola sıfırlama | Evet | Kullanıcının e-postalarını doğruladıktan sonra yeni bir parola seçmesini sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Çok faktörlü kimlik doğrulaması](custom-policy-multi-factor-authentication.md)</li><li>Belirteç uyumluluk ayarları</li><li>[Parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul> |
| Profil düzenlemesi | Evet | Kullanıcının kullanıcı özniteliklerini yapılandırmasına olanak sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li></ul> |
| ROPC kullanarak oturum açın | Hayır | Yerel bir hesaba sahip olan bir kullanıcının doğrudan yerel uygulamalarda oturum açmasını sağlar (tarayıcı gerekmez). Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li></ul> |
| Oturum aç | Hayır | Kullanıcının hesabında oturum açmasını sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Çok faktörlü kimlik doğrulaması](custom-policy-multi-factor-authentication.md)</li><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>Oturum açmayı engelle</li><li>Parola sıfırlamayı zorla</li><li>Oturumumu Açık tut (KMSı)</ul><br>Kullanıcı arabirimini bu kullanıcı akışıyla özelleştiremezsiniz. |
| Kaydolma | Hayır | Kullanıcının hesap oluşturmasını sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Çok faktörlü kimlik doğrulaması](custom-policy-multi-factor-authentication.md)</li><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>[Parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul> |
| Kaydolma ve oturum açma | Evet | Kullanıcının hesabı oluşturmasını veya hesabında oturum açmasını sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Çok faktörlü kimlik doğrulaması](custom-policy-multi-factor-authentication.md)</li><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>[Parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul>|

## <a name="v11"></a>V 1.1

| Kullanıcı akışı | Önerilen | Açıklama |
| --------- | ----------- | ----------- |
| Parola sıfırlama v 1.1 | Hayır | Kullanıcının e-postalarını doğruladıktan sonra yeni bir parola seçmesini sağlar (yeni sayfa düzeni kullanılabilir). Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Çok faktörlü kimlik doğrulaması](custom-policy-multi-factor-authentication.md)</li><li>Belirteç uyumluluk ayarları</li><li>[Parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul> |

## <a name="v2"></a>V2

| Kullanıcı akışı | Önerilen | Açıklama |
| --------- | ----------- | ----------- |
| Parola sıfırlama v2 | Hayır | Kullanıcının e-postalarını doğruladıktan sonra yeni bir parola seçmesini sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Çok faktörlü kimlik doğrulaması](custom-policy-multi-factor-authentication.md)</li><li>Belirteç uyumluluk ayarları</li><li>[Yaş sınırı](basic-age-gating.md)</li><li>[parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul> |
| Profil düzenlemesi v2 | Evet | Kullanıcının kullanıcı özniteliklerini yapılandırmasına olanak sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li></ul> |
| Oturum açma v2 | Hayır | Kullanıcının hesabında oturum açmasını sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Çok faktörlü kimlik doğrulaması](custom-policy-multi-factor-authentication.md)</li><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>[Yaş sınırı](basic-age-gating.md)</li><li>Oturum açma sayfası özelleştirmesi</li></ul> |
| Kaydolma v2 | Hayır | Kullanıcının hesap oluşturmasını sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Çok faktörlü kimlik doğrulaması](custom-policy-multi-factor-authentication.md)</li><li>[Belirteç ömrü](tokens-overview.md)</li><li>Belirteç uyumluluk ayarları</li><li>Oturum davranışı</li><li>[Yaş sınırı](basic-age-gating.md)</li><li>[Parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul> |
| Kaydolun ve oturum açın v2 | Hayır | Kullanıcının hesabında hesap oluşturmasını veya oturum açmasını sağlar. Bu kullanıcı akışını kullanarak şunları yapılandırabilirsiniz: <ul><li>[Çok faktörlü kimlik doğrulaması](custom-policy-multi-factor-authentication.md)</li><li>[Yaş sınırı](basic-age-gating.md)</li><li>[Parola karmaşıklığı gereksinimleri](user-flow-password-complexity.md)</li></ul> |