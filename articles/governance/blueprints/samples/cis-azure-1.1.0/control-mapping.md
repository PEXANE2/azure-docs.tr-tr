---
title: BDT Microsoft Azure Vakıflar Kıyaslama planı örnek denetimleri
description: BDT Microsoft Azure Temelleri Kıyaslama planı örneğinin Azure Politikası için öneri eşlemesi.
ms.date: 11/04/2019
ms.topic: sample
ms.openlocfilehash: 34d38f34dcd4233706f9b4578bc2dc2a644e4c2c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74707424"
---
# <a name="recommendation-mapping-of-the-cis-microsoft-azure-foundations-benchmark-blueprint-sample"></a>BDT Microsoft Azure Vakıflar Kıyaslama planı örneğinin öneri eşlemesi

Aşağıdaki makalede, Azure Blueprints BDT Microsoft Azure Temelleri Kıyaslama planı örnek haritaların Nasıl BDT Microsoft Azure Temelleri Kıyaslama önerilerine göre olduğu ayrıntılı olarak açıklanmaktadır. Öneriler hakkında daha fazla bilgi için [BDT Microsoft Azure Temelleri Kıyaslama'ya](https://www.cisecurity.org/benchmark/azure/)bakın.

Aşağıdaki eşlemeler **BDT Microsoft Azure Vakıflar Benchmark v1.1.0** önerileri içindir. Doğrudan belirli bir öneri eşleme sine atlamak için sağdaki gezinmeyi kullanın.
Eşlenen önerilerin çoğu bir Azure [İlkesi](../../../policy/overview.md) girişimiyle uygulanır. Girişimin tamamını incelemek için Azure portalında **İlke'yi** açın ve **Tanımlar** sayfasını seçin. Ardından, ** \[\] Önizleme Denetimi BDT Microsoft Azure Foundations Benchmark v1.1.0 önerilerini** bulun ve seçin ve yerleşik denetim gereksinimlerini desteklemek için belirli VM Uzantılarıdağıtın.

> [!IMPORTANT]
> Aşağıdaki her denetim bir veya daha fazla [Azure İlkesi](../../../policy/overview.md) tanımıyla ilişkilidir. Bu ilkeler, denetime [uygunluğu değerlendirmenize](../../../policy/how-to/get-compliance-data.md) yardımcı olabilir; ancak, genellikle bir denetim ve bir veya daha fazla ilke arasında 1:1 veya tam bir eşleşme yoktur. Bu nedenle, Azure İlkesinde **Uyumlu olmak** yalnızca ilkelerin kendileri için başvurur; bu, bir denetimin tüm gereksinimlerine tam olarak uymanızı sağlamaz. Ayrıca, uyumluluk standardı şu anda herhangi bir Azure İlkesi tanımı tarafından ele alınmayan denetimleri içerir. Bu nedenle, Azure İlkesi'ndeki uyumluluk, genel uyumluluk durumunuza yalnızca kısmi bir görünümdür. Bu uyumluluk planı örneği için denetimler ve Azure İlkesi tanımları arasındaki ilişkilendirmeler zaman içinde değişebilir. Değişiklik geçmişini görüntülemek için [GitHub Commit History'ye](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/cis-azure-1.1.0/control-mapping.md)bakın.

## <a name="11-ensure-that-multi-factor-authentication-is-enabled-for-all-privileged-users"></a>1.1 Çok faktörlü kimlik doğrulamanın tüm ayrıcalıklı kullanıcılar için etkinleştirildiğinden emin olun

Bu plan, ayrıcalıklı Azure Etkin Dizin hesaplarında çok faktörlü kimlik doğrulamaetkinleştirilemiyorsa izlemenize yardımcı olan [Azure İlkesi](../../../policy/overview.md) tanımları atar.

- MFA, aboneliğinizde sahip izni olan hesaplarda etkinleştirilmelidir
- MFA, aboneliğinizde yazma izinleri olan hesaplarda etkinleştirilmelidir

## <a name="12-ensure-that-multi-factor-authentication-is-enabled-for-all-non-privileged-users"></a>1.2 Çok faktörlü kimlik doğrulamanın tüm ayrıcalıklı olmayan kullanıcılar için etkinleştirildiğinden emin olun

Bu plan, ayrıcalıklı olmayan Azure Etkin Dizin hesaplarında çok faktörlü kimlik doğrulamanın etkinleştirilemediğini izlemenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- MFA, aboneliğinizde okuma izinleri olan hesaplarda etkinleştirilmelidir

## <a name="13-ensure-that-there-are-no-guest-users"></a>1.3 Konuk kullanıcı olmadığından emin olun

