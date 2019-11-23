---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/04/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 06b54c3038e8b4f5879a93b696920534c2199008
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74414561"
---
This article provides you with an overview of the availability features of Azure virtual machines (VMs).

## <a name="high-availability"></a>Yüksek kullanılabilirlik

Workloads are typically spread across different virtual machines to gain high throughput, performance, and to create redundancy in case a VM is impacted due to an update or other event. 

There are few options that Azure provides to achieve High Availability. First let’s talk about basic constructs. 

### <a name="availability-zones"></a>Kullanılabilirlik alanları

[Availability zones](../articles/availability-zones/az-overview.md) expand the level of control you have to maintain the availability of the applications and data on your VMs. An Availability Zone is a physically separate zone, within an Azure region. There are three Availability Zones per supported Azure region. 

Her Kullanılabilirlik Alanı farklı bir güç kaynağı, ağ ve soğutma sistemine sahiptir. By architecting your solutions to use replicated VMs in zones, you can protect your apps and data from the loss of a datacenter. If one zone is compromised, then replicated apps and data are instantly available in another zone. 

![Kullanılabilirlik alanları](./media/virtual-machines-common-regions-and-availability/three-zones-per-region.png)

Learn more about deploying a [Windows](../articles/virtual-machines/windows/create-powershell-availability-zone.md) or [Linux](../articles/virtual-machines/linux/create-cli-availability-zone.md) VM in an Availability Zone.


### <a name="fault-domains"></a>Hata etki alanları

Hata etki alanı, ortak bir güç kaynağı ve ağ anahtarını paylaşan, şirket içi veri merkezindeki rafa benzer bir temel alınan donanım mantık grubudur. 

### <a name="update-domains"></a>Güncelleme etki alanları

Güncelleme etki alanı, bakımdan geçirilebilen ya da aynı anda yeniden başlatılabilen bir temel alınan donanım mantıksal grubudur. 

Bu yaklaşım, Azure platformu periyodik bakımdan geçirilirken uygulamanızın en az bir örneğinin her zaman çalışır durumda kalmasını sağlar. The order of update domains being rebooted may not proceed sequentially during maintenance, but only one update domain is rebooted at a time.


## <a name="virtual-machines-scale-sets"></a>Virtual Machines Scale Sets 

Azure virtual machine scale sets let you create and manage a group of load balanced VMs. Tanımlı bir zamanlamaya veya talebe yanıt olarak sanal makine örneği sayısı otomatik olarak artabilir ya da azalabilir. Scale sets provide high availability to your applications, and allow you to centrally manage, configure, and update many VMs. We recommended that two or more VMs are created within a scale set to provide for a highly available application and to meet the [99.95% Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). There is no cost for the scale set itself, you only pay for each VM instance that you create. When a single VM is using [Azure premium SSDs](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#premium-ssd), the Azure SLA applies for unplanned maintenance events. Virtual machines in a scale set can be deployed across multiple update domains and fault domains to maximize availability and resilience to outages due to data center outages, and planned or unplanned maintenance events. Virtual machines in a scale set can also be deployed into a single Availability zone, or regionally. Availability zone deployment options may differ based on the orchestration mode.

### <a name="preview-orchestration-mode-preview"></a>Preview: Orchestration mode Preview
Virtual machines scale sets allow you to specify orchestration mode.  With the virtual machine scale set orchestration mode (preview), you can now choose whether the scale set should orchestrate virtual machines which are created explicitly outside of a scale set configuration model, or virtual machine instances created implicitly based on the configuration model. Choose the orchestration mode that VM orchestration model allows you group explicitly defined Virtual Machines together in a region or in an availability zone. Virtual machines deployed in an Availability Zone provides zonal isolation to VMs are they are bound to the availability zone boundary and are not subjected to any failures that may occur in other availability zone in the region. 

|   | “orchestrationMode”: “VM” (VirtualMachine)| “orchestrationMode”: “ScaleSetVM” (VirtualMachineScaleSetVM) |
|----|----|----|
| VM configuration model| Hiçbiri. VirtualMachineProfile is undefined in the scale set model. | Gereklidir. VirtualMachineProfile is populated in the scale set model. |
| Adding new VM to Scale Set| VMs are explicitly added to the scale set when the VM is created. | VMs are implicitly created and added to the scale set based on the VM configuration model, instance count, and AutoScaling rules. |
| Kullanılabilirlik Alanları| Supports regional deployment or VMs in one Availability Zone| Supports regional deployment or multiple Availability Zones; Can define the zone balancing strategy |
| Hata etki alanları| Can define fault domains count. 2 or 3 based on regional support and 5 for Availability zone. The assigned VM fault domain will persist with VM lifecycle, including deallocate and restart. | Can define 1, 2, or 3 fault domains for non-zonal deployments, and 5 for Availability zone deployments. The assigned VM fault domain does not persist with VM lifecycle, virtual machines are assigned a fault domain at time of allocation. |
| Güncelleme etki alanları| Yok. Update domains are automatically mapped to fault domains| Yok. Update domains are automatically mapped to fault domains |

**Fault domains and update domains**

Virtual machine scale sets simplify designing for high availability by aligning fault domains and update domains. You will only have to define fault domains count for the scale set. The number of fault domains available to the scale sets may vary by region. See [Number of Fault Domains per region](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#number-of-fault-domains-per-region).


## <a name="availability-sets"></a>Kullanılabilirlik kümeleri
An availability set is a logical grouping of VMs within a datacenter that allows Azure to understand how your application is built to provide for redundancy and availability. We recommended that two or more VMs are created within an availability set to provide for a highly available application and to meet the [99.95% Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). There is no cost for the Availability Set itself, you only pay for each VM instance that you create. When a single VM is using [Azure premium SSDs](../articles/virtual-machines/windows/disks-types.md#premium-ssd), the Azure SLA applies for unplanned maintenance events.

In an availability set, VMs are automatically distributed across these fault domains. Bu yaklaşım, olası fiziksel donanım hatalarının, ağ kesintilerinin veya güç kesintilerinin etkisini sınırlar.

[Azure Yönetilen Diskler](../articles/virtual-machines/windows/faq-for-disks.md)’i kullanan sanal makineler, yönetilen kullanılabilirlik kümesi kullanılırken yönetilen disk hata etki alanları ile hizalanır. Bu hizalama, bir VM'ye bağlı tüm yönetilen disklerin, aynı yönetilen disk hata etki alanı içinde olmasını sağlar. 

Yönetilen bir kullanılabilirlik kümesinde yalnızca, yönetilen disklere sahip VM’ler oluşturulabilir. Yönetilen disk hata etki alanlarının sayısı bölgeye göre farklılık gösterir (bölge başına iki ya da üç yönetilen disk hata etki alanı). You can read more about these managed disk fault domains for [Linux VMs](../articles/virtual-machines/linux/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set) or [Windows VMs](../articles/virtual-machines/windows/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set).

![Managed availability set](./media/virtual-machines-common-manage-availability/md-fd-updated.png)


VMs within an availability set are also automatically distributed across update domains. 

![Kullanılabilirlik kümeleri](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure ortamınızı oluşturmak için bu kullanılabilirlik ve yedeklilik özelliklerini kullanmaya başlayabilirsiniz. En iyi uygulama bilgileri için bkz. [Azure kullanılabilirlik en iyi uygulamaları](/azure/architecture/checklist/resiliency-per-service).

