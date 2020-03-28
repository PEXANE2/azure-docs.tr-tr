---
title: Node.js ve Visual Studio Code kullanarak takım geliştirme
services: azure-dev-spaces
ms.date: 07/09/2018
ms.topic: tutorial
description: Bu öğretici, Azure Kubernetes Hizmeti'ndeki bir Düğüm uygulamasında takım geliştirme yapmak için Azure Dev Alanları ve Visual Studio Kodunu nasıl kullanacağınızı gösterir
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, Miğfer, servis kafesi, servis örgü yönlendirme, kubectl, k8s '
ms.openlocfilehash: abcf4934af056d508ac136f80758597294d40b1a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78251928"
---
# <a name="team-development-using-nodejs-and-visual-studio-code-with-azure-dev-spaces"></a>Azure Dev Spaces ile Node.js ve Visual Studio Code kullanarak takım geliştirme

Bu eğitimde, geliştirme boşluklarını kullanarak geliştiricilerden oluşan bir ekibin aynı Kubernetes kümesinde aynı anda nasıl işbirliği yapabildiğini öğreneceksiniz.

## <a name="learn-about-team-development"></a>Ekip geliştirmesi hakkında bilgi edinme
Şu ana kadar uygulamanızın kodunu uygulama üzerinde çalışan tek geliştiriciymişsiniz gibi çalıştırıyordunuz. Bu bölümde, Azure Dev Spaces’ın ekip geliştirmesini nasıl kolaylaştırdığını öğreneceksiniz:
* Geliştiricilerden oluşan bir ekibin paylaşılan bir geliştirme alanında veya gerektiğinde farklı geliştirme alanlarında çalışarak aynı ortamda çalışmasını etkinleştirin.
* Her geliştiricinin yalıtılmış olarak ve başkalarını bölme korkusu olmadan kodlarını yinelemelerini destekler.
* Kod işlemeden önce sahtelerini yaratmaya veya bağımlılıkları benzetmeye gerek kalmadan kodu uçtan uca test edin.

### <a name="challenges-with-developing-microservices"></a>Mikro hizmet geliştirme zorlukları
Örnek uygulamanız şu an için pek karmaşık değil. Ancak gerçek dünyada geliştirme sırasında daha çok hizmet ekledikçe ve geliştirme ekibi büyüdükçe zorluklar kısa sürede ortaya çıkar. Geliştirme için her şeyi yerel olarak çalıştırmak pek mantıklı olmayabilir.

* Geliştirme makineniz, gereksinim duyduğunuz her hizmeti aynı anda çalıştırmak için yeterli kaynağa sahip olmayabilir.
* Bazı hizmetlere genel olarak erişilebilen olması gerekebilir. Örneğin, bir hizmetin bir webhook'a yanıt veren bir bitiş noktası olması gerekebilir.
* Bir hizmet alt kümesini çalıştırmak istiyorsanız, tüm hizmetleriniz arasındaki tam bağımlılık hiyerarşisini bilmeniz gerekir. Özellikle hizmet sayınız arttıkça, bunu belirlemek zor olabilir.
* Bazı geliştiriciler, hizmet bağımlılıklarının birçoğunu benzetmeye veya bu bağımlılıkların sahtelerini oluşturmaya başvurur. Bu yaklaşım yardımcı olabilir, ancak bu mocks yönetmek yakında geliştirme maliyetini etkileyebilir. Ayrıca, bu yaklaşım geliştirme ortamınızı üretimden çok farklı görünmesine yol açar, bu da ince hataların sürünmesine olanak tanır.
* Bu entegrasyon testi her türlü yapmanın zor hale gelir izler. Tümleştirme testi yalnızca yürütme sonrası gerçekleşebilir, bu da geliştirme döngüsünün sonraki aşamalarında sorunlarla karşılaşacağınız anlamına gelir.

    ![](media/common/microservices-challenges.png)

### <a name="work-in-a-shared-dev-space"></a>Paylaşılan geliştirme alanında çalışma
Azure Dev Spaces ile, Azure’da *paylaşılan* bir geliştirme alanı ayarlayabilirsiniz. Her geliştirici, uygulamanın yalnızca kendisine ayrılan kısmıyla ilgilenebilir ve senaryolarının bağımlı olduğu diğer tüm hizmetleri ve bulut kaynaklarını barındırmakta olan bir geliştirme alanında yinelemeli olarak *yürütme öncesi kod* geliştirebilir. Bağımlılıklar her zaman günceldir ve geliştiriciler üretimi yansıtan bir şekilde çalışır.

