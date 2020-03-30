---
title: Birden çok IP yapılandırmalarında yük dengelemesi - Azure portalı
titleSuffix: Azure Load Balancer
description: Bu makalede, Azure portalını kullanarak birincil ve ikincil IP yapılandırmaları arasında yük dengeleme hakkında bilgi edinin.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: se0dec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: allensu
ms.openlocfilehash: 4bf74986462ecb2659505f8a1261b9b24aba3fee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74076998"
---
# <a name="load-balancing-on-multiple-ip-configurations-by-using-the-azure-portal"></a>Azure portalını kullanarak birden fazla IP yapılandırması üzerinde dengeleme

> [!div class="op_single_selector"]
> * [Portal](load-balancer-multiple-ip.md)
> * [Powershell](load-balancer-multiple-ip-powershell.md)
> * [CLI](load-balancer-multiple-ip-cli.md)


Bu makalede, ikincil bir ağ arabirimi denetleyicisinde (NIC) birden çok IP adresine sahip Azure Yük Dengeleyicisi'ni nasıl kullanacağınızı göstereceğiz. Aşağıdaki diyagram senaryomuzu göstermektedir:

![Yük dengeleyici senaryosu](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

Senaryomuzda, aşağıdaki yapılandırmayı kullanıyoruz:

- Windows çalıştıran iki sanal makine (VM).
- Her VM'nin birincil ve ikincil bir NIC'si vardır.
- Her ikincil NIC'in iki IP yapılandırması vardır.
- Her VM iki web sitesi barındıran: contoso.com ve fabrikam.com.
- Her web sitesi ikincil NIC'de bir IP yapılandırmasına bağlıdır.
- Azure Yük Dengeleyicisi, her web sitesi için bir tane olmak üzere iki ön uç IP adresini ortaya çıkarmak için kullanılır. Ön uç adresleri, trafiği her web sitesi için ilgili IP yapılandırmasına dağıtmak için kullanılır.
- Aynı bağlantı noktası numarası hem ön uç IP adresleri hem de arka uç havuzu IP adresleri için kullanılır.

## <a name="prerequisites"></a>Ön koşullar

Senaryo örneğimiz, **contosofabrikam** adlı bir kaynak grubunuz olduğunu varsayar ve bu grup aşağıdaki gibi yapılandırılır:

- Kaynak grubu **myVNet**adlı bir sanal ağ içerir.
- **myVNet** ağı **VM1** ve **VM2**adlı iki VM içerir.
- VM1 ve VM2 **myAvailset**adlı aynı kullanılabilirlik kümesi bulunmaktadır. 
- VM1 ve VM2'nin her birinde sırasıyla **VM1NIC1** ve **VM2NIC1**adlı birincil NIC vardır. 
- VM1 ve VM2'nin her birinde sırasıyla **VM1NIC2** ve **VM2NIC2**adında ikincil bir NIC vardır.

Birden çok NIC'li VM oluşturma hakkında daha fazla bilgi için [PowerShell'i kullanarak birden çok NIC'li VM oluştur'a](../virtual-machines/windows/multiple-nics.md)bakın.

## <a name="perform-load-balancing-on-multiple-ip-configurations"></a>Birden çok IP yapılandırmalarında yük dengeleme gerçekleştirme

Bu makalede özetlenen senaryoyu elde etmek için aşağıdaki adımları tamamlayın.

### <a name="step-1-configure-the-secondary-nics"></a>Adım 1: İkincil NIC'leri yapılandırın

Sanal ağınızdaki her VM için ikincil NIC için IP yapılandırmasını ekleyin:  

1. Azure portalına göz https://portal.azure.comatın: . Azure hesabınızla oturum açın.

2. Ekranın sol üst kısmında Kaynak **Grubu** simgesini seçin. Ardından, VM'lerinizin bulunduğu kaynak grubunu seçin (örneğin, **contosofabrikam).** **Kaynak grupları** bölmesi, VM'ler için tüm kaynakları ve NIC'leri görüntüler.

3. Her VM'nin ikincil NIC'si için IP yapılandırmasını ekleyin:

    1. Yapılandırmak istediğiniz ikincil NIC'yi seçin.
    
    2. **IP yapılandırmalarını**seçin. Bir sonraki bölmede, en üste yakın, **Ekle'yi**seçin.

    3. **IP ekle yapılandırmaları**altında NIC'e ikinci bir IP yapılandırması ekleyin: 

        1. İkincil IP yapılandırması için bir ad girin. (Örneğin, VM1 ve VM2 için, IP **yapılandırması VM1NIC2-ipconfig2** ve **VM2NIC2-ipconfig2,** sırasıyla adlandırın.)

        2. Özel **IP adresi**, **Ayırma** ayarı için **Statik'i**seçin.

        3. **Tamam'ı**seçin.

