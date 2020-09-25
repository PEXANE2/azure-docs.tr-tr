---
title: Kubernetes için Azure Defender-avantajlar ve Özellikler
description: Kubernetes için Azure Defender 'ın avantajları ve özellikleri hakkında bilgi edinin.
author: memildin
ms.author: memildin
ms.date: 9/12/2020
ms.topic: conceptual
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: a7fb97f1036515f82fa3e61c18f95eadf0af704d
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91301712"
---
# <a name="introduction-to-azure-defender-for-kubernetes"></a>Kubernetes için Azure Defender 'a giriş

Azure Kubernetes Service (AKS), Kapsayıcılı uygulamalar geliştirmeye, dağıtmaya ve yönetmeye yönelik Microsoft tarafından yönetilen bir hizmettir.

Azure Güvenlik Merkezi ve AKS, en iyi bulut Yerel Kubernetes güvenlik sunumunu ve birlikte aşağıda özetlenen ortam sağlamlaştırma, iş yükü koruması ve çalışma zamanı koruması sağlar.

Kubernetes kümelerinize yönelik tehdit algılama için, **Kubernetes Için Azure Defender**'ı etkinleştirin.

Linux AKS düğümleriniz için ana bilgisayar düzeyinde tehdit algılama, [sunucular Için Azure Defender 'ı](defender-for-servers-introduction.md)etkinleştirdiğinizde kullanılabilir.

## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Genel olarak kullanılabilir (GA)|
|Fiyat|**Kubernetes Için Azure Defender** , [fiyatlandırma sayfasında](security-center-pricing.md) gösterildiği gibi faturalandırılır|
|Gerekli roller ve izinler:|**Güvenlik Yöneticisi** , uyarıları kapatabilir.<br>**Güvenlik okuyucusu** bulguları görüntüleyebilir.|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![No](./media/icons/no-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov)|
|||

## <a name="what-are-the-benefits-of-azure-defender-for-kubernetes"></a>Kubernetes için Azure Defender 'ın yararları nelerdir?

### <a name="run-time-protection"></a>Çalışma zamanı koruması

Güvenlik Merkezi, aşağıdaki AKS kaynaklarının sürekli analizi aracılığıyla Kapsayıcılı ortamlarınız için gerçek zamanlı tehdit koruması sağlar ve ana bilgisayar *ve* aks kümesi düzeyinde algılanan tehditler ve kötü amaçlı etkinlikler için uyarı oluşturur. Bu bilgileri kullanarak güvenlik sorunlarını hızlı bir şekilde çözebilir ve kapsayıcılarınızın güvenlik düzeyini artırabilirsiniz.

Güvenlik Merkezi, farklı düzeylerde tehdit koruması sağlar: 

- **Konak düzeyi (sunucular Için Azure Defender tarafından verilmiştir)** -Güvenlik Merkezi 'Nin diğer VM 'lerde kullandığı aynı Log Analytics aracısını kullanarak, Azure Defender, LINUX aks düğümlerinizi Web kabuğu algılama ve BILINEN şüpheli IP adresleriyle bağlantı gibi şüpheli etkinlikler için izler. Aracı Ayrıca, ayrıcalıklı kapsayıcı oluşturma, API sunucularına şüpheli erişim ve bir Docker kapsayıcısı içinde çalışan Secure Shell (SSH) sunucuları gibi kapsayıcıya özel analizler için de izler.

    AKS konak düzeyi uyarılarının bir listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-containerhost)bakın.

    >[!IMPORTANT]
    > Aracıları konaklarınıza yüklememeyi seçerseniz, yalnızca tehdit koruması avantajları ve güvenlik uyarılarının bir alt kümesini alırsınız. Hala kötü amaçlı sunucularla ağ analizi ve iletişimlerle ilgili uyarılar alacaksınız.