Bu plan, kaldırılması gereken konuk hesaplarını izlemenize yardımcı olan [Azure İlkesi](../../../policy/overview.md) tanımları atar.

- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Okuma izinleri olan harici hesaplar aboneliğinizden kaldırılmalıdır
- Yazma izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır

## <a name="123-ensure-that-no-custom-subscription-owner-roles-are-created"></a>1.23 Özel abonelik sahibi rollerinin oluşturulmadığından emin olun

Bu plan, kaldırılması gereken özel abonelik sahibi rollerini izlemenize yardımcı olan [Azure İlkesi](../../../policy/overview.md) tanımları atar.

- Özel abonelik sahibi rolleri olmamalıdır

## <a name="21-ensure-that-standard-pricing-tier-is-selected"></a>2.1 Standart fiyatlandırma katmanının seçildiğinden emin olun

Bu plan, Güvenlik Merkezi standart katmanının etkin olmadığı ağları ve sanal makineleri izlemenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

 - Güvenlik Merkezi standart fiyatlandırma katmanı seçilmelidir

## <a name="22-ensure-that-automatic-provisioning-of-monitoring-agent-is-set-to-on"></a>2.2 'İzleme aracısının otomatik olarak sağlanması' 'A)'ya ayarlandığından emin olun

Bu plan, Log Analytics aracısının otomatik olarak sağlanmasının etkin olmasını sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Log Analytics izleme aracısının otomatik olarak sağlanması aboneliğinizde etkinleştirilmelidir

## <a name="23-ensure-asc-default-policy-setting-monitor-system-updates-is-not-disabled"></a>2.3 ASC Varsayılan ilke ayarı "Monitör Sistem Güncelleştirmeleri" "Devre Dışı" olmadığından emin olun

Bu plan, sistem güncelleştirmelerinin sanal makinelere yüklenmesini sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Makinelerinize sistem güncelleştirmeleri yüklenmelidir

## <a name="24-ensure-asc-default-policy-setting-monitor-os-vulnerabilities-is-not-disabled"></a>2.4 ASC Varsayılan ilke ayarı "İşletim Sistemi Güvenlik Açıklarını İzle" nin "Devre Dışı" olmadığından emin olun

Bu plan, düzeltilmemiş sanal makine güvenlik açıklarını izlemenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir

## <a name="25-ensure-asc-default-policy-setting-monitor-endpoint-protection-is-not-disabled"></a>2.5 ASC Varsayılan ilke ayarı "Monitor Endpoint Protection" "Devre Dışı" değildir emin olun

Bu plan, sanal makinelerde uç nokta korumasının etkin olmasını sağlamaya yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Azure Güvenlik Merkezi'nde eksik Uç Nokta Koruması'nı izleme

## <a name="26-ensure-asc-default-policy-setting-monitor-disk-encryption-is-not-disabled"></a>2.6 ASC Varsayılan ilke ayarı "Monitör Disk Şifreleme" "Devre Dışı" olmadığından emin olun

Bu plan, sanal makine disklerinin şifrelenmenizi sağlamaya yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Disk şifreleme sanal makinelerde uygulanmalıdır

## <a name="27-ensure-asc-default-policy-setting-monitor-network-security-groups-is-not-disabled"></a>2.7 ASC Varsayılan ilke ayarı "Ağ Güvenlik Gruplarını İzle" nin "Devre Dışı Bırakılmadığından" emin olun

Bu plan, Internet'e bakan sanal makineleri korumanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Internet'in sanal makinelerle karşı karşıya olması için Ağ Güvenliği Grubu Kuralları sertleştirilmiş olmalıdır

## <a name="28-ensure-asc-default-policy-setting-monitor-web-application-firewall-is-not-disabled"></a>2.8 ASC Varsayılan ilke ayarı "Web Uygulama Güvenlik Duvarını İzle" nin "Devre Dışı Bırakılmadığından" emin olun

Bu plan, web uygulamaları çalıştıran sanal makineleri korumanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- IaaS'deki web uygulamaları için NSGs kuralları sertleştirilmiş olmalıdır

## <a name="29-ensure-asc-default-policy-setting-enable-next-generation-firewallngfw-monitoring-is-not-disabled"></a>2.9 ASC Varsayılan ilke ayarı "Enable Next Generation Firewall(NGFW) İzleme" "Devre Dışı" değildir emin olun

Bu plan, erişimi kısıtlayarak alt ağların ve sanal makinelerin tehditlere karşı korunmasına yardımcı olan [Azure İlkesi](../../../policy/overview.md) tanımları atar. Bu BDT Microsoft Azure Temelleri Kıyaslama önerisi tarafından başvurulan Güvenlik Merkezi ilkesi iki yeni öneriyle değiştirilmiştir. Aşağıda başvurulan ilkeler yeni önerileri ele adabEder.

