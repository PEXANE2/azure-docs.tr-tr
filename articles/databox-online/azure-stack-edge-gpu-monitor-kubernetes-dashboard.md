---
title: Kubernetes panosu aracılığıyla Azure Stack Edge cihazınızı izleme | Microsoft Docs
description: Azure Stack Edge cihazınızı izlemek için Kubernetes panosunu nasıl erişebileceğiniz ve kullanabileceğinizi açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/29/2020
ms.author: alkohli
ms.openlocfilehash: 7274cef73bff3fb87d55ad636ff0167c8a064796
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89180686"
---
# <a name="use-kubernetes-dashboard-to-monitor-your-azure-stack-edge-gpu-device"></a>Azure Stack Edge GPU cihazınızı izlemek için Kubernetes panosunu kullanma

Bu makalede, Azure Stack Edge GPU cihazınızı izlemek için Kubernetes panosuna nasıl erişebileceğiniz ve kullanılacağı açıklanır. Cihazınızı izlemek için Azure portal grafikleri kullanabilir, Kubernetes panosunu görüntüleyebilir veya `kubectl` komutları cihazın PowerShell arabirimi aracılığıyla çalıştırabilirsiniz. 

Bu makale yalnızca Kubernetes panosunda gerçekleştirilebilecek izleme görevlerine odaklanır.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * Cihazınızdaki Kubernetes panosuna erişin
> * `aseuser`Yapılandırmayı indir
> * Cihazınızda dağıtılan modülleri görüntüleme
> * Cihazınızda dağıtılan uygulamalar için IP adresi al
> * Cihazınızda dağıtılan modüller için kapsayıcı günlüklerini görüntüleme


## <a name="about-kubernetes-dashboard"></a>Kubernetes panosu hakkında

Kubernetes panosu, Kapsayıcılı uygulamalarınızın sorunlarını gidermek için kullanabileceğiniz web tabanlı bir kullanıcı arabirimidir. Kubernetes panosu, Kubernetes komut satırı için Kullanıcı arabirimi tabanlı bir alternatiftir `kubectl` . Daha fazla bilgi için bkz. [Kubernetes panosu](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/). 

Azure Stack Edge cihazınızda, Kubernetes panosunu *salt okunurdur* modunda kullanarak Azure Stack Edge cihazınızda çalışan uygulamalara bir genel bakış alabilir, Kubernetes küme kaynaklarının durumunu görüntüleyebilir ve cihazda oluşan tüm hataları görebilirsiniz.

## <a name="access-dashboard"></a>Panoya erişin

Kubernetes panosu *salt okunurdur* ve 31000 numaralı bağlantı noktasında Kubernetes ana düğümünde çalışır. Panoya erişmek için şu adımları izleyin: 

