---
title: Azure ön kapısı-arka uç sistem durumu izleme | Microsoft Docs
description: Bu makale, Azure ön kapısının arka uçlarınızın sistem durumunu nasıl izlediğini anlamanıza yardımcı olur
services: frontdoor
documentationcenter: ''
author: duongau
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: duau
ms.openlocfilehash: c96dac55df2cdc15b7d3699e947c851a9fe69b02
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89399642"
---
# <a name="health-probes"></a>Sistem durumu araştırmaları

Belirli bir ön kapıdaki her bir arka ucun durumunu ve yakınlığını tespit etmek için, her bir ön kapı ortamı, yapılandırılmış her bir arka uç ortamında düzenli olarak yapay bir HTTP/HTTPS isteği gönderir. Front Door bu araştırmalardan gelen yanıtları kullanarak gerçek istemci isteklerini yönlendireceği "en iyi" arka uçları belirler. 

> [!WARNING]
> Ön kapıda genel olarak çok uç ortamları olduğundan, arka uçlarınıza yönelik durum araştırma istekleri birimi, yapılandırılan durum araştırma sıklığı uyarınca dakikada her dakikada en fazla 1200 istek kadar yüksek bir biçimde olabilir. Varsayılan araştırma sıklığı 30 saniye ile, arka ucunuzdaki araştırma hacmi, dakikada yaklaşık 200 istek olmalıdır.

## <a name="supported-protocols"></a>Desteklenen protokoller

Ön kapı, HTTP veya HTTPS protokolleri üzerinden araştırmaları göndermeyi destekler. Bu araştırmalar istemci isteklerini yönlendirmek için yapılandırılmış olan TCP bağlantı noktaları üzerinden gönderilir ve bu ayar geçersiz kılınamaz.

## <a name="supported-http-methods-for-health-probes"></a>Durum araştırmaları için desteklenen HTTP yöntemleri

Ön kapı, sistem durumu araştırmalarını göndermek için aşağıdaki HTTP yöntemlerini destekler:

1. **Al:** GET yöntemi, Istek URI 'SI tarafından tanımlanan bilgilerin (bir varlık biçiminde) alınması anlamına gelir.
2. **Baş:** HEAD yöntemi GET ile aynıdır, ancak sunucu yanıtta bir ileti gövdesi döndürmemelidir. Yeni ön kapı profilleri için, varsayılan olarak araştırma yöntemi baş olarak ayarlanır.

> [!NOTE]
> Ön kapı, arka uçlarınızın daha düşük yük ve maliyet için sistem durumu araştırmalarının baş isteklerini kullanmanızı önerir.

## <a name="health-probe-responses"></a>Durum araştırma yanıtları

| Yanıtlar  | Description | 
| ------------- | ------------- |
| Sistem durumunu belirleme  |  200 OK durum kodu arka ucun sağlıklı olduğunu gösterir. Diğer her şey hata olarak kabul edilir. Herhangi bir nedenle (ağ arızası dahil) bir araştırma için geçerli bir HTTP yanıtı alınmıyorsa, araştırma bir hata olarak sayılır.|
| Ölçüm gecikmesi  | Gecikme süresi, yanıtın son baytını elde ettiğimiz zaman araştırma isteğini gönderdiğimiz zamandan hemen önce ölçülen duvar saati zamanı. Her istek için yeni bir TCP bağlantısı kullanıyoruz ve bu ölçüm, mevcut bir sıcak bağlantıyla arka uçlara doğru değil.  |

## <a name="how-front-door-determines-backend-health"></a>Ön kapı, arka uç durumunu belirler

Azure ön kapısı, sistem durumunu öğrenmek için tüm algoritmalarda aşağıdaki üç adımlı işlemi kullanır.

1. Devre dışı bırakılan arka uçları hariç tut.

2. Sistem durumu araştırmaları hataları olan arka uçları hariç tut:
    * Bu seçim, son _n_ durum araştırma yanıtlarına bakarak yapılır. En az _x_ sağlıklı ise, arka uç sağlıklı olarak değerlendirilir.

    * _n_ , Yük Dengeleme ayarlarındaki SampleSize özelliği değiştirilerek yapılandırılır.

    * _x_ , Yük Dengeleme ayarlarında başarılı bir şekilde, gerekli özelliği değiştirilerek yapılandırılır.

3. Arka uç havuzundaki sağlıklı arka uçların kümesinden, ön kapı ek olarak her arka uç için gecikme süresini (gidiş dönüş süresi) ölçer ve korur.


## <a name="complete-health-probe-failure"></a>Tüm sistem durumu araştırma hatası

Bir arka uç havuzundaki her arka uç için sistem durumu araştırmaları başarısız olursa, ön kapı tüm arka uçların sağlıklı olduğunu varsayar ve trafiği tüm bunlar arasında hepsini bir kez deneme dağıtımında yönlendirir.

Arka uç, sağlıklı duruma döndüğünde, ön kapı normal yük dengeleme algoritmasını sürdürür.

## <a name="disabling-health-probes"></a>Durum araştırmalarını devre dışı bırakma

Arka uç havuzunuzdaki tek bir arka uç varsa, uygulamanızın arka ucunuzdaki yükünü azaltan sistem durumu araştırmalarını devre dışı bırakmayı seçebilirsiniz. Arka uç havuzunda birden fazla arka uç olsa bile, ancak bunlardan yalnızca biri etkin durumdaysa, sistem durumu araştırmalarını devre dışı bırakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
