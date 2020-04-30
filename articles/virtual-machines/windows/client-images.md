---
title: Azure 'da Windows istemci görüntülerini kullanma
description: Azure 'da geliştirme ve test senaryoları için Windows 7, Windows 8 veya Windows 10 dağıtmak üzere Visual Studio abonelik avantajlarını kullanma
author: cynthn
ms.subservice: imaging
ms.service: virtual-machines-windows
ms.topic: article
ms.workload: infrastructure-services
ms.date: 12/15/2017
ms.author: cynthn
ms.openlocfilehash: 51e4862fe55428f112841ef7176cdb916653de0b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82083283"
---
# <a name="use-windows-client-in-azure-for-devtest-scenarios"></a>Azure’da geliştirme/test senaryoları için Windows istemcisini kullanma
Azure 'da, uygun bir Visual Studio (eski adıyla MSDN) aboneliğine sahip olduğunuz geliştirme ve test senaryoları için Windows 7, Windows 8 veya Windows 10 Enterprise (x64) kullanabilirsiniz. Bu makalede, Azure 'da Windows 7, Windows 8.1, Windows 10 Enterprise ve aşağıdaki Azure Galeri görüntülerinin kullanımıyla ilgili uygunluk gereksinimleri özetlenmektedir.

![Azure portal görüntü ayrıntıları](./media/client-images/windows-client-msdn-images.png) 

> [!NOTE]
> Azure Galerisi 'nde Windows 10 Pro ve Windows 10 Pro N görüntüsü için lütfen Azure Portal [Azure 'da çok kiracılı barındırma hakları](windows-desktop-multitenant-hosting-deployment.md)
>![Pro görüntü ayrıntıları ile Windows 10 ' u Dağıtma bölümüne bakın](./media/client-images/windows-client-pro-images.png) 
>

## <a name="subscription-eligibility"></a>Abonelik uygunluğu
Etkin Visual Studio aboneleri (Visual Studio aboneliği lisansı almış kişiler), geliştirme ve test amacıyla Windows istemcisini kullanabilir. Windows istemcisi, herhangi bir Azure aboneliği türünde çalışan kendi donanımınızla ve Azure sanal makinelerinizde kullanılabilir. Windows istemcisi, normal üretim kullanımı için Azure 'da dağıtılmayabilir veya Azure 'da kullanılamaz ya da etkin Visual Studio aboneleri olmayan kişiler tarafından kullanılabilir.

Size kolaylık olması için, [uygun geliştirme ve test tekliflerindeki](#eligible-offers)Azure galerisinden bazı Windows 10 görüntüleri mevcuttur. Herhangi bir teklif türündeki Visual Studio aboneleri, 64 bitlik bir Windows 7, Windows 8 veya Windows 10 görüntüsünü [yeterince hazırlayabilir ve oluşturabilir](prepare-for-upload-vhd-image.md) , sonra da [Azure 'a yükleyebilir](upload-generalized-managed.md). Kullanım, etkin Visual Studio aboneleri tarafından geliştirme ve test ile sınırlı kalır.

## <a name="eligible-offers"></a>Uygun teklifler
Aşağıdaki tabloda, Azure Galerisi aracılığıyla Windows 10 ' un dağıtılması için uygun olan teklif kimliklerinin ayrıntıları verilmiştir. Windows 10 görüntüleri yalnızca aşağıdaki tekliflere görünür. Windows istemcisini farklı bir teklif türünde çalıştırması gereken Visual Studio aboneleri, 64 bitlik bir Windows 7, Windows 8 veya Windows 10 görüntüsünü [yeterince hazırlamanızı ve oluşturmanız](prepare-for-upload-vhd-image.md) ve [ardından Azure 'a yüklemeniz](upload-generalized-managed.md)gerekir.

| Teklif Adı | Teklif numarası | Kullanılabilir istemci görüntüleri |
|:--- |:---:|:---:|
| [Kullandıkça Öde Geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0023p/) |0023P |Windows 10 |
| [Visual Studio Enterprise (MPN) aboneleri](https://azure.microsoft.com/offers/ms-azr-0029p/) |0029P |Windows 10 |
| [Visual Studio Professional aboneler](https://azure.microsoft.com/offers/ms-azr-0059p/) |0059P |Windows 10 |
| [Visual Studio Test Professional aboneleri](https://azure.microsoft.com/offers/ms-azr-0060p/) |0060P |Windows 10 |
| [Visual Studio Premium with MSDN (avantaj)](https://azure.microsoft.com/offers/ms-azr-0061p/) |0061P |Windows 10 |
| [Visual Studio Enterprise aboneleri](https://azure.microsoft.com/offers/ms-azr-0063p/) |0063P |Windows 10 |
| [Visual Studio Enterprise (BizSpark) aboneleri](https://azure.microsoft.com/offers/ms-azr-0064p/) |0064P |Windows 10 |
| [Kurumsal Geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0148p/) |0148P |Windows 10 |

## <a name="check-your-azure-subscription"></a>Azure aboneliğinizi denetleyin
Teklif KIMLIĞINIZI bilmediğinizde, bu iki şekilde Azure portal aracılığıyla elde edebilirsiniz:  

- *Abonelikler* penceresinde:

  ![Azure portal teklif KIMLIĞI ayrıntıları](./media/client-images/offer-id-azure-portal.png) 

- Ya da **faturalandırma** ' e ve ardından abonelik kimliğiniz ' ne tıklayın. Teklif KIMLIĞI *faturalandırma* penceresinde görünür.

Teklif KIMLIĞINI Azure hesap portalının [' abonelikler ' sekmesinden](https://account.windowsazure.com/Subscriptions) da görüntüleyebilirsiniz:

![Azure hesap portalından teklif KIMLIĞI ayrıntıları](./media/client-images/offer-id-azure-account-portal.png) 

## <a name="next-steps"></a>Sonraki adımlar
Artık VM 'lerinizi [PowerShell](quick-create-powershell.md), [Kaynak Yöneticisi şablonları](ps-template.md)veya [Visual Studio](../../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)kullanarak dağıtabilirsiniz.

