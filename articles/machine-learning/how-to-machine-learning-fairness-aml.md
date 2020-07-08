---
title: Python 'da ML modellerini değerlendir ' eşitliği
titleSuffix: Azure Machine Learning
description: Azure Machine Learning modellerinizin eşitliği nasıl değerlendirileyeceğinizi öğrenin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.author: mesameki
author: mesameki
ms.reviewer: luquinta
ms.date: 06/30/2020
ms.custom: tracking-python
ms.openlocfilehash: 9e88b87a7f6471f6c7344cc5548c37e947f18791
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85660863"
---
# <a name="use-azure-machine-learning-with-the-fairlearn-open-source-package-to-assess-the-fairness-of-ml-models"></a>ML modellerinin eşitliği değerlendirmek için Fairlearn açık kaynak paketiyle birlikte Azure Machine Learning kullanın  

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu nasıl yapılır kılavuzunda, aşağıdaki görevleri gerçekleştirmek için Azure Machine Learning ile [Fairlearn](https://fairlearn.github.io/) açık kaynaklı Python paketini kullanmayı öğreneceksiniz:

* Model tahminlerinizin eşitliği değerlendirin. Machine Learning 'de eşitliği hakkında daha fazla bilgi için bkz. [Machine Learning 'de eşitliği](concept-fairness-ml.md).
* Eşitliği Assessment Insights 'ı Azure Machine Learning Studio 'ya yükleyin, listeleyin ve indirin.
* Modelleriniz ' eşitliği öngörüleri ile etkileşim kurmak için Azure Machine Learning Studio 'da bir eşitliği değerlendirmesi panosu ' na bakın.

>[!NOTE]
> Eşitliği değerlendirmesi tamamen teknik bir alıştırma değildir. **Bu paket, bir makine öğrenimi modelinin eşitliği değerlendirmenize yardımcı olabilir, ancak yalnızca modelin nasıl gerçekleştirdiği hakkında kararlar verebilir ve yapabilirsiniz.**  Bu paket, eşitliği değerlendirmek için nicel ölçümlerini belirlemesine yardımcı olsa da, makine öğrenimi modellerinin geliştiricilerin, kendi modellerinin eşitliği değerlendirmek için bir nitel çözümlemesi gerçekleştirmesi gerekir.

## <a name="azure-machine-learning-fairness-sdk"></a>Azure Machine Learning eşitliği SDK 

