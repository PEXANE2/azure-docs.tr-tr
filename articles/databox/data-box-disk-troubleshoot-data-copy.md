---
title: Azure Data Box Disk veri kopyalama sorunlarını giderme | Microsoft Docs
description: Günlükleri kullanarak Azure Data Box Disk veri kopyalama sırasında görülen sorunların nasıl giderileceği açıklanmaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/13/2019
ms.author: alkohli
ms.openlocfilehash: 760f5c6c929aa082993683d7a466a71c6484289a
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "67148354"
---
# <a name="troubleshoot-data-copy-issues-in-azure-data-box-disk"></a>Azure Data Box Disk 'de veri kopyalama sorunlarını giderme

Bu makale, Microsoft Azure Data Box Disk için geçerlidir ve verileri disklere kopyalarken gördüğünüz sorunları nasıl giderebileceğinizi açıklar. Bu makalede bölünmüş kopyalama Aracı kullanılırken de sorunlar ele alınmaktadır.


## <a name="data-copy-issues-when-using-a-linux-system"></a>Linux sistemi kullanılırken veri kopyalama sorunları

Bu bölümde, verileri disklere kopyalamak için bir Linux istemcisi kullanılırken ortaya çıkan başlıca sorunlardan bazıları ayrıntılı olarak ele verilebilir.

### <a name="issue-drive-getting-mounted-as-read-only"></a>Sorun: sürücü, salt okuma olarak bağlandı
 
**Sağlamak** 

Bunun nedeni, temiz olmayan bir dosya sistemi olabilir.

Bir sürücünün okuma-yazma olarak takılması Data Box disklerle birlikte çalışmaz. Bu senaryo, disdolabı tarafından şifresi çözülen sürücülerle desteklenmez. Aşağıdaki komutu kullanarak cihazı başarıyla yeniden dağıtabilirsiniz:

    `# mount -o remount, rw /mnt/DataBoxDisk/mountVol1`

Geri bağlanıcı başarılı olsa da veriler devam etmez.

**Çözünürlük**

Linux sisteminizde aşağıdaki adımları uygulayın:

1. Ntfsdüzeltmesini yardımcı programı için `ntfsprogs` paketi yükler.
2. Kilit açma aracı tarafından sürücü için belirtilen bağlama noktalarını çıkarın. Bağlama noktası sayısı sürücüler için farklılık gösterecektir.

    ```
    unmount /mnt/DataBoxDisk/mountVol1
    ```

3. İlgili `ntfsfix` yol üzerinde çalıştırın. Vurgulanan sayı adım 2 ile aynı olmalıdır.

    ```
    ntfsfix /mnt/DataBoxDisk/bitlockerVol1/dislocker-file
    ```

4. Bağlama sorununa neden olabilecek hazırda bekleme meta verilerini kaldırmak için aşağıdaki komutu çalıştırın.

    ```
    ntfs-3g -o remove_hiberfile /mnt/DataBoxDisk/bitlockerVol1/dislocker-file /mnt/DataBoxDisk/mountVol1
    ```

5. Temiz bir çıkarma yapın.

    ```
    ./DataBoxDiskUnlock_x86_64 /unmount
    ```

6. Temiz bir kilit açma ve bağlama yapın.
7. Bağlama noktasını bir dosya yazarak test edin.
8. Dosya kalıcılığını doğrulamak için çıkarın ve yeniden bağlayın.
9. Veri kopyasıyla devam edin.
 
### <a name="issue-error-with-data-not-persisting-after-copy"></a>Sorun: kopyalama işleminden sonra kalıcı olmayan verilerle ilgili hata
 
**Sağlamak** 

Sürücünün takıldıktan sonra veri olmadığını görürseniz (veriler kendisine kopyalansa da), Sürücü salt okunurdur olarak bağlandıktan sonra bir sürücüyü okuma-yazma olarak yeniden takmış olmanız mümkündür.

**Çözünürlük**
 
