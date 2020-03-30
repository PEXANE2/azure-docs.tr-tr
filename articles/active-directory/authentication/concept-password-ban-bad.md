---
title: Dinamik olarak yasaklanmış parolalar - Azure Active Directory
description: Azure AD dinamik olarak yasaklanmış parolalarla ortamınızdan zayıf parolaları yasaklayın
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef127d120b32f5344bce0f68d79f48401087f0ce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79264004"
---
# <a name="eliminate-bad-passwords-in-your-organization"></a>Kuruluşunuzda kötü parolaları ortadan kaldırma

Sektör liderleri, aynı parolayı birden çok yerde kullanmamanızı, karmaşık hale getirmemenizi ve "Password123" gibi basit hale getirmemenizi söyler. Kuruluşlar, kullanıcılarının en iyi uygulama kılavuzunu takip ettiğini nasıl garanti edebilir? Kullanıcıların zayıf parolalar kullanmadığından ve hatta zayıf parolalarda varyasyonkullanmadığından nasıl emin olabilirler?

Daha güçlü parolalara sahip olmanın ilk adımı, kullanıcılarınıza rehberlik etmektir. Microsoft'un bu konudaki güncel kılavuzunu aşağıdaki bağlantıdan bulabilirsiniz:

[Microsoft Şifre Kılavuzu](https://www.microsoft.com/research/publication/password-guidance)

Iyi bir rehberlik olması önemlidir, ama biz bile birçok kullanıcı hala zayıf şifreleri seçerek sona erecek biliyorum. Azure AD Parola Koruması, bilinen zayıf parolaları ve bunların türevlerini algılayıp engelleyerek ve kuruluşunuza özel ek zayıf koşulları isteğe bağlı olarak engelleyerek kuruluşunuzu korur.

Güncel güvenlik çabaları hakkında daha fazla bilgi için [Microsoft Güvenlik İstihbaratı Raporu'na](https://www.microsoft.com/security/operations/security-intelligence-report)bakın.

## <a name="global-banned-password-list"></a>Genel yasaklı parola listesi

Azure AD Kimlik Koruma ekibi, sık kullanılan zayıf veya gizliliği ihlal edilen parolaları veya daha spesifik olarak zayıf parolalar için temel olarak kullanılan zayıf temel terimleri arayan Azure AD güvenlik telemetri verilerini sürekli olarak analiz eder. Bu tür zayıf terimler bulunduğunda, bunlar genel olarak yasaklanmış parola listesine eklenir. Genel olarak yasaklı parola listesinin içeriği herhangi bir dış veri kaynağına dayanmaz. Genel yasaklı parola listesi tamamen Azure AD güvenlik telemetrisinin ve analizinin devam eden sonuçlarına dayanır.

Azure AD'deki herhangi bir kiracıdaki herhangi bir kullanıcı için yeni bir parola değiştirildiğinde veya sıfırlandığında, genel olarak yasaklı parola listesinin geçerli sürümü, parolanın gücünü doğrularken anahtar girişi olarak kullanılır. Bu doğrulama, tüm Azure AD müşterileri için çok daha güçlü parolalar sağlar.

> [!NOTE]
> Siber suçlular da saldırılarında benzer stratejiler kullanırlar. Bu nedenle Microsoft bu listenin içeriğini herkese açık olarak yayımlamaz.

## <a name="custom-banned-password-list"></a>Özel yasaklı şifre listesi

Bazı kuruluşlar, Microsoft'un özel yasaklı parola listesi olarak adlandırdığı genel yasaklı parola listesinin üstüne kendi özelleştirmelerini ekleyerek güvenliği daha da artırmak isteyebilir. Microsoft, bu listeye eklenen terimlerin öncelikle kuruluşa özgü terimlere odaklanmasını önerir:

- Marka adları
- Ürün adları
- Konumlar (örneğin, şirket merkezi gibi)
- Şirkete özel iç terimler
- Belirli bir şirket anlamı olan kısaltmalar.

Terimler özel yasaklı parola listesine eklendikten sonra, parolaları doğrularken genel yasaklı parola listesindeki terimlerle birleştirilir.

> [!NOTE]
> Özel yasaklı parola listesi en fazla 1000 terime sahip olmakla sınırlıdır. Son derece büyük parola listelerini engellemek için tasarlanmaz. Özel yasaklı parola listesinin avantajlarından tam olarak yararlanmak için Microsoft, özel yasaklı listeye yeni terimler eklemeden önce parola değerlendirme algoritmasını (bkz. [parolaların nasıl değerlendirildiğini)](concept-password-ban-bad.md#how-are-passwords-evaluated)incelemenizi ve anlamanızı önerir. Algoritmanın nasıl çalıştığını anlamak, işletmenizin çok sayıda zayıf parolayı ve bunların türevlerini verimli bir şekilde algılamasını ve engellemesini sağlar.

Örneğin: Merkezi Londra'da bulunan ve "Widget" adlı bir ürün yapan "Contoso" adlı bir müşteri düşünün. Böyle bir müşteri için, bu terimlerin belirli varyasyonlarını engellemeye çalışmak hem savurgan hem de daha az güvenli olacaktır:

- "Contoso!1"
- "Contoso@London"
- "ContosoWidget"
- "! Contoso"
- "LondonHQ"
- ... Etcetera

Bunun yerine, yalnızca temel terimleri engellemek çok daha verimli ve güvenlidir:

- "Contoso"
- "Londra"
- "Widget"

Parola doğrulama algoritması daha sonra otomatik olarak zayıf türevleri ve yukarıdaki kombinasyonları engeller.

Özel yasaklı parola listesi ve şirket içi Active Directory tümleştirmesini etkinleştirme olanağı Azure portalı kullanılarak yönetilir.

![Kimlik Doğrulama Yöntemleri altında özel yasaklı parola listesini değiştirme](./media/concept-password-ban-bad/authentication-methods-password-protection.png)

## <a name="password-spray-attacks-and-third-party-compromised-password-lists"></a>Parola püskürtme saldırıları ve üçüncü taraf şifre listeleri

Önemli bir Azure AD parola koruma avantajı, parola püskürtme saldırılarına karşı savunmanıza yardımcı olmaktır. Bu tür davranışlar hesap kilitleme veya başka yollarla algılama olasılığını büyük ölçüde artırdığından, çoğu parola püskürtme saldırısı herhangi bir tek tek hesaba birkaç kereden fazla saldırmaya kalkışmaz. Bu nedenle, parola püskürtme saldırılarının çoğu, bir kuruluştaki hesapların her birine karşı bilinen en zayıf parolaların yalnızca az sayıda gönderilmesine dayanır. Bu teknik, saldırganın kolayca tehlikeye atabileceğiniz bir hesabı hızlı bir şekilde aramasına ve aynı zamanda olası algılama eşiklerini önlemesine olanak tanır.

Azure AD parola koruması, Azure AD tarafından görülen gerçek güvenlik telemetri verilerine dayalı olarak parola püskürtme saldırılarında kullanılması muhtemel bilinen tüm zayıf parolaları verimli bir şekilde engellemek için tasarlanmıştır.  Microsoft, daha önce bilinen güvenlik ihlallerinde ele geçirilen milyonlarca parolayı sıralayan üçüncü taraf web sitelerinin farkındadır. Üçüncü taraf parola doğrulama ürünlerinin bu milyonlarca parolayla kaba kuvvet karşılaştırmasına dayanması yaygındır. Microsoft, parola püskürtme saldırganları tarafından kullanılan tipik stratejiler göz önüne alındığında, bu tür tekniklerin genel parola gücünü artırmanın en iyi yolu olmadığını düşünüyor.

> [!NOTE]
> Microsoft'un genel olarak yasaklanan parola listesi, gizliliği ihlal edilmiş parola listeleri de dahil olmak üzere üçüncü taraf veri kaynaklarına dayanmaz.

Microsoft'un genel yasaklı listesi bazı üçüncü taraf toplu listelere kıyasla küçük olsa da, güvenlik etkileri gerçek parola püskürtme saldırılarında gerçek dünya güvenlik telemetrisinden kaynaklanmış olması yla artı Microsoft'un parola doğrulama algoritması akıllı bulanık eşleştirme tekniklerini kullanır. Sonuçta verimli bir şekilde algılamak ve kuruluşunuzda kullanılmaktan en yaygın zayıf parolaların milyonlarca engellemek. Özel yasaklı parola listesine kuruluşa özgü terimler eklemeyi seçen müşteriler de aynı algoritmadan yararlanır.

Parola tabanlı güvenlik sorunları yla ilgili ek bilgiler [Pa$$word](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Your-Pa-word-doesn-t-matter/ba-p/731984)önemli değildir gözden geçirilebilir.

## <a name="on-premises-hybrid-scenarios"></a>Şirket içi hibrit senaryolar

Yalnızca bulut hesaplarının korunması yararlıdır, ancak birçok kuruluş şirket içi Windows Server Etkin Dizini de dahil olmak üzere karma senaryolar tutar. Azure AD parola korumasının güvenlik avantajları, şirket içi aracıların yüklenmesi yoluyla Windows Server Active Directory ortamınıza da genişletilebilir. Artık Active Directory'de parolaları değiştiren veya sıfırlayan kullanıcıların ve yöneticilerin yalnızca bulut kullanıcılarıyla aynı parola ilkesine uymaları gerekiyor.

## <a name="how-are-passwords-evaluated"></a>Parolalar nasıl değerlendirilir?

Bir kullanıcı parolasını değiştirdiğinde veya sıfırladığında, yeni parola, genel ve özel yasaklı parola listelerindeki terimlerin birleşik listesiyle karşısına doğrulayarak (ikincisi yapılandırılırsa) güçlü ve karmaşıklık açısından denetlenir.

Bir kullanıcının parolası yasaklı bir parola içerse bile, genel parola aksi takdirde yeterince güçlüyse parola yine de kabul edilebilir. Yeni yapılandırılan bir parola, kabul edilip edilmemesi veya reddedilmesi gerekip gerekmeyeceğini belirlemek için genel gücünü değerlendirmek için aşağıdaki adımlardan geçer.

### <a name="step-1-normalization"></a>Adım 1: Normalleştirme

Yeni bir parola önce normalleştirme işleminden geçer. Bu teknik, küçük bir yasaklı parola kümesinin çok daha büyük bir potansiyel zayıf parola kümesiyle eşlenemesine olanak tanır.

Normalleştirme iki bölümden oluşur.  İlk olarak, tüm büyük harfler küçük harf olarak değiştirilir.  İkinci olarak, yaygın karakter değiştirmeleri gerçekleştirilir, örneğin:  

| Orijinal mektup  | Değiştirilen harf |
| --- | --- |
| '0'  | 'o' |
| '1'  | 'l' |
| '$'  | 's' |
| '\@'  | 'a' |

Örnek: "boş" parolanın yasaklandığını varsayalım ve kullanıcı parolasınıBl@nK" " olarak değiştirmeye çalışır. " "Bl@nközel olarak yasaklanmamış olsa da, normalleştirme işlemi bu parolayı yasaklı bir parola olan "boş"a dönüştürür.

### <a name="step-2-check-if-password-is-considered-banned"></a>Adım 2: Parolanın yasaklı kabul edilip edilemeyişamaz

#### <a name="fuzzy-matching-behavior"></a>Bulanık eşleştirme davranışı

Bulanık eşleştirme, normalleştirilmiş parolada, genel veya özel yasaklı parola listelerinde bulunan bir parola bulunip içermeden tanımlanabilmek için kullanılır. Eşleştirme işlemi, bir (1) karşılaştırmanın bir edit mesafesine dayanır.  

Örnek: "abcdef" parolasının yasaklandığını ve kullanıcının parolasını aşağıdakilerden biriyle değiştirmeye çalıştığını varsayalım:

'abcdeg' *(son karakter 'f' den 'g')* 'abcdefg' *'(g' sonuna eklenen)* 'abcde' *('f' izleyerek uçtan silindi)*

Yukarıdaki parolaların her biri özellikle yasaklı şifre "abcdef" eşleşmiyor. Ancak, her örnek yasaklı terim 'abcdef' 1 bir edit mesafesi içinde olduğundan, hepsi "abcdef" bir maç olarak kabul edilir.

#### <a name="substring-matching-on-specific-terms"></a>Substring eşleştirme (belirli terimlerüzerinde)

Alt dize eşlemesi, kullanıcının adı ve soyadının yanı sıra kiracı adını denetlemek için normalleştirilmiş parolada kullanılır (Etkin Dizin etki alanı denetleyicisindeki parolaları doğrularken kiracı adı eşleştirmesinin yapılmadığını unutmayın).

Örnek: şifresini "P0l123fb" olarak sıfırlamak isteyen Pol adlı bir kullanıcımız olduğunu varsayalım. Normalleştirme den sonra, bu şifre "pol123fb" olacaktır. Substring eşleştirme, parolanın kullanıcının ilk adı "Pol" içerdiğini bulur. "P0l123fb" özellikle yasaklı parola listesinde olmamasına rağmen, alt string eşleştirme parolada "Pol" bulundu. Bu nedenle bu parola reddedilir.

#### <a name="score-calculation"></a>Puan Hesaplama

Bir sonraki adım, kullanıcının normalize edilmiş yeni parolasındaki tüm yasaklı parola örneklerini belirlemektir. Ardından:

1. Bir kullanıcının parolasında bulunan her yasaklı parolaya bir puan verilir.
2. Kalan her benzersiz karaktere bir puan verilir.
3. Parolanın kabul edilemesi için en az beş (5) puan olmalıdır.

Sonraki iki örnek için, Contoso'nun Azure AD Parola Koruması kullandığını ve özel listelerinde "contoso" olduğunu varsayalım. "Boş"un genel listede olduğunu da varsayalım.

Örnek: Bir kullanıcı parolasını "C0ntos0Blank12" olarak değiştirir

Normalleştirmeden sonra, bu parola "contosoblank12" olur. Eşleştirme işlemi, bu parolanın iki yasaklı parola içerdiğini bulur: contoso ve boş. Bu parola daha sonra bir puan verilir:

[contoso] + [boş] + [1] + [2] = 4 puan Bu parola beş (5) puanın altında olduğundan, reddedilecektir.

Örnek: bir kullanıcı parolasınıContoS0Bl@nkf9" !"' olarak değiştirir.

Normalleştirmeden sonra, bu parola "contosoblankf9!" olur. Eşleştirme işlemi, bu parolanın iki yasaklı parola içerdiğini bulur: contoso ve boş. Bu parola daha sonra bir puan verilir:

[contoso] + [boş] + [f] + [9] + [!] = 5 puan Bu parola en az beş (5) puan olduğundan, kabul edilir.

   > [!IMPORTANT]
   > Yasaklı parola algoritmasının genel listeyle birlikte, devam eden güvenlik analizleri ve araştırmalarını temel alan Azure'da herhangi bir zamanda değişiklik yapabileceğini ve değiştirebileceğini lütfen unutmayın. Şirket içi DC aracı hizmeti için, güncelleştirilmiş algoritmalar yalnızca DC aracı yazılımı yeniden yüklendikten sonra etkili olur.

## <a name="license-requirements"></a>Lisans gereksinimleri

|   | Genel yasaklı parola listesi ile Azure AD parola koruması | Özel yasaklı parola listesi ile Azure AD parola koruması|
| --- | --- | --- |
| Yalnızca bulut kullanıcıları | Azure AD Ücretsiz | Azure AD Premium P1 veya P2 |
| Şirket içi Windows Server Active Directory'den senkronize edilen kullanıcılar | Azure AD Premium P1 veya P2 | Azure AD Premium P1 veya P2 |

> [!NOTE]
> Azure Active Directory ile senkronize olmayan şirket içi Windows Server Active Directory kullanıcıları, senkronize edilmiş kullanıcılar için varolan lisanslamayı temel alan Azure AD parola korumasından da yararlanır.

Maliyetler de dahil olmak üzere ek lisans bilgileri [Azure Active Directory fiyatlandırma sitesinde](https://azure.microsoft.com/pricing/details/active-directory/)bulunabilir.

## <a name="what-do-users-see"></a>Kullanıcıların ne görecek

Bir kullanıcı yasaklanacak bir şeyin parolasını sıfırlamaya çalıştığında, aşağıdaki hata iletisini görür:

Ne yazık ki, parolanız parolanızı kolayca tahmin edilebilir kılan bir sözcük, tümcecik veya desen içerir. Lütfen farklı bir parola ile yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel yasaklı parola listesini yapılandırma](howto-password-ban-bad.md)
- [Azure AD parola koruma aracılarını şirket içinde etkinleştirme](howto-password-ban-bad-on-premises-deploy.md)
