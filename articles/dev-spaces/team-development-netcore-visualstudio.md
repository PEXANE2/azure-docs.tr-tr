---
title: .NET Core ve Visual Studio kullanarak ekip geliştirme
services: azure-dev-spaces
ms.custom: vs-azure
ms.workload: azure-vs
author: DrEsteban
ms.author: stevenry
ms.date: 12/09/2018
ms.topic: tutorial
description: Bu öğretici, Azure Kubernetes Hizmeti'ndeki bir .NET Core uygulamasında ekip geliştirme yapmak için Azure Dev Spaces ve Visual Studio'yu nasıl kullanacağınızı gösterir
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, Miğfer, servis kafesi, servis örgü yönlendirme, kubectl, k8s '
ms.openlocfilehash: b4520ce35807fb022fa39ae9b00347a27e192380
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78245042"
---
# <a name="team-development-using-net-core-and-visual-studio-with-azure-dev-spaces"></a>Azure Dev Spaces ile .NET Core ve Visual Studio'yu kullanarak ekip geliştirme

Bu eğitimde, geliştirme boşluklarını kullanarak geliştiricilerden oluşan bir ekibin aynı Kubernetes kümesinde aynı anda nasıl işbirliği yapabildiğini öğreneceksiniz.

## <a name="learn-about-team-development"></a>Ekip geliştirmesi hakkında bilgi edinme

Şu ana kadar uygulamanızın kodunu uygulama üzerinde çalışan tek geliştiriciymişsiniz gibi çalıştırdınız. Bu bölümde, Azure Dev Spaces’ın ekip geliştirmesini nasıl kolaylaştırdığını öğreneceksiniz:
* Geliştiricilerden oluşan bir ekibin paylaşılan bir geliştirme alanında veya gerektiğinde farklı geliştirme alanlarında çalışarak aynı ortamda çalışmasını etkinleştirin.
* Her geliştiricinin yalıtılmış olarak ve başkalarını bölme korkusu olmadan kodlarını yinelemelerini destekler.
* Kod işlemeden önce sahtelerini yaratmaya veya bağımlılıkları benzetmeye gerek kalmadan kodu uçtan uca test edin.

### <a name="challenges-with-developing-microservices"></a>Mikro hizmet geliştirme zorlukları
Örnek uygulamanız şu anda karmaşık değil. Ancak gerçek dünyada geliştirme sırasında daha çok hizmet ekledikçe ve geliştirme ekibi büyüdükçe zorluklar kısa sürede ortaya çıkar.

* Geliştirme makineniz, gereksinim duyduğunuz her hizmeti aynı anda çalıştırmak için yeterli kaynağa sahip olmayabilir.
* Bazı hizmetlere genel olarak erişilebilen olması gerekebilir. Örneğin, bir hizmetin bir webhook'a yanıt veren bir bitiş noktası olması gerekebilir.
* Bir hizmet alt kümesini çalıştırmak istiyorsanız, tüm hizmetleriniz arasındaki tam bağımlılık hiyerarşisini bilmeniz gerekir. Bu hiyerarşiyi belirlemek, özellikle hizmet sayınız arttıkça zor olabilir.
* Bazı geliştiriciler, hizmet bağımlılıklarının birçoğunu benzetmeye veya bu bağımlılıkların sahtelerini oluşturmaya başvurur. Bu yaklaşım yardımcı olabilir, ancak bu mocks yönetmek yakında geliştirme maliyetini etkileyebilir. Ayrıca, bu yaklaşım geliştirme ortamının üretimden farklı görünmesine yol açar ve bu da ince hataların oluşmasına yol açabilir.
* Bu entegrasyon testi her türlü yapmanın zor hale gelir izler. Tümleştirme testi yalnızca yürütme sonrası gerçekleşebilir, bu da geliştirme döngüsünün sonraki aşamalarında sorunlarla karşılaşacağınız anlamına gelir.

    ![](media/common/microservices-challenges.png)

