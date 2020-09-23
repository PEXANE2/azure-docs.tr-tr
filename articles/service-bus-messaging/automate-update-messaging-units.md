---
title: Azure Service Bus-mesajlaşma birimlerini otomatik olarak güncelleştir
description: Bu makalede bir Service Bus ad alanının mesajlaşma birimlerini otomatik olarak güncelleştirme ' nin nasıl kullanılacağı gösterilir.
ms.topic: how-to
ms.date: 09/15/2020
ms.openlocfilehash: 0a72cc991e768a7bed01762d984cc56238ae0ad0
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90984815"
---
# <a name="automatically-update-messaging-units-of-an-azure-service-bus-namespace"></a>Azure Service Bus bir ad alanının mesajlaşma birimlerini otomatik olarak güncelleştir 
Otomatik ölçeklendirme, uygulamanızdaki yükü işlemek için çalışan kaynakların doğru miktarına sahip olmasını sağlar. Yük arttıkça idare edilecek kaynakları eklemenize ve ayrıca boştaki kaynakları kaldırarak paradan tasarruf etmenize olanak tanır. Azure Izleyici 'nin otomatik ölçeklendirme özelliği hakkında daha fazla bilgi edinmek için [Microsoft Azure otomatik ölçeklendirmeyi genel bakış](../azure-monitor/platform/autoscale-overview.md) bölümüne bakın. 

Service Bus Premium Mesajlaşma Hizmeti, CPU'da ve bellek düzeyinde kaynak yalıtımına olanak sağladığından her müşterinin iş yükü yalıtımlı şekilde çalışır. Bu kaynak kapsayıcısı **mesajlaşma birimi** olarak adlandırılır. Mesajlaşma birimleri hakkında daha fazla bilgi edinmek için bkz. [Service Bus Premium mesajlaşma](service-bus-premium-messaging.md). 

Service Bus Premium ad alanları için otomatik ölçeklendirme özelliğini kullanarak, en düşük ve en fazla [mesajlaşma birimi](service-bus-premium-messaging.md) sayısını belirtebilir ve bir dizi kurala göre mesajlaşma birimi otomatik olarak ekleyebilir veya kaldırabilirsiniz. 

Örneğin, otomatik ölçeklendirme özelliğini kullanarak Service Bus ad alanları için aşağıdaki ölçeklendirme senaryolarını uygulayabilirsiniz. 

- Ad alanı CPU kullanımı %75 üzerinde kaldığında bir Service Bus ad alanı için mesajlaşma birimlerini arttırın. 
- Ad alanı CPU kullanımı %25 ' in altına gittiğinde bir Service Bus ad alanı için mesajlaşma birimlerini küçültün. 
- Çalışma saatleri sırasında daha fazla mesajlaşma birimi ve saat boyunca daha az kullanın. 

Bu makalede, Azure portal bir Service Bus ad alanını ( [ileti birimi](service-bus-premium-messaging.md)güncelleştirme) nasıl otomatik olarak ölçeklendirebilmeniz gösterilmektedir. 

> [!IMPORTANT]
> Bu makale yalnızca Azure Service Bus **Premium** katmanı için geçerlidir. 

## <a name="autoscale-setting-page"></a>Otomatik ölçeklendirme ayarı sayfası
İlk olarak, Service Bus ad alanınız için **Otomatik ölçeklendirme ayarları** sayfasına gitmek üzere bu adımları izleyin.

