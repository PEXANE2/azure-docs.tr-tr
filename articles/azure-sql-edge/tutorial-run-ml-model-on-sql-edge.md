---
title: ONNX kullanarak Azure SQL Edge 'de ML modeli dağıtma
description: Bu üç parçalı Azure SQL Edge öğreticisinin üçüncü bir bölümünde, Iron ve yamallikleri tahmin etmek için, SQL Edge 'de ONNX makine öğrenimi modellerini çalıştırırsınız.
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: tutorial
author: VasiyaKrishnan
ms.author: vakrishn
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: f38a973611cb1ab18eead4ec51e6be91ada2cc40
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "85318650"
---
# <a name="deploy-ml-model-on-azure-sql-edge-using-onnx"></a>ONNX kullanarak Azure SQL Edge 'de ML modeli dağıtma 

Azure SQL Edge 'de Iron ve yamallikleri tahmin etmek için üç bölümden oluşan Bu öğreticinin üçüncü kısmında şunları yapmanız gerekir:

1. Azure SQL Edge örneğindeki SQL veritabanına bağlanmak için Azure Data Studio kullanın.
2. Azure SQL Edge 'de ONNX ile Iron Ore 'yi tahmin edin.

## <a name="connect-to-the-sql-database-in-the-azure-sql-edge-instance"></a>Azure SQl Edge örneğindeki SQL veritabanına bağlanma

1. Azure Data Studio'yu açın.

2. **Hoş geldiniz** sekmesinde, aşağıdaki ayrıntılarla yeni bir bağlantı başlatın:

   |_Alan_|_Değer_|
   |-------|-------|
   |Bağlantı türü| Microsoft SQL Server|
   |Sunucu|Bu tanıtım için oluşturulan VM 'de belirtilen genel IP adresi|
   |Kullanıcı adı|sa|
   |Parola|Azure SQL Edge örneği oluşturulurken kullanılan güçlü parola|
   |Veritabanı|Varsayılan|
   |Sunucu grubu|Varsayılan|
   |Ad (isteğe bağlı)|İsteğe bağlı bir ad sağlayın|

3. **Bağlan** 'a tıklayın

4. **Dosya** bölümünde yeni bir not defteri açın veya alt + Windows + N klavye kısayolunu kullanın. 

5. Çekirdeği Python 3 olarak ayarlayın.

## <a name="predict-iron-ore-impurities-with-onnx"></a>ONNX ile Iron ve ımpmallikleri tahmin etme

Azure Data Studio not defterine aşağıdaki python kodunu girin ve çalıştırın.

1. İlk olarak, gerekli paketleri yükleyip içeri aktarın.

   ```python
   !pip install azureml.core -q
   !pip install azureml.train.automl -q
   !pip install matplotlib -q
   !pip install pyodbc -q
   !pip install spicy -q
   
   import logging
   from matplotlib import pyplot as plt
   import numpy as np
   import pandas as pd
   import pyodbc
   
   from scipy import stats
   from scipy.stats import skew #for some statistics
   
   import azureml.core
   from azureml.core.experiment import Experiment
   from azureml.core.workspace import Workspace
   from azureml.train.automl import AutoMLConfig
   from azureml.train.automl import constants
   ```

1. Gerileme denemesi için Azure oto ml çalışma alanını ve oto ml deneme yapılandırmasını tanımlayın.

   ```python
   ws = Workspace(subscription_id="<Azure Subscription ID>",
                  resource_group="<resource group name>",
                  workspace_name="<ML workspace name>")
   # Choose a name for the experiment.
   experiment_name = 'silic_percent2-Forecasting-onnx'
   experiment = Experiment(ws, experiment_name)
   ```

1. Veri kümesini bir panda çerçevesine içeri aktarın. Model eğitiminin amacı doğrultusunda, kale 'nın [bir araştırma sürecinde](https://www.kaggle.com/edumagalhaes/quality-prediction-in-a-mining-process) eğitim verileri ayarlama kalitesi tahminini kullanın. Veri dosyasını indirin ve geliştirme makinenize yerel olarak kaydedin. Bu verileri, ne kadar etkili olduğunu tahmin etmek için kullanacaksınız.

   ```python
   df = pd.read_csv("<local path where you have saved the data file>",decimal=",",parse_dates=["date"],infer_datetime_format=True)
   df = df.drop(['date'],axis=1)
   df.describe()
   ```

