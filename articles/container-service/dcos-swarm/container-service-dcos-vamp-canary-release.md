---
title: Kullanım DıŞı Azure DC/OS kümesinde vamp ile Canary sürümü
description: Vamp 'yi bir Azure Container Service DC/OS kümesine akıllı trafik filtreleme ve uygulama hizmetleri için kullanma
author: gggina
ms.service: container-service
ms.topic: conceptual
ms.date: 04/17/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: dfdf6e1f8edfb4dafaf93e62090ed51878f9b2aa
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82734833"
---
# <a name="deprecated-canary-release-microservices-with-vamp-on-an-azure-container-service-dcos-cluster"></a>Kullanım DıŞı Azure Container Service DC/OS kümesinde vamp ile mikro hizmetler yayın

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bu kılavuzda, bir DC/OS kümesi ile Azure Container Service vamp 'yi ayarladık. Vamp demo hizmeti "Sava" ' ı yayınlarız ve ardından akıllı trafik filtrelemesi uygulayarak Firefox ile hizmetin bir uyumsuzluğunun çözümlenme. 

> [!TIP] 
> Bu kılavuzda, vamp bir DC/OS kümesinde çalışır, ancak Orchestrator olarak Kubernetes ile vamp 'yi de kullanabilirsiniz.
>

## <a name="about-canary-releases-and-vamp"></a>Kanarlı yayınlar ve vamp hakkında


