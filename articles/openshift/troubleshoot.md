---
title: Sorun Giderme Azure Red Hat OpenShift
description: Azure Red Hat OpenShift ile sık karşılaşılan sorunları giderme ve çözme
author: jimzim
ms.author: jzim
ms.service: container-service
ms.topic: troubleshooting
ms.date: 05/08/2019
ms.openlocfilehash: ee032cdf4a3f72b2cd2e7da0658effe75b6fb1fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76274923"
---
# <a name="troubleshooting-for-azure-red-hat-openshift"></a>Azure Red Hat OpenShift için Sorun Giderme

Bu makalede, Microsoft Azure Red Hat OpenShift kümeleri oluşturulurken veya yönetirken karşılaşılan bazı sık karşılaşılan sorunlar ayrıntıları.

## <a name="retrying-the-creation-of-a-failed-cluster"></a>Başarısız bir kümeoluşturmayı yeniden deneme

CLI komutunu `az` kullanarak bir Azure Red Hat OpenShift kümesi oluşturmak başarısız olursa, oluşturmayı yeniden denemek başarısız olmaya devam eder.
Başarısız `az openshift delete` kümeyi silmek ve ardından tamamen yeni bir küme oluşturmak için kullanın.

## <a name="hidden-azure-red-hat-openshift-cluster-resource-group"></a>Gizli Azure Red Hat OpenShift küme kaynak grubu

Şu anda, Azure CLI (komutu) `Microsoft.ContainerService/openShiftManagedClusters` `az openshift create` tarafından otomatik olarak oluşturulan kaynak Azure portalında gizlidir. Kaynak **grubu** görünümünde, kaynak grubunu görüntülemek için **gizli türleri göster'i** denetleyin.

![Portaldaki gizli tür onay kutusunun ekran görüntüsü](./media/aro-portal-hidden-type.png)

## <a name="creating-a-cluster-results-in-error-that-no-registered-resource-provider-found"></a>Küme oluşturma, kayıtlı kaynak sağlayıcısının bulamadığı hatalarla sonuçlanır

Bir küme `No registered resource provider found for location '<location>' and API version '2019-04-30' for type 'openShiftManagedClusters'. The supported api-versions are '2018-09-30-preview`oluşturmak, önizlemenin bir parçası olduğunuz ve genel olarak kullanılabilen ürünü kullanmak için Azure sanal makine ayrılmış örnekleri satın [almanız](https://aka.ms/openshift/buy) gereken bir hataya yol açıyorsa. Rezervasyon, tam olarak yönetilen Azure hizmetleri için ön ödeme yaparak harcamanızı azaltır. Rezervasyonlar ve size nasıl tasarruf ettikleri hakkında daha fazla bilgi edinmek için [*Azure Rezervasyonları*](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations) Nelerdir'e bakın.

## <a name="next-steps"></a>Sonraki adımlar

- OpenShift sorun giderme hakkında daha fazla şey için [Red Hat OpenShift Yardım Merkezi'ni](https://help.openshift.com/) deneyin.

- [Azure Red Hat OpenShift hakkında sık sorulan soruların](openshift-faq.md)yanıtlarını bulun.
