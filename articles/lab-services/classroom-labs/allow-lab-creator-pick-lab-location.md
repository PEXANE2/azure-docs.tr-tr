---
title: Laboratuvar oluşturucunun Azure Laboratuvar Hizmetleri'nde konum seçmesine izin ver
description: Bu makalede, bir laboratuvar hesabı yöneticisinin laboratuvar oluşturucularının laboratuvarları için konum ları seçmesine nasıl izin verebileceği açıklanmaktadır.
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
ms.date: 02/14/2020
ms.author: spelluru
ms.openlocfilehash: 52d5628698d1f945a7f672595ee7ce4739b6d13c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77444375"
---
# <a name="allow-lab-creator-to-pick-location-for-the-lab-in-azure-lab-services"></a>Azure Laboratuvar Hizmetleri'ndeki laboratuvar için konum belirlemesine izin verme
Azure Laboratuvar Hizmetleri'nde, bir laboratuvar hesabı sahibi laboratuvar oluşturucularının (eğitimcilerin) oluşturdukları laboratuvar için bir konum seçmesine izin verebilir. Bu konum, laboratuvar hesabının konumundan farklı olabilir. Konum, Azure bölgeleri grubudur. Örneğin, Amerika Birleşik Devletleri konumu Doğu ABD, Batı ABD ve benzeri bölgeler grubudur. 

Bir laboratuvar hesabı sahibi olarak, bir laboratuvar hesabı oluşturduğunuzda ve laboratuvar hesabını (veya varolan bir laboratuvar hesabını) oluşturduktan sonra laboratuvar konumu seçeneğini **seçmek için laboratuvar oluşturucuya izin** ver'i seçebilirsiniz. 

## <a name="at-the-time-of-lab-account-creation"></a>Laboratuvar hesabı oluşturma sırasında
Bir laboratuvar hesabı oluşturduğunuzda, bu seçeneği ilk ekranda **(Temel ler** sekmesi) görürsünüz. 

![Laboratuvar oluşturma sırasında seçeneği etkinleştirme](../media/allow-lab-creator-pick-lab-location/create-lab-account.png)

**Gelişmiş** sekmesinde laboratuvar hesabınız için bir eş sanal ağı seçerseniz, bu seçenek devre dışı bırakılır.  

![Eş sanal ağ etkinleştirildiğinde](../media/allow-lab-creator-pick-lab-location/peer-virtual-network.png)


## <a name="after-the-lab-account-is-created"></a>Laboratuvar hesabı oluşturulduktan sonra
Laboratuvar hesabını oluşturduktan sonra, aşağıdaki adımları izleyerek bu seçeneği etkinleştirebilir veya devre dışı kullanabilirsiniz: 

1. Laboratuvar **Hesabı** sayfasında, sol menüdeki **Laboratuvar ayarlarını** seçin.
2. Laboratuvar oluşturucusu laboratuvar için bir konum seçmesine izin vermek **istiyorsanız, laboratuvar konumu** seçeneğini seçmek için laboratuvar oluşturucusuna izin ver'i seçin. Devre dışı bırakılırsa, laboratuarlar otomatik olarak laboratuvar hesabının bulunduğu konumda oluşturulur. 
    
    **Eş sanal ağ** alanı için bir sanal ağ seçtiğinizde bu alan devre dışı bırakılır. Çünkü laboratuvar hesabındaki laboratuarlar, eş sanal ağdaki kaynaklara erişebilmeleri için laboratuvar hesabıyla aynı bölgede olmalıdır. 
1. Araç çubuğunda **Kaydet**’i seçin. 

    ![Laboratuvar ayarları](../media/allow-lab-creator-pick-lab-location/lab-settings.png)

## <a name="no-virtual-network-and-location-selection-isnt-allowed"></a>Sanal ağ ve konum seçimine izin verilmiyor
Bu senaryoda, **laboratuvar oluşturucusuna laboratuvar konumu** seçeneğini seçmesine izin vermediniz. 

![Laboratuvar konumu yok](../media/allow-lab-creator-pick-lab-location/lab-no-location.png)

Daha sonra, laboratuvar oluşturucuları (eğitimciler) laboratuvar için bir konum seçmek için bir seçenek görmüyorum. Onlar için kullanılabilir her boyut seçeneği için saat başına fiyat göreceksiniz. Bir laboratuvar oluşturduklarında, laboratuvar hesaplarının bulunduğu Azure bölgesiyle aynı konumda bulunan bir Azure bölgesinde oluşturulur. Örneğin, laboratuvar hesabı Batı **ABD'de**ise, o zaman laboratuvar **Güney Orta ABD'de** oluşturulabilir ama **Kanada Doğu**oluşturulmaz. Seçtiğimiz bölge yle ilgili hiçbir şeyi garanti etmiyoruz. Bir boyut şu anda kısıtlanmışsa, laboratuvar oluşturucusu normalde desteklediğimiz ancak şu anda kullanılamadığı boyutları görebilecekleri bir onay kutusu görür. 

## <a name="in-virtual-network-and-location-selection-isnt-allowed"></a>Sanal ağ da ve konum seçimine izin verilmez
Bu senaryoda, laboratuar hesabı için bir eş sanal ağ seçtiğiniziçin laboratuvar konumu seçeneğini **seçmeye izin ver** seçeneği devre dışı bırakılır. Daha sonra, laboratuvar oluşturucuları önceki seçenekle aynı ekranı görür. Tüm SANAL M'ler sanal ağla aynı Azure bölgesinde olması gerektiği için, laboratuvar sanal ağın bulunduğu aynı Azure bölgesinde oluşturulur. Söz konusu bölge bir boyut için sınırlandırılırsa, boyut kullanılamaz olarak görünür. 

## <a name="location-selection-is-enabled"></a>Konum seçimi etkin
**Laboratuvar oluşturucusu laboratuvar konumunu seçmek için izin ver'i**seçtiğinizde, laboratuvar oluşturucuları (eğitimciler) bir laboratuvar oluştururken bir konum seçme seçeneğini görür. 

![Laboratuvar konumu seçin](../media/allow-lab-creator-pick-lab-location/location-selection.png)

Laboratuvar oluşturucuları, boyutun bulunduğu tüm konumlar için fiyat aralığını görür ve bir Konum seçebilir. Laboratuvar, o konumla eş alan herhangi bir Azure bölgesinde oluşturulur.

Bir konum kısıtlanmışsa, varsayılan olarak listede gösterilmez. Açılan listeyi genişletin ve **bu boyut için kullanılamayan konumları göster'i**seçin. 

![Kullanılamayan konumları göster](../media/allow-lab-creator-pick-lab-location/show-unavailable-locations.png)

## <a name="cost"></a>Maliyet
Daha önce, fiyatlandırma laboratuvar için seçtiğiniz VM boyutuna dayanıyordu. Şimdi, fiyat İşletim Sistemi (OS), Boyut ve konumu birleşimine dayanmaktadır. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Laboratuvarağınızı bir eş sanal ağıyla bağlayın](how-to-connect-peer-virtual-network.md)
- [Paylaşılan bir resim galerisini laboratuvara ekleme](how-to-attach-detach-shared-image-gallery.md)
- [Kullanıcıyı laboratuvar sahibi olarak ekleme](how-to-add-user-lab-owner.md)
- [Laboratuvar için güvenlik duvarı ayarlarını görüntüleme](how-to-configure-firewall-settings.md)
- [Laboratuvar için diğer ayarları yapılandırma](how-to-configure-lab-accounts.md)