---
title: Azure AD Connect yapılandırma ayarlarını içeri ve dışarı aktarma
description: Bu belgede, bulut sağlaması için sık sorulan sorular açıklanmaktadır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 07/13/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a94d356cb3c0345f575b4b5a44aa7f228535e66d
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87019888"
---
# <a name="importing-and-exporting-azure-ad-connect-configuration-settings-public-preview"></a>Azure AD Connect yapılandırma ayarlarını içeri ve dışarı aktarma (Genel Önizleme) 

Azure AD Connect dağıtımları, tek bir orman hızlı modundan yüklemeden, özel eşitleme kuralları kullanılarak birden çok orman genelinde eşitleme yapan karmaşık dağıtımlara farklılık gösterir. Çok sayıda yapılandırma seçeneği ve mekanizması nedeniyle, hangi ayarların geçerli olduğunu anlamak ve aynı yapılandırmaya sahip bir sunucuyu hızlı bir şekilde dağıtmak için gereklidir. Bu özellik, belirli bir eşitleme sunucusunun yapılandırmasını katalog haline getirme ve ayarları yeni bir dağıtıma aktarma özelliğini sunar. Farklı eşitleme ayarları anlık görüntüleri, iki sunucu veya zaman içinde aynı sunucu arasındaki farkları kolayca görselleştirebileceğiniz şekilde karşılaştırılabilir. 

Yapılandırma Azure AD Connect sihirbazından her değiştirildiğinde, yeni zaman damgalı JSON ayarları dosyası **%ProgramData%\AADConnect**' e otomatik olarak verilir. Ayarlar dosya adı, **uygulanan-SynchronizationPolicy-* biçimindedir. **Dosya adının son bölümünün zaman damgası olduğu JSON. 

> [!IMPORTANT]
> Yalnızca Azure AD Connect tarafından yapılan değişiklikler otomatik olarak verilir. Güncel bir kopyayı sürdürmek için gerektiğinde PowerShell, Synchronization Service Manager veya eşitleme kuralları Düzenleyicisi kullanılarak yapılan tüm değişiklikler isteğe bağlı olarak verilmelidir. İsteğe bağlı dışarı aktarma, bir ayarların bir kopyasını olağanüstü durum kurtarma amacıyla güvenli bir konuma yerleştirmek için de kullanılabilir. 

## <a name="exporting-azure-ad-connect-settings"></a>Azure AD Connect ayarları dışarı aktarılıyor 

Yapılandırma ayarlarınızın bir özetini görüntülemek için Azure AD Connect aracını açın ve şu adlı ek görevi seçin: geçerli yapılandırmayı görüntüle veya dışarı aktar. Ayarlarınızın hızlı bir özeti, sunucunuzun tam yapılandırmasını dışarı aktarma özelliğiyle birlikte gösterilir. 

Ayarlar varsayılan olarak **%ProgramData%\AADConnect**' e aktarılacak, ancak bir olağanüstü durum durumunda kullanılabilirliği sağlamak için ayarları korumalı bir konuma kaydetmeyi tercih edebilirsiniz. Ayarlar JSON dosya biçimi kullanılarak verilir ve mantıksal tutarlılığı sağlamak için el oluşturulmamalıdır veya düzenlenmemelidir. Oluşturulan veya düzenlenen dosyanın içe aktarılması desteklenmez ve beklenmeyen sonuçlara neden olabilir. 

## <a name="importing-azure-ad-connect-settings"></a>Azure AD Connect ayarları içeri aktarılıyor 

Daha önce dışarı aktarılan ayarları içeri aktarmak için aşağıdakileri yapın:
 
1. **Azure AD Connect** yeni bir sunucuya yükler. 
2. **Hoş geldiniz** sayfasından sonra **Özelleştir** seçeneğini belirleyin. 
3. **Eşitleme ayarlarını Içeri aktar**' a tıklayın. Daha önce aktarılmış JSON ayarları dosyasına gözatamazsınız.  
4. **Yükle**'ye tıklayın.

![Bileşenleri yükler](media/how-to-connect-import-export-config/import1.png)

> [!NOTE]
> Lütfen bu sayfadaki ayarları, LocalDB yerine SQL Server kullanımı veya varsayılan VSA yerine mevcut hizmet hesabı kullanımı gibi geçersiz kılın. Bu ayarlar yapılandırma ayarları dosyasından içeri aktarılmaz, ancak bilgi ve karşılaştırma amaçlarıyla bulunur.

### <a name="import-installation-experience"></a>Yükleme deneyimini içeri aktar 

Yükleme deneyimini içeri aktarma işlemi, kullanıcının var olan bir sunucunun reproducibility kolayca sağlanması için en az sayıda girişe sahip olarak basit bir şekilde depolanır.  

