---
title: Sık sorulan sorular - Azure ayrılmış HSM | Microsoft Docs
description: Sık sorulan sorular Azure ayrılmış HSM üzerinde farklı konuları kapsayan
services: dedicated-hsm
author: johncdawson
manager: rkarlin
tags: azure-resource-manager
ms.custom: mvc
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 5/8/2019
ms.author: mbaldwin
ms.openlocfilehash: b79ed7ea3113f097f767ad7ff8bdc47b4e4916eb
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70883557"
---
# <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS)

Microsoft Azure ayrılmış HSM hakkında sık sorulan sorulara yanıtlar bulun.

## <a name="the-basics"></a>Temeller

### <a name="q-what-is-a-hardware-security-module-hsm"></a>S: Donanım güvenlik modülü (HSM) nedir?

Bir donanım güvenlik modülü (HSM) korunmasına ve şifreleme anahtarlarını yönetmek için kullanılan fiziksel bir bilgi işlem cihazıdır. Hsm'lerde depolanan anahtarları şifreleme işlemleri için kullanılabilir. Anahtar malzemesi kurcalamaya karşı korumalı, değiştirmeye donanım modülleri güvenli bir şekilde kalır. HSM yalnızca izin kimliği doğrulanmış ve yetkili anahtar kullanma için uygulamaları. Anahtar malzemeleri asla HSM koruma sınırlarından ayrılmaz.

### <a name="q-what-is-the-azure-dedicated-hsm-offering"></a>S: Azure ayrılmış HSM teklifi nedir?

