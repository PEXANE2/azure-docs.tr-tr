---
title: Azure'daki verileri Windows sunucusuna geri yükleme
description: Bu makalede, Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı ile Azure'da depolanan verileri windows sunucusuna veya Windows bilgisayarına nasıl geri yükleyebilirsiniz öğrenin.
ms.reviewer: saurse
ms.topic: conceptual
ms.date: 09/07/2018
ms.openlocfilehash: 25ca8eecaeb615f071340188a23fae7978ddb75c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79409822"
---
# <a name="restore-files-to-windows-by-using-the-azure-resource-manager-deployment-model"></a>Azure Kaynak Yöneticisi dağıtım modelini kullanarak dosyaları Windows'a geri yükleme

Bu makalede, yedekleme kasasından veri geri yükleme nasıl açıklanmaktadır. Verileri geri yüklemek için, Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısında Verileri Kurtarma sihirbazını kullanırsınız. Şunları yapabilirsiniz:

* Verileri yedeklemelerin alındığı makineye geri yükleyin.
* Verileri alternatif bir makineye geri yükleme.

Düzeltilebilir bir kurtarma noktası anlık görüntüsünü kurtarma birimi olarak takmak için Anında Geri Yükleme özelliğini kullanın. Daha sonra kurtarma düzeyini keşfedebilir ve dosyaları yerel bir bilgisayara kopyalayarak dosyaları seçerek geri alabilirsiniz.

