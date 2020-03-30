---
title: MySQL için Azure Active Directory - Azure Veritabanı'nı kullanma
description: MySQL için Azure Veritabanı ile kimlik doğrulama için Azure Active Directory'yi (Azure AD) nasıl ayarlayabilirsiniz hakkında bilgi edinin
author: lfittl-msft
ms.author: lufittl
ms.service: mysql
ms.topic: conceptual
ms.date: 01/22/2019
ms.openlocfilehash: 0403edadd491609c2c88d5b5ac6980d97163f8d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79299014"
---
# <a name="use-azure-active-directory-for-authenticating-with-mysql"></a>MySQL ile kimlik doğrulaması için Azure Etkin Dizini'ni kullanma

Bu makale, MySQL için Azure Active Directory erişimini nasıl yapılandıracağınız ve Azure AD belirteci kullanarak nasıl bağlanabileceğiniz adımlarını gözden geçirecektir.

> [!IMPORTANT]
> MySQL için Azure Veritabanı için Azure AD kimlik doğrulaması şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="setting-the-azure-ad-admin-user"></a>Azure AD Yöneticisi kullanıcısını ayarlama

Yalnızca bir Azure AD Yöneticisi kullanıcısı, Azure AD tabanlı kimlik doğrulaması için kullanıcılar oluşturabilir/etkinleştirebilir. Oluşturmak ve Azure AD Yöneticisi kullanıcısını oluşturmak için lütfen aşağıdaki adımları izleyin

1. Azure portalında, Azure AD için etkinleştirmek istediğiniz MySQL için Azure Veritabanı örneğini seçin.
2. Ayarlar altında Active Directory Admin'i seçin:

![azure reklam yöneticisi ayarlama][2]

3. Azure AD yöneticisi olmak için müşteri kiracısında geçerli bir Azure AD kullanıcısı seçin.

> [!IMPORTANT]
> Yöneticiyi ayarlarken, tam yönetici izinleriyle MySQL sunucusu için Azure Veritabanına yeni bir kullanıcı eklenir.

MySQL sunucusu başına yalnızca bir Azure AD yöneticisi oluşturulabilir ve başka bir yöneticinin seçimi sunucu için yapılandırılan mevcut Azure AD yöneticisinin üzerine yazar.

İleride yayınlanacak bir sürümde, birden fazla yöneticiye sahip olmak için tek bir kullanıcı yerine bir Azure REKLAM grubu belirtmeyi destekleyeceğiz, ancak bu henüz desteklenmedi.

Yöneticiyi yapılandırdıktan sonra artık oturum açabilirsiniz:

## <a name="connecting-to-azure-database-for-mysql-using-azure-ad"></a>Azure AD'yi kullanarak MySQL için Azure Veritabanına bağlanma

Aşağıdaki üst düzey diyagram, MySQL için Azure Veritabanı ile Azure AD kimlik doğrulamasını kullanmanın iş akışını özetler:

![kimlik doğrulama akışı][1]

Azure AD tümleştirmesini, Azure AD farkında olmayan ve yalnızca MySQL'e bağlanırken kullanıcı adı ve parola belirtmeyi destekleyen mysql CLI gibi ortak MySQL araçlarıyla çalışacak şekilde tasarladık. Yukarıdaki resimde gösterildiği gibi Azure AD belirteci'ni parola olarak geçiyoruz.

Şu anda aşağıdaki istemcileri test ettik:

- MySQLWorkbench 
- Mysql CLI

Biz de en yaygın uygulama sürücüleri test ettik, bu sayfanın sonunda ayrıntıları görebilirsiniz.

Aşağıda açıklanan Azure AD ile bir kullanıcının/uygulamanın kimlik doğrulaması yapması gereken adımlar şunlardır:

### <a name="step-1-authenticate-with-azure-ad"></a>Adım 1: Azure AD ile kimlik doğrulaması

