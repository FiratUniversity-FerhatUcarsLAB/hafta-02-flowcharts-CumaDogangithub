1. Başla

2. Kullanıcının sepetini yükle (üye girişi yaptıysa kullanıcı hesabından, misafir ise geçici token ile).

3. Ürün ekleme işlemi:
 a. Kullanıcıdan ürün ID ve varyant ID alınır.
 b. Ürün bilgileri veritabanından çekilir (fiyat, stok, maksimum sipariş adedi, KDV oranı vb.).
 c. Eğer ürün stokta yoksa kullanıcıya “Stokta yok” uyarısı gösterilir.
 d. Ürün zaten sepette varsa, miktarı güncellenir (stok ve limit sınırına göre).
 e. Ürün sepette yoksa yeni bir kalem olarak eklenir.

4. Ürün silme işlemi:
 a. Kullanıcı tarafından seçilen ürün sepet listesinden kaldırılır.

5. Miktar güncelleme işlemi:
 a. Yeni miktar stoktan büyükse veya sipariş limiti aşıyorsa, uygun miktara düşürülür.
 b. Eğer yeni miktar 0 ise ürün sepetten tamamen kaldırılır.

6. Kupon uygulama işlemi:
 a. Kullanıcıdan kupon kodu alınır.
 b. Kuponun geçerliliği ve uygunluğu kontrol edilir.
 c. Kupon geçerli değilse hata mesajı gösterilir.
 d. Geçerli ise kupon bilgisi sepete eklenir.

7. Kargo seçimi:
 a. Kullanıcı teslimat adresini ve kargo yöntemini belirler.
 b. Bu bilgiler sepete kaydedilir.

8. Toplamların hesaplanması:
 a. Alt toplam = tüm ürünlerin (fiyat × miktar) toplamıdır.
 b. Kalem bazlı KDV toplamı hesaplanır.
 c. Kupon indirimi uygulanır (yüzde, tutar veya bedava kargo olabilir).
 d. Kargo ücreti hesaplanır (veya kuponla sıfırlanabilir).
 e. Genel toplam = alt toplam + KDV + kargo - indirim olarak belirlenir.

9. Stok kontrolü:
 a. Ödeme öncesinde tüm ürünler için güncel stok miktarları kontrol edilir.
 b. Stok yetersizse miktar düşürülür veya ürün sepetten kaldırılır.

10. Ödeme süreci:
 a. Stok geçici olarak rezerve edilir (başka kullanıcılar aynı ürünü alamaz).
 b. Kullanıcının seçtiği ödeme yöntemi üzerinden onay istenir.
 c. Eğer ödeme reddedilirse stok rezervasyonu iptal edilir.

11. Sipariş oluşturma:
 a. Ödeme onaylandıysa sipariş kaydı oluşturulur.
 b. Stok kalıcı olarak düşülür.
 c. Sepet temizlenir (boşaltılır).

12. Sipariş sonucu:
 a. Kullanıcıya sipariş numarası ve onay mesajı gösterilir.

13. Bitiş
