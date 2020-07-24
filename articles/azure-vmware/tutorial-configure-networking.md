---
title: Öğretici-Azure 'da VMware özel bulutunuz için ağı yapılandırma
description: Özel bulutunuzu Azure 'da dağıtmak için gereken ağı oluşturmayı ve yapılandırmayı öğrenin
ms.topic: tutorial
ms.date: 07/22/2020
ms.openlocfilehash: aa4247f60c3e1ec54bfcde336d1ae8c8f70ff7a8
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87079440"
---
# <a name="tutorial-configure-networking-for-your-vmware-private-cloud-in-azure"></a>Öğretici: Azure 'da VMware özel bulutunuz için ağı yapılandırma

Bir Azure VMware çözümü (AVS) özel bulutu, bir Azure sanal ağı gerektirir. AVS, önizleme sırasında şirket içi vCenter 'ı desteklemediğinden, şirket içi ortamınıza tümleştirme için ek adımlar gereklidir. ExpressRoute bağlantı hattı ve bir sanal ağ geçidi kurmak da gereklidir ve bu öğreticide ele alınmıştır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Sanal ağ oluşturma
> * Sanal ağ geçidi oluşturma
> * ExpressRoute devrenizi ağ geçidine bağlama
> * VCenter ve NSX Manager için URL 'Leri bulma

## <a name="prerequisites"></a>Önkoşullar 
Bir sanal ağ oluşturabilmeniz için önce bir [AVS özel bulutu](tutorial-create-private-cloud.md)oluşturmuş olduğunuzdan emin olun. 

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. [Özel bulut oluşturma öğreticisinde](tutorial-create-private-cloud.md) oluşturduğunuz kaynak grubuna gidin ve yeni bir kaynak tanımlamak Için **+ Ekle** ' yi seçin. 

1. Market 'te **Ara** metin kutusuna **sanal ağ**yazın. Sanal ağ kaynağını bulup seçin.

1. **Sanal ağ sayfasında,** özel bulutunuz için Sanal ağınızı kurmak üzere **Oluştur** ' u seçin.

1. **Sanal ağ oluştur** sayfasında, sanal ağınızın ayrıntılarını girin.

1. **Temel bilgiler** sekmesinde, sanal ağ için bir ad girin ve uygun bölgeyi seçip **İleri ' yı seçin: IP adresleri**.

1. **IP adresleri** sekmesinde, **IPv4 adres alanı**altında, önceki öğreticide oluşturduğunuz adres alanını girin.

   > [!IMPORTANT]
   > Önceki öğreticide özel bulutunuzu oluştururken kullandığınız adres alanı **ile örtüşmeyen bir** adres alanı kullanmanız gerekir.

1. **+ Alt ağ**Ekle ' yi seçin ve **alt ağ ekle** sayfasında, alt ağa bir ad ve uygun adres aralığı verin. Bittiğinde **Ekle**’yi seçin.

1. **Gözden geçir ve oluştur**’u seçin.

   :::image type="content" source="./media/tutorial-configure-networking/create-virtual-network.png" alt-text="sanal ağ oluşturma" border="true":::

1. Bilgileri doğrulayıp **Oluştur**' u seçin. Dağıtım tamamlandıktan sonra, Sanal ağınızı kaynak grubunda görürsünüz.

## <a name="create-a-virtual-network-gateway"></a>Sanal ağ geçidi oluşturma

Artık bir sanal ağ oluşturduğunuza göre, bir sanal ağ geçidi oluşturacaksınız.

1. Kaynak grubunuzda, yeni bir kaynak eklemek için **+ Ekle** ' yi seçin.

1. **Market** metin kutusunda, **sanal ağ geçidi**yazın. Sanal ağ kaynağını bulup seçin.

1. **Sanal ağ geçidi** sayfasında **Oluştur**' u seçin.

