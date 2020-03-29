---
title: Azure'da sanal makine ölçeği kümeleri için bakım bildirimleri
description: Azure'da bakım bildirimlerini görüntüleyin ve sanal makine ölçeği kümeleri için self servis bakıma başlayın.
author: shants123
tags: azure-service-management,azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: shants
ms.openlocfilehash: 53ebb7c4710c5455ef90701dc7e94f1b846a874a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062694"
---
# <a name="planned-maintenance-notifications-for-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri için planlı bakım bildirimleri


Azure, sanal makineler (VM) için ana bilgisayar altyapısının güvenilirliğini, performansını ve güvenliğini artırmak için düzenli aralıklarla güncelleştirmeler gerçekleştirir. Güncelleştirmeler, barındırma ortamını düzeltmeyi veya donanımı yükseltmeyi ve devre dışı bırakmayı içerebilir. Güncelleştirmelerin çoğu barındırılan VM'leri etkilemez. Ancak, güncelleştirmeler şu senaryolarda VM'leri etkiler:

- Bakım yeniden başlatma gerektirmiyorsa, Azure ana bilgisayar güncelleştirilirken VM'yi duraklatmak için yerinde geçiş kullanır. Yeniden başlatma gerektirmeyen bakım işlemleri, hata etki alanına göre hata etki alanına uygulanır. Herhangi bir uyarı durumu sinyalleri alınırsa ilerleme durdurulur.

- Bakım yeniden başlatma gerektiriyorsa, bakımın ne zaman planlandığına dikkat edin. Bu gibi durumlarda, sizin için çalıştığında, genellikle 35 gün bakım kendiniz başlayabilirsiniz bir zaman penceresi verilir.


Yeniden başlatma gerektiren planlı bakım dalgalar halinde zamanlanır. Her dalganın farklı kapsamı (bölgeleri):

- Bir dalga müşterilere bir bildirim ile başlar. Varsayılan olarak, bildirim abonelik sahibine ve ortak sahiplerine gönderilir. Azure [Etkinlik Günlüğü uyarılarını](../azure-monitor/platform/platform-logs-overview.md)kullanarak bildirimlere e-posta, SMS ve web hook gibi alıcılar ve mesajlaşma seçenekleri ekleyebilirsiniz.  
- Bildirimle birlikte, *self servis penceresi* kullanıma hazır hale getirilir. Genellikle 35 gün olan bu pencerede, hangi VM'lerinizin dalgaya dahil edildiğini bulabilirsiniz. Kendi zamanlama gereksinimlerinize göre proaktif olarak bakıma başlayabilirsiniz.
- Self servis penceresinden *sonra, zamanlanmış* bir bakım penceresi başlar. Bu pencere sırasında bir noktada, Azure VM'niz için gerekli bakımı planlar ve uygular. 

İki pencereye sahip olmanın amacı, Azure'un otomatik olarak bakıma ne zaman başlayacağını bilerek bakım başlatmak ve VM'nizi yeniden başlatmak için yeterli zaman tanımaktır.

Sanal makine ölçeği ayarlı VM'lerinizin bakım pencerelerini sorgulamak ve self servis bakıma başlamak için Azure portalını, PowerShell'i, REST API'sini ve Azure CLI'yi kullanabilirsiniz.

## <a name="should-you-start-maintenance-during-the-self-service-window"></a>Self servis sırasında bakıma başlamalı mısın?  

Aşağıdaki yönergeler, bakıma seçtiğiniz bir zamanda başlayıp başlamamaya karar vermenize yardımcı olabilir.

> [!NOTE] 
> Self servis bakım tüm VM'leriniz için kullanılamayabilir. VM'niz için proaktif yeniden dağıtmanın kullanılabilir olup olmadığını belirlemek için bakım durumunda **şimdi Başlat'ı** arayın. Şu anda Azure Bulut Hizmetleri (Web/İşçi Rolü) ve Azure Hizmet Kumaşı için self servis bakım kullanılamıyor.


*Kullanılabilirlik kümelerini*kullanan dağıtımlar için self servis bakım önerilmez. Kullanılabilirlik kümeleri, herhangi bir zamanda yalnızca bir güncelleştirme etki alanının etkilendiği yüksek kullanılabilir kurulumlardır. Kullanılabilirlik kümeleri için:

