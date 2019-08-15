---
title: "Azure Backup: Linux VM 'lerinin uygulamayla tutarlı yedeklemeleri"
description: Linux sanal makinelerinizin Azure 'da uygulamayla tutarlı yedeklerini oluşturun. Bu makalede, betik çerçevesinin Azure tarafından dağıtılan Linux VM 'lerini yedeklemek için yapılandırılması açıklanmaktadır. Bu makalede, sorun giderme bilgileri de yer alır.
ms.reviewer: anuragm
author: dcurwin
manager: carmonm
keywords: uygulamayla tutarlı yedekleme; uygulamayla tutarlı Azure VM yedeklemesi; Linux VM yedeklemesi; Azure Backup
ms.service: backup
ms.topic: conceptual
ms.date: 1/12/2018
ms.author: dacurwin
ms.openlocfilehash: bb764cfa14b4bc8e53f25629961a1baecd6a6c1f
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68954699"
---
# <a name="application-consistent-backup-of-azure-linux-vms"></a>Azure Linux VM 'lerinin uygulamayla tutarlı yedeklemesi

Sanal makinelerinizin yedek anlık görüntülerini alırken, uygulama tutarlılığı, uygulamalar geri yüklendikten sonra önyükleme yapıldığında uygulamalarınızın başlaması anlamına gelir. Imagine de, uygulama tutarlılığı son derece önemlidir. Linux sanal makinelerinizin uygulamayla tutarlı olduğundan emin olmak için, uygulamayla tutarlı yedeklemeler almak üzere Linux ön betiğini ve betik sonrası Framework 'ü kullanabilirsiniz. Ön betik ve son betik çerçevesi Azure Resource Manager dağıtılan Linux sanal makinelerini destekler. Uygulama tutarlılığı için betikler, Service Manager dağıtılan sanal makineleri veya Windows sanal makinelerini desteklemez.

## <a name="how-the-framework-works"></a>Framework nasıl çalışacaktır?

Framework, VM anlık görüntülerini alırken özel betik ve son betik çalıştırma seçeneği sunar. Ön betikler yalnızca VM anlık görüntüsünü almadan önce çalışır ve sanal makine anlık görüntüsünü aldıktan sonra betikleri anında çalışır. Ön betikler ve son betikler, VM anlık görüntülerini alırken uygulamanızı ve ortamınızı denetleme esnekliği sağlar.

Ön betikler yerel uygulama API 'Lerini çağırır, bu da IOs 'u sessiz moda alın ve bellek içi içeriği diske boşaltır. Bu eylemler, anlık görüntünün uygulamayla tutarlı olduğundan emin olur. Betikleri, uygulamanın VM anlık görüntüsünden sonra normal işlemleri sürdürmesini sağlayan IOs 'u çözme için yerel uygulama API 'Lerini kullanır.

## <a name="steps-to-configure-pre-script-and-post-script"></a>Betik öncesi ve betik sonrası yapılandırma adımları

1. Yedeklemek istediğiniz Linux sanal makinesinde kök kullanıcı olarak oturum açın.

2. [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig)'dan **VMSnapshotScriptPluginConfig. JSON** dosyasını indirip yedeklemek istediğiniz tüm VM 'ler için **makinelerdeki/etc/Azure** klasörüne kopyalayın. **Makinelerdeki/etc/Azure** klasörü yoksa, oluşturun.

3. Yedeklemeyi planladığınız tüm VM 'lerde uygulamanız için ön betik ve son betik kopyalama. Betikleri VM 'deki herhangi bir konuma kopyalayabilirsiniz. **VMSnapshotScriptPluginConfig. JSON** dosyasındaki betik dosyalarının tam yolunu güncelleştirdiğinizden emin olun.

