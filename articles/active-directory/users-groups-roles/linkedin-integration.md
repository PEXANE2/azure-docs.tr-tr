---
title: LinkedIn hesap bağlantıları için yönetici onayı - Azure AD | Microsoft Dokümanlar
description: Azure Active Directory'deki Microsoft uygulamalarında LinkedIn tümleştirme hesabı bağlantılarını nasıl etkinleştirip devre dışı kaçıklanın
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 54e3821d269d11397ec4f9f5833e33ac6b555abc
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80755107"
---
# <a name="integrate-linkedin-account-connections-in-azure-active-directory"></a>LinkedIn hesap bağlantılarını Azure Etkin Dizini'ne entegre et

Kuruluşunuzdaki kullanıcıların bazı Microsoft uygulamalarında LinkedIn bağlantılarına erişmesine izin verebilirsiniz. Kullanıcılar hesaplarını bağlamayı kabul ettirene kadar hiçbir veri paylaşılmaz. Kuruluşunuzu Azure Etkin Dizin (Azure AD) [yönetici merkezine](https://aad.portal.azure.com)entegre edebilirsiniz.

> [!IMPORTANT]
> LinkedIn hesap bağlantıları ayarı şu anda Azure REKLAM kuruluşlarına dağıtılıyor. Kuruluşunuz için kullanıma sunulduğunda, varsayılan olarak etkinleştirilir.
>
> Özel durumlar:
>
> * Ayar, ABD Hükümeti, Microsoft Cloud Almanya veya Çin'de 21Vianet tarafından işletilen Azure ve Office 365 için Microsoft Cloud kullanan müşteriler için kullanılamaz.
> * Ayar, Almanya'da sağlanan kiracılar için varsayılan olarak kapalıdır. Ayarın Microsoft Cloud Germany kullanan müşteriler için kullanılmadığını unutmayın.
> * Ayar, Fransa'da sağlanan kiracılar için varsayılan olarak kapalıdır.
>
> LinkedIn hesap bağlantıları kuruluşunuz için etkinleştirildikten sonra, kullanıcılar şirket verilerine kendi adlarına erişen uygulamalara onay verdikten sonra hesap bağlantıları çalışır. Kullanıcı onayı ayarı hakkında bilgi için, [kullanıcının bir uygulamaya erişimini nasıl kaldırınız.](https://docs.microsoft.com/azure/active-directory/application-access-assignment-how-to-remove-assignment)

## <a name="enable-linkedin-account-connections-in-the-azure-portal"></a>Azure portalında LinkedIn hesap bağlantılarını etkinleştirme

LinkedIn hesap bağlantılarını yalnızca erişmek istediğiniz kullanıcılar için, tüm kuruluşunuzdan yalnızca kuruluşunuzdaki seçili kullanıcılara etkinleştirebilirsiniz.

1. Azure AD kuruluşu için genel bir yönetici olan bir hesapla [Azure AD yönetici merkezinde](https://aad.portal.azure.com/) oturum açın.
1. **Kullanıcılar**’ı seçin.
1. **Kullanıcılar** **sayfasında, Kullanıcı ayarlarını**seçin.
1. **LinkedIn hesap bağlantıları**altında, kullanıcıların bazı Microsoft uygulamalarında LinkedIn bağlantılarına erişmek için hesaplarını bağlamalarına izin verin. Kullanıcılar hesaplarını bağlamayı kabul ettirene kadar hiçbir veri paylaşılmaz.

    * Kuruluşunuzdaki tüm kullanıcılar için hizmeti etkinleştirmek için **Evet'i** seçin
    * Kuruluşunuzdaki yalnızca seçili bir kullanıcı grubu için hizmeti etkinleştirmek için **Seçili grubu** seçin
    * Kuruluşunuzdaki tüm kullanıcıların onayını geri çekmek için **Hayır'ı** seçin

    ![LinkedIn hesap bağlantılarını kuruluşa entegre edin](./media/linkedin-integration/linkedin-integration.png)

1. Işiniz bittiğinde, ayarlarınızı kaydetmek için **Kaydet'i** seçin.

> [!Important]
> LinkedIn tümleştirmesi, kullanıcılarınızın hesaplarını bağlamayı kabul etmelerine kadar tam olarak etkinleştirilemez. Kullanıcılarınız için hesap bağlantılarını etkinleştirdiğinizde hiçbir veri paylaşılmaz.

### <a name="assign-selected-users-with-a-group"></a>Seçili kullanıcıları bir grupla atama

LinkedIn ve Microsoft hesaplarını birçok bireysel kullanıcı yerine tek bir grup için bağlama olanağı sağlayabilmeniz için bir kullanıcı grubu seçme seçeneğine sahip bir kullanıcı listesini belirten 'Seçili' seçeneğini değiştirdik. Seçili kullanıcılar için LinkedIn hesap bağlantıları etkinleştirmiyorsanız, hiçbir şey yapmanız gerekmez. Seçili bireysel kullanıcılar için LinkedIn hesap bağlantılarını daha önce etkinleştirdiyseniz, şunları

1. Tek tek kullanıcıların geçerli listesini alma
1. Şu anda etkinleştirilen tek tek kullanıcıları bir gruba taşıma
1. Azure AD yönetici merkezindeki LinkedIn hesap bağlantıları ayarında seçilen grup olarak önceki grubu kullanın.

> [!NOTE]
> Şu anda seçili tek tek kullanıcılarınızı bir gruba taşımasanız bile, Microsoft uygulamalarında LinkedIn bilgilerini görmeye devam edebilirler.

### <a name="move-currently-selected-users-to-a-group"></a>Şu anda seçili kullanıcıları bir gruba taşıma

1. LinkedIn hesap bağlantıları için seçilen kullanıcıların csv dosyasını oluşturun.
1. Yönetici hesabınızla Microsoft 365'te oturum açın.
1. PowerShell’i başlatın.
1. Çalıştırarak Azure AD modüllerini yükleme`Install-Module AzureAD`
1. Şu betiği çalıştırın:

  ``` PowerShell
  $groupId = "GUID of the target group"
  
  $users = Get-Content 
  Path to the CSV file
  
  $i = 1
  foreach($user in $users} { Add-AzureADGroupMember -ObjectId $groupId -RefObjectId $user ; Write-Host $i Added $user ; $i++ ; Start-Sleep -Milliseconds 10 }
  ```

Grubu, Azure AD yönetici merkezindeki LinkedIn hesap bağlantıları ayarında seçili grup olarak ikinci adımdan kullanmak için Azure [portalındaki LinkedIn hesap bağlantılarını etkinleştir'e](#enable-linkedin-account-connections-in-the-azure-portal)bakın.

## <a name="use-group-policy-to-enable-linkedin-account-connections"></a>LinkedIn hesap bağlantılarını etkinleştirmek için Grup İlkesi'ni kullanın

1. Office [2016 Yönetim Şablonu dosyalarını indirin (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
1. **ADMX** dosyalarını ayıklayın ve merkezi mağazanıza kopyalayın.
1. Açık Grup İlkesi Yönetimi.
1. Aşağıdaki ayarda bir Grup **İlkesi**Nesnesi oluşturun: Kullanıcı > **YapılandırmaSı Yönetim Şablonları** > **Microsoft Office 2016** > Office**uygulamalarında LinkedIn'i****Çeşitli** > Göster özellikleri.
1. Etkin veya **Devre Dışı'yı**seçin. **Enabled**
  
   Durum | Etki
   ------ | ------
   **Etkin** | Office 2016 **Seçenekleri'nde Office uygulamaları ayarında LinkedIn'i göster özellikleri** etkinleştirilir. Kuruluşunuzdaki kullanıcılar Office 2016 uygulamalarında LinkedIn özelliklerini kullanabilir.
   **Devre dışı** | Office 2016 **Seçenekleri'nde Office uygulamaları** ayarını gösteren LinkedIn özellikleri devre dışı bırakılır ve son kullanıcılar bu ayarı değiştiremez. Kuruluşunuzdaki kullanıcılar Office 2016 uygulamalarında LinkedIn özelliklerini kullanamaz.

Bu grup ilkesi yalnızca yerel bir bilgisayar için Office 2016 uygulamalarını etkiler. Kullanıcılar Office 2016 uygulamalarında LinkedIn'i devre dışı kılabilirse, Office 365'teki LinkedIn özelliklerini görmeye devam edebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [LinkedIn için kullanıcı onayı ve veri paylaşımı](linkedin-user-consent.md)

* [Microsoft uygulamalarınızdaki LinkedIn bilgileri ve özellikleri](https://go.microsoft.com/fwlink/?linkid=850740)

* [LinkedIn yardım merkezi](https://www.linkedin.com/help/linkedin)

* [Azure portalında geçerli LinkedIn tümleştirme ayarınızı görüntüleyin](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings)
