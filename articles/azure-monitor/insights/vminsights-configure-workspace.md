---
title: VM'ler için Azure İzleyici için Log Analytics çalışma alanı yapılandırma
description: VM'ler için Azure İzleyici tarafından kullanılan Log Analytics çalışma alanının nasıl oluşturulduğunu ve yapılandırılacağını açıklar.
ms.subservice: ''
ms.topic: conceptual
ms.custom: references_regions
author: bwren
ms.author: bwren
ms.date: 07/27/2020
ms.openlocfilehash: 40ff7bfe713f401a23dea0b3660be1b7728a1380
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87828566"
---
# <a name="configure-log-analytics-workspace-for-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici için Log Analytics çalışma alanı yapılandırma
VM'ler için Azure İzleyici, Azure Izleyici 'deki bir veya daha fazla Log Analytics çalışma alanından verilerini toplar. Aracıları ekleme öncesinde, bir çalışma alanı oluşturmanız ve yapılandırmanız gerekir. Bu makalede, çalışma alanının gereksinimleri açıklanmakta ve VM'ler için Azure İzleyici için yapılandırılacak.

## <a name="overview"></a>Genel Bakış
Tek bir abonelik, gereksinimlerinize bağlı olarak herhangi bir sayıda çalışma alanı kullanabilir, çalışma alanının tek gereksinimi desteklenen bir konumda konumlandırılabilir ve *Vminsıghts* çözümü ile yapılandırılmalıdır.

Çalışma alanı yapılandırıldıktan sonra, gerekli aracıları VM ve VMMS 'ye yüklemek için kullanılabilir seçeneklerden herhangi birini kullanabilir ve bunların verilerini gönderebilmesi için bir çalışma alanı belirtebilirsiniz. VM'ler için Azure İzleyici, aboneliğindeki herhangi bir yapılandırılmış çalışma alanından veri toplar.

> [!NOTE]
> Azure portal kullanarak tek bir VM veya VMMS üzerinde VM'ler için Azure İzleyici etkinleştirdiğinizde, var olan bir çalışma alanını seçme veya yeni bir tane oluşturma seçeneği sunulur. *Vminsıghts* çözümü henüz yoksa bu çalışma alanına yüklenecek. Daha sonra bu çalışma alanını diğer aracılar için kullanabilirsiniz.


## <a name="create-log-analytics-workspace"></a>Log Analytics çalışma alanı oluşturma

>[!NOTE]
>Bu bölümde açıklanan bilgiler [hizmet eşlemesi çözümü](service-map.md)için de geçerlidir. 

**Log Analytics çalışma alanları** menüsünden Azure Portal Log Analytics çalışma alanlarına erişin.

