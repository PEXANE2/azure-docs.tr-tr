---
title: Azure Veri Kutusu Disk sorun giderme veri kopyalama sorunları| Microsoft Dokümanlar
description: Günlükleri kullanarak Azure Veri Kutusu Diski'nde veri kopyalama sırasında görülen sorunları nasıl gidereceğinizi açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/13/2019
ms.author: alkohli
ms.openlocfilehash: 760f5c6c929aa082993683d7a466a71c6484289a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67148354"
---
# <a name="troubleshoot-data-copy-issues-in-azure-data-box-disk"></a>Azure Veri Kutusu Diski'nde veri kopyalama sorunlarını giderme

Bu makale, Microsoft Azure Veri Kutusu Diski için geçerlidir ve verileri disklere kopyalarken gördüğünüz sorunları nasıl giderilen giderilemeyi açıklar. Makale, bölme kopyalama aracını kullanırken sorunları da kapsar.


## <a name="data-copy-issues-when-using-a-linux-system"></a>Linux sistemi kullanırken veri kopyalama sorunları

Bu bölümde, verileri disklere kopyalamak için bir Linux istemcisi kullanılırken karşılaşılan en önemli sorunlardan bazıları ayrıntılı olarak açıkolun.

### <a name="issue-drive-getting-mounted-as-read-only"></a>Sorun: Sürücü salt okunur olarak monte edilmiştir
 
**Neden** 

Bunun nedeni kirli bir dosya sistemi olabilir.

Bir sürücüyü okuma yazma olarak yeniden takmak Veri Kutusu Diskleri ile çalışmaz. Bu senaryo, dislocker tarafından şifresi çözülmüş sürücülerle desteklenmez. Aşağıdaki komutu kullanarak aygıtı başarıyla yeniden monte etmiş olabilirsiniz:

    `# mount -o remount, rw /mnt/DataBoxDisk/mountVol1`

Yeniden montaj başarılı olsa da, veriler devam etmez.

**Çözünürlük**

Linux sisteminizde aşağıdaki adımları atın:

1. ntfsfix yardımcı programı için paketi yükleyin. `ntfsprogs`
2. Kilit açma aracıyla sürücü için sağlanan montaj noktalarını açın. Montaj noktalarının sayısı sürücüler için değişir.

    ```
    unmount /mnt/DataBoxDisk/mountVol1
    ```

3. İlgili `ntfsfix` yolda çalıştırın. Vurgulanan sayı Adım 2 ile aynı olmalıdır.

    ```
    ntfsfix /mnt/DataBoxDisk/bitlockerVol1/dislocker-file
    ```

4. Montaj sorununa neden olabilecek hazırda bekleme meta verilerini kaldırmak için aşağıdaki komutu çalıştırın.

    ```
    ntfs-3g -o remove_hiberfile /mnt/DataBoxDisk/bitlockerVol1/dislocker-file /mnt/DataBoxDisk/mountVol1
    ```

5. Temiz bir unmount yapın.

    ```
    ./DataBoxDiskUnlock_x86_64 /unmount
    ```

6. Temiz bir kilidini açın ve monte edin.
7. Bir dosya yazarak montaj noktasını test edin.
8. Dosya kalıcılığını doğrulamak için sökme ve yeniden monte etme.
9. Veri kopyasıyla devam edin.
 
### <a name="issue-error-with-data-not-persisting-after-copy"></a>Sorun: Kopyadan sonra kalıcı olmayan verilerle ilgili hata
 
**Neden** 

Sürücünüzün monte edildikten sonra verisi olmadığını görürseniz (veriler kopyalanmış olsa da), sürücü yalnızca okuma olarak monte edildikten sonra sürücüyü okuma-yazma olarak yeniden monte etmiş olabilirsiniz.

**Çözünürlük**
 
