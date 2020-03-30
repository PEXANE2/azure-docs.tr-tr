---
title: StorSimple 8000 serisi cihaz yapılandırmasını değiştirin | Microsoft Dokümanlar
description: Zaten dağıtılmış olan bir StorSimple aygıtını yeniden yapılandırmak için StorSimple Device Manager hizmetinin nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/28/2017
ms.author: alkohli
ms.openlocfilehash: 774f5a73a5fc30352698c0af0c279fbbe488c480
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267696"
---
# <a name="use-the-storsimple-device-manager-service-to-modify-your-storsimple-device-configuration"></a>StorSimple aygıt yapılandırmanızı değiştirmek için StorSimple Aygıt Yöneticisi hizmetini kullanın

## <a name="overview"></a>Genel Bakış

**Ayarlar** bıçaktaki Azure portalı **Aygıt ayarları** bölümü, StorSimple Aygıt Yöneticisi hizmeti tarafından yönetilen bir StorSimple aygıtında yeniden yapılandırabileceğiniz tüm aygıt parametrelerini içerir. Bu öğretici, aşağıdaki aygıt düzeyindeki görevleri gerçekleştirmek için **Ayarlar** bıçaklarını nasıl kullanabileceğinizi açıklar:

* Cihaz dostu adı değiştirme
* Aygıt saat ayarlarını değiştirme
* İkincil bir DNS atama
* Ağ arabirimlerini değiştirme
* IP'leri değiştirme veya yeniden atama

## <a name="modify-device-friendly-name"></a>Cihaz dostu adı değiştirme

Azure portalını kullanarak cihaz adını değiştirebilir ve seçtiğiniz benzersiz bir kolay ad atayabilirsiniz. Cihaz dostu adı değiştirmek için cihazınızdaki **Genel ayarlar** bıçağını kullanın. Dost adı herhangi bir karakter içerebilir ve en fazla 64 karakter uzunluğunda olabilir.

> [!NOTE] 
> Aygıt kurulumu tamamlanmadan önce yalnızca Azure portalındaki aygıt adını değiştirebilirsiniz. Minimum aygıt kurulumu tamamlandıktan sonra aygıt adını değiştiremezsiniz.

![Genel ayarlarda aygıt adı](./media/storsimple-8000-modify-device-config/modify-general-settings3.png)

StorSimple Device Manager hizmetine bağlı bir StorSimple aygıtına varsayılan bir ad atanır. Varsayılan ad genellikle aygıtın seri numarasını yansıtır. Örneğin, 8600-SHX0991003G44HT gibi 15 karakter uzunluğunda varsayılan aygıt adı aşağıdakileri gösterir:

* **8600** – Cihaz modelini gösterir.
* **SHX** – Üretim sahasını gösterir.
* **0991003** - Belirli bir ürünü gösterir.
* **G44HT**- Benzersiz seri numaraları oluşturmak için son 5 basamak artımlı. Bu sıralı bir küme olmayabilir.

## <a name="modify-device-description"></a>Aygıt açıklamasını değiştirme

Aygıt açıklamasını değiştirmek için cihazınızdaki **Genel ayarlar** bıçağını kullanın.

![Genel ayarlarda aygıt açıklaması](./media/storsimple-8000-modify-device-config/modify-general-settings4.png)

Aygıt açıklaması genellikle cihazın sahibini ve fiziksel konumunu belirlemeye yardımcı olur. Açıklama alanı 256'dan az karakter içermelidir.

## <a name="modify-time-settings"></a>Zaman ayarlarını değiştirme

Cihazınızın, bulut depolama hizmeti sağlayıcınızla kimlik doğrulaması yapabilmesi için zamanı eşitlemesi gerekir. Cihazınızdaki **Genel ayarlar** bıçağını kullanarak cihaz saat ayarlarını değiştirin.

