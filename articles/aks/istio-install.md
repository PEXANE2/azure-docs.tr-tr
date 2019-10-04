---
title: Azure Kubernetes Service (AKS) ' de Istio 'yu kurma
description: Azure Kubernetes Service (AKS) kümesinde hizmet ağı oluşturma ve kullanma hakkında bilgi edinin
services: container-service
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 04/19/2019
ms.author: pabouwer
ms.openlocfilehash: 9344d2832c37c34d5690dc8f3aae7394ca644276
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827316"
---
# <a name="install-and-use-istio-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) ' de Istio 'yu yükleyip kullanma

[Istio][istio-github] , bir Kubernetes kümesindeki mikro hizmetlerde anahtar bir işlevsellik kümesi sağlayan açık kaynaklı bir hizmet kafesidir. Bu özellikler trafik yönetimi, hizmet kimliği ve güvenlik, ilke zorlama ve Observability içerir. Istio hakkında daha fazla bilgi için resmi [nedir?][istio-docs-concepts] belgesine bakın.

Bu makalede, Istio 'un nasıl yükleneceği gösterilmektedir. IBU `istioctl` istemci ikilisi istemci makinenize yüklenir ve ıKS bileşenleri AKS üzerinde bir Kubernetes kümesine yüklenir.

> [!NOTE]
> Bu yönergeler, `1.1.3` ' a başvurun.
>
> Ilero `1.1.x` yayınları, Kubernetes sürümleri `1.11`, `1.12`, `1.13` ' e karşı Istio ekibi tarafından test edilmiştir. [GitHub-Istio yayımlarında][istio-github-releases] ek istio sürümlerini ve her bir [Yayınımo haberleri][istio-release-notes]hakkındaki bilgileri bulabilirsiniz.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Istio 'yu indir
> * Istio istioctl istemci ikilisini yükler
> * AKS 'e Istio CRDs 'yi yükler
> * AKS 'e Istio bileşenlerini yükler
> * Istio yüklemesini doğrulama
> * Eklentilere erişme
> * AKS 'ten Istio 'dan kaldırma

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede açıklanan adımlarda, RBAC etkin olan bir AKS kümesi (Kubernetes `1.11` ve üzeri) oluşturduğunuzu ve kümeyle birlikte `kubectl` bağlantısı kurtığınızı varsayalım. Bu öğelerin herhangi biriyle ilgili yardıma ihtiyacınız varsa, [aks hızlı başlangıç][aks-quickstart]bölümüne bakın.

Bu yönergeleri izlemek ve Istio 'yu yüklemek için [Helm][helm] gerekir. Kümenizde doğru bir şekilde yüklenip yapılandırılanınızın @no__t veya daha yeni bir sürüme sahip olmanız önerilir. Held 'yi yüklemeyle ilgili yardıma ihtiyacınız varsa bkz. [aks helk Yükleme Kılavuzu][helm-install]. Tüm istio Pod 'ler de Linux düğümlerinde çalışacak şekilde zamanlanmalıdır.

