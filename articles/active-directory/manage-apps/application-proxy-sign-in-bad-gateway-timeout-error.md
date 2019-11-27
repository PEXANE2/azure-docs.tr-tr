---
title: Uygulama proxy 'Si uygulamasıyla bu kurumsal uygulama hatasına erişilemiyor
description: Azure AD uygulama proxy'si uygulamaları ile ortak erişim sorunlarını gidermek nasıl.
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
ms.date: 05/21/2019
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3d61f4b4bce9b8287dc13237f071684ea5d135fa
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74275463"
---
# <a name="cant-access-this-corporate-application-error-when-using-an-application-proxy-application"></a>Uygulama proxy'si uygulaması kullanılırken "Şirket bu uygulamaya erişemezsiniz" hatası

Bu makale bir Azure AD uygulama proxy'si uygulaması "Bu şirket uygulama erişilemiyor" hatası ile ilgili yaygın sorunları gidermenize yardımcı olur.

## <a name="overview"></a>Genel Bakış

Bu hatayı gördüğünüzde, durum kodu hata sayfasında bulabilirsiniz. Bu kodu, aşağıdaki durum kodları biri olabilir.:

- **Ağ geçidi zaman aşımı**: uygulama proxy 'si hizmeti bağlayıcıya ulaşamıyor. Bağlayıcıyı ağ kuralları ya da bu hata genellikle bağlayıcının kendisi, bağlayıcı atama ile ilgili bir sorun olduğunu gösterir.
- **Hatalı ağ geçidi**: bağlayıcı, arka uç uygulamasına ulaşamıyor. Bu hata, yanlış yapılandırma uygulamanın olduğunu gösteriyor olabilir.
- **Yasak**: Kullanıcı uygulamaya erişim yetkisine sahip değil. Kullanıcı Azure Active Directory'de uygulama atanmadığında veya arka uçta kullanıcının uygulamaya erişim izni yoksa, bu hata oluşabilir.

Kodu bulmak için "Durum kodu" alanı için hata iletisinin alt sol metin bakın. Sayfanın alt kısmındaki herhangi bir ek ipuçları da arayın.

![Örnek: ağ geçidi zaman aşımı hatası](./media/application-proxy-sign-in-bad-gateway-timeout-error/connection-problem.png)

Bu hataların temel nedenini gidermeye ilişkin ayrıntıları ve önerilen düzeltmeleri hakkında daha fazla bilgi için aşağıdaki ilgili bölümüne bakın.

## <a name="gateway-timeout-errors"></a>Ağ geçidi zaman aşımı hataları

Bir ağ geçidi zaman aşımı hizmet Bağlayıcısı erişmeye çalıştığında oluşur ve zaman aşımı penceresinde içinde açılamıyor. Bu hata genellikle uygulamanın çalışma bağlayıcılar ile bir bağlayıcı grubuna atanmış kaynaklanır ve bağlayıcı tarafından gereken bazı bağlantı noktalarının açık değildir.

## <a name="bad-gateway-errors"></a>Hatalı ağ geçidi hataları

Hatalı ağ geçidi hatasına bağlayıcı arka uç uygulaması ulaşamadık olduğunu gösterir. doğru uygulama yayımlandığından emin olun. Bu hataya neden olan yaygın hataları şunlardır:

- Bir yazım yanlışı ya da iç URL içinde hata
- Uygulamanın kök yayımlama değil. Örneğin, <http://expenses/reimbursement> yayımlama, ancak erişmeye çalışılıyor <http://expenses>
- Kerberos Kısıtlı temsilci (KCD) yapılandırma ile ilgili sorunlar
- Arka uç uygulaması ile ilgili sorunlar

## <a name="forbidden-errors"></a>Yasak hata

Yasak hatası görürseniz, kullanıcının uygulamaya atanmadı. Azure Active Directory'de veya arka uç uygulaması, bu hata olabilir.

Azure 'da uygulamaya Kullanıcı atamayı öğrenmek için [yapılandırma belgelerine](application-proxy-add-on-premises-application.md#test-the-application)bakın.

Azure'daki uygulama için kullanıcının atandığı onaylayın, arka uç uygulaması, kullanıcı yapılandırmasını denetleyin. Kerberos Kısıtlı temsilci/tümleşik Windows kimlik doğrulaması kullanıyorsanız, KCD sorun giderme sayfası yönergeleri için bkz.

## <a name="check-the-applications-internal-url"></a>Uygulamanın iç URL'yi denetleyin

İlk hızlı adım olarak, uygulamayı **Kurumsal uygulamalar**aracılığıyla açıp ardından **uygulama proxy 'si** menüsünü seçerek iç URL 'yi iki kez kontrol edin ve onarın. Uygulamaya erişmek için şirket içi ağınızdan kullanılan dahili URL 'nin olduğunu doğrulayın.

## <a name="check-the-application-is-assigned-to-a-working-connector-group"></a>Uygulama bağlayıcı grubunda çalışan atandığından emin olun

Uygulamayı doğrulamak için bağlayıcı grubunda çalışan atanır:

