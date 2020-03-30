---
title: Azure Blockchain Workbench veritabanı görünümleri
description: Kullanılabilir Azure Blockchain Workbench Preview SQL DB veritabanı görünümleri genel bakış.
ms.date: 09/05/2019
ms.topic: article
ms.reviewer: mmercuri
ms.openlocfilehash: 585084b4c85c48533bdad96d4f99813ef2e418b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74325982"
---
# <a name="azure-blockchain-workbench-database-views"></a>Azure Blockchain Workbench veritabanı görünümleri

Azure Blockchain Workbench Preview, dağıtılmış genel muhasebelerden *zincirdışı* bir SQL DB veritabanına veri sağlar. Zincir dışı veritabanı, blockchain verileriyle etkileşim kurmak için SQL ve [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)gibi varolan araçları kullanmayı mümkün kılar.

Azure Blockchain Workbench, sorgularınızı gerçekleştirirken yararlı olacak verilere erişim sağlayan bir veritabanı görünümü kümesi sağlar. Bu görünümler, hızlı bir şekilde raporlar, analizler oluşturmaya başlamayı ve aksi takdirde varolan araçlarla ve veritabanı personelini yeniden eğitmek zorunda kalmadan blockchain verilerini tüketmeyi kolaylaştırmak için büyük ölçüde denormalize edilir.

Bu bölümde veritabanı görünümleri ve içerdikleri veriler genel bir bakış içerir.

> [!NOTE]
> Bu görünümler dışında veritabanında bulunan veritabanı tablolarının doğrudan kullanımı, mümkün olsa da, desteklenmez.
>

## <a name="vwapplication"></a>vwApplication

Bu görünüm, Azure Blockchain Workbench'e yüklenen **Uygulamalar** hakkında ayrıntılı bilgi sağlar.

| Adı                             | Tür          | Null Olabilir | Açıklama                                                                                                                                                                                                                                                   |
|----------------------------------|---------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationID                    | int           | Hayır          | Uygulama için benzersiz bir tanımlayıcı |
| ApplicationName                  | nvarchar(50)  | Hayır          | Uygulamanın adı |
| Uygulama Açıklaması           | nvarchar(255) | Evet         | Uygulamanın açıklaması |
| UygulamaDisplayName           | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenecek ad |
| Uygulama Etkin               | bit           | Hayır          | Uygulamanın şu anda etkin olup olmadığını tanımlar<br /> **Not:** Bir uygulama veritabanına devre dışı bırakılmış olarak yansıtılabilir olsa da, ilişkili sözleşmeler blockchain'de kalır ve bu sözleşmelere ilişkin veriler veritabanında kalır. |
| UploadedDtTm                     | datetime2(7)  | Hayır          | Sözleşmenin yüklendiği tarih ve saat |
| UploadedByUserId                 | int           | Hayır          | Uygulamayı yükleyen kullanıcının kimliği |
| UploadedByUserExternalId         | nvarchar(255) | Hayır          | Uygulamayı yükleyen kullanıcının dış tanımlayıcısı. Varsayılan olarak, bu kimlik konsorsiyumun Azure Etkin Dizini'ndeki kullanıcıdır.                                                                                                |
| UploadedByUserProvisioningStatus | int           | Hayır          | Kullanıcı için sağlama işleminin geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 - Kullanıcı API tarafından oluşturuldu<br />1 - Bir anahtar veritabanında kullanıcı ile ilişkili olmuştur<br />2 - Kullanıcı tam olarak                         |
| UploadedByUserFirstName          | nvarchar(50)  | Evet         | Sözleşmeyi yükleyen kullanıcının ilk adı |
| UploadedByUserLastName           | nvarchar(50)  | Evet         | Sözleşmeyi yükleyen kullanıcının soyadı |
| UploadedByUserEmailAddress       | nvarchar(255) | Evet         | Sözleşmeyi yükleyen kullanıcının e-posta adresi |

## <a name="vwapplicationrole"></a>vwApplicationRole

Bu görünüm, Azure Blockchain Workbench uygulamalarında tanımlanan rollerhakkında ayrıntılı bilgi sağlar.

Bir *Varlık Transferi* uygulamasında, örneğin *Alıcı* ve *Satıcı* rolleri gibi roller tanımlanabilir.

| Adı                   | Tür             | Null Olabilir | Açıklama                                       |
|------------------------|------------------|-------------|---------------------------------------------------|
| ApplicationID          | int              | Hayır          | Uygulama için benzersiz bir tanımlayıcı           |
| ApplicationName        | nvarchar(50)     | Hayır          | Uygulamanın adı                       |
| Uygulama Açıklaması | nvarchar(255)    | Evet         | Uygulamanın açıklaması                  |
| UygulamaDisplayName | nvarchar(255)    | Hayır          | Kullanıcı arabiriminde görüntülenecek ad      |
| Roleıd                 | int              | Hayır          | Uygulamada bir rol için benzersiz bir tanımlayıcı |
| RoleName               | nvarchar50)      | Hayır          | Rolün adı                              |
| RoleDescription        | açıklama(255) | Evet         | Rolün açıklaması                         |

## <a name="vwapplicationroleuser"></a>vwApplicationRoleUser

Bu görünüm, Azure Blockchain Workbench uygulamalarında tanımlanan roller ve bunlarla ilişkili kullanıcılar hakkında ayrıntılı bilgi sağlar.

Bir *Varlık Transferi* uygulamasında, örneğin, *John Smith* *Alıcı* rolüyle ilişkili olabilir.

