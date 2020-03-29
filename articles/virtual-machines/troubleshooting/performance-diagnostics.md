---
title: Azure sanal makineleri için performans tanılama| Microsoft Dokümanlar
description: Windows için Azure Performans Tanılama'yı tanılar.
services: virtual-machines-windows'
documentationcenter: ''
author: anandhms
manager: cshepard
editor: przlplx
tags: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 9/20/2018
ms.author: anandh
ms.openlocfilehash: 16be3d1695608165405a3490b686a01ba6a2a62c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70080611"
---
# <a name="performance-diagnostics-for-azure-virtual-machines"></a>Azure sanal makineleri için performans tanılamaları

Performans tanılama aracı, Windows veya Linux sanal makinesini (VM) etkileyebilecek performans sorunlarını gidermenize yardımcı olur. Desteklenen sorun giderme senaryoları, bilinen sorunlar ve en iyi uygulamalar üzerinde hızlı denetimler ve yavaş VM performansı veya CPU, disk alanı veya bellek yüksek kullanımı içeren karmaşık sorunları içerir.

Performans tanılamalarını doğrudan Azure portalından çalıştırabilirsiniz, burada öngörüleri ve çeşitli günlükleri, zengin yapılandırma ve tanılama verilerini de inceleyebilirsiniz. Microsoft Destek'e başvurmadan önce performans tanılama ları çalıştırmanızı ve öngörüleri ve tanılama verilerini gözden geçirmenizi öneririz.

> [!NOTE]
> Windows için Performans tanılama şu anda .NET SDK sürüm 4.5 veya daha sonraki bir sürümü yüklü VM'lerde desteklenir. Klasik VM'lerde performans tanılamasını çalıştırma adımları için [Azure Performans Tanılama VM uzantısına](performance-diagnostics-vm-extension.md)bakın.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

### <a name="windows"></a>Windows

Windows 10, Windows 8, Windows 8 Enterprise, Windows 8 Pro, Windows 8.1, Windows Server 2016, Windows Server 2012, Windows Server 2012 Datacenter, Windows Server 2012 R2, Windows Server 2012 R2 Datacenter, Windows Server 2012 R2 Standard, Windows Server 2012 R2 Standard, Windows Server 2012 Standart, Windows Server 2008 R2, Windows Server 2008 R2 Datacenter, Windows Server 2008 R2 Enterprise, Windows Server 2008 R2 Foundation, Windows Server 2008 R2 SP1, Windows Server 2008 R2 Standard.

### <a name="linux"></a>Linux

Oracle Linux Server 6.10 [`*`], 7.3, 7.6, 7.5 (Oracle-Database-Ee 13.8 marketplace image), CentOS 6.5 [ ],`*`7.6, RHEL 7.2, 7.5, 8.0 [ ],`*`Ubuntu 14.04, 16.04, 18.04, Debian 8, 9, 10 ]`*`[, SLES`*`12S ]

