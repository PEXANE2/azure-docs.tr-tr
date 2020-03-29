---
title: Azure'da yük dengeleme hizmetlerini kullanma | Microsoft Dokümanlar
description: 'Bu öğretici, Azure yük dengeleme portföyünü kullanarak nasıl bir senaryo oluşturabileceğinizi gösterir: Trafik Yöneticisi, Uygulama Ağ Geçidi ve Yük Dengeleyici.'
services: traffic-manager
documentationcenter: ''
author: rohinkoul
manager: kumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2016
ms.author: rohink
ms.openlocfilehash: b77248813463f51d4bd2c5186e421aec43ffaf52
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76939218"
---
# <a name="using-load-balancing-services-in-azure"></a>Azure’daki yük dengeleme hizmetlerini kullanma

## <a name="introduction"></a>Giriş

Microsoft Azure, ağ trafiğinin nasıl dağıtıldığını ve yük dengeli olarak dağıtıldığını yönetmek için birden çok hizmet sağlar. En uygun çözümü oluşturmak için bu hizmetleri tek tek kullanabilir veya ihtiyaçlarınıza bağlı olarak yöntemlerini birleştirebilirsiniz.

Bu öğreticide, öncelikle bir müşteri kullanım örneğini tanımlıyoruz ve aşağıdaki Azure yük dengeleme portföyü olan Trafik Yöneticisi, Uygulama Ağ Geçidi ve Yük Dengeleyicisi'ni kullanarak nasıl daha sağlam ve performanslı hale getirilebildiğini görüyoruz. Daha sonra, coğrafi olarak gereksiz olan, trafiği VM'lere dağıtan ve farklı istek türlerini yönetmenize yardımcı olan bir dağıtım oluşturmak için adım adım yönergeler sağlarız.

Kavramsal düzeyde, bu hizmetlerin her biri yük dengeleme hiyerarşisinde ayrı bir rol oynar.

* **Trafik Yöneticisi** küresel DNS yük dengeleme sağlar. Gelen DNS isteklerini bakar ve müşterinin seçtiği yönlendirme ilkesine uygun olarak sağlıklı bir bitiş noktasıyla yanıt verir. Yönlendirme yöntemleri için seçenekler şunlardır:
  * İstekvereni gecikme sonu açısından en yakın uç noktaya göndermek için performans yönlendirmesi.
  * Tüm trafiği yedekleme olarak diğer uç noktalarla birlikte bir bitiş noktasına yönlendirmek için öncelik yönlendirmesi.
  * Trafiği her bitiş noktasına atanan ağırlıklandırmaya göre dağıtan ağırlıklı yuvarlak-robin yönlendirme.
  * Trafiğin kullanıcının coğrafi konumuna bağlı olarak uygulama uç noktalarına dağıtılması için coğrafya tabanlı yönlendirme.
  * Trafiğin kullanıcının alt ağına (IP adresi aralığı) dayalı olarak uygulama uç noktalarınıza dağıtılması için alt ağ tabanlı yönlendirme.
  * Tek bir DNS yanıtında birden fazla uygulama uç noktasının IP adreslerini göndermenizi sağlayan Çok Değerli yönlendirme.

  İstemci doğrudan Trafik Yöneticisi tarafından döndürülen bitiş noktasına bağlanır. Azure Trafik Yöneticisi, bir bitiş noktasının sağlıksız olduğunu algılar ve istemcileri başka bir sağlıklı örneğe yönlendirir. Hizmet hakkında daha fazla bilgi edinmek için [Azure Trafik Yöneticisi belgelerine](traffic-manager-overview.md) bakın.
