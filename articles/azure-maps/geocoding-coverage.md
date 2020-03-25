---
title: Coğrafi kodlama kapsamı | Microsoft Azure haritaları
description: Konum adresini Enlem ve boylam koordinatları olarak dönüştürme işlemi, coğrafi kodlama olarak bilinir. Bu makalede, Microsoft Azure Eşlemlerde coğrafi kodlama kapsamına sahip bölgeler hakkında bilgi edineceksiniz.
author: farah-alyasari
ms.author: v-faalya
ms.date: 12/31/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 149dd0a3e709bbf4660aaae176f76f560de873e2
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77210064"
---
# <a name="azure-maps-geocoding-coverage"></a>Azure haritalar coğrafi kodlama kapsamı

Azure haritalar ile bir konum aradığınızda Arama hizmeti, örneğin [arama adresi al](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress), arama terimlerinizi alır ve enlem ve boylam koordinatlarını döndürür. Bu işleme geokodlamaya denir. Ancak, Azure Maps tüm bölgeler ve ülkeler için aynı düzeyde bilgi ve doğruluk düzeyine sahip değildir. Her bölgede güvenilir bir şekilde arama yapmak istediğiniz konum türlerini belirlemek için bu makaleyi kullanın. 

Bir ülkede/bölgede kodlamayı özelliği, yol veri kapsamına ve coğrafi kodlama hizmetinin coğrafi kodlama duyarlığına bağımlıdır. Aşağıdaki sınıflandırmalar, her ülkede/bölgede coğrafi kodlama desteğinin düzeyini belirler.
* **Adres noktaları** -adresler verileri, adres alanı (özellik sınırı) içindeki bir Enlem/Boylam koordinatına çözümlenebilirler. Bazen ' Rooftop ' doğru olarak adlandırılır. Bu, adresler için kullanılabilen en yüksek doğruluk düzeyidir. 
* **Ev numaraları** -adresler cadde üzerinde bir enlem/boylam koordinatı ile yapılır.
* **Cadde düzeyi** -adresler, adresi içeren sokak 'nin Enlem/Boylam koordinatı olarak çözümlenir. Evin numarası işlenemiyor olabilir.
* **Şehir düzeyi** -şehir yeri adları desteklenir.

## <a name="americas"></a>Kuzey ve Güney Amerika

| Ülke/Bölge                                       | Adres noktaları | Ev numaraları | Cadde düzeyi | Şehir düzeyi | İlgi çekici noktaları |
|-----------------------------------------------------|:---------------:|:--------------:|:------------:|:----------:|:------------------:|
| Anguilla                                            |                 |                |              |      ✓     |          ✓         |
| Antarktika                                          |                 |                |              |      ✓     |          ✓         |
| Antigua ve Barbuda                                 |                 |                |       ✓      |      ✓     |          ✓         |
| Arjantin                                           |       ✓         |        ✓       |       ✓      |      ✓     |          ✓         |
| Aruba                                               |                 |                |              |      ✓     |          ✓         |
| Bahamalar                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Barbados                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Beliz                                              |                 |                |              |      ✓     |          ✓         |
| Bermuda                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Bolivya                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Bonaire, Sint Eustatius ve Saba                   |                 |                |              |      ✓     |          ✓         |
| Brezilya                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Kanada                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Kayman Adaları                                      |                 |                |       ✓      |      ✓     |          ✓         |
| Şili                                               |       ✓         |        ✓       |       ✓      |      ✓     |          ✓         |
| Kolombiya                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Kosta Rika                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Küba                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Dominika                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Dominicana                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Ekvador                                             |                 |                |       ✓      |      ✓     |          ✓         |
| El Salvador                                         |                 |                |       ✓      |      ✓     |          ✓         |
| Falkland Adaları                                    |                 |                |              |      ✓     |          ✓         |
| Fransız Ginesi                                       |                 |                |       ✓      |      ✓     |          ✓         |
| Grenada                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Guadalupe                                          |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Guam                                                |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Guatemala                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Guyana                                              |                |             |           |      ✓     |                 |
| Haiti                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Honduras                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Jamaika                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Martinik                                          |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Meksika                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Montserrat                                          |                 |                |              |      ✓     |          ✓         |
| Nikaragua                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Panama                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Paraguay                                            |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Peru                                                |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Porto Riko                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Saint Barthélimy                                    |                 |                |       ✓      |      ✓     |          ✓         |
| Saint Kitts ve Nevis                               |                 |                |       ✓      |      ✓     |          ✓         |
| Saint Lucia                                         |                 |                |              |      ✓     |          ✓         |
| Saint Marmara                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Saint Pierre ve Miquelon                           |                 |                |       ✓      |      ✓     |          ✓         |
| Saint Vincent ve Grenadinler                    |                 |                |              |      ✓     |          ✓         |
| Sint Maarten                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Güney Georgia ve Güney Sandwich Adaları        |                 |                |              |      ✓     |          ✓         |
| Surinam                                            |                 |                |              |      ✓     |          ✓         |
| Trinidad ve Tobago                                 |                 |                |       ✓      |      ✓     |          ✓         |
| Birleşik Devletler küçük harici Adaları                |                 |                |              |      ✓     |          ✓         |
| Amerika Birleşik Devletleri                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Uruguay                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Venezuela                                           |                 |                |       ✓      |      ✓     |          ✓         |
| İngiliz Virgin Adaları                              |                 |                |              |      ✓     |          ✓         |
| ABD Virgin Adaları                                 |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |

