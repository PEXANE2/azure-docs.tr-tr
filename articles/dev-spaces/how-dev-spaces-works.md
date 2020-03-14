---
title: Azure Dev Spaces nasıl çalışıyor ve yapılandırılır
services: azure-dev-spaces
ms.date: 03/04/2019
ms.topic: conceptual
description: Güç Azure Dev Spaces ve bunların azds. YAML yapılandırma dosyasında nasıl yapılandırıldığına ilişkin süreçler açıklanmaktadır
keywords: azds. YAML, Azure Dev Spaces, dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar
ms.openlocfilehash: e96541b0008dca9cbaeda92152f835c188036971
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79259922"
---
# <a name="how-azure-dev-spaces-works-and-is-configured"></a>Azure Dev Spaces nasıl çalışıyor ve yapılandırılır

Bir Kubernetes uygulamasının geliştirilmesi zor olabilir. Docker ve Kubernetes yapılandırma dosyalarına ihtiyacınız vardır. Uygulamanızı yerel olarak test etme ve diğer bağımlı hizmetlerle etkileşim kurma hakkında bilgi almanız gerekir. Birden çok hizmet üzerinde geliştirme ve test etmeyi bir kerede ve bir geliştirici ekibi ile gerçekleştirmeniz gerekebilir.

Azure Dev Spaces, Kubernetes uygulamalarını doğrudan Azure Kubernetes hizmeti 'nde (AKS) geliştirmenize, dağıtmanıza ve hata ayıklamanıza yardımcı olur. Azure Dev Spaces ayrıca ekibin bir geliştirme alanını paylaşmasına izin verir. Bir takım genelinde bir geliştirme alanını paylaşmak, tek tek takım üyelerinin, bağımlılıkları veya kümedeki diğer uygulamaları çoğaltmak veya geliştirmek zorunda kalmadan yalıtımda geliştirme yapmasına olanak sağlar.

Azure Dev Spaces, AKS 'deki Kubernetes uygulamalarınızı dağıtmak, çalıştırmak ve hata ayıklamak için bir yapılandırma dosyası oluşturur ve kullanır. Bu yapılandırma dosyası, uygulamanızın koduyla birlikte bulunur ve sürüm denetim sisteminize eklenebilir.

Bu makalede, güç Azure Dev Spaces ve bu işlemlerin Azure Dev Spaces yapılandırma dosyasında nasıl yapılandırıldığı açıklanır. Azure Dev Spaces hızlı bir şekilde çalışmaya başlayın ve bunu uygulamada görmek için hızlı başlangıçlardan birini doldurun:

* [CLı ve Visual Studio Code ile Java](quickstart-java.md)
* [CLı ve Visual Studio Code .NET Core](quickstart-netcore.md)
* [Visual Studio ile .NET Core](quickstart-netcore-visualstudio.md)
* [CLı ve Visual Studio Code ile Node. js](quickstart-nodejs.md)

## <a name="how-azure-dev-spaces-works"></a>Azure Dev Spaces nasıl çalışır?

Azure Dev Spaces, denetleyici ve istemci tarafı araçları ile etkileşimde bulunabilmeniz için iki ayrı bileşene sahiptir.

![Azure Dev Spaces bileşenleri](media/how-dev-spaces-works/components.svg)

Denetleyici aşağıdaki eylemleri gerçekleştirir:

* Geliştirme alanı oluşturmayı ve seçimini yönetir.
* Uygulamanızın helk grafiğini yükleyip Kubernetes nesnelerini oluşturur.
* Uygulamanızın kapsayıcı görüntüsünü oluşturur.
* Uygulamanızı AKS 'e dağıtır.
* , Kaynak kodunuz değiştiğinde Artımlı derlemeler ve yeniden başlatılır.
* Günlükleri ve HTTP izlemelerini yönetir.
* Stdout ve stderr 'i istemci tarafı araçlarına iletir.
* Ekip üyelerinin bir üst geliştirici alanından türetilmiş alt dev alanları oluşturmasına izin verir.
* Bir boşluk içindeki uygulamalar için ve üst ve alt boşluklar arasında yönlendirmeyi yapılandırır.

Denetleyici AKS dışında bulunur. İstemci tarafı araçları ve AKS kümesi arasındaki davranışı ve iletişimi yürütür. Azure Dev Spaces kullanmak için kümenizi hazırlarken Azure CLı kullanılarak denetleyici etkinleştirilir. Etkinleştirildikten sonra, istemci tarafı araçları kullanarak onunla etkileşime geçebilirsiniz.

İstemci tarafı araçları, kullanıcının şunları yapmasına izin verir:
* Uygulama için bir Dockerfile, Held grafik ve Azure Dev Spaces yapılandırma dosyası oluşturun.
* Üst ve alt dev alanları oluşturun.
* Denetleyiciyi, uygulamanızı derleyip başlatacak şekilde söyleyin.

Uygulamanız çalışırken, istemci tarafı araçları da:
* AKS 'de çalışan uygulamanızdan stdout ve stderr 'leri alır ve görüntüler.
* Http:\//localhost. kullanarak uygulamanıza Web erişimi sağlamak için [bağlantı noktası iletme](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/) kullanır
* AKS 'teki çalışan uygulamanıza bir hata ayıklayıcı ekler.
* Artımlı derlemeler için bir değişiklik algılandığında hızlı yinelemeye izin veren kaynak kodu dev alanınızda eşitler.

`azds` komutunun bir parçası olarak komut satırından istemci tarafı araçları ' nı kullanabilirsiniz. Ayrıca, ile birlikte istemci tarafı araçları 'nı kullanabilirsiniz:

