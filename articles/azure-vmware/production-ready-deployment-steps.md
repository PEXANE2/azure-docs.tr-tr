---
title: Azure VMware çözüm dağıtımını planlama
description: Bu makalede bir Azure VMware Çözüm dağıtımı iş akışı özetlenmektedir.  Nihai sonuç, sanal makine (VM) oluşturma ve geçirme için hazırlanma ortamıdır.
ms.topic: tutorial
ms.date: 10/16/2020
ms.openlocfilehash: 9b6d04e1e7a60bf812ca2b1e370c5075d306c432
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93287062"
---
# <a name="planning-the-azure-vmware-solution-deployment"></a>Azure VMware çözüm dağıtımını planlama

Bu makalede, dağıtım sırasında kullanılan verileri belirleyip toplayacağınız planlama süreci sunulmaktadır. Dağıtımınızı planlarken, dağıtım sırasında kolay başvuru için topladığınız bilgileri belgelediğinizden emin olun.

Bu hızlı başlangıç işlemlerine, sanal makineler (VM 'Ler) ve geçiş için üretime hazırlı bir ortam ile sonuçlanır. 

>[!IMPORTANT]
>Azure VMware Çözüm kaynağınızı oluşturmadan önce, konaklarınızın ayrılmaları için bir destek bileti göndermek üzere [Azure VMware Çözüm kaynağını etkinleştirme](enable-azure-vmware-solution.md) makalesini izleyin. Destek ekibi isteğinizi aldıktan sonra, isteğinizi doğrulamak ve konaklarınızı ayırmak için beş iş günü sürer. Mevcut bir Azure VMware çözümü özel bulutunuz varsa ve daha fazla ana bilgisayar ayırmak istiyorsanız, aynı işlemden geçmeniz gerekir. 


## <a name="subscription"></a>Abonelik

Azure VMware çözümünü dağıtmak için kullanmayı planladığınız aboneliği belirler.  Yeni bir abonelik oluşturabilir ya da var olan bir aboneliği yeniden kullanabilirsiniz.

>[!NOTE]
>Aboneliğin bir Microsoft Kurumsal Anlaşma ile ilişkilendirilmesi gerekir.

## <a name="resource-group"></a>Kaynak grubu

Azure VMware çözümünüz için kullanmak istediğiniz kaynak grubunu belirler.  Genellikle, Azure VMware çözümü için bir kaynak grubu oluşturulur, ancak var olan bir kaynak grubunu kullanabilirsiniz.

## <a name="region"></a>Region

Azure VMware çözümünün dağıtılmasını istediğiniz bölgeyi belirler.  Daha fazla bilgi için bkz. [bölgeye göre kullanılabilir Azure ürünleri kılavuzu](https://azure.microsoft.com/en-us/global-infrastructure/services/?products=azure-vmware).

## <a name="resource-name"></a>Kaynak adı

Dağıtım sırasında kullanacağınız kaynak adını tanımlayın.  Kaynak adı, Azure VMware çözümünüz özel bulutunuzu başlık olarak kullanabileceğiniz kolay ve açıklayıcı bir addır.

## <a name="size-hosts"></a>Ana bilgisayarları Boyutlandır

Azure VMware çözümünü dağıttığınızda kullanmak istediğiniz boyut konaklarınızı belirler.  Tüm liste için bkz. [Azure VMware çözümü özel bulutlar ve kümeler](concepts-private-clouds-clusters.md#hosts) belgeleri.

## <a name="number-of-hosts"></a>Ana bilgisayar sayısı

Azure VMware Çözüm özel bulutuna dağıtmak istediğiniz ana bilgisayar sayısını tanımlayın.  En küçük düğüm sayısı üç, küme başına en fazla 16 ' dır.  Daha fazla bilgi için bkz. [Azure VMware çözümü özel bulut ve kümeler](concepts-private-clouds-clusters.md#clusters) belgeleri.

İlk dağıtım numarasının ötesine gitmeniz gerekiyorsa, her zaman kümeyi genişletebilirsiniz.

## <a name="vcenter-admin-password"></a>vCenter yönetici parolası
VCenter yönetici parolasını tanımlayın.  Dağıtım sırasında bir vCenter yönetici parolası oluşturacaksınız. Parola, cloudadmin@vsphere.local vCenter derlemesi sırasında yönetici hesabıdır. VCenter 'da oturum açmak için kullanacaksınız.

## <a name="nsx-t-admin-password"></a>NSX-T yönetici parolası
NSX-T yönetici parolasını tanımlayın.  Dağıtım sırasında bir NSX-T yönetici parolası oluşturacaksınız. Parola NSX derlemesi sırasında NSX hesabındaki yönetici kullanıcısına atanır. NSX-T Manager 'da oturum açmak için kullanacaksınız.

## <a name="ip-address-segment"></a>IP adresi segmenti

Dağıtımı planlamada ilk adım, IP segmentlemesini planlıyor.  Azure VMware çözümü sağladığınız a/22 ağı. Daha sonra bu IP segmentlerini daha küçük parçalara ayırır ve vCenter, VMware HCX, NSX-T ve vMotion için bu IP segmentlerini kullanır.

Azure VMware çözümü, bir iç ExpressRoute devresi aracılığıyla Microsoft Azure Sanal Ağ bağlanır. Çoğu durumda, ExpressRoute Global Reach aracılığıyla veri merkezinize bağlanır. 

Azure VMware çözümü, mevcut Azure ortamınız ve şirket içi ortamınız tüm Exchange yolları (genellikle). Bu durumda, bu adımda tanımladığınız/22 CıDR ağ adresi bloğu, şirket içi veya Azure 'da zaten sahip olduğunuz herhangi bir şeyle çakışmamalıdır.

**Örnek:** 10.0.0.0/22

Daha fazla bilgi için bkz. [ağ planlama denetim listesi](tutorial-network-checklist.md#routing-and-subnet-considerations).

:::image type="content" source="media/pre-deployment/management-vmotion-vsan-network-ip-diagram.png" alt-text="IP adresi kesimini tanımla" border="false":::  

## <a name="ip-address-segment-for-virtual-machine-workloads"></a>Sanal makine iş yükleri için IP adresi segmenti

Özel bulutunuzda ilk ağınızı (NSX segmentinizi) oluşturmak için bir IP segmenti belirler.  Diğer bir deyişle, Azure VMware çözümünde VM 'Leri dağıtabilmeniz için Azure VMware çözümünde bir ağ kesimi oluşturmak istersiniz.   

Yalnızca L2 ağlarını genişletmeyi planlıyor olsanız bile, ortamı doğrulayacak bir ağ kesimi oluşturun.

Oluşturulan tüm IP segmentlerinin Azure ve şirket içi parmak izinizdeki benzersiz olması gerektiğini unutmayın.  

**Örnek:** 10.0.4.0/24

:::image type="content" source="media/pre-deployment/nsx-segment-diagram.png" alt-text="Sanal makine iş yükleri için IP adresi kesimini tanımla" border="false":::     

## <a name="optional-extend-networks"></a>Seçim Ağları Genişlet

Ağ kesimlerini Şirket içinden Azure VMware çözümüne genişletebilirsiniz ve bunu yaparsanız bu ağları şimdi tanımlayabilirsiniz.  

Şunları göz önünde bulundurun:

- Ağları Şirket içinden genişletmeyi planlıyorsanız, bu ağlar şirket içi VMware ortamınızda bir [vSphere dağıtılmış anahtarına (vDS)](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.networking.doc/GUID-B15C6A13-797E-4BCB-B9D9-5CBC5A60C3A6.html) bağlanmalıdır.  
- Bir [vSphere standart anahtarında](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.networking.doc/GUID-350344DE-483A-42ED-B0E2-C811EE927D59.html)canlı genişletmek istediğiniz ağ (ler) i yoksa genişletilemez.

## <a name="expressroute-global-reach-peering-network"></a>ExpressRoute Global Reach eşleme ağı

`/29`ExpressRoute Global Reach eşlemesi için gereken bir CIDR ağ adresi bloğunu belirler. Oluşturulan herhangi bir IP kesiminin Azure VMware çözümünüz ve şirket içi parmak izi genelinde benzersiz olması gerektiğini unutmayın. Bu kesimdeki IP 'Ler, Azure VMware çözümü ExpressRoute bağlantı hattını şirket içi ExpressRoute devresiyle bağlamak üzere ExpressRoute Global Reach bağlantısının her ucunda kullanılır. 

**Örnek:** 10.1.0.0/29

:::image type="content" source="media/pre-deployment/expressroute-global-reach-ip-diagram.png" alt-text="Tanımla-ExpressRoute Global Reach eşleme ağı" border="false":::

## <a name="azure-virtual-network-to-attach-azure-vmware-solution"></a>Azure VMware çözümü eklemek için Azure sanal ağı

Azure VMware çözümünüz özel bulutunuza erişmek için Azure VMware çözümüyle birlikte sunulan ExpressRoute bağlantı hattının bir Azure sanal ağına eklenmesi gerekir.  Dağıtım sırasında yeni bir sanal ağ tanımlayabilir veya var olan bir ağı seçebilirsiniz.

Azure VMware çözümünün ExpressRoute devresi, bu adımda tanımladığınız Azure sanal ağındaki bir ExpressRoute ağ geçidine bağlanır.  

>[!IMPORTANT]
>Sanal ağ başına dört ExpressRoute bağlantı hattı sınırını aşmadığı sürece Azure VMware çözümüne bağlanmak için mevcut bir ExpressRoute ağ geçidini kullanabilirsiniz.  Bununla birlikte, ExpressRoute aracılığıyla şirket içi Azure VMware çözümüne erişmek için ExpressRoute Global Reach sahip olmanız gerekir. Bu, ExpressRoute ağ geçidi, bağlı olan devreler arasında geçişli yönlendirme sağlamamalıdır.  

ExpressRoute bağlantı hattını Azure VMware çözümünden mevcut bir ExpressRoute ağ geçidine bağlamak istiyorsanız dağıtımdan sonra bunu yapabilirsiniz.  

Bu nedenle, Özet bölümünde Azure VMware çözümünü mevcut bir Express Route ağ geçidine bağlamak istiyor musunuz?  

* **Evet** = dağıtım sırasında kullanılmayan sanal ağı tanımla.
* **Hayır** = var olan bir sanal ağı tanımla veya dağıtım sırasında yeni bir tane oluştur.

Her iki şekilde de, bu adımda ne yapmak istediğinizi belgeleyin.

>[!NOTE]
>Bu sanal ağ, şirket içi ortamınız ve Azure VMware çözümünüz tarafından görülür, bu nedenle bu sanal ağda kullandığınız IP segmentinin ve alt ağların çakışmadığından emin olun.

:::image type="content" source="media/pre-deployment/azure-vmware-solution-expressroute-diagram.png" alt-text="Kimlik-Azure VMware çözümü eklemek için Azure sanal ağı" border="false":::

## <a name="vmware-hcx-network-segments"></a>VMware HCX ağ kesimleri

VMware HCX, Azure VMware çözümü ile paketlenmiş bir teknolojidir. VMware HCX 'in birincil kullanım örnekleri iş yükü geçişleri ve olağanüstü durum kurtarmasından sorumludur. Bunlardan birini yapmak istiyorsanız, şimdi ağı planlamanız en iyisidir.   Aksi halde, bir sonraki adıma atlayabilir ve devam edebilirsiniz.

[!INCLUDE [hcx-network-segments](includes/hcx-network-segments.md)]

## <a name="next-steps"></a>Sonraki adımlar
Artık Toplandığınıza ve gerekli bilgileri belgeleolduğunuza göre, Azure VMware Çözüm özel bulutunuzu oluşturmak için sonraki bölüme devam edin.

> [!div class="nextstepaction"]
> [Azure VMware Çözümü dağıtma](deploy-azure-vmware-solution.md)
