---
title: Azure depolama hesabı SAS anahtarıyla maliyet verilerini dışa aktarma
description: Bu makale, iş ortaklarının SAS anahtarı oluşturmasına ve maliyet yönetimi dışarı aktarmaları yapılandırmasına yardımcı olur.
author: bandersmsft
ms.author: banders
ms.date: 03/08/2021
ms.topic: how-to
ms.service: cost-management-billing
ms.subservice: cost-management
ms.reviewer: adwise
ms.openlocfilehash: 16302a6bcc3bf41432500d2fdaa4ec27c28dd5b3
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102509687"
---
# <a name="export-cost-data-with-an-azure-storage-account-sas-key"></a>Azure depolama hesabı SAS anahtarıyla maliyet verilerini dışa aktarma

Aşağıdaki bilgiler yalnızca Microsoft iş ortakları için geçerlidir.

İş ortaklarının genellikle kendi Microsoft Iş ortağı anlaşmasıyla ilişkili kiracısında kendi Azure abonelikleri yoktur. Faturalandırma hesaplarının genel yöneticileri olan bir Microsoft Iş ortağı sözleşmesi planına sahip iş ortakları, bir paylaşılan erişim hizmeti (SAS) anahtarı kullanarak farklı bir Kiracıdaki bir depolama hesabına maliyet verileri verebilir ve kopyalayabilir. Diğer bir deyişle SAS anahtarı olan bir depolama hesabı, iş ortağının, dışa aktarılmış bilgileri almak için iş ortağı sözleşmesinin dışında bir depolama hesabı kullanmasına izin verir. Bu makale, iş ortaklarının SAS anahtarı oluşturmasına ve maliyet yönetimi dışarı aktarmaları yapılandırmasına yardımcı olur.

## <a name="requirements"></a>Gereksinimler

- Microsoft Iş ortağı sözleşmesi ile bir iş ortağı olmanız ve müşterilerin Azure planına sahip olmanız gerekir.
- İş ortağı kuruluşunuzun faturalandırma hesabı için genel yönetici olmanız gerekir.
- İş ortağı kuruluşunuzun farklı kiracısında bulunan bir depolama hesabını yapılandırmak için erişiminizin olması gerekir. Verileri depolama hesabınıza dışa aktardığınızda izinlerin ve veri erişiminin korunmasından siz sorumlusunuz.

## <a name="configure-azure-storage-with-a-sas-key"></a>Bir SAS anahtarıyla Azure Storage 'ı yapılandırma

Bir depolama hesabı SAS belirteci alın veya Azure portal kullanarak bir tane oluşturun. Azure portal üzerinde oluşturmak için aşağıdaki adımları kullanın. SAS anahtarları hakkında daha fazla bilgi edinmek için bkz [. paylaşılan erişim imzaları (SAS) ile verilere sınırlı erişim verme.](../../storage/common/storage-sas-overview.md)

1. Azure portal depolama hesabına gidin.
    - Hesabınızın birden fazla kiracıya erişimi varsa, depolama hesabına erişmek için Dizin değiştirin. Azure portal sağ üst köşesinde hesabınızı seçin ve ardından **dizinleri Değiştir**' i seçin.
    - Depolama hesabına erişmek için karşılık gelen kiracı hesabıyla Azure portal oturum açmanız gerekebilir.
1. Sol taraftaki menüden **paylaşılan erişim imzası**' nı seçin.  
    :::image type="content" source="./media/export-cost-data-storage-account-sas-key/storage-shared-access-signature.png" alt-text="Yapılandırılmış bir Azure depolama paylaşılan erişim imzasını gösteren ekran görüntüsü." lightbox="./media/export-cost-data-storage-account-sas-key/storage-shared-access-signature.png" :::
1. Belirteci, önceki görüntüde belirtilen ayarlarla yapılandırın.
    1. _Izin verilen hizmetler_ için **BLOB** ' u seçin.
    1. _Izin verilen kaynak türleri_ için **hizmet**, **kapsayıcı** ve **nesne** ' yi seçin.
    1. _Izin verilen izinler_ için **okuma**, **yazma**, **silme**, **Listeleme**, **ekleme** ve **oluşturma** seçeneğini belirleyin.
    1. Süre sonu ve tarihleri seçin. Dışa aktarma SAS belirtecinizi süresi dolmadan önce güncelleştirdiğinizden emin olun. Süre sonundan önce yapılandırdığınız zaman dilimi arttıkça, dışarı aktarma işlemi, yeni bir SAS belirteci gerekmeden önce çalışır.
