---
title: Azure’da Cloudyn maliyet yönetimi raporlarını anlama
description: Bu makale, Cloudyn maliyet yönetimi raporlarının temel yapısını ve işlevlerini anlamanıza yardımcı olur.
author: bandersmsft
ms.author: banders
ms.date: 03/12/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.subservice: cloudyn
ms.reviewer: benshy
ms.custom: seodec18
ROBOTS: NOINDEX
ms.openlocfilehash: 9f42359cc843a934cf1258576d13eec8af2279cc
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88687663"
---
# <a name="understanding-cloudyn-cost-management-reports"></a>Cloudyn maliyet yönetimi raporlarını anlama

Bu makale, Cloudyn maliyet yönetimi raporlarının temel yapısını ve işlevlerini anlamanıza yardımcı olur. Cloudyn raporları genellikle sezgiseldir ve aynı görünüme sahiptir. Bu makaleyi okuduktan sonra tüm maliyet yönetimi raporlarını kullanabilirsiniz. Birçok standart özellik farklı raporlarda mevcuttur ve raporlarda kolayca gezinmenize olanak tanır. Raporlar özelleştirilebilir ve sonuçları hesaplayıp görüntülemek için çeşitli seçenekler arasından seçim yapabilirsiniz.

[!INCLUDE [cloudyn-note](../../../includes/cloudyn-note.md)]

## <a name="report-fields-and-options"></a>Rapor alanları ve seçenekleri

Aşağıdaki Zaman İçinde Maliyet raporuna göz atalım. Çoğu Cloudyn raporu benzer bir düzene sahiptir.

![Açıklamalara karşılık gelen numaralandırılmış alanlara sahip Zaman İçinde Maliyet raporu örneği](./media/understanding-cost-reports/sample-report.png)

Yukarıdaki görüntüde numaralandırılmış alanlar, aşağıdaki bilgilerle ayrıntılı bir şekilde açıklanmıştır:

1. **Tarih Aralığı**

    Hazır ayar veya özel değer kullanarak rapor zaman aralığı belirtmek için Tarih Aralığı listesini kullanın.
2. **Kayıtlı Filtre**

    Rapora uygulanan geçerli grupları ve filtreleri kaydetmek için Kayıtlı Filtre listesini kullanın. Kayıtlı filtreler, aşağıdakiler dahil olmak üzere maliyet ve performans raporlarında kullanılabilir:

      - Maliyet Analizi
      - Ayırma
      - Varlık Yönetimi
      - İyileştirme

   Bir filtre adı yazın ve **Kaydet**'e tıklayın.

3. **Etiketler**

    Etiket kategorilerine göre gruplandırmak için Etiketler alanını kullanın. Menüde listelenen etiketler, Azure departman veya maliyet merkezi etiketleri ya da Cloudyn maliyet varlığı ve abonelik etiketleridir. Sonuçları filtrelemek için farklı etiketler seçin. Sonuçları filtrelemek için etiket adı da (anahtar sözcük) yazabilirsiniz.

    ![Sonuçları filtrelemek için kullanılabilecek etiket listesi örneği](./media/understanding-cost-reports/select-options.png)

    Yeni bir filtre eklemek için **Ekle**'ye tıklayın.

    ![Seçenekleri ve filtreleme koşullarını gösteren filtre ekleme kutusu](./media/understanding-cost-reports/add-filter.png)

    Etiket gruplama veya filtreleme, Azure kaynakları veya kaynak grubu etiketleriyle bağlantılı değildir.

    Maliyet ayırma etiketi gruplama ve filtreleme işlemleri **Gruplar** menü seçeneğinden gerçekleştirilebilir.

