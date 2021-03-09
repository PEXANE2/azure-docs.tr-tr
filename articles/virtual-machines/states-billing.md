---
title: Azure sanal makineleri için durumlar ve faturalandırma
description: Bir sanal makinenin girebileceği ve bir kullanıcının faturalandırılabileceği çeşitli durumlara genel bakış.
services: virtual-machines
author: mimckitt
ms.service: virtual-machines
ms.subservice: billing
ms.topic: conceptual
ms.date: 03/8/2021
ms.author: mimckitt
ms.reviewer: cynthn
ms.openlocfilehash: 0325dcf16c8e637a58365311a4ebd37a442d6b8c
ms.sourcegitcommit: 956dec4650e551bdede45d96507c95ecd7a01ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102522479"
---
# <a name="states-and-billing-of-azure-virtual-machines"></a>Azure sanal makineleri için durumlar ve faturalandırma

Azure sanal makineleri (VM 'Ler), *sağlama* ve *Güç* durumlarına kategorilere ayrılmamış farklı durumlara göre gider. Bu makalenin amacı, bu durumları açıklamanız ve müşterilerin örnek kullanım için faturalandırılırken özel olarak vurgulanmasını sağlamaktır. 

## <a name="get-states-using-instance-view"></a>Örnek görünümünü kullanarak durumlar alın

Örnek görünümü API 'SI, VM çalışma durumu bilgilerini sağlar. Daha fazla bilgi için bkz. [sanal makineler-örnek görüntüleme](/rest/api/compute/virtualmachines/instanceview) API 'si belgeleri.

Azure Kaynak Gezgini, VM çalıştırma durumunu görüntülemek için basit bir kullanıcı arabirimi sağlar: [Kaynak Gezgini](https://resources.azure.com/).

Sağlama durumları VM özellikleri ve örnek görünümünde görülebilir. Güç durumları VM 'nin örnek görünümünde kullanılabilir.

Aboneliğinizdeki tüm VM 'lerin güç durumunu almak için, [sanal makineler-tüm API](/rest/api/compute/virtualmachines/listall) 'Leri parametresi **statusonly** *true* olarak ayarlanmış şekilde kullanın.

> [!NOTE]
> [Sanal makineler-](/rest/api/compute/virtualmachines/listall) **yalnızca statusparameter** parametresi Ile tüm API 'lerin listesi, bir abonelikteki tüm sanal makinelerin güç durumlarını alır. Öte yandan, bazı nadir durumlarda, alma işlemindeki aralıklı sorunlar nedeniyle güç durumu kullanılamıyor olabilir. Bu gibi durumlarda, sanal makinelerinizin güç durumunu denetlemek için aynı API 'yi veya [Azure Kaynak durumu](../service-health/resource-health-overview.md) veya [Azure Kaynak grafiğini](..//governance/resource-graph/overview.md) kullanmayı yeniden denemeyi öneririz.
 
## <a name="power-states-and-billing"></a>Güç durumları ve faturalandırma

Güç durumu, sanal makinenin bilinen son durumunu temsil eder.

:::image type="content" source="./media/virtual-machines-common-states-lifecycle/vm-power-states.png" alt-text="Görüntüde, bir VM 'nin gidebileceğini güç durumlarının diyagramı gösterilmektedir. ":::

Aşağıdaki tabloda her örnek durumunun açıklaması verilmiştir ve örnek kullanım için faturalandırıldığını gösterir.

| Güç durumu | Description | Faturalandırma |  
|---|---|---|
| Başlatılıyor| Sanal makine güç. |Faturalandırılmamış * | 
| Çalışma | Sanal makine tamamen çalışıyor. Bu, standart çalışma durumudur. | IP | 
| Durduruluyor | Bu, çalıştırma ve durdurulma arasındaki geçici bir durumdur. | IP| 
|Durduruldu | Sanal makine, Konuk işletim sistemi içinden veya PowerOff API 'Leri kullanılarak kapatıldı. Bu durumda, sanal makine temel alınan donanımı kiralamaya devam etmektedir. Bu durum *durdurulmuş (ayrılmış)* olarak da adlandırılır. | IP | 
| Serbest bırakılıyor | Bu, çalıştırma ve serbest bırakılmış arasındaki geçiş durumudur. | Faturalandırılmamış * | 
| Serbest bırakıldı | Sanal makine, temel alınan donanımda kirayı serbest bıraktı ve tamamen kapatıldı. Bu durum *durdurulmuş (serbest bırakıldı)* olarak da adlandırılır. | Faturalandırılmamış * | 

[Diskler](https://azure.microsoft.com/pricing/details/managed-disks) ve [ağ](https://azure.microsoft.com/pricing/details/bandwidth/) gibi bazı Azure kaynakları &#42; ücretlendirmeye devam edecektir.


## <a name="provisioning-states"></a>Sağlama durumları

Sağlama durumu, sanal makinede Kullanıcı tarafından başlatılan bir denetim düzlemi işleminin durumudur. Bu durumlar, bir VM 'nin güç durumundan ayrıdır.

:::image type="content" source="./media/virtual-machines-common-states-lifecycle/vm-provisioning-states.png" alt-text="Görüntü, bir VM 'nin gidebileceğini sağlama durumlarını gösterir.":::

| Sağlama durumu | Description | Güç durumu | Faturalandırma | 
|---|---|---|---|
| Oluştur | Sanal makine oluşturma. | Başlatılıyor | Faturalandırılmamış * | 
| Güncelleştir | Mevcut bir sanal makinenin modelini güncelleştirir. Bir sanal makinede, başlatma ve yeniden başlatma gibi bazı model olmayan değişiklikler güncelleştirme durumunda yer almalıdır. | Çalışma | IP | 
| Sil | Sanal makine silme. | Serbest bırakılıyor | Faturalandırılmamış * |
| Serbest bırakma | Sanal makine tam olarak durdurulur ve temel alınan konaktan kaldırılır. Bir sanal makinenin serbest çıkarılması bir güncelleştirme olarak değerlendirilir ve güncelleştirme gibi sağlama durumlarının görüntülenmesini sağlar. | Serbest bırakılıyor | Faturalandırılmamış * | 

[Diskler](https://azure.microsoft.com/pricing/details/managed-disks) ve [ağ](https://azure.microsoft.com/pricing/details/bandwidth/) gibi bazı Azure kaynakları &#42; ücretlendirmeye devam edecektir.

## <a name="os-provisioning-states"></a>İşletim sistemi sağlama durumları
İşletim sistemi sağlama durumları yalnızca bir işletim sistemi görüntüsüyle oluşturulan sanal makinelere uygulanır. Bu durumlar, özelleşmiş görüntüler tarafından görüntülenmez. 

:::image type="content" source="./media/virtual-machines-common-states-lifecycle/os-provisioning-states.png" alt-text="Görüntü, bir VM 'nin gidebileceğini işletim sistemi sağlama durumlarını gösterir.":::

| İşletim sistemi sağlama durumu | Description | Güç durumu | Faturalandırma | 
|---|---|---|---|
| Osprovisioningınprogress | VM çalışıyor ve konuk işletim sistemi yüklemesi devam ediyor. | Çalışma | IP | 
| Osprovisioningtamamlanmıştır | Bu kısa süreli bir durumdur. Sanal makine, bu durumdan **başarılı** olarak hızlı bir şekilde geçiş yapar. Uzantılar hala yükleniyorsa, tamamlanana kadar bu durumu görmeye devam edersiniz. | Çalışma | IP | 
| Başarılı | Kullanıcı tarafından başlatılan eylemler tamamlandı. | Çalışma | IP | 
| Başarısız | Başarısız bir işlemi temsil eder. Daha fazla bilgi ve olası çözümler için hata koduna bakın. | Çalışma  | IP | 


## <a name="next-steps"></a>Sonraki adımlar
- [Azure maliyet yönetimi ve faturalandırma belgelerini](https://docs.microsoft.com/azure/cost-management-billing/) gözden geçirin
- Dağıtımlarınızı planlamak için [Azure Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) ' nı kullanın.
- VM 'nizi izleme hakkında daha fazla bilgi için bkz. [Azure 'da sanal makineleri izleme](../azure-monitor/insights/monitor-vm-azure.md).