- Azure'un bakımı tetiklesin. Yeniden başlatma gerektiren bakım için, bakım güncelleştirme etki alanı tarafından güncelleştirme yapılır. Güncelleştir etki alanları, bakımı niçin sırayla alması gerekmez. Güncelleştirme etki alanları arasında 30 dakikalık bir duraklama vardır.
- Kapasitenizin bir kısmının geçici olarak kaybedilmesi (1/update etki alanı sayısı) bir sorunsa, bakım süresi boyunca ek örnekler ayırarak kaybı kolayca telafi edebilirsiniz.
- Yeniden başlatma gerektirmeyen bakım için, güncelleştirmeler hata etki alanı düzeyinde uygulanır. 
    
**Don't** Aşağıdaki senaryolarda self servis bakım kullanmayın: 

- DevTest Labs kullanarak, otomatik kapatma kullanarak veya bir zamanlamaiz izleyerek, VM'lerinizi el ile sık kapattıysanız. Bu senaryolarda self servis bakım, bakım durumunu tersine çevirebilir ve ek kapalı kalma sürelerine neden olabilir.
- Bildiğiniz kısa ömürlü VM'lerde bakım dalgası sona ermeden önce silinir. 
- Güncelleştirmeden sonra korumak istediğiniz yerel (geçici) diskte depolanan büyük bir durum la iş yükleri için. 
- VM'inizi sık sık yeniden boyutlandırın. Bu senaryo bakım durumunu geri alabilir. 
- Bakım kapatma başlamadan 15 dakika önce iş yükünüzün proaktif olarak başarısız olmasını veya zarif bir şekilde kapatılmasını sağlayan zamanlanmış olayları benimsediyseniz.

**Do** VM'inizi zamanlanan bakım aşamasında kesintisiz çalıştırmayı planlıyorsanız ve önceki karşı tendikasyonların hiçbiri geçerli değilse self servis bakım kullanın. 

Aşağıdaki durumlarda self servis bakım kullanmak en iyisidir:

- Yönetime veya müşterinize tam bir bakım penceresi iletmeniz gerekir. 
- Bakımı belirli bir tarihe kadar tamamlamanız gerekir. 
- Güvenli kurtarmayı garanti etmek için, örneğin çok katmanlı bir uygulamada bakım sırasını denetlemeniz gerekir.
- İki güncelleştirme etki alanı arasında 30 dakikadan fazla VM kurtarma süresine ihtiyacınız vardır. Güncelleştirme etki alanları arasındaki süreyi denetlemek için, VM'lerinizdeki bakımı bir defada bir güncelleştirme etki alanında tetiklemeniz gerekir.

 
## <a name="view-virtual-machine-scale-sets-that-are-affected-by-maintenance-in-the-portal"></a>Portaldaki bakımdan etkilenen sanal makine ölçek kümelerini görüntüleme

Planlanan bir bakım dalgası zamanlandığında, Azure portalını kullanarak yaklaşan bakım dalgasından etkilenen sanal makine ölçeği kümelerinin listesini görüntüleyebilirsiniz. 

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sol menüde **Tüm hizmetler'i**seçin ve ardından **Sanal makine ölçek kümelerini**seçin.
3. **Sanal makine ölçek kümeleri**altında, kullanılabilir sütunların listesini açmak için **sütunları edit'i** seçin.
4. Kullanılabilir **sütunlar** bölümünde Self **servis bakım'ı**seçin ve ardından **Seçili sütunlar** listesine taşıyın. **Uygula**’yı seçin.  

    **Self servis bakım** öğesini bulmayı kolaylaştırmak için, Kullanılabilir **sütunlar** bölümündeki açılır bırakma seçeneğini **Tümü'nden** **Özellikler'e**değiştirebilirsiniz.

**Self servis bakım** sütunu artık sanal makine ölçek kümeleri listesinde görünür. Her sanal makine ölçek kümesi self servis bakım sütunu için aşağıdaki değerlerden birine sahip olabilir:

| Değer | Açıklama |
|-------|-------------|
| Evet | Sanal makine ölçeği setinizdeki en az bir VM self servis penceresindedir. Bu self servis pencerede istediğiniz zaman bakıma başlayabilirsiniz. | 
| Hayır | Etkilenen sanal makine ölçeği kümesinde self servis pencerede hiçbir Sanal M'ler bulunmaz. | 
| - | Sanal makine ölçek kümeleriniz planlı bir bakım dalgasının parçası değildir.| 

