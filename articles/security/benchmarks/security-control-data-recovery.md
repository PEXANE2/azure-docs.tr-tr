---
title: Azure Güvenlik Denetimi - Veri Kurtarma
description: Azure Güvenlik Denetimi Veri Kurtarma
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 4f3e8540902809f951a441aa2fe8d00026c44d82
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81408580"
---
# <a name="security-control-data-recovery"></a>Güvenlik Denetimi: Veri Kurtarma

Tüm sistem verilerinin, yapılandırmalarının ve sırların düzenli olarak otomatik olarak yedeklenmediğinden emin olun.

## <a name="91-ensure-regular-automated-back-ups"></a>9.1: Düzenli otomatik yedeklemeler sağlayın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 9.1 | 10.1 | Müşteri |

Azure Yedekleme'yi etkinleştirin ve yedekleme kaynağını (Azure VM'ler, SQL Server veya Dosya Paylaşımları) ve istenen sıklık ve bekletme süresini yapılandırın.

- [Azure Yedekleme nasıl etkinleştirilir?](https://docs.microsoft.com/azure/backup/)

## <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Komple sistem yedeklemelerini gerçekleştirin ve müşteri yönetilen anahtarları yedekleme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 9.2 | 10.2 | Müşteri |

Azure Yedekleme'yi ve vm(ler'i) ve istenen sıklık ve bekletme sürelerini etkinleştirin. Azure Key Vault'ta müşteri yönetilen anahtarları yedekle.

- [Azure Yedekleme nasıl etkinleştirilir?](https://docs.microsoft.com/azure/backup/)

- [Azure'da anahtar kasa sıyrık anahtarlarını yedekleme](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

## <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Müşteri yönetilen anahtarlar dahil tüm yedeklemeleri doğrulayın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 9.3 | 10.3 | Müşteri |

Azure Yedekleme'deki içeriğin düzenli olarak veri geri yüklenmesini gerçekleştirebilme özelliğini sağlayın. Yedeklenen müşteri yönetilen anahtarlarının test geri yüklemesi.

- [Azure Virtual Machine yedeklemesinden dosyaları kurtarma](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm)

- [Azure'da anahtar kasa sıtuşlarını geri yükleme](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

## <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Yedeklerin ve müşteri yönetilen anahtarlarının korunmasını sağlayın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 9.4 | 10.4 | Müşteri |

Şirket içi yedekleme için, beklemedeyken şifreleme özelliği Azure'a yedekleme yaparken sağladığınız parola kullanılarak sağlanır. Azure VM'leri için, veriler Depolama Hizmeti Şifrelemesi (SSE) kullanılarak beklemedeyken şifrelenir. Yedeklemeleri ve müşteri yönetilen anahtarları korumak için rol tabanlı erişim denetimini kullanın.  

Anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Key Vault'ta Yumuşak Silme ve temizleme korumasını etkinleştirin.  Yedeklemeleri depolamak için Azure Depolama kullanılıyorsa, blobs veya blob anlık görüntüleri silindiğinde verilerinizi kaydetmek ve kurtarmak için yumuşak silme'yi etkinleştirin. 

- [Azure RBAC'ı Anlayın](https://docs.microsoft.com/azure/role-based-access-control/overview)

- [Key Vault'ta Soft-Delete ve Temizleme koruması nasıl etkinleştirilir?](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

- [Azure Depolama blobları için geçici silme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)


## <a name="next-steps"></a>Sonraki adımlar

- Sonraki Güvenlik Denetimine Bakın: [Olay Yanıtı](security-control-incident-response.md)