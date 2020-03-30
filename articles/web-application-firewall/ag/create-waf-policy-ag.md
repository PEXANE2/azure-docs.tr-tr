---
title: Uygulama Ağ Geçidi için Web Uygulaması Güvenlik Duvarı (WAF) ilkeleri oluşturma
description: Uygulama Ağ Geçidi için Web Uygulaması Güvenlik Duvarı ilkelerini nasıl oluşturabilirsiniz öğrenin.
services: web-application-firewall
ms.topic: conceptual
author: vhorne
ms.service: web-application-firewall
ms.date: 02/08/2020
ms.author: victorh
ms.openlocfilehash: 3e8cd2f1e594cd6a60296b2df135f275641df313
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77086973"
---
# <a name="create-web-application-firewall-policies-for-application-gateway"></a>Uygulama Ağ Geçidi için Web Uygulaması Güvenlik Duvarı ilkeleri oluşturma

Bir WAF ilkesini dinleyicilerle ilişkilendirmek, tek bir WAF'ın arkasındaki birden çok sitenin farklı ilkelertarafından korunmasına olanak tanır. Örneğin, WAF'Nizin arkasında beş site varsa, diğer dördünden etkilenmeden bir site için dışlamaları, özel kuralları ve yönetilen kural kümelerini özelleştirmek için beş ayrı WAF politikasına (her dinleyici için bir tane) sahip olabilirsiniz. Tüm sitelere tek bir ilkenin uygulanmasını istiyorsanız, politikayı tüm genel olarak geçerli kılmak için tek tek dinleyiciler yerine Uygulama Ağ Geçidi ile ilişkilendirebilirsiniz. İlkeler, yol tabanlı yönlendirme kuralına da uygulanabilir. 

İstediğiniz kadar ilke yapabilirsiniz. Bir ilke oluşturduktan sonra, yürürlüğe girmek için bir Uygulama Ağ Geçidi ile ilişkili olması gerekir, ancak Uygulama Ağ Geçitleri ve dinleyicilerin herhangi bir birleşimi ile ilişkili olabilir. 

