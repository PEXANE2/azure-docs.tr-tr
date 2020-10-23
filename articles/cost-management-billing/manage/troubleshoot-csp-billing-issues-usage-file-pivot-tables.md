---
title: Kullanım dosyası özet tabloları ile Azure CSP faturalama sorunlarını giderme
description: Bu makale, CSV kullanım dosyalarınızdan oluşturulan özet tablolarını kullanarak Azure Bulut Çözümü Sağlayıcısı (CSP) faturalama sorunlarını gidermenize yardımcı olur.
author: banders
ms.reviewer: isvargas
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: troubleshooting
ms.date: 10/09/2020
ms.author: banders
ms.openlocfilehash: 6525d809805da3a19a0efe423306f18d8e67a646
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92026871"
---
# <a name="troubleshoot-csp-billing-issues-with-usage-file-pivot-tables"></a>Kullanım dosyası özet tabloları ile CSP faturalama sorunlarını giderme

Bu makale, İş Ortağı Merkezi mutabakat (kullanım) dosyalarınızda özet tabloları kullanarak Bulut Çözümü Sağlayıcısı (CSP) faturalama sorunlarını gidermenize yardımcı olur. Azure kullanım dosyaları, Azure kullanımınız ve tüketiminizle tüm bilgileri içerir. Dosyadaki bilgiler şunları anlamanıza yardımcı olabilir:

- Azure rezervasyonlarının nasıl kullanıldığını ve uygulandığını anlama
- Azure Maliyet Yönetimi’ndeki bilgileri kesilen faturanızla karşılaştırma
- Ani maliyet artışı sorunlarını giderme
- Hizmet düzeyi sözleşmesi için para iadesi tutarını hesaplama

Kullanım dosyalarınızdaki bilgilerden faydalanarak, kullanım sorunlarını daha iyi anlayıp tanılayabilirsiniz. Kullanım dosyaları, virgülle ayrılmış (CSV) biçimde oluşturulur. Kullanım bilgilerinin yer aldığı CSV dosyalarının boyutu büyük olabileceğinden, Excel gibi bir elektronik tablo uygulamasında özet tablolar olarak yönetilip görüntülenmeleri daha kolaydır. Bu makaledeki örneklerde Excel tercih edilmiştir, ancak siz dilediğiniz elektronik tablo uygulamasını kullanabilirsiniz.

Mutabakat dosyalarını yalnızca Faturalama yöneticileri ve Genel yöneticiler indirebilir. Daha fazla bilgi için bkz. [İş Ortağı Merkezi mutabakat dosyalarınızdaki satır öğelerini okumayı öğrenme](/partner-center/use-the-reconciliation-files).

## <a name="get-the-data-and-format-it"></a>Verileri alma ve biçimlendirme

Azure kullanım dosyaları CSV biçiminde olduğundan, verileri Excel’de kullanılacak şekilde hazırlamanız gerekir. Verileri tablo olarak biçimlendirmek için aşağıdaki adımlardan yararlanın.

