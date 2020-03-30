---
title: Azure Geliştirme Alanları için proje hazırlama nasıl çalışır?
services: azure-dev-spaces
ms.date: 03/24/2020
ms.topic: conceptual
description: Azure Geliştirme Alanları ile projenizi hazırlamanın nasıl çalıştığını açıklar
keywords: azds.yaml, Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler
ms.openlocfilehash: 24a54fffdc8e94493d2a4a9aeb1c5f02dcd192b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80241640"
---
# <a name="how-preparing-a-project-for-azure-dev-spaces-works"></a>Azure Geliştirme Alanları için proje hazırlama nasıl çalışır?

Azure Geliştirme Alanları, Kubernetes uygulamalarını hızla tekrarlayıp hata ayıklamanın ve bir Azure Kubernetes Hizmeti (AKS) kümesinde ekibinizle işbirliği yapmanın birden çok yolunu sağlar. Dev Spaces, projeniz için Dockerfiles ve Helm grafikleri oluşturabilir. Dev Spaces ayrıca AKS'deki Kubernetes uygulamalarınızı dağıtmak, çalıştırmak ve hata ayıklama için bir yapılandırma dosyası oluşturur ve kullanır. Tüm bu dosyalar uygulamanızın kodunda bulunur ve sürüm kontrol sisteminize eklenebilir.

Bu makalede, Dev Spaces ile AKS'de çalışmaya projenizi hazırlamak ne olur açıklar.

## <a name="prepare-your-code"></a>Kodunuzu hazırlayın

Uygulamanızı bir geliştirme alanında çalıştırmak için, kaplanması ve Kubernetes'e nasıl dağıtılması gerektiğini tanımlamanız gerekir. Uygulamanızı kapsayıcı hale getirmek için bir Dockerfile'a ihtiyacınız var. Uygulamanızın Kubernetes'e nasıl dağıtılanını tanımlamak için bir [Miğfer grafiğine](https://docs.helm.sh/)ihtiyacınız vardır. Uygulamanız için hem Dockerfile hem de Miğfer grafiği oluşturmaya `prep` yardımcı olmak için istemci tarafı araçları komutu sağlar:

```cmd
azds prep --enable-ingress
```

Komut `prep` projenizdeki dosyalara bakar ve uygulamanızı Kubernetes'te çalıştırmak için Dockerfile ve Helm grafiğini oluşturmaya çalışır. Şu `prep` anda, komut aşağıdaki dilleri içeren bir Dockerfile ve Helm grafiği oluşturur:

* Java
* Node.js
* .NET Core

Komutu `prep` kaynak kodu içeren bir dizinden çalıştırmanız *gerekir.* Komutu `prep` doğru dizinden çalıştırmak, istemci tarafındaki aracın dili tanımlamasına ve uygulamanızı kapsayıcılaştırmak için uygun bir Dockerfile oluşturmasına olanak tanır. Komutu `prep` Java projeleri için *pom.xml* dosyası içeren bir dizinden de çalıştırabilirsiniz.

Kaynak kodu `prep` içermeyen dizinden komutçalıştırArsanız, istemci tarafı aracı Dockerfile oluşturmaz. Ayrıca bir hata söyleyerek görüntüler: *Dockerfile desteklenmeyen dil nedeniyle oluşturulamadı.* İstemci tarafı takım proje türünü tanımıyorsa, bu hata da oluşur.

Komutu `prep` çalıştırdığınızda, `--enable-ingress` bayrağı belirtme seçeneğiniz var. Bu bayrak, denetleyiciye bu hizmet için internet erişimine uygun bir bitiş noktası oluşturmasını söyler. Bu bayrağı belirtmezseniz, hizmete yalnızca küme nin içinden veya istemci tarafındaki araç için oluşturulan localhost tüneli kullanılarak erişilebilir. Oluşturulan Miğfer grafiğini güncelleştirerek `prep` komutu çalıştırdıktan sonra bu davranışı etkinleştirebilir veya devre dışı kullanabilirsiniz.

Komut, `prep` projenizde varolan Dockerfiles veya Helm grafiklerini değiştirmez. Varolan bir Dockerfile veya Helm grafiği komut tarafından oluşturulan `prep` dosyalarla `prep` aynı adlandırma kuralını kullanırsa, komut bu dosyaları oluşturmayı atlar. Aksi takdirde, `prep` komut varolan dosyaların yanı sıra kendi Dockerfile veya Helm grafik oluşturur.

> [!IMPORTANT]
> Azure Dev Spaces, kodunuzu oluşturmak ve çalıştırmak için projeniz için Dockerfile ve Miğfer grafiğini kullanır, ancak projenin oluşturulma ve çalışma şeklini değiştirmek istiyorsanız bu dosyaları değiştirebilirsiniz.

Komut, `prep` projenizin `azds.yaml` kökünde bir dosya da oluşturur. Azure Dev Spaces bu dosyayı uygulamanızı oluşturmak, yüklemek, yapılandırmak ve çalıştırmak için kullanır. Bu yapılandırma dosyası Dockerfile ve Helm grafiğinizin konumunu listeler ve ayrıca bu yapıların üzerinde ek yapılandırma sağlar.

Burada bir örnek azds.yaml dosyası kullanılarak oluşturulan [.NET Çekirdek örnek uygulaması:](https://github.com/Azure/dev-spaces/tree/master/samples/dotnetcore/getting-started/webfrontend)

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

Komut tarafından oluşturulan `azds.yaml` dosya, basit, tek proje geliştirme senaryosu için çalışmak üzere tasarlanmıştır. `prep` Belirli projenizkarmaşıklığı artırdıysa, komutu çalıştırdıktan sonra bu `prep` dosyayı güncelleştirmeniz gerekebilir. Örneğin, projeniz geliştirme veya hata ayıklama gereksinimlerinize bağlı olarak yapı veya başlatma sürecinizde bazı değişiklikler gerektirebilir. Projenizde birden çok yapı işlemi veya farklı bir yapı içeriği gerektiren birden çok uygulama nız da olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Kodunuzu geliştirme alanınızda çalıştırma hakkında daha fazla bilgi edinmek için [Azure Dev Spaces ile kodunuzu çalıştırmanın nasıl çalıştığını][how-it-works-up]öğrenin.

Projenizi Azure Geliştirme Alanı'na hazırlamak için Azure Geliştirme Alanları'nı kullanmaya başlamak için aşağıdaki hızlı başlangıçlara bakın:

* [Visual Studio Code ve Java ile hızla yineleyip hata ayıklama][quickstart-java]
* [Visual Studio Code ve .NET ile hızla yineleyip hata ayıklama][quickstart-netcore]
* [Visual Studio Code ve Node.js ile hızla yineleyin ve hata ayıklama][quickstart-node]
* [Visual Studio ve .NET Core ile hızla yineleyin ve hata ayıklama][quickstart-vs]
* [Kubernetes üzerinde bir uygulama geliştirmek için CLI kullanma][quickstart-cli]

[how-it-works-up]: how-dev-spaces-works-up.md
[quickstart-cli]: quickstart-cli.md
[quickstart-java]: quickstart-java.md
[quickstart-netcore]: quickstart-netcore.md
[quickstart-node]: quickstart-nodejs.md
[quickstart-vs]: quickstart-netcore-visualstudio.md