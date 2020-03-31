---
title: Veri Gölü Depolama Gen1'de erişim denetimine genel bakış | Microsoft Dokümanlar
description: Azure Veri Gölü Depolama Gen1'de erişim denetiminin nasıl çalıştığını anlama
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: d16f8c09-c954-40d3-afab-c86ffa8c353d
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: twooley
ms.openlocfilehash: 276e691351d852d6dcb0075d47bf33af6767fc10
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260338"
---
# <a name="access-control-in-azure-data-lake-storage-gen1"></a>Azure Data Lake Storage 1. Nesil'de erişim denetimi

Azure Veri Gölü Depolama Gen1, HDFS'den türetilen ve sırayla POSIX erişim kontrol modelinden türeyen bir erişim denetimi modeli uygular. Bu makalede, Veri Gölü Depolama Gen1 için erişim denetimi modelinin temelleri özetlenmiştir. 

## <a name="access-control-lists-on-files-and-folders"></a>Dosyalar ve klasörler üzerindeki erişim denetimi listeleri

İki tür erişim denetim listesi (ACL) vardır: **Erişim ACL’leri** ve **Varsayılan ACL’ler**.

* **Erişim ACL’leri**: Bunlar bir nesneye erişimi denetler. Hem dosyalar hem de klasörler Erişim ACL’lerine sahiptir.

* **Varsayılan ACL’ler**: Bir klasör ile ilişkili olan ACL’lerin o klasör altında oluşturulan tüm alt öğelere ilişkin Erişim ACL’lerini belirleyen bir "şablonudur". Dosyalar Varsayılan ACL’ye sahip değildir.


Hem Erişim ACL'leri hem de Varsayılan ACL'ler aynı yapıdadır.



> [!NOTE]
> Bir üst öğe üzerindeki Varsayılan ACL’nin değiştirilmesi zaten var olan alt öğelerin Erişim ACL’sini veya Varsayılan ACL’sini etkilemez.
>
>

## <a name="permissions"></a>İzinler

Dosya sistemi nesnesi üzerinde **Okuma**, **Yazma** ve **Yürütme** izinleri bulunur ve bunlar aşağıdaki tabloda gösterildiği gibi dosyalar ve klasörler üzerinde kullanılabilir:

|            |    Dosya     |   Klasör |
|------------|-------------|----------|
| **Okuma (R)** | Bir dosyanın içeriğini okuyabilir | Klasörün içeriğini listelemek için **Okuma** ve **Yürütme** izinlerini gerektirir|
| **Yazma (W)** | Bir dosyaya yazabilir veya ekleyebilir | Bir klasörde alt öğeler oluşturmak için **Yazma** ve **Yürütme** gerektirir |
| **Yürütme (X)** | Veri Gölü Depolama Gen1 bağlamında bir şey ifade etmez | Bir klasörün alt öğelerini geçirmek için gereklidir |

### <a name="short-forms-for-permissions"></a>İzinlerin kısaltmaları

**RWX**, **Okuma + Yazma + Yürütme** için kullanılır. **Okuma=4**, **Yazma=2** ve **Yürütme=1** olup toplamları izinleri temsil eden daha da kısaltılmış bir sayısal biçim mevcuttur. Bazı örnekler aşağıda verilmiştir.

| Sayısal biçim | Kısa biçim |      Anlamı     |
|--------------|------------|------------------------|
| 7            | `RWX`        | Okuma + Yazma + Yürütme |
| 5            | `R-X`        | Okuma + Yürütme         |
| 4            | `R--`        | Okuma                   |
| 0            | `---`        | İzin yok         |


### <a name="permissions-do-not-inherit"></a>İzinler devralınmaz

Veri Gölü Depolama Gen1 tarafından kullanılan POSIX stili modelinde, bir öğenin izinleri maddenin kendisinde depolanır. Diğer bir deyişle, bir öğenin izinleri üst öğelerinden devralınamaz.

