---
title: Team development using Node.js and Visual Studio Code
services: azure-dev-spaces
ms.date: 07/09/2018
ms.topic: tutorial
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, Helm, service mesh, service mesh routing, kubectl, k8s '
ms.openlocfilehash: 374a6f0944c7d2fe8d97ea2fa4610ba63598ee2e
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74325498"
---
# <a name="team-development-using-nodejs-and-visual-studio-code-with-azure-dev-spaces"></a>Team development using Node.js and Visual Studio Code with Azure Dev Spaces

In this tutorial, you'll learn how a team of developers can simultaneously collaborate in the same Kubernetes cluster using Dev Spaces.

## <a name="learn-about-team-development"></a>Ekip geliştirmesi hakkında bilgi edinme
Şu ana kadar uygulamanızın kodunu uygulama üzerinde çalışan tek geliştiriciymişsiniz gibi çalıştırıyordunuz. Bu bölümde, Azure Dev Spaces’ın ekip geliştirmesini nasıl kolaylaştırdığını öğreneceksiniz:
* Enable a team of developers to work in the same environment, by working in a shared dev space or in distinct dev spaces as needed.
* Her geliştiricinin yalıtılmış olarak ve başkalarını bölme korkusu olmadan kodlarını yinelemelerini destekler.
* Kod işlemeden önce sahtelerini yaratmaya veya bağımlılıkları benzetmeye gerek kalmadan kodu uçtan uca test edin.

### <a name="challenges-with-developing-microservices"></a>Mikro hizmet geliştirme zorlukları
Örnek uygulamanız şu an için pek karmaşık değil. Ancak gerçek dünyada geliştirme sırasında daha çok hizmet ekledikçe ve geliştirme ekibi büyüdükçe zorluklar kısa sürede ortaya çıkar. Geliştirme için her şeyi yerel olarak çalıştırmak pek mantıklı olmayabilir.

* Your development machine may not have enough resources to run every service you need at once.
* Some services may need to be publicly reachable. For example, a service may need to have an endpoint that responds to a webhook.
* If you want to run a subset of services, you have to know the full dependency hierarchy between all your services. Determining this can be difficult, especially as your number of services increase.
* Bazı geliştiriciler, hizmet bağımlılıklarının birçoğunu benzetmeye veya bu bağımlılıkların sahtelerini oluşturmaya başvurur. This approach can help, but managing those mocks can soon impact development cost. Plus, this approach leads to your development environment looking very different from production, which allows subtle bugs to creep in.
* It follows that doing any type of integration testing becomes difficult. Tümleştirme testi yalnızca yürütme sonrası gerçekleşebilir, bu da geliştirme döngüsünün sonraki aşamalarında sorunlarla karşılaşacağınız anlamına gelir.

    ![](media/common/microservices-challenges.png)

### <a name="work-in-a-shared-dev-space"></a>Paylaşılan geliştirme alanında çalışma
Azure Dev Spaces ile, Azure’da *paylaşılan* bir geliştirme alanı ayarlayabilirsiniz. Her geliştirici, uygulamanın yalnızca kendisine ayrılan kısmıyla ilgilenebilir ve senaryolarının bağımlı olduğu diğer tüm hizmetleri ve bulut kaynaklarını barındırmakta olan bir geliştirme alanında yinelemeli olarak *yürütme öncesi kod* geliştirebilir. Bağımlılıklar her zaman günceldir ve geliştiriciler üretimi yansıtan bir şekilde çalışır.

### <a name="work-in-your-own-space"></a>Kendi alanınızda çalışma
Hizmetiniz için kod geliştirirken ve kodu iade etmeye hazır olmadan önce, kodun iyi durumda olmadığı zamanlar olacaktır. Hala yinelemeli olarak şekillendiriyor, test ediyor ve çözümlerle deney yapıyorsunuz. Azure Dev Spaces, ekip üyelerinizi bölme korkusu olmadan yalıtılmış olarak çalışmanızı sağlayan **alan** kavramını sunar.

## <a name="use-dev-spaces-for-team-development"></a>Use Dev Spaces for team development
Let's demonstrate these ideas with a concrete example using our *webfrontend* -> *mywebapi* sample application. We'll imagine a scenario where a developer, Scott, needs to make a change to the *mywebapi* service, and *only* that service. The *webfrontend* won't need to change as part of Scott's update.

