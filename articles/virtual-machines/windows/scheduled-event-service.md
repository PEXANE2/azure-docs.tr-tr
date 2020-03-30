---
title: Windows VM'leriniz için zamanlanmış olayları Azure'da izleme
description: Zamanlanmış etkinlikler için Azure sanal makinelerinizi nasıl izleyeceğinizi öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: mysarn
manager: gwallace
ms.service: virtual-machines-windows
ms.tgt_pltfrm: vm-windows
ms.date: 08/20/2019
ms.author: sarn
ms.topic: conceptual
ms.openlocfilehash: 1cda07c18e4f5ef2a8c00b6a275f22ecc0935751
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74073306"
---
# <a name="monitoring-scheduled-events"></a>Zamanlanan Olayları İzleme

Güncelleştirmeler, hizmetlerin güvenli ve güncel çalışmasını sağlamak için her gün Azure'un farklı bölümlerine uygulanır. Planlanan güncelleştirmelere ek olarak, planlanmamış olaylar da oluşabilir. Örneğin, herhangi bir donanım bozulması veya hata algılanırsa, Azure hizmetlerinin planlanmamış bakım gerçekleştirmesi gerekebilir. Canlı geçiş, bellek koruma güncelleştirmeleri ve genellikle güncellemelerin etkisi üzerinde sıkı bir çubuk tutmak kullanarak, çoğu durumda bu olaylar müşteriler için neredeyse şeffaf ve hiçbir etkisi yoktur veya en fazla sanal makine dondurma birkaç saniye neden. Ancak, bazı uygulamalar için, sanal makine dondurma bile birkaç saniye bir etkiye neden olabilir. Bu uygulamalar için en iyi deneyimi sağlamak için yaklaşan Azure bakımı hakkında önceden bilgi almak önemlidir. [Zamanlanmış Etkinlikler hizmeti,](scheduled-events.md) yaklaşan bakım hakkında bilgilendirilmeniz için programlı bir arayüz sağlar ve bakımı zarif bir şekilde halletmenizi sağlar. 

Bu makalede, VM'lerinizi etkileyebilecek bakım olayları hakkında bilgilendirilmek üzere zamanlanmış olayları nasıl kullanabileceğinizi ve izleme ve analize yardımcı olabilecek bazı temel otomasyonları nasıl oluşturabileceğinizi göstereceğiz.


## <a name="routing-scheduled-events-to-log-analytics"></a>Zamanlanan olayları Log Analytics'e yönlendirme

