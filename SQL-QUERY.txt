--1. Product isimlerini (`ProductName`) ve birim başına miktar (`QuantityPerUnit`) değerlerini almak için sorgu yazın.
 select product_name, quantity_per_unit from Products;

--2. Ürün Numaralarını (`ProductID`) ve Product isimlerini (`ProductName`) değerlerini almak için sorgu yazın. Artık satılmayan ürünleri (`Discontinued`) filtreleyiniz.
 select product_id , product_name From Products where discontinued = 0;

--3. Durdurulan Ürün Listesini, Ürün kimliği ve ismi (`ProductID`, `ProductName`) değerleriyle almak için bir sorgu yazın.
 select product_id, product_name From Products where discontinued = 1;

--4. Ürünlerin maliyeti 20'dan az olan Ürün listesini (`ProductID`, `ProductName`, `UnitPrice`) almak için bir sorgu yazın.
 select product_id, product_name, unit_price from Products where unit_price < 20;

--5. Ürünlerin maliyetinin 15 ile 25 arasında olduğu Ürün listesini (`ProductID`, `ProductName`, `UnitPrice`) almak için bir sorgu yazın.
 select product_id, product_name, unit_price from Products where unit_price between 15 and 25; 

--6. Ürün listesinin (`ProductName`, `UnitsOnOrder`, `UnitsInStock`) stoğun siparişteki miktardan az olduğunu almak için bir sorgu yazın.
select product_name, units_on_order, units_in_stock from Products where units_in_stock < units_on_order;

--7. İsmi `a` ile başlayan ürünleri listeleyeniz.
 select lower(product_name) from Products where Lower(product_name) like 'a%';

--8. İsmi `i` ile biten ürünleri listeleyeniz.
 select product_name from Products where product_name like '%i';

--9. Ürün birim fiyatlarına %18’lik KDV ekleyerek listesini almak (ProductName, UnitPrice, UnitPriceKDV) için bir sorgu yazın.
select product_name, unit_price, unit_price * 1.18 as unit_price_kdv from Products;

--10. Fiyatı 30 dan büyük kaç ürün var?
 select count(*) from Products where unit_price > 30;

--11. Ürünlerin adını tamamen küçültüp fiyat sırasına göre tersten listele
 select lower(product_name) ,unit_price from Products order by unit_price desc;

--12. Çalışanların ad ve soyadlarını yanyana gelecek şekilde yazdır 
select first_name || ' ' || last_name as full_name from employees;

--13. Region alanı NULL olan kaç tedarikçim var?
 select count(*) from suppliers where region is null;

--14. Null olmayanlar?
 select count(*) from suppliers where region is not null;

--15. Ürün adlarının hepsinin soluna TR koy ve büyültüp olarak ekrana yazdır.
 select concat('TR ', UPPER(product_name)) as new_product_name from Products;

--16. a.Fiyatı 20den küçük ürünlerin adının başına TR ekle
 select concat('TR ', UPPER(product_name)) as new_product_name from Products where unit_price < 20;

--17. En pahalı ürün listesini (`ProductName` , `UnitPrice`) almak için bir sorgu yazın.
 select product_name ,unit_price from Products order by unit_price desc limit 1;

--18. En pahalı on ürünün Ürün listesini (`ProductName` , `UnitPrice`) almak için bir sorgu yazın.
 select product_name ,unit_price from Products order by unit_price desc limit 10;

--19. Ürünlerin ortalama fiyatının üzerindeki Ürün listesini (`ProductName` , `UnitPrice`) almak için bir sorgu yazın.
 select product_name ,unit_price from Products where unit_price > (select avg(unit_price) from Products);

--20. Stokta olan ürünler satıldığında elde edilen miktar ne kadardır.
 select sum(unit_price * units_in_stock) as toplam_gelir from Products where units_in_stock > 0;

--21. Mevcut ve Durdurulan ürünlerin sayılarını almak için bir sorgu yazın.
 select discontinued, units_in_stock from Products where discontinued = 1 and units_in_stock > 0;  

--22. Ürünleri kategori isimleriyle birlikte almak için bir sorgu yazın.
 select products.product_name,categories.category_name from products JOIN categories ON products.category_id = categories.category_id;

