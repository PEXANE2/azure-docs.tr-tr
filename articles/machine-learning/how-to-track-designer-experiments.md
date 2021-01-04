---
title: Tasarımcıda günlük ölçümleri
titleSuffix: Azure Machine Learning
description: Azure ML tasarımcı denemeleri izleyin. Python betik modülünü Yürüt ' ü kullanarak günlüğe kaydetmeyi etkinleştirin ve günlüğe kaydedilen sonuçları Studio 'da görüntüleyin.
services: machine-learning
author: likebupt
ms.author: keli19
ms.reviewer: peterlu
ms.service: machine-learning
ms.subservice: core
ms.date: 11/25/2020
ms.topic: conceptual
ms.custom: designer
ms.openlocfilehash: 29d83f4acddfce6294457f87519d62e35f52bf15
ms.sourcegitcommit: d488a97dc11038d9cef77a0235d034677212c8b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2020
ms.locfileid: "97709427"
---
# <a name="enable-logging-in-azure-machine-learning-designer-pipelines"></a>Azure Machine Learning tasarımcı işlem hatları 'nda günlüğü etkinleştir


Bu makalede, tasarımcı işlem hatlarına günlük kodu eklemeyi öğreneceksiniz. Ayrıca, bu günlükleri Azure Machine Learning Studio Web portalını kullanarak görüntülemeyi de öğreneceksiniz.

SDK yazma deneyimini kullanarak ölçümleri günlüğe kaydetme hakkında daha fazla bilgi için bkz. [Azure ML deneme çalıştırmaları ve ölçümlerini izleme](how-to-track-experiments.md).

## <a name="enable-logging-with-execute-python-script"></a>Python betiği yürütme ile günlüğe kaydetmeyi etkinleştirme

Tasarımcı işlem hatlarında günlüğe kaydetmeyi etkinleştirmek için __Python betiği yürütme__ modülünü kullanın. Bu iş akışı ile herhangi bir değer günlüğe kaydedebilir, ancak çalışma sırasında model performansını izlemek için __model değerlendir__ modülünden ölçümleri günlüğe kaydetmek yararlı olur.

Aşağıdaki örnek, modeli değerlendir ve Python betik modüllerini Yürüt ile iki eğitilen modelin ortalama kare içine nasıl ekleneceğini gösterir.

1. Bir __Python betik__ modülünü bir yürütme __modeli değerlendir__ modülünün çıktısına bağlayın.

    ![Model modülünü değerlendirmek için Python betik modülünü yürütmeyi bağlama](./media/how-to-track-experiments/designer-logging-pipeline.png)

1. Eğitilen modelinize ilişkin ortalama mutlak hatayı günlüğe kaydetmek için __Python betik__ kodu düzenleyicisini Yürüt ' e aşağıdaki kodu yapıştırın. Tasarımcıda başka bir değeri günlüğe kaydetmek için benzer bir model kullanabilirsiniz:

    ```python
    # dataframe1 contains the values from Evaluate Model
    def azureml_main(dataframe1=None, dataframe2=None):
        print(f'Input pandas.DataFrame #1: {dataframe1}')
    
        from azureml.core import Run
    
        run = Run.get_context()
    
        # Log the mean absolute error to the parent run to see the metric in the run details page.
        # Note: 'run.parent.log()' should not be called multiple times because of performance issues.
        # If repeated calls are necessary, cache 'run.parent' as a local variable and call 'log()' on that variable.
        parent_run = Run.get_context().parent
        
        # Log left output port result of Evaluate Model. This also works when evaluate only 1 model.
        parent_run.log(name='Mean_Absolute_Error (left port)', value=dataframe1['Mean_Absolute_Error'][0])
        # Log right output port result of Evaluate Model.
        parent_run.log(name='Mean_Absolute_Error (right port)', value=dataframe1['Mean_Absolute_Error'][1])

        return dataframe1,
    ```
    
Bu kod, değerlerini günlüğe kaydetmek için Python SDK Azure Machine Learning kullanır. Geçerli çalıştırmanın bağlamını almak için Run.get_context () kullanır. Ardından, Run. Parent. log () yöntemiyle bu bağlamdaki değerleri günlüğe kaydeder. Bu `parent` , değerleri modül çalıştırması yerine üst işlem hattı çalıştırmasına kaydetmek için kullanır.

Verileri günlüğe kaydetmek için Python SDK 'nın nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Azure ML eğitim çalıştırmaları 'nda günlüğü etkinleştirme](how-to-track-experiments.md).

## <a name="view-logs"></a>Günlükleri görüntüleme

İşlem hattı çalıştırıldıktan sonra, denemeleri sayfasında *Mean_Absolute_Error* görebilirsiniz.

1. **Denemeleri** bölümüne gidin.
1. Denemenizi seçin.
1. Denemek istediğiniz Deneyinizdeki çalıştırmayı seçin.
1. **Ölçümler**’i seçin.

    ![Studio 'da çalıştırma ölçümlerini görüntüleme](./media/how-to-track-experiments/experiment-page-metrics-across-runs.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, tasarımcıda günlükleri nasıl kullanacağınızı öğrendiniz. Sonraki adımlar için bu ilgili makalelere bakın:


* Tasarımcı işlem hatlarında sorun gidermeyi öğrenin, bkz. ML işlem hatları [sorunlarını giderme &](how-to-debug-pipelines.md#azure-machine-learning-designer).
* SDK yazma deneyiminde ölçümleri günlüğe kaydetmek için Python SDK 'yı nasıl kullanacağınızı öğrenin. bkz. [Azure ML eğitim çalıştırmaları 'nda günlüğü etkinleştirme](how-to-track-experiments.md).
