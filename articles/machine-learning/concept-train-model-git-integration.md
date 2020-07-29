---
title: Azure Machine Learning için git tümleştirmesi
titleSuffix: Azure Machine Learning
description: Azure Machine Learning yerel bir git deposu ile nasıl tümleştireceğinizi öğrenin. Bir git deposu olan yerel bir dizinden bir eğitim çalıştırıldığında, depo, dal ve geçerli işleme bilgileri çalıştırmanın bir parçası olarak izlenir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.date: 03/05/2020
ms.openlocfilehash: 7cc2e346a35cd1cdf1278b527dc451a903d60f89
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78402831"
---
# <a name="git-integration-for-azure-machine-learning"></a>Azure Machine Learning için git tümleştirmesi

[Git](https://git-scm.com/) , projelerinize paylaşmanıza ve işbirliği yapmanıza olanak sağlayan popüler bir sürüm denetim sistemidir. 

İşleri izlemek için Git depolarını tamamen destekler. depoları doğrudan paylaşılan çalışma alanı dosya sisteminize kopyalayabilir, yerel iş istasyonunuzda git kullanabilir veya bir CI/CD işlem hattından git kullanabilirsiniz. Azure Machine Learning

Azure Machine Learning bir iş gönderilirken, kaynak dosyalar yerel bir git deposunda depolanıyorsa, eğitim sürecinin bir parçası olarak depo hakkında bilgi izlenir.

Azure Machine Learning, yerel bir git deposundan bilgileri izlediğinden, belirli bir merkezi depoya bağlı değildir. Deponuz GitHub, GitLab, Bitbucket, Azure DevOps veya başka bir git uyumlu hizmetten kopyalanabilir.

## <a name="clone-git-repositories-into-your-workspace-file-system"></a>Git depolarını çalışma alanı dosya sisteminize kopyalama
Azure Machine Learning, çalışma alanındaki tüm kullanıcılar için paylaşılan bir dosya sistemi sağlar.
Bir Git deposunu bu dosya paylaşımında kopyalamak için bir terminal açmak & bir Işlem örneği oluşturmanız önerilir.
Terminal açıldıktan sonra tam bir git istemcisine erişiminiz olur ve git CLı deneyimi aracılığıyla Git 'i kopyalayabilir ve bunlarla çalışabilirsiniz.

Başkalarının doğrudan çalışma dalınızda çakışmalar yapamaması için depoyu kullanıcılar dizininize kopyalamanız önerilir.

Kimlik doğrulayabilecek herhangi bir Git deposunu (GitHub, Azure Repos, BitBucket vb.) kopyalayabilirsiniz.

Git CLı 'nın nasıl kullanılacağına ilişkin bir kılavuz için [burada buradan okuyun.](https://guides.github.com/introduction/git-handbook/)

## <a name="track-code-that-comes-from-git-repositories"></a>Git depolarından gelen kodu izleme

Python SDK 'dan veya CLı Machine Learning bir eğitim çalıştırması gönderdiğinizde, modeli eğitmek için gereken dosyalar çalışma alanınıza yüklenir. `git`Komut geliştirme ortamınızda kullanılabiliyorsa, karşıya yükleme işlemi, dosyaların bir git deposunda saklanıp saklanmadığını denetlemek için onu kullanır. Bu durumda, git deponuzdaki bilgiler, eğitim çalıştırmasının bir parçası olarak da yüklenir. Bu bilgiler, eğitim çalışması için aşağıdaki özelliklerde depolanır:

| Özellik | Değeri almak için kullanılan git komutu | Açıklama |
| ----- | ----- | ----- |
| `azureml.git.repository_uri` | `git ls-remote --get-url` | Deponuzdan kopyalanan URI. |
| `mlflow.source.git.repoURL` | `git ls-remote --get-url` | Deponuzdan kopyalanan URI. |
| `azureml.git.branch` | `git symbolic-ref --short HEAD` | Çalışma gönderildiğinde etkin dal. |
| `mlflow.source.git.branch` | `git symbolic-ref --short HEAD` | Çalışma gönderildiğinde etkin dal. |
| `azureml.git.commit` | `git rev-parse HEAD` | Çalıştırma için gönderilen kodun COMMIT karması. |
| `mlflow.source.git.commit` | `git rev-parse HEAD` | Çalıştırma için gönderilen kodun COMMIT karması. |
| `azureml.git.dirty` | `git status --porcelain .` | `True`dal/COMMIT kirli ise; Aksi takdirde, `false` . |

Bu bilgiler, bir Estimator, Machine Learning işlem hattı veya betik çalıştırması kullanan çalıştırmalar için gönderilir.

Eğitim dosyalarınız geliştirme ortamınızda bir git deposunda bulunmuyorsa veya `git` komut kullanılabilir değilse, git ile ilgili bilgiler izlenmez.

> [!TIP]
> Git komutunun geliştirme ortamınızda kullanılabilir olup olmadığını denetlemek için bir kabuk oturumu, komut istemi, PowerShell veya başka bir komut satırı arabirimi açın ve aşağıdaki komutu yazın:
>
> ```
> git --version
> ```
>
> Yüklüyse ve yolunda şuna benzer bir yanıt alırsınız `git version 2.4.1` . Geliştirme ortamınıza git 'i yükleme hakkında daha fazla bilgi için [Git Web sitesine](https://git-scm.com/)bakın.

## <a name="view-the-logged-information"></a>Günlüğe kaydedilen bilgileri görüntüleyin

Git bilgileri bir eğitim çalışmasının özelliklerinde depolanır. Azure portal, Python SDK ve CLı kullanarak bu bilgileri görüntüleyebilirsiniz. 

### <a name="azure-portal"></a>Azure portal

1. [Azure Portal](https://portal.azure.com), çalışma alanınızı seçin.
1. __Denemeleri__öğesini seçin ve denemeleri birini seçin.
1. __Çalıştırma numarası__ sütunundan çalıştırmaların birini seçin.
1. __Günlükler__' i seçin ve ardından __Günlükler__ ve __azureml__ girdilerini genişletin. __ ### \_ Azure__ile başlayan bağlantıyı seçin.

    ![Portalda # # #_azure girişi](./media/concept-train-model-git-integration/azure-machine-learning-logs.png)

Günlüğe kaydedilen bilgiler aşağıdaki JSON 'a benzer bir metin içerir:

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

Bir eğitim çalıştırması gönderdikten sonra bir [çalıştırma](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) nesnesi döndürülür. `properties`Bu nesnenin özniteliği, günlüğe kaydedilen git bilgilerini içerir. Örneğin, aşağıdaki kod, COMMIT karmasını alır:

```python
run.properties['azureml.git.commit']
```

### <a name="cli"></a>CLI

`az ml run`CLI komutu, bir çalıştırmanın özelliklerini almak için kullanılabilir. Örneğin, aşağıdaki komut adlı deneyde son çalıştırmanın özelliklerini döndürür `train-on-amlcompute` :

```azurecli-interactive
az ml run list -e train-on-amlcompute --last 1 -w myworkspace -g myresourcegroup --query '[].properties'
```

Daha fazla bilgi için, [az ml Run](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest) Reference belgelerine bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Model eğitimi için işlem hedeflerini ayarlama ve kullanma](how-to-set-up-training-targets.md)
