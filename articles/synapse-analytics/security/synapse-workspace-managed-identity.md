---
title: Azure Synapse çalışma alanında yönetilen kimlik
description: Azure Synapse çalışma alanında yönetilen kimliği açıklayan bir makale
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.date: 04/15/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: ee0e6249acf3fbbab369d42ae691a5a826df1ee8
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81425122"
---
# <a name="azure-synapse-workspace-managed-identity-preview"></a>Azure Synapse çalışma alanı yönetilen kimlik (önizleme)

Bu makalede, Azure Synapse çalışma alanında yönetilen kimlik hakkında bilgi edineceksiniz.

## <a name="managed-identities"></a>Yönetilen kimlikler

Azure kaynakları için yönetilen kimlik, Azure Etkin Dizini'nin bir özelliğidir. Bu özellik, Azure hizmetlerine Azure AD üzerinde otomatik olarak yönetilen bir kimlik sağlar. Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için Yönetilen Kimlik özelliğini kullanabilirsiniz.

Azure kaynakları için yönetilen kimlikler, eskiden Yönetilen Hizmet Kimliği (MSI) olarak bilinen hizmetin yeni adıdır. Daha fazla bilgi edinmek için [Yönetilen Kimlikler'e](../../active-directory/managed-identities-azure-resources/overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) bakın.

## <a name="azure-synapse-workspace-managed-identity"></a>Azure Synapse çalışma alanı yönetilen kimlik

Çalışma alanını oluştururken Azure Synapse çalışma alanınız için sistem tarafından atanmış yönetilen bir kimlik oluşturulur.

>[!NOTE]
>Bu çalışma alanı yönetilen kimlik, bu belgenin geri kalanı aracılığıyla yönetilen kimlik olarak anılacaktır.

Azure Synapse, ardışık hatları düzenlemek için yönetilen kimliği kullanır. Yönetilen kimlik yaşam döngüsü doğrudan Azure Synapse çalışma alanına bağlıdır. Azure Synapse çalışma alanını silerseniz, yönetilen kimlik de temizlenir.

Çalışma alanı yönetilen kimlik, ardışık işlemler gerçekleştirmek için izinlere ihtiyaç duyar. İzin verirken yönetilen kimliği bulmak için nesne kimliğini veya Azure Synapse çalışma alanı adınızı kullanabilirsiniz.

## <a name="retrieve-managed-identity-in-azure-portal"></a>Azure portalında yönetilen kimliği alma

Yönetilen kimliği Azure portalından alabilirsiniz. Azure portalında Azure Synapse çalışma alanınızı açın ve sol gezintiden **Genel Bakış'ı** seçin. Yönetilen kimliğin nesne kimliği ana ekranda görüntülenir.

![Yönetilen kimlik nesnesi kimliği](./media/synapse-workspace-managed-identity/workspace-managed-identity-1.png)

Azure Synapse Studio'dan yönetilen kimlik doğrulamasını destekleyen bağlantılı bir hizmet oluşturduğunuzda, yönetilen kimlik bilgileri de gösterilecek.

**Azure Synapse Studio'yu** başlatın ve sol gezintiden **Yönet** sekmesini seçin. Ardından **Bağlantılı hizmetleri** seçin ve yeni bir bağlantılı hizmet oluşturmak için **+ Yeni** seçeneğini seçin.

![Bağlantılı hizmet oluşturma 1](./media/synapse-workspace-managed-identity/workspace-managed-identity-2.png)

Yeni **bağlantılı hizmet** penceresinde *Azure Veri Gölü Depolama Gen2*yazın. Aşağıdaki listeden **Azure Veri Gölü Depolama Gen2** kaynak türünü seçin ve Devam **et'i**seçin.

![Bağlantılı hizmet oluşturma 2](./media/synapse-workspace-managed-identity/workspace-managed-identity-3.png)

Sonraki pencerede, Kimlik Doğrulama yöntemi için **Yönetilen** **Kimlik'i**seçin. Yönetilen kimliğin **Adı** ve Nesne **Kimliği'ni**görürsünüz.

![Bağlantılı hizmet oluşturma 3](./media/synapse-workspace-managed-identity/workspace-managed-identity-4.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Synapse çalışma alanı yönetilen kimliğine izin verme](./how-to-grant-workspace-managed-identity-permissions.md)
