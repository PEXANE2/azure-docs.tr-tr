---
title: Azure portalında otomatik ölçek sanal makine ölçeği kümeleri
description: Azure portalında sanal makine ölçeği kümeleri için otomatik ölçek lendirme kuralları nasıl oluşturulur?
author: ju-shim
tags: azure-resource-manager
ms.assetid: 88886cad-a2f0-46bc-8b58-32ac2189fc93
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: jushiman
ms.openlocfilehash: 1915b144aec5a5447504c70d18dbf420d255a08e
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81010299"
---
# <a name="automatically-scale-a-virtual-machine-scale-set-in-the-azure-portal"></a>Azure portalında ayarlanan sanal makine ölçeğini otomatik olarak ölçeklendirin
Ölçek kümesi oluşturduğunuzda, çalıştırmak istediğiniz VM örneği sayısını tanımlarsınız. Uygulamanızın talebi değiştikçe, sanal makine örneklerinin sayısını otomatik olarak artırabilir veya azaltabilirsiniz. Otomatik ölçeklendirme özelliği, uygulamanızın yaşam döngüsü boyunca uygulama performansındaki değişikliklere veya müşteri taleplerine ayak uydurmanıza olanak tanır.

Bu makalede, Azure portalında ölçek kümenizdeki VM örneklerinin performansını izleyen otomatik ölçekkurallarının nasıl oluşturulabileceğinizgösterilmektedir. Bu otomatik ölçeklendirme kuralları, bu performans ölçümlerine yanıt olarak VM örneklerinin sayısını artırır veya azaltır. Bu adımları [Azure PowerShell](tutorial-autoscale-powershell.md) veya [Azure CLI](tutorial-autoscale-cli.md)ile de tamamlayabilirsiniz.


## <a name="prerequisites"></a>Ön koşullar
Otomatik ölçek kuralları oluşturmak için varolan bir sanal makine ölçeği kümesine ihtiyacınız var. [Azure portalı](quick-create-portal.md), [Azure PowerShell](quick-create-powershell.md)veya [Azure CLI](quick-create-cli.md)ile bir ölçek kümesi oluşturabilirsiniz.


## <a name="create-a-rule-to-automatically-scale-out"></a>Otomatik olarak ölçeklendirmek için bir kural oluşturma
Uygulamanızın talebi artarsa, ölçek kümenizdeki sanal makine örneklerinde üzerindeki yük de artar. Bu kısa süreli bir talep olmayıp tutarlı şekilde yük artıyorsa, ölçek kümesindeki sanal makine örneği sayısını artırmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Bu sanal makine örnekleri oluşturulduğunda ve uygulamalarınız dağıtıldığında ölçek kümesi, yük dengeleyici aracılığıyla bunlara trafiği dağıtmaya başlar. CPU veya disk gibi hangi ölçümlerin izleneceğini, uygulama yükünün belirli bir eşiği ne kadar süre karşılaması gerektiği ve ölçek kümesine kaç tane sanal makine örneği ekleneceğini denetlersiniz.

1. Azure portalını açın ve panonun sol tarafındaki menüden **Kaynak grupları** seçin.
2. Ölçek kümenizi içeren kaynak grubunu seçin ve ardından kaynak listesinden ölçek kümenizi seçin.
3. Ölçek kümesi penceresinin sol tarafındaki menüden **Ölçekleme'yi** seçin. **Otomatik ölçeklendirmeyi etkinleştirmek**için düğmeyi seçin:

    ![Azure portalında otomatik ölçeklendirmeyi etkinleştirme](media/virtual-machine-scale-sets-autoscale-portal/enable-autoscale.png)

4. Otomatik *ölçeklendirme*gibi ayarlarınız için bir ad girin, ardından **kural ekleme**seçeneğini seçin.

