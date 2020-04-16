---
title: Özel bağlantıları kullanarak Azure Synapse çalışma alanına bağlanma
description: Bu makale, özel bağlantıları kullanarak Azure Synapse çalışma alanınıza nasıl bağlanacağınızı size öğretecektir
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: how-to
ms.date: 04/15/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: 5a00fc44021278a8b910cf454b43b0bae2c3a1f9
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81432196"
---
# <a name="connect-to-your-azure-synapse-workspace-using-private-links-preview"></a>Azure Synapse çalışma alanınıza özel bağlantıları kullanarak bağlanın (önizleme)

Bu makalede, Azure Synapse çalışma alanınıza özel bitiş noktası nasıl oluşturulacağı öğretilir. Daha fazla bilgi edinmek için [özel bağlantılara ve özel uç noktalara](https://docs.microsoft.com/azure/private-link/) bakın.

## <a name="step-1-open-your-azure-synapse-workspace-in-azure-portal"></a>Adım 1: Azure Synapse çalışma alanınızı Azure portalında açın

**Güvenlik** altında **Özel uç nokta bağlantısını** seçin ve sonra + Özel bitiş **noktası'nı**seçin.
![Azure portalında Azure Synapse çalışma alanını açın](./media/how-to-connect-to-workspace-with-private-links/private-endpoint-1.png)

## <a name="step-2-select-your-subscription-and-region-details"></a>Adım 2: Abonelik ve bölge bilgilerinizi seçin

**Özel bitiş noktası penceresindeki** **Temel Bilgiler** sekmesinin **altında, Abonelik** ve Kaynak **Grubu'nu**seçin. Oluşturmak istediğiniz özel bitiş noktasına **ad** verin. Özel bitiş noktasının oluşturulmasını istediğiniz **Bölgeyi** seçin.

Özel uç noktaları bir alt ağda oluşturulur. Abonelik, kaynak grubu ve bölge seçili özel uç nokta alt ağlarını filtreleyin. **Sonraki'ni seçin: Kaynak >** yapıldığında.
![Abonelik ve bölge ayrıntılarını seçme](./media/how-to-connect-to-workspace-with-private-links/private-endpoint-2.png)

## <a name="step-3-select-your-azure-synapse-workspace-details"></a>Adım 3: Azure Synapse çalışma alanı ayrıntılarınızı seçin

**Kaynak** **sekmesindeki dizinde bir Azure kaynağına bağlan'ı** seçin. Azure Synapse çalışma alanınızı içeren **Aboneliği** seçin. Azure Sinaps çalışma alanına özel uç noktaları oluşturmak için **kaynak türü** *Microsoft.Synapse/çalışma alanlarıdır.*

**Kaynak**olarak Azure Synapse çalışma alanınızı seçin. Her Azure Synapse çalışma alanının özel bir bitiş noktası oluşturabileceğiniz üç **Hedef alt kaynağı** vardır: Sql, SqlOnDemand ve Dev.

**Sonraki'yi seçin: Kurulumun** bir sonraki bölümüne ilerlemek için Yapılandırma>.
![Abonelik ve bölge ayrıntılarını seçme](./media/how-to-connect-to-workspace-with-private-links/private-endpoint-3.png)

**Yapılandırma** sekmesinde, özel bitiş noktasının oluşturulması gereken **Sanal ağ** ve **Alt Net'i** seçin. Ayrıca, özel bitiş noktasına eşleyen bir DNS kaydı oluşturmanız gerekir.

Özel bitiş noktanızı özel bir **DNS bölgesiyle tümleştirmek için özel DNS bölgesiyle tümleştirme** için **Evet'i** seçin. VNet'inizle ilişkili özel bir DNS bölgeniz yoksa, yeni bir özel DNS bölgesi oluşturulur. **Gözden Geçir +** bittiğinde oluştur'u seçin.

![Abonelik ve bölge ayrıntılarını seçme](./media/how-to-connect-to-workspace-with-private-links/private-endpoint-4.png)

Dağıtım tamamlandığında, Azure portalında Azure Synapse çalışma alanınızı açın ve **Özel bitiş noktası bağlantılarını**seçin. Özel bitiş noktasıyla ilişkili yeni özel bitiş noktası ve özel bitiş noktası bağlantı adı gösterilir.

![Abonelik ve bölge ayrıntılarını seçme](./media/how-to-connect-to-workspace-with-private-links/private-endpoint-5.png)

## <a name="next-steps"></a>Sonraki adımlar

[Yönetilen çalışma alanı VNet](./synapse-workspace-managed-vnet.md) hakkında daha fazla bilgi edinin

[Yönetilen özel uç noktalar](./synapse-workspace-managed-private-endpoints.md) hakkında daha fazla bilgi edinin

[Veri kaynaklarınıza yönetilen özel uç noktaları oluşturun](./how-to-create-managed-private-endpoints.md)
