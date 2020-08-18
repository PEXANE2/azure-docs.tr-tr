---
title: Topluluk araçları-klasik kaynakları Azure Resource Manager taşıyın
description: Bu makalede, IaaS kaynaklarını klasik ' ten Azure Resource Manager dağıtım modeline geçirmeye yardımcı olmak üzere topluluk tarafından sağlanan araçlar kataloglandırır.
author: tanmaygore
manager: vashan
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: conceptual
ms.date: 02/06/2020
ms.author: tagore
ms.openlocfilehash: afc20a39a5b426f37d6a6752056e3dd35dd8438a
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88504968"
---
# <a name="community-tools-to-migrate-iaas-resources-from-classic-to-azure-resource-manager"></a>IaaS kaynaklarını klasik 'ten Azure Resource Manager geçirmeye yönelik topluluk araçları

> [!IMPORTANT]
> Bugün, IaaS VM 'lerinin yaklaşık %90 ' u [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)kullanıyor. 28 Şubat 2020 itibariyle klasik VM 'Ler kullanımdan kaldırılmıştır ve 1 Mart 2023 tarihinde tamamen kullanımdan kaldırılacaktır. Bu kullanımdan kaldırma ve [nasıl etkilediği](../classic-vm-deprecation.md#how-does-this-affect-me)hakkında [daha fazla bilgi edinin]( https://aka.ms/classicvmretirement) .

Bu makalede, IaaS kaynaklarının klasik Azure Resource Manager dağıtım modeline geçişine yardımcı olmak üzere topluluk tarafından sunulan araçlar kataloglandırır.

> [!NOTE]
> Bu araçlar Microsoft Desteği tarafından resmi olarak desteklenmez. Bu nedenle, GitHub 'da açık kaynak olarak bulunur ve düzeltmeler veya ek senaryolar için PR 'ler kabul etmekten mutluluk duyuyoruz. Bir sorunu raporlamak için GitHub sorunları özelliğini kullanın.
> 
> Bu araçlarla geçiş yapmak, klasik sanal makineniz için kapalı kalma süresine neden olur. Platform destekli geçiş arıyorsanız, şu adresi ziyaret edin 
> 
>   * [Platform IaaS kaynaklarının klasik 'ten Azure Resource Manager yığınına geçirilmesini destekliyordu](migration-classic-resource-manager-overview.md)
>   * [Klasik ile Azure Resource Manager arasında teknik kapsamlı bakış platformu desteği](../migration-classic-resource-manager-deep-dive.md)
>   * [IaaS kaynaklarını klasik 'dan Azure Resource Manager Azure PowerShell kullanarak geçirme](migration-classic-resource-manager-ps.md)
> 
> 

## <a name="asmmetadataparser"></a>AsmMetadataParser
Bu, Azure hizmet yönetimi 'ndeki kurumsal geçişlerde Azure Resource Manager olarak oluşturulan bir yardımcı araç koleksiyonudur. Bu araç, altyapınızı test etme ve üretim aboneliğinizde geçişi çalıştırmadan önce herhangi bir sorunu dışarı aktarma için kullanılabilen başka bir aboneliğe çoğaltmanıza olanak sağlar.

[Araç belgelerine bağlantı](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

## <a name="migaz"></a>migAz
migAz, tüm klasik IaaS kaynaklarını IaaS kaynaklarına Azure Resource Manager geçirmek için ek bir seçenektir. Geçiş, aynı abonelik içinde veya farklı abonelikler ve Abonelik türleri (örn: CSP abonelikleri) arasında gerçekleşebilir.

[Araç belgelerine bağlantı](https://github.com/Azure/migAz)

## <a name="next-steps"></a>Sonraki Adımlar

* [IaaS kaynaklarının klasik ile Azure Resource Manager geçişine genel bakış](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Klasik modelden Azure Resource Manager’a platform destekli geçişe ayrıntılı teknik bakış](../migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarının Klasik’ten Azure Resource Manager’a geçişini planlama](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasik 'ten Azure Resource Manager geçirmek için PowerShell 'i kullanma](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasik 'ten Azure Resource Manager geçirmek için CLı kullanma](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasik konumundan Azure Resource Manager geçirme hakkında en sık sorulan soruları gözden geçirin](../migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
