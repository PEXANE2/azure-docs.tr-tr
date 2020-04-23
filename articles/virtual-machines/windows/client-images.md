---
title: Azure'da Windows istemci resimlerini kullanma
description: Geliştirme/test senaryoları için Windows 7, Windows 8 veya Windows 10'u Azure'da dağıtmak için Visual Studio abonelik avantajları nasıl kullanılır?
author: cynthn
ms.subservice: imaging
ms.service: virtual-machines-windows
ms.topic: article
ms.workload: infrastructure-services
ms.date: 12/15/2017
ms.author: cynthn
ms.openlocfilehash: 51e4862fe55428f112841ef7176cdb916653de0b
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82083283"
---
# <a name="use-windows-client-in-azure-for-devtest-scenarios"></a>Azure’da geliştirme/test senaryoları için Windows istemcisini kullanma
Uygun bir Visual Studio (eski MSDN) aboneliğiniz olması koşuluyla geliştirme/test senaryoları için Azure'da Windows 7, Windows 8 veya Windows 10 Enterprise (x64) kullanabilirsiniz. Bu makalede, Azure'da Windows 7, Windows 8.1, Windows 10 Enterprise'ı çalıştırmak ve aşağıdaki Azure Galerisi görüntülerinin kullanımı için uygunluk gereksinimleri özetlenmiştir.

![Azure portalındaki resim ayrıntıları](./media/client-images/windows-client-msdn-images.png) 

> [!NOTE]
> Azure Galerisi'ndeki Windows 10 Pro ve Windows 10 Pro N resmi için lütfen Azure portalından Multitenant Hosting Rights
>![Pro Image [ayrıntılarıyla Windows 10'u Azure'da nasıl dağıtAbilirsiniz'a](windows-desktop-multitenant-hosting-deployment.md)bakın](./media/client-images/windows-client-pro-images.png) 
>

## <a name="subscription-eligibility"></a>Abonelik uygunluğu
Active Visual Studio aboneleri (Visual Studio abonelik lisansı almış kişiler) Geliştirme ve test amacıyla Windows istemcisi kullanabilir. Windows istemcisi, herhangi bir Azure aboneliğinde çalışan kendi donanımınızda ve Azure sanal makinelerinizde kullanılabilir. Windows istemcisi, normal üretim kullanımı için Azure'da dağıtılamaz veya kullanılamaz veya etkin Visual Studio abonesi olmayan kişiler tarafından kullanılamaz.

Kolaylık sağlamak için, azure galerisinden uygun [geliştirme/test teklifleri](#eligible-offers)içinde belirli Windows 10 görüntüleri mevcuttur. Herhangi bir teklif türü içindeki Visual Studio aboneleri de [yeterli şekilde hazırlayabilir ve](prepare-for-upload-vhd-image.md) 64 bit Windows 7, Windows 8 veya Windows 10 görüntüsü oluşturabilir ve ardından [Azure'a yükleyebilir.](upload-generalized-managed.md) Kullanım, aktif Visual Studio aboneleri tarafından yapılan geliştirme/test ile sınırlı kalır.

## <a name="eligible-offers"></a>Uygun teklifler
Aşağıdaki tabloda, Windows 10'u Azure Galerisi üzerinden dağıtmaya uygun teklif i Windows 10 görüntüleri yalnızca aşağıdaki teklifler tarafından görülebilir. Windows istemcisini farklı bir teklif türünde çalıştırmanız gereken Visual Studio aboneleri, 64 bit Windows 7, Windows 8 veya Windows 10 görüntüsünü [yeterince hazırlamanızı ve oluşturmanızı](prepare-for-upload-vhd-image.md) ve [ardından Azure'a yüklemenizi](upload-generalized-managed.md)gerektirir.

| Teklif Adı | Teklif Numarası | Kullanılabilir istemci resimleri |
|:--- |:---:|:---:|
| [Kullandıkça Öde Geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0023p/) |0023P |Windows 10 |
| [Visual Studio Enterprise (MPN) aboneleri](https://azure.microsoft.com/offers/ms-azr-0029p/) |0029P |Windows 10 |
| [Visual Studio Professional aboneleri](https://azure.microsoft.com/offers/ms-azr-0059p/) |0059P |Windows 10 |
| [Visual Studio Test Professional aboneleri](https://azure.microsoft.com/offers/ms-azr-0060p/) |0060P |Windows 10 |
| [MSDN ile Visual Studio Premium (avantaj)](https://azure.microsoft.com/offers/ms-azr-0061p/) |0061P |Windows 10 |
| [Visual Studio Enterprise aboneleri](https://azure.microsoft.com/offers/ms-azr-0063p/) |0063P |Windows 10 |
| [Visual Studio Enterprise (BizSpark) aboneleri](https://azure.microsoft.com/offers/ms-azr-0064p/) |0064P |Windows 10 |
| [Kurumsal Geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0148p/) |0148P |Windows 10 |

## <a name="check-your-azure-subscription"></a>Azure aboneliğinizi kontrol edin
Teklif kimliğinizi bilmiyorsanız, bu kimliği Azure portalı üzerinden şu iki şekilde elde edebilirsiniz:  

- *Abonelikler* penceresinde:

  ![Azure portalından kimlik ayrıntıları sunun](./media/client-images/offer-id-azure-portal.png) 

- Veya, **Faturalandırma'yı** tıklatın ve ardından abonelik kimliğinizi tıklatın. Teklif kimliği *Faturapenceresinde* görünür.

Teklif kimliğini Azure Hesabı portalının ['Abonelikler' sekmesinden](https://account.windowsazure.com/Subscriptions) de görüntüleyebilirsiniz:

![Azure Hesabı portalından kimlik bilgileri sunun](./media/client-images/offer-id-azure-account-portal.png) 

## <a name="next-steps"></a>Sonraki adımlar
Artık [PowerShell,](quick-create-powershell.md) [Resource Manager şablonlarını](ps-template.md)veya Visual [Studio'u](../../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)kullanarak VM'lerinizi dağıtabilirsiniz.

