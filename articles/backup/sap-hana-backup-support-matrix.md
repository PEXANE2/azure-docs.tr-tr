---
title: SAP HANA Backup destek matrisi
description: Bu makalede, Azure VM 'lerde SAP HANA veritabanlarını yedeklemek için Azure yedekleme 'yi kullandığınızda desteklenen senaryolar ve sınırlamalar hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: 472bbfa09bff135fa9642c097974436241c433c6
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85959730"
---
# <a name="support-matrix-for-backup-of-sap-hana-databases-on-azure-vms"></a>Azure VM’leri üzerinde SAP HANA veritabanlarının yedeklerine yönelik destek matrisi

Azure Backup, SAP HANA veritabanlarının Azure 'a yedeklenmesini destekler. Bu makalede, Azure VM 'lerinde SAP HANA veritabanlarını yedeklemek için Azure Backup kullandığınızda desteklenen senaryolar ve sınırlamalar özetlenmektedir.

> [!NOTE]
> Günlük yedekleme sıklığı artık en az 15 dakika olarak ayarlanabilir. Günlük yedeklemeleri, veritabanı için başarılı bir tam yedekleme işlemi tamamlandıktan sonra yalnızca Flow 'a başlar.

## <a name="scenario-support"></a>Senaryo desteği

| **Senaryo**               | **Desteklenen konfigürasyonlar**                                | **Desteklenmeyen yapılandırmalar**                              |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Topoloji**               | Yalnızca Azure Linux VM 'lerinde çalışan SAP HANA                    | HANA büyük örnekler (HLI)                                   |
| **Bölgeler**                   | **GA**<br> **Kuzey** – Orta ABD, Doğu ABD 2, Doğu ABD, Orta Kuzey ABD, Orta Güney ABD, Batı ABD 2, Orta Batı ABD, Batı ABD, Kanada Orta, Kanada Doğu, Brezilya Güney <br> **Asya Pasifik** – Avustralya Orta, Avustralya Orta 2, Avustralya Doğu, Avustralya Güneydoğu, Japonya Doğu, Japonya Batı, Kore Orta, Kore Güney, Doğu Asya, Güneydoğu Asya, Orta Hindistan, Güney Hindistan, Batı Hindistan, Çin Doğu, Çin Kuzey, Çin doğu2, Çin Kuzey 2 <br> **Avrupa** – Batı Avrupa, Kuzey Avrupa, Fransa Orta, UK Güney, UK Batı, Almanya Kuzey, Almanya Orta Batı, İsviçre Kuzey, İsviçre Batı, Orta İsviçre Kuzey, Norveç Doğu, Norveç Batı <br> **Afrika/ben** -Güney Afrika Kuzey, Güney Afrika Batı, BAE Kuzey, BAE Orta  <BR>  **Azure Kamu bölgeleri** | Fransa Güney, Almanya Orta, Almanya Kuzeydoğu, US Gov IOWA |
| **İşletim sistemi sürümleri**            | SLES 12 with SP2, SP3 ve SP4; SP0 ve SP1 ile SLES 15 <br><br>   **Önizleme** -RHEL 7,4, 7,6, 7,7 ve 8,1  <br>     RHEL için SAP HANA yedekleme önizlemesine (7,4, 7,6, 7,7 ve 8,1 [) başlayın.](https://docs.microsoft.com/azure/backup/tutorial-backup-sap-hana-db) Daha fazla sorgu için, adresinden bize yazın [AskAzureBackupTeam@microsoft.com](mailto:AskAzureBackupTeam@microsoft.com) .                |                                             |
| **HANA sürümleri**          | Hana 1. x üzerinde SDC, HANA 2. x <= SPS04 Rev 48       | -                                                            |
| **HANA dağıtımları**       | Tek bir Azure VM üzerinde SAP HANA-yalnızca ölçeği artırma. <br><br> Yüksek kullanılabilirlik dağıtımlarında iki farklı makinede bulunan düğümler ayrı veri zincirleriyle tek tek düğümler olarak değerlendirilir.               | Ölçeği genişletme <br><br> Yüksek kullanılabilirlik dağıtımlarında yedekleme, ikincil düğüme otomatik olarak yük devretmez. Yedeklemenin yapılandırılması her düğüm için ayrı olarak yapılmalıdır.                                           |
| **HANA örnekleri**         | Tek bir Azure VM üzerinde tek bir SAP HANA örneği – yalnızca ölçeği büyütme | Tek bir VM 'de birden çok SAP HANA örneği                  |
| **HANA veritabanı türleri**    | 2. x üzerinde 1. x, çok veritabanı kapsayıcısı (MDC) ÜZERINDE Tek Veritabanı kapsayıcı (SDC) | HANA 1. x içinde MDC                                              |
| **HANA veritabanı boyutu**     | <= 2 TB boyutundaki HANA veritabanları (Bu, HANA sisteminin bellek boyutu değildir)               |                                                              |
| **Yedekleme türleri**           | Tam, değişiklik ve günlük yedeklemeleri                          | Artımlı, anlık görüntüler                                       |
| **Geri yükleme türleri**          | Desteklenen geri yükleme türleri hakkında bilgi edinmek için SAP HANA [1642148](https://launchpad.support.sap.com/#/notes/1642148) notuna bakın |                                                              |
| **Yedekleme limitleri**          | SAP HANA örneği başına 2 TB 'a kadar tam yedekleme boyutu         |                                                              |
| **Özel yapılandırma** |                                                              | SAP HANA + dinamik katmanlama <br>  , Geçişli aracılığıyla kopyalama        |

------

>[!NOTE]
>Azure Backup, bir Azure VM 'de çalışan bir SAP HANA Veritabanının yedeklenmesinde gün ışığından yararlanma saati değişikliklerini otomatik olarak ayarlamaz.
>
>İlkeyi gerektiği şekilde el ile değiştirin.


> [!NOTE]
> Artık Azure portal HANA yerel istemcilerinden (SAP HANA Studio/kokpit/DBA kokpiti) tetiklenen [yedekleme ve geri yükleme](https://docs.microsoft.com/azure/backup/sap-hana-db-manage#monitor-manual-backup-jobs-in-the-portal) (aynı makineye) işlerini izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure VM 'lerinde çalışan SAP HANA veritabanlarını nasıl yedekleyeceğinizi](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database) öğrenin
* [Azure VM 'lerinde çalışan SAP HANA veritabanlarını geri yüklemeyi](https://docs.microsoft.com/azure/backup/sap-hana-db-restore) öğrenin
* [Kullanarak yedeklenen SAP HANA veritabanlarını yönetmeyi öğrenin Azure Backup](sap-hana-db-manage.md)
* [SAP HANA veritabanlarını yedeklerken sık karşılaşılan sorunları gidermeye nasıl sorun](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot) alabileceğinizi öğrenin
