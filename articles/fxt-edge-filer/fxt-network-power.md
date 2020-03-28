---
title: "Öğretici: Kabloları Azure FXT Edge Filer'a bağlayın"
description: Azure FXT Edge Filer donanımı için ağ bağlantı noktalarını kabloya bağlama ve güç ekleme
author: ekpgh
ms.author: rohogue
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 07/01/2019
ms.openlocfilehash: 1e3c60fd955bd899955c46b7670acd3f088d0f86
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79239787"
---
# <a name="tutorial-make-network-connections-and-supply-power-to-the-azure-fxt-edge-filer-node"></a>Öğretici: Azure FXT Edge Filer düğümüne ağ bağlantıları ve güç sağlama

Bu öğretici, bir Azure FXT Edge Filer donanım düğümü için ağ bağlantılarını nasıl kabloyla bağlayın öğretir.

Bu öğreticide şunları öğreneceksiniz: 

> [!div class="checklist"]
> * Ortamınız için ağ kablosu türünü seçme
> * Azure FXT Edge Filer düğümünü veri merkezi ağınıza bağlama
> * Kablolar kablo yönetim kolundan (CMA) nasıl yönlendirilir?
> * Gücü raflı cihaza bağlama ve üzerinde güç verme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce Azure FXT Edge Filer standart bir donanım rafına yüklenmelidir. CMA filer düğümüne takılmalıdır. 

## <a name="identify-ports"></a>Bağlantı noktalarını tanımlama

Azure FXT Edge Filer'ınızın arka tarafındaki çeşitli bağlantı noktalarını tanımlayın. 
 
![Kablolu aygıtın arkası](media/fxt-back-annotated.png)

## <a name="cable-the-device"></a>Cihazın kablolarını bağlama

