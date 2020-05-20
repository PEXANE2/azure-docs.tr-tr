---
title: GitHub deponuzu uygulama yapılandırmasıyla eşitleyin
description: GitHub deponuzu güncelleştirdiğinizde uygulama yapılandırma örneğinizi otomatik olarak güncelleştirmek için GitHub eylemlerini kullanın.
author: lisaguthrie
ms.author: lcozzens
ms.date: 02/20/2020
ms.topic: conceptual
ms.service: azure-app-configuration
ms.openlocfilehash: 9cb1149073247b7f5fc3e74a1aef6f96388c7135
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83648122"
---
# <a name="sync-your-github-repository-to-app-configuration"></a>GitHub deponuzu uygulama yapılandırmasıyla eşitleyin

Mevcut kaynak denetimi uygulamalarını kullanmaya devam etmek isteyen takımlar, GitHub deposunu uygulama yapılandırma depolarıyla otomatik olarak eşitlemek için GitHub eylemlerini kullanabilir. Bu, aşağıdaki gibi uygulama yapılandırma avantajlarını alırken yapılandırma dosyalarınızda değişiklik yapmanıza olanak sağlar: <br>
&nbsp;&nbsp;&nbsp;&nbsp;• Kodunuzun dışında merkezi yapılandırma <br>
&nbsp;&nbsp;&nbsp;&nbsp;• Tüm uygulamanızı yeniden dağıtmaya gerek kalmadan yapılandırmayı güncelleştirme <br>
&nbsp;&nbsp;&nbsp;&nbsp;• Azure App Service ve Işlevleri gibi hizmetlerle tümleştirme. 

