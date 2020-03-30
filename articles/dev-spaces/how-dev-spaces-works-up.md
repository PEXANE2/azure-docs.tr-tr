---
title: Azure Geliştirme Alanları ile kodunuzu çalıştırma nasıl çalışır?
services: azure-dev-spaces
ms.date: 03/24/2020
ms.topic: conceptual
description: Azure Geliştirme Alanları ile Azure Kubernetes Hizmeti'nde kodunuzu çalıştırma işlemlerini açıklar
keywords: azds.yaml, Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler
ms.openlocfilehash: 6851c04ac0b72db1bd13c991875c16b0beadc573
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80241367"
---
# <a name="how-running-your-code-with-azure-dev-spaces-works"></a>Azure Geliştirme Alanları ile kodunuzu çalıştırma nasıl çalışır?

Azure Geliştirme Alanları, Kubernetes uygulamalarını hızla tekrarlayıp hata ayıklamanın ve bir Azure Kubernetes Hizmeti (AKS) kümesinde ekibinizle işbirliği yapmanın birden çok yolunu sağlar. [Projeniz bir dev alanında çalıştırılabilmek için hazır olduğunda,][how-it-works-prep]AKS kümenizde projenizi oluşturmak ve çalıştırmak için Dev Spaces'i kullanabilirsiniz.

Bu makalede, Dev Spaces ile AKS'de kodunuzu çalıştırmak ne olur açıklanır.

## <a name="run-your-code"></a>Kodunuzu çalıştırın

Kodunuzu bir dev alanında çalıştırmak `up` için komutu dosyanızla `azds.yaml` aynı dizinde düzenleyin:

```cmd
azds up
```

Komut, `up` projenizi oluşturmak ve geliştirme alanına çalıştırmak için gereken uygulama kaynağı dosyalarınızı ve diğer yapılarınızı yükler. Oradan, dev alanınızdaki denetleyici:

1. Uygulamanızı dağıtmak için Kubernetes nesnelerini oluşturur.
1. Uygulamanız için kapsayıcıyı oluşturur.
1. Uygulamanızı geliştirme alanına dağıtır.
1. Yapılandırılırsa uygulama bitiş noktanız için herkese açık bir DNS adı oluşturur.
1. Uygulama bitiş noktanıza erişim sağlamak için http://localhostbağlantı *noktasını* ileri ye doğru kullanır.
1. Müşteri tarafındaki takıma doğru sayılsa ve stderr' i iletin.


## <a name="starting-a-service"></a>Hizmet başlatma

Bir hizmeti geliştirme alanında başlattığınızda, istemci tarafı araç ve denetleyicisi, kaynak dosyalarınızı eşitlemek, kapsayıcınızı ve Kubernetes nesnelerinizi oluşturmak ve uygulamanızı çalıştırmak için eşgüdüm içinde çalışır.

Daha ayrıntılı bir düzeyde, çalıştırdığınızda ne `azds up`olur:

1. Dosyalar, kullanıcının bilgisayarından kullanıcının AKS kümesine özgü bir Azure dosya depolama alanına [eşitlenir.][sync-section] Kaynak kodu, Miğfer grafiği ve yapılandırma dosyaları yüklenir.
1. Denetleyici, yeni bir oturum başlatmak için bir istek oluşturur. Bu istek, benzersiz bir kimlik, alan adı, kaynak koduna giden yol ve hata ayıklama bayrağı gibi çeşitli özellikler içerir.
1. Denetleyici, Helm grafiğindeki *$(tag)* yer tutucuyu benzersiz oturum kimliğiyle değiştirir ve hizmetiniz için Helm grafiğini yükler. Miğfer grafiğine benzersiz oturum kimliğine bir başvuru eklemek, bu belirli oturum için AKS kümesine dağıtılan kapsayıcının oturum isteğine ve ilişkili bilgilere bağlanmasını sağlar.
1. Helm grafiğin yüklenmesi sırasında, Kubernetes webhook kabul sunucusu uygulamanızın bölmesine enstrümantasyon ve projenizin kaynak koduna erişim için ek kapsayıcılar ekler. Devspaces-proxy ve devspaces-proxy-init kapları HTTP izleme ve boşluk yönlendirme sağlamak için eklenir. Devspaces yapı kapsayıcıdocker örneğine erişim ve uygulamanızın kapsayıcı oluşturmak için proje kaynak kodu ile pod sağlamak için eklenir.
1. Uygulamanın bölmesi başlatıldığında, uygulama kapsayıcısını oluşturmak için devspaces-build kapsayıcı ve devspaces-proxy-init kapsayıcı kullanılır. Uygulama kapsayıcısı ve devspaces-proxy kapsayıcıları daha sonra başlatılır.
1. Uygulama kapsayıcısı başladıktan sonra, istemci tarafı işlevi, *port-forward* uygulamanız üzerinden http://localhostHTTP erişimi sağlamak için Kubernetes bağlantı noktası ileri etme işlevini kullanır. Bu bağlantı noktası yönlendirme, geliştirme bilgisayarınızı geliştirme alanınızdaki hizmete bağlar.
1. Bölmedeki tüm kapsayıcılar başlatıldığında, hizmet çalışıyor. Bu noktada, istemci tarafı işlevselliği HTTP izleri, stdout ve stderr akışı başlar. Bu bilgiler geliştirici için istemci tarafı işlevi tarafından görüntülenir.

