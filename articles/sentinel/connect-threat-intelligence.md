---
title: Tehdit zekası verilerini Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: Tehdit zekası verilerini Azure Sentinel'e nasıl bağlayabilirsiniz hakkında bilgi edinin.
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/22/2019
ms.author: yelevin
ms.openlocfilehash: eec07a01edc6b126bb7cd3a814912ea5c5b14195
ms.sourcegitcommit: c5661c5cab5f6f13b19ce5203ac2159883b30c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80529089"
---
# <a name="connect-data-from-threat-intelligence-providers"></a>Tehdit istihbarat sağlayıcılarından veri bağlama

> [!IMPORTANT]
> Azure Sentinel'deki Tehdit İstihbaratı veri bağlayıcıları şu anda genel önizlemededir.
> Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Azure Sentinel, kuruluşunuzun kullandığı tehdit göstergelerini içe aktarmanıza olanak tanıyan güvenlik analistlerinizin bilinen tehditleri algılama ve önceliklerini geliştirme sini sağlayabilir. Azure Sentinel'in çeşitli özellikleri kullanılabilir hale gelir veya geliştirilir:

- **Analytics,** tehdit göstergelerinizdeki günlük olaylarının eşleşmelerine dayalı olarak uyarılar ve olaylar oluşturmak için etkinleştirebileceğiniz bir dizi zamanlanmış kural şablonu içerir.

- **Çalışma kitapları,** Azure Sentinel'e aktarılan tehdit göstergeleri ve tehdit göstergelerinizle eşleşen analiz kurallarından oluşturulan uyarılar hakkında özet bilgiler sağlar.

- **Avcılık** sorguları, güvenlik araştırmacılarının yaygın avlanma senaryoları bağlamında tehdit göstergelerini kullanmalarına olanak sağlar.

- **Not defterleri,** anormallikleri araştırdığınızda ve kötü amaçlı davranışları araştırdığınızda tehdit göstergelerini kullanabilir.

