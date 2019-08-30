---
title: Azure Active Directory raporları SSS | Microsoft Docs
description: Azure Active Directory raporlarının etrafında sık sorulan sorular.
services: active-directory
documentationcenter: ''
author: cawrites
manager: daveba
ms.assetid: 534da0b1-7858-4167-9986-7a62fbd10439
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: e8c3138b82c7dc4a7217e8cb67448a5d824398ba
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70127017"
---
# <a name="frequently-asked-questions-around-azure-active-directory-reports"></a>Azure Active Directory raporlarının etrafında sık sorulan sorular

Bu makale Azure Active Directory (Azure AD) raporlama hakkında sık sorulan soruların yanıtlarını içerir. Daha fazla bilgi için bkz. [Azure Active Directory raporlaması](overview-reports.md). 

## <a name="getting-started"></a>Başlarken 

**S: Şu anda `https://graph.windows.net/<tenant-name>/reports/` Azure AD Audit ve tümleşik uygulama kullanım raporlarını raporlama sistemlerimize programlı bir şekilde çekmek için Endpoint API 'leri kullanıyorum. Ne geçiş yapmam gerekir?**

**C:** [Etkinlik raporlarına erişmek için API 'leri nasıl kullanabileceğinizi](concept-reporting-api.md)görmek için [API başvurusunu](https://developer.microsoft.com/graph/) bulun. Bu uç noktanın, eski API uç noktasında aldığınız tüm verileri sağlayan iki raporu (**Denetim** ve **oturum açma**) vardır. Bu yeni uç noktanın Ayrıca uygulama kullanımını, cihaz kullanımını ve Kullanıcı oturum açma bilgilerini almak için kullanabileceğiniz Azure AD Premium lisansıyla bir oturum açma raporu bulunur.

---

**S: Şu anda `https://graph.windows.net/<tenant-name>/reports/` Azure AD güvenlik raporlarını (IP adreslerinden gelen kimlik bilgileri veya anonim IP adreslerinden oturum açma işlemleri gibi), raporlama sistemlerimize programlama yoluyla çekmek için uç nokta API 'lerini kullanıyorum. Ne geçiş yapmam gerekir?**

**C:** Güvenlik algılamalarını Microsoft Graph aracılığıyla erişmek için [kimlik koruması risk algılama API](../identity-protection/graph-get-started.md) 'sini kullanabilirsiniz. Bu yeni biçim, Gelişmiş filtreleme, alan seçimi ve daha fazlası ile verileri nasıl sorgulayabilme ve risk algılamalarını bir tür halinde kullanarak Sıems ve diğer veri toplama araçlarına daha kolay tümleştirme için daha fazla esneklik sağlar. Veriler farklı bir biçimde olduğundan eski sorgularınız için yeni bir sorgu yerine kullanamazsınız. Ancak, [yenı API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent), O365 veya Azure AD gibi bu API 'Ler için Microsoft standard olan Microsoft Graph kullanır. Bu nedenle, gereken iş, geçerli MS Graf yatırımlarınızı genişletebilir veya bu yeni standart platforma geçişinizi başlamanıza yardımcı olabilir.

---

**S: Premium lisans almak Nasıl yaparım??**

**C:** Azure Active Directory sürümünüzü yükseltmek için bkz. [Azure Active Directory Premium kullanmaya](../fundamentals/active-directory-get-started-premium.md) başlama.

---

**S: Premium lisans aldıktan sonra etkinlik verilerini ne kadar yakında görmem gerekir?**

**C:** Zaten ücretsiz bir lisans olarak Etkinlikler verileriniz varsa, bunu hemen görebilirsiniz. Hiçbir veriniz yoksa, verilerin raporlarda görünmesi bir veya iki gün sürer.

---

**S: Bir Azure AD Premium lisansı aldıktan sonra geçen aya ait verileri görebilir miyim?**

**C:** Yakın zamanda bir premium sürüme (deneme sürümü dahil) geçiş yaptıysanız, başlangıçta en fazla 7 güne kadar verileri görebilirsiniz. Veriler birikme sırasında son 30 güne ait verileri görebilirsiniz.

---

**S: Azure portal etkinlik oturum açma işlemlerini görmek veya API aracılığıyla veri almak için genel yönetici olmam gerekir mi?**

**C:** Hayır, raporlama verilerine Portal üzerinden veya kiracı için **güvenlik okuyucunuz** veya **güvenlik yöneticisiyseniz** API aracılığıyla da erişebilirsiniz. Kuşkusuz, **genel yöneticilerin** bu verilere erişimi de olur.

---


## <a name="activity-logs"></a>Etkinlik günlükleri


**S: Azure portal etkinlik günlükleri (denetim ve oturum açma işlemleri) için veri saklama nedir?** 

**C:** Aşağıdaki tabloda, etkinlik günlükleri için veri saklama süresi listelenmektedir. Daha fazla bilgi için bkz. [Azure AD raporları için veri saklama ilkeleri](reference-reports-data-retention.md).

| Rapor                 | Azure AD Ücretsiz | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Denetim günlükleri             | 7 gün        | 30 gün             | 30 gün             |
| Oturum açma işlemleri               | Yok           | 30 gün             | 30 gün             |
| Azure MFA kullanımı        | 30 gün       | 30 gün             | 30 gün             |

---

**S: Görevimi tamamladıktan sonra etkinlik verilerini görediğimde ne kadar sürer?**

**C:** Denetim günlükleri 15 dakikadan bir saate kadar gecikme süresine sahiptir. Oturum açma etkinliği günlükleri, bazı kayıtlar için 15 dakikadan 2 saate kadar sürebilir.

---

**S: Azure portal aracılığıyla Office 365 etkinlik günlüğü bilgilerini alabilir miyim?**

**C:** Office 365 etkinliği ve Azure AD etkinlik günlükleri birçok dizin kaynağını paylaşsa da, Office 365 etkinlik günlüklerinin tam bir görünümünü istiyorsanız, Office 365 etkinlik günlüğü bilgilerini almak için [Microsoft 365 yönetim merkezine](https://admin.microsoft.com) gitmeniz gerekir.

---

**S: Office 365 etkinlik günlükleri hakkında bilgi almak için hangi API 'Leri kullanmalıyım?**

**C:** Office 365 etkinlik günlüklerine bir API aracılığıyla erişmek için [office 365 Yönetim API 'lerini](https://docs.microsoft.com/office/office-365-management-api/office-365-management-apis-overview) kullanın.

---

**S: Azure portal kaç kayıt indirebilirim?**

**C:** Azure portal en fazla 5000 kaydı indirebilirsiniz. Kayıtlar en *güncel* olarak sıralanır ve varsayılan olarak en son 5000 kayıtları alırsınız.

---

## <a name="risky-sign-ins"></a>Riskli oturum açma işlemleri

**S: Kimlik koruması 'nda bir risk algılama işlemi var, ancak oturum açma raporlarında ilgili oturum açma raporuna bakmıyor. Bu beklensin mi?**

**C:** Evet, kimlik koruması etkileşimli veya etkileşimli olmayan tüm kimlik doğrulama akışları için risk değerlendirir. Ancak, yalnızca oturum açma işlemleri raporu yalnızca etkileşimli oturum açma işlemlerini gösterir.

---

**S: Nasıl yaparım?, Azure portal bir oturum açma veya kullanıcının neden riskli olarak işaretlendiğinden emin misiniz?**

**C:** **Azure AD Premium** aboneliğiniz varsa, **risk açısından işaretlenen kullanıcılar** ' ı seçerek veya **riskli oturum açma** işlemleri raporundaki bir kaydı seçerek temeldeki risk algılamaları hakkında daha fazla bilgi edinebilirsiniz. **Ücretsiz** veya **temel** aboneliğiniz varsa, kullanıcılar risk ve riskli oturum açma raporları raporlarını görüntüleyebilir, ancak temel risk algılama bilgilerini göremezsiniz.

---

**S: Oturum açma ve riskli oturum açma işlemleri raporunda IP adresleri nasıl hesaplanır?**

**C:** IP adresleri, bir IP adresi arasında kesin bir bağlantı olmaması ve bu adrese sahip bilgisayarın fiziksel olarak bulunduğu bir şekilde verilir. IP adreslerini eşleme, mobil sağlayıcılar ve VPN 'lerin IP adreslerini istemci cihazının gerçekten kullanıldığı yerden çok büyük bir şekilde veren, mobil sağlayıcılar ve VPN 'Ler gibi faktörlerle daha karmaşıktır. Şu anda Azure AD raporlarında, IP adresi fiziksel bir konuma dönüştürülürken izlemeler, kayıt defteri verileri, ters aramalar ve diğer bilgiler temel alınarak en iyi çaba vardır. 

---

**S: Risk algılama "ek risk ile oturum açma algılandı" anlamı nedir?**

**C:** Ortamınızdaki tüm riskli oturum açma bilgileri hakkında bilgi vermek için, "ek risk algılandı" işlevleri, Azure AD Kimlik Koruması abonelere özel algılamalar için oturum açma işlemleri için yer tutucu olarak çalışır.

---

## <a name="conditional-access"></a>Koşullu Erişim

**S: Bu özellikle ilgili yenilikler nelerdir?**

**C:** Müşteriler artık tüm oturum açma raporları aracılığıyla koşullu erişim ilkelerini giderebilirler. Müşteriler, koşullu erişim durumunu gözden geçirebilir ve oturum açma için uygulanan ilkelerin ayrıntılarını ve her bir ilkenin sonucunu inceler.

**S: Nasıl yaparım? başlamak istiyor musunuz?**

**C:** Kullanmaya başlamak için:

* [Azure Portal](https://portal.azure.com)oturum açma raporuna gidin.
* Sorun gidermek istediğiniz oturum açma seçeneğine tıklayın.
* **Koşullu erişim** sekmesine gidin. Buradan, her ilke için oturum açma ve sonucu etkileyen tüm ilkeleri görüntüleyebilirsiniz. 
    
**S: Koşullu erişim durumu için olası tüm değerler nelerdir?**

**C:** Koşullu erişim durumu aşağıdaki değerlere sahip olabilir:

* **Uygulanmadı**: Bu, kapsamdaki Kullanıcı ve uygulamayla bir CA ilkesi olmadığı anlamına gelir. 
* **Başarı**: Bu, kapsamdaki Kullanıcı ve uygulamayla bir CA ilkesi olduğu ve CA ilkelerinin başarıyla karşılanmadığı anlamına gelir. 
* **Hata**: Bu, kapsamdaki Kullanıcı ve uygulamayla bir CA ilkesi olduğu ve CA ilkelerinin karşılanmadığı anlamına gelir. 
    
**S: Koşullu erişim ilkesi sonucu için olası tüm değerler nelerdir?**

**C:** Koşullu erişim ilkesi aşağıdaki sonuçlara sahip olabilir:

* **Başarı**: İlke başarıyla karşılanmadı.
* **Hata**: İlke karşılanmadı.
* **Uygulanmadı**: Bunun nedeni, ilke koşullarının uymamaları olabilir.
* **Etkin değil**: Bunun nedeni ilke devre dışı durumda. 
    
**S: Tüm oturum açma raporundaki ilke adı, CA 'daki ilke adı ile eşleşmiyor. Neden?**

**C:** Tüm oturum açma raporundaki ilke adı, oturum açma sırasında CA ilkesi adını temel alır. İlke adını daha sonra (oturum açma işleminden sonra) güncelleştirdiyseniz, bu, CA 'daki ilke adı ile tutarsız olabilir.

**S: Oturum açma, koşullu erişim ilkesi nedeniyle engellendi, ancak oturum açma Etkinliği raporu, oturum açma işlemi başarılı olduğunu gösteriyor. Neden?**

**C:** Şu anda oturum açma raporu, koşullu erişim uygulandığında Exchange ActiveSync senaryolarına doğru sonuçları göstermeyebilir. Raporda, oturum açma işleminin başarılı bir oturum açma gösterdiği durumlarda, ancak oturum açma işlemi aslında bir koşullu erişim ilkesi nedeniyle başarısız olduysa, bu durum oluşabilir. 
