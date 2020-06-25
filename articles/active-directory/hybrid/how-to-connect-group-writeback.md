---
title: 'Azure AD Connect: Grup geri yazma'
description: Bu makalede Azure AD Connect 'de grup geri yazma açıklanmaktadır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.date: 06/11/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: d2a41dcf9c224e9e4a9a280078432e0b57e16c2a
ms.sourcegitcommit: f98ab5af0fa17a9bba575286c588af36ff075615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85359424"
---
# <a name="azure-ad-connect-group-writeback"></a>Azure AD Connect grubu geri yazma

Grupların geri yazma özelliği, müşterilerin karma ihtiyaçları için bulut gruplarından yararlanmasını sağlar. Office 365 Grupları özelliğini kullanıyorsanız bu gruplar şirket içi Active Directory'nizde de temsil edilir. Bu seçenek **yalnızca** şirket Içi Active Directory Exchange varsa kullanılabilir.

## <a name="pre-requisites"></a>Ön koşullar
Grup geri yazma özelliğini etkinleştirmek için aşağıdaki önkoşulların karşılanması gerekir.
- Kiracınız için Azure Active Directory Premium lisanslar.
- Exchange şirket içi kuruluşunuz ve Office 365 arasında yapılandırılmış bir karma dağıtım ve düzgün çalıştığını doğrulama.
- Exchange 'in şirket içinde desteklenen bir sürümü yüklü
- Azure Active Directory Connect kullanarak çoklu oturum açma yapılandırıldı 

## <a name="enable-group-writeback"></a>Grup geri yazmayı etkinleştirme
Grup geri yazmayı etkinleştirmek için aşağıdaki adımları kullanın:

1. Azure AD Connect sihirbazını açın, **Yapılandır** ' ı seçin ve ardından **İleri**' ye tıklayın.
2. **Eşitleme seçeneklerini Özelleştir** ' i seçin ve ardından **İleri**' ye tıklayın.
3. **Azure AD 'ye Bağlan** sayfasında, kimlik bilgilerinizi girin. **İleri**’ye tıklayın.
4. **Isteğe bağlı özellikler** sayfasında, daha önce yapılandırdığınız seçeneklerin hala seçili olduğunu doğrulayın.
5. **Grup geri yazma** 'yı seçin ve ardından **İleri**' ye tıklayın.
6. **Geri yazma sayfasında**, Office 365 ' den eşitlenen nesneleri şirket içi kuruluşunuza depolamak için bir Active Directory kuruluş BIRIMI (OU) seçin ve ardından **İleri**' ye tıklayın.
7. Yapılandırmaya **hazırlanma** sayfasında, **Yapılandır**' a tıklayın.
8. Sihirbaz tamamlandığında, yapılandırma Tamam sayfasında **Çıkış** ' a tıklayın.
9. Azure Active Directory Connect sunucuda Windows PowerShell 'i açın ve aşağıdaki komutları çalıştırın.

```Powershell
$AzureADConnectSWritebackAccountDN =  <MSOL_ account DN>
Import-Module "C:\Program Files\Microsoft Azure Active Directory Connect\AdSyncConfig\AdSyncConfig.psm1"
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN $AzureADConnectSWritebackAccountDN
```

Office 365 gruplarını yapılandırma hakkında daha fazla bilgi için bkz. Şirket [Içi Exchange karma ile Microsoft 365 gruplarını yapılandırma](https://docs.microsoft.com/exchange/hybrid-deployment/set-up-office-365-groups#enable-group-writeback-in-azure-ad-connect).

## <a name="disabling-group-writeback"></a>Grup geri yazma devre dışı bırakılıyor
Grup geri yazmayı devre dışı bırakmak için aşağıdaki adımları kullanın: 


1. Azure Active Directory Connect Sihirbazı 'nı başlatın ve ek görevler sayfasına gidin. **Eşitleme seçeneklerini Özelleştir** görevini seçin ve **İleri**' ye tıklayın.
2. **Isteğe bağlı özellikler** sayfasında, grup geri yazma işaretini kaldırın.  Grupların silineceğini bildiren bir uyarı alırsınız.  **Evet**' e tıklayın.
   >[!IMPORTANT]
   > Grup geri yazma özelliğinin devre dışı bırakılması, bu özellik tarafından daha önce oluşturulan grupların bir sonraki eşitleme döngüsündeki yerel Active Directory silinmesine neden olur. 

   ![Onay kutusunun işaretini kaldır](media/how-to-connect-group-writeback/group2.png)
  
3. **İleri**’ye tıklayın.
4. **Yapılandır**'a tıklayın.

 >[!NOTE]
 > Grup geri yazma özelliğinin devre dışı bırakılması, Azure Active Directory bağlayıcısında tam Içeri aktarma ve tam eşitleme bayraklarını ' true ' olarak ayarlar, böylece daha önce Active Directory geri yazılmış olan grupları silerek kural değişikliklerinin bir sonraki eşitleme döngüsüne yayılmasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
