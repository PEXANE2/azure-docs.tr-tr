---
title: Azure Active Directory Kimlik Koruması nedir?
description: Azure AD Kimlik Koruması ile riski algılama, düzeltin, araştırın ve analiz edin
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: overview
ms.date: 03/17/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: d2b1d9748b243dcc2104ce7b8e0e8735a7b7276f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79497682"
---
# <a name="what-is-azure-active-directory-identity-protection"></a>Azure Active Directory Kimlik Koruması nedir?

Kimlik Koruması, kuruluşların üç temel görevi yerine getirmesini sağlayan bir araçtır:

- Kimlik tabanlı risklerin algılanmasını ve düzeltilmesini otomatikleştirin.
- Portaldaki verileri kullanarak riskleri araştırın.
- Daha fazla analiz için risk algılama verilerini üçüncü taraf yardımcı programlara aktarın.

Kimlik Koruması, Microsoft'un Azure AD'li kuruluşlardaki konumundan, Microsoft Hesapları'ndaki tüketici alanından ve kullanıcılarınızı korumak için Xbox ile oyun oynarken edindiği öğrenmeleri kullanır. Microsoft, müşterileri belirlemek ve tehditlere karşı korumak için günde 6,5 trilyon sinyali analiz eder.

Kimlik Koruması tarafından oluşturulan ve beslenen sinyaller, erişim kararları almak için Koşullu Erişim gibi araçlara daha fazla aktarılabilir veya kuruluşunuzun verilerine dayalı daha fazla araştırma için bir güvenlik bilgisi ve olay yönetimi (SIEM) aracına geri beslenebilir uygulanan ilkeler.

## <a name="why-is-automation-important"></a>Otomasyon neden önemlidir?

Microsoft'un Kimlik Güvenliği ve Koruma ekibinin başında ki Alex Weinert, [Ekim 2018'deki blog yazısında,](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Eight-essentials-for-hybrid-identity-3-Securing-your-identity/ba-p/275843) olayların hacmiyle uğraşırken otomasyonun neden bu kadar önemli olduğunu şöyle açıklıyor:

> Her gün, makine öğrenimi ve sezgisel sistemlerimiz 800 milyondan fazla farklı hesap için 18 milyar giriş denemesi için risk puanları sağlamaktadır ve bunların 300 milyonu düşmanlar (suç aktörleri, bilgisayar korsanları gibi varlıklar) tarafından fark edilir bir şekilde yapılmaktadır.
>
> Geçen yıl Ignite'da kimlik sistemlerimize yapılan en büyük 3 saldırıdan bahsetmiştim. İşte bu saldırıların son hacmi
>   
>   - **İhlal tekrarı**: Mayıs 2018'de 4,6 MILYAR saldırı tespit edildi
>   - **Şifre spreyi**: Nisan 2018'de 350k
>   - **Kimlik avı**: Bu tam olarak ölçmek zordur, ancak Mart 2018'de 23M risk olayları gördük ve bunların çoğu kimlik avıyla ilgili

## <a name="risk-detection-and-remediation"></a>Risk tespiti ve düzeltilmesi

Kimlik Koruması aşağıdaki sınıflandırmalarda riskleri tanımlar:

| Risk algılama türü | Açıklama |
| --- | --- |
| Atipik seyahat | Kullanıcının son oturum açma özelliklerine göre atipik bir konumdan oturum açın. |
| Anonim IP adresi | Anonim bir IP adresinden oturum açın (örneğin: Tor tarayıcısı, anonim VPN'ler). |
| Bilmediğiniz oturum açma özellikleri | Verilen kullanıcı için son zamanlarda görmediğimiz özelliklerle oturum açın. |
| Kötü amaçlı yazılım bağlantılı IP adresi | Kötü amaçlı yazılımbağlantılı BIR IP adresinden oturum açma |
| Sızdırılan Kimlik Bilgileri | Bu risk algılama, kullanıcının geçerli kimlik bilgilerinin sızdırıldığını gösterir |
| Azure AD tehdit zekası | Microsoft'un iç ve dış tehdit istihbarat kaynakları bilinen bir saldırı deseni tespit |

Bu riskler ve nasıl / ne zaman hesaplanır hakkında daha fazla ayrıntı makalede bulunabilir, [Risk nedir](concept-identity-protection-risks.md).

Risk sinyalleri, kullanıcıların şunları yapmasını gerektirmek gibi düzeltme çabalarını tetikleyebilir: Azure Çok Faktörlü Kimlik Doğrulaması gerçekleştirmek, self servis parola sıfırlama kullanarak parolalarını sıfırlama veya yönetici harekete geçene kadar engelleme.

## <a name="risk-investigation"></a>Risk araştırması

Yöneticiler algılamaları gözden geçirebilir ve gerekirse bunlar hakkında el ile işlem yapabilir. Yöneticilerin Kimlik Koruması'ndaki soruşturmalar için kullandıkları üç önemli rapor vardır:

- Riskli kullanıcılar
- Riskli oturum açma işlemleri
- Risk algılamaları

Daha fazla bilgi makalede bulunabilir, [Nasıl: Risk araştırmak](howto-identity-protection-investigate-risk.md).

## <a name="exporting-risk-data"></a>Risk verilerini dışa aktarma

Kimlik Koruma verileri arşiv ve daha fazla araştırma ve birlikte ilişki için diğer araçlara ihraç edilebilir. Microsoft Grafik tabanlı API'ler, kuruluşların BU verileri SIEM'leri gibi bir araçta daha fazla işlemek için toplamasına olanak sağlar. Kimlik Koruma API'sine nasıl erişebileceğinize ilişkin bilgileri makalede bulabilirsiniz, [Azure Active Directory Identity Protection ve Microsoft Graph ile başlayın](howto-identity-protection-graph-api.md)

Kimlik Koruması bilgilerini Azure Sentinel ile tümleştirme hakkındaki bilgiler makalede bulunabilir, [Azure AD Kimlik Koruması'ndan veri bağlayın.](../../sentinel/connect-azure-ad-identity-protection.md)

## <a name="permissions"></a>İzinler

Kimlik Koruması, kullanıcıların erişebilmeleri için Güvenlik Okuyucusu, Güvenlik Operatörü, Güvenlik Yöneticisi, Global Reader veya Global Administrator olmasını gerektirir.

| Rol | Yapabilirim | Yapamam. |
| --- | --- | --- |
| Genel yönetici | Kimlik Koruma'ya tam erişim |   |
| Güvenlik yöneticisi | Kimlik Koruma'ya tam erişim | Kullanıcı için parolayı sıfırlama |
| Güvenlik operatörü | Tüm Kimlik Koruma raporlarını ve Genel Bakış bıçaklarını görüntüleyin <br><br> Kullanıcı riskini göz ardı edin, güvenli oturum açma onaylayın, uzlaşmayı onaylayın | İlkeleri yapılandırma veya değiştirme <br><br> Kullanıcı için parolayı sıfırlama <br><br> Uyarı yapılandırma |
| Güvenlik okuyucusu | Tüm Kimlik Koruma raporlarını ve Genel Bakış bıçaklarını görüntüleyin | İlkeleri yapılandırma veya değiştirme <br><br> Kullanıcı için parolayı sıfırlama <br><br> Uyarı yapılandırma <br><br> Algılamalar hakkında geri bildirimde |

Koşullu Erişim yöneticileri, oturum açma riskini koşul olarak hesaba katan ilkeler de oluşturabilir, [Koşullu Erişim: Koşullar](../conditional-access/concept-conditional-access-conditions.md#sign-in-risk)makalesinde daha fazla bilgi bulabilir.

## <a name="license-requirements"></a>Lisans gereksinimleri

[!INCLUDE [Active Directory P2 license](../../../includes/active-directory-p2-license.md)]

| Özellik | Ayrıntılar | Azure AD Premium P2 | Azure AD Premium P1 | Azure AD Temel/Ücretsiz |
| --- | --- | --- | --- | --- |
| Risk politikaları | Kullanıcı risk politikası (Kimlik Koruması yoluyla) | Evet | Hayır | Hayır |
| Risk politikaları | Oturum açma risk politikası (Kimlik Koruması veya Koşullu Erişim ile) | Evet | Hayır | Hayır |
| Güvenlik raporları | Genel Bakış | Evet | Hayır | Hayır |
| Güvenlik raporları | Riskli kullanıcılar | Tam erişim | Sınırlı Bilgi | Sınırlı Bilgi |
| Güvenlik raporları | Riskli oturum açma işlemleri | Tam erişim | Sınırlı Bilgi | Sınırlı Bilgi |
| Güvenlik raporları | Risk algılamaları | Tam erişim | Sınırlı Bilgi | Hayır |
| Bildirimler | Risk altındaki kullanıcılar uyarı algılandı | Evet | Hayır | Hayır |
| Bildirimler | Haftalık özet | Evet | Hayır | Hayır |
| | MFA kayıt ilkesi | Evet | Hayır | Hayır |

## <a name="next-steps"></a>Sonraki adımlar

- [Güvenliğe genel bakış](concept-identity-protection-security-overview.md)

- [Risk nedir](concept-identity-protection-risks.md)

- [Riskleri azaltmak için kullanılabilen politikalar](concept-identity-protection-policies.md)
