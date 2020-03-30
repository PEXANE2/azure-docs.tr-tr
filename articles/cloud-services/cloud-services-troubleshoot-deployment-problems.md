---
title: Sorun giderme bulut hizmeti dağıtım sorunları | Microsoft Dokümanlar
description: Azure'a bir bulut hizmeti dağıtırken karşılaşabileceğiniz birkaç yaygın sorun vardır. Bu makalede, bazılarına çözümler sağlar.
services: cloud-services
documentationcenter: ''
author: simonxjx
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: a18ae415-0d1c-4bc4-ab6c-c1ddea02c870
ms.service: cloud-services
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: ccb08f853ae0f941dd5f9c0eca8c77f0f650905a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71122742"
---
# <a name="troubleshoot-cloud-service-deployment-problems"></a>Bulut hizmeti dağıtım sorunlarını giderme sorunları
Bir bulut hizmeti uygulama paketini Azure'a dağıttığınızda, Azure portalındaki **Özellikler** bölmesinden dağıtım hakkında bilgi edinebilirsiniz. Bulut hizmetiyle ilgili sorunları gidermenize yardımcı olmak için bu bölmedeki ayrıntıları kullanabilir ve yeni bir destek isteği açarken bu bilgileri Azure Desteği'ne sağlayabilirsiniz.

**Özellikler** bölmesini aşağıdaki gibi bulabilirsiniz:

* Azure portalında bulut hizmetinizin dağıtımını tıklatın, **Tüm ayarlar'ı**tıklatın ve ardından **Özellikler'i**tıklatın.

> [!NOTE]
> **Bölmenin** sağ üst köşesindeki simgeyi tıklatarak Özellikler bölmesinin içeriğini panoya kopyalayabilirsiniz.
>
>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>Sorun: Web siteme erişemiyorum, ancak dağıtımım başlatıldı ve tüm rol örnekleri hazır
Portalda gösterilen web sitesi URL bağlantısı bağlantı noktasını içermez. Web siteleri için varsayılan bağlantı noktası 80'dir. Uygulamanız farklı bir bağlantı noktasında çalışacak şekilde yapılandırıldıysa, web sitesine erişirken URL'ye doğru bağlantı noktası numarasını eklemeniz gerekir.

1. Azure portalında bulut hizmetinizin dağıtımını tıklatın.
2. Azure portalının **Özellikler** bölmesinde, rol örnekleri için bağlantı noktalarını denetleyin **(Giriş Uç Noktaları**altında).
3. Bağlantı noktası 80 değilse, uygulamaya erişdiğinizde URL'ye doğru bağlantı noktası değerini ekleyin. Varsayılan olmayan bir bağlantı noktası belirtmek için, URL'yi yazın ve ardından bir üst üste (:), ardından bağlantı noktası numarası, boşluk olmadan.

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>Sorun: Rolüm bana bir şey yapmadan geri dönüştürülmüş
Hizmet iyileması, Azure sorun düğümlerini algıladığında ve bu nedenle rol örneklerini yeni düğümlere taşırsa otomatik olarak gerçekleşir. Bu durumda, rol örneklerinizi otomatik olarak geri dönüştürünüzü görebilirsiniz. Hizmet iyileştirme oluştu olup olmadığını öğrenmek için:

1. Azure portalında bulut hizmetinizin dağıtımını tıklatın.
2. Azure portalının **Özellikler** bölmesinde, bilgileri gözden geçirin ve geri dönüşüm rollerini gözlemlediğiniz süre boyunca hizmet iyileştirmenin meydana gelip gelmediğini belirleyin.

Roller ayrıca, ev sahibi-os ve konuk-İşlet güncellemeleri sırasında ayda kabaca bir kez geri dönüşüm sağlar.  
Daha fazla bilgi için, [İşletim Sistemi Yükseltmeleri nedeniyle](https://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx) blog gönderisi Role Instance Restarts'a bakın

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>Sorun: VIP takas yapıp hata alamıyorum
Dağıtım güncelleştirmesi devam ediyorsa VIP değiştirmeye izin verilmez. Dağıtım güncelleştirmeleri, şu anda otomatik olarak oluşabilir:

* Yeni bir konuk işletim sistemi mevcuttur ve otomatik güncelleştirmeler için yapılandırılmıştır.
* Hizmet iyileşme oluşur.

Otomatik güncellemenin VIP takasını engelleyip engellemediğinizi öğrenmek için:

1. Azure portalında bulut hizmetinizin dağıtımını tıklatın.
2. Azure portalının **Özellikler** bölmesinde **Durum**değerine bakın. **Hazır**ise, o zaman VIP takas engelleyebilir son zamanlarda olup olmadığını görmek için **Son işlemi** kontrol edin.
3. Üretim dağıtımı için 1 ve 2 adımlarını yineleyin.
4. Otomatik güncelleştirme sürecindeyse, VIP takasını yapmadan önce tamamlanmasını bekleyin.

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>Sorun: Başlangıç, Başlatma, Meşgul ve Durduruldu arasında bir rol örneği döngü
Bu durum, uygulama kodunuz, paketiniz veya yapılandırma dosyanızla ilgili bir sorundan kaynaklanıyor olabilir. Bu durumda, durumun birkaç dakikada bir değiştiğini görebilmelisiniz ve Azure portalı **Geri Dönüşüm**, **Meşgul**veya **Başlatma**gibi bir şey söyleyebilir. Bu, rol örneğinin çalışmasını engelleyen uygulamada bir sorun olduğunu gösterir.

Bu sorunla ilgili sorun giderme hakkında daha fazla bilgi için Azure [PaaS Bilgi İşlem Diagnostik Verileri](https://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx) ve [rollerin geri dönüştürülmesine neden olan yaygın sorunlara](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md)bakın.

## <a name="problem-my-application-stopped-working"></a>Sorun: Uygulamam çalışmayı durdurdu
1. Azure portalında rol örneğini tıklatın.
2. Azure portalının **Özellikler** bölmesinde, sorununuzu çözmek için aşağıdaki koşulları göz önünde bulundurun:
   * Rol örneği yakın zamanda durdurulmuşsa **(İptal sayısının**değerini denetleyebilirsiniz), dağıtım güncelleniyor olabilir. Rol örneğinin kendi kendine çalışmaya devam edip etmeyişe devam edip etmeyişini bekleyin.
   * Rol örneği **Meşgulise,** [StatusCheck](/previous-versions/azure/reference/ee758135(v=azure.100)) olayının işlenir mi diye uygulama kodunuzu denetleyin. Bu olayı işleyen bazı kod eklemeniz veya düzeltmeniz gerekebilir.
   * [Azure PaaS İşlem Tanılama Verileri](https://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)blog gönderisindeki tanılama verilerini ve sorun giderme senaryolarını gözden geçirin.

> [!WARNING]
> Bulut hizmetinizi geri dönüştürürseniz, dağıtım özelliklerini sıfırlayarak özgün sorunun bilgilerini etkili bir şekilde silersiniz.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Bulut hizmetleri için daha fazla [sorun giderme makalelerini](https://docs.microsoft.com/azure/cloud-services/cloud-services-allocation-failures) görüntüleyin.

Azure PaaS bilgisayar tanılama verilerini kullanarak bulut hizmeti rol sorunlarını nasıl gidereceklerini öğrenmek için [Kevin Williamson'ın blog serisine](https://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)bakın.
