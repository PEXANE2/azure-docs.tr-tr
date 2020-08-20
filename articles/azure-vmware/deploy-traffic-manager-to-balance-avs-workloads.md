---
title: Azure VMware Solution (AVS) iş yüklerini dengelemek için Traffic Manager dağıtma
description: Farklı bölgelerde birden fazla uç nokta arasında uygulama iş yüklerini dengelemek için Traffic Manager Azure VMware çözümü (AVS) ile tümleştirmeyi öğrenin.
ms.topic: how-to
ms.date: 08/14/2020
ms.openlocfilehash: dc3107c6a237273b103fe52a91b3569d9e694e1a
ms.sourcegitcommit: 628be49d29421a638c8a479452d78ba1c9f7c8e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88643063"
---
# <a name="deploy-traffic-manager-to-balance-azure-vmware-solution-avs-workloads"></a>Azure VMware Solution (AVS) iş yüklerini dengelemek için Traffic Manager dağıtma

Bu makalede, birden fazla uç nokta genelinde uygulama iş yüklerini dengelemek için Azure VMware çözümü (AVS) ile Traffic Manager tümleştirme adım adım açıklanmaktadır. Çeşitli AVS bölgelerini kapsayan üç uygulama ağ geçidi arasında trafiği yönlendiren Traffic Manager bir senaryoya bakacağız: Doğu ABD içinde Batı ABD, Batı Avrupa ve şirket içi. 

Azure Traffic Manager, genel Azure bölgelerinde bulunan hizmetlere en iyi şekilde dağıtım yapmanızı sağlayan DNS tabanlı bir trafik yük dengeleyicidir. Azure çalışan iş yükleri ve dış genel uç noktalar arasında uygulama trafiğinin yükünü dengelemeye çalışır. Traffic Manager hakkında daha fazla bilgi için bkz. [Traffic Manager nedir?](../traffic-manager/traffic-manager-overview.md)

