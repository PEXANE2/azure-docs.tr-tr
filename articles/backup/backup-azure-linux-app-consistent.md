---
title: Linux VM'lerinin uygulama tutarlı yedekleri
description: Azure'da Linux sanal makinelerinizin uygulama tutarlı yedeklemelerini oluşturun. Bu makalede, komut dosyası çerçevesinin Azure tarafından dağıtılan Linux VM'lerini yedeklemek üzere yapılandırıldığı açıklanmaktadır. Bu makalede, sorun giderme bilgileri de içerir.
ms.reviewer: anuragm
ms.topic: conceptual
ms.date: 01/12/2018
ms.openlocfilehash: 36eeb9f63c67a01bf37412101e23be035596de94
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74173000"
---
# <a name="application-consistent-backup-of-azure-linux-vms"></a>Azure Linux VM'lerinin uygulama tutarlı yedeklemesi

VM'lerinizin yedek anlık görüntülerini alırken, uygulama tutarlılığı, uygulamalarınızın VM önyüklemesi geri yüklendikten sonra başladığı anlamına gelir. Tahmin edebileceğiniz gibi, uygulama tutarlılığı son derece önemlidir. Linux VM'lerinizin uygulama tutarlı olduğundan emin olmak için, uygulama tutarlı yedeklemeleri almak için Linux ön komut dosyası ve komut dosyası sonrası çerçevesini kullanabilirsiniz. Komut dosyası öncesi ve komut dosyası sonrası çerçeve, Azure Kaynak Yöneticisi tarafından dağıtılan Linux sanal makinelerini destekler. Uygulama tutarlılığı için komut dosyaları Service Manager tarafından dağıtılan sanal makineleri veya Windows sanal makinelerini desteklemez.

## <a name="how-the-framework-works"></a>Çerçeve nasıl çalışır?

Çerçeve, VM anlık görüntülerini alırken özel ön komut dosyaları ve komut dosyaları çalıştırma seçeneği sunar. Ön komut dosyaları VM anlık görüntüsünü almadan hemen önce çalışır ve komut dosyaları sonrası komut dosyaları VM anlık görüntüsünü aldıktan hemen sonra çalışır. Ön komut dosyaları ve komut dosyaları sonrası, VM anlık görüntülerini alırken uygulamanızı ve ortamınızı denetleme esnekliği sağlar.

Ön komut dosyaları, iOs'ları sorgulayan ve bellek içi içeriği diske temizleyen yerel uygulama API'lerini çağırır. Bu eylemler anlık görüntünün uygulama tutarlı olmasını sağlar. Komut dosyaları sonrası, uygulamanın VM anlık görüntüsünden sonra normal işlemleri sürdürmesini sağlayan iO'ları eritmek için yerel uygulama API'lerini kullanır.

## <a name="steps-to-configure-pre-script-and-post-script"></a>Ön komut dosyasını ve komut dosyası sonrası yapılandırma adımları

1. Yedeklemek istediğiniz Linux VM'nin kök kullanıcısı olarak oturum açın.

2. [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig)itibaren, **VMSnapshotScriptPluginConfig.json** indirin ve yedeklemek istediğiniz tüm VMs için **/ etc / azure** klasörüne kopyalayın. **/etc/azure** klasörü yoksa, oluşturun.

3. Yedeklemeyi planladığınız tüm VM'lerde uygulamanızın ön komut dosyasını ve post-script'i kopyalayın. Komut dosyalarını VM'deki herhangi bir konuma kopyalayabilirsiniz. **VMSnapshotScriptPluginConfig.json** dosyasındaki komut dosyası dosyalarının tam yolunu güncelleştirdiğinden emin olun.

4. Bu dosyalar için aşağıdaki izinleri sağlama:

   - **VMSnapshotScriptPluginConfig.json**: İzin "600." Örneğin, bu dosyaya yalnızca "root" kullanıcısının "okuma" ve "yazma" izinleri olmalıdır ve hiçbir kullanıcı "yürütme" izinlerine sahip olmamalıdır.

   - **Ön komut dosyası :** İzin "700."  Örneğin, yalnızca "root" kullanıcı "okumak", "yazmak" ve "yürütmek" izinleri bu dosyaya olmalıdır.

   - **Komut dosyası sonrası** "700"e izin ver. Örneğin, yalnızca "root" kullanıcı "okumak", "yazmak" ve "yürütmek" izinleri bu dosyaya olmalıdır.

   > [!IMPORTANT]
   > Çerçeve kullanıcılara çok fazla güç verir. Çerçeveyi güvenli hale getirmek ve yalnızca "root" kullanıcısının kritik JSON ve komut dosyası dosyalarına erişebilmesini sağlayın.
   > Gereksinimler karşılanmazsa, komut dosyası çalışmaz ve bu da dosya sistemi çökmesine ve tutarsız yedeklemeyle sonuçlanır.
   >

