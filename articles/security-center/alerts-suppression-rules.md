---
title: Azure Güvenlik Merkezi 'nde yanlış pozitifleri veya diğer istenmeyen güvenlik uyarılarını gizlemek için uyarı gizleme kurallarını kullanma.
description: Bu makalede, istenmeyen güvenlik uyarılarını gizlemek için Azure Güvenlik Merkezi 'nin gizleme kurallarının nasıl kullanılacağı açıklanmaktadır.
author: memildin
manager: rkarlin
services: security-center
ms.author: memildin
ms.date: 05/04/2020
ms.service: security-center
ms.topic: conceptual
ms.openlocfilehash: 18b1b4cb959603f5898e7c725102f35d7abc90cf
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85848989"
---
# <a name="suppress-alerts-from-azure-security-centers-threat-protection"></a>Azure Güvenlik Merkezi 'nin tehdit korumasından gelen uyarıları gösterme

Bu sayfada, Azure Güvenlik Merkezi 'nde yanlış pozitifleri veya diğer istenmeyen güvenlik uyarılarını gizlemek için uyarı gizleme kurallarını nasıl kullanabileceğiniz açıklanmaktadır.

## <a name="availability"></a>Kullanılabilirlik

- Yayın durumu: **Önizleme**
- Gerekli roller: **Güvenlik Yöneticisi** ve **sahibi** , kurallar oluşturabilir/silebilir. **Güvenlik okuyucu** ve **okuyucu** , kuralları görüntüleyebilir.
- Larının 
    - Ticari bulutlar ✔
    - ✔ National/Sovereign (US Gov, Çin gov, diğer gov)


## <a name="what-are-suppression-rules"></a>Gizleme kuralları nelerdir?

Azure Güvenlik Merkezi 'nin tehdit koruması bileşenleri, ortamınızın herhangi bir alanındaki tehditleri algılar ve güvenlik uyarıları oluşturur.

Tek bir uyarı ilgi çekici veya ilgili olmadığında bunu el ile kapatabilirsiniz. Alternatif olarak, daha sonra benzer uyarıları otomatik olarak kapatmak için gizleme kuralları özelliğini kullanın. Genellikle bir gizleme kuralı kullanarak şunları yapabilirsiniz:

- yanlış pozitif sonuçlar olarak belirlediğiniz uyarıları gösterme

- çok sık tetiklenen uyarıları, faydalı olması için gösterme

Gizleme kurallarınız, uyarıların otomatik olarak hangi ölçütlere göre kapatılacağını tanımlar.

> [!CAUTION]
> Güvenlik uyarılarını gizleme Güvenlik Merkezi 'nin tehdit korumasını azaltır. Herhangi bir gizleme kuralının olası etkisini dikkatle denetlemeniz ve zaman içinde izlemeniz gerekir.

![Uyarı gizleme kuralı oluştur](media\alerts-suppression-rules\create-suppression-rule.gif)

## <a name="create-a-suppression-rule"></a>Gizleme kuralı oluşturma

İstenmeyen güvenlik uyarılarını bastırmak için kurallar oluşturabileceğiniz birkaç yol vardır:

- Yönetim grubu düzeyinde uyarıları gizlemek için Azure Ilkesi 'ni kullanın

- Uyarıları abonelik düzeyinde gizlemek için, aşağıda açıklandığı gibi Azure portal veya REST API kullanabilirsiniz

Gizleme kuralları yalnızca seçili aboneliklerde zaten tetiklenen uyarıları kapatabilir.

Azure portal doğrudan bir kural oluşturmak için:

