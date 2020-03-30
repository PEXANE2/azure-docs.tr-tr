---
title: Azure Uygulama Yapılandırma Senkronizasyonu ile GitHub Eylemlerini Kullanma
description: GitHub deponuzu güncellediğinizde Uygulama Yapılandırma örneğiniz için bir güncelleştirmeyi tetiklemek için GitHub Eylemleri'ni kullanın
author: lisaguthrie
ms.author: lcozzens
ms.date: 02/20/2020
ms.topic: conceptual
ms.service: azure-app-configuration
ms.openlocfilehash: 46d4aa4d4d37e9cac928e8d1a9e5e77ca0f30f18
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80384067"
---
# <a name="sync-your-app-configuration-instance-using-github-actions"></a>GitHub Actions kullanarak Uygulama Yapılandırması örneğinizi eşitleme

Mevcut kaynak denetim uygulamalarını kullanmaya devam etmek isteyen ekipler, GitHub depolarını Uygulama Yapılandırma depolarıyla otomatik olarak senkronize etmek için GitHub Eylemleri'ni kullanabilir. Bu, config dosyalarınızda normalde yaptığınız gibi değişiklik yapmanızı sağlarken, uygulama yapılandırması avantajlarından yararlanır: <br>
&nbsp;&nbsp;&nbsp;&nbsp;• Kodunuzun dışında merkezi yapılandırma <br>
&nbsp;&nbsp;&nbsp;&nbsp;• Uygulamanızın tamamını yeniden dağıtmadan yapılandırmayı güncelleştirme <br>
&nbsp;&nbsp;&nbsp;&nbsp;• Azure Uygulama Hizmeti ve Fonksiyonları gibi hizmetlerle entegrasyon. 

