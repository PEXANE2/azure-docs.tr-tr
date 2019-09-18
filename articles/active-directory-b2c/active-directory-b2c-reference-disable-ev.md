---
title: Azure Active Directory B2C | tüketici kaydı sırasında e-posta doğrulamayı devre dışı bırakma Microsoft Docs
description: Azure Active Directory B2C ' de tüketici kaydı sırasında e-posta doğrulamanın nasıl devre dışı bırakılacağını gösteren bir konu.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: d827843bdbaff1ad95dce3318f39867abe8b2d0f
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71065630"
---
# <a name="disable-email-verification-during-consumer-sign-up-in-azure-active-directory-b2c"></a>Tüketici kaydı sırasında e-posta doğrulamayı devre dışı bırakma Azure Active Directory B2C
Etkinleştirildiğinde, Azure Active Directory B2C (Azure AD B2C) bir tüketiciye bir e-posta adresi girerek ve yerel hesap oluşturarak uygulamalara kaydolma olanağı sağlar. Azure AD B2C, tüketicilerin kaydolma işlemi sırasında doğrulamasını zorunlu kılarak geçerli e-posta adreslerini sağlar. Ayrıca kötü amaçlı otomatikleştirilmiş bir işlemin uygulamalar için sahte hesaplar oluşturmasını engeller.

Bazı uygulama geliştiricileri, kaydolma işlemi sırasında e-posta doğrulamasını atlamayı tercih eder ve bunun yerine tüketicilere daha sonra e-posta adresini doğrular. Bunu desteklemek için Azure AD B2C, e-posta doğrulamasını devre dışı bırakacak şekilde yapılandırılabilir. Bunun yapılması daha sorunsuz bir kaydolma işlemi oluşturur ve geliştiricilere, e-posta adreslerini doğrulayan tüketicileri ayırt etme esnekliği sunar.

Varsayılan olarak, kaydolma Kullanıcı akışlara e-posta doğrulaması açıktır. Devre dışı bırakmak için aşağıdaki adımları kullanın:

1. **Kullanıcı akışları**' na tıklayın.
2. Açmak için Kullanıcı akışınıza (örneğin, "B2C_1_SiUp") tıklayın.
3. **Sayfa düzenleri**' ne tıklayın.
4. **Yerel hesap kaydolma sayfası**' na tıklayın.
5. **Kullanıcı öznitelikleri** bölümünün altındaki **ad** sütununda **e-posta adresi** ' ne tıklayın.
6. **Doğrulama gerektiriyor**altında **Hayır**' ı seçin.
7. Dikey pencerenin en üstündeki **Kaydet**'e tıklayın. İşiniz bitti!

> [!NOTE]
> Kaydolma işleminde e-posta doğrulamasını devre dışı bırakmak istenmeyen postalara neden olabilir. Varsayılan olanı devre dışı bırakırsanız, kendi doğrulama sisteminizi eklemeniz önerilir.
>
>
