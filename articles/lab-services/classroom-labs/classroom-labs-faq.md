---
title: Azure Laboratuvar Hizmetlerinde Sınıf laboratuvarları — SSS | Microsoft Dokümanlar
description: Bu makalede, Azure Lab Hizmetleri'ndeki sınıf laboratuvarları hakkında sık sorulan soruların (SSS) yanıtları verilmektedir.
services: lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2020
ms.author: spelluru
ms.openlocfilehash: 8d1ed128181d036af0026ae273c2c5bf1d3a066e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77443508"
---
# <a name="classroom-labs-in-azure-lab-services--frequently-asked-questions-faq"></a>Azure Laboratuvar Hizmetleri'ndeki sınıf laboratuarları — Sık sorulan sorular (SSS)
Azure Laboratuvar Hizmetleri'ndeki sınıf laboratuvarları hakkında en sık sorulan sorulardan bazılarına yanıt alın. 

## <a name="quotas"></a>Kotalar

### <a name="is-the-quota-per-user-or-per-week-or-per-entire-duration-of-the-lab"></a>Kullanıcı başına veya haftalık kota mı yoksa laboratuvar süresinin tamamı mı? 
Bir laboratuvar için belirlediğiniz kota, laboratuar süresi boyunca her öğrenci içindir. Ayrıca, [VM'lerin zamanlanan çalışma süresi,](how-to-create-schedules.md) kullanıcıya ayrılan kotayla ilgili değildir. Kontenjan, öğrencinin VM'lerde harcadığı zamanlama saatleri dışındaki süreler içindir.  Kotalar hakkında daha fazla bilgi için, [kullanıcılar için kota ları ayarla'ya](how-to-configure-student-usage.md#set-quotas-for-users)bakın.

### <a name="if-professor-turns-on-a-student-vm-does-that-affect-the-student-quota"></a>Profesör bir öğrenci VM açarsa, bu öğrenci kotasını etkiler mi? 
Hayır. Öyle değil. Profesör öğrenci VM'yi açarsa, öğrenciye ayrılan kontenjanı etkilemez. 

## <a name="schedules"></a>Zamanlamalar

### <a name="do-all-vms-in-the-lab-start-automatically-when-a-schedule-is-set"></a>Bir zamanlama ayarlandığında laboratuardaki tüm VM'ler otomatik olarak mı başlatılır? 
Hayır. Tüm VM'ler değil. Yalnızca bir zamanlamada kullanıcılara atanan VM'ler. Kullanıcıya atanmayan VM'ler otomatik olarak başlatılamez. Bu tasarım gereği. 

## <a name="lab-accounts"></a>Laboratuvar hesapları

### <a name="why-am-i-not-able-to-create-a-lab-because-of-unavailability-of-the-address-range"></a>Adres aralığının kullanılamaması nedeniyle neden bir laboratuvar oluşturamıyorum? 
Sınıf laboratuvarları, Azure portalında laboratuvar hesabınızı oluştururken belirttiğiniz bir IP adres aralığında laboratuvar VM'leri oluşturabilir. Bir adres aralığı sağlandığında, laboratuvar VM'leri için 512 IP adresi ayrıldıktan sonra oluşturulan her laboratuvar. Laboratuvar hesabının adres aralığı, laboratuvar hesabı altında oluşturmak istediğiniz tüm laboratuvarları barındıracak kadar büyük olmalıdır. 

Örneğin, /19 - 10.0.0.0/19 bloğuna sahipseniz, bu adres aralığı 8192 IP adresi ve 16 laboratuvar (8192/512 = 16 laboratuar) barındırır. Bu durumda, laboratuvar oluşturma 17 laboratuvar oluşturma başarısız olur.

### <a name="what-port-ranges-should-i-open-on-my-organizations-firewall-setting-to-connect-to-lab-virtual-machines-via-rdpssh"></a>RDP/SSH üzerinden Lab sanal makinelerine bağlanmak için kuruluşumun güvenlik duvarı ayarında hangi bağlantı noktası aralıklarını açmalıyım?

Bağlantı noktaları: 49152-65535. Sınıf laboratuvarları yük dengeleyicinin arkasında yer alıyor. Her laboratuvarda tek bir genel IP adresi vardır ve laboratuardaki her sanal makinenin benzersiz bir bağlantı noktası vardır. 

Ayrıca, Azure portalındaki laboratuvar için ana sayfanın **Sanal makine havuzu** sekmesinde her sanal makinenin özel IP adresini de görebilirsiniz. Bir laboratuarı yeniden yayımlarsanız, laboratuvarın genel IP adresi değişmez, ancak laboratuardaki her sanal makinenin özel IP ve bağlantı noktası numarası değişebilir. Makalede daha fazla bilgi edinebilirsiniz: [Azure Lab Hizmetleri için Güvenlik Duvarı ayarları.](how-to-configure-firewall-settings.md)

### <a name="what-public-ip-address-range-should-i-open-on-my-organizations-firewall-settings-to-connect-to-lab-virtual-machines-via-rdpssh"></a>RDP/SSH üzerinden Lab sanal makinelerine bağlanmak için kuruluşumun güvenlik duvarı ayarlarında hangi genel IP adres aralığını açmalıyım?
Azure'daki veri merkezleri için genel IP adresi aralığını sağlayan [Azure IP Aralıkları ve Hizmet Etiketleri — Genel Bulut'](https://www.microsoft.com/download/details.aspx?id=56519)a bakın. Laboratuvar hesaplarınızın bulunduğu bölgelerin IP adreslerini açabilirsiniz.

## <a name="virtual-machine-images"></a>Sanal makine görüntüleri

### <a name="as-a-lab-creator-why-cant-i-enable-additional-image-options-in-the-virtual-machine-images-dropdown-when-creating-a-new-lab"></a>Bir laboratuvar oluşturucusu olarak, yeni bir laboratuvar oluştururken sanal makine görüntülerinde açılan ek görüntü seçeneklerini neden etkinleştiremiyorum?

Bir yönetici sizi bir laboratuvar hesabına laboratuvar oluşturucusu olarak eklediğinde, size laboratuvar oluşturma izni verilir. Ancak, etkin sanal makine görüntüleri listesi de dahil olmak üzere laboratuvar hesabı içindeki ayarları değiştirme izniniz yoktur. Ek görüntüleri etkinleştirmek için, sizin için yapması için laboratuvar hesap yöneticinize başvurun veya yöneticiden sizi laboratuvar hesabına Katılımcı rolü olarak eklemesini isteyin. Katılımcı rolü, laboratuvar hesabındaki sanal makine resim listesini atama izni verir.

## <a name="users"></a>Kullanıcılar

### <a name="how-many-users-can-be-in-a-classroom-lab"></a>Bir sınıf laboratuvarında kaç kullanıcı olabilir?
Bir sınıf laboratuvarına en fazla 400 kullanıcı ekleyebilirsiniz. 

## <a name="blog-post"></a>Blog yazısı
[Azure Lab Hizmetleri bloguna](https://azure.microsoft.com/blog/tag/azure-lab-services/)abone olun.

## <a name="update-notifications"></a>Bildirimleri güncelleştir
Laboratuvar Hizmetleri'ndeki yeni özelliklerden haberdar olmak için [Laboratuvar Hizmetleri güncellemelerine](https://azure.microsoft.com/updates/?product=lab-services) abone olun.

## <a name="general"></a>Genel
### <a name="what-if-my-question-isnt-answered-here"></a>Ya sorum burada cevaplandırılmazsa?
Sorunuz burada listelenmemişse, bir yanıt bulmanıza yardımcı olalım.

- Bu SSS'nin sonunda bir soru gönderin. 
- Daha geniş bir kitleye ulaşmak için Azure Lab Hizmetleri ' nde bir soru yayınlayın [— Taşma Yışma forumu](https://stackoverflow.com/questions/tagged/azure-lab-services). 
- Özellik istekleri için isteklerinizi ve fikirlerinizi [Azure Lab Hizmetleri'ne](https://feedback.azure.com/forums/320373-lab-services?category_id=352774)gönderin - Kullanıcı Sesi .