### <a name="work-in-a-shared-dev-space"></a>Paylaşılan geliştirme alanında çalışma
Azure Dev Spaces ile, Azure’da *paylaşılan* bir geliştirme alanı ayarlayabilirsiniz. Her geliştirici, uygulamanın yalnızca kendisine ayrılan kısmıyla ilgilenebilir ve senaryolarının bağımlı olduğu diğer tüm hizmetleri ve bulut kaynaklarını barındırmakta olan bir geliştirme alanında yinelemeli olarak *yürütme öncesi kod* geliştirebilir. Bağımlılıklar her zaman günceldir ve geliştiriciler üretimi yansıtan bir şekilde çalışır.

### <a name="work-in-your-own-space"></a>Kendi alanınızda çalışma
Hizmetiniz için kod geliştirirken ve kodu iade etmeye hazır olmadan önce, kodun iyi durumda olmadığı zamanlar olacaktır. Hala yinelemeli olarak şekillendiriyor, test ediyor ve çözümlerle deney yapıyorsunuz. Azure Dev Spaces, ekip üyelerinizi bölme korkusu olmadan yalıtılmış olarak çalışmanızı sağlayan **alan** kavramını sunar.

## <a name="use-dev-spaces-for-team-development"></a>Takım geliştirme için Dev Spaces'i kullanma
*Webfrontend* -> *mywebapi* örnek uygulamamızı kullanarak bu fikirleri somut bir örnekle gösterelim. Bir geliştirici, Scott, *mywebapi* hizmeti bir değişiklik yapmak gerekiyor bir senaryo hayal edeceğiz, ve *sadece* bu hizmet. *Webfrontend* Scott'ın güncelleştirmesinin bir parçası olarak değiştirmek gerekmez.

Dev Spaces _kullanmadan,_ Scott geliştirmek ve onun güncelleme test etmek için birkaç yolu olurdu, bunların hiçbiri ideal:
* Docker yüklü daha güçlü bir geliştirme makinesi gerektirir yerel olarak TÜM bileşenleri çalıştırın, ve potansiyel Olarak MiniKube.
* Tüm bileşenleri Kubernetes kümesinde yalıtılmış bir ad alanında çalıştırın. *Webfrontend* değişmediğinden, yalıtılmış bir ad alanı kullanmak küme kaynaklarının boşa harcadığını göstermektedir.
* SADECE *mywebapi*çalıştırın ve test etmek için manuel REST aramaları yapmak. Bu tür testler, uçtan uca akışı sınamaz.
* Geliştirici *mywebapi*farklı bir örneğine istek göndermek için izin *webfrontend* geliştirme odaklı kod ekleyin. Bu kodun eklenmesi *webfrontend* hizmetini zorlaştırır.

### <a name="set-up-your-baseline"></a>Taban çizginizi ayarlama
Önce hizmetlerimizin bir temelini dağıtmamız gerekecek. Bu dağıtım, yerel kodunuzu iade edilen sürümle kolayca karşılaştırabilmeniz için "bilinen son iyi" "bilinen iyiyi" temsil eder. Daha sonra bu temele dayalı bir alt alan oluşturacağız, böylece *mywebapi'deki* değişikliklerimizi daha büyük uygulama bağlamında test edebiliriz.