### <a name="work-in-your-own-space"></a>Kendi alanınızda çalışma
Hizmetiniz için kod geliştirirken ve kodu iade etmeye hazır olmadan önce, kodun iyi durumda olmadığı zamanlar olacaktır. Hala yinelemeli olarak şekillendiriyor, test ediyor ve çözümlerle deney yapıyorsunuz. Azure Dev Spaces, ekip üyelerinizi bölme korkusu olmadan yalıtılmış olarak çalışmanızı sağlayan **alan** kavramını sunar.

## <a name="use-dev-spaces-for-team-development"></a>Takım geliştirme için Dev Spaces'i kullanma
*Webfrontend* -> *mywebapi* örnek uygulamamızı kullanarak bu fikirleri somut bir örnekle gösterelim. Bir geliştirici, Scott, *mywebapi* hizmeti bir değişiklik yapmak gerekiyor bir senaryo hayal edeceğiz, ve *sadece* bu hizmet. *Webfrontend* Scott'ın güncelleştirmesinin bir parçası olarak değiştirmek gerekmez.

Dev Spaces _kullanmadan,_ Scott geliştirmek ve onun güncelleme test etmek için birkaç yolu olurdu, bunların hiçbiri ideal:
* TÜM bileşenleri yerel olarak çalıştırın. Bu Docker yüklü daha güçlü bir geliştirme makinesi gerektirir, ve potansiyel Olarak MiniKube.
* Tüm bileşenleri Kubernetes kümesinde yalıtılmış bir ad alanında çalıştırın. *Webfrontend* değişmediğinden, bu küme kaynaklarının israfıdır.
* SADECE *mywebapi*çalıştırın ve test etmek için manuel REST aramaları yapmak. Bu, uça akışını test etmez.
* Geliştirici *mywebapi*farklı bir örneğine istek göndermek için izin *webfrontend* geliştirme odaklı kod ekleyin. Bu *webfrontend* hizmeti karmaşıklaştırır.

### <a name="set-up-your-baseline"></a>Taban çizginizi ayarlama
Önce hizmetlerimizin bir temelini dağıtmamız gerekecek. Bu dağıtım, yerel kodunuzu iade edilen sürümle kolayca karşılaştırabilmeniz için "bilinen son iyi" "bilinen iyiyi" temsil eder. Daha sonra bu temele dayalı bir alt alan oluşturacağız, böylece *mywebapi'deki* değişikliklerimizi daha büyük uygulama bağlamında test edebiliriz.

