---
title: Kubernetes panosu aracılığıyla Azure Stack Edge Pro cihazınızı izleme | Microsoft Docs
description: Azure Stack Edge Pro cihazınızı izlemek için Kubernetes panosunu nasıl erişebileceğiniz ve kullanabileceğinizi açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 09/22/2020
ms.author: alkohli
ms.openlocfilehash: 137cff47d49be1405f60bc47cd16f7f027ab63a9
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91320838"
---
# <a name="use-kubernetes-dashboard-to-monitor-your-azure-stack-edge-pro-gpu-device"></a>Azure Stack Edge Pro GPU cihazınızı izlemek için Kubernetes panosunu kullanma

Bu makalede, Azure Stack Edge Pro GPU cihazınızı izlemek için Kubernetes panosuna nasıl erişebileceğiniz ve kullanılacağı açıklanmaktadır. Cihazınızı izlemek için Azure portal grafikleri kullanabilir, Kubernetes panosunu görüntüleyebilir veya `kubectl` komutları cihazın PowerShell arabirimi aracılığıyla çalıştırabilirsiniz. 

Bu makale yalnızca Kubernetes panosunda gerçekleştirilebilecek izleme görevlerine odaklanır.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * Cihazınızdaki Kubernetes panosuna erişin
> * Cihazınızda dağıtılan modülleri görüntüleme
> * Cihazınızda dağıtılan uygulamalar için IP adresi al
> * Cihazınızda dağıtılan modüller için kapsayıcı günlüklerini görüntüleme


## <a name="about-kubernetes-dashboard"></a>Kubernetes panosu hakkında

Kubernetes panosu, Kapsayıcılı uygulamalarınızın sorunlarını gidermek için kullanabileceğiniz web tabanlı bir kullanıcı arabirimidir. Kubernetes panosu, Kubernetes komut satırı için Kullanıcı arabirimi tabanlı bir alternatiftir `kubectl` . Daha fazla bilgi için bkz. [Kubernetes panosu](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/). 

Azure Stack Edge Pro cihazınızda, Kubernetes panosunu *salt okuma* modunda kullanarak Azure Stack Edge Pro cihazınızda çalışan uygulamalara bir genel bakış alabilir, Kubernetes küme kaynaklarının durumunu görüntüleyebilir ve cihazda oluşan tüm hataları görebilirsiniz.

## <a name="access-dashboard"></a>Panoya erişin

Kubernetes panosu *salt okunurdur* ve 31000 numaralı bağlantı noktasında Kubernetes ana düğümünde çalışır. Panoya erişmek için şu adımları izleyin: 

