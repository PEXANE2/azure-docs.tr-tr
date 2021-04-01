---
title: Azure SQL Edge 'i güvenli hale getirme
description: Azure SQL Edge 'de güvenlik hakkında bilgi edinin
keywords: SQL Edge, güvenlik
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 09/22/2020
ms.openlocfilehash: 56d5eb981aa02d9da83973d49e8df79fcd9c7e9c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "95021023"
---
# <a name="securing-azure-sql-edge"></a>Azure SQL Edge güvenliğini sağlama

Sektörlerde IoT ve Edge bilgi işlem benimseme artışının artması sayesinde cihaz sayısında ve bu cihazlardan oluşturulan verilerde bir artış vardır. Artan veri hacmi ve cihaz uç noktaları sayısı, verilerin ve cihazların güvenliği açısından önemli bir zorluk oluşturur. 

Azure SQL Edge, SQL Server veritabanları içindeki IoT verilerinin güvenliğini daha kolay hale getirmek için çok çeşitli özellikler ve yetenekler sunar. Azure SQL Edge, Microsoft SQL Server ve Azure SQL 'i destekleyen aynı SQL altyapısı kullanılarak oluşturulmuştur. Bu, aynı güvenlik ilkelerini ve uygulamaları buluttan kenara genişletmeyi kolaylaştırır.

Azure SQL Edge dağıtımlarının güvenliğini sağlamak Microsoft SQL Server ve Azure SQL gibi dört alan içeren bir dizi adım olarak görüntülenebilir: platform, kimlik doğrulama, nesneler (veriler dahil) ve sisteme erişen uygulamalar. 

## <a name="platform-and-system-security"></a>Platform ve sistem güvenliği

Azure SQL Edge platformu, fiziksel Docker konağını, konaktaki işletim sistemini ve fiziksel cihazı uygulamalara ve istemcilere bağlayan ağ sistemlerini içerir. 

Platform güvenliğini uygulamak, yetkisiz kullanıcıları ağdan tutmaya başlar. En iyi uygulamalardan bazıları şunlardır, ancak bunlarla sınırlı değildir:
- Kuruluş güvenlik ilkesini sağlamak için güvenlik duvarı kuralları uygulama. 
- Fiziksel aygıttaki işletim sisteminde en son güvenlik güncelleştirmelerinin uygulanmış olduğundan emin olun. 
- Azure SQL Edge için kullanılan ana bilgisayar bağlantı noktalarını belirtme ve kısıtlama
- Doğru erişim denetiminin Azure SQL Edge verilerini barındıran tüm veri birimlerine uygulandığından emin olmak. 

Azure SQL Edge ağ protokolleri ve TDS uç noktaları hakkında daha fazla bilgi için [ağ protokolleri ve tds uç noktaları](/previous-versions/sql/sql-server-2008-r2/ms191220(v=sql.105))konusuna bakın.

## <a name="authentication-and-authorization"></a>Kimlik doğrulaması ve yetkilendirme 

### <a name="authentication"></a>Kimlik Doğrulaması  
Kimlik doğrulama, kullanıcının talep ettikleri kim olduğunu kanıtlama işlemidir. Azure SQL Edge Şu anda yalnızca `SQL Authentication` mekanizmayı desteklemektedir.

- *SQL kimlik doğrulaması*:

    SQL kimlik doğrulaması, Kullanıcı adı ve parola kullanarak Azure SQL Edge 'e bağlanılırken bir kullanıcının kimlik doğrulamasını ifade eder. SQL Edge dağıtımı sırasında SQL **sa** oturum açma parolasının belirtilmesi gerekir. Bundan sonra, ek SQL oturum açmaları ve kullanıcılar, kullanıcıların Kullanıcı adı ve parola kullanarak bağlanmasına olanak tanıyan Sunucu Yöneticisi tarafından oluşturulabilir.

    SQL Edge 'de oturum açma ve Kullanıcı oluşturma ve yönetme hakkında daha fazla bilgi için bkz. [oturum oluşturma](/sql/relational-databases/security/authentication-access/create-a-login) ve [veritabanı kullanıcısı oluşturma](/sql/relational-databases/security/authentication-access/create-a-database-user).

