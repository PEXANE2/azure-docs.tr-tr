---
title: Azure Etkin Dizinde Kimlik Koruması Için SSS
description: Sık sorulan sorular Azure AD Kimlik Koruması
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: troubleshooting
ms.date: 12/13/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 140ad45d9c4f6b6f49a4ea4aefb9298e58a2cf10
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75443564"
---
# <a name="frequently-asked-questions-identity-protection-in-azure-active-directory"></a>Azure Active Dizininde Sık Sorulan Sorular Kimlik Koruması

## <a name="dismiss-user-risk-known-issues"></a>Kullanıcı riski bilinen sorunları yoksay

Klasik Kimlik Koruması'nda **kullanıcı riskini göz ardı etme,** aktörüN Kimlik Koruması'ndaki risk geçmişini Azure **AD'ye**ayarlar.

Kimlik Koruması'ndaki **kullanıcı riskini göz ardı** etmek, aktörü Kimlik Koruması'ndaki risk geçmişinde, ** \<kullanıcının\>bıçağını gösteren bir köprüyle Yönetici'nin adına**ayarlar.

Kullanıcı riski işten çıkarma akışında gecikmeye neden olan güncel bilinen bir sorun vardır. "Kullanıcı risk ilkesi" varsa, bu politika "Kullanıcı riskini düşür" seçeneğini tıkladıktan birkaç dakika sonra işten çıkarılan kullanıcılara uygulanmayı durdurur. Ancak, UX'nin görevden alınan kullanıcıların "Risk durumunu" yenilemesiyle ilgili bilinen gecikmeler vardır. Geçici çözüm olarak, en son kullanıcı "Risk durumu"nu görmek için sayfayı tarayıcı düzeyinde yenileyin.

## <a name="risky-users-report-known-issues"></a>Riskli kullanıcılar bilinen sorunları bildiriyor

**Kullanıcı adı** alanındaki sorgular büyük/küçük harf duyarlıdır, **Ad** alanındaki sorgular ise büyük/küçük harf duyarlıdır.

Toggling **TAzMİnAT SON GÜNCELLENDİ** sütunundaki tarihleri gizleyen **tarihleri göster.** Sütunu okumak için **Columns** Riskli Kullanıcılar bıçağının üst kısmındaki Sütunlar'ı tıklatın.

Klasik Kimlik Koruması'ndaki **tüm olayları reddetme,** risk algılama durumunu **Kapalı (çözümlenmiş)** olarak ayarlar.

## <a name="risky-sign-ins-report-known-issues"></a>Riskli oturum açma lar bilinen sorunları rapor

Risk algılama da **çözüm,** **risk tabanlı ilke tarafından yönlendirilen MFA'dan geçen Kullanıcılara**durumu ayarlar.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="why-is-a-user-is-at-risk"></a>Bir kullanıcı neden risk altında?

