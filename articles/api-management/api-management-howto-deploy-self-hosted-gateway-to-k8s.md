---
title: Kubernetes'e kendi barındırılan Azure API Yönetimi ağ geçidini dağıtma | Microsoft Dokümanlar
description: Kubernetes'e kendi barındırılan Azure API Yönetimi ağ geçidini nasıl dağıtılayacağımı öğrenin
services: api-management
documentationcenter: ''
author: miaojiang
manager: gwallace
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/31/2019
ms.author: apimpm
ms.openlocfilehash: 109316acb73d3c5f00186298c1f8840c516e5790
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73513841"
---
# <a name="deploy-a-self-hosted-azure-api-management-gateway-to-kubernetes"></a>Kubernetes'e kendi barındırılan azure API Yönetimi ağ geçidini dağıtma

Bu makalede, kendi barındırılan Azure API Yönetimi ağ geçidini bir Kubernetes Kümesine dağıtma adımları sağlanmaktadır.

> [!NOTE]
> Kendi kendine barındırılan ağ geçidi özelliği önizlemededir. Önizleme sırasında, kendi kendine barındırılan ağ geçidi ek ücret ödemeden yalnızca Geliştirici ve Premium katmanlarında kullanılabilir. Geliştirici katmanı, kendi kendine barındırılan tek bir ağ geçidi dağıtımıyla sınırlıdır.


## <a name="prerequisites"></a>Ön koşullar

- Aşağıdaki hızlı başlatmayı tamamlayın: [Azure API Yönetimi örneği oluşturun](get-started-create-service-instance.md)
- Bir Kubernetes kümesi oluşturun. [Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/) geliştirme ve değerlendirme amaçlı iyi bir seçenektir. Üretim iş yükleri için [Azure Kubernetes Hizmeti'ni](https://azure.microsoft.com/services/kubernetes-service/) veya yabancı bir bulutta Bir Kubernetes kümesini kullanabilirsiniz.
- [API Yönetimi örneğinde bir ağ geçidi kaynağı sağlama.](api-management-howto-provision-self-hosted-gateway.md)

## <a name="deploy-the-gateway-to-kubernetes"></a>Ağ geçidini Kubernetes'e dağıtın

1. **Ayarlar'ın**altından **Ağ Geçitleri'ni** seçin.
2. Dağıtmayı planlamayı planladiğiniz ağ geçidi kaynağını seçin.
3. **Dağıtım'ı**seçin.
4. **Belirteç** metin kutusundaki yeni bir belirteç varsayılan **Son Kullanma ve** Gizli **Anahtar** değerlerini kullanarak sizin için otomatik olarak oluşturulduğunu unutmayın. İstenirseniz ya da her ikisini de ayarlayın ve yeni bir belirteç oluşturmak için **Oluştur'u** seçin.
5. **Kubernetes'in Dağıtım komut dosyaları**altında **seçildiğinden** emin olun.
6. Dosyayı indirmek için **Dağıtım'ın** yanındaki **<ağ geçidi adı>.yml** dosya bağlantısını seçin.
7. Gerektiğinde yml dosyasındaki bağlantı noktası eşlemelerini ve konteyner adını ayarlayın.
8. Komutu panoya kaydetmek için **Dağıt** metin kutusunun sağ `kubectl` ucunda bulunan **kopya** simgesini seçin. 
9. Komutu terminal (veya komut) penceresine yapıştırın. Komutun indirilen ortam dosyasının geçerli dizinde bulunmasını beklediğini unutmayın.
```console
    kubectl apply -f <gateway-name>.yaml
```
10. Komutu yürütün. Komut, Kubernetes kümenize, Microsoft Konteyner Kayıt Defteri'nden indirilen kendi barındırılan ağ geçidinin görüntüsünü kullanarak kapsayıcıyı çalıştırmasını ve kapsayıcıyı HTTP (8080) ve HTTPS (443) bağlantı noktalarını ortaya çıkaracak şekilde yapılandırmasını emreder.
11. Ağ geçidi bölmesinin çalıştığını denetlemek için aşağıdaki komutu çalıştırın. Pod adınızın farklı olacağını unutmayın. 
```console
kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
local-gateway-55f774f844-bv9wt    1/1     Running   0          1m
```
12. Ağ geçidi hizmetinin çalıştığını denetlemek için aşağıdaki komutu çalıştırın. Hizmet adınızın farklı olacağını unutmayın. 
```console
kubectl get services
NAME             TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
localgateway     NodePort    10.110.230.87   <none>        80:32504/TCP,443:30043/TCP   1m
```
13. Azure portalına geri dön ve az önce dağıttığınız ağ geçidi düğümünün sağlıklı durum bildirdiğini doğrulayın.

![ağ geçidi durumu](media/api-management-howto-deploy-self-hosted-gateway-to-k8s/status.png)

> [!TIP]
> Kendi <code>kubectl logs <gateway-pod-name></code> kendine barındırılan ağ geçidi günlüğünün anlık görüntüsünü görüntülemek için komutu kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* Kendi kendine barındırılan ağ geçidi hakkında daha fazla bilgi edinmek için [Azure API Yönetimi'ne kendi kendine barındırılan ağ geçidine genel bakış](self-hosted-gateway-overview.md)
* [Azure Kubernetes Hizmeti](https://docs.microsoft.com/azure/aks/intro-kubernetes) hakkında daha fazla bilgi edinin


