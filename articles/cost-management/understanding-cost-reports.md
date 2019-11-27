---
title: Anlama Cloudyn maliyet Yönetimi raporlarını azure'da | Microsoft Docs
description: Bu makalede, Cloudyn maliyet yönetim raporları temel yapısını ve işlevlerini anlamanıza yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/20/2019
ms.topic: conceptual
ms.service: cost-management-billing
manager: benshy
ms.custom: seodec18
ms.openlocfilehash: d847b78ba9623f3543a3cb1e45b5187605deb550
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74229776"
---
# <a name="understanding-cloudyn-cost-management-reports"></a>Maliyet Yönetimi raporlarını anlama Cloudyn

Bu makalede, Cloudyn maliyet yönetim raporları temel yapısını ve işlevlerini anlamanıza yardımcı olur. Cloudyn raporların çoğu, sezgisel ve Tekdüzen bir görünümüne sahip. Bu makaleyi okuduktan sonra maliyet Yönetimi raporlarını kullanmak hazır olursunuz. Birçok standart özellikler raporları kolayca gezinmenizi sağlayan çeşitli raporlar boyunca kullanılabilir. Raporları özelleştirilebilir ve hesaplamak ve sonuçları görüntülemek için çeşitli seçenekler arasından seçim yapabilirsiniz.

## <a name="report-fields-and-options"></a>Rapor alanlar ve Seçenekler

Zaman içinde Maliyet raporu örneği göz aşağıda verilmiştir. Çoğu Cloudyn raporlarında benzer bir düzeni vardır.

![Zaman içinde Maliyet raporu açıklamaları için karşılık gelen numaralı alanları örneği](./media/understanding-cost-reports/sample-report.png)

Bir önceki resimde her numaralı alan aşağıdaki bilgileri ayrıntılı açıklanmıştır:

1. **Tarih aralığı**

    Hazır veya özel kullanarak bir raporun zaman aralığını tanımlamak için tarih aralığını listeyi kullanın.
2. **Filtre kaydedildi**

    Kaydedilmiş filtre listesi, rapora uygulanan filtreleri ve geçerli gruplar kaydetmek için kullanın. Kayıtlı filtreler gibi maliyet ve performans raporları kullanılabilir:

      - Maliyet analizi
      - ayırma
      - Varlık Yönetimi
      - İyileştirme

   Bir filtre adı yazın ve **Kaydet**' e tıklayın.

3. **Etiketler**

    Grup etiketlerini alana etiketi kategorilere göre kullanın. Menüde listelenen etiketler Azure kullanımında bölümünüzün veya maliyet merkezi etiketleri veya Cloudyn'ın maliyet varlığı ve abonelik etiketler. Sonuçları filtrelemek için etiketler seçin. Sonuçları filtrelemek için bir etiket adı (anahtar) de yazabilirsiniz.

    ![Sonuçları filtrelemek için Etiketler listesi örneği](./media/understanding-cost-reports/select-options.png)

    Yeni bir filtre eklemek için **Ekle** ' ye tıklayın.

    ![Seçenekler ve koşullara göre filtrelemek için gösteren filtre Kutusu Ekle](./media/understanding-cost-reports/add-filter.png)

    Gruplandırma ve süzme etiket, Azure kaynaklarını veya kaynak grubu etiketleri ilişkili değil.

    Maliyet ayırma etiketi gruplama ve filtreleme, **gruplar** menü seçeneğinde kullanılabilir.

