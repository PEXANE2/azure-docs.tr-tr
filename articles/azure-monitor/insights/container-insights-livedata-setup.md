---
title: Kapsayıcılar için Azure Monitörü'ü Canlı Veriler (önizleme) ayarlama | Microsoft Dokümanlar
description: Bu makalede, kapsayıcı günlüklerinin (stdout/stderr) ve olayların gerçek zamanlı görünümünün, kapsayıcılar için Azure Monitor ile kubectl kullanmadan nasıl ayarlanış edilebildiğini açıklanmaktadır.
ms.topic: conceptual
ms.date: 02/14/2019
ms.openlocfilehash: f19071ca642cd229cbd7d49b4eab90c970672eee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275379"
---
# <a name="how-to-set-up-the-live-data-preview-feature"></a>Canlı Veri (önizleme) özelliği nasıl ayarlanır?

Azure Kubernetes Service (AKS) kümelerinden kapsayıcılar için Azure Monitor ile Canlı Veri (önizleme) görüntülemek için, Kubernetes verilerinize erişim izni vermek için kimlik doğrulamayı yapılandırmanız gerekir. Bu güvenlik yapılandırması, doğrudan Azure portalındaki Kubernetes API'si aracılığıyla verilerinize gerçek zamanlı erişim sağlar.

Bu özellik, günlüklere, olaylara ve ölçümlere erişimi denetlemek için aşağıdaki yöntemleri destekler:

