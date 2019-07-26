---
title: Azure Active Directory 'de kimlik koruması (YENİLENDİ) ile ilgili SSS ve bilinen sorunlar | Microsoft Docs
description: Azure Active Directory 'de kimlik koruması (yenileme) ile ilgili SSS ve bilinen sorunlar.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: troubleshooting
ms.date: 01/24/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5d5aa50aec98b3944aed92b9da49182f0608f34c
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68333896"
---
# <a name="faqs-and-known-issues-with-identity-protection-refreshed-in-azure-active-directory"></a>Azure Active Directory 'da kimlik koruması (yenileme) ile ilgili SSS ve bilinen sorunlar

## <a name="dismiss-user-risk-known-issues"></a>Kullanıcı riski bilinen sorunlarını kapat

Klasik kimlik koruması ' nda **Kullanıcı riskini** kapatma kullanıcının kimlik koruması (YENİLENDİ) üzerindeki risk GEÇMIŞINI **Azure AD**'ye ayarlar.

Kimlik koruması 'nda **Kullanıcı riskini** kapatma (yenilenmiş) kullanıcının kimlik koruması  **\<'ndaki risk geçmişinde, kullanıcının dikey penceresine\>işaret eden bir köprüyle yönetici adına sahip olan**aktöri ayarlar.

Kullanıcı riski geri ayırma akışında gecikme sürelerine neden olan geçerli bir bilinen sorun var. "Kullanıcı risk ilkeniz" varsa, bu ilke "Kullanıcı riskini kapat" seçeneğine tıklanarak kapatılan kullanıcılara uygulamayı durdurur. Ancak, UX ile kapatılan kullanıcıların "risk durumunu" yenilemesinin bilinen gecikmeleri vardır. Geçici bir çözüm olarak, en son kullanıcıyı "risk durumu" görmek için tarayıcı düzeyindeki sayfayı yenileyin.

## <a name="risky-users-report-known-issues"></a>Riskli kullanıcılar bilinen sorunları raporlar

**Kullanıcı** adı alanındaki sorgular büyük/küçük harfe duyarlıdır, ancak **ad** alanındaki sorgular büyük/küçük harfe duyarlıdır.

**Tarihleri göster** değiştirme, **RISK Son güncelleme** sütununu gizler. Sütunu yeniden eklemek için riskli kullanıcılar dikey penceresinin en üstündeki **sütunlar** ' a tıklayın.

Klasik kimlik korumasında **tüm olayları kapat** risk olaylarının durumunu **Kapalı (çözüldü)** olarak ayarlar.

## <a name="risky-sign-ins-report-known-issues"></a>Riskli oturum açma işlemleri raporu bilinen sorunlar

Bir risk olayında **Çözümle** , durumu **risk tabanlı Ilke tarafından yönetilen MFA 'ya geçen kullanıcılara**ayarlar.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="why-cant-i-set-my-own-risk-levels-for-each-risk-event"></a>Neden her risk olayı için kendi risk düzeyimi ayarlayamıyorum?

Kimlik koruması 'ndaki risk düzeyleri algılama hassasiyetini temel alır ve denetimli makine öğrenimi tarafından desteklenir. Kullanıcıların hangi deneyimle sunulduğunu özelleştirmek için, Yönetici Kullanıcı risk ve oturum açma risk Ilkelerinden belirli kullanıcıları/grupları dahil edebilir/hariç tutabilir.

### <a name="why-does-the-location-of-a-sign-in-not-match-where-the-user-truly-signed-in-from"></a>Bir oturum açma konumu, kullanıcının gerçekten gerçekten oturum açmasından nasıl eşleşmez?

IP coğrafi coğrafi eşleme, sektör genelinde bir zorluk dır. Oturum açma raporunda listelenen konumun gerçek konumla eşleşmediğinden, lütfen desteğe ulaşın. 

### <a name="how-do-the-feedback-mechanisms-in-identity-protection-work"></a>Kimlik koruması 'ndaki geri bildirim mekanizmaları nasıl çalışır?