| Adı                       | Tür          | Null Olabilir | Açıklama                                                                                                                                                                                                                           |
|----------------------------|---------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationID              | int           | Hayır          | Uygulama için benzersiz bir tanımlayıcı                                                                                                                                                                                               |
| ApplicationName            | nvarchar(50)  | Hayır          | Uygulamanın adı                                                                                                                                                                                                           |
| Uygulama Açıklaması     | nvarchar(255) | Evet         | Uygulamanın açıklaması                                                                                                                                                                                                      |
| UygulamaDisplayName     | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenecek ad                                                                                                                                                                                          |
| UygulamaRoleid          | int           | Hayır          | Uygulamada bir rol için benzersiz bir tanımlayıcı                                                                                                                                                                                     |
| UygulamaRol Adı        | nvarchar50)   | Hayır          | Rolün adı                                                                                                                                                                                                                  |
| UygulamaRol Tanımı | nvarchar(255) | Evet         | Rolün açıklaması                                                                                                                                                                                                             |
| UserId                     | int           | Hayır          | Rol ile ilişkili kullanıcının kimliği |
| UserExternalId             | nvarchar(255) | Hayır          | Rolile ilişkili kullanıcı için dış tanımlayıcı. Varsayılan olarak, bu kimlik konsorsiyumun Azure Etkin Dizini'ndeki kullanıcıdır.                                                                     |
| UserProvisioningStatus     | int           | Hayır          | Kullanıcı için sağlama işleminin geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 - Kullanıcı API tarafından oluşturuldu<br />1 - Bir anahtar veritabanında kullanıcı ile ilişkili olmuştur<br />2 - Kullanıcı tam olarak |
| KullanıcıAdı              | nvarchar(50)  | Evet         | Rolile ilişkili kullanıcının ilk adı |
| Kullanıcı Adı               | nvarchar(255) | Evet         | Rolile ilişkili kullanıcının soyadı |
| UserEmailAddress           | nvarchar(255) | Evet         | Rol ile ilişkili kullanıcının e-posta adresi |

## <a name="vwconnectionuser"></a>vwConnectionUser

Bu görünüm, Azure Blockchain Workbench'te tanımlanan bağlantılar ve bunlarla ilişkili kullanıcılar hakkında ayrıntılı bilgi sağlar. Her bağlantı için bu görünüm aşağıdaki verileri içerir:

-   İlişkili genel muhasebe ayrıntıları
-   İlişkili kullanıcı bilgileri

| Adı                     | Tür          | Null Olabilir | Açıklama                                                                                                                                                                                                                           |
|--------------------------|---------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Connectionıd             | int           | Hayır          | Azure Blockchain Workbench'te bağlantı için benzersiz tanımlayıcı |
| BağlantıEndpointUrl    | nvarchar(50)  | Hayır          | Bağlantı için uç nokta url'si |
| ConnectionFundingAccount | nvarchar(255) | Evet         | Varsa, bir bağlantıyla ilişkili finansman hesabı |
| Genel Muhasebe                 | int           | Hayır          | Genel muhasebe için benzersiz tanımlayıcı |
| LedgerName               | nvarchar(50)  | Hayır          | Genel muhasebenin adı |
| LedgerDisplayName        | nvarchar(255) | Hayır          | Kullanıcı Arabirimi'nde görüntülenecek genel muhasebenin adı |
| UserId                   | int           | Hayır          | Bağlantıyla ilişkili kullanıcının kimliği |
| UserExternalId           | nvarchar(255) | Hayır          | Bağlantıyla ilişkili kullanıcıiçin dış tanımlayıcı. Varsayılan olarak, bu kimlik konsorsiyumun Azure Etkin Dizini'ndeki kullanıcıdır. |
| UserProvisioningStatus   | int           | Hayır          |Kullanıcı için sağlama işleminin geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 - Kullanıcı API tarafından oluşturuldu<br />1 - Bir anahtar veritabanında kullanıcı ile ilişkili olmuştur<br />2 - Kullanıcı tam olarak |
| KullanıcıAdı            | nvarchar(50)  | Evet         | Bağlantıyla ilişkili kullanıcının ilk adı |
| Kullanıcı Adı             | nvarchar(255) | Evet         | Bağlantıyla ilişkili kullanıcının soyadı |
| UserEmailAddress         | nvarchar(255) | Evet         | Bağlantıyla ilişkili kullanıcının e-posta adresi |

## <a name="vwcontract"></a>vwSözleşme

Bu görünüm, dağıtılan sözleşmeler hakkında ayrıntılı bilgi sağlar. Her sözleşme için bu görünüm aşağıdaki verileri içerir:

-   İlişkili uygulama tanımı
-   İlişkili iş akışı tanımı
-   İşlev için ilişkili genel muhasebe uygulaması
-   Eylemi başlatan kullanıcıiçin ayrıntılar
-   Blockchain blok ve işlem ile ilgili ayrıntılar

