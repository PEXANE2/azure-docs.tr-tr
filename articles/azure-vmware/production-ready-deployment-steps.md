---
title: Azure VMware çözüm dağıtımını planlama
description: Bu makalede bir Azure VMware Çözüm dağıtımı iş akışı özetlenmektedir.  Nihai sonuç, sanal makine (VM) oluşturma ve geçirme için hazırlanma ortamıdır.
ms.topic: tutorial
ms.date: 10/16/2020
ms.openlocfilehash: cdf4ddd6166920fa7461bfd85e01ef0efd6dfbb9
ms.sourcegitcommit: 75041f1bce98b1d20cd93945a7b3bd875e6999d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98704579"
---
# <a name="planning-the-azure-vmware-solution-deployment"></a>Azure VMware çözüm dağıtımını planlama

Bu makalede, dağıtım sırasında kullanılan verileri belirleyip toplayacağınız planlama süreci sunulmaktadır. Dağıtımınızı planlarken, dağıtım sırasında kolay başvuru için topladığınız bilgileri belgelediğinizden emin olun.

Bu hızlı başlangıç işlemlerine, sanal makineler (VM 'Ler) ve geçiş için üretime hazırlı bir ortam ile sonuçlanır. 

>[!IMPORTANT]
>Azure VMware Çözüm kaynağınızı oluşturmadan önce, konaklarınızın ayrılmaları için bir destek bileti göndermek üzere [Azure VMware Çözüm kaynağını etkinleştirme](enable-azure-vmware-solution.md) makalesini izleyin. Destek ekibi isteğinizi aldıktan sonra, isteğinizi doğrulamak ve konaklarınızı ayırmak için beş iş günü sürer. Mevcut bir Azure VMware çözümü özel bulutunuz varsa ve daha fazla ana bilgisayar ayırmak istiyorsanız, aynı işlemden geçmeniz gerekir. 


## <a name="subscription"></a>Abonelik

Azure VMware çözümünü dağıtmak için kullanmayı planladığınız aboneliği belirler.  Yeni bir abonelik oluşturabilir ya da var olan bir aboneliği yeniden kullanabilirsiniz.

>[!NOTE]
>Aboneliğin bir Microsoft Kurumsal Anlaşma veya bir bulut çözümü sağlayıcısı Azure planıyla ilişkilendirilmesi gerekir. Daha fazla bilgi için bkz. [Azure VMware Çözüm kaynağını etkinleştirme](enable-azure-vmware-solution.md).

## <a name="resource-group"></a>Kaynak grubu

Azure VMware çözümünüz için kullanmak istediğiniz kaynak grubunu belirler.  Genellikle, Azure VMware çözümü için bir kaynak grubu oluşturulur, ancak var olan bir kaynak grubunu kullanabilirsiniz.

## <a name="region"></a>Bölge

Azure VMware çözümünün dağıtılmasını istediğiniz bölgeyi belirler.  Daha fazla bilgi için bkz. [bölgeye göre kullanılabilir Azure ürünleri kılavuzu](https://azure.microsoft.com/en-us/global-infrastructure/services/?products=azure-vmware).

## <a name="resource-name"></a>Kaynak adı

Dağıtım sırasında kullanacağınız kaynak adını tanımlayın.  Kaynak adı, Azure VMware çözümünüz özel bulutunuzu başlık olarak kullanabileceğiniz kolay ve açıklayıcı bir addır.

>[!IMPORTANT]
>Ad 40 karakterden uzun olmamalıdır. Ad bu sınırı aşarsa, özel bulutla kullanılmak üzere genel IP adresleri oluşturamazsınız. 

## <a name="size-hosts"></a>Ana bilgisayarları Boyutlandır

Azure VMware çözümünü dağıttığınızda kullanmak istediğiniz boyut konaklarınızı belirler.  Tüm liste için bkz. [Azure VMware çözümü özel bulutlar ve kümeler](concepts-private-clouds-clusters.md#hosts) belgeleri.

## <a name="number-of-hosts"></a>Ana bilgisayar sayısı

Azure VMware Çözüm özel bulutuna dağıtmak istediğiniz ana bilgisayar sayısını tanımlayın.  En az sayıda ana bilgisayar vardır ve en yüksek değer, küme başına 16 ' dır.  Daha fazla bilgi için bkz. [Azure VMware çözümü özel bulut ve kümeler](concepts-private-clouds-clusters.md#clusters) belgeleri.

İlk dağıtım numarasının ötesine gitmeniz gerekiyorsa, her zaman kümeyi genişletebilirsiniz.

## <a name="vcenter-admin-password"></a>vCenter yönetici parolası
VCenter yönetici parolasını tanımlayın.  Dağıtım sırasında bir vCenter yönetici parolası oluşturacaksınız. Parola, cloudadmin@vsphere.local vCenter derlemesi sırasında yönetici hesabıdır. VCenter 'da oturum açmak için kullanacaksınız.

## <a name="nsx-t-admin-password"></a>NSX-T yönetici parolası
NSX-T yönetici parolasını tanımlayın.  Dağıtım sırasında bir NSX-T yönetici parolası oluşturacaksınız. Parola NSX derlemesi sırasında NSX hesabındaki yönetici kullanıcısına atanır. NSX-T Manager 'da oturum açmak için kullanacaksınız.

## <a name="ip-address-segment"></a>IP adresi segmenti

Dağıtımı planlamada ilk adım, IP segmentlemesini planlıyor.  Azure VMware çözümü sağladığınız a/22 ağı. Daha sonra bu IP segmentlerini daha küçük parçalara ayırır ve vCenter, VMware HCX, NSX-T ve vMotion için bu IP segmentlerini kullanır.

Azure VMware çözümü, bir iç ExpressRoute devresi üzerinden Microsoft Azure Sanal Ağ bağlanır. Çoğu durumda, ExpressRoute Global Reach aracılığıyla veri merkezinize bağlanır. 

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

## <a name="azure-virtual-network-to-attach-azure-vmware-solution"></a>Azure VMware çözümü eklemek için Azure sanal ağı

Bu adımda, bir ExpressRoute sanal ağ geçidi tanımlayabilir ve Azure VMware Solution ExpressRoute devresine bağlanmak için kullanılan Azure sanal ağını destektireceksiniz.  ExpressRoute bağlantı hattı, Azure VMware çözümü özel bulutuna ve diğer Azure hizmetlerine, Azure kaynaklarına ve şirket içi ortamlara yönelik bağlantıyı kolaylaştırır.

*Mevcut* veya *Yeni* bir ExpressRoute sanal ağ geçidi kullanabilirsiniz.

:::image type="content" source="media/pre-deployment/azure-vmware-solution-expressroute-diagram.png" alt-text="Kimlik-Azure VMware çözümü eklemek için Azure sanal ağı" border="false":::

### <a name="use-an-existing-expressroute-virtual-network-gateway"></a>Mevcut bir ExpressRoute sanal ağ geçidini kullanma

*Mevcut* bir ExpressRoute sanal ağ geçidi kullanırsanız, özel bulut dağıttıktan sonra Azure VMware çözümü ExpressRoute bağlantı hattı oluşturulur.  Bu nedenle, **sanal ağ** alanını doldurmanız gerekmez.  

Hangi ExpressRoute sanal ağ geçidini kullanacağınızı ve sonraki adıma devam etmeyi unutmayın.

### <a name="create-a-new-expressroute-virtual-network-gateway"></a>Yeni bir ExpressRoute sanal ağ geçidi oluştur

*Yeni* bir ExpressRoute sanal ağ geçidi oluşturuyorsanız, var olan bir Azure sanal ağı kullanılabilir veya yeni bir Azure sanal ağı oluşturulabilir.  

Seçim var olan bir Azure sanal ağını kullanacaksanız, sanal ağda önceden var olan ExpressRoute sanal ağ geçitleri olmadığından emin olun ve özel bulut dağıtımı oluştur ekranının sanal ağ açılan menüsünde seçin.

Seçim, yeni bir Azure sanal ağı oluşturmak için özel bulut dağıtımı oluşturma ekranının sanal ağ ' ın yeni seçeneğine tıklanarak bir süre önce veya dağıtım sırasında oluşturulabilir.

Aşağıdaki başvuru için, **özel bulut dağıtımı oluşturma** ekranının bir görüntüsü aşağıda verilmiştir ve kırmızı renkle Seviyelendirilmiş, bu bölümün tamamında başvurulan Azure **sanal ağ** alanıdır.

:::image type="content" source="media/pre-deployment/azure-vmware-solution-deployment-screen-vnet-circle.png" alt-text="Sanal ağ geçidi daire içinde Azure VMware Çözüm dağıtımı ekranının ekran görüntüsü.":::

>[!NOTE]
>Kullanılacak veya oluşturulacak sanal ağ, şirket içi ortamınız ve Azure VMware çözümünüz tarafından görülebilir, bu nedenle bu sanal ağda kullandığınız IP segmentinin ve alt ağların çakışmadığından emin olun.

## <a name="vmware-hcx-network-segments"></a>VMware HCX ağ kesimleri

VMware HCX, Azure VMware çözümü ile paketlenmiş bir teknolojidir. VMware HCX 'in birincil kullanım örnekleri iş yükü geçişleri ve olağanüstü durum kurtarmasından sorumludur. Bunlardan birini yapmak istiyorsanız, şimdi ağı planlamanız en iyisidir.   Aksi halde, bir sonraki adıma atlayabilir ve devam edebilirsiniz.

[!INCLUDE [hcx-network-segments](includes/hcx-network-segments.md)]

## <a name="next-steps"></a>Sonraki adımlar
Artık Toplandığınıza ve gerekli bilgileri belgeleolduğunuza göre, Azure VMware Çözüm özel bulutunuzu oluşturmak için sonraki bölüme devam edin.

> [!div class="nextstepaction"]
> [Azure VMware Çözümü dağıtma](deploy-azure-vmware-solution.md)
