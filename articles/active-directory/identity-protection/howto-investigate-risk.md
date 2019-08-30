---
title: Azure Active Directory kimlik koruması 'nda riskleri araştırma (yenileme) | Microsoft Docs
description: Azure Active Directory kimlik koruması 'nda riskli kullanıcıları, algılamaları ve oturum açma işlemlerini nasıl araştıracağınızı öğrenin (yenilenir).
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
ms.openlocfilehash: a6e9715b63ccfbf1da3bdaedf947ea4cd65109b7
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70129724"
---
# <a name="how-to-investigate-risk"></a>Nasıl Yapılır: Riski araştırın

Riskli oturum açma işlemleri, riskli kullanıcılar ve risk algılama raporlarının yanı sıra ortamınızda risk konusunda bilgi edinmek ve riskleri elde edebilirsiniz. Riskli oturum açma işlemlerini, kullanıcıları ve algılamaları filtreleyip sıralayabilme olanağı sayesinde, kuruluşunuzdaki olası yetkisiz erişimi daha iyi anlayabilirsiniz. 

## <a name="risky-users-report"></a>Riskli kullanıcılar raporu

Riskli kullanıcılar raporu tarafından verilen bilgiler ile, şu gibi soruların yanıtlarını bulabilirsiniz:

- Hangi kullanıcılar yüksek riskli?
- Hangi kullanıcıların risk durumu düzeltildi?

Bu rapora ilk giriş noktanız, güvenlik sayfasındaki **Araştır** bölümüdür.

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/01.png)

Riskli kullanıcılar raporu şunları gösteren bir varsayılan görünüme sahiptir:

- Name
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
- Son riskli oturum açma işlemleri
- Bir oturum açma ile bağlantılı olmayan risk algılamaları
- Risk geçmişi

Ayrıca şunları yapabilirsiniz:

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/08.png)

- Bu kullanıcının oturum açma raporunu görüntülemek için tüm oturum açma kısayollarını görüntüleyin.
- Riskli olarak işaretlenen bu kullanıcıya ait tüm oturum açma işlemlerini görüntülemek için tüm riskli oturum açma işlemlerini görüntüleyin.
- Kullanıcının kimliğinin tehlikede olduğunu düşünüyorsanız, kullanıcının parolasını sıfırlayın.
- Bir kullanıcının etkin risk algılamaları 'nın hatalı pozitif sonuçlar olduğunu düşünüyorsanız Kullanıcı riskini kapatın. Daha fazla bilgi için [Azure AD kimlik koruması risk algılamaları hakkında geri bildirim sağlama](howto-provide-risk-event-feedback.md)makalesine bakın.

### <a name="filter-risky-users"></a>Riskli kullanıcıları filtrele

Bildirilen verileri sizin için uygun bir düzeye daraltmak için, riskli Kullanıcı verilerini aşağıdaki varsayılan alanları kullanarak filtreleyebilirsiniz:

- Name
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

- Son hafta içinde anonim IP adresi riski bulunan çok sayıda başarılı oturum açma işlemi var mı?
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

Daha fazla bilgi için [Azure AD kimlik koruması risk algılamaları hakkında geri bildirim sağlama](howto-provide-risk-event-feedback.md)makalesine bakın.

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


## <a name="risk-detections-report"></a>Risk algılama raporu

Risk algılamaları raporu, kiracınızdaki her kimlik koruması riskini tespit etmenizi sağlar.


Risk algılama raporunda şunları gösteren bir varsayılan görünüm bulunur:

- Date

- Kullanıcı

- IP adresi

- Location

- Algılama türü

- Risk durumu

- Risk düzeyi

- İstek Kimliği
 

Araç çubuğunda **Sütunlar**’a tıklayarak liste görünümünü özelleştirebilirsiniz. Sütunlar iletişim kutusu, ek alanları görüntülemenizi veya zaten görüntülenen alanları kaldırmanızı sağlar.

Liste görünümündeki bir öğeye tıklayarak bu öğe hakkında mevcut olan tüm ayrıntıları yatay bir görünümde alabilirsiniz.


Ayrıntılar görünümünde risk algılama hakkında ek bilgiler gösterilir.

Ayrıntılar görünümü için bağlantıları da içerir

- Kullanıcının risk raporunu görüntüle
- Kullanıcının oturum açma işlemlerini görüntüleme
- Kullanıcının riskli oturum açma işlemlerini görüntüleme
- Bağlı riskli oturum açmayı görüntüle
- Kullanıcının risk algılamalarını görüntüleme


### <a name="filter-risk-detections"></a>Risk algılamalarını filtrele

Bildirilen verileri sizin için uygun bir düzeye daraltmak için, aşağıdaki varsayılan alanları kullanarak risk algılama verilerine filtre uygulayabilirsiniz:

- Algılama zamanı
- Algılama türü
- Risk durumu
- Risk düzeyi
- Kullanıcı
- Kullanıcı Adı
- IP adresi
- Location
- Algılama zamanlaması
- Etkinlik 
- Source
- Belirteç Verenin türü
- İstek Kimliği
- Bağıntı Kimliği


**Algılama süresi** filtresi, döndürülen veriler için bir zaman çerçevesi tanımlamanıza olanak sağlar. Bu filtre şunları seçmenizi sağlar:
- Son 90 gün
- Son 30 gün
- Son 7 gün
- Son 24 saat
- Özel zaman aralığı

**Algılama türü** filtresi, risk algılama türünü belirtmenizi sağlar (örneğin, bilmediğiniz oturum açma özellikleri gibi).

**Risk durumu** filtresi şunları seçmenizi sağlar:

- Risk altında
- Güvenliğin tehlikeye girdiği onaylandı
- Güvenli işlem onaylandı
- Kapatıldı
- Çözümlendi

**Risk düzeyi** filtresi şunları seçmenizi sağlar:

- Yüksek
- Orta
- Düşük

**Kullanıcı** filtresi, önem verdiğiniz kullanıcının adını veya nesne kimliğini belirtmenize olanak sağlar.

Kullanıcı **adı** filtresi, ilgilendiğiniz kullanıcının Kullanıcı asıl adını (UPN) belirtmenize olanak sağlar.

**Algılama zamanlaması** filtresi, algılamanın gerçek zamanlı veya çevrimdışı olduğunu belirtmenizi sağlar. Not: Oturum açma riski ilkesini kullanarak, gerçek zamanlı algılamalar ile oturum açma işlemlerini deneyebilirsiniz. 

**Etkinlik** filtresi, algılamanın bir oturum açma (örneğin, anonim IP adresi) veya bir Kullanıcı (örneğin, sızdırılan kimlik bilgileri) ile bağlantılı olup olmadığını belirtmenize olanak sağlar.

**Kaynak** filtresi, risk algılamanın kaynağını belirtmenize olanak sağlar (örneğin, Azure AD veya Microsoft Cloud App Security). 

**Belirteç verenin tür** filtresi, belirtecin nerede verildiğini belirtmenize olanak tanır (örneğin, Azure AD veya ad Federasyon Hizmetleri)


### <a name="download-risk-detections-data"></a>Risk algılama verilerini indirin

Azure portal dışında çalışmak istiyorsanız risk algılama verilerini indirebilirsiniz. Indir 'e tıkladığınızda en son 5.000 kaydın CSV dosyası oluşturulur. 

## <a name="next-steps"></a>Sonraki adımlar

Azure AD Kimlik Koruması genel bakış almak için bkz. [Azure AD kimlik koruması genel bakış](overview-v2.md).
