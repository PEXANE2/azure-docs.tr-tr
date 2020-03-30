---
title: Azure Güvenlik Denetimi - Veri Kurtarma
description: Güvenlik Denetimi Veri Kurtarma
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: c585ebd903d4070f6247456e06efffbc6ec45270
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934503"
---
# <a name="security-control-data-recovery"></a>Güvenlik Denetimi: Veri Kurtarma

Tüm sistem verilerinin, yapılandırmalarının ve sırların düzenli olarak otomatik olarak yedeklenmediğinden emin olun.

## <a name="91-ensure-regular-automated-back-ups"></a>9.1: Düzenli otomatik yedeklemeler sağlayın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 9.1 | 10.1 | Müşteri |

Azure Yedekleme'yi etkinleştirin ve yedekleme kaynağını (Azure VM'ler, SQL Server veya Dosya Paylaşımları) ve istenen sıklık ve bekletme süresini yapılandırın.

Azure Yedekleme nasıl etkinleştirilir:

https://docs.microsoft.com/azure/backup/

## <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Komple sistem yedeklemelerini gerçekleştirin ve müşteri yönetilen anahtarları yedekleme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 9.2 | 10.2 | Müşteri |

Azure Yedekleme'yi ve vm(ler'i) ve istenen sıklık ve bekletme sürelerini etkinleştirin. Azure Key Vault'ta müşteri yönetilen anahtarları yedekle.

Azure Yedekleme nasıl etkinleştirilir:

https://docs.microsoft.com/azure/backup/

Azure'da anahtar kasa sıyrık anahtarlarını yedekleme:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0

## <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Müşteri yönetilen anahtarlar dahil tüm yedeklemeleri doğrulayın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 9.3 | 10.3 | Müşteri |

Azure Yedekleme'deki içeriğin düzenli olarak veri geri yüklenmesini gerçekleştirebilme özelliğini sağlayın. Gerekirse, yalıtılmış vlan için geri yükleme test edin. Yedeklenen müşteri yönetilen anahtarlarının test geri yüklemesi.

Azure Virtual Machine yedeklemesinden dosyaları kurtarma:

https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm

Azure'da anahtar kasa sıtuşlarını geri yükleme:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0

## <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Yedeklerin ve müşteri yönetilen anahtarlarının korunmasını sağlayın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 9.4 | 10.4 | Müşteri |

Şirket içi yedekleme için, beklemedeyken şifreleme özelliği Azure'a yedekleme yaparken sağladığınız parola kullanılarak sağlanır. Azure VM'leri için, veriler Depolama Hizmeti Şifrelemesi (SSE) kullanılarak beklemedeyken şifrelenir. Anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Key Vault'ta Soft-Delete'i etkinleştirebilirsiniz.

Anahtar Kasasında Soft-Delete nasıl etkinleştirilir:

https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki güvenlik denetimine bakın: [Olay Yanıtı](security-control-incident-response.md)
