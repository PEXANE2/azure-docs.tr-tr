---
title: Kullanım dosyası özet tabloları ile Azure MCA faturalama sorunlarını giderme
description: Bu makale, CSV kullanım dosyalarınızdan oluşturulan özet tablolarını kullanarak Microsoft Müşteri Sözleşmesi (MCA) faturalama sorunlarını gidermenize yardımcı olur.
author: banders
ms.reviewer: isvargas
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: troubleshooting
ms.date: 10/09/2020
ms.author: banders
ms.openlocfilehash: 2ca4affaeae7f19cf3c913b5dfcf89a04e5bc628
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92026799"
---
# <a name="troubleshoot-mca-billing-issues-with-usage-file-pivot-tables"></a>Kullanım dosyası özet tabloları ile MCA faturalama sorunlarını giderme

Bu makale, kullanım dosyalarınızdaki özet tablolarını kullanarak Microsoft Müşteri Sözleşmesi (MCA) faturalama sorunlarını gidermenize yardımcı olur. Azure kullanım dosyaları, Azure kullanımınız ve tüketiminizle tüm bilgileri içerir. Dosyadaki bilgiler şunları anlamanıza yardımcı olabilir:

- Azure rezervasyonlarının nasıl kullanıldığını ve uygulandığını anlama
- Azure Maliyet Yönetimi’ndeki bilgileri kesilen faturanızla karşılaştırma
- Ani maliyet artışı sorunlarını giderme
- Hizmet düzeyi sözleşmesi için para iadesi tutarını hesaplama

Kullanım dosyalarınızdaki bilgilerden faydalanarak, kullanım sorunlarını daha iyi anlayıp tanılayabilirsiniz. Kullanım dosyaları, virgülle ayrılmış (CSV) biçimde oluşturulur. Kullanım bilgilerinin yer aldığı CSV dosyalarının boyutu büyük olabileceğinden, Excel gibi bir elektronik tablo uygulamasında özet tablolar olarak yönetilip görüntülenmeleri daha kolaydır. Bu makaledeki örneklerde Excel tercih edilmiştir, ancak siz dilediğiniz elektronik tablo uygulamasını kullanabilirsiniz.

