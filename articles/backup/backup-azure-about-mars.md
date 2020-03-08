---
title: MARS Aracısı hakkında
description: MARS aracısının yedekleme senaryolarını nasıl desteklediğini öğrenin
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: d2cc8e32152f6930c9c250e2811668cc2c924616
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78673287"
---
# <a name="about-the-microsoft-azure-recovery-services-mars-agent"></a>Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı hakkında

Bu makalede, Azure Backup hizmetinin dosyaları, klasörleri ve birim ya da sistem durumunu şirket içi bir bilgisayardan Azure 'a yedeklemek ve geri yüklemek için Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı nasıl kullandığı açıklanır.

MARS Aracısı aşağıdaki yedekleme senaryolarını destekler:

![MARS yedekleme senaryoları](./media/backup-try-azure-backup-in-10-mins/backup-scenarios.png)

- **Dosyalar ve klasörler**: Windows dosya ve klasörlerini seçmeli olarak koruyun.
- **Birim düzeyi**: makinenizin bir Windows biriminin tamamını koruyun.
- **Sistem düzeyi**: tüm Windows sistem durumunu koruyun.

MARS Aracısı aşağıdaki geri yükleme senaryolarını destekler:

![MARS kurtarma senaryoları](./media/backup-try-azure-backup-in-10-mins/restore-scenarios.png)

- **Aynı sunucu**: yedeklemenin ilk olarak oluşturulduğu sunucu.
  - **Dosyalar ve klasörler**: geri yüklemek istediğiniz tek tek dosyaları ve klasörleri seçin.
  - **Birim düzeyi**: geri yüklemek istediğiniz birim ve kurtarma noktasını seçin ve aynı konuma veya aynı makinede alternatif bir konuma geri yükleyin.  Mevcut dosyaların bir kopyasını oluşturun, varolan dosyaların üzerine yazın veya var olan dosyaları kurtarmayı atlayın.
  - **Sistem düzeyi**: belirtilen konumdaki aynı makineye geri yüklemek için sistem durumu ve kurtarma noktasını seçin.

- **Alternatif sunucu**: yedeklemenin alındığı sunucudan başka bir sunucu.
  - **Dosyalar ve klasörler**: kurtarma noktasını bir hedef makineye geri yüklemek istediğiniz tek tek dosyaları ve klasörleri seçin.
  - **Birim düzeyi**: başka bir konuma geri yüklemek istediğiniz birimi ve kurtarma noktasını seçin. Mevcut dosyaların bir kopyasını oluşturun, varolan dosyaların üzerine yazın veya var olan dosyaları kurtarmayı atlayın.
  - **Sistem düzeyi**: alternatif bir makineye sistem durumu dosyası olarak geri yüklemek için sistem durumu ve kurtarma noktasını seçin.

## <a name="backup-process"></a>Yedekleme işlemi

1. Azure portal, bir [Kurtarma Hizmetleri Kasası](install-mars-agent.md#create-a-recovery-services-vault)oluşturun ve yedekleme hedeflerinden dosyalar, klasörler ve sistem durumu ' nu seçin.
2. [Kurtarma Hizmetleri Kasası kimlik bilgilerini ve Aracı yükleyicisini](https://docs.microsoft.com/azure/backup/install-mars-agent#download-the-mars-agent) şirket içi bir makineye indirin.

    Yedekleme seçeneğini belirleyip şirket içi makineyi korumak için dosyalar, klasörler ve sistem durumu ' nu seçin ve ardından MARS aracısını indirin.

3. Altyapıyı hazırlama:

    a. [Aracıyı yüklemek](https://docs.microsoft.com/azure/backup/install-mars-agent#install-and-register-the-agent)için yükleyiciyi çalıştırın.

    b. Makineyi kurtarma hizmetleri kasasına kaydetmek için, indirilen kasa kimlik bilgilerinizi kullanın.
4. İstemcideki Aracı konsolundan [yedeklemeyi yapılandırın](https://docs.microsoft.com/azure/backup/backup-windows-with-mars-agent#create-a-backup-policy). Korumaya başlamak için yedekleme verilerinizin bekletme ilkesini belirtin.

![Azure Backup aracı diyagramı](./media/backup-try-azure-backup-in-10-mins/backup-process.png)

### <a name="additional-scenarios"></a>İlave senaryolar

- **Azure sanal makineler 'de belirli dosya ve klasörleri**yedekleme: Azure sanal makinelerini (VM) yedeklemeye yönelik BIRINCIL Yöntem VM 'de bir Azure Backup uzantısı kullanmaktır. Uzantı tüm VM 'yi yedekler. Bir VM içindeki belirli dosya ve klasörleri yedeklemek istiyorsanız, Azure VM 'lerine MARS Aracısı 'nı yükleyebilirsiniz. Daha fazla bilgi için bkz. [mimari: yerleşik Azure VM yedeklemesi](https://docs.microsoft.com/azure/backup/backup-architecture#architecture-built-in-azure-vm-backup).

- **Çevrimdışı dengeli dağıtım**: Azure 'a verilerin ilk tam yedeklemeleri genellikle büyük miktarda veriyi aktarır ve daha fazla ağ bant genişliği gerektirir. Sonraki yedeklemeler yalnızca Delta veya artımlı, veri miktarını aktarır. Azure Backup ilk yedeklemeleri sıkıştırır. Azure Backup *çevrimdışı dağıtım*sürecinde, sıkıştırılmış ilk yedekleme verilerini çevrimdışı olarak Azure 'a yüklemek için diskleri kullanabilir. Daha fazla bilgi için bkz. [Azure Data Box kullanarak çevrimdışı yedekleme Azure Backup](offline-backup-azure-data-box.md).

## <a name="next-steps"></a>Sonraki adımlar

[MARS Aracısı destek matrisi](https://docs.microsoft.com/azure/backup/backup-support-matrix-mars-agent)

[MARS Aracısı SSS](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq)
