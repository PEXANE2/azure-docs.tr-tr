---
title: Azure AD Etki Alanı Hizmetleri ile etki alanı birleştirme sorunu | Microsoft Dokümanlar
description: Bir VM'ye katılmaya veya bir uygulamayı Azure Etkin Dizin Etki Alanı Hizmetleri'ne bağlamayı denediğinizde ve yönetilen etki alanına bağlanamadığınızda veya kimlik doğrulaması yaptığınızda sık karşılaşılan sorunları nasıl giderebileceğinizi öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 10/02/2019
ms.author: iainfou
ms.openlocfilehash: f187dba4eace61695a72e4b7b08731e65ff0d7f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78299117"
---
# <a name="troubleshoot-domain-join-problems-with-an-azure-ad-domain-services-managed-domain"></a>Azure AD Etki Alanı Hizmetleri yönetilen etki alanıyla ilgili etki alanı birleştirme sorunlarını giderme

Sanal bir makineye (VM) katılmaya veya bir uygulamayı Azure Active Directory Etki Alanı Hizmetleri (AD DS) yönetilen bir etki alanına bağlamayı denediğinizde, bunu yapamayacağınız bir hata alabilirsiniz. Etki alanı birleştirme sorunlarını gidermek için, aşağıdaki noktalardan hangisinde sorun olduğunu gözden geçirin:

