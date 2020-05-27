---
title: Öğretici-Azure 'da VMware özel bulutunuz için ağı yapılandırma
description: Özel bulutunuzu Azure 'da dağıtmak için gereken ağı oluşturmayı ve yapılandırmayı öğrenin
ms.topic: tutorial
ms.date: 05/04/2020
ms.openlocfilehash: 5a8086f78f465f52d0f9107932c09c4690f505e8
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83873860"
---
# <a name="tutorial-configure-networking-for-your-vmware-private-cloud-in-azure"></a>Öğretici: Azure 'da VMWare özel bulutunuz için ağı yapılandırma

Bir Azure VMware çözümü (AVS) özel bulutu bir sanal ağ gerektirir. AVS, önizleme sırasında şirket içi vCenter 'ı desteklemediğinden, şirket içi ortamınıza tümleştirme için ek adımlar gereklidir. ExpressRoute bağlantı hattını ve bir sanal ağ geçidini ayarlamak da gereklidir ve bu öğreticide değinilir.

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Sanal Ağ Oluşturma
> * Sanal ağ geçidi oluşturma
> * ExpressRoute devrenizi ağ geçidine bağlama
> * VCenter ve NSX Manager için URL 'Leri bulma

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com) oturum açın.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[Önceki öğreticide](tutorial-create-private-cloud.md)oluşturduğunuz kaynak grubuna gidin ve yeni bir kaynak tanımlamak Için **+ Ekle** ' yi seçin.

**Market** metin kutusuna, **sanal ağ ' a**yazın. Sanal ağ kaynağını bulup seçin.

Sanal ağ sayfasında, özel bulutunuz için Sanal ağınızı kurmak üzere **Oluştur** ' u seçin.

**Sanal ağ oluştur** sayfasında, sanal ağınız için ilgili ayrıntıları girin, aşağıdaki tabloda Özellikler açıklaması gösterilir:

> [!IMPORTANT]
> Önceki öğreticide özel bulutunuzu oluştururken kullandığınız adres alanı **ile örtüşmeyen bir** adres alanı kullanmanız gerekir.

**Temel bilgiler** sekmesinde, sanal ağ için bir ad girin ve uygun bölgeyi seçip **İleri ' yı seçin: IP adresleri**

**IP adresleri** sekmesinde, **IPv4 adres alanı**altında, önceki öğreticide oluşturduğunuz adres alanını girin.

**+ Alt ağ**Ekle ' yi seçin ve **alt ağ ekle** sayfasında, alt ağa bir ad ve uygun adres aralığı verin. Bittiğinde **Ekle**’yi seçin.

**Gözden geçir + oluştur** ' u seçin

:::image type="content" source="./media/tutorial-configure-networking/create-virtual-network.png" alt-text="sanal ağ oluşturma" border="true":::

Bilgileri doğrulayıp **Oluştur**' u seçin. Dağıtım tamamlandıktan sonra, Sanal ağınızı kaynak grubunda görürsünüz.

## <a name="create-a-virtual-network-gateway"></a>Sanal ağ geçidi oluşturma

Yukarıdaki bölümde bir sanal ağ oluşturdunuz, artık bir sanal ağ geçidi oluşturacaksınız.

Kaynak grubunuzda, yeni bir kaynak eklemek için **+ Ekle** ' yi seçin.

**Market** metin kutusunda, **sanal ağ geçidi**yazın. Sanal ağ kaynağını bulup seçin.

**Sanal ağ geçidi** sayfasında **Oluştur**' u seçin.

**Sanal ağ geçidi oluştur** sayfasının temel bilgiler sekmesinde alanlar için değerler sağlayın. alanların açıklamaları aşağıdaki tabloda gösterilmiştir:

| Alan | Değer |
| --- | --- |
| **Abonelik** | Bu değer, kaynak grubunun ait olduğu abonelikle zaten doldurulmuştur. |
| **Kaynak grubu** | Bu değer, geçerli kaynak grubu için zaten doldurulmuştur. Bu, önceki bir testte oluşturduğunuz kaynak grubu olmalıdır. |
| **Adı** | Sanal ağ geçidi için benzersiz bir ad girin. |
| **Geli** | Sanal ağ geçidinin coğrafi konumunu seçin. |
| **Ağ geçidi türü** | **ExpressRoute**' ı seçin. |
| **VPN türü** | **Rota tabanlı**' ı seçin. |
| **ISTEYIN** | Varsayılan değeri bırakın: **Standart**. |
| **Sanal ağ** | Daha önce oluşturduğunuz sanal ağı seçin. Sanal ağı görmüyorsanız, ağ geçidinin bölgesinin sanal ağınızın bölgesiyle eşleştiğinden emin olun. |
| **Ağ geçidi alt ağ adres aralığı** | Bu değer, sanal ağı seçtiğinizde doldurulur. Varsayılan değeri değiştirmeyin. |
| **Genel IP adresi** | **Yeni oluştur**’u seçin. |