- Alt ağlar bir Ağ Güvenlik Grubu ile ilişkilendirilmelidir
- Sanal makineler bir Ağ Güvenlik Grubu ile ilişkilendirilmelidir

## <a name="210-ensure-asc-default-policy-setting-monitor-vulnerability-assessment-is-not-disabled"></a>2.10 ASC Varsayılan ilke ayarı "Güvenlik Açığı Değerlendirmesini İzle" nin "Devre Dışı" olmadığından emin olun

Bu plan, güvenlik açıklarının algılanıp düzeltilmesini sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Güvenlik açıkları, Güvenlik Açığı Değerlendirmesi çözümü yle düzeltilmelidir

## <a name="211-ensure-asc-default-policy-setting-monitor-storage-blob-encryption-is-not-disabled"></a>2.11 ASC Varsayılan ilke ayarı "Monitör Depolama Blob Şifreleme" "Devre Dışı" değildir emin olun

Azure Depolama şifrelemesi, tüm yeni ve mevcut depolama hesapları için etkinleştirilir ve devre dışı bırakılamaz. (Bu varsayılan bir Azure özelliğidir; ilke ataması yoktur.)

## <a name="212-ensure-asc-default-policy-setting-monitor-jit-network-access-is-not-disabled"></a>2.12 ASC Varsayılan ilke ayarı "Monitör JIT Network Access" "Devre Dışı" değildir emin olun

Bu plan, sanal makinelere erişimi denetlemenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Sanal makinelere anlık ağ erişim denetimi uygulanmalıdır

## <a name="213-ensure-asc-default-policy-setting-monitor-adaptive-application-whitelisting-is-not-disabled"></a>2.13 ASC Varsayılan ilke ayarı "Monitör Adaptif Uygulama Beyaz Liste" "Devre Dışı" değildir emin olun

Bu plan, uyarlanabilir Uygulama Denetimlerinin sanal makinelerde etkinleştirilmiş olmasını sağlamaya yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Uyarlanabilir Uygulama Denetimleri sanal makinelerde etkinleştirilmelidir

## <a name="214-ensure-asc-default-policy-setting-monitor-sql-auditing-is-not-disabled"></a>2.14 ASC Varsayılan ilke ayarı "Sql Denetimini İzle" nin "Devre Dışı" olmadığından emin olun

Bu plan, SQL sunucu denetiminin etkin olmasını sağlamaya yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- SQL Server'da gelişmiş veri güvenliği ayarlarında denetim etkinleştirilmelidir

## <a name="215-ensure-asc-default-policy-setting-monitor-sql-encryption-is-not-disabled"></a>2.15 ASC Varsayılan ilke ayarı "Monitör SQL Şifreleme" "Devre Dışı" değildir emin olun

Bu plan, SQL veritabanlarında Saydam Veri Şifrelemesi'nin etkin olmasını sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- SQL veritabanlarında Saydam Veri Şifrelemesi etkinleştirilmelidir

## <a name="216-ensure-that-security-contact-emails-is-set"></a>2.16 'Güvenlik iletişim e-postalarının' ayarlandığından emin olun

Bu plan, güvenlik bildirimlerinin düzgün şekilde etkinleştirilmesini sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar

- Aboneliğiniz için bir güvenlik kişisi e-posta adresi sağlanmalıdır

## <a name="217-ensure-that-security-contact-phone-number-is-set"></a>2.17 Güvenlik bağlantısı 'Telefon numarası' ayarlandığından emin olun

Bu plan, güvenlik bildirimlerinin düzgün şekilde etkinleştirilmesini sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar

- Aboneliğiniz için bir güvenlik kişi telefon numarası sağlanmalıdır

## <a name="218-ensure-that-send-email-notification-for-high-severity-alerts-is-set-to-on"></a>2.18 'Yüksek önem uyarıları için e-posta bildirimi gönder' 'A)'ya ayarlandığından emin olun

Bu plan, güvenlik bildirimlerinin düzgün şekilde etkinleştirilmesini sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar

- Yüksek önem uyarıları için e-posta bildirimi etkinleştirilmelidir

## <a name="219-ensure-that-send-email-also-to-subscription-owners-is-set-to-on"></a>2.19 'Abonelik sahiplerine de e-posta gönderin' 'A)'nın 'A)'ya ayarlandığından emin olun

Bu plan, güvenlik bildirimlerinin düzgün şekilde etkinleştirilmesini sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar

- Yüksek önem uyarıları için abonelik sahibine e-posta bildirimi etkinleştirilmelidir

