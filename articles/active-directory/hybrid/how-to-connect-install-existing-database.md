---
title: Mevcut bir ADSync veritabanını kullanarak Azure AD Connect 'yi yükler | Microsoft Docs
description: Bu konu başlığı altında, var olan bir ADSync veritabanının nasıl kullanılacağı açıklanmaktadır.
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
ms.topic: how-to
ms.date: 08/30/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 23bcb63b6b499e72cb43089659e513d276bd8306
ms.sourcegitcommit: f98ab5af0fa17a9bba575286c588af36ff075615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85358982"
---
# <a name="install-azure-ad-connect-using-an-existing-adsync-database"></a>Azure AD Connect'i mevcut bir AD Eşitleme veritabanını kullanarak yükleme
Azure AD Connect, verileri depolamak için SQL Server bir veritabanı gerektirir. Azure AD Connect yüklü olan SQL Server Express LocalDB varsayılan 2012 kullanabilir veya kendi tam SQL sürümünüzü kullanabilirsiniz. Daha önce Azure AD Connect yüklediğinizde, ADSync adlı yeni bir veritabanı her zaman oluşturulmuştur. Azure AD Connect sürüm 1.1.613.0 (veya sonrası) ile, var olan bir ADSync veritabanına işaret ederek Azure AD Connect yükleyebilirsiniz.

## <a name="benefits-of-using-an-existing-adsync-database"></a>Mevcut bir ADSync veritabanını kullanmanın avantajları
Varolan bir ADSync veritabanına işaret ederek:

- Kimlik bilgileri hariç, ADSync veritabanında depolanan eşitleme yapılandırması (özel eşitleme kuralları, bağlayıcılar, filtreleme ve isteğe bağlı özellikler yapılandırma dahil) otomatik olarak kurtarılır ve yükleme sırasında kullanılır. Azure AD Connect tarafından, şirket içi AD ve Azure AD ile değişiklikleri eşitlemeye yönelik kimlik bilgileri şifrelenir ve yalnızca önceki Azure AD Connect sunucusu tarafından erişilebilir.
- ADSync veritabanında depolanan tüm kimlik verileri (bağlayıcı alanları ve metadize ile ilişkili) ve eşitleme tanımlama bilgileri de kurtarılır. Yeni yüklenen Azure AD Connect sunucusu, bir tam eşitleme gerçekleştirmek zorunda kalmak yerine önceki Azure AD Connect sunucusunun sol tarafında eşitlemeye devam edebilir.

## <a name="scenarios-where-using-an-existing-adsync-database-is-beneficial"></a>Mevcut bir ADSync veritabanını kullanmanın faydalı olduğu senaryolar
Bu avantajlar aşağıdaki senaryolarda faydalıdır:


- Mevcut bir Azure AD Connect dağıtımınız var. Mevcut Azure AD Connect sunucunuz artık çalışmıyor, ancak ADSync veritabanını içeren SQL Server hala çalışıyor. Yeni bir Azure AD Connect sunucusu yükleyebilir ve var olan ADSync veritabanına işaret edebilirsiniz. 
- Mevcut bir Azure AD Connect dağıtımınız var. ADSync veritabanını içeren SQL Server 'niz artık çalışmıyor. Ancak, veritabanının yeni bir yedeğini alırsınız. ADSync veritabanını önce yeni bir SQL Server 'a geri yükleyebilirsiniz. Bundan sonra yeni bir Azure AD Connect sunucusu yükleyebilir ve geri yüklenen ADSync veritabanına işaret edebilirsiniz.
- LocalDB kullanan mevcut bir Azure AD Connect dağıtımınız var. LocalDB tarafından uygulanan 10 GB 'lik sınır nedeniyle tam SQL 'e geçiş yapmak istersiniz. ADSync veritabanını LocalDB 'den yedekleyebilir ve bunu bir SQL Server 'a geri yükleyebilirsiniz. Bundan sonra yeni bir Azure AD Connect sunucusunu yeniden yükleyebilir ve geri yüklenen ADSync veritabanına işaret edebilirsiniz.
- Hazırlama sunucusu kurmaya çalışıyorsunuz ve yapılandırmasının geçerli etkin sunucu ile eşleştiğinden emin olmak istiyor. ADSync veritabanını yedekleyebilir ve bunu başka bir SQL Server 'a geri yükleyebilirsiniz. Bundan sonra yeni bir Azure AD Connect sunucusunu yeniden yükleyebilir ve geri yüklenen ADSync veritabanına işaret edebilirsiniz.