* **Uygulama Ağ Geçidi,** uygulamanız için çeşitli Katman 7 yük dengeleme özellikleri sunan uygulama dağıtım denetleyicisi (ADC) hizmeti sağlar. Müşterilerin, CPU yoğun SSL sonlandırmaişlemini uygulama ağ geçidine boşaltarak web farm üretkenliğini optimize etmesine olanak tanır. Diğer Katman 7 yönlendirme yetenekleri arasında gelen trafiğin round-robin dağıtımı, çerez tabanlı oturum yakınlığı, URL yol tabanlı yönlendirme ve tek bir uygulama ağ geçidinin arkasında birden çok web sitesini barındırma olanağı yer almaktadır. Uygulama Ağ Geçidi, Internet'e bakan bir ağ geçidi, yalnızca dahili ağ geçidi veya her ikisinin birleşimi olarak yapılandırılabilir. Uygulama Ağ Geçidi tamamen Azure tarafından yönetilir, ölçeklenebilir ve yüksek oranda kullanılabilir. Daha iyi yönetilebilirlik için zengin tanılama ve günlüğe kaydetme özellikleri sağlar.
* **Yük Dengeleyici,** tüm UDP ve TCP protokolleri için yüksek performanslı, düşük gecikmeli Katman 4 yük dengeleme hizmetleri sağlayan Azure SDN yığınının ayrılmaz bir parçasıdır. Gelen ve giden bağlantıları yönetir. Yük dengeleme özelliğine sahip genel ve şirket içi yük uç noktaları yapılandırıp TCP ve HTTP hizmet durumu yoklama seçeneklerini kullanarak gelen bağlantıları arka uç havuz hedefleriyle eşleyebilir ve hizmet kullanılabilirliği sağlayabilirsiniz.

## <a name="scenario"></a>Senaryo