5. Burada açıklandığı gibi **VMSnapshotScriptPluginConfig.json** yapılandırın:
    - **pluginName**: Bu alanı olduğu gibi bırakın, yoksa komut dosyalarınız beklendiği gibi çalışmayabilir.

    - **preScriptLocation**: VM'de yedeklenecek olan ön komut dosyasının tam yolunu sağlayın.

    - **postScriptLocation**: Yedeklenecek olan VM'deki post-scriptin tam yolunu sağlayın.

    - **preScriptParams**: Ön komut dosyasına geçirilmesi gereken isteğe bağlı parametreleri sağlayın. Tüm parametreler tırnak içinde olmalıdır. Birden çok parametre kullanıyorsanız, parametreleri virgülle ayırın.

    - **postScriptParams**: Komut dosyası sonrası geçirilmesi gereken isteğe bağlı parametreleri sağlayın. Tüm parametreler tırnak içinde olmalıdır. Birden çok parametre kullanıyorsanız, parametreleri virgülle ayırın.

    - **preScriptNoOfRetries**: Sonlandırmadan önce herhangi bir hata varsa ön komut dosyasının kaç kez yeniden denenmesi gerektiğini ayarlayın. Sıfır, yalnızca bir deneme ve bir hata varsa yeniden deneme nin olmaması anlamına gelir.

    - **postScriptNoOfRetries**: Sonlandırmadan önce herhangi bir hata varsa, komut dosyası nın kaç kez yeniden denenmesi gerektiğini ayarlayın. Sıfır, yalnızca bir deneme ve bir hata varsa yeniden deneme nin olmaması anlamına gelir.

    - **timeoutInSeconds**: Ön komut dosyası ve komut dosyası sonrası için tek tek zaman ekmeleri belirtin (maksimum değer 1800 olabilir).

    - **continueBackupOnFailure**: Ön komut dosyası veya komut dosyası sonrası başarısız olursa Azure Yedekleme'nin tutarlı/kilitlenen tutarlı yedeklemedosyası sistemine geri dönmesini istiyorsanız bu değeri **doğru** olarak ayarlayın. Komut dosyası hatası durumunda yedeklemeyi **false** olarak ayarlamak başarısız olur (bu ayara bakılmaksızın kilitlenme tutarlı yedeklemeye geri dönen tek diskli VM'niz varsa).

    - **fsFreezeEnabled**: Dosya sistemi tutarlılığını sağlamak için VM anlık görüntüsünü alırken Linux fsfreeze'in çağrılması gerekip gerekmediğini belirtin. Uygulamanızın fsfreeze devre dışı bırakma bağımlılığı olmadığı sürece bu ayarı **doğru** tutmanızı öneririz.

    - **ScriptsExecutionPollTimeSeconds**: UzantAyın her anket arasında komut dosyası yürütmeiçin uyku süresi ayarlayın. Örneğin, değer 2 ise, uzantı ön/sonrası komut dosyası yürütmesinin her 2 saniyede bir tamamlanıp tamamlanmadığını denetler. Alacağı minimum ve maksimum değer sırasıyla 1 ve 5'tir. Değer kesinlikle bir tamsayı olmalıdır.