## <a name="31-ensure-that-secure-transfer-required-is-set-to-enabled"></a>3.1 'Güvenli aktarım gerekli' olarak ayarlandığından emin olun

Bu plan, güvenli olmayan bağlantılara izin veren depolama hesaplarını izlemenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Depolama hesaplarına güvenli aktarım etkinleştirilmelidir

## <a name="37-ensure-default-network-access-rule-for-storage-accounts-is-set-to-deny"></a>3.7 Depolama Hesapları için varsayılan ağ erişim kuralının

Bu plan, sınırsız erişime izin veren depolama hesaplarını izlemenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Depolama hesaplarına sınırsız ağ erişimini denetleme

## <a name="38-ensure-trusted-microsoft-services-is-enabled-for-storage-account-access"></a>3.8 Depolama Hesabı erişimi için 'Güvenilir Microsoft Hizmetlerinin' etkinleştirildiğinden emin olun

Bu plan, güvenilen Microsoft hizmetlerinden erişime izin verilmeyen depolama hesaplarını izlemenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Depolama hesapları, güvenilen Microsoft hizmetlerinden erişime izin vermelidir

## <a name="41-ensure-that-auditing-is-set-to-on"></a>4.1 'Denetim'in 'Açık' olarak ayarlandığından emin olun

Bu plan, SQL sunucu denetiminin etkin olmasını sağlamaya yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar. 

- SQL Server'da gelişmiş veri güvenliği ayarlarında denetim etkinleştirilmelidir

## <a name="42-ensure-that-auditactiongroups-in-auditing-policy-for-a-sql-server-is-set-properly"></a>4.2 Bir SQL sunucusu için 'denetim' ilkesindeki 'AuditActionGroups'un düzgün ayarlandığından emin olun

Bu plan, SQL sunucu denetiminin düzgün şekilde yapılandırıldığından emin olmak için bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- SQL Denetim ayarları, kritik etkinlikleri yakalamak için Eylem Grupları yapılandırıldı

## <a name="43-ensure-that-auditing-retention-is-greater-than-90-days"></a>4.3 'Denetim' Saklamanın '90 günden fazla' olduğundan emin olun

Bu plan, SQL sunucu günlüklerinin en az 90 gün süreyle tutulmasını sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- SQL sunucuları, 90 günden uzun denetim bekletme günleriyle yapılandırılmalıdır.

## <a name="44-ensure-that-advanced-data-security-on-a-sql-server-is-set-to-on"></a>4.4 BIR SQL sunucusundaki 'Gelişmiş Veri Güvenliği'nin 'A)'ya ayarlandığından emin olun

Bu plan, SQL sunucularında ve SQL yönetilen örneklerinde gelişmiş veri güvenliğinin etkin olmasını sağlamaya yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- SQL yönetilen örneklerinizde gelişmiş veri güvenliği etkinleştirilmelidir
- SQL sunucularınızda gelişmiş veri güvenliği etkinleştirilmelidir

## <a name="45-ensure-that-threat-detection-types-is-set-to-all"></a>4.5 'Tehdit Algılama türlerinin' 'Tümü' olarak ayarlandığından emin olun

Bu plan, gelişmiş tehdit korumasının SQL sunucularında ve SQL yönetilen örneklerinde düzgün şekilde yapılandırıldığından emin [olduğunuza](../../../policy/overview.md) yardımcı olan Azure İlkesi tanımları atar.

- Gelişmiş Tehdit Koruması türleri SQL sunucusu Gelişmiş Veri Güvenliği ayarlarında 'Tümü' olarak ayarlanmalıdır
- Gelişmiş Tehdit Koruması türleri SQL yönetilen örnek Gelişmiş Veri Güvenliği ayarlarında 'Tümü' olarak ayarlanmalıdır

## <a name="46-ensure-that-send-alerts-to-is-set"></a>4.6 'Uyarı gönder' ayarlı olduğundan emin olun

Bu plan, gelişmiş veri güvenliği bildirimlerinin düzgün şekilde etkinleştirilmesini sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- SQL yönetilen örnek için gelişmiş veri güvenliği ayarları güvenlik uyarıları almak için bir e-posta adresi içermelidir
- SQL sunucusu için gelişmiş veri güvenliği ayarları, güvenlik uyarıları almak için bir e-posta adresi içermelidir

## <a name="47-ensure-that-email-service-and-co-administrators-is-enabled"></a>4.7 'E-posta hizmeti ve yardımcı yöneticilerin' 'Etkin' olduğundan emin olun

