---
title: Azure Kaynak Durumu SSSS
description: Azure Kaynak Durumu'na Genel Bakış
ms.topic: conceptual
ms.date: 01/29/2019
ms.openlocfilehash: 7459a29dca01dc186d75b4545f89068569975607
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985040"
---
# <a name="azure-resource-health-faq"></a>Azure Kaynak Durumu SSSS
Azure Kaynak Durumu ile ilgili sık sorulan soruların yanıtlarını öğrenin.

## <a name="what-is-azure-resource-health"></a>Azure Kaynak Durumu nedir?
Kaynak Durumu, bir Azure sorunu kaynaklarınızı etkilediğinde bunu tanılamanıza ve destek almanıza yardımcı olur. Kaynaklarınızın güncel ve geçmiş durumu hakkında bilgiler sağlar ve sorunları azaltmaya yardımcı olur. Kaynak Durumu, Azure hizmet sorunları ile ilgili yardıma ihtiyacınız olduğunda teknik destek sağlar.  

## <a name="what-is-the-resource-health-intended-for"></a>Kaynak Durumu ne için tasarlanmıştır?
Kaynakla ilgili bir sorun algılandıktan sonra, Kaynak Durumu temel nedeni tanılamanıza yardımcı olabilir. Azure hizmet sorunları yla ilgili daha fazla yardıma ihtiyacınız olduğunda sorunu ve teknik desteği azaltmak için yardım sağlar.

## <a name="what-health-checks-are-performed-by-resource-health"></a>Kaynak Durumu tarafından hangi sistem durumu denetimleri gerçekleştirilir?
Kaynak [durumu, kaynak türüne](resource-health-checks-resource-types.md)göre çeşitli denetimler gerçekleştirir. Bu denetimler üç tür sorun uygulamak üzere tasarlanmıştır: 
- Planlanmamış olaylar, örneğin beklenmeyen bir ana bilgisayar yeniden başlatma
- Planlanmış ana bilgisayar işletim sistemi güncelleştirmeleri gibi planlanmış olaylar
- Kullanıcı eylemleri tarafından tetiklenen olaylar, örneğin bir kullanıcının sanal bir makineyi yeniden başlatması gibi

## <a name="what-does-each-of-the-health-status-mean"></a>Her bir sağlık durumu ne anlama geliyor?
Üç farklı sağlık durumu vardır:
- Kullanılabilir: Azure platformunda bu kaynağı etkileyebilecek bilinen herhangi bir sorun yok
- Kullanılamıyor: Kaynak durumu, kaynağı etkileyen sorunları algılamıştır
- Bilinmiyor: Kaynak durumu, bu konuda bilgi almayı bıraktığı için kaynağın durumunu belirleyemez. 

## <a name="what-does-the-unknown-status-mean-is-something-wrong-with-my-resource"></a>Bilinmeyen durum ne anlama geliyor? Kaynağımda bir sorun mu var?
Kaynak Durumu belirli bir kaynak hakkında bilgi almayı durdurduğunda sistem durumu bilinmiyor olarak ayarlanır. Bu durum kaynağın durumunun kesin bir göstergesi olmasa da, sorun yaşadığınız durumlarda bir Azure sorunu olduğunu gösterebilir.

## <a name="how-can-i-get-help-for-a-resource-that-is-unavailable"></a>Kullanılamayan bir kaynak için nasıl yardım alabilirim?
Kaynak Durumu bıçağından bir destek isteği gönderebilirsiniz. Kaynak kullanılamadığında, platform olayları nedeniyle bir isteği açmak için Microsoft ile bir destek anlaşmasına ihtiyacınız yoktur.

## <a name="does-resource-health-differentiate-between-unavailability-caused-by-platform-problems-versus-something-i-did"></a>Kaynak Durumu, platform sorunlarının neden olduğu kullanılamaması ile benim yaptığım bir şey arasında ayrım yapar mı?
Evet, bir kaynak kullanılamıyorsa, Kaynak Durumu bu kategorilerden birinde temel nedeni tanımlar: 
-   Kullanıcı başlatılan eylem
-   Planlanan olay 
-   Planlanmamış olay

Portalda, kullanıcı tarafından başlatılan eylemler mavi bir bildirim simgesi kullanılarak, planlı ve planlanmamış olaylar ise kırmızı uyarı simgesi kullanılarak gösterilir. Kaynak Durumu genel [bakışında](Resource-health-overview.md)daha fazla ayrıntı verilmiştir.  

## <a name="can-i-integrate-resource-health-with-my-monitoring-tools"></a>Kaynak Durumu'nun izleme araçlarımla bütünleşmesini sağlayabilir miyim?
Kaynak durumu, Etkinlik Günlüğü tabanlı uyarılar için [önizleme desteğine](resource-health-alert-arm-template-guide.md) sahiptir. Etkinlik Günlüğü uyarıları, kullanıcılara bir uyarının tetiklendiğini bildirmek için [eylem gruplarını](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) kullanır. Eylem grupları e-posta, SMS, webhook ve ITSM eylemleri gibi çeşitli bildirim kanallarını destekler.

