---
title: Hizmet Sağlığı genel bakış | Microsoft Dokümanlar
description: Azure uygulamalarınızın mevcut ve gelecekteki Azure hizmet sorunları ve bakımdan nasıl etkilendiği hakkında kişiselleştirilmiş bilgiler.
ms.topic: conceptual
ms.date: 05/10/2019
ms.openlocfilehash: a1c6a10ab48125c030c9a35864f829438404ad97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898432"
---
# <a name="service-health-overview"></a>Hizmet Sağlığına genel bakış

Hizmet Durumu, Azure hizmetlerinizin sistem durumunu kullandığınız bölgelerde izleyen özelleştirilebilir bir pano sağlar. Bu panoda, devam eden hizmet sorunları, yaklaşan planlı bakım veya ilgili sağlık danışma önerileri gibi etkin olayları izleyebilirsiniz. Olaylar etkin olmadığında, 90 güne kadar sağlık geçmişinize yerleştirilirler. Son olarak, hizmet sorunları sizi etkilediğinde sizi proaktif olarak bilgilendiren hizmet durumu uyarıları oluşturmak ve yönetmek için Hizmet Durumu panosunu kullanabilirsiniz.

## <a name="service-health-events"></a>Hizmet Sağlığı Etkinlikleri

Hizmet Sağlığı kaynaklarınızı etkileyebilecek dört tür sağlık olayı izler:

1. **Hizmet sorunları** - Azure hizmetlerinde şu anda sizi etkileyen sorunlar. 
2. **Planlı bakım** - Gelecekte hizmetlerinizin kullanılabilirliğini etkileyebilecek yaklaşan bakım.  
3. **Sağlık danışma ları** - Azure hizmetlerinde dikkatinizi gerektiren değişiklikler. Bunlara örnek olarak Azure özelliklerinin ne zaman amortismana sokulduğu veya bir kullanım kotası aşması verilebilir.
4. **Güvenlik danışma ları (önizleme)** - Azure hizmetlerinizin kullanılabilirliğini etkileyebilecek güvenlikle ilgili bildirimler.

> [!NOTE]
> Hizmet Durumu olaylarını görüntülemek için, kullanıcılara bir abonelikte [Okuyucu rolü](../role-based-access-control/role-assignments-portal.md) verilmelidir.

## <a name="get-started-with-service-health"></a>Hizmet Sağlığı ile başlayın

Hizmet Durumu panonuzu başlatmak için portal panonuzdaki Hizmet Durumu döşemesini seçin. Daha önce döşemeyi kaldırdıysanız veya özel pano kullanıyorsanız, "Daha fazla hizmet" (panonuzda sol altta) Hizmet Durumu hizmetini arayın.

![Hizmet Sağlığı ile başlayın](./media/service-health-overview/azure-service-health-overview-1.png)

## <a name="see-current-issues-which-impact-your-services"></a>Hizmetlerinizi etkileyen güncel sorunları görün

**Hizmet sorunları** görünümü, Azure hizmetlerinde kaynaklarınızı etkileyen devam eden sorunları gösterir. Sorunun ne zaman başladığını ve hangi hizmetlerin ve bölgelerin etkilendiğini anlayabilirsiniz. Azure'un sorunu çözmek için ne yaptığını anlamak için en son güncelleştirmeyi de okuyabilirsiniz. 

![Hizmet sorununu yönetme](./media/service-health-overview/azure-service-health-overview-2.png)

Sorundan etkilenebilecek belirli kaynak listesini görmek için **Olası etki** sekmesini seçin. Ekibinizle paylaşmak için bu kaynakların bir CSV listesini indirebilirsiniz.

![Hizmet sorununu yönetme - Etki](./media/service-health-overview/azure-service-health-overview-4.png)

## <a name="get-links-and-downloadable-explanations"></a>Bağlantıları ve indirilebilir açıklamaları alın 

Sorun yönetim sisteminizde kullanmak için bir bağlantı alabilirsiniz. Azure portalına erişimi olmayan kişilerle paylaşmak için PDF ve bazen CSV dosyalarını indirebilirsiniz.   

![Hizmet sorununu yönetme - Sorun yönetimi](./media/service-health-overview/azure-service-health-overview-3.png)

## <a name="get-support-from-microsoft"></a>Microsoft'tan destek alın

Sorunun çözülmesinden sonra bile kaynağınız kötü durumda bırakılırsa desteğe başvurun.  Sayfanın sağındaki destek bağlantılarını kullanın.  

## <a name="pin-a-personalized-health-map-to-your-dashboard"></a>Kişiselleştirilmiş bir sağlık haritasını panonuza sabitleme

İş açısından kritik aboneliklerinizi, bölgelerinizi ve kaynak türlerinizi göstermek için Hizmet Durumu'na Filtre Uygulayın. Filtreyi kaydedin ve sistem durumunu gösteren kişiselleştirilmiş dünya haritasını portal panonuza sabitleyin. 

![Kişiselleştirilmiş sağlık haritasını filtrele](./media/service-health-overview/azure-service-health-overview-6a.png)

![Kişiselleştirilmiş bir sağlık haritasını sabitleme](./media/service-health-overview/azure-service-health-overview-6b.png)

## <a name="configure-service-health-alerts"></a>Hizmet durumu uyarılarını yapılandırma

Service Health, iş açısından kritik kaynaklarınız etkilendiğinde e-postalar, kısa mesajlar ve web hook bildirimleri aracılığıyla sizi uyarmak için Azure Monitor ile tümleşir. Uygun hizmet durumu olayı için bir etkinlik günlüğü uyarısı ayarlayın. Eylem Grupları'nı kullanarak bu uyarıyı kuruluşunuzdaki uygun kişilere yönlendirin. Daha fazla bilgi için Hizmet [Durumu Uyarılarını Yapılandır](../azure-monitor/platform/alerts-activity-log-service-notifications.md)

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2OaXt]

## <a name="next-steps"></a>Sonraki adımlar

Sağlık sorunları hakkında bilgilendirilmeniz için uyarılar ayarlayın. Daha fazla bilgi için Azure [Hizmet Durumu Uyarıları'nı ayarlamaya yönelik en iyi uygulamalara](https://www.youtube.com/watch?v=k5d5ca8K6tc&list=PLLasX02E8BPBBSqygdRvlTnHfp1POwE8K&index=6&t=0s)bakın. 
