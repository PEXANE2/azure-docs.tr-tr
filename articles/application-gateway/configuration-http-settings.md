---
title: Azure Application Gateway HTTP ayarları yapılandırması
description: Bu makalede Azure Application Gateway HTTP ayarlarının nasıl yapılandırılacağı açıklanır.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 09/09/2020
ms.author: surmb
ms.openlocfilehash: c0c939a6a8323dfdfafddb46ccdb7d7ef3dd2f2c
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89653025"
---
# <a name="application-gateway-http-settings-configuration"></a>Application Gateway HTTP ayarları yapılandırması

Application Gateway, burada belirttiğiniz yapılandırmayı kullanarak trafiği arka uç sunucularına yönlendirir. Bir HTTP ayarı oluşturduktan sonra, bir veya daha fazla istek yönlendirme kuralıyla ilişkilendirmeniz gerekir.

## <a name="cookie-based-affinity"></a>Tanımlama bilgisi tabanlı benzeşim

Azure Application Gateway, kullanıcı oturumlarını sürdürmek için ağ geçidi ile yönetilen tanımlama bilgilerini kullanır. Bir Kullanıcı Application Gateway ilk isteği gönderdiğinde, yanıt ayrıntılarını içeren bir karma değer olan yanıtta benzeşim tanımlama bilgisini ayarlar. böylece, benzeşim tanımlama bilgisini taşıyan sonraki isteklerin, sürekliliği sürdürmek için aynı arka uç sunucusuna yönlendirilecektir. 

Bu özellik, bir kullanıcı oturumunu aynı sunucuda tutmak ve oturum durumunun bir Kullanıcı oturumu için sunucuda yerel olarak kaydedilmesi durumunda yararlıdır. Uygulama, tanımlama bilgisi tabanlı benzeşimi işleyemez, bu özelliği kullanamazsınız. Bunu kullanmak için, istemcilerin tanımlama bilgilerini desteklemesini sağlayın.