_Without_ using Dev Spaces, Scott would have a few ways to develop and test his update, none of which are ideal:
* Run ALL components locally. This requires a more powerful development machine with Docker installed, and potentially MiniKube.
* Run ALL components in an isolated namespace on the Kubernetes cluster. Since *webfrontend* isn't changing, this is a waste of cluster resources.
* ONLY run *mywebapi*, and make manual REST calls to test. This doesn't test the full end-to-end flow.
* Add development-focused code to *webfrontend* that allows the developer to send requests to a different instance of *mywebapi*. This complicates the *webfrontend* service.

### <a name="set-up-your-baseline"></a>Set up your baseline
First we'll need to deploy a baseline of our services. This deployment will represent the "last known good" so you can easily compare the behavior of your local code vs. the checked-in version. We'll then create a child space based on this baseline so we can test our changes to *mywebapi* within the context of the larger application.

1. Clone the [Dev Spaces sample application](https://github.com/Azure/dev-spaces): `git clone https://github.com/Azure/dev-spaces && cd dev-spaces`
1. Checkout the remote branch *azds_updates*: `git checkout -b azds_updates origin/azds_updates`
1. Select the _dev_ space: `azds space select --name dev`. When prompted to select a parent dev space, select _\<none\>_ .
1. Navigate to the _mywebapi_ directory and execute: `azds up -d`
1. Navigate to the _webfrontend_ directory and execute: `azds up -d`
1. Execute `azds list-uris` to see the public endpoint for _webfrontend_

> [!TIP]
> The above steps manually set up a baseline, but we recommend teams use CI/CD to automatically keep your baseline up to date with committed code.
>
> Check out our [guide to setting up CI/CD with Azure DevOps](how-to/setup-cicd.md) to create a workflow similar to the following diagram.
>
> ![Example CI/CD diagram](media/common/ci-cd-complex.png)

At this point your baseline should be running. `azds list-up --all` komutunu çalıştırdığınızda aşağıdakine benzer bir çıktı görürsünüz:

```
$ azds list-up --all

Name                          DevSpace  Type     Updated  Status
----------------------------  --------  -------  -------  -------
mywebapi                      dev       Service  3m ago   Running
mywebapi-56c8f45d9-zs4mw      dev       Pod      3m ago   Running
webfrontend                   dev       Service  1m ago   Running
webfrontend-6b6ddbb98f-fgvnc  dev       Pod      1m ago   Running
```

The DevSpace column shows that both services are running in a space named _dev_. Anyone who opens the public URL and navigates to the web app will invoke the checked-in code path that runs through both services. Now suppose you want to continue developing _mywebapi_. Nasıl kod değişiklikleri yapabilir ve bunları test edebilir, öte yandan da geliştirme ortamını kullanan diğer geliştiricilerin işlemini kesintiye uğratmazsınız? Bunu yapmak için kendi alanınızı ayarlarsınız.

### <a name="create-a-dev-space"></a>Geliştirme alanı oluşturma
To run your own version of _mywebapi_ in a space other than _dev_, you can create your own space by using the following command:

```cmd
azds space select --name scott
```

When prompted, select _dev_ as the **parent dev space**. This means our new space, _dev/scott_, will derive from the space _dev_. Bu durumun test sırasında bize nasıl yardımcı olacağını birazdan göreceksiniz.

Keeping with our introductory hypothetical, we've used the name _scott_ for the new space so peers can identify who is working in it. But it can be called anything you like, and be flexible about what it means, like _sprint4_ or _demo_. Whatever the case, _dev_ serves as the baseline for all developers working on a piece of this application:

![](media/common/ci-cd-space-setup.png)

Geliştirme ortamındaki tüm alanların listesini görmek için `azds space list` komutunu çalıştırın. The _Selected_ column indicates which space you currently have selected (true/false). In your case, the space named _dev/scott_ was automatically selected when it was created. İstediğiniz zaman `azds space select` komutuyla başka bir alan seçebilirsiniz.

Şimdi nasıl çalıştığını görelim.

### <a name="make-a-code-change"></a>Make a code change
Go to the VS Code window for `mywebapi` and make a code edit to the default GET `/` handler in `server.js`, for example:

```javascript
app.get('/', function (req, res) {
    res.send('mywebapi now says something new');
});
```

### <a name="run-the-service"></a>Hizmeti çalıştırma

To run the service, hit F5 (or type `azds up` in the Terminal Window) to run the service. The service will automatically run in your newly selected space _dev/scott_. Confirm that your service is running in its own space by running `azds list-up`:

```cmd
$ azds list-up

Name                      DevSpace  Type     Updated  Status
mywebapi                  scott     Service  3m ago   Running
webfrontend               dev       Service  26m ago  Running
```

Notice an instance of *mywebapi* is now running in the _dev/scott_ space. The version running in _dev_ is still running but it is not listed.

List the URLs for the current space by running `azds list-uris`.

```cmd
$ azds list-uris

Uri                                                                        Status
-------------------------------------------------------------------------  ---------
http://localhost:53831 => mywebapi.scott:80                                Tunneled
http://scott.s.dev.webfrontend.6364744826e042319629.ce.azds.io/  Available
```

Notice the public access point URL for *webfrontend* is prefixed with *scott.s*. This URL is unique to the _dev/scott_ space. This URL prefix tells the Ingress controller to route requests to the _dev/scott_ version of a service. When a request with this URL is handled by Dev Spaces, the Ingress Controller first tries to route the request to the *webfrontend* service in the _dev/scott_ space. If that fails, the request will be routed to the *webfrontend* service in the _dev_ space as a fallback. Also notice there is a localhost URL to access the service over localhost using the Kubernetes *port-forward* functionality. For more information about URLs and routing in Azure Dev Spaces, see [How Azure Dev Spaces works and is configured](how-dev-spaces-works.md).

![Space Routing](media/common/Space-Routing.png)

Azure Dev Spaces’ın bu yerleşik özelliği, her geliştiricinin alanlarındaki hizmetlerin tam yığınını yeniden oluşturmasına gerek kalmadan kodu paylaşılan bir alanda test etmenize olanak sağlar. Bu yönlendirme, bu kılavuzun önceki adımında gösterildiği gibi uygulama kodunuzun yayma üst bilgilerini iletmesini gerektirir.

### <a name="test-code-in-a-space"></a>Alanda kodu test etme
To test your new version of *mywebapi* with *webfrontend*, open your browser to the public access point URL for *webfrontend* and go to the About page. Yeni iletinizin görüntülendiğini görmelisiniz.

Şimdi, URL'nin "scott.s." bölümünü kaldırın ve tarayıcıyı yenileyin. You should see the old behavior (with the *mywebapi* version running in _dev_).

Once you have a _dev_ space, which always contains your latest changes, and assuming your application is designed to take advantage of DevSpace's space-based routing as described in this tutorial section, hopefully it becomes easy to see how Dev Spaces can greatly assist in testing new features within the context of the larger application. Rather than having to deploy _all_ services to your private space, you can create a private space that derives from _dev_, and only "up" the services you're actually working on. The Dev Spaces routing infrastructure will handle the rest by utilizing as many services out of your private space as it can find, while defaulting back to the latest version running in the _dev_ space. And better still, _multiple_ developers can actively develop different services at the same time in their own space without disrupting each other.

### <a name="well-done"></a>Bravo!
Başlangıç kılavuzunu tamamladınız! Şunları öğrendiniz:

> [!div class="checklist"]
> * Azure’da yönetilen bir Kubernetes ile Azure Dev Spaces’ı ayarlayın.
> * Kapsayıcılarda yinelemeli kod geliştirin.
> * İki ayrı hizmeti bağımsız olarak geliştirin ve Kubernetes’in DNS hizmet bulma yöntemini kullanarak başka bir hizmete çağrı yapın.
> * Kodunuzu bir ekip ortamında verimli bir şekilde geliştirip test edin.
> * Establish a baseline of functionality using Dev Spaces to easily test isolated changes within the context of a larger microservice application

Now that you've explored Azure Dev Spaces, [share your dev space with a team member](how-to/share-dev-spaces.md) and begin collaborating.

## <a name="clean-up"></a>Temizleme
Geliştirme alanları ve içinde çalışan hizmetler dahil olmak üzere bir kümedeki Azure Dev Spaces örneğini tamamen silmek için `az aks remove-dev-spaces` komutunu kullanın. Bu eylemin geri alınamayacağını unutmayın. İleride kümeye yeniden Azure Dev Spaces desteği ekleyebilirsiniz ancak sıfırdan başlamış gibi olursunuz. Eski hizmetleriniz ve alanlarınız geri yüklenmez.

Aşağıdaki örnek etkin aboneliğinizdeki Azure Dev Spaces denetleyicilerini listeler ve ardından 'myaks-rg' kaynak grubundaki 'myaks' AKS kümesiyle ilişkilendirilmiş Azure Dev Spaces denetleyicisini siler.

```cmd
    azds controller list
    az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```
