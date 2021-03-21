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
author: shkale
ms.author: shkale
ms.reviewer: mathoma, sstein
ms.date: 02/25/2021
ms.openlocfilehash: b1ba3f98f39511bcebf94502ed749d7cba1fc71b
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102184058"
---
# <a name="long-term-retention---azure-sql-database-and-azure-sql-managed-instance"></a>Uzun süreli saklama-Azure SQL veritabanı ve Azure SQL yönetilen örneği

Birçok uygulamanın, Azure SQL veritabanı ve Azure SQL yönetilen örnek [Otomatik yedeklemeleri](automated-backups-overview.md)tarafından belirtilen 7-35 günden daha fazla veritabanı yedeklerini tutmanız gereken mevzuata, uyumluluk veya diğer iş amaçları vardır. Uzun süreli saklama (LTR) özelliğini kullanarak, belirtilen SQL veritabanı ve SQL yönetilen örnek tam yedeklemelerini, en fazla 10 yıla kadar [yapılandırılmış yedeklerle](automated-backups-overview.md#backup-storage-redundancy) Azure Blob depolama alanında saklayabilirsiniz. Bu durumda, LTR yedeklemeleri yeni bir veritabanı olarak geri yüklenebilir.

Azure SQL veritabanı için uzun süreli saklama etkinleştirilebilir ve Azure SQL yönetilen örneği için genel önizlemede kullanılabilir. Bu makalede uzun süreli saklama hakkında kavramsal bir genel bakış sunulmaktadır. Uzun süreli saklama yapılandırmak için bkz. [Azure SQL VERITABANı LTR 'Yi yapılandırma](long-term-backup-retention-configure.md) ve [Azure SQL yönetilen örnek LTR 'yi yapılandırma](../managed-instance/long-term-backup-retention-configure.md). 

> [!NOTE]
> [Yalnızca kopya veritabanı yedeklerini](/sql/relational-databases/backup-restore/copy-only-backups-sql-server) 35 gün daha fazla LTR 'e alternatif olarak ZAMANLAMAK Için SQL Agent işlerini kullanabilirsiniz.

> [!IMPORTANT]
> Yönetilen örnekteki uzun süreli saklama, şu anda yalnızca Azure genel bölgelerinde genel önizlemede kullanılabilir. 


## <a name="how-long-term-retention-works"></a>Uzun vadeli bekletme nasıl kullanılır?
     
Uzun süreli yedek saklama (LTR), nokta-saat geri yüklemeyi (ıNR) etkinleştirmek için [otomatik olarak oluşturulan](automated-backups-overview.md) tam veritabanı yedeklemelerini kullanır. Bir LTR ilkesi yapılandırılmışsa, bu yedeklemeler uzun vadeli depolama için farklı bloblara kopyalanır. Kopya, veritabanı iş yükü üzerinde performans etkisi olmayan bir arka plan işi. SQL veritabanı 'ndaki her bir veritabanı için LTR ilkesi, LTR yedeklemelerin oluşturulma sıklığını da belirtebilir.

LTR özelliğini etkinleştirmek için, dört parametre birleşimini kullanarak bir ilke tanımlayabilirsiniz: haftalık yedekleme bekletme (W), aylık yedekleme bekletme (e), yıllık yedekleme bekletme (Y) ve yılın haftası (WeekOfYear). W belirtirseniz, her hafta bir yedekleme uzun vadeli depolamaya kopyalanacaktır. D belirtirseniz, her ayın ilk yedeklemesi uzun vadeli depolamaya kopyalanacaktır. Y belirtirseniz, WeekOfYear tarafından belirtilen hafta boyunca bir yedekleme uzun vadeli depolamaya kopyalanacaktır. Belirtilen WeekOfYear, ilke yapılandırıldığında, ilk LTR yedeklemesi aşağıdaki yılda oluşturulur. Her yedekleme, LTR yedeklemesi oluşturulduğunda yapılandırılan ilke parametrelerine göre uzun vadeli depolamada tutulur.

> [!NOTE]
> LTR ilkesinde yapılan herhangi bir değişiklik yalnızca gelecekteki yedeklemeler için geçerlidir. Örneğin, haftalık yedekleme bekletme (W), aylık yedekleme bekletme (e) veya yıllık yedekleme bekletme (Y) değiştirilirse, yeni bekletme ayarı yalnızca yeni yedeklemeler için geçerlidir. Mevcut yedeklemelerin saklama süresi değiştirilmez. Amacınız, saklama süresi dolmadan eski LTR yedeklemeleri silmek istiyorsanız [yedeklemeleri el ile silmeniz](./long-term-backup-retention-configure.md#delete-ltr-backups)gerekir.
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


## <a name="configure-long-term-backup-retention"></a>Uzun süreli yedek saklama yapılandırma

Azure SQL veritabanı ve Azure SQL yönetilen örneği için Azure portal ve PowerShell 'i kullanarak uzun süreli yedek saklama yapılandırabilirsiniz. Bir veritabanını LTR depolamadan geri yüklemek için belirli bir yedeklemeyi zaman damgasına göre seçebilirsiniz. Veritabanı, özgün veritabanıyla aynı abonelikte bulunan mevcut herhangi bir sunucuya veya yönetilen örneğe geri yüklenebilir.

Azure portal veya PowerShell 'i kullanarak SQL veritabanı için uzun süreli saklama veya bir veritabanını yedekten geri yükleme hakkında bilgi edinmek için bkz. [Azure SQL veritabanı uzun süreli yedekleme bekletmesini yönetme](long-term-backup-retention-configure.md).

Azure SQL yönetilen örneği için PowerShell 'i kullanarak uzun süreli saklama veya bir veritabanını yedekten geri yükleme hakkında bilgi edinmek için bkz. [Azure SQL yönetilen örnek uzun süreli yedekleme saklama](../managed-instance/long-term-backup-retention-configure.md).

Bir veritabanını LTR depolamadan geri yüklemek için belirli bir yedeklemeyi zaman damgasına göre seçebilirsiniz. Veritabanı, özgün veritabanıyla aynı abonelikte bulunan mevcut herhangi bir sunucuya geri yüklenebilir. Veritabanınızı bir LTR yedeklemesinden nasıl geri yükleyeceğinizi öğrenmek için, Azure portal veya PowerShell 'i kullanarak bkz. [Azure SQL veritabanı uzun süreli yedekleme bekletmesini yönetme](long-term-backup-retention-configure.md). 

## <a name="next-steps"></a>Sonraki adımlar

Veritabanı yedeklemeleri verileri yanlışlıkla bozulma veya silme işlemlerini koruduğundan, herhangi bir iş sürekliliği ve olağanüstü durum kurtarma stratejisinin önemli bir parçasıdır. 

- Diğer SQL veritabanı iş sürekliliği çözümleri hakkında bilgi edinmek için bkz. [iş sürekliliği genel bakış](business-continuity-high-availability-disaster-recover-hadr-overview.md).
- Hizmet tarafından oluşturulan otomatik yedekler hakkında bilgi edinmek için bkz. [otomatik yedekler](../database/automated-backups-overview.md)
