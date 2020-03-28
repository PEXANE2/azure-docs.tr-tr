---
title: Öğretici - Azure'da Windows sanal makine yapılandırması yönetme
description: Bu eğitimde, windows sanal makinesinde değişiklikleri nasıl tanımlayabileceğinizi ve paket güncelleştirmelerini nasıl yöneteceklerini öğreneceksiniz
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/05/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: d97323f1916ee46e6b1f8d4ca8723b950baca39c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79238583"
---
# <a name="tutorial-monitor-changes-and-update-a-windows-virtual-machine-in-azure"></a>Öğretici: Azure'da değişiklikleri izleyin ve Windows sanal makinesini güncelleştirin

Azure [Değişiklik İzleme](../../automation/change-tracking.md) ve [Güncelleştirme Yönetimi](../../automation/automation-update-management.md)ile Azure'daki Windows sanal makinelerinizdeki değişiklikleri kolayca tanımlayabilir ve bu Sanal Makineler için işletim sistemi güncelleştirmelerini yönetebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Windows güncelleştirmelerini yönetin.
> * Değişiklikleri ve envanteri izleyin.

## <a name="open-azure-cloud-shell"></a>Azure Cloud Shell’i açma

Azure Bulut Kabuğu, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Azure hesabınızla kullanmak üzere önceden yüklenmiş ve yapılandırılan ortak Azure araçları vardır.

Cloud Shell'deki herhangi bir kod bloğunu açmak için, kod bloğunun sağ üst köşesinden **deneyin'i** seçmenin tek yolu.

Ayrıca cloud shell'i ayrı bir tarayıcı [https://shell.azure.com/powershell](https://shell.azure.com/powershell)sekmesinde açabilirsiniz. Kod **Copy** bloklarını kopyalamak için Kopyala'yı seçin, Bulut Kabuğu sekmesine yapıştırın ve kodu çalıştırmak için Enter tuşunu seçin.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Bu öğreticide Azure izlemesi ve güncelleştirme yönetimini yapılandırmak için, Azure'da bir Windows VM'sine ihtiyacınız vardır.

İlk olarak, VM için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```

Ardından, [Yeni-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm)ile VM oluşturun. Aşağıdaki örnek, `myVM` `East US` konumda bir VM oluşturur. Bunlar zaten yoksa, kaynak grubu `myResourceGroupMonitor` ve destekleyici ağ kaynakları oluşturulur:

```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroupMonitor" `
    -Name "myVM" `
    -Location "East US" `
    -Credential $cred
