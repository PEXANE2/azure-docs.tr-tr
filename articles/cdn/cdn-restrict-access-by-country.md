---
title: Azure CDN içeriğini ülkeye/bölgeye göre kısıtlama | Microsoft Dokümanlar
description: Coğrafi filtreleme özelliğini kullanarak ülkeye/bölgeye göre erişimi Azure CDN içeriğinize göre nasıl kısıtlayacağız öğrenin.
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
ms.topic: article
ms.date: 06/19/2018
ms.author: allensu
ms.openlocfilehash: 7ae7224efdaa281106dfbe2118ab0092c8284c6e
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81260165"
---
# <a name="restrict-azure-cdn-content-by-countryregion"></a>Azure CDN içeriğini ülkeye/bölgeye göre kısıtlama

## <a name="overview"></a>Genel Bakış
Bir kullanıcı içeriğinizi istediğinde, varsayılan olarak, isteği yapan kullanıcının konumuna bakılmaksızın içerik sunulur. Ancak, bazı durumlarda, içeriğinize erişimi ülkeye/bölgeye göre kısıtlamak isteyebilirsiniz. Coğrafi *filtreleme* özelliği yle, belirli ülkelerde/bölgelerde ki içeriğe izin vermek veya engellemek için CDN bitiş noktanızda belirli yollarda kurallar oluşturabilirsiniz.

> [!IMPORTANT]
> **Microsoft profillerinden Azure CDN Standardı** yol tabanlı coğrafi filtrelemayı desteklemez.
> 

## <a name="standard-profiles"></a>Standart profiller
Bu bölümdeki yordamlar yalnızca Verizon profillerinden **Akamai'den Azure CDN Standardı** ve **Azure CDN Standardı** içindir. 

