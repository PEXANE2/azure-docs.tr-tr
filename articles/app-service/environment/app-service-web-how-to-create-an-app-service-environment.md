---
title: ASE v1 oluşturma
description: Bir uygulama servis ortamı v1 için oluşturma akışı açıklaması. Bu doküman yalnızca eski v1 ASE'yi kullanan müşteriler için sağlanır.
author: ccompy
ms.assetid: 81bd32cf-7ae5-454b-a0d2-23b57b51af47
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 752334e3d594b1f95786aecaca134b74c4e264d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74688686"
---
# <a name="how-to-create-an-app-service-environment-v1"></a>Nasıl Bir App Servis Ortamı v1 oluşturma 

> [!NOTE]
> Bu makale, App Service Environment v1 hakkındadır. Uygulama Hizmet Ortamı'nın kullanımı daha kolay olan ve daha güçlü altyapıda çalışan daha yeni bir sürümü vardır. Yeni sürüm hakkında daha fazla bilgi edinmek [için Uygulama Hizmet Ortamına Giriş](intro.md)ile başlayın.
> 

### <a name="overview"></a>Genel Bakış
Uygulama Hizmet Ortamı (ASE), Azure Uygulama Hizmeti'nin çok kiracılı pullarda bulunmayan gelişmiş bir yapılandırma özelliği sunan bir Premium hizmet seçeneğidir. ASE özelliği, Azure Uygulama Hizmetini müşterinin sanal ağına temel olarak dağıtır. App Service Environments tarafından sunulan özellikler hakkında daha iyi bir anlayış kazanmak için [Uygulama Hizmeti Ortamı][WhatisASE] nedir belgelerini okuyun.

### <a name="before-you-create-your-ase"></a>ASE'nizi oluşturmadan önce
Değiştiremeyeceğiniz şeylerin farkında olmak önemlidir. Oluşturulduktan sonra ASE hakkında değiştiremezsiniz bu yönleri şunlardır:

* Konum
* Abonelik
* Kaynak Grubu
* Kullanılan VNet
* Kullanılan subnet 
* Alt ağ boyutu

Bir VNet seçerken ve bir alt ağ belirtirken, gelecekteki herhangi bir büyümeyi karşılayacak kadar büyük olduğundan emin olun. 

### <a name="creating-an-app-service-environment-v1"></a>Bir Uygulama Hizmet Ortamı v1 oluşturma
Bir Uygulama Hizmet Ortamı v1 oluşturmak için, ***Uygulama Hizmet Ortamı v1***için Azure Marketi'nde arama yapabilir veya kaynak -> **Web + Mobil** -> **Uygulama Hizmet Ortamı** **Oluştur'dan**geçebilirsiniz. ASEv1 oluşturmak için:

1. ASE'nizin adını sağlayın. ASE için belirttiğiniz ad, ASE'de oluşturulan uygulamalar için kullanılacaktır. ASE adı appsvcenvdemo ise, alt etki alanı adı olacaktır: *appsvcenvdemo.p.azurewebsites.net*. Böylece *mytestapp*adlı bir uygulama oluşturduysanız, *mytestapp.appsvcenvdemo.p.azurewebsites.net*adresinde olacaktır. ASE'niz adına beyaz boşluk kullanamazsınız. Adda büyük harf karakterleri kullanırsanız, etki alanı adı bu adın toplam küçük harfli sürümü olacaktır. Bir ILB kullanıyorsanız, ASE adınız alt etki alanınızda kullanılmaz, bunun yerine ASE oluşturma sırasında açıkça belirtilir.
   
    ![][1]
2. Aboneliğinizi seçin. ASE'niz için kullandığınız abonelik, o ASE'de oluşturduğunuz tüm uygulamalar için de geçerli olacaktır. ASE'nizi başka bir abonelikteki bir VNet'e yerleştiremezsiniz.
3. Yeni bir kaynak grubu seçin veya belirtin. ASE'niz için kullanılan kaynak grubu, VNet'iniz için kullanılanla aynı olmalıdır. Önceden varolan bir VNet seçerseniz, ASE'nizin kaynak grubu seçimi VNet'inizin seçimini yansıtacak şekilde güncelleştirilir.
   
    ![][2]
