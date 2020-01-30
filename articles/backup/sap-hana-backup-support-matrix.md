---
title: SAP HANA Backup destek matrisi
description: Bu makalede, Azure VM 'lerde SAP HANA veritabanlarını yedeklemek için Azure yedekleme 'yi kullandığınızda desteklenen senaryolar ve sınırlamalar hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: 82d844385290ab0dc2953537c1f9a3387dd7b2b2
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76842640"
---
# <a name="support-matrix-for-backup-of-sap-hana-databases-on-azure-vms"></a>Azure VM’leri üzerinde SAP HANA veritabanlarının yedeklerine yönelik destek matrisi

Azure Backup, SAP HANA veritabanlarının Azure 'a yedeklenmesini destekler. Bu makalede, Azure VM 'lerinde SAP HANA veritabanlarını yedeklemek için Azure Backup kullandığınızda desteklenen senaryolar ve sınırlamalar özetlenmektedir.

## <a name="onboard-to-the-public-preview"></a>Genel önizlemeye ekleme

Genel önizlemeye aşağıdaki şekilde katılın:

* Portalda, [Bu makaleyi izleyerek](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors#solution-3---azure-portal)abonelik kimliğinizi Kurtarma Hizmetleri hizmet sağlayıcısına kaydedin.
* PowerShell için bu cmdlet 'i çalıştırın. "Kayıtlı" olarak tamamlanmalıdır.

```PowerShell
Register-AzProviderFeature -FeatureName "HanaBackup" –ProviderNamespace Microsoft.RecoveryServices
```

> [!NOTE]
> Günlük yedekleme sıklığı artık en az 15 dakika olarak ayarlanabilir. Günlük yedeklemeleri, veritabanı için başarılı bir tam yedekleme işlemi tamamlandıktan sonra yalnızca Flow 'a başlar.

## <a name="scenario-support"></a>Senaryo desteği

| **Senaryo**               | **Desteklenen konfigürasyonlar**                                | **Desteklenmeyen yapılandırmalar**                              |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Topoloji**               | Yalnızca Azure Linux VM 'lerinde çalışan SAP HANA                    | HANA büyük örnekler (HLI)                                   |
| **Coğrafyalar**                   | **GA**<br />**Avrupa** – Batı Avrupa, Kuzey Avrupa, Fransa Orta, UK Güney, UK Batı, Almanya Kuzey, Almanya Orta Batı, İsviçre Kuzey, İsviçre Batı<br />**Asya Pasifik** – Avustralya Orta, Avustralya Orta 2, Avustralya Doğu, Avustralya Güneydoğu, Japonya Doğu, Japonya Batı, Kore Orta, Kore Güney<br /><br>**Önizleme:**<br />**Kuzey** – Orta ABD, Doğu ABD 2, Doğu ABD, Orta Kuzey ABD, Orta Güney ABD, Batı ABD 2, Orta Batı ABD, Batı ABD, Kanada Orta, Kanada Doğu <br />**Asya Pasifik** – Doğu Asya, güneydoğu asya, Orta Hindistan Güney Hindistan | Çin Doğu, Çin Kuzey, Çin Doğu2, Çin Kuzey 2, Batı Hindistan, Merkezi İsviçre Kuzey, Güney Afrika Kuzey, Güney Afrika Batı, BAE Kuzey, BAE Orta, Azure Kamu bölgeleri, Fransa Güney Brezilya Güney |
| **İşletim sistemi sürümleri**            | SLES 12 with SP2, SP3 veya SP4                                | SLES 15, RHEL                                                |
| **HANA sürümleri**          | Hana 1. x üzerinde SDC, HANA 2. x < = SPS04 Rev 44            | -                                                            |
| **HANA dağıtımları**       | Tek bir Azure VM üzerinde SAP HANA-yalnızca ölçeği büyütme               | Ölçeklendirme                                                    |
| **HANA örnekleri**         | Tek bir Azure VM üzerinde tek bir SAP HANA örneği – yalnızca ölçeği büyütme | Tek bir VM 'de birden çok SAP HANA örneği                  |
| **HANA veritabanı türleri**    | 2\. x üzerinde 1. x, çok veritabanı kapsayıcısı (MDC) ÜZERINDE Tek Veritabanı kapsayıcı (SDC) | HANA 1. x içinde MDC                                              |
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
