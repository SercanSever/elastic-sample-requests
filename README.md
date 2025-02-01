**Elasticsearch Query Collection**
----------------------------------

Bu doküman, Elasticsearch ile temel CRUD işlemleri, indeks yönetimi ve sorgu teknikleri hakkında kapsamlı bir referans sunmaktadır.

**1\. Temel CRUD İşlemleri**

**1.1 Belirli Bir Dokümanı Kimlik (ID) ile Getirme**

Aşağıdaki sorgu, belirtilen products indeksindeki 1 kimliğine sahip dokümanı getirir.
```
GET products/\_doc/1
```
**1.2 Bir Dokümanın Sadece İçeriğini Getirme**

Bu sorgu, belirtilen dokümanın sadece içeriğini döndürür, metaveri içermez.
```
GET products/\_source/1
```
**1.3 Belirli Bir İndeksin Shard Bilgilerini Getirme**

Bu sorgu, belirtilen indeksin shard (bölüm) yapılandırmasını görüntüler.
```
GET \_cat/shards/products
```
**1.4 Doküman Oluşturma veya Güncelleme (Refresh Kapatılmış)**

Eğer 20 ID'li bir doküman yoksa, oluşturur; varsa günceller.
```
PUT products/\_doc/20?refresh=false

{

"name": "Iphone 11"

}
```
**1.5 Yeni Bir Doküman Oluşturma (Otomatik ID ile)**

Bu sorgu, rastgele bir kimlik (ID) atayarak yeni bir doküman ekler.
```
POST products/\_doc

{

"name": "Iphone 11"

}
```
**1.6 Belirli ID'ye Sahip Dokümanı Güncelleme**

Var olan dokümanı, belirtilen ID ile günceller.
```
PUT products/\_doc/25

{

"name": "Iphone 11",

"phonePrice": "15000"

}
```
**1.7 İndeks Ayarlarını Güncelleme**

İndeksin refresh\_interval ayarını değiştirerek, belirli bir zaman aralığında otomatik güncellenmesini sağlar.
```
PUT products/\_settings

{

"index": {"refresh\_interval": "5s"}

}
```
**1.8 Dokümanı Kısmi Olarak Güncelleme**

Bu sorgu, sadece belirtilen alanları günceller, mevcut diğer alanlara dokunmaz.
```
POST products/\_update/20

{

"doc": {

"name": "iphone 12"

}

}
```
**1.9 Dokümanı ID ile Silme**

Belirtilen ID'ye sahip dokümanı siler.
```
DELETE products/\_doc/25
```
**1.10 Belirtilen Dokümanın Var Olup Olmadığını Kontrol Etme**

Bu sorgu, 25 ID'li dokümanın var olup olmadığını kontrol eder.
```
HEAD products/\_doc/25
```
2\. Arama (Search) Sorguları
----------------------------

**2.1 Tüm Dokümanları Getirme**

Elasticsearch'teki tüm dokümanları döndürür.
```
GET products/\_search

{

"query": {"match\_all": {}}

}
```
**2.2 Birden Fazla Dokümanı ID ile Getirme**

Bu sorgu, belirtilen ID değerlerine sahip dokümanları getirir.
```
GET products/\_mget

{

"ids": \[20, "8dkqKJEBwI2OvwcgXNv7"\]

}
```
**2.3 Doküman İçeriğini ID ile Getirme**

Bu sorgu, belirtilen dokümanın sadece içeriğini getirir.
```
GET products/\_source/20
```
**2.4 Belirli Alanları Getirme**

Sadece belirtilen alanları (name ve phonePrice) döndürür.
```
GET products/\_doc/25?\_source\_includes=name,phonePrice
```
3\. İndeks Yönetimi
-------------------

**3.1 Yeni Bir İndeks Tanımlama ve Mapping Belirleme**

Bu sorgu, products indeksini oluşturur ve alan yapılandırmasını tanımlar.
```
PUT products

{

"mappings": {

"properties": {

"name": {"type": "text"},

"price": {"type": "long"},

"stock\_no": {"type": "keyword"},

"warehouse": {

"properties": {

"germany": {"type": "integer"},

"turkey": {"type": "integer"}

}

}

}

}

}
```
**3.2 Yeni Bir Alan Ekleyerek Mapping Güncelleme**

Var olan bir indekse yeni bir alan (color) ekler.
```
PUT products/\_mapping

{

"properties": {

"color": {"type": "keyword"}

}

}
```
**3.3 Var Olan Bir Alanı Multi-Field Yapısıyla Güncelleme**

Bir alanın hem text hem de keyword türlerinde kullanılmasını sağlar.
```
PUT products/\_mapping

{

"properties": {

"name": {

"type": "text",

"fields": {

"keyword": {"type": "keyword"}

}

}

}

}
```
**3.4 Bir İndeksin Mapping Bilgilerini Getirme**

Bu sorgu, products indeksinin mevcut mapping yapılandırmasını getirir.
```
GET products/\_mapping
```
4\. Gelişmiş Arama Sorguları
----------------------------

**4.1 Belirli Bir Değer ile Arama (Term Query)**

Belirtilen name alanında tam eşleşme araması yapar.
```
GET product/\_search

{

"query": {

"term": {

"name": {"value": "kalem 1"}

}

}

}
```
**4.2 Wildcard (Joker Karakter) Kullanarak Arama**

Belirtilen kelimenin sonuna "\*" koyarak arama yapar.
```
GET kibana\_sample\_data\_ecommerce/\_search

{

"query": {

"wildcard": {

"customer\_full\_name.keyword": {

"value": "\* Perkins"

}

}

}

}
```
**4.3 Fuzzy (Yaklaşık) Eşleşme Araması**

Benzer kelimeleri bulmak için belirli bir hata toleransı (fuzziness) ile arama yapar.
```
GET kibana\_sample\_data\_ecommerce/\_search

{

"query": {

"fuzzy": {

"customer\_first\_name.keyword": {

"value": "ssdie",

"fuzziness": 2

}

}

}

}
```
Bu dokümanda Elasticsearch ile temel CRUD işlemleri, indeks yönetimi ve arama sorguları detaylandırılmıştır. Bu sorgular, Elasticsearch veritabanınızda etkin bir şekilde veri yönetimi yapmanızı sağlar.
