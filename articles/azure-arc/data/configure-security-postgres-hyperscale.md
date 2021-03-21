---
title: Azure Arc özellikli PostgreSQL Hiper Ölçek sunucu grubunuz için güvenliği yapılandırma
description: Azure Arc özellikli PostgreSQL Hiper Ölçek sunucu grubunuz için güvenliği yapılandırma
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: d6e27fddceb69efbb2c1697c09ee9b61d7f38ee4
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101687983"
---
# <a name="configure-security-for-your-azure-arc-enabled-postgresql-hyperscale-server-group"></a>Azure Arc özellikli PostgreSQL Hiper Ölçek sunucu grubunuz için güvenliği yapılandırma

Bu belgede, sunucu grubunuzun güvenliği ile ilgili çeşitli yönleri açıklanmaktadır:
- Bekleme sırasında şifreleme
- Kullanıcı yönetimi
   - Genel Perspektifler
   - _Postgres_ yönetici kullanıcısının parolasını değiştirme
- Denetim

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme
Veritabanlarınızı depoladığınız diskleri şifreleyerek ve/veya eklediğiniz veya güncellediğiniz verileri şifrelemek için veritabanı işlevlerini kullanarak geri kalan şifrelemeyi uygulayabilirsiniz.

### <a name="hardware-linux-host-volume-encryption"></a>Donanım: Linux konak birimi şifrelemesi
Azure Arc etkin veri Hizmetleri kurulumlarınız tarafından kullanılan disklerde bulunan verilerin güvenliğini sağlamak için sistem veri şifrelemesini uygulayın. Bu konu hakkında daha fazla bilgi edinebilirsiniz:
- Linux 'ta [bekleyen veri şifrelemesi](https://wiki.archlinux.org/index.php/Data-at-rest_encryption) genel olarak 
- LUKS `cryptsetup` şifreleme komutu (Linux) ile disk şifrelemesi ( https://www.cyberciti.biz/security/howto-linux-hard-disk-encryption-with-luks-cryptsetup-command/) özellikle Azure Arc etkin veri Hizmetleri sağladığınız fiziksel altyapıda çalıştığından, altyapının güvenliğini sağlamaya yönelik olursunuz.

### <a name="software-use-the-postgresql-pgcrypto-extension-in-your-server-group"></a>Yazılım: sunucu grubunuzda PostgreSQL `pgcrypto` uzantısını kullanın
Azure Arc kurulumunuzu barındırmak için kullanılan diskleri şifrelemeye ek olarak, Azure Arc etkin PostgreSQL hiper ölçek sunucu grubunuzu, uygulamalarınızın veritabanınızdaki verileri şifrelemek için kullanabileceği mekanizmaları ortaya çıkarmak için yapılandırabilirsiniz. `pgcrypto`Uzantı, `contrib` Postgres uzantılarının bir parçasıdır ve Azure Arc etkin PostgreSQL hiper ölçek sunucu grubunda bulunur. Uzantı hakkındaki ayrıntıları burada bulabilirsiniz `pgcrypto` . [](https://www.postgresql.org/docs/current/pgcrypto.html)
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

   Çıktı 

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
Kullanıcıları veya rolleri oluşturmak için standart Postgres yolunu kullanabilirsiniz. Ama bunu yaparsanız, bu yapıtlar yalnızca koordinatör rolünde kullanılabilir. Önizleme süresince bu kullanıcılar/roller henüz Koordinatör düğümü dışına ve sunucu grubunuzun Çalışan düğümlerine dağıtılan verilere erişemez. Bunun nedeni önizlemede kullanıcı tanımının Çalışan düğümlerine çoğaltılmamasıdır.

### <a name="change-the-password-of-the-_postgres_-administrative-user"></a>_Postgres_ yönetici kullanıcısının parolasını değiştirme
Azure Arc etkin PostgreSQL hiper ölçek, sunucu grubunuzu oluştururken parolayı ayarladığınız standart Postgres Yönetici Kullanıcı _Postgres_ ile birlikte gelir.
Parolasının değiştirileceği komutun genel biçimi:
```console
azdata arc postgres server edit --name <server group name> --admin-password
```

`--admin-password`, AZDATA_PASSWORD **oturum** ortamı değişkeninde bir değerin varlığına ilişkin bir Boole değeri.
AZDATA_PASSWORD **oturum** ortamı değişkeni varsa ve bir değere sahipse yukarıdaki komutun çalıştırılması, Postgres kullanıcısının parolasını bu ortam değişkeninin değerine ayarlar.

AZDATA_PASSWORD **oturum** ortamı değişkeni varsa, ancak değer yoksa veya AZDATA_PASSWORD **oturum** ortamı değişkeni yoksa, yukarıdaki komutun çalıştırılması kullanıcıdan etkileşimli olarak parola girmesini ister

#### <a name="change-the-password-of-the-postgres-administrative-user-in-an-interactive-way"></a>Postgres yönetici kullanıcısının parolasını etkileşimli bir şekilde değiştirme

1. AZDATA_PASSWORD **oturum** ortam değişkenini silme veya değerini silme
2. Şu komutu çalıştırın:
   ```console
   azdata arc postgres server edit --name <server group name> --admin-password
   ```
   Örneğin:
   ```console
   azdata arc postgres server edit -n postgres01 --admin-password
   ```
   Parolayı girmeniz ve onaylamanız istenir:
   ```console
   Postgres Server password:
   Confirm Postgres Server password:
   ```
   Parola güncelleştirilirken komutun çıktısı şunları gösterir:
   ```console
   Updating password
   Updating postgres01 in namespace `arc`
   postgres01 is Ready
   ```
   
#### <a name="change-the-password-of-the-postgres-administrative-user-using-the-azdata_password-session-environment-variable"></a>AZDATA_PASSWORD **oturum** ortamı değişkenini kullanarak Postgres yönetici kullanıcısının parolasını değiştirin:
1. AZDATA_PASSWORD **oturum** ortamı değişkeninin değerini, parola eklemek istediğiniz şekilde ayarlayın.
2. Şu komutu çalıştırın:
   ```console
   azdata arc postgres server edit --name <server group name> --admin-password
   ```
   Örneğin:
   ```console
   azdata arc postgres server edit -n postgres01 --admin-password
   ```
   
   Parola güncelleştirilirken komutun çıktısı şunları gösterir:
   ```console
   Updating password
   Updating postgres01 in namespace `arc`
   postgres01 is Ready
   ```

> [!NOTE]
> AZDATA_PASSWORD oturumunun ortam değişkeninin var olduğunu ve sahip olduğu değeri doğrulamak için şunu çalıştırın:
> - Bir Linux istemcisinde:
> ```console
> printenv AZDATA_PASSWORD
> ```
>
> - PowerShell ile bir Windows istemcisinde:
> ```console
> echo $env:AZDATA_PASSWORD
> ```

## <a name="audit"></a>Denetim

Denetim senaryolarında lütfen sunucu grubunuzu, `pgaudit` Postgres uzantılarını kullanacak şekilde yapılandırın. Daha ayrıntılı bilgi için `pgaudit` bkz. [ `pgAudit` GitHub projesi](https://github.com/pgaudit/pgaudit/blob/master/README.md). `pgaudit`Sunucu grubunuzda uzantıyı etkinleştirmek Için [PostgreSQL uzantılarını kullanın](using-extensions-in-postgresql-hyperscale-server-group.md).


## <a name="next-steps"></a>Sonraki adımlar
- [ `pgcrypto` Uzantıya](https://www.postgresql.org/docs/current/pgcrypto.html) bakın
- Bkz. [PostgreSQL uzantılarını kullanma](using-extensions-in-postgresql-hyperscale-server-group.md)