4. Bu dosyalar için aşağıdaki izinlerin olduğundan emin olun:

   - **VMSnapshotScriptPluginConfig. JSON**: "600" izni Örneğin, bu dosyaya yalnızca "kök" kullanıcının "okuma" ve "yazma" izinlerinin olması gerekir ve hiçbir kullanıcının "yürütme" izinlerine sahip olması gerekir.

   - **Ön betik dosyası**: "700" izni  Örneğin, bu dosya için yalnızca "kök" kullanıcının "okuma", "yazma" ve "yürütme" izinlerine sahip olması gerekir.

   - **Betik sonrası** "700" izni Örneğin, bu dosya için yalnızca "kök" kullanıcının "okuma", "yazma" ve "yürütme" izinlerine sahip olması gerekir.

   > [!Important]
   > Framework kullanıcılara çok sayıda güç sunar. Framework 'ü güvenli hale getirin ve yalnızca "kök" kullanıcının kritik JSON ve betik dosyalarına erişimi olduğundan emin olun.
   > Gereksinimler karşılanmazsa, komut dosyası çalıştırılmaz, bu durum dosya sistemi kilitlenmesiyle ve tutarsız yedeklemeye neden olur.
   >

5. **VMSnapshotScriptPluginConfig. JSON** ' i burada açıklandığı gibi yapılandırın:
    - **PluginName**: Bu alanı olduğu gibi bırakın veya betikleriniz beklendiği gibi çalışmayabilir.

    - **Öngörütkonumu**: Yedeklenecek sanal makinede ön betiğin tam yolunu belirtin.

    - **Postscriptlocation**: Yedeklenecek sanal makinede, son betiğin tam yolunu belirtin.

    - Her ne kadar **Ptparams**: Ön betikte geçirilmesi gereken isteğe bağlı parametreleri sağlayın. Tüm parametrelerin tırnak içinde olması gerekir. Birden çok parametre kullanıyorsanız, parametreleri virgülle ayırın.

    - **Postscriptparams**: Betiğe geçirilmesi gereken isteğe bağlı parametreleri sağlayın. Tüm parametrelerin tırnak içinde olması gerekir. Birden çok parametre kullanıyorsanız, parametreleri virgülle ayırın.

    - Tüm **denemeler**: Sonlandırmadan önce herhangi bir hata oluşursa, ön betiğin yeniden denenme sayısını ayarlayın. Sıfır, bir hata varsa yalnızca bir try ve retry denemesi anlamına gelir.

    - **Postscriptnoofdenemeler**:  Sonlandırmadan önce herhangi bir hata oluşursa, son betiğin yeniden denenme sayısını ayarlayın. Sıfır, bir hata varsa yalnızca bir try ve retry denemesi anlamına gelir.

    - **Timeoutınseconds**: Ön betik ve son betik için zaman aşımlarını belirtin (maksimum değer 1800 olabilir).

    - **devam Backuponfailure**: Azure Backup ön betik veya komut dosyası sonrası başarısız olursa, bir dosya sistemine tutarlı/kilitlenmeyle tutarlı bir yedeklemeye geri döneceğini istiyorsanız bu değeri **true** olarak ayarlayın. Bunu **yanlış** olarak ayarlamak, komut dosyası hatası durumunda yedekleme işlemi başarısız olur (bu ayardan bağımsız olarak, kilitlenmeyle tutarlı yedeklemeye geri kalan tek disk sanal makinenizin olması dışında).

    - **Fsfreezeenabled**: Dosya sistemi tutarlılığını sağlamak için VM anlık görüntüsünü alırken Linux fsfreeze 'in çağrılması gerekip gerekmediğini belirtin. Uygulamanızda fsfreeze devre dışı bırakma bağımlılığı yoksa, bu ayarın **true** olarak tutulmasını öneririz.

