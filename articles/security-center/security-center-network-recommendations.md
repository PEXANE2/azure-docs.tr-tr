---
title: Azure Güvenlik Merkezi 'nde ağ kaynaklarınızı koruma
description: Bu belge, Azure Güvenlik Merkezi 'nde Azure ağ kaynaklarınızı korumanıza ve güvenlik ilkeleriyle uyumlu kalmanıza yardımcı olan önerilere yöneliktir.
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
ms.openlocfilehash: 868470292fbacd71e1eb2d39de7e3a9c5cf6900e
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90883902"
---
# <a name="protect-your-network-resources"></a>Ağ kaynaklarınızı koruyun
Azure Güvenlik Merkezi, ağ güvenliği en iyi uygulamaları için Azure kaynaklarınızın güvenlik durumunu sürekli olarak analiz eder. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, kaynaklarınızı korumak ve korumak için gerekli denetimleri yapılandırma sürecinde size kılavuzluk eden öneriler oluşturur.

Ağ önerilerinin tam listesi için bkz. [ağ önerileri](recommendations-reference.md#recs-network).

Bu makalede bir ağ güvenliği perspektifinden Azure kaynaklarınıza uygulanan öneriler ele alınmaktadır. Sonraki nesil güvenlik duvarları, ağ güvenlik grupları, JıT VM erişimi, aşırı izin veren gelen trafik kuralları ve daha fazlası etrafında ağ önerisi merkezi. Ağ önerileri ve düzeltme eylemlerinin bir listesi için bkz. [Azure Güvenlik Merkezi 'nde güvenlik önerilerini yönetme](security-center-recommendations.md).

Güvenlik Merkezi 'nin **ağ** özellikleri şunlardır: 

- Ağ eşlemesi (Azure Defender gerektirir)
- [Uyarlamalı ağ sağlamlaştırma](security-center-adaptive-network-hardening.md) (Azure Defender gerekir)
- Ağ güvenlik önerileri
 
## <a name="view-your-networking-resources-and-their-recommendations"></a>Ağ kaynaklarınızı ve bunların önerilerini görüntüleyin

[Varlık envanteri sayfasında](asset-inventory.md), araştırmak istediğiniz ağ kaynaklarını seçmek için kaynak türü filtresini kullanın:

:::image type="content" source="./media/security-center-network-recommendations/network-filters-inventory.png" alt-text="Varlık envanteri ağ kaynağı türleri" lightbox="./media/security-center-network-recommendations/network-filters-inventory.png":::


## <a name="network-map"></a>Ağ eşlemesi

Etkileşimli ağ eşlemesi, ağ kaynaklarınızı sağlamlaştırma konusunda öneriler ve Öngörüler sağlayan güvenlik Yerpaylaşımları içeren grafik bir görünüm sağlar. Eşlemeyi kullanarak Azure iş yüklerinizin ağ topolojisini, sanal makineleriniz ile alt ağlarınızla bağlantıları ve haritalardan belirli kaynaklara detaya gitme özelliğini ve bu kaynaklara yönelik önerileri görebilirsiniz.

Ağ haritasını açmak için:

1. Güvenlik Merkezi 'nin menüsünde, Azure Defender panosunu açın ve **Ağ Haritası**' nı seçin.

    :::image type="content" source="./media/security-center-network-recommendations/opening-network-map.png" alt-text="Azure Defender panosundan ağ haritasını açma" lightbox="./media/security-center-network-recommendations/opening-network-map.png":::

1. **Katmanlar** menüsünü seçin **topoloji**' yi seçin.
 
Topoloji haritasının varsayılan görünümü şunu görüntüler:

- Azure 'da seçtiğiniz abonelikler. Eşleme birden çok aboneliği destekler.
- Kaynak Yöneticisi kaynak türünün VM 'Leri, alt ağları ve sanal ağları (klasik Azure kaynakları desteklenmez)
- Eşlenen sanal ağlar
- Yalnızca yüksek veya orta önem derecesine sahip [ağ önerilerine](security-center-recommendations.md) sahip kaynaklar  
- Internet 'e yönelik kaynaklar
- Eşleme, Azure 'da seçtiğiniz abonelikler için iyileştirilmiştir. Seçiminizi değiştirirseniz, eşleme yeniden hesaplanır ve yeni ayarlarınıza göre yeniden iyileştirilir.  

[![Ağ topolojisi eşleme](./media/security-center-network-recommendations/network-map-info.png)](./media/security-center-network-recommendations/network-map-info.png#lightbox)

## <a name="understanding-the-network-map"></a>Ağ eşlemesini anlama

Ağ eşlemesi, Azure kaynaklarınızı bir **topoloji** görünümünde ve **trafik** görünümünde gösterebilir. 

### <a name="the-topology-view"></a>Topoloji görünümü

Ağ eşlemesinin **topoloji** görünümünde, ağ kaynaklarınız hakkında aşağıdaki öngörüleri görüntüleyebilirsiniz:

- İç daire içinde, seçili aboneliklerinizde tüm VNET 'leri görebilirsiniz, sonraki daire tüm alt ağlardan, dış daire ise tüm sanal makinelerdir.
- Eşlemdeki kaynakları bağlayan satırlar birbirleriyle hangi kaynakların ilişkilendirildiğini ve Azure ağınızın nasıl yapılandırıldığını bilmenizi sağlar. 
- Hangi kaynakların güvenlik merkezi 'nden açık önerilere sahip olduğunu hızlıca almak için önem göstergelerini kullanın.
- Kaynaklara gitmek ve bu kaynağın ayrıntılarını ve önerilerini doğrudan ve ağ haritası bağlamında görüntülemek için kaynakların herhangi birine tıklayabilirsiniz.  
- Haritada çok fazla kaynak görüntüleniyorsa Azure Güvenlik Merkezi, kaynaklarınızı akıllı Kümelendirmek, en kritik durumdaki kaynakları vurgulamak ve en yüksek önem derecesine sahip olmak için kendi özel algoritmasını kullanır. 

Harita etkileşimli ve dinamik olduğundan, her düğüm tıklatılabilir olur ve görünüm filtrelere göre değişebilir:

1. En üstteki filtreleri kullanarak ağ eşlemesinde gördüklerinizi değiştirebilirsiniz. Haritayı temel alarak buraya odaklanırsınız:

   -  **Güvenlik durumu**: Haritayı Azure kaynaklarınızın önem derecesine (yüksek, orta, düşük) göre filtreleyebilirsiniz.
   - **Öneriler**: bu kaynaklarda hangi önerilerin etkin olduğunu temel alarak hangi kaynakların görüntülendiğini seçebilirsiniz. Örneğin, yalnızca güvenlik merkezi 'nin ağ güvenlik gruplarını etkinleştirmenizi önerdiği kaynakları görüntüleyebilirsiniz.
   - **Ağ bölgeleri**: varsayılan olarak, haritada yalnızca Internet 'e yönelik kaynaklar görüntülenir, Iç VM 'leri de seçebilirsiniz.
 
2. Haritayı varsayılan durumuna döndürmek için herhangi bir zamanda sol üst köşede **Sıfırla** ' ya tıklayabilirsiniz.

Bir kaynağın detayına gitmek için:

1. Haritada belirli bir kaynağı seçtiğinizde, sağ bölme açılır ve kaynak hakkında genel bilgiler, varsa bağlı güvenlik çözümleri ve kaynakla ilgili öneriler sağlar. Seçtiğiniz her kaynak türü için aynı davranış türü vardır. 
2. Haritadaki bir düğümün üzerine geldiğinizde, kaynak hakkındaki abonelik, kaynak türü ve kaynak grubu dahil genel bilgileri görüntüleyebilirsiniz.
3. Araç ipucunu yakınlaştırmak ve Haritayı ilgili düğüme yeniden odaklamak için bağlantıyı kullanın. 
4. Haritayı belirli bir düğümden uzağa yeniden odaklamak için uzaklaştırın.

### <a name="the-traffic-view"></a>Trafik görünümü

**Trafik** görünümü, kaynaklarınız arasındaki tüm olası trafiğin haritasını sağlar. Bu, size hangi kaynakların hangi kaynaklara iletişim kurabildiğini tanımlayan, yapılandırdığınız tüm kuralların görsel haritasını sağlar. Bu, ağ güvenlik gruplarının var olan yapılandırmasını görmenizi ve iş yükleriniz dahilinde olası riskli yapılandırmaları hızlı bir şekilde tanımlamanızı sağlar.

### <a name="uncover-unwanted-connections"></a>İstenmeyen bağlantıları kayma

Bu görünümün kuvveti, var olan güvenlik açıklarına sahip bu izin verilen bağlantıları size gösterme imkanına sahiptir. bu sayede, kaynaklarınız üzerinde gerekli sağlamlaştırma işlemini gerçekleştirmek için bu verilerin çapraz bölümünü kullanabilirsiniz. 

Örneğin, iletişim kurabildiğinizi fark edemeyen iki makineyi tespit edebilir ve iş yüklerini ve alt ağları daha iyi yalıtmanızı sağlar.

### <a name="investigate-resources"></a>Kaynakları araştır

Bir kaynağın detayına gitmek için:

1. Haritada belirli bir kaynağı seçtiğinizde, sağ bölme açılır ve kaynak hakkında genel bilgiler, varsa bağlı güvenlik çözümleri ve kaynakla ilgili öneriler sağlar. Seçtiğiniz her kaynak türü için aynı davranış türü vardır. 
2. Kaynak üzerinde olası giden ve gelen trafik listesini görmek için **trafik** ' e tıklayın. Bu, kaynakla ve iletişim kurabildiği, hangi protokoller ve bağlantı noktalarıyla iletişim kurabildiğini gösteren kapsamlı bir listesidir. Örneğin, bir VM seçtiğinizde, iletişim kurabildiği tüm VM 'Ler gösterilir ve bir alt ağ seçtiğinizde, iletişim kurabildiği tüm alt ağlar gösterilir.

**Bu veriler ağ güvenlik gruplarının analizine ve çapraz nesnelerin ve etkileşimlerin anlaşılması için birden çok kuralı çözümleyen gelişmiş makine öğrenimi algoritmalarına dayanır.** 

[![Ağ trafiği Haritası](./media/security-center-network-recommendations/network-map-traffic.png)](./media/security-center-network-recommendations/network-map-traffic.png#lightbox)


## <a name="next-steps"></a>Sonraki adımlar

Diğer Azure kaynak türlerine uygulanan öneriler hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

- [Azure Güvenlik Merkezi'nde makinelerinizi ve uygulamalarınızı koruma](security-center-virtual-machine-protection.md)