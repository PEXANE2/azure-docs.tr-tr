---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 10/15/2020
ms.author: alkohli
ms.openlocfilehash: 354811b1db31731913f2a99287b0e4a71b2aff61
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96468857"
---
Başlamadan önce aşağıdakilerden emin olun:

* Microsoft Azure aboneliğiniz Azure Stack Edge kaynağı için etkinleştirildi. [Microsoft kurumsal anlaşma (EA)](https://azure.microsoft.com/overview/sales-number/), [bulut çözümü sağlayıcısı (CSP)](https://docs.microsoft.com/partner-center/azure-plan-lp)veya [Microsoft Azure sponsorluğu](https://azure.microsoft.com/offers/ms-azr-0036p/)gibi desteklenen bir abonelik kullandığınızdan emin olun.
* Azure Stack Edge/Azure Storage Gateway, IoT Hub ve Azure depolama kaynakları için kaynak grubu düzeyinde sahip veya katkıda bulunan erişiminiz var.

  * Herhangi bir Azure Stack Edge kaynağı oluşturmak için, kaynak grubu düzeyinde katkıda bulunan (veya üzeri) kapsamlı izinlere sahip olmanız gerekir. Ayrıca, sağlayıcının kayıtlı olduğundan emin olmanız gerekir `Microsoft.DataBoxEdge` . Nasıl kaydedileceği hakkında bilgi için [Kaynak sağlayıcısını kaydetme](../articles/databox-online/azure-stack-edge-gpu-manage-access-power-connectivity-mode.md#register-resource-providers) bölümüne gidin.
  * Herhangi bir IoT Hub kaynağı oluşturmak için Microsoft. Devices sağlayıcısının kayıtlı olduğundan emin olun. Nasıl kaydedileceği hakkında bilgi için [Kaynak sağlayıcısını kaydetme](../articles/databox-online/azure-stack-edge-gpu-manage-access-power-connectivity-mode.md#register-resource-providers) bölümüne gidin.
  * Bir depolama hesabı kaynağı oluşturmak için, kaynak grubu düzeyinde katkıda bulunan veya daha yüksek erişim kapsamına ihtiyacınız vardır. Azure depolama, varsayılan olarak kayıtlı bir kaynak sağlayıcısıdır.
* Azure Active Directory Graph API için yönetici veya Kullanıcı erişiminiz var. Daha fazla bilgi için bkz. [Azure Active Directory Graph API](https://docs.microsoft.com/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#default-access-for-administrators-users-and-guest-users-).
* Erişim kimlik bilgilerine sahip bir Microsoft Azure Storage hesabınız var.