## <a name="asia-pacific"></a>Asya Pasifik

| Ülke/Bölge                                      | Adres noktaları |Ev numaraları | Cadde düzeyi | Şehir düzeyi | İlgi çekici noktaları |
|-----------------------------------------------------|:---------------:|:--------------:|:------------:|:----------:|:------------------:|
| Amerikan Samoası                                      |                 |                |       ✓      |      ✓     |          ✓         |
| Avustralya                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Bangladeş                                          |                 |                |              |      ✓     |          ✓         |
| Butan                                              |                 |                |              |      ✓     |          ✓         |
| İngiliz Hint Okyanusu Toprakları                      |                 |                |              |      ✓     |          ✓         |
| Barış                                              |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Kamboçya                                            |                 |                |              |      ✓     |          ✓         |
| Çin                                               |                 |                |              |      ✓     |          ✓         |
| Christmas Adası                                    |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Cocos (Keeling) Adaları                             |                 |                |              |      ✓     |          ✓         |
| Komorolar                                             |                 |                |              |      ✓     |          ✓         |
| Cook Adaları                                        |                 |                |              |      ✓     |          ✓         |
| Fiji                                                |                  |                |              |      ✓     |          ✓        |
| Fransız Polinezyası                                    |                 |                |              |      ✓     |          ✓         |
| Heard Adası ve McDonald Adaları                   |                 |                |              |      ✓     |          ✓         |
| Hong Kong SAR                                       |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Endonezya                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Hindistan                                               |        ✓        |        ✓       |       ✓      |      ✓     |                   |
| Japonya                                               |                 |                |              |      ✓     |          ✓         |
| Kiribati                                            |                 |                |              |      ✓     |          ✓         |
| Güney Kore                                         |                 |                |              |      ✓     |          ✓         |
| Laos                                                |                 |                |              |      ✓     |          ✓         |
| Makao ÖİB                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Malezya                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Mikronezya                                          |                 |                |              |      ✓     |          ✓         |
| Moğolistan                                            |                 |                |              |      ✓     |          ✓         |
| Nauru                                               |                 |                |              |      ✓     |          ✓         |
| Nepal                                               |                 |                |              |      ✓     |          ✓         |
| Yeni Kaledonya                                       |                 |                |              |      ✓     |          ✓         |
| Yeni Zelanda                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Niue                                                |                 |                |              |      ✓     |          ✓         |
| Norfolk Adası                                      |                 |                |              |      ✓     |          ✓         |
| Kuzey Kore                                         |                 |                |              |      ✓     |          ✓         |
| Kuzey Mariana Adaları                            |                 |                |       ✓      |      ✓     |          ✓         |
| Pakistan                                            |                 |                |              |      ✓     |          ✓         |
| Palau                                               |                 |                |              |      ✓     |          ✓         |
| Papua Yeni Gine                                    |                 |                |              |      ✓     |          ✓         |
| Filipinler                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Pitcairn                                            |                 |                |              |      ✓     |          ✓         |
| Devleti                                               |                 |                |              |      ✓     |          ✓         |
| Senkaku Adaları                                     |        ✓        |                |              |      ✓     |          ✓         |
| Singapur                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Solomon Adaları                                     |                 |                |              |      ✓     |          ✓         |
| Güney Küıls                                     |        ✓        |                |              |      ✓     |          ✓         |
| Sri Lanka                                           |                 |                |              |      ✓     |          ✓         |
| Tayvan                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Tayland                                            |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Tokelau                                             |                 |                |              |      ✓     |          ✓         |
| Tonga                                               |                 |                |              |      ✓     |          ✓         |
| Turks ve Caicos Adaları                            |                 |                |              |      ✓     |          ✓         |
| Tuvalu                                              |                 |                |              |      ✓     |          ✓         |
| Vanuatu                                             |                 |                |              |      ✓     |          ✓         |
| Vietnam                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Wallis ve Futuna                                   |                 |                |              |      ✓     |          ✓         |

