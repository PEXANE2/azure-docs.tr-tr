---
title: Güvenlik özelliklerini yönetme ve izleme - Microsoft Azure | Microsoft Dokümanlar
description: Bu makalede, Azure bulut hizmetlerinin ve sanal makinelerin yönetimi ve izlenmesine yardımcı olmak için Azure'un sağladığı güvenlik özellikleri ve hizmetlerine genel bir bakış sağlanmaktadır.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 5cf2827b-6cd3-434d-9100-d7411f7ed424
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2019
ms.author: terrylan
ms.openlocfilehash: 7ad7a29a92d25556190b4cf44f4e48158a6f0952
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73162752"
---
# <a name="azure-security-management-and-monitoring-overview"></a>Azure güvenlik yönetimi ve izleme genel bakış
Bu makalede, Azure bulut hizmetlerinin ve sanal makinelerin yönetimi ve izlenmesine yardımcı olmak için Azure'un sağladığı güvenlik özellikleri ve hizmetlerine genel bir bakış sağlanmaktadır.

## <a name="role-based-access-control"></a>Rol Tabanlı Access Control

Role Tabanlı Erişim Denetimi (RBAC), Azure kaynakları için ayrıntılı erişim yönetimi sağlar. RBAC'ı kullanarak, insanlara yalnızca işlerini gerçekleştirmek için gereken erişim miktarını verebilirsiniz. RBAC ayrıca, insanlar kuruluştan ayrıldıklarında buluttaki kaynaklara erişimlerini kaybetmelerini sağlamanıza da yardımcı olabilir.

Daha fazla bilgi edinin:

