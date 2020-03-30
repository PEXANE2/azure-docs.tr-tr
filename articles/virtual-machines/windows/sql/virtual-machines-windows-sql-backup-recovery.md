---
title: Azure VM'lerde SQL Server için Yedekleme ve Geri Yükleme | Microsoft Dokümanlar
description: Azure Sanal Makinelerde çalışan SQL Server veritabanları için yedekleme ve geri yükleme konularını açıklar.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-resource-management
ms.assetid: 95a89072-0edf-49b5-88ed-584891c0e066
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/04/2018
ms.author: mikeray
ms.openlocfilehash: e4c126bbac73accb984f1040a7fea1740d919233
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249782"
---
# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Azure Sanal Makineler’de SQL Server için Yedekleme ve Geri Yükleme

Bu makalede, Azure'da bir Windows sanal makinede çalışan SQL Server için kullanılabilir yedekleme ve geri yükleme seçenekleri hakkında kılavuz lar sağlanmaktadır. Azure Depolama, veri kaybına veya fiziksel veri bozulmasına karşı koruma sağlamak için her Azure VM diskinin üç kopyasını saklar. Bu nedenle, şirket içinde aksine, donanım hataları odaklanmak gerekmez. Ancak, yanlışlıkla veri eklemeler veya silmeler gibi uygulama veya kullanıcı hatalarına karşı korumak için SQL Server veritabanlarınızı yedeklemeye devam edebilirsiniz. Bu durumda, zaman içinde belirli bir noktaya geri edebilmek için önemlidir.

Bu makalenin ilk bölümünde kullanılabilir yedekleme ve geri yükleme seçeneklerine genel bir bakış sağlar. Bunu, her strateji hakkında daha fazla bilgi sağlayan bölümler takip ediyor.

## <a name="backup-and-restore-options"></a>Yedekleme ve geri yükleme seçenekleri

Aşağıdaki tablo, Azure VM'lerde çalışan SQL Server için çeşitli yedekleme ve geri yükleme seçenekleri hakkında bilgi sağlar:

