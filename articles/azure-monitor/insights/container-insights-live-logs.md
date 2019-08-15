---
title: Kapsayıcılar için Azure İzleyici görünümü günlükleri gerçek zamanlı olarak | Microsoft Docs
description: Bu makalede, kapsayıcılar için Azure Izleyici ile kubectl kullanmadan kapsayıcı günlüklerinin (stdout/stderr) ve olayların gerçek zamanlı görünümü açıklanır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2019
ms.author: magoedte
ms.openlocfilehash: 2eab6fa75e4adbbde7bcf20f18301a1e516235c2
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035355"
---
# <a name="how-to-view-logs-and-events-in-real-time-preview"></a>Günlükleri ve olayları gerçek zamanlı görüntüleme (Önizleme)
Kapsayıcılar için Azure Izleyici, şu anda önizleme aşamasında olan ve kubectl komutlarını çalıştırmaya gerek kalmadan Azure Kubernetes Service (AKS) kapsayıcı günlüklerine (stdout/stderr) ve olaylara canlı bir görünüm sağlayan bir özelliği içerir. İki seçenekten birini seçtiğinizde **düğümler**, **denetleyiciler**ve **kapsayıcılar** görünümündeki performans verileri tablosunun altında yeni bir bölme belirir. Gerçek zamanlı sorun giderme sorunları konusunda daha fazla yardım almak için kapsayıcı altyapısı tarafından oluşturulan canlı günlüğe kaydetme ve olayları gösterir.

>[!NOTE]
>Bu özelliğin çalışması için küme kaynağına **katkıda bulunan** erişim gerekir.
>

Canlı Günlükler günlüklere erişimi denetlemek için üç farklı yöntemi destekler:

1. AKS etkin Kubernetes RBAC yetkilendirme olmadan
2. AKS ile Kubernetes RBAC yetkilendirme etkin
3. Azure Active Directory (AD) SAML tabanlı çoklu oturum açma ile AKS etkin

## <a name="kubernetes-cluster-without-rbac-enabled"></a>Kubernetes kümesi olmadan etkin RBAC
 
Kubernetes RBAC yetkilendirme ile yapılandırılmamışsa veya Azure AD çoklu oturum açma ile tümleşik bir Kubernetes kümesi varsa, bu adımları izlemeniz gerekmez. Kubernetes yetkilendirmesi kuin-api ' yi kullandığından salt okuma izinleri gereklidir.

## <a name="kubernetes-rbac-authorization"></a>Kubernetes RBAC yetkilendirme
Kubernetes RBAC yetkilendirme etkinleştirilirse, küme rolü bağlama uygulamak gerekir. Aşağıdaki örnek adımlarda bu yaml yapılandırma şablondan küme rolünü yapılandırmak nasıl ekleyebileceğiniz gösterilmektedir. 

1. Kopyalayın ve yapıştırın yaml dosyası ve LogReaderRBAC.yaml kaydedin.  

    ```
    apiVersion: rbac.authorization.k8s.io/v1 
    kind: ClusterRole 
    metadata: 
       name: containerHealth-log-reader 
    rules: 
       - apiGroups: [""] 
         resources: ["pods/log", "events"] 
         verbs: ["get", "list"]  
    --- 
    apiVersion: rbac.authorization.k8s.io/v1 
    kind: ClusterRoleBinding 
    metadata: 
       name: containerHealth-read-logs-global 
    roleRef: 
        kind: ClusterRole 
        name: containerHealth-log-reader 
        apiGroup: rbac.authorization.k8s.io 
    subjects: 
       - kind: User 
         name: clusterUser 
         apiGroup: rbac.authorization.k8s.io
    ```

2. İlk kez yapılandırıyorsanız, şu komutu çalıştırarak küme kuralı bağlamasını uygularsınız: `kubectl create -f LogReaderRBAC.yaml`. Canlı Günlükler önizlemesi desteğini daha önce etkinleştirdiyseniz, yapılandırmanızı güncelleştirmek için şu komutu çalıştırın: `kubectl apply -f LogReaderRBAC.yaml`.

## <a name="configure-aks-with-azure-active-directory"></a>AKS ile Azure Active Directory'yi yapılandırma

