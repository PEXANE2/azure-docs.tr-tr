---
title: Kapsayıcılar için Azure Izleyici günlüklerini gerçek zamanlı olarak görüntüleyin | Microsoft Docs
description: Bu makalede, kapsayıcılar için Azure Izleyici ile kubectl kullanmadan kapsayıcı günlüklerinin (stdout/stderr) ve olayların gerçek zamanlı görünümü açıklanır.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 07/12/2019
ms.openlocfilehash: 25cfe10ec192f874d050bca22ce1b85c2d1afbb4
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72554094"
---
# <a name="how-to-view-logs-and-events-in-real-time-preview"></a>Günlükleri ve olayları gerçek zamanlı görüntüleme (Önizleme)
Kapsayıcılar için Azure Izleyici, şu anda önizleme aşamasında olan ve kubectl komutlarını çalıştırmaya gerek kalmadan Azure Kubernetes Service (AKS) kapsayıcı günlüklerine (stdout/stderr) ve olaylara canlı bir görünüm sağlayan bir özelliği içerir. İki seçenekten birini seçtiğinizde **düğümler**, **denetleyiciler**ve **kapsayıcılar** görünümündeki performans verileri tablosunun altında yeni bir bölme belirir. Gerçek zamanlı sorun giderme sorunları konusunda daha fazla yardım almak için kapsayıcı altyapısı tarafından oluşturulan canlı günlüğe kaydetme ve olayları gösterir.

>[!NOTE]
>Bu özellik, Azure Çin dahil olmak üzere tüm Azure bölgelerinde kullanılabilir. Azure ABD kamu 'da Şu anda kullanılamıyor.

