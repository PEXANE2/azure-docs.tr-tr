---
title: Azure uygulama yapılandırma eşitleme ile GitHub eylemlerini kullanma
description: GitHub deposunda tanımlı eylemler gerçekleştirildiğinde uygulama yapılandırma örneğiniz için bir güncelleştirme tetiklemek üzere GitHub eylemlerini kullanın
author: lisaguthrie
ms.author: lcozzens
ms.date: 01/14/2020
ms.topic: conceptual
ms.service: azure-app-configuration
ms.openlocfilehash: ce8d42ec7c37b19378b6f4ae0c81548f2eff5c9c
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77190389"
---
# <a name="sync-your-app-configuration-instance-using-github-actions"></a>GitHub Actions kullanarak Uygulama Yapılandırması örneğinizi eşitleme
Azure Uygulama yapılandırması, GitHub deposunda gerçekleştirilen bir eylem tarafından tetiklendiğinde bir uygulama yapılandırma örneğini güncelleştirmek için GitHub eylemlerini kullanır. Uygulama yapılandırmasını güncelleştirmek için GitHub iş akışlarından yararlanabilirsiniz ve uygulama yapılandırma güncelleştirmelerinin uygulama kodunu güncelleştirmek için kullanılan iş akışıyla tümleştirilmesini sağlayabilirsiniz.