## <a name="europe"></a>Avrupa

| Ülke/Bölge                                      | Adres noktaları |Ev numaraları | Cadde düzeyi | Şehir düzeyi | İlgi çekici noktaları |
|-----------------------------------------------------|:---------------:|:--------------:|:------------:|:----------:|:------------------:|
| Arnavutluk                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Andorra                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Ermenistan                                             |        ✓        |        ✓       |              |      ✓     |          ✓         |
| Avusturya                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Azerbaycan                                          |        ✓        |        ✓       |              |      ✓     |          ✓         |
| Belçika                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Bosna Hersek                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Bulgaristan                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Belarus                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Hırvatistan                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Kıbrıs                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Çek Cumhuriyeti                                      |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Danimarka                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Estonya                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Faroe Adaları                                       |                 |                |              |      ✓     |          ✓         |
| Finlandiya                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Fransa                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Gürcistan                                             |        ✓        |        ✓       |              |      ✓     |          ✓         |
| Almanya                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Cebelitarık                                           |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Yunanistan                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Grönland                                           |                 |                |              |      ✓     |          ✓         |
| Guernsey                                            |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Macaristan                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| İzlanda                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| İrlanda                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Man Adası                                         |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| İtalya                                               |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Jan Mayen                                           |        ✓        |                |              |      ✓     |          ✓         |
| Jersey                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Kazakistan                                          |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Kosova                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Kırgızistan                                          |                 |                |              |      ✓     |          ✓         |
| Letonya                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Lihtenştayn                                       |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Litvanya                                           |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Lüksemburg                                          |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Kuzey Makedonya                                     |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Malta                                               |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Moldova                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Monako                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Karadağ                                          |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Hollanda                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Norveç                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Polonya                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Portekiz                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| \+ Azor Adaları ve Madeira                                 |                 |                |       ✓      |      ✓     |          ✓         |
| Romanya                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Rusya Federasyonu                                  |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| San Marino                                          |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Sırbistan                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Slovakya                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Slovenya                                            |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| İspanya                                               |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Svalbard                                            |        ✓        |                |       ✓      |      ✓     |          ✓         |
| İsveç                                              |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| İsviçre                                         |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Tacikistan                                          |                 |                |              |      ✓     |          ✓         |
| Türkiye                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Türkmenistan                                        |                 |                |              |      ✓     |          ✓         |
| Ukrayna                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Birleşik Krallık                                      |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Özbekistan                                          |                 |                |              |      ✓     |          ✓         |
| Vatikan şehri                                        |                 |                |       ✓      |      ✓     |          ✓         |


## <a name="middle-east-and-africa"></a>Orta Doğu ve Afrika

