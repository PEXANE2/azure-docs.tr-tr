---
title: Azure Güvenlik Merkezi 'nde dosya bütünlüğü izleme | Microsoft Docs
description: Bu yönergeyi kullanarak Azure Güvenlik Merkezi 'nde dosya bütünlüğü izleme (FIM) yapılandırma hakkında bilgi edinin.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2020
ms.author: memildin
ms.openlocfilehash: b64ff51836f8d291acf57b1cd9ca100c4f87ebed
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91541178"
---
# <a name="file-integrity-monitoring-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde dosya bütünlüğü izleme
Bu yönergeyi kullanarak Azure Güvenlik Merkezi 'nde dosya bütünlüğü izleme (FIM) yapılandırma hakkında bilgi edinin.


## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Genel olarak kullanılabilir (GA)|
|Fiyat|[Sunucular Için Azure Defender](defender-for-servers-introduction.md) gerekir|
|Gerekli roller ve izinler:|**Çalışma alanı sahıbı** FIM 'yi etkinleştirebilir/devre dışı bırakabilir (daha fazla bilgi için bkz. [Log Analytics için Azure rolleri](https://docs.microsoft.com/services-hub/health/azure-roles#azure-roles)).<br>**Okuyucu** , sonuçları görüntüleyebilir.|
|Larının|![Evet ](./media/icons/yes-icon.png) ticari bulutlar<br>![Evet ](./media/icons/yes-icon.png) US gov<br>![](./media/icons/no-icon.png)Çin gov, diğer gov yok<br>Yalnızca Azure Otomasyonu 'nun değişiklik izleme çözümünün kullanılabildiği bölgelerde desteklenir.<br>Bkz. [bağlantılı Log Analytics çalışma alanı Için desteklenen bölgeler](../automation/how-to/region-mappings.md).<br>[Değişiklik izleme hakkında daha fazla bilgi edinin](../automation/change-tracking.md) |
|||





## <a name="what-is-fim-in-security-center"></a>Güvenlik Merkezi 'nde FIM nedir?
Değişiklik izleme olarak da bilinen dosya bütünlüğü izleme (FIM), işletim sisteminin, uygulama yazılımlarının ve diğer kullanıcıların bir saldırıyı gösterebilen değişiklikler için dosyaları ve kayıt defterlerini inceler. Bir karşılaştırma yöntemi, dosyanın geçerli durumunun dosyanın son taramasından farklı olup olmadığını belirlemede kullanılır. Dosyalarınızda geçerli veya şüpheli değişiklikler yapıldığını öğrenmek için bu karşılaştırmayı kullanabilirsiniz.

Güvenlik Merkezi 'nin dosya bütünlüğü izleme, Windows dosyaları, Windows kayıt defteri ve Linux dosyalarının bütünlüğünü doğrular. FIM 'yi etkinleştirerek izlenmesini istediğiniz dosyaları seçersiniz. Güvenlik Merkezi, şu gibi etkinlik için FIM 'yi etkin olan dosyaları izler:

- Dosya ve kayıt defteri oluşturma ve kaldırma
- Dosya değişiklikleri (dosya boyutundaki değişiklikler, erişim denetim listeleri ve içeriğin karması)
- Kayıt defteri değişiklikleri (boyut, erişim denetim listeleri, tür ve içeriğe yapılan değişiklikler)

Güvenlik Merkezi, içinde FIM 'yi kolayca etkinleştirebilmeniz için varlıkların izlenmesini öneriyor. Ayrıca, izlemek üzere kendi FIM ilkelerinizi veya varlıklarınızı tanımlayabilirsiniz. Bu izlenecek yol size nasıl yapılacağını gösterir.

> [!NOTE]
> Dosya bütünlüğü izleme (FIM) özelliği, Windows ve Linux bilgisayarları ve VM 'Leri için geçerlidir ve yalnızca, **sunucular Için Azure Defender** etkinleştirildiğinde kullanılabilir. Daha fazla bilgi için bkz. [fiyatlandırma](security-center-pricing.md) . FIM Log Analytics çalışma alanına veri yükler. Veri ücretleri, karşıya yüklediğiniz veri miktarına bağlı olarak uygulanır. Daha fazla bilgi için bkz. [Log Analytics fiyatlandırması](https://azure.microsoft.com/pricing/details/log-analytics/) .

