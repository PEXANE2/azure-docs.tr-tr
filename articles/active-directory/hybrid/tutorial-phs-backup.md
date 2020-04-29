---
title: "Öğretici: Azure AD Connect 'de AD FS için yedekleme olarak bir yedekleme ayarlama | Microsoft Docs"
description: Parola karması eşitlemesini yedekleme ve AD FS için nasıl kullanacağınızı gösterir.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: tutorial
ms.date: 04/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3e5ad7badfa44a006fd7e71d3b0e42ee95ac698d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "64919000"
---
# <a name="tutorial--setting-up-phs-as-backup-for-ad-fs-in-azure-ad-connect"></a>Öğretici: Azure AD Connect AD FS için bir yedekleme olarak e-s ayarlama

Aşağıdaki öğreticide, Parola karması eşitlemesini yedekleme olarak ayarlama ve AD FS için yük devretme işlemi adım adım gösterilmektedir.  Bu belge Ayrıca, AD FS başarısız olduysa veya kullanılamaz hale gelirse, birincil kimlik doğrulama yöntemi olarak parola karması eşitlemesini nasıl etkinleştireceğinizi gösterir.

>[!NOTE] 
>Bu adımlar genellikle acil durum veya kesinti durumları sırasında gerçekleştirilebilse de, bir kesinti gerçekleşmeden önce bu adımları test etmeniz ve yordamlarınızı doğrulamanız önerilir.

