---
title: SAP HANA Backup destek matrisi
description: Bu makalede, Azure VM 'lerde SAP HANA veritabanlarını yedeklemek için Azure yedekleme 'yi kullandığınızda desteklenen senaryolar ve sınırlamalar hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: 8496dc4996cac68535bfe9be30e4b5f72e2d5721
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79252447"
---
# <a name="support-matrix-for-backup-of-sap-hana-databases-on-azure-vms"></a>Azure VM’leri üzerinde SAP HANA veritabanlarının yedeklerine yönelik destek matrisi

Azure Backup, SAP HANA veritabanlarının Azure 'a yedeklenmesini destekler. Bu makalede, Azure VM 'lerinde SAP HANA veritabanlarını yedeklemek için Azure Backup kullandığınızda desteklenen senaryolar ve sınırlamalar özetlenmektedir.

> [!NOTE]
> Günlük yedekleme sıklığı artık en az 15 dakika olarak ayarlanabilir. Günlük yedeklemeleri, veritabanı için başarılı bir tam yedekleme işlemi tamamlandıktan sonra yalnızca Flow 'a başlar.

## <a name="scenario-support"></a>Senaryo desteği

| **Senaryo**               | **Desteklenen konfigürasyonlar**                                | **Desteklenmeyen yapılandırmalar**                              |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Topoloji**               | Yalnızca Azure Linux VM 'lerinde çalışan SAP HANA                    | HANA büyük örnekler (HLI)                                   |
| **Coğrafyalar**                   | **GA**<br> **Kuzey** – Orta ABD, Doğu ABD 2, Doğu ABD, Orta Kuzey ABD, Orta Güney ABD, Batı ABD 2, Orta Batı ABD, Batı ABD, Kanada Orta, Kanada Doğu, Brezilya Güney <br> **Asya Pasifik** – Avustralya Orta, Avustralya Orta 2, Avustralya Doğu, Avustralya Güneydoğu, Japonya Doğu, Japonya Batı, Kore Orta, Kore Güney, Doğu Asya, Güneydoğu Asya, Orta Hindistan, Güney Hindistan, Batı Hindistan, Çin Doğu, Çin Kuzey, Çin doğu2, Çin Kuzey 2 <br> **Avrupa** – Batı Avrupa, Kuzey Avrupa, Fransa Orta, UK Güney, UK Batı, Almanya Kuzey, Almanya Orta Batı, İsviçre Kuzey, İsviçre Batı, Orta İsviçre Kuzey <br> **Afrika/ben** -Güney Afrika Kuzey, Güney Afrika Batı, BAE Kuzey, BAE Orta  <BR>  **Azure Kamu bölgeleri** | Fransa Güney, Almanya Orta, Almanya Kuzeydoğu, US Gov IOWA |
| **İşletim sistemi sürümleri**            | SLES 12 with SP2, SP3 veya SP4; SLES 15 SP1                              | RHEL                                                |
| **HANA sürümleri**          | Hana 1. x üzerinde SDC, HANA 2. x <= SPS04 Rev 46       | -                                                            |
| **HANA dağıtımları**       | Tek bir Azure VM üzerinde SAP HANA-yalnızca ölçeği artırma. <br><br> Yüksek kullanılabilirlik dağıtımlarında iki farklı makinede bulunan düğümler ayrı veri zincirleriyle tek tek düğümler olarak değerlendirilir.               | Ölçeği genişletme <br><br> Yüksek kullanılabilirlik dağıtımlarında yedekleme, ikincil düğüme otomatik olarak yük devretmez. Yedeklemenin yapılandırılması her düğüm için ayrı olarak yapılmalıdır.                                           |
| **HANA örnekleri**         | Tek bir Azure VM üzerinde tek bir SAP HANA örneği – yalnızca ölçeği büyütme | Tek bir VM 'de birden çok SAP HANA örneği                  |
| **HANA veritabanı türleri**    | 2. x üzerinde 1. x, çok veritabanı kapsayıcısı (MDC) ÜZERINDE Tek Veritabanı kapsayıcı (SDC) | HANA 1. x içinde MDC                                              |
| **HANA veritabanı boyutu**     | 2-TB, HANA tarafından bildirildiği şekilde tam yedekleme boyutu)                   |                                                              |
| **Yedekleme türleri**           | Tam, değişiklik ve günlük yedeklemeleri                          | Artımlı, anlık görüntüler                                       |
| **Geri yükleme türleri**          | Desteklenen geri yükleme türleri hakkında bilgi edinmek için SAP HANA [1642148](https://launchpad.support.sap.com/#/notes/1642148) notuna bakın |                                                              |
| **Yedekleme limitleri**          | SAP HANA örneği başına 2 TB 'a kadar tam yedekleme boyutu         |                                                              |
| **Özel yapılandırma** |                                                              | SAP HANA + dinamik katmanlama <br>  , Geçişli aracılığıyla kopyalama        |

------

> [!NOTE]
> SAP HANA yerel istemcilerden (SAP HANA Studio/kokpit/DBA kokpiti) yedekleme ve geri yükleme işlemleri şu anda desteklenmemektedir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure VM 'lerinde çalışan SAP HANA veritabanlarını nasıl yedekleyeceğinizi](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database) öğrenin
* [Azure VM 'lerinde çalışan SAP HANA veritabanlarını geri yüklemeyi](https://docs.microsoft.com/azure/backup/sap-hana-db-restore) öğrenin
* [Kullanarak yedeklenen SAP HANA veritabanlarını yönetmeyi öğrenin Azure Backup](sap-hana-db-manage.md)
* [SAP HANA veritabanlarını yedeklerken sık karşılaşılan sorunları gidermeye nasıl sorun](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot) alabileceğinizi öğrenin