[Kanarya serbest bırakma](https://martinfowler.com/bliki/CanaryRelease.html) , Netflix, Facebook ve Spotify gibi yenilikçi kuruluşların benimsediği bir akıllı dağıtım stratejisidir. Sorunları azalttığı, güvenlik ağları sunan ve yeniliklerin arttığı için anlamlı bir yaklaşım. Öyleyse, neden tüm şirketler bunu kullanmıyor? Bir CI/CD işlem hattının, kanarary stratejileri içerecek şekilde genişletilmesi karmaşıklık sağlar ve kapsamlı DevOps bilgi ve deneyimi gerektirir. Bu, daha küçük şirketlerin ve kuruluşların çalışmaya başlamadan önce, benzer şekilde engellenmesi için yeterlidir. 

[Vamp](https://vamp.io/) , bu geçişi kolaylaştırmak ve tercih ettiğiniz kapsayıcı zamanlayıcısına özellikler getirmek için tasarlanan açık kaynaklı bir sistemdir. Vamp 'nin kanarya işlevselliği, yüzde tabanlı piyasaya çıkarma 'dan daha fazla gider. Trafik, belirli kullanıcıları, IP aralıklarını veya cihazları hedeflemek için çok çeşitli koşullara göre filtrelenebilir ve ayrılabilir. Vamp, performans ölçümlerini izler ve analiz eder ve gerçek dünya verilerine göre Otomasyon sağlar. Hatalarda otomatik geri alma veya yükleme ya da gecikme süresine göre tek tek hizmet türevlerini ölçeklendirebilirsiniz.

## <a name="set-up-azure-container-service-with-dcos"></a>DC/OS ile Azure Container Service ayarlama



1. Bir ana ve iki aracı varsayılan boyutu olan bir [DC/OS kümesi dağıtın](container-service-deployment.md) . 

2. DC/OS kümesine bağlanmak için [BIR SSH tüneli oluşturun](../container-service-connect.md) . Bu makalede, yerel bağlantı noktasındaki 80 kümesine tünel oluşturduğunuz varsayılır.


## <a name="set-up-vamp"></a>Vamp 'yi ayarlama

Artık çalışan bir DC/OS kümeniz olduğuna göre, DC/OS kullanıcı arabiriminden vamp 'yi yükleyebilirsiniz (http: \/ /localhost: 80). 

![DC/OS Kullanıcı Arabirimi](./media/container-service-dcos-vamp-canary-release/01_set_up_vamp.png)

Yükleme iki aşamada yapılır:

1. **Elaa aramasını dağıtın**.

2. Ardından, vamp DC/OS Universe paketini yükleyerek **vamp 'yi dağıtın** .

### <a name="deploy-elasticsearch"></a>Elaun aramasını dağıtma

Vamp, ölçüm toplama ve toplama için Elana Search gerektirir. [Manyetik ticio Docker görüntülerini](https://hub.docker.com/r/magneticio/elastic/) , uyumlu bir vamp Elaticsearch yığını dağıtmak için kullanabilirsiniz.

1. DC/OS Kullanıcı arabiriminde, **Hizmetler** ' e gidin ve **Hizmeti Dağıt**' a tıklayın.

2. **Yeni hizmet dağıtma** açılır listesinden **JSON modu** ' nu seçin.

   ![JSON modunu seç](./media/container-service-dcos-vamp-canary-release/02_deploy_service_json_mode.png)

3. Aşağıdaki JSON 'a yapıştırın. Bu yapılandırma, kapsayıcıyı 1 GB RAM ve Elaana arama bağlantı noktası üzerinde temel bir sistem durumu denetimi ile çalıştırır.
  
   ```JSON
   {
    "id": "elasticsearch",
    "instances": 1,
    "cpus": 0.2,
    "mem": 1024.0,
    "container": {
      "docker": {
        "image": "magneticio/elastic:2.2",
        "network": "HOST",
        "forcePullImage": true
      }
    },
    "healthChecks": [
      {
        "protocol": "TCP",
        "gracePeriodSeconds": 30,
        "intervalSeconds": 10,
        "timeoutSeconds": 5,
        "port": 9200,
        "maxConsecutiveFailures": 0
      }
    ]
   }
   ```
  

3. **Dağıt**' a tıklayın.

   DC/OS, Elayoara kapsayıcısını dağıtır. İlerlemeyi **Hizmetler** sayfasında izleyebilirsiniz.  

   ![e-dağıt mı? Elasticsearch](./media/container-service-dcos-vamp-canary-release/03_deply_elasticsearch.png)

### <a name="deploy-vamp"></a>Vamp dağıtma

Bir kez **çalışıyor**olarak rapor aradıktan sonra, vamp DC/OS Universe paketini ekleyebilirsiniz. 

1. **Universe** adresine gidin ve **vamp**araması yapın. 
   ![DC/OS Universe üzerinde vamp](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)

2. Vamp paketinin yanındaki **yükleme** ' ye tıklayın ve **Gelişmiş yükleme**' yi seçin.

3. Aşağı kaydırın ve aşağıdaki elaun arama URL 'sini girin: `http://elasticsearch.marathon.mesos:9200` . 

   ![Elaun Search URL 'sini girin](./media/container-service-dcos-vamp-canary-release/05_universe_elasticsearch_url.png)

4. **İnceleme ve**Kurulum ' a tıklayın ve ardından dağıtımı başlatmak için **Kurulum** ' a tıklayın.  

   DC/OS tüm gerekli vamp bileşenlerini dağıtır. İlerlemeyi **Hizmetler** sayfasında izleyebilirsiniz.
  
   ![Vamp 'yi Universe paketi olarak dağıtma](./media/container-service-dcos-vamp-canary-release/06_deploy_vamp.png)
  
5. Dağıtım tamamlandıktan sonra, vamp Kullanıcı arabirimine erişebilirsiniz:

   ![DC/OS üzerinde vamp hizmeti](./media/container-service-dcos-vamp-canary-release/07_deploy_vamp_complete.png)
  
   ![Vamp Kullanıcı arabirimi](./media/container-service-dcos-vamp-canary-release/08_vamp_ui.png)


## <a name="deploy-your-first-service"></a>İlk hizmetinizi dağıtma

Artık vamp çalışır durumdadır ve bir Blueprint 'ten bir hizmet dağıtın. 

En basit biçimde, bir [vamp şeması](https://docs.vamp.io/how-vamp-works/vamp-and-kubernetes#vamp-deployments) , dağıtılacak uç noktaları (ağ geçitleri), kümeleri ve hizmetleri açıklar. Vamp, aynı hizmetin farklı türevlerini, kanlarca serbest bırakma veya A/B testi için mantıksal gruplar halinde gruplamak için kümeler kullanır.  

Bu senaryo, sürüm 1,0 ' de olan [**Sava**](https://github.com/magneticio/sava-product)adlı örnek bir tek parçalı uygulama kullanır. Mimariden, manyetik ticio/Sava: 1.0.0 altındaki Docker Hub 'ında bulunan bir Docker kapsayıcısında paketlenir. Uygulama normalde bağlantı noktası 8080 ' de çalışır, ancak bu durumda, bağlantı noktası 9050 altında kullanıma sunabilirsiniz. Basit bir Blueprint kullanarak uygulamayı vamp aracılığıyla dağıtın.

1. **Dağıtımlar**'a gidin.

2. **Ekle**'ye tıklayın.

3. Aşağıdaki şema YAML 'ye yapıştırın. Bu şema, daha sonraki bir adımda değiştirdiğimiz tek bir hizmet çeşidine sahip bir küme içerir:

   ```YAML
   name: sava                        # deployment name
   gateways:
    9050: sava_cluster/webport      # stable endpoint
   clusters:
    sava_cluster:               # cluster to create
        -
          breed:
            name: sava:1.0.0        # service variant name
            deployable: magneticio/sava:1.0.0
            ports:
              webport: 8080/http # cluster endpoint, used for canary releasing
   ```

4. **Kaydet**’e tıklayın. Vamp, dağıtımı başlatır.

Dağıtım, **dağıtımlar** sayfasında listelenir. Durumunu izlemek için dağıtıma tıklayın.

![Vamp UI-Sava dağıtma](./media/container-service-dcos-vamp-canary-release/09_sava100.png)

![Vamp Kullanıcı arabirimindeki Sava hizmeti](./media/container-service-dcos-vamp-canary-release/09a_sava100.png)

**Ağ geçitleri** sayfasında listelenen iki ağ geçidi oluşturulur:

* çalışan hizmete erişmek için kararlı bir uç nokta (bağlantı noktası 9050) 
* Vamp tarafından yönetilen bir iç ağ geçidi (daha sonra bu ağ geçidinde daha fazla). 

![Vamp UI-Sava ağ geçitleri](./media/container-service-dcos-vamp-canary-release/10_vamp_sava_gateways.png)

Sava hizmeti artık dağıtıldı, ancak Azure Load Balancer trafiği henüz buraya iletmediğini bilmediğinden dışarıdan erişemezsiniz. Hizmete erişmek için Azure ağ yapılandırmasını güncelleştirin.


## <a name="update-the-azure-network-configuration"></a>Azure ağ yapılandırmasını güncelleştirme

Vamp, 9050 numaralı bağlantı noktasında kararlı bir uç nokta ortaya çıkaran DC/OS aracı düğümlerinde Sava hizmetini dağıttı. DC/OS kümesi dışından hizmete erişmek için, küme dağıtımınızdaki Azure ağ yapılandırmasında aşağıdaki değişiklikleri yapın: 

1. Aracılar için Azure Load Balancer ( **DCOS-Agent-lb-xxxx**adlı kaynak) bir sistem durumu araştırması ve 9050 numaralı bağlantı noktasındaki trafiği Sava örneklerine iletmek için bir kuralla **yapılandırın** . 

2. 9050 numaralı bağlantı noktasında trafiğe izin vermek için genel aracıların ( **xxxx-Agent-public-NSG-xxxx**adlı kaynak) **ağ güvenlik grubunu güncelleştirin** .

Azure portal kullanarak bu görevleri tamamlamaya yönelik ayrıntılı adımlar için bkz. [bir Azure Container Service uygulamasına genel erişimi etkinleştirme](container-service-enable-public-access.md). Tüm bağlantı noktası ayarları için 9050 bağlantı noktasını belirtin.


Her şey oluşturulduktan sonra DC/OS Aracısı Yük dengeleyicinin **genel bakış** dikey penceresine gidin ( **DCOS-Agent-lb-xxxx**adlı kaynak). **Genel IP adresini**bulun ve 9050 numaralı bağlantı noktasında Sava 'ya erişmek için adresi kullanın.

![Azure portal-genel IP adresi al](./media/container-service-dcos-vamp-canary-release/18_public_ip_address.png)

![Sava dili](./media/container-service-dcos-vamp-canary-release/19_sava100.png)


## <a name="run-a-canary-release"></a>Kanarya yayını çalıştırma

Bu uygulamanın, üretime yönelik olarak yayınlamak istediğiniz yeni bir sürümüne sahip olduğunuzu varsayalım. Manyetik ticio/Sava: 1.1.0 olarak kapsayıþmanıza ve gönderilmeye hazırsınızdır. Vamp, çalışan dağıtıma kolayca yeni hizmetler eklemenize olanak tanır. Bu "birleştirilmiş" hizmetler, kümedeki mevcut hizmetlerle birlikte dağıtılır ve %0 ağırlığı atanır. Trafik dağıtımını değiştirene kadar yeni bir birleştirilmiş hizmete hiçbir trafik yönlendirilemez. Vamp Kullanıcı arabirimindeki ağırlık kaydırıcısı, dağıtım üzerinde tam denetim elde etmenizi sağlar ve artımlı ayarlamalar (Canary yayını) veya anında geri alma işlemi için size izin verir.

### <a name="merge-a-new-service-variant"></a>Yeni bir hizmet türevini birleştirme

Yeni Sava 1,1 hizmetini çalışan dağıtımla birleştirmek için:

1. Vamp Kullanıcı arabiriminde, **planlar**' a tıklayın.

2. Aşağıdaki şema YAML 'ye **Ekle** ve Yapıştır 'a tıklayın: Bu şema, mevcut kümede (sava_cluster) dağıtılacak yeni bir hizmet varyantı (Sava: 1.1.0) tanımlıyor.

   ```YAML
   name: sava:1.1.0      # blueprint name
   clusters:
    sava_cluster:       # cluster to update
        -
          breed:
            name: sava:1.1.0    # service variant name
            deployable: magneticio/sava:1.1.0    
            ports:
              webport: 8080/http # cluster endpoint to update
   ```
  
3. **Kaydet**’e tıklayın. Şema, **planlar** sayfasında depolanır ve listelenir.

4. Sava: 1.1 şema üzerinde Eylem menüsünü açın ve **Birleştir ' e**tıklayın.

   ![Vamp Kullanıcı arabirimi-planlar](./media/container-service-dcos-vamp-canary-release/20_sava110_mergeto.png)

5. **Sava** dağıtımını seçin ve **Birleştir**' e tıklayın.

   ![Vamp UI-birleştirilecek şema](./media/container-service-dcos-vamp-canary-release/21_sava110_merge.png)

Vamp, çalışan dağıtımın **sava_cluster** Sava: 1.0.0 ile tanımlanan New Sava: 1.1.0 Service türevini dağıtır. 

![Vamp UI-güncelleştirilmiş Sava dağıtımı](./media/container-service-dcos-vamp-canary-release/22_sava_cluster.png)

**Sava/sava_cluster/WebPort** Gateway (küme uç noktası) de güncellenir ve yeni dağıtılan Sava 'ya bir rota eklenerek güncelleştirilir: 1.1.0. Bu noktada, burada trafik yönlendirilmez ( **Ağırlık** %0 olarak ayarlanır).

![Vamp UI-küme ağ geçidi](./media/container-service-dcos-vamp-canary-release/23_sava_cluster_webport.png)

### <a name="canary-release"></a>Canary yayını

Aynı kümede dağıtılan Sava sürümlerinin her ikisi de, **Ağırlık** kaydırıcısını taşıyarak aralarındaki trafiğin dağıtımını yapın.

1. Ağırlık ' ın yanındaki Ara ' yı tıklatın ![ ](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) . **WEIGHT**

2. Ağırlık dağılımını 50%/%50 olarak ayarlayın ve **Kaydet**' e tıklayın.

   ![Vamp UI-ağ geçidi ağırlığı kaydırıcısı](./media/container-service-dcos-vamp-canary-release/24_sava_cluster_webport_weight.png)

3. Tarayıcınıza geri dönün ve Sava sayfasını birkaç kez yenileyin. Sava uygulaması şimdi bir Sava: 1.0 sayfası ile Sava: 1.1 sayfası arasında geçiş yapar.

   ![değişen Sava 1.0 ve Sava 1.1 Hizmetleri](./media/container-service-dcos-vamp-canary-release/25_sava_100_101.png)


  > [!NOTE]
  > Bu sayfanın değişim biçimi, statik varlıkların önbelleğe alınması nedeniyle tarayıcınızın "ınbilito" veya "anonim" moduyla en iyi şekilde çalışacaktır.
  >

### <a name="filter-traffic"></a>Trafiği filtrele

Sava: 1.1.0 içinde Firefox tarayıcılarında görüntü sorunlarına neden olan bir uyumsuzluk bulduğunuzu varsayalım. Vamp 'yi gelen trafiği filtrelemek ve tüm Firefox kullanıcılarını bilinen kararlı Sava: 1.0.0 'e geri yönlendirmek için ayarlayabilirsiniz. Bu filtre, Firefox kullanıcıları için kesintiyi anında çözümler, ancak başka bir deyişle, gelişmiş Sava: 1.1.0 avantajlarından faydalanmaya devam eder.

Vamp, bir ağ geçidinde yollar arasındaki trafiği filtrelemek için **koşullar** kullanır. Trafik ilk olarak filtrelenir ve her bir rotaya uygulanan koşullara göre yönlendirilir. Kalan tüm trafik, ağ geçidi ağırlığı ayarına göre dağıtılır.

Tüm Firefox kullanıcılarını filtrelemek ve bunları eski Sava 'ya yönlendirmek için bir koşul oluşturabilirsiniz: 1.0.0:

1. Sava/sava_cluster/WebPort Gateway sayfasında **Gateways** , ![ ](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) route Sava/sava_cluster/Sava: 1.0.0/WebPort dizinine bir **koşul** eklemek için vamp UI-Edit ' e tıklayın. 

2. **User-Agent = = Firefox** koşulunu girin ve ![ vamp Kullanıcı arabirimi-Kaydet ' e tıklayın ](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) .

   Vamp, koşulu varsayılan %0 kuvvetle ekler. Trafiği filtrelemeye başlamak için, koşul gücünü ayarlamanız gerekir.

3. ![ ](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) Koşula uygulanan **kuvveti** DEğIşTIRMEk için vamp UI-Düzenle ' ye tıklayın.
 
4. **Kuvveti** %100 olarak ayarlayın ve ![ vamp UI-kaydetmek için Kaydet ' e tıklayın ](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) .

   Vamp artık koşulla eşleşen tüm trafiği (tüm Firefox kullanıcıları) Sava: 1.0.0 ' e gönderir.

   ![Vamp UI-ağ geçidine koşul uygulama](./media/container-service-dcos-vamp-canary-release/26_apply_condition.png)

5. Son olarak, tüm kalan trafiği (tüm Firefox kullanıcıları) yeni Sava: 1.1.0 göndermek için ağ geçidi ağırlığını ayarlayın. ![Vamp Kullanıcı arabirimi ' ne tıklayın ](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) , **Ağırlık** ' ın yanındaki düzenleme yapın ve ağırlık dağılımını ayarlayın. bu nedenle %100, route Sava/sava_cluster/Sava: 1.1.0/WebPort yoluna yönlendirilir.

   Koşula göre filtrelenmemiş tüm trafik artık New Sava: 1.1.0 'a yönlendirilir.

6. Filtre eylemini görmek için iki farklı tarayıcı (bir Firefox ve başka bir tarayıcı) açın ve her ikisiyle de Sava hizmetine erişin. Tüm Firefox istekleri Sava: 1.0.0 'e gönderilir, ancak diğer tüm tarayıcılar Sava: 1.1.0 'ya yönlendirilir.

   ![Vamp UI-trafiği filtrele](./media/container-service-dcos-vamp-canary-release/27_filter_traffic.png)

## <a name="summing-up"></a>Toplam

Bu makale, bir DC/OS kümesinde vamp 'ye hızlı bir giriş niteliğindedir. Başlangıçlar için Azure Container Service DC/OS kümenizde çalışır duruma gelir, vamp şeması ile bir hizmet dağıtıldı ve kullanıma sunulan uç noktada (Gateway) erişilir.

Ayrıca, vamp 'nin bazı güçlü özelliklerine de dokunduk: çalışan dağıtıma yeni bir hizmet varyantı birleştirme ve bu işlemi artımlı olarak tanıtma ve sonra bilinen bir uyumsuzluğun çözülmesi için trafiği Filtreleme.


## <a name="next-steps"></a>Sonraki adımlar

* Vamp eylemlerini, [vamp REST API](https://docs.vamp.io/how-vamp-works/events-and-metrics#events)aracılığıyla yönetme hakkında bilgi edinin.

* Node.js ' de vamp Otomasyon betikleri oluşturun ve bunları [vamp iş akışları](https://docs.vamp.io/how-vamp-works/concepts-and-components#workflows)olarak çalıştırın.

* Ek [vamp öğreticilerine](https://docs.vamp.io/tutorials/)bakın.

