---
title: Kavramlar - Azure Kubernetes Hizmetleri (AKS) için Kubernetes temelleri
description: Kubernetes'in temel küme ve iş yükü bileşenlerini ve Bunların Azure Kubernetes Hizmeti (AKS)'deki özelliklerle nasıl ilişkili olduğunu öğrenin
services: container-service
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 019c886aba1c8fe34211e73e4d960b14e79303b9
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80617433"
---
# <a name="kubernetes-core-concepts-for-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti (AKS) için Kubernetes temel kavramları

Uygulama geliştirme kapsayıcı tabanlı bir yaklaşıma doğru ilerlerken, kaynakları düzenleme ve yönetme ihtiyacı önemlidir. Kubernetes, hataya dayanıklı uygulama iş yüklerinin güvenilir bir zamanlama sını sağlayan lider bir platformdur. Azure Kubernetes Service (AKS), kapsayıcı tabanlı uygulama dağıtımını ve yönetimini daha da basitleştiren yönetilen bir Kubernetes teklifidir.

Bu makalede, *kontrol düzlemi,* *düğümler*ve *düğüm havuzları*gibi çekirdek Kubernetes altyapı bileşenleri tanıtılmaktadır. *Bölmeler,* *dağıtımlar*ve *kümeler* gibi iş yükü kaynakları, kaynakların *ad alanlarına*nasıl gruplandırılanın yanı sıra da tanıtılır.

## <a name="what-is-kubernetes"></a>Kubernetes nedir?

Kubernetes, konteyner tabanlı uygulamaları ve bunların ilişkili ağ ve depolama bileşenlerini yöneten hızla gelişen bir platformdur. Odak noktası, temel altyapı bileşenleri değil, uygulama iş yükleridir. Kubernetes, yönetim işlemleri için güçlü bir API kümesi tarafından desteklenen dağıtımlara bildirimsel bir yaklaşım sağlar.

Kubernetes'in bu uygulama bileşenlerinin kullanılabilirliğini yönetmesinden ve yönetmesinden yararlanan modern, taşınabilir, mikro hizmetler tabanlı uygulamalar oluşturabilir ve çalıştırabilirsiniz. Kubernetes, mikro hizmetlere dayalı uygulamaların benimsenme yoluyla ekipler ilerledikçe hem devletsiz hem de durumdışı uygulamaları destekler.

Açık bir platform olarak Kubernetes, tercih ettiğiniz programlama dili, işletim sistemi, kitaplıklar veya mesajlaşma veri tonunuz ile uygulamalarınızı oluşturmanıza olanak tanır. Mevcut sürekli tümleştirme ve sürekli teslimat (CI/CD) araçları, bültenleri zamanlamak ve dağıtmak için Kubernetes ile tümleştirebilirsiniz.

Azure Kubernetes Hizmeti (AKS), yükseltmeleri koordine etmek de dahil olmak üzere dağıtım ve temel yönetim görevlerinin karmaşıklığını azaltan yönetilen bir Kubernetes hizmeti sağlar. AKS denetim düzlemi Azure platformu tarafından yönetilir ve yalnızca uygulamalarınızı çalıştıran AKS düğümleri için ödeme yaptığınızda. AKS, açık kaynak kodlu Azure Kubernetes Servis Motoru'nun[(aks motoru)][aks-engine]üzerine inşa edilmiştir.

## <a name="kubernetes-cluster-architecture"></a>Kubernetes küme mimarisi

Bir Kubernetes kümesi iki bileşene ayrılır:

- *Kontrol düzlemi* düğümleri temel Kubernetes hizmetlerini ve uygulama iş yüklerinin düzenlenmesini sağlar.
- *Düğümler* uygulama iş yüklerinizi çalıştırın.

![Kubernetes kontrol düzlemi ve düğüm bileşenleri](media/concepts-clusters-workloads/control-plane-and-nodes.png)

