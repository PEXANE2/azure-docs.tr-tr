---
title: Azure'da AWS ve diğer platformlardan Yönetilen Disklere geçiş
description: AWS veya diğer sanallaştırma platformları gibi diğer bulutlardan yüklenen VHD'leri kullanarak Azure'da Sanal M'ler oluşturun ve Azure Yönetilen Disklerden yararlanın.
author: roygara
manager: twooley
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: conceptual
ms.date: 10/07/2017
ms.author: rogarana
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: dc283d1845926b79fb541d8ccb011fe853f50484
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81870292"
---
# <a name="migrate-from-amazon-web-services-aws-and-other-platforms-to-managed-disks-in-azure"></a>Amazon Web Hizmetleri 'nden (AWS) ve diğer platformlardan Azure'daki Yönetilen Disklere geçiş

Yönetilen Disklerden yararlanan VM'ler oluşturmak için Azure'a AWS'den veya şirket içi sanallaştırma çözümlerinden VHD dosyaları yükleyebilirsiniz. Azure Yönetilen Diskler, Azure IaaS VM'lerinin depolama hesaplarını yönetme gereksinimini ortadan kaldırır. Yalnızca ihtiyacınız olan diskin türünü (Premium veya Standart) ve boyutunu belirtmeniz gerekir ve Azure diski sizin için oluşturur ve yönetir. 

Genelleştirilmiş ve özel leştirilmiş VHD'leri yükleyebilirsiniz. 
- **Genelleştirilmiş VHD** - Tüm kişisel hesap bilgilerinizi Sysprep kullanarak kaldırıldı. 
- **ÖzelLeştirilmiş VHD** - kullanıcı hesaplarını, uygulamaları ve diğer durum verilerini orijinal VM'inizden korur. 

> [!IMPORTANT]
> Azure'a herhangi bir VHD yüklemeden önce, [Azure'a yüklemek için Windows VHD veya VHDX Hazırlama'yı](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) izlemeniz gerekir
>
>