| Adı                                     | Tür           | Null Olabilir | Açıklama                                                                                                                                                                                                                                                   |
|------------------------------------------|----------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Connectionıd                             | int            | Hayır          | Azure Blockchain Workbench'te bağlantı için benzersiz tanımlayıcı.                                                                                                                                                                                         |
| BağlantıEndpointUrl                    | nvarchar(50)   | Hayır          | Bağlantı için uç nokta url'si |
| ConnectionFundingAccount                 | nvarchar(255)  | Evet         | Varsa, bir bağlantıyla ilişkili finansman hesabı |
| Genel Muhasebe                                 | int            | Hayır          | Genel muhasebe için benzersiz tanımlayıcı |
| LedgerName                               | nvarchar(50)   | Hayır          | Genel muhasebenin adı |
| LedgerDisplayName                        | nvarchar(255)  | Hayır          | Kullanıcı Arabirimi'nde görüntülenecek genel muhasebenin adı |
| ApplicationID                            | int            | Hayır          | Uygulama için benzersiz bir tanımlayıcı |
| ApplicationName                          | nvarchar (50)  | Hayır          | Uygulamanın adı |
| UygulamaDisplayName                   | nvarchar (255) | Hayır          | Kullanıcı arabiriminde görüntülenecek ad |
| Uygulama Etkin                       | bit            | Hayır          | Uygulamanın şu anda etkin olup olmadığını tanımlar.<br /> **Not:** Bir uygulama veritabanına devre dışı bırakılmış olarak yansıtılabilir olsa da, ilişkili sözleşmeler blockchain'de kalır ve bu sözleşmelere ilişkin veriler veritabanında kalır.  |
| İş Akışı Kimliği                               | int            | Hayır          | Sözleşmeyle ilişkili iş akışı için benzersiz bir tanımlayıcı |
| İş AkışıAdı                             | nvarchar(50)   | Hayır          | Bir sözleşmeyle ilişkili iş akışının adı |
| İş AkışıDisplayName                      | nvarchar(255)  | Hayır          | Kullanıcı arabiriminde görüntülenen sözleşmeyle ilişkili iş akışının adı |
| İş Akışı Açıklama                      | nvarchar(255)  | Evet         | Bir sözleşmeyle ilişkili iş akışının açıklaması |
| ContractCodeId                           | int            | Hayır          | Sözleşmeyle ilişkili sözleşme kodu için benzersiz bir tanımlayıcı |
| SözleşmeDosya Adı                         | int            | Hayır          | Bu iş akışı için akıllı sözleşme kodunu içeren dosyanın adı. |
| SözleşmeUploadedDtTm                     | int            | Hayır          | Sözleşme kodunun yüklendiği tarih ve saat |
| Sözleşmeli Kimlik                               | int            | Hayır          | Sözleşme için benzersiz tanımlayıcı |
| SözleşmeSağlama Durumu               | int            | Hayır          | Sözleşme için sağlama sürecinin geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 - Sözleşme veritabanında API tarafından oluşturuldu<br />1 - Sözleşme deftere<br />2 - Sözleşme deftere başarıyla dağıtılmıştır<br />3 veya 4 - Sözleşme genel muhasebeye dağıtılmadı<br />5 - Sözleşme deftere başarıyla dağıtıldı <br /><br />Sürüm 1.5'ten başlayarak 0 ile 5 arasında değerler desteklenir. Geçerli sürümde geriye dönük uyumluluk için, yalnızca 0 ile 2 arasında değerleri destekleyen **vwContractV0'ı** görüntüleyin. |
| Sözleşmeli LedgerIdentifier                 | nvarchar (255) |             | Sözleşmeyi dağıtan kullanıcının e-posta adresi |
| SözleşmeDeployedByUserId                 | int            | Hayır          | Sözleşmeyi dağıtan kullanıcı için harici bir tanımlayıcı. Varsayılan olarak, bu kimlik kullanıcı için Azure Etkin Dizin Kimliğini temsil eden kılavuzdur.                                                                                                          |
| SözleşmeDeployedByUserExternalId         | nvarchar(255)  | Hayır          | Sözleşmeyi dağıtan kullanıcı için harici bir tanımlayıcı. Varsayılan olarak, bu kimlik kullanıcı için Azure Etkin Dizin Kimliğini temsil eden kılavuzdur.                                                                                                         |
| SözleşmeDeployedByUserProvisioningStatus | int            | Hayır          | Kullanıcı için sağlama işleminin geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 – kullanıcı API tarafından oluşturuldu<br />1 - Bir anahtar veritabanında kullanıcı ile ilişkili olmuştur <br />2 - Kullanıcı tam olarak                     |
| SözleşmeDeployedByUserFirstName          | nvarchar(50)   | Evet         | Sözleşmeyi dağıtan kullanıcının ilk adı |
| SözleşmeDeployedByUserLastName           | nvarchar(255)  | Evet         | Sözleşmeyi dağıtan kullanıcının soyadı |
| SözleşmeDeployedByUserEmailAddress       | nvarchar(255)  | Evet         | Sözleşmeyi dağıtan kullanıcının e-posta adresi |

## <a name="vwcontractaction"></a>vwContractAction

Bu görünüm, sözleşmelerde gerçekleştirilen eylemlerle ilgili bilgilerin çoğunluğunu temsil eder ve yaygın raporlama senaryolarını kolayca kolaylaştırmak üzere tasarlanmıştır. Alınan her eylem için bu görünüm aşağıdaki verileri içerir:

-   İlişkili uygulama tanımı
-   İlişkili iş akışı tanımı
-   İlişkili akıllı sözleşme fonksiyonu ve parametre tanımı
-   İşlev için ilişkili genel muhasebe uygulaması
-   Parametreler için sağlanan özel örnek değerleri
-   Eylemi başlatan kullanıcıiçin ayrıntılar
-   Blockchain blok ve işlem ile ilgili ayrıntılar