## <a name="control-plane"></a>Kontrol düzlemi

Bir AKS kümesi oluşturduğunuzda, bir denetim düzlemi otomatik olarak oluşturulur ve yapılandırılır. Bu denetim düzlemi, kullanıcıdan soyutlanmış yönetilen bir Azure kaynağı olarak sağlanır. Kontrol düzlemi için hiçbir maliyet yoktur, yalnızca AKS kümesinin bir parçası olan düğümler.

Kontrol düzlemi aşağıdaki çekirdek Kubernetes bileşenlerini içerir:

- *kube-apiserver* - API sunucusu, altta yatan Kubernetes API'lerinin nasıl açığa çiktIğidir. Bu bileşen, Kubernetes panosu `kubectl` gibi yönetim araçları için etkileşim sağlar.
- *etcd* - Kubernetes küme ve yapılandırma durumunu korumak için, yüksek kullanılabilir *etcd* Kubernetes içinde önemli bir değer deposudur.
- *kube-scheduler* - Uygulamaları oluşturduğunuzda veya ölçeklendirdiğinizde, Zamanlayıcı hangi düğümlerin iş yükünü çalıştırabileceğini belirler ve bunları başlatır.
- *kube-controller-manager* - Denetleyici Yöneticisi, bölmeçoğaltma ve düğüm işlemlerini işleme gibi eylemleri gerçekleştiren bir dizi küçük Denetleyiciyi denetler.

AKS, özel bir API sunucusu, Zamanlayıcı, vb. içeren tek kiracılı bir kontrol düzlemi sağlar. Düğümlerin sayısını ve boyutunu tanımlarsınız ve Azure platformu denetim düzlemi ile düğümler arasındaki güvenli iletişimi yapılandırır. Kontrol düzlemi ile etkileşim Kubernetes API'ler aracılığıyla oluşur, gibi `kubectl` veya Kubernetes panosu.

Bu yönetilen denetim düzlemi, bileşenleri yüksek kullanılabilir *bir etcd* deposu gibi yapılandırmanız gerekolmadığı anlamına gelir, ancak aynı zamanda kontrol düzlemine doğrudan erişemediğiniz anlamına gelir. Kubernetes yükseltmeleri, kontrol düzlemini ve ardından düğümleri yükselten Azure CLI veya Azure portalı aracılığıyla düzenlenir. Olası sorunları gidermek için, Azure Monitor günlükleri aracılığıyla denetim düzlemi günlüklerini inceleyebilirsiniz.

Kontrol düzlemini belirli bir şekilde yapılandırmanız gerekiyorsa veya doğrudan erişime ihtiyacınız varsa, [aks-engine][aks-engine]kullanarak kendi Kubernetes kümenizi dağıtabilirsiniz.

