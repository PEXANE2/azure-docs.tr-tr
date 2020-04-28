---
title: Azure AD Connect-Azure AD Connect kullanarak Azure AD ile AD FS güveni yönetme | Microsoft Docs
description: Azure AD Connect tarafından Azure AD güven işleme 'nin işletimsel ayrıntıları.
keywords: AD FS, ADFS, AD FS Yönetimi, AAD Connect, Connect, Azure AD, güven, AAD, talep, talep, talep kuralları, verme, dönüşüm, kurallar, yedekleme, geri yükleme
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80331732"
---
# <a name="manage-ad-fs-trust-with-azure-ad-using-azure-ad-connect"></a>Azure AD Connect kullanarak Azure AD ile AD FS güvenini yönetme

## <a name="overview"></a>Genel Bakış

Azure AD Connect, şirket içi Active Directory Federasyon Hizmeti (AD FS) ve Azure AD arasında Federasyonu yönetebilir. Bu makalede bir genel bakış sunulmaktadır:

* Azure AD Connect tarafından güveninde yapılandırılan çeşitli ayarlar
* Azure AD Connect tarafından ayarlanan verme dönüştürme kuralları (talep kuralları)
* Yükseltme ve yapılandırma güncelleştirmeleri arasında talep kurallarınızı yedekleme ve geri yükleme. 

## <a name="settings-controlled-by-azure-ad-connect"></a>Azure AD Connect tarafından denetlenen ayarlar

Azure AD Connect **yalnızca** Azure AD güveniyle ilgili ayarları yönetir. Azure AD Connect, AD FS diğer bağlı olan taraf güvenlerinde herhangi bir ayarı değiştirmez. Aşağıdaki tablo Azure AD Connect tarafından denetlenen ayarları gösterir.

| Ayar | Açıklama |
| :--- | :--- |
| Belirteç imzalama sertifikası | Azure AD Connect, Azure AD ile güveni sıfırlamak ve yeniden oluşturmak için kullanılabilir. Azure AD Connect, belirteç imzalama sertifikalarının AD FS ve Azure AD etki alanı Federasyon ayarlarını güncelleştiren bir kerelik anında geçiş yapar.|
| Belirteç imzalama algoritması | Microsoft, belirteç imzalama algoritması olarak SHA-256 kullanmasını önerir. Azure AD Connect, belirteç imzalama algoritmasının SHA-256 ' den daha az güvenli bir değere ayarlandığını algılayabilir. Sonraki olası yapılandırma işleminde bu ayarı SHA-256 olarak güncelleştirir. Yeni belirteç imzalama sertifikasını kullanmak için diğer bağlı olan taraf güveni de güncelleştirilmeleri gerekir. |
| Azure AD güven tanımlayıcısı | Azure AD Connect, Azure AD güveninin doğru tanımlayıcı değerini ayarlar. AD FS, Azure AD güvenini tanımlayıcı değeri kullanarak benzersiz şekilde tanımlar. |
| Azure AD uç noktaları | Azure AD Connect, Azure AD güveni için yapılandırılan uç noktaların dayanıklılık ve performans için en son önerilen değerlere göre her zaman olduğundan emin olur. |
| Verme dönüştürme kuralları | Federasyon ayarındaki Azure AD özelliklerinin en iyi performansı için gereken sayıda talep kuralı vardır. Azure AD Connect, Azure AD güveninin her zaman önerilen talep kuralları kümesiyle yapılandırıldığından emin olur. |
| Alternatif kimlik | Eşitleme, alternatif kimliği kullanacak şekilde yapılandırıldıysa Azure AD Connect, alternatif kimliği kullanarak kimlik doğrulaması gerçekleştirmek için AD FS yapılandırır. |
| Otomatik meta veri güncelleştirmesi | Azure AD ile güven otomatik meta veri güncelleştirmesi için yapılandırılmıştır. AD FS Azure AD güveninin meta verilerini düzenli aralıklarla denetler ve Azure AD tarafında değiştiği zaman güncel tutar. |
| Tümleşik Windows kimlik doğrulaması (ıWA) | Karma Azure AD JOIN işlemi sırasında, alt düzey cihazlarda karma Azure AD birleştirmesini kolaylaştırmak için ıWA cihaz kaydı için etkinleştirilir |

## <a name="execution-flows-and-federation-settings-configured-by-azure-ad-connect"></a>Azure AD Connect tarafından yapılandırılan yürütme akışları ve Federasyon ayarları

