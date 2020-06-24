---
title: Azure CDN içeriğini ülkeye/bölgeye göre kısıtla | Microsoft Docs
description: Coğrafi filtreleme özelliğini kullanarak, ülkeye/bölgeye erişimi Azure CDN içeriğinize kısıtlama hakkında bilgi edinin.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 06/19/2018
ms.author: allensu
ms.openlocfilehash: fba1f0b1f8160dece41c312b61cbc8ae9571436d
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84887028"
---
# <a name="restrict-azure-cdn-content-by-countryregion"></a>Azure CDN içeriğini ülkeye/bölgeye göre kısıtla

## <a name="overview"></a>Genel Bakış
Bir kullanıcı içeriğinizi istediğinde, varsayılan olarak, istek yapan kullanıcının konumundan bağımsız olarak içerik sunulur. Ancak bazı durumlarda, içeriğinize/bölgeye göre içeriğinizi erişimi kısıtlamak isteyebilirsiniz. *Coğrafi filtreleme* özelliği ile, seçili ülkelerde/bölgelerde içeriğe izin vermek veya bunları engellemek için CDN uç noktanıza belirli yollarda kurallar oluşturabilirsiniz.

> [!IMPORTANT]
> **Microsoft profillerinin Azure CDN Standard** , yol tabanlı coğrafi filtreleme 'yi desteklemez.
> 

## <a name="standard-profiles"></a>Standart profiller
Bu bölümdeki yordamlar, Akamai Azure CDN ve yalnızca **Verizon** profillerinden standart **Azure CDN Standart** yöneliktir. 

