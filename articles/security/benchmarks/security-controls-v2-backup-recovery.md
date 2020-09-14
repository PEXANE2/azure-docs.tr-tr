---
title: Azure Güvenlik kıyaslama v2-yedekleme ve kurtarma
description: Azure Güvenlik kıyaslaması v2 yedekleme ve kurtarma
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/13/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: fe6861a3319b9d9c0e6535ee3303c90f0a0f26c8
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90059388"
---
# <a name="security-control-backup-and-recovery"></a>Güvenlik denetimi: yedekleme ve kurtarma

Yedekleme ve kurtarma, farklı hizmet katmanlarındaki veri ve yapılandırma yedeklemelerinin gerçekleştirilmesini, doğrulanmasını ve korunmasını sağlamak için denetimleri ele alır.

## <a name="br-1-ensure-regular-automated-backups"></a>BR-1: düzenli otomatik yedeklemeler sağlayın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| BR-1 | 10.1 | CP-2, CP4, CP-6, CP-9 |

Beklenmeyen bir olaydan sonra iş sürekliliği sağlamak için sistemleri ve verileri yedeklemediğinizden emin olun. Bu, kurtarma noktası hedefi (RPO) ve kurtarma süresi hedefi (RTO) için herhangi bir amaç tarafından rehberlik edilmelidir.

Azure Backup etkinleştirin ve yedekleme kaynağını (örn. Azure VM 'Leri, SQL Server, HANA veritabanları veya dosya paylaşımları) ve istenen sıklık ve bekletme süresini yapılandırın.  

Daha yüksek düzeyde artıklık için, coğrafi olarak yedekli depolama seçeneğini etkinleştirerek yedek verileri ikincil bir bölgeye çoğaltabilir ve çapraz bölge geri yükleme kullanarak kurtarabilirsiniz.

- [Azure Backup etkinleştirme](/azure/backup/)

- [Çapraz bölge geri yüklemeyi etkinleştirme](/azure/backup/backup-azure-arm-restore-vms#cross-region-restore)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [İlke ve standartlar](/azure/cloud-adoption-framework/organize/cloud-security-policy-standards)

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Olay hazırlama](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

## <a name="br-2-encrypt-backup-data"></a>BR-2: yedekleme verilerini şifreleme

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| BR-2 | 10.2 | CP-9 |

Yedeklemelerinizin saldırılara karşı korunmasını sağlayın. Bu, gizlilik kaybına karşı korunmak için yedeklemelerin şifrelenmesini içermelidir.   

Azure Backup kullanarak şirket içi yedekleme için, sağladığınız parola kullanılarak bekleyen şifreleme sağlanır. Normal Azure hizmeti yedeklemesi için, yedekleme verileri Azure platformu tarafından yönetilen anahtarlar kullanılarak otomatik olarak şifrelenir. Yedeklemeyi, müşteri tarafından yönetilen anahtarı kullanarak şifrelemeyi seçebilirsiniz. Bu durumda, anahtar kasasındaki müşterinin yönettiği anahtarın de yedekleme kapsamında olduğundan emin olun. 

Yedeklemeleri ve müşteri tarafından yönetilen anahtarları korumak için Azure Backup, Azure Key Vault veya diğer kaynaklarda rol tabanlı erişim denetimi kullanın. Ayrıca, yedeklemelerin değiştirilemeyeceği veya silinebilmesi için gelişmiş güvenlik özelliklerinin MFA gerektirmesini sağlayabilirsiniz.

- [Azure Backup güvenlik özelliklerine genel bakış](/azure/backup/security-overview)

- [Müşteri tarafından yönetilen anahtarları kullanarak yedekleme verilerinin şifrelenmesi](/azure/backup/encryption-at-rest-with-cmk) 

- [Azure 'da Key Vault anahtarlarını yedekleme](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Olay hazırlama](/) Azure/bulut-benimseme-çerçeve/düzenleme/bulut-güvenlik-olay hazırlığı

## <a name="br-3-validate-all-backups-including-customer-managed-keys"></a>BR-3: müşteri tarafından yönetilen anahtarlar dahil tüm yedeklemeleri doğrulama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| BR-3 | 10,3 | CP-4, CP-9 |

Yedeklemeniz düzenli aralıklarla veri geri yükleme işlemini gerçekleştirin. Yedeklenen müşteri tarafından yönetilen anahtarları geri yüklemek için emin olun.

- [Azure sanal makine yedeklemesinden dosyaları kurtarma](/azure/backup/backup-azure-restore-files-from-vm)

- [Azure 'da Key Vault anahtarlarını geri yükleme](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Olay hazırlama](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="br-4-mitigate-risk-of-lost-keys"></a>BR-4: kayıp anahtarların riskini azaltma

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| BR-4 | 10,4 | CP-9 |

Anahtar kaybını engellemek ve kurtarmak için ölçülerde yer aldığından emin olun. Anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Azure Key Vault ' de geçici silme ve Temizleme korumasını etkinleştirin.  

- [Key Vault 'da geçici silme ve Temizleme korumasını etkinleştirme](/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Olay hazırlama](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Veri güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-data-security)

