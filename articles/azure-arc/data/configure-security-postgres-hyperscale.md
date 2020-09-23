---
title: Azure Arc etkin PostgreSQL hiper ölçek sunucu grubu için güvenliği yapılandırma
description: Azure Arc etkin PostgreSQL hiper ölçek sunucu grubu için güvenliği yapılandırma
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: b166348031e9f72e8005e866a198855db9c01a9c
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941598"
---
# <a name="configure-security-for-your-azure-arc-enabled-postgresql-hyperscale-server-group"></a>Azure Arc etkin PostgreSQL hiper ölçek sunucu grubu için güvenliği yapılandırma

Bu belgede, sunucu grubunuzun güvenliği ile ilgili çeşitli yönleri açıklanmaktadır:
- Bekleme sırasında şifreleme
- Kullanıcı yönetimi
   - Genel Perspektifler
   - _Postgres_ yönetici kullanıcısının parolasını değiştirme

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme
Veritabanlarınızı depoladığınız diskleri şifreleyerek ve/veya eklediğiniz veya güncellediğiniz verileri şifrelemek için veritabanı işlevlerini kullanarak geri kalan şifrelemeyi uygulayabilirsiniz.

### <a name="hardware-linux-host-volume-encryption"></a>Donanım: Linux konak birimi şifrelemesi
Azure Arc etkin veri Hizmetleri kurulumlarınız tarafından kullanılan disklerde bulunan verilerin güvenliğini sağlamak için sistem veri şifrelemesini uygulayın. Bu konu hakkında daha fazla bilgi edinebilirsiniz:
- Linux 'ta [bekleyen veri şifrelemesi](https://wiki.archlinux.org/index.php/Data-at-rest_encryption) genel olarak 
- LUKS `cryptsetup` şifreleme komutu (Linux) ile disk şifrelemesi ( https://www.cyberciti.biz/security/howto-linux-hard-disk-encryption-with-luks-cryptsetup-command/) özellikle Azure Arc etkin veri Hizmetleri sağladığınız fiziksel altyapıda çalıştığından, altyapının güvenliğini sağlamaya yönelik olursunuz.

### <a name="software-use-the-postgresql-pgcrypto-extension-in-your-server-group"></a>Yazılım: sunucu grubunuzda PostgreSQL `pgcrypto` uzantısını kullanın
Azure Arc kurulumunuzu barındırmak için kullanılan diskleri şifrelemeye ek olarak, Azure Arc etkin PostgreSQL hiper ölçek sunucu grubunuzu, uygulamalarınızın veritabanınızdaki verileri şifrelemek için kullanabileceği mekanizmaları ortaya çıkarmak için yapılandırabilirsiniz. `pgcrypto`Uzantı, `contrib` Postgres uzantılarının bir parçasıdır ve Azure Arc etkin PostgreSQL hiper ölçek sunucu grubunda bulunur. Uzantı hakkındaki ayrıntıları burada bulabilirsiniz `pgcrypto` . [here](https://www.postgresql.org/docs/current/pgcrypto.html)
Özet ' de, aşağıdaki komutlarla uzantıyı etkinleştirir, oluşturur ve bunu kullanırsınız:


#### <a name="create-the-pgcrypto-extension"></a>Uzantıyı oluşturma `pgcrypto`
İstediğiniz istemci aracıyla sunucu grubunuza bağlanın ve standart PostgreSQL sorgusunu çalıştırın:
```console
CREATE EXTENSION pgcrypto;
```

> Nasıl bağlanabileceğinizi öğrenmek için [buradaki](get-connection-endpoints-and-connection-strings-postgres-hyperscale.md) ayrıntıları bulabilirsiniz.

#### <a name="verify-the-list-the-extensions-ready-to-use-in-your-server-group"></a>Sunucu grubunuzda uzantıları kullanıma hazırlama listesini doğrulayın
`pgcrypto`Sunucu grubunuzda bulunan uzantıları listeleyerek uzantının kullanıma hazır olduğunu doğrulayabilirsiniz.
İstediğiniz istemci aracıyla sunucu grubunuza bağlanın ve standart PostgreSQL sorgusunu çalıştırın:
```console
select * from pg_extension;
```
`pgcrypto`Yukarıda belirtilen komutlarla etkinleştirilip oluşturmadıysanız ve oluşturduysanız, bunu görmeniz gerekir.

#### <a name="use-the-pgcrypto-extension"></a>Uzantıyı kullanma `pgcrypto`
Artık uygulamalarınızı, tarafından sunulan işlevlerden herhangi birini kullanacak şekilde ayarlayabilirsiniz `pgcrypto` :
- Genel karma işlevleri
- Parola karma işlevleri
- PGP şifreleme işlevleri
- Ham şifreleme işlevleri
- Rastgele veri işlevleri

Örneğin, karma değerler oluşturmak için. Şu komutu çalıştırın:

```console
Select crypt('Les sanglots longs des violons de l_automne', gen_salt('md5'));
```

Aşağıdaki karmayı döndürür:

```console
              crypt
------------------------------------
 $1$/9ACBYOV$z52PAGjQ5WTU9xvEECBNv/   
```

Ya da, örneğin:

```console
select hmac('Les sanglots longs des violons de l_automne', 'md5', 'sha256');
```

Aşağıdaki karmayı döndürür:

```console
                                hmac
--------------------------------------------------------------------
 \xd4e4790b69d2cc8dbce3385ee63272bc7760f1603640bb211a7b864e695570c5
```

Örneğin, şifreli verileri bir parola gibi depolamak için:

Uygulamamın gizli dizileri aşağıdaki tabloda depoladığını varsayalım:

```console
create table mysecrets(USERid int, USERname char(255), USERpassword char(512));
```

Ve Kullanıcı oluştururken parolalarını şifreleyemiyorum:

```console
insert into mysecrets values (1, 'Me', crypt('MySecretPasswrod', gen_salt('md5')));
```

Şimdi Parolamın şifrelendiğini görüyorum:

```console
select * from mysecrets;
```

Çıkış:

- Kullanıcı kimliği: 1
- Kullanıcı adı: Ben
- USERpassword: $1 $ Uc7jzZOp $ NTfcGo7F10zGOkXOwjHy31

Uygulamam ile bağlandığımda ve bir parola geçirdiğimde, bu, `mysecrets` tabloya bakar ve uygulamaya sunulan parola ile tabloda depolanan parolalar arasında bir eşleşme varsa kullanıcının adını döndürür. Örnek:

- Yanlış parolayı geçirdim:
   ```console
   select USERname from mysecrets where (USERpassword = crypt('WrongPassword', USERpassword));
   ```

   Çıkış 

   ```returns
    USERname
   ---------
   (0 rows)
   ```
- Doğru parolayı geçirdim:

   ```console
   select USERname from mysecrets where (USERpassword = crypt('MySecretPasswrod', USERpassword));
   ``` 

   Çıkış:

   ```output
    USERname
   ---------
   Me
   (1 row)
   ```

Bu küçük örnek, Postgres uzantısını kullanarak, Azure Arc etkin PostgreSQL hiper ölçeğinde bekleyen verileri (şifrelenmiş verileri depolama) şifreleyebilir `pgcrypto` ve uygulamalarınızın `pgcrypto` Bu şifrelenmiş verileri değiştirmek için tarafından sunulan işlevleri kullanabilmesi gerektiğini gösterir.

## <a name="user-management"></a>Kullanıcı yönetimi
### <a name="general-perspectives"></a>Genel Perspektifler
Kullanıcıları veya rolleri oluşturmak için standart Postgres yolunu kullanabilirsiniz. Ancak bunu yaparsanız, bu yapıtlar yalnızca Düzenleyici rolünde kullanılabilir. Önizleme süresince, bu kullanıcılar/roller henüz düzenleyici düğümünün dışında ve sunucu grubunuzun çalışan düğümlerinde dağıtılan verilere erişemez. Bunun nedeni, önizleme aşamasında Kullanıcı tanımının çalışan düğümlerine çoğaltılmamıştır.

### <a name="change-the-password-of-the-_postgres_-administrative-user"></a>_Postgres_ yönetici kullanıcısının parolasını değiştirme
Azure Arc etkin PostgreSQL hiper ölçek, sunucu grubunuzu oluştururken parolayı ayarladığınız standart Postgres Yönetici Kullanıcı _Postgres_ ile birlikte gelir.
Parolasının değiştirileceği komutun genel biçimi:
```console
azdata arc postgres server edit --name <server group name> --admin-password <new password>
```
Parola, varsa AZDATA_PASSWORD **oturumunun**ortam değişkeninin değerine ayarlanır. Aksi takdirde, kullanıcıdan bir değer girmesi istenir.
AZDATA_PASSWORD oturumunun ortam değişkeninin ne olduğunu ve/veya hangi değere ayarlandığını doğrulamak için şunu çalıştırın:
```console
printenv AZDATA_PASSWORD
```
Yeni bir parola girmeniz istenirse, değerini silmek isteyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
- Bu `pgcrypto` uzantının ayrıntılarını [buradan](https://www.postgresql.org/docs/current/pgcrypto.html)okuyun.
- Postgres uzantılarının nasıl kullanılacağına ilişkin ayrıntıları [burada](using-extensions-in-postgresql-hyperscale-server-group.md)bulabilirsiniz.

