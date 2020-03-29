---
title: Varolan bir ADSync veritabanını kullanarak Azure AD Connect'i yükleme | Microsoft Dokümanlar
description: Bu konu, varolan bir ADSync veritabanının nasıl kullanılacağını açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.reviewer: cychua
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/30/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4dc6993586063c9c99a287c51d799b44f921768d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60245217"
---
# <a name="install-azure-ad-connect-using-an-existing-adsync-database"></a>Azure AD Connect'i mevcut bir AD Eşitleme veritabanını kullanarak yükleme
Azure AD Connect, verileri depolamak için bir SQL Server veritabanı gerektirir. Azure AD Connect yüklü varsayılan SQL Server 2012 Express LocalDB'yi kullanabilir veya kendi SQL sürümünüzü kullanabilirsiniz. Daha önce, Azure AD Connect'i yüklediğinizde, ADSync adında yeni bir veritabanı her zaman oluşturulmuştu. Azure AD Connect sürümü 1.1.613.0 (veya sonrası) ile, varolan bir ADSync veritabanına işaret ederek Azure AD Connect'i yükleme seçeneğiniz vardır.

## <a name="benefits-of-using-an-existing-adsync-database"></a>Varolan bir ADSync veritabanıkullanmanın yararları
Varolan bir ADSync veritabanını işaret ederek:

- Kimlik bilgileri dışında, ADSync veritabanında depolanan senkronizasyon yapılandırması (özel eşitleme kuralları, bağlayıcılar, filtreleme ve isteğe bağlı özellikler yapılandırması dahil) otomatik olarak kurtarılır ve yükleme sırasında kullanılır . Azure AD Connect tarafından şirket içi AD ve Azure AD ile değişiklikleri eşitlemek için kullanılan kimlik bilgileri şifrelenir ve yalnızca önceki Azure AD Connect sunucusu tarafından erişilebilir.
- ADSync veritabanında depolanan tüm kimlik verileri (bağlayıcı boşlukları ve metaverse ile ilişkili) ve eşitleme çerezleri de kurtarılır. Yeni yüklenen Azure AD Connect sunucusu, tam eşitleme gerçekleştirme gereksinimine sahip olmak yerine önceki Azure AD Connect sunucusunun kaldığı yerden eşitleme yapmaya devam edebilir.

## <a name="scenarios-where-using-an-existing-adsync-database-is-beneficial"></a>Varolan bir ADSync veritabanını kullanmanın yararlı olduğu senaryolar
Bu avantajlar aşağıdaki senaryolarda yararlıdır:


- Varolan bir Azure AD Connect dağıtımınız var. Varolan Azure AD Connect sunucunuz artık çalışmıyor, ancak ADSync veritabanını içeren SQL sunucusu hala çalışıyor. Yeni bir Azure AD Connect sunucusu yükleyebilir ve varolan ADSync veritabanına yönlendirebilirsiniz. 
- Varolan bir Azure AD Connect dağıtımınız var. ADSync veritabanını içeren SQL sunucunuz artık çalışmıyor. Ancak, veritabanının yeni bir yedekleme var. Önce ADSync veritabanını yeni bir SQL sunucusuna geri yükleyebilirsiniz. Bundan sonra, yeni bir Azure AD Connect sunucusu yükleyebilir ve geri yüklenen ADSync veritabanına yönlendirebilirsiniz.
- YerelDB kullanan varolan bir Azure AD Connect dağıtımınız vardır. LocalDB tarafından uygulanan 10 GB sınırı nedeniyle, tam SQL'e geçiş yapmak istiyorsunuz. ADSync veritabanını LocalDB'den yedekleyebilir ve bir SQL sunucusuna geri yükleyebilirsiniz. Bundan sonra, yeni bir Azure AD Connect sunucusunu yeniden yükleyebilir ve geri yüklenen ADSync veritabanına yönlendirebilirsiniz.
- Bir evreleme sunucusu ayarlamaya çalışıyorsunuz ve yapılandırmasının geçerli etkin sunucuyla eşleştiğinden emin olmak istiyor. ADSync veritabanını yedekleyebilir ve başka bir SQL sunucusuna geri yükleyebilirsiniz. Bundan sonra, yeni bir Azure AD Connect sunucusunu yeniden yükleyebilir ve geri yüklenen ADSync veritabanına yönlendirebilirsiniz.