1. [Dev Spaces örnek uygulamasını](https://github.com/Azure/dev-spaces)klonla:`git clone https://github.com/Azure/dev-spaces && cd dev-spaces`
1. Çıkış uzak şube *azds_updates:*`git checkout -b azds_updates origin/azds_updates`
1. _Dev_ alanını seçin: `azds space select --name dev`. Bir üst dev alanı seçmek istendiğinde, _ \<\>hiçbiri'ni_seçin.
1. _Mywebapi_ dizinine gidin ve çalıştırın:`azds up -d`
1. _Webfrontend_ dizinine gidin ve çalıştırın:`azds up -d`
1. `azds list-uris` _Webfrontend_ için genel bitiş noktasını görmek için yürütme

> [!TIP]
> Yukarıdaki adımlar el ile bir taban çizgisi ayarlar, ancak takımların temel inizi taahhüt edilen kodla otomatik olarak güncel tutmak için CI/CD kullanmalarını öneririz.
>
> Aşağıdaki diyagrama benzer bir iş akışı oluşturmak için [Azure DevOps ile CI/CD ayarlama kılavuzumuza](how-to/setup-cicd.md) göz atın.
>
> ![Örnek CI/CD diyagramı](media/common/ci-cd-complex.png)

Bu noktada taban çizginiz çalışıyor olmalıdır. `azds list-up --all` komutunu çalıştırdığınızda aşağıdakine benzer bir çıktı görürsünüz:

```
$ azds list-up --all

Name                          DevSpace  Type     Updated  Status
----------------------------  --------  -------  -------  -------
mywebapi                      dev       Service  3m ago   Running
mywebapi-56c8f45d9-zs4mw      dev       Pod      3m ago   Running
webfrontend                   dev       Service  1m ago   Running
webfrontend-6b6ddbb98f-fgvnc  dev       Pod      1m ago   Running
```

DevSpace sütunu, her iki hizmetin _de dev_adlı bir alanda çalıştığını gösterir. Herkese açık URL'yi açan ve web uygulamasına yönlendiren herkes, her iki hizmette de çalışan iade edilmiş kod yolunu çağırır. Şimdi _mywebapi_geliştirmeye devam etmek istediğinizi varsayalım. Nasıl kod değişiklikleri yapabilir ve bunları test edebilir, öte yandan da geliştirme ortamını kullanan diğer geliştiricilerin işlemini kesintiye uğratmazsınız? Bunu yapmak için kendi alanınızı ayarlarsınız.

### <a name="create-a-dev-space"></a>Geliştirme alanı oluşturma
_Dev_dışındaki bir alanda _mywebapi_ kendi sürümünü çalıştırmak için, aşağıdaki komutu kullanarak kendi alanı oluşturabilirsiniz:

```cmd
azds space select --name scott
```

İstendiğinde, **üst dev alanı**olarak _dev'i_ seçin. Bu bizim yeni alan, _dev / scott_, uzay _dev_türetilecek anlamına gelir. Bu durumun test sırasında bize nasıl yardımcı olacağını birazdan göreceksiniz.

Giriş varsayımlarımıza uygun olarak, yeni alan için _Scott_ adını kullandık, böylece akranlar bu alanda kimin çalıştığını belirleyebilirler. Ama istediğiniz her şey çağrılabilir ve _sprint4_ veya _demo_gibi ne anlama geldiğini hakkında esnek olabilir. Ne olursa olsun, _dev_ bu uygulamanın bir parçası üzerinde çalışan tüm geliştiriciler için temel olarak hizmet vermektedir:

![](media/common/ci-cd-space-setup.png)

Geliştirme ortamındaki tüm alanların listesini görmek için `azds space list` komutunu çalıştırın. _Seçili_ sütun, şu anda hangi alanı seçtiğinizi (doğru/yanlış) gösterir. Sizin durumunuzda, _dev/scott_ adlı alan oluşturulduğunda otomatik olarak seçildi. İstediğiniz zaman `azds space select` komutuyla başka bir alan seçebilirsiniz.

Şimdi nasıl çalıştığını görelim.

### <a name="make-a-code-change"></a>Kod değişikliği yapma
Vs Code penceresine `mywebapi` gidin ve örneğin `/` `server.js`varsayılan GET işleyicisi için bir kod editi yapın:

```javascript
app.get('/', function (req, res) {
    res.send('mywebapi now says something new');
});
```

### <a name="run-the-service"></a>Hizmeti çalıştırma

Hizmeti çalıştırmak için, hizmeti çalıştırmak `azds up` için F5'e (veya Terminal Penceresine yazın) vurun. Hizmet otomatik olarak yeni seçilen boşluk _dev / scott_çalışacaktır. Hizmetinizin kendi alanında çalıştığını doğrulayın: `azds list-up`

```cmd
$ azds list-up

Name                      DevSpace  Type     Updated  Status
mywebapi                  scott     Service  3m ago   Running
webfrontend               dev       Service  26m ago  Running
```

*Mywebapi* bir örnek şimdi dev _/ scott_ alanda çalışıyor dikkat edin. _Dev'de_ çalışan sürüm hala çalışıyor, ancak listede yok.

Çalıştırarak geçerli alan için URL'leri listele. `azds list-uris`

```cmd
$ azds list-uris

Uri                                                                        Status
-------------------------------------------------------------------------  ---------
http://localhost:53831 => mywebapi.scott:80                                Tunneled
http://scott.s.dev.webfrontend.6364744826e042319629.ce.azds.io/  Available
```

*Webfrontend* için genel erişim noktası *URL'sinin scott.s*ile önceden belirlenmiş olduğuna dikkat edin. Bu URL _dev/scott_ alanına özgüdür. Bu URL öneki, Ingress denetleyicisine istekleri bir hizmetin _dev/scott_ sürümüne yönlendirmesini söyler. Bu URL'ye sahip bir istek Dev Spaces tarafından işlendiğinde, Giriş Denetleyicisi önce isteği _geliştirme/scott_ alanında *webfrontend* hizmetine yönlendirmeye çalışır. Bu başarısız olursa, istek bir geri dönüş olarak _dev_ alanında *webfrontend* hizmetine yönlendirilecektir. Ayrıca, Kubernetes *bağlantı noktası ileri etme* işlevini kullanarak yerel host üzerinden hizmete erişmek için bir localhost URL'si olduğunu da unutmayın. Azure Dev Spaces'te URL'ler ve yönlendirme hakkında daha fazla bilgi için [Azure Dev Spaces'in nasıl çalıştığını ve nasıl yapılandırıldığına](how-dev-spaces-works.md)bakın.

