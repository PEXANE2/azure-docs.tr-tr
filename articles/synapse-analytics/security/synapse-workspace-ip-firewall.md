---
title: Azure Synapse Analytics'te IP güvenlik duvarı kurallarını yapılandırma
description: Azure Synapse Analytics'te IP güvenlik duvarı kurallarını yapılandırmayı öğreten bir makale
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.date: 04/15/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: f36a76187538d93b57d2d3f5973408f141271f67
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424114"
---
# <a name="azure-synapse-analytics-ip-firewall-rules-preview"></a>Azure Synapse Analytics IP güvenlik duvarı kuralları (önizleme)

Bu makalede, IP güvenlik duvarı kuralları açıklanacaktır ve Azure Synapse Analytics'te bunları nasıl yapılandıracağınızı öğretecektir.

## <a name="ip-firewall-rules"></a>IP güvenlik duvarı kuralları

IP güvenlik duvarı kuralları, her isteğin menşeli IP adresine bağlı olarak Synapse çalışma alanınıza erişim izni verir veya reddeder. Çalışma alanınız için IP güvenlik duvarı kurallarını yapılandırabilirsiniz. Çalışma alanı düzeyinde yapılandırılan IP güvenlik duvarı kuralları çalışma alanının tüm ortak uç noktaları (SQL havuzları, isteğe bağlı SQL ve Geliştirme) için geçerlidir.

## <a name="create-and-manage-ip-firewall-rules"></a>IP güvenlik duvarı kuralları oluşturma ve yönetme

Bir Synapse çalışma alanına IP güvenlik duvarı kurallarının eklenmesinin iki yolu vardır. Çalışma alanınıza bir IP güvenlik duvarı eklemek için **Security + networking'i** seçin ve çalışma alanı oluşturma sırasında **tüm IP adreslerinden bağlantılara izin ver'i** işaretleyin.

![Azure portalı Synapse çalışma alanı IP yapılandırması.](./media/synpase-workspace-ip-firewall/ip-firewall-1.png)

![Azure portalı Synapse çalışma alanı IP yapılandırması.](./media/synpase-workspace-ip-firewall/ip-firewall-2.png)

Çalışma alanı oluşturulduktan sonra Bir Synapse çalışma alanına IP güvenlik duvarı kuralları da ekleyebilirsiniz. Azure portalından **Güvenlik** altında **Güvenlik Duvarları'nı** seçin. Yeni bir IP güvenlik duvarı kuralı eklemek için, bir ad verin, IP Başlat ve IP'yi Bitirin. Tümünü seçtikten sonra **Kaydet**'i seçin.

![Azure portalında Azure Synapse çalışma alanı IP yapılandırması.](./media/synpase-workspace-ip-firewall/ip-firewall-3.png)

## <a name="connecting-to-synapse-from-your-own-network"></a>Kendi ağınızdan Synapse'ye bağlanma

Synapse Studio'nuzu kullanarak Synapse çalışma alanınıza bağlanabilirsiniz. Çalışma alanınızdaki SQL kaynaklarına (SQL havuzları ve isteğe bağlı SQL) bağlanmak için SQL Server Management Studio'yu (SSMS) de kullanabilirsiniz.

Ağınızdaki ve yerel bilgisayarınızdaki güvenlik duvarının Synapse Studio için TCP bağlantı noktaları 80, 443 ve 1443'te giden iletişime izin verdiğinden emin olun.

Ayrıca, Synapse Studio için UDP bağlantı noktası 53'te giden iletişime izin verebilirsiniz. SSMS ve Power BI gibi araçları kullanarak bağlanmak için TCP bağlantı noktası 1433'te giden iletişime izin vermelisiniz.

Varsayılan Yeniden Yönlendirme bağlantı ilkesi ayarını kullanıyorsanız, ek bağlantı noktalarında giden iletişime izin vermeniz gerekebilir. Bağlantı ilkeleri hakkında daha fazla bilgi edinebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure [Synapse Çalışma Alanı](../quickstart-create-workspace.md) Oluşturma

[Yönetilen çalışma alanı VNet](./synapse-workspace-managed-vnet.md) ile Azure Synapse çalışma alanı oluşturma