1. [Dev Spaces örnek uygulamasını](https://github.com/Azure/dev-spaces)klonla:`git clone https://github.com/Azure/dev-spaces && cd dev-spaces`
1. *azds_updates*uzak şubeye göz atın:`git checkout -b azds_updates origin/azds_updates`
1. Her iki hizmetin de F5/hata ayıklama oturumunu kapatın, ancak projeleri Visual Studio pencerelerinde açık tutun.
1. _Mywebapi_ projesiyle Visual Studio penceresine geçin.
1. **Çözüm Gezgini**’nde projeye sağ tıklayın ve **Özellikler**’i seçin.
1. Azure Dev Spaces ayarlarını göstermek için sol tarafta bulunan **Hata ayıkla** sekmesini seçin.
1. F5 veya Ctrl+F5 hizmetinizde kullanılacak alanı oluşturmak için **Değiştir'i** seçin.
1. Space açılır düşüşünde ** \<Yeni Alan Oluştur'u seçin... \>**.
1. Üst alanın ** \<\>hiçbirinde**ayarlandıklarına emin olun ve boşluk adı **dev'i**girin. Tamam'a tıklayın.
1. Hata ayıklama bağlı olmadan _mywebapi_ çalıştırmak için Ctrl+F5 tuşuna basın.
1. _Webfrontend_ projesiyle Visual Studio penceresine geçin ve çalıştırmak için Ctrl+F5 tuşuna basın.

> [!Note]
> Ctrl+F5’e bastıktan sonra web sayfasının ilk olarak görüntülenmesinin ardından tarayıcınızı bazen yenilemeniz gerekir.

> [!TIP]
> Yukarıdaki adımlar el ile bir taban çizgisi ayarlar, ancak takımların temel inizi taahhüt edilen kodla otomatik olarak güncel tutmak için CI/CD kullanmalarını öneririz.
>
> Aşağıdaki diyagrama benzer bir iş akışı oluşturmak için [Azure DevOps ile CI/CD ayarlama kılavuzumuza](how-to/setup-cicd.md) göz atın.
>
> ![Örnek CI/CD diyagramı](media/common/ci-cd-complex.png)

Herkese açık URL'yi açan ve web uygulamasına yönlendiren herkes, varsayılan _geliştirme_ alanını kullanarak her iki hizmette de çalışan yazdığınız kod yolunu çağırır. Şimdi *mywebapi* geliştirmeye devam etmek istediğinizi varsayalım - nasıl bunu yapabilir ve dev alanı kullanan diğer geliştiriciler kesintiye değil? Bunu yapmak için kendi alanınızı ayarlarsınız.

### <a name="create-a-new-dev-space"></a>Yeni bir geliştirme alanı oluşturma
Visual Studio içinde, hizmetinizi F5 veya Ctrl+F5 tuşlarıyla yenilediğinizde kullanılacak olan ek alanları oluşturabilirsiniz. Alana istediğiniz adı verebilir ve ne anlama geldiği konusunda esnek davranabilirsiniz (ör. _sprint4_ veya _demo)._

Yeni bir alan oluşturmak için aşağıdakileri yapın:
1. *Mywebapi* projesiyle Visual Studio penceresine geçin.
2. **Çözüm Gezgini**’nde projeye sağ tıklayın ve **Özellikler**’i seçin.
3. Azure Dev Spaces ayarlarını göstermek için sol tarafta bulunan **Hata ayıkla** sekmesini seçin.
4. Buradan, F5 veya Ctrl+F5 tuşlarına basarken kullanılacak olan küme ve/veya alanı değiştirebilir ya da oluşturabilirsiniz. *Daha önce oluşturduğunuz Azure Dev Space’in seçildiğinden emin olun*.
5. Space açılır düşüşünde ** \<Yeni Alan Oluştur'u seçin... \>**.

    ![](media/get-started-netcore-visualstudio/Settings.png)

6. Alan **Ekle** iletişim kutusunda, üst alanı **dev**olacak şekilde ayarlayın ve yeni alanınız için bir ad girin. İş arkadaşlarınızın hangi alanda çalıştığınızı tanımlayabilmesi adına yeni alan için adınızı kullanabilirsiniz (örneğin, "scott"). **Tamam**'a tıklayın.

    ![](media/get-started-netcore-visualstudio/AddSpace.png)

7. Artık proje özellikleri sayfasında AKS kümenizi ve seçili yeni Alanı görmeniz gerekir.

    ![](media/get-started-netcore-visualstudio/Settings2.png)

### <a name="update-code-for-mywebapi"></a>*mywebapi* için güncelleştirme kodu

1. *Mywebapi* projesinde aşağıdaki gibi dosyayönteminde `string Get(int id)` `Controllers/ValuesController.cs` bir kod değişikliği yapmak:
 
    ```csharp
    [HttpGet("{id}")]
    public string Get(int id)
    {
        return "mywebapi now says something new";
    }
    ```

2. Bu güncelleştirilmiş kod bloğunda bir kesme noktası ayarlayın (zaten önceden ayarladığınız bir kesme noktanız olabilir).
3. Seçilen alanı kullanarak kümenizdeki hizmeti başlatacak _olan mywebapi_ hizmetini başlatmak için F5 tuşuna basın. Bu durumda seçilen alan _scott'_ tır.

Aşağıda, farklı alanların nasıl çalıştığını anlamanıza yardımcı olacak bir diyagram verilmiştir. Mor yol, _URL'ye_ hiçbir alan hazırlanmamışsa kullanılan varsayılan yol olan dev alanı üzerinden bir istek gösterir. Pembe yol _dev/scott_ alanı üzerinden bir istek gösterir.

![](media/common/Space-Routing.png)

Azure Dev Spaces’ın bu yerleşik özelliği, her bir geliştiricinin alanlarındaki hizmetlerin tam yığınını yeniden oluşturmasına gerek kalmadan kodu paylaşılan bir ortamda uçtan uca test etmenize olanak sağlar. Bu yönlendirme, bu kılavuzun önceki adımında gösterildiği gibi yayma üst bilgilerinin uygulama kodunuzda iletilmesini gerektirir.

### <a name="test-code-running-in-the-_devscott_-space"></a>_Dev/scott_ alanında çalışan test kodu
*Mywebapi'nin* yeni sürümünü *webfrontend*ile birlikte test etmek için tarayıcınızı *webfrontend* için http://dev.webfrontend.123456abcdef.eus.azds.io) genel erişim noktası URL'sine açın (örneğin, Ve Hakkında sayfasına gidin. "Hello from webfrontend and Hello from mywebapi" özgün iletisini görmelisiniz.

Şimdi de http\://scott.s.dev.webfrontend.123456abcdef.eus.azds.io gibi bir şey okur ve tarayıcıyı yenilemek için URL'ye bir parçası. *Mywebapi* projenizde belirlediğiniz kırılma noktası vurulmalıdır. Devam etmek için F5’e tıkladığınızda tarayıcınızda yeni "Hello from webfrontend and mywebapi now says something new." iletisini artık görmeniz gerekir. Bunun nedeni, *mywebapi'deki* güncelleştirilmiş kodunuza giden yolun _dev/scott_ alanında çalışıyor olmasıdır.

Her zaman en son değişiklikleri içeren bir _dev_ alana sahip olduktan sonra ve uygulamanızın bu öğretici bölümde açıklandığı gibi DevSpace'in uzay tabanlı yönlendirmeden yararlanmak üzere tasarlandığını varsayarsak, umarım Dev Spaces'in daha büyük uygulama bağlamında yeni özellikleri test etmeye nasıl yardımcı olabileceğini görmek kolaylaşır. _Tüm_ hizmetleri özel alanınıza dağıtmak yerine, _dev'den_ve yalnızca üzerinde çalıştığınız hizmetlerden "yukarı" türeyen özel bir alan oluşturabilirsiniz. Dev Spaces yönlendirme altyapısı, geri kalanını, bulabildiği kadar çok hizmeti özel alanınızdan kullanarak, geri kalanı yla da ilgilenecek ve _dev_ alanda çalışan en son sürüye geri döner. Ve daha da iyisi, _birden fazla_ geliştirici aktif birbirlerini bozmadan kendi alanında aynı anda farklı hizmetler geliştirebilirsiniz.

### <a name="well-done"></a>Bravo!
Başlangıç kılavuzunu tamamladınız! Şunları öğrendiniz:

> [!div class="checklist"]
> * Azure'da yönetilen bir Kubernetes ile Azure Dev Spaces'ı ayarlayın.
> * Kapsayıcılarda yinelemeli kod geliştirin.
> * İki ayrı hizmeti bağımsız olarak geliştirin ve Kubernetes’in DNS hizmet bulma yöntemini kullanarak başka bir hizmete çağrı yapın.
> * Kodunuzu bir ekip ortamında verimli bir şekilde geliştirip test edin.
> * Daha büyük bir mikro hizmet uygulaması bağlamında yalıtılmış değişiklikleri kolayca test etmek için Dev Spaces'i kullanarak bir işlevsellik temeli oluşturma

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
