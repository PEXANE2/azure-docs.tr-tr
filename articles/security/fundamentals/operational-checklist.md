---
title: Azure operasyonel güvenlik denetim listesi| Microsoft Dokümanlar
description: Bu makalede, Azure işletim güvenliği için bir denetim listesi kümesi sağlar.
services: security
documentationcenter: na
author: unifycloud
manager: barbkess
editor: tomsh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh
ms.openlocfilehash: fb940857a0f88590cb9bbbf56b9e6a791299309f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75980955"
---
# <a name="azure-operational-security-checklist"></a>Azure operasyonel güvenlik denetim listesi
Bir uygulamayı Azure'da dağıtmak hızlı, kolay ve uygun maliyetlidir. Bulut uygulamasını üretime dağıtmadan önce, uygulamanızı göz önünde bulundurmanız gereken temel ve önerilen operasyonel güvenlik eylemleri listesine karşı değerlendirmenize yardımcı olacak bir denetim listesine sahip olmak yararlıdır.

## <a name="introduction"></a>Giriş

Azure, uygulamalarınızı dağıtmak için kullanabileceğiniz bir altyapı hizmetleri paketi sağlar. Azure Operasyonel Güvenlik, Microsoft Azure'daki verilerini, uygulamalarını ve diğer varlıklarını korumak için kullanıcıların kullanabileceği hizmetler, denetimler ve özellikler anlamına gelir.

-   Bulut platformundan maksimum faydayı elde etmek için Azure hizmetlerinden yararlanmanızı ve denetim listesini izlemenizi öneririz.
-   Lansmandan önce uygulamalarının operasyonel hazır olup olmadığını değerlendiren zaman ve kaynak yatırımı yapan kuruluşlar, çalışmayanlara göre çok daha yüksek bir memnuniyet oranına sahiptir. Bu çalışmayı gerçekleştirirken, denetim listeleri uygulamaların tutarlı ve bütünsel olarak değerlendirilmesini sağlamak için paha biçilmez bir mekanizma olabilir.
-   Operasyonel değerlendirme düzeyi, kuruluşun bulut olgunluk düzeyine ve uygulamanın geliştirme aşamasına, kullanılabilirlik gereksinimlerine ve veri duyarlılığı gereksinimlerine bağlı olarak değişir.

## <a name="checklist"></a>Denetim Listesi

Bu denetim listesi, kuruluşların Azure'da gelişmiş kurumsal uygulamaları dağıtırken çeşitli operasyonel güvenlik hususları üzerinde düşünmelerine yardımcı olmayı amaçlamaktadır. Ayrıca, kuruluşunuz için güvenli bir bulut geçişi ve çalışma stratejisi oluşturmanıza yardımcı olmak için de kullanılabilir.

