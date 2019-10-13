---
title: UK resmi & UK NHS şema örnekleri-denetim eşleme
description: UK RESMI ve UK NHS şeması örneklerinin denetim eşlemesi. Her denetim, değerlendirmenize yardımcı olan bir veya daha fazla Azure Ilkesiyle eşleştirilir.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/26/2019
ms.topic: conceptual
ms.service: blueprints
ms.openlocfilehash: 72b7b370158f228d725e9c6970dfcffb09e7986b
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2019
ms.locfileid: "72297123"
---
# <a name="control-mapping-of-the-uk-official-and-uk-nhs-blueprint-samples"></a>UK RESMI ve UK NHS şeması örneklerinin denetim eşlemesi

Aşağıdaki makalede, UK RESMI ve UK NHS şeması örneklerinin UK RESMI ve UK NHS denetimleriyle nasıl eşlendiğini ayrıntılarıyla anlatılmaktadır. Denetimler hakkında daha fazla bilgi için bkz. [UK resmi](https://www.gov.uk/government/publications/government-security-classifications).

Aşağıdaki eşlemeler **UK resmi** ve **UK NHS** denetimlerine göre yapılır. Sağ taraftaki gezinmeyi kullanarak doğrudan belirli bir denetim eşlemesine atlayın. Eşlenmiş denetimlerin birçoğu bir [Azure Policy](../../../policy/overview.md) girişimi ile uygulanır. Tüm girişimi gözden geçirmek için Azure portal **ilkeyi** açın ve **tanımlar** sayfasını seçin. Ardından, **\[Preview @ no__t-2 DENETIM UK resmi ve UK NHS denetimlerini bulun ve seçin ve denetim gereksinimleri yerleşik ilke girişimi ' ni desteklemek için belırlı VM uzantılarını dağıtın** .

> [!IMPORTANT]
> Aşağıdaki her denetim bir veya daha fazla [Azure ilke](../../../policy/overview.md) tanımı ile ilişkilidir. Bu ilkeler, denetimiyle [uyumluluğu değerlendirmenize](../../../policy/how-to/get-compliance-data.md) yardımcı olabilir; Ancak, bir denetim ve bir veya daha fazla ilke arasında genellikle bir 1:1 veya bir eşleşme yoktur. Bu nedenle, Azure Ilkesi ile **uyumlu** , yalnızca ilkelerin kendilerine başvurur; Bu, bir denetimin tüm gereksinimleriyle tamamen uyumlu olduğunuzdan emin değildir. Buna ek olarak, uyumluluk standardı şu anda herhangi bir Azure Ilke tanımı tarafından açıklanmayan denetimler içerir. Bu nedenle, Azure Ilkesinde uyumluluk, genel uyumluluk durumunuzu yalnızca kısmi görünümüdür. Bu uyumluluk şeması örneği için denetimler ve Azure Ilke tanımları arasındaki ilişkilendirmeler zaman içinde değişebilir. Değişiklik geçmişini görüntülemek için [GitHub kayıt geçmişine](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/ukofficial/control-mapping.md)bakın.

## <a name="1-data-in-transit-protection"></a>geçiş koruması 1 veri

Şema, depolama hesaplarına Güvenli olmayan bağlantıları denetleyen [Azure ilke](../../../policy/overview.md) tanımlarını atayarak ve Redis Cache Azure hizmetleri ile bilgi aktarımını güvence altına almanıza yardımcı olur.

- Yalnızca Redis Cache güvenli bağlantılar etkinleştirilmelidir
- Depolama hesaplarına Güvenli aktarım etkinleştirilmelidir

## <a name="23-data-at-rest-protection"></a>2,3 veri bekleyen koruma

Bu şema, belirli bir cryptograph denetimi uygulayan [Azure ilke](../../../policy/overview.md) tanımlarını atayarak ve zayıf şifreleme ayarlarının kullanımını denetleyerek, ilkenizi cryptograph denetimleri kullanarak zorlamanıza yardımcı olur.
Azure kaynaklarınızın en iyi durumda olmayan şifreleme yapılandırmalarının nerede olabileceğini anlamak, kaynakların bilgi güvenliği ilkenize uygun şekilde yapılandırıldığından emin olmak için düzeltici eylemler almanıza yardımcı olabilir. Özellikle, bu şema tarafından atanan ilkeler Data Lake Storage hesapları için şifrelemeyi gerektirir; SQL veritabanlarında saydam veri şifrelemesi gerektir; depolama hesaplarında, SQL veritabanlarında, sanal makine disklerinde ve Otomasyon hesabı değişkenlerinde eksik şifrelemeyi denetleyin; depolama hesaplarına Güvenli olmayan bağlantıları denetleme ve Redis Cache; zayıf sanal makine parola şifrelemesini denetleme; ve şifrelenmemiş Service Fabric iletişimini denetleyin.

- SQL veritabanlarındaki Saydam Veri Şifrelemesi etkinleştirilmelidir
- Disk şifrelemesi sanal makinelere uygulanmalıdır
- Otomasyon hesabı değişkenleri şifrelenmelidir
- Depolama hesaplarına Güvenli aktarım etkinleştirilmelidir
- Service Fabric kümelerinde ClusterProtectionLevel özelliği EncryptAndSign olarak ayarlanmalıdır
- SQL veritabanlarındaki Saydam Veri Şifrelemesi etkinleştirilmelidir
- SQL DB saydam veri şifrelemesini dağıtma
- Data Lake Store hesaplarında şifreleme gerektir
- İzin verilen konumlar ("UK Güney" ve "UK Batı" için sabit olarak kodlanmış)
- Kaynak grupları için izin verilen konumlar ("UK Güney" ve "UK Batı" olarak kodlanmıştır)

## <a name="52-vulnerability-management"></a>5,2 güvenlik açığı yönetimi

Bu şema, eksik Endpoint Protection, eksik sistem güncelleştirmeleri, işletim sistemi güvenlik açıkları, SQL güvenlik açıkları ve sanal olan [Azure ilke](../../../policy/overview.md) tanımlarını atayarak bilgi sistemi güvenlik açıklarını yönetmenize yardımcı olur. Makine güvenlik açıkları. Bu Öngörüler, dağıtılan kaynaklarınızın güvenlik durumu hakkında gerçek zamanlı bilgiler sağlar ve düzeltme eylemlerinin önceliklerini belirlemenize yardımcı olabilir.

- Azure Güvenlik Merkezi 'nde eksik Endpoint Protection izleme
- Sistem güncelleştirmelerinin makinelerinizde yüklü olması gerekir
- Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- SQL veritabanlarınızdaki güvenlik açıkları düzeltilmelidir
- Güvenlik açıkları bir güvenlik açığı değerlendirme çözümü tarafından düzeltilmelidir

## <a name="53-protective-monitoring"></a>5,3 koruyucu Izleme

Bu şema, Kısıtlanmamış erişim, beyaz liste etkinliği ve tehditler üzerinde koruyucu izleme sağlayan [Azure ilke](../../../policy/overview.md) tanımlarını atayarak bilgi sistemi varlıklarını korumanıza yardımcı olur.

- Depolama hesaplarına Kısıtlanmamış ağ erişimini denetleme
- Uyarlamalı uygulama denetimleri sanal makinelerde etkinleştirilmelidir
- SQL Server 'lar üzerinde tehdit algılamayı dağıtma
- Windows Server için varsayılan Microsoft IaaS kötü amaçlı yazılımdan koruma uzantısını dağıt

## <a name="9-secure-user-management--10-identity-and-authentication"></a>9 güvenli Kullanıcı Yönetimi/10 kimliği ve kimlik doğrulaması

Azure, Azure 'daki kaynaklara kimlerin erişebileceğini yönetmenize yardımcı olmak için rol tabanlı erişim denetimi (RBAC) uygular. Azure portal kullanarak, Azure kaynaklarına kimlerin erişebileceğini ve bunların izinlerini gözden geçirebilirsiniz. Bu şema, sahip ve/veya okuma/yazma izinleri ile dış hesapları denetlemek için [Azure ilke](../../../policy/overview.md) tanımları ve çok faktörlü sahip olmayan okuma ve/veya yazma izinlerine sahip olan hesapları denetleyerek erişim haklarını kısıtlayıp denetlemenize yardımcı olur kimlik doğrulama etkinleştirildi.

- MFA, aboneliğinizde sahip izinleri olan hesaplarda etkinleştirilmelidir
- MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir
- MFA, aboneliğinizde okuma izinleri olan hesaplarda etkinleştirilmelidir
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Yazma izinleri olan dış hesapların aboneliğinizden kaldırılması gerekir
- Okuma izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekir

Bu şema, SQL Server ve Service Fabric için Azure Active Directory kimlik doğrulamasının kullanımını denetlemek üzere Azure ilke tanımları atar. Azure Active Directory kimlik doğrulaması kullanmak, veritabanı kullanıcıları ve diğer Microsoft Hizmetleri için Basitleştirilmiş izin yönetimi ve merkezi kimlik yönetimine izin verebilir.

- SQL sunucuları için bir Azure Active Directory Yöneticisi sağlanmalıdır
- Service Fabric kümeler yalnızca istemci kimlik doğrulaması için Azure Active Directory kullanmalıdır

Bu şema Ayrıca, amortisman hesapları ve dış hesaplar dahil olmak üzere incelenmesi gereken denetim hesaplarına Azure Ilke tanımları atar. Gerektiğinde, hesapların oturum açması (veya kaldırılması) engellenebilir ve bu da Azure kaynaklarına erişim haklarını hemen kaldırır. Bu şema, kaldırma için göz önünde bulundurmanız gereken amorti edilmiş hesabı denetlemek için iki Azure Ilke tanımı atar.

- Kullanım dışı bırakılan hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan kullanım dışı hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Yazma izinleri olan dış hesapların aboneliğinizden kaldırılması gerekir

Bu şema Ayrıca, Linux VM parola dosyası izinlerini yanlış ayarlandıklarında uyarı vermek üzere denetleyen bir Azure ilke tanımı atar. Bu tasarım, kimlik doğrulayıcılar güvenliğinin aşılmadığından emin olmak için düzeltici eylem gerçekleştirmenizi sağlar.

- \[Preview @ no__t-1: denetim Linux VM/etc/passwd dosyası izinleri 0644 olarak ayarlanmıştır

Bu şema, en düşük güç ve diğer parola gereksinimlerini zorlayamama Windows VM 'Leri denetleyen Azure Ilke tanımlarını atayarak güçlü parolalar zorlamanıza yardımcı olur. Parola gücü ilkesini ihlal eden VM 'lerin farkında, tüm VM Kullanıcı hesaplarının parolalarının ilkeyle uyumlu olduğundan emin olmak için düzeltici eylemler almanıza yardımcı olur.

- \[Preview @ no__t-1: parola karmaşıklığı ayarı etkin olmayan Windows VM 'Leri denetlemek için gereksinimleri dağıtma
- \[Preview @ no__t-1: maksimum parola yaşı 70 gün olmayan Windows VM 'Leri denetlemek için gereksinimleri dağıtın
- \[Preview @ no__t-1: en az 1 günlük parola yaşı olmayan Windows VM 'Leri denetlemek için gereksinimleri dağıtın
- \[Preview @ no__t-1: en az parola uzunluğu 14 karakter olan Windows sanal makinelerini denetlemek için gereksinimleri dağıtın
- \[Preview @ no__t-1: önceki 24 parolanın yeniden kullanılmasına izin veren Windows VM 'Leri denetlemek için gereksinimleri dağıtın
- \[Preview @ no__t-1: parola karmaşıklığı ayarı etkin olmayan Windows VM 'Leri denetleme
- \[Preview @ no__t-1: maksimum parola yaşı 70 güne sahip olmayan Windows VM 'Leri denetleyin
- \[Preview @ no__t-1: en az 1 günlük parola yaşı olmayan Windows VM 'Leri denetleyin
- \[Preview @ no__t-1: en az parola uzunluğu 14 karakter olan Windows VM 'lerini denetleyin
- \[Preview @ no__t-1: önceki 24 parolanın yeniden kullanılmasına izin veren Windows VM 'lerini denetleyin

Bu şema ayrıca Azure ilke tanımlarını atayarak Azure kaynaklarına erişimi denetlemenize yardımcı olur. Bu ilkeler, kaynaklara daha fazla izin vermeyi sağlayan kaynak türlerinin ve yapılandırmaların kullanımını denetler. Bu ilkeleri ihlal eden kaynakları anlamak, Azure kaynaklarının yetkili kullanıcılarla sınırlı olduğundan emin olmak için düzeltici eylemler almanıza yardımcı olabilir.

- \[Preview @ no__t-1: parolaları olmayan hesaplara sahip Linux sanal makinelerini denetlemek için gereksinimleri dağıtın
- \[Preview @ no__t-1: parola olmadan hesaplardan uzak bağlantılara izin veren Linux VM 'lerini denetlemek için gereksinimleri dağıtın
- \[Preview @ no__t-1: parolası olmayan hesaplara sahip Linux VM 'lerini denetleme
- \[Preview @ no__t-1: parola olmadan hesaplardan uzak bağlantılara izin veren Linux VM 'lerini denetleme
- Depolama hesaplarının yeni Azure Resource Manager kaynaklarına geçirilmesi gerekir
- Sanal makinelerin yeni Azure Resource Manager kaynaklara geçirilmesi gerekir
- Yönetilen diskleri kullanmayan VM 'Leri denetleme

## <a name="11-external-interface-protection"></a>11 harici arabirim koruması

Uygun güvenli Kullanıcı yönetimi için 25 ' ten fazla ilke kullanmaktan başka, bu şema, kısıtlanmamış depolama hesaplarını izleyen bir [Azure ilke](../../../policy/overview.md) tanımı atayarak hizmet arabirimlerini yetkisiz erişimden korumanıza yardımcı olur. Sınırsız erişimi olan depolama hesapları, bilgi sisteminde bulunan bilgilere istenmeden erişime izin verebilir. Bu şema ayrıca sanal makinelerde Uyarlamalı uygulama denetimleri sağlayan bir ilke atar.

- Depolama hesaplarına Kısıtlanmamış ağ erişimini denetleme
- Uyarlamalı uygulama denetimleri sanal makinelerde etkinleştirilmelidir

## <a name="12-secure-service-administration"></a>12 güvenli hizmet yönetimi

Azure, Azure 'daki kaynaklara kimlerin erişebileceğini yönetmenize yardımcı olmak için rol tabanlı erişim denetimi (RBAC) uygular. Azure portal kullanarak, Azure kaynaklarına kimlerin erişebileceğini ve bunların izinlerini gözden geçirebilirsiniz. Bu şema, sahip ve/veya yazma izinlerine sahip olan dış hesapları ve/veya sahip olmayan yazma izinlerini denetlemek için beş [Azure ilke](../../../policy/overview.md) tanımı atayarak ayrıcalıklı erişim haklarını kısıtlayıp denetlemenize yardımcı olur. Multi-Factor Authentication etkinleştirildi.

Bulut hizmetini yönetmek için kullanılan sistemler bu hizmete yüksek ayrıcalıklı erişime sahip olur. Güvenlik denetimlerini atlama ve büyük hacimlere yönelik verileri çalmaya veya düzenlemeye yönelik araçlar da dahil olmak üzere bunun tehlikeye düşmesi önemli ölçüde etkiler. Hizmet sağlayıcısının yöneticileri tarafından işletimsel hizmeti yönetmek için kullanılan yöntemler, hizmetin güvenliğine daha fazla yararlanabilecek herhangi bir risk düzeyini azaltmak için tasarlanmalıdır. Bu ilke uygulanmadıysa, bir saldırgan güvenlik denetimlerini atlama ve büyük hacimlere veri çalmak veya işlemek için bir yol açabilir.

- MFA, aboneliğinizde sahip izinleri olan hesaplarda etkinleştirilmelidir
- MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Yazma izinleri olan dış hesapların aboneliğinizden kaldırılması gerekir

Bu şema, SQL Server ve Service Fabric için Azure Active Directory kimlik doğrulamasının kullanımını denetlemek üzere Azure ilke tanımları atar. Azure Active Directory kimlik doğrulaması kullanmak, veritabanı kullanıcıları ve diğer Microsoft Hizmetleri için Basitleştirilmiş izin yönetimi ve merkezi kimlik yönetimine izin verebilir.

- SQL sunucuları için bir Azure Active Directory Yöneticisi sağlanmalıdır
- Service Fabric kümeler yalnızca istemci kimlik doğrulaması için Azure Active Directory kullanmalıdır

Bu şema Ayrıca, hesap tanımlarını, ve yükseltilmiş izinlere sahip olan amortisman hesapları ve dış hesaplar dahil olmak üzere, incelenmek üzere önceliklendirilebilecek denetim hesaplarına atar. Gerektiğinde, hesapların oturum açması (veya kaldırılması) engellenebilir ve bu da Azure kaynaklarına erişim haklarını hemen kaldırır. Bu şema, kaldırma için göz önünde bulundurmanız gereken amorti edilmiş hesabı denetlemek için iki Azure Ilke tanımı atar.

- Kullanım dışı bırakılan hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan kullanım dışı hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Yazma izinleri olan dış hesapların aboneliğinizden kaldırılması gerekir

Bu şema Ayrıca, Linux VM parola dosyası izinlerini yanlış ayarlandıklarında uyarı vermek üzere denetleyen bir Azure ilke tanımı atar. Bu tasarım, kimlik doğrulayıcılar güvenliğinin aşılmadığından emin olmak için düzeltici eylem gerçekleştirmenizi sağlar.

- \[Preview @ no__t-1: denetim Linux VM/etc/passwd dosyası izinleri 0644 olarak ayarlanmıştır

## <a name="13-audit-information-for-users"></a>Kullanıcılar için 13 denetim bilgileri

Bu şema, Azure kaynaklarında günlük ayarlarını denetleyen [Azure ilke](../../../policy/overview.md) tanımları atanarak sistem olaylarının günlüğe kaydedildiğinden emin olmanıza yardımcı olur. Atanan bir ilke, sanal makinelerin belirli bir Log Analytics çalışma alanına günlük gönderip göndermemişse de denetler.

- SQL Server üzerindeki gelişmiş veri güvenliği ayarlarında denetim etkinleştirilmelidir
- Tanılama ayarını denetle
- SQL Server Level denetim ayarlarını denetleme
- \[Preview @ no__t-1: Linux sanal makineleri için Log Analytics aracısı dağıtma
- \[Preview @ no__t-1: Windows VM 'Leri için Log Analytics aracısı dağıtma
- Sanal ağlar oluşturulduğunda Ağ İzleyicisi dağıt

## <a name="next-steps"></a>Sonraki adımlar

UK RESMI ve UK NHS şemaları denetim eşlemesini gözden geçirdiğinize göre, genel bakış ve bu örneği dağıtma hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [UK resmi ve UK NHS şemaları-genel bakış](./index.md)
> [Birleşik KRALLıK resmi ve UK NHS şemaları-dağıtım adımları](./deploy.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.