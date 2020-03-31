---
title: U-SQL kullanıcı tanımlı operatörler geliştirin - Azure Data Lake Analytics
description: Azure Veri Gölü Analizi işlerinde kullanılmak ve yeniden kullanılmak üzere kullanıcı tanımlı işleçleri nasıl geliştireceğinizi öğrenin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: e5189e4e-9438-46d1-8686-ed4836bf3356
ms.topic: conceptual
ms.date: 12/05/2016
ms.openlocfilehash: b2d1293b06b4d8791138ed666bc3cb4abe3adf40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71316533"
---
# <a name="develop-u-sql-user-defined-operators-udos"></a>U-SQL kullanıcı tanımlı işleçler (UDOs) geliştirme
Bu makalede, u-SQL işinde verileri işlemek için kullanıcı tanımlı işleçler nasıl geliştirilecektir.

## <a name="define-and-use-a-user-defined-operator-in-u-sql"></a>U-SQL'de kullanıcı tanımlı bir işleç tanımlayın ve kullanın
**U-SQL işi oluşturmak ve göndermek için**

1. Visual Studio'dan **Dosya > Yeni > Projesi > U-SQL Project'i**seçin.
2. **Tamam**'a tıklayın. Visual Studio, Script.usql dosyasıyla bir çözüm oluşturur.
3. **Solution Explorer'dan,** Script.usql'i genişletin ve ardından **Script.usql.cs**çift tıklatın.
4. Aşağıdaki kodu dosyaya yapıştırın:

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;

        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Suisse", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };

                public override IRow Process(IRow input, IUpdatableRow output)
                {

                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");

                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);

                    return output.AsReadOnly();
                }
            }
        }
6. **Script.usql'i**açın ve aşağıdaki U-SQL komut dosyasını yapıştırın:

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);

        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    

        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);
7. Data Lake Analytics hesabını, Veritabanı'nı ve Şema'yı belirtin.
8. **Çözüm Gezgini**'nden, **Script.usql** öğesine sağ tıklayın ve ardından **Betik Oluştur**'a tıklayın.
9. **Çözüm Gezgini**'nden, **Script.usql** öğesine sağ tıklayın ve ardından **Betiği Gönder**'e tıklayın.
10. Azure aboneliğinize bağlı değilseniz, Azure hesap kimlik bilgilerinizi girmeniz istenir.
11. **Gönder'i**tıklatın. Gönderim sonuçları ve iş bağlantısı, gönderim tamamlandığında Sonuçlar penceresinde mevcuttur.
12. En son iş durumunu görmek ve ekranı yenilemek için **Yenile** düğmesini tıklatın.

**Çıktıyı görmek için**

1. **Server**Explorer'dan, **Azure'u**genişletin, **Veri Gölü Analizi'ni**genişletin, Veri Gölü Analizi hesabınızı genişletin, **Depolama Hesapları'nı**genişletin, Varsayılan Depolama Alanını sağ tıklatın ve ardından **Explorer'ı**tıklatın.
2. Örnekleri Genişletin, Çıktıları genişletin ve **ardından Drivers.csv'yi**çift tıklatın.

## <a name="see-also"></a>Ayrıca bkz.
* [U-SQL İfadelerini Kullanıcı Kodu ile Genişletme](/u-sql/concepts/extending-u-sql-expressions-with-user-code)
* [U-SQL uygulamaları geliştirmek için Visual Studio için Veri Gölü Araçlarını Kullanma](data-lake-analytics-data-lake-tools-get-started.md)
