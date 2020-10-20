---
title: Azure Veri Paylaşımı sorunlarını giderme
description: Azure veri paylaşımı ile veri paylaşımları oluştururken veya alırken davetiye ve hatalarla ilgili sorunları nasıl giderebileceğinizi öğrenin.
services: data-share
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: troubleshooting
ms.date: 10/15/2020
ms.openlocfilehash: a323dec66a3077784ff85deadd4f12086648fb3a
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92220467"
---
# <a name="troubleshoot-common-issues-in-azure-data-share"></a>Azure Veri Paylaşımı'nda sık karşılaşılan sorunları giderme 

Bu makalede, Azure veri paylaşımında yaygın sorunların nasıl giderileceği gösterilmektedir. 

## <a name="azure-data-share-invitations"></a>Azure Data Share davetleri 

Bazı durumlarda, yeni bir kullanıcı gönderilen e-posta **davetinde daveti kabul et** ' i tıklattığında, bu kişiler boş bir davetiye listesi ile sunulabilir. 

![Davetiye yok](media/no-invites.png)

Bunun nedeni şunlar olabilir:

* **Azure Veri Paylaşımı hizmeti Azure kiracısındaki herhangi bir Azure aboneliğinin kaynak sağlayıcısı olarak kaydedilmedi.** Azure kiracınızda Veri Paylaşımı kaynağı yoksa bu sorunla karşılaşırsınız. Azure Veri Paylaşımı kaynağı oluşturduğunuzda, bu kaynak Azure aboneliğinizde kaynak sağlayıcısını otomatik olarak kaydeder. Ayrıca aşağıdaki adımları izleyerek Veri Paylaşımı hizmetini el ile de kaydedebilirsiniz. Bu adımları tamamlamak için Azure Katkıda Bulunanı rolünüz olmalıdır.

    1. Azure portalda **Abonelikler** sayfasına gidin
    1. Azure Veri Paylaşımı kaynağını oluştururken kullanmak istediğiniz aboneliği seçin
    1. **Kaynak Sağlayıcıları**’na tıklayın
    1. **Microsoft.DataShare** araması yapın
    1. **Kaydet**'e tıklayın 

    Bu adımları tamamlayabilmeniz için Azure aboneliği için [Azure katılımcısı rolüne](../role-based-access-control/built-in-roles.md#contributor) sahip olmanız gerekir. 

* **Davet Azure oturum açma e-postanız yerine e-posta diğer adınıza gönderilir.** Azure Veri Paylaşımı hizmetini kaydettiyseniz veya Azure kiracısında zaten bir Veri Paylaşımı kaynağı oluşturduysanız ama hala daveti göremiyorsanız, bunun nedeni sağlayıcının Azure oturum açma e-posta adresiniz yerine e-posta diğer adınızı girmiş olması olabilir. Veri sağlayıcınıza başvurun ve daveti e-posta diğer adınıza değil Azure oturum açma e-posta adresinize gönderdiğinden emin olun.

* **Davet zaten kabul edilmişti.** E-postadaki bağlantı sizi Azure portalda yalnızca bekleyen davetlerin listelendiği Veri Paylaşımı Daveti sayfasına ulaştırır. Daveti zaten kabul ettiyseniz, artık Veri Paylaşımı Daveti sayfasında gösterilmez. Alınan paylaşımları görüntülemek üzere daveti kabul etmek için kullandığınız Veri Paylaşımı kaynağınızla devam edin ve hedef Azure Veri Gezgini küme ayarını yapılandırın.

## <a name="error-when-creating-or-receiving-a-new-share"></a>Yeni bir paylaşma oluştururken veya alınırken hata oluştu

"Veri kümeleri eklenemedi"

"Veri kümeleri eşleştirilemedi"

"Veri paylaşma kaynağı x erişimi y 'ye verilemedi"

"X için uygun izinleriniz yok"

"Azure veri paylaşma hesabı için bir veya daha fazla seçili kaynaklarınızdan yazma izinleri eklenemedi"

Yeni bir paylaşma veya veri kümeleri eşleme oluştururken yukarıdaki hatalardan birini alırsanız, bu, Azure veri deposuna yetersiz izinler nedeniyle olabilir. Gerekli izinler için [rol ve gereksinimlere](concepts-roles-permissions.md) bakın. 

Genellikle katkıda bulunan rolünde bulunan bir Azure veri deposundaki verileri paylaştırmak veya almak için yazma izninizin olması gerekir. 

Azure veri deposundan ilk kez veri paylaşıyorsanız veya alıyorsanız, genellikle sahip rolünde bulunan *Microsoft. Authorization/role atama/yazma* iznine de ihtiyacınız vardır. Azure veri deposu kaynağını oluşturmuş olsanız bile, kaynak sahibini otomatik olarak yapmaz. Uygun izne sahip olan Azure veri paylaşma hizmeti, veri deposu kaynağının yönetilen kimlik erişimini otomatik olarak verir. Bu işlemin etkili olması birkaç dakika sürebilir. Bu gecikme nedeniyle hata yaşarsanız birkaç dakika sonra yeniden deneyin.

SQL tabanlı paylaşım ek izinler gerektirir. Ayrıntılı önkoşul listesi için bkz. [SQL kaynaklarından paylaşma](how-to-share-from-sql.md) .

## <a name="snapshot-failed"></a>Anlık görüntü başarısız oldu
Çeşitli nedenlerden dolayı anlık görüntü başarısız olabilir. Anlık görüntünün başlangıç saatine ve sonra her bir veri kümesinin durumuna tıklayarak ayrıntılı hata iletisi bulabilirsiniz. Anlık görüntünün başarısız olmasının yaygın nedenlerinden bazıları aşağıda verilmiştir:

* Veri paylaşımının, kaynak veri deposundan okuma veya hedef veri deposuna yazma izni yok. Ayrıntılı izin gereksinimleri için [Roller ve gereksinimlere](concepts-roles-permissions.md) bakın. İlk kez bir anlık görüntü çekiyorsunuz, veri paylaşımının kaynağı için Azure veri deposuna erişim verilmesi birkaç dakika sürebilir. Birkaç dakika bekleyip yeniden deneyin.
* Kaynak veya hedef veri deposuna yönelik veri paylaşımının bağlantısı güvenlik duvarı tarafından engelleniyor.
* Paylaşılan veri kümesi veya kaynak ya da hedef veri deposu silinir.

SQL kaynakları için, anlık görüntü hatalarının ek nedenleri aşağıda verilmiştir. 

* Veri paylaşma izni verilecek kaynak veya hedef SQL betiği çalışmıyor veya Azure Active Directory kimlik doğrulaması yerine SQL kimlik doğrulaması kullanılarak çalıştırıldı.  
* Kaynak veya hedef SQL veri deposu duraklatıldı.
* SQL veri türleri anlık görüntü işlemi veya hedef veri deposu tarafından desteklenmiyor. Ayrıntılar için [SQL kaynaklarından paylaşma](how-to-share-from-sql.md#supported-data-types) bölümüne bakın.
* Kaynak veya hedef SQL veri deposu diğer süreçler tarafından kilitlidir. Azure veri paylaşımında, kaynak ve hedef SQL veri deposuna kilit uygulanmaz. Ancak, kaynak ve hedef SQL veri deposundaki mevcut kilitler anlık görüntü hatasına neden olur.
* Hedef SQL tablosuna bir yabancı anahtar kısıtlaması tarafından başvuruluyor. Anlık görüntü sırasında, aynı ada sahip bir hedef tablo varsa, Azure veri paylaşımında tablo bırakılır ve yeni bir tablo oluşturulur. Hedef SQL tablosuna bir yabancı anahtar kısıtlaması başvuruluyorsa tablo bırakılamaz.
* Hedef CSV dosyası oluşturuldu, ancak veriler Excel 'de okunamıyor. Bu durum, kaynak SQL tablosu Ingilizce olmayan karakterler içeren veriler içerdiğinde meydana gelebilir. Excel 'de ' veri al ' sekmesini seçin ve CSV dosyasını seçin, dosya kaynağı 65001: Unicode (UTF-8) ve yükleme verileri ' ni seçin.

## <a name="next-steps"></a>Sonraki adımlar

Verileri paylaşmaya nasıl başlayacağınızı öğrenmek için [verilerinizi paylaşma](share-your-data.md) öğreticiye geçin. 

Veri alma hakkında bilgi edinmek için, [verileri kabul etme ve alma](subscribe-to-data-share.md) öğreticisine geçin.