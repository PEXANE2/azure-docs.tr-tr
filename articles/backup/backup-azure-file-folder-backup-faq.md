---
title: Dosya ve klasörleri yedekleme - sık sorulan sorular
description: Azure Yedekleme ile dosya ve klasörleri yedekleme yle ilgili sık sorulan soruları giderer.
ms.topic: conceptual
ms.date: 07/29/2019
ms.openlocfilehash: 55819ce7ec5196812d935a21c096c132144d78af
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80421317"
---
# <a name="common-questions-about-backing-up-files-and-folders"></a>Dosya ve klasörleri yedekleme hakkında sık sorulan sorular

Bu makalede, [Azure Yedekleme](backup-overview.md) hizmetinde Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı ile dosya ve klasörleri yedekleme yle ilgili sık sorulan soruları yanıtlar.

## <a name="configure-backups"></a>Yedeklemeleri yapılandırma

### <a name="where-can-i-download-the-latest-version-of-the-mars-agent"></a>MARS aracısının en son sürümünü nereden indirebilirim?

Windows Server makinelerini, System Center DPM'i ve Microsoft Azure Yedekleme sunucularını yedeklerken kullanılan en son MARS [aracısını indirebilirsiniz.](https://aka.ms/azurebackup_agent)

### <a name="how-long-are-vault-credentials-valid"></a>Kasa kimlik bilgileri ne kadar süre geçerlidir?

Kasa kimlik bilgilerinin süresi 48 sonra dolar. Kimlik bilgileri dosyasının süresi doluyorsa, dosyayı Azure portalından yeniden indirin.

### <a name="from-what-drives-can-i-back-up-files-and-folders"></a>Hangi sürücülerden dosya ve klasörleri yedekleyebilirim?

Aşağıdaki sürücü ve birim türlerini yedekleyemezsiniz:

* Çıkarılabilir ortam: Tüm yedek madde kaynakları sabit olarak rapor lanmalıdır.
* Salt okunur birimler: Birim gölge kopyalama hizmetinin (VSS) çalışması için birim yazılabilir olmalıdır.
* Çevrimdışı birimler: VSS'nin çalışması için birim çevrimiçi olmalıdır.
* Ağ paylaşımları: Çevrimiçi yedekleme kullanılarak yedeklenecek ses düzeyi sunucunun yerel olması gerekir.
* BitLocker korumalı birimler: Yedeklemenin oluşmadan önce ses düzeyinin kilidi açılmalıdır.
* Dosya Sistemi Tanımı: NTFS, desteklenen tek dosya sistemidir.

### <a name="what-file-and-folder-types-are-supported"></a>Hangi dosya ve klasör türleri desteklenir?