5. Ortalama CPU yükü 10 dakikalık bir süre içinde %70'ten büyük olduğunda, bir ölçek kümesindeki VM örneklerinin sayısını artıran bir kural oluşturalım. Kural tetiklendiğinde, VM örneklerinin sayısı %20 artar. Az sayıda VM örneği içeren ölçek kümelerinde, **Operation** *İşlem'i sayıyı artırmaya* ayarlayabilir ve *örnek sayısı*için *1* veya *2* belirtebilirsiniz. Çok sayıda VM örneğinin olduğu ölçek kümelerinde %10 veya %20 VM örneği daha uygun olabilir.

    Kuralınız için aşağıdaki ayarları belirtin:
    
    | Parametre              | Açıklama                                                                                                         | Değer          |
    |------------------------|---------------------------------------------------------------------------------------------------------------------|----------------|
    | *Zaman Toplama*     | Toplanan ölçümlerin analiz için nasıl bir araya getirileceğini tanımlar.                                                | Ortalama        |
    | *Metrik Adı*          | İzlenecek ve ölçek kümesi eylemlerinin uygulanmasında temel alınacak performans ölçümü.                                                   | CPU yüzdesi |
    | *Zaman dilimi istatistiği* | Her tanede toplanan ölçümlerin analiz için nasıl toplanması gerektiğini tanımlar.                             | Ortalama        |
    | *Işleç*             | Ölçüm verilerini eşikle karşılaştırmak için kullanılan işleç.                                                     | Büyüktür   |
    | *Eşik*            | Otomatik ölçek kuralının bir eylemi tetiklemesine neden olan yüzde.                                                 | 70             |
    | *Süre*             | Ölçüm ve eşik değerleri karşılaştırılmadan önce izlenecek süre.                                   | 10 dakika     |
    | *İşlem*            | Kural uygulandığında ölçek kümesinin yukarı veya aşağı ölçeklendirilip küçültülmemesi ve hangi artışla ölçeklendirilip küçültülmemesi gerektiğini tanımlar                        | Yüzde artırın |
    | *Örnek sayısı*       | Kural tetiklendiğinde değiştirilmesi gereken sanal makine örneklerinin yüzdesi.                                            | 20             |
    | *Soğuma (dakika)*  | Otomatik ölçeklendirme eylemlerinin geçerli olması için kural tekrar uygulanmadan önceki bekleme süresi. | 5 dakika      |

    Aşağıdaki örnekler, Azure portalında oluşturulan ve bu ayarlarla eşleşen bir kural gösterir:

    ![VM örneklerinin sayısını artırmak için otomatik ölçekkuralı oluşturma](media/virtual-machine-scale-sets-autoscale-portal/rule-increase.png)

6. Kuralı oluşturmak için **Ekle'yi** seçin


## <a name="create-a-rule-to-automatically-scale-in"></a>Otomatik olarak ölçeklendirecek bir kural oluşturma
Bir akşam veya hafta sonu uygulama talebiniz azalabilir. Yük belirli bir süreye yayılarak tutarlı şekilde azalıyorsa, ölçek kümesindeki sanal makine örneği sayısını azaltmak için otomatik ölçeklendirme kuralları yapılandırabilirsiniz. Mevcut talebi karşılamak için gerekli örnek sayısını yalnızca siz çalıştırdığınızdan, bu ölçeği daraltma eylemi ölçek kümenizi çalıştırma maliyetini azaltır.

1. Kuralı yeniden **eklemeyi** seçin.
2. Ortalama CPU yükü 10 dakikalık bir süre içinde %30'un altına düştüğünde, bir ölçek kümesindeki VM örneklerinin sayısını azaltan bir kural oluşturun. Kural tetiklendiğinde, VM örneklerinin sayısı %20 azalır.

    Önceki kuralla aynı yaklaşımı kullanın. Kuralınız için aşağıdaki ayarları ayarlayın:
    
    | Parametre              | Açıklama                                                                                                          | Değer          |
    |------------------------|----------------------------------------------------------------------------------------------------------------------|----------------|
    | *Işleç*             | Ölçüm verilerini eşikle karşılaştırmak için kullanılan işleç.                                                      | Küçüktür   |
    | *Eşik*            | Otomatik ölçek kuralının bir eylemi tetiklemesine neden olan yüzde.                                                 | 30             |
    | *İşlem*            | Kural uygulandığında ölçek kümesinin yukarı veya aşağı ölçeklendirilip küçültülmemesi ve hangi artışla ölçeklendirilip küçültülmemesi gerektiğini tanımlar                         | Yüzde yi azaltma |
    | *Örnek sayısı*       | Kural tetiklendiğinde değiştirilmesi gereken sanal makine örneklerinin yüzdesi.                                             | 20             |

3. Kuralı oluşturmak için **Ekle'yi** seçin


## <a name="define-autoscale-instance-limits"></a>Otomatik ölçek örnek sınırlarını tanımlama
Otomatik ölçek profiliniz en az, en büyük ve varsayılan vm örneği sayısını tanımlamalıdır. Otomatik ölçek kurallarınız uygulandığında, bu örnek sınırları en fazla örnek sayısının ötesine geçmemenizi veya en az örneği aşmadığınızdan emin olun.

1. Aşağıdaki örnek sınırlarını ayarlayın:

    | Minimum | Maksimum | Varsayılan|
    |---------|---------|--------|
    | 2       | 10      | 2      |

2. Otomatik ölçek kurallarınızı ve örnek limitlerinizi uygulamak için **Kaydet'i**seçin.


