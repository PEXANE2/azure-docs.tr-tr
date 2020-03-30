---
title: Azure dosya paylaşımı yedeklemesi hakkında
description: Kurtarma Hizmetleri kasasında Azure dosya paylaşımlarını nasıl yedekleyabileceğinizi öğrenin
ms.topic: conceptual
ms.date: 03/05/2020
ms.openlocfilehash: 0e17b05a3febaa673fb29d45c2bcef25e2996df8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78386726"
---
# <a name="about-azure-file-share-backup"></a>Azure dosya paylaşımı yedeklemesi hakkında

Azure dosya paylaşımı yedeklemesi, buluttaki verilerinizi koruyan ve şirket içi yedekleme çözümleriyle ilgili ek bakım ek lerini ortadan kaldıran yerel, bulut tabanlı bir yedekleme çözümüdür. Azure Yedekleme hizmeti Azure dosya eşitlemeyiyle sorunsuz bir şekilde tümleşir ve dosya paylaşım verilerinizi ve yedeklemelerinizi merkezileştirmenize olanak tanır. Bu basit, güvenilir ve güvenli çözüm, herhangi bir felaket senaryosu durumunda verilerinizi kurtarabileceğinize dair güvence vermek le birlikte birkaç basit adımda kurumsal dosya paylaşımlarınız için koruma yapılandırmanızı sağlar.

## <a name="key-benefits-of-azure-file-share-backup"></a>Azure dosya paylaşımı yedeklemesinin temel avantajları

* Sıfır altyapı: Dosya paylaşımlarınız için korumayapılandırmak için dağıtım gerekmez.
* Yerleşik yönetim özellikleri: Yedeklemeleri zamanlayabilir ve veri budama ek yükü olmadan istenen bekletme süresini belirtebilirsiniz.
* Anında geri yükleme: Azure dosya paylaşımı yedeklemesi dosya paylaşımı anlık görüntülerini kullanır, böylece yalnızca anında geri yüklemek istediğiniz dosyaları seçebilirsiniz.
* Uyarı ve raporlama: Yedekleme ve geri yükleme hataları için uyarıları yapılandırabilir ve dosya paylaşımlarınızda yedeklemeler hakkında bilgi almak için Azure Yedekleme tarafından sağlanan raporlama çözümlerini kullanabilirsiniz.

## <a name="architecture"></a>Mimari

![Azure dosya paylaşımı yedekleme mimarisi](./media/azure-file-share-backup-overview/azure-file-shares-backup-architecture.png)

## <a name="how-the-backup-process-works"></a>Yedekleme işlemi nasıl çalışır?

1. Azure Dosyası paylaşımları için yedekleme yapılandırmanın ilk adımı, kurtarma hizmetleri kasası oluşturmaktır. Kasa, farklı iş yükleri arasında yapılandırılan yedeklemelerin birleştirilmiş bir görünümünü sağlar.

2. Bir kasa oluşturduğunuzda, Azure Yedekleme hizmeti kasaya kaydedilebilen depolama hesaplarını keşfeder. Korumak istediğiniz dosya paylaşımlarını barındıran depolama hesabını seçebilirsiniz.

3. Depolama hesabını seçtikten sonra, Azure Yedekleme hizmeti depolama hesabında bulunan dosya paylaşımları kümesini listeler ve adlarını yönetim katmanı kataloğunda saklar.

4. Daha sonra yedekleme ilkesini (zamanlama ve bekletme) gereksinimlerinize göre yapılandırın ve yedeklemek için dosya paylaşımlarını seçin. Azure Yedekleme hizmeti, zamanlanmış yedeklemeler yapmak için denetim düzlemindeki zamanlamaları kaydeder.

5. Belirtilen ilkeyi temel alan Azure Yedekleme zamanlayıcısı, zamanlanan zamanda yedeklemeleri tetikler. Bu işin bir parçası olarak, dosya paylaşımı anlık görüntüsü Dosya paylaşımı API'si kullanılarak oluşturulur. Meta veri deposunda yalnızca anlık görüntü URL'si depolanır.

    >[!NOTE]
    >Yedekleme hizmeti depolama hesabınızın bir parçası olan anlık görüntüler oluşturduğundan ve yönettiğinden, dosya paylaşımı verileri Yedekleme hizmetine aktarılmaz.

6. Azure dosya paylaşımı içeriğini (tek tek dosyalar veya tam paylaşım) kaynak dosya paylaşımında bulunan anlık görüntülerden geri yükleyebilirsiniz. İşlem tetiklendikten sonra, anlık görüntü URL'si meta veri deposundan alınır ve veriler listelenir ve kaynak anlık görüntüden seçtiğiniz hedef dosya paylaşımına aktarılır.

7. İş izleme verilerinin yedeklemesi ve geri yüklenmesi Azure Yedekleme İzleme hizmetine itilir. Bu, dosya paylaşımlarınızın bulut yedeklemelerini tek bir panoda izlemenize olanak tanır. Ayrıca, yedekleme durumu etkilendiğinde uyarıları veya e-posta bildirimlerini de yapılandırabilirsiniz. E-postalar Azure e-posta hizmeti aracılığıyla gönderilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure dosya paylaşımlarını](backup-afs.md) nasıl yedekleyin
* [Azure Dosyalarını yedeklemeyle ilgili soruların yanıtlarını](backup-azure-files-faq.md) bulma
