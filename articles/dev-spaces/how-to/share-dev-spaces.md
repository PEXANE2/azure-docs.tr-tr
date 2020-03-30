---
title: Azure Dev Alanları nasıl paylaşılatır?
services: azure-dev-spaces
ms.date: 05/11/2018
ms.topic: conceptual
description: Azure Kubernetes Hizmeti'ndeki dev alanı ekibinizdeki diğer kişilerle paylaşmak için Azure Geliştirme Alanlarını nasıl kullanacağınızı öğrenin
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, Miğfer, servis kafesi, servis örgü yönlendirme, kubectl, k8s '
ms.openlocfilehash: 0fcb8be5107c7769af7e51dece9f190c8e7e22df
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79474416"
---
# <a name="share-azure-dev-spaces"></a>Azure Dev Spaces'i paylaşma

Azure Dev Spaces ile geliştirme alanınızı ekibinizdeki diğer kişilerle paylaşabilirsiniz. Her geliştirici başkalarını kırma korkusu olmadan kendi alanında çalışabilir. Ayrıca, tek bir alanda birlikte çalışmak, alay oluşturmak veya bağımlılıkları simüle etmek zorunda kalmadan kodu uçlardan uca sınamaya olanak tanır. Daha fazla bilgi için ekip geliştirme kılavuzu [hakkında bilgi](../team-development-nodejs.md) edinin.

## <a name="set-up-a-dev-space-for-multiple-developers"></a>Birden çok geliştirici için bir geliştirme alanı ayarlama

1. Azure'da Dev Alanı oluşturun. [.NET Core ve VS Kodu](../get-started-netcore.md), [.NET Core ve Visual Studio](../get-started-netcore-visualstudio.md)veya [Düğüm.js ve VS Kodu'ni](../get-started-nodejs.md)seçin. Seçili Azure aboneliğine Sahip veya Katılımcı erişimine sahip olmanız gerekir.
1. Her ekip üyesinin [Azure Dev Spaces denetleyicisine erişmek için uygun izinlere](../troubleshooting.md#incorrect-rbac-permissions-for-calling-dev-spaces-controller-and-apis)sahip olduğundan emin olun. Örneğin, Azure Dev Alanı'nın kaynak **grubunu,** [Katılımcıya](/azure/active-directory/role-based-access-control-configure) her ekip üyesi için erişim hakkı verecek şekilde yapılandırabilirsiniz. Bu komutu çalıştırarak bir dev alanının kaynak grubunu denetleyebilirsiniz:`azds show-context`
1. Ekip üyelerinden geliştirme alanının geliştirilmesi için **dev alanını seçmelerini** isteyin.
   * **Komut satırı veya VS Kodu**: Erişebildiğiniz mevcut `azds space list`Azure Geliştirme Alanlarını görmek için: . Bir dev alanı `azds space select`seçmek için: .
   * **Visual Studio IDE**: Visual Studio'da bir proje açın, başlatma ayarlarından **Azure Dev Spaces'i** seçin. Açılan iletişim kutusunda varolan bir kümeyi seçin.

     ![Visual Studio başlatma ayarları açılır](../media/get-started-netcore-visualstudio/LaunchSettings.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [ekip geliştirme hakkında bilgi](../team-development-nodejs.md) edinin.
