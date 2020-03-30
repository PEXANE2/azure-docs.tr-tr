---
title: Azure Güvenlik Merkezi'nde Dosya Bütünlüğü İzleme | Microsoft Dokümanlar
description: Bu gözden geçirme yi kullanarak Azure Güvenlik Merkezi'nde Dosya Bütünlüğü İzleme (FIM) nasıl yapılandırılabildiğini öğrenin.
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
ms.openlocfilehash: 4d65ca8d97e1cca81886259d4f15cc880e45be9c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77604280"
---
# <a name="file-integrity-monitoring-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde Dosya Bütünlüğü İzleme
Bu gözden geçirme yi kullanarak Azure Güvenlik Merkezi'nde Dosya Bütünlüğü İzleme (FIM) nasıl yapılandırılabildiğini öğrenin.

## <a name="what-is-fim-in-security-center"></a>Güvenlik Merkezi'nde FIM nedir?
Değişiklik izleme olarak da bilinen Dosya Bütünlüğü İzleme (FIM), bir saldırıyı gösterebilecek değişiklikler için işletim sistemi, uygulama yazılımı ve diğerlerinin dosyalarını ve kayıt defterlerini inceler. Dosyanın geçerli durumunun dosyanın son tayından farklı olup olmadığını belirlemek için bir karşılaştırma yöntemi kullanılır. Dosyalarınızda geçerli veya şüpheli değişiklikler yapıldığını belirlemek için bu karşılaştırımdan yararlanabilirsiniz.

Güvenlik Merkezi'nin Dosya Bütünlüğü İzlemesi, Windows dosyalarının, Windows kayıt defterinin ve Linux dosyalarının bütünlüğünü doğrular. FIM'i etkinleştirerek izlenmesini istediğiniz dosyaları seçersiniz. Güvenlik Merkezi gibi etkinlik için FIM etkin dosyaları izler:

- Dosya ve Kayıt Defteri oluşturma ve kaldırma
- Dosya modifikasyonları (dosya boyutundaki değişiklikler, erişim denetim listeleri ve içeriğin karması)
- Kayıt defteri değişiklikleri (boyut değişiklikleri, erişim denetim listeleri, türü ve içeriği)

Güvenlik Merkezi, FIM'i kolayca etkinleştirebileceğiniz varlıkları izlemenizi önerir. İzlenecek kendi FIM ilkelerinizi veya varlıklarınızı da tanımlayabilirsiniz. Bu gözden geçirme size nasıl.

