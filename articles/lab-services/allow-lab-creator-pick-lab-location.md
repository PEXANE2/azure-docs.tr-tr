---
title: Laboratuvar oluşturucusunun Azure Lab Services konum seçmesine izin ver
description: Bu makalede laboratuvar hesabı yöneticisinin laboratuvar oluşturucuları Labs için konum seçmesine nasıl izin verilebileceğiniz açıklanır.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 3b6c2d83414b1abc763755fbf15402b122c2186b
ms.sourcegitcommit: 74ba70139781ed854d3ad898a9c65ef70c0ba99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85444208"
---
# <a name="allow-lab-creator-to-pick-location-for-the-lab-in-azure-lab-services"></a>Laboratuvar oluşturucusunun Azure Lab Services laboratuvar için konum seçmesine izin ver
Azure Lab Services, bir laboratuar hesabı sahibi, laboratuvar oluşturucularının (eğitimciler) oluşturdukları laboratuvar için bir konum seçmesine izin verebilir. Bu konum, laboratuvar hesabının konumundan farklı olabilir. Konum bir Azure bölgesi grubudur. Örneğin Birleşik Devletler konum Doğu ABD, Batı ABD vb. gibi bir bölge grubudur. 

Laboratuvar hesabı sahibi olarak, bir laboratuar hesabı oluştururken ve laboratuvar hesabı (veya mevcut bir laboratuvar hesabı) oluşturduktan sonra laboratuvar **konumu seçmesini Izin ver** seçeneğini belirleyebilirsiniz. 

## <a name="at-the-time-of-lab-account-creation"></a>Laboratuvar hesabı oluşturma sırasında
Laboratuvar hesabı oluşturduğunuzda, bu seçeneği ilk ekranda görürsünüz (**temel bilgiler** sekmesi). 

![Laboratuvar oluşturma sırasında seçeneği etkinleştirin](./media/allow-lab-creator-pick-lab-location/create-lab-account.png)

**Gelişmiş** sekmesinde laboratuvar hesabınız için bir eş sanal ağ seçerseniz bu seçenek devre dışıdır.  

![Eş sanal ağ etkinleştirildiğinde](./media/allow-lab-creator-pick-lab-location/peer-virtual-network.png)


## <a name="after-the-lab-account-is-created"></a>Laboratuvar hesabı oluşturulduktan sonra
Laboratuvar hesabını oluşturduktan sonra, aşağıdaki adımları izleyerek bu seçeneği etkinleştirebilir veya devre dışı bırakabilirsiniz: 

1. **Laboratuvar hesabı** sayfasında, soldaki menüden **Laboratuvar ayarları** ' nı seçin.
2. Laboratuvar oluşturucusunun laboratuvar için bir konum seçmesine izin vermek istiyorsanız, laboratuvar Oluşturucu oluşturma laboratuvar **konumunu** Seç seçeneğini belirleyin. Devre dışıysa, laboratuvarlar laboratuvar hesabının mevcut olduğu yerde otomatik olarak oluşturulur. 
    
    **Eş sanal ağ** alanı için bir sanal ağ seçtiğinizde bu alan devre dışıdır. Bunun nedeni, laboratuvar hesabındaki laboratuvarların, eş sanal ağdaki kaynaklara erişmesi için laboratuvar hesabıyla aynı bölgede olması gerekir. 
1. Araç çubuğunda **Kaydet**’i seçin. 

    ![Laboratuvar ayarları](./media/allow-lab-creator-pick-lab-location/lab-settings.png)

## <a name="no-virtual-network-and-location-selection-isnt-allowed"></a>Sanal ağ ve konum seçimine izin verilmiyor
Bu senaryoda, **Laboratuvar oluşturucunun laboratuvar konumunu seçmesini Izin ver** seçeneğini etkinleştirmemiş olursunuz. 

![Laboratuvar konumu yok](./media/allow-lab-creator-pick-lab-location/lab-no-location.png)

Ardından, laboratuvar oluşturucuları (eğitimciler) laboratuvar için bir konum seçme seçeneği görmez. Bunlar için kullanılabilir her boyut seçeneği için saat başına fiyat görüntülenir. Laboratuvar oluşturduklarında, laboratuvar hesaplarının bulunduğu Azure bölgesi ile aynı konumdaki bir Azure bölgesinde oluşturulur. Örneğin, laboratuvar hesabı **Batı ABD**ise, laboratuvar **Orta Güney ABD** oluşturulmuş olabilir ancak **Kanada Doğu**oluşturulmaz. Konumda olduğu gibi seçtiğimiz bölge hakkında hiçbir şeyin garantisi vermeyiz. Bir boyut Şu anda kısıtlanmamışsa, laboratuvar Oluşturucu normalde desteklediğimiz ancak şu anda kullanılamayan boyutları görebilecekleri bir onay kutusu görür. 

## <a name="in-virtual-network-and-location-selection-isnt-allowed"></a>Sanal ağ ve konum seçimine izin verilmiyor
Bu senaryoda, laboratuvar hesabı için bir eş sanal ağı seçtiğiniz için laboratuvar **oluşturucusunun laboratuvar konumunu seçmesine Izin ver** seçeneği devre dışı bırakıldı. Daha sonra laboratuvar oluşturucuları, önceki seçenekle aynı ekranı görür. Tüm VM 'Lerin sanal ağla aynı Azure bölgesinde olması gerektiğinden, laboratuvar sanal ağın içinde bulunduğu aynı Azure bölgesinde oluşturulacaktır. Belirli bir bölge bir boyut için kısıtlanmamışsa boyut kullanılamaz olarak görünür. 

## <a name="location-selection-is-enabled"></a>Konum seçimi etkin
Laboratuvar **oluşturucusunun laboratuvar konumunu seçmesine Izin ver**' i seçtiğinizde laboratuvar oluşturucuları (eğitimciler) laboratuvar oluştururken konum seçme seçeneği için bir seçenek görürsünüz. 

![Laboratuvar konumu seçin](./media/allow-lab-creator-pick-lab-location/location-selection.png)

Laboratuvar oluşturucuları, boyutun bulunduğu tüm konumlar için fiyat aralığını görün ve bir konum seçebilir. Laboratuvar, bu konumla eşleşen herhangi bir Azure bölgesinde oluşturulur.

Bir konum kısıtlanmamışsa, varsayılan olarak listede gösterilmez. Açılan listeyi genişletin ve **Bu boyut için kullanılamayan konumları göster**' i seçin. 

![Kullanılamayan konumları göster](./media/allow-lab-creator-pick-lab-location/show-unavailable-locations.png)

## <a name="cost"></a>Maliyet
Daha önce, fiyatlandırma, laboratuvar için seçtiğiniz VM boyutunu temel alır. Artık Fiyat, Işletim sistemi (OS), boyut ve konum birleşimini temel alır. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Laboratuvarınızın ağını eş bir sanal ağla bağlama](how-to-connect-peer-virtual-network.md)
- [Laboratuvara paylaşılan görüntü Galerisi iliştirme](how-to-attach-detach-shared-image-gallery.md)
- [Laboratuvar sahibi olarak Kullanıcı ekleme](how-to-add-user-lab-owner.md)
- [Laboratuvar için güvenlik duvarı ayarlarını görüntüleme](how-to-configure-firewall-settings.md)
- [Laboratuvar için diğer ayarları yapılandırma](how-to-configure-lab-accounts.md)