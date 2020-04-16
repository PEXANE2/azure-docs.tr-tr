---
title: Veri kaynağı sonuçlarınıza bağlanmak için Yönetilen özel bitiş noktası oluşturun.
description: Bu makalede, bir Azure Synapse çalışma alanından veri kaynaklarınız için yönetilen özel bitiş noktası nasıl oluşturulacağı öğretilir.
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: how-to
ms.date: 04/15/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: 363dc4d469d912c14f5f89ef6ff433a2243587e8
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81428907"
---
# <a name="create-a-managed-private-endpoint-to-your-data-source-preview"></a>Veri kaynağınıza Yönetilen özel bitiş noktası oluşturma (önizleme)

Bu makalede, Azure'daki veri kaynağınız için yönetilen özel bir bitiş noktası nasıl oluşturulacağı öğretilir. Daha fazla bilgi edinmek için [Yönetilen özel uç noktalarına](./synapse-workspace-managed-private-endpoints.md) bakın.

## <a name="step-1-open-your-azure-synapse-workspace-in-azure-portal"></a>Adım 1: Azure Synapse çalışma alanınızı Azure portalında açın

Azure Synapse Studio'dan veri kaynağınıza yönetilen özel bir bitiş noktası oluşturabilirsiniz. Azure portalında **Genel Bakış** sekmesini seçin ve **Synapse Studio'yu Başlat'ı**seçin.
![Azure Synapse Studio'u başlatın](./media/how-to-create-managed-private-endpoints/managed-private-endpoint-1.png)

## <a name="step-2-navigate-to-the-managed-virtual-networks-tab-in-synapse-studio"></a>Adım 2: Synapse Studio'da Yönetilen Sanal Ağlar sekmesine gidin

Azure Synapse Studio'da sol gezintiden **Yönet** sekmesini seçin. **Yönetilen Sanal Ağlar'ı** seçin ve ardından **+ Yeni'yi**seçin.
![Yönetilen yeni özel bitiş noktası oluşturma](./media/how-to-create-managed-private-endpoints/managed-private-endpoint-2.png)

## <a name="step-3-select-the-data-source-type"></a>Adım 3: Veri kaynağı türünü seçin

Veri kaynağı türünü seçin. Bu durumda, hedef veri kaynağı bir ADLS gen2 hesabıdır. **Devam**'ı seçin.
![Hedef veri kaynağı türü seçin](./media/how-to-create-managed-private-endpoints/managed-private-endpoint-3.png)

## <a name="step-4-enter-information-about-the-data-source"></a>Adım 4: Veri kaynağı hakkında bilgi girin

Sonraki pencerede, veri kaynağı hakkında bilgi girin. Bu örnekte, bir ADLS gen2 hesabına yönetilen özel bitiş noktası oluşturuyoruz. Yönetilen özel bitiş noktası için bir **Ad** girin. Azure **aboneliği** ve **Depolama hesabı adı**sağlayın. **Oluştur**’u seçin.
![Hedef veri kaynağı ayrıntılarını girin](./media/how-to-create-managed-private-endpoints/managed-private-endpoint-4.png)

## <a name="step-5-verify-that-your-managed-private-endpoint-was-successfully-created"></a>Adım 5: Yönetilen özel bitiş noktanızın başarıyla oluşturulduğunu doğrulama

İsteği gönderdikten sonra, isteği görürsünüz. Yönetilen özel bitiş noktanızın başarılı bir şekilde oluşturulduğunu doğrulamak için, *Sağlama Durumunu*denetleyin. Sağlama durumunu güncelleştirmek için 1 dakika beklemeniz ve **Yenile'yi** seçmeniz gerekebilir. ADLS gen2 hesabının Yönetilen özel bitiş noktasının başarıyla oluşturulduğunu görebilirsiniz.

*Onay* *Durumu'nun Beklemede*olduğunu da görebilirsiniz. Hedef kaynağın sahibi, özel uç nokta bağlantısı isteğini onaylayabilir veya reddedebilir. Sahibi özel bitiş noktası bağlantı isteğini onaylarsa, özel bir bağlantı kurulur. Reddedilirse, özel bir bağlantı kurulmaz.
![Yönetilen özel uç nokta oluşturma isteği durumu](./media/how-to-create-managed-private-endpoints/managed-private-endpoint-5.png)

## <a name="next-steps"></a>Sonraki adımlar

[Yönetilen özel uç noktalar](./synapse-workspace-managed-private-endpoints.md) hakkında daha fazla bilgi edinin