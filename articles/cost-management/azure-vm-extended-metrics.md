---
title: Azure sanal makineleri için genişletilmiş ölçümler ekleme | Microsoft Docs
description: Bu makalede etkinleştirmek ve Azure sanal makineleriniz için genişletilmiş bir tanılama ölçümünü yapılandırmanıza yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
manager: vitavor
ms.author: banders
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: cost-management-billing
ms.custom: seodec18
ms.openlocfilehash: ebbdd89d3ef41d4fb40197cbd83038b5cbc02073
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74230154"
---
# <a name="add-extended-metrics-for-azure-virtual-machines"></a>Azure sanal makineleri için genişletilmiş ölçümler ekleme

Cloudyn Azure vm'lerinizden Azure ölçüm veri kaynakları hakkında bilgi ayrıntılı göstermek için kullanır. Ölçüm verilerini, performans sayaçları, olarak da bilinir, Cloudyn tarafından raporlar oluşturmak için kullanılır. Bununla birlikte, Cloudyn otomatik olarak tüm Azure ölçüm verileri Konuk Vm'lerden toplamak — ölçüm toplama etkinleştirmeniz gerekir. Bu makalede, Azure sanal makineleriniz için ek tanılama ölçümünü yapılandırmak ve etkinleştirmek yardımcı olur.

Ölçüm toplama etkinleştirdikten sonra şunları yapabilirsiniz:

- Vm'lerinizi, bellek, disk ve CPU sınırlarını ulaşma ne zaman bilirsiniz.
- Kullanım eğilimleri ve anormallikleri algılar.
- Boyutlandırma kullanım göre maliyetlerinizi kontrol.
- Maliyet etkin boyutlandırma Cloudyn iyileştirme önerileri alın.

Örneğin, Azure sanal makinelerinizin bellek % ve % CPU izlemek isteyebilirsiniz. Azure VM ölçümleri, kullanım _YÜZDESI CPU_ ve _\Bellek\% kaydedilmiş bayt miktarı_' na karşılık gelir.

> [!NOTE]
> Genişletilmiş ölçüm verileri toplama yalnızca Azure Konuk düzeyinde izlemeyi ile desteklenir. Cloudyn [Log Analytics aracısıyla](../azure-monitor/platform/agents-overview.md)uyumlu değildir. 

## <a name="determine-whether-extended-metrics-are-enabled"></a>Genişletilmiş ölçümler etkinleştirilip etkinleştirilmediğini belirleme

1. https://portal.azure.com adresinden Azure portalında oturum açın.
2. **Sanal makineler**altında bir VM seçip **izleme**altında **ölçümler**' i seçin. Kullanılabilir ölçümler bir listesi gösterilir.
3. Bazı ölçümler seçin ve bunlar için verileri bir grafiği görüntüler.  
    ![Örnek ölçüm – konak CPU yüzdesi](./media/azure-vm-extended-metrics/metric01.png)

Yukarıdaki örnekte, sınırlı sayıda standart ölçüm konaklarınız için kullanılabilir, ancak bellek ölçümleri değil. Bellek ölçümleri genişletilmiş ölçümler bir parçasıdır. Bu durumda, sanal makine için genişletilmiş ölçümler etkin değil. Genişletilmiş ölçümleri etkinleştirmek üzere bazı ek adımlar gerçekleştirmeniz gerekir. Aşağıdaki bilgiler, etkinleştirme işleminde size yol gösterir.

## <a name="enable-extended-metrics-in-the-azure-portal"></a>Azure portalında genişletilmiş ölçümlerini etkinleştir

Standart ölçümler, ana bilgisayar ölçümleridir. _CPU Ölçümü yüzdesi_ bir örnektir. Genişletilmiş ölçümler de adlandırılırlar ve Konuk sanal makineler için temel ölçümleri de vardır. Genişletilmiş ölçümlere örnek olarak, _kullanılmakta olan \ bellek\% kaydedilmiş bayt_ ve _\Bellek\kullanılabilir bayt_verilebilir.

Genişletilmiş ölçümlerini etkinleştirme oldukça basittir. Her VM için konuk düzeyinde izlemeyi etkinleştir. Konuk düzeyinde izlemeyi etkinleştirdiğinizde, Azure tanılama aracısını sanal makinede yüklü. Varsayılan olarak, genişletilmiş ölçümleri temel bir dizi eklenir. Aşağıdaki normal ve klasik VM'ler için aynı ve Windows ve Linux Vm'leri için aynı işlemidir.