1. Herhangi bir çarpıklığı belirlemek için verileri çözümleyin. Bu işlem sırasında, veri çerçevesindeki her bir sütunun dağıtım ve eğriltme bilgilerine bakın.

   ```python
   ## We can use a histogram chart to view the data distribution for the Dataset. In this example, we are looking at the histogram for the "% Silica Concentrate" 
   ## and the "% Iron Feed". From the histogram, you'll notice the data distribution is skewed for most of the features in the dataset. 
   
   f, (ax1,ax2,ax3) = plt.subplots(1,3)
   ax1.hist(df['% Iron Feed'], bins='auto')
   #ax1.title = 'Iron Feed'
   ax2.hist(df['% Silica Concentrate'], bins='auto')
   #ax2.title = 'Silica Concentrate'
   ax3.hist(df['% Silica Feed'], bins='auto')
   #ax3.title = 'Silica Feed'
   ```

1. Verilerdeki çarpıklığı düzeyini denetleyin ve düzeltir.

   ```python
   ##Check data skewness with the skew or the kurtosis function in spicy.stats
   ##Skewness using the spicy.stats skew function
   for i in list(df):
           print('Skew value for column "{0}" is: {1}'.format(i,skew(df[i])))
   
   #Fix the Skew using Box Cox Transform
   from scipy.special import boxcox1p
   for i in list(df):
       if(abs(skew(df[i])) >= 0.20):
           #print('found skew in column - {0}'.format(i))
           df[i] = boxcox1p(df[i], 0.10)
           print('Skew value for column "{0}" is: {1}'.format(i,skew(df[i])))
   ```

1. Tahmin özelliğiyle diğer özelliklerin bağıntısını kontrol edin. Bağıntı yüksek değilse, bu özellikleri kaldırın.

   ```python
   silic_corr = df.corr()['% Silica Concentrate']
   silic_corr = abs(silic_corr).sort_values()
   drop_index= silic_corr.index[:8].tolist()
   df = df.drop(drop_index, axis=1)
   df.describe()
   ```