FIM, ortamınızdaki değişiklikleri izlemek ve tanımlamak için Azure Değişiklik İzleme çözümünü kullanır. Dosya bütünlüğü izleme etkinleştirildiğinde, **çözüm**türünde bir **değişiklik izleme** kaynağınız olur. Veri toplama sıklığı ayrıntılarını, Azure Değişiklik İzleme için [Değişiklik izleme verileri toplama ayrıntıları](https://docs.microsoft.com/azure/automation/automation-change-tracking#change-tracking-data-collection-details) konusunda bulabilirsiniz.

> [!NOTE]
> **Değişiklik izleme** kaynağını kaldırırsanız, güvenlik merkezi 'nde de dosya bütünlüğü izleme özelliğini devre dışı bırakacaksınız.

## <a name="which-files-should-i-monitor"></a>Hangi dosyaları izmalıyım?
Hangi dosyaların izleneceğini seçerken sistem ve uygulamalarınız için kritik olan dosyaları düşünmeniz gerekir. Planlanmadan değiştirilmesini beklemediğiniz dosyaları seçmeyi düşünün. Uygulamalar veya işletim sistemi tarafından sıkça değiştirilen dosyaları seçme (örneğin, günlük dosyaları ve metin dosyaları), bir saldırının tanımlanmasını zorlaştıran çok sayıda gürültü oluşturur.

Güvenlik Merkezi, bilinen saldırı desenlerine göre izlemek için önerilen öğelerin aşağıdaki listesini sağlar. Bu dosyalar ve Windows kayıt defteri anahtarları bulunur. Tüm anahtarlar tablodaki HKEY_LOCAL_MACHINE ("HKLM") altındadır.)

