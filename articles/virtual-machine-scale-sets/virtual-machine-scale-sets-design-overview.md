---
title: Azure Sanal Makine Ölçek Setleri için Tasarım Dadikkat Edilecek Noktalar
description: Azure Sanal Makine Ölçek Kümeleriniziçin tasarım la ilgili hususlar hakkında bilgi edinin. Ölçek kümelerini Özellikleri VM özellikleriyle karşılaştırın.
keywords: linux sanal makine, sanal makine ölçek setleri
author: mimckitt
tags: azure-resource-manager
ms.assetid: c27c6a59-a0ab-4117-a01b-42b049464ca1
ms.service: virtual-machine-scale-sets
ms.tgt_pltfrm: vm-linux
ms.topic: conceptual
ms.date: 06/01/2017
ms.author: mimckitt
ms.openlocfilehash: 20f6cb08781c7c6aca7a4022e75a7be8640ef18a
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273775"
---
# <a name="design-considerations-for-scale-sets"></a>Ölçek Kümeleri Için Tasarım Konuları
Bu makalede, Sanal Makine Ölçek Kümeleri için tasarım konuları anlatılmaktadır. Sanal Makine Ölçeği Kümelerinin ne olduğu hakkında bilgi için [Sanal Makine Ölçeği Kümelerine Genel Bakış'a](virtual-machine-scale-sets-overview.md)bakın.

## <a name="when-to-use-scale-sets-instead-of-virtual-machines"></a>Sanal makineler yerine ölçek kümeleri ne zaman kullanılır?
Genellikle, ölçek kümeleri, bir makine kümesinin benzer yapılandırmaya sahip olduğu yüksek kullanılabilir altyapıyı dağıtmak için yararlıdır. Ancak, bazı özellikler yalnızca ölçek kümelerinde, diğer özellikler ise yalnızca VM'lerde kullanılabilir. Her teknolojinin ne zaman kullanılacağı konusunda bilinçli bir karar vermek için, öncelikle VM'lerde değil ölçek kümelerinde kullanılabilen ve yaygın olarak kullanılan özelliklerden bazılarına göz atmalısınız:

### <a name="scale-set-specific-features"></a>Kümeye özgü özellikleri ölçeklendirin

- Ölçek kümesi yapılandırmasını belirttikten sonra, paralel olarak daha fazla VM dağıtmak için *kapasite* özelliğini güncelleştirebilirsiniz. Bu işlem, birçok ayrı VM'yi paralel olarak dağıtmak için bir komut dosyası yazmaktan daha iyidir.
- Bir [ölçek kümesini otomatik olarak ölçeklendirmek için Azure Otomatik Ölçek'i kullanabilirsiniz,](./virtual-machine-scale-sets-autoscale-overview.md) ancak tek tek VM'leri ölçeklendiremezsiniz.
- Ölçek [kümesi VM'leri yeniden görüntüleyebilirsiniz,](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage) ancak [tek tek VM'leri yeniden görüntüleyebilirsiniz.](https://docs.microsoft.com/rest/api/compute/virtualmachines)
- Daha fazla güvenilirlik ve daha hızlı dağıtım süreleri için ölçek kümesi VM'leri [fazla kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview#overprovisioning) yapabilirsiniz. Bu eylemi gerçekleştirmek için özel kod yazmadığınız sürece tek tek VM'leri fazla sağlayamazsınız.
- Ölçek setinizde VM'ler arasında yükseltmeleri kullanıma kolaylaştırmayı kolaylaştırmak için bir [yükseltme ilkesi](./virtual-machine-scale-sets-upgrade-scale-set.md) belirtebilirsiniz. Tek tek VM'ler ile güncellemeleri kendiniz düzenlemeniz gerekir.

### <a name="vm-specific-features"></a>VM'ye özel özellikler

Bazı özellikler şu anda yalnızca VM'lerde kullanılabilir:

- Tek bir VM'den görüntü yakalayabilirsiniz, ancak ölçek kümesindeki bir VM'den görüntü yakalayamaabilirsiniz.
- Tek bir VM'yi yerel disklerden yönetilen disklere geçirebilirsiniz, ancak bir ölçek kümesinde VM örneklerini geçiremezsiniz.
- IPv6 genel IP adreslerini tek tek VM sanal ağ arabirim kartlarına (NIC) atayabilirsiniz, ancak bunu ölçek kümesindeki VM örnekleri için yapamazsınız. Bireysel VM'lerin veya ölçek kümesi VM'lerinin önündeki bakiyeleri yüklemek için IPv6 genel IP adresleri atayabilirsiniz.

## <a name="storage"></a>Depolama

### <a name="scale-sets-with-azure-managed-disks"></a>Azure Yönetilen Diskler ile ölçek kümeleri
Geleneksel Azure depolama hesapları yerine [Azure Yönetilen Diskler](../virtual-machines/windows/managed-disks-overview.md) ile ölçek kümeleri oluşturulabilir. Yönetilen Diskler aşağıdaki avantajları sağlar:
- Ölçek kümesi VM'leri için bir Azure depolama hesabı kümesini önceden oluşturmanız gerekmez.
- Ölçek kümenizde VM'ler için [ekli veri diskleri](virtual-machine-scale-sets-attached-disks.md) tanımlayabilirsiniz.
- Ölçek [kümeleri, bir kümede 1.000 VM'ye kadar desteklenecek](virtual-machine-scale-sets-placement-groups.md)şekilde yapılandırılabilir. 

Varolan bir şablonunuzun varsa, [yönetilen diskleri kullanmak için şablonu](virtual-machine-scale-sets-convert-template-to-md.md)da güncelleştirebilirsiniz.

### <a name="user-managed-storage"></a>Kullanıcı tarafından yönetilen Depolama
Azure Yönetilen Diskler ile tanımlanmayan bir ölçek kümesi, VM'lerin işletim sistemi disklerini kümede depolamak için kullanıcı tarafından oluşturulan depolama hesaplarına güvenir. Maksimum IO'ya ulaşmak ve _ayrıca aşırı sağlamadan_ yararlanmak için depolama hesabı başına 20 VM veya daha az vm oranı önerilir (aşağıya bakın). Ayrıca, depolama hesabı adlarının başlangıç karakterlerini alfabeye yaymanız da önerilir. Bunu yapmak, yükün farklı dahili sistemlere yayılmasına yardımcı olur. 


## <a name="overprovisioning"></a>Aşırı sağlama
Ölçek şu anda varsayılan olarak "aşırı sağlama" VM'leri kümeler. Aşırı sağlama açıkken, ölçek kümesi aslında istediğinizden daha fazla VM döndürür ve istenen VM sayısı başarıyla sağlandıktan sonra fazladan VM'leri siler. Aşırı sağlama, sağlama başarı oranlarını artırır ve dağıtım süresini azaltır. İlave VM'ler için faturalandırılmamanız ve bunlar kota sınırlarınıza dahil edilmez.

Aşırı sağlama başarı oranlarını artırırken, görünen ve sonra kaybolan ekstra VM işlemek için tasarlanmayan bir uygulama için kafa karıştırıcı davranışlara neden olabilir. Fazla hükmü kapatmak için şablonunuzda aşağıdaki dize olduğundan `"overprovision": "false"`emin olun: . Daha fazla ayrıntı [Ölçek Seti REST API belgelerinde](/rest/api/virtualmachinescalesets/create-or-update-a-set)bulunabilir.

Ölçek kümeniz kullanıcı tarafından yönetilen depolama alanını kullanıyorsa ve fazla sağlamayı kapatırsanız, depolama hesabı başına 20'den fazla VM'niz olabilir, ancak IO performans nedenleriyle 40'ın üzerine çıkmanız önerilmez. 

## <a name="limits"></a>Sınırlar
Pazar Yeri görüntüsü (platform görüntüsü olarak da bilinir) üzerine inşa edilmiş ve Azure Yönetilen Diskler'i kullanacak şekilde yapılandırılan ölçek kümesi, 1.000 VM'ye kadar kapasiteyi destekler. Ölçek kümenizi 100'den fazla VM'yi destekleyecek şekilde yapılandırırsanız, tüm senaryolar aynı çalışmaz (örneğin yük dengeleme). Daha fazla bilgi için bkz: [Büyük sanal makine ölçek kümeleriyle çalışma.](virtual-machine-scale-sets-placement-groups.md) 

Kullanıcı tarafından yönetilen depolama hesaplarıyla yapılandırılan bir ölçek kümesi şu anda 100 VM ile sınırlıdır (ve bu ölçek için 5 depolama hesabı önerilir).

Azure Yönetilen disklerle yapılandırıldığında özel bir görüntü (sizin yeriniz tarafından oluşturulmuş bir resim) üzerine oluşturulmuş bir ölçek kümesi, 600 VM'ye kadar kapasiteye sahip olabilir. Ölçek kümesi kullanıcı tarafından yönetilen depolama hesaplarıyla yapılandırılırsa, tek bir depolama hesabındatüm işletim sistemi disk VHD'lerini oluşturması gerekir. Sonuç olarak, özel bir görüntü ve kullanıcı tarafından yönetilen depolama üzerine oluşturulmuş bir ölçek kümesinde önerilen maksimum VM sayısı 20'dir. Fazla hükmü kapatırsanız, 40'a kadar çıkabilirsiniz.

Bu sınırların izin gösterdiğinden daha fazla VM için, [bu şablonda](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale)gösterildiği gibi birden çok ölçek kümesi dağıtmanız gerekir.

