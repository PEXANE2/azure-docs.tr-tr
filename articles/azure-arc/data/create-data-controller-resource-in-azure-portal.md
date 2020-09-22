---
title: Azure portal Azure Arc veri denetleyicisi oluşturma
description: Azure portal Azure Arc veri denetleyicisi oluşturma
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 297efa83fb1563e3a360f652a6ac1bc2b1679998
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941875"
---
# <a name="create-an-azure-arc-data-controller-in-the-azure-portal"></a>Azure portal Azure Arc veri denetleyicisi oluşturma

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="introduction"></a>Giriş

Azure Arc veri denetleyicisi oluşturmak için Azure portal kullanabilirsiniz.

Oluşturulacak veya yönetilecek kaynak Azure altyapısının dışında olmasına rağmen, Azure Arc için oluşturma deneyimlerinin birçoğu Azure portal başlar. Bu durumlarda Kullanıcı deneyimi deseninin, özellikle de Azure ile ortamınız arasında doğrudan bağlantı olmaması durumunda, daha sonra Azure 'a güvenli bir bağlantı kurmak için ortamınızda indirilebilen ve yürütülebilecek bir betik oluşturmak için Azure portal kullanmaktır. Örneğin, Azure Arc etkin sunucular, [yay özellikli sunucular oluşturmak](../servers/onboard-portal.md)için bu kalıbı izler.

Önizleme, şimdilik yalnızca Azure Arc etkin veri hizmetlerinin dolaylı bağlantı modunu desteklediğine göre, daha sonra indirilebilen ve Kubernetes kümenize karşı Azure Data Studio çalıştırabileceğiniz bir not defteri oluşturmak için Azure portal kullanabilirsiniz. Gelecekte, doğrudan bağlı modu kullanılabilir olduğunda, veri denetleyicisini doğrudan Azure portal sağlayabileceksiniz. [Bağlantı modları](connectivity.md)hakkında daha fazla bilgi edinebilirsiniz.

## <a name="use-the-azure-portal-to-create-an-azure-arc-data-controller"></a>Azure Arc veri denetleyicisi oluşturmak için Azure portal kullanma

Azure portal ve Azure Data Studio kullanarak Azure Arc veri denetleyicisi oluşturmak için aşağıdaki adımları izleyin.

1. İlk olarak [Azure Portal Market](https://ms.portal.azure.com/#blade/Microsoft_Azure_Marketplace/MarketplaceOffersBlade/selectedMenuItemId/home/searchQuery/azure%20arc%20data%20controller)' te oturum açın.  Market arama sonuçları ' Azure Arc veri denetleyicisi ' ni gösterecek şekilde filtrelenecektir.
2. İlk adım arama ölçütlerine girmediyseniz. Lütfen arama sonuçlarına girin, ' Azure Arc veri denetleyicisi ' seçeneğine tıklayın.
3. Market 'ten Azure veri denetleyicisi kutucuğunu seçin.
4. **Oluştur** düğmesine tıklayın.
5. Azure Arc veri denetleyicisi oluşturma ve Azure Data Studio ve kubectl gibi tüm önkoşul yazılımlarını yüklemeye yönelik gereksinimleri gözden geçirin.
6. **Veri denetleyicisi ayrıntıları** düğmesine tıklayın.
7. Azure portal oluşturduğunuz diğer tüm kaynaklar için yaptığınız gibi bir abonelik, kaynak grubu ve Azure konumu seçin. Bu durumda, seçtiğiniz Azure konumu kaynakla ilgili meta verilerin depolanacağı yeri olacaktır.  Kaynak, seçtiğiniz herhangi bir altyapıda oluşturulur. Azure altyapısında olması gerekmez.
8. Veri denetleyiciniz için bir ad girin.
9. Şu anda önizlemede yalnızca dolaylı olarak bağlı mod desteklenir.
10. Bir dağıtım yapılandırma profili seçin.
11. **Azure Studio 'Da aç** düğmesine tıklayın.
12. Bir sonraki ekranda, seçimlerinizin ve oluşturulan bir not defterinizin özetini görürsünüz.  Not defterini indirmek için **sağlama Not defterini indir** düğmesine tıklayabilirsiniz.
13. Azure Data Studio bölümünde Not defterini açın ve üstteki **Tümünü Çalıştır** düğmesine tıklayın.
14. Veri denetleyicisi oluşturma işleminin tamamlanabilmesi için Not defteri 'ndeki komut istemlerini ve yönergeleri izleyin.

## <a name="monitoring-the-creation-status"></a>Oluşturma durumunu izleme

Denetleyicinin oluşturulması tamamlanması birkaç dakika sürer. İlerleme durumunu başka bir Terminal penceresinde aşağıdaki komutlarla izleyebilirsiniz:

> [!NOTE]
>  Aşağıdaki örnek komutlar, ' Arc ' adlı bir veri denetleyicisi ve Kubernetes ad alanı oluşturduğunuzu varsayar.  Farklı bir ad alanı/veri denetleyicisi adı kullandıysanız, ' yay ' değerini adınızla değiştirebilirsiniz.

```console
kubectl get datacontroller/arc --namespace arc
```

```console
kubectl get pods --namespace arc
```

Ayrıca, aşağıdaki gibi bir komut çalıştırarak belirli bir pod 'un oluşturma durumunu da denetleyebilirsiniz.  Bu, özellikle herhangi bir sorunu gidermek için kullanışlıdır.

```console
kubectl describe po/<pod name> --namespace arc

#Example:
#kubectl describe po/control-2g7bl --namespace arc
```

## <a name="troubleshooting-creation-problems"></a>Oluşturma sorunlarını giderme

Oluşturma ile herhangi bir sorun yaşarsanız, lütfen [sorun giderme kılavuzuna](troubleshoot-guide.md)bakın.