## <a name="monitor-number-of-instances-in-a-scale-set"></a>Bir ölçek kümesindeki örneklerin sayısını izleme
VM örneklerinin sayısını ve durumunu görmek için, ölçek kümesi penceresinin sol tarafındaki menüden **Örnekler'i** seçin. Durum, ölçek kümesi otomatik olarak ölçeklendikçe VM örneğinin *oluşturulup oluşturulmadı* veya ölçek otomatik olarak ölçeklendikçe *silinip* silinmediğini gösterir.

![Ölçek kümesi VM örneklerinin listesini görüntüleme](media/virtual-machine-scale-sets-autoscale-portal/view-instances.png)


## <a name="autoscale-based-on-a-schedule"></a>Zamanlamaya dayalı otomatik ölçeklendirme
Önceki örnekler, CPU kullanımı gibi temel ana bilgisayar ölçümleri ile ayarlanmış bir ölçeği otomatik olarak ölçeklendirin. Zamanlamaları temel alan otomatik ölçeklendirme kuralları da oluşturabilirsiniz. Bu zamanlama tabanlı kurallar, temel çalışma saatleri gibi uygulama talebinde beklenen artıştan önce VM örneklerinin sayısını otomatik olarak ölçeklendirmenize ve hafta sonu gibi daha az talep beklediğiniz bir anda örnek sayısını otomatik olarak ölçeklendirmenize olanak sağlar.

1. Ölçek kümesi penceresinin sol tarafındaki menüden **Ölçekleme'yi** seçin. Önceki örneklerde oluşturulan varolan otomatik ölçeklendirme kurallarını silmek için çöp kutusu simgesini seçin.

    ![Varolan otomatik ölçek kurallarını silme](media/virtual-machine-scale-sets-autoscale-portal/delete-rules.png)

2. Ölçek **koşulu eklemeyi**seçin. Kural adının yanındaki kalem simgesini seçin ve *her iş günü boyunca ölçeklendir*gibi bir ad sağlayın.

    ![Varsayılan otomatik ölçek kuralını yeniden adlandırma](media/virtual-machine-scale-sets-autoscale-portal/rename-rule.png)

3. **Belirli bir örnek sayısına ölçeklendirmek için**radyo düğmesini seçin.
4. Örnek sayısını büyütmek için örnek sayısı olarak *10* girin.
5. **Zamanlama** türü için **belirli günleri yinele'yi** seçin.
6. Pazartesi'den Cuma'ya kadar tüm iş günlerini seçin.
7. Uygun saat dilimini seçin ve ardından *09:00* **Başlangıç saati** belirtin.
8. Yeniden **ölçek koşulu eklemeyi** seçin. *Akşam saatlerinde Ölçek* adlı bir zamanlama oluşturmak için işlemi yineleyin, *3* örneğine ölçeklendirin, hafta içi her gün yinelenir ve *saat 18:00'de*başlar.
9. Zamanlama tabanlı otomatik ölçek kurallarınızı uygulamak için **Kaydet'i**seçin.

    ![Zamanlamada ölçeklendirilen otomatik ölçeklendirme kuralları oluşturma](media/virtual-machine-scale-sets-autoscale-portal/schedule-autoscale.PNG)

Otomatik ölçeklendirme kurallarınızın nasıl uygulandığını görmek için **Ölçeklendirme** penceresinin en üstündeki **Geçmiş çalıştır'ı** seçin. Grafik ve olaylar listesi, otomatik ölçek kurallarının tetiklediğinde ve ölçek kümenizdeki VM örneklerinin sayısının ne zaman arttığını veya azaldığını gösterir.


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, ölçek kümenizdeki VM örneklerinin *sayısını* yatay olarak ölçeklendirmek ve azaltmak için otomatik ölçeklendirme kurallarının nasıl kullanılacağını öğrendiniz. VM örnek *boyutunu*artırmak veya azaltmak için dikey olarak ölçeklendirebilirsiniz. Daha fazla bilgi için Sanal [Makine Ölçeği kümeleriyle Dikey otomatik ölçeklendirmeye](virtual-machine-scale-sets-vertical-scale-reprovision.md)bakın.

VM örneklerinizi nasıl yöneteceğimiz hakkında bilgi için [Azure PowerShell ile sanal makine ölçeği kümelerini yönet'e](virtual-machine-scale-sets-windows-manage.md)bakın.

Otomatik ölçeklendirme kurallarınız tetiklendiğinde nasıl uyarı oluşturacağınızı öğrenmek için Azure [Monitor'da e-posta ve webhook uyarı bildirimleri göndermek için otomatik ölçeklendirme eylemlerini kullanın'a](../azure-monitor/platform/autoscale-webhook-email.md)bakın. [Azure Monitor'da e-posta ve webhook uyarı bildirimleri göndermek için denetim günlüklerini de kullanabilirsiniz.](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