Azure AD Kimlik Koruması müşterisiyseniz, [riskli kullanıcıların](howto-identity-protection-investigate-risk.md#risky-users) görünümünü gidin ve risk altındaki bir kullanıcıyı tıklatın. Alttaki çekmecede , 'Risk geçmişi' sekmesi, kullanıcı risk değişikliğine yol açan tüm olayları gösterir. Kullanıcı için tüm riskli oturum açmaları görmek için 'Kullanıcının riskli oturum açma'larını tıklayın. Bu kullanıcı için tüm risk tespitlerini görmek için 'Kullanıcının risk algılamalarını' tıklayın.

### <a name="how-can-i-get-a-report-of-detections-of-a-specific-type"></a>Belirli bir türdeki algılamaların raporunu nasıl alabilirim?

Risk algılamalarına gidin', 'Algılama türüne' göre görüntüleyin ve filtreleyin. Daha sonra bu raporu 'de indirebilirsiniz. CSV veya . Üstteki **İndir** düğmesini kullanarak JSON formatı. Daha fazla bilgi için [How To: Investigate risk](howto-identity-protection-investigate-risk.md#risk-detections)bkz.

### <a name="why-cant-i-set-my-own-risk-levels-for-each-risk-detection"></a>Neden her risk tespiti için kendi risk seviyemi belirleyemiyorum?

Kimlik Koruması'ndaki risk düzeyleri, algılamanın hassasiyetine dayanır ve denetlenen makine öğrenimimiz tarafından desteklenmektedir. Kullanıcıların hangi deneyimin sunulduğunu özelleştirmek için yönetici, belirli kullanıcıları/grupları Kullanıcı Riski ve Oturum Açma Risk İlkeleri'nden ekleyebilir/hariç tutabilir.

### <a name="why-does-the-location-of-a-sign-in-not-match-where-the-user-truly-signed-in-from"></a>Oturum açma nın konumu neden kullanıcının gerçekten oturum açtığı yerle eşleşmiyor?

IP coğrafi konum haritalama endüstri çapında bir sorundur. Oturum açma raporunda listelenen konumun gerçek konumla eşleşmediğini düşünüyorsanız, Microsoft desteğine ulaşın. 

### <a name="how-can-i-close-specific-risk-detections-like-i-did-in-the-old-ui"></a>Eski u-u-u bilgisinde yaptığım gibi belirli risk algılamalarını nasıl kapatabilirim?

Bağlantılı oturum açmanın tehlikeye veya güvenli olduğunu onaylayarak risk algılamaları hakkında geri bildirimde bulunabilirsiniz. Oturum açma da verilen geri bildirimler, oturum açma da yapılan tüm algılamalara bağlıdır. Oturum açmayla bağlantılı olmayan algılamaları kapatmak istiyorsanız, bu geri bildirimi kullanıcı düzeyinde sağlayabilirsiniz. Daha fazla bilgi için [bkz: Azure AD Kimlik Koruması'nda risk geri bildirimi nde nasıl](howto-identity-protection-risk-feedback.md)olun.

### <a name="how-far-can-i-go-back-in-time-to-understand-whats-going-on-with-my-user"></a>Kullanıcımda neler olduğunu anlamak için zamanda ne kadar geriye gidebilirim?

- [Riskli kullanıcı](howto-identity-protection-investigate-risk.md#risky-users) görünümü, kullanıcının geçmişteki tüm oturum açma lara bağlı olarak ayakta durma riskini gösterir. 
- [Riskli oturum açma](howto-identity-protection-investigate-risk.md#risky-sign-ins) görünümü son 30 gün içinde risk altında olduğunu gösteriyor. 
- [Risk algılama](howto-identity-protection-investigate-risk.md#risk-detections) görünümü, son 90 gün içinde yapılan risk tespitlerini gösterir.

### <a name="how-can-i-learn-more-about-a-specific-detection"></a>Belirli bir algılama hakkında nasıl daha fazla bilgi edinebilirim?

Tüm risk tespitleri makalede [risk nedir](concept-identity-protection-risks.md#risk-types-and-detection)belgelenmiştir. Bir algılama hakkında daha fazla bilgi edinmek için Azure portalındaki algılamanın yanındaki (i) sembolünün üzerinde gezinebilirsiniz.

### <a name="how-do-the-feedback-mechanisms-in-identity-protection-work"></a>Kimlik Koruması'ndaki geri bildirim mekanizmaları nasıl çalışır?

**Gizliliği ihlal ilerler** (oturum açmada) - Azure AD Kimlik Koruması'na oturum açmanın kimlik sahibi tarafından gerçekleştirilmediğini bildirir ve bir uzlaşma gösterir.

- Bu geri bildirimi aldıktan sonra, oturum açma ve kullanıcı risk durumunu **Teyit edilen tehlikeye ve** risk düzeyine yüksek seviyeye taşıyoruz. **High**

- Buna ek olarak, risk değerlendirmesinde gelecekteki iyileştirmeler için makine öğrenimi sistemlerimize bilgi sağlıyoruz.

    > [!NOTE]
    > Kullanıcı zaten iyileşmişse, oturum açma ve kullanıcı risk durumunu **Onaylandı'ya** ve risk düzeyine yüksek **High**seviyeye taşıdır diye güvenliği **onayla'yı** tıklatmayın.

**Güvenli onaylayın** (oturum açma da) – Azure AD Kimlik Koruması'na oturum açmanın kimlik sahibi tarafından gerçekleştirildiğini ve bir uzlaşma yada işaret etmediğini bildirir.

- Bu geri bildirimi aldıktan sonra, oturum açma (kullanıcı değil) risk durumunu **Onaylanan** **-** güvenli ve risk düzeyini 'ye' ye taşıyoruz.

- Buna ek olarak, risk değerlendirmesinde gelecekteki iyileştirmeler için makine öğrenimi sistemlerimize bilgi sağlıyoruz.

    > [!NOTE]
    > Kullanıcının gizliliğinin ihlal edilemeyeceğini düşünüyorsanız, oturum açma düzeyinde **Onaylı güvenli** kullanmak yerine kullanıcı düzeyinde **Kullanıcı Riskini Kapat'ı** kullanın. Kullanıcı **düzeyindeki Kullanıcı Riskini Kapatma,** kullanıcı riskini ve geçmişteki tüm riskli oturum açma ve risk algılamalarını kapatır.

### <a name="why-am-i-seeing-a-user-with-a-low-or-above-risk-score-even-if-no-risky-sign-ins-or-risk-detections-are-shown-in-identity-protection"></a>Kimlik Koruması'nda riskli oturum açma veya risk tespiti gösterilmese bile, neden düşük (veya daha yüksek) risk puanına sahip bir kullanıcı görüyorum?

Kullanıcı riskinin birikmeli olduğu ve süresinin dolmadığı göz önüne alındığında, kullanıcının kimlik korumasında gösterilen son riskli oturum açma veya risk algılamaları olmasa bile düşük veya daha yüksek bir kullanıcı riski olabilir. Bu durum, bir kullanıcı üzerindeki tek kötü amaçlı faaliyet, riskli oturum açma ve risk algılama larının ayrıntılarını sakladığımız zaman diliminin ötesinde gerçekleşirse meydana gelebilir. Kötü aktörlerin saldırılarını hızlandırmadan önce 140 gün boyunca müşterilerin ini geride bıraktıkları bilindiği için kullanıcı riskini sona erdirmeyiz. Müşteriler, aşağıdakilere giderek bir kullanıcının neden risk altında olduğunu anlamak için kullanıcının risk zaman çizelgesini gözden geçirebilirsiniz:`Azure Portal > Azure Active Directory > Risky users’ report > Click on an at-risk user > Details’ drawer > Risk history tab`

### <a name="why-does-a-sign-in-have-a-sign-in-risk-aggregate-score-of-high-when-the-detections-associated-with-it-are-of-low-or-medium-risk"></a>İlişkili algılamalar düşük veya orta riskli olduğunda, oturum açma nın "oturum açma riski (toplam)" yüksek puanı neden var?

Yüksek toplam risk puanı oturum açmanın diğer özelliklerine veya oturum açma için birden fazla tespitin ateşlenmiş olmasına dayanıyor olabilir. Ve tam tersine, oturum açma ile ilişkili algılamalar Yüksek risk altında olsa bile, oturum açma riski (toplam) Orta olabilir. 
