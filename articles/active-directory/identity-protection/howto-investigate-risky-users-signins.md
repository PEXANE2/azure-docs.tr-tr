---
title: Azure Active Directory kimlik koruması 'nda riskli kullanıcıları ve oturum açma işlemlerini araştırma (yenileme) | Microsoft Docs
description: Azure Active Directory kimlik koruması 'nda riskli kullanıcıları ve oturum açma işlemlerini nasıl araştırılacağını öğrenin (yenileme).
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: c89658e962654f005eaee5ceff220d5fb343e86e
ms.sourcegitcommit: c71306fb197b433f7b7d23662d013eaae269dc9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68370313"
---
# <a name="how-to-investigate-risky-users-and-sign-ins"></a>Nasıl Yapılır: Riskli kullanıcıları ve oturum açma işlemlerini araştırma 

Riskli oturum açma işlemleri ve riskli kullanıcılar raporları kullanarak ortamınızda risk konusunda bilgi edinebilir ve bu riski inceleyebilirsiniz. Riskli oturum açma işlemlerini ve kullanıcıları filtreleme ve sıralayabilme olanağı sayesinde, kuruluşunuzdaki olası yetkisiz erişimi daha iyi anlayabilirsiniz. 

## <a name="risky-users-report"></a>Riskli kullanıcılar raporu

Riskli kullanıcılar raporu tarafından verilen bilgiler ile, şu gibi soruların yanıtlarını bulabilirsiniz:

- Hangi kullanıcılar yüksek riskli?
- Hangi kullanıcıların risk durumu düzeltildi?

Bu rapora ilk giriş noktanız, güvenlik sayfasındaki **Araştır** bölümüdür.

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/01.png)

Riskli kullanıcılar raporu şunları gösteren bir varsayılan görünüme sahiptir:

- Ad
- Risk durumu
- Risk düzeyi
- Risk ayrıntısı
- Riskin son güncelleştirilmesi
- Type
- Durum

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/03.png)

Araç çubuğunda **Sütunlar**’a tıklayarak liste görünümünü özelleştirebilirsiniz.

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/04.png)

Sütunlar iletişim kutusu, ek alanları görüntülemenizi veya zaten görüntülenen alanları kaldırmanızı sağlar.

Liste görünümündeki bir öğeye tıklayarak bu öğe hakkında mevcut olan tüm ayrıntıları yatay bir görünümde alabilirsiniz.

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/05.png)

Ayrıntılar görünümü şunları gösterir:

- Temel bilgiler
- Son riskli oturum açmalar
- Bir oturum açma adına bağlı olmayan riskli olaylar
- Risk geçmişi

Ayrıca şunları yapabilirsiniz:

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/08.png)

- Bu kullanıcının oturum açma raporunu görüntülemek için tüm oturum açma kısayollarını görüntüleyin.
- Riskli olarak işaretlenen bu kullanıcıya ait tüm oturum açma işlemlerini görüntülemek için tüm riskli oturum açma işlemlerini görüntüleyin.
- Kullanıcının kimliğinin tehlikede olduğunu düşünüyorsanız, kullanıcının parolasını sıfırlayın.
- Bir kullanıcının etkin risk olaylarının hatalı pozitif sonuçlar olduğunu düşünüyorsanız Kullanıcı riskini kapatın. Daha fazla bilgi için [Azure AD kimlik koruması risk olayları hakkında geri bildirim sağlama](howto-provide-risk-event-feedback.md)makalesine bakın.

### <a name="filter-risky-users"></a>Riskli kullanıcıları filtrele

Bildirilen verileri sizin için uygun bir düzeye daraltmak için, riskli Kullanıcı verilerini aşağıdaki varsayılan alanları kullanarak filtreleyebilirsiniz:

- Ad
- Kullanıcı Adı
- Risk durumu
- Risk düzeyi
- Type
- Durum

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/06.png)

**Ad** filtresi, önem verdiğiniz kullanıcının adını veya Kullanıcı asıl adını (UPN) belirtmenize olanak sağlar.

**Risk durumu** filtresi şunları seçmenizi sağlar:

- Risk altında
- Çözümlendi
- Kapatıldı

**Risk düzeyi** filtresi şunları seçmenizi sağlar:

- Yüksek
- Orta
- Düşük

**Tür** filtresi şunları seçmenizi sağlar:

- Guest
- Üyesi

**Durum** filtresi şunları seçmenizi sağlar:

- Silinmiş
- Etkin

### <a name="download-risky-users-data"></a>Riskli Kullanıcı verilerini indirin

Azure portal dışında çalışmak istiyorsanız riskli kullanıcılar verilerini indirebilirsiniz. Indir 'e tıkladığınızda en son 2.500 kaydın CSV dosyası oluşturulur. 

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/07.png)

Araç çubuğundaki Sütunlar ' a tıklayarak liste görünümünü özelleştirebilirsiniz.
 
