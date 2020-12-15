---
title: Azure VM dosya kurtarma sorunlarını giderme
description: Azure VM yedeğinden dosya ve klasörleri kurtarmaya özgü sorunları giderin.
ms.reviewer: srinathv
ms.topic: troubleshooting
ms.date: 07/12/2020
ms.openlocfilehash: 96876415405cc5099b4d9ca209c36086b793d6e0
ms.sourcegitcommit: 63d0621404375d4ac64055f1df4177dfad3d6de6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97513413"
---
# <a name="troubleshooting-issues-in-file-recovery-of-azure-vm-backup"></a>Azure VM yedeklemesi 'nin dosya kurtarmasında sorun giderme sorunları

Bu makalede, Azure VM yedeğinden dosya ve klasörleri kurtarmaya özgü sorunlarla ilgili Azure Backup hataları çözmenize yardımcı olabilecek sorun giderme adımları sunulmaktadır. 

## <a name="exception-caught-while-connecting-to-target"></a>Hedefe bağlanılırken özel durum yakalandı

Olası neden: komut dosyası önerilen kurtarma noktasına erişemiyor eylem: makinenin tüm [erişim gereksinimlerini](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-4-access-requirements-to-successfully-run-the-script)yerine getirip getirmediğini denetleyin.

## <a name="the-target-has-already-been-logged-in-via-an-iscsi-session"></a>Hedef, bir Iscsı oturumu aracılığıyla zaten oturum açtı

Olası neden: komut dosyası aynı makinede zaten yürütüldü ve sürücüler eklenmiş.
Önerilen eylem: kurtarma noktası birimleri zaten eklenmiş. Özgün VM 'nin aynı sürücü harflerine bağlı olmayabilir. Dosya Gezgini 'nde kullanılabilir tüm birimlere göz atabilirsiniz.

## <a name="this-script-is-invalid-because-the-disks-have-been-dismounted-via-portalexceeded-the-12-hr-limit-download-a-new-script-from-the-portal"></a>Diskler Portal üzerinden çıkartılırsa/12-hr sınırını aştığından bu betik geçersizdir. Portaldan yeni bir betiği indirme

Olası neden: diskler portaldan çıkarıldı veya 12 saatlik sınır aşıldı.
Önerilen eylem: betik, indirildiği zamandan itibaren 12 saat sonra geçersizdir ve yürütülemez. Portalı ziyaret edin ve dosya kurtarmaya devam etmek için yeni bir betik indirin.

## <a name="troubleshooting-common-issues"></a>Genel sorunları giderme

Bu bölümde, dosya kurtarma için betiği indirme ve yürütme sırasında karşılaşılan sorunları gidermeye yönelik adımlar sağlanmaktadır.

## <a name="cannot-download-the-script"></a>Betik indirilemiyor

Önerilen eylem:

1. [Betiği indirmek için gereken tüm izinlere](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#select-recovery-point-who-can-generate-script)sahip olduğunuzdan emin olun.
2. Azure hedef IP 'leri için bağlantı olduğundan emin olun.
Bağlantıyı doğrulamak için yükseltilmiş bir komut isteminden aşağıdaki komutu çalıştırın. 
    - *nslookup download.Microsoft.com* veya
    - *Ping download.microsoft.com*

## <a name="the-script-downloads-successfully-but-fails-to-run"></a>Betik başarıyla indirilir, ancak çalıştırılamadı

### <a name="for-sles-12-sp4-os-linux"></a>SLES 12 SP4 OS (Linux) için

Hata: iscsi_tcp_module bulunamadı

Olası neden: SUSE Linux işletim sistemi sürüm 12sp4 üzerinde öğe düzeyinde kurtarma için Python betiğini çalıştırırken, bir hata vererek başarısız olur ***iscsi_tcp modülü yüklenemiyor** _. ILR modülü, yedekleme hizmetine TCP bağlantısı kurmak için ' iscsi_tcp ' kullanır. bu nedenle, 12 SP4 sürümünün bir parçası olan SUSE, Open-iSCSI paketinden iscsi_tcp kaldırılmıştır ve ıLR işlemi başarısız olur.

Önerilen eylem: dosya kurtarma betiği yürütme, SUSE 12SP4 VM 'lerinde desteklenmez, geri yükleme işlemini SUSE 12SP4 'ün eski bir sürümünde deneyin.

## <a name="the-script-runs-but-connection-to-iscsi-target-failed"></a>Betik çalışıyor ancak Iscsı hedefine bağlantı başarısız oldu

Hata: hedefe bağlanılırken özel durum yakalandı

1. Betiğin çalıştırıldığı makinenin tüm [erişim gereksinimlerini](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-4-access-requirements-to-successfully-run-the-script)karşıladığından emin olun.
2. Azure hedef IP 'leri bağlantısını denetleyin.
Bağlantıyı doğrulamak için yükseltilmiş bir komut isteminden aşağıdaki komutu çalıştırın. 
    - _nslookup download.microsoft.com * veya<br>
    - *Ping download.microsoft.com*
3. Iscsı giden bağlantı noktası 3260 ' e erişim olduğundan emin olun.
4. Azure hedef IP 'Leri veya kurtarma hizmeti URL 'Lerine trafiği engelleyen bir güvenlik duvarı/NSG olmadığından emin olun.
5. Virüsten koruma 'nın betiğin yürütülmesini engelleyip engellemediğini denetleyin.

## <a name="connected-to-recovery-point-but-disks-did-not-get-attached"></a>Kurtarma noktasına bağlanıldı ancak diskler iliştirilmedi

[Betiği çalıştırmak için doğru makineye](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script) sahip olduğunuzdan emin olun

### <a name="on-windows-vm"></a>Windows VM 'de

**VM 'Deki depolama havuzu salt okunurdur modunda bağlı** Windows 2012 R2 ve Windows 2016 (depolama havuzu ile) için, betiği ilk kez çalıştırırken, VM 'ye bağlı depolama havuzu salt okuma durumuna geçebilir.

Bu sorunu çözmek için, depolama havuzuna ilk kez Read-Write erişimi el ile ayarlamanız ve sanal diskleri bağlamanız gerekir, aşağıdaki adımları izleyin:

1. Read-Write erişimi ayarlayın.
Sunucu Yöneticisi > dosya ve depolama hizmetleri > birimler > depolama havuzları ' na gidin.

   ![Windows Storage](./media/backup-azure-restore-files-from-vm/windows-storage-1.png)

2. **Depolama havuzu** penceresinde, kullanılabilir depolama havuzuna sağ tıklayın ve **Read-Write erişim seçeneğini ayarla** ' yı seçin.

   ![Windows Storage okuma yazma](./media/backup-azure-restore-files-from-vm/windows-storage-read-write-2.png)

3. Depolama havuzu okuma/yazma erişimiyle atandıktan sonra, sanal diski iliştirmemiz gerekir.
Sanal diskler bölümünün altındaki **Sunucu Yöneticisi Kullanıcı arabirimine** gidin > **sanal disk Ekle** seçeneğini belirlemek için sağ tıklayın.

   ![Sunucu Yöneticisi sanal diski](./media/backup-azure-restore-files-from-vm/server-manager-virtual-disk-3.png)

### <a name="on-linux-vm"></a>Linux VM 'de

#### <a name="file-recovery-fails-to-auto-mount-because-disk-does-not-contain-volumes"></a>Disk birim içermediğinden dosya kurtarma işlemi otomatik olarak bağlanamıyor

Dosya kurtarma gerçekleştirilirken, yedekleme hizmeti birimleri algılar ve otomatik olarak takar. Ancak, yedeklenen disklerin ham bölümleri varsa, bu diskler otomatik olarak bağlanmaz ve kurtarma için veri diskini göremedik.

Bu sorunu gidermek için, bu [makalede](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#lvmraid-arrays-for-linux-vms)belgelenen adımları izleyin.
 
#### <a name="os-could-not-identify-the-filesystem-causing-linux-file-recovery-to-fail-while-mountings-disks"></a>İşletim sistemi, Linux dosya kurtarmanın, dağlar diskler sırasında başarısız olmasına neden olan FileSystem 'ı belirleyemedi

Dosya kurtarma betiği veri diski çalıştırılırken aşağıdaki hatayla bağlantı kurulamadı: *işletim sistemi dosya sistemini tanımlamadığından, şu bölümler bağlanamadı*

- Bu sorunu çözmek için birimin bir üçüncü taraf uygulamayla şifrelenip şifrelenmediğini denetleyin. Şifrelendiyse, disk veya VM portalda şifreli olarak gösterilmez.
1. Yedeklenen VM 'de oturum açın ve şu komutu çalıştırın: *lsblk-f*<br>

   ![Birim olmadan disk](./media/backup-azure-restore-files-from-vm/disk-without-volume-5.png)

2. FileSystem ve şifrelemeyi doğrulayın.
3. Birim şifrelenirse, dosya kurtarma desteklenmez. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-support-matrix-iaas#support-for-file-level-restore).

## <a name="disks-are-attached-but-unable-to-mount-volumes"></a>Diskler iliştirildi, ancak birimler takılamadı

- [Betiği çalıştırmak için doğru makineye](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script)sahip olduğunuzdan emin olun.

### <a name="on-windows-vm"></a>Windows VM 'de

Windows için dosya kurtarma betiği çalıştırılırken, ***0 birime iliştirilmiş** _, ancak diskler Disk Yönetimi konsolu 'nda keşfedilir. Iscsı aracılığıyla birim eklenirken, algılanan bazı birimler çevrimdışı duruma geçer. Iscsı kanalı VM ile hizmet arasında iletişim kurduğunda, bu birimleri algılar ve bunları çevrimiçi duruma getirir, ancak bağlanmazlar.

   ![Disk iliştirilmemiş](./media/backup-azure-restore-files-from-vm/disk-not-attached-6.png)

Bu sorunu tanımlamak ve çözmek için aşağıdaki adımları gerçekleştirin:

1. Cmd penceresinde **diskmgmt** komutunu çalıştırarak _ *disk yönetimi** bölümüne gidin.
2. Ek disklerin görüntülenip görüntülenmediğini denetleyin. Aşağıdaki örnekte, disk 2 ek bir disktir.

   ![Disk management0](./media/backup-azure-restore-files-from-vm/disk-management-7.png)

3. **Birime** sağ tıklayın ve **sürücü harfini ve yollarını Değiştir**' i seçin.

   ![Disk management1](./media/backup-azure-restore-files-from-vm/disk-management-8.png)

4. **Sürücü harfi veya yolu Ekle** penceresinde **Şu sürücü harfini ata** ' yı seçin ve kullanılabilir bir sürücü atayıp **Tamam**' a tıklayın. 

   ![Disk management2](./media/backup-azure-restore-files-from-vm/disk-management-9.png)

5. Dosya Gezgini 'nden sürücüyü görüntüleyebilirsiniz.

   ![Disk Management3](./media/backup-azure-restore-files-from-vm/disk-management-10.png)

6. Yeni birimler eklenmelidir.  

   ![Disk takılamadı](./media/backup-azure-restore-files-from-vm/disk-not-mounting-11.png)

7. Dosya Gezgini 'nde, komut dosyası yeniden çalıştırıldıktan sonra yeni birimler görünür olur.

   ![Disk mounting1 değil](./media/backup-azure-restore-files-from-vm/disk-not-mounting-12.png)

   ![Disk mounting2 değil](./media/backup-azure-restore-files-from-vm/disk-not-mounting-13.png)

#### <a name="on-linux-vms"></a>Linux VM 'lerde 

- [Betiği çalıştırmak için doğru makineye](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script)sahip olduğunuzdan emin olun.
- Korunan Linux sanal makinesi LVM ve/veya RAID dizileri kullanıyorsa, bu [makalede](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#lvmraid-arrays-for-linux-vms)listelenen adımları izleyin.

## <a name="cannot-copy-the-files-from-mounted-volumes"></a>Dosyalar bağlı birimlerden kopyalanamıyor

Kopya, 0x80070780 hatasıyla başarısız olabilir: dosyaya sistem tarafından erişilemiyor. Bu durumda, kaynak sunucuda disk yinelenenleri kaldırma özelliğinin etkin olup olmadığını denetleyin. Ardından, geri yükleme sunucusunda sürücülerde yinelenenleri kaldırma özelliği de bulunduğundan emin olun. Geri yükleme sunucusunda sürücüleri çoğaltmamanız için Yinelenenleri kaldırma rolünü yapılandırılmamış olarak bırakabilirsiniz.