## <a name="updating-a-running-service"></a>Çalışan bir hizmeti güncelleştirme

Bir hizmet çalışırken, Azure Geliştirme Alanları, proje kaynak dosyalarından herhangi biri değişirse bu hizmeti güncelleştirme yeteneğine sahiptir. Dev Spaces, değiştirilen dosya türüne bağlı olarak hizmeti farklı şekilde güncelleştirmeyi de işler. Dev Spaces'in çalışan bir hizmeti güncelleştirmenin üç yolu vardır:

* Dosyayı doğrudan güncelleştirme
* Çalışan uygulamanın kapsayıcısı içinde uygulamanın işlemini yeniden oluşturma ve yeniden başlatma
* Uygulamanın kapsayıcısını yeniden oluşturma ve yeniden dağıtma

![Azure Dev Spaces dosya eşitlemi](media/how-dev-spaces-works/file-sync.svg)

Html, css ve cshtml dosyaları gibi statik varlıklar olan belirli proje dosyaları, hiçbir şeyi yeniden başlatmadan doğrudan uygulamanın kapsayıcısında güncellenebilir. Statik bir varlık değişirse, yeni dosya dev alanına eşitlenir ve ardından çalışan kapsayıcı tarafından kullanılır.

Kaynak kodu veya uygulama yapılandırma dosyaları gibi dosyalarda yapılan değişiklikler, uygulamanın işlemini çalışan kapsayıcı içinde yeniden başlatarak uygulanabilir. Bu dosyalar eşitlendikten sonra, uygulamanın işlemi *devhostagent* işlemi kullanılarak çalışan kapsayıcı içinde yeniden başlatılır. Başlangıçta uygulamanın kapsayıcıoluştururken, denetleyici *devhostagent*denilen farklı bir işlem ile uygulama için başlangıç komutu değiştirir. Uygulamanın gerçek süreci daha sonra *devhostagent*altında bir alt süreç olarak çalıştırılır ve çıkışı *devhostagent*'çıkış kullanılarak dışarı borulu. *Devhostagent* işlemi de Dev Spaces'in bir parçasıdır ve Dev Spaces adına çalışan kapsayıcıdaki komutları yürütebilir. Yeniden başlatma yaparken, *devhostagent:*

* Uygulamayla ilişkili geçerli işlemi veya işlemleri durdurur
* Uygulamayı yeniden yeniden çalışır
* Uygulamayla ilişkili işlem veya işlemleri yeniden başlatır

*Devhostagent* önceki adımları yürütür yolu [ `azds.yaml`. ][azds-yaml-section]

Dockerfiles, csproj dosyaları veya Helm grafiğinin herhangi bir bölümü gibi proje dosyalarına yapılan güncelleştirmeler, uygulamanın kapsayıcısının yeniden oluşturulmasını ve yeniden dağıtılmasını gerektirir. Bu dosyalardan biri dev alanına eşitlendiğinde, denetleyici [dümen yükseltme][helm-upgrade] komutunu çalıştırAr ve uygulamanın kapsayıcısı yeniden oluşturulur ve yeniden dağıtılır.

## <a name="file-synchronization"></a>Dosya Eşitleme

Bir uygulama ilk kez bir geliştirme alanında başlatıldığında, uygulamanın tüm kaynak dosyaları yüklenir. Uygulama çalışırken ve daha sonra yeniden başlatılırken, yalnızca değiştirilen dosyalar yüklenir. Bu işlemi koordine etmek için iki dosya kullanılır: istemci tarafı dosyası ve denetleyici tarafı dosyası.