AKS, Azure Active Directory (AD) kullanıcı kimlik doğrulaması için kullanmak üzere yapılandırılabilir. İlk kez yapılandırıyorsanız, bkz. [Azure Kubernetes hizmeti ile Azure Active Directory tümleştirme](../../aks/azure-ad-integration.md). [İstemci uygulamasını](../../aks/azure-ad-integration.md#create-the-client-application)oluşturma adımları sırasında, aşağıdakileri belirtin:

-  **Yeniden yönlendirme URI 'si**: İki **Web** uygulaması türünün oluşturulması gerekir. İlk temel URL değeri `https://afd.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` olmalıdır ve ikinci taban URL değeri olmalıdır. `https://monitoring.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html`
- Uygulamayı kaydettikten sonra, **genel bakış** sayfasından sol bölmedeki **kimlik doğrulaması** ' nı seçin. **Kimlik doğrulama** sayfasında, **Gelişmiş ayarlar** ' ın altında, **erişim belirteçleri** ve **Kimlik belirteçleri** ' ni örtülü olarak verin ve ardından değişikliklerinizi kaydedin.

>[!NOTE]
>Tek oturum açma için Azure Active Directory kimlik doğrulamasını yapılandırma, yalnızca yeni bir AKS kümesinin ilk dağıtımı sırasında gerçekleştirilebilir. Çoklu oturum zaten dağıtılmış bir AKS kümesi için üzerinde yapılandıramazsınız.
  
>[!IMPORTANT]
>Güncelleştirilmiş URI 'yi kullanarak Kullanıcı kimlik doğrulaması için Azure AD 'yi yeniden yapılandırdıysanız, güncelleştirilmiş kimlik doğrulama belirtecinin indirilip uygulandığından emin olmak için tarayıcınızın önbelleğini temizleyin.   

## <a name="view-live-logs-and-events"></a>Canlı günlükleri ve olayları görüntüleme

Gerçek zamanlı günlük olaylarını **düğümler**, **denetleyiciler**ve **kapsayıcılar** görünümünden kapsayıcı altyapısı tarafından oluşturulan şekilde görüntüleyebilirsiniz. Özellikler bölmesinde, **canlı verileri görüntüle (Önizleme)** seçeneğini belirleyin ve günlük ve olayları sürekli bir akışta görüntüleyebileceğiniz performans verileri tablosunun altına bir bölme sunulur.

![Düğüm özellikleri bölmesi canlı Günlükler seçeneğini görüntüle](./media/container-insights-live-logs/node-properties-live-logs-01.png)  

Günlük ve olay iletileri, görünümde seçilen kaynak türüne göre sınırlandırılır.

| Görünüm | Kaynak türü | Günlük veya olay | Sunulan veriler |
|------|---------------|--------------|----------------|
| Düğümler | Düğüm | Olay | Düğüm seçildiğinde olaylar filtrelenmez ve küme genelinde Kubernetes olaylarını gösterir. Bölme başlığı, kümenin adını gösterir. |
| Düğümler | Pod | Olay | Pod seçildiğinde olaylar ad alanına filtrelenir. Bölme başlığı, Pod 'un ad alanını gösterir. | 
| Denetleyiciler | Pod | Olay | Pod seçildiğinde olaylar ad alanına filtrelenir. Bölme başlığı, Pod 'un ad alanını gösterir. |
| Denetleyiciler | Denetleyici | Olay | Bir denetleyici seçildiğinde olaylar ad alanına filtrelenir. Bölme başlığı denetleyicinin ad alanını gösterir. |
| Düğümler/denetleyiciler/kapsayıcılar | Kapsayıcı | Günlükler | Bölme başlığı, kapsayıcının gruplandırıldığı Pod 'ın adını gösterir. |

AKS kümesi ile SSO AAD kullanılarak yapılandırıldıysa, ilk kez kullanıldığında, tarayıcı oturumu sırasında kimlik doğrulaması istenir. Hesabınızı ve Azure ile kimlik doğrulamasını tamamlamak seçin.  

Başarıyla kimlik doğrulandıktan sonra dinamik günlük bölmesini Orta bölmenin alt kısmında görünür. Getirme durum göstergesi sağda bölmesinin olan yeşil onay işareti, gösteriliyorsa veri alabildiği anlamına gelir.
    
  ![Canlı günlükler alınan veriler](./media/container-insights-live-logs/live-logs-pane-01.png)  

Arama çubuğunda, anahtar sözcüğe göre filtreleyerek günlük veya olayda bu metni vurgulayabilirsiniz ve en sağdaki arama çubuğunda, filtreden kaç sonucun eşleştiğini gösterir.

  ![Canlı günlükler bölmesi filtresi örneği](./media/container-insights-live-logs/live-logs-pane-filter-example-01.png)

Olayları görüntülerken, arama çubuğunun sağında bulunan **filtre** hap 'i kullanarak sonuçları da sınırlayabilirsiniz. Seçtiğiniz kaynağa bağlı olarak, hap, arasından seçim yapmak için bir pod, Namespace veya Cluster listeler.  

Otomatik kaydırmayı askıya almak ve bölmenin davranışını denetlemek ve yeni verilerde el ile kaydırma yapmanıza izin vermek için **kaydırma** seçeneğine tıklayın. Otomatik kaydırmayı yeniden etkinleştirmek için, **kaydırma** seçeneğine yeniden tıklamanız yeterlidir. Ayrıca, **Duraklat** seçeneğine tıklayarak günlük veya olay verilerinin alınmasını duraklatabilirsiniz ve devam etmek Için hazırsanız **Yürüt**' e tıklamanız yeterlidir.  

![Canlı günlükler bölmesi duraklatma Canlı görünümü](./media/container-insights-live-logs/live-logs-pane-pause-01.png)

**Analytics 'teki**aşağı açılan liste görünümünden **kapsayıcı günlüklerini görüntüle** seçeneğini belirleyerek geçmiş kapsayıcı günlüklerini görmek için Azure izleyici günlüklerine gidebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Azure İzleyici ve diğer yönleri AKS kümenizi izlemek öğrenme devam etmek için bkz: [görünümü Azure Kubernetes hizmeti sistem durumu](container-insights-analyze.md).

- Daha önceden tanımlanmış sorguları ve Uyarıları izlemek için değerlendirmek veya özelleştirmek üzere ön tanımlı sorguları ve örnekleri görmek için [günlük sorgusu örneklerini](container-insights-log-search.md#search-logs-to-analyze-data) görüntüleyin, kümelerinizi görselleştirmeyi veya çözümlemeyi inceleyin.