* Kimlik doğrulama istemi almazsanız, VM veya uygulama Azure AD DS yönetilen etki alanına bağlanabilir.
    * Etki alanına [katılmak için bağlantı sorunlarını](#connectivity-issues-for-domain-join)gidermeye başlayın.
* Kimlik doğrulama sırasında bir hata alırsanız, Azure AD DS yönetilen etki alanına bağlantı başarılı olur.
    * Etki alanı [birleştirme sırasında kimlik bilgileriyle ilgili sorunları](#credentials-related-issues-during-domain-join)gidermeye başlayın.

## <a name="connectivity-issues-for-domain-join"></a>Etki alanına katılmak için bağlantı sorunları

VM Azure AD DS yönetilen etki alanını bulamazsa, genellikle bir ağ bağlantısı veya yapılandırma sorunu vardır. Sorunu bulmak ve çözmek için aşağıdaki sorun giderme adımlarını gözden geçirin:

1. VM'nin Azure AD DS için etkin leştirilmiş aynı veya eşlenmiş sanal ağa bağlı olduğundan emin olun. Değilse, VM katılmak için etki alanı bulamaz ve etki alanına bağlanamaz.
    * VM aynı sanal ağa bağlı değilse, trafiğin doğru akmasını sağlamak için sanal ağ eşleme veya VPN bağlantısının *Etkin* veya *Bağlı* olduğunu doğrulayın.
1. Azure AD DS yönetilen etki alanının etki alanı adını `ping aaddscontoso.com`kullanarak etki alanını pinglemeye çalışın.
    * Ping yanıtı başarısız olursa, Azure AD DS yönetilen etki alanınız için portaldaki genel bakış sayfasında görüntülenen `ping 10.0.0.4`etki alanının IP adreslerini pinglemeyi deneyin.
    * Ip adresini başarılı bir şekilde pingleyebilir, ancak etki alanını pingleyebilirseniz, DNS yanlış yapılandırılmış olabilir. Sanal ağ için Azure AD DS yönetilen etki alanı DNS sunucularını yapılandırdığınızdan emin olun.
1. DNS çözümleyici önbelleğini sanal makinede yıkamayı deneyin, örneğin. `ipconfig /flushdns`

### <a name="network-security-group-nsg-configuration"></a>Ağ Güvenlik Grubu (NSG) yapılandırması

Azure AD DS yönetilen bir etki alanı oluşturduğunuzda, başarılı etki alanı çalışması için uygun kurallarla bir ağ güvenlik grubu da oluşturulur. Ek ağ güvenliği grubu kurallarını düzenliyor veya oluşturursanız, Azure AD DS'nin bağlantı ve kimlik doğrulama hizmetleri sağlaması için gereken bağlantı noktalarını istemeden engelleyebilirsiniz. Bu ağ güvenliği grubu kuralları, parola eşitlemenin tamamlanmaması, kullanıcıların oturum açamaması veya etki alanı birleştirme sorunları gibi sorunlara neden olabilir.

Bağlantı sorunlarınız devam ederse, aşağıdaki sorun giderme adımlarını gözden geçirin:

1. Azure portalında Azure AD DS yönetilen etki alanınızın sistem durumu durumunu kontrol edin. *AADDS001*için bir uyarınız varsa, ağ güvenlik grubu kuralı erişimi engelliyor.
1. Gerekli [bağlantı noktalarını ve ağ güvenlik grubu kurallarını][network-ports]gözden geçirin. Bu ağ bağlantı noktalarını engellemekten bağlandığınız VM veya sanal ağa ağ güvenlik grubu kuralları uygulanmadığından emin olun.
1. Herhangi bir ağ güvenlik grubu yapılandırma sorunları çözüldükten sonra, *AADDS001* uyarısı sistem durumu sayfasından yaklaşık 2 saat içinde kaybolur. Ağ bağlantısı artık kullanılabilirken, VM'ye yeniden katılmayı deneyin.

## <a name="credentials-related-issues-during-domain-join"></a>Etki alanı birleştirme sırasında kimlik bilgileriyle ilgili sorunlar

Azure AD DS yönetilen etki alanına katılmak için kimlik bilgilerini isteyen bir iletişim kutusu alırsanız, VM Azure sanal ağını kullanarak etki alanına bağlanabilir. Etki alanı birleştirme işlemi, sağladığı kimlik bilgilerini kullanarak etki alanı birleştirme işlemini tamamlamak için etki alanına veya yetkilendirmeye kimlik doğrulamada başarısız olur.

Kimlik bilgileriyle ilgili sorunları gidermek için aşağıdaki sorun giderme adımlarını gözden geçirin:

1. Gibi kimlik bilgilerini belirtmek için UPN `dee@aaddscontoso.onmicrosoft.com`biçimini kullanmayı deneyin. Bu UPN'nin Azure AD'de doğru şekilde yapılandırıldığından emin olun.
    * Kiracınızda aynı UPN önekine sahip birden fazla kullanıcı varsa veya UPN önekiniz aşırı uzunsa, hesabınızdaki *SAMAccountName* otomatik olarak oluşturulabilir. Bu nedenle, hesabınızdaki *SAMAccountName* biçimi, şirket içi etki alanınızda beklediğiniz veya kullandığınızdan farklı olabilir.
1. VM'leri yönetilen etki alanına katılmak için Azure AD DS yönetilen etki alanının bir parçası olan bir kullanıcı hesabının kimlik bilgilerini kullanmayı deneyin.
1. [Parola eşitlemesini etkinleştirdiğinizden][enable-password-sync] ve ilk parola eşitlemesini tamamlamak için yeterince uzun süre beklediğinden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Etki alanına katılma işleminin bir parçası olarak Etkin Dizin işlemlerinin daha iyi anlaşılması [için, Birleştirme ve kimlik doğrulama sorunlarına][join-authentication-issues]bakın.

VM'inizi Azure AD DS yönetilen etki alanına katılmakta hala sorun yaşıyorsanız, [yardım bulun ve Azure Etkin Dizin için bir destek bileti açın.][azure-ad-support]

<!-- INTERNAL LINKS -->
[enable-password-sync]: tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds
[network-ports]: network-considerations.md#network-security-groups-and-required-ports
[azure-ad-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md

<!-- EXTERNAL LINKS -->
[join-authentication-issues]: /previous-versions/windows/it-pro/windows-2000-server/cc961817(v=technet.10)
