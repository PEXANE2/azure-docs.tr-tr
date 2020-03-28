---
title: Azure Site Kurtarma ile Azure VM'lerini başka bir bölgeye taşıma
description: Azure VM'lerini bir Azure bölgesinden diğerine taşımak için Azure Site Kurtarma'yı kullanma.
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: 3f715af835df6783ae5d59dd073a042a553fba4d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75498054"
---
# <a name="moving-azure-vms-to-another-azure-region"></a>Azure VM'lerini başka bir Azure bölgesine taşıma

Bu makalede, [Azure Site Kurtarma'yı](site-recovery-overview.md)kullanarak Azure VM'lerinin başka bir Azure bölgesine taşınmasıyla ilgili nedenler ve adımlara genel bir bakış sağlanmaktadır. 


## <a name="reasons-to-move-azure-vms"></a>Azure VM'lerini taşımak için nedenler

Aşağıdaki nedenlerle VM'leri taşıyabilirsiniz:

- Zaten bir bölgede dağıtım yapmışsınız ve uygulamanızın veya hizmetin son kullanıcılarına daha yakın yeni bir bölge desteği eklendi. Bu senaryoda, gecikme süresini azaltmak için VM'lerinizi yeni bölgeye olduğu gibi taşımak isteyebilirsiniz. Abonelikleri birleştirmek istiyorsanız veya taşımanızı gerektiren yönetim veya kuruluş kuralları varsa aynı yaklaşımı kullanın.
- VM'niz tek örnekli VM olarak veya kullanılabilirlik kümesinin bir parçası olarak dağıtıldı. Kullanılabilirlik SLA'larını artırmak istiyorsanız, VM'lerinizi kullanılabilirlik bölgesine taşıyabilirsiniz.

## <a name="steps-to-move-azure-vms"></a>Azure VM'leri taşıma adımları

Hareketli VM'ler aşağıdaki adımları içerir:

1. Önkoşulları doğrulama.
2. Kaynak VM'leri hazırlayın.
3. Hedef bölgeyi hazırlayın.
4. Verileri hedef bölgeye kopyalayın. Kaynak VM'deki verileri hedef bölgeye kopyalamak için Azure Site Kurtarma çoğaltma teknolojisini kullanın.
5. Yapılandırmayı test edin. Çoğaltma tamamlandıktan sonra, üretim dışı bir ağa bir test başarısız gerçekleştirerek yapılandırmayı test edin.
6. Hareketi gerçekleştirin.
7. Kaynak bölgedeki kaynakları atın.

> [!NOTE]
> Bu adımlarla ilgili ayrıntılar aşağıdaki bölümlerde verilmiştir.
> [!IMPORTANT]
> Şu anda Azure Site Kurtarma, VM'lerin bir bölgeden diğerine taşınmasını destekler, ancak bir bölge içinde taşınmayı desteklemez.

## <a name="typical-architectures-for-a-multi-tier-deployment"></a>Çok katmanlı dağıtım için tipik mimariler

Bu bölümde, Azure'da çok katmanlı bir uygulama için en yaygın dağıtım mimarileri açıklanmaktadır. Örnek, genel BIR IP'ye sahip üç katmanlı bir uygulamadır. Katmanların her birinde (web, uygulama ve veritabanı) her biri iki VM'dir ve bunlar bir Azure yük dengeleyicisi tarafından diğer katmanlara bağlanır. Veritabanı katmanı, yüksek kullanılabilirlik için VM'ler arasında SQL Server Always On çoğaltma vardır.

* **Çeşitli katmanlarda dağıtılan tek örnekli VM'ler**: Bir katmandaki her VM, tek örnekli VM olarak yapılandırılır ve yük dengeleyicileri tarafından diğer katmanlara bağlanır. Bu yapılandırmayı benimsemesi en basit yapılandırmadır.

     ![Katmanlar arasında tek örnekli VM dağıtımı](media/move-vm-overview/regular-deployment.png)

* **Kullanılabilirlik kümeleri arasında dağıtılan her kademedeki VM'ler**: Bir katmandaki her VM, bir kullanılabilirlik kümesinde yapılandırılır. [Kullanılabilirlik kümeleri,](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) Azure'da dağıttığınız VM'lerin kümedeki birden çok yalıtılmış donanım düğümüne dağıtılmasını sağlar. Bu, Azure'da bir donanım veya yazılım hatası olursa, Yalnızca VM'lerinizin bir alt kümesinin etkilenmesini ve genel çözümünizin kullanılabilir ve çalışır durumda kalmasını sağlar.

     ![Kullanılabilirlik kümeleri arasında VM dağıtımı](media/move-vm-overview/avset.png)

* **Kullanılabilirlik Bölgeleri arasında dağıtılan her kademedeki VM'ler**: Bir katmandaki her [VM, Kullanılabilirlik Bölgeleri](https://docs.microsoft.com/azure/availability-zones/az-overview)arasında yapılandırılır. Azure bölgesindeki Kullanılabilirlik Bölgesi, hata etki alanı ve güncelleştirme etki alanının birleşimidir. Örneğin, bir Azure bölgesinde üç bölgede üç veya daha fazla VM oluşturursanız, VM'leriniz üç hata etki alanına ve üç güncelleştirme etki alanına etkili bir şekilde dağıtılır. Azure platformu, farklı bölgelerdeki VM'lerin aynı anda güncelleştirilemediğinden emin olmak için bu dağıtımı güncelleştirme etki alanlarında tanır.

     ![Kullanılabilirlik Bölgesi dağıtımı](media/move-vm-overview/zone.png)

## <a name="move-vms-as-is-to-a-target-region"></a>VM'leri hedef bölgeye olduğu gibi taşıyın

Daha önce bahsedilen [mimarilere](#typical-architectures-for-a-multi-tier-deployment) dayanarak, hedef bölgeye olduğu gibi hareketi gerçekleştirdikten sonra dağıtımlar şu şekilde görünecektir.

* **Çeşitli katmanlarda dağıtılan tek örnekli VM'ler**

     ![Katmanlar arasında tek örnekli VM dağıtımı](media/move-vm-overview/single-zone.png)

* **Kullanılabilirlik kümeleri arasında dağıtılan her katmandaki VM'ler**

     ![Bölgeler arası kullanılabilirlik kümeleri](media/move-vm-overview/crossregionaset.png)

* **Kullanılabilirlik Bölgeleri arasında dağıtılan her katmandaki VM'ler**

     ![Kullanılabilirlik Bölgeleri arasında VM dağıtımı](media/move-vm-overview/azonecross.png)

## <a name="move-vms-to-increase-availability"></a>Kullanılabilirliği artırmak için VM'leri taşıma

* **Çeşitli katmanlarda dağıtılan tek örnekli VM'ler**

     ![Katmanlar arasında tek örnekli VM dağıtımı](media/move-vm-overview/single-zone.png)

* **Kullanılabilirlik kümeleri arasında dağıtılan her katmandaki VM'ler**: Azure Site Kurtarma'yı kullanarak VM'niz için çoğaltmayı etkinleştirdiğinizde, Kullanılabilirlik kümesinde VM'lerinizi ayrı Kullanılabilirlik Bölgeleri olarak yapılandırabilirsiniz. Kullanılabilirlik için SLA, taşıma işlemini tamamladıktan sonra %99,99 olacaktır.

     ![Kullanılabilirlik kümeleri ve Kullanılabilirlik Bölgeleri arasında VM dağıtımı](media/move-vm-overview/aset-azone.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> 
> * [Azure VM’lerini başka bir bölgeye taşıma](azure-to-azure-tutorial-migrate.md)
> 
> * [Azure VM'lerini Kullanılabilirlik Alanlarına taşıma](move-azure-vms-avset-azone.md)