Bu plan, gelişmiş veri güvenliği bildirimlerinin düzgün şekilde etkinleştirilmesini sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Yöneticilere ve abonelik sahiplerine e-posta bildirimleri SQL yönetilen örnek gelişmiş veri güvenliği ayarlarında etkinleştirilmelidir
- Yöneticilere ve abonelik sahiplerine e-posta bildirimleri SQL sunucusu gelişmiş veri güvenliği ayarlarında etkinleştirilmelidir

## <a name="48-ensure-that-azure-active-directory-admin-is-configured"></a>4.8 Azure Active Directory Admin'in yapılandırıldığından emin olun

Bu plan, Bir Azure Active Directory yöneticisinin SQL sunucuları için sağlanmasını sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- SQL sunucuları için bir Azure Active Directory yöneticisi sağlanmalıdır

## <a name="49-ensure-that-data-encryption-is-set-to-on-on-a-sql-database"></a>4.9 SQL Veritabanında 'Veri şifreleme'nin 'A)'ya ayarlandığından emin olun

Bu plan, SQL veritabanlarında Saydam Veri Şifrelemesi'nin etkin olmasını sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- SQL veritabanlarında Saydam Veri Şifrelemesi etkinleştirilmelidir

## <a name="410-ensure-sql-servers-tde-protector-is-encrypted-with-byok-use-your-own-key"></a>4.10 SQL sunucusunun TDE koruyucusu BYOK ile şifrelenir (Kendi anahtarınızı kullanın)

Bu plan, SQL sunucuları için saydam veri şifreli koruyucunun ve SQL yönetilen örneklerinin kendi anahtarınızla şifrelenmesini sağlamanıza yardımcı olan [Azure İlkesi](../../../policy/overview.md) tanımları atar.

- SQL yönetilen örnek TDE koruyucukendi anahtarı ile şifrelenmelidir
- SQL server TDE koruyucusu kendi anahtarınızla şifrelenmelidir

## <a name="411-ensure-enforce-ssl-connection-is-set-to-enabled-for-mysql-database-server"></a>4.11 MySQL Database Server için 'SSL bağlantısını uygula' olarak ayarlandığından emin olun

Bu plan, MySQL veritabanı sunucularının SSL bağlantılarını zorlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- MySQL veritabanı sunucuları için SSL bağlantısını zorla

## <a name="412-ensure-server-parameter-log_checkpoints-is-set-to-on-for-postgresql-database-server"></a>4.12 PostgreSQL Database Server için sunucu parametresi 'log_checkpoints' 'A)'ya ayarlandığından emin olun

Bu plan, PostgreSQL veritabanı sunucularının denetim noktalarını günlüğe kaydetmenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- PostgreSQL veritabanı sunucuları için günlük denetim noktaları etkinleştirilmelidir

## <a name="413-ensure-enforce-ssl-connection-is-set-to-enabled-for-postgresql-database-server"></a>4.13 PostgreSQL Veritabanı Sunucusu için 'SSL bağlantısını uygula' olarak ayarlandığından emin olun

Bu plan, PostgreSQL veritabanı sunucularının SSL bağlantılarını zorlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- PostgreSQL veritabanı sunucuları için SSL bağlantısını uygulayın etkinleştirilmelidir

## <a name="414-ensure-server-parameter-log_connections-is-set-to-on-for-postgresql-database-server"></a>4.14 PostgreSQL Database Server için sunucu parametresi 'log_connections' 'A)'ya ayarlandığından emin olun

Bu plan, PostgreSQL veritabanı sunucularının bağlantıları günlüğe kaydetmenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- PostgreSQL veritabanı sunucuları için günlük bağlantıları etkinleştirilmelidir

## <a name="415-ensure-server-parameter-log_disconnections-is-set-to-on-for-postgresql-database-server"></a>4.15 PostgreSQL Database Server için sunucu parametresi 'log_disconnections' 'A)'ya ayarlandığından emin olun

Bu plan, PostgreSQL veritabanı sunucularında günlük bağlantı bağlantılarını sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- PostgreSQL veritabanı sunucuları için bağlantı bağlantıları günlüğe kaydedilmelidir.

## <a name="416-ensure-server-parameter-log_duration-is-set-to-on-for-postgresql-database-server"></a>4.16 PostgreSQL Database Server için sunucu parametresi 'log_duration' 'A)'ya ayarlandığından emin olun

Bu plan, PostgreSQL veritabanı sunucularının tamamlanan deyimlerin süresini günlüğe kaydetmenize yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- PostgreSQL veritabanı sunucuları için günlük süresi etkinleştirilmelidir

## <a name="417-ensure-server-parameter-connection_throttling-is-set-to-on-for-postgresql-database-server"></a>4.17 PostgreSQL Database Server için sunucu parametresi 'connection_throttling' 'A)'ya ayarlandığından emin olun

