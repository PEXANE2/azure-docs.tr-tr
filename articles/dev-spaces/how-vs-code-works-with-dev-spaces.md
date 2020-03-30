---
title: Azure Dev Spaces ile Visual Studio Code nasıl çalışır?
services: azure-dev-spaces
ms.date: 07/08/2019
ms.topic: conceptual
description: Visual Studio Code ve Azure Dev Spaces'in Hata ayıklamanıza ve Kubernetes uygulamalarınızı hızla yinelemenize nasıl yardımcı olduğunu öğrenin
keywords: Azure Dev Alanlar, Dev Alanlar, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler
ms.openlocfilehash: 91440e59fdb8c21579ef1f04e78e66f933221ba0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240458"
---
# <a name="how-visual-studio-code-works-with-azure-dev-spaces"></a>Azure Dev Spaces ile Visual Studio Code nasıl çalışır?

Azure Dev Spaces ile hizmetlerinizi hazırlamak, çalıştırmak ve hata ayıklamak için Visual Studio Code ve [Azure Dev Spaces uzantısını][azds-extension] kullanabilirsiniz. Visual Studio Code ve Azure Dev Spaces uzantısı ile şunları yapabilirsiniz:

* AKS'de hizmetleri çalıştırmak ve hata ayıklama için varlıklar oluşturma
* Java, Node.js ve .NET Core hizmetlerinizi bir dev alanında çalıştırın
* Bir dev alanında çalışan Java, Node.js ve .NET Core hizmetlerinizi doğrudan hata ayıklama

## <a name="generate-assets"></a>Varlık oluşturma

Visual Studio Code ve Azure Geliştirme Alanları uzantısı, projeniz için aşağıdaki varlıkları oluşturur:

* Maven, Node.js uygulamaları ve .NET Core uygulamalarını kullanarak Java uygulamaları için Dockerfiles
* Dockerfile ile hemen hemen her dil için miğfer grafikleri
* Projeniz için Azure Geliştirme Alanları yapılandırma dosyası olan bir `azds.yaml` [dosya][azds-yaml]
* Maven, Node.js uygulamaları ve .NET Core uygulamalarını kullanarak Java uygulamaları için projenizin Visual Studio Code başlatma yapılandırmasını içeren bir `.vscode` klasör

Dockerfile, Helm grafiği `azds.yaml` ve dosyaları çalışırken `azds prep`oluşturulan aynı varlıklardır. Bu dosyalar, projenizi AKS'de `azds up`çalıştırmak için Visual Studio kodunun dışında da kullanılabilir. Klasör `.vscode` yalnızca Visual Studio Kodu'ndan AKS'de projenizi çalıştırmak için Visual Studio kodu tarafından kullanılır.

## <a name="run-your-service-in-aks"></a>AKS'de hizmetinizi yürütün

Projeniz için varlıkları oluşturduktan sonra, Visual Studio Code'tan java, Node.js ve .NET Core hizmetlerinizi varolan bir geliştirme alanında çalıştırabilirsiniz. Visual Studio Code'un *Hata Ayıklama* sayfasında, projenizi `.vscode` çalıştırmak için dizinden başlatma yapılandırmasını çağırabilirsiniz.

AKS kümenizi oluşturmalı ve Visual Studio Code dışında kümenizde Azure Dev Alanları'nı etkinleştirmelisiniz. Örneğin, bu kurulumu yapmak için Azure CLI'yi veya Azure portalını kullanabilirsiniz. Çalışan tarafından oluşturulan varlıklar gibi, Visual `azds.yaml` Studio Code dışında oluşturulan varolan Dockerfiles, `azds prep`Helm grafikleri ve dosyaları yeniden kullanabilirsiniz. Visual Studio Code dışında oluşturulan varlıkları yeniden kullanırsanız, yine `.vscode` de bir dizine sahip olmanız gerekir. Bu `.vscode` dizin Visual Studio kodu ve Azure Dev Spaces uzantısı tarafından yeniden oluşturulabilir ve varolan varlıklarınızın üzerine yazmaz.

.NET Core projeleri için ,NET hizmetinizi Visual Studio Code'tan çalıştırmak için [C# uzantısı][csharp-extension] yüklü olmalıdır. Ayrıca Maven'i kullanan Java projeleri için Azure [Dev Spaces uzantısı için Java Hata Ayıklayıcı'nın][java-extension] yanı sıra [Maven'in][maven] Visual Studio Code'tan Java hizmetinizi çalıştıracak şekilde yüklü ve yapılandırılmış olması gerekir.

## <a name="debug-your-service-in-aks"></a>AKS'deki hizmetinizi hata ayıklama

Projenizi başlattıktan sonra, doğrudan Visual Studio Code'tan bir dev alanında çalışan Java, Node.js ve .NET Core hizmetlerinizi hata ayıklayabilirsiniz. Dizindeki `.vscode` başlatma yapılandırması, bir dev alanında hata ayıklama etkin bir hizmet çalıştırmak için ek hata ayıklama bilgileri sağlar. Visual Studio Code ayrıca, kesme noktaları ayarlamanızı, değişkenleri incelemenize ve diğer hata ayıklama işlemlerini gerçekleştirmenize olanak tanıyan, dev boşluklarınızdaki çalışan kapsayıcıdaki hata ayıklama işlemine de eklenir.


## <a name="use-visual-studio-code-with-azure-dev-spaces"></a>Azure Geliştirme Alanları ile Visual Studio Kodunu Kullanma

Visual Studio Code ve Azure Dev Spaces uzantısının Azure Dev Spaces ile çalıştığını aşağıdaki hızlı başlangıçlarda görebilirsiniz:

* [Visual Studio Code ve Java ile hızla yineleyip hata ayıklama][quickstart-java]
* [Visual Studio Code ve .NET ile hızla yineleyip hata ayıklama][quickstart-netcore]
* [Visual Studio Code ve Node.js ile hızla yineleyin ve hata ayıklama][quickstart-node]

[azds-extension]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[azds-yaml]: how-dev-spaces-works-prep.md#prepare-your-code
[csharp-extension]: https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp
[java-extension]: https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debugger-azds
[maven]: https://maven.apache.org
[quickstart-java]: quickstart-java.md
[quickstart-netcore]: quickstart-netcore.md
[quickstart-node]: quickstart-nodejs.md