**Tehlikeden emin olun** (bir oturum açma sırasında) – oturum açma kimliğinin kimlik sahibi tarafından gerçekleştirilmediğini ve bir güvenliğinin aşılmasına neden olduğunu Azure AD Kimlik Koruması bildirir.

- Bu geri bildirimin alınması sırasında, oturum açma ve Kullanıcı risk durumunu, **güvenliği aşılmış** ve risk düzeyini **yüksek**olarak geçeceğiz.

- Ayrıca, risk değerlendirmesinde gelecekteki geliştirmeler için Machine Learning sistemlerimize bilgi sağlıyoruz.

    > [!NOTE]
    > Kullanıcı zaten düzeltildiyse, oturum açma ve Kullanıcı risk durumunu onaylanabilecek ve risk düzeyi **yüksek**olarak taşıdığı için **güvenliği aşılmış** '  i tıklamayın.

**Güvenli onaylama** (bir oturum açma sırasında) – oturum açma kimliğinin kimlik sahibi tarafından gerçekleştirildiğini Azure AD Kimlik Koruması ve bir uzlaşmaya işaret olmadığını bildirir.

- Bu geri bildirimin alınması sırasında, oturum açma (Kullanıcı değil) risk durumunu **onaylı güvenli** ve risk düzeyine **-** taşıyacağız.

- Ayrıca, risk değerlendirmesinde gelecekteki geliştirmeler için Machine Learning sistemlerimize bilgi sağlıyoruz.

    > [!NOTE]
    > Kullanıcının güvenliğinin aşılmadığını düşünüyorsanız, oturum açma düzeyinde **onaylanmış güvenli** kullanmak yerine Kullanıcı düzeyinde Kullanıcı **riskini kapat** seçeneğini kullanın. Kullanıcı düzeyinde **Kullanıcı** riskini kapatma, Kullanıcı riskini ve tüm geçmiş riskli oturum açma işlemlerini ve risk olaylarını kapatır.

### <a name="why-am-i-seeing-a-user-with-a-low-or-above-risk-score-even-if-no-risky-sign-ins-or-risk-events-are-shown-in-identity-protection"></a>Kimlik koruması 'nda hiçbir riskli oturum açma veya risk olayı gösterilmese bile, bir kullanıcıyı düşük (veya üzeri) risk puanı ile birlikte görmem gerekir mi?

Kullanıcı riskini doğası gereği, kullanım süreleri dolana kadar, kimlik koruması 'nda son riskli oturum açma veya risk olayları olmasa bile Kullanıcı düşük veya daha yüksek bir Kullanıcı riski oluşturabilir. Bu durum, bir kullanıcının riskli oturum açma ve risk olaylarının ayrıntılarını depolayabileceği zaman dilimini aşacak şekilde gerçekleştiyse meydana gelebilir. Kötü aktörlerin, saldırılarına devam etmeden önce güvenliği aşılmış bir kimliğin arkasındaki 140 gün içinde müşterilerin ortamında kalması bilindiği için Kullanıcı riskini sona ermedik. Müşteriler kullanıcının risk zaman çizelgesini inceleyerek, bir kullanıcının neden risk altında olduğunu anlayabilir:`Azure Portal > Azure Active Directory > Risky users’ report > Click on an at-risk user > Details’ drawer > Risk history tab`

### <a name="why-does-a-sign-in-have-a-sign-in-risk-aggregate-score-of-high-when-the-detections-associated-with-it-are-of-low-or-medium-risk"></a>Neden bir oturum açma, bununla ilişkili algılamalar düşük veya orta riskli olduğunda bir "oturum açma riski (toplama)" puanı elde ediyor mu?

Yüksek toplu risk puanı, oturum açma işleminin diğer özelliklerine veya bu oturum açma için birden fazla algılamanın tetiklenmesi durumunda olabilir. Üstelik, oturum açma ile ilişkili algılamalar yüksek riskli olsa da, oturum açma, ortamın bir oturum açma riski (toplama) olabilir. 