Önce [önkoşulları](#prerequisites) gözden geçirin; daha sonra şu işlemleri yapmak için yordamlar izlenecek:

> [!div class="checklist"]
> * Uygulama ağ geçitlerinizin yapılandırmasını doğrulayın
> * NSX-T segmentinin yapılandırmasını doğrulama
> * Traffic Manager profilinizi oluşturma
> * Traffic Manager profilinize dış uç noktalar ekleyin

## <a name="topology"></a>Topoloji

Azure Traffic Manager, aşağıdaki şekilde gösterildiği gibi, bölgesel uç noktalar arasındaki DNS düzeyindeki uygulamalar için Yük Dengeleme sağlar. Uygulama ağ geçitleri, IIS sunucuları olarak yapılandırılmış arka uç havuzu üyelerine sahiptir ve AVS dış uç noktaları olarak başvurulur.

İki AVS özel bulut bölgesi, Batı ABD ve Batı Avrupa ve Doğu ABD içindeki şirket içi sunucu arasında sanal ağ bağlantısı, bir ExpressRoute ağ geçidi kullanır.   

![AVS ile tümleştirme Traffic Manager](media/traffic-manager/traffic-manager-topology.png)
 
## <a name="prerequisites"></a>Ön koşullar

- Farklı AVS bölgelerinde çalışan Microsoft IIS sunucuları olarak yapılandırılmış üç sanal makine: Batı ABD, Batı Avrupa ve şirket içi. 

- Batı ABD, Batı Avrupa ve şirket içinde dış uç noktaları olan bir uygulama ağ geçidi.

- Doğrulama için internet bağlantısı olan ana bilgisayar. 

## <a name="verify-configuration-of-your-application-gateways"></a>Uygulama ağ geçitlerinizin yapılandırmasını doğrulayın

[Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) , Web uygulamalarınıza yönelik trafiği yönetmenizi sağlayan bir katman 7 Web trafiği yük dengeleyicidir. Application Gateway hakkında daha fazla bilgi için bkz. [Azure Application Gateway nedir?](../application-gateway/overview.md) 

Bu senaryoda, üç uygulama ağ geçidi örneği dış AVS uç noktaları olarak yapılandırılır. Uygulama ağ geçitlerinde, gelen katman 7 isteklerinin yükünü dengelemek için arka uç havuzu üyeleri olarak yapılandırılmış AVS sanal makineleri vardır. (AVS sanal makineleriyle arka uç havuzları olarak Application Gateway yapılandırma hakkında bilgi edinmek için bkz. Azure [Application Gateway kullanarak Web uygulamalarınızı Azure VMware çözümünde koruma](protect-avs-web-apps-with-app-gateway.md).)  

Aşağıdaki adımlar, uygulama ağ geçitlerinizin doğru yapılandırmasını doğrular.

1. Azure portal açın ve geçerli uygulama ağ geçitlerinizin listesini görüntülemek için **uygulama ağ geçitleri** ' ni seçin. 

    Bu senaryo için üç uygulama ağ geçidi yapılandırdık:
    - AVS-GW-WUS
    - AVS-EUS (Şirket içi)
    - AVS-GW-WEU

    :::image type="content" source="media/traffic-manager/app-gateways-list-1.png" alt-text="Uygulama ağ geçitlerinin listesi." lightbox="media/traffic-manager/app-gateways-list-1.png":::

2. Önceden dağıtılan uygulama ağ geçitlerinden birini seçin. Uygulama ağ geçidinde çeşitli bilgileri gösteren bir pencere açılır. Arka uç havuzlarından birinin yapılandırmasını doğrulamak için **arka uç havuzlarını** seçin.

   :::image type="content" source="media/traffic-manager/backend-pool-config.png" alt-text="Application Gateway ayrıntıları." lightbox="media/traffic-manager/backend-pool-config.png":::
 
3. Bu durumda, 172.29.1.10 IP adresine sahip bir Web sunucusu olarak yapılandırılmış bir sanal makine arka uç havuzu üyesi görüyoruz.
 
     :::image type="content" source="media/traffic-manager/backend-pool-ip-address.png" alt-text="Arka uç havuzunu düzenleyin.":::

    Benzer şekilde, diğer uygulama ağ geçitlerinin ve arka uç havuz üyelerinin yapılandırılmasını da doğrulayabilirsiniz. 

## <a name="verify-configuration-of-the-nsx-t-segment"></a>NSX-T segmentinin yapılandırmasını doğrulama

NSX-T Manager 'da oluşturulan ağ kesimleri, vCenter 'daki sanal makineler için ağlar olarak kullanılır. Daha fazla bilgi için [Azure VMware çözümünde (AVS) BIR NSX-T ağ kesimi oluşturma](tutorial-nsx-t-network-segment.md)öğreticisine bakın.

Senaryolarımızda, bir NSX-T segmenti, arka uç havuzu üyesi sanal makinenin eklendiği AVS ortamında yapılandırılır.

1. Yapılandırılmış segmentlerinizi görüntülemek için **segmentleri** seçin. Bu durumda, contoso-SEGMENT1 ' nin bir katman 1 esnek yönlendirici olan contoso-T01 ağ geçidine bağlı olduğunu görüyoruz.

    :::image type="content" source="media/traffic-manager/nsx-t-segment-avs.png" alt-text="NSX-T Manager 'daki segment profilleri.":::    

2. Katman 1 ağ geçitlerinizin bir listesini, bağlantılı parçaların sayısıyla birlikte görmek için **Katman 1 ağ geçitleri** ' ni seçin. Contoso-T01 ile bağlantılı segmenti seçin. Katman-01 yönlendiricisinde yapılandırılan mantıksal arabirimi gösteren bir pencere açılır. Bu, kesime bağlı arka uç havuzu üye sanal makinesinin ağ geçidi olarak görev yapar.

   :::image type="content" source="media/traffic-manager/nsx-t-segment-linked-2.png" alt-text="Bağlantılı segmentler.":::    

3. VM vSphere istemcisinde, ayrıntılarını görüntülemek için sanal makineyi seçin. IP adresi, yukarıdaki bölümün 3. adımında gördük ile eşleşir: 172.29.1.10.

    :::image type="content" source="media/traffic-manager/nsx-t-vm-details.png" alt-text="Sanal makine ayrıntıları.":::    

4. Sanal makineyi seçin ve ardından NSX-T segmentiyle bağlantıyı doğrulamak için **eylemler > Ayarları Düzenle** ' ye tıklayın.

## <a name="create-your-traffic-manager-profile"></a>Traffic Manager profilinizi oluşturma

1. [Azure Portal](https://rc.portal.azure.com/#home)oturum açın. **Azure hizmetleri > ağ**altında **Traffic Manager profiller**' i seçin.

2. Yeni bir Traffic Manager profili oluşturmak için **+ Ekle** ' yi seçin.
 
3. Profil adı, yönlendirme yöntemi belirtin (Bu senaryoda ağırlıklı olarak kullanacağız; [Traffic Manager yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md)), abonelik ve kaynak grubu ' na bakın ve **Oluştur**' u seçin.

## <a name="add-external-endpoints-into-the-traffic-manager-profile"></a>Traffic Manager profiline dış uç noktalar ekleme

1. Arama sonuçları bölmesinden Traffic Manager profilini seçin, **uç noktalar** ' ı ve sonra **+ Ekle**' yi seçin.

2. Gerekli ayrıntıları girin: tür, ad, tam etki alanı adı (FQDN) veya IP ve ağırlık (Bu senaryoda, her bir uç noktaya 1 ağırlık atacağız). **Ekle**’yi seçin.

    :::image type="content" source="media/traffic-manager/traffic-manager-profile.png" alt-text="Traffic Manager profili-uç nokta ekleyin.":::  
 
    Bu, dış uç noktayı oluşturur. İzleyici durumu **çevrimiçi**olmalıdır. 

    Biri farklı bir bölgede ve diğer şirket içi olmak üzere iki farklı dış uç nokta oluşturmak için aynı adımları yineleyin. Oluşturulduktan sonra üçü Traffic Manager profilinde görüntülenir ve üçünün durumu **çevrimiçi**olmalıdır.

3. **Genel bakış**'ı seçin. **DNS adı**altında URL 'yi kopyalayın.

    :::image type="content" source="media/traffic-manager/traffic-manager-endpoints.png" alt-text="DNS adıyla Traffic Manager genel bakış."::: 

4. DNS adı URL 'sini bir tarayıcıya yapıştırın. Aşağıdaki ekran görüntüsünde Batı Avrupa bölgesine yönlendiren trafik gösterilmektedir.

    :::image type="content" source="media/traffic-manager/traffic-to-west-europe.png" alt-text="Batı Avrupa yönlendirilen trafik."::: 

5. Tarayıcınızı yenileyin. Aşağıdaki ekran görüntüsünde trafik artık Batı ABD bölgesindeki başka bir arka uç havuzu üyesi kümesine yönlendirmektedir.

    :::image type="content" source="media/traffic-manager/traffic-to-west-us.png" alt-text="Batı ABD yönlendirilen trafik."::: 

6. Tarayıcınızı yeniden yenileyin. Aşağıdaki ekran görüntüsünde trafik artık şirket içi havuz üyelerinin son kümesini yönlendiren trafik gösterilmektedir.

    :::image type="content" source="media/traffic-manager/traffic-to-on-premises.png" alt-text="Şirket içine yönlendirilen trafik.":::

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:

- [Azure VMware çözümünde (AVS) Azure Application Gateway kullanma](protect-avs-web-apps-with-app-gateway.md)
- [Traffic Manager yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md)
- [Yük Dengeleme hizmetlerini Azure 'da birleştirme](../traffic-manager/traffic-manager-load-balancing-azure.md)
- [Traffic Manager performansını ölçme](../traffic-manager/traffic-manager-performance-considerations.md)