Bu sayede ek alanları görüntüleyebilir ya da zaten görüntülenen alanları kaldırabilirsiniz.
 
Riskli bir kullanıcı hakkında daha fazla bilgi edinmek için Ayrıntılar çekmecesine tıklayarak genişletin

## <a name="risky-sign-ins-report"></a>Riskli oturum açma işlemleri raporu

Riskli oturum açma işlemleri raporu tarafından verilen bilgiler ile, şu gibi soruların yanıtlarını bulabilirsiniz:

- Son haftada adsız IP adresi risk olayları olan kaç başarılı oturum açma işlemi var?
- Geçen ay hangi kullanıcıların güvenliği aşılmış?
- Hangi kullanıcılar Office 365 portalında oturum açma risklerine sahip?

Bu rapora ilk giriş noktanız, güvenlik sayfasındaki **Araştır** bölümüdür.

![Riskli oturum açma işlemleri raporu](./media/howto-investigate-risky-users-signins/02.png)

Riskli oturum açma işlemleri raporu şunları gösteren bir varsayılan görünüme sahiptir:

- Date
- Kullanıcı
- Uygulama
- Oturum açma durumu
- Risk durumu
- Risk düzeyi (toplama)
- Risk düzeyi (gerçek zamanlı)
- Koşullu Erişim
- MFA gerekli  

![Riskli oturum açma işlemleri raporu](./media/howto-investigate-risky-users-signins/09.png)

Araç çubuğunda **Sütunlar**’a tıklayarak liste görünümünü özelleştirebilirsiniz.

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/11.png)

Sütunlar iletişim kutusu, ek alanları görüntülemenizi veya zaten görüntülenen alanları kaldırmanızı sağlar.

Liste görünümündeki bir öğeye tıklayarak bu öğe hakkında mevcut olan tüm ayrıntıları yatay bir görünümde alabilirsiniz.

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/12.png)

Ayrıntılar görünümü şunları gösterir:

- Temel bilgiler
- Cihaz bilgileri
- Risk bilgileri
- MFA bilgileri
- Koşullu Erişim

Ayrıca şunları yapabilirsiniz:

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/13.png)

- Güvenliğin tehlikeye girdiğini onaylayın 
- Güvenli olduğunu onayla

Daha fazla bilgi için [Azure AD kimlik koruması risk olayları hakkında geri bildirim sağlama](howto-provide-risk-event-feedback.md)makalesine bakın.

### <a name="filter-risky-sign-ins"></a>Riskli oturum açma işlemlerini filtrele

Bildirilen verileri sizin için uygun bir düzeye daraltmak için, riskli Kullanıcı verilerini aşağıdaki varsayılan alanları kullanarak filtreleyebilirsiniz:

- Kullanıcı
- Uygulama
- Oturum açma durumu
- Risk durumu
- Risk düzeyi (toplama)
- Risk düzeyi (gerçek zamanlı)
- Koşullu Erişim
- Date
- Risk düzeyi türü

![Riskli oturum açma işlemleri raporu](./media/howto-investigate-risky-users-signins/14.png)

**Ad** filtresi, önem verdiğiniz kullanıcının adını veya Kullanıcı asıl adını (UPN) belirtmenize olanak sağlar.

**Uygulama** filtresi, kullanıcının erişmeye çalıştığı bulut uygulamasını belirtmenizi sağlar.

**Oturum açma durumu** filtresi aşağıdakilerden birini seçmenize imkan tanır:

- Tümü
- Başarılı
- Hata

**Risk durumu** filtresi şunları seçmenizi sağlar:

- Risk altında
- Güvenliğin tehlikeye girdiği onaylandı
- Güvenli işlem onaylandı
- Kapatıldı
- Çözümlendi

**Risk düzeyi (toplama)** filtresi şunları seçmenizi sağlar:

- Yüksek
- Orta
- Düşük

**Risk düzeyi (gerçek zamanlı)** filtresi şunları seçmenizi sağlar:

- Yüksek
- Orta
- Düşük

**Koşullu erişim** filtresi şunları seçmenizi sağlar:

- Tümü
- Uygulanmadı
- Başarılı
- Hata

**Tarih** filtresi, döndürülen veriler için bir zaman çerçevesi tanımlamanıza olanak sağlar.
Olası değerler şunlardır:

- Son 1 ay
- Son 7 gün
- Son 24 saat
- Özel zaman aralığı

### <a name="download-risky-sign-ins-data"></a>Riskli oturum açma işlemleri verilerini indirin

Azure portal dışında çalışmak istiyorsanız riskli oturum açma verilerini indirebilirsiniz. Indir 'e tıkladığınızda en son 2.500 kaydın CSV dosyası oluşturulur. 

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/15.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure AD Kimlik Koruması genel bakış almak için bkz. [Azure AD kimlik koruması genel bakış](overview-v2.md).
