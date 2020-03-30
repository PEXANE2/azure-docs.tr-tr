---
title: Grup sona erme ilkesi hızlı başlat - Azure AD | Microsoft Dokümanlar
description: Office 365 grupları için süre sonu - Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: quickstart
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 16b6e4f521568c89e415e6e9fa621175052cf34d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74026892"
---
# <a name="quickstart-set-office-365-groups-to-expire-in-azure-active-directory"></a>Hızlı başlangıç: Azure Active Directory'de Office 365 grupları için süre sonu ayarlama

Bu hızlı başlangıçta Office 365 gruplarınız için süre sonu ilkesini ayarlayacaksınız. Kullanıcılar kendi gruplarını oluşturma iznine sahip olduğunda, kullanılmayan grupların sayısı artabilir. Kullanılmayan grupları yönetmenin bir yolu, bu grupların için sona erme ilkesi ayarlayarak grupları el ile silme zahmetini ortadan kaldırmaktır.

Süre sonu ilkesi oldukça basittir:

- Kullanıcı etkinliklerinin olduğu gruplar, son kullanma tarihi yaklaştıkça otomatik olarak yenilenir
- Grup sahiplerine süresi dolan bir grupla ilgili yenileme bildirimi gönderilir
- Yenilenmeyen gruplar silinir
- Silinen Office 365 grupları, grup sahibi veya bir Azure AD yöneticisi tarafından 30 gün içinde geri yüklenebilir

> [!NOTE]
> Gruplar artık Azure AD zekasını kullanarak son kullanımda olup olmadıklarına bağlı olarak otomatik olarak yenileniyor. Bu yenileme kararı, Outlook, SharePoint, Teams, Yammer ve diğerleri gibi Office 365 hizmetlerindeki gruplardaki kullanıcı etkinliğine dayanır.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisite"></a>Önkoşul

 Grup sona ermesini ayarlamak için gereken en az ayrıcalıklı rol, kuruluştaki Kullanıcı yöneticisidir.

## <a name="turn-on-user-creation-for-groups"></a>Kullanıcılar için grup oluşturma özelliğini açma

1. Kullanıcı yöneticisi hesabıyla [Azure portalında](https://portal.azure.com) oturum açın.

2. **Gruplar**'ı ve ardından **Genel**'i seçin.
  
   ![Self servis grup ayarları sayfası](./media/groups-quickstart-expiration/self-service-settings.png)

3. **Kullanıcılar Office 365 grupları oluşturabilir** ayarını **Evet** olarak belirleyin.

4. İşiniz tamamlandığında grup ayarlarını kaydetmek için **Kaydet**'i seçin.

## <a name="set-group-expiration"></a>Grup süre sonunu ayarlama

1. [Azure portalında](https://portal.azure.com)oturum açın, son kullanma tarihi ayarlarını açmak için **Azure Etkin Dizin** > **Grupları** > **Sona Ermesi'ni** seçin.
  
   ![Grup için son kullanma ayarları sayfası](./media/groups-quickstart-expiration/expiration-settings.png)

2. Süre sonu aralığını ayarlayın. Önceden belirlenmiş değerlerden birini seçin veya 31 günden yüksek bir değer girin. 

3. Sahibi olmayan gruplar için sona erme bildirimlerinin gönderileceği e-posta adresini girin.

4. Bu hızlı başlangıç için **Bu Office 365 grupları için sona ermeyi etkinleştir** ayarını **Tümü** olarak belirleyin.

5. İşiniz tamamlandığında süre sonu ayarlarını kaydetmek için **Kaydet**'i seçin.

İşte bu kadar! Bu hızlı başlangıçta seçilen Office 365 grupları için süre sonu ilkesini başarıyla ayarladınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

### <a name="to-remove-the-expiration-policy"></a>Süre sonu ilkesi kaldırmak için

1. Kiracınızın Genel Yöneticisi olan bir hesapla [Azure portalda](https://portal.azure.com) oturum açtığınızdan emin olun.
2. **Azure Etkin Dizin** > **Grupları** > **Sona Ermesi'ni**seçin.
3. **Bu Office 365 grupları için sona ermeyi etkinleştir** ayarını **Yok** olarak değiştirin.

### <a name="to-turn-off-user-creation-for-groups"></a>Kullanıcılar için grup oluşturma özelliğini kapatmak için

1. **Azure Etkin Dizin** > **Grupları** > **Genel'i**seçin. 
2. **Kullanıcılar, Azure portallarında Office 365 grupları oluşturabilir** ayarını **Hayır** olarak değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

PowerShell talimatları ve teknik kısıtlamalar dahil olmak üzere son kullanma tarihi hakkında daha fazla bilgi için aşağıdaki makaleye bakın:

> [!div class="nextstepaction"]
> [Sona erme ilkesi PowerShell](groups-lifecycle.md)
