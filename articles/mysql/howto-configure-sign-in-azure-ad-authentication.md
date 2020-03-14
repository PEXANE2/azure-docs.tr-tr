---
title: Azure Active Directory kullanma-MySQL için Azure veritabanı
description: MySQL için Azure veritabanı ile kimlik doğrulaması için Azure Active Directory (Azure AD) ayarlama hakkında bilgi edinin
author: lfittl-msft
ms.author: lufittl
ms.service: mysql
ms.topic: conceptual
ms.date: 01/22/2019
ms.openlocfilehash: 0403edadd491609c2c88d5b5ac6980d97163f8d6
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79299014"
---
# <a name="use-azure-active-directory-for-authenticating-with-mysql"></a>MySQL ile kimlik doğrulaması için Azure Active Directory kullanma

Bu makale, MySQL için Azure veritabanı ile Azure Active Directory erişimi yapılandırma ve Azure AD belirteci kullanarak bağlanma adımlarında size yol gösterecektir.

> [!IMPORTANT]
> MySQL için Azure veritabanı Azure AD kimlik doğrulaması şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="setting-the-azure-ad-admin-user"></a>Azure AD yönetici kullanıcısını ayarlama

Yalnızca bir Azure AD Yönetici kullanıcısı, kullanıcıları Azure AD tabanlı kimlik doğrulaması için oluşturabilir/etkinleştirebilir. Azure AD Yönetici kullanıcısı oluşturmak için lütfen aşağıdaki adımları izleyin

1. Azure portal Azure AD için etkinleştirmek istediğiniz MySQL için Azure veritabanı örneğini seçin.
2. Ayarlar altında yönetici Active Directory ' yi seçin:

![Azure AD yöneticisini ayarlama][2]

3. Müşteri kiracısında Azure AD yöneticisi olacak geçerli bir Azure AD kullanıcısı seçin.

> [!IMPORTANT]
> Yönetici ayarlanırken, MySQL için Azure veritabanı sunucusuna tam yönetici izinleriyle yeni bir Kullanıcı eklenir.

Her MySQL sunucusu için yalnızca bir Azure AD yöneticisi oluşturulabilir ve başka bir tane seçilebilir, sunucu için yapılandırılmış mevcut Azure AD yöneticisinin üzerine yazılır.

Gelecekteki bir sürümde, tek bir kullanıcı yerine birden çok yönetici olacak şekilde bir Azure AD grubu belirtmeyi destekliyoruz, ancak şu anda henüz desteklenmiyor.

Yönetici yapılandırıldıktan sonra artık oturum açabilirsiniz:

## <a name="connecting-to-azure-database-for-mysql-using-azure-ad"></a>Azure AD kullanarak MySQL için Azure veritabanı 'na bağlanma

Aşağıdaki üst düzey diyagram MySQL için Azure veritabanı ile Azure AD kimlik doğrulaması kullanma iş akışını özetler:

![Kimlik doğrulama akışı][1]

Azure AD tümleştirmesini, Azure AD Aware olmayan ve yalnızca MySQL 'e bağlanırken Kullanıcı adı ve parola belirtmeyi destekleyen MySQL CLı gibi ortak MySQL araçlarıyla çalışacak şekilde tasarladık. Yukarıdaki resimde gösterildiği gibi Azure AD belirtecini parola olarak geçiyoruz.

Şu anda aşağıdaki istemcileri test ettik:

- Mysqlçalışma ekranı 
- MySQL CLı

Ayrıca, en yaygın uygulama sürücülerini test ettik, bu sayfanın sonundaki ayrıntıları görebilirsiniz.

Bu adımlar, bir kullanıcı/uygulamanın, aşağıda açıklanan Azure AD ile kimlik doğrulaması yapmak için gereken adımlardır:

### <a name="step-1-authenticate-with-azure-ad"></a>1\. Adım: Azure AD ile kimlik doğrulama