![Genel ayarlarda aygıt açıklaması](./media/storsimple-8000-modify-device-config/modify-general-settings2.png)

 Açılan listeden saat diliminizi seçin. En fazla iki Ağ Süresi Protokolü (NTP) sunucusu belirtebilirsiniz:

 - **Birincil NTP sunucusu** - Yapılandırma gereklidir ve cihazınızı yapılandırmak için StorSimple için Windows PowerShell kullandığınızda belirtilir. Varsayılan Windows Server **time.windows.com** NTP sunucunuz olarak belirtebilirsiniz. Azure portalı üzerinden birincil NTP sunucu yapılandırmasını görüntüleyebilirsiniz, ancak değiştirmek için Windows PowerShell arabirimini kullanmanız gerekir. Cihazınızın `Set-HcsNTPClientServerAddress` Birincil NTP sunucusunu değiştirmek için cmdlet'i kullanın. Daha fazla bilgi için [Set-HcsNTPClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) cmdlet için sözdizimine gidin.

- **İkincil NTP sunucusu** - Yapılandırma isteğe bağlıdır. Portalı ikincil bir NTP sunucusunu yapılandırmak için kullanabilirsiniz.

NTP sunucusunu yapılandırırken, ağınızın NTP trafiğinin veri merkezinizden Internet'e geçmesine izin verdiğinden emin olun. Ortak bir NTP sunucusu belirtirken, ağ güvenlik duvarlarınızın ve diğer güvenlik aygıtlarınızın NTP trafiğinin dış ağa gidip gelmesine izin verecek şekilde yapılandırıldığından emin olmalısınız. Çift yönlü NTP trafiğine izin verilmiyorsa, dahili bir NTP sunucusu kullanmanız gerekir (Windows etki alanı denetleyicisi bu işlevi sağlar). Aygıtınız zamanı eşitleyemiyorsa, bulut depolama sağlayıcınızla iletişim kuramayabilir.

