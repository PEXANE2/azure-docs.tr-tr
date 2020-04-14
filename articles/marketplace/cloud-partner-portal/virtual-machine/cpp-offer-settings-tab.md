---
title: Azure Marketi Için Bulut İş Ortağı Portalı'nda Sanal Makine Teklif Ayarları sekmesi
description: Azure Marketi VM teklifi oluştururken kullanılan Teklif Ayarları sekmesini açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: dsindona
ms.openlocfilehash: 94be2e5d3c2c941ab17401a743ea86acbe8b6252
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273809"
---
# <a name="virtual-machine-offer-settings-tab"></a>Sanal makine Teklif Ayarları sekmesi

> [!IMPORTANT]
> 13 Nisan 2020'den itibaren Azure Sanal Makine tekliflerinizin yönetimini İş Ortağı Merkezi'ne taşımaya başlayacağız. Geçişten sonra, Tekliflerinizi İş Ortağı Merkezi'nde oluşturur ve yönetirsiniz. Geçirilen tekliflerinizi yönetmek için [Azure Sanal Makine Oluştur teklifindeki](https://aka.ms/CreateAzureVMoffer) yönergeleri izleyin.

Sanal makineler için **Yeni Teklif** sayfası Teklif **Ayarları**adlı ilk sekmede açılır.  

![Sanal makineler için Yeni Teklif sayfası](./media/publishvm_004.png)


## <a name="offer-settings-fields"></a>Teklif Ayarları alanları

Teklif **Ayarları** sekmesinde aşağıdaki alanları sağlamanız gerekir.  Alan adına eklenen bir yıldız işareti (*) bunun gerekli olduğunu gösterir. 

|  **Alan**       |     **Açıklama**                                                          |
|  ---------       |     ---------------                                                          |
| **Teklif Kimliği\***   | Teklif için benzersiz bir tanımlayıcı (yayımcı profili içinde). Bu tanımlayıcı ürün URL'lerinde, Azure Kaynak Yöneticisi şablonlarında ve fatura lama raporlarında görünür. Bu 50 karakter maksimum uzunluğu vardır, sadece küçük alfasayısal karakterler ve tireler (-) oluşabilir, ancak bir çizgi ile sona eremez. Bir teklif yayından çıktıktan sonra bu alan değiştirilemez. <br> Örneğin, Contoso teklif kimliği **örnek-vm**ile bir teklif yayınlarsa, `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview` Azure Marketi URL'si atanır. |
| **Yayımcı\***  | Kuruluşunuzun Azure Marketi'ndeki benzersiz tanımlayıcısı. Tüm teklifleri yayıncı kimliğinizle ilişkilendirilmelidir. Teklif kaydedildikten sonra bu değer değiştirilemez. |
| **Adı\***       | Teklifiniz için görüntü adı. Bu ad Azure Marketi'nde ve Bulut İş Ortağı Portalı'nda görüntülenir. En fazla 50 karakter olabilir. Burada rehberlik ürününuz için tanınabilir bir marka adı eklemektir. Bu şekilde pazarlanan sürece kuruluşunuzun adını buraya eklemeyin. Bu teklifi diğer web sitelerinde ve yayınlarda pazarlıyorsanız, adın tüm yayınlarında tamamen aynı olduğundan emin olun. |
|   |   |
 
Tüm alanları sağladıktan sonra **Kaydet'i** tıklatın. 


## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki sekmede, teklifinize [SNU](./cpp-skus-tab.md) eklersiniz.
