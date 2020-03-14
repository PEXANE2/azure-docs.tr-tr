---
title: Dosya ve klasörleri yedekleme-sık sorulan sorular
description: Azure Backup ile dosya ve klasörleri yedekleme hakkında sık sorulan sorulara yöneliktir.
ms.topic: conceptual
ms.date: 07/29/2019
ms.openlocfilehash: adcbf5c3b404de46634423f8f59c4798d44bebe0
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79273429"
---
# <a name="common-questions-about-backing-up-files-and-folders"></a>Dosya ve klasörleri yedekleme hakkında sık sorulan sorular

Bu makalede, [Azure Backup](backup-overview.md) hizmetindeki MICROSOFT Azure kurtarma HIZMETLERI (mars) aracısıyla dosya ve klasörlerin yedeklenmesiyle ilgili sık karşılaşılan sorular yanıtlanmaktadır.

## <a name="configure-backups"></a>Yedekleri yapılandırma

### <a name="where-can-i-download-the-latest-version-of-the-mars-agent"></a>MARS aracısının en son sürümünü nereden indirebilirim?

Windows Server makineleri, System Center DPM ve Microsoft Azure Backup sunucusu yedeklenirken kullanılan en son MARS Aracısı [indirilebilir](https://aka.ms/azurebackup_agent).

### <a name="how-long-are-vault-credentials-valid"></a>Kasa kimlik bilgileri ne kadar süreyle geçerlidir?

Kasa kimlik bilgilerinin süresi 48 sonra dolar. Kimlik bilgileri dosyasının süresi dolarsa, Azure portal dosyayı yeniden indirin.

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

## <a name="manage-backups"></a>Yedekleri yönetme

### <a name="what-happens-if-i-rename-a-windows-machine-configured-for-backup"></a>Yedekleme için yapılandırılmış bir Windows makinesini yeniden adlandırdığımda ne olur?

Bir Windows makinesini yeniden adlandırdığınızda, şu anda yapılandırılmış tüm yedeklemeler durdurulur.

* Yeni makine adını yedekleme kasasıyla kaydetmeniz gerekir.
* Yeni adı kasayla kaydettiğinizde, ilk işlem *tam* yedekleme olur.
* Eski sunucu adıyla kasaya yedeklenen verileri kurtarmanız gerekiyorsa, verileri kurtar sihirbazındaki alternatif bir konuma geri yükleme seçeneğini kullanın. [Daha fazla bilgi edinin](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine).

### <a name="what-is-the-maximum-file-path-length-for-backup"></a>Yedekleme için en fazla dosya yolu uzunluğu nedir?

MARS Aracısı NTFS kullanır ve [Windows API 'siyle](/windows/desktop/FileIO/naming-a-file#fully-qualified-vs-relative-paths)sınırlı olan FilePath length belirtimini kullanır. Korumak istediğiniz dosyalar izin verilen değerden uzunsa, üst klasörü veya disk sürücüsünü yedekleyin.  

### <a name="what-characters-are-allowed-in-file-paths"></a>Dosya yollarında hangi karakterlere izin veriliyor?

MARS Aracısı NTFS kullanır ve dosya adlarında/yollarında [desteklenen karakterlere](/windows/desktop/FileIO/naming-a-file#naming-conventions) izin verir.

### <a name="the-warning-azure-backups-have-not-been-configured-for-this-server-appears"></a>"Azure yedeklemeleri bu sunucu için yapılandırılmamış" uyarısı görünür

Bu uyarı, bir yedekleme ilkesi yapılandırdığınız halde, yerel sunucuda depolanan yedekleme zamanlaması ayarları, yedekleme kasasında depolanan ayarlarla aynı olmadığında bile görünebilir.

* Sunucu veya ayarlar bilinen iyi bir duruma kurtarıldığı zaman, yedekleme zamanlamaları eşitlenmemiş hale gelebilir.
* Bu uyarıyı alırsanız, yedekleme ilkesini yeniden [yapılandırın](backup-azure-manage-windows-server.md) ve ardından yerel sunucuyu Azure ile yeniden eşitlemek için isteğe bağlı bir yedekleme çalıştırın.

## <a name="manage-the-backup-cache-folder"></a>Yedekleme önbelleği klasörünü yönetme

### <a name="whats-the-minimum-size-requirement-for-the-cache-folder"></a>Önbellek klasörü için minimum boyut gereksinimini nedir?

Önbellek klasörünün boyutu, yedeklediğiniz veri miktarını belirler.

* Önbellek klasörü birimlerinde toplam yedekleme verileri boyutunun en az% 5-10 ' ü eşit olan boş alan bulunmalıdır.
* Birimde %5 ' ten az boş alan varsa, birim boyutunu artırın veya [Bu adımları](#how-do-i-change-the-cache-location-for-the-mars-agent)izleyerek önbellek klasörünü yeterli alana sahip bir birime taşıyın.
* Windows sistem durumunu yedeklemenizin, önbellek klasörünü içeren birimde ek 30-35 GB boş alan olması gerekir.

### <a name="how-to-check-if-scratch-folder-is-valid-and-accessible"></a>Karalama klasörünün geçerli ve erişilebilir olup olmadığını denetleme

1. Varsayılan olarak karalama klasörü `\Program Files\Microsoft Azure Recovery Services Agent\Scratch` konumunda bulunur
2. Karalama klasörü konumunun yolunun aşağıda gösterilen kayıt defteri anahtarı girişlerinin değerleriyle eşleştiğinden emin olun:

    | Kayıt defteri yolu | Kayıt Defteri Anahtarı | Değer |
    | --- | --- | --- |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Yeni önbellek klasörü konumu* |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Yeni önbellek klasörü konumu* |

### <a name="how-do-i-change-the-cache-location-for-the-mars-agent"></a>Nasıl yaparım? MARS aracısının önbellek konumu değiştirilsin mi?

1. Yedekleme altyapısını durdurmak için bu komutu yükseltilmiş bir komut isteminde çalıştırın:

    ```Net stop obengine```
2. Sistem durumu yedeklemesini yapılandırdıysanız, disk yönetimi 'ni açın ve `"CBSSBVol_<ID>"`biçimdeki disk (ler) i çıkarın.
3. Varsayılan olarak, karalama klasörü `\Program Files\Microsoft Azure Recovery Services Agent\Scratch` konumunda bulunur
4. Tüm `\Scratch` klasörünü yeterli alana sahip farklı bir sürücüye kopyalayın. İçeriğin kopyalanmadığından ve taşınmadığından emin olun.
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

Aşağıdaki öznitelikler veya bunların bileşimleri, önbellek klasörü için desteklenmez:

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

**Parolamı unuttum, kurtarabilir miyim?**
Azure Backup Aracısı, geri yükleme sırasında yedeklenen verilerin şifresini çözmek için bir parola (kayıt sırasında verdiğiniz) gerektirir. Kayıp bir parolayı işleme seçeneklerinizi anlamak için aşağıdaki senaryoları gözden geçirin:

| Özgün makine <br> *(yedeklemelerin alındığı kaynak makine)* | Parola | Kullanılabilir seçenekler |
| --- | --- | --- |
| Kullanılabilir |Mesi |Özgün makineniz (yedeklemelerin alındığı yer) kullanılabilir ve aynı kurtarma hizmetleri kasasıyla kaydedilmişse, bu [adımları](https://docs.microsoft.com/azure/backup/backup-azure-manage-mars#re-generate-passphrase)izleyerek parolayı yeniden oluşturabilirsiniz.  |
| Mesi |Mesi |Verilerin kurtarılması mümkün değil veya veriler kullanılamıyor |

Aşağıdaki koşulları göz önünde bulundurun:

* Aracıyı, ile aynı orijinal makineye kaldırır ve yeniden kaydedersiniz
  * *Aynı parola*, yedeklenen verilerinizi geri yükleyebileceksiniz.
  * *Farklı bir parola*, yedeklenen verilerinizi geri yükleyemezsiniz.
* Aracıyı ile *farklı bir makineye* yüklerseniz
  * *Aynı parola* (özgün makinede kullanılır), daha sonra yedeklenen verilerinizi geri yükleyebileceksiniz.
  * *Farklı parola*, yedeklenen verilerinizi geri yükleyemezsiniz.
* Özgün makineniz bozuksa (parolayı MARS konsolu aracılığıyla yeniden üretmeyi önler), ancak MARS Aracısı tarafından kullanılan orijinal karalama klasörünü geri yükleyebilir veya erişebilirsiniz, ardından geri yükleme yapabilirsiniz (parolayı unuttuysanız). Daha fazla yardım için müşteri desteğine başvurun.

**Özgün makinemi (yedeklemelerin alındığı) kaybetmem durumunda kurtarma Nasıl yaparım??**

Özgün makinenin aynı parolaya sahipseniz (kayıt sırasında sağladıysanız), yedeklenen verileri alternatif bir makineye geri yükleyebilirsiniz. Geri yükleme seçeneklerinizi anlamak için aşağıdaki senaryoları gözden geçirin.

| Özgün makine | Parola | Kullanılabilir seçenekler |
| --- | --- | --- |
| Mesi |Kullanılabilir |MARS aracısını, özgün makinenin kaydı sırasında sağladınız aynı parola ile başka bir makineye yükleyebilir ve kaydedebilirsiniz. Geri yükleme işlemini gerçekleştirmek için **başka bir konum** > **Kurtarma seçeneğini** belirleyin. Daha fazla bilgi için [bu makaleye](https://docs.microsoft.com/azure/backup/backup-azure-restore-windows-server#use-instant-restore-to-restore-data-to-an-alternate-machine) bakın.
| Mesi |Mesi |Verilerin kurtarılması mümkün değil veya veriler kullanılamıyor |


### <a name="what-happens-if-i-cancel-an-ongoing-restore-job"></a>Devam eden bir geri yükleme işini iptal etdiğimde ne olur?

Devam eden bir geri yükleme işi iptal edilirse, geri yükleme işlemi duraklar. İptal etmeden önce tüm dosyalar, geri alma yapılmadan yapılandırılmış hedefte (orijinal veya alternatif konum) kalır.

### <a name="does-the-mars-agent-back-up-and-restore-acls-set-on-files-folders-and-volumes"></a>MARS Aracısı dosyalarda, klasörlerde ve birimlerde ayarlanan ACL 'Leri yedekler ve geri yükler mi?

* MARS Aracısı, dosyalar, klasörler ve birimler üzerinde ayarlanan ACL 'Leri yedekler
* Birim geri yükleme kurtarma seçeneği için, MARS Aracısı, kurtarılan dosya veya klasöre ACL izinleri geri yüklemeyi atlama seçeneği sunar
* Tek dosya ve klasörler kurtarma seçeneği için, MARS Aracısı ACL izinleriyle geri yüklenir (ACL geri yüklemeyi atlama seçeneği yoktur).

## <a name="next-steps"></a>Sonraki adımlar

Bir Windows makinesini nasıl [yedekleyeceğinizi öğrenin](tutorial-backup-windows-server-to-azure.md) .
