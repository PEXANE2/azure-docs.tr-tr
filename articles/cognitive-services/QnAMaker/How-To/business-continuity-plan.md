---
title: İş sürekliliği planı-Soru-Cevap Oluşturma
description: İş sürekliliği planının birincil amacı dayanıklı bir bilgi tabanı uç noktası oluşturmaktır, bu, bot veya onu kullanan uygulama için zaman kaybı olmamasını sağlar.
ms.topic: conceptual
ms.date: 04/07/2020
ms.openlocfilehash: 5d1501ecc42fe948959075cec7d728f6c9df908a
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80887086"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>Soru-Cevap Oluşturma hizmetiniz için bir iş sürekliliği planı oluşturma

İş sürekliliği planının birincil amacı dayanıklı bir bilgi tabanı uç noktası oluşturmaktır, bu, bot veya onu kullanan uygulama için zaman kaybı olmamasını sağlar.

## <a name="business-continuity-with-traffic-manager"></a>Traffic Manager ile iş sürekliliği

> [!div class="mx-imgBorder"]
> ![Soru-Cevap Oluşturma bcp planı](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

Yukarıda gösterilen üst düzey fikir aşağıdaki gibidir:

1. [Azure eşlenmiş bölgelerde](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)iki paralel [soru-cevap oluşturma hizmeti](set-up-qnamaker-service-azure.md) ayarlayın.

2. Birincil Soru-Cevap Oluşturma App Service 'i [yedekleyin](../../../app-service/manage-backup.md) ve ikincil kuruluma [geri yükleyin](../../../app-service/web-sites-restore.md) . Bu, her iki kurulum 'un aynı ana bilgisayar adı ve anahtarlarla çalışmasını sağlayacaktır.

3. Birincil ve ikincil Azure arama dizinlerini eşitlenmiş halde tutun. Azure dizinlerini yedekleme ve geri yükleme işlemlerinin nasıl yapılacağını görmek için [burada](https://github.com/pchoudhari/QnAMakerBackupRestore) GitHub örneğini kullanın.

4. Application Insights [sürekli dışarı aktarma](../../../application-insights/app-insights-export-telemetry.md)kullanarak yedekleyin.

5. Birincil ve ikincil yığınlar kurulduktan sonra, iki uç noktayı yapılandırmak ve bir yönlendirme yöntemi ayarlamak için [Traffic Manager](../../../traffic-manager/traffic-manager-overview.md) 'ı kullanın.

6. Traffic Manager uç noktanız için önceden Güvenli Yuva Katmanı (SSL) olarak bilinen bir Aktarım Katmanı Güvenliği (TLS) oluşturmanız gerekir. Uygulama hizmetlerinize [TLS/SSL sertifikası bağlayın](../../../app-service/configure-ssl-bindings.md) .

7. Son olarak, bot veya uygulamanızdaki Traffic Manager uç noktasını kullanın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kapasite seçme](./improve-knowledge-base.md)