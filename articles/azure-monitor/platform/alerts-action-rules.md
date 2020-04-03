---
title: Azure Monitör uyarıları için eylem kuralları
description: Azure Monitor'da eylem kurallarının ne olduğunu ve bunları nasıl yapılandırıp yöneteceklerini anlama.
ms.topic: conceptual
ms.date: 04/25/2019
ms.subservice: alerts
ms.openlocfilehash: 6585890395d7656f239ac3098cd374ecd4757842
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80618983"
---
# <a name="action-rules-preview"></a>Eylem kuralları (önizleme)

Eylem kuralları, herhangi bir Azure Kaynak Yöneticisi kapsamındaki eylemleri (Azure aboneliği, kaynak grubu veya hedef kaynak) tanımlamanıza veya bastırmanıza yardımcı olur. Bunlar, harekete geçmenize yardımcı olan uyarı örneklerinin belirli alt kümesini daraltmama yardımcı olan çeşitli filtrelere sahiptir.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4rBZ2]

## <a name="why-and-when-should-you-use-action-rules"></a>Neden ve ne zaman eylem kurallarını kullanmalısınız?

### <a name="suppression-of-alerts"></a>Uyarıların bastırılması

Uyarıların oluşturduğu bildirimleri bastırmanın yararlı olduğu birçok senaryo vardır. Bu senaryolar, planlanan bakım penceresi sırasında bastırmadan iş dışı saatlerde bastırmaya kadar değişir. Örneğin, **ContosoVM'dan** sorumlu ekip, **contosovm** planlı bakımdan geçtiği için, önümüzdeki hafta sonu için uyarı bildirimlerini bastırmak istiyor. 

Takım, **ContosoVM'da** el ile yapılandırılan her uyarı kuralını devre dışı bırakıp bakımdan sonra yeniden etkinleştirebilir, ancak bu basit bir işlem değildir. Eylem kuralları, bastırma süresini esnek bir şekilde yapılandırabilme yeteneğiyle ölçekte uyarı bastırma özelliğini tanımlamanıza yardımcı olur. Önceki örnekte, takım **ContosoVM'da** hafta sonu için tüm uyarı bildirimlerini bastıran bir eylem kuralı tanımlayabilir.


### <a name="actions-at-scale"></a>Ölçekte ki eylemler

Uyarı kuralları, uyarı oluşturulduğunda tetikleyen eylem grubunu tanımlamanıza yardımcı olsa da, müşterilerin genellikle operasyon kapsamları arasında ortak bir eylem grubu vardır. Örneğin, **ContosoRG** kaynak grubundan sorumlu bir ekip büyük olasılıkla **ContosoRG**içinde tanımlanan tüm uyarı kuralları için aynı eylem grubunu tanımlar. 

Eylem kuralları bu işlemi basitleştirmenize yardımcı olur. Ölçekte eylemleri tanımlayarak, yapılandırılan kapsamda oluşturulan herhangi bir uyarı için bir eylem grubu tetiklenebilir. Önceki örnekte, takım **ContosoRG'da,** içinde oluşturulan tüm uyarılar için aynı eylem grubunu tetikleyecek bir eylem kuralı tanımlayabilir.

> [!NOTE]
> Eylem kuralları şu anda Azure Hizmet Durumu uyarıları için geçerli değildir.

## <a name="configuring-an-action-rule"></a>Eylem kuralı yapılandırma

Azure Monitor'daki **Uyarılar** açılış sayfasından **eylemleri yönet'i** seçerek özelliğe erişebilirsiniz. Ardından, **Eylem kurallarını (önizleme)** seçin. Uyarılar için açılış sayfasının panosundan **Eylem kuralları (önizleme)** seçerek kurallara erişebilirsiniz.

![Azure Monitöraçılış sayfasından eylem kuralları](media/alerts-action-rules/action-rules-landing-page.png)

Select **+ Yeni Eylem Kuralı**. 

![Yeni eylem kuralı ekleme](media/alerts-action-rules/action-rules-new-rule.png)

Alternatif olarak, bir uyarı kuralını yapılandırırken bir eylem kuralı oluşturabilirsiniz.