Bu durumda, [salt okuma olarak bağlanmış sürücüler](#issue-drive-getting-mounted-as-read-only)için çözünürlüğe bakın.

Böyle bir durum söz konusu değilse, günlükleri Data Box Disk kilit açma aracının bulunduğu klasörden kopyalayın ve [Microsoft desteği başvurun](data-box-disk-contact-microsoft-support.md).


## <a name="data-box-disk-split-copy-tool-errors"></a>Data Box Disk Split Copy aracı hataları

Verileri birden çok diske ayırmak için bölünmüş kopyalama Aracı kullanılırken görülen sorunlar aşağıdaki tabloda özetlenmiştir.

|Hata iletisi/Uyarılar |Öneriler |
|---------|---------|
|Bilgisine Birim için BitLocker parolası alınıyor: a <br>Hatayla B birimi için BitLocker anahtarı alınırken özel durum yakalandı:<br> Sıra hiçbir öğe içermiyor.|Bu hata hedef Data Box Disk çevrimdışı olduğunda gösterilir. <br> Diskleri çevrimiçi duruma getirmek için `diskmgmt.msc` aracını kullanın.|
|[Hata] Özel durum oluştu: WMI işlemi başarısız oldu:<br> Method=UnlockWithNumericalPassword, ReturnValue=2150694965, <br>Win32Message=Girilen kurtarma parolası biçimi geçersiz. <br>BitLocker kurtarma parolaları 48 hanelidir. <br>Kurtarma parolasının doğru biçimde olduğunu doğrulayıp yeniden deneyin.|Data Box Disk kilit açma aracını kullanarak disklerin kilidini açın ve komutu yeniden deneyin. Daha fazla bilgi için bkz. <li> [Windows istemcileri için Data Box Disk kilidini açma](data-box-disk-deploy-set-up.md#unlock-disks-on-windows-client). </li><li> [Linux istemcileri için Data Box Disk kilidini açma](data-box-disk-deploy-set-up.md#unlock-disks-on-linux-client). </li>|
|[Hata] Özel durum oluştu: Hedef sürücüde DriveManifest.xml dosyası var. <br> Bu durum hedef sürücünün farklı bir günlük dosyasıyla hazırlanmış olabileceğini gösterir. <br>Aynı sürücüye daha fazla veri eklemek için önceki günlük dosyasını kullanın. Mevcut verileri silmek ve yeni bir içeri aktarma işi için hedef sürücüyü yeniden kullanmak için, sürücüdeki *Drivemanifest. xml* ' yi silin. Bu komutu yeni bir günlük dosyasıyla yeniden çalıştırın.| Bu hata aynı sürücü kümesini birden fazla içeri aktarma oturumunda kullanmaya çalıştığınızda ortaya çıkar. <br> Bir sürücü kümesini yalnızca bir bölme ve kopyalama oturumu için kullanın.|
|[Hata] Özel durum oluştu: CopySessionId importdata-sept-test-1 eski bir kopyalama oturumunu gösteriyor ve yeni bir kopyalama oturumu için yeniden kullanılamaz.|Bu hata yeni bir işe önceden başarıyla tamamlanan bir işin adının verilmeye çalışılması durumunda bildirilir.<br> Yeni işiniz için benzersiz bir ad atayın.|
|[Bilgi] Hedef dosya veya dizin adı NTFS uzunluk sınırını aşıyor. |Bu ileti hedef dosya uzun dosya yolu nedeniyle yeniden adlandırıldığında bildirilir.<br> Bu davranışı denetlemek için `config.json` dosyasındaki değerlendirme seçeneğini değiştirin.|
|[Hata] Özel durum oluştu: Bozuk JSON kaçış dizisi. |Bu ileti config.json dosyasında geçersiz biçim olduğunda bildirilir. <br> Dosyayı kaydetmeden önce [JSONlint](https://jsonlint.com/) kullanarak `config.json` için doğrulama gerçekleştirin.|


## <a name="next-steps"></a>Sonraki adımlar

- [Doğrulama aracı sorunlarını giderme](data-box-disk-troubleshoot.md)hakkında bilgi edinin.
