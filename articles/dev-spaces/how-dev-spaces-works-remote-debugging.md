---
title: Azure Dev Spaces ile kodunuzu uzaktan hata ayıklama nasıl çalışır?
services: azure-dev-spaces
ms.date: 03/24/2020
ms.topic: conceptual
description: Azure Geliştirme Alanları ile Azure Kubernetes Hizmeti'nde uzaktan hata ayıklama işlemlerini açıklar
keywords: Azure Dev Alanlar, Dev Alanlar, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler
ms.openlocfilehash: a9f303ea6f954285a319be137a594f7ce127f740
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80241406"
---
# <a name="how-remote-debugging-your-code-with-azure-dev-spaces-works"></a>Azure Dev Spaces ile kodunuzu uzaktan hata ayıklama nasıl çalışır?

Azure Geliştirme Alanları, Kubernetes uygulamalarını hızla tekrarlayıp hata ayıklamanın ve bir Azure Kubernetes Hizmeti (AKS) kümesinde ekibinizle işbirliği yapmanın birden çok yolunu sağlar. Projeniz bir geliştirme alanında çalıştırDıktan sonra, Azure Geliştirme Spaces AKS'de çalışan bir uygulamaya ekleme nin ve hata ayıklamanın bir yolunu sağlar.

Bu makalede, Dev Spaces ile uzaktan hata ayıklamanın nasıl çalıştığı açıklanmaktadır.

## <a name="debug-your-code"></a>Kodunuzda hata ayıklama

Java, .NET Core ve Node.js uygulamaları için Visual Studio Code veya Visual Studio'yu kullanarak doğrudan geliştirme alanınızda çalışan uygulamanızı hata ayıklayabilirsiniz. Visual Studio Code ve Visual Studio, geliştirme alanınıza bağlanmak, uygulamanızı başlatmak ve bir hata ayıklayıcı takmak için araç sağlar. Çalıştırdıktan `azds prep`sonra, Visual Studio Code veya Visual Studio projenizi açabilirsiniz. Visual Studio Code veya Visual Studio çalışan `azds prep`ayrı bağlanmak için kendi yapılandırma dosyaları oluşturacaktır. Visual Studio Code veya Visual Studio içinden, kesme noktaları ayarlayabilir ve uygulamanızı geliştirme alanınıza başlatabilirsiniz.

![Kodunuzu hata ayıklama](media/get-started-node/debug-configuration-nodejs2.png)

Hata ayıklama için Visual Studio Code veya Visual Studio kullanarak uygulamabaşlattığınızda, onlar başlatma ve çalışan `azds up`aynı şekilde dev alana bağlanma ele. Ayrıca, Visual Studio Code ve Visual Studio istemci tarafı araç hata ayıklama için belirli bilgileri içeren ek bir parametre sağlar. Parametre hata ayıklama görüntüsünün adını, hata ayıklamanın görüntüsündeki hata ayıklamanın konumunu ve hata ayıklama klasörünü monte etmek için uygulamanın kapsayıcıiçindeki hedef konumunu içerir.

Hata ayıklama görüntüsü istemci tarafındaki takım tarafından otomatik olarak belirlenir. Dockerfile ve Helm grafiği sırasında kullanılan yönteme benzer `azds prep`bir yöntem kullanır. Hata ayıklama uygulamanın görüntüsüne monte edildikten sonra, .. `azds exec`

## <a name="next-steps"></a>Sonraki adımlar

Projenizi uzaktan hata ayıklamak için Azure Geliştirme Alanları'nı kullanmaya başlamak için aşağıdaki hızlı başlangıçlara bakın:

* [Visual Studio Code ve Java ile hızla yineleyip hata ayıklama][quickstart-java]
* [Visual Studio Code ve .NET ile hızla yineleyip hata ayıklama][quickstart-netcore]
* [Visual Studio Code ve Node.js ile hızla yineleyin ve hata ayıklama][quickstart-node]
* [Visual Studio ve .NET Core ile hızla yineleyin ve hata ayıklama][quickstart-vs]


[quickstart-java]: quickstart-java.md
[quickstart-netcore]: quickstart-netcore.md
[quickstart-node]: quickstart-nodejs.md
[quickstart-vs]: quickstart-netcore-visualstudio.md