--23. Ürünlerin kategorilerine göre fiyat ortalamasını almak için bir sorgu yazın.
 select categories.category_name,AVG(products.unit_price) as avg_price from products JOIN categories on products.category_id = categories.category_id GROUP BY categories.category_name;

--24. En pahalı ürünümün adı, fiyatı ve kategorisin adı nedir?
 select products.product_name,products.unit_price,categories.category_name from products JOIN categories on products.category_id= categories.category_id order by products.unit_price desc LIMIT 1;

--25. En çok satılan ürününün adı, kategorisinin adı ve tedarikçisinin adı
 select products.product_name,categories.category_name,suppliers.company_name from products JOIN categories on products.category_id= categories.category_id JOIN suppliers on products.supplier_id= suppliers.supplier_id order by units_on_order desc LIMIT 1;

--26. Stokta bulunmayan ürünlerin ürün listesiyle birlikte tedarikçilerin ismi ve iletişim numarasını (`ProductID`, `ProductName`, `CompanyName`, `Phone`) 
--almak için bir sorgu yazın
select p.product_id, p.product_name,s.company_name, s.phone from products p
inner join suppliers s on s.supplier_id = p.supplier_id
where p.units_in_stock = 0
order by product_id;

--27. 1998 yılı mart ayındaki siparişlerimin adresi, siparişi alan çalışanın adı, çalışanın soyadı
select o.order_date,o.ship_address, e.first_name || ' ' || e.last_name as "Adı ve Soyadı" from orders o
inner join employees e on e.employee_id = o.employee_id
where extract(month from order_date)= 03 and extract(year from order_date)= 1998;

--28. 1997 yılı şubat ayında kaç siparişim var?
select count(*) as "1997 Şubat ayı Top. Sipariş sayısı" from orders
where extract(month from order_date)= 02 and extract(year from order_date)= 1997;

--29. London şehrinden 1998 yılında kaç siparişim var?
select count(*) as "London şehrinden 1998 yılındaki sipariş sayısı" from orders o
where extract(year from o.order_date)= 1998 and o.ship_city = 'London';

--30. 1997 yılında sipariş veren müşterilerimin contactname ve telefon numarası
select c.contact_name,c.phone  from orders o
inner join customers c on c.customer_id = o.customer_id
where extract(year from o.order_date)= 1997;

--31. Taşıma ücreti 40 üzeri olan siparişlerim
select * from orders
where freight > 40;

--32. Taşıma ücreti 40 ve üzeri olan siparişlerimin şehri, müşterisinin adı
select c.contact_name,o.ship_city from orders o
inner join customers c on c.customer_id = o.customer_id
where o.freight >= 40

--33.1997 yılında verilen siparişlerin tarihi, şehri, çalışan adı -soyadı ( ad soyad birleşik olacak ve büyük harf),
select o.order_date,o.ship_city,upper(e.first_name || ' ' || e.last_name) as "AD-SOYAD" from orders o
inner join employees e on e.employee_id = o.employee_id
where extract(year from order_date) = 1997;

--34.1997 yılında sipariş veren müşterilerin contactname i, ve telefon numaraları ( telefon formatı 2223322 gibi olmalı )
select c.contact_name,regexp_replace(right(c.phone,8), '[^0-9]','','')  from orders o
inner join customers c on c.customer_id = o.customer_id
where extract(year from order_date) = 1997;

--35.Sipariş tarihi, müşteri contact name, çalışan ad, çalışan soyad
select o.order_date,c.contact_name,e.first_name,e.last_name from orders o 
inner join employees e on e.employee_id = o.employee_id
inner join customers c on c.customer_id = o.customer_id

--36. Geciken siparişlerim?
select * from orders
where shipped_date > required_date;

--37.Geciken siparişlerimin tarihi, müşterisinin adı
select o.order_date, c.contact_name from orders o
inner join customers c on c.customer_id = o.customer_id
where shipped_date >required_date

--38. 10248 nolu siparişte satılan ürünlerin adı, kategorisinin adı, adedi
select p.product_name,c.category_name,od.quantity from order_details od
inner join products p on p.product_id = od.product_id
inner join categories c on c.category_id = p.category_id
where od.order_id = 10248;

--39. 10248 nolu siparişin ürünlerinin adı , tedarikçi adı
select p.product_name,s.company_name from order_details od 
inner join products p on p.product_id = od.product_id
inner join suppliers s on s.supplier_id = p.supplier_id
where od.order_id = 10248;

