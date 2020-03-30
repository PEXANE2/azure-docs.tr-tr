---
title: Azure Veri Paylaşımı sorunlarını giderme
description: Azure Veri Paylaşımı ile veri paylaşımı oluştururken veya alırken davetlerle ve hatalarla ilgili sorunları nasıl gidereceklerini öğrenin.
services: data-share
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: troubleshooting
ms.date: 07/10/2019
ms.openlocfilehash: 901f2b56bc045dc9a9837dd18b2e6ce7169aa3b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76964235"
---
# <a name="troubleshoot-common-issues-in-azure-data-share"></a>Azure Veri Paylaşımı'nda sık karşılaşılan sorunları giderme 

Bu makalede, Azure Veri Paylaşımı için sık karşılaşılan sorunları nasıl gideriylen giderilen gösterilmektedir. 

## <a name="azure-data-share-invitations"></a>Azure Veri Paylaşımı davetiyeleri 

Bazı durumlarda, yeni bir kullanıcı gönderilen e-posta davetinden **Daveti Kabul** Et'i tıklattığında, bunlara boş bir davet listesi sunulabilir. 

![Davet yok](media/no-invites.png)

Bunun nedeni aşağıdaki nedenlerden olabilir:

* **Azure Veri Paylaşımı hizmeti, Azure kiracısında herhangi bir Azure aboneliğinin kaynak sağlayıcısı olarak kayıtlı değildir.** Azure kiracınızda Veri Paylaşımı kaynağı yoksa bu sorunla karşılaşırsınız. Bir Azure Veri Paylaşımı kaynağı oluşturduğunuzda, kaynak sağlayıcısını Azure aboneliğinize otomatik olarak kaydeder. Aşağıdaki adımları izleyerek Veri Paylaşımı hizmetini el ile de kaydedebilirsiniz. Bu adımları tamamlamak için Azure Katılımcısı rolüne sahip olmanız gerekir.

    1. Azure portalında **Abonelikler'e** gidin
    1. Azure Veri Paylaşımı kaynağı oluşturmak için kullanmak istediğiniz aboneliği seçin
    1. Kaynak **Sağlayıcılar'a** tıklayın
    1. **Microsoft.DataShare** ara
    1. **Kayıt'yı** tıklatın 

    Bu adımları tamamlamak için [Azure Katılımcır RBAC rolüne](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) sahip olmanız gerekir. 

* **Davet, Azure giriş e-postanız yerine e-posta takma adınıza gönderilir.** Azure Veri Paylaşımı hizmetini kaydettiyseniz veya Azure kiracısında zaten bir Veri Paylaşımı kaynağı oluşturduysanız, ancak yine de daveti göremiyorsanız, bunun nedeni sağlayıcının e-posta takma adınıAzure giriş e-posta adresiniz yerine alıcı olarak girmiş olmasıolabilir. Veri sağlayıcınıza başvurun ve daveti e-posta takma adınıza değil, Azure giriş e-posta adresinize gönderdiklerini emin olun.

* **Davet zaten kabul edildi.** E-postadaki bağlantı sizi Azure portalındaki Veri Paylaşımı Daveti sayfasına götürür ve yalnızca bekleyen davetleri listeler. Daveti zaten kabul ettiyseniz, artık Veri Paylaşımı Daveti sayfasında gösterilmez. Alınan paylaşımları görüntülemek ve hedef Azure Veri Gezgini küme ayarınızı yapılandırmak için daveti kabul etmek için kullandığınız Veri Paylaşımı kaynağınıza devam edin.

## <a name="error-when-creating-or-receiving-a-new-share"></a>Yeni bir pay oluştururken veya alırken hata

"Veri kümeleri eklenemedi"

"Veri kümelerini eşlemekte başarısız olundu"

"Veri Paylaşımı kaynağına x erişimi verilemeyen y"

"X için uygun izinleri yok"

"Seçtiğiniz kaynaklardan birine veya daha fazlasına Azure Veri Paylaşımı hesabı için yazma izinleri ekleyemedik"

