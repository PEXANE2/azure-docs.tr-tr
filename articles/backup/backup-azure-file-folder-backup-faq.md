---
title: Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı – SSS
description: Azure Backup ile dosya ve klasörleri yedekleme hakkında sık sorulan sorulara yöneliktir.
ms.topic: conceptual
ms.date: 07/29/2019
ms.openlocfilehash: e3a5b6d07982c3261b457d4999025c44489c0a8d
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89182522"
---
# <a name="frequently-asked-questions---microsoft-azure-recovery-services-mars-agent"></a>Sık sorulan sorular-Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı

Bu makalede, [Azure Backup](backup-overview.md) hizmetinde MICROSOFT Azure kurtarma HIZMETLERI (mars) aracısıyla veri yedekleme hakkında sık sorulan sorular yanıtlanmaktadır.

## <a name="configure-backups"></a>Yedeklemeleri yapılandırma

### <a name="where-can-i-download-the-latest-version-of-the-mars-agent"></a>MARS aracısının en son sürümünü nereden indirebilirim?

Windows Server makineleri, System Center DPM ve Microsoft Azure Backup sunucusu yedeklenirken kullanılan en son MARS Aracısı [indirilebilir](https://aka.ms/azurebackup_agent).

### <a name="where-can-i-download-the-vault-credentials-file"></a>Kasa kimlik bilgileri dosyasını nereden indirebilirim?

Azure portal, kasanızın **Özellikler** ' e gidin. **Yedekleme kimlik bilgileri**altında, **zaten en son kurtarma hizmetleri aracısını kullanan**onay kutusunu seçin. **İndir**'i seçin.

![Kimlik bilgilerini indir](./media/backup-azure-file-folder-backup-faq/download-credentials.png)

### <a name="how-long-are-vault-credentials-valid"></a>Kasa kimlik bilgileri ne kadar süreyle geçerlidir?

Kasa kimlik bilgilerinin 10 gün sonra sona ereceği süre. Kimlik bilgileri dosyasının süresi dolarsa, Azure portal dosyayı yeniden indirin.

### <a name="what-characters-are-allowed-for-the-passphrase"></a>Parola için hangi karakterlere izin veriliyor?

Parola, ASCII karakter kümesindeki karakterleri kullanmalıdır, bu [değer 127 değerinden küçük veya buna eşit](https://docs.microsoft.com/office/vba/language/reference/user-interface-help/character-set-0127)olmalıdır.

### <a name="from-what-drives-can-i-back-up-files-and-folders"></a>Dosyaları ve klasörleri hangi sürücülerden yedekleyebilirim?

Aşağıdaki sürücü ve birim türlerini yedekleyemiyoruz:

* Çıkarılabilir medya: tüm yedekleme öğesi kaynakları sabit olarak raporlanmalıdır.
* Salt okuma birimleri: birim gölge kopyası hizmeti (VSS) işlevinin çalışması için birimin yazılabilir olması gerekir.
* Çevrimdışı birimler: VSS 'nin çalışması için birimin çevrimiçi olması gerekir.
* Ağ paylaşımları: birim, çevrimiçi yedekleme kullanılarak yedeklenecek sunucuda yerel olmalıdır.
* BitLocker korumalı birimler: yedeklemenin gerçekleşebilmesi için birimin kilidinin açılması gerekir.
* Dosya Sistemi Tanımı: NTFS, desteklenen tek dosya sistemidir.

### <a name="what-file-and-folder-types-are-supported"></a>Hangi dosya ve klasör türleri desteklenir?

Yedekleme için desteklenen dosya ve klasörlerin türleri hakkında [daha fazla bilgi edinin](backup-support-matrix-mars-agent.md#supported-file-types-for-backup) .

### <a name="can-i-use-the-mars-agent-to-back-up-files-and-folders-on-an-azure-vm"></a>Azure VM 'de dosya ve klasörleri yedeklemek için MARS aracısını kullanabilir miyim?  

Evet. Azure Backup, Azure VM aracısına yönelik VM uzantısını kullanarak Azure VM 'Leri için VM düzeyinde yedekleme sağlar. VM 'deki Konuk Windows işletim sistemi üzerindeki dosya ve klasörleri yedeklemek istiyorsanız, bunu yapmak için MARS aracısını yükleyebilirsiniz.

### <a name="can-i-use-the-mars-agent-to-back-up-files-and-folders-on-temporary-storage-for-the-azure-vm"></a>Azure VM 'nin geçici depolamadaki dosya ve klasörleri yedeklemek için MARS aracısını kullanabilir miyim?

Evet. MARS aracısını yükleyip Konuk Windows işletim sistemi üzerindeki dosya ve klasörleri geçici depolama alanına yedekleyin.

* Geçici depolama verileri silinmeden yedekleme işleri başarısız olur.
* Geçici depolama verileri silinirse, yalnızca geçici olmayan depolamaya geri yükleyebilirsiniz.

### <a name="how-do-i-register-a-server-to-another-region"></a>Nasıl yaparım? sunucuyu başka bir bölgeye kaydetmek istiyor musunuz?

Yedekleme verileri, sunucunun kayıtlı olduğu kasadaki veri merkezine gönderilir. Veri merkezini değiştirmenin en kolay yolu, aracıyı kaldırmak ve yeniden yüklemek ve ardından makineyi ihtiyacınız olan bölgede yeni bir kasaya kaydetmeniz gerekir.

### <a name="does-the-mars-agent-support-windows-server-2012-deduplication"></a>MARS Aracısı Windows Server 2012 Yinelenenleri kaldırmayı destekliyor mu?

Evet. MARS Aracısı, yinelenenleri kaldırılmış verileri, yedekleme işlemini hazırlarken normal verilere dönüştürür. Ardından verileri yedekleme için iyileştirir, verileri şifreler ve sonra şifreli verileri kasaya gönderir.

### <a name="do-i-need-administrator-permissions-to-install-and-configure-the-mars-agent"></a>MARS aracısını yüklemek ve yapılandırmak için yönetici izinlerine ihtiyacım var mı?

Evet, Mars konsolunu kullanarak MARS aracısının ve yedeklemelerin yapılandırmasının yüklenmesi, kullanıcının korumalı sunucuda yerel yönetici olması gerekir.

## <a name="manage-backups"></a>Yedekleri yönetme

### <a name="what-happens-if-i-rename-a-windows-machine-configured-for-backup"></a>Yedekleme için yapılandırılmış bir Windows makinesini yeniden adlandırdığımda ne olur?

Bir Windows makinesini yeniden adlandırdığınızda, şu anda yapılandırılmış tüm yedeklemeler durdurulur.

* Yeni makine adını yedekleme kasasıyla kaydetmeniz gerekir.
* Yeni adı kasayla kaydettiğinizde, ilk işlem *tam* yedekleme olur.
* Eski sunucu adıyla kasaya yedeklenen verileri kurtarmanız gerekiyorsa, verileri kurtar sihirbazındaki alternatif bir konuma geri yükleme seçeneğini kullanın. [Daha fazla bilgi edinin](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine).

### <a name="what-is-the-maximum-file-path-length-for-backup"></a>Yedekleme için en fazla dosya yolu uzunluğu nedir?

MARS Aracısı NTFS kullanır ve [Windows API 'siyle](/windows/win32/FileIO/naming-a-file#fully-qualified-vs-relative-paths)sınırlı olan FilePath length belirtimini kullanır. Korumak istediğiniz dosyalar izin verilen değerden uzunsa, üst klasörü veya disk sürücüsünü yedekleyin.  

### <a name="what-characters-are-allowed-in-file-paths"></a>Dosya yollarında hangi karakterlere izin veriliyor?

MARS Aracısı NTFS kullanır ve dosya adlarında/yollarında [desteklenen karakterlere](/windows/win32/FileIO/naming-a-file#naming-conventions) izin verir.

### <a name="the-warning-azure-backups-have-not-been-configured-for-this-server-appears"></a>"Azure yedeklemeleri bu sunucu için yapılandırılmamış" uyarısı görünür

Bu uyarı, bir yedekleme ilkesi yapılandırdığınız halde, yerel sunucuda depolanan yedekleme zamanlaması ayarları, yedekleme kasasında depolanan ayarlarla aynı olmadığında bile görünebilir.

* Sunucu veya ayarlar bilinen iyi bir duruma kurtarıldığı zaman, yedekleme zamanlamaları eşitlenmemiş hale gelebilir.
* Bu uyarıyı alırsanız, yedekleme ilkesini yeniden [yapılandırın](backup-azure-manage-windows-server.md) ve ardından yerel sunucuyu Azure ile yeniden eşitlemek için isteğe bağlı bir yedekleme çalıştırın.

## <a name="manage-the-backup-cache-folder"></a>Yedekleme önbelleği klasörünü yönetme

### <a name="whats-the-minimum-size-requirement-for-the-cache-folder"></a>Önbellek klasörü için minimum boyut gereksinimini nedir?

Önbellek klasörünün boyutu, yedeklediğiniz veri miktarını belirler.

* Önbellek klasörü birimlerinde toplam yedekleme verileri boyutunun en az% 5-10 ' ü eşit olan boş alan bulunmalıdır.
* Birimde %5 ' ten az boş alan varsa, birim boyutunu artırın veya [Bu adımları](#how-do-i-change-the-cache-location-for-the-mars-agent)izleyerek önbellek klasörünü yeterli alana sahip bir birime taşıyın.
* Windows sistem durumunu yedeklemeniz durumunda önbellek klasörünü içeren birimde ek 30-35 GB boş alan gerekir.

### <a name="how-to-check-if-scratch-folder-is-valid-and-accessible"></a>Karalama klasörünün geçerli ve erişilebilir olup olmadığını denetleme

1. Varsayılan olarak karalama klasörü şurada bulunur: `\Program Files\Microsoft Azure Recovery Services Agent\Scratch`
2. Karalama klasörü konumunun yolunun aşağıda gösterilen kayıt defteri anahtarı girişlerinin değerleriyle eşleştiğinden emin olun:

    | Kayıt defteri yolu | Kayıt Defteri Anahtarı | Değer |
    | --- | --- | --- |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Yeni önbellek klasörü konumu* |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Yeni önbellek klasörü konumu* |

### <a name="how-do-i-change-the-cache-location-for-the-mars-agent"></a>Nasıl yaparım? MARS aracısının önbellek konumu değiştirilsin mi?

1. Yedekleme altyapısını durdurmak için bu komutu yükseltilmiş bir komut isteminde çalıştırın:

    ```Net stop obengine```
2. Sistem durumu yedeklemesini yapılandırdıysanız, disk yönetimi 'ni açın ve adları biçimindeki disk (ler) i çıkarın `"CBSSBVol_<ID>"` .
3. Varsayılan olarak, karalama klasörü şurada bulunur: `\Program Files\Microsoft Azure Recovery Services Agent\Scratch`
4. Tüm `\Scratch` klasörü yeterli alana sahip farklı bir sürücüye kopyalayın. İçeriğin kopyalanmadığından ve taşınmadığından emin olun.
5. Aşağıdaki kayıt defteri girdilerini yeni taşınan karalama klasörünün yoluyla güncelleştirin.

    | Kayıt defteri yolu | Kayıt Defteri Anahtarı | Değer |
    | --- | --- | --- |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Yeni karalama klasörü konumu* |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Yeni karalama klasörü konumu* |

6. Yedekleme altyapısını yükseltilmiş bir komut isteminde yeniden başlatın:

    ```command
    Net stop obengine

    Net start obengine
    ```

7. İsteğe bağlı yedekleme çalıştırın. Yedekleme işlemi yeni konumu kullanarak başarılı bir şekilde tamamlandıktan sonra özgün önbellek klasörünü kaldırabilirsiniz.

### <a name="where-should-the-cache-folder-be-located"></a>Önbellek klasörü nerede konumlandırılmalıdır?

Önbellek klasörü için aşağıdaki konumlar önerilmez:

* Ağ paylaşma/çıkarılabilir medya: önbellek klasörü, çevrimiçi yedekleme kullanılarak yedeklenmesi gereken sunucu için yerel olmalıdır. Ağ konumları veya USB sürücüleri gibi çıkarılabilir medya desteklenmez.
* Çevrimdışı birimler: önbellek klasörü, Azure Backup Aracısı kullanılarak beklenen yedekleme için çevrimiçi olmalıdır

### <a name="are-there-any-attributes-of-the-cache-folder-that-arent-supported"></a>Önbellek klasörünün desteklenmeyen öznitelikleri var mı?

Önbellek klasörü için aşağıdaki öznitelikler veya birleşimleri desteklenmez:

* Şifreli
* Yinelenenleri kaldırma işlemi uygulanmış
* Sıkıştırılmış
* Seyrek
* Yeniden Ayrıştırma Noktası

Önbellek klasörü ve meta veri VHD 'SI Azure Backup Aracısı için gerekli özniteliklere sahip değildir.

### <a name="is-there-a-way-to-adjust-the-amount-of-bandwidth-used-for-backup"></a>Yedekleme için kullanılan bant genişliği miktarını ayarlamak için bir yol var mı?

Evet, bant genişliğini ve zamanlamayı ayarlamak için MARS aracısındaki **Özellikleri Değiştir** seçeneğini kullanabilirsiniz. [Daha fazla bilgi edinin](backup-windows-with-mars-agent.md#enable-network-throttling).

## <a name="restore"></a>Geri Yükleme

### <a name="manage"></a>Yönetme

#### <a name="can-i-recover-if-i-forgot-my-passphrase"></a>Parolamı unuttum, kurtarabilir miyim?

Azure Backup Aracısı, geri yükleme sırasında yedeklenen verilerin şifresini çözmek için bir parola (kayıt sırasında verdiğiniz) gerektirir. Kayıp bir parolayı işleme seçeneklerinizi anlamak için aşağıdaki senaryoları gözden geçirin:

| Özgün makine <br> *(yedeklemelerin alındığı kaynak makine)* | Deyimi | Kullanılabilir seçenekler |
| --- | --- | --- |
| Kullanılabilir |Mesi |Özgün makineniz (yedeklemelerin alındığı yer) kullanılabilir ve aynı kurtarma hizmetleri kasasıyla kaydedilmişse, bu [adımları](./backup-azure-manage-mars.md#re-generate-passphrase)izleyerek parolayı yeniden oluşturabilirsiniz.  |
| Mesi |Mesi |Verilerin kurtarılması mümkün değil veya veriler kullanılamıyor |

Aşağıdaki koşulları göz önünde bulundurun:

* Aracıyı Kaldırma ve ile aynı orijinal makineye yeniden kaydetme
  * *Aynı parola*, yedeklenen verilerinizi geri yükleyebilirsiniz.
  * *Farklı bir parola*, daha sonra yedeklenen verilerinizi geri alamazsınız.
* Aracıyı, ile *farklı bir makineye* yüklerseniz
  * *Aynı parola* (özgün makinede kullanılır), daha sonra yedeklenen verilerinizi geri yükleyebilirsiniz.
  * *Farklı parola*, yedeklenen verilerinizi geri alamazsınız.
* Özgün makineniz bozuksa (parolayı MARS konsolu aracılığıyla yeniden üretmeyi önler), ancak MARS Aracısı tarafından kullanılan orijinal karalama klasörünü geri yükleyebilir veya erişebilirsiniz, ardından geri yükleme yapabilirsiniz (parolayı unuttuysanız). Daha fazla yardım için müşteri desteğine başvurun.

#### <a name="how-do-i-recover-if-i-lost-my-original-machine-where-backups-were-taken"></a>Özgün makinemi (yedeklemelerin alındığı) kaybetmem durumunda kurtarma Nasıl yaparım??

Özgün makinenin aynı parolaya sahipseniz (kayıt sırasında sağladıysanız), yedeklenen verileri alternatif bir makineye geri yükleyebilirsiniz. Geri yükleme seçeneklerinizi anlamak için aşağıdaki senaryoları gözden geçirin.

| Özgün makine | Deyimi | Kullanılabilir seçenekler |
| --- | --- | --- |
| Mesi |Kullanılabilir |MARS aracısını, özgün makinenin kaydı sırasında sağladınız aynı parola ile başka bir makineye yükleyebilir ve kaydedebilirsiniz. **Recovery Option**  >  Geri yükleme işlemini gerçekleştirmek için kurtarma seçeneğini**başka bir konum** seçin. Daha fazla bilgi için bu [makaleye](./backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine)bakın.
| Mesi |Mesi |Verilerin kurtarılması mümkün değil veya veriler kullanılamıyor |

### <a name="my-backup-jobs-have-been-failing-or-not-running-for-a-long-time-im-past-the-retention-period-can-i-still-restore"></a>Yedekleme işlerim uzun süredir başarısız olmuş veya çalışmıyor. Saklama süresini geçti. Hala geri yükleme yapabilir miyim?

Bir güvenlik önlemi olarak, Azure Backup bekletme süresini geçmiş olsa bile en son kurtarma noktasını korur. Yedeklemeler çalışmaya başladıktan ve yeni kurtarma noktaları kullanılabilir hale geldiğinde, eski kurtarma noktası belirtilen bekletmeye göre kaldırılır.

### <a name="what-happens-if-i-cancel-an-ongoing-restore-job"></a>Devam eden bir geri yükleme işini iptal etdiğimde ne olur?

Devam eden bir geri yükleme işi iptal edilirse, geri yükleme işlemi duraklar. İptal etmeden önce tüm dosyalar, geri alma yapılmadan yapılandırılmış hedefte (orijinal veya alternatif konum) kalır.

### <a name="does-the-mars-agent-back-up-and-restore-acls-set-on-files-folders-and-volumes"></a>MARS Aracısı dosyalarda, klasörlerde ve birimlerde ayarlanan ACL 'Leri yedekler ve geri yükler mi?

* MARS Aracısı, dosyalar, klasörler ve birimler üzerinde ayarlanan ACL 'Leri yedekler
* Birim geri yükleme kurtarma seçeneği için, MARS Aracısı, kurtarılan dosya veya klasöre ACL izinleri geri yüklemeyi atlama seçeneği sunar
* Tek dosya ve klasörler kurtarma seçeneği için, MARS Aracısı ACL izinleriyle geri yüklenecek (ACL geri yüklemeyi atlama seçeneği yoktur).

## <a name="next-steps"></a>Sonraki adımlar

Bir Windows makinesini nasıl [yedekleyeceğinizi öğrenin](tutorial-backup-windows-server-to-azure.md) .
