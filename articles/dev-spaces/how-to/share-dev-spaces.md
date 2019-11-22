---
title: Azure Dev Spaces paylaşma
services: azure-dev-spaces
ms.date: 05/11/2018
ms.topic: conceptual
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s '
ms.openlocfilehash: 2c85625a4f61b701bc1e8b0a4a06f71dc0989ce0
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74279987"
---
# <a name="share-azure-dev-spaces"></a>Azure Dev Spaces'i paylaşma

Azure Dev Spaces sayesinde, geliştirme alanınızı takımınızda başkalarıyla paylaşabilirsiniz. Her geliştirici, diğerlerinin parçalara bölünmesi gerekmeden kendi alanında çalışabilir. Ayrıca, tek bir alanda birlikte çalışmak, hareketleri oluşturmak veya bağımlılıklara benzetim yapmak zorunda kalmadan kodu uçtan uca test etmeyi olanaklı hale getirebilirsiniz. Daha fazla bilgi için bkz. [Takım geliştirme Kılavuzu hakkında bilgi edinin](../team-development-nodejs.md) .

## <a name="set-up-a-dev-space-for-multiple-developers"></a>Birden çok geliştirici için bir geliştirme alanı ayarlama

1. Azure 'da bir geliştirme alanı oluşturun. [.NET Core ve vs Code](../get-started-netcore.md), [.NET Core ve Visual Studio](../get-started-netcore-visualstudio.md)ya da [Node. js ve vs Code](../get-started-nodejs.md)seçin. Seçili Azure aboneliğine sahip veya katkıda bulunan erişimine sahip olmanız gerekir.
1. Her ekip üyesine [katkıda bulunan erişim sağlamak](/azure/active-directory/role-based-access-control-configure) Için Azure dev alanının **kaynak grubunu** yapılandırın. Şu komutu çalıştırarak bir geliştirme alanının kaynak grubunu kontrol edebilirsiniz: `azds list-up`
1. Ekip üyelerinin, içinde geliştirme için **geliştirme alanını seçmesini** isteyin.
   * **Komut satırı veya vs Code**: `azds space list`erişiminizin bulunduğu mevcut Azure dev Spaces görmek için:. Bir dev alanı seçmek için: `azds space select`.
   * **Visual STUDIO IDE**: Visual Studio 'da bir proje açın, başlatma ayarları açılır listesinden **Azure dev Spaces** seçin. Açılan iletişim kutusunda, mevcut bir kümeyi seçin.

     ![Visual Studio başlatma ayarları açılan ayarı](../media/get-started-netcore-visualstudio/LaunchSettings.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Takım geliştirme hakkında bilgi edinin](../team-development-nodejs.md) .