1. Uygulamayı portalda açın **Azure Active Directory**, **Kurumsal uygulamalar**' a ve ardından **tüm uygulamalar** ' a tıklayın. Uygulamayı açın, sonra sol menüden **uygulama proxy 'si** ' ni seçin.
1. Bağlayıcı grubu alanına bakın. Grup içinde etkin bağlayıcı yok varsa, bir uyarı görürsünüz. Herhangi bir uyarı görmüyorsanız, gerekli tüm bağlantı noktalarına izin verildiğini doğrulamak için üzerine gidin.
1. Bağlayıcı grubu yanlış gösteriliyorsa, doğru grubunu seçin ve tüm uyarılar artık bkz onaylamak için açılan listeyi kullanın. Hedeflenen bir bağlayıcı grubu gösteriliyorsa, uyarı iletisi Bağlayıcısı Yönetim sayfasını açmak için tıklayın.
1. Buradan daha fazla ayrıntıya birkaç yolu vardır:

   - Etkin bir bağlayıcı grubuna taşıyın: Bu gruba ait olmalıdır ve görebilmesi hedef arka uç uygulaması için olan bir etkin bağlayıcı varsa, bağlayıcı atanan grubuna taşıyabilirsiniz. Bunu yapmak için bağlayıcı'yı tıklayın. "Bağlayıcı grubu" alanında doğru grubunu seçin ve Kaydet'e tıklayın için açılan listeyi kullanın.
   - Bu grup için yeni bir bağlayıcı indirin: Bu sayfadan [Yeni bir bağlayıcı indirme](https://download.msappproxy.net/Subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/Connector/Download)bağlantısını alabilirsiniz. Bağlayıcıyı doğrudan görebilmesi için arka uç uygulaması ile bir makineye yükleyin. Genellikle bağlayıcı, uygulamayla aynı sunucuya yüklenir. Bağlayıcı bağlantısını indirin, hedef makineye bir bağlayıcı indirmek için kullanın. Ardından, Bağlayıcısı'nı tıklatın ve "Bağlayıcı grubu" açılan doğru gruba ait emin olmak için kullanın.
   - Etkin olmayan bir bağlayıcı araştırın: bağlayıcıyı devre dışı olarak gösteriyorsa, hizmete bağlanamıyoruz. Bu hata genellikle engellenme bazı gerekli bağlantı noktaları nedeniyle kaynaklanır. Bu sorunu gidermek için, gerekli tüm bağlantı noktalarına izin verildiğini doğrulamak için üzerine gidin.

Kullandıktan sonra uygulama bağlayıcıları, çalışma ile bir gruba atanmış emin olmak için aşağıdaki adımları uygulamayı yeniden sınayın. Yine de çalışmazsa sonraki bölüme devam edin.

## <a name="check-all-required-ports-are-open"></a>Tüm gerekli bağlantı noktalarının açık olduğundan emin olun

Tüm gerekli bağlantı noktalarının açık olduğunu doğrulamak için bağlantı noktalarını açma üzerinde belgelerine bakın. Tüm gerekli bağlantı noktalarının açık olduğundan, sonraki bölüme Taşı.

## <a name="check-for-other-connector-errors"></a>Diğer bağlayıcı hataları denetleyin

Yukarıdakilerin hiçbiri sorunu çözmezse, sonraki sorunları veya Bağlayıcısı ile hataları aramak için adımdır. [Sorun giderme belgesinde](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#connector-errors)bazı yaygın hataları görebilirsiniz.

Ayrıca, hataları belirlemek için doğrudan bağlayıcı günlüklere bakabilirsiniz. Hata iletilerinin birçoğunu düzeltmeleri yönelik belirli öneriler paylaşın. Günlükleri görüntülemek için [Bağlayıcılar belgelerine](application-proxy-connectors.md#under-the-hood)bakın.

## <a name="additional-resolutions"></a>Ek çözümler

Yukarıdaki sorunu sorun, birkaç farklı olası nedeni vardır. Sorunu tanımlamak için:

Uygulamanız, tümleşik Windows kimlik doğrulaması (IWA) kullanmak için yapılandırılmışsa, uygulamayı çoklu oturum açma olmadan test edin. Aksi halde, sonraki paragrafın taşıyın. Çoklu oturum açma olmadan uygulamayı denetlemek için, uygulamanızı **Kurumsal uygulamalar** aracılığıyla açın ve **Çoklu oturum açma** menüsüne gidin. Aşağı açılan "Tümleşik Windows kimlik doğrulamasını", "Azure AD çoklu oturum devre dışı açma" değiştirin.

Artık bir tarayıcı açın ve uygulamaya yeniden erişmeyi deneyin. Kimlik doğrulaması için sorulması gerekir ve uygulamanın içine alın. Kimlik doğrulaması için, sorunu çoklu oturum açmayı etkinleştirir Kerberos Kısıtlı temsilci (KCD) yapılandırmaya sahip olur. Daha fazla bilgi için sorun giderme KCD sayfasına bakın.

Bu hatayı görmeye devam ederseniz, makineye bağlayıcısının yüklendiği yerde, bir tarayıcı açın ve uygulama için kullanılan iç URL ulaşmaya çalışır gidin. Bağlayıcı aynı makinedeki başka bir istemci gibi davranır. Uygulama ulaşamazsa, neden bu makineye uygulamaya ulaşmadan veya uygulamaya erişmeye çalıştığında bir sunucuda bağlayıcı kullanma olduğunu araştırın.

Sorunları veya Bağlayıcısı ile hataları aramak için bu makinede uygulama erişebiliyorsa. [Sorun giderme belgesinde](application-proxy-troubleshoot.md#connector-errors)bazı yaygın hataları görebilirsiniz. Ayrıca, hataları belirlemek için doğrudan bağlayıcı günlüklere bakabilirsiniz. Bizim hata iletilerinin birçoğunu düzeltmeleri için ayrıntılı öneriler paylaşamaz. Günlükleri görüntülemeyi öğrenmek için [Bağlayıcılar belgelerimize](application-proxy-connectors.md#under-the-hood)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD Uygulama Ara Sunucusu bağlayıcılarını anlama](application-proxy-connectors.md)
