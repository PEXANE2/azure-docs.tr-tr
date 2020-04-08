---
title: PostgreSQL için Azure Active Directory - Azure Veritabanı 'nı kullanın - Tek Sunucu
description: PostgreSQL - Single Server için Azure Veritabanı ile kimlik doğrulama için Azure Active Directory 'i (AAD) nasıl ayarlayabilirsiniz hakkında bilgi edinin
author: lfittl
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: f5588503825281f407ddbbc2c1c57cd94a9c7ee6
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804716"
---
# <a name="use-azure-active-directory-for-authenticating-with-postgresql"></a>PostgreSQL ile kimlik doğrulaması için Azure Etkin Dizini'ni kullanın

Bu makale, PostgreSQL için Azure Veritabanı ile Azure Active Directory erişimini nasıl yapılandıracağınız ve Azure AD belirteci kullanarak nasıl bağlanabileceğiniz adımlarını gözden geçirecektir.

> [!IMPORTANT]
> PostgreSQL için Azure Veritabanı için Azure AD kimlik doğrulaması şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="setting-the-azure-ad-admin-user"></a>Azure AD Yöneticisi kullanıcısını ayarlama

Azure AD tabanlı kimlik doğrulaması için yalnızca Azure AD yöneticisi kullanıcılar kullanıcıları oluşturabilir/etkinleştirebilir. Azure AD yöneticisini, kullanıcı izinlerini yükselttikginden (örneğin CREATEDB) normal veritabanı işlemleri için kullanmamanızı öneririz.

Azure AD yöneticisini ayarlamak için (bir kullanıcı veya grup kullanabilirsiniz), lütfen aşağıdaki adımları izleyin

1. Azure portalında, Azure AD için etkinleştirmek istediğiniz PostgreSQL için Azure Veritabanı örneğini seçin.
2. Ayarlar altında Active Directory Admin'i seçin:

![azure reklam yöneticisi ayarlama][2]

3. Azure AD yöneticisi olmak için müşteri kiracısında geçerli bir Azure AD kullanıcısı seçin.

> [!IMPORTANT]
> Yöneticiyi ayarlarken, tam yönetici izinleriyle PostgreSQL sunucusu için Azure Veritabanına yeni bir kullanıcı eklenir. PostgreSQL için Azure Veritabanı'ndaki Azure AD `azure_ad_admin`Yöneticisi kullanıcısı bu role sahip olacaktır.

PostgreSQL sunucusu başına yalnızca bir Azure AD yöneticisi oluşturulabilir ve başka bir sunucu seçimi sunucu için yapılandırılan mevcut Azure AD yöneticisinin üzerine yazar. Birden çok yöneticiye sahip olmak için tek bir kullanıcı yerine bir Azure REKLAM grubu belirtebilirsiniz. Daha sonra yönetim amacıyla grup adı ile oturum açacağınızı unutmayın.

## <a name="connecting-to-azure-database-for-postgresql-using-azure-ad"></a>Azure AD'yi kullanarak PostgreSQL için Azure Veritabanına bağlanma

Aşağıdaki üst düzey diyagram, PostgreSQL için Azure Veritabanı ile Azure AD kimlik doğrulamasını kullanmanın iş akışını özetler:

![kimlik doğrulama akışı][1]

Azure AD tümleştirmesini, Azure AD farkında olmayan ve yalnızca PostgreSQL'e bağlanırken kullanıcı adı ve parola belirtmeyi destekleyen psql gibi yaygın PostgreSQL araçlarıyla çalışacak şekilde tasarladık. Yukarıdaki resimde gösterildiği gibi Azure AD belirteci'ni parola olarak geçiyoruz.

Şu anda aşağıdaki istemcileri test ettik:

- psql commandline (belirteci geçmek için PGPASSWORD değişkenini kullanmak, aşağıya bakın)
- Azure Veri Stüdyosu (PostgreSQL uzantısını kullanarak)
- Diğer libpq tabanlı istemciler (örn. ortak uygulama çerçeveleri ve ORM'ler)

> [!NOTE]
> PgAdmin ile Azure AD belirteci kullanmanın şu anda desteklenmediğini, parolalar için 256 karakter (belirteç aşıldığında) için sabit kodlu bir sınırlamaya sahip olduğunu lütfen unutmayın.

Aşağıda açıklanan Azure AD ile bir kullanıcının/uygulamanın kimlik doğrulaması yapması gereken adımlar şunlardır:

### <a name="step-1-authenticate-with-azure-ad"></a>Adım 1: Azure AD ile kimlik doğrulaması

[Azure CLI'nin yüklü](/cli/azure/install-azure-cli)olduğundan emin olun.

Azure AD ile kimlik doğrulaması yapmak için Azure CLI aracını çağırın. Azure AD kullanıcı kimliğinizi ve parolanızı vermenizi gerektirir.

```azurecli-interactive
az login
```

Bu komut, Azure AD kimlik doğrulama sayfasına bir tarayıcı penceresi başlatacaktır.

> [!NOTE]
> Bu adımları gerçekleştirmek için Azure Bulut Su Toplarını da kullanabilirsiniz.
> Azure Bulut Su Ağıtı'nda Azure AD erişim jetonunu alırken açıkça `az login` aramanız ve yeniden oturum açmanız gerekeceğini lütfen unutmayın (kodlu ayrı pencerede). Bu işaretten `get-access-token` sonra komut beklendiği gibi çalışacaktır.

### <a name="step-2-retrieve-azure-ad-access-token"></a>Adım 2: Azure AD erişim jetonunu alın

PostgreSQL için Azure Veritabanı'na erişmek için 1 adımdan Azure AD kimlik doğrulaması yapılan kullanıcıiçin bir erişim jetonu edinmek için Azure CLI aracını çağırın.

Örnek (Genel Bulut için):

```azurecli-interactive
az account get-access-token --resource https://ossrdbms-aad.database.windows.net
```

Yukarıdaki kaynak değeri tam olarak gösterildiği gibi belirtilmelidir. Diğer bulutlar için kaynak değeri aşağıdakileri kullanarak aranabilir:

```azurecli-interactive
az cloud show
```

Azure CLI sürüm 2.0.71 ve sonrası için komut tüm bulutlar için aşağıdaki daha kullanışlı sürümde belirtilebilir:

```azurecli-interactive
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

Belirteç, kimliği doğrulanan kullanıcı hakkındaki tüm bilgileri kodlayan ve PostgreSQL hizmeti için Azure Veritabanı'nı hedefleyen bir Base 64 dizesidir.

> [!NOTE]
> Erişim belirteci geçerliliği 5 dakika ile 60 dakika arasındadır. PostgreSQL için Azure Veritabanı'na giriş başlatmadan hemen önce erişim jetonuna ulaşmanızı öneririz.

### <a name="step-3-use-token-as-password-for-logging-in-with-postgresql"></a>Adım 3: PostgreSQL ile oturum açmak için parola olarak belirteç kullanın

Bağlanırken postgreSQL kullanıcı parolası olarak erişim belirteci kullanmanız gerekir.

Komut satırı `psql` istemcisi kullanırken, erişim belirteci `PGPASSWORD` doğrudan kabul `psql` edebilirsiniz parola uzunluğunu aştığından, erişim belirteci ortam değişkeninden geçirilmesi gerekir:

Windows Örneği:

```shell
set PGPASSWORD=<copy/pasted TOKEN value from step 2>
```

Linux/macOS Örneği:

```shell
export PGPASSWORD=<copy/pasted TOKEN value from step 2>
```

Artık, normalde yaptığınız gibi PostgreSQL için Azure Veritabanı ile bağlantı başlatabilirsiniz:

```shell
psql "host=mydb.postgres... user=user@tenant.onmicrosoft.com@mydb dbname=postgres sslmode=require"
```

Azure AD kimlik doğrulamasını kullanarak artık PostgreSQL sunucunuza kimlik doğrulaması yapılır.

## <a name="creating-azure-ad-users-in-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı'nda Azure AD kullanıcıları oluşturma

PostgreSQL veritabanı için Azure Veritabanınıza bir Azure AD kullanıcısı eklemek için bağlandıktan sonra aşağıdaki adımları gerçekleştirin (nasıl bağlanılabağlanınız la ilgili sonraki bölüme bakın):

1. Öncelikle Azure AD kullanıcısının `<user>@yourtenant.onmicrosoft.com` Azure AD kiracısında geçerli bir kullanıcı olduğundan emin olun.
2. Azure AD Yöneticisi kullanıcısı olarak PostgreSQL örneği için Azure Veritabanınızda oturum açın.
3. PostgreSQL için Azure Veritabanı'nda rol `<user>@yourtenant.onmicrosoft.com` oluşturun.
4. Rol `<user>@yourtenant.onmicrosoft.com` azure_ad_user bir üyesi olun. Bu yalnızca Azure AD kullanıcılarına verilmelidir.

**Örnek:**

```sql
CREATE ROLE "user1@yourtenant.onmicrosoft.com" WITH LOGIN IN ROLE azure_ad_user;
```

> [!NOTE]
> Bir kullanıcının Azure AD aracılığıyla kimliğini doğrulamak, kullanıcıya PostgreSQL veritabanı için Azure Veritabanı'ndaki nesnelere erişme izni vermez. Kullanıcıya gerekli izinleri el ile vermelisiniz.

## <a name="creating-azure-ad-groups-in-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı'nda Azure REKLAM grupları oluşturma

Veritabanınıza erişmek için bir Azure REKLAM grubunu etkinleştirmek için, kullanıcılarla aynı mekanizmayı kullanın, ancak bunun yerine grup adını belirtin:

**Örnek:**

```sql
CREATE ROLE "Prod DB Readonly" WITH LOGIN IN ROLE azure_ad_user;
```

Oturum açarken, grup üyeleri kişisel erişim belirteçlerini kullanır, ancak kullanıcı adı olarak belirtilen grup adı ile imzalar.

## <a name="token-validation"></a>Belirteç Doğrulama

PostgreSQL için Azure Veritabanı'ndaki Azure AD kimlik doğrulaması, kullanıcının PostgreSQL sunucusunda bulunmasını sağlar ve belirteciiçeriğini doğrulayarak belirteci geçerliliğini denetler. Aşağıdaki belirteç doğrulama adımları gerçekleştirilir:

- Belirteç Azure AD tarafından imzalanmıştır ve kurcalanmamıştır
- Belirteç, sunucuyla ilişkili kiracı için Azure AD tarafından verildi
- Belirteci'nin süresi dolmadı
- Belirteç, PostgreSQL kaynağı için Azure Veritabanı içindir (başka bir Azure kaynağı için değil)

## <a name="migrating-existing-postgresql-users-to-azure-ad-based-authentication"></a>Varolan PostgreSQL kullanıcılarını Azure AD tabanlı kimlik doğrulamasına geçirme

Varolan kullanıcılar için Azure AD kimlik doğrulamasını etkinleştirebilirsiniz. Göz önünde bulundurulması gereken iki durum vardır:

### <a name="case-1-postgresql-username-matches-the-azure-ad-user-principal-name"></a>Örnek 1: PostgreSQL kullanıcı adı Azure AD Kullanıcı Adı ile eşleşir

Mevcut kullanıcılarınızın Azure AD kullanıcı adlarıyla zaten eşleşmeolasılığı düşük `azure_ad_user` olması durumunda, azure AD kimlik doğrulaması için bunları etkinleştirmek için bu rolü onlara verebilirsiniz:

```sql
GRANT azure_ad_user TO "existinguser@yourtenant.onmicrosoft.com";
```

Artık daha önce yapılandırılmış PostgreSQL kullanıcı parolalarını kullanmak yerine Azure AD kimlik bilgileriyle oturum açabilecekler.

### <a name="case-2-postgresql-username-is-different-than-the-azure-ad-user-principal-name"></a>Örnek 2: PostgreSQL kullanıcı adı Azure AD Kullanıcı Adı'ndan farklıdır

Bir PostgreSQL kullanıcısı Azure AD'de yoksa veya farklı bir kullanıcı adı varsa, bu PostgreSQL kullanıcısı olarak kimlik doğrulaması yapmak için Azure AD gruplarını kullanabilirsiniz. PostgreSQL kullanıcıları için varolan Azure Veritabanını, PostgreSQL kullanıcısıyla eşleşen bir ada sahip bir Azure REKLAM grubu oluşturarak ve ardından mevcut PostgreSQL kullanıcısına rol azure_ad_user vererek Azure AD'ye geçirebilirsiniz:

```sql
GRANT azure_ad_user TO "DBReadUser";
```

Bu, Azure REKLAM'ınızda "DBReadUser" grubu oluşturduğunuzu varsayar. Bu gruba ait kullanıcılar artık bu kullanıcı olarak veritabanında oturum açabilecek.

## <a name="next-steps"></a>Sonraki adımlar

* [PostgreSQL için Azure Veritabanı ile Azure Active Directory kimlik doğrulaması için](concepts-aad-authentication.md) genel kavramları gözden geçirin - Tek Sunucu

<!--Image references-->

[1]: ./media/concepts-aad-authentication/authentication-flow.png
[2]: ./media/concepts-aad-authentication/set-aad-admin.png
