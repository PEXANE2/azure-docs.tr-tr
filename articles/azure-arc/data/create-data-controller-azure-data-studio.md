---
title: Azure Data Studio veri denetleyicisi oluşturma
description: Azure Data Studio veri denetleyicisi oluşturma
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: f6fd8169c587e928da9946d74335ddc758889144
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91273152"
---
# <a name="create-data-controller-in-azure-data-studio"></a>Azure Data Studio veri denetleyicisi oluşturma

Dağıtım Sihirbazı ve Not defterleri aracılığıyla Azure Data Studio kullanarak bir veri denetleyicisi oluşturabilirsiniz.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="prerequisites"></a>Önkoşullar

- Bir Kubernetes kümesine erişmeniz ve kubeconfig dosyanızı, dağıtmak istediğiniz Kubernetes kümesine işaret etmek üzere yapılandırmış olmanız gerekir.
- **Azure Arc** ve **Azure Data CLI**adlı Azure Data Studio uzantıları **Azure Data Studio** dahil [istemci araçlarını yüklemeniz](install-client-tools.md) gerekir.
- Azure Data Studio 'de Azure 'da oturum açmanız gerekir.  Bunu yapmak için: CTRL/Command + SHIFT + P yazarak komut metni penceresini açın ve **Azure**yazın.  **Azure: oturum aç '** ı seçin.   Panelde, bir Azure hesabı eklemek için sağ üst köşedeki + simgesine tıklayın.

## <a name="use-the-deployment-wizard-to-create-azure-arc-data-controller"></a>Azure Arc veri denetleyicisi oluşturmak için Dağıtım Sihirbazı 'nı kullanma

Dağıtım sihirbazını kullanarak bir Azure Arc veri denetleyicisi oluşturmak için bu adımları izleyin.

1. Azure Data Studio, sol gezinti bölmesinde Bağlantılar sekmesine tıklayın.
2. Bağlantılar panelinin üstündeki **...** düğmesine tıklayın ve **yeni dağıtım...** seçeneğini belirleyin.
3. Yeni dağıtım sihirbazında, **Azure Arc veri denetleyicisi**' ni seçin, lisans kabulü onay kutusunu işaretleyin ve ardından alttaki **Seç** düğmesine tıklayın.
4. Varsayılan kubeconfig dosyasını kullanın veya başka bir tane seçin.  **İleri**’ye tıklayın.
5. Bir Kubernetes küme bağlamı seçin. **İleri**’ye tıklayın.
6. Hedef Kubernetes kümenize bağlı olarak bir dağıtım yapılandırma profili dosyası seçin. **Ileri ' ye tıklayın**.
8. İstediğiniz aboneliği ve kaynak grubunu seçin.
9. Veri denetleyicisi ve veri denetleyicisinin oluşturulacağı ad alanı için bir ad girin.  

> [!NOTE]
> Ad alanı zaten varsa, ad alanı zaten başka Kubernetes nesneleri içermiyorsa, vb. kullanılır.  Ad alanı yoksa, ad alanını oluşturma girişimi yapılır.  Bir Kubernetes kümesinde ad alanı oluşturmak için Kubernetes Küme Yöneticisi ayrıcalıkları gerekir.  Kubernetes Küme Yöneticisi ayrıcalıklarınız yoksa, bu Sihirbazı tamamlamadan önce bir Kubernetes Yöneticisi tarafından gerçekleştirilmesi gereken Kubernetes [-Native Araçları ' nı kullanarak bir veri denetleyicisi oluşturma makalesini kullanarak](./create-data-controller-using-kubernetes-native-tools.md) , Kubernetes küme yöneticinizden ilk birkaç adımı gerçekleştirmesini isteyin.

> [!NOTE]
> Note: veri denetleyicisi ve ad alanı adı, Kubernetes [adlandırma kurallarına](https://kubernetes.io/docs/concepts/overview/working-with-objects/names/#names)uyması gereken Kubernetes kümesinde özel bir kaynak oluşturmak için kullanılacaktır.

10. Bir Azure konumu seçin.
   
> [!NOTE]
> Burada seçilen Azure konumu, veri denetleyicisi ve yönettiği veritabanı örnekleri hakkındaki *meta verilerin* depolanacağı Azure konumudur.  Veri denetleyicisi ve veritabanı örnekleri, Kubernetes kümenizde olduğu her yerde fiili olarak sunulacaktır.

11.  Bir Kullanıcı adı ve parola girin ve veri denetleyicisi yönetici kullanıcı hesabı için parolayı onaylayın.

> [!NOTE]
> Parola en az 8 karakter uzunluğunda olmalıdır.

1.  **İleri**’ye tıklayın.
2.  Betiği gözden geçirin ve **Not defterine**tıklayın.
3.  **Oluşturulan Not defterini gözden geçirin**.  Depolama sınıfı adları veya hizmet türleri gibi gerekli tüm değişiklikleri yapın.
4.  Not defterinin en üstünde **Tümünü Çalıştır** ' a tıklayın.

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
