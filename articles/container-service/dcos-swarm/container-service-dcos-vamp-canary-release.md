---
title: (AmortismanA Uğradı) Azure DC/OS kümesinde ile kanarya sürümü
description: Vamp'ı kanarya sürümü hizmetlerine kullanma ve bir Azure Kapsayıcı Hizmeti DC/OS kümesinde akıllı trafik filtresi uygulama
author: gggina
ms.service: container-service
ms.topic: conceptual
ms.date: 04/17/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: 2af20a1ddf4239b7eec6cceabf2ff9711959c128
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77189099"
---
# <a name="deprecated-canary-release-microservices-with-vamp-on-an-azure-container-service-dcos-cluster"></a>(AmortismanA Uğradı) Azure Konteyner Hizmeti DC/OS kümesinde içeren kanarya sürümü mikro hizmetleri

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bu izbarada, Azure Konteyner Hizmeti'nde BIR DC/OS kümesiyle Vamp'ı kurduk. Biz kanarya demo hizmeti sava serbest bırakmak ve daha sonra akıllı trafik filtreleme uygulayarak Firefox ile hizmet uyumsuzluğu çözmek. 

> [!TIP] 
> Bu izme bir DC/OS kümesi üzerinde çalışır, ancak orkestratör olarak Kubernetes ile kullanabilirsiniz.
>

## <a name="about-canary-releases-and-vamp"></a>Kanarya bültenleri ve hakkında


