---
title: Uygulama Proxy uygulaması nasıl yapılandırılır | Microsoft Dokümanlar
description: Birkaç basit adımda bir APplication Proxy uygulamasını nasıl oluşturup yapılandırılamayı öğrenin
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7aaf2eb282bc3fd0b9f3853ce493c479a3d3c3a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67807849"
---
# <a name="how-to-configure-an-application-proxy-application"></a>Uygulama Proxy uygulaması nasıl yapılandırılır?

Bu makale, şirket içi uygulamalarınızı buluta maruz bırakmak için Azure AD'deki bir Uygulama Proxy uygulamasını nasıl yapılandırabileceğinizi anlamanıza yardımcı olur.

## <a name="recommended-documents"></a>Önerilen belgeler

Yönetici Portalı üzerinden ilk yapılandırmaları ve uygulama proxy uygulaması oluşturma hakkında bilgi edinmek için [Azure AD Application Proxy'yi kullanarak Uygulamaları](application-proxy-add-on-premises-application.md)Yayımla'yı izleyin.

Bağlayıcıları yapılandırma yla ilgili ayrıntılar için [Azure portalında Uygulama Proxy'sini Etkinleştir'e](application-proxy-add-on-premises-application.md)bakın.

Sertifika yükleme ve özel etki alanlarını kullanma hakkında daha fazla bilgi için Azure [AD Application Proxy'de özel etki alanlarıyla çalışma](application-proxy-configure-custom-domain.md)bilgisine bakın.

## <a name="create-the-applicationsetting-the-urls"></a>Uygulama Oluşturma/URL'leri Ayarlama

Azure AD Application Proxy belgelerini [kullanarak Uygulama Yayımla](application-proxy-add-on-premises-application.md) adımlarını takip ediyorsanız ve uygulamayı oluştururken bir hata alıyorsanız, uygulamayı nasıl düzelteceğimiz le ilgili bilgi ve öneriler için hata ayrıntılarına bakın. Çoğu hata iletisi önerilen düzeltmeyi içerir. Sık karşılaşılan hataları önlemek için şu leri doğrulayın:

- Bir Uygulama Proxy uygulaması oluşturmak için izni olan bir yönetici vardır
- Dahili URL benzersizdir
- Harici URL benzersizdir
- URL'ler http veya https ile başlar ve "/" ile sona erer
- URL bir etki alanı adı olmalı, IP adresi değil

Uygulamayı oluştururken hata iletisi sağ üst köşede görüntülenmelidir. Hata iletilerini görmek için bildirim simgesini de seçebilirsiniz.

![Azure portalında Bildirim istemini nerede bulacağımı gösterir](./media/application-proxy-config-how-to/error-message.png)

## <a name="configure-connectorsconnector-groups"></a>Konektörleri/bağlayıcı gruplarını yapılandırma

Bağlayıcılar ve bağlayıcı grupları hakkında uyarı nedeniyle uygulamanızı yapılandırmada güçlük çekiyorsanız, bağlayıcıları nasıl karşıdan yükleyin hakkında ayrıntılar için Uygulama Proxy'sini etkinleştirme yönergelerine bakın. Bağlayıcılar hakkında daha fazla bilgi edinmek istiyorsanız, [bağlayıcılar belgelerine](application-proxy-connectors.md)bakın.

Bağlayıcılarınız etkin değilse, bu hizmete erişemedikleri anlamına gelir. Bunun nedeni genellikle gerekli tüm bağlantı noktalarının açık olmamasıdır. Gerekli bağlantı noktalarının listesini görmek için, etkinleştirilen Uygulama Proxy belgelerinin ön koşullar bölümüne bakın.

## <a name="upload-certificates-for-custom-domains"></a>Özel etki alanları için sertifika yükleme

Özel Etki Alanları, harici URL'lerinizin etki alanını belirtmenize olanak sağlar. Özel etki alanlarını kullanmak için, söz alan adı için sertifika yüklemeniz gerekir. Özel etki alanlarını ve sertifikaları kullanma hakkında daha fazla bilgi için Azure [AD Application Proxy'de özel etki alanlarıyla çalışma](application-proxy-configure-custom-domain.md)bilgisine bakın.

Sertifikanızı yüklerken sorunlarla karşılaşıyorsanız, sertifikayla ilgili sorun hakkında ek bilgi için portaldaki hata iletilerini arayın. Yaygın sertifika sorunları şunlardır:

- Süresi dolmuş sertifika
- Sertifika kendi imzalı
- Sertifika özel anahtar eksik

Sertifikayı yüklemeye çalışırken sağ üst köşede hata iletisi görüntülenir. Hata iletilerini görmek için bildirim simgesini de seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD Uygulama Ara Sunucusu ile uygulama yayımlama](application-proxy-add-on-premises-application.md)
