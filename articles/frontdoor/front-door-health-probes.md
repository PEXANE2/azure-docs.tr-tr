---
title: Azure Ön Kapı - arka uç sağlık izleme | Microsoft Dokümanlar
description: Bu makale, Azure Ön Kapı'nın arka uçlarınızın durumunu nasıl izlediğini anlamanıza yardımcı olur
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: e2e656c395f1a31c1f5ebbd46d5a18a046f854f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471583"
---
# <a name="health-probes"></a>Sistem durumu araştırmaları

Belirli bir Ön Kapı ortamından her bir arka uç sağlığını ve yakınlığını belirlemek için, her Ön Kapı ortamı düzenli olarak yapılandırılmış arka uçlarınızın her birine sentetik bir HTTP/HTTPS isteği gönderir. Front Door bu araştırmalardan gelen yanıtları kullanarak gerçek istemci isteklerini yönlendireceği "en iyi" arka uçları belirler. 

> [!WARNING]
> Front Door küresel olarak birçok kenar ortamına sahip olduğundan, arka uçlarınıza kadar sağlık sondası istekleri hacmi oldukça yüksek olabilir - yapılan sağlık sondası sıklığına bağlı olarak her dakika 25 istekten dakikada 1200 isteke kadar değişen bir hacim. Varsayılan sonda frekansı 30 saniye ile arka uçtaki sonda hacmi dakikada yaklaşık 200 istek olmalıdır.

## <a name="supported-protocols"></a>Desteklenen protokoller

Ön Kapı, HTTP veya HTTPS protokolleri üzerinden prob göndermeyi destekler. Bu araştırmalar istemci isteklerini yönlendirmek için yapılandırılmış olan TCP bağlantı noktaları üzerinden gönderilir ve bu ayar geçersiz kılınamaz.

## <a name="supported-http-methods-for-health-probes"></a>Sağlık sondaları için desteklenen HTTP yöntemleri

Ön Kapı sağlık sondaları göndermek için aşağıdaki HTTP yöntemleri destekler:

1. **GET:** GET yöntemi, Request-URI tarafından tanımlanan her türlü bilgiyi (varlık biçiminde) almak anlamına gelir.
2. **BAŞ:** HEAD yöntemi, sunucunun yanıtta bir ileti gövdesi döndürmemesi dışında GET ile aynıdır. Yeni Ön Kapı profilleri için varsayılan olarak sonda yöntemi HEAD olarak ayarlanır.

> [!NOTE]
> Arka uçlarınıza daha düşük yük ve maliyet için, Ön Kapı sağlık sondaları için HEAD isteklerini kullanmanızı önerir.

## <a name="health-probe-responses"></a>Sağlık sondası yanıtları

| Yanıtlar  | Açıklama | 
| ------------- | ------------- |
| Sağlığın Belirlenmesi  |  200 Ok durum kodu arka uç sağlıklı olduğunu gösterir. Diğer her şey başarısız olarak kabul edilir. Herhangi bir nedenle (ağ hatası da dahil olmak üzere) bir sonda için geçerli bir HTTP yanıtı alınmazsa, sonda bir hata olarak sayılır.|
| Gecikme Ölçme  | Gecikme süresi, sonda isteğini yanıtın son baytını aldığımız ana göndermeden hemen önce ölçülen duvar saati süresidir. Her istek için yeni bir TCP bağlantısı kullanırız, bu nedenle bu ölçüm mevcut sıcak bağlantılarla arka uçlara doğru önyargılı değildir.  |

## <a name="how-front-door-determines-backend-health"></a>Ön Kapı arka uç sağlığını nasıl belirler?

Azure Ön Kapı, durumu belirlemek için tüm algoritmalarda aşağıdaki üç adımlı işlemi kullanır.

1. Devre dışı bırakılmış arka uçları hariç tut.

2. Sistem durumu sondaları hataları olan arka uçları hariç tut:
    * Bu seçim son _n_ sağlık sondası yanıtları bakarak yapılır. En azından _x_ sağlıklı ise, arka uç sağlıklı olarak kabul edilir.

    * _n,_ yük dengeleme ayarlarında SampleSize özelliği değiştirilerek yapılandırılır.

    * _x,_ yük dengeleme ayarlarında Başarılı Örnekler Gerekli özelliği değiştirilerek yapılandırılır.

3. Arka uç havuzundaki sağlıklı arka uçkümesinin dışında, Ön Kapı her arka uç için gecikme süresini (gidiş-dönüş) da ölçer ve korur.


## <a name="complete-health-probe-failure"></a>Tam sağlık sondası hatası

Sağlık sondaları bir arka uç havuzunda her arka uç için başarısız olursa, o zaman Ön Kapı tüm backends sağlıklı ve hepsi arasında yuvarlak robin dağıtım trafik yolları dikkate alır.

Herhangi bir arka uç sağlıklı bir duruma döndüğünde, Ön Kapı normal yük dengeleme algoritmasına devam edecektir.

## <a name="disabling-health-probes"></a>Sağlık sondalarının devre dışı bırakılması

Arka uç havuzunuzda tek bir arka uç varsa, uygulama arka uçunuzdaki yükü azaltan sistem sondalarını devre dışı bırakmayı seçebilirsiniz. Arka uç havuzunda birden çok arka uç olsa, ancak bunlardan yalnızca biri etkin durumda olsa bile, sistem durumu sondalarını devre dışı bırakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
