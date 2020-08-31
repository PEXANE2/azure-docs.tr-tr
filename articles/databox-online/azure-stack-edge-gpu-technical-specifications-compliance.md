---
title: GPU teknik belirtimleri ve uyumluluğuyla Microsoft Azure Stack Edge | Microsoft Docs
description: GPU ile Azure Stack Edge cihazınız için teknik belirtimler ve uyumluluk hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 08/26/2020
ms.author: alkohli
ms.openlocfilehash: 87c96ef6487895d3230541f0ae5fe15c5a645368
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89086974"
---
# <a name="azure-stack-edge-technical-specifications-and-compliance"></a>Azure Stack Edge teknik belirtimleri ve uyumluluğu

Birlikte bulunan bir grafik Işleme birimi (GPU) Azure Stack Kenarlarınızın donanım bileşenleri, bu makalede açıklanan teknik belirtimler ve yasal standartlar standartlarına uyar. Teknik belirtimler donanım, güç kaynağı birimleri (PSUs), depolama kapasitesi, kasaları ve çevresel standartları anlatmaktadır.

## <a name="compute-and-memory-specifications"></a>İşlem ve bellek belirtimleri

Azure Stack Edge cihazının işlem ve bellek için aşağıdaki belirtimleri vardır:

| Belirtim           | Değer                  |
|-------------------------|----------------------------|
| CPU                     | 2 X Intel Xeon gümüş 4214 (Cascade Lake) CPU            |
| Bellek                  | 128 (8x16 GB) GB RAM                     |


## <a name="compute-acceleration-specifications"></a>İşlem hızlandırma belirtimleri

Grafik Işleme birimi (GPU), Kubernetes, derin öğrenme ve makine öğrenimi senaryolarına izin veren her bir Azure Stack Edge cihazına dahildir.