1. **Sanal ağ geçidi oluştur** sayfasının temel bilgiler sekmesinde, alanlar için değerler girin ve ardından **gözden geçir + oluştur**' u seçin. 

   | Alan | Değer |
   | --- | --- |
   | **Abonelik** | Bu değer, kaynak grubunun ait olduğu abonelikle zaten doldurulmuştur. |
   | **Kaynak grubu** | Bu değer, geçerli kaynak grubu için zaten doldurulmuştur. Bu, önceki bir testte oluşturduğunuz kaynak grubu olmalıdır. |
   | **Ad** | Sanal ağ geçidi için benzersiz bir ad girin. |
   | **Bölge** | Sanal ağ geçidinin coğrafi konumunu seçin. |
   | **Ağ geçidi türü** | **ExpressRoute**' ı seçin. |
   | **SKU** | Varsayılan değeri bırakın: **Standart**. |
   | **Sanal ağ** | Daha önce oluşturduğunuz sanal ağı seçin. Sanal ağı görmüyorsanız, ağ geçidinin bölgesinin sanal ağınızın bölgesiyle eşleştiğinden emin olun. |
   | **Ağ geçidi alt ağ adres aralığı** | Bu değer, sanal ağı seçtiğinizde doldurulur. Varsayılan değeri değiştirmeyin. |
   | **Genel IP adresi** | **Yeni oluştur**’u seçin. |

   :::image type="content" source="./media/tutorial-configure-networking/create-virtual-network-gateway.png" alt-text="ağ geçidi oluşturma" border="true":::

1. Ayrıntıların doğru olduğundan emin olun ve sanal ağ geçidinizin dağıtımını başlatmak için **Oluştur** ' u seçin. 
1. Dağıtım tamamlandıktan sonra, ExpressRoute bağlantınızı AVS özel bulutunuzu içeren sanal ağ geçidine bağlamak için sonraki bölüme geçin.

## <a name="connect-expressroute-to-the-virtual-network-gateway"></a>ExpressRoute 'ı sanal ağ geçidine bağlama

Artık bir sanal ağ geçidi dağıttığınıza göre, BT ve AVS özel bulutunuz arasında bir bağlantı ekleyeceksiniz.

1. Önceki öğreticide oluşturduğunuz özel buluta gidin ve **Yönet**' ın altındaki **bağlantı** ' yı seçin, **ExpressRoute** sekmesini seçin.

1. Yetkilendirme anahtarını kopyalayın. Bir yetkilendirme anahtarı yoksa, bunu yapmanız gerekir ve bunu yapmak için bir **Yetkilendirme anahtarı iste** seçeneğini belirleyin

   :::image type="content" source="./media/tutorial-configure-networking/request-auth-key.png" alt-text="Yetkilendirme anahtarı isteme" border="true":::

1. Önceki adımda oluşturduğunuz sanal ağ geçidine gidin ve **Ayarlar**altında **Bağlantılar**' ı seçin. **Bağlantılar** sayfasında **+ Ekle**' yi seçin.

1. **Bağlantı ekle** sayfasında, alanlar için değerler sağlayın ve **Tamam**' ı seçin. 

   | Alan | Değer |
   | --- | --- |
   | **Ad**  | Bağlantı için bir ad girin.  |
   | **Bağlantı türü**  | **ExpressRoute**' ı seçin.  |
   | **Yetkilendirmeyi kullanma**  | Bu kutunun seçili olduğundan emin olun.  |
   | **Sanal ağ geçidi** | Daha önce oluşturduğunuz sanal ağ geçidi.  |
   | **Yetkilendirme anahtarı**  | Yetkilendirme anahtarını kopyalayın ve kaynak grubunuzun ExpressRoute sekmesinden yapıştırın. |
   | **Eş devre URI 'SI**  | ExpressRoute KIMLIĞINI kopyalayın ve kaynak grubunuzun ExpressRoute sekmesinden yapıştırın.  |

   :::image type="content" source="./media/tutorial-configure-networking/add-connection.png" alt-text="bağlantı ekleme" border="true":::

ExpressRoute bağlantı hattı ve sanal ağınız arasındaki bağlantı oluşturulur.



## <a name="locate-the-urls-for-vcenter-and-nsx-manager"></a>VCenter ve NSX Manager için URL 'Leri bulma

VCenter ve NSX Manager 'da oturum açmak için vCenter web istemcisinin ve NSX-T Manager sitesinin URL 'Lerine ihtiyacınız vardır. 

AVS özel bulutuna gidin, **Yönet**bölümünde **kimlik**' i seçin, burada gereken bilgileri bulabilirsiniz.

:::image type="content" source="./media/tutorial-configure-networking/locate-urls.png" alt-text="vCenter URL 'lerini bulma" border="true":::

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Sanal ağ oluşturma
> * Sanal ağ geçidi oluşturma
> * ExpressRoute devrenizi ağ geçidine bağlama
> * VCenter ve NSX Manager için URL 'Leri bulma

Özel bulutunuzu yerel olarak yönetebilmeniz için ortamınıza bağlanmak üzere kullanılan bir bağlantı kutusu oluşturmayı öğrenmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Özel Buluta Erişme](tutorial-access-private-cloud.md)