## <a name="common-scenarios-related-to-permissions"></a>İzinlerle ilgili yaygın senaryolar

Aşağıda, bir Veri Gölü Depolama Gen1 hesabında belirli işlemleri gerçekleştirmek için hangi izinlerin gerekli olduğunu anlamanıza yardımcı olacak yaygın senaryolar verilmiştir.

| İşlem | Nesne              |    /      | Seattle/   | Portland/   | Data.txt       |
|-----------|---------------------|-----------|------------|-------------|----------------|
| Okuma      | Data.txt            |   `--X`   |   `--X`    |  `--X`      | `R--`          |
| Ek | Data.txt            |   `--X`   |   `--X`    |  `--X`      | `RW-`          |
| Sil    | Data.txt            |   `--X`   |   `--X`    |  `-WX`      | `---`          |
| Oluşturma    | Data.txt            |   `--X`   |   `--X`    |  `-WX`      | `---`          |
| Liste      | /                   |   `R-X`   |   `---`    |  `---`      | `---`          |
| Liste      | /Seattle/           |   `--X`   |   `R-X`    |  `---`      | `---`          |
| Liste      | /Seattle/Portland/  |   `--X`   |   `--X`    |  `R-X`      | `---`          |


> [!NOTE]
> Önceki iki koşul geçerli oldukça dosyayı silmek için dosya üzerinde yazma izinleri gerekli değildir.
>
>


## <a name="users-and-identities"></a>Kullanıcılar ve kimlikler

Her dosya ve klasör bu kimlikler için farklı izinlere sahiptir:

* Sahip olan kullanıcı
* Sahip olan grup
* Adlandırılmış kullanıcılar
* Adlandırılmış gruplar
* Diğer tüm kullanıcılar

Kullanıcıların ve grupların kimlikleri, Azure Active Directory (Azure AD) kimlikleridir. Bu nedenle, aksi belirtilmedikçe, Veri Gölü Depolama Gen1 bağlamında bir "kullanıcı", bir Azure REKLAM kullanıcısı veya Azure AD güvenlik grubu anlamına gelebilir.

### <a name="the-super-user"></a>Süper kullanıcı

Bir süper kullanıcı, Veri Gölü Depolama Gen1 hesabındaki tüm kullanıcıların en fazla hakkına sahiptir. Süper kullanıcı:

* **Tüm** dosya ve klasörlerde RWX İzinlerine sahiptir.
* Herhangi bir dosya veya klasörün izinlerini değiştirebilir.
* Herhangi bir dosya veya klasörün sahibi olan kullanıcıyı ya da grubu değiştirebilir.

Veri Gölü Depolama Gen1 hesabının **Sahipleri** rolünün bir parçası olan tüm kullanıcılar otomatik olarak bir süper kullanıcıdır.

### <a name="the-owning-user"></a>Sahip olan kullanıcı

Öğeyi oluşturan kullanıcı otomatik olarak öğenin sahibi olan kullanıcıdır. Sahip olan kullanıcı şunları yapabilir:

* Sahip olunan bir dosyanın izinlerini değiştirme.
* Sahip olan kullanıcı aynı zamanda hedef grubun bir üyesi oldukça, sahip olunan bir dosyanın sahibi olan grubunu değiştirme.

> [!NOTE]
> Sahip olan kullanıcı bir dosya veya klasörün sahibi olan kullanıcıyı *değiştiremez*. Bir dosya veya klasörün sahibi olan kullanıcıyı yalnızca süper kullanıcılar değiştirebilir.
>
>

### <a name="the-owning-group"></a>Sahip olan grup

**Arka plan**