[Azure CLI'nin yüklü](/cli/azure/install-azure-cli)olduğundan emin olun.

Azure AD ile kimlik doğrulaması yapmak için Azure CLI aracını çağırın. Azure AD kullanıcı kimliğinizi ve parolanızı vermenizi gerektirir.

```
az login
```

Bu komut, Azure AD kimlik doğrulama sayfasına bir tarayıcı penceresi başlatacaktır.

> [!NOTE]
> Bu adımları gerçekleştirmek için Azure Bulut Su Toplarını da kullanabilirsiniz.
> Azure Bulut Su Ağıtı'nda Azure AD erişim jetonunu alırken açıkça `az login` aramanız ve yeniden oturum açmanız gerekeceğini lütfen unutmayın (kodlu ayrı pencerede). Bu işaretten `get-access-token` sonra komut beklendiği gibi çalışacaktır.

### <a name="step-2-retrieve-azure-ad-access-token"></a>Adım 2: Azure AD erişim jetonunu alın

MySQL için Azure Veritabanı'na erişmek için 1 adımdan Azure AD kimlik doğrulaması yapılan kullanıcıiçin bir erişim jetonu edinmek için Azure CLI aracını çağırın.

Örnek (Genel Bulut için):

```shell
az account get-access-token --resource https://ossrdbms-aad.database.windows.net
```

Yukarıdaki kaynak değeri tam olarak gösterildiği gibi belirtilmelidir. Diğer bulutlar için kaynak değeri aşağıdakileri kullanarak aranabilir:

```shell
az cloud show
```

Azure CLI sürüm 2.0.71 ve sonrası için komut tüm bulutlar için aşağıdaki daha kullanışlı sürümde belirtilebilir:

```shell
az account get-access-token --resource-type oss-rdbms
```

Kimlik doğrulama başarılı olduktan sonra, Azure AD bir erişim jetonu döndürecek:

```json
{
  "accessToken": "TOKEN",
  "expiresOn": "...",
  "subscription": "...",
  "tenant": "...",
  "tokenType": "Bearer"
}
```

Belirteç, kimliği doğrulanan kullanıcı hakkındaki tüm bilgileri kodlayan ve MySQL hizmeti için Azure Veritabanı'nı hedefleyen bir Base 64 dizesidir.

> [!NOTE]
> Erişim belirteci geçerliliği 5 dakika ile 60 dakika arasındadır. MySQL için Azure Veritabanı'na giriş başlatmadan hemen önce erişim jetonuna ulaşmanızı öneririz.

### <a name="step-3-use-token-as-password-for-logging-in-with-mysql"></a>Adım 3: MySQL ile oturum açmak için parola olarak belirteç kullanın

Bağlanırken MySQL kullanıcı parolası olarak erişim belirteci kullanmanız gerekir. MySQLWorkbench gibi GUI istemcilerini kullanırken, belirteci almak için yukarıdaki yöntemi kullanabilirsiniz. 

CLI'yi kullanırken, bağlanmak için bu kısa eli kullanabilirsiniz: 

**Örnek (Linux/macOS):**
```
mysql -h mydb.mysql.database.azure.com \ 
  --user user@tenant.onmicrosoft.com@mydb \ 
  --enable-cleartext-plugin \ 
  --password=`az account get-access-token --resource-type oss-rdbms --output tsv --query accessToken`
```

"Metin eklentisini etkinleştirme" ayarına dikkat edin – belirteçlerin işlenmeden sunucuya gönderilmesini sağlamak için diğer istemcilerle benzer bir yapılandırma kullanmanız gerekir.

Azure AD kimlik doğrulamasını kullanarak MySQL sunucunuza kimlik doğrulaması yapılır.

## <a name="creating-azure-ad-users-in-azure-database-for-mysql"></a>MySQL için Azure Veritabanı'nda Azure AD kullanıcıları oluşturma

MySQL veritabanınız için Azure Veritabanınıza bir Azure REKLAM kullanıcısı eklemek için bağlandıktan sonra aşağıdaki adımları gerçekleştirin (nasıl bağlanılabağlanınız la ilgili sonraki bölüme bakın):

1. Öncelikle Azure AD kullanıcısının `<user>@yourtenant.onmicrosoft.com` Azure AD kiracısında geçerli bir kullanıcı olduğundan emin olun.
2. Azure AD Yöneticisi kullanıcısı olarak MySQL örneği için Azure Veritabanınızda oturum açın.
3. MySQL `<user>@yourtenant.onmicrosoft.com` için Azure Veritabanı'nda kullanıcı oluşturun.

**Örnek:**

```sql
CREATE AADUSER 'user1@yourtenant.onmicrosoft.com';
```

32 karakteri aşan kullanıcı adları için, bağlanırken kullanılmak üzere bir takma ad kullanmanız önerilir: 

Örnek:

```sql
CREATE AADUSER 'userWithLongName@yourtenant.onmicrosoft.com' as 'userDefinedShortName'; 
```

> [!NOTE]
> Azure AD aracılığıyla bir kullanıcının kimliğini doğrulamak, kullanıcıya MySQL veritabanı için Azure Veritabanı'ndaki nesnelere erişme izni vermez. Kullanıcıya gerekli izinleri el ile vermelisiniz.

## <a name="creating-azure-ad-groups-in-azure-database-for-mysql"></a>MySQL için Azure Veritabanı'nda Azure REKLAM grupları oluşturma

Veritabanınıza erişmek için bir Azure REKLAM grubunu etkinleştirmek için, kullanıcılarla aynı mekanizmayı kullanın, ancak bunun yerine grup adını belirtin:

**Örnek:**

```sql
CREATE AADUSER 'Prod_DB_Readonly';
```

Oturum açarken, grup üyeleri kişisel erişim belirteçlerini kullanır, ancak kullanıcı adı olarak belirtilen grup adı ile imzalar.

## <a name="token-validation"></a>Belirteç Doğrulama

MySQL için Azure Veritabanı'ndaki Azure AD kimlik doğrulaması, kullanıcının MySQL sunucusunda bulunmasını sağlar ve belirteç içeriğini doğrulayarak belirteci geçerliliğini denetler. Aşağıdaki belirteç doğrulama adımları gerçekleştirilir:

-   Belirteç Azure AD tarafından imzalanmıştır ve kurcalanmamıştır
-   Belirteç, sunucuyla ilişkili kiracı için Azure AD tarafından verildi
-   Belirteci'nin süresi dolmadı
-   Belirteç, MySQL kaynağı için Azure Veritabanı içindir (başka bir Azure kaynağı için değil)

## <a name="compatibility-with-application-drivers"></a>Uygulama sürücüleri ile uyumluluk

Çoğu sürücü desteklenir, ancak parolayı açık metin olarak göndermek için ayarları kullandığınızdan emin olun, böylece belirteç değiştirilmeden gönderilir.

* C/C++
  * libmysqlclient: Desteklenen
  * mysql-konektör-c++: Desteklenen
* Java
  * Konektör/J (mysql-konektör-java): Desteklenen, `useSSL` ayarı kullanmalıdır
* Python
  * Bağlayıcı/Python: Desteklenen
* Ruby
  * mysql2: Desteklenen
* .NET
  * mysql-connector-net: Desteklenen, mysql_clear_password için eklenti eklemek gerekir
  * mysql-net/MySqlConnector: Desteklenen
* Node.js
  * mysqljs: Desteklenmez (yama olmadan açık metin de belirteç göndermez)
  * düğüm-mysql2: Desteklenen
* Perl
  * DBD::mysql: Desteklenen
  * Net::MySQL: Desteklenmiyor
* Başlayın
  * go-sql-driver: Desteklenen, `?tls=true&allowCleartextPasswords=true` bağlantı dizesine ekle

## <a name="next-steps"></a>Sonraki adımlar

* [MySQL için Azure Veritabanı ile Azure Active Directory kimlik doğrulaması için](concepts-azure-ad-authentication.md) genel kavramları gözden geçirin

<!--Image references-->

[1]: ./media/concepts-azure-ad-authentication/authentication-flow.png
[2]: ./media/concepts-azure-ad-authentication/set-azure-ad-admin.png
