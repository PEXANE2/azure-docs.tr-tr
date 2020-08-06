---
title: Uzun süreli yedek saklama
titleSuffix: Azure SQL Database & Azure SQL Managed Instance
description: Azure SQL veritabanı 'nın Azure SQL yönetilen örnek &, uzun süreli saklama ilkesi aracılığıyla 10 yıla kadar tam veritabanı yedeklerini depolamayı nasıl kullandığını öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 05/18/2019
ms.openlocfilehash: 4b2324c480ef81ef241f4d639c22c2ed4dd1545b
ms.sourcegitcommit: 85eb6e79599a78573db2082fe6f3beee497ad316
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87808856"
---
# <a name="long-term-retention---azure-sql-database-and-azure-sql-managed-instance"></a>Uzun süreli saklama-Azure SQL veritabanı ve Azure SQL yönetilen örneği

Birçok uygulamanın, Azure SQL veritabanı ve Azure SQL yönetilen örnek [Otomatik yedeklemeleri](automated-backups-overview.md)tarafından belirtilen 7-35 günden daha fazla veritabanı yedeklerini tutmanız gereken mevzuata, uyumluluk veya diğer iş amaçları vardır. Uzun süreli saklama (LTR) özelliğini kullanarak, Azure Blob depolamada belirtilen SQL veritabanı ve SQL yönetilen örnek tam yedeklemelerini, Okuma Erişimli Coğrafi olarak yedekli depolama ile 10 yıla kadar saklayabilirsiniz. Daha sonra tüm yedeklemeyi yeni bir veritabanı olarak geri yükleyebilirsiniz.