* [RBAC'da Aktif Dizin ekibi blogu](https://cloudblogs.microsoft.com/enterprisemobility/?product=azure-active-directory)
* [Azure Rol Tabanlı Erişim Denetimi](../../role-based-access-control/role-assignments-portal.md)

## <a name="antimalware"></a>Antimalware

Azure ile Microsoft, Symantec, Trend Micro, McAfee ve Kaspersky gibi büyük güvenlik satıcılarının kötü amaçlı yazılımdan koruma yazılımlarını kullanabilirsiniz. Bu yazılım, sanal makinelerinizi kötü amaçlı dosyalara, reklam yazılımlarına ve diğer tehditlerden korumaya yardımcı olur.

Azure Bulut Hizmetleri ve Sanal Makineler için Microsoft Kötü Amaçlı Yazılımdan Koruma, hem PaaS rolleri hem de sanal makineler için kötü amaçlı yazılımdan koruma aracısı yükleme olanağı sunar. System Center Endpoint Protection'a dayanan bu özellik, kanıtlanmış şirket içi güvenlik teknolojisini buluta getirir.

Ayrıca Azure platformunda Trend'in [Deep Security](https://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/) ve [SecureCloud](https://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/) ürünleri için derin entegrasyon sunuyoruz. Deep Security bir virüsten koruma çözümüdür ve SecureCloud bir şifreleme çözümüdür. Deep Security, bir uzantı modeli aracılığıyla VM'lerin içine dağıtılır. Azure portalı UI ve PowerShell'i kullanarak, yeni Geliştirilen Yeni VM'lerde veya zaten dağıtılan mevcut VM'lerde Derin Güvenlik'i kullanmayı seçebilirsiniz.

Symantec Endpoint Protection (SEP) azure'da da desteklenir. Portal tümleştirmesi sayesinde, SEP'i bir VM'de kullanmayı planladığınızı belirtebilirsiniz. SEP, Azure portalı üzerinden yeni bir VM'ye veya PowerShell üzerinden mevcut bir VM'ye yüklenebilir.

Daha fazla bilgi edinin:

* [Azure Sanal Makinelerinde Kötü Amaçlı Yazılıma Karşı Koruma Çözümleri Dağıtma](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Azure Bulut Hizmetleri ve Sanal Makineler için Microsoft Kötü Amaçlı Yazılımdan Koruma](antimalware.md)
* [Trend Micro Deep Security'yi Windows VM'de Hizmet Olarak yükleme ve yapılandırma](/azure/virtual-machines/windows/classic/install-trend)
* [Windows VM'de Symantec Endpoint Protection nasıl yüklenir ve yapılandırılır?](/azure/virtual-machines/windows/classic/install-symantec)
* [Azure Sanal Makinelerini Korumak için Yeni Kötü Amaçlı Yazılımdan Koruma Seçenekleri](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Azure Çok Faktörlü Kimlik Doğrulama, birden fazla doğrulama yöntemi kullanımını gerektiren bir kimlik doğrulama yöntemidir. Kullanıcı oturum açma ve hareketlere kritik bir ikinci güvenlik katmanı ekler.

Çok Faktörlü Kimlik Doğrulama, basit bir oturum açma işlemi için kullanıcı talebini karşılarken verilere ve uygulamalara erişimi korumaya yardımcı olur. Çeşitli doğrulama seçenekleri (telefon görüşmesi, kısa mesaj veya mobil uygulama bildirimi veya doğrulama kodu) ve üçüncü taraf OATH belirteçleri aracılığıyla güçlü kimlik doğrulaması sağlar.

Daha fazla bilgi edinin:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Azure Multi-Factor Authentication nedir?](/azure/active-directory/authentication/multi-factor-authentication)
* [Azure Multi-Factor Authentication nasıl çalışır?](../../active-directory/authentication/concept-mfa-howitworks.md)

## <a name="expressroute"></a>ExpressRoute

Bir bağlantı sağlayıcısı tarafından kolaylaştırılabilen özel bir bağlantı üzerinden şirket içi ağlarınızı Microsoft Bulutu'na genişletmek için Azure ExpressRoute'u kullanabilirsiniz. ExpressRoute ile Azure, Office 365 ve CRM Online gibi Microsoft bulut hizmetlerine bağlantılar kurabilirsiniz. Bağlantı şu şekilde olabilir:

* Herhangi bir ağ-any (IP VPN) ağı.
* Noktadan noktaya Ethernet ağı.
* Ortak konum tesisindeki bir bağlantı sağlayıcısı aracılığıyla sanal çapraz bağlantı.

ExpressRoute bağlantıları genel internet üzerinden gitmez. Daha fazla güvenilirlik, daha hızlı hızlar, daha düşük gecikme süreleri ve internet üzerinden tipik bağlantılara göre daha yüksek güvenlik sunabilirler.

Daha fazla bilgi edinin:

* [ExpressRoute teknik genel bakış](../../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Sanal ağ geçitleri

Azure sanal ağ ağ geçitleri olarak da adlandırılan VPN ağ geçitleri, sanal ağlar ve şirket içi konumlar arasında ağ trafiği göndermek için kullanılır. Ayrıca Azure içindeki birden çok sanal ağ (ağdan ağa) arasında trafik göndermek için de kullanılırlar. VPN ağ geçitleri, Azure ile altyapınız arasında güvenli binalar arası bağlantı sağlar.

Daha fazla bilgi edinin:

* [VPN ağ geçitleri hakkında](../../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [Azure ağ güvenliğine genel bakış](network-overview.md)

## <a name="privileged-identity-management"></a>Privileged Identity Management

Bazen kullanıcıların Azure kaynaklarında veya diğer SaaS uygulamalarında ayrıcalıklı işlemler gerçekleştirmeleri gerekir. Bu genellikle kuruluşların Azure Etkin Dizini'nde (Azure AD) kalıcı ayrıcalıklı erişim verdiği anlamına gelir.

Kuruluşlar bu kullanıcıların ayrıcalıklı erişimleriyle ne yaptıklarını yeterince izleyemediğinden, bu bulut tarafından barındırılan kaynaklar için büyüyen bir güvenlik riskidir. Ayrıca, ayrıcalıklı erişimi olan bir kullanıcı hesabı tehlikeye girerse, bir ihlal kuruluşun genel bulut güvenliğini etkileyebilir. Azure AD Ayrıcalıklı Kimlik Yönetimi, ayrıcalıkların açığa kalma süresini düşürerek ve kullanıma görünürlüğü artırarak bu riski çözmeye yardımcı olur.  

Ayrıcalıklı Kimlik Yönetimi, bir rol veya "tam zamanında" yönetici erişimi için geçici yönetici kavramını sunar. Bu tür bir yönetici, atanan rol için etkinleştirme işlemini tamamlaması gereken bir kullanıcıdır. Etkinleştirme işlemi, kullanıcının atanmasını belirli bir süre için etkin olmayandan etkin ekime kadar Azure AD'deki bir role değiştirir.

Daha fazla bilgi edinin:

* [Azure AD Privileged Identity Management](../../active-directory/privileged-identity-management/pim-configure.md)
* [Azure AD Privileged Identity Management ile çalışmaya başlama](../../active-directory/privileged-identity-management/pim-getting-started.md)

## <a name="identity-protection"></a>Kimlik Koruması

Azure AD Kimlik Koruması, işletmenizin korunmasına yardımcı olmak için şüpheli oturum açma etkinliklerinin ve olası güvenlik açıklarının birleştirilmiş bir görünümünü sağlar. Kimlik Koruması, kullanıcılar ve ayrıcalıklı (yönetici) kimlikler için şüpheli etkinlikleri şu gibi sinyallere dayanarak algılar:

* Kaba kuvvet saldırıları.
* Sızdırılmış kimlik bilgileri.
* Yabancı yerlerden ve virüslü cihazlardan oturum açma.

Kimlik Koruması, bildirimler ve önerilen düzeltme sağlayarak riskleri gerçek zamanlı olarak azaltmaya yardımcı olur. Kullanıcı risk şiddetini hesaplar. Uygulama erişimini gelecekteki tehditlere karşı otomatik olarak korumaya yardımcı olacak şekilde risk tabanlı ilkeler yapılandırabilirsiniz.

Daha fazla bilgi edinin:

* [Azure Active Directory Kimlik Koruması](/azure/active-directory/active-directory-identityprotection)
* [Kanal 9: Azure REKLAM ve Kimlik Gösterisi: Kimlik Koruma Önizleme](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Güvenlik Merkezi

Azure Güvenlik Merkezi, tehditleri önlemenize, algılamanıza ve yanıtlamanıza yardımcı olur. Güvenlik Merkezi, Azure kaynaklarınızın güvenliği yle ilgili daha fazla görünürlük ve denetim sağlar. Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar. Aksi takdirde fark edilmeyecek tehditleri algılamaya yardımcı olur ve geniş bir güvenlik çözümleri ekosistemi yle çalışır.

Güvenlik Merkezi, Azure kaynaklarınızın güvenliğini şu şekilde optimize edip izlemenize yardımcı olur:

* Azure abonelik kaynaklarınız için ilkeleri şu şekilde tanımlamanızı sağlayan:
  * Şirketinizin güvenlik ihtiyaçları.
  * Her abonelikteki uygulamaların türü veya verilerin duyarlılığı.
* Azure sanal makinelerinizin, ağlarınızın ve uygulamalarınızın durumunu izleme.
* Tümleşik iş ortağı çözümlerinden gelen uyarılar da dahil olmak üzere öncelik verilen güvenlik uyarılarının bir listesini sağlama. Ayrıca, bir saldırıyı hızlı bir şekilde araştırmak için ihtiyacınız olan bilgileri ve saldırıyı nasıl düzeltirdiğinize ilişkin önerileri de sağlar.

Daha fazla bilgi edinin:

* [Azure Güvenlik Merkezi'ne Giriş](../../security-center/security-center-intro.md)
* [Azure Güvenlik Merkezi'nde güvenli puanınızı artırın](../../security-center/security-center-secure-score.md)

## <a name="intelligent-security-graph"></a>Akıllı Güvenlik Grafiği

Akıllı Güvenlik Grafiği, Microsoft ürün ve hizmetlerinde gerçek zamanlı tehdit koruması sağlar. Kuruluş güvenliğini güçlendirebilecek öngörüler sağlamak için büyük miktarda tehdit istihbaratı ve güvenlik verilerini birbirine bağlayan gelişmiş analizler kullanır. Microsoft, daha zengin öngörüler sunmak için ayda 450 milyardan fazla kimlik doğrulaması işleyen, kötü amaçlı yazılım ve kimlik avı için 400 milyar e-postayı tarayan ve bir milyar cihazı güncelleyen gelişmiş analitik kullanır. Bu öngörüler, kuruluşunuzun saldırıları hızlı bir şekilde algılamasına ve bunlara yanıt vermesine yardımcı olabilir.

* [Akıllı Güvenlik Grafiği](https://www.microsoft.com/security/intelligence)

## <a name="next-steps"></a>Sonraki Adımlar
[Paylaşılan sorumluluk modeli](shared-responsibility.md) ve hangi güvenlik görevlerinin Microsoft tarafından işlendiği ve hangi görevlerin sizin yeriniz tarafından işlendiği hakkında bilgi edinin.

Güvenlik yönetimi hakkında daha fazla bilgi için [Azure'da Güvenlik yönetimine](management.md)bakın.