>[!NOTE]
>Azure AD Connect sunucusuna erişiminiz yok veya sunucunun internet erişimi yok durumunda, Azure AD tarafında yapılan değişikliklere yardımcı olmak için [Microsoft desteği](https://support.microsoft.com/en-us/contactus/) başvurabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu öğretici [öğreticiye dayanır: tek BIR ad ormanı ortamını buluta Federasyonun](tutorial-federation.md) ve bu öğreticiyi denemeden önce bir önkoşul vardır.  Bu öğreticiyi tamamlamadıysanız, bu belgedeki adımları denemeden önce bunu yapın.

>[!IMPORTANT]
>PHS 'ye geçmeden önce AD FS ortamınızın bir yedeğini oluşturmalısınız.  Bu işlem, [AD FS hızlı geri yükleme aracı](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-rapid-restore-tool#how-to-use-the-tool)kullanılarak yapılabilir.

## <a name="enable-phs-in-azure-ad-connect"></a>Azure AD Connect ' de PHS 'yi etkinleştirme
İlk adım, artık Federasyon kullanan bir Azure AD Connect ortamınız olduğuna göre, Parola karması eşitlemesini açmak ve Azure AD Connect karmaları eşitlemesine izin vermek.

Şunları yapın:

1.  Masaüstünde oluşturulan Azure AD Connect simgesine çift tıklayın
2.  **Yapılandır**'a tıklayın.
3.  Ek görevler sayfasında, **eşitleme seçeneklerini Özelleştir** ' i seçin ve **İleri**' ye tıklayın.
4.  Genel yöneticiniz için Kullanıcı adını ve parolayı girin.  Bu hesap, önceki [öğreticide oluşturulmuştur.](tutorial-federation.md#create-a-global-administrator-in-azure-ad)
5.  **Dizinlerinizi bağlama** ekranında **İleri**' ye tıklayın.
6.  **Etki alanı ve OU filtreleme** ekranında **İleri**' ye tıklayın.
7.  **Isteğe bağlı özellikler** ekranında **Parola karması eşitlemesini** denetleyip **İleri**' ye tıklayın.
![Seç](media/tutorial-phs-backup/backup1.png)</br>
8.  **Yapılandırmaya hazırlanma** ekranında **Yapılandır**' a tıklayın.
9.  Yapılandırma tamamlandıktan sonra **Çıkış**' a tıklayın.
10. İşte bu kadar!  İşiniz bitti.  Parola karması eşitleme artık oluşur ve AD FS kullanılamaz hale gelirse yedekleme olarak kullanılabilir.

## <a name="switch-to-password-hash-synchronization"></a>Parola karması eşitlemesine geç
Bu aşamada, Parola karması eşitlemesine nasıl geçiş yapılacağını göstereceğiz. Başlamadan önce, anahtarı yapmanız gereken koşulları göz önünde bulundurun. Ağ kesintisi, küçük AD FS bir sorun veya kullanıcılarınızın bir alt kümesini etkileyen bir sorun gibi geçici nedenlerle anahtarı yapmayın. Sorunu düzeltmek çok uzun sürebileceğinden anahtarı yapmaya karar verirseniz şunları yapın:

> [!IMPORTANT]
> Parola karmalarının Azure AD 'ye eşitlenmesi biraz zaman alabilir.  Bu, eşitlemelerin tamamlanması için 3 saat sürebileceği ve parola karmalarını kullanarak kimlik doğrulamaya başlayabilmeniz için önce bu süreyi belirtir.

1. Masaüstünde oluşturulan Azure AD Connect simgesine çift tıklayın
2.  **Yapılandır**'a tıklayın.
3.  **Kullanıcı oturumunu Değiştir** ' i seçin ve **İleri**' ye tıklayın.
![Değiştir](media/tutorial-phs-backup/backup2.png)</br>
4.  Genel yöneticiniz için Kullanıcı adını ve parolayı girin.  Bu hesap, önceki [öğreticide oluşturulmuştur.](tutorial-federation.md#create-a-global-administrator-in-azure-ad)
5.  **Kullanıcı oturum açma** ekranında **Parola karması eşitleme** ' yi seçin ve **Kullanıcı hesaplarını dönüştürme** kutusuna bir onay işareti koyun.  
6.  Varsayılan **olarak çoklu oturum açmayı etkinleştir** ' i seçili bırakın ve **İleri**' ye tıklayın.
7.  **Çoklu oturum açmayı etkinleştir** ekranında **İleri**' ye tıklayın.
8.  **Yapılandırmaya hazırlanma** ekranında, **Yapılandır**' a tıklayın.
9.  Yapılandırma tamamlandıktan sonra **Çıkış**' a tıklayın.
10. Kullanıcılar artık parolalarını kullanarak Azure 'da ve Azure hizmetlerinde oturum açabilir.

## <a name="test-signing-in-with-one-of-our-users"></a>Kullanıcılarımızdan biriyle oturum açma testi

1. Buraya gidin[https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Yeni kiracımızda oluşturulmuş bir kullanıcı hesabıyla oturum açın.  Şu biçimi kullanarak oturum açmanız gerekir: (user@domain.onmicrosoft.com). Kullanıcının şirket içinde oturum açması için kullandığı parolayı kullanın.</br>
   ![Doğrulama](media/tutorial-password-hash-sync/verify1.png)</br>

## <a name="switch-back-to-federation"></a>Federasyona geri dönün
Şimdi, Federasyona nasıl geri geçiş yapılacağını göstereceğiz.  Bunu yapmak için aşağıdakileri yapın:

1.  Masaüstünde oluşturulan Azure AD Connect simgesine çift tıklayın
2.  **Yapılandır**'a tıklayın.
3.  **Kullanıcı oturumunu Değiştir** ' i seçin ve **İleri**' ye tıklayın.
4.  Genel yöneticiniz için Kullanıcı adını ve parolayı girin.  Bu, önceki öğreticide [burada](tutorial-federation.md#create-a-global-administrator-in-azure-ad) oluşturulan hesaptır.
5.  **Kullanıcı oturum açma** ekranında **AD FS Federasyon** ' ı seçin ve **İleri**' ye tıklayın.  
6. Etki alanı yöneticisi kimlik bilgileri sayfasında, contoso\Administrator Kullanıcı adını ve parolasını girin ve Ileri ' ye tıklayın **.**
7. AD FS grubu ekranında **İleri**' ye tıklayın.
8. **Azure AD etki alanı** ekranında, açılan listeden etki alanını seçin ve **İleri**' ye tıklayın.
9. **Yapılandırmaya hazırlanma** ekranında, **Yapılandır**' a tıklayın.
10. Yapılandırma tamamlandıktan sonra **İleri**' ye tıklayın.
![Yapılandırma](media/tutorial-phs-backup/backup4.png)</br>
11. **Federasyon bağlantısını doğrula** ekranında **Doğrula**' ya tıklayın.  Bunun başarıyla tamamlanabilmesi için DNS kayıtlarını (bir ve AAAA kayıtları ekleyin) yapılandırmanız gerekebilir.
![Doğrulama](media/tutorial-phs-backup/backup5.png)</br>
12. **Çıkış**'a tıklayın.

## <a name="reset-the-ad-fs-and-azure-trust"></a>AD FS ve Azure güvenini sıfırlayın
Artık AD FS ve Azure arasındaki güveni sıfırlamamız gerekir.

1.  Masaüstünde oluşturulan Azure AD Connect simgesine çift tıklayın
2.  **Yapılandır**'a tıklayın.
3.  **Federasyonu Yönet** ' i seçin ve **İleri**' ye tıklayın.
4.  **Azure AD güvenini Sıfırla** ' yı seçin ve **İleri**' ye tıklayın.
![Reset](media/tutorial-phs-backup/backup6.png)</br>
5.  **Azure AD 'ye bağlanma** ekranında, genel yöneticiniz için Kullanıcı adını ve parolayı girin.
6.  **AD FS Bağlan** ekranında, contoso\administrator Kullanıcı adını ve parolasını girin ve ileri ' ye tıklayın **.**
7.  **Sertifikalar** ekranında **İleri**' ye tıklayın.

## <a name="test-signing-in-with-one-of-our-users"></a>Kullanıcılarımızdan biriyle oturum açma testi

1.  Buraya gidin[https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Yeni kiracımızda oluşturulmuş bir kullanıcı hesabıyla oturum açın.  Şu biçimi kullanarak oturum açmanız gerekir: (user@domain.onmicrosoft.com). Kullanıcının şirket içinde oturum açması için kullandığı parolayı kullanın.
![Doğrulama](media/tutorial-password-hash-sync/verify1.png)

Artık Azure 'un sunabileceği bir karma kimlik ortamını test etmek ve tanımak için kullanabileceğiniz bir karma kimlik ortamı oluşturdunuz.

## <a name="next-steps"></a>Sonraki adımlar


- [Donanım ve önkoşullar](how-to-connect-install-prerequisites.md) 
- [Hızlı ayarlar](how-to-connect-install-express.md)
- [Parola karması eşitleme](how-to-connect-password-hash-synchronization.md)