| Adı                                     | Tür          | Null Olabilir | Açıklama                                                                                                                                                                                                                                                                                                    |
|------------------------------------------|---------------|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationID                            | int           | Hayır          | Uygulama için benzersiz bir tanımlayıcı |
| ApplicationName                          | nvarchar(50)  | Hayır          | Uygulamanın adı |
| UygulamaDisplayName                   | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenecek ad |
| Uygulama Etkin                       | bit           | Hayır          | Bu alan, uygulamanın şu anda etkin olup olmadığını tanımlar. Not – Bir uygulama veritabanına devre dışı bırakılmış olarak yansıtılabilse de, ilişkili sözleşmeler blockchain'de kalır ve bu sözleşmelere ilişkin veriler veritabanında kalır.                                                  |
| İş Akışı Kimliği                               | int           | Hayır          | İş akışı için benzersiz bir tanımlayıcı |
| İş AkışıAdı                             | nvarchar(50)  | Hayır          | İş akışının adı |
| İş AkışıDisplayName                      | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenecek iş akışının adı |
| İş Akışı Açıklama                      | nvarchar(255) | Evet         | İş akışının açıklaması |
| Sözleşmeli Kimlik                               | int           | Hayır          | Sözleşme için benzersiz bir tanımlayıcı |
| SözleşmeSağlama Durumu               | int           | Hayır          | Sözleşme için sağlama sürecinin geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 - Sözleşme veritabanında API tarafından oluşturuldu<br />1 - Sözleşme deftere<br />2 - Sözleşme deftere başarıyla dağıtılmıştır<br />3 veya 4 - Sözleşme genel muhasebeye dağıtılmadı<br />5 - Sözleşme deftere başarıyla dağıtıldı <br /><br />Sürüm 1.5'ten başlayarak 0 ile 5 arasında değerler desteklenir. Geçerli sürümde geriye dönük uyumluluk için, yalnızca 0 ile 2 arasında değerleri destekleyen **vwContractActionV0'ü** görüntüleyin. |
| ContractCodeId                           | int           | Hayır          | Sözleşmenin kod uygulaması için benzersiz bir tanımlayıcı |
| Sözleşmeli LedgerIdentifier                 | nvarchar(255) | Evet         | Belirli bir dağıtılmış genel muhasebe için akıllı sözleşmenin dağıtılan sürümüyle ilişkili benzersiz bir tanımlayıcı. Örneğin, Ethereum. |
| SözleşmeDeployedByUserId                 | int           | Hayır          | Sözleşmeyi dağıtan kullanıcının benzersiz tanımlayıcısı |
| SözleşmeDeployedByUserFirstName          | nvarchar(50)  | Evet         | Sözleşmeyi dağıtan kullanıcının adı |
| SözleşmeDeployedByUserLastName           | nvarchar(255) | Evet         | Sözleşmeyi dağıtan kullanıcının soyadı |
| SözleşmeDeployedByUserExternalId         | nvarchar(255) | Hayır          | Sözleşmeyi dağıtan kullanıcının dış tanımlayıcısı. Varsayılan olarak, bu kimlik, Azure Etkin Dizini konsorsiyumunda kimliklerini temsil eden kılavuzdur.                                                                                                                                                |
| SözleşmeDeployedByUserEmailAddress       | nvarchar(255) | Evet         | Sözleşmeyi dağıtan kullanıcının e-posta adresi |
| İş AkışıFonksiyonId                       | int           | Hayır          | İş akışı işlevi için benzersiz bir tanımlayıcı |
| İş AkışıİşAdı                     | nvarchar(50)  | Hayır          | İşlevin adı |
| İş AkışıFunctionDisplayName              | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenecek bir işlevin adı |
| İş AkışıFonksiyonuAçıklama              | nvarchar(255) | Hayır          | Fonksiyonun açıklaması |
| SözleşmeActionId                         | int           | Hayır          | Bir sözleşme eylemi için benzersiz tanımlayıcı |
| ContractActionProvisioningStatus         | int           | Hayır          | Sözleşme eylemi için sağlama işleminin geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 - Sözleşme eylemi veritabanında API tarafından oluşturulmuştur<br />1 - Sözleşme eylemi genel muhasebeye gönderildi<br />2 - Sözleşme eylemi deftere başarıyla dağıtılmıştır<br />3 veya 4 - Sözleşme genel muhasebeye dağıtılmadı<br />5 - Sözleşme deftere başarıyla dağıtıldı <br /><br />Sürüm 1.5'ten başlayarak 0 ile 5 arasında değerler desteklenir. Geçerli sürümde geriye dönük uyumluluk için, yalnızca 0 ile 2 arasında değerleri destekleyen **vwContractActionV0'ü** görüntüleyin. |
| SözleşmeActionTimestamp                  | tarih zamanı(2,7) | Hayır          | Sözleşme eyleminin zaman damgası |
| SözleşmeEylemExecutedByUserid           | int           | Hayır          | Sözleşme eylemini gerçekleştiren kullanıcının benzersiz tanımlayıcısı |
| SözleşmeEylemExecutedByUserFirstName    | int           | Evet         | Sözleşme eylemini gerçekleştiren kullanıcının adı |
| SözleşmeEylemExecutedByUserLastName     | nvarchar(50)  | Evet         | Sözleşme eylemini gerçekleştiren kullanıcının soyadı |
| SözleşmeEylemExecutedByUserexternalid   | nvarchar(255) | Evet         | Sözleşme eylemini yürüten kullanıcının dış tanımlayıcısı. Varsayılan olarak, bu kimlik, Azure Etkin Dizini konsorsiyumunda kimliklerini temsil eden kılavuzdur. |
| SözleşmeEylemExecutedByUserEmailAddress | nvarchar(255) | Evet         | Sözleşme eylemini gerçekleştiren kullanıcının e-posta adresi |
| İş AkışıFonksiyonuParametreId              | int           | Hayır          | İşlevin bir parametresi için benzersiz bir tanımlayıcı |
| İş AkışıFonksiyonuParameterName            | nvarchar(50)  | Hayır          | İşlevin bir parametresinin adı |
| İş AkışıFonksiyonuParameterDisplayNameName     | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenecek bir işlev parametresinin adı |
| İş AkışıFonksiyonuParametreDataTypeId      | int           | Hayır          | İş akışı işlev parametresi ile ilişkili veri türü için benzersiz tanımlayıcı |
| İş AkışıParametreDataTypeName            | nvarchar(50)  | Hayır          | İş akışı işlevi parametresi ile ilişkili bir veri türünün adı |
| SözleşmeEylemParameterValue             | nvarchar(255) | Hayır          | Akıllı sözleşmede depolanan parametrenin değeri |
| BlockHash                                | nvarchar(255) | Evet         | Bloğun karma |
| Blok Numarası                              | int           | Evet         | Defterdeki bloğun sayısı |
| BlockTimestamp                           | tarih zamanı(2,7) | Evet         | Bloğun zaman damgası |
| Transactionıd                            | int           | Hayır          | Hareket için benzersiz bir tanımlayıcı |
| İşlemGönderen                          | nvarchar(255) | Evet         | İşlemi oluşturan taraf |
| İşlemler                            | nvarchar(255) | Evet         | İle işlem yapılan parti |
| İşlemHash                          | nvarchar(255) | Evet         | Bir işlemin karma |
| İşlemIsWorkbenchİşlem        | bit           | Evet         | İşlemin Bir Azure Blockchain Workbench hareketi olup olmadığını tanımlayan bir bit |
| İşlemSağlama Durumu            | int           | Evet         | Hareket için sağlama işleminin geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 - İşlem veritabanındaki API tarafından oluşturulmuştur<br />1 - İşlem genel muhasebeye gönderildi<br />2 - İşlem deftere başarıyla dağıtılmıştır                 |
| İşlem Değeri                         | ondalık(32,2) | Evet         | İşlemin değeri |

