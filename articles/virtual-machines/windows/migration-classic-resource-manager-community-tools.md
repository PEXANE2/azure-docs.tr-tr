---
title: Topluluk araçları-klasik kaynakları Azure Resource Manager taşıyın
description: Bu makalede, IaaS kaynaklarını klasik ' ten Azure Resource Manager dağıtım modeline geçirmeye yardımcı olmak üzere topluluk tarafından sağlanan araçlar kataloglandırır.
services: virtual-machines-windows
documentationcenter: ''
author: tanmaygore
manager: vashan
editor: ''
tags: azure-resource-manager
ms.assetid: 228b697b-3950-49f5-84bb-283bb56621b1
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 02/06/2020
ms.author: tagore
ms.openlocfilehash: 5688854404ca6d3e64ca6d59276628d08bbf3ebe
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2020
ms.locfileid: "77086012"
---
# <a name="community-tools-to-migrate-iaas-resources-from-classic-to-azure-resource-manager"></a>IaaS kaynaklarını klasik 'ten Azure Resource Manager geçirmeye yönelik topluluk araçları
Bu makalede, IaaS kaynaklarının klasik Azure Resource Manager dağıtım modeline geçişine yardımcı olmak üzere topluluk tarafından sunulan araçlar kataloglandırır.

> [!NOTE]
> Bu araçlar Microsoft Desteği tarafından resmi olarak desteklenmez. Bu nedenle, GitHub 'da açık kaynak olarak bulunur ve düzeltmeler veya ek senaryolar için PR 'ler kabul etmekten mutluluk duyuyoruz. Bir sorunu raporlamak için GitHub sorunları özelliğini kullanın.
> 
> Bu araçlarla geçiş yapmak, klasik sanal makineniz için kapalı kalma süresine neden olur. Platform destekli geçiş arıyorsanız, şu adresi ziyaret edin 
> 
>   * [Platform IaaS kaynaklarının klasik 'ten Azure Resource Manager yığınına geçirilmesini destekliyordu](migration-classic-resource-manager-overview.md)
>   * [Klasik ile Azure Resource Manager arasında teknik kapsamlı bakış platformu desteği](migration-classic-resource-manager-deep-dive.md)
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
* [Klasik modelden Azure Resource Manager’a platform destekli geçişe ayrıntılı teknik bakış](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarının Klasik’ten Azure Resource Manager’a geçişini planlama](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasik 'ten Azure Resource Manager geçirmek için PowerShell 'i kullanma](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasik 'ten Azure Resource Manager geçirmek için CLı kullanma](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasik konumundan Azure Resource Manager geçirme hakkında en sık sorulan soruları gözden geçirin](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

