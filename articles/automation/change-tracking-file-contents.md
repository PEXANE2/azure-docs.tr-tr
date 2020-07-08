---
title: Azure Otomasyonu 'nda Değişiklik İzleme ve stoku yönetme
description: Bu makalede, ortamınızdaki yazılım ve Microsoft hizmet değişikliklerini izlemek için Değişiklik İzleme ve envanterin nasıl kullanılacağı açıklanır.
services: automation
ms.subservice: change-inventory-management
ms.date: 06/15/2020
ms.topic: conceptual
ms.openlocfilehash: 0eebd626013614bb6240fc0e6530a358a2b86d1c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84781200"
---
# <a name="manage-change-tracking-and-inventory"></a>Değişiklik İzleme ve Stok yönetimi

İzlemek üzere yeni bir dosya veya kayıt defteri anahtarı eklediğinizde Azure Otomasyonu, [değişiklik izleme ve envanter](change-tracking.md)için bunu sağlar. Bu makalede izleme yapılandırma, izleme sonuçlarının incelenmesi ve değişiklikler algılandığında uyarıları işlemek açıklanır.

Bu makaledeki yordamları kullanmadan önce, aşağıdaki tekniklerden birini kullanarak sanal makinelerinizdeki Değişiklik İzleme ve envanteri etkinleştirdiğinizden emin olun:

* [Otomasyon hesabından Değişiklik İzleme ve Stok özelliğini etkinleştirme](automation-enable-changes-from-auto-acct.md)
* [Azure portal göz atarak Değişiklik İzleme ve envanteri etkinleştirin](automation-enable-changes-from-browse.md)
* [Runbook’tan Değişiklik İzleme ve Stok özelliğini etkinleştirme](automation-enable-changes-from-runbook.md)
* [Azure VM’den Değişiklik İzleme ve Stok özelliğini etkinleştirme](automation-enable-changes-from-vm.md)

## <a name="limit-the-scope-for-the-deployment"></a><a name="scope-configuration"></a>Dağıtımın kapsamını sınırlandırma

Değişiklik İzleme ve stok, değişiklikleri almak üzere bilgisayarları hedeflemek için çalışma alanındaki kapsam yapılandırmasını kullanır. Daha fazla bilgi için bkz. [Limit değişiklik izleme ve Inventory Deployment Scope](automation-scope-configurations-change-tracking.md).

## <a name="track-files"></a>Dosyaları izle

Dosya ve klasör/dizinlerdeki değişiklikleri izlemek için Değişiklik İzleme ve envanteri kullanabilirsiniz. Bu bölümde, Windows 'da ve Linux 'ta dosya izlemenin nasıl yapılandırılacağı açıklanır.

### <a name="configure-file-tracking-on-windows"></a>Windows 'da dosya izlemeyi yapılandırma

Windows bilgisayarlarda dosya izlemeyi yapılandırmak için aşağıdaki adımları kullanın:

1. Otomasyon hesabınızda **yapılandırma yönetimi**altında **değişiklik izleme** ' yi seçin. 
2. **Ayarları Düzenle** (dişli simgesi) seçeneğine tıklayın.
3. Çalışma alanı yapılandırması sayfasında **Windows dosyaları**' nı seçin ve ardından **+ Ekle** ' ye tıklayarak izlenecek yeni bir dosya ekleyin.
4. Değişiklik İzleme için Windows dosyası Ekle bölmesinde, izlenecek dosya veya klasör için bilgileri girin ve **Kaydet**' e tıklayın. Aşağıdaki tabloda, bilgiler için kullanabileceğiniz özellikler tanımlanmaktadır.

    |Özellik  |Açıklama  |
    |---------|---------|
    |Etkin     | Ayar uygulanmışsa true, aksi takdirde false.        |
    |Öğe Adı     | İzlenecek dosyanın kolay adı.        |
    |Grup     | Dosyaları mantıksal olarak gruplandırmak için bir grup adı.        |
    |Yolu girin     | Dosyanın denetlenecek yol (örneğin, **c:\Temp \\ \* . txt**). Ayrıca, gibi ortam değişkenlerini de kullanabilirsiniz `%winDir%\System32\\\*.*` .       |
    |Yol Türü     | Yolun türü. Olası değerler dosya ve klasördür.        |    
    |Özyineleme     | Bu öğeyi izlenecek öğe ararken, aksi takdirde false ise true.        |    
    |Dosya içeriğini karşıya yükle | İzlenen değişikliklerle dosya içeriğini karşıya yüklemek için true, aksi durumda false.|

