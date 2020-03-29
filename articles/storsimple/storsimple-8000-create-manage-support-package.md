---
title: StorSimple 8000 serisi destek paketi oluşturma
description: StorSimple 8000 serisi aygıtınız için bir destek paketi oluşturmayı, şifreyi nasıl çözer ve nasıl kodamayı öğrenin.
author: alkohli
ms.service: storsimple
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: 9ca033f6f786c0142261dafa31b93b71a8b3336a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76277064"
---
# <a name="create-and-manage-a-support-package-for-storsimple-8000-series"></a>StorSimple 8000 serisi için bir destek paketi oluşturma ve yönetme

## <a name="overview"></a>Genel Bakış

StorSimple destek paketi, Microsoft Destek'e, StorSimple aygıt sorunlarını gidermede yardımcı olmak için ilgili tüm günlükleri toplayan kullanımı kolay bir mekanizmadır. Toplanan günlükler şifrelenir ve sıkıştırılır.

Bu öğretici, StorSimple 8000 serisi cihazınıziçin destek paketini oluşturmak ve yönetmek için adım adım talimatlar içerir. Bir StorSimple Sanal Dizi ile çalışıyorsanız, [bir günlük paketi oluşturmak](storsimple-ova-web-ui-admin.md#generate-a-log-package)için gidin.

## <a name="create-a-support-package"></a>Destek paketi oluşturma

Bazı durumlarda, StorSimple için Windows PowerShell üzerinden destek paketini el ile oluşturmanız gerekir. Örnek:

* Microsoft Destek ile paylaşmadan önce önemli bilgileri günlük dosyalarınızdan kaldırmanız gerekiyorsa.
* Bağlantı sorunları nedeniyle paketi yüklemekte güçlük çekiyorsanız.

El ile oluşturulan destek paketinizi e-posta üzerinden Microsoft Destek ile paylaşabilirsiniz. StorSimple için Windows PowerShell'de bir destek paketi oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell'de bir destek paketi oluşturmak için

1. StorSimple aygıtınıza bağlanmak için kullanılan uzak bilgisayarda yönetici olarak bir Windows PowerShell oturumu başlatmak için aşağıdaki komutu girin:
   
    `Start PowerShell`
2. Windows PowerShell oturumunda, cihazınızın SSAdmin Konsoluna bağlanın:
   
   1. Komut isteminde şunu girin:
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   2. Açılan iletişim kutusuna aygıt yöneticisi parolanızı girin. Varsayılan parola _Password1'dir._
     
      ![PowerShell kimlik bilgileri iletişim kutusu](./media/storsimple-8000-create-manage-support-package/IC740962.png)
   3. **Tamam'ı**seçin.
   4. Komut isteminde şunu girin:
     
      `Enter-PSSession $MS`
3. Açılan oturumda, uygun komutu girin.
   
   * Parola korumalı ağ paylaşımları için şunları girin:
     
       `Export-HcsSupportPackage -Path <\\IP address\location of the shared folder> -Include Default -Credential domainname\username`
     
       Bir parola ve şifreleme parolası için istenir (destek paketi şifrelenmiş olduğundan). Ardından varsayılan klasörde bir destek paketi oluşturulur (Aygıt adı geçerli tarih ve saatle birlikte eklenir).
   * Parola korumalı olmayan paylaşımlar için parametreye `-Credential` ihtiyacınız yoktur. Aşağıdakileri girin:
     
       `Export-HcsSupportPackage`
     
       Destek paketi varsayılan klasördeki her iki denetleyici için de oluşturulur. Paket, sorun giderme için Microsoft Destek'e gönderilebilen şifreli, sıkıştırılmış bir dosyadır. Daha fazla bilgi için [Microsoft Desteğine Başvurun'](storsimple-8000-contact-microsoft-support.md)a bakın.

### <a name="the-export-hcssupportpackage-cmdlet-parameters"></a>İhracat-HcsDestekPaketi cmdlet parametreleri

İhracat-HcsSupportPackage cmdlet ile aşağıdaki parametreleri kullanabilirsiniz.