İstemci tarafı dosyası geçici bir dizinde depolanır ve Dev Spaces'te çalıştırdığınız proje dizininin bir karmasını temel alınarak adlandırılır. Örneğin, Windows'da *Kullanıcılar\USERNAME\AppData\Local\Temp\1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef.synclog* gibi bir dosyanız olacaktır. Linux'ta istemci tarafındaki dosya */tmp* dizininde depolanır. `echo $TMPDIR` Komutu çalıştırarak macOS'taki dizini bulabilirsiniz.

Bu dosya JSON biçimindedir ve şunları içerir:

* Dev alanıyla senkronize edilen her proje dosyası için bir giriş
* Eşitleme kimliği
* Son eşitleme işleminin zaman damgası

Her proje dosyası girişi, dosyaya giden bir yol ve zaman damgası içerir.

Denetleyici tarafı dosyası AKS kümesinde depolanır. Eşitleme kimliğini ve son eşitlemenin zaman damgasını içerir.

Eşitleme zaman damgaları istemci tarafı ve denetleyici tarafı dosyaları arasında eşleşmediğinde olur. Eşitleme sırasında, istemci tarafı araç lama istemci tarafındaki dosya daki dosya girişleri üzerinde yineler. Dosyanın zaman damgası eşitleme zaman damgası sonra ise, bu dosya dev alanı eşitlenir. Eşitleme tamamlandıktan sonra, eşitleme zaman damgaları hem istemci tarafında hem de denetleyici tarafındaki dosyalarda güncelleştirilir.

İstemci tarafı dosyası yoksa tüm proje dosyaları eşitlenir. Bu davranış, istemci tarafı dosyasını silerek tam bir eşitleme zorlamanızı sağlar.

## <a name="how-running-your-code-is-configured"></a>Kodunuzu çalıştırma nasıl yapılandırılır?

Azure Dev Spaces, hizmetinizi yüklemek ve yapılandırmak için dosyayı `azds.yaml` kullanır. Denetleyici, Miğfer `install` `azds.yaml` grafiğini yüklemek ve Kubernetes nesnelerini oluşturmak için dosyadaki özelliği kullanır:

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

Varsayılan olarak, `prep` komut Miğfer grafiği oluşturur. Ayrıca *install.chart* özelliğini Helm grafiğidizine ayarlar. Farklı bir konumda bir Miğfer grafiği kullanmak istiyorsanız, bu özelliği bu konumu kullanmak üzere güncelleştirebilirsiniz.

Miğfer grafiklerini yüklerken, Azure Dev Spaces, Miğfer grafiğindeki değerleri geçersiz kılmanın bir yolunu sağlar. Miğfer grafiği için varsayılan `charts/APP_NAME/values.yaml`değerler .

*Install.values* özelliğini kullanarak, Miğfer grafiğinde değiştirilmesini istediğiniz değerleri tanımlayan bir veya daha fazla dosyayı listeleyebilirsiniz. Örneğin, uygulamanızı özellikle bir geliştirme alanında çalıştırırken bir ana bilgisayar adı veya veritabanı yapılandırması istiyorsanız, bu geçersiz kılma işlevini kullanabilirsiniz. Ayrıca bir *ekleyebilirsiniz?* isteğe bağlı olarak ayarlamak için dosya adlarından herhangi birinin sonunda.

*Install.set* özelliği, Miğfer grafiğinde değiştirilmesini istediğiniz bir veya daha fazla değeri yapılandırmanızı sağlar. *install.set'te* yapılandırılan tüm *değerler, install.values*listesinde ki dosyalarda yapılandırılan değerleri geçersiz kılar. *install.set* altındaki özellikler Miğfer grafiğindeki değerlere bağlıdır ve oluşturulan Miğfer grafiğine bağlı olarak farklı olabilir.

Yukarıdaki örnekte, *install.set.replicaCount* özelliği denetleyiciye uygulamanızın kaç örneğinin geliştirme alanınızda çalıştırılmasını söyler. Senaryonuza bağlı olarak, bu değeri artırabilirsiniz, ancak uygulamanızın bölmesine bir hata ayıklama ekleme üzerinde etkisi olacaktır. Daha fazla bilgi için [sorun giderme makalesine][troubleshooting]bakın.

