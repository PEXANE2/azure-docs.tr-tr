---
title: Parola karmaşıklığı gereksinimlerini yapılandırma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de tüketiciler tarafından sağlanan parolalar için karmaşıklık gereksinimlerini yapılandırma.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c5ef550af0c7e19531ea19093ea937880f7dcf14
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78185650"
---
# <a name="configure-complexity-requirements-for-passwords-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'deki parolalar için karmaşıklık gereksinimlerini yapılandırma

Azure Active Directory B2C (Azure AD B2C), hesap oluştururken son kullanıcı tarafından sağlanan parolaların karmaşıklık gereksinimlerini değiştirmeyi destekler. Varsayılan olarak, Azure AD `Strong` B2C parolalar kullanır. Azure AD B2C, müşterilerin kullanabileceği parolaların karmaşıklığını denetlemek için yapılandırma seçeneklerini de destekler.

## <a name="password-rule-enforcement"></a>Parola kuralı zorlama

Kaydolma veya parola sıfırlama sırasında, son kullanıcıkarmaşıklık kurallarına uygun bir parola sağlamalıdır. Parola karmaşıklığı kuralları kullanıcı akışı başına uygulanır. Bir kullanıcı akışının kaydolma sırasında dört basamaklı bir pin gerektirmesi, başka bir kullanıcı akışının ise kaydolma sırasında sekiz karakterli bir dize gerektirmesi mümkündür. Örneğin, yetişkinler için çocuklardan farklı parola karmaşıklığına sahip bir kullanıcı akışı kullanabilirsiniz.

Oturum açma sırasında parola karmaşıklığı hiçbir zaman zorlanmaz. Geçerli karmaşıklık gereksinimini karşılamadığından, oturum açma sırasında kullanıcılardan parolalarını değiştirmeleri istenmez.

Parola karmaşıklığı, aşağıdaki kullanıcı akışları türlerinde yapılandırılabilir:

- Kaydolma veya Oturum Açma kullanıcı akışı
- Parola Sıfırlama kullanıcı akışı

Özel ilkeler kullanıyorsanız, parola[karmaşıklığını özel bir ilkede yapılandırabilirsiniz.](custom-policy-password-complexity.md)

## <a name="configure-password-complexity"></a>Parola karmaşıklığını yapılandırma

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Portal araç çubuğundaki **Dizin + Abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
3. Azure **portalında, Azure AD B2C'yi**arayın ve seçin.
4. **Kullanıcı akışlarını (ilkeler)** seçin.
2. Bir kullanıcı akışı seçin ve **Özellikler'i**tıklatın.
3. **Parola karmaşıklığı**altında, bu kullanıcı akışı için parola karmaşıklığını **Basit,** **Güçlü**veya **Özel**olarak değiştirin.

### <a name="comparison-chart"></a>Karşılaştırma Grafiği

| Karmaşıklık | Açıklama |
| --- | --- |
| Basit | En az 8 ila 64 karakterolan bir parola. |
| Güçlü | En az 8 ila 64 karakterolan bir parola. Küçük, büyük harf, sayı veya sembollerin 4 üzerinden 3'ü gerektirir. |
| Özel | Bu seçenek, parola karmaşıklığı kuralları üzerinde en fazla denetimi sağlar.  Özel bir uzunluk yapılandırmasağlar.  Ayrıca yalnızca numara parolalarını (pinleri) kabul etmeye de izin verir. |

## <a name="custom-options"></a>Özel seçenekler

### <a name="character-set"></a>Karakter Seti

Yalnızca basamakları (pinler) veya tam karakter kümesini kabul etmenizi sağlar.

- **Sayılar** yalnızca parola girerken yalnızca (0-9) basamaklara izin verir.
- **Tümü** herhangi bir harfe, numaraya veya sembole izin verir.

### <a name="length"></a>Uzunluk

Parolanın uzunluk gereksinimlerini denetlemenizi sağlar.

- **Minimum Uzunluk** en az 4 olmalıdır.
- **Maksimum Uzunluk** daha büyük veya minimum uzunluğa eşit olmalı ve en fazla 64 karakter olabilir.

### <a name="character-classes"></a>Karakter sınıfları

Parolada kullanılan farklı karakter türlerini denetlemenizi sağlar.

- **2 / 4: Küçük harf karakter, Büyük harf karakter, Sayı (0-9), Sembol** şifre en az iki karakter türleri içerir sağlar. Örneğin, bir sayı ve küçük bir karakter.
- **4'ün 3'ü: Küçük harf karakteri, Büyük harf karakteri, Sayı (0-9), Sembol** parolanın en az üç karakter türü içermesini sağlar. Örneğin, bir sayı, küçük bir karakter ve bir büyük harf karakter.
- **4/ 4: Küçük harf karakteri, Büyük harf karakteri, Sayı (0-9), Sembol,** parolanın karakter türleri için tümü içerdiğinden emin dir.

    > [!NOTE]
    > **4'ün 4'ünün** gerekli liği, son kullanıcı hayal kırıklığına neden olabilir. Bazı çalışmalar, bu gereksinimin parola entropiyi iyileştirmediğini göstermiştir. Bkz. [NIST Şifre Yönergeleri](https://pages.nist.gov/800-63-3/sp800-63b.html#appA)
