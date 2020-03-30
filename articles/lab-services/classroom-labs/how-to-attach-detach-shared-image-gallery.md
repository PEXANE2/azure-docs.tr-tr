---
title: Azure Laboratuvar Hizmetleri'nde paylaşılan bir resim galerisi ekleme veya ayırma | Microsoft Dokümanlar
description: Bu makalede, Azure Lab Hizmetleri'ndeki bir sınıf laboratuvarına paylaşılan bir resim galerisinin nasıl eklenebildiğini açıklanmaktadır.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/24/2020
ms.author: spelluru
ms.openlocfilehash: 00dbef7b4453ffcb54020340bde51f55827759a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284323"
---
# <a name="attach-or-detach-a-shared-image-gallery-in-azure-lab-services"></a>Azure Laboratuvar Hizmetleri'nde paylaşılan bir resim galerisi ekleme veya ayırma
Öğretmenler/laboratuvar yöneticisi, şablon vm görüntüsünü başkaları tarafından yeniden kullanılabilsi için Azure [paylaşılan resim galerisine](../../virtual-machines/windows/shared-image-galleries.md) kaydedebilir. İlk adım olarak, laboratuvar yöneticisi varolan paylaşılan bir resim galerisini laboratuvar hesabına bağlar. Paylaşılan resim galerisi bağlandıktan sonra, laboratuvar hesabında oluşturulan laboratuvarlar görüntüleri paylaşılan resim galerisine kaydedebilir. Diğer öğretmenler, sınıfları için bir şablon oluşturmak için paylaşılan resim galerisinden bu görüntüyü seçebilir. 

