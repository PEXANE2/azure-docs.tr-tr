---
title: Azure Geçir applianc'ı ayarlama
description: VMware VM'leri değerlendirmek ve geçirmek için bir Azure Geçir cihazını nasıl ayarlayamanızı öğrenin.
ms.topic: article
ms.date: 11/18/2019
ms.openlocfilehash: 0447443bb6a642cac09566450ea2d9bb6f6453d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80337668"
---
# <a name="set-up-an-azure-migrate-appliance"></a>Azure Geçiş cihazı ayarlama

Bu makalede, Azure Geçiş cihazını ayarlama seçenekleri özetlenmiştir. 

Azure Geçiş cihazı, şirket içinde hafif bir cihaz dağıtımıdır ve bir dizi senaryoda kullanılır.

**Senaryo** | **Şey**
--- | ---
Azure Geçiş:Sunucu Değerlendirmesi ile VMware VM'leri Değerlendirin | VM uygulamalarını ve bağımlılıklarını keşfedin<br/><br/> Değerlendirmeler için makine meta verilerini ve performans meta verilerini toplayın.
Azure Geçişile Hyper-V VM'leri Değerlendirin:Sunucu Değerlendirmesi | Hyper-V VM'leri Keşfedin<br/><br/> Değerlendirmeler için makine meta verilerini ve performans meta verilerini toplayın.
Fiziksel makineleri değerlendirin | Makineleri fiziksel sunucu lar olarak keşfedin<br/><br/> Değerlendirmeler için makine meta verilerini ve performans meta verilerini toplayın.
VMware VM'leri aracısız geçişle çoğaltın. | Çoğaltmak istediğiniz VM'lere hiçbir şey yüklemeden VMware VM'leri çoğaltın.


## <a name="deployment-options"></a>Dağıtım seçenekleri

Cihazı birkaç şekilde dağıtabilirsiniz.

**Senaryo** | **Şablon** | **Betik** 
--- | --- | --- | ---
**VMware VM'leri değerlendirin** | İndirilen BIR OVA şablonuyla dağıtın.<br/><br/> Bir [şablon kullanarak](how-to-set-up-appliance-vmware.md)cihazı nasıl dağıtacağımız veya değerlendirme öğreticisini nasıl [başlatacaklarınızı](tutorial-prepare-vmware.md) ve öğretici sırasında cihazı bir şablonla nasıl dağıtacağımı öğrenin.  | PowerShell yükleyici komut dosyası kullanarak dağıtın.<br/><br/>  Cihaz komut dosyası yönergelerini [gözden geçirin.](deploy-appliance-script.md)
**Hyper-V VM'leri Değerlendirin** | İndirilen VHD şablonuyla dağıtın. <br/><br/> Bir [şablon kullanarak](how-to-set-up-appliance-vmware.md)cihazı nasıl dağıtacağımız veya değerlendirme öğreticisini nasıl [başlatacaklarınızı](tutorial-prepare-vmware.md) ve öğretici sırasında cihazı bir şablonla nasıl dağıtacağımı öğrenin. | PowerShell yükleyici komut dosyası kullanarak dağıtın.<br/><br/> Cihaz komut dosyası yönergelerini [gözden geçirin.](deploy-appliance-script.md) 
**Fiziksel sunucuları değerlendirme** | Şablon yok. | PowerShell yükleyici komut dosyası kullanarak dağıtın.<br/><br/> [Cihaz komut dosyası yönergelerini](how-to-set-up-appliance-physical.md)gözden geçirin veya değerlendirme [öğreticisini](tutorial-prepare-physical.md)başlatın ve öğretici sırasında cihazı dağıtın.
**VMware VM'leri çoğaltma (aracısız)** | İndirilen BIR OVA şablonuyla dağıtın.<br/><br/> Çoğaltdığınız VM'leri zaten değerlendirdiyseniz, değerlendirme sırasında cihazı zaten kurmuşsunuz demektir.<br/><br/> VMware VM'leri değerlendirmeden çoğaltırsanız, bir şablon kullanarak cihazı nasıl dağıtacağınız öğrenin veya [aracısız geçiş öğreticisini](tutorial-migrate-vmware.md)başlatın ve öğretici sırasında cihazı bir şablonla dağıtın. | PowerShell yükleyici komut dosyası kullanarak dağıtın. <br/><br/> Cihaz komut dosyası yönergelerini [gözden geçirin.](deploy-appliance-script.md) 




## <a name="next-steps"></a>Sonraki adımlar

Cihaz gereksinimlerini [gözden geçirin.](migrate-appliance.md)