**Verizon profillerinden Azure CDN Premium** için, coğrafi filtrelemayı etkinleştirmek için **Yönet** portalını kullanmanız gerekir. Daha fazla bilgi için [Verizon profillerinden Azure CDN Premium'a](#azure-cdn-premium-from-verizon-profiles)bakın.

### <a name="define-the-directory-path"></a>Dizin yolunu tanımlama
Coğrafi filtreleme özelliğine erişmek için portal içindeki CDN bitiş noktanızı seçin ve ardından sol menüdeki AYARLAR altında **Coğrafi filtreleme'yi** seçin. 

![Coğrafi filtreleme standardı](./media/cdn-filtering/cdn-geo-filtering-standard.png)

**PATH** kutusundan, kullanıcıların erişilmesine izin verilebilen veya erişilenlerin engelleneceği konuma göreli yolu belirtin. 

Tüm dosyalarınız için bir ileri eğik çizgi (/) ile coğrafi filtreleme uygulayabilir veya dizin yollarını (örneğin, */resimler/ )* belirterek belirli klasörleri seçebilirsiniz. Coğrafi filtrelemeyi tek bir dosyaya da uygulayabilirsiniz (örneğin */resimler/city.png).* Birden çok kurala izin verilir; bir kural girdikten sonra, bir sonraki kuralı girmeniz için boş bir satır görüntülenir.

Örneğin, aşağıdaki dizin yolu filtrelerinin tümü geçerlidir:   
*/*                                 
*/Fotoğraflar/*     
*/Fotoğraflar/Strazburg/*     
*/Fotoğraflar/Strasbourg/city.png*

### <a name="define-the-type-of-action"></a>Eylem türünü tanımlama

**EYLEM** listesinden İzin Ver veya **Engelle'yi** seçin: **Block** 

- **İzin Ver**: Yalnızca belirtilen ülkelerden/bölgelerden kullanıcılar, özyinelemeli yoldan istenen varlıklara erişebilir.

- **Blok**: Belirtilen ülkelerden/bölgelerden gelen kullanıcıların özyinelemeli yoldan istenen varlıklara erişimi engellenir. Bu konum için başka bir ülke/bölge filtreleme seçeneği yapılandırılmamışsa, diğer tüm kullanıcılara erişim izni verilir.

Örneğin, yolu engellemek için bir coğrafi filtreleme kuralı */Fotoğraflar/Strazburg/* aşağıdaki dosyaları filtreler:     
*\//http:\<endpoint>.azureedge.net/Fotoğraflar/Strasbourg/1000.jpg*
*http:\//\<endpoint>.azureedge.net/Fotoğraflar/Strasbourg/Cathedral/1000.jpg*

### <a name="define-the-countriesregions"></a>Ülkeleri/bölgeleri tanımlayın
ÜLKE **KODLARI** listesinden, yolu engellemek veya izin vermek istediğiniz ülkeleri/bölgeleri seçin. 

Ülkeleri/bölgeleri seçmeyi bitirdikten sonra, yeni coğrafi filtreleme kuralını etkinleştirmek için **Kaydet'i** seçin. 

![Coğrafi filtreleme kuralları](./media/cdn-filtering/cdn-geo-filtering-rules.png)

### <a name="clean-up-resources"></a>Kaynakları temizleme
Bir kuralı silmek için, Coğrafi **filtreleme** sayfasındaki listeden seçin ve ardından **Sil'i**seçin.

## <a name="azure-cdn-premium-from-verizon-profiles"></a>Verizon profillerinden Azure CDN Premium
**Verizon profillerinden Azure CDN Premium** için, coğrafi filtreleme kuralı oluşturmak için kullanıcı arabirimi farklıdır:

1. Azure CDN profilinizdeki üst menüden **Yönet'i**seçin.

2. Verizon portalından **HTTP Large'ı**seçin, ardından **Ülke Filtreleme'yi**seçin.

    ![Coğrafi filtreleme standardı](./media/cdn-filtering/cdn-geo-filtering-premium.png)

3. **Ülke Filtresi Ekle'yi**seçin.

    **Adım Bir:** sayfa görüntülenir.

4. Dizin yolunu girin, **Engelle** veya **Ekle'yi**seçin, ardından **İleri'yi**seçin.

    **Adım İki:** sayfa görüntülenir. 

5. Listeden bir veya daha fazla ülke/bölge seçin ve ardından kuralı etkinleştirmek için **Bitir'i** seçin. 
    
    Yeni kural **Ülke Filtreleme** sayfasındaki tabloda görünür.

    ![Coğrafi filtreleme kuralları](./media/cdn-filtering/cdn-geo-filtering-premium-rules.png)

### <a name="clean-up-resources"></a>Kaynakları temizleme
Ülke/bölge filtreleme kuralları tablosunda, silmek için kuralın yanındaki sil simgesini veya değiştirmek için düzen simgesini seçin.

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* Coğrafi filtreleme yapılandırmanızdaki değişiklikler hemen etkili olmaz:
   * **Microsoft’tan Azure CDN Standart** profilleri için yayma işlemi genellikle 10 dakikada tamamlanır. 
   * **Akamai’den Azure CDN Standart** profilleri için yayma işlemi genellikle bir dakika içinde tamamlanır. 
   * **Verizon’dan Azure CDN Standart** ve **Verizon’dan Azure CDN Premium** profilleri için yayma işlemi genellikle 10 dakika içinde tamamlanır. 
 
* Bu özellik joker karakter desteklemez (örneğin, *).

* Göreli yol ile ilişkili coğrafi filtreleme yapılandırması bu yola özyinelemeli olarak uygulanır.

* Aynı göreli yola yalnızca bir kural uygulanabilir. Diğer bir diğer olarak, aynı göreli yolu gösteren birden çok ülke/bölge filtresi oluşturamazsınız. Ancak, ülke/bölge filtreleri özyinelemeli olduğundan, bir klasörün birden çok ülke/bölge filtresi olabilir. Başka bir deyişle, önceden yapılandırılmış bir klasörün alt klasörüne farklı bir ülke/bölge filtresi atanabilir.

* Coğrafi filtreleme özelliği, güvenli bir dizin için isteğe izin verilen veya engellenen ülkeleri/bölgeleri tanımlamak için ülke kodlarını kullanır. Akamai ve Verizon profilleri aynı ülke kodlarının çoğunu desteklemesine rağmen, birkaç fark vardır. Daha fazla bilgi için [Azure CDN ülke kodlarına](/previous-versions/azure/mt761717(v=azure.100))bakın. 