Bu örnek senaryoda, iki tür içeriğe hizmet veren basit bir web sitesi kullanıyoruz: resimler ve dinamik olarak işlenmiş web sayfaları. Web sitesi coğrafi olarak gereksiz olmalıdır ve kullanıcılarına en yakın (en düşük gecikme) konumdan hizmet etmelidir. Uygulama geliştiricisi, desen/görüntülerle/* eşleşen tüm URL'lerin, web çiftliğinin geri kalanından farklı özel bir VM havuzundan sunulduğuna karar vermiştir.

Ayrıca, dinamik içeriğe hizmet veren varsayılan VM havuzunun yüksek kullanılabilirlik kümesinde barındırılan bir arka uç veritabanıyla konuşması gerekir. Dağıtımın tamamı Azure Kaynak Yöneticisi aracılığıyla ayarlanır.

Trafik Yöneticisi, Uygulama Ağ Geçidi ve Yük Dengeleyici kullanarak bu web sitesi bu tasarım hedeflerine ulaşmak için izin verir:

* **Çok coğrafi artıklık**: Bir bölge çökerse, Trafik Yöneticisi, uygulama sahibinin herhangi bir müdahalesi olmaksızın trafiği sorunsuz bir şekilde en yakın bölgeye yönlendirir.
* **Azaltılmış gecikme :** Trafik Yöneticisi müşteriyi otomatik olarak en yakın bölgeye yönlendirdiği için, müşteri web sayfası içeriğini talep ederken daha düşük gecikme gecikmesi yle karşılar.
* **Bağımsız ölçeklenebilirlik**: Web uygulaması iş yükü içerik türüne göre ayrıldığından, uygulama sahibi istek iş yüklerini birbirinden bağımsız ölçeklendirebilir. Uygulama Ağ Geçidi, belirtilen kurallara ve uygulamanın durumuna göre trafiğin doğru havuzlara yönlendirilmesini sağlar.
* **Dahili yük dengelemesi**: Yük Dengeleyicisi yüksek kullanılabilirlik kümesinin önünde olduğundan, yalnızca bir veritabanı için etkin ve sağlıklı bitiş noktası uygulamaya maruz kalır. Ayrıca, bir veritabanı yöneticisi, ön uç uygulamasından bağımsız olarak kümeye etkin ve pasif yinelemeler dağıtarak iş yükünü en iyi duruma getirebilir. Load Balancer, yüksek kullanılabilirlik kümesine bağlantılar sağlar ve yalnızca sağlıklı veritabanlarının bağlantı istekleri almasını sağlar.

Aşağıdaki diyagram, bu senaryonun mimarisini gösterir:

![Yük dengeleme mimarisinin diyagramı](./media/traffic-manager-load-balancing-azure/scenario-diagram.png)

> [!NOTE]
> Bu örnek, Azure'un sunduğu yük dengeleme hizmetlerinin birçok olası yapılandırmalarından yalnızca biridir. Trafik Yöneticisi, Uygulama Ağ Geçidi ve Yük Dengeleyicisi karıştırılabilir ve yük dengeleme ihtiyaçlarınıza en uygun şekilde eşlenebilir. Örneğin, SSL boşaltma veya Katman 7 işleme gerekli değilse, Yük Dengeleyici Uygulama Ağ Geçidi yerine kullanılabilir.

## <a name="setting-up-the-load-balancing-stack"></a>Yük dengeleme yığınını ayarlama

### <a name="step-1-create-a-traffic-manager-profile"></a>Adım 1: Trafik Yöneticisi profili oluşturma

1. Azure portalında, **kaynak** > **Oluşturma** > **Trafik Yöneticisi Profili** > **Oluştur'u**tıklatın.
2. Aşağıdaki temel bilgileri girin:

   * **Adı**: Trafik Yöneticisi profilinize DNS öneki adı verin.
   * **Yönlendirme yöntemi**: Trafik yönlendirme yöntemi ilkesini seçin. Yöntemler hakkında daha fazla bilgi için [Trafik Yöneticisi trafik yönlendirme yöntemleri hakkında](traffic-manager-routing-methods.md)bilgi.
   * **Abonelik**: Profili içeren aboneliği seçin.
   * **Kaynak grubu**: Profili içeren kaynak grubunu seçin. Yeni veya varolan bir kaynak grubu olabilir.
   * **Kaynak grubu konumu**: Trafik Yöneticisi hizmeti geneldir ve bir konuma bağlı değildir. Ancak, Trafik Yöneticisi profiliyle ilişkili meta verilerin bulunduğu grup için bir bölge belirtmeniz gerekir. Bu konumun profilin çalışma zamanı kullanılabilirliği üzerinde hiçbir etkisi yoktur.

3. Trafik Yöneticisi profilini oluşturmak için **Oluştur'u** tıklatın.

   !["Trafik Yöneticisi Oluştur" bıçağı](./media/traffic-manager-load-balancing-azure/s1-create-tm-blade.png)

### <a name="step-2-create-the-application-gateways"></a>Adım 2: Uygulama ağ geçitlerini oluşturma

1. Azure portalında, sol bölmede, **kaynak** > **Ağ** > **Uygulaması Ağ Geçidi**Oluştur'u tıklatın.
2. Uygulama ağ geçidi hakkında aşağıdaki temel bilgileri girin:

   * **Adı**: Uygulama ağ geçidinin adı.
   * **SKU boyutu**: Küçük, Orta veya Büyük olarak kullanılabilen uygulama ağ geçidinin boyutu.
   * **Örnek sayısı**: Örnek sayısı, 2 ile 10 arasında bir değer.
   * **Kaynak grubu**: Uygulama ağ geçidini tutan kaynak grubu. Varolan bir kaynak grubu veya yeni bir kaynak grubu olabilir.
   * **Konum**: Kaynak grubuyla aynı konumolan uygulama ağ geçidinin bulunduğu bölge. Sanal ağ ve genel IP ağ geçidi yle aynı konumda olması gerektiğinden, konum önemlidir.
3. **Tamam**'a tıklayın.
4. Uygulama ağ geçidi için sanal ağı, alt ağı, ön uç IP'sini ve dinleyici yapılandırmalarını tanımlayın. Bu senaryoda, ön uç IP **adresi,** daha sonra Trafik Yöneticisi profiline bir bitiş noktası olarak eklenmesini sağlayan Geneldir.
5. Dinleyiciyi aşağıdaki seçeneklerden biriyle yapılandırın:
    * HTTP kullanıyorsanız, yapılandırmak için hiçbir şey yoktur. **Tamam**'a tıklayın.
    * HTTPS kullanıyorsanız, daha fazla yapılandırma gereklidir. 9. adımdan başlayarak [bir uygulama ağ geçidi oluşturma'ya](../application-gateway/application-gateway-create-gateway-portal.md)bakın. Yapılandırmayı tamamladığınızda **Tamam'ı**tıklatın.

#### <a name="configure-url-routing-for-application-gateways"></a>Uygulama ağ geçitleri için URL yönlendirmeyi yapılandırma

Bir arka uç havuzu seçtiğinizde, yol tabanlı bir kuralla yapılandırılan bir uygulama ağ geçidi, round-robin dağıtımına ek olarak istek URL'sinin yol deseni alır. Bu senaryoda, herhangi bir URL'yi "/images/\*" ile görüntü sunucusu havuzuna yönlendirmek için yol tabanlı bir kural ekliyoruz. Bir uygulama ağ geçidi için URL yol tabanlı yönlendirme yapılandırma hakkında daha fazla bilgi için, [bir uygulama ağ geçidi için yol tabanlı bir kural oluştur'a](../application-gateway/application-gateway-create-url-route-portal.md)bakın.

![Uygulama Ağ Geçidi web katmanı diyagramı](./media/traffic-manager-load-balancing-azure/web-tier-diagram.png)

1. Kaynak grubunuzdan, önceki bölümde oluşturduğunuz uygulama ağ geçidi örneğine gidin.
2. **Ayarlar**altında, **Arka Uç havuzlarını**seçin ve ardından web katmanı arka uç havuzlarıyla ilişkilendirmek istediğiniz VM'leri eklemek için **Ekle'yi** seçin.
3. Arka uç havuzunun adını ve havuzda bulunan makinelerin tüm IP adreslerini girin. Bu senaryoda, sanal makinelerin iki arka uç sunucu havuzubağlıyoruz.

   ![Uygulama Ağ Geçidi "Arka uç havuzu ekle"](./media/traffic-manager-load-balancing-azure/s2-appgw-add-bepool.png)

4. Uygulama ağ geçidinin **Ayarları'nda** **Kurallar'ı**seçin ve ardından kural eklemek için **Yol tabanlı** düğmesini tıklatın.

   ![Uygulama Ağ Geçidi Kuralları "Yol tabanlı" düğmesi](./media/traffic-manager-load-balancing-azure/s2-appgw-add-pathrule.png)

5. Aşağıdaki bilgileri sağlayarak kuralı yapılandırın.

   Temel ayarlar:

   + **Adı**: Portalda erişilebilen kuralın dostu adıdır.
   + **Dinleyici**: Kural için kullanılan dinleyici.
   + **Varsayılan arka uç havuzu**: Varsayılan kuralla kullanılacak arka uç havuzu.
   + **Varsayılan HTTP ayarları**: Varsayılan kuralla kullanılacak HTTP ayarları.

   Yol tabanlı kurallar:

   + **Adı**: Yol tabanlı kuralın dostu adı.
   + **Yollar**: Trafiği iletmek için kullanılan yol kuralı.
   + **Arka uç havuzu**: Bu kuralla kullanılacak arka uç havuzu.
   + **HTTP Ayarı**: Bu kuralla kullanılacak HTTP ayarları.

   > [!IMPORTANT]
   > Yollar: Geçerli yollar "/" ile başlamalıdır. Joker "\*" " yalnızca sonunda izin verilir. Geçerli örnekler /xyz, /xyz\*, veya\*/xyz/ .

   ![Uygulama Ağ Geçidi "Yol tabanlı kural ekle" bıçak](./media/traffic-manager-load-balancing-azure/s2-appgw-pathrule-blade.png)