## <a name="prerequisite-information"></a>Önkoşul bilgileri

Devam etmeden önce dikkat edilmesi gereken önemli notlar:

- Azure AD Connect'i Donanıma ve ön koşullara yüklemek için ön koşulları ve Azure AD Connect'i yüklemek için gereken hesap ve izinleri gözden geçirin. Azure AD Connect'i "varolan veritabanını kullan" modunu kullanarak yüklemek için gereken izinler, "özel" yüklemeyle aynıdır.
- Azure AD Connect'i varolan bir ADSync veritabanına dağıtmak yalnızca tam SQL ile desteklenir. SQL Express LocalDB ile desteklenmez. LocalDB'de kullanmak istediğiniz varolan bir ADSync veritabanınız varsa, önce ADSync veritabanını yedeklemeniz ve tam SQL'e geri yüklemeniz gerekir. Bundan sonra, bu yöntemi kullanarak geri yüklenen veritabanına karşı Azure AD Connect dağıtabilirsiniz.
- Yükleme için kullanılan Azure AD Connect sürümü aşağıdaki ölçütleri karşılamalıdır:
    - 1.1.613.0 veya üzeri, VE
    - ADSync veritabanında en son kullanılan Azure AD Connect sürümüyle aynı veya daha yüksek. Yükleme için kullanılan Azure AD Connect sürümü, ADSync veritabanında en son kullanılan sürümden daha yüksekse, tam bir eşitleme gerekebilir.  İki sürüm arasında şema veya eşitleme kuralı değişiklikleri varsa tam eşitleme gereklidir. 
- AdSync veritabanı kullanılan nispeten yeni bir eşitleme durumu içermelidir. Varolan ADSync veritabanı ile son eşitleme etkinliği son üç hafta içinde olmalıdır.
- Azure AD Connect'i "varolan veritabanını kullan" yöntemini kullanarak yüklerken, önceki Azure AD Connect sunucusunda yapılandırılan oturum açma yöntemi korunmaz. Ayrıca, yükleme sırasında oturum açma yöntemini yapılandıramazsınız. Oturum açma yöntemini yalnızca yükleme tamamlandıktan sonra yapılandırabilirsiniz.
- Birden çok Azure AD Connect sunucusunun aynı ADSync veritabanını paylaşmasını sağlayamazsınız. "Varolan veritabanını kullan" yöntemi, varolan bir ADSync veritabanını yeni bir Azure AD Connect sunucusuyla yeniden kullanmanıza olanak tanır. Paylaşımı desteklemez.

## <a name="steps-to-install-azure-ad-connect-with-use-existing-database-mode"></a>Azure AD Connect'i "varolan veritabanını kullan" moduyla yükleme adımları
1.  Azure AD Connect yükleyicisini (AzureADConnect.MSI) Windows sunucusuna indirin. Azure AD Connect'i yüklemeye başlamak için Azure AD Connect yükleyicisini çift tıklatın.
2.  MSI yüklemesi tamamlandıktan sonra Azure AD Connect sihirbazı Hızlı mod kurulumu açılır. Çıkış simgesine tıklayarak ekranı kapatın.
![Hoş geldiniz](./media/how-to-connect-install-existing-database/db1.png)
3.  Yeni bir komut istemi veya PowerShell oturumu başlatın. "C:\Program Files\Microsoft Azure Active Directory Connect" klasörüne gidin. Azure AD Connect sihirbazını "Var olan veritabanını kullan" modunda başlatmak için .\AzureADConnect.exe /useexistingdatabase komutunu çalıştırın.

> [!NOTE]
> Yalnızca veritabanı daha önceki bir Azure AD Connect yüklemesinden veri içeriyorsa **/UseExistingDatabase** anahtarını kullanın. Örneğin, yerel bir veritabanından tam bir SQL Server veritabanına taşınırken veya Azure AD Connect sunucusu yeniden oluşturulurken ve ADSync veritabanının SQL yedeklemesini daha önceki bir Azure AD Connect yüklemesinden geri yüklediğinizde. Veritabanı boşsa, diğer bir deyişle önceki Bir Azure AD Connect yüklemesinden herhangi bir veri içermiyorsa, bu adımı atlayın.