1. En iyi algoritmayı bulmak ve eğmek için AzureML deneme deneyimini başlatın. Bu durumda, normalleştirilmiş kök ortalama kare hatası (NRMO) birincil ölçümüyle tüm regresyon algoritmalarıyla test ediliyoruz. Daha fazla bilgi için bkz. [Azure ML denemeleri birincil ölçümü](https://docs.microsoft.com/azure/machine-learning/how-to-configure-auto-train#primary-metric). Aşağıdaki kod, ML denemenizin yerel olarak çalıştırılmasını başlatacak.

   ```python
   ## Define the X_train and the y_train data sets for the AutoML experiments. X_Train are the inputs or the features, while y_train is the outcome or the prediction result. 
   
   y_train = df['% Silica Concentrate']
   x_train = df.iloc[:,0:-1]
   automl_config = AutoMLConfig(task = 'regression',
                                primary_metric = 'normalized_root_mean_squared_error',
                                iteration_timeout_minutes = 60,
                                iterations = 10,                        
                                X = x_train, 
                                y = y_train,
                                featurization = 'off',
                                enable_onnx_compatible_models=True)
   
   local_run = experiment.submit(automl_config, show_output = True)
   best_run, onnx_mdl = local_run.get_output(return_onnx_model=True)
   ```

1. Yerel Puanlama için modeli Azure SQL Edge veritabanına yükleyin.

   ```python
   ## Load the Model into a SQL Database.
   ## Define the Connection string parameters. These connection strings will be used later also in the demo.
   server = '<SQL Server IP address>'
   username = 'sa' # SQL Server username
   password = '<SQL Server password>'
   database = 'IronOreSilicaPrediction'
   db_connection_string = "Driver={ODBC Driver 17 for SQL Server};Server=" + server + ";Database=" + database + ";UID=" + username + ";PWD=" + password + ";"
   conn = pyodbc.connect(db_connection_string, autocommit=True)
   cursor = conn.cursor()
   
   # Insert the ONNX model into the models table
   query = f"insert into models ([description], [data]) values ('Silica_Percentage_Predict_Regression_NRMSE_New1',?)"
   model_bits = onnx_mdl.SerializeToString()
   insert_params  = (pyodbc.Binary(model_bits))
   cursor.execute(query, insert_params)
   conn.commit()
   cursor.close()
   conn.close()
   ```

1. Son olarak, eğitilen modeli kullanarak tahminleri gerçekleştirmek için Azure SQL Edge modelini kullanın.

   ```python
   ## Define the Connection string parameters. These connection strings will be used later also in the demo.
   server = '<SQL Server IP address>'
   username = 'sa' # SQL Server username
   password = '<SQL Server password>'
   database = 'IronOreSilicaPrediction'
   db_connection_string = "Driver={ODBC Driver 17 for SQL Server};Server=" + server + ";Database=" + database + ";UID=" + username + ";PWD=" + password + ";"
   conn = pyodbc.connect(db_connection_string, autocommit=True)
   #cursor = conn.cursor()
   query = \
           f'declare @model varbinary(max) = (Select [data] from [dbo].[Models] where [id] = 1);' \
           f' with d as ( SELECT  [timestamp] ,cast([cur_Iron_Feed] as real) [__Iron_Feed] ,cast([cur_Silica_Feed]  as real) [__Silica_Feed]' \
           f',cast([cur_Starch_Flow] as real) [Starch_Flow],cast([cur_Amina_Flow] as real) [Amina_Flow]' \
           f' ,cast([cur_Ore_Pulp_pH] as real) [Ore_Pulp_pH] ,cast([cur_Flotation_Column_01_Air_Flow] as real) [Flotation_Column_01_Air_Flow]' \
           f' ,cast([cur_Flotation_Column_02_Air_Flow] as real) [Flotation_Column_02_Air_Flow]' \
           f' ,cast([cur_Flotation_Column_03_Air_Flow] as real) [Flotation_Column_03_Air_Flow]' \
           f' ,cast([cur_Flotation_Column_07_Air_Flow] as real) [Flotation_Column_07_Air_Flow]' \
           f' ,cast([cur_Flotation_Column_04_Level] as real) [Flotation_Column_04_Level]' \
           f' ,cast([cur_Flotation_Column_05_Level] as real) [Flotation_Column_05_Level]' \
           f' ,cast([cur_Flotation_Column_06_Level] as real) [Flotation_Column_06_Level]' \
           f' ,cast([cur_Flotation_Column_07_Level] as real) [Flotation_Column_07_Level]' \
           f' ,cast([cur_Iron_Concentrate] as real) [__Iron_Concentrate]' \
           f' FROM [dbo].[IronOreMeasurements1]' \
           f' where timestamp between dateadd(hour,-1,getdate()) and getdate()) ' \
           f' SELECT d.*, p.variable_out1' \
           f' FROM PREDICT(MODEL = @model, DATA = d) WITH(variable_out1 numeric(25,17)) as p;' 
     
   df_result = pd.read_sql(query,conn)
   df_result.describe()
   ```

1. Python kullanarak, Iron Feed, DateTime ve silıca akışlarından oluşan tahmini silolardan oluşan bir grafik oluşturun.

   ```python
   import plotly.graph_objects as go
   fig = go.Figure()
   fig.add_trace(go.Scatter(x=df_result['timestamp'],y=df_result['__Iron_Feed'],mode='lines+markers',name='Iron Feed',line=dict(color='firebrick', width=2)))
   fig.add_trace(go.Scatter(x=df_result['timestamp'],y=df_result['__Silica_Feed'],mode='lines+markers',name='Silica Feed',line=dict(color='green', width=2)))
   fig.add_trace(go.Scatter(x=df_result['timestamp'],y=df_result['variable_out1'],mode='lines+markers',name='Silica Percent',line=dict(color='royalblue', width=3)))
   fig.update_layout(height= 600, width=1500,xaxis_title='Time')
   fig.show()
   ```

## <a name="next-steps"></a>Sonraki adımlar

Azure SQL Edge 'de ONNX modellerini kullanma hakkında daha fazla bilgi için bkz. [SQL Edge 'de onnx Ile makine öğrenimi ve AI (Önizleme)](onnx-overview.md).
