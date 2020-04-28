---
title: .NET Core ve Visual Studio kullanarak takım geliştirme
services: azure-dev-spaces
ms.custom: vs-azure
ms.workload: azure-vs
author: DrEsteban
ms.author: stevenry
ms.date: 12/09/2018
ms.topic: tutorial
description: Bu öğreticide, Azure Kubernetes hizmetinde .NET Core uygulamasında takım geliştirme yapmak için Azure Dev Spaces ve Visual Studio 'Nun nasıl kullanılacağı gösterilmektedir
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s '
ms.openlocfilehash: c84c77fe7a425318700903427ff1c4aaa4e73a11
ms.sourcegitcommit: 6a4fbc5ccf7cca9486fe881c069c321017628f20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "82166045"
---
# <a name="team-development-using-net-core-and-visual-studio-with-azure-dev-spaces"></a>Azure Dev Spaces ile .NET Core ve Visual Studio kullanarak takım geliştirme

Bu öğreticide, geliştiricilerin bir ekibinin geliştirme alanlarını kullanarak aynı Kubernetes kümesinde eşzamanlı olarak nasıl işbirliği yapabildiği hakkında bilgi edineceksiniz.

## <a name="learn-about-team-development"></a>Ekip geliştirmesi hakkında bilgi edinme

Şu ana kadar uygulamanızın kodunu uygulama üzerinde çalışan tek geliştiriciymişsiniz gibi çalıştırdınız. Bu bölümde, Azure Dev Spaces’ın ekip geliştirmesini nasıl kolaylaştırdığını öğreneceksiniz:
* Bir geliştiricilerin bir ekibini, paylaşılan bir geliştirme alanında ya da gerektiğinde ayrı geliştirme alanlarında çalışarak aynı ortamda çalışmasını sağlama.
* Her geliştiricinin yalıtılmış olarak ve başkalarını bölme korkusu olmadan kodlarını yinelemelerini destekler.
* Kod işlemeden önce sahtelerini yaratmaya veya bağımlılıkları benzetmeye gerek kalmadan kodu uçtan uca test edin.

### <a name="challenges-with-developing-microservices"></a>Mikro hizmet geliştirme zorlukları
Örnek uygulamanız Şu anda karmaşık değil. Ancak gerçek dünyada geliştirme sırasında daha çok hizmet ekledikçe ve geliştirme ekibi büyüdükçe zorluklar kısa sürede ortaya çıkar.

* Geliştirme makinenizin her seferinde ihtiyacınız olan her hizmeti çalıştırmak için yeterli kaynak bulunmayabilir.
* Bazı hizmetlerin herkese açık bir şekilde erişilebilir olması gerekebilir. Örneğin, bir hizmetin bir Web kancasına yanıt veren bir uç noktaya sahip olması gerekebilir.
* Hizmetlerin bir alt kümesini çalıştırmak istiyorsanız, tüm hizmetlerinize ilişkin tam bağımlılık hiyerarşisini bilmeniz gerekir. Özellikle hizmet numaranız artdıkça bu hiyerarşinin belirlenmesi zor olabilir.
* Bazı geliştiriciler, hizmet bağımlılıklarının birçoğunu benzetmeye veya bu bağımlılıkların sahtelerini oluşturmaya başvurur. Bu yaklaşım yardımcı olabilir ancak bu hareketleri yönetmek, geliştirme maliyetini yakında etkileyebilir. Ayrıca, bu yaklaşım, geliştirme ortamınızın üretimden farklı olmasına yol açar ve bu da hafif hatalara neden olabilir.
* Her türlü tümleştirme testini yapmak zor olur. Tümleştirme testi yalnızca yürütme sonrası gerçekleşebilir, bu da geliştirme döngüsünün sonraki aşamalarında sorunlarla karşılaşacağınız anlamına gelir.

    ![](media/common/microservices-challenges.png)

### <a name="work-in-a-shared-dev-space"></a>Paylaşılan geliştirme alanında çalışma
Azure Dev Spaces ile, Azure’da *paylaşılan* bir geliştirme alanı ayarlayabilirsiniz. Her geliştirici, uygulamanın yalnızca kendisine ayrılan kısmıyla ilgilenebilir ve senaryolarının bağımlı olduğu diğer tüm hizmetleri ve bulut kaynaklarını barındırmakta olan bir geliştirme alanında yinelemeli olarak *yürütme öncesi kod* geliştirebilir. Bağımlılıklar her zaman günceldir ve geliştiriciler üretimi yansıtan bir şekilde çalışır.