* [Azure dev Spaces uzantısını](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds)kullanarak Visual Studio Code.
* [Kubernetes için Visual Studio Araçları](https://aka.ms/get-vsk8stools)Ile Visual Studio.

Azure Dev Spaces ayarlamak ve kullanmak için temel akış aşağıda verilmiştir:
1. AKS kümenizi Azure Dev Spaces için hazırlama
1. Kodunuzu Azure Dev Spaces üzerinde çalıştırmaya hazırlama
1. Kodunuzu bir geliştirme alanında çalıştırın
1. Geliştirme alanında kodunuzda hata ayıklama
1. Geliştirme alanı paylaşma

Azure Dev Spaces aşağıdaki bölümlerin her birinde nasıl çalıştığına ilişkin daha fazla ayrıntı ele alınacaktır.

## <a name="prepare-your-aks-cluster"></a>AKS kümenizi hazırlama

AKS kümenizi hazırlama şunları içerir:
* AKS kümenizin [Azure dev Spaces tarafından desteklenen][supported-regions]bir bölgede doğrulanması.
* Kubernetes 1.10.3 veya üstünü çalıştırdığınız doğrulanıyor.
* `az aks use-dev-spaces` kullanarak kümenizdeki Azure Dev Spaces etkinleştirme

Azure Dev Spaces için bir AKS kümesi oluşturma ve yapılandırma hakkında daha fazla bilgi için, bkz. başlangıç kılavuzlarından biri:
* [Java ile Azure Dev Spaces kullanmaya başlama](get-started-java.md)
* [.NET Core ve Visual Studio ile Azure Dev Spaces kullanmaya başlama](get-started-netcore-visualstudio.md)
* [.NET Core ile Azure Dev Spaces kullanmaya başlama](get-started-netcore.md)
* [Node. js ile Azure Dev Spaces kullanmaya başlama](get-started-nodejs.md)

AKS kümenizde Azure Dev Spaces etkinleştirildiğinde, kümeniz için denetleyiciyi yükler. Denetleyici, kümenizin dışındaki ayrı bir Azure kaynağıdır ve kümenizdeki kaynaklar için aşağıdakileri yapar:

* Geliştirme alanı olarak kullanılacak bir Kubernetes ad alanı oluşturur veya belirtir.
* Varsa *azds*adlı tüm Kubernetes ad alanını kaldırır ve yeni bir tane oluşturur.
* Bir Kubernetes Web kancası yapılandırması dağıtır.
* Bir Web kancası giriş sunucusu dağıtır.
    

Ayrıca, AKS kümenizin diğer Azure Dev Spaces bileşenlerine hizmet çağrısı yapmak için kullandığı hizmet sorumlusunu kullanır.

![Kümeyi hazırlama Azure Dev Spaces](media/how-dev-spaces-works/prepare-cluster.svg)

Azure Dev Spaces kullanmak için en az bir dev alanı olmalıdır. Azure Dev Spaces, AKS kümenizdeki geliştirme alanları için Kubernetes ad alanlarını kullanır. Bir denetleyici yüklenirken, yeni bir Kubernetes ad alanı oluşturmanızı veya ilk geliştirme alanım olarak kullanılacak mevcut bir ad alanını seçmenizi ister. Bir ad alanı bir geliştirme alanı olarak belirlendiyse, denetleyici *azds.io/Space=true* etiketini bir geliştirme alanı olarak tanımlamak için bu ad alanına ekler. Kümenizi hazırladıktan sonra oluşturduğunuz veya belirleyeceğiniz ilk geliştirme alanı varsayılan olarak seçilidir. Bir boşluk seçildiğinde, yeni iş yükleri oluşturmak için Azure Dev Spaces tarafından kullanılır.

Varsayılan olarak, denetleyici var olan *varsayılan* Kubernetes ad alanını yükselterek *varsayılan* adlı bir geliştirme alanı oluşturur. Yeni dev alanları oluşturmak ve var olan geliştirme alanlarını kaldırmak için istemci tarafı araçları 'nı kullanabilirsiniz. Kubernetes kısıtlamasından dolayı *varsayılan* dev alanı kaldırılamaz. Denetleyici Ayrıca, *AZD* adlı mevcut Kubernetes ad alanlarını kaldırarak istemci tarafı araçları tarafından kullanılan `azds` komutuyla çakışmaları ortadan kaldırır.

Kubernetes Web kancası giriş sunucusu, izleme dağıtımı sırasında üç kapsayıcıyla düğüm eklemek için kullanılır: bir devspaces-proxy kapsayıcısı, bir devspaces-proxy-init kapsayıcı ve bir devspaces oluşturma kapsayıcısı. **Bu kapsayıcıların üçü de AKS kümenizde kök erişimle çalışır.** Ayrıca, AKS kümenizin diğer Azure Dev Spaces bileşenlerine hizmet çağrısı yapmak için kullandığı hizmet sorumlusunu kullanır.

![Kubernetes Web kancası giriş sunucusu Azure Dev Spaces](media/how-dev-spaces-works/kubernetes-webhook-admission-server.svg)

Devspaces-proxy kapsayıcısı, uygulama kapsayıcısının içindeki ve giden tüm TCP trafiğini işleyen ve yönlendirmeyi gerçekleştirmeye yardımcı olan bir sepet kapsayıcısıdır. Bazı boşluklar kullanılıyorsa devspaces-proxy kapsayıcısı reroutes HTTP iletileri. Örneğin, üst ve alt alanlarda uygulamalar arasında HTTP iletileri yönlendirmenize yardımcı olabilir. HTTP olmayan tüm trafik, devspaces-proxy değiştirilmeden geçer. Devspaces-proxy kapsayıcısı, tüm gelen ve giden HTTP iletilerini de günlüğe kaydeder ve bunları izleme olarak istemci tarafı araçlarına gönderir. Bu izlemeler daha sonra uygulamanın davranışını incelemek üzere geliştirici tarafından görüntülenebilir.

Devspaces-proxy-init kapsayıcısı, uygulamanızın kapsayıcısına alan hiyerarşisi temelinde ek yönlendirme kuralları ekleyen bir [init kapsayıcıdır](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) . Uygulama kapsayıcısının */etc/resolv.exe* dosyasını ve Iptables yapılandırmasını başlamadan önce güncelleştirerek yönlendirme kuralları ekler. */Etc/resolv.exe* güncelleştirmeleri, üst alanlarda hizmetlerin DNS çözümlemesine izin verir. Iptables yapılandırma güncelleştirmeleri, uygulamanın kapsayıcısının içindeki ve giden tüm TCP trafiğinin devspaces-proxy olmasına rağmen yönlendirilmesini güvence altına alınır. Devspaces tarafından yapılan tüm güncelleştirmeler-proxy-init, Kubernetes 'in eklediği kurallara ek olarak gerçekleşir.

Devspaces oluşturma kapsayıcısı bir başlangıç kapsayıcısıdır ve proje kaynak kodu ve Docker yuvası takılı olur. Proje kaynak kodu ve Docker erişimi, uygulama kapsayıcısının doğrudan Pod tarafından oluşturulmasına izin verir.

> [!NOTE]
> Azure Dev Spaces uygulamanızın kapsayıcısını derlemek ve çalıştırmak için aynı düğümü kullanır. Sonuç olarak, Azure Dev Spaces uygulamanızı oluşturmak ve çalıştırmak için bir dış kapsayıcı kayıt defteri gerekmez.

Kubernetes Web kancası giriş sunucusu, AKS kümesinde oluşturulan yeni Pod 'ları dinler. Bu Pod, *azds.io/Space=true* etiketi ile herhangi bir ad alanına dağıtılmışsa, bu Pod öğesini ek kapsayıcılar ile çıkartır. Devspaces oluşturma kapsayıcısı yalnızca uygulamanın kapsayıcısı istemci tarafı araçları kullanılarak çalıştırıldığında eklenir.

AKS kümenizi hazırladıktan sonra, geliştirme alanınızda kodunuzu hazırlamak ve çalıştırmak için istemci tarafı araçları 'nı kullanabilirsiniz.

## <a name="prepare-your-code"></a>Kodunuzu hazırlayın

Uygulamanızı bir geliştirme alanında çalıştırmak için Kapsayıcılı olması gerekir ve Kubernetes 'e nasıl dağıtılması gerektiğini tanımlamanız gerekir. Uygulamanızı konteynize etmek için bir Dockerfile gerekir. Uygulamanızın Kubernetes 'e nasıl dağıtıldığını tanımlamak için bir [HELI grafiğine](https://docs.helm.sh/)ihtiyacınız vardır. Uygulamanız için hem Dockerfile hem de Held grafiği oluşturmaya yardımcı olmak için, istemci tarafı araçları `prep` komutunu sağlar:

```cmd
azds prep --public
```

`prep` komutu projenizdeki dosyalara bakar ve uygulamanızı Kubernetes 'te çalıştırmak için Dockerfile ve helk grafiğini oluşturmaya çalışır. Şu anda `prep` komut şu dilleri içeren bir Dockerfile ve HELI grafiği oluşturacak:

* Java
* Node.js
* .NET Core

`prep` komutunu kaynak kodu içeren bir dizinden çalıştırmanız *gerekir* . `prep` komutunu doğru dizinden çalıştırmak, istemci tarafı araçları 'nın, uygulamanızı kapsayıtacak şekilde dili belirlemesine ve uygun bir Dockerfile oluşturmasına izin verir. Java projeleri için bir *Pod. xml* dosyası içeren bir dizinden `prep` komutunu da çalıştırabilirsiniz.

Kaynak kodu içermeyen dizinden `prep` komutunu çalıştırırsanız, istemci tarafı araçları Dockerfile oluşturmaz. Ayrıca, *Desteklenmeyen bir dil nedeniyle Dockerfile*oluşturulamadı hatası görüntülenir. Bu hata, istemci tarafı araçları proje türünü tanımadığı zaman da oluşur.

`prep` komutunu çalıştırdığınızda `--public` bayrağını belirtme seçeneğiniz vardır. Bu bayrak, denetleyiciye bu hizmet için internet 'ten erişilebilen bir uç nokta oluşturmasını söyler. Bu bayrağı belirtmezseniz, hizmete yalnızca küme içinden veya istemci tarafı araçları tarafından oluşturulan localhost tüneli kullanılarak erişilebilir. Oluşturulan Held grafiğini güncelleştirerek `prep` komutunu çalıştırdıktan sonra bu davranışı etkinleştirebilir veya devre dışı bırakabilirsiniz.

`prep` komutu, projenizde mevcut olan Dockerfiles veya Held grafiklerini değiştirmez. Mevcut bir Dockerfile veya HELI grafiği `prep` komutu tarafından oluşturulan dosyalarla aynı adlandırma kuralını kullanıyorsa, `prep` komutu bu dosyaları oluşturmayı atlar. Aksi takdirde, `prep` komutu, kendi Dockerfile veya Helu grafiğini, mevcut dosyaların yanı yana oluşturacaktır.

`prep` komutu, projenizin kökünde de bir `azds.yaml` dosyası oluşturacaktır. Azure Dev Spaces uygulamanızı derlemek, yüklemek, yapılandırmak ve çalıştırmak için bu dosyayı kullanır. Bu yapılandırma dosyası, Dockerfile ve Held grafiğinin konumunu listeler ve ayrıca bu yapıtların üstünde ek yapılandırma sağlar.

[.NET Core örnek uygulaması](https://github.com/Azure/dev-spaces/tree/master/samples/dotnetcore/getting-started/webfrontend)kullanılarak oluşturulan bir azds. YAML dosyası aşağıda verilmiştir:

```yaml
kind: helm-release
apiVersion: 1.1
build:
  context: .
  dockerfile: Dockerfile
install:
  chart: charts/webfrontend
  values:
  - values.dev.yaml?
  - secrets.dev.yaml?
  set:
    replicaCount: 1
    image:
      repository: webfrontend
      tag: $(tag)
      pullPolicy: Never
    ingress:
      annotations:
        kubernetes.io/ingress.class: traefik-azds
      hosts:
        # This expands to [space.s.][rootSpace.]webfrontend.<random suffix>.<region>.azds.io
        # Customize the public URL by changing the 'webfrontend' text between the $(rootSpacePrefix) and $(hostSuffix) tokens
        # For more information see https://aka.ms/devspaces/routing
        - $(spacePrefix)$(rootSpacePrefix)webfrontend$(hostSuffix)
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
      useGitIgnore: true
      args:
        BUILD_CONFIGURATION: ${BUILD_CONFIGURATION:-Debug}
    container:
      sync:
      - "**/Pages/**"
      - "**/Views/**"
      - "**/wwwroot/**"
      - "!**/*.{sln,csproj}"
      command: [dotnet, run, --no-restore, --no-build, --no-launch-profile, -c, "${BUILD_CONFIGURATION:-Debug}"]
      iterate:
        processesToKill: [dotnet, vsdbg]
        buildCommands:
        - [dotnet, build, --no-restore, -c, "${BUILD_CONFIGURATION:-Debug}"]
```

`prep` komutu tarafından oluşturulan `azds.yaml` dosyası basit, tek bir proje geliştirme senaryosunda sorunsuz çalışmalıdır. Belirli projeniz karmaşıklığa sahipse, `prep` komutunu çalıştırdıktan sonra bu dosyayı güncelleştirmeniz gerekebilir. Örneğin, projeniz geliştirme veya hata ayıklama gereksinimlerinize göre derleme veya başlatma sürecinizi miktar halinde alabilir. Ayrıca, projenizde birden çok derleme işlemi veya farklı derleme içeriği gerektiren birden fazla uygulamanız olabilir.

## <a name="run-your-code"></a>Kodunuzu çalıştırın

Kodunuzu bir geliştirme alanında çalıştırmak için `azds.yaml` dosyanızdaki aynı dizinde `up` komutunu verin:

```cmd
azds up
```

`up` komutu, projenizi derlemek ve geliştirme alanında çalıştırmak için gereken uygulama kaynak dosyalarını ve diğer yapıtları karşıya yükler. Buradan, geliştirme alanınızdaki denetleyici:

1. Uygulamanızı dağıtmak için Kubernetes nesnelerini oluşturur.
1. Uygulamanız için kapsayıcıyı oluşturur.
1. Uygulamanızı geliştirme alanına dağıtır.
1. Yapılandırıldıysa, uygulama uç noktanız için genel olarak erişilebilen bir DNS adı oluşturur.
1. http://localhostkullanarak uygulama uç noktanıza erişim sağlamak için *bağlantı noktası İleri* kullanır.
1. Stdout ve stderr 'i istemci tarafı araçlarına iletir.


### <a name="starting-a-service"></a>Hizmet başlatılıyor

Bir hizmeti bir geliştirme alanında başlattığınızda, istemci tarafı araçları ve denetleyicisi, kaynak dosyalarınızı eşitlemeye, kapsayıcınızı ve Kubernetes nesnelerinizi oluşturmaya ve uygulamanızı çalıştırmaya yönelik olarak çalışır.

Daha ayrıntılı bir düzeyde, `azds up`çalıştırdığınızda ne olur:

1. Dosyalar, kullanıcının makinesinden, kullanıcının AKS kümesi için benzersiz olan bir Azure dosya depolama alanına eşitlenir. Kaynak kodu, Helz grafiği ve yapılandırma dosyaları karşıya yüklenir. Eşitleme işlemi hakkında daha fazla ayrıntı için sonraki bölümde bulabilirsiniz.
1. Denetleyici yeni bir oturum başlatma isteği oluşturur. Bu istek, benzersiz KIMLIK, alan adı, kaynak kodu yolu ve bir hata ayıklama bayrağı dahil olmak üzere çeşitli özellikler içerir.
1. Denetleyici, Held grafiğindeki *$ (Tag)* yer tutucusunu BENZERSIZ oturum kimliğiyle değiştirir ve hizmetiniz Için Held grafiğini yüklüyor. Hele grafiğine benzersiz oturum KIMLIĞI başvurusu eklemek, bu belirli oturumun AKS kümesine dağıtılan kapsayıcının oturum isteğine ve ilgili bilgilere geri bağlanması için izin verir.
1. Helk grafiğinin yüklenmesi sırasında Kubernetes Web kancası giriş sunucusu, uygulamanızın Pod öğesine yönetim için ek kapsayıcılar ekler ve projenin kaynak koduna erişin. Devspaces-proxy ve devspaces-proxy-init kapsayıcıları, HTTP izleme ve alan yönlendirmesi sağlamak için eklenir. Devspaces-Build kapsayıcısı, uygulama kapsayıcısını oluşturmak için Docker örneğine ve proje kaynak koduna erişimi olan Pod 'yu sağlamak için eklenmiştir.
1. Uygulamanın Pod 'ı başlatıldığında, uygulama kapsayıcısını oluşturmak için devspaces oluşturma kapsayıcısı ve devspaces-proxy-init kapsayıcısı kullanılır. Uygulama kapsayıcısı ve devspaces-proxy kapsayıcıları başlatılır.
1. Uygulama kapsayıcısı başladıktan sonra, istemci tarafı işlevselliği http://localhostüzerinden uygulamanıza HTTP erişimi sağlamak için Kubernetes *bağlantı noktası iletme* işlevini kullanır. Bu bağlantı noktası iletme, geliştirme makinenizi geliştirme alanınızdaki hizmete bağlar.
1. Pod 'daki tüm kapsayıcılar başlatıldığında hizmet çalışır. Bu noktada, istemci tarafı işlevleri HTTP izlemelerinin, stdout 'ın ve stderr 'in akışa başlamaktadır. Bu bilgiler, geliştiricinin istemci tarafı işlevselliği tarafından görüntülenir.

### <a name="updating-a-running-service"></a>Çalışan bir hizmet güncelleştiriliyor

Bir hizmet çalışırken, proje kaynak dosyalarından herhangi biri değiştiğinde bu hizmeti güncelleştirebilme Azure Dev Spaces. Geliştirme alanları, değiştirilen dosya türüne bağlı olarak hizmetin güncelleştirilmesini de işler. Geliştirme alanlarının çalışan bir hizmeti güncelleştirebilmenin üç yolu vardır:

* Bir dosyayı doğrudan güncelleştirme
* Uygulamanın çalışan uygulamanın kapsayıcısı içindeki işlemini yeniden oluşturma ve yeniden başlatma
* Uygulamanın kapsayıcısını yeniden oluşturma ve yeniden dağıtma

![Azure Dev Spaces dosya eşitleme](media/how-dev-spaces-works/file-sync.svg)

HTML, CSS ve cshtml dosyaları gibi statik varlıklar olan bazı proje dosyaları, hiçbir şeyi yeniden başlatmadan doğrudan uygulamanın kapsayıcısında güncelleştirilemeyebilir. Statik bir varlık değişirse, yeni dosya dev alanıyla eşitlenir ve ardından çalışan kapsayıcı tarafından kullanılır.

Kaynak kodu veya uygulama yapılandırma dosyaları gibi dosyalardaki değişiklikler, uygulamanın çalışan kapsayıcı içindeki işlemi yeniden başlatılarak uygulanabilir. Bu dosyalar eşitlendikten sonra, *devhostagent* işlemi kullanılarak çalışan kapsayıcı içinde uygulamanın işlemi yeniden başlatılır. Başlangıçta uygulamanın kapsayıcısını oluştururken denetleyici, uygulama için başlatma komutunun yerine *devhostagent*adlı farklı bir işlem koyar. Uygulamanın gerçek işlemi daha sonra *devhostagent*altında bir alt işlem olarak çalıştırılır ve çıkışı *devhostagent*'ın çıkışı kullanılarak gönderilir. *Devhostagent* Işlemi de dev alanlarının bir parçasıdır ve çalışan kapsayıcıda geliştirme alanları adına komutları yürütebilir. Yeniden başlatma işlemi gerçekleştirirken *devhostagent*:

* Uygulamayla ilişkili geçerli işlem veya işlemleri durdur
* Uygulamayı yeniden oluşturur
* Uygulamayla ilişkili işlem veya işlemleri yeniden başlatır

*Devhostagent* 'ın önceki adımları yürütme yöntemi `azds.yaml` yapılandırma dosyasında yapılandırılır. Bu yapılandırma, sonraki bölümde ayrıntılı olarak açıklanmıştır.

Dockerfiles, csproj dosyaları veya hele grafiğinin herhangi bir bölümü gibi proje dosyalarına yapılan güncelleştirmeler, uygulamanın kapsayıcısının yeniden oluşturulmasını ve dağıtılmasını gerektirir. Bu dosyalardan biri dev alanı ile eşitlendiğinde, denetleyici [HELI Upgrade](https://helm.sh/docs/intro/using_helm/#helm-upgrade-and-helm-rollback-upgrading-a-release-and-recovering-on-failure) komutunu çalıştırır ve uygulamanın kapsayıcısı yeniden oluşturulur ve yeniden dağıtılır.

### <a name="file-synchronization"></a>Dosya eşitleme

Bir geliştirme alanında uygulama ilk kez başlatıldığında, uygulamanın tüm kaynak dosyaları karşıya yüklenir. Uygulama çalışırken ve daha sonra yeniden başlatıldığında yalnızca değiştirilen dosyalar karşıya yüklenir. Bu işlemi koordine etmek için iki dosya kullanılır: bir istemci tarafı dosyası ve bir denetleyici tarafı dosyası.

İstemci tarafı dosyası geçici bir dizinde depolanır ve geliştirme alanlarında çalıştırdığınız proje dizini karmasını temel alarak adlandırılır. Örneğin, Windows 'da, projeniz için *Users\username\appdata\local\temp\1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef synclog* gibi bir dosyanız olur. Linux 'ta, istemci tarafı dosya */tmp* dizininde depolanır. `echo $TMPDIR` komutunu çalıştırarak macOS 'ta dizini bulabilirsiniz.

Bu dosya JSON biçimindedir ve şunları içerir:

* Geliştirme alanı ile eşitlenen her proje dosyası için bir giriş
* Bir eşitleme KIMLIĞI
* Son eşitleme işleminin zaman damgası

Her proje dosyası girişi, dosyanın ve zaman damgasının yolunu içerir.

Denetleyici tarafı dosya AKS kümesinde depolanır. Eşitleme KIMLIĞINI ve son eşitlemenin zaman damgasını içerir.

Eşitleme zaman damgaları, istemci tarafı ve denetleyici tarafı dosyaları arasında eşleşmediği zaman eşitleme gerçekleşir. Eşitleme sırasında, istemci tarafı araçları, istemci tarafı dosyadaki dosya girdileri üzerinde yinelenir. Dosyanın zaman damgası eşitleme zaman damgasından sonra ise, bu dosya geliştirme alanıyla eşitlenir. Eşitleme tamamlandıktan sonra, eşitleme zaman damgaları hem istemci tarafı hem de denetleyici tarafı dosyalarda güncelleştirilir.

İstemci tarafı dosyası yoksa, tüm proje dosyaları eşitlenir. Bu davranış, istemci tarafı dosyasını silerek tam eşitlemeye zorlamanıza olanak sağlar.

### <a name="how-routing-works"></a>Yönlendirmenin nasıl çalıştığı

Geliştirici alanı AKS 'in üzerine kurulmuştur ve aynı [ağ kavramlarını](../aks/concepts-network.md)kullanır. Ayrıca Azure Dev Spaces merkezi bir *ingressmanager* hizmetine sahiptir ve kendi giriş denetleyiciyi aks kümesine dağıtır. *Inressmanager* hizmeti, aks kümelerini dev Spaces ile izler ve uygulama yığınlarında yönlendirme için giriş nesneleriyle kümedeki Azure dev Spaces giriş denetleyicisini iyileştirir. Her pod 'daki devspaces-proxy kapsayıcısı, URL 'yi temel alan bir geliştirme alanına HTTP trafiği için `azds-route-as` HTTP üstbilgisi ekler. Örneğin, URL *http://azureuser.s.default.serviceA.fedcba09...azds.io* bir istek `azds-route-as: azureuser`bir http üst bilgisi alır. Devspaces-proxy kapsayıcısı zaten varsa, bir `azds-route-as` üst bilgisi eklemez.

Küme dışından bir hizmete HTTP isteği yapıldığında, istek giriş denetleyicisine gider. Giriş denetleyicisi, isteği, giriş nesnelerine ve kurallarına göre doğrudan uygun Pod 'a yönlendirir. Pod 'daki devspaces-proxy kapsayıcısı isteği alır, URL 'ye göre `azds-route-as` üst bilgisini ekler ve sonra isteği uygulama kapsayıcısına yönlendirir.

Küme içindeki başka bir hizmetten bir hizmete HTTP isteği yapıldığında, istek ilk olarak çağıran hizmetin devspaces-proxy kapsayıcısından geçer. Devspaces-proxy kapsayıcısı HTTP isteğine bakar ve `azds-route-as` üst bilgisini denetler. Üst bilgiye bağlı olarak, devspaces-proxy kapsayıcısı, üst bilgi değeriyle ilişkili hizmetin IP adresini arar. Bir IP adresi bulunursa, devspaces-proxy kapsayıcısı bu IP adresine isteği reroutes. Bir IP adresi bulunamazsa, devspaces-proxy kapsayıcısı, isteği üst uygulama kapsayıcısına yönlendirir.

Örneğin, *Servicea* ve *serviceb* uygulamaları *varsayılan*adlı bir üst geliştirme alanına dağıtılır. *Servicea* , *serviceb* 'YI kullanır ve buna http çağrıları yapar. Azure Kullanıcı, *azureuser*adlı *varsayılan* alana göre bir alt dev alanı oluşturur. Azure Kullanıcı aynı zamanda kendi *hizmet* sürümünü kendi alt alanına dağıtır. *http://azureuser.s.default.serviceA.fedcba09...azds.io* için bir istek yapıldığında:

![Azure Dev Spaces yönlendirme](media/how-dev-spaces-works/routing.svg)

1. Giriş denetleyicisi, *Servicea. azureuser*olan URL ile ilişkili pod için IP 'yi arar.
1. Giriş denetleyicisi, Azure kullanıcısının geliştirme alanında pod için IP 'yi bulur ve isteği *Servicea. azureuser* Pod 'a yönlendirir.
1. *Servicea. azureuser* Pod içindeki devspaces-proxy kapsayıcısı, isteği alır ve http üstbilgisi olarak `azds-route-as: azureuser` ekler.
1. *Servicea. azureuser* Pod içindeki devspaces-proxy kapsayıcısı, isteği *servicea. Azureuser* Pod içindeki *servicea* uygulama kapsayıcısına yönlendirir.
1. *Servicea. azureuser* Pod Içindeki *Servicea* uygulaması, *serviceb*'ye bir çağrı yapar. *Servicea* uygulaması, var olan `azds-route-as` üstbilgisini koruyacak kodu de içerir. bu durumda `azds-route-as: azureuser`.
1. *Servicea. azureuser* Pod içindeki devspaces-proxy kapsayıcısı, isteği alır ve `azds-route-as` üst bilgisinin değerine bağlı olarak *serviceb* IP 'sini arar.
1. *Servicea. azureuser* Pod içindeki devspaces-proxy kapsayıcısı *serviceb. AZUREUSER*için bir IP bulamadı.
1. *Servicea. azureuser* Pod içindeki devspaces-proxy kapsayıcısı, ana alanda *SERVICEB* için IP 'yi arar. *varsayılan*.
1. *Servicea. azureuser* Pod içindeki devspaces-proxy kapsayıcısı, *serviceb. Default* için IP 'Yi bulur ve isteği *serviceb. Default* Pod öğesine yönlendirir.
1. *Serviceb. Default* Pod içindeki devspaces-proxy kapsayıcısı, isteği alır ve Isteği *serviceb. Default* Pod içindeki *serviceb* uygulama kapsayıcısına yönlendirir.
1. *Serviceb. Default* Pod Içindeki *serviceb* uygulaması *servicea. azureuser* Pod 'a bir yanıt döndürür.
1. *Servicea. azureuser* Pod içindeki devspaces-proxy kapsayıcısı yanıtı alır ve yanıtı servicea *. Azureuser* Pod içindeki *servicea* uygulama kapsayıcısına yönlendirir.
1. *Servicea* uygulaması yanıtı alır ve kendi yanıtını döndürür.
1. *Servicea. azureuser* Pod içindeki devspaces-proxy kapsayıcısı, *servicea* uygulama kapsayıcısından gelen yanıtı alır ve yanıtı küme dışında özgün çağırana yönlendirir.

HTTP olmayan diğer tüm TCP trafiği, giriş denetleyicisi ve devspaces-proxy kapsayıcılarından değiştirilmemiş şekilde geçer.

### <a name="how-running-your-code-is-configured"></a>Kodunuzun çalıştırılması nasıl yapılandırılır?

Azure Dev Spaces, hizmetinizi yüklemek ve yapılandırmak için `azds.yaml` dosyasını kullanır. Denetleyici, helk grafiğini yüklemek ve Kubernetes nesnelerini oluşturmak için `azds.yaml` dosyasındaki `install` özelliğini kullanır:

```yaml
...
install:
  chart: charts/webfrontend
  values:
  - values.dev.yaml?
  - secrets.dev.yaml?
  set:
    replicaCount: 1
    image:
      repository: webfrontend
      tag: $(tag)
      pullPolicy: Never
    ingress:
      annotations:
        kubernetes.io/ingress.class: traefik-azds
      hosts:
      # This expands to [space.s.][rootSpace.]webfrontend.<random suffix>.<region>.azds.io
      # Customize the public URL by changing the 'webfrontend' text between the $(rootSpacePrefix) and $(hostSuffix) tokens
      # For more information see https://aka.ms/devspaces/routing
      - $(spacePrefix)$(rootSpacePrefix)webfrontend$(hostSuffix)
...
```

Varsayılan olarak, `prep` komutu HELI grafiğini oluşturacaktır. Ayrıca *Install. Chart* özelliğini Held grafiğinin dizinine ayarlar. Farklı bir konumda HELI grafiği kullanmak isterseniz, bu özelliği bu konumu kullanacak şekilde güncelleştirebilirsiniz.

Hele grafiklerini yüklerken Azure Dev Spaces Helu grafiğindeki değerleri geçersiz kılmak için bir yol sağlar. Held grafiğinin varsayılan değerleri `charts/APP_NAME/values.yaml`.

*Install. Values* özelliğini kullanarak, Held grafiğinde değiştirilmesini istediğiniz değerleri tanımlayan bir veya daha fazla dosyayı listeleyebilirsiniz. Örneğin, uygulamanızı bir geliştirme alanında çalıştırırken bir ana bilgisayar adı veya veritabanı yapılandırması isterseniz, bu geçersiz kılma işlevini kullanabilirsiniz. Ayrıca *, bir de* ekleyebilirsiniz. dosya adlarından sonuna kadar isteğe bağlı olarak ayarlayın.

*Install. set* özelliği, hele grafiğinde değiştirilmesini istediğiniz bir veya daha fazla değeri yapılandırmanıza olanak tanır. *Install. set* içinde yapılandırılan tüm değerler, *Install. Values*içinde listelenen dosyalarda yapılandırılan değerleri geçersiz kılar. *Install. set* altındaki özellikler, Held grafiğindeki değerlere bağımlıdır ve oluşturulan Held grafiğine bağlı olarak farklı olabilir.

Yukarıdaki örnekte, *Install. set. replicaCount* özelliği denetleyiciye geliştirme alanınızda uygulamanızın kaç örnek çalıştırılacağını söyler. Senaryonuza bağlı olarak, bu değeri artırabilirsiniz, ancak uygulamanızın Pod öğesine bir hata ayıklayıcı ekleme etkisi olur. Daha fazla bilgi için bkz. [sorun giderme makalesi](troubleshooting.md).

Oluşturulan HELI grafiğinde kapsayıcı görüntüsü *{{olarak ayarlanır. Values. Image. Repository}}: {{. Values. Image. Tag}}* . `azds.yaml` dosyası, *install. set. Image. Tag* özelliğini varsayılan olarak *$ (Tag)* olarak tanımlar; bu, {{için değer olarak kullanılır *. Values. Image. Tag}}* . *Install. set. Image. Tag* özelliğini bu şekilde ayarlayarak, uygulamanızın kapsayıcı görüntüsünün Azure dev Spaces çalıştırılırken farklı bir şekilde etiketlenmesine izin verir. Bu özel durumda görüntü, *Image. repository >: $ (Tag)\<değer*olarak etiketlendi. Geliştirme alanları tanıması ve AKS kümesindeki kapsayıcıyı bulmak için, *$ (Tag)* değişkenini *Install. set. image. Tag* değeri olarak kullanmanız gerekir.

Yukarıdaki örnekte, `azds.yaml` *Install. set. ınress. Konakları*tanımlıyor. *Install. set. ingress. hosts* özelliği, genel uç noktalar için bir ana bilgisayar adı biçimi tanımlar. Bu özellik ayrıca, denetleyici tarafından belirtilen değerler olan *$ (Spaceprefix)* , *$ (rootspaceprefix)* ve *$ (hostsuffix*) kullanır. 

*$ (Spaceprefix)* , *spacename. s*biçimini alan alt dev alanının adıdır. *$ (Rootspaceprefix)* , üst alanın adıdır. Örneğin, *azureuser* *varsayılan*bir alt alandır, *$ (rootspaceprefix)* değeri *varsayılandır* ve *$ (spaceprefix)* değeri *azureuser. s*' dir. Boşluk bir alt boşluk değilse, *$ (Spaceprefix)* boştur. Örneğin, *varsayılan* alanın üst alanı yoksa, *$ (rootspaceprefix)* değeri *varsayılandır* ve *$ (spaceprefix)* değeri boştur. *$ (Hostsuffix)* , aks kümenizde çalışan Azure dev Spaces giriş denetleyicisine işaret eden bir DNS son ekidir. Bu DNS son eki, örneğin\*bir joker karakter DNS girdisine karşılık gelir *. Azure Dev Spaces denetleyicisi AKS kümenize eklendiğinde oluşturulan RANDOM_VALUE. EUS. azds. IO*.

Yukarıdaki `azds.yaml` dosyasında, *Install. set. ınress. konaklarınızı* , uygulamanızın ana bilgisayar adını değiştirmek için de güncelleştirebilirsiniz. Örneğin, uygulamanızın ana bilgisayar adını *$ (Spaceprefix) $ (rootSpacePrefix) webön uç $ (hostSuffix* ) $ ( *rootspaceönek) Web $ (hostsuffix*) ile $ (hostsuffix) olarak basitleştirecek şekilde basitleştirmek istiyorsanız.

Uygulamanızın kapsayıcısını oluşturmak için, denetleyici `azds.yaml` yapılandırma dosyasının aşağıdaki bölümlerini kullanır:

```yaml
build:
  context: .
  dockerfile: Dockerfile
...
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
      useGitIgnore: true
      args:
        BUILD_CONFIGURATION: ${BUILD_CONFIGURATION:-Debug}
...
```

Denetleyici, uygulamanızı derlemek ve çalıştırmak için bir Dockerfile kullanır.

*Build. Context* özelliği, Dockerfiles 'ın mevcut olduğu dizini listeler. *Build. dockerfile* özelliği, uygulamanın üretim sürümünü oluşturmak Için dockerfile adını tanımlar. *Configurations. geliþme. Build. dockerfile* özelliği, uygulamanın geliştirme sürümü Için dockerfile adını yapılandırır.

Geliştirme ve üretim için farklı Dockerfiles bulundurmak, geliştirme sırasında belirli şeyleri etkinleştirmenizi ve üretim dağıtımlarında bu öğeleri devre dışı bırakmanızı sağlar. Örneğin, bir üretim ortamında geliştirme ve devre dışı bırakma sırasında hata ayıklamayı veya daha ayrıntılı günlük kaydını etkinleştirebilirsiniz. Ayrıca, Dockerfiles farklı şekilde adlandırılmışsa veya farklı bir konumdaysa bu özellikleri güncelleştirebilirsiniz.

Geliştirme sırasında hızlıca yinelemenize yardımcı olmak için Azure Dev Spaces değişiklikleri yerel projenizden eşitler ve uygulamanızı artımlı olarak güncelleştirir. `azds.yaml` yapılandırma dosyasının aşağıdaki bölümü eşitleme ve güncelleştirmeyi yapılandırmak için kullanılır:

```yaml
...
configurations:
  develop:
    ...
    container:
      sync:
      - "**/Pages/**"
      - "**/Views/**"
      - "**/wwwroot/**"
      - "!**/*.{sln,csproj}"
      command: [dotnet, run, --no-restore, --no-build, --no-launch-profile, -c, "${BUILD_CONFIGURATION:-Debug}"]
      iterate:
        processesToKill: [dotnet, vsdbg]
        buildCommands:
        - [dotnet, build, --no-restore, -c, "${BUILD_CONFIGURATION:-Debug}"]
...
```

Değişiklikleri eşitlenecek dosyalar ve dizinler, *Configurations. geliþme. Container. Sync* özelliğinde listelenir. Bu dizinler, `up` komutunu çalıştırdığınızda ve değişiklikler algılandığında başlangıçta eşitlenir. Geliştirme alanınızda eşitlenmesini istediğiniz ek veya farklı dizinler varsa, bu özelliği değiştirebilirsiniz.

*Configurations. geliştirmenin. Container. Iterate. buildCommands* özelliği, uygulamanın geliştirme senaryosunda nasıl oluşturulacağını belirtir. *Configurations. geliştirmenin. Container. Command* özelliği, uygulamayı bir geliştirme senaryosunda çalıştırmak için komutunu sağlar. Geliştirme sırasında kullanmak istediğiniz ek derleme veya çalışma zamanı bayrakları veya parametreler varsa, bu özelliklerden birini güncellemek isteyebilirsiniz.

*Configurations. geliþme. Container. Iterate. Processestokıll* , uygulamayı durdurmak için sonlandırılmak üzere süreçler listeler. Geliştirme sırasında uygulamanızın yeniden başlatma davranışını değiştirmek istiyorsanız bu özelliği güncellemek isteyebilirsiniz. Örneğin, *konfigürasyonları güncelleştirdiyseniz. geliştirme. Container. Iterate. buildCommands* veya *Configurations. geliþme. Container. Command* özellikleri, uygulamanın nasıl oluşturulduğunu veya başlatıldığını değiştirmek için, hangi işlemlerin durdurulduğunu değiştirmeniz gerekebilir.

`azds prep` komutunu kullanarak kodunuzu hazırlarken `--public` bayrağını ekleme seçeneğiniz vardır. `--public` bayrağını eklemek, uygulamanız için genel olarak erişilebilen bir URL oluşturur. Bu bayrağı atlarsanız, uygulamaya yalnızca küme içinden veya localhost tüneli kullanılarak erişilebilir. `azds prep` komutunu çalıştırdıktan sonra, bu ayarı değiştirmek için, `charts/APPNAME/values.yaml`içindeki giriş *. Enabled* özelliğini değiştirme:

```yaml
ingress:
  enabled: true
```

## <a name="debug-your-code"></a>Kodunuzda hata ayıklama

Java, .NET ve Node. js uygulamaları için, Visual Studio Code veya Visual Studio kullanarak doğrudan geliştirme alanınızda çalışan uygulamanızda hata ayıklaması yapabilirsiniz. Visual Studio Code ve Visual Studio, geliştirme alanınıza bağlanmak, uygulamanızı başlatmak ve bir hata ayıklayıcı eklemek için araç sağlar. `azds prep`çalıştırdıktan sonra projenizi Visual Studio Code veya Visual Studio 'da açabilirsiniz. Visual Studio Code veya Visual Studio, `azds prep`çalıştırmanın ayrı olarak bağlanması için kendi yapılandırma dosyalarını oluşturur. Visual Studio Code veya Visual Studio içinden kesme noktaları ayarlayabilir ve uygulamanızı geliştirme alanınızda başlatabilirsiniz.

![Kodunuzun hatalarını ayıklama](media/get-started-node/debug-configuration-nodejs2.png)

Hata ayıklama için Visual Studio Code veya Visual Studio kullanarak uygulamanızı başlattığınızda, `azds up`çalıştıran şekilde geliştirme alanınıza başlatma ve bağlantı kurma işlemini işler. Visual Studio Code ve Visual Studio 'daki istemci tarafı araçları, hata ayıklama için özel bilgiler içeren ek bir parametre de sağlar. Parametresi hata ayıklayıcı görüntüsünün adını, hata ayıklayıcının görüntüsündeki içindeki hata ayıklayıcının konumunu ve hata ayıklayıcı klasörünü bağlamak için uygulamanın kapsayıcısı içindeki hedef konumu içerir.

Hata ayıklayıcı görüntüsü, istemci tarafı araçları tarafından otomatik olarak belirlenir. Dockerfile sırasında kullanılan yönteme benzer bir yöntem kullanır ve `azds prep`çalıştırılırken Held grafik oluşturur. Hata ayıklayıcı uygulamanın görüntüsüne bağlandıktan sonra `azds exec`kullanılarak çalıştırılır.

## <a name="sharing-a-dev-space"></a>Geliştirme alanı paylaşma

Bir takımla çalışırken, bir [ekibin tamamında bir geliştirme alanı paylaşabilir](how-to/share-dev-spaces.md) ve türetilmiş dev alanları oluşturabilirsiniz. Geliştirici alanı, dev alanının kaynak grubuna katkıda bulunan erişimi olan herkes tarafından kullanılabilir.

Ayrıca, başka bir geliştirme alanından türetilmiş yeni bir geliştirme alanı da oluşturabilirsiniz. Türetilmiş bir geliştirme alanı oluşturduğunuzda, *azds.io/Parent-Space=Parent-Space-Name* etiketi türetilmiş geliştirme alanının ad alanına eklenir. Ayrıca, üst geliştirici alanındaki tüm uygulamalar, türetilmiş geliştirme alanıyla paylaşılır. Uygulamanın güncelleştirilmiş bir sürümünü türetilmiş geliştirme alanına dağıtırsanız, bu yalnızca türetilmiş dev alanında bulunur ve üst dev alanı etkilenmeden kalır. En fazla üç türetilmiş geliştirme alanı düzeyi veya en *üst* boşluk olabilir.

Türetilmiş geliştirme alanı Ayrıca, kendi uygulamaları ve üst öğesinden paylaşılan uygulamalar arasında istekleri de akıllıca yönlendirir. Yönlendirme, istek türetilmiş geliştirme alanındaki bir uygulamaya yönlendirilmeye çalışarak ve üst geliştirme alanından paylaşılan uygulamaya geri dönerek yapılır. Uygulama üst alanda değilse, yönlendirme, ana alanda bulunan paylaşılan uygulamaya geri döner.

Örnek:
* Dev alanı *varsayılan* olarak, *Servicea* ve *serviceb* uygulamalarına sahiptir.
* *Azureuser* dev Space, *varsayılan*olarak türetilir.
* *Servicea* 'nın güncelleştirilmiş bir sürümü *azureuser*'e dağıtılır.

*Azureuser*kullanılırken, *servicea* 'ya yapılan tüm istekler *azureuser*içindeki güncelleştirilmiş sürüme yönlendirilir. *Serviceb* 'ye yönelik bir istek, önce *serviceb*'nin *azureuser* sürümüne yönlendirilmek üzere denenir. Mevcut olmadığından, bu hizmet, *serviceb*'nin *varsayılan* sürümüne yönlendirilir. *Servicea* 'nın *azureuser* sürümü kaldırılırsa, *servicea* 'Ya yapılan tüm istekler *varsayılan* *servicea*sürümünü kullanmaya geri döner.

## <a name="next-steps"></a>Sonraki adımlar

Azure Dev Spaces kullanmaya başlamak için aşağıdaki hızlı başlangıçlara bakın:

* [CLı ve Visual Studio Code ile Java](quickstart-java.md)
* [CLı ve Visual Studio Code .NET Core](quickstart-netcore.md)
* [Visual Studio ile .NET Core](quickstart-netcore-visualstudio.md)
* [CLı ve Visual Studio Code ile Node. js](quickstart-nodejs.md)

Ekip geliştirmeyi kullanmaya başlamak için aşağıdaki nasıl yapılır makalelerine bakın:

* [Team Development-CLı ve Visual Studio Code ile Java](team-development-java.md)
* [Team Development-CLı ve Visual Studio Code ile .NET Core](team-development-netcore.md)
* [Team Development-Visual Studio ile .NET Core](team-development-netcore-visualstudio.md)
* [Team Development-CLı ve Visual Studio Code ile Node. js](team-development-nodejs.md)



[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