1. Cihazınızın yerel kullanıcı arabiriminde **cihaz** ' a gidin ve **cihaz uç noktaları**' na gidin. 
1. Panoya erişmenizi sağlayan bir indirmek için **yapılandırmayı indir** ' i seçin `kubeconfig` . `config.json`Dosyayı yerel sisteminize kaydedin.
1. Panoyu bir tarayıcıda açmak için Kubernetes panosu URL 'sini seçin.

    ![Yerel Kullanıcı arabirimindeki cihaz sayfasında Kubernetes Pano URL 'SI](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-dashboard-url-local-ui-1.png)

1. **Kubernetes Pano oturum açma** sayfasında:
    
    1. **Kubeconfig**' i seçin. 
        ![Kubeconfig seçeneğini belirleyin](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-dashboard-sign-in-1.png) 
    1. Üç noktayı seçin **...**. `kubeconfig` Daha önce yerel sisteminizde indirdiğiniz öğesine gidin ve üzerine gelin. **Oturum aç**'ı seçin.
        ![Kubeconfig dosyasına gidin](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-dashboard-sign-in-2.png)    

6. Artık Azure Stack Edge Pro cihazınız için Kubernetes panosunu salt okunurdur modunda görüntüleyebilirsiniz.

    ![Kubernetes panosu ana sayfası](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-dashboard-main-page-1.png)

## <a name="view-module-status"></a>Modül durumunu görüntüle

İşlem modülleri, iş mantığı uygulanmış kapsayıcılardır. Azure Stack Edge Pro cihazınızda bir işlem modülünün başarıyla dağıtılıp dağıtılmadığını doğrulamak için panoyu kullanabilirsiniz.

Modül durumunu görüntülemek için panoda bu adımları izleyin:

1. Panonun sol bölmesinde **ad alanı**' na gidin. IoT Edge modüllerinin görüntülendiği ad alanına göre filtreleyin, bu durumda **ıotedge**.
1. Sol bölmede **Iş yükleri > dağıtımlar**' a gidin.
1. Sağ bölmede, cihazınızda dağıtılan tüm modülleri görürsünüz. Bu durumda, Azure Stack Edge Pro 'da bir GettingStartedWithGPU modülü dağıtılmıştı. Modülün dağıtıldığını görebilirsiniz.

    ![Modül dağıtımını görüntüle](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-view-module-deployment-1.png)

 
## <a name="get-ip-address-for-services-or-modules"></a>Hizmetler veya modüller için IP adresi al

Panoyu, Kubernetes kümesi dışında sunmak istediğiniz hizmetlerin veya modüllerin IP adreslerini almak için kullanabilirsiniz. 

Bu dış hizmetler için IP aralığını, **işlem ağ ayarları** sayfasında cihazın yerel Web Kullanıcı arabirimi aracılığıyla atarsınız. IoT Edge modüllerini dağıttıktan sonra, belirli bir modüle veya hizmete atanan IP adresini almak isteyebilirsiniz. 

IP adresini almak için panoda bu adımları izleyin:

1. Panonun sol bölmesinde **ad alanı**' na gidin. Bir dış Hizmetin dağıtıldığı ad alanına göre filtreleyin, bu durumda **ıotedge**.
1. Sol bölmede **bulma ve yük dengeleme > Hizmetleri**' ne gidin.
1. Sağ bölmede, `iotedge` Azure Stack Edge Pro cihazınızda ad alanında çalışan tüm hizmetleri görürsünüz.

    ![Dış hizmetler için IP al](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-get-ip-external-service-1.png)

## <a name="view-container-logs"></a>Kapsayıcı günlüklerini görüntüleme

Kapsayıcı günlüklerini görüntülemeniz gereken örnekler vardır. Bu panoyu, Kubernetes kümenize dağıttığınız belirli bir kapsayıcının günlüklerini almak için kullanabilirsiniz.

Kapsayıcı günlüklerini görüntülemek için panoda bu adımları izleyin:

1. Panonun sol bölmesinde **ad alanı**' na gidin. IoT Edge modüllerinin dağıtıldığı ad alanına göre filtreleyin, bu durumda **ıotedge**.
1. Sol bölmedeki **Iş yükleri > pods**' ye gidin.
1. Sağ bölmede, cihazınızda çalışan tüm FID 'leri görürsünüz. Günlüklerini görüntülemek istediğiniz modülü çalıştıran Pod 'u belirler. Tanımladığınız pod için dikey üç nokta ve bağlam menüsünden **Günlükler**' i seçin.

    ![Kapsayıcı günlüklerini görüntüle 1](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-view-container-logs-1.png)

1. Günlükler, panoda yerleşik olarak bulunan bir Günlükler görüntüleyicisinde görüntülenir. Günlükleri de indirebilirsiniz.

    ![Kapsayıcı günlüklerini görüntüle 2](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-view-container-logs-1.png)
    

## <a name="view-cpu-memory-usage"></a>CPU, bellek kullanımı görüntüleme

Azure Stack Edge Pro cihazının Kubernetes panosu, Kubernetes kaynakları genelinde CPU ve bellek kullanımını toplayan bir [ölçüm sunucusu eklentisi](https://kubernetes.io/docs/tasks/debug-application-cluster/resource-metrics-pipeline/) de içerir.
 
Örneğin, tüm ad alanlarındaki dağıtımlar genelinde tüketilen CPU ve belleği görüntüleyebilirsiniz. 

![Tüm dağıtımlar genelinde CPU ve bellek kullanımını görüntüleme](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/view-cpu-memory-all-1.png)

Belirli bir ad alanına göre de filtre uygulayabilirsiniz. Aşağıdaki örnekte, CPU ve bellek tüketimini yalnızca Azure Arc dağıtımları için görüntüleyebilirsiniz.  

![Azure Arc dağıtımları için CPU ve bellek kullanımını görüntüleme](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/view-cpu-memory-azure-arc-1.png)

Kubernetes ölçüm sunucusu, [yatay Pod otomatik gizleme](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)gibi otomatik ölçeklendirme işlem hatları sağlar.


## <a name="next-steps"></a>Sonraki adımlar

[Cihaz sorunlarını giderme](azure-stack-edge-gpu-troubleshoot.md)hakkında bilgi edinin.
