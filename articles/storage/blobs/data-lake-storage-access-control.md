---
title: Azure Veri Gölü Depolama Gen2'de erişim denetimine genel bakış | Microsoft Dokümanlar
description: Azure Veri Gölü Depolama Gen2'de erişim denetiminin nasıl çalıştığını anlama
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: normesta
ms.reviewer: jamesbak
ms.openlocfilehash: 192e46fd7f86b6053eaf658fa65e3c6cdfa3a4e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79528617"
---
# <a name="access-control-in-azure-data-lake-storage-gen2"></a>Azure Veri Gölü Depolama Gen2'de erişim denetimi

Azure Veri Gölü Depolama Gen2, hem Azure rol tabanlı erişim denetimini (RBAC) hem de POSIX benzeri erişim denetim listelerini (AçS) destekleyen bir erişim denetimi modeli uygular. Bu makalede, Veri Gölü Depolama Gen2 için erişim denetimi modelinin temelleri özetlenmiştir.

<a id="azure-role-based-access-control-rbac" />

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

RBAC, *güvenlik ilkelerine*izin kümelerini etkili bir şekilde uygulamak için rol atamaları kullanır. *Güvenlik ilkesi,* Azure etkin dizininde (AD) tanımlanan ve Azure kaynaklarına erişim isteyen bir kullanıcıyı, grubu, hizmet yöneticisini veya yönetilen kimliği temsil eden bir nesnedir.

Genellikle, bu Azure kaynakları üst düzey kaynaklarla sınırlandırılmıştır (Örneğin: Azure Depolama hesapları). Azure Depolama ve dolayısıyla Azure Veri Gölü Depolama Gen2 durumunda, bu mekanizma kapsayıcı (dosya sistemi) kaynağına genişletildi.

Depolama hesabınız kapsamındaki güvenlik ilkelerine nasıl rol atayabilirsiniz öğrenmek için Azure [portalında RBAC ile Azure blob ve sıra verilerine Erişim Izni bölümüne](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)bakın.

> [!NOTE]
> Konuk kullanıcı rol ataması oluşturamaz.

### <a name="the-impact-of-role-assignments-on-file-and-directory-level-access-control-lists"></a>Rol atamalarının dosya ve dizin düzeyi erişim denetim listeleri üzerindeki etkisi

RBAC rol atamaları kullanarak erişim izinleri denetlemek için güçlü bir mekanizma olsa da, ALA'lara göre çok kaba taneli mekanizmadır. RBAC için en küçük taneciklilik konteyner düzeyindedir ve bu durum ALA'lardan daha yüksek bir öncelikte değerlendirilecektir. Bu nedenle, bir kapsayıcı kapsamında bir güvenlik ilkesine bir rol atarsanız, bu güvenlik ilkesi, ACL atamalarıne bakılmaksızın, bu kapsayıcıdaki TÜM dizinler ve dosyalar için bu rolile ilişkili yetkilendirme düzeyine sahiptir.