- Kubernetes RBAC yetkilendirmesi olmadan AKS etkin
- AKS, Kubernetes RBAC yetkilendirmesi ile etkinleştirildi
    - AKS küme rol bağlama ** [clusterMonitoringUser](https://docs.microsoft.com/rest/api/aks/managedclusters/listclustermonitoringusercredentials?view=azurermps-5.2.0) ile yapılandırılmış**
- AKS, Azure Active Directory (AD) SAML tabanlı tek oturum açma özelliğine sahip

Bu yönergeler hem Kubernetes kümenize yönetimsel erişim gerektirir hem de kullanıcı kimlik doğrulaması için Azure Active Directory 'i (AD) kullanacak şekilde yapılandırıyorsanız, Azure AD'ye yönetimerişimi gerekir.  

Bu makalede, kümedeki Canlı Veri (önizleme) özelliğine erişimi denetlemek için kimlik doğrulamanın nasıl yapılandırılabildiğini açıklanmaktadır:

- Rol tabanlı erişim denetimi (RBAC) etkin AKS kümesi
- Azure Active Directory entegre AKS kümesi. 

>[!NOTE]
>[Özel kümeler](https://azure.microsoft.com/updates/aks-private-cluster/) olarak etkinleştirilen AKS kümeleri bu özellik ile desteklenmez. Bu özellik, tarayıcınızdan bir proxy sunucusu aracılığıyla Kubernetes API'ye doğrudan erişmenize dayanır. Bu proxy Kubernetes API engellemek için ağ güvenliği etkinleştirmek bu trafiği engeller. 

>[!NOTE]
>Bu özellik, Azure Çin'i de dahil olmak üzere tüm Azure bölgelerinde kullanılabilir. Şu anda Azure ABD Hükümeti'nde kullanılamıyor.

## <a name="authentication-model"></a>Kimlik doğrulaması modeli

Canlı Veri (önizleme) özellikleri, komut satırı aracıyla aynı `kubectl` olan Kubernetes API'sini kullanır. Kubernetes API uç noktaları, tarayıcınızın doğrulayamayacağı kendi imzalı bir sertifika kullanır. Bu özellik, aks hizmeti ile sertifikayı doğrulamak için bir iç proxy kullanır, trafik güvenilir olmasını sağlamak.

Azure portalı, bir Azure Etkin Dizin kümesi için oturum açma kimlik bilgilerinizi doğrulamanızı ve küme oluşturma sırasında (ve bu makalede yeniden yapılandırıldı) istemci kayıt kurulumuna yönlendirmenizi ister. Bu davranış, `kubectl`'. 

>[!NOTE]
>Kümenizin yetkilendirmesi Kubernetes tarafından yönetilir ve bu modelle yapılandırıldığı güvenlik modeli tarafından yönetilir. Bu özelliğe erişen kullanıcılar, çalıştırmaya `az aks get-credentials -n {your cluster name} -g {your resource group}`benzer şekilde Kubernetes yapılandırmasını *(kubeconfig)* indirmek için izin gerektirir. Bu yapılandırma dosyası, Azure RBAC etkinleştirilmiş ve RBAC izni etkin olmayan AKS kümeleri durumunda **Azure Kubernetes Hizmet Kümesi Kullanıcı Rolü**için yetkilendirme ve kimlik doğrulama belirteci içerir. AKS Azure Active Directory (AD) SAML tabanlı tek oturum açma özelliğiyle etkinleştirildiğinde Azure AD ve istemci kayıt ayrıntıları hakkında bilgi içerir.

>[!IMPORTANT]
>Bu özelliğin kullanıcıları, bu özelliği indirmek `kubeconfig` ve kullanmak için kümeye Azure [Kubernetes Küme Kullanıcı Rolü](../../azure/role-based-access-control/built-in-roles.md#azure-kubernetes-service-cluster-user-role permissions) gerektirir. Kullanıcılar bu özelliği kullanmak için kümeye katkıda bulunanların erişmesi **gerekmez.** 

## <a name="using-clustermonitoringuser-with-rbac-enabled-clusters"></a>RBAC özellikli kümelere sahip clusterMonitoringUser'ı kullanma

Kubernetes kullanıcı rol bağlama **kümeSIRBac** yetkilendirmesini [etkinleştirdikten](#configure-kubernetes-rbac-authorization) sonra Canlı Veri (önizleme) özelliğine kullanıcı erişimine izin vermek için ek yapılandırma değişiklikleri uygulama gereksinimini ortadan kaldırmak için AKS **clusterMonitoringUser**adı verilen yeni bir Kubernetes küme rol bağlama ekledi. Bu küme rol bağlama, Kubernetes API'sine ve Canlı Veri (önizleme) özelliğini kullanmak için uç noktalara erişmek için gerekli tüm izinlere sahip.

Bu yeni kullanıcıyla Canlı Veri (önizleme) özelliğini kullanabilmek için AKS küme kaynağındaki [Katılımcı](../../role-based-access-control/built-in-roles.md#contributor) rolünün bir üyesi olmanız gerekir. Kapsayıcılar için Azure Monitör, etkinleştirildiğinde, varsayılan olarak bu kullanıcı kullanılarak kimlik doğrulaması yapacak şekilde yapılandırılır. ClusterMonitoringKullanıcı rol bağlama bir küme üzerinde yoksa, **clusterUser** yerine kimlik doğrulama için kullanılır.

AKS Ocak 2020'de bu yeni rol bağlama yayımladı, bu yüzden Ocak 2020'den önce oluşturulan kümeler yok. Ocak 2020'den önce oluşturulmuş bir kümeniz varsa, yeni **clusterMonitoringUser** küme üzerinde bir PUT işlemi gerçekleştirerek veya küme üzerinde başka bir işlem gerçekleştirerek varolan kümeye eklenebilir, küme sürümünü güncelleştirme küçülmü gibi küme üzerinde bir PUT işlemi gerçekleştirir.

## <a name="kubernetes-cluster-without-rbac-enabled"></a>RBAC etkin olmadan Kubernetes küme

Kubernetes RBAC yetkilendirmesi ile yapılandırılmamış veya Azure AD ile tümleştirilmiş bir Kubernetes kümeniz varsa, aşağıdaki adımları izlemeniz gerekmez. Bunun nedeni, RBAC olmayan bir yapılandırmada varsayılan olarak yönetim izinlerine sahip olmasıdır.

## <a name="configure-kubernetes-rbac-authorization"></a>Kubernetes RBAC yetkilendirmesi yapılandırın

Kubernetes RBAC yetkilendirmesini etkinleştirdiğinizde, iki kullanıcı kullanılır: **clusterUser** ve **clusterAdmin** Kubernetes API'sine erişmek için. Bu, yönetim `az aks get-credentials -n {cluster_name} -g {rg_name}` seçeneği olmadan çalışmaya benzer. Bu, **clusterUser'a** Kubernetes API'deki bitiş noktalarına erişim izni verilmesi gerektiği anlamına gelir.

Aşağıdaki örnek adımlar, bu yaml yapılandırma şablonundan küme rol bağlamayapılandırmasını nasıl yapılandırılabildiğini gösterir.

1. Kopyalayın ve yaml dosyasını yapıştırın ve LogReaderRBAC.yaml olarak kaydedin.  

    ```
    apiVersion: rbac.authorization.k8s.io/v1 
    kind: ClusterRole 
    metadata: 
       name: containerHealth-log-reader 
    rules: 
        - apiGroups: ["", "metrics.k8s.io", "extensions", "apps"] 
          resources: 
             - "pods/log" 
             - "events" 
             - "nodes" 
             - "pods" 
             - "deployments" 
             - "replicasets" 
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

2. Yapılandırmanızı güncelleştirmek için aşağıdaki `kubectl apply -f LogReaderRBAC.yaml`komutu çalıştırın: .

>[!NOTE] 
> Dosyanın `LogReaderRBAC.yaml` önceki bir sürümünü kümenize uyguladıysanız, yukarıdaki adım 1'de gösterilen yeni kodu kopyalayıp yapıştırarak dosyayı güncelleştirin ve ardından kümenize uygulamak için adım 2'de gösterilen komutu çalıştırın.

## <a name="configure-ad-integrated-authentication"></a>AD ile tümleşik kimlik doğrulamayı yapılandırma 

Kullanıcı kimlik doğrulaması için Azure Active Directory (AD) kullanacak şekilde yapılandırılan bir AKS kümesi, bu özelliğe erişen kişinin giriş kimlik bilgilerini kullanır. Bu yapılandırmada, Azure AD kimlik doğrulama belirtecinizi kullanarak bir AKS kümesinde oturum açabilirsiniz.

Azure AD istemci kaydı, Azure portalının yetkilendirme sayfalarını güvenilir bir yönlendirme URL'si olarak yeniden yönlendirmesine olanak sağlayacak şekilde yeniden yapılandırılmalıdır. Azure AD'deki kullanıcılara **ClusterRoles** ve **ClusterRoleBindings**aracılığıyla aynı Kubernetes API uç noktalarına doğrudan erişim hakkı verilir. 

Kubernetes'teki gelişmiş güvenlik kurulumu hakkında daha fazla bilgi için [Kubernetes belgelerini](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)inceleyin. 

>[!NOTE]
>RBAC özellikli yeni bir küme oluşturuyorsanız, [Azure Etkin Dizini Azure Kubernetes Hizmeti ile tümleştir'e](../../aks/azure-ad-integration.md) bakın ve Azure AD kimlik doğrulamasını yapılandırma adımlarını izleyin. İstemci uygulamasını oluşturma adımları sırasında, bu bölümdeki bir not, aşağıdaki Adım 3'te belirtilenlerle eşleşen kapsayıcılar için Azure Monitor için oluşturmanız gereken iki yeniden yönlendirme URL'sini vurgular.

### <a name="client-registration-reconfiguration"></a>İstemci kaydı yeniden yapılandırma

1. Azure Portalı'ndaki **Azure Active Directory > Uygulama kayıtları** altında Azure AD'deki Kubernetes kümeniz için istemci kaydını bulun.

2. Sol bölmeden **Kimlik Doğrulama'yı** seçin. 

3. **Web** uygulama türleri olarak bu listeye iki yeniden yönlendirme URL'si ekleyin. İlk temel URL değeri, `https://afd.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` ikinci temel URL `https://monitoring.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html`değeri ise.

    >[!NOTE]
    >Bu özelliği Azure China'da kullanıyorsanız, ilk temel `https://afd.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` URL değeri ve ikinci `https://monitoring.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html`temel URL değeri olmalıdır. 
    
4. **Dolaylı hibe**altında yeniden yönlendirme URL'lerini kaydettikten **sonra, Access belirteçleri** ve **kimlik belirteçleri** seçeneklerini seçin ve değişikliklerinizi kaydedin.

>[!NOTE]
>Kimlik doğrulamayı tek oturum açma için Azure Etkin Dizini ile yapılandırma yalnızca yeni bir AKS kümesinin ilk dağıtımı sırasında gerçekleştirilebilir. Zaten dağıtılan bir AKS kümesi için tek oturum üzerinde yapılandıramazsınız.
  
>[!IMPORTANT]
>Güncelleştirilmiş URI'yi kullanarak kullanıcı kimlik doğrulaması için Azure AD'yi yeniden yapılandırıldıysanız, güncelleştirilmiş kimlik doğrulama belirteci indirilir ve uygulandığından emin olmak için tarayıcınızın önbelleğini temizleyin.

## <a name="grant-permission"></a>Hibe izni

Her Azure REKLAM hesabına Canlı Veri (önizleme) özelliğine erişmek için Kubernetes'teki ilgili API'lere izin verilmesi gerekir. Azure Etkin Dizin hesabı verme [adımları, Kubernetes RBAC kimlik doğrulama](#configure-kubernetes-rbac-authorization) bölümünde açıklanan adımlara benzer. Kümenize yaml yapılandırma şablonunu uygulamadan önce **ClusterRoleBinding** altında **clusterUser'ı** istediğiniz kullanıcıyla değiştirin. 

>[!IMPORTANT]
>RBAC bağlayıcısı için hibe kullanıcı aynı Azure AD kiracı ise, userPrincipalName dayalı izinler atayın. Kullanıcı farklı bir Azure AD kiracıysa, objectid özelliğini sorgula ve kullan.

AKS küme **clusterRoleBinding**yapılandırma ek yardım için, [RBAC bağlayıcı oluştur](../../aks/azure-ad-integration-cli.md#create-rbac-binding)'a bakın.

## <a name="next-steps"></a>Sonraki adımlar

Artık kurulum kimlik doğrulamanız olduğuna göre, [ölçümleri,](container-insights-livedata-metrics.md) [Dağıtımları](container-insights-livedata-deployments.md)ve [olayları ve günlükleri](container-insights-livedata-overview.md) kümenizden gerçek zamanlı olarak görüntüleyebilirsiniz.
