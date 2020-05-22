---
title: Azure Otomasyonu 'nda Değişiklik İzleme ve stoku yönetme
description: Bu makalede, ortamınızdaki yazılım ve Microsoft hizmet değişikliklerini izlemek için Değişiklik İzleme ve envanterin nasıl kullanılacağı açıklanır.
services: automation
ms.subservice: change-inventory-management
ms.date: 07/03/2018
ms.topic: conceptual
ms.openlocfilehash: 8e5ee8df1dfd250a6713d832bf176daecdaef7ea
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83744402"
---
# <a name="manage-change-tracking-and-inventory"></a>Değişiklik İzleme ve Stok yönetimi

Azure Otomasyonu, ortamınızdaki makineler için [değişiklik izleme ve envanter](change-tracking.md) özelliğini sunar. Özelliği, kayıt defteri anahtarlarında, dosyalarında, içerikte ve gibi kullanılabilir değişiklikleri izler ve sunar. Bu makale, bu özellikle çalışma yordamlarını içerir.

## <a name="enable-the-full-change-tracking-and-inventory-feature"></a>Tam Değişiklik İzleme ve envanter özelliğini etkinleştirme

[Azure Güvenlik Merkezi dosya bütünlüğü izleme 'yi (FIM)](https://docs.microsoft.com/azure/security-center/security-center-file-integrity-monitoring)etkinleştirdiyseniz, makineleriniz için aşağıda açıklandığı gibi tam değişiklik izleme ve envanter özelliğini de kullanabilirsiniz. Ayarlarınız bu işlem tarafından kaldırılmaz.

> [!NOTE]
> Tam Değişiklik İzleme ve envanter özelliğinin etkinleştirilmesi ek ücretlere neden olabilir. Bkz. [Otomasyon Fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/).

1. Çalışma alanına giderek ve bunları [yüklü izleme çözümleri listesinde](../azure-monitor/insights/solutions.md#list-installed-monitoring-solutions)bularak izleme çözümünü kaldırın.
2. Çözüm adına tıklayarak Özet sayfasını açın ve [izleme çözümünü kaldırma](../azure-monitor/insights/solutions.md#remove-a-monitoring-solution)bölümünde açıklandığı gibi **Sil**' e tıklayın.
3. Değişiklik İzleme ve envanteri yeniden etkinleştirmek için Otomasyon hesabına gidin ve **yapılandırma yönetimi**altında **değişiklik izleme** veya **Stok** ' ı seçin.
4. Log Analytics çalışma alanı ve Otomasyon hesabı ' nı seçin, çalışma alanı ayarlarınızı doğrulayın ve **Etkinleştir**' e tıklayın.

## <a name="enable-machines-for-change-tracking-and-inventory"></a><a name="onboard"></a>Değişiklik İzleme ve envanter için makineleri etkinleştirme

Değişiklikleri izlemeye başlamak için Azure Otomasyonu 'nda Değişiklik İzleme ve envanteri etkinleştirmeniz gerekir. Bu özelliği makineleriniz için etkinleştirmenin önerilen ve desteklenen yolları aşağıda verilmiştir: 

* [Sanal makineden etkinleştir](automation-onboard-solutions-from-vm.md)
* [Birden çok makineye göz atmayı etkinleştir](automation-onboard-solutions-from-browse.md)
* [Otomasyon hesabınızda etkinleştirin](automation-onboard-solutions-from-automation-account.md)
* [Azure Otomasyonu runbook 'unda etkinleştirin](automation-onboard-solutions.md)

## <a name="track-files"></a>Dosyaları izle

### <a name="configure-file-tracking-on-windows"></a>Windows 'da dosya izlemeyi yapılandırma

Windows bilgisayarlarda dosya izlemeyi yapılandırmak için aşağıdaki adımları kullanın:

1. Otomasyon hesabınızda **yapılandırma yönetimi**altında **değişiklik izleme** ' yi seçin. 
2. **Ayarları Düzenle** (dişli simgesi) seçeneğine tıklayın.
3. Çalışma alanı yapılandırması sayfasında **Windows dosyaları**' nı seçin ve ardından **+ Ekle** ' ye tıklayarak izlenecek yeni bir dosya ekleyin.
4. Değişiklik İzleme için Windows dosyası Ekle bölmesinde, izlenecek dosyanın bilgilerini girin ve **Kaydet**' e tıklayın. Aşağıdaki tabloda, bilgiler için kullanabileceğiniz özellikler tanımlanmaktadır.

    |Özellik  |Açıklama  |
    |---------|---------|
    |Etkin     | Ayar uygulanmışsa true, aksi takdirde false.        |
    |Öğe Adı     | İzlenecek dosyanın kolay adı.        |
    |Grup     | Dosyaları mantıksal olarak gruplandırmak için bir grup adı.        |
    |Yolu girin     | Dosyanın denetlenecek yol (örneğin, **c:\Temp \\ \* . txt**). Ayrıca, gibi ortam değişkenlerini de kullanabilirsiniz `%winDir%\System32\\\*.*` .       |
    |Yol Türü     | Yolun türü. Olası değerler dosya ve dizindir.        |    
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
    |Bağlantılar     | Dizinler arasında geçiş yaparken sembolik bağlantılarla nasıl başa çıkılacağını belirleyen ayar. Olası değerler şunlardır:<br> Ignore-sembolik bağlantıları yoksayar ve başvurulan dosya/dizinleri içermez.<br>Takip et-özyineleme sırasında sembolik bağlantıları Izler ve başvurulan dosya/dizinleri de içerir.<br>Yönet-sembolik bağlantıları Izler ve döndürülen içeriğin değiştirilmesine izin verir. **Note** -Bu seçenek, dosya içeriği alımı desteklenmediği için önerilmez.    |
    |Dosya içeriğini karşıya yükle | İzlenen değişikliklerle dosya içeriğini karşıya yüklemek için true, aksi durumda false. |

5. **Dosya Içeriğini karşıya yüklemek**Için true değerini belirttiğinizden emin olun. Bu ayar, belirtilen dosya yolu için dosya içeriği izlemeyi etkinleştirilir.

   ![Linux dosyası Ekle](./media/change-tracking-file-contents/add-linux-file.png)

## <a name="track-file-contents"></a>Dosya içeriğini izleme

Dosya içeriği izleme, izlenen bir değişiklikten önceki ve sonraki bir dosyanın içeriğini görüntülemenize izin verir. Özellik, her değişiklik oluştuktan sonra dosya içeriğini bir depolama hesabına kaydeder. Dosya içeriğini izlemek için izlenecek bazı kurallar aşağıda verilmiştir:

* Dosya içeriğini depolamak için Kaynak Yöneticisi dağıtım modelini kullanan standart bir depolama hesabı gerekir. 

* Premium ve klasik dağıtım modeli depolama hesapları kullanmayın. Bkz. [Azure depolama hesapları hakkında](../storage/common/storage-create-storage-account.md).

* Kullandığınız depolama hesabı yalnızca bir Otomasyon hesabına bağlanabilir.

* Otomasyon hesabınızda [değişiklik izleme ve envanter](change-tracking.md) etkinleştirilmiştir.

### <a name="enable-tracking-for-file-content-changes"></a>Dosya içeriği değişiklikleri için izlemeyi etkinleştir

1. Azure portal, Otomasyon hesabınızı açın ve **yapılandırma yönetimi**altında **değişiklik izleme** ' yi seçin.
2. **Ayarları Düzenle** (dişli simgesi) seçeneğine tıklayın.
3. **Dosya içeriği** ' ni seçin ve **bağlantı**' ya tıklayın. Bu seçim, Değişiklik İzleme için Içerik konumu Ekle bölmesini açar.

   ![İçerik konumunu etkinleştir](./media/change-tracking-file-contents/enable.png)

4. Dosya içeriğini depolamak için kullanılacak aboneliği ve depolama hesabını seçin. 

5. Tüm mevcut izlenen dosyalar için dosya içeriği izlemeyi etkinleştirmek istiyorsanız, **Tüm ayarlar için dosya Içeriğini karşıya yükle** **' yi seçin.** Bu ayarı, her dosya yolu için daha sonra değiştirebilirsiniz.

   ![Depolama hesabı ayarla](./media/change-tracking-file-contents/storage-account.png)

6. Değişiklik İzleme ve Inventory, dosya içeriği değişiklik izlemeyi etkinleştirdiğinden depolama hesabı ve paylaşılan erişim Imzası (SAS) URI 'Lerini gösterir. İmzaların kullanım süreleri 365 gün sonra doluyor ve yeniden **Oluştur**' a tıklayarak yeniden oluşturabilirsiniz.

   ![Hesap anahtarlarını listeleme](./media/change-tracking-file-contents/account-keys.png)

### <a name="view-the-contents-of-a-tracked-file"></a>İzlenen bir dosyanın içeriğini görüntüleme

Değişiklik İzleme ve envanter izlenen bir dosya için değişiklik algıladığında, değişiklik ayrıntıları bölmesinde dosya içeriğini görüntüleyebilirsiniz.  

![Liste değişiklikleri](./media/change-tracking-file-contents/change-list.png)

1. Azure portal, Otomasyon hesabınızı açın ve **yapılandırma yönetimi**altında **değişiklik izleme** ' yi seçin.

2. Değişiklik listesinden bir dosya seçin ve dosyanın içeriğini görmek için **Dosya Içeriğini görüntüle değişikliklerini** seçin. Değişiklik ayrıntıları bölmesi, dosya bilgilerinin önceki ve sonraki bir standart olduğunu gösterir.

   ![Değişiklik ayrıntıları](./media/change-tracking-file-contents/change-details.png)

3. Dosya içeriğini yan yana görünümde görüntülüyorsunuz. Değişikliklerin satır içi görünümünü görmek için **satır içi** seçeneğini belirleyebilirsiniz.

## <a name="track-registry-keys"></a>Kayıt defteri anahtarlarını izleme

Windows bilgisayarlarda kayıt defteri anahtarı izlemeyi yapılandırmak için aşağıdaki adımları kullanın:

1. Otomasyon hesabınızda **yapılandırma yönetimi**altında **değişiklik izleme** ' yi seçin. 
2. **Ayarları Düzenle** (dişli simgesi) seçeneğine tıklayın.
3. Çalışma alanı yapılandırması sayfasında **Windows kayıt defteri**' ni seçin.
4. İzlemek üzere yeni bir kayıt defteri anahtarı eklemek için **+ Ekle** ' ye tıklayın.
5. Değişiklik İzleme için Windows kayıt defteri Ekle bölmesinde, izlenecek anahtarın bilgilerini girin ve **Kaydet**' e tıklayın. Aşağıdaki tabloda, bilgiler için kullanabileceğiniz özellikler tanımlanmaktadır.

    |Özellik  |Açıklama  |
    |---------|---------|
    |Etkin     | Bir ayar uygulanmışsa true, aksi takdirde false.        |
    |Öğe Adı     | İzlenecek kayıt defteri anahtarının kolay adı.        |
    |Grup     | Kayıt defteri anahtarlarını mantıksal olarak gruplandırmak için Grup adı.        |
    |Windows Kayıt Defteri Anahtarı   | Yol ile anahtar adı, örneğin, **HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common Startup**.      |

## <a name="search-logs-for-change-records"></a>Değişiklik kayıtları için arama günlükleri

Değişiklik kayıtları için Azure Izleyici günlüklerinde çeşitli aramalar yapabilirsiniz. Değişiklik izleme sayfası açıkken, Günlükler sayfasını açmak için **Log Analytics** ' a tıklayın. Aşağıdaki tabloda değişiklik kayıtları için örnek günlük aramaları sunulmaktadır.

|Sorgu  |Açıklama  |
|---------|---------|
|ConfigurationData<br>&#124; config DataType = = "Microsoft Services" ve SvcStartupType = = "Auto"<br>&#124; SvcState = = "durduruldu"<br>SoftwareName, bilgisayar tarafından arg_max (TimeGenerated, *) özetleme &#124;         | Otomatik olarak ayarlanan ancak durdurulma olarak bildirilen Microsoft Hizmetleri için en son envanter kayıtlarını gösterir. Sonuçlar, belirtilen yazılım adı ve bilgisayar için en son kayıtla sınırlıdır.    |
|ConfigurationChange<br>&#124; ConfigChangeType = = "Software" ve ChangeCategory = = "removed"<br>&#124; zaman tarafından üretilen DESC 'e göre sırala|Kaldırılan yazılım için değişiklik kayıtlarını gösterir.|

## <a name="create-alerts-on-changes"></a>Değişiklikler üzerinde uyarı oluştur

Aşağıdaki örnek, bir makinede **C:\Windows\system32\drivers\etc\hosts** dosyasının değiştirildiğini gösterir. Windows bunu, ana bilgisayar adlarını IP adreslerine çözümlemek üzere kullandığından, bu dosya önemlidir. Bu işlem DNS 'den önceliklidir ve bağlantı sorunlarına yol açabilir. Ayrıca, trafiğin kötü amaçlı ya da başka türlü tehlikeli Web sitelerine yönlendirilmesine de yol açabilir.

![Hosts dosyası değişikliğini gösteren bir grafik](./media/change-tracking-file-contents/changes.png)

Bu örneği bir değişiklik üzerinde uyarı oluşturma adımlarını tartışmak için kullanalım.

1. Otomasyon hesabınızda **yapılandırma yönetimi**altında **izlemeyi Değiştir** ' i seçin ve ardından **Log Analytics**' yi seçin. 
2. Günlükler aramasında, **Hosts** dosyasında bulunan içerik değişikliklerini sorgu ile arayın `ConfigurationChange | where FieldsChanged contains "FileContentChecksum" and FileSystemPath contains "hosts"` . Bu sorgu, "konaklar" sözcüğünü içeren tam bir yola sahip dosyalar için içerik değişikliğini arar. Ayrıca, örneğin kullanarak yol bölümünü tam nitelikli biçimde değiştirerek belirli bir dosyayı sorabilirsiniz `FileSystemPath == "c:\windows\system32\drivers\etc\hosts"` .

3. Sorgu istenen sonuçları döndürdüğünden, uyarı oluşturma sayfasını açmak için günlük aramasında **Yeni uyarı kuralı** ' na tıklayın. Ayrıca, Azure portal **Azure izleyici** aracılığıyla bu sayfaya gidebilirsiniz. 

4. Sorgunuzu yeniden denetleyin ve uyarı mantığını değiştirin. Bu durumda, ortamdaki tüm makinelerde tek bir değişiklik algılanırsa uyarının tetiklenmesi istersiniz.

    ![Ana bilgisayar dosyasındaki değişiklikleri izlemek için sorguya Değiştir](./media/change-tracking-file-contents/change-query.png)

5. Uyarı mantığı ayarlandıktan sonra, tetiklenen uyarıya yanıt olarak eylemler gerçekleştirmek için eylem grupları atayın. Bu durumda, gönderilecek e-postaları ve oluşturulacak bir BT hizmet yönetimi (ıTSM) anahtarını ayarlıyoruz. 

    ![Eylem grubunu değişiklik üzerinde uyarı verecek şekilde yapılandırma](./media/change-tracking/action-groups.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Değişiklik İzleme ve envantere genel bakış](change-tracking.md)
* [Değişiklik İzleme ve envanter sorunlarını giderme](troubleshoot/change-tracking.md)
* [Azure Izleyici günlüklerinde günlük aramaları](../log-analytics/log-analytics-log-searches.md)