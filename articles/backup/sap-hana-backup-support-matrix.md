---
title: SAP HANA yedekleme destek matrisi
description: Bu makalede, Azure VM 'lerde SAP HANA veritabanlarını yedeklemek için Azure yedekleme 'yi kullandığınızda desteklenen senaryolar ve sınırlamalar hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: 0d847d2131a9a9c21fde14cae40a184de4195223
ms.sourcegitcommit: e50a39eb97a0b52ce35fd7b1cf16c7a9091d5a2a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74288236"
---
# <a name="support-matrix-for-backup-of-sap-hana-databases-on-azure-vms"></a>Azure VM 'lerinde SAP HANA veritabanlarının yedeklenmesi için destek matrisi

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
| **Coğrafyalar**                   | Avustralya Güney Doğu, Doğu Avustralya Brezilya Güney Kanada Orta, Kanada Doğu Güney Doğu Asya, Doğu Asya Doğu ABD, Doğu ABD 2, Orta Batı ABD, Batı ABD, Batı ABD 2, Orta Kuzey ABD, Orta ABD, Orta Güney ABD Hindistan Orta, Hindistan Güney Japonya Doğu, Japonya St Kore Orta, Kore Güney Kuzey Avrupa, Batı Avrupa UK Güney, UK Batı | Avustralya Orta, Avustralya Orta 2 Çin Doğu, Çin Kuzey, Çin Doğu2, Çin Kuzey 2 Batı Hindistan Fransa Orta, Fransa Güney Almanya Kuzey, Almanya Orta Batı İsviçre Kuzey, İsviçre Batı Güney Afrika Kuzey, Güney Afrika Batı, BAE Kuzey, UAE Centra l Azure Kamu bölgeleri |
| **İşletim sistemi sürümleri**            | SLES 12 with SP2, SP3 veya SP4           | SLES 15, RHEL                                                |
| **HANA sürümleri**          | Hana 1. x üzerinde SDC, HANA 2. x < = SPS04 Rev 44           | -                                                            |
| **HANA dağıtımları**       | Tek bir Azure VM üzerinde SAP HANA-yalnızca ölçeği büyütme               | Ölçeklendirme                                                    |
| **HANA örnekleri**         | Tek bir Azure VM üzerinde tek bir SAP HANA örneği – yalnızca ölçeği büyütme | Tek bir VM 'de birden çok SAP HANA örneği                  |
| **HANA veritabanı türleri**    | 2\. x üzerinde 1. x, çok veritabanı kapsayıcısı (MDC) ÜZERINDE Tek Veritabanı kapsayıcı (SDC) | HANA 1. x içinde MDC                                              |
| **HANA veritabanı boyutu**     | Sıkıştırmadan sonra 2 TB tam yedekleme boyutu (d serisi 2 TB, 4-TB RAM) |                                                              |
| **Yedekleme türleri**           | Tam, değişiklik ve günlük yedeklemeleri                           | Artımlı, anlık görüntüler                                       |
| **Geri yükleme türleri**          | Desteklenen geri yükleme türleri hakkında bilgi edinmek için SAP HANA [1642148](https://launchpad.support.sap.com/#/notes/1642148) notuna bakın |                                                              |
| **Yedekleme limitleri**          | SAP HANA örneği başına 2 TB 'a kadar tam yedekleme boyutu  |                                                              |
| **Özel yapılandırma** |                                                              | SAP HANA + dinamik katmanlama <br>  , Geçişli aracılığıyla kopyalama            |

------

> [!NOTE]
> SAP HANA yerel istemcilerden (SAP HANA Studio/kokpit/DBA kokpiti) yedekleme ve geri yükleme işlemleri şu anda desteklenmemektedir.



## <a name="next-steps"></a>Sonraki adımlar

* [Azure VM 'lerinde çalışan SAP HANA veritabanlarını nasıl yedekleyeceğinizi](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database) öğrenin
* [Azure VM 'lerinde çalışan SAP HANA veritabanlarını geri yüklemeyi](https://docs.microsoft.com/azure/backup/sap-hana-db-restore) öğrenin
* [Kullanarak yedeklenen SAP HANA veritabanlarını yönetmeyi öğrenin Azure Backup](sap-hana-db-manage.md)
* [SAP HANA veritabanlarını yedeklerken sık karşılaşılan sorunları gidermeye nasıl sorun](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot) alabileceğinizi öğrenin
