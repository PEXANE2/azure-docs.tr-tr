---
title: Azure 'da Intel SGX quote Yardımcısı DaemonSet ile işlem dışı kanıtlama desteği
description: DaemonSet,, SGX uygulama işlemi dışında teklif oluşturmak için. Bu makalede, bir kapsayıcı içinde çalışan gizli iş yükleri için işlem dışı kanıtlama özelliğinin nasıl sağlandığı açıklanmaktadır.
ms.service: container-service
author: agowdamsft
ms.topic: overview
ms.date: 9/22/2020
ms.author: amgowda
ms.openlocfilehash: 90b74e51dce994c3041c15feec729ff1fae2bb00
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91001074"
---
# <a name="platform-software-management-with-sgx-quote-helper-daemon-set"></a>SGX quote yardımcı Daemon kümesi ile platform yazılım yönetimi

Uzaktan kanıtlama gerçekleştiren [uygulamalar](confidential-computing-enclaves.md) , oluşturulan bir teklif gerektirir. Bu TEKLIF, kimliğin ve uygulamanın durumunun şifreli kanıtını ve kuşın çalıştığı ortamı sağlar. TEKLIFIN oluşturulması, Intel platformu yazılım bileşenlerinin (PSW) parçası olan güvenilir yazılım bileşenleri gerektirir.

## <a name="overview"></a>Genel Bakış
 
Intel, teklif üretimini çalıştırmak için iki kanıtlama modunu destekler:
- işlem **içi**: şifreleme uygulama işleminin içinde güvenilir yazılım bileşenlerini barındırır

- **proc dışı**: güvenilir yazılım bileşenlerini, şifreleme uygulamasının dışında barındırır.
 
Open Enclave SDK kullanılarak oluşturulan SGX uygulamaları varsayılan olarak proc kanıtlama modunu kullanır. SGX tabanlı uygulamalar işlem dışı çalışmasına izin verir ve daha fazla barındırma gerektirir ve mimari şifreleme Service Manager (AESD) gibi gerekli bileşenleri uygulamanın dışında kullanıma sunacaktı.

Bu özelliğin kullanılması, Intel Platform güncelleştirmeleri veya DCAP sürücü güncelleştirmeleri sırasında, kuşve uygulamalarınızın çalışma süresini artıran bir şekilde **önerilmektedir**.

## <a name="why-and-what-are-the-benefits-of-out-of-proc"></a>Proc 'un neden ve ne avantajları vardır?

-   Her Kapsayıcılı uygulama için PSW 'nin teklif üretimi bileşenleri için hiçbir güncelleştirme gerekmez: işlem dışı, kapsayıcı sahiplerinin, kapsayıcılarında güncelleştirmeleri yönetmesi gerekmez. Kapsayıcı sahipleri, sağlayıcı tarafından güncelleştirilip yönetilecek ve kapsayıcı dışında Merkezi hizmeti çağıran sağlayıcı tarafından sağlanmış arabirimi kullanır.

-   Güncel olmayan PSW bileşenleri nedeniyle kanıtlama hatalarıyla ilgili endişelenmenize gerek yok: teklif üretimi, güvenilir bilgi işlem tabanının bir parçası olan (PCE) & sağlama sertifika kuşakı (QE), güvenilir yazılım bileşenleri (TCB) ile ilgilidir. Kanıtlama gereksinimlerinin korunması için bu SW bileşenlerinin güncel olması gerekir. Sağlayıcı, bu bileşenlere yönelik güncelleştirmeleri yönettiğinden, kapsayıcılarında güncel olmayan güvenilen SW bileşenleri nedeniyle müşterilerin kanıtlama hatalarıyla uğraşmak zorunda kalmazlar.

-   -Proc kanıtlama modunda EPC belleğinin daha iyi bir şekilde kullanılması, her bir kuşın uygulamasının, uzak kanıtlama için QE ve PCE 'nin kopyasını oluşturması gerekir. İşlem dışında, kapsayıcının bu kuştları barındırması gerekmez ve bu nedenle kapsayıcı kotasından şifreleme belleğini tüketmez.

-   Çekirdek zorlamasına karşı korumalar, SGX sürücü Linux çekirdeğine akışa eklendiğinde, bir kuşatma için daha yüksek ayrıcalıklara sahip olmaya yönelik bir zorlama olur. Bu ayrıcalık, kuşatma 'nın, işlem içi modda çalışan şifreleme uygulamasını kesen PCE 'yi çağırmasına izin verir. Varsayılan olarak, şifreleme bu izni almaz. Bu ayrıcalığı bir şifreleme uygulamasına vermek, uygulama yükleme işleminde değişiklik yapılmasını gerektirir. Bu, işlem dışı istekleri işleyen hizmetin sağlayıcısı bu ayrıcalıkla birlikte yüklendiğinden emin olmak için işlem dışı model için kolayca işlenir.

-   PSW & DCAP ile geriye dönük uyumluluğu denetleme gereksinimi yoktur. PSW 'nin teklif üretimi bileşenleri güncelleştirmeleri, güncelleştirmeden önce sağlayıcı tarafından geriye dönük uyumluluk için onaylanır. Bu, uyumluluk sorunlarını ele almanıza ve gizli iş yükleri için güncelleştirmeleri dağıtmadan önce bunları ele almanıza yardımcı olur.

## <a name="how-does-the-out-of-proc-attestation-mode-work-for-confidential-workloads-scenario"></a>İşlem dışı kanıtlama modu, gizli iş yükleri senaryosu için nasıl çalışır?