1. [Faturanızı bulma](/partner-center/read-your-bill#find-your-bill) sayfasındaki yönergeleri izleyerek kullanım dosyasını indirin.
1. Dosyayı Excel'de açın.
1. Biçimlendirilmemiş veriler aşağıdaki örneğe benzer.  
    :::image type="content" source="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/raw-csv-data.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/raw-csv-data.png" :::
1. Tablodaki ilk alan olan **PartnerID**’yi seçin.
1. Tablodaki tüm bilgileri seçmek için Ctrl + Shift + Aşağı Ok tuşuna ve sonra Ctrl + Shift + Sağ Ok tuşuna basın.
1. Üst menüden **Ekle** > **Tablo**’yu seçin. Tablo oluştur kutusunda **Tablomda üst bilgiler var** seçeneğini belirleyin ve sonra **Tamam**’ı seçin.  
    :::image type="content" source="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/create-table-dialog.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" :::
1. Üst menüden **Ekle** > **PivotTable** seçeneğini belirleyin ve sonra **Tamam**’ı seçin. Bu eylem, dosyada yeni bir sayfa oluşturur ve sizi sayfanın sağ tarafındaki özet tablosu alanına götürür.  
    :::image type="content" source="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/pivot-table-fields.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/pivot-table-fields.png" :::

PivotTable Alanları, sürükle bırak yöntemiyle işleyen bir bölümdür. Özet tablosu oluşturmak için sonraki bölüme geçin.

## <a name="create-pivot-table-to-view-azure-costs-by-resources"></a>Kaynaklara göre Azure maliyetlerini görüntülemek için özet tablosu oluşturma

Bu bölümde, genel Azure kullanımındaki sorunları giderebileceğiniz bir özet tablosu oluşturacaksınız. Örnek tablo, en çok kaynak kullanan hizmeti belirlemenize yardımcı olabilir. Dilerseniz en çok maliyet yansıtan kaynakları görüntüleyebilir ve bir hizmetin nasıl ücretlendirildiğine bakabilirsiniz.

1. PivotTable Alanları kısmında **Hizmet Adı** ve **Kaynak** öğelerini **Satırlar** alanına sürükleyin. **Kaynak** öğesini **Hizmet Adı** öğesinin altına yerleştirin.  
    :::image type="content" source="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/rows-section.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/rows-section.png" :::
1. Daha sonra **Vergi Sonrası Toplamı** öğesini **Değerler** alanına ekleyin. Tüketim birimleri ve işlemleri hakkındaki bilgileri almak için dilerseniz Tüketilen Miktar sütununu da kullanabilirsiniz. Örneğin, GB veya Saat türünde görüntüleyebilirsiniz. USD, EUR ve INR gibi farklı para birimlerine göre maliyetler yerine yapılan işlemleri seçebilirsiniz.  
    :::image type="content" source="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/add-pivot-table-fields.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/add-pivot-table-fields.png" :::
1. Artık genelleştirilmiş tüketim araştırmasına yönelik bir panonuz var. Özet tablosundaki filtreleme seçeneklerini kullanarak belirli bir hizmeti filtreleyebilirsiniz.  
    :::image type="content" source="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/pivot-table-filter-option-row-label.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/pivot-table-filter-option-row-label.png" :::
    Özet tablosundaki ikinci bir düzeyi (örneğin kaynak) filtrelemek için tablodaki ikinci düzey bir öğeyi seçin.
    :::image type="content" source="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/pivot-table-filter-option-select-field.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/pivot-table-filter-option-select-field.png" :::
1. Ek filtreler için **Filtreler** alanına **SubscriptionID** ve **Müşterinin Şirket Adı** öğelerini ekleyebilir ve dilediğiniz kapsamı seçebilirsiniz.

## <a name="create-a-pivot-table-to-view-azure-usage-by-date"></a>Tarihe göre Azure kullanımını görüntülemek için özet tablosu oluşturma

Bu bölümde, Tüketilen Miktar ve tarihe göre genel Azure kullanımındaki sorunları giderebileceğiniz bir özet tablosu oluşturacaksınız. Faturalama artışlarını tarihe ve hizmete göre belirlemek yararlıdır. Dilerseniz en çok maliyet yansıtan kaynakları görüntüleyebilir ve bir hizmetin nasıl ücretlendirildiğine bakabilirsiniz.

Mutabakat dosyanızda iki tablo vardır. Bunlardan biri üstteki tablo (ana tablo), diğeri ise belgenin altındaki başka bir tablodur. İkinci tabloda çoğunlukla aynı bilgiler vardır, ancak fiyatlandırma veya maliyet ayrıntıları yer almaz. Bu tabloda, kullanım tarihi ve tüketim miktarı gibi öğeler yer alır.

:::image type="content" source="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/reconciliation-file-two-tables.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/reconciliation-file-two-tables.png" :::

1. Mutabakat dosyasının altındaki bilgileri içeren bir Excel tablosu oluşturmak için [Verileri alma ve biçimlendirme](#get-the-data-and-format-it) bölümündeki adımların aynılarını uygulayın.
1. Tablo hazır olduğunda ve bir özet tablo sayfanız olduğunda, Kaynaklara göre Azure maliyetlerini görüntülemek için özet tablosu oluşturma bölümündeki adımları uygulayarak panoyu hazırlayın. Vergi Sonrası toplamını kullanmak yerine, **Değerler** alanına **Tüketilen miktar** öğesini ekleyin.
1. Sütunlar bölümüne **Kullanım Tarihi** öğesini ekleyin. Özet tablosu, aşağıdaki örnekteki gibi görünmelidir.  
    :::image type="content" source="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/final-pivot-table-fields.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/final-pivot-table-fields.png" :::
1. Şimdi tarihe göre kullanımı gösteren bir pano elde ettiniz. **+** simgesini seçerek her ayı genişletebilirsiniz.

Panoda tüketilen miktar GB, Saat ve Aktarım gibi birimler cinsinden gösterilir.

Günlük fiyatı görüntülemek için **Satırlar** alanına **Kaynak GUID** öğesini ekleyebilirsiniz. Bir üstteki tabloda, kaynağa birim fiyatını ( **ListPrice** ) ekleyin. Vergi öncesi ücretlerinizi hesaplamak için **ListPrice** öğesini **Tüketilen miktar** ile çarpın. Miktarların eşleşmesi gerekir.

Bazı kaynaklar (hizmetler) tüketilen miktara göre ölçeklendirilen fiyatlandırmaya sahiptir. Örneğin, bazı kaynakların tüketilen ilk 100 GB için daha yüksek, sonradan kullanılan GB için daha düşük fiyatı vardır. Maliyetleri el ile hesaplarken, ölçeklendirilen fiyatlandırmayı göz önünde bulundurun.

## <a name="create-pivot-table-to-view-cost-for-a-specific-resource"></a>Belirli bir kaynağın maliyetlerini görüntülemek için özet tablosu oluşturma

Tek bir kaynak, farklı hizmetler için birden fazla ücret yansıtabilir. Örneğin, bir sanal makinenin yansıttığı ücretler arasında Hesaplama maliyetleri, işletim sistemi lisanslaması, Bant Genişliği (Veri aktarımları), RI kullanımı ve anlık görüntülere yönelik depolama alanı olabilir. Belirli kaynaklara yönelik genel kullanımı gözden geçirmek istediğinizde, kullanım dosyalarıyla birlikte genel kullanımınızı görüntülemeye yönelik bir pano oluşturmak için aşağıdaki adımları izleyebilirsiniz.

Mutabakat dosyalarında kaynağa özgü ayrıntılar yer almaz. Dolayısıyla toplu kullanım dosyalarından yararlanırsınız. Aboneliğinizin toplu kullanım dosyasını sağlaması için [Azure Faturalama desteğine](https://go.microsoft.com/fwlink/?linkid=2083458) ulaşın. Toplu dosyalar abonelik düzeyinde oluşturulur. Biçimlendirilmemiş veriler aşağıdaki örneğe benzer.

:::image type="content" source="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/raw-aggregated-usage-file.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/raw-aggregated-usage-file.png" :::

Dosya aşağıdaki sütunları içerir.

- **UsageStart** ve **UsageEnd**: Her satır öğesinin (her kullanım birimi) tarihi. Örneğin, günlük.
- **MeteredResourceID**: Azure’da ölçüm kimliğine karşılık gelir.
- **Özellikler**: Konum gibi diğer ayrıntılarla birlikte Örnek Kimliğini (kaynak adını) içerir.
- **Miktar**: Mutabakat dosyasındaki tüketilen miktar.

1. Tablodaki ilk alan olan **PartnerID**’yi seçin.  
1. Tablodaki tüm bilgileri seçmek için Ctrl + Shift + Aşağı Ok tuşuna ve sonra Ctrl + Shift + Sağ Ok tuşuna basın.
1. Üst menüden **Ekle** > **Tablo**’yu seçin. Tablo oluştur kutusunda **Tablomda üst bilgiler var** seçeneğini belirleyin ve sonra **Tamam**’ı seçin.  
    :::image type="content" source="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/create-table-dialog.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" :::
1. Üst menüden **Ekle** > **PivotTable** seçeneğini belirleyin ve sonra **Tamam**’ı seçin. Bu eylem, dosyada yeni bir sayfa oluşturur ve sizi sayfanın sağ tarafındaki özet tablosu alanına götürür.  
    :::image type="content" source="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/pivot-table-fields-reconciliation.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/pivot-table-fields-reconciliation.png" :::
1. Daha sonra, **Satırlar** alanına **MeteredResourceID** öğesini, **Değerler** alanına **Miktar** öğesini ekleyin. Sonuçlarda genel kullanım bilgileri gösterilir. Ek ayrıntılar için **UsageEndDateTime** öğesini **Sütunlar** alanına ekleyin.  
    :::image type="content" source="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/overall-usage.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-csp-billing-issues-usage-file-pivot-tables/overall-usage.png" :::
1. Genel bir raporu görüntülemek için **Özellikler** öğesini **MeteredResourceID** bölümündeki **Satırlar** alanına ekleyin. Kullanımınıza yönelik eksiksiz bir pano görüntülenir.
1. Belirli bir kaynağa göre filtrelemek için **Filtreler** alanına **Özellikler** öğesini ekleyin ve dilediğiniz kullanımı seçin. Kaynak adını bulmak için Arama’yı kullanabilirsiniz.
    Kaynağın maliyetini görüntülemek için toplam tüketilen miktarı bulun ve değeri liste fiyatıyla çarpın. Her Kaynak GUID (MeteredResourceID) için özel bir liste fiyatı vardır. Bir kaynak birden çok MeteredResourceID kullanıyorsa her bir kimlik için toplam değeri dikkate almanız gerekir.

[!INCLUDE [Transform data before using large usage files](../../../includes/cost-management-billing-transform-data-before-using-large-usage-files.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [İş ortakları için Azure Maliyet Yönetimi’ni kullanmaya başlama](../costs/get-started-partners.md).