--40. 3 numaralı ID ye sahip çalışanın 1997 yılında sattığı ürünlerin adı ve adeti
select p.product_name,od.quantity from orders o
inner join order_details od on od.order_id = o.order_id
inner join products p on p.product_id = od.product_id
where o.employee_id = 3 and extract(year from o.order_date) = 1997;

--41. 1997 yılında bir defasinda en çok satış yapan çalışanımın ID,Ad soyad
select e.employee_id,e.first_name || ' ' || e.last_name as "Adı ve soyadı", sum(od.quantity*od.unit_price) as "toplam sipariş" from employees e
inner join orders o on o.employee_id = e.employee_id
inner join order_details od on od.order_id = o.order_id
where extract(year from o.order_date) = 1997
group by e.employee_id
order by "toplam sipariş" desc
limit 1;

--42. 1997 yılında en çok satış yapan çalışanımın ID,Ad soyad ****
select e.employee_id, e.first_name || ' ' || e.last_name as "Çalışanın Adı-Soyadı" , count(*) as "sattığı miktar" from order_details od
inner join orders o on o.order_id = od.order_id
inner join employees e on o.employee_id = e.employee_id
where extract(year from o.order_date) = 1997 
group by e.employee_id
order by "sattığı miktar" desc 
limit 1;

--43. En pahalı ürünümün adı,fiyatı ve kategorisin adı nedir?
select p.product_name,p.unit_price,c.category_name from products p
inner join categories c on c.category_id = p.category_id
order by p.unit_price desc
limit 1;

--44. Siparişi alan personelin adı,soyadı, sipariş tarihi, sipariş ID. Sıralama sipariş tarihine göre
select e.first_name || ' ' || e.last_name as "Personelin Adı-Soyadı",o.order_date,o.order_id from orders o
inner join employees e on e.employee_id = o.employee_id
order by o.order_date;

--45. SON 5 siparişimin ortalama fiyatı ve orderid nedir?
select o.order_id, avg(p.unit_price) as "Ortalama fiyat" from orders o
inner join order_details od on od.order_id = o.order_id
inner join products p on p.product_id = od.product_id
group by o.order_id
order by o.order_date desc
limit 5;

--46. Ocak ayında satılan ürünlerimin adı ve kategorisinin adı ve toplam satış miktarı nedir?
select p.product_name,c.category_name, sum(od.quantity) from orders o
inner join order_details od on od.order_id = o.order_id
inner join products p on p.product_id = od.product_id
inner join categories c on c.category_id = p.category_id
where extract(month from o.order_date) = 1
group by p.product_name, c.category_name;

--47. Ortalama satış miktarımın üzerindeki satışlarım nelerdir?
select * from order_details od
inner join orders o on o.order_id = od.order_id
where od.quantity > (select AVG(quantity) from order_details);

--48. En çok satılan ürünümün(adet bazında) adı, kategorisinin adı ve tedarikçisinin adı
SELECT p.product_name, c.category_name, s.company_name FROM order_details od
INNER JOIN products p ON od.product_id = p.product_id
INNER JOIN categories c ON p.category_id = c.category_id
INNER JOIN suppliers s ON p.supplier_id = s.supplier_id
GROUP BY p.product_name, c.category_name, s.company_name
ORDER BY SUM(od.quantity) DESC
LIMIT 1;

--49. Kaç ülkeden müşterim var
select count(distinct(country)) as "Kaç ülkeden Müşterim var" from customers;

--50. 3 numaralı ID ye sahip çalışan (employee) son Ocak ayından BUGÜNE toplamda ne kadarlık ürün sattı?
select sum(od.quantity*od.unit_price) as "toplamda ne kadarlık ürün sattı" from orders o 
inner join order_details od on od.order_id = o.order_id
where o.employee_id = 3 and o.order_date >= date '1998-01-01' and o.order_date <= date '2023-11-13';

--51. 10248 nolu siparişte satılan ürünlerin adı, kategorisinin adı, adedi
select p.product_name,c.category_name,od.quantity from order_details od
inner join products p on p.product_id = od.product_id
inner join categories c on c.category_id = p.category_id
where od.order_id = 10248;

