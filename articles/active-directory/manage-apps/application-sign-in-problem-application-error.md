---
title: Oturum açmadan sonra uygulama sayfasında hata iletisi görünür | Microsoft Dokümanlar
description: Uygulama bir hata iletisi döndürdüğünde Azure AD oturum açma ile ilgili sorunları nasıl gidereceğiniz.
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
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9b8d20b31e96973a492355f0515d0532deea0ac9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77185486"
---
# <a name="an-app-page-shows-an-error-message-after-the-user-signs-in"></a>Bir uygulama sayfası, kullanıcı tarafından işaretledikten sonra bir hata iletisi gösterir

Bu senaryoda, Azure Etkin Dizin (Azure AD) kullanıcıyı imzalar. Ancak uygulama bir hata iletisi görüntüler ve kullanıcının oturum açma akışını bitirmesine izin vermez. Sorun, uygulamanın Azure AD'nin verdiği yanıtı kabul etmemiş olmasıdır.

Uygulamanın Azure AD'den gelen yanıtı kabul etmemiş olmasının birkaç olası nedeni vardır. Hata iletisi yanıtta eksik olanları açıkça tanımlayamazsa, aşağıdakileri deneyin:

-   Uygulama Azure REKLAM galerisiyse, Azure AD'deki [uygulamalarda SAML tabanlı tek oturum açma hataayıklama adımlarını](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging)izlediğinizi doğrulayın.