Ortak NTP sunucularının listesini görmek için [NTP Sunucular Web'e](https://support.ntp.org/bin/view/Servers/WebHome)gidin.

### <a name="what-happens-if-the-device-is-deployed-in-a-different-time-zone"></a>Aygıt farklı bir saat diliminde dağıtılırsa ne olur?

Aygıt farklı bir saat diliminde dağıtılırsa, aygıt saat dilimi değişir. Tüm yedekleme ilkelerinin aygıt saat dilimini kullandığı göz önüne alındığında, yedekleme ilkeleri yeni saat dilimine göre otomatik olarak ayarlanır. Kullanıcı müdahalesi gerekli değildir.

## <a name="modify-dns-settings"></a>DNS ayarlarını değiştirme

Aygıtınız bulut depolama hizmeti sağlayıcınızla iletişim kurmaya çalıştığında bir DNS sunucusu kullanılır. Yapılandırılan DNS ayarlarını görüntülemek ve değiştirmek için cihazınızdaki **Ağ ayarları** bıçağını kullanın. 

![Ağ ayarlarında DNS ayarları](./media/storsimple-8000-modify-device-config/modify-network-settings1.png)

Yüksek kullanılabilirlik için, ilk aygıt dağıtımı sırasında hem birincil hem de ikincil DNS sunucularını yapılandırmanız gerekir.

**Birincil DNS sunucusu** - İlk kurulum sırasında Birincil DNS sunucusunu belirtmek için StorSimple için Windows PowerShell'i kullanırsınız. Birincil DNS sunucusunu yalnızca Windows PowerShell arabirimi üzerinden yeniden yapılandırabilirsiniz. Cihazınızın `Set-HcsDNSClientServerAddress` birincil DNS sunucusunu değiştirmek için cmdlet'i kullanın. Daha fazla bilgi için [Set-HcsDNSClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) cmdlet için sözdizimine gidin.

**İkincil DNS sunucusu** - İkincil DNS `Set-HcsDNSClientServerAddress` sunucusunu değiştirmek için, aygıtın Windows PowerShell arabirimindeki cmdlet'i veya Azure portalındaki StorSimple cihazınızın **Ağ ayarları** bıçağını kullanın.

Azure portalındaki ikincil DNS sunucusunu değiştirmek için aşağıdaki adımları gerçekleştirin.

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. Aygıtlar listesinden cihazınızı seçin ve tıklatın.

2. **Ayarlar** bıçağında, **Aygıt ayarları > Ağ'a**gidin. Bu, **Ağ ayarları** bıçağını açar. **DNS ayarları** döşemesi'ni tıklatın. İkincil DNS sunucusu IP adresini değiştirin.

    ![İkincil DNS sunucusu IP adresini değiştirme](./media/storsimple-8000-modify-device-config/modify-secondary-dns1.png)

4. Komut çubuğundan **Kaydet'i** tıklatın ve onay istendiğinde **Tamam'ı**tıklatın.

    ![Değişiklikleri kaydetme ve onaylama](./media/storsimple-8000-modify-device-config/modify-secondary-dns-2.png)



## <a name="modify-network-interfaces"></a>Ağ arabirimlerini değiştirme

Cihazınızda dört tane 1 GbE ve ikisi 10 GbE olmak üzere altı cihaz ağ arabirimi vardır. Bu arabirimler DATA 0 – DATA 5 olarak etiketlenir. DATA 0, DATA 1, DATA 4 ve DATA 5 1 GbE, DATA 2 ve DATA 3 ise 10 GbE ağ arabirimleridir.

Kullanılacak arabirimlerin her birini yapılandırmak için **Ağ ayarları** bıçaklarını kullanın.

![Ağ ayarları üzerinden ağ arabirimlerini yapılandırma](./media/storsimple-8000-modify-device-config/modify-network-settings3.png)

Yüksek kullanılabilirlik sağlamak için cihazınızda en az iki iSCSI arabirimi ve iki bulut özellikli arabiriminiz olmasını öneririz. Kullanılmayan arabirimlerin devre dışı bırakılmasını öneririz, ancak gerektirmez.

Her ağ arabirimi için aşağıdaki parametreler görüntülenir:

* **Hız** – Kullanıcı tarafından yapılandırılabilen bir parametre değil. DATA 0, DATA 1, DATA 4 ve DATA 5 her zaman 1 GbE, DATA 2 ve DATA 3 ise 10 GbE arabirimidir.
  
  > [!NOTE]
  > Hız ve çift yönlü her zaman otomatik olarak görüşülür. Jumbo çerçeveler desteklenmez.
  
* **Arabirim durumu** – Arabirim etkinleştirilebilir veya devre dışı bırakılır. Etkinleştirilirse, aygıt arabirimi kullanmaya çalışır. Yalnızca ağa bağlı ve kullanılan arabirimlerin etkinleştirilmesini öneririz. Kullanmadığınız arabirimleri devre dışı bırakma.
* **Arabirim türü** – Bu parametre, iSCSI trafiğini bulut depolama trafiğinden yalıtmanızı sağlar. Bu parametre aşağıdakilerden biri olabilir:
  
  * **Bulut etkin** – etkinleştirildiğinde, aygıt bulutla iletişim kurmak için bu arabirimi kullanır.
  * **iSCSI etkin** – etkinleştirildiğinde, aygıt iSCSI ana bilgisayarile iletişim kurmak için bu arabirimi kullanır.
    
    iSCSI trafiğini bulut depolama trafiğinden yalıtmanızı öneririz. Ayrıca, ana bilgisayarınızın aygıtınızla aynı alt ağ da varsa, bir ağ geçidi atamanız gerekmediğini unutmayın; ancak, ana bilgisayarınız cihazınızdan farklı bir alt ağdaysa, bir ağ geçidi atamanız gerekir.
* **IP adresi** – Ağ arabirimlerinden herhangi birini yapılandırdığınızda, sanal bir IP (VIP) yapılandırmanız gerekir. Bu IPv4 veya IPv6 veya her ikisi de olabilir. Aygıt ağ arabirimleri için Hem IPv4 hem de IPv6 adres aileleri desteklenir. IPv4 kullanırken, nokta ondalık gösteriminde 32 bit IP adresi *(xxx.xxx.xxx.xxx)* belirtin. IPv6 kullanırken, 4 basamaklı bir önek sağlamanız yeterlidir ve bu önek bağlı olarak cihazınızın ağ arabirimi için otomatik olarak 128 bitlik bir adres oluşturulur.
* **Subnet** – Bu alt ağ maskesi anlamına gelir ve Windows PowerShell arabirimi üzerinden yapılandırılır.
* **Ağ Geçidi** – Bu, aynı IP adresi alanı (alt ağ) içinde olmayan düğümlerle iletişim kurmaya çalıştığında bu arabirim tarafından kullanılması gereken varsayılan ağ geçididir. Varsayılan ağ geçidi, alt ağ maskesi tarafından belirlendiği gibi, arabirim IP adresiyle aynı adres alanında (alt ağ) olmalıdır.
* **Sabit IP adresi** – Bu alan yalnızca DATA 0 arabirimini yapılandırırken kullanılabilir. Güncelleştirmeler veya aygıtta sorun giderme gibi işlemler için doğrudan aygıt denetleyicisine bağlanmanız gerekebilir. Sabit IP adresi, cihazınızdaki etkin ve pasif denetleyiciye erişmek için kullanılabilir.

> [!NOTE]
> * Düzgün çalışmasını sağlamak için, her aygıt arabiriminin bağlı olduğu anahtarüzerindeki arabirim hızını ve çift yönlü cizimi doğrulayın. Anahtar arabirimleri Gigabit Ethernet (1000 Mbps) için anlaşma yapmalı veya yapılandırılmalı ve tam çift yönlü olmalıdır. Daha yavaş hızlarda veya yarı çift yönlü olarak çalışan arabirimler performans sorunlarına neden olur.
> * Kesintileri ve kapalı kalma süresini en aza indirmek için, aygıtınızın iSCSI ağ arabiriminin bağlanacağı anahtar bağlantı noktalarının her birinde portfast'ı etkinleştirmenizi öneririz. Bu, bir arıza durumunda ağ bağlantısının hızlı bir şekilde kurulmasını sağlayacaktır.

### <a name="configure-data-0"></a>VERİlerİ yapılandırma 0

DATA 0 varsayılan olarak bulut etkindir. DATA 0'ı yapılandırırken, her denetleyici için bir tane olmak üzere iki sabit IP adresini yapılandırmanız gerekir. Bu sabit IP adresleri aygıt denetleyicilerine doğrudan erişmek için kullanılabilir ve aygıta güncelleştirmeler yüklediğinizde, çöp toplamanın düzgün çalışması için veya sorun giderme amacıyla denetleyicilere erişdiğinizde yararlıdır.

Sabit IP denetleyicilerini DATA 0 ayarları bıçağı ile yeniden yapılandırabilirsiniz.

![Ağ arabirimini yapılandırma - DATA 0](./media/storsimple-8000-modify-device-config/modify-network-settings2.png)

> [!NOTE]
> Denetleyiciiçin sabit IP adresleri, aygıtgüncelleştirmelerine servis yapmak ve düzgün çalışması için alan ıslah algoritmaları (çöp toplama) için kullanılır. Bu nedenle, sabit IP’lerin yönlendirilebilir ve İnternet’e bağlanabilir olması gerekir.

### <a name="configure-data-1---data-5"></a>VERİLERİ YAPıLANDıRMA 1 - DATA 5

DATA 1 - DATA 5 ağ arabirimleri için, aşağıdaki ekran görüntüsünde gösterildiği gibi tüm ağ ayarlarını yapılandırabilirsiniz:

![Ağ arayüzlerini yapılandırma DATA 1 - DATA 5](./media/storsimple-8000-modify-device-config/modify-network-settings4.png)


## <a name="swap-or-reassign-ips"></a>IP'leri değiştirme veya yeniden atama

Şu anda, denetleyicideki herhangi bir ağ arabirimi kullanılıyorsa (ağdaki aynı aygıt veya başka bir aygıt tarafından) bir VIP atanırsa, denetleyici başarısız olur. Bir aygıt ağı arabirimi için VIP'leri değiştirir veya yeniden atarsanız, yinelenen bir IP durumu oluşturabileceğiniz için uygun bir yordam izlemeniz gerekir.

VIP'leri ağ arabirimlerinden herhangi biri için değiştirmek veya yeniden atamak için aşağıdaki adımları gerçekleştirin:

#### <a name="to-reassign-ips"></a>IP'leri yeniden atamak için

1. Her iki arabirim için IP adresini temizleyin.
2. IP adresleri temizlendikten sonra, yeni IP adreslerini ilgili arabirimlere atayın.

## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple aygıtınız için MPIO'yi nasıl yapılandırıştırıyarız öğrenin.](storsimple-8000-configure-mpio-windows-server.md)
* [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini](storsimple-8000-manager-service-administration.md)nasıl kullanacağınızı öğrenin.

