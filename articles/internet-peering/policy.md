---
title: Microsoft bakış ilkesi
titleSuffix: Azure
description: Microsoft bakış ilkesi
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: conceptual
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: a683ad71f5e80c91728262dc7bbabf36e9d68deb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75775231"
---
# <a name="peering-policy"></a>Eşleme ilkesi
Microsoft'un ağınızdaki genel gereksinimleri aşağıdaki bölümlerde açıklanmıştır. Bunlar hem Doğrudan bakış hem de Exchange bakış istekleri için geçerlidir.

## <a name="technical-requirements"></a>Teknik gereksinimler

* Trafik sıkışıklığı olmadan trafik değişimi için yeterli kapasiteye sahip tamamen yedekli ağ.
* Peer'in halka açık bir Özerk Sistem Numarası (ASN) olacaktır.
* Hem IPv4 hem de IPv6 desteklenir ve Microsoft her bir eşleme konumunda her iki türde oturumlar oluşturmayı bekler.
* MD5 desteklenmez.
* **ASN ayrıntıları:**
    * Microsoft, AS8075'i aşağıdaki ASN'lerle birlikte yönetir: AS8068, AS8069, AS12076. AS8075 görünümlü ASN'lerin tam listesi için AS-SET MICROSOFT'a başvurun.
    * Microsoft'a bakan tüm taraflar hiçbir koşulda AS12076(Ekspres Rota) yollarını kabul etmemeyi kabul eder ve AS12076'yı tüm eşlere filtrelemelidir.
* **Yönlendirme ilkesi:**
    * Peer en az bir kamuya routable / 24 olacaktır.
    * Microsoft, alınan Multi-Exit Ayırıcılar (MED) üzerine yazacaktır.
    * Microsoft, rota kaynağını belirtmek için eşlerden BGP topluluk etiketleri almayı tercih eder.
    * Peer'in rotalarını filtreleme amacıyla genel bir Internet Yönlendirme Kayıt Defteri (IRR) veritabanına kaydetmesi beklenir ve bu bilgileri güncel tutmak için iyi niyetli çaba gösterecektir.
    * Eşarkadaşlarının Microsoft ile eşleme oturumları için 1000 (IPv4) ve 100 (IPv6) rotanın en üst düzeye çuygunlarını belirlemelerini öneririz.
    * Önceden özel olarak kararlaştırılmadığı sürece, eşarkadaşlarının Microsoft ile eşleştiği tüm konumlarda tutarlı rotalar duyurması beklenir.
    * Genel olarak, AS8075 ile eşleme oturumları tüm AS-MICROSOFT yollarının reklamını yapacaktır. Afrika ve Asya'daki AS8075 bağlantıları ilgili bölgeyle ilgili rotalarla sınırlı olabilir.
    * Taraflardan hiçbiri statik bir rota, son çare bir rota oluşturacak veya BGP üzerinden duyurulmayan bir rota için trafiği diğer tarafa göndermeyecek.
    * Akranların rota güvenliği için [MANRS](https://www.manrs.org/) endüstri standartlarına uymaları beklenmektedir.

## <a name="operational-requirements"></a>Operasyonel gereksinimler
* Tüm teknik ve performans sorunlarının, güvenlik ihlallerinin, hizmet reddi saldırılarının veya eş veya müşterilerinin içinde ortaya çıkan diğer suistimallerin çözümüne yardımcı olabilecek tam donanımlı 24x7 Ağ Operasyon Merkezi (NOC).
* Eşarkadaşlarının [PeeringDB'de](https://www.peeringdb.com) kurumsal etki alanından ve telefon numarasından gelen 24x7 NOC e-postası da dahil olmak üzere eksiksiz ve güncel bir profile sahip olması beklenir. Bu bilgileri, noc bilgileri, teknik iletişim bilgileri ve bunların akran tesislerindeki varlığı vb. gibi eşin bilgilerini doğrulamak için kullanırız. Kişisel Yahoo, Gmail ve hotmail hesapları kabul edilmez.

## <a name="physical-connection-requirements"></a>Fiziksel bağlantı gereksinimleri
* Doğrudan eşleme veya Exchange eşleme için Microsoft'a bağlanabileceğiniz konumlar [PeeringDB'de](https://www.peeringdb.com/net/694) listelenir
* **Exchange akran:**
    * Ara bağlantı, uygun 10Gbps optik kullanılarak tek modlu fiber üzerinde olmalıdır.
    * En yüksek kullanım %50'yi aştığında, eşlerin bağlantı noktalarını yükseltmeleri beklenir.
* **Doğrudan bakma:**
    * Ara bağlantı, uygun 10Gbps veya 100Gbps optik kullanılarak tek modlu fiber üzerinde olmalıdır.
    * Microsoft yalnızca ISS veya Ağ Hizmeti sağlayıcılarıyla Doğrudan bakış oluşturmayı sağlar.
    * En yüksek kullanım %50'yi aştığında ve her metroda, tek bir konumda veya metroda çeşitli konumlarda farklı kapasiteyi koruduğunda, akranların limanlarını yükseltmeleri beklenir.
    * Her Doğrudan bakış, Peer'in kenarında bulunan Eş yönlendiricilerinden iki Microsoft kenar yönlendiricisine iki bağlantıdan oluşur. Microsoft, bu bağlantılar arasında çift BGP oturumları gerektirir. Eş, sonunda gereksiz aygıtları dağıtmamayı seçebilir.

## <a name="traffic-requirements"></a>Trafik gereksinimleri
* Exchange bakan üzerinde Akranları trafik en az 200Mb ve 2Gb daha az olmalıdır.  2Gb'ı aşan trafik için Doğrudan bakış gözden geçirilmelidir.
* Doğrudan bakma için, ağınızdan Microsoft'a olan trafiğin minimum gereksinimin altında olması gerekir.

    | Coğrafi Bölge                      | Microsoft'a minimum trafik   |
    | :----------------------- |:-------------------------------|
    | Afrika                   | 500 Mbps                       |
    | APAC (Hindistan hariç)      |   2 Gbps                       |
    | APAC (sadece Hindistan)        | 500 Mbps                       |
    | Avrupa                   |   2 Gbps                       |
    | LATAM                    |   2 Gbps                       |
    | Orta Doğu              | 500 Mbps                       |
    | NA                       |   2 Gbps                       |

* **Çeşitlilik:**
    * NA, Avrupa, APAC ve LATAM'da, mümkünse en az üç coğrafi farklı yerde birbirine bağlanır ve her metroda trafiğin aksamasına izin verecek çeşitli kapasiteye sahiptir.
    * Afrika, Orta Doğu ve Hindistan'da, mümkün olduğunca çok farklı yerlerde birbirine bağlayın. Bölgede trafiğin kalmasını sağlamak için yeterli farklı kapasiteye sahip olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* Microsoft'a doğrudan bakış ayarlama adımları hakkında bilgi edinmek için [Doğrudan bakan gözden geçirme yi](walkthrough-direct-all.md)izleyin.
* Exchange'in Microsoft ile eşlemeyi ayarlama adımları hakkında bilgi edinmek için [Exchange'in eşleme gözden geçirmesini](walkthrough-exchange-all.md)izleyin.