Üst düzey tasarım, teklif istek sahibinin ve teklif oluşturmanın ayrı olarak yürütüldüğü modeli, ancak aynı fiziksel makineye göre de izler. Teklif üretimi, merkezi bir şekilde yapılır ve tüm varlıklardan gelen teklıflere yönelik isteklere hizmet verir. Her varlık için, tekliflerin talep etmek için uygun şekilde tanımlanması ve keşfedilebilir olması gerekir.

![SGX quote Yardımcısı aesd](./media/confidential-nodes-out-of-proc-attestation/aesmmanager.png)

Yukarıdaki soyut model, zaten kullanılabilir AESE hizmetinden yararlanarak gizli iş yükü senaryosu için geçerlidir. AESK Kapsayıcılı ve Kubernetes kümesinde bir daemonSet olarak dağıtılır. Kubernetes, her bir aracı düğümüne dağıtılacak bir pod 'a kaydırılmış bir AESE hizmet kapsayıcısının tek bir örneğini garanti eder. AESM hizmet kapsayıcısının, QE ve PCE kuşakları başlatmak üzere SGX-Device-Plugin ' den EPC belleği istemesi gerektiğinden, yeni SGX quote daemonset, SGX-Device-Plugin daemonset üzerinde bir bağımlılığa sahip olacaktır.

Oluşturma sırasında **SGX_AESM_ADDR = 1** ortam değişkenini ayarlayarak, her kapsayıcının proc dışı teklif üretimini kullanması gerekir. Kapsayıcı Ayrıca, isteği varsayılan UNIX etki alanı yuvasına yönlendirmekten sorumlu olan libsgx-quote-EX paketini de içermelidir

Bir uygulama daha önce olduğu gibi işlem içi kanıtlama kullanmaya devam edebilir, ancak her iki işlem içi ve proc dışı bir uygulama içinde kullanılamaz. Proc dışı altyapı varsayılan olarak kullanılabilir ve kaynakları tüketir.

## <a name="sample-implementation"></a>Örnek uygulama

Aşağıdaki Docker dosyası, açık bir Kuşve tabanlı uygulama için bir örnektir. Docker dosyasında SGX_AESM_ADDR = 1 ortam değişkenini ayarlayın veya dağıtım dosyasında ayarlayın. Docker dosyası ve dağıtımı YAML ayrıntıları için aşağıdaki örneği izleyin. 

  > [!Note] 
  > Intel 'ten alınan **libsgx-quote-EX** , işlem dışı kanıtlama işleminin düzgün şekilde çalışması için uygulama kapsayıcısında paketlenmesi gerekir.
    
```yaml
# Refer to Intel_SGX_Installation_Guide_Linux for detail
FROM ubuntu:18.04 as sgx_base
RUN apt-get update && apt-get install -y \
    wget \
    gnupg

# Add the repository to sources, and add the key to the list of
# trusted keys used by the apt to authenticate packages
RUN echo "deb [arch=amd64] https://download.01.org/intel-sgx/sgx_repo/ubuntu bionic main" | tee /etc/apt/sources.list.d/intel-sgx.list \
    && wget -qO - https://download.01.org/intel-sgx/sgx_repo/ubuntu/intel-sgx-deb.key | apt-key add -
# Add Microsoft repo for az-dcap-client
RUN echo "deb [arch=amd64] https://packages.microsoft.com/ubuntu/18.04/prod bionic main" | tee /etc/apt/sources.list.d/msprod.list \
    && wget -qO - https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

FROM sgx_base as sgx_sample
RUN apt-get update && apt-get install -y \
    clang-7 \
    libssl-dev \
    gdb \
    libprotobuf10 \
    libsgx-dcap-ql \
    libsgx-quote-ex \
    az-dcap-client \
    open-enclave
WORKDIR /opt/openenclave/share/openenclave/samples/remote_attestation
RUN . /opt/openenclave/share/openenclave/openenclaverc \
    && make build
# this sets the flag for out of proc attestation mode. alternatively you can set this flag on the deployment files
ENV SGX_AESM_ADDR=1 

CMD make run
```
Alternatif olarak, proc kanıtlama modu aşağıda gösterildiği gibi Deployment YAML dosyasında ayarlanabilir

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: sgx-test
spec:
  template:
    spec:
      containers:
      - name: sgxtest
        image: <registry>/<repository>:<version>
        env:
        - name: SGX_AESM_ADDR
          value: 1
        resources:
          limits:
            kubernetes.azure.com/sgx_epc_mem_in_MiB: 10
        volumeMounts:
        - name: var-run-aesmd
          mountPath: /var/run/aesmd
      restartPolicy: "Never"
      volumes:
      - name: var-run-aesmd
        hostPath:
          path: /var/run/aesmd
```

## <a name="next-steps"></a>Sonraki Adımlar
[AKS üzerinde gizli düğümler (DCsv2-Series) sağlama](./confidential-nodes-aks-get-started.md)

[Hızlı başlangıç örnekleri gizli kapsayıcıları](https://github.com/Azure-Samples/confidential-container-samples)

[DCsv2 SKU listesi](https://docs.microsoft.com/azure/virtual-machines/dcv2-series)

<!-- LINKS - external -->
[Azure Attestation]: https://docs.microsoft.com/en-us/azure/attestation/


<!-- LINKS - internal -->
[DC Virtual Machine]: /confidential-computing/virtual-machine-solutions