## <a name="where-do-i-find-resource-health"></a>Kaynak Sağlığı'nı nerede bulabilirim?
Azure portalında oturum açtıktan sonra Kaynak Durumu'na erişmenin birden çok yolu vardır:
- Kaynağınıza gidin. Sol daki gezinmede **Kaynak sağlık**
- Azure Hizmet Durumu bıçağına gidin.  Sol daki gezinmede **Kaynak durumu'nu**seçin.
- Portalın sağ üst köşesindeki soru işaretini seçerek **yardım + Destek** bıçağını açın ve ardından Yardım + **Destek'i**seçin. Bıçak açıldıktan sonra **Kaynak sistem durumunu** seçin

Kaynak Durumu API'sini, kaynaklarınızın durumu hakkında bilgi almak için de kullanabilirsiniz.

## <a name="is-resource-health-available-for-all-resource-types"></a>Kaynak Durumu tüm kaynak türleri için kullanılabilir mi?
Kaynak Durumu ile desteklenen sistem durumu denetimleri ve kaynak türlerinin listesini [burada](resource-health-checks-resource-types.md)bulabilirsiniz.

## <a name="what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not"></a>Kaynağım mevcut görünüyorsa ama olmadığına inanıyorum."
Bir kaynağın durumunu kontrol ederken, sistem durumu durumunun hemen altında **Yanlış Sağlık Durumu Bildir'i**tıklatabilirsiniz. Raporu göndermeden önce, geçerli sağlık durumunun neden yanlış olduğuna inandığınız hakkında ek bilgi sağlama seçeneğiniz var.

## <a name="is-resource-health-available-for-all-azure-regions"></a>Kaynak Durumu tüm Azure bölgeleri için kullanılabilir mi? 
Kaynak durumu tüm Azure coğrafyalarında kullanılabilir.

## <a name="how-is-resource-health-different-from-azure-status-or-the-service-health-dashboard"></a>Kaynak Durumu'nun Azure durumundan veya Hizmet Durumu panosundan farkı nedir?
Kaynak Durumu tarafından sağlanan bilgiler, Azure durumu veya Hizmet Durumu panosu tarafından sağlananbilgilerden daha belirgindir.

Azure [durumu](https://status.azure.com) ve Hizmet Durumu panosu, geniş bir müşteri kümesini (örneğin bir Azure bölgesi) etkileyen hizmet sorunları hakkında sizi bilgilendirirken, Kaynak Durumu yalnızca belirli bir kaynakla alakalı daha ayrıntılı olaylar ortaya çıkarır. Örneğin, bir ana bilgisayar beklenmeyen bir şekilde yeniden başlatılırsa, Kaynak Durumu yalnızca sanal makineleri bu ana bilgisayarda çalışan müşterileri uyarır.

Kaynaklarınızı etkileyen olayların tam görünürlüğünü sağlamak için, Kaynak Sağlığı'nın Hizmet Durumu panosunda yayınlanan olayları da ortaya çıkardığını fark etmek önemlidir.

## <a name="do-i-need-to-activate-resource-health-for-each-resource"></a>Her kaynak için Kaynak Durumu'nun etkinleştirilmesi gerekiyor mu?
Hayır, sistem durumu bilgileri Kaynak Durumu aracılığıyla kullanılabilen tüm kaynak türleri için kullanılabilir. 

## <a name="do-we-need-to-enable-resource-health-for-my-organization"></a>Kuruluşum için Kaynak Sağlığı'nı etkinleştirmemiz gerekiyor mu?
Hayır.  Azure Kaynak Durumu'na azure portalında herhangi bir kurulum gereksinimi olmadan erişilebilir.

## <a name="is-resource-health-available-free-of-charge"></a>Kaynak Sağlığı ücretsiz mi?
Evet.  Azure Kaynak Durumu ücretsizdir.

## <a name="what-are-the-recommendations-that-resource-health-provides"></a>Kaynak Durumu'nun sağladığı öneriler nelerdir?
Kaynak Durumu, sistem durumu durumuna bağlı olarak, sorun giderme için harcadığınız zamanı azaltmak amacıyla size öneriler sunar. Kullanılabilir kaynaklar için öneriler, müşterilerin karşılaştığı en yaygın sorunları nasıl çözeceğiniz üzerinde odaklanır. Kaynak, planlanmamış bir olay nedeniyle kullanılamıyorsa, kurtarma işlemi sırasında ve sonrasında size yardımcı olmak üzerinde durulacaktır. 

## <a name="next-steps"></a>Sonraki adımlar

Kaynak Durumu hakkında daha fazla bilgi edinin:
-  [Azure Kaynak Durumu'na genel bakış](Resource-health-overview.md)
-  [Azure Kaynak Durumu aracılığıyla kullanılabilen kaynak türleri ve durum denetimleri](resource-health-checks-resource-types.md)