## <a name="vwcontractproperty"></a>vwContractEmlak

Bu görünüm, bir sözleşmeyle ilişkili özelliklerle ilgili bilgilerin çoğunluğunu temsil eder ve sık karşılaşılan raporlama senaryolarını kolayca kolaylaştırmak üzere tasarlanmıştır. Alınan her özellik için bu görünüm aşağıdaki verileri içerir:

-   İlişkili uygulama tanımı
-   İlişkili iş akışı tanımı
-   İş akışını dağıtan kullanıcının ayrıntıları
-   İlişkili akıllı sözleşme özellik tanımı
-   Özellikler için belirli örnek değerleri
-   Sözleşmenin devlet malı için ayrıntılar

| Adı                               | Tür          | Null Olabilir | Açıklama                                                                                                                                                                                                                                                                        |
|------------------------------------|---------------|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationID                      | int           | Hayır          | Uygulama için benzersiz bir tanımlayıcı |
| ApplicationName                    | nvarchar(50)  | Hayır          | Uygulamanın adı |
| UygulamaDisplayName             | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenecek ad |
| Uygulama Etkin                 | bit           | Hayır          | Uygulamanın şu anda etkin olup olmadığını tanımlar.<br />**Not:** Bir uygulama veritabanına devre dışı bırakılmış olarak yansıtılabilir olsa da, ilişkili sözleşmeler blockchain'de kalır ve bu sözleşmelere ilişkin veriler veritabanında kalır.                      |
| İş Akışı Kimliği                         | int           | Hayır          | İş akışı için benzersiz tanımlayıcı |
| İş AkışıAdı                       | nvarchar(50)  | Hayır          | İş akışının adı |
| İş AkışıDisplayName                | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenen iş akışının adı |
| İş Akışı Açıklama                | nvarchar(255) | Evet         | İş akışının açıklaması |
| Sözleşmeli Kimlik                         | int           | Hayır          | Sözleşme için benzersiz tanımlayıcı |
| SözleşmeSağlama Durumu         | int           | Hayır          | Sözleşme için sağlama sürecinin geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 - Sözleşme veritabanında API tarafından oluşturuldu<br />1 - Sözleşme deftere<br />2 - Sözleşme deftere başarıyla dağıtılmıştır<br />3 veya 4 - Sözleşme genel muhasebeye dağıtılmadı<br />5 - Sözleşme deftere başarıyla dağıtıldı <br /><br />Sürüm 1.5'ten başlayarak 0 ile 5 arasında değerler desteklenir. Geçerli sürümde geriye dönük uyumluluk için, yalnızca 0 ile 2 arasında değerleri destekleyen **vwContractPropertyV0'yi** görüntüleyin. |
| ContractCodeId                     | int           | Hayır          | Sözleşmenin kod uygulaması için benzersiz bir tanımlayıcı |
| Sözleşmeli LedgerIdentifier           | nvarchar(255) | Evet         | Belirli bir dağıtılmış genel muhasebe için akıllı sözleşmenin dağıtılan sürümüyle ilişkili benzersiz bir tanımlayıcı. Örneğin, Ethereum. |
| SözleşmeDeployedByUserId           | int           | Hayır          | Sözleşmeyi dağıtan kullanıcının benzersiz tanımlayıcısı |
| SözleşmeDeployedByUserFirstName    | nvarchar(50)  | Evet         | Sözleşmeyi dağıtan kullanıcının adı |
| SözleşmeDeployedByUserLastName     | nvarchar(255) | Evet         | Sözleşmeyi dağıtan kullanıcının soyadı |
| SözleşmeDeployedByUserExternalId   | nvarchar(255) | Hayır          | Sözleşmeyi dağıtan kullanıcının dış tanımlayıcısı. Varsayılan olarak, bu kimlik, Azure Etkin Dizini konsorsiyumunda kimliklerini temsil eden kılavuzdur |
| SözleşmeDeployedByUserEmailAddress | nvarchar(255) | Evet         | Sözleşmeyi dağıtan kullanıcının e-posta adresi |
| İş AkışıEmlakid                 | int           |             | İş akışı özelliği için benzersiz bir tanımlayıcı |
| İş AkışıEmlakDataTypeId         | int           | Hayır          | Özelliğin veri türünün kimliği |
| İş AkışıEmlakDataTypeName       | nvarchar(50)  | Hayır          | Özelliğin veri türünün adı |
| İş AkışıEmlakName               | nvarchar(50)  | Hayır          | İş akışı özelliğinin adı |
| İş AkışıÖzellikDisplayName        | nvarchar(255) | Hayır          | İş akışı özelliğinin görüntü adı |
| İş AkışıEmlak Açıklaması        | nvarchar(255) | Evet         | Özelliğin açıklaması |
| SözleşmeEmlak Değeri              | nvarchar(255) | Hayır          | Sözleşmedeki bir malın değeri |
| StateName                          | nvarchar(50)  | Evet         | Bu özellik sözleşmenin durumunu içeriyorsa, eyaletin görüntü adıdır. Durumla ilişkili değilse, değer null olacaktır. |
| StateDisplayName                   | nvarchar(255) | Hayır          | Bu özellik durumu içeriyorsa, durum için görüntü adıdır. Durumla ilişkili değilse, değer null olacaktır. |
| Durum Değeri                         | nvarchar(255) | Evet         | Bu özellik durumu içeriyorsa, durum değeridir. Durumla ilişkili değilse, değer null olacaktır. |

