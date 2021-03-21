---
title: VM öngörüleri için Log Analytics çalışma alanını yapılandırma
description: VM öngörüleri tarafından kullanılan Log Analytics çalışma alanının nasıl oluşturulduğunu ve yapılandırılacağını açıklar.
ms.topic: conceptual
ms.custom: references_regions
author: bwren
ms.author: bwren
ms.date: 12/22/2020
ms.openlocfilehash: 5a0e04772cfc1c9be77c0ad8b32b0e93be8b9f54
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102046729"
---
# <a name="configure-log-analytics-workspace-for-vm-insights"></a>VM öngörüleri için Log Analytics çalışma alanını yapılandırma
VM öngörüleri, Azure Izleyici 'deki bir veya daha fazla Log Analytics çalışma alanından verilerini toplar. Aracıları ekleme öncesinde, bir çalışma alanı oluşturmanız ve yapılandırmanız gerekir. Bu makalede, çalışma alanının gereksinimleri açıklanmakta ve VM öngörüleri için yapılandırılacak.

## <a name="overview"></a>Genel Bakış
Tek bir abonelik, gereksinimlerinize bağlı olarak istediğiniz sayıda çalışma alanını kullanabilir. Çalışma alanının tek gereksinimi, desteklenen bir konumda konumlandırılabilir ve *Vminsıghts* çözümü ile yapılandırılmalıdır.

Çalışma alanı yapılandırıldıktan sonra, gerekli aracıları sanal makineye ve sanal makine ölçek kümesine yüklemek için kullanılabilir seçeneklerden herhangi birini kullanabilir ve bunların verilerini gönderebilmesi için bir çalışma alanı belirtebilirsiniz. VM öngörüleri, aboneliğindeki herhangi bir yapılandırılmış çalışma alanından veri toplar.

> [!NOTE]
> VM öngörülerini tek bir sanal makinede veya Azure portal kullanarak sanal makine ölçek kümesinde etkinleştirdiğinizde, var olan bir çalışma alanını seçme veya yeni bir tane oluşturma seçeneği sunulur. *Vminsıghts* çözümü henüz yoksa bu çalışma alanına yüklenecek. Daha sonra bu çalışma alanını diğer aracılar için kullanabilirsiniz.


## <a name="create-log-analytics-workspace"></a>Log Analytics çalışma alanı oluşturma

>[!NOTE]
>Bu bölümde açıklanan bilgiler [hizmet eşlemesi çözümü](service-map.md)için de geçerlidir. 

**Log Analytics çalışma alanları** menüsünden Azure Portal Log Analytics çalışma alanlarına erişin.

