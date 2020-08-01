---
title: Azure VMware çözümü (AVS) için sanal Işlem Işlemlerini ayarlama
description: ''
ms.topic: how-to
ms.date: 08/06/2020
ms.openlocfilehash: 57b513b681c4d2522b1c92946aab80fe8ba95746
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87476199"
---
# <a name="set-up-vrealize-operations-for-azure-vmware-solution-avs"></a>Azure VMware çözümü (AVS) için sanal Işlem Işlemlerini ayarlama


Vfarkında Operations Manager, VMware altyapı yöneticilerinin sistem kaynaklarını izlemesine izin veren bir Operations Management platformudur. Bu sistem kaynakları uygulama düzeyi veya altyapı düzeyi (fiziksel ve sanal) nesneler olabilir. Geçmişte, çoğu VMware Yöneticisi, VMware özel bulut bileşenlerini (vCenter, ESXi, NSX-T, vSAN ve Hibrix) izlemek ve yönetmek için sanal Işlemleri kullandı. Her AVS özel bulutu, adanmış bir vCenter, NSX-T, vSAN ve HCX dağıtımıyla birlikte sağlanır. 

[Başlamadan önce](#before-you-begin) ve [ön koşullardan](#prerequisites) önce ayrıntılı bir şekilde gözden geçirin. Daha sonra, AVS ile sanal Operations Manager yönelik iki tipik dağıtım topolojisi boyunca size kılavuzluk ederiz:

> [!div class="checklist"]
> * [Şirket içi sanal Işlemler, AVS dağıtımını yöneten](#on-premises-vrealize-operations-managing-avs-deployment)
> * [AVS dağıtımında çalışan sanal Işlemler](#vrealize-operations-running-on-avs-deployment)

## <a name="before-you-begin"></a>Başlamadan önce
* Vanme Işlemleri dağıtma hakkında daha fazla bilgi edinmek için [Operations Manager ürün belgelerini](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-7FFC61A0-7562-465C-A0DC-46D092533984.html) gözden geçirin. 
* Temel AVS yazılım tanımlı veri merkezi (SDDC) [öğretici serisini](tutorial-network-checklist.md)gözden geçirin.
* İsteğe bağlı olarak, AVS dağıtım seçeneğini yöneten şirket içi sanal Işlemler için [sanal Işlemler uzak denetleyicisi](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-263F9219-E801-4383-8A59-E84F3D01ED6B.html) ürün belgelerini gözden geçirin. 



## <a name="prerequisites"></a>Önkoşullar
* VPN veya Azure ExpressRoute, şirket içi ve AVS SDDC arasında yapılandırılmalıdır.
* Azure 'da bir AVS özel bulutu dağıtıldı.



## <a name="on-premises-vrealize-operations-managing-avs-deployment"></a>Şirket içi sanal Işlemler, AVS dağıtımını yöneten
Çoğu müşterinin, bir veya daha fazla şirket içi vCenter etki alanını yönetmek için kullanılan, şirket içi bir sanal Işlem dağıtımı vardır. Müşteriler, Azure 'da yeni bir AVS özel bulutu sağlarken, genellikle aşağıda gösterildiği gibi bir Azure ExpressRoute veya katman 3 VPN çözümü kullanarak şirket içi ortamınızı AVS ile birbirine bağlayabilirler.   

:::image type="content" source="media/vrealize-operations-manager/vrealize-operations-deployment-option-1.png" alt-text="Şirket içi sanal Işlemler, AVS dağıtımını yöneten"  border="false":::

Sanal Işlemler yeteneklerini AVS özel bulutuna genişletmek için, AVS özel bulut[ kaynakları için](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.config.doc/GUID-640AD750-301E-4D36-8293-1BFEB67E2600.html) bir bağdaştırıcı örneği OLUŞTURURSUNUZ. AVS özel bulutundaki verileri toplamak ve şirket Içi sanal işlem işlemlerine getirmek için. Şirket içi sanal Operations Manager örneği, AVS 'de vCenter ve NSX-T yöneticisine doğrudan bağlanabilir ya da isteğe bağlı olarak AVS özel bulutu 'nda bir sanal Işlem uzak toplayıcısı dağıtabilirsiniz. Bir sanal Işlem uzak toplayıcı, ExpressRoute veya VPN ağı üzerinden şirket içinde çalışan Operations Manager sanal makine üzerinden gönderilmeden önce, AVS özel bulutlarından toplanan verileri sıkıştırır ve şifreler. 

> [!TIP]
> Vfarkında Operations Manager yükleme hakkında adım adım kılavuz için [VMware belgelerine](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-7FFC61A0-7562-465C-A0DC-46D092533984.html) bakın. 



## <a name="vrealize-operations-running-on-avs-deployment"></a>AVS dağıtımında çalışan sanal Işlemler

Başka bir dağıtım seçeneği, aşağıda gösterildiği gibi, AVS özel bulutundaki vSphere kümelerinden birine bir vingOperations Manager örneği dağıtmaktır. 

:::image type="content" source="media/vrealize-operations-manager/vrealize-operations-deployment-option-2.png" alt-text="AVS üzerinde çalışan sanal Işlemler" border="false":::

VNET, sanal Işlemler örneğini dağıttıktan sonra vCenter, ESXi, NSX-T, vSAN ve HCX 'ten veri toplamak için sanal Işlemleri yapılandırabilirsiniz. 

> [!TIP]
> Vfarkında Operations Manager yükleme hakkında adım adım kılavuz için [VMware belgelerine](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-7FFC61A0-7562-465C-A0DC-46D092533984.html) bakın.




<!-- LINKS - external -->


<!-- LINKS - internal -->




