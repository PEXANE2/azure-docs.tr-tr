---
title: Uygulama erişim paneli tarayıcı uzantısını yükleyin - Azure AD
description: Access paneltarayıcı uzantısını yüklediğinizde karşılaşılan sık karşılaşılan hataları düzeltin.
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
ms.topic: article
ms.date: 05/04/2018
ms.author: mimart
ms.reviewer: japere,asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 771ba79f067cbff1ab8bbfece64f4028b4ca50b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74275849"
---
# <a name="install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısını yükleme

Erişim paneli web tabanlı bir portaldır. Azure Etkin Dizini'nde (Azure AD) bir çalışma nız veya okul hesabınız varsa, bir Azure AD yöneticisinin size erişim izni verdiği bulut tabanlı uygulamaları görüntülemek ve başlatmak için erişim panelini kullanabilirsiniz. 

Azure AD sürümlerini kullanıyorsanız, erişim paneli aracılığıyla self servis grubu ve uygulama yönetimi özelliklerini de kullanabilirsiniz. 

Erişim paneli Azure portalından ayrıdır. Azure aboneliğiniz olmasını gerektirmez.

## <a name="web-browser-requirements"></a>Web tarayıcısı gereksinimleri

En azından, erişim paneli JavaScript destekleyen ve CSS etkin bir tarayıcı gerektirir. Erişim panelinde parola tabanlı SSO aracılığıyla uygulamalara giriş yapAbilmek için tarayıcınızda erişim paneli uzantısı yüklü olmalıdır. Uzantı, parola tabanlı SSO için yapılandırılan bir uygulamayı seçtiğinizde otomatik olarak indirilir.

Parola tabanlı SSO için aşağıdaki tarayıcılardan birini kullanabilirsiniz:

- **Microsoft Edge**: Windows 10 Anniversary Edition veya sonraki sürümde. 
- **Chrome**: Windows 7 veya sonraki ve MacOS X veya daha sonra.
- **Firefox 26.0 veya sonrası**: Windows XP SP2 veya sonraki ve Mac OS X 10.6 veya daha sonra.

## <a name="install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısını yükleme

Access paneltarayıcı uzantısını yüklemek için aşağıdakileri yapın:

1.  Desteklenen tarayıcılardan birinde [erişim panelini](https://myapps.microsoft.com)açın ve ardından Azure REKLAM hesabınızda kullanıcı olarak oturum açın.

2.  Parola tabanlı bir SSO uygulaması seçin.

3.  Sizden istendiğinde, **Şimdi Yükle'yi**seçin.  
    Seçtiğiniz tarayıcının indirme bağlantısına yönlendirilirsiniz. 
    
4.  **Ekle'yi**seçin.

5.  Sizden istenirse, uzantıyı **etkinleştir** veya **İzin** ver.

6.  Yükleme tamamlandıktan sonra tarayıcınızı yeniden başlatın.

7.  Erişim panelinde oturum açın ve parola tabanlı SSO uygulamalarınızı başlatıp başlatamayacağınızı kontrol edin.

Chrome ve Microsoft Edge uzantını doğrudan aşağıdaki sitelerden de indirebilirsiniz:

- [Chrome uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)
- [Microsoft Edge uzantısı](https://www.microsoft.com/en-us/p/my-apps-secure-sign-in-extension/9pc9sckkzk84)
- [Firefox uzantısı](https://addons.mozilla.org/en-US/firefox/addon/access-panel-extension/)

## <a name="use-the-my-apps-secure-sign-in-extension"></a>Uygulamalarım Güvenli Oturum Açma Uzantısını Kullanma
* Uygulamalarım URL'si dışında `https://myapps.microsoft.com`bir URL kullanıyorsanız, varsayılan URL'nizi aşağıdakileri yaparak yapılandırın:
   1. Uzantıda oturum açmış *değilken,* uzantı simgesine sağ tıklayın.
   2. Menüde **Uygulamalarım URL'sini**seçin.
   3. Varsayılan URL'nizi seçin.
   4. Uzantı simgesini seçin.
   5. Uzantında oturum açabilmek için **başlamak için Oturum Aç'ı**seçin.

* Doğrudan tarayıcıdan bir uygulamada oturum açabilmek için aşağıdakileri yapın:
   1. Uzantıyı yükledikten **sonra, başlamak için Oturum Aç'ı**seçerek oturum açın.
   2. Oturum açma URL'si ile uygulamada oturum açın.  
       Oturum açma URL'si genellikle oturum açma formunu görüntüleyen uygulamanın URL'sidir.
      Uzantı durumu değiştirmeli ve bir parolanın kullanılabilir olduğunu size bildirmelidir.
   3. Oturum açabilmek için uzantı simgesini seçin.

* Uzantıdan bir uygulama başlatmak için aşağıdakileri yapın:
   1. Uzantıyı yükledikten **sonra, başlamak için Oturum Aç'ı**seçerek oturum açın.
   2. Menüsünü açmak için uzantı simgesini seçin.
   3. Uygulamalarım portalında kullanılabilen bir uygulamayı arayın.
   4. Arama sonuçları listesinde uygulamayı seçin.  
       Kullandığınız son üç **uygulama, Son Kullanılan** kısayol listesinde görüntülenir.
       
* Uzakken dahili şirket URL'lerini kullanmak için aşağıdakileri yapın:
    1. [Kiracınızda Uygulama Proxy'si yapılandır](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable)
    2. Uygulama Proxy üzerinden uygulama ve URL [yayımlayın](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal)
    3. Uzantıyı yükleyin ve başlamak için Oturum Aç'ı seçerek bu uzantıda oturum açın
    4. Artık uzaktayken bile dahili şirket URL'sine göz atabilirsiniz

> [!NOTE]
> Önceki seçenekler yalnızca Microsoft Edge, Chrome ve Firefox için kullanılabilir.

## <a name="set-up-a-group-policy-for-internet-explorer"></a>Internet Explorer için bir grup ilkesi ayarlama

Internet Explorer için erişim paneli uzantısını kullanıcılarınızın makinelerine uzaktan yüklemenize olanak tanıyan bir grup ilkesi ayarlayabilirsiniz.

Bir grup ilkesi ayarlamadan önce şunları emin olun:

-   [Active Directory Domain Services'ı](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx)kurdunuz ve kullanıcılarınızın makinelerini etki alanınıza katıldınız.

-   Grup İlkesi Nesnesini (GPO) yeniden ayarlamak için *ayarları ayarlarını ediniz* gerekir. Varsayılan olarak, bu izin aşağıdaki güvenlik gruplarının üyelerine verilir: etki alanı yöneticileri, kurumsal yöneticiler ve grup ilkesi oluşturucusu sahipleri.

Grup ilkesini yapılandırma ve kullanıcılara dağıtma ile ilgili adım adım talimatlar için, [grup ilkesini kullanarak Internet Explorer için erişim paneli uzantısını dağıt'a](deploy-access-panel-browser-extension.md)bakın.

## <a name="troubleshoot-the-access-panel-extension-in-internet-explorer"></a>Internet Explorer'daki erişim paneli uzantısını giderme

Bir tanılama aracına ve Internet Explorer uzantısını yapılandırma yla ilgili bilgilere erişmek [için, Internet Explorer için erişim paneli uzantısısorun](manage-access-panel-browser-extension.md)giderme'ye bakın.

> [!NOTE]
> Internet Explorer sınırlı destek tedir ve artık yeni yazılım güncelleştirmeleri almaz. Microsoft Edge önerilen tarayıcıdır.

## <a name="if-the-preceding-steps-do-not-resolve-the-issue"></a>Önceki adımlar sorunu çözmezse

Varsa, aşağıdaki bilgileri içeren bir destek bileti açın:

-   Korelasyon hatası kimliği
-   UPN (kullanıcı e-posta adresi)
-   Kiracı Kimliği
-   Tarayıcı türü
-   Saat dilimi ve hatanın oluştuğu saat veya zaman dilimi
-   Kemancı izleri

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](what-is-single-sign-on.md)