## <a name="prerequisite-information"></a>Önkoşul bilgileri

Devam etmeden önce dikkat etmeniz gereken önemli notlar:

- Donanım ve önkoşullara Azure AD Connect yüklemek için önkoşulları ve Azure AD Connect yüklemek için gereken hesap ve izinleri gözden geçirdiğinizden emin olun. "Var olan veritabanını kullan" modunu kullanarak Azure AD Connect yüklemek için gereken izinler "özel" yükleme ile aynıdır.
- Mevcut bir ADSync veritabanına karşı Azure AD Connect dağıtmak yalnızca tam SQL ile desteklenir. SQL Express LocalDB ile desteklenmez. LocalDB 'de kullanmak istediğiniz mevcut bir ADSync veritabanınız varsa, önce ADSync veritabanını (LocalDB) yedeklemeniz ve tam SQL 'e geri yüklemeniz gerekir. Bundan sonra, bu yöntemi kullanarak geri yüklenen veritabanına karşı Azure AD Connect dağıtabilirsiniz.
- Yükleme için kullanılan Azure AD Connect sürümünün aşağıdaki ölçütleri karşılaması gerekir:
    - 1.1.613.0 veya üzeri ve
    - ADSync veritabanıyla en son kullanılan Azure AD Connect sürümünden aynı veya daha yüksek. Yükleme için kullanılan Azure AD Connect sürümü, ADSync veritabanıyla en son kullanılan sürümden yüksekse, tam eşitleme gerekebilir.  İki sürüm arasında şema veya eşitleme kuralı değişikliği varsa tam eşitleme gereklidir. 
- Kullanılan ADSync veritabanı görece olarak yeni bir eşitleme durumu içermelidir. Mevcut ADSync veritabanına sahip son eşitleme etkinliği son üç hafta içinde olmalıdır.
- "Var olan veritabanını kullan" yöntemi kullanılarak Azure AD Connect yüklenirken, önceki Azure AD Connect sunucusunda yapılandırılan oturum açma yöntemi korunmaz. Ayrıca, yükleme sırasında oturum açma yöntemini yapılandıramazsınız. Oturum açma yöntemini, yükleme tamamlandıktan sonra yapılandırabilirsiniz.
- Aynı ADSync veritabanını paylaşan birden çok Azure AD Connect sunucusu olamaz. "Var olan veritabanını kullan" yöntemi, var olan bir ADSync veritabanını yeni bir Azure AD Connect sunucusuyla birlikte kullanmanıza olanak sağlar. Paylaşmayı desteklemez.

## <a name="steps-to-install-azure-ad-connect-with-use-existing-database-mode"></a>"Var olan veritabanını kullan" modunu kullanarak Azure AD Connect yüklemek için gereken adımlar
1.  Azure AD Connect yükleyicisi 'ni (AzureADConnect.MSI) Windows Server 'a indirin. Azure AD Connect yüklemeye başlamak için Azure AD Connect yükleyiciye çift tıklayın.
2.  MSI yüklemesi tamamlandıktan sonra Azure AD Connect sihirbazı Hızlı mod kurulumu açılır. Çıkış simgesine tıklayarak ekranı kapatın.
![Hoş geldiniz](./media/how-to-connect-install-existing-database/db1.png)
3.  Yeni bir komut istemi veya PowerShell oturumu başlatın. "C:\Program Files\Microsoft Azure Active Directory Connect" klasörüne gidin. Azure AD Connect sihirbazını "Var olan veritabanını kullan" modunda başlatmak için .\AzureADConnect.exe /useexistingdatabase komutunu çalıştırın.