## <a name="vwcontractstate"></a>vwContractState

Bu görünüm, belirli bir sözleşmenin durumuyla ilgili bilgilerin çoğunluğunu temsil eder ve ortak raporlama senaryolarını kolayca kolaylaştırmak üzere tasarlanmıştır. Bu görünümdeki her kayıt aşağıdaki verileri içerir:

-   İlişkili uygulama tanımı
-   İlişkili iş akışı tanımı
-   İş akışını dağıtan kullanıcının ayrıntıları
-   İlişkili akıllı sözleşme özellik tanımı
-   Sözleşmenin devlet malı için ayrıntılar

| Adı                               | Tür          | Null Olabilir | Açıklama                                                                                                                                                                                                                                                                        |
|------------------------------------|---------------|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationID                      | int           | Hayır          | Uygulama için benzersiz bir tanımlayıcı |
| ApplicationName                    | nvarchar(50)  | Hayır          | Uygulamanın adı |
| UygulamaDisplayName             | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenecek ad |
| Uygulama Etkin                 | bit           | Hayır          | Uygulamanın şu anda etkin olup olmadığını tanımlar.<br />**Not:** Bir uygulama veritabanına devre dışı bırakılmış olarak yansıtılabilir olsa da, ilişkili sözleşmeler blockchain'de kalır ve bu sözleşmelere ilişkin veriler veritabanında kalır. |
| İş Akışı Kimliği                         | int           | Hayır          | İş akışı için benzersiz bir tanımlayıcı |
| İş AkışıAdı                       | nvarchar(50)  | Hayır          | İş akışının adı |
| İş AkışıDisplayName                | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenen ad |
| İş Akışı Açıklama                | nvarchar(255) | Evet         | İş akışının açıklaması |
| SözleşmeliLedgerImplementationId     | nvarchar(255) | Evet         | Belirli bir dağıtılmış genel muhasebe için akıllı sözleşmenin dağıtılan sürümüyle ilişkili benzersiz bir tanımlayıcı. Örneğin, Ethereum. |
| Sözleşmeli Kimlik                         | int           | Hayır          | Sözleşme için benzersiz bir tanımlayıcı |
| SözleşmeSağlama Durumu         | int           | Hayır          |Sözleşme için sağlama sürecinin geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 - Sözleşme veritabanında API tarafından oluşturuldu<br />1 - Sözleşme deftere<br />2 - Sözleşme deftere başarıyla dağıtılmıştır<br />3 veya 4 - Sözleşme genel muhasebeye dağıtılmadı<br />5 - Sözleşme deftere başarıyla dağıtıldı <br /><br />Sürüm 1.5'ten başlayarak 0 ile 5 arasında değerler desteklenir. Geçerli sürümde geriye dönük uyumluluk için, yalnızca 0 ile 2 arasında değerleri destekleyen **vwContractStateV0'i** görüntüleyin. |
| Connectionıd                       | int           | Hayır          | Blockchain örneği için benzersiz bir tanımlayıcı iş akışı |
| ContractCodeId                     | int           | Hayır          | Sözleşmenin kod uygulaması için benzersiz bir tanımlayıcı |
| SözleşmeDeployedByUserId           | int           | Hayır          | Sözleşmeyi dağıtan kullanıcının benzersiz tanımlayıcısı |
| SözleşmeDeployedByUserExternalId   | nvarchar(255) | Hayır          | Sözleşmeyi dağıtan kullanıcının dış tanımlayıcısı. Varsayılan olarak, bu kimlik, Azure Etkin Dizini konsorsiyumunda kimliklerini temsil eden kılavuzdur. |
| SözleşmeDeployedByUserFirstName    | nvarchar(50)  | Evet         | Sözleşmeyi dağıtan kullanıcının adı |
| SözleşmeDeployedByUserLastName     | nvarchar(255) | Evet         | Sözleşmeyi dağıtan kullanıcının soyadı |
| SözleşmeDeployedByUserEmailAddress | nvarchar(255) | Evet         | Sözleşmeyi dağıtan kullanıcının e-posta adresi |
| İş AkışıEmlakid                 | int           | Hayır          | İş akışı özelliği için benzersiz bir tanımlayıcı |
| İş AkışıEmlakDataTypeId         | int           | Hayır          | İş akışı özelliğinin veri türünün kimliği |
| İş AkışıEmlakDataTypeName       | nvarchar(50)  | Hayır          | İş akışı özelliğinin veri türünün adı |
| İş AkışıEmlakName               | nvarchar(50)  | Hayır          | İş akışı özelliğinin adı |
| İş AkışıÖzellikDisplayName        | nvarchar(255) | Hayır          | Kullanıcı Arabirimi'nde gösterilen özelliğin görüntü adı |
| İş AkışıEmlak Açıklaması        | nvarchar(255) | Evet         | Özelliğin açıklaması |
| SözleşmeEmlak Değeri              | nvarchar(255) | Hayır          | Sözleşmede depolanan bir malın değeri |
| StateName                          | nvarchar(50)  | Evet         | Bu özellik durumu içeriyorsa, durum için görüntü adı. Durumla ilişkili değilse, değer null olacaktır. |
| StateDisplayName                   | nvarchar(255) | Hayır          | Bu özellik durumu içeriyorsa, durum için görüntü adıdır. Durumla ilişkili değilse, değer null olacaktır. |
| Durum Değeri                         | nvarchar(255) | Evet         | Bu özellik durumu içeriyorsa, durum değeridir. Durumla ilişkili değilse, değer null olacaktır. |

## <a name="vwuser"></a>vwUser

Bu görünüm, Azure Blockchain Workbench'i kullanmak üzere sağlanan konsorsiyum üyeleri hakkında ayrıntılı bilgi sağlar. Varsayılan olarak, veriler kullanıcının ilk sağlama yoluyla doldurulur.