--52. 10248 nolu siparişin ürünlerinin adı , tedarikçi adı
select p.product_name,s.company_name from order_details od 
inner join products p on p.product_id = od.product_id
inner join suppliers s on s.supplier_id = p.supplier_id
where od.order_id = 10248;

--53. 3 numaralı ID ye sahip çalışanın 1997 yılında sattığı ürünlerin adı ve adeti
select p.product_name,od.quantity from orders o
inner join order_details od on od.order_id = o.order_id
inner join products p on p.product_id = od.product_id
where o.employee_id = 3 and extract(year from o.order_date) = 1997;

--54. 1997 yılında bir defasinda en çok satış yapan çalışanımın ID,Ad soyad
select e.employee_id,e.first_name || ' ' || e.last_name as "Adı ve soyadı",sum(od.quantity*od.unit_price) as "toplam sipariş" from employees e
inner join orders o on o.employee_id = e.employee_id
inner join order_details od on od.order_id = o.order_id
where extract(year from o.order_date) = 1997
group by e.employee_id
order by "toplam sipariş" desc
limit 1;

--55. 1997 yılında en çok satış yapan çalışanımın ID,Ad soyad ****
select e.employee_id, e.first_name || ' ' || e.last_name as "Çalışanın Adı-Soyadı" , count(*) as "sattığı miktar" from order_details od
inner join orders o on o.order_id = od.order_id
inner join employees e on o.employee_id = e.employee_id
where extract(year from o.order_date) = 1997 
group by e.employee_id
order by "sattığı miktar" desc 
limit 1;

--56.En pahalı ürünümün adı,fiyatı ve kategorisin adı nedir?
select p.product_name,p.unit_price,c.category_name from products p
inner join categories c on c.category_id = p.category_id
order by p.unit_price desc
limit 1;

--57. Siparişi alan personelin adı,soyadı, sipariş tarihi, sipariş ID. Sıralama sipariş tarihine göre
select e.first_name || ' ' || e.last_name as "Personelin Adı-Soyadı",o.order_date,o.order_id from orders o
inner join employees e on e.employee_id = o.employee_id
order by o.order_date;

--58. SON 5 siparişimin ortalama fiyatı ve order id nedir?
select o.order_id, avg(p.unit_price) as "Ortalama fiyat" from orders o
inner join order_details od on od.order_id = o.order_id
inner join products p on p.product_id = od.product_id
group by o.order_id
order by o.order_date desc
limit 5;

--59. Ocak ayında satılan ürünlerimin adı ve kategorisinin adı ve toplam satış miktarı nedir?
select p.product_name,c.category_name,od.quantity from orders o
inner join order_details od on od.order_id = o.order_id
inner join products p on p.product_id = od.product_id
inner join categories c on c.category_id = p.category_id
where extract(month from o.order_date) = 1;

--60. Ortalama satış miktarımın üzerindeki satışlarım nelerdir?
select * from order_details od
inner join orders o on o.order_id = od.order_id
where od.quantity > (select AVG(quantity) from order_details);

--61. En çok satılan ürünümün(adet bazında) adı, kategorisinin adı ve tedarikçisinin adı
select p.product_name,c.category_name,s.company_name from order_details od
inner join products p on p.product_id = od.product_id
inner join suppliers s on s.supplier_id = p.supplier_id
inner join categories c on c.category_id = p.category_id
order by od.quantity desc
limit 1;

--62. Kaç ülkeden müşterim var
select count(distinct(country)) as "Kaç ülkeden Müşterim var" from customers;

--63. Hangi ülkeden kaç müşterimiz var
select country, count(distinct customer_id) as "kaç müşterimiz var" from customers
group by country;

--64. 3 numaralı ID ye sahip çalışan (employee) son Ocak ayından BUGÜNE toplamda ne kadarlık ürün sattı?
select sum(od.quantity*od.unit_price) as "toplamda ne kadarlık ürün sattı" from orders o 
inner join order_details od on od.order_id = o.order_id
where o.employee_id = 3 and o.order_date >= date '1998-01-01' and o.order_date <= date '2023-11-13';

--65. 10 numaralı ID ye sahip ürünümden son 3 ayda ne kadarlık ciro sağladım?
select sum(od.unit_price * od.quantity) as "3 aylık ciro" from products p
inner join order_details od on od.product_id = p.product_id
inner join orders o on o.order_id = od.order_id
where p.product_id = 10 and o.order_date >= current_date - interval '3 months';