Oluşturulan Miğfer grafiğinde, kapsayıcı görüntüsü *{{ olarak ayarlanır. Values.image.repository }}:{{ . Değerler.image.tag }}*. Dosya, `azds.yaml` {{ değeri olarak kullanılan *install.set.image.tag* özelliğini varsayılan olarak *$(tag)* olarak *tanımlar. Değerler.image.tag }}*. *install.set.image.tag* özelliğini bu şekilde ayarlayarak, Azure Dev Spaces çalıştırırken uygulamanızın kapsayıcı görüntüsünün farklı bir şekilde etiketlenmesini sağlar. Bu özel durumda, görüntü * \<image.repository>:$(tag) değer*olarak etiketlenir. Dev Spaces'in AKS kümesindeki kapsayıcıyı tanıması ve bulabilmesi için *install.set.image.tag* değeri olarak *$(tag)* değişkenini kullanmanız gerekir.

Yukarıdaki örnekte `azds.yaml` *install.set.ingress.hosts'ı*tanımlar. *install.set.ingress.hosts* özelliği ortak uç noktalar için ana bilgisayar adı biçimini tanımlar. Bu özellik ayrıca denetleyici tarafından sağlanan değerler olan *$(spacePrefix)*, *$(rootSpacePrefix)* ve *$(hostSuffix)* kullanır.

*$(spacePrefix)* *SPACENAME.s*şeklini alan çocuk dev alanının adıdır. *$(rootSpacePrefix)* üst alanın adıdır. Örneğin, *azureuser* *varsayılan*bir alt alan ise, *$(rootSpacePrefix)* değeri *varsayılandır* ve *$(spacePrefix)* değeri *azureuser.s*. Alan bir alt alan değilse, *$(spacePrefix)* boştur. Örneğin, *varsayılan* alanın üst alanı yoksa, *$(rootSpacePrefix)* değeri *varsayılandır* ve *$(spacePrefix)* değeri boştur. *$(hostSuffix), AKS* kümenizde çalışan Azure Dev Spaces Ingress Controller'ı işaret eden bir DNS sonekidir. Bu DNS soneki, örneğin * \*bir joker DNS girişine karşılık gelir. RANDOM_VALUE.eus.azds.io*, Azure Dev Spaces denetleyicisi AKS kümenize eklendiğinde oluşturuldu.

Yukarıdaki `azds.yaml` dosyada, uygulamanızın ana bilgisayar adını değiştirmek için *install.set.ingress.hosts'ı* da güncelleştirebilirsiniz. Örneğin, *$(spacePrefix)$(rootSpacePrefix)webfrontend$(hostSuffix)* ile *$(spacePrefix)$(rootSpacePrefix)web$(hostSuffix)* adresinden uygulamanızın ana bilgisayar adını basitleştirmek istiyorsanız.

Uygulamanız için kapsayıcıyı oluşturmak için denetleyici yapılandırma `azds.yaml` dosyasının aşağıdaki bölümlerini kullanır:

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

Denetleyici, uygulamanızı oluşturmak ve çalıştırmak için bir Dockerfile kullanır.

*Build.context* özelliği Dockerfiles'in bulunduğu dizini listeler. *build.dockerfile* özelliği, uygulamanın üretim sürümünü oluşturmak için Dockerfile adını tanımlar. *Configurations.develop.build.dockerfile* özelliği, uygulamanın geliştirme sürümü için Dockerfile adını yapılandırır.

Geliştirme ve üretim için farklı Dockerdosyalarına sahip olmak, geliştirme sırasında belirli şeyleri etkinleştirmenize ve bu öğeleri üretim dağıtımları için devre dışı bilebilir. Örneğin, geliştirme sırasında hata ayıklama veya daha fazla ayrıntılı günlüğe kaydetmeyi ve üretim ortamında devre dışı kullanabilirsiniz. Dockerdosyalarınız farklı adlandırıldıysa veya farklı bir konumdaysa bu özellikleri de güncelleştirebilirsiniz.

Azure Dev Spaces, geliştirme sırasında hızla yinelemenize yardımcı olmak için yerel projenizdeki değişiklikleri eşitler ve uygulamanızı aşamalı olarak günceller. Yapılandırma dosyasındaki `azds.yaml` aşağıdaki bölüm eşitleme ve güncelleştirmeyi yapılandırmak için kullanılır:

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

