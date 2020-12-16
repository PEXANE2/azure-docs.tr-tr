---
title: Azure VM dosya kurtarma sorunlarını giderme
description: Azure VM yedeklemesinden dosya ve klasörleri kurtarırken karşılaşılan sorunları giderin.
ms.topic: troubleshooting
ms.date: 07/12/2020
ms.openlocfilehash: bd369577e320cf6dca510183948f41e6cf91779b
ms.sourcegitcommit: e15c0bc8c63ab3b696e9e32999ef0abc694c7c41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97605301"
---
# <a name="troubleshooting-issues-in-file-recovery-of-azure-vm-backup"></a>Azure VM yedeklemesi 'nin dosya kurtarmasında sorun giderme sorunları

Bu makalede, Azure VM yedeğinden dosya ve klasörleri kurtarırken sorunlarla ilgili Azure Backup hataları çözmenize yardımcı olabilecek sorun giderme adımları sunulmaktadır.

## <a name="common-error-messages"></a>Genel hata iletileri

### <a name="exception-caught-while-connecting-to-target"></a>Hedefe bağlanılırken özel durum yakalandı

**Olası neden**: betik kurtarma noktasına erişemiyor.

**Önerilen eylem**: makinenin tüm [erişim gereksinimlerini](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-4-access-requirements-to-successfully-run-the-script)yerine getirip getirmediğini denetleyin.

### <a name="the-target-has-already-been-logged-in-via-an-iscsi-session"></a>Hedef, bir Iscsı oturumu aracılığıyla zaten oturum açtı

**Olası neden**: komut dosyası aynı makinede zaten yürütüldü ve sürücüler eklenmiş.

**Önerilen eylem**: kurtarma noktası birimleri zaten eklenmiş. Özgün VM 'nin aynı sürücü harflerine bağlı olmayabilir. Dosya Gezgini 'nde kullanılabilir tüm birimlere göz atabilirsiniz.

### <a name="this-script-is-invalid-because-the-disks-have-been-dismounted-via-portalexceeded-the-12-hr-limit-download-a-new-script-from-the-portal"></a>Diskler Portal üzerinden çıkartılırsa/12-hr sınırını aştığından bu betik geçersizdir. Portaldan yeni bir betiği indirme

**Olası neden**: diskler portaldan çıkarıldı veya 12 saatlik sınır aşıldı.

**Önerilen eylem**: betik, indirildiği zamandan itibaren 12 saat sonra geçersizdir ve yürütülemez. Portalı ziyaret edin ve dosya kurtarmaya devam etmek için yeni bir betik indirin.

## <a name="troubleshooting-common-scenarios"></a>Yaygın senaryoların sorunlarını giderme

Bu bölümde, dosya kurtarma için betiği indirme ve yürütme sırasında karşılaşabileceğiniz sorunları gidermeye yönelik adımlar sağlanmaktadır.

### <a name="cant-download-the-script"></a>Betik indirilemiyor

Önerilen eylem:

1. [Betiği indirmek için gereken tüm izinlere](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#select-recovery-point-who-can-generate-script)sahip olduğunuzdan emin olun.
1. Azure hedef IP 'lerine bağlantı olduğundan emin olun

Bağlantıyı doğrulamak için, yükseltilmiş bir komut isteminden aşağıdaki komutlardan birini çalıştırın.

`nslookup download.microsoft.com`

veya

`ping download.microsoft.com`

### <a name="the-script-downloads-successfully-but-fails-to-run"></a>Betik başarıyla indirilir, ancak çalıştırılamadı

#### <a name="for-sles-12-sp4-os-linux"></a>SLES 12 SP4 OS (Linux) için

**Hata**: iscsi_tcp_module bulunamadı

**Olası neden**: SUSE Linux işletim sistemi sürüm 12sp4 üzerinde öğe düzeyinde kurtarma (ILR) için Python betiği çalıştırılırken, bu hata ile başarısız olur **_iscsi_tcp modülü yüklenemiyor_* _.

ILR modülü, yedekleme hizmetine TCP bağlantısı kurmak için _ *iscsi_tcp** kullanır. 12SP4 sürümünün bir parçası olarak, SUSE açık iSCSI paketinden **iscsi_tcp** KALDıRDıKÇA ILR işlemi başarısız olur.

**Önerilen eylem**: SUSE 12Sp4 VM 'lerinde dosya kurtarma betiği yürütme desteklenmez. SUSE 12SP4 'ün eski bir sürümünde geri yükleme işlemini deneyin.

### <a name="the-script-runs-but-connection-to-iscsi-target-failed"></a>Betik çalışıyor ancak Iscsı hedefine bağlantı başarısız oldu

**Hata**: hedefe bağlanılırken özel durum yakalandı

1. Betiğin çalıştırıldığı makinenin tüm [erişim gereksinimlerini](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-4-access-requirements-to-successfully-run-the-script)karşıladığından emin olun.
1. Azure hedef IP 'lerine bağlantı olup olmadığını denetleyin.
Bağlantıyı doğrulamak için, yükseltilmiş bir komut isteminden aşağıdaki komutlardan birini çalıştırın.

   `nslookup download.microsoft.com`

   veya

   `ping download.microsoft.com`
1. Iscsı giden bağlantı noktası 3260 ' e erişim olduğundan emin olun.
1. Azure hedef IP 'Leri veya kurtarma hizmeti URL 'Lerine giden trafiği engelleyen güvenlik duvarı veya NSG olmadığından emin olun.
1. Virüsten koruma 'nın betiğin yürütülmesini engelleyip engellemediğini denetleyin.

### <a name="connected-to-recovery-point-but-disks-didnt-get-attached"></a>Kurtarma noktasına bağlanıldı ancak diskler iliştirilmedi

[Betiği çalıştırmak için doğru makineye](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script) sahip olduğunuzdan emin olun

#### <a name="on-a-windows-vm"></a>Bir Windows sanal makinesinde

**VM 'Deki depolama havuzu salt okunurdur**: Windows 2012 R2 ve Windows 2016 (depolama havuzları ile), betiği ilk kez ÇALıŞTıRıRKEN, VM 'ye bağlı depolama havuzu salt okunurdur.

Bu sorunu çözmek için, depolama havuzuna ilk kez Read-Write erişimi el ile ayarlamanız ve sanal diskleri iliştirmemiz gerekir. Aşağıdaki adımları izleyin:

1. Read-Write erişimi ayarlayın.

   **Sunucu Yöneticisi**  >  **dosya ve depolama hizmetleri**  >  **birimleri**  >  **depolama havuzları**' na gidin.

   ![Windows Storage](./media/backup-azure-restore-files-from-vm/windows-storage-1.png)

1. **Depolama havuzu** penceresinde, kullanılabilir depolama havuzuna sağ tıklayın ve **Read-Write erişimi ayarla** seçeneğini belirleyin.

   ![Windows Storage okuma yazma](./media/backup-azure-restore-files-from-vm/windows-storage-read-write-2.png)

1. Depolama havuzu okuma/yazma erişimiyle atandıktan sonra, sanal diski bağlayın.

   **Sunucu Yöneticisi Kullanıcı arabirimine** gidin. **Sanal diskler** bölümünün altında > **sanal disk Ekle** seçeneğini belirlemek için sağ tıklayın.

   ![Sunucu Yöneticisi sanal diski](./media/backup-azure-restore-files-from-vm/server-manager-virtual-disk-3.png)

#### <a name="on-a-linux-vm"></a>Bir Linux sanal makinesinde

##### <a name="file-recovery-fails-to-auto-mount-because-disk-doesnt-contain-volumes"></a>Disk birim içermediğinden dosya kurtarma otomatik olarak bağlanamıyor

Dosya kurtarma gerçekleştirilirken, yedekleme hizmeti birimleri ve otomatik takar algılar. Ancak, yedeklenen disklerin ham bölümleri varsa, bu diskler yeniden bağlanmaz ve kurtarma için veri diski göremezsiniz.

Bu sorunu gidermek için, bu [makalede](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#lvmraid-arrays-for-linux-vms)belgelenen adımları izleyin.

##### <a name="the-os-couldnt-identify-the-filesystem-causing-linux-file-recovery-to-fail-while-mountings-disks"></a>İşletim sistemi, Linux dosya kurtarmanın, dağlar diskler sırasında başarısız olmasına neden olan FileSystem 'ı tanımlayamadı

Dosya kurtarma betiği çalıştırılırken, veri diski aşağıdaki hatayla iliştirilemedi:

"İşletim sisteminin FileSystem 'ı tanımlamadığından aşağıdaki bölümler bağlanamadı"

Bu sorunu çözmek için birimin bir üçüncü taraf uygulamayla şifrelenip şifrelenmediğini denetleyin. Şifrelendiyse, disk veya VM portalda şifreli olarak gösterilmez.

1. Yedeklenen VM 'de oturum açın ve şu komutu çalıştırın:

   `*lsblk -f*`

   ![Birim olmadan disk](./media/backup-azure-restore-files-from-vm/disk-without-volume-5.png)

2. FileSystem ve şifrelemeyi doğrulayın.
3. Birim şifrelenirse, dosya kurtarma desteklenmez. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-support-matrix-iaas#support-for-file-level-restore).

### <a name="disks-are-attached-but-unable-to-mount-volumes"></a>Diskler iliştirildi, ancak birimler takılamadı

- [Betiği çalıştırmak için doğru makineye](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script)sahip olduğunuzdan emin olun.

#### <a name="on-windows-vms"></a>Windows VM 'lerde

Windows için dosya kurtarma betiği çalıştırılırken, ***0 birim eklendiği** bir ileti vardır. Ancak diskler Disk Yönetimi konsolunda bulunur. Iscsı aracılığıyla birim eklenirken algılanan bazı birimler çevrimdışı duruma geçer. Iscsı kanalı VM ile hizmet arasında iletişim kurduğunda, bu birimleri algılar ve bunları çevrimiçi duruma getirir ancak bağlanmazlar.

   ![Disk iliştirilmemiş](./media/backup-azure-restore-files-from-vm/disk-not-attached-6.png)

Bu sorunu tanımlamak ve çözmek için aşağıdaki adımları gerçekleştirin:

1. Cmd penceresinde **diskmgmt** komutunu çalıştırarak _ *disk yönetimi** bölümüne gidin.
1. Ek disklerin görüntülenip görüntülenmediğini denetleyin. Aşağıdaki örnekte disk 2 ek bir disktir.

   ![Disk management0](./media/backup-azure-restore-files-from-vm/disk-management-7.png)

1. **Yeni birime** sağ tıklayın ve **sürücü harfini ve yollarını Değiştir**' i seçin.

   ![Disk management1](./media/backup-azure-restore-files-from-vm/disk-management-8.png)

1. **Sürücü harfi veya yolu Ekle** penceresinde, **aşağıdaki sürücü harfini ata** ' yı seçin ve kullanılabilir bir sürücü atayın ve **Tamam**' ı seçin.

   ![Disk management2](./media/backup-azure-restore-files-from-vm/disk-management-9.png)

1. Dosya Gezgini 'nden seçtiğiniz sürücü harfini görüntüleyin ve dosyaları araştırın.

#### <a name="on-linux-vms"></a>Linux VM 'lerde

- [Betiği çalıştırmak için doğru makineye](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script)sahip olduğunuzdan emin olun.
- Korunan Linux sanal makinesi LVM veya RAID dizileri kullanıyorsa, bu [makalede](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#lvmraid-arrays-for-linux-vms)listelenen adımları izleyin.

### <a name="cant-copy-the-files-from-mounted-volumes"></a>Dosyalar bağlı birimlerden kopyalanamıyor

Kopya 0x80070780 hatası ile başarısız olabilir **: dosyaya sistem tarafından erişilemiyor.** Bu durumda, kaynak sunucuda disk yinelenenleri kaldırma özelliğinin etkin olup olmadığını denetleyin. Ardından, geri yükleme sunucusunda sürücülerde yinelenenleri kaldırma özelliği de bulunduğundan emin olun. Geri yükleme sunucusunda sürücüleri çoğaltmamanız için Yinelenenleri kaldırma rolünü yapılandırılmamış olarak bırakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure sanal makine yedeğinden dosya ve klasörleri kurtarma](backup-azure-restore-files-from-vm.md)