--66. Hangi çalışan şimdiye kadar toplam kaç sipariş almış..?
select e.first_name || ' ' || e.last_name as "Adı ve Soyadı", count(o.order_id) as "Toplam aldığı sipariş sayısı" from orders o
inner join order_details od on od.order_id = o.order_id
inner join employees e on e.employee_id = o.employee_id
group by "Adı ve Soyadı";

--67. 91 müşterim var. Sadece 89’u sipariş vermiş. Sipariş vermeyen 2 kişiyi bulun
select * from customers c
left join orders o on o.customer_id = c.customer_id
where o.order_id is null;

--68. Brazil’de bulunan müşterilerin Şirket Adı, TemsilciAdi, Adres, Şehir, Ülke bilgileri
select c.company_name,c.contact_name,c.address,c.city,c.country from customers c
where c.country = 'Brazil';

--69. Brezilya’da olmayan müşteriler
select * from customers c
where c.country <> 'Brazil';

--70. Ülkesi (Country) YA Spain, Ya France, Ya da Germany olan müşteriler
select * from customers c
where c.country = 'Spain' or c.country = 'France' or c.country = 'Germany';

--71. Faks numarasını bilmediğim müşteriler
select * from customers c
where fax is null;

--72. Londra’da ya da Paris’de bulunan müşterilerim
select * from customers c
where c.city = 'London' or c.city = 'Paris'

--73. Hem Mexico D.F’da ikamet eden HEM DE ContactTitle bilgisi ‘owner’ olan müşteriler
select * from customers c
where c.city = 'México D.F.' and c.contact_title = 'Owner';

--74. C ile başlayan ürünlerimin isimleri ve fiyatları
select p.product_name,p.unit_price from products p
where p.product_name like 'C%';

--75. Adı (FirstName) ‘A’ harfiyle başlayan çalışanların (Employees); Ad, Soyad ve Doğum Tarihleri
select e.first_name , e.last_name ,e.birth_Date from employees e
where e.first_name like 'A%';

--76. İsminde ‘RESTAURANT’ geçen müşterilerimin şirket adları
select c.company_name from customers c
where c.company_name Ilike '%RESTAURANT%';

--77. 50$ ile 100$ arasında bulunan tüm ürünlerin adları ve fiyatları
select p.product_name,p.unit_price from products p 
where unit_price between 50 and 100;

--78. 1 temmuz 1996 ile 31 Aralık 1996 tarihleri arasındaki siparişlerin (Orders), SiparişID (OrderID) ve SiparişTarihi (OrderDate) bilgileri
select o.order_id,o.order_date from orders o
where o.order_date between '1996-07-01' and '1996-12-31';

--79. Ülkesi (Country) YA Spain, Ya France, Ya da Germany olan müşteriler
select * from customers c
where c.country = 'Spain' or c.country = 'France' or c.country = 'Germany';

--80. Faks numarasını bilmediğim müşteriler
select * from customers c
where fax is null;

--81. Müşterilerimi ülkeye göre sıralıyorum:
select * from customers c
order by c.country;

--82. Ürünlerimi en pahalıdan en ucuza doğru sıralama, sonuç olarak ürün adı ve fiyatını istiyoruz
select p.product_name , p.unit_price from products p
order by p.unit_price desc;

--83. Ürünlerimi en pahalıdan en ucuza doğru sıralasın, ama stoklarını küçükten-büyüğe doğru göstersin sonuç olarak ürün adı ve fiyatını istiyoruz
select p.product_name , p.unit_price , p.units_in_stock from products p
order by p.unit_price desc, p.units_in_stock asc;

--84. 1 Numaralı kategoride kaç ürün vardır..?
select count(*) as "ürün sayısı" from products p
where p.category_id = 1;

--85. Kaç farklı ülkeye ihracat yapıyorum..?
select count(distinct country) as "İhracat yapılan ülke sayısı" from customers;

--85. Kaç farklı ülkeye ihracat yapıyorum..?
select count(distinct country) as "Number of export countries" from customers;

--86. a.Bu ülkeler hangileri..?
select distinct country from customers;

