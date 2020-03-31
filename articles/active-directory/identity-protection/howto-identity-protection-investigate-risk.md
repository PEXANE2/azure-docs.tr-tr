---
title: Riski araştırın Azure Etkin Dizin Kimlik Koruması
description: Azure Active Directory Identity Protection'da riskli kullanıcıları, algılamaları ve oturum açmaları nasıl araştırırabilirsiniz öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5191f05752c177aa29d121abe9d1aa29fde265a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253487"
---
# <a name="how-to-investigate-risk"></a>Nasıl Yapılsın: Riski araştırın

Kimlik Koruması, kuruluşlara çevrelerindeki kimlik risklerini araştırmak için kullanabilecekleri üç rapor sağlar. Bu raporlar **riskli kullanıcılar,** **riskli oturum açma ve**risk **tespitleridir.** Olayların araştırılması, güvenlik stratejinizdeki zayıf noktaları daha iyi anlamanın ve tanımlamanın anahtarıdır.

Her üç rapor da olayların indirilmesine olanak sağlar. Azure portalı dışında daha fazla analiz için CSV biçimi. Riskli kullanıcılar ve riskli oturum açma raporları en son 2500 girişi indirmeye olanak sağlarken, risk algılama raporu en son 5000 kaydı indirmeye olanak tanır.

Kuruluşlar, microsoft graph API tümleştirmelerinden yararlanarak verileri kuruluş olarak erişebilecekleri diğer kaynaklarla toplayabilir.

Üç rapor **Azure portalı** > **Azure Active Directory** > **Security'de**bulunur.

## <a name="navigating-the-reports"></a>Raporlarda gezinme

Her rapor, raporun en üstünde gösterilen döneme ait tüm algılamaların bir listesini başlatır. Her rapor, yönetici tercihine göre sütun eklemesine veya kaldırılmasına izin verir. Yöneticiler verileri 'de karşıdan yüklemeyi seçebilirler. CSV veya . JSON biçimi. Raporlar, raporun üst kısmındaki filtreler kullanılarak filtrelenebilir.

Tek tek girişlerin seçilmesi, raporun üst kısmında oturum açmanın tehlikeye girildiğini veya güvenli olarak onaylanması, kullanıcının tehlikeye girildiğini onaylama veya kullanıcı riskini niçin ortadan kattığı gibi ek girişlere olanak sağlayabilir.

Tek tek girişleri seçmek, algılamaların altındaki ayrıntılar penceresini genişletir. Ayrıntılar görünümü, yöneticilerin her algılamayla ilgili eylemleri araştırmasına ve gerçekleştirmesine olanak tanır. 

![Riskli oturum açma ve ayrıntıları gösteren örnek Kimlik Koruma raporu](./media/howto-identity-protection-investigate-risk/identity-protection-risky-sign-ins-report.png)

## <a name="risky-users"></a>Riskli kullanıcılar

Riskli kullanıcı raporu tarafından sağlanan bilgilerle, yöneticiler şunları bulabilir:

- Hangi kullanıcılar risk altında, risk düzeltildi veya risk reddedildi?
- Algılamalar hakkında ayrıntılar
- Tüm riskli oturum açmaların tarihçesi
- Risk geçmişi
 
Yöneticiler daha sonra bu olaylar hakkında işlem yapmayı seçebilir. Yöneticiler şunları seçebilir:

- Kullanıcı parolasını sıfırlama
- Kullanıcı uzlaşmalarını onaylama
- Kullanıcı riskini yok say
- Kullanıcının oturum açmalarını engelleme
- Azure ATP'yi kullanarak daha fazla araştırma

## <a name="risky-sign-ins"></a>Riskli oturum açma işlemleri

Riskli oturum açma raporu, son 30 güne (1 ay) kadar filtrelenebilir veriler içerir.

Riskli oturum açma raporunda verilen bilgilerle yöneticiler şunları bulabilir:

- Hangi oturum açma lar risk altında olarak sınıflandırılır, açığa çıkar, güvenli olarak onaylanır, reddedilir veya düzeltilir.
- Oturum açma girişimleriyle ilişkili gerçek zamanlı ve toplam risk düzeyleri.
- Tetiklenen algılama türleri
- Uygulanan Koşullu Erişim ilkeleri
- MFA ayrıntıları
- Cihaz bilgileri
- Uygulama bilgileri
- Konum bilgileri

Yöneticiler daha sonra bu olaylar hakkında işlem yapmayı seçebilir. Yöneticiler şunları seçebilir:

- Oturum açma uzlaşmasını onaylama
- Oturum açma güvenlionay

## <a name="risk-detections"></a>Risk algılamaları

Risk algılama raporu, son 90 güne (3 ay) kadar filtrelenebilir veriler içerir.

Risk tespitleri raporu tarafından sağlanan bilgilerle, yöneticiler şunları bulabilir:

- Türü de dahil olmak üzere her risk algılama hakkında bilgi.
- Aynı anda tetiklenen diğer riskler
- Oturum açma girişimi konumu
- Microsoft Cloud App Security'den (MCAS) daha fazla ayrıntı yada bağlantı kurun.

Yöneticiler daha sonra toplanan bilgilere dayalı eylemlerde bulunmak için kullanıcının risk veya oturum açma raporuna dönmeyi seçebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Riskleri azaltmak için kullanılabilen politikalar](concept-identity-protection-policies.md)

- [Oturum açma ve kullanıcı risk ilkelerini etkinleştirme](howto-identity-protection-configure-risk-policies.md)