Bu plan, PostgreSQL veritabanı sunucularında kaba kuvvet saldırılarını azaltmanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- PostgreSQL veritabanı sunucuları için bağlantı azaltma etkinleştirilmelidir

## <a name="419-ensure-that-azure-active-directory-admin-is-configured"></a>4.19 Azure Active Directory Admin'in yapılandırıldığından emin olun

Bu plan, Bir Azure Active Directory yöneticisinin SQL sunucuları için sağlanmasını sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar. BDT Microsoft Azure Temelleri Kıyaslama bu öneriyi içerir; ancak, [tavsiye 4.8](#48-ensure-that-azure-active-directory-admin-is-configured)bir kopyasıdır.

- SQL sunucuları için bir Azure Active Directory yöneticisi sağlanmalıdır

## <a name="511-ensure-that-a-log-profile-exists"></a>5.1.1 Günlük Profili olduğundan emin olun

Bu plan, tüm Azure abonelikleri için günlük profilinin bulunmasını sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar. 

- Azure aboneliklerinin Etkinlik Günlüğü için bir günlük profili olmalıdır

## <a name="512-ensure-that-activity-log-retention-is-set-365-days-or-greater"></a>5.1.2 Etkinlik Günlüğü Bekletme 365 gün veya daha büyük ayarlandığından emin olun

Bu plan, etkinlik günlüklerinin en az bir yıl süreyle tutulmasını sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Etkinlik günlüğü en az bir yıl boyunca tutulmalıdır

## <a name="513-ensure-audit-profile-captures-all-the-activities"></a>5.1.3 Denetim profilinin tüm faaliyetleri yakalamasını sağlamak

Bu plan, günlük profilinin düzgün şekilde yapılandırıldığından emin olmak için bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Azure Monitor günlük profili 'yazma', 'silme' ve 'eylem' kategorileri için günlükleri toplamalı

## <a name="514-ensure-the-log-profile-captures-activity-logs-for-all-regions-including-global"></a>5.1.4 Günlük profilinin küresel dahil tüm bölgeler için etkinlik günlüklerini yakaladığından emin olun

Bu plan, günlük profilinin düzgün şekilde yapılandırıldığından emin olmak için bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Azure Monitor tüm bölgelerden etkinlik günlükleri toplamalıdır

## <a name="517-ensure-that-logging-for-azure-keyvault-is-enabled"></a>5.1.7 Azure KeyVault için günlüğe kaydetmenin 'Etkin' olduğundan emin olun

Bu plan, önemli kasalar için tanılama günlüklerinin etkin olmasını sağlamaya yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Key Vault'taki tanılama günlükleri etkinleştirilmeli

## <a name="65-ensure-that-network-watcher-is-enabled"></a>6.5 Ağ İzleyicisi'nin 'Etkin' olduğundan emin olun

Bu plan, kaynakların dağıtıldığı tüm bölgeler için Ağ İzleyicisi'nin etkin olmasını sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar. Bu ilke, tüm geçerli bölgeleri belirten bir parametre dizisi gerektirir. Bu ilke girişimi tanımındaki varsayılan değer 'eastus'tır.

- Ağ İzleyicisi etkinleştirilmelidir

## <a name="71-ensure-that-os-disk-are-encrypted"></a>7.1 'işletim sistemi diskinin' şifrelendirilmesini sağlamak

Bu plan, sanal makinelerde disk şifrelemenin etkin olmasını sağlamaya yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Disk şifreleme sanal makinelerde uygulanmalıdır

## <a name="72-ensure-that-data-disks-are-encrypted"></a>7.2 'Veri disklerinin' şifrelendirilmesini sağlamak

Bu plan, sanal makinelerde disk şifrelemenin etkin olmasını sağlamaya yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Disk şifreleme sanal makinelerde uygulanmalıdır

## <a name="73-ensure-that-unattached-disks-are-encrypted"></a>7.3 'Eklenmemiş disklerin' şifrelendirilmesini sağlamak

Bu plan, eklenmemiş disklerin şifrelenmenizi sağlamaya yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Eklenmemiş diskler şifrelenmelidir

## <a name="74-ensure-that-only-approved-extensions-are-installed"></a>7.4 Yalnızca onaylı uzantıların yüklendiğinden emin olun

Bu plan, yalnızca onaylanmış sanal makine uzantılarının yüklendiğinden emin olmak için bir [Azure İlkesi](../../../policy/overview.md) tanımı atar. Bu ilke, onaylanan tüm sanal makine uzantılarını belirten bir parametre dizisi gerektirir. Bu ilke girişimi tanımı, müşterilerin doğrulanması gereken önerilen varsayılanları içerir. 

 - Yalnızca onaylanmış VM uzantıları yüklenmelidir

## <a name="75-ensure-that-the-latest-os-patches-for-all-virtual-machines-are-applied"></a>7.5 Tüm Sanal Makineler için en son işletim sistemi yamaları uygulandığından emin olun

Bu plan, sistem güncelleştirmelerinin sanal makinelere yüklenmesini sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Makinelerinize sistem güncelleştirmeleri yüklenmelidir

## <a name="76-ensure-that-the-endpoint-protection-for-all-virtual-machines-is-installed"></a>7.6 Tüm Sanal Makineler için uç nokta korumasının yüklendiğinden emin olun

Bu plan, sanal makinelerde uç nokta korumasının etkin olmasını sağlamaya yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Azure Güvenlik Merkezi'nde eksik Uç Nokta Koruması'nı izleme

## <a name="84-ensure-the-key-vault-is-recoverable"></a>8.4 Anahtar kasasının kurtarılabilir olduğundan emin olun

Bu plan, yanlışlıkla silinme durumunda anahtar kasa nesnelerinin kurtarılabilir olmasını sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Key Vault nesneleri kurtarılabilir olmalıdır

## <a name="85-enable-role-based-access-control-rbac-within-azure-kubernetes-services"></a>8.5 Azure Kubernetes Hizmetleri'nde rol tabanlı erişim denetimini (RBAC) etkinleştirme

Bu plan, Rol tabanlı erişim denetiminin Kubernetes hizmet kümelerinde yönetilen izinlerde kullanılmasını sağlamanıza yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar

- \[Önizleme\]: Kubernetes Hizmetlerinde Rol Tabanlı Erişim Kontrolü (RBAC) kullanılmalıdır

## <a name="92-ensure-web-app-redirects-all-http-traffic-to-https-in-azure-app-service"></a>9.2 Azure Uygulama Hizmeti'nde web uygulamasının tüm HTTP trafiğini HTTPS'ye yönlendirdiğinden emin olun

Bu plan, web uygulamalarına yalnızca güvenli bağlantılar üzerinden erişilebilmesini sağlamaya yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) tanımı atar.