Azure AD Connect, yapılandırma akışları sırasında Azure AD güveninin tüm ayarlarını güncelleştirmez. Değiştirilen ayarlar hangi görev veya yürütme akışının yürütüldüğü üzerinde değişir. Aşağıdaki tablo, farklı yürütme akışlarında etkilenen ayarları listeler.

| Yürütme akışı | Etkilenen ayarlar |
| :--- | :--- |
| İlk geçiş yükleme (Express) | Hiçbiri |
| Yüklemeyi ilk geçir (yeni AD FS grubu) | Yeni bir AD FS grubu oluşturulur ve sıfırdan Azure AD ile bir güven oluşturulur. |
| İlk geçiş yükleme (mevcut AD FS grubu, mevcut Azure AD güveni) | Azure AD güven tanımlayıcısı, verme dönüştürme kuralları, Azure AD uç noktaları, alternatif kimliği (gerekliyse), otomatik meta veri güncelleştirmesi |
| Azure AD güvenini sıfırlama | Belirteç imzalama sertifikası, belirteç imzalama algoritması, Azure AD güven tanımlayıcısı, verme dönüştürme kuralları, Azure AD uç noktaları, alternatif kimlik (gerekliyse), otomatik meta veri güncelleştirmesi |
| Federasyon sunucusu Ekle | Hiçbiri |
| WAP sunucusu Ekle | Hiçbiri |
| Cihaz seçenekleri | Verme dönüştürme kuralları, cihaz kaydı için ıWA |
| Federasyon etki alanı Ekle | Etki alanı ilk kez eklendiyse, diğer bir deyişle, kurulum tek etki alanı federasyonunu birden çok etki alanı federasyona değiştiriyor – Azure AD Connect güveni sıfırdan yeniden oluşturur. Azure AD ile güven zaten birden çok etki alanı için yapılandırılmışsa, yalnızca verme dönüşüm kuralları değiştirilir |
| TLS güncelleştirme | Hiçbiri |

Tüm işlemler sırasında, herhangi bir ayar değiştirildiğinde, Azure AD Connect **%ProgramData%\aadconnect\adfs** konumundaki geçerli güven ayarlarının yedeklemesini yapar

![Mevcut Azure AD güven yedeğiyle ilgili iletiyi gösteren Azure AD Connect sayfası](./media/how-to-connect-azure-ad-trust/backup2.png)

> [!NOTE]
> Sürüm 1.1.873.0 ' den önce, yedekleme yalnızca verme dönüşüm kurallarından oluşur ve sihirbaz izleme günlük dosyasında yedeklenir.

## <a name="issuance-transform-rules-set-by-azure-ad-connect"></a>Azure AD Connect tarafından ayarlanan verme dönüştürme kuralları

Azure AD Connect, Azure AD güveninin her zaman önerilen talep kuralları kümesiyle yapılandırıldığından emin olur. Microsoft, Azure AD güvenini yönetmek için Azure AD Connect 'in kullanılmasını önerir. Bu bölümde, verme dönüştürme kuralları kümesi ve bunların açıklaması listelenmektedir.

