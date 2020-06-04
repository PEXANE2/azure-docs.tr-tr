---
title: Yeni MySQL IP 'Leri ile Azure Databricks yönlendirme tablolarınızı ve güvenlik duvarlarını güncelleştirme
description: Azure Databricks yönlendirme tablolarınızı ve güvenlik duvarlarını yeni MySQL IP adresleriyle nasıl güncelleştireceğinizi öğrenin.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/02/2020
ms.openlocfilehash: 5bd4822c21e6d9dbe8f3534dd0171a6f785bd5ac
ms.sourcegitcommit: 58ff2addf1ffa32d529ee9661bbef8fbae3cddec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84325918"
---
# <a name="update-your-azure-databricks-route-tables-and-firewalls-with-new-mysql-ips"></a>Yeni MySQL IP 'Leri ile Azure Databricks yönlendirme tablolarınızı ve güvenlik duvarlarını güncelleştirme

MySQL için Azure veritabanı 'nın Azure Databricks meta veri deposu olarak kullanılan tüm IP adresleri değişiyor. Azure Databricks yönlendirme tablolarınızı veya güvenlik duvarlarını 30 Haziran 2020 tarihinden önce yeni MySQL IP 'Leri ile güncelleştirin ve kesintiye uğramasını önleyin.

Kendi sanal ağınıza dağıtılan bir Azure Databricks çalışma alanı, bağlı bir [Rota tablosuna](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/udr#--metastore-artifact-blob-storage-log-blob-storage-and-event-hub-endpoint-ip-addresses) sahiptir. Yol tablosu doğrudan bir Azure Databricks meta veri deposu IP adresi veya adresi bir güvenlik duvarı ya da bir ara sunucu gereci yoluyla, adresin beyaz listelenmesi olabilir.

## <a name="recommended-actions"></a>Önerilen Eylemler

Hizmetinizin kesintiye uğramaması için, bu eylemleri 30 Haziran 2020 ' den önce gözden geçirin ve uygulayın.

* Yol tablonuzun Azure MySQL IP adresi güncelleştirmesinde bir değişiklikten etkilenip etkilenmediğini denetleyin.

* Yeni IP adreslerini aramak için aşağıdaki bölümdeki tabloyu kullanın. Her bölge için, MySQL için Azure veritabanı ağ geçitleri için birincil ve ikincil IP 'Ler olarak kullanılan birden fazla IP adresi olabilir. Birincil IP adresi, ağ geçidinin geçerli IP adresidir ve ikinci IP adresleri, birincil hata durumunda yük devretme IP adresleridir. Hizmet durumunu sağlamak için, tüm IP adreslerine giden gidene izin vermeniz gerekir. Dış bir meta veri [deposu](https://docs.microsoft.com/azure/databricks/data/metastores/external-hive-metastore)kullanıyorsanız, lütfen Azure MySQL bildirimleri başına GEÇERLI bir IP 'nin yönlendirilmekte veya beyaz listelenmiş olduğundan emin olun.

* Yönlendirme tablosu, güvenlik duvarı veya proxy gerecini yeni IP 'Ler ile güncelleştirin.

## <a name="updated-mysql-ip-addresses"></a>MySQL IP adresleri güncelleştirildi

Aşağıdaki tablo, beyaz listelenmesi gereken tüm IP adreslerini içerir. **Kalın** ' daki IP ADRESLERI yeni IP adresleridir. 

| Bölge adı          | Ağ geçidi IP adresleri                                                                                       |
| -------------------- | ---------------------------------------------------------------------------------------------------------- |
| Orta Avustralya    | 20.36.105.0                                                                                                |
| Orta Avustralya 2  | 20.36.113.0                                                                                                |
| Doğu Avustralya       | 13.75.149.87<br><br>40.79.161.1                                                                            |
| Avustralya Güneydoğu | 191.239.192.109<br><br>13.73.109.251                                                                       |
| Güney Brezilya         | 104.41.11.5 <br><br> 191.233.201.8 <br><br> **191.233.200.16**                                             |
| Orta Kanada       | 40.85.224.249                                                                                              |
| Doğu Kanada          | 40.86.226.166                                                                                              |
| Orta ABD           | 23.99.160.139<br><br>13.67.215.62<br><br>**52.182.136.37**<br><br>**52.182.136.38**                        |
| Doğu Çin           | 139.219.130.35                                                                                             |
| Çin Doğu 2         | 40.73.82.1                                                                                                 |
| Kuzey Çin          | 139.219.15.17                                                                                              |
| Çin Kuzey 2        | 40.73.50.0                                                                                                 |
| Doğu Asya            | 191.234.2.139<br><br>52.175.33.150<br><br>**13.75.33.20**<br><br>**13.75.33.21**                           |
| Doğu ABD              | 40.121.158.30<br>191.238.6.43                                                                              |
| Doğu ABD 2            | 40.79.84.180<br><br>191.239.224.107<br><br>52.177.185.181<br><br>**40.70.144.38**<br><br>**52.167.105.38** |
| Orta Fransa       | 40.79.137.0<br><br>40.79.129.1                                                                             |
| Orta Almanya      | 51.4.144.100                                                                                               |
| Almanya Kuzey Doğu   | 51.5.144.179                                                                                               |
| Hindistan Orta        | 104.211.96.159                                                                                             |
| Hindistan Güney          | 104.211.224.146                                                                                            |
| Hindistan Batı           | 104.211.160.80                                                                                             |
| Doğu Japonya           | 13.78.61.196<br><br>191.237.240.43                                                                         |
| Batı Japonya           | 104.214.148.156<br><br>191.238.68.11<br><br>**40.74.96.6**<br><br>**40.74.96.7**                           |
| Güney Kore - Orta        | 52.231.32.42                                                                                               |
| Güney Kore - Güney          | 52.231.200.86                                                                                              |
| Orta Kuzey ABD     | 23.96.178.199<br><br>23.98.55.75<br><br>**52.162.104.35**<br><br>**52.162.104.36**                         |
| Kuzey Avrupa         | 40.113.93.91<br><br>191.235.193.75<br><br>**52.138.224.6**<br><br>**52.138.224.7**                         |
| Güney Afrika Kuzey   | 102.133.152.0                                                                                              |
| Güney Afrika Batı    | 102.133.24.0                                                                                               |
| Orta Güney ABD     | 13.66.62.124<br><br>23.98.162.75<br><br>**104.214.16.39**<br><br>**20.45.120.0**                           |
| Güneydoğu Asya      | 104.43.15.0<br><br>23.100.117.95<br><br>**40.78.233.2**<br><br>**23.98.80.12**                             |
| BAE Orta          | 20.37.72.64                                                                                                |
| BAE Kuzey            | 65.52.248.0                                                                                                |
| Güney Birleşik Krallık             | 51.140.184.11                                                                                              |
| Batı Birleşik Krallık              | 51.141.8.11                                                                                                |
| Orta Batı ABD      | 13.78.145.25                                                                                               |
| Batı Avrupa          | 40.68.37.158<br><br>191.237.232.75<br><br>**13.69.105.208**                                                |
| Batı ABD              | 104.42.238.205<br><br>23.99.34.75                                                                          |
| Batı ABD 2            | 13.66.226.202                                                                                              |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure sanal ağınızda Azure Databricks dağıtma (VNet ekleme)](https://docs.microsoft.com/azure/databricks/administration-guide/cloud-configurations/azure/vnet-inject)
* [Dış Apache Hive meta veri deposu](https://docs.microsoft.com/azure/databricks/data/metastores/external-hive-metastore)
