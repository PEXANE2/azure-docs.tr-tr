---
title: include dosyası
description: include dosyası
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: include
ms.date: 10/09/2020
ms.author: banders
ms.reviwer: ''
ms.custom: include file
ms.openlocfilehash: fb34b4ef1cf209a15c7e6ac9a59cc11dd4b0056e
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92026738"
---
## <a name="transform-data-before-using-large-usage-files"></a>Büyük kullanım dosyalarını kullanmadan önce verileri dönüştürme

Bazen kullanım veya mutabakat dosyanız açılmayacak kadar büyük olabilir. Ya da bir sorunu gidermek için çok az bilgiye ihtiyaç duyabilirsiniz. Örneğin, yalnızca belirli bir kaynağa ait bilgilere veya belirli hizmetlerin ya da kaynak gruplarının tüketimine ihtiyacınız olabilir. Özet tablolarını oluşturmadan önce verileri özetlemek amacıyla dönüştürebilirsiniz.

1. Excel’de boş bir çalışma kitabı açın.
1. Üstteki menüden **Veri** > **Metinden/CSV’den** seçeneğini belirleyin, kullanım dosyanızı seçin ve sonra **İçeri Aktar** seçeneğini belirleyin.
1. Pencerenin alt kısmında **Verileri Dönüştür**’ü seçin. Yeni bir pencerede verilerin özeti gösterilir.  
    :::image type="content" source="./media/cost-management-billing-transform-data-before-using-large-usage-files/summarized-data.png" alt-text="Özetlenen verilerin gösterildiği örnek" lightbox="./media/cost-management-billing-transform-data-before-using-large-usage-files/summarized-data.png" :::
1. MCA kullanım dosyalarının sütun başlıkları genellikle ilk satırlarda olduğundan, Microsoft Müşteri Sözleşmeniz varsa bu adımı atlayıp sonraki adımla devam edin. Tabloyu oluşturarak verileri hazırlayın. Üstteki satırları kaldırın ve yalnızca başlıkları bırakın. **Satırları Kaldır** > **Üstteki Satırları Kaldır**’ı seçin.  
     :::image type="content" source="./media/cost-management-billing-transform-data-before-using-large-usage-files/remove-top-rows.png" alt-text="Özetlenen verilerin gösterildiği örnek" :::
1. Üstteki Satırları Kaldır penceresine, kaldırılacak satır sayısını girin. Bu sayı EA için genellikle 2, CSP için 1’dir. **Tamam**’ı seçin.
1. **İlk Satırı Üst Bilgi Olarak Kullan**’ı seçin.  
    :::image type="content" source="./media/cost-management-billing-transform-data-before-using-large-usage-files/use-first-row-as-header.png" alt-text="Özetlenen verilerin gösterildiği örnek" :::
    
    Tablo görünümü, sütun başlıklarını üstte gösterir.
1. Ardından, filtre ekleyin. Filtrelemek için her sütun başlığının sağındaki seçici okları kullanın. Önerilen filtreler Abonelik Kimliği, Hizmet Adı (Ölçüm Kategorisi), Örnek Kimliği ve kaynak grubudur. Aynı belgede birden çok filtre kullanabilirsiniz. Belge boyutunu azaltmak ve daha sonraki çalışmaları kolaylaştırmak için tüm olası filtreleri uygulamanızı öneririz.
1. Filtrelerinizi uyguladıktan sonra **Kapat ve Yükle**’yi seçin.  
    :::image type="content" source="./media/cost-management-billing-transform-data-before-using-large-usage-files/close-and-load.png" alt-text="Özetlenen verilerin gösterildiği örnek" :::

Dosya yüklenir ve filtrelenmiş kullanım verilerini içeren bir tablo gösterir. Artık kullanım sorunlarını gidermek için yeni bir özet tablosu oluşturabilirsiniz.