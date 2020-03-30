---
title: Azure Yedekleme Sunucusu ve DPM SSS
description: Bu makalede, Microsoft Azure Yedekleme Sunucusu (MABS) ve DPM (Veri Koruma Yöneticisi) ile ilgili sık sorulan soruların yanıtlarını bulun.
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 07/05/2019
ms.openlocfilehash: 35957a1e8a3d6c3d9be06d9d44dbcd47efa0e6ee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74173153"
---
# <a name="azure-backup-server-and-dpm---faq"></a>Azure Yedekleme Sunucusu ve DPM - SSS

## <a name="general-questions"></a>Genel sorular

Bu makalede, Azure Yedekleme Sunucusu ve DPM hakkında sık sorulan sorular yanıtlanmaktadır.

### <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server"></a>Bir fiziksel sunucu için Tam Kurtarma (BMR) yedeklemesi oluşturmak üzere Azure Backup Sunucusu'nu kullanabilir miyim?

Evet.

### <a name="can-i-register-the-server-to-multiple-vaults"></a>Sunucuyu birden çok kasaya kaydedebilir miyim?

Hayır. Bir DPM veya Azure Yedekleme sunucusu yalnızca bir kasaya kaydedilebilir.

### <a name="can-i-use-dpm-to-back-up-apps-in-azure-stack"></a>Azure Yığını'ndaki uygulamaları yedeklemek için DPM'yi kullanabilir miyim?

Hayır. Azure Yığınını korumak için Azure Yedekleme'yi kullanabilirsiniz, Azure Yedekleme, Azure Yığını'ndaki uygulamaları yedeklemek için DPM kullanımını desteklemez.

### <a name="if-ive-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-install-system-center-dpm-to-back-up-on-premises-workloads-to-azure"></a>Dosyalarımı ve klasörlerimi korumak için Azure Yedekleme aracısını yükledim, şirket içi iş yüklerini Azure'a yedeklemek için System Center DPM'yi yükleyebilir miyim?

Evet. Ancak önce DPM'yi ayarlamalı, sonra Azure Yedekleme aracısını yüklemeniz gerekir.  Bileşenleri bu sıraya yüklemek, Azure Yedekleme aracısının DPM ile çalışmasını sağlar. DPM'yi yüklemeden önce aracıyı yüklemek tavsiye edilmez veya desteklenmez.

### <a name="why-cant-i-add-an-external-dpm-server-after-installing-ur7-and-latest-azure-backup-agent"></a>UR7 ve en son Azure Yedekleme aracısı yükledikten sonra neden harici bir DPM sunucusu ekleyemiyorum?

Buluta karşı korunan veri kaynaklarına sahip DPM sunucuları için (Güncelleştirme Rollup 7'den önce bir güncelleştirme toplama kullanarak), Ur7'yi ve en son Azure Yedekleme aracısını yükledikten sonra harici **DPM sunucusu ekle'yi**başlatmak için en az bir gün beklemeniz gerekir. DPM koruma gruplarının meta verilerini Azure'a yüklemek için bir günlük zaman dilimi gereklidir. Koruma grubu meta verileri ilk kez bir gecelik iş aracılığıyla yüklenir.

## <a name="vmware-and-hyper-v-backup"></a>VMware ve Hyper-V yedekleme

### <a name="can-i-back-up-vmware-vcenter-servers-to-azure"></a>VMware vCenter sunucularını Azure'a yedekleyebilir miyim?

Evet. VMware vCenter Server ve ESXi ana bilgisayarlarını Azure'a yedeklemek için Azure Yedekleme Sunucusu'nı kullanabilirsiniz.

- Desteklenen sürümler hakkında [daha fazla bilgi edinin.](backup-mabs-protection-matrix.md)
- Bir VMware sunucusuyedeklemek için [aşağıdaki adımları izleyin.](backup-azure-backup-server-vmware.md)

### <a name="do-i-need-a-separate-license-to-recover-a-full-on-premises-vmwarehyper-v-cluster"></a>Tam bir şirket içi VMware/Hyper-V kümesini kurtarmak için ayrı bir lisansa ihtiyacım var mı?

VMware/Hyper-V koruması için ayrı lisanslama nız gerekmez.

- Sistem Merkezi müşterisiyseniz, VMware VM'leri korumak için Sistem Merkezi Veri Koruma Yöneticisi'ni (DPM) kullanın.
- Sistem Merkezi müşterisi değilseniz, VMware VM'leri korumak için Azure Yedekleme Sunucusu'nı (kullandıkça öde) kullanabilirsiniz.

## <a name="sharepoint"></a>SharePoint

### <a name="can-i-recover-a-sharepoint-item-to-the-original-location-if-sharepoint-is-configured-by-using-sql-alwayson-with-protection-on-disk"></a>SharePoint SQL AlwaysOn (diskte koruma ile) kullanılarak yapılandırılırsa, bir SharePoint öğesini özgün konuma kurtarabilir miyim?

Evet, öğe orijinal SharePoint sitesine kurtarılabilir.

### <a name="can-i-recover-a-sharepoint-database-to-the-original-location-if-sharepoint-is-configured-by-using-sql-alwayson"></a>SharePoint SQL AlwaysOn kullanılarak yapılandırılırsa, bir SharePoint veritabanını özgün konuma kurtarabilir miyim?

SharePoint veritabanları SQL AlwaysOn'da yapılandırıldığından, kullanılabilirlik grubu kaldırılmadığı sürece değiştirilemezler. Sonuç olarak, DPM özgün konuma bir veritabanı geri yükleyemez. Bir SQL Server veritabanını başka bir SQL Server örneğine kurtarabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Diğer SSS belgelerini okuyun:

- Azure Yedekleme Sunucusu ve DPM destek matrisi hakkında [daha fazla bilgi edinin.](backup-support-matrix-mabs-dpm.md)
- Azure Yedekleme Sunucusu ve DPM sorun giderme yönergeleri hakkında [daha fazla bilgi edinin.](backup-azure-mabs-troubleshoot.md)