### <a name="authorization"></a>Yetkilendirme   

Yetkilendirme, Azure SQL Edge 'deki bir veritabanı içindeki bir kullanıcıya atanan izinlere başvurur ve kullanıcının ne yapmasına izin verileceğini belirler. İzinler, [veritabanı rollerine](/sql/relational-databases/security/authentication-access/database-level-roles) Kullanıcı hesapları eklenerek ve bu rollere veritabanı düzeyi izinleri atanarak veya kullanıcıya belirli [nesne düzeyi izinleri](/sql/relational-databases/security/permissions-database-engine)verilerek denetlenir. Daha fazla bilgi için bkz. [Oturumlar ve kullanıcılar](../azure-sql/database/logins-create-manage.md).

En iyi uygulama olarak, gerektiğinde özel roller oluşturun. Kullanıcıları, iş işlevlerini yapmak için gereken en düşük ayrıcalıklara sahip olan role ekleyin. İzinleri doğrudan kullanıcılara atamayın. Sunucu Yöneticisi hesabı, kapsamlı izinlere sahip ve yalnızca yönetim görevleri olan birkaç kullanıcıya verilmesi gereken yerleşik db_owner rolünün bir üyesidir. Uygulamalar için, çağrılan modülün Yürütme bağlamını belirtmek için [execute as](/sql/t-sql/statements/execute-as-clause-transact-sql) kullanın veya sınırlı Izinlerle [uygulama rollerini](/sql/relational-databases/security/authentication-access/application-roles) kullanın. Bu uygulama, veritabanına bağlanan uygulamanın uygulama için gereken en düşük ayrıcalıklara sahip olmasını sağlar. Bu en iyi uygulamaları takip etmek, görevlerin ayrılmasını de çok daha da fazla.

## <a name="database-object-security"></a>Veritabanı nesnesi güvenliği

Asıl kişiler, gruplar ve SQL Edge 'e erişim izni verilen süreçlerdir. "Securables", veritabanının içerdiği sunucu, veritabanı ve nesnelerdir. Her birinin yüzey alanını azaltmaya yardımcı olmak için yapılandırılabilecek bir izinler kümesi vardır. Aşağıdaki tabloda sorumlular ve securables hakkında bilgiler yer almaktadır.

|Hakkında bilgi için|Bkz.|  
|---------------------------|---------|  
|Sunucu ve veritabanı kullanıcıları, rolleri ve süreçler|[Asıl veritabanı altyapısı](/sql/relational-databases/security/authentication-access/principals-database-engine)|  
|Sunucu ve veritabanı nesneleri güvenliği|[Securables](/sql/relational-databases/security/securables)|
| &nbsp; | &nbsp; |

### <a name="encryption-and-certificates"></a>Şifreleme ve sertifikalar  
 
Şifreleme, erişim denetimi sorunlarını çözmüyor. Ancak, erişim denetimlerinin atlanacağı nadir oluşumda veri kaybını sınırlayarak güvenliği geliştirir. Örneğin, veritabanı ana bilgisayar yanlış yapılandırılmış ise ve kötü niyetli bir Kullanıcı kredi kartı numaraları gibi hassas verileri alırsa, bu çalınmış bilgiler, şifrelendiyse, bu şekilde erişilebilir olmayabilir. Aşağıdaki tabloda, Azure SQL Edge 'de şifreleme hakkında daha fazla bilgi yer almaktadır.  
  
