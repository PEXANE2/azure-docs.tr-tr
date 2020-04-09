---
title: Azure Active Directory ile tümleştirme
description: Uygulamanızı Azure Etkin Dizinile tümleştirmenin avantajlarını öğrenin ve basitleştirilmiş oturum açma, kimlik yönetimi, çok faktörlü kimlik doğrulama ve erişim denetimi gibi özellikler için kaynak edinin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/08/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev, seoapril2019
ms.openlocfilehash: 4eaca7350ac006a2fcf2eed8d3eb0cb8ef274841
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885710"
---
# <a name="integrating-with-azure-active-directory"></a>Azure Etkin Dizini ile Tümleştirme

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Bu makalede, uygulamanızı Azure Etkin Dizini (Azure AD) ile tümleştirmenin yararları hakkında bilgi edinin ve tümleştirme için kaynaklar elde elabilirsiniz. Azure AD, kuruluşlara bulut uygulamaları için kurumsal sınıf kimlik yönetimi sağlar. Azure AD tümleştirmesi, kullanıcılarınıza kolaylaştırılmış bir oturum açma deneyimi sağlar ve uygulamanızın BT ilkesine uymasına yardımcı olur.

## <a name="how-to-integrate"></a>Nasıl tümleştirilir

Uygulamanızın Azure AD ile tümleştirmesi için çeşitli yollar vardır. Uygulamanız için uygun olduğu kadar çok veya birkaç bu senaryolardan yararlanın.

### <a name="support-azure-ad-as-a-way-to-sign-in-to-your-application"></a>Uygulamanızda oturum açmanın bir yolu olarak Azure AD'yi destekleyin

**Oturum açma sürtünmeyi azaltın ve destek maliyetlerini azaltın.** Uygulamanızda oturum açtıracak Azure AD'yi kullanarak, kullanıcılarınızın hatırlanacak bir adı ve parolası kalmaz. Geliştirici olarak, depolamak ve korumak için bir parolanız daha az olur. Unutulan parola sıfırlamaişlemlerini işlemek zorunda kalmamak tek başına önemli bir tasarruf olabilir. Azure AD yetkileri, Office 365 ve Microsoft Azure gibi dünyanın en popüler bulut uygulamalarından bazıları için oturum açarak oturum açarak oturum açsın. Milyonlarca kuruluştan yüz milyonlarca kullanıcıyla, kullanıcınızın Azure AD'de oturum açmış olma olasılığı yüksektir. [Azure AD oturum açma desteği ekleme](authentication-scenarios.md)hakkında daha fazla bilgi edinin.

**Uygulamanız için kaydolun basitleştirin.**  Uygulamanız için kaydolma sırasında Azure AD, kayıt formunuzu önceden doldurabilmeniz veya tamamen ortadan kaldırabilmeniz için kullanıcı hakkında gerekli bilgileri gönderebilir. Kullanıcılar, sosyal medya ve mobil uygulamalarda bulunanlara benzer tanıdık bir rıza deneyimi yle Azure REKLAM hesaplarını kullanarak uygulamanız için kaydolabilir. Herhangi bir kullanıcı, BT katılımıgerektirmeden Azure AD ile tümleşik bir uygulamaya kaydolabilir ve oturum açabilir. Azure AD Hesabı oturum açma [için uygulamanızı kaydetme](../../app-service/configure-authentication-provider-aad.md)hakkında daha fazla bilgi edinin.

### <a name="browse-for-users-manage-user-provisioning-and-control-access-to-your-application"></a>Kullanıcılara göz atın, kullanıcı sağlamayı yönetin ve uygulamanıza erişimi kontrol edin

**Dizindeki kullanıcılar için göz atın.**  Kullanıcıların e-posta adresleri yazmalarını gerektirmek yerine, kullanıcıların kuruluşlarındaki diğer kişileri davet ederken veya erişim sağlarken arama yapmalarına ve göz atmalarına yardımcı olmak için Microsoft Graph API'sini kullanın. Kullanıcılar, kuruluş hiyerarşisinin ayrıntılarını görüntülemek de dahil olmak üzere tanıdık bir adres defteri stili arabirimi kullanarak göz atabilir. Microsoft Graph [API](https://docs.microsoft.com/graph/overview)hakkında daha fazla bilgi edinin.

**Etkin Dizin gruplarını ve dağıtım listelerini yeniden kullanın ve müşterinizin zaten yönetmekte olduğu dağıtım listelerini kullanın.**  Azure AD, müşterinizin e-posta dağıtımı ve erişimi yönetmek için zaten kullanmakta olduğu grupları içerir. Microsoft Graph API'yi kullanarak, müşterinizin uygulamanızda ayrı bir grup kümesi oluşturmasıve yönetmesini gerektirmek yerine bu grupları yeniden kullanın. Grup bilgileri, oturum açma belirteçleri olarak başvurunuza da gönderilebilir. Microsoft Graph [API](https://docs.microsoft.com/graph/overview)hakkında daha fazla bilgi edinin.

