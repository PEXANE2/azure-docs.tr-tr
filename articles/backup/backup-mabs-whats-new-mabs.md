---
title: Microsoft Azure Yedekleme Sunucusu'ndaki yenilikler
description: Microsoft Azure Yedekleme Sunucusu, VM'leri, dosyaları ve klasörleri, iş yüklerini ve daha fazlasını korumak için gelişmiş yedekleme özellikleri sağlar. Azure Backup Server V3'ü nasıl yükleyip yükselteceklerini öğrenin.
ms.topic: conceptual
ms.date: 11/13/2018
ms.openlocfilehash: 61430ce06d3e441fcfe0443eaaf5de3755b04624
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77582815"
---
# <a name="whats-new-in-microsoft-azure-backup-server"></a>Microsoft Azure Yedekleme Sunucusu'ndaki yenilikler

Microsoft Azure Backup Server sürüm 3 (MABS V3) en son yükseltmedir ve kritik hata düzeltmeleri, Windows Server 2019 desteği, SQL 2017 desteği ve diğer özellikler ve geliştirmeleri içerir. Düzeltilen hataların listesini ve MABS V3 için kurulum yönergelerini görmek için KB madde [4457852'ye](https://support.microsoft.com/help/4457852/microsoft-azure-backup-server-v3)bakın.

Aşağıdaki özellikler MABS V3'e dahildir:

## <a name="volume-to-volume-migration"></a>Birimden Hacim geçişine

MABS V2'deki Modern Yedekleme Depolama (MBS) ile, depolama özelliklerine göre belirli iş yüklerini belirli depolama alanına yedeklenecek şekilde yapılandırdığınız İş Yükü ne kadar depolama alanı duyurduk. Ancak, yapılandırmadan sonra, optimize edilmiş kaynak kullanımı için belirli veri kaynaklarının yedeklerini başka depolama alanına taşıma gereksinimi bulabilirsiniz. MABS V3, yedeklemelerinizi geçirme ve [bunları üç adımda](https://techcommunity.microsoft.com/t5/system-center-blog/sc-2016-dpm-ur4-migrate-backup-storage-in-3-simple-steps/ba-p/351842)farklı bir ses düzeyine depolanacak şekilde yapılandırma olanağı sağlar.

## <a name="prevent-unexpected-data-loss"></a>Beklenmeyen veri kaybını önleme

İşletmelerde, MABS bir yönetici ekibi tarafından yönetilir. Yedeklemeler için kullanılması gereken depolama yönergeleri olmakla birlikte, yedekleme depolama olarak MABS'ye verilen yanlış bir birim kritik veri kaybına neden olabilir. MABS V3 ile, [bu powershell cmdlets](https://docs.microsoft.com/azure/backup/backup-mabs-add-storage)kullanarak depolama için kullanılamaz olanlar olarak bu birimleri yapılandırarak bu tür senaryoları önleyebilirsiniz.

## <a name="custom-size-allocation"></a>Özel boyut tahsisi

Modern Yedekleme Depolama (MBS) depolamayı gerektiği gibi ve gerektiği zaman ince bir şekilde tüketir. Bunu yapmak için, MABS, koruma için yapılandırıldığında yedeklenen verilerin boyutunu hesaplar. Ancak, bir dosya sunucusunda olduğu gibi birçok dosya ve klasör birlikte yedekleniyorsa, boyut hesaplaması uzun sürebilir. MABS V3 ile, her dosyanın boyutunu hesaplamak yerine mabs birim boyutunu varsayılan olarak kabul ederek zamandan tasarruf edebilirsiniz.

## <a name="optimized-cc-for-rct-vms"></a>RCT VM'ler için optimize edilmiş CC

MABS, VM çöktüğı senaryolarda zaman alan tutarlılık denetimlerine olan gereksinimi azaltan RCT (Hyper-V'deki yerel değişim izleme) kullanır. RCT, VSS anlık görüntü tabanlı yedeklemelerin sağladığı değişiklik izlemeden daha dayanıklıdır. MABS V3, herhangi bir tutarlılık denetimi sırasında yalnızca değiştirilen verileri aktararak ağ ve depolama tüketimini daha da optimize eder.

## <a name="support-to-tls-12"></a>TLS 1.2 desteği

TLS 1.2, Microsoft tarafından en iyi sınıf şifrelemesi ile önerilen güvenli bir iletişim yoludur. MABS artık TLS 1.2 iletişimini MABS ile korumalı sunucular arasında, sertifika tabanlı kimlik doğrulaması ve bulut yedeklemeleri için destekliyor.

## <a name="vmware-vm-protection-support"></a>VMware VM koruma desteği

VMware VM yedekleme şimdi üretim dağıtımları için desteklenir. MABS V3, VMware VM koruması için aşağıdakileri sunar:

- vCenter ve ESXi 6.5 desteği ve 5.5 ve 6.0 desteği.
- VMware VM'lerin buluta otomatik koruması. Korumalı bir klasöre yeni VMware VM'ler eklenirse, disk ve bulut için otomatik olarak korunurlar.
- VMware alternatif konum kurtarma için kurtarma verimliliği iyileştirmeleri.

## <a name="sql-2017-support"></a>SQL 2017 desteği

MABS V3, SQL 2017 ile MABS veritabanı olarak yüklenebilir. SQL sunucusunu SQL 2016'dan SQL 2017'ye yükseltebilir veya yeni yükleyebilirsiniz. Ayrıca, SQL 2017 iş yükünü hem kümelenmiş hem de kümesiz ortamda MABS V3 ile yedekleyebilirsiniz.

## <a name="windows-server-2019-support"></a>Windows Server 2019 desteği

MABS V3, Windows Server 2019'a yüklenebilir. WS2019 ile MABS V3'ü kullanmak için, MABS V3'e yüklemeden/yükseltmeden önce işletim sisteminizi WS2019'a yükseltebilir veya WS2016'da V3 yükleme/yükseltme işletim sistemi gönderinizi yükseltebilirsiniz.

MABS V3 tam sürümdür ve doğrudan Windows Server 2016, Windows Server 2019'a yüklenebilir veya MABS V2'den yükseltilebilir. Backup Server V3'e yükseltmeden veya yüklemeden önce yükleme ön koşulları hakkında bilgi edinin.
Burada [MABS](https://docs.microsoft.com/azure/backup/backup-azure-microsoft-azure-backup#software-package)için yükleme / yükseltme adımları hakkında daha fazla bilgi bulabilirsiniz.

> [!NOTE]
>
> MABS, Sistem Merkezi Veri Koruma Yöneticisi ile aynı kod tabanına sahiptir. MABS v3, Veri Koruma Yöneticisi 1807'ye eşdeğerdir.

## <a name="next-steps"></a>Sonraki adımlar

Sunucunuzu nasıl hazırlayacağınızı veya iş yükünüzü nasıl koruyacağınızı öğrenin:

- [MABS V3'te bilinen sorunlar](backup-mabs-release-notes-v3.md)
- [Yedekleme Sunucusu iş yüklerini hazırlama](backup-azure-microsoft-azure-backup.md)
- [VMware sunucusuyedeklemek için Yedek Sunucu'yı kullanma](backup-azure-backup-server-vmware.md)
- [SQL Server'ı yedeklemek için Yedekleme Sunucusu'nı kullanın](backup-azure-sql-mabs.md)
- [Yedekleme Sunucusu ile Modern Yedekleme Depolama'yı Kullanma](backup-mabs-add-storage.md)
