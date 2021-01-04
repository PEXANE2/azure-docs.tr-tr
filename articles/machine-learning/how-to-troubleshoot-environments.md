---
title: Ortam görüntülerinin sorunlarını giderme
titleSuffix: Azure Machine Learning
description: Ortam görüntüsü derlemeleri ve paket yüklemeleri ile ilgili sorunları nasıl giderebileceğinizi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
author: saachigopal
ms.author: sagopal
ms.date: 12/3/2020
ms.topic: troubleshooting
ms.custom: devx-track-python
ms.openlocfilehash: b452c24b4b2ed6021910f267b9941f3829acccd8
ms.sourcegitcommit: a89a517622a3886b3a44ed42839d41a301c786e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2020
ms.locfileid: "97733399"
---
# <a name="troubleshoot-environment-image-builds"></a>Ortam görüntüsü Derlemeleriyle ilgili sorunları giderme
Docker ortam görüntü yapıları ve paket yüklemeleri ile ilgili sorunları nasıl giderebileceğinizi öğrenin.

## <a name="prerequisites"></a>Önkoşullar

* Bir **Azure aboneliği**. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree)deneyin.
* [Azure MACHINE LEARNING SDK](/python/api/overview/azure/ml/install?preserve-view=true&view=azure-ml-py).
* [Azure CLI](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest).
* [Azure Machine Learning Için CLI uzantısı](reference-azure-machine-learning-cli.md).
* Yerel olarak hata ayıklamak için yerel sisteminizde çalışan bir Docker yüklemeniz olmalıdır.

## <a name="docker-image-build-failures"></a>Docker görüntü oluşturma sorunları
 
Görüntü derleme hatalarının çoğunluğu için, temel neden görüntü derleme günlüğünde bulunabilir.
Görüntü derleme günlüğünü Azure Machine Learning portalından (20 \_ görüntü \_ Derleme \_log.txt) veya ACR görevinizden çalıştırma günlüklerinden bulabilirsiniz
 
Çoğu durumda, hataları yerel olarak yeniden oluşturmak daha kolaydır. Hata türünü denetleyip aşağıdakilerden birini deneyin `setuptools` :

- Conda bağımlılığını yerel olarak Install `conda install suspicious-dependency==X.Y.Z`
- PIP bağımlılığını yerel olarak yükler `pip install suspicious-dependency==X.Y.Z`
- Tüm ortamı çalıştırmayı deneyin `conda create -f conda-specification.yml`

> [!IMPORTANT]
> Yerel hesaplamanıza ait platform ve yorumlayıcı 'nın uzak ortamda olanlarla eşleştiğinden emin olun. 

### <a name="timeout"></a>Zaman aşımı 
 
Çeşitli ağ sorunları için zaman aşımı sorunları oluşabilir:
- Düşük internet bant genişliği
- Sunucu sorunları
- Verilen Conda veya PIP zaman aşımı ayarlarıyla indirilebilen büyük bağımlılık
 
Aşağıdakine benzer iletiler bu sorunu gösterir:
 
```
('Connection broken: OSError("(104, \'ECONNRESET\')")', OSError("(104, 'ECONNRESET')"))
```
```
ReadTimeoutError("HTTPSConnectionPool(host='****', port=443): Read timed out. (read timeout=15)",)
```

Olası çözümler:
 
- Yansıma, BLOB depolama veya diğer Python akışları gibi kullanılabilir olduğunda bağımlılık için farklı bir kaynak deneyin.
- Conda veya PIP 'yi güncelleştirin. Özel bir Docker dosyası kullanılıyorsa, zaman aşımı ayarlarını güncelleştirin.
- Bazı PIP sürümlerinde bilinen sorunlar var. Ortam bağımlılıklarına belirli bir PIN sürümü eklemeyi göz önünde bulundurun.

### <a name="package-not-found"></a>Paket bulunamadı

Bu, görüntü derleme hatalarının en yaygın durumdur.

- Conda paketi bulunamadı
        ```
        ResolvePackageNotFound: 
          - not-existing-conda-package
        ```

- Belirtilen PIP paketi veya sürümü bulunamadı
        ```
        ERROR: Could not find a version that satisfies the requirement invalid-pip-package (from versions: none)
        ERROR: No matching distribution found for invalid-pip-package
        ```

- Hatalı iç içe nokta bağımlılığı
        ```
        ERROR: No matching distribution found for bad-backage==0.0 (from good-package==1.0)
        ```

