---
title: Azure Data Factory için Azure özel bağlantısı
description: Azure Data Factory 'de Azure özel bağlantısı hakkında bilgi edinin.
services: data-factory
ms.author: abnarain
author: nabhishek
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 09/01/2020
ms.openlocfilehash: 164a1005d9382711874b644e14b23d2154d613a0
ms.sourcegitcommit: 1b320bc7863707a07e98644fbaed9faa0108da97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89596031"
---
# <a name="azure-private-link-for-azure-data-factory"></a>Azure Data Factory için Azure özel bağlantısı

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-xxx-md.md)]

Özel bağlantı, Azure 'daki çeşitli PaaS hizmetlerine özel bir uç nokta aracılığıyla bağlanmanızı sağlar. Özel bağlantı işlevselliğini destekleyen PaaS hizmetlerinin bir listesi için [özel bağlantı belgeleri sayfasına](https://docs.microsoft.com/azure/private-link/)gidin. Özel uç nokta, belirli bir sanal ağ ve alt ağ içindeki özel bir IP adresidir.

## <a name="secure-communication-between-customer-network-and-azure-data-factory-service"></a>Müşteri ağı ile Azure Data Factory hizmeti arasında güvenli iletişim
Azure kaynaklarınızı ortak ağdaki saldırılara karşı korumak veya birbirleriyle güvenli bir şekilde iletişim kurmasına izin vermek için, bulutta ağınızın mantıksal temsili olarak bir Azure sanal ağı ayarlayabilirsiniz. Ayrıca, IPSec VPN (siteden siteye) veya ExpressRoute (özel eşleme) ayarlayarak da şirket içi bir ağı sanal ağınıza bağlayabilirsiniz. Şirket içinde barındırılan Integration Runtime, bir bulut veri deposu ile bir veri deposu arasında kopyalama etkinliklerini çalıştırmak veya şirket içi bir ağda veya bir Azure sanal ağında işlem kaynaklarına karşı dönüştürme etkinliklerini çalıştırmak için sanal ağ üzerinde şirket içi bir makineye veya sanal makineye yüklenebilir. 

Data Factory ve müşteri sanal ağı arasında birkaç iletişim kanalı gerekir.


| **Etki alanı** | **Bağlantı noktası** | **Açıklama** |
| ---------- | -------- | --------------- |
| `adf.azure.com` | 443 | Denetim düzlemi. Data Factory yazma ve izleme için gereklidir. |
| `*.{region}.datafactory.azure.net` | 443 | Data Factory hizmetine bağlanmak için şirket içinde barındırılan Integration Runtime gereklidir. |
| `*.servicebus.windows.net` | 443 | Etkileşimli yazma için şirket içinde barındırılan Integration Runtime gereklidir. |
| `download.microsoft.com` | 443 | Güncelleştirmeleri indirmek için şirket içinde barındırılan Integration Runtime gereklidir. |


Azure Data Factory için Azure özel bağlantısı desteğiyle, sanal ağınızda özel bir uç nokta (PE) oluşturabilir ve özel bağlantıyı belirli Azure Data Factory etkinleştirebilirsiniz. Azure Data Factory hizmetine yönelik iletişimler, güvenli özel bağlantı sağlayan Azure özel bağlantısı üzerinden gider. Ve sanal ağda veya şirket güvenlik duvarınızdan, kaynaklarınızı korumak için daha güvenli bir yol sağlayan yukarıdaki etki alanı ve bağlantı noktasını yapılandırmanız gerekmez.  

![Özel bağlantı mimarisi Azure Data Factory](./media/data-factory-private-link/private-link-architecture.png)

Aşağıda, yukarıda gösterilen iletişim kanallarının her biri için özel bağlantı hizmetini etkinleştirmenin avantajları verilmiştir:
- Destek Tüm giden iletişimleri engelleseniz bile, sanal ağınızda Azure Data Factory yazma ve izleme yapabilirsiniz.
- Destek Şirket içinde barındırılan Integration Runtime ve Azure Data Factory hizmeti arasındaki komut iletişimleri özel bir ağ ortamında güvenli bir şekilde gerçekleştirilebilir. Şirket içinde barındırılan Integration Runtime ve Azure Data Factory hizmeti arasındaki trafik özel bağlantı aracılığıyla gider. 
- (Şu anda desteklenmiyor) Şirket içinde barındırılan Integration Runtime kullanarak etkileşimli yazma, test bağlantısı, klasör listesi ve tablo listesi, şema al ve Önizleme verileri gibi özel bağlantı aracılığıyla yapılır.
- (Şu anda desteklenmiyor) Otomatik güncelleştirme 'yi etkinleştirirseniz, otomatik olarak barındırılan Integration Runtime 'nin yeni sürümü indirme merkezinden otomatik olarak indirilebilir.

> [!NOTE]
> Şu anda desteklenmeyen işlevler için, sanal ağ veya şirket güvenlik duvarınız üzerinde hala yukarıdaki etki alanı ve bağlantı noktasını yapılandırmanız gerekir. 

> [!WARNING]
> Bağlı hizmet oluşturduğunuzda, kimlik bilgisinin Azure Key Vault depolandığından emin olun. Aksi takdirde, Azure Data Factory ' de özel bağlantı hizmetini etkinleştirdiğinizde çalışmaz.

## <a name="how-to-set-up-private-link-for-azure-data-factory"></a>Azure Data Factory için özel bağlantı ayarlama
Özel uç noktalar Azure portal, PowerShell veya Azure CLı kullanılarak oluşturulabilir:

[Portal](https://docs.microsoft.com/azure/private-link/create-private-endpoint-portal)


Ayrıca, Azure portal ' de Azure Data Factory gidebilir ve özel uç nokta (PE) oluşturabilirsiniz:

![Özel Uç Nokta Oluşturma](./media/data-factory-private-link/create-private-endpoint.png)


Bu Azure Data Factory genel erişimini engellemek ve yalnızca özel bağlantı üzerinden erişime izin vermek istiyorsanız, Azure portal Azure Data Factory ağ erişimini devre dışı bırakabilirsiniz:

![Özel Uç Nokta Oluşturma](./media/data-factory-private-link/disable-network-access.png)

> [!NOTE]
> Genel ağ erişimini devre dışı bırakmak, Azure Integration Runtime ve SSIS Integration Runtime değil yalnızca şirket içinde barındırılan Integration Runtime geçerlidir.

> [!NOTE]
> Kullanıcılar, genel ağ erişimini devre dışı bıraktıktan sonra ortak ağ üzerinden Azure Data Factory portalına erişebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory UI kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-portal.md)

- [Azure Data Factory'ye giriş](introduction.md)

- [Azure Data Factory 'de görsel yazma](author-visually.md)

