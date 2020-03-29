---
title: Koşullu Erişim ilkesinde oturum denetimleri - Azure Etkin Dizini
description: Azure AD Koşullu Erişim ilkesinde oturum denetimleri nedir
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 02/11/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: e99b9b87f939d614679fdecf24c9d36d99bf2938
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78671905"
---
# <a name="conditional-access-session"></a>Koşullu Erişim: Oturum

Koşullu Erişim ilkesi içinde, yönetici belirli bulut uygulamaları içinde sınırlı deneyimleri etkinleştirmek için oturum denetimlerinden yararlanabilir.

![Çok faktörlü kimlik doğrulaması gerektiren hibe denetimine sahip Koşullu Erişim ilkesi](./media/concept-conditional-access-session/conditional-access-session.png)

## <a name="application-enforced-restrictions"></a>Uygulama zorunlu kısıtlamalar

Kuruluşlar bu denetimi, Azure AD'nin aygıt bilgilerini seçili bulut uygulamalarına aktarmasını istemek için kullanabilir. Cihaz bilgileri, bulut uygulamalarının uyumlu veya etki alanı yla birleştirilmiş bir aygıttan bağlantı başlatılıp başlatılmadığını bilmesini sağlar. Bu denetim yalnızca SharePoint Online ve Exchange Online'ı seçili bulut uygulamaları olarak destekler. Bulut uygulaması seçildiğinde, kullanıcılara cihaz durumuna bağlı olarak sınırlı veya tam bir deneyim sağlamak için cihaz bilgilerini kullanır.

Uygulama zorunlu kısıtlamalarının kullanımı ve yapılandırması hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [SharePoint Online ile sınırlı erişim sağlama](/sharepoint/control-access-from-unmanaged-devices)
- [Exchange Online ile sınırlı erişim sağlama](https://aka.ms/owalimitedaccess)

## <a name="conditional-access-application-control"></a>Koşullu Erişim uygulama kontrolü

Koşullu Erişim Uygulama Denetimi ters proxy mimarisi kullanır ve Azure AD Koşullu Erişim ile benzersiz bir şekilde tümleştirilir. Azure AD Koşullu Erişim, kuruluşunuzun uygulamalarında belirli koşullara bağlı olarak erişim denetimleri uygulamanıza olanak tanır. Koşullar, koşullu Erişim ilkesinin kime (kullanıcı veya kullanıcı grubu) ve hangi (hangi bulut uygulamaları) ve nerede (hangi konumlar ve ağlar) uygulandığını tanımlar. Koşulları belirledikten sonra, kullanıcıları, erişim ve oturum denetimleri uygulayarak Koşullu Erişim Uygulaması Denetimi ile verileri koruyabileceğiniz [Microsoft Bulut Uygulaması Güvenliği'ne](/cloud-app-security/what-is-cloud-app-security) yönlendirebilirsiniz.

Koşullu Erişim Uygulaması Denetimi, kullanıcı uygulaması erişiminin ve oturumlarının erişim ve oturum ilkelerine bağlı olarak gerçek zamanlı olarak izlenmesini ve denetlenmesini sağlar. Erişim ve oturum ilkeleri, filtreleri daha da hassaslaştırmak ve kullanıcı üzerinde yapılacak eylemleri ayarlamak için Bulut Uygulaması Güvenliği portalında kullanılır. Erişim ve oturum ilkeleriyle şunları yapabilirsiniz:

- Veri sızmasını önleyin: Hassas belgelerin karşıdan yüklenmemesini, kesilmesini, kopyalanmasını ve yazdırmasını engelleyebilirsiniz, örneğin yönetilmeyen aygıtlarda.
- İndirmede koruyun: Hassas belgelerin karşıdan yüklendiğini engellemek yerine, belgelerin Azure Bilgi Koruması ile etiketlenmesini ve korunmasını talep edebilirsiniz. Bu eylem, belgenin korunmasını ve kullanıcı erişiminin riskli olabilecek bir oturumda kısıtlanmasını sağlar.
- Etiketlenmemiş dosyaların yüklenmesini engelleyin: Hassas bir dosya başkaları tarafından yüklenmeden, dağıtılmadan ve kullanılmadan önce, dosyanın doğru etiket e ve korumaya sahip olduğundan emin olmak önemlidir. Kullanıcı içeriği sınıflandırana kadar hassas içeriğe sahip etiketlenmemiş dosyaların yüklenmesini engelleyebilirsiniz.
- Uyumluluk için kullanıcı oturumlarını izleyin: Riskli kullanıcılar uygulamalarda oturum açtıklarında izlenir ve eylemleri oturum içinden kaydedilir. Gelecekte oturum ilkelerinin nerede ve hangi koşullar altında uygulanması gerektiğini anlamak için kullanıcı davranışını araştırabilir ve çözümleyebilirsiniz.
- Engelleme erişimi: Çeşitli risk faktörlerine bağlı olarak belirli uygulamalar ve kullanıcılar için erişimi ayrıntılı olarak engelleyebilirsiniz. Örneğin, istemci sertifikalarını aygıt yönetimi biçimi olarak kullanıyorlarsa bunları engelleyebilirsiniz.
- Özel etkinlikleri engelleyin: Bazı uygulamaların risk taşıyan benzersiz senaryoları vardır, örneğin Microsoft Teams veya Slack gibi uygulamalarda hassas içeriğe sahip iletiler gönderir. Bu tür senaryolarda, hassas içerik için iletileri tarayabiliyor ve bunları gerçek zamanlı olarak engelleyebilirsiniz.

Daha fazla bilgi için, [özellikli uygulamalar için Koşullu Erişim Uygulama Denetimini Dağıt makalesine](/cloud-app-security/proxy-deployment-aad)bakın.

## <a name="sign-in-frequency-preview"></a>Oturum açma sıklığı (Önizleme)

Oturum açma sıklığı, bir kaynağa erişmeye çalışırken kullanıcıdan yeniden oturum açmasının istenmesinden önceki süreyi tanımlar.

Oturum açma sıklığı ayarı, standartlara uygun OAUTH2 veya OIDC protokollerini uygulayan uygulamalarla çalışır. Aşağıdaki web uygulamaları da dahil olmak üzere Windows, Mac ve Mobile için Microsoft'un yerel uygulamalarının çoğu ayarı uygun.

- Word, Excel, PowerPoint Online
- OneNote Online
- Office.com
- O365 Yönetici portalı
- Exchange Online
- SharePoint ve OneDrive
- Takımlar web istemcisi
- Dynamics CRM Online
- Azure portalında

Daha fazla bilgi için, [Koşullu Erişim ile kimlik doğrulama oturumu yönetimini yapılandırın](howto-conditional-access-session-lifetime.md#user-sign-in-frequency)makalesine bakın.

## <a name="persistent-browser-session-preview"></a>Kalıcı tarayıcı oturumu (Önizleme)

Kalıcı bir tarayıcı oturumu, kullanıcıların tarayıcı pencerelerini kapatıp yeniden açtıktan sonra oturumda kalmalarını sağlar.

Daha fazla bilgi için, [Koşullu Erişim ile kimlik doğrulama oturumu yönetimini yapılandırın](howto-conditional-access-session-lifetime.md#persistence-of-browsing-sessions)makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu Erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

- [Yalnızca rapor modu](concept-conditional-access-report-only.md)
