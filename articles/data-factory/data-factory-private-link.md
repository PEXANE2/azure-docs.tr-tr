---
title: Azure Data Factory için Azure özel bağlantısı
description: Azure özel bağlantısının Azure Data Factory nasıl çalıştığı hakkında bilgi edinin.
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
ms.openlocfilehash: 48ab83db3dcbcf5c99b640ccab205ed1f0ee7ca1
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90604385"
---
# <a name="azure-private-link-for-azure-data-factory"></a>Azure Data Factory için Azure özel bağlantısı

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-xxx-md.md)]

Azure özel bağlantısını kullanarak, özel bir uç nokta aracılığıyla Azure 'daki çeşitli hizmet olarak platform (PaaS) dağıtımlarını bağlayabilirsiniz. Özel uç nokta, belirli bir sanal ağ ve alt ağ içindeki özel bir IP adresidir. Özel bağlantı işlevselliğini destekleyen PaaS dağıtımlarının bir listesi için bkz. [özel bağlantı belgeleri](https://docs.microsoft.com/azure/private-link/). 

## <a name="secure-communication-between-customer-networks-and-azure-data-factory"></a>Müşteri ağları ve Azure Data Factory arasında güvenli iletişim 
Azure sanal ağını, bulutta ağınızın mantıksal temsili olarak ayarlayabilirsiniz. Bunun yapılması aşağıdaki avantajları sağlar:
* Azure kaynaklarınızın ortak ağlardaki saldırılara karşı korunmasına yardımcı olursunuz.
* Ağların ve Data Factory birbirleriyle güvenli bir şekilde iletişim kurmasına izin verin. 

Ayrıca, bir Internet Protokolü güvenliği (IPSec) VPN (siteden siteye) bağlantısı veya bir Azure ExpressRoute (özel eşleme) bağlantısı ayarlayarak şirket içi bir ağı sanal ağınıza bağlayabilirsiniz. 

Şirket içi bir makineye veya sanal ağdaki bir sanal makineye şirket içinde barındırılan bir tümleştirme çalışma zamanı da yükleyebilirsiniz. Bunu yapmanız şunları yapmanızı sağlar:
* Bir bulut veri deposu ve bir özel ağdaki veri deposu arasında kopyalama etkinliklerini çalıştırın.
* Şirket içi bir ağda veya Azure sanal ağında işlem kaynaklarına karşı dönüştürme etkinliklerini gönderme. 

Aşağıdaki tabloda gösterildiği gibi Azure Data Factory ve müşteri sanal ağı arasında birkaç iletişim kanalı gereklidir:

| Etki alanı | Bağlantı noktası | Description |
| ---------- | -------- | --------------- |
| `pe-adf.azure.com` | 443 | Data Factory yazma ve izleme için gereken bir denetim düzlemi. |
| `*.{region}.datafactory.azure.net` | 443 | Şirket içinde barındırılan tümleştirme çalışma zamanı tarafından Data Factory hizmetine bağlanmak için gereklidir. |
| `*.servicebus.windows.net` | 443 | Etkileşimli yazma için şirket içinde barındırılan tümleştirme çalışma zamanı için gereklidir. |
| `download.microsoft.com` | 443 | Güncelleştirmeleri indirmek için şirket içinde barındırılan tümleştirme çalışma zamanı için gereklidir. |

Azure Data Factory için özel bağlantı desteğiyle şunları yapabilirsiniz:
* Sanal ağınızda özel bir uç nokta oluşturun.
* Belirli bir Data Factory örneğine özel bağlantıyı etkinleştirin. 

Azure Data Factory hizmetine yönelik iletişimler özel bağlantı üzerinden gidip güvenli özel bağlantı sağlanmasına yardımcı olur. Kaynaklarınızı korumanın daha güvenli bir yolunu sağlamak için bir sanal ağ veya şirket güvenlik duvarındaki yukarıdaki etki alanını ve bağlantı noktasını yapılandırmanız gerekmez.  

![Azure Data Factory mimarisine yönelik özel bağlantı diyagramı.](./media/data-factory-private-link/private-link-architecture.png)

Önceki iletişim kanallarının her biri için özel bağlantı hizmetinin etkinleştirilmesi aşağıdaki işlevleri sunar:
- **Desteklenen**:
   - Tüm giden iletişimleri engelleseniz bile, sanal ağınızda veri fabrikasını yazabilir ve izleyebilirsiniz.
   - Şirket içinde barındırılan tümleştirme çalışma zamanı ve Azure Data Factory hizmeti arasındaki komut iletişimleri, özel bir ağ ortamında güvenli bir şekilde gerçekleştirilebilir. Şirket içinde barındırılan tümleştirme çalışma zamanı ve Azure Data Factory hizmeti arasındaki trafik özel bağlantı aracılığıyla gider. 
- **Şu anda desteklenmiyor**:
   - Sağ barındırılan bir tümleştirme çalışma zamanı kullanan etkileşimli yazma, test bağlantısı, klasör listesi ve tablo listesi, şema al ve veri önizleme gibi özel bağlantı üzerinden ilerler.
   - Otomatik olarak barındırılan tümleştirme çalışma zamanının yeni sürümü, otomatik güncelleştirme 'yi etkinleştirirseniz Microsoft Indirme merkezi 'nden otomatik olarak indirilebilir.

   > [!NOTE]
   > Şu anda desteklenmeyen işlevler için, sanal ağda veya şirket güvenlik duvarında daha önce bahsedilen etki alanını ve bağlantı noktasını yapılandırmanız gerekir. 

> [!WARNING]
> Bağlı bir hizmet oluşturduğunuzda, kimlik bilgilerinizin bir Azure Anahtar Kasası 'nda depolandığından emin olun. Aksi takdirde, Azure Data Factory özel bağlantıyı etkinleştirdiğinizde kimlik bilgileri çalışmayacaktır.

## <a name="set-up-private-link-for-azure-data-factory"></a>Azure Data Factory için özel bağlantı ayarlama
[Azure Portal](https://docs.microsoft.com/azure/private-link/create-private-endpoint-portal), PowerShell veya Azure CLI kullanarak özel uç noktalar oluşturabilirsiniz.

Ayrıca, Azure portal Azure Data Factory 'nize giderek aşağıda gösterildiği gibi özel bir uç nokta oluşturabilirsiniz:

![Özel uç nokta oluşturmak için "özel uç nokta bağlantıları" bölmesinin ekran görüntüsü.](./media/data-factory-private-link/create-private-endpoint.png)


Azure Data Factory 'ye Genel erişimi engellemek ve yalnızca özel bağlantı aracılığıyla erişime izin vermek istiyorsanız, aşağıda gösterildiği gibi Azure portal Azure Data Factory için ağ erişimini devre dışı bırakın:

![Özel uç nokta oluşturmak için "ağ erişimi" bölmesinin ekran görüntüsü.](./media/data-factory-private-link/disable-network-access.png)

> [!NOTE]
> Ortak ağ erişimini devre dışı bırakmak yalnızca şirket içinde barındırılan tümleştirme çalışma zamanı için geçerlidir, Azure Integration Runtime ve SQL Server Integration Services (SSIS) Integration Runtime.

> [!NOTE]
> Ortak ağ erişimini devre dışı bıraktıktan sonra, Azure Data Factory portalına ortak bir ağ üzerinden erişmeye devam edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory UI kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-portal.md)
- [Azure Data Factory'ye giriş](introduction.md)
- [Azure Data Factory 'de görsel yazma](author-visually.md)

