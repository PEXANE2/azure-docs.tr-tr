---
title: Kimlik güvenli puanı nedir? - Azure Etkin Dizin
description: Dizininizin güvenlik duruşunu geliştirmek için kimlik güvenli puanını nasıl kullanabilirsiniz?
services: active-directory
ms.service: active-directory
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: tilarso
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0f75dea2cffbe710bf2778ceab5eacc91ffcca9c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77523126"
---
# <a name="what-is-the-identity-secure-score-in-azure-active-directory"></a>Azure Active Directory'de kimlik güvenli puanı nedir?

Azure AD kiracınız ne düzeyde güvenli? Bu soruyu nasıl yanıtladığınızı bilmiyorsanız, bu makalede kimlik güvenli puanının kimlik güvenliği duruşunuzu izlemenize ve geliştirmenize nasıl yardımcı olduğu açıklanmaktadır.

## <a name="what-is-an-identity-secure-score"></a>Kimlik güvenliği puanı nedir?

Kimlik güvenli puanı, Microsoft'un güvenlik için en iyi uygulama önerileriyle ne kadar uyumlu olduğunuza ilişkin bir gösterge olarak işlev gösteren 1 ile 223 arasındaki sayıdır. Kimlik güvenli puanındaki her iyileştirme eylemi, özel yapılandırmanıza göre uyarlanır.  

![Güvenlik puanı](./media/identity-secure-score/identity-secure-score-overview.png)

Puanın yardımıyla:

- Kimlik güvenliği duruşunuzu nesnel olarak ölçebilirsiniz
- Kimlik güvenliği geliştirmelerini planlayabilirsiniz
- Geliştirmelerinizin başarısını gözden geçirebilirsiniz

Puana ve ilgili bilgilere kimlik güvenliği puanı panosundan erişebilirsiniz. Bu panoda şunları bulursunuz:

- Kimliğiniz güvenli puan
- Kimlik güvenli puanınızın aynı sektördeki ve benzer boyuttaki diğer kiracılarla karşılaştırışını gösteren bir karşılaştırma grafiği
- Kimlik güvenli puanınızın zaman içinde nasıl değiştiğini gösteren bir eğilim grafiği
- Olası iyileştirmelerin listesi

Geliştirme eylemlerini izleyerek:

- Güvenlik duruşunuzu ve puanınızı geliştirin
- Kimlik yatırımlarınızın bir parçası olarak kuruluşunuzun kullanabileceği özelliklerden yararlanın

## <a name="how-do-i-get-my-secure-score"></a>Güvenlik puanımı nasıl alabilirim?

Kimlik güvenli puanı Azure AD'nin tüm sürümlerinde kullanılabilir. Kuruluşlar, Azure **portalı** > **Azure Active Directory** > **Security** > **Identity Secure Score'dan**kimlik güvenli puanlarına erişebilirler.

## <a name="how-does-it-work"></a>Nasıl çalışır?

Her 48 saatte bir Azure güvenlik yapılandırmanıza bakar ve sizin ayarlarınızı önerilen en iyi yöntemlerle karşılaştırır. Bu değerlendirmenin sonucuna bağlı olarak, dizininiz için yeni bir puan hesaplanır. Güvenlik yapılandırmanızın en iyi uygulama kılavuzuyla tam olarak uyumlu olmaması ve geliştirme eylemlerinin yalnızca kısmen karşılanmış olması mümkündür. Bu senaryolarda, yalnızca denetim için kullanılabilir maksimum puanın bir kısmı verilir.

Her öneri, Azure AD yapılandırmanız temelinde ölçülür. En iyi uygulama önerisini etkinleştirmek için üçüncü taraf ürünleri kullanıyorsanız, bu yapılandırmayı bir geliştirme eyleminin ayarlarında belirtebilirsiniz. Ayrıca, ortamınız için geçerli değilse, göz ardı edilecek öneriler belirleme seçeneğiniz de var. Yoksayılan öneri puanınızın hesaplanmasına katılmaz.

![Eylemi üçüncü taraflar tarafından kapsanan olarak yoksayma veya işaretleme](./media/identity-secure-score/identity-secure-score-ignore-or-third-party-reccomendations.png)

## <a name="how-does-it-help-me"></a>Bana nasıl yardımcı olur?

Güvenlik puanının yardımıyla:

- Kimlik güvenliği duruşunuzu nesnel olarak ölçebilirsiniz
- Kimlik güvenliği geliştirmelerini planlayabilirsiniz
- Geliştirmelerinizin başarısını gözden geçirebilirsiniz

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

### <a name="who-can-use-the-identity-secure-score"></a>Kimlik güvenliği puanını kimler kullanabilir?

Kimlik güvenliği puanı aşağıdaki roller tarafından kullanılabilir:

- Genel yönetici
- Güvenlik yöneticisi
- Güvenlik okuyucuları

