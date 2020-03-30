---
title: Azure AD Connect - Azure AD Connect'i kullanarak AZURE AD ile AD FS güvenini yönetin | Microsoft Dokümanlar
description: Azure AD tarafından azure AD güven işlemenin operasyonel ayrıntıları bağlanır.
keywords: AD FS, ADFS, AD FS yönetimi, AAD Connect, Connect, Azure AD, güven, AAD, talep, talep, talep kuralları, verme, dönüştürme, kurallar, yedekleme, geri yükleme
services: active-directory
documentationcenter: ''
ms.reviewer: anandyadavmsft
manager: daveba
ms.subservice: hybrid
ms.assetid: 2593b6c6-dc3f-46ef-8e02-a8e2dc4e9fb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/28/2018
ms.author: billmath
author: billmath
ms.custom: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0f3e521fb7668305ce511aaddd63ed2cce8dfed0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80331732"
---
# <a name="manage-ad-fs-trust-with-azure-ad-using-azure-ad-connect"></a>Azure AD Connect kullanarak Azure AD ile AD FS güvenini yönetme

## <a name="overview"></a>Genel Bakış

Azure AD Connect, şirket içi Active Directory Federation Service (AD FS) ve Azure AD arasındaki federasyonu yönetebilir. Bu makalede, genel bir bakış sağlar:

* Azure AD Connect tarafından güven üzerine yapılandırılan çeşitli ayarlar
* Azure AD Connect tarafından ayarlanan verme dönüştürme kuralları (talep kuralları)
* Yükseltmeler ve yapılandırma güncelleştirmeleri arasındaki talep kurallarınızı yedekleme ve geri yükleme. 

## <a name="settings-controlled-by-azure-ad-connect"></a>Azure AD Connect tarafından denetlenir ayarlar

Azure AD Connect **yalnızca** Azure AD güveniyle ilgili ayarları yönetir. Azure AD Connect, AD FS'deki diğer bağlı taraf güvenlerinin ayarlarını değiştirmez. Aşağıdaki tablo, Azure AD Connect tarafından denetlenir ayarları gösterir.

| Ayar | Açıklama |
| :--- | :--- |
| Belirteç imzalama sertifikası | Azure AD Connect, Azure AD ile güveni sıfırlamak ve yeniden oluşturmak için kullanılabilir. Azure AD Connect, AD FS için belirteç imzalama sertifikalarının bir defaya sık rollover'ını yapar ve Azure AD etki alanı federasyonu ayarlarını güncelleştirir.|
| Belirteç imzalama algoritması | Microsoft, belirteç imzalama algoritması olarak SHA-256'nın kullanılmasını önerir. Azure AD Connect, belirteç imzalama algoritmasının SHA-256'dan daha az güvenli bir değere ayarlı olup olmadığını algılayabilir. Bir sonraki olası yapılandırma işleminde ayarı SHA-256'ya günceller. Yeni belirteç imzalama sertifikasını kullanmak için diğer güvenen taraf güveni güncelleştirilmelidir. |
| Azure AD güven tanımlayıcısı | Azure AD Connect, Azure AD güveni için doğru tanımlayıcı değerini ayarlar. AD FS, tanımlayıcı değerini kullanarak Azure AD güvenini benzersiz bir şekilde tanımlar. |
| Azure AD bitiş noktaları | Azure AD Connect, Azure AD güveni için yapılandırılan uç noktaların her zaman esneklik ve performans için önerilen en son değerlere uygun olmasını sağlar. |
| Dönüştürme kuralları verme | Azure AD özelliklerinin federe bir ortamda en iyi performansı için gereken çok sayıda talep kuralı vardır. Azure AD Connect, Azure AD güveninin her zaman doğru önerilen talep kuralları kümesiyle yapılandırıldığından emin olmasını sağlar. |
| Alternatif kimlik | Eşitleme alternatif id kullanacak şekilde yapılandırılırsa, Azure AD Connect ad FS'yi alternatif kimlik kullanarak kimlik doğrulaması gerçekleştirmek üzere yapılandırır. |
| Otomatik meta veri güncelleştirmesi | Azure AD ile güven, otomatik meta veri güncelleştirmesi için yapılandırılır. AD FS, Azure AD güveninin meta verilerini düzenli aralıklarla denetler ve Azure AD tarafında değişiklik olması durumunda bu verileri güncel tutar. |
| Tümleşik Windows Kimlik Doğrulama (IWA) | Karma Azure AD birleştirme işlemi sırasında IWA, alt düzey aygıtlar için Karma Azure AD birleştirme işlemlerini kolaylaştırmak için aygıt kaydı için etkinleştirilir |

