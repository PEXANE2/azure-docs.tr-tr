---
title: Azure Güvenlik Merkezi mevzuat uyumluluk panonuzda dinamik mevzuata uyumluluğu izlemeye güncelleştirme | Microsoft Docs
description: Yasal uyumluluk paketlerinizi güncelleştirme
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
ms.openlocfilehash: 6ba0be3a6fba35e413270dd6770f5d3f47586b5e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83873333"
---
# <a name="customizing-the-set-of-standards-in-your-regulatory-compliance-dashboard"></a>Yasal uyumluluk panonuzda standartlar kümesini özelleştirme

Azure Güvenlik Merkezi, kaynaklarınızın yapılandırmalarını endüstri standartları, yönetmelikler ve kıyaslamalar ile sürekli olarak karşılaştırır. **Mevzuat uyumluluk panosu** , uyumluluk denetimi ve gereksinimleri nasıl karşıladığınızı temel alarak uyumlulukla ilgili öngörüler sağlar.


## <a name="overview-of-compliance-packages"></a>Uyumluluk paketlerine genel bakış

Endüstri standartları, mevzuat standartları ve değerlendirmeleri, güvenlik merkezi 'nde *uyumluluk paketleri*olarak temsil edilir.  Her paket Azure Ilkesinde tanımlanan bir girişimdir. Panonuzda değerlendirmelere eşlenen uyumluluk verilerini görmek için, **güvenlik ilkesi** sayfasından yönetim grubunuza veya aboneliğinize bir uyumluluk paketi ekleyin. (Azure Ilkesi ve [güvenlik Ilkeleriyle çalışma](tutorial-security-policy.md)girişimleri hakkında daha fazla bilgi edinin.)

Seçtiğiniz kapsamınızda bir standart veya kıyaslama eklendi aldığınızda, girişimi kapsama atar ve standart, mevzuat uyumluluk panonuzda değerlendirmelere eşlenen tüm ilişkili uyumluluk verileriyle birlikte görünür. Ayrıca, eklendi olan standartlardan herhangi biri için Özet raporlar indirebilirsiniz.

Microsoft ayrıca yasal düzenleme standartlarını de izler ve zaman içindeki bazı paketlerdeki kapsamı otomatik olarak geliştirir. Microsoft, girişim için yeni içerik yayımlarsa (Standart içindeki daha fazla denetim ile eşlenen yeni ilkeler), panonuzda ek içerik otomatik olarak görünür.

> [!TIP]
> Microsoft 'un yeni içerik yayımlaması için zaman içinde gelişdiği bir standart, **Azure CIS 1.1.0 (yeni)** (daha resmi olarak [Microsoft Azure, CIS, kıyaslama sürümü 1.1.0](https://www.cisecurity.org/benchmark/azure/)). Bunu panonuza "Azure CIS 1.1.0", her güvenlik merkezi ortamında varsayılan olarak yapılandırılan Azure CIS 'nin temsili ile eklemeniz gerekir. Bu paket, bir statik kural kümesine bağlıdır. Daha yeni paket daha fazla ilke içerir ve zaman içinde otomatik olarak güncelleştirilecek. Aşağıda açıklandığı gibi yeni dinamik pakete güncelleştirin.


## <a name="available-packages"></a>Kullanılabilir paketler

NıST SP 800-53 R4, SWIFT CSP CSCF-V2020, UK resmi ve UK NHS, Kanada Federal PBMM ve Azure CIS 1.1.0 (yeni) gibi standartları ekleyerek Azure CIS 1.1.0 'in daha kapsamlı bir gösterimidir. 

Ayrıca, genel uyumluluk çerçevelerine göre güvenlik ve uyum için en iyi uygulamalar için Microsoft tarafından yazılmış, Azure 'a özgü yönergeler olan **Azure Güvenlik kıyaslaması**ekleyebilirsiniz. ([Azure Güvenlik kıyaslaması hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/security/benchmarks/introduction).)

Panoda kullanılabilir hale geldiğinde ek standartlar desteklenecektir. 


## <a name="adding-a-regulatory-standard-to-your-dashboard"></a>Panonuza düzenleme standardı ekleme

Aşağıdaki adımlarda, desteklenen mevzuata standartlarından biriyle uyumluluğunuzu izlemek için bir paket ekleme açıklanmaktadır.

> [!NOTE]
> Yalnızca sahibi veya ilke katılımcısı olan kullanıcılar uyumluluk standartları eklemek için gerekli izinlere sahiptir. 

1. Güvenlik Merkezi 'nin kenar çubuğundan, mevzuat uyumluluk panosunu açmak için **mevzuata uyumluluğu** ' nu seçin. Burada şu anda seçili olan aboneliklerde Şu anda atanmış olan uyumluluk standartlarını görebilirsiniz.   

1. Sayfanın üst kısmından **Uyumluluk Ilkelerini Yönet**' i seçin. Ilke Yönetimi sayfası görüntülenir.

1. Mevzuat uyumluluk duruşunu yönetmek istediğiniz aboneliği veya yönetim grubunu seçin. 

    > [!TIP]
    > Tüm iç içe geçmiş kaynaklar için uyumluluk verilerinin toplanması ve izlenmesi için standart için geçerli olan en yüksek kapsamı seçmenizi öneririz. 

1. Kuruluşunuza uygun standartları eklemek için **daha fazla standartlar Ekle**' ye tıklayın. 

1. **Yasal uyumluluk standartları Ekle** sayfasında, kullanılabilir standartlardan herhangi birine yönelik paketleri arayabilirsiniz. Kullanılabilir standartların bazıları şunlardır:

    - **Azure Güvenlik Karşılaştırması**
    - **NIST SP 800-53 R4**
    - **SWIFT CSP CSCF-V2020**
    - **UKO ve UK NHS**
    - **Kanada PBMM**
    
    ![Azure Güvenlik Merkezi 'nin mevzuat uyumluluk panosuna mevzuat paketleri ekleme](./media/update-regulatory-compliance-packages/dynamic-regulatory-compliance-additional-standards.png)

1. Güvenlik Merkezi 'nin kenar çubuğundan, mevzuat uyumluluk panosuna geri dönmek için **yasal uyumluluk** ' i seçin.
    * Yeni standart, sektör & mevzuat standartları listenizde görünür. 
    * **Azure CIS 1.1.0 (yeni)** eklediyseniz, Azure CIS 1.1.0 uyumluluğun orijinal *statik* görünümü de bunun yanında kalır. Gelecekte otomatik olarak kaldırılabilir.

    > [!NOTE]
    > Yeni eklenen bir standart, uyumluluk panosu 'nda görünmesi birkaç saat sürebilir.

    [![Eski ve yeni Azure CIS 'yi gösteren yasal uyumluluk panosu](media/update-regulatory-compliance-packages/regulatory-compliance-dashboard-with-benchmark-small.png)](media/update-regulatory-compliance-packages/regulatory-compliance-dashboard-with-benchmark.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, ek standartlarla uyumluluğunuzu izlemek için **uyumluluk paketleri eklemeyi** öğrendiniz. 

Diğer ilgili malzemeler için aşağıdaki makalelere bakın: 

- [Azure Güvenlik Karşılaştırması](https://docs.microsoft.com/azure/security/benchmarks/introduction)
- [Güvenlik Merkezi mevzuata uyumluluğu panosu](security-center-compliance-dashboard.md)
- [Güvenlik ilkeleriyle çalışma](tutorial-security-policy.md)