**Uygulamanıza kimlerin erişebilenleri denetlemek için Azure AD'yi kullanın.**  Azure AD'deki yöneticiler ve uygulama sahipleri, uygulamalara erişim atayabilir ve belirli kullanıcılara ve gruplara erişebilir. Microsoft Graph API'yi kullanarak, bu listeyi okuyabilir ve uygulamanızda kaynakların ve erişimin sağlanmasını ve bunların sağlanmasını ve bunların sağlanmasını denetlemeyi denetlemek için kullanabilirsiniz.

**Rollere Dayalı Erişim Denetimi için Azure AD'yi kullanın.**  Yöneticiler ve uygulama sahipleri, uygulamanızı Azure AD'de kaydettirirken tanımladığınız rollere kullanıcıları ve grupları atayabilir. Rol bilgileri oturum açma belirteçleri olarak uygulamanıza gönderilir ve Microsoft Graph API kullanılarak da okunabilir. [Yetkilendirme için Azure AD'yi kullanma](https://cloudblogs.microsoft.com/enterprisemobility/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles/)hakkında daha fazla bilgi edinin.

### <a name="get-access-to-users-profile-calendar-email-contacts-files-and-more"></a>Kullanıcının profiline, takvimine, e-postalarına, kişilere, dosyalara ve daha fazlasına erişin

**Azure AD, Office 365 ve diğer Microsoft iş hizmetleri için yetkilendirme sunucusudur.**  OAuth 2.0'ı kullanarak uygulamanızda oturum açmanız veya geçerli kullanıcı hesaplarınızı Azure AD kullanıcı hesaplarına bağlamanızı desteklemek için Azure AD'yi destekliyorsanız, kullanıcının profiline, takvimine, e-postasına, kişilere, dosyalara ve diğer bilgilerine okuma ve yazma erişimi isteyebilirsiniz. Kullanıcının takvimine sorunsuz bir şekilde etkinlik yazabilir ve OneDrive'larına dosya okuyabilir veya yazabilirsiniz. [Office 365 API'leri'ne erişim](https://msdn.microsoft.com/office/office365/howto/platform-development-overview)hakkında daha fazla bilgi edinin.

### <a name="promote-your-application-in-the-azure-and-office-365-marketplaces"></a>Uygulamanızı Azure ve Office 365 Pazar Yerleri'nde tanıtın

**Uygulamanızı Azure AD'yi kullanmakta olan milyonlarca kuruluşa tanıtın.**  Bu pazarlarda arama yapan ve bu pazaryerleri arama ve göz atanlar zaten bir veya daha fazla bulut hizmeti kullanarak onları nitelikli bulut hizmeti müşterileri haline getiriyor. Azure [Marketi'nde](https://azure.microsoft.com/marketplace/partner-program/)uygulamanızı tanıtma hakkında daha fazla bilgi edinin.

**Kullanıcılar uygulamanız için kaydolduklarında, uygulama Azure AD erişim panelinde ve Office 365 uygulama başlatıcılarında görünür.**  Kullanıcılar daha sonra hızlı ve kolay bir şekilde uygulamanıza geri dönebilir ve kullanıcı etkileşimini geliştirebilir. Azure AD [erişim paneli](../user-help/active-directory-saas-access-panel-introduction.md)hakkında daha fazla bilgi edinin.

### <a name="secure-device-to-service-and-service-to-service-communication"></a>Güvenli cihazdan servise ve servise servise iletişim

**Hizmetlerin ve aygıtların kimlik yönetimi için Azure AD'yi kullanmak, yazmanız gereken kodu azaltır ve BT'nin erişimi yönetmesini sağlar.**  Hizmetler ve aygıtlar, OAuth'u kullanarak Azure AD'den belirteçler alabilir ve web API'lerine erişmek için bu belirteçleri kullanabilir. Azure AD'yi kullanarak karmaşık kimlik doğrulama kodu yazmaktan kaçınabilirsiniz. Hizmetlerin ve cihazların kimlikleri Azure AD'de depolandığından, BT, uygulamanızda bunu ayrı olarak yapmak zorunda kalmak yerine tuşları yönetebilir ve iptal edebilir.

## <a name="benefits-of-integration"></a>Entegrasyonun faydaları

Azure AD ile tümleştirme, ek kod yazmanızı gerektirmeyen avantajlarla birlikte gelir.

### <a name="integration-with-enterprise-identity-management"></a>Kurumsal kimlik yönetimi ile entegrasyon

**Uygulamanızın BT ilkelerine uymalarına yardımcı olun.**  Kuruluşlar kurumsal kimlik yönetim sistemlerini Azure AD ile tümleştirir, böylece bir kişi kuruluştur, BT'nin ekstra adımlar atması gerekmeden uygulamanız için erişimini otomatik olarak kaybeder. BT, uygulamanıza kimlerin erişebileceğini yönetebilir ve hangi erişim ilkelerinin gerekli olduğunu (örneğin çok faktörlü kimlik doğrulama) karmaşık şirket ilkelerine uymak için kod yazma gereksiniminizi azaltabilir. Azure AD, BT'nin kullanımı izleyabilmesi için yöneticilere uygulamanızda kimlerin oturum açmış olduğuna ait ayrıntılı bir denetim günlüğü sağlar.

**Azure AD, uygulamanızın AD ile tümleşebileceği için Etkin Dizini buluta genişletir.**  Dünyanın dört bir yanındaki birçok kuruluş Active Directory'yi temel oturum açma ve kimlik yönetim sistemi olarak kullanır ve uygulamalarının AD ile çalışmasını gerektirir. Azure AD ile tümleştirme, uygulamanızı Active Directory ile tümleştirir.

### <a name="advanced-security-features"></a>Gelişmiş güvenlik özellikleri

**Çok faktörlü kimlik doğrulama.**  Azure AD, yerel çok faktörlü kimlik doğrulaması sağlar. BT yöneticileri, uygulamanıza erişmek için çok faktörlü kimlik doğrulaması gerektirebilir, böylece bu desteği kendiniz kodlamanız gerekmez. [Çok Faktörlü Kimlik Doğrulama](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)hakkında daha fazla bilgi edinin.

**Tespitte anormal işaret.**  Azure AD, şüpheli etkinlikleri algılamak ve BT yöneticilerine olası sorunları bildirmek için makine öğrenimi algoritmalarını kullanırken günde bir milyardan fazla oturum açma işlemi işler. Azure AD oturum açma'yı destekleyerek, uygulamanız bu korumadan yararlanır. [Azure Active Directory erişim raporunu görüntüleme](../active-directory-view-access-usage-reports.md)hakkında daha fazla bilgi edinin.

**Koşullu Erişim.**  Çok faktörlü kimlik doğrulamaya ek olarak, yöneticiler kullanıcıların uygulamanızda oturum açabilmeleri için belirli koşulların karşılanmasını gerektirebilir. Ayarlanabilecek koşullar arasında istemci aygıtlarının IP adres aralığı, belirtilen gruplara üyelik ve erişim için kullanılan aygıtın durumu yer almaktadır. [Azure Active Directory Koşullu Erişim](../active-directory-conditional-access-azure-portal.md)hakkında daha fazla bilgi edinin.

### <a name="easy-development"></a>Kolay geliştirme

**Endüstri standardı protokoller.**  Microsoft, endüstri standartlarını desteklemeye kendini adamıştır. Microsoft kimlik platformu, endüstri standardı OAuth 2.0 ve OpenID Connect 1.0 protokollerini destekler. [Microsoft kimlik platformu kimlik doğrulama protokolleri](active-directory-v2-protocols.md)hakkında daha fazla bilgi edinin.

**Açık kaynak kitaplıkları.**  Microsoft, geliştirmeyi hızlandırmak için popüler diller ve platformlar için tam destekli açık kaynak kitaplıkları sağlar. Kaynak kodu Apache 2.0 altında lisanslanır ve çatal ve projelere geri katkıda bulunmak için ücretsizdir. [Microsoft Kimlik Doğrulama Kitaplığı (MSAL)](reference-v2-libraries.md)hakkında daha fazla bilgi edinin.

### <a name="worldwide-presence-and-high-availability"></a>Dünya çapında ki varlık ve yüksek kullanılabilirlik

**Azure AD, dünyanın dört bir yanındaki veri merkezlerinde dağıtılır ve 24 saat yönetilir ve izlenir.**  Azure AD, Microsoft Azure ve Office 365 için kimlik yönetim sistemidir ve dünya çapında 28 veri merkezinde dağıtılır. Dizin verilerinin en az üç veri merkezine çoğaltılması garanti edilir. Genel yük dengeleyicileri, kullanıcıların verilerini içeren Azure AD'nin en yakın kopyasına erişmesini ve bir sorun algılanması durumunda istekleri otomatik olarak diğer veri merkezlerine yeniden yönlendirmesini sağlar.

## <a name="next-steps"></a>Sonraki adımlar

[Kod yazmaya başlayın.](v2-overview.md#getting-started)

[Microsoft kimlik platformlarını kullanarak oturum açma](authentication-scenarios.md)