|**Linux dosyaları**|**Windows dosyaları**|**Windows kayıt defteri anahtarları**|
|:----|:----|:----|
|/bin/login|C:\autoexec.bat|HKLM\SOFTWARE\Microsoft\Cryptography\OID\EncodingType 0 \ Cryptsıpdllremovesigneddatamsg \{ C689AAB8-8E78-11D0-8C47-00C04FC295EE}|
|/bin/passwd|C:\boot.ini|HKLM\SOFTWARE\Microsoft\Cryptography\OID\EncodingType 0 \ Cryptsıpdllremovesigneddatamsg \{ 603Bcc1f-4b59-4e08-b724-d2c6297ef351}|
|/etc/*. conf|C:\config.sys|HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\IniFileMapping\SYSTEM.ini \Boot|
|/usr/bin|C:\Windows\system.ini|HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows|
|/usr/sbin|C:\Windows\win.ini|HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon|
|/bin|C:\Windows\regedit.exe|HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Shell klasörleri|
|/sbin|C:\Windows\System32\userinit.exe|HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User kabuk klasörleri|
|/Boot|C:\Windows\explorer.exe|HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run|
|/usr/local/bin|C:\Program Files\Microsoft Security Client\msseces.exe|HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce|
|/usr/local/sbin||HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnceEx|
|/seçenek/bin||HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunServices|
|/opt/sbin||HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunServicesOnce|
|/etc/crontab||HKLM\SOFTWARE\WOW6432Node\Microsoft\Cryptography\OID\EncodingType 0 \ Cryptsıpdllremovesigneddatamsg \{ C689AAB8-8E78-11D0-8C47-00C04FC295EE}|
|/etc/init.d||HKLM\SOFTWARE\WOW6432Node\Microsoft\Cryptography\OID\EncodingType 0 \ Cryptsıpdllremovesigneddatamsg \{ 603Bcc1f-4b59-4e08-b724-d2c6297ef351}|
|/etc/cron.hourly||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\IniFileMapping\system.ini \Boot|
|/etc/cron.daily||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\Windows|
|/etc/cron.weekly||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows NT\CurrentVersion\Winlogon|
|/etc/cron.monthly||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Explorer\Shell klasörleri|
|||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Explorer\User Shell klasörleri|
|||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Run|
|||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\RunOnce|
|||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\RunOnceEx|
|||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\RunServices|
|||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\RunServicesOnce|
|||HKLM\SYSTEM\CurrentControlSet\Control\hivelist|
|||HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\KnownDLLs|
|||Hklm\system\currentcontrolset\services\sharedaccess\parameters\firewallilkedomainprofile|
|||Hklm\system\currentcontrolset\services\sharedaccess\parameters\firewallilkepublicprofile|
|||Hklm\system\currentcontrolset\services\sharedaccess\parameters\firewallilkestandardprofile|


## <a name="enable-file-integrity-monitoring"></a>Dosya bütünlüğünü izlemeyi etkinleştir 

1. **Azure Defender** panosunun **Gelişmiş koruma** alanından **Dosya bütünlüğü izleme**' yi seçin.

   :::image type="content" source="./media/security-center-file-integrity-monitoring/open-file-integrity-monitoring.png" alt-text="FIM başlatılıyor" lightbox="./media/security-center-file-integrity-monitoring/open-file-integrity-monitoring.png":::

    **Dosya bütünlüğü izleme** yapılandırması sayfası açılır.

    Her çalışma alanı için aşağıdaki bilgiler sağlanır:

    - Geçen hafta içinde gerçekleşen toplam değişiklik sayısı (FIM çalışma alanında etkinleştirilmemişse "-" bir tire görebilirsiniz)
    - Çalışma alanına rapor veren toplam bilgisayar ve VM sayısı
    - Çalışma alanının coğrafi konumu
    - Çalışma alanının altında bulunduğu Azure aboneliği

1. Bu sayfayı şu şekilde kullanın:

    - Her bir çalışma alanının durumunu ve ayarlarını erişin ve görüntüleyin

    - ![Yükseltme planı simgesi ][4] çalışma alanını Azure Defender 'ı kullanacak şekilde yükseltin. Bu simge, çalışma alanının veya aboneliğin Azure Defender tarafından korunmadığını gösterir. FIM özelliklerini kullanmak için aboneliğinizin Azure Defender tarafından korunması gerekir. [Daha fazla bilgi edinin](security-center-pricing.md).

    - ![Simgeyi etkinleştir][3] Çalışma alanındaki tüm makinelerde FIM 'yi etkinleştirin ve FIM seçeneklerini yapılandırın. Bu simge, FIM 'in çalışma alanı için etkinleştirilmediğini belirtir.

        :::image type="content" source="./media/security-center-file-integrity-monitoring/workspace-list-fim.png" alt-text="FIM başlatılıyor":::


    > [!TIP]
    > Etkinleştir veya yükselt düğmesi yoksa ve alan boşsa, FIM 'nin çalışma alanında zaten etkin olduğu anlamına gelir.


1. **Etkinleştir**' i seçin. Çalışma alanının altındaki Windows ve Linux makinelerin sayısı dahil, çalışma alanının ayrıntıları gösterilir.

    :::image type="content" source="./media/security-center-file-integrity-monitoring/workspace-fim-status.png" alt-text="FIM başlatılıyor":::

   Windows ve Linux için önerilen ayarlar da listelenir.  Önerilen öğelerin tam listesini görmek için **Windows dosyaları**, **kayıt defteri**ve **Linux dosyalarını** genişletin.

1. FIM tarafından izlenmesini istemediğiniz önerilen varlıkların onay kutularını temizleyin.

1. FIM 'yi etkinleştirmek için **Dosya bütünlüğü Izlemeyi Uygula** ' yı seçin.

> [!NOTE]
> Ayarları dilediğiniz zaman değiştirebilirsiniz. Daha fazla bilgi edinmek için aşağıda [izlenen varlıkları düzenleme](#edit-monitored-entities) bölümüne bakın.



## <a name="audit-monitored-workspaces"></a>İzlenen çalışma alanlarını denetleme 

**Dosya bütünlüğü izleme** panosu FIM 'nin etkinleştirildiği çalışma alanlarını görüntüler. FIM panosu, bir çalışma alanında FIM 'yi etkinleştirdikten sonra veya zaten FIM 'nin etkinleştirildiği **Dosya bütünlüğü izleme** penceresinde bir çalışma alanı seçtiğinizde açılır.

:::image type="content" source="./media/security-center-file-integrity-monitoring/fim-dashboard.png" alt-text="FIM başlatılıyor":::

Bir çalışma alanı için FIM Panosu aşağıdaki ayrıntıları görüntüler:

- Çalışma alanına bağlı toplam makine sayısı
- Seçilen süre boyunca gerçekleşen toplam değişiklik sayısı
- Değişiklik türü dökümü (dosyalar, kayıt defteri)
- Değişiklik kategorisinin bir dökümü (değiştirildi, eklendi, kaldırıldı)

Değişikliklerin gösterildiği zaman aralığını değiştirmek için panonun en üstünde bulunan **filtre** ' yi seçin.

:::image type="content" source="./media/security-center-file-integrity-monitoring/dashboard-filter.png" alt-text="FIM başlatılıyor":::

**Sunucular** sekmesi, bu çalışma alanına rapor veren makineleri listeler. Her makine için Pano şunları listeler:

- Seçilen süre boyunca gerçekleşen toplam değişiklik sayısı
- Dosya değişiklikleri veya kayıt defteri değişiklikleri olarak toplam değişiklik dökümü

Arama alanına bir makine adı girdiğinizde veya bilgisayarlar sekmesi altında listelenen bir makineyi seçtiğinizde **günlük araması** açılır. günlük araması, makinenin seçili zaman döneminde yapılan tüm değişiklikleri görüntüler. Daha fazla bilgi için bir değişikliği genişletebilirsiniz.

![Günlük araması][8]

**Değişiklikler** sekmesi (aşağıda gösterilmiştir), seçilen dönemde çalışma alanı için tüm değişiklikleri listeler. Değiştirilen her varlık için Pano şunları listeler:

- Değişikliğin gerçekleştiği bilgisayar
- Değişiklik türü (kayıt defteri veya dosya)
- Değişiklik kategorisi (değiştirildi, eklendi, kaldırıldı)
- Değişikliğin tarih ve saati

![Çalışma alanı değişiklikleri][9]

Değişiklik **ayrıntıları** , arama alanına bir değişiklik girdiğinizde veya **değişiklikler** sekmesi altında listelenen bir varlık seçtiğinizde açılır.

![Değişiklik ayrıntıları][10]

## <a name="edit-monitored-entities"></a>İzlenen varlıkları Düzenle

1. **Dosya bütünlüğü izleme panosuna** dönün ve **Ayarlar**' ı seçin.

   ![Ayarlar][11]

   **Çalışma alanı yapılandırması** üç sekme görüntülemeyi açar: **Windows kayıt defteri**, **Windows dosyaları**ve **Linux dosyaları**. Her sekme, bu kategoride düzenleyebileceğiniz varlıkları listeler. Listelenen her varlık için, güvenlik merkezi FIM 'nin etkinleştirilip etkinleştirilmediğini (true) veya etkinleştirilmediğini (false) belirler.  Varlığı düzenlediğinizde FIM 'yi etkinleştirmenizi veya devre dışı bırakmanızı sağlayabilirsiniz.

   ![Çalışma alanı yapılandırması][12]

2. Kimlik koruması seçin. Bu örnekte, Windows kayıt defteri altında bir öğe seçtik. **Değişiklik izleme Için Düzenle** açılır.

   ![Düzenleme veya değişiklik izleme][13]

**Değişiklik izleme Için Düzenle** altında şunları yapabilirsiniz:

- (True) veya Disable (false) dosya bütünlüğü izlemeyi etkinleştir
- Varlık adını sağlayın veya değiştirin
- Değer veya yolu belirtin veya değiştirin
- Varlığı silme, değişikliği atma veya değişikliği kaydetme

## <a name="add-a-new-entity-to-monitor"></a>İzlenecek yeni bir varlık ekleyin
1. **Dosya bütünlüğü izleme panosuna** dönün ve en üstteki **Ayarlar** ' ı seçin. **Çalışma alanı yapılandırması** açılır.
2. **Çalışma alanı yapılandırması**altında, eklemek istediğiniz varlık türünün sekmesini seçin: Windows kayıt defteri, Windows dosyaları veya Linux dosyaları. Bu örnekte, **Linux dosyalarını**seçtik.

   ![İzlenecek yeni bir öğe ekleyin][14]

3. **Add (Ekle)** seçeneğini belirleyin. **Değişiklik izleme Için Ekle** açılır.

   ![İstenen bilgileri girin][15]

4. **Ekle** sayfasında, istenen bilgileri yazın ve **Kaydet**' i seçin.

## <a name="disable-monitored-entities"></a>İzlenen varlıkları devre dışı bırak
1. **Dosya bütünlüğü izleme** panosuna dönün.
2. FIM 'in Şu anda etkin olduğu bir çalışma alanı seçin. Etkinleştir düğmesi veya planı yükselt düğmesi eksikse, bir çalışma alanı FIM için etkinleştirilir.

   ![FIM 'nin etkinleştirildiği bir çalışma alanı seçin][16]

3. Dosya bütünlüğü izleme altında **Ayarlar**' ı seçin.

   ![Ayarlar'ı seçin][17]

4. **Çalışma alanı yapılandırması**altında, **etkin** ' in true olarak ayarlandığı bir grup seçin.

   ![Çalışma alanı yapılandırması][18]

5. **Değişiklik izleme Için Düzenle** ' nin altında **Enabled** ayarı false olarak ayarlandı.

   ![Etkin ayarını false olarak ayarla][19]

6. **Kaydet**'i seçin.

## <a name="folder-and-path-monitoring-using-wildcards"></a>Joker karakterler kullanılarak klasör ve yol izleme

Dizinler genelinde izlemeyi basitleştirmek için joker karakterler kullanın. Joker karakterler kullanarak klasör izlemeyi yapılandırdığınızda aşağıdaki kurallar geçerlidir:
-   Birden çok dosyayı izlemek için joker karakterler gereklidir.
-   Joker karakterler yalnızca bir yolun son kesiminde kullanılabilir (örneğin, C:\folder\file veya/etc/*. conf)
-   Bir ortam değişkeni geçerli olmayan bir yol içeriyorsa, doğrulama başarılı olur ancak sayım çalıştırıldığında yol başarısız olur.
-   Yolu ayarlarken, c:. * gibi genel yollardan kaçının, \* Bu da çok fazla klasör çapraz olarak sonuçlanır.

## <a name="disable-fim"></a>FIM 'yi devre dışı bırak
FIM 'yi devre dışı bırakabilirsiniz. FIM, ortamınızdaki değişiklikleri izlemek ve tanımlamak için Azure Değişiklik İzleme çözümünü kullanır. FIM 'yi devre dışı bırakarak, Değişiklik İzleme çözümünü seçili çalışma alanından kaldırırsınız.

1. FIM 'yi devre dışı bırakmak için **Dosya bütünlüğü izleme** panosuna dönün.
2. Bir çalışma alanı seçin.
3. **Dosya bütünlüğü izleme**altında **devre dışı bırak**' ı seçin.

   ![FIM 'yi devre dışı bırak][20]

4. Devre dışı bırakmak için **Kaldır** ' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, güvenlik merkezi 'nde dosya bütünlüğü izleme (FIM) kullanmayı öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki sayfalara bakın:

* [Güvenlik Ilkelerini ayarlama](tutorial-security-policy.md) --Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Güvenlik önerilerini yönetme](security-center-recommendations.md) --önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Azure Güvenlik blogu](https://docs.microsoft.com/archive/blogs/azuresecurity/) - En son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-file-integrity-monitoring/security-center-dashboard.png
[3]: ./media/security-center-file-integrity-monitoring/enable.png
[4]: ./media/security-center-file-integrity-monitoring/upgrade-plan.png
[5]: ./media/security-center-file-integrity-monitoring/enable-fim.png
[7]: ./media/security-center-file-integrity-monitoring/filter.png
[8]: ./media/security-center-file-integrity-monitoring/log-search.png
[9]: ./media/security-center-file-integrity-monitoring/changes-tab.png
[10]: ./media/security-center-file-integrity-monitoring/change-details.png
[11]: ./media/security-center-file-integrity-monitoring/fim-dashboard-settings.png
[12]: ./media/security-center-file-integrity-monitoring/workspace-config.png
[13]: ./media/security-center-file-integrity-monitoring/edit.png
[14]: ./media/security-center-file-integrity-monitoring/add.png
[15]: ./media/security-center-file-integrity-monitoring/add-item.png
[16]: ./media/security-center-file-integrity-monitoring/fim-dashboard-disable.png
[17]: ./media/security-center-file-integrity-monitoring/fim-dashboard-settings-disabled.png
[18]: ./media/security-center-file-integrity-monitoring/workspace-config-disable.png
[19]: ./media/security-center-file-integrity-monitoring/edit-disable.png
[20]: ./media/security-center-file-integrity-monitoring/disable-fim.png