| Parametre | Gerekli/İsteğe Bağlı | Açıklama |
| --- | --- | --- |
| `-Path` |Gerekli |Destek paketinin yerleştirildiği ağ paylaşılan klasörünün konumunu sağlamak için kullanın. |
| `-EncryptionPassphrase` |Gerekli |Destek paketini şifrelemeye yardımcı olmak için bir parola sağlamak için kullanın. |
| `-Credential` |İsteğe bağlı |Paylaşılan ağ klasörü için erişim kimlik bilgilerini sağlamak için kullanın. |
| `-Force` |İsteğe bağlı |Şifreleme parola onay adımını atlamak için kullanın. |
| `-PackageTag` |İsteğe bağlı |Destek paketinin yerleştirildiği *Yol* altında bir dizini belirtmek için kullanın. Varsayılan [aygıt adı]-[geçerli tarih ve saat:yyyy-MM-dd-HH-mm-ss]. |
| `-Scope` |İsteğe bağlı |Her iki denetleyici için de bir destek paketi oluşturmak için **Küme** (varsayılan) olarak belirtin. Yalnızca geçerli denetleyici için bir paket oluşturmak istiyorsanız, **Denetleyici'yi**belirtin. |

## <a name="edit-a-support-package"></a>Destek paketini edin

Bir destek paketi oluşturduktan sonra, hassas bilgileri kaldırmak için paketi kaldırmanız gerekebilir. Bu, günlük dosyalarındaki birim adları, aygıt IP adresleri ve yedekleme adlarını içerebilir.

> [!IMPORTANT]
> Yalnızca StorSimple için Windows PowerShell aracılığıyla oluşturulan bir destek paketini edinebilirsiniz. Azure portalında oluşturulan bir paketi StorSimple Device Manager hizmetiyle birlikte kaldıramaz.

Bir destek paketini Microsoft Destek sitesine yüklemeden önce yeniden yüklemek için, önce destek paketinin şifresini çözmek, dosyaları yeniden çözmek ve sonra yeniden şifrelemek için. Aşağıdaki adımları uygulayın.

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell'de bir destek paketini yeniden sağlamak için

1. [StorSimple için Windows PowerShell'de bir destek paketi oluşturmak için](#to-create-a-support-package-in-windows-powershell-for-storsimple)daha önce açıklandığı gibi bir destek paketi oluşturun.
2. Komut dosyasını istemcinize yerel olarak [indirin.](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65)
3. Windows PowerShell modüllerini içeri aktarın. Komut dosyasını indirdiğiniz yerel klasöre giden yolu belirtin. Modülü almak için şunları girin:
   
    `Import-module <Path to the folder that contains the Windows PowerShell script>`
4. Tüm dosyalar sıkıştırılmış ve şifrelenmiş *.aes* dosyalarıdır. Dosyaları decompress ve şifresini çözmek için girin:
   
    `Open-HcsSupportPackage <Path to the folder that contains support package files>`
   
    Gerçek dosya uzantılarının artık tüm dosyalar için görüntülendiğini unutmayın.
   
    ![Destek paketini edin](./media/storsimple-8000-create-manage-support-package/IC750706.png)
5. Şifreleme geçiş cümlesi için istendiğinde, destek paketi oluşturulduğunda kullandığınız parolayı girin.
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for the following parameters:EncryptionPassphrase: ****
6. Günlük dosyalarını içeren klasöre göz atın. Günlük dosyaları artık decompressed ve şifresi çözülmüş olduğundan, bu orijinal dosya uzantıları olacaktır. Birim adları ve aygıt IP adresleri gibi müşteriye özgü bilgileri kaldırmak ve dosyaları kaydetmek için bu dosyaları değiştirin.
7. Dosyaları gzip ile sıkıştırmak ve AES-256 ile şifrelemek için kapatın. Bu, destek paketinin bir ağ üzerinden aktarılmasında hız ve güvenlik içindir. Dosyaları sıkıştırmak ve şifrelemek için aşağıdakileri girin:
   
    `Close-HcsSupportPackage <Path to the folder that contains support package files>`
   
    ![Destek paketini edin](./media/storsimple-8000-create-manage-support-package/IC750707.png)
8. İstendiğinde, değiştirilen destek paketi için bir şifreleme geçiş cümlesi sağlayın.
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****
9. İstendiğinde Microsoft Destek ile paylaşabilmek için yeni parolayı yazın.

### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a>Örnek: Parola korumalı bir paylaşımda destek paketindeki dosyaları düzenleme

Aşağıdaki örnek, bir destek paketinin şifresini nasıl çözecek, güncellenebildiğini ve yeniden şifrelenebildiğini gösterir.

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a>Sonraki adımlar

* [Destek paketinde toplanan bilgiler](https://support.microsoft.com/help/3193606/storsimple-support-packages-and-device-logs) hakkında bilgi edinin
* [Cihaz dağıtımınızı gidermek için destek paketlerini ve aygıt günlüklerini](storsimple-8000-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting)nasıl kullanacağınızı öğrenin.
* [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini](storsimple-8000-manager-service-administration.md)nasıl kullanacağınızı öğrenin.

