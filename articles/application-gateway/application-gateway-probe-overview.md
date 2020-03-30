---
title: Azure Uygulama Ağ Geçidi için sistem durumu izleme genel bakış
description: Azure Application Gateway, arka uç havuzundaki tüm kaynakların sistem durumunu izler ve sağlıksız olduğu düşünülen tüm kaynakları havuzdan otomatik olarak kaldırır.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2020
ms.author: victorh
ms.openlocfilehash: c5a53167c6a4ca6c886b858a1608eaa173185bd8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335853"
---
# <a name="application-gateway-health-monitoring-overview"></a>Uygulama Ağ Geçidi sistem durumu izleme genel bakış

Azure Application Gateway varsayılan olarak arka uç havuzundaki tüm kaynakların sistem durumunu izler ve sağlıksız olduğu düşünülen tüm kaynakları havuzdan otomatik olarak kaldırır. Application Gateway durumu iyi olmayan örnekleri izlemeye devam eder; bunlar kullanılabilir duruma geldiğinde ve sağlık durumu yoklamalarına yanıt verdiğinde bunları yeniden iyi durumdaki arka uç havuzuna ekler. Uygulama ağ geçidi, sistem durumu sondalarını arka uç HTTP ayarlarında tanımlanan aynı bağlantı noktasına gönderir. Bu yapılandırma, sondanın müşterilerin arka uca bağlanmak için kullanmak üzere kullanmak üzere kullandığı aynı bağlantı noktasını test etmesini sağlar. 

Uygulama Ağ Geçidi'nin sistem durumu sondaları için kullandığı kaynak IP adresi arka uç havuzuna bağlıdır:
 
- Arka uç havuzu genel bir bitiş noktasıise, kaynak adres uygulama ağ geçidi önuç genel IP adresidir.
- Arka uç havuzu özel bir bitiş noktası ise, kaynak IP adresi uygulama ağ geçidi subnet özel IP adresi alanından dır.


![uygulama ağ geçidi sondası örneği][1]

Varsayılan sistem durumu sondası izlemenin yanı sıra, sistem durumu sondasını uygulamanızın gereksinimlerine uyacak şekilde özelleştirebilirsiniz. Bu makalede, hem varsayılan hem de özel sistem durumu sondaları kapsanmaktadır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="default-health-probe"></a>Varsayılan sistem durumu sondası

Herhangi bir özel sonda yapılandırması ayarlamadığınızda, uygulama ağ geçidi varsayılan bir sistem durumu sondasını otomatik olarak yapılandırır. İzleme davranışı, arka uç havuzu için yapılandırılan IP adreslerine BIR HTTP isteği yaparak çalışır. Arka uç http ayarları HTTPS için yapılandırılırsa varsayılan sondalar için, sonda arka uçların sistem durumunu test etmek için de HTTPS kullanır.

Örneğin: Uygulama ağ geçidinizi, 80 no'lu bağlantı noktasındaki HTTP ağ trafiğini almak için A, B ve C arka uç sunucularını kullanacak şekilde yapılandırırsınız. Varsayılan sistem durumu izleme sağlıklı bir HTTP yanıtı için her 30 saniyede üç sunucutestleri. Sağlıklı bir HTTP yanıtı 200 ve 399 arasında bir [durum kodu](https://msdn.microsoft.com/library/aa287675.aspx) vardır.

Varsayılan sonda denetimi sunucu A için başarısız olursa, uygulama ağ geçidi onu arka uç havuzundan kaldırır ve ağ trafiği bu sunucuya akmayı durdurur. Varsayılan sonda hala sunucu A için her 30 saniyede kontrol etmeye devam ediyor. Sunucu A varsayılan bir sistem durumu sondasından gelen bir isteğe başarılı bir şekilde yanıt verdiğinde, arka uç havuzuna sağlıklı olarak geri eklenir ve trafik yeniden sunucuya akmaya başlar.

### <a name="probe-matching"></a>Sonda Eşleştirme

