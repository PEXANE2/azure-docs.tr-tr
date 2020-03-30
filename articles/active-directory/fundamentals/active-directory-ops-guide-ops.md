---
title: Azure Active Directory genel işlemler kılavuzu başvurusu
description: Bu işlemler başvuru kılavuzu, genel işlemleri güvence altına almak için çekmeniz gereken denetimleri ve eylemleri açıklar
services: active-directory
author: martincoetzer
manager: daveba
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: fundamentals
ms.date: 10/31/2019
ms.author: martinco
ms.openlocfilehash: d039373d3e70076149da2b970a234b59d7aa661a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75422940"
---
# <a name="azure-active-directory-general-operations-guide-reference"></a>Azure Active Directory genel işlemler kılavuzu başvurusu

Azure AD [işlemleri başvuru kılavuzunun](active-directory-ops-guide-intro.md) bu bölümünde, Azure Etkin Dizini'nin (Azure AD) genel işlemlerini optimize etmek için çekmeniz gereken denetimler ve eylemler açıklanmaktadır.

> [!NOTE]
> Bu öneriler yayımlama tarihi itibariyle geçerlidir, ancak zaman içinde değişebilir. Kuruluşlar, Microsoft ürünleri ve hizmetleri zaman içinde geliştikçe operasyonel uygulamalarını sürekli olarak değerlendirmelidir.

## <a name="key-operational-processes"></a>Temel operasyonel süreçler

### <a name="assign-owners-to-key-tasks"></a>Anahtar görevlere sahipleri atama

Azure Active Directory'nin yönetimi, kullanıma sunulması projesinin parçası olmayabilecek önemli operasyonel görevlerin ve süreçlerin sürekli olarak yürütülmesini gerektirir. Ortamınızı en iyi duruma getirmek için bu görevleri ayarlamanız yine de önemlidir. Önemli görevler ve önerilen sahipleri şunlardır:

| Görev | Sahip |
| :- | :- |
| Kimlik Güvenli Puanında Sürücü Geliştirmeleri | InfoSec Operasyon Ekibi |
| Azure AD Bağlantı Sunucularını Koruyun | IAM Operasyon Ekibi |
| IdFix Raporlarını düzenli olarak yürütmek ve üçe kıvırın | IAM Operasyon Ekibi |
| Triage Azure AD Senkronizasyon ve AD FS için Sistem Durumu Uyarıları bağlayın | IAM Operasyon Ekibi |
| Azure AD Connect Health kullanmıyorsa, müşteri yebenzer işlem ve özel altyapıyı izlemek için araçlara sahiptir | IAM Operasyon Ekibi |
| AD FS kullanmıyorsanız, müşteri özel altyapıyı izlemek için eşdeğer işlem ve araçlara sahiptir | IAM Operasyon Ekibi |
| Hibrit Günlükleri İzle: Azure AD App Proxy Bağlayıcıları | IAM Operasyon Ekibi |
| İzle Hibrit Günlükleri: Passthrough Kimlik Doğrulama Aracıları | IAM Operasyon Ekibi |
| Hibrit Günlükleri İzle: Şifre Yazma Hizmeti | IAM Operasyon Ekibi |
| Hibrit Günlükleri İzle: Şirket içi parola koruma ağ geçidi | IAM Operasyon Ekibi |
| Karma Günlükleri İzle: Azure MFA NPS Uzantısı (varsa) | IAM Operasyon Ekibi |

Listenizi gözden geçirerken, sahibi eksik olan görevler için bir sahip atamanız veya yukarıdaki önerilerle uyumlu olmayan sahiplerle olan görevlerin sahipliğini ayarlamanız gerekebilir.

#### <a name="owners-recommended-reading"></a>Sahipleri okuma tavsiye