> [!NOTE]
> Dosya Bütünlüğü İzleme (FIM) özelliği Windows ve Linux bilgisayarları ve VM'leri için çalışır ve Güvenlik Merkezi'nin Standart katmanında kullanılabilir. Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md). FIM verileri Log Analytics çalışma alanına yükler. Yüklediğiniz veri miktarına bağlı olarak veri ücretleri uygulanır. Daha fazla bilgi edinmek için [Log Analytics fiyatlandırmasına](https://azure.microsoft.com/pricing/details/log-analytics/) bakın.

FIM, ortamınızdaki değişiklikleri izlemek ve tanımlamak için Azure Değişiklik İzleme çözümünü kullanır. Dosya Bütünlüğü İzleme etkinleştirildiğinde, tür **Çözüm'ün** **Değişiklik İzleme** kaynağına sahipsiniz. Veri toplama sıklığı ayrıntıları için Azure Değişiklik İzleme için İzleme yi [Değiştir veri toplama ayrıntılarına](https://docs.microsoft.com/azure/automation/automation-change-tracking#change-tracking-data-collection-details) bakın.

> [!NOTE]
> **İzlemeyi Değiştir** kaynağını kaldırırsanız, Güvenlik Merkezi'ndeki Dosya Bütünlüğü İzleme özelliğini de devre dışı kağınızda.

## <a name="which-files-should-i-monitor"></a>Hangi dosyaları izlemem gerekir?
Hangi dosyaların izleneceğini seçerken sisteminiz ve uygulamalarınız için kritik olan dosyaları düşünmelisiniz. Planlama yapmadan değiştirmeyi beklemediğiniz dosyaları seçmeyi düşünün. Uygulamalar veya işletim sistemi (günlük dosyaları ve metin dosyaları gibi) tarafından sık sık değiştirilen dosyaları seçmek, bir saldırıyı tanımlamayı zorlaştıran çok fazla gürültü oluşturur.

Güvenlik Merkezi, dosya ve kayıt defteri değişikliklerini içeren bilinen saldırı desenlerine göre varsayılan olarak izlemeniz gereken dosyaları önerir.

## <a name="using-file-integrity-monitoring"></a>Dosya Bütünlüğü İzlemeyi Kullanma
1. **Güvenlik Merkezi** panosunu açın.
2. **Gelişmiş Bulut Savunması**altındaki sol bölmede Dosya Bütünlüğü **İzleme'yi**seçin.
![Güvenlik Merkezi panosu][1]

**Dosya Bütünlüğü İzleme** açılır.
  ![Güvenlik Merkezi panosu][2]

Her çalışma alanı için aşağıdaki bilgiler sağlanır:

- Son hafta içinde gerçekleşen toplam değişiklik sayısı (çalışma alanında FIM etkin değilse bir tire "-" görebilirsiniz)
- Çalışma alanına rapor eden toplam bilgisayar ve VM sayısı
- Çalışma alanının coğrafi konumu
- Çalışma alanının altında olduğu Azure aboneliği

Çalışma alanı için aşağıdaki düğmeler de gösterilebilir:

- ![Simgeyi etkinleştir][3] Çalışma alanı için FIM'in etkin olmadığını gösterir. Çalışma alanını seçmek, çalışma alanı altındaki tüm makinelerde FIM'i etkinleştirmenizi sağlar.
- ![Yükseltme planı][4] simgesi Çalışma alanının veya aboneliğin Güvenlik Merkezi'nin Standart katmanı altında çalışmadığını gösterir. FIM özelliğini kullanmak için aboneliğinizin Standart olarak çalışıyor olması gerekir.  Çalışma alanını seçmek, Standart'a yükseltmenizi sağlar. Standart katman ve yükseltme hakkında daha fazla bilgi edinmek [için gelişmiş güvenlik için Güvenlik Merkezi'nin Standart katmanına Yükseltme](security-center-pricing.md)konusuna bakın.
- Boş (düğme yok) FIM'in çalışma alanında zaten etkin olduğu anlamına gelir.

**Dosya Bütünlüğü İzleme**altında, bu çalışma alanı için FIM'i etkinleştirmek, bu çalışma alanı için Dosya Bütünlüğü İzleme panosunu görüntülemek veya çalışma alanını [Standart'a yükseltmek](security-center-pricing.md) için bir çalışma alanı seçebilirsiniz.

## <a name="enable-fim"></a>FIM'i etkinleştir
FiM'i çalışma alanında etkinleştirmek için:

1. **Dosya Bütünlüğü İzleme**altında, **Etkinleştir** düğmesini içeren bir çalışma alanı seçin.
2. **Çalışma** alanı altında Windows ve Linux makinelerinin sayısını gösteren dosya bütünlüğü izleme yi etkinleştirin.

   ![Dosya bütünlüğü izlemeyi etkinleştirme][5]

   Windows ve Linux için önerilen ayarlar da listelenir.  Önerilen öğelerin tam listesini görmek için **Windows dosyalarını,** **Kayıt Defteri'ni**ve **Linux dosyalarını** genişletin.

3. FIM uygulamak istemediğiniz önerilen varlıkların denetimini kaldırın.
4. FIM'i etkinleştirmek için **dosya bütünlüğü izleme uygula'yı** seçin.

> [!NOTE]
> Ayarları istediğiniz zaman değiştirebilirsiniz. Daha fazla bilgi edinmek için aşağıdaki izlenen varlıkları edit'e bakın.
>
>

## <a name="view-the-fim-dashboard"></a>FIM panosunu görüntüleme
**Dosya bütünlüğü izleme** panosu, FIM'in etkin olduğu çalışma alanlarını görüntüler. FIM panosu, bir çalışma alanında FIM'i etkinleştirdikten sonra veya **Dosya Bütünlüğü İzleme** penceresinde FIM etkinleştirilmiş bir çalışma alanı seçtiğinizde açılır.

![Dosya Bütünlüğü İzleme panosu][6]

Çalışma alanı için FIM panosu aşağıdaki ayrıntıları görüntüler:

- Çalışma alanına bağlı toplam makine sayısı
- Seçili zaman dilimi içinde gerçekleşen toplam değişiklik sayısı
- Değişiklik türü (dosyalar, kayıt defteri) dökümü
- Değişiklik kategorisinin dökümü (değiştirilmiş, eklenen, kaldırılmış)

Panonun üst kısmında Filtre'yi seçmek, değişiklikleri görmek istediğiniz süreyi uygulamanızı sağlar.

![Zaman dilimi filtresi][7]

**Bilgisayarlar** sekmesi (yukarıda gösterilmiştir) bu çalışma alanına rapor eden tüm makineleri listeler. Her makine için pano listeleri:

- Seçili zaman döneminde gerçekleşen toplam değişiklikler
- Dosya değişiklikleri veya kayıt defteri değişiklikleri olarak toplam değişikliklerin dökümü

**Arama Günlüğü,** arama alanına bir makine adı girdiğinizde veya Bilgisayarlar sekmesi altında listelenen bir makine seçtiğinizde açılır. Log Search, makine için seçilen zaman dilimi içinde yapılan tüm değişiklikleri görüntüler. Daha fazla bilgi için değişikliği genişletebilirsiniz.

![Günlük araması][8]

**Değişiklikler** sekmesi (aşağıda gösterilmiştir) seçili zaman dilimi boyunca çalışma alanı için tüm değişiklikleri listeler. Değiştirilen her varlık için pano şunları listeler:

- Değişikliğin gerçekleştiği bilgisayar
- Değişiklik türü (kayıt defteri veya dosya)
- Değişiklik kategorisi (değiştirilmiş, eklenen, kaldırılmış)
- Değişikliğin tarihi ve saati

![Çalışma alanı için değişiklikler][9]

Arama alanına bir değişiklik girdiğinizde veya **Değişiklikler** sekmesi altında listelenen bir varlık seçtiğinizde **ayrıntıları değiştir** açılır.

![Ayrıntıları değiştirme][10]

## <a name="edit-monitored-entities"></a>İzlenen varlıkları edin

1. **Dosya Bütünlüğü İzleme panosuna** dönün ve **Ayarlar'ı**seçin.

   ![Ayarlar][11]

   **Çalışma Alanı Yapılandırması** üç sekme yle açılır: **Windows Registry,** **Windows Files**ve **Linux Files**. Her sekme, bu kategoride atabileceğiniz varlıkları listeler. Listelenen her varlık için, Güvenlik Merkezi FIM etkin (true) veya etkin (false) olup olmadığını tanımlar.  Varlığı düzenlemek FIM'i etkinleştirmenizi veya devre dışı etmenizi sağlar.

   ![Çalışma alanı yapılandırması][12]

2. Kimlik koruması seçin. Bu örnekte, Windows Registry altında bir öğe seçtik. **Değişiklik İzleme için Edit** açılır.

   ![İzlemeyi değiştirme veya değiştirme][13]

**Değişiklik İzleme için Edit** altında şunları yapabilirsiniz:

- Etkinleştir (True) veya devre dışı (False) dosya bütünlüğü izleme
- Varlık adını sağlama veya değiştirme
- Değeri veya yolu sağlama veya değiştirme
- Varlığı silme, değişikliği atma veya değişikliği kaydetme

## <a name="add-a-new-entity-to-monitor"></a>İzlemek için yeni bir varlık ekleme
1. **Dosya bütünlüğü izleme panosuna** dönün ve en üstteki **Ayarlar'ı** seçin. **Çalışma Alanı Yapılandırması** açılır.
2. **Çalışma Alanı Yapılandırması**altında, eklemek istediğiniz varlık türü için sekmeyi seçin: Windows Kayıt Defteri, Windows Dosyaları veya Linux Dosyaları. Bu örnekte, **Linux Dosyalarını**seçtik.

   ![İzlemek için yeni bir öğe ekleme][14]

3. **Ekle'yi**seçin. **Change Tracking için ekle** açılır.

   ![İstenen bilgileri girin][15]

4. **Ekle** sayfasında, istenen bilgileri yazın ve **Kaydet'i**seçin.

## <a name="disable-monitored-entities"></a>İzlenen varlıkları devre dışı
1. **Dosya Bütünlüğü İzleme** panosuna geri dönün.
2. FIM'in şu anda etkin olduğu bir çalışma alanı seçin. Etkinleştir düğmesi veya Yükseltme Planı düğmesi eksikse FIM için bir çalışma alanı etkinleştirilir.

   ![FIM'in etkin olduğu bir çalışma alanı seçme][16]

3. Dosya Bütünlüğü İzleme altında **Ayarlar'ı**seçin.

   ![Ayarlar'ı seçin][17]

4. **Çalışma Alanı Yapılandırması**altında, **Etkin'in** doğru olarak ayarlandığı bir grup seçin.

   ![Çalışma Alanı Yapılandırması][18]

5. **Değişiklik İzleme penceresi için Edit** altında **Etkin'den** False'a ayarlayın.

   ![Etkin'in yanlış ait olmasını ayarlama][19]

6. **Kaydet'i**seçin.

## <a name="folder-and-path-monitoring-using-wildcards"></a>Joker karakterleri kullanarak klasör ve yol izleme

Dizinler arasında izlemeyi kolaylaştırmak için joker karakterleri kullanın. Joker karakterleri kullanarak klasör izlemeyi yapılandırdığınızda aşağıdaki kurallar geçerlidir:
-   Joker karakterler birden çok dosyayı izlemek için gereklidir.
-   Joker karakterler yalnızca C:\folder\file veya /etc/*.conf gibi bir yolun son segmentinde kullanılabilir
-   Bir ortam değişkeni geçerli olmayan bir yol içeriyorsa, doğrulama başarılı olur, ancak stok çalıştığında yol başarısız olur.
-   Yolu ayarlarken, çok fazla klasörün\*geçmesine neden olacak c: .* gibi genel yollardan kaçının.

## <a name="disable-fim"></a>FIM'i devre dışı
FIM'i devre dışı kullanabilirsiniz. FIM, ortamınızdaki değişiklikleri izlemek ve tanımlamak için Azure Değişiklik İzleme çözümünü kullanır. FIM'i devre dışı bırakarak, İzlemeyi Değiştir çözümünü seçili çalışma alanından kaldırırsınız.

1. FIM'i devre dışı kakmak için **Dosya Bütünlüğü İzleme** panosuna dönün.
2. Bir çalışma alanı seçin.
3. **Dosya Bütünlüğü İzleme**altında, Devre Dışı **Nı**seçin.

   ![FIM'i devre dışı][20]

4. Devre dışı kalmak için **Kaldır'ı** seçin.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Güvenlik Merkezi'nde Dosya Bütünlüğü İzleme (FIM) kullanmayı öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki sayfalara bakın:

* [Güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini nasıl yapılandırıştıracaklarını öğrenin.
* [Güvenlik önerilerini yönetme](security-center-recommendations.md) -- Önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Güvenlik durumu izleme](security-center-monitoring.md)-- Azure kaynaklarınızın sistem durumunu nasıl izleyeceğinizi öğrenin.
* [Güvenlik uyarılarını yönetme ve yanıtla --](security-center-managing-and-responding-alerts.md)Güvenlik uyarılarını yönetme yi ve yanıt vermeyi öğrenin.
* [İş ortağı çözümlerini izleme](security-center-partner-solutions.md) -- İş ortağı çözümlarınızın sağlık durumunu nasıl izleyeceğinizi öğrenin.
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