> [!NOTE]
> Yalnızca veritabanı daha önceki bir Azure AD Connect yüklemesinden veri içerdiğinde **/Useexistingdatabase** anahtarını kullanın. Örneğin, yerel bir veritabanından tam SQL Server veritabanına geçiş yaparken veya Azure AD Connect sunucusu yeniden oluşturulduğunda ve ADSync veritabanının bir SQL yedeklemesini daha önceki bir Azure AD Connect yüklemesinden geri yükledikten sonra. Veritabanı boşsa, diğer bir deyişle, önceki Azure AD Connect yüklemesinden herhangi bir veri içermiyorsa, bu adımı atlayın.

![PowerShell](./media/how-to-connect-install-existing-database/db2.png)
1. Azure AD Connect'e Hoş Geldiniz ekranı açılır. Lisans koşullarını ve gizlilik bildirimini kabul ettikten sonra **Devam**'a tıklayın.
   ![Hoş geldiniz](./media/how-to-connect-install-existing-database/db3.png)
1. **Gerekli bileşenleri yükleme** ekranında **Mevcut bir SQL Server'ı kullanma** seçeneği etkinleştirilir. AD Eşitleme veritabanını barındıran SQL sunucusunun adını belirtin. AD Eşitleme veritabanını barındırmak için kullanılan SQL altyapısı örneği SQL sunucusundaki varsayılan örnek değilse SQL altyapısı örneği adını belirtmeniz gerekir. Ayrıca SQL göz atma özelliği etkin değilse SQL altyapısı örneği bağlantı noktası numarasını da belirtmeniz gerekir. Örneğin:         
   ![Hoş geldiniz](./media/how-to-connect-install-existing-database/db4.png)           

1. **Azure AD'ye bağlan** ekranında Azure AD dizininizin genel yöneticisinin kimlik bilgilerini girmeniz gerekir. Varsayılan onmicrosoft.com etki alanındaki bir hesabı kullanmanız önerilir. Bu hesap yalnızca Azure AD'de hizmet hesabı oluşturmak için kullanılır ve sihirbaz tamamlandıktan sonra kullanılmaz.
   ![Bağlanma](./media/how-to-connect-install-existing-database/db5.png)
 
1. **Connect your directories** (Dizinlerinize bağlanın) ekranında dizin eşitlemesi için yapılandırılmış olan mevcut AD ormanı yanında kırmızı bir çarpı işaretiyle listelenir. Şirket içi AD ormanında yapılan değişiklikleri eşitlemek için bir AD DS hesabı kullanmanız gerekir. Kimlik bilgileri şifrelenmiş olduğundan ve şifresi yalnızca önceki Azure AD Connect sunucusu tarafından çözülebildiğinden Azure AD Connect sihirbazı, AD Eşitleme veritabanında depolanan AD DS hesabının kimlik bilgilerini alamaz. AD ormanına ait AD DS hesabını belirtmek için **Change Credentials** (Kimlik Bilgilerini Değiştir) öğesine tıklayın.
   ![Dizinler](./media/how-to-connect-install-existing-database/db6.png)
 
1. Açılan iletişim kutusunda (i) Kuruluş Yöneticisi kimlik bilgileri belirtip Azure AD Connect'in sizin için bir AD DS hesabı oluşturmasını sağlayabilir veya (ii) AD DS hesabını kendiniz oluşturarak kimlik bilgilerini Azure AD Connect'e girebilirsiniz. Bir seçeneği belirleyip gerekli kimlik bilgilerini girdikten sonra **Tamam**'a tıklayarak açılan iletişim kutusunu kapatın.
   ![Hoş geldiniz](./media/how-to-connect-install-existing-database/db7.png)
 
1. Kimlik bilgileri girildikten sonra kırmızı çarpı işaretinin yerine yeşil onay işareti görünür. **İleri**’ye tıklayın.
   ![Hoş geldiniz](./media/how-to-connect-install-existing-database/db8.png)
 
1. **Yapılandırma için hazır** ekranında **Yükle**'ye tıklayın.
   ![Hoş geldiniz](./media/how-to-connect-install-existing-database/db9.png)
 
1. Yükleme tamamlandıktan sonra Azure AD Connect otomatik olarak Hazırlama Modunda etkinleştirilir. Hazırlama Modunu devre dışı bırakmadan önce sunucu yapılandırmasını ve bekleme durumundaki dışarı aktarma işlemlerini gözden geçirerek beklenmeyen değişikliklerin olup olmadığını kontrol etmeniz önerilir. 

