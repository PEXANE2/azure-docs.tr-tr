---
title: Azure SQL Edge 'in desteklenen özellikleri (Önizleme)
description: Azure SQL Edge (Önizleme) tarafından desteklenen özelliklerin ayrıntıları hakkında bilgi edinin
keywords: SQL Edge 'e giriş, SQL Edge nedir, SQL Edge 'e genel bakış
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: f0994ac1d28118869f0d5c2844a034623d101ee8
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684376"
---
# <a name="supported-features-of-azure-sql-edge-preview"></a>Azure SQL Edge 'in desteklenen özellikleri (Önizleme) 

Bu makalede, Azure SQL Edge tarafından desteklenen özelliklerin ayrıntıları sağlanmaktadır. Azure SQL Edge, Linux üzerinde Microsoft SQL Server veritabanı altyapısının en son sürümüne kurulmuştur ve Linux veya Windows üzerinde SQL Server 2019 ' de kullanılamayan veya mevcut olan bazı özelliklere ek olarak Linux için SQL Server 2019 ' de desteklenen özelliklerin bir alt kümesini destekler. Linux üzerinde SQL Server desteklenen özelliklerin kapsamlı bir listesi için bkz. [Linux üzerinde SQL Server 2019 'Nin sürümleri ve desteklenen özellikleri](https://docs.microsoft.com/sql/linux/sql-server-linux-editions-and-components-2019). Windows üzerinde SQL Server sürümleri ve desteklenen özellikler için, [SQL Server 2019 (15. x) ' ın sürümleri ve desteklenen özellikleri](https://docs.microsoft.com/sql/sql-server/editions-and-components-of-sql-server-version-15)konusuna bakın.

> [!NOTE]
> Azure SQL Edge Şu anda (Önizleme) ve üretim ortamlarında kullanılmamalıdır. Microsoft, dağıtım ve kullanım örneği senaryolarını doğrulamadan sonra üretim ortamlarında Azure SQL Edge 'i çalıştırmayı önerebilir.

## <a name="azure-sql-edge-editions"></a>Azure SQL Edge sürümleri

Azure SQL Edge, iki farklı sürüm veya yazılım planlarıyla kullanılabilir. Bu sürümler, aynı özellik kümelerine sahiptir ve yalnızca kullanım hakları ve konak sisteminde erişebileceği CPU/bellek miktarı bakımından farklılık gösterir.

   |**Plan**  |**Açıklama**  |
   |---------|---------|
   |Azure SQL Edge geliştiricisi  |  Yalnızca geliştirme SKU 'su, her Azure SQL Edge geliştirici kapsayıcısı 4 çekirdeğe ve 32 GB bellekle sınırlıdır  |
   |Azure SQL Edge    |  Üretim SKU 'su olan her bir Azure SQL Edge kapsayıcısı, en fazla 8 çekirdek ve 64 GB bellek ile sınırlıdır  |

## <a name="operating-system"></a>İşletim Sistemi

Azure SQL Edge kapsayıcıları Şu anda Ubuntu 16,04 ' i temel alır ve bu nedenle yalnızca Ubuntu 16,04 (önerilen) veya Ubuntu 18,04 çalıştıran Docker konaklarında çalışmak üzere desteklenir. Azure SQL Edge, diğer işletim sistemi konaklarında da çalıştırılabilir (örneğin, Linux veya Windows üzerinde diğer dağıtımlar (Docker CE veya Docker EE kullanılarak), ancak bu konfigürasyonlar Microsoft tarafından kapsamlı bir şekilde sınanmamıştır.

Azure SQL Edge Şu anda yalnızca Azure IoT Edge aracılığıyla dağıtım için desteklenir. Azure IoT Edge için desteklenen sistemler hakkında daha fazla bilgi için, [Azure IoT Edge desteklenen sistemlere](https://docs.microsoft.com/azure/iot-edge/support)bakın.

Windows üzerinde Azure SQL Edge çalıştırmak için önerilen yapılandırma, Windows konakta bir Ubuntu VM 'yi yapılandırmak ve ardından Linux VM içinde SQL Edge 'i çalıştırmaktır.

## <a name="hardware-support"></a>Donanım desteği

Azure SQL Edge, en az bir işlemci ve konakta bir GB RAM ile Intel, AMD veya ARM 'den olabilen 64 bitlik bir işlemci gerektirir. Azure SQL Edge 'in başlangıç belleği parmak izi 500 MB 'a yakın olsa da, Edge cihazında çalışan diğer IoT Edge modüller için ek bellek gerekir.

## <a name="azure-sql-edge-components"></a>Azure SQL Edge bileşenleri

Azure SQL Edge yalnızca veritabanı altyapısını destekler ve Windows üzerinde SQL Server 2019 ile veya Linux üzerinde SQL Server 2019 ile kullanılabilen diğer bileşenlere yönelik destek içermez. Azure SQL Edge, Analysis Services, Reporting Services, Integration Services, Ana Veri Hizmetleri Machine Learning Services (-Database) ve Machine Learning Server (tek başına) gibi SQL Server bileşenleri desteklemez.

## <a name="supported-features"></a>Desteklenen Özellikler

Azure SQL Edge, Linux üzerinde SQL Server özelliklerinin bir alt kümesini desteklemeye ek olarak aşağıdaki yeni özellikler için destek içerir. 

- Azure Stream Analytics destekleyen aynı altyapıyı temel alan SQL akışı, Azure SQL Edge 'de gerçek zamanlı veri akışı özellikleri sağlar. 
- Yeni T-SQL işlev çağrısı, zaman serisi veri analizi için Date_Bucket.
- SQL altyapısına dahil edilen ONNX çalışma zamanı aracılığıyla Machine Learning özellikleri.

## <a name="sql-server-on-linux-features-not-supported-in-azure-sql-edge"></a>Azure SQL Edge 'de desteklenmeyen Linux üzerinde SQL Server özellikleri

Aşağıdaki liste, şu anda Azure SQL Edge 'de desteklenmeyen Linux özelliklerinin 2019 SQL Server içerir.

| Alan | Desteklenmeyen Özellik veya hizmet |
|-----|-----|
| **Veritabanı tasarımı** | Bellek içi OLTP ve ilgili DDL komutları ve Transact-SQL işlevleri, katalog görünümleri ve dinamik yönetim görünümleri |
| &nbsp; | HierarchyId veri türü ve ilgili DDL komutları ve Transact-SQL işlevleri, katalog görünümleri ve dinamik yönetim görünümleri |
| &nbsp; | Uzamsal veri türü ve ilgili DDL komutları ve Transact-SQL işlevleri, katalog görünümleri ve dinamik yönetim görünümleri |
| &nbsp; | DB ve ilgili DDL komutlarını ve Transact-SQL işlevlerini, katalog görünümlerini ve dinamik yönetim görünümlerini uzat |
| &nbsp; | Tam metin dizinleri ve arama ve ilgili DDL komutları ve Transact-SQL işlevleri, katalog görünümleri ve dinamik yönetim görünümleri|
| &nbsp; | FileTable, FıLESTREAM ve ilgili DDL komutları ve Transact-SQL işlevleri, katalog görünümleri ve dinamik yönetim görünümleri|
| **Veritabanı Altyapısı** | Çoğaltma. Ancak Azure SQL Edge, çoğaltma topolojisinin bir itme abonesi olarak yapılandırılabilir. |
| &nbsp; | PolyBase. Ancak Azure SQL Edge, PolyBase 'deki dış tablolar için hedef olarak yapılandırılabilir |
| &nbsp; | Java ve Spark ile dil genişletilebilirliği |
| &nbsp; | Active Directory Tümleştirmesi |
| &nbsp; | Veritabanı anlık görüntüleri |
| &nbsp; | Kalıcı bellek desteği |
| &nbsp; | Microsoft Dağıtılmış İşlem Düzenleyicisi |
| &nbsp; | Resource Governor ve GÇ kaynak İdaresi |
| &nbsp; | Arabellek havuzu uzantısı |
| &nbsp; | Üçüncü taraf bağlantıları olan dağıtılmış sorgu |
| &nbsp; | Bağlı Sunucular |
| &nbsp; | Sistem genişletilmiş saklı yordamları (XP_CMDSHELL, vb.) |
| &nbsp; | CLR derlemeleri ve ilgili DDL komutları ve Transact-SQL işlevleri, katalog görünümleri ve dinamik yönetim görünümleri |
| &nbsp; | CLR bağımlı T-ASSEMBLYPROPERTY, FORMAT, PARSE, TRY_PARSE gibi SQL işlevleri |
| &nbsp; | CLR bağımlı tarih ve saat Kataloğu görünümleri, işlevleri ve sorgu yan tümceleri |
| &nbsp; | Arabellek havuzu uzantısı |
| &nbsp; | Veritabanı Postası |
| **SQL Server Agent** |  Alt sistemler: CmdExec, PowerShell, Queue Reader, SSIS, SSAS, SSRS |
| &nbsp; | Uyarılar |
| &nbsp; | Yönetilen Yedekleme |
| **Yüksek Kullanılabilirlik** | Always on kullanılabilirlik grupları  |
| &nbsp; | Temel kullanılabilirlik grupları |
| &nbsp; | Her zaman yük devretme kümesi örneği |
| &nbsp; | Veritabanı Yansıtması |
| &nbsp; | Etkin bellek ve CPU ekleme |
| **Güvenlik** | Genişletilebilir anahtar yönetimi |
| &nbsp; | Active Directory Tümleştirmesi|
| &nbsp; | Güvenli şifreleme desteği|
| **Hizmetler** | SQL Server Browser |
| &nbsp; | R ve Python ile Machine Learning |
| &nbsp; | Streamıntertim |
| &nbsp; | Analysis Services |
| &nbsp; | Raporlama Hizmetleri |
| &nbsp; | Veri kalitesi hizmetleri |
| &nbsp; | Ana Veri Hizmetleri |
| &nbsp; | Dağıtılmış Yeniden Yürütme |
| **Yönetilebilirlik** | SQL Server Utility denetim noktası |

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure SQL Edge 'i dağıtma](deploy-portal.md)
- [Azure SQL Edge 'i yapılandırma](configure.md)
- [SQL Server istemci araçlarını kullanarak Azure SQL Edge örneğine bağlanma](connect.md)
- [Azure SQL Edge 'de veritabanlarını yedekleme ve geri yükleme](backup-restore.md)
