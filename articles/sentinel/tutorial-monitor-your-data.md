---
title: Azure 'da Azure Izleyici çalışma kitaplarını temel alan panoları kullanarak verilerinizi izleyin | Microsoft Docs
description: Azure Sentinel 'deki çalışma kitaplarına göre panoları kullanarak verilerinizi nasıl izleyeceğinizi öğrenmek için bu öğreticiyi kullanın.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: 3f5e01a0ceebac092084e339994161763a11c49b
ms.sourcegitcommit: 992e070a9f10bf43333c66a608428fcf9bddc130
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71240419"
---
# <a name="tutorial-monitor-your-data"></a>Öğretici: Veri ardışık



 [Veri kaynaklarınızı](quickstart-onboard.md) Azure Sentinel 'e bağladıktan sonra, özel panolar oluşturma konusunda çok yönlülük sağlayan Azure izleyici çalışma kitapları ile Azure Sentinel tümleştirmesini kullanarak verileri izleyebilirsiniz. Çalışma kitapları Azure Sentinel 'de farklı şekilde görüntülenirken, [Azure Izleyici çalışma kitapları ile etkileşimli raporlar oluşturmayı](../azure-monitor/app/usage-workbooks.md)görmeniz yararlı olabilir. Azure Sentinel, verileriniz genelinde özel çalışma kitapları oluşturmanızı sağlar ve ayrıca, bir veri kaynağını bağladığınızda verilerinize hızlı bir şekilde Öngörüler elde etmenizi sağlayan yerleşik çalışma kitabı şablonlarıyla birlikte gelir.


Bu öğretici, verilerinizi Azure Sentinel 'de izlemenize yardımcı olur.
> [!div class="checklist"]
> * Yerleşik çalışma kitaplarını kullanma
> * Yeni çalışma kitapları oluşturma

## <a name="prerequisites"></a>Önkoşullar

- Azure Sentinel çalışma alanının kaynak grubunda en az çalışma kitabı okuyucusu veya çalışma kitabı katılımcısı izinlerinizin olması gerekir.

> [!NOTE]
> Azure Sentinel 'de görebileceğiniz çalışma kitapları, Azure Sentinel çalışma alanı kaynak grubu içinde kaydedilir ve oluşturuldukları çalışma alanına göre etiketlenebilir.

## <a name="use-built-in-workbooks"></a>Yerleşik çalışma kitaplarını kullanma

1. Azure Sentinel yerleşik çalışma kitaplarının tam listesini görmek için **çalışma kitaplarına** gidin ve **Şablonlar** ' ı seçin. Bağladığınız veri türleriyle ilgili olan hangisi olduğunu görmek için, her çalışma kitabındaki **gerekli veri türleri** alanı, ilgili verileri zaten Azure Sentinel 'e akıdıysanız yeşil onay işaretinin yanındaki veri türünü listeleyecek.
  ![çalışma kitaplarına git](./media/tutorial-monitor-data/access-workbooks.png)
1. Şablondaki verileri doldurulmuş olarak görmek için **çalışma kitabını görüntüle** ' ye tıklayın.
  
1. Çalışma kitabını düzenlemek için **Kaydet**' i seçin ve ardından şablon için JSON dosyasını kaydetmek istediğiniz konumu seçin. 

   > [!NOTE]
   > Bu, ilgili şablonu temel alan bir Azure kaynağı oluşturur ve şablon JSON dosyasının kendisini değil, verileri kaydeder.


1. **Çalışma kitabını görüntüle**' yi seçin. Ardından, üstteki **Düzenle** düğmesine tıklayın. Artık çalışma kitabını düzenleyebilir ve gereksinimlerinize göre özelleştirebilirsiniz. Çalışma kitabını özelleştirme hakkında daha fazla bilgi için bkz. [Azure Izleyici çalışma kitapları ile etkileşimli raporlar oluşturma](../azure-monitor/app/usage-workbooks.md).
![çalışma kitaplarını görüntüleme](./media/tutorial-monitor-data/workbook-graph.png)
1. Değişikliklerinizi yaptıktan sonra çalışma kitabını kaydedebilirsiniz. 

1. Çalışma kitabını da kopyalayabilirsiniz: **Düzenle** ' yi ve ardından **farklı kaydet**' i seçip aynı abonelik ve kaynak grubu altında başka bir adla kaydettiğinizden emin olun. Bu çalışma kitapları, **çalışma kitapları** sekmesinin altında görüntülenir.


## <a name="create-new-workbook"></a>Yeni çalışma kitabı oluştur

1. Çalışma **kitaplarına** gidip sıfırdan yeni bir çalışma kitabı oluşturmak için **çalışma kitabı Ekle** ' yi seçin.
  ![çalışma kitaplarına git](./media/tutorial-monitor-data/create-workbook.png)

1. Çalışma kitabını düzenlemek için **Düzenle**' yi seçin ve ardından gerektiğinde metin, sorgular ve parametreler ekleyin. Çalışma kitabını özelleştirme hakkında daha fazla bilgi için bkz. [Azure Izleyici çalışma kitapları ile etkileşimli raporlar oluşturma](../azure-monitor/app/usage-workbooks.md). 

1. Bir sorgu oluştururken, **veri kaynağını** **Günlükler**olarak ayarlayın, **kaynak türü** **Log Analytics** olarak ayarlanır ve ilgili çalışma alanlarını seçin. 

1. Çalışma kitabınızı oluşturduktan sonra, Azure Sentinel çalışma alanınızın abonelik ve kaynak grubunun altına kaydettiğinizden emin olmak için çalışma kitabını kaydedin.

1. Kuruluşunuzdaki başkalarının çalışma kitabını kullanmasına izin vermek istiyorsanız, **paylaşılan raporlar**' ı seçmek **için Kaydet** ' in altında. Bu çalışma kitabının yalnızca sizin için kullanılabilir olmasını istiyorsanız, **Raporlarım**' ı seçin.

1. Çalışma alanınızdaki çalışma kitapları arasında geçiş yapmak için herhangi bir çalışma kitabının üst bölmesinde](./media/tutorial-monitor-data/switch.png)anahtar çalışma kitaplarını **Aç** ![' ı seçebilirsiniz. Sağ tarafta açılan pencerede, çalışma kitapları arasında geçiş yapın.

   ![Çalışma kitaplarını değiştirme](./media/tutorial-monitor-data/switch-workbooks.png)


## <a name="how-to-delete-workbooks"></a>Çalışma kitaplarını silme

Azure Sentinel şablonundan oluşturulan çalışma kitaplarını silebilirsiniz. 

Özelleştirilmiş çalışma kitabını silmek için:
1.  **Çalışma kitabını görüntüle**' yi seçin.
2.  En üstte **Aç** ' ı seçin.
3.  Çalışma kitaplarınızı listeleme tablosunda, silmek istediğiniz çalışma kitabının satırında, satırın sonundaki üç noktayı seçin... menüsünü açın ve **Sil**' i seçin. Bu, kaydedilen çalışma kitabını kaldırır.

> [!NOTE]
> Bu, kaynağın yanı sıra şablonda yaptığınız değişiklikleri de kaldırır. Özgün şablon kullanılabilir olmaya devam edecektir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Sentinel 'de verilerinizi görüntülemeyi öğrendiniz.

Tehditlere yönelik yanıtlarınızı otomatik hale getirmeyi öğrenmek için bkz. [Azure Sentinel 'de otomatik tehdit yanıtlarını ayarlama](tutorial-respond-threats-playbook.md).
