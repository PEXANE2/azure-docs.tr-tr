---
title: "Azure sanal WAN: hub 'da bir ağ sanal gereci (NVA) oluşturma"
description: Bu öğreticide, sanal WAN hub 'ında bir ağ sanal gereci dağıtmayı öğrenin.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 07/14/2020
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to create a Network Virtual Appliance (NVA) in my Virtual WAN hub.
ms.openlocfilehash: c5f120d6d9d80db4eaa9cf6f68e8dd27d45c8aea
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87100066"
---
# <a name="how-to-create-a-network-virtual-appliance-in-an-azure-virtual-wan-hub-preview"></a>Azure sanal WAN hub 'ında ağ sanal gereci oluşturma (Önizleme)

Bu öğreticide, Azure 'daki bir **ağ sanal** gereci (NVA) aracılığıyla Azure 'daki kaynaklarınıza bağlanmak IÇIN sanal WAN 'ın nasıl kullanılacağı gösterilmektedir. Bu bağlantı türü için, şirket içinde yer alan ve kendisine atanmış dışarıya yönelik bir genel IP adresi atanmış olan bir VPN cihazı gerekir. Sanal WAN hakkında daha fazla bilgi için bkz. [Sanal WAN'a Genel Bakış](virtual-wan-about.md).

Bu makaledeki adımlar, sanal WAN hub 'ında bir **Barbcuda CloudGen WAN** ağ sanal gereci oluşturmanıza yardımcı olur. Bu alıştırmayı tamamlayabilmeniz için, başlamadan önce hub 'a dağıttığınız bir barbcuda bulut şirket Içi aygıtı (CPE) ve bir Barrampaya da CloudGen WAN gereci lisansına sahip olmanız gerekir.

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki ölçütleri karşıladığınızı doğrulayın:

* Barvacuda CloudGen WAN Gateway için bir lisans edinin. Bunun nasıl yapılacağı hakkında daha fazla bilgi için bkz. [Barbcuda CloudGen WAN belgeleri](https://www.barracuda.com/products/cloudgenwan)

* Bağlanmak istediğiniz bir sanal ağınız var. Şirket içi ağlarınızın alt ağlarının hiçbirinin, bağlanmak istediğiniz sanal ağlarla çakışmadığından emin olun. Azure portal bir sanal ağ oluşturmak için [hızlı](../virtual-network/quick-create-portal.md)başlangıca bakın.

* Sanal ağınızda sanal ağ geçidi yok. Sanal ağınızda bir ağ geçidi (VPN veya ExpressRoute) varsa, tüm ağ geçitlerini kaldırmanız gerekir. Bu yapılandırma, sanal ağın bunun yerine sanal WAN hub ağ geçidine bağlanmasını gerektirir.

* Hub bölgenizden bir IP adresi aralığı edinin. Hub, sanal WAN tarafından oluşturulan ve kullanılan bir sanal ağ. Hub için belirttiğiniz adres aralığı, bağlandığınız mevcut sanal ağlarınızla çakışamaz. Ayrıca bağlandığınız şirket içi adres aralıklarıyla da çakışamaz. Şirket içi ağ yapılandırmanızda bulunan IP adresi aralıklarını tanımıyorsanız, sizin için bu ayrıntıları sağlayabilecek biriyle koordine edebilirsiniz.

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-virtual-wan"></a><a name="openvwan"></a>Sanal WAN oluşturma

[!INCLUDE [Create virtual WAN](../../includes/virtual-wan-create-vwan-include.md)]

## <a name="create-a-hub"></a><a name="hub"></a>Hub oluşturma

Hub, siteden siteye, ExpressRoute, Noktadan siteye veya ağ sanal gereç işlevlerine yönelik ağ geçitleri içerebilen bir sanal ağ. Hub oluşturulduktan sonra site eklemeseniz bile hub için ücretlendirilirsiniz. Siteden siteye VPN Ağ Geçidi oluşturmayı seçerseniz, sanal hub 'da siteden siteye VPN ağ geçidinin oluşturulması 30 dakika sürer. Siteden siteye, ExpressRoute veya Noktadan siteye farklı olarak, hub VNet 'e bir ağ sanal gereci dağıtabilmeniz için önce hub oluşturulması gerekir.

1. Oluşturduğunuz sanal WAN 'ı bulun. **Sanal WAN** sayfasında, **bağlantı** bölümünde, **hub**' ı seçin.
1. **Hub 'lar** sayfasında + yeni hub ' ı seçerek **sanal hub oluştur** sayfasını açın.

   :::image type="content" source="./media/how-to-nva-hub/vwan-hub.png" alt-text="Temel bilgiler":::
1. **Sanal hub** sayfası **temel bilgileri** Oluştur sekmesinde aşağıdaki alanları doldurun:

   **Proje ayrıntıları**

   * Bölge (daha önce konum olarak adlandırılır)
   * Name
   * Hub özel adres alanı. En küçük adres alanı bir hub oluşturmak için/24 ' tir. Bu,/25 ile/32 arasında herhangi bir aralığın oluşturulması sırasında bir hata üretecektir. Microsoft tarafından yönetilen bir hizmet olan Azure sanal WAN, farklı ağ geçitleri/hizmetler için sanal hub 'da uygun alt ağları oluşturur. (Örneğin: ağ sanal cihazları, VPN ağ geçitleri, ExpressRoute ağ geçitleri, kullanıcı VPN/Noktadan siteye ağ geçitleri, güvenlik duvarı, yönlendirme vb.). Microsoft bunu hizmetin bir parçası olarak yaptığından, kullanıcının sanal hub 'daki hizmetler için alt ağ adres alanını açıkça planlaması gerekmez.
1. Doğrulamak için **gözden geçir + oluştur** ' u seçin.
1. Hub 'ı oluşturmak için **Oluştur** ' u seçin.

## <a name="create-the-network-virtual-appliance-in-the-hub"></a>Hub 'da ağ sanal gereç oluşturma

Bu adımda, hub 'da bir ağ sanal gereci oluşturacaksınız. Her NVA 'nın yordamı her NVA ortağının ürünü için farklı olacaktır. Bu örnekte, bir Barbcuda CloudGen WAN Gateway oluşturuyoruz.

1. Önceki adımda oluşturduğunuz sanal WAN hub 'ını bulun ve açın.

   :::image type="content" source="./media/how-to-nva-hub/nva-hub.png" alt-text="Sanal hub":::
1. Ağ sanal cihazları kutucuğunu bulun ve **Oluştur** bağlantısını seçin.
1. **Ağ sanal gereç** dikey penceresinde **Barescuda cloudgen WAN**' ı seçin ve ardından **Oluştur** düğmesini seçin.

   :::image type="content" source="./media/how-to-nva-hub/select-nva.png" alt-text="NVA seçin":::
1. Bu işlem sizi Barırcuda CloudGen WAN Gateway için Azure Market teklifine götürür. Koşulları okuyun ve ardından, hazırsanız **Oluştur** düğmesini seçin.

   :::image type="content" source="./media/how-to-nva-hub/barracuda-create-basics.png" alt-text="Barbcuda NVA temelleri":::
1. **Temel bilgiler** sayfasında, aşağıdaki bilgileri sağlamanız gerekir:

   * **Abonelik** -sanal WAN ve hub 'ı dağıtmak için kullandığınız aboneliği seçin.
   * **Kaynak grubu** -sanal WAN ve hub 'ı dağıtmak Için kullandığınız kaynak grubunu seçin.
   * **Bölge** -sanal hub kaynağınızın bulunduğu bölgeyi seçin.
   * **Uygulama adı** -Barbcuda NEXTGEN WAN, yönetilen bir uygulamadır. Bu kaynağı, aboneliğinizde göründüğünde çağrılabilmesi için, bu kaynağı kolayca tanımlamanızı kolaylaştıran bir ad seçin.
   * **Yönetilen kaynak grubu** -bu, Barçcuda tarafından yönetilen kaynakları dağıtan yönetilen kaynak grubunun adıdır. Bu ad için önceden doldurulmuş olmalıdır.
1. **Sonraki: CloudGen WAN Gateway** düğmesini seçin.

   :::image type="content" source="./media/how-to-nva-hub/barracuda-cloudgen-wan.png" alt-text="CloudGen WAN Gateway":::
1. Aşağıdaki bilgileri buraya sağlayın:

   * **Sanal WAN hub 'ı** -bu NVA 'yı dağıtmak ISTEDIĞINIZ sanal WAN hub 'ı.
   * **NVA altyapı birimleri** -bu NVA 'yı dağıtmak istediğiniz NVA altyapı birimi sayısını belirtin. Bu NVA aracılığıyla bu hub 'a Bağlanılacak tüm dal sitelerinde sağlamak istediğiniz toplam bant genişliği kapasitesini seçin.
   * **Token** -barkcuda, kendinizi bu ürünün kayıtlı kullanıcısı olarak tanımlamak için burada bir kimlik doğrulama belirteci sağlamanızı gerektirir. Bunu Barçcuda 'dan edinmeniz gerekir.
1. Devam etmek için **gözden geçir ve oluştur** düğmesini seçin.
1. Bu sayfada, ortak yönetici erişim sözleşmesi koşullarını kabul etmeniz istenir. Bu, yayımcının bu dağıtımdaki bazı kaynaklara erişebileceği yönetilen uygulamalarla standart bir standarttır. **Yukarıdaki hüküm ve koşulları kabul ediyorum** kutusunu işaretleyin ve ardından **Oluştur**' u seçin.

## <a name="connect-the-vnet-to-the-hub"></a><a name="vnet"></a>VNet 'i hub 'a bağlama

[!INCLUDE [Connect](../../includes/virtual-wan-connect-vnet-hub-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.
* Sanal WAN hub 'ında NVA 'lar hakkında daha fazla bilgi edinmek için bkz. [sanal WAN hub 'ında (Önizleme) ağ sanal gereci hakkında](about-nva-hub.md).