:::image type="content" source="./media/tutorial-configure-networking/create-virtual-network-gateway.png" alt-text="ağ geçidi oluşturma" border="true":::

**Gözden geçir + oluştur**' u seçin, sonraki sayfada ayrıntıların doğru olduğunu doğrulayın ve sanal ağ geçidinizin dağıtımını başlatmak için **Oluştur** ' u seçin. Dağıtım tamamlandıktan sonra, ExpressRoute bağlantınızı özel bulutunuzu içeren sanal ağa bağlamak için bu öğreticideki bir sonraki bölüme geçin.

## <a name="connect-expressroute-to-the-virtual-network-gateway"></a>ExpressRoute 'ı sanal ağ geçidine bağlama

Bu bölümde, AVS özel bulutunuz ile oluşturduğunuz sanal ağ geçidi arasında bir bağlantı ekleme işlemi adım adım açıklanmaktadır.

Önceki öğreticide oluşturduğunuz özel buluta gidin ve **Yönet**' ın altındaki **bağlantı** ' yı seçin, **ExpressRoute** sekmesini seçin.

Yetkilendirme anahtarını kopyalayın. Bir yetkilendirme anahtarı yoksa, bunu yapmanız gerekir ve bunu yapmak için bir **Yetkilendirme anahtarı iste** seçeneğini belirleyin

:::image type="content" source="./media/tutorial-configure-networking/request-auth-key.png" alt-text="Yetkilendirme anahtarı isteme" border="true":::

Önceki adımda oluşturduğunuz sanal ağ geçidine gidin ve **Ayarlar**altında **Bağlantılar**' ı seçin. **Bağlantılar** sayfasında **+ Ekle**' yi seçin.

**Bağlantı ekle** sayfasında, alanlar için değerler sağlayın. Alanların açıklamaları aşağıdaki tabloda gösterilmiştir:

| Alan | Değer |
| --- | --- |
| **Adı**  | Bağlantı için bir ad girin.  |
| **Bağlantı türü**  | **ExpressRoute**' ı seçin.  |
| **Yetkilendirmeyi kullanma**  | Bu kutunun seçili olduğundan emin olun.  |
| **Sanal ağ geçidi** | Daha önce oluşturduğunuz sanal ağ geçidi  |
| **Yetkilendirme anahtarı**  | Yetkilendirme anahtarını kopyalayın ve kaynak grubunuzun ExpressRoute sekmesinden yapıştırın. |
| **Eş devre URI 'SI**  | ExpressRoute KIMLIĞINI kopyalayın ve kaynak grubunuzun ExpressRoute sekmesinden yapıştırın.  |

**Tamam**’ı seçin. Bu, ExpressRoute bağlantı hattı ve sanal ağınız arasında bağlantı oluşturur.

:::image type="content" source="./media/tutorial-configure-networking/add-connection.png" alt-text="bağlantı ekleme" border="true":::

## <a name="locate-the-urls-for-vcenter-and-nsx-manager"></a>VCenter ve NSX Manager için URL 'Leri bulma

VVenter ve NSX Manager 'da oturum açmak için vCenter web istemcisinin ve NSX-T Manager sitesinin URL 'lerine ihtiyacınız vardır. URL 'leri bulmak için:

AVS özel bulutuna gidin, **Yönet**bölümünde **kimlik**' i seçin, burada gereken bilgileri bulabilirsiniz.

:::image type="content" source="./media/tutorial-configure-networking/locate-urls.png" alt-text="vCenter URL 'lerini bulma" border="true":::

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Sanal Ağ Oluşturma
> * Sanal ağ geçidi oluşturma
> * ExpressRoute devrenizi ağ geçidine bağlama
> * VCenter ve NSX Manager için URL 'Leri bulma

Özel bulutunuzu yerel olarak yönetebilmeniz için ortamınıza bağlanmak üzere kullanılan bir bağlantı kutusu oluşturmayı öğrenmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Özel Buluta Erişme](tutorial-access-private-cloud.md)