Azure ayrılmış HSM doğrudan bir müşterinin sanal ağa bağlı bir Azure veri merkezlerinde bulunan Hsm'lerde sağlayan bir bulut tabanlı bir hizmettir. Bu HSM'ler, adanmış ağ Gereçleri (Gemalto'nın SafeNet ağ HSM 7 modeli A790) sahiptir. Doğrudan bir müşterilerin özel IP adresi alanına dağıtılan ve Microsoft HSM'ler şifreleme işlevselliği için herhangi bir erişimi yok. Yalnızca müşteri bu cihazlar üzerinde tam yönetimsel ve şifreleme denetime sahiptir. Cihaz yönetimi için müşterilerin sorumluluğundadır ve doğrudan cihazlarından tam etkinlik günlüklerini alabilirsiniz. Ayrılmış Hsm'lerin 140-2 Düzey 3, HIPAA, PCI-DSS ve eIDAS ve diğer birçok gibi FIPS uyumluluk/yasal gereksinimleri karşılamak müşterilerin yardımcı olur.

### <a name="q-what-hardware-is-used-for-dedicated-hsm"></a>S: Ayrılmış HSM donanımı kullanılır?

Microsoft, Azure ayrılmış HSM hizmetini sunmak için Gemalto ile işbirliği yaptı. Kullanılan belirli cihaz [SafeNet Luna ağ HSM 7 modeli A790](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/). Bu cihaz yalnızca FIPS 140-2 Düzey 3'ü doğrulanmış bellenim sağlar, ancak ayrıca düşük gecikme süreli, yüksek performanslı ve yüksek kapasiteli 10 bölümleri aracılığıyla sunar. 

### <a name="q-what-is-an-hsm-used-for"></a>S: HSM ne için kullanılır?

HSM’ler SSL (güvenli yuva katmanı) gibi şifreleme işlevleri, verileri şifreleme, PKI (ortak anahtar altyapısı), DRM (dijital hak yönetimi) ve belgeleri imzalama amacıyla kullanılır.

### <a name="q-how-does-dedicated-hsm-work"></a>S: Ayrılmış HSM nasıl çalışır?

Müşteriler, PowerShell veya komut satırı arabirimi kullanarak belirli bölgelerde HSM'ler sağlayabilirsiniz. Müşteri, HSM bağlı ve bir kez sağladınız hangi sanal ağ Hsm'leri atanmış IP adresleri müşterinin özel IP adres alanı içinde belirtilen alt ağda kullanılabilir olacağını belirtir. Ardından müşteriler HSM'ler Gereci Yönetim için SSH kullanarak bağlanabilirsiniz ve yönetim, HSM istemci bağlantıları kurmak HSM'ler başlatmak, bölümleri oluşturmak, tanımlamak ve gibi bölüm Müdürü, şifreleme Sorumlu Başkan ve şifreleme kullanıcı rolleri atayın. Daha sonra müşteri uygulamalarından şifreleme işlemleri gerçekleştirmesi için HSM istemci araçları/SDK/yazılım sağlanan Gemalto kullanır.

### <a name="q-what-software-is-provided-with-the-dedicated-hsm-service"></a>S: Adanmış HSM hizmetiyle hangi yazılım sağlanır?

Gemalto kez Microsoft tarafından sağlanan HSM cihaz için tüm yazılım sağlar. Yazılım kullanılabilir [Gemalto müşteri desteği portalı](https://supportportal.gemalto.com/csm/). Ayrılmış HSM hizmetini kullanan müşteriler Gemalto desteklemek ve erişim ve ilgili yazılım indirilmesini sağlayan bir müşteri kimliği için kayıtlı olması gereklidir. FIPS 140-2 Düzey 3'ü doğrulanmış üretici yazılımı sürümü 7.0.3 uyumlu 7.2 sürüm desteklenen istemci yazılımıdır. 

### <a name="q-does-azure-dedicated-hsm-offer-password-based-and-ped-based-authentication"></a>S: Azure adanmış HSM, parola tabanlı ve uygulanmış tabanlı kimlik doğrulaması sunuyor mu?

Azure ayrılmış HSM, şu anda yalnızca parola tabanlı kimlik doğrulama ile HSM 'ler sağlar.

### <a name="q-will-azure-dedicated-hsm-host-my-hsms-for-me"></a>S: Azure ayrılmış HSM ana bilgisayarı benim için HSMs mi?

Microsoft, özel HSM hizmeti aracılığıyla yalnızca Gemalto SafeNet Luna ağ HSM 'sini sunar ve müşterinin sağladığı cihazları barındıramaz.

### <a name="q-does-azure-dedicated-hsm-support-payment-pinetf-features"></a>S: Azure ayrılmış HSM destek ödemesi (PIN/ETF) özellikleri mi?

Azure ayrılmış HSM hizmeti, SafeNet Luna ağ HSM 7 (model A790) cihazlarını kullanır. Bu cihazlar, ödeme HSM 'ye özgü işlevselliği (PIN veya ETF gibi) veya sertifikaları desteklemez. Azure ayrılmış HSM hizmeti 'nin gelecekte ödeme HSM 'leri desteklemesini istiyorsanız lütfen Microsoft hesap temsilcinizle ilgili geri bildirimleri geçirin.

### <a name="q-which-azure-regions-is-dedicated-hsm-available-in"></a>S: Hangi Azure bölgeleri için adanmış HSM var?

En geç 2019 itibarıyla, adanmış HSM aşağıda listelenen 14 bölgede kullanılabilir. Daha fazla bölge planlanmaktadır ve Microsoft hesap temsilciniz aracılığıyla tartışılmış olabilir.

* East US
* Doğu ABD 2
* Batı ABD
* Orta Güney ABD
* Güneydoğu Asya
* Doğu Asya
* Kuzey Avrupa
* Batı Avrupa
* Birleşik Krallık Güney
* Birleşik Krallık Batı
* Orta Kanada
* Doğu Kanada
* Avustralya Doğu
* Avustralya Güneydoğu

## <a name="interoperability"></a>Birlikte çalışabilirlik

### <a name="q-how-does-my-application-connect-to-a-dedicated-hsm"></a>S: Uygulamam adanmış bir HSM 'ye nasıl bağlanır?

HSM istemci araçları/SDK/yazılım sağlanan Gemalto uygulamalarınızdan şifreleme işlemleri gerçekleştirmek için kullanın. Yazılım kullanılabilir [Gemalto müşteri desteği portalı](https://supportportal.gemalto.com/csm/). Ayrılmış HSM hizmetini kullanan müşteriler Gemalto desteklemek ve erişim ve ilgili yazılım indirilmesini sağlayan bir müşteri kimliği için kayıtlı olması gereklidir.

### <a name="q-can-an-application-connect-to-dedicated-hsm-from-a-different-vnet-in-or-across-regions"></a>S: Bir uygulama, farklı bir VNET 'ten veya bölgeler arasında ayrılmış HSM 'ye bağlanabilir mi?

Evet, kullanmanız gerekecektir [VNET eşlemesi](../virtual-network/virtual-network-peering-overview.md) sanal ağlar arasında bağlantı kurmak için bir bölge içinde. Bölgeler arası bağlantı için kullanmalısınız [VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md).

### <a name="q-can-i-synchronize-dedicated-hsm-with-on-premises-hsms"></a>S: Adanmış HSM 'yi şirket içi HSM 'ler ile eşitlenebilir miyim?

Evet, şirket içi HSM'ler ayrılmış HSM ile eşitleyebilirsiniz. [Noktadan noktaya VPN veya siteden siteye noktası](../vpn-gateway/vpn-gateway-about-vpngateways.md) bağlantısı, şirket içi ağınız ile bağlantı kurmak için kullanılabilir.

### <a name="q-can-i-encrypt-data-used-by-other-azure-services-using-keys-stored-in-dedicated-hsm"></a>S: Özel HSM 'de depolanan anahtarları kullanarak diğer Azure hizmetleri tarafından kullanılan verileri şifreleyebilir miyim?

Hayır. Azure ayrılmış HSM'ler yalnızca, sanal ağ içinde erişilebilir.

### <a name="q-can-i-import-keys-from-an-existing-on-premises-hsm-to-dedicated-hsm"></a>S: Mevcut bir şirket içi HSM 'den adanmış HSM 'ye anahtar aktarabilir miyim?

Evet, şirket içi Gemalto SafeNet HSM'ler varsa. Birden fazla yöntem vardır. Gemalto HSM belgelerine başvurun.

### <a name="q-what-operating-systems-are-supported-by-dedicated-hsm-client-software"></a>S: Adanmış HSM istemci yazılımı tarafından hangi işletim sistemleri destekleniyor?

* Windows, Linux, Solaris, AIX, HP-UX, FreeBSD
* Sanal VMware, hyperv, Xen, KVM

### <a name="q-how-do-i-configure-my-client-application-to-create-a-high-availability-configuration-with-multiple-partitions-from-multiple-hsms"></a>S: İstemci uygulamamı birden çok HSM 'den birden çok bölümden oluşan yüksek kullanılabilirliğe sahip bir yapılandırma oluşturacak şekilde yapılandırmak Nasıl yaparım??

Yüksek kullanılabilirlik sağlamak için her HSM bölümleri kullanmak için HSM istemci uygulaması yapılandırması'kurmak gerekir. Gemalto HSM istemci yazılımı belgelerine bakın.

### <a name="q-what-authentication-mechanisms-are-supported-by-dedicated-hsm"></a>S: Adanmış HSM tarafından hangi kimlik doğrulama mekanizmaları destekleniyor?

Azure ayrılmış HSM SafeNet ağ HSM 7 cihazları (A790 model) kullanır ve parola tabanlı kimlik doğrulamasını destekler.

### <a name="q-what-sdks-apis-client-software-is-available-to-use-with-dedicated-hsm"></a>S: Adanmış HSM ile kullanılabilecek SDK 'Lar, API 'Ler, istemci yazılımı nedir?

PKCS #11, Java (JCA/JCE), Microsoft CAPI ve CNG, OpenSSL

### <a name="q-can-i-importmigrate-keys-from-luna-56-hsms-to-azure-dedicated-hsms"></a>S: Luna 5/6 HSM 'lerden Azure adanmış HSM 'lere anahtar aktarabilir/geçiş yapabilir miyim?

Evet. Lütfen Gemalto Geçiş Kılavuzu'na bakın. 

## <a name="using-your-hsm"></a>HSM kullanma

### <a name="q-how-do-i-decide-whether-to-use-azure-key-vault-or-azure-dedicated-hsm"></a>S: Azure Key Vault mi yoksa Azure ayrılmış HSM mi kullanacağınızı Nasıl yaparım? belirleyin?

Azure ayrılmış HSM HSM'ler kullanan Azure şirket içi uygulamaları geçirme kuruluşlar için uygun bir seçenektir. Ayrılmış Hsm'lerin çok az değişiklikle bir uygulamayı geçirmek için bir seçenek sunar. Bir Azure VM veya Web uygulamasıyla çalışan uygulama kodu şifreleme işlemleri gerçekleştirilir, ayrılmış HSM kullanabilirler. Genel olarak, yazılım HSM'ler anahtarsız SSL, ADCS (Active Directory Sertifika Hizmetleri), uygulama ağ geçidi veya traffic manager gibi ayrılması, HSM anahtar deposu kullanabilirsiniz gibi destekleyen Iaas (hizmet olarak altyapı) modelleri çalışan verdiği veya benzer PKI araçları, Araçlar/uygulamaları belge imzalama, kod imzalama veya bir EKM (Genişletilebilir anahtar yönetimi) sağlayıcısını kullanarak bir hsm'de ana anahtar ile TDE (saydam veritabanı şifrelemesi) ile yapılandırılmış SQL Server (Iaas) için kullanılır. Azure Key Vault "geliştirilen içinde-buluta" uygulamalar için veya şifreleme PaaS (hizmet olarak platform) tarafından Müşteri verilerinin nerede işlenen rest senaryoları veya Office 365 müşteri anahtarı, Azure Information Protection gibi SaaS (hizmet olarak yazılım) senaryoları için uygundur , Azure Disk şifrelemesi, Azure Data Lake Store şifrelemesi müşteri tarafından yönetilen anahtar, Azure depolama şifreleme ile müşteri ile yönetilen anahtar ve müşteri ile Azure SQL yönetilen anahtar.

### <a name="q-what-usage-scenarios-best-suit-azure-dedicated-hsm"></a>S: Azure ayrılmış HSM 'nin en iyi şekilde hangi kullanım senaryoları vardır?

Azure ayrılmış HSM geçiş senaryoları için çok uygundur. HSM'ler zaten kullanmakta olduğunuz şirket içi uygulamaları azure'a taşıyorsanız anlamına gelir. Bu uygulama için çok az değişiklikle Azure'a geçirmek için düşük uyuşmazlıkları seçeneği sağlar. Azure VM veya Web uygulaması çalışan uygulama kodu şifreleme işlemleri gerçekleştirilir, ayrılmış HSM kullanılabilir. Genel olarak, anahtar deposu ayırmanız HSM, gibi kullanabileceğiniz gibi HSM'ler destekleyen Iaas (hizmet olarak altyapı) modellerinde çalışan yazılımı verdiği:

* Uygulama ağ geçidi veya traffic Manager'a anahtarsız SSL
* ADCS (Active Directory Sertifika Hizmetleri)
* Benzer PKI araçları
* Belge imzalama için kullanılan araçları/uygulamalar
* Kod imzalama
* Bir EKM (Genişletilebilir anahtar yönetimi) sağlayıcısını kullanarak bir hsm'de ana anahtar ile TDE (saydam veritabanı şifrelemesi) ile yapılandırılmış SQL Server (Iaas)

### <a name="q-can-dedicated-hsm-be-used-with-office-365-customer-key-azure-information-protection-azure-data-lake-store-disk-encryption-azure-storage-encryption-azure-sql-tde"></a>S: Ayrılmış HSM; Office 365 Müşteri Anahtarı, Azure Information Protection, Azure Data Lake Store, Disk Şifreleme, Azure Depolama şifrelemesi, Azure SQL TDE ile birlikte kullanılabilir mi?

Hayır. Ayrılmış HSM erişilemiyor vermez böylece doğrudan bir müşterinin özel IP adres alanına diğer Microsoft ya da Azure Hizmetleri tarafından sağlanır.

## <a name="administration-access-and-control"></a>Yönetim ve erişim denetimi

### <a name="q-does-the-customer-get-full-exclusive-control-over-the-hsms-with-dedicated-hsms"></a>S: Müşteri, adanmış HSM 'ler ile HSM 'ler üzerinde tam özel denetim mi edindi?

Evet. Her bir HSM Gereci tam olarak tek bir müşteriye ayrılmış ve başka hiç kimse kez sağlanan yönetimsel denetim sahibi ve Yönetici Parola değiştirildi vardır.

### <a name="q-what-level-of-access-does-microsoft-have-to-my-hsm"></a>S: Microsoft 'un HSM 'me hangi erişim düzeyi vardır?

Microsoft, HSM herhangi bir yönetici veya şifreleme denetime sahip değil. Microsoft izleme düzeyi sıcaklık ve bileşen sistem durumu gibi temel telemetri almak için seri bağlantı noktası bağlantısı aracılığıyla erişimi yok. Bu sistem durumu sorunlarının proaktif bildirim sağlamak Microsoft sağlar. Gerekirse, müşteri bu hesabı devre dışı bırakabilirsiniz.

### <a name="q-what-is-the-tenantadmin-account-microsoft-uses-i-am-used-to-the-admin-user-being-admin-on-safenet-hsms"></a>S: Microsoft 'un kullandığı "TenantAdmin" hesabı nedir, yönetici kullanıcının SafeNet HSMs üzerinde "Yönetici" olması için kullandım mı?

HSM cihazı, normal varsayılan parolasıyla birlikte varsayılan bir yönetici kullanıcıyla birlikte gönderilir. Microsoft, tüm cihazlar müşteriler tarafından sağlanmayı bekleyen bir havuzda olduğu sürece varsayılan parolaların kullanımda olmasını istemedi. Bu, katı güvenlik gereksinimlerimizi karşılamıyor. Bu nedenle, sağlama zamanında atılan güçlü bir parola ayarlayacağız. Ayrıca, sağlama sırasında "TenantAdmin" adlı yönetici rolünde yeni bir Kullanıcı oluşturacağız. Bu Kullanıcı varsayılan parolaya sahiptir ve müşteriler, ilk kez yeni sağlanan cihaza oturum açarken bunu ilk eylem olarak değiştirir. Bu işlem, yüksek derecede güvenlik sağlar ve müşterilerimiz için tek yönetici denetimi taahhüdünü korur. Bir müşteri bu hesabı kullanmayı tercih ediyorsa Yönetici Kullanıcı parolasını sıfırlamak için "TenantAdmin" kullanıcısının kullanılabileceğini not edilmelidir. 

### <a name="q-can-microsoft-or-anyone-at-microsoft-access-keys-in-my-dedicated-hsm"></a>S: Microsoft ya da Microsoft erişim anahtarınızdaki herkes adanmış HSM 'mi olabilir mi?

Hayır. Microsoft, müşteri ayrılmış HSM ayrılan depolanan anahtarları için herhangi bir erişimi yok.

### <a name="q-can-i-upgrade-softwarefirmware-on-hsms-allocated-to-me"></a>S: Bana ayrılan HSM 'lerde yazılım/bellenim yükseltebilir miyim?

En iyi destek almak için Microsoft yazılım / HSM bellenimini yükseltme önerir. Ancak, müşteri belirli özellikleri farklı üretici yazılımı sürümlerini gerekli olduğunda yazılım/bellenimi yükseltmek dahil olmak üzere tam yönetimsel denetime sahip. Değişiklik yapmadan önce bu gibi etkilerini anlaşılması gerekir, örneğin, FIPS etkin durumu doğrulandı. 

### <a name="q-how-do-i-manage-dedicated-hsm"></a>S: Adanmış HSM 'yi yönetmek Nasıl yaparım? mı?

Bunları erişerek, ayrılmış HSM'ler yönetebilirsiniz SSH kullanarak.

### <a name="q-how-do-i-manage-partitions-on-the-dedicated-hsm"></a>S: Ayrılmış HSM 'de bölümleri yönetmek Nasıl yaparım? mı?

Gemalto HSM istemci yazılımı, bölümler ve HSM'ler yönetmek için kullanılır.

### <a name="q-how-do-i-monitor-my-hsm"></a>S: HSM 'mi Nasıl yaparım??

Bir müşteri, HSM etkinlik günlükleri syslog ve SNMP aracılığıyla tam erişime sahip olur. Bir müşteri HSM'ler olayları ve günlükleri almak için bir syslog sunucunuza veya SNMP sunucusu ayarlamak gerekir.

### <a name="q-can-i-get-full-access-log-of-all-hsm-operations-from-dedicated-hsm"></a>S: Adanmış HSM 'den tüm HSM işlemlerinde tam erişim günlüğü alabilir miyim?

Evet. Bir syslog sunucusuna HSM Gereci günlüklerini gönderebilirsiniz

## <a name="high-availability"></a>Yüksek kullanılabilirlik

### <a name="q-is-it-possible-to-configure-high-availability-in-the-same-region-or-across-multiple-regions"></a>S: Aynı bölgede veya birden çok bölgede yüksek kullanılabilirlik yapılandırmak mümkün mü?

Evet. Yüksek kullanılabilirlik yapılandırma ve Kurulum Gemalto tarafından sağlanan HSM istemci yazılımının gerçekleştirilir. Aynı VNET 'ten veya aynı bölgedeki veya bölgedeki diğer VNET 'lerden ya da siteler arası veya noktadan noktaya VPN kullanarak bir VNET 'e bağlı olan şirket içi HSM 'ler aynı yüksek kullanılabilirliğe sahip yapılandırmaya eklenebilir. Bunun, yalnızca anahtar malzemesini eşitlediği ve roller gibi belirli yapılandırma öğeleri olmadığı unutulmamalıdır.

### <a name="q-can-i-add-hsms-from-my-on-premises-network-to-a-high-availability-group-with-azure-dedicated-hsm"></a>S: HSM'ler my şirket içi ağdan Azure ayrılmış HSM ile yüksek kullanılabilirlik grubuna ekleyebilirim?

Evet. Bunlar, SafeNet Luna ağ HSM 7 için yüksek kullanılabilirlik gereksinimleri karşılaması gerekir.

### <a name="q-can-i-add-luna-56-hsms-from-on-premises-networks-to-a-high-availability-group-with-azure-dedicated-hsm"></a>S: 5/6 Luna HSM'ler şirket içi ağlardan Azure ayrılmış HSM ile yüksek kullanılabilirlik grubuna ekleyebilirim?

Hayır.

### <a name="q-how-many-hsms-can-i-add-to-the-same-high-availability-configuration-from-one-single-application"></a>S: Tek bir uygulamadan aynı yüksek kullanılabilirliğe sahip yapılandırmaya kaç tane HSMs ekleyebilirim?

bir HA grubunun 16 üyesi, çok iyi sonuçlarla, tam kısıtlama içeren bir testle sahiptir.

## <a name="support"></a>Destek

### <a name="q-what-is-the-sla-for-dedicated-hsm-service"></a>S: Adanmış HSM hizmeti için SLA nedir?

Adanmış HSM hizmeti için belirli bir çalışma süresi garantisi sağlanmamıştır. Microsoft ağ aygıtına düzeyi erişim sağlayacak ve bu nedenle standart Azure ağ SLA'lar uygulayın.

### <a name="q-how-are-the-hsms-used-in-azure-dedicated-hsm-protected"></a>S: Azure adanmış HSM 'de nasıl korunan HSMs?

Azure veri merkezleri, fiziksel ve yordam kapsamlı güvenlik denetimleri vardır. Buna ek olarak, ayrılmış HSM'ler bir veri merkezinin daha kısıtlı erişim alanında barındırılır. Bu alanlar, ek fiziksel erişim denetimi ve ek güvenlik için video kamera gözetim vardır.

### <a name="q-what-happens-if-there-is-a-security-breach-or-hardware-tampering-event"></a>S: Güvenlik ihlali veya donanım üzerinde değişiklik olayı varsa ne olur?

Ayrılmış HSM hizmetini SafeNet ağ HSM 7 cihazları kullanır. Bu gereçler, fiziksel ve mantıksal değiştirme algılama destekler. HSM'ler otomatik olarak zeroized bir kurcalamaya olay olursa.

### <a name="q-how-do-i-ensure-that-keys-in-my-dedicated-hsms-are-not-lost-due-to-error-or-a-malicious-insider-attack"></a>S: Nasıl yaparım?, özel HSM 'lerde bulunan anahtarların hata veya kötü amaçlı bir Insider saldırısı nedeniyle kaybolmamasını sağlar mi?

HSM'ler normal düzenli yedeklemesi için olağanüstü durum kurtarma gerçekleştirmek için bir şirket içi HSM yedekleme cihazı kullanmak için önerilir. Eşler arası veya siteden siteye VPN bağlantısı için bir HSM yedekleme cihazı bağlı bir şirket içi iş istasyonu kullanmanız gerekecektir.

### <a name="q-how-do-i-get-support-for-dedicated-hsm"></a>S: Adanmış HSM için destek almak Nasıl yaparım? mı?

Destek, hem Microsoft hem de Gemalto tarafından sağlanır.  Donanımla veya ağ erişimiyle ilgili bir sorununuz varsa, Microsoft ile bir destek talebi yükseltin ve HSM yapılandırması, yazılım ve uygulama geliştirme ile ilgili bir sorununuz varsa, lütfen Gemalto ile bir destek isteği yükseltin. Belirlenmeyen bir sorununuz varsa, Microsoft ile bir destek isteği oluşturup daha sonra Gemalto Için gereken bir sorun olabilir. 

### <a name="q-how-do-i-get-the-client-software-documentation-and-access-to-integration-guidance-for-the-safenet-luna-7-hsm"></a>S: Nasıl yaparım? istemci yazılımı, belgeleri ve SafeNet Luna 7 HSM için tümleştirme yönergelerine erişin mi?

Hizmete kaydolduktan sonra, Gemalto müşteri destek portalı 'nda kayda izin veren bir müşteri KIMLIĞI sağlanacaktır. Bu, tüm yazılım ve belgelere erişimi etkinleştirir ve doğrudan Gemalto ile destek isteklerini etkinleştirir.

### <a name="q-if-there-is-a-security-vulnerability-found-and-a-patch-is-released-by-gemalto-who-is-responsible-for-upgradingpatching-osfirmware"></a>S: Bir güvenlik güvenlik açığı varsa ve Gemalto tarafından bir düzeltme eki yayımlanmışsa, işletim sistemi/üretici yazılımı yükseltmekten/bu açıktan geri yüklenebilir?

Microsoft, müşterilere ayrılmış HSM'ler için bağlantı özelliği yok. Müşteriler, yükseltme ve bunların HSM'ler düzeltme eki gerekir.

### <a name="q-what-if-i-need-to-reboot-my-hsm"></a>S: HSM 'mi yeniden başlatmalıyım?

HSM 'nin bir komut satırı yeniden başlatma seçeneği vardır, ancak yeniden başlatma askıda kalma sorunları zaman zaman yaşanıyor ve bu nedenle, cihazın fiziksel olarak yeniden başlatılmasını sağlamak için Microsoft ile bir destek isteği yükseltmenize en güvenli hale getirtiğimiz için önerilir. 

## <a name="cryptography-and-standards"></a>Şifreleme ve standartları

### <a name="q-is-it-safe-to-store-encryption-keys-for-my-most-important-data-in-dedicated-hsm"></a>S: En önemli veriler için şifreleme anahtarlarını ayrılmış HSM 'de depolamak güvenli mi?

Evet, özel HSM kullanın FIPS 140-2 Düzey 3 hsm'leri SafeNet ağ HSM 7 cihazları sağlar. 

### <a name="q-what-cryptographic-keys-and-algorithms-are-supported-by-dedicated-hsm"></a>S: Adanmış HSM tarafından desteklenen şifreleme anahtarları ve algoritmaları nelerdir?

Ayrılmış HSM hizmetini hazırlar SafeNet ağ HSM 7 cihazları. Bunlar aşağıdakiler dahil olmak üzere çok çeşitli şifreleme anahtar türlerini ve algoritmaları destekler: Tam Suite B desteği

* Asimetrik:
  * RSA
  * DSA
  * Diffie-Hellman
  * Eliptik Eğri
  * Şifreleme (ECDSA, ECDH, Ed25519, ECIES) ile kullanıcı tanımlı adlandırılmış ve Brainpool Eğriler, KCDSA
* Simetrik:
  * AES-GCM
  * Üçlü DES
  * DES
  * ARIA, ÇEKİRDEK
  * RC2
  * RC4
  * RC5
  * CAST
  * Karma/Ileti Özeti/HMAC: SHA-1, SHA-2, SM3
  * Anahtar türetme: SP800-108 sayaç modu
  * Anahtar sarmalama: SP800-38F
  * Rastgele sayı oluşturma: FIPS 140-2 onaylı DRBG (SP 800-90 Mrk), BSı DRG. 4 ile uyumlu

### <a name="q-is-dedicated-hsm-fips-140-2-level-3-validated"></a>S: Adanmış HSM FIPS 140-2 düzey 3 ' ü doğruladı mi?

Evet. Ayrılmış HSM hizmetini kullanım FIPS 140-2 Düzey 3 hsm'leri SafeNet ağ HSM 7 cihazları sağlar.

### <a name="q-what-do-i-need-to-do-to-make-sure-i-operate-dedicated-hsm-in-fips-140-2-level-3-validated-mode"></a>S: FIPS 140-2 düzey 3 tarafından doğrulanan modda adanmış HSM 'yi işlediğimde emin olmak için ne yapmam gerekir?

Ayrılmış HSM hizmetini SafeNet Luna ağ HSM 7 cihazları sağlar. Bu gereçler kullanma FIPS 140-2 Düzey 3 hsm'leri. Ayrıca dağıtılan varsayılan yapılandırma, işletim sistemi ve üretici yazılımı FIPS doğrulanmasına sahiptir. FIPS 140-2 Düzey 3 uyumluluk için herhangi bir eylemde bulunmanız gerekmez.

### <a name="q-how-does-a-customer-ensure-that-when-an-hsm-is-deprovisioned-all-the-key-material-is-wiped-out"></a>S: Bir müşteri bir HSM sağlaması kaldırıldığında tüm anahtar malzemesi silindikten olduğunu nasıl emin?

Sağlama kaldırmayı istemeden önce bir müşteri Gemalto sağlanan HSM istemci araçlarını kullanarak HSM zeroized gerekir.

## <a name="performance-and-scale"></a>Performans ve ölçeklendirme

### <a name="q-how-many-cryptographic-operations-are-supported-per-second-with-dedicated-hsm"></a>S: Adanmış HSM ile saniye başına kaç tane şifreleme işlemi destekleniyor?

Ayrılmış HSM hükümlerine SafeNet ağ HSM 7 cihazları (model A790). Bazı işlemler için en yüksek performansı bir özeti aşağıda verilmiştir: 

* RSA-2048: saniye başına 10.000 işlem
* ECC P256: saniye başına 20.000 işlem
* AES-GCM: saniye başına 17.000 işlem

### <a name="q-how-many-partitions-can-be-created-in-dedicated-hsm"></a>S: Adanmış HSM 'de kaç bölüm oluşturulabilirler?

Kullanılan SafeNet Luna HSM 7 modeli A790, hizmet maliyetinde 10 bölüm için bir lisans içerir. Cihazda 100 bölümden oluşan bir sınır bulunur ve bu sınıra kadar bölüm eklemek ek lisanslama maliyetlerine neden olur ve cihazda yeni bir lisans dosyası yüklemesi gerektirir.

### <a name="q-how-many-keys-can-be-supported-in-dedicated-hsm"></a>S: Adanmış HSM 'de kaç anahtar desteklenebilir?

Maksimum anahtar sayısı, kullanılabilir belleğin bir işlevidir. A790 'in kullanımdaki SafeNet Luna 7 modeli, 32 MB belleğe sahiptir. Asimetrik anahtarlar kullanılıyorsa, aşağıdaki sayılar da anahtar çiftlerine uygulanabilir.

* RSA 2048 - 19,000
* ECC P256 - 91,000

Kapasite, şablon anahtar oluşturma ve bölüm sayısı belirli bir anahtar özniteliklere bağlı olarak değişir.
