---
title: Kimlik doğrulama yöntemleri kullanımı & öngörüleri - Azure Active Directory
description: Azure AD self servis parola sıfırlama ve Çok Faktörlü Kimlik Doğrulama yöntemi kullanımı hakkında raporlama
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1730939de399cacd13c62988259904ba84ee78ff
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80654141"
---
# <a name="authentication-methods-usage--insights-preview"></a>Kimlik doğrulama yöntemleri kullanımı & öngörüleri (önizleme)

Kullanım & öngörüler, Azure Çok Faktörlü Kimlik Doğrulama ve self servis parola sıfırlama gibi özelliklerin kimlik doğrulama yöntemlerinin kuruluşunuzda nasıl çalıştığını anlamanızı sağlar. Bu raporlama özelliği, kuruluşunuz için hangi yöntemlerin kaydedildiğini ve bunların nasıl kullanıldığını anlama olanağı sağlar.

## <a name="permissions-and-licenses"></a>İzinler ve lisanslar

Aşağıdaki roller kullanıma ve öngörülere erişebilir:

- Genel Yönetici
- Güvenlik Okuyucu
- Güvenlik Yöneticisi
- Raporlar Okuyucu

Kullanıma ve öngörülere erişmek için ek lisans gerekmez. Azure Çok Faktörlü Kimlik Doğrulama ve self servis parola sıfırlama (SSPR) lisanslama bilgileri [Azure Active Directory fiyatlandırma sitesinde](https://azure.microsoft.com/pricing/details/active-directory/)bulunabilir.

## <a name="how-it-works"></a>Nasıl çalışır?

Kimlik doğrulama yöntemi kullanımına ve öngörülerine erişmek için:

1. [Azure portalına](https://portal.azure.com)göz atın.
1. **Azure Active Directory** > **Password sıfırlama** > **Kullanımı & öngörülerine**göz atın.
1. **Kayıt** veya **Kullanıma** genel bakışlardan, gereksinimlerinize göre filtrelemek için önceden filtrelenmiş raporları açmayı seçebilirsiniz.

![Kullanım & öngörülere genel bakış](./media/howto-authentication-methods-usage-insights/usage-insights-overview.png)

Kullanıma & öngörülere doğrudan erişmek [https://portal.azure.com/#blade/Microsoft_AAD_IAM/AuthMethodsOverviewBlade](https://portal.azure.com/#blade/Microsoft_AAD_IAM/AuthMethodsOverviewBlade)için. Bu bağlantı, kayıt genel bakış size getirecektir.

Kullanıcılar kayıtlı, Kullanıcılar etkin ve Kullanıcılar yeteneğine sahip kutucuklar kullanıcılarınız için aşağıdaki kayıt verilerini gösterir:

- Kayıtlı: Bir kullanıcı( veya bir yönetici) kuruluşunuzun SSPR veya Çok Faktörlü Kimlik Doğrulama ilkesini karşılamak için yeterli kimlik doğrulama yöntemleri kaydetmişse kayıtlı kabul edilir.
- Etkin: SSPR ilkesi kapsamında olan bir kullanıcı etkin kabul edilir. Bir grup için SSPR etkinse, kullanıcı bu gruptaysa etkin kabul edilir. Tüm kullanıcılar için SSPR etkinse, kiracıdaki (misafirler hariç) tüm kullanıcılar etkin kabul edilir.
- Yetenekli: Bir kullanıcı, hem kayıtlı hem de etkinse yetenekli kabul edilir. Bu durum, gerektiğinde istedikleri zaman SSPR gerçekleştirebilecekleri anlamına gelir.

Bu döşemelerden herhangi birini tıklattığınızda veya bunlarda gösterilen öngörüler sizi önceden filtrelenmiş bir kayıt ayrıntıları listesine getirir.

**Kayıt** sekmesindeki **Kayıtlar** grafiği, kimlik doğrulama yöntemiyle başarılı ve başarısız kimlik doğrulama yöntemi kayıtlarının sayısını gösterir. **Kullanım** sekmesindeki **Sıfırlama** grafiği, kimlik doğrulama yöntemiyle parola sıfırlama akışı sırasında başarılı ve başarısız kimlik doğrulamalarının sayısını gösterir.

Grafiklerden herhangi birini tıklattığınızda, önceden filtrelenmiş bir kayıt veya sıfırlama olayları listesine geçebilirsiniz.

Üst, sağ köşedeki denetimi kullanarak, Kayıtlar ve Sıfırlayıcı grafiklerde gösterilen denetim verilerinin tarih aralığını 24 saat, 7 gün veya 30 gün olarak değiştirebilirsiniz.

### <a name="registration-details"></a>Kayıt bilgileri

**Kayıtlı Kullanıcılar'a**tıkladığınızda, **Kullanıcılar etkinleştirilmiş**veya **Kullanıcılar yetenekli** kutucuklar veya öngörüler kayıt ayrıntılarına ulaştıracaktır.

Kayıt ayrıntıları raporu her kullanıcı için aşağıdaki bilgileri gösterir:

- Adı
- Kullanıcı adı
- Kayıt durumu (Tümü, Kayıtlı, Kayıtlı Değil)
- Etkin leştirilmiş durum (Tümü, Etkin, Etkin Değil)
- Yetenekli durum (Tüm, Yetenekli, Yetenekli Değil)
- Yöntemler (Uygulama bildirimi, Uygulama kodu, Telefon görüşmesi, SMS, E-posta, Güvenlik soruları)

Listenin üst kısmındaki denetimleri kullanarak, bir kullanıcı arayabilir ve gösterilen sütunlara göre kullanıcı listesini filtreleyebilirsiniz.

### <a name="reset-details"></a>Ayrıntıları sıfırlama

Kayıtlar veya Sıfırlama çizelgelerine tıkladığınızda sıfırlama ayrıntıları sizi getirecektir.

Sıfırlama ayrıntıları raporu, aşağıdakiler de dahil olmak üzere son 30 güne ait kayıt ve sıfırlama olaylarını gösterir:

- Adı
- Kullanıcı adı
- Özellik (Tümü, Kayıt, Sıfırlama)
- Kimlik doğrulama yöntemi (Uygulama bildirimi, Uygulama kodu, Telefon görüşmesi, Ofis araması, SMS, E-posta, Güvenlik soruları)
- Durum (Tümü, Başarı, Başarısızlık)

Listenin üst kısmındaki denetimleri kullanarak, bir kullanıcı arayabilir ve gösterilen sütunlara göre kullanıcı listesini filtreleyebilirsiniz.

## <a name="limitations"></a>Sınırlamalar

Bu raporlarda gösterilen veriler 60 dakikaya kadar geciktirilir. Verilerinizin ne kadar yeni olduğunu belirlemek için Azure portalında "Son yenilenen" bir alan bulunur.

Kullanım ve öngörü verileri, Azure Çok Faktörlü Kimlik Doğrulama etkinlik raporlarının veya Azure AD oturum açma raporunda yer alan bilgilerin yerini alamaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Kimlik doğrulama yöntemleri yle çalışma kullanım raporu API](https://docs.microsoft.com/graph/api/resources/authenticationmethods-usage-insights-overview?view=graph-rest-beta)
- [Kuruluşunuz için kimlik doğrulama yöntemleri seçme](concept-authentication-methods.md)
- [Kombine kayıt deneyimi](concept-registration-mfa-sspr-combined.md)