Tehdit göstergelerini bir sonraki bölümde listelenen tümleşik tehdit istihbaratı platformu (TIP) ürünlerinden birini kullanarak, TAXII sunucularına bağlanarak veya [Microsoft Graph Security tiIndicators API](https://aka.ms/graphsecuritytiindicators)ile doğrudan tümleştirme kullanarak Azure Sentinel'e aktarabilirsiniz.

## <a name="integrated-threat-intelligence-platform-products"></a>Entegre tehdit istihbarat platformu ürünleri

- [MISP Açık Kaynak Tehdit İstihbarat Platformu](https://www.misp-project.org/)
    
    MISS örnekleri olan istemcilerin tehdit göstergelerini Microsoft Graph Security API'sine geçirmelerini sağlayan örnek bir komut dosyası [için, MISS'yi Microsoft Graph Security Script'e](https://github.com/microsoftgraph/security-api-solutions/tree/master/Samples/MISP)bakın.

- [Anomali ThreatStream](https://www.anomali.com/products/threatstream)

    ThreatStream Entegratörü ve Uzantıları'nı ve ThreatStream istihbaratını Microsoft Graph Security API'sine bağlama yönergelerini indirmek için [ThreatStream indirme sayfasına](https://ui.threatstream.com/downloads) bakın.

- [Palo Alto Ağları MineMeld](https://www.paloaltonetworks.com/products/secure-the-network/subscriptions/minemeld)
    
    Kılavuzlu yönergeler için [MineMeld kullanarak Microsoft Graph Security API'sine IOC gönderme'ye](https://live.paloaltonetworks.com/t5/MineMeld-Articles/Sending-IOCs-to-the-Microsoft-Graph-Security-API-using-MineMeld/ta-p/258540)bakın.

- [ThreatConnect Platformu](https://threatconnect.com/solution/)

    Daha fazla bilgi için [ThreatConnect Tümleştirmeleri'ne](https://threatconnect.com/integrations/) bakın ve sayfadaki Microsoft Graph Security API'sini arayın.


## <a name="connect-azure-sentinel-to-your-threat-intelligence-platform"></a>Azure Sentinel'i tehdit istihbarat platformunuza bağlayın

## <a name="prerequisites"></a>Ön koşullar  

- TIP ürününe veya Microsoft Graph Security tiIndicators API ile doğrudan tümleştirme kullanan özel uygulamanıza izin vermek için Global administrator veya Security yöneticisinin Azure AD rolü.

- Tehdit göstergelerinizi depolamak için Azure Sentinel çalışma alanına izinler okuyun ve yazın.

## <a name="instructions"></a>Yönergeler

1. Uygulama kimliği, uygulama sırrı ve Azure Etkin Dizin kiracı kimliği almak için [bir uygulamayı](/graph/auth-v2-service#1-register-your-app) Azure Etkin Dizini'ne kaydedin. Microsoft Graph Security tiIndicators API ile doğrudan tümleştirme kullanan tümleşik TIP ürününüzü veya uygulamanızı yapılandırdığınızda bu değerlere ihtiyacınız vardır.

2. Kayıtlı uygulama için [API izinlerini yapılandırın:](/graph/auth-v2-service#2-configure-permissions-for-microsoft-graph) Microsoft Graph Application izni **ThreatIndicators.ReadWrite.OwnedBy'yi** kayıtlı uygulamanıza ekleyin.

3. Azure Active Directory kiracı yöneticinizden kuruluşunuz için kayıtlı uygulama için yönetici izni vermesini isteyin. Azure portalından: **Azure Active Directory** > **App kayıtları** > **\<_uygulama adı_>** > **Görünüm API İzinleri** > Kiracı** \< _adı_>için yönetici onayı verin.**

4. Aşağıdakileri belirterek Azure Sentinel'e göstergeler göndermek için Microsoft Graph Security tiIndicators API ile doğrudan tümleştirme kullanan TIP ürününüzü veya uygulamanızı yapılandırın:
    
    a. Kayıtlı uygulamanın kimliği, gizli ve kiracı kimliği için değerler.
    
    b. Hedef ürün için Azure Sentinel'i belirtin.
    
    c. Eylem için uyarı belirtin.

5. Azure portalında Azure **Sentinel** > **Veri bağlayıcılarına** gidin ve ardından **Tehdit İstihbarat Platformları (Önizleme)** bağlayıcısını seçin.

6. **Bağlayıcıyı Aç sayfasını**seçin ve sonra **Bağlan.**

7. Azure Sentinel'e aktarılan tehdit göstergelerini görüntülemek için **Azure Sentinel - Logs** > **SecurityInsights**adresine gidin ve **threatIntelligenceIndicator'ı**genişletin.

## <a name="connect-azure-sentinel-to-taxii-servers"></a>Azure Sentinel'i TAXII sunucularına bağlayın

## <a name="prerequisites"></a>Ön koşullar  

- Tehdit göstergelerinizi depolamak için Azure Sentinel çalışma alanına izinler okuyun ve yazın.

- TAXII 2.0 sunucu URI ve Koleksiyon Kimliği.

## <a name="instructions"></a>Yönergeler

1. Azure portalında Azure **Sentinel** > **Veri bağlayıcılarına** gidin ve ardından **Tehdit İstihbaratı - TAXII (Önizleme)** bağlayıcısını seçin.

2. **Bağlayıcıyı Aç'ı**seçin.

3. Metin kutularında gerekli ve isteğe bağlı bilgileri belirtin.

4. TAXII 2.0 sunucusuna bağlantıyı etkinleştirmek için **Ekle'yi** seçin.

5. Ek TAXII 2.0 sunucularınız varsa: 3 ve 4 adımlarını yineleyin.

6. Azure Sentinel'e aktarılan tehdit göstergelerini görüntülemek için **Azure Sentinel - Logs** > **SecurityInsights**adresine gidin ve **threatIntelligenceIndicator'ı**genişletin.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, tehdit istihbarat sağlayıcınızı Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın.

- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