- Web Uygulamasına yalnızca HTTPS üzerinden erişilebilir olmalıdır

## <a name="93-ensure-web-app-is-using-the-latest-version-of-tls-encryption"></a>9.3 Web uygulamasının TLS şifrelemenin en son sürümünü kullandığından emin olun

Bu plan, web uygulamalarının en son TLS sürümünü kullandığından emin olmak için [Azure İlkesi](../../../policy/overview.md) tanımları atar.

- API Uygulamanızda en son TLS sürümü kullanılmalıdır
- Fonksiyon Uygulamanızda en son TLS sürümü kullanılmalıdır
- Web Uygulamanızda en son TLS sürümü kullanılmalıdır

## <a name="94-ensure-the-web-app-has-client-certificates-incoming-client-certificates-set-to-on"></a>9.4 Web uygulamasının 'Müşteri Sertifikaları (Gelen müşteri sertifikaları)' 'A)' olarak ayarlandığından emin olun

Bu plan, yalnızca geçerli sertifikalara sahip istemcilerin bir web uygulamasına erişebilmesini sağlamaya yardımcı olan [Azure İlkesi](../../../policy/overview.md) tanımları atar.

- API uygulamasının 'Müşteri Sertifikaları (Gelen istemci sertifikaları)' 'A)' olarak ayarlandığından emin olun
- İşlev uygulamasının 'İstemci Sertifikaları (Gelen istemci sertifikaları)' 'A)' olarak ayarlandığından emin olun
- WEB uygulamasının 'Müşteri Sertifikaları (Gelen istemci sertifikaları)' 'A)' olarak ayarlandığından emin olun

## <a name="95-ensure-that-register-with-azure-active-directory-is-enabled-on-app-service"></a>9.5 Uygulama Hizmetinde Azure Active Directory ile Kaydolun etkinleştirildiğinden emin olun

Bu plan, web uygulamalarının yönetilen bir kimlik kullandığından emin olmak için [Azure İlkesi](../../../policy/overview.md) tanımları atar.

- API uygulamasında Azure Active Directory ile Kaydolun etkinleştirildiğinden emin olun
- İşlev Uygulamasında Azure Active Directory ile Kaydolun etkinleştirildiğinden emin olun
- AZURE Active Directory ile Kaydol'un WEB Uygulamasında etkinleştirildiğinden emin olun

