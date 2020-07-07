---
title: Azure Dev Spaces ile kodunuzda uzaktan hata ayıklama nasıl yapılır?
services: azure-dev-spaces
ms.date: 03/24/2020
ms.topic: conceptual
description: Azure Dev Spaces ile Azure Kubernetes hizmetinde uzaktan hata ayıklama işlemlerini açıklar
keywords: Azure Dev Spaces, dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar
ms.openlocfilehash: a9f303ea6f954285a319be137a594f7ce127f740
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80241406"
---
# <a name="how-remote-debugging-your-code-with-azure-dev-spaces-works"></a>Azure Dev Spaces ile kodunuzda uzaktan hata ayıklama nasıl yapılır?

Azure Dev Spaces, Kubernetes uygulamalarının hızla yinelenemez ve hata ayıklamanın yanı sıra Azure Kubernetes hizmeti (AKS) kümesinde ekibinizle işbirliği yapmak için birden çok yol sağlar. Projeniz bir geliştirme alanında çalışmaya başladıktan sonra, Azure Dev Spaces, AKS 'de çalışan bir uygulamaya eklemek ve hata ayıklamak için bir yol sağlar.

Bu makalede, dev Spaces ile uzaktan hata ayıklamanın nasıl çalıştığı açıklanır.

## <a name="debug-your-code"></a>Kodunuzda hata ayıklama

Java, .NET Core ve Node.js uygulamaları için, Visual Studio Code veya Visual Studio kullanarak doğrudan geliştirme alanınızda çalışan uygulamanızda hata ayıklaması yapabilirsiniz. Visual Studio Code ve Visual Studio, geliştirme alanınıza bağlanmak, uygulamanızı başlatmak ve bir hata ayıklayıcı eklemek için araç sağlar. Çalıştırdıktan sonra `azds prep` projenizi Visual Studio Code veya Visual Studio 'da açabilirsiniz. Visual Studio Code veya Visual Studio, bağlanmak için kendi yapılandırma dosyalarını oluşturur ve bu ayrı çalışır `azds prep` . Visual Studio Code veya Visual Studio içinden kesme noktaları ayarlayabilir ve uygulamanızı geliştirme alanınızda başlatabilirsiniz.

![Kodunuzda hata ayıklama](media/get-started-node/debug-configuration-nodejs2.png)

Hata ayıklama için Visual Studio Code veya Visual Studio 'Yu kullanarak uygulamanızı başlattığınızda, geliştirme alanınıza çalıştırmayı ve bu şekilde çalıştırma işlemini işler `azds up` . Ayrıca, Visual Studio Code ve Visual Studio 'daki istemci tarafı araçları, hata ayıklama için özel bilgilerle ek bir parametre sağlar. Parametresi hata ayıklayıcı görüntüsünün adını, hata ayıklayıcının görüntüsündeki içindeki hata ayıklayıcının konumunu ve hata ayıklayıcı klasörünü bağlamak için uygulamanın kapsayıcısı içindeki hedef konumu içerir.

Hata ayıklayıcı görüntüsü, istemci tarafı araçları tarafından otomatik olarak belirlenir. Dockerfile sırasında kullanılan yönteme benzer bir yöntem kullanır ve hele grafiği çalışırken üretir `azds prep` . Hata ayıklayıcı uygulamanın görüntüsüne bağlandıktan sonra kullanılarak çalıştırılır `azds exec` .

## <a name="next-steps"></a>Sonraki adımlar

Projenizde uzaktan hata ayıklama için Azure Dev Spaces kullanmaya başlamak için aşağıdaki hızlı başlangıçlara bakın:

* [Visual Studio Code ve Java ile hızlıca yineleme ve hata ayıklama][quickstart-java]
* [Visual Studio Code ve .NET ile hızla yineleme ve hata ayıklama][quickstart-netcore]
* [Visual Studio Code ve Node.jsile hızlıca yineleme ve hata ayıklama][quickstart-node]
* [Visual Studio ve .NET Core ile hızla yineleme ve hata ayıklama][quickstart-vs]


[quickstart-java]: quickstart-java.md
[quickstart-netcore]: quickstart-netcore.md
[quickstart-node]: quickstart-nodejs.md
[quickstart-vs]: quickstart-netcore-visualstudio.md