4. **Raporlardaki gruplar**

    Grupları kullanma maliyet analizi raporları, standart, gösterilecek dökümü raporunuzdaki faturalama öğesinden kategoriler.  Bununla birlikte, maliyet ayırma raporları show gruplarında etiket tabanlı kategorilerini görüntüleyin. Etiket tabanlı kategoriler, maliyet dağıtma modeli ve faturalama verileri standart dökümü kategorilerden tanımlanır.

    ![Etiketlere göre gruplandırabilirsiniz ilk örnek listesi](./media/understanding-cost-reports/groups-tags01.png)

    ![Etiketlere göre gruplandırabilirsiniz ikinci örnek listesi](./media/understanding-cost-reports/groups-tags02.png)

    Maliyet ayırma raporlarında etiketi tabanlı Grup kategorilerde grupları şunlar olabilir:
      - Etiketler
      - kaynak grubu etiketleri
      - Cloudyn varlık etiketleri maliyeti
      - Maliyet ayırmayı amaçlar için abonelik etiketi kategorileri

   Örnek verilebilir:
   - Maliyet merkezi
   - Bölüm
   - Uygulama
   - Ortam
   - Maliyet kodu

     Raporlarda kullanılabilir yerleşik gruplar listesi aşağıdadır:

     - **Maliyet türü**
     - Maliyet türü veya birden çok maliyet türü seçin veya tümünü seçin. Maliyet türleri şunlardır:
       - Tek seferlik bir ücret
       - Destek
       - Kullanım ücreti
     - **Müşterisi**
       - Belirli bir müşteri, birden çok müşteriyi veya tüm müşterileri seçin.
     - **Hesap adı**
       - Hesabı veya aboneliği adı. Azure'da, Azure aboneliğinin adıdır.
     - **Hesap No**
       - Bir hesap, birden çok hesabı veya tüm hesapları seçin. Azure'da, bu Azure aboneliğinin GUID değeridir.
     - **Ana hesap**
       - Ana hesap, birden çok hesabı veya Seç'i seçin.
     - **Hizmet**
       - Bir hizmet birden çok hizmeti seçin veya tüm hizmetleri seçin.
     - **Sağlayıcı**
       - Varlıklar ve masrafları ilişkili olduğu bulut sağlayıcısı.
     - **Bölge**
       - Kaynak barındırıldığı bölge.
     - **Kullanılabilirlik alanı**
       - Bir bölge içinde yalıtılmış AWS konumları.
     - **Kaynak Türü**
       - Kaynak Kullanım türü.
     - **Alt tür**
       - Alt türü seçin.
     - **İşlem**
       - İşlemi seçin veya **Tümünü gösterin**.
     - **Fiyat modeli**
       - Tüm ön maliyet
       - Ön maliyet yok
       - Kısmi ön maliyet
       - İsteğe Bağlı
       - Ayırma
       - Nokta
     - **Ücret türü**
       - Negatif veya pozitif farkı türü veya her ikisini de seçin.
     - **Mimari**
       - Olup bir makine adanmış bir makinede çalışıyorsa.
     - **Kullanım türü**
       - Kullanım türü, tek seferlik ücretler veya yinelenen ücretlerini olabilir.

5. **Yorsa**

    Aralıkları seçili değerlere ayarlamak için tek veya çoklu seçim filtrelerini kullanın. Filtre ayarlamak için **Ekle** ' ye tıklayın ve kategorileri ve değerleri filtrele ' yi seçin.

6. **Maliyet modeli**

    Maliyet modeli ile 360 maliyet, daha önce oluşturduğunuz bir maliyet modeli seçmek için kullanın. Birden çok Cloudyn maliyet modelleri, maliyet ayırma gereksinimlerinize bağlı olarak olabilir. Kuruluş takımlarınızın bazıları diğerlerinden farklı ayırma gereksinimlerini maliyet. Her ekip kendi özel bir maliyet modeli olabilir.

    Maliyet ayırma modeli tanımı oluşturma hakkında daha fazla bilgi için bkz. [özel etiketleri kullanarak maliyet ayırma](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs).

7. **Ödeme**

    Maliyet ayırmayı raporlarda kullanmak itfa kullanım dışı görüntülemek için hizmet ücretlerini veya tek seferlik borç maliyetleri ve bunların kullanım ömrü boyunca eşit olarak zaman içinde maliyetlerine yayılabilir. Tek seferlik ücretler örnekleri içerebilir:
    - Yıllık destek ücretleri
    - Yıllık güvenlik bileşenleri ücretleri
    - Ayrılmış örnekler satın alma ücreti
    - Bazı Azure Market öğesi.

   İtfası altında, **itfası maliyeti** veya **gerçek maliyet**' i seçin.

8. **Çözünürlüğüne**

    Çözümleme zamanı çözünürlüğü seçili tarih aralığı içinde seçmek için kullanın. Birimleri nasıl bir raporda görüntülenen ve olabilir, zaman çözünürlüğünü belirler:
    - Günlük
    - Haftalık
    - Aylık
    - Üç Aylık
    - Yıllık

9. **Ayırma kuralları**

    Ayırma kurallarını uygulamak veya maliyet ayırma Maliyeti yeniden hesaplama devre dışı bırakmak için kullanın. Etkinleştirmek veya faturalama verileri için maliyet ayırmayı yeniden hesaplama devre dışı bırakabilirsiniz. Rapordaki seçili kategorilerdeki yeniden hesaplama uygular. Fatura ham verilere karşı maliyet ayırma yeniden hesaplama etkisini değerlendirmenize olanak tanır.

10. **Kategorilere ayrılmamış**

    Kategorilere ayrılmamış dahil etmek veya rapordaki Kategorilere ayrılmamış maliyetleri hariç tutmak için kullanın.

11. **Alanları göster/gizle**

    Gösterme/gizleme seçeneğini raporlarında hiçbir etkisi yok.