Zamanlanmış Etkinlikler, her Azure sanal makinesinde kullanılabilen [Azure Örneği Meta Veri Hizmeti'nin](instance-metadata-service.md)bir parçası olarak kullanılabilir. Müşteriler, zamanlanmış bakım bildirimlerini bulmak ve durumu kaydetmek ve sanal makineyi döndürme dışında almak gibi azaltıcı etkenler gerçekleştirmek için sanal makinelerinin bitiş noktasını sorgulamak için otomasyon yazabilir. Azure bakım etkinliklerinin denetim günlüğünü yapabilmeniz için Zamanlanmış Etkinlikleri kaydetmek için bina otomasyonu öneririz. 

Bu makalede, Analitik oturum açmak için zamanlanmış etkinliklerin nasıl yakalandığını size ileteceğiz. Ardından, ekibinize e-posta göndermek ve sanal makinelerinizi etkileyen tüm olayların geçmiş görünümünü almak gibi bazı temel bildirim eylemlerini tetikleriz. Olay toplama ve otomasyon için [Log Analytics'i](/azure/azure-monitor/learn/quick-create-workspace)kullanacağız, ancak bu günlükleri toplamak ve otomasyonu tetiklemek için herhangi bir izleme çözümünü kullanabilirsiniz.

![Olay yaşam döngüsünü gösteren diyagram](./media/notifications/events.png)

## <a name="prerequisites"></a>Ön koşullar

Bu örnekte, [Kullanılabilirlik Kümesi'nde bir Windows Sanal Makine](tutorial-availability-sets.md)oluşturmanız gerekir. Zamanlanan Etkinlikler, kullanılabilirlik setinizdeki sanal makinelerden herhangi birini, Bulut Hizmeti'ni, Sanal Makine Ölçeği Kümesini veya bağımsız VM'leri etkileyebilecek değişiklikler hakkında bildirimler sağlar. Kullanılabilirlik kümesindeki diğer Tüm VM'ler için olayları almak için, toplayıcı olarak hareket edecek vm'lerden birinde planlanan etkinlikler için anket yapan bir [hizmet](https://github.com/microsoft/AzureScheduledEventsService) çalıştıracağız.    

Öğreticinin sonunda grup kaynak grubunu silmeyin.

Ayrıca, kullanılabilirlik kümesindeki VM'lerden bilgi toplamak için kullanacağımız [bir Log Analytics çalışma alanı oluşturmanız](/azure/azure-monitor/learn/quick-create-workspace) gerekir.

## <a name="set-up-the-environment"></a>Ortamı ayarlama

Şimdi bir kullanılabilirlik kümesinde 2 ilk VM olmalıdır. Şimdi aynı kullanılabilirlik kümesinde myCollectorVM adı verilen 3. 

```azurepowershell-interactive
New-AzVm `
   -ResourceGroupName "myResourceGroupAvailability" `
   -Name "myCollectorVM" `
   -Location "East US" `
   -VirtualNetworkName "myVnet" `
   -SubnetName "mySubnet" `
   -SecurityGroupName "myNetworkSecurityGroup" `
   -OpenPorts 3389 `
   -PublicIpAddressName "myPublicIpAddress3" `
   -AvailabilitySetName "myAvailabilitySet" `
   -Credential $cred
```
 

[GitHub'dan](https://github.com/microsoft/AzureScheduledEventsService/archive/master.zip)projenin yükleme .zip dosyasını indirin.

**MyCollectorVM'e** bağlanın ve .zip dosyasını sanal makineye kopyalayın ve tüm dosyaları ayıklayın. VM'nizde bir PowerShell istemi açın. İstecinizi , örneğin: `SchService.ps1` `PS C:\Users\azureuser\AzureScheduledEventsService-master\AzureScheduledEventsService-master\Powershell>`, içeren klasöre taşıyın ve hizmeti ayarlayın.

```powershell
.\SchService.ps1 -Setup
```

Hizmeti başlatın.

```powershell
.\SchService.ps1 -Start
```

Hizmet artık planlanan olaylar için her 10 saniyede bir yoklama yapmaya başlayacak ve bakımı hızlandırmak için olayları onaylayacak.  Dondurma, Yeniden Başlatma, Yeniden Dağıtma ve Preempt Zamanlama olayları tarafından yakalanan olaylardır.   Komut dosyasını, olayı onaylamadan önce bazı azaltıcı etkenleri tetiklemek üzere genişletebileceğinizi unutmayın.

Hizmet durumunu doğrulayın ve çalışırken olduğundan emin olun.

```powershell
.\SchService.ps1 -status  
```

Bu geri `Running`dönecek.

Hizmet artık planlanan olaylar için her 10 saniyede bir yoklama yapmaya başlayacak ve bakımı hızlandırmak için olayları onaylayacak.  Dondurma, Yeniden Başlatma, Yeniden Dağıtma ve Preempt Zamanlama olayları tarafından yakalanan olaylardır. Komut dosyasını, olayı onaylamadan önce bazı azaltıcı etkenleri tetiklemek üzere genişletebilirsiniz.

Yukarıdaki olaylardan herhangi biri Program Olay hizmeti tarafından yakalandığında, Uygulama Olayı Günlüğü Olay Durumu, Olay Türü, Kaynaklar (Sanal makine adları) ve NotBefore (minimum bildirim süresi) oturum açar. Uygulamaları Etkinlik Günlüğünde ID 1234 ile olayları bulabilirsiniz.

Hizmet ayarlandıktan ve başlatıldıktan sonra, olayları Windows Application günlüklerinde günlüğe kaydeder.   Bu işe yaradığını doğrulamak için, kullanılabilirlik kümesindeki sanal makinelerden birini yeniden başlatın ve Windows Günlükleri > Uygulama günlüğünde VM'nin yeniden başlatıldığını gösteren olay görüntüleyicisinde günlüğe kaydedilmiş bir olay görmeniz gerekir. 

![Olay görüntüleyicinin ekran görüntüsü.](./media/notifications/event-viewer.png)

Olaylar Program Olay hizmeti tarafından yakalandığında, uygulamada olay durumu, olay türü, kaynaklar (VM adı) ve NotBefore (minimum bildirim süresi) ile günlüğe kaydedilir. Uygulamaları Etkinlik Günlüğünde ID 1234 ile olayları bulabilirsiniz.

> [!NOTE] 
> Bu örnekte, sanal makineler, zamanlanmış olayları dinlemek ve günlük analiz çalışmaları alanımıza yönlendirmek için toplayıcı olarak tek bir sanal makine belirlememizi sağlayan bir kullanılabilirlik kümesindeydi. Bağımsız sanal makineleriniz varsa, hizmeti her sanal makinede çalıştırabilir ve bunları günlük analizi çalışma alanınıza tek tek bağlayabilirsiniz.
>
> Kurulumumuz için Windows'u seçtik, ancak Linux'ta benzer bir çözüm tasarlayabilirsiniz.

Herhangi bir noktada, anahtarları `–stop` kullanarak Zamanlanan Olay Hizmetini `–remove`durdurabilir/kaldırabilirsiniz.

## <a name="connect-to-the-workspace"></a>Çalışma alanına bağlanma


Şimdi bir Log Analytics Çalışma Alanı'nı toplayıcı VM'ye bağlamak istiyoruz. Log Analytics çalışma alanı bir depo görevi görür ve toplayıcı VM'den uygulama günlüklerini yakalamak için olay günlüğü koleksiyonunu yapılandıracağız. 

 Zamanlanan Olayları, hizmetimiz tarafından Uygulama günlüğü olarak kaydedilecek olan Etkinlik Günlüğü'ne yönlendirmek için sanal makinenizi Log Analytics çalışma alanınıza bağlamanız gerekir.  
 
1. Oluşturduğunuz çalışma alanı için sayfayı açın.
1. **Bir veri kaynağına bağlan'ın** altında Azure sanal **makinelerini (VM)** seçin.

    ![Veri kaynağı olarak VM'ye bağlanma](./media/notifications/connect-to-data-source.png)

1. **Benim CollectorVM'i**arayın ve seçin. 
1. **BenimCollectorVM**için yeni sayfada, **Connect**seçin.

Bu, [Microsoft İzleme aracısını](/azure/virtual-machines/extensions/oms-windows) sanal makinenize yükler. VM'nizi çalışma alanına bağlamak ve uzantıyı yüklemek birkaç dakika sürer. 

## <a name="configure-the-workspace"></a>Çalışma alanını yapılandırma

1. Çalışma alanınız için sayfayı açın ve **Gelişmiş ayarları**seçin.
1. Sol menüden **Veri'yi** seçin ve **ardından Windows Olay Günlükleri'ni**seçin.
1. **Aşağıdaki olay günlüklerinden**topla'da, *uygulamayı* yazmaya başlayın ve ardından listeden **Uygulama'yı** seçin.

    ![Gelişmiş ayarları seçin](./media/notifications/advanced.png)

1. **HATA**, **UYARI**ve **BİlGİ'yi** seçili bırakın ve ardından ayarları kaydetmek için **Kaydet'i** seçin.


> [!NOTE]
> Bazı gecikmeler olacaktır ve günlüğün kullanılabilir olması 10 dakika kadar sürebilir. 


## <a name="creating-an-alert-rule-with-azure-monitor"></a>Azure Monitor ile uyarı kuralı oluşturma 


Olaylar Log Analytics'e itildikten sonra, Etkinlikler çizelgesini aramak için aşağıdaki [sorguyu](/azure/azure-monitor/log-query/get-started-portal) çalıştırabilirsiniz.

1. Sayfanın üst kısmında **Günlükler'i** seçin ve aşağıdakileri metin kutusuna yapıştırın:

    ```
    Event
    | where EventLog == "Application" and Source contains "AzureScheduledEvents" and RenderedDescription contains "Scheduled" and RenderedDescription contains "EventStatus" 
    | project TimeGenerated, RenderedDescription
    | extend ReqJson= parse_json(RenderedDescription)
    | extend EventId = ReqJson["EventId"]
    ,EventStatus = ReqJson["EventStatus"]
    ,EventType = ReqJson["EventType"]
    ,NotBefore = ReqJson["NotBefore"]
    ,ResourceType = ReqJson["ResourceType"]
    ,Resources = ReqJson["Resources"]
    | project-away RenderedDescription,ReqJson
    ```

1. **Kaydet'i**seçin ve ardından ad için *logQuery* yazın, **Sorgu'yu** tür olarak bırakın, **Kategori**olarak *VMLogs* yazın ve sonra **Kaydet'i**seçin. 

    ![Sorguyu kaydetme](./media/notifications/save-query.png)

1. **Yeni uyarı kuralı**'nı seçin. 
1. Oluştur **kuralı** sayfasında Kaynak `collectorworkspace` olarak **bırakın.**
1. **Koşul**altında, *müşteri günlüğü arama olduğunda <login undefined> *girişi seçin. **Yapılaşı sinyali mantık** sayfası açılır.
1. **Eşik değerinin**altında, *0* girin ve sonra **Bitti'yi**seçin.
1. **Eylemler**altında, **eylem grubu oluştur'u**seçin. **Eylem grubu ekle** sayfası açılır.
1. **Eylem grup adı,** *myActionGroup*yazın.
1. **Kısa ad**, **myActionGroup**yazın.
1. **Kaynak grubunda** **myResourceGroupAvailability'ı**seçin.
1. Eylemler altında, **EYLEM İsİm** türü **E-posta**ve sonra **E-posta / SMS / Push / Voice**seçin. **E-posta/SMS/Push/Voice** sayfası açılacaktır.
1. **E-posta'yı**seçin, e-posta adresinizi yazın, ardından **Tamam'ı**seçin.
1. Eylem **grubu ekle** sayfasında **Tamam'ı**seçin. 
1. Create **kural** sayfasında, **ALERT DETAILS**altında, **Uyarı kural adı**için *myAlert* yazın ve ardından **Açıklama**için *E-posta uyarı kuralı* yazın.
1. İşi bittiğinde, **uyarı kuralı nı oluştur'u**seçin.
1. Kullanılabilirlik kümesindeki VM'lerden birini yeniden başlatın. Birkaç dakika içinde, uyarının tetiklendiğini belirten bir e-posta almalısınız.

Uyarı kurallarınızı yönetmek için kaynak grubuna gidin, sol menüden **Uyarılar'ı** seçin ve ardından sayfanın üst kısmından **uyarı kurallarını yönet'i** seçin.

     
## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek için GitHub'daki [Zamanlanmış olaylar hizmeti](https://github.com/microsoft/AzureScheduledEventsService) sayfasına bakın.