| Adı               | Tür          | Null Olabilir | Açıklama                                                                                                                                                                                                                               |
|--------------------|---------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kimlik                 | int           | Hayır          | Bir kullanıcı için benzersiz bir tanımlayıcı |
| ExternalID         | nvarchar(255) | Hayır          | Bir kullanıcı için harici bir tanımlayıcı. Varsayılan olarak, bu kimlik kullanıcı için Azure Etkin Dizin Kimliğini temsil eden kılavuzdur. |
| Sağlama Durumu | int           | Hayır          |Kullanıcı için sağlama işleminin geçerli durumunu tanımlar. Olası değerler şunlardır: <br />0 - Kullanıcı API tarafından oluşturuldu<br />1 - Bir anahtar veritabanında kullanıcı ile ilişkili olmuştur<br />2 - Kullanıcı tam olarak |
| FirstName          | nvarchar(50)  | Evet         | Kullanıcının ilk adı |
| LastName           | nvarchar(50)  | Evet         | Kullanıcının soyadı |
| Emailaddress       | nvarchar(255) | Evet         | Kullanıcının e-posta adresi |

## <a name="vwworkflow"></a>vwİş Akışı

Bu görünüm, temel iş akışı meta verilerinin ayrıntılarının yanı sıra iş akışının işlevlerini ve parametrelerini de temsil eder. Raporlama için tasarlanmış, aynı zamanda iş akışı ile ilişkili uygulama hakkında meta veri içerir. Bu görünüm, iş akışları hakkında raporlamayı kolaylaştırmak için birden çok temel tablodan veri içerir. Her iş akışı için bu görünüm aşağıdaki verileri içerir:

-   İlişkili uygulama tanımı
-   İlişkili iş akışı tanımı
-   İlişkili iş akışı başlangıç durumu bilgileri

| Adı                              | Tür          | Null Olabilir | Açıklama                                                                                                                                |
|-----------------------------------|---------------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationID                     | int           | Hayır          | Uygulama için benzersiz bir tanımlayıcı |
| ApplicationName                   | nvarchar(50)  | Hayır          | Uygulamanın adı |
| UygulamaDisplayName            | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenecek ad |
| Uygulama Etkin                | bit           | Hayır          | Uygulamanın etkin olup olmadığını tanımlar |
| İş Akışı Kimliği                        | int           | Evet         | İş akışı için benzersiz bir tanımlayıcı |
| İş AkışıAdı                      | nvarchar(50)  | Hayır          | İş akışının adı |
| İş AkışıDisplayName               | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenen ad |
| İş Akışı Açıklama               | nvarchar(255) | Evet         | İş akışının açıklaması. |
| İş AkışıConstructorFunctionId     | int           | Hayır          | İş akışı için oluşturucu olarak hizmet veren iş akışı işlevinin tanımlayıcısı |
| İş AkışıStartStateId              | int           | Hayır          | Devlet için benzersiz bir tanımlayıcı |
| İş AkışıStartStateName            | nvarchar(50)  | Hayır          | Devletin adı |
| İş AkışıStartStateDisplayName     | nvarchar(255) | Hayır          | Durum için kullanıcı arabiriminde görüntülenecek ad |
| İş AkışıBaşlangıçDurumuAçıklama     | nvarchar(255) | Evet         | İş akışı durumunun açıklaması |
| İş AkışıStartStateStyle           | nvarchar(50)  | Evet         | Bu değer, iş akışının bu durumda olduğu yüzde tamamlanmayı tanımlar |
| İş AkışıBaşlangıçDevlet Değeri           | int           | Hayır          | Devletin değeri |
| İş AkışıStartStatePercentComplete | int           | Hayır          | Bu durumu Kullanıcı BirA'da nasıl işleteceklerine ilişkin istemcilere ipucu sağlayan bir metin açıklaması. Desteklenen devletler *Başarı* ve *Başarısızlık* içerir |

## <a name="vwworkflowfunction"></a>vwWorkflowFonksiyonu

Bu görünüm, temel iş akışı meta verilerinin ayrıntılarının yanı sıra iş akışının işlevlerini ve parametrelerini de temsil eder. Raporlama için tasarlanmış, aynı zamanda iş akışı ile ilişkili uygulama hakkında meta veri içerir. Bu görünüm, iş akışları hakkında raporlamayı kolaylaştırmak için birden çok temel tablodan veri içerir. Her iş akışı işlevi için bu görünüm aşağıdaki verileri içerir:

-   İlişkili uygulama tanımı
-   İlişkili iş akışı tanımı
-   İş akışı işlevi ayrıntıları

| Adı                                 | Tür          | Null Olabilir | Açıklama                                                                          |
|--------------------------------------|---------------|-------------|--------------------------------------------------------------------------------------|
| ApplicationID                        | int           | Hayır          | Uygulama için benzersiz bir tanımlayıcı |
| ApplicationName                      | nvarchar(50)  | Hayır          | Uygulamanın adı |
| UygulamaDisplayName               | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenecek ad |
| Uygulama Etkin                   | bit           | Hayır          | Uygulamanın etkin olup olmadığını tanımlar |
| İş Akışı Kimliği                           | int           | Hayır          | İş akışı için benzersiz bir tanımlayıcı |
| İş AkışıAdı                         | nvarchar(50)  | Hayır          | İş akışının adı |
| İş AkışıDisplayName                  | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenen iş akışının adı |
| İş Akışı Açıklama                  | nvarchar(255) | Evet         | İş akışının açıklaması |
| İş AkışıFonksiyonId                   | int           | Hayır          | Bir işlev için benzersiz bir tanımlayıcı |
| İş AkışıİşAdı                 | nvarchar(50)  | Evet         | İşlevin adı |
| İş AkışıFunctionDisplayName          | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenecek bir işlevin adı |
| İş AkışıFonksiyonuAçıklama          | nvarchar(255) | Evet         | İş akışı işlevinin açıklaması |
| İş AkışıFonksiyonYapıcı        | bit           | Hayır          | İş akışı işlevinin iş akışı için oluşturucu olup olmadığını tanımlar |
| İş AkışıFonksiyonuParametreId          | int           | Hayır          | Bir işlevin parametresi için benzersiz bir tanımlayıcı |
| İş AkışıFonksiyonuParameterName        | nvarchar(50)  | Hayır          | İşlevin bir parametresinin adı |
| İş AkışıFonksiyonuParameterDisplayNameName | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenecek bir işlev parametresinin adı |
| İş AkışıFonksiyonuParametreDataTypeId  | int           | Hayır          | İş akışı işlev parametresi ile ilişkili veri türü için benzersiz bir tanımlayıcı |
| İş AkışıParametreDataTypeName        | nvarchar(50)  | Hayır          | İş akışı işlevi parametresi ile ilişkili bir veri türünün adı |

