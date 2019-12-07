---
title: MARS Aracısı hakkında
description: MARS aracısının yedekleme senaryolarını nasıl desteklediğini öğrenin
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: c036d93c09195c0c330cfe86f307d28866131d9f
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74897331"
---
# <a name="about-the-microsoft-azure-recovery-services-mars-agent"></a>Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı hakkında

Bu makalede, Azure Backup hizmetinin dosyaları/klasörleri, hacmi veya sistem durumunu şirket içi bilgisayardan Azure 'a yedeklemek ve geri yüklemek için Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını nasıl kullandığı açıklanmaktadır.

MARS Aracısı aşağıdaki yedekleme senaryolarını destekler:

![Kurtarma Hizmetleri Kasası panosu](./media/backup-try-azure-backup-in-10-mins/backup-scenarios.png)

- **Dosyalar ve klasörler**: Windows dosya ve klasörlerini seçmeli olarak koruyun.
- **Birim düzeyi**: makinenizin bir Windows biriminin tamamını koruyun.
- **Sistem düzeyi**: tüm Windows sistem durumunu koruyun.

MARS Aracısı aşağıdaki geri yükleme senaryolarını destekler:

![Kurtarma Hizmetleri Kasası panosu](./media/backup-try-azure-backup-in-10-mins/restore-scenarios.png)

-   **Aynı sunucu**: yedeklemenin başlangıçta oluşturulduğu sunucu.
    -    **Dosyalar ve klasörler**: geri yüklemek istediğiniz dosya ve klasörleri tek tek gözatabilir ve seçebilirsiniz.
    -    **Birim düzeyi**: geri yüklemek istediğiniz birimi ve kurtarma noktasını seçebilir ve aynı makinede aynı konuma veya alternatif konuma geri yükleyebilirsiniz.  Mevcut dosyaların bir kopyasını oluşturabilir, varolan dosyaların üzerine yazabilir veya var olan dosyaları kurtarmayı atlayabilirsiniz.
    -    **Sistem düzeyi**: belirtilen konumda aynı makineye geri yüklemek için sistem durumu ve kurtarma noktasını seçebilirsiniz.


-   **Alternatif sunucu**: başka bir sunucu (örneğin, yedeğin alındığı sunucu değil).
    -    **Dosyalar ve klasörler**: kurtarma noktasını bir hedef makineye geri yüklemek istediğiniz dosya ve klasörlere gözatıp seçebilirsiniz.
    -    **Birim düzeyi**: var olan dosyaların bir kopyasını oluşturarak, varolan dosyaların üzerine yazarak veya var olan dosyaları kurtarmayı atlayarak alternatif bir konuma geri yüklemek istediğiniz birimi ve kurtarma noktasını seçebilirsiniz.
    -    Sistem **düzeyi**: sistem durumu dosyası olarak alternatif bir makineye geri yüklemek için sistem durumu ve kurtarma noktasını seçebilirsiniz.

## <a name="backup-process"></a>Yedekleme işlemi

1.  Azure portal, bir [kurtarma hizmeti Kasası](https://docs.microsoft.com/azure/backup/backup-configure-vault#create-a-recovery-services-vault) oluşturun ve yedekleme hedeflerinden dosya ve klasör ve/veya sistem durumu ' nu seçin.
2.  Kurtarma hizmeti Kasası kimlik bilgilerini ve Aracı yükleyicisini şirket içi bir makineye [indirin](https://docs.microsoft.com/azure/backup/backup-configure-vault#download-the-mars-agent) . Yedekleme seçeneğini belirleyerek şirket içi makineleri korumak için dosyalar ve klasörler ve sistem durumu ' nu seçin ve MARS aracısını indirin.
3.  Altyapıyı hazırlama:

    a.    Aracıyı [yüklemek](https://docs.microsoft.com/azure/backup/backup-configure-vault#install-and-register-the-agent) için yükleyiciyi çalıştırın.

    b.  Makineyi kurtarma hizmetleri kasasına kaydetmek için indirilen kasa kimlik bilgilerini kullanın.
4.  İstemci üzerindeki Aracı konsolundan yedeklemeyi yapılandırmak için [zamanlamayı zamanla](https://docs.microsoft.com/azure/backup/backup-configure-vault#create-a-backup-policy) ' yı kullanın. Yedekleme verilerinizin bekletme ilkesini belirtin ve korumayı başlatın.

![Kurtarma Hizmetleri Kasası panosu](./media/backup-try-azure-backup-in-10-mins/backup-process.png)


### <a name="additional-scenarios"></a>İlave senaryolar
-   **Azure VM 'de belirli dosya ve klasörleri yedekleme** -Azure sanal makinelerini (VM 'ler) yedeklemeye yönelik birincil yöntem, sanal makinede bir Azure Backup uzantısı kullanmaktır. Bu, tüm VM 'yi yedekler. Bir VM içindeki belirli dosya ve klasörleri yedeklemek istiyorsanız, Azure VM 'lerine MARS Aracısı 'nı yükleyebilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-architecture#architecture-built-in-azure-vm-backup).

-   **Çevrimdışı dengeli dağıtım** -verilerin Azure 'a ilk tam yedeklemeleri, genellikle büyük miktarda veriyi aktarır ve yalnızca Delta/artımlı aktarım yapan sonraki yedeklemelere kıyasla daha fazla ağ bant genişliği gerektirir. Azure Backup ilk yedeklemeleri sıkıştırır. Azure Backup çevrimdışı dağıtım sürecinde, sıkıştırılmış ilk yedekleme verilerini çevrimdışı olarak Azure 'a yüklemek için diskleri kullanabilir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-azure-backup-server-import-export-).


## <a name="next-steps"></a>Sonraki adımlar
[MARS Aracısı destek matrisi](https://docs.microsoft.com/azure/backup/backup-support-matrix-mars-agent)

[SSS-MARS Aracısı](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq)