* RJ-45 bağlantı noktalarını [Ağ bağlantı noktalarında](#network-ports)açıklandığı şekilde veri merkezinizin ağ kaynağına bağlayın.  
* [iDRAC bağlantı noktasını](#idrac-port) güvenli bir DHCP sunucusuyla ayrı bir ağa güvenli bir şekilde bağlayın. 
* Bir klavyeyi bağlamak ve ilk kurulum için düğüme monitör bağlamak için USB bağlantı noktalarını ve VGA bağlantı noktasını kullanın. Düğümü önyüklemeniz ve düğümün diğer bağlantı noktalarını etkinleştirmek için [bir başlangıç parolaayarlamanız](fxt-node-password.md) gerekir. Ayrıntılar için [ilk parolaları ayarla'yı](fxt-node-password.md) okuyun. 

Bu makalede, düğüm için [AC gücü](#connect-power-cables) nasıl bağlanılmayı da açıklamaktadır. 

Bu makalede, özel sorun giderme için gerekirse düğümün [seri bağlantı noktasına](#serial-port-only-when-necessary)nasıl bağlanılacak açıklanır. 

### <a name="network-ports"></a>Ağ bağlantı noktaları 

Her Azure FXT Kenar Filer düğümü aşağıdaki ağ bağlantı noktalarını içerir: 

* Altı yüksek hızlı 25GbE/10GbE çift rate veri portu: 

  * İki çift bağlantı noktası eklentisi ağ bağdaştırıcısı tarafından sağlanan dört bağlantı noktası
  * Anakart asma ağ bağdaştırıcısı tarafından sağlanan iki bağlantı noktası 

* Anakart asma ağ adaptörü tarafından sağlanan iki 1GbE bağlantı noktası 

Yüksek hızlı 25GbE/10GbE veri bağlantı noktaları standart SFP28 uyumlu kafeslere sahiptir. Optik kabloları kullanmak için SFP28 optik alıcı-verici modüllerini yüklemeniz gerekir (sağlanmaz).

1GbE bağlantı noktaları standart RJ-45 konektörlerine sahiptir.

Desteklenen kabloların, anahtarların ve alıcı vericilerin tam listesi için [Cavium FastlinQ 41000 Serisi Birlikte Çalışabilirlik](https://www.marvell.com/documents/xalflardzafh32cfvi0z/)Matrisi'ne başvurun.

Sisteminiz için kullanılacak bağlantı türü veri merkezi ortamınıza bağlıdır.

* 25GbE'lik bir ağa bağlanıyorsanız, yüksek hızlı veri bağlantı noktalarının her birini aşağıdaki kablo türlerinden biriyle kablolayın:

  * 25GbE veya çift sınıf 25GbE/10GbE özelliğine sahip optik kablo ve SFP28 optik alıcı-verici
  * SFP28 tipi 25GbE özellikli doğrudan takmak twinaxial kablo

* 10GbE'lik bir ağa bağlanıyorsanız, yüksek hızlı veri bağlantı noktalarının her birini aşağıdakilerden biriyle kablolayın: 

  * 10GbE veya çift sınıf 25GbE/10GbE özelliğine sahip optik kablo ve SFP28 optik alıcı-verici.
  * SFP28 tipi 25GbE özellikli doğrudan takmak twinaxial kablo
  * SFP28 tipi 10GbE özellikli doğrudan takmak twinaxial kablo

* 1GbE ağ bağlantı noktaları küme yönetimi trafiği için kullanılır. Küme yapılandırması için fiziksel olarak ayrı bir ağ oluşturmak istiyorsanız küme oluşturulurken **1Gb mgmt ağ** kullan seçeneğini işaretleyin [(yönetim ağını Yapılandırma'da](fxt-cluster-create.md#configure-the-management-network)açıklanmıştır). Bağlantı noktalarını desteklenen kablolar listesinde açıklandığı gibi standart Cat5 veya daha iyi kabloyla kablolayın.

  Tüm trafik için yüksek hızlı bağlantı noktalarını kullanmayı planlıyorsanız, 1GbE bağlantı noktalarını kablosuz bırakabilirsiniz. Varsayılan olarak, daha yüksek hızlı bir veri bağlantı noktası varsa 1GbE ağ bağlantı noktaları kullanılmaz.  

### <a name="idrac-port"></a>iDRAC bağlantı noktası  

iDRAC etiketli bağlantı noktası, donanım yönetimi ve izleme için kullanılan bir uzaktan erişim denetleyicisi ile iletişim sağlayan bir 1Gb bağlantısıdır. FXT yazılımı, sorun giderme ve kurtarma için bu denetleyiciile Akıllı Platform Yönetim Arabirimi 'ni (IPMI) kullanır. Bu bağlantı noktası üzerinden donanım izlemek için yerleşik [iDRAC arabirimini](https://www.dell.com/support/manuals/idrac9-lifecycle-controller-v3.30.30.30/idrac_3.30.30.30_ug/) kullanabilirsiniz. iDRAC ve IPMI erişimi varsayılan olarak etkinleştirilir. 

> [!Note]
> iDRAC bağlantı noktası işletim sistemini atlayabilir ve düğümüzerindeki donanımla doğrudan etkileşimde bulunabilir. 

iDRAC bağlantı noktasını bağlarken ve yapılandırırken bu güvenlik stratejilerini kullanın:

* Yalnızca iDRAC bağlantı noktalarını kümeye erişmek için kullanılan veri ağından fiziksel olarak ayrılmış bir ağa bağlayın.
* Her düğümde güvenli bir iDRAC yönetici parolası ayarlayın. Donanımı etkinleştirmek için bu parolayı ayarlamanız gerekir - [Donanım parolalarını](fxt-node-password.md)ayarla'daki yönergeleri izleyin.
* Varsayılan iDRAC bağlantı noktası yapılandırması, IP adresi ataması için DHCP ve IPv4 kullanır. DHCP ortamınızın iyi korunduğundan ve bağlantıların DHCP istemcileri ile DHCP sunucusu arasında kısıtlandığından emin olun. (Küme denetim paneli, kümeyi oluşturduktan sonra düğümlerin adres yapılandırma yöntemini değiştirmek için ayarlar içerir.)
* iDRAC bağlantı noktasını, iDRAC/IPMI ağ trafiğini özel RJ-45 bağlantı noktasına sınırlayan "özel mod" (varsayılan) olarak bırakın.

iDRAC bağlantı noktası yüksek hızlı ağ bağlantısı gerektirmez.
  
### <a name="serial-port-only-when-necessary"></a>Seri bağlantı noktası (yalnızca gerektiğinde)

Bazı durumlarda, Microsoft Service ve Support, bir sorunu tanılamak için bir terminali düğümün seri bağlantı noktasına bağlamanızı söyleyebilir.  

Konsolu takmak için:

1. FXT Edge Filer düğümünün arkasındaki seri (COM1) bağlantı noktasını bulun.
1. Seri bağlantı noktasını ANSI-115200-8N1 için yapılandırılmış bir terminale bağlamak için null modem kablosu kullanın.
1. Konsolda oturum açın ve destek personeli tarafından yönlendirilen diğer adımları atın.

## <a name="route-cables-in-the-cable-management-arm-cma"></a>Kablo yönetim kolundaki rota kabloları (CMA)

Her Azure FXT Edge Filer düğümü isteğe bağlı kablo yönetim koluyla birlikte gelir. CMA kablo yönlendirmeyi kolaylaştırır ve kabloların kesilmesine gerek kalmadan kasanın arkasına daha kolay erişim sağlar. 

Kabloları CMA üzerinden yönlendirmek için aşağıdaki talimatları izleyin: 

1. Sağlanan kravat sargılarını kullanarak, kabloları bitişik sistemlere müdahale etmemeleri için sepetlere girerken ve çıkarken bir araya getirin (1).
1. CMA servis konumundaolduğu için, kablo paketini iç ve dış sepetlere yönlendirin (2).
1. Kabloları sabitlemek için sepetlerin her iki ucundaki önceden yüklenmiş kanca ve döngü kayışlarını kullanın (3).
1. CMA'yı tepside tekrar yerine yerleştirin (4).
1. Durum göstergesi kablosunu sistemin arkasına töhalive CMA üzerinden yönlendirme yaparak kabloyu sabitle. Kablonun diğer ucunu dış CMA sepetinin köşesine takın (5). 

   > [!CAUTION]
   > Çıkıntılı kablolardan gelebilecek olası hasarları önlemek için, bu kabloyu CMA üzerinden yönlendirmeden sonra durum göstergesi kablosundaki gevşekliği sabitlayın. 

![Kabloları yüklü CMA İllüstrasyon](media/fxt-install/cma-cabling-400.png)

> [!NOTE]
>  CMA'yı yüklemediyseniz, sisteminizin arka sideki kabloları yönlendirmek için ray kitinde bulunan iki kanca ve döngü kayışını kullanın.
> 
>  1. Her iki raf flanşının iç taraflarında dış CMA braketlerini bulun.
>  2. Kabloları yavaşça paketleyin ve sistem konektörlerinden sol ve sağ kenarlara doğru çekin.
>  3. Kablo demetlerini sabitlemek için kanca ve döngü kayışlarını sistemin her iki tarafındaki dış CMA braketleri üzerindeki takımlı yuvalardan geçirin.
> 
>     ![CMA olmadan yönlendirilen kablolar](media/fxt-install/fxt-route-cables-no-cma-400.png)

## <a name="about-ip-address-requirements"></a>IP adresi gereksinimleri hakkında

Azure FXT Edge Filer karma depolama önbelleğindeki donanım düğümleri için IP adresleri küme yazılımı tarafından yönetilir.

Her düğüm en az bir IP adresi gerektirir, ancak düğümler kümeye eklendiğinde veya kümeden kaldırıldığında düğüm adresleri atanır. 

Gereken toplam IP adresi sayısı, önbelleğinizi oluşturan düğüm sayısına bağlıdır. 

Düğümler yüklendikten sonra Kontrol Masası yazılımını kullanarak IP adres aralığını yapılandırın. Daha fazla bilgi edinmek [için küme için bilgi topla'yı](fxt-cluster-create.md#gather-information-for-the-cluster)okuyun.  

## <a name="connect-power-cables"></a>Güç kablolarını bağlayın

Her Azure FXT Edge Filer düğümü iki güç kaynağı birimi (PSU) kullanır. 

> [!TIP] 
> İki gereksiz PSU'dan yararlanmak için, her AC güç kablosunu bağımsız bir dal devresi üzerindeki bir güç dağıtım ünitesine (PDU) takın.  
> 
> Ekstra koruma için PSU'lara güç sağlamak için bir UPS kullanabilirsiniz. 

1. Kasadaki PSU'lara dahil güç kablolarını bağlayın. Kabloların ve PSU'ların tamamen oturduğundan emin olun. 
1. Güç kablolarını ekipman rafındaki güç dağıtım ünitelerine takın. Mümkünse, iki kablo için iki ayrı güç kaynağı kullanın. 
 
### <a name="power-on-an-azure-fxt-edge-filer-node"></a>Azure FXT Kenar Filer düğümünde güç

Düğümü açmak için sistemin önündeki güç düğmesine basın. Düğme sağ taraftaki kontrol panelinde. 

### <a name="power-off-an-azure-fxt-edge-filer-node"></a>Azure FXT Edge Filer düğümükapalı güç

Güç düğmesi, test sırasında ve kümeye eklemeden önce sistemi kapatmak için kullanılabilir. Ancak, bir Azure FXT Kenar Filer düğümü kümenin bir parçası olarak kullanımda kaldıktan sonra, donanımı kapatmak için küme denetim paneli yazılımını kullanmanız gerekir. Ayrıntılar [için Azure FXT Edge Filer donanımı](fxt-power-off.md) güvenli bir şekilde nasıl kapatabilirsiniz'ı okuyun. 

## <a name="next-steps"></a>Sonraki adımlar

Donanımı kablolamayı bitirdikten sonra, düğümlerin her birinde güç ve kök parolalarını ayarlayarak onları başlatma. 
> [!div class="nextstepaction"]
> [İlk parolaları ayarlama](fxt-node-password.md)
