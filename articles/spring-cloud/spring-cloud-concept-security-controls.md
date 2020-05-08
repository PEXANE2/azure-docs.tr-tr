---
title: Kavram-Azure yay bulut hizmeti için güvenlik denetimleri
description: Azure yay bulut hizmetinde yerleşik olarak bulunan güvenlik denetimlerini kullanın.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 04/23/2020
ms.openlocfilehash: 5b459ef57d0e8a22ce1cd53f56c44d31e53c7c93
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82594993"
---
# <a name="security-controls-for-azure-spring-cloud-service"></a>Azure yay bulut hizmeti için güvenlik denetimleri
Güvenlik denetimleri Azure Spring bulut hizmetinde yerleşik olarak bulunur.

Güvenlik denetimi, hizmetin güvenlik açıklarını önleme, algılama ve yanıtlama yeteneğine katkıda bulunan bir Azure hizmeti kalitesi veya özelliğidir.  Her denetim için, hizmetin Şu anda kullanımda olup olmadığını belirtmek için *Evet* veya *Hayır* ' ı kullanıyoruz.  Hizmet için geçerli olmayan bir denetim için *N/A* kullandık. 

**Veri koruma güvenlik denetimleri**

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler |
|:-------------|:-------|:-------------------------------|:----------------------|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | Yes | Kaynak ve yapıtları, yapılandırma sunucusu ayarlarını, uygulama ayarlarını ve kalıcı depolama alanındaki verileri karşıya yükleyen kullanıcı, Azure depolama 'da depolanır ve bu, bekleyen içeriği otomatik olarak şifreler.<br><br>Yapılandırma sunucusu önbelleği, karşıya yüklenen kaynaktan oluşturulan çalışma zamanı ikilileri ve uygulama ömrü boyunca uygulama günlükleri, bekleyen içeriği otomatik olarak şifreleyen Azure tarafından yönetilen diske kaydedilir.<br><br>Karşıya yüklenen kullanıcının oluşturduğu kapsayıcı görüntüleri, bekleyen resim içeriğini otomatik olarak şifreleyen Azure Container Registry kaydedilir. | [Bekleyen veri için Azure Depolama şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)<br><br>[Azure yönetilen disklerinin sunucu tarafı şifrelemesi](https://docs.microsoft.com/azure/virtual-machines/linux/disk-encryption)<br><br>[Azure Container Registry kapsayıcı görüntüsü depolaması](https://docs.microsoft.com/azure/container-registry/container-registry-storage) |
| Geçici olarak şifreleme | Yes | Kullanıcı uygulaması genel uç noktaları varsayılan olarak gelen trafik için HTTPS kullanır. |  |
| Şifrelenmiş API çağrıları | Yes | Azure yay bulut hizmetini yapılandırmaya yönelik yönetim çağrıları, HTTPS üzerinden Azure Resource Manager çağrıları aracılığıyla oluşur. | [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/) |