12. **Görüntüleme biçimleri**

    Görüntü biçimlerini çeşitli graph'i ya da tablo görünümleri seçmek için kullanın.

    ![Görüntü biçimlerinin seçebileceğiniz semboller](./media/understanding-cost-reports/display-formats.png)

13. **Çoklu renk**

    Çok renkli raporunuzda grafikleri rengini ayarlamak için kullanın.

14. **Eylemler**

    Eylemleri Kaydet, dışarı aktarma veya raporu zamanladığınız kullanın.

15. **İlke**

    Resimli değil olsa da, bazı raporlar öngörülen maliyet hesaplaması ilke içerir. Bu raporlarda, **birleştirilmiş** ilke Microsoft kaydı veya AWS yükümlüsü gibi geçerli varlık kapsamındaki tüm hesaplar ve abonelikler için öneriler gösterir. **Tek başına** ilkesi, başka bir abonelik yoksa, bir hesap veya aboneliğin önerilerini gösterir. Kuruluşunuz tarafından kullanılan en iyi duruma getirme stratejisinin temel seçtiğiniz ilke değişir. Maliyet tahminleri üzerinde son 30 gün kullanımı temel alır.

## <a name="save-and-schedule-reports"></a>Kaydet ve raporları zamanlama

Bir rapor oluşturduktan sonra gelecekte kullanılmak üzere kaydedebilirsiniz. Kaydedilen raporlar **araçlarım** > **Raporlarım**kullanılabilir. Mevcut bir rapora değişiklikleri yapın ve kaydedin, rapor yeni bir sürüm olarak kaydedilir. Veya yeni bir rapor olarak kaydedin.

### <a name="save-a-report-to-the-cloudyn-portal"></a>Cloudyn portalında rapor kaydetme

Herhangi bir raporu görüntülerken, **Eylemler** ' e tıklayın ve ardından **raporlarıma kaydet**' i seçin. Rapor adı ve ekleyin ya da sonra bir kendi URL ya da otomatik olarak oluşturulan URL'yi kullanın. İsteğe bağlı olarak, raporu kuruluşunuzdaki diğer kullanıcılarla **paylaşabilir** veya varlığınızla paylaşabilirsiniz. Rapor paylaşmıyorsa, kişisel rapor kalır ve yalnızca görüntüleyebilir. Raporu kaydedin.


### <a name="save-a-report-to-cloud-provider-storage"></a>Bulut sağlayıcısı depolama için rapor kaydetme

Bulut hizmet sağlayıcınız bir raporu kaydetmek için zaten bir depolama hesabı yapılandırmış olmanız gerekir. Herhangi bir raporu görüntülerken, **Eylemler** ' i ve ardından **rapor zamanla**' yı seçin. Rapor adı ve ekleyin ya da sonra bir kendi URL ya da otomatik olarak oluşturulan URL'yi kullanın. **Depolama alanına kaydet** ' i seçin ve ardından Depolama hesabını seçin veya yeni bir tane ekleyin. Raporu dosya adına eklenmiş bir ön eki girin. Bir CSV veya JSON dosya biçimi seçin ve sonra raporu kaydedin.

### <a name="schedule-a-report"></a>Bir raporu zamanlama

Zamanlanan aralıklarda raporlar çalıştırabilir ve gönderdiğiniz bunları bir alıcı listesi veya Bulut hizmeti sağlayıcısı depolama hesabı için. Herhangi bir raporu görüntülerken, **Eylemler** ' i ve ardından **rapor zamanla**' yı seçin. Raporun e-posta ile gönderin ve bir depolama hesabına kaydedin. **Zamanlama**bölümünde aralığı (günlük, haftalık veya aylık) seçin. Haftalık ve aylık için gün veya tarih sunun ve saatini seçin'i seçin. Zamanlanmış raporu kaydedin. Excel rapor biçimini seçerseniz rapor ek olarak gönderilir. İçerik e-posta biçimini seçtiğinizde, grafik biçiminde gösterilen rapor sonuçlarını grafik olarak sunulur.

### <a name="export-a-report-as-a-csv-file"></a>Bir raporu CSV dosyası olarak dışarı aktarın.

Herhangi bir raporu görüntülerken, **Eylemler** ' i ve ardından **tüm rapor verilerini dışarı aktar**' ı seçin. Bir açılır pencere görünür ve bir CSV dosyası indirilir.

## <a name="next-steps"></a>Sonraki adımlar

- Cloudyn ' de bulunan ve [Cloudyn raporlarını kullanan](use-reports.md)raporlar hakkında bilgi edinin.
- [Panolar](dashboards.md)oluşturmak için raporların nasıl kullanılacağı hakkında bilgi edinin.
