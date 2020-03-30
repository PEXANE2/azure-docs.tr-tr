---
title: Azure Etkin Dizin Raporları SSS | Microsoft Dokümanlar
description: Azure Active Directory raporları yla ilgili sık sorulan sorular.
services: active-directory
documentationcenter: ''
author: cawrites
manager: MarkusVi
ms.assetid: 534da0b1-7858-4167-9986-7a62fbd10439
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 273fdb80475defb0576bcd29d1944c5f6c595cfc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266513"
---
# <a name="frequently-asked-questions-around-azure-active-directory-reports"></a>Azure Active Directory raporları yla ilgili sık sorulan sorular

Bu makalede, Azure Etkin Dizin (Azure AD) raporlaması hakkında sık sorulan soruların yanıtları yer almaktadır. Daha fazla bilgi için bkz. [Azure Active Directory raporlaması](overview-reports.md). 

## <a name="getting-started"></a>Başlarken 

**S: Şu anda, Azure AD denetimini `https://graph.windows.net/<tenant-name>/reports/` ve tümleşik uygulama kullanım raporlarını raporlama sistemlerimize programlı olarak çekmek için uç nokta API'lerini kullanıyorum. Neye geçmeliyim?**

**A:** [Etkinlik raporlarına erişmek için API'leri](concept-reporting-api.md)nasıl kullanabileceğinizi görmek için [API başvurusuna](https://developer.microsoft.com/graph/) bakın. Bu bitiş noktası, eski API bitiş noktasında ki tüm verileri sağlayan iki rapor **(Denetim** ve **Oturum Açma)** vardır. Bu yeni bitiş noktası, uygulama kullanımı, cihaz kullanımı ve kullanıcı oturum açma bilgilerini almak için kullanabileceğiniz Azure AD Premium lisansına sahip oturum açma raporuna da sahiptir.

---

**S: Şu `https://graph.windows.net/<tenant-name>/reports/` anda, Azure AD güvenlik raporlarını (sızan kimlik bilgileri veya anonim IP adreslerinden oturum açma gibi belirli türde algılamalar) raporlama sistemlerimize programlı olarak çekmek için son nokta API'lerini kullanıyorum. Neye geçmeliyim?**

**A:** Microsoft Graph aracılığıyla güvenlik algılamalarına erişmek için [Kimlik Koruması risk algılamaları API'sini](../identity-protection/graph-get-started.md) kullanabilirsiniz. Bu yeni biçim, gelişmiş filtreleme, alan seçimi ve daha fazlası ile verileri nasıl sorgulayabildiğinizkonusunda daha fazla esneklik sağlar ve risk algılamalarını SIEM'lere ve diğer veri toplama araçlarına daha kolay entegrasyon için tek bir türde standartlaştırır. Veriler farklı bir biçimde olduğundan, eski sorgularınız için yeni bir sorgu değiştiremezsiniz. Ancak, [yeni API,](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent)O365 veya Azure AD gibi API'ler için Microsoft standardı olan Microsoft Graph'ı kullanır. Bu nedenle, gereken iş geçerli Microsoft Graph yatırımlarınızı genişletebilir veya bu yeni standart platforma geçişinizi başlatmanıza yardımcı olabilir.

---

**S: Nasıl premium lisans alabilirim?**

**A:** Azure Active Directory baskınızı yükseltmek için [Azure Active Directory Premium ile başlarken](../fundamentals/active-directory-get-started-premium.md) bakın.

---

**S: Premium lisans aldıktan sonra etkinlik verilerini ne kadar sürede görmem gerekir?**

**A:** Ücretsiz lisans olarak etkinlik verileriniz zaten varsa, bunu hemen görebilirsiniz. Herhangi bir veriniz yoksa, verilerin raporlarda gösterilmesi bir veya iki gün sürer.

---

**S: Azure AD premium lisansı aldıktan sonra geçen ayın verilerini görebilir miyim?**

**A:** Yakın zamanda Premium sürüme (deneme sürümü de dahil) geçtiyseniz, başlangıçta 7 güne kadar veriyi görebilirsiniz. Veriler biriktiğinde, son 30 güne ait verileri görebilirsiniz.

---

**S: Azure portalında etkinlik oturum açma larını görmek veya API üzerinden veri almak için genel bir yönetici mi olmak gerekiyor?**

**A:** Hayır, kiracı için **bir Güvenlik Okuyucu** suandırızgırıyız veya Güvenlik **Yüzünden** yseniz, raporlama verilerine portal veya API yoluyla da erişebilirsiniz. Tabii ki, **Global Yöneticiler** de bu verilere erişebilir.

---


## <a name="activity-logs"></a>Etkinlik günlükleri


**S: Azure portalındaki etkinlik günlükleri (Denetim ve Oturum Açma) için veri saklama nedir?** 

**A:** Aşağıdaki tabloda etkinlik günlükleri için veri saklama süresi listelenir. Daha fazla bilgi için [Azure AD raporları için veri saklama ilkelerine](reference-reports-data-retention.md)bakın.

| Rapor                 | Azure AD Ücretsiz | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Denetim günlükleri             | 7 gün        | 30 gün             | 30 gün             |
| Oturum açma işlemleri               | Yok           | 30 gün             | 30 gün             |
| Azure MFA Kullanımı        | 30 gün       | 30 gün             | 30 gün             |

---

**S: Görevimi tamamladıktan sonra etkinlik verilerini göremem ne kadar sürer?**

**A:** Denetim günlüklerinde 15 dakika ile bir saat arasında değişen bir gecikme vardır. Oturum açma etkinlik günlükleri bazı kayıtlar için 15 dakikaile 2 saat arasında sürebilir.

---

**S: Office 365 etkinlik günlüğü bilgilerini Azure portalından alabilir miyim?**

**A:** Office 365 etkinliği ve Azure AD etkinlik günlükleri çok sayıda dizin kaynaklarını paylaşsa da, Office 365 etkinlik günlüklerinin tam görünümünü istiyorsanız, Office 365 Etkinlik günlüğü bilgilerini almak için [Microsoft 365 yönetici merkezine](https://admin.microsoft.com) gitmeniz gerekir.

---

**S: Office 365 Etkinlik günlükleri hakkında bilgi almak için hangi API'leri kullanırım?**

**A:** ApI aracılığıyla Office 365 Etkinlik günlüklerine erişmek için [Office 365 Yönetim API'lerini](https://docs.microsoft.com/office/office-365-management-api/office-365-management-apis-overview) kullanın.

---

**S: Azure portalından kaç kayıt indirebilirim?**

**A:** Azure portalından en fazla 5000 kayıt indirebilirsiniz. Kayıtlar *en son* sıralanır ve varsayılan olarak en son 5000 kaydı alırsınız.

---

## <a name="risky-sign-ins"></a>Riskli oturum açma işlemleri

**S: Kimlik Koruması'nda bir risk tespiti vardır, ancak oturum açma raporunda buna karşılık gelen oturum açma ları göremiyorum. Bu beklenen bir şey mi?**

**A:** Evet, Kimlik Koruması, etkileşimli veya etkileşimli olmayan tüm kimlik doğrulama akışları için riski değerlendirir. Ancak, tüm oturum açma lar yalnızca etkileşimli oturum açma ları gösterir.

---

**S: Azure portalında oturum açma nın veya kullanıcının neden riskli olarak işaretlendiğini nasıl anlarım?**

**A:** Azure AD **Premium** aboneliğiniz varsa, **risk için işaretlenen Kullanıcılar'da** kullanıcıyı seçerek veya **Riskli oturum açma** raporunda bir kayıt seçerek temel risk algılamaları hakkında daha fazla bilgi edinebilirsiniz. **Ücretsiz** veya **Temel** aboneliğiniz varsa, risk altındaki ve riskli oturum açma raporlarını görüntüleyebilirsiniz, ancak temel risk algılama bilgilerini göremezsiniz.

---

**S: OTURUM açma ve riskli oturum açma raporunda IP adresleri nasıl hesaplanır?**

**A:** IP adresleri, IP adresi ile bu adrese sahip bilgisayarın fiziksel olarak bulunduğu yer arasında kesin bir bağlantı olmayacak şekilde verilir. IP adreslerinin eşlemesi, mobil sağlayıcılar ve VPN'lerin genellikle istemci aygıtının gerçekten kullanıldığı yerden çok uzakta ki merkezi havuzlardan IP adresleri vermesi gibi etkenler tarafından daha da karmaşık hale getirilir. Şu anda Azure AD raporlarında, IP adresini fiziksel bir konuma dönüştürmek, izlemelere, kayıt defteri verilerine, ters görünümlere ve diğer bilgilere dayalı en iyi çabadır. 

---

**S: "Ek risk tespit edilen oturum açma" risk tespiti ne anlama gelir?**

**A:** Ortamınızdaki tüm riskli oturum açmalar hakkında bilgi vermek için, Azure AD Kimlik Koruması abonelerine özel algılamalar için oturum açma için yer tutucu olarak "Ek risk algılandırıla giriş" işlevleri.

---

## <a name="conditional-access"></a>Koşullu Erişim

**S: Bu özellik ile ilgili yenilikler nelerdir?**

**A:** Müşteriler artık tüm oturum açma raporu yla Koşullu Erişim ilkelerini sorun giderebilir. Müşteriler Koşullu Erişim durumunu gözden geçirebilir ve oturum açma da uygulanan ilkelerin ayrıntılarına ve her ilke için sonucu inceleyebilir.

**S: Nasıl başlarım?**

**A:** Başlamak için:

* [Azure portalındaki](https://portal.azure.com)oturum açma raporuna gidin.
* Sorun gidermek istediğiniz oturum açma'ya tıklayın.
* **Koşullu Erişim** sekmesine gidin. Burada, oturum açma yı etkileyen tüm ilkeleri ve her ilke için sonucu görüntüleyebilirsiniz. 
    
**S: Koşullu Erişim durumu için tüm olası değerler nelerdir?**

**A:** Koşullu Erişim durumu aşağıdaki değerlere sahip olabilir:

* **Uygulanmaz**: Bu, kullanıcı ve uygulama kapsamında ca ilkesi olmadığı anlamına gelir. 
* **Başarı**: Bu, kullanıcı ve uygulamakapsamında bir CA ilkesi olduğu ve CA ilkelerinin başarıyla karşılandığını anlamına gelir. 
* **Hata**: Bu, kullanıcı ve uygulamakapsamında bir CA ilkesi olduğu ve CA ilkelerinin tatmin olmadığı anlamına gelir. 
    
**S: Koşullu Erişim ilkesi sonucu için tüm olası değerler nelerdir?**

**A:** Koşullu Erişim ilkesi aşağıdaki sonuçlara sahip olabilir:

* **Başarı**: Politika başarıyla karşılandı.
* **Hata**: İlke memnun değildi.
* **Uygulanmadı**: Bunun nedeni ilke koşullarının karşılanmaması olabilir.
* **Etkin değil**: Bunun nedeni devre dışı bırakılmış durumdaki ilkedir. 
    
**S: Tüm oturum açma raporundaki ilke adı CA'daki ilke adıyla eşleşmiyor. Neden?**

**A:** Tüm oturum açma raporundaki ilke adı, oturum açma sırasındaki CA ilkesi adını temel adatır. Bu, ilke adını daha sonra, yani oturum açmadan sonra güncellediyseniz, CA'daki ilke adı ile tutarsız olabilir.

**S: Oturum açma melim Koşullu Erişim ilkesi nedeniyle engellendi, ancak oturum açma faaliyet raporu oturum açmanın başarılı olduğunu gösteriyor. Neden?**

**A:** Şu anda oturum açma raporu, Koşullu Erişim uygulandığında Exchange ActiveSync senaryoları için doğru sonuçlar göstermeyebilir. Raporda oturum açma sonucunun başarılı bir oturum açma gösterdiği durumlar olabilir, ancak oturum açma şartlı erişim ilkesi nedeniyle aslında başarısız olur. 
