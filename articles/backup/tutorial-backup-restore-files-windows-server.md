---
title: "Öğretici: öğeleri Windows Server 'a kurtarma"
description: Bu öğreticide, Azure 'dan Windows Server 'a öğe kurtarmak için Microsoft Azure Kurtarma Hizmetleri Agent (MARS) aracısının nasıl kullanılacağını öğrenin.
ms.topic: tutorial
ms.date: 02/14/2018
ms.custom: mvc
ms.openlocfilehash: 746c901747cf1c0b87612a31fbabcb657d5c4a0c
ms.sourcegitcommit: 64ad2c8effa70506591b88abaa8836d64621e166
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88263121"
---
# <a name="recover-files-from-azure-to-a-windows-server"></a>Azure’dan Windows Server’a dosya kurtarma

Azure Backup, Windows Server’ınızın yedeklemelerinden tek tek öğelerin kurtarılmasını sağlar. Yanlışlıkla silinen dosyaları hızlı şekilde geri yüklemeniz gerekirse tek tek dosyaların kurtarılması yararlı olur. Bu öğretici, Azure'da önceden gerçekleştirdiğiniz yedeklemelerden öğeleri kurtarmak için Microsoft Azure Kurtarma Hizmetleri Aracısı (MARS) aracısını kullanabilirsiniz. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Tek tek öğelerin kurtarmasını başlatma
> * Bir kurtarma noktası seçme
> * Bir kurtarma noktasından öğeleri geri yükleme

Bu öğreticide, [bir Windows Server 'ı Azure 'A yedekleme](backup-windows-with-mars-agent.md) ve Azure 'Da Windows Server dosyalarınız için en az bir yedeklemeniz olan adımları zaten gerçekleştirdiğinizi varsaymaktadır.

## <a name="initiate-recovery-of-individual-items"></a>Tek tek öğelerin kurtarmasını başlatma

Microsoft Azure Kurtarma Hizmetleri (MARS) aracısı ile Microsoft Azure Backup adlı yardımcı bir kullanıcı arabirimi sihirbazı yüklenir. Microsoft Azure Backup sihirbazı, Azure’da depolanan kurtarma noktalarından yedekleme verilerini almak için Microsoft Azure Kurtarma Hizmetleri (MARS) aracısı ile birlikte çalışır. Windows Server’a geri yüklemek istediğiniz dosyaları veya klasörleri belirlemek için Microsoft Azure Backup sihirbazını kullanın.

1. **Microsoft Azure Backup** ek bileşenini açın. Bunu, makinenizde **Microsoft Azure Backup** aramasını yaparak bulabilirsiniz.

    ![Microsoft Azure Backup ek bileşen](./media/tutorial-backup-restore-files-windows-server/mars.png)

2. Sihirbazda, **veri kurtarma** Sihirbazı 'nı başlatmak için aracı konsolunun **Eylemler bölmesinde** **verileri kurtar** ' ı seçin.

    ![Verileri kurtar ' ı seçin](./media/tutorial-backup-restore-files-windows-server/mars-recover-data.png)

3. **Başlarken** sayfasında, **Bu sunucuyu (sunucu adı)** seçin ve **İleri ' yi**seçin.

4. **Kurtarma modunu seçin** sayfasında, **tek tek dosyalar ve klasörler** ' i seçin ve ardından **İleri** ' yi seçerek kurtarma noktası seçim işlemini başlatın.

5. **Birim ve tarih seçin** sayfasında, geri yüklemek istediğiniz dosyaları veya klasörleri içeren birimi seçin ve **bağla**' yı seçin. Bir tarih seçin ve bir kurtarma noktasına karşılık gelen açılır menüden bir saat seçin. **Kalın** yazılan tarihler, o günde en az bir kurtarma noktasının kullanılabilir olduğunu belirtir.

    ![Birim ve tarih seçin](./media/tutorial-backup-restore-files-windows-server/mars-select-date.png)

    **Bağla**' yı seçtiğinizde, kurtarma noktasını bir disk olarak kullanılabilir hale getirir Azure Backup. Göz atıp diskteki dosyaları kurtarın.

## <a name="restore-items-from-a-recovery-point"></a>Bir kurtarma noktasından öğeleri geri yükleme

1. Kurtarma birimi bağlandıktan sonra, Windows Gezgini 'ni açmak ve kurtarmak istediğiniz dosya ve klasörleri bulmak için **Araştır** ' ı seçin.

    ![Gözatmayı Seç](./media/tutorial-backup-restore-files-windows-server/mars-browse-recover.png)

    Doğrudan kurtarma biriminden dosyaları açabilir ve dosyaları doğrulayabilirsiniz.

2. Windows Gezgini 'nde, geri yüklemek istediğiniz dosya ve klasörleri kopyalayın ve sunucudaki istediğiniz konuma yapıştırın.

    ![Dosya ve klasörleri kopyala](./media/tutorial-backup-restore-files-windows-server/mars-final.png)

3. Dosya ve klasörleri geri yüklemeyi tamamladığınızda, **verileri kurtar** sihirbazının **tarama ve kurtarma dosyaları** sayfasında, çıkar ' **ı seçin.**

    ![Çıkarma seçin](./media/tutorial-backup-restore-files-windows-server/unmount-and-confirm.png)

4. Birimi çıkarmak istediğinizi onaylamak için **Evet** ' i seçin.

    Anlık görüntü çıkarıldıktan sonra, aracı konsolunun **İşler** bölmesinde **İş Tamamlandı** görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

Böylece, Windows Server verilerinin Azure’a yedeklenmesi ve geri yüklenmesi ile ilgili öğreticiler tamamlanmış olur. Azure Backup hakkında daha fazla bilgi edinmek için, şifrelenmiş sanal makinelerin yedeklenmesine yönelik PowerShell örneğine bakın.

> [!div class="nextstepaction"]
> [Şifrelenmiş sanal makineyi yedekleme](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