--87. En Pahalı 5 ürün?
select product_name, unit_price from Products order by unit_price DESC limit 5;

--88. ALFKI CustomerID’sine sahip müşterimin sipariş sayısı..?
select c.customer_id, count(od.quantity) as quantity_count from customers c
left join orders o on c.customer_id = o.customer_id
left join order_details od on o.order_id = od.order_id
where c.customer_id = 'ALFKI' group by c.customer_id; v

--89. Ürünlerimin toplam maliyeti?
SELECT SUM(unit_price * units_in_stock) AS "Toplam Maliyet" FROM products;

--90. Şirketim, şimdiye kadar ne kadar ciro yapmış..?
select sum((od.unit_price * od.quantity) - (od.unit_price * od.quantity * od.discount)) 
as total_proceeds from order_details od inner join orders o on od.order_id = o.order_id;

--91. Ortalama Ürün Fiyatım?
select avg(unit_price) from products;

--92. En Pahalı Ürünün Adı?
select product_name from products order by unit_price DESC LIMIT 1;

--93. En az kazandıran sipariş?
select order_id, sum(od.unit_price * od.quantity) as least_proceeds_order from order_details od
group by order_id order by least_proceeds_order asc limit 1;

--94. Müşterilerimin içinde en uzun isimli müşteri?
select contact_name from customers order by length(contact_name) desc limit 1;

--95. Çalışanlarımın Ad, Soyad ve Yaşları?
select first_name, last_name, extract(year from age(now(), birth_date)) as age from employees; 

--96. Hangi üründen toplam kaç adet alınmış..?
select p.product_id, p.product_name, sum(od.quantity) as total_number from order_details od
inner join products p on od.product_id = p.product_id group by p.product_id, p.product_name
order by total_number desc;

--97. Hangi siparişte toplam ne kadar kazanmışım..?
select order_id, sum(unit_price * quantity * (1 - discount)) from order_details group by order_id;

--98. Hangi kategoride toplam kaç adet ürün bulunuyor..?
select category_id, count(*) as total_products from products group by category_id;

--99. 1000 Adetten fazla satılan ürünler?
select product_id, sum(quantity) from order_details 
where product_id in (select product_id from order_details group by product_id having sum(quantity) > 1000)
group by product_id;

--100. Hangi Müşterilerim hiç sipariş vermemiş..?
select c.customer_id, c.company_name from customers c
where not exists (select 1 from orders o where c.customer_id = o.customer_id);

--101. Hangi tedarikçi hangi ürünü sağlıyor ?
select s.company_name, p.product_name
from suppliers s, products p
where s.supplier_id = p.supplier_id
order by s.company_name, p.product_name;

--102. Hangi sipariş hangi kargo şirketi ile ne zaman gönderilmiş..?
select o.order_id, sh.company_name, o.shipped_date as sending_date
from orders o inner join shippers sh on o.ship_via = sh.shipper_id;

--103. Hangi siparişi hangi müşteri verir..?
select o.order_id, c.company_name from orders o
join customers c on o.customer_id = c.customer_id;

--104. Hangi çalışan, toplam kaç sipariş almış..?
select e.employee_id, e.first_name, e.last_name, count(o.order_id) as total_order_count from employees e
left join orders o on e.employee_id = o.employee_id
group by e.employee_id, e.first_name, e.last_name
order by total_order_count desc;

--105. En fazla siparişi kim almış..?
select e.employee_id, e.first_name, e.last_name, count(o.order_id) as total_order_count from employees e
join orders o on e.employee_id = o.employee_id
group by e.employee_id, e.first_name, e.last_name
order by total_order_count desc limit 1;

--106. Hangi siparişi, hangi çalışan, hangi müşteri vermiştir..?
select o.order_id, concat(e.first_name , ' ' , e.last_name) as employeer_name, c.company_name from orders o
inner join customers c on o.customer_id = c.customer_id
inner join employees e on o.employee_id = e.employee_id;

--107. Hangi ürün, hangi kategoride bulunmaktadır..? Bu ürünü kim tedarik etmektedir..?
select p.product_name, c.category_name, s.company_name from products p
join categories c on p.category_id = c.category_id
join suppliers s on p.supplier_id = s.supplier_id;