-   SAML isteğini, yanıtı ve belirteci yakalamak için [Fiddler](https://www.telerik.com/fiddler) gibi bir araç kullanın.

-   SAML yanıtını uygulama satıcısına gönderin ve eksik olan ne olduğunu sorun.

## <a name="attributes-are-missing-from-the-saml-response"></a>SAML yanıtında öznitelikler eksik

Azure AD yanıtında gönderilecek Azure AD yapılandırmasına bir öznitelik eklemek için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve genel yönetici veya yardımcı yönetici olarak oturum açın.

2. Sol taraftaki gezinti bölmesinin üst kısmında, Azure AD uzantısını açmak için **Tüm hizmetler'i** seçin.

3. Filtre arama kutusuna **Azure Etkin Dizini** yazın ve ardından **Azure Etkin Dizin'i**seçin.

4. Azure AD gezinti bölmesinde **Kurumsal Uygulamalar'ı** seçin.

5. Uygulamalarınızın listesini görüntülemek için **Tüm Uygulamalar'ı** seçin.

   > [!NOTE]
   > İstediğiniz uygulamayı görmüyorsanız, **Tüm Uygulamalar Listesi'nin**en üstündeki **Filtre** denetimini kullanın. **Göster** seçeneğini "Tüm Uygulamalar" olarak ayarlayın.

6. Tek oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra gezinti bölmesinde **Tek oturum açma'yı** seçin.

8. Kullanıcı **Öznitelikleri** bölümünde, **Görünüm'i seçin ve diğer tüm kullanıcı özniteliklerini edin.** Burada, kullanıcılar oturum açarken SAML belirtecinde uygulamaya hangi öznitelikleri göndereceğinizi değiştirebilirsiniz.

   Bir öznitelik eklemek için:

   1. **Öznitelik Ekle'yi**seçin. **Adı**girin ve açılan listeden **Değeri** seçin.

   1.  **Kaydet'i**seçin. Tabloda yeni özniteliği görürsünüz.

9. Yapılandırmayı kaydedin.

   Kullanıcı uygulamaya bir sonraki kaydedinde, Azure AD SAML yanıtında yeni özniteliği gönderir.

## <a name="the-app-doesnt-identify-the-user"></a>Uygulama kullanıcıyı tanımlamaz

SAML yanıtı rol gibi bir özniteliği eksik olduğundan uygulamada oturum açma başarısız olur. Veya uygulama **NameID** (Kullanıcı Tanımlayıcısı) özniteliği için farklı bir biçim veya değer beklediği için başarısız olur.

Uygulamadaki kullanıcıları oluşturmak, korumak ve kaldırmak için [Azure AD otomatikleştirilmiş kullanıcı sağlama](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) yı kullanıyorsanız, kullanıcının SaaS uygulamasına sağlandığını doğrulayın. Daha fazla bilgi için [bkz.](../app-provisioning/application-provisioning-config-problem-no-users-provisioned.md)

## <a name="add-an-attribute-to-the-azure-ad-app-configuration"></a>Azure AD uygulaması yapılandırmasına bir öznitelik ekleme

Kullanıcı Tanımlayıcı değerini değiştirmek için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve genel yönetici veya yardımcı yönetici olarak oturum açın.

2. Azure AD uzantısını açmak için sol taraftaki gezinti bölmesinin üst kısmındaki **Tüm hizmetleri** seçin.

3. Filtre arama kutusuna **Azure Etkin Dizini** yazın ve ardından **Azure Etkin Dizin'i**seçin.

4. Azure AD gezinti bölmesinde **Kurumsal Uygulamalar'ı** seçin.

5. Uygulamalarınızın listesini görüntülemek için **Tüm Uygulamalar'ı** seçin.

   > [!NOTE]
   > İstediğiniz uygulamayı görmüyorsanız, **Tüm Uygulamalar Listesi'nin**en üstündeki **Filtre** denetimini kullanın. **Göster** seçeneğini "Tüm Uygulamalar" olarak ayarlayın.

6. SSO için yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra gezinti bölmesinde **Tek oturum açma'yı** seçin.

8. **Kullanıcı öznitelikleri**altında, **Kullanıcı Tanımlayıcı** açılır listesinden kullanıcı için benzersiz tanımlayıcıyı seçin.

## <a name="change-the-nameid-format"></a>NameID biçimini değiştirme

Uygulama **NameID** (Kullanıcı Tanımlayıcısı) özniteliği için başka bir biçim bekliyorsa, NameID biçimini değiştirmek için [Düzenleme nameID'e](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization#editing-nameid) bakın.

Azure AD, seçilen değere veya SAML AuthRequest'teki uygulama tarafından istenen biçime göre **NameID** özniteliğinin (Kullanıcı Tanımlayıcısı) biçimini seçer. Daha fazla bilgi için, Tek oturum [açma SAML protokolünün](https://docs.microsoft.com/azure/active-directory/develop/single-sign-on-saml-protocol#nameidpolicy)"NameIDPolicy" bölümüne bakın.

## <a name="the-app-expects-a-different-signature-method-for-the-saml-response"></a>Uygulama SAML yanıtı için farklı bir imza yöntemi bekliyor

SAML belirtecinin hangi bölümlerinin Azure AD tarafından dijital olarak imzalanmış olduğunu değiştirmek için aşağıdaki adımları izleyin:

1. Azure [portalını](https://portal.azure.com/) açın ve genel yönetici veya yardımcı yönetici olarak oturum açın.

2. Azure AD uzantısını açmak için sol taraftaki gezinti bölmesinin üst kısmındaki **Tüm hizmetleri** seçin.

3. Filtre arama kutusuna **Azure Etkin Dizini** yazın ve ardından **Azure Etkin Dizin'i**seçin.

4. Azure AD gezinti bölmesinde **Kurumsal Uygulamalar'ı** seçin.

5. Uygulamalarınızın listesini görüntülemek için **Tüm Uygulamalar'ı** seçin.

   > [!NOTE]
   > İstediğiniz uygulamayı görmüyorsanız, **Tüm Uygulamalar Listesi'nin**en üstündeki **Filtre** denetimini kullanın. **Göster** seçeneğini "Tüm Uygulamalar" olarak ayarlayın.

6. Tek oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra gezinti bölmesinde **Tek oturum** açma'yı seçin.

8. **SAML İmzaSertifikası**altında gelişmiş **sertifika imzalama ayarlarını göster'i**seçin.

9. Uygulamanın bu seçenekler arasından beklediği **İmzalama Seçeneğini** seçin:

   * **SamL yanıtını imzala**
   * **SamL yanıtını ve iddiasını imzala**
   * **Sign SAML iddiası**

   Kullanıcı uygulamada bir sonraki oturum açtırsa, Azure AD seçtiğiniz SAML yanıtının bir bölümünü imzalar.

## <a name="the-app-expects-the-sha-1-signing-algorithm"></a>Uygulama SHA-1 imzalama algoritması bekliyor

Varsayılan olarak, Azure AD en güvenli algoritmayı kullanarak SAML belirteci imzalar. Uygulama SHA-1 gerektirmedikçe imzalama algoritmasını *SHA-1* olarak değiştirmemenizi öneririz.

İmzaalgoritmasını değiştirmek için aşağıdaki adımları izleyin:

1. Azure [portalını](https://portal.azure.com/) açın ve genel yönetici veya yardımcı yönetici olarak oturum açın.

2. Azure AD uzantısını açmak için sol taraftaki gezinti bölmesinin üst kısmındaki **Tüm hizmetleri** seçin.

3. Filtre arama kutusuna **Azure Etkin Dizini** yazın ve ardından **Azure Etkin Dizin'i**seçin.

4. Azure AD gezinti bölmesinde **Kurumsal Uygulamalar'ı** seçin.

5. **Uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı seçin.

   > [!NOTE]
   > İstediğiniz uygulamayı görmüyorsanız, **Tüm Uygulamalar Listesi'nin**en üstündeki **Filtre** denetimini kullanın. **Göster** seçeneğini "Tüm Uygulamalar" olarak ayarlayın.

6. Tek oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol tarafındaki gezinti bölmesinden **Tek oturum** açma'yı seçin.

8. **SAML İmzaSertifikası**altında gelişmiş **sertifika imzalama ayarlarını göster'i**seçin.

9. **İmza Algoritması**olarak **SHA-1'i** seçin.

   Kullanıcı uygulamayı bir sonraki oturuma dahil ettiğinde, Azure AD SHA-1 algoritmasını kullanarak SAML belirteci oturum alacaktır.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD'deki uygulamalarda SAML tabanlı tek oturum](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging)açma hataayıklama nasıl.
