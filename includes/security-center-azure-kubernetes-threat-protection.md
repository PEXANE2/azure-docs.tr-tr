---
author: memildin
ms.author: memildin
manager: rkarlin
ms.date: 06/30/2020
ms.topic: include
ms.openlocfilehash: ba695337a1da1e440895f6b9f6fe2eb34f5041e3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85800622"
---
Güvenlik Merkezi, Kapsayıcılı ortamlarınız için gerçek zamanlı tehdit koruması sağlar ve şüpheli etkinlikler için uyarı oluşturur. Bu bilgileri kullanarak güvenlik sorunlarını hızlı bir şekilde çözebilir ve kapsayıcılarınızın güvenlik düzeyini artırabilirsiniz.

Güvenlik Merkezi, farklı düzeylerde tehdit koruması sağlar: 

* **Konak düzeyi** -Log Analytics Aracısı Linux 'u şüpheli etkinlikler için izler. Aracı, düğüm veya üzerinde çalışan bir kapsayıcıdan kaynaklanan şüpheli etkinlikler için uyarıları tetikler. Web kabuğu algılama ve bilinen şüpheli IP adresleriyle bağlantı gibi etkinliklere örnek olarak verilebilir.

    Kapsayıcılı ortamınızın güvenliğine daha ayrıntılı bir bakış için, aracı kapsayıcıya özgü Analizi izler. Ayrıcalıklı kapsayıcı oluşturma, API sunucularına şüpheli erişim ve bir Docker kapsayıcısı içinde çalışan Secure Shell (SSH) sunucuları gibi olaylar için uyarı tetikleyecektir.

    >[!IMPORTANT]
    > Aracıları konaklarınıza yüklememeyi seçerseniz, yalnızca tehdit koruması avantajları ve güvenlik uyarılarının bir alt kümesini alırsınız. Hala kötü amaçlı sunucularla ağ analizi ve iletişimlerle ilgili uyarılar alacaksınız.

    AKS konak düzeyi uyarılarının bir listesi için, [Uyarı tablosuna](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-containerhost)bakın.


* **Aks kümesi düzeyinde**tehdit koruması, Kubernetes 'ın denetim günlüklerini çözümlemeye dayalıdır. **Aracısız** izlemeyi etkinleştirmek Için, **fiyatlandırma & ayarları** sayfasından Kubernetes seçeneğini aboneliğinize ekleyin (bkz. [fiyatlandırma](https://docs.microsoft.com/azure/security-center/security-center-pricing)). Bu düzeyde uyarı oluşturmak için, güvenlik merkezi AKS tarafından yönetilen Hizmetleri AKS tarafından alınan günlükleri kullanarak izler. Bu düzeydeki olay örnekleri, sunulan Kubernetes panoları, yüksek ayrıcalıklı rollerin oluşturulmasını ve hassas takmaları oluşturmayı içerir.

    >[!NOTE]
    > Güvenlik Merkezi, Azure Kubernetes hizmet eylemleri ve abonelik ayarlarında Kubernetes seçeneği etkinleştirildikten sonra gerçekleşen dağıtımlar için güvenlik uyarıları oluşturur. 

    AKS küme düzeyi uyarılarının bir listesi için, [Uyarı tablosuna](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-akscluster)bakın.

Ayrıca, güvenlik araştırmacıları küresel takımımız tehdidi sürekli olarak izler. Bulundukları gibi kapsayıcıya özgü uyarıları ve güvenlik açıklarını ekler.

> [!TIP]
> [Bu blog gönderisine](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-demonstrate-the-new-containers-features-in-azure-security/ba-p/1011270)ait yönergeleri izleyerek kapsayıcı uyarılarının benzetimini yapabilirsiniz.