| Strateji | SQL sürümleri | Açıklama |
|---|---|---|
| [Otomatik Yedekleme](#automated) | 2014<br/> 2016<br/> 2017 | Otomatik Yedekleme, bir SQL Server VM'deki tüm veritabanları için düzenli yedekleme zamanlayasınız. Yedeklemeler 30 güne kadar Azure depolama alanında saklanır. SQL Server 2016 ile başlayan Otomatik Yedekleme v2, manuel zamanlamayı yapılandırma ve tam ve günlük yedeklemelerinin sıklığını yapılandırma gibi ek seçenekler sunar. |
| [SQL VM'leri için Azure Backup](#azbackup) | 2008<br/> 2012<br/> 2014<br/> 2016<br/> 2017 | Azure Yedekleme, Azure VM'lerde çalışan SQL Server için Kurumsal sınıf yedekleme özelliği sağlar. Bu hizmetle, birden çok sunucu ve binlerce veritabanının yedeklerini merkezi olarak yönetebilirsiniz. Veritabanları portalda belirli bir zaman noktasına geri yüklenebilir. Yedeklemeleri yıllarca koruyabilen özelleştirilebilir bir bekletme ilkesi sunar. |
| [Manuel yedekleme](#manual) | Tümü | SQL Server sürümünüze bağlı olarak, Azure VM'de çalışan SQL Server'ı el ile yedeklemek ve geri yüklemek için çeşitli teknikler vardır. Bu senaryoda, veritabanlarınızın nasıl yedeklenme şeklinden ve bu yedeklemelerin depolama konumu ndan ve yönetiminden siz sorumlusunuz. |

Aşağıdaki bölümlerde her seçeneği daha ayrıntılı olarak açıklayınız. Bu makalenin son bölümü bir özellik matris şeklinde bir özet sağlar.

## <a name="automated-backup"></a><a id="automated"></a>Otomatik Yedekleme

Otomatik Yedekleme, Azure'da bir Windows VM'de çalışan SQL Server Standard ve Enterprise sürümleri için otomatik yedekleme hizmeti sağlar. Bu hizmet, Azure portalındaki SQL Server Windows sanal makine resimlerine otomatik olarak yüklenen [SQL Server IaaS Agent Extension](virtual-machines-windows-sql-server-agent-extension.md)tarafından sağlanır.

Tüm veritabanları, yapılandırdığınız bir Azure depolama hesabına yedeklenir. Yedeklemeler şifrelenebilir ve 30 güne kadar saklanabilir.

SQL Server 2016 ve daha yüksek VM'ler Otomatik Yedekleme v2 ile daha fazla özelleştirme seçeneği sunar. Bu iyileştirmeler şunlardır:

- Sistem veritabanı yedeklemeleri
- El ile yedekleme çizelgesi ve zaman penceresi
- Tam ve günlük dosya yedekleme frekansı

Bir veritabanını geri yüklemek için, depolama hesabında gerekli yedekleme dosyasını bulmanız ve SQL Server Management Studio (SSMS) veya Transact-SQL komutlarını kullanarak SQL VM'nizde geri yükleme gerçekleştirmeniz gerekir.

SQL VM'ler için Otomatik Yedekleme'yi yapılandırma hakkında daha fazla bilgi için aşağıdaki makalelerden birine bakın:

- **SQL Server 2016/2017**: [Azure Sanal Makineler için Otomatik Yedekleme v2](virtual-machines-windows-sql-automated-backup-v2.md)
- **SQL Server 2014**: [SQL Server 2014 Sanal Makineler için Otomatik Yedekleme](virtual-machines-windows-sql-automated-backup.md)

## <a name="azure-backup-for-sql-vms"></a><a id="azbackup"></a>SQL VM'ler için Azure Yedekleme

[Azure Yedekleme,](/azure/backup/) Azure VM'lerde çalışan SQL Server için Kurumsal sınıf yedekleme özelliği sağlar. Tüm yedeklemeler Bir Kurtarma Hizmetleri kasasında saklanır ve yönetilir. Bu çözümün özellikle İşletmeler için sağladığı çeşitli avantajlar vardır:

- **Sıfır altyapı yedeklemesi**: Yedekleme sunucularını veya depolama konumlarını yönetmeniz gerekmez.
- **Ölçek**: Birçok SQL VM'yi ve binlerce veritabanını koruyun.
- **Kullandıkça Öde**: Bu özellik Azure Yedekleme tarafından sağlanan ayrı bir hizmettir, ancak tüm Azure hizmetlerinde olduğu gibi, yalnızca kullandığınız ücretin karşılığını verirsiniz.
- **Merkezi yönetim ve izleme**: Azure Yedekleme'nin desteklediği diğer iş yükleri de dahil olmak üzere tüm yedeklemelerinizi Azure'daki tek bir panodan merkezi olarak yönetin.
- **İlke odaklı yedekleme ve bekletme**: Düzenli yedeklemeler için standart yedekleme ilkeleri oluşturun. Yedeklemeleri yıllarca korumak için bekletme ilkeleri belirleyin.
- **SQL Always On Desteği**: Bir SQL Server Always On yapılandırmasını algılayıp koruyun ve yedekleme Kullanılabilirlik Grubu yedekleme tercihini onurlandırın.
- **15 dakikalık Kurtarma Noktası Hedefi (RPO)**: SQL hareket günlüğü yedeklemelerini her 15 dakikaya kadar yapılandırın.
- **Zaman geri yüklemesi**: Birden çok tam, diferansiyel ve günlük yedeklemesini el ile geri yüklemek zorunda kalmadan veritabanlarını belirli bir noktaya kurtarmak için portalı kullanın.
- **Hatalar için birleştirilmiş e-posta uyarıları**: Tüm hatalar için birleştirilmiş e-posta bildirimlerini yapılandırın.
- **Rol tabanlı erişim denetimi**: Portal üzerinden yedekleme ve geri yükleme işlemlerini kimin yönetebileceğini belirleyin.

Bir demoyla birlikte nasıl çalıştığına hızlı bir genel bakış için aşağıdaki videoyu izleyin:

> [!VIDEO https://www.youtube.com/embed/wmbANpHos_E]

SQL VM'ler için bu Azure Yedekleme çözümü genellikle kullanılabilir. Daha fazla bilgi için SQL [Server veritabanını Azure'a yedekleyin.](../../../backup/backup-azure-sql-database.md)

## <a name="manual-backup"></a><a id="manual"></a>Manuel yedekleme

SQL VM'lerinizde yedekleme ve geri yükleme işlemlerini el ile yönetmek istiyorsanız, kullanmakta olduğunuz SQL Server sürümüne bağlı olarak çeşitli seçenekler vardır. Yedekleme ve geri yüklemeye genel bir bakış için, SQL Server sürümünüze dayalı aşağıdaki makalelerden birine bakın:

- [SQL Server 2016 ve sonrası için Yedekleme ve Geri Yükleme](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases)
- [SQL Server 2014 için Yedekleme ve Geri Yükleme](https://msdn.microsoft.com/library/ms187048%28v=sql.120%29.aspx)
- [SQL Server 2012 için Yedekleme ve Geri Yükleme](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx)
- [SQL Server SQL Server 2008 R2 için Yedekleme ve Geri Yükleme](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx)
- [SQL Server 2008 için Yedekleme ve Geri Yükleme](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx)

Aşağıdaki bölümlerde birkaç el ile yedekleme ve geri yükleme seçenekleri daha ayrıntılı olarak açıklayınız.

### <a name="backup-to-attached-disks"></a>Ekli disklere yedekleme

Azure VM'lerde çalışan SQL Server için, yedekleme dosyalarının hedefi için VM'deki ekli diskleri kullanarak yerel yedekleme ve geri yükleme tekniklerini kullanabilirsiniz. Ancak, [sanal makinenin boyutuna](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bağlı olarak bir Azure sanal makinesine ekebileceğiniz disk sayısının bir sınırı vardır. Ayrıca disk yönetiminin genel yükü de göz önünde bulundurulması gereken bir durumdur.

SQL Server Management Studio (SSMS) veya Transact-SQL kullanarak tam bir veritabanı yedeklemesinin el ile nasıl [oluşturulabildiğini](https://docs.microsoft.com/sql/relational-databases/backup-restore/create-a-full-database-backup-sql-server)öğrenmek için bkz.

### <a name="backup-to-url"></a>URL'ye yedekleme

SQL Server 2012 SP1 CU2 ile başlayarak, yedekleme yapabilir ve doğrudan URL'ye yedekleme olarak da bilinen Microsoft Azure Blob depolama alanına geri yükleyebilirsiniz. SQL Server 2016 da bu özellik için aşağıdaki geliştirmeleri tanıttı:

| 2016 Geliştirme | Ayrıntılar |
| --- | --- |
| **Şeritleme** |SQL Server 2016, Microsoft Azure blob depolamasını desteklerken, büyük veritabanlarının en fazla 12,8 TB'a kadar yedeklemesini etkinleştirmek için birden çok blob'a yedeklemeyi destekler. |
| **Anlık Görüntü Yedekleme** |SQL Server File-Snapshot Backup, Azure anlık görüntülerini kullanarak Azure Blob depolama hizmeti kullanılarak depolanan veritabanı dosyaları için neredeyse anında yedekleme ve hızlı geri yükleme sağlar. Bu özellik, yedeklemenizi basitleştirmenize ve ilkelerinizi geri yüklemenize olanak tanır. Dosya anlık görüntü yedeklemede nokta geri yüklemede de destek lenir. Daha fazla bilgi [için Azure'daki Veritabanı Dosyaları için Anlık Görüntü Yedeklemeleri'ne](https://docs.microsoft.com/sql/relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure)bakın. |

Daha fazla bilgi için, SQL Server sürümünüze dayalı aşağıdaki makalelerden birine bakın:

- **SQL Server 2016/2017**: [SQL Server Backup to URL](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service)
- **SQL Server 2014**: [SQL Server 2014 URL'ye Yedekleme](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx)
- **SQL Server 2012**: [SQL Server 2012 URL'ye Yedekleme](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx)

### <a name="managed-backup"></a>Yönetilen Yedekleme

SQL Server 2014 ile başlayarak Yönetilen Yedekleme, Azure depolamasına yedekleme oluşturulmasını otomatikleştirir. Yönetilen Yedekleme, arka planda bu makalenin önceki bölümünde açıklanan Yedekleme-URL özelliğinden kullanır. Yönetilen Yedekleme, SQL Server VM Otomatik Yedekleme hizmetini destekleyen temel özelliktir.

SQL Server 2016'dan başlayarak Yönetilen yedekleme, zamanlama, sistem veritabanı yedeklemesi ve tam ve günlük yedekleme sıklığı için ek seçenekler edindi.

Daha fazla bilgi için, SQL Server sürümünüze dayalı aşağıdaki makalelerden birine bakın:

- [SQL Server 2016 ve sonrası için Microsoft Azure'a Yönetilen Yedekleme](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure)
- [SQL Server 2014 için Microsoft Azure'a Yönetilen Yedekleme](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx)

## <a name="decision-matrix"></a>Karar matrisi

Aşağıdaki tablo, Azure'daki SQL Server sanal makineleri için her yedekleme ve geri yükleme seçeneğinin özelliklerini özetleyin.

|| **Otomatik Yedekleme** | **SQL için Azure Yedekleme** | **Manuel yedekleme** |
|---|---|---|---|
| Ek Azure hizmeti gerektirir |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Azure portalında yedekleme ilkesini yapılandırma | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Azure portalında veritabanlarını geri yükleme |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Birden çok sunucuyu tek bir panoda yönetme |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Belirli bir noktaya geri yükleme | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| 15 dakikalık Kurtarma Noktası Hedefi (RPO) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| Kısa vadeli yedekleme bekletme ilkesi (gün) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Uzun vadeli yedekleme saklama ilkesi (aylar, yıllar) |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| SQL Server Always On için yerleşik destek |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Azure Depolama hesabına yedekleme(ler) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png)(otomatik) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png)(otomatik) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png)(müşteri yönetimi) |
| Depolama ve yedekleme dosyalarının yönetimi | | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |  |
| VM'de ekli disklere yedekleme |   |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| Merkezi özelleştirilebilir yedekleme raporları |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Hatalar için konsolide e-posta uyarıları |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Azure Monitör günlüklerine göre izlemeyi özelleştirme |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| SSMS veya Transact-SQL komut dosyalarıyla yedekleme işlerini izleme | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| SSMS veya Transact-SQL komut dosyalarıyla veritabanlarını geri yükleme | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   | ![Evet](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |

## <a name="next-steps"></a>Sonraki adımlar

SQL Server'ı bir Azure VM'de dağıtımını planlıyorsanız, aşağıdaki kılavuzda sağlama kılavuzunda bulabilirsiniz: [Azure portalında Windows SQL Server sanal makine nasıl sağlanır.](virtual-machines-windows-portal-sql-server-provision.md)

Yedekleme ve geri yükleme verilerinizi geçirmek için kullanılabilse de, Azure VM'de SQL Server'a daha kolay veri geçiş yolları olabilir. Geçiş seçenekleri ve önerileri hakkında tam bir tartışma için [bkz.](virtual-machines-windows-migrate-sql.md)