Yükleme deneyimi sırasında yapılabilecek tek değişiklikler aşağıda verilmiştir. Diğer tüm değişiklikler Azure AD Connect sihirbazından yüklemeden sonra yapılabilir.: 
- **Azure Active Directory kimlik bilgileri**   – özgün sunucuyu yapılandırmak için kullanılan Azure genel yöneticisi hesap adı varsayılan olarak **önerilir ve**   bilgileri yeni bir dizinle eşleştirmek istiyorsanız değiştirilmelidir.
- **Kullanıcı oturumu açma**   – özgün sunucunuz için yapılandırılmış oturum açma seçenekleri varsayılan olarak seçilidir ve kimlik bilgilerini veya yapılandırma sırasında gereken diğer bilgileri otomatik olarak ister. Nadir durumlarda, etkin sunucu 'nun davranışını değiştirmekten kaçınmak için farklı seçeneklere sahip bir sunucu ayarlamanız gerekebilir. Aksi takdirde, aynı ayarları kullanmak için Ileri 'ye basın. 
- **Şirket Içi Dizin kimlik bilgileri**   – Eşitleme ayarlarınıza dahil edilen her şirket içi dizin için, bir eşitleme hesabı oluşturmak veya önceden oluşturulmuş bir özel eşitleme hesabı sağlamak üzere kimlik bilgilerini sağlamalısınız. Bu yordam, Dizin ekleyemez veya kaldırabilmeniz için özel durum ile temiz yüklemesi deneyimiyle aynıdır. 
- **Yapılandırma seçenekleri**   – temiz bir yüklemede olduğu gibi, otomatik eşitlemenin başlatılıp başlatılmayacağını veya hazırlama modunun etkinleştirilmesi için başlangıç ayarlarını yapılandırmayı tercih edebilirsiniz. Temel fark, hazırlama modunun, sonuçları Azure 'a etkin bir şekilde dışarı aktarmadan önce yapılandırma ve eşitleme sonuçlarının karşılaştırmasına izin vermek için, varsayılan olarak kasıtlı olarak etkinleştirilmiştir. 

![Dizinlerinizi bağlama](media/how-to-connect-import-export-config/import2.png)

> [!NOTE]
> Birincil rolde yalnızca bir eşitleme sunucusu olabilir ve yapılandırma değişiklikleri Azure 'a etkin bir şekilde dışarı aktarabilirsiniz. Diğer tüm sunucuların hazırlama modunda yerleştirilmesi gerekir. 

## <a name="migrating-settings-from-an-existing-server"></a>Ayarları var olan bir sunucudan geçirme 

Var olan bir sunucu ayarlar yönetimini desteklemiyorsa, sunucuyu yerinde yükseltmeyi veya yeni bir hazırlama sunucusunda kullanılacak ayarları geçirmeyi tercih edebilirsiniz.  

Geçiş için, Yeni yüklemede kullanılmak üzere var olan ayarları çıkaran bir PowerShell betiği çalıştırması gerekir.Bu yöntem, var olan sunucunuzun ayarlarını kataloglamak için önerilir ve sonra bunları yeni yüklenen bir hazırlama sunucusuna uygular.Özgün sunucunun ayarlarını yeni oluşturulan sunucuyla karşılaştırmak, sunucular arasındaki değişiklikleri hızlıca görselleşmesini sağlar.Her zaman olduğu gibi, ek bir yapılandırmanın gerekli olmamasını sağlamak için kuruluşunuzun sertifika sürecini izleyin.  

### <a name="migration-process"></a>Geçiş Işlemi 
Ayarları geçirmek için aşağıdakileri yapın:

1. Yeni hazırlama sunucusunda **AzureADConnect.msi** başlatın ve Azure AD Connect hoş geldiniz sayfasından durun.

2. Microsoft Azure AD Connect\Tools dizininden **MigrateSettings.ps1** var olan sunucudaki bir konuma kopyalayın.  Örneğin, C:\setup.  Burada kurulum, var olan sunucuda oluşturulmuş bir dizindir.

   ![Dizinlerinizi bağlama](media/how-to-connect-import-export-config/migrate1.png)

3. Aşağıda gösterildiği gibi betiği çalıştırın ve tüm alt düzey sunucu yapılandırma dizinini kaydedin. Bu dizini yeni hazırlama sunucusuna kopyalayın. Lütfen tüm **aktarılmış-ServerConfiguration-*** klasörünü yeni sunucuya kopyalamanız gerektiğini unutmayın.

   ![Bağlantı dizinleri bağlantı ](media/how-to-connect-import-export-config/migrate2.png)
    ![ dizinleri](media/how-to-connect-import-export-config/migrate3.png)

