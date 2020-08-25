---
title: Azure VMware çözümü için sanal Işlemleri ayarlama
description: Azure VMware çözümünüz özel bulutunuz için sanal Işlemleri ayarlamayı öğrenin.
ms.topic: how-to
ms.date: 08/06/2020
ms.openlocfilehash: 729ee5c64776d7d04f702af62451175f7c53421b
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88750390"
---
# <a name="set-up-vrealize-operations-for-azure-vmware-solution"></a>Azure VMware çözümü için sanal Işlemleri ayarlama


Vfarkında Operations Manager, VMware altyapı yöneticilerinin sistem kaynaklarını izlemesine izin veren bir Operations Management platformudur. Bu sistem kaynakları uygulama düzeyi veya altyapı düzeyi (fiziksel ve sanal) nesneler olabilir. Geçmişte, çoğu VMware Yöneticisi, VMware özel bulut bileşenlerini (vCenter, ESXi, NSX-T, vSAN ve Hibrix) izlemek ve yönetmek için sanal Işlemleri kullandı. Her Azure VMware çözümü özel bulutu, adanmış bir vCenter, NSX-T, vSAN ve HCX dağıtımıyla birlikte sağlanır. 

[Başlamadan önce](#before-you-begin) ve [ön koşullardan](#prerequisites) önce ayrıntılı bir şekilde gözden geçirin. Daha sonra, Azure VMware çözümü ile sanal Operations Manager için iki tipik dağıtım topolojisi boyunca size kılavuzluk ederiz:

> [!div class="checklist"]
> * [Azure VMware çözüm dağıtımını yöneten şirket içi sanal Işlemler](#on-premises-vrealize-operations-managing-azure-vmware-solution-deployment)
> * [Azure VMware Çözüm dağıtımında çalışan sanal Işlemler](#vrealize-operations-running-on-azure-vmware-solution-deployment)

## <a name="before-you-begin"></a>Başlamadan önce
* Vanme Işlemleri dağıtma hakkında daha fazla bilgi edinmek için [Operations Manager ürün belgelerini](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-7FFC61A0-7562-465C-A0DC-46D092533984.html) gözden geçirin. 
* Temel Azure VMware çözümü yazılım tanımlı veri merkezi (SDDC) [öğretici serisini](tutorial-network-checklist.md)gözden geçirin.
* İsteğe bağlı olarak, Azure VMware Çözüm dağıtımı seçeneğini yöneten şirket içi sanal Işlemler için [sanal Işlemler uzak denetleyicisi](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-263F9219-E801-4383-8A59-E84F3D01ED6B.html) ürün belgelerini gözden geçirin. 



## <a name="prerequisites"></a>Ön koşullar
* VPN veya Azure ExpressRoute, şirket içi ve Azure VMware çözümü SDDC arasında yapılandırılmalıdır.
* Azure 'da bir Azure VMware çözümü özel bulutu dağıtıldı.



## <a name="on-premises-vrealize-operations-managing-azure-vmware-solution-deployment"></a>Azure VMware çözüm dağıtımını yöneten şirket içi sanal Işlemler
Çoğu müşterinin, bir veya daha fazla şirket içi vCenter etki alanını yönetmek için kullanılan, şirket içi bir sanal Işlem dağıtımı vardır. Müşteriler Azure 'da yeni bir Azure VMware çözümü özel bulutu sağlarken, genellikle aşağıda gösterildiği gibi bir Azure ExpressRoute veya bir katman 3 VPN çözümü kullanarak şirket içi ortamınızı Azure VMware çözümüyle birbirine bağlayabilirler.   

:::image type="content" source="media/vrealize-operations-manager/vrealize-operations-deployment-option-1.png" alt-text="Azure VMware çözüm dağıtımını yöneten şirket içi sanal Işlemler"  border="false":::

Sanal Işlem yeteneklerini Azure VMware Çözüm özel bulutuna genişletmek için, Azure VMware çözümü özel bulutundaki verileri toplamak ve şirket içi sanal Işlem Işlemlerine getirmek için [Azure VMware çözümü özel bulut kaynakları için](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.config.doc/GUID-640AD750-301E-4D36-8293-1BFEB67E2600.html) bir bağdaştırıcı örneği oluşturursunuz. Şirket içi sanal Operations Manager örneği, Azure VMware çözümünde vCenter ve NSX-T yöneticisine doğrudan bağlanabilir ya da isteğe bağlı olarak Azure VMware Çözüm özel bulutu 'nda bir sanal Işlem uzak toplayıcısı dağıtabilirsiniz. Bir sanal Işlem uzak toplayıcı, ExpressRoute veya VPN ağı üzerinden, şirket içinde çalışan Operations Manager sanal makine üzerinden gönderilmeden önce Azure VMware Çözüm özel bulutlarından toplanan verileri sıkıştırır ve şifreler. 

> [!TIP]
> Vfarkında Operations Manager yükleme hakkında adım adım kılavuz için [VMware belgelerine](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-7FFC61A0-7562-465C-A0DC-46D092533984.html) bakın. 



## <a name="vrealize-operations-running-on-azure-vmware-solution-deployment"></a>Azure VMware Çözüm dağıtımında çalışan sanal Işlemler

Başka bir dağıtım seçeneği, aşağıda gösterildiği gibi, Azure VMware Çözüm özel bulutu 'ndaki vSphere kümelerinden birine bir vingOperations Manager örneği dağıtmaktır. 

:::image type="content" source="media/vrealize-operations-manager/vrealize-operations-deployment-option-2.png" alt-text="Azure VMware çözümünde çalışan sanal Işlemler" border="false":::

VNET 'in Azure VMware Çözüm örneğini dağıttıktan sonra, vCenter, ESXi, NSX-T, vSAN ve HCX 'ten veri toplamak için sanal Işlemleri yapılandırabilirsiniz. 

> [!TIP]
> Vfarkında Operations Manager yükleme hakkında adım adım kılavuz için [VMware belgelerine](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-7FFC61A0-7562-465C-A0DC-46D092533984.html) bakın.




<!-- LINKS - external -->


<!-- LINKS - internal -->