- [Azure Active Directory’de yönetici rolü atama](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal)
- [Azure’da idare](https://docs.microsoft.com/azure/security/governance-in-azure)

## <a name="hybrid-management"></a>Hibrit yönetimi

### <a name="recent-versions-of-on-premises-components"></a>Şirket içi bileşenlerin son sürümleri

Şirket içi bileşenlerin en güncel sürümlerine sahip olmak, müşteriye en son güvenlik güncelleştirmelerini, performans iyileştirmelerini ve ortamı daha da basitleştirmeye yardımcı olabilecek işlevselliği sağlar. Çoğu bileşen, yükseltme işlemini otomatikleştiren otomatik yükseltme ayarına sahiptir.

Bu bileşenler şunlardır:

- Azure AD Connect
- Azure AD Uygulama Proxy Bağlayıcıları
- Azure AD Geçiş kimlik doğrulama aracıları
- Azure AD Sağlık Aracılarını Bağlayın

Bir tane oluşturulmadığı sürece, bu bileşenleri yükseltmek için bir işlem tanımlamalı ve mümkün olduğunca otomatik yükseltme özelliğine güvenmelisiniz. Altı veya daha fazla ay gerisinde bileşenler bulursanız, en kısa sürede yükseltmeniz gerekir.

#### <a name="hybrid-management-recommended-reading"></a>Hibrid yönetimi okuma önerilir

- [Azure AD Connect: Otomatik yükseltme](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-automatic-upgrade)
- [Azure AD Uygulama Proxy bağlayıcılarını anlama | Otomatik güncelleştirmeler](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-connectors#automatic-updates)

### <a name="azure-ad-connect-health-alert-baseline"></a>Azure AD Connect Sistem Durumu uyarı taban çizgisi

Kuruluşlar, Azure AD Connect ve AD FS'nin izlenmesi ve raporlanması için [Azure AD Connect Health'i](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-azure-ad-connect#what-is-azure-ad-connect-health) dağıtmalıdır. Azure AD Connect ve AD FS, yaşam döngüsü yönetimini ve kimlik doğrulamasını kırabilen ve bu nedenle kesintilere yol açan kritik bileşenlerdir. Azure AD Connect Health, şirket içi kimlik altyapınızı izlemenize ve öngörüler elde ederek ortamınızın güvenilirliğini sağlamaya yardımcı olur.

![Azure AD Connect Heath mimarisi](./media/active-directory-ops-guide/active-directory-ops-img16.png)

Ortamınızın durumunu izlerken, yüksek önem uyarılarını hemen ele almalı ve ardından daha düşük önem uyarılarını izlemelisiniz.

#### <a name="azure-ad-connect-health-recommended-reading"></a>Azure AD Connect Health önerilen okuma

- [Azure AD Connect Sağlık Aracısı Yükleme](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-agent-install)

### <a name="on-premises-agents-logs"></a>Şirket içi aracıgünlükleri

Bazı kimlik ve erişim yönetimi hizmetleri, karma senaryoları etkinleştirmek için şirket içi aracılar gerektirir. Bunlara örnek olarak parola sıfırlama, geçiş kimlik doğrulaması (PTA), Azure AD Application Proxy ve Azure MFA NPS uzantısı verilebilir. Sistem Merkezi Operasyon Yöneticisi veya SIEM gibi çözümleri kullanarak bileşen aracı günlüklerini arşivleyerek ve analiz ederek operasyon ekibinin taban çizgisini ve bu bileşenlerin durumunu izlemesi önemlidir. Infosec Operations ekibinizin veya yardım masanızın hata kalıplarını nasıl gidereceklerini anlamanız da aynı derecede önemlidir.

#### <a name="on-premises-agents-logs-recommended-reading"></a>Şirket içi aracılar günlükleri okuma önerilir

- [Uygulama Ara Sunucusu ile ilgili sorunları giderme](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot)
- [Self servis parola sıfırlama sorun giderme- Azure Etkin Dizini](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-troubleshoot#password-writeback-event-log-error-codes)
- [Azure AD Uygulama Proxy bağlayıcılarını anlama](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors)
- [Azure AD Connect: Geçiş Kimlik Doğrulaması Sorunu](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-pass-through-authentication#collecting-pass-through-authentication-agent-logs)
- [Azure MFA NPS uzantısı için sorun giderme hata kodları](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-errors)

### <a name="on-premises-agents-management"></a>Şirket içi acente yönetimi

En iyi uygulamaların benimsenmesi, şirket içi ajanların en iyi şekilde çalışmasına yardımcı olabilir. Aşağıdaki en iyi uygulamaları göz önünde bulundurun:

- Proxy uygulamalarına erişirken tek bir hata noktasından kaçınarak sorunsuz yük dengelemesi ve yüksek kullanılabilirlik sağlaması önerilir. Şu anda üretimdeki uygulamaları işleyen bir bağlayıcı grubunda yalnızca bir bağlayıcınız varsa, artıklık için en az iki bağlayıcı dağıtmanız gerekir.
- Hata ayıklama amacıyla bir uygulama proxy bağlayıcı grubu oluşturmak ve kullanmak, sorun giderme senaryoları ve yeni şirket içi uygulamalara binme de yararlı olabilir. Ayrıca bağlayıcı makinelere Message Analyzer ve Fiddler gibi ağ araçları yüklemenizi öneririz.
- Kimlik doğrulama akışı sırasında tek bir hata noktasından kaçınarak sorunsuz yük dengelemesi ve yüksek kullanılabilirlik sağlamak için birden çok geçiş kimlik doğrulama aracısı önerilir. Artıklık için en az iki geçiş kimlik doğrulama aracısı dağıttığından emin olun.

#### <a name="on-premises-agents-management-recommended-reading"></a>Şirket içi acente yönetimi okumayı tavsiye etti

- [Azure AD Uygulama Proxy bağlayıcılarını anlama](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-connectors)
- [Azure AD Pass-through Kimlik Doğrulaması - quickstart](../hybrid/how-to-connect-pta-quick-start.md#step-4-ensure-high-availability)

## <a name="management-at-scale"></a>Ölçekte yönetim

### <a name="identity-secure-score"></a>Kimlik secure score’u

[Kimlik güvenli puanı,](https://docs.microsoft.com/azure/active-directory/fundamentals/identity-secure-score) kuruluşunuzun güvenlik duruşunun ölçülebilir bir ölçüsünü sağlar. Rapor edilen bulguları sürekli gözden geçirmenin ve adresleştirmek ve mümkün olan en yüksek puana sahip olmak için çabalamak anahtardır. Puanın yardımıyla:

- Kimlik güvenliği duruşunuzu nesnel olarak ölçebilirsiniz
- Kimlik güvenliği geliştirmelerini planlayabilirsiniz
- Geliştirmelerinizin başarısını gözden geçirebilirsiniz

![Güvenlik puanı](./media/active-directory-ops-guide/active-directory-ops-img17.png)

Kuruluşunuzun şu anda Kimlik Güvenli Puanı'ndaki değişiklikleri izlemek için bir programı yoksa, bir plan uygulamanız ve iyileştirme eylemlerini izlemek ve yönlendirmek için sahipleri atamanız önerilir. Kuruluşlar, en kısa sürede 30'un üzerinde bir puan etkisi ile iyileştirme eylemlerini düzeltmelidir.

### <a name="notifications"></a>Bildirimler

Microsoft, hizmetteki çeşitli değişiklikleri, gereken yapılandırma güncelleştirmelerini ve yönetici müdahalesi gerektiren hataları bildirmek için yöneticilere e-posta iletileri gönderir. Müşterilerin bildirimlerin tüm bildirimleri kabul edebilen ve bunlara göre hareket edebilen uygun ekip üyelerine gönderilmesi için bildirim e-posta adreslerini ayarlamaları önemlidir. [Office 365 İleti Merkezi'ne](https://docs.microsoft.com/office365/admin/manage/message-center) birden fazla alıcı eklemenizi ve bildirimlerin (Azure AD Bağlantı Durumu bildirimleri dahil) bir dağıtım listesine veya paylaşılan posta kutusuna gönderilmesini talep etmenizi öneririz. Yalnızca bir e-posta adresine sahip bir global yönetici hesabınız varsa, en az iki e-posta özellikli hesabı yapılandırmayı unutmayın.

Azure AD tarafından kullanılan iki "Gönderen" <o365mc@email2.microsoft.com>adresi vardır: Office 365 İleti Merkezi bildirimleri gönderen; ve <azure-noreply@microsoft.com>, ilgili bildirimler gönderir:

- [Azure AD Erişim Yorumları](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)
- [Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-operations#enable-email-notifications)
- [Azure AD Kimlik Koruması](/azure/active-directory/identity-protection/howto-identity-protection-configure-notifications)
- [Azure AD Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-email-notifications)
- [Kurumsal Uygulama Süresi Dolan Sertifika Bildirimleri](https://docs.microsoft.com/azure/active-directory/manage-apps/manage-certificates-for-federated-single-sign-on#add-email-notification-addresses-for-certificate-expiration)
- Kurumsal Uygulama Sağlama Hizmeti Bildirimleri

Gönderilen bildirimlerin türünü ve bunların nerede denetleneceğini öğrenmek için aşağıdaki tabloya bakın:

| Bildirim kaynağı | Ne gönderilir | Kontrol edile |
|:-|:-|:-|
| Teknik iletişim | Eşitleme hataları | Azure portalı - özellikleri blade |
| Office 365 İleti Merkezi | Kimlik Hizmetleri ve O365 arka uç hizmetlerinin olay ve bozulma bildirimleri | Ofis Portalı |
| Kimlik Koruma Haftalık Digest | Kimlik Koruma Özeti | Azure AD Kimlik Koruma bıçağı |
| Azure AD Connect Health | Uyarı bildirimleri | Azure portalı - Azure AD Connect Health blade |
| Kurumsal Uygulamalar Bildirimleri | Sertifikaların süresi dolmak üzereyken bildirimler ve düzeltme hataları | Azure portalı - Kurumsal Uygulama bıçağı (her uygulamanın kendi e-posta adresi ayarı vardır) |

#### <a name="notifications-recommended-reading"></a>Bildirimler okuma önerilir

- [Kuruluşunuzun adresini, teknik ilgili kişilerini ve daha fazlasını değiştirme - Office 365](https://docs.microsoft.com/office365/admin/manage/change-address-contact-and-more)

## <a name="operational-surface-area"></a>Operasyonel yüzey alanı

### <a name="ad-fs-lockdown"></a>AD FS kilitleme

Uygulamaları doğrudan Azure AD'ye doğrulayabilmek için yapılandırılan kuruluşlar [Azure AD akıllı kilitlemeden](https://docs.microsoft.com/azure/active-directory/active-directory-secure-passwords)yararlanır. Windows Server 2012 R2'de AD FS kullanıyorsanız, AD FS [extranet kilitleme koruması](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-soft-lockout-protection)uygulayın. Windows Server 2016 veya sonraki lerinde AD FS kullanıyorsanız, [extranet akıllı kilitleme](https://support.microsoft.com/help/4096478/extranet-smart-lockout-feature-in-windows-server-2016)uygulayın. En azından, şirket içi Active Directory'ye karşı kaba kuvvet saldırıları riskini içerecek şekilde extranet lokavtını etkinleştirmenizi öneririz. Ancak, Windows 2016 veya daha yüksek ad FS'niz varsa, [parola püskürtme](https://www.microsoft.com/microsoft-365/blog/2018/03/05/azure-ad-and-adfs-best-practices-defending-against-password-spray-attacks/) saldırılarını azaltmaya yardımcı olacak extranet akıllı kilitlemeyi de etkinleştirmelisiniz.

AD FS yalnızca Azure AD federasyonu için kullanılıyorsa, saldırı yüzey alanını en aza indirmek için kapatılabilen bazı uç noktalar vardır. Örneğin, AD FS yalnızca Azure AD için kullanılıyorsa, **kullanıcı adı karışık** ve **windowstransport**için etkinleştirilen uç noktaları dışındaki WS-Trust uç noktalarını devre dışı bmelisiniz.

### <a name="access-to-machines-with-on-premises-identity-components"></a>Şirket içi kimlik bileşenlerine sahip makinelere erişim

Kuruluşlar, şirket içi karma bileşenlere sahip makinelere erişimi şirket içi etki alanınızla aynı şekilde kilitlemelidir. Örneğin, bir yedekleme operatörü veya Hyper-V yöneticisi kuralları değiştirmek için Azure AD Connect Server'da oturum açamamalıdır.

Active Directory yönetim katmanı modeli, Çevrenin tam denetimi (Tier 0) ile saldırganların sık sık ödün verdiği yüksek riskli iş istasyonu varlıkları arasında bir arabellek bölgeleri kümesi kullanarak kimlik sistemlerini korumak üzere tasarlanmıştır. ![Katman modelinin üç katmanını gösteren diyagram](./media/active-directory-ops-guide/active-directory-ops-img18.png)

[Katman modeli](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material) üç düzeyden oluşur ve yalnızca standart kullanıcı hesaplarını değil, yönetim hesaplarını içerir.

- **Tier 0** - Çevredeki kurumsal kimliklerin doğrudan kontrolü. Katman 0 Active Directory ormanının, etki alanlarının veya etki alanı denetleyicilerinin ve içindeki tüm varlıkların doğrudan ya da dolaylı yönetimsel denetimine sahip hesapları, grupları ve diğer varlıkları içerir. Tüm Katman 0 varlıkları birbirlerini etkin bir şekilde denetlediği için güvenlik açısından hassasiyetleri denktir.
- **Tier 1** - Kurumsal sunucuların ve uygulamaların kontrolü. Katman 1 varlıklarına sunucu işletim sistemleri, bulut hizmetleri ve kuruluş uygulamaları dahildir. Katman 1 yönetim hesapları, bu varlıklar üzerinde barındırılan önemli miktarda iş değeri üzerinde yönetimsel denetime sahiptir. Yaygın olarak kullanılan bir rol örneği, bu işletim sistemlerinin bakımını yapan ve tüm kuruluş hizmetlerini etkileme imkanına sahip olan sunucu yöneticileridir.
- **Tier 2** - Kullanıcı iş istasyonlarının ve cihazların kontrolü. Katman 2 yönetim hesapları, kullanıcı iş istasyonları ve cihazları üzerinde barındırılan önemli miktarda iş değeri üzerinde yönetimsel denetime sahiptir. Örnek olarak neredeyse tüm kullanıcı verilerinin bütünlüğünü etkileyebilecek Yardım Masası ve bilgisayar destek yöneticileri verilebilir.

Azure AD Connect, AD FS ve SQL hizmetleri gibi şirket içi kimlik bileşenlerine erişimi, etki alanı denetleyicilerinde olduğu gibi kilitleyin.

## <a name="summary"></a>Özet

Güvenli bir kimlik altyapısının yedi yönü vardır. Bu liste, Azure Etkin Dizini (Azure AD) için işlemleri optimize etmek için gerçekleştirdiğiniz eylemleri bulmanıza yardımcı olur.

- Önemli görevlere sahipleri atayın.
- Şirket içi hibrit bileşenler için yükseltme işlemini otomatikleştirin.
- Azure AD Connect ve AD FS'nin izlenmesi ve raporlanması için Azure AD Connect Health'i dağıtın.
- System Center Operations Manager veya SIEM çözümünü kullanarak bileşen aracı günlüklerini arşivleyerek ve analiz ederek şirket içi hibrit bileşenlerin durumunu izleyin.
- Identity Secure Score ile güvenlik duruşunuzu ölçerek güvenlik iyileştirmeleri uygulayın.
- AD FS'yi kilitleyin.
- Şirket içi kimlik bileşenlerine sahip makinelere erişimi kilitleyin.

## <a name="next-steps"></a>Sonraki adımlar

Dağıtmadığınız tüm özelliklerle ilgili uygulama ayrıntıları için [Azure AD dağıtım planlarına](active-directory-deployment-plans.md) bakın.
