---
title: Azure VMware çözümünü dağıtma ve yapılandırma
description: Azure VMware çözümü özel bulutu 'nı dağıtmak için planlama aşamasında toplanan bilgileri nasıl kullanacağınızı öğrenin.
ms.topic: tutorial
ms.date: 12/24/2020
ms.openlocfilehash: 4c6929ca59bae022642082e8382203a10bd41309
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100382064"
---
# <a name="deploy-and-configure-azure-vmware-solution"></a>Azure VMware çözümünü dağıtma ve yapılandırma

Bu makalede, Azure VMware çözümünü dağıtmak için [planlama bölümündeki](production-ready-deployment-steps.md) bilgileri kullanacaksınız. 

>[!IMPORTANT]
>Bilgileri henüz tanımlamadıysanız devam etmeden önce [Planlama bölümüne](production-ready-deployment-steps.md) geri dönün.

## <a name="register-the-resource-provider"></a>Kaynak sağlayıcısını kaydetme

[!INCLUDE [register-resource-provider-steps](includes/register-resource-provider-steps.md)]


## <a name="deploy-azure-vmware-solution"></a>Azure VMware Çözümü dağıtma

[Azure VMware Çözüm dağıtımı planlama](production-ready-deployment-steps.md) makalesinde topladığınız bilgileri kullanın:

>[!NOTE]
>Azure VMware çözümünü dağıtmak için abonelikte en az katkıda bulunan düzeyinde olmanız gerekir.

[!INCLUDE [create-avs-private-cloud-azure-portal](includes/create-private-cloud-azure-portal-steps.md)]

