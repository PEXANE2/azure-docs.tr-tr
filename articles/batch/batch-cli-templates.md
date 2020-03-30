---
title: Şablonları kullanarak işleri uçlardan uca çalıştırma - Azure Toplu İş
description: Yalnızca CLI komutlarıyla bir havuz oluşturabilir, giriş verilerini yükleyebilir, iş ve ilişkili görevler oluşturabilir ve elde edilen çıktı verilerini indirebilirsiniz.
services: batch
author: LauraBrenner
manager: evansma
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.workload: big-compute
ms.date: 12/07/2018
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: c7459c4dc700f034feafbf133b831a52b9233d11
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77020174"
---
# <a name="use-azure-batch-cli-templates-and-file-transfer"></a>Azure Toplu CLI şablonlarını ve dosya aktarımlarını kullanma

Azure CLI'deki Azure Toplu İşlem uzantısını kullanarak, toplu işlerini kod yazmadan çalıştırmak mümkündür.

Toplu iş havuzları, işler ve görevler oluşturmak için Azure CLI ile JSON şablon dosyaları oluşturun ve kullanın. Toplu Iş hesabıyla ilişkili depolama hesabına iş giriş dosyalarını kolayca yüklemek ve iş çıktısı dosyalarını indirmek için CLI uzantı komutlarını kullanın.

## <a name="overview"></a>Genel Bakış

Azure CLI'nin bir uzantısı, Toplu İşlem'in geliştirici olmayan kullanıcılar tarafından uçlardan uca kullanılmasını sağlar. Yalnızca CLI komutlarıyla bir havuz oluşturabilir, giriş verilerini yükleyebilir, iş ve ilişkili görevler oluşturabilir ve elde edilen çıktı verilerini indirebilirsiniz. Ek kod gerekmez. CLI komutlarını doğrudan çalıştırın veya komut dosyalarına entegre edin.

