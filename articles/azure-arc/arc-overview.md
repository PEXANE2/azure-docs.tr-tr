---
title: Azure Arc'a genel bakış
description: Azure Arc 'ın ne olduğu ve müşterilerin karma kaynaklarını diğer Azure hizmetleri ve özellikleriyle yönetimi ve idare etmesine nasıl yardımcı olduğunu öğrenin.
ms.date: 08/25/2020
ms.topic: overview
ms.openlocfilehash: fa919b839683fdf870a1bf052454d8bd836a6e38
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89040480"
---
# <a name="azure-arc-overview"></a>Azure Arc'a genel bakış

Günümüzde, şirketler daha fazla ve daha karmaşık hale gelen bir ortamı kontrol etmek ve yönetmek için kullanılabilir. Bu ortamlar veri merkezleri, birden çok bulut ve kenar genelinde genişletilir. Her ortam ve bulutun, öğrenmesini ve işlemesini gerektiren, kendi ayrık yönetim araçları kümesi vardır.

Paralel olarak, mevcut araçlar yeni bulut Yerel desenleri için destek sağlayamayacağı için, yeni DevOps ve Ila operasyonel modeller uygulamak zordur.

Azure Arc, tutarlı bir çok bulut ve şirket içi yönetim platformu sunarak idare ve yönetimi basitleştirir. Azure Arc, mevcut kaynaklarınızı Azure Resource Manager bir şekilde yansıtırken tek bir cam bölmeden tüm ortamınızı yönetmenizi sağlar. Artık sanal makineleri, Kubernetes kümelerini ve veritabanlarını Azure 'da çalışıyor gibi yönetebilirsiniz. Nerede yaşdıklarından bağımsız olarak tanıdık Azure hizmetlerini ve yönetim yeteneklerini de kullanabilirsiniz. Azure Arc, ortamınızda yeni bulut Yerel düzenlerini desteklemek için DevOps uygulamalarına giriş yaparken geleneksel ıdaki 'leri kullanmaya devam etmenize olanak sağlar.

:::image type="content" source="./media/arc-overview/azure-arc-control-plane.png" alt-text="Azure Arc yönetim denetim düzlemi diyagramı" border="false":::

Azure Arc bugün Azure dışında barındırılan aşağıdaki kaynak türlerini yönetmenizi sağlar:

* Sunucular-Windows veya Linux çalıştıran fiziksel ve sanal makineler.
* Kubernetes kümeleri-çoklu Kubernetes dağıtımlarını destekleme.
* Azure veri Hizmetleri-Azure SQL veritabanı ve PostgreSQL hiper ölçek Hizmetleri.

## <a name="what-does-azure-arc-deliver"></a>Azure Arc ne sunuyor?

Azure Arc 'ın temel özellikleri şunlardır:

* Ortamınız genelinde sunucularınız için tutarlı envanter, yönetim, idare ve güvenlik uygulayın.

* Azure [VM uzantılarını](./servers/manage-vm-extensions.md) , sunucularınızı izlemek, güvenli hale getirmek ve güncelleştirmek için Azure Yönetim Hizmetleri 'ni kullanacak şekilde yapılandırın.

* Kubernetes kümelerini ölçekte yönetin ve yönetir. 

* GitHub gibi kaynak denetiminden doğrudan bir veya daha fazla küme arasında uygulama ve yapılandırma dağıtmak için, Gilar tabanlı yapılandırmayı kod yönetimi olarak kullanın.

* Azure Ilkesini kullanarak Kubernetes kümeleriniz için sıfır dokunma uyumluluğu ve yapılandırması.

* Azure Data Services 'ı tüm Kubernetes ortamında, özellikle Azure SQL yönetilen örneği ve PostgreSQL için Azure veritabanı, Azure 'da çalıştığı gibi yükseltmeler/güncelleştirmeler, güvenlik ve izleme gibi avantajlarla çalıştırın. Sürekli Azure bağlantısı olmasa bile, uygulama kapalı kalma süresi olmadan esnek ölçeklendirmeden yararlanın, güncelleştirmeleri uygulayın.

* Azure portal, Azure CLı, Azure PowerShell veya Azure REST API kullanıp kullanmayacağınızı, Azure Arc etkin kaynaklarınızı görüntüleyen birleştirilmiş bir deneyim.

## <a name="how-much-does-azure-arc-cost"></a>Azure yay maliyeti ne kadar sürer?

Aşağıda, Azure Arc ile sunulan özelliklerin fiyatlandırma ayrıntıları verilmiştir.

### <a name="arc-enabled-servers"></a>Arc özellikli sunucular

Geçerli önizleme aşamasında, Azure Arc etkin sunucuları ek bir ücret ödemeden sunulur.

Azure Güvenlik Merkezi veya Azure Izleyici gibi Arc etkin sunucularda kullanılan tüm Azure Hizmetleri, söz konusu hizmet için fiyatlandırmaya göre ücretlendirilir. Daha fazla bilgi için bkz. [Azure fiyatlandırma sayfası](https://azure.microsoft.com/pricing/).

### <a name="azure-arc-enabled-kubernetes"></a>Azure Arc özellikli Kubernetes

Geçerli önizleme aşamasında, Azure Arc etkin Kubernetes ek bir ücret ödemeden sunulmaktadır.

### <a name="azure-arc-enabled-data-services"></a>Azure Arc etkin veri Hizmetleri

Geçerli önizleme aşamasında, Azure Arc etkin veri Hizmetleri ek bir ücret ödemeden sunulmaktadır.

## <a name="next-steps"></a>Sonraki adımlar

* Yay etkin sunucular hakkında daha fazla bilgi edinmek için aşağıdaki [genel bakışa](./servers/overview.md) bakın

* Arc etkin Kubernetes hakkında daha fazla bilgi edinmek için aşağıdaki [genel bakışa](./kubernetes/overview.md) bakın

* Arc etkin veri hizmetleri hakkında daha fazla bilgi edinmek için aşağıdaki [genel bakışa](https://azure.microsoft.com/services/azure-arc/hybrid-data-services/) bakın