>[!NOTE]
>Bu adıma uçtan uca genel bakış için [Azure VMware çözümü: dağıtım](https://www.youtube.com/embed/gng7JjxgayI) videosunu görüntüleyin.

## <a name="create-the-jump-box"></a>Sıçrama kutusunu oluşturma

>[!IMPORTANT]
>**Özel bulut oluşturma** ekranında ilk sağlama adımı sırasında **sanal ağ** seçeneğini boş bıraktıysanız, bu bölüm Ile devam etmeden **önce** [VMware özel bulutunuz için ağ Yapılandır](tutorial-configure-networking.md) öğreticisini doldurun.  

Azure VMware çözümünü dağıttıktan sonra, sanal ağın vCenter ve NSX 'e bağlanan bir bağlantı kutusunu oluşturacaksınız. ExpressRoute devreleri ve ExpressRoute Global Reach yapılandırdıktan sonra, bağlantı kutusu gerekli değildir.  Ancak, Azure VMware çözümünüzde vCenter ve NSX 'e ulaşmak yararlı olur.  

:::image type="content" source="media/pre-deployment/jump-box-diagram.png" alt-text="Azure VMware çözümü geçiş kutusunu oluşturma" border="false" lightbox="media/pre-deployment/jump-box-diagram.png":::

[Dağıtım işleminin bir parçası olarak tanımladığınız veya oluşturduğunuz](production-ready-deployment-steps.md#attach-virtual-network-to-azure-vmware-solution)sanal ağda bir sanal makıne (VM) oluşturmak için aşağıdaki yönergeleri izleyin: 

[!INCLUDE [create-avs-jump-box-steps](includes/create-jump-box-steps.md)]

## <a name="connect-to-a-virtual-network-with-expressroute"></a>ExpressRoute ile sanal ağa bağlanma

>[!IMPORTANT]
>Azure 'da dağıtım ekranında zaten bir sanal ağ tanımladıysanız bir sonraki bölüme atlayın.

Dağıtım adımında bir sanal ağ tanımlamadıysanız ve amaç Azure VMware çözümünün ExpressRoute 'u mevcut bir ExpressRoute ağ geçidine bağlamak istiyorsanız, bu adımları izleyin.

[!INCLUDE [connect-expressroute-to-vnet](includes/connect-expressroute-vnet.md)]

## <a name="verify-network-routes-advertised"></a>Tanıtılan ağ yollarını doğrulama

Geçiş kutusu, Azure VMware çözümünün ExpressRoute bağlantı hattı üzerinden bağlandığı sanal ağ üzerindedir.  Azure 'da, atbox 'ın ağ arabirimine gidin ve [geçerli yolları görüntüleyin](../virtual-network/manage-route-table.md#view-effective-routes).

Etkin rotalar listesinde, Azure VMware Çözüm dağıtımının bir parçası olarak oluşturulan ağları görmeniz gerekir. Bu makalenin önceki bölümlerinde, [dağıtım adımı](#deploy-azure-vmware-solution) sırasında [ `/22` tanımladığınız ağdan](production-ready-deployment-steps.md#ip-address-segment) türetilmiş birden çok ağ görürsünüz.

:::image type="content" source="media/pre-deployment/azure-vmware-solution-effective-routes.png" alt-text="Azure VMware çözümünden Azure Virtual Network 'e tanıtılan ağ yollarını doğrulama" lightbox="media/pre-deployment/azure-vmware-solution-effective-routes.png":::

Bu örnekte, 10.74.72.0/22 ağı dağıtım sırasında giriştir/24 ağı türetir.  Benzer bir durum görürseniz, Azure VMware çözümünde vCenter 'a bağlanabilirsiniz.

## <a name="connect-and-sign-in-to-vcenter-and-nsx-t"></a>VCenter ve NSX-T ' e bağlanma ve oturum açma

Önceki adımda oluşturduğunuz sıçrama kutusunda oturum açın. Oturum açtıktan sonra bir Web tarayıcısı açın ve git ve NSX-T Yöneticisi ' ne gidin ve oturum açın.  

VCenter ve NSX-T Manager konsolunun IP adreslerini ve kimlik bilgilerini Azure portal tanımlayabilirsiniz.  Özel bulutunuzu seçip **genel bakış** görünümünde **kimlik > varsayılan**' ı seçin. 

## <a name="create-a-network-segment-on-azure-vmware-solution"></a>Azure VMware çözümünde bir ağ kesimi oluşturma

Azure VMware Çözüm ortamınızda yeni ağ kesimleri oluşturmak için NSX-T Yöneticisi 'Ni kullanırsınız.  Oluşturmak istediğiniz ağları [planlama bölümünde](production-ready-deployment-steps.md)tanımladınız.  Bunları tanımlamadıysanız devam etmeden önce [Planlama bölümüne](production-ready-deployment-steps.md) geri dönün.

>[!IMPORTANT]
>Tanımladığınız CıDR ağ adresi bloğunun Azure veya şirket içi ortamlarınızdaki herhangi bir şeyle çakışmadığından emin olun.  

Azure VMware çözümünde bir NSX-T ağ kesimi oluşturmak için [Azure VMware Çözüm öğreticisinde NSX-t ağ kesimi oluşturma](tutorial-nsx-t-network-segment.md) öğreticisini izleyin.

## <a name="verify-advertised-nsx-t-segment"></a>Tanıtılan NSX-T segmentini doğrula

[Tanıtılan ağ yollarını doğrula](#verify-network-routes-advertised) adımına geri dönün. Önceki adımda oluşturduğunuz ağ kesimlerini temsil eden listede diğer yollar görürsünüz.  

Sanal makineler için, [Azure VMware çözüm adımında ağ kesimi oluşturma](#create-a-network-segment-on-azure-vmware-solution) bölümünde oluşturduğunuz segmentleri atayacaksınız.  

DNS gerekli olduğundan, hangi DNS sunucusunu kullanmak istediğinizi belirlemek.  

- ExpressRoute Global Reach yapılandırdıysanız, şirket içinde kullandığınız DNS sunucusunu kullanın.  
- Azure 'da bir DNS sunucunuz varsa, bunu kullanın.  
- Bunlardan birine sahip değilseniz, atlamanız gereken DNS sunucusunu kullanın.

>[!NOTE]
>Bu adım, DNS sunucusunu belirlemektir ve bu adımda hiçbir yapılandırma yapılmaz.

## <a name="optional-provide-dhcp-services-to-nsx-t-network-segment"></a>Seçim NSX-T ağ kesimine DHCP hizmetleri sağlama

NSX-T segmentlerinizi DHCP kullanmayı planlıyorsanız, bu bölüm ile devam edin. Aksi takdirde, [NSX-T ağ KESIMINE VM ekleme](#add-a-vm-on-the-nsx-t-network-segment) bölümüne atlayın.  

NSX-T ağ segmentinizi oluşturduğunuza göre, Azure VMware çözümünde DHCP 'yi iki şekilde oluşturabilir ve yönetebilirsiniz:

* DHCP sunucunuzu barındırmak için NSX-T kullanıyorsanız, [BIR DHCP sunucusu oluşturmanız](manage-dhcp.md#create-a-dhcp-server) ve [Bu sunucuya geçiş](manage-dhcp.md#create-dhcp-relay-service)yapmanız gerekir. 
* Ağınızda üçüncü taraf bir dış DHCP sunucusu kullanıyorsanız, [DHCP geçiş hizmeti oluşturmanız](manage-dhcp.md#create-dhcp-relay-service)gerekir.  Bu seçenek için [yalnızca geçiş yapılandırmasını yapın](manage-dhcp.md#create-dhcp-relay-service).


## <a name="add-a-vm-on-the-nsx-t-network-segment"></a>NSX-T ağ kesimine VM ekleme

Azure VMware Çözüm vCenter 'larınızda bir VM dağıtın ve Azure VMware Çözüm ağlarınızdan bağlantıyı doğrulamak için bunu kullanın:

- Internet
- Azure Sanal Ağları
- Şirket içi.  

VM 'yi herhangi bir vSphere ortamında olduğu gibi dağıtın.  VM 'yi, NSX-T içinde daha önce oluşturduğunuz ağ kesimlerinden birine ekleyin.  

>[!NOTE]
>Bir DHCP sunucusu ayarlarsanız, sanal makine için ağ yapılandırmanızı alırsınız (kapsamı ayarlamayı unutmayın).  Statik olarak yapılandıracaksanız, normalde yaptığınız gibi yapılandırın.

## <a name="test-the-nsx-t-segment-connectivity"></a>NSX-T segmentinin bağlantısını test etme

Önceki adımda oluşturulan VM 'de oturum açın ve bağlantıyı doğrulayın;

1. İnternet 'teki bir IP 'ye ping gönderin.
2. Bir Web tarayıcısında, bir internet sitesine gidin.
3. Azure sanal ağında bulunan atlama kutusuna ping gönderin.

Azure VMware çözümü artık çalışır durumdadır ve Azure sanal ağı ve internet 'ten başarıyla bağlantı kurdunuz.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki bölümde, ExpressRoute aracılığıyla Azure VMware çözümünü şirket içi ağınıza bağlayacaksınız.
> [!div class="nextstepaction"]
> [Azure VMware çözümünü şirket içi ortamınıza bağlama](azure-vmware-solution-on-premises.md)
