---
title: Microsoft eşleme ilkesi
titleSuffix: Azure
description: Microsoft eşleme ilkesi
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: conceptual
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: a683ad71f5e80c91728262dc7bbabf36e9d68deb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75775231"
---
# <a name="peering-policy"></a>Eşleme ilkesi
Microsoft 'un ağınız için genel gereksinimleri aşağıdaki bölümlerde açıklanmıştır. Bunlar hem doğrudan eşleme hem de Exchange eşleme istekleri için geçerlidir.

## <a name="technical-requirements"></a>Teknik gereksinimler

* Sıkışıklık olmadan trafiği değiş tokuş etmek için yeterli kapasiteye sahip tamamen yedekli bir ağ.
* Eşin ortak yönlendirilebilir bir otonom sistem numarası (ASN) olacaktır.
* Hem IPv4 hem de IPv6 desteklenir ve Microsoft her eşleme konumunda her iki türde oturum kurmayı bekler.
* MD5 desteklenmez.
* **ASN ayrıntıları:**
    * Microsoft, AS8075 ve AS8068, AS8069, AS12076 ile birlikte şunları yönetir. AS8075 eşlemesi olan ASNs 'lerin tam listesi için, MICROSOFT tarafından ayarlanan MICROSOFT.
    * Tüm taraflar Microsoft ile eşleme, herhangi bir koşullarda AS12076 (Express Route) kaynağından gelen yolları kabul etmemelidir ve tüm eşlere AS12076 filtrelemelidir.
* **Yönlendirme ilkesi:**
    * Eş, en az bir ortak yönlendirilebilir/24 olur.
    * Microsoft, alınan çok çıkış ayrımlarını (MED) üzerine yazacak.
    * Microsoft, rota almamayı göstermek için eşlerden BGP topluluk etiketleri almayı tercih eder.
    * Eşler, filtre uygulamak amacıyla yollarını ortak bir Internet yönlendirme kayıt defteri (IÇ_VERIM_ORANı) veritabanına kaydetmelerinin ve bu bilgilerin güncel tutulması için iyi Fai çabalara sahip olması beklenir.
    * Eşler, Microsoft ile eşleme oturumlarında 1000 (IPv4) ve 100 (IPv6) yollarının en yüksek ön eki olarak ayarlandığını öneriyor.
    * Özellikle de önceden kabul etmediğiniz müddetçe, eşler Microsoft ile meslektaşlarındaki tüm konumlarda tutarlı yollar duyurmaları beklenir.
    * Genel olarak, AS8075 ile eşleme oturumları, tüm MICROSOFT yollarını duyuracaktır. AS8075 birbirine bağlı olarak, Afrika ve Asya 'daki bağlantılar ilgili bölgeyle ilgili olan yollarla sınırlandırılabilir.
    * Taraf, bir statik yol, en son çare bir yol veya diğer tarafa BGP aracılığıyla duyurulmayan bir yolun diğer tarafına trafik göndermeyecektir.
    * Eşler, rota güvenliği için [Manrs](https://www.manrs.org/) sektör standartlarına bağlı kalmayı bekliyordu.

## <a name="operational-requirements"></a>İşletimsel gereksinimler
* Tüm teknik ve performans sorunlarını, güvenlik ihlallerini, hizmet reddi saldırılarını veya eşler ya da müşterileri içindeki başka bir kötüye kullanımı ele almak için, tam olarak bir çok 7/24 ağ Işlem Merkezi (NOC).
* Eşler, şirket etki alanı ve telefon numarasından 7/24 NOC e-postası dahil olmak üzere [Peeringdb](https://www.peeringdb.com) üzerinde tam ve güncel bir profile sahip olması beklenir. Bu bilgileri, NOC bilgileri, teknik iletişim bilgileri ve eşleme tesislerinde bulunan bu kişilerin varlığı gibi eşdüzey ayrıntılarını doğrulamak için kullanırız. Kişisel Yahoo, Gmail ve hotmail hesapları kabul edilmez.

## <a name="physical-connection-requirements"></a>Fiziksel bağlantı gereksinimleri
* Doğrudan eşleme veya Exchange eşlemesi için Microsoft ile bağlantı oluşturabileceğiniz konumlar, [Peeringdb](https://www.peeringdb.com/net/694) 'de listelenir
* **Exchange eşlemesi:**
    * İç bağlantı, uygun 10 Gbps optik kullanılarak tek modlu fiber üzerinde olmalıdır.
    * Yoğun kullanım %50 ' ü aştığında eşler bağlantı noktalarını yükseltmesi beklenir.
* **Doğrudan eşleme:**
    * İç bağlantı, uygun 10 Gbps veya 100Gbps optik kullanan tek modlu fiber üzerinde olmalıdır.
    * Microsoft, yalnızca ISS veya ağ hizmeti sağlayıcılarıyla doğrudan eşleme oluşturacak.
    * Yoğun kullanım %50 ' i aştığında ve her bir metro 'da tek bir konum içinde veya bir metro 'daki çeşitli konumlarda farklı kapasiteyi korumadığında eşler bağlantı noktalarını yükseltmesi beklenir.
    * Her bir doğrudan eşleme, eşin kenarında bulunan eşler arası yönlendiricilerden iki Microsoft Edge yönlendiricilerine iki bağlantılardan oluşur. Microsoft bu bağlantılar genelinde iki BGP oturumu gerektirir. Eş, yedekli cihazları kendi sonunda dağıtmayabilir.

## <a name="traffic-requirements"></a>Trafik gereksinimleri
* Exchange eşlemesi üzerindeki eşler en az 200 MB trafiğe ve 2 GB 'den az olmalıdır.  2Gb doğrudan eşlemeyi aşan trafik için gözden geçirilmesi gerekir.
* Doğrudan eşleme için, ağınızdan Microsoft 'a giden trafiğin en düşük gereksinimi karşılamalıdır.

    | Coğrafi Bölge                      | En düşük Microsoft trafik   |
    | :----------------------- |:-------------------------------|
    | Afrika                   | 500 Mbps                       |
    | APAC (Hindistan hariç)      |   2 Gbps                       |
    | APAC (yalnızca Hindistan)        | 500 Mbps                       |
    | Avrupa                   |   2 Gbps                       |
    | LATAM                    |   2 Gbps                       |
    | Orta Doğu              | 500 Mbps                       |
    | NA                       |   2 Gbps                       |

* **Lılık**
    * Yani, Avrupa, APAC ve LATAM ' de, mümkünse en az üç coğrafi olarak farklı konumda bağlantı sağlar ve her bir metro içinde yük devretmeye izin vermek için farklı kapasiteyi koruyun.
    * Afrika, Orta Doğu ve Hindistan 'da mümkün olduğunca birçok farklı konumda bağlantı kurulabilir. Trafiğin bölgede kalmasını sağlamak için yeterli miktarda kapasite saklanması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* Microsoft ile doğrudan eşleme ayarlama adımları hakkında bilgi edinmek için [doğrudan eşleme](walkthrough-direct-all.md)Kılavuzu ' nu izleyin.
* Exchange eşlemesini Microsoft ile ayarlamaya yönelik adımlar hakkında bilgi edinmek için [Exchange eşleme](walkthrough-exchange-all.md)Kılavuzu ' nu izleyin.