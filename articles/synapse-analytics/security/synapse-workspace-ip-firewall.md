---
title: Azure SYNAPSE Analytics 'te IP güvenlik duvarı kurallarını yapılandırma
description: Azure SYNAPSE Analytics 'te IP güvenlik duvarı kurallarını yapılandırmanızı öğretir bir makale
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.date: 04/15/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: f36a76187538d93b57d2d3f5973408f141271f67
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81424114"
---
# <a name="azure-synapse-analytics-ip-firewall-rules-preview"></a>Azure SYNAPSE Analytics IP güvenlik duvarı kuralları (Önizleme)

Bu makalede IP güvenlik duvarı kuralları açıklanacaktır ve bunları Azure SYNAPSE Analytics 'te nasıl yapılandırabileceğiniz öğretir.

## <a name="ip-firewall-rules"></a>IP güvenlik duvarı kuralları

IP güvenlik duvarı kuralları, her isteğin kaynak IP adresine göre SYNAPSE çalışma alanınıza erişim izni verir veya erişimi reddeder. Çalışma alanınız için IP güvenlik duvarı kurallarını yapılandırabilirsiniz. Çalışma alanı düzeyinde yapılandırılan IP güvenlik duvarı kuralları, çalışma alanının tüm genel uç noktaları için geçerlidir (SQL havuzları, istek üzerine SQL ve geliştirme).

## <a name="create-and-manage-ip-firewall-rules"></a>IP güvenlik duvarı kuralları oluşturma ve yönetme

IP Güvenlik Duvarı kurallarının bir Synapse çalışma alanına Eklenme iki yolu vardır. Çalışma alanınıza bir IP Güvenlik Duvarı eklemek için **güvenlik + ağ** ' ı seçin ve çalışma alanı oluşturma SıRASıNDA **tüm IP adreslerinden gelen bağlantılara izin ver** ' i işaretleyin.

![Azure portal SYNAPSE çalışma alanı IP yapılandırması.](./media/synpase-workspace-ip-firewall/ip-firewall-1.png)

![Azure portal SYNAPSE çalışma alanı IP yapılandırması.](./media/synpase-workspace-ip-firewall/ip-firewall-2.png)

Ayrıca, çalışma alanı oluşturulduktan sonra bir Synapse çalışma alanına IP güvenlik duvarı kuralları ekleyebilirsiniz. Azure portal güvenlik altında **güvenlik** **duvarları** ' nı seçin. Yeni bir IP güvenlik duvarı kuralı eklemek için, buna bir ad, başlangıç IP 'si ve bitiş IP 'si verin. Tümünü seçtikten sonra **Kaydet**'i seçin.

![Azure portal 'de Azure SYNAPSE Workspace IP yapılandırması.](./media/synpase-workspace-ip-firewall/ip-firewall-3.png)

## <a name="connecting-to-synapse-from-your-own-network"></a>Kendi ağınızdan SYNAPSE 'e bağlanma

SYNAPSE Studio kullanarak SYNAPSE çalışma alanınıza bağlanabilirsiniz. Çalışma alanınızdaki SQL kaynaklarına (SQL havuzları ve isteğe bağlı SQL) bağlanmak için SQL Server Management Studio (SSMS) de kullanabilirsiniz.

Ağınızdaki ve yerel bilgisayarınızdaki güvenlik duvarının SYNAPSE Studio için 80, 443 ve 1443 TCP bağlantı noktalarında giden iletişime izin verdiğinden emin olun.

Ayrıca, SYNAPSE Studio için UDP bağlantı noktası 53 ' de giden iletişime izin vermeniz gerekir. SSMS ve Power BI gibi araçları kullanarak bağlanmak için TCP bağlantı noktası 1433 ' de giden iletişime izin vermeniz gerekir.

Varsayılan yeniden yönlendirme bağlantısı ilke ayarını kullanıyorsanız, ek bağlantı noktalarında giden iletişime izin vermeniz gerekebilir. Burada bağlantı ilkeleri hakkında daha fazla bilgi edinebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure SYNAPSE çalışma alanı](../quickstart-create-workspace.md) oluşturma

[Yönetilen çalışma alanı VNET](./synapse-workspace-managed-vnet.md) Ile Azure SYNAPSE çalışma alanı oluşturma