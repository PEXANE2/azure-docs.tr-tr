---
title: Çalışma alanlarını yönetme
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio (klasik) çalışma alanlarına erişimi yönetin ve Machine Learning API web hizmetlerini dağıtın ve yönetin
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 02/27/2017
ms.openlocfilehash: 68c9ca4c7b5f30d6f62d333014f4098fe7826c99
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79217958"
---
# <a name="manage-an-azure-machine-learning-studio-classic-workspace"></a>Azure Machine Learning Studio (klasik) çalışma alanını yönetme

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

> [!NOTE]
> Machine Learning Web Services portalında Web hizmetlerini yönetme hakkında daha fazla bilgi için [Azure Machine Learning Web Hizmetleri portalını kullanarak bir Web hizmetini yönet'e](manage-new-webservice.md)bakın.
> 
> 

Azure portalında Machine Learning Studio (klasik) çalışma alanlarını yönetebilirsiniz.



## <a name="use-the-azure-portal"></a>Azure portalı kullanma

Azure portalında bir Studio (klasik) çalışma alanını yönetmek için:

1. Azure abonelik yöneticisi hesabını kullanarak [Azure portalında](https://portal.azure.com/) oturum açın.
2. Sayfanın üst kısmındaki arama kutusuna "machine learning Studio (klasik) çalışma alanlarını" girin ve ardından **Machine Learning Studio (klasik) çalışma alanlarını**seçin.
3. Yönetmek istediğiniz çalışma alanını tıklatın.

Mevcut standart kaynak yönetimi bilgilerine ve seçeneklerine ek olarak şunları yapabilirsiniz:

- Özellikleri **Properties** Görüntüle - Bu sayfa çalışma alanı ve kaynak bilgilerini görüntüler ve bu çalışma alanının bağlı olduğu abonelik ve kaynak grubunu değiştirebilirsiniz.
- **Depolama Anahtarlarını Yeniden Eşitleme** - Çalışma alanı depolama hesabının anahtarlarını korur. Depolama hesabı anahtarları değiştirirse, anahtarları çalışma alanıyla eşitlemek için **Yeniden Eşitle tuşlarını** tıklatabilirsiniz.

Bu Studio (klasik) çalışma alanıyla ilişkili web hizmetlerini yönetmek için Machine Learning Web Services portalını kullanın. Bkz. Tam bilgi için [Azure Machine Learning Web Services portalını kullanarak bir Web hizmetini yönetin.](manage-new-webservice.md)

> [!NOTE]
> Yeni web hizmetlerini dağıtmak veya yönetmek için, web hizmetinin dağıtıldığı abonelikte bir katılımcı veya yönetici rolü atanması gerekir. Başka bir kullanıcıyı makine öğrenme Stüdyosu (klasik) çalışma alanına davet ederseniz, web hizmetlerini dağıtmadan veya yönetebilmeleri için onları abonelikteki bir katılımcı veya yönetici rolüne atamanız gerekir. 
> 
>Erişim izinleri ayarlama hakkında daha fazla bilgi için Bkz. [RBAC ve Azure portalını kullanarak erişimi yönet.](../../role-based-access-control/role-assignments-portal.md)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Kaynak Yöneticisi Şablonları ile Makine Öğrenimini dağıtma](deploy-with-resource-manager-template.md)hakkında daha fazla bilgi edinin. 
