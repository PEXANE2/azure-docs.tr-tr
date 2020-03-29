---
title: Azure Site Kurtarma ile şirket içi olağanüstü durum kurtarma için ağ bağdaştırıcılarını yönetme
description: Azure Site Kurtarma ile Azure'da şirket içi olağanüstü durum kurtarma için ağ arabirimlerinin nasıl yönetilenaçıklanmaktadır
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 4/9/2019
ms.author: mayg
ms.openlocfilehash: 2a4752b501e40f9e8a4f3bc82cb2533c11f9e526
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73954599"
---
# <a name="manage-vm-network-interfaces-for-on-premises-disaster-recovery-to-azure"></a>Azure'da şirket içi olağanüstü durum kurtarma için VM ağ arabirimlerini yönetme

Azure'daki sanal bir makinenin (VM) en az bir ağ arabirimi olmalıdır. VM boyutu desteklediği kadar bağlı birçok ağ arabirimi olabilir.

Varsayılan olarak, bir Azure sanal makinesine bağlı ilk ağ arabirimi birincil ağ arabirimi olarak tanımlanır. Sanal makinedeki diğer tüm ağ arabirimleri ikincil ağ arabirimleridir. Ayrıca varsayılan olarak, sanal makineden gelen tüm giden trafik, birincil ağ arabiriminin birincil IP yapılandırmasına atanan IP adresine gönderilir.

Şirket içi ortamda, sanal makineler veya sunucular ortam içindeki farklı ağlar için birden çok ağ arabirimine sahip olabilir. Farklı ağlar genellikle yükseltmeler, bakım ve internet erişimi gibi belirli işlemleri gerçekleştirmek için kullanılır. Şirket içi bir ortamdan Azure'a geçiş yaparken veya başarısız olduğunuzda, aynı sanal makinedeki ağ arabirimlerinin hepsinin aynı sanal ağa bağlanması gerektiğini unutmayın.

Varsayılan olarak, Azure Site Kurtarma, bir Azure sanal makinesinde şirket içi sunucuya bağlı olduğu kadar çok ağ arabirimi oluşturur. Yinelenen sanal makine ayarlarının altında ağ arabirimi ayarlarını düzenleyerek geçiş sırasında gereksiz ağ arabirimleri oluşturmaktan veya başarısız lığı önlemekten kaçınabilirsiniz.

## <a name="select-the-target-network"></a>Hedef ağı seçin

VMware ve fiziksel makineler ve Hyper-V (System Center Virtual Machine Manager olmadan) sanal makineler için, tek tek sanal makineler için hedef sanal ağı belirtebilirsiniz. Virtual Machine Manager ile yönetilen Hyper-V sanal makineler için, kaynak Sanal Makine Yöneticisi sunucusundaki VM ağlarını eşlemek ve Azure ağlarını hedeflemek için [ağ eşlemeyi](site-recovery-network-mapping.md) kullanın.

1. Kurtarma Hizmetleri kasasında **çoğaltılan öğeler** altında, çoğaltılan öğenin ayarlarına erişmek için çoğaltılan herhangi bir öğeyi seçin.

2. Yinelenen öğenin ağ ayarlarına erişmek için **Bilgi İşlem ve Ağ** sekmesini seçin.

3. **Ağ özellikleri**altında, kullanılabilir ağ arabirimleri listesinden bir sanal ağ seçin.

    ![Ağ ayarları](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/compute-and-network.png)

Hedef ağın değiştirilmesi, belirli bir sanal makine için tüm ağ arabirimlerini etkiler.

Sanal Makine Yöneticisi bulutları için, ağ eşlemi nin değiştirilmesi tüm sanal makineleri ve ağ arabirimlerini etkiler.

## <a name="select-the-target-interface-type"></a>Hedef arabirim türünü seçin

**Bilgi İşlem ve Ağ** bölmesinin Ağ **arabirimleri** bölümü altında ağ arabirimi ayarlarını görüntüleyebilir ve değiştirebilirsiniz. Hedef ağ arabirimi türünü de belirtebilirsiniz.

- Başarısız olmak için **Birincil** ağ arabirimi gereklidir.
- Varsa diğer tüm seçili ağ arabirimleri **İkincil** ağ arabirimleridir.
- Ağ **Do not use** arabirimini başarısız bir şekilde oluşturmayı dışlamak için kullanmayın'ı seçin.

Varsayılan olarak, çoğaltmayı etkinleştirirken, Site Kurtarma şirket içi sunucuda algılanan tüm ağ arabirimlerini seçer. Biri **Birincil,** diğerlerini de **Ikincil**olarak işaretler. Şirket içi sunucuya eklenen sonraki arabirimler varsayılan olarak **kullanmayın** olarak işaretlenir. Daha fazla ağ arabirimi eklerken, gerekli tüm ağ arabirimlerini karşılamak için doğru Azure sanal makine hedef boyutunun seçildiğinden emin olun.

## <a name="modify-network-interface-settings"></a>Ağ arabirimi ayarlarını değiştirme

Çoğaltılan bir öğenin ağ arabirimleri için alt ağ ve IP adresini değiştirebilirsiniz. Bir IP adresi belirtilmemişse, Site Kurtarma bir sonraki kullanılabilir IP adresini alt ağdan ağ arabirimine başarısız olarak atar.

1. Ağ arabirimi ayarlarını açmak için kullanılabilir ağ arabirimini seçin.

2. Kullanılabilir alt ağlar listesinden istediğiniz alt ağı seçin.

3. İstenilen IP adresini girin (gerektiği gibi).

    ![Ağ arabirimi ayarları](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/network-interface-settings.png)

4. Düzenlemeyi bitirmek ve İşlem **ve Ağ** bölmesine dönmek için **Tamam'ı** seçin.

5. Diğer ağ arabirimleri için 1-4 adımlarını yineleyin.

6. Tüm değişiklikleri kaydetmek için **Kaydet'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar
  Azure sanal makineleri için ağ arabirimleri hakkında [daha fazla bilgi edinin.](../virtual-network/virtual-network-network-interface-vm.md)