## <a name="post-installation-tasks"></a>Yükleme sonrası görevler
1.2.65.0 ' den önceki bir Azure AD Connect sürümü tarafından oluşturulan bir veritabanı yedeklemesini geri yüklerken, hazırlama sunucusu otomatik olarak **yapılandırma**için bir oturum açma yöntemi seçer. Parola karması eşitleme ve parola geri yazma tercihleri geri yüklenecek olsa da, oturum açma yöntemini daha sonra etkin eşitleme sunucunuz için geçerli olan diğer ilkelerle eşleşecek şekilde değiştirmeniz gerekir.  Bu adımların tamamlanamaması, bu sunucunun etkin hale gelmesi için kullanıcıların oturum açmasını önleyebilir.  

Gerekli olan ek adımları doğrulamak için aşağıdaki tabloyu kullanın.

|Özellik|Adımlar|
|-----|-----|
|Parola karması eşitleme| Parola karması eşitleme ve parola geri yazma ayarları, 1.2.65.0 ile başlayan Azure AD Connect sürümleri için tamamen geri yüklenir.  Azure AD Connect eski bir sürümünü kullanarak geri yükleme işlemi, etkin eşitleme sunucunuza eşleştiğinden emin olmak için bu özellikler için eşitleme seçeneği ayarlarını gözden geçirin.  Başka yapılandırma adımları gerekli değildir.|
|AD FS ile Federasyon|Azure kimlik doğrulamaları, etkin eşitleme sunucunuz için yapılandırılmış AD FS ilkesini kullanmaya devam edecektir.  AD FS grubunuzu yönetmek için Azure AD Connect kullanıyorsanız, isteğe bağlı olarak, oturum açma yöntemini, etkin eşitleme örneği haline gelmelerine izin vermek üzere, bekleyen sunucunuzun hazırlanması AD FS Federasyon olarak değiştirebilirsiniz.   Etkin eşitleme sunucusunda cihaz seçenekleri etkinleştirilmişse, "cihaz seçeneklerini yapılandırma" görevini çalıştırarak bu sunucuda bu seçenekleri yapılandırın.|
|Geçişli kimlik doğrulaması ve Masaüstü çoklu oturum açma|Oturum açma yöntemini, etkin eşitleme sunucunuzdaki yapılandırmayla eşleşecek şekilde güncelleştirin.  Bu durum, sunucuyu birincil olarak yükseltmeden önce, sorunsuz çoklu oturum açma ile birlikte geçişli kimlik doğrulaması devre dışı bırakılır ve yedekleme oturum açma seçeneği olarak parola karması eşitleme seçeneği yoksa kiracınız kilitlenmiş olabilir. Ayrıca, hazırlama modunda geçiş kimlik doğrulamasını etkinleştirdiğinizde yeni bir kimlik doğrulama aracısının yükleneceğini, kaydedildiğini ve oturum açma isteklerini kabul edecek yüksek kullanılabilirliğe sahip bir aracı olarak çalışacağını unutmayın.|
|PingFederate ile federasyon|Azure kimlik doğrulamaları, etkin eşitleme sunucunuz için yapılandırılmış PingFederate ilkesini kullanmaya devam edecektir.  İsteğe bağlı olarak, bekleme sunucunuzun etkin eşitleme örneği haline gelmesini sağlamak için oturum açma yöntemini PingFederate olarak değiştirebilirsiniz.  Bu adım, PingFederate ile ek etki alanlarını federasyona kadar ertelenebilir.|

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD Connect'i yüklediniz, artık [yüklemeyi doğrulayabilir ve lisans atayabilirsiniz](how-to-connect-post-installation.md).
- Yüklemeyle etkinleştirilen özellikler hakkında daha fazla bilgi edinin: [Yanlışlıkla silmeleri engelleme](how-to-connect-sync-feature-prevent-accidental-deletes.md) ve [Azure AD Connect Health](how-to-connect-health-sync.md).
- Şu genel konu başlıkları hakkında daha fazla bilgi edinin: [Zamanlayıcı ve eşitleme tetikleme](how-to-connect-sync-feature-scheduler.md).
- [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
