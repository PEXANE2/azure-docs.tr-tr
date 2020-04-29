---
title: Azure kapsayıcıları görüntüsü teklifi | Azure Marketi
description: Azure Marketi 'nde bir kapsayıcı teklifi yayımlama sürecine genel bakış.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: dsindona
ms.openlocfilehash: 81908de5c2fb3960684ed6cf37952e8815d8a5d5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82148305"
---
# <a name="containers"></a>Kapsayıcılar

> [!IMPORTANT]
> 13 Nisan 2020 ' den itibaren, Azure Container tekliflerinizin yönetimini Iş Ortağı Merkezi 'ne taşımaya başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilmiş tekliflerinizi yönetmek için [bir Azure kapsayıcı oluşturma teklifi oluşturma](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-azure-container-offer) bölümündeki yönergeleri izleyin.

<table> <tr> <td>Bu bölümde, <a href="https://azuremarketplace.microsoft.com">Azure Marketi 'nde</a>bir kapsayıcı görüntüsünün nasıl yayımlanacağı açıklanmaktadır.  
Kapsayıcı teklif türü, <a href="https://docs.microsoft.com/azure/aks/index">Azure Kubernetes hizmet</a> örnekleri veya <a href="https://docs.microsoft.com/azure/container-instances/container-instances-overview">Azure Container Instances</a> olarak sağlanan ve bir <a href="https://docs.microsoft.com/azure/container-registry">Azure Container Registry</a> deposunda barındırılan Docker kapsayıcı görüntülerini destekler. </td> <td><img src="./media/container-icon.png"  alt="Azure container icon" /></td> </tr> </table>

## <a name="offer-components"></a>Teklif bileşenleri

Bu bölümde bir kapsayıcı yayımlama öğeleri özetlenmektedir ve yayımcı Azure Marketi 'ne yönelik bir kılavuz olarak hazırlanmıştır. Yayımlama aşağıdaki ana bölümlere ayrılmıştır:

- [Önkoşullar](./cpp-prerequisites.md) -bir kapsayıcı teklifi oluşturmadan veya yayınlamadan önce teknik ve iş gereksinimlerini listeler.
- [Teklifi oluşturma](./cpp-create-offer.md) -bulut iş ortağı portalı kullanarak yeni bir kapsayıcı teklifi girişi oluşturmak için gereken adımları listeler.
- [Teknik varlıkları hazırlama](./cpp-create-technical-assets.md) -Azure Marketi 'nde teklif olarak bir kapsayıcı çözümü için teknik varlıkları oluşturma.
- [Teklifi yayımlayın](./cpp-publish-offer.md) -teklifi Azure Marketi 'nde yayımlama için gönderme.

## <a name="container-publishing-process"></a>Kapsayıcı yayımlama işlemi

Aşağıdaki diyagramda bir VM teklifi yayımlamanın üst düzey adımları gösterilmektedir.
![Teklif yayımlama adımları](./media/containers-offer-process.png)

Bir kapsayıcı teklifini yayımlamaya yönelik üst düzey adımlar şunlardır:

1. Teklifi oluşturun-teklifle ilgili ayrıntılı bilgi sağlayın. Bu bilgiler şunları içerir: teklif açıklaması, pazarlama malzemeleri, destek bilgileri ve varlık belirtimleri.
2. İş ve teknik varlıkları oluşturma-ilişkili çözüme yönelik iş varlıklarını (yasal belgeler ve pazarlama malzemeleri) ve teknik varlıkları oluşturun (bir Azure Container Registry barındırılan kapsayıcılar görüntüleri).
3. SKU 'YU oluşturun-teklifle ilişkili SKU 'ları oluşturun. Yayımlamayı planladığınız her görüntü için benzersiz bir SKU gereklidir.
4. Teklifi onaylama ve yayımlama-teklif ve teknik varlıklar tamamlandıktan sonra teklifi gönderebilirsiniz. Bu gönderim, yayımlama işlemini başlatır. Bu işlem sırasında çözüm, Azure Marketi 'nde test edilmiş, doğrulanan, sertifikalı ve "canlı gidiyor" olarak yapılır.

## <a name="next-steps"></a>Sonraki adımlar

Bu adımları kabul etmeden önce, bir kapsayıcıyı Microsoft Azure Market yayımlamak için [Teknik ve iş gereksinimlerini](./cpp-prerequisites.md) karşılamanız gerekir.