![PowerShell](./media/how-to-connect-install-existing-database/db2.png)
1. Azure AD Connect'e Hoş Geldiniz ekranı açılır. Lisans koşullarını ve gizlilik bildirimini kabul ettikten sonra **Devam**'a tıklayın.
   ![Hoş geldiniz](./media/how-to-connect-install-existing-database/db3.png)
1. **Gerekli bileşenleri yükleme** ekranında **Mevcut bir SQL Server'ı kullanma** seçeneği etkinleştirilir. AD Eşitleme veritabanını barındıran SQL sunucusunun adını belirtin. AD Eşitleme veritabanını barındırmak için kullanılan SQL altyapısı örneği SQL sunucusundaki varsayılan örnek değilse SQL altyapısı örneği adını belirtmeniz gerekir. Ayrıca SQL göz atma özelliği etkin değilse SQL altyapısı örneği bağlantı noktası numarasını da belirtmeniz gerekir. Örnek:         
   ![Hoş geldiniz](./media/how-to-connect-install-existing-database/db4.png)           

1. **Azure AD'ye bağlan** ekranında Azure AD dizininizin genel yöneticisinin kimlik bilgilerini girmeniz gerekir. Varsayılan onmicrosoft.com etki alanındaki bir hesabı kullanmanız önerilir. Bu hesap yalnızca Azure AD'de hizmet hesabı oluşturmak için kullanılır ve sihirbaz tamamlandıktan sonra kullanılmaz.
   ![Bağlanma](./media/how-to-connect-install-existing-database/db5.png)
 
1. **Connect your directories** (Dizinlerinize bağlanın) ekranında dizin eşitlemesi için yapılandırılmış olan mevcut AD ormanı yanında kırmızı bir çarpı işaretiyle listelenir. Şirket içi AD ormanında yapılan değişiklikleri eşitlemek için bir AD DS hesabı kullanmanız gerekir. Kimlik bilgileri şifrelenmiş olduğundan ve şifresi yalnızca önceki Azure AD Connect sunucusu tarafından çözülebildiğinden Azure AD Connect sihirbazı, AD Eşitleme veritabanında depolanan AD DS hesabının kimlik bilgilerini alamaz. AD ormanına ait AD DS hesabını belirtmek için **Change Credentials** (Kimlik Bilgilerini Değiştir) öğesine tıklayın.
   ![Dizinler](./media/how-to-connect-install-existing-database/db6.png)
 
 
1. Açılan iletişim kutusunda (i) Kuruluş Yöneticisi kimlik bilgileri belirtip Azure AD Connect'in sizin için bir AD DS hesabı oluşturmasını sağlayabilir veya (ii) AD DS hesabını kendiniz oluşturarak kimlik bilgilerini Azure AD Connect'e girebilirsiniz. Bir seçeneği belirleyip gerekli kimlik bilgilerini girdikten sonra **Tamam**'a tıklayarak açılan iletişim kutusunu kapatın.
   ![Hoş geldiniz](./media/how-to-connect-install-existing-database/db7.png)
 
 
1. Kimlik bilgileri girildikten sonra kırmızı çarpı işaretinin yerine yeşil onay işareti görünür. **İleri**'ye tıklayın.
   ![Hoş geldiniz](./media/how-to-connect-install-existing-database/db8.png)
 
 
1. **Yapılandırma için hazır** ekranında **Yükle**'ye tıklayın.
   ![Hoş geldiniz](./media/how-to-connect-install-existing-database/db9.png)
 
 
1. Yükleme tamamlandıktan sonra Azure AD Connect otomatik olarak Hazırlama Modunda etkinleştirilir. Hazırlama Modunu devre dışı bırakmadan önce sunucu yapılandırmasını ve bekleme durumundaki dışarı aktarma işlemlerini gözden geçirerek beklenmeyen değişikliklerin olup olmadığını kontrol etmeniz önerilir. 

