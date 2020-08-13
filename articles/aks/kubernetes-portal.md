---
title: Azure portal (Önizleme) ile Kubernetes kaynaklarına erişin
description: Azure portal Azure Kubernetes hizmeti (AKS) kümesini yönetmek için Kubernetes kaynaklarıyla nasıl etkileşim kuracağınızı öğrenin.
services: container-service
author: laurenhughes
ms.topic: article
ms.date: 08/11/2020
ms.author: lahugh
ms.openlocfilehash: 109192efa19605af003dcfb30592c865ce7495b5
ms.sourcegitcommit: 1aef4235aec3fd326ded18df7fdb750883809ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88136991"
---
# <a name="access-kubernetes-resources-from-the-azure-portal-preview"></a>Azure portal (Önizleme) ile Kubernetes kaynaklarına erişin

Azure portal, Azure Kubernetes Service (AKS) kümenizdeki Kubernetes kaynaklarına kolay erişim için bir Kubernetes kaynak Görüntüleyici (Önizleme) içerir. Azure portal Kubernetes kaynaklarını görüntüleme, Azure portal ve komut satırı aracı arasındaki bağlam geçişini azaltır ve `kubectl` Kubernetes kaynaklarınızı görüntüleme ve düzenlemeyle ilgili deneyimi kolaylaştırır. Kaynak Görüntüleyicisi Şu anda dağıtımlar, pods ve çoğaltma kümeleri gibi birden çok kaynak türü içerir.

Azure portal Kubernetes kaynak görünümü, kullanımdan kaldırılması için ayarlanan [aks Pano eklentisinin][kubernetes-dashboard]yerini alır.

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

## <a name="prerequisites"></a>Ön koşullar

Azure portal Kubernetes kaynaklarını görüntülemek için bir AKS kümeniz olması gerekir. Herhangi bir küme desteklenir, ancak Azure Active Directory (Azure AD) Tümleştirmesi kullanılıyorsa, kümenizde [aks tarafından yönetilen Azure AD tümleştirmesi][aks-managed-aad]kullanılmalıdır. Kümeniz eski Azure AD kullanıyorsa, kümenizi portalda veya [Azure CLI][cli-aad-upgrade]ile yükseltebilirsiniz.

## <a name="view-kubernetes-resources"></a>Kubernetes kaynaklarını görüntüle

Kubernetes kaynaklarını görmek için Azure portal AKS kümenize gidin. Sol taraftaki Gezinti Bölmesi kaynaklarınıza erişmek için kullanılır. Kaynaklar şunları içerir:

- **Ad alanları** , kümenizin ad alanlarını görüntüler. Ad alanı listesinin en üstündeki filtre, ad alanı kaynaklarınızı filtrelemeye ve görüntülemeye yönelik hızlı bir yol sağlar.
- **Iş yükleri** , kümenize dağıtılan dağıtımlar, pods, çoğaltma kümeleri ve Daemon kümeleri hakkında bilgi gösterir. Aşağıdaki ekran görüntüsünde örnek bir AKS kümesindeki varsayılan sistem kümeleri gösterilmektedir.
- **Hizmetler ve** giriş kaynakları, kümenizin tüm hizmet ve giriş kaynaklarını gösterir.

:::image type="content" source="media/kubernetes-portal/workloads.png" alt-text="Kubernetes Pod bilgileri Azure portal görüntülendi." lightbox="media/kubernetes-portal/workloads.png":::

### <a name="deploy-an-application"></a>Uygulama dağıtma

Bu örnekte, [aks hızlı başlangıç][portal-quickstart]noktasından Azure oy uygulamasını dağıtmak için örnek aks kümemizi kullanacağız.

1. Kaynak görünümlerinden herhangi birinden (ad alanı, Iş yükleri veya hizmetler ve giriş) **Ekle** ' yi seçin.
1. [Aks hızlı başlangıç][portal-quickstart]konumundan Azure oy uygulaması için YAML 'yi yapıştırın.
1. Uygulamayı dağıtmak için YAML düzenleyicisinin altında **Ekle** ' yi seçin. 

YAML dosyası eklendikten sonra, Kaynak Görüntüleyicisi oluşturulan Kubernetes hizmetlerini gösterir: Azure oy uygulamasına erişmek için iç hizmet (Azure-oylamalı) ve dış hizmet (Azure-oyubi). Dış hizmet bağlı bir dış IP adresi içerdiğinden, uygulamayı tarayıcınızda kolayca görüntüleyebilmenizi sağlayabilirsiniz.

