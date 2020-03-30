---
title: Azure Sentinel'deki Azure Monitör Çalışma Kitaplarını temel alan panolar kullanarak verilerinizi görselleştirin | Microsoft Dokümanlar
description: Azure Sentinel'deki çalışma kitaplarını temel alan panolar kullanarak verilerinizi nasıl görselleştirerinizi öğrenmek için bu öğreticiyi kullanın.
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
ms.date: 01/01/2020
ms.author: yelevin
ms.openlocfilehash: b4461ac43e9356536914b345ef28f5de62fc9f82
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77585229"
---
# <a name="tutorial-visualize-and-monitor-your-data"></a>Öğretici: Verilerinizi görselleştirin ve izleyin



Veri [kaynaklarınızı](quickstart-onboard.md) Azure Sentinel'e bağladıktan sonra, özel panolar oluşturmada çok yönlülük sağlayan Azure Monitor Çalışma Kitapları'nın Azure Sentinel benimsenmesini kullanarak verileri görselleştirebilir ve izleyebilirsiniz. Çalışma Kitapları Azure Sentinel'de farklı görüntülense de, Azure [Monitör Çalışma Kitapları ile etkileşimli raporların](../azure-monitor/app/usage-workbooks.md)nasıl oluşturulabileceğini görmeniz yararlı olabilir. Azure Sentinel, verileriniz arasında özel çalışma kitapları oluşturmanıza olanak tanır ve bir veri kaynağını bağlar bağlamaz verilerinizde hızlı bir şekilde öngörüler elde etmenizi sağlayan yerleşik çalışma kitabı şablonlarıyla birlikte gelir.


Bu öğretici, verilerinizi Azure Sentinel'de görselleştirmenize yardımcı olur.
> [!div class="checklist"]
> * Yerleşik çalışma kitaplarını kullanma
> * Yeni çalışma kitapları oluşturma

## <a name="prerequisites"></a>Ön koşullar

- Azure Sentinel çalışma alanının kaynak grubunda en az Çalışma Kitabı okuyucuveya Çalışma Kitabı katılımcısı izinlerine sahip olmalısınız.

> [!NOTE]
> Azure Sentinel'de görebileceğiniz çalışma kitapları Azure Sentinel çalışma alanı kaynak grubuna kaydedilir ve oluşturuldukları çalışma alanı tarafından etiketlenir.

## <a name="use-built-in-workbooks"></a>Yerleşik çalışma kitaplarını kullanma

1. Çalışma **Kitapları'na** gidin ve ardından Azure Sentinel yerleşik çalışma kitaplarının tam listesini görmek için **Şablonlar'ı** seçin. Bağlı olduğunuz veri türleri ile hangisinin alakalı olduğunu görmek için, her çalışma kitabındaki **Gerekli veri türleri** alanı, ilgili verileri Azure Sentinel'e aktarmışsanız, veri türünü yeşil onay işaretinin yanında listeler.
  ![çalışma kitaplarına gidin](./media/tutorial-monitor-data/access-workbooks.png)
1. Verilerinizle dolu şablonu görmek için **çalışma kitabını görüntüle'yi** tıklatın.
  
1. Çalışma kitabını yeniden yapmak için **Kaydet'i**seçin ve ardından şabloniçin json dosyasını kaydetmek istediğiniz konumu seçin. 

   > [!NOTE]
   > Bu, ilgili şablonu temel alan bir Azure kaynağı oluşturur ve verileri değil şablon Json dosyasını kaydeder.


1. **Çalışma kitabını görüntüle'yi**seçin. Ardından, üstteki **Edit** düğmesini tıklatın. Artık çalışma kitabını ve gereksinimlerinize göre özelleştirebilirsiniz. Çalışma kitabını özelleştirme hakkında daha fazla bilgi için [Azure Monitörçalışma Kitapları ile etkileşimli raporların](../azure-monitor/app/usage-workbooks.md)nasıl oluşturulabildiğini öğrenin.
![çalışma kitaplarını görüntüleme](./media/tutorial-monitor-data/workbook-graph.png)
1. Değişikliklerinizi yaptıktan sonra çalışma kitabını kaydedebilirsiniz. 

1. Çalışma kitabını da klonlayabilirsiniz: **Edit'i** seçin ve **ardından**aynı abonelik ve kaynak grubu altında başka bir adla kaydettiğinizi belirterek kaydedin. Bu çalışma kitapları **Çalışma Kitaplarım** sekmesi altında görüntülenir.


## <a name="create-new-workbook"></a>Yeni çalışma kitabı oluşturma

1. Çalışma **Kitapları'na** gidin ve ardından sıfırdan yeni bir çalışma kitabı oluşturmak için **çalışma kitabı ekle'yi** seçin.
  ![çalışma kitaplarına gidin](./media/tutorial-monitor-data/create-workbook.png)

1. Çalışma kitabını dalabilmek için **Edit'i**seçin ve ardından gerektiğinde metin, sorgu ve parametreler ekleyin. Çalışma kitabını özelleştirme hakkında daha fazla bilgi için [Azure Monitörçalışma Kitapları ile etkileşimli raporların](../azure-monitor/app/usage-workbooks.md)nasıl oluşturulabildiğini öğrenin. 

1. Bir sorgu oluşturmak, **Veri kaynağı** **Günlükler**olarak ayarlanır ayarlayın, **Kaynak türü** **Log Analytics** olarak ayarlanır ve sonra ilgili çalışma alanı(lar seçin). 

1. Çalışma kitabınızı oluşturduktan sonra, çalışma kitabınızı Azure Sentinel çalışma alanınızın abonelik ve kaynak grubuna kaydettiğinizden emin olmak için kaydedin.

1. Kuruluşunuzdaki diğer **kuruluşların, Paylaşılan raporları**seçmek **için Kaydet** altında çalışma kitabını kullanmasına izin vermek istiyorsanız. Bu çalışma kitabının yalnızca sizin kullanabileceğiniz olmasını istiyorsanız, **Raporlarım'ı**seçin.

1. Çalışma alanınızdaki çalışma kitapları arasında geçiş yapmak için,](./media/tutorial-monitor-data/switch.png)herhangi bir çalışma kitabının üst bölmesinde **Açık** ![Anahtar çalışma kitaplarını seçebilirsiniz. Sağa açılan pencerede, çalışma kitapları arasında geçiş yapabilirsiniz.

   ![Çalışma kitaplarını değiştirme](./media/tutorial-monitor-data/switch-workbooks.png)


## <a name="how-to-delete-workbooks"></a>Çalışma kitaplarını silme

Azure Sentinel şablonundan oluşturulan Çalışma Kitaplarını silebilirsiniz. 

Özelleştirilmiş bir çalışma kitabını silmek için, Çalışma Kitapları sayfasında, silmek istediğiniz kayıtlı çalışma kitabını seçin ve **Sil'i**seçin. Bu, kaydedilen çalışma kitabını kaldırır.

> [!NOTE]
> Bu, kaynağı ve şablonda yaptığınız değişiklikleri kaldırır. Özgün şablon kullanılabilir kalır.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, verilerinizi Azure Sentinel'de görüntülemeyi öğrendiniz.

Tehditlere karşı yanıtlarınızı nasıl otomatikleştirerinizi öğrenmek için Azure [Sentinel'de otomatik tehdit yanıtları ayarlama'ya](tutorial-respond-threats-playbook.md)bakın.