![Uzay Yönlendirme](media/common/Space-Routing.png)

Azure Dev Spaces’ın bu yerleşik özelliği, her geliştiricinin alanlarındaki hizmetlerin tam yığınını yeniden oluşturmasına gerek kalmadan kodu paylaşılan bir alanda test etmenize olanak sağlar. Bu yönlendirme, bu kılavuzun önceki adımında gösterildiği gibi uygulama kodunuzun yayma üst bilgilerini iletmesini gerektirir.

### <a name="test-code-in-a-space"></a>Alanda kodu test etme
*Mywebapi'nin* yeni sürümünü *webfrontend*ile test etmek için tarayıcınızı *webfrontend* için genel erişim noktası URL'sine açın ve Hakkında sayfasına gidin. Yeni iletinizin görüntülendiğini görmelisiniz.

Şimdi, URL'nin "scott.s." bölümünü kaldırın ve tarayıcıyı yenileyin. Eski davranışı görmelisiniz *(mywebapi* sürümü _dev_çalışan).

Her zaman en son değişikliklerinizi içeren bir _dev_ alanınız olduğunda ve uygulamanızın bu öğretici bölümde açıklandığı gibi DevSpace'in alan tabanlı yönlendirmeden yararlanmak üzere tasarlandığını varsayarsak, umarım Dev Spaces'in daha büyük uygulama bağlamında yeni özellikleri test etmeye nasıl yardımcı olabileceğini görmek kolaylaşır. _Tüm_ hizmetleri özel alanınıza dağıtmak yerine, _dev'den_ve yalnızca üzerinde çalıştığınız hizmetlerden "yukarı" türeyen özel bir alan oluşturabilirsiniz. Dev Spaces yönlendirme altyapısı, geri kalanını, bulabildiği kadar çok hizmeti özel alanınızdan kullanarak, geri kalanı yla da ilgilenecek ve _dev_ alanda çalışan en son sürüye geri döner. Ve daha da iyisi, _birden fazla_ geliştirici aktif birbirlerini bozmadan kendi alanında aynı anda farklı hizmetler geliştirebilirsiniz.

### <a name="well-done"></a>Bravo!
Başlangıç kılavuzunu tamamladınız! Şunları öğrendiniz:

> [!div class="checklist"]
> * Azure'da yönetilen bir Kubernetes ile Azure Dev Spaces'ı ayarlayın.
> * Kapsayıcılarda yinelemeli kod geliştirin.
> * İki ayrı hizmeti bağımsız olarak geliştirin ve Kubernetes’in DNS hizmet bulma yöntemini kullanarak başka bir hizmete çağrı yapın.
> * Kodunuzu bir ekip ortamında verimli bir şekilde geliştirip test edin.
> * Daha büyük bir mikro hizmet uygulaması bağlamında yalıtılmış değişiklikleri kolayca test etmek için Dev Spaces'i kullanarak bir işlevsellik temeli oluşturma

Azure Dev Alanlarını keşfettiğinize göre, [geliştirme alanınızı bir ekip üyesiyle paylaşın](how-to/share-dev-spaces.md) ve işbirliği yapmaya başlayın.

## <a name="clean-up"></a>Temizleme
Geliştirme alanları ve içinde çalışan hizmetler dahil olmak üzere bir kümedeki Azure Dev Spaces örneğini tamamen silmek için `az aks remove-dev-spaces` komutunu kullanın. Bu eylemin geri alınamayacağını unutmayın. İleride kümeye yeniden Azure Dev Spaces desteği ekleyebilirsiniz ancak sıfırdan başlamış gibi olursunuz. Eski hizmetleriniz ve alanlarınız geri yüklenmez.

Aşağıdaki örnek etkin aboneliğinizdeki Azure Dev Spaces denetleyicilerini listeler ve ardından 'myaks-rg' kaynak grubundaki 'myaks' AKS kümesiyle ilişkilendirilmiş Azure Dev Spaces denetleyicisini siler.

```cmd
azds controller list
```

```azurecli
az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```
