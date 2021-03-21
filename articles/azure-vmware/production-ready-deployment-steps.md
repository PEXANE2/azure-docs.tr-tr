---
title: Azure VMware çözüm dağıtımını planlama
description: Bu makalede bir Azure VMware Çözüm dağıtımı iş akışı özetlenmektedir.  Nihai sonuç, sanal makine (VM) oluşturma ve geçirme için hazırlanma ortamıdır.
ms.topic: tutorial
ms.date: 03/17/2021
ms.openlocfilehash: 2ded5d706ab71b3880633cd324fb366d0a1bccbe
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104584644"
---
# <a name="planning-the-azure-vmware-solution-deployment"></a>Azure VMware çözüm dağıtımını planlama

Bu makalede, dağıtım sırasında kullanacağınız bilgileri tanımlamak ve toplamak için planlama işlemi sağlanmıştır. Dağıtımınızı planlarken, dağıtım sırasında kolay başvuru için topladığınız bilgileri belgelediğinizden emin olun.

Bu hızlı başlangıç bölümünde özetlenen adımlar, sanal makineler (VM 'Ler) ve geçiş için üretim için hazırlanmış bir ortam sağlar. 

>[!IMPORTANT]
>Azure VMware Çözüm kaynağınızı oluşturmadan önce, konaklarınızın ayrılmaları için bir destek bileti göndermek üzere [Azure VMware Çözüm kaynağını etkinleştirme](enable-azure-vmware-solution.md) makalesini izleyin. Destek ekibi isteğinizi aldıktan sonra, isteğinizi doğrulamak ve konaklarınızı ayırmak için beş iş günü sürer. Mevcut bir Azure VMware çözümü özel bulutunuz varsa ve daha fazla ana bilgisayar ayırmak istiyorsanız, aynı işlemden geçmeniz gerekir. 

## <a name="subscription"></a>Abonelik

Azure VMware çözümünü dağıtmak için kullanmayı planladığınız aboneliği belirler.  Yeni bir abonelik oluşturabilir ya da var olan bir aboneliği yeniden kullanabilirsiniz.

>[!NOTE]
>Aboneliğin bir Microsoft Kurumsal Anlaşma veya bir bulut çözümü sağlayıcısı Azure planıyla ilişkilendirilmesi gerekir. Daha fazla bilgi için bkz. [Azure VMware Çözüm kaynağını etkinleştirme](enable-azure-vmware-solution.md).

## <a name="resource-group"></a>Kaynak grubu

Azure VMware çözümünüz için kullanmak istediğiniz kaynak grubunu belirler.  Genellikle, Azure VMware çözümü için bir kaynak grubu oluşturulur, ancak var olan bir kaynak grubunu kullanabilirsiniz.

## <a name="region"></a>Region

Azure VMware çözümünün dağıtılmasını istediğiniz bölgeyi belirler.  Daha fazla bilgi için bkz. [bölgeye göre kullanılabilir Azure ürünleri kılavuzu](https://azure.microsoft.com/en-us/global-infrastructure/services/?products=azure-vmware).

## <a name="resource-name"></a>Kaynak adı

Dağıtım sırasında kullanacağınız kaynak adını tanımlayın.  Kaynak adı, Azure VMware çözümünüz özel bulutunuzu başlık olarak kullanabileceğiniz kolay ve açıklayıcı bir addır.

>[!IMPORTANT]
>Ad 40 karakterden uzun olmamalıdır. Ad bu sınırı aşarsa, özel bulutla kullanılmak üzere genel IP adresleri oluşturamazsınız. 

## <a name="size-hosts"></a>Ana bilgisayarları Boyutlandır

Azure VMware çözümünü dağıttığınızda kullanmak istediğiniz boyut konaklarınızı belirler.  Tüm liste için bkz. [Azure VMware çözümü özel bulutlar ve kümeler](concepts-private-clouds-clusters.md#hosts) belgeleri.

## <a name="number-of-clusters-and-hosts"></a>Küme ve ana bilgisayar sayısı

Yaptığınız ilk Azure VMware Çözüm dağıtımı, tek bir küme içeren özel bir buluttan oluşur. Dağıtımınız için, ilk kümeye dağıtmak istediğiniz ana bilgisayar sayısını tanımlamanız gerekir.

>[!NOTE]
>Küme başına en düşük ana bilgisayar sayısı üç, üst sınır 16 ' dır. Özel bulut başına en fazla küme sayısı dördü. 

Daha fazla bilgi için bkz. [Azure VMware çözümü özel bulut ve kümeler](concepts-private-clouds-clusters.md#clusters) belgeleri.

>[!TIP]
>İlk dağıtım numarasının ötesine gitmeniz gerekiyorsa, her zaman kümeyi genişletebilir ve daha sonra ek kümeler ekleyebilirsiniz.

## <a name="ip-address-segment-for-private-cloud-management"></a>Özel bulut yönetimi için IP adresi segmenti

Dağıtımı planlamada ilk adım, IP segmentlemesini planlıyor. Azure VMware çözümü bir/22 CıDR ağı gerektirir. Bu adres alanı, daha küçük ağ kesimlerine (alt ağlar) ve vCenter, VMware HCX, NSX-T ve vMotion işlevselliği dahil olmak üzere Azure VMware Çözüm Yönetimi kesimleri için kullanılır. Aşağıdaki görselleştirme bu segmentin kullanılacağı vurgulanmıştır.

Bu/22 CıDR ağ adresi bloğunun, şirket içinde veya Azure 'da zaten mevcut olan herhangi bir ağ kesimiyle çakışmamalıdır.

**Örnek:** 10.0.0.0/22

/22 CıDR ağının özel bulut [ağı planlama denetim listesi](tutorial-network-checklist.md#routing-and-subnet-considerations)başına nasıl bölündüğü hakkında ayrıntılı bir döküm için.

:::image type="content" source="media/pre-deployment/management-vmotion-vsan-network-ip-diagram.png" alt-text="IP adresi kesimini tanımla" border="false":::  

## <a name="ip-address-segment-for-virtual-machine-workloads"></a>Sanal makine iş yükleri için IP adresi segmenti

Her VMware ortamıyla de olduğu gibi, sanal makinelerin bir ağ kesimine bağlanması gerekir. Azure VMware çözümünde, iki tür segment vardır, L2 genişletilmiş kesimleri (daha sonra ele alınmıştır) ve NSX-T ağ kesimleri. Azure VMware çözümünün üretim dağıtımı genişledikçe, genellikle şirket içi ve yerel NSX-T ağ kesimlerinden L2 genişletilmiş segmentlerinin bir birleşimi vardır. İlk dağıtımı planlamak için Azure VMware çözümünde, tek bir ağ segmenti (IP ağı) yapın. Bu ağ, Şirket içindeki veya Azure 'daki herhangi bir ağ kesimiyle çakışmamalıdır ve daha önce tanımlanan/22 ağ kesiminde yer almalıdır.

Bu ağ kesimi öncelikle ilk dağıtım sırasında test amacıyla kullanılır.

>[!NOTE]
>Dağıtım sırasında bu ağ veya ağlara gerek duyulmayacak. Dağıtım sonrası bir adım olarak oluşturulur.
  
**Örnek:** 10.0.4.0/24

:::image type="content" source="media/pre-deployment/nsx-segment-diagram.png" alt-text="Sanal makine iş yükleri için IP adresi kesimini tanımla" border="false":::     

## <a name="optional-extend-your-networks"></a>Seçim Ağlarınızı genişletin

Ağ kesimlerini Şirket içinden Azure VMware çözümüne genişletebilirsiniz ve bunu yaparsanız bu ağları şimdi tanımlayabilirsiniz.  

Şunları göz önünde bulundurun:

- Ağları Şirket içinden genişletmeyi planlıyorsanız, bu ağlar şirket içi VMware ortamınızda bir [vSphere dağıtılmış anahtarına (vDS)](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.networking.doc/GUID-B15C6A13-797E-4BCB-B9D9-5CBC5A60C3A6.html) bağlanmalıdır.  
- Bir [vSphere standart anahtarında](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.networking.doc/GUID-350344DE-483A-42ED-B0E2-C811EE927D59.html)canlı genişletmek istediğiniz ağ (ler) i yoksa genişletilemez.

>[!NOTE]
>Bu ağlar, dağıtım sırasında değil, yapılandırmanın son adımı olarak genişletilir.

## <a name="attach-azure-virtual-network-to-azure-vmware-solution"></a>Azure sanal ağını Azure VMware çözümüne ekleyin

Azure VMware çözümüne bağlantı sağlamak için bir ExpressRoute, Azure VMware Çözüm özel bulutu 'ndan bir ExpressRoute sanal ağ geçidine oluşturulmuştur.

*Mevcut* veya *Yeni* bir ExpressRoute sanal ağ geçidi kullanabilirsiniz.

:::image type="content" source="media/pre-deployment/azure-vmware-solution-expressroute-diagram.png" alt-text="Kimlik-Azure VMware çözümü eklemek için Azure sanal ağı" border="false":::

### <a name="use-an-existing-expressroute-virtual-network-gateway"></a>Mevcut bir ExpressRoute sanal ağ geçidini kullanma

*Mevcut* bir ExpressRoute sanal ağ geçidi kullanmayı planlıyorsanız, Azure VMware çözümü ExpressRoute bağlantı hattı dağıtım sonrası bir adım olarak oluşturulur. Bu durumda, **sanal ağ** alanını boş bırakın.

Genel bir öneri olarak, mevcut bir ExpressRoute sanal ağ geçidini kullanmak kabul edilebilir. Planlama amacıyla, hangi ExpressRoute sanal ağ geçidini kullanacağınızı ve ardından bir sonraki adımla devam edin.

### <a name="create-a-new-expressroute-virtual-network-gateway"></a>Yeni bir ExpressRoute sanal ağ geçidi oluştur

*Yeni* bir ExpressRoute sanal ağ geçidi oluşturduğunuzda, var olan bir Azure sanal ağını kullanabilir veya yeni bir tane oluşturabilirsiniz.  

- Mevcut bir Azure sanal ağı için:
   1. Önceden var olan ExpressRoute sanal ağ geçitlerinin olmadığı bir Azure sanal ağını belirler.
   2. Dağıtımdan önce Azure sanal ağında bir [Gatewaysubnet](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md#create-the-gateway-subnet) oluşturun.

- Yeni bir Azure sanal ağı için daha önce veya dağıtım sırasında oluşturabilirsiniz. **Sanal ağ** listesi altında **Yeni oluştur** bağlantısını seçin.

Aşağıdaki görüntüde, **sanal ağ** alanı vurgulanmış şekilde **özel bir bulut dağıtımı oluşturma** ekranı gösterilmektedir.

:::image type="content" source="media/pre-deployment/azure-vmware-solution-deployment-screen-vnet-circle.png" alt-text="Sanal ağ alanı vurgulanmış şekilde Azure VMware Çözüm dağıtımı ekranının ekran görüntüsü.":::

>[!NOTE]
>Kullanılacak veya oluşturulacak sanal ağ, şirket içi ortamınız ve Azure VMware çözümünüz tarafından görülebilir, bu nedenle bu sanal ağda kullandığınız IP segmentinin ve alt ağların çakışmadığından emin olun.

## <a name="vmware-hcx-network-segments"></a>VMware HCX ağ kesimleri

VMware HCX, Azure VMware çözümü ile paketlenmiş bir teknolojidir. VMware HCX 'in birincil kullanım örnekleri iş yükü geçişleri ve olağanüstü durum kurtarmasından sorumludur. Bunlardan birini yapmak istiyorsanız, şimdi ağı planlamanız en iyisidir.   Aksi halde, bir sonraki adıma atlayabilir ve devam edebilirsiniz.

[!INCLUDE [hcx-network-segments](includes/hcx-network-segments.md)]

## <a name="next-steps"></a>Sonraki adımlar
Artık Toplandığınıza ve gerekli bilgileri belgeleolduğunuza göre, Azure VMware Çözüm özel bulutunuzu oluşturmak için sonraki bölüme devam edin.

> [!div class="nextstepaction"]
> [Azure VMware Çözümü dağıtma](deploy-azure-vmware-solution.md)
