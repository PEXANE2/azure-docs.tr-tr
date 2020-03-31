---
title: Azure Active Directory B2C'de Çok Faktörlü Kimlik Doğrulama | Microsoft Dokümanlar
description: Azure Active Directory B2C tarafından güvence altına alınmıştır tüketiciye dönük uygulamalarda Çok Faktörlü Kimlik Doğrulama'yı etkinleştirme.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 25724ba82e57c5e3800fa1a989dd4f504df1c163
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78189285"
---
# <a name="enable-multi-factor-authentication-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de çok faktörlü kimlik doğrulamayı etkinleştirme

Azure Active Directory B2C (Azure AD B2C), uygulamalarınızda kaydolma ve kaydolma deneyimleri ne kadar ikinci bir güvenlik katmanı ekleyebilmeniz için doğrudan [Azure Çok Faktörlü Kimlik Doğrulaması](../active-directory/authentication/multi-factor-authentication.md) ile tümleşir. Tek bir kod satırı yazmadan çok faktörlü kimlik doğrulamasını etkinleştirin. Kaydol ve oturum açma kullanıcı akışlarını zaten oluşturduysanız, çok faktörlü kimlik doğrulamayı yine de etkinleştirebilirsiniz.

Bu özellik, uygulamaların aşağıdaki gibi senaryoları işlemesine yardımcı olur:

- Bir uygulamaya erişmek için çok faktörlü kimlik doğrulamasına gerek yok, ancak başka bir uygulamaya erişmesi gerekir. Örneğin, müşteri sosyal veya yerel bir hesapla otomatik sigorta uygulamasında oturum açabilir, ancak aynı dizinde kayıtlı ev sigortası uygulamasına erişmeden önce telefon numarasını doğrulaması gerekir.
- Genel olarak bir uygulamaya erişmek için çok faktörlü kimlik doğrulamasına gerek yok, ancak uygulamanın içindeki hassas bölümlere erişmek için gerekli. Örneğin, müşteri sosyal veya yerel bir hesapla bir bankacılık uygulamasında oturum açabilir ve hesap bakiyesini kontrol edebilir, ancak banka havalesi denemeden önce telefon numarasını doğrulaması gerekir.

## <a name="set-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulamayı ayarlama

Bir kullanıcı akışı oluşturduğunuzda, çok faktörlü kimlik doğrulamasını etkinleştirme seçeneğiniz vardır.

![Çok faktörlü kimlik doğrulamayı ayarlama](./media/custom-policy-multi-factor-authentication/add-policy.png)

**Çok Faktörlü kimlik doğrulamasını** **Etkin**olarak ayarlayın.

Deneyimi doğrulamak için **Kullanıcı akışını çalıştır'ı** kullanabilirsiniz. Aşağıdaki senaryoyu onaylayın:

Çok faktörlü kimlik doğrulama adımı oluşmadan önce kiracınızda bir müşteri hesabı oluşturulur. Adım sırasında, müşteriden bir telefon numarası sağlaması ve doğrulaması istenir. Doğrulama başarılı olursa, telefon numarası daha sonra kullanılmak üzere hesaba eklenir. Müşteri iptal etse veya çıksa bile, müşteriden çok faktörlü kimlik doğrulaması etkinleştirilmiş bir sonraki oturum açma sırasında bir telefon numarasını yeniden doğrulaması istenebilir.

## <a name="add-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulama ekleme

Daha önce oluşturduğunuz bir kullanıcı akışında çok faktörlü kimlik doğrulamasını etkinleştirebilirsiniz.

Çok faktörlü kimlik doğrulamayı etkinleştirmek için:

1. Kullanıcı akışını açın ve ardından **Özellikler'i**seçin.
2. Çok **Faktörlü kimlik doğrulamanın**yanında **Etkin'i**seçin.
3. Sayfanın üst kısmından **Kaydet**'e tıklayın.


