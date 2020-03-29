---
title: 'Azure AD Connect: LocalDB 10GB limit sorunundan nasıl kurtarılabilen | Microsoft Dokümanlar'
description: Bu konu, LocalDB 10GB sınır sorunuyla karşılaştığında Azure AD Connect Eşitleme Hizmeti'nin nasıl kurtarılabildiğini açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 41d081af-ed89-4e17-be34-14f7e80ae358
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d420c64c5834f7d3cb11d2f5f59e3ed85a54891
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60386933"
---
# <a name="azure-ad-connect-how-to-recover-from-localdb-10-gb-limit"></a>Azure AD Connect: LocalDB 10 GB sınırından kurtarma
Azure AD Connect’e kimlik verilerini depolamak için bir SQL Server veritabanı gerekiyor. Azure AD Connect ile yüklenen varsayılan SQL Server 2012 Express LocalDB’yi kullanabileceğiniz gibi, kendi tam SQL’nizi de kullanabilirsiniz. SQL Server Express 10 GB boyut sınırını uygular. LocalDB’yi kullanırken bu sınıra ulaşıldığında, Azure AD Connect Eşitleme Hizmeti artık düzgün başlatılamaz veya eşitleme yapamaz. Bu makalede kurtarma adımları sağlar.

## <a name="symptoms"></a>Belirtiler
İki yaygın belirti vardır:

* Azure AD Connect Eşitleme Hizmeti **çalışıyor,** ancak *"durduruldu-veritabanı-disk dolu"* hatasıyla eşitleme de başarısız oluyor.

* Azure AD Connect Eşitleme Hizmeti **başlatılamıyor.** Hizmeti başlatmayı denediğinizde, 6323 olayı ve hata iletisi "SQL Server disk alanı dışında olduğu için *sunucu bir hatayla karşılaştı."*