Bir resim paylaşılan bir resim galerisine kaydedildiğinde, Azure Lab Hizmetleri kaydedilen görüntüyü aynı [coğrafyada](https://azure.microsoft.com/global-infrastructure/geographies/)bulunan diğer bölgelere çoğaltır. Görüntünün aynı coğrafyadaki diğer bölgelerde oluşturulan laboratuvarlar için kullanılabilir olmasını sağlar. Görüntüleri paylaşılan bir resim galerisine kaydetmek, çoğaltılan tüm görüntüler için maliyet içeren ek bir maliyete neden olur. Bu maliyet, Azure Lab Hizmetleri kullanım maliyetinden ayrıdır. Paylaşılan Resim Galerisi fiyatlandırması hakkında daha fazla bilgi için [Bkz. Paylaşılan Resim Galerisi – Faturalama.]( https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#billing)

Bu makalede, paylaşılan bir resim galerisini bir laboratuvar hesabına nasıl ekbildiğinizi veya ayırabileceğinizi gösterir. 

> [!NOTE]
> Şu anda Azure Laboratuvar Hizmetleri, paylaşılan bir resim galerisinde yalnızca **genelleştirilmiş** VM görüntülerine (özel leştirilmiş resimler değil) dayalı şablon VM'lerin oluşturulmasını destekler. 


## <a name="configure-at-the-time-of-lab-account-creation"></a>Laboratuvar hesabı oluşturma sırasında yapılandırma
Bir laboratuvar hesabı oluştururken, paylaşılan bir resim galerisini laboratuvar hesabına ekleyebilirsiniz. Açılan listeden varolan paylaşılan bir resim galerisini seçebilir veya yeni bir resim oluşturabilirsiniz. Ortak bir resim galerisi oluşturmak ve laboratuvar hesabına eklemek için **yeni oluştur'u**, galeri için bir ad girin ve **Tamam'ı**girin. 

![Laboratuvar hesabı oluşturma sırasında paylaşılan resim galerisini yapılandırma](../media/how-to-use-shared-image-gallery/new-lab-account.png)

## <a name="configure-after-the-lab-account-is-created"></a>Laboratuvar hesabı oluşturulduktan sonra yapılandır
Laboratuvar hesabı oluşturulduktan sonra aşağıdaki görevleri yapabilirsiniz:

- Paylaşılan bir resim galerisi oluşturma ve ekleme
- Paylaşılan bir resim galerisini laboratuvar hesabına ekleme
- Paylaşılan resim galerisini laboratuvar hesabından ayırma

## <a name="create-and-attach-a-shared-image-gallery"></a>Paylaşılan bir resim galerisi oluşturma ve ekleme
1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sol menüden **Tüm Hizmetler'i** seçin. **DEVOPS** bölümünde **Laboratuvar Hizmetleri'ni** seçin. Laboratuvar`*` **Hizmetleri'nin**yanındaki yıldız ( ) seçeneğini seçerseniz, sol menüdeki SıK **Kullanılanlar** bölümüne eklenir. Bir sonraki andan itibaren, **Sık Kullanılanlar**altında Laboratuvar **Hizmetleri'ni** seçersiniz.

    ![Tüm Hizmetler -> Laboratuvar Hizmetleri](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. **Laboratuvar Hesabı** sayfasını görmek için laboratuvar hesabınızı seçin. 
4. Sol menüde **Paylaşılan resim galerisini** seçin ve araç çubuğunda **+ Oluştur'u** seçin.  

    ![Paylaşılan resim galerisi düğmesi oluşturma](../media/how-to-use-shared-image-gallery/new-shared-image-gallery-button.png)
5. Paylaşılan **resim galerisi oluştur** penceresinde, galeri için bir **ad** girin ve **Tamam'ı**girin. 

    ![Paylaşılan resim galerisi penceresi oluşturma](../media/how-to-use-shared-image-gallery/create-shared-image-gallery-window.png)

    Azure Lab Hizmetleri paylaşılan resim galerisini oluşturur ve laboratuvar hesabına iliştirilir. Bu laboratuvar hesabında oluşturulan tüm laboratuvarlar ekli paylaşılan resim galerisine erişebilir. 

    ![Ekli resim galerisi](../media/how-to-use-shared-image-gallery/image-gallery-in-list.png)

    Alt bölmede, paylaşılan resim galerisinde resimler görürsünüz. Bu yeni galeride resim yok. Resimleri galeriye yüklediğinizde, bunları bu sayfada görürsünüz.     

    Ekli paylaşılan resim galerisindeki tüm görüntüler varsayılan olarak etkinleştirilir. Seçili görüntüleri listede seçerek ve **seçili görüntüleri etkinleştirme** veya **seçili görüntüleri devre dışı bırak** düğmesini kullanarak seçili görüntüleri etkinleştirebilir veya devre dışı bırakabilirsiniz.

## <a name="attach-an-existing-shared-image-gallery"></a>Varolan paylaşılan resim galerisi ekleme
Aşağıdaki yordam, varolan paylaşılan bir resim galerisini bir laboratuvar hesabına nasıl eklediğinizi gösterir. 

1. Laboratuvar **Hesabı** sayfasında, sol menüde **Paylaşılan resim galerisini** seçin ve araç çubuğunda **Ekle'yi** seçin. 

    ![Paylaşılan resim galerisi - Ekle düğmesi](../media/how-to-use-shared-image-gallery/sig-attach-button.png)
5. **Varolan Paylaşılan Resim Galerisi** sayfasında paylaşılan resim galerinizi seçin ve **Tamam'ı**seçin.

    ![Varolan bir galeri seçin](../media/how-to-use-shared-image-gallery/select-image-gallery.png)
6. Aşağıdaki ekranı görüyorsunuz: 

    ![Listedeki galerim](../media/how-to-use-shared-image-gallery/my-gallery-in-list.png)
    
    Bu örnekte, paylaşılan resim galerisinde henüz resim bulunmamaktadır.

    Azure Lab Hizmetleri kimliği, laboratuvara eklenen paylaşılan resim galerisine katkıda bulunan kişi olarak eklenir. Öğretmenlerin/BT yöneticilerinin sanal makine görüntülerini paylaşılan resim galerisine kaydetmesine olanak tanır. Bu laboratuvar hesabında oluşturulan tüm laboratuvarlar ekli paylaşılan resim galerisine erişebilir. 

    Ekli paylaşılan resim galerisindeki tüm görüntüler varsayılan olarak etkinleştirilir. Seçili görüntüleri listede seçerek ve **seçili görüntüleri etkinleştirme** veya **seçili görüntüleri devre dışı bırak** düğmesini kullanarak seçili görüntüleri etkinleştirebilir veya devre dışı bırakabilirsiniz. 

## <a name="detach-a-shared-image-gallery"></a>Paylaşılan resim galerisini ayırma
Bir laboratuvara yalnızca bir paylaşılan resim galerisi eklenebilir. Başka bir paylaşılan resim galerisi eklemek istiyorsanız, yenisini takmadan önce geçerli olanı ayırın. Paylaşılan bir resim galerisini laboratuarınızdan ayırmak için araç çubuğundaki **Ayır'ı** seçin ve ayırma işlemini onaylayın. 

![Paylaşılan resim galerisini laboratuvar hesabından ayırın](../media/how-to-use-shared-image-gallery/detach.png)

## <a name="next-steps"></a>Sonraki adımlar
Paylaşılan resim galerisine bir laboratuvar görüntüsünü kaydetme veya VM oluşturmak için paylaşılan resim galerisinden bir resim kullanma hakkında bilgi edinmek için [paylaşılan resim galerisini nasıl kullanacağınızı](how-to-use-shared-image-gallery.md)öğrenin.

Genel olarak paylaşılan resim galerileri hakkında daha fazla bilgi için [paylaşılan resim galerisine](../../virtual-machines/windows/shared-image-galleries.md)bakın.
