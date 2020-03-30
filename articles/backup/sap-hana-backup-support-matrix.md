---
title: SAP HANA Backup destek matrisi
description: Bu makalede, Azure VM'lerde SAP HANA veritabanlarını yedeklemek için Azure yedeklemesini kullandığınızda desteklenen senaryolar ve sınırlamalar hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: 8496dc4996cac68535bfe9be30e4b5f72e2d5721
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252447"
---
# <a name="support-matrix-for-backup-of-sap-hana-databases-on-azure-vms"></a>Azure VM’leri üzerinde SAP HANA veritabanlarının yedeklerine yönelik destek matrisi

Azure Yedekleme, SAP HANA veritabanlarının Azure'a yedeklemesini destekler. Bu makalede, Azure VM'lerde SAP HANA veritabanlarını yedeklemek için Azure Yedekleme'yi kullandığınızda desteklenen senaryolar ve sınırlamalar özetlenmiştir.

> [!NOTE]
> Günlük yedekleme sıklığı artık en az 15 dakika olarak ayarlanabilir. Günlük yedeklemeleri yalnızca veritabanı için başarılı bir tam yedekleme tamamlandıktan sonra akmaya başlar.

## <a name="scenario-support"></a>Senaryo desteği

| **Senaryo**               | **Desteklenen yapılandırmalar**                                | **Desteklenmeyen yapılandırmalar**                              |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Topoloji**               | Yalnızca Azure Linux VM'lerinde çalışan SAP HANA                    | HANA Büyük Örnekleri (HLI)                                   |
| **Geos**                   | **Ga:**<br> **Amerika** - Orta ABD, Doğu ABD 2, Doğu ABD, Kuzey Orta ABD, Güney Orta ABD, Batı ABD 2, Batı Orta ABD, Batı ABD, Kanada Orta, Kanada Doğu, Brezilya Güney <br> **Asya Pasifik** - Avustralya Orta, Avustralya Orta 2, Avustralya Doğu, Avustralya Güneydoğu, Japonya Doğu, Japonya Batı, Kore Orta, Kore Güney, Doğu Asya, Güneydoğu Asya, Orta Hindistan, Güney Hindistan, Batı Hindistan, Çin Doğu, Çin Kuzey, Çin Doğu2, Çin Kuzey 2 <br> **Avrupa** – Batı Avrupa, Kuzey Avrupa, Fransa Merkez, İngiltere Güney, İngiltere Batı, Almanya Kuzey, Almanya Batı Orta, İsviçre Kuzey, İsviçre Batı, Orta İsviçre Kuzey <br> **Afrika / ME** - Güney Afrika Kuzey, Güney Afrika Batı, BAE Kuzey, BAE Merkez  <BR>  **Azure Kamu bölgeleri** | Fransa Güney, Almanya Merkez, Almanya Kuzeydoğu, ABD Gov IOWA |
| **İşletim sistemi sürümleri**            | SP2, SP3 veya SP4 ile SLES 12; SP1 ile SLES 15                              | RHEL                                                |
| **HANA sürümleri**          | SDC ÜZERINDE HANA 1.x, MDC ÜZERINDE HANA 2.x <= SPS04 Rev 46       | -                                                            |
| **HANA dağıtımları**       | SAP HANA tek bir Azure VM'de - Yalnızca ölçeklendirin. <br><br> Yüksek kullanılabilirlik dağıtımları için, iki farklı makinedeki her iki düğüm ayrı veri zincirlerine sahip tek tek düğümler olarak kabul edilir.               | Ölçeği genişletme <br><br> Yüksek kullanılabilirlik dağıtımlarında yedekleme otomatik olarak ikincil düğüme geçmez. Yedekleme yapılandırma her düğüm için ayrı ayrı yapılmalıdır.                                           |
| **HANA Örnekleri**         | Tek bir Azure VM'de tek bir SAP HANA örneği – yalnızca ölçeklendirin | Tek bir VM'de birden fazla SAP HANA örneği                  |
| **HANA veritabanı türleri**    | Tek Veritabanı Kapsayıcı (SDC) ON 1.x, Multi-Database Konteyner (MDC) üzerinde 2.x | MDC IÇINDE HANA 1.x                                              |
| **HANA veritabanı boyutu**     | HANA tarafından bildirilen 2-TB tam yedekleme boyutu)                   |                                                              |
| **Yedekleme türleri**           | Tam, Diferansiyel ve Günlük yedekleri                          | Artımlı, Anlık Görüntüler                                       |
| **Türleri geri yükleme**          | Desteklenen geri yükleme türleri hakkında bilgi edinmek için SAP HANA Note [1642148'e](https://launchpad.support.sap.com/#/notes/1642148) bakın |                                                              |
| **Yedekleme sınırları**          | SAP HANA örneğine göre 2 TB'a kadar tam yedekleme boyutu         |                                                              |
| **Özel yapılandırmalar** |                                                              | SAP HANA + Dinamik Seviyeleme <br>  LaMa ile Klonlama        |

------

> [!NOTE]
> SAP HANA yerel istemcilerinden (SAP HANA Studio/ Cockpit/ DBA Cockpit) yedekleme ve geri yükleme işlemleri şu anda desteklenmemektedir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure VM'lerde çalışan SAP HANA veritabanlarını nasıl yedekleriz](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database) öğrenin
* [Azure VM'lerde çalışan SAP HANA veritabanlarını](https://docs.microsoft.com/azure/backup/sap-hana-db-restore) nasıl geri yükleyin
* [Azure Yedekleme'yi kullanarak yedeklenen SAP HANA veritabanlarını nasıl yöneteceğimize](sap-hana-db-manage.md) öğrenin
* [SAP HANA veritabanlarını yedeklerken sık karşılaşılan sorunları](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot) nasıl gidereceklerini öğrenin