## <a name="short-term-recovery-steps"></a>Kısa vadeli kurtarma adımları
Bu bölümde, Azure AD Connect Eşitleme Hizmeti'nin çalışmaya devam etmesi için gereken DB alanını geri almak için adımlar sağlanmaktadır. Adımlar şunlardır:
1. [Eşitleme Hizmeti durumunu belirleme](#determine-the-synchronization-service-status)
2. [Veritabanını küçültme](#shrink-the-database)
3. [Çalışma geçmişi verilerini silme](#delete-run-history-data)
4. [Çalışma geçmişi verileri için bekletme süresini kısaltma](#shorten-retention-period-for-run-history-data)

### <a name="determine-the-synchronization-service-status"></a>Eşitleme Hizmeti durumunu belirleme
İlk olarak, Eşitleme Hizmeti'nin hala çalışıp çalışmadığını belirleyin:

1. Azure AD Connect sunucunuzda yönetici olarak oturum açın.

2. Servis **Kontrol Yöneticisi'ne**gidin.

3. Microsoft Azure **AD Eşitleme**durumunu denetleyin.


4. Çalışıyorsa, hizmeti durdurmayın veya yeniden başlatmayın. [Veritabanı adımını küçült'ün](#shrink-the-database) ve [geçmiş verilerini sil'e](#delete-run-history-data) gidin.

5. Çalışmıyorsa, hizmeti başlatmayı deneyin. Hizmet başarıyla başlarsa, [veritabanı adımını küçült'ün](#shrink-the-database) u yedin ve [geçmiş verilerini sil'e](#delete-run-history-data) gidin. Aksi takdirde, [veritabanı adımını küçültmeye devam edin.](#shrink-the-database)

### <a name="shrink-the-database"></a>Veritabanını küçültme
Eşitleme Hizmetini başlatmak için yeterli DB alanı boşaltmak için Shrink işlemini kullanın. Veritabanındaki beyaz boşlukları kaldırarak DB alanını boşaltır. Her zaman yer kurtarabileceğiniz garanti edilmez gibi bu adım en iyi çabadır. Shrink işlemi hakkında daha fazla bilgi edinmek için bu makaleyi okuyun [Bir veritabanını küçültün.](https://msdn.microsoft.com/library/ms189035.aspx)

> [!IMPORTANT]
> Eşitleme Hizmetini çalıştırabiliyorsanız bu adımı atlayın. Artan parçalanma nedeniyle düşük performansa yol açabileceğinden, SQL DB'nin küçültilmesi önerilmez.

Azure AD Connect için oluşturulan veritabanının adı **ADSync'dir.** Shrink işlemini gerçekleştirmek için veritabanının sysadmin veya DBO olarak oturum açmanız gerekir. Azure AD Connect yüklemesi sırasında aşağıdaki hesaplara sysadmin hakları verilir:
* Yerel Yöneticiler
* Azure AD Connect yüklemesini çalıştırmak için kullanılan kullanıcı hesabı.
* Azure AD Connect Senkronizasyon Hizmeti'nin işletim bağlamı olarak kullanılan Eşitleme Hizmeti hesabı.
* Yükleme sırasında oluşturulan yerel grup ADSyncAdmins.

1. Altında `%ProgramFiles%\Microsoft Azure AD Sync\Data` bulunan **ADSync.mdf** ve **ADSync_log.ldf** dosyalarını güvenli bir konuma kopyalayarak veritabanını yedekleyin.

2. Yeni bir PowerShell oturumu başlatın.

3. Klasöre `%ProgramFiles%\Microsoft SQL Server\110\Tools\Binn`gidin.

4. Bir sysadmin veya `./SQLCMD.EXE -S "(localdb)\.\ADSync" -U <Username> -P <Password>`veritabanı DBO kimlik kullanarak komutu çalıştırarak **sqlcmd** yardımcı programı başlatın.

5. Veritabanını küçültmek için, sqlcmd komut istemi `DBCC Shrinkdatabase(ADSync,1);`(1 `GO`>) girin, sonraki satırda takip.

6. İşlem başarılı olursa, Eşitleme Hizmetini yeniden başlatmayı deneyin. Eşitleme Hizmetini başlatabilirseniz, geçmiş [verilerini sil'e](#delete-run-history-data) gidin. Değilse, Destek'e başvurun.

### <a name="delete-run-history-data"></a>Çalışma geçmişi verilerini silme
Varsayılan olarak, Azure AD Connect yedi güne kadar çalışan geçmiş verilerini korur. Bu adımda, Azure AD Connect Eşitleme Hizmeti'nin yeniden eşitlenmeye başlayabilmesi için DB alanını geri almak için çalışan geçmiş verilerini siliyoruz.

1. START → Eşitleme Hizmeti'ne giderek **Eşitleme Hizmet Yöneticisi'ni** başlatın.

2. **İşlemler** sekmesine gidin.

3. **Eylemler**altında, **Açık Çalışır'ı**seçin ...

4. **Tüm çalıştırmaları temizle'yi** veya daha önce **çalıştır'ı temizle'yi seçebilirsiniz... tarih \<>** seçenek. İki günden eski olan çalışma geçmişi verilerini temizleyerek başlamanız önerilir. DB boyutu sorunuyla karşınıza devam ederseniz, **tüm çalıştırmaları temizle** seçeneğini belirleyin.

### <a name="shorten-retention-period-for-run-history-data"></a>Çalışma geçmişi verileri için bekletme süresini kısaltma
Bu adım, birden çok eşitleme döngüsünden sonra 10 GB'lık sınır sorununa girme olasılığını azaltmaktır.

1. Yeni bir PowerShell oturumu açın.

2. Geçerli `Get-ADSyncScheduler` bekletme süresini belirten PurgeRunHistoryInterval özelliğini çalıştırın ve not alın.

3. Bekletme süresini iki güne ayarlamak için çalıştırın. `Set-ADSyncScheduler -PurgeRunHistoryInterval 2.00:00:00` Bekletme süresini uygun şekilde ayarlayın.

## <a name="long-term-solution--migrate-to-full-sql"></a>Uzun vadeli çözüm – Tam SQL'e geçir
Genel olarak sorun, 10 GB veritabanı boyutunun artık Azure AD Connect'in şirket içi Active Directory'nizi Azure AD ile senkronize etmesi için yeterli olmadığını gösterir. SQL sunucusunun tam sürümünü kullanmaya geçmeniz önerilir. Mevcut Azure AD Connect dağıtımının LocalDB’sini doğrudan tüm SQL sürümünün veritabanıyla değiştiremezsiniz. Bunun yerine, tam SQL sürümü içeren yeni bir Azure AD Connect sunucusu dağıtmanız gerekir. Yeni Azure AD Connect sunucusunun (SQL DB ile), mevcut Azure AD Connect sunucusunun (LocalDB ile) yanında hazırlık sunucusu olarak dağıtıldığı durumlarda, Swing geçişi yapmanız önerilir. 
* Azure AD Connect ile uzak SQL’i yapılandırma yönergeleri için, [Azure AD Connect özel yüklemesi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom) makalesine bakın.
* Azure AD Connect yükseltmesinde Swing geçişi için, [Azure AD Connect: Önceki bir sürümden en son sürümü yükseltme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#swing-migration) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
