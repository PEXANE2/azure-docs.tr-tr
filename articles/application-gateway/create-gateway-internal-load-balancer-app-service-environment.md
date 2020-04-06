---
title: Azure'da Bir Uygulama Ağ Geçidini Giderme – ILB ASE | Microsoft Dokümanlar
description: Azure'da Uygulama Hizmeti Ortamı olan bir İç Yük Dengeleyicisi kullanarak uygulama ağ geçidini nasıl gidereceklerini öğrenin
services: vpn-gateway
documentationCenter: na
author: genlin
manager: dcscontentpm
editor: ''
tags: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/06/2018
ms.author: genli
ms.openlocfilehash: 4edeea749ba22bef173c15f3a0855679b784ce33
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80668570"
---
# <a name="back-end-server-certificate-is-not-whitelisted-for-an-application-gateway-using-an-internal-load-balancer-with-an-app-service-environment"></a>Arka uç sunucu sertifikası, Uygulama Hizmet Ortamına sahip bir İç Yük Dengeleyicisi kullanılarak bir uygulama ağ geçidi için beyaz listeye alınmaz

Bu makale, aşağıdaki sorunu gidermektedir: Azure'da uçlardan uca TLS kullanırken arka uçta bir Uygulama Hizmet Ortamı (ASE) ile birlikte bir Dahili Yük Dengeleyicisi (ILB) kullanarak bir uygulama ağ geçidi oluşturduğunuzda sertifika beyaz listeye alınmaz.

## <a name="symptoms"></a>Belirtiler

Arka uçta ASE olan bir ILB kullanarak bir uygulama ağ geçidi oluşturduğunuzda, arka uç sunucusu sağlıksız hale gelebilir. Bu sorun, uygulama ağ geçidinin kimlik doğrulama sertifikası arka uç sunucusunda yapılandırılan sertifikayla eşleşmiyorsa oluşur. Aşağıdaki senaryoya örnek olarak bakın:

**Uygulama Ağ Geçidi yapılandırması:**

- **Dinleyici:** Çok siteli
- **Bağlantı Noktası:** 443
- **Ev Sahibi Adı:** test.appgwtestase.com
- **SSL Belgesi:** CN=test.appgwtestase.com
- **Arka uç Havuzu:** IP adresi veya FQDN
- **IP Adresi:** 10.1.5.11
- **HTTP Ayarları:** HTTPS
- **Bağlantı Noktası:** 443
- **Özel Sonda:** Hostname – test.appgwtestase.com
- **Kimlik Doğrulama Sertifikası:** .cer of test.appgwtestase.com
- **Arka Uç Sağlık:** Sağlıksız – Backend sunucu sertifikası Application Gateway ile beyaz listeye alınmaz.

**ASE yapılandırması:**

- **ILB IP:** 10.1.5.11
- **Alan adı:** appgwtestase.com
- **Uygulama Hizmeti:** test.appgwtestase.com
- **SSL Bağlama:** SNI SSL – CN=test.appgwtestase.com

Uygulama ağ geçidine erişdiğinizde, arka uç sunucusu sağlıksız olduğundan aşağıdaki hata iletisini alırsınız:

**502 – Web sunucusu, ağ geçidi veya proxy sunucusu görevi yaparken geçersiz bir yanıt aldı.**

## <a name="solution"></a>Çözüm

Bir HTTPS web sitesine erişmek için bir ana bilgisayar adı kullanmadığınızda, SNI devre dışı bırakılmış olması durumunda, arka uç sunucusu yapılandırılan sertifikayı varsayılan web sitesinde döndürecektir. Bir ILB ASE için varsayılan sertifika ILB sertifikasından gelir. ILB için yapılandırılmış sertifika yoksa, sertifika ASE App sertifikasından gelir.

ILB'ye erişmek için tam nitelikli bir etki alanı adı (FQDN) kullandığınızda, arka uç sunucusu HTTP ayarlarına yüklenen doğru sertifikayı döndürecektir. Bu durumda, aşağıdaki seçenekleri göz önünde bulundurun:

- ILB'nin IP adresini işaret etmek için uygulama ağ geçidinin arka uç havuzunda FQDN'yi kullanın. Bu seçenek yalnızca özel bir DNS bölgeniz veya yapılandırılmış özel bir DNS'niz varsa çalışır. Aksi takdirde, ortak bir DNS için bir "A" kaydı oluşturmanız gerekir.

- Yüklenen sertifikayı Http ayarlarında ILB'de veya varsayılan sertifikada (ILB sertifikası) kullanın. Uygulama ağ geçidi, sonda için ILB'nin IP'sine eriştığında sertifikayı alır.

- Tüm web siteleri için sertifikanın yaygın olması için ILB ve arka uç sunucusunda joker karakter sertifikası kullanın. Ancak, bu çözüm yalnızca alt etki alanlarında mümkündür ve web sitelerinin her biri farklı ana bilgisayar adları gerektiriyorsa değil.

- ILB'nin IP adresini kullanıyorsanız, uygulama ağ geçidi **için Uygulama hizmeti** için Kullanım seçeneğini temizleyin.

Ek yükü azaltmak için, sonda yolunun çalışmasını sağlamak için ILB sertifikasını HTTP ayarlarına yükleyebilirsiniz. (Bu adım sadece beyaz liste içindir. TLS iletişimi için kullanılmaz.) ILB sertifikasını, TARAYıCıNıZDAN IP adresiyle https adresinden erişerek, ardından TLS/SSL sertifikasını Base-64 kodlanmış CER formatında dışa aktararak ve sertifikayı ilgili HTTP ayarlarına yükleyerek alabilirsiniz.

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun

Yine de yardıma ihtiyacınız varsa sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