:::image type="content" source="media/kubernetes-portal/portal-services.png" alt-text="Azure oy uygulama bilgileri Azure portal görüntülendi." lightbox="media/kubernetes-portal/portal-services.png":::

### <a name="monitor-deployment-insights"></a>Dağıtım öngörülerini izleme

[Kapsayıcılar Için Azure izleyici][enable-monitor] özellikli aks kümeleri, dağıtım öngörülerini hızla görüntüleyebilir. Kullanıcılar, Kubernetes kaynakları görünümünden, CPU ve bellek kullanımı dahil olmak üzere bireysel dağıtımların canlı durumunu görebilir ve daha ayrıntılı bilgi için Azure izleyici 'ye geçiş yapabilir. Örnek bir AKS kümesinden dağıtım öngörülerine bir örnek aşağıda verilmiştir:

:::image type="content" source="media/kubernetes-portal/deployment-insights.png" alt-text="Dağıtım öngörüleri Azure portal görüntülendi." lightbox="media/kubernetes-portal/deployment-insights.png":::

## <a name="edit-yaml"></a>YAML 'yi Düzenle

Kubernetes kaynak görünümü bir YAML Düzenleyicisi de içerir. Yerleşik bir YAML Düzenleyicisi, Portal içinden Hizmetleri ve dağıtımları güncelleştirebilir veya oluşturabileceğiniz ve değişiklikleri hemen uygulayabileceðiniz anlamına gelir.

:::image type="content" source="media/kubernetes-portal/service-editor.png" alt-text="Azure portal gösterildiği bir Kubernetes hizmeti için YAML Düzenleyicisi.":::

YAML 'yi düzenledikten sonra değişiklikler, **gözden geçir + kaydet**' i seçerek, değişiklikleri onayladıktan sonra yeniden kaydedilerek uygulanır.

>[!WARNING]
> UI veya CLı aracılığıyla doğrudan üretim değişikliklerinin gerçekleştirilmesi önerilmez, [sürekli tümleştirme (CI) ve sürekli dağıtım (CD) en iyi uygulamalarından](kubernetes-action.md)yararlanın. Azure portalı Kubernetes yönetim özellikleri ve YAML Düzenleyicisi, geliştirme ve test ayarında Yeni dağıtımlar için öğrenmede ve bu dağıtım için oluşturulmuştur.

## <a name="troubleshooting"></a>Sorun giderme

Bu bölümde, yaygın sorunlar ve sorun giderme adımları ele alınmaktadır.

### <a name="unauthorized-access"></a>Yetkisiz erişim

Kubernetes kaynaklarına erişmek için AKS kümesine, Kubernetes API 'sine ve Kubernetes nesnelerine erişiminizin olması gerekir. AKS kümesine erişmek için uygun izinlere sahip bir Küme Yöneticisi veya Kullanıcı olduğunuzdan emin olun. Küme güvenliği hakkında daha fazla bilgi için bkz. [AKS Için erişim ve kimlik seçenekleri][concepts-identity].

### <a name="enable-resource-view"></a>Kaynak görünümünü etkinleştir

Mevcut kümeler için Kubernetes kaynak görünümünü etkinleştirmeniz gerekebilir. Kaynak görünümünü etkinleştirmek için, kümenizin portalındaki komut istemlerini izleyin.

:::image type="content" source="media/kubernetes-portal/enable-resource-view.png" alt-text="Kubernetes kaynak görünümünü etkinleştirmek için Azure portal ileti." lightbox="media/kubernetes-portal/enable-resource-view.png":::

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, AKS kümeniz için Kubernetes kaynaklarına nasıl erişebileceğiniz gösterilmektedir. Küme kaynaklarını ve Kubernetes kaynak görüntüleyicisinden erişilen YAML dosyalarını daha ayrıntılı olarak anlamak için [dağıtımlar ve YAML bildirimleri][deployments] bölümüne bakın.

<!-- LINKS - internal -->
[kubernetes-dashboard]: kubernetes-dashboard.md
[concepts-identity]: concepts-identity.md
[portal-quickstart]: kubernetes-walkthrough-portal.md#run-the-application
[deployments]: concepts-clusters-workloads.md#deployments-and-yaml-manifests
[aks-managed-aad]: managed-aad.md
[cli-aad-upgrade]: managed-aad.md#upgrading-to-aks-managed-azure-ad-integration
[enable-monitor]: ../azure-monitor/insights/container-insights-enable-existing-clusters.md