### <a name="work-in-your-own-space"></a>Kendi alanınızda çalışma
Hizmetiniz için kod geliştirirken ve kodu iade etmeye hazır olmadan önce, kodun iyi durumda olmadığı zamanlar olacaktır. Hala yinelemeli olarak şekillendiriyor, test ediyor ve çözümlerle deney yapıyorsunuz. Azure Dev Spaces, ekip üyelerinizi bölme korkusu olmadan yalıtılmış olarak çalışmanızı sağlayan **alan** kavramını sunar.

## <a name="use-dev-spaces-for-team-development"></a>Takım geliştirmesi için geliştirme alanlarını kullanma
*Webön uç* -> *mywebapi* örnek uygulamanızı kullanarak bu fikirleri somut bir örnekle gösterelim. Bir geliştiricinin, Scott, *mywebapi* hizmetinde değişiklik yapması gereken bir senaryo ve *yalnızca* bu hizmet. *Web ön* ucu Scott 'ın güncelleştirmesinin bir parçası olarak değişikliğe gerek kalmaz.

Geliştirici _alanlarını kullanmadan Scott_ , bu güncelleştirmeyi geliştirmek ve test etmek için bazı yöntemlere sahiptir ve bunlardan hiçbiri idealdir:
* Docker yüklü ve potansiyel olarak MiniKube daha güçlü bir geliştirme makinesi gerektiren tüm bileşenleri yerel olarak çalıştırın.
* Tüm bileşenleri Kubernetes kümesinde yalıtılmış bir ad alanında çalıştırın. *Web ön* ucu değişmediğinden, yalıtılmış bir ad alanı kullanmak küme kaynaklarının bir atık olur.
* YALNıZCA *mywebapi*' i çalıştırın ve El Ile yapılan Rest çağrılarını test edin. Bu test türü, uçtan uca tam akışı test etmez.
* Geliştiricilerin farklı *mywebapi*örneğine istek göndermesini sağlayan *Web ön* ucunda geliştirme odaklı kod ekleyin. Bu kodun eklenmesi *Web ön uç* hizmetini karmaşıklaştırır.

### <a name="set-up-your-baseline"></a>Taban çizgisini ayarlama
İlk olarak hizmetlerimizin temelini dağıtmamız gerekecektir. Bu dağıtım, "bilinen son iyi" kullanımı temsil eder; böylece yerel kodunuzun davranışını ve iade edilen sürümü ile kolayca karşılaştırabilirsiniz. Daha sonra bu taban çizgisine göre bir alt alan oluşturacak ve daha büyük bir uygulama bağlamında *mywebapi* 'teki değişiklikleri test edebilmeniz için

