---
title: Pazar yeri tekliflerini sil | Azure Marketi
description: Bulut İş Ortağı Portalını kullanarak Azure ve AppSource Pazar yerlerine ilişkin teklifleri silme
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: dsindona
ms.openlocfilehash: fa8ee834bd6d9261c47c540e9517c200c894b6c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80286464"
---
# <a name="delete-azure-marketplace-and-appsource-offers-or-skus"></a>Azure Marketi ve AppSource tekliflerini veya SDO'ları silme

Çeşitli nedenlerden dolayı, teklifinizi iki şekilde alabileceğiniz Microsoft pazar tarihinden geri çekmeye karar verebilirsiniz:

- *Teklif kaldırma,* yeni müşterilerin teklifinizi artık satın alamayamamasını veya dağıtmamasını, ancak lisans sözleşmenize ve ilgili yasalara göre desteklemeniz gereken mevcut müşteriler üzerinde hiçbir etkisi olmamasını sağlar. 
- *Teklif indama,* siz ve mevcut müşterileriniz arasındaki hizmet ve/veya lisans sözleşmesini sona erdirme işlemidir. 

Teklif kaldırma ve sonlandırmayla ilgili rehberlik ve ilkeler [Microsoft Marketplace Publisher Agreement](https://go.microsoft.com/fwlink/?LinkID=699560) ve Katılım [İlkeleri](https://azure.microsoft.com/support/legal/marketplace/participation-policies/) 'ne tabidir [(askıya alma ve kaldırma teklifi](https://docs.microsoft.com/legal/marketplace/participation-policy#offering-suspension-and-removal)bölümü). 

Bu makalede, farklı desteklenen silme senaryoları ve her gerçekleştirmek için gereken adımlar hakkında görüşmeler.  

> [!NOTE]
> **Editör** sekmesinin araç çubuğundaki **Sil** düğmesini seçerek yayınlanmamış bir teklifi silebilirsiniz.


## <a name="delete-a-published-sku-from-the-azure-marketplace"></a>Azure Marketinden yayımlanmış bir SKU'yu silme

Aşağıdaki adımları kullanarak Azure Marketi'nden yayınlanan bir SKU'yu silebilirsiniz:

1.  Bulut İş [Ortağı Portalı'nda](https://cloudpartner.azure.com/)oturum açın.
2.  Tüm **teklifler** sayfasında teklifinizi seçin.  Teklifiniz **Düzenleyici** sekmesinde görüntülenmelidir.
3.  Sol araç çubuğunda **SK'ler** sekmesini seçin. 
4.  Silmek istediğiniz SKU'yu seçin ve **Sil** düğmesini tıklatın.
5.  Teklifi Azure Marketi'nde [yeniden yayınlayın.](./cpp-publish-offer.md)

Değiştirilen teklif Azure Marketi'nde yayınlandıktan sonra, seçili SKU artık Azure Marketi ve Azure portalında listelenmez.


## <a name="roll-back-to-a-previous-sku-version"></a>Önceki SKU sürümüne geri dön

Burada yayınlanan bir SKU'nun geçerli sürümünü Azure Marketi'nden silebilirsiniz. İşlem tamamlandığında, SKU önceki sürümüne geri alınır.

1. Bulut İş [Ortağı Portalı'nda](https://cloudpartner.azure.com/)oturum açın.
2. Tüm **teklifler** sayfasında teklifinizi seçin.  Teklifiniz **Düzenleyici** sekmesinde görüntülenmelidir.
3. Sol araç çubuğunda **SK'ler** sekmesini seçin. 
4. İlişkili çözüm varlığının en son sürümünü disk sürümleri listesinden silin.  Teklif türüne bağlı olarak, bu alan **Disk Sürümü,** **Paket Sürümleri**veya benzer bir varlık olabilir. 
5. Teklifi Azure Marketi'nde [yeniden yayınlayın.](./cpp-publish-offer.md)

Değiştirilen teklif TheAzure Marketplace'te yayınlandıktan sonra, listelenen SKU'nun geçerli sürümü artık listelenmez. Azure Marketi'nde ve Azure portalında.  SKU önceki sürümüne geri alınır.


## <a name="delete-a-live-offer"></a>Canlı teklifi silme

Canlı bir teklifi kaldırmak için çeşitli usul, iş ve yasal yönleri vardır. Azure Marketi'nden canlı bir teklifi kaldırmak için destek ekibinden rehberlik almak için aşağıdaki adımları izleyin:

1.  [Olay oluştur](https://go.microsoft.com/fwlink/?linkid=844975) sayfasını kullanarak veya Bulut İş [Ortağı Portalı'nın](https://cloudpartner.azure.com/)sağ üst **köşesindeki Destek'i** tıklatarak destek bileti yükseltin.

2.  **Sorun türü** listesinde belirli teklif türünü seçin ve **Kategori** listesinde yayınlanmış bir **teklifi kaldır'ı** seçin.

3.  İsteğini gönderin.

Destek ekibi teklif silme işlemi boyunca size rehberlik eder.

> [!NOTE]
> Bir teklifin (veya SKU)'nın silmesi, bu teklifin (veya SKU) geçerli satın alımlarını etkilemez. Bu satın almalar eskisi gibi çalışmaya devam edecektir. Ancak, silinen teklifler veya SDO gelecekteki satın almaişlemleri için kullanılamaz.


## <a name="next-steps"></a>Sonraki adımlar

Teklifleri yönetmek için kullanılan temel işlemleri tanıdıktan sonra, Microsoft [pazar yeri teklifinin](../cpp-marketplace-offers.md)bir örneğini oluşturmaya hazırsınız.