**Verizon profillerden Azure CDN Premium** için, coğrafi filtrelemeyi etkinleştirmek üzere portalı **Yönet** ' i kullanmanız gerekir. Daha fazla bilgi için bkz. [Verizon Profiles 'dan Azure CDN Premium](#azure-cdn-premium-from-verizon-profiles).

### <a name="define-the-directory-path"></a>Dizin yolunu tanımlama
Coğrafi filtreleme özelliğine erişmek için Portal içindeki CDN uç noktanızı seçin, ardından sol taraftaki menüde Ayarlar ' ın altında **coğrafi filtreleme** ' yi seçin. 

![Coğrafi filtreleme standardı](./media/cdn-filtering/cdn-geo-filtering-standard.png)

**Yol** kutusunda, kullanıcıların erişime izin verilecek veya erişimi reddedildiği konumun göreli yolunu belirtin. 

Tüm dosyalarınız için bir eğik çizgi (/) ile coğrafi filtreleme uygulayabilir veya dizin yollarını belirterek belirli klasörleri seçebilirsiniz (örneğin, */resim/*). Ayrıca, tek bir dosyaya coğrafi filtreleme uygulayabilirsiniz (örneğin, */resim/city.png*). Birden çok kurala izin verilir; bir kural girdikten sonra, sonraki kuralı girmeniz için boş bir satır görüntülenir.

Örneğin, aşağıdaki dizin yolu filtrelerinin tümü geçerlidir:   
*/*                                 
*Fotoğraflar*     
*/Photos/Strasbourg/*     
*/Photos/Strasbourg/city.png*

### <a name="define-the-type-of-action"></a>Eylem türünü tanımlayın

**Eylem** listesinden **Izin ver** veya **Engelle**' yi seçin: 

- **Izin ver**: yalnızca belirtilen ülkelere/bölgelere ait kullanıcılara özyinelemeli yoldan istenen varlıklara erişim izni verilir.

- **Engelle**: belirtilen ülkelerin/bölgelerdeki kullanıcıların özyinelemeli yoldan istenen varlıklara erişimi reddedildi. Bu konum için başka bir ülke/bölge filtreleme seçeneği yapılandırılmamışsa, diğer tüm kullanıcılara erişim izni verilir.

Örneğin, */photos/Strasbourg/* yolunu engellemek için bir coğrafi filtreleme kuralı aşağıdaki dosyaları filtreler:     
*http: \/ / \<endpoint> . azureedge.net/photos/Strasbourg/1000.jpg* 
 *http: \/ / \<endpoint> . azureedge.net/photos/Strasbourg/Cathedral/1000.jpg*

### <a name="define-the-countriesregions"></a>Ülkeleri/bölgeleri tanımlama
**Ülke kodları** listesinden, yol için engellemek veya izin vermek istediğiniz ülkeleri/bölgeleri seçin. 

Ülkeleri/bölgeleri seçmeyi tamamladıktan sonra yeni coğrafi Filtreleme kuralını etkinleştirmek için **Kaydet** ' i seçin. 

![Coğrafi filtreleme kuralları](./media/cdn-filtering/cdn-geo-filtering-rules.png)

### <a name="clean-up-resources"></a>Kaynakları temizleme
Bir kuralı silmek için, **coğrafi filtreleme** sayfasındaki listeden seçin ve **Sil**' i seçin.

## <a name="azure-cdn-premium-from-verizon-profiles"></a>Verizon profillerden Azure CDN Premium
**Verizon profillerden Azure CDN Premium** için, coğrafi filtreleme kuralı oluşturmaya yönelik kullanıcı arabirimi farklıdır:

1. Azure CDN profilinizde üst menüden **Yönet**' i seçin.

2. Verizon portalından, **http büyük**' i seçin ve ardından **ülke filtreleme**' yi seçin.

    ![Coğrafi filtreleme standardı](./media/cdn-filtering/cdn-geo-filtering-premium.png)

3. **Ülke filtresi ekle**' yi seçin.

    **Birinci adım:** sayfa görüntülenir.

4. Dizin yolunu girin, **blok** veya **Ekle**' yi seçin ve ardından **İleri**' yi seçin.

    **Ikinci adım:** sayfa görüntülenir. 

5. Listeden bir veya daha fazla ülke/bölge seçin ve ardından kuralı etkinleştirmek için **son** ' u seçin. 
    
    Yeni kural, **ülke filtreleme** sayfasındaki tabloda görüntülenir.

    ![Coğrafi filtreleme kuralları](./media/cdn-filtering/cdn-geo-filtering-premium-rules.png)

### <a name="clean-up-resources"></a>Kaynakları temizleme
Ülke/bölge filtreleme kuralları tablosunda, bir kuralın yanındaki Sil simgesini veya değiştirmek için Düzenle simgesini seçin.

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* Coğrafi filtreleme yapılandırmanızda yapılan değişiklikler hemen geçerli olmaz:
   * **Microsoft’tan Azure CDN Standart** profilleri için yayma işlemi genellikle 10 dakikada tamamlanır. 
   * **Akamai’den Azure CDN Standart** profilleri için yayma işlemi genellikle bir dakika içinde tamamlanır. 
   * **Verizon’dan Azure CDN Standart** ve **Verizon’dan Azure CDN Premium** profilleri için yayma işlemi genellikle 10 dakika içinde tamamlanır. 
 
* Bu özellik joker karakterleri (örneğin, *) desteklemez.

* Göreli yol ile ilişkili coğrafi filtreleme yapılandırması, bu yola yinelemeli olarak uygulanır.

* Aynı göreli yola yalnızca bir kural uygulanabilir. Diğer bir deyişle, aynı göreli yolu işaret eden birden çok ülke/bölge filtresi oluşturamazsınız. Ancak, ülke/bölge filtreleri özyinelemeli olduğundan, bir klasörde birden fazla ülke/bölge filtresi olabilir. Diğer bir deyişle, daha önce yapılandırılmış bir klasörün alt klasörüne farklı bir ülke/bölge filtresi atanabilir.

* Coğrafi filtreleme özelliği, bir isteğin güvenli bir dizin için izin verilen veya engellenen ülkeleri/bölgeleri tanımlamak için ülke kodlarını kullanır. Akamai ve Verizon profilleri aynı ülke kodlarının çoğunu desteklese de, bazı farklılıklar vardır. Daha fazla bilgi için bkz. [Azure CDN ülke kodları](/previous-versions/azure/mt761717(v=azure.100)). 

