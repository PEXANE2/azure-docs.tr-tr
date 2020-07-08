---
title: Azure Kaynak Durumu SSS
description: Azure Kaynak Durumu genel bakış
ms.topic: conceptual
ms.date: 01/29/2019
ms.openlocfilehash: 7459a29dca01dc186d75b4545f89068569975607
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80985040"
---
# <a name="azure-resource-health-faq"></a>Azure Kaynak Durumu SSS
Azure Kaynak Durumu hakkında sık sorulan soruların yanıtlarını öğrenin.

## <a name="what-is-azure-resource-health"></a>Azure Kaynak Durumu nedir?
Kaynak Durumu, bir Azure sorunu kaynaklarınızı etkilediğinde bunu tanılamanıza ve destek almanıza yardımcı olur. Kaynaklarınızın güncel ve geçmiş durumu hakkında bilgiler sağlar ve sorunları azaltmaya yardımcı olur. Kaynak Durumu, Azure hizmet sorunları ile ilgili yardıma ihtiyacınız olduğunda teknik destek sağlar.  

## <a name="what-is-the-resource-health-intended-for"></a>Kaynak Durumu amacı nedir?
Bir kaynakla ilgili bir sorun algılandığında Kaynak Durumu, kök nedeni tanılamanıza yardımcı olabilir. Azure hizmet sorunları hakkında daha fazla yardıma ihtiyacınız olduğunda sorunu ve teknik desteği azaltmaya yönelik yardım sağlar.

## <a name="what-health-checks-are-performed-by-resource-health"></a>Kaynak Durumu hangi sistem durumu denetimleri gerçekleştirilir?
Kaynak sistem durumu, [kaynak türünü](resource-health-checks-resource-types.md)temel alan çeşitli denetimler gerçekleştirir. Bu denetimler, üç tür sorunu uygulamak için tasarlanmıştır: 
- Planlanmamış olaylar, örneğin beklenmeyen bir ana bilgisayar yeniden başlatması
- Zamanlanan ana bilgisayar işletim sistemi güncelleştirmeleri gibi planlı olaylar
- Kullanıcı eylemleri tarafından tetiklenen olaylar (örneğin, bir sanal makineyi yeniden başlatma)

## <a name="what-does-each-of-the-health-status-mean"></a>Sistem durumunun her biri ne anlama geliyor?
Üç farklı durum durumu vardır:
- Kullanılabilir: Azure platformunda bu kaynağı etkileyebilecek bilinen bir sorun yok
- Kullanılamıyor: kaynak durumu, kaynağı etkileyen sorunlar algıladı
- Bilinmiyor: kaynak sistem durumu, kendisiyle ilgili bilgi almayı durdurduğundan kaynağın sistem durumunu belirleyemiyor. 

## <a name="what-does-the-unknown-status-mean-is-something-wrong-with-my-resource"></a>Bilinmeyen durum ne anlama geliyor? Kaynaktaki bir sorun mu var?
Kaynak Durumu belirli bir kaynakla ilgili bilgi almayı kestiğinde sistem durumu bilinmiyor olarak ayarlanır. Bu durum kaynağın durumunun kesin bir göstergesi olmasa da, sorun yaşadığınızda bir Azure sorunu olduğunu gösterebilir.

## <a name="how-can-i-get-help-for-a-resource-that-is-unavailable"></a>Kullanılamayan bir kaynak için nasıl yardım alabilirim?
Kaynak Durumu dikey penceresinden bir destek isteği gönderebilirsiniz. Platform olayları nedeniyle kaynak kullanılamadığında bir isteği açmak için Microsoft ile destek sözleşmenize gerek yoktur.

## <a name="does-resource-health-differentiate-between-unavailability-caused-by-platform-problems-versus-something-i-did"></a>Platform sorunlarından kaynaklanan bir şeye karşı neden olan kullanım dışı Kaynak Durumu birbirinden ayırt edilsin mi?
Evet, bir kaynak kullanılamadığında Kaynak Durumu, bu kategorilerden birindeki kök nedeni tanımlar: 
-   Kullanıcı tarafından başlatılan eylem
-   Planlanmış olay 
-   Planlanmamış olay

Portalda, Kullanıcı tarafından başlatılan eylemler mavi bir bildirim simgesiyle gösterilir, planlı ve plansız olaylar da kırmızı bir uyarı simgesi kullanılarak gösterilir. [Kaynak durumu genel bakışta](Resource-health-overview.md)daha fazla ayrıntı sağlanır.  

## <a name="can-i-integrate-resource-health-with-my-monitoring-tools"></a>Kaynak Durumu İzleme Araçlarıma tümleştirebilir miyim?
Kaynak durumu, etkinlik günlüğü tabanlı uyarılar için [Önizleme desteğine](resource-health-alert-arm-template-guide.md) sahiptir. Etkinlik günlüğü uyarıları, kullanıcılara bir uyarının tetiklendiğini bildirmek için [eylem gruplarını](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) kullanır. Eylem grupları, e-posta, SMS, Web kancası ve ıTSM eylemleri gibi çeşitli bildirim kanallarını destekler.

