---
title: Azure CDN ile dinamik site hızlandırma
description: Azure CDN, dinamik içeriğe sahip dosyalar için dinamik site hızlandırma (DSA) optimizasyonunu destekler.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: allensu
ms.openlocfilehash: 26559adf183a5e008d77b87654a1bd4dabebbca0
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81253841"
---
# <a name="dynamic-site-acceleration-via-azure-cdn"></a>Azure CDN ile dinamik site hızlandırma

Sosyal medya, elektronik ticaret ve hiper-kişiselleştirilmiş web patlama ile, son kullanıcılara sunulan içeriğin hızla artan bir yüzdesi gerçek zamanlı olarak oluşturulur. Kullanıcılar tarayıcılarından, konumlarından, aygıtlarından veya ağlarından bağımsız olarak hızlı, güvenilir ve kişiselleştirilmiş bir web deneyimi beklerler. Ancak, bu deneyimleri bu kadar ilgi çekici hale çok yenilikler de yavaş sayfa indirme ve risk tüketici deneyimi nin kalitesini koymak. 

Standart içerik teslim ağı (CDN) özelliği, statik dosyaların teslimini hızlandırmak için dosyaları son kullanıcılara daha yakın önbelleğe alabilme özelliğini içerir. Ancak, dinamik web uygulamalarıyla, sunucu kullanıcı davranışına yanıt olarak içeriği oluşturduğundan, kenar konumlarında bu içeriğin önbelleğe alınmış olması mümkün değildir. Bu tür içeriğin teslimini hızlandırmak geleneksel kenar önbelleğe almaktan daha karmaşıktır ve başlangıçtan teslimata kadar tüm veri yolu boyunca her öğeyi ince bir şekilde ayarlayan uçtan uca bir çözüm gerektirir. Azure CDN dinamik site hızlandırma (DSA) optimizasyonu ile dinamik içeriğe sahip web sayfalarının performansı ölçülebilir şekilde artırılır.

**Akamai'den Azure CDN** ve **Verizon'dan Azure CDN,** uç nokta oluşturma sırasında **Optimize edilmiş** menü aracılığıyla DSA optimizasyonu sunar. Microsoft'tan dinamik site [ivmesi Azure Ön Kapı Hizmeti](https://docs.microsoft.com/azure/frontdoor/front-door-overview)ile sunulmaktadır.

> [!Important]
> **Akamai profillerinden Azure CDN** için, oluşturulduktan sonra bir CDN bitiş noktasının optimizasyonunu değiştirebilirsiniz.
>   
> **Verizon Azure CDN** profilleri için, bir CDN uç noktası oluşturulduktan sonra uç nokta iyileştirmesini değiştiremezsiniz.

## <a name="cdn-endpoint-configuration-to-accelerate-delivery-of-dynamic-files"></a>Dinamik dosyaların teslimini hızlandırmak için CDN uç nokta yapılandırması

Dinamik dosyaların teslimini en iyi duruma getirmek için bir CDN bitiş noktası yapılandırmak için, aynı şeyi programlı olarak yapmak için Azure portalını, REST API'lerini veya istemci SDK'larından herhangi birini kullanabilirsiniz. 

**Azure portalını kullanarak DSA optimizasyonu için bir CDN bitiş noktası yapılandırmak için:**

1. **CDN profil** sayfasında **Bitiş Noktası'nı**seçin.

   ![Yeni bir CDN bitiş noktası ekleme](./media/cdn-dynamic-site-acceleration/cdn-endpoint-profile.png) 

   **Uç nokta ekleyin** bölmesi görünür.

2. **Için Optimize Edilmiş**altında, Dinamik site **ivmeseçin.**

    ![DSA ile yeni bir CDN bitiş noktası oluşturma](./media/cdn-dynamic-site-acceleration/cdn-endpoint-dsa.png)

3. **Sonda yolu**için, bir dosyaya geçerli bir yol girin.

    Sonda yolu DSA'ya özgü bir özelliktir ve oluşturma için geçerli bir yol gereklidir. DSA, CDN için ağ yönlendirme yapılandırmalarını optimize etmek için kaynak sunucuya yerleştirilen küçük bir *sonda yolu* dosyası kullanır. Sonda yolu dosyası için örnek dosyayı indirip sitenize yükleyebilir veya kaynağınızda yaklaşık 10 KB boyutunda olan varolan bir varlığı kullanabilirsiniz.