### <a name="how-are-controls-scored"></a>Kontroller nasıl puanlandırılır?

Kontroller iki şekilde puanlanabilir. Bazı ikili bir şekilde puanlandırılır - eğer özellik veya ayar bizim tavsiyeye göre yapılandırılmış varsa puan% 100 olsun. Diğer puanlar toplam yapılandırmanın yüzdesi olarak hesaplanır. Örneğin, iyileştirme önerisi, tüm kullanıcılarınızı MFA ile korursanız ve korunan toplam 100 kullanıcıdan yalnızca 5'ine sahipseniz 30 puan alırsınız, size 2 puan (5 korumalı / 100 toplam * 30 max pts = 2 puan kısmi puan) civarında kısmi puan verilir.

### <a name="what-does-not-scored-mean"></a>[Puanlanmadı] ne anlama geliyor?

[Puanlanmadı] olarak etiketlenen eylemler, kuruluşunuzda gerçekleştirebileceğiniz ama araca bağlanmadığından (henüz!) puanlanmayacak olan eylemlerdir. Dolayısıyla, yine de güvenliğinizi geliştirebilirsiniz ama şu anda bu eylemler için puan almazsınız.

### <a name="how-often-is-my-score-updated"></a>Puanım ne sıklıkta güncelleştirilir?

Puan günde bir kez hesaplanır (yaklaşık 24:00 PST). Ölçülen eylemde değişiklik yaparsanız, puan ertesi gün otomatik olarak güncelleştirilir. Değişikliğin puanınıza yansıması 48 saat kadar sürebilir.

### <a name="my-score-changed-how-do-i-figure-out-why"></a>Puanım değişti. Nedenini nasıl anlayabilirim?

[Microsoft 365 güvenlik merkezine](https://security.microsoft.com/)gidin ve burada microsoft güvenli puanınızı tamamlayın. Geçmiş sekmesindeki ayrıntılı değişiklikleri inceleyerek güvenli puanınızdaki tüm değişiklikleri kolayca görebilirsiniz.

### <a name="does-the-secure-score-measure-my-risk-of-getting-breached"></a>Güvenli skor ihlal edilebiki riskimi ölçüyor mu?

Kısacası, hayır. Güvenli skor, ihlal edilme olasılığınız hakkında kesin bir ölçü ifade etmez. Güvenlik ihlaline uğrama riskini azaltabilecek özellikleri ne düzeyde benimsediğinizi ortaya koyar. Hiçbir hizmet ihlal edilmeyeceğinigaranti edemez ve güvenli puan hiçbir şekilde garanti olarak yorumlanmamalıdır.

### <a name="how-should-i-interpret-my-score"></a>Puanımı nasıl yorumlayabilirim?

Önerilen güvenlik özelliklerini yapılandırmanızdan veya güvenlikle ilgili görevleri (raporları okuma gibi) gerçekleştirmenizden dolayı size puan verilir. Kullanıcılarınız için Multi-Factor Authentication'ı (MFA) etkinleştirmeniz gibi bazı eylemler, kısmen tamamlanmış olarak puanlanır. Güvenli puanınız, kullandığınız Microsoft güvenlik hizmetlerini doğrudan temsil eder. Güvenliğin kullanılabilirlikle dengelenmesi gerektiğini unutmayın. Tüm güvenlik denetimlerinin bir de kullanıcı etkisi bileşeni vardır. Düşük kullanıcı etkisine sahip denetimlerin, kullanıcılarınızın gündelik işlemleri üzerinde çok az etkisi olması veya hiç olmaması gerekir.

Puan geçmişinizi görmek için [Microsoft 365 güvenlik merkezine](https://security.microsoft.com/) gidin ve genel Microsoft güvenli puanınızı gözden geçirin. Genel güvenli puanınızdaki değişiklikleri gözden geçirebilirsiniz. Belirli bir tarih seçerek o gün hangi denetimlerin etkinleştirildiğini ve her birinden kaç puan kazandığınızı görün.

### <a name="how-does-the-identity-secure-score-relate-to-the-office-365-secure-score"></a>Kimlik güvenliği puanı ile Office 365 güvenlik puanı arasında nasıl bir ilişki vardır?

[Microsoft güvenli puanı](https://docs.microsoft.com/office365/securitycompliance/microsoft-secure-score) beş farklı denetim ve puan kategorisi içerir:

- Kimlik
- Veri
- Cihazlar
- Altyapı
- Uygulamalar

Kimlik güvenli puanı, Microsoft güvenli puanının kimlik kısmını temsil eder. Bu çakışma, kimlik güvenli puanı ve Microsoft'taki kimlik puanı için önerileriniz aynı olduğu anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar

[Microsoft secure score hakkında daha fazla bilgi edinin](https://docs.microsoft.com/office365/securitycompliance/microsoft-secure-score)