Uygulama Ağ Geçidinizde bir ilke uygulanırsa ve bu Uygulama Ağ Geçidi'ndeki bir dinleyiciye farklı bir ilke uygularsanız, dinleyicinin ilkesi yalnızca atandıkları dinleyici(ler) için geçerli olur. Uygulama Ağ Geçidi ilkesi, kendilerine atanmış belirli bir ilkesi olmayan diğer tüm dinleyiciler için de geçerlidir. 

   > [!NOTE]
   > Site başına ve URI BAŞıNA WAF İlkeleri Genel Önizleme'dedir. Bu, bu özelliğin Microsoft'un Ek Kullanım Koşullarına tabi olduğu anlamına gelir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Tüm yeni Web Uygulaması Güvenlik Duvarı'nın WAF ayarları (özel kurallar, yönetilen rulset yapılandırmaları, dışlamalar, vb.) bir WAF Politikası'nın içinde yaşar. Varolan bir WAF'ınız varsa, bu ayarlar WAF config'inizde hala bulunabilir. Yeni WAF İlkesine nasıl taşınış edileilgili adımlar için, [waf Config'inizi](#migrate) bu makalenin ilerleyen saatlerinde bir WAF Politikasına geçirin' e bakın. 

## <a name="create-a-policy"></a>İlke oluşturma

İlk olarak, Azure portalını kullanarak yönetilen varsayılan kural kümesi (DRS) ile temel bir WAF ilkesi oluşturun.

1. Portalın sol üst tarafında **kaynak oluştur'u**seçin. **WAF'ı**arayın, **Web Uygulaması Güvenlik Duvarı'nı**seçin ve ardından **Oluştur'u**seçin.
2. **WAF ilke sayfası oluştur'da,** **Temel ler** sekmesini girin veya aşağıdaki bilgileri seçin, kalan ayarlar için varsayılanları kabul edin ve sonra Gözden Geçir + oluştur seçeneğini **belirleyin:**

   |Ayar  |Değer  |
   |---------|---------|
   |İlke için     |Bölgesel WAF (Uygulama Ağ Geçidi)|
   |Abonelik     |Abonelik adınızı seçin|
   |Kaynak grubu     |Kaynak grubunuzu seçin|
   |İlke adı     |WAF politikanız için benzersiz bir ad yazın.|
3. **İlişkilendirme** sekmesinde, aşağıdaki ayarlardan birini girin ve sonra **Ekle'yi**seçin:

   |Ayar  |Değer  |
   |---------|---------|
   |Ortak Başvuru Ağ Geçidi     |Uygulama Ağ Geçidi profil adınızı seçin.|
   |Yardımcı Dinleyiciler     |Uygulama Ağ Geçidi Dinleyicinizin adını seçin ve sonra **Ekle'yi**seçin.|

   > [!NOTE]
   > Uygulama Ağ Geçidinize (veya dinleyicinize) zaten bir ilke olan bir ilke atarsanız, özgün ilke üzerine yazılır ve yeni ilke yle değiştirilir.
4. **Gözden Geçir + oluştur'u**seçin, ardından **Oluştur'u**seçin.

   ![WAF politikasıtemelleri](../media/create-waf-policy-ag/waf-policy-basics.png)

## <a name="configure-waf-rules-optional"></a>WAF kurallarını yapılandırma (isteğe bağlı)

Bir WAF ilkesi oluşturduğunuzda, varsayılan olarak *Algılama* modundadır. Algılama modunda, WAF herhangi bir isteği engellemez. Bunun yerine, eşleşen WAF kuralları WAF günlüklerinde günlüğe kaydedilir. WAF'ı iş başında görmek için mod ayarlarını *Önleme*olarak değiştirebilirsiniz. Önleme modunda, seçtiğiniz CRS Kural Kümesi'nde tanımlanan eşleşen kurallar engellenir ve/veya WAF günlüklerinde oturum açar.

## <a name="managed-rules"></a>Yönetilen kurallar

Azure tarafından yönetilen OWASP kuralları varsayılan olarak etkinleştirilir. Kural grubu içindeki tek bir kuralı devre dışı bırakıp, bu kural grubu içindeki kuralları genişletin, kural numarasının önündeki onay kutusunu seçin ve yukarıdaki sekmede **Devre Dışı Nı** seçin.

[![Yönetilen](../media/create-waf-policy-ag/managed-rules.png) kurallar](../media/create-waf-policy-ag/managed-rules-lrg.png#lightbox)

## <a name="custom-rules"></a>Özel kurallar

Özel bir kural oluşturmak **için, Özel kurallar** sekmesialtında **Özel Kural Ekle'yi** seçin. Bu, özel kural yapılandırma sayfasını açar. Aşağıdaki ekran görüntüsü, sorgu dizesi metin *blokme*içeriyorsa, isteği engellemek için yapılandırılan örnek bir özel kural gösterir.

[![Özel kuralı](../media/create-waf-policy-ag/edit-custom-rule.png) nı edin](../media/create-waf-policy-ag/edit-custom-rule-lrg.png#lightbox)

## <a name="migrate-your-waf-config-to-a-waf-policy"></a><a name="migrate"></a>WAF Config'inizi bir WAF Politikasına geçirin

Varolan bir WAF'Niz varsa, portalda bazı değişiklikler fark etmiş olabilirsiniz. Öncelikle WAF'ınızda ne tür bir İlke etkinleştirdiğinizi belirlemeniz gerekir. Üç potansiyel durum vardır:

- WAF Politikası Yok
- Özel Kurallar yalnızca İlke
- WAF Politikası

Portalda bakarak WAF'ınızın hangi eyalette olduğunu anlayabilirsiniz. WAF ayarları görünürse ve Uygulama Ağ Geçidi görünümü içinden değiştirilebiliyorsa, WAF'ınız durum 1'dedir.

[![WAF](../media/create-waf-policy-ag/waf-configure.png) yapılandırması](../media/create-waf-policy-ag/waf-configure-lrg.png#lightbox)

**Web Uygulaması Güvenlik Duvarı'nı** seçerseniz ve size ilişkili bir ilke gösterirse, WAF durum 2'de veya durum 3'tedir. İlkede gezindikten sonra, **yalnızca** özel kuralları ve İlişkili Uygulama Ağ Geçitleri gösteriyorsa, yalnızca Özel Kurallar İlkesi'dir.

![WAF özel kuralları](../media/create-waf-policy-ag/waf-custom-rules.png)

İlke Ayarları ve Yönetilen Kurallar da gösteriyorsa, tam bir Web Uygulaması Güvenlik Duvarı ilkesidir. 

![WAF ilke ayarları](../media/create-waf-policy-ag/waf-policy-settings.png)

## <a name="migrate-to-waf-policy"></a>WAF İlkesine Geçiş

Özel Kurallar yalnızca WAF İlkesi varsa, yeni WAF İlkesine geçmek isteyebilirsiniz. İleriye dönük olarak, güvenlik duvarı ilkesi WAF ilke ayarlarını, yönetilen kural kümelerini, hariç tutmaları ve devre dışı bırakılmış kural gruplarını destekler. Esasen, daha önce Uygulama Ağ Geçidi içinde yapılan tüm WAF yapılandırmaları şimdi WAF Politikası üzerinden yapılır. 

Özel kurala yönelik yapılan lar yalnızca WAF ilkesi devre dışı bırakılır. Kuralları devre dışı bırakma, dışlamaekleme vb. gibi WAF ayarlarını yeniden atamak için yeni bir üst düzey güvenlik duvarı ilkesi kaynağına geçiş yapmanız gerekir.

Bunu yapmak için bir *Web Uygulaması Güvenlik Duvarı İlkesi* oluşturun ve uygulama ağ geçidi(ler) ve dinleyici(ler) ile ilişkilendirin. Bu yeni İlke, her özel kural, dışlama, devre dışı bırakma kuralı vb. oluşturmakta olduğunuz yeni İlke'ye kopyalanması gerektiği anlamına gelen geçerli WAF config ile tam olarak aynı **olmalıdır.** Uygulama Ağ Geçidinizle ilişkili bir Politikanız olduktan sonra WAF kurallarınızda ve ayarlarınızda değişiklik yapmaya devam edebilirsiniz. Bunu Azure PowerShell ile de yapabilirsiniz. Daha fazla bilgi için [waf ilkesini varolan bir Uygulama Ağ Geçidi ile ilişkilendirme bölümüne](associate-waf-policy-existing-gateway.md)bakın.

İsteğe bağlı olarak, bir WAF ilkesine geçiş yapmak için bir geçiş komut dosyası kullanabilirsiniz. Daha fazla bilgi için Azure [PowerShell'i kullanarak Web Uygulaması Güvenlik Duvarı ilkelerini geçirin.](migrate-policy.md)

## <a name="next-steps"></a>Sonraki adımlar

[Web Uygulaması Güvenlik Duvarı CRS kural grupları ve kuralları](application-gateway-crs-rulegroups-rules.md)hakkında daha fazla bilgi edinin.
