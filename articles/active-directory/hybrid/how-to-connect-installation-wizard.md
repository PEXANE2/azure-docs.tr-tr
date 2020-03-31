---
title: Azure AD Connect yükleme sihirbazını yeniden çalıştırma | Microsoft Dokümanlar
description: Yükleme sihirbazının ikinci kez çalıştırdığınızda nasıl çalıştığını açıklar.
keywords: Azure AD Connect yükleme sihirbazı, bakım ayarlarını ikinci kez çalıştırdığınızda yapılandırmanıza olanak tanır
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: d800214e-e591-4297-b9b5-d0b1581cc36a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/17/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5440c54b01f62b3ad61b355f4c622a31910a65c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261339"
---
# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a>Azure AD Connect eşitlemi: Yükleme sihirbazını ikinci kez çalıştırma
Azure AD Connect yükleme sihirbazını ilk kez çalıştırdığınızda, yüklemenizi nasıl yapılandırabileceğinizkonusunda size yol sunar. Yükleme sihirbazını yeniden çalıştıran bu sihirbaz, bakım seçenekleri sunar.

>[!IMPORTANT]
>Eşitleme devam ederken yükleme sihirbazını çalıştıramayacağınızı unutmayın.  Sihirbazı başlatmadan önce bir eşitlemenin çalışmadığını doğrulayın.

Yükleme sihirbazını **Azure AD Connect**adlı başlat menüsünde bulabilirsiniz.

![Başlat menüsü](./media/how-to-connect-installation-wizard/startmenu.png)

Yükleme sihirbazını başlattığınızda, şu seçeneklere sahip bir sayfa görürsünüz:

![Ek görevlerin listesini içeren sayfa](./media/how-to-connect-installation-wizard/additionaltasks.png)

ADFS'yi Azure AD Connect ile yüklediyseniz, daha da fazla seçeneğiniz vardır. ADFS için sahip olduğunuz ek seçenekler [ADFS yönetiminde](how-to-connect-fed-management.md#manage-ad-fs)belgelenmiştir.

Görevlerden birini seçin ve devam etmek için **İleri'yi** tıklatın.

> [!IMPORTANT]
> Yükleme sihirbazı açıkken, eşitleme motorundaki tüm işlemler askıya alınır. Yapılandırma değişikliklerinizi tamamlar tamamlamaz yükleme sihirbazını kapattığınızdan emin olun.
>
>

## <a name="view-current-configuration"></a>Geçerli yapılandırmayı görüntüleme
Bu seçenek, şu anda yapılandırılmış seçeneklerinizin hızlı bir görünümünü sağlar.

![Tüm seçeneklerin ve durumlarının listesini içeren sayfa](./media/how-to-connect-installation-wizard/viewconfig.png)

Geri dönmek için **Önceki'yi** tıklatın. **Exit'i**seçerseniz, yükleme sihirbazını kapatırsınız.

## <a name="customize-synchronization-options"></a>Eşitleme seçeneklerini özelleştirme
Bu seçenek eşitleme yapılandırması değişiklikleri yapmak için kullanılır. Özel yapılandırma yükleme yolundan bir seçenek alt kümesi görürsünüz. Başlangıçta ekspres yüklemeyi kullanmış olsanız bile bu seçeneği görürsünüz.

* [Daha fazla dizin ekleyin.](how-to-connect-install-custom.md#connect-your-directories) Bir dizini kaldırmak için [bkz.](how-to-connect-sync-service-manager-ui-connectors.md#delete)
* [Etki Alanı ve OU filtreleme değiştirin.](how-to-connect-install-custom.md#domain-and-ou-filtering)
* Grup filtrelemi kaldır.
* [İsteğe bağlı özellikleri değiştirin.](how-to-connect-install-custom.md#optional-features)

İlk yüklemedeki diğer seçenekler değiştirilemez ve kullanılamıyor. Bu seçenekler şunlardır:

* UserPrincipalName ve sourceAnchor için kullanılacak özniteliği değiştirin.
* Farklı ormandaki nesneler için birleştirme yöntemini değiştirin.
* Grup tabanlı filtrele'yi etkinleştirin.

## <a name="refresh-directory-schema"></a>Dizin şealarını yenile
Bu seçenek, şirket içi AD DS ormanlarınızdan birinde şema değiştirdiyseniz kullanılır. Örneğin, Exchange'i yüklemiş veya aygıt nesneleriyle windows server 2012 şemasına yükseltilmiş olabilirsiniz. Bu durumda, Azure AD Connect'e şemayı AD DS'den yeniden okuması ve önbelleğini güncelleştirmesi için talimat vermeniz gerekir. Bu eylem eşitleme kurallarını da yeniden oluşturur. Exchange şemasını örnek olarak eklerseniz, Exchange için Eşitleme Kuralları yapılandırmaya eklenir.

Bu seçeneği seçtiğinizde, yapılandırmanızdaki tüm dizinler listelenir. Varsayılan ayarı tutabilir ve tüm ormanları yenileyebilir veya bazılarını seçebilirsiniz.

![Çevredeki tüm dizinlerin listesini içeren sayfa](./media/how-to-connect-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>Hazırlama modunu yapılandırma
Bu seçenek, sunucuda hazırlama modunu etkinleştirmenizi ve devre dışı etmenizi sağlar. Hazırlama modu ve nasıl kullanıldığı hakkında daha fazla bilgiyi [Operations'da](how-to-connect-sync-staging-server.md)bulabilirsiniz.

Bu seçenek, evrelemenin şu anda etkin veya devre dışı bırakılmış olup olmadığını gösterir:  
![Evreleme modunun geçerli durumunu da gösteren seçenek](./media/how-to-connect-installation-wizard/stagingmodecurrentstate.png)

Durumu değiştirmek için bu seçeneği seçin ve onay kutusunu seçin veya seçin.  
![Evreleme modunun geçerli durumunu da gösteren seçenek](./media/how-to-connect-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>Kullanıcı oturum açma'yı değiştirme
Bu seçenek, kullanıcı oturum açma yöntemini parola karma eşitleme, geçiş kimlik doğrulaması veya federasyondan değiştirmenize olanak tanır. **Yapılandırmamak**için değiştiremezsiniz.

Bu seçenek hakkında daha fazla bilgi için [kullanıcı oturum açma'ya](plan-connect-user-signin.md#changing-the-user-sign-in-method)bakın.

## <a name="next-steps"></a>Sonraki adımlar
* [Açıklayıcı Sağlama'yı Anlama'da](concept-azure-ad-connect-sync-declarative-provisioning.md)Azure AD Connect eşitlemetarafından kullanılan yapılandırma modeli hakkında daha fazla bilgi edinin.

**Genel bakış konuları**

* [Azure AD Connect eşitlemesi: Eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
