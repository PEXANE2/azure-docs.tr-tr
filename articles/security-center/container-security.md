---
title: Azure Güvenlik Merkezi 'nde kapsayıcı güvenliği | Microsoft Docs
description: Azure Güvenlik Merkezi 'nin kapsayıcı güvenlik özellikleri hakkında bilgi edinin.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2020
ms.author: memildin
ms.openlocfilehash: c01ed6dbbd6e1f7febfb99df11d2ee67cb1e5465
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85800621"
---
# <a name="container-security-in-security-center"></a>Güvenlik Merkezi 'nde kapsayıcı güvenliği

Azure Güvenlik Merkezi, kapsayıcılarınızın güvenliğini sağlamaya yönelik Azure Native çözümüdür. Güvenlik Merkezi aşağıdaki kapsayıcı kaynak türlerini koruyabilir:



|Kaynak |Name  |Ayrıntılar  |
|:---------:|---------|---------|
|![Kapsayıcı Konağı](./media/security-center-virtual-machine-recommendations/icon-container-host-rec.png)|Kapsayıcı Konakları (Docker çalıştıran sanal makineler)|Güvenlik Merkezi, Docker yapılandırmalarınızı tarar ve değerlendirilen başarısız kuralların listesini sunarak hatalı yapılandırmalar konusunda görünürlük sağlar. Güvenlik Merkezi, bu sorunları hızlı bir şekilde çözmenize ve zamandan tasarruf etmenize yardımcı olacak yönergeler sağlar. Güvenlik Merkezi, Docker yapılandırmalarını sürekli değerlendirir ve son durumlar hakkında bilgi verir.|
|![Kubernetes hizmeti](./media/security-center-virtual-machine-recommendations/icon-kubernetes-service-rec.png)|Azure Kubernetes hizmeti (AKS) kümeleri|Standart katman kullanıcıları için [Güvenlik Merkezi 'nin isteğe bağlı aks paketiyle](azure-kubernetes-service-integration.md) aks düğümleriniz, bulut trafiğiniz ve güvenlik denetimleriniz hakkında daha ayrıntılı bir görünürlük elde edin.|
|![Kapsayıcı kayıt defteri](./media/security-center-virtual-machine-recommendations/icon-container-registry-rec.png)|Azure Container Registry (ACR) kayıt defterleri|Standart katman kullanıcıları için [Güvenlik Merkezi 'nin isteğe bağlı ACR](azure-kubernetes-service-integration.md) grubu ile ARM tabanlı ACR kayıt defterlerinden görüntülerin güvenlik açıklarına ilişkin daha derin görünürlük elde edin.|
||||


Bu makalede, kapsayıcılarınızın ve uygulamalarının güvenliğini artırmak, izlemek ve korumak için bu paketleri nasıl kullanabileceğiniz açıklanır. Güvenlik Merkezi 'nin kapsayıcı güvenliğinin bu temel yönlerini nasıl yardımcı olduğunu öğreneceksiniz:

- [Güvenlik açığı yönetimi-kapsayıcı görüntülerini tarama](#vulnerability-management---scanning-container-images)
- [Ortam sağlamlaştırma-Docker yapılandırmanızın ve Kubernetes kümelerinin sürekli izlenmesi](#environment-hardening)
- [Çalışma zamanı koruması-gerçek zamanlı tehdit algılama](#run-time-protection---real-time-threat-detection)

[![Azure Güvenlik Merkezi 'nin kapsayıcı güvenlik sekmesi](media/container-security/container-security-tab.png)](media/container-security/container-security-tab.png#lightbox)

Bu özellikleri kullanma hakkında yönergeler için bkz. [kapsayıcılarınızın güvenliğini izleme](monitor-container-security.md).

## <a name="vulnerability-management---scanning-container-images"></a>Güvenlik açığı yönetimi-kapsayıcı görüntülerini tarama
ARM tabanlı Azure Container Registry izlemek için Güvenlik Merkezi 'nin standart katmanında olduğunuzdan emin olun (bkz. [fiyatlandırma](/azure/security-center/security-center-pricing)). Ardından, isteğe bağlı kapsayıcı kayıt defterleri paketini etkinleştirin. Yeni bir görüntü gönderildiğinde, Güvenlik Merkezi, sektör lideri güvenlik açığı tarama satıcısı, Qualys 'den bir tarayıcı kullanarak görüntüyü tarar.

Sorunlar bulunduğunda: Qualys veya Güvenlik Merkezi tarafından – Güvenlik Merkezi panosunda bildirim alırsınız. Güvenlik Merkezi her güvenlik açığı için, eyleme dönüştürülebilir öneriler, bir önem sınıflandırması ve sorunun nasıl düzeltileceği ile ilgili rehberlik sağlar. Güvenlik Merkezi 'nin kapsayıcılar için önerilerin ayrıntıları için bkz. [önerilerin başvuru listesi](recommendations-reference.md#recs-containers).

Güvenlik Merkezi, tarayıcıdan bulguları filtreler ve sınıflandırır. Bir görüntü sağlıklı olduğunda, güvenlik merkezi bunu bu şekilde işaretler. Güvenlik Merkezi yalnızca çözümleme sorunları olan görüntüler için güvenlik önerileri oluşturur. Güvenlik Merkezi, yalnızca sorun olduğunda bilgilendirerek istenmeyen bilgi uyarıları için potansiyelini azaltır.

## <a name="environment-hardening"></a>Ortam sağlamlaştırma

### <a name="continuous-monitoring-of-your-docker-configuration"></a>Docker yapılandırmanızın sürekli izlenmesi
Azure Güvenlik Merkezi, IaaS Linux VM 'lerinde barındırılan yönetilmeyen kapsayıcıları veya Docker Kapsayıcıları çalıştıran diğer Linux makinelerini tanımlar. Güvenlik Merkezi, bu kapsayıcıların yapılandırmalarının sürekli değerlendirir. Daha sonra bunları [Internet güvenliği (CIS) Docker kıyaslaması Için merkezi](https://www.cisecurity.org/benchmark/docker/)ile karşılaştırır.

Güvenlik Merkezi, CIS Docker kıyaslaması 'nın kural kümesinin tamamını içerir ve kapsayıcılarınız denetimlerden herhangi birini karşılamadığı takdirde sizi uyarır. Güvenlik Merkezi, yanlış yapılandırma bulduğunda güvenlik önerileri oluşturur. Önerileri görüntülemek ve sorunları düzeltmek için **öneriler sayfasını** kullanın. Ayrıca, Docker ile dağıtılan tüm sanal makineleri görüntüleyen **kapsayıcılar** sekmesinde öneriler görürsünüz. 

Bu özellik için görünebilen ilgili güvenlik merkezi önerilerinin ayrıntıları için, öneriler başvuru tablosunun [kapsayıcı bölümüne](recommendations-reference.md#recs-containers) bakın.

Bir VM 'nin güvenlik sorunlarını araştırırken, güvenlik merkezi makinedeki kapsayıcılar hakkında ek bilgiler sağlar. Bu tür bilgiler, Docker sürümünü ve konakta çalışan görüntü sayısını içerir. 

>[!NOTE]
> Bu CIS kıyaslama denetimleri, AKS tarafından yönetilen örneklerde veya Databricks tarafından yönetilen VM 'lerde çalışmaz.

### <a name="continuous-monitoring-of-your-kubernetes-clusters"></a>Kubernetes kümelerinizi sürekli izleme
Güvenlik Merkezi Azure Kubernetes hizmeti (AKS) ile birlikte çalışarak Kapsayıcılı uygulamalar geliştirmek, dağıtmak ve yönetmek için Microsoft 'un yönetilen kapsayıcı düzenleme hizmetidir.

AKS, kümelerinizin güvenlik duruşuna güvenlik denetimleri ve görünürlük sağlar. Güvenlik Merkezi şu özellikleri kullanır:
* AKS kümelerinizin yapılandırmasını sürekli izleyin
* Endüstri standartlarına göre hizalanmış güvenlik önerileri oluşturun

Bu özellik için görünebilen ilgili güvenlik merkezi önerilerinin ayrıntıları için, öneriler başvuru tablosunun [kapsayıcı bölümüne](recommendations-reference.md#recs-containers) bakın.

## <a name="run-time-protection---real-time-threat-detection"></a>Çalışma zamanı koruması-gerçek zamanlı tehdit algılama

[!INCLUDE [AKS in ASC threat protection](../../includes/security-center-azure-kubernetes-threat-protection.md)]




## <a name="next-steps"></a>Sonraki adımlar

Bu genel bakışta, Azure Güvenlik Merkezi 'nde kapsayıcı güvenliğinin temel öğeleri hakkında bilgi edindiniz. [Kapsayıcılarınızın güvenliğini izlemeye](monitor-container-security.md)devam edin.
> [!div class="nextstepaction"]
> [Kapsayıcılarınızın güvenliğini izleme](monitor-container-security.md)