---
title: Apache Hive ile Twitter verilerini analiz edin - Azure HDInsight
description: Raw TWitter verilerini aranabilir bir Hive tablosuna dönüştürmek için HDInsight'ta Apache Hive ve Apache Hadoop'u nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.date: 12/16/2019
ms.openlocfilehash: f3705170be28f33e5994bd00e363dc7ec7f94642
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75435610"
---
# <a name="analyze-twitter-data-using-apache-hive-and-apache-hadoop-on-hdinsight"></a>HDInsight'ta Apache Hive ve Apache Hadoop kullanarak Twitter verilerini analiz edin

Twitter verilerini işlemek için [Apache Hive'ı](https://hive.apache.org/) nasıl kullanacağınızı öğrenin. Sonuç, belirli bir kelime içeren en çok tweets gönderilen Twitter kullanıcılarının bir listesidir.

> [!IMPORTANT]  
> Bu belgedeki adımlar HDInsight 3.6 üzerinde test edilmiştir.

## <a name="get-the-data"></a>Verileri alma

Twitter, her tweet'in verilerini BIR REST API'si aracılığıyla JavaScript Object Notation (JSON) belgesi olarak almanıza olanak tanır. ApI'nin kimlik doğrulaması için [OAuth](https://oauth.net) gereklidir.

### <a name="create-a-twitter-application"></a>Twitter uygulaması oluşturma

1. Bir web tarayıcısından, [https://developer.twitter.com/apps/](https://developer.twitter.com/apps/)oturum aç. Twitter hesabınız yoksa **Şimdi Kaydol** bağlantısını seçin.

2. **Yeni Uygulama Oluştur'u**seçin.

3. **Ad**, **Açıklama**, **Web Sitesi**girin . **Web Sitesi** alanı için bir URL hazırlayabilirsiniz. Aşağıdaki tabloda kullanılacak bazı örnek değerler gösterilmektedir:

   | Alan | Değer |
   |--- |--- |
   | Adı |MyHDInsightApp |
   | Açıklama |MyHDInsightApp |
   | Web sitesi |`https://www.myhdinsightapp.com` |

4. **Evet'i seçin, katılıyorum**ve ardından **Twitter uygulamanızı oluştur'u**seçin.

5. **İzinler** sekmesini seçin. Varsayılan izin **yalnızca okunur.**

6. **Anahtarlar ve Erişim Jetonları** sekmesini seçin.

7. **Erişim jetonumu oluştur'u**seçin.

8. Sayfanın sağ üst köşesinde **Test OAuth'u** seçin.

9. Tüketici **anahtarı,** **Tüketici sırrı,** **Erişim belirteci**ve **Erişim belirteci gizli**yazın.

### <a name="download-tweets"></a>Tweetleri indirin

Aşağıdaki Python kodu Twitter'dan 10.000 tweets indirir ve **tweets.txt**adlı bir dosyaya kaydedin.

> [!NOTE]  
> Python zaten yüklü olduğundan, aşağıdaki adımlar HDInsight kümesinde gerçekleştirilir.

1. Kümenize bağlanmak için [ssh komutunu](./hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME'yi kümenizin adıyla değiştirerek aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. [Tweepy,](https://www.tweepy.org/) [Progress çubuğunu](https://pypi.python.org/pypi/progressbar/2.2)ve diğer gerekli paketleri yüklemek için aşağıdaki komutları kullanın:

   ```bash
   sudo apt install python-dev libffi-dev libssl-dev
   sudo apt remove python-openssl
   python -m pip install virtualenv
   mkdir gettweets
   cd gettweets
   virtualenv gettweets
   source gettweets/bin/activate
   pip install tweepy progressbar pyOpenSSL requests[security]
   ```

1. **gettweets.py**adlı bir dosya oluşturmak için aşağıdaki komutu kullanın:

   ```bash
   nano gettweets.py
   ```

1. Aşağıdaki kodu twitter uygulamanızdan `Your consumer secret` `Your consumer key`ilgili `Your access token`bilgileri `Your access token secret` değiştirerek ve değiştirerek düzenleme yapın. Ardından düzenlenen kodu **gettweets.py** dosyanın içeriği olarak yapıştırın.

   ```python
   #!/usr/bin/python

   from tweepy import Stream, OAuthHandler
   from tweepy.streaming import StreamListener
   from progressbar import ProgressBar, Percentage, Bar
   import json
   import sys

   #Twitter app information
   consumer_secret='Your consumer secret'
   consumer_key='Your consumer key'
   access_token='Your access token'
   access_token_secret='Your access token secret'

   #The number of tweets we want to get
   max_tweets=100

   #Create the listener class that receives and saves tweets
   class listener(StreamListener):
       #On init, set the counter to zero and create a progress bar
       def __init__(self, api=None):
           self.num_tweets = 0
           self.pbar = ProgressBar(widgets=[Percentage(), Bar()], maxval=max_tweets).start()

       #When data is received, do this
       def on_data(self, data):
           #Append the tweet to the 'tweets.txt' file
           with open('tweets.txt', 'a') as tweet_file:
               tweet_file.write(data)
               #Increment the number of tweets
               self.num_tweets += 1
               #Check to see if we have hit max_tweets and exit if so
               if self.num_tweets >= max_tweets:
                   self.pbar.finish()
                   sys.exit(0)
               else:
                   #increment the progress bar
                   self.pbar.update(self.num_tweets)
           return True

       #Handle any errors that may occur
       def on_error(self, status):
           print status

   #Get the OAuth token
   auth = OAuthHandler(consumer_key, consumer_secret)
   auth.set_access_token(access_token, access_token_secret)
   #Use the listener class for stream processing
   twitterStream = Stream(auth, listener())
   #Filter for these topics
   twitterStream.filter(track=["azure","cloud","hdinsight"])
   ```

    > [!TIP]  
    > Popüler anahtar kelimeleri izlemek için son satırdaki konular filtresini ayarlayın. Komut dosyasını çalıştırdığınızda popüler anahtar kelimeler kullanmak verilerin daha hızlı yakalanmasına olanak tanır.

1. Dosyayı kaydetmek için **Ctrl + X,** ardından **Y'yi** kullanın.

1. Dosyayı çalıştırmak ve tweetleri indirmek için aşağıdaki komutu kullanın:

    ```bash
    python gettweets.py
    ```

    İlerleme göstergesi görüntülenir. Tweetler indirildikçe %100'e kadar sayar.

   > [!NOTE]  
   > İlerleme çubuğunun ilerlemesi uzun sürüyorsa, popüler konuları izlemek için filtreyi değiştirmeniz gerekir. Filtrenizde konu yla ilgili birçok tweet olduğunda, gerekli 100 tweet'i hızlı bir şekilde alabilirsiniz.

### <a name="upload-the-data"></a>Verileri yükleyin

Verileri HDInsight depolama alanına yüklemek için aşağıdaki komutları kullanın:

```bash
hdfs dfs -mkdir -p /tutorials/twitter/data
hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt
```

Bu komutlar, verileri kümedeki tüm düğümlerin erişebileceği bir konumda depolar.

## <a name="run-the-hiveql-job"></a>HiveQL işini çalıştırın

1. [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) deyimleri içeren bir dosya oluşturmak için aşağıdaki komutu kullanın:

   ```bash
   nano twitter.hql
   ```

    Aşağıdaki metni dosyanın içeriği olarak kullanın:

   ```hiveql
   set hive.exec.dynamic.partition = true;
   set hive.exec.dynamic.partition.mode = nonstrict;
   -- Drop table, if it exists
   DROP TABLE tweets_raw;
   -- Create it, pointing toward the tweets logged from Twitter
   CREATE EXTERNAL TABLE tweets_raw (
       json_response STRING
   )
   STORED AS TEXTFILE LOCATION '/tutorials/twitter/data';
   -- Drop and recreate the destination table
   DROP TABLE tweets;
   CREATE TABLE tweets
   (
       id BIGINT,
       created_at STRING,
       created_at_date STRING,
       created_at_year STRING,
       created_at_month STRING,
       created_at_day STRING,
       created_at_time STRING,
       in_reply_to_user_id_str STRING,
       text STRING,
       contributors STRING,
       retweeted STRING,
       truncated STRING,
       coordinates STRING,
       source STRING,
       retweet_count INT,
       url STRING,
       hashtags array<STRING>,
       user_mentions array<STRING>,
       first_hashtag STRING,
       first_user_mention STRING,
       screen_name STRING,
       name STRING,
       followers_count INT,
       listed_count INT,
       friends_count INT,
       lang STRING,
       user_location STRING,
       time_zone STRING,
       profile_image_url STRING,
       json_response STRING
   );
   -- Select tweets from the imported data, parse the JSON,
   -- and insert into the tweets table
   FROM tweets_raw
   INSERT OVERWRITE TABLE tweets
   SELECT
       cast(get_json_object(json_response, '$.id_str') as BIGINT),
       get_json_object(json_response, '$.created_at'),
       concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
       substr (get_json_object(json_response, '$.created_at'),27,4)),
       substr (get_json_object(json_response, '$.created_at'),27,4),
       case substr (get_json_object(json_response,    '$.created_at'),5,3)
           when "Jan" then "01"
           when "Feb" then "02"
           when "Mar" then "03"
           when "Apr" then "04"
           when "May" then "05"
           when "Jun" then "06"
           when "Jul" then "07"
           when "Aug" then "08"
           when "Sep" then "09"
           when "Oct" then "10"
           when "Nov" then "11"
           when "Dec" then "12" end,
       substr (get_json_object(json_response, '$.created_at'),9,2),
       substr (get_json_object(json_response, '$.created_at'),12,8),
       get_json_object(json_response, '$.in_reply_to_user_id_str'),
       get_json_object(json_response, '$.text'),
       get_json_object(json_response, '$.contributors'),
       get_json_object(json_response, '$.retweeted'),
       get_json_object(json_response, '$.truncated'),
       get_json_object(json_response, '$.coordinates'),
       get_json_object(json_response, '$.source'),
       cast (get_json_object(json_response, '$.retweet_count') as INT),
       get_json_object(json_response, '$.entities.display_url'),
       array(
           trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
           trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
           trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
           trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
           trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
       array(
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
           trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
       trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
       trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
       get_json_object(json_response, '$.user.screen_name'),
       get_json_object(json_response, '$.user.name'),
       cast (get_json_object(json_response, '$.user.followers_count') as INT),
       cast (get_json_object(json_response, '$.user.listed_count') as INT),
       cast (get_json_object(json_response, '$.user.friends_count') as INT),
       get_json_object(json_response, '$.user.lang'),
       get_json_object(json_response, '$.user.location'),
       get_json_object(json_response, '$.user.time_zone'),
       get_json_object(json_response, '$.user.profile_image_url'),
       json_response
   WHERE (length(json_response) > 500);
   ```

1. **Ctrl + X**tuşuna basın, ardından dosyayı kaydetmek için **Y** tuşuna basın.

1. Dosyada bulunan HiveQL çalıştırmak için aşağıdaki komutu kullanın:

   ```bash
   beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i twitter.hql
   ```

    Bu komut **twitter.hql** dosyasını çalıştırın. Sorgu tamamlandıktan sonra bir `jdbc:hive2//localhost:10001/>` istem görürsünüz.

1. Beeline komut isteminden, verilerin içe aktarıldığını doğrulamak için aşağıdaki sorguyu kullanın:

   ```hiveql
   SELECT name, screen_name, count(1) as cc
   FROM tweets
   WHERE text like "%Azure%"
   GROUP BY name,screen_name
   ORDER BY cc DESC LIMIT 10;
   ```

    Bu sorgu, ileti metninde **Azure** sözcüğü nün bulunduğu en fazla 10 tweet döndürür.

    > [!NOTE]  
    > Komut dosyasındaki filtreyi `gettweets.py` değiştirdiyseniz, **Azure'u** kullandığınız filtrelerden biriyle değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

Yapılandırılmamış bir JSON veri kümesini yapılandırılmış bir [Apache Hive](https://hive.apache.org/) tablosuna dönüştürmeyi öğrendiniz. HDInsight'ta Hive hakkında daha fazla bilgi edinmek için aşağıdaki belgelere bakın:

* [HDInsight'ı kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [HDInsight'ı kullanarak uçuş gecikme verilerini analiz edin](/azure/hdinsight/interactive-query/interactive-query-tutorial-analyze-flight-data)