Belirtilen kaynaklarda paketin var olduğunu denetleyin. PIP bağımlılıklarını doğrulamak için [PIP aramasını](https://pip.pypa.io/en/stable/reference/pip_search/) kullanın.

`pip search azureml-core`

Conda bağımlılıkları için [Conda Search](https://docs.conda.io/projects/conda/en/latest/commands/search.html)' u kullanın.

`conda search conda-forge::numpy`

Daha fazla seçenek için:
- `pip search -h`
- `conda search -h`

#### <a name="installer-notes"></a>Yükleyici notları

Gerekli dağıtımın belirtilen platform ve Python yorumlayıcı sürümü için mevcut olduğundan emin olun.

PIP bağımlılıkları için, `https://pypi.org/project/[PROJECT NAME]/[VERSION]/#files` gerekli sürümün kullanılabilir olup olmadığını görmek için bölümüne gidin. Örneğin, https://pypi.org/project/azureml-core/1.11.0/#files

Conda bağımlılıkları için kanal deposundaki paketi denetleyin.
Anaconda, Inc. tarafından tutulan kanallar için [buraya](https://repo.anaconda.com/pkgs/)bakın.

### <a name="pip-package-update"></a>PIP paketi güncelleştirmesi

Bir PIP paketinin yüklenmesi veya güncelleştirilmesi sırasında, çözümleyici 'nin, yeni gereksinimleri karşılamak için önceden yüklenmiş bir paketi güncelleştirmesi gerekebilir.
PIP sürümü veya bağımlılığın yüklenme yöntemi ile ilgili çeşitli nedenlerle kaldırma işlemi başarısız olabilir.
En yaygın senaryo, Conda tarafından yüklenen bir bağımlılığın PIP tarafından kaldırılamayacağı bir yöntemdir.
Bu senaryo için, kullanarak bağımlılığı kaldırmayı göz önünde bulundurun `conda remove mypackage` .

```
  Attempting uninstall: mypackage
    Found existing installation: mypackage X.Y.Z
ERROR: Cannot uninstall 'mypackage'. It is a distutils installed project and thus we cannot accurately determine which files belong to it which would lead to only a partial uninstall.
``` 
### <a name="installer-issues"></a>Yükleyici sorunları

Bazı yükleyici sürümlerinde, paket çözümleyicilerine bir yapı hatasına yol açabilecek sorunlar vardır.

Özel bir temel görüntü veya dockerfile kullanılıyorsa, Conda sürümü 4.5.4 veya üstünü kullanmanızı öneririz.

PIP bağımlılıklarını yüklemek için zar paketi gereklidir ve ortamda bir sürüm belirtilmemişse, en son sürüm kullanılacaktır.
Geçici sorunları veya aracın en son sürümü neden olabilecek değişiklikleri önlemek için, PIP 'nin bilinen bir sürümünü kullanmanızı öneririz.

Aşağıdaki iletilerden herhangi birini görürseniz, PIP sürümünü ortamınıza sabitlemeyi göz önünde bulundurun:

`Warning: you have pip-installed dependencies in your environment file, but you do not list pip itself as one of your conda dependencies. Conda may not use the correct pip to install your packages, and they may end up in the wrong place. Please add an explicit pip dependency. I'm adding one for you, but still nagging you.`

`Pip subprocess error:
ERROR: THESE PACKAGES DO NOT MATCH THE HASHES FROM THE REQUIREMENTS FILE. If you have updated the package versions, update the hashes as well. Otherwise, examine the package contents carefully; someone may have tampered with them.`

Ayrıca, bağımlılıklarda çözümlenemeyen çakışmalar varsa, PIP yüklemesi sonsuz bir döngüye takılmış olabilir. Yerel olarak çalışıyorsa, PIP sürümünün < 20,3 ' ye indirgemesini sağlar. YAML dosyasından oluşturulan bir Conda ortamında, bu sorun yalnızca Conda-Forge en yüksek öncelikli kanal olduğunda görülür. Sorunu azaltmak için, Conda belirtim dosyasında bir Conda bağımlılığı olarak < 20,3 (! = 20,3 veya = 20.2.4 PIN 'i diğer sürüme sabitle) açıkça belirtin.

## <a name="service-side-failures"></a>Hizmet tarafı sorunları

### <a name="unable-to-pull-image-from-mcraddress-could-not-be-resolved-for-container-registry"></a>Görüntü, MCR/adres 'den çekilemiyor Container Registry için çözümlenemedi.
Olası sorunlar:
- Kapsayıcı kayıt defterinin yol adı doğru şekilde çözümlenmeyebilir. Görüntü adlarının çift eğik çizgi ve Linux ile Windows konaklarının yönü doğru olduğundan emin olun.
- VNET 'in arkasındaki ACR, [Desteklenmeyen bir bölgede](https://docs.microsoft.com/azure/private-link/private-link-overview#availability)özel bir uç nokta kullanıyorsa, portaldan hizmet uç noktası (genel erişim) kullanarak VNET 'in arkasındaki ACR 'yi yapılandırın ve yeniden deneyin.
- ACR 'yi VNet 'in arkasına geçirdikten sonra [ARM şablonunun](https://docs.microsoft.com/azure/machine-learning/how-to-enable-virtual-network#azure-container-registry) çalıştığından emin olun. Bu, çalışma alanının ACR örneğiyle iletişim kurmasını sağlar.

### <a name="401-error-from-workspace-acr"></a>çalışma alanı ACR 'den 401 hatası
[Ws.sync_keys ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#sync-keys--) kullanarak depolama anahtarlarını yeniden eşitleme

### <a name="environment-keeps-throwing-waiting-for-other-conda-operations-to-finish-error"></a>Ortam, "diğer Conda işlemlerinin bitmesi bekleniyor..." Hatayla
Bir görüntü oluşturma devam ederken, Conda SDK istemcisi tarafından kilitlenir. İşlem kilitlendi veya Kullanıcı tarafından yanlış bir şekilde iptal edilirse, Conda kilitli durumda kalır. Bu sorunu çözmek için kilit dosyasını el ile silin. 

### <a name="custom-docker-image-not-in-registry"></a>Özel Docker görüntüsü kayıt defterinde değil
[Doğru etiketin](https://docs.microsoft.com/azure/machine-learning/how-to-use-environments#create-an-environment) kullanıldığını ve olduğunu denetleyin `user_managed_dependencies = True` . `Environment.python.user_managed_dependencies = True` Conda 'yi devre dışı bırakır ve kullanıcının yüklü paketlerini kullanır.

### <a name="common-vnet-issues"></a>Ortak VNet sorunları

1. Depolama hesabı, işlem kümesi ve Azure Container Registry sanal ağın aynı alt ağında olduğundan emin olun.
2. ACR bir sanal ağın arkasında olduğunda, doğrudan görüntü oluşturmak için kullanılamaz. Görüntü derlemek için işlem kümesinin kullanılması gerekir.
3. Depolama alanı, şu durumlarda bir sanal ağın arkasına yerleştirilmesi gerekebilir:
    - Inor sınırlama veya özel tekerlek kullanma
    - 403 (yetkilendirilmemiş) hizmet hatalarını görme
    - ACR/MCR 'den görüntü ayrıntıları alınamıyor

### <a name="image-build-fails-when-trying-to-access-network-protected-storage"></a>Ağ korumalı depolamaya erişmeye çalışırken resim oluşturma işlemi başarısız oluyor
- ACR görevleri VNet 'in arkasında çalışmaz. Kullanıcının VNet 'in arkasında ACR 'si varsa, bir görüntü oluşturmak için işlem kümesini kullanmaları gerekir.
- Bağımlılıkları bundan sonra alabilmesi için depolama alanı VNet 'in arkasında olmalıdır. 

### <a name="cannot-run-experiments-when-storage-has-network-security-enabled"></a>Depolama alanı ağ güvenliği etkinken denemeleri çalıştırılamaz
Varsayılan Docker görüntülerini kullanırken ve Kullanıcı tarafından yönetilen bağımlılıkları etkinleştirdikten sonra, microsoftcontainerregistry ve azurefrontkapısı. firstparti [hizmeti etiketlerini](https://docs.microsoft.com/azure/machine-learning/how-to-enable-virtual-network) izin MCR ve bağımlılıklarını kullanmanız gerekir.

 Bkz. VNET 'i daha fazla [etkinleştirme](https://docs.microsoft.com/azure/machine-learning/how-to-enable-virtual-network#azure-container-registry) .

### <a name="creating-an-icm"></a>ICM oluşturma

Bir ICM 'yi oluştururken/atarken sorunu daha iyi anlayabilmemiz için lütfen CSS destek anahtarını ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir makine öğrenme modelini, çiçekleri kategorilere ayırmak için eğitme](how-to-train-scikit-learn.md)
- [Özel bir Docker görüntüsü kullanarak makine öğrenimi modelini eğitme](how-to-train-with-custom-image.md)