---
title: Dinamik grup üyelikleriyle ilgili sorunları giderin - Azure AD | Microsoft Dokümanlar
description: Azure Active Directory'de dinamik grup üyeliği için sorun giderme ipuçları
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6f685ac63e3b4a8cf466be4eb4561472fb084d49
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74026554"
---
# <a name="troubleshoot-and-resolve-groups-issues"></a>Grup sorunlarını giderme ve çözme

## <a name="troubleshooting-group-creation-issues"></a>Sorun giderme grubu oluşturma sorunları

**Azure portalında güvenlik grubu oluşturmayı devre dışı bıraktım, ancak gruplar Powershell aracılığıyla oluşturulabilir** Kullanıcı, Azure portalında ayar lı **Azure portallarında güvenlik grupları oluşturabilir** ve yönetici olmayan kullanıcıların Access panelinde veya Azure portalında güvenlik grupları oluşturup oluşturamayacağını denetler. Powershell üzerinden güvenlik grubu oluşturmayı denetlemez.

Powershell'de yönetici olmayan kullanıcılar için grup oluşturmayı devre dışı kalım:
1. Yönetici olmayan kullanıcıların grup oluşturmasına izin verildiğini doğrulayın:
   

   ```powershell
   Get-MsolCompanyInformation | Format-List UsersPermissionToCreateGroupsEnabled
   ```

  
2. Dönerse, `UsersPermissionToCreateGroupsEnabled : True`yönetici olmayan kullanıcılar gruplar oluşturabilir. Bu özelliği devre dışı kalmak için:
  

   ``` 
   Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False
   ```

<br/>**Powershell'de Dinamik Grup oluşturmaya çalışırken izin verilen maksimum gruplar aldı**<br/>
Powershell'de, izin verilen _gruplar sayısının en yüksek_olduğunu belirten dinamik grup ilkelerini belirten bir ileti alırsanız, bu, kiracınızdaki Dinamik gruplar için maksimum sınıra ulaştığınız anlamına gelir. Kiracı başına en fazla Dinamik grup sayısı 5.000'dir.

Yeni Dinamik gruplar oluşturmak için öncelikle varolan bazı Dinamik grupları silmeniz gerekir. Sınırı arttırmak için bir yol yok.

## <a name="troubleshooting-dynamic-memberships-for-groups"></a>Gruplar için dinamik üyelik sorunlarını giderme

**Bir grup la ilgili bir kural yapılandırıldım ancak grupta hiçbir üyelik güncelleştirilemiş**<br/>
1. Kuraldaki kullanıcı veya aygıt özniteliklerinin değerlerini doğrulayın. Kuralı karşılayan kullanıcılar olduğundan emin olun. Aygıtlar için, eşitlenen özniteliklerin beklenen değerleri içerdiğinden emin olmak için aygıt özelliklerini denetleyin.<br/>
2. Tamam olup olmadığını doğrulamak için üyelik işleme durumunu kontrol edin. Grup için **Genel Bakış** sayfasında [üyelik işleme durumunu](groups-create-rule.md#check-processing-status-for-a-rule) ve son güncellenen tarihi kontrol edebilirsiniz.

Her şey yolunda görünüyorsa, lütfen grubun doldurması için biraz zaman verin. Kiracınızın boyutuna bağlı olarak, ilk seferinde veya kural değişikliğinden sonra grubun doldurulması 24 saat kadar sürebilir.

**Bir kural yapılandırıldım, ancak şimdi kuralın varolan üyeleri kaldırıldı**<br/>Bu beklenen bir davranıştır. Bir kural etkinleştirildiğinde veya değiştirildiğinde grubun varolan üyeleri kaldırılır. Kuralın değerlendirilmesinden döndürülen kullanıcılar gruba üye olarak eklenir.

**Bir kural eklediğimde veya değiştirdiğimde üyelik değişikliklerini anında görmüyorum, neden olmasın?**<br/>Özel üyelik değerlendirmesi periyodik olarak eşzamanlı bir arka plan sürecinde yapılır. İşlemin ne kadar süreceği, dizininizdeki kullanıcı sayısına ve kuralın sonucu olarak oluşturulan grubun boyutuna göre belirlenir. Genellikle, az sayıda kullanıcıya sahip dizinler grup üyeliğinin birkaç dakikadan kısa bir sürede değiştiğini görür. Çok sayıda kullanıcıya sahip dizinlerin doldurulması 30 dakika veya daha uzun sürebilir.

**Grubu şimdi işleme almaya nasıl zorlayabilirim?**<br/>
Şu anda, isteğe bağlı olarak işlenecek grubu otomatik olarak tetiklemenin bir yolu yoktur. Ancak, sonunda bir boşluk eklemek için üyelik kuralını güncelleştirerek yeniden işlemeyi el ile tetikleyebilirsiniz.  

**Bir kural işleme hatasıyla karşılaştım**<br/>Aşağıdaki tabloda ortak dinamik üyelik kuralı hataları ve bunların nasıl düzeltilen listelenilir.

| Kural ayrıştırıcı hatası | Hata kullanımı | Düzeltilmiş kullanım |
| --- | --- | --- |
| Hata: Öznitelik desteklenmez. |(user.invalidProperty -eq "Değer") |(user.department -eq "değer")<br/><br/>Özniteliğin desteklenen özellikler [listesinde](groups-dynamic-membership.md#supported-properties)olduğundan emin olun. |
| Hata: İşleç öznitelik olarak desteklenmez. |(user.accountEnabled -true içerir) |(user.accountEtkin -eq true)<br/><br/>Kullanılan işleç özellik türü için desteklenmez (bu örnekte, -içeren tip boolean kullanılamaz). Özellik türü için doğru işleçleri kullanın. |
| Hata: Sorgu derleme hatası. | 1. (user.department -eq "Satış") (user.department -eq "Marketing")<br>2. (user.userPrincipalName -match@domain.ext"* ") | 1. Eksik operatör. -ve -veya -veya iki birleştirme yüklemlerini kullanma<br>(user.department -eq "Satış") -veya (user.department -eq "Marketing")<br>2. -match ile kullanılan normal ifadehata<br>(user.userPrincipalName -match ".*@domain.ext")<br>veya alternatif olarak: (user.userPrincipalName@domain.ext-match " $") |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](../fundamentals/active-directory-manage-groups.md)
* [Azure Active Directory’de Uygulama Yönetimi](../manage-apps/what-is-application-management.md)
* [Azure Etkin Dizin nedir?](../fundamentals/active-directory-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md)