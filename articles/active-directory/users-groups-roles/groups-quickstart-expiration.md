---
title: Office 365 grupları için sona erme ilkesi hızlı başlangıcı-Azure Active Directory | Microsoft Docs
description: Office 365 grupları için süre sonu - Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: quickstart
ms.date: 10/24/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: aabfe803a6ee40491dd95fe7def9a31838c80e65
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72928986"
---
# <a name="quickstart-set-office-365-groups-to-expire-in-azure-active-directory"></a>Hızlı başlangıç: Azure Active Directory'de Office 365 grupları için süre sonu ayarlama

Bu hızlı başlangıçta Office 365 gruplarınız için süre sonu ilkesini ayarlayacaksınız. Kullanıcılar kendi gruplarını oluşturma iznine sahip olduğunda, kullanılmayan grupların sayısı artabilir. Kullanılmayan grupları yönetmenin bir yolu, bu grupların için sona erme ilkesi ayarlayarak grupları el ile silme zahmetini ortadan kaldırmaktır.

Süre sonu ilkesi oldukça basittir:

- Kullanıcı etkinlikleri olan gruplar, süre sonu yaklaştığında otomatik olarak yenilenir (Önizleme)
- Grup sahiplerine süresi dolan bir grupla ilgili yenileme bildirimi gönderilir
- Yenilenmeyen gruplar silinir
- Silinen Office 365 grupları, grup sahibi veya bir Azure AD yöneticisi tarafından 30 gün içinde geri yüklenebilir

> [!NOTE]
> Gruplar artık Azure AD zekasını, en son kullanımda olup olmadıklarından (Şu anda genel önizlemede, bu nedenle kaydolma gerekli değildir) göre otomatik olarak yenilenir. Bu özellik, Outlook, SharePoint, takımlar, Yammer ve diğerleri gibi Office 365 Hizmetleri içindeki gruplardaki Kullanıcı etkinliklerini temel alır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisite"></a>Önkoşul

 Grup süre sonunu ayarlamak için gereken en az ayrıcalıklı rol, kuruluştaki Kullanıcı yöneticisidir.

## <a name="turn-on-user-creation-for-groups"></a>Kullanıcılar için grup oluşturma özelliğini açma

1. [Azure Portal](https://portal.azure.com) bir kullanıcı yönetici hesabıyla oturum açın.

2. **Gruplar**'ı ve ardından **Genel**'i seçin.
  
   ![Self servis grup ayarları sayfası](./media/groups-quickstart-expiration/self-service-settings.png)

3. **Kullanıcılar Office 365 grupları oluşturabilir** ayarını **Evet** olarak belirleyin.

4. İşiniz tamamlandığında grup ayarlarını kaydetmek için **Kaydet**'i seçin.

## <a name="set-group-expiration"></a>Grup süre sonunu ayarlama

1. [Azure Portal](https://portal.azure.com)oturum açtıktan sonra, süre sonu ayarlarını açmak için **Azure Active Directory** > **gruplar** > **süre sonu** ' nu seçin.
  
   ![Grup için süre sonu ayarları sayfası](./media/groups-quickstart-expiration/expiration-settings.png)

2. Süre sonu aralığını ayarlayın. Önceden belirlenmiş değerlerden birini seçin veya 31 günden yüksek bir değer girin. 

3. Sahibi olmayan gruplar için sona erme bildirimlerinin gönderileceği e-posta adresini girin.

4. Bu hızlı başlangıç için **Bu Office 365 grupları için sona ermeyi etkinleştir** ayarını **Tümü** olarak belirleyin.

5. İşiniz tamamlandığında süre sonu ayarlarını kaydetmek için **Kaydet**'i seçin.

İşte bu kadar! Bu hızlı başlangıçta seçilen Office 365 grupları için süre sonu ilkesini başarıyla ayarladınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

### <a name="to-remove-the-expiration-policy"></a>Süre sonu ilkesini kaldırmak için

1. Kiracınızın Genel Yöneticisi olan bir hesapla [Azure portalda](https://portal.azure.com) oturum açtığınızdan emin olun.
2. **Azure Active Directory** > **Gruplar** > **Süre Sonu**'nu seçin.
3. **Bu Office 365 grupları için sona ermeyi etkinleştir** ayarını **Yok** olarak değiştirin.

### <a name="to-turn-off-user-creation-for-groups"></a>Gruplar için Kullanıcı oluşturmayı devre dışı bırakmak için

1. **Azure Active Directory** > **Gruplar** > **Genel**'i seçin. 
2. **Kullanıcılar, Azure portallarında Office 365 grupları oluşturabilir** ayarını **Hayır** olarak değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

PowerShell yönergelerini ve teknik kısıtlamaları içeren süre sonu hakkında daha fazla bilgi için aşağıdaki makaleye bakın:

> [!div class="nextstepaction"]
> [Sona erme ilkesi PowerShell](groups-lifecycle.md)