POSIX ACL’lerinde her kullanıcı bir "birincil grup" ile ilişkilendirilir. Örneğin, "gamze" adlı kullanıcı "finans" grubuna ait olabilir. Gamze ayrıca birden fazla gruba ait olabilir, ancak bir grup her zaman birincil grubu olarak atanır. POSIX’te Gamze bir dosya oluşturduğunda o dosyanın sahibi olan grup birincil grubu olarak ayarlanır (bu örnekte "finans" grubudur). Aksi takdirde sahip olan grup, diğer kullanıcılar/gruplar için atanan izinlere benzer şekilde davranır.

Veri Gölü Depolama Gen1'deki kullanıcılarla ilişkili bir "birincil grup" olmadığından, sahip olan grup aşağıdaki gibi atanır.

**Yeni bir dosya veya klasör için sahip olma grubunu atama**

* **Olay 1**: Kök klasör "/". Bu klasör, Bir Veri Gölü Depolama Gen1 hesabı oluşturulduğunda oluşturulur. Bu durumda, sahip olan grup tamamen sıfır GUID olarak ayarlanır.  Bu değer herhangi bir erişime izin vermez.  Bu süre bir grup atanana kadar bir yer tutucudur.
* **Olay 2** (Diğer her olay): Yeni bir olay oluşturulduğunda sahip olan grup üst klasörden kopyalanır.

**Sahip grubuna sahip olmak**

Sahip olan grup aşağıdakiler tarafından değiştirilebilir:
* Herhangi bir süper kullanıcı.
* Sahip olan kullanıcı aynı zamanda hedef grubun üyesi ise sahip olan kullanıcı.

> [!NOTE]
> Sahip olan grup, bir dosya veya klasörün ACL’lerini *değiştiremez*.
>
> Eylül 2018 tarihinde veya öncesinde oluşturulan hesaplar için, sahip olan grup, yukarıdaki **Durumda 1'in**kök klasörü durumunda hesabı oluşturan kullanıcıya ayarlanmıştı.  Tek bir kullanıcı hesabı, sahip olan grup üzerinden izin sağlamak için geçerli değildir, bu nedenle bu varsayılan ayar tarafından izin verilmez. Bu izni geçerli bir kullanıcı grubuna atayabilirsiniz.


## <a name="access-check-algorithm"></a>Erişim denetimi algoritması

Aşağıdaki pseudocode, Veri Gölü Depolama Gen1 hesapları için erişim denetimi algoritmasını temsil eder.

```
def access_check( user, desired_perms, path ) : 
  # access_check returns true if user has the desired permissions on the path, false otherwise
  # user is the identity that wants to perform an operation on path
  # desired_perms is a simple integer with values from 0 to 7 ( R=4, W=2, X=1). User desires these permissions
  # path is the file or folder
  # Note: the "sticky bit" is not illustrated in this algorithm
  
# Handle super users.
  if (is_superuser(user)) :
    return True

  # Handle the owning user. Note that mask IS NOT used.
  entry = get_acl_entry( path, OWNER )
  if (user == entry.identity)
      return ( (desired_perms & entry.permissions) == desired_perms )

  # Handle the named users. Note that mask IS used.
  entries = get_acl_entries( path, NAMED_USER )
  for entry in entries:
      if (user == entry.identity ) :
          mask = get_mask( path )
          return ( (desired_perms & entry.permmissions & mask) == desired_perms)

  # Handle named groups and owning group
  member_count = 0
  perms = 0
  entries = get_acl_entries( path, NAMED_GROUP | OWNING_GROUP )
  for entry in entries:
    if (user_is_member_of_group(user, entry.identity)) :
      member_count += 1
      perms | =  entry.permissions
  if (member_count>0) :
    return ((desired_perms & perms & mask ) == desired_perms)
 
  # Handle other
  perms = get_perms_for_other(path)
  mask = get_mask( path )
  return ( (desired_perms & perms & mask ) == desired_perms)
```

### <a name="the-mask"></a>Maske

Erişim Denetimi Algoritması'nda gösterildiği gibi, maske **adlandırılmış kullanıcılar**, **sahip olan grup**ve **adlandırılmış grupların**erişimini sınırlar.  