Azure Machine Learning eşitliği SDK, `azureml-contrib-fairness` Azure Machine Learning içindeki açık kaynaklı Python paketini [Fairlearn](http://fairlearn.github.io), tümleştirir. Fairlearn 'in Azure Machine Learning dahilinde tümleştirmesi hakkında daha fazla bilgi edinmek için bu [örnek not defterlerine](https://github.com/Azure/MachineLearningNotebooks/tree/master/contrib/fairness)göz atın. Fairlearn hakkında daha fazla bilgi için bkz. [örnek kılavuz](https://fairlearn.github.io/auto_examples/notebooks/index.html) ve [örnek Not defterleri](https://github.com/fairlearn/fairlearn/tree/master/notebooks). 

Ve paketlerini yüklemek için aşağıdaki komutları kullanın `azureml-contrib-fairness` `fairlearn` :
```bash
pip install azureml-contrib-fairness
pip install fairlearn==0.4.6
```



## <a name="upload-fairness-insights-for-a-single-model"></a>Tek bir model için eşitliği öngörülerini karşıya yükleyin

Aşağıdaki örnek, model eşitliği öngörülerini Azure Machine Learning yüklemek için eşitliği paketinin nasıl kullanılacağını gösterir ve Azure Machine Learning Studio 'daki eşitliği değerlendirme panosunu görmenizi sağlar.

1. Bir Jupyter not defterinde örnek modeli eğitme. 

    Veri kümesi için, `shap` (kolaylık sağlaması için) kullanarak yüklediğimiz, iyi bilinen yetişkinlere yönelik veri kümesini kullanırız. Bu örneğin amaçları doğrultusunda, bu veri kümesini bir kredi kararı sorunu olarak kabul ediyoruz ve etiketin, her bireysel bir tek tek bir kredi kimliği 'nin geçmişte bir kredisi olduğunu gösterdiği anlamına gelir. Önceden görmeyen kişilerin krediden geri doğru olup olmayacağını tahmin etmek için bu verileri kullanacağız. Varsayım, bir bireyin bir kredi olarak sunulup sunulmayacağına karar vermek için model tahminlerinin kullanılmasını sağlar.

    ```python
    from sklearn.model_selection import train_test_split
    from fairlearn.widget import FairlearnDashboard
    from sklearn.linear_model import LogisticRegression
    from sklearn.preprocessing import LabelEncoder, StandardScaler
    import pandas as pd
    import shap

    # Load the census dataset
    X_raw, Y = shap.datasets.adult()
    X_raw["Race"].value_counts().to_dict()
    

    # (Optional) Separate the "sex" and "race" sensitive features out and drop them from the main data prior to training your model
    A = X_raw[['Sex','Race']]
    X = X_raw.drop(labels=['Sex', 'Race'],axis = 1)
    X = pd.get_dummies(X)
    
    sc = StandardScaler()
    X_scaled = sc.fit_transform(X)
    X_scaled = pd.DataFrame(X_scaled, columns=X.columns)

    # Perform some standard data preprocessing steps to convert the data into a format suitable for the ML algorithms
    le = LabelEncoder()
    Y = le.fit_transform(Y)

    # Split data into train and test
    from sklearn.model_selection import train_test_split
    from sklearn.model_selection import train_test_split
    X_train, X_test, Y_train, Y_test, A_train, A_test = train_test_split(X_scaled, 
                                                        Y, 
                                                        A,
                                                        test_size = 0.2,
                                                        random_state=0,
                                                        stratify=Y)

    # Work around indexing issue
    X_train = X_train.reset_index(drop=True)
    A_train = A_train.reset_index(drop=True)
    X_test = X_test.reset_index(drop=True)
    A_test = A_test.reset_index(drop=True)

    # Improve labels
    A_test.Sex.loc[(A_test['Sex'] == 0)] = 'female'
    A_test.Sex.loc[(A_test['Sex'] == 1)] = 'male'


    A_test.Race.loc[(A_test['Race'] == 0)] = 'Amer-Indian-Eskimo'
    A_test.Race.loc[(A_test['Race'] == 1)] = 'Asian-Pac-Islander'
    A_test.Race.loc[(A_test['Race'] == 2)] = 'Black'
    A_test.Race.loc[(A_test['Race'] == 3)] = 'Other'
    A_test.Race.loc[(A_test['Race'] == 4)] = 'White'


    # Train a classification model
    lr_predictor = LogisticRegression(solver='liblinear', fit_intercept=True)
    lr_predictor.fit(X_train, Y_train)

    # (Optional) View this model in Fairlearn's fairness dashboard, and see the disparities which appear:
    from fairlearn.widget import FairlearnDashboard
    FairlearnDashboard(sensitive_features=A_test, 
                       sensitive_feature_names=['Sex', 'Race'],
                       y_true=Y_test,
                       y_pred={"lr_model": lr_predictor.predict(X_test)})
    ```

2. Azure Machine Learning açın ve modelinizi kaydedin.
   
    Eşitliği panosu kayıtlı veya kayıtsız modeller ile tümleştirilebilir. Modelinizi aşağıdaki adımlarla Azure Machine Learning kaydedin:
    ```python
    from azureml.core import Workspace, Experiment, Model
    import joblib
    import os
    
    ws = Workspace.from_config()
    ws.get_details()

    os.makedirs('models', exist_ok=True)
    
    # Function to register models into Azure Machine Learning
    def register_model(name, model):
        print("Registering ", name)
        model_path = "models/{0}.pkl".format(name)
        joblib.dump(value=model, filename=model_path)
        registered_model = Model.register(model_path=model_path,
                                        model_name=name,
                                        workspace=ws)
        print("Registered ", registered_model.id)
        return registered_model.id

    # Call the register_model function 
    lr_reg_id = register_model("fairness_linear_regression", unmitigated_predictor)
    ```

3. Ön işlem eşitliği ölçümleri.

    Fairlearn 'in paketini kullanarak bir pano sözlüğü oluşturun `metrics` . `_create_group_metric_set`Yöntemi, pano oluşturucusuna benzer bağımsız değişkenlere sahiptir, ancak gizli özellikler sözlük olarak geçirilir (adların kullanılabilir olmasını sağlamak için). Bu yöntemi çağırırken tahmin türünü (Bu durumda ikili sınıflandırma) de belirtmemiz gerekir.

    ```python
    #  Create a dictionary of model(s) you want to assess for fairness 
    sf = { 'Race': A_test.Race, 'Sex': A_test.Sex}
    ys_pred = unmitigated_predictor.predict(X_test)
    from fairlearn.metrics._group_metric_set import _create_group_metric_set

    dash_dict = _create_group_metric_set(y_true=Y_test,
                                        predictions=ys_pred,
                                        sensitive_features=sf,
                                        prediction_type='binary_classification')
    ```
4. Önceden hesaplanan eşitliği ölçümlerini karşıya yükleyin.
    
    Şimdi `azureml.contrib.fairness` karşıya yüklemeyi gerçekleştirmek için paketi içeri aktarın:

    ```python
    from azureml.contrib.fairness import upload_dashboard_dictionary, download_dashboard_by_upload_id
    ```
    Bir deneme ve bir çalıştırma oluşturun ve panoyu buna yükleyin:
    ```python
    exp = Experiment(ws, "Test_Fairness_Census_Demo")
    print(exp)

    run = exp.start_logging()

    # Upload the dashboard to Azure Machine Learning
    try:
        dashboard_title = "Fairness insights of Logistic Regression Classifier"
        # Set validate_model_ids parameter of upload_dashboard_dictionary to False if you have not registered your model(s)
        upload_id = upload_dashboard_dictionary(run,
                                                dash_dict,
                                                dashboard_name=dashboard_title)
        print("\nUploaded to id: {0}\n".format(upload_id))
        
        # To test the dashboard, you can download it back and ensure it contains the right information
        downloaded_dict = download_dashboard_by_upload_id(run, upload_id)
    finally:
        run.complete()
    ```
5. Azure Machine Learning Studio 'dan eşitliği panosunu denetleyin

    Önceki adımları (generated eşitliği Insights 'ı Azure Machine Learning karşıya yükleyerek) tamamladıktan sonra, eşitliği panosunu [Azure Machine Learning Studio](https://ml.azure.com)'da görüntüleyebilirsiniz. Bu Pano, Fairlearn ' de sunulan görselleştirme panosu, hassas özelliğin alt gruplarının (örn. erkek vs. Female) arasındaki ayırt etmeyi incelemenize olanak sağlar.
    Azure Machine Learning Studio 'daki görselleştirme panosuna erişmek için bu yollardan birini izleyin:

    * **Denemeleri bölmesi (Önizleme)**
    1. Azure Machine Learning üzerinde çalıştırdığınız denemeleri listesini görmek için sol bölmedeki **denemeleri** öğesini seçin.
    1. Bu deneyteki tüm çalıştırmaları görüntülemek için belirli bir deneme seçin.
    1. Bir çalıştır ve sonra **eşitliği** sekmesini açıklama görselleştirme panosu olarak seçin.


    [![Eşitliği panosu](./media/how-to-machine-learning-fairness-aml/dashboard.png)](./media/how-to-machine-learning-fairness-aml/dashboard.png#lightbox)
    
    * **Modeller bölmesi**
    1. Önceki adımları izleyerek özgün modelinize kaydolduysanız, görüntülemek için sol bölmede **modeller** seçebilirsiniz.
    1. Açıklama görselleştirme panosunu görüntülemek için bir model ve sonra **eşitliği** sekmesini seçin.

    Görselleştirme panosu ve içerdiği özellikler hakkında daha fazla bilgi edinmek için lütfen Fairlearn 'in [Kullanıcı kılavuzuna](https://fairlearn.github.io/user_guide/assessment.html#fairlearn-dashboard)göz atın.

s # # birden çok model için eşitliği öngörülerini karşıya yükleyin

Birden çok modeli karşılaştırmakla ilgileniyorsanız ve eşitliği değerlendirmelerinin nasıl farklı olduğunu görüyorsanız, görselleştirme panosuna birden fazla model geçirebilir ve Performance-eşitliği ticareti üzerinde gezinebilirsiniz.

1. Modellerinizi eğitme:
    
    Önceki lojistik regresyon modeline ek olarak, bir destek vektör makinesi Estimator ' ı temel alan ikinci bir sınıflandırıcı oluşturuyoruz ve Fairlearn 'in paketini kullanarak bir eşitliği panosu sözlüğünü karşıya yükleyeceğiz `metrics` . Burada, verileri yükleme ve ön işleme adımlarını atlayıp doğrudan model eğitimi aşamasına gitmenizi unutmayın.


    ```python
    # Train your first classification model
    from sklearn.linear_model import LogisticRegression
    lr_predictor = LogisticRegression(solver='liblinear', fit_intercept=True)
    lr_predictor.fit(X_train, Y_train)

    # Train your second classification model
    from sklearn import svm
    svm_predictor = svm.SVC()
    svm_predictor.fit(X_train, Y_train)
    ```

2. Modellerinizi kaydetme

    Ardından Azure Machine Learning içinde her iki modeli de kaydedin. Sonraki yöntem çağrılarında kolaylık sağlaması için, sonuçları `id` kayıtlı modelin (biçimde bir dize) tahmine ici ile eşleyen bir sözlükte saklayın `name:version` :

    ```python
    model_dict = {}

    lr_reg_id = register_model("fairness_linear_regression", lr_predictor)
    model_dict[lr_reg_id] = lr_predictor

    svm_reg_id = register_model("fairness_svm", svm_predictor)
    model_dict[svm_reg_id] = svm_predictor
    ```

3. Fairlearn panosunu yerel olarak yükleme

    Eşitliği öngörülerini Azure Machine Learning karşıya yüklemeden önce, bu tahminleri yerel olarak çağrılan Fairlearn panosunda inceleyebilirsiniz. 



    ```python
    #  Generate models' predictions and load the fairness dashboard locally 
    ys_pred = {}
    for n, p in model_dict.items():
        ys_pred[n] = p.predict(X_test)

    from fairlearn.widget import FairlearnDashboard

    FairlearnDashboard(sensitive_features=A_test, 
                    sensitive_feature_names=['Sex', 'Race'],
                    y_true=Y_test.tolist(),
                    y_pred=ys_pred)
    ```

3. Ön işlem eşitliği ölçümleri.

    Fairlearn 'in paketini kullanarak bir pano sözlüğü oluşturun `metrics` .

    ```python
    sf = { 'Race': A_test.Race, 'Sex': A_test.Sex }

    from fairlearn.metrics._group_metric_set import _create_group_metric_set

    dash_dict = _create_group_metric_set(y_true=Y_test,
                                        predictions=ys_pred,
                                        sensitive_features=sf,
                                        prediction_type='binary_classification')
    ```
4. Önceden hesaplanan eşitliği ölçümlerini karşıya yükleyin.
    
    Şimdi `azureml.contrib.fairness` karşıya yüklemeyi gerçekleştirmek için paketi içeri aktarın:

    ```python
    from azureml.contrib.fairness import upload_dashboard_dictionary, download_dashboard_by_upload_id
    ```
    Bir deneme ve bir çalıştırma oluşturun ve panoyu buna yükleyin:
    ```python
    exp = Experiment(ws, "Compare_Two_Models_Fairness_Census_Demo")
    print(exp)

    run = exp.start_logging()

    # Upload the dashboard to Azure Machine Learning
    try:
        dashboard_title = "Fairness Assessment of Logistic Regression and SVM Classifiers"
        # Set validate_model_ids parameter of upload_dashboard_dictionary to False if you have not registered your model(s)
        upload_id = upload_dashboard_dictionary(run,
                                                dash_dict,
                                                dashboard_name=dashboard_title)
        print("\nUploaded to id: {0}\n".format(upload_id))
        
        # To test the dashboard, you can download it back and ensure it contains the right information
        downloaded_dict = download_dashboard_by_upload_id(run, upload_id)
    finally:
        run.complete()
    ```


    Önceki bölüme benzer şekilde, görselleştirme panosuna erişmek ve eşitliği ve performans açısından iki modeli karşılaştırmak için, yukarıda açıklanan yollardan birini ( **denemeleri** veya **modeller**aracılığıyla Azure Machine Learning) takip edebilirsiniz.


## <a name="upload-unmitigated-and-mitigated-fairness-insights"></a>Azaltılan ve hafiftılan eşitliği öngörülerini karşıya yükleme

Fairlearn 'in [risk azaltma algoritmalarını](https://fairlearn.github.io/user_guide/mitigation.html)kullanabilir, üretilen hafifletme modellerini orijinal olmayan modele karşılaştırabilir ve karşılaştırılan modeller arasında performans/eşitliği ticaretlerini izleyebilirsiniz.

[Izgara arama](https://fairlearn.github.io/user_guide/mitigation.html#grid-search) azaltma algoritması kullanımını gösteren bir örnek görmek için (farklı eşitliği ve performans ticaretine sahip azaltılan modellerin bir koleksiyonunu oluşturur) bu [örnek not defterine](https://github.com/Azure/MachineLearningNotebooks/blob/master/contrib/fairness/fairlearn-azureml-mitigation.ipynb)göz atın. 

Birden çok model ' eşitliği Insights 'ı tek bir çalıştırmada karşıya yüklemek, modellerin eşitliği ve performansına göre karşılaştırmasına izin verir. Belirli bir modelin ayrıntılı eşitliği içgörülerini görmek için model karşılaştırma grafiğinde görüntülenecek modellerin herhangi birine ek olarak tıklayabilirsiniz.


[![Model karşılaştırma Fairlearn panosu](./media/how-to-machine-learning-fairness-aml/multi-model-dashboard.png)](./media/how-to-machine-learning-fairness-aml/multi-model-dashboard.png#lightbox)
    

## <a name="next-steps"></a>Sonraki adımlar

[Model eşitliği hakkında daha fazla bilgi edinin](concept-fairness-ml.md)

[Azure Machine Learning eşitliği örnek not defterlerine göz atın](https://github.com/Azure/MachineLearningNotebooks/tree/master/contrib/fairness)