Yalnızca Faturalama profili sahipleri, Katkıda Bulunanlar, Okuyucular veya Fatura Yöneticileri kullanım dosyalarını indirebilir. Daha fazla bilgi için bkz. [Microsoft Müşteri Sözleşmeniz için kullanımı indirme](https://docs.microsoft.com/azure/cost-management-billing/manage/download-azure-invoice-daily-usage-date#download-usage-for-your-microsoft-customer-agreement). 

## <a name="get-the-data-and-format-it"></a>Verileri alma ve biçimlendirme

Azure kullanım dosyaları CSV biçiminde olduğundan, verileri Excel’de kullanılacak şekilde hazırlamanız gerekir. Verileri tablo olarak biçimlendirmek için aşağıdaki adımlardan yararlanın.

1. [Azure portalda kullanımı indirme](https://docs.microsoft.com/azure/cost-management-billing/manage/download-azure-invoice-daily-usage-date#download-usage-in-azure-portal) sayfasındaki yönergeleri izleyerek kullanım dosyasını indirin.
1. Dosyayı Excel'de açın.
1. Biçimlendirilmemiş veriler aşağıdaki örneğe benzer.  
    :::image type="content" source="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/raw-csv-data-mca.png" alt-text="Biçimlendirilmemiş verilerin gösterildiği örnek" lightbox="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/raw-csv-data-mca.png" :::
1. Tablodaki ilk alan olan **invoiceID**’yi seçin.
1. Tablodaki tüm bilgileri seçmek için Ctrl + Shift + Aşağı Ok tuşuna ve sonra Ctrl + Shift + Sağ Ok tuşuna basın.
1. Üst menüden **Ekle** > **Tablo**’yu seçin. Tablo oluştur kutusunda **Tablomda üst bilgiler var** seçeneğini belirleyin ve sonra **Tamam**’ı seçin.  
:::image type="content" source="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/create-table-dialog.png" alt-text="Biçimlendirilmemiş verilerin gösterildiği örnek" :::
1. Üst menüden **Ekle** > **PivotTable** seçeneğini belirleyin ve sonra **Tamam**’ı seçin. Bu eylem, dosyada yeni bir sayfa oluşturur ve sizi sayfanın sağ tarafındaki özet tablosu alanına götürür.  
    :::image type="content" source="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/pivot-table-fields.png" alt-text="Biçimlendirilmemiş verilerin gösterildiği örnek" lightbox="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/pivot-table-fields.png" :::

PivotTable Alanları, sürükle bırak yöntemiyle işleyen bir bölümdür. Özet tablosu oluşturmak için sonraki bölüme geçin.

## <a name="create-pivot-table-to-view-azure-costs-by-resources"></a>Kaynaklara göre Azure maliyetlerini görüntülemek için özet tablosu oluşturma

Bu bölümde, genel Azure kullanımındaki sorunları giderebileceğiniz bir özet tablosu oluşturacaksınız. Örnek tablo, en çok kaynak kullanan hizmeti belirlemenize yardımcı olabilir. Dilerseniz en çok maliyeti yansıtan kaynakları görüntüleyebilir ve bir hizmetin nasıl ücretlendirildiğine bakabilirsiniz.

1. PivotTable Alanları kısmında **Ölçüm Kategorisi** ve **Ürün** öğelerini **Satırlar** bölümüne sürükleyin. **Ürün** öğesini **Ölçüm Kategorisi** öğesinin altına ekleyin.  
    :::image type="content" source="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/rows-section.png" alt-text="Biçimlendirilmemiş verilerin gösterildiği örnek" lightbox="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/rows-section.png" :::
1. Daha sonra, **costInBillingCurrency** sütununu **Değerler** bölümüne ekleyin. Tüketim birimleri ve işlemleri hakkındaki bilgileri almak için dilerseniz **Miktar** sütununu da kullanabilirsiniz. Örneğin, GB veya Saat türünde görüntüleyebilirsiniz. USD, EUR ve INR gibi farklı para birimlerine göre maliyetler yerine yapılan işlemleri seçebilirsiniz.  
    :::image type="content" source="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/add-pivot-table-fields.png" alt-text="Biçimlendirilmemiş verilerin gösterildiği örnek" lightbox="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/add-pivot-table-fields.png" :::
1. Artık genelleştirilmiş tüketim araştırmasına yönelik bir panonuz var. Özet tablosundaki filtreleme seçeneklerini kullanarak belirli bir hizmeti filtreleyebilirsiniz.  
    :::image type="content" source="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/pivot-table-filter-option-row-label.png" alt-text="Biçimlendirilmemiş verilerin gösterildiği örnek" lightbox="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/pivot-table-filter-option-row-label.png" :::
    Özet tablosundaki ikinci bir düzeyi (örneğin kaynak) filtrelemek için tablodaki ikinci düzey bir öğeyi seçin.  
    :::image type="content" source="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/pivot-table-filter-option-select-field.png" alt-text="Biçimlendirilmemiş verilerin gösterildiği örnek" lightbox="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/pivot-table-filter-option-select-field.png" :::
1. Her hizmetin kaynağa göre maliyetini görmek için **ResourceID** sütununu **Ürün** kategorisi altındaki **Satırlar** alanına sürükleyin.
1. Ürünün günlük tüketimini görmek için **Tarih** sütununu **Sütunlar** alanına ekleyin.  
    :::image type="content" source="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/pivot-table-date.png" alt-text="Biçimlendirilmemiş verilerin gösterildiği örnek" lightbox="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/pivot-table-date.png" :::
1. Her ayın sütunu için ayları **+** simgeleriyle genişletip daraltın.  
    :::image type="content" source="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/pivot-table-month-expand-collapse.png" alt-text="Biçimlendirilmemiş verilerin gösterildiği örnek" lightbox="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/pivot-table-month-expand-collapse.png" :::

**Değerler** alanına hem **Maliyet** hem de **Miktar** sütunlarını eklemek isteğe bağlıdır. Bunu yaptığınızda, Tarih sütunu özet tablosunun Sütunlar bölümünde olduğunda her ay ve günün altındaki her veri bölümü için iki sütun oluşturulur.

Ek filtreler için InvoiceSection, costCenter, SubscriptionID, ResourceGroupName, veya Etiketler sütunlarını filtreler bölümüne ekleyip istediğiniz kapsamı seçebilirsiniz.

## <a name="create-pivot-table-to-view-cost-for-a-specific-resource"></a>Belirli bir kaynağın maliyetlerini görüntülemek için özet tablosu oluşturma

Tek bir kaynak, farklı hizmetler için birden fazla ücret yansıtabilir. Örneğin, bir sanal makine Hesaplama ücretleri, işletim sistemi lisanslaması, Bant Genişliği (veri aktarımları), RI kullanımı ve anlık görüntülere yönelik depolama alanını yansıtabilir. Belirli kaynaklara yönelik genel kullanımı gözden geçirmek istediğinizde, kullanım dosyalarıyla birlikte genel kullanımınızı görüntülemeye yönelik bir pano oluşturmak için aşağıdaki adımları izleyebilirsiniz.

1. Sağdaki menüde, **ResourceID** öğesini özet tablosu menüsündeki **Filtre** bölümüne sürükleyin.
1. Maliyetini görmek istediğiniz kaynağı seçin. Kaynak adını bulmak için **Arama** kutusuna yazın.  
    :::image type="content" source="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/resource-id-search.png" alt-text="Biçimlendirilmemiş verilerin gösterildiği örnek" lightbox="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/resource-id-search.png" :::
1. **meterCategory** ve **Ürün** öğelerini **Satırlar** alanına ekleyin. **Ürün** öğesini **meterCategory** öğesinin altına ekleyin.  
    :::image type="content" source="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/pivot-table-fields-meter-category.png" alt-text="Biçimlendirilmemiş verilerin gösterildiği örnek" lightbox="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/pivot-table-fields-meter-category.png" :::
1. Daha sonra, **Genişletilmiş Maliyet** sütununu **Değerler** bölümüne ekleyin. Tüketim birimleri ve işlemleri hakkındaki bilgileri almak için dilerseniz Tüketilen Miktar sütununu da kullanabilirsiniz. Örneğin, GB veya Saat türünde görüntüleyebilirsiniz. USD, EUR ve INR gibi farklı para birimlerine göre maliyetler yerine yapılan işlemleri seçebilirsiniz. Artık kaynağın tükettiği tüm hizmetleri gösteren bir panonuz var.
1. **Tarih** sütununu **Sütunlar** bölümüne ekleyin. Günlük tüketimi gösterir.
1. Her ayın sütunundaki **+** simgelerini kullanarak genişletip daraltabilirsiniz.  
    :::image type="content" source="./media/troubleshoot-customer-agreement-billing-issues-usage-file-pivot-tables/pivot-table-month-expand-collapse.png" alt-text="Biçimlendirilmemiş verilerin gösterildiği örnek" :::

[!INCLUDE [Transform data before using large usage files](../../../includes/cost-management-billing-transform-data-before-using-large-usage-files.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Maliyet analiziyle maliyetleri araştırma ve analiz etme](../costs/quick-acm-cost-analysis.md).