### <a name="step-3-add-application-gateways-to-the-traffic-manager-endpoints"></a>Adım 3: Trafik Yöneticisi uç noktalarına uygulama ağ geçitleri ekleme

Bu senaryoda, Trafik Yöneticisi farklı bölgelerde bulunan uygulama ağ geçitlerine (önceki adımlarda yapılandırıldığı gibi) bağlanır. Artık uygulama ağ geçitleri yapılandırıldığına göre, bir sonraki adım bunları Trafik Yöneticisi profilinize bağlamaktır.

1. Trafik Yöneticisi profilinizi açın. Bunu yapmak için kaynak grubunuza bakın veya **Tüm Kaynaklar'dan**Trafik Yöneticisi profilinin adını arayın.
2. Sol bölmede, **Bitiş Noktaları'nı**seçin ve ardından bitiş noktası eklemek için **Ekle'yi** tıklatın.

   ![Trafik Yöneticisi Uç Noktaları "Ekle" düğmesi](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint.png)

3. Aşağıdaki bilgileri girerek bir bitiş noktası oluşturun:

   * **Türü**: Yük dengesi için uç nokta türünü seçin. Bu senaryoda, azure **bitiş noktasını** seçin, çünkü daha önce yapılandırılan uygulama ağ geçidi örneklerine bağlıyoruz.
   * **Adı**: Bitiş noktasının adını girin.
   * **Hedef kaynak türü**: **Ortak IP adresini** seçin ve ardından Hedef **kaynağının**altında, daha önce yapılandırılan uygulama ağ geçidinin genel IP'sini seçin.

   ![Trafik Yöneticisi "Bitiş Noktası Ekle"](./media/traffic-manager-load-balancing-azure/s3-tm-add-endpoint-blade.png)