| Senaryo                                                                                                                         | Belgeler                                                                                                                       |
|----------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| Yönetilen diskleri kullanarak Azure VM'lerine geçirmek istediğiniz varolan AWS EC2 örnekleriniz var                              | [Bir VM'yi Amazon Web Services'tan (AWS) Azure'a taşıma](aws-to-azure.md)                           |
| Birden çok Azure VM oluşturmak için görüntü olarak kullanmak istediğiniz başka bir sanallaştırma platformundan bir VM'inize sahipsiniz. | [Genelleştirilmiş bir VHD yükleyin ve Azure'da yeni bir VM oluşturmak için kullanın](upload-generalized-managed.md) |
| Azure'da yeniden oluşturmak istediğiniz benzersiz olarak özelleştirilmiş bir VM'iniz var.                                                      | [Azure'a özel bir VHD yükleyin ve yeni bir VM oluşturun](create-vm-specialized.md)         |


## <a name="overview-of-managed-disks"></a>Yönetilen Disklere Genel Bakış

Azure Yönetilen Diskler, depolama hesaplarını yönetme gereksinimini ortadan kaldırarak VM yönetimini kolaylaştırır. Yönetilen Diskler, kullanılabilirlik kümesindeki VM'lerin daha iyi güvenilirliğinden de yararlanır. Kullanılabilirlik Kümesi'ndeki farklı VM'lerin disklerinin, tek bir hata noktasını önlemek için birbirinden yeterince yalıtılmış olmasını sağlar. Donanım ve yazılım hatalarından kaynaklanan tek Depolama ölçeği birim hatalarının etkisini sınırlayan farklı Depolama ölçeği birimlerinde (damgalar) farklı VM'lerin disklerini otomatik olarak yerleştirir.
İhtiyaçlarınıza bağlı olarak, dört tür depolama seçeneği arasından seçim yapabilirsiniz. Kullanılabilir disk türleri hakkında bilgi edinmek için makalemize bakın [bir disk türü seçin.](disks-types.md)

## <a name="plan-for-the-migration-to-managed-disks"></a>Yönetilen Disklere geçiş planı

Bu bölüm, VM ve disk türleri hakkında en iyi kararı vermenize yardımcı olur.

Yönetilmeyen disklerden yönetilen disklere geçiş yapmayı planlıyorsanız, [Sanal Makine Katılımcısı](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) rolüne sahip kullanıcıların VM boyutunu (dönüşüm öncesi yapabilecekleri gibi) değiştiremeyeceğini unutmayın. Bunun nedeni, yönetilen disklere sahip VM'lerin kullanıcının işletim sistemi disklerinde Microsoft.Compute/disks/write iznine sahip olmasını gerektirmesidir.

### <a name="location"></a>Konum

Azure Yönetilen Disklerin kullanılabildiği bir konum seçin. Premium Yönetilen Disklere geçiş yapıyorsanız, Premium depolama nın göç etmeyi planladığınız bölgede de kullanılabilir olduğundan emin olun. Kullanılabilir konumlarla ilgili güncel bilgiler için [Bölgeye göre Azure Hizmetleri'ne](https://azure.microsoft.com/regions/#services) bakın.

### <a name="vm-sizes"></a>VM boyutları

Premium Yönetilen Disklere geçiş yapıyorsanız, VM'nin bulunduğu bölgede bulunan Premium Depolama kapasitesine göre VM boyutunu güncelleştirmeniz gerekir. Premium Depolama yeteneğine sahip VM boyutlarını gözden geçirin. Azure VM boyutu belirtimleri [sanal makineler için Boyutlar'da](sizes.md)listelenir.
Premium Depolama ile çalışan sanal makinelerin performans özelliklerini gözden geçirin ve iş yükünüze en uygun VM boyutunu seçin. VM'nizde disk trafiğini yönlendirmek için yeterli bant genişliği olduğundan emin olun.

### <a name="disk-sizes"></a>Disk boyutları

**Premium Yönetilen Diskler**

VM'inizle kullanılabilen ve her birinin belirli IOP'leri ve iş elde edilme sınırları olan yedi tür premium yönetilen disk vardır. Kapasite, performans, ölçeklenebilirlik ve tepe yükleri açısından uygulamanızın ihtiyaçlarına göre VM'niz için Premium disk türünü seçerken bu sınırları göz önünde bulundurun.

| Premium Diskler Türü  | P4    | P6    | P10   | P15   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|-------|
| Disk boyutu           | 32 GB| 64 GB| 128 GB| 256 GB|512 GB | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| Disk başına IOPS       | 120   | 240   | 500   | 1100  |2300              | 5000              | 7.500              | 7.500              | 
| Disk başına aktarım hızı | Saniyede 25 MB  | Saniyede 50 MB  | Saniyede 100 MB | Saniyede 125 MB |Saniyede 150 MB | Saniyede 200 MB | Saniyede 250 MB | Saniyede 250 MB |

**Standart Yönetilen Diskler**

VM'inizle kullanılabilecek yedi tür standart yönetilen disk vardır. Her biri farklı kapasiteye sahiptir ancak aynı IOPS ve iş geliştirme limitlerine sahiptir. Uygulamanızın kapasite gereksinimlerine göre Standart Yönetilen disktürünü seçin.

| Standart Disk Türü  | S4               | S6               | S10              | S15              | S20              | S30              | S40              | S50              | 
|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------| 
| Disk boyutu           | 30 GB            | 64 GB            | 128 GB           | 256 GB           |512 GB           | 1024 GB (1 TB)   | 2048 GB (2 TB)    | 4095 GB (4 TB)   | 
| Disk başına IOPS       | 500              | 500              | 500              | 500              |500              | 500              | 500             | 500              | 
| Disk başına aktarım hızı | Saniyede 60 MB | Saniyede 60 MB | Saniyede 60 MB | Saniyede 60 MB |Saniyede 60 MB | Saniyede 60 MB | Saniyede 60 MB | Saniyede 60 MB | 

### <a name="disk-caching-policy"></a>Disk önbelleğe alma ilkesi 

**Premium Yönetilen Diskler**

Varsayılan olarak, disk önbelleğe alma ilkesi tüm Premium veri diskleri için *Salt Okunur* ve VM'ye bağlı Premium işletim sistemi diski için *Okuma-Yazma'dır.* Bu yapılandırma ayarı, uygulamanızın IO'ları için en iyi performansı elde etmek için önerilir. Yazma ağır veya yalnızca yazma veri diskleri (SQL Server günlük dosyaları gibi) için, daha iyi uygulama performansı elde edebilmek için disk önbelleği devre dışı kullanabilirsiniz.

### <a name="pricing"></a>Fiyatlandırma

Yönetilen [Diskler için fiyatlandırmayı gözden geçirin.](https://azure.microsoft.com/pricing/details/managed-disks/) Premium Yönetilen Disklerin fiyatlandırması, Premium Yönetilmeyen Disklerle aynıdır. Ancak Standart Yönetilen Diskler için fiyatlandırma, Standart Yönetilmeyen Disklerden farklıdır.


## <a name="next-steps"></a>Sonraki Adımlar

- Azure'a herhangi bir VHD yüklemeden önce, [Azure'a yüklemek için Windows VHD veya VHDX Hazırlama'yı](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) izlemeniz gerekir