4. **Raporlardaki gruplar**

    Maliyet Analizi raporlarındaki grupları kullanarak faturalama verilerinizdeki standart, madde işaretli kategorilerin raporunuzda gösterilmesini sağlayabilirsiniz.  Ancak Maliyet Ayırma raporlarındaki gruplar, etiket tabanlı kategorileri görüntüler. Etiket tabanlı kategoriler, maliyet ayırma modelinde ve faturalama verilerindeki standart madde işaretli kategorilerde tanımlanır.

    ![Gruplama yapabileceğiniz ilk etiket listesi örneği](./media/understanding-cost-reports/groups-tags01.png)

    ![Gruplama yapabileceğiniz ikinci etiket listesi örneği](./media/understanding-cost-reports/groups-tags02.png)

    Maliyet Ayırma Raporlarında, etiket tabanlı grup kategorilerindeki gruplar şunları içerebilir:
      - Etiketler
      - kaynak grubu etiketleri
      - Cloudyn maliyet varlığı etiketleri
      - Maliyet ayırma için abonelik etiketi kategorileri

   Örnekler şunları içerebilir:
   - Maliyet merkezi
   - Bölüm
   - Uygulama
   - Ortam
   - Maliyet kodu

     Raporlarda bulunan yerleşik grupların listesi aşağıda verilmiştir:

     - **Maliyet Türü**
     - Bir veya daha fazla maliyet türü seçin ya da hepsini birden seçin. Maliyet türleri şunlardır:
       - Tek Seferlik Ücret
       - Destek
       - Kullanım Maliyeti
     - **Müşteri**
       - Bir veya daha fazla müşteri seçin ya da tüm müşterileri birden seçin.
     - **Hesap Adı**
       - Hesap veya abonelik adı. Azure'da Azure aboneliğinin adıdır.
     - **Hesap Numarası**
       - Bir veya daha fazla hesap seçin ya da tüm hesapları birden seçin. Azure'da bu Azure aboneliğinin GUID değeridir.
     - **Üst Hesap**
       - Üst hesabı, birden fazla hesabı veya tümünü seçin.
     - **Hizmet**
       - Bir veya daha fazla hizmet seçin ya da tüm hizmetleri birden seçin.
     - **Sağlayıcı**
       - Varlıkların ve giderlerin ilişkilendirildiği bulut sağlayıcısı.
     - **Bölge**
       - Kaynağın barındırıldığı bölge.
     - **Kullanılabilirlik Alanı**
       - Bir bölge içindeki yalıtılmış AWS konumları.
     - **Kaynak Türü**
       - Kullanılan kaynağın türü.
     - **Alt Tür**
       - Alt türü seçin.
     - **İşlem**
       - İşlemi veya **Tümünü göster**'i seçin.
     - **Fiyat Modeli**
       - Tamamı Peşin
       - Peşin Ödeme Yok
       - Kısmi Peşin
       - İsteğe Bağlı
       - Ayırma
       - Spot
     - **Ücret Türü**
       - Negatif veya Pozitif ücret türünü ya da ikisini birden seçin.
     - **Kiracı**
       - Makinenin ayrılmış makine olarak çalışıp çalışmadığını belirtir.
     - **Kullanım Türü**
       - Kullanım türü, tek seferlik ücretler veya yinelenen ücretler olabilir.

5. **Filtreler**

    Aralıkları seçili değerlere göre ayarlamak için tekli veya çoklu seçim filtrelerini kullanın. Filtre ayarlamak için **Ekle**'ye tıklayın ve filtre kategorileri ile değerlerini seçin.