Varsayılan olarak, 200 ve 399 arasında durum kodu ile bir HTTP(S) yanıtı sağlıklı olarak kabul edilir. Özel sistem durumu sondaları ayrıca iki eşleşen ölçütleri de destekler. Eşleştirme ölçütleri isteğe bağlı olarak sağlıklı bir yanıt yapan şeyin varsayılan yorumunu değiştirmek için kullanılabilir.

Aşağıdaki eşleşen kriterler şunlardır: 

- **HTTP yanıt durum kodu eşlemi** - Belirtilen kullanıcıyı kabul etmek için sonda eşleştirme ölçütü http yanıt kodu veya yanıt kodu aralıkları. Tek tek virgülden ayrılmış yanıt durum kodları veya çeşitli durum kodu desteklenir.
- **HTTP yanıt gövde eşleşmesi** - HTTP yanıt gövdesine bakan ve kullanıcı tarafından belirtilen dizeyle eşleşen sonda eşleştirme ölçütü. Eşleşme yalnızca yanıt gövdesinde kullanıcı belirtilen dize varlığını arar ve tam bir normal ifade eşleşmesi değildir.

Maç kriterleri `New-AzApplicationGatewayProbeHealthResponseMatch` cmdlet kullanılarak belirtilebilir.

Örnek:

```azurepowershell
$match = New-AzApplicationGatewayProbeHealthResponseMatch -StatusCode 200-399
$match = New-AzApplicationGatewayProbeHealthResponseMatch -Body "Healthy"
```
Eşleşme ölçütleri belirlendikten sonra PowerShell'de bir `-Match` parametre kullanılarak prob yapılandırmasına eklenebilir.

### <a name="default-health-probe-settings"></a>Varsayılan sistem durumu sondası ayarları

| Sonda özelliği | Değer | Açıklama |
| --- | --- | --- |
| Sonda URL'si |http://127.0.0.1:\<port\>/ |URL yolu |
| Interval |30 |Bir sonraki sağlık sondası gönderilmeden önce saniyeler içinde bekleme süresi.|
| Zaman aşımı |30 |Uygulama ağ geçidinin saniye cinsinden süresi, sondayı sağlıksız olarak işaretlemeden önce sonda yanıtını bekler. Bir sonda sağlıklı olarak dönerse, karşılık gelen arka uç hemen sağlıklı olarak işaretlenir.|
| Sağlıksız durum eşiği |3 |Normal sağlık sondasının başarısız olması ihtimaline karşı kaç tane sonda gönderilemelerini yönetir. Bu ek sağlık sondaları hızlı bir şekilde arka uç sağlığını belirlemek için hızlı bir şekilde art arda gönderilir ve sonda aralığı için beklemeyin. Bu behaivor sadece v1 SKU olduğunu. v2 SKU durumunda, sağlık sondaları aralığı bekleyin. Arka uç sunucusu, ardışık sonda hata sayısı sağlıksız eşiğe ulaştıktan sonra aşağı işaretlenir. |

> [!NOTE]
> Bağlantı noktası, arka uç HTTP ayarlarıyla aynı bağlantı noktasıdır.

Varsayılan sonda yalnızca http\/bakar: /127.0.0.1:\<bağlantı noktası\> sağlık durumunu belirlemek için. Özel bir URL'ye gidecek veya diğer ayarları değiştirecek şekilde sistem durumu sondasını yapılandırmanız gerekiyorsa, özel sondalar kullanmanız gerekir.

### <a name="probe-intervals"></a>Sonda aralıkları

Uygulama Ağ Geçidi'nin tüm örnekleri arka uçbirbirlerinden bağımsız olarak sonda. Aynı sonda yapılandırması her Uygulama Ağ Geçidi örneği için geçerlidir. Örneğin, sonda yapılandırması her 30 saniyede bir sistem durumu sondaları gönderecekse ve uygulama ağ geçidinde iki örnek varsa, her iki örnek de her 30 saniyede bir sistem durumu sondası gönderir.

