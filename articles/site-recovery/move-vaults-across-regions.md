---
title: Azure Site Recovery yapılandırması başka bir Azure bölgesine taşınıyor | Microsoft Docs
description: Site Recovery yapılandırmasını başka bir Azure bölgesine taşımaya yönelik kılavuz
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/31/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: ba0e2577d6fb8bac66322917936fe06825af0d96
ms.sourcegitcommit: 6ad03fa28a0f60cb6dce6144f728c2ceb56ff6e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68708301"
---
# <a name="moving-recovery-services-vault-and--azure-site-recovery-configuration-to-another-azure-region"></a>Kurtarma Hizmetleri kasasını ve Azure Site Recovery yapılandırmasını başka bir Azure bölgesine taşıma

Mevcut Azure kaynaklarınızı bir bölgeden başka yönetilebilirlik, idare nedenlerinden veya şirket merbilirlerinin/alımlarından kaynaklanan nedenlerle taşımak istediğiniz çeşitli senaryolar vardır. Azure VM 'Leri taşırken taşımak isteyebileceğiniz ilgili kaynaklardan biri aynı durum için olağanüstü durum kurtarma yapılandırması olur. Mevcut bir olağanüstü durum kurtarma yapılandırmasını bir bölgeden diğerine taşımanın ilk sınıf yolu yoktur. Bu aslında hedef bölgenizi kaynak VM bölgenize göre yapılandırdığınıza ve bunu değiştirmeye karar verirken, hedef bölgenin daha önce var olan yapılandırmalarının yeniden kullanılamaz ve sıfırlanması gerekir. Bu makale, olağanüstü durum kurtarma kurulumunu yeniden yapılandırmak ve farklı bir bölgeye taşımak için adım adım işlemi tanımlar.

Bu belgede şunları yapmanız gerekir:

> [!div class="checklist"]
> * Taşıma için önkoşulları doğrulama
> * Azure Site Recovery tarafından kullanılan kaynakları belirler 
> * Çoğaltmayı devre dışı bırakma
> * Kaynakları Sil 
> * VM 'Lerin yeni kaynak bölgesine bağlı olarak Site Recovery 'yi yeniden kurun.

> [!IMPORTANT]
> Şu anda bir kurtarma hizmetleri kasasını ve DR yapılandırmasını farklı bir bölgeye taşımak için birinci sınıf yolu yoktur, bu belge, çoğaltmayı devre dışı bırakma ve yeni bölgede sıfırlama sürecinde müşteriye kılavuzluk eder.

## <a name="prerequisites"></a>Önkoşullar

- Azure VM 'lerini farklı bir bölgeye taşımayı denemeden önce olağanüstü durum kurtarma yapılandırmasını kaldırıp sildiğinizden emin olun. 

> [!NOTE]
> Azure VM için yeni hedef bölgeniz olağanüstü durum kurtarma hedefi bölgesiyle aynıysa, mevcut çoğaltma yapılandırmanızı kullanabilir ve [burada](azure-to-azure-tutorial-migrate.md) bahsedilen adımlara göre hareket edebilirsiniz 

- Bilinçli bir karar verirken ve sanal makinenizin, VM 'nin taşınması tamamlanana kadar olağanüstü durumlara karşı korunmadığından emin olun. 

## <a name="identify-the-resources-that-were-used-by-azure-site-recovery"></a>Azure Site Recovery tarafından kullanılan kaynakları belirler
VM 'Ler hala çoğaltılırken ilgili kaynakları belirlemek daha kolay olacağı için, bir sonraki adıma geçmeden önce bu adımı yapmanız önerilir

Çoğaltmakta olan her bir Azure VM için, **korumalı öğeler** > **çoğaltılan öğeler**>**özellikleri** ' ne gidin ve aşağıdaki kaynakları tanımla

- Hedef kaynak grubu
- Önbellek depolama hesabı
- Hedef depolama hesabı (yönetilmeyen disk tabanlı Azure VM olması durumunda) 
- Hedef ağ


## <a name="disable-existing-disaster-recovery-configuration"></a>Mevcut olağanüstü durum kurtarma yapılandırmasını devre dışı bırak

1. **Kurtarma Hizmetleri kasasına** gitme 
2.  **Korunan öğeler** > **çoğaltılan öğeler**bölümünde, **çoğaltmayı devre dışı bırakmak**> makineye sağ tıklayın.
3. Taşımak istediğiniz tüm VM 'Ler için bunu tekrarlayın.
> [!NOTE]
> Mobility hizmeti korumalı sunuculardan kaldırılmaz, el ile kaldırmanız gerekir. Sunucuyu yeniden korumayı planlıyorsanız Mobility hizmetini kaldırmayı atlayabilirsiniz.

## <a name="delete-the-resources"></a>Kaynakları Sil

1. **Kurtarma Hizmetleri kasasına** git
2. **Sil** 'e tıklayın
3. [Önceki adımda](#identify-the-resources-that-were-used-by-azure-site-recovery) tanımlanan diğer tüm kaynakları silmeye devam edin
 
## <a name="move-azure-vms-to-the-new-target-region"></a>Azure VM 'lerini yeni hedef bölgeye taşıma

Azure VM 'lerini hedef bölgeye taşımak için gereksiniminize bağlı olarak aşağıda bahsedilen adımları izleyin.

- [Azure VM’lerini başka bir bölgeye taşıma](azure-to-azure-tutorial-migrate.md)
- [Azure VM'lerini Kullanılabilirlik Alanlarına taşıma](move-azure-VMs-AVset-Azone.md)

## <a name="re-set-up-site-recovery-based-on-the-new-source-region-for-the-vms"></a>VM 'Lerin yeni kaynak bölgesine göre Site Recovery yeniden kurun

[Burada](azure-to-azure-tutorial-enable-replication.md) bahsedilen adımlar kullanılarak yeni bölgeye taşınmış olan Azure VM 'leri için olağanüstü durum kurtarmayı yapılandırın