4. Artık kurulumunuzu Trafik Yöneticisi profilinizin DNS'si ile erişerek test edebilirsiniz (bu örnekte: TrafficManagerScenario.trafficmanager.net). İstekleri yeniden gönderebilir, farklı bölgelerde oluşturulan VM'leri ve web sunucularını gündeme getirebilir veya kaldırabilir ve kurulumunuzu test etmek için Trafik Yöneticisi profil ayarlarını değiştirebilirsiniz.

### <a name="step-4-create-a-load-balancer"></a>Adım 4: Yük dengeleyicisi oluşturma

Bu senaryoda, Load Balancer bağlantıları web katmanından yüksek kullanılabilirlik kümesi içindeki veritabanlarına dağıtır.

Yüksek kullanılabilirlik içeren veritabanı kümeniz SQL Server AlwaysOn kullanıyorsa, adım adım talimatlar için [bir veya daha fazla Always On Availability Group Dinleyicisini Yapılandırmaya](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) bakın.

Dahili yük dengeleyicisi yapılandırma hakkında daha fazla bilgi için Azure [portalında Dahili yük dengeleyicisi oluşturma](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)konusuna bakın.

1. Azure portalında, sol bölmede, **kaynak** > **Ağ** > **Yük bakiyesi**oluştur'u tıklatın.
2. Yük bakiyeniz için bir ad seçin.
3. **Türü** **İçe**ayarlayın ve yük dengeleyicisinin ikamet etmesi için uygun sanal ağı ve alt ağı seçin.
4. **IP adresi ataması**altında Dinamik **veya** **Statik'i**seçin.
5. **Kaynak grubu**altında, yük dengeleyicisi için kaynak grubunu seçin.
6. **Konum**altında, yük dengeleyicisi için uygun bölgeyi seçin.
7. Yük bakiyesini oluşturmak için **Oluştur'u** tıklatın.

#### <a name="connect-a-back-end-database-tier-to-the-load-balancer"></a>Bir arka uç veritabanı katmanını yük bakiyesine bağlama