Ayrıca birden fazla dinleyici varsa, o zaman her dinleyici birbirinden bağımsız arka uç yoklar. Örneğin, iki farklı bağlantı noktası (iki arka uç http ayarları tarafından yapılandırılan) aynı arka uç havuzunu işaret eden iki dinleyici varsa, her dinleyici aynı arka uç bağımsız olarak sondalar. Bu durumda, iki dinleyici için her uygulama ağ geçidi örneğinden iki sonda vardır. Bu senaryoda uygulama ağ geçidinin iki örneği varsa, arka uç sanal makine yapılandırılan sonda aralığı başına dört prob görür.

## <a name="custom-health-probe"></a>Özel sağlık sondası

Özel sondalar, sistem durumu izleme üzerinde daha ayrıntılı bir denetime sahip olmak sağlar. Özel sondalar kullanırken, sonda aralığını, TEST EDEBİYAT ALABİlECEK URL'yi ve kaç başarısız yanıtı, arka uç havuzu örneğini sağlıksız olarak işaretlemeden önce kabul etmek üzere yapılandırabilirsiniz.

### <a name="custom-health-probe-settings"></a>Özel sistem durumu sondası ayarları

Aşağıdaki tablo, özel bir sistem durumu sondasının özellikleri için tanımlar sağlar.

| Sonda özelliği | Açıklama |
| --- | --- |
| Adı |Sondanın adı. Bu ad, arka uç HTTP ayarlarında sonda başvurmak için kullanılır. |
| Protokol |Protokol sondayı göndermek için kullanılırdı. Sonda, arka uç HTTP ayarlarında tanımlanan protokolü kullanır |
| Host |Sondayı göndermek için ana bilgisayar adı. Yalnızca Uygulama Ağ Geçidi'nde çok siteli yapılandırıldığında uygulanabilir, aksi takdirde '127.0.0.1' kullanın. Bu değer VM ana bilgisayar adından farklıdır. |
| Yol |Sondanın göreli yolu. Geçerli yol '/' adresinden başlar. |
| Interval |Sonda aralığı saniyeler içinde. Bu değer, art arda iki sonda arasındaki zaman aralığıdır. |
| Zaman aşımı |Saniyeler içinde sonda lama. Bu zaman ayırMadı süresi içinde geçerli bir yanıt alınmazsa, sonda başarısız olarak işaretlenir.  |
| Sağlıksız durum eşiği |Sonda yeniden say. Arka uç sunucusu, ardışık sonda hata sayısı sağlıksız eşiğe ulaştıktan sonra aşağı işaretlenir. |

> [!IMPORTANT]
> Uygulama Ağ Geçidi tek bir site için yapılandırılırsa, varsayılan olarak Ana Bilgisayar adı, özel sondada aksi yapılmadığı sürece '127.0.0.1' olarak belirtilmelidir.
> Referans için özel bir \<sonda protokole\>gönderilir ://\<\>ana bilgisayar :\<bağlantı noktası\>\<yolu\>. Kullanılan bağlantı noktası, arka uç HTTP ayarlarında tanımlanan bağlantı noktasıyla aynı bağlantı noktası olacaktır.

## <a name="nsg-considerations"></a>NSG hususlar

Uygulama Ağ Geçidi v1 SKU için TCP bağlantı noktalarında 65503-65534 ve V2 SKU için 65200-65535 arasında gelen **Any** Internet trafiğine izin vermelisiniz. **GatewayManager** Bu bağlantı noktası aralığı Azure altyapı iletişimi için gereklidir.

Ayrıca, giden Internet bağlantısı engellenemez ve **AzureLoadBalancer** etiketinden gelen trafiğe izin verilmelidir.

Daha fazla bilgi için Bkz. [Uygulama Ağ Geçidi yapılandırmagenel bakışı.](configuration-overview.md#network-security-groups-on-the-application-gateway-subnet)

## <a name="next-steps"></a>Sonraki adımlar
Uygulama Ağ Geçidi sistem durumu izlemesini öğrendikten sonra, Azure portalında özel bir [sistem durumu sondası](application-gateway-create-probe-portal.md) veya PowerShell ve Azure Kaynak Yöneticisi dağıtım modelini kullanarak özel bir [sistem durumu sondası](application-gateway-create-probe-ps.md) yapılandırabilirsiniz.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png