AKS kümenizde Istio çalıştırmaya yönelik ek kaynak gereksinimlerini anlamak için, [Istio performans ve ölçeklenebilirlik](https://istio.io/docs/concepts/performance-and-scalability/) belgelerini okuduğunuzdan emin olun. Çekirdek ve bellek gereksinimleri, belirli iş yükünüze göre farklılık gösterecektir. Kuruluma uygun sayıda düğüm ve VM boyutu seçin.

Bu makale, Istio yükleme kılavuzunu çeşitli ayrı adımlara ayırır. Nihai sonuç, resmi Istio yükleme [kılavuzundaki][istio-install-helm]yapıda aynıdır.

## <a name="download-istio"></a>Istio 'yu indir

İlk olarak, en son Istio sürümünü indirip ayıklayın. Adımlar MacOS, Linux veya Linux için Windows alt sistemi ve bir PowerShell kabuğu için bash kabuğu için biraz farklıdır. Tercih ettiğiniz ortamla eşleşen aşağıdaki yüklemede birini seçin:

* [Linux için MacOS, Linux veya Windows alt sisteminde Bash](#bash)
* [PowerShell](#powershell)

### <a name="bash"></a>Bash

MacOS 'ta en son Istio sürümünü indirmek için `curl` kullanın ve ardından `tar` ' i kullanarak aşağıdaki şekilde ayıklayın:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.1.3

# MacOS
curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-osx.tar.gz" | tar xz
```

Linux veya Linux için Windows alt sistemi üzerinde, en son Istio sürümünü indirmek için `curl` kullanın ve ardından `tar` ' i kullanarak aşağıdaki şekilde ayıklayın:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.1.3

curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-linux.tar.gz" | tar xz
```

Şimdi, [Istio istioctl istemci ikilisini yüklemek](#install-the-istio-istioctl-client-binary)için bölümüne gidin.

### <a name="powershell"></a>PowerShell

PowerShell 'de, en son Istio sürümünü indirmek için `Invoke-WebRequest` kullanın ve ardından `Expand-Archive` ' i kullanarak aşağıdaki şekilde ayıklayın:

```powershell
# Specify the Istio version that will be leveraged throughout these instructions
$ISTIO_VERSION="1.1.3"

# Windows
# Use TLS 1.2
[Net.ServicePointManager]::SecurityProtocol = "tls12"
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-win.zip" -OutFile "istio-$ISTIO_VERSION.zip"
Expand-Archive -Path "istio-$ISTIO_VERSION.zip" -DestinationPath .
```

Şimdi, [Istio istioctl istemci ikilisini yüklemek](#install-the-istio-istioctl-client-binary)için bölümüne gidin.

## <a name="install-the-istio-istioctl-client-binary"></a>Istio istioctl istemci ikilisini yükler

> [!IMPORTANT]
> Bu bölümdeki adımları, indirdiğiniz ve ayıkladığınız Istio sürümünün en üst düzey klasöründen çalıştırtığınızdan emin olun.

@No__t-0 istemci ikili dosyası, istemci makinenizde çalışır ve Istio hizmeti ağıyla etkileşime girebilmeniz için sağlar. Yüklemeyi adımlar, istemci işletim sistemleri arasında biraz farklıdır. Tercih ettiğiniz ortamla eşleşen aşağıdaki yüklemede birini seçin:

* [MacOS](#macos)
* [Linux için Linux veya Windows alt sistemi](#linux-or-windows-subsystem-for-linux)
* [Windows](#windows)

### <a name="macos"></a>MacOS

MacOS `istioctl` istemci ikilisini MacOS üzerinde Bash tabanlı bir kabuğa yüklemek için aşağıdaki komutları kullanın. Bu komutlar `istioctl` istemci ikilisini `PATH` ' inizdeki standart Kullanıcı programı konumuna kopyalar.

```bash
cd istio-$ISTIO_VERSION
sudo cp ./bin/istioctl /usr/local/bin/istioctl
sudo chmod +x /usr/local/bin/istioctl
```

IBU `istioctl` istemci ikilisi için komut satırı tamamlamayı isterseniz, bunu aşağıdaki şekilde ayarlayın:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && istioctl collateral --bash -o ~/completions
source ~/completions/istioctl.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/istioctl.bash" >> ~/.bashrc
```

Şimdi de bir sonraki bölüme geçiş [yapın.](#install-the-istio-crds-on-aks)

### <a name="linux-or-windows-subsystem-for-linux"></a>Linux için Linux veya Windows alt sistemi

Linux veya [Linux Için Windows alt sistemi veya Linux][install-wsl]üzerinde bir bash tabanlı kabuğa `istioctl` istemci ikilisini yüklemek için aşağıdaki komutları kullanın. Bu komutlar `istioctl` istemci ikilisini `PATH` ' inizdeki standart Kullanıcı programı konumuna kopyalar.

```bash
cd istio-$ISTIO_VERSION
sudo cp ./bin/istioctl /usr/local/bin/istioctl
sudo chmod +x /usr/local/bin/istioctl
```

IBU `istioctl` istemci ikilisi için komut satırı tamamlamayı isterseniz, bunu aşağıdaki şekilde ayarlayın:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && istioctl collateral --bash -o ~/completions
source ~/completions/istioctl.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/istioctl.bash" >> ~/.bashrc
```

Şimdi de bir sonraki bölüme geçiş [yapın.](#install-the-istio-crds-on-aks)

### <a name="windows"></a>Windows

@No__t-0 istemci ikilisini Windows üzerinde **PowerShell**tabanlı bir kabuğa yüklemek için aşağıdaki komutları kullanın. Bu komutlar `istioctl` istemci ikilisini bir Istio klasörüne kopyalar ve ardından, `PATH` ' i kullanarak (geçerli kabukta) ve kalıcı olarak (kabukta yeniden başlatmalar arasında) kullanılabilir hale getirir. Bu komutları çalıştırmak için yükseltilmiş (yönetici) ayrıcalıklarına ihtiyacınız yoktur ve kabuğunu yeniden başlatmanız gerekmez.

```powershell
# Copy istioctl.exe to C:\Istio
cd istio-$ISTIO_VERSION
New-Item -ItemType Directory -Force -Path "C:\Istio"
Copy-Item -Path .\bin\istioctl.exe -Destination "C:\Istio\"

# Add C:\Istio to PATH. 
# Make the new PATH permanently available for the current User, and also immediately available in the current shell.
$PATH = [environment]::GetEnvironmentVariable("PATH", "User") + "; C:\Istio\"
[environment]::SetEnvironmentVariable("PATH", $PATH, "User") 
[environment]::SetEnvironmentVariable("PATH", $PATH)
```

Şimdi de bir sonraki bölüme geçiş [yapın.](#install-the-istio-crds-on-aks)

## <a name="install-the-istio-crds-on-aks"></a>AKS 'e Istio CRDs 'yi yükler

> [!IMPORTANT]
> Bu bölümdeki adımları, indirdiğiniz ve ayıkladığınız Istio sürümünün en üst düzey klasöründen çalıştırtığınızdan emin olun.

İstio, çalışma zamanı yapılandırmasını yönetmek için [özel kaynak tanımları (CRDs)][kubernetes-crd] kullanır. IBU bileşenleri üzerinde bir bağımlılığı olduğundan, önce Istio CRDs 'yi yüklememiz gerekir. AKS kümenizdeki `istio-system` ad alanına Istio CRDs 'yi yüklemek için Helm ve `istio-init` grafiğini kullanın:

```azurecli
helm install install/kubernetes/helm/istio-init --name istio-init --namespace istio-system
```

[İşler][kubernetes-jobs] , crds 'yi yüklemek için `istio-init` hele grafiğinin bir parçası olarak dağıtılır. Bu işlerin tamamlanması, küme ortamınıza bağlı olarak 1 ila 2 dakika arasında sürer. İşlerin şu şekilde başarıyla tamamlandığını doğrulayabilirsiniz:

```azurecli
kubectl get jobs -n istio-system
```

Aşağıdaki örnek çıktıda başarıyla tamamlanan işler gösterilmektedir.

```console
NAME                COMPLETIONS   DURATION   AGE
istio-init-crd-10   1/1           16s        18s
istio-init-crd-11   1/1           15s        18s
```

İşlerin başarılı bir şekilde tamamlandığını Onayladığımıza göre, doğru sayıda Istio CRDs yüklü olduğunu doğrulayalım. Ortamınız için uygun komutu çalıştırarak tüm 53 Istio CRDs 'nin yüklendiğini doğrulayabilirsiniz. Komut `53` sayısını döndürmelidir.

Bash

```bash
kubectl get crds | grep 'istio.io' | wc -l
```

Powershell

```powershell
(kubectl get crds | Select-String -Pattern 'istio.io').Count
```

Bu noktaya sahipseniz, bu durumda Istio CRDs 'yi başarıyla yüklemiş olursunuz. Şimdi de bir sonraki bölüme geçin. bu [bileşenleri AKS 'e yükleyebilirsiniz](#install-the-istio-components-on-aks).

## <a name="install-the-istio-components-on-aks"></a>AKS 'e Istio bileşenlerini yükler

> [!IMPORTANT]
> Bu bölümdeki adımları, indirdiğiniz ve ayıkladığınız Istio sürümünün en üst düzey klasöründen çalıştırtığınızdan emin olun.

[Grafana][grafana] ve [kiali][kiali] 'nin bir parçası olarak ilerliyoruz. Grafana, analiz ve izleme panoları sağlar ve Kiali bir hizmet ağı Observability panosu sağlar. Kurulumumuzda, bu bileşenlerin her biri [gizli][kubernetes-secrets]olarak sağlanması gereken kimlik bilgilerini gerektirir.

Istio bileşenlerini yükleyebilmemiz için, hem Grafana hem de Kiali için gizli dizileri oluşturuyoruz. Ortamınız için uygun komutları çalıştırarak bu gizli dizileri oluşturun.

### <a name="add-grafana-secret"></a>Grafana gizli dizisi Ekle

@No__t-0 belirtecini parolanızla değiştirin ve aşağıdaki komutları çalıştırın:

#### <a name="macos-linux"></a>MacOS, Linux

```bash
GRAFANA_USERNAME=$(echo -n "grafana" | base64)
GRAFANA_PASSPHRASE=$(echo -n "REPLACE_WITH_YOUR_SECURE_PASSWORD" | base64)

cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: grafana
  namespace: istio-system
  labels:
    app: grafana
type: Opaque
data:
  username: $GRAFANA_USERNAME
  passphrase: $GRAFANA_PASSPHRASE
EOF
```

#### <a name="windows"></a>Windows

```powershell
$GRAFANA_USERNAME=[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("grafana"))
$GRAFANA_PASSPHRASE=[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("REPLACE_WITH_YOUR_SECURE_PASSWORD"))

"apiVersion: v1
kind: Secret
metadata:
  name: grafana
  namespace: istio-system
  labels:
    app: grafana
type: Opaque
data:
  username: $GRAFANA_USERNAME
  passphrase: $GRAFANA_PASSPHRASE" | kubectl apply -f -
```

### <a name="add-kiali-secret"></a>Kiali gizli dizisi Ekle

@No__t-0 belirtecini parolanızla değiştirin ve aşağıdaki komutları çalıştırın:

#### <a name="macos-linux"></a>MacOS, Linux

```bash
KIALI_USERNAME=$(echo -n "kiali" | base64)
KIALI_PASSPHRASE=$(echo -n "REPLACE_WITH_YOUR_SECURE_PASSWORD" | base64)

cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: kiali
  namespace: istio-system
  labels:
    app: kiali
type: Opaque
data:
  username: $KIALI_USERNAME
  passphrase: $KIALI_PASSPHRASE
EOF
```

#### <a name="windows"></a>Windows

```powershell
$KIALI_USERNAME=[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("kiali"))
$KIALI_PASSPHRASE=[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("REPLACE_WITH_YOUR_SECURE_PASSWORD"))

"apiVersion: v1
kind: Secret
metadata:
  name: kiali
  namespace: istio-system
  labels:
    app: kiali
type: Opaque
data:
  username: $KIALI_USERNAME
  passphrase: $KIALI_PASSPHRASE" | kubectl apply -f -
```

### <a name="install-istio-components"></a>Istio bileşenlerini yükler

AKS kümenizdeki Grafana ve Kiali gizli dizilerini başarıyla oluşturduğumuzdan, bu durumda Istio bileşenlerinin yüklenmesi zaman alabilir. AKS kümenizdeki `istio-system` ad alanına Istio bileşenlerini yüklemek için Helm ve `istio` grafiğini kullanın. Ortamınız için uygun komutları kullanın.

> [!NOTE]
> Yüklememizin kapsamında aşağıdaki seçenekleri kullanıyoruz:
> - `global.controlPlaneSecurityEnabled=true`-denetim düzlemi için etkin ve karşılıklı TLS
> - `mixer.adapters.useAdapterCRDs=false`-devre dışı oldukları için karıştırıcı bağdaştırıcısı CRDs 'deki izlemeleri kaldırın ve bu, performansı iyileştirir
> - `grafana.enabled=true`-analiz ve izleme panoları için Grafana dağıtımını etkinleştirme
> - `grafana.security.enabled=true`-Grafana için kimlik doğrulamasını etkinleştir
> - `tracing.enabled=true`-izleme için Jaeger dağıtımını etkinleştir
> - `kiali.enabled=true`-bir hizmet ağı Observability panosu için Kiali dağıtımını etkinleştirme

Bash

```bash
helm install install/kubernetes/helm/istio --name istio --namespace istio-system \
  --set global.controlPlaneSecurityEnabled=true \
  --set mixer.adapters.useAdapterCRDs=false \
  --set grafana.enabled=true --set grafana.security.enabled=true \
  --set tracing.enabled=true \
  --set kiali.enabled=true
```

Powershell

```powershell
helm install install/kubernetes/helm/istio --name istio --namespace istio-system `
  --set global.controlPlaneSecurityEnabled=true `
  --set mixer.adapters.useAdapterCRDs=false `
  --set grafana.enabled=true --set grafana.security.enabled=true `
  --set tracing.enabled=true `
  --set kiali.enabled=true
```

@No__t-0 Held grafiği çok sayıda nesne dağıtır. Listeyi yukarıdaki `helm install` komutunun çıktısından görebilirsiniz. Istio bileşenlerinin dağıtımı, küme ortamınıza bağlı olarak 4 ila 5 dakika sürebilir.

> [!NOTE]
> Tüm istio Pod 'leri Linux düğümlerinde çalışacak şekilde zamanlanmalıdır. Kümenizde Linux düğüm havuzlarının yanı sıra Windows Server düğüm havuzlarınız varsa, tüm Istil 'lerin Linux düğümlerinde çalışacak şekilde zamanlandığını doğrulayın.

Bu noktada, AKS kümenize Istio 'u dağıttık. Başarılı bir Istio dağıtımına sahip olduğunuzdan emin olmak için, bir sonraki bölüme ilerleyip bu [yüklemeyi doğrulayalım](#validate-the-istio-installation).

## <a name="validate-the-istio-installation"></a>Istio yüklemesini doğrulama

Önce beklenen hizmetlerin oluşturulduğunu onaylayın. Çalışan hizmetleri görüntülemek için [kubectl Get svc][kubectl-get] komutunu kullanın. @No__t-0 ad alanını sorgulayın, burada, Istio ve eklenti bileşenlerinin `istio` Helm grafiği tarafından yüklendiği yer:

```console
kubectl get svc --namespace istio-system --output wide
```

Aşağıdaki örnek çıktıda Şu anda çalışıyor olması gereken hizmetler gösterilmektedir:

- `istio-*` hizmetleri
- `jaeger-*`, `tracing` ve `zipkin` eklenti izleme hizmetleri
- `prometheus` eklenti ölçüm hizmeti
- `grafana` eklenti Analizi ve izleme panosu hizmeti
- `kiali` eklenti hizmeti kafesi Pano hizmeti

@No__t-0 ' ı bir dış IP `<pending>` gösteriyorsa, Azure ağ tarafından bir IP adresi atanmadan birkaç dakika bekleyin.

```console
NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                                                                                                                                      AGE       SELECTOR
grafana                  ClusterIP      10.0.81.182    <none>          3000/TCP                                                                                                                                     119s      app=grafana
istio-citadel            ClusterIP      10.0.96.33     <none>          8060/TCP,15014/TCP                                                                                                                           119s      istio=citadel
istio-galley             ClusterIP      10.0.237.158   <none>          443/TCP,15014/TCP,9901/TCP                                                                                                                   119s      istio=galley
istio-ingressgateway     LoadBalancer   10.0.154.12    20.188.211.19   15020:30603/TCP,80:31380/TCP,443:31390/TCP,31400:31400/TCP,15029:31198/TCP,15030:30610/TCP,15031:30937/TCP,15032:31344/TCP,15443:31499/TCP   119s      app=istio-ingressgateway,istio=ingressgateway,release=istio
istio-pilot              ClusterIP      10.0.178.56    <none>          15010/TCP,15011/TCP,8080/TCP,15014/TCP                                                                                                       119s      istio=pilot
istio-policy             ClusterIP      10.0.116.118   <none>          9091/TCP,15004/TCP,15014/TCP                                                                                                                 119s      istio-mixer-type=policy,istio=mixer
istio-sidecar-injector   ClusterIP      10.0.31.160    <none>          443/TCP                                                                                                                                      119s      istio=sidecar-injector
istio-telemetry          ClusterIP      10.0.187.246   <none>          9091/TCP,15004/TCP,15014/TCP,42422/TCP                                                                                                       119s      istio-mixer-type=telemetry,istio=mixer
jaeger-agent             ClusterIP      None           <none>          5775/UDP,6831/UDP,6832/UDP                                                                                                                   119s      app=jaeger
jaeger-collector         ClusterIP      10.0.116.63    <none>          14267/TCP,14268/TCP                                                                                                                          119s      app=jaeger
jaeger-query             ClusterIP      10.0.22.108    <none>          16686/TCP                                                                                                                                    119s      app=jaeger
kiali                    ClusterIP      10.0.142.50    <none>          20001/TCP                                                                                                                                    119s      app=kiali
prometheus               ClusterIP      10.0.138.134   <none>          9090/TCP                                                                                                                                     119s      app=prometheus
tracing                  ClusterIP      10.0.165.210   <none>          80/TCP                                                                                                                                       118s      app=jaeger
zipkin                   ClusterIP      10.0.126.211   <none>          9411/TCP                                                                                                                                     118s      app=jaeger
```

Ardından, gerekli yığınların oluşturulduğunu onaylayın. [Kubectl Get Pod][kubectl-get] komutunu kullanın ve `istio-system` ad alanını sorgulayın:

```console
kubectl get pods --namespace istio-system
```

Aşağıdaki örnek çıktı, çalıştıran Pod 'leri göstermektedir:

- `istio-*` Pod
- `prometheus-*` eklenti ölçümleri Pod
- `grafana-*` eklenti Analizi ve izleme panosu Pod
- `kiali` eklenti hizmeti kafes panosu Pod

```console
NAME                                     READY     STATUS      RESTARTS   AGE
grafana-88779954d-nzpm7                  1/1       Running     0          6m26s
istio-citadel-7f699dc8c8-n7q8g           1/1       Running     0          6m26s
istio-galley-649bc8cd97-wfjzm            1/1       Running     0          6m26s
istio-ingressgateway-65dfbd566-42wkn     1/1       Running     0          6m26s
istio-init-crd-10-tmtw5                  0/1       Completed   0          20m38s
istio-init-crd-11-ql25l                  0/1       Completed   0          20m38s
istio-pilot-958dd8cc4-4ckf9              2/2       Running     0          6m26s
istio-policy-86b4b7cf9-zf7v7             2/2       Running     4          6m26s
istio-sidecar-injector-d48786c5c-pmrj9   1/1       Running     0          6m26s
istio-telemetry-7f6996fdcc-84w94         2/2       Running     3          6m26s
istio-tracing-79db5954f-h7hmz            1/1       Running     0          6m26s
kiali-5c4cdbb869-s28dv                   1/1       Running     0          6m26s
prometheus-67599bf55b-pgxd8              1/1       Running     0          6m26s
```

@No__t-1 durumu olan iki `istio-init-crd-*` Pod olmalıdır. Bu FID 'ler, daha önceki bir adımda CRDs 'yi oluşturan işleri çalıştırmaktan sorumludur. Diğer tüm yığınların `Running` durumunu göstermesi gerekir. Ayırımlarınızın bu durumları yoksa, tamamlanana kadar bir dakika veya iki tane bekleyin. Herhangi bir pod bir sorun bildirirse, çıktısını ve durumlarını gözden geçirmek için [kubectl 'yi bir pod betimleyen][kubectl-describe] komutunu kullanın.

## <a name="accessing-the-add-ons"></a>Eklentilere erişme

Yukarıdaki kurulumumuzda daha fazla işlev sağlayan bir dizi eklenti yüklenmiştir. Eklentilerin Kullanıcı arabirimleri, bir dış IP adresi aracılığıyla herkese açık bir şekilde gösterilmez. Eklenti kullanıcı arabirimlerine erişmek için [kubectl Port-Forward][kubectl-port-forward] komutunu kullanın. Bu komut, istemci makineniz ile AKS kümenizdeki ilgili Pod arasında güvenli bir bağlantı oluşturur.

Bu makalenin önceki kısımlarında yer aldığı için kimlik bilgilerini belirterek Grafana ve Kiali için ek bir güvenlik katmanı ekledik.

### <a name="grafana"></a>Grafana

Istio için analiz ve izleme panoları [Grafana][grafana]tarafından sağlanır. İstemci makinenize `3000` yerel bağlantı noktasını, AKS kümenizde Grafana çalıştıran Pod üzerindeki `3000` numaralı bağlantı noktasına iletin:

```console
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000
```

Aşağıdaki örnek çıktıda, Grafana için yapılandırılmış bağlantı noktası iletme görülmektedir:

```console
Forwarding from 127.0.0.1:3000 -> 3000
Forwarding from [::1]:3000 -> 3000
```

Artık Grafana [-1 @no__t](http://localhost:3000)istemci makinenizde aşağıdaki URL 'ye ulaşabilirsiniz. İstendiğinde, Grafana gizli anahtarı aracılığıyla oluşturduğunuz kimlik bilgilerini kullanmayı unutmayın.

### <a name="prometheus"></a>Prometheus

[Ceso ölçümleri Prometheus][prometheus]tarafından sağlanmaktadır. İstemci makinenizde `9090` yerel bağlantı noktasını AKS kümenizde Prometheus çalıştıran pod üzerinde `9090` bağlantı noktasına iletin:

```console
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=prometheus -o jsonpath='{.items[0].metadata.name}') 9090:9090
```

Aşağıdaki örnek çıktıda, Prometheus için yapılandırılmış bağlantı noktası iletme görülmektedir:

```console
Forwarding from 127.0.0.1:9090 -> 9090
Forwarding from [::1]:9090 -> 9090
```

Artık, istemci makinenizde ( [http://localhost:9090](http://localhost:9090)) aşağıdaki URL 'de Prometheus ifade tarayıcısına ulaşabilirsiniz.

### <a name="jaeger"></a>Jaeger

Istio 'da izleme, [Jaeger][jaeger]tarafından sağlanır. İstemci makinenizdeki `16686` ' a ait yerel bağlantı noktasını AKS kümenizde Caeger çalıştıran pod üzerinde `16686` bağlantı noktasına iletin:

```console
kubectl port-forward -n istio-system $(kubectl get pod -n istio-system -l app=jaeger -o jsonpath='{.items[0].metadata.name}') 16686:16686
```

Aşağıdaki örnek çıktıda, Jaeger için yapılandırılmış bağlantı noktası iletme görülmektedir:

```console
Forwarding from 127.0.0.1:16686 -> 16686
Forwarding from [::1]:16686 -> 16686
```

Artık, istemci makinenizde aşağıdaki URL 'de Jaeger izleme Kullanıcı arabirimine ulaşabilirsiniz- [http://localhost:16686](http://localhost:16686).

### <a name="kiali"></a>Kiali

Bir hizmet ağı Observability panosu [Kiali][kiali]tarafından sağlanır. İstemci makinenize `20001` yerel bağlantı noktasını, AKS kümenizde Kiali çalıştıran Pod üzerindeki `20001` numaralı bağlantı noktasına iletin:

```console
kubectl port-forward -n istio-system $(kubectl get pod -n istio-system -l app=kiali -o jsonpath='{.items[0].metadata.name}') 20001:20001
```

Aşağıdaki örnek çıktıda, Kiali için yapılandırılmış bağlantı noktası iletme görülmektedir:

```console
Forwarding from 127.0.0.1:20001 -> 20001
Forwarding from [::1]:20001 -> 20001
```

Artık Kiali Service kafesobservability panosuna istemci makinenizde aşağıdaki URL 'ye ulaşabilirsiniz- [http://localhost:20001/kiali/console/](http://localhost:20001/kiali/console/). İstendiğinde Kiali gizli dizisi aracılığıyla oluşturduğunuz kimlik bilgilerini kullanmayı unutmayın.

## <a name="uninstall-istio-from-aks"></a>AKS 'ten Istio 'dan kaldırma

> [!WARNING]
> Çalışan bir sistemden Istio 'un silinmesi, hizmetleriniz arasında trafik ile ilgili sorunlar oluşmasına neden olabilir. Devam etmeden önce sisteminizin ilerlemeden düzgün şekilde çalışmaya yönelik hükümler oluşturduğunuzdan emin olun.

### <a name="remove-istio-components-and-namespace"></a>Istio bileşenlerini ve ad alanını kaldır

AKS kümenizdeki ICU 'yi kaldırmak için aşağıdaki komutları kullanın. @No__t-0 komutları, `istio` ve `istio-init` grafiklerini kaldırır ve `kubectl delete ns` komutu `istio-system` ad alanını kaldırır.

```azurecli
helm delete --purge istio
helm delete --purge istio-init
kubectl delete ns istio-system
```

### <a name="remove-istio-crds"></a>Istio CRD 'leri kaldırma

Yukarıdaki komutlar tüm Istio bileşenlerini ve ad alanını siler, ancak yine de Istil CRD 'leri kullanmaya devam ediyoruz. CRDs 'yi silmek için aşağıdaki yaklaşımlardan birini kullanabilirsiniz.

Yaklaşım #1-Bu komut, bu adımı, ' nin yüklü ve çıkarılan sürümünün en üst düzey klasöründen, ile birlikte yüklemek için kullandığınız varsayılmaktadır.

```azure-cli
kubectl delete -f install/kubernetes/helm/istio-init/files
```

Yaklaşım #2-Bu komutlardan birini kullanarak, ' ın birlikte bulunan ve ' nin yüklenmiş ve çıkarılan sürümüne artık erişiminiz yoksa, bu komutlardan birini kullanın. Bu komutun tamamlanması biraz daha uzun sürer. Bu işlem birkaç dakika sürer.

Bash
```bash
kubectl get crds -o name | grep 'istio.io' | xargs -n1 kubectl delete
```

Powershell
```powershell
kubectl get crds -o name | Select-String -Pattern 'istio.io' |% { kubectl delete $_ }
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki belgelerde, bir kanarya yayını kullanıma sunmak üzere akıllı yönlendirme sağlamak için istio 'yu nasıl kullanabileceğiniz açıklanmaktadır:

> [!div class="nextstepaction"]
> [AKS Istio akıllı yönlendirme senaryosu][istio-scenario-routing]

Istio 'ın daha fazla yükleme ve yapılandırma seçeneklerini araştırmak için aşağıdaki resmi Istio makalelerine bakın:

- [İstio-Helm Yükleme Kılavuzu][istio-install-helm]
- [İstio-Helm yükleme seçenekleri][istio-install-helm-options]

Ayrıca, [Istio BookInfo uygulama örneğini][istio-bookinfo-example]kullanarak ek senaryolar da izleyebilirsiniz.

Application Insights ve Istio kullanarak AKS uygulamanızı nasıl izleyeceğinizi öğrenmek için aşağıdaki Azure Izleyici belgelerine bakın:
- [Kubernetes barındırılan uygulamalar için sıfır izleme uygulaması izleme][app-insights]

<!-- LINKS - external -->
[istio]: https://istio.io
[helm]: https://helm.sh

[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-github]: https://github.com/istio/istio
[istio-github-releases]: https://github.com/istio/istio/releases
[istio-release-notes]: https://istio.io/news/
[istio-install-download]: https://istio.io/docs/setup/kubernetes/download-release/
[istio-install-helm]: https://istio.io/docs/setup/kubernetes/install/helm/
[istio-install-helm-options]: https://istio.io/docs/reference/config/installation-options/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/
[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10

[kubernetes-crd]: https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions
[kubernetes-jobs]: https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/
[kubernetes-secrets]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward

[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/
[jaeger]: https://www.jaegertracing.io/
[kiali]: https://www.kiali.io/

[app-insights]: https://docs.microsoft.com/azure/azure-monitor/app/kubernetes

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-scenario-routing]: ./istio-scenario-routing.md
[helm-install]: ./kubernetes-helm.md
