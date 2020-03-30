---
title: Azure Veri Gezgini'ndeki verileri çözümlemek için Bir Jupyter Notebook kullanma
description: Bu konu, Jupyter Notebook ve Kqlmagic uzantısını kullanarak Azure Veri Gezgini'ndeki verileri nasıl analiz edebildiğinizi gösterir.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 83902ea5a3e73603311a0c469126ed603d0ebd16
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77064878"
---
# <a name="use-a-jupyter-notebook-and-kqlmagic-extension-to-analyze-data-in-azure-data-explorer"></a>Azure Veri Gezgini'ndeki verileri analiz etmek için Jupyter Notebook ve Kqlmagic uzantısı kullanın

Jupyter Notebook, canlı kod, denklemler, görselleştirmeler ve anlatı metni içeren belgeler oluşturmanıza ve paylaşmanıza olanak tanıyan açık kaynaklı bir web uygulamasıdır. Kullanım veri temizleme ve dönüştürme, sayısal simülasyon, istatistiksel modelleme, veri görselleştirme ve makine öğrenimi içerir.
[Jupyter Notebook,](https://jupyter.org/) ek komutları destekleyerek çekirdeğin yeteneklerini genişleten sihirli işlevleri destekler. KQL büyüsü, Kusto dil sorgularını yerel olarak çalıştırabilmeniz için Jupyter Notebook'taki Python çekirdeğinin yeteneklerini genişleten bir komuttur. `render` Python ve Kusto sorgu dilini kolayca birleştirerek komutlarla bütünleşmiş zengin Plot.ly kitaplığını kullanarak verileri sorgulayabilir ve görselleştirebilirsiniz. Sorguları çalıştırmak için veri kaynakları desteklenir. Bu veri kaynakları, günlük ve telemetri verileri için hızlı ve yüksek ölçeklenebilir bir veri arama hizmeti olan Azure Veri Gezgini'nin yanı sıra Azure Monitor günlüklerini ve Uygulama Öngörülerini içerir. KQL magic ayrıca Azure Dizüstü Bilgisayarlar, Jupyter Lab ve Visual Studio Code Jupyter uzantısı ile de çalışır.

## <a name="prerequisites"></a>Ön koşullar

- Azure Active Directory (AAD) üyesi olan kuruluşe bağlı e-posta hesabı.
- Yerel makinenizde yüklü olan Jupyter Notebook veya Azure Dizüstü Bilgisayar'ı kullanın ve örnek [Azure Not Defteri'ni](https://kustomagicsamples-manojraheja.notebooks.azure.com/j/notebooks/Getting%20Started%20with%20kqlmagic%20on%20Azure%20Data%20Explorer.ipynb) kopyala

## <a name="install-kql-magic-library"></a>KQL sihirli kitaplığını yükleyin

1. KQL büyüsünü yükleyin:

    ```python
    !pip install Kqlmagic --no-cache-dir  --upgrade
    ```
    > [!NOTE]
    > Azure Not Defteri kullanırken bu adım gerekli değildir.

1. Yük KQL sihirli:

    ```python
    %reload_ext Kqlmagic
    ```
    > [!NOTE]
    > Çekirdek sürümünü Python 3.6 olarak değiştirin, Çekirdek > Değiştir Çekirdeği'ne > Python 3.6'ya tıklayarak
    
## <a name="connect-to-the-azure-data-explorer-help-cluster"></a>Azure Veri Gezgini Yardım kümesine bağlanın

*Yardım* kümesinde barındırılan *Örnekler* veritabanına bağlanmak için aşağıdaki komutu kullanın. Microsoft'a ait olmayan AAD `Microsoft.com` kullanıcıları için kiracı adını AAD Kiracınızla değiştirin.

```python
%kql AzureDataExplorer://tenant="Microsoft.com";code;cluster='help';database='Samples'
```

## <a name="query-and-visualize"></a>Sorgula ve görselleştir

[Render işleci](/azure/kusto/query/renderoperator) kullanarak verileri sorgula ve ploy.ly kitaplığını kullanarak verileri görselleştirin. Bu sorgu ve görselleştirme, yerel KQL kullanan tümleşik bir deneyim sağlar. Kqlmagic dışında `timepivot`en `pivotchart`listeleri `ladderchart`destekler , , ve . Render , , `kind` `ysplit`ve `accumulate`. hariç tüm öznitelikleri ile desteklenir. 

### <a name="query-and-render-piechart"></a>Piechart'ı sorgula ve işleme

```python
%%kql
StormEvents
| summarize statecount=count() by State
| sort by statecount 
| limit 10
| render piechart title="My Pie Chart by State"
```

### <a name="query-and-render-timechart"></a>Zaman çizelgesini sorgula ve işleme

```python
%%kql
StormEvents
| summarize count() by bin(StartTime,7d)
| render timechart
```

> [!NOTE]
> Bu grafikler etkileşimli. Belirli bir süreyi yakınlaştırmak için bir zaman aralığı seçin.

### <a name="customize-the-chart-colors"></a>Grafik renklerini özelleştirme

Varsayılan renk paletini beğenmiyorsanız, palet seçeneklerini kullanarak grafikleri özelleştirin. Mevcut paletler burada bulunabilir: [KQL sihirli sorgu grafik sonucu için renk paleti seçin](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FColorYourCharts.ipynb)

1. Palet listesi için:

    ```python
    %kql --palettes -popup_window
    ```

1. Renk `cool` paletini seçin ve sorguyu yeniden işleyin:

    ```python
    %%kql -palette_name "cool"
    StormEvents
    | summarize statecount=count() by State
    | sort by statecount
    | limit 10
    | render piechart title="My Pie Chart by State"
    ```

## <a name="parameterize-a-query-with-python"></a>Python ile sorguya parametre ekleme

KQL sihirli Kusto sorgu dili ve Python arasında basit bir değişim sağlar. Daha fazla bilgi için: [Python ile KQL sihirli sorgunuzu parametrenize](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FParametrizeYourQuery.ipynb)

### <a name="use-a-python-variable-in-your-kql-query"></a>KQL sorgunuzda Python değişkeni kullanma

Verileri filtrelemek için sorgunuzdaki Python değişkeninin değerini kullanabilirsiniz:

```python
statefilter = ["TEXAS", "KANSAS"]
```

```python
%%kql
let _state = statefilter;
StormEvents 
| where State in (_state) 
| summarize statecount=count() by bin(StartTime,1d), State
| render timechart title = "Trend"
```

### <a name="convert-query-results-to-pandas-dataframe"></a>Sorgu sonuçlarını Pandas DataFrame'e dönüştürme

Pandas DataFrame'te bir KQL sorgusunun sonuçlarına erişebilirsiniz. Son yürütülen sorgu sonuçlarına `_kql_raw_result_` değişkene göre erişin ve sonuçları aşağıdaki gibi Pandas DataFrame'e kolayca dönüştürün:

```python
df = _kql_raw_result_.to_dataframe()
df.head(10)
```

### <a name="example"></a>Örnek

Birçok analiz senaryosunda, çok sayıda sorgu içeren yeniden kullanılabilir not defterleri oluşturmak ve bir sorgudan sonraki sorgulara sonuçları beslemek isteyebilirsiniz. Aşağıdaki örnekte verileri `statefilter` filtrelemek için Python değişkeni kullanılır.

1. En fazla `DamageProperty`en iyi 10 eyaleti görüntülemek için bir sorgu çalıştırın:

    ```python
    %%kql
    StormEvents
    | summarize max(DamageProperty) by State
    | order by max_DamageProperty desc
    | limit 10
    ```

1. En üst durumu ayıklamak ve Python değişkenine ayarlamak için bir sorgu çalıştırın:

    ```python
    df = _kql_raw_result_.to_dataframe()
    statefilter =df.loc[0].State
    statefilter
    ```

1. İfadeyi ve `let` Python değişkenini kullanarak bir sorgu çalıştırın:

    ```python
    %%kql
    let _state = statefilter;
    StormEvents 
    | where State in (_state)
    | summarize statecount=count() by bin(StartTime,1d), State
    | render timechart title = "Trend"
    ```

1. Yardım komutunu çalıştırın:

    ```python
    %kql --help "help"
    ```

> [!TIP]
> Kullanılabilir tüm yapılandırmalar hakkında `%config Kqlmagic`bilgi almak için kullanın. Bağlantı sorunları ve yanlış sorgular gibi Kusto hatalarını gidermek ve yakalamak için`%config Kqlmagic.short_errors=False`

## <a name="next-steps"></a>Sonraki adımlar

Desteklenen tüm özellikleri içeren aşağıdaki örnek not defterlerini keşfetmek için yardım komutunu çalıştırın:
- [Azure Veri Gezgini için KQL büyüsüile başlayın](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FQuickStart.ipynb) 
- [Uygulama Öngörüleri için KQL büyüsü ile başlayın](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FQuickStartAI.ipynb) 
- [Azure Monitör günlükleri için KQL büyüsü ile başlayın](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FQuickStartLA.ipynb) 
- [Python ile KQL sihirli sorgunuzu parametrize edin](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FParametrizeYourQuery.ipynb) 
- [KQL sihirli sorgu grafiği sonucunuz için renk paletini seçin](https://mybinder.org/v2/gh/Microsoft/jupyter-Kqlmagic/master?filepath=notebooks%2FColorYourCharts.ipynb)
