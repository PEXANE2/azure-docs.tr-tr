---
title: Güvenlik duvarının arkasındaki Anahtar Kasasına Erişin - Azure Key Vault | Microsoft Dokümanlar
description: Güvenlik duvarının arkasındaki bir uygulamadan Azure Anahtar Kasasına nasıl erişebileceğinizi öğrenin
services: key-vault
author: amitbapat
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: ambapat
ms.openlocfilehash: 42b4eab5f2417f97ab3d37274e6fca1965eca95f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79457466"
---
# <a name="access-azure-key-vault-behind-a-firewall"></a>Güvenlik duvarının ardındayken Azure Anahtar Kasası’na erişme

## <a name="what-ports-hosts-or-ip-addresses-should-i-open-to-enable-my-key-vault-client-application-behind-a-firewall-to-access-key-vault"></a>Güvenlik duvarının arkasındaki anahtar kasa istemci uygulamamın anahtar kasasına erişmesini sağlamak için hangi bağlantı noktalarını, ana bilgisayarları veya IP adreslerini açmalıyım?

Bir anahtar kasasına erişmek için, anahtar kasası istemci uygulamanızın çeşitli işlevlere ilişkin birden çok uç noktaya erişebilmesi gerekir:

* Azure Active Directory aracılığıyla kimlik doğrulama (Azure AD).
* Azure Anahtar Kasası'nın yönetimi. Buna, Azure Resource Manager aracılığıyla erişim ilkeleri oluşturma, okuma, güncelleştirme, silme ve ayarlama da dahildir.
* Key Vault'ta depolanan nesnelere (anahtarlar ve sırlar) erişme ve yönetme, Key [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)Vault'a özgü bitiş noktasından (örneğin).  

Yapılandırmanıza ve ortamınıza bağlı olarak, bazı farklılıklar mevcuttur.

## <a name="ports"></a>Bağlantı Noktaları

Üç işlev (kimlik doğrulama, yönetim ve veri düzlemi erişimi) için de anahtar kasası trafiği HTTPS: bağlantı noktası 443 üzerinden gider. Ancak CRL için zaman zaman HTTP (bağlantı noktası 80) trafiği de olacaktır. OCSP'yi destekleyen istemciler CRL'ye ulaşmamalı, ancak zaman zaman [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl).  

## <a name="authentication"></a>Kimlik doğrulaması

Anahtar kasası istemci uygulamasının, kimlik doğrulaması için Azure Active Directory uç noktalarına erişmesi gerekir. Kullanılan uç nokta, Azure AD kiracı yapılandırmasına, sorumlu türüne (kullanıcı sorumlusu veya hizmet sorumlusu) ve hesap türüne (örneğin, Microsoft hesabı ya da iş veya okul hesabı) bağlıdır.  

| Sorumlu türü | Uç nokta:bağlantı noktası |
| --- | --- |
| Microsoft hesabı kullanan kullanıcı<br> (örneğin, user@hotmail.com) |**Küresel:**<br> login.microsoftonline.com:443<br><br> **Azure Çin:**<br> login.chinacloudapi.cn:443<br><br>**Azure ABD Hükümeti:**<br> login.microsoftonline.us:443<br><br>**Azure Almanya:**<br> login.microsoftonline.de:443<br><br> ve <br>login.live.com:443 |
| Azure AD ile iş veya okul hesabı kullanan kullanıcı veya hizmet sorumlusu (örneğin, user@contoso.com) |**Küresel:**<br> login.microsoftonline.com:443<br><br> **Azure Çin:**<br> login.chinacloudapi.cn:443<br><br>**Azure ABD Hükümeti:**<br> login.microsoftonline.us:443<br><br>**Azure Almanya:**<br> login.microsoftonline.de:443 |
| İş veya okul hesabı ve Active Directory Federasyon Hizmetleri (AD FS) veya başka bir federasyon uç noktası kullanan kullanıcı veya hizmet sorumlusu (örneğin, user@contoso.com) |İş veya okul hesabı için tüm uç noktalar ve AD FS veya diğer federasyon uç noktaları |