![Yeni eylem kuralı ekleme](media/alerts-action-rules/action-rules-alert-rule.png)

Şimdi eylem kuralları oluşturmak için akış sayfasını görmeniz gerekir. Aşağıdaki öğeleri yapılandırın: 

![Yeni eylem kuralı oluşturma akışı](media/alerts-action-rules/action-rules-new-rule-creation-flow.png)

### <a name="scope"></a>Kapsam

Önce kapsamı seçin (Azure aboneliği, kaynak grubu veya hedef kaynak). Ayrıca, tek bir abonelik içindeki kapsamların birleşimini birden çok seçebilirsiniz.

![Eylem kuralı kapsamı](media/alerts-action-rules/action-rules-new-rule-creation-flow-scope.png)

### <a name="filter-criteria"></a>Filtre ölçütleri

Ayrıca, filtreleri uyarıların belirli bir alt kümesine daraltmak için de tanımlayabilirsiniz. 

Kullanılabilir filtreleri şunlardır: 

* **Önem Derecesi**: Bir veya daha fazla uyarı şiddeti seçme seçeneği. **Önem = Sev1,** eylem kuralının Sev1 olarak ayarlanan tüm uyarılar için geçerli olduğu anlamına gelir.
* **Monitör Hizmeti**: Kaynak izleme hizmetine dayalı bir filtredir. Bu filtre de birden çok seçimdir. Örneğin, **Monitor Service = "Application Insights",** eylem kuralının tüm Application Insights tabanlı uyarılar için geçerli olduğu anlamına gelir.
* **Kaynak Türü**: Belirli bir kaynak türüne dayalı bir filtre. Bu filtre de birden çok seçimdir. Örneğin, **Kaynak Türü = "Sanal Makineler"** eylem kuralının tüm sanal makineler için geçerli olduğu anlamına gelir.
* **Uyarı Kuralı Kimliği**: Uyarı kuralının Kaynak Yöneticisi Kimliğini kullanarak belirli uyarı kurallarına filtre uygulama seçeneğidir.
* **Monitör Durumu**: Monitör koşulu olarak **Ateşlenen** veya **Çözülmüş** olan uyarı örnekleri için bir filtredir.
* **Açıklaması**: Uyarı kuralının bir parçası olarak tanımlanan, açıklamaya karşı bir dize eşleşmesi tanımlayan bir regex (normal ifade) eşleşmesi. Örneğin, **Açıklama içerir 'prod'** kendi açıklamaları "prod" dizesini içeren tüm uyarıları eşleşir.
* **Uyarı Bağlamı (yük)**: Bir uyarının yükünün uyarı bağlamı alanlarına karşı dize eşleşmesi tanımlayan regex eşleşmesi. Örneğin, **Uyarı bağlamı (yük) 'Computer-01' içerir yükleri "Computer-01"** dizesini içeren tüm uyarıları eşleşir.

Bu filtreler birbiriyle birlikte uygulanır. Örneğin, **Kaynak türü' = Sanal Makineler** ve **Önem' = Sev0'** olarak ayarlarsanız, yalnızca Sanal M'lerinizdeki tüm **Sev0** uyarıları için filtre uygulanmış olursunuz. 

![Eylem kuralı filtreleri](media/alerts-action-rules/action-rules-new-rule-creation-flow-filters.png)

### <a name="suppression-or-action-group-configuration"></a>Bastırma veya eylem grubu yapılandırması

Ardından, uyarı bastırma veya eylem grubu desteği için eylem kuralını yapılandırın. İkisini birden seçemezsin. Yapılandırma, daha önce tanımlanan kapsam ve filtrelerle eşleşen tüm uyarı örneklerinde hareket eder.

#### <a name="suppression"></a>Gizleme

**Bastırma'yı**seçerseniz, eylemlerin ve bildirimlerin bastırılması için süreyi yapılandırın. Aşağıdaki seçeneklerden birini belirleyin:
* **Şu andan itibaren (Her zaman)**: Tüm bildirimleri süresiz olarak bastırır.
* **Zamanlanan bir zamanda**: Bildirimleri sınırlı bir süre içinde bastırır.
* **Yineleme ile**: Bildirimleri yinelenen günlük, haftalık veya aylık bir zamanlamada bastırır.