[![Anlytiği çalışma alanlarını günlüğe kaydet](media/vminsights-configure-workspace/log-analytics-workspaces.png)](media/vminsights-configure-workspace/log-analytics-workspaces.png#lightbox)

Aşağıdaki yöntemlerden herhangi birini kullanarak yeni bir Log Analytics çalışma alanı oluşturabilirsiniz. Ortamınızda kullanmanız gereken çalışma alanı sayısını ve erişim stratejisini nasıl tasarlayacağınızı belirlemek için bkz. [Azure Izleyici günlükleri dağıtımınızı tasarlama](../platform/design-logs-deployment.md) .


* [Azure Portal](../../azure-monitor/learn/quick-create-workspace.md)
* [Azure CLI](../../azure-monitor/learn/quick-create-workspace-cli.md)
* [PowerShell](../platform/powershell-workspace-configuration.md)
* [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)

## <a name="supported-regions"></a>Desteklenen bölgeler

VM'ler için Azure İzleyici, herhangi bir bölgedeki sanal makineleri izleyebilseniz de, aşağıdaki bölgelerde Log Analytics çalışma alanlarını destekler. Sanal makinelerin kendisi Log Analytics çalışma alanı tarafından desteklenen bölgelerle sınırlı değildir.

- Orta Batı ABD
- Batı ABD
- Batı ABD 2
- Orta Güney ABD
- Doğu ABD
- Doğu ABD 2
- Central US
- Orta Kuzey ABD
- US Gov az
- US Gov VA
- Orta Kanada
- Güney Birleşik Krallık
- Kuzey Avrupa
- West Europe
- Doğu Asya
- Güneydoğu Asya
- Orta Hindistan
- Doğu Japonya
- Doğu Avustralya
- Güneydoğu Avustralya

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi
VM'ler için Azure İzleyici özellikleri etkinleştirmek ve bu özelliklere erişmek için, çalışma alanında [Log Analytics katkıda bulunan rolüne](../platform/manage-access.md#manage-access-using-azure-permissions) sahip olmanız gerekir. Performansı, sistem durumunu ve eşleme verilerini görüntülemek için, Azure VM için [izleme okuyucu rolüne](../platform/roles-permissions-security.md#built-in-monitoring-roles) sahip olmanız gerekir. Log Analytics çalışma alanına erişimi denetleme hakkında daha fazla bilgi için bkz. [çalışma alanlarını yönetme](../platform/manage-access.md).

## <a name="add-vminsights-solution-to-workspace"></a>Çalışma alanına Vminsıghts çözümü ekleme
Bir Log Analytics çalışma alanının VM'ler için Azure İzleyici ile kullanılabilmesi için önce *Vminsıghts* çözümünün yüklü olması gerekir. Çalışma alanını yapılandırma yöntemleri aşağıdaki bölümlerde açıklanmıştır.

> [!NOTE]
> *Vminsıghts* çözümünü çalışma alanına eklediğinizde, çalışma alanına bağlı olan tüm mevcut sanal makineler, verileri ınsightsölçümlerini gönderecek şekilde başlayacaktır. Diğer veri türleri için veriler, çalışma alanına bağlı mevcut sanal makinelere Dependency Agent ekleyene kadar toplanmaz.

### <a name="azure-portal"></a>Azure portal
Azure portal var olan bir çalışma alanını yapılandırmak için üç seçenek vardır.

Tek bir çalışma alanını yapılandırmak için **diğer ekleme seçeneklerini** belirleyin ve ardından **bir çalışma alanı yapılandırın**. Bir abonelik ve çalışma alanı seçin ve ardından **Yapılandır**' a tıklayın.

[![Çalışma alanını yapılandırma](media/vminsights-enable-at-scale-policy/configure-workspace.png)](media/vminsights-enable-at-scale-policy/configure-workspace.png#lightbox)

Birden çok çalışma alanını yapılandırmak için Azure portal **Monitor** menüsündeki **sanal makineler** menüsünde bulunan **çalışma alanı yapılandırması** sekmesini seçin. Varolan çalışma alanlarının listesini göstermek için filtre değerlerini ayarlayın. Etkinleştirilecek her çalışma alanının yanındaki kutuyu işaretleyin ve ardından **Seçileni Yapılandır** ' a tıklayın.

[![Çalışma alanı yapılandırması](media/vminsights-enable-at-scale-policy/workspace-configuration.png)](media/vminsights-enable-at-scale-policy/workspace-configuration.png#lightbox)


Azure portal kullanarak tek bir VM veya VMMS üzerinde VM'ler için Azure İzleyici etkinleştirdiğinizde, var olan bir çalışma alanını seçme veya yeni bir tane oluşturma seçeneği sunulur. *Vminsıghts* çözümü henüz yoksa bu çalışma alanına yüklenecek. Daha sonra bu çalışma alanını diğer aracılar için kullanabilirsiniz.

[![Portalda tek VM 'yi etkinleştirme](media/vminsights-enable-single-vm/enable-vminsights-vm-portal.png)](media/vminsights-enable-single-vm/enable-vminsights-vm-portal.png#lightbox)


### <a name="resource-manager-template"></a>Resource Manager şablonu
VM'ler için Azure İzleyici için Azure Resource Manager şablonları, [GitHub deponuzdan indirebileceğiniz](https://aka.ms/VmInsightsARMTemplates)bir arşiv dosyasında (. zip) verilmiştir. Bu, VM'ler için Azure İzleyici için Log Analytics çalışma alanı yapılandıran **configureworkspace** adlı bir şablon içerir. Aşağıdaki örnek PowerShell ve CLı komutları dahil olmak üzere standart yöntemlerden birini kullanarak bu şablonu dağıtabilirsiniz: 

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
- Aracılardan VM'ler için Azure İzleyici bağlamak için [VM'ler için Azure izleyici aracıları](vminsights-enable-overview.md) ekleme bölümüne bakın.
- Bir çözümünden çalışma alanına gönderilen veri miktarını sınırlamak için bkz. [Azure izleyici 'de izleme çözümlerini hedefleme (Önizleme)](solution-targeting.md) .