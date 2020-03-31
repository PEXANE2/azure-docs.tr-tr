---
title: Azure Active Directory B2C'de yaş gating'i etkinleştirme | Microsoft Dokümanlar
description: Uygulamanızı kullanarak reşit olmayanları nasıl tanımlayınız hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 56cbeb8e8fe21f4b39c2f5c6af43e83ae330e5d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78189982"
---
# <a name="enable-age-gating-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de Yaş Gating'i etkinleştirme

>[!IMPORTANT]
>Bu özellik genel önizleme aşamasındadır. Üretim uygulamaları için özellik kullanmayın.
>

Azure Active Directory B2C'de (Azure AD B2C) yaş fark, uygulamanızı kullanmak isteyen reşit olmayanları belirlemenize olanak tanır. Reşit olmayan kişinin uygulamaya oturum etmesini engellemeyi seçebilirsiniz. Kullanıcılar ayrıca uygulamaya geri dönebilir ve yaş gruplarını ve ebeveyn onam durumlarını belirleyebilirler. Azure AD B2C, ebeveynlerinin izni olmadan reşit olmayanları engelleyebilir. Azure AD B2C, uygulamanın reşit olmayanlarla ne yapacağına karar vermesine izin verecek şekilde de ayarlanabilir.

[Kullanıcı akışınızda](user-flow-overview.md)yaş akışını etkinleştirdikten sonra, kullanıcılara ne zaman doğdukları ve hangi ülkede/bölgede yaşadıkları sorulur. Daha önce bilgileri girmemiş bir kullanıcı oturum açsa, bir sonraki oturum açabildiklerinde bu bilgileri girmeleri gerekir. Kurallar, bir kullanıcı her girdiği zaman uygulanır.

Azure AD B2C, reşit olup olmadıklarını belirlemek için kullanıcının girdiği bilgileri kullanır. **AgeGroup** alanı daha sonra hesaplarında güncelleştirilir. Değeri `null`, , `Undefined` `Minor`, `Adult`, `NotAdult`ve .  **AgeGroup** ve **consentProvidedForMinor** alanları daha sonra **yasalAgeGroupClassification**değerini hesaplamak için kullanılır.

Yaş gating iki yaş değerleri içerir: bir kişinin artık reşit olmayan olarak kabul edilir yaş ve hangi bir reşit olmayan ebeveyn onayı olması gereken yaş. Aşağıdaki tabloda, reşit olmayan ve rıza gerektiren küçük bir çocuk tanımlamak için kullanılan yaş kuralları listelenir.

| Ülke/Bölge | Ülke/Bölge adı | Küçük rıza yaşı | Reşit olmayan yaş |
| -------------- | ------------------- | ----------------- | --------- |
| Varsayılan | None | None | 18 |
| AE | Birleşik Arap Emirlikleri | None | 21 |
| AT | Avusturya | 14 | 18 |
| BE | Belçika | 14 | 18 |
| BG | Bulgaristan | 16 | 18 |
| BH | Bahreyn | None | 21 |
| CM | Kamerun | None | 21 |
| CY | Kıbrıs | 16 | 18 |
| CZ | Çek Cumhuriyeti | 16 | 18 |
| DE | Almanya | 16 | 18 |
| DK | Danimarka | 16 | 18 |
| EE | Estonya | 16 | 18 |
| EG | Mısır | None | 21 |
| ES | İspanya | 13 | 18 |
| GS | Fransa | 16 | 18 |
| GB | Birleşik Krallık | 13 | 18 |
| GR | Yunanistan | 16 | 18 |
| HR | Hırvatistan | 16 | 18 |
| HU | Macaristan | 16 | 18 |
| IE | İrlanda | 13 | 18 |
| BT | İtalya | 16 | 18 |
| KR | Kore Cumhuriyeti | 14 | 18 |
| LT | Litvanya | 16 | 18 |
| LU | Lüksemburg | 16 | 18 |
| LV | Letonya | 16 | 18 |
| MT | Malta | 16 | 18 |
| NA | Namibya | None | 21 |
| NL | Hollanda | 16 | 18 |
| PL | Polonya | 13 | 18 |
| PT | Portekiz | 16 | 18 |
| RO | Romanya | 16 | 18 |
| SE | İsveç | 13 | 18 |
| SG | Singapur | None | 21 |
| SI | Slovenya | 16 | 18 |
| SK | Slovakya | 16 | 18 |
| TD | Çad | None | 21 |
| TH | Tayland | None | 20 |
| TW | Tayvan | None | 20 |
| ABD | Amerika Birleşik Devletleri | 13 | 18 |

