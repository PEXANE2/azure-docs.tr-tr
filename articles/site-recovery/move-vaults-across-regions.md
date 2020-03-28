---
title: Azure Site Kurtarma kasasını başka bir bölgeye taşıma
description: Kurtarma Hizmetleri kasasının (Azure Site Kurtarma) başka bir Azure bölgesine nasıl taşınabileceğinizi açıklar
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/31/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: 32dff9a165125ab1949560ce36438ae266cd3036
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74090307"
---
# <a name="move-a-recovery-services-vault-and-azure-site-recovery-configuration-to-another-azure-region"></a>Kurtarma Hizmetleri kasası ve Azure Site Kurtarma yapılandırmasını başka bir Azure bölgesine taşıma

Varolan Azure kaynaklarınızı bir bölgeden diğerine taşımak isteyebileceğin çeşitli senaryolar vardır. Örnekler yönetilebilirlik, yönetişim nedenleri veya şirket birleşme leri ve satın almaları dır. Azure VM'lerinizi taşırken taşımak isteyebileceğin ilgili kaynaklardan biri olağanüstü durum kurtarma yapılandırmasıdır. 

Varolan bir olağanüstü durum kurtarma yapılandırmasını bir bölgeden diğerine taşımanın birinci sınıf bir yolu yoktur. Bunun nedeni, hedef bölgenizi kaynak VM bölgenize göre yapılandırmanızdır. Kaynak bölgeyi değiştirmeye karar verdiğinizde, hedef bölgenin önceden varolan yapılandırmaları yeniden kullanılamaz ve sıfırlanmalıdır. Bu makalede, olağanüstü durum kurtarma kurulumyeniden yapılandırmak ve farklı bir bölgeye taşımak için adım adım işlem tanımlanır.

Bu belgede şunları yapacaksınız:

> [!div class="checklist"]
> * Taşıma için ön koşulları doğrulayın.
> * Azure Site Kurtarma tarafından kullanılan kaynakları tanımlayın.
> * Çoğaltmayı devre dışı.
> * Kaynakları silin.
> * VM'ler için yeni kaynak bölgeye dayalı Site Kurtarma'yı ayarlayın.

> [!IMPORTANT]
> Şu anda, kurtarma hizmetleri kasasını ve olağanüstü durum kurtarma yapılandırmasını farklı bir bölgeye taşımanın birinci sınıf bir yolu yoktur. Bu makalede, çoğaltma devre dışı bırakma ve yeni bölgede kurma işlemi boyunca size rehberlik eder.

## <a name="prerequisites"></a>Ön koşullar

- Azure VM'leri farklı bir bölgeye taşımaya çalışmadan önce olağanüstü durum kurtarma yapılandırmasını kaldırdığınızdan ve sildiğinizden emin olun. 

  > [!NOTE]
  > Azure VM için yeni hedef bölgeniz olağanüstü durum kurtarma hedef bölgesiyle aynıysa, varolan çoğaltma yapılandırmanızı kullanabilir ve taşıyabilirsiniz. [Azure IaaS VM'lerini başka bir Azure bölgesine taşıyın adımlarını](azure-to-azure-tutorial-migrate.md)izleyin.

- Bilinçli bir karar aldığınızdan ve paydaşların bilgilendirildiğinden emin olun. VM'inizin hareketi tamamlanana kadar VM'iniz felaketlere karşı korunmaz.

## <a name="identify-the-resources-that-were-used-by-azure-site-recovery"></a>Azure Site Kurtarma tarafından kullanılan kaynakları belirleme
Bir sonrakine geçmeden önce bu adımı yapmanızı öneririz. VM'ler çoğaltılırken ilgili kaynakları tanımlamak daha kolaydır.

Çoğaltılan her Azure VM **için, Korumalı Öğeler** > **Çoğaltılan Öğeler** > **Özellikleri'ne** gidin ve aşağıdaki kaynakları tanımlayın:

- Hedef kaynak grubu
- Önbellek depolama hesabı
- Hedef depolama hesabı (yönetilmeyen disk tabanlı Azure VM durumunda) 
- Hedef ağ


## <a name="disable-the-existing-disaster-recovery-configuration"></a>Varolan olağanüstü durum kurtarma yapılandırmasını devre dışı

1. Kurtarma Hizmetleri kasasına git.
2. **Korumalı Öğeler** > **Çoğaltılan Öğeler'de**makineyi sağ tıklatın ve **çoğaltmayı devre dışı dışı kseçin.**
3. Taşımak istediğiniz tüm VM'ler için bu adımı yineleyin.

> [!NOTE]
> Mobilite hizmeti korumalı sunuculardan kaldırılamaz. El ile kaldırmanız gerekir. Sunucuyu yeniden korumayı planlıyorsanız, mobilite hizmetini kaldırmayı atlayabilirsiniz.

## <a name="delete-the-resources"></a>Kaynakları silme

1. Kurtarma Hizmetleri kasasına git.
2. **Sil**’i seçin.
3. [Daha önce tanımladığınız](#identify-the-resources-that-were-used-by-azure-site-recovery)diğer tüm kaynakları silin.
 
## <a name="move-azure-vms-to-the-new-target-region"></a>Azure VM'leri yeni hedef bölgeye taşıma

Azure VM'lerini hedef bölgeye taşıma gereksiniminize bağlı olarak bu makalelerdeki adımları izleyin:

- [Azure VM’lerini başka bir bölgeye taşıma](azure-to-azure-tutorial-migrate.md)
- [Azure VM'lerini Kullanılabilirlik Alanlarına taşıma](move-azure-VMs-AVset-Azone.md)

## <a name="set-up-site-recovery-based-on-the-new-source-region-for-the-vms"></a>VM'ler için yeni kaynak bölgeye göre Site Kurtarma'yı ayarlama

Azure VM'leri için olağanüstü durum kurtarma yı ayarlama adımlarını izleyerek yeni bölgeye taşınan Azure VM'leri [için olağanüstü durum kurtarmayı](azure-to-azure-tutorial-enable-replication.md)yapılandırın.
