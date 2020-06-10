---
title: Azure Güvenlik Merkezi 'nde dosya bütünlüğü Izleme | Microsoft Docs
description: Bu yönergeyi kullanarak Azure Güvenlik Merkezi 'nde dosya bütünlüğü Izleme (FIM) yapılandırma hakkında bilgi edinin.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/13/2019
ms.author: memildin
ms.openlocfilehash: c58f70126c72a84b09f6eadc251949a0f0021657
ms.sourcegitcommit: d7fba095266e2fb5ad8776bffe97921a57832e23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84628285"
---
# <a name="file-integrity-monitoring-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde Dosya Bütünlüğü İzleme
Bu yönergeyi kullanarak Azure Güvenlik Merkezi 'nde dosya bütünlüğü Izleme (FIM) yapılandırma hakkında bilgi edinin.


## <a name="availability"></a>Kullanılabilirlik

- Yayın durumu: **genel olarak kullanılabilir**
- Gerekli roller: **çalışma alanı sahıbı** FIM 'yi etkinleştirebilir/devre dışı bırakabilir (daha fazla bilgi için bkz. [Log Analytics için Azure rolleri](https://docs.microsoft.com/services-hub/health/azure-roles#azure-roles)). **Okuyucu** , sonuçları görüntüleyebilir.
- Larının
    - Ticari bulutlar ✔
    - ✔ US Gov bulutu
    - ✘ Çin gov/diğer gov


## <a name="what-is-fim-in-security-center"></a>Güvenlik Merkezi 'nde FIM nedir?
Değişiklik izleme olarak da bilinen dosya bütünlüğü Izleme (FIM), işletim sisteminin, uygulama yazılımlarının ve diğer kullanıcıların bir saldırıyı gösterebilen değişiklikler için dosyaları ve kayıt defterlerini inceler. Bir karşılaştırma yöntemi, dosyanın geçerli durumunun dosyanın son taramasından farklı olup olmadığını belirlemede kullanılır. Dosyalarınızda geçerli veya şüpheli değişiklikler yapıldığını öğrenmek için bu karşılaştırmayı kullanabilirsiniz.

Güvenlik Merkezi 'nin dosya bütünlüğü Izleme, Windows dosyaları, Windows kayıt defteri ve Linux dosyalarının bütünlüğünü doğrular. FIM 'yi etkinleştirerek izlenmesini istediğiniz dosyaları seçersiniz. Güvenlik Merkezi, şu gibi etkinlik için FIM 'yi etkin olan dosyaları izler:

- Dosya ve kayıt defteri oluşturma ve kaldırma
- Dosya değişiklikleri (dosya boyutundaki değişiklikler, erişim denetim listeleri ve içeriğin karması)
- Kayıt defteri değişiklikleri (boyut, erişim denetim listeleri, tür ve içeriğe yapılan değişiklikler)

Güvenlik Merkezi, içinde FIM 'yi kolayca etkinleştirebilmeniz için varlıkların izlenmesini öneriyor. Ayrıca, izlemek üzere kendi FIM ilkelerinizi veya varlıklarınızı tanımlayabilirsiniz. Bu izlenecek yol size nasıl yapılacağını gösterir.

