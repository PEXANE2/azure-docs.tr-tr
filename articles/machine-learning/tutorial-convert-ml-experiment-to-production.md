---
title: Makine öğrenimi deneme kodunu üretim koduna dönüştürme
titleSuffix: Azure Machine Learning
description: MLOpsPython kod şablonunu kullanarak makine öğrenimi deneysel kodunu üretim koduna nasıl dönüştüreceklerini öğrenin.
author: bjcmit
ms.author: brysmith
ms.service: machine-learning
ms.topic: tutorial
ms.date: 03/13/2020
ms.openlocfilehash: e3c9b16ae3d2b06ec19ecd29d15762a065c0c1ae
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521433"
---
# <a name="tutorial-convert-ml-experimental-code-to-production-code"></a>Öğretici: ML deneysel kodunu üretim koduna dönüştürün

Bir makine öğrenme projesi hipotezler gerçek veri kümeleri kullanarak Jupyter Notebook gibi çevik araçlar ile test edilmiştir deneme gerektirir. Model üretime hazır olduğunda, model kodu bir üretim kodu deposuna yerleştirilmelidir. Bazı durumlarda, model kodunun üretim kodu deposuna yerleştirilebilmek için Python komut dosyasına dönüştürülmesi gerekir. Bu öğretici, deneme kodunun Python komut dosyasına nasıl dışa aktarılacak bir yaklaşım olduğunu kapsar.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Gereksiz kodu temizleme
> * Jupyter Notebook kodunu işlevlere dönüştür
> * İlgili görevler için Python komut dosyaları oluşturma
> * Birim testleri oluşturma

## <a name="prerequisites"></a>Ön koşullar

