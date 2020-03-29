---
title: Azure AD App Proxy ile Uzak Masaüstü Yayınlama | Microsoft Dokümanlar
description: Azure AD Application Proxy bağlayıcıları hakkındaki temel bilgileri kapsar.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: mimart
ms.custom: it-pro
ms.reviewer: harshja
ms.collection: M365-identity-device-management
ms.openlocfilehash: d6ca64e2de5734c567173fc735776074f4c87fbc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67108458"
---
# <a name="publish-remote-desktop-with-azure-ad-application-proxy"></a>Azure AD Uygulama Proxy ile Uzak Masaüstü Yayımlama

Uzak Masaüstü Hizmeti ve Azure AD Uygulama Proxy, şirket ağından uzakta olan çalışanların üretkenliğini artırmak için birlikte çalışır. 

Bu makalenin amaçlanan hedef kitlesi:
- Şirket içi uygulamaları Uzak Masaüstü Hizmetleri üzerinden yayınlayarak son kullanıcılarına daha fazla uygulama sunmak isteyen geçerli Uygulama Proxy müşterileri.
- Azure AD Application Proxy'yi kullanarak dağıtımlarının saldırı yüzeyini azaltmak isteyen mevcut Uzak Masaüstü Hizmetleri müşterileri. Bu senaryo, RDS için sınırlı sayıda iki aşamalı doğrulama ve Koşullu Erişim denetimleri sağlar.

## <a name="how-application-proxy-fits-in-the-standard-rds-deployment"></a>Uygulama Proxy'si standart RDS dağıtımına nasıl sığar?

Standart bir RDS dağıtımı, Windows Server'da çalışan çeşitli Uzak Masaüstü rol hizmetlerini içerir. [Uzak Masaüstü Hizmetleri mimarisine](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture)bakıldığında, birden çok dağıtım seçeneği vardır. Diğer RDS dağıtım seçeneklerinin aksine, [Azure AD Application Proxy ile RDS dağıtımı](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture) (aşağıdaki diyagramda gösterilmiştir) bağlayıcı hizmetini çalıştıran sunucudan kalıcı bir giden bağlantıya sahiptir. Diğer dağıtımlar, yük dengeleyicisi aracılığıyla gelen açık bağlantıları bırakır.

![Uygulama Proxy RDS VM ve kamu internet arasında oturur](./media/application-proxy-integrate-with-remote-desktop-services/rds-with-app-proxy.png)

RDS dağıtımında, RD Web rolü ve AR-Ge'si rolü Internet'e bakan makinelerde çalıştırılır. Bu uç noktalar aşağıdaki nedenlerle ortaya alınır:
- RD Web, kullanıcıya erişebilecekleri çeşitli şirket içi uygulamaları ve masaüstü bilgisayarları oturum açmaları ve görüntülemeleri için ortak bir bitiş noktası sağlar. Bir kaynak seçtikten sonra, işletim sistemi üzerindeki yerel uygulama kullanılarak bir RDP bağlantısı oluşturulur.
- Bir kullanıcı RDP bağlantısını başlattıktan sonra RD Ağ Geçidi resmin içine girer. RD Ağ Geçidi, internet üzerinden gelen şifreli RDP trafiğini işler ve kullanıcının bağlandığı şirket içi sunucuya çevirir. Bu senaryoda, RD Ağ Geçidi'nin aldığı trafik Azure AD Uygulama Proxy'sinden gelir.

>[!TIP]
>RDS'yi daha önce dağıtmadıysanız veya başlamadan önce daha fazla bilgi istiyorsanız, [AZURE Kaynak Yöneticisi ve Azure Marketi ile RDS'yi sorunsuz bir şekilde nasıl dağıtabileceğinizi](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure)öğrenin.

## <a name="requirements"></a>Gereksinimler

- Web istemcisi Application Proxy'yi desteklemediğinden, Uzak Masaüstü web istemcisi dışında bir istemci kullanın.

- Hem RD Web hem de RD Ağ Geçidi uç noktaları aynı makinede ve ortak bir kökle olmalıdır. RD Web ve RD Ağ Geçidi, iki uygulama arasında tek bir oturum açma deneyimi ne görebilirsiniz, böylece Uygulama Proxy ile tek bir uygulama olarak yayımlanır.

- [RDS'yi](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure)zaten dağıtmış ve [Uygulama Proxy'yi etkinleştirilmiş](application-proxy-add-on-premises-application.md)olmalıydınız.

