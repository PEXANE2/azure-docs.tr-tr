---
title: Uzun süreli yedek saklama
description: Azure SQL veritabanı 'nın, uzun süreli saklama ilkesi aracılığıyla 10 yıla kadar tam veritabanı yedeklerini depolamayı nasıl desteklediğini öğrenin.
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
ms.openlocfilehash: 6fc5fab9ae61c8c8ade9260b32078ffa430b077f
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83771409"
---
# <a name="azure-sql-database-long-term-retention"></a>Azure SQL veritabanı uzun süreli saklama

Birçok uygulamanın, veritabanı yedeklerini Azure SQL veritabanı [Otomatik yedeklemeleri](sql-database-automated-backups.md)tarafından belirtilen 7-35 günden daha fazla tutmanız gereken mevzuata, uyumluluk veya diğer iş amaçları vardır. Uzun süreli saklama (LTR) özelliğini kullanarak, belirtilen SQL veritabanı tam yedeklemelerini, Okuma Erişimli Coğrafi olarak yedekli depolama ile 10 yıla kadar Azure Blob depolamada saklayabilirsiniz. Daha sonra tüm yedeklemeyi yeni bir veritabanı olarak geri yükleyebilirsiniz. Azure depolama artıklığı hakkında daha fazla bilgi için bkz. [Azure Storage yedekliği](../storage/common/storage-redundancy.md). 

Uzun süreli saklama, tek ve havuza alınmış veritabanları için etkinleştirilebilir ve Azure SQL veritabanı yönetilen örnekleri için sınırlı genel önizlemede bulunabilir. 

> [!NOTE]
> [Yalnızca kopya veritabanı yedeklerini](https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server) 35 gün daha fazla LTR 'e alternatif olarak ZAMANLAMAK Için SQL Agent işlerini kullanabilirsiniz.


## <a name="how-sql-database-long-term-retention-works"></a>SQL Veritabanı uzun süreli saklama nasıl çalışır?

Uzun süreli yedek saklama (LTR), nokta-saat geri yüklemeyi (ıNR) etkinleştirmek için [otomatik olarak oluşturulan](sql-database-automated-backups.md) tam veritabanı yedeklemelerini kullanır. Bir LTR ilkesi yapılandırılmışsa, bu yedeklemeler uzun vadeli depolama için farklı bloblara kopyalanır. Kopya, veritabanı iş yükü üzerinde performans etkisi olmayan bir arka plan işi. Her SQL veritabanı için LTR ilkesi, LTR yedeklemelerin oluşturulma sıklığını da belirtebilir.

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

   ![LTR örneği](./media/sql-database-long-term-retention/ltr-example.png)



Yukarıdaki ilkeyi değiştirir ve W = 0 (haftalık yedeklemeler yok) seçeneğini ayarlarsanız, yedekleme kopyalarının temposunda, yukarıdaki tabloda vurgulanan tarihlerle gösterildiği gibi değişecektir. Bu yedeklemelerin tutulması için gereken depolama miktarı uygun şekilde azaltıyordu. 

> [!IMPORTANT]
> Tek bir LTR yedeklemelerin zamanlaması Azure SQL veritabanı tarafından denetlenir. El ile bir LTR yedeklemesi oluşturamaz veya yedekleme oluşturma zamanlamasını kontrol edebilirsiniz. Bir LTR ilkesini yapılandırdıktan sonra, ilk LTR yedeklemenin kullanılabilir yedeklemeler listesinde gösterilmesi 7 güne kadar sürebilir.  
> 

## <a name="geo-replication-and-long-term-backup-retention"></a>Coğrafi çoğaltma ve uzun süreli yedekleme saklama

İş sürekliliği çözümünüz olarak etkin coğrafi çoğaltma veya yük devretme grupları kullanıyorsanız, son yük devretme işlemleri için hazırlık yapmanız ve coğrafi ikincil veritabanında aynı LTR ilkesini yapılandırmanız gerekir. Yedekleme, ikincgöre oluşturulmadığından, LTR depolama maliyetiniz artmaz. Yalnızca ikincili birincil hale geldiğinde yedeklemeler oluşturulacaktır. Yük devretme tetiklendiğinde ve birincil ikincil bölgeye geçirildiğinde, LTR yedeklemelerin kesintiye uğratılmamış olarak oluşturulmasını sağlar. 

> [!NOTE]
> Özgün birincil veritabanı, yük devretmeye neden olan bir kesintiden kurtarıldığında yeni bir ikincil olur. Bu nedenle, yedekleme oluşturma SÜRDÜRÜLMEYECEKTİR ve mevcut LTR ilkesi, birincil olana kadar etkili olmayacaktır. 

## <a name="managed-instance-support"></a>Yönetilen örnek desteği

Azure SQL veritabanı yönetilen örnekleri ile uzun süreli yedek saklama kullanılması aşağıdaki sınırlamalara sahiptir:

- **Sınırlı genel önizleme** -bu ÖNIZLEME yalnızca EA ve CSP aboneliklerinde kullanılabilir ve sınırlı kullanılabilirliğe tabidir.  
- [**Yalnızca PowerShell**](sql-database-managed-instance-long-term-backup-retention-configure.md) -şu anda Azure Portal desteği yok. LTR, PowerShell kullanılarak etkinleştirilmelidir. 

Kayıt istemek için bir [Azure destek bileti](https://azure.microsoft.com/support/create-ticket/)oluşturun. Sorun türü için teknik sorunu seçin, hizmet için SQL veritabanı yönetilen örneğini seçin ve sorun türü için **yedekleme, geri yükleme ve iş sürekliliği/uzun süreli yedekleme saklama ' yı**seçin. İsteğiniz içinde, yönetilen örnek için LTR 'nin sınırlı genel önizlemesine kaydedilmesini istediğinizi unutmayın.

## <a name="configure-long-term-backup-retention"></a>Uzun süreli yedek saklama yapılandırma

Azure portal veya PowerShell 'i kullanarak uzun süreli saklama yapılandırmayı öğrenmek için bkz. [Azure SQL veritabanı uzun süreli yedekleme saklama](sql-database-long-term-backup-retention-configure.md).

## <a name="restore-database-from-ltr-backup"></a>Veritabanı, LTR yedeğinden geri yükle

Bir veritabanını LTR depolamadan geri yüklemek için belirli bir yedeklemeyi zaman damgasına göre seçebilirsiniz. Veritabanı, özgün veritabanıyla aynı abonelikte bulunan mevcut herhangi bir sunucuya geri yüklenebilir. Veritabanınızı bir LTR yedeklemesinden nasıl geri yükleyeceğinizi öğrenmek için, Azure portal veya PowerShell 'i kullanarak bkz. [Azure SQL veritabanı uzun süreli yedekleme bekletmesini yönetme](sql-database-long-term-backup-retention-configure.md). İsteğiniz içinde, yönetilen örnek için LTR 'ın sınırlı genel önizlemesine kaydedilmesini istediğiniz durumu girin.

## <a name="next-steps"></a>Sonraki adımlar

Veritabanı yedeklemeleri verileri yanlışlıkla bozulma veya silme işlemlerini koruduğundan, herhangi bir iş sürekliliği ve olağanüstü durum kurtarma stratejisinin önemli bir parçasıdır. Diğer SQL veritabanı iş sürekliliği çözümleri hakkında bilgi edinmek için bkz. [iş sürekliliği genel bakış](sql-database-business-continuity.md).