>[!NOTE]
>Bu özelliğin çalışması için **Azure Kubernetes hizmet kümesi Kullanıcı rolü** , küme kaynağına erişim gerektirir. [Azure Kubernetes kümesi Kullanıcı rolü hakkında daha fazla bilgi edinin](https://docs.microsoft.com/en-us/azure/aks/control-kubeconfig-access#available-cluster-roles-permissions).

Canlı Günlükler günlüklere erişimi denetlemek için üç farklı yöntemi destekler:

1. Kubernetes RBAC yetkilendirmesi etkin olmayan AKS
2. Kubernetes RBAC yetkilendirmesi ile AKS etkin
3. Azure Active Directory (AD) SAML tabanlı çoklu oturum açma ile AKS etkin

## <a name="kubernetes-cluster-without-rbac-enabled"></a>RBAC etkin olmayan Kubernetes kümesi
 
Kubernetes RBAC yetkilendirmesi ile yapılandırılmayan veya Azure AD çoklu oturum açma ile tümleştirilmiş bir Kubernetes kümeniz varsa, bu adımları izlemeniz gerekmez. Kubernetes yetkilendirmesi kuin-api ' yi kullandığından salt okuma izinleri gereklidir.

## <a name="kubernetes-rbac-authorization"></a>Kubernetes RBAC yetkilendirmesi
Kubernetes RBAC yetkilendirmesini etkinleştirdiyseniz, küme rolü bağlamayı uygulamanız gerekir. Aşağıdaki örnek adımlarda, bu YAML yapılandırma şablonundan küme rolü bağlamasının nasıl yapılandırılacağı gösterilmektedir. 

1. YAML dosyasını kopyalayıp yapıştırın ve Logreaderrtza. YAML olarak kaydedin.  

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

2. İlk kez yapılandırıyorsanız, şu komutu çalıştırarak küme kuralı bağlamasını uygularsınız: `kubectl create -f LogReaderRBAC.yaml`. Canlı Günlükler önizlemesi için daha önce etkin bir olay günlüğü sunmadan önce desteğini etkinleştirdiyseniz, yapılandırmanızı güncelleştirmek için şu komutu çalıştırın: `kubectl apply -f LogReaderRBAC.yaml`.

## <a name="configure-aks-with-azure-active-directory"></a>AKS 'leri Azure Active Directory ile yapılandırma

AKS, Kullanıcı kimlik doğrulaması için Azure Active Directory (AD) kullanacak şekilde yapılandırılabilir. İlk kez yapılandırıyorsanız, bkz. [Azure Kubernetes hizmeti ile Azure Active Directory tümleştirme](../../aks/azure-ad-integration.md). [İstemci uygulamasını](../../aks/azure-ad-integration.md#create-the-client-application)oluşturma adımları sırasında, aşağıdakileri belirtin:

-  **Yeniden yönlendirme URI 'si**: iki **Web** uygulaması türünün oluşturulması gerekir. İlk temel URL değeri `https://afd.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` olmalıdır ve ikinci taban URL değeri `https://monitoring.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` olmalıdır.
- Uygulamayı kaydettikten sonra, **genel bakış** sayfasından sol bölmedeki **kimlik doğrulaması** ' nı seçin. **Kimlik doğrulama** sayfasında, **Gelişmiş ayarlar** ' ın altında, **erişim belirteçleri** ve **Kimlik belirteçleri** ' ni örtülü olarak verin ve ardından değişikliklerinizi kaydedin.

>[!NOTE]
>Bu özelliği Azure Çin bölgesinde kullanıyorsanız, ilk temel URL değeri `https://afd.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` olmalıdır ve ikinci taban URL değeri `https://monitoring.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` olmalıdır.

>[!NOTE]
>Tek oturum açma için Azure Active Directory kimlik doğrulamasını yapılandırma, yalnızca yeni bir AKS kümesinin ilk dağıtımı sırasında gerçekleştirilebilir. Zaten dağıtılmış bir AKS kümesi için çoklu oturum açma yapılandıramazsınız.
  
>[!IMPORTANT]
>Güncelleştirilmiş URI 'yi kullanarak Kullanıcı kimlik doğrulaması için Azure AD 'yi yeniden yapılandırdıysanız, güncelleştirilmiş kimlik doğrulama belirtecinin indirilip uygulandığından emin olmak için tarayıcınızın önbelleğini temizleyin.   

## <a name="view-live-logs-and-events"></a>Canlı günlükleri ve olayları görüntüleme

Gerçek zamanlı günlük olaylarını **düğümler**, **denetleyiciler**ve **kapsayıcılar** görünümünden kapsayıcı altyapısı tarafından oluşturulan şekilde görüntüleyebilirsiniz. Özellikler bölmesinde, **canlı verileri görüntüle (Önizleme)** seçeneğini belirleyin ve günlük ve olayları sürekli bir akışta görüntüleyebileceğiniz performans verileri tablosunun altına bir bölme sunulur.

![Düğüm özellikleri bölmesi canlı Günlükler seçeneğini görüntüle](./media/container-insights-live-logs/node-properties-live-logs-01.png)  

Günlük ve olay iletileri, görünümde seçilen kaynak türüne göre sınırlandırılır.

| Görüntüle | Kaynak türü | Günlük veya olay | Sunulan veriler |
|------|---------------|--------------|----------------|
| Düğümler | Düğüm | Olay | Düğüm seçildiğinde olaylar filtrelenmez ve küme genelinde Kubernetes olaylarını gösterir. Bölme başlığı, kümenin adını gösterir. |
| Düğümler | Ayak | Olay | Pod seçildiğinde olaylar ad alanına filtrelenir. Bölme başlığı, Pod 'un ad alanını gösterir. | 
| Örleri | Ayak | Olay | Pod seçildiğinde olaylar ad alanına filtrelenir. Bölme başlığı, Pod 'un ad alanını gösterir. |
| Örleri | Kumandasını | Olay | Bir denetleyici seçildiğinde olaylar ad alanına filtrelenir. Bölme başlığı denetleyicinin ad alanını gösterir. |
| Düğümler/denetleyiciler/kapsayıcılar | Kapsayıcı | Günlükler | Bölme başlığı, kapsayıcının gruplandırıldığı Pod 'ın adını gösterir. |

AKS kümesi AAD kullanılarak SSO ile yapılandırıldıysa, bu tarayıcı oturumu sırasında ilk kullanımda kimlik doğrulaması yapmanız istenir. Hesabınızı seçin ve Azure ile kimlik doğrulamayı doldurun.  

Kimlik doğrulaması başarılı olduktan sonra, canlı günlük bölmesi orta bölmenin alt bölümünde görünür. Getirme durumu göstergesi, bölmenin en sağında yer alan yeşil bir onay işareti gösteriyorsa verileri alabileceği anlamına gelir.
    
  ![Canlı Günlükler bölmesi veri aldı](./media/container-insights-live-logs/live-logs-pane-01.png)  

Arama çubuğunda, anahtar sözcüğe göre filtreleyerek günlük veya olayda bu metni vurgulayabilirsiniz ve en sağdaki arama çubuğunda, filtreden kaç sonucun eşleştiğini gösterir.

  ![Canlı Günlükler bölmesi filtre örneği](./media/container-insights-live-logs/live-logs-pane-filter-example-01.png)

Olayları görüntülerken, arama çubuğunun sağında bulunan **filtre** hap 'i kullanarak sonuçları da sınırlayabilirsiniz. Seçtiğiniz kaynağa bağlı olarak, hap, arasından seçim yapmak için bir pod, Namespace veya Cluster listeler.  

Otomatik kaydırmayı askıya almak ve bölmenin davranışını denetlemek ve yeni verilerde el ile kaydırma yapmanıza izin vermek için **kaydırma** seçeneğine tıklayın. Otomatik kaydırmayı yeniden etkinleştirmek için, **kaydırma** seçeneğine yeniden tıklamanız yeterlidir. Ayrıca, **Duraklat** seçeneğine tıklayarak günlük veya olay verilerinin alınmasını duraklatabilirsiniz ve devam etmek Için hazırsanız **Yürüt**' e tıklamanız yeterlidir.  

![Canlı Günlükler bölmesi canlı görünümü Duraklat](./media/container-insights-live-logs/live-logs-pane-pause-01.png)

**Analytics 'teki**aşağı açılan liste görünümünden **kapsayıcı günlüklerini görüntüle** seçeneğini belirleyerek geçmiş kapsayıcı günlüklerini görmek için Azure izleyici günlüklerine gidebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Izleyici 'yi kullanmayı ve AKS kümenizin diğer yönlerini izlemeyi öğrenmeye devam etmek için bkz. [Azure Kubernetes hizmet durumunu görüntüleme](container-insights-analyze.md).

- Daha önceden tanımlanmış sorguları ve Uyarıları izlemek için değerlendirmek veya özelleştirmek üzere ön tanımlı sorguları ve örnekleri görmek için [günlük sorgusu örneklerini](container-insights-log-search.md#search-logs-to-analyze-data) görüntüleyin, kümelerinizi görselleştirmeyi veya çözümlemeyi inceleyin.
