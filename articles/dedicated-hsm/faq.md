---
title: Sık sorulan sorular - Azure Özel HSM | Microsoft Dokümanlar
description: Azure Özel HSM'de farklı konuları kapsayan sık sorulan sorular
services: dedicated-hsm
author: johncdawson
manager: rkarlin
tags: azure-resource-manager
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/05/2020
ms.author: mbaldwin
ms.openlocfilehash: a0cb7957008308425d91abb3e0f828cc40301736
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064920"
---
# <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS)

Microsoft Azure Özel HSM ile ilgili sık sorulan soruların yanıtlarını bulun.

## <a name="the-basics"></a>Temeller

### <a name="q-what-is-a-hardware-security-module-hsm"></a>S: Donanım güvenlik modülü (HSM) nedir?

Donanım Güvenlik Modülü (HSM), şifreleme anahtarlarını korumak ve yönetmek için kullanılan fiziksel bir bilgi işlem aygıtıdır. HSM'lerde depolanan anahtarlar şifreleme işlemleri için kullanılabilir. Anahtar malzeme kurcalamaya dayanıklı, kurcalamaya açık donanım modüllerinde güvenle kalır. HSM yalnızca kimlik doğrulaması ve yetkili uygulamaların anahtarları kullanmasına izin verir. Anahtar malzeme HSM koruma sınırını asla terk eder.

### <a name="q-what-is-the-azure-dedicated-hsm-offering"></a>S: Azure Özel HSM teklifi nedir?