[![Anlytiği çalışma alanlarını günlüğe kaydet](media/vminsights-configure-workspace/log-analytics-workspaces.png)](media/vminsights-configure-workspace/log-analytics-workspaces.png#lightbox)

Aşağıdaki yöntemlerden herhangi birini kullanarak yeni bir Log Analytics çalışma alanı oluşturabilirsiniz. Ortamınızda kullanmanız gereken çalışma alanı sayısını ve erişim stratejisini nasıl tasarlayacağınızı belirlemek için bkz. [Azure Izleyici günlükleri dağıtımınızı tasarlama](../logs/design-logs-deployment.md) .


* [Azure portalı](../logs/quick-create-workspace.md)
* [Azure CLI](../logs/quick-create-workspace-cli.md)
* [PowerShell](../logs/powershell-workspace-configuration.md)
* [Azure Resource Manager](../logs/resource-manager-workspace.md)

## <a name="supported-regions"></a>Desteklenen bölgeler
VM öngörüleri aşağıdakiler hariç [Log Analytics tarafından desteklenen bölgelerin](https://azure.microsoft.com/global-infrastructure/services/?products=monitor&regions=all) herhangi birinde bir Log Analytics çalışma alanını destekler:

- Almanya Orta Batı
- Güney Kore - Orta

>[!NOTE]
>Azure VM 'Leri herhangi bir bölgede izleyebilirsiniz. VM 'Ler, Log Analytics çalışma alanı tarafından desteklenen bölgelerle sınırlı değildir.

## <a name="azure-role-based-access-control"></a>Azure rol tabanlı erişim denetimi
VM öngörülerinin özelliklerini etkinleştirmek ve bu özelliklere erişmek için, çalışma alanında [Log Analytics katkıda bulunan rolüne](../logs/manage-access.md#manage-access-using-azure-permissions) sahip olmanız gerekir. Performansı, sistem durumunu ve eşleme verilerini görüntülemek için, Azure VM için [izleme okuyucu rolüne](../roles-permissions-security.md#built-in-monitoring-roles) sahip olmanız gerekir. Log Analytics çalışma alanına erişimi denetleme hakkında daha fazla bilgi için bkz. [çalışma alanlarını yönetme](../logs/manage-access.md).

## <a name="add-vminsights-solution-to-workspace"></a>Çalışma alanına Vminsıghts çözümü ekleme
Bir Log Analytics çalışma alanının VM öngörüleri ile kullanılabilmesi için, *Vminsıghts* çözümünün yüklü olması gerekir. Çalışma alanını yapılandırma yöntemleri aşağıdaki bölümlerde açıklanmıştır.

> [!NOTE]
> *Vminsıghts* çözümünü çalışma alanına eklediğinizde, çalışma alanına bağlı olan tüm mevcut sanal makineler, verileri ınsightsölçümlerini gönderecek şekilde başlayacaktır. Diğer veri türleri için veriler, çalışma alanına bağlı mevcut sanal makinelere Dependency Agent ekleyene kadar toplanmaz.

### <a name="azure-portal"></a>Azure portalı
Azure portal kullanarak var olan bir çalışma alanını yapılandırmak için üç seçenek vardır. Her biri aşağıda açıklanmıştır.

Tek bir çalışma alanını yapılandırmak için **Azure izleyici** menüsünde **sanal makineler** seçeneğine gidin, **diğer ekleme seçeneklerini** belirleyin ve ardından **bir çalışma alanı yapılandırın**. Bir abonelik ve çalışma alanı seçin ve ardından **Yapılandır**' a tıklayın.

[![Çalışma alanını yapılandırma](../vm/media/vminsights-enable-policy/configure-workspace.png)](../vm/media/vminsights-enable-policy/configure-workspace.png#lightbox)

Birden çok çalışma alanını yapılandırmak için Azure portal **Monitor** menüsündeki **sanal makineler** menüsünde bulunan **çalışma alanı yapılandırması** sekmesini seçin. Varolan çalışma alanlarının listesini göstermek için filtre değerlerini ayarlayın. Etkinleştirilecek her çalışma alanının yanındaki kutuyu işaretleyin ve ardından **Seçileni Yapılandır**' a tıklayın.

[![Çalışma alanı yapılandırması](../vm/media/vminsights-enable-policy/workspace-configuration.png)](../vm/media/vminsights-enable-policy/workspace-configuration.png#lightbox)


VM öngörülerini tek bir sanal makinede veya Azure portal kullanarak sanal makine ölçek kümesinde etkinleştirdiğinizde, var olan bir çalışma alanını seçme veya yeni bir tane oluşturma seçeneği sunulur. *Vminsıghts* çözümü henüz yoksa bu çalışma alanına yüklenecek. Daha sonra bu çalışma alanını diğer aracılar için kullanabilirsiniz.

[![Portalda tek VM 'yi etkinleştirme](../vm/media/vminsights-enable-portal/enable-vminsights-vm-portal.png)](../vm/media/vminsights-enable-portal/enable-vminsights-vm-portal.png#lightbox)


### <a name="resource-manager-template"></a>Resource Manager şablonu
VM öngörüleri için Azure Resource Manager şablonları, [GitHub deponuzdan indirebileceğiniz](https://aka.ms/VmInsightsARMTemplates)bir arşiv dosyasında (. zip) verilmiştir. Bu, VM öngörüleri için bir Log Analytics çalışma alanı yapılandıran **configureworkspace** adlı bir şablon içerir. Aşağıdaki örnek PowerShell ve CLı komutları dahil olmak üzere standart yöntemlerden birini kullanarak bu şablonu dağıtabilirsiniz: 

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli
az deployment group create --name ConfigureWorkspace --resource-group my-resource-group --template-file CreateWorkspace.json  --parameters workspaceResourceId='/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/my-resource-group/providers/microsoft.operationalinsights/workspaces/my-workspace' workspaceLocation='eastus'

```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```powershell
New-AzResourceGroupDeployment -Name ConfigureWorkspace -ResourceGroupName my-resource-group -TemplateFile ConfigureWorkspace.json -workspaceResourceId /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/my-resource-group/providers/microsoft.operationalinsights/workspaces/my-workspace -location eastus
```

---



## <a name="next-steps"></a>Sonraki adımlar
- Aracıları VM öngörülerine bağlamak için bkz. [VM öngörülerine](vminsights-enable-overview.md) aracılar ekleme.
- Bir çözümünden çalışma alanına gönderilen veri miktarını sınırlamak için bkz. [Azure izleyici 'de izleme çözümlerini hedefleme (Önizleme)](../insights/solution-targeting.md) .
