---
title: Kullanım dosyası özet tabloları ile Azure EA faturalama sorunlarını giderme
description: Bu makale, CSV kullanım dosyalarınızdan oluşturulan özet tablolarını kullanarak Kurumsal Anlaşma (EA) faturalama sorunlarını gidermenize yardımcı olur.
author: banders
ms.reviewer: isvargas
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: troubleshooting
ms.date: 10/09/2020
ms.author: banders
ms.openlocfilehash: a30f85ae1b65321a25ee93239374d2a8aae26769
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92026682"
---
# <a name="troubleshoot-ea-billing-issues-with-usage-file-pivot-tables"></a>Kullanım dosyası özet tabloları ile EA faturalama sorunlarını giderme

Bu makale, kullanım dosyalarınızdaki özet tablolarını kullanarak EA faturalama sorunlarını gidermenize yardımcı olur. Azure kullanım dosyaları, Azure kullanımınız ve tüketiminizle tüm bilgileri içerir. Dosyadaki bilgiler şunları anlamanıza yardımcı olabilir:

- Azure rezervasyonlarının nasıl kullanıldığını ve uygulandığını anlama
- Azure Maliyet Yönetimi’ndeki bilgileri kesilen faturanızla karşılaştırma
- Ani maliyet artışı sorunlarını giderme
- Hizmet düzeyi sözleşmesi için para iadesi tutarını hesaplama

Kullanım dosyalarınızdaki bilgilerden faydalanarak, kullanım sorunlarını daha iyi anlayıp tanılayabilirsiniz. Kullanım dosyaları, virgülle ayrılmış (CSV) biçimde oluşturulur. Kullanım bilgilerinin yer aldığı CSV dosyalarının boyutu büyük olabileceğinden, Excel gibi bir elektronik tablo uygulamasında özet tablolar olarak yönetilip görüntülenmeleri daha kolaydır. Bu makaledeki örneklerde Excel tercih edilmiştir, ancak siz dilediğiniz elektronik tablo uygulamasını kullanabilirsiniz.

Yalnızca EA yöneticileri, Hesap Sahipleri ve Departman Yöneticileri kullanım dosyalarını indirebilir.

## <a name="get-the-data-and-format-it"></a>Verileri alma ve biçimlendirme

Azure kullanım dosyaları CSV biçiminde olduğundan, verileri Excel’de kullanılacak şekilde hazırlamanız gerekir. Verileri tablo olarak biçimlendirmek için aşağıdaki adımlardan yararlanın.

