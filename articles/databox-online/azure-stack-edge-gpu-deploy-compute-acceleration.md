---
title: Kubernetes dağıtımları için Azure Stack Edge cihazlarda işlem hızlandırma GPU 'SU veya VPU kullanma | Microsoft Docs
description: Azure Stack Edge Pro GPU, Azure Stack Edge Pro R veya Azure Stack Edge Mini RI for Kubernetes dağıtımları üzerinde işlem hızlandırma GPU 'SU veya VPU kullanımını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/22/2021
ms.author: alkohli
ms.openlocfilehash: 3eb648af60a7be62d08f6b172347778d2358643c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102440131"
---
# <a name="use-compute-acceleration-on-azure-stack-edge-pro-gpu-for-kubernetes-deployment"></a>Kubernetes dağıtımı için Azure Stack Edge Pro GPU üzerinde işlem hızlandırma kullanma

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

Bu makalede, Kubernetes dağıtımlarını kullanırken Azure Stack Edge cihazlarında işlem hızlandırmasının nasıl kullanılacağı açıklanır. Makale Azure Stack Edge Pro GPU, Azure Stack Edge Pro R ve Azure Stack Edge Mini R cihazları için geçerlidir.


## <a name="about-compute-acceleration"></a>İşlem hızlandırma hakkında 

Merkezi Işlem birimi (CPU), bir bilgisayarda çalışan çoğu işlem için varsayılan genel amaçlı hesaplamanıza yöneliktir. Genellikle özel bir bilgisayar donanımı, bazı işlevleri bir CPU 'da yazılımda çalıştırmadan daha verimli bir şekilde gerçekleştirmek için kullanılır. Örneğin, piksel verilerinin işlenmesini hızlandırmak için bir grafik Işleme birimi (GPU) kullanılabilir.  

İşlem hızlandırma, bir grafik Işleme birimi (GPU), bir görüntü Işleme birimi (VPU) veya bir alan programlanabilir kapı dizisi (FPGA) donanım hızlandırma için kullanıldığı Azure Stack Edge cihazları için özel olarak kullanılan bir terimdir. Azure Stack Edge cihazınıza dağıtılan iş yüklerinin çoğu kritik zamanlamayı, birden çok kamera akışını ve/veya yüksek kare hızını ve bunların tümünü belirli donanım hızlandırmayı gerektirir.

Makale yalnızca aşağıdaki cihazlar için işlem hızlandırmayı yalnızca GPU veya VPU kullanarak tartışacaktır:

- **Azure Stack Edge Pro GPU** -bu cihazlarda 1 veya 2 NVIDIA T4 Tensor Core GPU bulunabilir. Daha fazla bilgi için bkz. [NVIDIA T4](https://www.nvidia.com/en-us/data-center/tesla-t4/).
- **Azure Stack Edge Pro R** -bu cihazlarda 1 NVIDIA T4 Tensor Core GPU vardır. Daha fazla bilgi için bkz. [NVIDIA T4](https://www.nvidia.com/en-us/data-center/tesla-t4/).
- **Azure Stack Edge Mini R** -bu cihazlarda 1 Intel Movidius Myriad X VPU vardır. Daha fazla bilgi için bkz. [Intel Movidius Myriad X VPU](https://www.movidius.com/MyriadX).


## <a name="use-gpu-for-kubernetes-deployment"></a>Kubernetes dağıtımı için GPU kullanma

Aşağıdaki örnek, `yaml` GPU ile Azure Stack Edge Pro GPU veya Azure Stack Edge Pro R cihazı için kullanılabilir.

<!--In a production scenario, Pods are not used directly and these are wrapped around higher level constructs like Deployment, ReplicaSet which maintain the desired state in case of pod restarts, failures.-->

```yml
apiVersion: v1
kind: Pod
metadata:
  name: gpu-pod
spec:
  containers:
    - name: cuda-container
      image: nvidia/cuda:9.0-devel
      resources:
        limits:
          nvidia.com/gpu: 1 # requesting 1 GPU
    - name: digits-container
      image: nvidia/digits:6.0
      resources:
        limits:
          nvidia.com/gpu: 1 # requesting 1 GPU
```


## <a name="use-vpu-for-kubernetes-deployment"></a>Kubernetes dağıtımı için VPU kullanma

Aşağıdaki örnek, `yaml` VPU içeren bir Azure Stack Edge Mini R cihazında kullanılabilir.

```yml
apiVersion: batch/v1
kind: Job
metadata:
 name: intelvpu-demo-job
 labels:
   jobgroup: intelvpu-demo
spec:
 template:
   metadata:
     labels:
       jobgroup: intelvpu-demo
   spec:
     restartPolicy: Never
     containers:
       -
         name: intelvpu-demo-job-1
         image: ubuntu-demo-openvino:devel
         imagePullPolicy: IfNotPresent
         command: [ "/do_classification.sh" ]
         resources:
           limits:
             vpu.intel.com/hddl: 1
```


## <a name="next-steps"></a>Sonraki adımlar

[Kubectl 'yi, Azure Stack Edge Pro GPU cihazınızda bir PersistentVolume ile bir Kubernetes durum bilgisi olan bir uygulamayı çalıştırmak için](azure-stack-edge-gpu-deploy-stateful-application-static-provision-kubernetes.md)nasıl kullanacağınızı öğrenin.