```

Kaynakların ve sanal makinenin oluşturulması birkaç dakika sürer.

## <a name="manage-windows-updates"></a>Windows güncelleştirmelerini yönetme

Güncelleştirme Yönetimi, Azure Windows VM'leriniz için güncelleştirmeleri ve yamaları yönetmenize yardımcı olur. Doğrudan VM'nizden:

- Kullanılabilir güncelleştirmelerin durumunu değerlendirme.
- Gerekli güncelleştirmeleri yüklemeyi zamanlama.
- Güncelleştirmelerin VM'ye başarıyla uygulandığını doğrulamak için dağıtım sonuçlarını gözden geçirin.

Fiyatlandırma bilgileri [için, Güncelleştirme yönetimi için Otomasyon fiyatlandırması'na](https://azure.microsoft.com/pricing/details/automation/)bakın.

### <a name="enable-update-management"></a>Güncelleştirme Yönetimi’ni etkinleştirme

VM'iniz için Güncelleştirme Yönetimini etkinleştirmek için:

1. Pencerenin en sol tarafında **Sanal makineleri**seçin.
1. Listeden bir VM seçin.
1. VM penceresinin **İşlemler** bölmesinde, **Update yönetimini**seçin.
1. **Güncelleştirme Yönetimini Etkinleştir** penceresi açılır.

Doğrulama, bu VM için Güncelleştirme Yönetimi etkin olup olmadığını belirlemek için yapılır. Doğrulama, log analitiği çalışma alanı, bağlantılı bir Otomasyon hesabı ve çözümün çalışma alanında olup olmadığını kontrol eder.

Güncelleştirme Yönetimi gibi özellikler ve hizmetler tarafından oluşturulan verileri toplamak için bir [Günlük Analizi](../../log-analytics/log-analytics-overview.md) çalışma alanı kullanırsınız. Çalışma alanı, birden fazla kaynaktan alınan verilerin incelenip analiz edilebileceği ortak bir konum sağlar.

VM'lerde güncelleştirme gerektiren ek eylemler gerçekleştirmek için, runbook'ları VM'lere karşı çalıştırmak için Azure Otomasyonu'nu kullanabilirsiniz. Bu tür eylemler, güncelleştirmeleri indirmeyi veya uygulamayı içerir.

Doğrulama işlemi, VM'nin Microsoft İzleme Aracısı (MMA) ve Otomasyon Karma Runbook Worker ile sağlanıp sağlanmadÝðýný da denetler. Aracıyı VM ile iletişim kurmak ve güncelleştirme durumu hakkında bilgi almak için kullanırsınız.

Güncelleştirme **Yönetimini Etkinleştir** penceresinde, Günlük Analizi çalışma alanı ve otomasyon hesabını seçin ve ardından **Etkinleştir'i**seçin. Çözümün etkin hale gelmesi 15 dakika kadar sürer.

Uçağa binme sırasında eksik olan aşağıdaki ön koşullar otomatik olarak eklenir:

* [Günlük Analytics](../../log-analytics/log-analytics-overview.md) çalışma alanı
* [Otomasyon](../../automation/automation-offering-get-started.md)
* VM'de etkinleştirilen [Karma çalışma kitabı çalışanı](../../automation/automation-hybrid-runbook-worker.md)

Çözüm etkinleştirildikten sonra **Güncelleştirme yönetimi** penceresi açılır. Konumu, Log Analytics çalışma alanını ve Otomasyon hesabını kullanmak üzere yapılandırın ve ardından **Etkinleştir'i**seçin. Bu seçenekler soluk görünüyorsa, VM için başka bir otomasyon çözümü etkinleştirilir ve bu çözümün çalışma alanı ve Otomasyon hesabı kullanılmalıdır.

![Güncelleştirme Yönetimi çözümlerini etkinleştirin](./media/tutorial-monitoring/manageupdates-update-enable.png)

Güncelleştirme Yönetimi çözümemi etkinleştirmek 15 dakika kadar sürebilir. Bu süre boyunca tarayıcı penceresini kapatmayın. Çözüm etkinleştirildikten sonra, VM akışlarındaki eksik güncelleştirmelerle ilgili bilgiler Azure Monitor günlüklerine akar. Verilerin analiz edilebis olması 30 dakika ile 6 saat arasında sürebilir.

### <a name="view-an-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

Güncelleştirme Yönetimi etkinleştirildikten sonra **Güncelleştirme yönetimi** penceresi görüntülenir. Güncelleştirmelerin değerlendirilmesi tamamlandıktan sonra, **Eksik güncelleştirmeler** sekmesinde eksik güncelleştirmelerin listesini görürsünüz.

 ![Güncelleştirme durumunu görüntüleme](./media/tutorial-monitoring/manageupdates-view-status-win.png)

### <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi izleyen bir dağıtım zamanlayın. Dağıtıma hangi güncelleştirme türlerinin ekeceğini siz seçersiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

VM için yeni bir güncelleştirme dağıtımı zamanlamak için, **Güncelleştirme yönetimi** penceresinin üst kısmında **ki Güncelleştir dağıtımını Zamanla'yı** seçin. Yeni **güncelleştirme dağıtım** penceresinde aşağıdaki bilgileri belirtin:

| Seçenek | Açıklama |
| --- | --- |
| **Adı** |Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad girin. |
|**İşletim sistemi**| **Linux** veya **Windows'u**seçin.|
| **Güncelleştirilen gruplar** |Azure'da barındırılan VM'ler için, abonelik, kaynak grupları, konumlar ve etiketlerin birleşimini temel alan bir sorgu tanımlayın. Bu sorgu, dağıtımınıza dahil etmek üzere Azure tarafından barındırılan dinamik bir VM grubu oluşturur. </br></br>Azure'da barındırılmayan VM'ler için varolan kaydedilmiş bir aramayı seçin. Bu aramada, dağıtıma dahil etmek üzere bu VM'lerden oluşan bir grup seçebilirsiniz. </br></br> Daha fazla bilgi için [Dinamik Gruplar'a](../../automation/automation-update-management-groups.md)bakın.|
| **Güncelleştirilen makineler** |**Kaydedilen arama,** **Alınan grup**veya **Makineler'i**seçin.<br/><br/>**Makineler'i**seçerseniz, açılan listeden tek tek makineleri seçebilirsiniz. Her makinenin hazır olması tablonun **UPDATE AGENT READINESS** sütununda gösterilir.</br></br> Azure Monitor günlüklerinde bilgisayar grupları oluşturmanın farklı yöntemleri hakkında bilgi edinmek için [Azure Monitor günlüklerinde Bilgisayar gruplarına](../../azure-monitor/platform/computer-groups.md) bakın |
|**Sınıflandırmaları güncelleştir**|Gerekli tüm güncelleştirme sınıflandırmalarını seçin.|
|**Güncelleştirmeleri ekleme/hariç tutma**|**Ekle/Hariç tut** bölmesini açmak için bu seçeneği belirleyin. Eklenecek ve hariç tutulacak güncelleştirmeler ayrı sekmelerde dir. İçermenin nasıl işleyiş işleyiş yaptığı hakkında daha fazla bilgi için [bkz.](../../automation/automation-tutorial-update-management.md#schedule-an-update-deployment) |
|**Zamanlama ayarları**|Başlamak için zaman seçin ve **Bir kez** veya **Yinelenen**seçin.|
| **Ön komut dosyaları + Post-scriptler**|Dağıtımdan önce ve sonra çalışacak komut dosyalarını seçin.|
| **Bakım penceresi** | Güncelleştirmeler için ayarlanan dakika sayısını girin. Geçerli değerler 30 ile 360 dakika arasında değişir. |
| **Yeniden başlatma denetimi**| Yeniden başlatmaların nasıl işleneceğini seçin. Kullanılabilir seçimler şunlardır:<ul><li>**Gerekirse yeniden başlatın**</li><li>**Her zaman yeniden başlat**</li><li>**Hiçbir zaman yeniden başlatma**</li><li>**Yalnızca yeniden başlatma**</li></ul>**Gerekirse** yeniden başlatma varsayılan seçimdir. **Yalnızca yeniden başlatmayı**seçerseniz, güncelleştirmeler yüklenmiyor.|

Zamanlamayı yapılandırmayı bitirdikten sonra durum panosuna dönmek için **Oluştur'u** tıklatın. **Zamanlanan** tablo oluşturduğunuz dağıtım zamanlamasını gösterir.

Güncelleştirme dağıtımlarını programlı olarak da oluşturabilirsiniz. REST API ile nasıl bir güncelleştirme dağıtımı oluşturabilirsiniz öğrenmek için Bkz. [Yazılım Güncelleştirme Yapılandırmaları - Oluştur](/rest/api/automation/softwareupdateconfigurations/create). Ayrıca, haftalık güncelleştirme dağıtımı oluşturmak için kullanabileceğiniz örnek bir runbook da vardır. Bu runbook hakkında daha fazla bilgi edinmek [için](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1)bkz.

### <a name="view-results-of-an-update-deployment"></a>Güncelleştirme dağıtımının sonuçlarını görüntüleme

Zamanlanan dağıtım başladıktan sonra, **Güncelleştirme yönetimi** penceresinin **Güncelleştirme dağıtımları** sekmesinde dağıtım durumunu görebilirsiniz.

Dağıtım şu anda çalışıyorsa, durumu "Devam ediyor" olarak gösterir. Başarılı bir şekilde tamamlandıktan sonra durum "Başarılı" olarak değişir. Ancak dağıtımdaki herhangi bir güncelleştirme başarısız olursa, durum "Kısmen başarısız olur."

Bu dağıtımiçin panoyu görmek için tamamlanmış güncelleştirme dağıtımını seçin.

![Belirli bir dağıtım için Güncelleştirme Dağıtımı durum panosu](./media/tutorial-monitoring/manageupdates-view-results.png)

**Güncelleştirme sonuçları** döşemesi, VM'deki toplam güncelleştirme ve dağıtım sonuçlarının bir özetini gösterir. Sağdaki tablo, her güncelleştirmenin ve yükleme sonuçlarının ayrıntılı bir dökümünü gösterir. Her sonuç aşağıdaki değerlerden birine sahiptir:

* **Denenmedi**: Güncelleştirme yüklü değil. Tanımlanan bakım penceresi süresine bağlı olarak yeterli zaman yoktu.
* **Başarılı**: Güncelleştirme başarılı oldu.
* **Başarısız**: Güncelleştirme başarısız oldu.

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm günlükler**’i seçin.

Hedef VM'de güncelleştirme dağıtımını yönetmekten sorumlu runbook'un iş akışını görmek için **Çıktı** döşemesini seçin.

Dağıtım hataları hakkında ayrıntılı bilgi görmek için **Hatalar'ı** seçin.

## <a name="monitor-changes-and-inventory"></a>Değişiklikleri ve sayımı izleme

Bilgisayarlarınızda yazılım, dosya, Linux daemons, Windows hizmetleri ve Windows kayıt defteri anahtarlarının envanterini toplayabilir ve görüntüleyebilirsiniz. Makinelerinizin yapılandırmalarını izlemek, ortamınızdaki operasyonel sorunları belirlemenize ve makinelerinizin durumunu daha iyi anlamanıza yardımcı olur.

### <a name="enable-change-and-inventory-management"></a>Değişim ve stok yönetimini etkinleştirme

VM'iniz için değişim ve stok yönetimini etkinleştirmek için:

1. Pencerenin en sol tarafında **Sanal makineleri**seçin.
1. Listeden bir VM seçin.
1. VM penceresindeki **İşlemler** altında Stok **veya** **Değişiklik izlemeyi**seçin.
1. **Değişim İzlemesini Etkinleştir ve Stok** bölmesi açılır.

Konumu, Günlük Analitiği çalışma alanını ve Otomasyon hesabını kullanmak üzere yapılandırın ve ardından **Etkinleştir'i**seçin. Seçenekler soluk görünüyorsa, VM için bir otomasyon çözümü zaten etkinleştirilir. Bu durumda, zaten etkin leştirilmiş çalışma alanı ve Otomasyon hesabı kullanılmalıdır.

Çözümler menüde ayrı ayrı görünse de, aynı çözümdür. Biri etkinleştirildiğinde, sanal makinenizde her ikisi de etkinleştirilir.

![Değişiklik ve Sayım izlemeyi etkinleştirme](./media/tutorial-monitoring/manage-inventory-enable.png)

Çözüm etkinleştirildikten sonra, verilerin görünmesi için envanterin VM'de toplanması biraz zaman alabilir.

### <a name="track-changes"></a>Değişiklikleri izleme

**OPERATIONS**altındaki VM'nizde **İzlemeyi Değiştir'i** seçin ve ardından **Ayarları Edit'i**seçin. **Değişiklik İzleme** bölmesi açılır. İzlemek istediğiniz ayar türünü seçin ve **+ Ekle**’yi seçerek ayarları yapılandırın.

Windows için kullanılabilir ayarlar şunlardır:

* Windows Kayıt Defteri
* Windows Dosyaları

İzlemeyi Değiştir hakkında ayrıntılı bilgi için [VM'deki Sorun Giderme değişikliklerine](../../automation/automation-tutorial-troubleshoot-changes.md)bakın.

### <a name="view-inventory"></a>Sayımı görüntüleme

Sanal makinenizde **İŞLEMLER** bölümünden **Sayım**’ı seçin. **Yazılım** sekmesinde, bulunan yazılımı gösteren bir tablo vardır. Her yazılım kaydının üst düzey ayrıntıları tabloda görünür. Bu ayrıntılar, yazılım adını, sürümü, yayımcıyı ve son yenilenen zamanı içerir.

![Sayımı görüntüleme](./media/tutorial-monitoring/inventory-view-results.png)

### <a name="monitor-activity-logs-and-changes"></a>Etkinlik günlüklerini ve değişiklikleri izleme

VM'nizdeki **İzlemeYi Değiştir** penceresinden **Azure Etkinliği günlük** bölmesini açmak için Etkinlik Günlüğü Bağlantısını **Yönet'i** seçin. VM'iniz için Değişiklik İzleme'yi Azure etkinlik günlüğüne bağlamak için **Bağlan'ı** seçin.

Değişiklik İzleme etkinleştirildikten sonra, VM'niz için **Genel Bakış** bölmesine gidin ve VM'nizi durdurmak için **Durdur'u** seçin. Sorulduğunda **Evet**'i seçerek VM'yi durdurun. VM ayrıldıktan sonra, VM'nizi yeniden başlatmak için **Başlat'ı** seçin.

Bir VM'yi durdurma ve yeniden başlatma, etkinlik günlüğünde bir olayı günlüğe kaydeder. **İzleme** bölmesine geri dön ve bölmenin altındaki **Olaylar** sekmesini seçin. Bir süre sonra, olaylar grafikte ve tabloda görünür. Bu olay için ayrıntılı bilgileri görüntülemek için her olayı seçebilirsiniz.

![Etkinlik günlüğünde değişiklikleri görüntüleme](./media/tutorial-monitoring/manage-activitylog-view-results.png)

Önceki grafik, zaman içinde gerçekleşen değişiklikleri gösterir. Azure Etkinlik Günlüğü bağlantısı ekledikten sonra, üstteki çizgi grafiği Azure Etkinlik Günlüğü etkinliklerini görüntüler.

Çubuk grafiklerin her satırı farklı bir izlenebilir değişiklik türünü temsil eder. Bu tür Linux daemons, dosyalar, Windows kayıt defteri anahtarları, yazılım ve Windows hizmetleri vardır. **Değiştir** sekmesi değişiklik ayrıntılarını gösterir. Değişiklikler, her birinin oluştuğu sırada görünür ve en son değişiklik önce gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, VM'iniz için Değişiklik İzleme ve Güncelleştirme Yönetimi'ni yapılandırmış ve gözden geçirmişsiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Bir kaynak grubu ve VM oluşturun.
> * Windows güncelleştirmelerini yönetin.
> * Değişiklikleri ve envanteri izleyin.

VM'nizi izleme hakkında bilgi edinmek için bir sonraki öğreticiye gidin.

> [!div class="nextstepaction"]
> [Sanal makineleri izleme](tutorial-monitor.md)
