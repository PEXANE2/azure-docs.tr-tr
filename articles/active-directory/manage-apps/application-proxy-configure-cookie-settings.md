---
title: Uygulama Proxy çerez ayarları - Azure Active Directory | Microsoft Dokümanlar
description: Azure Active Directory (Azure AD), Uygulama Proxy aracılığıyla şirket içi uygulamalara erişmek için erişim ve oturum tanımlama bilgilerine sahiptir. Bu makalede, çerez ayarlarını nasıl kullanacağınızı ve yapılandıracağınızı öğreneceksiniz.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: bcedb24a0efdbabaaef150fc3d5aff07d210ce23
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481373"
---
# <a name="cookie-settings-for-accessing-on-premises-applications-in-azure-active-directory"></a>Azure Active Directory'de şirket içi uygulamalara erişmek için çerez ayarları

Azure Active Directory (Azure AD), Uygulama Proxy aracılığıyla şirket içi uygulamalara erişmek için erişim ve oturum tanımlama bilgilerine sahiptir. Uygulama Proxy çerez ayarlarını nasıl kullanacağınızı öğrenin. 

## <a name="what-are-the-cookie-settings"></a>Çerez ayarları nelerdir?

[Application Proxy](application-proxy.md) aşağıdaki erişim ve oturum çerez ayarlarını kullanır.

| Çerez ayarı | Varsayılan | Açıklama | Öneriler |
| -------------- | ------- | ----------- | --------------- |
| Yalnızca HTTP Çerezi Kullanma | **Hayır** | **Evet,** Uygulama Proxy'nin HTTP yanıt üstbilgilerine HTTPOnly bayrağını eklemesine izin verir. Bu bayrak ek güvenlik avantajları sağlar, örneğin, istemci tarafı komut dosyası (CSS) tanımlama bilgilerini kopyalamasını veya değiştirmesini engeller.<br></br><br></br>Yalnızca HTTP ayarını desteklemeden önce, Uygulama Proxy'si çerezleri güvenli bir TLS kanalı üzerinden şifreledi ve ileterek değişikliklere karşı koruma sağladı. | Ek güvenlik avantajları nedeniyle **Evet'i** kullanın.<br></br><br></br>Oturum çerezine erişim gerektiren istemciler veya kullanıcı aracıları için **Hayır'ı** kullanın. Örneğin, Uygulama Proxy üzerinden Uzak Masaüstü Ağ Geçidi sunucusuna bağlanan bir RDP veya MTSC istemcisi için **Hayır'ı** kullanın.|
| Güvenli Çerez Kullanın | **Hayır** | **Evet,** Uygulama Proxy'nin HTTP yanıt başlıklarına Güvenli bayrağı eklemesine izin verir. Güvenli Çerezler, https gibi TLS güvenli bir kanal üzerinden çerezleri ileterek güvenliği artırır. Bu, tanımlama bilgilerinin açık metin olarak iletilmesi nedeniyle çerezlerin yetkisiz taraflarca gözlemlemesini önler. | Ek güvenlik avantajları nedeniyle **Evet'i** kullanın.|
| Kalıcı Çerez Kullanın | **Hayır** | **Evet,** Uygulama Proxy'nin erişim çerezlerini web tarayıcısı kapatıldığında süresinin dolmasına göre ayarlamasına olanak tanır. Kalıcılık, erişim belirteci sona erene veya kullanıcı kalıcı tanımlama bilgilerini el ile silene kadar sürer. | Kullanıcıların kimliğinin doğrulanmış tutulmasıyla ilişkili güvenlik riski nedeniyle **Hayır'ı** kullanın.<br></br><br></br>Çerezleri işlemler arasında paylaşamayan eski uygulamalar için yalnızca **Evet** kullanmanızı öneririz. Kalıcı tanımlama bilgileri kullanmak yerine çerezleri işlemler arasında paylaşmayı işlemek için uygulamanızı güncelleştirmek daha iyidir. Örneğin, bir kullanıcının Office belgelerini SharePoint sitesinden explorer görünümünde açmasına izin vermek için kalıcı tanımlama bilgilerine ihtiyacınız olabilir. Kalıcı tanımlama bilgileri olmadan, erişim çerezleri tarayıcı, explorer işlemi ve Office işlemi arasında paylaşılmazsa bu işlem başarısız olabilir. |

