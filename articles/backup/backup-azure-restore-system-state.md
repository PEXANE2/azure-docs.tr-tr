---
title: Sistem Durumunu Windows Sunucusuna Geri Yükleme
description: Windows Server System State'i Azure'daki bir yedeklemeden geri oluşturmak için adım adım açıklama.
ms.reviewer: saurse
ms.topic: conceptual
ms.date: 08/18/2017
ms.openlocfilehash: 6d46a091a4e620e26d05735f12a201009663e65d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77602457"
---
# <a name="restore-system-state-to-windows-server"></a>Sistem Durumunu Windows Server'a Geri Yükleme

Bu makalede, Windows Server System State yedeklemelerinin Azure Kurtarma Hizmetleri kasasından nasıl geri yüklenir. Sistem Durumunu geri yüklemek için bir Sistem Durumu yedeklemeniz olmalıdır [(Sistem Durumunu Yedekleme](backup-azure-system-state.md#back-up-windows-server-system-state)yönergeleri kullanılarak oluşturulmuştur ve [Microsoft Azure Kurtarma Hizmetleri (MARS) aracısının en son sürümünü](https://aka.ms/azurebackup_agent)yüklediğinizden emin olun. Windows Server System State verilerini Azure Kurtarma Hizmetleri kasasından kurtarmak iki aşamalı bir işlemdir:

1. Sistem Durumunu Azure Yedekleme'den dosya olarak geri yükleyin. System State'i Azure Yedekleme'den dosya olarak geri gönderirken şunları yapabilirsiniz:
   * Sistem Durumunu yedeklemelerin alındığı aynı sunucuya geri yükleme veya
   * System State dosyasını alternatif bir sunucuya geri yükleyin.

2. Geri yüklenen System State dosyalarını bir Windows Server'a uygulayın.

## <a name="recover-system-state-files-to-the-same-server"></a>Sistem Durumu dosyalarını aynı sunucuda kurtarma

Aşağıdaki adımlar, Windows Server yapılandırmanızı önceki bir duruma nasıl geri döndüreceklerini açıklar. Sunucu yapılandırmanızı bilinen, kararlı bir duruma geri getirmek son derece değerli olabilir. Aşağıdaki adımlar, sunucunun Sistem Durumunu Kurtarma Hizmetleri kasasından geri yüklenir.

1. **Microsoft Azure Backup** ek bileşenini açın. Snap-in'in nerede yüklandığını bilmiyorsanız, **Microsoft Azure Yedekleme'yi**bulmak için bilgisayarı veya sunucuda arama yapın.

    Masaüstü uygulaması arama sonuçlarında görünmelidir.

2. Sihirbazı başlatmak için **Verileri Kurtar'ı** tıklatın.

    ![Verileri Kurtarma](./media/backup-azure-restore-windows-server/recover.png)

3. **Başlarken** bölmede, verileri aynı sunucuya veya bilgisayara geri yüklemek için **Bu`<server name>`sunucuyu ( )** seçin ve **İleri'yi**tıklatın.

    ![Verileri aynı makineye geri yüklemek için bu sunucu seçeneğini seçin](./media/backup-azure-restore-system-state/samemachine.png)

4. Kurtarma **Modu'nu Seç** bölmesinde **Sistem Durumu'nu** seçin ve sonra **İleri'yi**tıklatın.

    ![Dosyalara göz atın](./media/backup-azure-restore-system-state/recover-type-selection.png)

5. Birim ve **Tarih** seç bölmesinde takvimde bir kurtarma noktası seçin.

    Zaman içinde herhangi bir kurtarma noktasından geri yükleyebilirsiniz. **Kalın** tarihler en az bir kurtarma noktasının kullanılabilirliğini gösterir. Bir tarih seçtikten sonra, birden çok kurtarma noktası varsa, **Zaman** açılır menüsünden belirli kurtarma noktasını seçin.

    ![Hacim ve Tarih](./media/backup-azure-restore-system-state/select-date.png)

6. Geri yüklemeiçin kurtarma noktasını seçtikten sonra **İleri'yi**tıklatın.

    Azure Yedekleme, yerel kurtarma noktasını bağlar ve kurtarma birimi olarak kullanır.

7. Bir sonraki bölmede, kurtarılan System State dosyalarının hedefini belirtin ve Windows Gezgini'ni açmak ve istediğiniz dosya ve klasörleri bulmak için **Gözat'ı** tıklatın. Seçenek, Her iki sürümü var, tüm Sistem Durumu arşivinin kopyasını oluşturmak yerine varolan bir System State dosya arşivinde tek tek dosyaların kopyalarını **oluşturur, böylece kopyaoluşturma.**

    ![Kurtarma seçenekleri](./media/backup-azure-restore-system-state/recover-as-files.png)

8. **Onay** bölmesindeki kurtarma ayrıntılarını doğrulayın ve **Kurtar'ı**tıklatın.

   ![kurtarma eylemini kabul etmek için Kurtar'ı tıklatın](./media/backup-azure-restore-system-state/confirm-recovery.png)

9. Kurtarma hedefindeki *WindowsImageBackup* dizinini sunucunun kritik olmayan bir birimine kopyalayın. Genellikle, Windows işletim sistemi birimi kritik birimdir.

10. Kurtarma başarılı olduktan sonra, bölümdeki adımları izleyin, [Geri yüklenen System State dosyalarını Windows Server'a uygulayın](backup-azure-restore-system-state.md), System State kurtarma işlemini tamamlamak için.

## <a name="recover-system-state-files-to-an-alternate-server"></a>Sistem Durumu dosyalarını alternatif bir sunucuya kurtarma

Windows Server'ınız bozuksa veya erişilemiyorsa ve Windows Server System State'i kurtararak onu kararlı bir duruma geri yüklemek istiyorsanız, bozuk sunucunun Sistem Durumunu başka bir sunucudan geri yükleyebilirsiniz. Ayrı bir sunucuda System State'i geri yüklemek için aşağıdaki adımları kullanın.  

Bu adımlarda kullanılan terminoloji şunları içerir:

* *Kaynak makinesi* – Yedeklemenin alındığı ve şu anda kullanılamayan orijinal makine.
* *Hedef makine* – Verilerin kurtarıldığı makine.
* *Örnek kasa* – Kaynak makinesi ve *Hedef makinenin* kayıtlı olduğu Kurtarma Hizmetleri kasası. *Source machine* <br/>

> [!NOTE]
> Bir makineden alınan yedeklemeler işletim sisteminin önceki bir sürümünü çalıştıran bir makineye geri yüklenemez. Örneğin, Bir Windows Server 2016 makinesinden alınan yedeklemeler Windows Server 2012 R2'ye geri yüklenemez. Ancak, ters mümkündür. Windows Server 2016'yı geri yüklemek için Windows Server 2012 R2 yedeklerini kullanabilirsiniz.
>

1. *Hedef makinesinde* **Microsoft Azure Yedekleme** snap-in'i açın.
2. *Hedef makine* ve *Kaynak makinesinin* aynı Kurtarma Hizmetleri kasasına kayıtlı olduğundan emin olun.
3. İş akışını başlatmak için **Verileri Kurtar'ı** tıklatın.
4. **Başka bir sunucu** seçin

    ![Başka bir Sunucu](./media/backup-azure-restore-system-state/anotherserver.png)

5. *Örnek kasasına*karşılık gelen kasa kimlik dosyasını sağlayın. Kasa kimlik bilgileri dosyası geçersizse (veya süresi dolmuşsa), Azure portalındaki *Örnek kasasından* yeni bir kasa kimlik bilgileri dosyasını indirin. Kasa kimlik bilgileri dosyası sağlandıktan sonra, kasa kimlik bilgileri dosyasıyla ilişkili Kurtarma Hizmetleri kasası görüntülenir.

6. Yedekleme Sunucusu Seç bölmesindeki, görüntülenen makineler listesinden *Kaynak makineyi* seçin.
7. Kurtarma Modu'nu Seç bölmesine Sistem **Durumu'nu** seçin ve **İleri'yi**tıklatın.

    ![Search](./media/backup-azure-restore-system-state/recover-type-selection.png)

8. **Birim ve Tarih seç** bölmesindeki Takvim'de bir kurtarma noktası seçin. Zaman içinde herhangi bir kurtarma noktasından geri yükleyebilirsiniz. **Kalın** tarihler en az bir kurtarma noktasının kullanılabilirliğini gösterir. Bir tarih seçtikten sonra, birden çok kurtarma noktası varsa, **Zaman** açılır menüsünden belirli kurtarma noktasını seçin.

    ![Öğeleri ara](./media/backup-azure-restore-system-state/select-date.png)

9. Geri yüklemeiçin kurtarma noktasını seçtikten sonra **İleri'yi**tıklatın.

10. Sistem **Durumu Kurtarma Modu'nu Seç** bölmesinde, Sistem Durumu dosyalarının kurtarılmasını istediğiniz hedefi belirtin ve **ardından İleri'yi**tıklatın.

    ![Şifreleme](./media/backup-azure-restore-system-state/recover-as-files.png)

    Seçenek, Her iki sürümü var, tüm Sistem Durumu arşivinin kopyasını oluşturmak yerine varolan bir System State dosya arşivinde tek tek dosyaların kopyalarını **oluşturur, böylece kopyaoluşturma.**

11. Onay bölmesindeki kurtarma ayrıntılarını doğrulayın ve **Kurtar'ı**tıklatın.

    ![kurtarma işlemini onaylamak için Kurtar düğmesini tıklatın](./media/backup-azure-restore-system-state/confirm-recovery.png)

12. *WindowsImageBackup* dizinini sunucunun kritik olmayan bir birimine kopyalayın\)(örneğin D: . Genellikle Windows işletim sistemi birimi kritik birimdir.

13. Kurtarma işlemini tamamlamak için, [geri yüklenen System State dosyalarını Windows Server'a uygulamak](#apply-restored-system-state-on-a-windows-server)için aşağıdaki bölümü kullanın.

## <a name="apply-restored-system-state-on-a-windows-server"></a>Windows Server’da geri yüklenen Sistem Durumunu uygulama

System State'i Azure Kurtarma Hizmetleri Aracısı'nı kullanarak dosya olarak kurtardıktan sonra, kurtarılan System State'i Windows Server'a uygulamak için Windows Server Yedekleme yardımcı programını kullanın. Windows Server Yedekleme yardımcı programı zaten sunucuda kullanılabilir. Aşağıdaki adımlar, kurtarılan Sistem Durumu'nun nasıl uygulanacağı açıklanır.

1. *Dizin Hizmetleri Onarım Modunda*sunucunuzu yeniden başlatmak için aşağıdaki komutları kullanın. Yükseltilmiş bir komut isteminde:

    ```cmd
    Bcdedit /set safeboot dsrepair
    Shutdown /r /t 0
    ```

2. Yeniden başlatmadan sonra Windows Server Backup snap-in'i açın. Snap-in'in nerede kurulduğunu bilmiyorsanız, **Windows Server Backup**için bilgisayarı veya sunucuda arama yapın.

    Masaüstü uygulaması arama sonuçlarında görünür.

3. Snap-in'de **Yerel Yedekleme'yi**seçin.

    ![oradan geri yüklemek için Yerel Yedekleme'yi seçin](./media/backup-azure-restore-system-state/win-server-backup-local-backup.png)

4. Yerel Yedekleme konsolunda, **Eylemler Bölmesi'nde**Kurtarma Sihirbazı'nı açmak için **Kurtar'ı** tıklatın.

5. Seçeneği seçin, **Başka bir konumda depolanan bir yedekleme**ve **İleri'yi**tıklatın.

   ![farklı bir sunucuya kurtarmak için seçin](./media/backup-azure-restore-system-state/backup-stored-in-diff-location.png)

6. Konum türünü belirtirken, System State yedeklemeniz başka bir sunucuda kurtarıldıysa **Uzaktan paylaşılan klasörü** seçin. Sistem Durumunuz yerel olarak kurtarıldıysa, **Yerel sürücüleri**seçin.

    ![yerel sunucudan mı yoksa başka bir sunucudan mı kurtarma yı tedavi edemeyeceğini seçin](./media/backup-azure-restore-system-state/ss-recovery-remote-shared-folder.png)

7. *WindowsImageBackup* dizinine giden yolu girin veya Azure Kurtarma Hizmetleri Aracısı'nı kullanarak Sistem Durumu dosyaları kurtarmanın bir parçası olarak kurtarılan bu dizini (örneğin, D:\WindowsImageBackup) içeren yerel sürücüyü seçin ve **İleri'yi**tıklatın.

    ![paylaşılan dosyaya giden yol](./media/backup-azure-restore-system-state/ss-recovery-remote-folder.png)

8. Geri yüklemek istediğiniz Sistem Durumu sürümünü seçin ve **İleri'yi**tıklatın.

9. Kurtarma Türü Seç bölmesinde **Sistem Durumu'nu** seçin ve **İleri'yi**tıklatın.

10. Sistem Durumu Kurtarma konumu için **Özgün Konum'u**seçin ve **İleri'yi**tıklatın.

11. Onay ayrıntılarını gözden geçirin, yeniden başlatma ayarlarını doğrulayın ve geri yüklenen System State dosyalarını uygulamak için **Kurtar'ı** tıklatın.

    ![geri yükleme Sistemi Durumu dosyalarını başlatın](./media/backup-azure-restore-system-state/launch-ss-recovery.png)

## <a name="special-considerations-for-system-state-recovery-on-active-directory-server"></a>Active Directory sunucusunda System State kurtarma için özel hususlar

System State yedekleme, Etkin Dizin verilerini içerir. Etkin Dizin Etki Alanı Hizmetini (AD DS) geçerli durumundan önceki durumuna geri yüklemek için aşağıdaki adımları kullanın.

1. Dizin Hizmetleri Geri Yükleme Modu'nda (DSRM) etki alanı denetleyicisini yeniden başlatın.
2. AD DS'yi kurtarmak için Windows Server Backup cmdlets'i kullanmak için [aşağıdaki](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/ad-forest-recovery-nonauthoritative-restore) adımları izleyin.

## <a name="troubleshoot-failed-system-state-restore"></a>Başarısız Sistem Durumu geri yükleme sorunlarını giderme

System State'i uygulama işlemi başarıyla tamamlanmamışsa, Windows Server'ınızı kurtarmak için Windows Kurtarma Ortamını (Win RE) kullanın. Aşağıdaki adımlar, Win RE'yi kullanarak nasıl kurtarılanın yapılacağını açıklar. Bu seçeneği yalnızca Windows Server sistem durumu geri yüklemesi sonrasında normal olarak önyükleme yapmıyorsa kullanın. Aşağıdaki işlem sistem dışı verileri siler, dikkatli olun.

1. Windows Server'ınızı Windows Kurtarma Ortamına Önyükleme (Win RE).

2. Kullanılabilir üç seçenekten Sorun Giderme'yi seçin.

    ![açma menüsü](./media/backup-azure-restore-system-state/winre-1.png)

3. Gelişmiş **Seçenekler** ekranından **Komut İstemi'ni** seçin ve sunucu yöneticisikullanıcı adı ve parolasını sağlayın.

   ![açma menüsü](./media/backup-azure-restore-system-state/winre-2.png)

4. Sunucu yöneticisi kullanıcı adı ve parolasağlayın.

    ![açma menüsü](./media/backup-azure-restore-system-state/winre-3.png)

5. Yönetici modunda komut istemini açtığınızda, System State yedekleme sürümlerini almak için aşağıdaki komutu çalıştırın.

    ```cmd
    Wbadmin get versions -backuptarget:<Volume where WindowsImageBackup folder is copied>:
    ```

    ![System State yedekleme sürümlerini alın](./media/backup-azure-restore-system-state/winre-4.png)

6. Yedeklemede bulunan tüm birimleri almak için aşağıdaki komutu çalıştırın.

    ```cmd
    Wbadmin get items -version:<copy version from above step> -backuptarget:<Backup volume>
    ```

    ![System State yedekleme sürümlerini alın](./media/backup-azure-restore-system-state/winre-5.png)

7. Aşağıdaki komut, Sistem Durumu Yedeklemesinin parçası olan tüm birimleri kurtarır. Bu adımın yalnızca Sistem Durumu'nun bir parçası olan kritik birimleri kurtardığını unutmayın. Sistem dışı tüm veriler silinir.

    ```cmd
    Wbadmin start recovery -items:C: -itemtype:Volume -version:<Backupversion> -backuptarget:<backup target volume>
    ```

     ![System State yedekleme sürümlerini alın](./media/backup-azure-restore-system-state/winre-6.png)

## <a name="next-steps"></a>Sonraki adımlar

* Artık dosyalarınızı ve klasörlerinizi kurtardığınıza [göre, yedeklerinizi yönetebilirsiniz.](backup-azure-manage-windows-server.md)