5. Masaüstündeki simgeye çift tıklayarak **Azure AD Connect** başlatın. EULA 'Yı kabul et, sonraki sayfada **Özelleştir** düğmesine tıklayın. 
6. **Eşitleme ayarlarını Içeri aktar** onay kutusunu Işaretleyin ve dışarı aktarılan-ServerConfiguration-* klasörüne kopyalamak için **Gözden** geçir düğmesine tıklayın ve geçirilen ayarları içeri aktarmak için MigratedPolicy.jsseçin.

   ![Dizinlerinizi bağlama](media/how-to-connect-import-export-config/migrate4.png)

7. Geçirilen ayarları uygulanan ayarların ile karşılaştırmak için, en son **geçirilmekte olan-SynchronizationPolicy-* ' i arayın. JSON** ve **uygulandı-synchronizationpolicy-*. JSON** (* zaman damgasıdır), **%ProgramData%\AADConnect**altında. Eşlik ' i karşılaştırmak için en sevdiğiniz dosya karşılaştırma aracını kullanın. 

## <a name="post-installation-verification"></a>Yükleme sonrası doğrulama 

İlk olarak içeri aktarılan ayarlar dosyası, verilen ayarlar dosyası ile karşılaştırıldığında, yeni dağıtılan sunucu, amaçlanan dağıtım ve sonuçta elde edilen dağıtım arasındaki farkları anlamak için önemli bir adımdır. En sevdiğiniz yan yana metin karşılaştırma uygulamanızı kullanmak, istenen veya yanlışlıkla yapılan değişiklikleri hızlıca vurgulayan bir anlık görselleştirme oluşturur. Daha önce el ile yapılan birçok yapılandırma adımı ortadan kalkmış olsa da, ek bir yapılandırmanın gerekli olmamasını sağlamak için kuruluşunuzun sertifika sürecini yine de izlemeniz gerekir. Bu yapılandırma, şu anda ayarlar yönetimi 'nin genel önizleme sürümünde yakalanmayan gelişmiş ayarlardan yararlandıysanız meydana gelebilir. 

Bilinen Sınırlamalar şunları içerir: 
- **Eşitleme kuralları**   – Microsoft 'un standart kurallarıyla çakışmaları önlemek için özel bir kuralın önceliği 0-99 ayrılmış aralığında olmalıdır. Özel bir kuralın ayrılmış aralığın dışına yerleştirilmesi, yapılandırmaya standart kurallar eklendikçe özel kuralınızın etrafında kaydırılmasıyla sonuçlanabilir. Yapılandırmanız değiştirilmiş standart kuralları içeriyorsa, benzer bir sorun oluşur. Standart bir kuralı değiştirmenin kesinlikle önerilmez ve kural yerleşimi büyük olasılıkla yanlış olabilir. 
- **Cihaz geri yazma**   – Bu ayarlar kataloglandığında, ancak şu anda yapılandırma sırasında uygulanmaz. Özgün sunucunuz için cihaz geri yazma etkinleştirildiyse, özelliği yeni dağıtılan sunucuda el ile yapılandırmanız gerekir. 
- **Eşitlenmiş nesne türleri**   – Synchronization Service Manager kullanarak eşitlenmiş nesne türleri (kullanıcılar, kişiler, gruplar, vb.) listesini kısıtlamak mümkün olsa da, bu özellik şu anda eşitleme ayarları aracılığıyla desteklenmez. Yüklemeyi tamamladıktan sonra, gelişmiş yapılandırmayı el ile yeniden uygulamalısınız. 
- **Özel çalıştırma profilleri**   -Synchronization Service Manager kullanarak varsayılan çalıştırma profili kümesini değiştirmek mümkün olsa da, bu özellik şu anda eşitleme ayarları aracılığıyla desteklenmez. Yüklemeyi tamamladıktan sonra, gelişmiş yapılandırmayı el ile yeniden uygulamalısınız. 
- **Sağlama hiyerarşisini yapılandırma**   – Synchronization Service Manager Bu gelişmiş özelliği eşitleme ayarları aracılığıyla desteklenmez ve ilk dağıtımı tamamladıktan sonra el ile yeniden yapılandırılması gerekir. 
- **AD FS ve PingFederate kimlik doğrulaması**   – Bu kimlik doğrulama özellikleriyle ilişkili oturum açma yöntemleri otomatik olarak önceden seçilir, ancak diğer tüm gerekli yapılandırma parametrelerini etkileşimli olarak sağlamanız gerekir. 
- **Devre dışı bir özel eşitleme kuralı, etkin olarak içeri aktarılacak** Devre dışı bir özel eşitleme kuralı, etkin olarak içeri aktarılır. Bunu yeni sunucu üzerinde de devre dışı bıraktığınızdan emin olun.

 ## <a name="next-steps"></a>Sonraki Adımlar

- [Donanım ve önkoşullar](how-to-connect-install-prerequisites.md) 
- [Hızlı ayarlar](how-to-connect-install-express.md)
- [Özelleştirilmiş ayarlar](how-to-connect-install-custom.md)
- [Azure AD Connect Health aracılarını yükleme](how-to-connect-health-agent-install.md) 