6. Betik çerçevesi artık yapılandırıldı. VM yedeklemesi zaten yapılandırıldıysa, sonraki yedekleme betikleri çağırır ve uygulamayla tutarlı yedeklemeyi tetikler. VM yedeklemesi yapılandırılmamışsa, [Azure sanal makinelerini kurtarma hizmetleri kasalarına yedekle](https://docs.microsoft.com/azure/backup/backup-azure-vms-first-look-arm) seçeneğini kullanarak yapılandırın.

## <a name="troubleshooting"></a>Sorun giderme

Komut dosyası ve betik sonrası bilgilerinizi yazarken uygun günlük kaydını eklediğinizden emin olun ve betik oluşturma sorunlarını gidermek için betik günlüklerinizi gözden geçirin. Betikleri çalıştırırken sorun yaşamaya devam ediyorsanız daha fazla bilgi için aşağıdaki tabloya bakın.

| Hata | Hata iletisi | Önerilen eylem |
| ------------------------ | -------------- | ------------------ |
| Pre-ScriptExecutionFailed |Ön betik bir hata döndürdü, bu nedenle yedekleme uygulamayla tutarlı olmayabilir.   | Sorunu çözmesi için betiğinizin hata günlüklerine bakın.|  
|   Scriptexecutionpost başarısız oldu |    Betik sonrası uygulama durumunu etkileyebilecek bir hata döndürdü. |    Sorunu düzeltemedi ve uygulama durumunu kontrol etmek için betikinizin hata günlüklerine bakın. |
| ScriptNotFound |  **VMSnapshotScriptPluginConfig. JSON** yapılandırma dosyasında belirtilen konumda ön betik bulunamadı. |   Uygulamayla tutarlı yedekleme sağlamak için yapılandırma dosyasında belirtilen yolda ön betikte bulunduğundan emin olun.|
| ScriptNotFound sonrası | Betik sonrası **VMSnapshotScriptPluginConfig. JSON** yapılandırma dosyasında belirtilen konumda bulunamadı. |   Uygulamayla tutarlı yedekleme sağlamak için yapılandırma dosyasında belirtilen yolda komut dosyasının mevcut olduğundan emin olun.|
| IncorrectPluginhostFile | VmSnapshotLinux uzantısıyla birlikte gelen **Pluginhost** dosyası bozuk, bu nedenle ön betik ve son betik çalıştırılamıyor ve yedekleme uygulamayla tutarlı olmayacak. | **VmSnapshotLinux** uzantısını kaldırın ve sorunu gidermek için otomatik olarak sonraki yedeklemeyle yeniden yüklenecektir. |
| IncorrectJSONConfigFile | **VMSnapshotScriptPluginConfig. JSON** dosyası yanlış, bu nedenle ön betik ve son betik çalıştırılamıyor ve yedekleme uygulamayla tutarlı olmayacak. | Kopyayı [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig) 'dan indirin ve yeniden yapılandırın. |
| InsufficientPermissionforPre-betik | Betikleri çalıştırmak için, "kök" kullanıcının dosyanın sahibi olması ve dosyada "700" izinlerinin olması gerekir (yani yalnızca "Owner", "okuma", "yazma" ve "yürütme" izinlerine sahip olmalıdır). | "Kök" kullanıcının betik dosyasının "sahip" olduğundan ve yalnızca "Owner" öğesinin "okuma", "yazma" ve "yürütme" izinlerine sahip olduğundan emin olun. |
| InsufficientPermissionforPost-betik | Betikleri çalıştırmak için, kök kullanıcı dosyanın sahibi olmalıdır ve dosya "700" izinlerine sahip olmalıdır (yani yalnızca "Owner", "okuma", "yazma" ve "yürütme" izinlerine sahip olmalıdır). | "Kök" kullanıcının betik dosyasının "sahip" olduğundan ve yalnızca "Owner" öğesinin "okuma", "yazma" ve "yürütme" izinlerine sahip olduğundan emin olun. |
| ScriptTimeout öncesi | Uygulamayla tutarlı yedekleme ön betiğini yürütme zaman aşımına uğradı. | Komut dosyasını denetleyin ve **makinelerdeki/etc/Azure**'da bulunan **VMSnapshotScriptPluginConfig. JSON** dosyasındaki zaman aşımını artırın. |
| ScriptTimeout sonrası | Uygulamayla tutarlı yedekleme sonrası komut dosyasının yürütülmesi zaman aşımına uğradı. | Komut dosyasını denetleyin ve **makinelerdeki/etc/Azure**'da bulunan **VMSnapshotScriptPluginConfig. JSON** dosyasındaki zaman aşımını artırın. |

## <a name="next-steps"></a>Sonraki adımlar
[VM yedeklemesini bir kurtarma hizmetleri kasasına yapılandırma](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms)
