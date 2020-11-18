---
title: VM'ler için Azure İzleyici Konuk sistem durumu (Önizleme)
description: Sanal makinelerinizin durumunu nasıl görüntüleyekullanabileceğinizi ve bir sanal makine sağlıksız hale geldiğinde uyarılar alabilmeniz dahil VM'ler için Azure İzleyici sistem durumu özelliğine genel bakış.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/27/2020
ms.openlocfilehash: 96bed9f3b04e54e2e9a5234d78f9a2660126742e
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94687192"
---
# <a name="azure-monitor-for-vms-guest-health-preview"></a>VM'ler için Azure İzleyici Konuk sistem durumu (Önizleme)
VM'ler için Azure İzleyici Konuk sistem durumu, Konuk işletim sisteminden düzenli aralıklarla örneklendiği performans ölçümlerinin bir kümesini temel alarak sanal makinelerin sistem durumunu görüntülemenizi sağlar. Bir abonelik veya kaynak grubundaki tüm sanal makinelerin sistem durumunu hızlı bir şekilde denetleyebilir, belirli bir sanal makinenin ayrıntılı durumunu ayrıntılandırıp bir sanal makine sağlıksız hale geldiğinde önceden bildirimde bulunabilir. 

## <a name="enable-virtual-machine-health"></a>Sanal makine sistem durumunu etkinleştir
Konuk sistem durumu özelliğinin ve ekleme sanal makinelerinin etkinleştirilmesi hakkındaki ayrıntılar için bkz. [VM'ler için Azure izleyici Konuk sistem durumunu etkinleştirme (Önizleme)](vminsights-health-enable.md) .

## <a name="pricing"></a>Fiyatlandırma
Konuk sistem durumu özelliği için doğrudan maliyet yoktur, ancak Log Analytics çalışma alanında sistem durumu verilerinin alımı ve depolanması için bir maliyet mevcuttur. Tüm veriler *Healthstatechangeevent* tablosunda depolanır. Fiyatlandırma modelleri ve maliyetler hakkında daha fazla bilgi için bkz. [Azure Izleyici günlükleriyle kullanımı ve maliyetleri yönetme](../platform/manage-cost-storage.md) .

## <a name="view-virtual-machine-health"></a>Sanal makine durumunu görüntüle
**Başlarken** SAYFASıNDAKI **Konuk VM sistem durumu** sütunu, belirli bir abonelik veya kaynak grubundaki her bir sanal makinenin sistem durumunun hızlı bir görünümünü sağlar. Her bir sanal makinenin geçerli durumu görüntülenir, her bir grubun simgeleri o gruptaki her durumda o anda sanal makine sayısını gösterir.

[![Konuk VM sistem durumu ile başlangıç sayfası](media/vminsights-health-overview/get-started-page.png)](media/vminsights-health-overview/get-started-page.png#lightbox)


## <a name="monitors"></a>İzleyiciler
İzleyicilerinin her birinin ayrıntılı durumunu görüntülemek için bir sanal makinenin sistem durumu ' na tıklayın. Her izleyici, belirli bir bileşenin sistem durumunu ölçer. Sanal makinenin genel sistem durumu, bireysel izlemelerinin sistem durumuna göre belirlenir. 

![İzleyiciler örneği](media/vminsights-health-overview/monitors.png)

Aşağıdaki tabloda, her bir sanal makine için şu anda kullanılabilen toplam ve birim izleyicileri listelenmektedir. 

| İzleyici | Tür | Açıklama |
|:---|:---|:---|
| CPU kullanımı | Birim | İşlemcinin yüzde kullanımı. |
| Dosya sistemleri | Toplama | Linux VM 'de tüm dosya sistemlerinin toplam durumunu toplayın. |
| Dosya sistemi  | Toplama | Linux VM 'de her bir dosya sisteminin sistem durumu. İzleyicinin adı dosya sisteminin adıdır. |
| Boş alan | Birim | Dosya sistemindeki boş alan yüzdesi. |
| Mantıksal diskler | Toplama | Windows VM 'deki tüm mantıksal disklerin toplam durumu. |
| Mantıksal disk  | Toplama | Windows VM 'de tek bir mantıksal diskin sistem durumu. İzleyicinin adı, diskin adıdır. |
| Bellek | Toplama | VM üzerindeki belleğin toplam durumu. |
| Kullanılabilir bellek | Birim | VM 'de kullanılabilir megabayt. |

## <a name="health-states"></a>Sağlık durumları
Her izleyici, aracıdaki her dakikada bir değer örneklenir ve örneklenmiş değerleri her sistem durumu ölçütlerine göre karşılaştırır. Belirli bir durum için ölçüt doğruysa, izleyici bu duruma ayarlanır. Ölçütlerden hiçbiri doğru değilse, izleyici sağlıklı bir duruma ayarlanır. Veriler aracıdan Azure Izleyici 'ye üç dakikada bir veya bir durum değişikliği olduğunda anında gönderilir.

Her monitörün geriye doğru izleme penceresi bulunur ve bu süre içinde toplanan tüm örnekleri analiz eder. Örneğin, bir izleyici, en az 2 örneklenmiş değer olan ancak son 3 ' ten fazla olan maksimum değeri bulmak için 240 saniyelik geriye doğru izleme penceresine sahip olabilir. Değerler düzenli bir hızda örneklenirken, belirli bir pencerede örneklenmiş sayı, aracı işleminde herhangi bir kesinti olması durumunda biraz farklılık gösterebilir.

İzleyicilerin her biri, aşağıdaki tabloda yer aldığı olası sistem durumlarına sahiptir ve belirli bir zamanda tek bir ve yalnızca biri olacaktır. Bir izleyici başlatıldığında sağlıklı bir durumda başlar.

| Sistem Durumu | Açıklama |
|:---|:---|
| Sağlam  | İzleyici Şu anda uyarı veya kritik eşiği aşmıyor. |
| Uyarı  | İzleyici, uyarı eşiğini (tanımlanmışsa) aştı. |
| Kritik | İzleyici kritik eşiği (tanımlanmışsa) aştı. |
| Bilinmiyor  | Sistem durumunu belirlemekte yeterli veri toplanmadı. |
| Devre dışı | İzleyici Şu anda devre dışı. |
| Yok     | İzleyici yeni başlatılmış ve henüz değerlendirilmedi ya da izlenen nesne artık yok. |



Aşağıdaki tabloda gösterildiği gibi iki tür izleyici vardır.

| İzleyici | Açıklama |
|:---|:---|
| Birim izleyicisi | Bir kaynağın veya uygulamanın bazı yönlerini ölçer. Bu, kaynağın performansını veya kullanılabilirliğini belirlemede bir performans sayacı denetleniyor olabilir. |
| Toplam Izleyici | Tek bir toplu sistem durumu sağlamak için birden çok izleyici gruplandırır. Bir toplam izleyici, bir veya daha fazla birim İzleyicisi ve diğer toplama izleyicileri içerebilir. |


  
### <a name="health-rollup-policy"></a>Sistem durumu toplama ilkesi
Bir sanal makinenin sistem durumu, birim ve toplam izleyicilerinden her birinin sistem durumu toplamasına göre belirlenir. Her toplam izleyici, toplam İzleyici durumunun alt izleyici durumuyla en kötü sistem durumu ile eşleştiği bir en kötü durum durumu toplama İlkesi kullanır.  

Aşağıdaki örnekte, **boş alan** İzleyicisi, örnek için, ve daha sonra **dosya sistemlerine** yönelik olan toplamları içeren kritik bir durumdur. Bir uyarı durumunda **CPU kullanımı** olsa da, bunun altında en kötü durum olduğu için sanal makine kritik olarak ayarlanır. Boş alan sağlıklı bir duruma geri döneceden sonra, CPU kullanımı en kötü duruma sahip olan izleyici olacak şekilde, sanal makine bir uyarı durumuna değişir.

![Sistem durumu toplama örneği](media/vminsights-health-overview/health-rollup-example.png)


## <a name="monitor-details"></a>İzleyici ayrıntıları
Aşağıdaki sekmeleri içeren ayrıntılarını görüntülemek için bir izleyici seçin.

**Genel bakış** , izleyicinin açıklamasını, son değerlendirilme zamanını ve geçerli sistem durumunu belirlemekte kullanılan değerleri sağlar. Örneklerin sayısı, izleyicinin tanımına ve değerlerin volaisine göre değişiklik gösterebilir.

[![İzleme ayrıntılarına genel bakış](media/vminsights-health-overview/monitor-details-overview.png)](media/vminsights-health-overview/monitor-details-overview.png#lightbox)

**Geçmiş** , izleyici için durum değişikliklerinin geçmişini listeler. Sistem durumunu belirlemekte değerlendirilen değerleri görüntülemek için durum değişikliklerinden herhangi birini genişletebilirsiniz. Sistem durumunun değiştirildiği sırada izleyicinin yapılandırmasını görüntülemek için **Görünüm uygulanmış yapılandırma** ' ya tıklayın.



[![İzleme ayrıntıları geçmişi](media/vminsights-health-overview/monitor-details-history.png)](media/vminsights-health-overview/monitor-details-history.png#lightbox)

### <a name="configuration"></a>Yapılandırma
Seçili VM için izleyicinin yapılandırmasını görüntüleyin ve değiştirin. Ayrıntılar için bkz. [VM'ler için Azure izleyici Konuk sistem durumu 'nda Izlemeyi yapılandırma (Önizleme)](vminsights-health-enable.md) .

[![İzleme ayrıntıları yapılandırması](media/vminsights-health-overview/monitor-details-configuration.png)](media/vminsights-health-overview/monitor-details-configuration.png#lightbox)




## <a name="next-steps"></a>Sonraki adımlar

- [VM'ler için Azure İzleyici ve aracılarda Konuk sistem durumunu etkinleştirin.](vminsights-health-enable.md)
- [Azure portal kullanarak izleyicileri yapılandırın.](vminsights-health-configure.md)
- [Veri toplama kurallarını kullanarak izleyicileri yapılandırın.](vminsights-health-configure-dcr.md)