Ayrıca, SQL yönetilen örneği, yerel olarak yedekli (LRS), bölgesel olarak yedekli (ZRS) veya coğrafi olarak yedekli (RA-GRS) [depolama Blobları](../../storage/common/storage-redundancy.md)arasında seçim yapmak için esneklik sağlayan [yapılandırılabilir yedek depolama artıklığı](automated-backups-overview.md#backup-storage-redundancy) sunar. Bu seçenek şu anda yalnızca yönetilen örnek oluşturma işlemi sırasında kullanılabilir ve kaynak sağlandıktan sonra değiştirilemez.

Azure SQL veritabanı için uzun süreli saklama etkinleştirilebilir ve Azure SQL yönetilen örneği için sınırlı genel önizlemede bulunabilir. Bu makalede uzun süreli saklama hakkında kavramsal bir genel bakış sunulmaktadır. Uzun süreli saklama yapılandırmak için bkz. [Azure SQL VERITABANı LTR 'Yi yapılandırma](long-term-backup-retention-configure.md) ve [Azure SQL yönetilen örnek LTR 'yi yapılandırma](../managed-instance/long-term-backup-retention-configure.md). 

> [!NOTE]
> [Yalnızca kopya veritabanı yedeklerini](https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server) 35 gün daha fazla LTR 'e alternatif olarak ZAMANLAMAK Için SQL Agent işlerini kullanabilirsiniz.


## <a name="how-long-term-retention-works"></a>Uzun vadeli bekletme nasıl kullanılır?
     
Uzun süreli yedek saklama (LTR), nokta-saat geri yüklemeyi (ıNR) etkinleştirmek için [otomatik olarak oluşturulan](automated-backups-overview.md) tam veritabanı yedeklemelerini kullanır. Bir LTR ilkesi yapılandırılmışsa, bu yedeklemeler uzun vadeli depolama için farklı bloblara kopyalanır. Kopya, veritabanı iş yükü üzerinde performans etkisi olmayan bir arka plan işi. SQL veritabanı 'ndaki her bir veritabanı için LTR ilkesi, LTR yedeklemelerin oluşturulma sıklığını da belirtebilir.

LTR özelliğini etkinleştirmek için, dört parametre birleşimini kullanarak bir ilke tanımlayabilirsiniz: haftalık yedekleme bekletme (W), aylık yedekleme bekletme (e), yıllık yedekleme bekletme (Y) ve yılın haftası (WeekOfYear). W belirtirseniz, her hafta bir yedekleme uzun vadeli depolamaya kopyalanacaktır. D belirtirseniz, her ayın ilk yedeklemesi uzun vadeli depolamaya kopyalanacaktır. Y belirtirseniz, WeekOfYear tarafından belirtilen hafta boyunca bir yedekleme uzun vadeli depolamaya kopyalanacaktır. Belirtilen WeekOfYear, ilke yapılandırıldığında, ilk LTR yedeklemesi aşağıdaki yılda oluşturulur. Her yedekleme, LTR yedeklemesi oluşturulduğunda yapılandırılan ilke parametrelerine göre uzun vadeli depolamada tutulur.

> [!NOTE]
> LTR ilkesinde yapılan herhangi bir değişiklik yalnızca gelecekteki yedeklemeler için geçerlidir. Örneğin, haftalık yedekleme bekletme (W), aylık yedekleme bekletme (e) veya yıllık yedekleme bekletme (Y) değiştirilirse, yeni bekletme ayarı yalnızca yeni yedeklemeler için geçerlidir. Mevcut yedeklemelerin saklama süresi değiştirilmez. Amacınız, saklama süresi dolmadan eski LTR yedeklemeleri silmek istiyorsanız [yedeklemeleri el ile silmeniz](https://docs.microsoft.com/azure/sql-database/sql-database-long-term-backup-retention-configure#delete-ltr-backups)gerekir.
> 

LTR ilkesinin örnekleri:

-  W = 0, z = 0, Y = 5, WeekOfYear = 3

   Her yılın üçüncü tam yedeklemesi beş yıl boyunca saklanır.
   
- W = 0, Z = 3, Y = 0

   Her ayın ilk tam yedeklemesi üç ay boyunca saklanır.

- W = 12, Z = 0, Y = 0

   Her haftalık tam yedekleme, 12 hafta boyunca saklanacaktır.

- W = 6, z = 12, Y = 10, WeekOfYear = 16

   Her haftalık tam yedekleme altı hafta boyunca saklanır. 12 ay boyunca tutulacak olan her ayın ilk tam yedeklemesi hariç olur. Tam yedekleme, yılın 16. haftası üzerinde alınmış olması dışında, 10 yıl boyunca tutulacaktır. 

Aşağıdaki tabloda, şu ilke için uzun süreli yedeklemelerin temposunda ve süre sonu gösterilmektedir:

W = 12 hafta (84 gün), z = 12 ay (365 gün), Y = 10 yıl (3650 gün), WeekOfYear = 15 (15 Nisan 'dan sonra hafta)

   ![LTR örneği](./media/long-term-retention-overview/ltr-example.png)


Yukarıdaki ilkeyi değiştirir ve W = 0 (haftalık yedeklemeler yok) seçeneğini ayarlarsanız, yedekleme kopyalarının temposunda, yukarıdaki tabloda vurgulanan tarihlerle gösterildiği gibi değişecektir. Bu yedeklemelerin tutulması için gereken depolama miktarı uygun şekilde azaltıyordu. 

> [!IMPORTANT]
> Tek bir LTR yedeklemelerin zamanlaması Azure tarafından denetlenir. Bir LTR yedeklemesini el ile oluşturamaz veya yedekleme oluşturma zamanlamasını kontrol edebilirsiniz. Bir LTR ilkesini yapılandırdıktan sonra, ilk LTR yedeklemenin kullanılabilir yedeklemeler listesinde gösterilmesi 7 güne kadar sürebilir.  


## <a name="geo-replication-and-long-term-backup-retention"></a>Coğrafi çoğaltma ve uzun süreli yedekleme saklama

İş sürekliliği çözümünüz olarak etkin coğrafi çoğaltma veya yük devretme grupları kullanıyorsanız, son yük devretme işlemleri için hazırlık yapmanız ve ikincil veritabanında veya örnekte aynı LTR ilkesini yapılandırmanız gerekir. Yedekleme, ikincgöre oluşturulmadığından, LTR depolama maliyetiniz artmayacaktır. Yedeklemeler yalnızca ikincili birincil hale geldiğinde yedeklemeler oluşturulacaktır. Yük devretme tetiklendiğinde ve birincil ikincil bölgeye geçirildiğinde, LTR yedeklemelerin kesintiye uğratılmamış olarak oluşturulmasını sağlar. 

> [!NOTE]
> Özgün birincil veritabanı, yük devretmeye neden olan bir kesintiden kurtarıldığında yeni bir ikincil olur. Bu nedenle, yedekleme oluşturma SÜRDÜRÜLMEYECEKTİR ve mevcut LTR ilkesi, birincil olana kadar etkili olmayacaktır. 

## <a name="sql-managed-instance-support"></a>SQL yönetilen örnek desteği

Azure SQL yönetilen örneği ile uzun süreli yedek saklama kullanılması aşağıdaki sınırlamalara sahiptir:

- **Sınırlı genel önizleme** -bu ÖNIZLEME yalnızca EA ve CSP aboneliklerinde kullanılabilir ve sınırlı kullanılabilirliğe tabidir.  
- [**Yalnızca PowerShell**](../managed-instance/long-term-backup-retention-configure.md) -şu anda Azure Portal desteği yok. LTR, PowerShell kullanılarak etkinleştirilmelidir. 

Kayıt istemek için bir [Azure destek bileti](https://azure.microsoft.com/support/create-ticket/)oluşturun. Sorun türü için teknik sorunu seçin, hizmet için SQL yönetilen örneği seçin ve sorun türü için **yedekleme, geri yükleme ve iş sürekliliği/uzun süreli yedekleme saklama '** yı seçin. İsteğiniz içinde, SQL yönetilen örneği için LTR 'ın sınırlı genel önizlemesine kaydedilmesini istediğinizi unutmayın.

## <a name="configure-long-term-backup-retention"></a>Uzun süreli yedek saklama yapılandırma

Azure SQL veritabanı için Azure portal ve PowerShell 'i ve Azure SQL yönetilen örneği için PowerShell 'i kullanarak uzun süreli yedek saklama yapılandırabilirsiniz. Bir veritabanını LTR depolamadan geri yüklemek için belirli bir yedeklemeyi zaman damgasına göre seçebilirsiniz. Veritabanı, özgün veritabanıyla aynı abonelikte bulunan mevcut herhangi bir sunucuya veya yönetilen örneğe geri yüklenebilir.

Uzun süreli saklama veya Azure portal veya PowerShell kullanarak bir veritabanını SQL veritabanı yedeğinden geri yükleme hakkında bilgi edinmek için bkz. [Azure SQL veritabanı uzun süreli yedekleme bekletmesini yönetme](long-term-backup-retention-configure.md)

Azure SQL yönetilen örneği için PowerShell 'i kullanarak uzun süreli saklama veya bir veritabanını yedekten geri yükleme hakkında bilgi edinmek için bkz. [Azure SQL yönetilen örnek uzun süreli yedekleme saklama](../managed-instance/long-term-backup-retention-configure.md).

Bir veritabanını LTR depolamadan geri yüklemek için belirli bir yedeklemeyi zaman damgasına göre seçebilirsiniz. Veritabanı, özgün veritabanıyla aynı abonelikte bulunan mevcut herhangi bir sunucuya geri yüklenebilir. Veritabanınızı bir LTR yedeklemesinden nasıl geri yükleyeceğinizi öğrenmek için, Azure portal veya PowerShell 'i kullanarak bkz. [Azure SQL veritabanı uzun süreli yedekleme bekletmesini yönetme](long-term-backup-retention-configure.md). İsteğiniz içinde, SQL yönetilen örneği için LTR 'ın sınırlı genel önizlemesine kaydedilmesini istediğiniz durumu girin.

## <a name="next-steps"></a>Sonraki adımlar

Veritabanı yedeklemeleri verileri yanlışlıkla bozulma veya silme işlemlerini koruduğundan, herhangi bir iş sürekliliği ve olağanüstü durum kurtarma stratejisinin önemli bir parçasıdır. Diğer SQL veritabanı iş sürekliliği çözümleri hakkında bilgi edinmek için bkz. [iş sürekliliği genel bakış](business-continuity-high-availability-disaster-recover-hadr-overview.md).
 