## <a name="age-gating-options"></a>Yaş gating seçenekleri

### <a name="allowing-minors-without-parental-consent"></a>Ebeveynlerinin rızası olmadan reşit olmayanlara izin verme

Kaydolma, oturum açma veya her ikisine birden izin veren kullanıcı akışları için, reşit olmayanların uygulamanıza rızası olmadan girmesine izin vermeyi seçebilirsiniz. Ebeveyn izni olmayan reşit olmayan ların normal oturum açmalarına veya normal olarak kaydolmalarına izin verilir ve Azure AD B2C, **yasalAgeGroupClassification** iddiasıyla bir kimlik belirteci yayınlar. Bu talep, ebeveynlerinin rızasını toplamak ve **onay** alanını güncellemek gibi kullanıcıların sahip olduğu deneyimi tanımlar.

### <a name="blocking-minors-without-parental-consent"></a>Ebeveynlerinin rızası olmadan reşit olmayanları engelleme

Kaydolma, oturum açma veya her ikisine birden izin veren kullanıcı akışları için, uygulamanın izni olmadan reşit olmayanları engellemeyi seçebilirsiniz. Azure AD B2C'de engellenen kullanıcıları işlemek için aşağıdaki seçenekler kullanılabilir:

- Uygulamaya bir JSON geri gönderme - bu seçenek, reşit olmayan bir kişinin engellendiğini uygulamaya yanıt gönderir.
- Bir hata sayfası göster - kullanıcıya uygulamaya erişemediklerini bildiren bir sayfa gösterilir.

## <a name="set-up-your-tenant-for-age-gating"></a>Kiracınızı yaş gating için ayarlama

Kullanıcı akışında yaş gating kullanmak için, ek özelliklere sahip kiracı yapılandırmanız gerekir.

1. Üst menüdeki **Dizin + abonelik** filtresini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Kiracınızı içeren dizini seçin.
2. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin, Azure **AD B2C'yi**arayın ve seçin.
3. Soldaki menüde kiracınız için **Özellikler'i** seçin.
2. Yaş **gating** bölümü altında, **Yapıla'ya**tıklayın.
3. İşlemin tamamlanmasını bekleyin ve kiracınız yaş gating için ayarlanır.

## <a name="enable-age-gating-in-your-user-flow"></a>Kullanıcı akışınızda yaş gating etkinleştirme

Kiracınız yaş gating'i kullanacak şekilde ayarlandıktan sonra, bu özelliği etkinleştirildiği [kullanıcı akışlarında](user-flow-versions.md) kullanabilirsiniz. Aşağıdaki adımlarla yaş gating etkinleştirin:

1. Yaş gating etkin olan bir kullanıcı akışı oluşturun.
2. Kullanıcı akışını oluşturduktan sonra menüdeki **Özellikler'i** seçin.
3. Yaş **gating** bölümünde **Etkin'i**seçin.
4. Daha sonra, reşit olmayan olarak tanımlanan kullanıcıları nasıl yönetmek istediğinize siz karar verirsiniz. **Kaydolmak veya kaydolmak**için, `Allow minors to access your application` seçin `Block minors from accessing your application`veya . Engelleyici reşit olmayanlar seçilirse, seçin `Send a JSON back to the application` veya `Show an error message`.