Farklı olası karmaşık senaryolar da mevcuttur. Daha fazla bilgi için bkz. [Azure Active Directory Kimlik Doğrulaması Akışı](../active-directory/develop/authentication-scenarios.md), [Uygulamaları Azure Active Directory ile Tümleştirme](../active-directory/develop/active-directory-how-to-integrate.md) ve [Active Directory Kimlik Doğrulaması Protokolleri](https://msdn.microsoft.com/library/azure/dn151124.aspx).  

## <a name="key-vault-management"></a>Anahtar Kasası yönetimi

Anahtar Kasası yönetimi için (CRUD ve erişim ilkesi ayarı), anahtar kasası istemci uygulamasının Azure Resource Manager uç noktasına erişmesi gerekir.  

| İşlem türü | Uç nokta:bağlantı noktası |
| --- | --- |
| Anahtar Kasası denetim düzlemi işlemleri<br> Azure Resource Manager yoluyla |**Küresel:**<br> management.azure.com:443<br><br> **Azure Çin:**<br> management.chinacloudapi.cn:443<br><br> **Azure ABD Hükümeti:**<br> management.usgovcloudapi.net:443<br><br> **Azure Almanya:**<br> management.microsoftazure.de:443 |
| Microsoft Graph API'si |**Küresel:**<br> graph.microsoft.com:443<br><br> **Azure Çin:**<br> graph.chinacloudapi.cn:443<br><br> **Azure ABD Hükümeti:**<br> graph.microsoft.com:443<br><br> **Azure Almanya:**<br> graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>Anahtar Kasası işlemleri

Tüm anahtar kasası nesne (anahtarlar ve gizli anahtarlar) yönetimi ve şifreleme işlemleri için, anahtar kasası istemcisinin anahtar kasası uç noktasına erişmesi gerekir. Uç nokta DNS soneki, anahtar kasanızın konumuna bağlı olarak farklılık gösterir. Anahtar kasası uç noktası, aşağıdaki tabloda gösterildiği gibi *vault-name*.*region-specific-dns-suffix* biçimindedir.  

| İşlem türü | Uç nokta:bağlantı noktası |
| --- | --- |
| Anahtarlar üzerindeki şifreleme işlemlerini de içeren işlemler, anahtarları ve gizli anahtarları oluşturma, okuma, güncelleştirme ve silme, anahtar kasası nesnelerindeki etiketleri ve diğer öznitelikleri (anahtarlar ya da gizli anahtarlar) ayarlama veya alma |**Küresel:**<br> &lt;vault-name&gt;.vault.azure.net:443<br><br> **Azure Çin:**<br> &lt;vault-name&gt;.vault.azure.cn:443<br><br> **Azure ABD Hükümeti:**<br> &lt;vault-name&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Almanya:**<br> &lt;vault-name&gt;.vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>IP adresi aralıkları

Anahtar Kasası hizmeti, PaaS altyapısı gibi diğer Azure kaynaklarını kullanır. Bu nedenle, Anahtar Kasası hizmet uç noktalarının belirli bir zamanda sahip olacağı IP adresleri için özel bir aralık belirtmek mümkün değildir. Güvenlik duvarınız yalnızca IP adresi aralıklarını destekliyorsa [Microsoft Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/download/details.aspx?id=41653) belgesini inceleyin. Kimlik doğrulama ve Kimlik (Azure Active Directory) genel bir hizmettir ve diğer bölgelere yük devredebilir veya trafiği bilgi vermeden taşıyabilir. Bu senaryoda, [Kimlik Doğrulama ve Kimlik IP Adresleri](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity_ip)’nde listelenen tüm IP aralıklarının güvenlik duvarına eklenmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Key Vault hakkında sorularınız varsa Azure [Anahtar Kasası Forumlarını](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)ziyaret edin.