1. [Azure portalda](https://portal.azure.com) oturum açın. 
2. Arama çubuğuna **Service Bus**yazın, açılan listeden **Service Bus** ' i seçin ve **ENTER**tuşuna basın. 
1. Ad alanları listesinden **Premium ad** alanınızı seçin. 
1. **Ölçek** sayfasına geçiş yapın. 

    :::image type="content" source="./media/automate-update-messaging-units/scale-page.png" alt-text="Service Bus ad alanı-ölçek sayfası":::

## <a name="manual-scale"></a>El ile ölçeklendirme 
Bu ayar ad alanı için sabit sayıda mesajlaşma birimi ayarlamanıza olanak sağlar. 

1. **Otomatik ölçeklendirme ayarı** sayfasında, zaten seçili değilse **el ile ölçeklendir** ' i seçin. 
1. **Mesajlaşma birimleri** ayarı için, açılan listeden mesajlaşma birimi sayısını seçin.
1. Ayarları kaydetmek için araç çubuğunda **Kaydet** ' i seçin. 

    :::image type="content" source="./media/automate-update-messaging-units/manual-scale.png" alt-text="Mesajlaşma birimlerini el ile ölçeklendirme":::       


## <a name="custom-autoscale---default-condition"></a>Özel otomatik ölçeklendirme-varsayılan koşul
Koşulları kullanarak, mesajlaşma birimlerinin otomatik olarak ölçeklendirilmesini yapılandırabilirsiniz. Bu ölçek koşulu, diğer ölçek koşullarının hiçbiri eşleşmediği zaman yürütülür. Varsayılan koşulu aşağıdaki şekilde ayarlayabilirsiniz:

- Bir ölçüme göre ölçeklendirin (CPU veya bellek kullanımı gibi)
- Belirli sayıda mesajlaşma birimine ölçeklendirin

Varsayılan bir koşul için belirli bir gün veya tarih aralığında otomatik ölçeklendirme yapmak için bir zamanlama ayarlayamazsınız. Bu ölçek koşulu, zamanlamalarla diğer ölçek koşullarının hiçbiri eşleşmediğinden yürütülür. 

### <a name="scale-based-on-a-metric"></a>Bir ölçüme göre ölçeklendirin
Aşağıdaki yordamda, CPU kullanımı %75 ' den fazla olduğunda (ölçeği genişletme) ve CPU kullanımı %25 ' ten az olduğunda mesajlaşma birimlerini (ölçeği genişletme) otomatik olarak artırmak için bir koşul ekleme işlemi gösterilmektedir. Artımlar 1 ile 2 arasında, 2 ile 4 arasında ve 4 ile 8 arasında yapılır. Benzer şekilde, 8-4, 4 ile 2 arasında ve 2 ile 1 arasında bir azaltır. 

1. **Otomatik ölçeklendirme ayarı** sayfasında, **kaynağı nasıl ölçeklendireöğrenin seçeneğini belirleyin** için **özel otomatik ölçek** ' i seçin. 
1. Sayfanın **varsayılan** bölümünde varsayılan koşul için bir **ad** belirtin. Metni düzenlemek için **kalem** simgesini seçin. 
1. **Ölçek modu**için **bir ölçüme göre ölçek '** i seçin. 
1. **+ Kural Ekle**' yi seçin. 

    :::image type="content" source="./media/automate-update-messaging-units/default-scale-metric-add-rule-link.png" alt-text="Bir ölçüme göre varsayılan ölçeklendirme":::    
1. **Kural ölçeklendirme** sayfasında, aşağıdaki adımları izleyin:
    1. **Ölçüm adı** açılır listesinden bir ölçüm seçin. Bu örnekte **CPU**olur. 
    1. İşleç ve eşik değerlerini seçin. Bu örnekte, **ölçüm eşiğinin ölçek eylemini tetiklemesi**için ve **75** **daha büyüktür** . 
    1. **Eylem** bölümünde bir **işlem** seçin. Bu örnekte, **artış**olarak ayarlanmıştır. 
    1. Sonra **Ekle** ' yi seçin.
    
        :::image type="content" source="./media/automate-update-messaging-units/scale-rule-cpu-75.png" alt-text="Varsayılan-CPU kullanımı %75 ' den büyükse ölçeği genişletme":::       

        > [!NOTE]
        > Bu örnekte, genel CPU kullanımı %75 üzerinde olursa otomatik ölçeklendirme özelliği ad alanı için mesajlaşma birimlerini arttırır. Artımlar 1 ile 2 arasında, 2 ile 4 arasında ve 4 ile 8 arasında yapılır. 
1. **+ Kural Ekle** ' yi seçin ve **Ölçek kuralı** sayfasında aşağıdaki adımları izleyin:
    1. **Ölçüm adı** açılır listesinden bir ölçüm seçin. Bu örnekte **CPU**olur. 
    1. İşleç ve eşik değerlerini seçin. Bu örnekte, **Ölçek eyleminin tetiklenmesi Için ölçüm eşiği**' ne **küçüktür** ve **25 ' tir** . 
    1. **Eylem** bölümünde bir **işlem** seçin. Bu örnekte, **azaltma**olarak ayarlanmıştır. 
    1. Sonra **Ekle** ' yi seçin. 

        :::image type="content" source="./media/automate-update-messaging-units/scale-rule-cpu-25.png" alt-text="CPU kullanımı %25 ' ten az ise varsayılan ölçek":::       

        > [!NOTE]
        > Bu örnekte, genel CPU kullanımı %25 ' in altında olursa otomatik ölçeklendirme özelliği ad alanı için mesajlaşma birimlerini azaltır. Bu, 8-4, 4 ve 2 ' ye kadar azaltır ve 2 ile 1 arasında yapılır. 
1. **En düşük** ve **en yüksek** ve **varsayılan** mesajlaşma birimi sayısını ayarlayın.

    :::image type="content" source="./media/automate-update-messaging-units/default-scale-metric-based.png" alt-text="Ölçüm temelinde varsayılan kural":::
1. Otomatik ölçeklendirme ayarını kaydetmek için araç çubuğunda **Kaydet** ' i seçin. 
        
### <a name="scale-to-specific-number-of-messaging-units"></a>Belirli sayıda mesajlaşma birimine ölçeklendirin
Belirli sayıda mesajlaşma birimini kullanmak üzere ad alanını ölçeklendirmek üzere kuralı yapılandırmak için bu adımları izleyin. Yine, diğer ölçek koşullarının hiçbiri eşleşmediği zaman varsayılan koşul uygulanır. 

1. **Otomatik ölçeklendirme ayarı** sayfasında, **kaynağı nasıl ölçeklendireöğrenin seçeneğini belirleyin** için **özel otomatik ölçek** ' i seçin. 
1. Sayfanın **varsayılan** bölümünde varsayılan koşul için bir **ad** belirtin. 
1. **Ölçek modu**için **belirli mesajlaşma birimlerine ölçeklendirin** ' ı seçin. 
1. **Mesajlaşma birimleri**için, varsayılan mesajlaşma birimi sayısını seçin. 

    :::image type="content" source="./media/automate-update-messaging-units/default-scale-messaging-units.png" alt-text="Varsayılan-belirli mesajlaşma birimlerine ölçeklendirin":::       

## <a name="custom-autoscale---additional-conditions"></a>Özel otomatik ölçeklendirme-ek koşullar
Önceki bölümde, otomatik ölçeklendirme ayarı için nasıl varsayılan koşul ekleyeceğiniz gösterilmektedir. Bu bölümde, otomatik ölçeklendirme ayarına nasıl daha fazla koşul ekleyeceğiniz gösterilmektedir. Bu ek varsayılan olmayan koşullar için, bir haftanın belirli günlerine veya tarih aralığına göre bir zamanlama belirleyebilirsiniz. 

### <a name="scale-based-on-a-metric"></a>Bir ölçüme göre ölçeklendirin
1. **Otomatik ölçeklendirme ayarı** sayfasında, **kaynağı nasıl ölçeklendireöğrenin seçeneğini belirleyin** için **özel otomatik ölçek** ' i seçin. 
1. **Varsayılan** blok altında **Ölçek koşulu Ekle** ' yi seçin. 

    :::image type="content" source="./media/automate-update-messaging-units/add-scale-condition-link.png" alt-text="Özel-ölçek koşulu bağlantısı ekle":::    
1. Koşul için bir **ad** belirtin. 
1. **Ölçüm seçeneğine göre ölçeğin** seçili olduğunu doğrulayın. 
1. Genel CPU kullanımı %75 üzerinde kaldığında mesajlaşma birimlerini artırmak üzere bir kural eklemek için **+ Kural Ekle** ' yi seçin. [Varsayılan koşul](#custom-autoscale---default-condition) bölümündeki adımları izleyin. 
5. **En düşük** ve **en yüksek** ve **varsayılan** mesajlaşma birimi sayısını ayarlayın.
6. Ayrıca, özel bir koşula (varsayılan koşulda değil) bir **zamanlama** da ayarlayabilirsiniz. Koşul için başlangıç ve bitiş tarihlerini belirtebilir (veya) bir haftanın belirli günlerini (Pazartesi, Salı, vb.) seçebilirsiniz. 
    1. **Başlangıç/bitiş tarihlerini belirt**' i seçerseniz, koşulun etkin olması Için **saat dilimi**, **Başlangıç tarihi ve saati** ile **bitiş tarihi ve saatini** (aşağıdaki görüntüde gösterildiği gibi) seçin. 

       :::image type="content" source="./media/automate-update-messaging-units/custom-min-max-default.png" alt-text="İleti birimi sayısı için en düşük, en yüksek ve varsayılan değerler":::
    1. **Belirli günleri Yinele**' yi seçerseniz, koşulun uygulanacağı haftanın günleri, saat dilimi, başlangıç saati ve bitiş saati ' ni seçin. 

        :::image type="content" source="./media/automate-update-messaging-units/repeat-specific-days.png" alt-text="Belirli günleri Yinele":::
  
### <a name="scale-to-specific-number-of-messaging-units"></a>Belirli sayıda mesajlaşma birimine ölçeklendirin
1. **Otomatik ölçeklendirme ayarı** sayfasında, **kaynağı nasıl ölçeklendireöğrenin seçeneğini belirleyin** için **özel otomatik ölçek** ' i seçin. 
1. **Varsayılan** blok altında **Ölçek koşulu Ekle** ' yi seçin. 

    :::image type="content" source="./media/automate-update-messaging-units/add-scale-condition-link.png" alt-text="Özel-ölçek koşulu bağlantısı ekle":::    
1. Koşul için bir **ad** belirtin. 
2. **Ölçek modu**için **belirli mesajlaşma birimlerine Ölçekle** seçeneğini belirleyin. 
1. Açılan listeden **mesajlaşma birimi** sayısını seçin. 
6. **Zamanlama**için, koşul için başlangıç ve bitiş tarihlerini belirtin (veya) bir hafta ve saat için belirli günler (Pazartesi, Salı, vb.) seçin. 
    1. **Başlangıç/bitiş tarihlerini belirt**' i seçerseniz, koşulun geçerli olması Için saat **dilimi**, **Başlangıç tarihi ve saati** ile **bitiş tarihi ve saatini** seçin. 
    
    :::image type="content" source="./media/automate-update-messaging-units/scale-specific-messaging-units-start-end-dates.png" alt-text="belirli mesajlaşma birimlerine ölçeklendirin-başlangıç ve bitiş tarihleri":::        
    1. **Belirli günleri Yinele**' yi seçerseniz, koşulun uygulanacağı haftanın günleri, saat dilimi, başlangıç saati ve bitiş saati ' ni seçin.
    
    :::image type="content" source="./media/automate-update-messaging-units/repeat-specific-days-2.png" alt-text="belirli mesajlaşma birimlerine ölçeklendirin-belirli günleri yineleyin":::

> [!IMPORTANT]
> Otomatik ölçeklendirme ayarlarının nasıl çalıştığı, özellikle bir profil veya koşul nasıl yaptığı ve birden çok kuralı değerlendiren hakkında daha fazla bilgi edinmek için bkz. [Otomatik ölçeklendirme ayarlarını anlama](../azure-monitor/platform/autoscale-understanding-settings.md).          

## <a name="next-steps"></a>Sonraki adımlar
Mesajlaşma birimleri hakkında bilgi edinmek için bkz. [Premium mesajlaşma](service-bus-premium-messaging.md)