> [!NOTE]
> Yeni bir Veri Gölü Depolama Gen1 hesabı için, kök klasörünün Access ACL 'sinin ("/") maskesi RWX varsayılandır.
>
>

### <a name="the-sticky-bit"></a>Yapışkan bit

Yapışkan bit POSIX dosya sisteminin daha gelişmiş bir özelliğidir. Veri Gölü Depolama Gen1 bağlamında, yapışkan bit gerekli olması olası değildir. Özetle, yapışkan bit bir klasörde etkinleştirilmişse, alt öğe yalnızca alt öğenin sahibi kullanıcısı tarafından silinebilir veya yeniden adlandırılabilir.

Yapışkan bit Azure portalında gösterilmez.

## <a name="default-permissions-on-new-files-and-folders"></a>Yeni dosya ve klasörlerde varsayılan izinler

Var olan bir klasör altında yeni bir dosya ya da klasör oluşturulduğunda üst klasördeki Varsayılan ACL aşağıdakileri belirler:

- Bir alt klasörün Varsayılan ACL’si ve Erişim ACL’si.
- Bir alt dosyanın Erişim ACL’si (dosyaları Varsayılan ACL’ye sahip değildir).

### <a name="umask"></a>umask

Bir dosya veya klasör oluştururken, umask alt öğeüzerinde varsayılan ALA'ların nasıl ayarlanır değiştirin. umask, **kullanıcıya**sahip olmak , **grup sahibi**olmak için RWX değeri içeren ana klasörlerdeki 9 bitlik bir değerdir ve **diğer**.

Azure Veri Gölü Depolama Gen1 için umask 007 olarak ayarlanmış sabit bir değerdir. Bu değer,

| umask bileşeni     | Sayısal biçim | Kısa biçim | Anlamı |
|---------------------|--------------|------------|---------|
| umask.owning_user   |    0         |   `---`      | Kullanıcısahibi olmak için, üst öğenin Varsayılan ACL'sini çocuğun Access ACL'sine kopyalayın | 
| umask.owning_group  |    0         |   `---`      | Sahip grubu için, üst öğenin Varsayılan ACL'sini çocuğun Access ACL'sine kopyalayın | 
| umask.other         |    7         |   `RWX`      | Diğer olarak, çocuğun Access ACL'sindeki tüm izinleri kaldırın |

Azure Veri Gölü Depolama Gen1 tarafından kullanılan umask değeri, Varsayılan ACL'nin gösterdiğine bakılmaksızın, diğer değerin varsayılan olarak yeni çocuklara asla aktarılamayacağı anlamına gelir. 

Aşağıdaki pseudocode, bir alt öğe için AK'lar oluşturulurken umask'ın nasıl uygulandığını gösterir.

```
def set_default_acls_for_new_child(parent, child):
    child.acls = []
    for entry in parent.acls :
        new_entry = None
        if (entry.type == OWNING_USER) :
            new_entry = entry.clone(perms = entry.perms & (~umask.owning_user))
        elif (entry.type == OWNING_GROUP) :
            new_entry = entry.clone(perms = entry.perms & (~umask.owning_group))
        elif (entry.type == OTHER) :
            new_entry = entry.clone(perms = entry.perms & (~umask.other))
        else :
            new_entry = entry.clone(perms = entry.perms )
        child_acls.add( new_entry )
```

## <a name="common-questions-about-acls-in-data-lake-storage-gen1"></a>Data Lake Storage Gen1'deki Aç'lar hakkında sık sorulan sorular

### <a name="do-i-have-to-enable-support-for-acls"></a>ACL desteğini etkinleştirmem gerekiyor mu?

Hayır. ACD'ler üzerinden erişim denetimi, veri gölü depolama gen1 hesabı için her zaman açıktır.