Toplu iş şablonları, havuzlar, işler, görevler ve diğer öğeler oluştururken özellik değerlerini belirtmek için JSON dosyaları için [Azure CLI'deki varolan Toplu Iş desteğine](batch-cli-get-started.md#json-files-for-resource-creation) dayanıyor. Toplu iş şablonları aşağıdaki özellikleri ekler:

-   Parametreler tanımlanabilir. Şablon kullanıldığında, öğeyi oluşturmak için yalnızca parametre değerleri belirtilir ve şablon gövdesinde diğer öğe özelliği değerleri belirtilir. Toplu İşlem'i ve Toplu Iş tarafından çalıştırılacak uygulamaları anlayan bir kullanıcı, havuz, iş ve görev özelliği değerlerini belirterek şablonlar oluşturabilir. Toplu İşlem ve/veya uygulamalara daha az aşina olan bir kullanıcının yalnızca tanımlanan parametrelerin değerlerini belirtmesi gerekir.

-   İş görevi fabrikaları, birçok görev tanımının oluşturulması gereğini önleyerek ve iş gönderimini önemli ölçüde basitleştirerek bir işile ilişkili bir veya daha fazla görev oluşturur.


İşler genellikle giriş veri dosyalarını kullanır ve çıktı veri dosyaları üretir. Bir depolama hesabı, varsayılan olarak her Toplu Iş hesabıyla ilişkilidir. CLI'yi kullanarak, kodlama ve depolama kimlik bilgileri olmadan dosyaları bu depolama hesabına aktarın.

Örneğin, [ffmpeg](https://ffmpeg.org/) ses ve video dosyalarını işleyen popüler bir uygulamadır. Kaynak video dosyalarını farklı çözünürlüklere aktarmaya çağırmak için Azure Toplu İşlem CLI ile ilgili adımlar aşağıda veda edebilirsiniz.

-   Havuz şablonu oluşturun. Şablonu oluşturan kullanıcı ffmpeg uygulamasını ve gereksinimlerini nasıl arayacağını bilir; uygun işletim sistemi, VM boyutu, ffmpeg'in nasıl yüklenir (örneğin bir uygulama paketinden veya paket yöneticisi kullanarak) ve diğer havuz özellik değerlerini belirtirler. Parametreler, şablon kullanıldığında yalnızca havuz kimliği ve VM sayısının belirtilmesi için oluşturulur.

-   Bir iş şablonu oluşturun. Şablonu oluşturan kullanıcı ffmpeg'in kaynak videoyu farklı bir çözüme aktarmaya nasıl çağrılması gerektiğini bilir ve görev komut satırıbelirtir; ayrıca, giriş dosyası başına gerekli bir görev ile kaynak video dosyalarını içeren bir klasör olduğunu biliyorum.

-   Transcode için video dosyaları kümesi olan bir son kullanıcı ilk havuz şablonu kullanarak bir havuz oluşturur, yalnızca havuz kimliği ve gerekli VM sayısını belirterek. Daha sonra kaynak dosyaları transcode yükleyebilirsiniz. Daha sonra bir iş, yalnızca havuz kimliğini ve yüklenen kaynak dosyaların konumunu belirterek iş şablonu kullanılarak gönderilebilir. Toplu İşlem, giriş dosyası başına bir görev oluşturulduğu için oluşturulur. Son olarak, kodlanmış çıktı dosyaları indirilebilir.

## <a name="installation"></a>Yükleme

Azure Toplu İşlem CLI uzantısını yüklemek için önce [Azure CLI 2.0'ı yükleyin](/cli/azure/install-azure-cli)veya Azure CLI'yi [Azure Bulut BulutU'nda](../cloud-shell/overview.md)çalıştırın.

Aşağıdaki Azure CLI komutunu kullanarak Toplu İş uzantısının en son sürümünü yükleyin:

```azurecli
az extension add --name azure-batch-cli-extensions
```

Toplu İşlem CLI uzantısı ve ek yükleme seçenekleri hakkında daha fazla bilgi için [GitHub repo'ya](https://github.com/Azure/azure-batch-cli-extensions)bakın.


CLI uzantı özelliklerini kullanmak için bir Azure Toplu İş hesabı ve bağlantılı bir depolama hesabı olan depolama alanına ve depolamadan dosya aktarAn komutlar için gerekir.

Azure CLI ile Toplu İş hesabında oturum açmak için Azure [CLI ile Toplu İş kaynaklarını yönet'e](batch-cli-get-started.md)bakın.

## <a name="templates"></a>Şablonlar

Azure Toplu İş şablonları, işlevsellik ve sözdiziminde Azure Kaynak Yöneticisi şablonlarına benzer. Bunlar, öğe özelliği adları ve değerleri içeren, ancak aşağıdaki ana kavramları eklemek JSON dosyalarıdır:

-   **Parametreler**

    -   Şablon kullanıldığında yalnızca parametre değerlerinin sağlanması gereken bir gövde bölümünde özellik değerlerinin belirtilmesine izin verin. Örneğin, bir havuz için tam tanım gövdeye yerleştirilebilir ve havuz kimliği için tanımlanan yalnızca bir parametre; bu nedenle havuz oluşturmak için yalnızca bir havuz kimliği dizesi sağlanmalıdır.
        
    -   Şablon gövdesi Toplu iş bilgisine ve Toplu Iş tarafından çalıştırılacak uygulamalara sahip biri tarafından yazılabilir; şablon kullanıldığında yalnızca yazar tarafından tanımlanan parametreleriçin değerler sağlanmalıdır. Bu nedenle, ayrıntılı Toplu İşlem ve/veya uygulama bilgisine olmayan bir kullanıcı şablonları kullanabilir.

-   **Değişkenler**

    -   Basit veya karmaşık parametre değerlerinin tek bir yerde belirtilmesine ve şablon gövdesinde bir veya daha fazla yerde kullanılmasına izin verin. Değişkenler, şablonun boyutunu basitleştirebilir ve küçültebilir ve özellikleri değiştirmek için tek bir konuma sahip olarak şablonu daha koruyabilir hale getirebilir.

-   **Üst düzey yapılar**

    -   Toplu İşlem API'lerinde henüz bulunmayan şablonda bazı üst düzey yapılar mevcuttur. Örneğin, bir görev fabrikası, ortak bir görev tanımı kullanılarak iş için birden çok görev oluşturan bir iş şablonunda tanımlanabilir. Bu yapılar, görev başına bir dosya gibi birden çok JSON dosyası oluşturmak ve bir paket yöneticisi aracılığıyla uygulamaları yüklemek için komut dosyası dosyaları oluşturmak için kod oluşturma gereksinimini önler.

    -   Bir noktada, bu yapılar Toplu İşlem hizmetine eklenebilir ve Toplu İş, UI,.

### <a name="pool-templates"></a>Havuz şablonları

Havuz şablonları parametrelerin ve değişkenlerin standart şablon özelliklerini destekler. Ayrıca aşağıdaki üst düzey yapıyı desteklerler:

-   **Paket başvuruları**

    -   İsteğe bağlı olarak, paket yöneticileri kullanılarak yazılımın havuz düğümlerine kopyalanmasını sağlar. Paket yöneticisi ve paket kimliği belirtilir. Bir veya daha fazla paket beyan ederek, gerekli paketleri alan bir komut dosyası oluşturmaktan, komut dosyasını yüklemekten ve komut dosyasını her havuz düğümünde çalıştırmaktan kaçınırsınız.

Aşağıda ffmpeg yüklü Linux VM havuzu oluşturan bir şablon örneği verilmiştir. Kullanmak için yalnızca bir havuz kimliği dizesi ve havuzdaki VM sayısını sağlayabilir:

```json
{
    "parameters": {
        "nodeCount": {
            "type": "int",
            "metadata": {
                "description": "The number of pool nodes"
            }
        },
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "The pool ID "
            }
        }
    },
    "pool": {
        "type": "Microsoft.Batch/batchAccounts/pools",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "[parameters('poolId')]",
            "virtualMachineConfiguration": {
                "imageReference": {
                    "publisher": "Canonical",
                    "offer": "UbuntuServer",
                    "sku": "16.04-LTS",
                    "version": "latest"
                },
                "nodeAgentSKUId": "batch.node.ubuntu 16.04"
            },
            "vmSize": "STANDARD_D3_V2",
            "targetDedicatedNodes": "[parameters('nodeCount')]",
            "enableAutoScale": false,
            "maxTasksPerNode": 1,
            "packageReferences": [
                {
                    "type": "aptPackage",
                    "id": "ffmpeg"
                }
            ]
        }
    }
}
```

Şablon dosyası _pool-ffmpeg.json_olarak adlandırıldıysa, şablonu aşağıdaki gibi çağırın:

```azurecli
az batch pool create --template pool-ffmpeg.json
```

CLI, parametreler `poolId` ve `nodeCount` değerler sağlamanızı ister. Parametreleri json dosyasında da sağlayabilirsiniz. Örnek:

```json
{
  "poolId": {
    "value": "mypool"
  },
  "nodeCount": {
    "value": 2
  }
}
```

Parametreler JSON dosyası *pool-parameters.json*olarak adlandırıldıysa, şablonu aşağıdaki gibi çağırır:

```azurecli
az batch pool create --template pool-ffmpeg.json --parameters pool-parameters.json
```

### <a name="job-templates"></a>İş şablonları

İş şablonları parametrelerin ve değişkenlerin standart şablon özelliklerini destekler. Ayrıca aşağıdaki üst düzey yapıyı desteklerler:

-   **Görev fabrikası**

    -   Bir görev tanımından bir iş için birden çok görev oluşturur. Parametrik süpürme, dosya başına görev ve görev toplama olmak üzere üç tür görev fabrikası desteklenir.

Aşağıda, FFMpeg ile MP4 video dosyalarını iki alt çözünürlükten birine aktarma işi oluşturan bir şablon örneği verilmiştir. Kaynak video dosyası başına bir görev oluşturur. İş girişi ve çıktısı için dosya grupları hakkında daha fazla şey için dosya gruplarına ve [dosya aktarımına](#file-groups-and-file-transfer) bakın.

```json
{
    "parameters": {
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "The name of Azure Batch pool which runs the job"
            }
        },
        "jobId": {
            "type": "string",
            "metadata": {
                "description": "The name of Azure Batch job"
            }
        },
        "resolution": {
            "type": "string",
            "defaultValue": "428x240",
            "allowedValues": [
                "428x240",
                "854x480"
            ],
            "metadata": {
                "description": "Target video resolution"
            }
        }
    },
    "job": {
        "type": "Microsoft.Batch/batchAccounts/jobs",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "[parameters('jobId')]",
            "constraints": {
                "maxWallClockTime": "PT5H",
                "maxTaskRetryCount": 1
            },
            "poolInfo": {
                "poolId": "[parameters('poolId')]"
            },
            "taskFactory": {
                "type": "taskPerFile",
                "source": { 
                    "fileGroup": "ffmpeg-input"
                },
                "repeatTask": {
                    "commandLine": "ffmpeg -i {fileName} -y -s [parameters('resolution')] -strict -2 {fileNameWithoutExtension}_[parameters('resolution')].mp4",
                    "resourceFiles": [
                        {
                            "blobSource": "{url}",
                            "filePath": "{fileName}"
                        }
                    ],
                    "outputFiles": [
                        {
                            "filePattern": "{fileNameWithoutExtension}_[parameters('resolution')].mp4",
                            "destination": {
                                "autoStorage": {
                                    "path": "{fileNameWithoutExtension}_[parameters('resolution')].mp4",
                                    "fileGroup": "ffmpeg-output"
                                }
                            },
                            "uploadOptions": {
                                "uploadCondition": "TaskSuccess"
                            }
                        }
                    ]
                }
            },
            "onAllTasksComplete": "terminatejob"
        }
    }
}
```

Şablon dosyası _iş-ffmpeg.json_olarak adlandırıldıysa, şablonu aşağıdaki gibi çağırın:

```azurecli
az batch job create --template job-ffmpeg.json
```

Daha önce olduğu gibi, CLI parametreleri değerleri sağlamak ister. Parametreleri json dosyasında da sağlayabilirsiniz.

### <a name="use-templates-in-batch-explorer"></a>Toplu İş Gezgini'nde şablonları kullanma

Toplu İşlem havuzu veya iş oluşturmak için Toplu Toplu CLI şablonu (eski adıyla BatchLabs) [Toplu Gezgin](https://github.com/Azure/BatchExplorer) masaüstü uygulamasına yükleyebilirsiniz. Toplu Gezgin Galerisi'ndeki önceden tanımlanmış havuz ve iş şablonlarından da seçim yapabilirsiniz.

Şablon yüklemek için:

1. Toplu İş Gezgini'nde **Galeri** > **Yerel şablonlarını**seçin.

2. Yerel bir havuz veya iş şablonu seçin veya sürükleyin ve bırakın.

3. **Bu şablonu kullan'ı**seçin ve ekrandaki istemleri izleyin.

## <a name="file-groups-and-file-transfer"></a>Dosya grupları ve dosya aktarımı

Çoğu iş ve görev giriş dosyaları gerektirir ve çıktı dosyaları üretir. Genellikle, giriş dosyaları ve çıktı dosyaları istemciden düğüme veya düğümden istemciye aktarılır. Azure Toplu İşlem CLI uzantısı dosya aktarımını özetler ve her Toplu Işlem hesabıyla ilişkilendirebileceğiniz depolama hesabını kullanır.

Dosya grubu, Azure depolama hesabında oluşturulan bir kapsayıcıya eşittir. Dosya grubunun alt klasörleri olabilir.

Toplu İşlem CLI uzantısı, istemciden belirli bir dosya grubuna dosya yüklemek ve belirtilen dosya grubundan istemciye dosya indirmek için komutlar sağlar.

```azurecli
az batch file upload --local-path c:\source_videos\*.mp4 
    --file-group ffmpeg-input

az batch file download --file-group ffmpeg-output --local-path
    c:\output_lowres_videos
```

Havuz ve iş şablonları, dosya gruplarında depolanan dosyaların havuz düğümlerine kopyalamak veya havuz düğümlerini bir dosya grubuna geri almak üzere belirtilmesine olanak sağlar. Örneğin, daha önce belirtilen iş şablonunda, dosya grubu *ffmpeg girişi,* transcoding için düğüme kopyalanan kaynak video dosyalarının konumu olarak görev fabrikası için belirtilir. Dosya grubu *ffmpeg çıktısı,* her görevi çalıştıran düğümden kodlanmış çıktı dosyalarının kopyalandığı yerdir.

## <a name="summary"></a>Özet

Şablon ve dosya aktarım desteği şu anda yalnızca Azure CLI'ye eklenmiştir. Amaç, Toplu İş'i kullanabilen kitleyi, araştırmacılar ve BT kullanıcıları gibi Toplu İş API'lerini kullanarak kod geliştirmesi gerekmeyen kullanıcılara genişletmektir. Kodlama olmadan, Azure, Toplu Iş ve Toplu Iş tarafından çalıştırılacak uygulamalar bilgisine sahip kullanıcılar havuz ve iş oluşturma için şablonlar oluşturabilir. Şablon parametreleri ile, Toplu Iş ve uygulamalar hakkında ayrıntılı bilgisi olmayan kullanıcılar şablonları kullanabilir.

Azure CLI için Toplu Iş uzantısını deneyin ve bu makalenin yorumlarında veya [Toplu Topluluk reposu](https://github.com/Azure/Batch)aracılığıyla bize herhangi bir geri bildirim veya öneri sağlayın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure [GitHub repo'sunda](https://github.com/Azure/azure-batch-cli-extensions)ayrıntılı yükleme ve kullanım belgeleri, örnekler ve kaynak kodu mevcuttur.

- Toplu Iş kaynakları oluşturmak ve yönetmek için [Toplu Gezgin'i](https://github.com/Azure/BatchExplorer) kullanma hakkında daha fazla bilgi edinin.