1. Yalnızca _Izin verilen protokoller_ için **https** 'yi seçin.
1. _Tercih edilen yönlendirme katmanı_ için **temel** ' yı seçin.
1. _İmzalama anahtarı_ için **KEY1** öğesini seçin. SAS belirtecini imzalamak için kullanılan anahtarı döndürdüğünüzde veya güncelleştirirseniz, dışarı aktarma işlemi için yeni bir SAS belirteci oluşturmanız gerekir.
1. **SAS'yi ve bağlantı dizesini oluştur**’u seçin.
    Gösterilen **SAS belirteci** değeri, dışarı aktarmaları yapılandırırken ihtiyacınız olan belirteçtir.

## <a name="create-a-new-export-with-a-sas-token"></a>SAS belirteci ile yeni bir dışarı aktarma oluşturma

Faturalama hesabı kapsamındaki **dışarı aktarmalar** ' a gidin ve aşağıdaki adımları kullanarak yeni bir dışarı aktarma oluşturun.

1. **Oluştur**’u seçin.
1. Dışarı aktarma ayrıntılarını normal dışarı aktarma için yaptığınız gibi yapılandırın. Dışarı aktarmayı, var olan bir dizini veya kapsayıcıyı kullanacak şekilde yapılandırabilir veya yeni bir dizin ya da kapsayıcı belirtebilirsiniz ve dışarı aktarmalar bunları sizin için oluşturur.
1. Depolama yapılandırılırken, **SAS belirteci kullan**' ı seçin.  
    :::image type="content" source="./media/export-cost-data-storage-account-sas-key/new-export.png" alt-text="SAS belirtecini seçtiğiniz yeni dışarı aktarmayı gösteren ekran görüntüsü." lightbox="./media/export-cost-data-storage-account-sas-key/new-export.png" :::
1. Depolama hesabının adını girin ve SAS belirtecinize yapıştırın.
1. Mevcut bir kapsayıcı veya dizin belirtin ya da oluşturulacak yeni olanları belirleyin.
1. **Oluştur**’u seçin.

SAS belirteci tabanlı dışarı aktarma yalnızca belirteç geçerli olmaya devam ederken işe yarar. Geçerli bir süre dolmadan önce belirteci sıfırlayın veya dışarı aktarma işlemi çalışmayı durdurur. Belirteç depolama hesabınıza erişim sağladığından, diğer hassas bilgiler gibi, belirteci dikkatle koruyun. Verileri depolama hesabınıza dışa aktardığınızda izinleri ve veri erişimini sürdürmekten siz sorumlusunuz.

## <a name="troubleshoot-exports-using-sas-tokens"></a>SAS belirteçlerini kullanarak dışarı aktarmaları sorunlarını giderme

SAS belirteç tabanlı dışarı aktarmaları yapılandırdığınızda veya kullandığınızda oluşabilecek yaygın sorunlar aşağıda verilmiştir.

- Azure portal SAS anahtarı seçeneğini görmezsiniz.
  - Bir Microsoft Iş ortağı sözleşmesi olan ve faturalandırma hesabı için genel yönetici izninizin olduğunu doğrulayın. Bunlar bir SAS anahtarıyla dışarı aktarerişebilen insanlardır.

- Dışarı aktarmayı yapılandırmaya çalışırken aşağıdaki hata iletisini alırsınız:

    **Lütfen SAS belirtecinin blob hizmeti için geçerli olduğundan, kapsayıcı ve nesne kaynak türleri için geçerli olduğundan ve izinlere sahip olduğundan emin olun: ekleme okuma yazma oluşturma silme. (Depolama hizmeti hata kodu: Authorizationresourcettypelib uyuşmazlığı)**

    - Azure depolama 'da SAS anahtarını doğru bir şekilde yapılandırdığınızdan ve üretdiğinizden emin olun.

- Dışarı aktarma oluşturduktan sonra tam SAS anahtarını göremezsiniz.
  - Anahtarın beklenen davranış görmemektedir. SAS dışarı aktarma yapılandırıldıktan sonra, anahtar güvenlik nedenleriyle gizlidir.

- Depolama hesabına dışarı aktarmanın yapılandırıldığı kiracıdan erişemezsiniz.
  - Beklenen davranış. Depolama hesabı başka bir kiracıda varsa, depolama hesabını bulmak için Azure portal önce bu kiracıya gitmeniz gerekir.

- SAS belirteci ile ilgili bir hata nedeniyle dışa aktarma işlemi başarısız oluyor.
  - Dışarı aktarma işlemi yalnızca SAS belirteci geçerli olmaya devam ederken işe yarar. Yeni bir anahtar oluşturun ve dışarı aktarmayı çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

- Maliyet yönetimi verilerini dışa aktarma hakkında daha fazla bilgi için bkz. [veri oluşturma ve dışarı aktarma](tutorial-export-acm-data.md).
- Büyük miktarlarda kullanım verilerini dışa aktarma hakkında daha fazla bilgi için bkz. [dışarı aktarmalar ile büyük veri kümelerini alma](ingest-azure-usage-at-scale.md).