| Belirtim           | Değer                  |
|-------------------------|----------------------------|
| GPU   | Bir veya iki NVIDIA T4 GPU <br> Daha fazla bilgi için bkz. [NVIDIA T4](https://www.nvidia.com/en-us/data-center/tesla-t4/).| 


## <a name="power-supply-unit-specifications"></a>Güç kaynağı birimi belirtimleri

Azure Stack Edge cihazında, yüksek performanslı fanlarla iki 100-240 V güç kaynağı (PSUs) vardır. İki PSUs, yedekli bir güç yapılandırması sağlar. Bir PSU başarısız olursa, başarısız modül değiştirilinceye kadar cihaz normal olarak diğer PSU üzerinde çalışmaya devam eder. Aşağıdaki tabloda, PSUs teknik özellikleri listelenmektedir.

| Belirtim           | 750 W PSU                  |
|-------------------------|----------------------------|
| Maksimum çıkış gücü    | 750 W                     |
| Frequency               | 50/60 Hz                   |
| Voltaj aralığı seçimi | Otomatik olarak değişen: 100-240 V AC |
| Etkin takılabilir           | Yes                        |


## <a name="network-interface-specifications"></a>Ağ arabirimi belirtimleri

Azure Stack Edge cihazınız altı ağ arabirimine sahiptir, PORT1-PORT6.

| Belirtim           | Açıklama                 |
|-------------------------|----------------------------|
|  Ağ arabirimleri    | **2 X 1 GbE arabirimler** – 1 yönetim arabirimi bağlantı noktası 1, ilk kurulum için kullanılır ve varsayılan olarak statiktir. İlk kurulum tamamlandıktan sonra, herhangi bir IP adresine sahip veriler için arabirimi kullanabilirsiniz. Ancak, sıfırlama sırasında Arabirim statik IP 'ye geri döner. <br>Diğer arabirim bağlantı noktası 2 Kullanıcı tarafından yapılandırılabilir, veri aktarımı için kullanılabilir ve varsayılan olarak DHCP 'dir. <br>**4 X 25 GbE arabirim** – bu veri arabirimleri, bağlantı noktası 6 ile bağlantı noktası 3, Kullanıcı tarafından DHCP (varsayılan) veya statik olarak yapılandırılabilir. Bunlar ayrıca 10 GbE arabirimi olarak da çalışabilir.  | 

Azure Stack Edge cihazınız aşağıdaki ağ donanımlarına sahiptir:

* **Özel Microsoft Qlogic Cavium 25G NDC bağdaştırıcısı** -bağlantı noktası 4 ile bağlantı noktası 1.
* **Mellanox Dual Port 25G ConnectX-4 kanal ağ bağdaştırıcısı** -bağlantı noktası 5 ve bağlantı noktası 6.

Aşağıda, Mellanox kartının ayrıntıları verilmiştir:

| Parametre           | Açıklama                 |
|-------------------------|----------------------------|
| Modelleme    | ConnectX®-4 LX EN fazla ağ arabirim kartı                      |
| Model açıklaması               | 25GbE çift bağlantı noktası SFP28; PCIe 3.0 x8; ROHS R6                    |
| Cihaz bölüm numarası (R640) | MCX4121A-ACAT  |
| PSıD (R640)           | MT_2420110034                         |

Bu ağ kartlarında desteklenen kabloların, anahtarların ve alıcı sayısının tam listesi için şuraya gidin:

- [Qlogic Cavium 25G NDC bağdaştırıcısı birlikte çalışabilirlik matrisi](https://www.marvell.com/documents/xalflardzafh32cfvi0z/).
- [Mellanox Dual Port 25G ConnectX-4 kanal ağ bağdaştırıcısına uyumlu ürünler](https://docs.mellanox.com/display/ConnectX4LxFirmwarev14271016/Firmware+Compatible+Products).  

## <a name="storage-specifications"></a>Depolama belirtimleri

Azure Stack Edge cihazlarında beş 2,5 "NVMe DC P4610 SSDs, her biri 1,6 TB kapasiteye sahiptir. Önyükleme sürücüsü 240 GB SATA SSD 'dir. Cihaz için kullanılabilen toplam kapasite kabaca 8,28 TB 'tır. Aşağıdaki tabloda, cihazın depolama kapasitesi listelenmektedir.

|     Belirtim                          |     Değer             |
|--------------------------------------------|-----------------------|
|    Önyükleme SATA katı hal sürücüleri (SSD)      |    1                  |
|    NVMe SSDs sayısı                     |    5                  |
|    Önyükleme SSD kapasitesi                       |    240 GB             |
|    Tek NVMe SSD kapasitesi                |    1,6 TB             |
|    Toplam kapasite                          |    8,28 TB            |
|    Toplam kullanılabilir kapasite *                  |    ~ 7,95 TB          |
|    SAS denetleyicisi                          |    HBA330 12 Gbps     |


**Bazı boşluklar iç kullanım için ayrılmıştır.*

<!--Remove based on feedback from Ravi
## Other hardware specifications

Your Azure Stack Edge device also contains the following hardware:

* iDRAC baseboard management
* Performance fans
* Custom ID module-->

## <a name="enclosure-dimensions-and-weight-specifications"></a>Kutu boyutları ve ağırlık belirtimleri

Aşağıdaki tablolarda, Boyutlar ve ağırlığa yönelik çeşitli muhafaza belirtimleri listelenmektedir.

### <a name="enclosure-dimensions"></a>Kutu boyutları

Aşağıdaki tabloda, milimetre ve inç olarak bulunan 1U cihaz Kasası boyutları listelenmektedir.

|     Kutu     |     Milimetre     |     Cm     |
|-------------------|---------------------|----------------|
|    Height         |    44,45            |    1,75 "       |
|    Width          |    434,1            |    17,09 "      |
|    Uzunluk         |    740,4            |    29,15 "      |

Aşağıdaki tabloda, nakliye paketinin boyutları milimetre ve inç olarak listelenmektedir.

|     Paket     |     Milimetre     |     Cm     |
|-------------------|---------------------|----------------|
|    Height         |    311,2            |    12,25 "          |
|    Width          |    642,8            |    25,31 "          |
|    Uzunluk         |    1.051,1          |    41,38 "          |

### <a name="enclosure-weight"></a>Kasa ağırlığı

Cihaz paketi 66 lbs. ve bunu işlemek için iki kişi gerektirir. Cihazın ağırlığı, kutunun yapılandırmasına bağlıdır.

|     Kutu                                 |     Ağırlık          |
|-----------------------------------------------|---------------------|
|    Paketleme dahil toplam ağırlık       |    61 lbs.          |
|    Cihazın ağırlığı                       |    35 lbs.          |

## <a name="enclosure-environment-specifications"></a>Muhafaza ortamı belirtimleri

Bu bölümde sıcaklık, nem ve yükseklik gibi çevre duvarı ortamıyla ilgili belirtimler listelenmektedir.

### <a name="temperature-and-humidity"></a>Sıcaklık ve nem

|     Kutu         |     Çevresel sıcaklık aralığı     |     Çevresel göreli nem     |     En fazla Dew noktası     |
|-----------------------|--------------------------------------|--------------------------------------|---------------------------|
|    Operasyonel        |    10 °C-35 °C (50 °F-86 °F)         |    %10-%80 yoğunlaşmayan.         |    29 °C (84 °F)            |
|    İşlemsel olmayan    |    -40 °C ila 65 °C (-40 °F-149 °F)     |    %5-%95 yoğunlaşmayan.          |    33 °C (91 °F)            |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Airflow, yükseklik, darbe, titreşim, yönlendirme, güvenlik ve EMC

|     Kutu                           |     İşletimsel belirtimler                                                                                                                                                                                         |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    Airflow                              |    Sistem Airflow, baştan sona. Sistemin düşük basınç, arka uç yüklemesiyle birlikte çalıştırılması gerekir. <!--Back pressure created by rack doors and obstacles should not exceed 5 pascals (0.5 mm water gauge).-->    |
|    Maksimum yükseklik, işletimsel        |    3048 metreler (10.000 metre), [çalışma sıcaklığı derecelendirme belirtimlerine](#operating-temperature-de-rating-specifications)göre belirlenen maksimum işletim sıcaklığı ile derecelendirildi.                                                                                |
|    Maksimum yükseklik, işlemsel olmayan    |    12.000 ölçü (39.370 feet)                                                                                                                                                                                         |
|    Darbe, işlemsel                   |    6 yönde 11 milisaniye için 6 G                                                                                                                                                                         |
|    Darbe, işlemsel olmayan               |    6 yönte 2 milisaniye için 71 G                                                                                                                                                                           |
|    Titreşim, işletimsel               |    0,26 G<sub>RMS</sub> 5 hz Ila 350 Hz rastgele                                                                                                                                                                                     |
|    Titreşim, işlemsel olmayan           |    15 dakika boyunca 1,88 G<sub>RMS</sub> 10 hz Ila 500 Hz (altı tarafta test edilmiştir.)                                                                                                                                                  |
|    Yönlendirme ve bağlama             |    Standart 19 "raf bağlama (1U)                                                                                                                                                                                       |
|    Güvenlik ve onaylar                 |    EN 60950-1:2006 + a1:2010 + a2:2013 + A11:2009 + A12:2011/ıEC 60950-1:2005 ED2 + a1:2009 + a2:2013 EN 62311:2008                                                                                                                                                                       |
|    EMC                                  |    FCC A, ıCES-003 <br>EN 55032:2012/CıSPPR 32:2012  <br>EN 55032:2015/CıSPPR 32:2015  <br>EN 55024:2010 + A1:2015/CıSPPR 24:2010 + A1:2015  <br>EN 61000-3-2:2014/ıEC 61000-3-2:2014 (sınıf D)   <br>EN 61000-3-3:2013/ıEC 61000-3-3:2013                                                                                                                                                                                         |
|    Enerji             |    Komisyon Yönetmeliği (AB) No. 617/2013                                                                                                                                                                                        |
|    RoHS           |    EN 50581:2012                                                                                                                                                                                        |

### <a name="operating-temperature-de-rating-specifications"></a>İşletim sıcaklığı de derecelendirme belirtimleri

|     İşletim sıcaklığı de derecelendirme     |     Çevresel sıcaklık aralığı                                                         |
|--------------------------------------------|------------------------------------------------------------------------------------------|
|    En fazla 35 °C (95 °F)                       |    Maksimum sıcaklık 950 (3.117 ft) üzerinde 1 °C/300 milyon (1 °F/547 ft) azaltılır.    |
|    35 °C ila 40 °C (95 °F ila 104 °F)            |    Maksimum sıcaklık 1 °C/175 milyon (1 °F/319 ft) 950 (3.117 ft) üzerinde azaltılır.    |
|    40 °C ila 45 °C (104 °F/113 °f)           |    Maksimum sıcaklık 1 °C/125 milyon (1 °F/228 ft) 950 (3.117 ft) üzerinde azaltılır.    |

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack kenarını dağıtın](azure-stack-edge-gpu-deploy-prep.md)