İkincil NIC için ikinci IP yapılandırması tamamlandıktan sonra, verilen NIC için **IP yapılandırma ayarları** altında görüntülenir.

### <a name="step-2-create-the-load-balancer"></a>2. Adım: Yük dengeleyiciyi oluşturun

Yapılandırma için yük dengeleyicinizi oluşturun:

1. Azure portalına göz https://portal.azure.comatın: . Azure hesabınızla oturum açın.

2. Ekranın sol üst kısmında, **kaynak** > **Ağ** > **Yük Dengeleyicisi**oluştur'u seçin. Ardından **Oluştur'u**seçin.

3. **Yük dengeleyicisi oluştur**altında, yük bakiyeciniz için bir ad yazın. Bu senaryoda, **mylb**adını kullanıyoruz.

4. **Genel IP adresi**altında, **PublicIP1**adında yeni bir genel IP oluşturun.

5. **Kaynak Grubu**altında, VM'leriniz için varolan kaynak grubunu seçin (örneğin, **contosofabrikam).** Yük dengeleyicinizi dağıtmak için konumu seçin ve ardından **Tamam'ı**seçin.

Yük dengeleyicisi dağıtmaya başlar. Dağıtımın başarıyla tamamlanması birkaç dakika sürebilir. Dağıtım tamamlandıktan sonra, yük dengeleyici kaynak grubunuzda kaynak olarak görüntülenir.

### <a name="step-3-configure-the-front-end-ip-pool"></a>Adım 3: Ön uç IP havuzunu yapılandırın

Her web sitesi için (contoso.com ve fabrikam.com), yük bakiyenizin üzerindeki ön uç IP havuzunu yapılandırın:

1. Portalda, **Diğer hizmetleri**seçin. Filtre kutusunda, **Genel IP adresini** yazın ve ardından Genel IP **adreslerini**seçin. Bir sonraki bölmede, en üste yakın, **Ekle'yi**seçin.

2. Her iki web sitesi (contoso.com ve fabrikam.com) için iki genel IP adresini **(PublicIP1** ve **PublicIP2)** yapılandırın:

   1. Ön uç IP adresiniz için bir ad yazın.

   2. **Kaynak Grubu**için, VM'leriniz için varolan kaynak grubunu seçin (örneğin, **contosofabrikam).**

   3. **Konum**için, VM'ler ile aynı konumu seçin.

   4. **Tamam'ı**seçin.

      Ortak IP adresleri oluşturulduktan sonra, Ortak **IP** adresleri altında görüntülenir.

3. <a name="step3-3"></a>Portalda, **Diğer hizmetleri**seçin. Filtre kutusunda yük **dengeleyicisini** yazın ve ardından **Yük Dengeleyici'ni**seçin. 

4. Ön uç IP havuzunu eklemek istediğiniz yük bakiyesini **(mylb)** seçin.

5. **Ayarlar**altında **Frontend IP yapılandırması'nı**seçin. Bir sonraki bölmede, en üste yakın, **Ekle'yi**seçin.

6. Ön uç IP adresiniz için bir ad yazın (örneğin, **contosofe** veya **fabrikamfe).**

7. <a name="step3-7"></a>**IP adresini**seçin. **Genel IP adresini seçin**altında, ön uç **(PublicIP1** veya **PublicIP2)** için IP adreslerini seçin.

8. Bu bölümde adım <a href="#step3-3">7'den adım 3'e</a> kadar yineleyerek ikinci ön uç IP adresini oluşturun. <a href="#step3-7">step 7</a>

Ön uç havuzu yapılandırıldıktan sonra, IP adresleri yük dengeleyicifrontend **IP yapılandırma** ayarları altında görüntülenir. 
    
### <a name="step-4-configure-the-back-end-pool"></a>Adım 4: Arka uç havuzunu yapılandırın

Her web sitesi için (contoso.com ve fabrikam.com), yük bakiyenizin arka uç adresi havuzunu yapılandırın:
        
1. Portalda, **Diğer hizmetleri**seçin. Filtre kutusunda yük **dengeleyicisini** yazın ve ardından **Yük Dengeleyici'ni**seçin.

2. Arka uç havuzunu eklemek istediğiniz yük bakiyesini **(mylb)** seçin.

