---
title: Azure Güvenlik Merkezi ve Azure Kubernetes hizmeti
description: Azure Güvenlik Merkezi 'nin Azure Kubernetes hizmetleriyle tümleştirmesi hakkında bilgi edinin
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/30/2020
ms.author: memildin
ms.openlocfilehash: 94d1bccc9a7f45d24d8c5b92aecba54d9f7f630a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85800195"
---
# <a name="azure-kubernetes-services-integration-with-security-center"></a>Güvenlik Merkezi ile Azure Kubernetes Hizmetleri Tümleştirmesi

Azure Kubernetes Service (AKS), Kapsayıcılı uygulamalar geliştirmeye, dağıtmaya ve yönetmeye yönelik Microsoft tarafından yönetilen bir hizmettir. 

Azure Güvenlik Merkezi 'nin standart katmanı kullanıyorsanız aks, bulut trafiğinizi ve güvenlik denetimlerinizi daha derin bir görünürlük elde etmek için AKS paketini (bkz. [fiyatlandırma](security-center-pricing.md)) ekleyebilirsiniz.

Bir arada, güvenlik merkezi ve AKS, en iyi bulut Yerel Kubernetes güvenlik teklifini oluşturur.

## <a name="what-are-the-components-of-security-centers-kubernetes-protection"></a>Güvenlik Merkezi 'nin Kubernetes korumasının bileşenleri nelerdir?

Kubernetes için Güvenlik Merkezi 'nin korumaları iki öğe birleşimi tarafından sağlanır:

- **Azure Güvenlik Merkezi 'nin sanal makineler için tehdit koruması** -Güvenlik Merkezi 'Nin diğer VM 'lerde kullandığı aynı Log Analytics aracısını kullanarak, güvenlik merkezi aks düğümleriniz üzerinde oluşan güvenlik sorunlarını gösterebilir. Aracı Ayrıca kapsayıcıya özgü analiz için de izler.

- **Azure Güvenlik Merkezi 'nin isteğe bağlı Kubernetes paketi** -Kubernetes demeti, aks hizmeti aracılığıyla Kubernetes alt sisteminden Günlükler ve bilgiler alır. Bu Günlükler Azure 'da AKS hizmeti aracılığıyla zaten kullanılabilir. Güvenlik Merkezi 'nin Kubernetes paketini etkinleştirdiğinizde, günlüklere güvenlik merkezi erişimi verirsiniz. Bu nedenle, güvenlik merkezi aks ana düğümü tarafından zaten toplanan verileri kullanarak AKS kümelerinize yönelik güvenlik avantajları sunar. Azure Güvenlik Merkezi tarafından Kubernetes ortamınızdan taranan bazı veriler hassas bilgiler içerebilir.

    ![Azure Güvenlik Merkezi ve Azure Kubernetes hizmeti (AKS) üst düzey genel bakış](./media/azure-kubernetes-service-integration/aks-asc-integration-overview.png)

## <a name="what-protections-are-provided"></a>Hangi korumalar sunulmaktadır?

İki hizmetin birlikte kullanılması aşağıdakileri sağlar:

* **Güvenlik önerileri** : Güvenlik Merkezi, aks kaynaklarınızı tanımlar ve bunları kümeler halinde tek tek sanal makinelere kategorilere ayırır. Ardından, kaynak başına güvenlik önerilerini görüntüleyebilirsiniz. Daha fazla bilgi için bkz. [önerilerin başvuru listesindeki](recommendations-reference.md#recs-containers)kapsayıcı önerileri. 

* **Ortam sağlamlaştırma** -Güvenlik Merkezi, Kubernetes kümelerinizin ve Docker yapılandırmalarının yapılandırmasını sürekli izler. Daha sonra sektör standartlarını yansıtan güvenlik önerileri oluşturur.

* **Çalışma zamanı koruması** -aşağıdaki aks kaynaklarının sürekli Analizi Ile Güvenlik Merkezi, ana bilgisayar *ve* aks kümesi düzeyinde algılanan tehditler ve kötü amaçlı etkinlikler hakkında sizi uyarır. [Kapsayıcılar için tehdit koruması hakkında daha fazla bilgi edinin](threat-protection.md#azure-containers).


     

![Azure Güvenlik Merkezi ve Azure Kubernetes hizmeti (AKS) daha ayrıntılı](./media/azure-kubernetes-service-integration/aks-asc-integration-detailed.png)



## <a name="aks-with-security-center-faq"></a>Güvenlik Merkezi ile AKS SSS

### <a name="can-i-still-get-aks-protections-without-the-log-analytics-agent"></a>Log Analytics Aracısı olmadan AKS korumalarını almaya devam edebilir miyim?

Yukarıda belirtildiği gibi, isteğe bağlı Kubernetes paketi, küme düzeyinde koruma sağlar, Azure Güvenlik Merkezi Standart katmanının Log Analytics Aracısı düğümlerinizi korur. 

Mümkün olan en iyi koruma için her ikisini de dağıtmanız önerilir.

Aracıyı konaklarınıza yüklememeyi seçerseniz, yalnızca tehdit koruması avantajları ve güvenlik uyarılarının bir alt kümesini alacaksınız. Hala kötü amaçlı sunucularla ağ analizi ve iletişimlerle ilgili uyarılar alacaksınız.



## <a name="next-steps"></a>Sonraki adımlar

Güvenlik Merkezi 'nin kapsayıcı güvenlik özellikleri hakkında daha fazla bilgi edinmek için bkz.:

* [Azure Güvenlik Merkezi ve kapsayıcı güvenliği](container-security.md)

* [Azure Container Registry ile tümleştirme](azure-container-registry-integration.md)

* [Microsoft 'Ta veri yönetimi](https://www.microsoft.com/trust-center/privacy/data-management) -Microsoft hizmetlerinin (Azure, ıntune ve Microsoft 365 dahil) veri Ilkelerini, Microsoft 'un veri yönetimiyle ilgili ayrıntıları ve verilerinizi etkileyen bekletme ilkelerini açıklar
