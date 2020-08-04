---
title: Azure Veri Paylaşımı sorunlarını giderme
description: Azure veri paylaşımı ile veri paylaşımları oluştururken veya alırken davetiye ve hatalarla ilgili sorunları nasıl giderebileceğinizi öğrenin.
services: data-share
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: troubleshooting
ms.date: 07/30/2020
ms.openlocfilehash: 0fb2f9dd156d18705308b41ef8d6b015b3b6d71b
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87534456"
---
# <a name="troubleshoot-common-issues-in-azure-data-share"></a>Azure veri paylaşımında sık karşılaşılan sorunları giderme 

Bu makalede, Azure veri paylaşımında yaygın sorunların nasıl giderileceği gösterilmektedir. 

## <a name="azure-data-share-invitations"></a>Azure Data Share davetleri 

Bazı durumlarda, yeni bir kullanıcı gönderilen e-posta **davetinde daveti kabul et** ' i tıklattığında, bu kişiler boş bir davetiye listesi ile sunulabilir. 

![Davetiye yok](media/no-invites.png)

Bunun nedeni aşağıdakilerden biri olabilir:

* **Azure veri paylaşma hizmeti, Azure kiracısında herhangi bir Azure aboneliğinin kaynak sağlayıcısı olarak kayıtlı değil.** Azure kiracınızda veri paylaşma kaynağı yoksa bu sorunla karşılaşırsınız. Bir Azure veri paylaşma kaynağı oluşturduğunuzda, kaynak sağlayıcıyı otomatik olarak Azure aboneliğinize kaydeder. Ayrıca, bu adımları izleyerek veri paylaşma hizmetini el ile de kaydedebilirsiniz. Bu adımları tamamlayabilmeniz için Azure katkıda bulunan rolüne sahip olmanız gerekir.

    1. Azure portal **abonelikler** ' e gidin
    1. Azure veri paylaşımında kaynak oluşturmak için kullanmak istediğiniz aboneliği seçin
    1. **Kaynak sağlayıcıları** ' na tıklayın
    1. **Microsoft. DataShare** araması yapın
    1. **Kaydet** 'e tıklayın 

    Bu adımları gerçekleştirmek için [katkıda bulunan Azure rolüne](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) sahip olmanız gerekir. 

* **Davet, Azure oturum açma e-postanız yerine e-posta diğer adınızla gönderilir.** Azure Data Share hizmetini kaydettiniz veya Azure kiracısında zaten bir veri paylaşma kaynağı oluşturduysanız, ancak yine de daveti göremiyorsanız, bu, sağlayıcının Azure oturum açma e-posta adresiniz yerine e-posta diğer adınızı alıcı olarak girdiğinden olabilir. Veri sağlayıcınızla iletişim kurun ve daveti, e-posta diğer adınızla değil, Azure oturum açma e-posta adresinize gönderdiklerinden emin olun.

* **Davet zaten kabul edildi.** E-postadaki bağlantı, sizi yalnızca bekleyen davetleri listeleyen Azure portal veri paylaşma daveti sayfasına götürür. Daveti zaten kabul ettiyseniz, veri paylaşma daveti sayfasında artık görünmez. Alınan paylaşımları görüntülemek ve hedef Azure Veri Gezgini kümesi ayarınızı yapılandırmak için ' i daveti kabul etmek üzere kullandığınız veri paylaşım kaynağınız ile devam edin.

## <a name="error-when-creating-or-receiving-a-new-share"></a>Yeni bir paylaşma oluştururken veya alınırken hata oluştu

"Veri kümeleri eklenemedi"

"Veri kümeleri eşleştirilemedi"

"Veri paylaşma kaynağı x erişimi y 'ye verilemedi"

"X için uygun izinleriniz yok"

"Azure veri paylaşma hesabı için bir veya daha fazla seçili kaynaklarınızdan yazma izinleri eklenemedi"

