---
title: Azure Dev Alanlarına Giriş
services: azure-dev-spaces
ms.date: 05/07/2019
ms.topic: overview
description: Azure Geliştirme Alanları'nın Azure Kubernetes Hizmet kümelerinde ekipler için nasıl hızlı, yinelemeli Kubernetes geliştirme deneyimi sağladığını öğrenin
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, kubectl, k8s
manager: gwallace
ms.openlocfilehash: 586b19070ec36517add21f7aac86ddf15121be2d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240705"
---
# <a name="introduction-to-azure-dev-spaces"></a>Azure Dev Alanlarına Giriş

Azure Dev Spaces, Azure Kubernetes Service (AKS) kümelerinde bulunan ekipler için hızlı, yinelemeli Kubernetes geliştirme deneyimi sağlar. Azure Dev Spaces ayrıca, uygulamanızın tüm bileşenlerini aks'ta en az geliştirme makinesi kurulumuyla, bağımlılıkları çoğaltmadan veya alay etmeden ayıklamanıza ve test etmenizi sağlar.

![](media/azure-dev-spaces/collaborate-graphic.gif)

## <a name="how-azure-dev-spaces-simplifies-kubernetes-development"></a>Azure Dev Spaces’in Kubernetes geliştirmeyi kolaylaştırması

Azure Dev Spaces, ekiplerin aks'te çalışan tüm mikro hizmet mimarisi veya uygulamalarıyla doğrudan çalışmalarına izin vererek ekiplerin mikro hizmet uygulamalarının geliştirilmesine ve hızlı bir şekilde yinelenmesine odaklanmalarına yardımcı olur. Azure Dev Spaces, aks kümesinin geri kalanını veya diğer geliştiricileri etkilemeden mikrohizmetler mimarinizin bölümlerini bağımsız olarak bağımsız olarak güncelleştirmenin bir yolunu da sağlar. Azure Dev Alanları, alt düzey geliştirme ve test ortamlarında geliştirme ve sınama içindir ve üretim AKS kümelerinde çalıştırılması amaçlanmamıştır.

Ekipler tüm uygulamayla çalışabildiği ve doğrudan AKS'de işbirliği yapabildiği için Azure Geliştirme Alanları:

* Yerel makine kurulumını en aza indirir
* Takımdaki yeni geliştiricilerin kurulum süresini kısaltıyor
* Daha hızlı yineleme yoluyla takımın hızını artırır
* Ekip üyeleri bir kümeyi paylaşabildiği için gereksiz geliştirme ve tümleştirme ortamlarının sayısını azaltır
* Bağımlılıkları çoğaltma veya alay etme gereksinimini ortadan kaldırır
* DevOps ekipleri gibi geliştirme ekiplerinin yanı sıra birlikte çalıştıkları ekipler arasında işbirliğini geliştirir

Azure Dev Spaces, projeleriniz için Docker ve Kubernetes varlıkları oluşturmak için araç sağlar. Bu takım, hem dev alana hem de diğer AKS kümelerine kolayca yeni ve varolan uygulamalar eklemenize olanak tanır.

Azure Geliştirme Alanları'nın nasıl çalıştığı hakkında daha fazla bilgi için Azure [Dev Alanları'nın nasıl çalıştığı ve nasıl yapılandırıldığı][how-dev-spaces-works]nabakını öğrenin.

## <a name="supported-regions-and-configurations"></a>Desteklenen bölgeler ve yapılandırmalar

Azure Dev Spaces yalnızca [bazı bölgelerdeki][supported-regions]AKS kümeleri tarafından desteklenir. Azure Dev Spaces, AKS üzerinde uygulamalarınızı derleyip çalıştırmak için [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) veya [Visual Studio Code](https://code.visualstudio.com/download) ile birlikte Linux, MacOS veya Windows 8 ya da sonraki sürümlerde yüklü [Azure Dev Spaces uzantısının](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) kullanılmasını gerektirir. Ayrıca, Windows 8 veya daha büyük bir yerde yüklü [olan Visual Studio'nun](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) kullanılmasını da destekler. Visual Studio 2019 için Azure Geliştirme iş yükünün gerekir. Visual Studio 2017 [için Kubernetes için](https://aka.ms/get-vsk8stools)Web Geliştirme iş yüküne ve Visual Studio Araçlarına ihtiyacınız olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Azure Dev Spaces'e sahip ekipler için hızlı ve yinelemeli geliştirme hakkında daha fazla bilgi edinin ve ekip geliştirme hızlı bir şekilde başlayın.

> [!div class="nextstepaction"]
> [Takım Geliştirme hızlı başlat](quickstart-team-development.md)


[how-dev-spaces-works]: how-dev-spaces-works.md
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[team-development-quickstart]: quickstart-team-development.md
