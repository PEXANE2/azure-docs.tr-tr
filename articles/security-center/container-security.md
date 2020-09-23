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
ms.openlocfilehash: bd4487d3d0664699a32eec2ab47297839eaf8a8a
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90894915"
---
# <a name="container-security-in-security-center"></a>Güvenlik Merkezi 'nde kapsayıcı güvenliği

Azure Güvenlik Merkezi, kapsayıcılarınızın güvenliğini sağlamaya yönelik Azure Native çözümüdür.

Güvenlik Merkezi aşağıdaki kapsayıcı kaynak türlerini koruyabilir:

| Kaynak türü | Güvenlik Merkezi tarafından sunulan korumalar |
|:--------------------:|-----------|
| ![Kubernetes hizmeti](./media/security-center-virtual-machine-recommendations/icon-kubernetes-service-rec.png)<br>**Azure Kubernetes hizmeti (AKS) kümeleri** | -AKS kümelerinizin yapılandırmalarını, yanlış yapılandırma ve keşfedilen sorunları çözmenize yardımcı olacak yönergeler sağlamak için sürekli değerlendirme.<br>[Güvenlik önerileri aracılığıyla ortam sağlamlaştırma hakkında daha fazla bilgi edinin](#environment-hardening).<br><br>-AKS kümeleri ve Linux düğümleri için tehdit koruması. Şüpheli etkinliklerin uyarıları,  [Kubernetes için isteğe bağlı Azure Defender](defender-for-kubernetes-introduction.md)tarafından sağlanır.<br>[AKS düğümleri ve kümeleri için çalışma zamanı koruması hakkında daha fazla bilgi edinin](#run-time-protection-for-aks-nodes-and-clusters).|
| ![Kapsayıcı Konağı](./media/security-center-virtual-machine-recommendations/icon-container-host-rec.png)<br>**Kapsayıcı Konakları**<br>(Docker çalıştıran VM 'Ler) | -Docker yapılandırmalarının sürekli değerlendirmesi, yanlış yapılandırma ve  [sunucular için isteğe bağlı Azure Defender](defender-for-servers-introduction.md)ile keşfedilen sorunları çözmenize yardımcı olacak yönergeler sağlar.<br>[Güvenlik önerileri aracılığıyla ortam sağlamlaştırma hakkında daha fazla bilgi edinin](#environment-hardening).|
| ![Kapsayıcı kayıt defteri](./media/security-center-virtual-machine-recommendations/icon-container-registry-rec.png)<br>**Azure Container Registry (ACR) kayıt defterleri** | -Azure Resource Manager tabanlı ACR kayıt defterlerine ait görüntüler için, [kapsayıcı kayıt defterlerine yönelik isteğe bağlı Azure Defender](defender-for-container-registries-introduction.md)ile ilgili güvenlik açığı değerlendirmesi ve yönetim araçları.<br>[Güvenlik açıkları için kapsayıcı görüntülerinizi tarama hakkında daha fazla bilgi edinin](#vulnerability-management---scanning-container-images). |
|||

Bu makalede, kapsayıcılarınızın ve uygulamalarının güvenliğini artırmak, izlemek ve korumak için kapsayıcı kayıt defterleri, SEI 'ler ve Kubernetes için isteğe bağlı Azure Defender planlarıyla birlikte Güvenlik Merkezi 'ni nasıl kullanabileceğiniz açıklanır.

Güvenlik Merkezi 'nin kapsayıcı güvenliğinin bu temel yönlerini nasıl yardımcı olduğunu öğreneceksiniz:

- [Güvenlik açığı yönetimi-kapsayıcı görüntülerini tarama](#vulnerability-management---scanning-container-images)
- [Ortam sağlamlaştırma](#environment-hardening)
- [AKS düğümleri ve kümeleri için çalışma zamanı koruması](#run-time-protection-for-aks-nodes-and-clusters)

Aşağıdaki ekran görüntüsünde, varlık Envanteri sayfası ve Güvenlik Merkezi tarafından korunan çeşitli kapsayıcı kaynak türleri gösterilmektedir.

:::image type="content" source="./media/container-security/container-security-tab.png" alt-text="Güvenlik Merkezi 'nin varlık envanteri sayfasında kapsayıcı ile ilgili kaynaklar" lightbox="./media/container-security/container-security-tab.png":::

## <a name="vulnerability-management---scanning-container-images"></a>Güvenlik açığı yönetimi-kapsayıcı görüntülerini tarama

Azure Resource Manager tabanlı Azure Container kayıt defterlerinden görüntüleri izlemek için, [kapsayıcı kayıt defterleri Için Azure Defender](defender-for-container-registries-introduction.md)'ı etkinleştirin. Güvenlik Merkezi, son 30 gün içinde çekilen, Kayıt defterinize gönderilen veya içeri aktardığınız tüm görüntüleri tarar. Tümleşik tarayıcı, sektörde önde gelen güvenlik açığı tarama satıcısı, Qualys tarafından sağlanır.

Sorunlar bulunduğunda, Qualys veya Güvenlik Merkezi tarafından – [Azure Defender panosunda](azure-defender-dashboard.md)bildirim alırsınız. Güvenlik Merkezi her güvenlik açığı için, eyleme dönüştürülebilir öneriler, bir önem sınıflandırması ve sorunun nasıl düzeltileceği ile ilgili rehberlik sağlar. Güvenlik Merkezi 'nin kapsayıcılar için önerilerin ayrıntıları için bkz. [önerilerin başvuru listesi](recommendations-reference.md#recs-containers).

Güvenlik Merkezi, tarayıcıdan bulguları filtreler ve sınıflandırır. Bir görüntü sağlıklı olduğunda, güvenlik merkezi bunu bu şekilde işaretler. Güvenlik Merkezi yalnızca çözümleme sorunları olan görüntüler için güvenlik önerileri oluşturur. Güvenlik Merkezi, yalnızca sorun olduğunda bilgilendirerek istenmeyen bilgi uyarıları için potansiyelini azaltır.

## <a name="environment-hardening"></a>Ortam sağlamlaştırma

### <a name="continuous-monitoring-of-your-docker-configuration"></a>Docker yapılandırmanızın sürekli izlenmesi

Azure Güvenlik Merkezi, IaaS Linux VM 'lerinde barındırılan yönetilmeyen kapsayıcıları veya Docker Kapsayıcıları çalıştıran diğer Linux makinelerini tanımlar. Güvenlik Merkezi, bu kapsayıcıların yapılandırmalarının sürekli değerlendirir. Daha sonra bunları [Internet güvenliği (CIS) Docker kıyaslaması Için merkezi](https://www.cisecurity.org/benchmark/docker/)ile karşılaştırır.

Güvenlik Merkezi, CIS Docker kıyaslaması 'nın kural kümesinin tamamını içerir ve kapsayıcılarınız denetimlerden herhangi birini karşılamadığı takdirde sizi uyarır. Güvenlik Merkezi, yanlış yapılandırma bulduğunda güvenlik önerileri oluşturur. Önerileri görüntülemek ve sorunları düzeltmek için **öneriler sayfasını** kullanın. Ayrıca, Docker ile dağıtılan tüm sanal makineleri görüntüleyen **kapsayıcılar** sekmesinde öneriler görürsünüz. CIS kıyaslama denetimleri, AKS tarafından yönetilen örneklerde veya Databricks tarafından yönetilen VM 'lerde çalıştırılmayın.

Bu özellik için görünebilen ilgili güvenlik merkezi önerilerinin ayrıntıları için, öneriler başvuru tablosunun [kapsayıcı bölümüne](recommendations-reference.md#recs-containers) bakın.

Bir VM 'nin güvenlik sorunlarını araştırırken, güvenlik merkezi makinedeki kapsayıcılar hakkında ek bilgiler sağlar. Bu tür bilgiler, Docker sürümünü ve konakta çalışan görüntü sayısını içerir. 

IaaS Linux VM 'lerinde barındırılan yönetilmeyen kapsayıcıları izlemek için, [sunucular için isteğe bağlı Azure Defender 'ı](defender-for-servers-introduction.md)etkinleştirin.


### <a name="continuous-monitoring-of-your-kubernetes-clusters"></a>Kubernetes kümelerinizi sürekli izleme
Güvenlik Merkezi Azure Kubernetes hizmeti (AKS) ile birlikte çalışarak Kapsayıcılı uygulamalar geliştirmek, dağıtmak ve yönetmek için Microsoft 'un yönetilen kapsayıcı düzenleme hizmetidir.

AKS, kümelerinizin güvenlik duruşuna güvenlik denetimleri ve görünürlük sağlar. Güvenlik Merkezi şu özellikleri kullanır:
* AKS kümelerinizin yapılandırmasını sürekli izleyin
* Endüstri standartlarına göre hizalanmış güvenlik önerileri oluşturun

Bu özellik için görünebilen ilgili güvenlik merkezi önerilerinin ayrıntıları için, öneriler başvuru tablosunun [kapsayıcı bölümüne](recommendations-reference.md#recs-containers) bakın.

###  <a name="workload-protection-best-practices-using-kubernetes-admission-control"></a>İş yükü koruma en iyi-Kubernetes giriş denetimi kullanan uygulamalar

**Kubernetes Için Azure ilke eklentisi '** ni, Kubernetes kapsayıcılarınızın iş yüklerini korumaya yönelik öneriler grubunu almak için yükler.

[Bu Azure Ilkesinde Kubernetes sayfasında](../governance/policy/concepts/policy-for-kubernetes.md)açıklandığı gibi, eklenti açık kaynak [Gatekeeper v3](https://github.com/open-policy-agent/gatekeeper)   giriş denetleyicisi Web kancasını [Açık ilke aracısına](https://www.openpolicyagent.org/)genişletir. Kubernetes giriş denetleyicileri, kümelerinizin nasıl kullanıldığını zorlayan eklentilerdir. Eklenti, Kubernetes giriş denetimine bir Web kancası olarak kaydedilir ve kümelerinizde yerleşik olarak öngörülebilir ve korumalar üzerinde merkezi ve tutarlı bir şekilde uygulanmasını sağlar. 

Eklentiyi AKS kümenize yüklediğinizde, Kubernetes API sunucusuna gönderilen her istek, kümede kalıcı hale gelmeden önce önceden tanımlanmış en iyi uygulamalar kümesine göre izlenir. Daha sonra, en iyi uygulamaları zorlamak ve gelecekteki iş yükleri için bunları **zorunlu kılmak** üzere yapılandırabilirsiniz. 

Örneğin, ayrıcalıklı kapsayıcıların oluşturulmaması ve ileride yapılacak istekleri engellenemeyeceksiniz.

[Kubernetes iş yüklerinizi koruma](kubernetes-workload-protections.md)hakkında daha fazla bilgi edinin.


## <a name="run-time-protection-for-aks-nodes-and-clusters"></a>AKS düğümleri ve kümeleri için çalışma zamanı koruması

[!INCLUDE [AKS in ASC threat protection](../../includes/security-center-azure-kubernetes-threat-protection.md)]



## <a name="next-steps"></a>Sonraki adımlar

Bu genel bakışta, Azure Güvenlik Merkezi 'nde kapsayıcı güvenliğinin temel öğeleri hakkında bilgi edindiniz. İlgili malzemeler için bkz:

- [Kubernetes için Azure Defender 'a giriş](defender-for-kubernetes-introduction.md)
- [Kapsayıcı kayıt defterleri için Azure Defender 'a giriş](defender-for-container-registries-introduction.md)