GitHub eylemleri [iş akışı](https://help.github.com/articles/about-github-actions#workflow) , GitHub deposunda otomatikleştirilmiş bir işlem tanımlar. *Azure uygulama yapılandırma eşitleme* eylemi, kaynak deposunda değişiklik yapıldığında bir uygulama yapılandırma örneği için güncelleştirmeleri tetikler. `/.github/workflows/`Adımları ve parametreleri tanımlamak üzere deponuzun yolunda bulunan BIR YAML (. yıml) dosyasını kullanır. Uygulama kodu ile tıpkı uygulama yapılandırma dosyalarını gönderdiğinizde, gözden geçirirken veya dallandırdığınızda yapılandırma güncelleştirmelerini tetikleyebilirsiniz.

GitHub [belgeleri](https://help.github.com/actions/automating-your-workflow-with-github-actions/configuring-a-workflow) , GitHub iş akışlarının ve eylemlerinin ayrıntılı görünümünü sağlar. 

## <a name="enable-github-actions-in-your-repository"></a>Deponuzda GitHub eylemlerini etkinleştirme
Bu GitHub eylemini kullanmaya başlamak için deponuza gidin ve **Eylemler** sekmesini seçin. **Yeni Iş akışı**' na tıklayın, ardından **bir iş akışını kendiniz ayarlayın**. Son olarak, Market 'te "Azure uygulama yapılandırma eşitlemesi" için arama yapın.
> [!div class="mx-imgBorder"]
> ![Eylem sekmesini seçin](media/find-github-action.png)

> [!div class="mx-imgBorder"]
> ![Uygulama yapılandırma eşitleme eylemini seçin](media/app-configuration-sync-action.png)

## <a name="sync-configuration-files-after-a-push"></a>Bir gönderim sonrasında yapılandırma dosyalarını eşitleme
Bu eylem, bir değişikliğin gönderildiği Azure uygulama yapılandırma dosyalarını eşitler `appsettings.json` . Bir geliştirici bir değişikliği uygulamasına ilettiğinde `appsettings.json` , uygulama yapılandırma eşitleme eylemi, uygulama yapılandırma örneğini yeni değerlerle güncelleştirir.

Bu iş akışının ilk bölümü, eylemin *on* *push* `appsettings.json` *ana* dala sahip bir gönderim üzerinde tetikleyeceğini belirtir. İkinci bölüm, eylem tetiklendiğinde çalıştırılan işleri listeler. Eylem ilgili dosyaları denetler ve depoda gizli dizi olarak depolanan bağlantı dizesini kullanarak uygulama yapılandırma örneğini güncelleştirir.  GitHub 'da gizli dizileri kullanma hakkında daha fazla bilgi için bkz. [GitHub 'ın](https://help.github.com/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets) şifrelenmiş gizli dizileri oluşturma ve kullanma hakkında daha fazla bilgi

```json
on: 
  push: 
    branches: 
      - 'master' 
    paths: 
      - 'appsettings.json' 
 
jobs: 
  syncconfig: 
    runs-on: ubuntu-latest 
    steps: 
      # checkout done so that files in the repo can be read by the sync 
      - uses: actions/checkout@v1 
      - uses: azure/appconfiguration-sync@v1 
        with: 
          configurationFile: 'appsettings.json' 
          format: 'json' 
          # Replace <ConnectionString> with the name of the secret in your                        
          # repository 
          connectionString: ${{ secrets.<ConnectionString> }} 
          separator: ':' 
```

## <a name="use-strict-sync"></a>Katı eşitleme kullan
Varsayılan olarak, GitHub eylemi katı modu etkinleştirmez, yani eşitlemenin yalnızca yapılandırma dosyasından uygulama yapılandırma örneğine anahtar değerleri eklemesi gerekir (anahtar-değer çiftleri silinmez). Katı modu etkinleştirmek, yapılandırma dosyası ile eşleşmesi için uygulama yapılandırma örneğinden silinmeyen anahtar-değer çiftleri anlamına gelir. Birden çok kaynaktan eşitleme yapıyorsanız veya uygulama yapılandırmasıyla Azure Key Vault kullanıyorsanız, diğer dosyalardan yapılandırma ayarlarını silme işleminin önüne geçmek için, katı eşitleme ile farklı ön ekleri veya etiketleri kullanmak isteyeceksiniz (aşağıdaki örneklere bakın). 

```json
on: 
  push: 
    branches: 
      - 'master' 
    paths: 
      - 'appsettings.json' 
 
jobs: 
  syncconfig: 
    runs-on: ubuntu-latest 
    steps: 
      # checkout done so that files in the repo can be read by the sync 
      - uses: actions/checkout@v1 
      - uses: azure/appconfiguration-sync@v1 
        with: 
          configurationFile: 'appsettings.json' 
          format: 'json' 
          # Replace <ConnectionString> with the name of the secret in your 
          # repository 
          connectionString: ${{ secrets.<ConnectionString> }}  
          separator: ':' 
          label: 'Label' 
          prefix: 'Prefix:' 
          strict: true 
```
## <a name="sync-multiple-files-in-one-action"></a>Birden çok dosyayı tek bir eylemde eşitleme 

Yapılandırmanız birden çok dosya içinde ise, herhangi bir dosya değiştirildiğinde eşitleme tetiklemek için aşağıdaki kalıbı kullanabilirsiniz. Bu model, glob kitaplığını kullanırhttps://www.npmjs.com/package/glob 

```json
on:
  push:
    branches:
      - 'master'
    paths:
      - 'appsettings.json'
      - 'appsettings2.json'

jobs:
  syncconfig:
    runs-on: ubuntu-latest
    steps:
      # checkout done so that files in the repo can be read by the sync
      - uses: actions/checkout@v1
      - uses: azure/appconfiguration-sync@v1
        with:
          configurationFile: '{appsettings.json,appsettings2.json}'
          format: 'json'
          # Replace <ConnectionString> with the name of the secret in your repository
          connectionString: ${{ secrets.<ConnectionString> }}
          separator: ':'
```

## <a name="sync-by-prefix-or-label"></a>Ön eke veya etikete göre Eşitle
Eşitleme eyleinizde ön ekleri veya etiketleri belirtmek yalnızca belirli bir kümeyi eşitler. Bu, birden çok dosya ile katı eşitlemenin kullanılması için önemlidir. Yapılandırmanın nasıl ayarlandığına bağlı olarak, her bir dosya ile bir ön ek veya etiket ilişkilendirilebilir ve ardından hiçbir şeyin üzerine yazılmaması için her önek veya etiket ayrı olarak eşitlenebilir. Genellikle ön ekler farklı uygulamalar veya hizmetler için kullanılır ve Etiketler farklı ortamlar için kullanılır. 

Ön eke göre Eşitle: 

```json
on:
  push:
    branches:
      - 'master'
    paths:
      - 'appsettings.json'

jobs:
  syncconfig:
    runs-on: ubuntu-latest
    steps:
      # checkout done so that files in the repo can be read by the sync
      - uses: actions/checkout@v1
      - uses: azure/appconfiguration-sync@v1
        with:
          configurationFile: 'appsettings.json'
          format: 'json'
          # Replace <ConnectionString> with the name of the secret in your repository
          connectionString: ${{ secrets.<ConnectionString> }}
          separator: ':'
          prefix: 'Prefix::'
```

Etikete göre Eşitle: 

```json
on:
  push:
    branches:
      - 'master'
    paths:
      - 'appsettings.json'

jobs:
  syncconfig:
    runs-on: ubuntu-latest
    steps:
      # checkout done so that files in the repo can be read by the sync
      - uses: actions/checkout@v1
      - uses: azure/appconfiguration-sync@v1
        with:
          configurationFile: 'appsettings.json'
          format: 'json'
          # Replace <ConnectionString> with the name of the secret in your repository
          connectionString: ${{ secrets.<ConnectionString> }}
          separator: ':'
          label: 'Label'

```

## <a name="use-a-dynamic-label-on-sync"></a>Eşitleme sırasında dinamik etiket kullan
Aşağıdaki eylem her eşitlemede dinamik bir etiket ekler ve böylece her bir eşitleme benzersiz şekilde tanımlanabilir ve kod değişikliklerinin yapılandırma değişikliklerine eşlenmesine izin verebilir.

Bu iş akışının ilk bölümü, eylemin *on* *push* `appsettings.json` *ana* dala sahip bir gönderim üzerinde tetikleyeceğini belirtir. İkinci bölüm, işleme karmasını temel alan yapılandırma güncelleştirmesi için benzersiz bir etiket oluşturan bir işi çalıştırır. Ardından iş, bu güncelleştirme için yeni değerlerle ve benzersiz etiketle birlikte uygulama yapılandırma örneğini güncelleştirir.

```json
on: 
  push: 
    branches: 
      - 'master' 
    paths: 
      - 'appsettings.json' 
 
jobs: 
  syncconfig: 
    runs-on: ubuntu-latest 
    steps: 
      # Creates a label based on the branch name and the first 8 characters          
      # of the commit hash 
      - id: determine_label 
        run: echo ::set-output name=LABEL::"${GITHUB_REF#refs/*/}/${GITHUB_SHA:0:8}" 
      # checkout done so that files in the repo can be read by the sync 
      - uses: actions/checkout@v1 
      - uses: azure/appconfiguration-sync@v1 
        with: 
          configurationFile: 'appsettings.json' 
          format: 'json' 
          # Replace <ConnectionString> with the name of the secret in your 
          # repository 
          connectionString: ${{ secrets.<ConnectionString> }}  
          separator: ':' 
          label: ${{ steps.determine_label.outputs.LABEL }} 
```

## <a name="use-azure-key-vault-with-github-action"></a>GitHub eylemiyle Azure Key Vault kullanma
AppConfiguration ile Azure Key Vault kullanan geliştiriciler, genellikle bir appSettings. JSON ve secretreferences. JSON olmak üzere iki ayrı dosya kullanmalıdır. Secretreferences. JSON, Anahtar Kasası parolasının URL 'sini içerir.

{"mySecret": "{ \" Uri \" : \" https://myKeyVault.vault.azure.net/secrets/mySecret "} "}

GitHub eylemi daha sonra appSettings. JSON üzerinde katı bir eşitleme yapmak üzere yapılandırılabilir ve ardından secretreferences. JSON üzerinde katı olmayan bir eşitleme gelir. Aşağıdaki örnek, herhangi bir dosya güncelleştirildiği zaman bir eşitleme tetikleyecektir:

```json
on:
  push:
    branches:
      - 'master'
    paths:
      - 'appsettings.json'
      - 'secretreferences.json'

jobs:
  syncconfig:
    runs-on: ubuntu-latest
    steps:
      # checkout done so that files in the repo can be read by the sync
      - uses: actions/checkout@v1
      - uses: azure/appconfiguration-sync@v1
        with:
          configurationFile: 'appsettings.json'
          format: 'json'
          # Replace <ConnectionString> with the name of the secret in your repository
          connectionString: ${{ secrets.<ConnectionString> }}
          separator: ':'
          strict: true
      - uses: azure/appconfiguration-sync@v1
        with:
          configurationFile: 'secretreferences.json'
          format: 'json'
          # Replace <ConnectionString> with the name of the secret in your repository
          connectionString: ${{ secrets.<ConnectionString> }}
          separator: ':'
          contentType: 'application/vnd.microsoft.appconfig.keyvaultref+json;charset=utf-8'

```

## <a name="use-max-depth-to-limit-github-action"></a>GitHub eylemini sınırlandırmak için maksimum derinliği kullanın
İç içe geçmiş JSON özniteliklerinin varsayılan davranışı nesnenin tamamını düzleştirebilir.  Aşağıdaki JSON bu anahtar-değer çiftini tanımlar:

| Anahtar | Değer |
| --- | --- |
| Nesne: Iç: ınnerkey | Innervalue |

```json
{ "Object": 
    { "Inner":
        {
        "InnerKey": "InnerValue"
        }
    }
}
```

İç içe yerleştirilmiş nesnenin yapılandırma örneğine itilmiş olması amaçlanıyorsa, doğru derinlikte düzleştirmeyi durdurmak için *derinlik* değerini kullanabilirsiniz. 

```json
on: 
  push: 
    branches: 
      - 'master' 
    paths: 
      - 'appsettings.json' 
 
jobs: 
  syncconfig: 
    runs-on: ubuntu-latest 
    steps: 
      # checkout done so that files in the repo can be read by the sync 
      - uses: actions/checkout@v1 
      - uses: azure/appconfiguration-sync@v1 
        with: 
          configurationFile: 'appsettings.json' 
          format: 'json' 
          # Replace <ConnectionString> with the name of the secret in your 
          # repository 
          connectionString: ${{ secrets.<ConnectionString> }}  
          separator: ':' 
          depth: 2 
```

2 derinliği verildiğinde, yukarıdaki örnek şu anahtar-değer çiftini döndürür:

| Anahtar | Değer |
| --- | --- |
| Nesne: Iç | {"Innerkey": "ınnervalue"} |

## <a name="understand-action-inputs"></a>Eylem girişlerini anlama
Giriş parametreleri, çalışma zamanı sırasında eylem tarafından kullanılan verileri belirtir.  Aşağıdaki tablo, uygulama yapılandırma eşitlemesi tarafından kabul edilen giriş parametrelerini ve her biri için beklenen değerleri içerir.  GitHub eylemlerine yönelik eylem girişleri hakkında daha fazla bilgi için bkz. GitHub 'ın [belgeleri](https://help.github.com/actions/automating-your-workflow-with-github-actions/metadata-syntax-for-github-actions#inputs).

> [!Note]
> Giriş kimlikleri büyük/küçük harfe duyarlıdır.


| Giriş adı | Gerekli mi? | Değer |
|----|----|----|
| configurationFile | Yes | Depodaki yapılandırma dosyasının göreli yolu.  Glob desenleri desteklenir ve birden çok dosya içerebilir. |
| biçim | Yes | Yapılandırma dosyasının dosya biçimi.  Geçerli biçimler şunlardır: JSON, YAML, Özellikler. |
| Dizisi | Yes | Uygulama yapılandırma örneği için bağlantı dizesi. Bağlantı dizesi GitHub deposunda bir gizli dizi olarak depolanmalıdır ve yalnızca gizli ad iş akışında kullanılmalıdır. |
| ayırıcı | Yes | Yapılandırma dosyası anahtar-değer çiftlerine düzleştirilmesi sırasında kullanılan ayırıcı.  Geçerli değerler:. , ; : - _ __ / |
| koy | Hayır | Anahtarların başlangıcına eklenecek ön ek. |
| etiket | Hayır | Anahtar-değer çiftleri ayarlanırken kullanılan etiket. Belirtilmemişse, null bir etiket kullanılır. |
| sert | Hayır | Katı modunun etkin olup olmadığını belirleyen bir Boole değeri. Varsayılan değer false'tur. |
| derinliğini | Hayır | Yapılandırma dosyasını düzleştirme için en yüksek derinlik.  Derinlik pozitif bir sayı olmalıdır.  Varsayılan değer en fazla derinliğe sahip olmayacaktır. |
| etiketler | Hayır | Anahtar-değer çiftleri üzerinde ayarlanan etiketi belirtir.  Beklenen biçim, şu şekildeki bir JSON nesnesinin strıngıingform biçimidir: {[propertyName: String]: dize;} Her özellik adı-değeri bir etiket haline gelir. |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, uygulama yapılandırma eşitlemesi GitHub eylemi ve uygulama yapılandırma örneğiniz için güncelleştirmeleri otomatikleştirmek üzere nasıl kullanılabileceği hakkında bilgi edindiniz. Azure Uygulama yapılandırması 'nın anahtar-değer çiftlerinde değişikliklere nasıl tepki verdiğini öğrenmek için sonraki [makaleye](./concept-app-configuration-event.md)geçin.
