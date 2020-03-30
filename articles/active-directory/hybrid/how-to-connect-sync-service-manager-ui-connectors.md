---
title: Azure AD Senkronizasyon Hizmet Yöneticisi Kullanıcı Arabirimi'ndeki bağlayıcılar | Microsoft Dokümanlar'
description: Azure AD Connect için Eşitleme Hizmet Yöneticisi'ndeki Bağlayıcılar sekmesini anlayın.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 60f1d979-8e6d-4460-aaab-747fffedfc1e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: ae932191c7b76590ea217386dfd729add5566f87
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261053"
---
# <a name="using-connectors-with-the-azure-ad-connect-sync-service-manager"></a>Azure AD Connect Sync Service Manager ile bağlayıcıları kullanma

![Eşitleme Servis Yöneticisi](./media/how-to-connect-sync-service-manager-ui-connectors/connectors.png)

Bağlayıcılar sekmesi, eşitleme motorunun bağlı olduğu tüm sistemleri yönetmek için kullanılır.

## <a name="connector-actions"></a>Bağlayıcı eylemleri
| Eylem | Açıklama |
| --- | --- |
| Oluşturma |Kullanmayın. Ek AD ormanlarına bağlanmak için yükleme sihirbazını kullanın. |
| Özellikler |Etki alanı ve OU filtreleme için kullanılır. |
| [Sil](#delete) |Bağlayıcı alanındaki verileri silmek veya bir ormana bağlantıyı silmek için kullanılır. |
| [Run Profillerini Yapılandırma](#configure-run-profiles) |Etki alanı filtreleme dışında, burada yapılandırmak için hiçbir şey. Önceden yapılandırılmış çalışan profilleri görmek için bu eylemi kullanabilirsiniz. |
| Çalıştırın |Bir profilin tek seferlik bir çalışmasını başlatmak için kullanılır. |
| Durdur |Şu anda profil çalıştıran bir Bağlayıcıyı durdurur. |
| İhracat Konektörü |Kullanmayın. |
| Bağlayıcıyı İthalat |Kullanmayın. |
| Bağlayıcıyı Güncelleştir |Kullanmayın. |
| Şema'yı Yenile |Önbelleğe alınmış şeayı yeniler. Eşitleme kurallarını da güncelleştirir, çünkü bu seçeneği yükleme sihirbazında kullanmak tercih edilir. |
| [Bağlantı Alanı Ara](#search-connector-space) |Nesneleri bulmak ve sistem üzerinden bir nesneyi ve verilerini takip etmek için kullanılır. |

### <a name="delete"></a>Sil
Silme eylemi iki farklı şey için kullanılır.  
![Eşitleme Servis Yöneticisi](./media/how-to-connect-sync-service-manager-ui-connectors/connectordelete.png)

Sil **bağlayıcısı alanı yalnızca** tüm verileri kaldırır, ancak yapılandırmayı korur.

**Bağlayıcıyı ve bağlayıcı alanını sil** seçeneği verileri ve yapılandırmayı kaldırır. Bu seçenek, artık bir ormana bağlanmak istemiyorsanız kullanılır.

Her iki seçenek de tüm nesneleri eşitler ve metaverse nesneleri güncelleştirin. Bu eylem uzun süren bir işlemdir.

### <a name="configure-run-profiles"></a>Run Profillerini Yapılandırma
Bu seçenek, bağlayıcı için yapılandırılan çalışan profilleri görmenizi sağlar.

![Eşitleme Servis Yöneticisi](./media/how-to-connect-sync-service-manager-ui-connectors/configurerunprofiles.png)

### <a name="search-connector-space"></a>Bağlantı Alanı Ara
Arama bağlayıcısı alanı eylemi nesneleri bulmak ve veri sorunlarını gidermek için yararlıdır.

![Eşitleme Servis Yöneticisi](./media/how-to-connect-sync-service-manager-ui-connectors/cssearch.png)

**Kapsam**seçerek başlayın. Verilere (RDN, DN, Anchor, Alt Ağaç) veya nesnenin durumuna (diğer tüm seçenekler) göre arama yapabilirsiniz.  
![Eşitleme Servis Yöneticisi](./media/how-to-connect-sync-service-manager-ui-connectors/cssearchscope.png)  
Örneğin bir Alt Ağaç araması yaparsanız, tüm nesneleri tek bir OU'da alırsınız.  
![Eşitleme Servis Yöneticisi](./media/how-to-connect-sync-service-manager-ui-connectors/cssearchsubtree.png)  
Bu ızgaradan bir nesne seçebilir, **özellikleri**seçebilir ve kaynak bağlayıcı uzaydan, metaverse üzerinden ve hedef bağlayıcı uzaya [kadar takip](tshoot-connect-object-not-syncing.md) edebilirsiniz.

### <a name="changing-the-ad-ds-account-password"></a>AD DS hesap parolasını değiştirme
Hesap parolasını değiştirirseniz, Eşitleme Hizmeti artık şirket içi AD'ye değişiklikleri içe aktaramaz/dışa aktaramaz.   Şu durumlarla karşılaşabilirsiniz:

- AD bağlayıcısı için alma/dışa aktarma adımı "başlangıçsız kimlik bilgileri" hatasıyla başarısız olur.
- Windows Olay Görüntüleyicisi altında, uygulama olay günlüğü Olay Kimliği 6000 ile ilgili bir hata içerir ve "Yönetim aracısı "contoso.com" kimlik bilgileri geçersiz olduğu için çalıştırılamamış." iletisi.

Sorunu gidermek için, AŞAĞıDAKIleri kullanarak AD DS kullanıcı hesabını güncelleştirin:


1. Eşitleme Hizmet Yöneticisi'ni başlatın (START → Eşitleme Hizmeti).
</br>![Eşitleme Servis Yöneticisi](./media/how-to-connect-sync-service-manager-ui-connectors/startmenu.png)
2. **Bağlayıcılar** sekmesine gidin.
3. AD DS hesabını kullanmak üzere yapılandırılan AD Bağlayıcısı'nı seçin.
4. Eylemler altında **Özellikler'i**seçin.
5. Açılan iletişim kutusunda, Etkin Dizin Ormanı'na Bağlan'ı seçin:
6. Orman adı, şirket içinde karşılık gelen AD'yi gösterir.
7. Kullanıcı adı, eşitleme için kullanılan AD DS hesabını gösterir.
8. Parola textbox ![Azure AD Connect Sync Şifreleme Anahtarı Yardımcı Programı'na AD DS hesabının yeni parolasını girin](./media/how-to-connect-sync-service-manager-ui-connectors/key6.png)
9. Yeni parolayı kaydetmek için Tamam'ı tıklatın ve eski parolayı bellek önbelleğinden kaldırmak için Eşitleme Hizmetini yeniden başlatın.



## <a name="next-steps"></a>Sonraki adımlar
Azure AD [Connect eşitleme](how-to-connect-sync-whatis.md) yapılandırması hakkında daha fazla bilgi edinin.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