[Kanarya serbest](https://martinfowler.com/bliki/CanaryRelease.html) Netflix, Facebook ve Spotify gibi yenilikçi kuruluşlar tarafından benimsenen akıllı bir dağıtım stratejisidir. Bu mantıklı bir yaklaşımdır, çünkü sorunları azaltır, güvenlik ağları sunar ve yeniliği artırır. Peki neden tüm şirketler kullanmıyor? Bir CI/CD boru hattını kanarya stratejilerini içerecek şekilde genişletmek karmaşıklık katar ve kapsamlı devops bilgi ve deneyimi gerektirir. Bu, daha başlamadan küçük şirketleri ve işletmeleri engellemeye yeter. 

[Vamp](https://vamp.io/) bu geçişi kolaylaştırmak ve tercih ettiğiniz konteyner zamanlayıcısına kanarya bırakma özellikleri getirmek için tasarlanmış açık kaynak kodlu bir sistemdir. Vamp'ın kanarya işlevselliği yüzde tabanlı kullanıma göre daha fazladır. Trafik filtrelenebilir ve örneğin belirli kullanıcıları, IP aralıklarını veya aygıtları hedeflemek için çok çeşitli koşullara bölünebilir. , gerçek dünya verilerine dayalı otomasyona olanak tanıyan performans ölçümlerini izler ve analiz eder. Hatalar üzerinde otomatik geri alma ayarlayabilir veya yük veya gecikme durumuna göre tek tek hizmet türevlerini ölçeklendirebilirsiniz.

## <a name="set-up-azure-container-service-with-dcos"></a>DC/OS ile Azure Kapsayıcı Hizmeti ayarlama



1. Bir ana ve varsayılan boyutta iki aracıyla [bir DC/İŞLetim sistemi kümesi dağıtın.](container-service-deployment.md) 

2. DC/OS kümesine bağlanmak için [bir SSH tüneli oluşturun.](../container-service-connect.md) Bu makalede, yerel bağlantı noktası 80 üzerinde kümeye tünel varsayar.


## <a name="set-up-vamp"></a>Vamp'ı ayarlama

Artık çalışan bir DC/OS kümeniz olduğuna göre, DC/OS Kullanıcı Arabirimi'nden yükleyebilirsiniz (http:\//localhost:80). 

![DC/OS Kullanıcı Arabirimi](./media/container-service-dcos-vamp-canary-release/01_set_up_vamp.png)

Kurulum iki aşamada yapılır:

1. **Elasticsearch dağıtın.**

2. Ardından DC/OS evren paketini kurarak **Vamp'ı dağıtın.**

### <a name="deploy-elasticsearch"></a>Elasticsearch'u dağıt

, ölçümlerin toplanması ve toplanması için Elasticsearch gerektirir. Uyumlu bir Elasticsearch yığını dağıtmak için [magneticio Docker görüntülerini](https://hub.docker.com/r/magneticio/elastic/) kullanabilirsiniz.

1. DC/OS Kullanıcı İşi'nde **Hizmetler'e** gidin ve **Hizmeti Dağıt'ı**tıklatın.

2. Yeni Hizmet Aç'ını **Dağıt'tan** **JSON modunu** seçin.

   ![JSON modunu seçin](./media/container-service-dcos-vamp-canary-release/02_deploy_service_json_mode.png)

3. Aşağıdaki JSON yapıştırın. Bu yapılandırma, 1 GB RAM ve Elasticsearch bağlantı noktasında temel bir sistem durumu denetimi ile konteyner çalışır.
  
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
  

3. **Dağıt'ı**tıklatın.

   DC/OS Elasticsearch konteynerini dağıtıyor. **İlerlemeyi Hizmetler** sayfasından izleyebilirsiniz.  

   ![dağıtmak e? Elastik arama](./media/container-service-dcos-vamp-canary-release/03_deply_elasticsearch.png)

### <a name="deploy-vamp"></a>Vamp'ı Dağıt

**Elasticsearch'ü Çalıştırma**olarak bildirdikten sonra DC/OS Universe paketini ekleyebilirsiniz. 

1. **Evrene** git ve **vampir**ara. 
   ![DC/OS evreninde](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)

2. paketinin yanına **yükle'yi** tıklatın ve **Gelişmiş Kurulum'u**seçin.

3. Aşağı kaydırın ve aşağıdaki elasticsearch-url girin: `http://elasticsearch.marathon.mesos:9200`. 

   ![Elasticsearch URL'yi girin](./media/container-service-dcos-vamp-canary-release/05_universe_elasticsearch_url.png)

4. **Gözden Geçir ve Yükle'yi**tıklatın, ardından dağıtımı başlatmak için **Yükle'yi** tıklatın.  

   DC/OS gerekli tüm bileşenlerini dağıtMaktadır. **İlerlemeyi Hizmetler** sayfasından izleyebilirsiniz.
  
   ![Vamp'ı evren paketi olarak dağıtın](./media/container-service-dcos-vamp-canary-release/06_deploy_vamp.png)
  
5. Dağıtım tamamlandıktan sonra UI'ye erişebilirsiniz:

   ![DC/OS'de hizmeti](./media/container-service-dcos-vamp-canary-release/07_deploy_vamp_complete.png)
  
   ![UI](./media/container-service-dcos-vamp-canary-release/08_vamp_ui.png)


## <a name="deploy-your-first-service"></a>İlk hizmetinizi dağıtın

çalışmaya devam ettiğine göre, bir plandan bir hizmet dağıtın. 

En basit haliyle, [Bir planı](https://docs.vamp.io/how-vamp-works/vamp-and-kubernetes#vamp-deployments) dağıtılabilmek için uç noktaları (ağ geçitleri), kümeleri ve hizmetleri açıklar. , aynı hizmetin farklı türevlerini kanarya serbesti veya A/B testi için mantıksal gruplarhalinde gruplandırmak için kümeler kullanır.  

Bu senaryo, sürüm 1.0 olan [**sava**](https://github.com/magneticio/sava)adlı bir örnek monolitik uygulama kullanır. Monolit, Docker Hub'da magneticio/sava:1.0.0 altında bulunan Docker konteynerinde paketlenir. Uygulama normalde port 8080 üzerinde çalışır, ancak bu durumda port 9050 altında ortaya çıkarmak istiyorum. Basit bir plan kullanarak uygulamayı üzerinden dağıtın.

1. **Dağıtımlara**gidin.

2. **Ekle**’ye tıklayın.

3. Aşağıdaki plana yapıştırın YAML. Bu plan, daha sonraki bir adımda değiştirdiğimiz tek bir hizmet varyantı olan bir küme içerir:

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

4. **Kaydet**'e tıklayın. dağıtımı başlatır.

Dağıtım **Dağıtımlar** sayfasında listelenir. Durumunu izlemek için dağıtıma tıklayın.

![UI - sava dağıtma](./media/container-service-dcos-vamp-canary-release/09_sava100.png)

![UI sava hizmeti](./media/container-service-dcos-vamp-canary-release/09a_sava100.png)

**Ağ Geçitleri** sayfasında listelenen iki ağ geçidi oluşturulur:

* çalışan hizmete erişmek için kararlı bir bitiş noktası (bağlantı noktası 9050) 
* tarafından yönetilen bir iç ağ geçidi (daha sonra bu ağ geçidinde daha fazla). 

![UI - sava ağ geçitleri](./media/container-service-dcos-vamp-canary-release/10_vamp_sava_gateways.png)

Sava hizmeti şu anda dağıtıldı, ancak Azure Yük Dengeleyici'si trafiği henüz iletmeyi bilmediği için dışarıdan erişemezsiniz. Hizmete erişmek için Azure ağ yapılandırmasını güncelleştirin.


## <a name="update-the-azure-network-configuration"></a>Azure ağ yapılandırmasını güncelleştirme

, sava hizmetini DC/OS aracı düğümlerine yerleştirmiş ve 9050 numaralı bağlantı noktasında sabit bir bitiş noktası ortaya çıkarmış. Hizmete DC/İŞLET kümesi dışından erişmek için, küme dağıtımınızda Azure ağ yapılandırmasında aşağıdaki değişiklikleri yapın: 

1. Aracılar için **Azure Yük Dengeleyicisini** **(dcos-agent-lb-xxxx**adlı kaynak) bir sağlık sondası ve 9050 no'lu bağlantı noktasındaki trafiği sava örneklerine iletme kuralıyla yapılandırın. 

2. Bağlantı noktası 9050'deki trafiğe izin vermek için ortak aracılar **(XXXX-agent-public-nsg-XXXX**adlı kaynak) için **ağ güvenlik grubunu güncelleştirin.**

Azure portalını kullanarak bu görevleri tamamlamak için ayrıntılı adımlar [için](container-service-enable-public-access.md)bkz. Tüm bağlantı noktası ayarları için bağlantı noktası 9050'yi belirtin.


Her şey oluşturulduktan sonra, DC/OS aracıyük bakiyesinin **(dcos-agent-lb-xxxx**adlı kaynak) **Genel Bakış** bıçağına gidin. Genel **IP adresini**bulun ve 9050 portundaki sava'ya erişmek için adresi kullanın.

![Azure portalı - genel IP adresi alın](./media/container-service-dcos-vamp-canary-release/18_public_ip_address.png)

![Sava](./media/container-service-dcos-vamp-canary-release/19_sava100.png)


## <a name="run-a-canary-release"></a>Bir kanarya sürümü çalıştırın

Bu uygulamanın, kanaryanın üretime salmak istediği yeni bir sürümü olduğunu varsayalım. Manyetikio/sava:1.1.0 olarak kaplanmış ve gitmeye hazır. , çalışan dağıtıma kolayca yeni hizmetler eklemenizi sağlar. Bu "birleştirilmiş" hizmetler kümedeki varolan hizmetlerin yanında dağıtılır ve %0'lık bir ağırlık atanır. Trafik dağıtımını ayarlayana kadar trafik yeni birleştirilmiş bir hizmete yönlendirilir. UI'deki ağırlık kaydırıcısı, dağıtım üzerinde tam kontrol sağlar ve artımlı ayarlamalar (kanarya sürümü) veya anında geri alma sağlar.

### <a name="merge-a-new-service-variant"></a>Yeni bir hizmet varyantı birleştirme

Yeni sava 1.1 hizmetini çalışan dağıtımla birleştirmek için:

1. UI'de **Planlar'ı**tıklatın.

2. Aşağıdaki plan YAML'de **Ekle** ve yapıştır'ı tıklatın: Bu plan, varolan küme (sava_cluster) içinde dağıtmak için yeni bir hizmet varyantını (sava:1.1.0) açıklar.

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
  
3. **Kaydet**'e tıklayın. Plan saklanır ve **Planlar** sayfasında listelenir.

4. Sava:1.1 planındaki eylem menüsünü açın ve **Birleştir'e**tıklayın.

   ![UI - planlar](./media/container-service-dcos-vamp-canary-release/20_sava110_mergeto.png)

5. **Sava** dağıtımını seçin ve **Birleştir'i**tıklatın.

   ![UI - planı dağıtımla birleştirme](./media/container-service-dcos-vamp-canary-release/21_sava110_merge.png)

, çalışan dağıtımın **sava_cluster** sava:1.0.0 ile birlikte planda açıklanan yeni sava:1.1.0 hizmet çeşidini dağıtır. 

![UI - güncellenmiş sava dağıtım](./media/container-service-dcos-vamp-canary-release/22_sava_cluster.png)

**Sava/sava_cluster/webport** ağ geçidi (küme bitiş noktası) da güncellenerek yeni dağıtılan sava:1.1.0'a bir rota ekler. Bu noktada, hiçbir trafik burada yönlendirilir **(WEIGHT** 0%) olarak ayarlanır.

![UI - küme ağ geçidi](./media/container-service-dcos-vamp-canary-release/23_sava_cluster_webport.png)

### <a name="canary-release"></a>Kanarya serbest

Aynı kümede konuşlandırılmış sava'nın her iki sürümüyle, **WEIGHT** kaydırıcısını hareket ettirerek aralarındaki trafiğin dağılımını ayarlayın.

1. UI'yi ![](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) tıklatın - **WEIGHT'in**yanında edin.

2. Ağırlık dağılımını %50/50 olarak ayarlayın ve **Kaydet'i**tıklatın.

   ![UI - ağ geçidi ağırlık kaydırıcısı](./media/container-service-dcos-vamp-canary-release/24_sava_cluster_webport_weight.png)

3. Tarayıcınıza geri dön ve sava sayfasını birkaç kez daha yenileyin. Sava uygulaması artık bir sava:1.0 sayfası ile sava:1.1 sayfası arasında geçiş yap.

   ![alternatif sava1.0 ve sava1.1 hizmetleri](./media/container-service-dcos-vamp-canary-release/25_sava_100_101.png)


  > [!NOTE]
  > Sayfanın bu şekilde değiştirilmesi, statik varlıkların önbelleğe aldaması nedeniyle tarayıcınızın "Gizli" veya "Anonim" moduyla en iyi şekilde çalışır.
  >

### <a name="filter-traffic"></a>Filtre trafiği

Dağıtımdan sonra sava:1.1.0'da Firefox tarayıcılarında görüntü sorunlarına neden olan bir uyumsuzluk keşfettiğinizi varsayalım. Vamp'ı gelen trafiği filtrelemek ve tüm Firefox kullanıcılarını bilinen kararlı sava:1.0.0'a yönlendirecek şekilde ayarlayabilirsiniz. Bu filtre, Firefox kullanıcılarının aksaklıklarını anında çözerken, herkes geliştirilmiş sava:1.1.0 avantajlarından yararlanmaya devam ediyor.

, ağ geçidindeki rotalar arasındaki trafiği filtrelemek için **koşulları** kullanır. Trafik ilk olarak her rotaya uygulanan koşullara göre filtrelenir ve yönlendirilir. Kalan tüm trafik ağ geçidi ağırlık ayarına göre dağıtılır.

Tüm Firefox kullanıcılarını filtrelemek ve eski sava:1.0.0'a yönlendirmek için bir koşul oluşturabilirsiniz:

1. Sava/sava_cluster/webport **Ağ Geçitleri** sayfasında, UI'yi tıklatın ![- rota sava/sava_cluster/sava:1.0.0/webport'a bir](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) **DURUM** eklemek için düzenlendi. 

2. Koşul **kullanıcı aracısı ==** Firefox ![girin ve UI tıklayın - kaydet](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).

   varsayılan gücü %0 olan koşulu ekler. Trafiği filtrelemeye başlamak için durum gücünü ayarlamanız gerekir.

3. UI'yi ![](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) tıklatın - koşula uygulanan **GÜCÜ** değiştirmek için değiştirin.
 
4. **STRENGTH'ı** %100'e ![ayarlayın ve UI'yi tıklatın - kaydetmek için kaydedin.](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png)

   şimdi sava:1.0.0 için durum (tüm Firefox kullanıcıları) eşleşen tüm trafik gönderir.

   ![UI - ağ geçidine koşul uygulayın](./media/container-service-dcos-vamp-canary-release/26_apply_condition.png)

5. Son olarak, kalan tüm trafiği (Firefox olmayan tüm kullanıcılar) yeni sava:1.1.0'a göndermek için ağ geçidi ağırlığını ayarlayın. UI'yi ![](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) tıklatın - **WEIGHT'in** yanında edin ve ağırlık dağılımını %100 sava/sava_cluster/sava:1.1.0/webport rotasına yönlendirilir şekilde ayarlayın.

   Koşultarafından filtrelenmemiş tüm trafik artık yeni sava:1.1.0'a yönlendirilir.

6. Filtreyi iş başında görmek için iki farklı tarayıcı (bir Firefox ve bir diğer tarayıcı) açın ve her ikisinden de sava hizmetine erişin. Tüm Firefox istekleri sava:1.0.0'a gönderilirken, diğer tüm tarayıcılar sava:1.1.0 adresine yönlendirilir.

   ![UI - filtre trafiği](./media/container-service-dcos-vamp-canary-release/27_filter_traffic.png)

## <a name="summing-up"></a>Özetleme

Bu makale, bir DC/OS kümesinde Vamp'a hızlı bir girişti. Yeni başlayanlar için, Azure Kapsayıcı Hizmeti DC/OS kümenizde Vamp'ı çalışır hale verdiniz, planı olan bir hizmeti dağıttınız ve açık uç noktada (ağ geçidi) eriştin.

Vamp'ın bazı güçlü özelliklerine de değindik: yeni bir hizmet varyantını çalışan dağıtıma birleştirme ve kademeli olarak kullanıma sunma, ardından bilinen bir uyumsuzluğu gidermek için trafiği filtreleme.


## <a name="next-steps"></a>Sonraki adımlar

* [Vamp REST API](https://docs.vamp.io/how-vamp-works/events-and-metrics#events)ile eylemlerini yönetme hakkında bilgi edinin.

* Düğüm.js'de otomasyon komut dosyaları oluşturun ve bunları iş akışları olarak [çalıştırın.](https://docs.vamp.io/how-vamp-works/concepts-and-components#workflows)

* Ek [VAMP öğreticilerine](https://docs.vamp.io/tutorials/)bakın.