### <a name="which-permissions-are-required-to-recursively-delete-a-folder-and-its-contents"></a>Bir klasörü ve içindekileri yinelemeli olarak silmek için hangi izinler gereklidir?

* Üst klasör **Yazma + Yürütme** izinlerine sahip olmalıdır.
* Silinecek klasör ve içindeki her klasör **Okuma + Yazma + Yürütme** izinlerini gerektirir.

> [!NOTE]
> Klasörlerdeki dosyaları silmek için Yazma izni gerekmez. Ayrıca, "/" kök klasör **hiçbir zaman** silinemez.
>
>

### <a name="who-is-the-owner-of-a-file-or-folder"></a>Bir dosyanın veya klasörün sahibi kimdir?

Bir dosyayı veya klasörü oluşturan kişi bunların sahibi olur.

### <a name="which-group-is-set-as-the-owning-group-of-a-file-or-folder-at-creation"></a>Oluşturma sırasında bir dosyanın veya klasörün sahibi olan grubu olarak hangi grup ayarlanır?

Sahip olan grup, yeni dosya veya klasörün oluşturulduğu üst klasörün sahibi olan gruptan kopyalanır.

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a>Bir dosyanın sahibiyim, ancak gereken RWX izinlerine sahip değilim. Ne yapmalıyım?

Sahip olan kullanıcı kendisine gerekli olan her türlü RWX iznini vermek için dosyanın izinlerini değiştirebilir.

### <a name="when-i-look-at-acls-in-the-azure-portal-i-see-user-names-but-through-apis-i-see-guids-why-is-that"></a>Azure portalında ACL’lere baktığımda kullanıcı adlarını görüyorum, ancak API’lere baktığımda GUID’leri görüyorum, bunun nedeni nedir?

ACL’lerdeki girişler, Azure AD’de kullanıcılara karşılık gelen GUID’ler olarak depolanır. API’ler GUID’leri olduğu gibi döndürür. Azure portalı mümkün olduğunda GUID’leri kolay adlara çevirerek ACL’lerin daha kolay kullanılmasını sağlamaya çalışır.

### <a name="why-do-i-sometimes-see-guids-in-the-acls-when-im-using-the-azure-portal"></a>Azure portalını kullanırken neden bazen ACL’lerde GUID’leri görüyorum?

Kullanıcı artık Azure AD’de mevcut değilse bir GUID gösterilir. Bu genellikle, kullanıcı şirketten ayrıldığında veya Azure AD’de kullanıcının hesabı silindiğinde gerçekleşir.

### <a name="does-data-lake-storage-gen1-support-inheritance-of-acls"></a>Data Lake Storage Gen1 ACD'lerin devrini destekliyor mu?

Hayır, ancak üst klasör altında yeni oluşturulan alt dosyalara ve klasöre yönelik ACL’yi ayarlamak için Varsayılan ACL’ler kullanılabilir.  

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>POSIX erişim denetimi modeli hakkında daha fazla bilgiyi nereden bulabilirim?

* [Linux üzerinde POSIX Erişim Denetim Listeleri](https://www.linux.com/news/posix-acls-linux)
* [HDFS izin kılavuzu](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html)
* [POSIX SSS](https://www.opengroup.org/austin/papers/posix_faq.html)
* [POSIX 1003.1 2008](https://standards.ieee.org/findstds/standard/1003.1-2008.html)
* [POSIX 1003.1 2013](https://pubs.opengroup.org/onlinepubs/9699919799.2013edition/)
* [POSIX 1003.1 2016](https://pubs.opengroup.org/onlinepubs/9699919799.2016edition/)
* [Ubuntu üzerinde POSIX ACL](https://help.ubuntu.com/community/FilePermissionsACLs)
* [Linux üzerinde erişim denetim listelerini kullanan ACL](https://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Veri Gölü Depolama Gen1'e Genel Bakış](data-lake-store-overview.md)
