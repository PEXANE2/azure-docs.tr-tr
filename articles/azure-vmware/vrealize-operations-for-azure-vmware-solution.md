---
title: Azure VMware çözümü için sanal Işlemleri ayarlama
description: Azure VMware çözümünüz özel bulutunuz için sanal Işlemleri ayarlamayı öğrenin.
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 9e512d107ddc4d9bca28323658d09f4b4b378dc3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91580679"
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

:::image type="content" source="media/vrealize-operations-manager/vrealize-operations-deployment-option-2.png" alt-text="Azure VMware çözüm dağıtımını yöneten şirket içi sanal Işlemler" border="false":::

VNET 'in Azure VMware Çözüm örneğini dağıttıktan sonra, vCenter, ESXi, NSX-T, vSAN ve HCX 'ten veri toplamak için sanal Işlemleri yapılandırabilirsiniz. 

> [!TIP]
> Vfarkında Operations Manager yükleme hakkında adım adım kılavuz için [VMware belgelerine](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-7FFC61A0-7562-465C-A0DC-46D092533984.html) bakın.


## <a name="known-limitations"></a>Bilinen sınırlamalar

- **cloudadmin@vsphere.local**Azure VMware çözümünde kullanıcının [sınırlı ayrıcalıkları](concepts-role-based-access-control.md)vardır. Azure VMware çözümünde sanal makineler (VM) ile VMware araçları kullanılarak Konuk içi bellek koleksiyonu desteklenmez. Etkin ve tüketilen bellek kullanımı belirlemeleri bu durumda çalışmaya devam eder.
- Azure VMware çözümleri, DRS ayarları dahil olmak üzere küme yapılandırmasını yönettiğinden, ana bilgisayar tabanlı iş amacı için iş yükü iyileştirmesi çalışmaz.
- Küme tabanlı iş amacını kullanarak SDDC içindeki çapraz küme yerleştirme için iş yükü iyileştirmesi Operations Manager 8,0 ve sonraki sürümlerde tamamen desteklenir. Ancak, iş yükü iyileştirmesi kaynak havuzlarından haberdar değildir ve sanal makineleri küme düzeyine koyar. Bir Kullanıcı bunu Azure VMware Çözüm vCenter Server arabiriminde el ile düzeltebilir.
- Azure VMware çözümünüzü vCenter Server kimlik bilgilerinizi kullanarak Operations Manager VNET 'te oturum açılamıyor. 
- Azure VMware çözümü, Vfarkında Operations Manager eklentisini desteklemez.

Azure VMware Çözüm vCenter 'ı vCenter Server bir bulut hesabı kullanarak Operations Manager Vfarkında 'e bağlarken aşağıdaki uyarıyla karşılaşırsınız:

:::image type="content" source="./media/vrealize-operations-manager/warning-adapter-instance-creation-succeeded.png" alt-text="Azure VMware çözüm dağıtımını yöneten şirket içi sanal Işlemler":::

**cloudadmin@vsphere.local**Azure VMware çözümündeki Kullanıcı kayıt için gereken tüm vCenter Server eylemlerini yapmak için yeterli ayrıcalıklara sahip olmadığından uyarı oluşur. Ancak, ayrıcalıklar aşağıda görüldüğü gibi, bağdaştırıcı örneğinin veri toplamayı yapması için yeterlidir:

:::image type="content" source="./media/vrealize-operations-manager/adapter-instance-to-perform-data-collection.png" alt-text="Azure VMware çözüm dağıtımını yöneten şirket içi sanal Işlemler":::

Daha fazla bilgi için bkz. [vCenter bağdaştırıcı örneği yapılandırmak Için gereken ayrıcalıklar](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.core.doc/GUID-3BFFC92A-9902-4CF2-945E-EA453733B426.html).

<!-- LINKS - external -->


<!-- LINKS - internal -->