3. **Ayarlar**altında, **Arka Uç Havuzları'nı**seçin. Arka uç havuzunuz için bir ad yazın (örneğin, **contosopool** veya **fabrikampool).** Bir sonraki bölmede, en üste yakın, **Ekle'yi**seçin. 

4. **İlişkili için,** **Kullanılabilirlik kümesini**seçin.

5. **Kullanılabilirlik kümesi**için **myAvailset'i**seçin.

6. Her iki VM için hedef ağ IP yapılandırmalarını ekleyin: 

    ![Yük dengeleyicisi için arka uç havuzlarını yapılandırma](./media/load-balancer-multiple-ip/lb-backendpool.PNG)
    
    1. **Hedef sanal makine**için, arka uç havuzuna eklemek istediğiniz VM'yi (örneğin, **VM1** veya **VM2)** seçin.

    2. **Ağ IP yapılandırması**için, önceki adımda seçtiğiniz VM için ikincil NIC'nin IP yapılandırmasını seçin (örneğin, **VM1NIC2-ipconfig2** veya **VM2NIC2-ipconfig2).**

7. **Tamam'ı**seçin.

Arka uç havuzu yapılandırıldıktan sonra, adresler yük bakiyesi **Arka uç havuz** ayarlarınızın altında görüntülenir.

### <a name="step-5-configure-the-health-probe"></a>Adım 5: Sistem sondasını yapılandırın

Yük bakiyeciniz için bir sistem durumu sondası yapılandırın:

1. Portalda, **Diğer hizmetleri**seçin. Filtre kutusunda yük **dengeleyicisini** yazın ve ardından **Yük Dengeleyici'ni**seçin.

2. Sağlık sondasını eklemek istediğiniz yük dengeleyicisini **(mylb)** seçin.

3. **Ayarlar**altında, **Sağlık sondası**seçin. Bir sonraki bölmede, en üste yakın, **Ekle'yi**seçin. 

4. Sistem durumu sondası için bir ad yazın (örneğin, **HTTP).** **Tamam'ı**seçin.

### <a name="step-6-configure-load-balancing-rules"></a>Adım 6: Yük dengeleme kurallarını yapılandırma

Her web sitesi için (contoso.com ve fabrikam.com, yük dengeleme kurallarını yapılandırın:
    
1. <a name="step6-1"></a>**Ayarlar** **altında, Yük dengeleme kurallarını**seçin. Bir sonraki bölmede, en üste yakın, **Ekle'yi**seçin. 

2. **Ad**için, yük dengeleme kuralı için bir ad yazın (örneğin, contoso.com için **HTTPc** veya fabrikam.com için **HTTPf).**

3. **Frontend IP adresi**için, daha önce oluşturduğunuz ön uç IP adresini seçin (örneğin, **contosofe** veya **fabrikamfe).**

4. **Bağlantı noktası** ve Arka uç **bağlantı noktası**için varsayılan değeri **80**olarak tutun.

5. **Kayan IP (doğrudan sunucu dönüşü)** için **Devre Dışı'yı**seçin.

6. <a name="step6-6"></a>**Tamam'ı**seçin.

7. Bu bölümde <a href="#step6-1">adım 1'i</a> <a href="#step6-6">adım 6'dan</a> yineleyerek ikinci yük dengeleyici kuralını oluşturun.

Kurallar yapılandırıldıktan sonra, yük dengeleyiciniz **Yük dengeleme kuralları** ayarlarıaltında görüntülenir.

### <a name="step-7-configure-dns-records"></a>Adım 7: DNS kayıtlarını yapılandırma

Son adım olarak, DNS kaynak kayıtlarınızı yük bakiyeniz için ilgili ön uç IP adreslerini işaret etmek üzere yapılandırın. Etki alanlarınızı Azure DNS'de barındırabilirsiniz. Yük Bakiyesi ile Azure DNS kullanma hakkında daha fazla bilgi [için](../dns/dns-for-azure-services.md)bkz.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure'da yük dengeleme hizmetlerini kullanmada](../traffic-manager/traffic-manager-load-balancing-azure.md)Azure'da yük dengeleme hizmetlerini nasıl birleştirebilirsiniz hakkında daha fazla bilgi edinin.
- [Azure Yük Dengeleyicisi için Azure Monitor günlüklerinde](../load-balancer/load-balancer-monitor-log.md)yük dengeleyicisini yönetmek ve sorun gidermek için farklı günlük türlerini nasıl kullanabileceğinizi öğrenin.