> [!NOTE]
> Dosya bütünlüğü Izleme (FIM) özelliği Windows ve Linux bilgisayarları ve VM 'Leri için çalışarak, güvenlik merkezi 'nin standart katmanında kullanılabilir. Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md). FIM Log Analytics çalışma alanına veri yükler. Veri ücretleri, karşıya yüklediğiniz veri miktarına bağlı olarak uygulanır. Daha fazla bilgi için bkz. [Log Analytics fiyatlandırması](https://azure.microsoft.com/pricing/details/log-analytics/) .

FIM, ortamınızdaki değişiklikleri izlemek ve tanımlamak için Azure Değişiklik İzleme çözümünü kullanır. Dosya bütünlüğü Izleme etkinleştirildiğinde, **çözüm**türünde bir **değişiklik izleme** kaynağınız olur. Veri toplama sıklığı ayrıntılarını, Azure Değişiklik İzleme için [Değişiklik izleme verileri toplama ayrıntıları](https://docs.microsoft.com/azure/automation/automation-change-tracking#change-tracking-data-collection-details) konusunda bulabilirsiniz.

> [!NOTE]
> **Değişiklik izleme** kaynağını kaldırırsanız, güvenlik merkezi 'Nde de dosya bütünlüğü izleme özelliğini devre dışı bırakacaksınız.

## <a name="which-files-should-i-monitor"></a>Hangi dosyaları izmalıyım?
Hangi dosyaların izleneceğini seçerken sistem ve uygulamalarınız için kritik olan dosyaları düşünmeniz gerekir. Planlanmadan değiştirilmesini beklemediğiniz dosyaları seçmeyi düşünün. Uygulamalar veya işletim sistemi tarafından sıkça değiştirilen dosyaları seçme (örneğin, günlük dosyaları ve metin dosyaları), bir saldırının tanımlanmasını zorlaştıran çok sayıda gürültü oluşturur.

Güvenlik Merkezi, bilinen saldırı desenlerine göre izlemek için önerilen öğelerin aşağıdaki listesini sağlar. Bu dosyalar ve Windows kayıt defteri anahtarları bulunur. Tüm anahtarlar tablodaki HKEY_LOCAL_MACHINE ("HKLM") altındadır.)

|**Linux dosyaları**|**Windows dosyaları**|**Windows kayıt defteri anahtarları**|
|:----|:----|:----|
|/bin/login|C:\oto Exec.bat|HKLM\SOFTWARE\Microsoft\Cryptography\OID\EncodingType 0 \ Cryptsıpdllremovesigneddatamsg \{ C689AAB8-8E78-11D0-8C47-00C04FC295EE}|
|/bin/passwd|C:\Boot.ini|HKLM\SOFTWARE\Microsoft\Cryptography\OID\EncodingType 0 \ Cryptsıpdllremovesigneddatamsg \{ 603Bcc1f-4b59-4e08-b724-d2c6297ef351}|
|/etc/*. conf|C:\Config.sys|HKLM\SOFTWARE\Microsoft\Windows Nt\currentversion\inidosyasımapping\system.ın\boot|
|/usr/bin|C:\Windows\system.ini|HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows|
|/usr/sbin|C:\Windows\win.ini|HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon|
|/bin|C:\Windows\regedit.exe|HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Shell klasörleri|
|/sbin|C:\windows\system32\userınit.exe|HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User kabuk klasörleri|
|/Boot|C:\Windows\explorer.exe|HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run|
|/usr/local/bin|C:\Program Files\Microsoft Security Client\msseces.exe|HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce|
|/usr/local/sbin||HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnceEx|
|/seçenek/bin||HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunServices|
|/opt/sbin||HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunServicesOnce|
|/etc/crontab||HKLM\SOFTWARE\WOW6432Node\Microsoft\Cryptography\OID\EncodingType 0 \ Cryptsıpdllremovesigneddatamsg \{ C689AAB8-8E78-11D0-8C47-00C04FC295EE}|
|/etc/init.d||HKLM\SOFTWARE\WOW6432Node\Microsoft\Cryptography\OID\EncodingType 0 \ Cryptsıpdllremovesigneddatamsg \{ 603Bcc1f-4b59-4e08-b724-d2c6297ef351}|
|/etc/cron.hourly||HKLM\SOFTWARE\WOW6432Node\Microsoft\Windows Nt\currentversion\ınıfilemapping\system.ini\boot|
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

## <a name="using-file-integrity-monitoring"></a>Dosya bütünlüğü Izlemeyi kullanma
1. **Güvenlik Merkezi** panosunu açın.
2. **Gelişmiş bulut savunması**altındaki sol bölmede **Dosya bütünlüğü izleme**' yi seçin.
![Güvenlik Merkezi panosu][1]

**Dosya bütünlüğü izleme** açılır.
  ![Güvenlik Merkezi panosu][2]

Her çalışma alanı için aşağıdaki bilgiler sağlanır:

- Geçen hafta içinde gerçekleşen toplam değişiklik sayısı (FIM çalışma alanında etkinleştirilmemişse "-" bir tire görebilirsiniz)
- Çalışma alanına rapor veren toplam bilgisayar ve VM sayısı
- Çalışma alanının coğrafi konumu
- Çalışma alanının altında bulunduğu Azure aboneliği

Bir çalışma alanı için aşağıdaki düğmeler de görüntülenebilir:

- ![Simgeyi etkinleştir][3] FIM 'nin çalışma alanı için etkinleştirilmediğini belirtir. Çalışma alanının seçilmesi, çalışma alanının altındaki tüm makinelerde FIM 'yi etkinleştirmenizi sağlar.
- ![Planı Yükselt simgesi ][4] , çalışma alanının veya aboneliğin Güvenlik Merkezi 'Nin standart katmanı altında çalışmadığını gösterir. FIM özelliğini kullanmak için aboneliğinizin standart çalıştırması gerekir.  Çalışma alanının seçilmesi standart sürümüne yükseltmenizi sağlar. Standart katman ve yükseltme hakkında daha fazla bilgi edinmek için bkz. [Güvenlik Merkezi 'Nin standart katmanını gelişmiş güvenlik Için yükseltme](security-center-pricing.md).
- Boş (düğme yok), FIM 'nin çalışma alanında zaten etkinleştirildiği anlamına gelir.

**Dosya bütünlüğü izleme**altında, söz konusu çalışma alanı için FIM 'yi etkinleştirmek üzere bir çalışma alanı seçebilir, bu çalışma alanı Için dosya bütünlüğü izleme panosunu görüntüleyebilir veya çalışma alanını standart olarak [yükseltebilirsiniz](security-center-pricing.md) .

## <a name="enable-fim"></a>FIM 'yi etkinleştir
Bir çalışma alanında FIM 'yi etkinleştirmek için:

1. **Dosya bütünlüğü izleme**altında **Etkinleştir** düğmesine sahip bir çalışma alanı seçin.
2. **Dosya bütünlüğü Izlemeyi etkinleştirme** , çalışma alanındaki Windows ve Linux makinelerin sayısını görüntüleyerek açılır.

   ![Dosya bütünlüğünü izlemeyi etkinleştir][5]

   Windows ve Linux için önerilen ayarlar da listelenir.  Önerilen öğelerin tam listesini görmek için **Windows dosyaları**, **kayıt defteri**ve **Linux dosyalarını** genişletin.

3. FIM 'yi uygulamak istemediğiniz tüm önerilen varlıkların işaretini kaldırın.
4. FIM 'yi etkinleştirmek için **Dosya bütünlüğü Izlemeyi Uygula** ' yı seçin.

> [!NOTE]
> Ayarları dilediğiniz zaman değiştirebilirsiniz. Daha fazla bilgi edinmek için aşağıda izlenen varlıkları düzenleme bölümüne bakın.


## <a name="view-the-fim-dashboard"></a>FIM panosunu görüntüleme
**Dosya bütünlüğü izleme** panosu FIM 'nin etkinleştirildiği çalışma alanlarını görüntüler. FIM panosu, bir çalışma alanında FIM 'yi etkinleştirdikten sonra veya zaten FIM 'nin etkinleştirildiği **Dosya bütünlüğü izleme** penceresinde bir çalışma alanı seçtiğinizde açılır.

![Dosya bütünlüğü Izleme panosu][6]

Bir çalışma alanı için FIM Panosu aşağıdaki ayrıntıları görüntüler:

- Çalışma alanına bağlı toplam makine sayısı
- Seçilen süre boyunca gerçekleşen toplam değişiklik sayısı
- Değişiklik türü dökümü (dosyalar, kayıt defteri)
- Değişiklik kategorisinin bir dökümü (değiştirildi, eklendi, kaldırıldı)

Panonun üst kısmında filtrenin seçilmesi, değişikliklerini görmek istediğiniz süreyi uygulamanıza olanak sağlar.

![Zaman aralığı Filtresi][7]

**Bilgisayarlar** sekmesi (yukarıda gösterilen), bu çalışma alanına rapor veren tüm makineleri listeler. Her makine için Pano şunları listeler:

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

3. **Ekle**'yi seçin. **Değişiklik izleme Için Ekle** açılır.

   ![İstenen bilgileri girin][15]

4. **Ekle** sayfasında, istenen bilgileri yazın ve **Kaydet**' i seçin.

## <a name="disable-monitored-entities"></a>İzlenen varlıkları devre dışı bırak
1. **Dosya bütünlüğü izleme** panosuna dönün.
2. FIM 'in Şu anda etkin olduğu bir çalışma alanı seçin. Etkinleştir düğmesi veya planı yükselt düğmesi eksikse, bir çalışma alanı FIM için etkinleştirilir.

   ![FIM 'nin etkinleştirildiği bir çalışma alanı seçin][16]

3. Dosya bütünlüğü Izleme altında **Ayarlar**' ı seçin.

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
Bu makalede, güvenlik merkezi 'nde dosya bütünlüğü Izleme (FIM) kullanmayı öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki sayfalara bakın:

* [Güvenlik Ilkelerini ayarlama](tutorial-security-policy.md) --Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Güvenlik önerilerini yönetme](security-center-recommendations.md) --önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) - En son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-file-integrity-monitoring/security-center-dashboard.png
[2]: ./media/security-center-file-integrity-monitoring/file-integrity-monitoring.png
[3]: ./media/security-center-file-integrity-monitoring/enable.png
[4]: ./media/security-center-file-integrity-monitoring/upgrade-plan.png
[5]: ./media/security-center-file-integrity-monitoring/enable-fim.png
[6]: ./media/security-center-file-integrity-monitoring/fim-dashboard.png
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