4. Diğer gerekli bitiş noktası seçeneklerini girin (daha fazla bilgi için bkz. [yeni CDN bitiş noktası oluştur),](cdn-create-new-endpoint.md#create-a-new-cdn-endpoint)sonra **Ekle'yi**seçin.

   CDN bitiş noktası oluşturulduktan sonra, belirli ölçütlerle eşleşen tüm dosyalar için DSA optimizasyonları uygular. 


**DSA için varolan bir bitiş noktasını yapılandırmak için (yalnızca Akamai profillerinden Azure CDN):**

1. **CDN profil** sayfasında, değiştirmek istediğiniz bitiş noktasını seçin.

2. Sol bölmeden **Optimizasyon'u**seçin. 

   **Optimizasyon** sayfası görüntülenir.

3. **Için Optimize Edilmiş**, Dinamik site **ivmeseçin,** sonra **Kaydet'i**seçin.

> [!Note]
> DSA ek ücrete tabidir. Daha fazla bilgi için [İçerik Dağıtım Ağı fiyatlandırması](https://azure.microsoft.com/pricing/details/cdn/)için bkz.

## <a name="dsa-optimization-using-azure-cdn"></a>Azure CDN kullanarak DSA Optimizasyonu

Azure CDN'deki Dinamik Site Hızlandırma, aşağıdaki teknikleri kullanarak dinamik varlıkların teslimini hızlandırAr:

-   [Rota optimizasyonu](#route-optimization)
-   [TCP optimizasyonları](#tcp-optimizations)
-   [Nesne ön alma (Yalnızca Akamai'den Azure CDN)](#object-prefetch-azure-cdn-from-akamai-only)
-   [Uyarlanabilir görüntü sıkıştırma (Yalnızca Akamai'den Azure CDN)](#adaptive-image-compression-azure-cdn-from-akamai-only)

### <a name="route-optimization"></a>Rota Optimizasyonu

Rota optimizasyonu önemlidir, çünkü Internet, trafiğin ve geçici kesintilerin ağ topolojisini sürekli olarak değiştirdiği dinamik bir yerdir. Border Gateway Protocol (BGP), Internet'in yönlendirme protokolüdür, ancak aracı Lık Noktası (PoP) sunucuları aracılığıyla daha hızlı yollar olabilir. 

Rota optimizasyonu, bir sitenin sürekli olarak erişilebilir olması ve dinamik içeriğin mümkün olan en hızlı ve en güvenilir rota üzerinden son kullanıcılara iletilmesi için menşee giden en uygun yolu seçer. 

Akamai ağı, gerçek zamanlı veri toplamak ve Akamai sunucusundaki farklı düğümler üzerinden çeşitli yolları karşılaştırmak için teknikler ve kaynak ile CDN kenarı arasındaki en hızlı rotayı belirlemek için açık Internet üzerinden varsayılan BGP rotasını kullanır. Bu teknikler Internet tıkanıklık noktaları ve uzun yolları önlemek. 

Benzer şekilde, Verizon ağı, istemciden menşee en iyi veri yönlendirmek için en iyi ağ geçitlerini belirlemek için Anycast DNS, yüksek kapasiteli destek POP'ları ve sistem durumu denetimlerinin bir birleşimini kullanır.
 
Sonuç olarak, tam dinamik ve işlemsel içerik, önbelleğe alınamamış olsa bile son kullanıcılara daha hızlı ve daha güvenilir bir şekilde teslim edilir. 

### <a name="tcp-optimizations"></a>TCP Optimizasyonları

İletim Kontrol Protokolü (TCP), IP ağındaki uygulamalar arasında bilgi sağlamak için kullanılan Internet protokol paketinin standardıdır.  Varsayılan olarak, bir TCP bağlantısı kurmak için birkaç ileri ve geri istek ve ağ tıkanıklıklarını önlemek için sınırlar gereklidir ve bu da ölçekte verimsizliklere neden olur. **Akamai'den Azure CDN** bu sorunu üç alanda en iyi duruma alarak işler: 

 - [TCP yavaş başlangıcı ortadan kaldırma](#eliminating-tcp-slow-start)
 - [Kalıcı bağlantılardan yararlanma](#leveraging-persistent-connections)
 - [TCP paket parametrelerini tuning](#tuning-tcp-packet-parameters)

#### <a name="eliminating-tcp-slow-start"></a>TCP yavaş başlangıcı ortadan kaldırma

TCP *yavaş başlatma,* ağ üzerinden gönderilen veri miktarını sınırlayarak ağ tıkanıklığını önleyen TCP protokolünün bir algoritmasıdır. En fazla ulaşılana veya paket kaybı algılanıncaya kadar gönderen ve alıcı arasındaki küçük tıkanıklık penceresi boyutlarıyla başlar.

 **Akamai'nin** hem de **Verizon profillerinden Azure CDN'nin** Azure CDN'i, TCP'nin aşağıdaki üç adımla yavaş başlamasını ortadan kaldırır:

1. Kenar PoP sunucuları arasındaki bağlantıların bant genişliğini ölçmek için sistem durumu ve bant genişliği izleme kullanılır.
    
2. Ölçümler kenar PoP sunucuları arasında paylaşılır, böylece her sunucu ağ koşullarından ve çevrelerindeki diğer PoP'ların sunucu sağlığından haberdar olur.  
    
3. CDN kenar sunucuları, yakınlığındaki diğer CDN kenar sunucularıyla iletişim kurarken en uygun pencere boyutunun ne olması gerektiği gibi bazı aktarım parametreleri hakkında varsayımlarda bulunur. Bu adım, CDN kenar sunucuları arasındaki bağlantının durumu daha yüksek paket veri aktarımları yeteneğine sahipse, ilk tıkanıklık penceresi boyutunun artırılabildiği anlamına gelir.  

#### <a name="leveraging-persistent-connections"></a>Kalıcı bağlantılardan yararlanma

CDN kullanarak, doğrudan kaynağınıza bağlanan kullanıcılarla karşılaştırıldığında, doğrudan başlangıç sunucunuza daha az benzersiz makine bağlanır. Azure CDN ayrıca, kullanıcı isteklerini bir araya getirarak kaynakla daha az bağlantı kurmayı da sağlar.

Daha önce de belirtildiği gibi, bir TCP bağlantısı kurmak için birkaç el sıkışma isteği gereklidir. `Keep-Alive` HTTP üstbilgitarafından uygulanan kalıcı bağlantılar, gidiş-dönüş sürelerinden tasarruf etmek ve teslimatı hızlandırmak için birden çok HTTP isteği için varolan TCP bağlantılarını yeniden kullanır. 

**Verizon'daki Azure CDN,** açık bir bağlantının kapatılmasını önlemek için TCP bağlantısı üzerinden düzenli olarak canlı tutulan paketler gönderir.

#### <a name="tuning-tcp-packet-parameters"></a>TCP paket parametrelerini tuning

**Akamai'den Azure CDN,** sunucudan sunucuya bağlantıları yöneten parametreleri inceler ve siteye katıştıedilen içeriği almak için gereken uzun mesafeli tur yolculuklarının miktarını aşağıdaki teknikleri kullanarak azaltır:

- Daha fazla paket onay beklemeden gönderilebilir böylece ilk tıkanıklık penceresi artırılması.
- Bir kayıp algılansın ve yeniden iletim daha hızlı gerçekleşir, böylece ilk yeniden iletim zaman dışarısı azalır.
- Paketlerin iletimde kaybolduğunu varsaymadan önce bekleme süresini azaltmak için minimum ve maksimum yeniden iletim süresini azaltmak.

### <a name="object-prefetch-azure-cdn-from-akamai-only"></a>Nesne ön alma (Yalnızca Akamai'den Azure CDN)

Web sitelerinin çoğu, resimler ve komut dosyaları gibi çeşitli kaynaklara başvuran bir HTML sayfasından oluşur. Genellikle, bir istemci bir web sayfası istediğinde, tarayıcı önce HTML nesnesini karşıdan yükler ve ayrıştırır ve ardından sayfayı tam olarak yüklemek için gereken bağlantılı varlıklara ek isteklerde bulundur. 

*Prefetch,* HTML tarayıcıya sunulurken ve tarayıcı bu nesne isteklerini yapmadan önce HTML sayfasına katıştırılmış görüntüleri ve komut dosyalarını almak için bir tekniktir. 

CDN'nin HTML temel sayfasını istemcinin tarayıcısına sunduğu anda ön getirme seçeneği açıkken, CDN HTML dosyasını ayrıştırır ve bağlantılı kaynaklar için ek isteklerde bulunarak önbelleğinde saklar. İstemci bağlı varlıklar için isteklerde bulununca, CDN kenar sunucusu zaten istenen nesnelere sahiptir ve kaynağına gidiş-dönüş olmadan bunları hemen hizmet edebilir. Bu en iyi duruma getirilmesi hem önbelleğe çıkarılabilir hem de önbelleğe alamayan içeriğe yarar sağlar.

### <a name="adaptive-image-compression-azure-cdn-from-akamai-only"></a>Uyarlanabilir görüntü sıkıştırma (Yalnızca Akamai'den Azure CDN)

Bazı cihazlar, özellikle mobil cihazlar, zaman zaman daha yavaş ağ hızları yaşar. Bu senaryolarda, kullanıcının tam çözünürlüklü görüntüler için uzun süre beklemekyerine web sayfalarında daha hızlı küçük görüntüler alması daha yararlıdır.

Bu özellik ağ kalitesini otomatik olarak izler ve ağ hızları teslimat süresini artırmak için daha yavaş olduğunda standart JPEG sıkıştırma yöntemlerikullanır.

Adaptif Görüntü Sıkıştırma | Dosya Uzantıları  
--- | ---  
JPEG sıkıştırma | .jpg, .jpeg, .jpe, .jig, .jgig, .jgi

## <a name="caching"></a>Önbelleğe alma

DSA ile önbelleğe alma, başlangıç yanıtında `Cache-Control` veya `Expires` üstbilgiiçerse bile CDN'de varsayılan olarak kapatılır. DSA genellikle, her istemciye özgü olduğu için önbelleğe alınmaması gereken dinamik varlıklar için kullanılır. Önbelleğe alma bu davranışı bozabilir.

Statik ve dinamik varlıkların bir karışımı ile bir web sitesi varsa, en iyi performansı elde etmek için hibrid bir yaklaşım almak en iyisidir. 

**Akamai profillerinden** Verizon ve Azure CDN **Standard'dan Azure CDN Standardı** için [önbelleğe alma kurallarını](cdn-caching-rules.md)kullanarak belirli DSA uç noktaları için önbelleğe alma açabilirsiniz.

Önbelleğe alma kurallarına erişmek için:

1. **CDN profil** sayfasından, ayarlar altında **Önbelleğe Alma kurallarını**seçin.  
    
    ![CDN Önbelleğe alma kuralları düğmesi](./media/cdn-dynamic-site-acceleration/cdn-caching-rules-btn.png)

    **Önbelleğe Alma kuralları** sayfası açılır.

2. DSA bitiş noktanız için önbelleğe alma açmak için genel veya özel önbelleğe alma kuralı oluşturun. 

**Yalnızca Verizon profillerinden Azure CDN Premium** için, kurallar altyapısını kullanarak belirli DSA uç noktaları için önbelleğe alma'yı açarsınız. [rules engine](cdn-rules-engine.md) Oluşturulan tüm kurallar, profilinizin yalnızca DSA için optimize edilmiş uç noktalarını etkiler. 

Kurallar motoruna erişmek için:
    
1. **CDN profil** sayfasından **Yönet'i**seçin.  
    
    ![CDN profil yönetme düğmesi](./media/cdn-dynamic-site-acceleration/cdn-manage-btn.png)

    CDN yönetim portalı açılır.

2. CDN yönetim portalından **ADN'yi**seçin, ardından **Kurallar Motoru'nu**seçin. 

    ![DSA için kurallar motoru](./media/cdn-dynamic-site-acceleration/cdn-dsa-rules-engine.png)



Alternatif olarak, iki CDN uç noktası kullanabilirsiniz: dinamik varlıkları sunmak için DSA ile optimize edilmiş bir uç nokta ve genel web teslimi gibi statik optimizasyon türüyle optimize edilmiş başka bir uç nokta, önbelleğe alınabilir varlıklara. Web sayfası URL'lerinizi, kullanmayı planladığınız CDN bitiş noktasındaki varlığa doğrudan bağlantı vermek için değiştirin. 

Örneğin: `mydynamic.azureedge.net/index.html` dinamik bir sayfadır ve DSA bitiş noktasından yüklenir.Html sayfası, JavaScript kitaplıkları veya statik CDN bitiş noktasından yüklenen görüntüler `mystatic.azureedge.net/banner.jpg` `mystatic.azureedge.net/scripts.js`gibi birden çok statik varlıktan başvurur. 