Bu durumda, [yalnızca okunur olarak monte edilen sürücüler](#issue-drive-getting-mounted-as-read-only)için çözünürlüğe bakın.

Bu durumda, Veri Kutusu Disk Kilidini aracı olan klasörden günlükleri kopyalayın ve [Microsoft Destek'e başvurun.](data-box-disk-contact-microsoft-support.md)


## <a name="data-box-disk-split-copy-tool-errors"></a>Data Box Disk Split Copy aracı hataları

Verileri birden çok diske bölmek için Bölünmüş Kopya aracı kullanılırken görülen sorunlar aşağıdaki tabloda özetlenmiştir.

|Hata iletisi/Uyarılar |Öneriler |
|---------|---------|
|[Bilgi] Birim için BitLocker parolası alma: m <br>[Hata] Birim m için BitLocker tuşunu alırken yakalanan özel durum:<br> Sıra hiçbir öğe içermiyor.|Bu hata hedef Data Box Disk çevrimdışı olduğunda gösterilir. <br> Diskleri çevrimiçi duruma getirmek için `diskmgmt.msc` aracını kullanın.|
|[Hata] Özel durum oluştu: WMI işlemi başarısız oldu:<br> Method=UnlockWithNumericalPassword, ReturnValue=2150694965, <br>Win32Message=Girilen kurtarma parolası biçimi geçersiz. <br>BitLocker kurtarma parolaları 48 hanelidir. <br>Kurtarma parolasının doğru biçimde olduğunu doğrulayıp yeniden deneyin.|Data Box Disk kilit açma aracını kullanarak disklerin kilidini açın ve komutu yeniden deneyin. Daha fazla bilgi için bkz. <li> [Windows istemcileri için Data Box Disk kilidini açma](data-box-disk-deploy-set-up.md#unlock-disks-on-windows-client). </li><li> [Linux istemcileri için Data Box Disk kilidini açma](data-box-disk-deploy-set-up.md#unlock-disks-on-linux-client). </li>|
|[Hata] Özel durum oluştu: Hedef sürücüde DriveManifest.xml dosyası var. <br> Bu durum hedef sürücünün farklı bir günlük dosyasıyla hazırlanmış olabileceğini gösterir. <br>Aynı sürücüye daha fazla veri eklemek için önceki günlük dosyasını kullanın. Varolan verileri silmek ve yeni bir alma işi için hedef sürücüyü yeniden kullanmak için sürücüdeki *DriveManifest.xml'i* silin. Bu komutu yeni bir günlük dosyasıyla yeniden çalıştırın.| Bu hata aynı sürücü kümesini birden fazla içeri aktarma oturumunda kullanmaya çalıştığınızda ortaya çıkar. <br> Bir sürücü kümesini yalnızca bir bölme ve kopyalama oturumu için kullanın.|
|[Hata] Özel durum oluştu: CopySessionId importdata-sept-test-1 eski bir kopyalama oturumunu gösteriyor ve yeni bir kopyalama oturumu için yeniden kullanılamaz.|Bu hata yeni bir işe önceden başarıyla tamamlanan bir işin adının verilmeye çalışılması durumunda bildirilir.<br> Yeni işiniz için benzersiz bir ad atayın.|
|[Bilgi] Hedef dosya veya dizin adı NTFS uzunluk sınırını aşıyor. |Bu ileti hedef dosya uzun dosya yolu nedeniyle yeniden adlandırıldığında bildirilir.<br> Bu davranışı denetlemek için `config.json` dosyasındaki değerlendirme seçeneğini değiştirin.|
|[Hata] Özel durum oluştu: Bozuk JSON kaçış dizisi. |Bu ileti config.json dosyasında geçersiz biçim olduğunda bildirilir. <br> Dosyayı kaydetmeden önce [JSONlint](https://jsonlint.com/) kullanarak `config.json` için doğrulama gerçekleştirin.|


## <a name="next-steps"></a>Sonraki adımlar

- Doğrulama aracı sorunlarını nasıl [gidereceklerini](data-box-disk-troubleshoot.md)öğrenin.
