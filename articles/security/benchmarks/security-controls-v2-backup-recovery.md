---
title: Azure Güvenlik kıyaslama v2-yedekleme ve kurtarma
description: Azure Güvenlik kıyaslaması v2 yedekleme ve kurtarma
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 02/22/2021
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 39466ad621eff1a7d3490c936c90fbff6f63e0fc
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102051557"
---
# <a name="security-control-v2-backup-and-recovery"></a>Güvenlik denetimi v2: yedekleme ve kurtarma

Yedekleme ve kurtarma, farklı hizmet katmanlarındaki veri ve yapılandırma yedeklemelerinin gerçekleştirilmesini, doğrulanmasını ve korunmasını sağlamak için denetimleri ele alır.

Geçerli yerleşik Azure Ilkesini görmek için bkz. [Azure Güvenlik kıyaslaması uyumluluğu yerleşik girişiminin ayrıntıları: yedekleme ve kurtarma](../../governance/policy/samples/azure-security-benchmark.md#backup-and-recovery)

## <a name="br-1-ensure-regular-automated-backups"></a>BR-1: düzenli otomatik yedeklemeler sağlayın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| BR-1 | 10.1 | CP-2, CP4, CP-6, CP-9 |

Beklenmeyen bir olaydan sonra iş sürekliliği sağlamak için sistemleri ve verileri yedeklemediğinizden emin olun. Bu, kurtarma noktası hedefi (RPO) ve kurtarma süresi hedefi (RTO) için herhangi bir amaç tarafından tanımlanmalıdır.

Azure Backup etkinleştirin ve yedekleme kaynağını (Azure VM 'Leri, SQL Server, HANA veritabanları veya dosya paylaşımları gibi) ve istenen sıklık ve bekletme süresini yapılandırın.

Daha yüksek düzeyde koruma için, coğrafi olarak yedekli depolama seçeneğini, yedek verileri ikincil bir bölgeye çoğaltmak ve çapraz bölge geri yükleme kullanarak kurtarmak için etkinleştirebilirsiniz.

- [Kurumsal ölçekli iş sürekliliği ve olağanüstü durum kurtarma](/azure/cloud-adoption-framework/ready/enterprise-scale/business-continuity-and-disaster-recovery)

- [Azure Backup etkinleştirme](../../backup/index.yml)

- [Bölgeler arası geri yükleme nasıl etkinleştirilir?](../../backup/backup-azure-arm-restore-vms.md#cross-region-restore)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [İlke ve standartlar](/azure/cloud-adoption-framework/organize/cloud-security-policy-standards)

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Olay hazırlama](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

## <a name="br-2-encrypt-backup-data"></a>BR-2: yedekleme verilerini şifreleme

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| BR-2 | 10.2 | CP-9 |

Yedeklemelerinizin saldırılara karşı korunduğundan emin olun. Bu, gizlilik kaybına karşı korunmak için yedeklemelerin şifrelenmesini içermelidir.

Azure Backup kullanarak şirket içi yedeklemeler için, sağladığınız parola kullanılarak bekleyen şifreleme sağlanır. Normal Azure hizmet yedeklemeleri için, yedekleme verileri Azure platformu tarafından yönetilen anahtarlar kullanılarak otomatik olarak şifrelenir. Yedeklemeleri, müşteri tarafından yönetilen anahtarı kullanarak şifrelemeyi seçebilirsiniz. Bu durumda, anahtar kasasındaki müşterinin yönettiği anahtarın de yedekleme kapsamında olduğundan emin olun.

Yedeklemeleri ve müşteri tarafından yönetilen anahtarları korumak için Azure Backup, Azure Key Vault veya diğer kaynaklarda Azure rol tabanlı erişim denetimi kullanın. Ayrıca, yedeklemelerin değiştirilemeyeceği veya silinebilmesi için gelişmiş güvenlik özelliklerinin MFA gerektirmesini sağlayabilirsiniz.

- [Azure Backup güvenlik özelliklerine genel bakış](../../backup/security-overview.md)

- [Müşteri tarafından yönetilen anahtarları kullanarak yedekleme verilerini şifreleme](../../backup/encryption-at-rest-with-cmk.md) 

- [Azure 'da Key Vault anahtarlarını yedekleme](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey)

- [Karma yedeklemelerin saldırılara karşı korunmasına yardımcı olacak güvenlik özellikleri](../../backup/backup-azure-security-feature.md#prevent-attacks)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Olay hazırlama](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

## <a name="br-3-validate-all-backups-including-customer-managed-keys"></a>BR-3: Müşteri tarafından yönetilen anahtarlar dahil olmak üzere tüm yedeklemeleri doğrulayın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| BR-3 | 10.3 | CP-4, CP-9 |

Yedeklemeniz düzenli aralıklarla veri geri yükleme işlemini gerçekleştirin. Yedeklenen müşteri tarafından yönetilen anahtarları geri yüklemek için emin olun.

- [Azure sanal makine yedeklemesinden dosyaları kurtarma](../../backup/backup-azure-restore-files-from-vm.md)

- [Azure 'da Key Vault anahtarlarını geri yükleme](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Olay hazırlama](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="br-4-mitigate-risk-of-lost-keys"></a>BR-4: Kayıp anahtar riskini azaltma

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| BR-4 | 10,4 | CP-9 |

Anahtar kaybını engellemek ve kurtarmak için ölçülerde yer aldığından emin olun. Anahtarları yanlışlıkla veya kötü amaçlı olarak silinmeye karşı korumak için Azure Key Vault'ta geçici silme ve temizleme koruması özelliklerini etkinleştirin.

- [Key Vault'ta geçici silme ve temizleme koruması özelliklerini etkinleştirme](../../storage/blobs/soft-delete-blob-overview.md?tabs=azure-portal)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Olay hazırlama](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Veri güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-data-security)