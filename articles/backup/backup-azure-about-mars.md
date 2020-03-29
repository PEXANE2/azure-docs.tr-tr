---
title: MARS Ajanı Hakkında
description: MARS Aracısı'nın yedekleme senaryolarını nasıl desteklediğini öğrenin
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: d2cc8e32152f6930c9c250e2811668cc2c924616
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78673287"
---
# <a name="about-the-microsoft-azure-recovery-services-mars-agent"></a>Microsoft Azure Kurtarma Hizmetleri (MARS) aracısı hakkında

Bu makalede, Azure Yedekleme hizmetinin dosyaları, klasörleri ve birim durumunu şirket içi bir bilgisayardan Azure'a yedeklemek ve geri yüklemek için Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını nasıl kullandığı açıklanmaktadır.

MARS aracısı aşağıdaki yedekleme senaryolarını destekler:

![MARS yedekleme senaryoları](./media/backup-try-azure-backup-in-10-mins/backup-scenarios.png)

- **Dosya ve Klasörler**: Windows dosya ve klasörlerini seçerek koruyun.
- **Ses Düzeyi**: Makinenizin tüm Windows hacmini koruyun.
- **Sistem Düzeyi**: Tüm Windows sistem durumunu koruyun.

MARS aracısı aşağıdaki geri yükleme senaryolarını destekler:

![MARS kurtarma senaryoları](./media/backup-try-azure-backup-in-10-mins/restore-scenarios.png)

- **Aynı Sunucu**: Yedeklemenin ilk oluşturulduğu sunucu.
  - **Dosya ve Klasörler**: Geri yüklemek istediğiniz tek tek dosya ve klasörleri seçin.
  - **Ses Düzeyi**: Geri yüklemek istediğiniz ses ve kurtarma noktasını seçin ve ardından aynı konuma veya aynı makinede alternatif bir konuma geri yükleyin.  Varolan dosyaların bir kopyasını oluşturun, varolan dosyaların üzerine yazın veya varolan dosyaları kurtarmayı atlayın.
  - **Sistem Düzeyi**: Belirli bir konumda aynı makineye geri yüklemek için sistem durumunu ve kurtarma noktasını seçin.

- **Alternatif Sunucu**: Yedeklemenin alındığı sunucu dışında bir sunucu.
  - **Dosya ve Klasörler**: Bir hedef makineye geri yüklemek istediğiniz kurtarma noktası tek tek dosya ve klasörleri seçin.
  - **Birim Düzeyi**: Başka bir konuma geri yüklemek istediğiniz birim ve kurtarma noktasını seçin. Varolan dosyaların bir kopyasını oluşturun, varolan dosyaların üzerine yazın veya varolan dosyaları kurtarmayı atlayın.
  - **Sistem Düzeyi**: Sistem Durumu dosyası olarak alternatif bir makineye geri yüklemek için sistem durumunu ve kurtarma noktasını seçin.

## <a name="backup-process"></a>Yedekleme işlemi

1. Azure portalından, Bir [Kurtarma Hizmetleri kasası](install-mars-agent.md#create-a-recovery-services-vault)oluşturun ve Yedekleme hedeflerinden dosyaları, klasörleri ve sistem durumunu seçin.
2. [Kurtarma Hizmetleri kasa kimlik bilgilerini ve aracı yükleyicisini](https://docs.microsoft.com/azure/backup/install-mars-agent#download-the-mars-agent) şirket içi makineye indirin.

    Yedekleme seçeneğini seçerek şirket içi makineyi korumak için dosyaları, klasörleri ve sistem durumunu seçin ve ardından MARS aracısını indirin.

3. Altyapıyı hazırlayın:

    a. Aracıyı yüklemek için [yükleyiciyi çalıştırın.](https://docs.microsoft.com/azure/backup/install-mars-agent#install-and-register-the-agent)

    b. Makineyi Kurtarma Hizmetleri kasasına kaydetmek için indirilen kasa kimlik bilgilerinizi kullanın.
4. İstemci üzerindeki aracı konsolundan [yedeklemeyi yapılandırın.](https://docs.microsoft.com/azure/backup/backup-windows-with-mars-agent#create-a-backup-policy) Yedekleme verilerinizin saklama ilkesini korumaya başlamak için belirtin.

![Azure Yedekleme aracı diyagramı](./media/backup-try-azure-backup-in-10-mins/backup-process.png)

### <a name="additional-scenarios"></a>İlave senaryolar

- **Azure sanal makineleri içindeki belirli dosya ve klasörleri yedekleme**: Azure sanal makinelerini (VM' ler) yedeklemenin birincil yöntemi, VM'de Bir Azure Yedekleme uzantısı kullanmaktır. Uzantı tüm VM'yi yedekler. VM içindeki belirli dosya ve klasörleri yedeklemek istiyorsanız, MARS aracısını Azure VM'lerine yükleyebilirsiniz. Daha fazla bilgi için [bkz: Mimari: Yerleşik Azure VM](https://docs.microsoft.com/azure/backup/backup-architecture#architecture-built-in-azure-vm-backup)Yedeklemesi.

- **Çevrimdışı tohumlama**: Verilerin Azure'a ilk tam yedeklemeleri genellikle büyük miktarda veri aktarımı sağlar ve daha fazla ağ bant genişliği gerektirir. Sonraki yedeklemeler yalnızca delta veya artımlı veri miktarını aktarıyor. Azure Yedekleme ilk yedeklemeleri sıkıştırır. Azure Yedekleme, *çevrimdışı tohumlama*işlemi sayesinde sıkıştırılmış ilk yedekleme verilerini Azure'a çevrimdışı yüklemek için diskler kullanabilir. Daha fazla bilgi için Azure [Veri Kutusu'ni kullanarak Azure Yedekleme çevrimdışı yedekleme'ye](offline-backup-azure-data-box.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[MARS aracısı destek matrisi](https://docs.microsoft.com/azure/backup/backup-support-matrix-mars-agent)

[MARS aracısı SSS](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq)
