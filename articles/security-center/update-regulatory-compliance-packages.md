---
title: Azure Güvenlik Merkezi mevzuat uyumluluk panonuzda dinamik mevzuat uyumluluğu denetimine nasıl güncellenir? Microsoft Dokümanlar
description: Mevzuata uygunluk paketlerinizi güncelleme
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: fa5027ed285456247891c84e559b74a14237f553
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537789"
---
# <a name="update-to-dynamic-compliance-packages-in-your-regulatory-compliance-dashboard"></a>Mevzuata Uygunluk panonuzdaki dinamik uyumluluk paketlerine güncelleştirme

Azure Güvenlik Merkezi, kaynaklarınızın yapılandırmasını sürekli olarak endüstri standartları, yönetmelikler ve kıyaslama gereksinimleriyle karşılaştırır. **Mevzuata uygunluk panosu,** belirli uyumluluk denetimlerini ve gereksinimlerini nasıl karşıladığınıza bağlı olarak uyumluluk duruşunuzla ilgili öngörüler sağlar.

Uyumluluk duruşunuzu izleyebileceğiniz bir standart [da Azure CIS 1.1.0](https://www.cisecurity.org/benchmark/azure/) 'dır (daha resmi olarak"BDT Microsoft Azure Vakıflar Kıyaslama sürümü 1.1.0"). 

Azure BDT'nin başlangıçta uyumluluk panonuzda görünen gösterimi, Güvenlik Merkezi'ne dahil olan statik bir kural kümesine dayanır.

Dinamik **uyumluluk paketleri** özelliği ile Güvenlik Merkezi, zaman içinde endüstri standartlarının kapsamını otomatik olarak geliştirir. Uyumluluk paketleri temelde Azure İlkesi'nde tanımlanan girişimlerdir. Bunlar seçtiğiniz kapsama (abonelik, yönetim grubu vb.) atanabilir. Uyumluluk verilerinin panonuzda değerlendirme olarak eşlenmiş olduğunu görmek için, Güvenlik Politikası'ndan yönetim grubunuzun veya aboneliğinize bir uyumluluk paketi ekleyin. Bir uyumluluk paketi eklemek, düzenleyici uyumluluk girişimini seçtiğiniz kapsama etkin bir şekilde atar. Bu şekilde, yeni yayınlanan düzenleyici girişimleri panonuzda uyumluluk standartları olarak izleyebilirsiniz. Microsoft girişim için yeni içerik yayımladığında (standartta daha fazla denetimle eşleyen yeni ilkeler), ek içerik panonuzda otomatik olarak görünür.

Azure BDT kıyaslama, **Azure CIS 1.1.0 (yeni)** için dinamik uyumluluk paketi, orijinal *statik* sürümü nde şunları yaparak geliştirir:

* Daha fazla ilke dahil
* Eklendikçe yeni kapsama alanıyla otomatik olarak güncelleniyor 

Aşağıda açıklandığı gibi yeni dinamik pakete güncelleştirin.

## <a name="adding-a-dynamic-compliance-package"></a>Dinamik bir uyumluluk paketi ekleme

Aşağıdaki adımlar, Azure BDT kıyaslama v1.1.0 ile uyumluluğunuziçin dinamik paketin nasıl eklendiğini açıklar.   

### <a name="update-to-the-azure-cis-110-new-dynamic-compliance-package"></a>Azure CIS 1.1.0 (yeni) dinamik uyumluluk paketine güncelleştirme 

1. Güvenlik **ilkesi** sayfasını açın. Bu sayfa, yönetim gruplarının, aboneliklerin, çalışma alanlarının ve yönetim grubu yapınızın sayısını gösterir.

1. Mevzuata uygunluk duruşunu yönetmek istediğiniz abonelik veya yönetim grubunu seçin. Uyumluluk verilerinin tüm iç içe geçen kaynaklar için toplanıp izlenmesi için standardın geçerli olduğu en yüksek kapsamı seçmenizi öneririz. 

1. Endüstri & düzenleyici standartlar bölümünde, Azure CIS 1.1.0'ın yeni içerik için güncelleştirilebildiğinizi görürsünüz. **Şimdi Güncelleştir'i**tıklatın. 

1. İsteğe bağlı olarak, **mevzuata uygunluk standartları ekle** sayfasını açmak için daha fazla standart **ekle'yi** tıklatın. Burada, **NIST SP 800-53 R4**, **SWIFT CSP CSCF-v2020**, **UKO ve UK NHS**ve Kanada **PBMM**gibi diğer uyumluluk standartları için **Azure CIS 1.1.0 (Yeni)** ve dinamik paketleri el ile arayabilirsiniz.
    
    > [!TIP]
    > Yalnızca sahip veya ilke katkıda bulunan kullanıcılar uyumluluk standartları eklemek için gerekli izinlere sahiptir. 

    ![Azure Güvenlik Merkezi'nin düzenleyici uyumluluk panosuna düzenleyici paketler ekleme](./media/update-regulatory-compliance-packages/security-center-dynamic-regulatory-compliance-additional-standards.png)


1. Güvenlik Merkezi'nin kenar çubuğundan, mevzuata uygunluk panosunu açmak için **Mevzuata uygunluk** seçeneğini belirleyin. 
    * Azure CIS 1.1.0 (Yeni), endüstri & düzenleyici standartları listenizde görünür. 
    * Azure CIS 1.1.0 uyumluluğunun özgün *statik* görünümü de bu uyumluluğun yanında kalır. Gelecekte otomatik olarak kaldırılabilir.

    > [!NOTE]
    > Yeni eklenen bir standardın uyumluluk panosunda görünmesi birkaç saat sürebilir.


    [![Eski ve yeni Azure BDT'yi gösteren düzenleyici uyumluluk panosu](media/update-regulatory-compliance-packages/security-center-dynamic-regulatory-compliance-cis-old-and-new.png)](media/update-regulatory-compliance-packages/security-center-dynamic-regulatory-compliance-cis-old-and-new.png#lightbox)


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendim:

* Mevzuata uygunluk panonuzda gösterilen standartları yeni *dinamik* paketlere **yükseltme**
* Ek standartlara uygunluğunuzu izlemek için **uyumluluk paketleri** ekleme. 

Diğer ilgili materyaller için aşağıdaki makalelere bakın: 

- [Güvenlik merkezi mevzuat uyumluluk panosu](security-center-compliance-dashboard.md)
- [Güvenlik ilkeleriyle çalışma](tutorial-security-policy.md)
- [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) - Azure kaynaklarınızı korumaya yardımcı olmak için Azure Güvenlik Merkezi'nde önerileri nasıl kullanacağınızı öğrenin.