1. [EA müşterileri için kullanımı indirme](https://docs.microsoft.com/azure/cost-management-billing/manage/download-azure-invoice-daily-usage-date#download-usage-for-ea-customers) sayfasındaki yönergeleri kullanarak Tüm Ücretler (kullanım ve satın almalar) dahil Kullanım Ayrıntıları Sürüm 2’yi İndirin.
1. Dosyayı Excel'de açın.
1. Biçimlendirilmemiş veriler aşağıdaki örneğe benzer.  
    :::image type="content" source="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/raw-csv-data-ea.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/raw-csv-data-ea.png" :::
1. İlk sütun başlığı **BillingAccountID**’yi içeren, tablodaki ilk alanı seçin.
1. Tablodaki tüm bilgileri seçmek için Ctrl + Shift + Aşağı Ok tuşuna ve sonra Ctrl + Shift + Sağ Ok tuşuna basın.
1. Üst menüden **Ekle** > **Tablo**’yu seçin. Tablo oluştur kutusunda **Tablomda üst bilgiler var** seçeneğini belirleyin ve sonra **Tamam**’ı seçin.  
    :::image type="content" source="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/create-table-dialog.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" :::
1. Üst menüden **Ekle** > **PivotTable** seçeneğini belirleyin ve sonra **Tamam**’ı seçin. Bu eylem, dosyada yeni bir sayfa oluşturur. Sizi sayfanın sağ tarafındaki özet tablosu alanına götürür.  
    :::image type="content" source="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/pivot-table-fields.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/pivot-table-fields.png" :::

PivotTable Alanları, sürükle bırak yöntemiyle işleyen bir bölümdür. Özet tablosu oluşturmak için sonraki bölüme geçin.

## <a name="create-pivot-table-to-view-azure-costs-by-resources"></a>Kaynaklara göre Azure maliyetlerini görüntülemek için özet tablosu oluşturma

Bu bölümde, genel Azure kullanımındaki sorunları giderebileceğiniz bir özet tablosu oluşturacaksınız. Örnek tablo, en çok kaynak kullanan hizmeti belirlemenize yardımcı olabilir. Dilerseniz en çok maliyeti yansıtan kaynakları görüntüleyebilir ve bir hizmetin nasıl ücretlendirildiğine bakabilirsiniz.

1. PivotTable Alanları kısmında **Ölçüm Kategorisi** ve **Ürün** öğelerini **Satırlar** bölümüne sürükleyin. **Ürün** öğesini **Ölçüm Kategorisi** öğesinin altına ekleyin.  
    :::image type="content" source="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/rows-section.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/rows-section.png" :::
1. Daha sonra **Maliyet** sütununu **Değerler** bölümüne ekleyin. Tüketim birimleri ve işlemleri hakkındaki bilgileri almak için dilerseniz Tüketilen Miktar sütununu da kullanabilirsiniz. Örneğin, GB veya Saat türünde görüntüleyebilirsiniz. USD, EUR ve INR gibi farklı para birimlerine göre maliyetler yerine yapılan işlemleri seçebilirsiniz.  
    :::image type="content" source="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/add-pivot-table-fields.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/add-pivot-table-fields.png" :::
1. Artık genelleştirilmiş tüketim araştırmasına yönelik bir panonuz var. Özet tablosundaki filtreleme seçeneklerini kullanarak belirli bir hizmeti filtreleyebilirsiniz.  
    :::image type="content" source="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/pivot-table-filter-option-row-label.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/pivot-table-filter-option-row-label.png" :::
    Özet tablosundaki ikinci bir düzeyi (örneğin kaynak) filtrelemek için tablodaki ikinci düzey bir öğeyi seçin.  
    :::image type="content" source="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/pivot-table-filter-option-select-field.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/pivot-table-filter-option-select-field.png" :::
1. Her hizmetin kaynağa göre maliyetini görmek için **ResourceID** sütununu **Ürün** kategorisi altındaki **Satırlar** alanına sürükleyin. Ayrıntılı fiyatlandırma bilgilerini görüntülemek için kuruluşunuzun UnitPrice değerine bakın ve fiyat listesinin ilk sütununda **Ürün** araması yapın.
1. Ürünün günlük tüketimini görmek için **Tarih** sütununu **Sütunlar** alanına ekleyin.  
    :::image type="content" source="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/pivot-table-date.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/pivot-table-date.png" :::
1. Her ayın sütunu için ayları **+** simgeleriyle genişletip daraltın.  
    :::image type="content" source="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/pivot-table-month-expand-collapse.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/pivot-table-month-expand-collapse.png" :::  
    **Değerler** alanına hem **Maliyet** hem de **Miktar** sütunlarını eklemek isteğe bağlıdır. Bunu yaptığınızda, Tarih sütunu özet tablosunun Sütunlar bölümünde olduğunda her ay ve günün altındaki her veri bölümü için iki sütun oluşturulur.
1. Ek filtreler için SubscriptionID, Departman, ResourceGroup, Etiketler veya Maliyet Merkezi sütunlarını **Filtreler** alanına ekleyebilir ve istediğiniz öğeyi seçebilirsiniz.

## <a name="create-pivot-table-to-view-cost-for-a-specific-resource"></a>Belirli bir kaynağın maliyetlerini görüntülemek için özet tablosu oluşturma

Tek bir kaynak, farklı hizmetler için birden fazla ücret yansıtabilir. Örneğin, bir sanal makinenin yansıttığı ücretler arasında Hesaplama maliyetleri, işletim sistemi lisanslaması, Bant Genişliği (Veri aktarımları), RI kullanımı ve anlık görüntülere yönelik depolama alanı olabilir. Belirli kaynaklara yönelik genel kullanımı gözden geçirmek istediğinizde, kullanım dosyalarıyla birlikte genel kullanımınızı görüntülemeye yönelik bir pano oluşturmak için aşağıdaki adımları izleyebilirsiniz.

1. Sağdaki menüde, **ResourceID** öğesini özet tablosu menüsündeki **Filtre** bölümüne sürükleyin.
1. Maliyetini görmek istediğiniz kaynağı seçin. Kaynak adını bulmak için **Arama** kutusuna yazın.
1. **Ölçüm Kategorisi** ve **Ürün** öğelerini Satırlar bölümüne ekleyin. **Ürün** öğesini **Ölçüm Kategorisi** öğesinin altına ekleyin.  
    :::image type="content" source="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/pivot-table-fields-meter-category.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" lightbox="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/pivot-table-fields-meter-category.png" :::
1. Daha sonra **Maliyet** sütununu **Değerler** bölümüne ekleyin. Tüketim birimleri ve işlemleri hakkındaki bilgileri almak için dilerseniz Tüketilen Miktar sütununu da kullanabilirsiniz. Örneğin, GB veya Saat türünde görüntüleyebilirsiniz. USD, EUR ve INR gibi farklı para birimlerine göre maliyetler yerine yapılan işlemleri seçebilirsiniz. Artık kaynağın tükettiği tüm hizmetleri gösteren bir panonuz var.
1. **Tarih** sütununu **Sütunlar** bölümüne ekleyin. Günlük tüketimi gösterir.
1. Her ayın sütunundaki **+** simgelerini kullanarak genişletip daraltabilirsiniz.  
    :::image type="content" source="./media/troubleshoot-ea-billing-issues-usage-file-pivot-tables/pivot-table-month-expand-collapse.png" alt-text="Biçimlendirilmemiş verilerin Excel’de gösterildiği örnek" :::

[!INCLUDE [Transform data before using large usage files](../../../includes/cost-management-billing-transform-data-before-using-large-usage-files.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Maliyet analiziyle maliyetleri araştırma ve analiz etme](../costs/quick-acm-cost-analysis.md).