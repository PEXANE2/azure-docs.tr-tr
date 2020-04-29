---
title: Azure Marketi için Bulut İş Ortağı Portalı sanal makine teklifi ayarları sekmesi
description: Azure Marketi VM teklifi oluşturma konusunda kullanılan teklif ayarları sekmesini açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: dsindona
ms.openlocfilehash: abeadf5f3c0a19212a12256e06602e840c9d1fa9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82146825"
---
# <a name="virtual-machine-offer-settings-tab"></a>Sanal Makine teklifi ayarları sekmesi

> [!IMPORTANT]
> 13 Nisan 2020 ' den itibaren, Azure sanal makine tekliflerinizin yönetimini Iş Ortağı Merkezi 'ne taşımaya başlayacağız. Geçişten sonra, Iş Ortağı Merkezi 'nde tekliflerinizi oluşturup yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure sanal makine oluşturma teklifi](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-create-offer) ' nde bulunan yönergeleri izleyin.

Sanal makineler için **yeni teklif** sayfası, **teklif ayarları**adlı ilk sekmede açılır.  

![Sanal makineler için yeni teklif sayfası](./media/publishvm_004.png)


## <a name="offer-settings-fields"></a>Teklif ayarları alanları

**Teklif ayarları** sekmesinde, aşağıdaki alanları sağlamanız gerekir.  Alan adı üzerinde eklenen bir yıldız işareti (*) gerekli olduğunu gösterir. 

|  **Alan**       |     **Açıklama**                                                          |
|  ---------       |     ---------------                                                          |
| **Teklif Kimliği\***   | Teklif için benzersiz bir tanımlayıcı (yayımcı profili içinde). Bu tanımlayıcı, ürün URL 'Lerinde, Azure Resource Manager şablonlarında ve faturalandırma raporlarında görünür olacaktır. En fazla 50 karakter uzunluğunda olabilir, yalnızca küçük harf alfasayısal karakterler ve kısa çizgilerden (-) oluşabilir, ancak kısa çizgi ile bitemez. Bu alan, bir teklif canlı olduktan sonra değiştirilemez. <br> Örneğin, contoso teklif KIMLIĞI **örneği-VM**ile bir teklif yayımladığında, Azure Marketi URL 'si `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview` atanır. |
| **'In\***  | Kuruluşunuzun Azure Marketi 'nde benzersiz tanımlayıcısı. Tüm tekliflerinizin yayımcı KIMLIĞINIZLE ilişkilendirilmesi gerekir. Teklif kaydedildikten sonra bu değer değiştirilemez. |
| **Adı\***       | Teklifiniz için görünen ad. Bu ad, Azure Marketi 'nde ve Bulut İş Ortağı Portalı görüntülenecektir. En fazla 50 karakter olabilir. Buradaki kılavuz, ürününüz için tanınabilir bir marka adı içermektedir. Pazarlanmadığı müddetçe kuruluşunuzun adını buraya eklemeyin. Bu teklifi diğer web siteleri ve yayımlarda pazartadıysanız, adın tüm yayınlarda tam olarak aynı olduğundan emin olun. |
|   |   |
 
Tüm alanları sağladıktan sonra **Kaydet** ' e tıklayın. 


## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki sekmede, teklifiniz için [SKU 'ları](./cpp-skus-tab.md) ekleyeceksiniz.