![Eylem kuralı bastırma](media/alerts-action-rules/action-rules-new-rule-creation-flow-suppression.png)

#### <a name="action-group"></a>Eylem grubu

Geçişte **Eylem grubunu** seçerseniz, varolan bir eylem grubu ekleyin veya yeni bir grup oluşturun. 

> [!NOTE]
> Yalnızca bir eylem grubunu eylem kuralıyla ilişkilendirebilirsiniz.

![Eylem grubu seçerek yeni eylem kuralı ekleme veya oluşturma](media/alerts-action-rules/action-rules-new-rule-creation-flow-action-group.png)

### <a name="action-rule-details"></a>Eylem kuralı ayrıntıları

Son olarak, eylem kuralı için aşağıdaki ayrıntıları yapılandırın:
* Adı
* Kaydedildiği kaynak grubu
* Açıklama 

## <a name="example-scenarios"></a>Örnek senaryolar

### <a name="scenario-1-suppression-of-alerts-based-on-severity"></a>Senaryo 1: Önem derecesine göre uyarıların bastırılması

Contoso, her hafta sonu **ContosoSub** aboneliğindeki tüm VM'ler üzerindeki tüm Sev4 uyarılarıiçin bildirimleri bastırmak istiyor.

**Çözüm:** Şularla bir eylem kuralı oluşturun:
* Kapsam = **ContosoSub**
* Filtreler
    * Önem = **Sev4**
    * Kaynak Türü = **Sanal Makineler**
* Yineleme ile bastırma haftalık olarak ayarlanır ve **Cumartesi** ve **Pazar** kontrol

### <a name="scenario-2-suppression-of-alerts-based-on-alert-context-payload"></a>Senaryo 2: Uyarıların uyarı bağlamına (yük) göre bastırılması

Contoso, **ContosoSub'da** **Bilgisayar-01** için oluşturulan tüm günlük uyarılarının bildirimlerini süresiz olarak bastırmak istiyor.

**Çözüm:** Şularla bir eylem kuralı oluşturun:
* Kapsam = **ContosoSub**
* Filtreler
    * Monitör Hizmeti = **Günlük Analizi**
    * Uyarı Bağlamı (yük) **Bilgisayar-01** içerir
* Bastırma **şimdi (Her zaman)** ayarlayın

### <a name="scenario-3-action-group-defined-at-a-resource-group"></a>Senaryo 3: Kaynak grubunda tanımlanan eylem grubu