GitHub Eylemleri [iş akışı,](https://help.github.com/articles/about-github-actions#workflow) GitHub deposundaki otomatik bir işlemi tanımlar. *Azure App Configuration Sync* Action, kaynak deposunda değişiklikler yapıldığında Bir Uygulama Yapılandırmaörneğine güncelleştirmeleri tetikler. Adımları ve parametreleri tanımlamak için deponuzun `/.github/workflows/` yolunda bulunan bir YAML (.yml) dosyası kullanır. Uygulama kodunda yaptığınız gibi uygulama yapılandırma dosyalarını iterken, incelerken veya dallanırken yapılandırma güncelleştirmelerini tetikleyebilirsiniz.

GitHub [belgeleri,](https://help.github.com/actions/automating-your-workflow-with-github-actions/configuring-a-workflow) GitHub iş akışlarının ve eylemlerinin ayrıntılı görünümünü sağlar. 

## <a name="enable-github-actions-in-your-repository"></a>Deponuzda GitHub Eylemlerini Etkinleştir
Bu GitHub eylemini kullanmaya başlamak için deponuza gidin ve **Eylemler** sekmesini seçin. **Yeni iş akışını**tıklatın, ardından bir iş akışı kendiniz **ayarlayın.** Son olarak, "Azure Uygulama Yapılandırma Eşitlemi" için pazarda arama yapın.
> [!div class="mx-imgBorder"]
> ![Eylem sekmesini seçin](media/find-github-action.png)

> [!div class="mx-imgBorder"]
> ![Uygulama yapılandırma eşitleme Eylem'ini seçin](media/app-configuration-sync-action.png)

## <a name="sync-configuration-files-after-a-push"></a>Bir itme den sonra yapılandırma dosyalarını eşitleme
Bu eylem, bir değişiklik itildiğinde Azure `appsettings.json`Uygulama Yapılandırma dosyalarını eşitler. Bir geliştirici bir değişikliği `appsettings.json`zorladığında , App Configuration Sync eylemi Uygulama Yapılandırma örneğini yeni değerlerle güncelleştirir.

Bu iş akışının ilk bölümü, eylemin *ana* dala `appsettings.json` içeren bir *itme* *üzerinde* tetiklediğini belirtir. İkinci bölümde, eylem tetiklendikten sonra çalıştırılan işler listelenir. Eylem, ilgili dosyaları denetler ve depoda gizli olarak depolanan bağlantı dizesini kullanarak Uygulama Yapılandırma örneğini güncelleştirir.  GitHub'da sırları kullanma hakkında daha fazla bilgi için [GitHub'ın](https://help.github.com/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets) şifreli sırları oluşturma ve kullanma hakkındaki makalesine bakın.

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

## <a name="use-a-dynamic-label-on-sync"></a>Eşitleme üzerinde dinamik bir etiket kullanma
Önceki eylem, `appsettings.json` güncelleştirildiğinde Uygulama Yapılandırma örneğini güncelleştirir. Bu eylem, her eşitlemenin benzersiz olarak tanımlanabilmesini ve kod değişikliklerinin config değişikliklerine eşleştirilmesini sağlayarak her eşitlemenin dinamik bir etiketi ekler.

Bu iş akışının ilk bölümü, eylemin *ana* dala `appsettings.json` içeren bir *itme* *üzerinde* tetiklediğini belirtir. İkinci bölüm, işleyişe karmasını temel alan config güncelleştirmesi için benzersiz bir etiket oluşturan bir iş çalıştırıyor. İş daha sonra Uygulama Yapılandırma örneğini yeni değerler ve bu güncelleştirme için benzersiz etiketle güncelleştirir.

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

## <a name="use-strict-sync"></a>Sıkı eşitleme kullanın
Katı mod etkinleştirildiğinde, eşitleme, Uygulama Yapılandırma örneği verilen önek ve etiket için yapılandırma dosyasıyla tam olarak eşleşmesini sağlar. Yapılandırma dosyasında olmayan aynı önek ve etikete sahip anahtar değer çiftleri silinir. 
 
Sıkı mod etkinleştirilemezse, eşitleme yalnızca yapılandırma dosyasından anahtar değerlerini ayarlar. Anahtar değeri çiftleri silinmez. 

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

## <a name="use-max-depth-to-limit-github-action"></a>GitHub Aksiyonu'nun sınırlandırılması için maksimum derinliği kullanın
İç içe json öznitelikleri için varsayılan davranış tüm nesneyi düzleştirmektir.  Aşağıdaki JSON bu anahtar değeri çiftini tanımlar:

| Anahtar | Değer |
| --- | --- |
| Nesne:İç:InnerKey | InnerValue |

```json
{ "Object": 
    { "Inner":
        {
        "InnerKey": "InnerValue"
        }
    }
}
```

İç içe geçirilmiş nesne, Yapılandırma örneğine itilen değer olarak tasarlanmıştırsa, düzleştirmeyi uygun derinlikte durdurmak için *derinlik* değerini kullanabilirsiniz. 

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

2 derinliği göz önüne alındığında, yukarıdaki örnek şimdi aşağıdaki anahtar değeri çiftini döndürür:

| Anahtar | Değer |
| --- | --- |
| Nesne:İç | {"InnerKey":"InnerValue"} |

## <a name="understand-action-inputs"></a>Eylem girişlerini anlama
Giriş parametreleri, çalışma zamanı sırasında eylem tarafından kullanılan verileri belirtir.  Aşağıdaki tablo, App Configuration Sync tarafından kabul edilen giriş parametrelerini ve her biri için beklenen değerleri içerir.  GitHub Eylemleri için eylem girişleri hakkında daha fazla bilgi için GitHub [belgelerine](https://help.github.com/actions/automating-your-workflow-with-github-actions/metadata-syntax-for-github-actions#inputs)bakın.

> [!Note]
> Giriş t,.c.'leri büyük/küçük harf duyarsızdır.


| Giriş adı | Gerekli mi? | Değer |
|----|----|----|
| Configurationfile | Evet | Depodaki yapılandırma dosyasına göreli yol.  Glob desenleri desteklenir ve birden çok dosya içerebilir. |
| biçim | Evet | Yapılandırma dosyasının dosya biçimi.  Geçerli biçimleri şunlardır: JSON, YAML, özellikleri. |
| Connectionstring | Evet | Uygulama Yapılandırmaörneği için bağlantı dizesi. Bağlantı dizesi GitHub deposunda bir sır olarak depolanmalıdır ve iş akışında yalnızca gizli ad kullanılmalıdır. |
| Ayırıcı | Evet | Yapılandırma dosyasını anahtar değer çiftlerine düzleştirirken kullanılan ayırıcı.  Geçerli değerler şunlardır: . , ; : - _ __ / |
| Önek | Hayır | Anahtarların başlangıcına eklenecek önek. |
| etiket | Hayır | Anahtar değeri çiftleri ayarı yaparken kullanılan etiket. Belirtilmemişse, null etiket kullanılır. |
| Sıkı | Hayır | Katı modun etkin olup olmadığını belirleyen bir boolean değeri. Varsayılan değer false'tur. |
| Derinlik | Hayır | Yapılandırma dosyasını düzleştirmek için maksimum derinlik.  Derinlik pozitif bir sayı olmalıdır.  Varsayılan değer, maksimum derinliğe sahip olmayacaktır. |
| etiketler | Hayır | Anahtar değeri çiftleri üzerinde ayarlanan etiketi belirtir.  Beklenen biçim, aşağıdaki şeklin Bir JSON nesnesinin dizilmiş biçimidir: { [propertyName: string]: string; } Her özellik ad değeri bir etiket haline gelir. |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Uygulama Yapılandırması Sync GitHub Eylem ve nasıl App Configuration örneğiniz güncellemeleri otomatikleştirmek için nasıl kullanılabilir hakkında öğrendim. Azure Uygulama Yapılandırmasının anahtar değer çiftlerinden gelen değişikliklere nasıl tepki vereceğini öğrenmek için bir sonraki [makaleye](./concept-app-configuration-event.md)devam edin.
