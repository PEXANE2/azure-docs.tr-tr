---
title: İş sürekliliği planı - QnA Maker
description: İş sürekliliği planının birincil amacı, Bot veya onu tüketen uygulama için hiçbir kesinti süresi sağlayacak esnek bir bilgi tabanı bitiş noktası oluşturmaktır.
ms.topic: conceptual
ms.date: 04/07/2020
ms.openlocfilehash: 5d1501ecc42fe948959075cec7d728f6c9df908a
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80887086"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>QnA Maker hizmetiniz için bir iş sürekliliği planı oluşturun

İş sürekliliği planının birincil amacı, Bot veya onu tüketen uygulama için hiçbir kesinti süresi sağlayacak esnek bir bilgi tabanı bitiş noktası oluşturmaktır.

## <a name="business-continuity-with-traffic-manager"></a>Trafik yöneticisi ile iş sürekliliği

> [!div class="mx-imgBorder"]
> ![QnA Maker bcp planı](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

Yukarıda temsil edildiği gibi üst düzey fikir aşağıdaki gibidir:

1. [Azure eşleştirilmiş bölgelerde](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)iki paralel [QnA Maker hizmeti](set-up-qnamaker-service-azure.md) ayarlayın.

2. Birincil QnA Maker App hizmetinizi [yedekleyip](../../../app-service/manage-backup.md) ikincil kurulumda [geri yükleyin.](../../../app-service/web-sites-restore.md) Bu, her iki kurulumun da aynı ana bilgisayar adı ve anahtarlarıyla çalışmasını sağlar.

3. Birincil ve ikincil Azure arama dizinlerini eşit tutun. Azure dizinlerini yedeklemek için github örneğini [buradan](https://github.com/pchoudhari/QnAMakerBackupRestore) kullanın.

4. [Sürekli dışa aktarmayı](../../../application-insights/app-insights-export-telemetry.md)kullanarak Uygulama Öngörülerini yedekle.

5. Birincil ve ikincil yığınlar ayarlandıktan sonra, iki uç noktayı yapılandırmak ve yönlendirme yöntemi ayarlamak için [trafik yöneticisini](../../../traffic-manager/traffic-manager-overview.md) kullanın.

6. Trafik yöneticisi bitiş noktanız için daha önce Güvenli Soketkatmanı (SSL) olarak bilinen bir Aktarım Katmanı Güvenliği (TLS) oluşturmanız gerekir. [TLS/SSL sertifikasını](../../../app-service/configure-ssl-bindings.md) Uygulama hizmetlerinizde bağla.

7. Son olarak, Bot veya Uygulamanızdaki trafik yöneticisi bitiş noktasını kullanın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kapasite seçme](./improve-knowledge-base.md)