- Bu senaryo, son kullanıcılarınızın RD Web sayfası üzerinden bağlanan Windows 7 veya Windows 10 masaüstü bilgisayarlarda Internet Explorer'dan geçtiğini varsayar. Diğer işletim sistemlerini desteklemeniz gerekiyorsa, [diğer istemci yapılandırmaları için Destek'e](#support-for-other-client-configurations)bakın.

- RD Web yayımlarken, aynı iç ve dış FQDN kullanılması önerilir. İç ve dış FQDN'ler farklıysa, istemcinin geçersiz bağlantılar almasını önlemek için İstek Üstbilgi Çevirisi'ni devre dışı kolmalısınız. 

- Internet Explorer'da RDS ActiveX eklentisini etkinleştirin.

- Azure AD kimlik doğrulama akışı için kullanıcılar yalnızca **RemoteApp ve Masaüstü** bölmesinde kendilerine yayınlanan kaynaklara bağlanabilir. Kullanıcılar **uzak bir bilgisayar bölmesine bağlan'ı** kullanarak masaüstüne bağlanabilir.

## <a name="deploy-the-joint-rds-and-application-proxy-scenario"></a>Ortak RDS ve Uygulama Proxy senaryosunu dağıtma

Ortamınız için RDS ve Azure AD Uygulama Proxy'sini ayarladıktan sonra, iki çözümü birleştirmek için aşağıdaki adımları izleyin. Bu adımlar, web'e bakan iki RDS uç noktasını (RD Web ve RD Ağ Geçidi) uygulama olarak yayımlayarak ve ardından RDS'nizdeki trafiği Uygulama Proxy'sine yönlendirerek geçer.

### <a name="publish-the-rd-host-endpoint"></a>RD ana bilgisayar bitiş noktasını yayımla

1. Aşağıdaki değerlere sahip [yeni bir Uygulama Proxy uygulaması yayımlayın:](application-proxy-add-on-premises-application.md)
   - Dahili `https://\<rdhost\>.com/`URL:, RD Web ve RD Ağ Geçidi'nin paylaştığı ortak kök nerededir. `\<rdhost\>`
   - Dış URL: Bu alan, uygulamanın adına bağlı olarak otomatik olarak doldurulur, ancak değiştirebilirsiniz. Kullanıcılarınız RDS'ye erişilerinde bu URL'ye gider.
   - Ön kimlik doğrulama yöntemi: Azure Etkin Dizini
   - URL üstbilgilerini çevirin: Hayır
2. Kullanıcıları yayımlanmış RD uygulamasına atayın. Hepsinin rds'ye erişimi olduğundan emin ol.
3. Uygulama için tek oturum açma yöntemini **Azure AD tek oturum**açma devre dışı bırakılmış olarak bırakın. Kullanıcılarınızın azure AD'ye bir kez ve RD Web'e bir kez kimlik doğrulamaları istenir, ancak RD Ağ Geçidi'nde tek oturum açma ları istenir.
4. **Azure Etkin Dizini'ni**ve ardından **Uygulama Kayıtlarını**seçin. Uygulamanızı listeden seçin.
5. **Yönet**altında, **Markalama'yı**seçin.
6. **Rd** Web bitiş noktanıza (beğen) `https://\<rdhost\>.com/RDWeb`işaret etmek için Giriş sayfası URL alanını güncelleştirin.

### <a name="direct-rds-traffic-to-application-proxy"></a>Uygulama Proxy doğrudan RDS trafik

RDS dağıtımına yönetici olarak bağlanın ve dağıtım için RD Ağ Geçidi sunucu adını değiştirin. Bu yapılandırma, bağlantıların Azure AD Application Proxy hizmetinden geçmesini sağlar.

1. RD Connection Broker rolünü çalıştıran RDS sunucusuna bağlanın.
2. **Sunucu Yöneticisi**başlatın.
3. Soldaki bölmeden **Uzak Masaüstü Hizmetleri'ni** seçin.
4. **Genel Bakış**’ı seçin.
5. Dağıtıma Genel Bakış bölümünde, açılır menüyü seçin ve **dağıtım özelliklerini edit'i**seçin.
6. RD Ağ Geçidi sekmesinde, **Sunucu adı** alanını Uygulama Proxy'deki RD ana bilgisayarı bitiş noktası için ayarladığınız Dış URL olarak değiştirin.
7. Oturum **Açma yöntemi** alanını Parola Kimlik Doğrulaması olarak **değiştirin.**

   ![RDS'de Dağıtım Özellikleri ekranı](./media/application-proxy-integrate-with-remote-desktop-services/rds-deployment-properties.png)

8. Her koleksiyon için bu komutu çalıştırın. * \<Koleksiyon adınızı\> * ve * \<proxyfrontendurl'nizi\> * kendi bilgilerinizle değiştirin. Bu komut, RD Web ve RD Ağ Geçidi arasında tek oturum açma sağlar ve performansı optimize eder:

   ```
   Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s:<proxyfrontendurl>`nrequire pre-authentication:i:1"
   ```

   **Örneğin:**
   ```
   Set-RDSessionCollectionConfiguration -CollectionName "QuickSessionCollection" -CustomRdpProperty "pre-authentication server address:s:https://remotedesktoptest-aadapdemo.msappproxy.net/`nrequire pre-authentication:i:1"
   ```
   >[!NOTE]
   >Yukarıdaki komut "'nrequire" bir backtick kullanır.

9. Özel RDP özelliklerinin modifikasyonunu doğrulamak ve bu koleksiyon için RDWeb'den indirilecek RDP dosya içeriğini görüntülemek için aşağıdaki komutu çalıştırın:
    ```
    (get-wmiobject -Namespace root\cimv2\terminalservices -Class Win32_RDCentralPublishedRemoteDesktop).RDPFileContents
    ```

Uzak Masaüstü'nü yapılandırdığınıza göre, Azure AD Application Proxy RDS'nin Internet'e bakan bileşeni olarak devraldı. RD Web ve RD Ağ Geçidi makinelerinizdeki diğer genel internet etek uç noktalarını kaldırabilirsiniz.

## <a name="test-the-scenario"></a>Senaryoyu test edin

Senaryoyu Bir Windows 7 veya 10 bilgisayarda Internet Explorer ile test edin.

1. Ayarladığınız harici URL'ye gidin veya uygulamanızı [MyApps panelinde](https://myapps.microsoft.com)bulun.
2. Azure Etkin Dizini'nde kimliğinin doğrulanması istenir. Uygulamaya atadığınız bir hesabı kullanın.
3. RD Web'de kimlik doğrulamanız istenir.
4. RDS kimlik doğrulamanız başarılı olduktan sonra, istediğiniz masaüstünü veya uygulamayı seçebilir ve çalışmaya başlayabilirsiniz.

## <a name="support-for-other-client-configurations"></a>Diğer istemci yapılandırmaları için destek

Bu makalede özetlenen yapılandırma, Internet Explorer artı RDS ActiveX eklentisi ile Windows 7 veya 10 kullanıcıları içindir. Ancak, gerekirse, diğer işletim sistemlerini veya tarayıcıları destekleyebilirsiniz. Fark, kullandığınız kimlik doğrulama yöntemindedir.

| Kimlik doğrulama Yöntemi | Desteklenen istemci yapılandırması |
| --------------------- | ------------------------------ |
| Ön kimlik doğrulama    | Internet Explorer + RDS ActiveX eklentisi kullanarak Windows 7/10 |
| Geçiş | Microsoft Uzak Masaüstü uygulamasını destekleyen diğer işletim sistemi |

Kimlik doğrulama öncesi akış, geçiş akışından daha fazla güvenlik avantajı sunar. Ön kimlik doğrulama ile, şirket içi kaynaklarınız için tek oturum açma, Koşullu Erişim ve iki aşamalı doğrulama gibi Azure AD kimlik doğrulama özelliklerini kullanabilirsiniz. Ayrıca, yalnızca kimlik doğrulaması verilen trafiğin ağınıza ulaştığından da emin olabilirsiniz.

Geçiş kimlik doğrulaması kullanmak için, bu makalede listelenen adımlarda sadece iki değişiklik vardır:
1. RD ana bilgisayar bitiş noktası adımı [1'de,](#publish-the-rd-host-endpoint) Ön Authentication yöntemini **Passthrough**olarak ayarlayın.
2. [Doğrudan RDS trafikUygulama Proxy](#direct-rds-traffic-to-application-proxy), adım 8 tamamen atlayın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD Uygulama Ara Sunucusu ile SharePoint’e uzaktan erişimi etkinleştirme](application-proxy-integrate-with-sharepoint-server.md)  
[Azure AD Application Proxy'yi kullanarak uygulamalara uzaktan erişmek için güvenlik hususları](application-proxy-security.md)