1. Cihazınızın yerel kullanıcı arabiriminde **cihaz** ' a gidin ve **cihaz uç noktaları**' na gidin. Panoyu bir tarayıcıda açmak için Kubernetes panosu URL 'sini seçin.

    ![Yerel Kullanıcı arabirimindeki cihaz sayfasında Kubernetes Pano URL 'SI](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-dashboard-url-local-ui-1.png)

1. **Kubernetes Pano oturum açma** sayfasında **belirteç**' ı seçin. 
1. Belirteç girin. 
    1. Belirteci almak için, [cihazınızın PowerShell arabirimi aracılığıyla bağlanın](azure-stack-edge-gpu-connect-powershell-interface.md).
    1. Şu komutu çalıştırın:  `Get-HcsKubernetesDashboardToken`
    
    1. Komut isteminde size sunulan belirteç dizesini kopyalayın. Örnek çıktı aşağıdaki gibidir:
        
        ```powershell
        [10.100.10.10]: PS>Get-HcsKubernetesDashboardToken
        eyJhbGciOiJSUzI1NiIsImtpZCI6IkpFTEtBYTMyZ0Ezb01OYTVFSnVaUV85OWtLdXNETTZQR0k0UlFybGdReFUifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJrdWJlcm5ldGVzLWRhc2hib2FyZC10b2tlbi03czZ6ayIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6IjU3NzY3ZDAzLTJlYWUtNDlkMi1hNDEyLTNkOTU3MDFiMThiMyIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlcm5ldGVzLWRhc2hib2FyZDprdWJlcm5ldGVzLWRhc2hib2FyZCJ9.UgNrpVYVJBEaWxFlljuENUQQmzFXMYG2VsJUIYFdp2AO20zX0k5dRvwcCpeGlqSKb9MyYjG0c6RmT9uCOZk-vAwt7btszQLD7KPCwh_nn_NiIyO8ApgGRYZP8NuP8CBTX3tl_hpwfHtZ0ksbuKAduIL-0uPF0rG5wgLk9cTEw6fKSc2UZW6bIzhNSp_uSiP6MexOS6OftF9JFZejkIGd33dSp-k-tgFlm2Zy96sdFJC0q-XsH7jygiVnfxA9XMs5wqW26LkCh0rfO2WI3C1XFK-4TpufRZLJHo5WPlu-Tnsxa8xmtk2jQ3us-sXcBRrvhPNPrNKkbqc9hbjmWfGD0Q
        [10.100.10.10]: PS>
        ```
        
1. **Oturum aç**'ı seçin.

    ![Kubernetes panosu 'nda oturum açma](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-dashboard-sign-in-1.png)

6. Artık Azure Stack Edge cihazınız için Kubernetes panosunu salt okunurdur modunda görüntüleyebilirsiniz.

    ![Kubernetes panosu ana sayfası](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-dashboard-main-page-1.png)

## <a name="view-module-status"></a>Modül durumunu görüntüle

İşlem modülleri, iş mantığı uygulanmış kapsayıcılardır. Azure Stack Edge cihazınızda bir işlem modülünün başarıyla dağıtılıp dağıtılmadığını doğrulamak için panoyu kullanabilirsiniz.

Modül durumunu görüntülemek IÇIN panoda bu adımları izleyin:

1. Panonun sol bölmesinde **ad alanı**' na gidin. IoT Edge modüllerinin görüntülendiği ad alanına göre filtreleyin, bu durumda **ıotedge**.
1. Sol bölmede **Iş yükleri > dağıtımlar**' a gidin.
1. Sağ bölmede, cihazınızda dağıtılan tüm modülleri görürsünüz. Bu durumda, Azure Stack Edge üzerinde bir GettingStartedWithGPU modülü dağıtılır. Modülün dağıtıldığını görebilirsiniz.

    ![Modül dağıtımını görüntüle](./media/azure-stack-edge-gpu-monitor-kubernetes-dashboard/kubernetes-view-module-deployment-1.png)

 
## <a name="get-ip-address-for-services-or-modules"></a>Hizmetler veya modüller için IP adresi al

Panoyu, Kubernetes kümesi dışında sunmak istediğiniz hizmetlerin veya modüllerin IP adreslerini almak için kullanabilirsiniz. 

Bu dış hizmetler için IP aralığını, **işlem ağ ayarları** sayfasında cihazın yerel Web Kullanıcı arabirimi aracılığıyla atarsınız. IoT Edge modüllerini dağıttıktan sonra, belirli bir modüle veya hizmete atanan IP adresini almak isteyebilirsiniz. 

IP adresini almak için panoda bu adımları izleyin:

1. Panonun sol bölmesinde **ad alanı**' na gidin. Bir dış Hizmetin dağıtıldığı ad alanına göre filtreleyin, bu durumda **ıotedge**.
1. Sol bölmede **bulma ve yük dengeleme > Hizmetleri**' ne gidin.
1. Sağ bölmede, `iotedge` Azure Stack Edge cihazındaki ad alanında çalışan tüm hizmetleri görürsünüz.

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
    

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes sorunlarını giderme hakkında bilgi edinin <!--insert link-->.
