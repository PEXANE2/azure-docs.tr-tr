---
title: Azure Güvenlik denetimi-veri kurtarma
description: Güvenlik denetimi veri kurtarma
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: 1cd3e39fd504b5095a83192a4a6314c71d3ca980
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75564302"
---
# <a name="security-control-data-recovery"></a>Güvenlik denetimi: veri kurtarma

Tüm sistem verilerinin, yapılandırmaların ve parolaların düzenli olarak otomatik olarak yedeklendiğinden emin olun.

## <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 9,1 | 10.1 | Müşteri |

Yedekleme kaynağını (Azure VM 'Leri, SQL Server veya dosya paylaşımları) ve istediğiniz sıklık ve bekletme süresini Azure Backup etkinleştirin ve yapılandırın.

Azure Backup etkinleştirme: https://docs.microsoft.com/azure/backup/

## <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tüm sistem yedeklemelerini gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 9.2 | 10.2 | Müşteri |

Azure Backup ve hedef VM 'leri, ayrıca istediğiniz sıklığı ve bekletme dönemlerini etkinleştirin. Müşteri tarafından yönetilen anahtarları Azure Key Vault içinde yedekleyin.

Azure Backup etkinleştirme: https://docs.microsoft.com/azure/backup/

Azure 'da Anahtar Kasası anahtarları nasıl yedekleirsiniz: https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0

## <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşterinin yönettiği anahtarlar dahil tüm yedeklemeleri doğrulama

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 9,3 | 10,3 | Müşteri |

Azure Backup içinde içeriğin düzenli olarak veri geri yüklemesini gerçekleştirme olanağı sağlayın. Gerekirse, yalıtılmış bir VLAN 'a geri yükleme testi yapın. Yedeklenen müşteri tarafından yönetilen anahtarların test geri yüklenmesi.

Azure sanal makine yedeklemesinden dosyaları kurtarma:

https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm

Azure 'da Anahtar Kasası anahtarlarını geri yükleme:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0

## <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

| Azure KIMLIĞI | CIS kimlikleri | Sorumluluk |
|--|--|--|
| 9.4 | 10,4 | Müşteri |

Şirket içi yedekleme için, beklemedeyken şifreleme özelliği Azure'a yedekleme yaparken sağladığınız parola kullanılarak sağlanır. Azure VM'leri için, veriler Depolama Hizmeti Şifrelemesi (SSE) kullanılarak beklemedeyken şifrelenir. Anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Key Vault ' de geçici silme özelliğini etkinleştirebilirsiniz.

Key Vault 'da geçici silmeyi etkinleştirme:

https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal

## <a name="next-steps"></a>Sonraki adımlar

Sonraki güvenlik denetimine bakın: [olay yanıtı](security-control-incident-response.md)