## <a name="execution-flows-and-federation-settings-configured-by-azure-ad-connect"></a>Azure AD Connect tarafından yapılandırılan yürütme akışları ve federasyon ayarları

Azure AD bağlantısı, yapılandırma akışları sırasında Azure AD güveninin tüm ayarlarını güncelleştirmez. Değiştirilen ayarlar, hangi görev veya yürütme akışının yürütüldolduğuna bağlıdır. Aşağıdaki tabloda farklı yürütme akışlarında etkilenen ayarlar listelenir.

| Yürütme akışı | Etkilenen ayarlar |
| :--- | :--- |
| İlk geçiş yüklemesi (ekspres) | None |
| İlk geçiş yüklemesi (yeni AD FS çiftliği) | Yeni bir AD FS çiftliği oluşturulur ve Azure AD ile bir güven sıfırdan oluşturulur. |
| İlk geçiş yüklemesi (varolan AD FS çiftliği, varolan Azure AD güveni) | Azure AD güven tanımlayıcısı, Dönüştürme kuralları verme, Azure AD uç noktaları, Alternatif kimlik (gerekirse), otomatik meta veri güncelleştirmesi |
| Azure AD güvenini sıfırla | Belirteç imzalama sertifikası, Belirteç imzalama algoritması, Azure AD güven tanımlayıcısı, Dönüştürme kuralları verme, Azure AD uç noktaları, Alternatif id (gerekirse), otomatik meta veri güncelleştirmesi |
| Federasyon sunucusu ekleme | None |
| WAP sunucusu ekle | None |
| Cihaz seçenekleri | Verme dönüştürme kuralları, cihaz kaydı için IWA |
| Federe etki alanı ekleme | Etki alanı ilk kez ekleniyorsa, yani kurulum tek etki alanı federasyonundan çok etki alanı federasyonuna değişiyorsa – Azure AD Connect güveni sıfırdan yeniden oluşturur. Azure AD ile güven zaten birden çok etki alanı için yapılandırıldıysa, yalnızca Verme dönüştürme kuralları değiştirilir |
| TLS'yi güncelleştir | None |

Azure AD Connect, herhangi bir ayarın değiştirildiği tüm işlemler sırasında **%ProgramData%\AADConnect\ADFS'de** geçerli güven ayarlarını yedekler

![Mevcut Azure AD güven yedeklemesi ile ilgili iletiyi gösteren Azure AD Connect sayfası](./media/how-to-connect-azure-ad-trust/backup2.png)

> [!NOTE]
> Sürüm 1.1.873.0'dan önce, yedekleme yalnızca verme dönüştürme kurallarından oluşuyordu ve bunlar sihirbaz izleme günlüğü dosyasında yedeklendi.

## <a name="issuance-transform-rules-set-by-azure-ad-connect"></a>Azure AD Connect tarafından belirlenen dönüştürme kurallarını verme

Azure AD Connect, Azure AD güveninin her zaman doğru önerilen talep kuralları kümesiyle yapılandırıldığından emin olmasını sağlar. Microsoft, Azure REKLAM güveninizi yönetmek için Azure AD bağlantısını kullanmanızı önerir. Bu bölümde, verme dönüştürme kuralları kümesi ve bunların açıklaması listelenebedilir.