|Kontrol Listesi Kategorisi| Açıklama|
| ------------ | -------- |
| [<br>Güvenlik Rolleri & Erişim Denetimleri](../../security-center/security-center-planning-and-operations-guide.md)|<ul><li>Belirli bir kapsamda kullanıcılara, gruplara ve uygulamalara izin atamak için kullanılan kullanıcıya özgü erişim [denetimi (RBAC)](../../role-based-access-control/role-assignments-portal.md) kullanın.</li></ul> |
| [<br>Veri Toplama & Depolama](../../storage/blobs/security-recommendations.md)|<ul><li>[Role Tabanlı Erişim Denetimi 'ni (RBAC)](../../role-based-access-control/role-assignments-portal.md)kullanarak Depolama Hesabınızı güvence altına almak için Yönetim Düzlemi Güvenliğini kullanın.</li><li>[Paylaşılan Erişim İmzaları (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md) ve Depolanan Erişim İlkeleri'ni kullanarak Verilerinize Erişimi Güvence Altına Almak için Veri Düzlemi Güvenliği.</li><li>[Azure Dosya Paylaşımları](../../storage/files/storage-dotnet-how-to-use-files.md)için AKTarım Düzeyi Şifrelemesini ve [SMB (Sunucu ileti bloğu protokolleri) 3.0](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) tarafından kullanılan şifrelemeyi kullanma .</li><li>Şifreleme anahtarlarının tek denetimine ihtiyaç duyduğunuzda depolama hesaplarına gönderdiğiniz verileri güvence altına almak için [İstemci tarafı şifrelemesini](../../storage/common/storage-client-side-encryption.md) kullanın. </li><li>Azure Depolama'daki verileri otomatik olarak şifrelemek için [Depolama Hizmeti Şifrelemesini (SSE)](../../storage/common/storage-service-encryption.md) ve Işletim Sistemi ve veri diskleri için sanal makine disk dosyalarını şifrelemek için [Azure Disk Şifreleme'yi](../azure-security-disk-encryption-overview.md) kullanın.</li><li>Yetkilendirme türünü izlemek için Azure [Depolama Analizi'ni](https://docs.microsoft.com/rest/api/storageservices/storage-analytics) kullanın; Blob Depolama'da olduğu gibi, kullanıcıların Paylaşılan Erişim İmzası veya depolama hesabı anahtarlarını kullanıp kullanmadığınızı görebilirsiniz.</li><li>Farklı etki alanlarından depolama kaynaklarına erişmek için [Çapraz Kaynak Paylaşımı'nı (CORS)](https://docs.microsoft.com/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) kullanın.</li></ul> |
|[<br>Güvenlik İlkeleri & Öneriler](../../security-center/security-center-planning-and-operations-guide.md)|<ul><li>Uç nokta çözümlerini dağıtmak için [Azure Güvenlik Merkezi'ni](../../security-center/security-center-install-endpoint-protection.md) kullanın.</li><li>Web uygulamalarını güvenli hale getirmek için bir [web uygulaması güvenlik duvarı (WAF)](../../application-gateway/waf-overview.md) ekleyin.</li><li>   Güvenlik korumalarınızı artırmak için bir Microsoft iş ortağının [güvenlik duvarını](../../sentinel/connect-data-sources.md) kullanın. </li><li>Azure aboneliğiniz için güvenlik iletişim bilgilerini uygulayın; bu, müşteri verilerinize yasadışı veya yetkisiz bir taraf tarafından erişildiğini fark [ederse, Microsoft Güvenlik Yanıt Merkezi (MSRC)](https://technet.microsoft.com/security/dn528958.aspx) sizinle iletişim edeyim.</li></ul> |
| [<br>Kimlik & Erişim Yönetimi](identity-management-best-practices.md)|<ul><li>[Azure AD'yi kullanarak şirket içi dizininizi bulut dizininizle senkronize edin.](../../active-directory/hybrid/whatis-hybrid-identity.md)</li><li>Kullanıcıların Azure AD'deki kuruluş hesaplarını temel alarak SaaS uygulamalarına erişmelerini sağlamak için [Tek Oturum](https://azure.microsoft.com/resources/videos/overview-of-single-sign-on/) Açma'yı kullanın.</li><li>Kayıt olan kullanıcıları izlemek için [Parola Sıfırlama Kayıt Faaliyet](../../active-directory/active-directory-passwords-reporting.md) raporunu kullanın.</li><li>Kullanıcılar için [çok faktörlü kimlik doğrulamayı (MFA)](../../active-directory/authentication/multi-factor-authentication.md) etkinleştirin.</li><li>Geliştiriciler Microsoft Güvenlik Geliştirme Yaşam [Döngüsü (SDL)](https://www.microsoft.com/download/details.aspx?id=12379)gibi uygulamalar için güvenli kimlik özellikleri kullanmak.</li><li>Azure AD Premium anomali raporlarını ve [Azure AD kimlik koruma özelliğini](../../active-directory/identity-protection/overview.md)kullanarak şüpheli etkinlikleri etkin bir şekilde izleyin.</li></ul> |
|[<br>Devam Eden Güvenlik İzleme](../../security-center/security-center-planning-and-operations-guide.md)|<ul><li>Altyapınızdaki kötü amaçlı yazılımdan koruma durumunu bildirmek için Kötü Amaçlı Yazılım Değerlendirme Çözümü [Azure Monitor günlüklerini](../../log-analytics/log-analytics-queries.md) kullanın.</li><li>Olası güvenlik sorunlarına genel olarak maruz kalmanın ve bu güncelleştirmelerin ortamınız için ne kadar önemli olduğunu veya ne kadar kritik olduğunu belirlemek için [Güncelleştirme değerlendirmesini](../../automation/automation-update-management.md) kullanın.</li><li>[Kimlik ve Erişim,](../../security-center/security-center-monitoring.md) kullanıcıya genel bir bakış sağlar </li><ul><li>kullanıcı kimlik durumu,</li><li>oturum açma girişimlerinin sayısı,</li><li> bu girişimler sırasında kullanılan kullanıcının hesabı, kilitlenmiş hesaplar</li> <li>değiştirilen veya parolayı sıfırlanan hesaplar </li><li>Şu anda oturum açan hesap sayısı.</li></ul></ul> |
| [<br>Azure Güvenlik Merkezi algılama özellikleri](../../security-center/security-center-alerts-overview.md#detect-threats)|<ul><li>Microsoft Azure kaynaklarınızı hedefleyen etkin tehditleri belirlemek için [algılama özelliklerini](../../security-center/security-center-alerts-overview.md#detect-threats)kullanın.</li><li>Microsoft ürün ve hizmetlerinden, [Microsoft Dijital Suçlar Birimi'nden (DCU),](https://www.microsoft.com/trustcenter/security/cybercrime)Microsoft Güvenlik Yanıt [Merkezi'nden (MSRC)](response-center.md)ve harici akışlardan küresel tehdit istihbaratından yararlanarak bilinen kötü aktörleri arayan [tümleşik tehdit istihbaratını](https://blogs.msdn.microsoft.com/azuresecurity/2016/12/19/get-threat-intelligence-reports-with-azure-security-center/) kullanın.</li><li>Kötü amaçlı davranışları keşfetmek için bilinen desenleri uygulayan [Davranış salanalizini](https://blogs.technet.microsoft.com/enterprisemobility/2016/06/30/ata-behavior-analysis-monitoring/) kullanın. </li><li>Tarihsel bir taban çizgisi oluşturmak için istatistiksel profil oluşturma yı kullanan [Anomali algılamasını](https://msdn.microsoft.com/library/azure/dn913096.aspx) kullanın.</li></ul> |
| [<br>Geliştirici İşlemleri (DevOps)](https://docs.microsoft.com/azure/architecture/checklist/dev-ops)|<ul><li>[Kod Olarak Altyapı (IaC),](https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/) güvenli ve kararlı uygulama barındırma platformları sunmada yardımcı olmak için ağların ve sanal makinelerin oluşturulması ve yıkılmasının otomasyonu ve doğrulanmasını sağlayan bir uygulamadır.</li><li>[Sürekli Tümleştirme ve Dağıtım,](/visualstudio/containers/overview#continuous-delivery-and-continuous-integration-cicd) kodların sürekli birleştirilmesini ve test edilmesine neden olur ve bu da hataların erken bulunmasına yol açar. </li><li>[Yayın Yönetimi](https://msdn.microsoft.com/library/vs/alm/release/overview) Ardınız ın her aşamasında otomatik dağıtımları yönetin.</li><li>Uygulama durumu ve müşteri kullanımı için üretim ortamları da dahil olmak üzere çalışan uygulamaların [Uygulama Performansı İzlenmesi,](https://azure.microsoft.com/documentation/articles/app-insights-start-monitoring-app-health-usage/) kuruluşların bir hipotez oluşturmasına ve stratejileri hızla doğrulamalarına veya çürütmelerine yardımcı olur.</li><li>[Yük Testi & Otomatik Ölçekkullanarak,](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing) dağıtım kalitesini artırmak ve uygulamamızın iş gereksinimlerini karşılamak için her zaman hazır veya kullanılabilir olduğundan emin olmak için uygulamamızda performans sorunları bulabiliriz.</li></ul> |


## <a name="conclusion"></a>Sonuç
Birçok kuruluş bulut uygulamalarını Azure'da başarıyla dağıttı ve çalıştırdı. Sağlanan denetim listeleri, önemli olan birkaç denetim listesini vurgular ve başarılı dağıtımlar ve hayal kırıklığı içermeyen işlemler olasılığını artırmanıza yardımcı olur. Azure'daki mevcut ve yeni uygulama dağıtımlarınız için bu operasyonel ve stratejik hususları önemle tavsiye ederiz.

## <a name="next-steps"></a>Sonraki adımlar
Güvenlik hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Tasarım ve operasyonel güvenlik.](https://www.microsoft.com/trustcenter/security/designopsecurity)
- [Azure Güvenlik Merkezi planlama ve işlemleri.](../../security-center/security-center-planning-and-operations-guide.md)