## <a name="samesite-cookies"></a>SameSite Çerezleri
Chrome 80 sürümünden başlayarak ve sonunda Krom'dan yararlanan tarayıcılarda, [SameSite](https://web.dev/samesite-cookies-explained) özniteliğini belirtmeyan tanımlama bilgileri **SameSite=Lax**olarak ayarlanmış gibi ele alınacaktır. SameSite özniteliği, tanımlama bilgilerinin aynı site bağlamıyla nasıl sınırlandırılması gerektiğini bildirir. Lax olarak ayarlandığında, çerez yalnızca aynı site isteklerine veya üst düzey gezinmeye gönderilir. Ancak, Uygulama Proxy kullanıcıların oturum sırasında düzgün oturum tutmak için bu çerezleri üçüncü taraf bağlamında korunmasını gerektirir. Bu nedenle, bu değişikliğin olumsuz etkilerini önlemek için Uygulama Proxy erişimi ve oturum tanımlama bilgileri için güncelleştirmeler yapıyoruz. Güncelleştirmeler şunlardır:

* Aynı **Site** özniteliğini **Yok'a**ayarlama. Bu, Uygulama Proxy erişimine ve oturum tanımlama bilgilerinin üçüncü taraf bağlamında düzgün bir şekilde gönderilmesine olanak tanır.
* Varsayılan olarak **Evet'i** kullanacak **Güvenli Çerez kullan** ayarını ayarlama. Chrome ayrıca çerezlerin Güvenli bayrağı belirtebini veya reddedilmesini gerektirir. Bu değişiklik, Application Proxy aracılığıyla yayınlanan tüm varolan uygulamalar için geçerli olacaktır. Application Proxy erişim tanımlama bilgilerinin her zaman Güvenli olarak ayarlandığını ve yalnızca HTTPS üzerinden aktarıldığını unutmayın. Bu değişiklik yalnızca oturum tanımlama bilgileri için geçerli olacaktır.

Uygulama Proxy tanımlama bilgilerinde yapılan bu değişiklikler, Chrome 80 çıkış tarihinden önceki birkaç hafta boyunca kullanıma sunulacaktır.

Ayrıca, arka uç uygulamanızda üçüncü taraf bağlamında kullanılabilir olması gereken tanımlama bilgileri varsa, bu çerezler için SameSite=None kullanmak üzere uygulamanızı değiştirerek açıkça kabul etmeniz gerekir. Application Proxy, Set-Cookie üstbilgisini URL'lerine çevirir ve arka uç uygulaması tarafından ayarlanan bu tanımlama bilgilerinin ayarlarına saygı gösterir.



## <a name="set-the-cookie-settings---azure-portal"></a>Çerez ayarlarını ayarlama - Azure portalı
Azure portalını kullanarak çerez ayarlarını ayarlamak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. 
2. Azure **Active Directory** > **Enterprise uygulamalarına** >gidin **Tüm uygulamalar**.
3. Çerez ayarını etkinleştirmek istediğiniz uygulamayı seçin.
4. **Uygulama Proxy'yi**tıklatın.
5. **Ek Ayarlar**altında, çerez ayarını **Evet** veya **Hayır**olarak ayarlayın.
6. Değişikliklerinizi uygulamak için **Kaydet**’e tıklayın. 

## <a name="view-current-cookie-settings---powershell"></a>Geçerli çerez ayarlarını görüntüleyin - PowerShell

Uygulamaiçin geçerli çerez ayarlarını görmek için şu PowerShell komutunu kullanın:  

```powershell
Get-AzureADApplicationProxyApplication -ObjectId <ObjectId> | fl * 
```

## <a name="set-cookie-settings---powershell"></a>Çerez ayarlarını ayarlama - PowerShell

Aşağıdaki PowerShell komutlarında, ```<ObjectId>``` uygulamanın ObjectId'si ve 

**Sadece Çerez** 

```powershell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsHttpOnlyCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsHttpOnlyCookieEnabled $false 
```

**Güvenli Çerez**

```powershell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsSecureCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsSecureCookieEnabled $false 
```

**Kalıcı Çerezler**

```powershell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsPersistentCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsPersistentCookieEnabled $false 
```
