---
title: Azure Machine Learning için Git entegrasyonu
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'in yerel bir Git deposuyla nasıl entegre olduğunu öğrenin. Git deposu olan yerel bir dizinden bir eğitim çalıştırırken, repo, şube ve geçerli işleme hakkındaki bilgiler çalıştırmanın bir parçası olarak izlenir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.date: 03/05/2020
ms.openlocfilehash: 7cc2e346a35cd1cdf1278b527dc451a903d60f89
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78402831"
---
# <a name="git-integration-for-azure-machine-learning"></a>Azure Machine Learning için Git entegrasyonu

[Git,](https://git-scm.com/) projelerinizle paylaşmanıza ve işbirliği yapmanızı sağlayan popüler bir sürüm kontrol sistemidir. 

Azure Machine Learning, çalışmaları izlemek için Git depolarını tam olarak destekler - depoları doğrudan paylaşılan çalışma alanı dosya sisteminize klonlayabilir, yerel iş istasyonunuzda Git'i kullanabilir veya Bir CI/CD ardışık kanalından Git'i kullanabilirsiniz.

Azure Machine Learning'e iş gönderirken, kaynak dosyalar yerel bir git deposunda depolanırsa, repo hakkındaki bilgiler eğitim sürecinin bir parçası olarak izlenir.

Azure Machine Learning yerel bir git repo'daki bilgileri izlediğinden, belirli bir merkezi depoya bağlı değildir. Deponuz GitHub, GitLab, Bitbucket, Azure DevOps veya git uyumlu başka herhangi bir hizmetten klonlanabilir.

## <a name="clone-git-repositories-into-your-workspace-file-system"></a>Çalışma alanı dosya sisteminize Klon Git depoları
Azure Machine Learning, çalışma alanındaki tüm kullanıcılar için paylaşılan bir dosya sistemi sağlar.
Bu dosya paylaşımında bir Git deposunu klonlamak için, bir terminalaçmak & bir Bilgi İşlem Örneği oluşturmanızı öneririz.
Terminal açıldıktan sonra, tam bir Git istemcisine erişebilirsiniz ve Git CLI deneyimi aracılığıyla git ile klonlayabilir ve çalışabilirsiniz.

Başkalarının doğrudan çalışma dalınıza çakışmasın diye depoyu kullanıcı dizininize klonlamanızı öneririz.

Doğruluğunu doğrulayabildiğiniz herhangi bir Git deposunu klonlayabilirsiniz (GitHub, Azure Repos, BitBucket, vb.)

Git CLI nasıl kullanılacağı hakkında bir rehber için, [burada](https://guides.github.com/introduction/git-handbook/)okuyun.

## <a name="track-code-that-comes-from-git-repositories"></a>Git depolarından gelen izleme kodu

Python SDK veya Machine Learning CLI'den bir eğitim çalışması gönderdiğinizde, modeli eğitmek için gereken dosyalar çalışma alanınıza yüklenir. `git` Komut geliştirme ortamınızda varsa, yükleme işlemi dosyaların git deposunda depolanmış olup olmadığını denetlemek için kullanır. Eğer öyleyse, git deposundan gelen bilgiler de eğitim çalışmasının bir parçası olarak yüklenir. Bu bilgiler, eğitim çalışması için aşağıdaki özelliklerde depolanır:

| Özellik | Değeri almak için kullanılan Git komutu | Açıklama |
| ----- | ----- | ----- |
| `azureml.git.repository_uri` | `git ls-remote --get-url` | Deponuzun klonlandığınız URI. |
| `mlflow.source.git.repoURL` | `git ls-remote --get-url` | Deponuzun klonlandığınız URI. |
| `azureml.git.branch` | `git symbolic-ref --short HEAD` | Çalıştırma gönderildiğinde etkin dal. |
| `mlflow.source.git.branch` | `git symbolic-ref --short HEAD` | Çalıştırma gönderildiğinde etkin dal. |
| `azureml.git.commit` | `git rev-parse HEAD` | Çalıştırma için gönderilen kodun işleyen karma. |
| `mlflow.source.git.commit` | `git rev-parse HEAD` | Çalıştırma için gönderilen kodun işleyen karma. |
| `azureml.git.dirty` | `git status --porcelain .` | `True`, şube/taahhüt kirliise; aksi `false`takdirde, . |

Bu bilgiler, tahminci, makine öğrenimi ardışık hattı veya komut dosyası çalıştırıcı kullanan çalıştırmalar için gönderilir.

Eğitim dosyalarınız geliştirme ortamınızla ilgili bir git deposunda bulunmuyorsa veya `git` komut kullanılamıyorsa, git ile ilgili hiçbir bilgi izlenmez.

> [!TIP]
> Geliştirme ortamınızda git komutu kullanılabilir olup olmadığını kontrol etmek için bir kabuk oturumu, komut istemi, PowerShell veya diğer komut satırı arabirimi açın ve aşağıdaki komutu yazın:
>
> ```
> git --version
> ```
>
> Yüklüyse ve yolda, `git version 2.4.1`' a benzer bir yanıt alırsınız. Geliştirme ortamınıza git yükleme hakkında daha fazla bilgi için [Git web sitesine](https://git-scm.com/)bakın.

## <a name="view-the-logged-information"></a>Günlüğe kaydedilen bilgileri görüntüleme

Git bilgileri, bir eğitim çalışması için özelliklerde depolanır. Bu bilgileri Azure portalı, Python SDK ve CLI'yi kullanarak görüntüleyebilirsiniz. 

### <a name="azure-portal"></a>Azure portalında

1. Azure [portalından](https://portal.azure.com)çalışma alanınızı seçin.
1. __Denemeler'i__seçin ve ardından denemelerinizden birini seçin.
1. __RUN NUMBER__ sütunundan bir çalıştırma seçin.
1. __Günlükleri__seçin ve ardından günlükleri ve __azureml__ __girişlerini__ genişletin. __ ### \_Azure__ile başlayan bağlantıyı seçin.

    ![Portaldaki ###_azure girişi](./media/concept-train-model-git-integration/azure-machine-learning-logs.png)

Günlüğe kaydedilen bilgiler aşağıdaki JSON'a benzer bir metin içerir:

```json
"properties": {
    "_azureml.ComputeTargetType": "batchai",
    "ContentSnapshotId": "5ca66406-cbac-4d7d-bc95-f5a51dd3e57e",
    "azureml.git.repository_uri": "git@github.com:azure/machinelearningnotebooks",
    "mlflow.source.git.repoURL": "git@github.com:azure/machinelearningnotebooks",
    "azureml.git.branch": "master",
    "mlflow.source.git.branch": "master",
    "azureml.git.commit": "4d2b93784676893f8e346d5f0b9fb894a9cf0742",
    "mlflow.source.git.commit": "4d2b93784676893f8e346d5f0b9fb894a9cf0742",
    "azureml.git.dirty": "True",
    "AzureML.DerivedImageName": "azureml/azureml_9d3568242c6bfef9631879915768deaf",
    "ProcessInfoFile": "azureml-logs/process_info.json",
    "ProcessStatusFile": "azureml-logs/process_status.json"
}
```

### <a name="python-sdk"></a>Python SDK'sı

Bir eğitim çalışması gönderdikten sonra, [Bir Çalıştır](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) nesnesi döndürülür. `properties` Bu nesnenin özniteliği günlüğe kaydedilmiş git bilgilerini içerir. Örneğin, aşağıdaki kod işleyiş karmasını alır:

```python
run.properties['azureml.git.commit']
```

### <a name="cli"></a>CLI

`az ml run` CLI komutu, bir çalışmadan özellikleri almak için kullanılabilir. Örneğin, aşağıdaki komut, aşağıdaki denemede son çalıştırmanın `train-on-amlcompute`özelliklerini döndürür:

```azurecli-interactive
az ml run list -e train-on-amlcompute --last 1 -w myworkspace -g myresourcegroup --query '[].properties'
```

Daha fazla bilgi için [az ml çalıştır referans](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest) belgelerine bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Model eğitimi için işlem hedeflerini ayarlama ve kullanma](how-to-set-up-training-targets.md)