| Ülke/Bölge                                      | Adres noktaları |Ev numaraları | Cadde düzeyi | Şehir düzeyi | İlgi çekici noktaları |
|-----------------------------------------------------|:---------------:|:--------------:|:------------:|:----------:|:------------------:|
| Afganistan                                         |                 |                |              |      ✓     |          ✓         |
| Cezayir                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Angola                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Bahreyn                                             |        ✓        |       ✓        |       ✓      |      ✓     |          ✓         |
| Benin                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Botsvana                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Bouvet Adası                                       |                 |                |              |      ✓     |          ✓         |
| Burkina Faso                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Burundi                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Kamerun                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Cabo Verde                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Orta Afrika Cumhuriyeti                            |                 |                |       ✓      |      ✓     |          ✓         |
| Çad                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Kongo Cumhuriyeti                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Fildişi Sahili                                       |                 |                |       ✓      |      ✓     |          ✓         |
| Kongo Demokratik Cumhuriyeti                    |                 |                |       ✓      |      ✓     |          ✓         |
| Cibuti                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Mısır                                               |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Ekvator Ginesi, Cumhuriyeti                      |                 |                |       ✓      |      ✓     |          ✓         |
| Eritre                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Etiyopya                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Fransız Güney toprakları|                        |                |              |      ✓     |          ✓         |
| Gabon                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Gambiya                                              |                 |                |              |      ✓     |          ✓         |
| Gana                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Gine                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Gine Bissau                                       |                 |                |       ✓      |      ✓     |          ✓         |
| İran                                                |                 |                |              |      ✓     |          ✓         |
| Irak                                                |                 |                |       ✓      |      ✓     |          ✓         |
| İsrail                                              |        ✓        |       ✓        |              |      ✓     |          ✓         |
| Ürdün                                              |        ✓        |       ✓        |       ✓      |      ✓     |          ✓         |
| Kenya                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Kuveyt                                              |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Lübnan                                             |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Lesoto                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Liberya                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Libya                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Madagaskar                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Malavi                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Maldivler                                            |                 |                |              |      ✓     |          ✓         |
| Mali                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Marshall Adaları                                    |                 |                |              |      ✓     |          ✓         |
| Moritanya                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Mauritius                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Mayotte                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Fas                                             |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Mozambik                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Myanmar                                             |                 |                |              |      ✓     |          ✓         |
| Namibya                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Nijer                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Nijerya                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Umman                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Katar                                               |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Reunion                                             |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Rwanda                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Saint Helena                                        |                 |                |              |      ✓     |          ✓         |
| Suudi Arabistan                                        |                 |        ✓       |       ✓      |      ✓     |          ✓         |
| Senegal                                             |                 |                |       ✓      |      ✓     |          ✓         |
| Seyş                                          |                 |                |       ✓      |      ✓     |          ✓         |
| Sierra Taone                                        |                 |                |       ✓      |      ✓     |          ✓         |
| Somali                                             |                 |                |              |      ✓     |          ✓         |
| Güney Afrika                                        |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Güney Sudan                                         |                 |                |       ✓      |      ✓     |          ✓         |
| Sudan                                               |                 |                |       ✓      |      ✓     |          ✓         |
| Svaziland                                           |                 |                |       ✓      |      ✓     |          ✓         |
| Suriye                                               |                 |                |              |      ✓     |          ✓         |
| Sao Tome ve Principe                               |                 |                |       ✓      |      ✓     |          ✓         |
| Tanzanya                                            |                 |                |       ✓      |      ✓     |          ✓         |
| Togo                                                |                 |                |       ✓      |      ✓     |          ✓         |
| Tunus                                             |        ✓        |                |       ✓      |      ✓     |          ✓         |
| Uganda                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Birleşik Arap Emirlikleri                                |        ✓        |        ✓       |       ✓      |      ✓     |          ✓         |
| Yemen                                               |                 |                |              |      ✓     |          ✓         |
| Zambiya                                              |                 |                |       ✓      |      ✓     |          ✓         |
| Zimbabve                                            |                 |                |       ✓      |      ✓     |          ✓         |



## <a name="next-steps"></a>Sonraki adımlar

Azure haritalar coğrafi kodlama hakkında daha fazla bilgi için bkz. [arama](https://docs.microsoft.com/rest/api/maps/search) başvuru sayfaları.

[Haritalar trafik hizmeti için kapsam bölgeleri](traffic-coverage.md)hakkında bilgi edinin. 