> [!NOTE]
> Verileri geri yüklemek için Anında Geri Yükleme'yi kullanmak istiyorsanız [Ocak 2017 Azure Yedekleme güncelleştirmesi](https://support.microsoft.com/help/3216528/azure-backup-update-for-microsoft-azure-recovery-services-agent-januar) gereklidir. Ayrıca, yedekleme verileri destek makalesinde listelenen yerel tonozlarda korunmalıdır. Anında Geri Yükleme'yi destekleyen en son yerel yerel halk listesi için [Ocak 2017 Azure Yedekleme](https://support.microsoft.com/help/3216528/azure-backup-update-for-microsoft-azure-recovery-services-agent-januar) güncelleştirmesi'ne başvurun.
>

Azure portalındaki Kurtarma Hizmetleri kasalarıyla Anında Geri Yükleme'yi kullanın. Verileri Yedekleme kasalarında depoladıysanız, bunlar Kurtarma Hizmetleri kasalarına dönüştürülür. Anında Geri Yükleme'yi kullanmak istiyorsanız, MARS güncellemesini indirin ve Anında Geri Yükleme'den bahseden yordamları izleyin.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="use-instant-restore-to-recover-data-to-the-same-machine"></a>Verileri aynı makineye kurtarmak için Anında Geri Yükleme'yi kullanma

Yanlışlıkla bir dosyayı sildiyseniz ve dosyayı aynı makineye geri yüklemek istiyorsanız (yedeklemenin alındığı), aşağıdaki adımlar verileri kurtarmanıza yardımcı olur.

1. **Microsoft Azure Backup** ek bileşenini açın. Snap-in'in nerede yüklandığını bilmiyorsanız, **Microsoft Azure Yedekleme'yi**bulmak için bilgisayarı veya sunucuda arama yapın.

    Masaüstü uygulaması arama sonuçlarında görünmelidir.

2. Sihirbazı başlatmak için **Verileri Kurtar'ı** seçin.

    ![Verileri Kurtarma vurgulanırken Azure Yedekleme ekran görüntüsü](./media/backup-azure-restore-windows-server/recover.png)

3. **Başlarken** sayfasında, verileri aynı sunucuya veya bilgisayara geri yüklemek için **Bu sunucuyu`<server name>`( )** > **Sonraki'yi**seçin.

    ![Veri Kurtarma Sihirbazı'nın Başlatılanın ekran görüntüsü](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. Kurtarma **Modu'nu Seç** sayfasında, **Sonraki**dosya **ve klasörleri** > tek tek seçin.

    ![Verileri Kurtarma Sihirbazı'nın Ekran Görüntüsü Kurtarma Modu sayfasını seçin](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)
   > [!IMPORTANT]
   > Tek tek dosya ve klasörleri geri yükleme seçeneği için .NET Framework 4.5.2 veya daha sonra gerektirir. Bireysel dosya ve **klasörler** seçeneğini görmüyorsanız, .NET Framework'ü sürüm 4.5.2 veya sonraki sürümlere yükseltmeniz ve yeniden denemeniz gerekir.
 
   > [!TIP]
   > **Bireysel dosya ve klasörler** seçeneği, kurtarma noktası verilerine hızlı erişim sağlar. Boyutları toplam 80 GB'ı geçmemek üzere tek tek dosyaları kurtarmak için uygundur ve kurtarma sırasında 6 MBp'ye kadar aktarım veya kopyalama hızları sunar. **Birim** seçeneği, belirli bir birimdeki yedeklenen tüm verileri kurtarır. Bu seçenek, büyük boyutlu verileri veya tüm birimleri kurtarmak için ideal olan daha hızlı aktarım hızları (60 MBp'ye kadar) sağlar.

5. Birim **ve Tarih Seç** sayfasında, geri yüklemek istediğiniz dosya ve klasörleri içeren birimi seçin.

    Takvimde bir kurtarma noktası seçin. **Kalın** tarihler en az bir kurtarma noktasının kullanılabilirliğini gösterir. Tek bir tarih içinde birden çok kurtarma noktası varsa, **Zaman** açılır menüsünden belirli kurtarma noktasını seçin.

    ![Veri Kurtarma Sihirbazı Ekran Görüntüsü Ses ve Tarih sayfasını seçin](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. Geri yükleme için kurtarma noktasını seçtikten sonra **Mount'u**seçin.

    Azure Yedekleme, yerel kurtarma noktasını bağlar ve kurtarma birimi olarak kullanır.

7. **Dosyalara Gözat ve Kurtarma** sayfasında, Windows Gezgini'ni açmak için **Gözat'ı** seçin ve istediğiniz dosya ve klasörleri bulun.

    ![Verileri Kurtarma Sihirbazı'nın Dosyaları Gözatla ve Kurtarma sayfasının ekran görüntüsü](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)

8. Windows Gezgini'nde, geri yüklemek istediğiniz dosya ve klasörleri kopyalayın ve bunları sunucuveya bilgisayara yerel herhangi bir konuma yapıştırın. Dosyaları doğrudan kurtarma biriminden açabilir veya aktarabilir ve doğru sürümleri kurtardığınızı doğrulayabilirsiniz.

    ![Windows Explorer ekran görüntüsü, Kopya vurgulanır](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. İşi bittiğinde, **Dosyalara Gözat ve Kurtarma** sayfasında **Dağı'nı**seçin. Ardından, ses düzeyini sökmek istediğinizi doğrulamak için **Evet'i** seçin.

    ![Verileri Kurtarma Sihirbazı'nın Dosyaları Gözatla ve Kurtarma sayfasının ekran görüntüsü](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > **Unmount'u**seçmezseniz, kurtarma hacmi monte edildiği andan itibaren 6 saat boyunca monte edilmiş olarak kalır. Ancak, devam eden bir dosya kopyası durumunda montaj süresi en fazla 24 saate kadar uzatılır. Ses seviyesi takılıyken yedekleme işlemleri çalışmaz. Birimin monte edildiğinde çalışması zamanlanan herhangi bir yedekleme işlemi, kurtarma birimi söküldükten sonra çalışır.
    >

## <a name="use-instant-restore-to-restore-data-to-an-alternate-machine"></a>Verileri alternatif bir makineye geri yüklemek için Anında Geri Yükleme'yi kullanma

Sunucunuz kaybolursa, Azure Yedekleme'deki verileri başka bir makineye kurtarabilirsiniz. Aşağıdaki adımlar iş akışını gösterir.

Bu adımlar aşağıdaki terminolojiyi içerir:

* *Kaynak makinesi* – Yedeklemenin alındığı ve şu anda kullanılamayan orijinal makine.
* *Hedef makine* – Verilerin kurtarıldığı makine.
* *Örnek kasa* – Kaynak makine ve hedef makinenin kayıtlı olduğu Kurtarma Hizmetleri kasası.

> [!NOTE]
> Yedeklemeler, işletim sisteminin önceki bir sürümünü çalıştıran bir hedef makineye geri yükedilemez. Örneğin, Windows 7 bilgisayarından alınan bir yedekleme, Windows 7 (veya daha sonraki) bir bilgisayarda geri yüklenebilir. Windows 8 bilgisayarından alınan yedekleme, Windows 7 bilgisayarına geri yüklenemez.
>
>

1. Hedef makinede **Microsoft Azure Yedekleme** snap-in'i açın.

2. Hedef makinenin ve kaynak makinenin aynı Kurtarma Hizmetleri kasasına kayıtlı olduğundan emin olun.

3. Verileri Kurtar Sihirbazı'nı açmak için **Verileri Kurtar'ı** seçin. **Recover Data Wizard**

    ![Verileri Kurtarma vurgulanırken Azure Yedekleme ekran görüntüsü](./media/backup-azure-restore-windows-server/recover.png)

4. **Başlarken** sayfasında başka **bir sunucu**seçin.

    ![Veri Kurtarma Sihirbazı'nın Başlatılanın ekran görüntüsü](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. Örnek kasaya karşılık gelen kasa kimlik dosyasını sağlayın ve **İleri'yi**seçin.

    Kasa kimlik bilgisi dosyası geçersizse (veya süresi dolmuşsa), Azure portalındaki örnek kasadan yeni bir kasa kimlik dosyası indirin. Geçerli bir kasa kimlik bilgisi sağladıktan sonra, ilgili yedekleme tonozunun adı görüntülenir.

6. Yedekleme **Sunucu** seç sayfasında, görüntülenen makineler listesinden kaynak makineyi seçin ve parolayı sağlayın. Ardından **İleri'yi**seçin.

    ![Verileri Kurtarma Sihirbazı'nın Ekran Görüntüsü Yedek Sunucu sayfasını seçin](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. Kurtarma **Modu'nu Seç** sayfasında,**Sonraki**dosya **ve klasörleri** > tek tek seçin.

    ![Verileri Kurtarma Sihirbazı'nın Ekran Görüntüsü Kurtarma Modu sayfasını seçin](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. Birim **ve Tarih Seç** sayfasında, geri yüklemek istediğiniz dosya ve klasörleri içeren birimi seçin.

    Takvimde bir kurtarma noktası seçin. **Kalın** tarihler en az bir kurtarma noktasının kullanılabilirliğini gösterir. Tek bir tarih içinde birden çok kurtarma noktası varsa, **Zaman** açılır menüsünden belirli kurtarma noktasını seçin.

    ![Veri Kurtarma Sihirbazı Ekran Görüntüsü Ses ve Tarih sayfasını seçin](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. Kurtarma noktasını hedef makinenize kurtarma hacmi olarak yerel olarak monte etmek için **Mount'u** seçin.

10. **Dosyalara Gözat ve Kurtarma** sayfasında, Windows Gezgini'ni açmak için **Gözat'ı** seçin ve istediğiniz dosya ve klasörleri bulun.

    ![Verileri Kurtarma Sihirbazı'nın Dosyalara Göz Atve Kurtarma sayfasının ekran görüntüsü](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. Windows Gezgini'nde, dosyaları ve klasörleri kurtarma biriminden kopyalayın ve hedef makine konumunuza yapıştırın. Dosyaları doğrudan kurtarma biriminden açabilir veya aktarabilir ve doğru sürümlerin kurtarıldığını doğrulayabilirsiniz.

    ![Windows Explorer ekran görüntüsü, Kopya vurgulanır](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. İşi bittiğinde, **Dosyalara Gözat ve Kurtarma** sayfasında **Dağı'nı**seçin. Ardından, ses düzeyini sökmek istediğinizi doğrulamak için **Evet'i** seçin.

    ![Verileri Kurtarma Sihirbazı'nın Dosyalara Göz Atve Kurtarma sayfasının ekran görüntüsü](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > **Unmount'u**seçmezseniz, kurtarma hacmi monte edildiği andan itibaren 6 saat boyunca monte edilmiş olarak kalır. Ancak, devam eden bir dosya kopyası durumunda montaj süresi en fazla 24 saate kadar uzatılır. Ses seviyesi takılıyken yedekleme işlemleri çalışmaz. Birimin monte edildiğinde çalışması zamanlanan herhangi bir yedekleme işlemi, kurtarma birimi söküldükten sonra çalışır.
    >

## <a name="next-steps"></a>Sonraki adımlar

* Artık dosyalarınızı ve klasörlerinizi kurtardığınıza [göre, yedeklerinizi yönetebilirsiniz.](backup-azure-manage-windows-server.md)

* [Dosya ve klasörleri yedekleme yle ilgili sık sorulan soruları](backup-azure-file-folder-backup-faq.md)bulun.