Contoso, [abonelik düzeyinde bir metrik uyarı](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-overview#monitoring-at-scale-using-metric-alerts-in-azure-monitor)tanımlamıştır. Ancak, özellikle **contosoRG**kaynak grubundan oluşturulan uyarılar için tetikleyen eylemleri tanımlamak istiyor.

**Çözüm:** Şularla bir eylem kuralı oluşturun:
* Kapsam = **ContosoRG**
* Filtre yok
* Eylem grubu **ContosoActionGroup** olarak ayarlandı

> [!NOTE]
> *Eylem kuralları ve uyarı kuralları içinde tanımlanan eylem grupları, çoğaltma olmadan bağımsız olarak çalışır.* Daha önce açıklanan senaryoda, uyarı kuralı için bir eylem grubu tanımlanırsa, eylem kuralında tanımlanan eylem grubuyla birlikte tetiklenir. 

## <a name="managing-your-action-rules"></a>Eylem kurallarınızı yönetme

Eylem kurallarınızı liste görünümünden görüntüleyebilir ve yönetebilirsiniz:

![Eylem kuralları liste görünümü](media/alerts-action-rules/action-rules-list-view.png)

Buradan, yanlarındaki onay kutusunu seçerek ölçekteki eylem kurallarını etkinleştirebilir, devre dışı edebilir veya silebilirsiniz. Bir eylem kuralı seçtiğinizde, yapılandırma sayfası açılır. Sayfa, eylem kuralının tanımını güncelleştirmenize ve etkinleştirmenize veya devre dışı bırakmanıza yardımcı olur.

## <a name="best-practices"></a>En iyi uygulamalar

[Sonuç sayısı](alerts-unified-log.md) seçeneğiyle oluşturduğunuz günlük uyarıları, tüm arama sonucunu kullanarak tek bir uyarı örneği oluşturur (bu uyarılar birden çok bilgisayara yayılabilecek). Bu senaryoda, bir eylem kuralı **Uyarı Bağlamı (yük)** filtresini kullanıyorsa, eşleşme olduğu sürece uyarı örneğinde hareket eder. Daha önce açıklanan Senaryo 2'de, oluşturulan günlük uyarısının arama sonuçları hem **Bilgisayar-01** hem de **Computer-02**içeriyorsa, bildirimin tamamı bastırılır. **Computer-02** için oluşturulan bir bildirim yok.

![Eylem kuralları ve günlük uyarıları (sonuç sayısı)](media/alerts-action-rules/action-rules-log-alert-number-of-results.png)

İşlem kurallarıyla günlük uyarılarını en iyi şekilde kullanmak için [metrik ölçüm](alerts-unified-log.md) seçeneğine sahip günlük uyarıları oluşturun. Tanımlanan grup alanına göre bu seçenek tarafından ayrı uyarı örnekleri oluşturulur. Daha sonra, Senaryo 2'de, **Bilgisayar-01 ve Computer-02** için ayrı uyarı örnekleri oluşturulur. **Computer-02** Senaryoda açıklanan eylem kuralı nedeniyle, yalnızca **Computer-01** bildirimi bastırılır. **Computer-02** için bildirim normal olarak ateş etmeye devam ediyor.

![Eylem kuralları ve günlük uyarıları (sonuç sayısı)](media/alerts-action-rules/action-rules-log-alert-metric-measurement.png)

## <a name="faq"></a>SSS

### <a name="while-im-configuring-an-action-rule-id-like-to-see-all-the-possible-overlapping-action-rules-so-that-i-avoid-duplicate-notifications-is-it-possible-to-do-that"></a>Bir eylem kuralını yapılandırırken, yinelenen bildirimlerden kaçınmak için tüm olası çakışan eylem kurallarını görmek istiyorum. Bunu yapmak mümkün mü?

Eylem kuralını yapılandırırken bir kapsam tanımladıktan sonra, aynı kapsamda (varsa) çakışan eylem kuralları nın listesini görebilirsiniz. Bu çakışma aşağıdaki seçeneklerden biri olabilir:

* Tam eşleşme: Örneğin, tanımladığınız eylem kuralı ve çakışan eylem kuralı aynı aboneliktedir.
* Bir alt küme: Örneğin, tanımladığınız eylem kuralı bir abonelikte dir ve çakışan eylem kuralı abonelik içindeki bir kaynak grubundadır.
* Bir üst küme: Örneğin, tanımladığınız eylem kuralı bir kaynak grubundadır ve çakışan eylem kuralı kaynak grubunu içeren aboneliktedir.
* Bir kesişim: Örneğin, tanımladığınız eylem kuralı **VM1** ve **VM2'de**dir ve çakışan eylem kuralı **VM2** ve **VM3'tedir.**

![Çakışan eylem kuralları](media/alerts-action-rules/action-rules-overlapping.png)

### <a name="while-im-configuring-an-alert-rule-is-it-possible-to-know-if-there-are-already-action-rules-defined-that-might-act-on-the-alert-rule-im-defining"></a>Bir uyarı kuralını yapılandırırken, tanımladığım uyarı kuralına göre hareket edebilecek tanımlanmış eylem kuralları nın olup olmadığını bilmek mümkün mü?

Uyarı kuralınız için hedef kaynağı tanımladıktan sonra, **Eylemler** bölümünde ki **yapılandırılmış eylemleri** görüntüle'yi seçerek aynı kapsamda (varsa) hareket eden eylem kuralları listesini görebilirsiniz. Bu liste, kapsam için aşağıdaki senaryolara göre doldurulur:

* Tam eşleşme: Örneğin, tanımladığınız uyarı kuralı ve eylem kuralı aynı aboneliktedir.
* Alt küme: Örneğin, tanımladığınız uyarı kuralı bir abonelikte dir ve eylem kuralı abonelik içindeki bir kaynak grubundadır.
* Bir üst küme: Örneğin, tanımladığınız uyarı kuralı bir kaynak grubundadır ve eylem kuralı kaynak grubunu içeren aboneliktedir.
* Bir kesişim: Örneğin, tanımladığınız uyarı kuralı **VM1** ve **VM2'de**ve eylem kuralı **VM2** ve **VM3'te.**
    
![Çakışan eylem kuralları](media/alerts-action-rules/action-rules-alert-rule-overlapping.png)

### <a name="can-i-see-the-alerts-that-have-been-suppressed-by-an-action-rule"></a>Eylem kuralı tarafından bastırılan uyarıları görebilir miyim?

Uyarılar [listesi sayfasında,](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-managing-alert-instances) **Bastırma Durumu**adlı ek bir sütun seçebilirsiniz. Bir uyarı örneği için bildirim bastırıldıysa, bu durum listede gösterir.

![Bastırılmış uyarı örnekleri](media/alerts-action-rules/action-rules-suppressed-alerts.png)

### <a name="if-theres-an-action-rule-with-an-action-group-and-another-with-suppression-active-on-the-same-scope-what-happens"></a>Bir eylem grubu ve aynı kapsamda etkin bastırma etkin başka bir eylem kuralı varsa, ne olur?

Bastırma her zaman aynı kapsamda önceliklidir.

### <a name="what-happens-if-i-have-a-resource-thats-monitored-in-two-separate-action-rules-do-i-get-one-or-two-notifications-for-example-vm2-in-the-following-scenario"></a>İki ayrı eylem kuralında izlenen bir kaynağım varsa ne olur? Bir veya iki bildirim alabilir miyim? Örneğin, aşağıdaki senaryoda **VM2:**

      action rule AR1 defined for VM1 and VM2 with action group AG1
      action rule AR2 defined for VM2 and VM3 with action group AG1

VM1 ve VM3'teki her uyarı için AG1 eylem grubu bir kez tetiklenir. **VM2'deki**her uyarı için, eylem kuralları eylemleri çoğaltmadığından, eylem grubu AG1 iki kez tetiklenir. 

### <a name="what-happens-if-i-have-a-resource-monitored-in-two-separate-action-rules-and-one-calls-for-action-while-another-for-suppression-for-example-vm2-in-the-following-scenario"></a>İki ayrı eylem kuralında izlenen bir kaynağım varsa ve biri eylem çağrısı nda bulunursa, diğeri bastırma için? Örneğin, aşağıdaki senaryoda **VM2:**

      action rule AR1 defined for VM1 and VM2 with action group AG1 
      action rule AR2 defined for VM2 and VM3 with suppression

VM1'deki her uyarı için, eylem grubu AG1 bir kez tetiklenir. VM2 ve VM3'teki her uyarı için eylemler ve bildirimler bastırılır. 

### <a name="what-happens-if-i-have-an-alert-rule-and-an-action-rule-defined-for-the-same-resource-calling-different-action-groups-for-example-vm1-in-the-following-scenario"></a>Farklı eylem gruplarını çağıran aynı kaynak için tanımlanmış bir uyarı kuralı ve eylem kuralı varsa ne olur? Örneğin, aşağıdaki senaryoda **VM1:**

      alert rule rule1 on VM1 with action group AG2
      action rule AR1 defined for VM1 with action group AG1 
 
VM1'deki her uyarı için, eylem grubu AG1 bir kez tetiklenir. "Kural1" uyarı kuralı tetiklendiğinde, ag2'yi de ayrıca tetikler. Eylem kuralları ve uyarı kuralları içinde tanımlanan eylem grupları, çoğaltma olmadan bağımsız olarak çalışır. 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'daki uyarılar hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)
