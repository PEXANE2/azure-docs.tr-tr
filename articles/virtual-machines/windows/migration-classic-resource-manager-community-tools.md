---
title: Topluluk araçları - Klasik kaynakları Azure Kaynak Yöneticisi'ne taşıyın
description: Bu makalede, IaaS kaynaklarını klasikten Azure Kaynak Yöneticisi dağıtım modeline geçirmelerine yardımcı olmak için topluluk tarafından sağlanan araçlar kataloglanmıştır.
author: tanmaygore
manager: vashan
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: conceptual
ms.date: 02/06/2020
ms.author: tagore
ms.openlocfilehash: 9839f411458eeb4fd071177ec8208baa94dca3a9
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81866147"
---
# <a name="community-tools-to-migrate-iaas-resources-from-classic-to-azure-resource-manager"></a>IaaS kaynaklarını klasikten Azure Kaynak Yöneticisi'ne geçirmek için topluluk araçları

> [!IMPORTANT]
> Bugün, IaaS VM'lerinin yaklaşık %90'ı [Azure Kaynak Yöneticisi](https://azure.microsoft.com/features/resource-manager/)kullanıyor. 28 Şubat 2020 itibariyle, klasik VM'ler amortismana kaldırılmıştır ve 1 Mart 2023'te tamamen emekliye ayırılacaktır. Bu amortisman ve sizi [nasıl etkilediği](https://docs.microsoft.com/azure/virtual-machines/classic-vm-deprecation#how-does-this-affect-me)hakkında [daha fazla bilgi edinin.]( https://aka.ms/classicvmretirement)

Bu makalede, IaaS kaynaklarının klasikten Azure Kaynak Yöneticisi dağıtım modeline geçişine yardımcı olmak için topluluk tarafından sağlanan araçlar kataloglanmıştır.

> [!NOTE]
> Bu araçlar resmi olarak Microsoft Desteği tarafından desteklenmez. Bu nedenle github açık kaynaklı ve biz düzeltmeler veya ek senaryolar için PRs kabul etmekten mutluluk duyuyoruz. Bir sorunu bildirmek için GitHub sorunları özelliğini kullanın.
> 
> Bu araçlarla geçiş, klasik Sanal Makineniziçin kapalı kalma süresine neden olur. Platform destekli geçiş arıyorsanız, 
> 
>   * [Platform, IaaS kaynaklarının Klasik'ten Azure Kaynak Yöneticisi yığınına geçişini destekledi](migration-classic-resource-manager-overview.md)
>   * [Platformda Teknik Derin Dalış, Classic'ten Azure Kaynak Yöneticisi'ne geçişi destekledi](migration-classic-resource-manager-deep-dive.md)
>   * [Azure PowerShell'i kullanarak IaaS kaynaklarını Klasik'ten Azure Kaynak Yöneticisi'ne geçirin](migration-classic-resource-manager-ps.md)
> 
> 

## <a name="asmmetadataparser"></a>AsmMetadataParser
Bu, Azure Hizmet Yönetimi'nden Azure Kaynak Yöneticisi'ne kurumsal geçişlerin bir parçası olarak oluşturulan yardımcı araçlar topluluğudur. Bu araç, altyapınızı, geçişi test etmek için kullanılabilecek başka bir aboneliğe çoğaltmanızı ve Üretim aboneliğinizde geçişi çalıştırmadan önce tüm sorunları gidermenizi sağlar.

[Araç belgelerine bağlantı](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

## <a name="migaz"></a>migAz
migAz, klasik IaaS kaynaklarının tamamını Azure Kaynak Yöneticisi IaaS kaynaklarına geçirmek için ek bir seçenektir. Geçiş, aynı abonelik içinde veya farklı abonelikler ve abonelik türleri (ör. CSP abonelikleri) arasında oluşabilir.

[Araç belgelerine bağlantı](https://github.com/Azure/migAz)

## <a name="next-steps"></a>Sonraki Adımlar

* [IaaS kaynaklarının klasikten Azure Kaynak Yöneticisi'ne platform destekli geçişine genel bakış](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Klasik modelden Azure Resource Manager’a platform destekli geçişe ayrıntılı teknik bakış](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarının Klasik’ten Azure Resource Manager’a geçişini planlama](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasikten Azure Kaynak Yöneticisi'ne geçirmek için PowerShell'i kullanın](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasikten Azure Kaynak Yöneticisi'ne geçirmek için CLI'yi kullanın](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [En sık karşılaşılan geçiş hatalarını gözden geçirme](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [IaaS kaynaklarını klasikten Azure Kaynak Yöneticisi'ne geçirme yle ilgili en sık sorulan soruları gözden geçirin](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