1. Güvenlik Merkezi 'nin güvenlik uyarıları sayfasından:

    - Artık görmek istemediğiniz belirli uyarıyı bulun ve uyarının üç nokta menüsünden (...) **gizleme kuralı oluştur**' u seçin:

        [![* * Gizleme kuralı oluştur * * seçeneği](media/alerts-suppression-rules/auto-dismiss-future-option.png)](media/alerts-suppression-rules/auto-dismiss-future-option.png#lightbox)

    - Ya da sayfanın üst kısmındaki **gizleme kuralları** bağlantısını ve gizleme kuralları sayfasında **Yeni gizleme kuralı oluştur**' u seçin:

        ![Yeni gizleme kuralı oluştur * * düğmesi](media/alerts-suppression-rules/create-new-suppression-rule.png)

1. Yeni gizleme kuralı bölmesinde yeni kuralınızın ayrıntılarını girin.

    - Kuralınız, daha sonra bu gibi bir uyarı alabilmek için **tüm kaynaklardaki** uyarıları kapatabilir. 
    
    - Kuralınız belirli bir **ölçütte** uyarıyı kapatabilir; belırlı bir IP adresi, işlem adı, Kullanıcı hesabı, Azure kaynağı veya konum ile ilgilidir.

    > [!TIP]
    > Yeni kural sayfasını belirli bir uyarıdan açtıysanız, uyarı ve abonelik yeni kuralınızdan otomatik olarak yapılandırılır. **Yeni gizleme kuralı oluştur** bağlantısını kullandıysanız, seçili abonelikler portaldaki geçerli filtreyle eşleşir.

    [![Kural oluşturma bölmesinin gizleme](media/alerts-suppression-rules/new-suppression-rule-pane.png)](media/alerts-suppression-rules/new-suppression-rule-pane.png#lightbox)

1. Kuralın ayrıntılarını girin:

    - **Ad** -kural için bir ad. Kural adları bir harf veya sayı ile başlamalı, 2 ila 50 karakter arasında olmalıdır ve tire (-) veya alt çizgi (_) dışında bir sembol içermemelidir. 
    - **Durum** -etkin veya devre dışı.
    - **Neden** -gereksinimlerinizi karşılamıyorsa, yerleşik nedenlerden birini veya ' diğer ' seçeneğini belirleyin.
    - **Sona erme tarihi** -kuralın bitiş tarihi ve saati. Kurallar, altı aya kadar çalışabilir.

1. İsteğe bağlı olarak, bu kural etkin olduğunda kaç uyarı kapatılacağını görmek için **Benzetim** düğmesini kullanarak kuralı test edin.

1. Kuralı kaydedin. 

## <a name="edit-a-suppression-rules"></a>Gizleme kurallarını Düzenle

Oluşturduğunuz kuralları düzenlemek için gizleme kuralları sayfasını kullanın.

1. Güvenlik Merkezi 'nin güvenlik uyarıları sayfasında, sayfanın üst kısmındaki **gizleme kuralları** bağlantısını seçin.

1. Gizleme kuralları sayfası, seçili abonelikler için tüm kurallarla birlikte açılır.

    [![Gizleme kuralları listesi](media/alerts-suppression-rules/suppression-rules-page.png)](media/alerts-suppression-rules/suppression-rules-page.png#lightbox)

1. Tek bir kuralı düzenlemek için, kural için üç nokta menüsünü (...) açın ve **Düzenle**' yi seçin.

1. Gerekli değişiklikleri yapın ve **Uygula**' yı seçin. 

## <a name="delete-a-suppression-rule"></a>Gizleme kuralını Sil

Oluşturduğunuz bir veya daha fazla kuralı silmek için gizleme kuralları sayfasını kullanın.

1. Güvenlik Merkezi 'nin güvenlik uyarıları sayfasında, sayfanın üst kısmındaki **gizleme kuralları** bağlantısını seçin.

1. Gizleme kuralları sayfası, seçili abonelikler için tüm kurallarla birlikte açılır.

1. Tek bir kuralı silmek için, kural için üç nokta menüsünü (...) açın ve **Sil**' i seçin.

1. Birden çok kuralı silmek için, kuralların silineceği onay kutularını seçin ve **Sil**' i seçin.

    ![Bir veya daha fazla gizleme kuralını silme](media/alerts-suppression-rules/delete-multiple-alerts.png)

## <a name="view-suppressed-alerts"></a>Gizlenen Uyarıları görüntüle

Etkin gizleme kuralları ile eşleşen uyarılar üretilmeye devam eder, ancak durumları **kapatıldı**olarak ayarlanır. Durumu Azure portal görebilirsiniz, ancak Güvenlik Merkezi güvenlik uyarılarınıza erişebilirsiniz. 

> [!TIP]
> [Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/) , gizlenen uyarılar için olaylar oluşturmaz. Diğer Sıems 'ler için, uyarılar ' durumu (' kapatıldı ') kullanarak gizlenen uyarıları filtreleyebilirsiniz.

Kurallarınız tarafından kapatılan uyarıları görüntülemek için Güvenlik Merkezi 'nin filtresini kullanın.

* Güvenlik Merkezi 'nin güvenlik uyarıları sayfasında, filtre seçeneklerini açın ve **kapatıldı**' ı seçin.  

   [![Kapatılan uyarıları görüntüleme](media/alerts-suppression-rules/view-dismissed-alerts.png)](media/alerts-suppression-rules/view-dismissed-alerts.png#lightbox)


## <a name="create-and-manage-suppression-rules-with-the-api"></a>API ile gizleme kuralları oluşturma ve yönetme

Güvenlik Merkezi 'nin REST API aracılığıyla uyarı gizleme kuralları oluşturabilir, görüntüleyebilir veya silebilirsiniz. 

REST API kuralları gizleme için ilgili HTTP yöntemleri şunlardır:

- **PUT**: belirtilen abonelikte bir gizleme kuralı oluşturmak veya güncelleştirmek için.

- **Al**:

    - Belirtilen bir abonelik için yapılandırılmış tüm kuralları listelemek için. Bu yöntem, uygulanabilir kuralların dizisini döndürür.

    - Belirtilen bir abonelikte belirli bir kuralın ayrıntılarını almak için. Bu yöntem bir gizleme kuralı döndürür.

    - Gizleme kuralının etkisinin hala tasarım aşamasında benzetimini yapmak. Bu çağrı, kuralın etkin olması durumunda mevcut uyarılarınızın hangisinin kapatılacağını tanımlar.

- **Sil**: varolan bir kuralı siler (ancak tarafından zaten kapatılan uyarıların durumunu değiştirmez).

Tam ayrıntılar ve kullanım örnekleri için [API belgelerine](https://docs.microsoft.com/rest/api/securitycenter/)bakın. 


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Güvenlik Merkezi 'nde istenmeyen uyarıları otomatik olarak kapatmak için gizleme kuralları açıklanmaktadır.

Azure Güvenlik Merkezi 'ndeki güvenlik uyarıları hakkında daha fazla bilgi için aşağıdaki sayfalara bakın:

- [Güvenlik uyarıları ve amaç sonlandırma zinciri](alerts-reference.md) -Azure Güvenlik Merkezi 'Nin tehdit koruması modülünde görebileceğiniz güvenlik uyarıları için bir başvuru kılavuzu.
- [Azure Güvenlik Merkezi 'Nde tehdit koruması](threat-protection.md) -ortamınızın Azure Güvenlik Merkezi 'Nin tehdit koruması modülü tarafından izlenen birçok yönü hakkında bir açıklama.