---
title: Azure Güvenlik Merkezi'nde ağ kaynaklarınızı koruma
description: Bu belge, Azure ağ kaynaklarınızı korumanıza ve güvenlik ilkeleriyle uyumlu kalmanıza yardımcı olan Azure Güvenlik Merkezi'ndeki önerilere yönelikdir.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 96c55a02-afd6-478b-9c1f-039528f3dea0
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/05/2019
ms.author: memildin
ms.openlocfilehash: 792b95b120f67afcd360730acbd783a3071388b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77431494"
---
# <a name="protect-your-network-resources"></a>Ağ kaynaklarınızı koruyun
Azure Güvenlik Merkezi, ağ güvenliği en iyi uygulamaları için Azure kaynaklarınızın güvenlik durumunu sürekli olarak analiz eder. Güvenlik Merkezi olası güvenlik açıklarını tanımladığında, kaynaklarınızı sertleştirmek ve korumak için gerekli denetimleri yapılandırma sürecinde size rehberlik eden öneriler oluşturur.

Bu makalede, Güvenlik Merkezi'nin kaynak güvenliği bölümünün **Ağ** sayfası açıklanmaktadır.

Ağ için önerilerin tam listesi için [Ağ önerilerine](recommendations-reference.md#recs-network)bakın.

Bu makalede, ağ güvenliği açısından Azure kaynaklarınız için geçerli olan öneriler ele alılmıştır. Ağ önerileri, yeni nesil güvenlik duvarları, Ağ Güvenlik Grupları, JIT VM erişimi, aşırı izin veren gelen trafik kuralları ve daha fazlasını merkeze almaktadır. Ağ önerileri ve düzeltme eylemlerinin listesi için Azure [Güvenlik Merkezi'nde güvenlik önerilerini yönetme bölümüne](security-center-recommendations.md)bakın.

> [!NOTE]
> **Ağ** sayfası, ağ açısından Azure kaynak sağlığınıza derinlemesine dalmanızı sağlar. Ağ haritası ve Uyarlanabilir Ağ Denetimleri yalnızca Azure Güvenlik Merkezi standart katmanı için kullanılabilir. [Boş katmanı kullanıyorsanız, **eski ağ** ları görüntülemek için düğmeyi tıklatabilir ve ağ kaynak önerileri alabilirsiniz.](#legacy-networking)
>

**Ağ** ağı kaynaklarınızın durumu hakkında daha fazla bilgi almak için, ağ sayfası derinlemesine dalıp yapabileceğiniz bölümlere genel bir bakış sağlar:

- Ağ haritası (yalnızca Azure Güvenlik Merkezi Standart katmanı)
- Uyarlamalı Ağ Sağlamlaştırma
- Ağ güvenliği önerileri.
- Eski **Ağ** bıçağı (önceki ağ bıçak) 
 
[![Ağ bölmesi](./media/security-center-network-recommendations/networking-pane.png)](./media/security-center-network-recommendations/networking-pane.png#lightbox)

## <a name="network-map"></a>Ağ haritası
Etkileşimli ağ haritası, ağ kaynaklarınızı sertleştirmeniz için öneriler ve öngörüler sunan güvenlik bindirmeleriyle birlikte grafik bir görünüm sağlar. Haritayı kullanarak Azure iş yüklerinizin ağ topolojisini, sanal makineleriniz ve alt ağlarınız arasındaki bağlantıları ve haritadan belirli kaynaklara ve bu kaynaklara ilişkin önerilere kadar bilgi edinebilme yeteneğini görebilirsiniz.

Ağ haritasını açmak için:

1. Güvenlik Merkezi'nde, Kaynak Güvenlik Hijyeni altında **Ağ'ı**seçin.
2. **Ağ haritası** altında **Bkz. topolojiyi görün.**
 
Topoloji haritasının varsayılan görünümü görüntüler:

- Azure'da seçtiğiniz abonelikler. Harita birden çok aboneliği destekler.
- Kaynak Yöneticisi kaynak türünün VM'leri, alt ağları ve VNet'leri (Klasik Azure kaynakları desteklenmez)
- Emsalli VNets
- Yalnızca yüksek veya orta öneme sahip [ağ önerileri](security-center-recommendations.md) olan kaynaklar  
- İnternete bakan kaynaklar
- Harita, Azure'da seçtiğiniz abonelikler için optimize edecektir. Seçiminizi değiştirirseniz, harita yeni ayarlarınıza göre yeniden hesaplanır ve yeniden optimize edilir.  

[![Ağ topoloji haritası](./media/security-center-network-recommendations/network-map-info.png)](./media/security-center-network-recommendations/network-map-info.png#lightbox)

## <a name="understanding-the-network-map"></a>Ağ haritasını anlama

Ağ haritası, Azure kaynaklarınızı **Topoloji** görünümünde ve **Trafik** görünümünde gösterebilir. 

### <a name="the-topology-view"></a>Topoloji görünümü

Ağ haritasının **Topoloji** görünümünde, ağ kaynaklarınız hakkında aşağıdaki öngörüleri görüntüleyebilirsiniz:

- İç dairede, seçtiğiniz abonelikler içindeki tüm Vnet'leri görebilirsiniz, bir sonraki daire tüm alt ağlar, dış daire tüm sanal makinelerdir.
- Haritadaki kaynakları bağlayan satırlar, hangi kaynakların birbiriyle ilişkili olduğunu ve Azure ağınızın nasıl yapılandırılacağını bilmenizi sağlar. 
- Güvenlik Merkezi'nden hangi kaynakların açık önerileri ne kadar önemli olduğunu hızlı bir şekilde görmek için önem göstergelerini kullanın.
- Bunları ayrıntılı olarak inceleyebilir ve bu kaynağın ayrıntılarını ve önerilerini doğrudan ve Ağ haritası bağlamında görüntülemek için kaynaklardan herhangi birini tıklatabilirsiniz.  
- Haritada çok fazla kaynak görüntüleniyorsa, Azure Güvenlik Merkezi kaynaklarınızı akıllı kümelemek için özel algoritmasını kullanır, en kritik durumdaki kaynakları vurgular ve en yüksek önem derecesi önerilerine sahip olur. 

Harita etkileşimli ve dinamik olduğundan, her düğüm tıklanabilir ve görünüm filtrelere göre değişebilir:

1. Üstteki filtreleri kullanarak ağ haritasında gördüklerinizi değiştirebilirsiniz. Haritayı aşağıdakilere göre odaklayabilirsiniz:

   -  **Güvenlik durumu**: Azure kaynaklarınızın Önem Derecesine (Yüksek, Orta, Düşük) göre haritaya filtre uygulayabilirsiniz.
   - **Öneriler**: Bu kaynaklarda hangi önerilerin etkin olduğuna bağlı olarak hangi kaynakların görüntüleneceğini seçebilirsiniz. Örneğin, yalnızca Güvenlik Merkezi'nin Ağ Güvenlik Grupları'nı etkinleştirmenizi önerdiği kaynakları görüntüleyebilirsiniz.
   - **Ağ bölgeleri**: Varsayılan olarak, harita yalnızca Internet'e bakan kaynakları görüntüler, dahili VM'leri de seçebilirsiniz.
 
2. Haritayı varsayılan durumuna döndürmek için istediğiniz zaman sol üst köşede **Sıfırla'yı** tıklatabilirsiniz.

Bir kaynağa sondaj yapmak için:

1. Haritada belirli bir kaynak seçtiğinizde, sağ bölme açılır ve varsa kaynak, bağlı güvenlik çözümleri ve kaynakla ilgili öneriler hakkında genel bilgiler verir. Seçtiğiniz her kaynak türü için aynı davranış türüdür. 
2. Haritadaki bir düğümün üzerinde gezinirken, abonelik, kaynak türü ve kaynak grubu da dahil olmak üzere kaynak la ilgili genel bilgileri görüntüleyebilirsiniz.
3. Araç ucunu yakınlaştırmak ve haritayı belirli bir düğüme yeniden odaklamak için bağlantıyı kullanın. 
4. Haritayı belirli bir düğümden uzaklaştırmak için uzaklaştırın.

### <a name="the-traffic-view"></a>Trafik görünümü

**Trafik** görünümü, kaynaklarınız arasındaki tüm olası trafiğin bir haritasını sağlar. Bu, hangi kaynakların kiminle iletişim kurabileceğini tanımlayan yapılandırdığınız tüm kuralların görsel bir eşlenimi sağlar. Bu, ağ güvenlik gruplarının varolan yapılandırmasını görmenizi ve iş yüklerinizdeki olası riskli yapılandırmaları hızla belirlemenizi sağlar.

### <a name="uncover-unwanted-connections"></a>İstenmeyen bağlantıları ortaya çıkarmak

Bu görünümün gücü, bu izin verilen bağlantıları varolan güvenlik açıklarıyla birlikte size gösterebilme yeteneğidir, böylece kaynaklarınız üzerinde gerekli sertleştirmeyi gerçekleştirmek için bu veri kesitini kullanabilirsiniz. 

Örneğin, farkında olmadığınız iki makinenin iletişim kurarak iş yüklerini ve alt ağlarını daha iyi yalıtmanızı sağlayabilirsiniz.

### <a name="investigate-resources"></a>Kaynakları araştırma

Bir kaynağa sondaj yapmak için:

1. Haritada belirli bir kaynak seçtiğinizde, sağ bölme açılır ve varsa kaynak, bağlı güvenlik çözümleri ve kaynakla ilgili öneriler hakkında genel bilgiler verir. Seçtiğiniz her kaynak türü için aynı davranış türüdür. 
2. Kaynaktaki olası giden ve gelen trafiğin listesini görmek için **Trafik'i** tıklatın - bu, kaynakla kimlerin iletişim kurabileceğini ve kimlerle iletişim kurabileceğini ve hangi protokoller ve bağlantı noktaları aracılığıyla iletişim kurabileceğinin kapsamlı bir listesidir. Örneğin, bir VM seçtiğinizde, iletişim kurabileceği tüm VM'ler gösterilir ve bir alt ağ seçtiğinizde, iletişim kurabileceği tüm alt ağlar gösterilir.

**Bu veriler, Ağ Güvenlik Gruplarının analizinin yanı sıra çapraz geçişlerini ve etkileşimlerini anlamak için birden çok kuralı analiz eden gelişmiş makine öğrenimi algoritmalarına dayanmaktadır.** 

[![Ağ trafik haritası](./media/security-center-network-recommendations/network-map-traffic.png)](./media/security-center-network-recommendations/network-map-traffic.png#lightbox)


## <a name="legacy-networking"></a>Eski ağ<a name ="legacy-networking"></a>

Güvenlik Merkezi Standart katmanınız yoksa, bu bölümde ücretsiz Ağ önerilerinasıl görüntüleneniz açıklanmaktadır.

Bu bilgilere erişmek için Ağ çubuğunda **eski ağ larını görüntüle'yi**tıklatın. 

[![Eski Ağ](./media/security-center-network-recommendations/legacy-networking.png)](./media/security-center-network-recommendations/legacy-networking.png#lightbox)

### <a name="internet-facing-endpoints-section"></a>İnternet'e yönelik uç noktalar bölümü
**Internet'e bakan uç noktalar** bölümünde, şu anda Internet'e bakan uç nokta ve durumuyla yapılandırılan sanal makineleri görebilirsiniz.

Bu tabloda bitiş noktası adı, IP adresine bakan Internet ve ağ güvenlik grubunun geçerli önem durumu ve NGFW önerileri vardır. Tablo önem derecesine göre sıralanır.

### <a name="networking-topology-section"></a>Ağ topolojisi bölümü
**Ağ topolojisi** bölümü kaynakların hiyerarşik bir görünümüne sahiptir.

Bu tablo önem derecesine göre sıralanır (sanal makineler ve alt ağlar).

Bu topoloji görünümünde, birinci düzey Vnets görüntüler. İkinci alt ağları görüntüler ve üçüncü düzey bu alt ağlara ait sanal makineleri görüntüler. Sağ sütun, bu kaynaklar için ağ güvenlik grubu önerilerinin geçerli durumunu gösterir.

Üçüncü düzey, daha önce açıklanana benzer sanal makineleri görüntüler. Daha fazla bilgi edinmek veya gerekli güvenlik denetimini veya yapılandırmasını uygulamak için herhangi bir kaynağı tıklatabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Diğer Azure kaynak türleri için geçerli olan öneriler hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [Azure Güvenlik Merkezi'nde makinelerinizi ve uygulamalarınızı koruma](security-center-virtual-machine-protection.md)
* [Azure SQL hizmetinizi Azure Güvenlik Merkezi'nde koruma](security-center-sql-service-recommendations.md)