1. [Geliştirici alanları örnek uygulamasını](https://github.com/Azure/dev-spaces)kopyalayın:`git clone https://github.com/Azure/dev-spaces && cd dev-spaces`
1. Uzak dala *azds_updates*göz atın:`git checkout -b azds_updates origin/azds_updates`
1. Her iki hizmetin de F5/hata ayıklama oturumunu kapatın, ancak projeleri Visual Studio pencerelerinde açık tutun.
1. _Mywebapi_ projesiyle Visual Studio penceresine geçiş yapın.
1. **Çözüm Gezgini**’nde projeye sağ tıklayın ve **Özellikler**’i seçin.
1. Azure Dev Spaces ayarlarını göstermek için sol tarafta bulunan **Hata ayıkla** sekmesini seçin.
1. F5 veya CTRL + F5 tuşuna bastığınızda kullanılacak alanı oluşturmak için **Değiştir** ' i seçin.
1. Boşluk açılan menüsünde ** \<yeni alan oluştur... öğesini seçin. \>**.
1. Üst alanın ** \<None\>** olarak ayarlandığından emin olun ve alan adı **dev**' i girin. Tamam'a tıklayın.
1. Hata ayıklayıcı ekli olmadan _mywebapi_ çalıştırmak için CTRL + F5 tuşlarına basın.
1. _Web ön uç_ projesiyle Visual Studio penceresine geçin ve aynı zamanda çalıştırmak için CTRL + F5 tuşlarına basın.

> [!Note]
> Ctrl+F5’e bastıktan sonra web sayfasının ilk olarak görüntülenmesinin ardından tarayıcınızı bazen yenilemeniz gerekir.

> [!TIP]
> Yukarıdaki adımlar el ile bir taban çizgisi ayarlar, ancak ekiplerin ayrılmış kodla otomatik olarak güncel tutulması için CI/CD kullanmasını öneririz.
>
> Aşağıdaki diyagrama benzer bir iş akışı oluşturmak için [Azure DevOps Ile CI/CD ayarlamaya yönelik kılavuzumuza](how-to/setup-cicd.md) göz atın.
>
> ![Örnek CI/CD diyagramı](media/common/ci-cd-complex.png)

Genel URL 'YI açan ve Web uygulamasına giden herkes, varsayılan _geliştirme_ alanını kullanarak her iki hizmet üzerinden çalışan yazdığınız kod yolunu çağırır. Şimdi *mywebapi* geliştirmeye devam etmek istediğinizi varsayalım. bunu nasıl yapabilir ve dev alanını kullanan diğer geliştiricileri kesintiye uğramayın. Bunu yapmak için kendi alanınızı ayarlarsınız.

### <a name="create-a-new-dev-space"></a>Yeni bir geliştirme alanı oluşturma
Visual Studio içinde, hizmetinizi F5 veya Ctrl+F5 tuşlarıyla yenilediğinizde kullanılacak olan ek alanları oluşturabilirsiniz. Alana istediğiniz adı verebilir ve ne anlama geldiği konusunda esnek davranabilirsiniz (ör. _sprint4_ veya _demo_).

Yeni bir alan oluşturmak için aşağıdakileri yapın:
1. *Mywebapi* projesiyle Visual Studio penceresine geçiş yapın.
2. **Çözüm Gezgini**’nde projeye sağ tıklayın ve **Özellikler**’i seçin.
3. Azure Dev Spaces ayarlarını göstermek için sol tarafta bulunan **Hata ayıkla** sekmesini seçin.
4. Buradan, F5 veya Ctrl+F5 tuşlarına basarken kullanılacak olan küme ve/veya alanı değiştirebilir ya da oluşturabilirsiniz. *Daha önce oluşturduğunuz Azure Dev Space’in seçildiğinden emin olun*.
5. Boşluk açılan menüsünde ** \<yeni alan oluştur... öğesini seçin. \>**.

    ![](media/get-started-netcore-visualstudio/Settings.png)

6. **Boşluk Ekle** iletişim kutusunda, üst alanı **dev**olarak ayarlayın ve yeni alanınız için bir ad girin. İş arkadaşlarınızın hangi alanda çalıştığınızı tanımlayabilmesi adına yeni alan için adınızı kullanabilirsiniz (örneğin, "scott"). **Tamam**'a tıklayın.

    ![](media/get-started-netcore-visualstudio/AddSpace.png)

7. Artık proje özellikleri sayfasında AKS kümenizi ve seçili yeni Alanı görmeniz gerekir.

    ![](media/get-started-netcore-visualstudio/Settings2.png)

### <a name="update-code-for-mywebapi"></a>*mywebapi* için güncelleştirme kodu

1. *Mywebapi* projesinde, dosyadaki `string Get(int id)` `Controllers/ValuesController.cs` yöntemine aşağıdaki gibi bir kod değişikliği yapın:
 
    ```csharp
    [HttpGet("{id}")]
    public string Get(int id)
    {
        return "mywebapi now says something new";
    }
    ```

2. Bu güncelleştirilmiş kod bloğunda bir kesme noktası ayarlayın (zaten önceden ayarladığınız bir kesme noktanız olabilir).
3. _Mywebapi_ hizmetini başlatmak için F5 tuşuna basın, bu işlem, seçili alanı kullanarak kümenizdeki hizmeti başlatır. Bu örnekte seçilen alan _Scott_'dir.

Aşağıda, farklı alanların nasıl çalıştığını anlamanıza yardımcı olacak bir diyagram verilmiştir. Mor yol, URL 'ye hiçbir boşluk yoksa kullanılan varsayılan yol olan _geliştirme_ alanı aracılığıyla bir isteği gösterir. Pembe yol, _geliştirme/Scott_ alanı aracılığıyla bir isteği gösterir.

![](media/common/Space-Routing.png)

Azure Dev Spaces’ın bu yerleşik özelliği, her bir geliştiricinin alanlarındaki hizmetlerin tam yığınını yeniden oluşturmasına gerek kalmadan kodu paylaşılan bir ortamda uçtan uca test etmenize olanak sağlar. Bu yönlendirme, bu kılavuzun önceki adımında gösterildiği gibi yayma üst bilgilerinin uygulama kodunuzda iletilmesini gerektirir.

### <a name="test-code-running-in-the-_devscott_-space"></a>_Geliştirme/Scott_ alanında çalışan test kodu
*Web ön*ucu ile birlikte yeni *mywebapi* sürümünüzü test etmek için tarayıcınızı *Web ön* ucu için genel erişim noktası URL 'sine (örneğin, `http://dev.webfrontend.123456abcdef.eus.azds.io`) açın ve hakkında sayfasına gidin. "Hello from webfrontend and Hello from mywebapi" özgün iletisini görmelisiniz.

Şimdi de http\://Scott.s.dev.webfrontend.123456abcdef.EUS.azds.io gibi bir şeyi okuyup tarayıcıyı YENILEMEK için URL 'nin bir parçası. *Mywebapi* projenizde ayarladığınız kesme noktasına isabet almalısınız. Devam etmek için F5’e tıkladığınızda tarayıcınızda yeni "Hello from webfrontend and mywebapi now says something new." iletisini artık görmeniz gerekir. Bunun nedeni, *mywebapi* içindeki güncelleştirilmiş kodunuzun yolunun _dev/Scott_ alanında çalışıyor olmasından kaynaklanır.

En son değişiklerinizi her zaman içeren bir _geliştirme_ alanınız olduğunda ve uygulamanızın bu öğretici bölümünde açıklandığı gibi devspace 'in alan tabanlı yönlendirmesinden yararlanmak üzere tasarlandığına ilişkin olarak, geliştirme alanlarının daha büyük uygulama bağlamında yeni özellikleri test ederken büyük ölçüde nasıl yardımcı olduğunu görmek daha kolay hale gelir. _Tüm_ Hizmetleri özel alanınıza dağıtmak zorunda kalmak yerine, _geliştirme_ve gerçekten üzerinde çalıştığınız hizmetlerden oluşan özel bir alan oluşturabilirsiniz. Geliştirme alanları yönlendirme altyapısı, bulma sırasında özel alanınızda çok sayıda hizmetten yararlanarak bu geri kalanı işleyerek, _geliştirme_ alanında çalışan en son sürüme doğru bir şekilde geri dönüş yapabilir. Daha iyi bir şekilde, _birden çok_ geliştirici, birbirini kesintiye uğratmadan kendi alanında aynı anda farklı hizmetler geliştirebilir.

### <a name="well-done"></a>Bravo!
Başlangıç kılavuzunu tamamladınız! Şunları öğrendiniz:

> [!div class="checklist"]
> * Azure'da yönetilen bir Kubernetes ile Azure Dev Spaces'ı ayarlayın.
> * Kapsayıcılarda yinelemeli kod geliştirin.
> * İki ayrı hizmeti bağımsız olarak geliştirin ve Kubernetes’in DNS hizmet bulma yöntemini kullanarak başka bir hizmete çağrı yapın.
> * Kodunuzu bir ekip ortamında verimli bir şekilde geliştirip test edin.
> * Daha büyük bir mikro hizmet uygulaması bağlamı içindeki yalıtılmış değişiklikleri kolayca test etmek için geliştirme alanlarını kullanarak bir işlev temeli oluşturun

Artık Azure Dev Spaces'i öğrendiğinize göre, [Dev Space'inizi bir takım üyesiyle paylaşın](how-to/share-dev-spaces.md) ve onların birlikte işbirliği yapmasının ne kadar kolay olduğunu görmelerine yardımcı olun.

## <a name="clean-up"></a>Temizleme
Geliştirme alanları ve içinde çalışan hizmetler dahil olmak üzere bir kümedeki Azure Dev Spaces örneğini tamamen silmek için `az aks remove-dev-spaces` komutunu kullanın. Bu eylemin geri alınamayacağını unutmayın. İleride kümeye yeniden Azure Dev Spaces desteği ekleyebilirsiniz ancak sıfırdan başlamış gibi olursunuz. Eski hizmetleriniz ve alanlarınız geri yüklenmez.

Aşağıdaki örnek etkin aboneliğinizdeki Azure Dev Spaces denetleyicilerini listeler ve ardından 'myaks-rg' kaynak grubundaki 'myaks' AKS kümesiyle ilişkilendirilmiş Azure Dev Spaces denetleyicisini siler.

```cmd
azds controller list
```

```azurecli
az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```
