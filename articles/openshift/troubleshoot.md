---
title: Azure Red Hat OpenShift sorunlarını giderme
description: Azure Red Hat OpenShift ile ilgili sık karşılaşılan sorunları giderme ve çözme
author: jimzim
ms.author: jzim
ms.service: container-service
ms.topic: troubleshooting
ms.date: 05/08/2019
ms.openlocfilehash: ee032cdf4a3f72b2cd2e7da0658effe75b6fb1fa
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76274923"
---
# <a name="troubleshooting-for-azure-red-hat-openshift"></a>Azure Red Hat OpenShift sorunlarını giderme

Bu makalede Microsoft Azure Red Hat OpenShift kümelerini oluşturma veya yönetme sırasında karşılaşılan bazı yaygın sorunlar ayrıntılı olarak açıklanır.

## <a name="retrying-the-creation-of-a-failed-cluster"></a>Başarısız bir kümenin oluşturulması yeniden deneniyor

`az` CLI komutunu kullanarak bir Azure Red Hat OpenShift kümesi oluşturulamazsa, oluşturma işleminin yeniden denenmeye devam edilecek.
Başarısız `az openshift delete` kümeyi silmek için kullanın, ardından tamamen yeni bir küme oluşturun.

## <a name="hidden-azure-red-hat-openshift-cluster-resource-group"></a>Gizli Azure Red Hat OpenShift kümesi kaynak grubu

Şu anda, `Microsoft.ContainerService/openShiftManagedClusters` Azure CLI (`az openshift create` komut) tarafından otomatik olarak oluşturulan kaynak Azure Portal gizlenir. Kaynak grubunu görüntülemek için **kaynak grubu** görünümünde **gizli türleri göster** ' i işaretleyin.

![Portalda gizli tür onay kutusunun ekran görüntüsü](./media/aro-portal-hidden-type.png)

## <a name="creating-a-cluster-results-in-error-that-no-registered-resource-provider-found"></a>Bir küme oluşturmak, kayıtlı kaynak sağlayıcısı bulunmadığından hata ile sonuçlanır

Bir küme oluşturmak bir hata ile sonuçlanırsa `No registered resource provider found for location '<location>' and API version '2019-04-30' for type 'openShiftManagedClusters'. The supported api-versions are '2018-09-30-preview`, önizlemenin bir parçası olan ve artık genel olarak kullanılabilir ürünü kullanmak için [Azure sanal makinesine ayrılmış örnekler satın](https://aka.ms/openshift/buy) almanız gerekir. Bir rezervasyon, tam olarak yönetilen Azure hizmetleri için ön ödeme yaparak harcamalarınızı azaltır. Rezervasyonlar ve nasıl tasarruf ettikleri hakkında daha fazla bilgi edinmek için [*Azure ayırmaları*](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations) nelerdir bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

- OpenShift sorun giderme hakkında daha fazla bilgi için [Red Hat OpenShift yardım merkezini](https://help.openshift.com/) deneyin.

- [Azure Red Hat OpenShift hakkında sık sorulan soruların](openshift-faq.md)yanıtlarını bulun.