## <a name="post-installation-tasks"></a>Yükleme sonrası görevler
1.2.65.0'dan önce Azure AD Connect sürümü tarafından oluşturulan bir veritabanı yedeklemesini geri gönderirken, evreleme sunucusu otomatik olarak **Yapılandırma Yapma'nın**oturum açma yöntemini seçer. Parola karma eşitleme ve parola yazma tercihleriniz geri yüklenirken, oturum açma yöntemini etkin eşitleme sunucunuz için geçerli olan diğer ilkelerle eşleşecek şekilde değiştirmeniz gerekir.  Bu adımların tamamlanmaması, bu sunucunun etkin hale gelmesi durumunda kullanıcıların oturum açmasını engelleyebilir.  

Gerekli olan ek adımları doğrulamak için aşağıdaki tabloyu kullanın.

|Özellik|Adımlar|
|-----|-----|
|Şifre Karma Senkronizasyonu| Parola Karma Eşitleme ve Parola geri yazma ayarları, 1.2.65.0'dan başlayan Azure AD Connect sürümleri için tamamen geri yüklenir.  Azure AD Connect'in eski bir sürümünü kullanarak geri geliyorsanız, etkin eşitleme sunucunuzla eşleştiklerinden emin olmak için bu özelliklerin eşitleme seçeneği ayarlarını gözden geçirin.  Başka yapılandırma adımları gerekli olmamalıdır.|
|AD FS ile Federasyon|Azure kimlik doğrulamaları, etkin eşitleme sunucunuz için yapılandırılan AD FS ilkesini kullanmaya devam eder.  AD FS çiftliğinizi yönetmek için Azure AD Connect'i kullanıyorsanız, bekleme sunucunuzun etkin eşitleme örneği olmasına hazırlanmak için oturum açma yöntemini isteğe bağlı olarak AD FS federasyonu olarak değiştirebilirsiniz.   Etkin eşitleme sunucusunda aygıt seçenekleri etkinse, "Aygıt seçeneklerini yapılandır" görevini çalıştırarak bu sunucudaki bu seçenekleri yapılandırın.|
|Geçiş kimlik doğrulaması ve Masaüstü Tek Oturum Açma|Etkin eşitleme sunucunuzdaki yapılandırmayla eşleşecek şekilde oturum açma yöntemini güncelleştirin.  Sunucuyu birincil olarak tanıtmadan önce bu izlenmezse, Kesintisiz Tek Oturum açma ile birlikte geçiş kimlik doğrulaması devre dışı bırakılır ve yedek oturum açma seçeneği olarak parola karma eşitlemeniz yoksa kiracınız kilitlenebilir. Ayrıca, evreleme modunda geçiş kimlik doğrulamasını etkinleştirdiğinizde, yeni bir kimlik doğrulama aracısının yüklenir, kaydedilir ve oturum açma isteklerini kabul edecek yüksek kullanılabilirlikli bir aracı olarak çalışacaktır.|
|PingFederate ile federasyon|Azure kimlik doğrulamaları, etkin eşitleme sunucunuz için yapılandırılan PingFederate ilkesini kullanmaya devam eder.  Oturum açma yöntemini isteğe bağlı olarak, bekleme sunucunuzun etkin senkronizasyon örneği haline gelmesine hazırlık olarak PingFederate olarak değiştirebilirsiniz.  Bu adım, PingFederate ile ek etki alanları federe gerekir kadar ertelenebilir.|

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD Connect'i yüklediniz, artık [yüklemeyi doğrulayabilir ve lisans atayabilirsiniz](how-to-connect-post-installation.md).
- Yüklemeyle etkinleştirilen özellikler hakkında daha fazla bilgi edinin: [Yanlışlıkla silmeleri engelleme](how-to-connect-sync-feature-prevent-accidental-deletes.md) ve [Azure AD Connect Health](how-to-connect-health-sync.md).
- Şu genel konu başlıkları hakkında daha fazla bilgi edinin: [Zamanlayıcı ve eşitleme tetikleme](how-to-connect-sync-feature-scheduler.md).
- [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
