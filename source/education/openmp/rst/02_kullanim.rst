OpenMP Kullanımı
================

-  OpenMP hem C/C++ hem Fortran derleyicileri ile kullanılabilir. Bu
   dokümanda C++ ile kullanımını gözden geçireceğiz.
-  Bu bölümün amacı hâlihazırda var olan kodumuzu OMP direktifleri
   aracılığı ile en az eforu göstererek hızlandırmaktır.

C++ “pragma”
------------

-  C++’da ``pragma`` direktifi derleyiciye kodun kendisinin dışında
   ekstra bilgi vermek için kullanılan bir standarttır.
-  Biz bu direktifi derleyiciye OpenMP özelliklerini kullanmasını
   belirtmek için ``#pragma omp ...`` şeklinde kullanacağız.

C++ OpenMP kodu derlemek
------------------------

-  OpenMP standartı *gcc*, *clang*, *msvc* gibi popüler bir çok C/C++
   derleyicisi tarafından desteklenir.

   -  Derleyiceler arasında OpenMP açısından bazı farklar olması
      doğaldır.

-  Bu doküman TRUBA'xa yüklü olan ``gcc`` (C++ için ``g++`` olarak
   çağırılır) derleyicisini kullanmaktadır.

-  TRUBA'ya giriş yaptığımızda yüklü ``gcc`` versiyonunun 4.8 olduğunu
   görüyoruz. (Haziran 2021 itibariyle).

-  Bu versiyon bazı örnekler için yeterli olmakla birlikte yeni C++ ve OpenMP
   özelliklerini desteklememektedir. Dolayısıyla yeni bir versiyonun modül
   sistemiyle yüklenmesi önerilir.

-  ``module avail gcc`` komutu kullanılarak çevre modülü (ing.,
   environment module) sistemi kullanılarak yüklenebilecek ``gcc``
   versiyonları görüntülenebilir.

-  Daha sonra ``module load <gcc versiyonu>`` şekline bu modüller
   yüklenebilir.

-  Örneğin: ``module load centos7.3/comp/gcc/9.2`` kullanılarak
   ``gcc 9.2`` yüklenebilir.

-  C++ kodunu OpenMP özellikleri ile birlikte derlemek için derleme komutuna
   ``-fopenmp`` eklemek yeterlidir.

-  Örnek (C++14, OpenMP ve 3.seviye optimizasyon):

.. code:: bash

   g++ main.cpp -o main -fopenmp -O3 -std=c++14