| Kural adı | Açıklama |
| --- | --- |
| Sorun UPN 'si | Bu kural userPrincipalName değerini, userPrincipalName için eşitleme ayarları 'nda yapılandırılan öznitelikten farklı şekilde sorgular.|
| Özel ImmutableID talebi için Objectguıd ve msdsıımıbu GUID sorgula | Bu kural, varsa Objectguıd ve MSDS, şifreli GUID değeri için işlem hattına geçici bir değer ekler |
| MSDS, şifreli GUID 'nin varlığını denetle | Msdsıse \ GUID değerinin var olup olmadığına bağlı olarak, ImmutableID olarak kullanılacak öğeleri yönlendirmek için geçici bir bayrak belirledik. |
| Msdsımnguıd, varsa sabit KIMLIK olarak verme | Değer varsa ImmutableID olarak msdsı |
| Msdsst, Guid kuralı yoksa objectGuidRule verme | Msdsıse, Guid değeri yoksa, objectGUID değeri ImmutableID olarak verilecek |
| Sorun NameIdentifier | Bu kural, NameIdentifier talebi için değer verir.|
| Etki alanına katılmış bilgisayarlar için sorun AccountType | Kimliği doğrulanan varlık etki alanına katılmış bir cihaz ise, bu kural, etki alanına katılmış bir cihazı belirten DJ olarak hesap türünü verir |
| Bilgisayar hesabı olmadığında Kullanıcı değeri ile AccountType sorunu | Kimliği doğrulanan varlık bir kullanıcı ise, bu kural hesap türünü Kullanıcı olarak yayınlar |
| Bilgisayar hesabı olmadığında ıssuerıd verme | Bu kural, kimlik doğrulama varlığı bir cihaz olmadığında ıssuerıd değerini yayınlar. Değer, Azure AD Connect tarafından yapılandırılan bir Regex ile oluşturulur. Regex, Azure AD Connect kullanılarak federe tüm etki alanlarının dikkate alınması sırasında oluşturulur. |
| DJ bilgisayar kimlik doğrulaması için sorun ıssuerıd | Bu kural, kimlik doğrulama varlığı bir cihaz olduğunda ıssuerıd değerini yayınlar |
| Etki alanına katılmış bilgisayarlar için onpremobjectguid 'i verme | Kimliği doğrulanan varlık etki alanına katılmış bir cihaz ise, bu kural cihaz için şirket içi objectGUID değerini yayınlar |
| Geçiş birincil SID 'SI | Bu kural, kimlik doğrulama varlığının birincil SID 'sini verir |
| Doğrudan geçiş talebi-insideCorporateNetwork | Bu kural, kimlik doğrulamasının kurumsal ağ içinden mi yoksa dışarıdan mi geldiğini Azure AD 'ye yardımcı olan bir talep verir |
| Doğrudan geçiş talebi – PSSO |   |
| Parola süre sonu taleplerini verme | Bu kural parola sona erme saati için üç talep, kimlik doğrulaması yapılan varlığın süresinin dolması için gereken gün sayısını ve parolanın değiştirilmesi için kullanılacak URL 'YI verir.|
| Doğrudan geçiş talebi – authnmethodsreferences | Bu kural altında verilen talepteki değer, varlık için hangi tür kimlik doğrulamanın gerçekleştirildiğini belirtir |
| Doğrudan geçiş talebi-multifactorauthenticationinstant | Bu talebin değeri, kullanıcının en son birden çok öğeli kimlik doğrulaması gerçekleştirdiği süreyi UTC olarak belirtir. |
| Doğrudan geçiş talebi-AlternateLoginID | Bu kural, kimlik doğrulaması alternatif oturum açma KIMLIĞI kullanılarak gerçekleştirildiyse AlternateLoginID talebini yayınlar. |

> [!NOTE]
> Azure AD Connect yapılandırması sırasında varsayılan olmayan seçeneği kullandığınızda, UPN ve ImmutableID için sorun için talep kuralları farklı olacaktır

## <a name="restore-issuance-transform-rules"></a>Verme dönüştürme kurallarını geri yükleme

Azure AD Connect Version 1.1.873.0 veya sonraki sürümleri, Azure AD güven ayarlarında bir güncelleştirme yapıldığında Azure AD güven ayarlarının yedeklenmesini sağlar. Azure AD güven ayarları, **%ProgramData%\aadconnect\adfs**konumunda yedeklenir. Dosya adı,&lt;aadtrust-Date&gt;-&lt;&gt;. txt, örneğin-AadTrust-20180710-150216. txt biçiminde şu biçimdedir.

![Azure AD güveninin örnek bir ekran görüntüsü](./media/how-to-connect-azure-ad-trust/backup.png)

Aşağıdaki önerilen adımları kullanarak verme dönüştürme kurallarını geri yükleyebilirsiniz

1. AD FS Yönetimi Kullanıcı arabirimini Sunucu Yöneticisi açın
2. **AD FS &gt; bağlı olan taraf &gt; güvenleri Microsoft Office 365 kimlik platformu &gt; düzenleme TALEPLERI verme ilkesi** ' ne giderek Azure AD güven özelliklerini açın
3. **Kural Ekle** 'ye tıklayın
4. Talep kuralı şablonunda, talepleri özel bir kural kullanarak gönder ' i seçin ve **İleri** ' ye tıklayın.
5. Talep kuralının adını yedekleme dosyasından kopyalayın ve alan **talep kuralı adına** yapıştırın
6. Talep kuralını yedekleme dosyasından **özel kural** için metin alanına kopyalayın ve **son** ' a tıklayın.

> [!NOTE]
> Ek kurallarınızın Azure AD Connect tarafından yapılandırılan kurallarla çakışmadığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Connect kullanarak Active Directory Federasyon Hizmetleri (AD FS) yönetin ve özelleştirin](how-to-connect-fed-management.md)