Bir güvenlik ilkesine yerleşik bir rol veya özel bir [rol](https://docs.microsoft.com/azure/storage/common/storage-auth-aad?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#built-in-rbac-roles-for-blobs-and-queues)aracılığıyla RBAC veri izinleri verildiğinde, bu izinler önce bir isteğin yetkilendirmesi üzerine değerlendirilir. İstenen işlem güvenlik sorumlusunun RBAC atamaları tarafından yetkilendirildiği takdirde yetkilendirme hemen çözülür ve ek ACL denetimleri yapılmaz. Alternatif olarak, güvenlik sorumlusunun RBAC ataması yoksa veya isteğin çalışması atanan izinle eşleşmiyorsa, güvenlik sorumlusunun istenen işlemi gerçekleştirmeye yetkili olup olmadığını belirlemek için ACL denetimleri gerçekleştirilir.

> [!NOTE]
> Güvenlik ilkesine Depolama Blob Veri Sahibi yerleşik rol ataması atanmışsa, güvenlik ilkesi *süper kullanıcı* olarak kabul edilir ve bir dizin veya dosyanın sahibinin yanı sıra sahip olmadıkları dizinler ve dosyalar için ACD'ler ayarlamak da dahil olmak üzere tüm mutasyon işlemlerine tam erişim hakkı verilir. Süper kullanıcı erişimi, kaynağın sahibini değiştirmek için tek yetkili yoldur.

## <a name="shared-key-and-shared-access-signature-sas-authentication"></a>Paylaşılan Anahtar ve Paylaşılan Erişim İmzası (SAS) kimlik doğrulaması

Azure Veri Gölü Depolama Gen2, kimlik doğrulaması için Paylaşılan Anahtar ve SAS yöntemlerini destekler. Bu kimlik doğrulama yöntemlerinin bir özelliği, arayanla hiçbir kimliğin ilişkilendirilmesi ve bu nedenle güvenlik ilkesine dayalı yetkilendirmenin gerçekleştirilememesidir.

Paylaşılan Anahtar durumunda, arayan etkin bir şekilde 'süper kullanıcı' erişimi kazanır, bu da sahibinin ayarlanması ve ACD'lerin değiştirilmesi de dahil olmak üzere tüm kaynaklardaki tüm işlemlere tam erişim anlamına gelir.

SAS belirteçleri belirteçbir parçası olarak izin verilen izinleri içerir. SAS belirtecinde yer alan izinler tüm yetkilendirme kararlarına etkili bir şekilde uygulanır, ancak ek ACL denetimleri yapılmaz.

## <a name="access-control-lists-on-files-and-directories"></a>Dosyalar ve dizinler üzerindeki denetim listelerine erişin

Bir güvenlik ilkesini dosyalar ve dizinler için erişim düzeyiyle ilişkilendirebilirsiniz. Bu ilişkilendirmeler bir *erişim denetim listesinde (ACL)* yakalanır. Depolama hesabınızdaki her dosya ve dizin bir erişim denetim listesi ne sahiptir.

> [!NOTE]
> ALA'lar yalnızca aynı kiracıdaki güvenlik ilkeleri için geçerlidir. Konuk kullanıcıyı erişim düzeyiyle ilişkilendiremezsiniz.  

Depolama hesabı düzeyinde bir güvenlik ilkesine bir rol atadıysanız, bu güvenlik ilkesinin belirli dosyalara ve dizinlere yüksek erişim hakkı vermek için erişim denetim listelerini kullanabilirsiniz.

Bir rol ataması tarafından verilen düzeyin altında bir erişim düzeyi sağlamak için erişim denetim listelerini kullanamazsınız. Örneğin, [Depolama Blob Veri Katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor) rolünü bir güvenlik ilkesine atarsanız, güvenlik sorumlusunun bir dizine yazmasını önlemek için erişim denetim listelerini kullanamazsınız.


### <a name="set-file-and-directory-level-permissions-by-using-access-control-lists"></a>Erişim denetimi listelerini kullanarak dosya ve dizin düzeyi izinlerini ayarlama

Dosya ve dizin düzeyi izinlerini ayarlamak için aşağıdaki makalelerden herhangi birini görün:

|||
|--------|-----------|
|Azure Storage Gezgini |[Azure Veri Gölü Depolama Gen2'deki dizinleri, dosyaları ve ACD'leri yönetmek için Azure Depolama Gezgini'ni kullanın](data-lake-storage-explorer.md#managing-access)|
|.NET |[Azure Veri Gölü Depolama Gen2'deki dizinleri, dosyaları ve ACD'leri yönetmek için .NET'i kullanın](data-lake-storage-directory-file-acl-dotnet.md)|
|Java|[Azure Veri Gölü Depolama Gen2'deki dizinleri, dosyaları ve ACD'leri yönetmek için Java'yı kullanın](data-lake-storage-directory-file-acl-java.md)|
|Python|[Azure Veri Gölü Depolama Gen2'deki dizinleri, dosyaları ve ACD'leri yönetmek için Python'u kullanın](data-lake-storage-directory-file-acl-python.md)|
|PowerShell|[Azure Veri Gölü Depolama Gen2'deki dizinleri, dosyaları ve ACD'leri yönetmek için PowerShell'i kullanın](data-lake-storage-directory-file-acl-powershell.md)|
|Azure CLI|[Azure Veri Gölü Depolama Gen2'deki dizinleri, dosyaları ve ACD'leri yönetmek için Azure CLI'yi kullanın](data-lake-storage-directory-file-acl-cli.md)|
|REST API |[Yol - Güncelle](https://docs.microsoft.com/rest/api/storageservices/datalakestoragegen2/path/update)|

> [!IMPORTANT]
> Güvenlik sorumlusu bir *hizmet* sorumlusuysa, ilgili uygulama kaydının nesne kimliğini değil, hizmet sorumlusunun nesne kimliğini kullanmak önemlidir. Hizmet sorumlusunun nesne kimliğini almak için Azure CLI'yi açın `az ad sp show --id <Your App ID> --query objectId`ve ardından şu komutu kullanın: . yer tutucuyu `<Your App ID>` uygulama kaydınızın Uygulama Kimliği ile değiştirdiğinizden emin olun.

### <a name="types-of-access-control-lists"></a>Erişim denetimi listeleri türleri

İki tür erişim denetim listesi vardır: *erişim ALA'ları* ve *varsayılan ALA'lar.*

Access ALA'lar bir nesneye erişimi denetler. Dosyaların ve dizinlerin her ikisi de erişim ALA'larına sahiptir.

Varsayılan ALA'lar, bu dizin altında oluşturulan tüm alt öğelerin erişim ALA'larını belirleyen bir dizinle ilişkili ALA'ların şablonlarıdır. Dosyalarda varsayılan ALA'lar yoktur.

Hem erişim ALA'ları hem de varsayılan ALA'lar aynı yapıya sahiptir.

> [!NOTE]
> Bir üst öğedeki varsayılan ACL'yi değiştirmek, zaten var olan alt öğelerin erişim ACL'sini veya varsayılan ACL'sini etkilemez.

### <a name="levels-of-permission"></a>İzin düzeyleri

Kapsayıcı nesnesi üzerindeki izinler **Oku,** **Yaz**ve **Yürüt ,** ve aşağıdaki tabloda gösterildiği gibi dosya ve dizinler üzerinde kullanılabilir:

|            |    Dosya     |   Dizin |
|------------|-------------|----------|
| **Okuma (R)** | Bir dosyanın içeriğini okuyabilir | Dizinin içeriğini listelemek için **Okuma** ve **Yürütme** gerektirir |
| **Yazma (W)** | Bir dosyaya yazabilir veya ekleyebilir | Bir dizinde alt öğeler oluşturmak için **Yazma** ve **Yürüt'e** Gerektirir |
| **Yürütme (X)** | Veri Gölü Depolama Gen2 bağlamında bir şey ifade etmez | Bir dizinin alt öğelerini geçmek için gerekli |

> [!NOTE]
> Yalnızca ALA'lar (RBAC yok) kullanarak izin veriyorsanız, bir güvenlik ilkesine dosyaya okuma veya yazma erişimi vermek için, güvenlik **ilkesine** kapsayıcıya ve dosyaya giden klasörler hiyerarşisindeki her klasöre izin vermeniz gerekir.

#### <a name="short-forms-for-permissions"></a>İzinlerin kısaltmaları

**RWX**, **Okuma + Yazma + Yürütme** için kullanılır. **Okuma=4**, **Yazma=2** ve **Yürütme=1** olup toplamları izinleri temsil eden daha da kısaltılmış bir sayısal biçim mevcuttur. Bazı örnekler aşağıda verilmiştir.

| Sayısal biçim | Kısa biçim |      Anlamı     |
|--------------|------------|------------------------|
| 7            | `RWX`        | Okuma + Yazma + Yürütme |
| 5            | `R-X`        | Okuma + Yürütme         |
| 4            | `R--`        | Okuma                   |
| 0            | `---`        | İzin yok         |

#### <a name="permissions-inheritance"></a>İzinler devralma

Veri Gölü Depolama Gen2 tarafından kullanılan POSIX stili modelinde, bir öğenin izinleri maddenin kendisinde depolanır. Başka bir deyişle, izinler alt öğe zaten oluşturulduktan sonra ayarlanmışsa, bir öğenin izinleri üst öğelerden devralınamaz. İzinler yalnızca alt öğeler oluşturulmadan önce üst öğelerde varsayılan izinler ayarlanmışsa devralınır.

### <a name="common-scenarios-related-to-permissions"></a>İzinlerle ilgili yaygın senaryolar

Aşağıdaki tablo, bir depolama hesabında belirli işlemleri gerçekleştirmek için hangi izinlerin gerekli olduğunu anlamanıza yardımcı olacak bazı yaygın senaryoları listeler.

|    İşlem             |    /    | Oregon/ | Portland/ | Data.txt     |
|--------------------------|---------|----------|-----------|--------------|
| Data.txt'yi okuyun            |   `--X`   |   `--X`    |  `--X`      | `R--`          |
| Data.txt'ye Ek       |   `--X`   |   `--X`    |  `--X`      | `RW-`          |
| Verileri Sil.txt          |   `--X`   |   `--X`    |  `-WX`      | `---`          |
| Data.txt Oluştur          |   `--X`   |   `--X`    |  `-WX`      | `---`          |
| Liste /                   |   `R-X`   |   `---`    |  `---`      | `---`          |
| Liste /Oregon/           |   `--X`   |   `R-X`    |  `---`      | `---`          |
| Liste /Oregon/Portland/  |   `--X`   |   `--X`    |  `R-X`      | `---`          |

> [!NOTE]
> Önceki iki koşul doğru olduğu sürece, dosyaya yazma izinlerinin silinmesi gerekmez.

### <a name="users-and-identities"></a>Kullanıcılar ve kimlikler

Her dosya ve dizin bu kimlikler için farklı izinlere sahiptir:

- Sahip olan kullanıcı
- Sahip olan grup
- Adlandırılmış kullanıcılar
- Adlandırılmış gruplar
- Adlandırılmış hizmet ilkeleri
- Adlandırılmış yönetilen kimlikler
- Diğer tüm kullanıcılar

Kullanıcıların ve grupların kimlikleri, Azure Active Directory (Azure AD) kimlikleridir. Bu nedenle, aksi belirtilmedikçe, Veri Gölü Depolama Gen2 bağlamında bir *kullanıcı*bir Azure REKLAM kullanıcısına, hizmet sorumlusuna, yönetilen kimliğe veya güvenlik grubuna başvurabilir.

#### <a name="the-owning-user"></a>Sahip olan kullanıcı

Öğeyi oluşturan kullanıcı otomatik olarak öğenin sahibi olan kullanıcıdır. Sahip olan kullanıcı şunları yapabilir:

* Sahip olunan bir dosyanın izinlerini değiştirme.
* Sahip olan kullanıcı aynı zamanda hedef grubun bir üyesi oldukça, sahip olunan bir dosyanın sahibi olan grubunu değiştirme.

> [!NOTE]
> Sahibi kullanıcı, bir dosyanın veya dizinin sahibi kullanıcısını *değiştiremez.* Yalnızca süper kullanıcılar bir dosyanın veya dizinin sahibi kullanıcısını değiştirebilir.

#### <a name="the-owning-group"></a>Sahip olan grup

POSIX ALIK'lerinde, her kullanıcı bir *birincil grupla*ilişkilidir. Örneğin, kullanıcı "Alice" "finans" grubuna ait olabilir. Alice birden çok gruba da ait olabilir, ancak bir grup her zaman birincil grup olarak belirlenir. POSIX’te Gamze bir dosya oluşturduğunda o dosyanın sahibi olan grup birincil grubu olarak ayarlanır (bu örnekte "finans" grubudur). Aksi takdirde sahip olan grup, diğer kullanıcılar/gruplar için atanan izinlere benzer şekilde davranır.

##### <a name="assigning-the-owning-group-for-a-new-file-or-directory"></a>Yeni bir dosya veya dizin için sahip olma grubunu atama

* **Örnek 1**: Kök dizini "/". Bu dizin, Bir Veri Gölü Depolama Gen2 kapsayıcı oluşturulduğunda oluşturulur. Bu durumda, sahip olan grup, OAuth kullanılarak yapıldıysa kapsayıcıyı oluşturan kullanıcıya ayarlanır. Kapsayıcı Paylaşılan Anahtar, Hesap SAS veya Hizmet SAS kullanılarak oluşturulursa, sahibi ve sahip grubu **$superuser.**
* **Büyük/Küçük Harf 2** (Diğer tüm servis talebi): Yeni bir öğe oluşturulduğunda, sahip olan grup üst dizinden kopyalanır.

##### <a name="changing-the-owning-group"></a>Sahip grubuna sahip olmak

Sahip olan grup aşağıdakiler tarafından değiştirilebilir:
* Herhangi bir süper kullanıcı.
* Sahip olan kullanıcı aynı zamanda hedef grubun üyesi ise sahip olan kullanıcı.

> [!NOTE]
> Sahip grubu, bir dosyanın veya dizinin AK'larını değiştiremez.  Sahip grubu kök dizini durumunda hesabı oluşturan kullanıcıya ayarlanmış olsa da, yukarıdaki **Örnek 1,** tek bir kullanıcı hesabı sahibi grubu üzerinden izin sağlamak için geçerli değildir. Uygunsa bu izni geçerli bir kullanıcı hesabına atayabilirsiniz.

### <a name="access-check-algorithm"></a>Erişim denetimi algoritması

Aşağıdaki pseudocode depolama hesapları için erişim denetimi algoritmasını temsil eder.

```console
def access_check( user, desired_perms, path ) : 
  # access_check returns true if user has the desired permissions on the path, false otherwise
  # user is the identity that wants to perform an operation on path
  # desired_perms is a simple integer with values from 0 to 7 ( R=4, W=2, X=1). User desires these permissions
  # path is the file or directory
  # Note: the "sticky bit" isn't illustrated in this algorithm
  
# Handle super users.
  if (is_superuser(user)) :
    return True

# Handle the owning user. Note that mask isn't used.
entry = get_acl_entry( path, OWNER )
if (user == entry.identity)
    return ( (desired_perms & entry.permissions) == desired_perms )

# Handle the named users. Note that mask IS used.
entries = get_acl_entries( path, NAMED_USER )
for entry in entries:
    if (user == entry.identity ) :
        mask = get_mask( path )
        return ( (desired_perms & entry.permissions & mask) == desired_perms)

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

#### <a name="the-mask"></a>Maske

Erişim Denetimi Algoritması'nda gösterildiği gibi, maske adlandırılmış kullanıcıların, sahip grubun ve adlandırılmış grupların erişimini sınırlar.  

> [!NOTE]
> Yeni bir Veri Gölü Depolama Gen2 kapsayıcısı için, kök dizinin ("/") erişim ACL'si için maske dizinler için 750 ve dosyalar için 640 olarak varsayılandır. Yalnızca mağaza sistemindeki dosyalarla ilgisi olmadığı için dosyalar X bitini almaz.
>
> Maske, çağrı başına olarak belirtilebilir. Bu, kümeler gibi farklı tüketim sistemlerinin dosya işlemleri için farklı etkili maskelere sahip olmasını sağlar. Belirli bir istekte bir maske belirtilirse, varsayılan maskeyi tamamen geçersiz kılar.

#### <a name="the-sticky-bit"></a>Yapışkan bit

Yapışkan bit bir POSIX konteyner daha gelişmiş bir özelliğidir. Veri Gölü Depolama Gen2 bağlamında, yapışkan bit gerekli olması olası değildir. Özetle, yapışkan bit bir dizinde etkinleştirilmişse, alt öğe yalnızca alt öğenin sahibi kullanıcısı tarafından silinebilir veya yeniden adlandırılabilir.

Yapışkan bit Azure portalında gösterilmez.

### <a name="default-permissions-on-new-files-and-directories"></a>Yeni dosyalar da ve dizinlerde varsayılan izinler

Varolan bir dizin altında yeni bir dosya veya dizin oluşturulduğunda, üst dizindeki varsayılan ACL belirler:

- Bir alt dizini varsayılan ACL ve erişim ACL.
- Alt dosyanın erişim ACL 's (dosyalar varsayılan Bir ACL yok).

#### <a name="umask"></a>umask

Bir dosya veya dizin oluştururken, umask varsayılan ALA'ların alt öğeüzerinde nasıl ayarlanır değişiklik yapmak için kullanılır. umask, **kullanıcıya sahip**olmak , **grup sahibi**olmak ve **diğer**.

Azure Veri Gölü Depolama Gen2 için umask 007 olarak ayarlanmış sabit bir değer. Bu değer şu şekilde tercüme edilir:

| umask bileşeni     | Sayısal biçim | Kısa biçim | Anlamı |
|---------------------|--------------|------------|---------|
| umask.owning_user   |    0         |   `---`      | Kullanıcısahibi olmak için, üst öğenin varsayılan ACL'sini çocuğun erişimine kopyalayarak ACL | 
| umask.owning_group  |    0         |   `---`      | Sahip grubu için, üst ebeveynin varsayılan ACL'sini çocuğun erişimine kopyalayın | 
| umask.other         |    7         |   `RWX`      | Diğer olarak, çocuğun erişimi acl tüm izinleri kaldırın |

Azure Veri Gölü Depolama Gen2 tarafından kullanılan umask değeri, varsayılan ACL'nin gösterdiğine bakılmaksızın, **diğer** değerin varsayılan olarak yeni çocuklara asla aktarılamayacağı anlamına gelir. 

Aşağıdaki pseudocode, bir alt öğe için AK'lar oluşturulurken umask'ın nasıl uygulandığını gösterir.

```console
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

## <a name="common-questions-about-acls-in-data-lake-storage-gen2"></a>Data Lake Storage Gen2'deki Aç'lar hakkında sık sorulan sorular

### <a name="do-i-have-to-enable-support-for-acls"></a>ACL desteğini etkinleştirmem gerekiyor mu?

Hayır. Hiyerarşik Ad Alanı (HNS) özelliği açık olduğu sürece bir depolama hesabı için ACD'ler üzerinden erişim denetimi etkinleştirilir.

HNS kapalıysa, Azure RBAC yetkilendirme kuralları yine de geçerlidir.

### <a name="what-is-the-best-way-to-apply-acls"></a>ALA'ları uygulamanın en iyi yolu nedir?

Azure AD güvenlik gruplarını her zaman ALA'larda atanan anapara olarak kullanın. Tek tek kullanıcıları veya hizmet ilkelerini doğrudan atama fırsatına karşı direnin. Bu yapıyı kullanmak, tüm dizin yapısına Yeniden Uygulama nız gerekmeden kullanıcı veya hizmet ilkelerini eklemenize ve kaldırmanıza olanak sağlar. ) Bunun yerine, bunları ilgili Azure AD güvenlik grubuna eklemeniz veya kaldırmanız yeterlidir. ACL'lerin kalıtsal olmadığını ve bu nedenle ACL'lerin yeniden uygulanması için her dosya ve alt dizinde ACL'nin güncellenmesi gerektiğini unutmayın. 

### <a name="which-permissions-are-required-to-recursively-delete-a-directory-and-its-contents"></a>Bir dizin ve içeriğini yinelemeli olarak silmek için hangi izinler gereklidir?

- Arayan 'süper kullanıcı' izinleri vardır,

Veya

- Üst dizinin Yazma + Yürüt izinleri olmalıdır.
- Silinecek dizin ve içindeki her dizin, Okuma + Yazma + Yürütme izinleri gerektirir.

> [!NOTE]
> Dizinlerde dosyaları silmek için Yazma izinlerine gerek yoktur. Ayrıca, kök dizini "/" asla silinemez.

### <a name="who-is-the-owner-of-a-file-or-directory"></a>Bir dosyanın veya dizinin sahibi kimdir?

Bir dosyanın veya dizinin yaratıcısı sahibi olur. Kök dizini söz konusu olduğunda, bu kapsayıcıyı oluşturan kullanıcının kimliğidir.

### <a name="which-group-is-set-as-the-owning-group-of-a-file-or-directory-at-creation"></a>Hangi grup, bir dosyanın veya dizin oluşturmanın sahibi grubu olarak ayarlanır?

Sahip grubu, yeni dosya veya dizinin oluşturulduğu ana dizinin sahip olduğu gruptan kopyalanır.

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a>Ben bir dosyanın sahibi kullanıcı ama ihtiyacım olan RWX izinleri yok. Ne yapmalıyım?

Sahip olan kullanıcı kendisine gerekli olan her türlü RWX iznini vermek için dosyanın izinlerini değiştirebilir.

### <a name="why-do-i-sometimes-see-guids-in-acls"></a>Neden bazen ALA'larda GUID'ler görüyorum?

Giriş bir kullanıcıyı temsil ediyorsa ve bu kullanıcı artık Azure AD'da yoksa GUID gösterilir. Bu genellikle, kullanıcı şirketten ayrıldığında veya Azure AD’de kullanıcının hesabı silindiğinde gerçekleşir. Ayrıca, hizmet ilkelerinin ve güvenlik gruplarının bunları tanımlamak için Kullanıcı Adı (UPN) yoktur ve bu nedenle OID özniteliği (yol gösterici) ile temsil edilirler.

### <a name="how-do-i-set-acls-correctly-for-a-service-principal"></a>Bir hizmet müdürü için ALA'ları nasıl doğru ayarlıyorum?

Hizmet ilkeleri için ABM'ler tanımladığınızda, oluşturduğunuz uygulama kaydı için *hizmet sorumlusunun* Nesne Kimliğini (OID) kullanmanız önemlidir. Kayıtlı uygulamaların belirli Azure AD kiracısında ayrı bir hizmet ilkesi olduğunu unutmayın. Kayıtlı uygulamaların Azure portalında görünen bir OID'si vardır, ancak *hizmet sorumlusunun* başka bir (farklı) OID'si vardır.

Bir uygulama kaydına karşılık gelen hizmet ilkesi için OID almak `az ad sp show` için komutu kullanabilirsiniz. Uygulama Kimliğini parametre olarak belirtin. Burada App ID = 18218b12-1895-43e9-ad80-6e8fc1ea88ce ile bir uygulama kaydı karşılık gelen hizmet ilkesi için OID elde edilmesi ne ilişkin bir örnek. Azure CLI'de aşağıdaki komutu çalıştırın:

```azurecli
az ad sp show --id 18218b12-1895-43e9-ad80-6e8fc1ea88ce --query objectId
```

OID görüntülenir.

Hizmet sorumlusu için doğru OID'ye sahip olduğunuzda, OID'yi eklemek ve OID için uygun izinleri atamak için Depolama Gezgini **Yönet Access** sayfasına gidin. **Kaydet'i**seçtiğinizden emin olun.

### <a name="does-data-lake-storage-gen2-support-inheritance-of-acls"></a>Data Lake Storage Gen2 ACD'lerin devrini destekliyor mu?

Azure RBAC atamaları devralır. Atamalar abonelik, kaynak grubu ve depolama hesabı kaynaklarından kapsayıcı kaynağına akar.

ALA'lar devralınmaz. Ancak, varsayılan ALA'lar alt dizinler ve üst dizini altında oluşturulan dosyalar için ALA'lar ayarlamak için kullanılabilir. 

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

* [Azure Veri Gölü Depolama Gen2'ye Genel Bakış](../blobs/data-lake-storage-introduction.md)