- [MLOpsPython şablonunu](https://github.com/microsoft/MLOpsPython/generate) oluşturun `experimentation/Diabetes Ridge Regression Training.ipynb` ve `experimentation/Diabetes Ridge Regression Scoring.ipynb` not defterlerini kullanın. Bu not defterleri denemeden üretime dönüştürmeye örnek olarak kullanılır. Bu defterleri [https://github.com/microsoft/MLOpsPython/tree/master/experimentation](https://github.com/microsoft/MLOpsPython/tree/master/experimentation).
- `nbconvert`yükleyin. [Yükleme](https://nbconvert.readthedocs.io/en/latest/install.html) sayfasında __nbconvert yükleme__ bölümü altında sadece yükleme yönergeleri izleyin.

## <a name="remove-all-nonessential-code"></a>Tüm gereksiz kodu kaldırma

Deneme sırasında yazılan bazı kodlar yalnızca keşif amaçlıdır. Bu nedenle, deney kodunu üretim koduna dönüştürmek için ilk adım, bu gereksiz kodu kaldırmaktır. Gereksiz kodun kaldırılması da kodu daha koruyabilir hale getirecektir. Bu bölümde, `experimentation/Diabetes Ridge Regression Training.ipynb` kodu not defterinden kaldırırsınız. Şekli `X` yazdıran ifadeler `y` ve hücre `features.describe` araması yalnızca veri araştırması içindir ve kaldırılabilir. Gereksiz kodu kaldırdıktan `experimentation/Diabetes Ridge Regression Training.ipynb` sonra, işaretleme olmadan aşağıdaki kod gibi görünmelidir:

```python
from sklearn.datasets import load_diabetes
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
import joblib
import pandas as pd

sample_data = load_diabetes()

df = pd.DataFrame(
    data=sample_data.data,
    columns=sample_data.feature_names)
df['Y'] = sample_data.target

X = df.drop('Y', axis=1).values
y = df['Y'].values

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=0)
data = {"train": {"X": X_train, "y": y_train},
        "test": {"X": X_test, "y": y_test}}

args = {
    "alpha": 0.5
}

reg_model = Ridge(**args)
reg.fit(data["train"]["X"], data["train"]["y"])

preds = reg_model.predict(data["test"]["X"])
mse = mean_squared_error(preds, y_test)
metrics = {"mse": mse}
print(metrics)

model_name = "sklearn_regression_model.pkl"
joblib.dump(value=reg, filename=model_name)
```

## <a name="refactor-code-into-functions"></a>Yeniden düzenleme kodu işlevlere

İkinci olarak, Jupyter kodunun işlevlere yeniden dahil edilmesi gerekiyor. Kodu işlevlere yeniden düzenleme birim sınamasını kolaylaştırır ve kodun daha korunabilir hale getirir. Bu bölümde, yeniden faktör olacak:

- Diabetes Ridge Regresyon`experimentation/Diabetes Ridge Regression Training.ipynb`Eğitimi not defteri( )
- Diabetes Ridge Regresyon`experimentation/Diabetes Ridge Regression Scoring.ipynb`Puanlama defteri( )

### <a name="refactor-diabetes-ridge-regression-training-notebook-into-functions"></a>Fonksiyonlar içine Refactor Diabetes Ridge Regresyon Eğitim dizüstü

In `experimentation/Diabetes Ridge Regression Training.ipynb`, aşağıdaki adımları tamamlayın:

1. Veri çerçevesini `split_data` test ve train verilerine bölmek için çağrılan bir işlev oluşturun. İşlev veri çerçevesini `df` parametre olarak almalı ve anahtarları `train` `test`içeren bir sözlük döndürmeli ve .

    İşlev içine başlığı *Eğitim ve Doğrulama Kümeleri içine Bölünmüş Veri* altında kod taşıyın ve nesneyi döndürmek için değiştirin. `data` `split_data`

1. Parametreleri `train_model` `data` alan ve `args` eğitimli bir modeli döndüren , adlı bir işlev oluşturun.

    *Eğitim Kümesi'ndeki Eğitim Modeli* başlığı altındaki `train_model` kodu işleve taşıyın ve nesneyi döndürmek için değiştirin. `reg_model` `args` Sözlüğü kaldırın, değerler `args` parametreden gelecektir.

1. Parametreleri `get_model_metrics` `reg_model` alan ve `data`modeli değerlendiren ve sonra eğitilen model için bir ölçüt sözlüğü döndüren , adlı bir işlev oluşturun.

    Kodu *Doğrulama Kümesi'ndeki Modeli doğrulama* işlevine `get_model_metrics` taşıyın ve nesneyi döndürmek için değiştirin. `metrics`

Üç işlev aşağıdaki gibi olmalıdır:

```python
# Split the dataframe into test and train data
def split_data(df):
    X = df.drop('Y', axis=1).values
    y = df['Y'].values

    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=0)
    data = {"train": {"X": X_train, "y": y_train},
            "test": {"X": X_test, "y": y_test}}
    return data


# Train the model, return the model
def train_model(data, args):
    reg_model = Ridge(**args)
    reg_model.fit(data["train"]["X"], data["train"]["y"])
    return reg_model


# Evaluate the metrics for the model
def get_model_metrics(reg_model, data):
    preds = reg_model.predict(data["test"]["X"])
    mse = mean_squared_error(preds, data["test"]["y"])
    metrics = {"mse": mse}
    return metrics
```

Hala, `experimentation/Diabetes Ridge Regression Training.ipynb`aşağıdaki adımları tamamlamak:

1. Hiçbir parametre `main`almaz ve hiçbir şey döndürür adlı yeni bir işlev oluşturun.
1. Kodu "Veri Yükle" başlığı altında `main` işleve taşıyın.
1. `main` İşlev içine yeni yazılmış işlevler için çağrı ekleme:
    ```python
    # Split Data into Training and Validation Sets
    data = split_data(df)
    ```

    ```python
    # Train Model on Training Set
    args = {
        "alpha": 0.5
    }
    reg = train_model(data, args)
    ```

    ```python
    # Validate Model on Validation Set
    metrics = get_model_metrics(reg, data)
    ```
1. Kodu "Modeli Kaydet" başlığı altında `main` işleve taşıyın.

İşlev `main` aşağıdaki kod gibi görünmelidir:

```python
def main():
    # Load Data
    sample_data = load_diabetes()

    df = pd.DataFrame(
        data=sample_data.data,
        columns=sample_data.feature_names)
    df['Y'] = sample_data.target

    # Split Data into Training and Validation Sets
    data = split_data(df)

    # Train Model on Training Set
    args = {
        "alpha": 0.5
    }
    reg = train_model(data, args)

    # Validate Model on Validation Set
    metrics = get_model_metrics(reg, data)

    # Save Model
    model_name = "sklearn_regression_model.pkl"

    joblib.dump(value=reg, filename=model_name)
```

Bu aşamada, not defterinde ilk hücredeki alma deyimleri dışında bir işlevde olmayan kod kalmalıdır.

`main` İşlev çağıran bir deyim ekleyin.

```python
main()
```

Yeniden düzenlemeden `experimentation/Diabetes Ridge Regression Training.ipynb` sonra, işaretleme olmadan aşağıdaki kod gibi görünmelidir:

```python
from sklearn.datasets import load_diabetes
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
import pandas as pd
import joblib


# Split the dataframe into test and train data
def split_data(df):
    X = df.drop('Y', axis=1).values
    y = df['Y'].values

    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=0)
    data = {"train": {"X": X_train, "y": y_train},
            "test": {"X": X_test, "y": y_test}}
    return data


# Train the model, return the model
def train_model(data, args):
    reg_model = Ridge(**args)
    reg_model.fit(data["train"]["X"], data["train"]["y"])
    return reg_model


# Evaluate the metrics for the model
def get_model_metrics(reg_model, data):
    preds = reg_model.predict(data["test"]["X"])
    mse = mean_squared_error(preds, data["test"]["y"])
    metrics = {"mse": mse}
    return metrics


def main():
    # Load Data
    sample_data = load_diabetes()

    df = pd.DataFrame(
        data=sample_data.data,
        columns=sample_data.feature_names)
    df['Y'] = sample_data.target

    # Split Data into Training and Validation Sets
    data = split_data(df)

    # Train Model on Training Set
    args = {
        "alpha": 0.5
    }
    reg = train_model(data, args)

    # Validate Model on Validation Set
    metrics = get_model_metrics(reg, data)

    # Save Model
    model_name = "sklearn_regression_model.pkl"

    joblib.dump(value=reg, filename=model_name)

main()
```

### <a name="refactor-diabetes-ridge-regression-scoring-notebook-into-functions"></a>Refactor Diabetes Ridge Regresyon Fonksiyonlara Not Defteri Puanlama

In `experimentation/Diabetes Ridge Regression Scoring.ipynb`, aşağıdaki adımları tamamlayın:

1. Hiçbir parametre `init`almaz ve hiçbir şey döndürmeden adlandırılan yeni bir işlev oluşturun.
1. Kodu "Yük Modeli" başlığı altında `init` işleve kopyalayın.

İşlev `init` aşağıdaki kod gibi görünmelidir:

```python
def init():
    model_path = Model.get_model_path(
        model_name="sklearn_regression_model.pkl")
    model = joblib.load(model_path)
```

`init` İşlev oluşturulduktan sonra, "Yük Modeli" başlığı altındaki tüm kodu `init` aşağıdaki gibi tek bir çağrıyla değiştirin:

```python
init()
```

In `experimentation/Diabetes Ridge Regression Scoring.ipynb`, aşağıdaki adımları tamamlayın:

1. Parametre olarak `run`ve `request_headers` aşağıdaki `raw_data` gibi bir sonuç sözlüğü döndüren ve döndüren yeni bir işlev oluşturun:

    ```python
    {"result": result.tolist()}
    ```

1. Kodu "Veri Hazırlama" ve "Veri Puan" başlıkları `run` altında işleve kopyalayın.

    İşlev `run` aşağıdaki kod gibi görünmelidir (Değişkenleri `raw_data` ayarlayan ve `request_headers`işlev çağrıldığında daha `run` sonra kullanılacak olan ifadeleri kaldırmayı unutmayın):

    ```python
    def run(raw_data, request_headers):
        data = json.loads(raw_data)["data"]
        data = numpy.array(data)
        result = model.predict(data)

        return {"result": result.tolist()}
    ```

`run` İşlev oluşturulduktan sonra, "Veri Hazırlama" ve "Veri Puan" başlıkları altındaki tüm kodu aşağıdaki kodla değiştirin:

```python
raw_data = '{"data":[[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}'
request_header = {}
prediction = run(raw_data, request_header)
print("Test result: ", prediction)
```

Önceki kod değişkenleri `raw_data` `request_header`ayarlar ve `run` , `raw_data` `request_header`işlevi çağırır ve , ve tahminleri yazdırır.

Yeniden düzenlemeden `experimentation/Diabetes Ridge Regression Scoring.ipynb` sonra, işaretleme olmadan aşağıdaki kod gibi görünmelidir:

```python
import json
import numpy
from azureml.core.model import Model
import joblib

def init():
    model_path = Model.get_model_path(
        model_name="sklearn_regression_model.pkl")
    model = joblib.load(model_path)

def run(raw_data, request_headers):
    data = json.loads(raw_data)["data"]
    data = numpy.array(data)
    result = model.predict(data)

    return {"result": result.tolist()}

init()
test_row = '{"data":[[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}'
request_header = {}
prediction = run(test_row, {})
print("Test result: ", prediction)
```

## <a name="combine-related-functions-in-python-files"></a>Python dosyalarında ilgili işlevleri birleştirme

Üçüncü olarak, ilgili işlevlerin kodun yeniden kullanımına daha iyi yardımcı olması için Python dosyalarında birleştirilmesi gerekir. Bu bölümde, aşağıdaki not defterleri için Python dosyaları oluşturacaksınız:

- Diabetes Ridge Regresyon`experimentation/Diabetes Ridge Regression Training.ipynb`Eğitimi not defteri( )
- Diabetes Ridge Regresyon`experimentation/Diabetes Ridge Regression Scoring.ipynb`Puanlama defteri( )

### <a name="create-python-file-for-the-diabetes-ridge-regression-training-notebook"></a>Diabetes Ridge Regresyon Eğitimi dizüstü bilgisayarı için Python dosyası oluşturma

Aşağıdaki ifadeyi `nbconvert` bir komut isteminde çalıştırarak not defterinizi çalıştırılabilir bir komut `experimentation/Diabetes Ridge Regression Training.ipynb`dosyasına dönüştürün:

```
jupyter nbconvert -- to script "Diabetes Ridge Regression Training.ipynb" –output train
```

Not defteri dönüştürüldükten `train.py`sonra, istenmeyen yorumları kaldırın. Aşağıdaki kod `main()` gibi koşullu bir çağırma ile dosyanın sonundaki çağrıyı değiştirin:

```python
if __name__ == '__main__':
    main()
```

Dosyanız `train.py` aşağıdaki kod gibi görünmelidir:

```python
from sklearn.datasets import load_diabetes
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import train_test_split
import pandas as pd
import joblib


# Split the dataframe into test and train data
def split_data(df):
    X = df.drop('Y', axis=1).values
    y = df['Y'].values

    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=0)
    data = {"train": {"X": X_train, "y": y_train},
            "test": {"X": X_test, "y": y_test}}
    return data


# Train the model, return the model
def train_model(data, args):
    reg_model = Ridge(**args)
    reg_model.fit(data["train"]["X"], data["train"]["y"])
    return reg_model


# Evaluate the metrics for the model
def get_model_metrics(reg_model, data):
    preds = reg_model.predict(data["test"]["X"])
    mse = mean_squared_error(preds, data["test"]["y"])
    metrics = {"mse": mse}
    return metrics


def main():
    # Load Data
    sample_data = load_diabetes()

    df = pd.DataFrame(
        data=sample_data.data,
        columns=sample_data.feature_names)
    df['Y'] = sample_data.target

    # Split Data into Training and Validation Sets
    data = split_data(df)

    # Train Model on Training Set
    args = {
        "alpha": 0.5
    }
    reg = train_model(data, args)

    # Validate Model on Validation Set
    metrics = get_model_metrics(reg, data)

    # Save Model
    model_name = "sklearn_regression_model.pkl"

    joblib.dump(value=reg, filename=model_name)

if __name__ == '__main__':
    main()
```

`train.py`artık çalıştırılarak `python train.py`bir terminalden çağrılabilir.
İşlevler `train.py` diğer dosyalardan da çağrılabilir.

MLOpsPython deposundaki `train_aml.py` `diabetes_regression/training` dizinde bulunan dosya, Azure Machine Learning `train.py` deneme çalışması bağlamında tanımlanan işlevleri çağırır. Fonksiyonlar, daha sonra bu kılavuzda ele alınan birim testlerinde de çağrılabilir.

### <a name="create-python-file-for-the-diabetes-ridge-regression-scoring-notebook"></a>Diabetes Ridge Regresyon Puanlama not defteri için Python dosyası oluşturma

Aşağıdaki ifadeyi `nbconvert` paketi ve aşağıdaki yolu kullanan bir komut isteminde çalıştırarak not `experimentation/Diabetes Ridge Regression Scoring.ipynb`defterinizi çalıştırılabilir bir komut dosyasına dahil edin:

```
jupyter nbconvert -- to script "Diabetes Ridge Regression Scoring.ipynb" –output score
```

Not defteri dönüştürüldükten `score.py`sonra, istenmeyen yorumları kaldırın. Dosyanız `score.py` aşağıdaki kod gibi görünmelidir:

```python
import json
import numpy
from azureml.core.model import Model
import joblib

def init():
    model_path = Model.get_model_path(
        model_name="sklearn_regression_model.pkl")
    model = joblib.load(model_path)

def run(raw_data, request_headers):
    data = json.loads(raw_data)["data"]
    data = numpy.array(data)
    result = model.predict(data)

    return {"result": result.tolist()}

init()
test_row = '{"data":[[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}'
request_header = {}
prediction = run(test_row, request_header)
print("Test result: ", prediction)
```

Değişkenin `model` genel olması gerekir, böylece komut dosyası boyunca görünür olur. İşlevin başında aşağıdaki ifadeyi `init` ekleyin:

```python
global model
```

Önceki deyim ekledikten `init` sonra, işlev aşağıdaki kod gibi görünmelidir:

```python
def init():
    global model

    # load the model from file into a global object
    model_path = Model.get_model_path(
        model_name="sklearn_regression_model.pkl")
    model = joblib.load(model_path)
```

## <a name="create-unit-tests-for-each-python-file"></a>Her Python dosyası için birim testleri oluşturma

Dördüncü olarak, Python işlevleriniz için birim testleri oluşturun. Birim testleri kodu işlevsel gerilemelere karşı korur ve bakımı kolaylaştırır. Bu bölümde, `train.py`'deki işlevler için birim testleri oluşturacaksınız.

`train.py`birden çok işlev içerir, ancak bu öğreticide `train_model` Pytest çerçevesini kullanarak işlev için yalnızca tek bir birim testi oluştururuz. Pytest sadece Python birim test çerçevesi değil, ama en yaygın olarak kullanılan biridir. Daha fazla bilgi için [Pytest](https://pytest.org)adresini ziyaret edin.

Bir birim testi genellikle üç ana eylem içerir:

- Nesneyi düzenleme - gerekli nesneleri oluşturma ve ayarlama
- Bir nesneye göre hareket edin
- Bekleneni ortaya koy

Birim testi bazı `train_model` sabit kodlanmış veriler ve bağımsız değişkenler ile çağırır ve bir tahmin yapmak için elde edilen eğitilen modeli kullanarak ve beklenen bir değer için bu tahmin karşılaştırarak beklendiği gibi `train_model` hareket doğrulayın.

```python
import numpy as np
from code.training.train import train_model


def test_train_model():
    # Arrange
    X_train = np.array([1, 2, 3, 4, 5, 6]).reshape(-1, 1)
    y_train = np.array([10, 9, 8, 8, 6, 5])
    data = {"train": {"X": X_train, "y": y_train}}

    # Act
    reg_model = train_model(data, {"alpha": 1.2})

    # Assert
    preds = reg_model.predict([[1], [2]])
    np.testing.assert_almost_equal(preds, [9.93939393939394, 9.03030303030303])
```

## <a name="next-steps"></a>Sonraki adımlar

Artık bir denemeden üretim koduna nasıl dönüştürüldüğünü anladığınızdan, daha fazla bilgi ve sonraki adımlar için aşağıdaki bağlantılara bakın:

+ [MLOpsPython](https://github.com/microsoft/MLOpsPython/blob/master/docs/custom_model.md): Azure Boru Hatları ve Azure Machine Learning'i kullanarak kendi modelinizi eğitmek, değerlendirmek ve dağıtmak için bir CI/CD ardışık hattı oluşturun
+ [Azure ML deneme çalıştırmalarını ve ölçümlerini izleyin](https://docs.microsoft.com/azure/machine-learning/how-to-track-experiments)
+ [ML web hizmeti uç noktalarından veri izleme ve toplama](https://docs.microsoft.com/azure/machine-learning/how-to-enable-app-insights)
