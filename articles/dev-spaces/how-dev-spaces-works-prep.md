---
title: Projeyi Azure Dev Spaces için hazırlama
services: azure-dev-spaces
ms.date: 03/24/2020
ms.topic: conceptual
description: Projenizin Azure Dev Spaces çalışma ile nasıl hazırlanıldığını açıklar
keywords: azds. YAML, Azure Dev Spaces, dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar
ms.openlocfilehash: 24a54fffdc8e94493d2a4a9aeb1c5f02dcd192b9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80241640"
---
# <a name="how-preparing-a-project-for-azure-dev-spaces-works"></a>Projeyi Azure Dev Spaces için hazırlama

Azure Dev Spaces, Kubernetes uygulamalarının hızla yinelenemez ve hata ayıklamanın yanı sıra Azure Kubernetes hizmeti (AKS) kümesinde ekibinizle işbirliği yapmak için birden çok yol sağlar. Geliştirme alanları, projeniz için Dockerfiles ve Help grafikleri oluşturabilir. Dev Spaces Ayrıca AKS 'te Kubernetes uygulamalarınızı dağıtmak, çalıştırmak ve hata ayıklamak için bir yapılandırma dosyası oluşturur ve kullanır. Bu dosyaların tümü uygulamanızın koduyla birlikte bulunur ve sürüm denetim sisteminize eklenebilir.

Bu makalede, projenizi dev Spaces ile AKS 'de çalışmaya hazırlarken ne olacağı açıklanmaktadır.

## <a name="prepare-your-code"></a>Kodunuzu hazırlayın

Uygulamanızı bir geliştirme alanında çalıştırmak için Kapsayıcılı olması gerekir ve Kubernetes 'e nasıl dağıtılması gerektiğini tanımlamanız gerekir. Uygulamanızı konteynize etmek için bir Dockerfile gerekir. Uygulamanızın Kubernetes 'e nasıl dağıtıldığını tanımlamak için bir [HELI grafiğine](https://docs.helm.sh/)ihtiyacınız vardır. Uygulamanız için hem Dockerfile hem de Held grafiği oluşturmaya yardımcı olmak için, istemci tarafı araçları şu `prep` komutu sağlar:

```cmd
azds prep --enable-ingress
```

`prep`Komut, projenizdeki dosyalara bakar ve uygulamanızı Kubernetes 'te çalıştırmak Için Dockerfile ve helk grafiğini oluşturmaya çalışır. Şu anda `prep` komut şu dilleri içeren bir Dockerfile ve HELI grafiği oluşturacak:

* Java
* Node.js
* .NET Core

*must* `prep` Komutunu kaynak kodu içeren bir dizinden çalıştırmanız gerekir. `prep`Komutu doğru dizinden çalıştırmak, istemci tarafı araçlarının dili belirlemesine ve uygulamanızı kapsayıtacak uygun bir Dockerfile oluşturmasına izin verir. `prep`Komutunu, Java projeleri için *pom.xml* dosyası içeren bir dizinden da çalıştırabilirsiniz.

`prep`Komutunu kaynak kodu içermeyen dizinden çalıştırırsanız, istemci tarafı araçları Dockerfile oluşturmaz. Ayrıca, *Desteklenmeyen bir dil nedeniyle Dockerfile*oluşturulamadı hatası görüntülenir. Bu hata, istemci tarafı araçları proje türünü tanımadığı zaman da oluşur.

`prep`Komutunu çalıştırdığınızda, bayrağını belirtme seçeneğiniz vardır `--enable-ingress` . Bu bayrak, denetleyiciye bu hizmet için internet 'ten erişilebilen bir uç nokta oluşturmasını söyler. Bu bayrağı belirtmezseniz, hizmete yalnızca küme içinden veya istemci tarafı araçları tarafından oluşturulan localhost tüneli kullanılarak erişilebilir. Oluşturulan Held grafiğini güncelleştirerek komutu çalıştırdıktan sonra bu davranışı etkinleştirebilir veya devre dışı bırakabilirsiniz `prep` .

`prep`Bu komut, projenizde mevcut olan Dockerfiles veya Held grafiklerini değiştirmez. Mevcut bir Dockerfile veya HELI grafiği, komutu tarafından oluşturulan dosyalarla aynı adlandırma kuralını kullanıyorsa `prep` , `prep` komut bu dosyaları oluşturmayı atlar. Aksi takdirde, `prep` komut, var olan dosyalar üzerinde kendi Dockerfile veya Held grafiğini oluşturacaktır.

> [!IMPORTANT]
> Azure Dev Spaces, kodunuzu derlemek ve çalıştırmak için projeniz için Dockerfile ve HELI grafiğini kullanır, ancak projenin nasıl oluşturulduğunu ve çalıştırılacağını değiştirmek istiyorsanız bu dosyaları değiştirebilirsiniz.

`prep`Bu komut ayrıca `azds.yaml` projenizin kökünde bir dosya oluşturur. Azure Dev Spaces uygulamanızı derlemek, yüklemek, yapılandırmak ve çalıştırmak için bu dosyayı kullanır. Bu yapılandırma dosyası, Dockerfile ve Held grafiğinin konumunu listeler ve ayrıca bu yapıtların üstünde ek yapılandırma sağlar.

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

`azds.yaml`Komut tarafından oluşturulan dosya `prep` basit, tek proje geliştirme senaryosunda çalışmak üzere tasarlanmıştır. Belirli projeniz karmaşıklığa sahipse, komutunu çalıştırdıktan sonra bu dosyayı güncelleştirmeniz gerekebilir `prep` . Örneğin, projeniz geliştirme veya hata ayıklama gereksinimlerinize göre derleme veya başlatma işleminde bazı değişiklikler yapılmasını gerektirebilir. Ayrıca, projenizde birden çok derleme işlemi veya farklı derleme içeriği gerektiren birden fazla uygulamanız olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Geliştirme alanınızda kodunuzu çalıştırma hakkında daha fazla bilgi edinmek için bkz. [Azure dev Spaces ile kodunuzu çalıştırma nasıl çalışır?][how-it-works-up].

Projenizi Azure dev alanı için hazırlamak üzere Azure Dev Spaces kullanmaya başlamak için aşağıdaki hızlı başlangıçlara bakın:

* [Visual Studio Code ve Java ile hızlıca yineleme ve hata ayıklama][quickstart-java]
* [Visual Studio Code ve .NET ile hızla yineleme ve hata ayıklama][quickstart-netcore]
* [Visual Studio Code ve Node.jsile hızlıca yineleme ve hata ayıklama][quickstart-node]
* [Visual Studio ve .NET Core ile hızla yineleme ve hata ayıklama][quickstart-vs]
* [Kubernetes 'te uygulama geliştirmek için CLı kullanma][quickstart-cli]

[how-it-works-up]: how-dev-spaces-works-up.md
[quickstart-cli]: quickstart-cli.md
[quickstart-java]: quickstart-java.md
[quickstart-netcore]: quickstart-netcore.md
[quickstart-node]: quickstart-nodejs.md
[quickstart-vs]: quickstart-netcore-visualstudio.md