>[!Note]
>[`*`] Lütfen [bilinen sorunlara](how-to-use-perfinsights-linux.md#known-issues) bakın

## <a name="install-and-run-performance-diagnostics-on-your-vm"></a>VM'nizde performans tanılamayı yükleyin ve çalıştırın

Performans tanılama PerfInsights adlı bir tanılama aracı çalıştıran bir VM uzantısı yükler. PerfInsights hem [Windows](https://aka.ms/perfinsights) hem de [Linux](https://aka.ms/perfinsightslinux)için kullanılabilir. Performans tanılamayı yüklemek ve çalıştırmak için aşağıdaki adımları izleyin:

1. Komutların sol sütununda **Sanal makineleri**seçin.
1. VM adları listesinden, tanılamayı çalıştırmak istediğiniz VM'yi seçin.
1. Sağ komut sütununda Performans **tanılama'yı**seçin.

    ![Azure portalının ekran görüntüsü, Performans tanılama yı yükle düğmesi vurgulanır](media/performance-diagnostics/performance-diagnostics-install.png)

    > [!NOTE]
    > Bu ekran görüntüsünde, VM adlarının bıçak gizlidir.
1. Bir depolama hesabı seçin (isteğe bağlı)

    Performans tanılama sonuçlarını birden çok VM'nin depolanması için tek bir depolama hesabı kullanmak istiyorsanız, araç çubuğundaki **Ayarlar** düğmesini tıklatarak bir depolama hesabı seçebilirsiniz. Depolama hesabını seçtikten sonra **Tamam** düğmesini tıklatın.

    Bir depolama hesabı belirtmezseniz, varsayılan olarak yeni bir depolama hesabı oluşturulur.

    ![Ayarlar araç çubuğu düğmesi vurgulanmış performans tanılama bıçağının ekran görüntüsü](media/performance-diagnostics/settings-button.png)

    ![Performans tanılama ayarları blade'den depolama hesabı seçiminin ekran görüntüsü](media/performance-diagnostics/select-storage-account.png)

1. Performans **tanılama yı yükle** düğmesini seçin.
1. Yükleme tamamlandıktan sonra tanılama çalıştırmak istiyorsanız **Çalıştır tanılama** onay kutusunu seçin. Bu seçimi yaparsanız, performans çözümleme senaryosunu ve ilgili seçenekleri seçebilirsiniz.

    ![Performans tanılama yükleme düğmesinin ekran görüntüsü](media/performance-diagnostics/install-diagnostics-button.png)

## <a name="select-an-analysis-scenario-to-run"></a>Çalıştırmak için bir analiz senaryosu seçin

Aşağıdaki analiz senaryoları Azure portalından edinilebilir. Yaşadığınız performans sorununa bağlı olarak bir çözümleme seçin. Çözümleme için gerekli olan süre ve izleme seçeneklerini seçin.

* **Hızlı performans analizi**  
    Bilinen sorunları denetler, en iyi uygulamaları analiz eder ve tanılama verilerini toplar. Bu çözümlemenin çalışması birkaç dakika sürer. Daha fazla [Windows](https://aka.ms/perfinsights/quick) veya [Linux](https://aka.ms/perfinsightslinux/quick) öğrenin

* **Performans analizi**  
    Hızlı performans analizindeki tüm denetimleri içerir ve yüksek kaynak tüketimini izler. Yüksek CPU, bellek ve disk kullanımı gibi genel performans sorunlarını gidermek için bu sürümü kullanın. Bu çözümleme, seçili süreye bağlı olarak 30 saniye ile 15 dakika arasında sürer. Daha fazla [Windows](https://aka.ms/perfinsights/vmslow) veya [Linux](https://aka.ms/perfinsightslinux/vmslow) öğrenin

* **Gelişmiş performans analizi**`*`  
    Performans çözümlemesindeki tüm denetimleri içerir ve aşağıdaki bölümlerde listelenen izlerden birini veya birkaçını toplar. Ek izlemegerektiren karmaşık sorunları gidermek için bu senaryoyu kullanın. Bu senaryoyu daha uzun süreler boyunca çalıştırmak, VM'nin boyutuna ve seçilen izleme seçeneklerine bağlı olarak tanılama çıktısının genel boyutunu artırır. Bu çözümleme, seçili süreye bağlı olarak çalışması 30 saniye ile 15 dakika arasında sürer. [Daha fazlasını öğrenin](https://aka.ms/perfinsights/advanced)

* **Azure Dosyaları analizi**`*`  
    Performans analizindeki tüm denetimleri içerir ve bir ağ izleme sini ve Kobİ sayaçlarını yakalar. Azure dosyalarının performansını gidermek için bu senaryoyu kullanın. Bu çözümleme, seçili süreye bağlı olarak çalışması 30 saniye ile 15 dakika arasında sürer. [Daha fazlasını öğrenin](https://aka.ms/perfinsights/azurefiles)

>[!Note]
>[`*`] Bu çözümleme senaryoları yalnızca Windows'da desteklenir.

![Performans tanılama bıçağının içindeki Çalıştır tanılama bölmesinin ekran görüntüsü](media/performance-diagnostics/run-diagnostics-pane.png)

### <a name="provide-symptoms-optional"></a>Belirtiler sağlayın (isteğe bağlı)

Listeden önceden seçilmiş tüm belirtileri seçin veya yeni belirtiler ekleyin. Bu, gelecekte analiz geliştirmemize yardımcı olur.

### <a name="provide-support-request-number-if-available-optional"></a>Varsa destek istek numarası sağlayın (isteğe bağlı)

Varolan bir destek bileti üzerinde bir Microsoft destek mühendisiyle çalışıyorsanız, destek bilet numarasını sağlayın.

### <a name="review-the-privacy-policy-and-legal-terms-and-select-the-check-box-to-acknowledge-required"></a>Gizlilik politikasını ve yasal koşulları gözden geçirin ve onay için onay kutusunu seçin (gerekli)

Tanılamayı çalıştırmak için yasal koşulları kabul etmeli ve gizlilik politikasını kabul etmelisiniz.

### <a name="select-ok-to-run-the-diagnostics"></a>Tanılamayı çalıştırmak için Tamam'ı seçin

Performans tanılama yüklemeye başladığında bir bildirim görüntülenir. Yükleme tamamlandıktan sonra, yüklemenin başarılı olduğunu belirten bir bildirim görürsünüz. Seçili çözümleme daha sonra belirtilen süre için çalıştırılır. Tanılama verilerinin doğru zamanda yakalanması için performans sorununu yeniden oluşturmak için iyi bir zaman olabilir.

Çözümleme tamamlandıktan sonra, aşağıdaki öğeler Azure tablolarına ve belirtilen depolama hesabındaki ikili büyük nesne (BLOB) kapsayıcısına yüklenir:

* Çalışma yla ilgili tüm bilgiler ve ilgili bilgiler
* Windows'da sıkıştırılmış (.zip) **(PerformanceDiagnostics_yyyy-MM-dd_hh-mm-ss-fff.zip** olarak adlandırılır) ve Linux'ta günlük dosyaları içeren katran topu dosyası **(PerformanceDiagnostics_yyyy-MM-dd_hh-mm-ss-fff.tar.gz** olarak adlandırılır)
* Bir HTML raporu

Yüklemeden sonra, Azure portalında yeni bir tanılama raporu listelenir.

![Performans tanılama bıçağında tanılama raporunun bir listesinin ekran görüntüsü](media/performance-diagnostics/diagnostics-report-list.png)

## <a name="how-to-change-performance-diagnostics-settings"></a>Performans tanılama ayarları nasıl değiştirilir?

Tanılama öngörülerinin ve çıktılarının depolanabileceği depolama hesabını değiştirmek için **Ayarlar** araç çubuğunu kullanın. Performans tanılama kullanan birden çok VM için aynı depolama hesabını kullanabilirsiniz. Depolama hesabını değiştirdiğinizde, eski raporlar ve öngörüler silinmez. Ancak, bunlar artık tanılama raporları listesinde görüntülenmez.

## <a name="review-insights-and-performance-diagnostics-report"></a>Öngörüleri ve performans tanılama raporunu gözden geçirin

Her tanılama çalışması, toplanan öngörüler ve öneriler, etkilenen kaynaklar, günlük dosyaları ve toplanan diğer zengin tanılama bilgilerinin yanı sıra çevrimdışı görüntüleme için bir rapor içerir. Toplanan tüm tanılama verilerinin tam listesi için, [Windows](how-to-use-perfinsights.md#what-kind-of-information-is-collected-by-perfinsights) veya [Linux'ta](how-to-use-perfinsights-linux.md#what-kind-of-information-is-collected-by-perfinsights) **PerfInsights tarafından ne tür bilgilerin toplandığını** görün.

### <a name="select-a-performance-diagnostics-report"></a>Performans tanılama raporu seçme

Çalıştırılanan tüm tanılama raporlarını bulmak için tanılama raporu listesini kullanabilirsiniz. Liste, kullanılan analiz, bulunan öngörüler ve bunların etki düzeyleri hakkında ayrıntılar içerir. Daha fazla ayrıntı görüntülemek için bir satır seçin.

![Performans tanılama bıçağından bir tanılama raporu seçme ekran görüntüsü](media/performance-diagnostics/select-report.png)

### <a name="review-a-performance-diagnostics-report"></a>Performans tanılama raporunu gözden geçirme

Her performans tanılama raporu çeşitli öngörüler içerebilir ve etki düzeyi Yüksek, Orta veya Düşük'ü gösterebilir. Her içgörü aynı zamanda endişeyi azaltmaya yardımcı olacak öneriler de içerir. İstatistikler kolay filtreleme için gruplandırılır.

Etki düzeyleri, yanlış yapılandırma, bilinen sorunlar veya diğer kullanıcılar tarafından bildirilen sorunlar gibi etkenlere bağlı olarak performans sorunları potansiyelini temsil ediyor. Listelenen sorunlardan birini veya daha fazlasını henüz yaşıyor olmayabilirsiniz. Örneğin, aynı veri diskinde SQL günlük dosyaları ve veritabanı dosyaları olabilir. Veritabanı kullanımı yüksekse, bu durum darboğazlar ve diğer performans sorunları için yüksek bir potansiyele sahiptir, ancak kullanım düşükse bir sorun fark etmeyebilirsiniz.

![Performans tanılama raporunun ekran görüntüsü](media/performance-diagnostics/performance-diagnostics-report-overview.png)

### <a name="reviewing-performance-diagnostics-insights-and-recommendations"></a>Performans tanılama öngörülerini ve önerilerini gözden geçirme

Etkilenen kaynaklar, önerilen azaltıcı etkenler ve başvuru bağlantıları hakkında daha fazla ayrıntı görüntülemek için bir öngörü seçebilirsiniz.

![Performans tanılama içgörü ayrıntısının ekran görüntüsü](media/performance-diagnostics/insight-detail.png)

### <a name="download-and-review-the-full-performance-diagnostics-report"></a>Tam performans tanılama raporunu indirin ve gözden geçirin

Depolama ve ağ yapılandırması, performans sayaçları, izlemeler, işlemler listesi ve günlükler gibi ek zengin tanılama bilgileri içeren bir HTML raporunu indirmek için **Raporu İndir** düğmesini kullanabilirsiniz. İçerik seçili analize bağlıdır. Gelişmiş sorun giderme için rapor, yüksek CPU kullanımı, yüksek disk kullanımı ve aşırı bellek tüketen işlemlerle ilgili ek bilgiler ve etkileşimli grafikler içerebilir. Performans tanılama raporu hakkında daha fazla bilgi için [Windows](how-to-use-perfinsights.md#review-the-diagnostics-report) veya [Linux'a](how-to-use-perfinsights-linux.md#review-the-diagnostics-report)bakın.

## <a name="manage-performance-diagnostics-reports"></a>Performans tanılama raporlarını yönetme

**Raporu Sil** düğmesini kullanarak bir veya daha fazla performans tanılama raporunu silebilirsiniz.

## <a name="how-to-uninstall-performance-diagnostics"></a>Performans tanılama nasıl kaldırılı

Performans tanılamalarını bir VM'den kaldırabilirsiniz. Bu eylem VM uzantısını kaldırır, ancak depolama hesabındaki tanılama verilerini etkilemez.

![Kaldır düğmesi vurgulanmış performans tanılama bıçak araç çubuğunun ekran görüntüsü](media/performance-diagnostics/uninstal-button.png)

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="where-is-the-diagnostics-data-from-my-vm-stored"></a>VM depolanan tanılama verileri nerede

Tüm performans tanılama öngörüleri ve raporları kendi depolama hesabınızda depolanır. Öngörüler Azure tablolarında depolanır. Raporlar sıkıştırılmış dosya azdiagextnresults adlı bir ikili büyük nesne (BLOB) kapsayıcısında depolanır.

Araç çubuğundaki Ayarlar düğmesini kullanarak depolama hesabı bilgilerini görüntüleyebilirsiniz.

### <a name="how-do-i-share-this-data-with-microsoft-customer-support"></a>Bu verileri Microsoft Müşteri Desteği ile nasıl paylaşacağım?

Tanılama raporunu Microsoft ile paylaşmanın birden çok yolu vardır.

**Seçenek 1:** En son raporu otomatik olarak paylaşın  
Microsoft ile bir destek bileti açtığınızda, performans tanılama raporunu paylaşmak önemlidir. Tanılamayı çalıştırırken bu bilgileri Microsoft ile paylaşmayı seçtiyseniz ("**Tanılama bilgilerini Microsoft ile paylaşmayı kabul ediyorum**" onay kutusunu seçerek), Microsoft çalışma tarihinden itibaren 30 gün boyunca çıktı zip dosyasına bir SAS bağlantısı kullanarak rapora depolama hesabınızdan erişebilir. Destek mühendisi ne kadar son rapora açıktır.

**Seçenek 2:** Tanılama raporu sıkıştırılmış dosya için Paylaşılan Erişim İmzası oluşturma  
Paylaşılan Erişim İmzaları'nı kullanarak sıkıştırılmış raporlara bağlantı paylaşabilirsiniz. Bunu yapmak için şu adımları uygulayın:

1. Azure portalında, tanılama verilerinin depolandığı depolama hesabına göz atın.
1. **Blob servis** bölümünün altındaki **Blobs'u** seçin.
1. **azdiagextnresults konteynerini** seçin.
1. Paylaşmak istediğiniz Performans tanılama çıktısı sıkıştırılmış dosyayı seçin.
1. **SAS oluştur** sekmesinde, paylaşım ölçütlerini seçin.
1. **Blob SAS belirteci ve URL oluştur'u**tıklatın.
1. **Blob SAS URL'sini**kopyalayın ve destek mühendisiyle paylaşın.

**Seçenek 3:** Raporu depolama hesabından indirin

Ayrıca, Seçenek 2'deki 1-4 adımlarını kullanarak performans tanılama raporu sıkıştırılmış dosyayı da bulabilirsiniz. Dosyayı karşıdan yükleyin ve ardından e-posta yoluyla paylaşın veya destek mühendisinden dosyayı yüklemesi için talimat isteyin.  

### <a name="how-do-i-capture-the-diagnostics-data-at-the-correct-time"></a>Tanılama verilerini doğru zamanda nasıl yakalarım?

Her performans tanılama çalışmasının iki aşaması vardır:

1. Performans tanılama VM uzantısını yükleyin veya güncelleyin.
1. Belirtilen süre boyunca tanılamayı çalıştırın.

Şu anda VM uzantı yüklemesinin tam olarak ne zaman tamamolduğunu bilmenin kolay bir yolu yoktur. Genellikle VM uzantısı yüklemek için 1 dakika yaklaşık 45 saniye sürer. VM uzantısı yüklendikten sonra, performans tanılamalarının sorun giderme için doğru veri kümesini yakalamasını sağlamak için repro adımlarınızı çalıştırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Performans tanılama öngörülerini ve raporunu gözden geçirdikten sonra, sorunun nedenini hala belirleyemiyorsanız ve daha fazla yardıma ihtiyacınız varsa, Microsoft Müşteri Desteği ile bir destek bileti açabilirsiniz.

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/)gidin ve destek **al'ı**seçin. Azure desteğini kullanma hakkında daha fazla bilgi için [Microsoft Azure destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.