--108. Hangi siparişi hangi müşteri vermiş, hangi çalışan almış, hangi tarihte, hangi kargo şirketi tarafından gönderilmiş hangi üründen kaç adet alınmış, hangi fiyattan alınmış, ürün hangi kategorideymiş bu ürünü hangi tedarikçi sağlamış?
select o.order_id,
       cu.customer_id,
       CONCAT(cu.contact_name, ' ', cu.contact_title),
       e.employee_id,
       CONCAT(e.first_name, ' ', e.last_name),
       o.order_date,
       s.company_name,
       p.product_name,
       od.quantity,
       od.unit_price,
       c.category_name,
       sup.company_name FROM orders o
join customers cu on o.customer_id = cu.customer_id
join employees e on o.employee_id = e.employee_id
join shippers s on o.ship_via = s.shipper_id
join order_details od on o.order_id = od.order_id
join products p on od.product_id = p.product_id
join categories c on p.category_id = c.category_id
join suppliers sup on p.supplier_id = sup.supplier_id;
 
--109. Altında ürün bulunmayan kategoriler?
select c.category_id, c.category_name from categories c
left join products p on c.category_id = p.category_id
where p.product_id is NULL;

--110. Manager ünvanına sahip tüm müşterileri listeleyiniz?
select customer_id, company_name,contact_name from customers where contact_title Ilike '%Manager%';

--111. FR ile başlayan 5 karekter olan tüm müşterileri listeleyiniz?
select customer_id, company_name, contact_name from customers
where customer_id like 'FR___%'; -- and length(customer_id) = 5;

--112. (171) alan kodlu telefon numarasına sahip müşterileri listeleyiniz?
select customer_id, company_name, contact_name, phone from customers
where phone like '%(171)%';

--113. BirimdekiMiktar alanında boxes geçen tüm ürünleri listeleyiniz?
select product_id, product_name, quantity_per_unit from products
where quantity_per_unit like '%boxes%';

--114. Fransa ve Almanyadaki (France,Germany) Müdürlerin (Manager) Adını ve Telefonunu listeleyiniz.(MusteriAdi,Telefon)?
select c.contact_name, c.phone from customers c
where c.country in ('France', 'Germany') and c.contact_title like '%Manager%';

--115. En yüksek birim fiyata sahip 10 ürünü listeleyiniz?
select * from products order by unit_price desc limit 10;

--116. Müşterileri ülke ve şehir bilgisine göre sıralayıp listeleyiniz?
select * from customers order by country, city;

--117. Personellerin ad,soyad ve yaş bilgilerini listeleyiniz?
select first_name, last_name, extract(year from age(now(), birth_date)) as age from employees;

--118. 35 gün içinde sevk edilmeyen satışları listeleyiniz?
select * from orders where shipped_date is NULL or (order_date - shipped_date) > 35;

--119. Birim fiyatı en yüksek olan ürünün kategori adını listeleyiniz? (Alt Sorgu)
select category_name from categories c
where exists ( select category_id from products p where p.category_id = c.category_id
order by unit_price desc) limit 1;

--120. Kategori adında 'on' geçen kategorilerin ürünlerini listeleyiniz? (Alt Sorgu)
SELECT product_name FROM products p
WHERE EXISTS ( SELECT * FROM categories c WHERE p.category_id = c.category_id
AND c.category_name LIKE '%on%');

--121. Konbu adlı üründen kaç adet satılmıştır?
select sum(quantity) as total_sold from order_details
where product_id = (select product_id from products
where product_name = 'Konbu');
					
--122. Japonyadan kaç farklı ürün tedarik edilmektedir?
select count(distinct p.product_id) from products p
join suppliers s ON p.supplier_id = s.supplier_id
where s.country = 'Japan';

--123. 1997 yılında yapılmış satışların en yüksek, en düşük ve ortalama nakliye ücretlisi ne kadardır?
select max(freight) as maximum_fee, min(freight) as minimum_fee, avg(freight) as average_fee
from orders where extract(year from order_date) = 1997;

--124. Faks numarası olan tüm müşterileri listeleyiniz?
select * from customers where fax is not NULL;

--125. 1996-07-16 ile 1996-07-30 arasında sevk edilen satışları listeleyiniz?
select order_id, customer_id, order_date, shipped_date from orders
where shipped_date between '1996-07-16' and '1996-07-30';