1. Kaynak grubunuzdan, önceki adımlarda oluşturulan yük dengeleyicisini bulun.
2. **Ayarlar'ın** **altında, Arka Uç havuzları'nı**tıklatın ve sonra arka uç havuzu eklemek için **Ekle'yi** tıklatın.

   ![Yük Dengeleyici "Arka uç havuzu ekle"](./media/traffic-manager-load-balancing-azure/s4-ilb-add-bepool.png)

3. Arka uç havuzunun adını girin.
4. Tek tek makineleri veya arka uç havuzuna bir kullanılabilirlik kümesi ekleyin.

#### <a name="configure-a-probe"></a>Sondayı yapılandırma

1. Yük bakiyenizde, **Ayarlar**altında, **Problar'ı**seçin ve ardından sonda eklemek için **Ekle'yi** tıklatın.

   ![Yük Dengeleyici "Prob ekle"](./media/traffic-manager-load-balancing-azure/s4-ilb-add-probe.png)

2. Sondanın adını girin.
3. Sonda için **Protokol'u** seçin. Bir veritabanı için, http sondası yerine bir TCP sondası isteyebilirsiniz. Yük dengeleyici probları hakkında daha fazla bilgi edinmek için [yük dengeleyici problarını anlayın'a](../load-balancer/load-balancer-custom-probe-overview.md)bakın.
4. Sondaya erişmek için veritabanınızın **bağlantı noktasını** girin.
5. **Aralık**altında, uygulamayı ne sıklıkta sondalamak için belirtin.
6. **Sağlıksız eşik**altında, arka uç VM sağlıksız olarak kabul edilmesi için oluşması gereken sürekli sonda hatalarının sayısını belirtin.
7. Sondayı oluşturmak için **Tamam'ı** tıklatın.

#### <a name="configure-the-load-balancing-rules"></a>Yük dengeleme kurallarını yapılandırma

1. Yük **Settings** bakiyenizin Ayarları'nda, **Yük dengeleme kurallarını**seçin ve ardından kural oluşturmak için **Ekle'yi** tıklatın.
2. Yük dengeleme kuralı için **Adı** girin.
3. Yük dengeleyicisinin, **Protokol'ün**ve Bağlantı **Noktasının** **Frontend IP Adresini** seçin.
4. **Backend bağlantı noktasıaltında,** arka uç havuzunda kullanılacak bağlantı noktasını belirtin.
5. Kuralı uygulamak için önceki adımlarda oluşturulan **Arka Uç havuzunu** ve **Sonda'yı** seçin.
6. **Oturum kalıcılığı**altında, oturumların nasıl devam etmesini istediğinizi seçin.
7. **Boşta zaman zaman ları**altında, boşta kalma zaman aramadan önceki dakika sayısını belirtin.
8. **Kayan IP**altında, **Devre Dışı veya** **Etkin'i**seçin.
9. Kuralı oluşturmak için **Tamam**'a tıklayın.

### <a name="step-5-connect-web-tier-vms-to-the-load-balancer"></a>Adım 5: Web katmanındaki VM'leri yük dengeleyicisine bağlayın

Artık, herhangi bir veritabanı bağlantısı için web katmanı VM'lerinizde çalışan uygulamalarda IP adresini ve yük dengeleyici ön uç bağlantı noktasını yapılandırıyoruz. Bu yapılandırma, bu VM'lerde çalışan uygulamalara özgüdür. Hedef IP adresini ve bağlantı noktasını yapılandırmak için uygulama belgelerine bakın. Azure portalında ön uçTAKI IP adresini bulmak için **Yük bakiyesi ayarlarındaki**ön uç IP havuzuna gidin.

![Yük Dengeleyici "Frontend IP havuzu" navigasyon bölmesi](./media/traffic-manager-load-balancing-azure/s5-ilb-frontend-ippool.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Traffic Manager'a Genel Bakış](traffic-manager-overview.md)
* [Application Gateway’e genel bakış](../application-gateway/application-gateway-introduction.md)
* [Azure Yük Dengeleyicisi'ne genel bakış](../load-balancer/load-balancer-overview.md)
