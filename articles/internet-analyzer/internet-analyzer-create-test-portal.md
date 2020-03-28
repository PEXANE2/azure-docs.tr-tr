---
title: Portal kullanarak Internet Analyzer testi oluşturun | Microsoft Dokümanlar
description: Bu makalede, ilk Internet Analyzer testioluşturmak için nasıl öğrenin.
services: internet-analyzer
author: megan-beatty
ms.service: internet-analyzer
ms.topic: tutorial
ms.date: 10/16/2019
ms.author: mebeatty
ms.openlocfilehash: 64bbed9b558d4c20889b28a5247e2113d20daa77
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73501774"
---
# <a name="create-an-internet-analyzer-test-using-portal-preview"></a>Portal 'ı kullanarak Internet Analyzer testi oluşturma (Önizleme)

Azure portalını kullanarak veya [CLI'yi](internet-analyzer-cli.md)kullanarak Internet Analyzer kaynağı oluşturmanın iki yolu vardır. Bu bölüm, portal deneyimimizi kullanarak yeni bir Azure Internet Çözümleyici kaynağı oluşturmanıza yardımcı olur.

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Başlamadan önce

Genel önizleme genel olarak kullanılabilir; ancak, veri depolama önizleme sırasında *US West 2* ile sınırlıdır.

## <a name="basics"></a>Temel Bilgiler

1. [Azure Internet Analyzer SSS'den](internet-analyzer-faq.md)gelen yönergeleri izleyerek Internet Analyzer önizleme erişimi **alın.**
2. [Azure portalındaki](https://preview.portal.azure.com)ana sayfadan **+ Kaynak oluştur'a**tıklayın. Internet Analyzer şu anda yalnızca Azure portalının önizleme sürümünden edinilebilir.
3. **Yeni** sayfada, Pazar Yeri Arama alanında "Internet Analyzer"ı arayın. *Search the Marketplace*
4. **Internet Analyzer (önizleme)** tıklatın. Yayımcının *Microsoft* olduğundan ve kategorinin *Ağ olduğundan*emin olun.
5. Internet **Analyzer (önizleme)** sayfasında, **Internet Çözümleyicisi oluştur** sayfasını açmak için **Oluştur'u** tıklatın.
6. Internet Analyzer kaynağınız için aşağıdaki yapılandırma ayarlarını belirtin:

    * **Abonelik:** Azure aboneliği, yeni Internet Analyzer kaynağını barındırmak için. ***Önizleme erişimi istemek için kullanılan abonelik kimliğini kullanın.***
    * **Kaynak Grubu:** Yeni Internet Analyzer kaynağının oluşturulacağı Azure kaynak grubu. Varolan bir kaynak grubunuz yoksa, yeni bir kaynak oluşturabilirsiniz.
    * **Adı:** Yeni Internet Analyzer kaynak profilinin adı.
    * **Bölge:** Kaynağın oluşturulacağı Azure ortak bölgesi. Önizleme sırasında yalnızca *US West 2* kullanılabilir.

7. Profil ayarlarınızı belirtmeyi bitirdikten sonra **Gözden Geçir + oluştur'u**tıklatın.

## <a name="configuration"></a>Yapılandırma

Temel adımları tamamlamak, bir testi yapılandırmak ve JavaScript istemcisini katıştırmak için bir ön koşuldur. Bir profil oluşturduktan sonra, ilk testinizi yapılandırmak için **Ayarlar > Yapılandırma'ya** gidin.

1. Testinizin adını **Test Adı** kutusuna verin.
2. **Açıklama** alanına testiniz için bir açıklama ekleyin.
3. **Bitiş Noktasını Yapılandır'ı** tıklatın - sağ taraftan bir sekme görüntülenir. Yapılandırmak istediğiniz bitiş noktası türünü seçin- tek bir Azure bölgesi, birden çok Azure bölgesi veya özel bir bitiş noktası.

    >
    ***Önceden yapılandırılmış uç noktalar: tek ve birden çok Azure bölge birleşimi***
    * Önceden yapılandırılmış bir [Azure uç noktaları listesinden](internet-analyzer-faq.md)bir bölge veya bölge kümesi seçin.
    * Ardından, değerlendirmek istediğiniz uygulama veya içerik teslim mimarisi türünü seçin.
        * Tek Azure bölgesi: Site hızlandırma[(Azure Ön Kapı](https://azure.microsoft.com/services/frontdoor/)), statik içerik önbelleğe alma ([Microsoft için Azure CDN](https://azure.microsoft.com/services/cdn/)), veya hiçbiri.
        * Birden çok Azure bölgesi: Site hızlandırma ([Azure Ön Kapı](https://azure.microsoft.com/services/frontdoor/)), DNS direksiyon ( Azure Trafik[Yöneticisi](https://azure.microsoft.com/services/traffic-manager/))  

    ***Özel uç noktalar***
    * [Özel Bitiş Noktası Oluştur](internet-analyzer-custom-endpoint.md) sayfasındaki yönergeleri izleyin.
    * Portaldaki tek piksellik görüntünün HTTPS URL konumunu yapıştırın.
    >

4. Testinize bitiş noktası eklemek için **Ekle'yi** tıklatın.
5. İkinci bitiş noktanızı yapılandırmak için 1-4 adımlarını yineleyin. Bitiş Noktası B her zaman Uç Nokta A'ya göre ölçülür - uç noktaları yapılandırırken, test denetiminizin hangi uç nokta olması gerektiğini göz önünde bulundurun.
6. Testi kaydetmek için **Kaydet** düğmesini tıklatın. Bir testi kaydettikten sonra, belirli bir testin bitiş noktalarını artık kaldırabilirsiniz.
7. Başlamak istediğiniz test(ler)'i seçin ve **Testi Başlat'ı**tıklatın. ***Bu,*** test(ler) durumunu ***Çalıştırma***olarak değiştirir. Testleri istediğiniz zaman başlatabilirsiniz, ancak testin ölçüm toplamaya başlaması için JavaScript istemcisinin katıştirilmesi gerekir.
8. Herhangi bir noktada daha fazla test ekleyin. Bir test oluşturulana kadar benzersiz JavaScript istemcisinin oluşturulmayacağını unutmayın.

## <a name="embed-client"></a>İstemciyi Göm

Herhangi bir sınama başlatmak için, JavaScript istemcisinin Web uygulamanıza katıştırılmış olması gerekir. En az bir test yapılandırdıktan **sonra, Gözden Geçir + oluştur,** **Ayarlar > Yapılandırma'ya**gidin ve JavaScript istemcisini kopyalayın'ı tıklatın. Embed [Internet Analyzer Client](internet-analyzer-embed-client.md) sayfasında özel talimatlar bulunabilir.  

## <a name="next-steps"></a>Sonraki adımlar

* Internet [Analyzer SSS'yi](internet-analyzer-faq.md) okuyun
* [Internet Analyzer İstemci'sini](internet-analyzer-embed-client.md) katıştırma ve özel bir [bitiş noktası](internet-analyzer-custom-endpoint.md)oluşturma hakkında daha fazla bilgi edinin.
