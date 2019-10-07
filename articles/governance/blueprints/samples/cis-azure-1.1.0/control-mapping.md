---
title: Örnek-CIS Microsoft Azure temel kıyaslama kıyaslama şeması-öneri eşleme
description: CIS, kıyaslama şeması örneğini Azure Ilkesine göre Microsoft Azure.
author: DCtheGeek
ms.author: dacoulte
ms.date: 08/09/2019
ms.topic: sample
ms.service: blueprints
ms.openlocfilehash: 3814b5169a687a854cb977eee90fbf6c3fd45afe
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2019
ms.locfileid: "71980971"
---
# <a name="recommendation-mapping-of-the-cis-microsoft-azure-foundations-benchmark-blueprint-sample"></a>CIS Microsoft Azure temel kıyaslama kıyaslama şeması örneği

Aşağıdaki makalede, Azure şemaları CIS Microsoft Azure, kıyaslama şeması örnek haritaları olan CIS Microsoft Azure, kıyaslama önerilerini temel alarak ve Öneriler hakkında daha fazla bilgi için bkz. [cıs Microsoft Azure kuruluş kıyaslaması](https://www.cisecurity.org/benchmark/azure/).

Aşağıdaki eşlemeler, **Yönetim ve kıyaslama v 1.1.0** önerilerini temel alarak Microsoft Azure. Sağ taraftaki gezinmeyi kullanarak doğrudan belirli bir öneri eşlemesine atlayın.
Eşlenmiş önerilerin birçoğu bir [Azure Policy](../../../policy/overview.md) girişimi ile uygulanır. Tüm girişimi gözden geçirmek için Azure portal **ilkeyi** açın ve **tanımlar** sayfasını seçin. Ardından, **\[Preview @ no__t-2 denetim MICROSOFT Azure CIS ' yi bulun ve seçin, kıyaslama v 1.1.0 önerilerini ve belırlı VM uzantılarını dağıtarak denetim gereksinimleri** yerleşik ilke girişimi ' ni destekler.

> [!IMPORTANT]
> Aşağıdaki her denetim bir veya daha fazla [Azure ilke](../../../policy/overview.md) tanımı ile ilişkilidir. Bu ilkeler, denetimiyle [uyumluluğu değerlendirmenize](../../../policy/how-to/get-compliance-data.md) yardımcı olabilir; Ancak, bir denetim ve bir veya daha fazla ilke arasında genellikle bir 1:1 veya bir eşleşme yoktur. Bu nedenle, Azure Ilkesi ile **uyumlu** , yalnızca ilkelerin kendilerine başvurur; Bu, bir denetimin tüm gereksinimleriyle tamamen uyumlu olduğunuzdan emin değildir. Buna ek olarak, uyumluluk standardı şu anda herhangi bir Azure Ilke tanımı tarafından açıklanmayan denetimler içerir. Bu nedenle, Azure Ilkesinde uyumluluk, genel uyumluluk durumunuzu yalnızca kısmi görünümüdür. Bu uyumluluk şeması örneği için denetimler ve Azure Ilke tanımları arasındaki ilişkilendirmeler zaman içinde değişebilir. Değişiklik geçmişini görüntülemek için [GitHub kayıt geçmişine](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/cis-azure-1.1.0/control-mapping.md)bakın.

## <a name="11-ensure-that-multi-factor-authentication-is-enabled-for-all-privileged-users"></a>1,1 tüm ayrıcalıklı kullanıcılar için Multi-Factor Authentication 'ın etkinleştirildiğinden emin olun

Bu şema, bu CIS önerisiyle hizalı [Azure ilke](../../../policy/overview.md) tanımları atar.

- MFA, aboneliğinizde sahip izinleri olan hesaplarda etkinleştirilmelidir
- MFA, aboneliğinizde yazma izinleri olan hesaplarda etkinleştirilmelidir

## <a name="12-ensure-that-multi-factor-authentication-is-enabled-for-all-non-privileged-users"></a>1,2 çok faktörlü kimlik doğrulamasının tüm ayrıcalıklı olmayan kullanıcılar için etkinleştirildiğinden emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- MFA, aboneliğinizde okuma izinleri olan hesaplarda etkinleştirilmelidir

## <a name="13-ensure-that-there-are-no-guest-users"></a>1,3 Konuk Kullanıcı bulunmadığından emin olun

Bu şema, bu CIS önerisiyle hizalı [Azure ilke](../../../policy/overview.md) tanımları atar.

- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Okuma izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekir
- Yazma izinleri olan dış hesapların aboneliğinizden kaldırılması gerekir

## <a name="23-ensure-asc-default-policy-setting-monitor-system-updates-is-not-disabled"></a>2,3 "sistem güncelleştirmelerini Izleme" için ASC varsayılan ilke ayarı "devre dışı" olarak belirlendiğinden emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- Sistem güncelleştirmelerinin makinelerinizde yüklü olması gerekir

## <a name="24-ensure-asc-default-policy-setting-monitor-os-vulnerabilities-is-not-disabled"></a>2,4 "Monitor OS güvenlik açıklarını Izleme", "devre dışı" olarak ASC varsayılan ilke ayarını doğrulayın

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir

## <a name="25-ensure-asc-default-policy-setting-monitor-endpoint-protection-is-not-disabled"></a>2,5 "Monitor Endpoint Protection" varsayılan ilke ayarının "devre dışı" olmadığından emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- Azure Güvenlik Merkezi 'nde eksik Endpoint Protection izleme

## <a name="26-ensure-asc-default-policy-setting-monitor-disk-encryption-is-not-disabled"></a>2,6 "disk şifrelemeyi Izle" ayarının "devre dışı" olarak ASC varsayılan ilke ayarını sağlayın

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- Disk şifrelemesi sanal makinelere uygulanmalıdır

## <a name="28-ensure-asc-default-policy-setting-monitor-web-application-firewall-is-not-disabled"></a>2,8 ASC varsayılan ilke ayarı "Web uygulaması güvenlik duvarını Izle" ayarının "devre dışı" olmadığından emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- IaaS üzerinde Web uygulamalarına yönelik NSG kuralları sağlamlaştırılmış olmalıdır

## <a name="210-ensure-asc-default-policy-setting-monitor-vulnerability-assessment-is-not-disabled"></a>2,10 "Izleme güvenlik açığı değerlendirmesi" için ASC varsayılan ilke ayarının "devre dışı" olmadığından emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- Güvenlik açıkları bir güvenlik açığı değerlendirme çözümü tarafından düzeltilmelidir

## <a name="212-ensure-asc-default-policy-setting-monitor-jit-network-access-is-not-disabled"></a>2,12 "JıT ağ erişimini Izleme" için ASC varsayılan ilke ayarının "devre dışı" olmadığından emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- Tam zamanında ağ erişim denetimi, sanal makinelere uygulanmalıdır

## <a name="215-ensure-asc-default-policy-setting-monitor-sql-encryption-is-not-disabled"></a>2,15 "SQL şifrelemeyi Izleme" için ASC varsayılan ilke ayarının "devre dışı" olmadığından emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- SQL veritabanlarındaki Saydam Veri Şifrelemesi etkinleştirilmelidir

## <a name="31-ensure-that-secure-transfer-required-is-set-to-enabled"></a>3,1 ' güvenli aktarım gerekli ' ' Enabled ' olarak ayarlandığından emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- Depolama hesaplarına Güvenli aktarım etkinleştirilmelidir

## <a name="37-ensure-default-network-access-rule-for-storage-accounts-is-set-to-deny"></a>3,7 depolama hesapları için varsayılan ağ erişim kuralının Reddet olarak ayarlandığından emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- Depolama hesaplarına Kısıtlanmamış ağ erişimini denetleme

## <a name="41-ensure-that-auditing-is-set-to-on"></a>4,1 ' denetim ' ' on ' olarak ayarlandığından emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- SQL Server üzerindeki gelişmiş veri güvenliği ayarlarında denetim etkinleştirilmelidir

## <a name="42-ensure-that-auditactiongroups-in-auditing-policy-for-a-sql-server-is-set-properly"></a>4,2 SQL Server için ' denetim ' ilkesindeki ' AuditActionGroups ' işleminin düzgün şekilde ayarlandığından emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- SQL denetim ayarları, kritik etkinlikleri yakalamak için yapılandırılmış eylem grupları olmalıdır

## <a name="43-ensure-that-auditing-retention-is-greater-than-90-days"></a>4,3 ' denetim ' saklama 'nin 90 günden daha fazla olduğundan emin olun '

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- SQL Server 'lar 90 günden daha uzun denetim bekletme günlerindeki şekilde yapılandırılmalıdır.

## <a name="44-ensure-that-advanced-data-security-on-a-sql-server-is-set-to-on"></a>4,4 SQL Server üzerinde ' Gelişmiş veri güvenliği 'nin ' açık ' olarak ayarlandığından emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- SQL sunucularınızda gelişmiş veri güvenliği etkin olmalıdır

## <a name="45-ensure-that-threat-detection-types-is-set-to-all"></a>4,5 ' tehdit algılama türleri ' ' All ' olarak ayarlandığından emin olun

Bu şema, bu CIS önerisiyle hizalı [Azure ilke](../../../policy/overview.md) tanımları atar.

- Gelişmiş tehdit koruması türleri SQL Server Gelişmiş veri güvenliği ayarları 'nda ' tümü ' olarak ayarlanmalıdır
- Gelişmiş tehdit koruması türleri SQL yönetilen örneği gelişmiş veri güvenliği ayarları içinde ' All ' olarak ayarlanmalıdır

## <a name="46-ensure-that-send-alerts-to-is-set"></a>4,6 ' uyarı gönder 'in ayarlanmış olduğundan emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- SQL Server için gelişmiş veri güvenliği ayarları, güvenlik uyarılarını almak için bir e-posta adresi içermelidir

## <a name="47-ensure-that-email-service-and-co-administrators-is-enabled"></a>4,7 ' e-posta hizmeti ve ortak yöneticiler ' ' in ' etkin ' olduğundan emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- SQL yönetilen örneği için gelişmiş veri güvenliği ayarları, güvenlik uyarılarını almak için bir e-posta adresi içermelidir

## <a name="48-ensure-that-azure-active-directory-admin-is-configured"></a>4,8 Azure Active Directory yöneticisinin yapılandırıldığından emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- SQL sunucuları için bir Azure Active Directory Yöneticisi sağlanmalıdır

## <a name="49-ensure-that-data-encryption-is-set-to-on-on-a-sql-database"></a>4,9 SQL veritabanında ' veri şifreleme ' ' on ' olarak ayarlandığından emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- SQL veritabanlarındaki Saydam Veri Şifrelemesi etkinleştirilmelidir

## <a name="410-ensure-sql-servers-tde-protector-is-encrypted-with-byok-use-your-own-key"></a>4,10 SQL Server 'ın TDE koruyucunun BYOK ile şifrelendiğinden emin olun (kendi anahtarınızı kullanın)

Bu şema, bu CIS önerisiyle hizalı [Azure ilke](../../../policy/overview.md) tanımları atar.

- SQL Server TDE koruyucusu kendi anahtarınızla şifrelenmelidir
- SQL yönetilen örnek TDE koruyucusu kendi anahtarınızla şifrelenmelidir

## <a name="517-ensure-that-logging-for-azure-keyvault-is-enabled"></a>5.1.7 Azure Anahtar Kasası günlüğü 'nün ' Enabled ' olduğundan emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- Key Vault tanılama günlükleri etkinleştirilmelidir

## <a name="71-ensure-that-os-disk-are-encrypted"></a>7,1 ' işletim sistemi diski 'nin şifrelendiğinden emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- Disk şifrelemesi sanal makinelere uygulanmalıdır

## <a name="72-ensure-that-data-disks-are-encrypted"></a>7,2 ' veri diskleri ' nin şifrelendiğinden emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- Disk şifrelemesi sanal makinelere uygulanmalıdır

## <a name="75-ensure-that-the-latest-os-patches-for-all-virtual-machines-are-applied"></a>7,5 tüm sanal makineler için en son işletim sistemi düzeltme eklerinin uygulandığından emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- Sistem güncelleştirmelerinin makinelerinizde yüklü olması gerekir

## <a name="76-ensure-that-the-endpoint-protection-for-all-virtual-machines-is-installed"></a>7,6 tüm sanal makineler için Endpoint Protection 'ın yüklü olduğundan emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- Azure Güvenlik Merkezi 'nde eksik Endpoint Protection izleme

## <a name="85-enable-role-based-access-control-rbac-within-azure-kubernetes-services"></a>8,5 Azure Kubernetes Hizmetleri içinde rol tabanlı erişim denetimi 'ni (RBAC) etkinleştirme

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- \[Preview @ no__t-1: Kubernetes hizmetlerinde rol tabanlı Access Control (RBAC) kullanılmalıdır

## <a name="92-ensure-web-app-redirects-all-http-traffic-to-https-in-azure-app-service"></a>9,2 Web uygulamasının tüm HTTP trafiğini Azure App Service ' de HTTPS 'ye yönlendirdiğinden emin olun

Bu şema, bu CIS önerisiyle hizalanan bir [Azure ilke](../../../policy/overview.md) tanımı atar.

- Web uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır

## <a name="next-steps"></a>Sonraki adımlar

Artık CIS Microsoft Azure temel alan Kıyaslanmaların denetim eşlemesini gözden geçirdiğinize göre, girişimi atamak için Azure portal Azure Ilkesi şeması hakkında bilgi edinmek için aşağıdaki makaleyi ziyaret edin:

> [!div class="nextstepaction"]
> [Cıs Microsoft Azure temel geçişleri kıyaslama şeması-genel bakış](./index.md)
> [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyMenuBlade/Definitions)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.