Azure Özel HSM, azure veri merkezlerinde barındırılan ve müşterinin sanal ağına doğrudan bağlanan HSM'ler sağlayan bulut tabanlı bir hizmettir. Bu HSM'ler özel ağ cihazlarıdır (Gemalto'nun SafeNet Network HSM 7 Model A790). Bunlar doğrudan müşterinin özel IP adresi alanına dağıtılır ve Microsoft'un HSM'lerin şifreleme işlevlerine erişimi yoktur. Yalnızca müşteri bu aygıtlar üzerinde tam yönetimsel ve şifreleme denetimine sahiptir. Müşteriler cihazın yönetiminden sorumludur ve tam etkinlik günlüklerini doğrudan aygıtlarından alabilir. Özel HSM'ler, müşterilerin FIPS 140-2 Düzey 3, HIPAA, PCI-DSS ve eIDAS gibi uyumluluk/mevzuat gerekliliklerini karşılamalarına yardımcı olur.

### <a name="q-what-hardware-is-used-for-dedicated-hsm"></a>S: Özel HSM için hangi donanım kullanılır?

Microsoft, Azure Özel HSM hizmetini sunmak için Gemalto ile ortaklık yaptı. Kullanılan özel cihaz [SafeNet Luna Network HSM 7 Model A790.](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/) Bu cihaz sadece FIPS 140-2 Düzey 3 doğrulanmış firmware sağlar, ama aynı zamanda düşük gecikme, yüksek performans ve 10 bölüm üzerinden yüksek kapasite sunuyor. 

### <a name="q-what-is-an-hsm-used-for"></a>S: HSM ne için kullanılır?

HSM'ler, SSL (güvenli soket katmanı), şifreleme verileri, PKI (ortak anahtar altyapısı), DRM (dijital haklar yönetimi) ve belgeleri imzalama gibi şifreleme işlevleri için kullanılan şifreleme anahtarlarını depolamak için kullanılır.

### <a name="q-how-does-dedicated-hsm-work"></a>S: Özel HSM nasıl çalışır?

Müşteriler PowerShell veya komut satırı arabirimini kullanarak belirli bölgelerde HSM'ler sağlayabilir. Müşteri, HSM'lerin hangi sanal ağa bağlanacağını belirtir ve hsm'ler sağlandıktan sonra müşterinin özel IP adres alanında atanan IP adreslerinde belirlenen alt ağda kullanılabilir olacaktır. Daha sonra müşteriler cihaz yönetimi ve yönetimi için SSH kullanarak HSM'lere bağlanabilir, HSM istemci bağlantılarını kurabilir, HSM'leri açabilir, bölümler oluşturabilir, bölümleyebilir, tanımlayabilir ve bölüm sorumlusu, kripto sorumlusu ve kripto kullanıcısı gibi rolleri atayabilir. Daha sonra müşteri, uygulamalarından şifreleme işlemleri gerçekleştirmek için Gemalto'yu hsm istemci araçları/SDK/yazılım sağladı.

### <a name="q-what-software-is-provided-with-the-dedicated-hsm-service"></a>S: Özel HSM hizmetiyle hangi yazılım sağlanır?

Gemalto, HSM aygıtı için tüm yazılımları bir kez Microsoft tarafından sağlanan sağlar. Yazılım [Gemalto müşteri destek portalı](https://supportportal.gemalto.com/csm/)mevcuttur. Özel HSM hizmetini kullanan müşterilerin Gemalto desteği için kaydolması ve ilgili yazılımlara erişim ve indirme olanağı sağlayan bir Müşteri Kimliğine sahip olması gerekir. Desteklenen istemci yazılımı, FIPS 140-2 Düzey 3 onaylı firmware sürümü 7.0.3 ile uyumlu sürüm 7.2'dir. 

### <a name="q-does-azure-dedicated-hsm-offer-password-based-and-ped-based-authentication"></a>S: Azure Özel HSM Parola tabanlı ve PED tabanlı kimlik doğrulaması sunuyor mu?

Şu anda Azure Özel HSM, HSM'lere yalnızca parola tabanlı kimlik doğrulama sağlar.

### <a name="q-will-azure-dedicated-hsm-host-my-hsms-for-me"></a>S: Azure Özel HSM benim için HSM'lerimi barındıracak mı?

Microsoft, Gemalto SafeNet Luna Network HSM'yi yalnızca Özel HSM hizmeti aracılığıyla sunar ve müşteri tarafından sağlanan hiçbir cihazı barındıramaz.

### <a name="q-does-azure-dedicated-hsm-support-payment-pineft-features"></a>S: Azure Özel HSM ödeme (PIN/EFT) özelliklerini destekliyor mu?

Azure Özel HSM hizmeti, SafeNet Luna Network HSM 7 (Model A790) aygıtlarını kullanır. Bu aygıtlar ödeme HSM'ye özgü işlevselliği (PIN veya EFT gibi) veya sertifikaları desteklemez. Azure Özel HSM hizmetinin gelecekte ödeme HSM'lerini desteklemesini istiyorsanız, geri bildirimi Microsoft Hesap Temsilcinize iletin.

### <a name="q-which-azure-regions-is-dedicated-hsm-available-in"></a>S: Özel HSM hangi Azure bölgelerinde kullanılabilir?

Mart 2019 sonu itibariyle, Özel HSM aşağıda listelenen 14 bölgede mevcuttur. Diğer bölgeler planlanır ve Microsoft Hesap Temsilciniz aracılığıyla tartışılabilir.

* Doğu ABD
* Doğu ABD 2
* Batı ABD
* Orta Güney ABD
* Güneydoğu Asya
* Doğu Asya
* Hindistan Orta
* Hindistan Güney
* Doğu Japonya
* Batı Japonya
* Kuzey Avrupa
* Batı Avrupa
* Güney Birleşik Krallık
* Batı Birleşik Krallık
* Orta Kanada
* Doğu Kanada
* Doğu Avustralya
* Güneydoğu Avustralya

## <a name="interoperability"></a>Birlikte Çalışabilirlik

### <a name="q-how-does-my-application-connect-to-a-dedicated-hsm"></a>S: Uygulamam Özel Bir HSM'ye nasıl bağlanır?

Uygulamalarınızdan şifreleme işlemleri gerçekleştirmek için Gemalto'yu hsm istemci araçları/SDK/yazılım sağladınız. Yazılım [Gemalto müşteri destek portalı](https://supportportal.gemalto.com/csm/)mevcuttur. Özel HSM hizmetini kullanan müşterilerin Gemalto desteği için kaydolması ve ilgili yazılımlara erişim ve indirme olanağı sağlayan bir Müşteri Kimliğine sahip olması gerekir.

### <a name="q-can-an-application-connect-to-dedicated-hsm-from-a-different-vnet-in-or-across-regions"></a>S: Bir uygulama özel HSM'ye bölgeler içinde veya dışında farklı bir VNET'ten bağlanabilir mi?

Evet, sanal ağlar arasında bağlantı kurmak için bir bölge içinde [VNET bakışlarını](../virtual-network/virtual-network-peering-overview.md) kullanmanız gerekir. Bölgeler arası bağlantı için VPN [Ağ Geçidi'ni](../vpn-gateway/vpn-gateway-about-vpngateways.md)kullanmanız gerekir.

### <a name="q-can-i-synchronize-dedicated-hsm-with-on-premises-hsms"></a>S: Özel HSM'yi şirket içi HSM'lerle senkronize edebilir miyim?

Evet, şirket içi HSM'leri Özel HSM ile eşitleyebilirsiniz. [Noktadan noktaya VPN veya noktadan siteye](../vpn-gateway/vpn-gateway-about-vpngateways.md) bağlantı, şirket içi ağınızla bağlantı kurmak için kullanılabilir.

### <a name="q-can-i-encrypt-data-used-by-other-azure-services-using-keys-stored-in-dedicated-hsm"></a>S: Özel HSM'de depolanan anahtarları kullanarak diğer Azure hizmetleri tarafından kullanılan verileri şifreleyebilir miyim?

Hayır. Azure Özel HSM'lere yalnızca sanal ağınızın içinden erişilebilir.

### <a name="q-can-i-import-keys-from-an-existing-on-premises-hsm-to-dedicated-hsm"></a>S: Anahtarları mevcut bir şirket içi HSM'den Özel HSM'ye aktarabilir miyim?

Evet, tesis bünyesinde Gemalto SafeNet HSM'ler varsa. Birden çok yöntem vardır. Gemalto HSM belgelerine bakın.

### <a name="q-what-operating-systems-are-supported-by-dedicated-hsm-client-software"></a>S: Hangi işletim sistemleri Özel HSM istemci yazılımı tarafından desteklenir?

* Windows, Linux, Solaris, AIX, HP-UX, FreeBSD
* Sanal: VMware, hyperv, Xen, KVM

### <a name="q-how-do-i-configure-my-client-application-to-create-a-high-availability-configuration-with-multiple-partitions-from-multiple-hsms"></a>S: Birden çok HSM'den birden fazla bölüm içeren yüksek kullanılabilirlik yapılandırması oluşturmak için istemci uygulamamı nasıl yapılandırıyorum?

Yüksek kullanılabilirliğe sahip olmak için, her HSM'den bölümleri kullanmak için HSM istemci uygulama yapılandırmanızı ayarlamanız gerekir. Gemalto HSM istemci yazılım belgelerine bakın.

### <a name="q-what-authentication-mechanisms-are-supported-by-dedicated-hsm"></a>S: Özel HSM tarafından hangi kimlik doğrulama mekanizmaları desteklenir?

Azure Özel HSM, SafeNet Network HSM 7 cihazlarını (A790 modeli) kullanır ve parola tabanlı kimlik doğrulamasını destekler.

### <a name="q-what-sdks-apis-client-software-is-available-to-use-with-dedicated-hsm"></a>S: Özel HSM ile hangi SDK'lar, API'ler, istemci yazılımları kullanılabilir?

PKCS#11, Java (JCA/JCE), Microsoft CAPI ve CNG, OpenSSL

### <a name="q-can-i-importmigrate-keys-from-luna-56-hsms-to-azure-dedicated-hsms"></a>S: Luna 5/6 HSM'lerden Azure Özel HSM'lere anahtarları alabilir/aktarabilir miyim?

Evet. Gemalto geçiş kılavuzuna bakın. 

## <a name="using-your-hsm"></a>HSM'nizi kullanma

### <a name="q-how-do-i-decide-whether-to-use-azure-key-vault-or-azure-dedicated-hsm"></a>S: Azure Key Vault'u mu yoksa Azure Özel HSM'yi mi kullanacağıma nasıl karar veririm?

Azure Özel HSM, Azure'da HSM kullanan şirket içi uygulamalara geçiş yapan işletmeler için uygun seçimdir. Özel HSM'ler, en az değişiklikle bir uygulamayı geçirme seçeneği sunar. Bir Azure VM veya Web App'te çalışan uygulama kodunda şifreleme işlemleri gerçekleştirilirse, Özel HSM'yi kullanabilirler. Genel olarak, IaaS (hizmet olarak altyapı) modellerinde çalışan shrink-wrapped yazılım, Anahtarsız SSL için Uygulama ağ geçidi veya trafik yöneticisi gibi HSM'leri anahtarsız SSL, ADCS (Active Directory Certificate Services) veya belge imzalama, kod imzalama veya bir EKM (genişletilebilir anahtar yönetimi) sağlayıcısı nı kullanarak HSM'de ana anahtarla Yapılan TDE (şeffaf veritabanı şifrelemesi) ile yapılandırılan SQL Server (IaaS) için kullanılan benzer PKI araçları, araçları/uygulamaları gibi önembelgesi HSM'yi önemli bir mağaza olarak destekleyen Lerm'i kullanabilir. Azure Key Vault, "bulutta doğan" uygulamalar veya müşteri verilerinin PaaS (hizmet olarak platform) veya Office 365 Müşteri Anahtarı, Azure Bilgi Koruması gibi SaaS (hizmet olarak yazılım) senaryoları tarafından işlendiği dinlenme senaryolarında şifreleme için uygundur , Azure Disk Şifreleme, müşteri tarafından yönetilen anahtarla Azure Veri Gölü Deposu şifrelemesi, müşteri yönetilen anahtarla Azure Depolama şifrelemesi ve müşteri yönetilen anahtarla Azure SQL.

### <a name="q-what-usage-scenarios-best-suit-azure-dedicated-hsm"></a>S: Hangi kullanım senaryoları Azure Özel HSM'ye en uygun?

Azure Özel HSM geçiş senaryoları için en uygundur. Bu, şirket içi uygulamaları Zaten HSM kullanmakta olan Azure'a geçirerek gerçeklediğiniz anlamına gelir. Bu, uygulamada en az değişiklikle Azure'a geçiş yapmak için düşük sürtünme seçeneği sağlar. Azure VM veya Web App'te çalışan uygulama kodunda şifreleme işlemleri gerçekleştirilirse, Özel HSM kullanılabilir. Genel olarak, Önemli bir mağaza olarak HSM'leri destekleyen IaaS (hizmet olarak altyapı) modellerinde çalışan küçültülmüş yazılım, aşağıdakiler gibi Adanmış HSM'yi kullanabilir:

* Anahtarsız SSL için uygulama ağ geçidi veya trafik yöneticisi
* ADCS (Aktif Dizin Sertifika Hizmetleri)
* Benzer PKI araçları
* Belge imzalama için kullanılan araçlar/uygulamalar
* Kod imzalama
* SQL Server (IaaS), EKM (genişletilebilir anahtar yönetimi) sağlayıcısı kullanılarak HSM'de ana anahtarla TDE (saydam veritabanı şifrelemesi) ile yapılandırıldı

### <a name="q-can-dedicated-hsm-be-used-with-office-365-customer-key-azure-information-protection-azure-data-lake-store-disk-encryption-azure-storage-encryption-azure-sql-tde"></a>S: Özel HSM, Office 365 Müşteri Anahtarı, Azure Bilgi Koruması, Azure Veri Gölü Deposu, Disk Şifreleme, Azure Depolama şifrelemesi, Azure SQL TDE ile kullanılabilir mi?

Hayır. Özel HSM, diğer Azure veya Microsoft hizmetleri tarafından erişilememesi için doğrudan müşterinin özel IP Adresi alanına verilir.

## <a name="administration-access-and-control"></a>Yönetim, erişim ve kontrol

### <a name="q-does-the-customer-get-full-exclusive-control-over-the-hsms-with-dedicated-hsms"></a>S: Müşteri Özel HSM'ler ile HSM'ler üzerinde tam özel kontrole sahip mi?

Evet. Her HSM cihazı tamamen tek bir müşteriye adamıştır ve bir kez verilen ve yönetici parolası değiştirildikten sonra başka hiç kimse yönetim denetimine sahip değildir.

### <a name="q-what-level-of-access-does-microsoft-have-to-my-hsm"></a>S: Microsoft'un HSM'me ne kadar erişim düzeyi var?

Microsoft'un HSM üzerinde herhangi bir yönetimsel veya şifreleme denetimi yoktur. Microsoft sıcaklık ve bileşen durumu gibi temel telemetri almak için seri bağlantı noktası bağlantısı üzerinden monitör düzeyi erişimi var. Bu, Microsoft'un sistem durumu sorunları hakkında proaktif bildirim sağlamasına olanak tanır. Gerekirse, müşteri bu hesabı devre dışı kılabilir.

### <a name="q-what-is-the-tenantadmin-account-microsoft-uses-i-am-used-to-the-admin-user-being-admin-on-safenet-hsms"></a>S: Microsoft'un kullandığı "tenantadmin" hesabı nedir, yönetici kullanıcının SafeNet HSM'lerde "yönetici" olmasına alışkınım?

HSM aygıtı, her zamanki varsayılan parolasıyla varsayılan bir yönetici kullanıcısıyla birlikte iletilir. Microsoft, herhangi bir aygıt müşteriler tarafından sağlanmayı bekleyen bir havuzdayken varsayılan parolaların kullanılmasını istemedi. Bu bizim katı güvenlik gerekliliklerini karşılamaz. Bu nedenle, sağlama zamanında atılan güçlü bir parola belirleriz. Ayrıca, sağlama zamanında biz "tenantadmin" olarak adlandırılan yönetici rolü yeni bir kullanıcı oluşturun. Bu kullanıcı varsayılan parolaya sahiptir ve müşteriler bunu yeni sağlanan aygıta ilk günlüğe giriş yaptığında ilk eylem olarak değiştirir. Bu süreç yüksek güvenlik dereceleri sağlar ve müşterilerimiz için tek yönetim kontrolü sözümüzü korur. Bir müşteri bu hesabı kullanmayı tercih ederse, "kiracı yönetici" kullanıcısının yönetici kullanıcı parolasını sıfırlamak için kullanılabileceğini belirtmek gerekir. 

### <a name="q-can-microsoft-or-anyone-at-microsoft-access-keys-in-my-dedicated-hsm"></a>S: Microsoft veya Microsoft'taki herhangi biri Özel HSM'mdeki microsoft erişim anahtarlarına erişebilir mi?

Hayır. Microsoft'un müşteriye tahsis edilen Özel HSM'de depolanan anahtarlara erişimi yoktur.

### <a name="q-can-i-upgrade-softwarefirmware-on-hsms-allocated-to-me"></a>S: Bana tahsis edilen HSM'lerde yazılım/firmware yükseltebilir miyim?

En iyi desteği almak için Microsoft, HSM'deki yazılım/yazılım yazılımlarını yükseltmemenizi şiddetle önerir. Ancak, müşteri, farklı firmware sürümlerinden belirli özellikler gerekiyorsa, yazılım/firmware yükseltme de dahil olmak üzere tam yönetim kontrolüne sahiptir. Değişiklik yapmadan önce, etkileri, örneğin FIPS doğrulanmış durumu etkileyebileceğinden anlaşılmalıdır. 

### <a name="q-how-do-i-manage-dedicated-hsm"></a>S: Özel HSM'yi nasıl yönetirim?

Özel HSM'lere SSH kullanarak erişerek yönetebilirsiniz.

### <a name="q-how-do-i-manage-partitions-on-the-dedicated-hsm"></a>S: Özel HSM'deki bölümleri nasıl yönetirim?

Gemalto HSM istemci yazılımı HSM'leri ve bölümleri yönetmek için kullanılır.

### <a name="q-how-do-i-monitor-my-hsm"></a>S: HSM'mi nasıl izleyebilirim?

Bir müşteri syslog ve SNMP üzerinden HSM etkinlik günlüklerine tam erişime sahiptir. Bir müşterinin HSM'lerden günlükleri veya olayları almak için bir syslog sunucusu veya SNMP sunucusu ayarlaması gerekir.

### <a name="q-can-i-get-full-access-log-of-all-hsm-operations-from-dedicated-hsm"></a>S: Özel HSM'den tüm HSM işlemlerinin tam erişim kaydını alabilir miyim?

Evet. HSM cihazından günlükleri bir syslog sunucusuna gönderebilirsiniz

## <a name="high-availability"></a>Yüksek kullanılabilirlik

### <a name="q-is-it-possible-to-configure-high-availability-in-the-same-region-or-across-multiple-regions"></a>S: Aynı bölgede veya birden çok bölgede yüksek kullanılabilirlik yapılandırmak mümkün mü?

Evet. Gemalto tarafından sağlanan HSM istemci yazılımında yüksek kullanılabilirlik yapılandırması ve kurulum gerçekleştirilir. Aynı bölgedeki veya bölgeler arasında aynı VNET'lerden veya diğer VNET'lerden veya siteden siteye veya noktadan noktaya VPN kullanarak VNET'e bağlı şirket içi HSM'lerden gelen HSM'ler aynı yüksek kullanılabilirlik yapılandırmasına eklenebilir. Bunun yalnızca önemli malzemeyi eşitlediğini ve roller gibi belirli yapılandırma öğelerini eşitlemediğini belirtmek gerekir.

### <a name="q-can-i-add-hsms-from-my-on-premises-network-to-a-high-availability-group-with-azure-dedicated-hsm"></a>S: Şirket içi ağımdan HSM'leri Azure Özel HSM ile yüksek kullanılabilirlik grubuna ekleyebilir miyim?

Evet. SafeNet Luna Network HSM 7 için yüksek kullanılabilirlik gereksinimlerini karşılamaları gerekir.

### <a name="q-can-i-add-luna-56-hsms-from-on-premises-networks-to-a-high-availability-group-with-azure-dedicated-hsm"></a>S: Luna 5/6 HSM'leri şirket içi ağlardan Azure Özel HSM ile yüksek kullanılabilirlik grubuna ekleyebilir miyim?

Hayır.

### <a name="q-how-many-hsms-can-i-add-to-the-same-high-availability-configuration-from-one-single-application"></a>S: Tek bir uygulamadan aynı yüksek kullanılabilirlik yapılandırmasına kaç HSM ekleyebilirim?

Bir HA grubunun 16 üyesi, mükemmel sonuçlar la birlikte tam gaz testine sahip.

## <a name="support"></a>Destek

### <a name="q-what-is-the-sla-for-dedicated-hsm-service"></a>S: Özel HSM hizmeti için SLA nedir?

Özel HSM hizmeti için belirli bir çalışma süresi garantisi sağlanmaz. Microsoft, aygıta ağ düzeyinde erişim sağlar ve bu nedenle standart Azure ağ SLA'ları geçerlidir.

### <a name="q-how-are-the-hsms-used-in-azure-dedicated-hsm-protected"></a>S: Azure Özel HSM'de HSM'ler nasıl korunur?

Azure veri merkezlerikapsamlı fiziksel ve yordamsal güvenlik denetimleri ne sahiptir. Buna ek olarak Özel HSM'ler veri merkezinin daha sınırlı erişim alanında barındırılır. Bu alanlarda ek güvenlik için ek fiziksel erişim kontrolleri ve video kamera gözetimi var.

### <a name="q-what-happens-if-there-is-a-security-breach-or-hardware-tampering-event"></a>S: Bir güvenlik ihlali veya donanım tahrifatı olayı olduğunda ne olur?

Özel HSM hizmeti SafeNet Network HSM 7 cihazlarını kullanır. Bu cihazlar fiziksel ve mantıksal kurcalama algılamayı destekler. Bir kurcalama olayı olursa HSM'ler otomatik olarak sıfırlanır.

### <a name="q-how-do-i-ensure-that-keys-in-my-dedicated-hsms-are-not-lost-due-to-error-or-a-malicious-insider-attack"></a>S: Özel HSM'lerimdeki anahtarların hata veya kötü amaçlı bir içeriden saldırı nedeniyle kaybolmadığından nasıl emin olabilirim?

Olağanüstü durum kurtarma için HSM'lerin düzenli periyodik yedeklemesini gerçekleştirmek için şirket içi HSM yedekleme aygıtı kullanılması önerilir. Bir HSM yedekleme aygıtına bağlı şirket içi iş istasyonuna eşler arası veya siteden siteye VPN bağlantısı kullanmanız gerekir.

### <a name="q-how-do-i-get-support-for-dedicated-hsm"></a>S: Özel HSM için nasıl destek alabilirim?

Destek hem Microsoft hem de Gemalto tarafından sağlanır.  Donanım veya ağ erişimiyle ilgili bir sorununuz varsa, Microsoft'a bir destek isteği getirin ve HSM yapılandırması, yazılım ve uygulama geliştirme yle ilgili bir sorununuz varsa Gemalto ile bir destek isteği yükseltin. Belirsiz bir sorununuzun varsa, Microsoft ile bir destek isteği getirin ve gemalto gerektiği gibi meşgul olabilir. 

### <a name="q-how-do-i-get-the-client-software-documentation-and-access-to-integration-guidance-for-the-safenet-luna-7-hsm"></a>S: SafeNet Luna 7 HSM için istemci yazılımını, dokümantasyonunu ve entegrasyon kılavuzuna erişimi nasıl edebilirim?

Hizmete kaydolduktan sonra, Gemalto müşteri destek portalına kayıt yapılmasına olanak tanıyan bir Gemalto Müşteri Kimliği sağlanacaktır. Bu, tüm yazılım ve belgelere erişimin yanı sıra destek isteklerini doğrudan Gemalto ile etkinleştirme olanağı sağlayacaktır.

### <a name="q-if-there-is-a-security-vulnerability-found-and-a-patch-is-released-by-gemalto-who-is-responsible-for-upgradingpatching-osfirmware"></a>S: Bir güvenlik açığı bulunursa ve Gemalto tarafından bir yama serbest bırakılırsa, işletim sistemi/Firmware'in yükseltilmesi/yamalanmasından kim sorumludur?

Microsoft'un müşterilere tahsis edilen HSM'lere bağlanma yeteneği yoktur. Müşteriler HSM'lerini yükseltmeli ve yamalıdır.

### <a name="q-what-if-i-need-to-reboot-my-hsm"></a>S: HSM'mi yeniden başlatmam gerekirse ne olur?

HSM'nin komut satırı yeniden başlatma seçeneği vardır, ancak, aralıklı olarak yeniden başlatma sorunları yaşıyoruz ve bu nedenle aygıtın fiziksel olarak yeniden başlatılması için Microsoft ile bir destek isteği yükseltmeniz en güvenli yeniden başlatma önerilir. 

## <a name="cryptography-and-standards"></a>Kriptografi ve standartlar

### <a name="q-is-it-safe-to-store-encryption-keys-for-my-most-important-data-in-dedicated-hsm"></a>S: En önemli verilerim için şifreleme anahtarlarını Özel HSM'de depolamak güvenli midir?

Evet, ÖZEL HSM hükümleri FIPS 140-2 Düzey 3 onaylı HSM kullanan SafeNet Network HSM 7 cihazları. 

### <a name="q-what-cryptographic-keys-and-algorithms-are-supported-by-dedicated-hsm"></a>S: Hangi şifreleme anahtarları ve algoritmaları Özel HSM tarafından desteklenir?

Özel HSM servis hükümleri SafeNet Network HSM 7 cihazları. Bunlar dahil olmak üzere çok çeşitli şifreleme anahtar türlerini ve algoritmalarını destekler: Full Suite B desteği

* Asimetrik:
  * RSA
  * Dsa
  * Diffie-Hellman
  * Eliptik Eğri
  * Kriptografi (ECDSA, ECDH, Ed25519, ECIES) adlı, kullanıcı tanımlı ve Brainpool eğrileri ile, KCDSA
* Simetrik:
  * AES-GCM
  * Üçlü DES
  * DES
  * ARIA, TOHUM
  * RC2
  * RC4
  * RC5
  * CAST
  * Karma/Mesaj Özeti/HMAC: SHA-1, SHA-2, SM3
  * Anahtar Türetme: SP800-108 Sayaç Modu
  * Anahtar Sarma: SP800-38F
  * Rastgele Sayı Üretimi: FIPS 140-2 onaylı DRBG (SP 800-90 TO modu), BSI DRG.4 ile uyumlu

### <a name="q-is-dedicated-hsm-fips-140-2-level-3-validated"></a>S: Özel HSM FIPS 140-2 Düzey 3 doğrulandı mı?

Evet. Özel HSM hizmet hükümleri FIPS 140-2 Düzey 3 onaylı HSM kullanan SafeNet Network HSM 7 cihazları.

### <a name="q-what-do-i-need-to-do-to-make-sure-i-operate-dedicated-hsm-in-fips-140-2-level-3-validated-mode"></a>S: FIPS 140-2 Düzey 3 doğrulanmış modda Özel HSM çalıştırdığımdan emin olmak için ne yapmalıyım?

Özel HSM hizmet hükümleri SafeNet Luna Network HSM 7 aletleri. Bu cihazlar FIPS 140-2 Düzey 3 onaylı HSM'leri kullanır. Varsayılan dağıtılan yapılandırma, işletim sistemi ve firmware de FIPS doğrulanır. FIPS 140-2 Düzey 3 uyumluluğu için herhangi bir işlem yapmanız gerekmez.

### <a name="q-how-does-a-customer-ensure-that-when-an-hsm-is-deprovisioned-all-the-key-material-is-wiped-out"></a>S: Bir müşteri, bir HSM'nin tüm anahtar malzemenin yok edilmesini nasıl sağlar?

Deprovisioning istemeden önce, bir müşteri Gemalto sağlanan HSM istemci araçlarını kullanarak HSM sıfırlanmış olmalıdır.

## <a name="performance-and-scale"></a>Performans ve ölçeklendirme

### <a name="q-how-many-cryptographic-operations-are-supported-per-second-with-dedicated-hsm"></a>S: Özel HSM ile saniyede kaç şifreleme işlemi desteklenir?

Özel HSM hükümleri SafeNet Network HSM 7 aletleri (Model A790). Aşağıda, bazı işlemler için maksimum performansın bir özeti verem: 

* RSA-2048: Saniyede 10.000 işlem
* ECC P256: Saniyede 20.000 işlem
* AES-GCM: Saniyede 17.000 işlem

### <a name="q-how-many-partitions-can-be-created-in-dedicated-hsm"></a>S: Özel HSM'de kaç bölüm oluşturulabilir?

SafeNet Luna HSM 7 modeli A790 kullanılan hizmet maliyeti 10 bölüm için bir lisans içerir. Aygıtın 100 bölüm sınırı vardır ve bu sınıra kadar bölümler eklemek ek lisans maliyetlerine neden olur ve aygıta yeni bir lisans dosyası nın yüklenmesini gerektirir.

### <a name="q-how-many-keys-can-be-supported-in-dedicated-hsm"></a>S: Özel HSM'de kaç anahtar desteklenebilir?

En fazla anahtar sayısı, kullanılabilir belleğin bir işlevidir. SafeNet Luna 7 modeli A790 32MB belleğe sahiptir. Aşağıdaki sayılar, asimetrik tuşlar kullanıyorsanız anahtar çiftleri için de geçerlidir.

* RSA-2048 - 19.000
* ECC-P256 - 91.000

Kapasite, anahtar oluşturma şablonunda ve bölüm sayısına göre belirli anahtar özniteliklerine bağlı olarak değişir.
