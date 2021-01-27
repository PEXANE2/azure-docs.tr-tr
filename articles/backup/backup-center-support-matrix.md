---
title: Yedekleme merkezi için destek matrisi
description: Bu makalede, her iş yükü türü için yedekleme merkezi 'nin desteklediği senaryolar özetlenmektedir
ms.topic: conceptual
ms.date: 09/07/2020
ms.openlocfilehash: 672f0e127f173260f25978497198fd7b554aa390
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98893601"
---
# <a name="support-matrix-for-backup-center"></a>Yedekleme merkezi için destek matrisi

Yedekleme merkezi [, kuruluşların her ölçekte yedeklemeleri yönetmek, izlemek, çalıştırmak ve analiz](backup-center-overview.md)etmek için tek bir cam bölmesi sağlar. Bu makalede, her iş yükü türü için yedekleme merkezi 'nin desteklediği senaryolar özetlenmektedir.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

| **Kategori** | **Senaryo**  | **Desteklenen iş yükleri**  | **Sınırlar** |
| -------------| ------------- | ----------------------- |------------|
| İzleme   | Tüm işleri görüntüle | <li> Azure Sanal Makinesi <br><br> <li> PostgreSQL için Azure veritabanı sunucusu <br><br> <li> Azure VM 'de SQL <br><br> <li> Azure VM 'de SAP HANA <br><br> <li> Azure dosyaları | <li> 7 gün için kullanıma hazır iş. <br> <li> Her filtre/açılan liste en fazla 1000 öğeyi destekler. Bu nedenle, kiracı genelinde en fazla 1000 aboneliği ve 1000 kasalarını izlemek için yedekleme merkezi kullanılabilir. |
| İzleme | Tüm yedekleme örneklerini görüntüle | <li> Azure Sanal Makinesi <br><br> <li> PostgreSQL için Azure veritabanı sunucusu <br><br> <li> Azure VM 'de SQL <br><br> <li> Azure VM 'de SAP HANA <br><br> <li> Azure Dosyaları | Yukarıdakiyle aynı |
| İzleme | Tüm yedekleme ilkelerini görüntüleme | <li> Azure Sanal Makinesi <br><br> <li> PostgreSQL için Azure veritabanı sunucusu <br><br> <li> Azure VM 'de SQL <br><br> <li> Azure VM 'de SAP HANA <br><br> <li> Azure Dosyaları | Yukarıdakiyle aynı |
| İzleme | Tüm kasaları görüntüle | <li> Azure Sanal Makinesi <br><br> <li> PostgreSQL için Azure veritabanı sunucusu <br><br> <li> Azure VM 'de SQL <br><br> <li> Azure VM 'de SAP HANA <br><br> <li> Azure Dosyaları | Yukarıdakiyle aynı |
| Eylemler | Yedeklemeyi yapılandırma | <li> Azure Sanal Makinesi <br><br> <li> PostgreSQL için Azure veritabanı sunucusu <br><br> <li> Azure VM 'de SQL <br><br> <li> Azure VM 'de SAP HANA <br><br> <li> Azure Dosyaları | [Azure VM yedeklemesi](./backup-support-matrix-iaas.md) için destek matrislerini ve [PostgreSQL Için Azure veritabanı sunucu yedeklemesini](backup-azure-database-postgresql.md#support-matrix) inceleyin |
| Eylemler | Yedekleme örneğini geri yükle | <li> Azure Sanal Makinesi <br><br> <li> PostgreSQL için Azure veritabanı sunucusu <br><br> <li> Azure VM 'de SQL <br><br> <li> Azure VM 'de SAP HANA <br><br> <li> Azure Dosyaları | [Azure VM yedeklemesi](./backup-support-matrix-iaas.md) için destek matrislerini ve [PostgreSQL Için Azure veritabanı sunucu yedeklemesini](backup-azure-database-postgresql.md#support-matrix) inceleyin |
| Eylemler | Kasa oluştur | <li> Azure Sanal Makinesi <br><br> <li> PostgreSQL için Azure veritabanı sunucusu <br><br> <li> Azure VM 'de SQL <br><br> <li> Azure VM 'de SAP HANA <br><br> <li> Azure Dosyaları | [Kurtarma Hizmetleri Kasası](./backup-support-matrix.md#vault-support) için destek matrislerini inceleyin |
| Eylemler | Yedekleme İlkesi Oluştur | <li> Azure Sanal Makinesi <br><br> <li> PostgreSQL için Azure veritabanı sunucusu <br><br> <li> Azure VM 'de SQL <br><br> <li> Azure VM 'de SAP HANA <br><br> <li> Azure Dosyaları | [Kurtarma Hizmetleri Kasası](./backup-support-matrix.md#vault-support) için destek matrislerini inceleyin |
| Eylemler | Yedekleme örneği için isteğe bağlı yedekleme yürütme | <li> Azure Sanal Makinesi <br><br> <li> PostgreSQL için Azure veritabanı sunucusu <br><br> <li> Azure VM 'de SQL <br><br> <li> Azure VM 'de SAP HANA <br><br> <li> Azure Dosyaları | [Azure VM yedeklemesi](./backup-support-matrix-iaas.md) için destek matrislerini ve [PostgreSQL Için Azure veritabanı sunucu yedeklemesini](backup-azure-database-postgresql.md#support-matrix) inceleyin |
| Eylemler | Yedekleme örneği için Yedeklemeyi Durdur | <li> Azure Sanal Makinesi <br><br> <li> PostgreSQL için Azure veritabanı sunucusu <br><br> <li> Azure VM 'de SQL <br><br> <li> Azure VM 'de SAP HANA <br><br> <li> Azure Dosyaları | [Azure VM yedeklemesi](./backup-support-matrix-iaas.md) için destek matrislerini ve [PostgreSQL Için Azure veritabanı sunucu yedeklemesini](backup-azure-database-postgresql.md#support-matrix) inceleyin |
| Insights | Yedekleme raporlarını görüntüleme | <li> Azure Sanal Makinesi <br><br> <li> Azure sanal makinesinde SQL <br><br> <li> Azure sanal makinesinde SAP HANA <br><br> <li> Azure Dosyaları <br><br> <li> System Center Data Protection Manager <br><br> <li> Azure Backup Aracısı (MARS) <br><br> <li> Azure Backup Sunucusu (MABS) | [Yedekleme raporları için desteklenen senaryolar](./configure-reports.md#supported-scenarios) bölümüne bakın |
| İdare | ' Backup ' kategorisi altında yerleşik ve özel Azure Ilkelerini görüntüleyin ve atayın | Yok | Yok |
| İdare | Yedekleme için yapılandırılmamış veri kaynaklarını görüntüle | <li> Azure Sanal Makinesi <br><br> <li> PostgreSQL için Azure veritabanı sunucusu | Yok |

## <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

| **Kategori** | **Senaryo**  |
|--------------|---------------|
| İzleme | Uyarıları ölçeklendirerek görüntüleme |
| Eylemler | Kasa ayarlarını ölçekli olarak yapılandırma |
| Eylemler | Çapraz bölge geri yükleme işini yedekleme merkezinden Çalıştır |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Backup için destek matrisini gözden geçirin](./backup-support-matrix.md)
* [Azure VM yedeklemesi için destek matrisini gözden geçirin](./backup-support-matrix-iaas.md)
* [PostgreSQL için Azure veritabanı sunucu yedeklemesi için destek matrisini gözden geçirin](backup-azure-database-postgresql.md#support-matrix)