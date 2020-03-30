---
title: Azure altyapı bütünlüğü
description: Bu makalede, Azure altyapısının bütünlüğü ele allanmaktadır.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2018
ms.author: terrylan
ms.openlocfilehash: ef81e74b07a351139aa8feefbdf1b89ea7e4994f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68727181"
---
# <a name="azure-infrastructure-integrity"></a>Azure altyapı bütünlüğü

## <a name="software-installation"></a>Yazılım kurulumu
Azure ortamında yüklenen yazılım yığınındaki tüm bileşenler, Microsoft Güvenlik Geliştirme Yaşam Döngüsü (SDL) işlemini izleyerek özel olarak oluşturulmuştür. İşletim sistemi (OS) görüntüleri ve SQL Veritabanı dahil olmak üzere tüm yazılım bileşenleri, değişim yönetimi ve sürüm yönetimi sürecinin bir parçası olarak dağıtılır. Tüm düğümlerde çalışan işletim sistemi, Windows Server 2008 veya Windows Server 2012'nin özelleştirilmiş bir sürümüdür. Tam sürüm, işletim sistemi için oynamak niyetinde rolüne göre kumaş denetleyicisi (FC) tarafından seçilir. Buna ek olarak, ana bilgisayar işletim sistemi herhangi bir yetkisiz yazılım bileşeninin yüklenmesine izin vermez.

Bazı Azure bileşenleri, konuk işletim sistemi üzerinde çalışan konuk Bir VM'de Azure müşterileri olarak dağıtılır.

## <a name="virus-scans-on-builds"></a>Yapılarda virüs taraması
Azure yazılım bileşeni (işletim sistemi dahil) yapılarının, Endpoint Protection anti-virüs aracını kullanan bir virüs taramasından geçmesi gerekir. Her virüs taraması, taranan ve tarama sonuçlarını ayrıntılı olarak açıklayan ilişkili yapı dizini içinde bir günlük oluşturur. Virüs taraması, Azure'daki her bileşen için yapı kaynak kodunun bir parçasıdır. Kod, temiz ve başarılı bir virüs taraması yapılmadan üretime geçirilir. Herhangi bir sorun kaydedilirse, yapı dondurulur ve "haydut" kodunun yapıya nereye girdiğini belirlemek için Microsoft Security içindeki güvenlik ekiplerine gider.

## <a name="closed-and-locked-environment"></a>Kapalı ve kilitli ortam
Varsayılan olarak, Azure altyapı düğümleri ve konuk VM'lerde kullanıcı hesapları oluşturulmaz. Ayrıca, varsayılan Windows yönetici hesapları da devre dışı bırakılır. Azure canlı desteğinden yöneticiler, uygun kimlik doğrulaması yla bu makinelerde oturum açabilir ve acil onarımlar için Azure üretim ağını yönetebilir.

## <a name="azure-sql-database-authentication"></a>Azure SQL Veritabanı kimlik doğrulaması
SQL Server'ın herhangi bir uygulamasında olduğu gibi, kullanıcı hesabı yönetimi sıkı bir şekilde denetlenmelidir. Azure SQL Veritabanı yalnızca SQL Server kimlik doğrulamasını destekler. Bir müşterinin veri güvenliği modelini tamamlamak için, güçlü parolalarla ve belirli haklara göre yapılan dır.

## <a name="acls-and-firewalls-between-the-microsoft-corporate-network-and-an-azure-cluster"></a>Microsoft kurumsal ağı ile Azure kümesi arasındaki ALAK'lar ve güvenlik duvarları
Hizmet platformu ile Microsoft kurumsal ağı arasındaki erişim denetim listeleri (ALA'lar) ve güvenlik duvarları, SQL Veritabanı örneklerini yetkisiz içeriden erişime karşı korur. Ayrıca, Yalnızca Microsoft kurumsal ağındaki IP adresinden gelen kullanıcılar Windows Fabric platform yönetimi bitiş noktasına erişebilir.

## <a name="acls-and-firewalls-between-nodes-in-a-sql-database-cluster"></a>SQL Veritabanı kümesindeki düğümler arasındaki ADA'lar ve güvenlik duvarları
Ek bir koruma olarak, savunma derinliği stratejisinin bir parçası olarak, Bir SQL Veritabanı kümesindeki düğümler arasında ADA'lar ve güvenlik duvarı uygulanmıştır. Windows Fabric platform kümesi içindeki tüm iletişimin yanı sıra tüm çalışan kodgüvenilirdir.

## <a name="custom-monitoring-agents"></a>Özel izleme aracıları
SQL Veritabanı, SQL Veritabanı kümesinin durumunu izlemek için izleme örgütü olarak da adlandırılan özel izleme aracıları (MAs) kullanır.

## <a name="web-protocols"></a>Web protokolleri

### <a name="role-instance-monitoring-and-restart"></a>Rol örneği izleme ve yeniden başlatma
Azure, dağıtılmış, çalışan tüm rollerin (Internet'e dönük web veya arka uç işleme çalışanı rolleri) etkin ve verimli bir şekilde sağlandığı hizmetleri sunmalarını sağlamak için sürekli sağlık denetimine tabi olmasını sağlar. Bir rol, barındırılan uygulamada kritik bir hata veya rol örneğinin kendisinde altta yatan bir yapılandırma sorunu yla sağlıksız hale gelirse, FC rolün içindeki sorunu algılar ve düzeltici bir durum başlatır.

### <a name="compute-connectivity"></a>İşlem bağlantısı
Azure, dağıtılan uygulama veya hizmete standart web tabanlı protokoller aracılığıyla erişilebilmesini sağlar. Internet'e bakan web rollerinin sanal örnekleri harici internet bağlantısına sahiptir ve doğrudan web kullanıcıları tarafından ulaşılabilir. İşçi rollerinin kamuya açık web rolü sanal örnekleri adına gerçekleştirdiği işlemlerin duyarlılığını ve bütünlüğünü korumak için, arka uç işleme alt rolleri sanal örnekleri harici internet bağlantısı na sahiptir, ancak doğrudan harici web kullanıcıları tarafından erişilir.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft'un Azure altyapısını korumak için ne yaptığı hakkında daha fazla bilgi edinmek için bkz:

- [Azure tesisleri, tesisler ve fiziksel güvenlik](physical-security.md)
- [Azure altyapısı kullanılabilirliği](infrastructure-availability.md)
- [Azure bilgi sistemi bileşenleri ve sınırları](infrastructure-components.md)
- [Azure ağ mimarisi](infrastructure-network.md)
- [Azure üretim ağı](production-network.md)
- [Azure SQL Veritabanı güvenlik özellikleri](infrastructure-sql.md)
- [Azure üretim işlemleri ve yönetimi](infrastructure-operations.md)
- [Azure altyapı izleme](infrastructure-monitoring.md)
- [Azure müşteri veri koruması](protection-customer-data.md)