Yeni bir paylaşma veya veri kümeleri eşleme oluştururken yukarıdaki hatalardan birini alırsanız, bu, Azure veri deposuna yetersiz izinler nedeniyle olabilir. Gerekli izinler için [rol ve gereksinimlere](concepts-roles-permissions.md) bakın. 

Genellikle katkıda bulunan rolünde bulunan bir Azure veri deposundaki verileri paylaştırmak veya almak için yazma izninizin olması gerekir. 

Azure veri deposundan ilk kez veri paylaşıyorsanız veya alıyorsanız, genellikle sahip rolünde bulunan *Microsoft. Authorization/role atama/yazma* iznine de ihtiyacınız vardır. Azure veri deposu kaynağını oluşturmuş olsanız bile, kaynak sahibini otomatik olarak yapmaz. Uygun izne sahip olan Azure veri paylaşma hizmeti, veri deposu kaynağının yönetilen kimlik erişimini otomatik olarak verir. Bu işlemin etkili olması birkaç dakika sürebilir. Bu gecikme nedeniyle hata yaşarsanız birkaç dakika sonra yeniden deneyin.

SQL tabanlı paylaşım ek izinler gerektirir. Ayrıntılar için bkz. SQL tabanlı Paylaşım sorunlarını giderme.

## <a name="troubleshooting-sql-based-sharing"></a>SQL tabanlı paylaşım sorunlarını giderme

"Kullanıcı x, SQL veritabanında yok"

SQL tabanlı bir kaynaktan veri kümesi eklerken bu hatayı alırsanız, bunun nedeni, SQL veritabanında Azure veri paylaşımının yönettiği kimliği için bir Kullanıcı oluşturmadınız olabilir.  Bu sorunu çözmek için aşağıdaki betiği çalıştırın:

```sql
    create user "<share_acct_name>" from external provider; 
    exec sp_addrolemember db_datareader, "<share_acct_name>";
```      
Veri kümesini SQL tabanlı bir hedefe eşlerken bu hatayı alırsanız, bunun nedeni SQL Server Azure veri paylaşımında yönetilen kimliği için bir kullanıcı oluşturmanız olabilir.  Bu sorunu çözmek için aşağıdaki betiği çalıştırın:

```sql
    create user "<share_acc_name>" from external provider; 
    exec sp_addrolemember db_datareader, "<share_acc_name>"; 
    exec sp_addrolemember db_datawriter, "<share_acc_name>"; 
    exec sp_addrolemember db_ddladmin, "<share_acc_name>";
```
*<share_acc_name>* veri paylaşma kaynağınızın adı olduğunu unutmayın.      

[Verilerinizi paylaşma](share-your-data.md) bölümünde listelenen tüm önkoşulları izlediğinizden emin olun ve [veri al öğreticisini kabul edin](subscribe-to-data-share.md) .

## <a name="snapshot-failed"></a>Anlık görüntü başarısız oldu
Çeşitli nedenlerden dolayı anlık görüntü başarısız olabilir. Anlık görüntünün başlangıç saatine ve sonra her bir veri kümesinin durumuna tıklayarak ayrıntılı hata iletisi bulabilirsiniz. 

Hata iletisi izinle ilişkiliyse, veri paylaşma hizmeti 'nin gerekli izne sahip olduğunu doğrulayın. Ayrıntılar için [Roller ve gereksinimlere](concepts-roles-permissions.md) bakın. İlk kez bir anlık görüntü çekiyorsunuz, veri paylaşımının kaynağı için Azure veri deposuna erişim verilmesi birkaç dakika sürebilir. Birkaç dakika bekleyip yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Verileri paylaşmaya nasıl başlayacağınızı öğrenmek için [verilerinizi paylaşma](share-your-data.md) öğreticiye geçin. 

Veri alma hakkında bilgi edinmek için, [verileri kabul etme ve alma](subscribe-to-data-share.md) öğreticisine geçin.