4. Sanal Ağ ve Konum seçimlerinizi yapın. Yeni bir VNet oluşturmayı veya önceden varolan bir VNet seçebilirsiniz. Yeni bir VNet seçerseniz, bir ad ve konum belirtebilirsiniz. Yeni VNet adres aralığı 192.168.250.0/23 ve 192.168.250.0/24 olarak tanımlanan **varsayılan** adlı bir alt net olacaktır. Ayrıca, önceden varolan bir Klasik veya Kaynak Yöneticisi VNet'i de seçebilirsiniz. VIP Türü seçimi, ASE'nize internetten (Harici) doğrudan erişilip erişilemeyebileceğini veya Dahili Yük Dengeleyicisi (ILB) kullanıp kullanmayabileceğini belirler. Onlar hakkında daha fazla bilgi edinmek için [Bir Uygulama Hizmet Ortamı ile Bir İç Yük Dengeleyici kullanma][ILBASE]okuyun. VIP harici türünü seçerseniz, ipssl amacıyla sistemin kaç harici IP adresi yle oluşturulduğunu seçebilirsiniz. Dahili'yi seçerseniz, ASE'nizin kullanacağı alt etki alanını belirtmeniz gerekir. GENEL ADRES Aralıkları *veya* RFC1918 adres alanları (örn. özel adresler) *kullanan* sanal ağlara dağıtılabilir. Genel adres aralığına sahip bir sanal ağ kullanmak için VNet'i önceden oluşturmanız gerekir. Önceden varolan bir VNet seçtiğinizde, ASE oluşturma sırasında yeni bir alt ağ oluşturmanız gerekir. **Portalda önceden oluşturulmuş bir alt ağ kullanamazsınız. Kaynak yöneticisi şablonu kullanarak ASE'nizi oluşturursanız, önceden varolan bir alt ağiçeren bir ASE oluşturabilirsiniz.** Şablondan bir ASE oluşturmak için buradaki bilgileri kullanın, [şablondan bir Uygulama Hizmet Ortamı oluşturma][ILBAseTemplate] ve burada [şablondan bir ILB Uygulama Hizmet Ortamı oluşturma.][ASEfromTemplate]

### <a name="details"></a>Ayrıntılar
2 Ön Uç ve 2 İşçi ile bir ASE oluşturulur. Ön Uçlar, HTTP/HTTPS bitiş noktaları gibi davranır ve uygulamalarınızı barındıran roller olan Çalışanlara trafik gönderir. ASE oluşturulduktan sonra miktarı ayarlayabilir ve hatta bu kaynak havuzlarında otomatik ölçeklendirme kuralları ayarlayabilirsiniz. Bir Uygulama Hizmet Ortamının manuel ölçekleme, yönetimi ve izlenmesi hakkında daha fazla bilgi için buraya gidin: [Uygulama Hizmet Ortamı nasıl yapılandırılabilen][ASEConfig] 

ASE tarafından kullanılan alt ağda yalnızca bir ASE bulunabilir. Alt ağ ASE dışında hiçbir şey için kullanılamaz

### <a name="after-app-service-environment-v1-creation"></a>Uygulama Hizmet Ortamı v1 oluşturma dan sonra
ASE oluşturulduktan sonra şunları ayarlayabilirsiniz:

* Ön Uç Miktarı (minimum: 2)
* İşçi Miktarı (minimum: 2)
* IP SSL için kullanılabilir IP adresi miktarı
* Ön Uçlar veya İşçiler tarafından kullanılan kaynak boyutlarını hesaplama (Ön Uç minimum boyutu P2'dir)

Uygulama Hizmet Ortamlarının manuel ölçekleme, yönetimi ve izlenmesi hakkında daha fazla ayrıntı vardır: [Uygulama Hizmet Ortamı nasıl yapılandırılır?][ASEConfig] 

Otomatik ölçeklendirme hakkında bilgi için burada bir kılavuz var: [Bir Uygulama Servis Ortamı için otomatik ölçeklendirme nasıl yapılandırılır?][ASEAutoscale]

Veritabanı ve depolama gibi özelleştirme için kullanılamayan ek bağımlılıklar vardır. Bunlar Azure tarafından işlenir ve sistemle birlikte gelir. Sistem depolama, tüm Uygulama Hizmeti Ortamı için 500 GB'a kadar destek sağlar ve veritabanı sistemin ölçeğine göre gerektiğinde Azure tarafından ayarlanır.

## <a name="getting-started"></a>Başlarken
App Service Environment v1'e başlamak [için][WhatisASE] bkz.

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[ASEConfig]: app-service-web-configure-an-app-service-environment.md
[AppServicePricing]: https://azure.microsoft.com/pricing/details/app-service/ 
[ASEAutoscale]: app-service-environment-auto-scale.md
[ILBASE]: app-service-environment-with-internal-load-balancer.md
[ILBAseTemplate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: app-service-app-service-environment-create-ilb-ase-resourcemanager.md