5. **Dosya Içeriğini karşıya yüklemek**Için true değerini belirttiğinizden emin olun. Bu ayar, belirtilen dosya yolu için dosya içeriği izlemeyi etkinleştirilir.

### <a name="configure-file-tracking-on-linux"></a>Linux 'ta dosya izlemeyi yapılandırma

Linux bilgisayarlarda dosya izlemeyi yapılandırmak için aşağıdaki adımları kullanın:

1. Otomasyon hesabınızda **yapılandırma yönetimi**altında **değişiklik izleme** ' yi seçin. 
2. **Ayarları Düzenle** (dişli simgesi) seçeneğine tıklayın.
3. Çalışma alanı yapılandırma sayfasında, **Linux dosyaları**' nı seçin ve ardından **+ Ekle** ' ye tıklayarak izlenecek yeni bir dosya ekleyin.
4. Değişiklik İzleme için Linux dosyası Ekle bölmesinde, izlenecek dosya veya dizin bilgilerini girin ve **Kaydet**' e tıklayın. Aşağıdaki tabloda, bilgiler için kullanabileceğiniz özellikler tanımlanmaktadır.

    |Özellik  |Açıklama  |
    |---------|---------|
    |Etkin     | Ayar uygulanmışsa true, aksi takdirde false.        |
    |Öğe Adı     | İzlenecek dosyanın kolay adı.        |
    |Grup     | Dosyaları mantıksal olarak gruplandırmak için bir grup adı.        |
    |Yolu girin     | Dosyayı denetlenecek yol, örneğin, **/etc/*. conf**.       |
    |Yol Türü     | Yolun türü. Olası değerler dosya ve dizindir.        |
    |Özyineleme     | Bu öğeyi izlenecek öğe ararken, aksi takdirde false ise true.        |
    |Sudo Kullan     | Öğe denetlenirken sudo 'ı kullanmak için true, aksi durumda false.         |
    |Bağlantılar     | Dizinler arasında geçiş yaparken sembolik bağlantılarla nasıl başa çıkılacağını belirleyen ayar. Olası değerler şunlardır:<br> Ignore-sembolik bağlantıları yoksayar ve başvurulan dosya/dizinleri içermez.<br>Takip et-özyineleme sırasında sembolik bağlantıları Izler ve başvurulan dosya/dizinleri de içerir.<br>Yönet-sembolik bağlantıları Izler ve döndürülen içeriğin değiştirilmesine izin verir.<br>**Note:** Dosya içeriğini almayı desteklemediğinden Yönet seçeneği önerilmez.    |
    |Dosya içeriğini karşıya yükle | İzlenen değişikliklerle dosya içeriğini karşıya yüklemek için true, aksi durumda false. |

5. **Dosya Içeriğini karşıya yüklemek**Için true değerini belirttiğinizden emin olun. Bu ayar, belirtilen dosya yolu için dosya içeriği izlemeyi etkinleştirilir.

   ![Linux dosyası Ekle](./media/change-tracking-file-contents/add-linux-file.png)

## <a name="track-file-contents"></a>Dosya içeriğini izleme

Dosya içeriği izleme, izlenen bir değişiklikten önceki ve sonraki bir dosyanın içeriğini görüntülemenize izin verir. Özellik, her değişiklik oluştuktan sonra dosya içeriğini bir [depolama hesabına](https://docs.microsoft.com/azure/storage/common/storage-account-overview) kaydeder. Dosya içeriğini izlemek için izlenecek bazı kurallar aşağıda verilmiştir:

* Dosya içeriğini depolamak için Kaynak Yöneticisi dağıtım modelini kullanan standart bir depolama hesabı gerekir. 
* Premium ve klasik dağıtım modeli depolama hesapları kullanmayın. Bkz. [Azure depolama hesapları hakkında](../storage/common/storage-create-storage-account.md).
* Depolama hesabını yalnızca bir Otomasyon hesabına bağlayabilirsiniz.
* Otomasyon hesabınızda [değişiklik izleme ve envanter](change-tracking.md) etkinleştirilmelidir.

### <a name="enable-tracking-for-file-content-changes"></a>Dosya içeriği değişiklikleri için izlemeyi etkinleştir

Dosya içeriklerinde yapılan değişikliklere yönelik izlemeyi etkinleştirmek için aşağıdaki adımları kullanın:

1. Azure portal, Otomasyon hesabınızı açın ve **yapılandırma yönetimi**altında **değişiklik izleme** ' yi seçin.
2. **Ayarları Düzenle** (dişli simgesi) seçeneğine tıklayın.
3. **Dosya içeriği** ' ni seçin ve **bağlantı**' ya tıklayın. Bu seçim, Değişiklik İzleme için Içerik konumu Ekle bölmesini açar.

   ![İçerik konumu Ekle](./media/change-tracking-file-contents/enable.png)

4. Dosya içeriğini depolamak için kullanılacak aboneliği ve depolama hesabını seçin. 

5. Tüm mevcut izlenen dosyalar için dosya içeriği izlemeyi etkinleştirmek istiyorsanız, **Tüm ayarlar için dosya Içeriğini karşıya yükle** **' yi seçin.** Bu ayarı, her dosya yolu için daha sonra değiştirebilirsiniz.

   ![Depolama hesabı ayarla](./media/change-tracking-file-contents/storage-account.png)

6. Değişiklik İzleme ve Inventory, dosya içeriği değişiklik izlemeyi etkinleştirdiğinden depolama hesabı ve paylaşılan erişim Imzası (SAS) URI 'Lerini gösterir. İmzaların kullanım süreleri 365 gün sonra doluyor ve yeniden **Oluştur**' a tıklayarak yeniden oluşturabilirsiniz.

   ![Hesap anahtarlarını listeleme](./media/change-tracking-file-contents/account-keys.png)

### <a name="view-the-contents-of-a-tracked-file"></a>İzlenen bir dosyanın içeriğini görüntüleme

Değişiklik İzleme ve envanter izlenen bir dosya için değişiklik algıladığında, değişiklik ayrıntıları bölmesinde dosya içeriğini görüntüleyebilirsiniz.  

![Liste değişiklikleri](./media/change-tracking-file-contents/change-list.png)

1. Azure portal, Otomasyon hesabınızı açın ve **yapılandırma yönetimi**altında **değişiklik izleme** ' yi seçin.

2. Değişiklik listesinden bir dosya seçin ve dosyanın içeriğini görmek için **Dosya Içeriğini görüntüle değişikliklerini** seçin. Değişiklik ayrıntıları bölmesi, her bir özellik için dosya bilgilerinin önüne ve sonrasına ilişkin standart bir araç görüntüler.

   ![Değişiklik ayrıntıları](./media/change-tracking-file-contents/change-details.png)

3. Dosya içeriğini yan yana görünümde görüntülüyorsunuz. Değişikliklerin satır içi görünümünü görmek için **satır içi** seçeneğini belirleyebilirsiniz.

## <a name="track-registry-keys"></a>Kayıt defteri anahtarlarını izleme

Windows bilgisayarlarda kayıt defteri anahtarı izlemeyi yapılandırmak için aşağıdaki adımları kullanın:

1. Azure portal, Otomasyon hesabınızı açın ve **yapılandırma yönetimi**altında **değişiklik izleme** ' yi seçin. 
2. **Ayarları Düzenle** (dişli simgesi) seçeneğine tıklayın.
3. Çalışma alanı yapılandırması sayfasında **Windows kayıt defteri**' ni seçin.
4. İzlemek üzere yeni bir kayıt defteri anahtarı eklemek için **+ Ekle** ' ye tıklayın.
5. Değişiklik İzleme için Windows kayıt defteri Ekle bölmesinde, izlenecek anahtarın bilgilerini girin ve **Kaydet**' e tıklayın. Aşağıdaki tabloda, bilgiler için kullanabileceğiniz özellikler tanımlanmaktadır.

    |Özellik  |Açıklama  |
    |---------|---------|
    |Etkin     | Bir ayar uygulanmışsa true, aksi takdirde false.        |
    |Öğe Adı     | İzlenecek kayıt defteri anahtarının kolay adı.        |
    |Grup     | Kayıt defteri anahtarlarını mantıksal olarak gruplandırmak için Grup adı.        |
    |Windows Kayıt Defteri Anahtarı   | Yol içeren anahtar adı, örneğin, `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup` .      |

## <a name="search-logs-for-change-records"></a>Değişiklik kayıtları için arama günlükleri

Değişiklik kayıtları için Azure Izleyici günlüklerinde çeşitli aramalar yapabilirsiniz. Değişiklik izleme sayfası açıkken, Günlükler sayfasını açmak için **Log Analytics** ' a tıklayın. Aşağıdaki tabloda değişiklik kayıtları için örnek günlük aramaları sunulmaktadır.

|Sorgu  |Açıklama  |
|---------|---------|
|`ConfigurationData`<br>&#124;`where ConfigDataType == "WindowsServices" and SvcStartupType == "Auto"`<br>&#124;`where SvcState == "Stopped"`<br>&#124;`summarize arg_max(TimeGenerated, *) by SoftwareName, Computer`         | Otomatik olarak ayarlanan ancak durdurulma olarak bildirilen Microsoft Hizmetleri için en son envanter kayıtlarını gösterir. Sonuçlar, belirtilen yazılım adı ve bilgisayar için en son kayıtla sınırlıdır.    |
|`ConfigurationChange`<br>&#124;`where ConfigChangeType == "Software" and ChangeCategory == "Removed"`<br>&#124;`order by TimeGenerated desc`|Kaldırılan yazılım için değişiklik kayıtlarını gösterir.|

## <a name="create-alerts-on-changes"></a>Değişiklikler üzerinde uyarı oluştur

Aşağıdaki örnek, bir makinede **c:\Windows\system32\drivers\etc\hosts** dosyasının değiştirildiğini gösterir. Windows bunu, ana bilgisayar adlarını IP adreslerine çözümlemek üzere kullandığından, bu dosya önemlidir. Bu işlem DNS 'den önceliklidir ve bağlantı sorunlarına yol açabilir. Ayrıca, trafiğin kötü amaçlı ya da başka türlü tehlikeli Web sitelerine yönlendirilmesine de yol açabilir.

![Hosts dosyası değişikliğini gösteren grafik](./media/change-tracking-file-contents/changes.png)

Bu örneği bir değişiklik üzerinde uyarı oluşturma adımlarını tartışmak için kullanalım.

1. Otomasyon hesabınızda **yapılandırma yönetimi**altında **izlemeyi Değiştir** ' i seçin ve ardından **Log Analytics**' yi seçin. 
2. Günlükler aramasında, **Hosts** dosyasında bulunan içerik değişikliklerini sorgu ile arayın `ConfigurationChange | where FieldsChanged contains "FileContentChecksum" and FileSystemPath contains "hosts"` . Bu sorgu, sözcüğü içeren tam nitelikli yol adlarına sahip dosyalar için içerik değişikliklerini arar `hosts` . Ayrıca, örneğin kullanarak yol bölümünü tam nitelikli biçimde değiştirerek belirli bir dosyayı sorabilirsiniz `FileSystemPath == "c:\windows\system32\drivers\etc\hosts"` .

3. Sorgu sonuçları geri döndüğünde, uyarı oluşturma sayfasını açmak için günlük aramasında **Yeni uyarı kuralı** ' na tıklayın. Ayrıca, Azure portal **Azure izleyici** aracılığıyla bu sayfaya gidebilirsiniz. 

4. Sorgunuzu yeniden denetleyin ve uyarı mantığını değiştirin. Bu durumda, ortamdaki tüm makinelerde tek bir değişiklik algılanırsa uyarının tetiklenmesi istersiniz.

    ![Ana bilgisayar dosyasındaki değişiklikleri izlemek için sorguya Değiştir](./media/change-tracking-file-contents/change-query.png)

5. Uyarı mantığı ayarlandıktan sonra, uyarının tetiklenme yanıt olarak eylem gerçekleştirmek için eylem grupları atayın. Bu durumda, gönderilecek e-postaları ve oluşturulacak bir BT hizmet yönetimi (ıTSM) anahtarını ayarlıyoruz. 

    ![Eylem grubunu değişiklik üzerinde uyarı verecek şekilde yapılandırma](./media/change-tracking/action-groups.png)

## <a name="next-steps"></a>Sonraki adımlar

* Kapsam konfigürasyonları hakkında daha fazla bilgi için bkz. [Limit değişiklik izleme and Inventory Deployment Scope](automation-scope-configurations-change-tracking.md).
* Log Analytics çalışma alanınızda depolanan günlüklerde arama yapmanız gerekiyorsa bkz. [Azure izleyici günlüklerinde günlük aramaları](../log-analytics/log-analytics-log-searches.md).
* Dağıtımlar ile tamamlandıysa, [değişiklik izleme ve envanter için çalışma alanının Otomasyon hesabından bağlantısını](automation-unlink-workspace-change-tracking.md)kaldırma bölümüne bakın.
* VM 'lerinizi Değişiklik İzleme ve envanterden silmek için bkz. [VM 'leri değişiklik izleme ve envanterden kaldırma](automation-remove-vms-from-change-tracking.md).
* Özellik hatalarını gidermek için bkz. [değişiklik izleme ve envanter sorunlarını giderme](troubleshoot/change-tracking.md).
