İş Paylaşımı
============

Giriş
-----

C++ ile OpenMP kullanımı aşağıdaki örnekte basit bir şekilde
gösterilmiştir.

.. code:: cpp

   int main(){

       // Tek iş parçacığı

   #pragma omp parallel 
   {
       // Birden fazla iş parçacığı
   }

       // Tek iş parçacığı
   }

Burada ``#pragma omp parallel`` bloğu dışındaki kısımlar tamamıyla
standard C++ kodundan ibarettir. Bu bloğun içinde ise OpenMP ile paralel
programlama gerçekleştirilebilir.

OMP For
-------

Paralel bloğun içerisinde birden fazla iş parçacığı bulunduğundan
eğer başka bir direktif verilmezse bu iş parçacıkları aynı kodu çalıştıracaktır. 
Kodun bazı kısımları için işin bu parçacıklara dağıtılması gerekir. 
``#pragma omp for`` direktifi kullanılarak standard C/C++ for döngüleri
paralel hale getirilebilir. Bu durumda döngünün içindeki kod birden
fazla iş parçacığı tarafından paralel olacak şekilde
çalıştırılacaktır.

Aşağıdaki örnekte bu direktif kullanılarak iki rastgele sayılardan
oluşan dizi (ing., array) paralel olarak çarpılmıştır.

.. code:: cpp

   #include <stdlib.h> // srand ve rand fonksiyonları
   #include <time.h> // time fonksiyonu

   #define N 1000


   int main(){

       // Diziler 
       int a[N],b[N],c[N];

       // Rastgele sayıların dizilere konması
       srand(time(NULL));
       for(int i=0; i<N,i++){
           a[i] = rand();
           b[i] = rand();
       }

   // Paralel blok
   #pragma omp parallel 
   {

   // Dizilerin paralel olarak çarpımı
   #pragma omp for
       for(int i=0; i<N, i++){
           c[i] = a[i]*b[i];
       }
   }
   }

Eğer parallel blok içerisinde başka bir işlem yapılmayacaksa (üstteki
örnekte olduğu gibi), iki direktif ``#pragma omp parallel for`` şeklinde
birliştirilebilir

Aşağıda aynı örnek bu kısayol kullanılarak verilmiştir.

.. code:: cpp

   #include <stdlib.h> // srand ve rand fonksiyonları
   #include <time.h> // time fonksiyonu

   #define N 1000


   int main(){

       // Diziler 
       int a[N],b[N],c[N];

       // Rastgele sayıların dizilere konması
       srand(time(NULL));
       for(int i=0; i<N,i++){
           a[i] = rand();
           b[i] = rand();
       }

   // Paralel blok
   #pragma omp parallel for
       for(int i=0; i<N, i++){
           c[i] = a[i]*b[i];
       }
   }

Bazı önemli detaylar: - Paralel bir çalışmada, seri çalışmada olduğu gibi yinelemelerin 
(ing., iteration) verilen sırayı takip etmesi beklenemez. Bir diğer değişle döngü
beklenenden farklı bir sırada çalıştırılabilir. - An itibariyle OpenMP
standardına göre sadece “canonical loop form” yani ``for(...;...;...)``
şeklindeki döngüler desteklenmektedir. C++11 ile birlikle gelen
``for(... : ...)`` şeklindeki döngüler bu direktif ile kullanılamaz. -
OpenMP 5 ile birlikte ``loop`` adında benzer bir direktif eklenmiştir.
An itibariyle TRUBA'da yüklü olan derleyeciler OpenMP 5’i desteklemediği
için bu direktif dokümana dahil edilmemiştir. - Yukarıda verilen
örneklerde iş parçacıkları veriyi (bu durumda *a*, *b*, *c* dizilerini)
paylaşmaktadır. Yani bütün parçacıklar aynı dizilere erişmekte ve
değiştirmektedir. Bu veri kapsamları bölümünde daha detaylı
açıklanacaktır. - Genelde döngünün yenileme sayısı iş parçacığı
sayısından fazla olacağından, bir iş dağıtımı yöntemi gereklidir.
Bu durumda varsayılan davranış derleyiciler arasında değişiklik
göstermektedir ve iş dağıtımı bölümünde daha detaylı açıklanacaktır.

OMP Sections
------------

``omp for`` direktifinde tüm iş parçacıkları ``for`` döngüsünün içinde
yer alan aynı kodu çalıştırmaktadır. Eğer bu iş parçacıklarının farklı
görevleri yerine getirmelerini istersek ``sections`` direktifini
kullanabiliriz.

Bu direktif için genel kullanım aşğıdaki örnekte gösterilmiştir.

.. code:: cpp

   int main()
   {
   #pragma omp parallel
   {    

   #pragma omp sections
   {
   #pragma omp section
   fonksiyon_1();
               
   #pragma omp section
   fonksiyon_2();
   }

   }
   return 0;
   }

``for`` direktifinde olduğu gibi ``parallel`` ve ``sections`` beraber
kullanılabilir (``#pragma omp parallel sections``). Bu tür bir kullanımda her bir ``section`` 
bir iş parçacığına atanır. 