6. Komut dosyası çerçevesi şimdi yapılandırıldı. VM yedeklemesi zaten yapılandırılmışsa, bir sonraki yedekleme komut dosyalarını çağırır ve uygulama tutarlı yedeklemeyi tetikler. VM yedeklemesi yapılandırılmamışsa, [Azure sanal makinelerini Kurtarma Hizmetleri kasalarına Yedekleme'yi](https://docs.microsoft.com/azure/backup/backup-azure-vms-first-look-arm) kullanarak yapılandırın.

## <a name="troubleshooting"></a>Sorun giderme

Komut öncesi ve komut dosyası sonrası yazarken uygun günlüğe kaydetme yi eklediğinizden emin olun ve komut dosyası sorunlarını gidermek için komut dosyası günlüklerinizi gözden geçirin. Komut dosyalarını çalıştıran sorunlar varsa, daha fazla bilgi için aşağıdaki tabloya bakın.

| Hata | Hata iletisi | Önerilen eylem |
| ------------------------ | -------------- | ------------------ |
| Komut Öncesi Yürütme Başarısız Oldu |Ön komut dosyası bir hata döndürür, bu nedenle yedekleme uygulama tutarlı olmayabilir.| Sorunu gidermek için komut dosyanızın hata günlüklerine bakın.|  
|Komut Dosyası Sonrası Yürütme Başarısız Oldu |Post-script uygulama durumunu etkileyebilecek bir hata döndürür. |Sorunu gidermek ve uygulama durumunu denetlemek için komut dosyanızın hata günlüklerine bakın. |
| Ön Komut DosyasıBulunamadı |Ön komut dosyası **VMSnapshotScriptPluginConfig.json** config dosyasında belirtilen konumda bulunamadı. |Uygulama tutarlı yedekleme sağlamak için ön komut dosyasıconfig dosyasında belirtilen yolda mevcut olduğundan emin olun.|
| Post-scriptnotfound |Post-script **VMSnapshotScriptPluginConfig.json** config dosyasında belirtilen konumda bulunamadı. |Uygulama tutarlı yedekleme sağlamak için komut dosyası sonrası config dosyasında belirtilen yolda olduğundan emin olun.|
| YanlışPluginhostFile |VmSnapshotLinux uzantısı ile birlikte gelen **Pluginhost** dosyası bozuktur, bu nedenle ön komut dosyası ve post-script çalıştırılamaz ve yedekleme uygulama tutarlı olmayacaktır.| **VmSnapshotLinux** uzantısını kaldırın ve sorunu gidermek için otomatik olarak bir sonraki yedekleme ile yeniden yüklenir. |
| YanlışJSONConfigDosya | **VMSnapshotScriptPluginConfig.json** dosyası yanlıştır, bu nedenle ön komut dosyası ve komut dosyası sonrası çalıştırılamaz ve yedekleme uygulama tutarlı olmayacaktır. | [Kopyayı GitHub'dan](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig) indirin ve yeniden yapılandırın. |
| YetersizİzinÖn Komut Dosyası | Komut dosyalarını çalıştırmak için "root" kullanıcısı dosyanın sahibi olmalı ve dosya "700" izinlere sahip olmalıdır (diğer bir süre "sahibi"nin "okuması", "yazması" ve "yürütme" izinleri olmalıdır). | "Root" kullanıcı komut dosyası dosyasının "sahibi" olduğundan ve yalnızca "sahibi"nin "okuma", "yazma" ve "yürütme" izinlerine sahip olduğundan emin olun. |
| YetersizPermissionforPost-Script | Komut dosyalarını çalıştırmak için kök kullanıcı dosyanın sahibi olmalı ve dosya "700" izinlere sahip olmalıdır (diğer bir süre "sahibi"nin "okuması", "yazması" ve "yürütme" izinleri olmalıdır). | "Root" kullanıcı komut dosyası dosyasının "sahibi" olduğundan ve yalnızca "sahibi"nin "okuma", "yazma" ve "yürütme" izinlerine sahip olduğundan emin olun. |
| Komut ÖncesiZaman | Uygulama tutarlı yedekleme ön komut dosyası zamanlanmış yürütülmesi. | Komut dosyasını kontrol edin ve **/etc/azure**adresinde bulunan **VMSnapshotScriptPluginConfig.json** dosyasındaki zaman aşımını artırın. |
| Post-ScriptTimeout | Uygulama tutarlı yedekleme sonrası komut dosyası yürütülmesi zaman doldu. | Komut dosyasını kontrol edin ve **/etc/azure**adresinde bulunan **VMSnapshotScriptPluginConfig.json** dosyasındaki zaman aşımını artırın. |

## <a name="next-steps"></a>Sonraki adımlar

[VM yedeklemesini Kurtarma Hizmetleri kasasına yapılandırma](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms)