[Kmıum Browser](https://www.chromium.org/Home) [V80 Update](https://chromiumdash.appspot.com/schedule) , [SameSite](https://tools.ietf.org/id/draft-ietf-httpbis-rfc6265bis-03.html#rfc.section.5.3.7) özniteliği olmayan http tanımlama bilgilerinin SameSite = LAX olarak değerlendirilmesi gereken bir mantarih getirdi. CORS (çıkış noktaları arası kaynak paylaşımı) istekleri söz konusu olduğunda, tanımlama bilgisinin bir üçüncü taraf bağlamında gönderilmesi gerekiyorsa, *SameSite = None; kullanması gerekir. Güvenli* öznitelikler ve yalnızca https üzerinden gönderilmesi gerekir. Aksi halde, yalnızca HTTP senaryosunda tarayıcı tanımlama bilgilerini üçüncü taraf bağlamında göndermez. Bu güncelleştirmenin Chrome 'dan hedefi, güvenliği artırmaktır ve siteler arası Istek sahteciliği (CSRF) saldırılarından kaçınmaktır. 

Bu değişikliği desteklemek için, 17 2020 Şubat tarihinden itibaren Application Gateway (tüm SKU türleri), mevcut *Applicationgatewaybenzeşim* tanımlama bilgisine ek olarak *Applicationgatewayaffinitycors* adlı başka bir tanımlama bilgisi ekleyecektir. *Applicationgatewayaffinitycors* tanımlama bilgisinin kendisine eklenen iki özniteliği vardır (*"SameSite = None; Güvenli "*) böylece, geçici kaynak istekleri için de yapışkan oturumun korunmasını sağlayabilirsiniz.

Varsayılan benzeşim tanımlama bilgisi adının *Applicationgatewaybenzeşim* olduğunu ve bunu değiştirebileceğinizi unutmayın. Özel bir benzeşim tanımlama bilgisi adı kullanıyorsanız, CORS ile son ek tanımlama bilgisi eklenir. Örneğin, *Customtanımlama bilgisi Enamecors*.

> [!NOTE]
> *SameSite = None* özniteliği ayarlandıysa, tanımlama bilgisinin de *güvenli* bayrağı içermesi zorunludur ve https üzerinden gönderilmesi gerekir.  CORS üzerinden oturum benzeşimi gerekliyse, iş yükünüzü HTTPS 'ye geçirmeniz gerekir. Burada Application Gateway için TLS yük boşaltma ve uçtan uca TLS belgelerine bakın: [genel bakış](ssl-overview.md), [Azure Portal kullanarak bir uygulama ağ geçidini TLS sonlandırmasıyla yapılandırma](create-ssl-portal.md), [Portal Ile Application Gateway kullanarak uçtan uca TLS Yapılandırma](end-to-end-ssl-portal.md).

## <a name="connection-draining"></a>Bağlantı boşaltma

Bağlantı boşaltma, planlı hizmet güncelleştirmeleri sırasında arka uç havuz üyelerini dikkatlice kaldırmanıza yardımcı olur. HTTP ayarında bağlantı boşaltma etkinleştirerek, bu ayarı bir arka uç havuzunun tüm üyelerine uygulayabilirsiniz. Bir arka uç havuzunun tüm kaydını kaldırmak örneklerinin mevcut bağlantıları sürdürmeye devam etmesini ve yapılandırılabilir bir zaman aşımı için, yeni istek veya bağlantı almamasını sağlar. Bunun tek istisnası, ağ geçidi tarafından yönetilen oturum benzeşimi nedeniyle kaydını kaldırmak örneklerine yönelik isteklerdir ve kaydını kaldırmak örneklerine iletilmeye devam edecektir. Bağlantı boşaltma, arka uç havuzundan açıkça kaldırılan arka uç örnekleri için geçerlidir.

## <a name="protocol"></a>Protokol

Application Gateway, arka uç sunucularına yönlendirme istekleri için hem HTTP hem de HTTPS 'yi destekler. HTTP ' yi seçerseniz, arka uç sunucularının trafiği şifrelenmemiş olur. Şifrelenmemiş iletişim kabul edilebilir değilse, HTTPS seçeneğini belirleyin.

Dinleyicide HTTPS ile birleştirilmiş bu ayar [uçtan uca TLS](ssl-overview.md)'yi destekler. Bu, arka uca şifrelenmiş hassas verileri güvenli bir şekilde aktarmanıza olanak tanır. Arka uç havuzundaki uçtan uca TLS etkin olan her arka uç sunucusu, güvenli iletişime izin verecek bir sertifikayla yapılandırılmalıdır.

## <a name="port"></a>Bağlantı noktası

Bu ayar, arka uç sunucularının uygulama ağ geçidinden gelen trafiği dinleyebileceği bağlantı noktasını belirtir. 1 ile 65535 arasında bir bağlantı noktası yapılandırabilirsiniz.

## <a name="request-timeout"></a>İstek zaman aşımı

Bu ayar, uygulama ağ geçidinin arka uç sunucusundan yanıt almak için beklediği saniye sayısıdır.

## <a name="override-back-end-path"></a>Arka uç yolunu geçersiz kıl

Bu ayar, istek arka uca iletildiğinde kullanılacak isteğe bağlı bir özel iletme yolu yapılandırmanıza olanak tanır. Gelen yolun, **arka uç yolunu geçersiz kılma** alanındaki özel yol ile eşleşen herhangi bir bölümü, iletilen yola kopyalanır. Aşağıdaki tabloda bu özelliğin nasıl çalıştığı gösterilmektedir:

- HTTP ayarı temel bir istek yönlendirme kuralına eklendiğinde:

  | Özgün istek  | Arka uç yolunu geçersiz kıl | İstek arka uca iletildi |
  | ----------------- | --------------------- | ---------------------------- |
  | /Home            | /override seçeneğiyle birlikte            | /override/Home/              |
  | /Home/Secondhome/ | /override seçeneğiyle birlikte            | /override/Home/Secondhome/   |

- HTTP ayarı yol tabanlı bir istek yönlendirme kuralına eklendiğinde:

  | Özgün istek           | Yol kuralı       | Arka uç yolunu geçersiz kıl | İstek arka uca iletildi |
  | -------------------------- | --------------- | --------------------- | ---------------------------- |
  | /pathrule/Home/            | pathrule      | /override seçeneğiyle birlikte            | /override/Home/              |
  | /pathrule/Home/Secondhome/ | pathrule      | /override seçeneğiyle birlikte            | /override/Home/Secondhome/   |
  | /Home                     | pathrule      | /override seçeneğiyle birlikte            | /override/Home/              |
  | /Home/Secondhome/          | pathrule      | /override seçeneğiyle birlikte            | /override/Home/Secondhome/   |
  | /pathrule/Home/            | /pathrule/Home * | /override seçeneğiyle birlikte            | /override seçeneğiyle birlikte                   |
  | /pathrule/Home/Secondhome/ | /pathrule/Home * | /override seçeneğiyle birlikte            | /override/Secondhome/        |
  | pathrule                 | pathrule      | /override seçeneğiyle birlikte            | /override seçeneğiyle birlikte                   |

## <a name="use-for-app-service"></a>App Service için kullanın

Bu yalnızca Azure App Service arka ucu için gereken iki ayarı seçen bir UI kısayoludur. **Ana bilgisayar adının arka uç adresinden seçim**yapmasına izin verebilir ve henüz yoksa yeni bir özel araştırma oluşturur. (Daha fazla bilgi için bu makalenin [arka uç adres ayarından konak adını seçin](#pick-host-name-from-back-end-address)bölümüne bakın.) Yeni bir araştırma oluşturulur ve araştırma üst bilgisi arka uç üyesinin adresinden çekilir.

## <a name="use-custom-probe"></a>Özel araştırma kullan

Bu ayar özel bir [araştırmayı](application-gateway-probe-overview.md#custom-health-probe) bir http ayarıyla ilişkilendirir. HTTP ayarıyla yalnızca bir özel araştırma ilişkilendirebilirsiniz. Özel bir araştırmayı açık bir şekilde ilişkilendirmezseniz, arka ucun sistem durumunu izlemek için [varsayılan araştırma](application-gateway-probe-overview.md#default-health-probe-settings) kullanılır. Arka uçlarınızın sistem durumu izlemesi üzerinde daha fazla denetim için özel bir araştırma oluşturmanız önerilir.

> [!NOTE]
> Özel araştırma, karşılık gelen HTTP ayarı açıkça bir dinleyiciyle ilişkilendirilmediği takdirde arka uç havuzunun sistem durumunu izlemez.

## <a name="pick-host-name-from-back-end-address"></a>Arka uç adresinden ana bilgisayar adını seçin

Bu yetenek, istekteki *ana bilgisayar* üst bilgisini arka uç havuzunun ana bilgisayar adına dinamik olarak ayarlar. Bir IP adresi veya FQDN kullanır.

Bu özellik, arka ucun etki alanı adının uygulama ağ geçidinin DNS adından farklı olması ve arka ucunun doğru uç noktaya çözülmesi için belirli bir ana bilgisayar üst bilgisine dayanmasına yardımcı olur.

Örnek olarak, arka uç olarak çok kiracılı hizmetler yer aldığı bir durumdur. App Service, tek bir IP adresi ile paylaşılan bir alan kullanan çok kiracılı bir hizmettir. Bu nedenle, bir App Service 'e yalnızca özel etki alanı ayarlarında yapılandırılmış ana bilgisayar adları aracılığıyla erişilebilir.

Varsayılan olarak, özel etki alanı adı *example.azurewebsites.net*' dir. App Service 'e açıkça kayıtlı olmayan bir ana bilgisayar adı aracılığıyla veya uygulama ağ geçidinin FQDN 'SI aracılığıyla uygulama hizmetinize erişmek için, özgün istekteki ana bilgisayar adını App Service 'in ana bilgisayar adına geçersiz kılarsınız. Bunu yapmak için, **arka uç adres ayarından seçim ana bilgisayar adını** etkinleştirin.

Var olan özel DNS adı App Service 'e eşlenmiş özel bir etki alanı için bu ayarı etkinleştirmeniz gerekmez.

> [!NOTE]
> Bu ayar, adanmış bir dağıtım olan App Service Ortamı için gerekli değildir.

## <a name="host-name-override"></a>Ana bilgisayar adı geçersiz kılma

Bu özellik, uygulama ağ geçidinde gelen istekteki *ana bilgisayar* üstbilgisinin yerine belirttiğiniz ana bilgisayar adını koyar.

Örneğin, *www.contoso.com* **konak adı** ayarında belirtilmişse, `https://appgw.eastus.cloudapp.azure.com/path1` `https://www.contoso.com/path1` istek arka uç sunucusuna iletildiğinde, * özgün istek * olarak değiştirilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Arka uç havuzu hakkında bilgi edinin](configuration-overview.md#back-end-pool)