Yedekleme için desteklenen dosya ve klasör türleri hakkında [daha fazla bilgi edinin.](backup-support-matrix-mars-agent.md#supported-file-types-for-backup)

### <a name="can-i-use-the-mars-agent-to-back-up-files-and-folders-on-an-azure-vm"></a>Azure VM'deki dosya ve klasörleri yedeklemek için MARS aracısını kullanabilir miyim?  

Evet. Azure Yedekleme, Azure VM aracısının VM uzantısını kullanarak Azure VM'ler için VM düzeyinde yedekleme sağlar. VM'deki konuk Windows işletim sistemindeki dosya ve klasörleri yedeklemek istiyorsanız, bunu yapmak için MARS aracısını yükleyebilirsiniz.

### <a name="can-i-use-the-mars-agent-to-back-up-files-and-folders-on-temporary-storage-for-the-azure-vm"></a>Azure VM için geçici depolama alanı üzerindeki dosya ve klasörleri yedeklemek için MARS aracısını kullanabilir miyim?

Evet. MARS aracısını yükleyin ve konuk Windows işletim sistemindeki dosya ve klasörleri geçici depolama ya da depolamaya yedekleyin.

* Geçici depolama verileri silindiğinde yedekleme işleri başarısız olur.
* Geçici depolama verileri silinirse, yalnızca geçici olmayan depolama alanına geri yükleyebilirsiniz.

### <a name="how-do-i-register-a-server-to-another-region"></a>Bir sunucuyı başbir bölgeye nasıl kaydedebilirim?

Yedekleme verileri, sunucunun kayıtlı olduğu kasanın veri merkezine gönderilir. Veri merkezini değiştirmenin en kolay yolu aracıyı kaldırmak ve yeniden yüklemek ve ardından makineyi ihtiyacınız olan bölgede yeni bir kasaya kaydetmektir.

### <a name="does-the-mars-agent-support-windows-server-2012-deduplication"></a>MARS aracısı Windows Server 2012 çoğaltmayı destekliyor mu?

Evet. MARS aracısı, yinelenen verileri yedekleme işlemini hazırlarken normal verilere dönüştürür. Daha sonra yedekleme için verileri optimize eder, verileri şifreler ve şifrelenmiş verileri kasaya gönderir.

### <a name="do-i-need-administrator-permissions-to-install-and-configure-the-mars-agent"></a>MARS aracısını yüklemek ve yapılandırmak için yönetici izinlerine ihtiyacım var mı?

Evet, MARS Aracısı'nın yüklenmesi ve MARS konsolunu kullanan yedeklemelerin yapılandırması, kullanıcının korumalı sunucuda yerel bir yönetici olması gerekir.

## <a name="manage-backups"></a>Yedekleri yönetme

### <a name="what-happens-if-i-rename-a-windows-machine-configured-for-backup"></a>Yedekleme için yapılandırılan bir Windows makinesini yeniden adlandırırsam ne olur?

Bir Windows makinesini yeniden adlandırdığınızda, şu anda yapılandırılan tüm yedeklemeler durdurulur.

* Yeni makine adını Yedekleme kasasına kaydettirmeniz gerekir.
* Yeni adı kasaya kaydettiğinizde, ilk işlem *tam* yedekleme olur.
* Eski sunucu adı ile kasaya yedeklenmiş verileri kurtarmanız gerekiyorsa, Verileri Kurtarma Sihirbazı'nda alternatif bir konuma geri yükleme seçeneğini kullanın. [Daha fazla bilgi edinin](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine).

### <a name="what-is-the-maximum-file-path-length-for-backup"></a>Yedekleme için maksimum dosya yolu uzunluğu nedir?

MARS aracısı NTFS'ye dayanır ve [Windows API](/windows/desktop/FileIO/naming-a-file#fully-qualified-vs-relative-paths)ile sınırlı dosya yolu uzunluğu belirtimini kullanır. Korumak istediğiniz dosyalar izin verilen değerden daha uzunsa, üst klasörü veya disk sürücüsünün yedeklemesine neden olur.  

### <a name="what-characters-are-allowed-in-file-paths"></a>Dosya yollarında hangi karakterlere izin verilir?

MARS aracısı NTFS'e güvenir ve dosya adlarında/yollarında [desteklenen karakterlere](/windows/desktop/FileIO/naming-a-file#naming-conventions) izin verir.

### <a name="the-warning-azure-backups-have-not-been-configured-for-this-server-appears"></a>"Azure Yedeklemeleri bu sunucu için yapılandırılmamıştır" uyarısı görüntülenir

Yerel sunucuda depolanan yedekleme zamanlama ayarları yedekleme kasasında depolanan ayarlarla aynı olmadığında, bir yedekleme ilkesi ni yapılandırmış olsanız bile bu uyarı görünebilir.

* Sunucu veya ayarlar bilinen iyi bir duruma kurtarıldığında, yedekleme zamanlamaları eşitlenmemiş olabilir.
* Bu uyarıyı alırsanız, yedekleme ilkesini yeniden [yapılandırın](backup-azure-manage-windows-server.md) ve yerel sunucuyu Azure ile yeniden eşitlemek için isteğe bağlı yedekleme çalıştırın.

## <a name="manage-the-backup-cache-folder"></a>Yedekleme önbellek klasörünü yönetme

### <a name="whats-the-minimum-size-requirement-for-the-cache-folder"></a>Önbellek klasörü için minimum boyut gereksinimini nedir?

Önbellek klasörünün boyutu, yedeklediğiniz veri miktarını belirler.

* Önbellek klasörü birimleri, yedekleme verilerinin toplam boyutunun en az %5-10'una eşit boş alana sahip olmalıdır.
* Birim %5'ten az boş alana sahipse, birim boyutunu artırın veya [önbellek](#how-do-i-change-the-cache-location-for-the-mars-agent)klasörünü aşağıdaki adımları izleyerek yeterli alana sahip bir büyüklüğe taşıyın.
* Windows System State'i yedeklerseniz, önbellek klasörünü içeren birimde 30-35 GB boş alana ihtiyacınız olur.

### <a name="how-to-check-if-scratch-folder-is-valid-and-accessible"></a>Scratch klasörü geçerli ve erişilebilir olup olmadığını nasıl kontrol etmek için?

1. Varsayılan olarak scratch klasörü`\Program Files\Microsoft Azure Recovery Services Agent\Scratch`
2. Scratch klasörünüzün konumunun yolunun, aşağıda gösterilen kayıt defteri anahtar girişlerinin değerleriyle eşleştiğinden emin olun:

    | Kayıt defteri yolu | Kayıt Defteri Anahtarı | Değer |
    | --- | --- | --- |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Yeni önbellek klasörü konumu* |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Yeni önbellek klasörü konumu* |

### <a name="how-do-i-change-the-cache-location-for-the-mars-agent"></a>MARS aracısının önbellek konumunu nasıl değiştirebilirim?

1. Yedekleme motorunu durdurmak için bu komutu yükseltilmiş bir komut isteminde çalıştırın:

    ```Net stop obengine```
2. System State yedeklemesini yapılandırmışsanız, Disk Yönetimi'ni açın ve disk(ler)'i biçimdeki `"CBSSBVol_<ID>"`adlarla açın.
3. Varsayılan olarak, çizik klasörü`\Program Files\Microsoft Azure Recovery Services Agent\Scratch`
4. Tüm `\Scratch` klasörü yeterli alana sahip farklı bir sürücüye kopyalayın. İçeriğin kopyalandığından, taşınmadığından emin olun.
5. Aşağıdaki kayıt defteri girişlerini yeni taşınan sıfırdan çizilen klasörün yolu ile güncelleştirin.

    | Kayıt defteri yolu | Kayıt Defteri Anahtarı | Değer |
    | --- | --- | --- |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Yeni scratch klasörü konumu* |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Yeni scratch klasörü konumu* |

6. Yedekleme motorunu yükseltilmiş bir komut isteminde yeniden başlatın:

    ```command
    Net stop obengine

    Net start obengine
    ```

7. İsteğe bağlı yedekleme çalıştırın. Yedekleme yeni konumu başarıyla tamamladıktan sonra, özgün önbellek klasörünü kaldırabilirsiniz.

### <a name="where-should-the-cache-folder-be-located"></a>Önbellek klasörü nerede bulunmalıdır?

Önbellek klasörü için aşağıdaki konumlar önerilmez:

* Ağ paylaşımı/çıkarılabilir ortam: Önbellek klasörü, çevrimiçi yedeklemeyi kullanarak yedeklemesi gereken sunucuiçin yerel olmalıdır. Ağ konumları veya USB sürücüler gibi çıkarılabilir ortamlar desteklenmez.
* Çevrimdışı birimler: Önbellek klasörü Azure Yedekleme Aracısı kullanarak beklenen yedekleme için çevrimiçi olmalıdır

### <a name="are-there-any-attributes-of-the-cache-folder-that-arent-supported"></a>Önbellek klasörünün desteklenmeyen öznitelikleri var mı?

Aşağıdaki öznitelikler veya bunların bileşimleri, önbellek klasörü için desteklenmez:

* Şifreli
* Yinelenenleri kaldırma işlemi uygulanmış
* Sıkıştırılmış
* Seyrek
* Yeniden Ayrıştırma Noktası

Önbellek klasörü ve meta veri VHD, Azure Yedekleme aracısı için gerekli özelliklere sahip değildir.

### <a name="is-there-a-way-to-adjust-the-amount-of-bandwidth-used-for-backup"></a>Yedekleme için kullanılan bant genişliği miktarını ayarlamanın bir yolu var mı?

Evet, bant genişliğini ve zamanlamayı ayarlamak için MARS aracısındaki **Özellikleri Değiştir** seçeneğini kullanabilirsiniz. [Daha fazla bilgi edinin](backup-windows-with-mars-agent.md#enable-network-throttling).

## <a name="restore"></a>Geri Yükleme

### <a name="manage"></a>Yönetme

**Şifremi unutursam iyileşebilir miyim?**
Azure Yedekleme aracısı, geri yükleme sırasında yedeklenen verilerin şifresini çözmek için bir parola (kayıt sırasında sağladığınız) gerektirir. Kayıp bir parolayı işleme seçeneklerinizi anlamak için aşağıdaki senaryoları gözden geçirin:

| Orijinal Makine <br> *(yedeklemelerin alındığı kaynak makinesi)* | Parola | Kullanılabilir Seçenekler |
| --- | --- | --- |
| Kullanılabilir |Kaybetti |Orijinal makineniz (yedeklerin alındığı yer) kullanılabilir ve hala aynı Kurtarma Hizmetleri kasasına kaydolmuşsa, aşağıdaki [adımları](https://docs.microsoft.com/azure/backup/backup-azure-manage-mars#re-generate-passphrase)izleyerek parolayı yeniden oluşturabilirsiniz.  |
| Kaybetti |Kaybetti |Verileri kurtarmak mümkün değil veya veri kullanılamıyor |

Aşağıdaki koşulları göz önünde bulundurun:

* Aracıyı aynı orijinal makineye yükler ve yeniden kaydederseniz,
  * *Aynı parola,* sonra yedeklenmiş verilerinizi geri yüklemek mümkün olacak.
  * *Farklı parola,* o zaman yedeklenmiş verilerinizi geri yükleyemezsiniz.
* Aracıyı farklı bir *makineye* yüklerseniz,
  * *Aynı parola* (orijinal makinede kullanılır), sonra yedeklenmiş verilerinizi geri yüklemek mümkün olacak.
  * *Farklı parola,* yedeklenmiş verilerinizi geri yükleyemeseniz.
* Orijinal makineniz bozuksa (mars konsolu üzerinden parolayı yeniden oluşturmanızı engelliyorsa), ancak MARS aracısı tarafından kullanılan orijinal karalama klasörünü geri yükleyebilir veya erişebilirsiniz, sonra geri yükleyebilirsiniz (parolayı unuttuysanız). Daha fazla yardım için Müşteri Desteği'ne başvurun.

**Orijinal makinemi (yedeklerin alındığı) kaybettiğimde nasıl kurtarabilirim?**

Orijinal makinenin aynı parolasına (kayıt sırasında sağladığınız) sahipseniz, yedeklenmiş verileri alternatif bir makineye geri yükleyebilirsiniz. Geri yükleme seçeneklerinizi anlamak için aşağıdaki senaryoları gözden geçirin.

| Orijinal Makine | Parola | Kullanılabilir Seçenekler |
| --- | --- | --- |
| Kaybetti |Kullanılabilir |Mars aracısını, orijinal makinenin kaydı sırasında sağladığınız parolayla başka bir makineye yükleyebilir ve kaydedebilirsiniz. **Kurtarma Seçeneği'ni** > geri yüklemenizi gerçekleştirmek için**başka bir konum** seçin. Daha fazla bilgi için bu [makaleye](https://docs.microsoft.com/azure/backup/backup-azure-restore-windows-server#use-instant-restore-to-restore-data-to-an-alternate-machine)bakın.
| Kaybetti |Kaybetti |Verileri kurtarmak mümkün değil veya veri kullanılamıyor |


### <a name="what-happens-if-i-cancel-an-ongoing-restore-job"></a>Devam eden bir geri yükleme işini iptal edersem ne olur?

Devam eden bir geri yükleme işi iptal edilirse, geri yükleme işlemi durur. İptal işleminden önce geri yüklenen tüm dosyalar, herhangi bir geri alma işlemi olmaksızın yapılandırılmış hedefte (orijinal veya alternatif konum) kalır.

### <a name="does-the-mars-agent-back-up-and-restore-acls-set-on-files-folders-and-volumes"></a>MARS aracısı, dosyalarda, klasörlerde ve birimlerde ayarlanan ALA'ları yedekleyip geri yükler mi?

* MARS aracısı, dosyalarda, klasörlerde ve birimlerde ayarlanan ALA'ları yedekler
* Toplu Geri Yükleme kurtarma seçeneği için, MARS aracısı kurtarılan dosya veya klasöre ACL izinlerini geri atama seçeneğini sunar
* Tek tek dosya ve klasörkurtarma seçeneği için, MARS aracısı ACL izinleri ile geri yüklenir (ACL geri yükleme atlamak için bir seçenek yoktur).

## <a name="next-steps"></a>Sonraki adımlar

Windows makinesini [nasıl](tutorial-backup-windows-server-to-azure.md) yedekleyin öğrenin.
