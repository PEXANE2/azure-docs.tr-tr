---
title: Grup süre sonu ilkesi hızlı başlangıç-Azure AD | Microsoft Docs
description: Office 365 grupları için süre sonu - Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: quickstart
ms.date: 04/29/2020
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 616910eda33b3ddc49fa6233ccb3989c5e4214e2
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82582854"
---
# <a name="quickstart-set-office-365-groups-to-expire-in-azure-active-directory"></a>Hızlı başlangıç: Azure Active Directory'de Office 365 grupları için süre sonu ayarlama

Bu hızlı başlangıçta Office 365 gruplarınız için süre sonu ilkesini ayarlayacaksınız. Kullanıcılar kendi gruplarını oluşturma iznine sahip olduğunda, kullanılmayan grupların sayısı artabilir. Kullanılmayan grupları yönetmenin bir yolu, bu grupların için sona erme ilkesi ayarlayarak grupları el ile silme zahmetini ortadan kaldırmaktır.

Süre sonu ilkesi oldukça basittir:

- Kullanıcı etkinlikleri olan gruplar, süre sonu yaklaştığında otomatik olarak yenilenir
- Grup sahiplerine süresi dolan bir grupla ilgili yenileme bildirimi gönderilir
- Yenilenmeyen gruplar silinir
- Silinen Office 365 grupları, grup sahibi veya bir Azure AD yöneticisi tarafından 30 gün içinde geri yüklenebilir

> [!NOTE]
> Gruplar artık, son kullanımda olup olmadığına göre otomatik olarak yenilenmekte olan Azure AD zekasını kullanır. Bu yenileme kararı Outlook, SharePoint, takımlar, Yammer ve diğerleri gibi Office 365 Hizmetleri içindeki gruplardaki Kullanıcı etkinliklerini temel alır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisite"></a>Önkoşul

 Grup süre sonunu ayarlamak için gereken en az ayrıcalıklı rol, kuruluştaki Kullanıcı yöneticisidir.

## <a name="turn-on-user-creation-for-groups"></a>Kullanıcılar için grup oluşturma özelliğini açma

1. [Azure Portal](https://portal.azure.com) bir kullanıcı yönetici hesabıyla oturum açın.

2. **Gruplar**'ı ve ardından **Genel**'i seçin.
  
   ![Self servis grup ayarları sayfası](./media/groups-quickstart-expiration/self-service-settings.png)

3. **Kullanıcılar Office 365 grupları oluşturabilir** ayarını **Evet** olarak belirleyin.

4. İşiniz tamamlandığında grup ayarlarını kaydetmek için **Kaydet**'i seçin.

## <a name="set-group-expiration"></a>Grup süre sonunu ayarlama

1. [Azure Portal](https://portal.azure.com)oturum açın, süre sonu ayarlarını açmak için **Azure Active Directory** > **grupları** > **süre sonu** ' nu seçin.
  
   ![Grup için süre sonu ayarları sayfası](./media/groups-quickstart-expiration/expiration-settings.png)

2. Süre sonu aralığını ayarlayın. Önceden belirlenmiş değerlerden birini seçin veya 31 günden yüksek bir değer girin. 

3. Sahibi olmayan gruplar için sona erme bildirimlerinin gönderileceği e-posta adresini girin.

4. Bu hızlı başlangıç için **Bu Office 365 grupları için sona ermeyi etkinleştir** ayarını **Tümü** olarak belirleyin.

5. İşiniz tamamlandığında süre sonu ayarlarını kaydetmek için **Kaydet**'i seçin.

İşte bu kadar! Bu hızlı başlangıçta seçilen Office 365 grupları için süre sonu ilkesini başarıyla ayarladınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

### <a name="to-remove-the-expiration-policy"></a>Süre sonu ilkesi kaldırmak için

1. Azure AD kuruluşunuzun genel yöneticisi olan bir hesapla [Azure Portal](https://portal.azure.com) oturum açtığınızdan emin olun.
2. **Azure Active Directory** > **Groups**grupları > **süre sonu**' nu seçin.
3. **Bu Office 365 grupları için sona ermeyi etkinleştir** ayarını **Yok** olarak değiştirin.

### <a name="to-turn-off-user-creation-for-groups"></a>Kullanıcılar için grup oluşturma özelliğini kapatmak için

1. **Azure Active Directory** > **Groups**grupları > **genel**' i seçin. 
2. **Kullanıcılar, Azure portallarında Office 365 grupları oluşturabilir** ayarını **Hayır** olarak değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

PowerShell yönergelerini ve teknik kısıtlamaları içeren süre sonu hakkında daha fazla bilgi için aşağıdaki makaleye bakın:

> [!div class="nextstepaction"]
> [Sona erme ilkesi PowerShell](groups-lifecycle.md)