GitHub eylemleri [iş akışı](https://help.github.com/articles/about-github-actions#workflow) , GitHub deponuzda tanımlanan otomatikleştirilmiş bir işlemdir. Bu işlem GitHub projenizi nasıl derleyip dağıtacağınızı gösterir. Azure Uygulama yapılandırması, kaynak deposunda değişiklik yapıldığında bir uygulama yapılandırma örneğinde güncelleştirmeleri etkinleştirmek için *Azure uygulama yapılandırma eşitleme* eylemini sağlar. 

Bir iş akışı, deponuzın `/.github/workflows/` yolunda bulunan bir YAML (. yıml) dosyası tarafından tanımlanır. Bu tanım, iş akışını tanımlayan çeşitli adımları ve parametreleri içerir.

Bir depoya gönderim gibi GitHub olayları bir GitHub eylem iş akışını tetikleyebilir.  Azure, belirtilen bir GitHub eylemi gerçekleştiğinde uygulama yapılandırma örneği güncelleştirmesini tetiklemeniz için *Azure uygulama yapılandırma eşitleme* eylemini sağlar. Bu, ekiplerin uygulama yapılandırma dosyalarını uygulama kodu ile yaptığı gibi gönderirken, gözden geçirirken veya dallandırırken GitHub 'ın temel özelliklerinden yararlanmasını sağlar.

GitHub [belgeleri](https://help.github.com/actions/automating-your-workflow-with-github-actions/configuring-a-workflow) , GitHub iş akışlarının ve eylemlerinin ayrıntılı görünümünü sağlar. 

## <a name="enable-github-actions-in-your-repository"></a>Deponuzda GitHub eylemlerini etkinleştirme
Bu GitHub eylemini kullanmaya başlamak için deponuza gidin ve **Eylemler** sekmesini seçin. "yeni iş akışı" na tıklayın ve ardından "iş akışını kendiniz ayarlayın". Buradan, Market 'te "Azure uygulama yapılandırma eşitlemesi" için arama yapın.
> [!div class="mx-imgBorder"]
> ![Eylem sekmesini seçin](media/find-github-action.png)

> [!div class="mx-imgBorder"]
> ![uygulama yapılandırması SYN eylemini seçin](media/app-configuration-sync-action.png)

## <a name="sync-configuration-files-after-a-push"></a>Bir gönderim sonrasında yapılandırma dosyalarını eşitleme
Bu eylem, `appsettings.json`bir değişiklik gönderildiğinde Azure uygulama yapılandırma dosyalarını eşitler. Bir geliştirici `appsettings.json`bir değişiklik yaptığında ve bu değişikliği yaptığında, uygulama yapılandırma eşitleme eylemi uygulama yapılandırma örneğini yeni değerlerle güncelleştirir.

Bu iş akışının ilk bölümü, eylemin *ana* dala `appsettings.json` içeren bir *gönderim* *üzerinde* tetikleyeceğini belirtir. İkinci bölüm, eylem tetiklendiğinde çalıştırılan işleri listeler. Eylem ilgili dosyaları denetler ve depoda gizli dizi olarak depolanan bağlantı dizesini kullanarak uygulama yapılandırma örneğini güncelleştirir.  GitHub 'da gizli dizileri kullanma hakkında daha fazla bilgi için, şifrelenmiş gizli dizileri oluşturma ve kullanma hakkında [Bu makaleye](https://help.github.com/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets) bakın.

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

## <a name="use-a-dynamic-label-on-sync"></a>Eşitleme sırasında dinamik etiket kullan
Önceki eylem, `appsettings.json` her güncelleştirildiğinde uygulama yapılandırma örneğini güncelleştirirler. Bu eylem her eşitleme için dinamik bir etiket ekler ve her eşitlemenin benzersiz şekilde tanımlanmasını sağlar.  Bu, kod değişikliklerinin hızlı bir şekilde yapılandırma değişiklikleriyle eşlenmenize olanak tanır.

Bu iş akışının ilk bölümü, eylemin *ana* dala `appsettings.json` içeren bir *gönderim* *üzerinde* tetikleyeceğini belirtir. İkinci bölüm, işleme karmasını temel alan yapılandırma güncelleştirmesi için benzersiz bir etiket oluşturan bir işi çalıştırır. Ardından iş, bu güncelleştirme için yeni değerlerle ve benzersiz etiketle birlikte uygulama yapılandırma örneğini güncelleştirir.

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

## <a name="use-strict-sync"></a>Katı eşitleme kullan
Katı mod etkinleştirildiğinde, eşitleme, uygulama yapılandırma örneğinin verilen önek ve etiket için yapılandırma dosyası ile eşleşmesini sağlar. Yapılandırma dosyasında bulunmayan aynı ön eke ve etikete sahip anahtar-değer çiftleri silinir. 
 
Katı mod etkinleştirilmemişse, eşitleme yalnızca yapılandırma dosyasından anahtar değerlerini ayarlar. Anahtar-değer çiftleri silinmez. 

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

2 derinliği verildiğinde, yukarıdaki örnek şu anahtarı döndürür: değer çifti:

| Anahtar | Değer |
| --- | --- |
| Nesne: Iç | {"Innerkey": "ınnervalue"} |

## <a name="understand-action-inputs"></a>Eylem girişlerini anlama
Giriş parametreleri, çalışma zamanı sırasında eylem tarafından kullanılan verileri belirtir.  Aşağıdaki tablo, uygulama yapılandırma eşitlemesi tarafından kabul edilen giriş parametrelerini ve her biri için beklenen değerleri içerir.  GitHub eylemlerine yönelik eylem girişleri hakkında daha fazla bilgi için bkz. GitHub 'ın [belgeleri](https://help.github.com/actions/automating-your-workflow-with-github-actions/metadata-syntax-for-github-actions#inputs).

> [!Note]
> Giriş kimlikleri büyük/küçük harfe duyarlıdır.


| Giriş adı | Gerekli mi? | Değer |
|----|----|----|
| configurationFile | Evet | Deponun köküne göre depodaki yapılandırma dosyasının yolu.  Glob desenleri desteklenir ve birden çok dosya içerebilir. |
| format | Evet | Yapılandırma dosyasının dosya biçimi.  Geçerli biçimler şunlardır: JSON, YAML, Özellikler. |
| connectionString | Evet | Uygulama yapılandırma örneği için bağlantı dizesi. Bağlantı dizesi GitHub deposunda bir gizli dizi olarak depolanmalıdır ve yalnızca gizli ad iş akışında kullanılmalıdır. |
| ayırıcı | Evet | Yapılandırma dosyası anahtar-değer çiftlerine düzleştirilmesi sırasında kullanılan ayırıcı.  Geçerli değerler:. , ; : - _ __ / |
| prefix | Hayır | Anahtarların başlangıcına eklenecek ön ek. |
| etiket | Hayır | Anahtar-değer çiftleri ayarlanırken kullanılan etiket. Belirtilmemişse, null bir etiket kullanılır. |
| sert | Hayır | Katı modunun etkin olup olmadığını belirleyen bir Boole değeri. Varsayılan değer false'tur. |
| derinliğini | Hayır | Yapılandırma dosyasını düzleştirme için en yüksek derinlik.  Derinlik pozitif bir sayı olmalıdır.  Varsayılan değer en fazla derinliğe sahip olmayacaktır. |
| etiketler | Hayır | Anahtar-değer çiftleri üzerinde ayarlanan etiketi belirtir.  Beklenen biçim, şu şekildeki bir JSON nesnesinin strıngıingform biçimidir: {[propertyName: String]: dize;} Her özellik adı-değeri bir etiket haline gelir. |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, uygulama yapılandırma eşitlemesi GitHub eylemi ve uygulama yapılandırma örneğiniz için güncelleştirmeleri otomatikleştirmek üzere nasıl kullanılabileceği hakkında bilgi edindiniz. Azure Uygulama yapılandırması 'nın anahtar-değer çiftlerinde değişikliklere nasıl tepki verdiğini öğrenmek için sonraki [makaleye](./concept-app-configuration-event.md)geçin.
