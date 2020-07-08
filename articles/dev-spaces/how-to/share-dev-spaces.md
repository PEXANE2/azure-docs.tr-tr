---
title: Azure Dev Spaces paylaşma
services: azure-dev-spaces
ms.date: 05/11/2018
ms.topic: conceptual
description: Azure Kubernetes hizmetindeki bir geliştirme alanını takımınızda başkalarıyla paylaşmak için Azure Dev Spaces kullanmayı öğrenin
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s '
ms.openlocfilehash: 0fcb8be5107c7769af7e51dece9f190c8e7e22df
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79474416"
---
# <a name="share-azure-dev-spaces"></a>Azure Dev Spaces'i paylaşma

Azure Dev Spaces sayesinde, geliştirme alanınızı takımınızda başkalarıyla paylaşabilirsiniz. Her geliştirici, diğerlerinin parçalara bölünmesi gerekmeden kendi alanında çalışabilir. Ayrıca, tek bir alanda birlikte çalışmak, hareketleri oluşturmak veya bağımlılıklara benzetim yapmak zorunda kalmadan kodu uçtan uca test etmeyi olanaklı hale getirebilirsiniz. Daha fazla bilgi için bkz. [Takım geliştirme Kılavuzu hakkında bilgi edinin](../team-development-nodejs.md) .

## <a name="set-up-a-dev-space-for-multiple-developers"></a>Birden çok geliştirici için bir geliştirme alanı ayarlama

1. Azure 'da bir geliştirme alanı oluşturun. [.NET Core ve vs Code](../get-started-netcore.md), [.NET Core ve Visual Studio](../get-started-netcore-visualstudio.md)veya [Node.js ve vs Code](../get-started-nodejs.md)seçeneklerini belirleyin. Seçili Azure aboneliğine sahip veya katkıda bulunan erişimine sahip olmanız gerekir.
1. Her bir takım üyesinin [Azure dev Spaces denetleyicisine erişmek için uygun izinlere](../troubleshooting.md#incorrect-rbac-permissions-for-calling-dev-spaces-controller-and-apis)sahip olduğundan emin olun. Örneğin, Azure dev alanının **kaynak grubunu** her bir ekip üyesine [katkıda bulunan erişim verecek](/azure/active-directory/role-based-access-control-configure) şekilde yapılandırabilirsiniz. Şu komutu çalıştırarak bir geliştirme alanının kaynak grubunu kontrol edebilirsiniz:`azds show-context`
1. Ekip üyelerinin, içinde geliştirme için **geliştirme alanını seçmesini** isteyin.
   * **Komut satırı veya vs Code**: erişiminizin bulunduğu mevcut Azure dev Spaces görmek için: `azds space list` . Bir dev alanı seçmek için: `azds space select` .
   * **Visual STUDIO IDE**: Visual Studio 'da bir proje açın, başlatma ayarları açılır listesinden **Azure dev Spaces** seçin. Açılan iletişim kutusunda, mevcut bir kümeyi seçin.

     ![Visual Studio başlatma ayarları açılan ayarı](../media/get-started-netcore-visualstudio/LaunchSettings.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Takım geliştirme hakkında bilgi edinin](../team-development-nodejs.md) .