İlişkili en iyi uygulamalar [için, AKS'deki küme güvenliği ve yükseltmeleri için en iyi uygulamalara][operator-best-practices-cluster-security]bakın.

## <a name="nodes-and-node-pools"></a>Düğümler ve düğüm havuzları

Uygulamalarınızı ve destekleyici hizmetleri çalıştırmak için bir Kubernetes *düğümüne*ihtiyacınız vardır. Bir AKS kümesinde, Kubernetes düğüm bileşenlerini ve kapsayıcı çalışma süresini çalıştıran bir Azure sanal makinesi (VM) olan bir veya daha fazla düğüm vardır:

- Kontrol `kubelet` düzleminden gelen düzenleme isteklerini işleyen ve istenen kapsayıcıları çalıştırmanın zamanlamasını sağlayan Kubernetes aracısidır.
- Sanal ağ her düğümde *kube-proxy* tarafından işlenir. Proxy ağ trafiğini yönlendirir ve hizmetler ve bölmeler için IP adresleme yönetir.
- *Kapsayıcı çalışma süresi,* kapsayıcı laştırılmış uygulamaların çalışmasına ve sanal ağ ve depolama gibi ek kaynaklarla etkileşimkurmasına olanak tanıyan bileşendir. AKS'de Moby, kapsayıcı çalışma zamanı olarak kullanılır.

![Azure sanal makine ve Bir Kubernetes düğümü için destek kaynakları](media/concepts-clusters-workloads/aks-node-resource-interactions.png)

Düğümleriniz için Azure VM boyutu kaç CPU'yu, ne kadar bellek olduğunu ve kullanılabilir depolama alanının boyutunu ve türünü (yüksek performanslı SSD veya normal HDD gibi) tanımlar. Büyük miktarda CPU ve bellek veya yüksek performanslı depolama gerektiren uygulamalar için bir gereksinim bekliyorsanız, düğüm boyutunu buna göre planlayın. Talebi karşılamak için AKS kümenizdeki düğüm sayısını da ölçeklendirebilirsiniz.

AKS'de, kümenizdeki düğümlerin VM görüntüsü şu anda Ubuntu Linux veya Windows Server 2019'u temel almaktadır. Bir AKS kümesi oluşturduğunuzda veya düğüm sayısını ölçeklediğinizde, Azure platformu istenen vm sayısını oluşturur ve bunları yapılandırır. Gerçekleştirmeniz için manuel yapılandırma yoktur. Aracı düğümleri standart sanal makineler olarak faturalandırılır, bu nedenle kullandığınız VM boyutunda [(Azure rezervasyonları][reservation-discounts]dahil) yaptığınız tüm indirimler otomatik olarak uygulanır.

Farklı bir ana bilgisayar işletim sistemi, konteyner çalışma zamanı kullanmanız veya özel paketler eklemeniz gerekiyorsa, [aks-engine][aks-engine]kullanarak kendi Kubernetes kümenizi dağıtabilirsiniz. Yukarı akış `aks-engine` özellikleri yayımlar ve aks kümelerinde resmi olarak desteklenmeden önce yapılandırma seçenekleri sağlar. Örneğin, Moby dışında bir kapsayıcı çalışma zamanı kullanmak istiyorsanız, `aks-engine` mevcut gereksinimlerinizi karşılayan bir Kubernetes kümesini yapılandırmak ve dağıtmak için kullanabilirsiniz.

### <a name="resource-reservations"></a>Kaynak rezervasyonları

Düğüm kaynakları, düğümün kümenizin bir parçası olarak çalışmasını sağlamak için AKS tarafından kullanılır. Bu, düğümünüzün toplam kaynakları ile AKS'de kullanıldığında ayrılabilen kaynaklar arasında bir tutarsızlık oluşturabilir. Bu, kullanıcı dağıtılan bölmeler için istek ler ve sınırlar belirlerken dikkat edilmesi gereken öneme sahiptir.

Düğümün ayrılabilen kaynaklarını çalıştırmak için:
```kubectl
kubectl describe node [NODE_NAME]

```

Düğüm performansını ve işlevselliğini korumak için, kaynaklar AKS tarafından her düğüme ayrılmıştır. Kaynaklarda bir düğüm büyüdükçe, yönetime ihtiyaç duyan daha yüksek miktarda kullanıcı dağıtılan bölmeler nedeniyle kaynak ayırma büyür.

>[!NOTE]
> Kapsayıcı Öngörüleri (OMS) gibi AKS eklentilerinin kullanılması ek düğüm kaynaklarını tüketir.

- **CPU** - ayrılmış CPU ek özellikleri çalıştıran nedeniyle daha az tahsis EDIlebilen CPU neden olabilir düğüm türü ve küme yapılandırmasına bağlıdır

| Ana bilgisayarda CPU çekirdekleri | 1 | 2 | 4 | 8 | 16 | 32|64|
|---|---|---|---|---|---|---|---|
|Kube-ayrılmış (millicores)|60|100|140|180|260|420|740|

- **AKS** tarafından kullanılan bellek - bellek iki değerin toplamını içerir.

1. Kubelet daemon konteyner oluşturma ve sonlandırma yönetmek için tüm Kubernetes aracı düğümleri yüklü. AKS varsayılan olarak, bu daemon aşağıdaki tahliye kuralı vardır: *memory.available<750Mi*, bir düğüm her zaman her zaman en az 750 Mi tahsis olmalıdır anlamına gelir.  Bir ana bilgisayar kullanılabilir bellek eşiğinin altında olduğunda, kubelet çalışan bölmelerden birini sonlandırarak ana makinedeki belleği serbest ve korur. Kullanılabilir bellek 750Mi eşiğinin ötesine geçince bu reaktif bir eylemdir.

2. İkinci değer, kubelet daemonunun düzgün çalışması için bellek rezervasyonlarının gerileme oranıdır (kube-ayrılmış).
    - İlk 4 GB bellek%25'i
    - Sonraki 4 GB belleğin %20'si (8 GB'a kadar)
    - Sonraki 8 GB belleğin %10'u (16 GB'a kadar)
    - Sonraki 112 GB bellek (128 GB'a kadar) %6'sı
    - 128 GB üzerindeki tüm belleklerin %2'si

Bellek ve CPU ayırma için yukarıdaki kurallar, sistem durumu kümelemek için kritik olan bazı barındırma sistemi bölmeleri de dahil olmak üzere aracı düğümlerini sağlıklı tutmak için kullanılır. Bu ayırma kuralları, düğümün bir Kubernetes kümesinin parçası değilse, daha az tahsis edilebilir bellek ve CPU bildirmesine de neden olur. Yukarıdaki kaynak rezervasyonları değiştirilemez.

Örneğin, bir düğüm 7 GB sunuyorsa, 750Mi sabit tahliye eşiğinin üzerine ayrılamayan belleğin %34'ünü bildirir.

`(0.25*4) + (0.20*3) = + 1 GB + 0.6GB = 1.6GB / 7GB = 22.86% reserved`

Kubernetes kendisi için rezervasyonları ek olarak, altta yatan düğüm os işlevlerini korumak için CPU ve bellek kaynakları bir miktar ayırır.

İlişkili en iyi uygulamalar [için, AKS'deki temel zamanlayıcı özellikleri için en iyi uygulamalara][operator-best-practices-scheduler]bakın.

### <a name="node-pools"></a>Düğüm havuzları

Aynı yapılandırmanın düğümleri düğüm *havuzlarında*birlikte gruplandırılır. Bir Kubernetes kümesi bir veya daha fazla düğüm havuzu içerir. Varsayılan düğüm *havuzu*oluşturan bir AKS kümesi oluşturduğunuzda ilk düğüm sayısı ve boyutu tanımlanır. AKS'deki bu varsayılan düğüm havuzu, aracı düğümünüzü çalıştıran temel VM'leri içerir.

> [!NOTE]
> Kümenizin güvenilir çalıştığından emin olmak için varsayılan düğüm havuzunda en az 2 (iki) düğüm çalıştırmalısınız.

Bir AKS kümesini ölçeklediğinizde veya yükselttiğiniz de, eylem varsayılan düğüm havuzuna karşı gerçekleştirilir. Ayrıca, belirli bir düğüm havuzuna ölçeklendirmeyi veya yükseltmeyi de seçebilirsiniz. Yükseltme işlemleri için, çalışan kapsayıcılar tüm düğümler başarıyla yükseltilene kadar düğüm havuzundaki diğer düğümlerde zamanlanır.

AKS'de birden çok düğüm havuzunun nasıl kullanılacağı hakkında daha fazla bilgi için, [AKS'deki bir küme için birden çok düğüm havuzu oluştur ve yönet'][use-multiple-node-pools]e bakın.

### <a name="node-selectors"></a>Düğüm seçicileri

Birden çok düğüm havuzu içeren bir AKS kümesinde, Kubernetes Zamanlayıcısı'na belirli bir kaynak için hangi düğüm havuzunu kullanacağınızı söylemeniz gerekebilir. Örneğin, giriş denetleyicileri Windows Server düğümlerinde çalışmamalıdır (şu anda AKS'de önizlemede). Düğüm seçiciler, bölmenin zamanlanması gereken yeri denetlemek için düğüm işletim sistemi gibi çeşitli parametreleri tanımlamanıza izin verebiliyor.

Aşağıdaki temel örnek, "beta.kubernetes.io/os" düğüm seçicisini kullanarak bir Linux düğümünde bir NGINX örneğini *planlar: linux:*

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: nginx
spec:
  containers:
    - name: myfrontend
      image: nginx:1.15.12
  nodeSelector:
    "beta.kubernetes.io/os": linux
```

Bölmelerin nerede zamanlandığını nasıl denetleyebileceğiniz hakkında daha fazla bilgi için [AKS'deki gelişmiş zamanlayıcı özellikleri için en iyi uygulamalara][operator-best-practices-advanced-scheduler]bakın.

## <a name="pods"></a>Bakla

Kubernetes, uygulamanızın bir örneğini çalıştırmak için *bölmekullanır.* Bir bölme, uygulamanızın tek bir örneğini temsil eder. Pod'ların genellikle kapsayıcıyla 1:1 eşlemeleri vardır, ancak bir bölmenin birden çok kapsayıcı içerebileceği gelişmiş senaryolar vardır. Bu çok kapsayıcılı bölmeler aynı düğümüzerinde birlikte zamanlanır ve kapsayıcıların ilgili kaynakları paylaşmasına izin verir.

Bir bölme oluşturduğunuzda, belirli bir miktarda CPU veya bellek kaynağı istemek için *kaynak isteklerini* tanımlayabilirsiniz. Kubernetes Zamanlayıcısı, isteği karşılamak için kullanılabilir kaynaklara sahip bir düğüm üzerinde çalışacak bölmeleri zamanlamaya çalışır. Ayrıca, belirli bir bölmenin alttaki düğümden çok fazla bilgi işlem kaynağı tüketmesini engelleyen maksimum kaynak sınırları da belirtebilirsiniz. En iyi uygulama, Kubernetes Zamanlayıcısının hangi kaynaklara ihtiyaç duyulduğunu ve izin verilip verilenolduğunu anlamasına yardımcı olmak için tüm bölmeler için kaynak sınırları eklemektir.

Daha fazla bilgi için [Kubernetes pod'ları][kubernetes-pods] ve [Kubernetes pod yaşam döngüsüne][kubernetes-pod-lifecycle]bakın.

Bölme mantıksal bir kaynaktır, ancak kapsayıcı(lar) uygulama iş yüklerinin çalıştığı yerdir. Bölmeler genellikle geçici, tek kullanımlık kaynaklardır ve tek tek zamanlanmış bölmeler Kubernetes'in sağladığı yüksek kullanılabilirlik ve artıklık özelliklerinden bazılarını kaçırır. Bunun yerine, bölmeler genellikle dağıtılır ve Dağıtım *Denetleyicisi*gibi Kubernetes Denetleyicileri tarafından yönetilir.

## <a name="deployments-and-yaml-manifests"></a>Dağıtımlar ve YAML bildirimleri

*Dağıtım,* Kubernetes Dağıtım Denetleyicisi tarafından yönetilen bir veya daha fazla özdeş bölmeyi temsil eder. Dağıtım oluşturulacak *yineleme* (bakla) sayısını tanımlar ve Kubernetes Zamanlayıcısı, bölmeveya düğümlerle karşılaşırsa, sağlıklı düğümlerde ek bölmelerin zamanlandığını sağlar.

Bölmelerin, kullanılan kapsayıcı görüntüsünün veya ekli depolama alanının yapılandırmasını değiştirmek için dağıtımları güncelleştirebilirsiniz. Dağıtım Denetleyicisi, belirli sayıda yinelemeyi boşaltır ve sonlandırır, yeni dağıtım tanımından yinelemeler oluşturur ve dağıtımdaki tüm yinelemeler güncelleştirilene kadar işlemi sürdürür.

AKS'deki çoğu durum ifadesüz uygulama, tek tek bölmeleri zamanletmek yerine dağıtım modelini kullanmalıdır. Kubernetes, küme içinde gerekli çoğaltma sayısının çalıştığından emin olmak için dağıtımların durumunu ve durumunu izleyebilir. Tek tek bölmeleri zamanladiğinizde, bir sorunla karşılaştıklarında bölmeler yeniden başlatılmazsa ve geçerli düğümleri bir sorunla karşılaşırsa sağlıklı düğümlerde yeniden zamanlanmaz.

Bir uygulama, yönetim kararlarının her zaman alınabilmesi için bir çok örnek gerektiriyorsa, bu özelliği bozacak bir güncelleştirme işlemi istemezsinüz. *Pod Disruption Bütçeleri,* bir güncelleştirme veya düğüm yükseltmesi sırasında bir dağıtımda kaç yinelemenin indirilebileceğini tanımlamak için kullanılabilir. Örneğin, dağıtımınızda *5* yineleme varsa, bir seferde silinen/yeniden zamanlanan yalnızca bir yinelemeye izin vermek için *4'lük* bir bölme kesintisi tanımlayabilirsiniz. Pod kaynak sınırlarında olduğu gibi, en iyi yöntem, her zaman var olmak için en az sayıda yineleme gerektiren uygulamalarda bölme kesintisi bütçelerini tanımlamaktır.

Dağıtımlar genellikle oluşturulur ve `kubectl create` yönetilir `kubectl apply`veya . Dağıtım oluşturmak için YAML (YAML Ain't Markup Language) biçiminde bir bildirim dosyası tanımlarsınız. Aşağıdaki örnek, NGINX web sunucusunun temel dağıtımını oluşturur. Dağıtım oluşturulacak *3* yineleme belirtir ve bu bağlantı noktası *80* kapsayıcı üzerinde açık olması. Kaynak istekleri ve sınırları da CPU ve bellek için tanımlanır.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.15.2
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: 250m
            memory: 64Mi
          limits:
            cpu: 500m
            memory: 256Mi
```

Yaml manifestosuna yük dengeleyicileri gibi hizmetler de dahil edilerek daha karmaşık uygulamalar oluşturulabilir.

Daha fazla bilgi için [Kubernetes dağıtımlarına][kubernetes-deployments]bakın.

### <a name="package-management-with-helm"></a>Helm ile paket yönetimi

Kubernetes uygulamaları yönetmek için ortak bir yaklaşım [Helm][helm]ile. Kaynakları dağıtmak için uygulama kodunun ve Kubernetes YAML bildirimlerinin paketlenmiş bir sürümünü içeren varolan genel Miğfer *grafiklerini* oluşturabilir ve kullanabilirsiniz. Bu Miğfer grafikleri yerel olarak veya genellikle Azure Kapsayıcı Kayıt [Defteri Miğferi grafiği reposu][acr-helm]gibi uzak bir depoda depolanabilir.

Helm'i kullanmak için, Kubernetes kümenizde *Tiller* adlı bir sunucu bileşeni yüklenir. Tiller küme içinde grafiklerin yüklenmesini yönetir. Helm istemcisinin kendisi bilgisayarınıza yerel olarak yüklenir veya [Azure Bulut BulutU][azure-cloud-shell]içinde kullanılabilir. İstemci ile Miğfer grafiklerini arayabilir veya oluşturabilir ve bunları Kubernetes kümenize yükleyebilirsiniz.

![Miğfer, Bir istemci bileşeni ve Kubernetes kümesi içinde kaynak oluşturan sunucu tarafı çapa makinesi bileşeni içerir](media/concepts-clusters-workloads/use-helm.png)

Daha fazla bilgi için Azure [Kubernetes Hizmetinde (AKS) Helm ile Uygulama Yükle'ye][aks-helm]bakın.

## <a name="statefulsets-and-daemonsets"></a>StatefulSets ve DaemonSets

Dağıtım Denetleyicisi, kullanılabilir kaynaklara sahip herhangi bir düğümde belirli sayıda yineleme çalıştırmak için Kubernetes Zamanlayıcısı'nı kullanır. Dağıtımları kullanma nın bu yaklaşımı, devletsiz uygulamalar için yeterli olabilir, ancak kalıcı bir adlandırma kuralı veya depolama gerektiren uygulamalar için yeterli olmayabilir. Bir küme içinde her düğümde veya seçili düğümlerde yineleme olmasını gerektiren uygulamalarda Dağıtım Denetleyicisi yinelemelerin düğümler arasında nasıl dağıtıldığına bakmaz.

Bu tür uygulamaları yönetmenize izin veren iki Kubernetes kaynağı vardır:

- *StatefulSets* - Depolama gibi tek bir pod yaşam döngüsünün ötesinde ki uygulamaların durumunu koruyun.
- *DaemonSets* - Kubernetes bootstrap işleminin başlarında, her düğüm üzerinde çalışan bir örnek olun.

### <a name="statefulsets"></a>StatefulSets

Modern uygulama geliştirme genellikle devletsiz uygulamaları hedefler, ancak *StatefulSets* veritabanı bileşenleri içeren uygulamalar gibi durumsal uygulamalar için kullanılabilir. StatefulSet, bir veya daha fazla özdeş bölmenin oluşturulduğu ve yönetildiği dağıtıma benzer. StatefulSet'teki yinelemeler dağıtım, ölçeklendirme, yükseltmeler ve sonlandırmalar için zarif ve sıralı bir yaklaşım izler. StatefulSet ile (yinelemeler yeniden zamanlanırken) adlandırma kuralı, ağ adları ve depolama devam eder.

Uygulamayı YAML biçiminde kullanarak `kind: StatefulSet`tanımlarsınız ve StatefulSet Denetleyicisi daha sonra gerekli yinelemelerin dağıtımını ve yönetimini işler. Veriler, Azure Yönetilen Diskler veya Azure Dosyaları tarafından sağlanan kalıcı depolama alanına yazılır. StatefulSets ile, StatefulSet silindiğinde bile altta yatan kalıcı depolama kalır.

Daha fazla bilgi için [Kubernetes StatefulSets'e][kubernetes-statefulsets]bakın.

StatefulSet'teki yinelemeler zamanlanır ve aks kümesindeki kullanılabilir düğümüzerinde çalıştırılır. Setinizdeki en az bir bölmenin düğüm üzerinde çalıştığından emin olmanız gerekiyorsa, bunun yerine bir DaemonSet kullanabilirsiniz.

### <a name="daemonsets"></a>DaemonSets

Belirli günlük toplama veya izleme gereksinimleri için, belirli bir bölmeyi tüm veya seçili düğümlerde çalıştırmanız gerekebilir. *DaemonSet* yine bir veya daha fazla özdeş bölmedağıtmak için kullanılır, ancak DaemonSet Denetleyici belirtilen her düğümün bölmenin bir örneğini çalıştırmasını sağlar.

DaemonSet Denetleyicisi, varsayılan Kubernetes zamanlayıcısı başlamadan önce küme önyükleme işleminin başlarında düğümler üzerinde bölmeleri zamanlayabilir. Bu yetenek, Dağıtım veya StatefulSet'teki geleneksel bölmeler zamanlanmadan önce DaemonSet'teki bölmelerin başlatılmasını sağlar.

StatefulSets gibi, Bir DaemonSet kullanarak `kind: DaemonSet`bir YAML tanımının bir parçası olarak tanımlanır.

Daha fazla bilgi için [Kubernetes DaemonSets'e][kubernetes-daemonset]bakın.

> [!NOTE]
> [Sanal Düğümler eklentisini](virtual-nodes-cli.md#enable-virtual-nodes-addon)kullanıyorsanız, DaemonSets sanal düğümüzerinde bölme oluşturmaz.

## <a name="namespaces"></a>Ad Alanları

Bölmeler ve Dağıtımlar gibi Kubernetes kaynakları mantıksal olarak bir *ad alanı*olarak gruplandırılır. Bu gruplandırmalar, aks kümesini mantıksal olarak bölmenin ve kaynakları oluşturmak, görüntülemek veya yönetmek için erişimi kısıtlamanın bir yolunu sağlar. Örneğin, iş gruplarını ayırmak için ad alanları oluşturabilirsiniz. Kullanıcılar yalnızca kendilerine atanan ad alanlarındaki kaynaklarla etkileşim kurabilir.

![Kaynakları ve uygulamaları mantıksal olarak bölmek için kubernetes ad alanları](media/concepts-clusters-workloads/namespaces.png)

Bir AKS kümesi oluşturduğunuzda, aşağıdaki ad alanları kullanılabilir:

- *varsayılan* - Bu ad alanı, bölmelerin ve dağıtımların hiçbiri sağlanmadığında varsayılan olarak oluşturulduğu yerdir. Daha küçük ortamlarda, ek mantıksal ayrımlar oluşturmadan uygulamaları doğrudan varsayılan ad alanına dağıtabilirsiniz. Kubernetes API ile etkileşimde olduğunuzda, `kubectl get pods`örneğin , varsayılan ad alanı belirtilmediğinde kullanılır.
- *kube-system* - Bu ad alanı, DNS ve proxy veya Kubernetes panosu gibi ağ özellikleri gibi temel kaynakların bulunduğu yerdir. Genellikle kendi uygulamalarınızı bu ad alanına dağıtmazsınız.
- *kube-public* - Bu ad alanı genellikle kullanılmaz, ancak kaynakların tüm küme de görülebilir için kullanılabilir ve herhangi bir kullanıcı tarafından görüntülenebilir.

Daha fazla bilgi için [Kubernetes ad alanlarına][kubernetes-namespaces]bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bazı çekirdek Kubernetes bileşenleri ve nasıl AKS kümeleri için geçerli kapsar. Çekirdek Kubernetes ve AKS kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Kubernetes / AKS erişim ve kimlik][aks-concepts-identity]
- [Kubernetes / AKS güvenlik][aks-concepts-security]
- [Kubernetes / AKS sanal ağlar][aks-concepts-network]
- [Kubernetes / AKS depolama][aks-concepts-storage]
- [Kubernetes / AKS ölçeği][aks-concepts-scale]

<!-- EXTERNAL LINKS -->
[aks-engine]: https://github.com/Azure/aks-engine
[kubernetes-pods]: https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/
[kubernetes-pod-lifecycle]: https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/
[kubernetes-deployments]: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
[kubernetes-statefulsets]: https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/
[kubernetes-daemonset]: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/
[kubernetes-namespaces]: https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/
[helm]: https://helm.sh/
[azure-cloud-shell]: https://shell.azure.com

<!-- INTERNAL LINKS -->
[aks-concepts-identity]: concepts-identity.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-network]: concepts-network.md
[acr-helm]: ../container-registry/container-registry-helm-repos.md
[aks-helm]: kubernetes-helm.md
[operator-best-practices-cluster-security]: operator-best-practices-cluster-security.md
[operator-best-practices-scheduler]: operator-best-practices-scheduler.md
[use-multiple-node-pools]: use-multiple-node-pools.md
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[reservation-discounts]:../cost-management-billing/reservations/save-compute-costs-reservations.md