## <a name="where-do-i-find-resource-health"></a>Kaynak Durumu nerede bulabilirim?
Azure portal oturum açtıktan sonra, Kaynak Durumu erişmek için kullanabileceğiniz birden çok yol vardır:
- Kaynağına gidin. Sol taraftaki gezinmede **kaynak durumu** ' nu seçin.
- Azure hizmet durumu dikey penceresine gidin.  Sol taraftaki gezinmede **kaynak durumu**' nu seçin.
- Portalın sağ üst köşesinde bulunan soru işaretini seçip **Yardım + Destek**' i seçerek **Yardım + Destek** dikey penceresini açın. Dikey pencere açıldıktan sonra **kaynak sistem durumu** ' nu seçin.

Kaynaklarınızın sağlık durumu hakkında bilgi almak için Kaynak Durumu API 'sini de kullanabilirsiniz.

## <a name="is-resource-health-available-for-all-resource-types"></a>Tüm kaynak türleri için Kaynak Durumu kullanılabilir mi?
Kaynak Durumu ile desteklenen durum denetimleri ve kaynak türleri listesi [burada](resource-health-checks-resource-types.md)bulunabilir.

## <a name="what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not"></a>Kaynaklarım kullanılabilir olarak gösterildiğimde ne yapmam gerekir? "
Bir kaynağın sistem durumunu denetlerken, sistem durumu ' nun altında **yanlış sistem durumunu raporla**' ya tıklayabilirsiniz. Raporu göndermeden önce, geçerli sağlık durumunun yanlış olduğunu düşünmenize ilişkin ek ayrıntılar sağlama seçeneğiniz vardır.

## <a name="is-resource-health-available-for-all-azure-regions"></a>Tüm Azure bölgeleri için Kaynak Durumu kullanılabilir mi? 
Kaynak sistem durumu tüm Azure geos 'ta kullanılabilir.

## <a name="how-is-resource-health-different-from-azure-status-or-the-service-health-dashboard"></a>Azure durumu veya hizmet durumu panosundan Kaynak Durumu nasıl farklıdır?
Kaynak Durumu tarafından belirtilen bilgiler, Azure durumu veya hizmet durumu panosu tarafından sağlandıklardan daha özgüdür.

[Azure durumu](https://status.azure.com) ve hizmet durumu panosu, çok sayıda müşteriyi (örneğin bir Azure bölgesini) etkileyen hizmet sorunları hakkında bilgi veren kaynak durumu, yalnızca belirli kaynakla ilgili daha ayrıntılı olaylar sunar. Örneğin, bir ana bilgisayar beklenmedik şekilde yeniden başlatıldığında Kaynak Durumu yalnızca sanal makineleri bu konakta çalışan müşterileri uyarır.

Kaynaklarınızı etkileyen olayların görünürlüğünü, Kaynak Durumu Ayrıca hizmet durumu panosunda yayınlanan olayları da doldurduğuna dikkat etmeniz önemlidir.

## <a name="do-i-need-to-activate-resource-health-for-each-resource"></a>Her kaynak için Kaynak Durumu etkinleştirmem gerekir mi?
Hayır, Kaynak Durumu aracılığıyla kullanılabilen tüm kaynak türleri için sistem durumu bilgileri kullanılabilir. 

## <a name="do-we-need-to-enable-resource-health-for-my-organization"></a>Kuruluşum için Kaynak Durumu etkinleştirmemiz gerekiyor mu?
Hayır.  Azure Kaynak Durumu, kurulum gereksinimleri olmadan Azure portal içinde erişilebilir.

## <a name="is-resource-health-available-free-of-charge"></a>Kaynak Durumu ücretsiz olarak kullanılabilir mi?
Evet.  Azure Kaynak Durumu ücretsizdir.

## <a name="what-are-the-recommendations-that-resource-health-provides"></a>Kaynak Durumu sağladığı öneriler nelerdir?
Kaynak Durumu sistem durumu temelinde, sorun gidermeye harcadığınız süreyi azaltma amacını içeren öneriler sağlar. Kullanılabilir kaynaklar için, müşterilerin karşılaştığı en yaygın sorunların nasıl çözüğiyle ilgili öneriler vardır. Kaynak, Azure plansız bir olay nedeniyle kullanılamıyorsa, odak kurtarma işleminden sonra ve sonrasında size yardımcı olur. 

## <a name="next-steps"></a>Sonraki adımlar

Kaynak Durumu hakkında daha fazla bilgi edinin:
-  [Azure Kaynak Durumu genel bakış](Resource-health-overview.md)
-  [Azure Kaynak Durumu aracılığıyla kullanılabilen kaynak türleri ve durum denetimleri](resource-health-checks-resource-types.md)
