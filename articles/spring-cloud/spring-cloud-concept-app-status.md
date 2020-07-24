---
title: Azure Spring Cloud’da uygulama durumunu anlama
description: Azure Spring Cloud 'da uygulama durumu kategorilerini öğrenin
author: MikeDodaro
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: e3ef202a1a98b8193b55bcc4c2cb616d4a2000d8
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87037772"
---
# <a name="understanding-app-status-in-azure-spring-cloud"></a>Azure Spring Cloud’da uygulama durumunu anlama

Azure yay bulutu Kullanıcı arabirimi, çalışan uygulamaların durumu hakkında bilgi sunar.  Abonelik içindeki her kaynak grubu için uygulama türlerinin genel durumunu görüntüleyen bir **uygulamalar** seçeneği vardır.  Her uygulama türü için **uygulama örnekleri**görüntülenir.

## <a name="apps-status"></a>Uygulama durumu
Bir uygulama türünün genel durumunu görüntülemek için bir kaynak grubunun sol gezinti bölmesindeki **uygulamalar** ' ı seçin. Sonuç, dağıtılan uygulamanın durumunu görüntüler:

* **Sağlama durumu** , dağıtımın sağlama durumunu gösterir
* **Çalışan örnek** , kaç tane uygulama örneğinin çalıştığını/kaç tane uygulama örneği istendiğini gösterir. Uygulamanın durdurulması gerekiyorsa, bu sütun *durdurulmuş*olarak gösterilir.
* **Kayıtlı örnek** , Eureka 'ya kaç tane uygulama örneğinin kaydedildiğini ve kaç tane uygulama örneği istendiğini gösterir. Uygulamanın durdurulması gerekiyorsa, bu sütun *durdurulmuş*olarak gösterilir.


 ![Uygulama durumu](media/spring-cloud-concept-app-status/apps-ui-status.png)

**Dağıtım durumu aşağıdaki değerlerden biri olarak bildirilir:**

| Sabit listesi | Tanım |
|:--:|:----------------:|
| Çalışıyor | Dağıtım çalışıyor olmalıdır. |
| Durduruldu | Dağıtım durdurulmalıdır. |

**Sağlama durumuna yalnızca CLı 'den erişilebilir.  Aşağıdaki değerlerden biri olarak bildirilir:**

| Sabit listesi | Tanım |
|:--:|:----------------:|
| Oluşturma | Kaynak oluşturuluyor. |
| Bilen | Kaynak güncelleştiriliyor. |
| Başarılı | Kaynaklar başarıyla sağlandı ve ikili dosya dağıtılır. |
| Başarısız | *Başarılı* hedef elde edilemedi. |
| Siliniyor | Kaynak siliniyor. Bu işlem işlemi engeller ve kaynak bu durumda kullanılamaz. |

## <a name="app-instances-status"></a>Uygulama örnekleri durumu

Dağıtılan bir uygulamanın belirli bir örneğinin durumunu görüntülemek için **uygulamalar** Kullanıcı arabirimindeki uygulamanın **adına** tıklayın. Sonuçlar şu şekilde görüntülenir:
* **Durum**: Örneğin çalışıp çalışmadığını veya durumunu belirtir
* **Discoverystatus**: Eureka sunucusunda uygulama örneğinin kayıtlı durumu

 ![Uygulama örnekleri durumu](media/spring-cloud-concept-app-status/apps-ui-instance-status.png)

**Örnek durumu aşağıdaki değerlerden biri olarak bildirilir:**

| Sabit listesi | Tanım |
|:--:|:----------------:|
| Başlatılıyor | İkili dosya, verilen örneğe başarıyla dağıtıldı. Jar düzgün şekilde çalışmadığından, JAR dosyasını önyüklenmekte örnek başarısız olabilir. |
| Çalışıyor | Örnek işe yarar. |
| Başarısız | Uygulama örneği birkaç yeniden denemeden sonra kullanıcının ikilisini başlatamadı. |
| Eriyor | Uygulama örneği kapatılıyor. |

**Örneğin bulma durumu aşağıdaki değerlerden biri olarak bildirilir:**

| Sabit listesi | Tanım |
|:--:|:----------------:|
| AYARLAMA | Uygulama örneği Eureka 'ya kaydedilir ve trafik almaya hazırdır |
| OUT_OF_SERVICE | Uygulama örneği Eureka 'ya kaydedilir ve trafik alabilir. Ancak trafiği bilinçli olarak kapatır. |
| KAPATıLıP | Uygulama örneği Eureka 'ya kayıtlı değil veya kayıtlı ancak trafik alamıyor. |


## <a name="see-also"></a>Ayrıca bkz.
* [Azure yay bulutu 'nda bir Java Spring uygulamasını dağıtıma hazırlama](spring-cloud-tutorial-prepare-app-deployment.md)