|Hakkında bilgi için|Bkz.|  
|---------------------------|---------|  
|Güvenli bağlantıları uygulama|[Bağlantıları şifreleme](/sql/linux/sql-server-linux-encrypted-connections)|  
|Şifreleme işlevleri|[&#40;Transact-SQL&#41;şifreleme Işlevleri ](/sql/t-sql/functions/cryptographic-functions-transact-sql)|
|Bekleyen veri şifrelemesi|[Saydam Veri Şifrelemesi](/sql/relational-databases/security/encryption/transparent-data-encryption)|
|Always Encrypted|[Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine)|
| &nbsp; | &nbsp; |

> [!NOTE]
> [Linux üzerinde SQL Server](/sql/linux/sql-server-linux-security-overview) açıklanan güvenlik sınırlamaları Azure SQL Edge için de geçerlidir. 


> [!NOTE]
> Azure SQL Edge, MSSQL-conf yardımcı programını içermez. Şifreleme ile ilgili yapılandırma dahil olmak üzere tüm yapılandırmaların [MSSQL. conf dosyası](configure.md#configure-by-using-an-mssqlconf-file) veya [ortam değişkenleri](configure.md#configure-by-using-environment-variables)aracılığıyla gerçekleştirilmesi gerekir. 


Azure SQL ve Microsoft SQL Server benzer şekilde, Azure SQL Edge, nesne ve bağlantı güvenliğini geliştirmek için sertifika oluşturmak ve kullanmak için aynı mekanizmayı sağlar. Daha fazla bilgi için bkz. [sertifika oluşturma (Transact-SQL)](/sql/t-sql/statements/create-certificate-transact-sql).


## <a name="application-security"></a>Uygulama güvenliği

### <a name="client-programs"></a>İstemci programları

Azure SQL Edge güvenlik en iyi uygulamaları, güvenli istemci uygulamalarının yazılmasını içerir. İstemci uygulamalarının Ağ katmanında güvenli hale getirilmesine yardımcı olma hakkında daha fazla bilgi için bkz. [Istemci ağ yapılandırması](/sql/database-engine/configure-windows/client-network-configuration).

### <a name="security-catalog-views-and-functions"></a>Güvenlik Kataloğu görünümleri ve işlevleri  
Güvenlik bilgileri, performans ve yardımcı için iyileştirilmiş çeşitli görünümlerde ve işlevlerde kullanıma sunulur. Aşağıdaki tabloda, Azure SQL Edge 'deki güvenlik görünümleri ve işlevleri hakkında bilgiler yer almaktadır.  
  
|İşlevler ve görünümler|Bağlantılar|  
|---------------------------|---------|  
|Veritabanı düzeyi ve sunucu düzeyi izinler, sorumlular, roller vb. hakkında bilgi döndüren Güvenlik Kataloğu görünümleri. Ayrıca, şifreleme anahtarları, sertifikalar ve kimlik bilgileri hakkında bilgi sağlayan Katalog görünümleri vardır.|[Güvenlik Kataloğu görünümleri &#40;Transact-SQL&#41;](/sql/relational-databases/system-catalog-views/security-catalog-views-transact-sql)|  
|Geçerli Kullanıcı, izinler ve şemalar hakkındaki bilgileri döndüren güvenlik işlevleri.|[Güvenlik Işlevleri &#40;Transact-SQL&#41;](/sql/t-sql/functions/security-functions-transact-sql)|  
|Güvenlik dinamik yönetim görünümleri.|[Güvenlik ile Ilgili dinamik yönetim görünümleri ve Işlevleri &#40;Transact-SQL&#41;](/sql/relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql)|  
| &nbsp; | &nbsp; |

### <a name="auditing"></a>Denetim 

Azure SQL Edge, SQL Server ile aynı denetim mekanizmalarını sağlar. Daha fazla bilgi için bkz. [SQL Server denetim (veritabanı altyapısı)](/sql/relational-databases/security/auditing/sql-server-audit-database-engine).


## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik özellikleriyle çalışmaya başlama](/sql/linux/sql-server-linux-security-get-started)
- [Azure SQL Edge 'i kök olmayan kullanıcı olarak çalıştırma](configure.md#run-azure-sql-edge-as-non-root-user)
- [IoT için Azure Güvenlik Merkezi](../defender-for-iot/overview.md)