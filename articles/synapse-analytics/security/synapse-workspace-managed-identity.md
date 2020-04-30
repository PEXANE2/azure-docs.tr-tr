---
title: Azure SYNAPSE çalışma alanında yönetilen kimlik
description: Azure SYNAPSE çalışma alanında yönetilen kimliği açıklayan bir makale
author: RonyMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.date: 04/15/2020
ms.author: ronytho
ms.reviewer: jrasnick
ms.openlocfilehash: ee0e6249acf3fbbab369d42ae691a5a826df1ee8
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81425122"
---
# <a name="azure-synapse-workspace-managed-identity-preview"></a>Azure SYNAPSE çalışma alanı yönetilen kimliği (Önizleme)

Bu makalede, Azure SYNAPSE çalışma alanında yönetilen kimlik hakkında bilgi edineceksiniz.

## <a name="managed-identities"></a>Yönetilen kimlikler

Azure kaynakları için yönetilen kimlik Azure Active Directory bir özelliktir. Bu özellik, Azure hizmetlerine Azure AD üzerinde otomatik olarak yönetilen bir kimlik sağlar. Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmak için yönetilen kimlik özelliğini kullanabilirsiniz.

Azure kaynakları için Yönetilen kimlikler, daha önce Yönetilen Hizmet Kimliği (MSI) olarak bilinen hizmetin yeni adıdır. Daha fazla bilgi için bkz. [Yönetilen kimlikler](../../active-directory/managed-identities-azure-resources/overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) .

## <a name="azure-synapse-workspace-managed-identity"></a>Azure SYNAPSE çalışma alanı yönetilen kimliği

Çalışma alanını oluştururken, Azure SYNAPSE çalışma alanınız için sistem tarafından atanan bir yönetilen kimlik oluşturulur.

>[!NOTE]
>Bu çalışma alanı yönetilen kimliği, bu belgenin geri kalanı aracılığıyla yönetilen kimlik olarak anılacaktır.

Azure SYNAPSE, işlem hatlarını düzenlemek için yönetilen kimliği kullanır. Yönetilen kimlik yaşam döngüsü doğrudan Azure SYNAPSE çalışma alanına bağlıdır. Azure SYNAPSE çalışma alanını silerseniz yönetilen kimlik de temizlenir.

Çalışma alanı yönetilen kimliği, işlem hatlarında işlemler gerçekleştirmek için izinlere ihtiyaç duyuyor. İzinleri verirken yönetilen kimliği bulmak için nesne KIMLIĞINI veya Azure SYNAPSE çalışma alanı adınızı kullanabilirsiniz.

## <a name="retrieve-managed-identity-in-azure-portal"></a>Azure portal yönetilen kimliği al

Yönetilen kimliği Azure portal elde edebilirsiniz. Azure SYNAPSE çalışma alanınızı Azure portal açın ve sol gezinmede **genel bakış** ' ı seçin. Yönetilen kimliğin nesne KIMLIĞI, ana ekranda görüntülenir.

![Yönetilen kimlik nesnesi KIMLIĞI](./media/synapse-workspace-managed-identity/workspace-managed-identity-1.png)

Yönetilen kimlik bilgilerini Azure SYNAPSE Studio 'dan yönetilen kimlik doğrulamasını destekleyen bir bağlı hizmet oluşturduğunuzda, yönetilen kimlik bilgileri de görüntülenir.

**Azure SYNAPSE Studio 'yu** başlatın ve sol gezinti bölmesinde **Yönet** sekmesini seçin. Ardından **bağlı hizmetler** ' i seçin ve yeni bir bağlı hizmet oluşturmak Için **+ Yeni** seçeneğini belirleyin.

![Bağlı hizmet oluşturma 1](./media/synapse-workspace-managed-identity/workspace-managed-identity-2.png)

**Yeni bağlı hizmet** penceresinde *Azure Data Lake Storage 2.* yazın. Aşağıdaki listeden **Azure Data Lake Storage 2.** kaynak türünü seçin ve **devam**' ı seçin.

![Bağlı hizmet oluşturma 2](./media/synapse-workspace-managed-identity/workspace-managed-identity-3.png)

Sonraki pencerede, **kimlik doğrulama yöntemi**Için **yönetilen kimlik** ' i seçin. Yönetilen kimliğin **adını** ve **nesne kimliğini**görürsünüz.

![Bağlı hizmet oluşturma 3](./media/synapse-workspace-managed-identity/workspace-managed-identity-4.png)

## <a name="next-steps"></a>Sonraki adımlar

[Azure SYNAPSE çalışma alanı yönetilen kimliği için izinler veriliyor](./how-to-grant-workspace-managed-identity-permissions.md)