## <a name="vwworkflowproperty"></a>vwWorkflowÖzellik

Bu görünüm, iş akışı için tanımlanan özellikleri temsil eder. Her özellik için bu görünüm aşağıdaki verileri içerir:

-   İlişkili uygulama tanımı
-   İlişkili iş akışı tanımı
-   İş akışı özelliği ayrıntıları

| Adı                         | Tür          | Null Olabilir | Açıklama                                                                                                                                                                                                                                                   |
|------------------------------|---------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationID                | int           | Hayır          | Uygulama için benzersiz bir tanımlayıcı |
| ApplicationName              | nvarchar(50)  | Hayır          | Uygulamanın adı |
| UygulamaDisplayName       | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenecek ad |
| Uygulama Etkin           | bit           | Hayır          | Uygulamanın şu anda etkin olup olmadığını tanımlar.<br />**Not:** Bir uygulama veritabanına devre dışı bırakılmış olarak yansıtılabilir olsa da, ilişkili sözleşmeler blockchain'de kalır ve bu sözleşmelere ilişkin veriler veritabanında kalır. |
| İş Akışı Kimliği                   | int           | Hayır          | İş akışı için benzersiz bir tanımlayıcı |
| İş AkışıAdı                 | nvarchar(50)  | Hayır          | İş akışının adı |
| İş AkışıDisplayName          | nvarchar(255) | Hayır          | Kullanıcı arabiriminde iş akışı için görüntülenecek ad |
| İş Akışı Açıklama          | nvarchar(255) | Evet         | İş akışının açıklaması |
| İş AkışıEmlakID           | int           | Hayır          | İş akışı özelliği için benzersiz bir tanımlayıcı |
| İş AkışıEmlakName         | nvarchar(50)  | Hayır          | Özelliğin adı |
| İş AkışıEmlak Açıklaması  | nvarchar(255) | Evet         | Özelliğin açıklaması |
| İş AkışıÖzellikDisplayName  | nvarchar(255) | Hayır          | Kullanıcı arabiriminde görüntülenecek ad |
| İş AkışıÖzellikİş Akışı   | int           | Hayır          | Bu özelliğin ilişkili olduğu iş akışının kimliği |
| İş AkışıEmlakDataTypeId   | int           | Hayır          | Özellik için tanımlanan veri türünün kimliği |
| İş AkışıEmlakDataTypeName | nvarchar(50)  | Hayır          | Özellik için tanımlanan veri türünün adı |
| İş AkışıPropertyIsState      | bit           | Hayır          | Bu alan, bu iş akışı özelliğinin iş akışının durumunu içerip içermeyişini tanımlar |

## <a name="vwworkflowstate"></a>vwWorkflowState

Bu görünüm, iş akışıyla ilişkili özellikleri temsil eder. Her sözleşme için bu görünüm aşağıdaki verileri içerir:

-   İlişkili uygulama tanımı
-   İlişkili iş akışı tanımı
-   İş akışı durumu bilgileri

| Adı                         | Tür          | Null Olabilir | Açıklama                                                                                                                                                                                                                                                   |
|------------------------------|---------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ApplicationID                | int           | Hayır          | Uygulama için benzersiz bir tanımlayıcı |
| ApplicationName              | nvarchar(50)  | Hayır          | Uygulamanın adı |
| UygulamaDisplayName       | nvarchar(255) | Hayır          | Uygulamanın açıklaması |
| Uygulama Etkin           | bit           | Hayır          | Uygulamanın şu anda etkin olup olmadığını tanımlar.<br />**Not:** Bir uygulama veritabanına devre dışı bırakılmış olarak yansıtılabilir olsa da, ilişkili sözleşmeler blockchain'de kalır ve bu sözleşmelere ilişkin veriler veritabanında kalır. |
| İş Akışı Kimliği                   | int           | Hayır          | İş akışı için benzersiz tanımlayıcı |
| İş AkışıAdı                 | nvarchar(50)  | Hayır          | İş akışının adı |
| İş AkışıDisplayName          | nvarchar(255) | Hayır          | İş akışı için kullanıcı arabiriminde görüntülenen ad |
| İş Akışı Açıklama          | nvarchar(255) | Evet         | İş akışının açıklaması |
| İş AkışıStateID              | int           | Hayır          | Devlet için benzersiz tanımlayıcı |
| İş AkışıStateName            | nvarchar(50)  | Hayır          | Devletin adı |
| İş AkışıStateDisplayName     | nvarchar(255) | Hayır          | Durum için kullanıcı arabiriminde görüntülenecek ad |
| İş AkışıStateDescription     | nvarchar(255) | Evet         | İş akışı durumunun açıklaması |
| İş AkışıStatePercentComplete | int           | Hayır          | Bu değer, iş akışının bu durumda olduğu yüzde tamamlanmayı tanımlar |
| İş AkışıStateValue           | nvarchar(50)  | Hayır          | Devletin değeri |
| İş AkışıStateStyle           | nvarchar(50)  | Hayır          | Bu durumu Kullanıcı BirA'da nasıl işleteceklerine ilişkin istemcilere ipucu sağlayan bir metin açıklaması. Desteklenen devletler *Başarı* ve *Başarısızlık* içerir |