Değişiklikleri eşitleyecek dosyalar ve dizinler *configurations.develop.container.sync* özelliğinde listelenir. Bu dizinler, komutu `up` çalıştırdığınızda ve değişiklikler algılandığında başlangıçta eşitlenir. Geliştirme alanınızla eşitlenmenizi istediğiniz ek veya farklı dizinler varsa, bu özelliği değiştirebilirsiniz.

*Configurations.develop.container.iterate.buildCommands* özelliği, uygulamanın bir geliştirme senaryosunda nasıl inşa edilebildiğini belirtir. *Configurations.develop.container.command* özelliği, uygulamayı bir geliştirme senaryosunda çalıştırmak için komut sağlar. Geliştirme sırasında kullanmak istediğiniz ek yapı veya çalışma zamanı bayrakları veya parametreleri varsa, bu özelliklerden birini güncelleştirmek isteyebilirsiniz.

*Configurations.develop.container.iterate.processesToKill* uygulamayı durdurmak için öldürme işlemlerini listeler. Geliştirme sırasında uygulamanızın yeniden başlatma davranışını değiştirmek istiyorsanız bu özelliği güncelleştirmek isteyebilirsiniz. Örneğin, uygulamanın oluşturulmasını veya başlatılmasını değiştirmek için *configuration.develop.container.iterate.buildKomutları* veya *configurations.develop.develop.command* özelliklerini güncellediyseniz, hangi işlemlerin durdurulduğunu değiştirmeniz gerekebilir.

Komutu `azds prep` kullanarak kodunuzu `--enable-ingress` hazırlarken, bayrağı ekleme seçeneğiniz vardır. Bayrağın `--enable-ingress` eklenmesi, uygulamanız için herkese açık bir URL oluşturur. Bu bayrağı atlarsanız, uygulamaya yalnızca küme içinde veya yerel ana bilgisayar tüneli kullanılarak erişilebilir. Komutu `azds prep` çalıştırdıktan sonra, *giriş.enabled* özelliğini değiştirerek `charts/APPNAME/values.yaml`bu ayarı değiştirebilirsiniz:

```yaml
ingress:
  enabled: true
```

## <a name="next-steps"></a>Sonraki adımlar

Ağ ağı hakkında daha fazla bilgi edinmek ve Azure Dev Spaces'te isteklerin nasıl yönlendirildiğini görmek için [Azure Dev Spaces'te yönlendirmenin nasıl çalıştığını][how-it-works-routing]görün.

Hızla yinelemek ve geliştirmek için Azure Geliştirme Boşluklarını kullanma hakkında daha fazla bilgi edinmek [için geliştirme bilgisayarınızı geliştirme alanınıza bağlamanın nasıl çalıştığını][how-it-works-connect] ve Kodunuzu Azure Geliştirme Alanları ile uzaktan hata [ayıklamanın nasıl çalıştığını][how-it-works-remote-debugging]öğrenin.

Projenizi çalıştırmak için Azure Geliştirme Alanları'nı kullanmaya başlamak için aşağıdaki hızlı başlangıçlara bakın:

* [Visual Studio Code ve Java ile hızla yineleyip hata ayıklama][quickstart-java]
* [Visual Studio Code ve .NET ile hızla yineleyip hata ayıklama][quickstart-netcore]
* [Visual Studio Code ve Node.js ile hızla yineleyin ve hata ayıklama][quickstart-node]
* [Visual Studio ve .NET Core ile hızla yineleyin ve hata ayıklama][quickstart-vs]
* [Kubernetes üzerinde bir uygulama geliştirmek için CLI kullanma][quickstart-cli]


[azds-yaml-section]: #how-running-your-code-is-configured
[helm-upgrade]: https://helm.sh/docs/intro/using_helm/#helm-upgrade-and-helm-rollback-upgrading-a-release-and-recovering-on-failure
[how-it-works-connect]: how-dev-spaces-works-connect.md
[how-it-works-prep]: how-dev-spaces-works-prep.md
[how-it-works-remote-debugging]: how-dev-spaces-works-remote-debugging.md
[how-it-works-routing]: how-dev-spaces-works-routing.md
[quickstart-cli]: quickstart-cli.md
[quickstart-java]: quickstart-java.md
[quickstart-netcore]: quickstart-netcore.md
[quickstart-node]: quickstart-nodejs.md
[quickstart-vs]: quickstart-netcore-visualstudio.md
[sync-section]: #file-synchronization
[troubleshooting]: troubleshooting.md