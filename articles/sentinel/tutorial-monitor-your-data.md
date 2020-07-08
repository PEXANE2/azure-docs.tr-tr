---
title: Azure 'da Azure Izleyici çalışma kitaplarını kullanarak verilerinizi görselleştirin | Microsoft Docs
description: Azure Sentinel 'de çalışma kitaplarını kullanarak verilerinizi görselleştirmeyi öğrenmek için bu öğreticiyi kullanın.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2020
ms.author: yelevin
ms.openlocfilehash: 8d8f1343d92f66dc464ab7064949bbabb813268e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83798540"
---
# <a name="tutorial-visualize-and-monitor-your-data"></a>Öğretici: verilerinizi görselleştirin ve izleyin



[Veri kaynaklarınızı](quickstart-onboard.md)   Azure Sentinel 'e bağladıktan sonra, özel panolar oluşturma konusunda çok yönlülük sağlayan Azure izleyici çalışma kitaplarını benimseyerek Azure Sentinel benimseme kullanarak verileri görselleştirebilir ve izleyebilirsiniz. Çalışma kitapları Azure Sentinel 'de farklı şekilde görüntülenirken, [Azure Izleyici çalışma kitapları ile etkileşimli raporlar oluşturmayı](../azure-monitor/platform/workbooks-overview.md)görmeniz yararlı olabilir. Azure Sentinel, verileriniz genelinde özel çalışma kitapları oluşturmanızı sağlar ve ayrıca, bir veri kaynağını bağladığınızda verilerinize hızlı bir şekilde Öngörüler elde etmenizi sağlayan yerleşik çalışma kitabı şablonlarıyla birlikte gelir.


Bu öğretici, verilerinizi Azure Sentinel 'de görselleştirmenize yardımcı olur.
> [!div class="checklist"]
> * Yerleşik çalışma kitaplarını kullanma
> * Yeni çalışma kitapları oluşturma

## <a name="prerequisites"></a>Ön koşullar

- Azure Sentinel çalışma alanının kaynak grubunda en az çalışma kitabı okuyucusu veya çalışma kitabı katılımcısı izinlerinizin olması gerekir.

> [!NOTE]
> Azure Sentinel 'de görebileceğiniz çalışma kitapları, Azure Sentinel çalışma alanının kaynak grubu içinde kaydedilir ve oluşturuldukları çalışma alanına göre etiketlenebilir.

## <a name="use-built-in-workbooks"></a>Yerleşik çalışma kitaplarını kullanma

1. Azure Sentinel yerleşik çalışma kitaplarının tam listesini görmek için **çalışma kitaplarına** gidin ve **Şablonlar** ' ı seçin. Bağladığınız veri türleriyle ilgili olan hangisi olduğunu görmek için, her çalışma kitabındaki **gerekli veri türleri** alanı, ilgili verileri zaten Azure Sentinel 'e akıdıysanız yeşil onay işaretinin yanındaki veri türünü listeleyecek.
  ![çalışma kitaplarına git](./media/tutorial-monitor-data/access-workbooks.png)
1. Şablondaki verileri doldurulmuş olarak görmek için **çalışma kitabını görüntüle** ' ye tıklayın.
  
1. Çalışma kitabını düzenlemek için **Kaydet**' i seçin ve ardından şablon için JSON dosyasını kaydetmek istediğiniz konumu seçin. 

   > [!NOTE]
   > Bu, ilgili şablonu temel alan bir Azure kaynağı oluşturur ve şablon JSON dosyasının kendisini değil, verileri kaydeder.


1. **Çalışma kitabını görüntüle**' yi seçin. Ardından, üstteki **Düzenle** düğmesine tıklayın. Artık çalışma kitabını düzenleyebilir ve gereksinimlerinize göre özelleştirebilirsiniz. Çalışma kitabını özelleştirme hakkında daha fazla bilgi için bkz. [Azure Izleyici çalışma kitapları ile etkileşimli raporlar oluşturma](../azure-monitor/platform/workbooks-overview.md).
![çalışma kitaplarını görüntüleme](./media/tutorial-monitor-data/workbook-graph.png)
1. Değişikliklerinizi yaptıktan sonra çalışma kitabını kaydedebilirsiniz. 

1. Çalışma kitabını da kopyalayabilir: **Düzenle** ' yi seçin ve **farklı kaydet**' i seçip aynı abonelik ve kaynak grubu altında başka bir adla kaydettiğinizden emin olun. Bu çalışma kitapları, **çalışma kitapları** sekmesinin altında görüntülenir.


## <a name="create-new-workbook"></a>Yeni çalışma kitabı oluştur

1. Çalışma **kitaplarına** gidip sıfırdan yeni bir çalışma kitabı oluşturmak için **çalışma kitabı Ekle** ' yi seçin.
  ![çalışma kitaplarına git](./media/tutorial-monitor-data/create-workbook.png)

1. Çalışma kitabını düzenlemek için **Düzenle**' yi seçin ve ardından gerektiğinde metin, sorgular ve parametreler ekleyin. Çalışma kitabını özelleştirme hakkında daha fazla bilgi için bkz. [Azure Izleyici çalışma kitapları ile etkileşimli raporlar oluşturma](../azure-monitor/platform/workbooks-overview.md). 

1. Bir sorgu oluştururken, **veri kaynağını** **Günlükler**olarak ayarlayın, **kaynak türü** **Log Analytics** olarak ayarlanır ve ilgili çalışma alanlarını seçin. 

1. Çalışma kitabınızı oluşturduktan sonra, Azure Sentinel çalışma alanınızın abonelik ve kaynak grubunun altına kaydettiğinizden emin olmak için çalışma kitabını kaydedin.

1. Kuruluşunuzdaki başkalarının çalışma kitabını kullanmasına izin vermek istiyorsanız, **paylaşılan raporlar**' ı seçmek **için Kaydet** ' in altında. Bu çalışma kitabının yalnızca sizin için kullanılabilir olmasını istiyorsanız, **Raporlarım**' ı seçin.

1. Çalışma alanınızdaki çalışma kitapları arasında geçiş yapmak için **Open** ![ ](./media/tutorial-monitor-data/switch.png) herhangi bir çalışma kitabının üst bölmesinde anahtar çalışma kitaplarını aç ' ı seçebilirsiniz. Sağ tarafta açılan pencerede, çalışma kitapları arasında geçiş yapın.

   ![Çalışma kitaplarını değiştirme](./media/tutorial-monitor-data/switch-workbooks.png)


## <a name="how-to-delete-workbooks"></a>Çalışma kitaplarını silme

Azure Sentinel şablonundan oluşturulan çalışma kitaplarını silebilirsiniz. 

Özelleştirilmiş bir çalışma kitabını silmek için, çalışma kitapları sayfasında, silmek istediğiniz kaydedilmiş çalışma kitabını seçin ve **Sil**' i seçin. Bu, kaydedilen çalışma kitabını kaldırır.

> [!NOTE]
> Bu, kaynağın yanı sıra şablonda yaptığınız değişiklikleri de kaldırır. Özgün şablon kullanılabilir olmaya devam edecektir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Sentinel 'de verilerinizi görüntülemeyi öğrendiniz.

Tehditlere yönelik yanıtlarınızı otomatik hale getirmeyi öğrenmek için bkz. [Azure Sentinel 'de otomatik tehdit yanıtlarını ayarlama](tutorial-respond-threats-playbook.md).