| Kural adı | Açıklama |
| --- | --- |
| Sorun UPN | Bu kural, kullanıcı adı için eşitleme ayarlarında yapılandırılan öznitelikteki kullanıcı adı değerini sorgular.|
| Özel ImmutableId iddiası için objectguid ve msdsconsistencyguid sorgula | Bu kural, varsa objectguid ve msdsconsistencyguid değeri için ardışık ardışık geçici bir değer ekler |
| msdsconsistencyguid varlığını kontrol edin | msdsconsistencyguid değerinin var olup olmadığına bağlı olarak, ImmutableId olarak kullanılacak ları yönlendirmek için geçici bir bayrak belirlir |
| Varsa Değişmez kimlik olarak msdsconsistencyguid sorunu | Değer varsa ImmutableId olarak msdsconsistencyguid sorunu |
| msdsConsistencyGuid kuralı yoksa sorun objectGuidRule | msdsconsistencyguid değeri yoksa, objectguid değeri ImmutableId olarak verilecektir |
| Sorun nameidentifier | Bu kural nameidentifier iddiası için değer sorunları.|
| Etki alanı birleştirilmiş bilgisayarlar için hesap türü sorunu | Kimlik doğrulaması yapılan varlık bir etki alanı birleştirilmiş aygıtsa, bu kural hesap türünü bir etki alanı nın birleştirilmiş aygıtı simgeleyen DJ olarak belirtir |
| Bilgisayar hesabı olmadığında Kullanıcı değeri ile Hesap Türünü Verme | Kimlik doğrulaması yapılan varlık bir kullanıcıysa, bu kural hesap türünü Kullanıcı olarak |
| Bilgisayar hesabı olmadığında sorun sorunu | Bu kural, kimlik doğrulaması olan varlık bir aygıt olmadığında verenId değerini bildirir. Değer, Azure AD Connect tarafından yapılandırılan bir regex aracılığıyla oluşturulur. Regex, Azure AD Connect kullanılarak beslenen tüm etki alanları dikkate alınarak oluşturulur. |
| DJ bilgisayar auth için sorun issuerid | Bu kural, kimlik doğrulaması sağlayan varlık bir aygıt olduğunda verenId değerini |
| Etki alanı birleştirilmiş bilgisayarlar için onpremobjectguid sorunu | Kimlik doğrulaması yapılan varlık bir etki alanı birleştirilmiş aygıtsa, bu kural aygıt için şirket içi nesneguid'i |
| Birincil SID'den geçme | Bu kural, kimlik doğrulaması olan varlığın birincil SID'sini |
| İddiadan geç - insideCorporateNetwork | Bu kural, Azure AD'nin kimlik doğrulamanın şirket ağı içinden mi yoksa dışarıdan mı geldiğini bilmesine yardımcı olan bir iddia yı |
| İddiadan Geç – Psso |   |
| Parola Son Kullanma Talepleri Sorunu | Bu kural, parola son kullanma süresi için üç talep, kimlik doğrulaması yapılan varlığın süresinin dolması için gün sayısı ve parolayı değiştirmek için yönlendirilebileceği niçin URL'yi belirtir.|
| İddiayoluyla geçiş – authnmethodsreferences | Bu kural uyarınca verilen talepteki değer, varlık için ne tür bir kimlik doğrulaması gerçekleştirildiğini gösterir |
| Talep yoluyla pass - multifactorauthenticationinstant | Bu talebin değeri, UTC'de kullanıcının en son birden çok faktörkimlik doğrulaması gerçekleştirdiği zamanı belirtir. |
| İddiadan geç - AlternativeLoginID | Bu kural, kimlik doğrulamasının alternatif giriş kimliği kullanılarak gerçekleştirilmesi durumunda AlternateLoginID iddiasını yayınlar. |

> [!NOTE]
> Azure AD Connect yapılandırması sırasında varsayılan olmayan seçim kullanırsanız, Issue UPN ve ImmutableId için talep kuralları farklı olacaktır

## <a name="restore-issuance-transform-rules"></a>Verme dönüştürme kurallarını geri yükleme

Azure AD Connect sürümü 1.1.873.0 veya daha sonra, Azure AD güven ayarlarında bir güncelleştirme yapıldığında Azure AD güven ayarlarının yedeğini yapar. Azure AD güven ayarları **%ProgramData%\AADConnect\ADFS'de**yedeklenir. Dosya adı aşağıdaki biçimde&lt;AadTrust-&gt;-&lt;&gt;tarih saati .txt, örneğin - AadTrust-20180710-150216.txt

![Azure AD güveninin yedekli örneğinin ekran görüntüsü](./media/how-to-connect-azure-ad-trust/backup.png)

Aşağıdaki önerilen adımları kullanarak verme dönüştürme kurallarını geri yükleyebilirsiniz

1. Server Manager'da AD FS yönetim ui'sini açın
2. **AD FS &gt; Relying Party Trusts &gt; Microsoft Office 365 Identity &gt; Platform Düzenleme İddiaları Verme İlkesi'ne** giderek Azure AD güven özelliklerini açın
3. Ekle **kuralına** tıklayın
4. Talep kuralı şablonunda, Özel Kural Kullanarak Talep Gönder'i seçin ve **İleri'yi** tıklatın
5. Talep kuralının adını yedekleme dosyasından kopyalayın ve alan **Claim kural adına** yapıştırın
6. Yedekleme dosyasından talep kuralını Özel **kural** için metin alanına kopyalayın ve **Bitiş'i** tıklatın

> [!NOTE]
> Ek kurallarınızın Azure AD Connect tarafından yapılandırılan kurallarla çakışmadığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Connect'i kullanarak Active Directory Federation Hizmetlerini yönetme ve özelleştirme](how-to-connect-fed-management.md)