[Azure CLI 'nin yüklü](/cli/azure/install-azure-cli)olduğundan emin olun.

Azure AD ile kimlik doğrulaması gerçekleştirmek için Azure CLı aracını çağırın. Azure AD Kullanıcı KIMLIĞINIZ ve parolanızı sağlamanız gerekir.

```
az login
```

Bu komut, Azure AD kimlik doğrulama sayfasında bir tarayıcı penceresi başlatır.

> [!NOTE]
> Bu adımları gerçekleştirmek için Azure Cloud Shell de kullanabilirsiniz.
> Lütfen Azure Cloud Shell Azure AD erişim belirtecini alırken `az login` açıkça çağırmanız ve yeniden oturum açmanız gerektiğini unutmayın (bir kod ile ayrı pencerede). `get-access-token` komutunda bu oturum açma, beklendiği gibi çalışır.

### <a name="step-2-retrieve-azure-ad-access-token"></a>2\. Adım: Azure AD erişim belirtecini alma

Azure AD kimliği doğrulanmış kullanıcı için adım 1 ' den MySQL için Azure veritabanı 'na erişim için bir erişim belirteci almak üzere Azure CLı aracını çağırın.

Örnek (genel bulut için):

```shell
az account get-access-token --resource https://ossrdbms-aad.database.windows.net
```

Yukarıdaki kaynak değeri tam olarak gösterildiği gibi belirtilmelidir. Diğer bulutlarda, kaynak değeri şu kullanılarak aranabilir:

```shell
az cloud show
```

Azure CLı sürüm 2.0.71 ve üzeri için, komut tüm bulutlar için aşağıdaki daha uygun sürümde belirtilebilir:

```shell
az account get-access-token --resource-type oss-rdbms
```

Kimlik doğrulaması başarılı olduktan sonra Azure AD, bir erişim belirteci döndürür:

```json
{
  "accessToken": "TOKEN",
  "expiresOn": "...",
  "subscription": "...",
  "tenant": "...",
  "tokenType": "Bearer"
}
```

Belirteç, kimliği doğrulanmış kullanıcıyla ilgili tüm bilgileri kodlayan ve MySQL için Azure veritabanı hizmetine hedeflenen bir temel 64 dizesidir.

> [!NOTE]
> Erişim belirteci geçerliliği, 5 dakikadan 60 dakika arasında bir süre sürer. MySQL için Azure veritabanı 'nda oturum açmayı başlatmadan önce erişim belirtecini almanızı öneririz.

### <a name="step-3-use-token-as-password-for-logging-in-with-mysql"></a>3\. Adım: MySQL ile oturum açmak için belirteci parola olarak kullanın

Bağlantı sırasında MySQL Kullanıcı parolası olarak erişim belirtecini kullanmanız gerekir. Mysqlme gibi GUI istemcilerini kullanırken, belirteci almak için yukarıdaki yöntemi kullanabilirsiniz. 

CLı kullanırken, bu kısa süreli bağlantı kurmak için şunu kullanabilirsiniz: 

**Örnek (Linux/macOS):**
```
mysql -h mydb.mysql.database.azure.com \ 
  --user user@tenant.onmicrosoft.com@mydb \ 
  --enable-cleartext-plugin \ 
  --password=`az account get-access-token --resource-type oss-rdbms --output tsv --query accessToken`
```

"Enable-şifresiz-Plugin" ayarını unutmayın. belirtecin karma kalmadan sunucuya gönderilmesini sağlamak için diğer istemcilerle benzer bir yapılandırma kullanmanız gerekir.

Artık Azure AD kimlik doğrulaması kullanarak MySQL sunucunuza kimliğiniz doğrulanır.

## <a name="creating-azure-ad-users-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda Azure AD kullanıcıları oluşturma

MySQL veritabanı için Azure veritabanı 'na bir Azure AD kullanıcısı eklemek için, bağlandıktan sonra aşağıdaki adımları gerçekleştirin (bkz. bağlanma hakkında sonraki bölüm):

1. İlk olarak Azure AD Kullanıcı `<user>@yourtenant.onmicrosoft.com` Azure AD kiracısında geçerli bir kullanıcı olduğundan emin olun.
2. MySQL için Azure veritabanı örneğinde Azure AD Yönetici kullanıcısı olarak oturum açın.
3. MySQL için Azure veritabanı 'nda Kullanıcı `<user>@yourtenant.onmicrosoft.com` oluşturun.

**Örnek:**

```sql
CREATE AADUSER 'user1@yourtenant.onmicrosoft.com';
```

32 karakteri aşan Kullanıcı adları için, bağlantı sırasında kullanılmak üzere bunun yerine bir diğer ad kullanmanız önerilir: 

Örnek:

```sql
CREATE AADUSER 'userWithLongName@yourtenant.onmicrosoft.com' as 'userDefinedShortName'; 
```

> [!NOTE]
> Azure AD aracılığıyla bir kullanıcının kimliğini doğrulamak, kullanıcıya MySQL için Azure veritabanı veritabanı içindeki nesnelere erişim izni vermez. Kullanıcıya gerekli izinleri el ile vermeniz gerekir.

## <a name="creating-azure-ad-groups-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda Azure AD grupları oluşturma

Veritabanınıza erişim için bir Azure AD grubunu etkinleştirmek üzere, kullanıcılar için aynı mekanizmayı kullanın, bunun yerine grup adını belirtin:

**Örnek:**

```sql
CREATE AADUSER 'Prod_DB_Readonly';
```

Oturum açarken, grubun üyeleri kendi kişisel erişim belirteçlerini kullanır, ancak Kullanıcı adı olarak belirtilen grup adıyla oturum açılır.

## <a name="token-validation"></a>Belirteç doğrulama

MySQL için Azure veritabanı 'nda Azure AD kimlik doğrulaması, kullanıcının MySQL sunucusunda mevcut olmasını sağlar ve belirtecin içeriğini doğrulayarak belirtecin geçerliliğini denetler. Aşağıdaki belirteç doğrulama adımları gerçekleştirilir:

-   Belirteç Azure AD tarafından imzalanmış ve bu değişiklik yapılmamıştır
-   Belirteç, sunucuyla ilişkili kiracı için Azure AD tarafından verildi
-   Belirtecin süresi sona ermedi
-   Belirteç, MySQL için Azure veritabanı kaynağına yöneliktir (başka bir Azure kaynağı değil)

## <a name="compatibility-with-application-drivers"></a>Uygulama sürücüleriyle uyumluluk

Çoğu sürücü desteklenir, ancak belirtecin değişiklik yapılmadan gönderilmesi için parolayı düz metin olarak gönderme ayarlarını kullandığınızdan emin olun.

* ,C++
  * libmysqlclient: destekleniyor
  * MySQL-bağlayıcı-c + +: desteklenir
* Java
  * Bağlayıcı/J (MySQL-Connector-Java): desteklenir, `useSSL` ayarı kullanılmalıdır
* Python
  * Bağlayıcı/Python: destekleniyor
* Ruby
  * mysql2: destekleniyor
* .NET
  * MySQL-Connector-net: desteklenen, mysql_clear_password için eklenti eklemesi gerekiyor
  * MySQL-net/MySqlConnector: destekleniyor
* Node.js
  * mysqljs: desteklenmez (düzeltme eki olmadan şifresiz metin olarak bir belirteç göndermez)
  * Node-mysql2: destekleniyor
* Perl
  * DBD:: MySQL: destekleniyor
  * Net:: MySQL: desteklenmiyor
* Başlayın
  * Go-SQL-Driver: desteklenir, bağlantı dizesine `?tls=true&allowCleartextPasswords=true` ekleyin

## <a name="next-steps"></a>Sonraki adımlar

* [MySQL Için Azure veritabanı ile Azure Active Directory kimlik doğrulaması](concepts-azure-ad-authentication.md) için genel kavramları gözden geçirin

<!--Image references-->

[1]: ./media/concepts-azure-ad-authentication/authentication-flow.png
[2]: ./media/concepts-azure-ad-authentication/set-azure-ad-admin.png
