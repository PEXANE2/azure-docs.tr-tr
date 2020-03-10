---
title: Visual Studio Code nasıl kullanılır Azure Dev Spaces
services: azure-dev-spaces
ms.date: 07/08/2019
ms.topic: conceptual
description: Visual Studio Code ve Azure Dev Spaces Kubernetes uygulamalarınızı hata ayıklamanıza ve hızla yinelemenize nasıl yardımcı olduğunu öğrenin
keywords: Azure Dev Spaces, dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar
ms.openlocfilehash: b6ab1330399ab2eb7afc9be61a7767a22ca82320
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78942490"
---
# <a name="how-visual-studio-code-works-with-azure-dev-spaces"></a>Visual Studio Code nasıl kullanılır Azure Dev Spaces

Visual Studio Code ve [Azure dev Spaces uzantısını][azds-extension] kullanarak hizmetlerinizi Azure dev Spaces hazırlayın, çalıştırabilir ve hatalarını ayıklayabilirsiniz. Visual Studio Code ve Azure Dev Spaces uzantısıyla şunları yapabilirsiniz:

* AKS 'de çalışan ve hata ayıklama Hizmetleri için varlıklar oluşturma
* Java, Node. js ve .NET Core hizmetlerinizi bir geliştirme alanında çalıştırın
* Geliştirme alanında çalışan Java, Node. js ve .NET Core hizmetlerinde doğrudan hata ayıklayın

## <a name="generate-assets"></a>Varlık oluşturma

Visual Studio Code ve Azure Dev Spaces uzantısı projeniz için aşağıdaki varlıkları oluşturur:

* Maven, Node. js uygulamaları ve .NET Core uygulamaları kullanılarak sunulan Java uygulamaları için dockerfiles
* Dockerfile ile neredeyse tüm diller için helb grafikleri
* Projeniz için [Azure dev Spaces yapılandırma dosyası][azds-yaml] olan bir `azds.yaml` dosyası
* Maven, Node. js uygulamalarını ve .NET Core uygulamalarını kullanarak Java uygulamaları için projenizin Visual Studio Code başlatma yapılandırmasına sahip `.vscode` bir klasör

Dockerfile, HELI grafiği ve `azds.yaml` dosyaları, `azds prep`çalıştırılırken oluşturulan varlıklardır. Bu dosyalar, Visual Studio Code dışında, projenizi `azds up`çalıştırma gibi AKS 'de çalıştırmak için de kullanılabilir. `.vscode` klasörü yalnızca Visual Studio Code tarafından, projenizi Visual Studio Code ' de AKS 'de çalıştırmak için kullanılır.

## <a name="run-your-service-in-aks"></a>Hizmetinizi AKS 'de çalıştırın

Projeniz için varlıkları oluşturduktan sonra, Java, Node. js ve .NET Core hizmetlerinizi Visual Studio Code ' den mevcut bir geliştirme alanında çalıştırabilirsiniz. Visual Studio Code *hata ayıklama* sayfasında, projenizi çalıştırmak için `.vscode` dizininden başlatma yapılandırmasını çağırabilirsiniz.

AKS kümenizi oluşturmanız ve kümenizdeki Azure Dev Spaces Visual Studio Code dışında etkinleştirmeniz gerekir. Örneğin, bu kurulumu yapmak için Azure CLı veya Azure portal kullanabilirsiniz. Mevcut Dockerfiles, helk grafiklerini ve Visual Studio Code dışında oluşturulan `azds.yaml` dosyalarını, `azds prep`çalıştırılarak oluşturulan varlıklar gibi yeniden kullanabilirsiniz. Visual Studio Code dışında oluşturulan varlıkları yeniden kullanıyorsanız, yine de bir `.vscode` dizinine sahip olmanız gerekir. Bu `.vscode` Dizin, Visual Studio Code ve Azure Dev Spaces uzantısı tarafından yeniden üretilebilir ve var olan varlıklarınızın üzerine yazılmayacak.

.NET Core projeleri için, Visual Studio Code 'den .net hizmetinizi çalıştırmak için [ C# uzantının][csharp-extension] yüklü olması gerekir. Ayrıca Maven kullanan Java projeleri için, [Azure dev Spaces uzantısı Için Java Debugger][java-extension] 'ın yanı sıra, Visual Studio Code ' dan Java hizmetinizi çalıştıracak şekilde [yüklenip yapılandırılmış][maven] olması gerekir.

## <a name="debug-your-service-in-aks"></a>AKS 'de hizmetinizde hata ayıklama

Projenizi başlattıktan sonra, geliştirici alanında çalışan Java, Node. js ve .NET Core hizmetlerinde doğrudan Visual Studio Code hata ayıklaması yapabilirsiniz. `.vscode` dizinindeki başlatma yapılandırması, bir geliştirme alanında hata ayıklama etkinken bir hizmeti çalıştırmaya yönelik ek hata ayıklama bilgileri sağlar. Visual Studio Code Ayrıca, geliştirme boşluklarında çalışan kapsayıcıda hata ayıklama işlemine ekler. böylece kesme noktaları ayarlayabilir, değişkenleri inceleyebilir ve diğer hata ayıklama işlemlerini gerçekleştirebilirsiniz.


## <a name="use-visual-studio-code-with-azure-dev-spaces"></a>Azure Dev Spaces ile Visual Studio Code kullanma

Aşağıdaki hızlı başlangıçlarda Visual Studio Code ve Azure Dev Spaces uzantısının Azure Dev Spaces ile çalıştığı hakkında bilgi alabilirsiniz:

* [Visual Studio Code ve Java ile hızlıca yineleme ve hata ayıklama][quickstart-java]
* [Visual Studio Code ve .NET ile hızla yineleme ve hata ayıklama][quickstart-netcore]
* [Visual Studio Code ve Node. js ile hızlıca yineleme ve hata ayıklama][quickstart-node]

[azds-extension]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[azds-yaml]: how-dev-spaces-works.md#prepare-your-code
[csharp-extension]: https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp
[java-extension]: https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debugger-azds
[maven]: https://maven.apache.org
[quickstart-java]: quickstart-java.md
[quickstart-netcore]: quickstart-netcore.md
[quickstart-node]: quickstart-nodejs.md