Yeni bir paylaşım oluştururken veya veri kümelerini eşlerken yukarıdaki hatalardan herhangi birini alırsanız, bunun nedeni Azure veri deposuna yapılan yetersiz izinlerden kaynaklanıyor olabilir. Bkz. Gerekli izinler için [Roller ve gereksinimler.](concepts-roles-permissions.md) 

Genellikle Katılımcı rolünde bulunan bir Azure veri deposundan veri paylaşmak veya almak için yazma iznine ihtiyacınız vardır. 

Azure veri deposundan ilk kez veri paylaşıyor veya alıyorsanız, genellikle Sahibi rolünde bulunan *Microsoft.Authorization/rol atamaları/yazma* iznine de ihtiyacınız vardır. Azure veri deposu kaynağını oluşturmuş olsanız bile, otomatik olarak sizi kaynağın sahibi yapmaz. Azure Veri Paylaşımı hizmeti, uygun izinle veri paylaşımı kaynağının veri deposuna yönetilen kimlik erişimini otomatik olarak verir. Bu işlemin etkili olması birkaç dakika sürebilir. Bu gecikme nedeniyle başarısızlıkla karşılaşırsanız, birkaç dakika içinde yeniden deneyin.

SQL tabanlı paylaşım ek izinler gerektirir. Ayrıntılar için Sorun Giderme SQL tabanlı paylaşıma bakın.

## <a name="troubleshooting-sql-based-sharing"></a>SQL tabanlı paylaşım sorunlarını giderme

"Kullanıcı x SQL veritabanında yok"

Bu hatayı, SQL tabanlı bir kaynaktan veri kümesi eklerken alırsanız, bunun nedeni SQL Server'ınızda Azure Veri Paylaşımı yönetilen kimliği için bir kullanıcı oluşturmamış olabilirsiniz.  Bu sorunu gidermek için aşağıdaki komut dosyasını çalıştırın:

```sql
    create user "<share_acct_name>" from external provider; 
    exec sp_addrolemember db_datareader, "<share_acct_name>";
```      
Bu hatayı, veri kümesini SQL tabanlı bir hedefe eşleme yaparken alırsanız, bunun nedeni SQL Server'ınızda Azure Veri Paylaşımı yönetilen kimliği için bir kullanıcı oluşturmamış olabilirsiniz.  Bu sorunu gidermek için aşağıdaki komut dosyasını çalıştırın:

```sql
    create user "<share_acc_name>" from external provider; 
    exec sp_addrolemember db_datareader, "<share_acc_name>"; 
    exec sp_addrolemember db_datawriter, "<share_acc_name>"; 
    exec sp_addrolemember db_ddladmin, "<share_acc_name>";
```
*<share_acc_name>* Veri Paylaşımı kaynağınızın adı olduğunu unutmayın.      

Verilerinizi Paylaşın'da listelenen tüm ön koşulları takip ettiğinizden ve veri [eğitimini](share-your-data.md) [kabul edip aldığınızdan](subscribe-to-data-share.md) emin olun.

## <a name="snapshot-failed"></a>Anlık görüntü başarısız oldu
Anlık görüntü çeşitli nedenlerden dolayı başarısız olabilir. Anlık görüntünün başlangıç saatini ve ardından her veri kümesinin durumunu tıklatarak ayrıntılı hata iletisi bulabilirsiniz. 

Hata iletisi izinle ilgiliyse, Veri Paylaşımı hizmetinin gerekli izine sahip olduğunu doğrulayın. Ayrıntılar için [Roller ve gereksinimlere](concepts-roles-permissions.md) bakın. İlk anlık görüntü alıyorsanız, Veri Paylaşımı kaynağının Azure veri deposuna erişim hakkı verilmesi birkaç dakika sürebilir. Birkaç dakika bekleyin ve tekrar deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Veri paylaşmaya nasıl başlayacağınızı öğrenmek [için, veri öğreticinizi paylaşmaya devam edin.](share-your-data.md) 

Veri almayı öğrenmek için, veri eğitimi [almaya ve kabul](subscribe-to-data-share.md) etmeye devam edin.