- **Aks kümesi düzeyi (Kubernetes Için Azure Defender tarafından sağlanır)** -küme düzeyinde tehdit koruması, Kubernetes 'ın denetim günlüklerini çözümlemeye dayanır. Bu **aracısız** izlemeyi etkinleştirmek Için Azure Defender 'ı etkinleştirin. Bu düzeyde uyarı oluşturmak için, güvenlik merkezi AKS tarafından yönetilen Hizmetleri AKS tarafından alınan günlükleri kullanarak izler. Bu düzeydeki olay örnekleri, sunulan Kubernetes panoları, yüksek ayrıcalıklı rollerin oluşturulmasını ve hassas takmaları oluşturmayı içerir.

    AKS küme düzeyi uyarılarının bir listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-akscluster)bakın.

    >[!NOTE]
    > Güvenlik Merkezi, Azure Kubernetes hizmet eylemleri ve abonelik ayarlarında Kubernetes seçeneği etkinleştirildikten sonra gerçekleşen dağıtımlar için güvenlik uyarıları oluşturur. 

Ayrıca, güvenlik araştırmacıları küresel takımımız tehdidi sürekli olarak izler. Bulundukları gibi kapsayıcıya özgü uyarıları ve güvenlik açıklarını ekler.

> [!TIP]
> [Bu blog gönderisine](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-demonstrate-the-new-containers-features-in-azure-security/ba-p/1011270)ait yönergeleri izleyerek kapsayıcı uyarılarının benzetimini yapabilirsiniz.



## <a name="how-does-security-centers-kubernetes-protection-work"></a>Güvenlik Merkezi 'nin Kubernetes koruması nasıl çalışır?

Azure Güvenlik Merkezi, Azure Kubernetes hizmeti ve Azure Ilkesi arasındaki etkileşimin üst düzey diyagramı aşağıda verilmiştir.

Güvenlik Merkezi tarafından alınan ve çözümlenen öğelerin şunları görmenizi sağlayabilirsiniz:

- API sunucusundan denetim günlükleri
- Log Analytics aracısından ham güvenlik olayları
- AKS kümesindeki küme yapılandırma bilgileri
- Azure Ilkesinden iş yükü yapılandırması ( **Kubernetes Için Azure ilke eklentisi**aracılığıyla). [Kubernetes giriş denetimini kullanan iş yükü koruma en iyi uygulamaları hakkında daha fazla bilgi edinin](container-security.md#workload-protection-best-practices-using-kubernetes-admission-control)

:::image type="content" source="./media/defender-for-kubernetes-intro/kubernetes-service-security-center-integration-detailed.png" alt-text="Azure Güvenlik Merkezi, Azure Kubernetes hizmeti ve Azure Ilkesi arasındaki etkileşimin üst düzey mimarisi" lightbox="./media/defender-for-kubernetes-intro/kubernetes-service-security-center-integration-detailed.png":::




## <a name="azure-defender-for-kubernetes---faq"></a>Kubernetes için Azure Defender-SSS

### <a name="can-i-still-get-aks-protections-without-the-log-analytics-agent"></a>Log Analytics Aracısı olmadan AKS korumalarını almaya devam edebilir miyim?

Yukarıda belirtildiği gibi, **Kubernetes için isteğe bağlı Azure Defender** planı, küme düzeyinde koruma sağlar, **sunucular için Azure Defender** Log Analytics Aracısı düğümlerinizi korur. 

Mümkün olan en iyi koruma için her ikisini de dağıtmanız önerilir.

Aracıyı konaklarınıza yüklememeyi seçerseniz, yalnızca tehdit koruması avantajları ve güvenlik uyarılarının bir alt kümesini alacaksınız. Hala kötü amaçlı sunucularla ağ analizi ve iletişimlerle ilgili uyarılar alacaksınız.


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Kubernetes için Azure Defender dahil güvenlik merkezi 'nin Kubernetes koruması hakkında bilgi edindiniz. 

İlgili malzemeler için aşağıdaki makalelere bakın: 

- [Azure Defender 'ı etkinleştir](security-center-pricing.md)
- [Uyarıları bir Azure Sentinel 'e veya üçüncü taraf SıEM 'ye aktarma](continuous-export.md)
- [Uyarıların başvuru tablosu](alerts-reference.md)