Bir depolama hesabı hem Azure hem de Linux Konuk düzeyinde izlemeyi gerektiğini aklınızda bulundurun. Daha sonra mevcut bir depolama hesabı seçmezseniz, Konuk düzeyinde izlemeyi etkinleştirdiğinizde, bir sizin için oluşturulur.

### <a name="enable-guest-level-monitoring-on-existing-vms"></a>Var olan Vm'lerde Konuk düzeyinde izlemeyi etkinleştir

1. **Sanal makinelerde**VM 'nizin listesini görüntüleyin ve ardından bir VM seçin.
2. **İzleme**altında **Tanılama ayarları**' nı seçin.
3. Tanılama Ayarları sayfasında, **Konuk düzeyinde Izlemeyi etkinleştir**' e tıklayın.  
    Genel Bakış sayfasında Konuk düzeyinde izlemeyi etkinleştirmek ![](./media/azure-vm-extended-metrics/enable-guest-monitoring.png)
4. Birkaç dakika sonra Azure tanılama aracısını sanal makinede yüklü. Temel ölçümler birtakım eklenir. Sayfayı yenileyin. Ek performans sayaçları genel bakış sekmesinde görünür.
5. Izleme altında **ölçümler**' i seçin.
6. **Ölçüm ad alanı**altındaki ölçümler grafiğinde **Konuk (klasik)** seçeneğini belirleyin.
7. Ölçüm listesinde, tüm performans sayaçlarının Konuk VM görüntüleyebilirsiniz.  
    ![Genişletilmiş örnek ölçümlerin listesi](./media/azure-vm-extended-metrics/extended-metrics.png)

### <a name="enable-guest-level-monitoring-on-new-vms"></a>Yeni Vm'lerde Konuk düzeyinde izlemeyi etkinleştir

Yeni VM 'Ler oluşturduğunuzda, Yönetim sekmesinde **Işletim sistemi Konuk tanılama**için **Açık** ' ı seçin.

![Konuk işletim sistemi tanılama açık olarak ayarlayın](./media/azure-vm-extended-metrics/new-enable-diag.png)

Azure sanal makineleri için genişletilmiş ölçümleri etkinleştirme hakkında daha fazla bilgi için bkz. [Azure Linux aracısını anlama ve kullanma](../virtual-machines/extensions/agent-linux.md) ve [Azure sanal makine aracısına genel bakış](../virtual-machines/extensions/agent-windows.md).

## <a name="resource-manager-credentials"></a>Kaynak Yöneticisi kimlik bilgileri

Genişletilmiş ölçümleri etkinleştirdikten sonra Cloudyn 'ın [Kaynak Yöneticisi kimlik bilgilerine](activate-subs-accounts.md)erişimi olduğundan emin olun. Kimlik bilgilerinizi toplamak ve Vm'leriniz için performans verilerini görüntülemek Cloudyn gereklidir. Maliyet iyileştirme önerileri oluşturmak için de kullanılırlar. Cloudyn, en az üç gün örneğinden bir aday downsizing önerinin olup olmadığını belirlemek için performans verilerini gerekir.

## <a name="enable-vm-metrics-with-a-script"></a>Bir betik ile VM ölçümlerini etkinleştir

Azure PowerShell betikleri ile VM ölçümlerini etkinleştirebilirsiniz. Ölçümler üzerinde etkinleştirmek istediğiniz birçok VM olduğunda işlemini otomatikleştirmek için bir komut dosyası kullanabilirsiniz. [Azure 'Da tanılama 'Yı etkinleştirmek](https://github.com/Cloudyn/azure-enable-diagnostics)için örnek betikler GitHub 'da bulunmaktadır.

## <a name="view-azure-performance-metrics"></a>Azure performans ölçümlerini görüntüleme

Cloudyn portalındaki Azure örneklerinizde performans ölçümlerini görüntülemek için **varlıklar** > **Işlem** > **örnek Gezgini**' ne gidin. Sanal makine örneklerinin listesini, örneğini genişletin ve ardından ayrıntılarını görüntülemek için bir kaynak genişletin.

![Örnek Gezgini'nde gösterilen örnek bilgileri](./media/azure-vm-extended-metrics/instance-explorer.png)

## <a name="next-steps"></a>Sonraki adımlar

- Hesaplarınız için Azure Resource Manager API erişimi henüz etkinleştirilmemişse, [Azure aboneliklerini ve hesaplarını etkinleştirme](activate-subs-accounts.md)adımına geçin.