6. **Maliyet Modeli**

    360 Maliyet Dağıtma ile oluşturduğunuz maliyet modelini seçmek için Maliyet Modeli'ni kullanın. Maliyet ayırma gereksinimlerinize bağlı olarak birden çok Cloudyn maliyet modeliniz olabilir. Bazı kuruluş ekipleriniz diğerlerinden farklı maliyet ayırma gereksinimlerine sahip olabilir. Her takımın kendine özel maliyet modeli olabilir.

    Maliyet ayırma modeli tanımı oluşturma hakkında daha fazla bilgi için bkz. [Maliyet ayırmak için özel etiketler kullanma](tutorial-manage-costs.md#use-custom-tags-to-allocate-costs).

7. **Amorti etme**

    Kullanıma dayalı olmayan hizmet ücretlerini veya tek seferlik maliyetleri görüntülemek ve bu maliyetleri ait oldukları zamana yaymak için Maliyet Yönetimi raporlarında amorti etmeyi kullanın. Tek seferlik ücretler şunları içerebilir:
    - Yıllık destek ücretleri
    - Yıllık güvenlik bileşeni ücretleri
    - Ayrılmış örnek satın alma ücretleri
    - Bazı Azure Market öğeleri.

   Amorti etme altında **Amorti edilmiş maliyet** veya **Gerçek maliyet**'i seçin.

8. **Çözünürlük**

    Seçili tarih aralığı içinde saat çözünürlüğünü seçmek için Çözünürlük ayarını kullanın. Zaman çözünürlüğünüz birimlerin raporda nasıl görüntülendiğini belirler ve şunlardan biri olabilir:
    - Günlük
    - Haftalık
    - Aylık
    - Üç aylık
    - Yıllık

9. **Ayırma kuralları**

    Maliyet ayırma maliyeti yeniden hesaplamayı uygulamak veya devre dışı bırakmak için ayırma kurallarını kullanın. Faturalandırma verileri için maliyet ayırmayı yeniden hesaplamayı etkinleştirebilir veya devre dışı bırakabilirsiniz. Yeniden hesaplama, rapordaki seçili kategoriler için geçerlidir. Ham faturalandırma verilerine göre maliyet ayırmayı yeniden hesaplama etkisini değerlendirmenize olanak tanır.

10. **Kategorilere ayrılmamış**

    Kategorilere ayrılmamış maliyetleri rapora dahil etmek veya hariç tutmak için kategorilere ayrılmamış seçeneğini kullanın.

11. **Alanları göster/gizle**

    Alanları göster/gizle seçeneğinin raporlar üzerinde bir etkisi yoktur.

12. **Görüntüleme biçimleri**

    Çeşitli grafik veya tablo görünümlerini seçmek için görüntü biçimlerini kullanın.

    ![Seçebileceğiniz görüntüleme biçimlerinin sembolleri](./media/understanding-cost-reports/display-formats.png)

13. **Çok renkli**

    Raporunuzdaki grafiklerin rengini ayarlamak için çoklu renkli seçeneğini kullanın.

14. **Eylemler**

    Raporu kaydetmek, dışarı aktarmak veya zamanlamak için Eylemler'i kullanın.

15. **İlke**

    Görünmese de bazı raporlar bir tahmini maliyet hesaplama ilkesi içerir. Bu raporlarda **Birleştirilmiş** ilkesi, Microsoft kaydı veya AWS yükümlüsü gibi geçerli varlık kapsamındaki tüm hesaplar ve abonelikler için öneriler gösterir. **Tek başına** ilkesi, başka bir abonelik yokmuş gibi bir hesap veya abonelik için önerileri gösterir. Seçtiğiniz ilke, kuruluşunuz tarafından kullanılan iyileştirme stratejisine göre değişir. Maliyet tahminleri, son 30 günlük kullanıma dayanır.

## <a name="save-and-schedule-reports"></a>Raporları kaydetme ve zamanlama

Oluşturduğunuz raporları daha sonra kullanmak üzere kaydedebilirsiniz. Kaydedilen raporlar **Araçlarım** > **Raporlarım** sayfasına eklenir. Var olan bir raporda değişiklik yaparsanız kaydettiğinizde rapor yeni bir sürüm olarak kaydedilir. Dilerseniz yeni bir rapor olarak da kaydedebilirsiniz.

### <a name="save-a-report-to-the-cloudyn-portal"></a>Raporu Cloudyn portalına kaydetme

Herhangi bir raporu görüntülerken **Eylemler**'e tıklayın ve ardından **Raporlarıma kaydet**'i seçin. Rapora bir ad verin ve kendi URL'nizi ekleyin ya da otomatik olarak oluşturulan URL'yi kullanın. İsteğe bağlı olarak, raporu kuruluşunuzdaki diğer kişilerle genel olarak **Paylaşabilir** veya varlığınızla paylaşabilirsiniz. Raporu paylaşmazsanız kişisel bir rapor olarak kalır ve yalnızca siz görüntüleyebilirsiniz. Raporu kaydedin.


### <a name="save-a-report-to-cloud-provider-storage"></a>Raporu bulut sağlayıcısı depolama alanına kaydetme

Bir raporu bulut hizmeti sağlayıcınıza kaydetmek için depolama hesabı yapılandırmış olmanız gerekir. Herhangi bir raporu görüntülerken **Eylemler**'e tıklayın ve ardından **Raporu zamanla**'yı seçin. Rapora bir ad verin ve kendi URL'nizi ekleyin ya da otomatik olarak oluşturulan URL'yi kullanın. **Depolama alanına kaydet**'i seçip bir depolama hesabı seçin veya yeni bir tane ekleyin. Rapor dosya adına eklenen bir ön ek girin. CSV veya JSON dosya biçimini seçin ve raporu kaydedin.

### <a name="schedule-a-report"></a>Raporu zamanlama

Raporların belirli zamanlarda çalıştırılmasını ve alıcı listesine veya bulut hizmeti sağlayıcısı depolama hesabına gönderilmesini sağlayabilirsiniz. Herhangi bir raporu görüntülerken **Eylemler**'e tıklayın ve ardından **Raporu zamanla**'yı seçin. Raporu e-posta ile gönderebilir ve bir depolama hesabına kaydedebilirsiniz. **Zamanla** bölümünde zaman aralığını (günlük, haftalık veya aylık) seçin. Haftalık ve aylık için teslim edilecek günleri veya tarihleri seçip saati belirleyin. Zamanlanmış raporu kaydedin. Excel rapor biçimini seçerseniz rapor ek olarak gönderilir. E-posta içeriği biçimini seçtiğinizde grafik biçiminde görüntülenen rapor sonuçları grafik olarak gönderilir.

### <a name="export-a-report-as-a-csv-file"></a>Raporu CSV dosyası olarak dışarı aktarma

Herhangi bir raporu görüntülerken **Eylemler**'e tıklayın ve ardından **Tüm rapor verilerini dışarı aktar**'ı seçin. Bir pencere açılır ve CSV dosyası indirilir.

## <a name="next-steps"></a>Sonraki adımlar

- Cloudyn'deki raporlar hakkında bilgi edinmek için bkz. [Cloudyn raporlarını kullanma](../../cost-management/use-reports.md).
- Raporları kullanarak [pano](../../cost-management/dashboards.md) oluşturmayı öğrenin.
