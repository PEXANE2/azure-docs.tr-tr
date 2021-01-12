---
title: Yönetilen kimlik (Önizleme) için rol ataması ekleme-Azure RBAC
description: Yönetilen kimlikle başlayarak bir rol ataması eklemeyi öğrenin ve ardından Azure portal ve Azure rol tabanlı erişim denetimi 'ni (Azure RBAC) kullanarak kapsamı ve rolü seçin.
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: how-to
ms.workload: identity
ms.date: 01/11/2021
ms.author: rolyon
ms.openlocfilehash: a01246c0cf35653f4d13262183cf9df28b056c69
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98122494"
---
# <a name="add-a-role-assignment-for-a-managed-identity-preview"></a>Yönetilen kimlik (Önizleme) için rol ataması ekleme

[Azure Portal kullanarak Azure rol atamaları ekleme veya kaldırma](role-assignments-portal.md)başlığı altında açıklandığı gibi, yönetilen bir kimlik için rol atamalarını, **ERIŞIM denetimi (IAM)** sayfasını kullanarak ekleyebilirsiniz. Erişim denetimi (ıAM) sayfasını kullandığınızda, kapsamla başlar ve ardından yönetilen kimliği ve rolü seçersiniz. Bu makalede, yönetilen bir kimlik için rol atamaları eklemenin alternatif bir yolu açıklanmaktadır. Bu adımları kullanarak, yönetilen kimlikle başlayıp kapsamı ve rolü seçersiniz.

> [!IMPORTANT]
> Bu alternatif adımları kullanan yönetilen bir kimlik için rol ataması eklemek Şu anda önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Azure role assignment prerequisites](../../includes/role-based-access-control/prerequisites-role-assignments.md)]

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

Yönetilen kimlikle başlayarak, sistem tarafından atanan yönetilen kimliğe bir rol atamak için bu adımları izleyin.

1. Azure portal, sistem tarafından atanan bir yönetilen kimlik açın.

1. Sol taraftaki menüden **kimlik**' e tıklayın.

    ![Sistem tarafından atanan yönetilen kimlik](./media/shared/identity-system-assigned.png)

1. **İzinler** altında **Azure rol atamaları**' na tıklayın.

    Seçilen sistem tarafından atanan yönetilen kimliğe roller zaten atanmışsa, rol atamalarının listesini görürsünüz. Bu liste, okuma izninizin olduğu tüm rol atamalarını içerir.

    ![Sistem tarafından atanan yönetilen kimlik için rol atamaları](./media/shared/role-assignments-system-assigned.png)

1. Aboneliği değiştirmek için **abonelik** listesine tıklayın.

1. **Rol ataması Ekle (Önizleme)** seçeneğine tıklayın.

1. Rol atamasının **abonelik**, **kaynak grubu** veya kaynak gibi uyguladığı kaynak kümesini seçmek için açılan listeleri kullanın.

    Seçili kapsam için rol ataması yazma izinleriniz yoksa, bir satır içi ileti görüntülenir. 

1. **Rol** açılan listesinde **Sanal Makine Katılımcısı** gibi bir rol seçin.

   ![Sistem tarafından atanan yönetilen kimlik için rol atama bölmesi ekleme](./media/role-assignments-portal-managed-identity/add-role-assignment-with-scope.png)

1. Rolü atamak için **Kaydet**’e tıklayın.

   Birkaç dakika sonra, yönetilen kimliğe seçili kapsamda rol atanır.

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Yönetilen kimlikle başlayarak Kullanıcı tarafından atanan yönetilen kimliğe bir rol atamak için bu adımları izleyin.

1. Azure portal, Kullanıcı tarafından atanan bir yönetilen kimlik açın.

1. Sol taraftaki menüden **Azure rol atamaları**' na tıklayın.

    Roller zaten seçili kullanıcı tarafından atanan yönetilen kimliğe atanmışsa, rol atamalarının listesini görürsünüz. Bu liste, okuma izninizin olduğu tüm rol atamalarını içerir.

    ![Kullanıcı tarafından atanan yönetilen kimlik için rol atamaları](./media/shared/role-assignments-user-assigned.png)

1. Aboneliği değiştirmek için **abonelik** listesine tıklayın.

1. **Rol ataması Ekle (Önizleme)** seçeneğine tıklayın.

1. Rol atamasının **abonelik**, **kaynak grubu** veya kaynak gibi uyguladığı kaynak kümesini seçmek için açılan listeleri kullanın.

    Seçili kapsam için rol ataması yazma izinleriniz yoksa, bir satır içi ileti görüntülenir. 

1. **Rol** açılan listesinde **Sanal Makine Katılımcısı** gibi bir rol seçin.

   ![Kullanıcı tarafından atanan yönetilen kimlik için rol atama bölmesi ekleme](./media/role-assignments-portal-managed-identity/add-role-assignment-with-scope.png)

1. Rolü atamak için **Kaydet**’e tıklayın.

   Birkaç dakika sonra, yönetilen kimliğe seçili kapsamda rol atanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için Yönetilen kimlikler nelerdir?](../active-directory/managed-identities-azure-resources/overview.md)
- [Azure portalını kullanarak Azure rol ataması ekleme veya kaldırma](role-assignments-portal.md)
- [Azure portal kullanarak Azure rol atamalarını listeleyin](role-assignments-list-portal.md)
