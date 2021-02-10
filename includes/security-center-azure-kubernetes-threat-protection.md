---
author: memildin
ms.author: memildin
manager: rkarlin
ms.date: 06/30/2020
ms.topic: include
ms.openlocfilehash: 4c09057f606423dc92b3364e502e632a385bf83f
ms.sourcegitcommit: 49ea056bbb5957b5443f035d28c1d8f84f5a407b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2021
ms.locfileid: "100007826"
---
Güvenlik Merkezi, Kapsayıcılı ortamlarınız için gerçek zamanlı tehdit koruması sağlar ve şüpheli etkinlikler için uyarı oluşturur. Bu bilgileri kullanarak güvenlik sorunlarını hızlı bir şekilde çözebilir ve kapsayıcılarınızın güvenlik düzeyini artırabilirsiniz.

Güvenlik Merkezi, farklı düzeylerde tehdit koruması sağlar: 

* **Konak düzeyi (sunucular Için Azure Defender tarafından verilmiştir)** -Güvenlik Merkezi 'Nin diğer VM 'lerde kullandığı aynı Log Analytics aracısını kullanarak, Azure Defender, LINUX aks düğümlerinizi Web kabuğu algılama ve BILINEN şüpheli IP adresleriyle bağlantı gibi şüpheli etkinlikler için izler. Aracı Ayrıca, ayrıcalıklı kapsayıcı oluşturma, API sunucularına şüpheli erişim ve bir Docker kapsayıcısı içinde çalışan Secure Shell (SSH) sunucuları gibi kapsayıcıya özel analizler için de izler.

    Aracıları konaklarınıza yüklememeyi seçerseniz, yalnızca tehdit koruması avantajları ve güvenlik uyarılarının bir alt kümesini alırsınız. Hala kötü amaçlı sunucularla ağ analizi ve iletişimlerle ilgili uyarılar alacaksınız.

    >[!IMPORTANT]
    > Şu anda sanal makine ölçek kümelerinde çalışan Azure Kubernetes hizmet kümelerine Log Analytics aracısının yüklenmesini desteklemiyoruz.

    AKS konak düzeyi uyarılarının bir listesi için, [Uyarı tablosuna](../articles/security-center/alerts-reference.md#alerts-containerhost)bakın.


* **Aks kümesi düzeyi (Kubernetes Için Azure Defender tarafından sağlanır)** -küme düzeyinde tehdit koruması, Kubernetes 'ın denetim günlüklerini çözümlemeye dayanır. Bu **aracısız** izlemeyi etkinleştirmek Için Azure Defender 'ı etkinleştirin. Bu düzeyde uyarı oluşturmak için, güvenlik merkezi AKS tarafından yönetilen Hizmetleri AKS tarafından alınan günlükleri kullanarak izler. Bu düzeydeki olay örnekleri, sunulan Kubernetes panoları, yüksek ayrıcalıklı rollerin oluşturulmasını ve hassas takmaları oluşturmayı içerir.

    >[!NOTE]
    > Güvenlik Merkezi, Azure Kubernetes hizmet eylemleri ve abonelik ayarlarında Kubernetes seçeneği etkinleştirildikten sonra gerçekleşen dağıtımlar için güvenlik uyarıları oluşturur. 

    AKS küme düzeyi uyarılarının bir listesi için, [Uyarı tablosuna](../articles/security-center/alerts-reference.md#alerts-akscluster)bakın.

Ayrıca, güvenlik araştırmacıları küresel takımımız tehdidi sürekli olarak izler. Bulundukları gibi kapsayıcıya özgü uyarıları ve güvenlik açıklarını ekler.

> [!TIP]
> [Bu blog gönderisine](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-demonstrate-the-new-containers-features-in-azure-security/ba-p/1011270)ait yönergeleri izleyerek kapsayıcı uyarılarının benzetimini yapabilirsiniz.