## <a name="96-ensure-that-net-framework-version-is-the-latest-if-used-as-a-part-of-the-web-app"></a>9.6 Web uygulamasının bir parçası olarak kullanıldığında '.Net Framework' sürümünün en son sürüm olduğundan emin olun

Bu plan, web uygulamalarının .Net Framework'ün en son sürümünü kullandığından emin olmak için [Azure İlkesi](../../../policy/overview.md) tanımları atar.

- API uygulamasının bir parçası olarak kullanıldığında '.Net Framework' sürümünün en son sürüm olduğundan emin olun
- İşlev Uygulaması'nın bir parçası olarak kullanıldığında '.Net Framework' sürümünün en son sürüm olduğundan emin olun
- Web uygulamasının bir parçası olarak kullanıldığında '.Net Framework' sürümünün en son sürüm olduğundan emin olun

## <a name="97-ensure-that-php-version-is-the-latest-if-used-to-run-the-web-app"></a>9.7 Web uygulamasını çalıştırmak için kullanılırsa 'PHP sürümü'nün en son sürüm olduğundan emin olun

Bu plan, web uygulamalarının PHP'nin en son sürümünü kullandığından emin olmak için [Azure İlkesi](../../../policy/overview.md) tanımları atar.

- Api uygulamasının bir parçası olarak kullanıldığında 'PHP sürümü'nün en son sürüm olduğundan emin olun
- Fonksiyon uygulamasının bir parçası olarak kullanıldığında 'PHP sürümü'nün en son sürüm olduğundan emin olun
- WEB uygulamasının bir parçası olarak kullanıldığında 'PHP sürümü'nün en son sürüm olduğundan emin olun

## <a name="98-ensure-that-python-version-is-the-latest-if-used-to-run-the-web-app"></a>9.8 Web uygulamasını çalıştırmak için kullanılırsa 'Python sürümü'nün en son sürüm olduğundan emin olun

Bu plan, web uygulamalarının Python'un en son sürümünü kullandığından emin olmak için [Azure İlkesi](../../../policy/overview.md) tanımları atar.

- Api uygulamasının bir parçası olarak kullanılıyorsa ,'Python sürümü'nün en son sürüm olduğundan emin olun
- İşlev uygulamasının bir parçası olarak kullanılıyorsa ,'Python sürümü'nün en son sürüm olduğundan emin olun
- Web uygulamasının bir parçası olarak kullanılıyorsa ,'Python sürümü'nün en son sürüm olduğundan emin olun

## <a name="99-ensure-that-java-version-is-the-latest-if-used-to-run-the-web-app"></a>9.9 Web uygulamasını çalıştırmak için kullanılırsa 'Java sürümü'nün en son sürüm olduğundan emin olun

Bu plan, web uygulamalarının Java'nın en son sürümünü kullandığından emin olmak için [Azure İlkesi](../../../policy/overview.md) tanımları atar.

- Api uygulamasının bir parçası olarak kullanıldığında 'Java sürümü'nün en son sürüm olduğundan emin olun
- Funtion uygulamasının bir parçası olarak kullanıldığında 'Java sürümü'nün en son sürüm olduğundan emin olun
- Web uygulamasının bir parçası olarak kullanılıyorsa, 'Java sürümü'nün en son sürüm olduğundan emin olun

## <a name="910-ensure-that-http-version-is-the-latest-if-used-to-run-the-web-app"></a>9.10 Web uygulamasını çalıştırmak için kullanılırsa 'HTTP Sürümü'nün en son sürüm olduğundan emin olun

Bu plan, web uygulamalarının HTTP'nin en son sürümünü kullandığından emin olmak için [Azure İlkesi](../../../policy/overview.md) tanımları atar.

- Api uygulamasını çalıştırmak için kullanılırsa 'HTTP Sürümü'nün en son sürüm olduğundan emin olun
- İşlev uygulamasını çalıştırmak için kullanılırsa 'HTTP Sürümü'nün en son sürüm olduğundan emin olun
- Web uygulamasını çalıştırmak için kullanılırsa ,'HTTP Sürümü'nün en son sürüm olduğundan emin olun


## <a name="next-steps"></a>Sonraki adımlar

BDT Microsoft Azure Temelleri Kıyaslama planının denetim eşlemini gözden geçirdiğiniz için, plan hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin veya girişimi atamak için Azure portalındaki Azure Politikası'nı ziyaret edin:

> [!div class="nextstepaction"]
> [BDT Microsoft Azure Temelleri Kıyaslama planı -](./index.md)
> [BDT Microsoft Azure Temelleri Kıyaslama planı - Dağıtım adımları](./deploy.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Plan yaşam döngüsü](../../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../../how-to/update-existing-assignments.md)öğrenin.