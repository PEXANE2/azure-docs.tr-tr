---
title: "Öğretici: Azure AD Connect'te AD FS için yedek olarak PHS'yi ayarlama | Microsoft Dokümanlar"
description: Yedekleme olarak ve AD FS için parola karma eşitlemesini nasıl açılabildiğinizi gösterir.
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
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "64919000"
---
# <a name="tutorial--setting-up-phs-as-backup-for-ad-fs-in-azure-ad-connect"></a>Öğretici: Azure AD Connect'te AD FS için yedek olarak PHS ayarlama

Aşağıdaki öğretici, AD FS için yedekleme ve başarısız lık olarak parola karma eşitlemesini ayarlamada size yol gösterir.  Bu belge, AD FS başarısız olmuşsa veya kullanılamıyorsa, birincil kimlik doğrulama yöntemi olarak parola karma eşitlemenin nasıl etkinleştirilen olduğunu da gösterir.

>[!NOTE] 
>Bu adımlar genellikle acil durum veya kesinti durumlarında gerçekleştirilse de, bu adımları test edip bir kesinti meydana gelmeden önce yordamlarınızı doğrulamanız önerilir.

>[!NOTE]
>Azure AD Connect sunucusuna erişiminiz yoksa veya sunucunun Internet erişimi yoksa, Azure AD tarafındaki değişikliklere yardımcı olmak için [Microsoft Destek'e](https://support.microsoft.com/en-us/contactus/) başvurabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu öğretici Öğretici üzerine inşa: [Bulut tek bir AD orman ortamı fetodi](tutorial-federation.md) ve bu öğretici denemeden önce bir per-quiquisite.  Bu öğreticiyi tamamlamadıysanız, bu belgedeki adımları denemeden önce bunu yapın.

>[!IMPORTANT]
>PHS'ye geçmeden önce AD FS ortamınızın bir yedeklemesini oluşturmanız gerekir.  Bu [AD FS Hızlı Geri Yükleme Aracı](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-rapid-restore-tool#how-to-use-the-tool)kullanılarak yapılabilir.

## <a name="enable-phs-in-azure-ad-connect"></a>Azure AD Connect'te PHS'i etkinleştirme
Federasyon kullanan bir Azure AD Connect ortamına sahip olduğumuza göre ilk adım, parola karma eşitlemeyi açmak ve Azure AD Connect'in karmaları eşitlemesine izin vermektir.

Şunları yapın:

1.  Masaüstünde oluşturulan Azure AD Bağlantısı simgesine çift tıklayın
2.  **Yapılandır**'a tıklayın.
3.  Ek görevler sayfasında, **eşitleme seçeneklerini özelleştir'i** seçin ve **İleri'yi**tıklatın.
4.  Global yöneticinizin kullanıcı adını ve parolasını girin.  Bu hesap [burada](tutorial-federation.md#create-a-global-administrator-in-azure-ad) önceki öğretici oluşturuldu.
5.  **Dizinlerinizi Bağla** ekranında **İleri'yi**tıklatın.
6.  Etki **Alanı ve OU filtreleme** ekranında **İleri'yi**tıklatın.
7.  **İsteğe Bağlı özellikler** ekranında **Parola karma senkronizasyonunu** denetleyin ve **İleri'yi**tıklatın.
![Seç](media/tutorial-phs-backup/backup1.png)</br>
8.  **Yapılandırmaya Hazır** ekranında **Yapılandırma'yı**tıklatın.
9.  Yapılandırma tamamlandıktan sonra **Çıkış'ı**tıklatın.
10. İşte bu kadar!  Senin bitirdin.  Parola karma eşitleme şimdi oluşur ve AD FS kullanılamaz hale gelirse yedek olarak kullanılabilir.

## <a name="switch-to-password-hash-synchronization"></a>Parola karma senkronizasyonuna geçiş
Şimdi, size şifre karma senkronizasyonuna nasıl geçeceğinizi göstereceğiz. Başlamadan önce, geçişi hangi koşullar altında yapmanız gerektiğini göz önünde bulundurun. Geçişi, ağ kesintisi, küçük bir AD FS sorunu veya kullanıcılarınızın bir alt kümesini etkileyen bir sorun gibi geçici nedenlerle yapmayın. Sorunu gidermek çok uzun süreceği için geçiş yapmaya karar verirseniz, aşağıdakileri yapın:

> [!IMPORTANT]
> Parola hashelerinin Azure AD ile eşitlesinin biraz zaman alacağını unutmayın.  Bu, eşitlemelerin tamamlanmasının 3 saat sürebileceği ve parola kalıplarını kullanarak kimlik doğrulamaya başlayabileceğiniz anlamına gelir.

1. Masaüstünde oluşturulan Azure AD Bağlantısı simgesine çift tıklayın
2.  **Yapılandır**'a tıklayın.
3.  **Kullanıcı oturum aç'ı değiştir'i** seçin ve **İleri'yi**tıklatın.
![Değiştir](media/tutorial-phs-backup/backup2.png)</br>
4.  Global yöneticinizin kullanıcı adını ve parolasını girin.  Bu hesap [burada](tutorial-federation.md#create-a-global-administrator-in-azure-ad) önceki öğretici oluşturuldu.
5.  Kullanıcı **oturum açma** ekranında **Parola Hash Senkronizasyonunu** seçin ve **kullanıcı hesaplarını dönüştürme kutusunun** üzerine bir çek yerleştirin.  
6.  Varsayılan tek oturum açma seçili **etkinleştir'i** bırakın ve **İleri'yi**tıklatın.
7.  Tek **oturum açma** ekranında **İleri'yi**tıklatın.
8.  **Yapılandırmaya Hazır** ekranında, **Yapılandırma'yı**tıklatın.
9.  Yapılandırma tamamlandıktan sonra **Çıkış'ı**tıklatın.
10. Kullanıcılar artık parolalarını Azure ve Azure hizmetlerinde oturum açabilmek için kullanabilir.

## <a name="test-signing-in-with-one-of-our-users"></a>Kullanıcılarımızdan biriyle oturum açma testini test edin

1. Göz atın[https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Yeni kiracımızda oluşturulan bir kullanıcı hesabıyla oturum açın.  Aşağıdaki biçimi kullanarak oturum açmanız gerekir: (user@domain.onmicrosoft.com). Kullanıcının şirket içinde oturum açmada kullandığı parolayı kullanın.</br>
   ![Doğrulama](media/tutorial-password-hash-sync/verify1.png)</br>

## <a name="switch-back-to-federation"></a>Federasyona geri dön
Şimdi size federasyona nasıl döneceğinizi göstereceğiz.  Bunu yapmak için aşağıdakileri yapın:

1.  Masaüstünde oluşturulan Azure AD Bağlantısı simgesine çift tıklayın
2.  **Yapılandır**'a tıklayın.
3.  **Kullanıcı oturum aç'ı değiştir'i** seçin ve **İleri'yi**tıklatın.
4.  Global yöneticinizin kullanıcı adını ve parolasını girin.  Bu, önceki öğreticide [burada](tutorial-federation.md#create-a-global-administrator-in-azure-ad) oluşturulan hesaptır.
5.  Kullanıcı **oturum açma** ekranında, **AD FS'li Federasyon'u** seçin ve **İleri'yi**tıklatın.  
6. Etki Alanı Yöneticisi kimlik bilgileri sayfasına contoso\Administrator kullanıcı adı ve parolasını girin ve **İleri'yi tıklatın.**
7. AD FS çiftlik ekranında **İleri'yi**tıklatın.
8. Azure **AD etki alanı** ekranında, açılan alandan etki alanını seçin ve **İleri'yi**tıklatın.
9. **Yapılandırmaya Hazır** ekranında, **Yapılandırma'yı**tıklatın.
10. Yapılandırma tamamlandıktan sonra **İleri'yi**tıklatın.
![Yapılandırma](media/tutorial-phs-backup/backup4.png)</br>
11. Federasyon **bağlantısını doğrula** ekranında, **Doğrula'yı**tıklatın.  Bunun başarılı bir şekilde tamamlanması için DNS kayıtlarını yapılandırmanız (A ve AAAA kayıtları eklemeniz) gerekebilir.
![Doğrulama](media/tutorial-phs-backup/backup5.png)</br>
12. **Çıkış**'a tıklayın.

## <a name="reset-the-ad-fs-and-azure-trust"></a>AD FS ve Azure güvenini sıfırlama
Şimdi AD FS ve Azure arasındaki güveni sıfırlamamız gerekiyor.

1.  Masaüstünde oluşturulan Azure AD Bağlantısı simgesine çift tıklayın
2.  **Yapılandır**'a tıklayın.
3.  **Federasyonu Yönet'i** seçin ve **İleri'yi**tıklatın.
4.  **Azure AD güvenini sıfırla'yı** seçin ve **İleri'yi**tıklatın.
![Reset](media/tutorial-phs-backup/backup6.png)</br>
5.  **Azure'a Bağlan AD** ekranına, global yöneticinizin kullanıcı adını ve parolasını girin.
6.  AD **FS'e Bağlan** ekranında contoso\Administrator kullanıcı adı ve parolasını girin ve **İleri'yi tıklatın.**
7.  **Sertifikalar** ekranında **İleri'yi**tıklatın.

## <a name="test-signing-in-with-one-of-our-users"></a>Kullanıcılarımızdan biriyle oturum açma testini test edin

1.  Göz atın[https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Yeni kiracımızda oluşturulan bir kullanıcı hesabıyla oturum açın.  Aşağıdaki biçimi kullanarak oturum açmanız gerekir:user@domain.onmicrosoft.com( ). Kullanıcının şirket içinde oturum açmada kullandığı parolayı kullanın.
![Doğrulama](media/tutorial-password-hash-sync/verify1.png)

Azure'un sunduklarını sınamak ve tanımak için kullanabileceğiniz karma bir kimlik ortamını başarıyla kurabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar


- [Donanım ve önkoşullar](how-to-connect-install-prerequisites.md) 
- [Ekspres ayarlar](how-to-connect-install-express.md)
- [Parola karması eşitleme](how-to-connect-password-hash-synchronization.md)