## <a name="notification-and-alerts-in-the-portal"></a>Portaldaki bildirim ve uyarılar

Azure, abonelik sahibine ve ortak sahiplerine bir e-posta göndererek planlı bakım için bir zamanlama iletir. Etkinlik Günlüğü uyarıları oluşturarak bu iletişime alıcılar ve kanallar ekleyebilirsiniz. Daha fazla bilgi için Azure [Etkinlik Günlüğü ile abonelik etkinliğini izleyin'e](../azure-monitor/platform/platform-logs-overview.md)bakın.

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sol menüde **Monitör'ü**seçin. 
3. Monitör **- Uyarılar (klasik)** bölmesinde **+Etkinlik günlüğü uyarısı ekle'yi**seçin.
4. Etkinlik **günlüğü uyarı ekle** sayfasında, istenen bilgileri seçin veya girin. **Ölçütler'de,** aşağıdaki değerleri ayarladığınızdan emin olun:
   - **Etkinlik kategorisi**: **Servis Durumu'nun**seçin.
   - **Hizmetler**: **Sanal Makine Ölçek Setleri ve Sanal Makineleri**seçin.
   - **Türü**: **Planlı bakım**seçin. 
    
Etkinlik Günlüğü uyarılarının nasıl yapılandırılabildiğini öğrenmek için [bkz.](../azure-monitor/platform/activity-log-alerts.md)
    
    
## <a name="start-maintenance-on-your-virtual-machine-scale-set-from-the-portal"></a>Portaldan ayarlanan sanal makine ölçeğinizde bakım başlatın

Sanal makine ölçek kümelerinin genel görünümünde bakımla ilgili daha fazla ayrıntı görebilirsiniz. Sanal makine ölçeği kümesinde planlanan bakım dalgasına en az bir VM eklenirse, sayfanın üst bölümüne yeni bir bildirim şeridi eklenir. **Bakım** sayfasına gitmek için bildirim şeridini seçin. 

**Bakım** sayfasında, planlanan bakımdan hangi VM örneğinin etkilendiğini görebilirsiniz. Bakıma başlamak için, etkilenen VM'ye karşılık gelen onay kutusunu seçin. Ardından, **başlat bakım'ı**seçin.

Bakıma başladıktan sonra, sanal makine ölçeği setinizdeki etkilenen VM'ler bakımdan geçer ve geçici olarak kullanılamaz. Self servis penceresini kaçırdıysanız, sanal makine ölçek kümenizin Azure tarafından korunacağı zaman penceresini görmeye devam edebilirsiniz.
 
## <a name="check-maintenance-status-by-using-powershell"></a>PowerShell'i kullanarak bakım durumunu kontrol edin

Azure PowerShell'i kullanarak sanal makine ölçek kümelerinizdeki Sanal M'lerin bakım için ne zaman zaman zamanlandığını görebilirsiniz. Planlanan bakım `-InstanceView` bilgileri, parametreyi kullandığınızda [Get-AzVmss](https://docs.microsoft.com/powershell/module/az.compute/get-azvmss) cmdlet kullanılarak kullanılabilir.
 
Bakım bilgileri yalnızca bakım planlanırsa döndürülür. VM örneğini etkileyen bir bakım zamanlanmamışsa, cmdlet herhangi bir bakım bilgisi döndürmez. 

```powershell
Get-AzVmss -ResourceGroupName rgName -VMScaleSetName vmssName -InstanceId id -InstanceView
```

Aşağıdaki özellikler **MaintenanceRedeployStatus**altında döndürülür: 

| Değer | Açıklama   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | VM'de şu anda bakımbaşlatıp başlatamayacağınızı gösterir. |
| Ön BakımPenceresiBaşlangıç Zamanı         | VM'inizde bakım başlatabileceğiniz bakım self servis penceresinin başlangıcı. |
| Ön BakımPenceresiSon Saat           | VM'inizin bakımını başlatabileceğiniz bakım self servis penceresinin sonu. |
| MaintenanceWindowStartTime            | Azure'un VM'nizde bakım başlattığı zamanlanan bakımın başlangıcı. |
| MaintenanceWindowEndTime              | Azure'un VM'nizde bakım başlattığı bakım zamanlanan pencerenin sonu. |
| LastOperationResultCode               | VM'de bakım başlatmak için yapılan son girişimin sonucu. |



### <a name="start-maintenance-on-your-vm-instance-by-using-powershell"></a>PowerShell'i kullanarak VM örneğinizin bakımını başlatın

**IsCustomerInitiatedMaintenanceAllowed** **doğru**ayarlanmışsa bir VM'de bakım başlatabilirsiniz. Parametreli `-PerformMaintenance` [Set-AzVmss](/powershell/module/az.compute/set-azvmss) cmdlet'i kullanın.

```powershell
Set-AzVmss -ResourceGroupName rgName -VMScaleSetName vmssName -InstanceId id -PerformMaintenance 
```

## <a name="check-maintenance-status-by-using-the-cli"></a>CLI'yi kullanarak bakım durumunu kontrol edin

Az [vmss liste örneklerini](/cli/azure/vmss?view=azure-cli-latest#az-vmss-list-instances)kullanarak planlanan bakım bilgilerini görüntüleyebilirsiniz.
 
Bakım bilgileri yalnızca bakım planlanırsa döndürülür. VM örneğini etkileyen hiçbir bakım zamanlanmamışsa, komut herhangi bir bakım bilgisi döndürmez. 

```azurecli
az vmss list-instances -g rgName -n vmssName --expand instanceView
```

Aşağıdaki özellikler, her VM örneği için **MaintenanceRedeployStatus** altında döndürülür: 

| Değer | Açıklama   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | VM'de şu anda bakımbaşlatıp başlatamayacağınızı gösterir. |
| Ön BakımPenceresiBaşlangıç Zamanı         | VM'inizde bakım başlatabileceğiniz bakım self servis penceresinin başlangıcı. |
| Ön BakımPenceresiSon Saat           | VM'inizin bakımını başlatabileceğiniz bakım self servis penceresinin sonu. |
| MaintenanceWindowStartTime            | Azure'un VM'nizde bakım başlattığı zamanlanan bakımın başlangıcı. |
| MaintenanceWindowEndTime              | Azure'un VM'nizde bakım başlattığı bakım zamanlanan pencerenin sonu. |
| LastOperationResultCode               | VM'de bakım başlatmak için yapılan son girişimin sonucu. |


### <a name="start-maintenance-on-your-vm-instance-by-using-the-cli"></a>CLI'yi kullanarak VM örneğinizin bakımını başlatın

Aşağıdaki `IsCustomerInitiatedMaintenanceAllowed` **çağrı, doğru**olarak ayarlanmışsa bir VM örneğinde bakım başlatır:

```azurecli
az vmss perform-maintenance -g rgName -n vmssName --instance-ids id
```

## <a name="faq"></a>SSS

**S: Neden şimdi benim VM yeniden başlatmanız gerekiyor?**

**A:** Azure platformundaki güncelleştirmelerin ve yükseltmelerin çoğu VM kullanılabilirliğini etkilemese de, bazı durumlarda Azure'da barındırılan VM'leri yeniden başlatmaktan kaçınamayız. VM'nin yeniden başlatılmasıyla sonuçlanacak sunucularımızı yeniden başlatmamızı gerektiren birkaç değişiklik yaptık.

**S: Bir kullanılabilirlik kümesi kullanarak yüksek kullanılabilirlik için önerilerinizi uygularsam, güvende miyim?**

**A:** Kullanılabilirlik kümesinde veya sanal makine ölçeğinde dağıtılan sanal makineler güncelleştirme etki alanlarını kullanır. Azure, bakım yaparken güncelleştirme etki alanı kısıtlamasını onurlandırZ ve farklı bir güncelleştirme etki alanından (aynı kullanılabilirlik kümesi içinde) VM'leri yeniden başlatmaz. Azure, bir sonraki VM grubuna geçmeden önce en az 30 dakika bekler. 

Yüksek kullanılabilirlik hakkında daha fazla bilgi için [Azure'daki sanal makineler için Bölgeler ve kullanılabilirlik](../virtual-machines/windows/availability.md)bilgisine bakın.

**S: Planlı bakım hakkında nasıl bilgilendirilebilirim?**

**A:** Planlanmış bir bakım dalgası, bir veya daha fazla Azure bölgesine zamanlama ayarlayarak başlar. Kısa bir süre sonra, abonelik sahiplerine bir e-posta bildirimi gönderilir (abonelik başına bir e-posta). Etkinlik Günlüğü uyarılarını kullanarak bu bildirim için kanallar ve alıcılar ekleyebilirsiniz. Planlanan bakımın zaten zamanlandığı bir bölgeye bir VM dağıtırsanız, bildirimi almazsınız. Bunun yerine, VM'nin bakım durumunu denetleyin.

**S: Portalda, PowerShell'de veya CLI'de planlı bakım olduğuna dair herhangi bir belirti göremiyorum. Ne oldu?**

**A:** Planlı bakımla ilgili bilgiler, yalnızca planlanan bakımdan etkilenen VM'ler için planlanan bakım dalgası sırasında kullanılabilir. Verileri görmüyorsanız, bakım dalgası zaten bitmiş (veya başlatılmayabilir) veya VM'niz güncelleştirilmiş bir sunucuda zaten barındırılabilir.

**S: VM'imin tam olarak ne zaman etkileneceğini bilmenin bir yolu var mı?**

**A:** Zamanlamayı ayarladığımızda, birkaç günlük bir zaman penceresi tanımlarız. Sunucuların (ve VM’lerin) bu pencere içindeki sırası tam olarak bilinmez. VM'lerinizin tam olarak ne zaman güncelleştirileceğini bilmek istiyorsanız, [zamanlanmış olayları](../virtual-machines/windows/scheduled-events.md)kullanabilirsiniz. Zamanlanmış olayları kullandığınızda, VM içinden sorgu yapabilir ve VM yeniden başlatmadan önce 15 dakikalık bir bildirim alabilirsiniz.

**S: VM'mi yeniden başlatmanız ne kadar sürer?**

**A:**  VM'nizin boyutuna bağlı olarak, self servis bakım penceresi sırasında yeniden başlatma birkaç dakika kadar sürebilir. Zamanlanan bakım penceresinde Azure tarafından başlatılan yeniden başlatma sırasında, yeniden başlatma genellikle yaklaşık 25 dakika sürer. Bulut Hizmetleri (Web/İşçi Rolü), sanal makine ölçek kümeleri veya kullanılabilirlik kümeleri kullanıyorsanız, zamanlanan bakım penceresi sırasında her grup VM (etki alanını güncelleştirme) arasında 30 dakika verilir. 

**S: VM'lerimde herhangi bir bakım bilgisi görmüyorum. Ters giden neydi?**

**A:** VM'lerinizde herhangi bir bakım bilgisi göremememenizin birkaç nedeni vardır:
   - *Microsoft Dahili*olarak işaretlenmiş bir abonelik kullanıyorsunuz.
   - VM'leriniz bakım için zamanlanmadı. Bakım dalgası sona ermiş olabilir, iptal edilmiş olabilir veya VM'lerinizin bundan etkilenmeyecek şekilde değiştirilmiş olabilir.
   - VM liste görünümünüze **Bakım** sütunu eklenmez. Bu sütunu varsayılan görünüme eklememize rağmen, görünümünüzü varsayılan olmayan sütunları görecek şekilde yapılandırırsanız, **Bakım** sütununu VM liste görünümünüze el ile eklemeniz gerekir.

**S: VM'im ikinci kez bakım için zamanlandı. Neden?**

**A:** Birkaç kullanım durumunda, bakımınızı tamamladıktan ve yeniden dağıttıktan sonra VM'iniz bakım için zamanlanır:
   - Bakım dalgasını iptal ettik ve farklı bir yük ile yeniden başlattık. Hatalı bir yük tespit etmiş olabiliriz ve ek bir yük dağıtmamız yeterli.
   - VM'niz, bir donanım hatası nedeniyle başka bir düğüme *iyileşti.*
   - VM'yi durdurmayı (anlaşma yapmayı) ve yeniden başlatmayı seçtiniz.
   - VM için **otomatik kapatma** yı açtınız.

## <a name="next-steps"></a>Sonraki adımlar

[Zamanlanmış olayları](../virtual-machines/windows/scheduled-events.md)kullanarak VM içinden bakım etkinlikleri için nasıl kaydolabilirsiniz öğrenin.
