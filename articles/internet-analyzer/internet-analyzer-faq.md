---
title: İnternet Çözümleyici SSS | Microsoft Dokümanlar
description: Azure Internet Çözümleyicisi için SSS.
services: internet-analyzer
author: diego-perez-botero
ms.service: internet-analyzer
ms.topic: guide
ms.date: 10/16/2019
ms.author: mebeatty
ms.openlocfilehash: a4a5b058666fab3e9048a7d92726dccd1360ff37
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74184263"
---
# <a name="azure-internet-analyzer-faq-preview"></a>Azure Internet Çözümleyicisi SSS (Önizleme)

Bu Azure Internet Analyzer için SSS-ek sorularınız varsa, [geri bildirim forumuna](https://aka.ms/internetAnalyzerFeedbackForum) gidin ve sorunuz gönderin. Bir soru sık sık sorulduğunda, hızlı ve kolay bir şekilde bulunabilmesi için bu makaleye ekliyoruz.

## <a name="how-do-i-participate-in-the-preview"></a>Önizlemeye nasıl katılabilirim?

Önizleme, belirli müşteriler için kullanılabilir. Önizlemeye katılmak istiyorsanız, lütfen aşağıdakileri yapın:

1. [Azure portalında](https://ms.portal.azure.com)oturum açın.
2. **Abonelikler** sayfasına gidin.
3. Internet Analyzer'ı birlikte kullanmayı planladığınız Azure aboneliğini tıklatın.
4. Abonelik için **Kaynak sağlayıcıları** ayarlarına gidin.
5. **Microsoft.Network'ü** arayın ve **Kaydol** (veya **Yeniden Kaydol)** düğmesini tıklatın.
![erişim isteği](./media/ia-faq/request-preview-access.png)

6. Erişim isteğinde bulunmak için kullanılan e-posta adresinizi ve Azure abonelik kimliğinizi bize vererek [onay isteyin.](https://aka.ms/internetAnalyzerContact)
7. İsteğiniz onaylandıktan sonra bir e-posta onayı alırsınız ve yeni izin verilen Azure aboneliğinden Internet Analyzer kaynaklarını oluşturabilir/güncelleyebilir/değiştirebilirsiniz.

## <a name="do-i-need-to-embed-the-client-to-run-a-test"></a>Bir testi çalıştırmak için istemciyi gömmem gerekiyor mu?

Evet, kullanıcılarınıza özel ölçümleri toplamak için Internet Analyzer istemcisinin uygulamanızda yüklenmesi gerekir. [İstemciyi nasıl yükleyin öğrenin.](internet-analyzer-embed-client.md) 

## <a name="do-i-get-billed-for-participating-in-the-preview"></a>Önizlemeye katıldığım için faturalandırılır mıyım?
Hayır, Azure Internet Analyzer önizlemede kullanmakta ücretsizdir. Önizleme sırasında hizmet düzeyi anlaşması yoktur.

## <a name="what-scenarios-is-internet-analyzer-designed-to-address"></a>Internet Analyzer hangi senaryoları ele almak üzere tasarlanmıştır?

Internet Analyzer, kullanıcı popülasyonunuza göre ağ performansı öngörüleri sunmak üzere tasarlanmıştır. Internet Analyzer, kullanıcılarınız için en iyi performans kararlarını vermeye yardımcı olmak için, farklı kullanıcı popülasyonunuzu kullanarak iki Internet uç noktasının performansını karşılaştırır. Internet Analyzer çok sayıda soruyu yanıtlayabilirken, en yaygın sorulardan bazıları şunlardır:

* Buluta geçişin performans etkisi nedir? 
    * *Önerilen Test: Özel (mevcut şirket içi altyapınız) ile Azure (önceden yapılandırılmış bitiş noktası)*
* Verilerimi bir veri merkezine koymanın değeri nedir? 
    *  *Önerilen Test: Azure ve Azure Ön Kapı, Azure ve Azure CDN Microsoft'tan*
* Azure Ön Kapı'nın performans avantajı nedir?
    *  *Önerilen Test: Özel/ Azure/ CDN ve Azure Ön Kapı*
* Azure CDN'nin Microsoft'un performans avantajı nedir? 
    *  *Önerilen Test: Microsoft'tan Özel/ Azure/ AFD ve Azure CDN*
* Microsoft'un Azure CDN'si nasıl birikir? 
    *  *Önerilen Test: Microsoft'tan Özel (diğer CDN bitiş noktası) ve Azure CDN*
* Her bölgedeki son kullanıcı popülasyonunuz için en iyi bulut nedir? 
    *  *Önerilen Test: Özel (diğer bulut hizmeti) ile Azure (önceden yapılandırılmış bitiş noktası)*

## <a name="which-tests-can-i-run-in-preview"></a>Önizlemede hangi testleri çalıştırabilirim?

Internet Analyzer'da oluşturduğunuz her test iki uç noktadan oluşur:Bitiş Noktası A ve Bitiş Noktası B. Aşağıdaki kombinasyonlardan herhangi biri test olarak çalıştırılabilir:  
* Önceden yapılandırılmış iki uç nokta,
* Bir özel ve önceden yapılandırılmış bir bitiş noktası veya
* İki [özel uç nokta](internet-analyzer-custom-endpoint.md) (bir özel bitiş noktası Azure'da olmalıdır).

Önizleme sırasında önceden yapılandırılmış aşağıdaki uç noktalar kullanılabilir:
* **Azure bölgeleri**
    * Güney Brezilya
    * Orta Hindistan
    * Orta ABD
    * Doğu Asya
    * Doğu ABD
    * Batı Japonya
    * Kuzey Avrupa
    * Güney Afrika Kuzey
    * Güneydoğu Asya
    * BAE Kuzey
    * Batı Birleşik Krallık  
    * Batı Avrupa
    * Batı ABD
    * Batı ABD 2
* **Birden çok Azure bölge birleşimi**
    * Doğu ABD, Brezilya Güney
    * Doğu ABD, Doğu Asya
    * Batı Avrupa, Brezilya Güney
    * Batı Avrupa, Güneydoğu Asya
    * Batı Avrupa, BAE Kuzey
    * Batı ABD, Doğu ABD
    * Batı ABD, Batı Avrupa
    * Batı ABD, BAE Kuzey
    * Batı Avrupa, BAE Kuzey, Güneydoğu Asya
    * Batı ABD, Batı Avrupa, Doğu Asya
    * Batı ABD, Kuzey Avrupa, Güneydoğu Asya, BAE Kuzey, Güney Afrika Kuzey 
* **Azure + Azure Ön Kapı** - yukarıda listelenen tek veya birden çok Azure bölge birleşiminde dağıtıldı
* **Microsoft'tan Azure + Azure CDN** - yukarıda listelenen tek bir Azure bölge birleşiminde dağıtıldı
* **Azure + Azure Trafik Yöneticisi** - yukarıda listelenen herhangi bir birden çok Azure bölge birleşiminde dağıtıldı

## <a name="how-is-internet-analyzer-different-from-other-monitoring-services-provided-by-azure"></a>Internet Analyzer'ın Azure tarafından sağlanan diğer izleme hizmetlerinden farkı nedir?

Internet Analyzer, son kullanıcılarınızın performansını anlamanıza yardımcı olur ve performanslarını artırmak için karar vermede yardımcı olur. Diğer Azure izleme araçları Azure hizmetleriniz hakkında öngörüler sağlarken, Internet Analyzer kullanıcılarınız için uçlardan uca Internet performansını ölçmeye odaklanır.

## <a name="how-is-measurement-data-handled-by-internet-analyzer"></a>Ölçüm verileri Internet Analyzer tarafından nasıl işlenir?

Azure [güçlü güvenlik süreçlerine sahiptir ve çok çeşitli uyumluluk standartlarını karşılar.](https://azure.microsoft.com/support/trust-center/) Verilerinize yalnızca siz ve atanmış ekibiniz erişebilir. Microsoft personeli, yalnızca sizin bilginiz dahilinde belirli sınırlı koşullar altında bu bilgilere sınırlı erişime sahip olabilir. Nakil ve istirahatte şifreli.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek için [Internet Analyzer Genel](internet-analyzer-overview.md)Bakış'ımıza bakın.
